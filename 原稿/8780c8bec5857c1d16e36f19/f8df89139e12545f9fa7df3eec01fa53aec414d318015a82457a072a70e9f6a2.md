## 介绍

**神经网络（Neural Network）**实际上一种古老的算法，亦在模仿人类大脑处理问题，实现能力的功能。起源于上个世纪70s~80s，并于90s逐渐消失。在计算机算力步入新阶段的21世纪，神经网络重新登上历史的舞台，成为了如今机器学习中最先进的**非线性假设（Non_linear Hypotheses）**之一。我们不清楚神经网络的内部实现，但是它却可以解决许多用多项式回归模型难以解决的问题。

## 模型表示

### 逻辑单元（Logistic unit）

为了更好地架构神经网络，我们可以把整个网络想象成一个个最小的单元。我们将将神经网络模拟成一个个**最小逻辑单元**。

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220528205101.png" style="width: 80%;"/>
</div>



当然了，图中除了输入节点$x_1,x_2,x_3$之外，还可以增加一个输入值恒为1的节点$x_0$，作用和线性回归中的偏置项作用类似，因此我们将节点x0称为**偏置单元（bias unit）**或**偏置神经元（bias neuron）**。因为$x_0$固定输入1，所以有的图中会画出，有的不会。

每一个圈都称作**节点（node）**。其中最后的那个圈（橘黄色的）内部就是一个函数，在神经网络术语中会常常看到**激励函数（activation function）**，指的就是那个和输出端相连的橘黄色小圈内装的函数。实际上这个函数和我们之前讨论的Logistic回归中的g(z)是一样的，我们称之为**s激励函数（Sigmoid/Logistic activation function）**：
$$
g(z)=\frac{1}{1+e^{-z}}
$$
还有需要说明的是在神经网络中，**权重（weights）**和**参数（parameters）**是指一个东西，不过在神经网络中，权重一词用得更多。



### 神经网络（Neural Network）

我们所谈的神经网络，实际上就是一系列逻辑单元的组合。

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220528205240.png" style="width: 80%;"/>
</div>



每一层的偏置神经元没画出。

需要说明的是，第一层因为接受输入，所以被称为**输入层（Input layer）**；最后一层负责将网络的运算结果输出，所以被称为**输出层（Output layer）**；其余中间的所有层都成为**隐层（Hidden layer）**。事实上，隐层不是一个很好的名字，这一点在界内饱受诟病，但可以这么理解：在运算中我们只能看到自己的输入和神经网络的输出，中间的过程我们一无所知，所以称之为隐层。

#### 符号约定

为了更好地解释神经网络的运算过程，有几个记号需要约定：

- $a_i^{(j)}$：第j层的第i个单元的激活值
- $\Theta^{(j)}$：第j层到第j+1层映射的参数矩阵



以上面那张三层的神经网络为例，它整体的运算过程如下：（x0和a0是没有画出的偏置神经元）
$$
a_1^{(2)}=g(\Theta_{10}^{(1)}x_0+\Theta_{11}^{(1)}x_1+\Theta_{12}^{(2)}x_2+\Theta_{13}^{(1)}x_3)\\
a_2^{(2)}=g(\Theta_{20}^{(1)}x_0+\Theta_{21}^{(1)}x_1+\Theta_{22}^{(2)}x_2+\Theta_{23}^{(1)}x_3)\\
a_3^{(2)}=g(\Theta_{30}^{(1)}x_0+\Theta_{31}^{(1)}x_1+\Theta_{32}^{(2)}x_3+\Theta_{33}^{(1)}x_3)\\
h_\Theta(x)=a_1^{(3)}=g(\Theta_{10}^{(2)}x_0+\Theta_{11}^{(2)}x_1+\Theta_{12}^{(2)}x_2+\Theta_{13}^{(2)}x_3)
$$
在这个例子中，我们的Theta（1）应该是一个3*4的矩阵，第一层到第二层做的事也就是将权重矩阵Theta（1）左乘输入神经元构成的列向量，即：
$$
\begin{bmatrix}
a_1^{(2)}\\
a_2^{(2)}\\
a_3^{(2)}\\
\end{bmatrix}=g(
\begin{bmatrix}
\Theta_{10}^{(1)}&\Theta_{11}^{(1)}&\Theta_{12}^{(1)}&\Theta_{13}^{(1)}\\
\Theta_{20}^{(2)}&\Theta_{21}^{(2)}&\Theta_{22}^{(2)}&\Theta_{23}^{(2)}\\
\Theta_{30}^{(3)}&\Theta_{31}^{(3)}&\Theta_{32}^{(3)}&\Theta_{33}^{(3)}\\
\end{bmatrix}
\begin{bmatrix}
x_0\\
x_1\\
x_2\\
x_3\\
\end{bmatrix})
$$
由线性代数的知识，我们知道：Theta(1)的维度是由前一层的神经元数（包括偏置神经元）和后一层的神经元数（不包括偏置神经元）共同决定的：

