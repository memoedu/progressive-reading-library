## 前言

废话少说了，最近在尝试在一个垂类场景下做大模型的 PEFT 和 Agentic RL，刚好这两个玩意儿在参数矩阵的更新上都有 low rank 的特性，正好混在一块写一下。

---

## LoRA

LoRA 的提出源于大家在某个垂类领域微调大模型时发现的参数分布上的规律。由于我们现在的大模型的 OP 基本都是 Transformer，Transformer 里面参数主要都由 Linear 组成，比如 FFN 中的 MLP 和计算 QKV 用到的投影矩阵，它们本质上都是一个 Linear。而 Linear 这个 OP 的运算又可以表示为：

$$
y = Wx, W \in \mathbb{R}^{d_{\text{out}} \times d_{\text{in}}}
$$
其中 $W$ 就是参数矩阵。假设我们在垂类数据集上完成了微调，微调后的参数矩阵为 $W'$。长期的训练经验让我们注意到一个现象：在小数据上全量微调大模型时，模型虽然拥有非常多的参数，但真正有效的参数更新似乎并没有覆盖整个参数空间，而是集中在少数几个主要的变化方向上。

### LoRA 的基本原理

为了用更加数学的语言来描述这个问题，我们不妨考察一下这两个矩阵的差距：

$$
\Delta W = W' - W
$$
那么上面那句话就可以进一步翻译为：虽然 $\Delta W$ 本身仍然是一个和 $W$ 维度完全相同的大矩阵，但它所包含的有效变化方向可能并没有矩阵维度看起来那么多。换句话说，$\Delta W$ 可能具有较低的内在秩，或者至少可以被一个低秩矩阵很好地近似。

而训练的本质，其实就是在寻找这个 $\Delta W$。

换句话说，如果 $\Delta W$ 的有效变化主要集中在少数几个方向上，那么我们真的有必要自由地计算和训练整个 $\Delta W$ 吗？

细细思考你会发现，当我们在某个领域微调大模型，或者只是让大模型学习一门新的技术时，确实没有必要让模型在所有可能的参数方向上都发生独立变化。难道我们学习一道新的菜肴的烹饪技巧，就需要重新学习一遍中文吗？显然，这是没有必要的。

而且垂类场景的数据往往也比较少，大模型的参数量巨大意味着模型拥有极高的表达能力。使用少量数据去自由地更新如此庞大的参数空间，不仅成本很高，也很容易带来过拟合等问题。

这些思考都催生我们进一步追问：我们真的有必要完整训练得到整个 $\Delta W$ 吗？

我们注意到，虽然 $\Delta W$ 的维度非常高，但它的有效更新方向可能很少。如果大家对线性代数比较熟悉，此时应该很容易联想到矩阵的低秩分解。一个低秩矩阵可以通过两个规模更小的矩阵相乘来表示，例如：

$$
\Delta W = BA, B \in \mathbb{R}^{d_{\text{out}} \times r}, A \in \mathbb{R}^{r \times d_{\text{in}}}
$$
于是前向计算可以写成：

$$
y = W_0x + BAx
$$
训练时冻结 $W_0$，只训练 $A$ 和 $B$。

### 真实的 LoRA 公式

为了更加契合工程中的各种使用，上面的 LoRA 还不是目前的主流 LoRA 库中使用的公式，由于我们经常会调整 $r$，而 $BA$ 的方差又和 $r$ 正相关，所以为了训练更稳定，且可以随时调整 LoRA 部分的权重，我们会对上面的公式做出如下的改造：

$$
y = W_0x + b + \frac{\alpha}{r}BAx
$$
其中，$b$ 是 Linear 本来就具备的参数 bias，既然 LoRA 是对 Linear 的改造，那么这个 bias 肯定不能少。

然后 LoRA 有一种特殊的变种 rsLoRA，rsLoRA 的原论文指出，普通 LoRA 使用的 $1/r$ 缩放会在较大的 rank 下导致学习速度下降，因此提出使用 $\alpha / \sqrt r$ 代替 $\alpha / r$。然后在很多实现中，还会对 LoRA 项添加正则 dropout，所以最终的公式为：

