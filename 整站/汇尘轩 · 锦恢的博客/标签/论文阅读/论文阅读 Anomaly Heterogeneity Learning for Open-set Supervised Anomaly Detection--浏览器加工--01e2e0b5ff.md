

## 介绍

异常检测（abnormaly detection，简称 AD）的目的在于识别出偏离样本总体的数据点。大部分方法都将异常检测看成是一个同构分布检测，即，只存在正常分布和一场分布。这么做存在不合理的地方，那就是没有考虑到数据分布本身存在的异构性。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-0a450802f93c9cf6de7be03f5d8316f6_1440w.jpeg" style="width: 60%;"/>
</div>

在上面这张图中，左侧是大部分异常检测对于样本分布的看法，即，只将数据看成异常数据和正常数据。但是正常数据内的分布不一定是均匀的，其中也可能是异构的，本文认为右图中看待分布的角度更加合理，也就是不将数据分布看成是均匀的。而分成了若干块的异常数据分布，被称为异构异常分布，本文需要解决的问题就在于如何从训练集中建模这样的异构异常分布。

额外需要说明的是，本文认为工业实践中是很容易拿到异常样本的，因此本文的重点在于如何在有少量异常样本的情况下，克服常规 AD 算法认为数据分布均匀的错误假设，从而提高性能。文中的讨论范围是：训练时输入数据混有少量异常样本，测试时会出现从没出现过的异常样本和出现过的异常样本。这个问题也被称为 Open-set supervised AD (OSAD)



## 问题建模

假设训练集为 $\{(\omega_i, y_i)\}_{i=1}^N$，其中 $\omega_i\in\Omega \subset \mathbb R^{H\times W\times C}$ 代表一张 RGB 图像，标签为 $y_i \in \mathcal Y \subset \{0,1\}$ 。 $y_i=1$ 代表异常，否则代表正常。由于该方法在训练阶段只采用了有限的异常数据，所以可以认为大部分的 $y_i$ 都是 0。

对于一个给定的 AD 模型 $f(\cdot)$，记 $\bold x_i = f(\omega_i) \in \mathcal X$ 是第 $i$ 张图像在 AD 模型下的特征。为了更好地区分异常数据和正常数据，分别使用 $\mathcal X_n$ 和 $\mathcal X_a$ 代表正常数据和异常数据的特征，它们满足：
$$
\begin{aligned}
\mathcal X_n &= \{\bold x_1, \bold x_2, \dots, \bold x_N\} \\
\mathcal X_a &= \{\bold x_1, \bold x_2, \dots, \bold x_M\} (N \gg M) \\

\end{aligned}
$$
由于训练过程中，模型只能见到一小部分的异常数据，所以 $\mathcal X_a$ 所属于的类别集合 $\mathcal S$ 只是 $\mathcal C$ 的子集，其中 $\mathcal C$ 是所有可能的异常类别的集合。

AHL 的任务就是建模一个映射 $g:\mathcal X \to \mathbb R$，使得可以赋给从不同分布中采样得到的异常图像更高的分数。

## AHL 框架

AHL 框架的核心思想是在模拟的异常分布中学到一个统一的异构的异常模型。它由两部分组成：Heterogeneous Anomaly Distribution Generation (HADG) 和 Collaborative Differentiable Learning of the anomaly heterogeneity (CDL)。

AHL 的流程图如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1895ba3566dee1843835083a315a8d0a_1440w.jpeg" style="width: 80%;"/>
<p class="img-caption">AHL 流程框架</p>
</div>

基本的要点如下：

- HADG 负责从训练集中生成 $T$ 个异构分布数据集 $\mathcal T = \{\mathcal D_i\}_{i=1}^T$，其中 $\mathcal D_i$ 包含了正常数据集和从异常数据中随机采样的数据。每一个 $\mathcal D_i$ 都能从保证代表与其他不同的分布的角度生成。
- CDL 则用于构建上文提到的 $g$ ，也就是 $g(\mathcal T; \theta_g)$。这个统一的异构异常检测模型可以看成是 $T$ 个子模型的合成，$\{\phi_i(\mathcal D_i;\theta_i)\}_{i=1}^T$，每一个子模型 $\phi_i:\mathcal D_i \to \mathbb R$ 学习 $\mathcal D_i$ 代表的异常分布中的异常数据。其中 $\theta_g$ 和 $\theta_i$ 分别代表统一模型和 $T$ 个子模型的参数，$\theta_g$ 基于 $\theta_i$ 进行协同更新。
- 由于不同的子模型的性能差异可能会很大，所以在 CDL 中加入了一个模块用于提升 $\theta_i$ 在协同更新中的权重，如果该子模型的泛化误差较小（因为这样损失值就比较小）。某种正则化。
- 在推理阶段，只有统一模型 $g(\mathcal T; \theta_g)$ 用于进行推理。



### HADG

