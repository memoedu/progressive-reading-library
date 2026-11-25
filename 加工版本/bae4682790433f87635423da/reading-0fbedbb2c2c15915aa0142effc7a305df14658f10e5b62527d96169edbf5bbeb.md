# 关于 memory systems 的想法

---

## 正文

[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

_作者：Michael Nielsen，2022 年 11 月 23 日_

我围绕 [memory systems](https://michaelnotebook.com/tag/tft.html) 做过大量工作——既作为 user，也作为 researcher，还帮助 develop 新的 systems。我发现它们极其有帮助。但我也相信，它们还可以被做得好得多。在下面的笔记中，我围绕如何改进它们，随想了几十个[1](https://michaelnotebook.com/bbms/index.html#fn1) ideas。我预计，其中大多数 ideas 的效果不佳，或者需要大量 iteration，或者只有与其他 ideas 配合才会奏效。但也许其中少数 ideas 经过一番努力，能让 memory systems 好得多。欢迎围绕这些 ideas 继续发挥，并将它们 build 到你的 systems 中。

**关于 provenance 的说明：** 对于任何认真使用过 memory systems、深入阅读过 human memory 相关 literature 的人来说，这些 ideas 中的许多都相当显而易见。我当然无意声称任何形式的 priority。其中少数 ideas 也许是新颖而有洞见的；如果确实如此，那么功劳当然有一部分属于 Andy Matuschak，我曾与他花费数百小时讨论 memory systems。

**预设的 audience：** 这些笔记是供我自己阅读，也供几位朋友参考的。我写这些笔记，主要是为了梳理和激发自己的思考。不过，如果其他人也感兴趣：这些笔记假定你已经大量（100+ hours）使用过 memory systems，包括大量用于 conceptual understanding，而不只是 remembering 一门新 language 中的 words 之类的情况。那些仅仅短暂尝试过 memory systems 的人，往往抱有许多 misconceptions：我刻意没有讨论那些 misconceptions。我此前也表达过[关于 memory systems 的 design 的许多看法](https://michaelnotebook.com/tag/tft.html)，其中大部分不会重复。不过，仍有几点背景说明值得一提：

**我不喜欢常见的 spaced repetition framing：** 对 memory systems 的一种常见 framing，是将其视为“spaced repetition systems”（SRS）。我自己也用过这种 framing。但 spacing 和 repetition 只是其中很小的一部分。更重要的是，我们的 memory（以及更广泛而言的 cognition）的运作方式，会受到我们与之 interact 的 systems 的 moulded。通过仔细 designing 那些 systems，我们可以从根本上 change 和 improve 我们的 memory 的运作方式。这不仅仅关乎 spacing 或 repetition。

**现有的 memory systems 有什么问题？** 下面是一份远不 exhaustive 的 list：花费太多时间；变得无聊；不够 vivid；不够 emotionally engaging；tests 不够 demanding；不够 social；与生活中其他部分 connected 得不够紧密；它们往往针对 disembodied facts，而不是 skills 的 real practice（例如 motor、sensory、social 和 emotional skills）；cues 并不 correspond to 实际的 situations of use；它们在 support material 的 deep processing 方面做得远远不够[2](https://michaelnotebook.com/bbms/index.html#fn2)；人们往往过分倚重 remembering facts，却不够重视 constructing abstractions 和多种 ways of understanding。这些 ideas 中的许多可以更积极地表述为 questions：

-   如何 increase 正在被 encoded 的 understanding 的 depth？
-   如何 raise emotional stakes？
-   如何让它更加 social，使 memory 成为我们一起做的事情？
-   如何将它 strongly connect 到 meaningful projects？
-   如何 support 人们 greatly improving 他们使用 memory systems 的方式？

**常见的 critiques：** 关于 memory systems，有许多常见的 critiques。常见的包括：“rote memory 并不等同于 understanding”\[很有道理，但如果 system 使用得当，就无关紧要\]；“我用它来 memorize 一些自己实际上没什么兴趣的东西”\[极其常见：你应该停止使用 memory systems！\]；“我用它来 memorize 一些东西，作为聚会上的炫技，结果觉得很无聊”\[同上：停止使用\]；“它们对我没有效果”\[这往往意味着使用不当，就像试图用脚拉小提琴，或者这个人根本不需要它们\]。虽然我列出了常见的反驳，但那些 questions 也是有用的启发。有没有办法让这些 questions 不那么 relevant，甚至变得 irrelevant[3](https://michaelnotebook.com/bbms/index.html#fn3)？

**关于 improvement 的老套 ideas：** programmers 初次了解 memory systems 时，最常见的三种积极回应是：“我敢说，你可以用 ML 来 automate question construction，从而获得更高的 efficiency”；“你为什么不把你的 note-taking 和 memory system integrate 起来”；以及：“你为什么不 improve scheduler？”这些 ideas（以及另外几个）_确实是_老生常谈，而我有个坏习惯，就是听到它们就翻白眼。这是我的错。的确，做这些事情的 naive attempts 所带来的 returns 有限。但更深入的思考和发挥，或许能带来真正的 returns。在列出下面的 ideas 时，我有意识地尝试拥抱它们，而不是回避它们。

**Memory 不是目的：** 使用 memory systems 的目的，是让生活更好，而不是为了 memory 本身。暂且换一种 tool 来说，note-taking systems 的一个常见现象是，它们会变成目的本身。我认识一些人，可以花上几个小时谈论自己的 note-taking system，但似乎并没有多大兴趣用它来改善生活中的其他事情。我想这是一种有趣的爱好，似乎也没什么不好，但我确实对这种动机感到疑惑。这是在用 system building 来自我安抚吗？这种事情在 memory systems 中似乎还不那么常见（至少目前如此），但很容易想象它会发生。

**许多 easy-to-measure 的事情相对不重要；许多重要的事情却是 hard-to-measure 的：** commercial products 和 academic research 中，大量关于“improving”的工作，都集中在容易 quantified 的事情上。寻找“optimal” schedule。研究人们的 memory response curves。诸如此类。这是一个典型错误：专注于那些容易 measure、却并没有那么重要的事情。在我个人的 memory practice 中，几乎所有收益都来自学会如何：更好地 digest material；制作更好的 questions 和 answers；更好地将 memory system connect 到我的生活和 creative work。这些都很难 quantify——围绕这些事情撰写 academic papers，或者为下一次 performance review 设定 OKR's，大概都很困难。但我怀疑，如果你真的想 improve memory systems，而不只是完成你的 OKRs，那么这类 difficult-to-quantify targets 才是好得多的 targets。

**“对于一个有强烈进取心、富有创造力，并且精通这个 system 的天才来说，memory system 应该是什么样的？”** 我认为，这才是研究 memory systems 时恰当的 prompt。Software design 常常过于关注一个人最初几个小时的 experience。然而，你真正想要的，是把一个人在使用到第一千或第一万个小时的 experience 发挥到极致。钢琴似乎主要是为 experts 而 designed，只是顺带照顾 beginners[4](https://michaelnotebook.com/bbms/index.html#fn4)。如果你按照现代 software design practice 的思路 designing 钢琴，它就会有 8 个 white keys，没有 black keys，也没有 pedals。用它弹一些简单的歌曲会很容易，但也仅此而已。我们真正寻找的，是那些能够成为 long-run improvement 的 foundation、并具有极高 ceiling 的 ideas。

**Simple-but-powerful 的 ideas，具有极高的 ceiling：** Anki 的价值有 90% 来自一件简单的事：按照 variable schedule 向你展示 prompts-and-responses，而这个 schedule 由你做出的 binary choices 来 controlled。它有一大堆额外的花哨功能，但几乎都无关紧要。还有哪些同样 powerful 的 ideas？换句话说：下面我围绕几十个 ideas 展开随想。全部探索一遍会很有趣。但我怀疑，building 一个更好的 memory system，意味着只需找出一个真正 powerful 的 idea，然后在它上面加倍投入[5](https://michaelnotebook.com/bbms/index.html#fn5)。

## 关于 memory systems 的想法

我发现自己在拟这份清单时，竟然有些保守：我希望这些想法是“好”想法，而这与它们是否富有想象力、能否启发思考存在张力。注意到这一点后，我特意加了六个左右有点“傻”的想法，穿插在清单中。

1.  **Memory coach：记忆教练：**使用 memory systems 是一项 skill。大多数人写出的 questions 都很差——就像刚接触 piano 的人弹得不太好一样。假设你每月与一位 coach 见一次面，每次 90 minutes，由对方帮助你写出更好的 questions。他们会与你一起逐一查看 10-20 个 questions，帮助你 improve 这些 questions。你们还会讨论你应该和不应该提出哪些 types 的 questions，以及这些 questions 如何与你更广泛的 life goals 和 projects 联系起来。这也可以远程进行。一个变化很大的变体，是与 therapist 一起查看你的 questions，目的与其说是 improve 这些 questions，不如说是将其作为 therapy 的一部分。这与 yoga 和 meditation teachers 等想法密切相关。
2.  **将 Memory coaching 作为更广泛的 coaching 或 therapy services 的一部分：**许多 coaches 和 therapists 都提供多种 modalities。你可以 develop 一门 course——比如 30 hours 的 in-person 学习，另外要求完成 100 hours 的 offline work——让一个人达到类似“memory coaching mastery”的水平，随后将其列为自己的 competencies 之一[6](https://michaelnotebook.com/bbms/index.html#fn6)。
3.  **Memory classes：记忆课程：**这是前两项的一个变体，含义相当直观。
4.  **将 DMV manual 放进一个 memory system：**理想情况下，不只是 Anki-style。而是加入某种 narrative embedding，或许更接近 [mnemonic medium](https://quantum.country/)。这个想法也可以 inverted：也许 DMV 应该直接接受完成 mnemonic medium 的整个学习过程，以此替代参加 test。这肯定是一项要求高得多的 requirement[7](https://michaelnotebook.com/bbms/index.html#fn7)。
5.  **Memory system karaoke：记忆系统卡拉 OK：**你会收到来自其他人 decks 的 random questions，并且需要给出 answers。要让它行得通，还需要 iteration——也许展示给你的 questions 看起来像是你自己的 questions 的 variations。这可能成为一个好玩的 game。很难说具体该怎么做，不过我怀疑“thought vectors”这个想法也许有帮助。
6.  **Memory pairing / memory buddy：记忆结对／记忆伙伴：**很像健身搭子，或者 Strava 好友：有人与你一起 co-construct 一个 deck，并一起 co-construct 一套 memory practice。共享 progress 的 graphs，一起庆祝，一起叹苦经。让 memory 成为一种 social practice，肯定会 transform 它，也许会变好，也许会变坏。
7.  **Crit-for-memory：explain 你的 questions 和 answers：**我怀疑，只要经常向朋友（或 audience）explaining 你的 questions 和 answers，就会形成促使它们改善的压力。设想一下 feedback 也很有意思。可以有 Memory crit groups。
8.  **耗资 \$100 million 的 mnemonic medium：**我好奇，如果像 Substack 那样，有一大群报酬丰厚的作者，[mnemonic medium](https://numinous.productions/ttft) 会是什么样子？再加上一支出色的 core team 来 improving 这个 medium 呢？设想设置一个 Chief Memory Officer 或 Chief Memory Coach 职位，也很有意思。（这个想法我曾与 Andy Matuschak 广泛讨论过。）
9.  **为 real testing 付费：**最简单的方式是：你付 $50，请一个人花一小时与你一起逐一检查 memory system deck。你需要 explain 那些 answers。如果他们怀疑你错了，或者在胡扯，就会追问你，或许会帮助你 improve 自己的 understanding，或者至少让你诚实面对自己的 understanding。这项服务的一个变体是，如果你答错了，他们就会_冲你大喊_ :-) 。一种更 formal 的延伸，是某种与 testing 相关联的 accreditation。
10.  **Audio version：音频版本：**我经常希望 Anki 能在我散步时大声读出 questions，让我给出 spoken answers。presentation 的 variation 是否会带来 recall 的 improvement，这个设想似乎说得通，也很值得 test。
11.  **presentation 的 Auto-variation：**同样，changing font、changing color、changing margins、varying 一天中的时间、varying 地点、varying ambient noise level，甚至做一些 inserting typos 之类的事，都可能有益处。
12.  **为 answers 进行 Illustrate：**我很少觉得 DALL-E 这样的 systems 适合 illustrating questions。它往往更擅长 illustrating noun phrases。但 questions 经常包含引人注目的 noun phrases，而 answers 也经常是 noun phrases。我怀疑，这里可以做些事情，提供引人注目的 illustrations，从而可能 improve 这个 memory system 的 effectiveness。
13.  **对 questions 和 answers 进行 Enacting：**这件事一直让我困扰。Anki 的 context 完全不对。如果我 learn 了 Javascript API 的一个新部分，我并不想要 Anki 的 context 中一个 artificial question 和 answer。理想情况下，我希望一个 real use case 在我的 text editor 中出现，然后真正去 enact 那个 solution。而且这一切都应该按照合适的 schedule 发生。我想要的还不只是电脑上的 tasks。我希望所有我想 remember 的东西都能如此。
14.  **对常见的 bad question types 进行 Diagnosing：**我预计，只要一位 memory coach 为几千个 questions 写出简短的 critiques，就可以用 classifier 或 large language model 来 critique 并帮助 improve 人们的 questions。目前，我对基于 large language models 的 virtual memory coaches 持怀疑态度。但通过一些十分巧妙的 design，这一方向的东西可能已经非常有用了[8](https://michaelnotebook.com/bbms/index.html#fn8)。往往只要有人告诉你“你可以考虑 removing 这个 question／以\[某种方式\] improving 它”，就会很有帮助。
15.  **“我们需要 go deeper”：**我使用成百上千种 strategies，在我的 memory system 中 more deeply encode understanding。这些 strategies 是经过多年逐渐 evolved 而来的[9](https://michaelnotebook.com/bbms/index.html#fn9)。其他认真使用 memory systems 的 users 也报告了类似情况。遗憾的是，我只有少数几种方式，能够与他人相互 transferring 这些 strategies：一对一地一起走一遍 practice；看其他人的 examples，并问“他们为什么 add 那个 question”；告诉人们，根据我的经验，question-asking 是一项可以不断精进的 skill。有时候，仅仅知道某件事可以做得好得多、好得多，本身就很有价值。
16.  **Memory system tarot：记忆系统塔罗牌：**我不知道这是什么意思，它只是某个时候突然冒进我脑海的一个短语。我猜 [Tim Hwang](https://timhwang.org/) 会有些有趣的想法！
17.  **Jesus 会 remember 什么？**同上。
18.  **让人们为 lapses 付出代价：**practice 中的 Lapses、recall 中的 errors、未达到 [proof of memory](https://notes.andymatuschak.org/The_mnemonic_medium_can_surface_%25E2%2580%259Cproof_of_memory%25E2%2580%259D_social_signals) 的某个 goal。可以把它想成 [Stickk-for-memory](https://www.stickk.com/)。
19.  **让 memory systems 成为 ambient：**怎样才能让你在四处走动时，轻松地 collect questions？例如，我喜欢在餐馆和其他新去的地方这样做。我在某家餐馆的菜单上发现的最好吃的菜是什么？它尝起来怎么样？体验如何？一顿饭就很容易 add 10 或 20 个 questions[10](https://michaelnotebook.com/bbms/index.html#fn10)。
20.  **Memory 是我们一起做的事：**这只是一个一般性的 intuition pump。Meaning 在很大程度上确实是我们一起 construct 的，这种程度令人惊讶。我知道，如果我想 improve 自己对某件事的 understanding，就应该 teach 它，或者写下来，尤其是在 highly interactive medium（比如 twitter）上。这会大大 heightens 我对自己所写内容的所有 emotional response。从这个意义上说，让你在意的人直接参与其中的说话或写作，对 memory，乃至更广泛的 understanding，都是极好的帮助。
21.  **将你的 memory system 进行 Perform：**与此相关的是，人们为了面子，愿意做_很多事_。有 audience 在场，会提升一切的 emotional intensity——无论高潮还是低谷。如果你在 practicing piano 时弹错了一个 note，一周后你就不会 remember 了。如果你在台上、当着一百位朋友的面弹错同一个 note，你可能会 remember 好几年。我讨厌“将你的 progress 发布到 social media”这种陈词滥调，但部分原因是许多 apps 把这件事做得太差。我认为 Wordle 和（在一定程度上）Oura 实际上做得很好。我敢说，memory systems 也可以把它做好，部分方式就是与许多所谓的 best practices 反着来（那些做法将 growth 置于 user 之上）。
22.  **与 Google Maps 进行 Integrate：**我有一种偏见，认为 memory palaces 大多只是些新奇玩意儿。但也许我错了。设想 extending memory palaces 很有意思，也许可以使用 VR 和 Google Street View，或者某种类似的、对世界的 3d experience。基本想法是：弄清楚一个_真正出色_的 memory palace 会是什么样子，然后 create 它。我还怀疑，memory palaces 与 spaced repetition systems 之间可能有很多 crossover。Memory palaces 基本上是一种特定 type 的 processing。我们还能找到哪些其他 kinds？
23.  **将你的 Anki cards 发到 Instagram：**不知道会发生什么。我敢说，有人会找到办法，把自己的 cards 做得精美诱人，成为很好的 Insta-bait。也可能把两边最糟糕的部分凑到了一起。
24.  **支付 \$100,000 for a false answer:** How well do you think you'd do if you were quizzed on an Anki deck – and had to pay out $100k，来换取一个 false answer？我怀疑自己会做得相当不错。
25.  **如何将 memory systems integrate 到 Figma 中？到 Google Docs 中？到 Twitter 中？到一般的 applications 中？**坦白说，我对自己的 Anki usage 与 Twitter usage 之间竟然存在如此多的 overlap 感到震惊。我不太知道该如何理解这一点。Twitter 有时似乎可以充当某种 [incremental writing](https://supermemo.guru/wiki/Incremental_writing)。更广泛地说，我怀疑几乎任何 application 都可以被视为一个 memory system。Video games 往往在这方面做得还不错，但其他 apps 很少如此。

## Acknowledgments（致谢）

感谢 Andy Matuschak，与我就 memory systems 进行了许多启发思考的交谈。感谢 Sebastian Bensusan，鼓励我在 footnotes 中放开来谈。

## Citation information（引用信息）

Michael Nielsen, "Building a Better Memory System", [https://michaelnotebook.com/bbms/index.html](https://michaelnotebook.com/bbms/index.html), San Francisco (2022).

## Footnotes（脚注）

* * *

1.  我不会给出确切的数量：将来我可能会在这份清单中增添或删去一些条目。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref1)

2.  我怀疑这是一种 100x（甚至大得多）的 effect。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref2)

3.  我喜欢 [“让 memory 成为一种选择”](http://augmentingcognition.com/ltm.html) 这一 framing 的一点，是它相对而言是 value neutral 的。人们对 memory 有强烈的感受：觉得它是自己“应该”更擅长的东西；或者心怀怨愤。他们对 memory 在 cognition 中的 role 也抱有许多误导性的看法——它并不重要，什么都可以查；凡是需要 remember 的东西，自然会在使用中 remember；或者反过来，过分崇尚或依赖 memory。这些想法中有一些包含少许道理，但如果没有重要的限定条件，它们就会造成伤害。要让 memory systems 最有用，最好让它们的使用建立在对 memory 在 cognition 中的 role 的 accurate beliefs 之上。的确：对 memory systems 的 effective use，有时意味着 _不_ 使用它们。但也可能意味着极其密集地使用它们。诀窍在于拥有一个 accurate、感受强烈的 internalized model。围绕 memory systems 进行交流的一项重大挑战，就是帮助人们形成这样的 model。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref3)

4.  我对此并不确定，尽管它似乎说得通。比如说，对于小提琴，我就更确定一些（尽管我上过一阵子钢琴课，却从没学过小提琴）。任何家里曾有人学过小提琴的人都知道，它不是一种容易学的乐器。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref4)

5.  我们几乎所有最强大的 tools for thought 都是如此。Writing、alphabet、mathematical notation：它们都建立在极其认真地对待少数几个强大想法的基础之上。而且，同样地，你可以在数千小时里持续进步。我们教五岁的孩子 write，但没有人真正完全掌握 writing。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref5)

6.  显然，这里存在被滥用的可能。但很可能也有积极的可能性。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref6)

7.  为了通过考试，我把 California DMV manual 进行了 Ankified。随后我 deleted 了这个 deck，对此我颇为后悔，因为这意味着 manual 中的许多内容已经在我的 memory 中 decayed。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref7)

8.  我怀疑，一个实际的问题是：如果人们相信建议来自另一个人，而不是 large language model，就会更认真地对待这些建议。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref8)

9.  我喜欢的一种 strategy，也是带动许多其他 strategies 的一种，是 compress understanding。例如：“fundamental theorem of algebra 的一个 simple one-sentence proof 是什么？”这样的 questions 通常 _require_ 先有几十个其他 questions 作为准备。它们并不总是能做到，但我觉得它们极其有价值。顺便注意，那个 question 违反了另一条 master strategy，即警惕 canonical answers。对于 question “Australia 的 capital 是什么？”，essentially unique answer 是“Canberra”。但大多数有趣的 questions 都有 multiple answers，而主动利用这一点极其有价值。一个更好的 question 是：“fundamental theorem of algebra 的 two \[three, four…\] 个 different simple one-sentence proofs 是什么？”然后再添加一些关于那些 proofs 之间 relationships 的 questions。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref9)

10.  经验告诉我，有些人会大叫：“哦，听起来太糟糕了，你为什么非得把一顿饭给毁掉？”好吧，如果是这样，那就别使用 memory system。如果它会让你的生活变得更好，那就使用它。[↩︎](https://michaelnotebook.com/bbms/index.html#fnref10)

---

## 原文

[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

_By: Michael Nielsen, November 23, 2022_

I've done a lot of work on [memory systems](https://michaelnotebook.com/tag/tft.html) – as a user, as a researcher, and as someone helping develop new systems. I've found them tremendously helpful. But I also believe they could be made far better. In the following notes I riff on a few dozen[1](https://michaelnotebook.com/bbms/index.html#fn1) ideas for how they could improved. I expect most of the ideas work poorly, or need substantial iteration, or would only work in concert with other ideas. But perhaps a few of the ideas would, with some work, make memory systems much better. Feel free to riff on these ideas, and build them into your systems.

**A note on provenance:** Many of these ideas are pretty obvious to anyone who has seriously used memory systems and read into the literature on human memory. I'm certainly not trying to claim any sort of priority. A few of the ideas are perhaps new and insightful; if that is the case, then credit certainly belongs in part to Andy Matuschak, with whom I've spent hundreds of hours talking about memory systems.

**Assumed audience:** These notes are intended for my consumption and for the interest of a few friends. I'm writing the notes mostly to organize and stimulate my own thinking. That said, if anyone else is interested: the notes assume you've engaged in substantial (100+ hours) use of memory systems, including significant use for conceptual understanding, beyond cases like remembering words in a new language. There are many misconceptions commonly held by people who've merely briefly tried memory systems: I deliberately haven't addressed those misconceptions. I've also previously stated [many opinions about the design of memory systems](https://michaelnotebook.com/tag/tft.html), and for the most part won't repeat those. That said, there's a few background remarks worth making:

**I don't like the common spaced repetition framing:** One common framing for memory systems is as "spaced repetition systems" (SRS). I've used that framing myself. But spacing and repetition are only a small part of the story. The larger point is that the way our memory (and our cognition in general) works is moulded by the systems we interact with. By designing those systems carefully we can radically change and improve the way our memory works. It's not just about spacing or repetition.

**What's wrong with existing memory systems?** Here's a very non-exhaustive list: take too much time; get boring; not vivid enough; not emotionally engaging enough; the tests aren't demanding enough; not social enough; not closely enough connected with other parts of your life; they tend to be for disembodied facts, not for real practice of skills (e.g., motor and sensory and social and emotional skills); the cues don't correspond to actual situations of use; they don't do anywhere near enough to support deep processing of material[2](https://michaelnotebook.com/bbms/index.html#fn2); people tend to lean overmuch on remembering facts, and not enough on constructing abstractions and multiple ways of understanding. Many of these ideas can be phrased more affirmatively as questions:

-   How to increase the depth of understanding being encoded?
-   How to raise the emotional stakes?
-   How to make it more social, so memory becomes something we do together?
-   How to strongly connect it to meaningful projects?
-   How to support people in greatly improving how they use memory systems?

**Standard critiques:** There are many standard critiques of memory systems. Common ones: "rote memory isn't the same as understanding" \[has a large grain of truth, irrelevant if the system is used well\]; "I used it to memorize stuff that I've little real interest in" \[incredibly common: you should stop using memory systems!\]; "I used it to memorize things as a party stunt, and found it boring" \[ditto: stop\]; "they didn't work for me" \[often means they're being used poorly, like attempting to play a violin with your feet, or the person has no need for them\]. While I've listed standard retorts, those questions are also useful stimuli. Are there ways of making the questions less relevant or even irrelevant[3](https://michaelnotebook.com/bbms/index.html#fn3)?

**The cliched ideas for improvement:** The three most common affirmative responses to memory systems from programmers learning about them for the first time are: "I'll bet you could use ML to automate question construction, for more efficiency"; "why don't you integrate your note-taking and your memory system"; and: "why don't you improve the scheduler?" These ideas (and a few others) _are_ cliches, and I have a bad habit of rolling my eyes at them. This is a mistake on my part. It's true that naive attempts to do these things have limited returns. But deeper thought and riffing may yield genuine returns. In listing ideas below I've consciously tried to lean into them, rather than avoiding them.

**Memory is not the point:** The point of using memory systems is to make a better life, not memory in and of itself. To momentarily switch to a different type of tool, one common phenomenon with note-taking systems is that they become ends in themselves. I know people who can talk for hours about their note-taking system, but don't seem all that interested in using it to improve anything else in their life. I suppose it's a fun hobby, and that seems fine, but I do wonder at the motivation. Is it system building as self-soothing? This kind of thing seems less common with memory systems (so far), but it's easy to imagine happening.

**Many easy-to-measure things are relatively unimportant; many important things are hard-to-measure:** Much work on "improving" both commercial products and academic research is focused on things which are easily quantified. Finding the "optimal" schedule. Studying people's memory response curves. And so on. This is the classic mistake of focusing on what you can easily measure, but which isn't all that important. In my personal memory practice, nearly all the benefit has come from learning how to: better digest material; make better questions and answers; better connect the memory system to my life and creative work. These are hard to quantify – it's likely difficult to write academic papers about these kinds of things, or set OKR's for your next performance review. But I suspect such difficult-to-quantify targets are far better targets if you actually want to improve memory systems, and not merely hit your OKRs.

**"What does a memory system look like for a driven creative genius who is an expert with the system?"** That is, I think, the right prompt for working on memory systems. Software design so often focus on the first few hours of someone's experience. Yet what you really want is to max out the experience someone is having in their thousandth or ten thousandth hour of use. Pianos seem designed primarily for experts and only incidentally for beginners[4](https://michaelnotebook.com/bbms/index.html#fn4). If you were designing the piano with modern software design practice in mind it would have 8 white keys, no black keys, and no pedals. It'd be easy to play some simple songs, and that's it. What we're really looking for is ideas which can be the foundation for long-run improvement, with an extraordinarily high ceiling.

**Simple-but-powerful ideas, with a very high ceiling:** 90% of the value of Anki comes from simply showing you prompts-and-responses on a variable schedule, with the schedule controlled by binary choices you make. It has a tonne of extra bells and whistles, but they're almost all irrelevant. What other similarly powerful ideas are there? Put another way: below I riff on dozens of ideas. Exploring all would be fun. But I suspect that building a better memory system means just identifying and doubling down on one really powerful idea[5](https://michaelnotebook.com/bbms/index.html#fn5).

## Ideas for memory systems

I found myself curiously conservative in constructing this list: I want the ideas to be "good" ideas, which is in tension with them being imaginative or stimulating. Once I noticed this I deliberately added a half dozen somewhat "silly" ideas, sprinkled through the list.

1.  **Memory coach:** Using memory systems is a skill. Most people write really bad questions – just like someone new to the piano isn't very good. Suppose you got together for 90 minutes once a month with a coach who would help you write better questions. They'd go through 10-20 questions with you, and help you improve those questions. You'd also discuss what types of questions you should and shouldn't be asking, and how it connects to your life goals and projects more broadly. Could be done remotely. A (large) variation is going over your questions with a therapist, not so much to improve the questions, but as part of therapy. Closely related to ideas like yoga and meditation teachers.
2.  **Memory coaching as part of a broader array of coaching or therapy services:** Many coaches and therapists offer a variety of modalities. You could develop a course – say, 30 hours in-person, with another 100 hours of offline work required – where a person would achieve something like "memory coaching mastery", which they'd then list as part of their competencies[6](https://michaelnotebook.com/bbms/index.html#fn6).
3.  **Memory classes:** A pretty self-explanatory variation on the last two.
4.  **Put the DMV manual into a memory system:** Ideally, not just Anki-style. But with some kind of narrative embedding, perhaps closer to the [mnemonic medium](https://quantum.country/). This can be inverted, too: maybe the DMV should simply accept completion of the mnemonic medium's journey as a substitute for taking the test. It's certainly a far more demanding requirement[7](https://michaelnotebook.com/bbms/index.html#fn7).
5.  **Memory system karaoke:** You get sent random questions from other people's decks, and need to provide answers. Needs iteration to work – maybe you're being shown questions which seem like variations on yours. Could be a fun game. Difficult to know how to do it, although I suspect the idea of "thought vectors" may be helpful.
6.  **Memory pairing / memory buddy:** Much like a gym buddy, or Strava friends, someone who you co-construct a deck with, and co-construct a memory practice with. Shared graphs of progress, shared celebration, shared commiseration. Making memory a social practice would certainly transform it, perhaps for the better, perhaps for the worse.
7.  **Crit-for-memory: explain your questions and answers:** I suspect that simply routinely explaining your questions and answers to a friend (or to an audience) would lead to upward pressure on them. Fun to imagine feedback, too. Memory crit groups.
8.  **The $100 million mnemonic medium:** I wonder what the [mnemonic medium](https://numinous.productions/ttft) looks like with, Substack style, a whole bunch of well-paid writers? And a terrific core team improving the medium? Fun to think about having a Chief Memory Officer or Chief Memory Coach position. (This is an idea I've discussed extensively with Andy Matuschak.)
9.  **Pay for real testing:** Most simply: you pay $50 for an hour of someone working through your memory system deck with you. You need to explain the answers. If they suspect you're wrong, or bullshitting, they push you on it, and perhaps help you improve your understanding, or at least be honest with yourself about your understanding. A variation service is where they _shout at you_ if you get it wrong :-). A more formal outgrowth is some sort of accreditation associated with the testing.
10.  **Audio version:** I've often wanted Anki to read me questions aloud as I walk, and for me to give spoken answers. It's plausible – and would be interesting to test – whether the variation in presentation would lead to an improvement in recall.
11.  **Auto-variation in presentation:** In a similar vein, there may be benefits in changing the font, changing the color, changing the margins, varying the time of day, varying the place, varying the ambient noise level, even doing things like inserting typos.
12.  **Illustrate the answers:** I've rarely found systems like DALL-E useful for illustrating questions. It tends to do better at illustrating noun phrases. But questions often contain striking noun phrases, and answers are often noun phrases. I suspect it's possible to do something here which provides striking illustrations that may improve the effectiveness of the memory system.
13.  **Enacting the questions and answers:** This is something that bugs me. Anki's context is all wrong. If I learn a new part of the Javascript API I don't want an artificial question and answer within Anki's context. Ideally, I want a real use case to arise, within my text editor, and then to actually enact the solution. And this should all happen on the right schedule. And I don't just want this for tasks at the computer. I want it for everything I'd like to remember.
14.  **Diagnosing common types of bad question:** I expect that if a memory coach simply wrote short critiques of a few thousand questions, then either a classifier or a large language model could be used to critique and help improve people's questions. I'm skeptical – for now – of virtual memory coaches based on large language models. But with some very clever design something in this vein might already be very useful[8](https://michaelnotebook.com/bbms/index.html#fn8). Simply being told "You might consider removing this question / improving it in \[such-and-such a way\]" would often be very helpful.
15.  **"We need to go deeper":** I use hundreds or thousands of strategies to more deeply encode understanding in my memory system. These strategies have been evolved over years[9](https://michaelnotebook.com/bbms/index.html#fn9). Other serious users of memory systems report similarly. Unfortunately, I only have a few means for transferring these strategies to or from others: one-on-one going through the practice; seeing examples from other people and asking "why did they add that question"; telling people that question-asking is, in my experience, a skill it's possible to get continually better at. Sometimes, simply knowing it's possible to be much much better at something is valuable in its own right.
16.  **Memory system tarot:** No idea what this means, it's just a phrase that popped into my head at some point. I suspect [Tim Hwang](https://timhwang.org/) would have interesting ideas!
17.  **What would Jesus remember?** Ditto.
18.  **Make people pay for lapses:** Lapses in practice, errors in recall, falling short of some goal for [proof of memory](https://notes.andymatuschak.org/The_mnemonic_medium_can_surface_%25E2%2580%259Cproof_of_memory%25E2%2580%259D_social_signals). Think of it as [Stickk-for-memory](https://www.stickk.com/).
19.  **Make memory systems ambient:** How do you make it so that as you wander through the world, you can easily collect questions? I enjoy doing this at restaurants and other new places, for example. What's the best item I've found on the menu at such-and-such a restaurant? What did it taste like? What's the experience like? One can easily add 10 or 20 questions for a single meal[10](https://michaelnotebook.com/bbms/index.html#fn10).
20.  **Memory is something we do together:** This is just a general intuition pump. Meaning really is something we construct together, to a remarkable extent. I know that if I want to improve my understanding of something, I should teach it, or write about it, especially in a highly interactive medium (like twitter). It greatly heightens all my emotional response around what I've written. In this sense, speaking or writing with the direct engagement of people you care about is a terrific aid to memory and more broadly to understanding.
21.  **Perform your memory system:** Relatedly, people will do _a lot_ for appearances. An audience ups the emotional intensity of everything – the highs and the lows. If you miss a note while practicing the piano you won't remember a week later. Miss the same note on stage in front of a hundred friends and you may remember it for years. I hate the cliched nature of "post your progress to social media", but it's partly because it's done so poorly by many apps. I think Wordle and (to some extent) Oura actually do it quite well. I'll bet it can be done well for memory systems, in part by doing the opposite of a lot of supposed best practices (which prioritize growth over the user).
22.  **Integrate with Google Maps:** I have a prejudice that memory palaces are largely curiosities. But maybe that's wrong. It's fun to think about extending memory palaces, maybe using VR and Google Street View or some similar 3d experience of the world. Basically: figure out what a _really good_ memory palace would be like, and create it. I also suspect there's a lot of crossover possible between memory palaces and spaced repetition systems. Memory palaces are basically a particular type of processing. What other kinds can we find?
23.  **Instagram your Anki cards:** No idea what would happen. I'll bet someone would find a way of making their cards nice and juicy, good Insta-bait. Possibly the worst of both worlds.
24.  **Pay $100,000 for a false answer:** How well do you think you'd do if you were quizzed on an Anki deck – and had to pay out $100k for a false answer? I suspect I'd do quite well.
25.  **How to integrate memory systems into Figma? Into Google Docs? Into Twitter? Into applications in general?** I'm frankly shocked how much overlap there is between my Anki usage and my Twitter usage. I don't quite know what to make of it. Twitter sometimes seems like it can function as a form of [incremental writing](https://supermemo.guru/wiki/Incremental_writing). More broadly, I suspect almost any application at all can be considered as a memory system. Video games tend to this moderately well, but few other apps do.

## Acknowledgments

Thanks to Andy Matuschak for many stimulating conversations about memory systems. Thanks to Sebastian Bensusan for encouragement to be discursive in footnotes.

## Citation information

Michael Nielsen, "Building a Better Memory System", [https://michaelnotebook.com/bbms/index.html](https://michaelnotebook.com/bbms/index.html), San Francisco (2022).

## Footnotes

* * *

1.  I won't be precise in the number: I may add to or subtract from this list in the future.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref1)

2.  I suspect this is a 100x (or much more) effect.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref2)

3.  Something I like about the ["make memory a choice"](http://augmentingcognition.com/ltm.html) framing is that it's relatively value neutral. People have strong feelings about memory: that it's something they "should" be better at; or feelings of resentment. They also believe many misleading things about the role of memory in cognition – it doesn't matter, they can look everything up; that anything they need to remember they will just naturally remember through use; or conversely they overly venerate or rely on memory. There is a grain of truth in some of these ideas, but without significant caveats they're damaging. For memory systems to be most useful it's helpful if their use is founded in accurate beliefs about the role of memory in cognition. Indeed: effective use of memory systems sometimes means _not_ using them. But it can also mean using them extremely intensively. The trick is to have an accurate and strongly felt internalized model. A major challenge in communicating about memory systems is helping people form such a model.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref3)

4.  I'm not sure of this, though it seems plausible. I'm more sure of it for, say, the violin (despite the fact that I took piano lessons for a while, but never the violin). Anyone who has ever had a family member learn the violin knows that it is not an easy instrument to learn.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref4)

5.  This is true of almost all our most powerful tools for thought. Writing, the alphabet, mathematical notation: all are founded in taking extremely seriously a small number of powerful ideas. And, again, you can keep getting better for thousands of hours. We teach five year olds to write, but no-one ever really masters writing.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref5)

6.  Obviously, there is potential for abuse here. But there's likely a positive possibility too.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref6)

7.  I Ankified the California DMV manual in order to pass the exam. I then deleted the deck, something I rather regret, as it means much of the manual's content has decayed in my memory.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref7)

8.  I suspect a real problem is that people take suggestions more seriously if they believe it's coming from another human being, as opposed to a large language model.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref8)

9.  One strategy I like that drives many others is to compress understanding. Eg: "What's a simple one-sentence proof of the fundamental theorem of algebra?" Such questions typically _require_ dozens of other questions as preparation first. They're not always achievable, but I find them incredibly valuable. Note, incidentally, that that question violates another master strategy, which is to beware canonical answers. The essentially unique answer to the question "What is the capital of Australia?" is "Canberra". But most interesting questions have multiple answers, and it's extremely valuable to lean into that. A better question is: "What are two \[three, four…\] different simple one-sentence proofs of the fundamental theorem of algebra?" And then to add some questions about the relationships between those proofs.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref9)

10.  Experience tells me some people will howl "oh, that sounds terrible, why do you have to take a meal and ruin it". Well, don't use the memory system if that's the case. And if it would make your life better, then use it.[↩︎](https://michaelnotebook.com/bbms/index.html#fnref10)