## 问题背景

最近一个做 AI4Science 的哥们问了我一个问题：给定 $B$ 组数据 $X_i\in\mathbb R^{m\times n}$（也就是 m 个 n 维的样本，一共有 $B$ 组这样的样本）。这些数据都是用 `torch.tensor` 包裹着的，很明显，这是一个 `shape=[B, m, n]` 的 tensor。

现在定义 $X_i$ 的“正态分布重采样” $X_i'$ 。 $X_i'$ 的 shape 和 $X_i$ 一样，且 $X_i'$ 的每一个行向量都采样自一个多元正态分布 $\mathcal N(\mu, \Sigma)$。其中正态分布的 $\mu$ 是 $X_i$ 的样本均值，$\Sigma$ 是$X_i$ 的样本离差阵。

> 这涉及到正态总体的无偏估计，不记得的同学请出门左拐重修概率论（狗头）

我朋友的问题就是输入一个 $B\times m \times n$ 的 tensor（第一个维度是 batch），返回一个 shape 完全相同的 tensor，返回的 tensor 的第 $i$ 个 batch 是输入 tensor 第 $i$ 个 batch 的正态分布重采样。

## 问题分析

样本均值和样本离差阵都可以通过 `torch` 的内置方法，比如按列求和快速获得，那么问题就划归为，在 pytorch 中，如何快速从一个均值和协方差矩阵已知的多元正态分布中采样多个向量？

## torch 的内置多元正态分布模块 distributions

其实 pytorch 本身就提供了生成多元正态分布的模块。假设我们需要采样的正态分布为：

$$
\mathcal N(\mu, \Sigma), \mu = \begin{pmatrix} 1\\2\\0 \end{pmatrix}, \Sigma = \begin{pmatrix} 2 & 0 & 0 \\ 0 & 5 & 0 \\ 0 & 0 & 1 \end{pmatrix}
$$

先用 torch 表示一下：

```python
mu = torch.FloatTensor([1, 2, 0])
sigma = torch.FloatTensor([
    [2, 0, 0],
    [0, 5, 0],
    [0, 0, 1]
])
```

那么假设我们需要从中采样100000个样本，那么代码如下：

```python
sampler = torch.distributions.MultivariateNormal(
    loc=mu, covariance_matrix=sigma
)
samples: torch.Tensor = sampler.sample((100000, ))
# samples.shape [100000, 3]
```

我们可以简单验证一下这个是否正确，只需要重新计算采样的均值和协方差在小幅度内是否和总体的相等即可：

```python
new_mu = samples.mean(dim=0)
new_sigma = (samples - mu).T @ (samples - mu) / len(samples)

print(new_mu.round())
print(new_sigma.round())
```

输出：

```python
tensor([1., 2., 0.])
tensor([[2., 0., -0.],
        [0., 5., 0.],
        [-0., 0., 1.]])
```

很完美，说明这个内置采样函数是没问题的。而且锦恢测量了这个采样函数的运算效率，效率非常高，10万个点从建立分布再到采样，只花了10 ms，性能非常优秀。

但是这个做法有一个缺点，那就是不够自由，因为每次实例化这个类只能生成一个正态分布的采样，而在我朋友的例子里面，我们需要同时生成 B
个正态分布的 m 组采样。使用这个方法，就不可避免地需要进行 for 循环，在 B 比较大时，类的初始化和销毁也是一笔不小的开销，那么有没有更加优雅的方法呢？

---

## 换一个角度思考

我们不妨换个角度思考这个问题， pytorch 可以基于 `torch.randn` 和 `torch.randn_like` 来生成任意维度的正态分布。

既然要生成的是一个任意尺度的多元正态分布，基于小学知识，我们知道，任意多元正态分布都是标准正态分布的线性变换，**原则上，如果我们可以获得标准正态分布到目标多元正态分布的线性变换**，那么这个问题就迎刃而解了。

简单回顾一下小学知识，如果分布 $X\sim \mathcal N_m(\mu, \Sigma)$，分布 $Y = AX + b, A \in \mathbb R^{n\times m}, b \in \mathbb R^{n}$，那么 $Y$ 也服从正态分布：

$$
Y \sim \mathcal N_n(A \mu + b, A \Sigma A^T)
$$

因为 torch 能快速拿到任意尺寸的标准正态分布，且标准正态分布的各个维度独立，此时$\mu = 0, \Sigma = I_m$，我们把它们带入上面的结论可得：

$$
Y \sim \mathcal N_n(b, AA^T)
$$


因此，我们就得到了 $\mu_Y = b, \Sigma_Y = AA^T$。

