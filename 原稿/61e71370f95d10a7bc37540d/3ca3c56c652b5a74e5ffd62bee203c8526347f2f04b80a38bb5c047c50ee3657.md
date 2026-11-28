## 前言

最近一直在忙着公司里的事情和我的infr系列的文章。Agent 这个系列的文章似乎很久没更新了，正好跟着之前的进度继续推进吧。

在之前章节中，我们把大模型是怎么演变到 agent，agent 的基本范式 ReAct，还有目前比较新的 Loop Engineering 都讲完了。不过之前我们讲的都比较理论，接下来几个章节，我们要讲点不那么理论，比较实用的东西了。

这一章节要讲的是 Vibe Coding 时的 Agent 基建，这一章节我很早之前就想写了，因为它真的很好用，并且落地成本还非常非常低。

无论是 [SKILL.md](http://SKILL.md) 还是 [AGENTS.md](http://AGENTS.md) 都是已经被提出了至少一年，并已经被几乎全部常用 Agent 支持的行业规范。

---

## 你能学到什么

通过本章学习，你将了解到在 Agent 项目工程中可以如何积累属于自己的数字资产，并理解工业界和社区过去几年总结出的关于记忆和外部知识引入的最佳实践——[SKILL.md](http://SKILL.md) 和 [AGENTS.md](http://AGENTS.md)。

你将学会如何使用 [SKILL.md](http://SKILL.md) 和 [AGENTS.md](http://AGENTS.md) 在你的智能体助手或者智能体项目中花费极低的心智构建 Agent 基建。

---

## 一个蓝图

设想一下，我们现在在搭建一个游戏私服管理助手，它负责帮我们管理我的世界服务器。搭建这个助手使用一个满足 [锦恢的 AI Agent 小白教程（四）智能体基本原理：Agent Loop，规划模式与子智能体](https://kirigaya.cn/blog/article?seq=382) 里面所提出的各项特性的智能体框架。

> 我会在后续教程出一期演示一下如何从零搭建这样一个智能体助手帮你干活

比如群组内的所有可信人员都可以命令助手来安装 mod，并根据要求把安装包扔到群里。

![图片](https://kirigaya.cn/api/storage/image/1338d591-fd7d-40eb-92e2-926f5cfcf5ed.webp)

然后这个助手还需要能把服务器文件自动备份到夸克网盘里。

![图片](https://kirigaya.cn/api/storage/image/b743fb8b-7867-445e-8656-de309364fe86.webp)

通过上面的例子，你应该能看出来，只是通过我们前几章讲的 agent 的基本算法和特性，并不能完全实现这样一个智能体助手。

第一个例子中，小奈说了「安装模组 jade」，我们的助手得知道：

1. 这是我的世界服务器。

2. 这里的模组指的就是 mod。

这代表我们的助手需要一个什么东西来存储长期记忆，这样就算服务器断电，第二次启动时，助手仍然需要知道自己是一个 MC 服务器管理员，而不是咖啡店的猫娘。这意味着我们需要某个“自由度”灵活地提供和定制长期记忆给我们的助手。本文讲解的 [AGENTS.md](http://AGENTS.md) 就是干的这件事。

第二例子中，看似稀松平常地进行一次简单的备份，但是你要知道，大模型它可不知道怎么通过命令行工具来操作夸克网盘完成文件的增删改查。而且夸克网盘原生是不支持命令行工具的，我们只能使用社区开发的第三方脚本来操作夸克网盘。显然，大模型是不知道怎么使用这种比较冷门的项目的 CLI 的，我们就需要某个“自由度”来给智能体提供一个“收藏夹”，来允许我们或者它自己把一些常用工具博客的代码记录进去。而这个“收藏夹”，其实就是我们的 [SKILL.md](http://SKILL.md)。

> 更正一下，最近夸克网盘已经原生支持了 CLI 和配套 SKILL：[夸克网盘Skill](https://b.quark.cn/apps/quarkcloud_skill_info/routes/V-sc7gOBT?uc_param_str=dsdnfrpfbivesscpgimibtbmnijblauputogpintnwktprchmtkp&uc_biz_str=S:custom%7COPT:SAREA@0%7COPT:IMMERSIVE@0%7Cqk_enable_gesture:false%7COPT:W_PAGE_REFRESH@0&webCompass=true&preview_pack_id=763584d0-8748-11f1-92ff-97186d1fa243)

好啦，相信诸位已经建立了一个大概的自上而下的认知，那么让我们开始吧。

---

## AI 的工具文档 —— [SKILL.md](http://SKILL.md)

> 已经知道 SKILL 及其细节的请自动跳过这一章节

[SKILL.md](http://SKILL.md) 相信绝大部分读者已经不陌生了，社区中关于 SKILL 的讨论从 2024 年年末开始就非常流行了，因为这属于大家在使用 Agent 帮自己干活时的最佳实践：使用简单，一个项目逐渐成长的 SKILL 也能成为自己的有效资产。

所以什么是 SKILL 呢？

SKILL 其实就是一个记录了一个工具使用方法或者一段特殊知识的 markdown 文件。它最初由 Claude 模型的母公司，MCP 的提出者 Anthropic 提出并维护，后来逐渐转为了跨 Agent 的行业规范，目前 [SKILL.md](http://SKILL.md) 的权威官方定义在 <https://agentskills.io/home>。

锦恢呢，也做了自己的 SKILL 商城，主要是自己在用：<https://kirigaya.cn/ktools/skillmanager>，也欢迎大家一起用用。

### [SKILL.md](http://SKILL.md) 的基本组织形式

一个标准的 SKILL 通过由下面一系列文件和文件夹组成的文件夹构成：

```
my-skill/
├── SKILL.md          # 必写: metadata + instructions
├── scripts/          # 可写可不写: executable code
├── references/       # 可写可不写: documentation
├── assets/           # 可写可不写: templates, resources
└── ...               # 其他文件
```

绝大部分你能在市场上能看到的 SKILL 其实就一个 [SKILL.md](http://SKILL.md)，所以在绝大部分语境下，[SKILL.md](http://SKILL.md) 和 SKILL 就是一个东西。而且根据奥卡姆剃刀原理，能流行的都是简单的，所以虽然规范上规定了一些文件夹什么的†是 `scripts`，`references`和`assets`啥的，但是如果你想要在 SKILL 里面设置自己的规范，也是没问题的，对于 harness 层级的代码，只有 [SKILL.md](http://SKILL.md) 这个名字是被写死的，别的文件夹名字都不是被广泛承认的规范。

[SKILL.md](http://SKILL.md) 文件也是有严格的编写规范的，一个合法的 [SKILL.md](http://SKILL.md) 应该长这样：

```markdown
---
name: jinhui-smart-air-conditioner
description: 描述怎么控制房间的空调
---

# 开空调
air-me 是控制我房间空调的命令，通过 air-me --help 可以看到使用说明
```

其中这个文件头部的一些键值对，也就是 name 和 description，通过这两个参数，外部的 Agent 就能知道有哪些 SKILL，这些 SKILL 的介绍又是什么。

### SKILL 发现与渐进式披露

那么对于一个智能体来说，它怎么找到当前系统中的 SKILL 并加以利用呢？

以 codex 为例，codex 搜索 SKILL 的路径一共有两条：

> 这也被称为 SKILL 发现（SKILL Discovery）

```
~/.codex/skills
.codex/skills
```

对于上面的搜索路径，codex 会自动搜集全部包含了 [SKILL.md](http://SKILL.md) 的文件夹作为初始化时搜索到的 SKILL，假设我们实际的文件夹为：

```
~/.codex/skills
├── kuake-wangpan
│   └── SKILL.md
├── qq-operation
│   └── SKILL.md
└── opencli-browser
    └── SKILL.md

.codex/skills
└── jinhui-smart-air-conditioner
    └── SKILL.md
```

那么 codex 就会在启动后检索出如下的四个 skill：

```
kuake-wangpan
qq-operation
opencli-browser
jinhui-smart-air-conditioner
```

并且我们根据之前关于标准 [SKILL.md](http://SKILL.md) 的组织逻辑，codex 还可以直接拿到每一个 skill 的 [SKILL.md](http://SKILL.md) 的 name 和 description 这两个信息。

然后在 codex 发起 llm call 的 system prompt 中，就会加入类似于下面的这样一段话：

```
下面是你能使用到的 skills:
- [kuake-wangpan](~/.codex/skills/kuake-wangpan): 夸克网盘的使用说明
- [qq-operation](~/.codex/skills/qq-operation): QQ 客户端的使用说明
- [opencli-browser](~/.codex/skills/opencli-browser): 操作控制浏览器的使用说明
- [jinhui-smart-air-conditioner](.codex/skills/jinhui-smart-air-conditioner): 锦恢家里的智能空调的控制手册
```

这样，后续的任务中，上下文里面，你的 Agent 就能知道当前系统中有哪些 skill，并通过 harness 设计中的一些诸如 read，glob 的内置工具（下一章会讲解通用 agent harness 的蓝图）找到这些 SKILL 的具体内容，并在 ReAct 中，把这些 SKILL 里面的全部或者部分关键信息加入对话上下文中，使得后续的 ReAct 中，Agent 可以学会使用这些自定义的工具。这样，Agent 的能力边界就可以被大幅度扩增了。

> 通过这个例子，大家也能看出来在 agent 运行时贸然增加或者删除 skill 都会导致前缀不匹配，从而导致 kv cache 丢失。所以在 agent 的生命周期中，增加或者减少 skill 一定是一个低频操作。

这种不是直接把全部 SKILL 的文本内容扔到 system prompt，而是采用文件链接+元信息配合 agent harness 实现 “按需拿 SKILL” 的 CE（上下文工程，Context Engineering 的简称）设计思路，也被我们称为「渐进式披露」。这种思路频繁出现在目前成熟的 Harness 设计和用户的使用习惯中，我们后面会在很多例子中潜移默化地提到。

「渐进式披露」的设计目的是为了让上下文有限的 Agent 能够有能力看到近乎无限的内容。SKILL 只是渐进式披露这种思想下的一个产物，除此之外，还有下面的一些方法也算是渐进式披露：

* 不把今天天气状况塞入 system prompt，而是给 Agent Harness 设计 Websearch 和 Webfetch，让它自己去当地天气网上找今天天气状况也算是一种上下文的渐进式披露。

* 不把全部需要分析的日志贴给 Agent，而是把日志文件的路径贴给 Agent，也算是一种渐进式披露，因为对于 agentic 能力极为优秀的顶级大模型，它们不会在 ReAct 中愚蠢地全量读入几十甚至上百 MB 的日志文件，而是会使用关键词搜索去搜它们要的精华信息，并将结果汇总给你。

* 不要把全部设计意图塞到代码的注释里面，而是把这部分设计意图写成完成的文档，然后把文档链接贴到注释里，并在 SKILL 里面配备用于阅读文档的 skill，这也是一种渐进式披露。

> 根据用户的问题明智挑选关键词，并在 Harness 中自由灵活地执行关键词搜索，正则表达式搜索来召回上下文的技术也被我们称为 Agentic Search。

顺带一提，在绝大部分 Agent 客户端软件中，比如 codex，claude code，qoder，智能体搜索到的这些 SKILL，你也可以通过 / 呼出命令来强制添加它的 [SKILL.md](http://SKILL.md) 的内容到上下文中（我们一般称之为 command）。比如，我想要我的智能体下面一个任务必须使用 opencli 打开浏览器，而不是使用内置浏览器（因为内置浏览器无法复用我的浏览器登录状态），那么就可以使用 /opencli 呼出所有符合名字的 SKILL

![图片](https://kirigaya.cn/api/storage/image/e39b032b-b5de-4a61-acca-e34f492fae70.webp)

然后选择我要的 SKILL，强制添加到下一次的问题中：

![图片](https://kirigaya.cn/api/storage/image/95362bb7-b8d0-4063-a4be-bb3e3d74b90a.webp)

这样，实际上，Agent 产品就会把这个 skill 对应的 [SKILL.md](http://SKILL.md) 内容替换到上面这个「Opencli Browser」上，然后发起大模型调用了。大模型就会优先使用这里描述的 skill 打开浏览器阅读指定文章内容，执行操作了。

这并非是 [SKILL.md](http://SKILL.md) 标准中的一环，但是确实绝大部分 Agent 产品都实现了这一点，并且这个功能真的很实用，所以分享给大家。

最最常用的 command 就是 skill-creator 了，几乎全部 agent 客户端产品都内置了一个 skill-creator，这也是一个 skill，是用来创建符合 [SKILL.md](http://SKILL.md) 规范的 skill 的 skill。一般在对话框打 /create 就能呼出：

![图片](https://kirigaya.cn/api/storage/image/d8de50d8-2565-4603-8cca-469e5b73def1.webp)

打上这个 skill 后，你后面就可以自由地输出你的需求来创建 skill 了。这在你使用 AI 踩坑或者跑完业务流程进行复盘和总结时会非常有用。当然，如果你还不知道有什么类型的 skill 可以创建提升工作效率，可以接着往下看。

### SKILL 的分类

建立认知的方法有两种，一种是自上而下建立分层认知，第二种就是分门别类啦。在上面讲 MC 服务器助手的时候，我们就已经做了前者了。那么现在，我们就要开始后者的认知建立了。锦恢将从自己的经历出发，讲讲常用的 SKILL 有哪些类别，这样不仅可以帮助大家更好地选择自己要给 Agent 维护的 skill，避免去做一些无用的 skill，也能帮助大家更好了解大模型的能力边界。

首先，大家要知道一点，skill 必须是对大模型本身能力或者只是的补充。如果你使用的这个大模型，它的能力已经 cover 了你想要写的这个 skill，你就不应该再去编写这个 skill 了。比如在两年前，那个时候的大模型还不会读取 pdf，所以有人写了 skill，告诉大模型调用一个现成的 Python 脚本来将输入的 pdf 转换为结构化的 json，这样大模型就能读取 pdf 了。但是如今的大模型的 PTC 范式下的 function calling 能力（下一章会讲）已经非常厉害了，gpt，kimi 或者 claude code 都能轻而易举当场编写一段 Python 脚本来完成 pdf 的读取，甚至 word/ppt/excel 或者一些更加冷门的格式的读取。

所以这种情况下，你就不应该去编写一个 pdf skill 教大模型怎么读取 pdf，因为通用大模型已经会了，这种随着大模型基础能力的进步而吞噬过去技术补丁的行为，也被我们称为“内化”。但是在一些特殊的行业，比如一些涉密单位，制造业，行政金融，他们必须本地部署模型，如果你看过 [AI Infra 软核教程（二）GPU 集群基本结构与多机组网拓扑](https://kirigaya.cn/blog/article?seq=387)，就应该知道现阶段最先进的大模型不可能在本地部署。所以本地部署的都是性能很弱的小模型，它们做不到当场编写一段 Python 脚本来完成 pdf 的读取，那么你就可以编写一个 pdf skill。是否需要编写某个 skill 需要具体情况具体分析。

而上面这种给大模型读取 pdf 代码的 skill 就是工具型 skill，也是绝大部分 skill 的类型。工具型 skill 的显著特点就是 [SKILL.md](http://SKILL.md) 里面带有代码和 bash 脚本。我们给 AI 配置的 CLI 往往都需要配套一个 [SKILL.md](http://SKILL.md)，这种往往都是工具型 skill，比较经典的工具型 skill 有：飞书 skill，夸克网盘 skill，智能家居协议 skill，浏览器控制 skill（比如 [opencli](https://kirigaya.cn/blog/article?seq=378)）。这些 skill 都是在告诉 agent 怎么使用某个工具。

工具类 skill 几乎就是我们最常用的 skill 了，在一个富有经验的 AI Builder 工作区内，工具类 skill 的数量能占到所有 skill 的 80% 以上。除了工具类 skill，skill 还分为如下几个类型，了解这些类型有助于阁下找到编写 SKILL 赋能工作的灵感：

#### 知识类 skill

很多时候大模型回答不了某个问题并非是因为能力不行，而是因为缺少了足够的上下文。

比如你让智能体帮你报名一个线下的技术交流会，但是你的智能体做不到，因为报名的表达上需要填写身份证号、文化衫尺寸和你忌口的饭菜，但是你的智能体并不知道这些你的个人信息。写一个锦恢 skill 就能让智能体知道这些基本信息从而完成报名。

再比如，你和你的朋友在一起玩 MC，突然你被苦力怕炸飞了，爆了一地装备，你和 MC 助手（一个 QQ 机器人，运行在我们的游戏群里面）说“请把我传送到小奈那边”，但是此时的 MC 助手是做不到的，因为它不知道“你”和“小奈”在 MC 服务器里面的用户 ID，此时如果有玩家 skill，并在有玩家加入后更新，Agent 就能把你传送过去了。

最后一个例子，假设你是一位芯片后端工程师，你想要用智能体帮你完成一部分工作，但是智能体并不知道工艺厂商的相关信息，如果有一个 经验老道的芯片工程师.skill，就能让智能体具备一部分公司内的信息，完成一部分工作。

这些都是知识类 skill 的一个应用场景。知识类 skill 的特点是，它包含了大量对解决问题至关重要，但是不存在于互联网上的数据或者信息，比如私密信息（你的用户画像，你的体检报告等等），商业机密信息（工艺数据，供应商信息，私有化协议等等）。一个知识类 skill 的典型可以看 [lark-markdown](https://kirigaya.cn/ktools/skillmanager/skill/1777304508345)，这个 skill 将发送飞书富文本格式卡片的编写语法作为额外的知识封装了起来，这样 Agent 就能知道了，我做的原因是因为让 Agent 自己网上搜这些信息搜不准。

不过这里需要特地说明一点，如果这些私密信息实在非常非常多，我们往往不会把它做成一个 skill，比如我们一般情况下不会做一个公司业务信息和项目架构.skill，而是会选择在公司的文档系统中组织一个架构良好，维护良好的工业级文档，然后把文档的访问方法以工具型 skill 或者 mcp 的形式装载给 Agent。

所以虽然知识类 skill 听起来非常直观，但是实际使用场景很有限。

#### 规则类 skill

有时候，智能体并不能如我们所愿地去执行一些任务，比如我们开发一个软件项目，即便我们在一开始千叮嘱万嘱咐，告诉了 Agent 不要修改测试文件，因为如果 Agent 可以自己随意修改测试文件，那么测试本身就失去了意义（为什么会这样留给大家思考）。我们当然可以每次都在对话开头和 AI 说一句：“没有我的允许，你不能修改测试文件”，但是：

1. 你不能保证你自己每次都记得这么做。

2. 你完全无法保证你的同事也记得这么做。

3. “没有我的允许，你不能修改测试文件” 这样的「规范」会随着业务和项目的增长越来越多。

所以我们可以编写一个基础的开发准则 skill，并在项目迭代的过程中对其进行长期维护。比如对于锦恢来说，一个通用化的工程任务，我往往会写一个类似这样的开发规则 skill：

```
我们这个项目的技术文档是：https://xxx/xxx
你可以通过 doc-cli 工具来访问这个文档，并进行操作。但是默认情况下文档是只读的，不要修改。

tests/ 是项目的测试工程的文件夹，默认情况下禁止对其进行任何修改。
```

这样虽然不能 100% 阻止 AI 自作主张修改测试，但是可以减轻我们 review 该死的同事的 pr 的压力。

#### 流程策略类 skill

如果你是某个领域富有经验的从业者，那么在你的工作环境中，一定存在一些“技巧”，“流程”，或者说的专业一点，一定有 SOP。

比如对于全栈工程师来说，从零搭建一个网站就是有一些相对固定的流程，也有一些不那么明确，需要基于个人审美和经验的流程的。仅仅针对这些相对固定的流程，比如选择什么前端框架和 UX 设计范式、什么后端框架和设计模式、测试工程和CI/CD怎么做，什么数据库和聚合访问方案、鉴权和支付的方案、静态存储和 CDN 的方案、怎么防止被盗刷流量等等，一般一个有多年工作经验的工程师一定有一套自己的类似“模板”的东西来复用这些相对固定的方案，这些都可以做成 skill，比如我分享一个我自己做网站的 skill：[dust-tarui-layout](https://kirigaya.cn/ktools/skillmanager/skill/1776065562944)，这个 skill 凝练了我使用 tarui 制作客户端软件的技巧，使用这个 skill 配合一定的工程经验，就能在喝咖啡的功夫下一键生成一个这样 UI 设计风格的软件：

![图片](https://kirigaya.cn/api/storage/image/d508d55d-adf7-4224-868f-ebfe7f7d6d7d.webp)

它的 UI 设计特征就是 xxxxxx，我知道你们不想听，你们想要一个类似的软件快速应付业务需求，所以这就是我们制作流程策略类 skill的意义。还等什么，快去总结你的工作经验，然后把这些东西沉淀成流程策略类 skill吧，这将成为你使用 Agent 时的重要资产，因为大模型再怎么学习，也学不到你的品味，因为这些品味是很难用有限的文字描述出来的，请记住，「语言的边界并非思想的边界」。

### 跨 Agent SKILL 发现：.agents/skills

在上文，我们谈到了 codex 执行 SKILL 发现的默认路径：

```
~/.codex/skills
.codex/skills
```

相信有点工程思维的朋友们这个时候已经开始思考了，那我如果换了别的 agent 呢？比如 kimi code，glm zcode，deepseek harness，claude，cursor 等等，我原本的 SKILL 的文件夹难道要全部重新摆放一下位置吗？毕竟从名字里也能看出来， codex 的默认 skill 搜索路径里面带有 “codex”，显然，其他厂商实现的 agent 不会这么“好心”地也支持从 `.codex` 路径上搜索 skill。

这几年业界有一个逐渐被大家公认但是还没写入协议中的 SKILL 发现路径：

```
~/.agents/skills
.agents/skills
```

很多常见的 agent 客户端和 harness 框架都会默认从上面路径完成 skill 发现，我总结了一些常见 agent 客户端对这一路径的支持情况：

| Agent / 产品 | `.agents/skills` |
| :--- | :---: |
| Codex | ✅ |
| Kimi Code | ✅ |
| GLM ZCode | ✅ |
| DeepSeek Harness | ✅ |
| Claude Code | ❌ |
| Cursor | ✅ |
| OpenCode | ✅ |
| Gemini CLI | ✅ |
| GitHub Copilot | ✅ |

了解这些信息有助于防止堆砌重复的 skills 文件夹。

---

## [AGENTS.md](http://AGENTS.md) 协议

如果说 [SKILL.md](http://SKILL.md) 提供了一个 Agent 收集工具和记忆的“收藏夹”，那么 [AGENTS.md](http://AGENTS.md) 协议就是提供了 Agent 行为定义的基础入口。

[AGENTS.md](http://AGENTS.md) 协议非常非常非常好理解，假设 Agent 的工作区如下：

```
src/
tests/
AGENTS.md
```

存在一个名为 `AGENTS.md` 的文本文件，那么 Agent 在新的对话启动的时候，就会把这个文件的内容读入，并全部附加到 system prompt 里面。利用这一特性你可以在不修改源代码或者 Agent 配置文件的情况下灵活自定义任何一个工作区的 Agent 的 system prompt。

[AGENTS.md](http://AGENTS.md) 协议是 OpenAI 在 2025 年 8 月份正式提出的，后来捐给了 **Linux Foundation 旗下的 Agentic AI Foundation（AAIF）**。目前 [AGENTS.md](http://AGENTS.md) 被几乎所有 Agent 支持。

> [AGNETS.md](http://AGNETS.md) 协议的官方维护网站域名非常好记，就叫做 [agents.md](https://agents.md/)。

### [CLAUDE.md](http://CLAUDE.md) -> [AGENTS.md](http://AGENTS.md)

当然，并不是全部的 Coding Agent 都支持 [AGENTS.md](http://AGENTS.md)，倔强的 Anthropic 说，SKILL 和 MCP 标准都是我提出的，凭什么我要兼容你的协议，所以 Claude 如今仍然不默认支持 [AGENTS.md](http://AGENTS.md)，而是支持他们的 [CLAUDE.md](http://CLAUDE.md)，那么这怎么办呢？

不是哥们，这可是我的电脑，我说什么就是什么。输入下面的 prompt 就能解决这个问题：

```
将 CLAUDE.md 合并到 AGENTS.md 中，并将 CLAUDE.md 改为指向 AGENTS.md 的软链接。
```

### [AGENTS.md](http://AGENTS.md) 极简实践

我几乎每一个项目使用 Agent 去工作都会先定义一个 [AGENTS.md](http://AGENTS.md)，用最短的文本把会影响到修改代码有关的业务信息、背景知识交代给 Agent。

结合 [AGENTS.md](http://AGENTS.md) 和 .agents/skills，加上你如果很了解现代 Agent 的 Harness，你就能非常专业地设计编排一个 Agent。这是 vibe coding 的最重要的技巧之一。[AGENTS.md](http://AGENTS.md) + [SKILL.md](http://SKILL.md) 也被称为 Agent 基建。

比如下面的结构是一个锦恢最常用，面向 vibe coding 场景的 Agent 基建：

```
project/
├── .agents/
│   └── skills/
│       ├── 文档查询 skill/
│       │   └── SKILL.md
│       ├── 代码托管平台 skill/
│       │   └── SKILL.md
│       └── 浏览器操作 skill/
│           └── SKILL.md
├── docs/
├── AGENTS.md
├── README.md
├── pyproject.toml
└── requirements.txt
```

[AGENTS.md](http://AGENTS.md) 里面则记录一些基本信息和准则。比如说文档链接在哪里，测试发布流程是什么样子的等等。docs 文件夹则是 [AGENTS.md](http://AGENTS.md) 的一个渐进式披露，我会把一些复杂含有大量文字彼此正交的信息放在 docs 文件夹里面，成为一个个 md，比如某个代码模块对应的数据契约和业务背景的简略介绍，我选择放在 docs/xxx.md 里面，而不是 [AGENTS.md](http://AGENTS.md) 里面，这样 Agent 基建就具备了无限了拓展性。算是我个人的一点点小技巧。

这就构成了一个基本的 vibe coding 场景的 Agent 基建，随着使用 vibe coding 推进项目， Agent 基建会被不断迭代，逐渐成为和文档一样重要的数字资产。

---

## 结语

这一章就不留作业了，因为这些内容实在过于简单，但是很有用。[AGENTS.md](http://AGENTS.md) 的流行也验证了我过去所强调的“奥卡姆剃刀”原则——只有足够简单的设计才能流行下去，做产品，做学术，搞工程规范和架构设计都是如此。

okey dokey，这里是 00 后全栈工程师锦恢，一个正在尝试撰写全网最小白最全面最系统的AI Agent系列教程的学徒。同时我的《**AI Infra 软核教程**》系列教程已经开始更新了：[https://kirigaya.cn/blog/article?seq=384](https://kirigaya.cn/blog/article?seq=384%E3%80%82%E6%AC%A2%E8%BF%8E%E6%83%B3%E8%A6%81%E4%BA%86%E8%A7%A3%E5%92%8C%E5%AD%A6%E4%B9%A0%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%BA%95%E5%B1%82%E6%8A%80%E6%9C%AF%E7%9A%84%E7%9B%B8%E5%85%B3%E6%9C%8B%E5%8F%8B%E6%9D%A5%E9%98%85%E8%AF%BB%E8%AE%A8%E8%AE%BA%E3%80%82)，欢迎想要了解和学习大模型底层技术的相关朋友来阅读讨论。

![图片](https://kirigaya.cn/api/storage/image/e40d7d07-4aa0-4744-9b6a-ea231632147c.jpg)