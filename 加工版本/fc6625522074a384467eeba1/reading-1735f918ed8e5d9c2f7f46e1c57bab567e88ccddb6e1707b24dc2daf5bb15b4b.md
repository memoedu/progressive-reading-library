<!-- reading-guide:start -->

## 阅读前置卡
### 一句话核心
这篇回答基于一篇Neuron观点文章，系统梳理了计算背景（计算机、统计等）进入脑科学领域的核心研究方向——Neural Data Science，重点分析了该领域的现状、人才分类、合作困境与职业前景。

### 核心洞见
1. 神经科学在数据和代码规范方面严重落后于结构生物学和生物信息学，这阻碍了大规模跨团队数据复用与AI突破（如AlphaFold）。
2. 神经科学数据科学家实际分为三类：面向科学问题的分析整合者、面向特定任务的算法开发者、面向基础设施的架构设计者；三类工作对能力和协作模式要求差异很大。
3. 数据科学家应早期介入实验采集过程，否则实验者可能被降级为“数据采集工”，而数据处理者则陷入“代码难读、依赖冲突、人员流动”的协作困境。
4. 学术界Data Science岗位存在严重的“活儿多钱少”困局，薪酬比工业界低约4倍，导致人才难以留存，只能靠兴趣和短期物质牺牲维持。
5. 作者以DataJoint工具为例，说明通过SQL与计算图标准化数据流，可以显著降低多人在神经科学数据分析中的沟通成本与重复劳动。

### 对我的价值判断
中。文章提供了Neural Data Science的框架性认知（人才分类、合作模式、薪资现实），对理解计算背景在脑科学中的角色有价值。但内容偏领域宏观现状，与个人知识系统的直接操作关联较低，更适用于补充背景知识而非直接指导工具选择或工作流设计。

### 信息密度判断
高。密度来自：对Neuron原文观点的凝练转述、三个数据科学家分类的清晰框架、DataJoint案例、薪资比例等真实经验数据。不是观点重复，而是有框架、案例和经验判断。

### 能解决我的什么痛点
1. 帮助明确“计算背景在脑科学领域具体做什么”的细分方向，避免空白的主观猜测。
2. 提供数据科学家角色分类模型，可用于反思个人知识系统中各角色（内容生产、工具开发、架构设计）的分工。
3. 揭示数据标准化和协作工具（DataJoint）的实际价值，启发在知识管理中引入规范化的元数据和计算图思维。
4. 对“学术界vs工业界选择”提供了真实薪资差和职业性价比判断，辅助个人在领域投入上的决策。

### 具体用处
1. **RemNote卡片**：沉淀数据科学家三类角色分类、DataJoint协作模型、神经科学数据规范滞后原因。
2. **渐进阅读高亮**：标记关于“数据科学家应早期介入实验”“编程作为神经科学必修技能”等观点性段落。
3. **AI工作流思考**：DataJoint的SQL+计算图设计可类比为“元数据驱动的流水线编排”，作为个人知识系统数据管理参考。
4. **课程/讲义设计**：若设计“计算神经科学入门”或“数据科学在脑科学中应用”主题，可直接引用其人才分类和薪酬案例。

### 是否急需阅读
近期。文章框架清晰、密度高，适合用于补充知识库中的领域认知，但并非当下搭建RemNote/Agent系统所需的操作指南或方法论，可安排在近期渐进阅读计划中，无需立即优先。

<!-- reading-guide:end -->

---

## 原文

# 脑科学+计算机（偏计算机） 有哪些具体的研究方向？

