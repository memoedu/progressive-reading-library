## 介绍

对偶问题（Dual Problem）是最优化方法中的一种常用技巧。在学习对偶化之前必须先了解 Lagrange 乘子法。比如有如下的优化式子：
$$
\min_{x} f(x)\\ \text{s.t.}\quad c(x) \le 0
$$

> 请注意不等号的方向。在约束优化方法中，一般而言，我们习惯使用小于等于而非大于等于

其中 $c(x) \in \mathbb R^n$ 是 n 个约束，它们可以是等式约束（记作 $\mathcal E$），也可以是不等式约束（记作 $\mathcal I$）。此处的不等号是逐个元素比较的不等号。总之在对偶化技巧的语境下，我们往往将这组约束优化式称为 **原问题（Primal Problem）**。

解决原问题的一个常用手段就是使用 Lagrange 乘子法：
$$
\min_{x,\lambda} L(x, \lambda) = f(x) + \lambda^T c(x)
$$
不同于等式约束，含有不等式约束的 Lagrange 乘子（multipliers）需要满足 KKT 条件：

| KKT 条件                           | 公式                                                         | 备注                                                         |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 平稳性（Stationarity）             | $\exists \tilde{\lambda},\quad \nabla_x L(x^\*, \tilde{\lambda}) = \nabla f(x^*) + \tilde{\lambda}^T c(x) = 0$ | $x^*$ 是一个原问题上满足约束的局部极小值                     |
| 原问题可行性（Prime feasibility）  | $\forall i \in \mathcal I, c_i(x) \le 0.\quad\forall j \in \mathcal E, c_j(x) = 0$ | 局部极小值当然也需要满足原问题的约束                         |
| 对偶问题可行性（Dual feasibility） | $\forall i \in \mathcal I,\lambda_i \ge 0$                   | 存在的 乘子 需要满足对偶问题中的假设                         |
| 互补性（Complementary）            | $\forall i \in \mathcal I, \lambda_i c_i(x) = 0$             | 对于 $\mathcal E$ 中的项本来就满足 $\lambda_i c_i(x) = 0$，所以这个约束会让 Lagrange 的结果和原问题尽可能一致 |

<br>

既然在 KKT 条件中提到了 对偶问题可行性，那不好好使用怎么行？

---

## 问题建模

如果原文的 Lagrange 乘子 $\lambda \ge 0$，考虑到 $c(x) \le 0$ 那么我们可以得到如下的推导：
$$
L(x, \lambda) = f(x) + \lambda ^T c(x) \le f(x)
$$
我们此时再对 $\lambda$ 进行优化：
$$
L(x, \lambda) \ge \min_x L(x, \lambda) := g(\lambda)
$$
我们就能得到一个非常有趣的夹逼公式：
$$
g(\lambda) \le L(x, \lambda) \le f(x)
$$
此处的 $L(x, \lambda)$ 就像 $g(\lambda)$ 与 $f(x)$ 这两个单变量函数的上界和下界，$g(\lambda)$ 的最大值不大于 $L(x, \lambda)$，$f(x)$ 的最小值不小于 $L(x, \lambda)$。也就是说在 $c(x)$ 的约束下（需要证明），下式满足：
$$
\left . \max_{\lambda} g(\lambda) \le L(x, \lambda) \le \min_x f(x) \right|_{c(x) \le 0, \lambda \ge 0} \\
$$
所以很自然有下面的转换：
$$
\begin{aligned}
\min_{x} f(x)\\ \text{s.t.}\quad c(x) \le 0
\end{aligned}
\quad
\Rightarrow
\quad
\begin{aligned}
\max_\lambda g(\lambda)\\
\text{s.t.} \quad \lambda \ge 0
\end{aligned}
$$
左侧的就是原问题，右侧就是原问题的 **对偶问题（Dual Problem）**。

