## 前言

作为图像分割算法考古的第二篇文章，这次介绍的是冷门但是非常优雅的 **主动外观模型 AAM [1,2,3] （Active Appearance Model）**。

AAM 是一类非常经典的图像建模方法，它可以将一堆含有控制点或者 mask 的图像，建模成另外一种更加方便的方法，从而对图像及其标签进行更加统一的表示。基于 AAM 的这种图像表示方法，我们可以很方便地实现许多应用，比如人脸检测，目标跟踪，图像分割等等。与 Graph Cut 算法类似， AAM 在大部分情况下无法端到端地生成一张图的 mask，而需要用户输入一些先验信息（比如画一下前景图像大概在哪个区域内），这样的算法非常适合制作交互式的分割标注软件。

笔者惊喜地发现全网（简中 + Google）似乎没有任何一篇文章完整地讲过 AAM 算法的原理和实现。于是，怀着看完 AAM 原文和相关应用文章的激动心情，笔者重新翻开了阔别已久的计算机图形学教材，写下了这篇博客。仅抛砖引玉，望阁下笑纳。

---

## 前置知识

- 线性代数：实空间线性变换、SVD 、PCA等
- 计算机图形学：分段仿射变换（piecewise affine transform）
- 图像处理：Canny边缘检测
- python

> 其余基于这些知识的推导我会在文中给出，基础知识不再 cover 了 :D

我们先安装一下这次博客要用到的库，因为文章比较长，笔者建议阁下可以顺便制作这一天的咖啡以解劳形之文：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-86118a84cedaa3bd1111db04a4a5334c_1440w.png" style="width: 50%;"/>
</div>

```bash
pip install numpy matplotlib opencv-python pillow scikit-image scipy
```

**友情提示，最终代码我会整理并发布到 https://github.com/LSTM-Kirigaya/Active-Appearance-Model ，倘若读者没有额外的兴致，大可不必实现或者纠结这些代码。**

---

## 问题模型

### 控制点 control points

在 AAM 构建的数学模型中，对于一组输入图像 $I_i, i=1,2,...,N$，输入除了图像外，还有一组对于图像的控制点（control points）。什么是控制点？控制点直观上就是用来描述图像中某个物体结构信息的二维点集，比如下图中的71个控制点（绿色的）描述了人脸的结构信息。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-3a54e55aff52cacc431cf910cfcb6010_1440w.png" style="width: 60%;"/>
</div>

除了点本身外，很显然，这些点之间还存在拓扑信息（连线）。不过这些拓扑信息我们暂时用不到。

额外多一句话，这些图形学里面的控制点与图像学里面的mask其实是可以互相转化的。这些我们会在后续的代码中有所体现，所以还请急于实现算法的读者不要着急。

### 形体 shape

#### 定义 shape

控制点的引入让我们多了一些描述图像结构信息的手段，从而让我们拥有额外的空间定义术语。 AAM 将一张图像中我们关心的特征分为两个：形体（shape）和外观（appearance）。先看看 shape 是如何被定义的：记一幅图像 $I$ 的控制点为 $(x_1, y_1), (x_2, y_2), ..., (x_v, y_v)$，那么 $I$ 的 shape $s$ 被定义为：
$$
s = (x_1, y_1, x_2, y_2, ..., x_v, y_v)^T
$$
对于一个图像集合 $I_1, I_2, ...$，我们对如何用简洁的式子建模这堆图像的 $s$ 比较感兴趣， AAM 给出了达到这个简洁表达的假设：**$s$ 是线性变分的**，简单说就是，$s$ 可以用基形体（base shape） $s_0$ 和一堆形体分量的线性组合表示：
$$
s = s_0 + \sum_{i=1}^np_is_i
$$
在这个式子中，系数 $p_i$ 被称为形体参数（shape parameters）。因为 $s_i$ 表达了不同的形体分量，从定义上我们就希望 $s_i$ 线性无关，最好彼此正交。 $p_i$ 是后续拟合 AAM 模型时才需要考虑的参数，我们现在只需要获取正确合理的 $s_0$ 和 $s_i$ 就可以声称完成了 AAM 的 **形体建模** (shape modeling)。

> 和 n 阶单纯形的定义式好像。。。

问题来了，既然给出了模型，那么式子中的基形体 $s_0$ 和 形体分量 $s_i$ 要如何求解呢？

AAM 给出的答案是 `全局 Kabsch 算法` + `主成分分析 PCA`。假设我们的训练集中含有 $m$ 张图像，每张图像上有 $k$ 个控制点，那么这里的图像样本控制点可以表示为一个 $m\times k \times 2$ 的矩阵，记作 $C$。则 AAM 中 $s_0$ 和 $s_i$ 的计算思路为：

- 使用 $C$ 的形体均值（shape mean）表示 $s_0$，它可以表示所有形体的共模量。形体均值可通过【Kabsch 算法】迭代得到。
- 将 $C$ 展平成 $(m\times 2k)$ 的矩阵，对该矩阵以 $m$ 增大的方向进行 PCA，并取前 n 大的特征值对应的特征向量作为 $s_i$，它们刚好可以满足我们对于 $s_i$ 的假设。

实际上，第二步也不是直接对 $C$ 进行 PCA。在第一步 Kabsch 算法迭代中， $C$ 会和 $s_0$ 的初值一起进入一个迭代中，

主成分分析 PCA 相信大部分读者都学过，但是【Kabsch 算法】对于现在的小朋友而言可能鲜有人知，于是，笔者打算花点篇幅来讲解一下 【Kabsch 算法】以及如何将该算法应用在形体均值的计算上。

> 下面的三个 subsection 会用于讲解 Kabsch 算法 的原理和迭代计算形体均值的伪代码

---

#### 正交 Procrustes 问题

