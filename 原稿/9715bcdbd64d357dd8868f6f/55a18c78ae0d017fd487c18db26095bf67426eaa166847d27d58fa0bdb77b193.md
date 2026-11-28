## 问题背景

在pytorch中为什么会使用pack_padded_sequence和pad_packed_sequence，讲一句话的id序列直接嵌入再跑一遍RNN不香吗？
举个简单的例子：
```python
import torch
from torch import nn

# 输入的句子的id序列
txt_id_seq = torch.tensor([[3,2,1,0],    
                           [2,1,0,0],
                           [5,0,0,0]])

embed_layer = nn.Embedding(num_embeddings=6,  # 可能会嵌入的数量，也就是词表的长度
                           embedding_dim=3,   # 单个id值的嵌入的维度，也就是词向量的维度
                           padding_idx=0)     # <PAD>的id值，默认为0

embed_seq = embed_layer(txt_id_seq)   # 将id映射成词向量

print("id序列嵌入后的形状：", embed_seq.shape)
print("嵌入矩阵的形状：", embed_layer.weight.data.shape)

gru = nn.GRU(input_size=3,       # 输入的单个time step的维度，也就是词向量的维度
             hidden_size=5,      # 隐层的维度
             num_layers=1,		 # 每个门控循环单元的层数
             batch_first=True)   # 导入的数据形状为[B, T, *]

out, h_n = gru(embed_seq, None)     # 将词向量的维度，从embedding_dim映射到hidden_dim
print("RNN的out的形状：", out.shape)
print("RNN最后一个cell的输出值的形状：", h_n.shape)

```
out:
```
id序列嵌入后的形状： torch.Size([3, 4, 3])
嵌入矩阵的形状： torch.Size([6, 3])
RNN的out的形状： torch.Size([3, 4, 5])
RNN最后一个cell的输出值的形状： torch.Size([1, 3, 5])
```

上述过程就是一般NLP任务feature extraction的步骤，我们一般会取循环神经网络out的最后一个（也就是最后一个输出的o_t）作为整句话的语义特征。

<div align=center>
<img src="https://img-blog.csdnimg.cn/20201001181748237.png" style="width: 60%;"/>
</div>

那么我们看看我们随便取的三个id序列最后的语义特征是什么。
```python
for i in range(3):
    print(out[i])
```
out:
```
tensor([[-0.3654,  0.0183, -0.4721, -0.5012, -0.0049],
        [-0.3866, -0.2832,  0.0739, -0.0595,  0.2906],
        [-0.1104,  0.1980,  0.0995,  0.5426,  0.1445],
        [-0.1349, -0.1310,  0.0822,  0.3626,  0.0617]],
       grad_fn=<SelectBackward>)
tensor([[-0.2425, -0.3400,  0.3298,  0.1249,  0.1939],
        [-0.0271,  0.1630,  0.2649,  0.6118,  0.0186],
        [-0.1007, -0.1612,  0.1775,  0.3766, -0.0449],
        [-0.1405, -0.2970,  0.1338,  0.2727, -0.0831]],
       grad_fn=<SelectBackward>)
tensor([[-0.0678, -0.1343, -0.1453, -0.1782, -0.3926],
        [-0.1260, -0.2608, -0.0760,  0.0901, -0.2029],
        [-0.1359, -0.3325, -0.0125,  0.1766, -0.1350],
        [-0.1429, -0.3722,  0.0308,  0.2016, -0.1150]],
       grad_fn=<SelectBackward>)
```
我们原来的id序列是`[[3,2,1,0],[2,1,0,0],[5,0,0,0]]`，其中\<PAD\>是0，是没有意义的填充符，但是这三个id序列在经过嵌入，GRU后得到了上面out中的结果，第一个id序列`[3,2,1,0]`被映射到了
[[-0.3654,  0.0183, -0.4721, -0.5012, -0.0049],
        [-0.3866, -0.2832,  0.0739, -0.0595,  0.2906],
        [-0.1104,  0.1980,  0.0995,  0.5426,  0.1445],
        [-0.1349, -0.1310,  0.0822,  0.3626,  0.0617]]
其中的0被映射到了矩阵的最后一行的  [-0.1349, -0.1310,  0.0822,  0.3626,  0.0617]。代表\<PAD\>的0应该是没有意义的，但是最终映射得到的结果是一串非稀疏的向量，而且没有意义的0还需要经过如上的一串运算，看起来并不是最好的结果。

