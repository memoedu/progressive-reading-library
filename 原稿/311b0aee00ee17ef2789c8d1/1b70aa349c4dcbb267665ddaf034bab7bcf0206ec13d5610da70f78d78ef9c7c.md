# 实验目标

## 摘要

本次实验目标为实现至少两种目标检测方法。任务自拟。

在本次实验中，我采用了Faster-RCNN和YOLOv5两种模型来进行目标识别和性能对比。任务为反恐精英中的警匪预测。我自制了反恐精英的警匪识别的小型数据集并在COCO2017数据集上预训练的Faster-RCNN和YOLOv5进行了微调并得到了相应的实验结果；最后我们还对一段预先录制的游戏视频的每一帧进行了目标检测，并将对比结果剪辑成了视频方便直观感受模型之间的差异。


## 任务介绍

本次任务为“反恐精英警匪目标检测”，很不专业，那就对了，因为是我随便起的名字。简单介绍一个这个任务吧。这是一个多目标检测的任务，需要检测的目标分为5类：

| 标签   | 解释           | 序号 |
| ------ | -------------- | ---- |
| null   | 背景（保留类） | 0    |
| C      | 反恐精英       | 1    |
| dead   | 尸体           | 2    |
| T      | 恐怖分子       | 3    |
| weapon | 武器           | 4    |

下图展示了其中的某一张训练集中的图片和对应的标签。

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220603212706.png" style="width: 80%;"/>
<p class="img-caption">反恐精英警匪目标识别示意图（第一张训练集和对应的标签）</p>
</div>

---


# 数据集