如果网络在第$j$层有$s_j$个单元，在$j+1$层有$s_{j+1}$个单元，那么$\Theta^{(j)}$的形状就是$s_{j+1}\times (s_j+1)$

我们继续定义一些额外的项，我们将第一层值$x_0,x_1,x_2,x_3$的线性组合记作$z$
$$
\Theta_{10}^{(1)}x_0+\Theta_{11}^{(1)}x_1+\Theta_{12}^{(2)}x_2+\Theta_{13}^{(1)}x_3):=z_1^{(2)}\\
\Theta_{20}^{(1)}x_0+\Theta_{21}^{(1)}x_1+\Theta_{22}^{(2)}x_2+\Theta_{23}^{(1)}x_3):=z_2^{(2)}\\
\Theta_{30}^{(1)}x_0+\Theta_{31}^{(1)}x_1+\Theta_{32}^{(2)}x_3+\Theta_{33}^{(1)}x_3):=z_3^{(2)}
$$
于是，我们有
$$
a_1^{(2)}=g(z_1^{(2)})\\
a_2^{(2)}=g(z_2^{(2)})\\
a_3^{(3)}=g(z_3^{(2)})
$$
于是我们可以把上面的式子向量化。记：
$$
\Theta^{(1)}=\begin{bmatrix}
\Theta_{10}^{(1)}&\Theta_{11}^{(1)}&\Theta_{12}^{(1)}&\Theta_{13}^{(1)}\\
\Theta_{20}^{(2)}&\Theta_{21}^{(2)}&\Theta_{22}^{(2)}&\Theta_{23}^{(2)}\\
\Theta_{30}^{(3)}&\Theta_{31}^{(3)}&\Theta_{32}^{(3)}&\Theta_{33}^{(3)}\\
\end{bmatrix}\\
x=\begin{bmatrix}
x_0\\
x_1\\
x_2\\
x_3\\
\end{bmatrix},
z^{(2)} =\begin{bmatrix}
z_1^{(2)} \\
z_2^{(2)} \\
z_3^{(2)} \\
\end{bmatrix},
a^{(2)} =\begin{bmatrix}
a_1^{(2)}\\
a_2^{(2)}\\
a_3^{(2)}\\
\end{bmatrix}
$$
那么Layer1->Layer2的计算过程就可以简写成如下形式：
$$
z^{(2)}=\Theta^{(1)}x\\
a^{(2)}=g(z^{(2)})
$$
有时候为了保持形式的一致性，第一层（输入层）的x向量也会写成a，即：
$$
a^{(1)}=x=
\begin{bmatrix}
x_0\\
x_1\\
x_2\\
x_3\\
\end{bmatrix}
$$
然后第二层向最后一层输出时，由于增加了一个偏置神经元，所以我们要对a(2)向量扩增:
$$
Add\quad a_0^{(2)}=1,so\quad a^{(2)} =\begin{bmatrix}
a_0^{(2)}\\
a_1^{(2)}\\
a_2^{(2)}\\
a_3^{(2)}\\
\end{bmatrix}\\
$$
然后就可以吧Layer2->Layer3的过程简写成：
$$
z^{(3)}=\Theta^{(2)}a^{(2)}\\
h_\Theta(x)=a^{(3)}=g(z^{3})
$$
这个从输入层到最终计算出h（x）的值的过程也称为**前向传播（Forward propagation）**。如果我们吧Layer1遮掉，会发现Layer2->Layer3的前向传播在形式上和标准的Logistic回归没有区别。所以为了区分两者，神经网络中的权重都用大写的theta来表示。

