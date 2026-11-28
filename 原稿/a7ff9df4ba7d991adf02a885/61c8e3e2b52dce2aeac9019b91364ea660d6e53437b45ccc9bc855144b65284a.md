## 数学形式

狄利克雷分布（Dirichlet distribution）是一种在概率和统计学中常用的分布，可以看成是 B 分布在 $K\ge 3$ 时的一种推广。我们记 $x = (x_1, x_2, ...,x_K)^T, \alpha = (\alpha_1, \alpha_2, ..., \alpha_K)^T$，其中 $x_i$ 满足
$$
\sum_{i=1}^K x_i = 1 \land \forall i \in \{1,...,K\}, x_i \in [0, 1]
$$
参数为 $\alpha$ Dirichlet 分布记作 $\mathrm{Dir}(\alpha)$ ，它的 PDF 表达式为：
$$
f(x; \alpha) = \frac{1}{B(\alpha)} \prod_{i=1}^K x_i^{\alpha_i - 1}
$$
其中 $B(\alpha)$ 是后面那一项的归一化系数，这样能保证 $f$ 对于 $x$ 的 $K$ 重积分得到的结果是 1。关于 B分布 的更多内容可以移步附录。

---

## 其他记号

在一些学科中，还会用如下记号表示 Dirichlet 分布：
$$
D(x | \alpha)
$$
这个式子可以等价表示为：
$$
x = (x_1, x_2,...,x_K) \sim \mathrm{Dir}(\alpha)
$$

---

## Dirichlet 分布的性质

| 名称              | 公式                                                         | 备注                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 长度 Length       | $\displaystyle\sum_{i=1}^K \alpha_i$                         | 记作 $\alpha_0$                                              |
| 均值 Mean         | $E[x_i] = \frac{\alpha_i}{\alpha_0}$                         | 记作 $\tilde{\alpha}_i$                                      |
| 方差 Variance     | $\mathrm{Var}[x_i] = \frac{\tilde{\alpha}_i (1 - \tilde{\alpha}_i)}{\alpha_0 + 1}$ |                                                              |
| 协方差 Covariance | $\mathrm{Cov}[x_i, x_j] = \frac{\delta_{ij} \tilde{\alpha}_i - \tilde{\alpha}_i \tilde{\alpha}_j}{\alpha_0 + 1}$ | $\delta_{ij}$ 是 Kronecker 算符，$i=j$ 时为 1，否则为 0      |
| 模点 Mode         | $x_i = \frac{\alpha_i - 1}{\alpha_0 - K}$                    | $\alpha_i > 1$                                               |
| 熵 Entropy        | $H(x) = \log B(\alpha) + (\alpha_0 - K) \psi(\alpha_0) - \displaystyle\sum_{j=1}^K(a_j - 1)\psi(\alpha_j)$ | $\psi(\cdot)$ 是双伽马函数，$\psi(x) =\frac{d}{dx} \ln{\Gamma(x)}= \frac{\Gamma'(x)}{\Gamma(x)}$ |

---

## 直观理解

Dirichlet分布 中的随机向量 $x$ 满足和为1，且都是正数，这刚好与状态空间为 $K$ 离散随机变量的输出值不谋而合，所以，Dirichlet分布 可以用来刻画 **K维离散随机变量** 的概率分布，即 **概率分布的概率分布**，更加确切的说法可以称为二阶概率。

比如深度学习分类器的输出层 softmax 后如果有 $M$ 个节点，那么可以用参数为 $\alpha\in \mathbb R^M$ 的 Dirichlet分布 来描述这 M 个概率值的分布，从而做出更加有趣的玩具。

如果是二分类，一般会输出两个概率值，分别代表 【类别一】 和 【类别二】 的概率，此时用于描述这两个分类概率的概率分布的 Dirichlet分布 退化为 B分布。

---

##  程序实现

计算机内部往往使用如下近似式来实现 Dirichlet分布 的PDF（Robert H. Windschitl在2002年提出的方法）：
$$
\Gamma(z) \approx \sqrt{\frac{2 \pi}{z} } \left( \frac{z}{e} \sqrt{ z \sinh \frac{1}{z}  + \frac{1}{810z^6} } \right)^{z}
$$

---

## 参考文献

- https://en.wikipedia.org/wiki/Dirichlet_distribution

---

## 附录

通过 [B 分布](https://en.wikipedia.org/wiki/Beta_function) 的推广，可以写出 $B(\alpha)$ 的闭式解：
$$
B(\alpha) = \frac{\displaystyle\prod _{i=1}^K\Gamma (\alpha_i)}{\Gamma\left(\displaystyle\prod_{i=1}^K \alpha_i\right)}
$$
其中 $\Gamma$ 是 [gamma 函数](https://en.wikipedia.org/wiki/Gamma_function)，原本只是一个 $\mathbb N^+ \mapsto \mathbb N^+$ 的映射：
$$
\Gamma(n) = (n-1)!
$$
但是对于 B 分布中的 $\alpha$， 可通过解析延拓定义出一个 $C  \mapsto C, \Re(z) > 0$：
$$
\Gamma(z) = \int_0^\infty t^{z- 1} e^{-t} dz
$$
或者基于无穷积分表示：
$$
\Gamma(z) = \frac{1}{z} \prod_{n=1}^\infty \left( 1 + \frac{z}{n} \right) ^{-1} \left(1 + \frac1n\right) ^ z
$$
如果这些无法理解（毕竟不是所有人都是数学系的，比如我），那么只需要知道 $B(\alpha)$ 是一个存在闭式解的归一化系数即可。