所以pytorch通过`pack_padded_sequence`和`pad_packed_sequence`这两个效果互逆的方法给与了RNN处理变长序列的能力。这样可以有效减小\<PAD>对运算效率与结果的影响。其实就是通过一个pack(压缩)和解压(unpack)的过程让网络的前向计算跳过\<PAD>所对应的词向量的计算过程。

我们试着对上述的嵌入矩阵做如下的处理：

```python
embed_seq_pack = nn.utils.rnn.pack_padded_sequence(input=embed_seq,       # 需要编码的batch_size个话
												   lengths=torch.tensor([3, 2, 1]), # 每句话的长度
												   batch_first=True)  # 输入的形状为[B, T, *]

out_pack, h_n_pack = gru(embed_seq_pack, None)    # gru支持编码后的序列输入

out, _  = nn.utils.rnn.pad_packed_sequence(sequence=out_pack,  # 需要解码的序列
										   batch_first=True,   # 解压的形状为[B, T, *]
                                           padding_value=0)    # 解压后的<PAD>的id值

print(out.shape)
print("输入的第三个id序列：", txt_id_seq[2])
print("对应的特征映射：", out[2])
```

out:
```
torch.Size([3, 4, 5])
输入的第三个id序列： tensor([5, 0, 0, 0])
对应的特征映射： tensor([[-0.2815,  0.4022, -0.2261,  0.1051, -0.2380],
        [ 0.0000,  0.0000,  0.0000,  0.0000,  0.0000],
        [ 0.0000,  0.0000,  0.0000,  0.0000,  0.0000],
        [ 0.0000,  0.0000,  0.0000,  0.0000,  0.0000]],
       grad_fn=<SelectBackward>)

```

从上述程序中可以看到`pack_padded_sequence`和`pad_packed_sequence`的使用方法：`pack_padded_sequence`接受batch_size个通过嵌入矩阵的词向量拼接成的矩阵，和一个长度为batch_size的一维tensor，这个tensor代表该batch中每个句子的长度。通过参数batch_size来指定数据的组织方式是[B, T, *]还是[T, B, *]。得到一个PackedSequence类（压缩序列类），这个类通过指定句长的参数去掉了所有的\<PAD>词向量，我们可以打印上述三个id序列的PackedSequence类和原本的嵌入矩阵看看：
```python
print("三个id序列的嵌入矩阵为：", embed_seq)
print("三个句子嵌入矩阵的压缩序列类：", embed_seq_pack)
```

```
三个id序列的嵌入矩阵为： tensor([[[-0.0358,  1.0701,  1.5646],
         [-0.0347, -0.3378,  0.2072],
         [ 2.0477, -0.0644,  0.2723],
         [ 0.0000,  0.0000,  0.0000]],

        [[-0.0347, -0.3378,  0.2072],
         [ 2.0477, -0.0644,  0.2723],
         [ 0.0000,  0.0000,  0.0000],
         [ 0.0000,  0.0000,  0.0000]],

        [[-0.8102,  1.3946,  0.1938],
         [ 0.0000,  0.0000,  0.0000],
         [ 0.0000,  0.0000,  0.0000],
         [ 0.0000,  0.0000,  0.0000]]], grad_fn=<EmbeddingBackward>)
三个句子嵌入矩阵的压缩序列类： PackedSequence(data=tensor([[-0.0358,  1.0701,  1.5646],
        [-0.0347, -0.3378,  0.2072],
        [-0.8102,  1.3946,  0.1938],
        [-0.0347, -0.3378,  0.2072],
        [ 2.0477, -0.0644,  0.2723],
        [ 2.0477, -0.0644,  0.2723]], grad_fn=<PackPaddedSequenceBackward>), batch_sizes=tensor([3, 2, 1]), sorted_indices=None, unsorted_indices=None)
```
因为我们指定了`batch_first=True`，所以所有的全0行向量都被去掉了。这么处理后的序列参与gru的运算时，原本的0向量就不参与运算，最后直接映射到全0序列。

需要注意的是，`pack_padded_sequence`中代表句长的向量，默认是需要强制降序，如果你给的不是降序的向量，会报错，此时你可以手动通过排序来解决，也可以通过设置`pack_padded_sequence`中的参数`enforce_sorted=False`来解决（因为`enforce_sorted=False`时，程序内部会无条件排序）。
