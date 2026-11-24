## 背景 & 效果

传统分割算法考古来到了最后一章，这次要讲的是一个非常有趣有用的免训练、可交互的图像分割算法——Graph Cut 图分割。原论文为： Interactive Graph Cuts for Optimal Boundary & Region Segmentation of Objects in N-D Images [1]

我将这个玩具写成了一个200行代码内就能在网页中打开的应用（代码在文章最后的附录上），用户可以不断在原图上画一下一定是前景的区域和一定是背景的区域（下图中左图中的绿色区域和红色区域），算法便能不断迭代生成最终的分割图（下图右图）。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-f6fcf0f12bdda7753f03c6d23719f25a_1440w.png" style="width: 70%;"/>
<p class="img-caption">绿色线条代表前景，红色线条代表背景</p>
</div>

这个方法最大的一个好处就是可以像 SAM 那样和图像不断进行交互和修正来完成图像分割。当然，如果阁下不会写前端应用的话（而不是图像算法），那么不妨移步我之前的文章：

---

## 前置知识

- opencv
- 计算机图论：网络流算法
- streamlit

> 以上知识不会在文中被 cover 到，请自行学习

---

## 原理讲解

### 问题建模——图模型

在之前的算法中，我们通常将图像看成是一堆像素点聚合成一个矩阵。除了将图像看成矩阵，我们还可以将图像看成是一个图，这件事在大部分的图像处理课上应该都有所提及。在 Graph Cut 中，原本的图像被建模成了一个图结构，为了更方便展示算法流程，我们假设我们需要建模的图像是一个 3 × 3 的矩阵，相邻像素点两两相连构成无向图 $G$，我们记像素点的集合为 $\mathcal P$。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1ce52e313dec07a79ea4f30b321665c0_1440w.png" style="width: 60%;"/>
</div>

为了表示哪些节点属于前景，哪些背景， Graph Cut 中引入了 T 节点和 S 节点，它们默认和所有像素点节点连接（为了绘图方便，我只绘制了一部分）。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d3dea4a8fb5e323b2a0db498634a9d62_1440w.png" style="width: 60%;"/>
</div>

显然，这种连接代表的含义与像素点节点之间的连接所代表的含义并不一样。我们在此做出两种规定：

1. 像素点之间的连接叫做 `n-link`
2. 像素点与 T 或 S 的连接叫做 `t-link`

由此，我们便建模完了一个描述这个 3 × 3 图像的模型 $G = <V, E>$，其中 $V$ 代表点集，在该方法中便是所有像素点加上两个终端节点 T 和 S； $E$ 代表边集，是所有的 `n-link` + `t-link` 构成的集合。

既然是要做分割，自然需要定义一下分割本身在这个图上的定义。在 $G$ 中，一个分割被定义为 $E$ 的子集 $E'$ ，该集合中的边需要全部断开，如下图的橙色的线条：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-cd2337edae02b12f59a144bb1a4c1724_1440w.png" style="width: 60%;"/>
</div>



上图中， $E'$ 定义的分割切分了所有的 `n-link`，并且属于前景的节点与 T 断开，属于背景的节点与 S 断开。这样的一次分割结束后，模型就将与 T 相连的节点预测为前景的节点，与 S 相连的节点就预测为背景的节点。

那么，如何衡量这样的一个分割好不好呢？

### 衡量分割质量的方法——惩罚函数 / 损失函数

在上文的讨论中，一个分割 $E'$ 可以直接输出一个图像的前景区域和背景区域。即 $E'$ 可以推导出一个像素点的 label 集合：
$$
A = \{A_1, ..., A_{\mathrm {card} \vert\mathcal P\vert}\}
$$
其中：
$$
A_i = \begin{cases}
0 & A_i \text{ link to } T\\
1 & A_i \text{ link to } S
\end{cases}
$$
在 Graph Cut 中，惩罚项 $E(A)$ 用于描述分割结束后的集合 $A$ 所代表的分割质量如何：
$$
E(A) = \lambda R(A) + B(A) \quad \lambda > 0
$$
其中：

- $R(A)$ 是区域项 (region properties)，衡量了 `t-link` 被切断的代价/损失。

- $B(A)$ 是边界项 (boundary properties)，衡量了 `n-link` 被切断的代价/损失。

我们一项一项来讲。



#### 区域项 region properties

