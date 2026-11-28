## 前言

最近实验室的某位师姐花了两节课的时间介绍了因果推断相关的工作，感觉这个方向会是一项对未来数据科学影响深远的技术点。于是乎，怀着“可以不用，但是不能不知道”的学术顽固派理论，我结合她的slice和搜集到的资料，佐以笔者愚笨的头脑，妄图狗尾续貂，写下了这篇博客。希望可以从一个更加感性且具体的角度，带领完全不知道因果推断的小白入门。

> 本文中除了开头几个fafa琳的表情包，其余图都是笔者使用 vscode-drawio 绘制，引用时请标明出处，3Q。

---

## 建议看的一些资料

在知乎上，与因果推断相关的高赞文章不少了，从我个人角度，我总觉得不太符合我的个人理解。但是我仍然希望阁下在阅读我的文章之前，可以前去阅读这些已有的文章，

- 与因果推断的哲学史有关，帮助建立自上而下的认知：https://zhuanlan.zhihu.com/p/343747851
- 适合从事因果推断的硕士和博士入门的文章，需要一定的数学基础：https://zhuanlan.zhihu.com/p/397796913

我在本文中可能会冒昧地使用这几篇文章中给出的case。

---

## 原料

为了能够更加顺畅看完这篇文章，你需要事先了解如下的知识点：

- 概率论，知道什么是条件概率，贝叶斯公式，什么是贝叶斯网
- 统计学基础，知道相关性系数，知道如何计算统计数据的期望

> 如果可以，我想把这篇文章归于“数据挖掘”这个专题，而非“机器学习幼儿园”。

特别申明：在概率论、统计学、机器学习、数据挖掘中，下面的名词往往等价：

- 变量 variable
- 特征 feature
- 属性 attribute

在不说明的情况下，上面三个名词说的都是一个东西。如果放在我们最经常研究的对象，表格中，上面三个东西都描述了表头（table head）中的内容。一个 4 行 3 列的表格描述了如下的数据：

- 该数据有4个采样（sample）/ 数据集大小为 4
- 该数据拥有3个变量/特征/属性

---

## 从相关性到因果：一个例子

### 回顾相关性 correlation

在统计学中，我们都知道如果给定一张表格或者数据立方体（data cube），我们可以通过计算变量之间的相关性系数来得到变量与变量之间的相关性。

这在许多领域中都有着极其关键的作用：

- 大型企业需要搜集用户数据，并计算用户数据中变量相关性，从而为后续的销售策略提供数据支撑，比如著名的例子：“啤酒与尿布”。
- 在深度学习中，几乎所有的 attention 模块都是在计算 token/feature 之间的相关性，比如 transformer，Luong attention。

在接近半个世纪的应用中，人们越来越依赖一个“不证自明”的事实：数据的相关性系数可以衡量变量之间的关系，假如属性 A 和 B 的相关性系数很高，我们可以认为 A 和 B 有密切的关系。所以，如果我们想要改变 B，可以从改变 A 出发考虑对策。

这，真的对吗？

<div align=center>
<img src="https://picx.zhimg.com/80/v2-3b943eff3a3084bba796175387f63f77_1440w.png" style="width: 30%;"/>
<p class="img-caption">这为啥不对？</p>
</div>



### 辛普森悖论（Simpson’s  Paradox）：相关性 ≠ 因果性

我们很容易进入一个思维陷阱中，当我们在说“改变 A 也能改变 B”时，其实我们已经在使用如下的假设了：A 是 B 的因，B 是 A 的果，即，A 与 B 具有因果性。但是我们之前计算的相关性系数，只能衡量变量之间的相关性，**有关系并不能说明 A 是 B 的因， B 是 A 的果**。

可能有的观众觉得非常疑惑，因为我的这个陈述句与一些观众的思维模式有点不一样。为了更加感性地认识这句话，我们不妨举一个具体的例子，想要证伪一个命题（相关性可以导出因果性），最好的方式就是举反例。

你是一家冰淇淋店的老板，你想要研究冰淇淋的销量和什么因素有关，于是你搜集了这个月的冰淇淋店相关的数据，整理如下：

| 冰淇淋营收额（元） | 电费（元） | 气温（摄氏度） |
| ------------------ | ---------- | -------------- |
| 2000               | 200        | 30             |
| 2500               | 220        | 33             |
| 1800               | 160        | 25             |
| 2300               | 220        | 31             |
| 2600               | 300        | 33             |
| 2200               | 230        | 31             |

