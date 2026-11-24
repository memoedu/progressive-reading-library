## 均方误差（mean squared error）

回归任务中最常用的性能度量方式，对于我们的假设空间h_\theta(x)和标签y，其均方误差为：
$$
E(h;D)=\frac{1}{N}\sum_{i}^{N}(h_\theta(x_i)-y_i)^2
$$
对于更加一般的数据分布D和概率密度分布p(·)，均方误差可描述为：
$$
E(h;D)=\int_{x\sim D}(h_{\theta}(x)-y)^2p(x)dx
$$





## 对数几率回归损失函数（logistics regression cost function）

为了避免在分类任务中出现非凸函数的损失函数（这样很那收敛到全局最小值），引入**logistics regression cost function**：
$$
Cost(h_\theta(x),y)=\begin{cases}
-\log(h_\theta(x))&if\ y=1\\
-log(1-h_\theta(x))&if\ y=0
\end{cases}
$$
图像如下：

<img src="https://cdn.uminsi.com/images/4ace9157-0ea5-4f67-9468-323aa058bc43.png" alt="image.png" style="zoom:80%;" />

很明显y=1时，如果h也为0，那就是毫无误差，我们的Cost就应是0，这个满足；y=1时，h若偏离1，接近0，说明预测出现了明显的误差，我们就要给与分类器一些“惩罚”，也就是增大它的Cost值，你离1越远，Cost越大；y=0时也是同理。所以简答分析下，这个误差函数是合理的。

咱们耍点小花招，把**logistics regression cost function**这个分段函数写成一个函数：
$$
Cost(h_\theta(x),y)=-y\log(h_\theta(x))-(1-y)\log(1-h_\theta(x))
$$
所以总的误差是：
$$
J(\theta)=\frac{1}{N}\sum_{i=1}^{N}Cost(h_\theta(x),y)\\
=-\frac{1}{N}\sum_{i=1}^{N}(y\log(h_\theta(x))+(1-y)\log(1-h_\theta(x)))
$$
由此我们得到了对数几率回归损失函数：
$$
J(\theta)
=-\frac{1}{N}\sum_{i=1}^{N}(y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)})))\
$$
>  实际上，这个式子是可以由统计学中的**极大似然法（maximum likelihood method）**推导来的。

那么如果我们想要将总的损失函数值降到最小，我们可以对上述对数几率损失函数的每一个权重项求偏导，为了方便展示，我们只对求和号中的似然项求偏导：
$$
设l(\theta)=(y\log(h_\theta(x))+(1-y)\log(1-h_\theta(x)))\\
\frac{\partial l(\theta)}{\partial w_j}=\frac{y}{h_\theta(x)}\frac{\partial h_\theta(x)}{\partial w_j}-\frac{1-y}{1-h_\theta(x)}\frac{\partial h_\theta(x)}{\partial w_j}\\
=\frac{ye^{-\theta^TX}}{1+e^{-\theta^TX}}x_j-\frac{1-y}{1+e^{-\theta^TX}}x_j\\
=(y-\frac{1}{1+e^{-\theta^TX}})x_j\\=
\boxed{-(h_\theta(x)-y)x_j}
$$
我们带回原式求和（注意那个符号），我们就可以得到第j个权重的梯度了。你会惊奇地发现，它和线性回归求得的梯度值在形式上完全一致！这也是我们选择Sigmond函数的原因之一：
$$
\frac{\partial J(\theta)}{\partial \theta_j}=\frac{1}{N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
$$
所以我们的迭代步骤在形式上也和线性回归的迭代完全一致：
$$
Want\ \ min_\theta J(\theta)\\
Repeat\{\\
\theta_j:=\theta_j-\alpha\frac{1}{N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}\\
(simultaneously\ update\ all\ \theta_j)
\\
\}
$$
乍一看，这线性回归没啥不同但是其中的假设函数发生了很大的不同：
$$
in\ linear\ regression:
h_\theta(x)=\theta^TX\\
in\ logistic\ regression:
h_\theta(x)=\frac{1}{1+e^{-\theta^TX}}
$$