区域项 $R(A)$ 由所有像素点的某种度量构成：
$$
R(A) = \sum_{p\in \mathcal P} R_p(A_p)
$$
其中 $R_p(A_p)$ 代表像素点 $p$ 被判断为 $A_p$ 类别的损失（$A_p$ 为 0 或者 1），这一项往往可以通过计算 $p$ 像素值的后验概率的负对数得到。更加形式化的表达为：
$$
R_p(A_p = 1) = -\ln \mathbb P(I_p | \mathcal F)\\
R_p(A_p = 0) = -\ln \mathbb P(I_p | \mathcal B)
$$
其中 $I_p, \mathcal F, \mathcal B$ 分别代表 像素点 $p$ 处的像素值，用户标注/系统默认的前景区域，用户标注/系统默认的背景区域。

不要忘了， Graph Cut 是有用户输入的信息的，我们会在一开始就利用这些输入信息，即哪些是前景区域 $\mathcal F$，哪些是背景区域 $\mathcal B$ 来构建对应的概率模型 $\mathbb P(\cdot | \mathcal F)$ 和 $\mathbb P(\cdot | \mathcal B)$（最简单的就是灰度直方图），基于这些概率模型，我们就能计算 $p$ 的后验概率啦。从而计算出 $R_p(A_p)$ 的值了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d74766de6047142419dc1db987cc1bdf_1440w.jpeg" style="width: 40%;"/>
</div>



#### 边界项 boundary properties

如果说区域项是以后验概率的方式，从全局信息的角度给出了分割的一种代价度量，那么边界项就是从空间信息出发，给出了局部像素点上的一种代价度量。

我们记，所有像素点之间的 `n-link` 为集合 $\mathcal N$，则 $B(A)$ 被定义为：
$$
B(A) = \sum_{(p, q) \in \mathcal N} B(p, q) \delta(A_p, A_q)
$$


其中 $B(p, q)$ 是关于像素点 $p,q$ 的函数，它衡量了像素点 $p, q$ 在空间域和色彩域上的差异。 $p, q$ 在空间域和色彩域上面差距越大，说明这两个点在局部看来越有可能不是同一个类别，代表这个分割是正确的，那么 $B(p, q)$ 就应该越小。一个很容易想到的建模 $B(p, q)$ 的函数就是双边滤波算子，当然，在原文中，用了一种更加简单的形式来建模 $B(p, q)$：
$$
B(p, q) \propto \exp \left(- \frac{(I_p - I_q)^2}{2\sigma^2}\right) \frac{1}{\mathrm{dist}(p, q)}
$$
其中 $\mathrm{dist}(p, q)$ 代表了这两个像素点在空间上距离。默认使用欧式距离。