然后精通小学数学和中文编程的你计算了一下“冰淇淋营收额”和“电费”的相关性系数：

```python
>>> 冰淇淋营收额 = np.array([2000, 2500, 1800, 2300, 2600, 2200])
>>> 电费 = np.array([200, 220, 160, 220, 300, 230])
>>> np.corrcoef(冰淇淋营收额, 电费)[0, 1]
0.8655123155193809
```

发现得到的结果非常大，所以你就说，电费是冰淇淋营收额的影响因素，电费越高，冰淇淋营收额越大。然后你兴奋地加大了压缩机的工作负载，让电表倒转，并将这一发现发在了朋友圈里，等待你的朋友来点赞。

等等，你听听，这是人话吗？

<div align=center>
<img src="https://pica.zhimg.com/80/v2-6544ee3be5b5edb2e6aed1e07a9c081f_1440w.png" style="width: 30%;"/>
</div>


从相关性系数的计算上，我得到了一个与我们的常识矛盾的结论——电费越高，冰淇淋营收额越大。也就是说，我们的推理系统不自洽了。为什么会这样呢？然后你请教了你辅修数据挖掘的朋友，你的朋友在电脑上操作一通，你看到很快电脑上跳出了一张图：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-3f7ecc3812a6f1953b2a2fda14859893_1440w.png" style="width: 60%;"/>
</div>

你的朋友接着解释到，这张图名为因果图，橘黄色的线条代表变脸之间的统计学关系，显然，从统计学角度，这三个变量之间都是强相关的；蓝色的线条代表它们之间的因果性，气温是电费和冰淇淋营收额的因，而电费和冰淇淋营收额之间本来就不存在因果关系。

电费与冰淇淋营收额因为“气温”这一共同因的影响，导致它们的相关性系数也很高（更加统计学一点，它们是一组 协变量 covariance），我们一般将这种由某种共同因子导致的伪相关称为 **混淆关联（confounding association）**。很显然，下图中的气温定义出了一条从“电费”到“冰淇淋营收额”的 confounding association，有关 confounding 的讨论，我们会在下文中详细讨论。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-3fbb1c0cd3d1a740dd1263caf949794c_1440w.png" style="width: 60%;"/>
</div>



> 大量 amusing 的社会现象都与这个本就不自洽的逻辑有关，比如基于身份认同的奢侈品购买；因为很多程序员都是二次元就说喜欢二次元的代码水平都很高；高知家庭的孩子擅长琴棋书画且成绩很好，就认为培养孩子的艺术造诣能够提高孩子的成绩；人们往往只会看到因素之间的相关性，但是相关性不代表因果性。很多时候，就和冰淇淋销售例子一样，我们看到的都是果，真正的因埋藏在果的背后，等待阁下前去挖掘。

很明显，只要有了因果图，我们就能更加准确地基于统计学信息来获取变量之间的因果性了。那么如何根据表格信息来获取因果图呢？你的朋友递给你了一本书：The Book of Why: The New Science of Cause and Effect.

