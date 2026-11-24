简单总结一下，这几个月开发 Agent 总结的一些方法论和思路。给后来者铺点路，权当抛砖引玉。

## Agent 三大核心组件

通过多篇学术论文和我自己的工作体验，我将目前的 AI Agent 系统的核心总结为三大件：

- planning
- memory
- action

### planning

planning 指的是对于 agent 对于一个给定的任务进行规划，将这个任务拆分为若干个子任务的过程。

> 为啥要继续拆分，而不是直接解决？当然是因为现在的大模型做不到了，如果现在的大模型智商比我还高了，那么这篇博客就应该是它来写，而不是我来写了。planning 本质上为了实现 agent 针对于目前大模型智能瓶颈的一次妥协。顺带提一嘴，目前训练大模型的大部分数据集本质都是 QA pair，也就是一问一答，这种数据集注定了训练出来的大模型只适合进行短程规划任务。

planning 是 Agent 执行任务的第一个入口，它类似于人类的大脑。planning 也是目前 Agent 三大模块中最难的，社区完成度最低的一个模块。很大的原因源于 planning 数据集的缺失。大部分 planning 的工作目前仍然只能从 prompt 着手去进行妥协。

我相信，想要解决这个问题，还是得从 planning 数据集和特化训练的大模型入手，等待后来人了。

### memory

memory 指的是 agent 对于之前的任务和环境的记忆。这个很好理解。

> 此处存疑一个东西：记忆和知识的区别。此处指谈论记忆，而不谈论知识

memory 不是大模型基于上下文进行“海底捞针”的能力，因为如果换一个新的对话，那么大模型就会丧失之前所有的“记忆”。此处的 memory 指的是一个独立于大模型的模块，比如 openmemory 就是一个 memory 模块，它可以永久性存储用户和大模型对话过程中产生的细碎“记忆”。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-facaf1a9c3b5177bad0bdf8316fd5a42_1440w.png" style="width: 100%;"/>
</div>

比如你今天和大模型说了你中午吃过了鸡鸣汤包，那么隔了一个月之后，你问 agent 推荐外卖的时候，agent 就应该要知道你吃过鸡鸣汤包。

记忆模块将成为未来 agent 必不可缺的模块之一。

此处值得额外一提的是，有些学者把记忆模块进行进行了拆分，拆分为了 profile 和 memory，此处的 profile 指的是某些“系统性的”，“相对固定”的数值，存储这些数据的数据结构相对固定，且在 agent 工作的工程中，这些数据不断更细。比如你的身份证，你的名字，你的住所这些固定的信息，可以被定义为 profile。

在我的设想中，我认为目前暂时将 profile 和 memory 合并在论述 Agent 必要组件这件事上，无伤大雅。倘若阁下认为拆分出 profile 更加合理，也可以如此更新您的认知模块。

目前 memory 的基本实现方案还是依靠 sentence2embedding + 向量数据库 技术来实现，没错，也就是 rag 那一套。

目前的不足就是，切分的太粗的记忆容易发生检索模糊，切分的太细的记忆容易缺失关系。

比如对于下面切分的5条记忆：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7ea8733a87d739c460b897809abd80d6_1440w.png" style="width: 100%;"/>
</div>

它们其实源于同一篇技术文章，但是如果我询问 docker 相关的文章的链接的时候，openmemory 会告诉我不知道，它只能匹配到「发布了文章《docker 的镜像源添加与 docker build 时的网络问题》」这一条，但是显然，这几条记忆之间应该是存在「关系」的，从而组织成半结构化的信息的。没有半结构化信息，从而无法自发地进行启发式关联是目前 memory 的一个重大缺陷。

### action

action 指的是 agent 对于一个给定的任务进行执行的过程。它紧接 planning 模块的输出，planning 负责将一个大的任务拆分为各个子任务，而 action 模块负责去具体执行这些子任务。action 类似于人类的小脑。

我这里举一个例子来讲解一下：

我们有一个管家 agent，那么我现在告诉它：“请帮我点一下外卖”，那么管家agent首先会先进行 planning，planning 模块的输出如下：

```python
plans = [
  "打开美团app",
  "打开外卖模块",
  "选择用户最喜欢的外卖",
  "选择套餐配置",
  "获取支付权限",
  "下单"
]
```

