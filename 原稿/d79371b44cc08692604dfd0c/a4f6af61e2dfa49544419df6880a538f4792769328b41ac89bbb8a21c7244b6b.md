<center>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=3&id=2533688005&auto=1&height=66"></iframe>
<p class="img-caption">群山化袖骨，百川即袍尾</p>
</center>

## 介绍

OOD 指这么一类任务，即测试时遇到的样本和训练时的样本要么不是一个 class 的（新类检测），要么是属于同一个 class 的不同 domain 的图像（比如同一个疾病的不同模态；同一个人像的不同绘画风格；同一个图像源的不同采集角度、采集参数等等）。根据具体的任务，OOD 还能继续细分为 near OOD 和 far OOD。其中 near OOD 就是指长得和训练数据很像，但是仍然属于 OOD 范畴的输入数据。医学影像中的罕见病灶往往都符合 near OOD 的定义，因此，将这些玩意儿检测出来显得具有挑战性。

为了解决 OOD 的问题，我们肯定希望模型本身具有判断自身结果置信度的途径。需要额外说明，在多数情况下，分类网络得到的结果只是当前类别的概率，但是并不代表对于当前这个 sample 预测的置信度。因此，如何让模型学习预测输入样本后，自身输出概率值的置信度成为了解决 OOD 问题的一个难点。我们不妨简单看一个非常简单的问题，MNIST 手写数字识别。假设现在已经训练完成了一个在测试集上精度达到 98.9% 的模型，我们现在玩一个简单的游戏——输入一个数字 1，并开始逐渐翻转，然后我们可视化模型的预测结果和数字 1 和 7 对应的 softmax 输出点的概率值随着旋转角度的变化曲线，如下图所示：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-1fdc937d338225752157b5bf6e34bb50_1440w.png" style="width: 40%;"/>
</div>

从图中可以看出，随着数字的翻转，预测结果开始预料之内地出现失误，从 1 到 5， 再到 7。如果你想使用 softmax 输出值的最小值最为 uncertainty 的判断标准，这也是很难做到的，从图中可以看出，不同数字的概率值总是此消彼长的，你很难找到一个恰当的值来描述当前系统的不确定性，这是由 softmax 激活函数的互斥性导致的，分类网络总会给出一个预测结果，无论好坏。

有的人可能会反驳，这是因为你的模型没做旋转不变性的特征增强。好好好，你说得对，但是旋转只是一种 domain 的差异，你怎么确保你能举出一个任务相关的所有 domain？所以计算模型的 uncertainty 还是非常有必要的。

而度量模型输出不确定性一共有两条路子：

- Aleatoric Uncertainty：从数据出发直接看，当前这个 sample 是不是 OOD 了
- Epistemic Uncertainty：从模型的参数或者输出值的角度来看，输入样本是不是 OOD 了

本文讲解的方法为后者（数据挖掘让炼丹师来做是不是有点越俎代庖了）目前解决这个问题的一个非常常用的手段就是 Evidiential Deep Learning（EDL）这是 NeurIPS 2018 上的一篇文章 Evidential Deep Learning to Quantify Classification Uncertainty 中提出的。

> 在不引起歧义的情况下，EDL 与 ENN 在本文中代指同一件事情。

在讲解具体的原理之前，我们不妨看看用 EDL 理论构建的模型是如何解决上面这个旋转数字不确定性问题的：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-44902f2530be46b67ce4ae9ce9906e0d_1440w.png" style="width: 40%;"/>
</div>

可以看到，随着图像的旋转（超出training domain），EDL 模型给出的 uncertainty 在不断升高，最后回到 1 后 uncertainty 也在不断降低，和预期结果完全吻合，从而很好地给出了当前输入样本“我没见过”或“我不知道”的概率。我们认为，这是一种解决 OOD 问题的方法。

---

## Evidential Deep Learning 模型

