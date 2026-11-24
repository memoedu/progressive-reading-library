## 前言

高级计算机网络考前临时抱佛脚第五篇。这次讲解的是网络流算法中的 Elastic Sketch。

在之前的章节中，我们简单阐述了网络流预测这个问题的背景。这次介绍的算法为 Elastic Sketch，相较于之前的 FlowRadar 算法，Elastic Sketch 可以更好地区分大流和小流，对于流量差异大的场景，效率更高。

---

## 回顾 Count-Min （CM）

CM 是一种用尽可能小的空间模糊估计的键值容器。空间复杂度比字典更小，但是有概率犯错。

原理也非常简单，CM 本身是一个 d 行，w 列的表格，每一行记为 $L_i(i=1,...,d)$。然后还给每行配置了一个哈希函数 $h_i$，$hi$ 保证把数字映射到 $[1,w]$ 。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9ea6c024e5a52bf4de5b892621d8bc2e_1440w.png" style="width: 70%;"/>
</div>

当需要更新 x 的值为 v 时，计算出 $h_i(x)$，然后 $L_i[h_i(x)] \leftarrow v (i=1,...,d)$。

当需要查询 x 的值时，返回 $\underset{i=1,...,d}{\min} L_i[h_i(x)]$。

当哈希发生冲突时，可能会让估计值偏大。但是 CM 能保证估计值一定不会偏小。

下面的图展示了，更新 x，y，z 和 查询 x，y 的过程：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-24efd50ad5c38e9ca7cf4a2420fb4092_1440w.png" style="width: 80%;"/>
</div>

---

## Elastic Sketch

Elastic Sketch 是一种流量识别算法，它能够根据流量大小自适应地将流量大的流量小的进行分别存储，从而最大化利用空间。虽然 Elastic Sketch 简称也是 es，但是它和 Elastic Search 是完全不同的两种东西哦！

es 包含两部分：

1. **heavy part（大象流）**：
    - 一张哈希表 H。
    - 哈希表的每一个位置存储一个四元数 $(f_1, \text{vote}^+, \text{flag}_1, \text{vote}^-)$，记作 bucket：
        - $f_1$：流ID（如5元组）
        - $\text{vote}^+$：正投票数（表示属于该流的包数量）
        - $\text{vote}^-$：负投票数（表示其他包的数量）
        - $\text{flag}_1$：标志位（表示轻部分是否包含该流的正投票）。

2. **light part（老鼠流）**：
    - CM sketch。



### 插入

在接收到流f的一个数据包时

- 通过哈希函数映射到$H[h(f)\%B]$，假设该桶的内容是$(f_1, \text{vote}^+, \text{flag}_1, \text{vote}^-)$

- case1：如果该桶为空，则插入$(f, 1, F, 0)$。

- case2：如果$f=f_1$，则 $\text{vote}^+$ 加1。

- case3：如果$f\not=f_1$，并且$\frac{\text{vote}^-}{\text{vote}^+}<\lambda$，则 $\text{vote}^-$ 加1，并通过将计数器加1的方式将$f_1$插入到 CM 中。

- case4：如果$f\not=f_1$，并且在 $\text{vote}^-$ 加1后，$\frac{\text{vote}^-}{\text{vote}^+}\ge\lambda$。则通过将该桶设置为$(f, 1, T, 1)$来弹出 f，并通过将计数器增加 $\text{vote}^+$ 的值来插入 $f_1$ 到 CM 中。



可以结合下方的例子进行理解，其中 $\lambda=8$

- f1：case2
- f5：case1
- f8：case3
- f9：case4

<div align=center>
<img src="https://pica.zhimg.com/80/v2-846dff20e79f186c77755cdce2ce2646_1440w.png" style="width: 80%;"/>
</div>

插入 f9 时，因为满足 case4，需要将原本 heavy part 内对应位置的 f4 弹出，并加入 light part 内。

### 查询

如果想要查询 f 的大小，分两种情况：

- 如果 f 不在 heavy part 内，使用 CM 的查询值作为估计值。
- 如果 f 在 heavy part 内，如果 Flag 为 F，返回 $\text{vote}^+$；如果 Flag 为 T，返回 $\text{vote}^+$ 和 f 在 CM 中估计值的和。



### 错误估计

最坏的情况就是 heavy part 中发生冲突了，概率为
$$
P_{hc} = 1 - (\frac Hw+1) e^{-\frac{H}{w}}
$$

> H is the number of elephant flows and w is the number of buckets.