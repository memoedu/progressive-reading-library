上一期文章：<https://kirigaya.cn/blog/article?seq=382>

> Vibe Coding 的杠杆点正在从“写好单次 Prompt”，转向“设计一个可靠的 Agent 工作循环”。

## 前言

在前面的章节中，我们介绍了大模型是如何一步步变成我们能够用到的 Agent，这里面不仅仅涉及到大模型本身的一些基本能力，基于这些能力的层层封装（比如 ChatML 和 function calling），再到 ReAct（Agent Loop） 和 Harness（planning，subagent 等等）。

### 更加自动化

还记得吗？我们最初想要将大模型做成 Agent 的一个基本出发点是什么？就是觉得每次都要不断将上下文复制粘贴给大模型太麻烦，我们希望大模型可以更加自动化地观测和执行任务对吧？所以业界通过一年的探索尝试，给出了目前的 Agent 相关的最佳实践，它大幅度解放了我们的生产力，让我们可以把更多时间拿来刷手机摸鱼了（bushi

而大家写的 prompt 质量其实很大程度会影响 Agent 的执行质量，执行长度，大部分时候 Agent 给的 plan 或者建议我们从来不看，都是一路 approve，这启发我们，当前的 Agent 工作流是不是还能更加自动化？

### 你使用 Agent 的工作仍然存在重复劳动

Agent 一路以来的设计哲学一直都是在不断消灭重复劳动，而很多朋友在拿 Agent 干活的时候其实不经意间也存在一定的重复劳动。比如当我们让 Agent 帮我们写一个关于植树节的网站，要求可以登录注册，并且设计稿中的 7 个路由都能正常访问并将访客数据落盘。于是我们编织了一段还算不赖的 PRD 扔给了 Agent，并开启了规划模式，于是就开始喝起了咖啡。

过了一段时间，你发现你的 Agent 告诉你，你的网站已经做完了，于是你打开 localhost:8080 查看效果，你发现页面确实都写完了，但是注册登录怎么有 bug 呀？然后指定的几个路由也只是实现了一个大概，如果说你设想中的满分是 100 分的话，那么除了主页面外的这几个页面只能打 50 分。

于是你开始重新编织 prompt：

```
你的这个登录页面有如下的问题：
- xxx
- xxx

然后你的 /user/profile 页面的基本信息都没写呀，你给我实现一下。

我的 PRD 里面说了，/blogs 里面要写上我们需要的博客，并将元信息入库，我发现你的分页还没做。

...
```

然后等待 AI 再写完一轮，你再检查，再给反馈。

大家不妨问自己一个问题：这个过程中，我真的在动脑筋吗？这里面真的没有重复劳动吗？

我想，对于一位经验尚浅的初级工程师来说，页面元素如何排列？博客博文存储结构如何设计？这些或许并不是很清楚，所以他大概率不会根据产品的 PRD 往里面加入这些工程层面的细节来让 AI 做得更好。但是对于高级工程师来说，他做过太多项目了，所以一眼就能把这些附加信息添加到 PRD 里面，对于他来说，再给 AI 进行检查和提醒就是重复劳动。

