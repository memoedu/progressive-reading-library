## 前言

最近心血来潮，看到了neovim的欢迎界面，打算自己做一个类似的效果。

简单写了几行代码，生成字符画，并结合了 [ASCII 艺术字的生成网站](https://tools.kalvinbg.cn/txt/ascii)。代码如下，其中 `image.100.png` 是一个 100 × 100 的二值化图像，你可以通过截屏+后期处理的方式获得这样一张图片。


## 图像像素化算法

```python
from PIL import Image
import numpy as np
import cv2

image = Image.open('./image.100.png').convert('L')
image = np.array(image)
image = cv2.resize(image, (30, 30))

_, image = cv2.threshold(image, 100, 255, cv2.THRESH_BINARY_INV)
image = 255 - image
ascii_art = []

def mat2col(mat: np.ndarray, ksize: int, stride:int=1) -> np.ndarray:
    """
    简单说明一下 向量化版本的 mat2col 的内部变量标记
        alpha_0 = (beta_0, beta_1, ..., beta_{out_h * out_w}) ^ T
        alpha = (alpha_0, alpha_1, ..., alpha_{k - 1})
    """
    assert len(mat.shape) == 2, 'mat2col only process 2d matrix!'
    h, w = mat.shape
    assert ksize <= min(h, w), 'ksize must be smaller than both height and width of input image!'

    out_h = (h - ksize) // stride + 1
    out_w = (w - ksize) // stride + 1

    beta_0 = np.arange(ksize)
    beta_i = np.arange(out_h * out_w)

    alpha_0_0 = (beta_i // out_w) * w + (beta_i % out_w) * stride
    alpha_0 = beta_0 + alpha_0_0[..., None].repeat(ksize, axis=1)

    alpha_i = np.arange(ksize) * w
    alpha_0 = np.tile(alpha_0, ksize)
    alpha_i = np.repeat(alpha_i, ksize)
    alpha = alpha_0 + alpha_i
    col = np.take(mat.flatten(), alpha)
    return col


blocks = ['█', '▄', '▀']
image = np.pad(image, pad_width=1)
cols = mat2col(image, ksize=2, stride=1)
pixes = [None] * len(cols)
pixes_size = int(np.sqrt(len(cols)))
mapping = {
    '[255 255 255 255]': '██',
    '[255 255 255   0]': '█▀',
    '[0 0 0 0]': '  ',
    '[255 255   0 255]': '▀█',
    '[  0   0   0 255]': ' ▄',
    '[255   0   0   0]': '▀ ',
    '[255   0 255 255]': '█▄',
    '[  0   0 255 255]': '▄▄',
    '[255   0 255   0]': '█ ',
    '[  0 255   0 255]': ' █',
    '[255 255   0   0]': '▀▀',
    '[  0 255 255 255]': '▄█',
    '[  0   0 255   0]': '▄ ',
    '[  0 255   0   0]': ' ▀',
    '[255   0   0 255]': '▀▄',
    '[  0 255 255   0]': '▄▀'
}

fp = open('kid.txt', 'w', encoding='utf-8')
for i, region in enumerate(cols):
    value = mapping[str(region)]
    pixes[i] = value
    fp.write(value)
    print(value, end='')
    if (i + 1) % pixes_size == 0:
        print()
        fp.write('\n')

fp.close()
```

---

## 完整的代码（老版本）

新版本需要将上面的代码嵌入其中，受限于时间，不搞了。

下方代码中的一些参数完全按照自己的实际情况进行修改：

```python
from PIL import Image
import numpy as np
import cv2

image = Image.open('./image.100.png').convert('L')
image = np.array(image)
image = cv2.resize(image, (40, 40))

_, image = cv2.threshold(image, 100, 255, cv2.THRESH_BINARY_INV)

ascii_art = []

for line in image:
    arts = ''
    for value in line:
        if value > 100:
            arts += ' ■'
        else:
            arts += '  '
    ascii_art.append(arts.strip())

captions = r""" _                    _ _                             
| |    ___   __ _  __| (_)_ __   __ _                 
| |   / _ \ / _` |/ _` | | '_ \ / _` |                
| |__| (_) | (_| | (_| | | | | | (_| |  _ _ _   _ _ _ 
|_____\___/ \__,_|\__,_|_|_| |_|\__, | (_|_|_) (_|_|_)
                                |___/                 


                                




 ____        _   _                                 ____            
|  _ \ _   _| |_| |__   ___  _ __     ___  _ __   / ___| _     _   
| |_) | | | | __| '_ \ / _ \| '_ \   / _ \| '__| | |   _| |_ _| |_ 
|  __/| |_| | |_| | | | (_) | | | | | (_) | |    | |__|_   _|_   _|
|_|    \__, |\__|_| |_|\___/|_| |_|  \___/|_|     \____||_|   |_|  
       |___/     







 _         _   _       ____                                   _   _     _             
| |    ___| |_( )___  |  _ \  ___    ___  ___  _ __ ___   ___| |_| |__ (_)_ __   __ _ 
| |   / _ \ __|// __| | | | |/ _ \  / __|/ _ \| '_ ` _ \ / _ \ __| '_ \| | '_ \ / _` |
| |__|  __/ |_  \__ \ | |_| | (_) | \__ \ (_) | | | | | |  __/ |_| | | | | | | | (_| |
|_____\___|\__| |___/ |____/ \___/  |___/\___/|_| |_| |_|\___|\__|_| |_|_|_| |_|\__, |
                                                                                |___/ 
 _       _                     _   _               _ 
(_)_ __ | |_ ___ _ __ ___  ___| |_(_)_ __   __ _  | |
| | '_ \| __/ _ \ '__/ _ \/ __| __| | '_ \ / _` | | |
| | | | | ||  __/ | |  __/\__ \ |_| | | | | (_| | |_|
|_|_| |_|\__\___|_|  \___||___/\__|_|_| |_|\__, | (_)
                                           |___/                                                  
               """

captions = captions.split('\n')

fp = open('kid.txt', 'w', encoding='utf-8')
for i, line in enumerate(ascii_art):

    print(' ' * 5, line, end='')
    fp.write('\033[37m' + ' ' * 5 + line)
    if i < len(captions):
        color_flag = '\033[32m'
        if i > 9:
            color_flag = '\033[34m'
        if i > 22:
            color_flag = '\033[33m'
        print(' ' * 10, color_flag, captions[i], '\033[0m')

        content = ' ' * 10 + color_flag + captions[i] + r'' + '\n'
        fp.write(content)
    else:
        print()
        fp.write('\n')
fp.close()
```
输出效果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-80e2b0e6336b6de211e54d119488bc8e_1440w.png" style="width: 80%;"/>
</div>