回顾一下我们上面的问题，上述的变量中，$\mu_Y$ 就是我们的样本均值，$\Sigma_Y$ 就是我们的样本离差阵，这两个值我们是知道的，$X$ 是可以通过 `torch.randn` 获得。我们的目标一直就是获得线性变换，也就是 $A, b$，只要能获得 $A, b$，我们就可以将 `torch.randn` 的生成结果进行线性变换得到 $N_n(\mu_Y, \Sigma_Y)$ 的采样。

我们已经知道 $b = \mu_Y, AA^T = \Sigma_Y$，显然， $b$ 已经知道了，那么如何获取 $A$ 呢？相信学过线性代数的朋友会迫不及待地说：对 $\Sigma_Y$ cholesky 分解！是的！这样，我们就得到了任意尺度的多元正态分布。

### 基于线性变换的多元正态分布生成算法

简单总结一下步骤：

输入：样本均值 $\mu \in \mathbb R^{m}$，样本离差阵 $\Sigma \in \mathbb R^{m\times m}$，采样数量 $n$.

输出：$\mathcal N_m(\mu, \Sigma)$ 中的 n 个采样结果 $Y \in \mathbb R^{n\times m}$.

处理：
1. $b \Leftarrow \mu$
2. $A \Leftarrow \mathrm{cholesky}(\Sigma)$
3. 使用 `torch.randn` 生成 $n\times m$ 的正态矩阵 $X$
4. $Y \Leftarrow A X + b$
5. 返回 $Y$

我们不妨通过编程的方法来验证这个算法的正确性(mu和sigma还是上面的那两个，此处就不初始化了)：

```python
b = mu
A = torch.linalg.cholesky(sigma)
X = torch.randn((10000, 3))
samples = X @ A.T + b
```

同理，验证一下正确性：

```python
new_mu = samples.mean(dim=0)
new_sigma = (samples - new_mu).T @ (samples - new_mu) / len(samples)

print(new_mu.round())
print(new_sigma.round())
```

输出：
```python
tensor([1., 2., 0.])
tensor([[2., -0., 0.],
        [-0., 5., 0.],
        [0., 0., 1.]])
```

看起来完全没问题！说明我们的方法没有问题。

### 拓展到多个多元正态分布的生成

此时，有些读者就显得不耐烦了：你这个算法看起来和 torch 的内置算法看起来差不多呀，不是也只能一次性生成一个吗？但是请阁下不要着急（后面有得你急的bushi），我们的这个算法中只有矩阵算法，我们将多元正态分布的生成从一个类的实例化转化到了矩阵运算，这意味着什么？这意味着我们可以利用循环向量化的思想，将 for 循环 + `torch.distributions.MultivariateNormal` 的写法基于 torch 的 GEMM 实现。这里引入一个热知识：

torch.matmul 是实现矩阵乘法的函数，但是如果输入的 tensor 是三维的，比如 [B, m, k] 和 [B, k, n]，那么 torch.matmul 的输出结果是会忽略第一个维度，输出 [B, m, n] 的 tensor，利用这个特性，我们就可以在不写 for 循环和额外类实例化的情况下几行实现我朋友的需求。

### 基于线性变换的多元正态分布生成 代码实现

为了模拟我朋友的问题，我暂时用 sklearn 内置的 iris 数据集进行演示。我们将 iris 分割为5份，组成一个 [5, 30, 4] 的 tensor：

```python
import torch
import numpy as np
from sklearn.datasets import load_iris

# 构造一个 5 * 30 * 4 的三阶张量
X, y = load_iris(return_X_y=True)
sample_num, feat_num = X.shape
batch_size = 5
three_d_tensor = X.reshape(batch_size, sample_num // batch_size, feat_num)
three_d_tensor = torch.from_numpy(three_d_tensor)
```

然后计算每个 batch 的正态分布重采样，也就是返回一个 [5, 30, 4] 的 tensor。这里我们实现最核心的多个多元正态并行生成算法 `make_normal_along_batch`：

```python
def make_normal_along_batch(tensors: torch.FloatTensor) -> torch.FloatTensor:
    assert len(tensors.shape) == 3
    sample_n = tensors.shape[1]
    mus = torch.mean(tensors, dim=1).unsqueeze(dim=1)
    nor_tensors = tensors - mus
    cov = torch.matmul(nor_tensors.permute(0, 2, 1), nor_tensors) / sample_n
    A: torch.FloatTensor = torch.linalg.cholesky(cov)
    X = torch.randn_like(tensors)
    Y = torch.matmul(X, A.permute(0, 2, 1)) + mus
    return Y
```

最后测试一下结果：

```
normals = make_normal_along_batch(three_d_tensor)
print(normals.shape)
```

输出：（感兴趣的读者可以自行验证结果的数值正确性）

```
torch.Size([5, 30, 4])
```

至此，我们完成了这个任务，可以进行下一个任务了。