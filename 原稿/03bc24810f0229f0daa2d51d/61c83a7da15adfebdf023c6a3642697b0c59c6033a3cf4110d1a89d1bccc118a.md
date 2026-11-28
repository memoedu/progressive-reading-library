## 前言

高级计算机网络考前临时抱佛脚第四篇。这次讲解的是网络流算法中的 FlowRadar。

---

## 简单复习：哈希表 & 布隆筛

哈希表 hash table 就不用多说了，这个用得非常非常多。讲解 FlowRadar 前，带着大家复习一下布隆筛（Bloom Filter）。

布隆筛是用于模糊解决 **Membership problem** 的一个算法，所谓 **Membership problem**，也就是判断一个元素是否属于一个集合。你当然可以使用哈希表或者并查集去实现，但是这两数据结构都需要存下所有曾经到访过的数据，在我们今天的任务背景下，它们的时间复杂度都太高了。

而布隆筛则是一种类似指纹算法的算法，它不直接记录到访的数据，而是记录到访数据的指纹。所以空间复杂度非常低。

布隆筛（Bloom Filter）是一种空间效率很高的概率型数据结构，它使用位数组来表示一个集合，并能够以较低的错误率判断一个元素是否属于这个集合。下面是根据您提供的参考详细解释布隆筛的工作原理

### 布隆筛的组成

布隆筛由一个长度为 m 的 bit 和 k 个哈希函数组成，记作 bloom(m, k)。理论空间复杂度为 $\mathcal O(m+k)$，和集合大小无关。

* **长度为 m 的 bit**：一个包含 `m` 位的数组，所有位初始化为0。这个数组用于存储集合 `S = {x1, x2, ..., xn}` 中元素的“存在”信息。
* **哈希函数**：`k` 个独立的哈希函数 `h1, h2, ..., hk`，这些哈希函数的输出范围都在 `{1, 2, ..., m}` 之间。这些哈希函数用于将集合中的每个元素映射到位数组中的 `k` 个位置。

### 插入元素

当要向布隆筛中插入一个元素 `x` 时，进行以下操作：

1. 对元素 `x` 应用 `k` 个哈希函数，得到 `k` 个哈希值 `h1(x), h2(x), ..., hk(x)`。
2. 将位数组中对应这 `k` 个哈希值位置的位设置为1。注意，由于哈希冲突的存在，不同的元素可能会将位数组中相同的位设置为1。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f480cc6f6fcb858e5a15d3d756f9cb80_1440w.png" style="width: 60%;"/>
</div>

### 检查元素

要检查一个元素 `y` 是否在集合 `S` 中时，进行以下操作：

1. 对元素 `y` 应用相同的 `k` 个哈希函数，得到 `k` 个哈希值 `h1(y), h2(y), ..., hk(y)`。
2. 检查位数组中对应这 `k` 个哈希值位置的位是否都为1。
    - 如果其中任何一位为0，则确定元素 `y` 不在集合 `S` 中（因为如果在集合中，那么这 `k` 个位置都应该被设置为1）。
    - 如果这 `k` 个位置的位都为1，则**可能**（但不绝对）认为元素 `y` 在集合 `S` 中。这是因为可能存在哈希冲突，即有其他元素也将这 `k` 个位置的位设置为1。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4a7a7947f74270e51c66dbdab8884640_1440w.png" style="width: 60%;"/>
</div>

### 注意事项

* 布隆筛是一种概率型数据结构，它可能会产生误报（false positive），即错误地认为某个元素在集合中，但实际上并不在。但布隆筛绝不会产生误报（false negative），即它不会错误地认为某个元素不在集合中。
* 误报率（false positive rate）与布隆筛的大小（`m`）、哈希函数的数量（`k`）以及集合中的元素数量（`n`）有关。一般来说，增加布隆筛的大小、哈希函数的数量或降低集合中的元素数量都可以降低误报率。
* 布隆筛不支持删除操作。一旦一个元素被插入到布隆筛中，就不能从其中删除，因为删除操作可能会影响到其他元素的判断。
* 在实际应用中，通常假设 `kn < m`，以确保布隆筛能够有效地工作。如果 `kn` 接近或超过 `m`，误报率会显著增加。

---

## 特殊的布隆筛：Counting Bloom Filter

Counting Bloom Filter（计数布隆过滤器）是布隆过滤器（Bloom Filter）的一个变种，用于解决布隆过滤器无法删除元素的问题。在普通的布隆过滤器中，每个条目是一个位（bit），只能表示元素是否可能存在；而在计数布隆过滤器中，每个条目是一个小的计数器（counter），可以表示元素被插入的次数，从而允许进行删除操作。

以下是Counting Bloom Filter的主要特点和工作原理：

原本的 m 个 bit 更换为了 m 个计数器，可以记录元素被插入的次数。由于每个条目都是计数器，可以通过减少计数器的值来“删除”元素。

当要插入一个元素时，使用多个哈希函数将元素映射到数组中的多个位置。

当要查询一个元素是否存在于集合中时，同样使用相同的哈希函数将元素映射到数组中的多个位置。

- 检查这些位置对应的计数器的值是否都大于0。
- 如果是，则可能认为元素存在于集合中（但可能存在误报）。
- 如果其中任何一个计数器的值为0，则确定元素不在集合中。

> 在某些情况下，通过计数器的调整可以降低误报率（尽管仍然不能完全消除）。

---

## 监视器算法 NetFlow 的局限

NetFlow 是一个网络流量分析系统，用于捕获和分析IP数据包在路由器接口之间的流动情况，由思科开发。它被设计用于监控网络流量，提供关于网络流量的详细信息，如源地址、目的地址、端口号等，从而帮助网络管理员识别和解决网络瓶颈、安全威胁和性能问题。通过解码原始数据并将其编码成流集，NetFlow 可以定期生成报告并进行分析，以便更有效地管理和优化网络资源。

