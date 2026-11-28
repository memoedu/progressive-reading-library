简单记录一下曾经的笔记。

## 光流简介

### 光流定义

首先我们看一下光流的定义：

<div align=center>
<img src="https://picx.zhimg.com/v2-68416746c730f7b8794c9e9be96ebcac_r.jpg?source=c8b7c179" style="width: 70%;"/>
<p class="img-caption">用于视频流</p>
</div>

在上图中，各个帧的光流可以用下图中橘黄色的箭头表示（笑脸代表我在上一帧所处的位置）

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f379a9616d0d7f6b5ffc50c954c189b3_1440w.webp?source=c8b7c179" style="width: 70%;"/>
<p class="img-caption">箭头就是光流</p>
</div>

### 背景

光流问题涉及尝试找出一幅图像中的许多点在第二幅图像中移动的位置--通常是以视频序列完成的，因此可以假定第一幅图像中的大部分点框架都可以在第二幅图像中找到。光流可以用于场景中物体的运动估计，设置用于相机相对于整个场景的自运动估计。光流算法的理想输出是两帧图像中每个像素的速度的估计关联，或者等效的，一幅图像中每个像素的位移矢量，指示该像素在另一幅图像中的相对位置，如果图像中的每个像素都采用这种方法通常称为“稠密光流”。还有一种算法称为“稀疏光流”，仅仅只跟踪图像中某些点的子集，该算法通常是快速且可靠的，因为其将注意力只放在容易跟踪的特定点上，稀疏跟踪的计算成本远远低于稠密跟踪，这也导致了后者的研究基本被限制在了学术圈。



---

## Lucas Kanade稀疏光流算法

### 简要介绍

Bruce D. Lucas 和 Takeo Kanade在1981年提出了Lucas Kanade（LK）算法试图计算稠密光流。然而该方法容易应用到输入图像中的点的子集，所以反而成为了稀疏光流算法的重要技术，该算法可以应用到稀疏场景中，因为它仅依赖于围绕某个兴趣点的一些小窗口导出的局部信息，但该算法的缺点在于，如果像素点的运动幅度过大，运动到了局部窗口之外，那么算法将无法找到。这个问题就导致了“金字塔-LK算法的发展”，该算法从图像金字塔的最低细节开始跟踪，并逐渐跟踪至更精细的细节。跟踪图像金字塔允许大幅度的运动被局部窗口捕捉。

### 工作原理

LK算法是一种两帧差分的光流估计算法，其基本思想基于以下三个假设。

1. 亮度恒定：场景中目标图像的像素看起来在帧到帧移动是不发生改变。对于灰度图像（对于彩色图像同适用）这意味着像素的灰度值不会随着帧的跟踪改变。
2. 时间持续性（微小移动）：图像上相机的移动随时间变化缓慢。实际上，这意味着时间的变化不会引起像素位置的剧烈变化，这样像素的灰度值才能对位置求对应的偏导数。
3. 空间一致性：场景中相同表面的相邻点具有相似的运动，并且其投影到图像平面上的距离也比较近。

### 公式推导

我们用$I(x,y,t)$代表视频流$t$时刻位于3位置$(x,y)$的像素值大小。

基于假设1（亮度恒定）和假设2（时间持续性），我们有：

$$
I(x, y, t) = I(x + \delta x, y + \delta y, t + \delta t)
$$

使用Taloy公式展开可得：

$$
I(x, y, t) = I(x, y, t) + \frac{\partial I}{\partial x}\delta x + \frac{\partial I}{\partial y}\delta y + \frac{\partial I}{\partial t}\delta t + R(x, y, t)
$$

其中$R(x, y, t)$是高阶项，近似为0，所以我们可得：

$$
\frac{\partial I}{\partial x}\delta x + \frac{\partial I}{\partial y}\delta y + \frac{\partial I}{\partial t}\delta t = 0
$$

两边除以$\delta t$可得：
$$
\frac{\partial I}{\partial x}\frac{\delta x}{\delta t} + \frac{\partial I}{\partial y}\frac{\delta y}{\delta t} + \frac{\partial I}{\partial t} = 0
$$