我们将问题缩小到一个简单的 K 分类网络 $f(x|\theta)$，它的参数是 $\theta$，输入特征 $x$，给出对应的 K 维向量，代表属于 K 个类别的概率。但是根据前面说的，这 K 个概率只是相对于训练样本而言的，如果是 OOD 样本，模型应该需要判断一下这 K 个概率的可信度。为了度量这种可信度，我们用 K 维向量 $b = (b_1, b_2, ...,b_K )$  来表示这 K 个类别概率的可信度，称为 belief mass，用 $u$ 表示预测结果的不确定性。显然， $b,u$ 一起构成了一组新的 PMF，自然满足：
$$
u + \sum_{i=1}^K b_i = 1
$$
所以 uncertainty 与 belief mass 的定义自然加上了一个归一化系数 $S$：
$$
b_k = \frac{e_k}{S}, \quad u = \frac{K}{S}, S = \sum_{i=1}^K(e_i + 1)
$$
其中， $e_i$ 被称为 evidence，正好点题了，原文对于 evidence 的描述是这样的：

> we term *evidence* as a measure of the amount of support collected from data in favor of a sample to be classified into a certain class

需要注意的是， evidence 与 类别的概率存在一致性，所以文中使用了分类网络的输出值加上一个非负激活函数建模了 $e$，在不引起歧义的情况下，我们有：
$$
e = f(x | \theta)
$$
显然， belief mass 和 uncertainty 其实建模了“概率的概率”，在统计学习中，有一个现成的多元概率分布恰好可以建模“概率的概率”，那就是 Dirichlet分布。因此， EDL 很自然地想到把上面建模的几个参数往 Dirichlet分布 上去靠，毕竟这个分布已经被研究烂了：
$$
f(x;\alpha) = \frac{1}{B(\alpha)} \prod_{i=1}^K x_i^{\alpha_i - 1}
$$
其中 $x_i$ 在0到1之间且之和为1，刚好满足 PMF 的要求。通过上面的式子和之前的几个符号的概念，很容易得到的它们和 Dirichlet分布 的关系：
$$
\alpha_i = e_i + 1\\
S = \sum_{i=1}^K \alpha_i
$$
这样，我们非常简单地直接套用 Dirichlet分布 建模了K分类网络概率值的置信度分布：
$$
D(p | \alpha) = \begin{cases}
\frac{1}{B(\alpha)} \displaystyle\prod_{i=1}^K p_i^{\alpha_i - 1} & p\in\mathcal S_K \\
0 & \text{else}
\end{cases}
$$
其中 $\mathcal S_K$ 代表一个 $K - 1$ 阶的标准单纯形的开集。

建模成功后，下面就是寻找 EDL 的损失函数了，这也是这项工作最核心最精彩的部分。

---

## 损失函数

如何优化上述模型提出的 opinion 呢？在第二类极大似然估计给出了答案。我们现在只考察第 $i$ 个样本 $x_i$，它的标签为 $y_i \in \mathbb N^K$（one hot）。记该样本在分类器中输出的 evidence 为 $e_i \in \mathbb R_+^K$，则，我们可以推导出此处的 Dirichlet分布 参数 $\alpha_i$，它是关于 $\theta$ 的函数：
$$
\alpha_i(\theta) = e_i + 1 = f(x_i | \theta) + 1
$$
此时我们得到了对应的 Dirichlet分布 $\mathrm{Dir}(\alpha_i)$ 和分布 的长度为 $S_i = \sum_{j=1}^K \alpha_{ij}$.

在已知  $p_i$ 和 $y_i$ 的情况下，我们自然是希望把 $p_{ij}|_{j:y_{ij} = 1}$ 最大化的，考虑到 $y_i$ 是 one hot 向量，在机器学习中，用于度量输出概率和 one hot 标签的方法有很多，主要为三类：

- 似然值 Likelihood：$- \prod_{j=1}^K p_{ij}^{y_{ij}}$
- 交叉熵 Cross Entropy：$- \sum_{i=1}^K y_{ij} \log p_{ij}$
- 均方误差 MSE：$||y_i - p_i||_2^2$

为了方便，我们先按照似然值的方式计算。

### 似然值损失 Likelihood

在正常的分类网络中， $p_i$ 是关于模型参数 $\theta$ 的函数，所以直接优化 $p_i$ 就能完成对于 $\theta$ 的优化。但是在 EDL 的语境下， $p_i$ 是从 Dirichlet分布 中采样得到的，而 Dirichlet分布 又是通过参数 $\alpha_i$ 确定的， $\alpha_i$ 又是关于网络 $\theta$ 的函数，即存在如下的参数依赖关系：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-25810eddd39dd872a15bcc46c89897a8_1440w.png" style="width: 60%;"/>
</div>