在线性代数中，找出尽可能将一个矩阵 $A$ 变换到 另一个矩阵 $B$ 的正交矩阵 $\Omega$ 问题被称为 [正交 Procrustes 问题](https://en.wikipedia.org/wiki/Orthogonal_Procrustes_problem)。在计算机图形学的语境下，$A, B$ 代表 $M\times N$ 的矩阵，$M$ 代表控制点的数量，$N$ 代表控制点的维度。在我们的问题中，因为处理问题只涉及 RGB 图像，所以显然 $N=2$。

> Procrustes 这个名字指的是希腊神话中的强盗，他通过伸展四肢或砍断受害者的四肢来让他们适应他的床。真不知道哪个小混蛋起的名字。如果是我应该会用我最喜欢的乐器或者食物来起名字。

形式化表达就是一个约束优化问题：
$$
\min_{\Omega} \Vert \Omega A - B \Vert_F \\ \text{s.t.}\ \ \Omega^T\Omega = I
$$
其中 $\Vert\cdot\Vert_F$ 是矩阵的 [Frobenius 范数](https://en.wikipedia.org/wiki/Frobenius_norm)。你当然可以用别的矩阵范数（如果你能解出来的话）。

> 在计算机图形学中，这个 $\Omega$ 有一个更加让人耳熟能详的名字——最优旋转矩阵（optimal rotation matrix）

很显然，在这个问题上，我们肯定希望这个正交矩阵 $\Omega$ 对于 $A$ 的行空间的变换不要改变方向（因为图像空间中坐标都是大于0的），所以这个问题中还需要加一个约束，即，$\Omega$ 的所有特征值都大于 0。而，小学数学告诉我们，正交矩阵的实数特征值只能是 $\pm 1$，所以所有特征值只能是 1。所以这个“不改变 $A$ 行空间变换”的额外约束等价于：
$$
\det\Omega = \prod \lambda_i = 1
$$
ok，在我们这个问题中，最终的约束优化问题如下：
$$
\min_{\Omega} \Vert \Omega A - B \Vert_F \\ \text{s.t.}\ \ \Omega^T\Omega = I \land \det \Omega = 1
$$
这个优化问题很简单，直接求解即可：
$$
\begin{aligned}
\hat{\Omega} &= \arg\min_{\Omega} \Vert \Omega A - B \Vert_F^2 \\
&= \arg\min_{\Omega} \Vert A \Vert_F^2 + \Vert B \Vert_F^2 - 2\langle \Omega A, B \rangle_F\\
&= \arg\max_{\Omega} \langle \Omega A, B \rangle_F \\
&= \arg\max_{\Omega} \langle \Omega, BA^T \rangle_F
\end{aligned}
$$
因为上面的式子中，需要求解的 $\Omega$ 是正交矩阵，如果将 $BA^T$ 中的正交分量分出来就可以和 $\Omega$ 一起组成正交矩阵了，所以很自然地想到可以将 $BA^T$ 进行 [SVD 分解](https://zh.wikipedia.org/zh-hans/%E5%A5%87%E5%BC%82%E5%80%BC%E5%88%86%E8%A7%A3)：

$$
\begin{aligned}
\hat{\Omega} &= \arg\max_{\Omega} \langle \Omega, U\Sigma V^T \rangle_F \\
&= \arg\max_{\Omega} \langle U^T \Omega V, \Sigma \rangle_F
\end{aligned}
$$

三个正交矩阵之积还是正交阵，而正交矩阵的每个元素的绝对值都小于等于1，又因为 $\Sigma = \mathrm{diag}(\sigma_1, \sigma_2,...,\sigma_r, ..., 0)$，所以上式最大时，$U^T\Omega V$ 代表的空间旋转一定要把前 r 个轴转到新空间的轴上，为了方便计算，我们不妨：
$$
U^T\Omega V = \mathrm{diag}(\pm1, \pm1, ..., \pm1) := I^*
$$

此时：

$$
\begin{aligned}
U^T\hat{\Omega} V &= I^* \\
\hat{\Omega} &= UI^*V^T
\end{aligned}
$$


不要忘记了，我们还有一个约束没有满足，那就是 $\det \Omega = 1$. 那么我们代入计算一下，利用行列式计算的线性性质。
$$
\begin{aligned}
\det \hat{\Omega} &= \det (UI^* V^T) \\
&= \det U \det I^* \det V^T \\
&= \det (UV^T) \det I^* \\
\end{aligned}
$$
为了让上式等于1，我们可以简单做个判断：

- 如果 $\det UV^T = 1$，则 $\det I^* = 1$，取 $I^* = I$
- 如果 $\det UV^T = -1$，则 $\det I^* = -1$，取 $I^* = \mathrm{diag} (1,1,...,1,-1)$

整理一下就是：
$$
\hat{\Omega} = \begin{cases}
UV^T & \det UV^T = 1 \\
U\mathrm{diag} (1,1,...,1,-1) V^T & \det UV^T = -1
\end{cases}
$$
okey dockey，这个优化问题的结果，就是计算机图形学中的 [Kabsch 算法](https://en.wikipedia.org/wiki/Kabsch_algorithm) 中最核心的一个步骤。

> 如果是对 $AB^T$ 做 SVD，那么很好证明： $\hat{\Omega} = (UI^\*V^T)^T=VI^*U^T$

#### Kabsch 算法

**Kabsch 算法**，有时也叫**Kabsch-Umeyama**算法，是以 Wolfgang Kabsch 和 Shinji Umeyama 命名的。它负责计算从矩阵 $A$ 矩阵 $B$ 的最优旋转矩阵。基于上文 Procustes 问题的求解答案，此时我就不再赘述 Kabsch 算法的证明，而是直接给出算法步骤：

输入：原点集 $A$，目标点集 $B$.

输出：旋转矩阵 $R$.

处理：

1. 计算协方差矩阵 $H = AB^T$.
2. 对 $H$ 进行 SVD 分解： $H = U\Sigma V^T$.
3. 计算旋转矩阵 $R = VU^T$.
4. 如果 $\mathrm{det}\  R = 1$，返回 $R$ .
5. 否则，$R = V\mathrm{diag}(1, 1,...,1,-1) U^T$

python 实现如下：

```python
def compute_kabsch_matrix(X: np.ndarray, Y: np.ndarray) -> np.ndarray:
    """
    Description
    	compute rotatiom matrix transform X to Y
    Param
        X: [k, 2]
        Y: [k, 2]
    """
    U, Sigma, Vt = np.linalg.svd(X @ Y.T)
    R = Vt.T @ U.T
    if np.linalg.det(R) == 1:
        return R
    else:
        I_star = np.eye(U.shape[0])
        I_star[-1, -1] = -1
        R = Vt.T @ I_star @ U.T
        return R
```



#### 形体建模算法 shape_modeling

实际上，得到的 $R$ 作用在 $A$ 上后，也就是 $RA$ 不一定和 $B$ 同向，在实际编程中，我们往往会迭代地进行上面的步骤，并且为了抑制源与目标点集的偏移分量，我们会对 $A,B,RA$ 进行归一化，也就是减去各自的中心点（质心）再除以减去中心点后点集的某种范数（一般用 F 范数）。对于 $C\in\mathbb R^{m\times k\times 2}$ 实现这个归一化很简单：

```python
def normalise_shape(points_sets: np.ndarray) -> np.ndarray:
    """
   	Param
   		points_sets : [m, k, 2]
   		e.g. points_sets.shape=[110, 71, 2] 代表有 110 张图像，每张图像有 71 个控制点
   	Return
		normalise_z: [m, k, 2]
    """
    centroids = np.mean(points_sets, axis=1, keepdims=True)
    normalise_points_sets = points_sets - centroids
    points_sets_norm = np.linalg.norm(normalise_points_sets, axis=(1, 2), keepdims=True)
    normalise_points_sets /= points_sets_norm
    return normalise_points_sets
```



有了上述的算法后，想要根据输入的 $C$ 计算出 $s_0$ 就变得轻而易举了。我们只需要迭代地进行 Kabsch 变换即可。需要注意的是，我们默认使用 $C$ 中的第一张图像的形体 $s$ 作为 $s_0$ 的初始值。

Talk is cheap, show me the code。为了节约篇幅，我们不妨直接看看代码：

```python
def shape_mean(points_sets: np.ndarray) -> Tuple[np.ndarray, np.ndarray]:
    """
    Description
    	根据输入点集计算形体均值 s_0
    Param
    	points_sets: [m, k, 2]
    Return
    	s_0: 计算得到的形体均值
    	C: 朝向 s_0 以 kabsch 矩阵旋转后的结果
    """
    C = points_sets
    # 使用第一个图的形体作为 s_0 的初始值
    s_0 = np.expand_dims(C[0], axis=0)
    image_num = C.shape[0]
    last_s_0 = s_0
    while True:
        C = normalise_shape(C)
        s_0 = normalise_shape(s_0)
        for i in range(image_num):
            R = compute_kabsch_matrix(C[i], s_0[0])
            C[i] = R @ C[i]
        s_0 = np.mean(C, axis=0, keepdims=True)
        s_0 /= np.linalg.norm(s_0)
        if np.linalg.norm(last_s_0 - s_0) < 1e-5:
            break
        last_s_0 = s_0
    return s_0, C
```



在得到 $s_0$ 后，只需要对 $C$ 进行 PCA 就能得到 $s_i$ 了，在得到 $s_0$ 和 $s_i$ ，我们就可以声称完成了 shape 的建模。我们可以将shape建模的过程写成一个简单的函数：

```python
def shape_modeling(points_sets: np.ndarray) -> Tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
    """
    """
    assert len(points_sets.shape) == 3, 'ensure `points_sets` is a 3D tensor!'
    s_0, aligned_C = shape_mean(points_sets)
    
    # 将 控制点集合 转换为 形体集合
    s_0 = s_0[0].reshape(-1)
    aligned_C = aligned_C.reshape(len(aligned_C), -1)
    
    # 进行 PCA，压缩图像数量的维度
    cov_matrix = np.cov(aligned_C.T)
    eig_values, eig_vecs = np.linalg.eig(cov_matrix)
    
    indice = eig_values.argsort()[::-1]
    eig_values = eig_values[indice]
    eig_vecs = eig_vecs[:, indice]
    
    return s_0, aligned_C, eig_values, eig_vecs
```



#### shape_modeling 结果可视化

好吧，我承认上面的步骤可能比较抽象，如果阁下对于基于 PCA 的图像分析不是非常地得心应手的话。不过没有关系，我们可以简单看一下上述过程的可视化结果，从而先对 shape modeling 建立一个感性的认知。此处我们选取的数据集为 `IMM-Frontal Face DB`，[下载链接](https://www2.imm.dtu.dk/pubdb/pubs/3943-full.html)。shape modeling 的过程中，我只选取其中的146张图像。每张图像都含有人像和对应的73个控制点。形如 [控制点 control points] 这一章中的那张图像一样。我们现在对这146张图像进行 shape modeling，并看一下最终得到的 $s_0$ 和  $s_i$。为了决定要看几个 $s_i$ ，我们先看一下特征值的吧，为了美观一些，我只展示前10大特征值的 $i\sim\log \lambda_i$ 曲线：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b7b6b2af7eb92a4031eed6fe482f7fab_1440w.png" style="width: 40%;"/>
</div>

可以看到，大部分的成分都归于前三个特征值，所以我们只展示 $s_0$ 和 $s_1, s_2, s_3$ 的特征向量重组成的控制点集：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b36e626cb77115fca764dfe9fdace086_1440w.png" style="width: 90%;"/>
</div>

可以看到一个非常神奇的现象，$s_1\sim s_3$ 都维持了稳定的状态，且描述了控制点表示人脸的不同方向，具体为什么会形成这样的结果读者可以自行思考。我们必须马不停蹄地赶往 AAM 的下一个阶段了。



---

### 外观 appearance

通过上述的步骤，我们完成了含有控制点的图像集合的建模。但是控制点只是描述了图像的结构信息，为了描述图像本身的一些其他信息（比如像素值大小，色彩域信息等），我们还需要建模与像素图像本身相关的信息。 AAM 将与图像本身相关的信息称为 外观（Appearance，可能是当时的任务多集中于和人脸相关的视觉任务上，所以取名为外观，毕竟人脸图像上与像素值最相关的概念就是“外观”了）

>  在有的文章和代码中，会用 纹理 （Texture）一词来指代外观。为了统一，本文统一使用外观一词。



#### 定义 appearance

与形体 shape 类似， AAM 将 外观 $A$ 也定义为一个线性变分模型：
$$
A(x) = A_0(x) + \sum_{i=1}^m \lambda _i A_i(x)\quad \forall x \in s_0
$$
其中 $x$ 代表图像中的像素点， $s_0$ 是我们上一步求取的形体均值（我们用形体均值来表示基形体）， $A_0$ 是基外观， $A_i$ 是外观分量。上面的式子表示，外观的线性变分模型的线性关系仅仅建立在 $s_0$ 框定的范围中。

和 shape 一样， $\lambda_i$ 参数只有在拟合模型时才需要考虑，此时我们只需要获取 $A_0$ 和 $A_i$ 就可以声称完成了 **外观的建模** (Appearance Modeling)。

> 有的文章中，将 shape 中的 $p_i$ 和 appearance 中的 $\lambda_i$ 进行耦合，即认为 $p_i = \lambda_i$，这样的 AAM 模型被称为 **Combined AAMs**。本文中将 $p_i$ 和 $\lambda_i$ 分开定义的 AAM 被称为 **Independent AAMs**。感兴趣的读者可以通过阅读文献[4] 的方式来进一步了解这两种方法上的区别。为了节约篇幅，本文不再赘述。



#### appearance标准化——分段仿射变换 piecewise affine transform

这里面涉及到一个有趣的问题——我们的数据集中的不同图像的控制点得到的形体 $s$ 都不一样，但是 Appearance 是定义在 $s_0$ 内的，很显然，我们需要将不同 $s$ 的图像归一化到 $s_0$ 的范围内，也就是对于 $s$ 中的每一个像素点，我们希望找到它在 $s_0$ 表示下位于哪个位置。这一点学过计算机图形学的同学应该直接就想到了 **分段仿射变换** （piecewise affine transform），后文简称 PAT 变换。

> 不了解的同学可以参考 Lecture 的前两页，或者其他的计算机图形学非刚性变换章节：https://www.doc.ic.ac.uk/~dfg/graphics/graphics2010/GraphicsLecture16.pdf

PAT 变换的做法其实也很简单：

1. 根据控制点序列构建 Delaunay 剖分模型，记作 $\mathcal D_1$，下图展示了控制点构建成 Delauny 剖分模型后的样子（绿点代表训练集的标签控制点）：

    > 不要在意 Delaunay 代码，因为我们会直接通过 scikit-image 中方法完成 Delaunay 剖分模型，此处只是原理讲解，通过原理讲解，阁下或许能更好地了解到输入的参数应该怎么排布。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-6b971b023db0a318194b2ba708a393a3_1440w.png" style="width: 70%;"/>
</div>

2. 将之前得到的 $s_0$ 根据这张图的控制点的中心点坐标和中心化后的范数反标准化到该图像的控制点的中心来。

    反归一化代码很简单：

    ```python
    def denormalise_shape(points_sets: np.ndarray, reference_points_sets: np.ndarray):
        """
        将中心化空间内的 points_sets 变换到真实空间 reference_points_sets 中
        Param
       		points_sets : [m, k, 2]
            reference_points_sets: [m, k, 2] 
       		e.g. points_sets.shape=[110, 71, 2] 代表有 110 张图像，每张图像有 71 个控制点
       	Return
    		normalise_z: [m, k, 2]
        """
        centroids = np.mean(reference_points_sets, axis=1, keepdims=True)
        normalise_points_sets = reference_points_sets - centroids
        points_sets_norm = np.linalg.norm(normalise_points_sets, axis=(1, 2), keepdims=True)
        denormalise_points_sets = points_sets * points_sets_norm + centroids
        return denormalise_points_sets
    ```

    对于反归一化的结果。同样也构建 Delaunay 剖分模型，记作 $\mathcal D_2$ ，可视化结果如下（紫色点代表 $s_0$  reshape 后的控制点）：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-08ceeb57b187d448131147be2d309518_1440w.png" style="width: 70%;"/>
</div>



3. 对于 $\mathcal D_1$ 的每个三角面片（后文简称 mesh）中的控制点，记 mesh 的每条边的单位向量表示为 $\alpha,\beta,\gamma\in\mathbb R ^2$，其中 $\gamma = 1 - \alpha - \beta$ （你当然可以只用 $\alpha, \beta$ 表示，这是你的自由）。对于 mesh 中的每一个像素点坐标 $p\in \mathbb R^2$，我们可以用这三个向量线性表示： 

> 你当然可以使用更高阶的单纯形来构造 mesh

$$
p = \begin{pmatrix} x_1 & x_2 & x_3 \end{pmatrix} \begin{pmatrix} \alpha \\ \beta \\ \gamma \end{pmatrix}:= x^T M
$$

不过嘛，这个做法不太正统，更加正统的做法是把这个式子化成一个齐次式，我们做如下的增广：
$$
p \rightarrow (p\quad 1)\\
M \rightarrow \begin{pmatrix} \alpha & 1 \\ \beta & 1 \\ \gamma & 1 \end{pmatrix}
$$
这样， $M$ 就从 $\mathbb R^{3\times 2}$ 变成 $\mathbb R^{3\times 3}$，加上二阶单纯形的好性质，我们可以断言，$M$ 是可逆的，这一点在后续的求解中会带来巨大的好处。

同理，我们也能在 $s_0$ 中表示 $p$ 在 PAT 变换之后的坐标。我们必然希望变换之后的 $p$ 在 $x_0$ 的对应 mesh 下的这些系数 $x_1,x_2,x_3$（类似于左边）不变，于是有：
$$
p' = \begin{pmatrix} x_1 & x_2 & x_3 \end{pmatrix} \begin{pmatrix} \alpha' \\ \beta' \\ \gamma' \end{pmatrix} := x^T M'
$$
线性变换已经呼之欲出了，我们简单用用小学数学就能解决惹：
$$
\begin{aligned}
p' &= x^T M' \\
&= x^T M M^{-1} M' \\
&= p M^{-1} M'
\end{aligned}
$$
fine，这样我们就找到了从 $p$ 到 $s_0$ 的空间的 $p'$ 的线性变换，那就是 $M^{-1} M'$，这两个矩阵都是已知的。

4. 根据 第三步 找到的线性变换 $M^{-1} M'$，对每个像素点施加其对应mesh构造出的线性变换，从而计算出它在 $s_0$ 中的位置；对于不在 mesh 中的元素，直接赋值0。下图展示了变换之后的人脸的结果。左图中的绿点代表标签中的控制点， 紫色代表了 $s_0$ 对应的控制点，右图是变换结果：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-569ec0d79ddd6f6f663a960c4fe6ed7c_1440w.png" style="width: 70%;"/>
</div>



对训练集的每一张图像这么做，我们就完成了基于 $s_0$ 对于所有图像进行归一化。



#### 外观建模算法 appearance_modeling

既然我们做到了对于外观的归一化，那么接下来自然就能仿照 shape_modeling 中的做法，来计算 $A_0$ 和 $A_i$ 。具体做法和 shape 差不多，只不过计算 $A_0$ 的迭代中把 Kabsch 变换变成了 Z 归一化，读者可以自行尝试使用其他的归一化方法来计算外观均值。此处只给出对应的代码。

```python
def normalise_appearance(image: np.ndarray, s: np.ndarray, s_0: np.ndarray):
    assert len(s.shape) == 1
    assert len(s_0.shape) == 1
    
    source_control_points = s_0.reshape(-1, 2)
    target_control_points = s.reshape(-1, 2)
    h, w = image.shape[:2]
    affine_trasnform = skimage.transform.PiecewiseAffineTransform()
    affine_trasnform.estimate(src=source_control_points, dst=target_control_points)
    warped_image = skimage.transform.warp(image, affine_trasnform)
    return warped_image


def appearance_mean(images: np.ndarray) -> Tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
    image_num = images.shape[0]
    
    A_0 = np.expand_dims(images[np.random.randint(image_num)], 0)
    
    last_A_0 = A_0
    while True:
        beta = np.mean(images, axis=(1, 2), keepdims=True)
        alpha = np.mean(images * A_0, axis=(1, 2), keepdims=True)
        
        normalise_images = (images - beta) / alpha
        
        A_0 = np.mean(normalise_images, axis=0, keepdims=True)
        A_0 -= np.mean(A_0, axis=(1, 2), keepdims=True)
        A_0 /= np.mean(A_0 ** 2, axis=(1, 2), keepdims=True)
        
        if np.linalg.norm(last_A_0 - A_0) < 1e-6:
            break
        
        last_A_0 = A_0
    
    return A_0, normalise_images, beta, alpha
    

def appearance_modeling(images: np.ndarray) -> np.ndarray:
    assert len(images.shape) == 3
    image_num = len(images)
    A_0, normalise_images, beta, alpha = appearance_mean(images)
    cov_matrix = np.cov((normalise_images.reshape(image_num, -1) - A_0.reshape(1, -1)).T)
    eig_values, eig_vecs = np.linalg.eig(cov_matrix)
    indice = eig_values.argsort()[::-1]
    eig_values = eig_values[indice]
    eig_vecs = eig_vecs[:, indice]
    
    return A_0, normalise_images, eig_values, eig_vecs
```



和之前一样，我们先简单看一下特征值的分布情况。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-16cb979b8a8d80aa0f836400462636b0_1440w.png" style="width: 40%;"/>
</div>



可以看到，在大约在 $\lambda_{120}$ 左右，特征值发生了锐减。通过计算，我们可以得出前100个特征值占了总特征值之和的 99.4%，于是，我们可以粗浅地之采取前100个特征值。我们下面简单阅览一下 $A_0$ 和 $A_1, A_{10}, A_{100}$ 的可视化结果。

> 为了方便大家复现，输入的图像预先被我 resize 到了 60 × 60

<div align=center>
<img src="https://picx.zhimg.com/80/v2-767a6e638374571b572960b63761c8f1_1440w.png" style="width: 90%;"/>
</div>

可以看到，这个结果是非常符合预期的：随着特征值的减小，能够反映人脸成分的效果越来越差，可以想象，再往后的特征值几乎都是噪音了。

于是乎，我们可以说我们完成了对于 Appearance 的建模。

---

## AAM 的推理过程——AAM 拟合

上述计算出 $s_0$,  $s_i$,  $A_0$, $A_i$ 的过程可以称之为 AAM 的建模过程，也能认为是 AAM 的训练过程。那么 AAM 的推理过程是什么样的呢？其实如果已经获取了shape 和 appearance 的线性变分模型，我们其实可以用这对模型来表示任何一张同类型的图像。

对于一组给定的形体参数 $p$ ，我们可以唯一地确定一个形体
$$
s = s_0 + \sum_{i=1}^{k_s} p_i s_i
$$
其中 $k_s$ 是形体分量的个数，根据之前的推导，我们已经知道了这个 $k_s$ 代表 PCA 后选取的特征值的数量，在本文实验中，我们选择的 $k_s$ 满足让前 $k_s$ 个特征值的和占总特征值之和的 99%。

有了原形体 $s_0$ 和目标形体 $s$， 我们就可以使用 PAT 计算出一个唯一的仿射变换 $W$。于是，我们用 $W(x; p)$ 代表形体参数 $p$ 下确定的仿射变换作用在 $x\in I(x)$ 上的结果，此处的 $x$ 代表的是坐标，而非像素值， $I(x)$ 代表输入图像。显然，这个仿射变换后的结果符合我们之前对于外观的定义，所以，我们希望能够让 $W(x;p)$ 的结果和 $A$ 尽可能接近。而根据外观的先行变分假设，我们有
$$
A(x) = A_0(x) + \sum_{i=1}^{k_A}\lambda_i A_i(x)
$$
其中 $k_A$ 代表选择的特征值的数量，满足条件 $k_s$ 一样。所以对于输入图像 $I$ 位于 $s$ 中的每一个像素点 $x$，我们都希望 $W(x, p)$ 和 $A(x)$ 尽可能接近，于是我们就有了需要优化的目标。但是在构建优化目标之前，我们需要思考，目前有两个坐标系——输入图像的坐标系和 AAM 模型坐标系，我们需要统一坐标系才能构建出正确的优化目标，此时我们尝试使用 AAM 坐标系进行构建。$x$ 如果是 $s_0$ 中的一个像素点坐标，那么它在输入图像坐标系下对应的坐标为 $W(x;p)$。于是，$x$ 在 AAM 中对应的像素值也就是 $A(x)$，$x$ 在输入图像坐标系下的像素值就是 $I(W(x;p))$。

### AAM 的优化目标

因此，我们得到了优化的目标：
$$
E(p, \lambda) = \sum_{x\in s_0}\left[ A_0(x) + \sum_{i=1}^{k_A} \lambda_i A_i(x) - I(W(x;p)) \right]^2
$$
我们如果可以成功优化上面这个目标从而得到估计的 $p$ 和 $\lambda$：
$$
\hat{p}, \hat{\lambda} = \arg\min_{p, \lambda} E(p, \lambda)
$$
那么，我们就可以将输入的图像嵌入到我们的 AAM 模型中，这样，就能直接得到输入图像的 shape 和 appearance了。有了 shape 和 appearance，许多的视觉任务也就迎刃而解了。比如，如果知道了输入图像的 shape，就可以通过一些简单的线性变换直接得到输入图像的 mask ，从而完成图像分割的任务。

所以如何优化这个函数呢？最粗暴的方法就是直接用梯度下降法求，但是分段仿射变换的梯度求取是比较麻烦的一件事，使用 梯度下降法 求上面的式子虽然很准确，但是优化空间比较大，导致 AAM 在推理一张图像时得到的结果比较慢。所以在 AAM 模型中，优化器的设计与实现在曾经一直是一个热门问题。参考文献 [4] 的 section 3 中详细描述了各种不同的 AAM 优化器。如果读者感兴趣，可以前去阅读。此处笔者只给出非常经典的 Lucas-Kanada 优化器 解读。这里可能会涉及到一些最优化的知识，完全没有优化理论基础的读者不妨移步我曾经的高赞文章系列：

- https://zhuanlan.zhihu.com/p/271088190
- https://zhuanlan.zhihu.com/p/273726875
- https://zhuanlan.zhihu.com/p/293951317
- https://zhuanlan.zhihu.com/p/306635632
- https://zhuanlan.zhihu.com/p/338829644
- https://zhuanlan.zhihu.com/p/338838078



### Lucas-Kanada 优化器

Lucas-Kanada 将损失函数简化为：
$$
E(p) = \sum_{x} \left[ A_0(x) - I(W(x;p)) \right]^2
$$


其中的 $A_0(x)$ 是模板图像（个人理解中，就是外观均值），是一个常数。原本问题直接优化 $p$ 比较麻烦， Lucas-Kanada 中转而先固定一个初始化的 $p$ ，然后去优化 $\Delta p$：
$$
\sum_{x} \left[ A_0(x) - I(W(x;p + \Delta p)) \right]^2
$$
使用 Taylor 级数展开我们可以将上面的问题进一步近似：
$$
\sum_{x} \left[ A_0(x) - I(W(x;p)) -\nabla I \frac{\partial W}{\partial p}\Delta p \right]^2
$$
其中 $\nabla I$ 是 $W(x; p)$ 这一点的梯度，你可以通过确定点之后使用 Sobel 算子来获取；$\frac{\partial W}{\partial p}$ 是 PAT 变换在 $p$ 这一点的雅可比矩阵。上式对 $\Delta p$ 求导等于0，可以解得：
$$
\hat{\Delta p} = H^{-1} \sum_{x} \left[\nabla \frac{\partial W}{\partial p} \right]^T [A_0(x) - I(W(x;p))]
$$
其中 $H$ 是使用高斯-牛顿估计法估计的 Hessian 矩阵：
$$
H = \sum_{x} \left[ \nabla I \frac{\partial W}{\partial p} \right]^T \left[ \nabla I \frac{\partial W}{\partial p} \right]
$$
进行一轮这样计算后，我们可以使用估计出来的形体参数增量 $\hat{\Delta p}$ 对 $p$ 做更新：
$$
p \leftarrow p + \hat{\Delta p}
$$
有关 Lucas Kanada 优化器的实现细节，有一篇论文讲得很好，详见文献 [9]。不过阁下不必直接攻读原论文，我会为你们代劳。在后面的代码实现中，上式中的变量的代码实现会被揭露。

---

## 代码实现：基于 AAM 的图像分割

下面终于开始代码环节了（上一次这么慢还是在写共轭梯度法时），与之前一样，我们本次实验还是采用的 Kvasir-Seg 数据集中的图像。

### 导入基础库 & 检视数据根目录

先导入预先的库：

```python
import os
import json
import shutil

import cv2
from PIL import Image
import numpy as np
import tqdm
```

设置基本的源数据和目标数据目录，使用 one hold 进行 8 : 2 的切分：

```python
kavsir_home = '../datasets/Kvasir-SEG'
kavsir_meta_path = os.path.join(kavsir_home, 'kavsir_bboxes.json')
kavsir_images = os.path.join(kavsir_home, 'images')
kavsir_masks = os.path.join(kavsir_home, 'masks')
new_dataset_home = './aam_datasets'
new_train_images = os.path.join(new_dataset_home, 'trainset')
new_test_images = os.path.join(new_dataset_home, 'testset')
kavsir_meta: dict = json.load(open(kavsir_meta_path, 'r', encoding='utf-8'))

image_names = os.listdir(kavsir_images)
image_indice = np.arange(len(image_names))
train_image_indice = np.random.choice(image_indice, size=int(len(image_indice) * 0.8))
test_image_indice = np.array(list(set(image_indice) - set(train_image_indice)))
```



### 将 mask 转换为 控制点序列

需要知道的是，AAM 的建模过程是基于控制点的，所以我们需要先将一张 mask 图像转化为对应的控制点序列文件，对于任何一张 mask 位图，我们需要转化为 `pts` 文件，一个 `pts` 文件形式如下：

```python
version: 1
n_points:  68
{
157.404624 295.066474
...
...
299.643851 438.082621
}
```

于是，我们先进行控制点序列转换，这里用到的算法为 `Canny 边缘检测` + `Satoshi Suzuki 轮廓提取算法`。为了省事，我们只对只有单个 mask 联通量的进行转换，并且制定采样点为68个，转换脚本如下：

```python
def mask2points(mask: np.ndarray, point_num: int) -> np.ndarray:
    edge = cv2.Canny(mask, 50, 150)
    contours, _ = cv2.findContours(edge, mode=cv2.RETR_TREE, method=cv2.CHAIN_APPROX_NONE)
    roi_points = contours[0].reshape(-1, 2)
    roi_point_num = len(roi_points)
    if roi_point_num > point_num:
        indices = np.arange(0, roi_point_num, roi_point_num // point_num)
        roi_points = roi_points[indices]
    return roi_points[:point_num]

def make_new_dataset(home_path: str, image_indice: np.ndarray, control_point_num: int = 68):
    os.makedirs(home_path, exist_ok=True)
    for i in tqdm.tqdm(image_indice, colour='orange', ncols=80):
        image_name = image_names[i]
        image_id = image_name.split('.')[0]
        bbox = kavsir_meta.get(image_id, { 'bbox': [] })['bbox']
        if len(bbox) > 1:
            print(f'{image_name} has {len(bbox)} bboxes, skip')
            continue
        pts_name = f'{image_id}.pts'
        target_image_path = os.path.join(home_path, image_name)
        target_pts_path = os.path.join(home_path, pts_name)
        
        shutil.copy(os.path.join(kavsir_images, image_name), target_image_path)
        pts = f"version: 1\nn_points:  {control_point_num}\n{{\n"
        mask_image = cv2.imread(os.path.join(kavsir_masks, image_name))
        points = mask2points(mask_image, point_num=control_point_num)
        for point in points:
            x = round(float(point[0]), 6)
            y = round(float(point[1]), 6)
            pts += f'{x} {y}\n'
        pts += '}'
        open(target_pts_path, 'w', encoding='utf-8').write(pts)

make_new_dataset(new_train_images, train_image_indice)
make_new_dataset(new_test_images, test_image_indice)
```

这样我们就得到了如下的 测试集 和 训练集，它们都符合基本的 pts 数据集规范：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-a5b176e48329b91c0c5abffa08ff7473_1440w.png" style="width: 70%;"/>
</div>



请放宽心，我设计的这组算法能让控制点精准吻合到息肉上：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-5e88ec01b491bcf6f3923816d57a21f0_1440w.png" style="width: 40%;"/>
</div>





### 计算 shape 并选择 $k_s$

数据集制作完成后只需要进行读入和建立即可：

```python
from shape import shape_mean, shape_modeling, normalise_shape, denormalise_shape
C = []

for image_file in os.listdir(new_train_images):
    if image_file.endswith('.jpg'):           
        image_path = os.path.join(new_train_images, image_file)
        points, _, _ = get_coordinates(image_path)
        if points.shape == (68, 2):
            C.append(points)

C = np.array(C)
s_0, aligned_C, eig_values, eig_vecs = shape_modeling(C)
```

计算满足 $\sum_{i=1}^{k_s} \lambda_i / \sum_{i=1}^N > 99\%$ 的最小的 $k_s$ 的值：

```python
def compute_min_k(eig_values: np.ndarray, ratio: float) -> int:
    sum_eig_value = eig_values.real.sum()
    accumlate = 0
    k = 0
    for value in eig_values.real:
        k += 1
        accumlate += value
        if accumlate / sum_eig_value > ratio:
            break
    return k
k_s = compute_min_k(eig_values, 0.99)
```

通过计算，可以得到只需要选择 $k_s = 3$ 就能满足我们的要求。然后我们看看训练出来的 形体均值和前几个 $s_i$ 代表的息肉轮廓张什么样子：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-2cc366b377d74081d67ae2ffdbf0e007_1440w.png" style="width: 90%;"/>
</div>

上面没有闭合的部分和我们生成采样点的算法有关，读者可以发挥想象完善我的控制点转换算法。我们可以随便找一张图像看看 $s_0$ 在上面长什么样子，可视化代码如下：

```python
import scipy.spatial
import matplotlib.pyplot as plt

test_image_path = os.path.join(new_train_images, 'cju1csmlc4ht10799b8ymmghg.jpg')
points, _, _ = get_coordinates(test_image_path)
s = np.array(points).reshape(1, -1, 2)
s_0_points = denormalise_shape(s_0.reshape(1, -1, 2), s).reshape(-1, 2).astype(np.int_)
draw_image = cv2.imread(test_image_path)

# draw points
for point in points:
    cv2.circle(draw_image, center=point, radius=1, color=(10, 200, 10), thickness=3)
plt.figure(dpi=120, figsize=[20, 6])
plt.subplot(1, 3, 1)
plt.title('origin image rendered with label points')
plt.imshow(draw_image[..., ::-1])

delaunay = scipy.spatial.Delaunay(s_0_points)
for simp in delaunay.simplices:
    cv2.line(draw_image, pt1=points[simp[0]], pt2=points[simp[1]], color=(200, 10, 10), thickness=1)
    cv2.line(draw_image, pt1=points[simp[1]], pt2=points[simp[2]], color=(200, 10, 10), thickness=1)
    cv2.line(draw_image, pt1=points[simp[2]], pt2=points[simp[0]], color=(200, 10, 10), thickness=1)

plt.subplot(1, 3, 2)
plt.title('origin image rendered with delaunay')
plt.imshow(draw_image[..., ::-1])

draw_image = cv2.imread(test_image_path)

delaunay = scipy.spatial.Delaunay(s_0_points)
for point in s_0_points:
    x = int(point[0])
    y = int(point[1])
    cv2.circle(draw_image, center=(x, y), radius=1, color=(10, 200, 10), thickness=3)
    
for simp in delaunay.simplices:
    cv2.line(draw_image, pt1=s_0_points[simp[0]], pt2=s_0_points[simp[1]], color=(200, 10, 10), thickness=1)
    cv2.line(draw_image, pt1=s_0_points[simp[1]], pt2=s_0_points[simp[2]], color=(200, 10, 10), thickness=1)
    cv2.line(draw_image, pt1=s_0_points[simp[2]], pt2=s_0_points[simp[0]], color=(200, 10, 10), thickness=1)

plt.subplot(1, 3, 3)
plt.title('origin image rendered with $s_0$ and its delaunay')
plt.imshow(draw_image[..., ::-1])
```

效果：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d2fc28220d20b73c55e4b136d5fc53d8_1440w.png" style="width: 90%;"/>
</div>





### 计算 appearance 并选择 $k_A$

接下来建模 appearance，过程也是一样，在建模之前，我需要使用 PAT 变换将输入图像对齐到 $s_0$，代码如下：

```python
import skimage.transform
import tqdm

images = []

for image_file in tqdm.tqdm(os.listdir(new_train_images), colour='green', ncols=80):
    if image_file.endswith('.jpg'):   
        image_path = os.path.join(new_train_images, image_file)
        points, _, _ = get_coordinates(image_path)
        if points.shape == (68, 2):
            image = cv2.imread(image_path, 0)
            h, w = image.shape[:2]
            s = np.array([points])
            real_s_0 = denormalise_shape(s_0.reshape(1, -1, 2), s)
            
            source_control_points = real_s_0.reshape(-1, 2)
            target_control_points = s.reshape(-1, 2)
            
            # 进行仿射变换
            affine_trasnform = skimage.transform.PiecewiseAffineTransform()
            affine_trasnform.estimate(src=source_control_points, dst=target_control_points)
            warped_image = skimage.transform.warp(image, affine_trasnform)
                        
            xmax, ymax = source_control_points.max(axis=0).astype(np.int_)
            xmin, ymin = source_control_points.min(axis=0).astype(np.int_)
            images.append(warped_image[ymin: ymax, xmin: xmax])
     
len(images)        
```

仿射变换之后的图像中我找了三张图像可视化了一下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-76e9730db11572b2e5d0f23cd6e3434a_1440w.png" style="width: 90%;"/>
</div>

可以看到，对齐后的图像很好的只保留了息肉部分。

在计算特征值之前，先把之前 shape 的特征值和特征向量存起来，我们只存前三个特征值和特征向量：

```python
model_params = {
    'shape': {
        'base': s_0,
        'eig_values': eig_values.real[:k_s],
        'eig_vecs': eig_vecs.real[:k_s]
    },
    'appearance': {
        'base': None,
        'eig_values': None,
        'eig_vecs': None,
        'beta': None,
        'alpha': None
    }
}
```



为了化为规整的矩阵，我们需要先简单 resize 每一张图像到 50 × 50：

```python
resize_images = []
for image in images:
    h, w = image.shape[:2]
    if h > 50 and w > 50:
        resize_image = cv2.resize(image, (50, 50))
        resize_images.append(resize_image)
resize_images = np.array(resize_images)
```



好，我们现在计算 appearance model，调取我们之前写好的函数即可：

```python
from appearance import appearance_modeling
A_0, normalise_images, eig_values, eig_vecs, beta, alpha = appearance_modeling(resize_images)

# 保存部分数据
model_params['appearance']['base'] = A_0
model_params['appearance']['alpha'] = alpha
model_params['appearance']['beta'] = beta
```

计算 需要保存的 特征值个数： $k_A$

```python
k_A = compute_min_k(eig_values, 0.99)
model_params['appearance']['eig_values'] = eig_values.real[:k_A]
model_params['appearance']['eig_vecs'] = eig_vecs.real[:k_A]
```



简单可视化一下 $A_0$ 和 $A_i$ 都张什么样子：

```python
def visualise_appearance(appearance: np.ndarray, title: str):
    image = appearance.real.reshape(50, 50)
    plt.imshow(image, cmap='gray')
    plt.title(title)
    plt.axis('off')
    
plt.figure(figsize=[25, 5])
plt.subplot(1, 4, 1)
visualise_appearance(A_0.reshape(-1), '$A_0$')
plt.subplot(1, 4, 2)
visualise_appearance(eig_vecs[:, 0], '$A_1$')
plt.subplot(1, 4, 3)
visualise_appearance(eig_vecs[:, 9], '$A_{10}$')
plt.subplot(1, 4, 4)
visualise_appearance(eig_vecs[:, 99], '$A_{100}$')
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e1bc3305e04dec6e0a7d9707c70c8ed1_1440w.png" style="width: 90%;"/>
</div>



### 保存模型

不要忘了保存模型：

```python
np.save('./aam.npy', model_params, allow_pickle=True)
```



### 进行推理

我们先读入一张图像和我们之前保存的模型文件：

```python
test_image_path = os.path.join(new_test_images, 'cju2zi4l09f5807991s8do2b2.jpg')
test_image = cv2.imread(test_image_path)
model_params: dict = np.load('./aam.npy', allow_pickle=True).tolist()
```



为什么说 AAM 很合适制作交互式的分割程序呢？因为从原本的优化式出发，你可以看到，对于每一组参数 $p$ 得到的仿射变换，它会将坐标从 $s_0$ 变换到 $s = s_0 + \sum_{i=1}^{k_s} s_i$ 中，但是变换前后的坐标系是否已知在前文中并没有提到。

为了偷懒（主要是最近实在太忙了），我们简化一下优化模型 $A(x) = A_0(x)$，在这种情况下， $s_0$ 所在坐标系我们是知道的，也就是 $A_0$ 本身。读者感兴趣可以亲自实现 $A(x) = A_0(x)+ \sum_{i}^{k_A} \lambda_i A_i(x)$ 的优化。但是变换之后的坐标系我们不知道，因此需要用户提供。

由于我是直接使用的 `scipy` 中提供的仿射变换，无法直接获取映射矩阵，所以下面采用无梯度方法进行优化。

> 你当然可以把输入图像本身作为一个坐标系，但是这么做你可能需要额外加入两个可学习的参数 $\alpha$ 和 $\beta$ 用于对仿射变换之后的 shape 做一下线性变换。

在这里，我们可以使用 图形界面 或者 手动输入图像的一个不太精准的 bbox，为了省事，我此处直接使用测试图像的 bbox。

```python
bbox = kavsir_meta['cju2zi4l09f5807991s8do2b2']['bbox'][0]
xmin = bbox['xmin']
ymin = bbox['ymin']
xmax = bbox['xmax']
ymax = bbox['ymax']

roi_image = skimage.color.rgb2gray(test_image[ymin: ymax, xmin: xmax, ::-1])
plt.imshow(roi_image, cmap='gray')
```



此处为了有更多时间打星际，我采取了 Nelder-Mead 单纯形算法进行无梯度优化：

```python
import scipy.optimize

def calculate_warp_image(p: np.ndarray):
    estimate_shape = s_0 + p @ s_s
    estimate_shape = shape_shift(estimate_shape, roi_image)
    source_control_points = s_0.reshape(-1, 2)
    target_control_points = estimate_shape.reshape(-1, 2)
    # 进行仿射变换
    affine_trasnform = skimage.transform.PiecewiseAffineTransform()
    affine_trasnform.estimate(src=source_control_points,
                              dst=target_control_points)
    warped_image = skimage.transform.warp(roi_image, affine_trasnform)
    return warped_image


def cost_function(p: np.ndarray):
    warped_image = calculate_warp_image(p)
    h, w = warped_image.shape
    resize_A_0 = cv2.resize(A_0[0], (w, h))
    cost = np.sum((resize_A_0 - warped_image)**2)
    return cost


res_p = scipy.optimize.minimize(cost_function,
                                p_s,
                                method='nelder-mead',
                                options={
                                    'disp': True
                                })

```



可视化一下结果：

```python
warp_image = calculate_warp_image(res_p.x)
_, mask = cv2.threshold(warped_image * 255, 100, 255, cv2.THRESH_BINARY)
kernel = np.ones([5, 5])
mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
plt.imshow(mask, cmap='gray')
```



<div align=center>
<img src="https://pic1.zhimg.com/80/v2-3aec254e5753def5264053c550e03d48_1440w.png" style="width: 40%;"/>
</div>



bingo!

----

## 总结

如此，我们便完成了 AAM 的理论建模到代码实现。

虽然 AAM 是20多年前的方法了，但是其中的建模思想对于如今的一些工作仍然有着深远的影响。比如一些基于图像结构的可解释性工作上，都可以看到 AAM 的影子。如果阁下可以轻松理解 AAM 的理论基础。那么我有理由相信，有朝一日阁下定能成为独当一面的算法工程师。


---

## 参考文献

- [1] Cootes T F, Edwards G J, Taylor C J. Active appearance models[J]. IEEE Transactions on pattern analysis and machine intelligence, 2001, 23(6): 681-685.
- [2] Zhou M, Liang L, Sun J, et al. AAM based face tracking with temporal matching and face segmentation[C]//2010 IEEE Computer Society Conference on Computer Vision and Pattern Recognition. IEEE, 2010: 701-708.
- [3] Pfeifer B, Seger M, Hintermüller C, et al. AAM-based segmentation for imaging cardiac electrophysiology[J]. Methods of Information in Medicine, 2007, 46(01): 36-42.
- [4] Matthews I, Baker S. Active appearance models revisited[J]. International journal of computer vision, 2004, 60: 135-164.
- [5] https://www.doc.ic.ac.uk/~dfg/graphics/graphics2010/GraphicsLecture16.pdf
- [6] https://en.wikipedia.org/wiki/Procrustes_analysis
- [7] https://en.wikipedia.org/wiki/Kabsch_algorithm
- [8] https://www.menpo.org/menpofit/aam.html
- [9] Baker S, Matthews I. Lucas-kanade 20 years on: A unifying framework[J]. International journal of computer vision, 2004, 56: 221-255.