## 前言

本篇讲解 MPTCP 协议，对于 TCP 不熟悉的朋友可以去看看小林 coding 整理的 [TCP 面试题](https://xiaolincoding.com/network/3_tcp/tcp_interview.html#tcp-%E5%9F%BA%E6%9C%AC%E8%AE%A4%E8%AF%86)。虽然我个人非常方案八股文，但是不得不说，小林 さま 写的是真的好，可以说 TA 关于 HTTP 及其衍生的讲解，虽然是面经，但是作为教科书级别的讲义也是合格的。

---

## MPTCP 设计背景

在我们日常使用的网络协议，比如你上网看的浏览器，在百度网盘下载的学习资料，这些底层大多是面向连接的可靠传输，也就是基于 TCP 实现。在数据中心或者比较复杂的情况下，可供 TCP 使用的网络路径不止一条（因为这些设备往往有一堆的以太网接口，不像大部分人手里的 PC，只有一个 inet0）。为了更快进行网络传输，有没有什么办法可以在底层实现一种网络协议，让计算机自动占用多条连接链路来实现一个 TCP 连接，顶层用户在编写应用层网络协议时，则完全不需要关心底层的复用机制。这种可以让一条 TCP 链接同时占用多条网络链路的协议被我们称为 **MPTCP（Multipath TCP）**。

### MPTCP 的应用

事实上， MPTCP 已经被纳入了 [RFC 8684](https://www.rfc-editor.org/rfc/rfc8684.html) 标准中，在不少编程语言（比如 Go 1.21+）中已经被内置网络库实现。在 go 中，可以使用内置网络库 `net` 如此开启一个 mptcp 连接：

```go
func launch() {
    addr := flag.String("addr", ":8080", "service address")
    config := &net.ListenConfig{}
	if config.MultipathTCP() {
        // 默认mptcp是禁用的
		panic("MultipathTCP should be off by default")
	}
    // 主动启用mptcp
	config.SetMultipathTCP(true)
    // 开启 mptcp
    link, err := config.Listen(context.Background(), "tcp", *addr)
    // ...
}
```

在 IOS 7 之后，苹果的产品在连接上 wifi 后能开启 MPTCP，主要的连接通过 wifi 进行，手机本身的 4G 则是备用连接，在一定情况下，IOS 7 允许设备同时使用这两个连接进行数据传输，或是保证信息的稳定性。在目前的诸多手机上，这已经成为了非常常见的做法。

在大型数据中心就更不用说了。Linux 甚至直接实现了 MPTCP 以供数据中心的服务器集群进行使用：[Linux kernel implementation](https://multipath-tcp.org)



### MPTCP 在技术栈上所处的位置

在技术栈上，因为要对 TCP 进行复用，又不能向应用层的用户暴露实现细节，所以 MPTCP 介于 TCP 和 socket 之间。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-4efc60eb8ffe19f780d5b95d9221ba9c_1440w.png" style="width: 60%;"/>
</div>

---

## MPTCP 的设计准则

为了实现 MPTCP，必须满足如下的基本准则：

- 能够为单个连接使用多个网络路径。
- 至少能够像使用常规 TCP 一样使用可用的网络路径，但不会让 TCP 进入 starving 状态。
- 对于现有的应用程序，可以像常规TCP一样可用(不需要更改终端主机)。

这几条简直就像废话一样，copy 即可。

---

## MPTCP 连接

学过 TCP 的朋友都知道，这种面向连接的玩意儿，要点无非这么几个：

- 如何保证连接的？
- 如何进行拥塞控制的？
- 如何控制首发顺序的？

我们先来看第一个，MPTCP 如何进行连接。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9673def44f7d833203ea28e0d26b8530_1440w.png" style="width: 60%;"/>
</div>

和普通的 TCP 一样， MPTCP 在建立第一个连接时也需要进行三次握手，不同的地方，因为 MPTCP 需要创建额外的通道，这些额外创建的通道被我们称为 **子流（subflow）**。为了让目标服务器知道多条看似平平无奇的连接其实在组团零元购，MPTCP 需要在创建子流时，让子流的三次握手的SYN数据包携带 MP_JOIN 选项，并携带第一次创建的连接的一些基本信息，这些信息其实就是一个 MP_CAPABLE token，如上图所示。这样服务器就知道现在来访问的N条请求其实 使能 了 MPTCP 协议。

---

## MPTCP 拥塞控制

拥塞控制（congestion control，简称 cc）亦在阻止客户端向服务端发送请求过快导致丢包太多，而不断重新发起请求，加重服务器的负担，导致最终大家全都完蛋。

### 普通 TCP 拥塞控制

先简单回顾一下普通 TCP 的拥塞控制，先维护一个拥塞窗口 w，用于控制每次发送的报文数量（窗口内放的都是 on the fly 状态的数据包，也就是发出去了，但是还没有被回应，就和你的手机里每天催甲方或乙方的消息一样，全都是已读不回），然后：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-b9c06681c64092a5214dc2867fcbd8d5_1440w.png" style="width: 80%;"/>
</div>

- *w* can be viewed as packet on the fly (sent out but not yet acked)
- RTT is the time for a packet to be sent and acked.
- So, in an RTT, if *w* packets are sent and acked, window size in increased by *w**1/*w*=1. Increase the window by one packet per RTT.
- Called AIMD: Additive Increase/Multiplicative Decrease

其实也就是一个倍减的思想了，看过我往期倍增算法的朋友对这个做法应该倍感亲切。倍增倍减常用于不知道系统边界的情况，进行一定的控制。之前的倍增是用来控制内存申请，现在的倍减是用来控制网络负载。

### 公平性 MPTCP cc 算法 EWTCP

每一个子流都维护自己单独的拥塞窗口 $w_r$，最简单的做法就是，和之前的 TCP 一样，不过这里的网络因为是复用的。增长的量有点不一样，这就是最简单的 EWTCP cc 算法，如下

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2cba663aa4ecce01e2d82f98a6771a6a_1440w.png" style="width: 80%;"/>
</div>

- 这个算法对于每一个子流都是公平的
- 这个算法有效的前提是，子流之间是独立的。即当前子流拥塞时，其他子流不会变得更加扩大自己的拥塞窗口。

除此之外， MPTCP 不能很好地利用网络，下面是一个例子

<div align=center>
<img src="https://picx.zhimg.com/80/v2-08432d65bc20c6c8a76df9c7d035300c_1440w.png" style="width: 80%;"/>
</div>

- Three links each has 12Mbps
- Link bandwidth evenly split by three identical subflows (although they from different connections)
- If split traffic evenly in two paths, each subflow has 4Mbps, total 8Mbps
- Should shift all traffic to the least-congested path, get 12Mbps.



### 高效选择网络的 cc 算法 COUNPLED

EWTCP 一个最大的问题就是假设了子流之间都是独立的，也就是解耦合的（decoupled），COUNPLED 假设了子流之间是耦合的（counpled），算法名由此而来。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-77b78f095b5b7b939254ebeb390d31e4_1440w.png" style="width: 80%;"/>
</div>

其中的 $w_{\text{total}}$ 通过如下方程解出：
$$
\frac{w_r}{\text{RTT}}(1 - p)\frac{1}{w_{\text{total}}} = \frac{w_r}{\text{RTT}}p \frac{w_{\text{total}}}{2}
$$
$p$ 代表了路径 r 上的丢包概率。解出：
$$
w_{\text{total}} \approx \sqrt{2 / p}
$$
该算法在开始时虽然会和 EWTCP 一样，都去抢占最不拥塞的通道，但是随着算法的进行，最终各个通道的丢包率会达到一个均衡。

下面这个例子很好表示了这种均衡有何好处。

假设现在有四条通道，带宽分别为5，12，10，3，现在有三条不同的，且都有两条子流的 mptcp 连接，分别记作 A，B，C，它们的子流和四条管道的分配关系如下。左图是 EWTCP 的工作结果，右图是 COUNPLED

<div align=center>
<img src="https://picx.zhimg.com/80/v2-0fae48626d75555eef2e7a453333923d_1440w.png" style="width: 80%;"/>
</div>

EWTCP 因为假设了子流之间是独立的，所以会将每个管道内的流量平均分配给参与进来的子流。因此，管道被切分为了

- 5MB：5
- 12MB：6 + 6
- 10MB：5 + 5
- 3MB：3

最终A，B，C的流量分别为 11（5+6），11（6+5），8（5+3）。

而 COUNPLED 算法由于丢包率的加入会让各个连接最终达到均衡，A，B，C都为 10（(5 + 12 + 10 + 3) / 3）。

### RTT 错配补偿算法

EWTCP 和 COUNPLED 都假设了不同管道的 RTT 相同。RTT 错配补偿算法 主要解决了当 RTT 不相同时，之前的两个算法的问题。当考虑了 RTT 不同的因素后，算法如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ed5c61cc8adf2f7c8493c19e5e76d9a0_1440w.png" style="width: 80%;"/>
</div>





