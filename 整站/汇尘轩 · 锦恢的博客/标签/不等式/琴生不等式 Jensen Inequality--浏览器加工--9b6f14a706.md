## 前言

琴生不等式 （Jensen Inequality）是一组根据函数凹凸性给出不等性的重要公式。

对于一组给定的实数 $\lambda_i (i=1,2,...,n)$，满足$\lambda_i > 0\land \sum_{i=1}^n \lambda_i=1$，如果 函数 $f: \mathbb R \to \mathbb R$ 是下凸函数，则

$$
f(\sum_{i=1}^n \lambda_i x_i) \le \sum_{i=1}^n \lambda_i f(x_i)
$$

如果 $f$ 是上凸函数，则：

$$
f(\sum_{i=1}^n \lambda_i x_i) \ge \sum_{i=1}^n \lambda_i f(x_i)
$$

等号成立时 $x_i$ 为常数，或者 $\lambda_i$ 中仅仅存在一个值为1，其他都是0（即 $\vec{\lambda}$ 为独热向量，或者 $\vec{\lambda}$ 是 $\mathbb R^n$ 的一个平凡基）


> 这组概念能拓展到 $f: \mathbb R^n \to \mathbb R$ 上，此时 $\sum_{i=1}^n \lambda_i x_i$ 描述了一组 $x_i$ 圈出的 $n- 1$ 阶单纯形的内部区域，此文不再赘述。

---

## 证明

基于第一类数学归纳法就能证明 Jensen 不等式。为了简易，此处只证明 $f$ 是下凸函数时。

① $n=2$ 时，由下凸函数的定义，可以得到：

$$
f(\lambda_1 x_1 + \lambda_2 x_2) \le \lambda_1 f(x_1) + \lambda_2 f(x_2) \quad \lambda_1 + \lambda_2 = 1
$$

② $n=k, k\ge 3$ 时，此时有：
$$
f(\sum_{i=1}^k \lambda_i x_i) \le \sum_{i=1}^k \lambda_i f(x_i)\quad \sum_{i=1}^k\lambda_i = 1
$$
当 $n=k+1$ 时，此时有 $\sum_{i=1}^{k+1}\lambda_i = 1$。考察 $f(\displaystyle\sum_{i=1}^{k+1} \lambda_i x_i)$：
$$
\begin{aligned}
f(\sum_{i=1}^{k+1} \lambda_i x_i) &=f(\lambda_{k+1}x_{k+1} + (1 - \lambda_{k+1})\sum_{i=1}^k \frac{\lambda_i}{1 - \lambda_{k+1}}x_i) \\
&\le \lambda_{k+1}f(x_{k+1}) + (1 - \lambda_{k+1})f(\sum_{i=1}^{k}\frac{\lambda_i}{1 - \lambda_{k+1}}x_i) \quad\quad(n=2)\\
&\le \lambda_{k+1}f(x_{k+1}) + (1 - \lambda_{k+1})\sum_{i=1}^{k}\frac{\lambda_i}{1 - \lambda_{k+1}}f(x_i) \quad\quad(n=k) \\
&= \sum_{i=1}^{k+1}\lambda_i f(x_i)
\end{aligned}
$$
所以 $n=k+1$ 时也成立，证毕。



---

## 应用

一些在物理学和统计学中的二级结论可以基于 Jensen 不等式轻松推导而出。

### KL 散度的非负性

信息论中的 KL 散度被定义为 
$$
D(p \vert\vert q) = \mathbb E_{p}\left[ \log\frac{p(x)}{q(x)} \right] = \sum_{x} p(x) \log \frac{p(x)}{q(x)}
$$
其中 $p,q$ 是关于 $x$ 的概率密度函数，满足 $\sum_x p(x) = \sum_x q(x) = 1$. Jensen 不等式可用于证明它的非负性：

设 $f(x) = \log x$，显然，$f$ 是上凸函数，所以有：
$$
\begin{aligned}
D(p||q) &= \sum_{x} p(x) \log \frac{p(x)}{q(x)} \\
&= - \sum_{x} p(x) \log \frac{q(x)}{p(x)} \\
& \ge -\log \left( \sum_x p(x) \frac{q(x)}{p(x)} \right) 

\\
&= - \log (\sum_x q(x)) = 0
\end{aligned}
$$



### Renyi 散度的极限

Renyi 散度被定义为：
$$
D_\alpha(P\vert\vert Q) = \frac{1}{\alpha - 1} \ln \left(\sum_{i=1}^n p_i^{\alpha}q_i^{1-\alpha} \right)\quad \alpha \ge 0
$$
其中 $p_i$ 和 $q_i$ 分别为分布 $P,Q$ 的 PMF。现在证明： $D_1(P||Q) = \displaystyle\sum_{i=1}^n p_i \ln \frac{p_i}{q_i}$.

利用 Jensen 不等式：

$$
\begin{aligned}
D_\alpha (P || Q) &= \frac{1}{\alpha - 1} \ln \left(\sum_{i=1}^n p_i \left(\frac{p_i}{q_i}\right)^{\alpha - 1} \right) \\
&\ge \frac{1}{\alpha - 1} \sum_{i=1}^n p_i \ln \left(\frac{p_i}{q_i}\right)^{\alpha - 1} \\
&= \sum_{i=1}^n p_i \ln \frac{p_i}{q_i}
\end{aligned}
$$
当 $\alpha \to 1$ 时，$\left(\frac{p_i}{q_i}\right)^{\alpha - 1} \to 1$ 是常数，此时 Jensen 不等式等号成立。所以：
$$
D_1(P||Q) = \lim_{\alpha \to 1} D_\alpha (P||Q) = \displaystyle\sum_{i=1}^n p_i \ln \frac{p_i}{q_i}
$$




### 基本不等式：代数平均数-几何平均数

在基本不等式中，代数平均数-几何平均数 （AM-GM 公式）表示为：
$$
\frac{1}{n} \sum_{i=1}^n a_i \ge \sqrt[n]{\prod_{i=1}^n a_i}
$$


证明：
$$
\frac{1}{n} \sum_{i=1}^n a_i \ge \sqrt[n]{\prod_{i=1}^n a_i} \Leftrightarrow \ln \frac{\sum_{i=1}^n a_i}{n} \ge \frac{1}{n} \sum_{i=1}^n \ln a_i
$$
因为 $f(x) = \ln x$ 是上凸函数，所以：
$$
f(\sum_{i=1}^n \lambda_i x_i) \ge \sum_{i=1}^n \lambda_i f(x_i)
$$
取 $\lambda_i = \frac{1}{n}$ 即可得证。

### 统计物理学

在统计物理学 (statistical physics)，有一个重要的结论，它与误差分析有关：
$$
\exp \mathbb E[X] \le \mathbb E[\exp(X)]
$$
如果 $f(x)$ 是下凸函数，那么：
$$
f(\mathbb E[X]) = f(\sum_x xp(x) ) \le \sum_x p(x) f(x) = \mathbb E[f(x)]
$$
取 $f(x) = \exp(x)$ 得证。