#### 简单神经网络实现布尔运算

为了更好的展示神经网络的大致工作方式，我们以最简单的逻辑单元来实现布尔运算。

**与运算（AND）**

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220528205533.png" style="width: 50%;"/>
</div>

**或运算（OR）**

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220528205733.png" style="width: 50%;"/>
</div>

类似的我们可以通过AND和OR的组合得到XNOR



## 神经网络中的分类问题

### 符号约定

![image.png](https://cdn.uminsi.com/images/2cfb771f-670f-4ea4-9152-ade976479df3.png)

比如在这个模型中就有$L=4,s_1=3,s_2=5,s_3=5,s_4=4$。注意，我们的$s_i$没有考虑第 $i$ 层的偏置神经元。我们还记$K=4$，其中的K可以看作输出层的输出单元数量。

我们会讨论两种问题：**二分类（Binary classification）**和**多分类（Multi-class classification）**。



### 二分类和多分类的特点

下面说明二分类与多分类在神经网络中的特点

——|二分类|多分类（K分类）
:-:|:-:|:-:
 标签（y） |标签值只能为0或1|标签值为K维向量，一般为K阶单位向量的列向量组
 输出层 |只有一个输出单元|有K个输出单元

对于二分类问题，有如下显而易见的式子：
$$
h_\Theta(x)\in\R,\ S_L=1,\ K=1
$$
对于多分类问题，我们一般不用y=0,1,2这样的数来当标签，而是以列向量。比如在物品识别时，我们将每个物品的标签设置成一个列向量：
$$
\begin{bmatrix}
1\\
0\\
0\\
0\\
\end{bmatrix}
\text{represents apple}, 
\begin{bmatrix}
0\\
1\\
0\\
0\\
\end{bmatrix}
\text{represents banana},
\begin{bmatrix}
0\\
0\\
1\\
0\\
\end{bmatrix}
\text{represents pear},
\begin{bmatrix}
1\\
0\\
0\\
0\\
\end{bmatrix}
\text{represents peach}
$$
对于多分类问题，有如下显而易见的式子：
$$
y\in \R^K,\ h_\Theta(x)\in\R^K,\ S_L=K(K\ge3)
$$



### 神经网络中的损失函数

对于我们的Logistic回归，我们有如下的损失函数：（正则化后的）
$$
J(\theta)=-\frac{1}{m}\left[\sum_{i=1}^{m}y^{(i)}\log h_\theta(x^{(i)}) +(1-y^{(i)})\log(1-h_\theta(x^{(i)})) \right]+\frac{\lambda}{2m}\sum_{j=1}^{n}\theta_j^2
$$
对于我们的神经网络，考虑到标签y不是一个标量，且不止一个输出，我们有如下记号：
$$
(h_\Theta(x))_i=a_i^{(K)}=i^{th}output\\
y_j^{(i)}：第i个样本的标签列向量的第j个值
$$
考虑整个神经网络，我们将神经网络的损失函数定义如下：
$$
J(\Theta)=-\frac{1}{m}\left[\sum_{i=1}^{m}\sum_{k=1}^{K}y_k^{(i)}\log(h_\Theta(x^{(i)}))_k+(1-y_k^{(i)})\log(1-(h_\Theta(x^{(i)}))_k)   \right]\\
+\frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_{l+1}}(\Theta_{ji}^{(l)})^2\qquad\qquad\qquad\qquad\qquad\qquad\qquad
$$
其中第一项是标准的损失项，它用两个求和符号：对于每一个样本，它的标签值是一个K维的列向量；前向传播后输出层的K个节点输出的K个值，即可以看作输出了一个K维列向量，则内层的求和符号就是求了第 i 个样本的输出向量和标签向量对应值的Logistic误差，并将这K个对应求得误差值相加；外层求和就是将m个样本的误差值求和。

