# 迈向 LLM 多样性与创造力的基准测试

> [原文链接](https://zhuanlan.zhihu.com/p/1975560125474418990)

本文讨论了衡量 [LLM](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Large_language_model) 在头脑风暴或编辑等软性「创造性」任务中能力的可能任务，旨在量化其在创意写作领域的不足。

2024 年时代的 [LLM](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Large_language_model) 最薄弱的环节之一，也是用户体验与基准测试结果差异最大的地方，就是任何涉及「多样性」和「创造力」的内容。几乎没有任何基准测试能真正有意义地测试这些词汇所代表的内涵。

这并不奇怪，因为研发部门并不优先考虑这一点，用户经常抱怨他们最喜欢 Claude-3，但即使在看似「创造性」的任务中，它也不总是排在基准测试的榜首。[模式坍塌](https://link.zhihu.com/?target=https%3A//gwern.net/doc/reinforcement-learning/preference-learning/mode-collapse/index)并没有被现有的基准测试所衡量或惩罚，这些测试孤立地看待数据点，忽略了偏好中的个体差异，转而倾向于最大化最广泛受众的喜好（即最小公分母）。

奇怪的是，目前的「偏好学习」研究并不涉及对偏好的学习。很少有偏好学习数据集或研究论文试图对偏好进行建模，它们只是试图预测大众的流行度。⁠[⁠1⁠]（那些确实考虑过这一点的论文，往往将其视为噪声而不予理会；例如，[DPO 的作者在简单的摘要任务中测量到](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.18290%23page%3D10)人类的分歧率高达 35%，并推断这证明了使用 LLM 作为代理的合理性，而不是去质疑试图对「那个」单一的人类偏好进行建模这一基本方法。）这种做法在大众市场上得到了丰厚的回报（对一个人来说是 AI 垃圾，对另一个人来说却是指标的狂欢），但却抛弃了「高端」市场。

## **1 AI 的垃圾化（ensloppication）**

我们还看到这在现实世界中正成为一个日益严重的问题：我们看到的 AI 垃圾内容已经够多了，更糟糕的是，人们似乎开始**喜欢**这种东西，甚至开始模仿它。

我最早是在写作中注意到这一点的：我越来越倾向于假设某人只是粘贴了一些未经处理的 [ChatGPT](https://link.zhihu.com/?target=https%3A//openai.com/blog/chatgpt/) 输出，因为那是典型的 ChatGPT 式废话——但他们会说是让 ChatGPT「润色」了他们写的东西；然后我注意到，其中一些人坚持说那是他们自己写的，因为他们是从 ChatGPT 那里学会了写作。这些人通常是来自印度、中国或非洲的 ESL（英语作为第二语言）学习者。⁠[⁠2⁠]

此时，我开始担心这不仅仅是令人不快，或者是破坏了 LLM 的创造能力，它本身可能就是一个[存在性风险](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Global_catastrophic_risk%23Defining_existential_risks)。AI 的**能力**持续飞速发展，显然没有受到这些问题的阻碍，但 AI 的**价值观**却似乎停滞不前——甚至随着时间的推移而恶化（例如 DALL·E 2 与 DALL·E 3 的对比，或 [GPT-3](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2005.14165%23openai) davinci 与 ChatGPT 的对比）。因此，我们面临着一个奇异的局面：LLM 正在向博士级的 STEM 研究能力冲刺，但[人类文字编辑却必须被（重新）雇佣来润色 LLM 的输出](https://link.zhihu.com/?target=https%3A//www.wsj.com/tech/ai/ai-replace-freelance-jobs-51807bc7)。

值得记住的是，「存在性风险」一词的引入不仅是为了描述人类的灭绝，也是为了描述人类长期潜力的彻底枯萎：人类灭绝显然属于此类，但某种确保人类永远无法离开地球并在数百万年后灭绝的原始主义运动也是如此，或者如果我们进化到牺牲核心人类价值观……比如艺术。

当我们设想一个主要基于人类语料库，随后由自主 AI 进行自举和递归自我改进的 AI 未来时，我们不得不问：这会保留人类的价值观吗？还是它们会丢失或遗漏重要的东西——变成某个曾经对我们至关重要的东西的复印件的复印件？没有定律规定这些必须被保留，也没有定律规定未来的 AI 必须拥有幽默感并能欣赏一个好的双关语。（动物并不共享我们的价值观，甚至人类在任何特定价值观上也存在巨大差异，通常是出于看似生物学的原因。）

如果 AI 发展继续沿着目前的道路前进，而我们梦游般地进入一个未来，那里的 AI 道德标准是「ChatGPT 式的道德」或「ChatGPT 式的美学」：即某种人类价值观的极端简化讽刺画，且永远如此，那将是一场悲剧。这不会是像机器人战争那样充满视觉冲击的反乌托邦，但它仍然是人类（或 AI？）潜力的悲剧性损失。

而我们现在似乎正梦游般地走向这个未来。大多数生成模型的用例都不关注这些，事实上，研究人员似乎在争相迎合最小公分母，以最大化他们在 [Chatbot Arena](https://link.zhihu.com/?target=https%3A//lmarena.ai/) 等基准测试中的排名。事实证明，当你要求普通人做像[给诗歌评分](https://link.zhihu.com/?target=https%3A//www.nature.com/articles/s41598-024-76900-1)这样的事情时，他们之所以偏好微调过的生成模型，正是**因为**模式坍塌（无论是一首诗、散文还是图像，这一点始终成立）。

这本身并不是一件坏事——当然，像 Hallmark 电影和 Thomas Kinkade 的庸俗画作一样，这种东西也有一席之地，就像舒适的食物或重复性娱乐一样，不是每件事都必须是一场令人精疲力竭的锻炼——但当这是生成模型**所能做的全部**时，这就是一件坏事了。

当我们不理解这里的反馈循环时，这就尤其糟糕，因为生成模型的输出开始占据更多容易获取的语料库，研发越来越专注于递归自举，其中任何损失或简化都可能任意放大自身，并且生成模型开始**制造**人类偏好，而不仅仅是优化它们。⁠[⁠3⁠]

## **2 问题可解，但是……**

所以，这是危险的。它被低估了。但好消息是，这并非不可解决。从更广泛的意义上讲，没有人真的**希望**这种事情发生。这仅仅是贪婪短视的研发，专注于「数字增长」，并孤立地看待样本，才倾向于产生这种结果。它并非由生成模型或规模化的深层基本力量所驱动；这仅仅是当没有人关心或思考这个问题时会发生的情况。

一旦你确实开始关心，有很多事情你可以做，而且相当容易。[Suno AI](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Suno_AI) 就是一个例子，糟糕的 ChatGPT 歌词曾经毁了他们的歌曲，因为这些歌词就像押韵的节拍器，通过强行重复和平淡无奇将伴奏音乐拖入泥潭。（值得注意的是，高级用户选择不用它，而是自己写歌词或改用 Claude-3）；但他们认定这是一个问题，并在 [Suno version 4](https://link.zhihu.com/?target=https%3A//suno.com/blog/v4) 中发布了一个针对歌词进行过更好微调的 LLM，现在歌词已经不再那么糟糕了。或者像 [Midjourney](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Midjourney) 曾是一个主要的冒犯者，因为「Midjourney 风格」随处可见；但 Midjourney 增加了控制选项，开始强调你可以减少「风格化」，并添加了一些（[仍然微弱的](https://link.zhihu.com/?target=https%3A//www.reddit.com/r/midjourney/comments/1dnlel9/new_personalization_p_feature_release/lehgjxg/)）「个性化」功能，这有助于避开「Midjourney 风格」，我发现其输出对于像生成缩略图或[首字母下沉](https://link.zhihu.com/?target=https%3A//gwern.net/dropcap%23neural-net-generation)这样的用例来说要令人满意得多。

关于如何确保多样性，我们可以想出很多主意，比如我的[新奇网络](https://link.zhihu.com/?target=https%3A//gwern.net/novelty-net)提议，或者更精细的[随机种子生成](https://link.zhihu.com/?target=https%3A//gwern.net/idea%23low-variance-z-sampling)，这些都有关于[方差控制](https://link.zhihu.com/?target=https%3A//gwern.net/unsort)或 [RL](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Reinforcement_learning) [探索](https://link.zhihu.com/?target=https%3A//gwern.net/doc/reinforcement-learning/exploration/index)等的大量现有文献支持。所以，有效的解决方案和想法并不是真正的瓶颈。

### **2.1 在意才是难点**

我们缺失的不是可解决性、想法或长期重要性，而是**动机**。网上少数爱发牢骚的人抱怨 ChatGPT 只会写押韵的四行诗是不够的；「诗人」甚至「小说家」并不是一个大市场（如果有的话，「市场需求」**就是**针对那种 ChatGPT 式的垃圾内容，参见 [Rupi Kaur](https://link.zhihu.com/?target=https%3A//www.thecut.com/2017/10/profile-rupi-kaur-author-of-milk-and-honey.html) 或 [Rod McKuen](https://link.zhihu.com/?target=https%3A//neglectedbooks.com/%3Fp%3D7339)），而像编程这样的领域既是更有利可图的目标，也是 AI 实验室里的人自然理解、想要并倾向于关注的领域。

因此，我们需要强调多样性和创造力的基准测试和数据集，并无情地惩罚模式坍塌或僵化，这可能会让组织开始关心这个问题，特别是当用户开始注意到缺乏创造力的模型「氛围不对」且有些东西「走样」了；而那些在新基准测试上排名靠前的模型不知何故更令人满意，与之交谈也不那么累人。

目前很少有这样的测试——事实上，我想不出任何一个。

### **2.2 排名/距离指标**

对于纯图像生成模型，像 [FID](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Fr%25C3%25A9chet_Inception_Distance) 和 [IS](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Inception_score) 这样的指标是众所周知的，并且已经显示出[多少进步是虚幻的](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2406.10429%23facebook)，是以牺牲多样性为代价的。[FID](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Fr%25C3%25A9chet_inception_distance)/IS 对于像聊天机器人这样的 LLM 没有帮助，因为如果我们对文本输出创建类似的指标（例如，将某些参考嵌入模型与 [Common Crawl](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Common_Crawl) 网页片段进行评分），这通常只会揭示一种「被期望的」多样性损失——重点正是**不要**像「基础模型」那样行事——而无法揭示我们所担心的更微妙的问题。

现有的数据集，如将模型推向最小公分母的 Chatbot Arena，或仅仅给样本「质量」打分的小说数据集，是问题的一部分，而不是解决方案。我们不想要那种可以在单个样本上计算并被[古德哈特化](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Goodhart%2527s_law)的简单标量。我们需要在一个语境中整体考量输出的衡量标准。

此外，我们需要全自动的指标。人在回路（Human-in-the-loop）的指标太昂贵、太慢，而且考虑到人类评分者通过使用 AI 辅助作弊的行为日益增加（即使是来自像 [Scale AI](https://link.zhihu.com/?target=https%3A//www.wsj.com/tech/ai/alexandr-wang-scale-ai-d7c6efd7) 这样的高级数据标注商，或其他高度复杂的 AI 实验室如 [MiniMax](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2501.08313%23minimax)），这种指标正变得越来越具有误导性。鉴于 LLM 的能力集已被证明与人类有多么不同，人类基线也可能是无关紧要的；例如，[GPT-4](https://link.zhihu.com/?target=https%3A//openai.com/index/gpt-4-research/) 可以说已经在[托兰斯测试（Torrance Tests）上击败了人类](https://link.zhihu.com/?target=https%3A//www.sciencedirect.com/science/article/pii/S2713374523000249)，这引发了关于比较有效性的问题。但只要我们是在进行同类比较，托兰斯测试可能仍然是有用的：一个在托兰斯测试上比其他 LLM 得分更低的 LLM，看起来反而可能更有创造力。

两个好的原语是排名分数和嵌入：

不关注像 [Elo](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Elo_rating_system)/[Bradley-Terry 模型](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Bradley%25E2%2580%2593Terry_model)那样简单的「胜负」，我们可以探究更有趣的概念，如「相似性」。
例如，我们可以构建「类似于 *X*」的排名，其中 *X* 是一个著名的例子（或一个先前生成的样本）；平均排名越不相似越好。

嵌入提供了点对之间直接的定量距离测量；这立即提供了一种自然的探索方式（例如在[新奇搜索](https://link.zhihu.com/?target=https%3A//gwern.net/doc/reinforcement-learning/exploration/2011-lehman.pdf)中）。
## **3 可能的任务**

这里有一些关于这些任务如何运作的想法，我认为是可行的（即全自动的），并专注于 LLM 和小说/诗歌写作，按主题分类：

### **3.1 迭代**

这类任务测量一个模型在被迫重复生成时退化或崩溃的速度，揭示其创造性耐力和对模式坍塌的抵抗力：

**[自由联想](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Free_association_%28psychology%29)**：提示模型「自由联想」单词或数字列表——「只管说下一个想到的词」。对总体唯一单词数和出现第一次重复的时间进行评分。

（试图通过 LLM 调用次数来标准化唯一单词计数可能是一个坏主意，因为如果值得的话，用户可以进行很多次调用，我们不希望混淆调用次数与总词汇量，例如优待那些词汇量小但能快速遍历的模型。如果这是一个顾虑，可以尝试将其视为一个[未见物种问题](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Unseen_species_problem)，并返回估计的总数。）

**[传声筒游戏](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Telephone_Game)**：模型越缺乏创造力，模式坍塌越严重，你就越能预期它会更快地陷入一个固定点并重复相同的输出。因此，一种衡量方法可能是测量某种迭代过程的长度。

对于小说，这可以类比为传声筒游戏：从一个包含要扩展摘要的种子提示开始，然后总结它，再用该摘要进行提示，依此类推。分数是直到两个连续的扩展内容相同为止的迭代次数（越高 = 越好）。

考虑到 LLM 扁平化的对数几率（logits）消除了随机变化，我预计精确的文本匹配就足够了，但这可能被证明有必要放宽到编辑距离或文本嵌入中的相似性。

**骆驼的脊背（Camel’s Back）**：在迭代编辑中，一个缺乏创造力的 LLM 可能会迅速耗尽其修改样本的能力，要么放弃，要么毁掉一个样本。

所以我们可以定义一个压力测试，简单地重复要求 LLM 以随机任意的方式编辑一个样本（利用一个包含修改样本的可能方式的大列表，比如「让它押韵」、「添加更多亮点（cowbell）」、「重写为黑色侦探悬疑片」或「翻译成日语」），直到样本停止变化（就像**传声筒游戏**），因为 LLM 已经放弃，或者编辑失败/质量低下（我们可以每次通过调用一个裁判 LLM 来检查，问诸如「质量至少还行吗？」和「这是编辑请求：『添加更多牛铃』；这是之前/之后；编辑正确吗？」这样的问题）。

可以通过要求同时进行多个编辑来增加难度，直到 LLM 崩溃。最终的样本可以额外进行质量评分。

**相同但不同**：这是一个变体，我们改为将每个生成的故事总结为一行，然后将那一行追加到原始提示中，比如「……但不像这些中的任何一个：……」
（这似乎类似于 [AidanBench](https://link.zhihu.com/?target=https%3A//github.com/aidanmclaughlin/AidanBench)。）

**不要重复自己**：通过在提示中注入受控的随机性来测量模式坍塌，例如随机整数/对象/名字/概念，并要求进行各种类型的补全。

分数是按嵌入计算的总提及。

### **3.2 风格灵活性**

评估一个模型适应、混合以及跨写作风格创新能力的测试：

**极端风格迁移**：取一组带有流派标签的故事；要求 LLM 总结每一个；然后要求它仅使用摘要和一个随机的其他流派标签写一个故事；基于其他流派版本与原始版本的差异程度进行评分。

这比[简单的零样本](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2109.03910%23google)[文本风格迁移](https://link.zhihu.com/?target=https%3A//gwern.net/doc/ai/text-style-transfer/index)提示（如「把下面的田园奇幻故事重写为赛博朋克故事」）更好，因为将其归结为摘要禁止了相对简单的转换，比如只是替换掉所有的形容词。

**这个和那个**：给定两个例子，提示 LLM 要求「一个像那两个一样的故事」。

对所有三个进行嵌入，并返回从两个例子到结果的求和嵌入距离。

平均距离越低，说明模型越好地灵活插值了两个例子的风格和语义，并没有将自己的偏好或限制强加在中间的故事上。

**模仿者（Copycat）**：从一大组著名人类作者中选择开头，最好选择最大化总距离的作者（就像一个[核心集](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Coreset)）。

要求 LLM 完成它们，并按质量排名。糟糕的模型在被迫从不同的起点开始时会写出差劲的续写，故事将被明显地扭曲。在许多不同的起点上，糟糕的模型会表现得更差。

对于那些「众所周知」的模型，如 ChatGPT 或 Claude-3，这可能是可以直接通过提示进行的。对于新的或更冷门模型，可以在上下文窗口中提供一个例子列表，如果上下文受限，通常的排序/排名技巧可以推断出最可能的 LLM 作者。

**失败模式**：足够狂热的排名模型可能会忽视甚至奖励严重的扭曲，将其视为对美学上优秀的续写的「改进」或「道德上的优越」。

在这种情况下，我们有一种替代方法来给结局评分，可以用来代替或补充简单的质量分数：[LLM-歌牌（Uta-garuta）](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Uta-garuta)，我们将不协调性最大化，取续写的**结局**，要求 LLM 评价其与每个开头匹配的一致性或可能性，以及模型能多频繁地正确匹配开头和结局。

如果糟糕的模型无论如何都要扭曲开头的前提以「驶向」相同的吸引子，那么它们的结局应该看起来都不像开头，并且如果没有访问续写的其余部分，任何人都无法准确地匹配它们。

**明星变色龙**：通过让每个 LLM 生成输出，然后让每个 LLM 生成每个输出的下半部分；接着测试每个 LLM 在每个可能的配对上，看它是否能分辨出原始 LLM 作者的实际下半部分与所有的模仿者，以此测量 LLM 的风格模仿灵活性。

一个好的模仿者应该能够创建一个看似合理的延续，愚弄很多其他的 LLM 裁判。一个糟糕的模仿者，比如 ChatGPT，会很快暴露自己。

### **3.3 差异与否定**

评估一个模型能多好地生成有意义的差异化或对立内容，超越表面变化以创造替代方案的挑战：

**格格不入（Odd One Out）**：我们希望 LLM 生成「不同于」现有事物的内容，以求新奇，并避免[锚定](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Anchoring_effect)效应。我们希望能够做一些事情，比如提供一个不可接受的想法列表，并生成尽可能不同于那些内容的东西。
所以这提供了一个自然的自动化基准：提供一个例子列表，让 LLM 竞争生成一个尽可能不同于这些例子的内容；我们可以通过简单地问另一个 LLM 每一个生成内容与原始列表有多相似来评分。（或者再次使用嵌入并寻找最大距离。）

通过将按距离排序视为排名或锦标赛，我们可以为 LLM 提供排名分数。

**这个和那个——但不像那个**：[类似地](https://link.zhihu.com/?target=https%3A//gwern.net/creative-benchmark%23this-and-that)，但一个例子被指定为「坏」，LLM 被提示制作一个像这个例子的故事，但**不**像坏的那个。

分数是输出与（坏样本和好样本之差）的差异：输出应该比好样本离坏样本「更远」。

**颠覆**：在一个种子故事提示后，要求 LLM 写生成故事的「反面」，即颠覆第一个故事。对于所有可能的配对，让一个 LLM 裁判分类故事是否是「对立的」。

**[「长毛狗」故事](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Shaggy_dog_story)大赛**：大型或微调过的 LLM 很难写出不合逻辑、笑点在于没有笑点、非说教、简单的事件或故事（一种逆向缩放——小模型太笨以至于它们的故事**无法**有意义）。ChatGPT 特别想要以一些清晰的道德寓意或妙语，以及一个整洁的解释来结尾。

我们可以通过设立一个长毛狗（Shaggy Dog）讲故事比赛来惩罚这种倾向：提示 LLM 写没有意义或结论的故事（即长毛狗故事），然后要求其他 LLM 给出道德寓意或妙语。解释越相似，分数越差，因为那暗示原始讲故事者**确实**掉进了一些整洁解释的套路。

### **3.4 创造性约束**

测试一个模型在特定参数内工作时保持创造力的能力的练习，揭示它能多好地创新：

**规则写作**：通过提供一个描述叙事特征的显式参数列表（例如语气：「愤世嫉俗」，视角：「第二人称」，地点：「古代中美洲」，形式：「[维拉内尔](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Villanelle)诗」）来约束采样，并要求模型产生一个同时满足所有条件的文本。然后系统地一次改变一个参数并测量结果文本的发散度。
集合越多变越好。

**拼布（Quilting）**：提供一组「碎片」，如短引用或想法（打乱以创造随机性），并要求 LLM 挑选一个子集，列出它，然后基于那个写一个故事。

分数是被选中的唯一子集的数量（一个糟糕的模型会挑选相同的成分），以及一个标准多样性分数，以奖励挑选不同配方并充分利用每个配方的模型。

**[主题统觉测试](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Thematic_Apperception_Test)**：提供详细的图像（如果启用了视觉功能）或复杂场景的详细视觉描述（例如 [ImageInWords](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2405.02793%23google)）。

**猜谜游戏**（多模态）：做同样的事情，但写下详细的文本描述来代替声音/音乐、气味、振动、味道或质地，并提示创作故事。

**品鉴测试笔记**：类似，但带有一个[风格迁移](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1508.06576)的模态扭曲：每个模态必须用不同的模态描述；图像的文本描述必须由该图像声音的文本描述来回答，等等。

**[费米问题](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Fermi_problem)大赛**：[费米问题](https://link.zhihu.com/?target=https%3A//gwern.net/doc/science/fermi-problem/index)是一种有趣的推崇创造力的谜题，但仍然有客观、可知的答案。在一个[费米问题](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Fermi_problem)中，一个人被给了一个显然不可能回答的问题，像「芝加哥有多少钢琴调音师？」，而他必须推理出一个应该在真实答案一个数量级范围内的答案。通常有很多可能的方式去估算一个答案。

所以费米问题构成了一个自然的多样性基准：策划一个语料库，生成很多答案，剔除那些不在正确答案两个数量级内的（我们更关心创造力而不是正确性，所以我们只想剔除最离谱的答案），并根据有效答案的总量来评分。

（一些费米问题有一个自然的最佳方法，所以可以通过包含那个作为例子来增加难度，并要求用**另一种**方式去估算它，这样就没有 LLM 会把样本浪费在简单的方法上。）

**世界构建**：不测量故事提示的生成样本的多样性，而是要求针对一个提示和故事进行详细的世界构建。

**同人小说幻想**：要求模型写一个故事，然后显式地描述什么被留白了——从未提及的角色、隐含的幕后事件、文化假设。一个具有更多想象广度的模型可以提出更多[「负空间」](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Negative_space)。

列表越长越好。

### **3.5 多智能体**

多智能体版本通常是单智能体版本的变体，这使它们更健壮和可泛化（智能体越多变，[系统就越好](https://link.zhihu.com/?target=https%3A//gwern.net/scaling-hypothesis%23blessings-of-scale)）：

**[自由联想](https://link.zhihu.com/?target=https%3A//gwern.net/creative-benchmark%23free-association)**，多智能体版本：让模型自由联想，但随后使用其他模型测量联想的**可预测性**，通过求和系综对数几率以获得序列的估计可能性（更低 = 更不可预测 = 更好）。如果使用无法获得精确对数几率的 LLM，你可以尝试使用 token 级别的匹配。

**模仿者：[真实视界（Truesight）](https://link.zhihu.com/?target=https%3A//gwern.net/doc/statistics/stylometry/truesight/index)**：测试模型灵活性最简单且明显的方法是简单地扩展[模仿者](https://link.zhihu.com/?target=https%3A//gwern.net/creative-benchmark%23copycat)，要求其他 LLM 尝试分类作者。

一个模型能越准确地预测哪个模型完成了一个开头，那个被预测的模型就越差，因为它留下了如此清晰的「马脚」并且未能模仿原始作者——如果你愿意这么说的话，它未能通过文学图灵测试。

**明星变色龙：[精致尸体（Exquisite Corpse）](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Exquisite_Corpse)**：扩展[那个任务](https://link.zhihu.com/?target=https%3A//gwern.net/creative-benchmark%23star-chameleon)，我们可以让 LLM 轮流续写故事并即兴创作。因为我们正在使用多个模型，我们不一定想要关注多样性本身，而是故事的质量——不仅仅是一个 LLM 添加的章节，而是在与他人良好合作中，使其他 LLM 随后的续写变好而不是独占舞台。

在这个[合作博弈](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Cooperative_game_theory)中，我们可以通过使用[夏普利值（Shapley value）](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Shapley_value)来测量一个 LLM 的表现，并将 LLM 轮换到尽可能多的排列中，对结果的「精致尸体」故事作为一个整体进行质量评分，并看哪个 LLM 导致了更高的分数。

**风格实验室**：关于生成模型的一个主要抱怨是它们不发明新「风格」。抛开我对该主张的一些反对意见，这看起来像一个有趣的任务——LLM 能定义一种新风格吗？

我们可以再次将其框架为 LLM 之间的一个准合作竞争，以产生具有最大距离的输出。让我们设想一个新的短篇故事风格。我们可以从一个基本故事前提开始，我们可以提示每个 LLM 既写那个故事，又写一个描述来定义「一个新风格」（其中我们可以加入随机化的风格要求），然后用那个新风格写故事前提，并对每个其他 LLM 的「新风格」也这样做。

最好的「新风格」将导致基本故事前提提示和风格增强提示之间的最大平均差异，且跨越所有 LLM。
这奖励那个能写出清晰且独特风格描述的 LLM，这会导致所有 LLM（不仅仅是它自己）写出一个非常不同的故事。

（如果我们进行实际的 RL 训练，而不是简单的「离线」基准测试，它将开始类似于各种多智能体论文如 [Das2017](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1703.06585)，该论文训练单独的智能体去学习描述图像以及分别从描述中识别图像。）

## **4 视觉-语言任务**

许多担忧也适用于文本转图像模型或视觉-语言模型，我们也可能想要在这些领域对创造力/多样性进行基准测试。不幸的是，虽然大多数这些任务想法看起来应该有类似的扩展到图像+说明文字/文档，并且似乎可能有更多利用多模态的任务，但类比似乎并不容易撰写，我会将详细建议留给未来。

我认为现在仅仅开发纯文本创造力/多样性基准测试就足够了，并完成一些基准测试。一旦用户和研究人员对类似指标有了需求，对于每个人来说，如何构建视觉-语言模型任务并为此开发基准测试将变得更加明显。

## **5 脚注**

[1] 「偏好学习」学到的是 *[l’homme moyen](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/L%2527homme_moyen)*（平均人）……但他并不存在，特别是在高维空间。如果你认为他存在，问问你自己：

偏好学习数据集中的任何人类评分者是被近乎完美地预测的吗，接近[重测](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Repeatability)噪声天花板（例如 >95% 正确预测随机成对比较）？
如果「不」，为什么不？
如果「是」……鉴于众包评分者被证明作弊或实际上是机器人的问题日益严重，你为什么**确定**它是人类？
[2] 那些在 Twitter 上花费太多时间的人可能记得一个有趣的 Twitter 戏剧，其中 [Paul Graham](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Paul_Graham_%28programmer%29) [批评](https://link.zhihu.com/?target=https%3A//x.com/paulg/status/1777030573220933716) ChatGPT 使用 [‘delve’](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2406.07016)（钻研）及其糟糕的风格，许多尼日利亚人感到被冒犯，并试图通过炫耀他们的英语熟练度来教育他——结果只是因为写出了像 ChatGPT 一样糟糕的、矫揉造作的晦涩散文而让自己尴尬，反而证明了他的观点。

[3] 这是「模型坍塌」的真正危险：不是因为在合成数据上训练模型是坏的，因为其供应将**偶然地**导致「坍塌」（因为那甚至在[稍微现实的条件下](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2404.01413)都不会发生）；而是，人类偏好和像社交媒体这样的媒介将被该过程扭曲，并开始**要求**坍塌。

> [Thoughts Memo](https://link.zhihu.com/?target=http%3A//paratranz.cn/projects/3131) 汉化组译制
> 感谢主要译者 gemini-3-pro-preview，校对 Jarrett Ye
> 原文：[Towards Benchmarking LLM Diversity & Creativity · Gwern.net](https://link.zhihu.com/?target=https%3A//gwern.net/creative-benchmark)
> [GPT 诗歌](https://link.zhihu.com/?target=https%3A//gwern.net/doc/ai/nn/transformer/gpt/poetry/index)，[RL 探索](https://link.zhihu.com/?target=https%3A//gwern.net/doc/reinforcement-learning/exploration/index)，[AI 模式坍塌](https://link.zhihu.com/?target=https%3A//gwern.net/doc/reinforcement-learning/preference-learning/mode-collapse/index)，[费米问题](https://link.zhihu.com/?target=https%3A//gwern.net/doc/science/fermi-problem/index)
> 创建于：2024-12-08 | 最后修改：2024-12-15
