<div align='center'>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=2096764115&auto=1&height=66"></iframe>
</div>

## 任务描述

> 在讲解如何使用 `voxelmorph` 进行图像配准之前，先简单讲讲图像配准这个任务本身的定义。倘若阁下知晓它的定义，不妨直接跳过这一章。

配准（registration）指这么一类任务，将不同时间，设备条件，采集参数下对同一信号源采集的信号进行对齐。而图像配准（Image registration）则指当信号为图像时的特殊情况。比如下面这张图中的两个信号，显然是从同一个信号源采集的：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-940ff5857d068c9dee553665c5421a9d_1440w.png" style="width: 50%;"/>
</div>

我们要做的事情就是设计一个算法，将这两张图像对齐。对齐总需要参考系吧？参考系怎么找？这就涉及到图像配准的两类方法了。

> 图像配准只是影像学配准中的一个子任务，更加广义且具有极强使用价值的是多模态数据配准。在脑科学中，如何将不同模态的脑影像这是一项困扰了研究人员20多年的问题，这是建立不同生物大脑模型的一项非常关键的技术。此乃后话，倘若锦恢未来从事相关研究，会尝试分享一些对应的研究进展。今天的主角仍然是医学影响，特别是三维影像 CT 的配准讲解。

### 相对配准

相对配准就是将两张输入图像的一张的坐标表示作为参考系，另一张图像向这张图像对齐，比如上面那张图，我们把左边的图作为参考图像，相对配准要做的事情，就是将右图对齐/配准到左图。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7aa308eb525dbb84b42faf057c16b925_1440w.png" style="width: 50%;"/>
</div>

在后续讲解的 voxelmorph 方法就是使用的相对配准，推理阶段你需要输入两张图像，一张作为固定图像（fixed image），另一张会被对齐到这个 fixed image 上。

### 绝对配准

