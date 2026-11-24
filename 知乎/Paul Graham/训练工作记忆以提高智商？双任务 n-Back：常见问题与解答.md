# 训练工作记忆以提高智商？双任务 n-Back：常见问题与解答

> [原文链接](https://zhuanlan.zhihu.com/p/1974434605189858200)

一份关于 DNB、WM、IQ 的资料汇编（更新至 2015 年）。

双任务 **n**-Back 是一种[认知训练](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Working_memory_training)，旨在增强你的[工作记忆](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Working_memory) (WM)，并有望提高你的智力 (IQ[1])。

最初的理论认为，新颖的[2]认知过程往往会相互重叠，且似乎都要经过[一个中央处理瓶颈](https://link.zhihu.com/?target=https%3A//www.ncbi.nlm.nih.gov/pmc/articles/PMC3158154/)。而研究发现，工作记忆 (WM) 不仅能预测智商 (IQ) [3]，还与之相关，两者甚至可能使用相同的神经网络[4]，这表明工作记忆或许**就是**智商[5]的本质。既然工作记忆是可以通过训练提升的，那么提升工作记忆就有望提高智商。而 N-back 正是一系列专门锻炼注意力和工作记忆的训练任务。

后来的研究发现，N-back 训练的表现及进步，与智商的相关性似乎比与传统的数字广度测试等工作记忆测量方法的相关性更高。这就引出了一个问题：N-back 的作用机制，究竟是扩大了工作记忆的容量，还是提升了自我控制能力、改善了对工作记忆内容的操作（而非容量本身），抑或是以某种方式直接训练了智商？[6] DNB 的表现与其它工作记忆或智商测试的表现之间存在着[复杂的关联](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2010-jaeggi-2.pdf)，因此我们尚不清楚它究竟锻炼了哪方面的能力。（此外，工作记忆与智商测试表现之间的关联也一直存在争议；例如，操作广度任务 (OSPAN) 测得的高工作记忆水平，与瑞文推理测验难题的得分并无良好的相关性[7]，并且单一的工作记忆训练测试的有效性也受到了质疑[8]。）

Brain Workshop 软件提供了多种训练模式，其中一些比简单的双任务 N-back 复杂得多。目前还没有针对这些模式的研究，因此我们对其益处、训练内容或可能带来的提升知之甚少。例如，[Jaeggi2010](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23jaeggi-2010) 的研究似乎发现单任务 N-back 的效果优于双任务 N-back。一些更复杂的模式似乎特别侧重于在不同感知类型（不仅是声音，还包括左/右方位等）之间切换正确的反应，这着重考验了[任务切换](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Task_switching_%28psychology%29)的能力。有研究结果表明，任务切换能力是可以通过训练得到提升并迁移到其他任务上的[9]，但这种提升的实用价值有多大，以及 Brain Workshop 的各种模式训练效果如何，目前仍是未知数。

## 1 论证

[工作记忆](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Working_memory%23Learning)对于学习乃至整体智力都至关重要。[10] 不难理解为什么工作记忆如此重要。说白了，工作记忆就是「你能在同一时间思考多少件事」。

想象一位遭受了脑损伤的不幸程序员，他的工作记忆一次只能容纳 1 个定义。他要怎么编程呢？要编写一段正确的程序，他需要同时知道 2 件事——比如，某个变量里存的是什么，以及某个函数的有效输入是什么。但遗憾的是，我们的程序员要么只能记住变量 foo 包含着输入字符串，要么只能记住函数 processInput 需要处理一个字符串，他无法同时记住这两件事！他会陷入永久的死锁，既不知道该拿 foo 怎么办，也不确定 processInput 究竟要处理什么。

更严肃地讲，工作记忆之所以如此有用，是因为它能让你在同一时间把握一个事物的更多结构。编程领域的评论者们[常常](https://link.zhihu.com/?target=https%3A//marginalrevolution.com/marginalrevolution/2011/06/does-this-reliably-increase-your-fluid-intelligence.html%23comment-157452631)写道，编程的一大挑战（除了接受并面对「计算机**真的**只是一台没有头脑、只会遵循规则的机器」这一现实之外），就在于它要求你将几十个细节和情境同时记在脑中——其中任何一个都可能让整个项目彻底完蛋。专注力是绝对必要的。而伟大程序员的特质之一，就是他们那种仿佛无所不知的能力。正是痴迷，赋予了他们这种**清楚自己究竟在做什么**的能力：

>  「对程序员来说，这尤其困难。生产力取决于能否在短期记忆中同时处理大量琐碎的细节。任何干扰都可能让这些细节瞬间崩塌。当你回来继续工作时，你会发现什么细节都想不起来了（比如你刚刚在用的局部变量名，或者实现那个搜索算法时进行到了哪一步），你不得不一遍遍地去查找这些信息，直到你好不容易找回状态，而这已经极大地拖慢了你的速度。」——[Joel Spolsky](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Joel_Spolsky)，[《这些人（非原创）的点子从何而来？》](https://link.zhihu.com/?target=https%3A//www.joelonsoftware.com/2000/04/19/where-do-these-people-get-their-unoriginal-ideas/)
>  「几位朋友都提到过黑客的专注力——正如其中一位所说，是那种『将自己头脑之外的一切都屏蔽掉』的能力。我当然也注意到了这一点。我还听好几位黑客说，哪怕只喝了半杯啤酒，他们就完全没法编程了。所以，或许编程真的需要某种特殊的专注能力。也许伟大的黑客能够将庞大的上下文加载到脑中，所以当他们看一行代码时，看到的不仅仅是那一行，而是它周围的整个程序。John McPhee 曾写道，篮球运动员 Bill Bradley 的成功，部分源于他超凡的周边视觉。『完美』视力的垂直周边视野约为 47°，而 Bill Bradley 有 70°；他看着地板时也能看见篮筐。或许伟大的黑客也有某种类似的天赋。（我则用一种非常精炼的语言来作弊，以此缩小『场地』。）这也许能解释在办公隔间问题上的分歧。或许那些负责办公设施的人，自己并没有什么需要高度集中的注意力，所以他们根本无法理解，对于一个黑客来说，在隔间里工作的感觉，就像把自己的大脑丢进了搅拌机。」——[Paul Graham](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Paul_Graham_%28programmer%29)，[《伟大的黑客》](https://link.zhihu.com/?target=https%3A//paulgraham.com/gh.html)
这或许令人惊讶，但程序 bug 的数量与代码行数之间存在着密切的关联——无论这门语言是像汇编一样底层，还是像 [Haskell](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Haskell) 一样高级（有个幽默的说法叫 [Norris’ number](https://link.zhihu.com/?target=https%3A//www.johndcook.com/blog/2011/11/22/norris-number/)）。这是否因为每一行代码都占用了相近容量的工作记忆和短期记忆，而人的记忆容量终究是有限的呢？[11]

### 1.1 银弹

虽然不那么显而易见，但计算机领域几乎每一项旨在提高生产力的创新，要么是为了减少程序员需要记住的信息量（例如[垃圾回收机制](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29)），要么是为了让他能更轻松地将信息存入或调出他的「短期记忆」。为什么像 [Jeff Atwood](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Jeff_Atwood) 这样的评论家会如此执着于[12]使用多个[显示器](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Computer_monitor)？话说回来，为什么**确实**有研究表明，仅仅增加第二台显示器就能带来惊人的生产力提升？[13] 这又不是说这个人本身发生了什么改变。而且，可以说，多显示器或大显示器还会带来分散注意力的负面影响[14]。

再比如，为什么 [Steve Yegge](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Steve_Yegge) 认为盲打是程序员**必须**掌握的少数几项技能之一（另一项是阅读）？[15] 为什么 [Unix](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Unix) 大师 [Ken Thompson](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Ken_Thompson) 唯一的遗憾是没学会打字？[16] 打字本身似乎并不那么重要——毕竟，重要的是内容，而不是你如何表达。编译器才不关心你的源代码是以每分钟 30 个单词还是 120 个单词的速度输入的。

>  我就喜欢这种不看键盘就能打字的感觉！能够以几乎和你思考一样快的速度打字，这**太令人振奋了**。你怎会满足于其它任何方式呢？
关键在于，无论是多显示器、盲打，还是速读[17]，它们都是为了让外部世界成为你心智的一部分。一个[类型签名](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Type_signature)是存在于你的短期记忆中，还是清晰地显示在你的第二块屏幕上，这之间有什么本质区别？一段[注释](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Comment_%28computer_programming%29)是在你脑中构思，还是在你构思的同时就通过盲打记录下来，这之间又有什么**本质**区别？

![](https://picx.zhimg.com/v2-e28a81c15a8a3e2cd92d97f610b73af9_1440w.jpg)
工作记忆的困境区别仅仅在于一点点速度，一点点时间。那个类型签名越是清晰可见，你记下那段注释的速度越快，你的「记忆」容量就越大。而你的记忆容量越大，你的智力/生产力就能越高。（你可以将此理解为[扩展心智理论](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Extended_mind_thesis%23)在编程领域的应用！）伟大的程序员常常[18] [19]会笼统地谈到要「将整个系统装在脑子里」或「建立一个心智模型」，并且他们极度厌恶分心[20]，认为分心会摧毁一个人[精心构建的思路](https://link.zhihu.com/?target=https%3A//paulgraham.com/head.html)。我认为，他们所说的，正是试图将所有相关细节都储存在自己的短期或工作记忆中。学习编程的能力也与工作记忆有关。[21] （一旦你开始留意，你会发现这种现象无处不在，比如在游戏中。[22]） Bug 率也是如此——有人提出，工作记忆的限制，正是导致程序的微小或庞大代码块相比中等代码块，出现错误比例更高的原因[23]。不过，那些[在设计时就考虑到记忆因素的编程工具](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2012-parnin.pdf)是否真的有帮助，还有待观察。

但是，尽管像[垃圾回收](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29)、盲打和多显示器这些工具非常出色（我本人就是它们的拥趸和用户），它们终究只是不完美的替代品。如果能够直接提升一个人的短期/工作记忆，那岂不是更好吗？那样或许更有效，而且无疑会更方便携带！

## 2 训练

然而，一个不幸的现实是，智商 (IQ)/[一般智力因素 (g)](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/G_factor_%28psychometrics%29) 和记忆力，似乎是无法通过训练提升的。许多看似显著的效果，其实都被锻炼、营养或简单的重复练习等因素所掩盖。而且，即便练习确实让参与者在某些任务或昂贵的游戏[24]中取得了进步，这些所谓的益处也往往无法**[迁移](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Transfer_of_learning)**到其他领域。许多流行的「脑力游戏」和训练项目都未能满足这一标准，或至少尚未被证明具有迁移效果[25] [26] [27]，即便是那些需要高超脑力技巧的活动，如[音乐](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2013-elpus.pdf)[28]、[国际象棋](https://link.zhihu.com/?target=https%3A//www.brunel.ac.uk/~hsstffg/preprints/chess_and_education.PDF)[29] 或记忆竞赛[30]也概莫能外。小说《[第二十二条军规](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Catch-22)》对此总结得恰如其分：

>  ……Dreedle 将军希望他的[飞行员]们，在设施和飞行计划允许的范围内，尽可能多地待在双向飞碟射击场上。每月进行八小时的[双向飞碟射击](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Skeet_shooting)训练对他们而言是绝佳的锻炼。它能训练他们……射击双向飞碟。
的确，纵观历史，所有试图提升儿童或成人智商的尝试，其结果与 [Arthur Jensen](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Arthur_Jensen) 在他 1969 年的论文[《我们能在多大程度上提升智商和学业成就？》](https://link.zhihu.com/?target=https%3A//gwern.net/doc/iq/1969-jensen.pdf)中所描述的并无二致——一部彻头彻尾的失败史。而少数的例外，恰恰印证了这条规律，因为它们要么只适用于有特定缺陷的少数人群，要么只在出生前起作用（例如[补碘](https://link.zhihu.com/?target=https%3A//gwern.net/iodine)）。（另见[阿尔吉侬定律](https://link.zhihu.com/?target=https%3A//gwern.net/drug-heuristic)：如果真有一种能简单提升适应性、让我们变聪明的办法，进化早就将其写入我们的基因了。）

但希望总是存在的，并且也存在**可能**的例外。本篇 FAQ 所关注的，正是双任务 [N-back](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/N-back) 训练，它是一种古老的工作记忆测试的变体。

N-back 的优点之一在于，无论它[是否](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23criticism)真的能提高你的智商，它都可能在其他方面对你有所帮助。工作记忆训练能帮助酗酒者减少饮酒量[31]，并增强正在戒除兴奋剂（可卡因和冰毒）的成瘾者的耐心[32]。学生的自律或意志力，与学业成绩的相关性甚至比智商更高[33]；工作记忆也[与学业成绩](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2005-aronen.pdf)及更少的行为问题相关[34]，并且它对 5 岁儿童 6 年后的学业成绩，以及对年龄较大儿童 2 年后成绩的预测能力更强[35]。研究表明，工作记忆训练对患有注意力缺陷多动障碍 (ADHD) 的儿童[36]和没有 ADHD 的学龄前儿童[37]都有帮助；[Lucas2008](https://link.zhihu.com/?target=https%3A//www.cdzjesenik.cz/APA_Poster_Lucas_May_2008.pdf) 的研究则发现夏令营参与者的行为有所改善。另一项干预研究让贫困的幼儿（7-9 岁）玩各种「推理」游戏，结果发现他们的正向数字广度（而非反向）和智商有所提升，而那些玩需要「快速视觉检测和快速运动反应」的游戏的受试者则没有获益[38]。但我们也要记住，儿童时期的智商分数并不可靠[39]，或者，正如一项青少年大脑成像研究所揭示的[40]，大脑在那个阶段可能只是更具可塑性。（针对青少年的工作记忆训练似乎研究不多，但考虑到他们面临的种种问题，这种训练或许有益；可参阅文章[《美丽的大脑》](https://link.zhihu.com/?target=https%3A//www.nationalgeographic.com/magazine/article/beautiful-brains)或[《青少年的烦恼》](https://link.zhihu.com/?target=https%3A//www.discovermagazine.com/2011/mar/24-the-brain-the-trouble-with-teens/article_view%3Fb_start%3Aint%3D1%26-C%3D)。）

工作记忆训练方法繁多。一篇值得一读的综述是 Morrison 与 Chein 在 2011 年发表的[《工作记忆训练有效吗？通过训练工作记忆以增强认知的希望与挑战》](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2010-morrison.pdf)；而 Shipstead 在 2012 年发表的[《工作记忆训练是否有效？》](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2012-shipstead.pdf)则探讨了设计工作记忆训练实验时会遇到的多种方法论上的困难（至少，如果你想证明训练带来了能迁移到非工作记忆技能上的**真正**提升，那么这些困难就尤为突出）。

### 2.1 N-Back

最初的 N-back 测试很简单，只要求你记住一连串字母，并在出现任何字母与（比如说）2 轮前的字母相同时发出信号。例如，序列「A S S R」不需要信号，但「**A** S **A** R」就需要，因为两个「A」之间正好相隔了 2 个位置。程序会给出下一个字母，你判断是否需要信号，如此循环。这个规则一旦理解就很简单，但解释起来稍有些费劲。最好的办法是阅读 [Brain Workshop 的教程](https://link.zhihu.com/?target=https%3A//brainworkshop.sourceforge.net/tutorial.html)，或者[观看演示视频](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2013-03-30-brainworkshop-brainworkshop481tutorialwindows.webm)。

### 2.2 双任务 N-Back

2003 年，Susan Jaeggi 和她的团队开始使用一种 N-back 的变体进行[功能性磁共振成像 (fMRI)](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Functional_magnetic_resonance_imaging) 研究。该变体试图增加每一轮的认知负荷——要求被试同时记住多项信息，而不仅仅是 1 项。研究摘要解释了其原因：

>  相对于单任务中前额叶皮层 (PFC) 的激活会随记忆负荷增加而增强，先前有假设认为，在双任务中，当处理需求过高（即超出工作记忆中央执行系统的容量）时，PFC 的活动会减少。然而，我们的结果显示，无论是在单任务还是双任务中，前额叶的激活水平都随着记忆负荷的增加而持续升高。即便在最困难的双任务条件下，处理需求已经超负荷（从行为准确率上可以看出），我们依然观察到前额叶激活的增加。因此，关于前额叶激活水平下降的假设未能得到证实，我们转而从动机因素的角度对此进行了讨论。[41]
这个版本被称为「**双任务** N-back」（以区别于经典的单任务 N-back），它依然是一个回合制游戏。在 [Brain Workshop](https://link.zhihu.com/?target=https%3A//brainworkshop.sourceforge.net/) 的版本中，你面前会呈现一个 3x3 的网格。每一轮，一个方块会出现在 9 个格子中的某一个，同时会播报一个字母。（这个游戏有无数变体：可以使用北约音标字母、钢琴音阶等等。Brain Workshop 也内置了多种模式，例如「算术 N-back」或「五任务 N-back」。）

### 2.2.2 1-Back

在 1-back 中，你的任务是判断当前轮次的**字母**是否与上一轮相同，以及**位置**是否与上一轮相同。两者可能都相同，也可能都不同，因此共有 4 种可能的反应（位置匹配、声音匹配、位置声音都匹配，或全不匹配）。

这给工作记忆带来了压力，因为你需要同时记住 4 件事：上一轮的位置和字母，以及当前轮的位置和字母（以便比较两者）。然后在下一轮，你需要立即忘掉已经无用的旧信息，并记住新的信息。你就在这个不断记忆、遗忘和比较的循环中。

### 2.2.3 2-Back

不过，1-back 相当容易。由于轮次间隔很短，你可以轻易地把字母信息暂存在[语音回路](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Baddeley%25E2%2580%2599s_model_of_working_memory%23Phonological_loop)中，从而减轻工作记忆的实际负荷。事实上，玩了大约 10 轮 1-back 后，我就完全掌握了——除非我一时忘了当前是 1-back 而非 2-back（或者完全走神），否则我几乎能做到 100% 正确。大多数人都会觉得 1-back 很容易上手，尽管一开始可能会因为持续不断的压力而感到有些挑战（一般的游戏和测试通常会有些喘息时间）。

而下一个级别——2-back——则是一个巨大的挑战。在 2-back 中，规则与 1-back 相同，但顾名思义，你需要匹配的是 **2** 轮之前的信息。所以，你不再是寻找「AA」这样的直接重复，而是要寻找「ABA」这样间隔开的重复。当然，你也不能忘得太快，因为你可能需要匹配像「ABABA」这样的序列。

2-back 对工作记忆的压力更大，因为现在你需要记住 6 个信息，而不是 4 个：2 轮前、1 轮前和当前轮次——每一轮都有 2 个显著特征（位置和字母）。6 个项目，也正好落在[正常工作记忆容量](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Working_memory%23Capacity)估算范围的中部：

>  工作记忆的容量通常被认为是有限的。最早对短期记忆容量进行量化的是 Miller (1956) 提出的[「神奇数字 7±2」](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/The_Magical_Number_Seven%2C_Plus_or_Minus_Two)。他注意到，无论记忆的元素是数字、字母还是单词，年轻人的记忆广度大约都是 7 个「组块」。后来的研究发现，记忆广度实际上取决于组块的类型（例如，数字约为 7 个，字母约为 6 个，单词约为 5 个），甚至还受同一类型组块特征的影响……由于还有其他一些因素会影响一个人的记忆广度，因此很难用一个确切的数字来界定短期或工作记忆的容量。尽管如此，Cowan (2001)提出，年轻人的工作记忆容量大约是 4 个组块（儿童和老人则更少）。
而且，即便需要记忆的项目不多，你需要做出的反应数量也会随着「模式」的数量呈指数级增长。因此，三任务 N-back 的可能反应种类不是比双任务 N-back 多 ⅓，而是多出了一倍以上：如果 **m** 代表模式数量，那么可能的反应种类就是 2^m - 1（减 1 是因为存在所有模式都不匹配的可能，但这种情况无需任何选择或思考）。所以，双任务 N-back (DNB)有 3 种反应[42]，三任务 N-back (TNB) 有 7 种[43]，四任务 N-back 有 15 种[44]，而五任务 N-back 则有 31 种[45]！

更糟糕的是，项目之间的时间间隔会造成极大的困扰。当出现重复时尤其麻烦——如果同一个方块和同一个字母连续出现两次，你可能反而会把它们都忘了！

所以，2-back 才是真正挑战的开始。我玩了大约 20 局之后才开始找到感觉。（一个有用的技巧是，先玩几局，每次只专注于一种刺激，比如只记字母；这能帮你适应 2-back 那种「回溯」的感觉。）

### 2.3 个人反思

到目前为止，我看到什么成效了吗？并没有。它就像冥想：我没有观察到任何具体的进步，但仅仅是探索专注力本身就很有趣——我发现我的专注能力比自己想象的要差得多！在 1-back 这种小儿科任务上只拿到 30% 的分数，并且费尽心力才能达到 D2B 的水平，这实在令人警醒。而更让人警醒的是，前一分钟还拿了 60% 的高分，几分钟后就可能跌到 20%。通过简单的练习来改变自己的大脑，这件事本身就很有吸引力——冥想也同样有趣，你会发现自己的心智如何抗拒「静坐」这件简单的工作，而且我听说在高阶阶段甚至会出现生动的幻觉。N-back 或许可以作为一种精神体操。很少有人纯粹因为喜欢而锻炼或拉伸，他们这样做通常是为了实现其他目标；有些人慢跑是享受跑步本身，但更多人是为了能更好地踢足球或活得更久。我还年轻，趁着生命还长，探索这类精神体操是件好事，这样才能收获最大的益处。

## 3 术语

N-back 训练有时被简称为「N-backing」，参与者则被称为「N-backers」。几乎所有人都使用免费、功能强大且跨平台的 [Brain Workshop](https://link.zhihu.com/?target=https%3A//brainworkshop.sourceforge.net/) 软件，简称「BW」（但你也可以在[软件部分](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23software)找到其他替代品）。

N-back 训练有多种变体，通常用一个以「B」结尾的三字母缩写来表示。例如，「D2B」和「D6B」都指双任务 N-back，但前者的回溯深度是 2 轮，而后者则必须回溯 6 轮。「D」代表「Dual」（双重），表示每一轮会呈现 2 种刺激（通常是方块的位置和播报的字母）。

但我们还可以增加更多刺激：播报的字母、方块的位置，以及方块的**颜色**。这就成了「三任务 N-back」，你可能会讨论自己在「T4B」（三任务 4-back）上的表现。

还可以更进一步：播报的字母、位置、颜色，以及**几何形状**。这就是「四任务 N-back」，你可能会讨论自己在「Q3B」（四任务 3-back）上的表现。（目前尚不清楚如何比较不同模式的难度，但从 D2B 升级到 T3B 似乎远比从 D2B 升级到 D3B 要困难得多。）

在四任务 N-back (QNB)之上，还有五任务 N-back (PNB)，这是 Brain Workshop 4.7 版本新增的功能 ([视频演示](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2009-argumzio-pentuple-nback.mp4))。第五种模式是通过第二个音频通道加入的——也就是说，声音变成了立体声。

其他常用缩写包括：「WM」代表「工作记忆」（working memory），「
![](https://www.zhihu.com/equation?tex=g_f)
」代表「[流体智力](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Fluid_and_crystallized_intelligence)」（fluid intelligence），而「*g*」则代表 IQ 测试所测量的[一般智力因素](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/G_factor_%28psychometrics%29)（general intelligence factor）。

## 4 作者手记

### 4. 1 N-Back 综述

对于时间宝贵的朋友：或许你读到这里就可以停下了。如果你想改善生活，并寻求最高性价比的方案，我奉劝你另寻他法。

例如，冥想更简单、见效更快，而且可以随时随地进行。打字训练能直接提升你操作电脑的熟练度，这在当今世界是一项宝贵技能。[间隔重复](https://link.zhihu.com/?target=https%3A//gwern.net/spaced-repetition)[1]能为学生带来无与伦比的学习优势。[促智药](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Nootropic)堪称便利的典范（吞下去就好！），其效果也更容易评估——你甚至可以对自己进行双盲实验，这在双任务 N-back 中是绝无可能的。像[褪黑素](https://link.zhihu.com/?target=https%3A//gwern.net/melatonin)[2]这样的补充剂所带来的益处，更是 DNB 无法比拟的——靠 DNB 训练让工作记忆多记住一个数字，其认知价值怎能与褪黑素带来的一夜安眠相提并论？对饮食和环境做些许调整，就能从根本上改善你的身心健康。即便是最基础的阅读训练，用最简陋的[速示器](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Tachistoscope)技巧，只要你的阅读速度低于 200 [WPM](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Words_per_minute) 且仍有默读习惯，也能收获丰厚的回报。而且，上述所有方法都能立竿见影。

相比之下，DNB 训练至少需要 15 个小时，才可能看到实质性的生理改善。这项训练本身的效果尚未得到证实——Jaeggi 的研究仅仅是启发性的，而非决定性的（并且也存在[相悖的研究结果](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23criticism)）。现有的 DNB 训练程序在探索广阔的设计空间时，基本依赖于猜测；关于哪些功能至关重要、何种呈现方式最佳，乃至训练时长与时机，都缺乏数据支持。训练过程本身毫无乐趣可言，它可能令人疲惫、备受打击、颜面尽失。它很可能成为压垮你日常安排的最后一根稻草，让你从那些本应对你具有最大[边际效用](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Marginal_utility)的事情上分心[46]。

那么，我为何还要坚持 DNB 呢？

因为我为之着迷。工作记忆竟能占据智商如此大的比重，这令我着迷；而它又能通过一项看似微不足道的练习得到提升，这令我着迷。大脑的活动、化学成分和结构会因此产生可测量的宏观变化[47]，这些效果并非纯粹的「精神作用」或安慰剂效应，这令我着迷。有时，位置和字母的序列在我的脑海中呈现出无与伦比的清晰，而有时，我又在混沌的思绪中迷茫摸索，甚至连上一个位置和字母都无法确定——尽管我明明可以离开电脑，正常地处理日常事务，这令我着迷。时间在 N-back 训练中时而拉伸时而压缩的体验[48]，也令我着迷。刚刚还能以 90% 甚至 100% 的准确率完成 **n-1** 级别的任务，仅仅将难度提升一级，任务就变得如噩梦般困难，这令我着迷。眼球扫视，另一项看似微不足道的练习，竟能稳定地将我的分数提升 10% 到 20%；我的大脑在短短几轮后就感觉精疲力竭，却又能在几分钟内恢复，这一切都令我着迷。同样，关于工作记忆的浩瀚文献——它是什么，有何用途，如何被操控等等——也令我深深着迷。

我并不认为 DNB 有多大的实用价值——但它有趣吗？非常有趣。

### 4.2 如何阅读本 FAQ

> **布莱恩**：「听着，你们都搞错了！你们**不需**要追随**我**，你们**不需**要追随**任何人**！你们得独立思考！你们**都是**特立独行的个体！」
>  **众人**：「是的！我们都是特立独行的个体！」[49]
本 FAQ 几乎完全由我个人完成。我已尽力做到公正，纳入了大部分相关研究，且不作遗漏。但错误或重要的疏忽在所难免。你必须带着批判的眼光来阅读。

你也必须带着批判的眼光来阅读，因为围绕[邮件列表](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training)形成的 N-back 社区，本身就是一个**社群**。这意味着它会受到社群所固有的一切偏见和问题的影响。一个围绕某项技术或实践形成的社群，我们有理由相信其成员都是从中获益的人；因此，任何出自该社群的材料（比如本 FAQ 或其中包含的个人经历分享）都会因[承诺升级或沉没成本偏见](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Escalation_of_commitment)等因素而自动变得可疑。试想，如果科学家只发表有新发现的论文，而从不报告那些未能[重复](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Reproducibility)前人实验的失败结果，会是怎样一番景象？一个发现 DNB 毫无用处甚至是骗局的 N-back 练习者，又有什么理由继续留在这个社群里呢？狂热的支持者们肯定不会欢迎他。（[Eliezer Yudkowsky](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Eliezer_Yudkowsky) 就此写过一篇精彩的文章，题为[《群体信念的蒸发冷却》](https://link.zhihu.com/?target=https%3A//www.lesswrong.com/posts/ZQG9cwKbct2LtmL3p/evaporative-cooling-of-group-beliefs)；所幸的是，与「蒸发冷却」的某些**其他**案例（如[「病态支持厌食症」](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Promotion_of_anorexia)或[「精神控制受害者」](https://link.zhihu.com/?target=https%3A//citeseerx.ist.psu.edu/viewdoc/download%3Fdoi%3D10.1.1.99.9838%26rep%3Drep1%26type%3Dpdf)）相比，双任务 **n**-back 可能造成的危害是有限的。）

最后，你必须带着批判的眼光来阅读，因为我们谈论的是心理学。心理学是公认最难获得可靠结果的科学领域之一，因为它的研究对象——人——大多是 **WEIRD**（来自西方 Western、受过良好教育 Educated、工业化 Industrialized、富裕 Rich 和民主化 Democratic 国家的英文首字母缩写）且千差万别的。正如我的一位教授所言：「如果你看到两篇心理学论文报告了相同的结果，那其中一篇肯定是错的。」 对任何一篇心理学研究都不能全盘轻信，这其中涉及诸多问题（为此我专门撰写了[一篇附录：「主流科学（及心理学）的缺陷」](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23flaws-in-mainstream-science-and-psychology)）。人们很容易陷入[「以偏概全」](https://link.zhihu.com/?target=https%3A//www.lesswrong.com/posts/baTWMegR42PAsH9qJ/generalizing-from-one-example)的诱惑，但你绝不能这样做。每个人都是不同的；你的正面（或负面）结果可能源于[安慰剂效应](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Placebo%23Effects)，可能得益于你近期改善的睡眠[50]，或者你睡的那个午觉[51]，可能是你正在进行的锻炼[52]，也可能是一种轻微[季节性抑郁](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Seasonal_affective_disorder)的好转（或发作），还可能是因为缺[钙](https://link.zhihu.com/?target=https%3A//www.lesswrong.com/posts/9ZodFr54FtpLThHZh/experiential-pica)、缺[锌](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Zinc_deficiency)[53] 或[缺碘](https://link.zhihu.com/?target=https%3A//gwern.net/nootropic/nootropics%23iodine)，或是[低血糖](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Hypoglycemia)[54] [55] [56]、[动机](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2010-moe.pdf)变化等等。

## 5 N-Back 训练

### 5.1 我应该每天进行多次训练，还是只进行一次？

[大多数用户](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/5e989058544ad122)似乎倾向于每天进行一次长时间的 N-back 训练，他们认为这能锻炼专注力。另一些人则选择早晚各一次，以便在每次训练中都能更集中精神。不过，有科学研究为「晚间训练更佳」的观点提供了一些支持；可参阅 [Kuriyama2008](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23sleep) 的研究，该研究发现睡前练习比睡醒后练习效果更好。

如果你将训练分散到两次以上，很可能会因为额外的准备和收尾工作而浪费时间，而且每次训练的时长可能不足以让你达到所需的强度。

### 5.2 策略

最简单，或许也是最常见的心理策略，是在脑海中维护一个列表，每一轮忘掉最后面的一个项目，同时记下最新的项目。但当 n 值升高后，这个方法就开始失效——如果你需要在脑中不断默念这个列表，那么默念本身就会耗费太长时间。

用户 Surcer 在他的帖子[《我的体系，让我们来分享策略》](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/zxGWSoftZYw)中，整理了一份针对不同 n 值的策略清单。

### 5.2.1 策略是好是坏？

人们[时常](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/fd08b93399939fb9)[询问](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/13ntwrki25U)[并](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/676676a8e34f56d3)[讨论](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/c4eaff3059759531)是否应该使用某种策略，以及如果使用，该用哪种。

相当一部分 N-back 练习者采用了一种「直觉」策略。他们并非有意识地复述刺激序列（例如，「f-上方，h-中间；f-上方，h-中间；g-下方，f-上方……」），而是全力集中精神，等待一种「应该按下『a』（声音匹配）」或「应该按下『l』（位置匹配）」的感觉出现。其中一些人，比如用户 SwedishChef，对此的[态度非常坚决](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/13ntwrki25U)：

>  真正的挑战在于帮助人们理解：双任务 n-back 的重点**不是**记住 n 个视觉和听觉刺激。它的重点是发展一种新的心智过程，一种能凭直觉认出某个刺激曾在 n 轮前出现过的过程。
>  一开始，大多数双任务 **n**-back 的学习者都想尽快记住 n 个项目，以便攻克 DNB 这座山头。他们会用自己已有的记忆技巧来辅助。他们可能会试图在脑海中维持图像，每出现一个新图像就回顾一遍；或者大声念出声音，每出现一个新声音也回顾一遍。但这**不是**我们想要的。我们希望大脑学习一种新的过程，能凭直觉判断出某个图像和声音是否在 3 轮或 4 轮前出现过。这有点像学习演奏一种全新的乐器。
>  我已经帮助网站上的一些学习者理解了这一点。重点不在于你能记住多少，而在于学习一种新的心智过程。理论上，这种新过程会转化为更强的工作记忆，从而帮助你更好、更快地建立联想。
而另一些 N-back 练习者则认为，直觉法行不通，或者至少效果不佳：

>  我不怎么信「直觉」法。当然，你或许能凭直觉记得几轮前听到过同一个字母，或在同一个位置见过方块，但我实在想不出，在不进行某种「主动」记忆的情况下，你怎么能「感觉」到那**恰好**是 6 轮或 7 轮前。——[Gaël DEEST](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/cWvW-MEL5S4)
>  我完全同意 Gaël 的看法，直觉法根本站不住脚……对我来说，很多时候直觉法完全不可靠。你可能这局还在玩 5-back，几局之后就在 3-back 上一败涂地……分数忽高忽低，极其不稳定。而且，从直觉的角度看，最好的方法是反复练习同一个 n 值，因为这样才能训练你的直觉……但这听起来就不太对劲。——MikeM（来自同一帖子）
很少有 N-back 练习者系统地追踪过直觉法与策略法的效果差异。用户 [DarkAlrx](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/Hf8JVcJ9Jes) 在他的[博客](https://link.zhihu.com/?target=https%3A//darkmindexperiment.blogspot.com/2009/04/neurogenesis-experimentdark-side.html)上报告了他的实验结果，虽然他本人认为结果是积极的，但其他人觉得这些结果并无定论，甚至像用户 Pheonexia 那样，认为实验结果对直觉法是不利的：

>  从你的图表和你整体成绩的下滑来看，我认为直觉法显然是行不通的。在你的第一张成绩图里，你在 44 天里用直觉法进行了 38 天的三任务 N-back 训练，平均 n 值提升了还不到 0.25。你之前的表现要好得多。而在你的神经发生实验中，你的平均 n 值实际上还下降了。
研究者 Jaeggi 本人在 2008 年左右的看法则[更为中立](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/CLxO4szQ34A)：

>  如果你的目标是训练双任务（也就是我们研究中使用的方式），我**不**建议你[将视觉和听觉任务分开训练]。原因是，将两种感知模式结合起来，和将它们分开训练，是完全不同的任务！如果你分开训练，我猜你会使用一些「复述策略」，比如在心里默念字母或位置。然而，在双任务版本中，这些策略可能更难应用（因为你必须同时处理两件事……），而这恰恰是我们想要达到的效果……我们不想训练策略，我们想训练的是（底层的）认知过程。这些过程，才有可能帮助你在其他未经训练的任务上表现得更好（这才是我们的最终目标）。所以，重点不是达到 7-back 或 8-back……重点是尽可能地将注意力完全集中在任务本身。
>  我可以向你保证，这是一个非常艰苦的训练方案……你一秒钟都不能分神（我相信你已经体会到了……）。但最终，你会看到自己的进步，也许你会发现自己能更好地专注于某些事情，或者更容易记住东西等等（希望如此）。
（然而，[Jaeggi2010](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23jaeggi-2010) 的研究显示**单任务** n-back 似乎也同样有效，这让上述建议的可信度打了折扣。如果单任务（视觉/位置）n-back 就能有效提升智商，那么或许只训练听觉或只训练视觉真的是个好主意。）

>  这个问题我经常被问到，但很遗憾，我不太确定能否帮上忙。我只能告诉你，我们对我们的被试说了（或者更确切地说，没说）什么，以及他们反馈了什么。首先，我们完全不告诉人们该用什么策略——这完全取决于他们自己。事实是，有些人的反馈和你描述的一样，他们说如果完全不使用策略，只是「让方块和字母自然流过」，效果最好。但当然，也有很多被试会使用更主动的策略，比如复述或将项目分组。但重申一次——我们让被试自己选择策略！[来源](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/lVUkyq8ukAE)
不过，使用何种策略可能并无差别。即使你使用了复杂的记忆术策略，你依然在锻炼你的工作记忆。策略本身可能并不起作用；引用 Jaeggi 2008 年的论文：

>  依此观点，我们的训练之所以能实现从工作记忆到流体智力 (
> ![](https://www.zhihu.com/equation?tex=g_f)
> )的迁移，原因之一可能在于，我们的训练程序提升了控制注意力的能力。之所以能产生这种能力，是因为每一个新刺激的出现都要求不断更新记忆表征，这必然需要注意力转换机制的参与。此外，我们的训练任务本身的设计，就不鼓励形成那种无需注意力控制便可自动执行的、针对任务本身的简单策略。
即便策略确实有效，也未必是件好事；引用自 [Jaeggi2010](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23jaeggi-2010)：

>  我们还提出，为了防止对特定技巧的习得，让被试只在最低限度上学习针对任务的策略，这一点很重要。我们认为，除了在矩阵推理任务上观察到的迁移效果外，近迁移任务上的进步也提供了额外的证据，表明被试训练的是任务底层的认知过程，而非依赖于特定材料的策略。
所以，即便某个技巧让你从 3-back 一下子跳到了 5-back，Brain Workshop 软件也只会不断提升难度，直到你再次感受到挑战。重要的不是你达到了哪个级别，而是你付出的努力。

### 5.3 那么，闪烁的对/错反馈是好是坏？

这取决于个人偏好，尽管支持禁用视觉反馈（SHOW_FEEDBACK = False）的人，其[呼声似乎更高](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/d1-6RujBY_E)或[人数更多](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/420f332fcd4317c9)。另一款软件 Brain Twister [据称](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/a8934cc1e04075f9)就不提供反馈。[Jaeggi 表示](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/lVUkyq8ukAE)：

>  游戏设计文献在这个问题上也没有定论——对此有不同的看法：一方面，每次尝试后的反馈能让你立刻知道自己做对了还是做错了；但另一方面，它也可能会分散你的注意力，因为你会不断地审视（和评价）自己的表现。我们当初决定，希望被试能将注意力完全、最大限度地集中在任务本身，因此选择了只在一轮训练结束后才提供反馈。不过，在我们为儿童设计的较新版本任务中，我们确实为每一次尝试都提供了某种形式的反馈（比如积分）。所以——我无法告诉你哪种方式是最佳的——我猜这其中也存在个体差异和个人偏好。
[Jonathan Toomim](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/7u-zymhfS3w) 写道：

>  我在做视觉心理物理学研究时，听实验室的同事说，这个问题已经有过实证研究（至少在视觉心理物理学领域），该领域的共识是：使用反馈会降低**即时表现**，但能提高**学习速率**。我没有亲自查阅文献来证实他们的观点，但这对我来说听起来很合理。我猜测这也同样适用于 Brain Workshop。在我看来，其原理是：反馈之所以降低即时表现，是因为当你答错并且意识到了错误，你的大脑会进入一种内省模式，去分析出错的原因并（有望）加以纠正，但在此模式下，你的大脑会从当前任务中分心，从而更有可能错过接下来的几次尝试。
### 5.4 如何提升 N-Back 表现？

更专注。多练。睡好觉，吃得健康。利用自然光[57]。采用分散式练习。压力越小，表现越好。

### 5.4.1 分散式练习

[Penner2012](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2012-penner.pdf) 的研究指出：

>  本研究比较了针对中年健康成年人的高强度工作记忆训练（每次 45 分钟，每周 4 次，持续 4 周）与分散式训练（每次 45 分钟，每周 2 次，持续 8 周）……结果显示，与高强度训练组和无训练的对照组相比，分散式训练在所有认知领域都带来了更显著的表现提升。交互对比揭示的最显著差异出现在言语和视觉工作记忆、言语短期记忆以及思维速度方面。
这让人联想到睡眠在其他形式的记忆和认知改变中所起的作用，以及 [Kuriyama2008](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23sleep) 的研究结果。

### 5.4.2 硬核训练

Curtis Warren[注意到](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/XnrH5kMrDXc)，当他执行了一项为期 4 天、每天练习超过 4 小时的计划后，他在四任务 N-back (Quad N-back)上甚至提升了整整一个级别[58]：

>  例如，过去一周我一直在尝试一个新的训练计划。我的目标是尽快提高智力。为此，在过去 4 天里，我总共完成了大约 360 局训练，每局试次间隔为 2 秒（总训练时长 ≈ 360 分钟）。周三我不得不休息，今天我也在休息（今天只打算做大约 40 局）。但我打算在周六做 100 局，周日再做 100 局，以此来结束这一周。如果能做到的话，甚至更多。
但他警告我们，这不仅需要巨大的时间投入，而且可能仅对他个人有效：

>  关键在于，虽然我可以毫无疑问地说这个时间表**对我**有效，但它可能**对你**无效。这些收益值得如此巨大的投入吗？你真的会注意到改善吗？这健康吗？这些因素完全取决于实际**执行**训练的个人。
Raman 开始了 DNB 训练，在他的最初 30 天里，他「每 5 天左右休息一次，每天做大约 20-30 局，在 n-back 水平上我取得了很好的进展（从 2 升到 7，中间一度触及 9）。」；他在[邮件列表](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/5e7ac7e6432b0d77/9f3b791ee0fb1c38)上记录了这段经历的日志，并且每天更新。

遗憾的是，Raman 和 Warren 在开始前都没有进行 IQ 或数字广度测试，所以他们只能报告 DNB 级别的提升和主观感受。

研究确实表明，在每天 10 或 15 分钟的训练强度下，[收益递减](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Diminishing_returns)效应并没有出现；例如，[Nutley2011](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23nutley-2011) 让 4 岁儿童分别进行 WM（工作记忆）练习、
![](https://www.zhihu.com/equation?tex=g_f)
 (NVR，非语言推理)练习，或两者兼有：

>  ……这些分析考虑到了各组在接受训练量上的差异：NVR 或 WM 组接受全剂量训练，而 CB（混合）组接受半剂量（表 3）。尽管这种模式并非在所有测试中都一致（见图 2），但这被解释为对预期的线性剂量效应的确认。我们的结果表明，在受训构念内向非受训任务的迁移量，大致与该构念上的训练量成正比。类似的发现，即迁移与训练量成正比，也由 [Jaeggi2008](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23jaeggi-2008) 报告过。这对未来认知训练范式的设计可能有启示，表明训练应该足够密集以产生显著的迁移，且训练超过一个构念本身并不带来任何优势。训练效果大概会达到一个渐近线，但这发生在何处有待未来的研究来确定。确保参与者在每个任务上花费足够的时间以看到临床上显著的迁移可能是重要的，而当增加被训练的任务数量时，这可能会变得困难。这可能是 [Owen 等人的研究 (2010)](https://link.zhihu.com/?target=https%3A//www.ncbi.nlm.nih.gov/pmc/articles/PMC2884087/)（在 10 分钟内训练六个任务）中缺乏迁移效果的解释之一。
### 5.5 遭遇瓶颈，或者说，如果我过不了 4-Back，我是不是在浪费时间？

有些人在开始 n-back 训练时劲头十足，级别迅速攀升，直到突然停滞不前，于是开始[恐慌](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_frm/thread/9303a95ba669742d)，怀疑自己是不是出了什么问题。完全不是！达到高水平固然是好事，如果能在短短几周内做到这一点，那就更令人印象深刻了，因为大多数成员需要比（比如说）2 周更长的时间才能在 D4B 上达到好成绩。事实上，如果你查看[小组调查](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_frm/thread/9303a95ba669742d)中的报告，大多数人都表示在几个月后停滞在 D4B 或 D5B。

N-back 的关键在于你在给你的工作记忆施加压力，仅此而已。实际的级别并不太重要，重要的是这是否是你勉强能应付的难度；在这方面，它有点像举重。引用 Jaeggi 2008 年的论文：

>  即使在剔除特定训练效果后，向 
> ![](https://www.zhihu.com/equation?tex=g_f)
>  的迁移仍然存在的发现似乎是反直觉的，特别是因为特定训练效果也与训练时间有关。这种能力的原因可能是，在训练期结束时 n 水平非常高的参与者可能已经发展了非常针对任务的策略，这显然提高了 n-back 表现，但可能阻碍迁移，因为这些策略仍然过于针对特定任务 (5, 20)。因此，最后一次训练中的平均 n-back 水平对于预测 
> ![](https://www.zhihu.com/equation?tex=g_f)
>  的增益并不关键；相反，似乎是在能力极限下工作促进了向 
> ![](https://www.zhihu.com/equation?tex=g_f)
>  的迁移。
邮件列表成员[报告了获益](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/eacf724158e04506)，即使他们停滞在 3 或 4-back；参见[益处部分](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq%23benefits)。

一个经常被报告的打破瓶颈的策略是：故意提升一个级别（或增加模态），并在那个额外困难的任务上努力练习，其思路是这将刺激大脑适应，从而使你能够胜任较低的级别。

### 5.6 休息会前功尽弃吗？

有些人想知道，如果一天/一周/一个月或其他长时间段不进行 n-back 训练，是否会让自己所有的辛勤工作付诸东流，从而导致 n-back 在长期内没有用处。

多个小组成员指出，他们在训练中曾有过长时间的中断，有时长达几个月甚至[一年](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/tree/browse_frm/thread/80613920c0fbd422/18f2a024cd8f87e8%3Frnum%3D31%26_done%3D%252Fgroup%252Fbrain-training%252Fbrowse_frm%252Fthread%252F80613920c0fbd422%252Fb960c61b9e515755%253Ftvc%253D1%2526%23doc_e87c3f795821ac64)，但这并没有显著改变他们的分数（休息后立即测试，分数可能会下降一个级别或准确率下降几个百分点，但很快就会回升到原有水平）。一些成员停止 n-back 训练 2 或 3 年，发现他们的分数只下降了[2-4](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/c/o-hkHA1f9iQ)个级别——远非跌回 1 或 2-back。（另一方面，[Pontus Granström](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_frm/thread/b4a99de43af6a33d) 休息了几个月后，长期从 D8B-D9B 跌落到 D6B-D7B；他推测这可能反映了缺乏动力。）[huhwhat/Nova](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_frm/thread/b4a99de43af6a33d) 从 D9B 下跌了 5 个级别，但恢复得很快：

>  在过去的几年里，我一直在断断续续地进行 n-back 训练，大多时候是停练状态。我大约 3 年前开始，能够达到 9-n back，但平均而言我会做大约 6 或 7 n back。然后我休息了几年。现在回来后，尽管我有相当一部分时间在聚会、打拳击、使用轻度毒品，甚至[多相睡眠](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Polyphasic_sleep)，在我最初的几次尝试中，我能够回到 5-6，一周后我又回到了 9 n back 的水平。
这一轶事证据得到了至少一封 [WM 训练信](https://link.zhihu.com/?target=https%3A//web.archive.org/web/20101104172646/http%3A//www.casl.umd.edu/node/1331)的支持，Chrabaszcz2010：

>  图 1b 说明了与无接触对照组相比，训练在多大程度上迁移到了一个表面上不同的（且未经训练的）言语工作记忆测量任务上。训练不仅显着增加了言语工作记忆，而且这些增益在停止训练后持续了 3 个月！
同样，[Dahlin2008](https://link.zhihu.com/?target=https%3A//gwern.net/doc/dual-n-back/2008-dahlin.pdf) 发现了 WM 训练增益，这些增益在一年多以后仍然持久：

>  作者调查了年轻和年长受试者在基于计算机的工作记忆信息更新训练 5 周后的即时训练增益、迁移效应和 18 个月的维持情况。受过训练的年轻人和老年人在标准任务（字母记忆）上的进步明显多于对照组，并且这些增益在 18 个月后得以维持。迁移效应通常是有限的，仅限于年轻参与者，他们表现出向需要更新的未经训练的任务（3-back）的迁移……
### 5.7 我听说 12-Back 是可能的

一些用户报告说能够一直达到 12-back；Ashirgo[经常玩](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/373089e3e049707b/5dbde0450a4d44ca)D13B，但其他模式下的最高水平似乎是 T9B 和 Q6B。

Ashirgo [分享了](https://link.zhihu.com/?target=https%3A//groups.google.com/g/brain-training/browse_thread/thread/72aa2de2fdf75edf/6c777b2646227c38%3Fq)她的 8 点方案，关于如何完成这样的壮举：

不惜一切代价保持专注。流体智力本身有时被称为「专注力的强度」。
你最好不要复述上一个位置/声音。这最终会降低你的表现！我是指「一步一步」的复述：它会让你慢下来并分心。唯一允许的复述应该是几乎无意识和「毫不费力」的（你很快就会意识到它的意思 :)
第 1 点和第 2 点因此意味着你必须尽可能强烈地专注于最新的刺激。尽管如此，你不能忘记之前的刺激。怎么做？你应该在脑海中保持它们的图像（图像、图片、图画，无论你喜欢什么）。注意，你那样做仍然没有复述任何东西。
考虑将数据流 (n)分割成更小的部分。例如，6-back 将是两个 3-back。
随着方块改变位置，用眼睛跟随它。
只需打开带有所有选项的 Jaeggi 模式，以确保你的任务最接近原始版本。
考虑做超过 20 局。我今天正打算做不少于 30 局。这也可能有帮助。
你可以通过将降级和晋级标准分别从 >75% 和 =## 6 相关研究

略

## 7 软件

略

## 8 我还能做些什么？

略

## 9 另请参阅

略

## 10 附录

略 

## 11 脚注

略

> [Thoughts Memo](https://link.zhihu.com/?target=http%3A//paratranz.cn/projects/3131) 汉化组译制
> 感谢主要译者 gemini-2.5-pro，校对 Jarrett Ye
> 原文：[Dual n-Back FAQ · Gwern.net](https://link.zhihu.com/?target=https%3A//gwern.net/dnb-faq)
> 创建于：2009-03-25 | 最后修改：2019-12-05