$$
y = W_0x + b + \frac{\alpha}{r}BA (D_p(x))
$$
### LoRA 训练的参数量

全量微调需要训练的参数量为 $d_{\text{out}}d_{\text{in}}$，LoRA 需要训练的参数量为 $rd_{\text{in}} + d_{\text{out}}r$。一般来说，这个 $r$ 取值为 8, 16，而 $d_{\text{out}}$, $d_{\text{in}}$ 一般都是 1024，4096 这个级别的数字，也就是说，我们有 $r \ll d_{\text{in}},d_{\text{out}}$。所以 LoRA 的可训练参数量远小于全量微调。

同时，如果大家的线性代数没忘光的话，我们还有如下的基本不等式：

$$
\operatorname{rank}(\Delta W) = \operatorname{rank}(BA)
\leq
\min\left(\operatorname{rank}(B),\operatorname{rank}(A)\right) = r
$$
因此，LoRA 将参数更新限制在一个低秩空间中。

### LoRA 的代码实现

社区比较常用的 LoRA 库是 huggingface 的 peft 库。一个 example 如下：

```python
from peft import LoraConfig, TaskType, get_peft_model

config = LoraConfig(
    r=16,
    lora_alpha=32,
    lora_dropout=0.05,
    target_modules=[
        "q_proj",
        "k_proj",
        "v_proj",
        "o_proj",
    ],
    bias="none",
    use_rslora=False,
    task_type=TaskType.CAUSAL_LM,
)

model = get_peft_model(model, config)
```

对着上面的 `LoraConfig` ，和下面的公式

$$
y = W_0x + b + \frac{\alpha}{r}BA (D_p(x))
$$
把参数对应关系搞清楚了，基本上你就可以用 LoRA 微调大模型了。需要注意的是，peft 这个库并不支持多机多卡训练，对于特别大的模型，比如 qwen 397b，需要自己重新基于 megatron 或者其他的训练后端来完成多机多卡的支持。只需要你具备一定的 infra 层面的工程能力才能做到。

---

## Agentic RL

我在 [锦恢的 AI Agent 小白教程](https://kirigaya.cn/blog/collection?name=%E9%94%A6%E6%81%A2%E7%9A%84+AI+Agent+%E5%B0%8F%E7%99%BD%E6%95%99%E7%A8%8B) 中提到过 Agent 的一个基本原理。显然，驱动 Agent 的大模型和正常进行问答的大模型是并不一致的。你完全可以将 Agent 所做的这样的一个 ReAct，看成是某一个特殊场景下的任务。而针对特殊场景下的任务来进行训练，其实就符合我们最早提到的垂类数据训练的这样的一个基本训练范式。

在大模型领域，我们通常使用 Agentic RL 的技术，来让大模型被训练的可以执行各类 Agent 任务。Agentic RL 通常不是让模型重新学习所有语言能力，而是强化少数特定行为，例如：

* 什么时候调用工具；

* 选择哪个工具；

* 如何填写工具参数；

* 如何根据 observation 继续推理；

* 什么时候停止调用工具；

* 如何减少无效工具调用。

这是因为这种训练本身可看成是在这类垂直场景下的训练，所以有大量的工程中也使用了 LoRA 作为了 Agentic RL 中的模型训练参数。

ARTIST、ToolRL 等工作表明，Agentic RL 可以通过结果奖励或更细粒度的工具奖励，改善工具选择、参数生成、多轮推理和最终任务成功率。这些行为相对于整个语言模型能力空间可能是局部的，因此有理由猜测在 Agentic RL 中，$\Delta W$ 具有较低的**内在维度**或可以被低秩矩阵近似。也就是说，Agentic RL 的参数更新极有可能也是低秩的。那么 LoRA 完全可以结合进来，从而完成某些特定业务下的大尺寸模型训练。

我想，具体的文章在我参与 Dressage 对接工作后，会再写一篇吧。