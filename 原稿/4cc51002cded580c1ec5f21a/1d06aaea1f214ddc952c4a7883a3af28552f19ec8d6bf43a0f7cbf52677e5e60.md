## 问题的产生

当我们构建模型时，总会希望假设空间参数尽可能多，系统越复杂，拟合得越好嘛！我们还希望我们的优化算法能使我们的模型产生的损失函数的值尽可能小（即我们的假设空间能够贴合每一个训练样本点）。但这样真的好吗？奥卡姆剃刀貌似又胜利了，以为假设我们的模型达成了上述的情况，有很大概率产生一个ML界非常令人头疼的一件事：**过拟合（Overfitting）**。

什么是过拟合呢？举一个简单的例子：我们设计了一个模型来判断一件物品是否为树叶。喂养这个模型的数据集中含有几张带有尖刺边缘的树叶。模型的设计者希望模型能满足每一个训练数据，模型就将尖刺边缘也纳入了参数中。当我们测试这个模型的泛化性能时，就会发现效果很差，因为模型钻牛角尖，它认为树叶必须带有尖刺边缘，所以它排除了所有没有带有尖刺边缘的树叶，但事实上，我们知道树叶并不一定带有尖刺边缘。结果为什么会这样？因为模型设计者的强迫症使得这个模型过分贴合整个训练集，结果把噪音点也拟合上了。

## 欠拟合与过拟合

我们以“波士顿房价预测”为例，来从图像的角度直观感受一下什么叫过拟合，顺便也介绍一下欠拟合：

### 例子1：线性回归（房价预测）

<div align=center>
  <img src="https://pic2.zhimg.com/80/v2-b4ccd2c27516e24531f956a85296f6b5_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> Fig.1 Linear regression</p>
</div>



我们看到第一张图的模型过于简单，而且损失函数的收敛速度很慢。这就使得优化算法做得再好，我们的模型的泛化性能也会很差，因为这条直线在训练集上的cost就很大，我们把这种训练集上的偏差很大的情况叫做**欠拟合（Underfitting）**，也有一种历史叫法：**高偏差（High bias）**。

第二张图是我们合理假设的一个模型。可以看到，选取了合理的模型后，图像大致穿过了样本点。像极了做物理实验时，最后用一条曲线大致地穿过既定的样本点；和第一张图比起来，至少损失值大大下降了。

第三张图引入了高次项，就题论题来说，这太复杂了。虽然事实上对于是任意n个点，总能找到n+1次曲线方程将这n个点全部穿过，但是从模型的角度来讲，这显然不是好模型（姑且不说要用一个高次模型去拟合离散点要迭代多少次，学习率要调到多低）。虽然它一板一眼地穿过了所有样本点，但谁能保证这种奇形怪状的曲线能穿过下一个随机给出的样本点呢？我们把这种和预测值和样本标签值几乎完全一致的情况叫做**过拟合（Overfitting）**，历史上也称为**高方差（High variance）**。

我们再看看分类问题中的过拟合。

### 例子2：Logistic回归

<div align=center>
  <img src="https://pic3.zhimg.com/80/v2-1c8011da5472155536e34205a7b0a5de_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> Fig2.Logistic regression </p>
</div>



三幅图哪个更好呢？不多说，第二张图应该是合理的划分方式，而不是像第三张图那样一板一眼。

## 定义

我们给出过拟合的定义：

> **Overfitting** : If we have too many features, the learned hypothesis may fit the training set vey well,  but fail to generalize to new examples.

其中的'fit the training set very well'的数学语言是：
$$
\frac{1}{2N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})\approx0(or=0)
$$
所以过拟合就是损失函数极小但泛化性能差的情况。这也说明了我们在训练模型时，损失函数关于迭代次数的图像一直下降到很小的数值并不是什么好事，这恰恰暗示了我们的模型存在过拟合的风险。

## 解决过拟合

我们一般有两种方法来减小过拟合的影响：

1.减少属性值（特征值）的数量。

- 人工选择哪些特征需要保留。
- 使用模型选择算法。

2.实行正则化

- 保留所有特征值，但是减小参数θ_j的值或数量级。
- 当我们有许多特征时，效果较好。其中每一个特征值都会对y造成影响。

### 正则化（Regularization）

#### 基本思路

下面的几张图是对房价问题实际构建了神经网络后得到的结果：（是Fig1的精装版）