![](https://pic1.zhimg.com/v2-ea7097a3fd111ba530fae22a1d1b8ba9_r.jpg?source=2c26e567)

今天在Neuron上看到了一篇观点文章，介绍Data Science在脑科学研究领域的现状，并谈到了一些他们对领域发展的一些建议。对于一些**计算背景方向（计算机、统计、物理、生医工等）对脑科学感兴趣的人来说，这个方向是一个更快速进入脑科学的入口**。 其实我四年前自己成立实验室时候，就想写一下这个话题，但一直拖到现在都没动笔。正好就借着读这篇文章的机会，一起穿插谈一下自己成立实验室四年间也有一些体会。

[](https://link.zhihu.com/?target=https%3A//www.cell.com/neuron/fulltext/S0896-6273%2824%2900641-X)

[](https://link.zhihu.com/?target=https%3A//www.biorxiv.org/content/10.1101/2024.03.20.585936v1.full.pdf)

[](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/JtQBiVa82RsSoLiw-KF3bg)

Neuron官网的文章在发表时候和bioRxiv上的版本略有不同，我觉得bioRxiv的版本更好一些。

## 背景介绍

很多神经科学问题的回答需要从分子、细胞、环路、行为等不同层面系统研究，这样的工作涉及多种实验技术和细分领域专长，所以依赖团队合作（Team science）才能完成。自从美国脑计划[BRAIN initiative](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=BRAIN+initiative&zhida_source=entity) 2014年实施后，他们的经费资助机构[NIH](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=NIH&zhida_source=entity)在在原有支持小团队的R01基础上，增加了主打team neuroscience的[U19项目](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=U19%E9%A1%B9%E7%9B%AE&zhida_source=entity)。这样的项目通过组队的方式，面向一个主要的长期研究目标，利用学科交叉方式进行研究。相关介绍参考文章（[链接](https://link.zhihu.com/?target=https%3A//www.cell.com/neuron/fulltext/S0896-6273%2820%2930898-9)）

![](https://picx.zhimg.com/v2-85cd5887205a6a15a7c8361b527ae6d2_r.jpg?source=2c26e567)

![](https://pica.zhimg.com/v2-79baa3a72d3fc78d90be3270c740c106_r.jpg?source=2c26e567)

U19在神经科学的资助领域 （K.K. David, Neuron 2020）

![](https://picx.zhimg.com/v2-3a6e3e418b7748cfcbf500afc60007e7_r.jpg?source=2c26e567)

21个U19资助项目的分布机构 （M. Schottdorf et.al, Neuron 2024）

所有实验科学最终都会产生数据，所有的数据都需要分析，而这个**分析难度会随着数据量、数据复杂度、参与人员多显著增加**。那么以U19支持的team neuroscience是怎么解决数据整合、分享和分析方面的问题呢？这篇文章的作者们（Manuel Schottdorf, Guoqiang Yu 和[Edgar](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=Edgar&zhida_source=entity) Walker）设计了一套调查问卷，发给21个U19团队的负责人或者联络书，收到了15份反馈后做了一个总结性汇报。

## 文章内容

这篇文章有很大一部分的关注点都是与数据管理和分享相关的数据规范，这部分大家可以直接去看文章，我就不赘述了，简单描述几个事情吧：1， 大部分数据共享（80%）都是通过网盘/邮件/Slack直接传输，只有不到20%采用标准的数据管理系统分享； 2， 不到50%的分析数据代码被他人代码审查（code review）过。

其实这些统计结论（n=15）和大家平时观察的结论也都差不多，没啥特别新颖的。但重要的还是一些观点性内容。 下面我将挑选几个介绍。

### 1\. 神经科学在制定数据和代码规范方面是落后的

作者指出

> We found that, across U19s, there is a shared interest to improve sharing and standardization of data formats, analysis, processing code, data pipelines, and data infrastructures but with limited success.

在[结构生物学](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E7%BB%93%E6%9E%84%E7%94%9F%E7%89%A9%E5%AD%A6&zhida_source=entity)和生物信息学领域，相关数据和软件的共享非常成功。这些数据库的长期积累，使得[AlphaFold](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=AlphaFold&zhida_source=entity)一类人工智能工作可以出现。但是在神经科学领域，我们的进展节奏还是偏慢，尤其是系统神经科学领域。其实fMRI认知神经科学领域我觉得做的还是不错的。

简单甩锅的话，我们可以说系统神经科学的数据太多样性，不同的动物，不同的记录手段，不同的研究科学问题等等，每一个都让这个工作显得不好做。但这些其实都不是借口，有这么多的多样性，我们就按照能做的规范把他们存起来，加上不同的标签，他们之间一定是存在某些共性的。数据采集非常的宝贵，我们需要做到让每个数据都能发挥其自身最大的作用。目前领域的DANDI做的不错。

[](https://link.zhihu.com/?target=https%3A//dandiarchive.org/)

### 2\. 神经科学迫切需求更好的数据科学家

> Good data science can accelerate the progress of science in predictable—and sometimes unpredictable—ways. Our survey of the current state across large U19-funded neuroscience collaborations revealed substantial challenges to data science that concern us all. Only as a community can we address these challenges. Let’s get to it.

只要是采集了数据的领域都需要数据分析，当数据量大到一定程度，或者是问题复杂到一定程度。我们都需要更加专业的数据分析人才。对于数据分析，如果你没有意识到他的重要性，那多半是因为你的数据还没到需要复杂分析手段的地步，或者你的研究领域已经出现了很成熟的商业或开源软件。

对于一些前沿的研究，尤其是发展了很多新的神经信号采集技术手段的方向，数据分析往往是阻碍研究进展的关键瓶颈。 企业招聘网站上有大量的[data scientists](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=data+scientists&zhida_source=entity)，一般一个人需要负责整个数据分析流程中的一个环节，定义非常清晰。可是在脑科学里做data scientists，很多时候要一条龙全做（稍后我讲一下相关原因）。 按照工作内容来分，我认为包含三类：

1.  面向**科学问题**的数据分析，整合不同分析方法，通向最终的科学结论；
2.  面向**特定任务**的算法开发，解决特定一个数据分析任务的算法/软件开发；
3.  面向**基础设施**的架构设计，解决团队数据管理、协作、共享、算法部署一系列问题的。

  

其实第一类工作在我们传统的科研训练中已经包含了。平常我们阅读的论文涉及到的那些普通数据分析，他们都可以通过调用matlab/python/excel的内置函数，或者别人写好的软件包，写一些简单的脚本完成。这方面的人才**更加关注与科学问题**，所有的数据分析都是为科学结论服务，具有很强的问题解决能力。所以他们需要掌握很多统计方面内容（如[假设检验](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E5%81%87%E8%AE%BE%E6%A3%80%E9%AA%8C&zhida_source=entity)，聚类，[回归分析](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E5%9B%9E%E5%BD%92%E5%88%86%E6%9E%90&zhida_source=entity)等）。我记得十多年前，data science刚火热时候，企业需求很大，出现了很多data camp，招聘一些基础研究领域的PhD，培训一下coding和统计知识，然后就可以去企业里工作了。过去的数据量下，很简单的数据分析就可以搞定了。**但是脑神经网络是复杂的，**我们不能指望一些简单的统计分析就能帮助我们理解大脑。现在神经科学信号采集技术使得我们可以同时采集大量神经元的活动，但是如何理解这样的高维数据，需要更多的统计/计算机方法，比如现在各种神经活动[降维](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E9%99%8D%E7%BB%B4&zhida_source=entity)、表征提取、拓扑分析、[动力学系统建模](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E5%8A%A8%E5%8A%9B%E5%AD%A6%E7%B3%BB%E7%BB%9F%E5%BB%BA%E6%A8%A1&zhida_source=entity)等都频繁出现在最新的神经科学顶刊文章中。

  

第二类工作类似于计算机领域的各种打榜问题，对于一些特定数据分析任务，如细胞分割、信号降噪、三维神经元追踪等，开发对应的分析算法，提升单一任务的自动化水平和准确度。理想情况下，大家可以像玩ImageNet一样，下载了数据集就直接开干，弄完了有标准的评价指标。不过在神经科学领域，事情做起来要复杂一点。首先标准的数据集几乎没有，这个原因跟该论文提到的数据标准数据格式不统一也有关系；其次评价指标也五花八门，没有固定的指标；最后，所有的对比都发生在各自的文章发表中，而这些文章都为了自己的指标好看，挑选数据集、胡乱跑其它方法的实验结果等等问题都存在。目前的现状是，实验组的人从周围人口中听到一个方法，去试一下，获得了不错的主观感受，然后就用那个方法了。所用的方法不一定是最优的，但是最能“凑合一下”干活的。 **这个领域太多的工作都是注重“发表”，而非“被人使用”**。

  

第三类工作一般实验室是不会涉及到的，通常是那种真正采集了大数据（要到数百TB甚至PB那个级别）的团队就会涉及到这个问题。这里的基础设施不仅指计算硬件资源，还指的是完整的串联数据分析流程的整体软件框架系统。 它存在的根本原因在于数据的分析涉及到了太多步骤，一个人无法从头到尾全部掌握。而多个人参与之后，如何提升大家合作的效率？比如常见的问题有：

-   前一个步骤分析的结果存成什么格式，怎么读取？
-   另外一个人写的程序，我怎么在我的数据分析中调用？我需要读他们的代码，但是程序可真是太难读了，连注释都不写。
-   把前一个人写的代码部署到我自己电脑上时候，它的软件依赖和我的有冲突了咋办？
-   先前一个人写的程序离职了，剩下的人每人懂了，那这个人的工作几乎对团队没有任何遗留下来的东西。

这篇文章的作者之一Edgar在读博期间，和他们实验室的Dimitri Yatsenko一起开发过一个DataJoint的工具，目的就是解决团队协作过程中的数据规范问题。他们设计了数据分析流程中每一步结果的标准格式，用[SQL](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=SQL&zhida_source=entity)数据库存储，每一条中间结果就是数据库中的一个表。 然后这些表之间存在计算关系，表于表之间是通过特定的数据分析完成，他们可以将对应的分析代码直接写入到DataJoint。简单理解的话，DataJoint就是一个用SQL标准化的数据存储，用一个[计算图](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E8%AE%A1%E7%AE%97%E5%9B%BE&zhida_source=entity)把所有的数据分析任务都连接在了一起。当初我在某个大科学项目时候，与Edgar他们课题有合作，使用了一下DataJoint，好处就是我只要去数据中找到我需要的那几个数据，简单读一下数据的规范定义，只用很少的额外沟通成本就可以开展自己的工作。等我做完自己的那份数据分析后，我依然设计了自己的规范，上传到数据库，其他人也几乎不需要过多问我，就能继续他们的数据分析。 整个体验还是不错的。

![](https://pica.zhimg.com/v2-cac2b21680fb3adf295fd608a3396c1a_r.jpg?source=2c26e567)

DataJoint管理科研数据

### 3\. [数据科学家](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=2&q=%E6%95%B0%E6%8D%AE%E7%A7%91%E5%AD%A6%E5%AE%B6&zhida_source=entity)和实验神经科学家的合作精神

> data scientists should be included early during data collection to **foster a spirit of collaboration rather than perceived competition** between data collection and processing. They should also be present in lab meetings to become a normal part of any laboratory involved with collecting large neurophysiological data. Without this collaborative spirit, and clear roles, **experimentalists might feel devalued to a technical role limited to data collection,** and do not see the advantages of **outsourcing coding to an expert**.

科研成果最后会涉及到成果利益分配，但是当一个研究需要**非常好的实验数据采集能力和非常高的分析手段**时候，谁才是摘最大果子的那个人？其实答案也很简单，找到彼此愿意合作的人来一起做事儿，然后一起摘果子。如果你能力够强，全都自己做也可以。但是当科研进入到某个阶段，人的精力需要更多放在自己最擅长的地方，即使实验和计算你都能做，那也不见得是最有效的方式。

我个人是计算为主的，一直以来对实验的人都是很佩服的，他们对科学问题的理解和实验设计的精巧都是日积月累的宝贵财富。神经科学还是要围绕着真实的大脑数据开展，我认为数据是核心。目前为止，我与实验课题组的合作还是很愉快的，一个主要原因就是我们都彼此欣赏对方所掌握的自己不具备的能力。

![](https://picx.zhimg.com/v2-f80456605ef7305059d406b7d619eb09_r.jpg?source=2c26e567)

我实验室（DAMI）的研究方向以数据为中心逐渐展开

  

### 4\. 加强神经科学从业人员数据分析能力的训练

> Neuroscientists are seeking increasingly close collaborations with experts in computation, statistics, and theory to mine and understand their data. **These interactions and associated analysis methods, many of which did not exist 10 years ago**, force us to reconceive what it means to be an experimental neuroscientist today.

我当初申请读[计算神经科学](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E8%AE%A1%E7%AE%97%E7%A5%9E%E7%BB%8F%E7%A7%91%E5%AD%A6&zhida_source=entity)博士时候（2010年），就是因为本科在实验室里分析数据觉得自身要学的东西太多了，本科导师提到了未来神经科学对计算需求会越来越大。后来也的确如此，神经科学技术在最近十几年发展日新月异，我们的数据分析能力严重滞后。

这篇文章提到了一个有意思的观点

> We wish to stress that **any study program that allows students to graduate without mandatory and formal training to code is doing these students a grave disservice for their individual careers** and the progress of science. It should also be noted that these are among the skills that are **directly transferable to an industry setting**.

他们提议把学会编程作为所有的神经科学项目的必备技能，如果这些学生/博士后最后不做科研了，还可以帮助他们换到工业界去工作。我不知道是否所有学生喜欢这样，以及是否有必要去强制如此。但是我觉得从个人利益出发，我也推荐所有的学生学习一下编程，其实它没有那么难，学会了之后路会宽广很多。尤其是现在[chatgpt](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=chatgpt&zhida_source=entity)一类人工智能软件的出现，会让学习编程格外的简单。有问题直接去问AI就好了。

### 5\. Neural data science发展的困局

> Data science funding might be a critical bottleneck. We discovered that the required job profile of a typical U19 data scientist is highly diverse. This diversity will require experienced workers, necessarily at a higher pay scale than traditional postdocs. While we can possibly **retain workers with a pay-cut of ~50% relative to an industry position, the current split exceeds 300%**. We believe this is not sustainable. To this end we have proposed alternative means of addressing this in the Actions section, for example by recruiting young early-care

在神经科学领域里做data science的苦逼指出概括起来就是**“活儿多，钱少”**。U19这样很大的项目，采集的数据就是几百TB，但是他们也只有1-2个data scientists，这里边需要做的事情在公司里边可能是一个团队在做，不同的人负责了不同的流程。但在学术界就需要[全栈工程师](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E5%85%A8%E6%A0%88%E5%B7%A5%E7%A8%8B%E5%B8%88&zhida_source=entity)/科学家，啥都自己来。

而这样更高更杂的能力要求，并没有换来物质的回报，我个人感觉这个差距和他们文章说的差不多，大约是四倍的差距。我当年博后五万多美元时候，有个[脑机接口](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E8%84%91%E6%9C%BA%E6%8E%A5%E5%8F%A3&zhida_source=entity)的公司给我开20+万左右的工资，而当时互联网企业就更多了。如果考虑到这些有能力做neural data science的综合素质，在公司工作几年后，他们的待遇差距可能远不止四倍。

尽管如此，脑科学本身还是可以吸引到一些感兴趣的人。我见过好多个在工业界赚了不少钱之后，愿意牺牲掉一大部分待遇，做一些感兴趣的事情。我们实验室也很幸运的招聘到了这样一位具有十多年丰富业界经验，以及极强[主观能动性](https://zhida.zhihu.com/search?content_id=693090979&content_type=Answer&match_order=1&q=%E4%B8%BB%E8%A7%82%E8%83%BD%E5%8A%A8%E6%80%A7&zhida_source=entity)的工程师。

  

### 个人感想

在神经科学领域，数据分析的重要性只会越来越高，但是我们的培养还是没有跟上，尤其是和计算科学的交叉融合。Neural data science的发展会越来越好，**但是对个人职业生涯来说，我不觉得是一个性价比很高的事情**，所以我并不会贸然推荐每个人来从事这一行业。但是对于那些很喜欢计算和脑科学研究的人，并且短期对物质要求不高的人来说，是可以考虑的。 当年我的很多同学/同事后来也不做这行了，去了工业界每天过的很开心（羡慕ing），相关的学术能力训练让他们做很多事情都游刃有余。

其实关于这个领域我有挺多想说的，但写的时候又觉得很多东西都不好说。就先这样，以后有机会了慢慢完善吧。