> 为什么不求解 $\min g(\lambda)$ 呢？因为没有意义， $f(x)$ 也无法在 $c(x)$ 的约束下取到。而上面的推导中 $\max g(\lambda)$ 是一个能够求到的下界。

很明显，如果上面求对偶问题时的 $x$ 不满足 $c(x) \le 0$，那么得到的 $g(\lambda)$ 就有可能会比 $f(x)$ 大。虽然得到的结果不是 $f(x)$ 的最小值，但是能保证 $g(\lambda)$导出的最小值 $f(x)$ 可以取到。

---

## 求解原问题的对偶问题

有了上面的认识，想要求解对偶问题就非常简单了，

第一步：先找出原问题：
$$
\min_{x} f(x)\\ \text{s.t.}\quad c(x) \le 0
$$
第二步：写出 Lagrange 乘子式：
$$
\min_{x,\lambda} L(x, \lambda) = f(x) + \lambda^T c(x)
$$
第三步：求解对偶问题：
$$
g(\lambda) = \min_x L(x,\lambda)
$$
第四步：列出对偶问题
$$
\max_{\lambda} g(\lambda) \\
\text{s.t.}\quad \lambda \ge 0
$$

---

## 可视化理解——关键在鞍点

可能很多观众对于对偶化问题仍然感觉抽象和陌生，那么我们不妨举个例子来看看对偶化在干什么吧，假设我们的问题为：
$$
\min_x x^2 \\
\text{s.t.} \quad x + 1 \le 0
$$
Lagrange 式为：
$$
L(x, \lambda) = x^2 + \lambda(x + 1)
$$
对偶问题为：
$$
\max_\lambda \min_x L(x, \lambda) = \max_\lambda -\frac{1}{4} \lambda ^2 + \lambda\\
\text{s.t.} \quad \lambda \ge 0
$$
需要注意，对偶问题有一个隐藏条件：$x = -\frac{1}{2}\lambda$.

我们不妨在 $\mathbb R^3$ 中把上面的三个函数画出来：（动画代码在文章末的附录里面）

<div align=center>
<img src="https://picx.zhimg.com/80/v2-69e26f196f76a00179a7b5d3c8c793c4_1440w.png" style="width: 70%;"/>
</div>

通过计算，我们可以得到原问题的最优解为 $x = -1$；对偶问题的最优解为 $x=-1, \lambda=2$。我们来看看这两个点和上面图的关系。

可以看到，在 Lagrange 乘子法下，原问题的求解维度被扩充了，也就是蓝色的曲面在 $\lambda \ge 0$ 的部分。我们按住 `d`，然后移动光标来换一个角度看一下，你会发现，对偶问题的最优解确实是橘黄色曲线的最大值处，如下图红色圈圈框住的地方：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d45a4730450e1bb9e6ca745037025a1f_1440w.png" style="width: 70%;"/>
</div>

同时，蓝色曲面在这个点的梯度也刚好是 0。你会发现，虽然这个点的梯度为0，但是这个点却不是曲面的极小值点，而是曲面的一个鞍点。


倘若知道了这个现象之后，其实我们对于 对偶问题 和 原问题 的关系就应该有点感觉了：原问题就是途经鞍点的开口向上的曲线，那么对偶问题就是和原问题的曲线相交的开口向下的曲线。感性地认识就是，原问题和对偶问题与曲线的鞍点有关，且是走向完全相反的两条曲线。

因此，在讨论 Lagrange 乘子法和 对偶问题 的本质或者说几何理解时，我们通常会通过曲面的鞍点来给学生们直观的感受（希望我以后开课能用上这些动画和图像吧）。

阁下不妨使用我在附录中的代码，尝试自己观察这个曲线的走向，并且更换使用其他的曲线方程和约束条件来看看有什么不同。



---

## 支持向量机 SVM

如果知道对偶化的流程后就能瞬间手撕 SVM 的推导，我们不妨重温一下 SVM。