绝对配准就是找到一个对于当前任务全局统一的控制网络，然后通过分别完成各分量图像的[几何校正](https://baike.baidu.com/item/几何校正/0?fromModule=lemma_inlink)来实现坐标系的统一（摘自网络）。因为和本文方法不一致，于此不再赘述。

### 配准的输入输出

明确输入输出对于配准这种底层算法而言非常重要。假定我们使用相对配准技术，显然输入是两张图像，即需要被配准的图像和参考图像。对于基于深度学习的端到端的配准算法而言，输出当然是配准之后的图像了，但是在很多情况下，只是知道被配准后的图像是什么样子的，而不知道 **原图是怎么被配准到结果上** 是不够的。因为在很多需要图像配准的任务中，是存在坐标，也就是图形学的概念的，比如无人机勘探，手术机器人导航。将配准算法嵌入到整套系统中后，我们不仅仅需要知道图像被配准后的样子（标准化），还需要知道原本图像上某个点的坐标在被配准后会出现在什么地方，这对于度量配准质量，数据标准化而言非常关键。

在图像配准中，为了描述 **原图是怎么被配准到结果上** ，一个最简单的想法就是不仅仅输出配准图像，还输出被配准图像到配准后图像的形变场 （deformation field），在许多文献中，研究人员似乎更喜欢使用 warp 这个单词来描述这个形变过程或者形变结果。为了节省字数，后续 deformation field 和 warp 在不引起歧义的情况下是一个意思。

我们假设输入图像是一个 $m \times n$ 的图像，那么这张图像的形变场就是一个 $m\times n \times 2$ 的张量，原本的图像每一个像素点被描述为一个二维向量，代表这个像素点在被配准后的移动位置。形式化的表示为：
$$
p' = p + u(p)
$$
其中 $p \in \mathbb R^2$ 代表当前像素点的坐标， $u(p)$ 代表 $p$ 在形变中的移动向量， $p'$ 代表形变后 $p$ 的坐标。

在上面给出的例子中，学过线性代数的观众一眼就能看出来这是一个第一类正交变换，知道旋转角度后就能直接写出 $u$ 的每一个元素的值（当然，这是一个仿射变换，要用齐次式）。在计算机图形学中，这种不改变物体几何形状的变换被称为刚性变换（Rigid Transformation），这种情况我们是可以直接求出形变场的闭式解的。但是大部分我们需要处理的真实图像的形变场都是非刚性变换，比如下图，左边的四宫格图和右侧的四宫格是同一个患者的同一个呼吸周期两个不同相位的CT图像，想要对齐这两张图像显然不是一个简单的线性变换可以搞定的。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c3a948385a88404adaa6d45e20b0e2f4_1440w.png" style="width: 70%;"/>
</div>

因此，几十年间出现了许多方法来更加优雅地解决 ground truth 为非刚性形变场下的图像配准。为了节约篇幅，此处只讲原理比较简单的基于深度学习的方法， voxelmorph。论文地址为 https://arxiv.org/pdf/1809.05231.pdf

---

## 问题建模

voxelmorph 解决的主要是 CT 图像的配准， CT 是一个 3D 图像，形式化表示为 $R^{M\times N\times L\times C}$，其中 M 和 N 是 CT 图像的长宽，受限于目前的成像技术和计算设备（三维 Radon 变换），一般是 256 × 256 或者 512 × 512 。 L 是 CT 的层数。C 是体素（voxel）的色彩通道，因为 CT 是根据辐射投影值确定的物理参数，并没有 RGB 的概念，所以这个通道一般为 1，或者直接不显示，在 voxelmorph 的代码中，是需要考虑它的。

通过之前的论述，采用相对配准进行任务建模，我们记输入的两张 CT 图像，$m\in\mathbb R^{M\times N\times L\times C}$ 是等待配准的图像， $f \in \mathbb R^{M\times N\times L\times C} $ 是参考图像。模型负责输出形变场 $\phi \in \mathbb R^{M\times N\times L\times 3}$ 和 $m$ 被配准后的结构 $m\circ \phi \in \mathbb R^{M\times N\times L\times C}$ . 

> 对于 CT 图像 C = 1 

### 网络结构

为了配准 CT 图像， voxelmorph 提出了基于 UNet 的图像配准，结构很简单：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-da363b5aee7f0636a4b824130d5c18a8_1440w.png" style="width: 70%;"/>
</div>

模型会先按照 C 通道进行拼接，然后扔到一个 UNet 中提取特征，得到 $m$ 相对于 $f$ 的形变场 $\phi$，然后将形变场作用到 $m$ 上得到的预测的配准后的图像 $m \circ \phi$，我们自然希望配准后的图像和 $f$ 尽可能一样，而且一般而言，形变场的作用变换不应该太大，因为对于 CT 图像而言，配准前后的扰动其实并不大。就像前面我放出的两张不同相位的图像，读者远看可能都看不出差别。你当然也能解释成对于模型的结构风险的约束，加入正则项防止过拟合。总之，我们需要约束 $\phi$ 本身，让它的能量不要太大。综上所述，我们得到了如下的损失函数：
$$
\mathcal L(f, m, \phi) = \mathcal L_{sim} (f, m \circ \phi) + \lambda \mathcal L _{smooth} (\phi)
$$
从机器学习的角度上，这是一个自监督的损失函数。优化这个函数，就能完成对网络的训练。

其中， $\mathcal L_{sim}$ 使用 L2 范数；$\mathcal L_{smooth}$ 使用形变场的空间梯度计算，也就是三个方向的 一阶差分梯度 的结果。

> 上图中还有一个 Auxiliary Information，这是对于存在分割信息的图像，利用分割信息进行配准的损失函数。大致原理类似，本文并不使用分割信息，所以不在赘述；还有，不知道 一阶差分梯度 的去重修 image processing 吧

除了 L2 范数之外，原文中还提供了一个可选的损失函数作为 L2 范数的替代，那就是  cross-correlation 损失（简称 CC 损失），可以更好的表示配准结果中配准点和周围点的一致性，在配准任务中有很好的参考价值：
$$
CC(f, m\circ \phi) = \sum_{p} \frac{\left(\sum_{q} ( f(q) - \hat{f} (p) )\left( m\circ \phi(q) - \hat{m}\circ \phi (p) \right)  \right) ^ 2}{\left( \sum_{q} (f(q) - \hat{f}(p))^2 \right)  \left( \sum_{q} (m\circ \phi(q) - \hat{m} \circ \phi(p))^2 \right)}
$$
其中 $\hat{f}(p)$ 描述了 $p$ 的局部强度均值（local mean intensity）：
$$
\hat{f}(p) = \frac{1}{n^3} \sum_{q\in W(p, n)} f(q)
$$
$W(p, n)$ 描述了以 $p$ 为中心的一个 $n\times n\times n$ 的3D窗口内的所有元素，在论文中 $n=9$。 $\hat{m} \circ \phi(p)$ 的定义也是一样的。



### Spatial Transform 让 warp 变得可导

上面的过程是不是很简单？有的读者可能在看到网络的一瞬间就觉得这项工作过于简单，甚至不需要单独写一篇博客来描述。但是请阁下不要忘记了一件事：我们目前使用的深度学习优化器都是基于梯度信息（一阶微分信息）进行的，但是上述操作中的 $m \circ \phi$ 有谁说它是可导的了？

通过之前对于形变场定义的描述，阁下应该清楚了：对于一个给定的形变场和图像，有如下的关系成立：
$$
m\circ \phi (p) = m (p') \quad p' = p + u(p)
$$
其中涉及到了取索引操作 $(\cdot)$，取索引操作并没有被定义在可求导的 torch 算子中（主要是这玩意儿不构成闭包），所以基于索引操作定义出的损失函数是不可优化的。因此，基于深度学习的配准算法需要解决的一大难点就是如何让 warp 操作可导。基本思路就是利用常规算子（定义在数域上的加减乘除什么的，你懂的，不懂去重修高代或者群论吧）构造一个从 $p'$ 到 $m \circ \phi$ 的可微映射。在 Spatial transformer networks 这篇文章中给出了具体的构造方法也就是 Spatial Transform，其构造非常巧妙，简单总结就是如下的式子：
$$
m\circ \phi (p) = \sum_{q\in \mathcal N(p')} m(q) sim(p', q)
$$
其中 $\mathcal N(p')$ 代表的是 $p'$ 的邻居体素，在 2D 处理中就是 8 邻域点，3D 中就是 26 邻域点。sim 函数用于度量 $q$ 和 $p'$ 的相似性，这两个点越靠近，sim的值越大，反之越小。这个公式非常巧妙地将取索引操作转化成了对于索引的邻域点的加权求和操作，我们只需要使用可导函数涉及 sim 就能构造出一个对于 $p'$ 可导的 warp 操作了。在 voxelmorph 中，采用了如下的函数构造了 sim：
$$
sim(p', q) = \prod_{d\in \{x, y, z\}} (1 - \vert p'_d - q_d \vert)
$$
容易验证，这个式子满足上面对应 sim 的定义，而且在优化它时，$p' \in \mathbb R^3$ 会更容易和 $q \in \mathbb N^3$ 对齐，从而让后续对于形变场的取整操作的精度损失更小。

> 这里需要说明一下，UNet 输出的 $\phi$ 还需要进行插值对齐到图像坐标域上，因为图像坐标都是整数，那么形变场也应该都是整数，3D 图像的 26 邻域点也都是整数坐标

综上所述，为了使得 warp 操作可导，我们使用如下的公式近似表示出 原图像 + 形变场 + 像素点坐标 到配准像素点的可导映射：
$$
m\circ \phi(p) = \sum_{q\in \mathcal N(p')} m(q) \prod_{d\in \{x, y, z\}} (1 - \vert p'_d - q_d \vert)
$$
这样，我们就可以用梯度优化器来训练 voxelmorph了。

---

## 代码实现

### 数据集

配准数据集最出名的就是 dir lab 出品的数据了。阁下可以去他们的官网上下载： https://med.emory.edu/departments/radiation-oncology/research-laboratories/deformable-image-registration/downloads-and-reference-data/index.html。这需要提交申请表格和时间。

介于本文是基于一次课堂作业写的，我们作业也提供了一个小的数据集来自娱自乐：

- 链接：https://pan.baidu.com/s/1JMf2U56fKzdZYXFGv_5ecw
- 提取码：wgi9



### 代码实现

voxelmorph 本身提供了官方实现，支持 torch 和 tensorflow，所以你当然需要先安装 torch。

但是该项目基于的 python 版本有点古老。如果你使用的 python 版本小于等于 3.7，直接运行如下命令安装即可：

```bash
pip install voxelmorph
```

否则，请直接安装 GitHub 上的新版本：

```bash
git clone https://github.com/voxelmorph/voxelmorph
pip install -e voxelmorph
```



需要注意，voxelmorph 官方给的参数有一个隐藏条件：输入的 CT 图像必须是 64 层的。数据集装载、训练和推理需要自己实现，但是非常简单。需要写一个脚本将所有 CT 图像整理成一个 txt 文件，一行一个 `nii.gz` 或 `.nii` 文件的绝对路径。我这里事先做成了 `train.txt`。数据装载器、训练代码、推理代码需要自己完成。我将代码放在了文章末的附录，一共三个文件 `dataloader.py`, `train.py` 和 `register.py`。

训练命令为：

```bash
python train.py --gpu 1 --img-list train.txt --model-dir checkpoints --batch-size 1 --epochs 1000 --steps-per-epoch 100 --load-model checkpoints/1090.pt --lr 0.00001
```

训练过程中的损失值会异步地刷新在日志文件 `./log/voxelmorph.log` 中，阁下可以根据自己的需求去使用这个文件绘制损失下降曲线。训到大火收汁即可：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ec819478773660fb48040bed5ba47b13_1440w.png" style="width: 50%;"/>
</div>





推理命令为：

```bash
python register.py --gpu 1 --moving RegistrationDataset/LungImage/case10_T00.nii.gz --fixed RegistrationDataset/LungImage/case10_T50.nii.gz --model checkpoints/1090.pt
```

配准后的图像和形变场会存储在 `./experiment` 文件夹的 `case10_moved.nii.gz` 和  `case10_warp.nii.gz` 中。 `case10_warp.nii.gz` 并不是一个 CT 文件，我只是偷懒，直接用 CT 的 IO 函数导出而已，你仍然可以使用 `nibabel` 来读取它。



### 指标 TRE

为了更好地对训练得到的模型进行评估，我们需要了解配准任务中的常用指标 TRE。

为什么不直接使用配准图和目标图的相似性呢？因为 L2 定义下的相似度除非完全等于0，否则我们不知道配准结果是否真的可靠。像素值的相似度之和并不能反映配准后 **关键结构** 上的一致性。所以我们需要衡量一些关键点上的相似性，只需要原图上的关键点（这些点我们通常称为 landmark）在形变场的作用下得到配准图中对应的关键点，然后将这些关键点与目标图的 landmark 进行比较就能得到配准的图像的好坏了，这就是 TRE （target registration error）的定义。

> TRE 最早这篇文章中被定义： Registration of head volume images using implantable fiducial markers, IEEE Transactions on Medical Imaging 16, 447–462.

公式也很简单，我们记 warp 对于三个轴的变换分别为： $u(\cdot),v(\cdot),w(\cdot)$，原图 landmark $\bold x = (x, y, z)$ 对应的目标图的 landmark 是 $(x', y', z')$，则 TRE 被定义为：
$$
\mathrm{TRE} = \sqrt{(x + u(\bold x) - x') ^ 2 + (y + v(\bold x) - y')^2 +( z + w(\bold x) - z' )^2}
$$

假设在上面已经跑完 `register.py` 后，我们尝试计算一下 TRE，先导入刚刚配准完成的数据：

```python
import nibabel as nb
import numpy as np

warp_file = './experiment/case10_warp.nii.gz'
origin_landmarks_file = './RegistrationDataset/LungLandmark/case10_300_T00_xyz.txt'
target_landmarks_file = './RegistrationDataset/LungLandmark/case10_300_T50_xyz.txt'

warp = nb.load(warp_file)
warp = np.array(warp.get_fdata())

origin_landmarks = np.genfromtxt(origin_landmarks_file, dtype=int)
target_landmarks = np.genfromtxt(target_landmarks_file, dtype=int)

warp.shape, origin_landmarks.shape, target_landmarks.shape
```

out:

```
((3, 512, 512, 120), (300, 3), (300, 3))
```

然后根据刚才的公式直接计算即可，非常简单：

```python
def target_registration_error(origin_landmarks: np.ndarray, target_landmarks: np.ndarray, warp: np.ndarray):
    assert origin_landmarks.shape == target_landmarks.shape
    errors = []
    for ol, tl in zip(origin_landmarks, target_landmarks):
        vec = warp[:, ol[0], ol[1], ol[2]]
        moved_ol = ol + vec
        errors.append(np.linalg.norm(moved_ol - tl))
    return sum(errors) / len(errors)

target_registration_error(origin_landmarks, target_landmarks, warp)
```

得到的结果就是 TRE。最后瞟一眼配准前后的图像：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-2755a37cdfc3e7748c2a101785da5638_1440w.png" style="width: 80%;"/>
</div>

没有发生严重畸变，那没啥问题了。






---

## 参考资料

- https://arxiv.org/pdf/1809.05231.pdf
- https://zhuanlan.zhihu.com/p/41738716
- https://pytorch.org/docs/stable/generated/torch.nn.functional.grid_sample.html

---

## 附录

`dataloader.py`：

```python
import os
import glob
import numpy as np
from voxelmorph import py

def volgen(
    vol_names,
    batch_size=1,
    segs=None,
    np_var='vol',
    pad_shape=None,
    resize_factor=1,
    add_feat_axis=True
):
    # convert glob path to filenames
    if isinstance(vol_names, str):
        if os.path.isdir(vol_names):
            vol_names = os.path.join(vol_names, '*')
        vol_names = glob.glob(vol_names)

    cases = {}
    for case_path in vol_names:
        case_path_name = os.path.basename(case_path)
        case_name = case_path_name.split('_')[0]
        if case_name not in cases:
            cases[case_name] = []
        cases[case_name].append(case_path)
    
    print('detect case number:', len(cases))

    cases_names = list(cases.keys())

    while True:
        # choose one case
        case_name = np.random.choice(cases_names)

        load_params = dict(np_var=np_var, add_batch_axis=True, add_feat_axis=add_feat_axis,
                           pad_shape=pad_shape, resize_factor=resize_factor)

        indices = np.random.randint(len(cases[case_name]), size=batch_size)        
        imgs = [py.utils.load_volfile(cases[case_name][i], **load_params) for i in indices]
        inputs = np.concatenate(imgs, axis=0)

        indices = np.random.randint(len(cases[case_name]), size=batch_size)        
        imgs = [py.utils.load_volfile(cases[case_name][i], **load_params) for i in indices]
        labels = np.concatenate(imgs, axis=0)

        yield inputs, labels

def scan_to_scan(vol_names, bidir=False, batch_size=1, prob_same=0, no_warp=False, **kwargs):
    zeros = None
    gen = volgen(vol_names, batch_size=batch_size, **kwargs)
    while True:
        scan1, scan2 = next(gen)
        
        assert scan1.shape[3] >= 64, 'make sure the #slice of input >= 64!'
        assert scan2.shape[3] >= 64, 'make sure the #slice of input >= 64!'
        
        scan1 = scan1[:, :, :, :64, :]
        scan2 = scan2[:, :, :, :64, :]
        # (1, 512, 512, 64, 1)
                
        # some induced chance of making source and target equal
        if prob_same > 0 and np.random.rand() < prob_same:
            if np.random.rand() > 0.5:
                scan1 = scan2
            else:
                scan2 = scan1

        # cache zeros
        if not no_warp and zeros is None:
            shape = scan1.shape[1:-1]
            zeros = np.zeros((batch_size, *shape, len(shape)))

        invols = [scan1, scan2]
        outvols = [scan2, scan1] if bidir else [scan2]
        if not no_warp:
            outvols.append(zeros)

        yield (invols, outvols)
```

`train.py`：

```python
import os
import random
import argparse
import time
import numpy as np
import torch

from loguru import logger

logger.remove(handler_id=None)
logger.add(
    sink='log/voxelmorph.log',
    level='INFO',
    encoding='utf-8',
    format="{time:YYYY-MM-DD HH:mm:ss} | {message} "
)

# import voxelmorph with pytorch backend
os.environ['NEURITE_BACKEND'] = 'pytorch'
os.environ['VXM_BACKEND'] = 'pytorch'
import voxelmorph as vxm  # nopep8

import dataloader

# parse the commandline
parser = argparse.ArgumentParser()

# data organization parameters
parser.add_argument('--img-list', required=True, help='line-seperated list of training files')
parser.add_argument('--img-prefix', help='optional input image file prefix')
parser.add_argument('--img-suffix', help='optional input image file suffix')
parser.add_argument('--model-dir', default='models',
                    help='model output directory (default: models)')
parser.add_argument('--multichannel', action='store_true',
                    help='specify that data has multiple channels')

# training parameters
parser.add_argument('--gpu', default='0', help='GPU ID number(s), comma-separatedatlas (default: 0)')
parser.add_argument('--atlas', default=None, help='GPU ID number(s), comma-separatedatlas (default: 0)')
parser.add_argument('--batch-size', type=int, default=1, help='batch size (default: 1)')
parser.add_argument('--epochs', type=int, default=1500,
                    help='number of training epochs (default: 1500)')
parser.add_argument('--steps-per-epoch', type=int, default=100,
                    help='frequency of model saves (default: 100)')
parser.add_argument('--load-model', help='optional model file to initialize with')
parser.add_argument('--initial-epoch', type=int, default=0,
                    help='initial epoch number (default: 0)')
parser.add_argument('--lr', type=float, default=1e-4, help='learning rate (default: 1e-4)')
parser.add_argument('--cudnn-nondet', action='store_true',
                    help='disable cudnn determinism - might slow down training')

# network architecture parameters
parser.add_argument('--enc', type=int, nargs='+',
                    help='list of unet encoder filters (default: 16 32 32 32)')
parser.add_argument('--dec', type=int, nargs='+',
                    help='list of unet decorder filters (default: 32 32 32 32 32 16 16)')
parser.add_argument('--int-steps', type=int, default=7,
                    help='number of integration steps (default: 7)')
parser.add_argument('--int-downsize', type=int, default=2,
                    help='flow downsample factor for integration (default: 2)')
parser.add_argument('--bidir', action='store_true', help='enable bidirectional cost function')

# loss hyperparameters
parser.add_argument('--image-loss', default='mse',
                    help='image reconstruction loss - can be mse or ncc (default: mse)')
parser.add_argument('--lambda', type=float, dest='weight', default=0.01,
                    help='weight of deformation loss (default: 0.01)')
args = parser.parse_args()

bidir = args.bidir

from tqdm import tqdm, trange

if __name__ == '__main__':
    # load and prepare training data
    train_files = vxm.py.utils.read_file_list(args.img_list, prefix=args.img_prefix,
                                            suffix=args.img_suffix)
    assert len(train_files) > 0, 'Could not find any training data.'

    # no need to append an extra feature axis if data is multichannel
    add_feat_axis = not args.multichannel
    
    # realize your own loader
    generator = dataloader.scan_to_scan(
        train_files, batch_size=args.batch_size, bidir=args.bidir, add_feat_axis=add_feat_axis)
    # if args.atlas:
    #     # scan-to-atlas generator
    #     atlas = vxm.py.utils.load_volfile(args.atlas, np_var='vol',
    #                                     add_batch_axis=True, add_feat_axis=add_feat_axis)
    #     generator = vxm.generators.scan_to_atlas(train_files, atlas,
    #                                             batch_size=args.batch_size, bidir=args.bidir,
    #                                             add_feat_axis=add_feat_axis)
    # else:
    #     # scan-to-scan generator
    #     generator = vxm.generators.scan_to_scan(
    #         train_files, batch_size=args.batch_size, bidir=args.bidir, add_feat_axis=add_feat_axis)
    

    # extract shape from sampled input
    inshape = next(generator)[0][0].shape[1:-1]

    # prepare model folder
    model_dir = args.model_dir
    os.makedirs(model_dir, exist_ok=True)

    # device handling
    gpus = args.gpu.split(',')
    nb_gpus = len(gpus)
    device = 'cuda'
    os.environ['CUDA_VISIBLE_DEVICES'] = args.gpu
    assert np.mod(args.batch_size, nb_gpus) == 0, \
        'Batch size (%d) should be a multiple of the nr of gpus (%d)' % (args.batch_size, nb_gpus)

    # enabling cudnn determinism appears to speed up training by a lot
    torch.backends.cudnn.deterministic = not args.cudnn_nondet

    # unet architecture
    enc_nf = args.enc if args.enc else [16, 32, 32, 32]
    dec_nf = args.dec if args.dec else [32, 32, 32, 32, 32, 16, 16]


    # otherwise configure new model
    print('init model, inshape:', inshape)
    model = vxm.networks.VxmDense(
        inshape=inshape,
        nb_unet_features=[enc_nf, dec_nf],
        bidir=bidir,
        int_steps=args.int_steps,
        int_downsize=args.int_downsize
    ) 
    
    if args.load_model:
        # load initial model (if specified)
        assert os.path.exists(args.load_model)
        model_state_dict = torch.load(args.load_model, map_location=device)
        model.load_state_dict(model_state_dict)
        print('load checkpoint from ', args.load_model)
                
    if nb_gpus > 1:
        # use multiple GPUs via DataParallel
        model = torch.nn.DataParallel(model)
        model.save = model.module.save

    # prepare the model for training and send to device
    model.to(device)
    model.train()

    # set optimizer
    optimizer = torch.optim.Adam(model.parameters(), lr=args.lr)

    # prepare image loss
    if args.image_loss == 'ncc':
        image_loss_func = vxm.losses.NCC().loss
    elif args.image_loss == 'mse':
        image_loss_func = vxm.losses.MSE().loss
    else:
        raise ValueError('Image loss should be "mse" or "ncc", but found "%s"' % args.image_loss)

    # need two image loss functions if bidirectional
    if bidir:
        losses = [image_loss_func, image_loss_func]
        weights = [0.5, 0.5]
    else:
        losses = [image_loss_func]
        weights = [1]

    # prepare deformation loss
    losses += [vxm.losses.Grad('l2', loss_mult=args.int_downsize).loss]
    weights += [args.weight]
    
    if args.load_model:
        ckp_name = os.path.basename(args.load_model)
        ckp_id = int(ckp_name.strip('.pt'))
        args.initial_epoch = ckp_id + 1

    # training loops
    for epoch in range(args.initial_epoch, args.epochs + args.initial_epoch):

        # save model checkpoint
        if epoch % 5 == 0:
            save_path = os.path.join(model_dir, '%04d.pt' % epoch)
            torch.save(model.state_dict(), save_path)

        epoch_loss = []
        epoch_total_loss = []

        tbar = trange(args.steps_per_epoch, colour='yellow', ncols=80)
        tbar.set_description_str(f'epoch: {epoch}, loss=unknown')
        for step in tbar:
            # generate inputs (and true outputs) and convert them to tensors
            inputs, y_true = next(generator)
            inputs = [torch.from_numpy(d).to(device).float().permute(0, 4, 1, 2, 3) for d in inputs]
            y_true = [torch.from_numpy(d).to(device).float().permute(0, 4, 1, 2, 3) for d in y_true]

            # run inputs through the model to produce a warped image and flow field
            y_pred = model(*inputs)

            # calculate total loss
            loss = 0
            loss_list = []
            for n, loss_function in enumerate(losses):
                curr_loss = loss_function(y_true[n], y_pred[n]) * weights[n]
                loss_list.append(curr_loss.item())
                loss += curr_loss

            epoch_loss.append(loss_list)
            epoch_total_loss.append(loss.item())

            # backpropagate and optimize
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()
            
            tbar.set_description_str(f'epoch: {epoch}, loss={round(loss.item(), 4)}')

        # print epoch info
        epoch_info = 'Epoch %d/%d' % (epoch + 1, args.epochs)
        losses_info = ', '.join(['%.4e' % f for f in np.mean(epoch_loss, axis=0)])
        loss_info = 'loss: %.4e  (%s)' % (np.mean(epoch_total_loss), losses_info)
        logger.info(loss_info)

    # final model save
    save_path = os.path.join(model_dir, '%04d.pt' % args.epochs)
    torch.save(model.state_dict(), save_path)
```

`register.py`：

```python
import os
import argparse
import math

import numpy as np
import nibabel as nib
import torch

os.environ['NEURITE_BACKEND'] = 'pytorch'
os.environ['VXM_BACKEND'] = 'pytorch'
import voxelmorph as vxm   # nopep8

parser = argparse.ArgumentParser()
parser.add_argument('--moving', required=True, help='moving image (source) filename')
parser.add_argument('--fixed', required=True, help='fixed image (target) filename')
parser.add_argument('--moved', default=None, help='warped image output filename')
parser.add_argument('--model', required=True, help='pytorch model for nonlinear registration')
parser.add_argument('--warp', default=None, help='output warp deformation filename')
parser.add_argument('-g', '--gpu', help='GPU number(s) - if not supplied, CPU is used')
parser.add_argument('--multichannel', action='store_true',
                    help='specify that data has multiple channels')
args = parser.parse_args()

if args.gpu and (args.gpu != '-1'):
    device = 'cuda'
    os.environ['CUDA_VISIBLE_DEVICES'] = args.gpu
else:
    device = 'cpu'
    os.environ['CUDA_VISIBLE_DEVICES'] = '-1'

# load moving and fixed images
add_feat_axis = not args.multichannel
moving = vxm.py.utils.load_volfile(args.moving, add_batch_axis=True, add_feat_axis=add_feat_axis)
fixed, fixed_affine = vxm.py.utils.load_volfile(
    args.fixed, add_batch_axis=True, add_feat_axis=add_feat_axis, ret_affine=True)

# load and set up model
model = vxm.networks.VxmDense(
    inshape=(512, 512, 64),
    nb_unet_features=[[16, 32, 32, 32], [32, 32, 32, 32, 32, 16, 16]],
    bidir=False,
    int_steps=7,
    int_downsize=2
)

assert os.path.exists(args.model)
model_state_dict = torch.load(args.model, map_location=device)
model.load_state_dict(model_state_dict)
print('load checkpoint from ', args.model)

model.to(device)
model.eval()

# set up tensors and permute
input_moving = torch.from_numpy(moving).to(device).float().permute(0, 4, 1, 2, 3)
input_fixed = torch.from_numpy(fixed).to(device).float().permute(0, 4, 1, 2, 3)

slices_num = input_moving.shape[-1]
assert slices_num >= 64

moveds = []
warps = []

for i in range(math.ceil(slices_num / 64)):
    min_idx = 64 * i
    max_idx = min_idx + 64
    exceed = 0
    if max_idx > slices_num:
        exceed = max_idx - slices_num
        max_idx -= exceed
        min_idx -= exceed
    

    # choose current 64
    input_moving_i = input_moving[..., min_idx: max_idx]
    input_fixed_i = input_fixed[..., min_idx: max_idx]
    
    # predict
    with torch.no_grad():
        moved, warp = model(input_moving_i, input_fixed_i, registration=True)
    
    if exceed > 0:
        moved = moved[..., exceed: ]
        warp = warp[..., exceed: ]

    moveds.append(moved.detach().cpu())
    warps.append(warp.detach().cpu())
    
    del moved
    del warp

moved = torch.cat(moveds, dim=4)
warp = torch.cat(warps, dim=4)
    
# save moved image
file_name = os.path.basename(args.moving)
case_name = file_name.split('_')[0]
if args.moved is None:
    moved_name = f'{case_name}_moved.nii.gz'
    args.moved = os.path.join('experiment', moved_name)

# save warp
if args.warp is None:
    warp_name = f'{case_name}_warp.nii.gz'
    args.warp = os.path.join('experiment', warp_name)
    
moved = moved.detach().cpu().numpy().squeeze()
print('moved shape:', moved.shape)
vxm.py.utils.save_volfile(moved, args.moved, fixed_affine)

warp = warp.detach().cpu().numpy().squeeze()
print('warp shape:', warp.shape)
vxm.py.utils.save_volfile(warp, args.warp, fixed_affine)

```