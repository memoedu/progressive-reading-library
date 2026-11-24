为了加快我们梯度下降或者其他凸优化理论的运行效率，我们有必要对我们学习的算法进行优化。这些优化更加复杂。

## 常用的优化算法

- 梯度下降法（Gradient descent）
- 共轭梯度法（Conjugate gradient）
- 变尺度法（BFGS）
- 限变尺度法（L-BFGS）

最后三种优化方法更加复杂，但是却带来了一些好处：

- 不需要人工挑选学习率。（No need to manually pick alpha）
- 通常比梯度下降法更快。（Often faster than gradient descent）