HADG 用于生成 $T$ 个异构异常分布的子数据集 $\mathcal D_i$. $\mathcal D_i = \mathcal X_{n,i} \cup \mathcal X_{a,i}$，其中 $\mathcal X_{n,i}\subset \mathcal X_n, \mathcal X_{a,i} \subset \mathcal X_a$。对于正常和异常数据，需要分开讨论采样方案。

对于正常数据 $\mathcal X_{n,i}$，需要让它能够代表正常数据的主体分布，且不同的子数据集的正常样本又不太一样，HADG 采用了 k-means 聚类，将 $\mathcal X_{n}$ 划分为 C 个簇，然后从这 C 个簇中的某一个中采样得到 $\mathcal X_{n, i}$

对于异常数据 $\mathcal X_{a, i}$，为了保证了异常样本的多样性，$\mathcal X_{a,i}$ 采样自两部分：

1. 从 $\mathcal X_a$ 中直接随机采样。
2. 伪异常样本生成算法。

为了符合 open set detection，HADG 将每个 $\mathcal D_i$ 划分为两个互斥的集合 $\mathcal D_i = \{\mathcal D_i^s, \mathcal D_i^q\}$，分别称为 support set 和 query set，分别用于训练和验证。并且保证两个集合中的正常样本属于两个完全不同的簇，两个集合的异常样本互不相同。



### CDL

CDL 用于对于 HADG 生成的 $T$ 个子样本构建子模型 $\phi_i$ 并将 $T$ 个子模型聚合为最终的异常检测模型 $g$。

首先对于子模型 $\phi_i$，通过下述的损失函数进行优化更新 $\theta_i$：
$$
\mathcal L_{\phi_i} = \sum_{j=1}^{\vert \mathcal D_i^s \vert} \ell_{dev}(\phi_i(\bold x_j;\theta_i), y_i)
$$
$\ell_{dev}$ 就是 AD 中常用的偏差损失：
$$
\ell_{dev} (\phi(\bold x;\Theta), \mu, \sigma) = (1 - y) \vert dev(\bold x, \mu, \sigma) \vert + y \max (0, a - dev(\bold x, \mu, \sigma)) \\
dev(\bold x, \mu, \sigma) = \frac{\phi(\bold x, \Theta) - \mu}{\sigma}
$$
其中的 $\sigma,\mu$ 是从样本中估计的总体的方差与均值。$a$ 用于规约显著性水平，文中选取为 5 。

更新完 $\phi_i$ 后，计算所有子模型在 query set 上的损失，并更新 $g$：
$$
\mathcal L_{cdl} = \sum_{i=1}^T\sum_{j=1}^{\vert \mathcal D_i^q \vert} \ell_{dev}(\phi_i(\bold x_j;\theta_i^t), y_i)
$$


### importance scores learning

为了解决不同子模型的性能差异过大导致更新不平稳等问题，文章中还提出了一个基于历史训练信息的 importance scores 用来修正 $\mathcal L_{cdl}$：
$$
\mathcal L_{cdl}^+ = \sum_{i=1}^T\sum_{j=1}^{\vert \mathcal D_i^q \vert} w_i^t \ell_{dev}(\phi_i(\bold x_j;\theta_i^t), y_i)
$$
原文认为这个权重 $w_i^t$ 需要能够反映子模型的泛化误差，泛化误差越小的模型需要补偿因为泛化误差减小而导致的损失值的降低。为了更好地度量这个语境下的泛化误差，本文提出了基于历史训练信息的 importance scores learning。既然是基于历史训练数据，因此，importance scores 被建模为了关于 epoch 的时间序列模型。

假设第 $j$ 轮中 $T$ 个子模型输出的异常分数为 $\bold s_j = \{s_{ji}\}_{i=1}^T$，维护一个长度为 $K$ 的队列：
$$
\bold S_j^t = [\bold s_j^{t - K}, \cdots, \bold s_j^{t-2}, \bold s_j^{t-1}]
$$
时间序列模型 $\psi(\cdot)$ 需要基于过去 K 轮的异常分数评估结果，去回归当前这一轮的异常分数，记 $\hat{\bold s}_j^t$ 是以队列中的历史分数预测的当前分数：
$$
\hat{\bold s}_j^t = \psi(\bold S_j^t;\theta_\psi)
$$
通过简单的自回归来完成序列模型的参数更新，损失函数为：
$$
\mathcal L_{seq} = \sum_{\bold x_j \in \mathcal D} \mathcal L_{mse} (\hat{\bold s}_j^t, \bold s_j^t)
$$
训练好序列模型后，使用预测的异常分数和标签的损失，就可以定义出 $\phi_i$ 的泛化误差了：
$$
r_i^t = \frac{1}{\vert \mathcal D' \vert} \sum_{\bold x_j \in \mathcal D'} c_j \mathcal L_{mse}(\hat{s}_{ji}^t, y_j)
$$
其中 $\mathcal D' = \mathcal D \setminus \mathcal X_{n,i}$， $c_j$ 是一个与类别有关的预定义参数。如果 $\bold x_j$ 是一个没见过的异常数据，那么 $c_j$ 会被赋予更大的值，用来提升该样本的权重，从而增强模型对于未见过的异常点的异常检测能力。