<div align=center>
  <img src="https://pic4.zhimg.com/80/v2-5490b142354424273b2c302ed048404b_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> 假设空间为一次函数，学习率=0.01，迭代1000次 </p>
</div>

<div align=center>
  <img src="https://pic1.zhimg.com/80/v2-595e79db115a8d46caca8a7473d48d08_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> 假设空间为二次函数，学习率=0.0001，迭代1000次 </p>
</div>

<div align=center>
  <img src="https://pic3.zhimg.com/80/v2-d2924ec9516eacc7a840c752b3d2422a_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> 假设空间为四次函数，学习率=0.0000001，迭代10000次 </p>
</div>



对于刚才线性回归的第三张图，为了使它演化到第二张图，我们其实只要把三次项和四次项扔掉就行了，对吧？怎么个扔法呢？实际上，我们只要对参数3和参数4实行一定的惩罚就行了。我们考察这个损失函数（1000只是随手写的一个比较大的数字）：
$$
\min_{\theta}\frac{1}{2N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})^2+1000\theta_3^2+1000\theta_4^2
$$
由于1000很大，这就使得为了使损失函数降到最小，我们不得不把参数3和参数4压缩到很小的数字，即：
$$
\theta_3\approx0,\theta_4\approx0
$$
这也就相当于去掉了这两个参数。更一般的，给出正则化后面的想法。我们都默认这样一个事实：对于多项式：
$$
f(x)=a_0+\lambda(\sum_{i=1}^{n}a_i x^i)
$$
λ越小，函数图像越平坦（可以自己画图试试）。也可以这么想：λ=0时，图像是一条平行于x轴的直线，绝对是“最平坦”的了。类似极限的“保序性”，那么λ往0靠近时，图像也应该是平坦的。那么将这个效果施加在我们预测房价的第二张图上，就可以使得那条歪歪曲曲的多项式曲线平坦拉伸，拉成类似于第二张图的样子。

因此，我们可以重新定义损失函数：
$$
J(\theta)=\frac{1}{2N}\left[\sum_{i=1}^{N}(h_\theta(x^{(i)}-y^{(i)})^2+\lambda\sum_{j=1}^{m}\theta_j^2\right]\\\min_\theta J(\theta)
$$
其中我们把多出来的那一项称为**正则化项**，lambda称为**正则化系数**。

下面我们简单讨论λ的取值。如果λ太小的话，形同虚设，继续过拟合；如果λ太大的话，那么除了常量参数θ_0外的其余所有参数都会被压缩到0，最终这就是一条平行于x轴的直线，很明显会造成欠拟合。所以λ的取值不能大，也不能小。如果我们取了一个恰当的值（比如0.001）我们原本的蓝线就会变成红线，虽然不是真正的二次曲线，但是过拟合的痕迹被压缩小了。

所以，我们就得到了含有正则表达式的梯度下降：
$$
\begin{align}
for \quad j=1,2,\dots,m \quad& Repeat\\
&\theta_0:=\theta_0-\alpha\frac{1}{N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})x_0^{(i)}\\
&\theta_j:=\theta_j-\alpha\frac{1}{N}\left[\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}+\lambda\theta_j\right] 
\end{align}
$$
当然，第二行也可以写成：
$$
\theta_j:=(1-\frac{\alpha\lambda}{N})\theta_j-\alpha\frac{1}{N}\sum_{i=1}^{N}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}
$$
那么我们更新梯度的时候，只要在原来的基础上把右式的系数改成这样就行了。**总的效果是使得我们的四次曲线更加的平坦、柔和，这样做可以防止曲线收到噪声点的干扰过于严重**。我们将以上式子带入，得到以下代码，尝试使用：

#### 代码

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.patches as pat

plt.figure(dpi=150,edgecolor="yellow")