但是我们无法直接优化这个交叉熵，有两个原因：

- sample 是不可导的 OP
- 采样得到的 $p_i$ 存在随机性，无法很好地衡量得到的 $\alpha_i(\theta)$



因此，采用第二类极大似然估计（Type Ⅱ Maximum Likelihood Estimation）的想法，我们不能直接优化损失函数，转而优化损失函数关于 Dirichlet分布 的期望：
$$
-\prod_{j=1}^K p_{ij}^{y_{ij}} \quad\to\quad - \underset{p_i \sim \mathrm{Dir}(\alpha_i)}{\mathbb E}\left[ \prod_{i=1}^K p_{ij}^{y_{ij}}\right]
$$
然后就可以愉快地进行数学层面上的推导了：
$$
\begin{aligned} \mathcal L_i(\theta) &= - \log \left( \underset{p_i \sim \mathrm{Dir}(\alpha_i)}{\mathbb E}\left[ \prod_{i=1}^K p_{ij}^{y_{ij}}\right] \right)\\ &= - \log \left( \int \prod_{i=1}^K p_{ij}^{y_{ij}} \frac{1}{B(\alpha_i)} \prod_{i=1}^K p_{ij}^{\alpha_{ij} - 1} d p_i  \right) \\ &= - \log \left( \frac{B(\alpha_i + y_i)}{B(\alpha_i)} \int \underbrace{\frac{1}{B(\alpha_i + y_i)} \prod_{i=1}^K p_{ij}^{\alpha_{ij} + y_{ij} - 1}}_{D(p_i | \alpha_i + y_i)} d p_i  \right) \\ &= - \log \frac{B(\alpha_i + y_i)}{B(\alpha_i)} \\ 
&= - \log \frac{\displaystyle\prod_{j=1}^K \Gamma(\alpha_{ij} + y_{ij})}{\Gamma \left( \displaystyle\sum_{j=1}^K(\alpha_{ij} + y_{ij}) \right)}\frac{\Gamma \left( \displaystyle\sum_{j=1}^K \alpha_{ij} \right)}{\displaystyle\prod_{j=1}^K \Gamma(\alpha_{ij})} \\ 
&=  \left. -\log \left( \frac{\Gamma (\alpha_{ik} + 1)}{\Gamma (\alpha_{ik})}\frac{\Gamma(\displaystyle\sum_{j = 1}^K \alpha_{ij} )}{\Gamma(1 + \displaystyle\sum_{j=1}^K \alpha_{ij} )} \right)\right|_{k : y_{ik} = 1} \\
&=  \left. -\log \left( \frac{\alpha_{ik}}{\underbrace{\displaystyle\sum_{j=1}^K \alpha_{ij}}_{S_i} } \right) \right|_{k : y_{ik} = 1} \\ 
&= \left. y_{ik}\left(\log S_i - \log \alpha_{ik}\right) \right|_{k : y_{ik} = 1} \\ 
&= \sum_{j=1}^K y_{ij}  \left(\log S_i - \log \alpha_{ij}\right) \end{aligned}\\
$$
这样，我们就成功将式子中的 $p_i$ 去除了，从而解决了 sample 操作带来的两个问题。



### 交叉熵损失 Cross Entropy

同理，如果我们优化的函数不是似然函数，而是交叉熵，那么优化函数为交叉熵关于 Dirichlet分布 的期望：
$$
-\underset{p_i \sim \mathrm{Dir}(\alpha_i)}{\mathbb E}\left[ \sum_{i=1}^K y_{ij} \log p_{ij}\right]
$$
我们也可以通过和上面一致的方法将 $p_i$ 去掉，从而得到的对应的损失函数：
$$
\mathcal L_i(\theta) = \sum_{j=1}^K y_{ij}  \left(\psi(S_i) - \psi(\alpha_{ij})\right)
$$
其中 $\psi(\cdot)$ 是双伽马函数，满足：
$$
\psi(x) = \frac{\partial }{\partial x} \ln \Gamma(x) \approx \ln x - \frac{1}{2x}
$$


