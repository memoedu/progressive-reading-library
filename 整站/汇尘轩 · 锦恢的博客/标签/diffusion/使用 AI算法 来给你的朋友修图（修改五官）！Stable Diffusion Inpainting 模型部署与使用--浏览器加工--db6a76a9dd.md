## 前言

最近正值中秋国庆连休，留守实验室的几个朋友一起出去玩了一会儿（而我回老家度假去了）。其中一位朋友在自拍时，并没有拍到满意的照片，他的眼睛是闭着的，于是找到我，希望我设计一个算法来修复闭眼的照片。

我当时觉得很有趣，这不就是一个五官修复的任务吗？我第一个想到的就是 [Repaint](https://huggingface.co/docs/diffusers/api/pipelines/repaint) 这个项目，但是效果一般，而且速度超级慢。后来尝试了使用Stable Diffusion做了一个Inpaint，发现效果不错，我们来简单看一下效果。

> 因为隐私问题，我下面都拿网上找到的图片做演示

比如一张需要修复五官的图如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c9e7566ac325697d5d20b2d3036c7fe3_1440w.jpeg" style="width: 50%;"/>
</div>

现在需要让将这张图的眼睛睁开，效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-33e2229ed6ced7d2be0a895318bd1175_1440w.png" style="width: 50%;"/>
<p class="img-caption">提词：eyes of a beautiful female, making it as normal as possible</p>
</div>

下面这篇文章就简单讲讲如何实现这样一个功能。

---

## 硬件要求

- 内存：> 16GB
- 显卡：支持cuda的显卡
- 显存：> 7.5GB

---

## 软件要求（python库的安装）

本项目需要用到如下的包和软件，使用pip一键安装：

```bash
pip install labelme opencv-python numpy matplotlib pillow torch diffusers
```

---

## 使用教程

使用起来非常简单，只需要三步就能完成：制作 mask、设置提词、运行模型。

### step.1 制作 mask

修复五官，首先需要让 AI模型 知道你要修复哪一个部分，这个逻辑需要通过 mask 来完成。

> 所谓 mask，就是像素值为0或者255的二值化图像。

#### 1.1 通过 labelme 绘制多边形
通过 `labelme` 的多边形绘制加上一小段程序就能完成 mask 的制作。还是以刚刚上面演示中提到的人脸为例：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c9e7566ac325697d5d20b2d3036c7fe3_1440w.jpeg" style="width: 50%;"/>
</div>

因为我们只需要让她睁开眼睛，所以我们只需要制作这张图中眼睛区域的 mask 即可。我们可以使用 labelme 完成这个任务，过程如下：

1. 打开命令行，输入 `labelme` 打开软件。
2. 将这张图片拖进去。
3. 使用多边形工具框选出两个眼睛所在的区域。
4. Ctrl + S 保存成 `woman-mask.json` 后退出。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-eb5e4c874ddadc42913abb1c5956c31f_1440w.png" style="width: 70%;"/>
<p class="img-caption">labelme 操作示意图</p>
</div>

#### 1.2 通过 opencv 转换得到mask

labelme 导出的只是我们画的框的点，不是mask，我们还需要自己编写程序转化，我已经写好了，名字叫 `make_mask.py`，复制如下代码：
```python
import json
import cv2
import matplotlib.pyplot as plt
import numpy as np

import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--label-json')
parser.add_argument('--input-image')
parser.add_argument('--output-mask', default='mask.png')
parser.add_argument('--show', action='store_true')
args = parser.parse_args()
 
with open(args.label_json, "r", encoding='utf-8') as fp:
    label_json = json.load(fp)
 
points = []
for ploy in label_json['shapes']:
    pts = np.array(ploy['points'], np.int32)
    points.append(pts)
 
img = cv2.imread(args.input_image)
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
mask = np.zeros_like(img) 
cv2.fillPoly(mask, points, (255, 255, 255))
img_add = cv2.addWeighted(mask, 0.3, img, 0.7, 0)
cv2.imwrite(args.output_mask, mask)
if args.show:
    plt.imshow(img_add)
    plt.axis('off')
    plt.show()
```
然后运行如下命令：

```bash
$ python make_mask.py --label-json woman-face.json --input-image woman.jpg --output-mask woman-mask.jpg
```

这样我们就得到了 mask（woman-mask.jpg）：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-44dbaa649eff8662fa9882bdc454b53a_1440w.jpeg" style="width: 50%;"/>
<p class="img-caption">mask 图像</p>
</div>

### step.2 设置提词

根据我们需要修复的部分，我们需要设置对应的提词交给模型运行，比如我们的这次任务是要让图像中人物的眼睛睁开，所以提词设置为：

```
eyes of a beautiful female, making it as normal as possible
```

如果你不会设置，那么不妨问问 文心一言 或者 chatgpt

### step.3 运行模型

通过如下的代码直接下载并运行 Stable Diffusion 模型，文件名为`inpaint.py`：

```python
import argparse

from PIL import Image
from diffusers import StableDiffusionInpaintPipeline
import torch

pipeline = StableDiffusionInpaintPipeline.from_pretrained(
    "runwayml/stable-diffusion-inpainting",
    torch_dtype=torch.float16,
    use_safetensors=True,
    variant="fp16",
)
pipeline = pipeline.to("cuda")

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--image')
    parser.add_argument('--mask')
    parser.add_argument('--prompt')
    parser.add_argument('--output', default='stable-diffusion-inpainting-output.png')
    args = parser.parse_args()

    origin_image = Image.open(args.image).convert('RGB').resize((512, 512))
    mask = Image.open(args.mask).convert('RGB').resize((512, 512))
    
    prompt = args.prompt.lower().strip()
    image = pipeline(prompt=prompt, image=origin_image, mask_image=mask).images[0]
    image.save(args.output)
```

输入命令：

```bash
$ python inpaint.py --image woman.jpg --mask woman-mask.jpg --prompt "eyes of a beautiful female, making it as normal as possible" --output sd-repaint.png
```

运行开始会去下载 stable diffusion inpaint模型。模型本体比较大，需要耐心等待，如果你遇到了huggingface上的网络问题，那么可以移步我的[这篇文章](https://zhuanlan.zhihu.com/p/646907543)，这里讲了遇到huggingface上下载模型的网络问题时，要如何解决。

运行结束后，会得到 sd-repaint.png 效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-33e2229ed6ced7d2be0a895318bd1175_1440w.png" style="width: 50%;"/>
</div>
