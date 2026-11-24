EvNet 损失函数：

$$
\mathcal L(\theta) = \sum_{i=1}^N \mathcal L_i(\theta) +\lambda_t \sum_{i=1}^N \mathrm{KL}\left(D(p_i|\tilde{\alpha}_i) || D(p_i | \bold 1)\right)
$$


$\mathcal L_i(\theta)$ 的三种配置：

$$
\begin{aligned}
\text{负对数似然} \quad \mathcal L_i(\theta) &=\sum_{j=1}^K y_{ij}  \left(\log S_i - \log \alpha_{ij}\right) \\
\text{交叉熵} \quad \mathcal L_i(\theta) &=\sum_{j=1}^K y_{ij}  \left(\psi(S_i) - \psi(\alpha_{ij})\right) \\
\text{均方误差} \quad \mathcal L_i(\theta) &=\sum_{j=1}^K (y_{ij} - \hat{p}_{ij})^2 + \frac{\hat{p}_{ij} (1 - \hat{p}_{ij})}{(S_i + 1)} \\
\end{aligned}
$$

## Loss 增益

找到决策边界旁边的样本点，增加它们的损失值的权重，或者给它们的决策边界周围的点更大的 KL 散度来增加不确定性。

尝试使用正向KL和反向KL。

$u$ 建模如下：

$$
u + \sum_{i=1}^K b_i = 1
$$

通过归一化得到上面的值：

$$
b_k = \frac{e_k}{S}, \quad u = \frac{K}{S}, S = \sum_{i=1}^K(e_i + 1)
$$

原本的 EvNet 没有考虑高阶概率，而是直接使用了最简单的均匀分布，全1进行了建模，考虑如何修改这个1，使得损失函数更加合理。

## 数据增强策略

mixup 策略不应该作用于 label 不同的样本上 (×)

对抗样本？

## Evidence 冗余去除

evidence 冗余去除 $e = e - \min_i e_i$ 如何进行改进
- 进行进一步的数学推导
- 随机进行冗余去除操作
- 参考 PostNet

## TODO

现在的实验中，OOD 检测中是假阳多还是假阴多？