SVM 试图寻找一个最鲁棒的超平面将样本空间切开，超平面的方程通常写为：
$$
w^Tx + b = 0
$$
假设样本空间只有两个类（记为 1 和 -1），我们使用不等式来作为分类依据：
$$
\begin{cases}
w^T x_i + b \ge 1 &\Leftrightarrow y_i = 1\\
w^T x_i + b \le -1 &\Leftrightarrow y_i = -1
\end{cases}
$$
距离超平面最近的异类点的距离为（上面两个不等式取等号后代表的两个超平面到 $w^Tx + b = 0$ 距离的和）：
$$
margin = \frac{||b - 1 - b||}{||w||} + \frac{||b + 1 - b||}{||w||} = \frac{2}{||w||}
$$
SVM 就是要最大化这个 margin，安全距离越大，越鲁棒嘛：
$$
\max_{w, b} \frac{2}{||w||}
$$
当然，判别得到的约束条件也不能少，上面的约束条件式等价于：
$$
y_i(w^T x_i + b) \ge 1
$$
合在一起，优化式再分子分母调转后加个平方（好求导）可以得到 SVM 的优化式（原问题）：
$$
\begin{aligned}
&\min_{w,b}\frac12||w||^2 \\
&\text{s.t.}\quad y_i(w^T x_i + b) \ge 1, \quad i=1,2,...,m
\end{aligned}
$$
我们下面尝试对上面的式子进行对偶化，首先写出 Lagrange 乘子式：
$$
L(w, b, \alpha) = \frac{1}{2} ||w||^2 + \sum_{i=1}^m \alpha_i (1 - y_i(w^Tx_i + b))
$$
然后求解对偶问题：
$$
\begin{aligned}
\nabla_w L(w, b, \alpha) = 0 &\Rightarrow \hat{w} = \sum_{i=1}^m \alpha_i y_i x_i \\
\nabla_b L(w, b, \alpha) = 0 &\Rightarrow 0 = \sum_{i=1}^m \alpha_i y_i \\
\Downarrow \\\\
g(\alpha) = \min_{w, b} L(w, b, \alpha) &= L(\hat{w}, \hat{b}, \alpha) = \sum_{i=1}^m \alpha_i - \frac{1}{2}\sum_{i=1}^m \sum_{j=1}^m \alpha_i\alpha_j y_iy_j x_i^T x_j
\end{aligned}
$$
整理得到对偶问题：
$$
\begin{aligned}
&\max_{\alpha}  \sum_{i=1}^m \alpha_i - \frac{1}{2}\sum_{i=1}^m \sum_{j=1}^m \alpha_i\alpha_j y_iy_j x_i^T x_j\\
&\text{s.t.}\quad \sum_{i=1}^m \alpha_i y_i = 0 \land \alpha_i \ge 0\quad i = 1,2,...,m
\end{aligned}
$$
优化上面的式子可以得到一组 $\alpha$，带入原本的式子就可以得到 SVM 分类器：
$$
f(x) = w^T x + b = \sum_{i=1}^m \alpha_i y_i x_i^T x + b
$$
上面的各项参数还需要满足 KKT 条件。

这就是 SVM 了，是不是非常简单？事实上，SVM 最精彩的地方并不是这部分的推导，而是后续的核方法的求解优化（在低维空间求解高维空间的问题）。但是因为篇幅有限，此处就不再赘述了。

---

## 什么时候对偶化，对偶化有什么好处

可以看到，通过对偶化，我们不仅可以解决约束优化问题了，而且将优化的决策变量从 $x$ 变成了 $\lambda$ 。那么什么时候需要对偶化，对偶化一定利于问题求解吗？

> 我还记得当年被问到的一道面试题：什么情况下进行对偶化会提升 SVM 的性能，什么时候会下降？

一个非常直观的感受就是，如果对偶化的问题比原问题好解决（原问题复杂到几乎解不了），显然做对偶是有价值的。我能想到的情况一共有两种。

### 对偶化后函数的凹凸性更好了