### 均方误差损失 MSE

类似，需要优化的函数为 MSE 关于 Dirichlet分布 的期望：
$$
\underset{p_i \sim \mathrm{Dir}(\alpha_i)}{\mathbb E} || y_i - p_i ||_2^2
$$
类似地，我们可以得到：
$$
\mathcal L_i(\theta) = \sum_{j=1}^K \mathbb E[ y_{ij}^2 - 2 y_{ij} p_{ij} +p_{ij}^2] = \sum_{j=1} ^K \left(y_{ij} - 2y_{ij} \underset{x\sim\mathrm{Dir}(\alpha_i)}{\mathbb E}[x] + \underset{x\sim\mathrm{Dir}(\alpha_i)}{\mathbb E}[x^2]\right)
$$
Dirichlet分布 的一阶矩和二阶矩的闭式解我们都是知道的。我们记：
$$
\hat{p}_i = \underset{x\sim\mathrm{Dir}(\alpha_i)}{\mathbb E}[x] = \frac{\alpha_{ij}}{S_i}
$$
所以上式可以进一步化简为：
$$
\mathcal L_i(\theta) = \sum_{j=1}^K (y_{ij} - \hat{p}_{ij})^2 + \frac{\hat{p}_{ij} (1 - \hat{p}_{ij})}{(S_i + 1)}
$$


