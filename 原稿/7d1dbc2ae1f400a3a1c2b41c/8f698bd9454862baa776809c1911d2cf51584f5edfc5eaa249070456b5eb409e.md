最近上一门和计算机视觉相关的课（老板亲自讲的），其中的一些关于图像分割的一些老方法感觉非常有意思，简单查了一下，发现这些方法在简中互联网上都没有非常多的文章讲解。正好整理一下，以博后人一笑。

## Mean Shift 算法简介

### 从分割到聚类

对于图像分割算法，一个视角就是将图像中的某些点集分为一类（前景），另外一些点集分为另一类（后景）。这个视角本身和聚类是一样的， Mean Shift（均值漂移法） 的思想即为利用聚类算法将图像中的像素点聚类为不同的簇，从而达到分割的目的。而 Mean Shift 就是这样一类基于聚类的分割方法。

> 如果只是需要前景和背景的分割，那么就可以看成一个簇为2的一个聚类任务

这篇文章就简单介绍一下 Mean Shift 的数学原理和代码实现。

### 用概率密度估计函数的极大值点来聚类！

不同于`KMeans` 这样的原型聚类，Mean Shift 有一套自己的聚类方法，原理其实也很简单。相信部分读者在看到这个子标题时就已经茅塞顿开。但是，为了照顾其他读者，我仍然打算完整描述 Mean Shift 的聚类流程。此处，我借用 [MeanShift_py](https://github.com/mattnedrich/MeanShift_py) 这个项目中的图：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-0ea9aa86dd529330765930b5ed79a9af_1440w.gif" style="width: 70%;"/>
<p class="img-caption">图源：github 项目 MeanShift_py</p>
</div>

假设，我们在初始时有一堆的点 $x_1, x_2, ...,x_n$ 等待我们聚类（为了方便展示，就假设这些点都是二维的好了）。首先，根据这些点，我们能否找到一个概率密度函数 $\hat{f}(x)$ 来描述这些样本点的分布（也被称为概率密度估计函数，近似了样本点自身的概率密度函数），假设我们已经获得了这些样本点的分布函数 $\hat{f}(x)$ 后，我们只需要让每个样本点 $x_i$ 往顺梯度方向 $\nabla \hat{f}(x_i)$ 进行迭代即可，如下式：
$$
x_i \leftarrow x_i + \alpha \nabla \hat{f}(x_i)
$$

> 这个过程可以通过上图展示

很直观，这个过程中，每个点都会最终收敛到 $\hat f(x)$ 的局部最大值，再上图中，很明显， $\hat{f}(x)$ 有三个极大值点，所以所有点最终聚成了三个簇。在 Mean Shift 中，这个极大值点就是聚类的中心点，在原本的论文中被称为 mode，国内有些人将其翻译为“模点”。总之，在后文中，极大值点，簇中心，mode，指的都是一个东西。

如果看到这里，阁下能够理解上文表达的逻辑，即，我们如何依照样本点的概率密度估计函数来得到样本点的聚类中心，那么，你已经快完全理解 Mean Shift 方法了。剩下的问题就只剩一个了：如何得到这个密度估计函数 $\hat{f}(x)$ 呢？

### 核密度估计 KDE

估计一组样本点的概率密度函数存在很多方法，比如最经典的混合高斯模型（GMM），核密度估计（KDE），经验模式分解（EMD）等等，在 Mean Shift 中，采样的是计算速度相对较快的 KDE 作为 $\hat{f}(x)$ 的计算方法，你完全可以使用其他的密度函数估计法（1000%早就有人发过相关文章了）。为了防止阁下忘记这个小学知识点，笔者在此摆上 KDE 的基础表达式：
$$
\hat{f}_h(x) = \frac{1}{nh}\sum_{i=1}^nK(\frac{x-x_i}{h})
$$

> 其中 $K$ 是核函数，一般我们用高斯函数，$h$ 是 bandwidth（带宽）。

当然，已经有不少成熟的库实现了 KDE 的计算（甚至 matplotlib 可以很高效地直接绘制一组点的 KDE 曲线），阁下自然不需要手动实现。

好啦好啦，我们接下来就可以非常简单愉快地实现基于 Mean Shift 的图像分割了。

### 从聚类到分割

下面是 Mean Shift 图像分割算法的流程

输入：输入图像 $I$

输出：分割图像 $O$

处理：

1. 将输入图像读入到 RGB 空间，采用 RGB颜色核坐标x, y 作为图像的特征，得到 $feature = \{(r_i, g_i, b_i, x_i, y_i)\}_{i=1}^{I_h\times I_w}$
2. 对 $feature$ 的特征进行归一化
3. 使用 Mean Shift 算法找到 $feature$ 的 $M$ 个簇中心，记簇中心为 $\{c_i\}_{i=1}^M$
4. 将每个点重新赋值为它在 Mean Shift 中对应的那个中心点的簇序号（标签），从而得到 $label=\{c_i\}_{i=1}^{I_h\times I_w}$
5. $label$ 重新 reshape 成原本的图像并输出

这样，我们就得到了分割之后的图，当然，如果希望制定分割的类数，可以尝试调整 bandwidth 或者在聚类完成后改变数量较小的点的label（大概率会得到很多的簇）

---

## 代码实现

我们先预装一下要用到的库：

```bash
pip install numpy scikit-learn opencv-python
```

简单介绍一下本次需要分割的对象，为 `kvasir-seg` 中的图像，需要分割的为下图中的息肉

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8991aabeed19f82f2edfd8cd9effa41d_1440w.png" style="width: 50%;"/>
</div>

它的 ground truth 为：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4dd3ebeb1f2f6f27954dca23f8ff65d8_1440w.png" style="width: 50%;"/>
</div>

下面尝试使用 Mean Shift 和 KMeans 来解决。

### Mean Shift

先引入需要的库。

```python
from collections import Counter
import numpy as np
from PIL import Image
import cv2
from sklearn.cluster import MeanShift, KMeans
```



Mean Shift 对于噪音非常敏感，我们先进行去噪，并降采样：

```python
image = cv2.imread('./test.png')
image = cv2.GaussianBlur(image, ksize=(15, 15), sigmaX=10)
origin_h = image.shape[0]
# resize 函数详见结尾附录
image = resize(image, height=100)
```

然后制作每个像素点对应的五维特征：

```python
h, w = image.shape[:2]
features = []
for i in range(h):
    for j in range(w):
        pixel = image[i, j]
        if len(pixel.shape) == 0:
            pixel = [pixel.tolist()]
        else:
            pixel = pixel.tolist()
        pixel.append(i * 1.)
        pixel.append(j * 1.)
        features.append(pixel)

features = np.array(features)

normalized_features = features / features.max(axis=0)
```

然后进行 Mean Shift 聚类，并保存簇的个数：

```python
mean_shift_model = MeanShift(bandwidth=50)
clusters = mean_shift_model.fit(normalized_features)
cluster_num = Counter(clusters.labels_)
cluster_num
```

> 如果簇太多，就调大 `bandwidth` 的值

run in 44.6 s:

```九二一
8
```

可以看到，有 8 个簇，一般簇在20个以内算是比较正常的，太多说明效果不佳。

我们来看一下这8个簇合在一起的可视化效果：

```python
seg = clusters.labels_.reshape(h, w)
seg = (seg / seg.max() * 255).astype('uint8')
seg = resize(seg, height=origin_h)
image = Image.fromarray(seg)
image.save('mean_shift.png')
image
```

渲染效果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-bd65e23a32884e4a153be0b906301b46_1440w.png" style="width: 50%;"/>
</div>

可以看到，目标区域被两个mask给覆盖了，我们只需要将这两个 mask 合并一下就是最终的结果了：

```python
masks = []
for cluster_id in range(cluster_num):
    seg = np.where(clusters.labels_.reshape(h, w) == cluster_id, 255, 0)
    masks.append(seg.astype('uint8'))

# 通过可视化每一个 masks 中的元素找到需要的元素索引 0 和 3
select_mask_ids = [0, 3]
final_mask = np.zeros((h, w)).astype(np.bool_)
for mask_id in select_mask_ids:
    mask = masks[mask_id].astype(np.bool_)
    final_mask |= mask
final_mask = final_mask.astype('uint8') * 255
final_mask = resize(final_mask, height=origin_h)
final_mask_image = Image.fromarray(final_mask)
final_mask_image.save('mean_shift.mask.png')
final_mask_image
```

最终效果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-0280ef7c9305d283558d9847cf76dd15_1440w.png" style="width: 50%;"/>
</div>



### KMeans

接下来我们再试试 KMeans，步骤和上面几乎完全一样，只需要注意使用 KMeans 不需要下采样，并且，高斯平滑的参数设的小一些，我设置的如下：

```python
image = cv2.GaussianBlur(image, ksize=(5, 5), sigmaX=2)
```

调用 KMeans 的函数如下：

```python
kmean_model = KMeans(n_clusters=5, n_init='auto')
clusters = kmean_model.fit(normalized_features)
cluster_num = len(Counter(clusters.labels_))
cluster_num
```

其余都一样，值得一提的是，KMeans 在我的 256 核服务器上瞬间就跑完了，打上 Mean Shift 却需要 44 秒。下来看看 KMeans 的所有 mask 堆叠的效果：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-bf6e5cb8759553f6527ffcb66b2bf90d_1440w.png" style="width: 50%;"/>
</div>

可以看到，效果非常不错，我们只需要选一个部分就可以，最终效果如下：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-e5d47d3df6796170cf85d0faa8e5d667_1440w.png" style="width: 50%;"/>
</div>

### 指标计算

我们最后可以算一下分割指标。

> 分割指标的代码可以 copy 我的博客：https://kirigaya.cn/blog/article?seq=141

| 指标                    | KMeans | Mean Shift |
| :---------------------- | ------ | ---------- |
| Dice $\uparrow$         | 0.900  | 0.898      |
| IoU $\uparrow$          | 0.818  | 0.815      |
| Sensitivity  $\uparrow$ | 0.829  | 0.832      |
| PPV  $\uparrow$         | 0.984  | 0.975      |
| HD95  $\downarrow$      | 6.650  | 114.0      |

可以看到，两者性能难分伯仲（但是Mean Shift 的 HD95 却很大）。但是从笔者使用体验下来，无论是调参难度还是运行速度，都是 `KMeans` 更胜一筹。因为 KMeans 只需要控制生成的簇的个数。且原型聚类本身就很适合图像这种样本点比较多的情况下的快速聚类。

---

## 附录

resize函数：

```python
def resize(img : np.ndarray, height=None, width=None) -> np.ndarray:
    if height is None and width is None:
        raise ValueError("not None at the same time")
    if height is not None and width is not None:
        raise ValueError("not not None at the same time")
    h, w = img.shape[0], img.shape[1]
    if height:
        width = int(w / h * height)
    else:
        height = int(h / w * width)
    target_img = cv2.resize(img, dsize=(width, height))
    return target_img
```