第一种情况是，如果原问题是非凸优化，而对偶问题是凸优化，显然，这个时候进行对偶是有意义的。这会让问题更好被优化，且一定达到全局极小值。

### 对偶化后维度降低了

第二种情况是，对偶化后的变量 $\lambda$ 的维度比原问题的变量 $x$ 维度低很多。$\lambda$ 的维度由原问题的约束条件的数量决定，而 $x$ 的维度则代表了数据的特征数。在 SVM 中，约束的条件数和样本点的数量有关。所以阁下可以简单理解为，如果样本点的特征远远大于样本数量（小样本问题），那么此时显然 $\lambda$ 的维度会比特征维度小，此时进行对偶化可以降低问题求解的复杂度。

如果数据本身的样本数远远大于特征，进行对偶化会让优化的变量的维度变高，这个时候不一定对偶化就是一定能提升 SVM 的求解速度。

---

## 附录

可视化代码 `main.py`。manimgl 安装教程：https://zhuanlan.zhihu.com/p/378999796

运行 `manimgl .\main.py ThreeDPlot`：

```python
from manimlib import *

class ThreeDPlot(ThreeDScene):
    def construct(self):
        self.camera.frame.set_euler_angles(
            theta=50 * DEGREES,
            phi=45 * DEGREES,
        )
        
        self.camera.frame.set_focal_distance(12)
        
        # 显示坐标轴
        axes = ThreeDAxes()
        self.play(ShowCreation(axes))

        x_axis = Tex('x')
        y_axis = Tex('\lambda')
        
        x_axis.move_to(np.array([3, -1, 0]))
        y_axis.move_to(np.array([1, -3, 0]))

        
        self.play(
            FadeIn(x_axis),
            FadeIn(y_axis),
        )

        
        # 定义曲线函数
        def lagrange(x, l):
            return x ** 2 + l * (x + 1)

        # 创建 3D 曲线
        surface = ParametricSurface(
            lambda u, v: [u, v, lagrange(u, v)],
            u_range=[-3, 3],
            v_range=[-3, 3],
            resolution=(100, 100),
        )

        # 设置曲线样式
        surface.set_opacity(0.7)
        surface.set_color(BLUE)
        
        surface_function = Tex('L(x, \lambda) = x ^ 2 + \lambda (x + 1)', font_size=30)
        surface_function.set_color(BLUE)
        surface_function.fix_in_frame()
        surface_function.move_to(LEFT_SIDE)
        surface_function.shift(RIGHT * 2)
        # 将曲线添加到场景中
        self.play(ShowCreation(surface))
        self.play(ShowCreation(surface_function))


        def prime(t):
            x = t
            y = 0
            z = t * t
            return np.array([x, y, z])

        # 创建二维曲线
        curve = ParametricCurve(prime, t_range=[-3, 3], color=RED)
        curve_function = Tex('f(x) = x ^ 2 \quad x + 1 \le 0', font_size=30)
        curve_function.set_color(RED)
        curve_function.fix_in_frame()
        curve_function.move_to(surface_function, aligned_edge=LEFT)
        curve_function.shift(DOWN)
        self.play(ShowCreation(curve_function))
        
        self.play(ShowCreation(curve))


        def dual(t):
            x = 0
            y = t
            z = -0.25 * t * t + t
            return np.array([x, y, z])

        # 创建二维曲线
        curve = ParametricCurve(dual, t_range=[-3, 3], color=ORANGE)
        dual_curve_function = Tex('g(\lambda) = -\\frac{1}{4} \lambda ^ 2 + \lambda', font_size=30)
        dual_curve_function.set_color(ORANGE)
        dual_curve_function.fix_in_frame()
        dual_curve_function.move_to(curve_function, aligned_edge=LEFT)
        dual_curve_function.shift(DOWN)
        self.play(ShowCreation(dual_curve_function))
        
        # 显示二维曲线
        self.play(ShowCreation(curve))
```