$\delta(A_p, A_q)$ 就是一个简单的指示函数，如下的式子描述了其关系：
$$
\delta(A_p, A_q) = \begin{cases}
1 & \text{if } A_p \not= A_q\\
0 & \text{otherwise}
\end{cases}
$$
这个式子保证了 $B(A)$ 只度量分割处的像素点差异，所以其实 $B(A)$ 的一种等价表示如下：
$$
B(A) = \sum_{(p, q) \in E'} B(p, q)
$$
阁下可以根据自己的理解选择喜欢的表达方式。

总之，我们肯定希望我们找到的分割 $E'$ 能够让代价函数 $E(A)$ 最小，那么如何优化这个函数呢？



### 最大流最小割

通过上面的一套建模，我们其实本质上给 $G$ 的每一条边都附上了一个权重：

- 对于 `n-link`，它的权重为 $B(p, q) \delta(A_p, A_q)$。
- 对于 `t-link`，它的权重为 $R_p(A_p)$

如此建模，我们就获取到了一个已知信息 100% 的图，优化问题也就直接变成了找到一个分割，使得分割图的边权值之和最小，这个问题被我们称为 “最小割”。既然是一个完全透明的图结构，很自然地可以想到，我们能否利用计算机图论算法来找到这个图的最小割呢？

根据[最大流最小割原理](https://zh.wikipedia.org/wiki/%E6%9C%80%E5%A4%A7%E6%B5%81%E6%9C%80%E5%B0%8F%E5%89%B2%E5%AE%9A%E7%90%86)，找到图的最小割就等价于找到它的最大流，关于最大流问题的定义和网络流算法，阁下可以参考：[【图像分割】“最大流-最小割”算法](https://shichaoxin.com/2018/10/26/%E5%9B%BE%E5%83%8F%E5%88%86%E5%89%B2-%E6%9C%80%E5%A4%A7%E6%B5%81-%E6%9C%80%E5%B0%8F%E5%89%B2-%E7%AE%97%E6%B3%95/)，此处关于如何求解网络流就不再赘述了。因此，整个 Graph Cut ，我们就从最小化分割代价函数走到了找到图的最大流，只要找到了图的最大流，我们就可以声称完成了图像的分割。

想要编程实现 Graph Cut 也很简单，不过 opencv 已经内嵌了这个经典算法，因此，我们也不再需要手搓算法了，转而做一些更加好玩的东西，比如一个可交互的前端应用。

---

## 代码实现

先安装一下这次需要使用的库：

```bash
pip install numpy pillow opencv-python streamlit streamlit-drawable-canvas
```



opencv 本身就提供了 Graph Cut 的实现函数 `grabCut` ，文档在这里：https://docs.opencv.org/3.4/d8/d83/tutorial_py_grabcut.html 。加上 Graph cut 并没有训练阶段，直接就能进行抠图，之前用了两期的息肉检测图像，大伙估计也看腻了，这次换点图片来测试。

`grabCut` 提供了两种分割模式，基于方框的和基于mask的，在代码中分别为 `cv2.GC_INIT_WITH_RECT` 和 `cv2.GC_INIT_WITH_MASK`。

### 基于 bbox 先验的图分割 `cv2.GC_INIT_WITH_RECT`

我们先看看前者。基于方框的 图分割 需要用户提供包含前景图像的一个方框，也就是 bounding box，这部分 bbox 内部的像素值可以认为是前景先验，bbox外部的像素点会被当成背景先验。bounding box 可以通过开发软件让用户绘制，也可以通过程序设置参数来调出这个框框，放宽心，我简单写了一个前端程序，可以让你手动标注bbox 和 后续的 mask：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-2569a38909b7ca4665d36581f0bd88a3_1440w.png" style="width: 70%;"/>
</div>

不过为了演示内部如何使用，我还是手动制作了一个 `rect` 。下面的程序中的 `rect` 就是一个恰好包住了前景图像（unicorn phantom的自画像，为什么这个世界上有人作曲和绘画的技艺都这么强 www）

```python
import cv2
import numpy as np
from PIL import Image

img = cv2.imread('./cafe.png')
rect = (160, 30, 720, 890)
draw_image = img.copy()
cv2.rectangle(draw_image, (rect[0], rect[1]), (rect[2], rect[3]), color=(20, 200, 20), thickness=3)
Image.fromarray(draw_image[... ,::-1])
```

out：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-10c687193ff26c66669dc954f799c9e9_1440w.png" style="width: 40%;"/>
</div>

然后就可以使用 `grabCut` 进行分割了：

```python
mask = np.zeros(img.shape[:2], np.uint8)

# inplace 变量，供函数内部使用，别问我为什么要这么设计，鬼知道
bgdModel = np.zeros((1, 65), np.float64)
fgdModel = np.zeros((1, 65), np.float64)

cv2.grabCut(img, mask, rect, bgdModel, fgdModel, 5, cv2.GC_INIT_WITH_MASK)
mask = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
img = img * mask[:, :, np.newaxis]
Image.fromarray(img[..., ::-1])
```

输出：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a56c2a1c3ce824244db640a980ec7fde_1440w.png" style="width: 40%;"/>
</div>



可以看到，左侧的分割效果非常好，但是右侧仍然存在一部分背景没有被分割出去，原因也很简单，这部分像素值在 bbox 内部像素值构成的概率密度函数中的值很高，也就是这部分像素点属于前景的置信度很高。毕竟 bbox 制定的前景区域仍然有点粗放，为了更加精准地进行分割，就需要使用基于 mask 先验的分割方法了。也就是让用户输入一个粗糙的 mask。



### 基于 mask 先验的图分割 `cv2.GC_INIT_WITH_MASK`

关于如何使用 mask 先验进行图像分割，opencv 给出的官网文档上有点小错误，不过我们不必纠结这些小细节了，因为我已经在我的代码中纠正了这个错误：

```python
    mask = np.ones(origin_image.shape[:2], np.uint8) * cv2.GC_PR_BGD
    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    mask[foreground_mask > 0] = cv2.GC_FGD
    mask[background_mask > 0] = cv2.GC_BGD

    cv2.grabCut(origin_image, mask, None, bgdModel, fgdModel,
                graph_cut_iterations, cv2.GC_INIT_WITH_MASK)
```

其中 foreground_mask 和 background_mask 分别是前景和背景的 mask 图像，你可以通过软件生成，但是我已经写好了一个前端软件（代码在文章最后），你可以直接使用。点击左侧的“标记模式”，选择 “mask 先验”，然后“标记区域” 选择“前景”，然后画出一定是前景的区域，这一部分会用于训练 $\mathbb P(\cdot | \mathcal F)$，然后点击图像左下角的下载一样的图标将画的mask反馈到后端并进行分割：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-0810915bb0e37bb902d45cb894534a2e_1440w.png" style="width: 40%;"/>
</div>

效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-fcd85b79c08899b3fe1f09319e3b5995_1440w.png" style="width: 60%;"/>
</div>

可以看到，效果并不好，有一些前景区域被忽略了，有一些背景区域被加入了。为了纠正第一个问题，我们选择 “标记区域” -> “前景”，把 unicorn phantom 的风衣下摆画上，为了纠正第二个问题，我们选择 “标记区域” -> “背景”，把左侧的窗户标记为背景，让模型建模 $\mathbb P(\cdot | \mathcal B)$，然后点击send to streamlit，效果如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f65069136595e4b3f22411bdb73b16a2_1440w.png" style="width: 60%;"/>
</div>

可以看到，效果非常好。

如此，我们便优雅地实现了 Graph Cut 算法。

---

## 附录：前端代码

一共有两个文件，`app.py`，`graph_cut.py` ，直接运行 `streamlit run app.py` 即可运行。

`app.py`：

```python
from streamlit_drawable_canvas import st_canvas
from PIL import Image
import streamlit as st
import numpy as np

from graph_cut import predict_from_bbox, predict_from_mask

st.set_page_config(
    page_title='交互式图像分割程序',
    page_icon='🐳',
    layout='wide'
)

st.sidebar.title("Graph Cut 交互式前景分割程序")

drawing_region, display_region = st.columns(spec=2)

# 感谢 andfanilo 贡献的绘画组件 streamlit_drawable_canvas
# 论坛链接： https://discuss.streamlit.io/t/drawable-canvas/3671
tagging_mode = st.sidebar.selectbox(
    "标记模式:",
    ("bbox 先验", "mask 先验"),
)

drawing_mode_mapping = {
    'bbox 先验': 'rect',
    'mask 先验': 'freedraw'
}

drawing_mode = drawing_mode_mapping[tagging_mode]

stroke_width = st.sidebar.slider("画笔宽度: ", 1, 100, 15)
if drawing_mode == "point":
    point_display_radius = st.sidebar.slider("点半径: ", 1, 25, 3)

graph_cut_iterations = st.sidebar.slider('网络流迭代次数: ', 2, 30, 5)
if drawing_mode == 'rect':
    stroke_color = '#4dc114'
else:
    stroke_color = st.sidebar.selectbox(
        "标记区域",
        ('前景', '背景')
    )
    stroke_color_mapping = {
        '前景': '#4dc114',
        '背景': '#ff4b4b'
    }
    stroke_color = stroke_color_mapping[stroke_color]

bg_image = st.sidebar.file_uploader("上传需要标记的图像: ", type=["png", "jpg"])
realtime_update = st.sidebar.checkbox("实时更新", False)

if bg_image:
    img = Image.open(bg_image)
    img_array = np.array(img)
    canvas_height, canvas_width = img_array.shape[:2]
    with drawing_region:
        canvas_result = st_canvas(
            fill_color="rgba(255, 165, 0, 0.3)",
            stroke_width=stroke_width,
            stroke_color=stroke_color,
            background_color='#eee',
            background_image=Image.open(bg_image) if bg_image else None,
            update_streamlit=realtime_update,
            height=canvas_height,
            width=canvas_width,
            drawing_mode=drawing_mode,
            point_display_radius=point_display_radius
            if drawing_mode == "point" else 0,
            display_toolbar=True,
            key="full_app",
        )
    if canvas_result.image_data is not None and canvas_result.image_data.sum() > 0:
        if drawing_mode == 'rect':
            masked_image, mask = predict_from_bbox(img_array, canvas_result.image_data, graph_cut_iterations)
            display_region.image(masked_image)
            
        if drawing_mode == 'freedraw':
            masked_image, mask = predict_from_mask(img_array, canvas_result.image_data, graph_cut_iterations)
            display_region.image(masked_image)
```



`graph_cut.py`

```python
from typing import Sequence, Callable, TypeAlias

import cv2
import numpy as np
import streamlit as st

def find_first_index(sequence: Sequence[float],
                     condition: Callable[[float], bool],
                     reverse: bool = False) -> float:
    index = 0
    reverse_flag = 1
    if reverse:
        reverse_flag = -1
    for value in sequence[::reverse_flag]:
        if condition(value):
            break
        else:
            index += 1
    if reverse:
        index = len(sequence) - 1 - index
    return index


@st.cache_data
def predict_from_bbox(origin_image: np.ndarray, image_data: np.ndarray,
                      graph_cut_iterations: int) -> np.ndarray:

    if len(image_data.shape) == 3:
        image_data = image_data.min(axis=2)

    if len(origin_image.shape) == 3 and origin_image.shape[-1] > 3:
        origin_image = origin_image[..., :3]

    bbox = [0, 0, 0, 0]
    bbox[0] = find_first_index(image_data.sum(axis=0), lambda value: value > 0)
    bbox[1] = find_first_index(image_data.sum(axis=1), lambda value: value > 0)
    bbox[2] = find_first_index(image_data.sum(axis=0),
                               lambda value: value > 0,
                               reverse=True)
    bbox[3] = find_first_index(image_data.sum(axis=1),
                               lambda value: value > 0,
                               reverse=True)

    mask = np.zeros(origin_image.shape[:2], np.uint8)

    # inplace 变量，供函数内部使用，别问我为什么要这么设计，鬼知道
    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    cv2.grabCut(origin_image, mask, bbox, bgdModel, fgdModel,
                graph_cut_iterations, cv2.GC_INIT_WITH_RECT)
    mask = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')

    mor_kernel = np.ones((5, 5))
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel=mor_kernel)
    masked_image = origin_image * mask[..., np.newaxis]
    return masked_image, mask


@st.cache_data
def predict_from_mask(origin_image: np.ndarray, image_data: np.ndarray,
                      graph_cut_iterations: int) -> np.ndarray:

    background_mask = image_data[..., 0]
    foreground_mask = image_data[..., 1]
    _, background_mask = cv2.threshold(background_mask, 100, 255, cv2.THRESH_BINARY)
    _, foreground_mask = cv2.threshold(foreground_mask, 100, 255, cv2.THRESH_BINARY)    

    if len(origin_image.shape) == 3 and origin_image.shape[-1] > 3:
        origin_image = origin_image[..., :3]

    mask = np.ones(origin_image.shape[:2], np.uint8) * cv2.GC_PR_BGD
    bgdModel = np.zeros((1, 65), np.float64)
    fgdModel = np.zeros((1, 65), np.float64)

    mask[foreground_mask > 0] = cv2.GC_FGD
    mask[background_mask > 0] = cv2.GC_BGD

    cv2.grabCut(origin_image, mask, None, bgdModel, fgdModel,
                graph_cut_iterations, cv2.GC_INIT_WITH_MASK)
    mask = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
    
    mor_kernel = np.ones((5, 5))
    mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel=mor_kernel)
    masked_image = origin_image * mask[..., np.newaxis]
    return masked_image, mask
```

---

## 参考文献

- [1] Boykov Y Y, Jolly M P. Interactive graph cuts for optimal boundary & region segmentation of objects in ND images[C]//Proceedings eighth IEEE international conference on computer vision. ICCV 2001. IEEE, 2001, 1: 105-112.
- [2] https://zh.wikipedia.org/wiki/%E6%9C%80%E5%A4%A7%E6%B5%81%E6%9C%80%E5%B0%8F%E5%89%B2%E5%AE%9A%E7%90%86
- [3] https://shichaoxin.com/2018/10/26/%E5%9B%BE%E5%83%8F%E5%88%86%E5%89%B2-%E6%9C%80%E5%A4%A7%E6%B5%81-%E6%9C%80%E5%B0%8F%E5%89%B2-%E7%AE%97%E6%B3%95/