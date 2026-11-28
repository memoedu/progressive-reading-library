## 前言

在 [上一章](https://kirigaya.cn/blog/article?seq=387) 中，我们介绍了现代 GPU 集群的一个底层硬件结构，这是我们充分理解后续分布式算法的一个基石，什么样子的硬件和什么样子的市场才能决定顶层的软件和算法的模样。这是很多底层软件和协议栈设计的第一性原理。

但是实际上要更加复杂一点，软件和硬件往往是协同发展的，算法或者软件开发有了新的需求，硬件跟进。硬件跟进后，软件的需求被满足，但是因为硬件的迭代周期远远长于软件，所以中间会存在很多的“参差”，这种参差就成为了很多变化和创新的起源，软件侧就会发现这个硬件为了满足过去某个需求的“特性”能够催生出新的技术点，萌生新的需求。

其中最典型的例子就是我们现在能看到的 AI 计算卡了，AI 计算卡的起源其实在座的各位都知道，那就是游戏 GPU。本世纪初，随着 GPU 开始在个人电脑普及，大家对于更流畅更绚丽的游戏画面需求越来越高了，这种软件上的需求就要求显卡厂商 NVIDIA 去把 GPU 的性能越做越高。这种硬件的升级确实满足了软件端对更高性能游戏的开发需求，然后一部分社区内的 “好事之徒” 发现 GPU 不仅仅可以用来渲染绚丽的游戏画面（对这部分感兴趣的可以看我之前的博客：[webgl2 教程（一）渲染模型、图形学基本概念与环境搭建](https://kirigaya.cn/blog/article?seq=206)），还可以利用它那强大的并行算力来为一些科学问题的演算进行提速（比如我之前拿 GPU 对医学影像的 CT 重建进行了千倍提速：[CT重建算法之 FBP 算法实现](https://kirigaya.cn/blog/article?seq=115)）。

但是早期的 NVIDIA 的 GPU 并没有提供接口来做 GEMM（General Matrix-Matrix Multiplication，通用矩阵乘）。当时社区玩家的做法是把需要并行计算的程序“伪装”成一次图形渲染任务。

比如下面 [这篇帖子](https://community.khronos.org/t/shaders-for-imaging-workaround-problem/51926) 清楚展示了 2004 年的玩家是如何利用 GPU 完成卷积运算的。

![图片](https://kirigaya.cn/api/storage/image/2e2cf26e-d339-45e2-afdb-f3f939850840.webp)

商业嗅觉灵敏的 NVIDIA 很早嗅到了未来的机遇，并在 20 年前（也就是 2006 年）公布了 CUDA 的第一个版本，通过 CUDA 接口，玩家们就可以直接利用 NVIDIA 的 GPU 完成并行计算了，相当于 NVIDIA 官方把社区玩家的这套 “邪修” 玩法给转正了。

> 需要注意的，并非只有 NVIDIA 的 cuda 在早期试图为未来的深度学习提供加速设备的软件接口，AMD 的 ROCm，中科院的 DaDianNao 也都是早期的先行者，差点忘了，DaDianNao 现在已经不存在了，它现在叫“寒武纪”。

后来，一个学心理学的老头拿了 CUDA 在团队里面加速语音识别模型的训练，并鼓励团队成员购买 NVIDIA 的卡，那个老头叫 Hinton，那一年是 2009 年。再后来，Hinton 带领团队使用  CUDA 搞出了 AlexNet，在 ImageNet 的比赛上拔得头筹，深度学习开始爆火的故事你们应该都知道了。

![图片](https://kirigaya.cn/api/storage/image/64c2a7d9-1b90-4408-aefc-b458743069eb.webp)

> 从左到右分别为 Ilya，Hinton，Alex，其中 Ilya 和 Alex 都是 Hinton 在多伦多大学的研究生。Hinton 后来拿了诺贝尔奖和图灵奖；震撼业界的 AlexNet 第一作者就是 Alex，他负责了算法设计和绝大部分 CUDA 工程实现，他也是最早尝试在多张 GPU 上训练模型的人之一；Ilya 在 AlexNet 爆火的三年后，也就是 2015 年，和一群朋友成立了 OpenAI，并担任首席科学家。

所以你能看到软件和硬件的发展是相互促进，左脚踩右脚的，NVIDIA 发明高性能显卡的初衷也并非是为了 AI，那个时候还没有 transformer 啥的呢。这或许就是技术进步的真相吧，很多事情的发展其实并没有那么多的计划，计划是赶不上变化的，科研创新也是无法被“计划”出来的。

这则小故事送给每一位读者，希望能对你认识这个世界有所启发。

好了，这章开始，要脱离硬件的苦海，开始讲更往上的东西了，也就是每一个做 infra 的朋友都会在每周报错日志和无数扣你工资的错误工单里出现的 NCCL 了。我们在上一章也提到了，CUDA 和 NCCL 是 NVIDIA 最厚的两大壁垒。CUDA 就是对一块 GPU 并行计算接口的封装，而 NCCL 则是对底层通信协议的封装。

先让我们暂时忽略 GPU 内部的工作原理，毕竟修复一块 GPU 并不在 infra 工程师的职责内，咱们不应该去抢技术支持的饭碗。

> 维护优良职业环境，人人有责

这篇教程开始我们将详细讲解一下 NCCL 的基础使用，技术细节和必要的通信原语的基础知识，这是对于我们上一章讲到的复杂底层硬件的抽象，抽象是计算机的核心思想之一，这一章节结束后，后面几个章节会讲到的各类分布式算法又是对 NCCL 的抽象。

抽象仍在继续 …

---

## 分布式进程标识

在之前，我们已经讲过了 GPU 集群硬件层面是如何完成通信和各种稀奇古怪的优化的，现在请把其中的部分细节通通忘掉，这是抽象的第一步，忽略被抽象层的技术细节，只记忆为什么要这么做。

目前大家可以将星罗棋布般排列的 GPU 集群抽象成彼此相连的一块块 GPU，这些 GPU 与 GPU 之间唯一的不同就只是它们之间的通信速率不一样而已。

为了从软件开发者的角度更好管理和识别这些 GPU，我们需要一组好用的“工具”来称呼不同机器上的 GPU，越简单越好，多年的工程化形成了一套较为通用的被称为“分布式进程标识”的术语。分布式进程标识会以参数的形式出现在动辄几百行的训练脚本里面，建议完全不懂的你不要跳过，否则你可能连同事写的分布式脚本都看不懂是什么意思。

假设我们是在 2 台 8 卡 GPU 服务器上训练或推理模型，那么我们将这套系统的总进程数称为 world_size，但是由于在 Python 中，进程总是和 GPU 一一对应的，所以总进程数等于总的 GPU 数量。嗯，记住，后面我永远默认一个进程对应一块 GPU。所以这套系统中，world_size=16。

> Python 中能不能一个进程管多个 GPU？为什么我们习惯性一个 Python 进程只管一块 GPU？留作课后作业了。

为了甄别每一个进程/GPU 在这个系统中的编号，我们使用 rank 来进行指代，比如 rank0 指的就是第 0 个进程/GPU。显然这套系统存在 rank0, rank1, …, rank15 一共 16 个进程。

但是显然这 16 张卡在通信网络中彼此的通信并非完全等价，通过上一章的学习，大家应该知道了，对于采用了 IB 卡 + NVLink + NVSwitch 的满血 GPU 服务器而言，同一台机器内的 GPU 彼此通信速度要快于不同机器的两张 GPU 的通信速度，这会对后续的算法设计产生影响。为了鉴别这种区别，我们还需要一套术语区分不同机器的 GPU。

我们用 local_rank 指代某个进程/GPU 在所在机器上的编号，比如 rank0\~15 的 local_rank 就分别为 rank0\~7 和 rank0\~7。具体关系如下图所示

![图片](https://kirigaya.cn/api/storage/image/ecffc65c-275d-4bb0-95dc-f88fb1fb2147.webp)

local_world_size 就指的是每台机器的进程数量了，比如这个例子的两台机器的 local_world_size 都是 8.

我们用 node_rank 指代机器的编号，比如第一台机器的 node_rank 就是 0，第二台机器的 node_rank 就是 1. nnodes 就用来指代机器的总数量，显然这里的 nnodes 就是 2.

当我们后续编程时，在一个进程眼里，上面这些术语都是系统中的一个个环境变量：

```python
world_size = os.getenv('WORLD_SIZE')
rank = os.getenv('RANK')
local_rank = os.getenv('LOCAL_RANK')
node_rank = os.getenv('NODE_RANK')
nnodes = os.getenv('NNODES')
```

> 当然，这些环境变量具体叫什么名字由你们的 infra 平台在创建进程容器时注入，但是一般都叫这个名字

通过这些变量，我们在编程时就能知道当前进程在整个集群中的位置，是哪个机器，是哪张卡，这是开展后续一切分布式算法的基石。没有这些信息，便如同巧妇难为无米之炊，你什么也做不了。

并且，在真实调试时，我们需要经常借助这些信息来作为调试信息的标头。比如你编写的某个分布式算法“卡”住了，我们通常用 hang 住了（或者夯住了）来表达这种情况，这也是分布式调试中出现频率最高，最麻烦的 bug。导致它的原因非常多，我们后面会提到。为了找到是哪个畜生导致了 hang，我们就需要用到上面的这些信息。比较常用的一种调试技巧就是打印上：

```python
log(f'[{local_rank}/{rank}] enter here')
```

当然，有些更加成熟的 infra 平台会在收集日志时帮你加上这样一个信息，这具体由你们公司的 infra 平台开发人员决定。

然后你检查一下日志，可能就会发现：

```
[0/16] enter here
[1/16] enter here
[2/16] enter here
```

你看到日志信息停在了 \[2/16\] 上，那么我们就知道目前出现问题的是第 0 台机器的第 2 个进程。然后再进行后续的排查就能得到结果，或者还是得不到结果（x）。

---

## 通信原语

显然，通过前面的认知你估计也有感觉了，分布式编程和传统编程很不一样——我们需要用一段程序来同时考虑、适配到 world_size 个进程，并达成我们的目的。

### 为什么需要通信原语？不能用传统进程间通信吗？

显然，这么多个进程肯定是需要彼此进行通信的，学习过操作系统的朋友应该还记得正常进程的方法的吧？

操作系统的进程间通信（IPC）一般分为：信号量、消息队列、内存映射、管道等等。但是无论如何，这些你在操作系统教科书能看到的，自己拿 C 语言手搓的进程间通信并不能用来直接用于 AI Infra 场景下的进程通信的，原因有如下两点：

1. 这些进程通信方法主要是系统内存上的操作，并不是 GPU 显存，并不能直接使用。

2. 这些方法并没有考虑到多机多进程的情况，适配起来会麻烦很多。

3. 这些方法并非为分布式计算而生，针对很多场景，这些方法对应的通信模型描述会变得很复杂，不利于 infra 工程师进行调度和规划的设计。

针对这些问题，学界业界通过多年的折腾，磨合迭代出了一套泛用性很强的通信操作集合，这些通信操作就如同编程语言中的 if else for 一样基础，但是基于它们又可以表达出整个并行计算想要表达出的复杂操作，这样的操作一般被我们称为“原子操作”，或者“原语”。而它们又是专门针对分布式通信而生的，所以我们将这一系列通信称为通信原语。

需要注意，通信原语是一套抽象，它与具体的硬件无关。纯 CPU 集群可以用通信原语实现超级计算机调度系统和分布式数据仓库，GPU 集群可以用通信原语实现分布式训练，国产的一些 NPU 也能实现类似的功能。它们的顶层都是近乎同一套的抽象，这套抽象就是通信原语。

### 通信原语基础

通信原语一共可以分为两类：P2P 通信（Peer-to-Peer Communication）和 集合通信（Collective Communication）

![图片](https://kirigaya.cn/api/storage/image/f7958c51-2cd8-4bc5-891a-9f1119165e8f.webp)

### P2P 通信

P2P 通信就是我们非常容易理解的点对点通信了，也就是将一张显卡上的数据传递给目标显卡，这非常类似于我们在正常的 CPU 进程中实现的进程通信 —— 数据总是一次只从一个点流动到另一个点上。

P2P 通信一共规定了四种基本通信协议：Send，Recv，iSend，iRecv。

> 协议/契约 只是高瞻远瞩地定义了设计的 IPO （input process output）模型是什么，但是它并不规定要怎么实现。当然，它也不负责实现。

拿用得最多的 Send 和 Recv 为例，它们必须成对出现在两个进程上，比如下面就是一个比较常见的用法：

```
rank0: Send(1) # rank0 把数据发送给 rank1
rank1: Recv(0) # rank1 想要接收从 rank0 发送的数据
```

需要说明的是，Send 和 Recv 操作会阻塞当前的进程，直到对应的数据传输/接收完成才会“松手”继续下面的流程。如下图所示：

![图片](https://kirigaya.cn/api/storage/image/82d2ae14-f2cb-4d3d-ab6b-3c042ac6e716.webp)

如果 rank0 跑得更快一些，当它执行到 Send 时，这个时候 rank1 还没跑到 Recv，也就是数据传输尚未就绪，所以 rank0 就会 hang 在 Send 操作上，直到 rank1 运行到 Recv，反之亦然。

所以这里经常出现的一个情况就是，rank1 还没执行到 Recv 就在前面的操作中爆炸了，或者程序没写好，直接从 Op A 跳到 Op C 了，那么 rank0 就会永远卡在 Send 上，也就是我们前面说的 hang 住了。

Send 和 iSend 唯一的区别就是阻塞不阻塞了。Send 是阻塞的（同步的），iSend 是不阻塞的（异步的），调用完后，iSend 会立刻返回一个对象，这个对象是可以被 wait 的。比如对于上面那个例子，我们把 Send(1) 替换为 iSend(1)

![图片](https://kirigaya.cn/api/storage/image/2249338f-0ed2-4380-8875-304e03929a47.webp)

那么 rank0 执行到 iSend(1) 时，iSend(1) 会返回一个对象 send_result，然后 rank0 不会等待 rank1 上的 Recv 被执行就会立刻执行 Op C。假设这个时候 rank0 希望和 rank1 的 Recv 同步了（比如 rank0 即将执行到一个环节，这个必须保证 rank1 已经执行完成 Recv 了），那么我们可以让 rank0 在 Op C 结束后立刻调用 send_result.wait()，那么此时 rank0 就会 hang 住，一直等待 rank0 把 Recv 执行完成，如上图蓝色虚线所示。

iSend 和 iRecv 就是如此的一对非阻塞通信方法，或者说异步通信方法。在执行一些通信调度优化时会很有用，在下一章节讲到部分算法时会再次提到。

这四种 p2p 通信原语的说明如下：

| 名称 | 说明 |
| :--- | :--- |
| `Send` | 阻塞发送。向指定 Rank 发送 Tensor，发送操作完成后才继续执行后续代码。 |
| `Recv` | 阻塞接收。从指定 Rank 接收 Tensor，接收完成后才继续执行后续代码。 |
| `iSend` | 非阻塞发送。发起发送后立即返回 `Work` 对象，可继续执行其他任务，之后通过 `wait()` 等待完成。 |
| `iRecv` | 非阻塞接收。发起接收后立即返回 `Work` 对象，接收完成前不要读取接收缓冲区。 |

### 集合通信

p2p 只发生在两个进程中，但是在分布式训练和推理中，会存在大量的操作按照一定的规律让 N 张卡同时地执行对称的集体操作，如果用 p2p 来表示这个过程会非常繁琐，而且硬件实现也困难。

我们来看一个分布式训练中最为简单的例子，数据并行 DP。DP 的思路非常简单，假设我们有 4 张卡，那么我们就将模型分别复制一份到 4 张卡上。之前的单卡训练可能是每一个 batch 32 条数据，那么我们现在就把这 32 条数据平摊到 4 张卡上，每张卡每个 batch 只需要处理 8 条数据，这样就可以加速训练了。

![图片](https://kirigaya.cn/api/storage/image/025fdf69-c38e-44ad-84a7-8815583af179.webp)

显然在这套模型中，数据通信主要发生在反向传播后，参数更新前。我们需要保证每轮的每个卡上的模型参数都一样（不一样你训练的都不是同一个模型了）。但是显然，每轮训练时散播到每张卡上的这 8 条数据都不一样，那么它们的产物极大概率不一样，那么反向传播积累的梯度值也就大概率不一样，更新后的参数也不一样。

因此，DP 模式下，每轮反向传播完成后，我们都需要收集各个进程上的梯度值，然后计算它们的平均值，才能得到这一个 size 为 32 的 batch 的实际梯度值。

> 不理解反向传播后的梯度值在程序层面的数据结构的朋友请移步：[纯Python实现原理级深度学习框架（二）前向运算和反向传播的实现，简单MLP实现boston预测](https://zhuanlan.zhihu.com/p/352130729)，全文搜索关键词：grad_table，这个 grad_table 就是 DP 时各个进程需要共享的东西。

“收集各个进程上的梯度值” 这个说法简单，做起来难点重重。大家最容易想到的做法就是，把全部的梯度值都发送给 rank0，然后在 rank0 上完成计算后再发送回各个 rank：

```
rank 1: Send(0)
rank 2: Send(0)
rank 3: Send(0)
rank 0: Recv(1), Recv(2), Recv(3)
       # 完成梯度平均值计算
	   compute_average_grad()
       Send(1), Send(2), Send(3)
rank 1: Recv(0)
rank 2: Recv(0)
rank 3: Recv(0)
```

伪代码表示就是：（简化版本，为了演示部分变量省略不写了）

```python
world_size = int(os.getenv("WORLD_SIZE"))
rank = int(os.getenv("RANK"))
local_rank = int(os.getenv("LOCAL_RANK"))

dist.init_process_group("nccl")
torch.cuda.set_device(local_rank)

if rank == 0:
    for src in range(1, world_size):
        dist.recv(src=src)

    compute_average_grad()

    for dst in range(1, world_size):
        dist.send(dst=dst)
else:
    dist.send(dst=0)
    dist.recv(src=0)
```

这么做带来的最明显的问题就是，负载不均衡了。通过上一章的学习我们知道卡与卡之间的通信带宽往往是对称的，但是在上面的方案中，rank0 承担了一切，全部其他 rank 的数据都在砸给 rank0，而且还是同步传递的，rank1 传递完成后 rank2 再传递，而其他几张卡的通信和计算都是完全空闲的状态，这样做性能非常差。在 world_size 变大后，这种负载不均衡带来的性能下降会更加显著。

并且从逻辑上，这种 “收集各个进程上的梯度值” 的行为应该是对称的，但是用 p2p 描述后，显然不对称了，而且这种描述不存在通用性。为了解决这一类问题，让这种涉及到多个 rank 的分布式系统可以基于某种对称描述进行通信，HPC 界提出了一种新的通信抽象，也就是集合通信（Collective Communication）。

针对很多分布式场景中如同上面那样，存在一定“对称性”的通信需求，集合通信给出了如下的几类：

![图片](https://kirigaya.cn/api/storage/image/aff671a0-8a4e-4edf-9dd8-9f9e3c998661.webp)

我们挑几个其中比较常见的讲一下，剩下的大家可以去问 AI。

强调一下，集合通信只是从数学层面给出了“一次集合通信要达成什么样的效果”，它并不指定具体的通信拓扑和实现方法，所以我们下面介绍的集合通信请把注意力放在输入是什么，输出是什么上面，具体如何实现的，数据在哪个 rank 上完成计算的，并不是集合通信协议层面需要考虑的事情。后续 NCCL 部分会讲具体的实现。

### Broadcast

broadcast 描述了一次广播操作，从一个 rank 上把数据广播给其他所有 rank。

例如：

```
rank 0: A
rank 1: -
rank 2: -
rank 3: -
```

Broadcast 后：

```
rank 0: A
rank 1: A
rank 2: A
rank 3: A
```

broadcast 操作只做通信，不做任何计算。

### Reduce & AllReduce

Reduce 也是大模型并行算法中用得最多的通信原语之一。Reduce 在这里并不翻译为“减少”，而是翻译为“归约”，这个数学中的术语表示用同一个二元运算，把一组元素不断合并，最后得到一个结果。

假设我们有一组如下的数据：

$$
x_0,x_1,...,x_{n-1}
$$
我们再选定一个二元操作符 $\oplus$，归约操作指的就是在给定上面这组数据和操作符的前提下，计算：

$$
x_0 \oplus x_1 \oplus ... \oplus x_{n-1}
$$
我们举个例子，比如我们给定这样一组整数域上的数字：

$$
1,2,3,4
$$
二元操作符如果选择为加法，那么归约就等价于求和：

$$
1+2+3+4
$$
二元操作符如果选择为乘法，那么归约就等价于求积：

$$
1\times2\times3\times4
$$
二元操作符如果选择为最大值，那么归约就等价于求这组数据的最大值：

$$
\mathrm{max}(1,2,3,4)
$$
学过小学数学的朋友们估计就能看出来了，能够参与归约运算的操作符和集合本质上只需要满足结合律即可：

$$
\forall a,b,c \in S\quad (a\oplus b)\oplus c = a \oplus (b \oplus c)
$$
显然，这让我们联想到了群论里的半群。对于深度学习常用的集合（向量与矩阵）和二元操作符（矩阵乘，element wise操作符），这些是否满足结合律，是否可以进行归约，这就等诸位自己慢慢验证了。

在集合通信里的 Reduce 操作也是类似的。这里的各个 rank 上准备执行 Reduce 的数据就是归约模型中的数据，二元操作符需要自己指定。为了方便理解，假设这里我们用加法作为二元操作符。那么，4 个 rank 的 Reduce 操作就会把 4 个 rank 上的数据加起来，然后传输给指定的 rank。

例如：

```
rank 0: A
rank 1: B
rank 2: C
rank 3: D
```

执行 Reduce 操作后，我们把目标 rank 指定为 rank0，那么结果就是：

```
rank 0: A + B + C + D
rank 1: -
rank 2: -
rank 3: -
```

那么想要让数据全部每个 rank 都保留一份呢？使用 AllReduce 就行了，AllReduce 和 Reduce 唯一的不同就是 AllReduce 会把归约得到的结果每个 rank 上分发一份。

对于上面那个例子，使用 AllReduce 后的结果就变成了：

```
rank 0: A + B + C + D
rank 1: A + B + C + D
rank 2: A + B + C + D
rank 3: A + B + C + D
```

大家看到这个是不是有点联想了？没错！我们前面 DP 的那个例子，完全没必要用 p2p 了，使用一个 AllReduce 就可以立刻解决问题：

```plaintext
rank 0: AllReduce(x, ReduceOp.SUM)
rank 1: AllReduce(x, ReduceOp.SUM)
rank 2: AllReduce(x, ReduceOp.SUM)
rank 3: AllReduce(x, ReduceOp.SUM)
for each rank: compute_average_grad()
```

优雅，对称，简洁，高效。

瞬间就能解决问题。

需要注意的是，一旦开启了 AllReduce 操作，那么当前这个 group 里的所有 rank 都会等待 AllReduce 归约并分发数据完成，再继续后续的操作。一旦有一个 rank 没有执行或者执行失败了，那么所有 rank 都会卡在 AllReduce 操作上，这个是不是非常像大家在大学操作系统课程上学到的 barrier？事实上，NCCL 后端通信的 barrier 就是通过 AllReduce 实现的。

### Scatter

Scatter 将源 Rank 上的一组数据分片分别发送给通信组中的各个 Rank，每个 Rank 最终只获得其中一个分片。

例如：

```
rank 0: [A, B, C, D]
rank 1: -
rank 2: -
rank 3: -
```

Scatter 后：

```
rank 0: A
rank 1: B
rank 2: C
rank 3: D
```

需要注意，Scatter 场景中的处理对象是一个数组。在我们后续的大部分场景处理的基本单元都是 tensor，那么 Scatter 的输入和输出就都是 tensor 数组。

### Gather & AllGather

Gather 所有 rank 的数据分片收集到一个目标 rank。Gather 和 Scatter 可以看成一对互逆的操作。

例如：

```
rank 0: A
rank 1: B
rank 2: C
rank 3: D
```

Gather 后：

```
rank 0: [A, B, C, D]
rank 1: -
rank 2: -
rank 3: -
```

如果希望聚合后的结果不仅仅只出现在一个 rank 上，可以改为 AllGather，对于上面的输入，使用 AllGather 后如下：

```
rank 0: [A, B, C, D]
rank 1: [A, B, C, D]
rank 2: [A, B, C, D]
rank 3: [A, B, C, D]
```

有的朋友可能会好奇，看起来 AllGather 操作也能用来实现 DP 的梯度同步需求呀？嗯，你说的没错，从结果来看，AllGather 操作也能让 DP 中的每一个 rank 都获取到别的 rank 的梯度信息，从而完成真实梯度的计算。

它们唯一的区别看起来也只有 AllGather 需要额外做一次加法，真的吗？

不要忘记了，朋友们，我们在前面说了这么一句话：

> 强调一下，集合通信只是从数学层面给出了“一次集合通信要达成什么样的效果”，它并不指定具体的通信拓扑和实现方法 …

通信原语只是“协议”，不是具体实现，如果大家看过我计算图那期教程 [纯Python实现原理级深度学习框架（二）前向运算和反向传播的实现，简单MLP实现boston预测](https://zhuanlan.zhihu.com/p/352130729)，就能知道很多东西协议和理论上是那么写的，但是实际实现的时候有大量的地方可以简化和优化。

AllReduce 和 AllGather 也是一样，它们的协议看起来很像，但是具体在硬件上规划它们的通信拓扑时，差别不小，想要知道具体差异，那就继续往下看，我会在 NCCL 那部分讲到。

---

## 通信原语的前端 PyTorch

虽然各家硬件厂商都提供了直接操作硬件进行通信的 sdk，但是考虑到移植性，通用性的问题，现代 HPC 软件很少会直接操作底层硬件，这样通用性差，移植性差，而是会选择中间套一层，使用这个层的更加抽象，与硬件无关的接口来使用这些通信原语。

这些中间层的通信框架被我们称为通信原语的前端，硬件厂商们实现的硬件的驱动层代码就被称为通信原语的后端。

最有名的通信前端就是 OpenMPI 了，这是每一个 HPC 方向的学生都会在教材里看到的一个 C++ 的库，可以用来编写 HPC 软件执行分布式通信。大家使用的 PyTorch 也是类似的，PyTorch 有一个子库叫做 distributed，就是用来做分布式通信。其他一些常见的通信原语前端如下表所示：

| 前端 / API | 典型写法 | 特点 |
| :--- | :--- | :--- |
| **MPI** | `MPI_Allreduce`、`MPI_Allgather`、`MPI_Send` | HPC 最经典、最标准的一套通信 API |
| **PyTorch Distributed** | `dist.all_reduce()`、`dist.all_gather()` | AI 训练里非常常见 |
| **Horovod** | `hvd.allreduce()`、`hvd.allgather()` | 专门面向分布式深度学习，抽象很接近 MPI |
| **TensorFlow Distributed** | `tf.distribute` / `ReplicaContext.all_reduce()` | TensorFlow 的分布式训练抽象 |
| **JAX / XLA** | `lax.psum`、`lax.all_gather` 等 | 更强调 SPMD / 编译器式并行 |
| **NCCL C API** | `ncclAllReduce()`、`ncclAllGather()` | 这已经更接近底层，不太应该称为“前端” |

PyTorch 不仅是目前最热门的深度学习框架，也是目前业界学界做分布式算法的核心基座。各大主流分布式训练框架，比如 Megatron-LM，DeepSpeed，全都不约而同选择了 PyTorch 作为通信前端进行算法开发。可见 PyTorch 在深度学习框架和通信前端层面的统治力。所以本教程选择基于 PyTorch 来讲讲通信前端的一些基础使用。

### 是 torchrun 而不是 python

我们都知道，PyTorch 是 Python 库，大家通常使用 PyTorch 写好网络代码后，都会使用 python [train.py](http://train.py) 来跑实验。但是由于我们训练的大模型通常都是在多机多卡上的，孤立的 Python 解释器或许可以在一台计算机上开启 8 个进程完成训练，但是对于多机场景就无可奈何了。

使用一个程序就把全部多台服务器连起来并不是一件简单的事情，一个很容易想到的方案就是在训练启动前约定好所有服务器要在哪个 IP 的哪个端口作为 “集合点”，大家在那里集合握手就能一起开始训练了（一般都是基于 TCP 协议的）。在 PyTorch 中，这个被称为集合点的 IP 所在的服务器被称为 rendezvous 节点，简称为 rdzv。

显然，上面这一段逻辑并不是一个 python 解释器就能自己解决的，需要很多的胶水代码，比如指定 rdzv 节点。为了方便大家使用，PyTorch 官方搞了一个分布式启动器叫 torchrun，它可以自动帮我们完成上面这些麻烦事。比如在一个 world_size=16, nnodes=2 的场景中，假设这两台服务器的 IP 分别为 10.0.0.1 和 10.0.0.2。

想要在这两台机器上做分布式训练，我们需要先指定谁是 rdzv，那么假设就是第一台服务器好了，它的 IP 是 10.0.0.1，那么只需要在两台服务器上分别执行下面的命令就能为后续开展分布式训练打开网络基础了：

```python
torchrun \
  --nnodes=2 \
  --nproc-per-node=8 \
  --rdzv-id=my_first_c10d \
  --rdzv-endpoint=10.0.0.1:29500 \
  train.py
```

my_first_c10d 就是给这个分布式任务起的一个 ID 编号，方便 torch 进行管理。--nproc-per-node 指的就是当前这个任务中，每台服务器有多少卡，或者说，每个 node 上有多少 rank。

但是这样还是有点麻烦，毕竟每次训练都需要在两台服务器上分别运行上面的命令，还需要解决麻烦的网络问题，比如我选择的 rdzv 节点可能因为某人手贱动了一下交换机导致无法被别的服务器 ping 到了，就不能再简单一点吗？

### 集群管理软件 Slurm/K8S

在上面的例子中，我们每次训练都需要到每台服务器都启动 torchrun 脚本，还要保证网络环境，这也太麻烦了！所以在真实的业界，一般都存在稳定的 infra 平台，用户通过提交任务的方式提交自己的分布式训练任务，然后 infra 平台负责帮你解决上面的这一切问题。

这些 infra 平台有很多说法，有叫“集群软件”，有叫“训练平台”，总之就是这么一个东西。目前业界做这类平台作为主流的基础框架就是 slurm 和 k8s 了，当然，本章是主讲通信原语和 NCCL 的，所以这一部分我大概会在后续多余的章节无话可说时讲讲，本章就略过了。

这类平台软件的使用方法就是指定你要的显卡数量和硬件配置，然后在平台上提交任务时，塞入上面的这串 torchrun xxx 的代码作为每一个节点的基础启动代码就好了。训练平台会自动帮你排队，分配卡，在每个机子上创建容器，然后每个容器都自动运行你上面在提交任务时指定的代码。

![图片](https://kirigaya.cn/api/storage/image/07120fe7-1f78-49a2-add3-ef0dd557753e.webp)

显然，这样做的优点就是你不再需要看锦恢后面的那几篇教程了，你不懂底层网络原理也能轻松跑多卡任务。缺点嘛，缺点就是你需要保证你每次提交的任务都是没问题的，一旦你的代码出现 infra 层面的 hang、算法层面的 loss 爆炸或者代码层面的某个地方变量写错了，你都不得不停止任务并重新修改代码，再提交任务。相比于传统的 Ctrl C 停止任务，方向键 ⬆️，回车↩︎，这样酣畅淋漓的瞬间重启，集群的任务提交显然麻烦地多，整体 debug 效率也会降低很多。

并且由于分布式训练会让日志产生在不同的机器上，集群有时还会负责将不同机器的日志统一聚合到平台上，好处是你不需要去每台机器上找日志，然后手动拼凑了；坏处嘛，还是一样，平台接管了所有底层操作，导致我们不能利用 AI 自动调试代码了，因为你本地使用的 AI 可看不到 infra 平台上聚合的调试日志，除非这个平台提供了 CLI 或者 MCP，但是据锦恢所知，大部分 infra 平台软件都没这些功能，你只能手动找到平台日志上的关键信息，然后丢给 AI，然后再根据 AI 要求修改代码，变回“半古法编程”时代。

对做算法的同学来说问题不大，因为大部分纯做算法的人不太在乎代码优雅度，会使用祖传屎山训练脚本跑，反正前辈说能跑，我就用它；但是对于做 infra 的同学来说，就很折磨了，你优化的分布式算法或制作的 Agentic RL 框架必须基于这个平台去测，你得充分了解每一处技术细节。

但是人总是需要适应的。至少你必须知道，集群调试和传统单机调试有很大的不同。

### 分布式程序，启动！

好了好吧，至少现在你应该知道我们在执行分布式训练脚本的时候，和传统单机单卡运行的python程序，至少在启动命令上有了很大的不同。

接下来让我们将注意力移动到单个 python 文件内部吧，看一看使用 PyTorch 这个通信前端写的脚本大概是什么样子的，有哪些基本的注意点，我们尽可能简短的介绍一下。

PyTorch 中通信部分封装在 `torch.distributed` 这个子库中，我们习惯这么来导入它：

```python
import torch.distributed as dist
```

PyTorch 在执行分布式通信前需要先建立一个 group，其实就是通知各个节点，确定当前的 GPU 数量呀等等基础信息。下面演示了一个最为基础的使用 NCCL 作为通信后端完成一次 AllReduce 的代码，它可作为镜像提交到上面的集群系统中执行通信调度：

```python
dist.init_process_group(
    backend="nccl"
)

x = torch.tensor([1], device="cuda")
dist.all_reduce(x) # 默认使用加法作为归约操作符
```

init_process_group 这个方法就是在初始化通信系统，让 PyTorch 知道哪些进程参与训练、每个进程是谁，以及它们通过什么通信后端交换数据。还记得我们前面的 torchrun 吗？torchrun 在启动每一个上面的 python 脚本，都会注入一个环境变量告诉当前进程 “当前咱们有 16 张卡，你是 rank2”，init_process_group 就能根据这些环境变量里的参数确定当前的通信组基本信息。

你也可以自己手动指定这些 rank，即便在大部分情况下这并没必要：

```python
dist.init_process_group(
    backend="nccl",
    rank=0,
    world_size=16
)
```

怎么知道当前进程在当前的通信组里面的 rank，world_size 等信息？dist 也提供了基础方法：

```python
rank = dist.get_rank()
world_size = dist.get_world_size()
```

### PyTorch 中进行 p2p 和集合通信

在 PyTorch 中，进行运算的基础单位是 tensor，所以 PyTorch 的通信前端也将最基础的数值类型包装成了 tensor 和 tensor 的数组。

玩过 tensor 的朋友们都知道，tensor 可以设置在 cpu （系统内存）上，也可以设置在 gpu （显存）上：

```python
rank = dist.get_rank()

# 默认创建在主机系统内存中，CPU 可以直接访问
tensor_0 = torch.tensor([rank])
# 创建在 GPU 的显存上，只有这块 GPU 可以直接访问
tensor_1 = torch.tensor([rank], device="cuda")
```

不同设备上的 tensor 是不能进行运算的，否则就会报错。这一点在通信上也是同理，CPU 上的 tensor 和 GPU 上的 tensor 不能共用一个通信后端。

比如上面介绍的 nccl 后端，这就是为 gpu 量身打造的（毕竟这是 NVIDIA 做的），它可用于 gpu 上 tensor 的传输通信。nccl 后端是不能用于 cpu 上 tensor 的通信，如果一些特殊场景（比如控制变量法做调试和性能优化）需要用到 cpu 上 tensor 的通信，需要把通信后端改为 gloo：

```python
dist.init_process_group(
    backend="gloo"
)

x = torch.tensor([1])
dist.all_reduce(x)
```

这样就没问题了（某些版本的 gloo 也能用来传输 gpu 上的 tensor，但是还是不推荐这么做）。

原生 PyTorch 的主分支代码只支持这三种通信后端：

| Backend | 底层实现 | 主要用途 | GPU | CPU | 典型场景 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **NCCL** | NVIDIA Collective Communications Library | GPU 集合通信 | ✅ | ❌ | 大模型训练主流方案 |
| **Gloo** | Meta 开源通信库 | 通用集合通信 | ✅(有限) | ✅ | CPU训练、调试、小规模GPU |
| **MPI** | Message Passing Interface | HPC 通信标准 | 取决于实现 | ✅ | 超算/HPC环境 |

而在 PyTorch 中进行通信其实用起来非常简单，和上面我们展示的那些伪代码，基本上没有什么差异，唯一的差异就是需要指定一下tenser。

比如

```python
# train.py
import torch
import torch.distributed as dist

dist.init_process_group("nccl")
rank = dist.get_rank()
tensor = torch.tensor([rank], device="cuda")

if rank == 0:
    dist.send(tensor, dst=1)

elif rank == 1:
    recv_tensor = torch.zeros(1, device="cuda")
    dist.recv(recv_tensor, src=0)
    print(recv_tensor)
```

运行下面命令来运行：

```python
torchrun --nproc-per-node=2 train.py
```

得到的结果：

```
tensor([0], device='cuda:1')
```

从这里也能看出来，p2p 通信在代码中会产生冗长的 if else。

其他一些集合通信使用就更简单了，下面只是让大家刷个脸熟，毕竟真实场景使用不记得了可以随时问 AI，关键在于判断 AI 的结果对不对，下面列举了部分集合通信在 PyTorch 中的使用：

```python
rank = dist.get_rank()
world_size = dist.get_world_size()

# 创建当前 rank 上的 GPU Tensor
tensor = torch.tensor([rank + 1], device="cuda")

# 将 src=0 的 Tensor 广播给所有进程
dist.broadcast(tensor, src=0)

# 所有进程执行 Reduce，并将结果同步给所有进程，归约操作符设置为加法，也就是求和归约
dist.all_reduce(tensor, op=dist.ReduceOp.SUM)

# 所有进程执行 Reduce，但只将结果发送给 dst=0
dist.reduce(tensor, dst=0, op=dist.ReduceOp.SUM)

# 创建接收所有 rank 数据的 Tensor 列表
tensor_list = [torch.zeros_like(tensor) for _ in range(world_size)]

# 收集所有进程的第二个参数的 tensor，并 gather 到第一个参数的 tensor_list 中
dist.all_gather(tensor_list, tensor)
```

这里教大家一个小技巧，遇到训练脚本 hang 住了，不一定是你的代码的问题（毕竟你们大部分人拿的都是祖传屎山脚本。。。），也有可能是环境问题（硬件或者驱动或者什么乱七八糟的版本号）。排除环境问题的最好方法就是利用上面教你们的 PyTorch 通信写法快速写一个最小案例，比如：

```python
import torch
import torch.distributed as dist

dist.init_process_group("nccl")
rank = dist.get_rank()
tensor = torch.tensor([rank + 1], device="cuda")

print("enter")
dist.all_reduce(tensor)
print("finish", rank, tensor)
```

然后跑它一遍，如果卡在 enter，就说明是环境问题，否则就是你的训练代码的问题。当然当然，你完全可以用 AI 帮你生成这样一份最小案例代码，我的这份代码就是 AI 生成的，但是关键在于你需要有能力判断这份代码对不对，而不是把 comprehension debt 越滚越大。

---

## 通信原语的后端 NCCL 基础

好吧，看起来通信基本上都讲完了，我的朋友们也喊我上线 MC 了。

但是你真的觉得这一章有这么简单吗？

别忘记了，我们前面说了，PyTorch 它只是一个通信前端，通信原语也只规定了当前的通信在数学层面上应该达到什么样一个效果。但是具体在硬件上怎么样规划两台服务器上指定的 rank 上数据的传输则是通信后端负责的事情。不同的通信后端对于这件事情使用的算法和各种逻辑也是不一样的。

虽然你们中的大多数并没有机会亲自去参与这些通信后端的开发与设计，但是了解这些基本的算法，仍然有利于帮助我们理解和编写复杂的分布式训练代码，并在我们规划具体的分布式策略的时候，提供一定的理论依据。

实际上通信后端很多啊，但是考虑到目前最为常用的，甚至已经成为行业标准的通信后端，就是 NCCL。所以本章节就以 NCCL 为代表。向大家科普一下目前通信后端对于各个通信原语实现的一个基本算法和一些很重要的技术细节。

### NCCL 是啥

NCCL 全称 NVIDIA Collective Communications Library，译为英伟达集合通信库，是 NVIDIA 为他们的 GPU 集群设计的基于 C++ 的通信库，并且这个小宝贝儿还是开源的：[NVIDIA/nccl](https://github.com/NVIDIA/nccl)。大家可以结合 deepwiki 去快速了解里面的一些算法细节（不知道如何用 deepwiki 的看这篇：[「不是哥们，遇到功能不知道咋用，先问 AI 再问我呀」deepwiki 使用教程](https://kirigaya.cn/blog/article?seq=373)）

> 无奖问答：NCCL 是 NVIDIA 的通信库，那么 HCCL 是谁的呢？

还记得我们前面例子里面举到的 AllReduce 和 AllGather 的例子吗？我们当时谈到这两个通信原语都能解决 DP 对于梯度同步的通信需求。但是实际上我们还是只会选择用 AllReduce，这里面的原因，一部分就和 NCCL 通信后端的具体实现有关系。这就是我们这一章节需要负责告知大家的内容。

### NCCL 的通信算法选择

当我们在使用 PyTorch 前端执行通信组的创建和通信时，底层要做的事情很多，一个直观的演示动画如下：

@[video](https://kirigaya.cn/api/storage/video/8841fecd-22a6-4707-a1f6-35e50136540f.mp4)

当我们在 PyTorch 里面执行如下命令创建通信组时：

```python
dist.init_process_group("nccl")
```

由于选中了 NCCL 作为后端，NCCL 的后端代码会执行一些初始化操作来为后续的通信算法提供基础数据。

首先，NCCL 会进行一次topology discovery（拓扑发现），这一步骤是为了确认当前通信组能看到这么多张卡是怎么连接的，有没有 NVLink，哪些在同一个 NUMA 内，哪些在同一个机器内，也就是我们上一章提到的 `nvidia-smi topo -m` 的输出内容那样的一个拓扑信息（不过 `nvidia-smi topo -m` 只会展示一台机子的拓扑信息，NCCL 的拓扑发现是需要找到多台机子上全部 GPU 之间的拓扑关系）。

当我们的 PyTorch 代码执行到第一次通信的时候：

```python
dist.all_reduce(tensor)
```

NCCL 层实际会调用 ncclAllReduce 标记一次操作和当前的基本信息，而不会真正去执行这次通信，然后压入调度队列中（[源码](https://github.com/NVIDIA/nccl/blob/master/src/collectives.cc#L168)）。等到需要调度这次通信时（为什么不设计为要执行通信则立刻调度的模式呢？因为不一定只有你在用通信，可能多个进程都在同时使用）

根据初始化时拿到的拓扑信息（任意两个 GPU 之间咋连接的，彼此通信带宽多少这些信息），当前要执行的通信原语（比如这次是 AllReduce），这次执行通信的输入数据大小（假设这次单个 rank 上执行通信的 tensor 有 1 个 G 这么大）NCCL 会根据内部算法计算出使用什么「NCCL 通信算法」完成这次 AllReduce。

> 当然，NCCL 并不会每次在执行通信操作之前都做这样的一个算法，那样也未免太耗时了。NCCL 会在完成一次通信算法的计算后，就对相同规格下的这一次通信原语的算法选择结果进行一次缓存。

### 什么是 NCCL 通信算法

所谓 “NCCL 的通信算法”，指的就在已经知道当前的 GPU 拓扑信息，要做的通信原语和通信的数据大小等信息的前提下，规划一下，每张 GPU 具体要怎么走底层的路径来抵达需要传递到的 GPU 上。这些传递的方法肯定不能是毫无章法的，都是需要严谨的数学建模和理论计算才能得出的，具体推导这些算法的复杂度会在下面展开讲解每一个算法时讲一下。

NCCL 内部默认提供了这几类算法：

| 算法 | 算法说明 | 适用的集合通信 | 典型场景 |
| :--- | :--- | :--- | :--- |
| **Ring** | 环形通信 | AllReduce、AllGather、ReduceScatter、Broadcast、Reduce 等 | 大消息、高带宽场景，最常用 |
| **Tree** | 双二叉树/树形通信 | 主要用于 AllReduce | 小消息、低延迟场景 |
| **CollNet** | 分层集合通信 | 主要用于 AllReduce，也可用于部分 AllGather / ReduceScatter 变体 | 大规模多节点集群 |
| **NVLS** | NVLink SHARP / NVLink Switch 加速 | AllReduce、AllGather、ReduceScatter 等 | NVSwitch / NVLink 高端系统 |
| **PAT** | 面向分块与流水化的树形算法 | AllGather、ReduceScatter | 大规模 rank 下的小/中消息通信 |

在训练时，你可以注入环境变量 `NCCL_ALGO` 来固定 NCCL 算法，这样，上述的算法选择就不会执行了。

```bash
export NCCL_ALGO=Ring
torchrun --nproc-per-node=8 train.py
```

这一招用来进行通信链路调试，探明 GPU 拓扑或是排除某些错误时会非常有用，当然，前提是你得自己知道这些具体的 NCCL 算法都是如何执行的，我们下面就逐一展示一下。

为了更好的对下面需要讲到各个通信算法进行数学建模，从而标准化系统化地构建算法性能的认知，我们需要引入一套在 AI 训练的通信分析中很常用的分析模型 $\alpha-\beta$ **模型**（也称 **Hockney 模型**），这套模型中，我做出如下定义：

* $p$: rank 的数量（也就是 world_size）

* $n$: 单个 rank 单次传输的数据量

* $α$：发起一次通信的固定延迟

* $β$：传输 1 Byte 数据所需时间

那么一次通信中，系统的通信开销就可以建模为：

$$
T=S(p)\cdot\alpha + F(p)\cdot n\cdot\beta
$$
只要能把函数 $S(p),F(p)$ 找出来，我们就能完美描述并基于数学形式分析一次通信。

由于 $\alpha, \beta$ 都是常数，所以我们用 $S(p)$ 描述 Latency（延时），用 $F(p)\cdot n$ 描述 Bandwidth（带宽）。而我们数学建模并执行通信分析的目的就是为了搞明白这个算法，它的 Latency 和 Bandwidth 关于 $p,n$ 的复杂度是什么样的。

为了更加直观地演示和计算具体的复杂度公式，我们下面都采用上面提到的 DP 梯度同步这个通信例子来演示，我们记当前网络一共有四层，每一层的梯度分别为 $g_1,g_2,g_3,g_4$。我们用 $g_{ij}$ 表示第 $i$ 个 rank 上的第 $j$ 层网络参数的梯度。

### 啥算法也没有，暴力通信

我们先来看看用最无脑的方法完成 DP 的梯度同步会是什么样子的，也就是所有数据先 gather 到 rank0 上，在 rank0 上完成全部梯度的平均值计算，然后再 broadcast 回各个 rank。用一个简单的例子也顺便带大家熟悉熟悉 $\alpha-\beta$ 模型。

@[video](https://kirigaya.cn/api/storage/video/4047442a-e953-439b-b476-ab8ba690b577.mp4)

首先，全部 $p-1$ 个 rank 上的数据全都 gather 到 rank0 上，由于 rank0 的下行带宽是有限的，所以真实情况下，不可能 $p-1$ 个 rank 全部并行都能把数据传输到 rank0，显然，rank0 自己的数据接收是有瓶颈的，整个系统的通信瓶颈全在 rank0 上，假设网络性能很好，这 $p-1$ 份数据全部同时发送，所以 $S(p)=1$，全部的数据一共为 $(p - 1)\cdot n$，rank0 用自己的带宽接收这些数据，所以 $F(p) = (p-1)\cdot n$。

所以此时系统通信耗时就等价于 rank0 的通信耗时：

$$
T_{\text{gather}} = \alpha + (p - 1)\cdot n \cdot \beta
$$
完成 gather 后，还需要进行 broadcast，显然这对操作近乎对称，所以 broadcast 的时间也是：

$$
T_{\text{broadcast}} = \alpha + (p - 1)\cdot n \cdot \beta
$$
因此，对于当前这个中心化的通信方案来说，它的通信开销就为：

$$
T_{\text{dummy}} = 2\alpha + 2(p - 1)\cdot n \cdot \beta
$$
所以我们有：

$$
\mathrm{Latency} = \mathcal{\Theta}(1), \mathrm{Bandwidth} = \mathcal{\Theta}(pn)
$$
有点算法经验的朋友估计都能看出来，这个算法的 Latency 复杂度虽然非常低，但是它的 Bandwidth 复杂度却是乘积，乘积的形式在复杂度分析里是我们不太希望看到的，因为两个很大的数字相乘会让数字更大。

有了这个作为基础，我们再看看别的吧！

### Ring

Ring 正如字面意思，它会让通信组的卡之间形成一个圈，比如我们当前有 gpu0, gpu1, gpu2, gpu3 四张卡，那么 Ring 算法会让四张卡首尾相连连成一个环：

```
gpu0 -> gpu1 -> gpu2 -> gpu3 -> gpu0
```

每张卡想要向别的卡通信，必须按照上面的方向去通信，比如 gpu2 向 gpu3 通信就是 `gpu2->gpu3`，反过来不能是 `gpu3->gpu2`，而必须是 `gpu3-> gpu0-> gpu1 -> gpu2`。

使用 Ring 算法实现 AllReduce 非常优雅，下面是动画展示：

@[video](https://kirigaya.cn/api/storage/video/01ff0fb3-29e8-4d78-9a8d-1c5af15b6733.mp4)

可以看到 Ring 算法在执行时先非常巧妙地使用了一次 ReduceScatter 让每一个 rank 都拿到了其中某一层的梯度的和，然后再通过一次 AllGather 让全部层的梯度和都落入各个 rank 中，并且 Ring 拓扑下的 AllGather 只需要往后继节点分别传递三次数值即可。

我们不妨计算一下这个情况下的通信，你会发现上面实现 ReduceScatter 和 AllGather 都是走了三轮，其实就是 $p-1$ 轮，所以实际上这个算法下的各个节点的通信是相当对称和均衡的，每个节点都向后继节点发送了 $2(p-1)$ 次数据，并且为了保证刚好是发送 $p-1$ 轮，数据会被分为 $p$ 份，那么每次的数据量就是 $n / p$，结合这些信息不难得出：

$$
S(p) = 2(p - 1), F(p)n = 2(p - 1) \frac{n}{p} = 2(1 - \frac{1}{p}) n
$$
所以我们就能得到 Ring 算法下的通信复杂度：

$$


\mathrm{Latency} = \mathcal{\Theta}(p), \mathrm{Bandwidth} = \mathcal{\Theta}(n)
$$
很漂亮，不是吗？你会发现 Ring 算法不仅不会受到单个 rank 带宽的影响，而且还让我们的带宽复杂度省去了乘积的爆炸复杂度。Latency 和 Bandwidth 都变成了线性复杂度。

### Tree

怎么样才能让各个节点彼此之间的相对距离更小，从而降低传递数据的次数呢？写过数据库内核或者数据结构学的好的朋友立刻就想到了一种数据结构——树。

假设我们现在有 7 个 rank，我们可以将原本的拓扑尽可能构建一棵逻辑上的 AVL（平衡二叉树），如下图所示：

![图片](https://kirigaya.cn/api/storage/image/05db197f-8d47-47b1-8e29-c3f6e3108011.webp)

进行 all reduce 时，只需要像小学上课收作业一样，让各个叶子节点一层层把梯度信息当做作业往父节点传递，父节点收到所有叶子节点的数据后，在本节点完成一次归约，然后把数据往上传递。最终根节点（图中的 R1）收到来自 R2 和 R3 的信息再归约一下，就是我们需要的梯度之和了。然后把结果从 R1 开始往下分发就好了。

这个做法在收作业的时候很优雅，但是发作业的时候就有点性能问题了：R3 和 R2 都有两个子节点，那么数据从 R1 -> R3 占用的带宽只有 R3 把数据分发到它的两个子节点带宽的一半，而这一段时间叶子节点都不干活，所以你能发现传统 AVL 结构在执行数据传输时内部节点往往承受了更大的通信压力，叶子节点闲着不干活，容易负载不均衡。

为了解决这个问题，NVIDIA 对这个做了一定的优化，Double Binary Tree，基本原理很简单，既然内部节点通信压力大，叶子节点容易闲着不干活，那我就再构造一棵二叉树，让内部节点尽可能成为叶子节点，让叶子节点尽可能成为内部节点，需要传输的数据分为两份，两棵树分别传递两份数据中的一份，然后继续上面的步骤就可以了，动画如下：

@[video](https://kirigaya.cn/api/storage/video/14ec42cd-78b5-4909-857f-e005a9748104.mp4)

我们不妨来计算一下这个算法的通信开销。

首先每次上发数据都可以看成父节点和子节点进行的一次 Reduce，目标是父节点，考虑到每次都是每一层同时发送数据给上一层，两颗树都是同时发送数据的，所以一次从叶子节点到根节点的数据传输的数据延时就相当于树的高度 $h$，再根据数据结构的常识，所以我们有：

$$
S(p) = 2h = 2\lceil\log_2 p\rceil
$$
在一棵树中，总的数据量为 $n / 2$，所有数据从叶子节点到根节点要全部走一遍，从根节点到叶子节点也是全部发送了一遍，所以单棵树的数据带宽为 $2 \times n / 2 = n$，所以考虑到两棵树，我们有：

$$
F(p)\cdot n = 2n
$$
所以 Tree 算法的通信开销为：

$$
\mathrm{Latency} = \mathcal{\Theta}(\log_2 p), \mathrm{Bandwidth} = \mathcal{\Theta}(n)
$$
可以看到，相比于 Ring 算法，Tree 算法在维持 Bandwidth 不变的情况下，将 Latency 的复杂度进一步下降了 $p \rightarrow \log_2 p$。

### CollNet & NVLS

前面讲了两种算法，更多是从逻辑层面上去完成规划的，但是正如我们上一章所言，GPU 集群的底层并非是完全对称的，卡与卡之间可能是在同一个 NUMA，可能是在同一台机器，可能是在不同机器中的，真实的底层硬件差异并没有在我们上面的算法中得到体现。

而 CollNet & NVLS 这两种算法，就考虑到了硬件层面的差异。通过上一章的学习，你应该能知道， intra-node 通信速度是高于 inter-node 的。所以从这里开始，NCCL 的算法就不再只考虑“数据应该组成一个环还是一棵树”，而是开始进一步考虑：**哪些通信应该留在高速的 intra-node 网络中完成，哪些数据才真正有必要进入昂贵的 inter-node 网络。**

CollNet 和 NVLS 就属于这种更明显依赖硬件能力的通信方案。先看 CollNet。

假设我们仍然有 4 个 rank，分别分布在两台服务器中，R1，R2 在一台，R3，R4 在一台。

假设我们还是用 Ring，按照上面的做法。那么从逻辑上看，R1、R2、R3、R4 仍然只是四个普通 rank，数据需要不断沿着 GPU、NIC 和网络之间转发。显然，这四个 rank 之间的通信并不是完全等价的，这种不等价的条件一加入，原本 Ring 的做法就不一定是通信最优了。

CollNet 利用了一个额外条件：「如果网络本身就具备 Collective Reduction 的能力，那么为什么一定要让 GPU 自己把所有跨节点 Reduce 做完？」例如 NVIDIA 的 SHARP，可以直接在支持该能力的 InfiniBand 交换网络中完成部分 reduction。CollNet 正是 NCCL 为这类 **in-network reduction** 提供的一套通信算法和接口。

所以对于 AllReduce 这类的算法，CollNet 会先让能归约的部分先在各个服务器节点内完成归约，将数据直接扔到网络中完成交换，对于支持 SHARP 的网络，在交换的过程中就能直接完成剩余部分的归约，从而把最终的梯度和在各个节点内完成，一个大致的动画如下：

@[video](https://kirigaya.cn/api/storage/video/0f227f9b-019a-49ad-b6d0-7c48def43c6d.mp4)

> 真实情况下，这几个阶段并不是严格串行执行的。真实 NCCL 会把数据切成 chunk，让 intra-node 数据传输、NIC 发送、网络 reduction 和结果返回构建一条可以 overlap 起来的流水线。

目前 NCCL_ALGO 支持的 CollNet 一共有两个：CollNetChain 和 CollNetDirect。这两者唯一的区别就是同一台服务器内的拓扑如何组织。CollNetDirect 在大部分场景下效果会更好一些。

如果你的服务器安装了 NVSwitch + NVLink 全家桶，从 Hopper 架构开始，NVIDIA 的 NVSwitch 提供 **NVLink SHARP 功能**，可以把部分集合通信 offload 到 NVSwitch 中。这个算法就是 NVLS，更多的内容大家可以去 NVIDIA 官网去了解细节。

---

## 结论

这一章带大家粗略过了一遍基本的集合通信和 NCCL，虽然看起来很枯燥，但是这是理解推理和训练 infra 中部分算法的基石。下一章开始，我们将开启分布式训练算法的讲解。

okey dokey，这里是 00 后全栈工程师锦恢，一个正在尝试撰写全网最小白最全面最系统的 AI 全栈系列教程（Agent + 大模型算法 + AI Infra）的学徒。我们最新做的 Agentic RL 框架 Dressage 也开源了，欢迎大家 star 支持一下：[Accio-Lab/Dressage](https://github.com/Accio-Lab/Dressage)，欢迎想要了解和学习大模型底层技术的相关朋友来阅读讨论。

![图片](https://kirigaya.cn/api/storage/image/e40d7d07-4aa0-4744-9b6a-ea231632147c.jpg)