相信到这里大部分观众应该都形成了共识：相关性 ≠ 因果性。其实，这个现象在20世纪初就有人讨论，但是直到1951年，[爱德华·H·辛普森](https://zh.wikipedia.org/wiki/愛德華·H·辛普森) 在他的论文中才系统描述了这个现象，因此被后人将“由统计学数据诱导出不正确的因果性”这样的现象称为 [辛普森悖论 (Simpsons’ Paradox)](https://zh.wikipedia.org/wiki/%E8%BE%9B%E6%99%AE%E6%A3%AE%E6%82%96%E8%AE%BA). 而大力推广因果推理而获图灵奖的 Judea Pearl （上面那本 the book of why 就是他写的）也在研究中发现**统计相关性并不能取代因果性**，统计中方法中给出的相关性系数计算也无法给出逻辑的方向性，从而直接导出了颠倒因果、伪相关结论。在后续的文章中， Judea Pearl 的理论将起到举足轻重的作用。

---

## 因果推断

因果推断在 wiki 上的定义为：

Causal inference is the process of *determining the independent, actual effect of a particular phenomenon that is a* component of a larger system.

其实就是字面意思。

因果推断（Causal Inference）可由两类任务组成：

- 因果关系挖掘（Causal Discovery）：即给定一组数据，挖掘出数据属性的因果图或者因果图的一部分。
- 因果效应估计（Causal Effect Estimation）：在得到变量因果关系的前提下，研究不同变量之间如何定量地相互影响；也可以用来定量计算出不同的因对于同一个果，谁的影响程度更大（可用于变量性能评估）。

当然，基于这些任务，我们也可以完成诸如深度学习模型可解释性、模型鲁棒性研究、模型bias 等等任务，此乃后话，于此就不再赘述了。

而本文下面会形式化地推导和证明有关“因果效应估计”的部分基础性质的内容，以便读者可以快速入门，其中涉及到的部分术语和概念会是在基于深度学习的因果推断中仍然会提到的。

为了便于读者建立一个感性的认知，我将这一部分的内容做出如下总结：尝试在不进行额外实验的情况下，仅仅通过统计学数据和额外的因果建模来实现因果关系的定量计算。说人话就是，既然统计学的相关性不等于因果性，那我就想办法通过数学建模，让观测量的函数在某些情况下能够正确地衡量因果性。

---

## 好戏开场：Causal Effect Estimation

Fine! 我们接下来终于可以使用最喜欢的数学来建模一个问题，从而让阁下更加理性地认识什么是因果效应估计了。

我们现在假设这样一个场景：我司研发了一种新药，并且做了一期临床实验，我们现在想要知道，这个药物是否真的有效。我们记病人的状态为随机变量 $Y$，取值为0或者1，分别代表病情没有好转和病情好转。用随机变量 $T$ 代表是不是给药了，0代表该组实验没有给药，1代表该组实验给药了（取值为0，即什么都不做，也被称为控制组 controled group）。

> 来自这个实验的数据我们称为观测数据，后续的讨论中，观测数据/观测分布本身是不会发生变化的，因为我们不被允许进行额外的实验。不进行额外实验，仅仅使用观测数据，这也是 causal inference 的前提假设。

我们现在很关心，使用了药物之后，病人的情况是否真的有所好转。我们将这个好转程度称为**平均治疗效果（Average treatment effect，后面都用 ATE 代指）**。很明显，基于条件期望就能定义出 ATE：
$$
\mathbb E[Y|T=1] - \mathbb E[Y|T=0]
$$
但是这么定义是存在很大的问题的，结合之前冰淇淋的例子阁下就应该能想到，如果关于某个额外的变量 X，它构成从 T 到 Y 的 confounding association，那么上面得到的式子就是不对的。如下图所示：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-44f2ddfc811f33d379d1d14132c79054_1440w.png" style="width: 60%;"/>
</div>

比如给药的房间和不给药的房间使用的设备什么不一样，医护人员的专业素质等等，都可能构成 confounding association。

想要让上式成立，那么只能：**T 与其他可能潜在的变量均独立**。



### 观测分布 vs 干预分布：Do 算子 (do-operator)

为了明确定义出观测分布上 ATE 的取值，我们需要**确切**地定义出一个与其他所有变量均独立的随机变量，这样才能心安理得让之前的 ATE 定义式生效。什么样的变量与潜在的所有观测变量都独立呢？一个想法就是用人为干预的结果作为新的变量，因为是后期处理时人为干预的结果，从逻辑上和观测数据本身发生时没有关系，我们认为人为干预的结果产生的分布与观测值本身是独立的。干预后的观测分布也就被转换成了 **干预分布（intervention distribution）**。

如何定义干预分布中我们人为施加的变化，便是 causal inference 的工作。其中申明式地定义了一个非常核心的概念：Do 算子（do-operator）。形式化的定义如下：
$$
\mathbb P(Y | do (T=t_0)) := \mathbb P(Y | \text{set T to } t_0)
$$
需要注意的是，在 do 算子定义下的概率 $P$ 已经不再是 $Y$ 观测结果（observed outcome）了，而是 $Y$ 的潜在结果（potential outcome）了。因此，我们不能直接通过观测数据（也就是一张记录了组学数据的表格）得到 $\mathbb P(Y | do (T=t_0))$ 的。

> do 算子的定义是 causal inference 中非常关键的一步定义，因为是申明式的定义，请阁下不要较真。倘若你还是无法理解 do 算子的含义，那么请只需记住一句话即可： **do 算子定义的变量与观测分布中的其他变量均独立**。



### *潜在结果 potential outcome

Do 算子的定义导出了潜在结果这个概念。在此，为了后续论述中概念的完整性，锦恢想要引出几个和潜在结果相关的概念，这一个 section 阁下完全可以选择跳过。

在之前的论述中，我们应该能知道，在人为干预，也就 do 算子的影响下，$Y | do (T=t_0)$ 与观测结果无关。也就是说，当我们在讨论 do 算子下的分布时，我们可以忽略掉观测数据中对应变量的真实取值（T 为 0 或 1）。潜在结果 / do 算子 的这种可以忽略观测结果的性质被称为 [可忽略性(Ignorability)](https://en.wikipedia.org/wiki/Ignorability)，它直接定义出了干预变量 $Y$ 与 观测变量 $T$ 的独立性，这对于解释或者指导设计可忽略 confounding 的统计学方法（比如 RCT）有很重要的作用。

> 无法理解没关系，后面会详细讨论。



### Do 算子定义下的 ATE

基于 do 算子的定义，我们认为如下的 ATE 定义成立：
$$
\mathbb E[Y | do(T=1)] - \mathbb E[Y | do(T=0)]
$$
为了方便后续的讨论，我们往往会将 $Y|do(T=t_0)$ 记作 $Y(t_0)$，从而 ATE 的定义式为：
$$
\mathbb E[Y(1) ] - \mathbb E[Y(0)]
$$
那么如何求解这个含义 do 算子的期望呢？本文中给出的思路非常朴素，用下面这张图就能概括：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2d81824b5efc1e154e45c0756b5cef9a_1440w.png" style="width: 60%;"/>
</div>



解释一下：既然我们可以从观测数据中获取 $\mathbb E[Y|T=1] - \mathbb E[Y|T=0]$，但是它却无法真正衡量 ATE 大小；真正能衡量 ATE 的式子 $\mathbb E[Y(1) ] - \mathbb E[Y(0)]$ 我们又无法直接从观测数据中获取。那么为了构建从 观测数据 到 因果性（比如ATE） 的一条逻辑通路，我们很自然地想到 $\mathbb E[Y|T=1] - \mathbb E[Y|T=0]$ 和 $\mathbb E[Y(1) ] - \mathbb E[Y(0)]$ 这两个玩意儿啥时候相等，只要在某种情况下它们两个相等，或者构建什么联系，我们就能通过如下的路径，通过观测数据计算出因果性惹：
$$
观测数据 \to \mathbb E[Y|T=1] - \mathbb E[Y|T=0] \to \mathbb E[Y(1)] - \mathbb E[Y(0)] \to 因果性 
$$
好，我们下面就研究一下什么情况下，这两个玩意儿会相等。从定义上，我们其实可以获得如下等式（观测分布和干预分布一致性）：
$$
\mathbb [Y | T = t_0] = \mathbb [Y(t_0) | T = t_0]
$$
如果我们能够证明下式就行了：
$$
\mathbb [Y(t_0) | T = t_0] = \mathbb [Y(t_0)]
$$
上式何时成立？答曰： Y 与 T 独立时，所以我们下面着重通过建模论述，合适 T 与 Y 成立，此处需要用到一个很重要的工具：贝叶斯网。

---

### 贝叶斯网

贝叶斯网（Bayesian network），可以看成是一阶马尔可夫链的一种推广形式，相信不少观众在本科学习机器学习时就遇到了这个工具（笔者最早是学习西瓜书时遇到的，当时的笔记：https://zhuanlan.zhihu.com/p/455998081）。我们在这里简单回顾一下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b1a25062f3ca6575f4f0af4c3bbfa44c_1440w.png" style="width: 60%;"/>
</div>

更加普遍的形式为，对于 n 个存在依赖关系的变量，它们的联合分布可以分解为：
$$
\mathbb P(X_1,X_2,...,X_n) = \prod_{i=1}^n \mathbb P(X_i|\text{pa}_i)
$$
上述公式有一个更加容易记住的term，叫做贝叶斯网分解（Bayesian network factorization）。它将成为我们分析存在 confounding 关系的变量的分布的一个强有力的工具。

另外提一嘴，我们在贝叶斯网中定义的 路径 和一些教材的不太一样，我们定义路径是将贝叶斯网当成无向图进行定义的，比如，下面的都算是上图中的一条路径：

- $X_1\to X_2\to X_3 \to X_4$
- $X_4\to X_3$
- $X_3 \to X_1 \to X_2$

总之，只要相连，不管谁是头谁是尾，都算是路径。

---

### 三节点贝叶斯网：Chain, Fork, Immorality

通过之前的讨论，我们其实已经可以将因果关系的讨论抽象到三个节点 $T, Y, X$ 了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-16ea2d4d93a5828fbc5d79200af7393c_1440w.png" style="width: 70%;"/>
</div>



事实上，上面的三节点只是描述了 X， T， Y 的一种可能的因果关系。对于含有三个节点的贝叶斯网，学术界早就完整地讨论过它们的可能因果关系了，一共有三种

<div align=center>
<img src="https://picx.zhimg.com/80/v2-931b5aac844b5e9f9d56c54b519450cd_1440w.png" style="width: 70%;"/>
</div>



很幸运的是，上面这三种结构都有一些很好的结论：



#### Chain 中的条件独立

结论：$X_1⊥X_3|X_2$.

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7773f9cf51f8cedc6eec418ef8f4d539_1440w.png" style="width: 60%;"/>
</div>

> 这都看不懂？你退学吧。（为什么总有人说我喜欢跳步骤）

此时， $X_1$ 指向 $X_3$ 的 confounding association 被阻断了。



#### Fork 中的条件独立

结论：$X_1⊥X_3|X_2$.

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-bea2fa142253b124523a94dd925b9d95_1440w.png" style="width: 60%;"/>
</div>

此时， $X_1$ 指向 $X_3$ 的 confounding association 被阻断了。



#### Immorality 中的边际独立性 marginal independence

结论：$X_1\perp\!\!\!\perp X_3|X_2$.

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-147669283fde83ef0ab247b3ad1702ac_1440w.png" style="width: 60%;"/>
</div>

上图中 $X_2$ 被称为 `collider` 。

这种情况比较特殊，能导出独立性的前提不是 $X_2$ 已知，而是 $X_2$ 未知。从推导过程也能看出，必须要让 $X_2$ **完全未知**才能顺利使用边缘分布公式反向展开。在贝叶斯网中，为了更加准确地描述一个变量未知情况下另外两个变量独立的情况，并和 Chain 和 Fork 的情况进行区分，定义了术语 边际独立性 （marginal independence，或者叫边缘独立性）。

如果 $X$ 与 $Y$ 关于 $Z$ 边缘独立，则记为：
$$
X \perp\!\!\!\perp Y | Z
$$
此时 $Z$ 应该关于 $X, Y$ 完全未知，而非已知的意思。

> 起这个名字我猜测是因为这个结论是基于 $X_2$ 可以使用边缘分布公式展开表示，即， $X_1,X_3$ 的独立性是体现在 $X_1,X_3$ 的边缘分布上的。



### 独立性与阻塞路径 blocked path

相信不少观众都注意到了，锦恢在解释三种结构的图中都画上了虚线箭头到一个橘黄色小砖头的符号。我其实想要表示这么一件事：因为我们证明了在种种条件下， $X_1$ 和 $X_3$ 独立，甭管是条件独立还是边际独立，总之独立了，概率论告诉我们如果两个变量的相关性系数为0，只能说明它们没有线性相关性，但是不一定独立；但是反过来，如果两个变量独立，那么它们的相关性系数一定为0！

也就是说，既然 $X_1$ 和 $X_3$ 是独立的，那么它们此时就不存在相关性，也不存在 confounding association了（混淆相关性也是相关性嘛），我们终于把该死的 confounding 在一定条件下删除了！

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-b58c6dbbc7df691fdfeb3343889a624f_1440w.png" style="width: 20%;"/>
</div>

先别急，让我们先整理一下有哪些情况下， $X_1$ 和 $X_3$ 可以独立。在 causal inference 中，如果贝叶斯网中两个变量在一定条件下独立，从而消除了它们之间的 confounding association 了，那么就称这种情况下这两个变量之间 confounding association （如果有的话）被阻塞了，这条关系也就成了 阻塞路径 （blocked path）。而我们论证过了，如果两个变量之间的路径是阻塞路径，那么它们之间就不存在 confounding association 了。

> 此时，我们就可以认为 do 算子和把do 算子摘掉后的条件概率是等价的。（这句话可能有问题，存疑）

我们来看看 causal inference 中如何总结并归纳了哪些情况算是 blocked path：

一条在节点 $X$ 与 $Y$ 之间的路径在如下情况 被定义为被 conditioning 集合 $Z$ 阻塞（其中 $Z$ 可以是空集）：

- 在路径中存在 `Chain` $... \to W \to ...$ ，其中 $W$ 被 condition 住（也就是 $W\in Z$）。
- 在路径中存在 `Fork` $... \leftarrow W \to ...$ ，其中 $W$ 被 condition 住（也就是 $W\in Z$）。
- 在路径中存在 `Immorality ` $...\to W \leftarrow ...$ ，其中 $W$ 和 它的所有后继节点都没有被 condition 住（也就是 $W\not\in Z \land \mathrm{de}(W) \not\subset Z$）。

> de 是 descendant（后代） 的缩写



### *有向分离 d-separation

> 这部分内容在后面用不到，请急急国王跳过

但是只是基于 blocked path 的定义，想要通过编程快速在一张图中得到任意两个变量的独立性还是不简单。因此，有人推导了一个叫有向分离的算法，用于快速判断贝叶斯网中任意两个变量是否独立。做法很简单，在我的往期博客中提到过，首先将贝叶斯网化成道德图：

<div align=center>
<img src="https://pic2.zhimg.com/v2-b34424b743d21d826f94b259dd9317f9_r.jpg" style="width: 60%;"/>
</div>

道德图是一个无向图，对于无向图上的任意三个点 $X, Y, Z$，如果去掉 $Z$ 后，$X$ 与 $Y$ 不连通（懒得用专业的图论术语表达了，你们爱吐槽就吐槽吧），则有：
$$
X \perp Y | Z
$$
此时，我们也称 $Z$ 有向分离了 $X$ 到 $Y$ （$Z$ d-separates $X$ from $Y$）。这在 causal inference 中是一个非常重要的算法，比如 Pearl 的 **后门准则（back-door criterion）** 中就会用到。

---

## Causal Effect Estimation 理论应用

在讲解了和 Causal Effect Estimation 相关的理论后，相信阁下迫切想知道这玩意儿怎么使用。在这个 section 中，我们会基于上面的理论，给出两种可以规避 confounding association 的统计学方法。

### 随机控制试验 RCT

[随机控制试验](https://zh.wikipedia.org/wiki/%E9%9A%8F%E6%9C%BA%E5%AF%B9%E7%85%A7%E8%AF%95%E9%AA%8C)，英文 **r**andomized **c**ontrolled **t**rial，业内一般简称 RCT，是在医疗领域常用的一种对于药物或者诊断方法进行有效性验证的手段。RTC 在司法、教育等涉及公平性的场景中也会被使用。

> 在 [PMC6235704](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235704/) 中指出， RTC 已经成为了有效性实验的金标准

RCT 的一大类方法的总称，这类方法往往具有如下的特点：

- 研究对象**随机分组**（一般是基于计算机的随机数）
- 对不同的组施加不同的**人为干预**
- 实验主体对于目前的实验数据部分了解或完全不了解（比如进行实验的医生不知道病人的患病情况和手里的药物是不是实验药物）

一个例子就是《理科生坠入情网，故尝试证明》中，第五集的例子，冰室和雪村为了研究男女喜欢彼此的一般条件，进行了“情侣吸管”实验，其中随机打乱了受试者和 TA 的对象，从而消除了喜欢的条件因为受试者身份产生的 confounding association.

<div align=center>
<img src="https://pic2.zhimg.com/80/v2-3b3b8402b6601c78ff3407e398e9fea9_1440w.webp" style="width: 70%;"/>
</div>

更加详细的做法，阁下不妨移步 Wiki https://en.wikipedia.org/wiki/Randomized_controlled_trial 。我们现在不妨通过上面的因果推断理论，来尝试理解，为什么 RCT 在样本足够多的情况下可以抵消已知或者未知的混杂因子对各组的影响（也就是干掉 confounding association）。

可以看到，RCT 中引入了人为干预（artificial intervention，为了偷懒，我就用 intervention 代替了）的手段，相比于原本的 $T$，我们得到的就是 $T_{inter}$ 了，这可以看成从观测分布到干预分布的一种采样近似。这让我们收集到的观测数据能够在一定程度上靠近 do 算子定义的随机变量 $Y(i)$，通过我们之前的论述，这样得到的 ATE 可以近似出真正的 ATE，整个过程如下图所示：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-4b4d5a08d7b61d4f0536d375cae3d4ff_1440w.png" style="width: 80%;"/>
</div>

在许多的资料中， $\mathbb E[Y|T_{inter}=1] - \mathbb E[Y|T_{inter}=0]$ 也被记为 $\text{ATE}_\text{RCT}$ 。

从因果图上，我们还可以得到另一种解释：人为干预的因子，会切断或削弱 协变量 $X$ 对于 $T$ 的影响：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-17537a66a57520fe7607ca75b10961d2_1440w.png" style="width: 60%;"/>
</div>

此时，原本存在的 confounding association 也就消失了。此时我们就可以架起观测变量到潜在变量的桥梁：
$$
\begin{aligned}
\text{ATE}_\text{RCT} &= \mathbb E[Y(1)] - \mathbb E[Y(0)] \\
&=\mathbb E[Y(1)|T=1] - \mathbb E[Y(1)|T=0]\quad\quad (\text{ignorablity})\\
&= \mathbb E[Y|T=1] - \mathbb E[Y|T=1] \quad\quad (\text{观测分布和干预分布一致性})
\end{aligned}
$$


### 条件可交换 Conditional Exchangeability

在之前理论中，我们研究过 Fork 类型的贝叶斯网，而在我们讨论 ATE 的模型中，它刚好也是一个 Fork 结构（causal association 不算边，我们还不知道它们之间要不要连边）。此时将， $X$ condition 住，根据之前的证明和 blocked path 的定义，此时 confouding assocaition 存在的路径是一个 blocked path，因此我们得到的 $T$ 和 $Y$ 独立，即 $T \perp Y | X$.

<div align=center>
<img src="https://picx.zhimg.com/80/v2-993ec06682f98c3a95ef43dd1b71f10d_1440w.png" style="width: 60%;"/>
</div>

由于独立，即协变量因子不存在，此时自然有下式成立：
$$
\begin{aligned}
\mathbb E[\text{ATE} | X] &= \mathbb E[Y(1) | X] - \mathbb E[Y(0) | X] \\
&= \mathbb E[Y(1) | T=1, X] - \mathbb E[Y(0) | T=0, X] \quad\quad (Y \perp T | X) \\
&= \mathbb E[Y | T = 1, X] - \mathbb E[Y | T = 0, X]
\end{aligned}
$$
因此，真实的 ATE 为：
$$
\begin{aligned}
\text{ATE} &= \mathbb E_X [\mathbb E[\text{ATE} | X]] \\
&= \mathbb E_X [\mathbb E[Y | T = 1, X] - \mathbb E[Y | T = 0, X]]
\end{aligned}
$$
通过上面的式子，我们就能计算出真实的 ATE 了，但是这个方法需要的样本量也不少，否则评估每一个 $X$ 取值的 期望可能就不够准确。

这时有的观众就要问了，你这个场景太简单了，如果是一个复杂的贝叶斯网，我要怎么办呢？我的回答是，此时可以使用 Pearl 后门调整算法，但是我有期末考试要准备，所以今天就先写到这儿了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8376d72ab1afd782ce2de0400454e824_1440w.png" style="width: 30%;"/>
</div>



----

## 参考资料

- https://zh.wikipedia.org/wiki/%E8%BE%9B%E6%99%AE%E6%A3%AE%E6%82%96%E8%AE%BA
- https://zhuanlan.zhihu.com/p/397796913
- https://zhuanlan.zhihu.com/p/343747851
- https://www.youtube.com/watch?v=dejZzJIZdow&t=11s
- https://www.degruyter.com/document/doi/10.2202/1557-4679.1203/html
- https://zh.wikipedia.org/wiki/%E9%9A%8F%E6%9C%BA%E5%AF%B9%E7%85%A7%E8%AF%95%E9%AA%8C
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235704/
- https://cse.sc.edu/~javidian/Notes_Presentations/BackFrontDoor.pdf
- https://www.hsph.harvard.edu/wp-content/uploads/sites/1268/2018/11/hernanrobins_v3.21.5.pdf