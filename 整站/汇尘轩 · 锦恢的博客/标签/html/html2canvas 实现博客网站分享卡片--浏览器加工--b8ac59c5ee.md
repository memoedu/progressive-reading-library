## 前言

我之前一直非常眼馋知乎的分享卡片功能呀，比如这样：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-c7529ed007004218f0d807343ebf537c_1440w.jpeg" style="width: 50%;"/>
</div>

这种方式对内容和平台的传播力度大于一般的链接，所以我想着，给我的网站也实现一个。

---

## 实现思路

为了实现将特定文字包装成卡片，再分享成为 png 图片，我第一个想到的是 `html2canvas` 这个库。右下方的二维码可以通过 `qrcodejs2` 来实现。先安装一下。

```bash
npm install qrcodejs2 html2canvas
```

假设你已经通过你的框架在 DOM 上实现了一个分享卡片的主体结构（文字渲染，布局设计），其实个人建议可以先在 figma 里面设计一下：（下图还未设计完成）

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-e3f4950fb0556a9265e2cb3f2d7ded0d_1440w.jpeg" style="width: 100%;"/>
</div>

假设你的主体部分设计完成了，下面就只需要生成当前页面的二维码和产生图片了。



### 1. 创建二维码

有 `qrcodejs2`，创建二维码非常简单：

html 中先创建一个元素：

```html
<div>
    <div id="share-qrcode"></div>
</div>
```

实现创建二维码的函数 `createQrCode`：

```javascript
import QRCode from 'qrcodejs2';

function createQrCode(link) {
    const qrCodeContainer = document.getElementById('share-qrcode');

    // 清空
    qrCodeContainer.replaceChildren();

    const qrcode = new QRCode("share-qrcode", {
        text: link,
        width: 70,
        height: 70,
        colorDark: "#fff",
        colorLight: "rgba(139, 73, 225, 1)",
        correctLevel: QRCode.CorrectLevel.H
    });    
}
```

这里有一个小坑，每次创建之前，一定要把目标容器清空，`qrcodejs2` 绑定元素ID后是追加元素的，而不是替换元素。如果不清空，多次调用 createQrCode 会产生好几个二维码。



### 2. html2canvas 将 html 转成 png

```javascript
async function createShareCard() {
    // 获取用户选择的文本
    const selection = window.getSelection();
    if (!selection.toString().trim()) {
        return;
    }

    // 创建一个容器来包裹选择的文本
    const selectedTextContainer = document.getElementById('share-image-container');
    selectedTextContainer.style.display = 'inline-block';
    selectedTextContainer.classList.add('share-card-image');

    const renderContainer = selectedTextContainer.querySelector('.share-render');
    renderContainer.textContent = selection.toString();
    document.body.appendChild(selectedTextContainer);
    
    const canvas = await html2canvas(selectedTextContainer, {
        backgroundColor: 'rgba(0, 0, 0, 0)',		// 背景透明
        scale: 3,
        dpi: window.devicePixelRatio * 3,
        logging: false,		// 不需要输出日志
        useCORS: true,		// 渲染跨域的图片
    });

    // 将 canvas 转换为图片 URL
    const imageUrl = canvas.toDataURL('image/png');

    // 通过 imageUrl 进行通信
    shareCard.imageUrl = imageUrl;
	
    // 隐藏真实的元素
    selectedTextContainer.style.display = 'none';
}
```

需要注意的是

- 上面的 `scale` 和 `dpi` 的那个乘3是必须的，否则渲染出的图像质量很差，也可以改成 4，实践下来在 4k 显示器上至少得是 3.
- 渲染得到的 `imageUrl`，当你把它装载到 DOM 上面时，一定要通过 css 设置 img 的宽度，否则上述的乘3仍然会以低分辨率渲染出屏幕。

```html
<template>
    <div>
        <div class="share-image-container">
            <img :src="shareCard.imageUrl">
        </div>
    </div>
</template>

<style>
.share-image-container img {
    width: 900px;
}
</style>
```

---

## 遇到的坑

在使用 html2canvas 时，遇到几个坑。此处记录一下。

---

### 坑1. CSS 子类无法正确解析

我们在写 CSS 时，为了更好的维护性，一般会把一段具有包含关系的 CSS 写成如下的形式：

```html
<template>
	<div class="card-container">
        <div class="title">这是一个简单的 Card</div>
    </div>
</template>

<style>
.card-container {
	font-family: "NerdFontMono";
}
</style>
```

上述代码把 `这是一个简单的` 这一段的文本的字体指定为 `NerdFontMono`，但是 html2canvas 似乎并不能 100% 让 CSS子类 继承父类的属性，导致渲染结果不一致。为了解决这个问题，尽量直接强制指定所有最底层节点的 CSS 属性：

```css
.card-container .title {
	font-family: "NerdFontMono";
}
```

---

### 坑2. 预渲染解决 html2canvas 无法实现复杂样式渲染的问题

需要说明一点，对于复杂的样式层叠，html2canvas 可能无法正确渲染，比如常见的高斯模糊遮罩层，也就是在一张图片上叠上一层白色毛玻璃特效的 div，这种 html2canvas 可能就无法实现。

解决这个问题最快的方法之一就是自己手动实现这个效果的 png，我们通过 python 来进行预渲染：

```python
from PIL import Image
import numpy as np
import cv2

def apply_custom_mask_with_blur(image_path, alpha, mask_color=(255, 255, 255), blur_radius=20, output_path='output.jpg'):
    """
    对输入图像应用自定义颜色的遮罩和模糊效果，并保存为 JPEG。

    :param image_path: 输入图像的路径
    :param alpha: 遮罩的透明度（0 到 1）
    :param mask_color: 遮罩的颜色，格式为 (R, G, B)，默认是白色 (255, 255, 255)
    :param blur_radius: 模糊半径，值越大，模糊效果越强
    :param output_path: 输出图像的保存路径
    """
    # 1. 加载图像
    image = Image.open(image_path).convert("RGBA")
    image_np = np.array(image)

    # 2. 创建自定义颜色的遮罩
    mask_color_rgb = np.array(mask_color, dtype=np.uint8)  # 转换为 numpy 数组
    mask = np.zeros_like(image_np)  # 创建与图像大小相同的遮罩
    mask[:, :, :3] = mask_color_rgb  # 设置 RGB 颜色
    mask[:, :, 3] = int(255 * alpha)  # 设置透明度

    # 3. 叠加遮罩
    blended = Image.alpha_composite(image, Image.fromarray(mask, 'RGBA'))

    # 4. 应用模糊效果
    blended_np = np.array(blended)
    blurred = cv2.GaussianBlur(blended_np, (0, 0), blur_radius)

    # 5. 转换为 RGB 模式并保存为 JPEG
    result = Image.fromarray(blurred, 'RGBA').convert("RGB")  # 转换为 RGB
    result.save(output_path, format="JPEG", quality=95)  # 保存为 JPEG
    result.show()

# 使用示例
apply_custom_mask_with_blur(
    image_path='background.jpeg',
    alpha=0.5,
    mask_color=(255, 255, 255),  # 白色遮罩
    blur_radius=10,
    output_path='output.jpg'
)
```

这样就能将左图转换成右图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a9936f116649f6aeff3302acb382f908_1440w.png" style="width: 100%;"/>
</div>



最终的渲染效果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-88b14c92b253b71b75a44aff753cdd24_1440w.jpeg" style="width: 100%;"/>
</div>

或者

<div align=center>
<img src="https://picx.zhimg.com/80/v2-78808b6723afabbf3a3fde52886c8a09_1440w.jpeg" style="width: 100%;"/>
</div>