NetFlow 的方法就是维护一张哈希表，将网络流的基本信息（packet counter, flow starting time, the time that the flow is last seen, etc.）存入一张哈希表。这么做有两个问题：

1. 需要及时去除旧的网络流，因为网络传输每秒都在发生，一直记录会占用大量的CPU和内存资源。
2. 对于体积庞大的数据包而言，哈希表IO和更新的速度太慢了。

---

## FlowRadar 基本思想

因此 NetFlow 的基本思想就是，在交互机处将网络流计数器进行编码，编码后的数据包满足如下三个条件：

- 每个包的插入时间固定，内存固定
- 不采样捕获所有流
- 在短时间内(如10ms)内向收集器报告

然后再在远程的收集器上对网络流计数器进行解码，因为远程服务器的计算性能足够强大。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-49a6712e8fcc1b9d41be44674d2fb067_1440w.png" style="width: 60%;"/>
</div>



### Encode Flowset 数据结构

FlowRadar 中，用于对 flow 进行编码的数据结构被称为 Encode Flowset。一个 Encode Flowset 由 flow filter 和 counting table 组成：

- flow filter：一个标准的布隆筛 $\mathrm{bloom}(m_f, k_f)$ 
- counting table：一个扩展计数布隆筛，长度为 $m_c$，带 $k_c$ 个哈希函数。每一个单元由三个计数器组成

使用 python 表示 counting table 为：

```python
@dataclass
class BloomCell:
    FlowXOR = 0
    FlowCount = 0
    PacketCount = 0

CountTable: list[BloomCell] = []
```

为了方便叙述，我们后文中将这个 python 类，也就是一个三元数，称为 cell。CountTable 就是一堆 cell 的集合。

使用 Encode Flowset 对 flow 进行编码的算法如下：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-0f1205a10790ccc823d7567d4962db37_1440w.png" style="width: 60%;"/>
</div>

转写成带有注释的 python 代码如下：

```python
# 布隆筛
class BloomFilter:
    def __init__(self):
        ...
    # 将元素入筛
    def add(self, x: int) -> bool:
        ...
    # 判断元素在不在筛内
	def __contains__(self, x: int) -> bool:
        ...

class ExtentBloomFilter:
    # 加入元素后 FlowXOR 计数器 ^= flowxor
    def update_flowxor(self, x: int):
        ...
	
    # 加入元素后 FlowCount 计数器 +1
    def update_flowcount(self, x: int):
        ...

    # 加入元素后 PacketCount 计数器 +1
    def update_packetcount(self, x: int):
        ...
        
bloom_filter = BloomFilter()
ext_bloom_filter = ExtentBloomFilter()
```

定义完数据结构后，代码非常简单：

```python
# 获取流
p = ...

if p.flow not in bloom_filter:
    bloom_filter.add(p.flow)
    ext_bloom_filter.update_flowxor(p.flow)
	ext_bloom_filter.update_flowcount(p.flow)
ext_bloom_filter.update_packetcount(p.flow)
```



### SingleDecode

当然流通过上面的方法进行编码后，想要解码也非常简单。

底层的交换机会每隔若干 ms 将当前的信息同步到远程的收集器终端。收集器要做的事情就是记录下当年的流量快照，也就是上面的一堆数据结构，然后找到当前存在的一个比较干净的流，也就是 FlowCount 为 1 的流，因为这意味着当前这个位置只有一个流。

假设我们现在发现了一个 f1，它的 cell 为 `CountTable[h1(f1)] = (f1, 1, 10)`，第二位是 1 ，说明这个 cell 当前只代表了f1，没有别的数字，所以记录下 f1 的 PacketCount 后就可以把 f1 扔出去了。服务器记录下了若干组 f1 的 PacketCount 后就能绘制 f1 流的流量曲线了，从而达到监控流量的目的。 

扔出去的方法也非常简单，在 f1 上遍历所有哈希函数，然后 FlowXOR 计数器进行异或，另外两个计数器减去 CountTable[h1(f1)] 里面的对应值。

比如现在还有第二个哈希函数 h2，`CountTable[h2(f1)] = (f1 ^ f2, 2, 25)`，将 `CountTable[h1(f1)]` 的信息从中扣除后，就变成了 `(f2, 1, 15)`。异或的数学性质保证了 FlowXOR 计数器 的成分在多次操作后，能将被去除的数字，从中剔除出去。剔除后的 cell 的 FlowCount 也是 1 了，这代表当前的 cell 只有 f2 的成分，于是记录下 cell 的 PacketCount 作为当前时刻 f2 的流量大小。

以上这套算法被我们称为 SingleDecode。它的正式写法如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-594ad0b9e4cf0b5bc1b631cb0f18dcc7_1440w.png" style="width: 60%;"/>
</div>



### 没有 FlowCount 为 1 的 cell 咋办？ FlowDecode

但是真实情况下，可能会出现突发流量，此时很可能不存在 FlowCount 为 1 的 cell，FlowRadar 的做法就是既然当前交换机给的数据不能进行解码，那么隔壁交换机的相同流能不能解码？于是有了 FlowDecode 算法，基本做法也很简单，先对所有流进行 SingleDecode 解码，然后如果当前交换机给的数据都无法使用 SingleDecode 解码，就使用隔壁交换机中存在 FlowCount 为 1 的数据来扣除当前流集合中的。算法如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-fc123203dd39d8d299b6d3f9b184b8fe_1440w.png" style="width: 60%;"/>
</div>