class Network(object):
    def __init__(self,weights_num):
        np.random.seed(0)
        self.w = np.array([[ 2.5],
       [-0.1138],
       [-0.0409],
       [ 0.0038]])
        self.b = -0.42
        self.lam = 10500 # 正则化系数

    def forward(self,x):
        z = np.dot(x,self.w) + self.b
        return z

    def loss(self,z,y):
        return np.mean((z - y)**2)

    def gradient(self,x,y):
        z = self.forward(x)
        gradient_w = np.mean((z - y) * x,axis=0)
        gradient_w.resize(4,1)
        gradient_b = np.mean(z - y)
        return gradient_w,gradient_b

    def update(self, gradient_w, gradient_b, eta=0.01):  # 梯度下降，学习率默认0.01
        self.w -= eta * gradient_w  # 往梯度相反的方向走
        self.b -= eta * gradient_b

    def train(self, x, y, iteration=100, eta=0.01):  # 训练函数，默认迭代100次
        losses = []  # 采集每次迭代的数据，用来绘制图像
        for i in range(iteration):
            z = self.forward(x)
            L = self.loss(z, y)
            gradient_w, gradient_b = self.gradient(x, y)
            self.update(gradient_w, gradient_b, eta)
            losses.append(L)
            if (i + 1) % 100 == 0:  # 每十个数据打印一下情况
                print("iter {} times,loss value is {}".format(i, L))
        return self.w,self.b,losses

    def re_loss(self,z,y): #正则化下的损失函数
        cost1 = np.mean((z - y) ** 2)
        cost2 = np.mean(self.w ** 2)
        return cost1 + cost2

    def re_update(self,gradient_w, gradient_b, eta=0.01): # 正则化下的梯度下降更新参数
        self.w =  (1 - eta * self.lam / 5) * self.w - eta * gradient_w
        self.b -= eta * gradient_b

    def re_train(self, x, y, iteration=100, eta=0.01):  # 正则化训练函数，默认迭代100次
        re_losses = []  # 采集每次迭代的数据，用来绘制图像
        for i in range(iteration):
            z = self.forward(x)
            L = self.re_loss(z, y)
            gradient_w, gradient_b = self.gradient(x, y)
            self.re_update(gradient_w, gradient_b, eta)
            re_losses.append(L)
            if (i + 1) % 100 == 0:  # 每十个数据打印一下情况
                print("iter {} times,loss value is {}".format(i, L))
        return self.w, self.b, re_losses


def Draw_sample_point(x,y): # 画出样本点
    plt.grid(True)
    plt.xlabel("Size of house", fontsize=15)
    plt.ylabel("Price", fontsize=15)
    #plt.text(4, 1, r'$\theta_0+\theta_1 x+\theta_2 x^2$''\n'r'$+\theta_3 x^3+\theta_4 x^4$', fontsize=15)
    plt.plot(x, y, 'rx')
    plt.ylim(0, 10)
    plt.xlim(0, 10)

def Draw_fitted_curve(w,b,color="b"): # 画出拟合后的曲线
    plt.grid(True)
    x = np.arange(0.2, 9.8, 0.01)
    y = b + w[0][0] * x + w[1][0] * x ** 2 + w[2][0] * x ** 3 + w[3][0] * x ** 4
    plt.plot(x, y,color)

def Draw_loss_function(iteration_time,losses,color="b"): # 画出损失函数的值关于迭代次数的函数图像
    plt.grid(True)
    plot_x = np.arange(iteration_time)
    plot_y = np.array(losses)
    plt.xlabel("times of iterations", fontsize=15)
    plt.ylabel("values of loss function", fontsize=15)
    plt.plot(plot_x, plot_y,color)

def get_data(x): # 获取四次函数的数据[x,x^2,x^3,x^4]
    x1 = x.tolist()
    x2 = (x ** 2).tolist()
    x3 = (x ** 3).tolist()
    x4 = (x ** 4).tolist()
    x_new = []
    for i in range(len(x)):
        roll = []
        roll.append(x1[i])
        roll.append(x2[i])
        roll.append(x3[i])
        roll.append(x4[i])
        x_new.append(roll)
    return np.array(x_new)

x = np.array([1,2,3.5,6,8])
y = np.array([2,4,6,7,7.5])
plt.subplot(121)
Draw_sample_point(x,y)
x = get_data(x)
y.resize(5,1)

net = Network(4)
iteration_time = 1000 #迭代次数
w,b,losses = net.train(x,y,iteration = iteration_time,eta = 0.0000001) #
print("Done")
re_w,re_b,re_losses = net.re_train(x,y,iteration = iteration_time,eta = 0.0000001)
print("w is{},b is {}\nre_w is {},re_b is {}".format(w,b,re_w,re_b))