第二项就是正则化项，它负责将所有的非偏置神经元的对应的权重的平方和相加。



### 反向传播算法（Back propagation algorithm）

既然我们得到了神经网络的损失函数，那么怎么使得这个损失函数最小化呢？这里就需要提到神经网络中和前向传播对应的一个词：**反向传播（Back propagation）**。

为了使用梯度下降，我们需要计算出损失函数对每个权重的偏导，即：
$$
\frac{\partial}{\partial\Theta_{ij}^{(l)}}J(\Theta)
$$

> 下面所做就是为了求出偏导数项。

我们先以一个样本为例，看看整个神经网络的情况。并且按照之前我们约定的符号，前向计算就是这样的：

![image.png](https://cdn.uminsi.com/images/d63b9459-7390-453a-b2e1-364049ce45ce.png)

我们下面做的就是称为**反向传播算法（Back propagation algorithm）**的过程。我们需要考察每一层的每一个参数对整体损失函数的影响。

我们记$\delta_j^{(l)}$为第$l$层节点$j$的误差（损失）

之所以叫反向传播是因为，我们要根据误差调整参数时，明显是先调整输出层的参数，毕竟那一块的参数是距离我们结果最近的地方，然后逐一向前调整。调整的顺序呈现从后往前，故得名反向传播。既然这样那么我们先考察输出层的delta（还是上面那个四层的神经网络）
$$
\delta_j^{(4)}=a_j^{(4)}-y_j(a_j^{(4)}=(h_\Theta(x))_j)
$$
接着我们定义Layer3和Layer2的delta：
$$
\delta^{(3)}=(\Theta^{(3)})^T\delta^{(4)}.*g'(z^{(3)})\\
\delta^{(2)}=(\Theta^{(2)})^T\delta^{(3)}.*g'(z^{(2)})\\
$$
其中“.*”和MATLAB中的一样，意思是两个同形矩阵或向量对应项相乘（和Numpy中的ndarray对象直接做四则运算效果一样）。我们到Layer2时，反向传播就结束了，因为第一层是输入层，输入不可能有任何误差，也是不能改动的。

不加证明地，我们给出以下结论：
$$
\frac{\partial}{\partial\Theta_{ij^{(l)}}}J(\Theta)=a_j^{(l)}\delta_{i}^{(l+1)}
$$
于是，我们有了如下的反向传播算法：

![image.png](https://cdn.uminsi.com/images/8459800a-2487-4099-a176-ab30e8929973.png)

最后一步的累加也可以写成向量式：
$$
\Delta^{(l)}:=\Delta^{(l)}+\delta^{(l+1)}(a^{(l)})^T
$$
接着

![image.png](https://cdn.uminsi.com/images/7836e598-0a32-4bc3-b562-9dce26ab8700.png)

然后对每一个权重使用梯度下降就行了。

> 吴恩达老师说证明太复杂，自己用了这么多年也没有对反向传播有深刻的理解（大佬在谦虚），所以笔者就不写证明过程了(｀・ω・´)

> 不行！理解反向传播过程及其精妙之处，对于理解、实现、设计和调试神经网络非常**关键**。必须明白推导过程，请移步https://blog.csdn.net/qq_32865355/article/details/80260212查看。（写得非常好(｀・ω・´)）



### 数值梯度检验

之前所讲的反向传播算法并不直观，怎样才能确保我们得反向传播在执行的过程中没有出错呢？我们可以使用**数值梯度检验（Numerical estimation of gradients）** 来对绝大多数情况来检测。

先看如下的图像。

![image.png](https://cdn.uminsi.com/images/613828fe-cc1b-495c-af70-149f4e19db3c.png)

根据拉格朗日中值定理，我们可以用**双侧差分（Two-sided difference）**来对theta点处的斜率来做近似：
$$
\frac{d}{d\theta}J(\theta)\approx\frac{J(\theta+\epsilon)-J(\theta-\epsilon)}{2\epsilon}
$$

- 注意是使用双侧差分，而不是单侧，因为双侧更有可能体现出反向传播过程中的错误。
- epsilon取值不能太大也不能太小，一般取值为10^-4左右。

所以我们可以把Theta权重矩阵按行展开成一系列行向量，然后对行向量中的每一个theta使用双侧差分，来看看得到的结果与损失函数对这个theta的偏导数之差是不是在一个很小的数之间。在的话，就说明反向传播得到的梯度矩阵中的这个权重没问题。
$$
\theta\in\R^n\qquad(\text{E.g. } \theta \quad \text{is "unrolled" version of}
\Theta^{(1)},\Theta^{(2)},\Theta^{(3)})\\\\
\theta=[\theta_1,\theta_2,\theta_3,\dots,\theta_n]\qquad\qquad\qquad\ \ \ \ \\\\
\frac{\partial}{\partial\theta_1}J(\theta)\approx
\frac{J(\theta_1+\epsilon,\theta_2,\theta_3,\dots,\theta_n)-J(\theta_1-\epsilon,\theta_2,\theta_3,\dots,\theta_n)}{2\epsilon}\\
\frac{\partial}{\partial\theta_2}J(\theta)\approx
\frac{J(\theta_1,\theta_2+\epsilon,\theta_3,\dots,\theta_n)-J(\theta_1,\theta_2-\epsilon,\theta_3,\dots,\theta_n)}{2\epsilon}\\
\dots\dots\\
\dots\dots\\
\dots\dots\\
\frac{\partial}{\partial\theta_n}J(\theta)\approx
\frac{J(\theta_1,\theta_2,\theta_3,\dots,\theta_n+\epsilon)-J(\theta_1,\theta_2,\theta_3,\dots,\theta_n-\epsilon)}{2\epsilon}\\
$$

所以我们就可以写成如下简单的octave代码：（同上，theta是上面的theta）
```octave
for i = 1:n,
	thetaPlus = theta;
	thetaPlus(i) = thetaPlus(i)+EPSILON;
	thetaMinus = theta;
	thetaMinus(i) = thetaMinus(i) - EPSILON
	gradApprox(i) = (J(thetaPlus) - J(thetaMinus)) / (2 * EPSILON)

% Check that gradApprox \approx DVec(Form back propagation)
```

**实施梯度检测的小结：**

- 实施反向传播，计算DVec（$D^{(1)},D^{(2)},D^{(3)}$的展开）。
- 实施数值梯度检测，计算gradApprox。
- 确保DVec和gradApprox的值相似。
- 禁用梯度检测。继续执行反向传播代码学习。

**注意**

- 确保在训练分类器之前将梯度检测代码禁用。如果你在梯度下降的每一次迭代中都运行数值梯度检测（或是在计算损失函数的内部循环中），那么你的代码会运行地非常慢。



### 随机初始化

神经网络本质就是根据数据集不断更新自身权重的模型，既然要更新权重，那么权重势必要有一个初始值，那么初始值怎么来呢？最方便的方法就是用随机数。切记不可以将权重全部设置成0或相同的数字，否则会导致神经网络的对称化，因为每一层接受的数据都是一样的，即整个神经网络功能上相当于一个神经元在工作。

所以为了打破可能出现的对称性，我们会选择一个关于0对称的区间随机取数赋给权重。

---

# 参考

- [[中英字幕\]吴恩达机器学习系列课程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV164411b7dx?p=45)