因为泛化误差越小，补偿要越大，因此，最终的权重为 $T$ 个子模型的泛化误差负数的 softmax
$$
w_i^t = \frac{\exp (-r_i^t)}{\sum_i^T\exp (-r_i^t)}
$$
将补偿权重带入 $\mathcal L_{cdl}^+$ 从而完成聚合检测器 $g$ 的更新。训练阶段结束。

---

## 实验结果

### 数据集

文章在 9 个真实数据集（5 个工业缺陷检测数据集、1 个行星探测数据集和 3 个医学数据集）上进行了测试。

### baseline

使用的基准模型为5个结果相近的SOTA模型：MLEP，SAOE，FLOS，DevNet 和 DRA。

- MLEP, DevNet & DRA：专门用于 OSAD 任务的模型。
- SAOE：基于合成异常和离群点曝光数据增强的监督检测器。
- FLOS：基于 focal loss 的类别不平衡分类器。

### 本文方法作为插件使用

AHL 作为插件施加在 DevNet 和 DRA 上面，作为评估本文方法的载体。

### 评估指标

评价指标采用 AUROC，最终评价指标为三次独立运行后的指标的平均值。 

### general & hard setting

为了评估不同异常点模式下 AHL 的性能，文中定义了两种不同的 setting ，不同的 setting 对于训练集中异常点的选取规则不一样。

- general setting：训练集的异常点随机抓取。
- hard setting：训练集的异常点只会从一个异常类中抓取。

### general setting 实验结果

general setting 的实验结果如下，从中可以反应出 AHL 对于常规异常检测任务的性能提升。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-11f83cd5fb6a85a16665e391b235b66b_1440w.jpeg" style="width: 80%;"/>
    <p class="img-caption">实验结果(general setting)。最好的结果和第二好的结果分别使用 <span style="color:red">红色</span> 和 <strong>加粗</strong> 表示</p>
</div>

从结果可以看到，增加了 AHL 插件的两个 SOTA 模型的性能都比原本的在 9 个数据集上有所提升。而且从整体的性能而言，增加了 AHL 插件的两个增强模型取得了所有数据集上的最佳性能。

### hard setting 实验结果

hard setting 的实验结果如下，从中可以证明 AHL 对于没有见过的异常类的场景的性能提升。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-33f895d9a07fc701e9c99ae8ac4fb40e_1440w.jpeg" style="width: 80%;"/>
    <p class="img-caption">实验结果(hard setting)。最好的结果和第二好的结果分别使用 <span style="color:red">红色</span> 和 <strong>加粗</strong> 表示</p>
</div>

结果也是非常不错的。

### cross-domain AD test

为了更进一步证明 AHL 的泛化性能，文章还进行了一项特殊的实验，使用 安装了 AHL 的 DRA 测试 cross-domain AD 任务。简单说就是，让模型现在一个类别上使用 OSAD 任务的定义训练好参数，然后只用另一个新的 domain 内的数据集中的正常图像去 finetone 模型。测试时，在这个新的 domain 上进行异常检测任务。

结果如下所示。在表格左侧，列上为 Grid，行上为 Carpet 的单元格为 0.983，代表在 Carpet 这个类别上训练，在 Grid 上仅用正常样本 finetone 的模型，在 Grid 上面的 AUROC 为 0.983。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f9482dfebd8ca3c62cfe7bb8a86e639f_1440w.jpeg" style="width: 80%;"/>
</div>

从结果可以看到，整体上， AHL 可以让 DRA 在大部分的测试对中取得更好的指标。

### ablation

为了验证不同模块的有效性，补偿权重的有效性，本文的消融实验如下。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ac02068b6d8b3266d0502630fea5e005_1440w.jpeg" style="width: 80%;"/>
</div>

可以看到， HADG 数据分割策略可以在大部分数据集上有效涨点。本文还提到，如果只是加 CDL，会让性能在大部分数据集上下降。

### 超参数讨论

metric-超参数的diagram如下所示：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-adfb934f10ab1c2ff308c006012e41c5_1440w.jpeg" style="width: 80%;"/>
</div>

- HADG 中划分正常样本的聚类簇中心数量 $C$： 选取 $C = 3$，该值越大，性能不一定会上升，因为过多的簇会导致采样的过程引入额外的偏差，因此，不建议该值太大。
- CDL 中的维持历史训练记录的队列的长度 $K$：选择 $K = 5$，选择 $K = 3$ 效果就很好了，但是选取 5 可以在所有数据集上都取得很好的效果。（这个值明显越大，计算和内存负担越大，显然是一个工程 trick 了）