blue_patch = pat.Patch(color="blue")
green_patch = pat.Patch(color="green")
Draw_fitted_curve(w,b,"b")
Draw_fitted_curve(re_w,re_b,"g")
plt.legend(handles=[blue_patch,green_patch],labels=["Original curve","Regularization"])

plt.subplot(122)
Draw_loss_function(iteration_time,losses,"b")
Draw_loss_function(iteration_time,re_losses,"g")
plt.legend(loc = "upper left",handles=[blue_patch,green_patch],labels=["Original curve","Regularization"])

plt.show()
```

#### 运行结果

<div align=center>
  <img src="https://pic4.zhimg.com/80/v2-13145fe0e8d64e3b19a961affbf29de7_720w.jpg" style="width: 80%;"/>
  <p class="img-caption"> 四次函数拟合离散点正则化尝试。正则化系数=10500，学习率=0.0000001 </p>
</div>


可以看到我们的正则化后的四次曲线虽然偏离了样本点，但是却变得更加平坦了。正如上面所论述的一样，更加平坦的曲线拥有更加好的泛用性能。当然，笔者手残，参数调不好，最理想的正则化情况应该是介于二次曲线和四次曲线之间的状态，参照这个模型可以接着调整。（其实这个例子不是特别好，因为正则化得作用就是使得高次项参数变小。但是这个例子中，各个次项得系数本身就很小，所以才导致结果不太明显）

#### *详细定义

以下是机器学习中正则化比较详细的定义：

> 正则化是结构风险（损失函数+正则化项）最小化策略的体现，是在经验风险（平均损失函数）上加一个正则化项。正则化的作用就是选择经验风险和模型复杂度同时较小的模型。
>
> 防止过拟合的原理：正则化项一般是模型复杂度的单调递增函数，而经验风险负责最小化误差，使模型偏差尽可能小经验风险越小，模型越复杂，正则化项的值越大。要使正则化项也很小，那么模型复杂程度受到限制，因此就能有效地防止过拟合。

正则化一般具有如下形式的优化目标：
$$
\min_{f\in F}\left[\frac{1}{N}\sum_{i=1}^{N}L(y_i,f(x_i))+\lambda J(f)   \right]
$$
其中，$\lambda \ge 0$是用来平衡正则化项和经验风险的系数。正则化项可以是模型参数向量的范数，经常用的有$L_1$数，$L_2$：
$$
L_1范数：||x||_1=\sum_{i=1}^{N}|x_i|，L_2范数：||x||_2=\sqrt{\sum_{i=1}^{N}x_i^2}
$$
我们考虑最简单的线性回归模型：
$$
给定数据集：D=\{(x_i,y_i)\}_{i=1}^N，其中，x_i=(x_{i1},x_{i2},\dots,x_{id}),y_i\in\R
$$

$$
代价函数为：J(w)=\frac{1}{N}||y-w^TX||^2=\frac{1}{N}\sum_{i=1}^{N}(y_i-w^Tx_i)^2
$$

##### （1）L2范数正则化（Ridge Regression，岭回归）

代价函数为：
$$
J(w)=\frac{1}{N}\sum_{i=1}^{N}(y_i-w^Tx_i)^2+\lambda||w||_2^2(\lambda>0)
$$

##### （2）L1范数正则化（LASSO, Least Absolute Shrinkage and Selection Operator, 最小绝对收缩选择算子）

$$
J(w)=\frac{1}{N}\sum_{i=1}^{N}(y_i-w^Tx_i)^2+\lambda||w||_1(\lambda>0)
$$

##### （3）L1正则项L2正则项结合（Elastic Net）

$$
J(w)=\frac{1}{N}\sum_{i=1}^{N}(y_i-w^Tx_i)^2+\lambda(\rho||x||_1+(1-\rho)||w||_2^2)
$$

其中，$L_1$范数正则化、$L_2$范数正则化都有助于降低过拟合风险，$L_2$范数通过对参数向量各元素平方和求平方根，使得$L_2$范数最小，从而使得参数的各个元素接近0 ，但不等于0。 而$L_1$范数正则化比$L_2$范数更易获得“稀疏”解，即范数正则化求得的会有更少的非零分量，所以$L_1$范数可用于特征选择，而$L_2$范数在参数规则化时经常用到（事实上，$L_0$范数得到的“稀疏”解最多，但$L_0$范数是$x$中非零元素的个数，不连续，难以优化求解。因此常用$L_1$范数来近似代替)。