在原文中，作者团队通过实验发现了基于 MSE 的损失函数效果更好，所以他们使用了基于 MSE 的。而且基于 MSE 的损失函数式子还存在一个 bonus，它的式子的两个式子具有很好的解释性：
$$
\mathcal L_i(\theta) = \sum_{j=1}^K \underbrace{(y_{ij} - \hat{p}_{ij})^2}_{\mathcal L_{ij}^{err}} + \underbrace{\frac{\hat{p}_{ij} (1 - \hat{p}_{ij})}{(S_i + 1)}}_{\mathcal L_{ij}^{var}}
$$
文章中还给出了三个定理来证明，基于 MSE 构建的损失函数可以达到更好的效果（evidence 的增加或减少都会让 $\mathcal L_{ij}^{err}$ 减少，详细见[原文](https://arxiv.org/pdf/1806.01768.pdf) 的附录部分）

总之，经过之前的讨论，我们知道了 N 个样本的总的损失为：
$$
\mathcal L(\theta) = \sum_{i=1}^N \sum_{j=1}^K (y_{ij} - \hat{p}_{ij})^2 + \frac{\hat{p}_{ij} (1 - \hat{p}_{ij})}{(S_i + 1)}
$$


### evidence 坍塌：正则项

如果只是这么训练，会遇到一个问题，那就是这个 evidence 生成网络会变得和原本的分类器类似，学会基于特征图模型进行判别。具体而言就是原文认为改判为第 j 类的特征，ENN也会认为这个特征的 evidence 很高，这样，我们训练 ENN 就失去了意义（evidence 和 probability 协变了）。为了抹除这种协变，我们肯定希望生成 evidence 的过程能够在一定的情况下坍塌，直接返回一个 evidence 全 0 结果，这种情况下，我们有：
$$
u = \frac{K}{\sum_{i=1}^K (e_i + 1)} =\frac{K}{\sum_{i=1}^K (0 + 1)} = 1
$$
也就说此时，模型可以返回一个 uncertainty 为 1 的结果，告诉我们当前结果预测不可靠，这也是我们希望模型表达出的一种模式。由于 $\alpha_i = e_i + 1 = 1$，该情况下 Dirichlet分布 为：
$$
D(p_i\vert \underbrace{(1,...,1)}_{K个1}) := D(p_i | \bold 1)
$$
所以我们其实希望，模型在训练的过程中，不仅可以拟合 evidence 与 label 的一致性，还能让模型和这种坍塌的模型分布靠近。也就是让模型去靠近上面这个全 1 Dirichlet分布。当然，标签给出的那个维度我们是不希望被预测为 uncertainty 的，所以我们需要过滤掉 $y_i$ 中不为零的那一项对应的 evidence 预测值，这一项需要保持一致，不能坍塌。所以我们希望往 $D(p_i | \bold 1)$ 靠近的分布不是 $D(p_i | \alpha_i)$，而是：
$$
D(p_i | \tilde{\alpha}_i), \tilde{\alpha}_i = y_i + (1 - y_i) \odot \alpha_i
$$
也就是说，我们希望 $y_i$ 对应的那个类别的 evidence不变，其他的 evidence 尽可能往“完全不可信”这个分布上去靠拢，很符合直观上的感受。机器学习中，度量分布的损失函数，最常用的就是 KL 散度了，所以这一项的增加不确定性的损失函数为：
$$
\sum_{i=1}^N \mathrm{KL}\left(D(p_i|\tilde{\alpha}_i) || D(p_i | \bold 1)\right)
$$
这一项可以当作正则项，可以理解为对于 ENN 完全不表现出不确定性预测模式的惩罚（我们训 ENN 本来就是希望知道啥时候模型不可靠，你总得表现一下吧？），总的基于 MSE 的 ENN 的损失函数为：
$$
\mathcal L(\theta) = \sum_{i=1}^N \mathcal L_i(\theta) +\lambda_t \sum_{i=1}^N \mathrm{KL}\left(D(p_i|\tilde{\alpha}_i) || D(p_i | \bold 1)\right)
$$
其中 $\lambda_t$ 是一个热启动参数，它是关于训练轮数 epoch（记作 t）的函数：
$$
\lambda_t = \min(1, t / 10)
$$

---

## 实验部分

目前我找到了两个版本的 EDL 实现：

- tensorflow 版本：https://github.com/aamini/evidential-deep-learning （300+ star）
- pytorch 版本：https://github.com/dougbrion/pytorch-classification-uncertainty （300+ star）



我们可以看一下 pytorch 版本对于之前那个手写数字识别旋转问题，先下载项目：

```bash
git clone https://github.com/dougbrion/pytorch-classification-uncertainty
pip install -r requirements.txt
```

然后简单训练个 50 轮到收敛即可，下面是使用基于 MSE 的损失函数训练：

```bash
python main.py --train --dropout --uncertainty --mse --epochs 50
```

你也可以基于 交叉熵 和 对数似然：

```bash
# 交叉熵损失
python main.py --train --dropout --uncertainty --digamma --epochs 50

# 对数似然损失
python main.py --train --dropout --uncertainty --log --epochs 50
```

训练完成后，测试三种损失值函数的模型如下：

```bash
python main.py --test --uncertainty --mse
python main.py --test --uncertainty --digamma
python main.py --test --uncertainty --log
```



在不同类型的损失函数下的结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a4246e077baf1bb61c0d13e1d4392b61_1440w.png" style="width: 90%;"/>
</div>



既然旋转异常的不确定性可以检测出来，我们来简单看看单个图像预测得到的结果和对应的不确定性，我们先看看正常输入的手写数字：（采用交叉熵模型）

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c73eb09b677eb203130f3ec6e42c6c05_1440w.png" style="width: 30%;"/>
</div>

可以看到，不确定性相当低，模型也成功预测出了结果为1.我们下面来整点其他图像，比如我的头像：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-9012f6fbc23406f1639bb2ab3b8943d5_1440w.png" style="width: 30%;"/>
</div>

可以看到，模型的不确定性相当高，可以认为这张图像相对于训练集而言是 OOD，在系统中可以直接返回：“我没见过这张图像”。我们继续往里面输入一些色彩更加鲜艳的图像，比如下面这几张：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d01071f2e0a0a537440a46345af81307_1440w.png" style="width: 90%;"/>
</div>

> 从左到右，从上到下为：莫奈的《月光下的航海夜景》；葛饰北斋的《神奈川冲浪里》；我的工作台；被损坏的表情包

不同的损失函数造成的结果差异可能会很大，比如如下的case：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ae9f8eada6cb5df4542ca654ed1d16e3_1440w.png" style="width: 90%;"/>
</div>

因此，损失函数的选取确实非常重要，也值得后续的探索。

---

## 参考文献

- https://github.com/dougbrion/pytorch-classification-uncertainty
- https://arxiv.org/pdf/1806.01768.pdf