由于网上并没有关于CS1.6的，满足实验要求的数据集，所以我选择自己制作并标记数据。大致过程分为数据采集、数据标记和数据组织三个步骤。数据我已经开源在了Kaggle上[cstrike detection | Kaggle](https://www.kaggle.com/datasets/lstmkirigaya/cstrike-detection)，欢迎下载：

## 数据采集

数据搜集环节其实就是打了半个小时CS1.6，打得很开心。顺便开个脚本，选定窗口区域，每隔10秒采集一次图像。最终得到了207张图片。

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220603220854.png" style="width: 80%;"/>
<p class="img-caption">收集数据</p>
</div>

## 数据标记

数据标记方面，使用开源脚本软件`labelImg`进行，安装方式：

```bash
python -m pip install labelImg
```

安装目录会在`Scripts`文件夹下，所以可以直接输入软件名打开：

```bash
labelImg.exe
```

使用方式很简单，首先简单配置一下本次实验需要的数据格式：点击view开启自动保存，然后点击左边栏的保存格式，选为YOLO。

下面开始打标签，按`w`开始框选，框完之后填入标签名称即可，按下d进入下一张图片。使用截图如下：

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220603222930.png" style="width: 80%;"/>
<p class="img-caption">使用labelImg标记数据</p>
</div>

## 数据组织

目标识别的标签表示方式有很多种。本次实验采用YOLO的标签风格。

### 目标识别任务的标签

对于任何一张图像而言，它的目标由两部分组成：

- 框住目标物体的最小矩形，称为bounding box，简称bbox
- 每一个bbox框选的区域所属的类别label

很明显，bbox至少需要四个数字描述，label至少需要一个数字。

### YOLO风格的标签

YOLO风格的标签采用$(x,y,w,h)$四个量描述，它们取值范围都是0-1，分别代表bbox的中心点的坐标x、坐标y、宽和长。之所以取值范围是0-1，是因为进行了归一化。一个YOLO标签风格的标注如下图所示：

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220603223904.png" style="width: 80%;"/>
<p class="img-caption">YOLO bbox数据格式演示</p>
</div>

在组织数据方面，一张图片对应一个`.txt`，YOLO风格标签一行一个，代表一个bbox。下图为`20220001.png`对应的YOLO风格标签：

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220603224909.png" style="width: 90%;"/>
<p class="img-caption">数据组织</p>
</div>

### 整体数据组织协定

主流的数据整理方式有两种，基于元文件的的和基于文件夹的。我们此处采用基于元文件的组织方式，因为它对于时常变化的自制数据集来说特别有用。假设我们的数据集放在`datasets`文件夹下，那么请遵守如下的约定：

- datasets下存在两个文件夹`images`和`labels`，文件夹里的文件数量必须相同，`images`里的一个`.png`文件对应`labels`里的一个`.txt`文件，且文件名完全相同。
- datasets下还存在`train.txt`和`test.txt`，`train.txt`里写着训练集对应的每张`images`下图片的绝对路径；`test.txt`同理。

---

# 目标检测模型

本次实验挑选了Faster-RCNN和YOLOv5，挑选它们的原因也很简单，它们代表了基于深度学习的目标检测算法中的两种主要算法。

## two-stage——Faster-RCNN

Faster-RCNN是RCNN家族的作品，由Ross B. Girshick在2016年提出。RCNN家族的模型大致思想为：先根据某种算法得到相应的候选框，然后对于每一个候选框单独预测其所属类别。正因为RCNN系列模型都需要先得到候选框，然后再分类，所以被称为two-stage的模型。

迭代到Faster-RCNN后，模型在诸多方面都有了优化。

<div align=center>
<img src="https://pic4.zhimg.com/v2-e64a99b38f411c337f538eb5f093bdf3_r.jpg" style="width: 90%;"/>
<p class="img-caption">Faster-RCNN结构</p>
</div>

Faster-RCNN的整体结构可以分为四个模块：特征提取层、候选框生成层（RPN）、ROI Pooling和分类层

### 特征提取层

对于输入的图片，Faster—RCNN会先将图片resize到固定大小（$800\times600$）。然后再通过最初的特征提取层（原文中为13个卷积层和4个池化，激活函数为relu）得到高层的特征图`Feature Map`。

> 反正能提特征就行，我的项目中使用的是50层的resnet

### 候选框生成层

候选框生成层，也称为RPN是Faster-RCNN的创新点之一，之前的RCNN方法的候选框生成算法基本都是基于规则的生成法，这些方法都非常耗时，在Faster-RCNN中，直接采用了一个小的分类网络来判断当前的框框是否会成为候选框。也就是下图的子网络：

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-1ab4b6c3dd607a5035b5203c76b078f3_720w.jpg" style="width: 90%;"/>
<p class="img-caption">RPN计算每个像素对应的9个锚点是否包含目标的子分类网络</p>
</div>

其中之前得到的特征图在经过额外的卷积后会在经过上图中的$1\times1$卷积，该卷积的目标特征图通道数为18。我们都知道$1\times1$卷积不会改变原本的特征图的尺度，只会改变通道数，所以为什么要这么做呢？这就必须提到Faster-RCNN中的锚点（anchors）了。

#### 候选框存在性分类

为了从多尺度的角度来得知特征图上每个像素点的周遭区域的候选框是否存在，如果存在应该多大，RPN在默认情况下就会为**每个特征图的像素点**分配9个锚点，这9个锚点可以描述为$(x_1, y_1, x_2, y_2)$，代表了9个固定大小的bbox，而且它们都是以该像素点为中心的。在原项目中，这9个锚点是：

```python
[[ -84.  -40.   99.   55.]
 [-176.  -88.  191.  103.]
 [-360. -184.  375.  199.]
 [ -56.  -56.   71.   71.]
 [-120. -120.  135.  135.]
 [-248. -248.  263.  263.]
 [ -36.  -80.   51.   95.]
 [ -80. -168.   95.  183.]
 [-168. -344.  183.  359.]]
```

这9个锚点的大小关系如下图所示：

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-7abead97efcc46a3ee5b030a2151643f_720w.jpg" style="width: 40%;"/>
<p class="img-caption">RPN为每个像素点固定分配的9个锚点</p>
</div>

每个锚点就是一个小的bbox，为了知道每个是否包含目标子区域，这个问题就转化成了一个二分类问题了。如果包含，那么该锚点就是正类，否则为负类。而基于Softmax的k分类需要k个值被转化为概率，那么此处对于每个锚点的二分类就需要2个值，而每个像素点对应9个锚点。所以为了分类得到每个像素点的所有锚点是否含有目标，需要$2\times9=18$个值，这是**逐像素的判断**，所以不需要改变特征图尺度，只需要改变特征图的通道数（这个trick也是CV里面很常用的）。因此此处将卷积核设置成尺寸$1\times1$，数量为18的原因就是这样。

除了挑选合适的候选框外，RPN还初步对每个候选框进行了偏移量的回归，因为原本选的候选框可能框多了也可能框小了，这个地方需要进行一定的矫正。

<div align=center>
<img src="https://pic3.zhimg.com/80/v2-1908feeaba591d28bee3c4a754cca282_720w.jpg" style="width: 90%;"/>
<p class="img-caption">RPN计算每个像素对应的9个锚点的偏移量</p>
</div>

由于每个锚点对应四个量，修正一个锚点也就需要4个偏移量，而且这个操作也是逐像素的，所以候选框回归需要将原特征图的尺度不变，通道变为$4\times9=36$，因此入口处的卷积的尺寸为$1\times1$，数量为36。

> # Faster-RCNN在RPN上的两个trick：1. 在特征图上选候选框而不是原图 2. 对偏移量做回归而不是锚点在特征图上的绝对坐标



#### 候选框回归

事实上，上面的“偏移量”是为了方便理解提出的。实际上，在候选框回归这一块上，每个候选框对应的四个量是四个用于做线性变换的参数。这也是Faster-RCNN理解起来较为费劲的地方，我来尝试解释一下。

首先，我们记需要修正的候选框为$A=(A_x,A_y,A_w,A_h)$，ground truth为$G=(G_x,G_y,G_w,G_h)$。当然，我们肯定无法完全修正到$G$，我们记$A$修正后的结果为$G'$，我们当然希望$G'\approx G$，如下图所示：

<div align=center>
<img src="https://pic2.zhimg.com/80/v2-ea7e6e48662bfa68ec73bdf32f36bb85_720w.jpg" style="width: 40%;"/>
<p class="img-caption">将候选框A修正到G</p>
</div>

一种可行的$F:A\to G’$可以写成：
$$
\begin{aligned}
G_x' &= A_w \cdot d_x(A) + A_x \\ 
G_y' &= A_h \cdot d_y(A) + A_y \\
G_w' &= A_w \cdot \exp(d_w(A)) \\
G_h' &= A_h \cdot \exp(d_h(A))
\end{aligned}
$$
其中$d_x, d_y, d_w, d_h$都是$\mathbb R^4\to\mathbb R$。那么$d_*$怎么求呢？

在这里，Faster-RCNN有一个假设：$A$和$G$差得不是很多时，$d_*$就是一个线性变换。不过在原文中，为了尽可能地end-to-end，该线性变化构造如下：
$$
d_*(A)=W_*^T\cdot \phi(A)
$$
其中的$\phi$也是一个线性变换，相当于做了两层。我们下面需要学习的是$d_*$，那么就需要将GT对其到$d_*$，我们记对齐后的结果为$(t_x,t_y,t_w,t_h)$，根据上面的式子，$G\to t$显而易见：
$$
\begin{aligned}
t_x &= \frac{G_x-A_x}{A_w} \\
t_y &= \frac{G_y-A_y}{A_h} \\
t_w &= \log\frac{G_w}{A_w} \\
t_h &= \log\frac{G_h}{A_h} \\
\end{aligned}
$$
那么为了让$G'$去靠近$G$，就需要让$d_*$去靠近$t$，我们记当前的像素的第$i$个候选框对应的候选框坐标$A$和相对变换量$t$分别为$A^i$和$t^i$。所以我们有损失函数：
$$
\mathcal L=\sum_{i=1}^N|t^i-W^T_*\phi(A^i)|
$$
其中$N$是候选框的数量，这个值等于$9\times H'\times W'$，其中$H'$和$W'$代表特征图的高度和宽度。加上正则化，那么第一次对候选框进行回归的损失函数为：
$$
\mathcal L=\sum_{i=1}^N|t^i-W^T_*\phi(A^i)|+\lambda ||W_*||
$$

#### Proposal Layer

在经过对候选框的分类和候选框的回归后，有必要整合这些结果来得到需要送入后续网络做进一步处理和分类的特征图与候选框的子集了，Proposal Layer就是干这件事情的。

Proposal Layer接收三个输入：

- 对于每个像素点的9个候选框是正类还是负类的分类结果，对于每个像素点，这是一个18维向量。
- 对于每个像素点的9个候选框的bbox的修正变换量，对于每个像素点，这是一个36维向量。
- im_info，它记录了主干网络将原图下采样了多少倍，以及输入图像（resize后的）的尺寸。

整合的步骤如下：

1. 首选根据之前说的变换公式利用bbox的修正变换量对候选框做回归。
2. 根据候选框在positive的得分，在所有候选框中选取positive分量最大的`pre_nms_topN`个候选框。
3. 去除面积小于一定阈值的候选框。
4. 根据im_info，判断是否有候选框在修正后超出了原图，去除这些候选框。
5. 对剩余候选框进行NMS，继续去除冗余候选框。
6. 将剩余候选框对应的修正后的bbox输出。

在RCNN系列中，我们将上述删选得到的bbox称为proposals。

> # 由于需要基于固定的锚点来生成候选框，RCNN系列的方法也被称为anchor-based的目标检测



> ## 特别需要注意，由于im_info信息的注入，输出的proposals的$(x_1,y_1,x_2,y_2)$是对应$M\times N$的原图的坐标而不是特征图`Feature Map`的！

### ROI Pooling

ROI Pooling负责将RPN输出的proposals对应到相应的特征图的区域，得到proposal feature maps，送入后续的网络进行分类。很明显，ROI Pooling有两个输入：

- RPN输出的proposals
- 特征图`Feature Map`

所以如何从特征图中挖出proposals对应的子区域，并保证大小完全相同？最粗暴的方式为crop和warp，如下图所示：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-e525342cbde476a11c48a6be393f226c_720w.jpg" style="width: 90%;"/>
<p class="img-caption">crops和warp</p>
</div>

这两种方法要么会破坏原图的完整信息，要么会污染原特征图的结构信息。

Faster-RCNN采用了ROI Pooling，这是一个动态调整步长的池化操作，不同于普通的池化，构造一个ROI Pooling不需要指定步长，只需要指定目标特征图的大小。因为ROI Pooling的区域划分是通过计算分出的。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-e3108dc5cdd76b871e21a4cb64001b5c_720w.jpg" style="width: 75%;"/>
<p class="img-caption">ROI Pooling</p>
</div>

ROI Pooling的输入是proposals对应在特征图上的子区域。通过ROI Pooling，每一个proposals都对应到原特征图的一个子区域，而且子区域的大小全部相同。

比如在原本的论文中，ROI Pooling的参数设为固定输出$7\times7$大小的特征图，那么在向量化后，每个proposal都对应一个49维向量。

### 分类

在ROI Pooling结束后，我们得到了每个proposals对应的49维向量，每个向量都会经过若干个全连接层得到一个特征向量，该特征向量被用于两个用途：

- 通过softmax来预测该proposal对应的区域是哪个类
- 通过一个全连接层来进行回归，得到bbox的偏移量，以此首先第二次bbox修正

<div align=center>
<img src="https://pic2.zhimg.com/80/v2-9377a45dc8393d546b7b52a491414ded_720w.jpg" style="width: 80%;"/>
<p class="img-caption">Faster-RCNN最终分类网络</p>
</div>

结束后，将上图的`bbox_pred`加上`proposals`就可以得到预测出的`bbox`，然后`cls_prob`最大值的索引就是`bbox`对应的类别了。

这就是Faster-RCNN的全流程了。

---

## one-stage——YOLO v5

YOLO v5是YOLO家族的较新的成果，YOLO系列（You only look once）也是如今实用性最强的目标检测算法之一，这主要得益于它端到端的设计使得YOLO系列的目标检测速度都非常快，内存运行时也很小，这使得使用YOLO在嵌入式等平台进行部署时非常香。

如果了解了Faster-RCNN之后，再去看YOLO就很简单了。Faster-RCNN的结构对于小白而言实在有点复杂，其中很多步骤还是不是那么得“深度学习”，深度学习基本都是只关心输入和输出，其余的都交给神经网络来“端到端”。YOLO的想法其实就是将目标检测做成端到端。

### YOLO v1

第一个版本的YOLO结构非常简单，如下图所示：

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-5d099287b1237fa975b1c19bacdfc07f_720w.jpg" style="width: 90%;"/>
<p class="img-caption">YOLO 结构图</p>
</div>

中间的卷积和全连接都不重要，我们只看最后的输出。了解了YOLOv1输出的编码格式，也就明白了YOLOv1的工作原理。输出为$7\times 7\times 30$。其中$7\times7$是特征图大小，如果把原本输入的图像等距划分成$7\times7$的窗口，那么输出的特征图的每个点就对应原本的一个窗口；30是含有当前窗口物体检测、分类、bbox框的信息的集合，它的计算如下：
$$
30=20+2\times(1+4)
$$
其中20代表总共的类别，YOLOv1使用的数据集的检测实体只有20个类别；2代表预定的锚点框的个数，这个数字在Faster-RCNN中是9个，在YOLO v1中是2个；1代表当前这个锚点框的置信度，置信度太低的bbox将会被认为不包含前景物体；4就是描述一个锚点框的$(x,y,w,h)$。

那么在推理时，当我们得到了最终的输出张量后，就可以先根据$7\times7\times20$得到每个块中所有bbox的预测类别，然后在各个类别预测的结果中，根据置信度和NMS过滤掉一些框。将最终的结果画在输入图像上就行（YOLO中bbox的尺寸都是归一化的，所以可以不用做转换直接输出）

### YOLO v5

之后的YOLO v2 v3 v4 v5都是基于YOLO v1的框架进行了一些增量改进，细节较多，但是新的idea不是很多，因此此处直接上今天的主角——YOLOv5了。

YOLO v5的结构图如下：

<div align=center>
<img src="https://img-blog.csdnimg.cn/20200815154041184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbjM1NTY1NTYwMA==,size_16,color_FFFFFF,t_70#pic_center" style="width: 90%;"/>
<p class="img-caption">YOLO v5 结构图</p>
</div>

其实YOLO v5的整体结构也是非常清楚的，就是一个从backbone中进行分层抽取的特征金字塔（FPN），我们直接说YOLO v5和YOLO v1的区别吧：

- YOLO v1划分一个$7\times7$的窗口，粒度太大，小物体很容易检测不到，特别大的物体可以也无法检测；YOLO v3开始使用了FPN，也就是采用三种划分策略，分别为$13\times13$的、$26\times26$的和$52\times52$的，产生相应的张量的通道是通过FPN的方式组织起来的。这样，网络对于大中小的物体都可以很好地检测。在v5中，窗口进一步优化为了$19\times19$、$38\times38$和$76\times76$。
- 锚点框的先验尺寸上，v1采用了固定的大小。v2开始，模型在训练前会采用kmeans在训练集上进行聚类，根据聚类结果的簇中心来选择先验框的尺寸，这一做法延续到了v5。
- v1的bbox回归计算是直接计算出一个归一化的绝对坐标，这一做法在v3中开始变成计算当前的bbox相对于当前这个网格的左上角的相对坐标，这一做法延续到了v5。
- v5的训练中的加载器，会按照概率传递数据增强后的训练集，增强方式：**缩放，色彩空间调整和马赛克增强**。
- 架构变化，YOLO v1的网络就是LeNet风格的简单的卷积网络；v5中加入了大量的优化后的模块和架构，不仅可以降低参数量，推理时间，还拥有更大的感受野和精度。

> 需要说明，v4和v5发布前后相差仅两个月，大部分人认为v4和v5是属于同一个层级的YOLO改进，因此，此处就不谈v4了

### Backbone CSP

CSP全称为[Cross Stage Partial Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1911.11929.pdf)，这是v5的backbone的主要结构，它的结构如下：

<div align=center>
<img src="https://img-blog.csdnimg.cn/img_convert/5a173591c11da50635187eb1b6537e17.png" style="width: 100%;"/>
<p class="img-caption">DenseNet和CSPDenseNet的示意图</p>
</div>

可以看到，和DenseNet结构不同，CSP只对输入特征的一部分进行DenseNet操作，剩余会和DenseNet的输出结果在最后拼接。这么做可以有效缓解梯度消失；由于网络参数被重复利用了，也能减小参数量；之前也有研究表明，在DenseNet上进行优化会更加容易。

在YOLO v5中的C3_i_j模块就是CSP的结构。

### Focus模块

这是一个很好的思路，Focus模块的作用是在不改变特征图数据量的前提下，仅通过拆分和拼接，将输入特征图的尺寸变小，但是通道数增加。大致的做法如下图所示：

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/20220608000905.png" style="width: 75%;"/>
<p class="img-caption">Focus模块</p>
</div>

我觉得图比文字清楚很多，因此就不再阐述了。在原论文中，将一张特征图像左图一样对某个位置（相同颜色的）进行分割和重组的过程被称为slice。很明显，上图中一共进行了四个slice，因为有四种不同的颜色。	

这么做的目的是可以大幅增加通道数，并且看起来完成了下采样，使用一个Focus，性能可以抵得上三个卷积+池化层的组合，并且参数量大幅下降。

### 其余模块

剩余模块基本都是基于上述思想和CV中常用的设计经验的改进，一个完整的结构图和相应的模块的细节如下：

<div align=center>
<img src="https://pic1.zhimg.com/v2-1c3b54eb417a17cbb79334e96b82acec_r.jpg" style="width: 90%;"/>
<p class="img-caption">YOLO v5剩余模块结构</p>
</div>

---

# 训练过程

项目已经开源：[LSTM-Kirigaya/cstrike-detection](https://github.com/LSTM-Kirigaya/cstrike-detection)

训练过程方面，无论是Faster-RCNN还是YOLOv5都采用了非常统一的项目组织方式。其中Faster-RCNN基于`torchvision`提供的Faster-RCNN实现，YOLOv5采用了官方`ultralytics`的代码进行训练，我修复了在原本代码在Windows下运行的一点bug。

具体的运行命令请参考我们的代码的README。请在成功配置完数据后再开始训练或者检测。

## 训练配置

对于Faster-RCNN和YOLOv5，都使用学习率为0.0001的Adam优化器迭代30轮进行finetone。数据方面使用187张作为训练集，20张作为验证集。最终只保存最后一次迭代后的权重和所有权重中验证集上误差最小的那个权重。

## 训练使用

每个算法都一个同名文件夹，想要训练只需要在命令行下进入文件夹，并运行文件夹下的`train.py`即可，具体的运行参数请通过如下命令获取：

```bash
python -u train.py --help
```

训练结束后，会在`run/train/expi`中存下训练记录，在`run/train/expi`中保存最后一次的权重和测试损失最小的权重。

---

# 实验结果

## 推理时间和模型大小

> 此处的推理时间指神经网络计算时间，图像前后处理和IO的时间均不计，且都是在Python解释器运行时下的推理时间。平台为i7-9700和RTX3060

| 模型名称    | 推理时间(ms) | 模型大小(MB) |
| ----------- | ------------ | ------------ |
| Faster-RCNN | 78.51        | 158          |
| YOLO-v5m    | 20.2         | 40.2         |

<br>

## mAP

本实验采用mAP@0.5，因此，IoU阈值为0.5。得到的P-R曲线如下：

<div align=center>
<img src="https://fastly.jsdelivr.net/gh/LSTM-Kirigaya/KImage/Img/pr.png" style="width: 90%;"/>
<p class="img-caption">Faster-RCNN和YOLO-v5的P-R曲线</p>
</div>

此处的mAP采用mAP@0.5、加权mAP@0.5两个指标和平均bbox_loss来评估。其中bbox_loss采用标准化空间的L2范数进行度量。

| 模型名称    | mAP@0.5 | 加权mAP@0.5 | bbox loss |
| ----------- | ------- | ----------- | --------- |
| Faster-RCNN | 0.725   | 0.190       | 0.014     |
| YOLO-v5m    | 0.389   | 0.115       | 0.017     |


<br>
<br>

## 视频检测效果 Faster-RCNN 和 YOLOv5 对比 


<div align='center'>
<iframe
height=600 width=100%
allowfullscreen=true
src="https://player.bilibili.com/player.html?bvid=BV1dB4y1X77G"
></iframe>
<p class="img-caption">demo</p>
</div>


## 总结

从数据和视频效果对比中可以看出，YOLO拥有更小的占用空间和更快的推理速度，但是在预测精度上，无论是对类别的预测精度还是单个bbox的位置回归精度都不如Faster-RCNN。这是正常现象，毕竟Faster-RCNN是two-stage方法，且在推理过程中会产生更多的proposals，因此Faster-RCNN推理速度更慢，但是精度更高是合理的事情。

---

# 总结

在本次实验中，我们完成了基于目标检测的反恐精英警匪识别，基于这套识别系统，我们可以很快地完成纯视觉的高精度自动瞄准开火系统。而且在对比Faster-RCNN和YOLOv5中也发现了trade-off——精度、模型大小和运行时间之间存在需要协调的地方。

## 不足

但是本实验还存在诸多不足，我谨此列举部分我只是范围内的缺点和可能的改进措施：

1. 分类的损失函数中很有必要加入代价敏感函数，很明显，没有检测出尸体和没有检测出敌人的损失是不一样的；讲武器检测为尸体和将队友检测为敌人的代价是不一样的。
2. 对于bbox需要加入一定的负样本来压制多余框的产生。对于一些很难出现的场景，比如人物飞出去、弹壳弹飞的场景下，模型总会将这些没有出现在训练集中实体判断为某个类，这是需要克服的。
3. 警匪在低阶图像上就存在一定的差异，可以通过一些统计学的方法来加大这个差异，减少误判的概率。
4. 所有的物体在时间上都是连续的，但是基于单帧图像识别的算法不能很好地利用物体在时间上的连续性，从而造成bbox的不稳定，经常闪烁出现，这不利于稳定的火力输出。可以考虑基于稀疏光流这样的方法对得到的bbox区域进行跟踪（假设bbox内为非角点区域）
5. 数据收集其实作了两轮，第一轮其实并没有武器这个标签，这是第二轮加进去的。而且第一轮中，队友经常倍检测为敌人，因为类别严重不平衡。所以我又加标了100张图片，这个过程中，很明显没有必要使用那么多标签（比如武器很多，可能只要两张就能把模型喂饱了，但是我不知道）。所以考虑使用主动学习或许可以大大降低需要标记的数据集大小。

## 未来的工作

目标识别只是枪械战斗体系统中很小的一环，想要实现一个能够独立完成战斗的智能体还有许多要做，包括但不限于：

- 决策系统：这可以通过强化学习和模仿学习来实现，得到的决策系统应该是层次化的，有类似于语义槽的方向判断和槽值的具体步骤判断。
- 群体智能：优秀的战士都是懂得合作的，如何让智能体之间相互协作来更加高效地达到目标也很重要，是采用中心化的控制系统还是（半）去中心化的协作系统。

---

# 参考

[1] [一文读懂Faster RCNN - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/31426458)

[2] [目标检测|YOLO原理与实现 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/32525231)

[3] [全网呕血整理：关于YOLO v3原理分析 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/345073218)

[4] [一文读懂YOLO V5 与 YOLO V4 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/161083602)

[5] [目标检测yolo系列之yolov5深度讲解（一） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/159371985)