我们标记并对上面的项做出解释：

- $I_x:=\frac{\partial I}{\partial x}$：当前图像的水平梯度
- $I_y:=\frac{\partial I}{\partial y}$：当前图像的垂直梯度
- $I_t:=\frac{\partial I}{\partial t}$：当前帧减去上一张图像帧


我们得到了最终的表达式：$I_x u + I_y v = -I_t$

为了解上面的方程，我们需要观测一些已有的数据。假设观测到的数据为$\{(x_i, y_i, t_i)\}_{i=1}^n$，我们能得到：

$$
\begin{pmatrix}
I_{x_1}&I_{y_1}\\
I_{x_2}&I_{y_2}\\
\vdots&\vdots\\
I_{x_n}&I_{y_n}
\end{pmatrix}
\begin{pmatrix}
u\\
v
\end{pmatrix}
=\begin{pmatrix}
I_{t_1}\\
I_{t_2}\\
\vdots\\
I_{t_n}
\end{pmatrix}
$$

我们记作：$AV=-b$，这是一个正规方程，它的解为：

$$
V= (A^TA)^{-1}A^T(-b)
$$

> 好奇的朋友就问了，什么时候上面的方程无解呢？很明显是$A^TA$不可逆时，下面就是反例：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-cdda95edbec611b1f8d4c1ce397be539_1440w.png" style="width: 70%;"/>
<p class="img-caption">光流法不可解的情况</p>
</div>


### 代码

基于opencv+python的实现如下：
```python
import cv2 as cv
import numpy as np
feature_params = dict(
    maxCorners=100,
    qualityLevel=0.3,
    minDistance=7
)
lk_params = dict(
    winSize=(15, 15),
    maxLevel=2
)
colors = np.random.randint(0, 255, (100, 3))
cap = cv.VideoCapture(0)
ret, pre_frame = cap.read()
pre_gray = cv.cvtColor(pre_frame, cv.COLOR_BGR2GRAY)
pre_points = cv.goodFeaturesToTrack(pre_gray, mask=None, **feature_params)
def resize(img, height):
    h, w = img.shape[0], img.shape[1]
    width = int(height / h * w)
    img = cv.resize(img, (width, height))
    return img
mask = np.zeros_like(pre_frame)
while True:
    ret, frame = cap.read()
    if ret is False:
        break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    points, st, err = cv.calcOpticalFlowPyrLK(pre_gray, gray, pre_points, None, **lk_params)
    # st == 1 represents that the point is found
    good_points = points[st == 1]
    pre_good_points = pre_points[st == 1]
    for i, (good_p, pre_good_p) in enumerate(zip(good_points, pre_good_points)):
        if np.linalg.norm(good_p - pre_good_p) < 0.5:
            continue
        x, y = good_p.reshape(-1).astype(np.int64)
        pre_x, pre_y = pre_good_p.reshape(-1).astype(np.int64)
        mask = cv.line(mask, (x, y), (pre_x, pre_y), colors[i].tolist(), 2)
        frame = cv.circle(frame, (x, y), 5, colors[i].tolist(), 2)
    
    img = cv.add(frame, mask)
    cv.imshow('Frame', resize(img, height=650))
    cv.imshow('Mask', resize(mask, height=650))
    pre_gray = gray
    pre_points = good_points.reshape(-1, 1, 2)
    if cv.waitKey(30) >= 0:
        break
cap.release()
cv.destroyAllWindows()

```


---

## Lucas-Kaneda光流法整体流程

事实上，为了更加精准的结果，整个Lucas-Kaneda光流法还需要检测角点区域，整个算法流程如下：

<div align=center>
<img src="https://picx.zhimg.com/v2-c3db32c70c311bd8506e6fd37d8e186e_r.jpg?source=c8b7c179" style="width: 70%;"/>
<p class="img-caption">Lucas-Kaneda流程</p>
</div>