你可以看到，这是一系列完成点外卖的子任务，然后针对每一个子任务，action 模块需要具体给出如何去执行它们。显然，在计算机中执行一个任务的直接执行单元是一个程序，或者一个函数。action 需要负责的就是将一个子任务映射到若干个函数的组合。

比如对于 “打开美团app” 这个子任务，action 的输出是如下的函数的执行队列：

```python
step_0 = [
  ["navigate", { "url": "https://外卖.com" }],
  ["login", { "user": "锦恢" } ]
]
```

也就是先导航到 https://外卖.com ，然后使用锦恢为ID进行登陆。

> 你告诉我为什么域名可以是中文？你应该去温故一下 RFC 3490 标准和 Punycode 了，然后祈祷一下，锦恢不是你的计算机网络技术的考官。否则，阁下差一点就喜提重修了。

对于后面的每一个子任务，action 都会如此将子任务映射到函数合集。所以你发现了吗？其实现在的 action 模块我们已经实现了。

MCP 服务器 + MCP 客户端 其实就已经是一个 action 模块了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b8deca8f69afc9a6facdd038e5aca09f_1440w.png" style="width: 100%;"/>
</div>

这个系统中的 planning 只不过是手动挡的，我们需要人工来规划和切分任务。当然，目前也是有一些 MCP 可以来姑且充当一个 planning 模块，比如 [claude task master](https://github.com/eyaltoledano/claude-task-master). 但是，还不够有趣，不够泛用。这里面是一片蓝海，欢迎阁下尽情探索。

值得一提的是，在具身智能领域，planning 结合 VLA 也是目前的重要探索方向，被认为是实现具身智能最有可能的一条技术路线。AI Agent 和机器人技术，在这个方向殊途同归了朋友们。

action 模块目前主要的问题已经不再是函数调用了，函数调用问题我认为基本解决了，我上线的几个 AI Agent 应用的函数调用成功率在为期三个月的测试中保持在 99% 以上，加上我设置的 retry 机制，调用成功率为 100%。目前 action 模块的主要缺陷在于 agent 会调用模型时使用压根不存在的参数和数据结构，产生它的根本原因在于大模型的幻觉，我在这篇文章中详细讲过具体的原因和解决办法：[从大模型的幻觉现象剖析开发 AI Agent 的技术瓶颈和解决思路｜到底谁的幻觉更大？](https://kirigaya.cn/blog/article?seq=327)

---

## 目前的缺点

### 1. 大模型的幻觉

大模型幻觉太严重了

大部分问题的根本原因，请见[从大模型的幻觉现象剖析开发 AI Agent 的技术瓶颈和解决思路｜到底谁的幻觉更大？](https://kirigaya.cn/blog/article?seq=327)

### 2. 缺少评估标准

目前市面上的 MCP 和 Agent 存在大量占坑现象，产品质量良莠不齐。

急需一个公认的评价标准来给出某些曾经的度量。来区分什么是好的 Agent，什么是不好的。

### 3. 缺少能力边界认知

有些工作就不适合大模型来做，也更不适合 AI Agent 来做。设计软件之前一定要想清楚这个问题：我的问题真的非得用大模型来解决吗？

### 4. 缺少 planning 数据集

字面意思。

---

## 社区推荐

### 魔塔社区

魔塔社区：https://www.modelscope.cn/

国内版本的 huggingface，阿里巴巴出品，mcp 推出后也拓展业务到 mcp hub 和 mcp 在线部署。你可以在上面寻找 mcp 相关的资源。

### Smithery

Smithery：https://smithery.ai/

目前应该算是我见过的最完善的 MCP serverless 解决方案。你可以利用这个平台将自己写的 mcp（推荐用 TS 写）快速部署为一个 http 服务。这为 mcp 快速在移动端进行使用提供了可能。

如果你在一个 mcp 项目里见到 `smithery.yaml`，说明这个项目是基于 smithery 做的部署。

---

## 工具推荐

- 开发 mcp 的利器：OpenMCP，https://kirigaya.cn/openmcp
- 写代码多用 AI Coding： trae/cursor/claude code
- 测试 mcp 的平台：cherry studio，claude desktop



---

## 参考

- https://arxiv.org/pdf/2309.07864
- https://arxiv.org/pdf/2308.11432