所以我们在思考一个问题：我们能不能让 Agent 自己检查自己写的网站的各个环节对不对并自动修改？我们只对其中几个需求模糊或者歧义的地方进行“事后处理”就好。锦恢在半年前经常玩的一件事就是，写一个 SKILL，告诉 AI 在开发网站，小程序，App 时你要如何自己检查自己的开发结果，并结合 PRD 进行自动修改。你已经是一个成熟的 AI 了，要学会自己 check 自己的作品了，当时写的这个 SKILL 在我自己的 SKILL 商城里面：[jinhui-stack-debug](https://kirigaya.cn/ktools/skillmanager/skill/1775036608142)。

有了这个 SKILL，我的 Agent 帮我实现一个小需求的时候，它就不会「接收需求」->「修改」->「总结本次修改」，而是变成了「接收需求」->「修改」->「通过 SKILL 和工具检查修改结果」->「修改」-> … -> 「总结本次修改」。 更加自动化了，不是吗？

![图片](https://kirigaya.cn/api/storage/image/d58b7cf9-720f-43df-a954-d73c974d2edc.webp)

其实这就已经是一种更加自动化的 Agent 了，或者用我们这期教程的话来说，这就是「循环工程」 Loop Engineering 了。而在 2026 年，几乎大部分常见的 Coding Agent 都已经内化了这种能力。我们下文再详细讲。

---

## Loop Engineering 的设计原理

在上文中，我们提到了「希望 Agent 的执行过程更加自动化」这件事。其实，如果系统地分析这个问题，就会发现：我们其实是在引入额外的判断条件，来延长 Agent Loop 本身的长度，让 Agent 更难退出 Agent Loop。

还记得前几章介绍过的 ReAct 基本结构吗？如果使用 Python 伪代码来表示，那么 ReAct 所做的事情，大致可以抽象成下面这个过程：

```python
while not loop_finished:
	# 调用大模型
    response = llm_call(...)
	# 根据结果判断是否退出 agent loop
	# 具体过程详见：https://kirigaya.cn/blog/article?seq=382
	loop_finished = len(response.tool_calls) == 0
```

我们上面做的事情——让 Agent 通过 SKILL 和工具检查修改结果，从伪代码的层面来看，就可以表示为：

```python
while not loop_finished and not goal_achieved:
    response = llm_call(...)
	loop_finished = len(response.tool_calls) == 0
	# 判断目标是否完成了
	goal_achieved = judge_goal_achieved(...)
```

我们通过添加一个目标校验逻辑 `judge_goal_achieved`，引入了额外的变量。`judge_goal_achieved` 可以是一段静态程序，比如检查我的支付宝余额是不是超过了一个亿，或者也可以是一个 LaaJ（LLM as a Judge，在后续关于 Agent 评测的章节，我们会提到）。

显然 `goal_achieved` 并不总是 true，所以这个 while 循环相较于前面那个会更加持久一些。也就是说，Agent Loop 被我们延长了。这，就是一种 Loop Engineering。

当然，Loop Engineering 并不只是这一个范式，下面的伪代码描述了另一个形态的 Loop Engineering：

```python
while not goal_achieved:
	while not loop_finished:
    	response = llm_call(...)
		loop_finished = len(response.tool_calls) === 0
	# 判断目标是否完成了
	goal_achieved = judge_goal_achieved(...)
```

所有的 Loop Engineering 都能通过上面的 python 伪代码进行表达，且大部分 Loop Engineering 总是通过总结日常生活中大家和 Agent 交互中产生的不必要的重复劳动，将这种劳动的重复抽象为一种可自动化完成的判决逻辑 `judge_goal_achieved`，从而来延长 loop 的执行轮数，试图让整个 Agent 的工作流程更加自动化。

Agent 领域没有银弹，Loop Engineering 也不是银弹，到处乱用不仅会大幅度增加 token 消耗量，还会增加你的用户和同事的精神损失费。

### 什么时候该用？

需求明确没有歧义、对于任务中涉及的各个基础设施，AI 都有工具可以程序化无阻碍地进行访问和操作、任务有非常明确的终止条件。

比如你要实现一个驱动程序的对接，并且这个驱动程序的对接脚本就在一本 6000 页的工程手册里面，任务终止的条件就是老师傅留下的冒烟测试全部通过。

### 什么时候不该用？

需求模糊、整个环节有很多东西无法让 AI 通过程序访问、任务终止条件模糊或不可通过程序实现的。

比如大部分老板的需求就不适合直接通过设置 Loop Engineering 完成，当然，这也给了你给老板画饼的空间。

不要试图用 loop engineering 来掩盖当前理解的无效性、模糊性和不确定性。如果你本身就对一个系统架构或者业务流程不熟悉，你想要将理解本身完全交付给 loop engineering 只会逐渐放大这种不确定性，为当前项目不断积累新的 comprehension debt（理解债，相对于 tech debt，技术债）。

---

## Loop Engineering 的最佳实践 —— /goal

当然，虽然说你看了锦恢的教程，可能会兴致勃勃地去下载 [jinhui-stack-debug](https://kirigaya.cn/ktools/skillmanager/skill/1775036608142)，就会让我感到非常的高兴。不过我并不喜欢和你们玩信息差。所以我不得不告诉你们，其实这个skill很早就不用了。因为从2026年开始，各个主流的 coding agent （Claude Code，Codex，Kimi Code 等等）都已经实现了原生的 loop engineering 工具，也就是 goal，目标模式。

在这些 Coding Agent 内，打 /goal 即可呼出目标模式

![图片](https://kirigaya.cn/api/storage/image/4cd6f002-9935-4172-9bcb-6a1ab37cf3ce.webp)

关于 goal，我强烈推荐大家读一读 OpenAI 的这篇博客：[Using Goals in Codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)，从 goal 更底层的架构设计，再到正确的使用姿势，看这篇文章，基本上你就能全明白了。

而我在此处可能就需要稍微拾人牙慧一点，借鉴一部分 OpenAI 的博客里面的认知（反正我知道你们大部分人根本不想去看这些纯英文的文章）。

### goal 的基本原理

goal 的设计其实就符合我们上面讲到的如下的 loop engineering 的设计范式。

```python
while not goal_achieved:
	while not loop_finished:
    	response = llm_call(...)
		loop_finished = len(response.tool_calls) === 0
	# 判断目标是否完成了
	goal_achieved = judge_goal_achieved(...)
```

拿流程图表示其实就是 ReAct 外面再套一层循环：

![图片](https://kirigaya.cn/api/storage/image/a804cb94-3d25-4b07-b14f-e7969f87e9e1.webp)

这就是 goal 的一个基本实现，有技术难点的地方就在于「判断目标是否完成」的这个 `judge_goal_achieved` 函数的实现，这需要从在 goal 模式下输入的 prompt 中自动构造出用来判断目标是否完成的函数。是的，判断目标是否完成，本身也是一个 agent，而原本的 ReAct 循环能看成是这个 goal agent 的一系列 subagent。执行的维度就从一维延展到了二维，所以你能看到 goal 每次的执行时间都非常长，短则半小时，长则 5 个小时以上。当然，自然就是非常消耗 token 了。

### goal 的使用技巧

goal 作为命令，提供一组基础的参数：

```
/goal      	   查看当前的 goal / 激活 goal 模式
/goal pause    暂停一个激活的 goal
/goal resume   恢复被暂停的 goal（暂停的可能原因：手段中断、网络问题等）
/goal clear    移除当前的 goal 
```

而正如我们上面所说的，正常的 goal 在使用时，会根据你输入的提示词，由 Agent 自动构造出上面的双重循环。内循环用于真正执行任务，可能包含多个负责不同工作的 subagent；外循环则根据内循环产生的结果和证据，判断当前目标是否已经完成，从而决定结束任务还是进入下一轮。因此，为了尽可能精准地让 AI 完成这个 goal，用户输入的提示词也应该包含构造上述循环所需要的各种必要信息。

其中最重要的问题就是：AI 要如何判断目标是否已经完成？这自然需要你在提示词中提供相关的验证信息。这类验证信息在 codex 的博客中被称为 evidence source，也就是“证据源”，即判断当前任务是否达标的依据。

除此之外，还需要告诉 AI 当前的约束条件是什么。毕竟，完成目标并不是不计代价的。完成某一个目标的前提，通常是现有的某些东西不会被破坏。比如，当你实现一个新的前端功能组件时，我们肯定希望已有的功能不会因此受到影响。而这种“已有功能不能被破坏”的要求，就是 constraint，也就是约束条件。

所以，一个合格的、能够驱动上述循环的提示词，应该是下面这个样子：

```
/goal <希望达成的状态>
通过 <证据源> 来完成验证
同时要保持 <约束>.
```

比如，我最近刚准备把我个人网站上的一些静态资源，比如博客、图片和视频迁移到腾讯云上，那么我就可以这么告诉 codex 来一键完成这个目标：

```
/goal 我最近刚准备把我个人网站上的一些静态资源，比如博客、图片和视频迁移到腾讯云，你可以通过 xxx 获取 bucket 的读写权限。
你应该通过上传和访问 bucket 内的数据来验证功能完成。
同时运行全部和 /api/xxx 路由相关的单元测试来保证迭代的健壮性。
```

这样，几乎就能以极高的成功率帮你完成一些这样子的简单任务了，如果放在之前，可能我们需要一步步告诉 Agent 怎么做，然后各个环节还需要自己运行一些很机械的测试文件来检查，现在对于其中一部分可以自动完成的板块，通过构造上面的 prompt 结合 goal 就能全自动完成目标。

这就是 loop engineering 的一个很不错的工业实践。

---

## 课后作业

### 两种 Loop Engineering

在上文中，我们提到了两种范式不一样的 loop engineering 的伪代码。

范式一：

```python
while not loop_finished and not goal_achieved:
    response = llm_call(...)
	loop_finished = len(response.tool_calls) === 0
	# 判断目标是否完成了
	goal_achieved = judge_goal_achieved(...)
```

范式二：

```python
while not goal_achieved:
	while not loop_finished:
    	response = llm_call(...)
		loop_finished = len(response.tool_calls) === 0
	# 判断目标是否完成了
	goal_achieved = judge_goal_achieved(...)
```

从伪代码的角度来看，它们的逻辑运行结果似乎是完全一样的。但真的是这样吗？请回答这两种范式的不同。

### goal 的设计

请查阅相关资料，判断一下内层循环和外层循环是否应该共享上下文和资源，要共享哪些，哪些又不能共享。

---

## **总结**

这一章就比较轻松了。实际上 loop engineering 是一个才提出不久的概念，相关的各种探索也是正在进行中。大家也可以考虑一下，在自己的工作和生活中，能不能用类似的方法来更加优雅地对我们的 agent 相关的工作流程进行优化。我是一个对 AGI 实现持悲观态度的人，但我仍然认为 AI/Agent/大模型 能够改变很多东西。当然了，你可以将我的这种理念也定义成一种 AGI。但对我来说，这并不重要。

okey dokey，这里是 00 后全栈工程师锦恢，一个正在尝试撰写全网最小白最全面最系统的AI Agent系列教程的学徒。同时我的《**AI Infra 软核教程**》系列教程已经开始更新了：[https://kirigaya.cn/blog/article?seq=384](https://kirigaya.cn/blog/article?seq=384%E3%80%82%E6%AC%A2%E8%BF%8E%E6%83%B3%E8%A6%81%E4%BA%86%E8%A7%A3%E5%92%8C%E5%AD%A6%E4%B9%A0%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%BA%95%E5%B1%82%E6%8A%80%E6%9C%AF%E7%9A%84%E7%9B%B8%E5%85%B3%E6%9C%8B%E5%8F%8B%E6%9D%A5%E9%98%85%E8%AF%BB%E8%AE%A8%E8%AE%BA%E3%80%82)，欢迎想要了解和学习大模型底层技术的相关朋友来阅读讨论。

![图片](https://kirigaya.cn/api/storage/image/e40d7d07-4aa0-4744-9b6a-ea231632147c.jpg)