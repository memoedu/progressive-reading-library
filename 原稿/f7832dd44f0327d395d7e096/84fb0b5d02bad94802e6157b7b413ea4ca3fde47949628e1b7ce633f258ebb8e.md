## 前言

刚刚吃完饭，写篇博客消消食。

TITO（Token in, token out） 是 Agentic RL 训练中会遇到的问题，只要你在做 Agentic RL 的训练或者数据相关的工作，大概率都会遇上这个问题。想要讲明白这个问题是啥之前，你需要先了解 Agent 基本原理和强化学习基本原理。

Agent 基本原理可以看一下我最近写的 [锦恢的 AI Agent 小白教程（三）提示词原理及其应用：ChatML、工具调用、提示词注入](https://kirigaya.cn/blog/article?seq=376) ，了解一下什么大模型的基本输出流程，什么是 ChatML。

强化学习基本原理可以看一下我多年前的 RL 教程：[PARL强化学习公开课学习笔记（一）强化学习初印象](https://zhuanlan.zhihu.com/p/149885358)，了解一下强化学习训练的“损失函数”，什么是 env，action 和 state。

---

## Agentic RL 的一次 Rollout

Agentic RL 和过往所有的大模型训练有一个很大的不同，那就是它的数据集结构。

Agentic RL 的训练是在模拟是 Agent 在某个场景下不断以某个特定 Harness 范式做任务的状态。这里以最简单的 ReAct 为例，为了训练一个大模型可以驱动 Agent 完成最基础的工具调用和多轮对话，我们的训练集只存储了任务一开始的上下文，可调用工具，初始提示词，label 则是一堆的 grader，这些 grader 有基于关键词的，有基于 LLM 的。

训练系统在拿到这个数据单例的时候，需要先基于这个上下文设置和初始的 prompt 去沙盒中模拟跑 N 轮，真实地基于现在的模型权重去和环境做交互，得到如下所示的 N 个 “轨迹”（trajectory）：

```python
messages_1 = [
    {
        "role": "user",
        "content": "请描述这块年糕。",
    },
    {
        "role": "assistant",
        "reasoning_content": "",
        "content": "大大的一块",
    },
]
```

轨迹 trajectory 描述了 Agent 是如何与环境发生交互的，获取/采样这样一条轨迹也被我们称为 rollout。在 Agentic RL 中，轨迹并非所有部分都是大模型生成的。

比如对于这样一个例子：

```plaintext
System Prompt: 你是一个私人助手 + {这是工具描述}
User Prompt：查询今天上海天气
Thought：思考并调用 weather 工具
Tool：QueryWeather('上海')
Observation：晴，最高 28℃
Assistant: 今天上海晴，最高 28℃
```

显然，对于这样一个 trajectory 来说，token 来源和对应的 RL 成分如下：

| token 来源 | RL 成分 |
| :--- | :--- |
| System Prompt | state |
| User Prompt | state |
| Thought | action |
| Tool | action |
| Observation | state |
| Assistant | action |

然后，知道了哪些 token 是 action，哪些 token 是 state 后，拥有强化学习经验的你就应该立刻知道，后续的 PPO 或者 GRPO 要怎么搞了，模型就能训起来了。通常情况，训练系统在捕获并记录上面的这些 token 时，一并会记录一个 mask_loss 变量，用于从整个 trajectory 的 token 中索引到 action 的那一部分，只有那部分对应的 token 才被视作大模型作为策略网络的输出，或者说，执行的 action。

---

## Retokenization Drift 问题

上面的过程看起来顺理成章可行，但是在实际训练中，我们发现当 trajectory 的轮数变长后，训练曲线出现了一些比较明显的问题，甚至有时候可能爆炸，社区中也有不少大佬讨论了这个问题，并给出了自己的解答，比如这篇文章：[Agentic RL: Token-In, Token-Out Done Right](https://huggingface.co/blog/huggingface/tito)。

首先，在 Agentic RL 早期对于上面这样一个 rollout，我们不妨简化一下它的流程，我们把做 ChatML 编码和分词统一都合并到 tokenizer 里面，那么一次 rollout 的过程就如下所示：

@[video](https://kirigaya.cn/api/storage/video/f708747a-9a93-4600-a07a-ae75d6340b79.mp4)

这里面其实有一个问题，那就是大模型在本轮 rollout 中生成的回答 “大大的一块”，它实际上是从 \[68249, 23582\] 解码过来的（我们这里采用了 deepseek v4 pro 官方的分词器做测试）。我们的 trajectory 是以文本形式存储的。

等到参与 PPO 训练时，我们还需要把大模型回答的那部分，也就是 “大大的一块” 这部分解码来参与 PPO 迭代，这个时候，你就会发现很神奇的一点：解码后的 “大大的一块” 变成了 \[19983, 110697\]，也就是说：

```
[68249, 23582]
        ↓ decode
    "大大的一块"
        ↓ encode
[19983, 110697]
```

> 大家可以玩玩我线上部署的 [Token 分词器](https://kirigaya.cn/ktools/tokenizer)

对于大模型来说，训练都是数字的运算，在 rollout 期间输出 \[68249, 23582\] 就是大模型作为策略网络在这一轮交互中的 “action”，但是等到 PPO 时，参与迭代的 action 却变为了 \[19983, 110697\]。虽然看起来的汉字都是 “大大的一块”，但是 Agentic RL 中，rollout 的采样 action 和 PPO 时的 action 其实已经不是同一串 token id 了，而在 PPO 中，我们计算策略梯度的公式为：

$$
L^{\mathrm{CLIP}}(\theta)
=
\mathbb{E}_t
\left[
\min\left(
\rho_t(\theta)\hat{A}_t,\,
\operatorname{clip}
\left(
\rho_t(\theta),
1-\epsilon,
1+\epsilon
\right)
\hat{A}_t
\right)
\right]
$$
其中概率比例 $\rho_t(\theta)$ 为：

$$
\rho_t(\theta)
=
\frac{
\pi_\theta(a_t\mid s_t)
}{
\pi_{\theta_{\mathrm{old}}}(a_t\mid s_t)
}
$$
也就是说，概率比例需要的 action 的输入必须是完全一样的。否则公式就有问题。训练曲线爆炸的问题就一点也不奇怪了。

为什么会出现这个问题呢？因为类似于 deepseek 或者 openai 自己的分词器，为了进行词语的合并优化等策略，往往在底层采用了 BPE 算法，BPE 编码通常按照学习到的合并规则或优先级反复合并符号。这使得实际上的分词编解码器已经不对 token 空间到自然语言空间构建严格意义上的映射了，而是多对一了。

这种同一串 token id 在解码后再次编码得到的 token id 发生变化的现象，被我们称为重分词漂移（retokenization drift）。为了解决这种问题，Agentic RL 框架在设计 Rollout 系统模块时都会采用 TITO 原则（token in token out）。

---

## TITO 原则

一句话概括 TITO 原则就是：Agentic RL 在训练时必须直接使用 rollout 阶段模型实际接收和生成的原始 token，而不能通过解码、重建消息和重新分词来还原训练轨迹。

从架构设计上，这就要求 Agentic RL 框架在进行 rollout 时，至少需要保留大模型采样时生成的 token，而不是只保存文本信息。设计上，我们一般会对整套系统采样时进行的 llm call 进行统一管理和请求的劫持（slime 中的 RolloutManager，但是 slime 还没有实现 TITO），确保所有的 token 数据会在系统中存留一份临时的 buffer。随着 rollout 过程中，每次 llm 要执行新一轮推理时并不是直接把 agent -> tokenizer 的 token id 输入，而是用“历史 token buffer + 新增 user/tool delta”增量获取 token id。然后 PPO 时也不再次对存储的文本形式的 rollout 的 trajectory 执行分词，而是直接采用 buffer 中存储的 token id。

这样，就能彻底避免 retokenization drift 的问题。

如何高效完成从训练系统到 sandbox 的架构设计？如何做好数据生命周期的管理？如何完成采样数据到训练执行的高效调度？如何这里面能做的优化空间就非常多了，具体的可以看一下我们的框架 [Dressage](https://github.com/Accio-Lab/Dressage) 的实现。

---

## 总结

从系统架构和算法实现的角度来看，TITO 除了能够避免重分词漂移，还可以带来以下好处：

| 问题 | 是否属于重分词漂移 | TITO 的处理方式 | 带来的好处 |
| :--- | :--- | :--- | :--- |
| assistant token、tool token 的边界以及 loss mask 丢失 | 否 | 在 token 被追加到轨迹时，同步记录其来源和对应的 loss mask | 不需要在训练前根据 chat template 重新推断哪些 token 由模型生成，哪些 token 来自工具或环境 |
| chat template 不满足前缀保持性质 | 否 | 检查新增消息重新渲染后的 token 序列是否以原序列为前缀；若不满足，则需要修复对应模板 | 防止 chat template 在加入工具结果后回头修改已经生成的历史 token |
| 历史压缩、删除 thinking 或仅保留子 Agent 总结 | 否 | 将最后一次历史改写后的上下文视为新的固定 prompt，并将其 loss mask 设为 0，只训练之后真实生成的 token | 避免在并非由当前策略真实采样得到的历史上计算策略梯度 |
| 生成过程因长度限制而中途截断 | 否 | 直接保留模型实际生成到截断位置的 token，并结束当前 rollout，不重新补全或重建响应 | 保证训练数据仍然对应模型真实采样出的 token 序列，避免因补全标签或重新渲染引入新的偏差 |

具体的，可以去看看上面 huggingface 的那篇文章，因为我们的架构设计已经避免了这些问题，在此就不赘述了，最后欢迎大家给我们的 Agentic RL 框架点个 star：<https://github.com/Accio-Lab/Dressage>