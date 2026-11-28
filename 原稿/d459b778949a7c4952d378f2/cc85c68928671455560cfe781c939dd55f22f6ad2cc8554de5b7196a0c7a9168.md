## 概要

torch.fx中，可以通过symbolic_trace函数，将一个Module或者函数映射到GraphModule，这是一个易于解析的IR（个人理解这就是一个静态计算图）。下面将叙述一下其中最基本的逻辑。

为了简单起见，我们假设输入的就只是一个Module，而且`concrete_args`参数为None。

## 基本数据结构

由于跟踪需要输出计算图，所以不可避免地需要规定其中节点和图本体的数据结构。规定好数据结构，后续的构建流程就非常简单了。

> 关于静态计算图是如何构建深度学习框架的，我之前写过一篇[纯Python实现原理级深度学习框架（一）计算图的原理，节点类的实现和计算图的可视化 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/351961835)。方便起见，下面我只会罗列torch.fx中的静态图的数据结构的不同

### Node

Node类构造函数的各项参数如下（参考torch.fx下的node.py）：

- graph：指明实例化的Node属于哪个Graph
- op：节点的类型。一共有如下的几种类型：
  - placeholder：占位符，一般代表输入。
  - call_method：表示一种操作，该操作表示让前驱节点的输出对象调用自己的方法。
  - call_module：表示一种操作，该操作表示将前驱节点的输出输入到nn.Module中。
  - call_function：表示一种操作，该操作表示将前驱节点的输出输入到一个函数中。
  - get_attr：表示一个操作，该操作获取Module自己的一个属性，并保存到输出中。
  - output：输出节点，表示该节点是所属Graph的输出节点，即在所属Graph中无后继节点。
  - root：整个Node在底层的数据结构是一个循环双向链表，root代表这个双向链表的头，是一个空的链表节点，用于维护双向链表。
- name：节点的名字。
- target：该节点需要调用的对象。如果op是call_function，那么target必须是一个Callable，否则必须是str。
- args：需要传递给target的变长参数。
- kwargs：需要传递给target的位置参数。
- return_type：代表该节点的输出数据的数据类型。（感觉基本没啥用，反正都duck type了）

> 关于name，op，target在一个具体的输入Module中对应什么，可以参考这篇文章：[PyTorch新技能解锁：torch.fx - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/428735136)

除此之外，Node在初始化时会创建几个属性，其中有几个比较重要：

- _input_nodes：一个哈希表，key是Node，value是None：代表self在Graph模式下的所有前驱节点。
- users：一个哈希表，数据类型同_input_nodes，代表self在Graph模式下的所有后继节点。
- _prev：self底层存储逻辑的前驱节点。
- _next：self底层存储逻辑的后继节点。

剩下来对于双向循环链表的插入（往前还是往后）和删除在Node中都实现了。

总结一下，Node通过_input_nodes和users来表示原本计算图的拓扑结构。其管理和存储是通过双向循环链表来的（和CPython的堆变量管理类似）

### Graph

由于Node中已经定义了完整管理计算图的属性和方法，因此Graph更多是对Node的管理和封装。

首先Graph在初始化时会创建一个root节点（在一张图中，有且仅有一个root）：

```python
self._root : Node = Node(self, '', 'root', '', (), {})
```

这个root就是底层用于存储和管理Node的双向循环链表。并且指定了插入节点的方法为向前插入。

在Graph的create_node方法中。就是创建一个node，然后再插入到链表中。

除此之外，Graph还指定了一些用于描述上下文、所属模块的信息，这些和后续的原理关系不大，就不赘述了。

### GraphModule

GraphModule是对Graph的封装，为啥还要封装呢？因为Graph的mro中没有nn.Module，为了工程规范，GraphModule继承了nn.Module并对Graph做了一个简单的封装。

### Proxy

正如其名，Proxy类是对Node类的一层包裹，它允许用户在不修改原图的情况下，用自定义的函数代理其中的节点，从而完成重载。

### Tracer

Tracer类是对符号跟踪的一层抽象，它的`symbolic_trace(m)`等价于`Tracer().trace(m)`。Tracer继承自TracerBase。

而symbolic_trace也只是对Tracer().trace的输出结果使用GraphModule进行了一次包装。因此，只需要看懂Tracer().trace的实现逻辑即可。

## symbolic trace流程

trace的步骤大致可以分为如下几步：

1. 解析输入Module基本信息：输入的module会被赋值给self.root，并且会对module的forward（默认）的函数签名进行解析。
2. 创建Graph
3. 为module的forward的除了self之外的每个输入参数创建其对应的Node以及Proxy
4. 进入patcher的context中（该context用于在调用patch_method方法时对部分的模块的函数进行暂时性地行为改变，等到退出context，所有的修改都会被撤销）：
   1. 暂时性修改nn.Module类的\__getattr__方法。
   2. 暂时性修改nn.Module类的\__call__方法。
   3. 对需要包裹的模块进行检查。
   4. 创建output节点。

在上述步骤中，关于Graph的变化只有两个地方，当3结束时，Graph中就会出现输入参数的Node；当4.4一结束，各个操作的中间结果（output可以看成所有中间结果的最后一个）的Node就会被创建。

> 额外说一句，为什么4.4只是创建output节点就能一下把前面的Node也全都创建了呢？因为创建节点需要args，也就是被创建节点的输入。在Tracer中有一个方法create_arg，这个方法会创建那些需要被用到，但是不存在于graph的节点，而且由于创建的节点的args的args也可能不存在，因此create_arg会递归地调用下去，直到输入地placeholder。

通过上述步骤之后，返回self.graph就是跟踪得到的结果了。