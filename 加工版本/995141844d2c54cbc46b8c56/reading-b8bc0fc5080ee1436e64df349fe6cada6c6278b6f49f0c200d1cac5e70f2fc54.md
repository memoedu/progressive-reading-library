# 引言

---

## 正文

**_免责声明_**_：我在 Anthropic 担任 researcher，但这篇文章完全代表我自己的 views，而不是我的雇主的 views_

## 引言

过去两年，我一直在进入 AI Safety 这个 field。我刚进入这个 field 时，听到的一条重要信息是，我需要“form 一个关于 AI Safety 的 inside view”，需要 form 自己的 beliefs 并且 think for myself，而不是仅仅因为比我聪明的人在意某些事情，就去做那些事情。这让我压力极大！我觉得，自己 interpreted 这条信息的方式相当不健康，给我带来了大量令人无法行动的 uncertainty 和 anxiety，几乎让我放弃进入这个 field。不过，我觉得自己现在已经到了一个让我感到自在的阶段，也多少觉得自己对一些事情有了自己的 inside views，并且理解了如何 form 它们。

在这篇文章中，我试着解释自己掉进了哪些陷阱以及原因，我实际经历了怎样的过程，以及在知道了哪些事情_不该_做之后，我对于如何看待 inside views 的建议。这是一个 complex topic，我认为有许多 valid perspectives，不过，希望我的视角对一些正试图就令人困惑的 topics（无论是 AI Safety 还是其他 topics）form 自己的 views 的人来说，既新颖又有用！（注：我不会讨论为什么自己现在_确实_认为 AI Safety 很重要、值得投入工作——那是以后另一篇文章的 topic！）

## 关于 Inside Views 的信息

首先，补充一些背景，说明我所说的 **inside views** 是什么意思。按照我的理解，这是一个 defined 得相当模糊的 concept，但大致是指：“在我脑海里有一个清晰的 model 和 argument，从一些关于世界的 basic 且 reasonable 的 beliefs 出发，让我能够得出诸如‘从事 AI Safety 很重要’这样的 conclusion，而不必依赖对他人的 deferring。”这感觉与 [gears-level models](https://www.lesswrong.com/posts/gvK5QWRLk3H8iqcNy/gears-vs-behavior) 这个 concept 高度 related。与之相对的是 **outside views**，或者对他人 **deferring**：我 believe 某件事的主要原因，是我尊敬的聪明人 believe 它。在我看来，rationality community 中普遍有一种氛围，认为 inside views 是好的，outside views 是不好的（关于 outside views 和 deferring 的重要性，可以参看 Greg Lewis 的 [为 Epistemic Modesty 辩护](https://forum.effectivealtruism.org/posts/WKPd79PESRGZHQ5GY/in-defence-of-epistemic-modesty)，其中有很好的 argument！）。请注意，这_不是_这些词在 forecasting 中使用的 Tetlockian 含义：在那里，outside view 意味着“查找一个 base rate”，inside view 意味着“使用我的 human intuition，而它的 calibrated 程度很差”，通常的看法是 outside view > inside view。

这类 reasoning 的好例子包括：Buck Shlegeris 的[我从事 AI Safety 的个人 Cruxes](https://forum.effectivealtruism.org/posts/Ayu5im98u8FeMWoBZ/my-personal-cruxes-for-working-on-ai-safety)、Richard Ngo 的[从 First Principles 出发探讨 AGI Safety](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ)，以及 Joseph Carlsmith 关于 [Power-Seeking AI 带来的 Existential Risk](https://www.lesswrong.com/posts/HduCjmXTBD4xYTegv/draft-report-on-existential-risk-from-power-seeking-ai) 的 report。请注意，虽然这些文章都围绕“AI Safety 到底是不是一个 problem”这个 question，但 inside view 这个 notion 也同样适用于这样的 questions：“de-confusion research/reinforcement learning from human feedback/interpretability 是 reduce 来自 AI 的 existential risk 的最佳方式”，以此为特定的 research agendas 和 directions 提供 arguing。

## _我_如何 Interpreted 关于 Inside Views 的信息

我通常是一个相当容易焦虑的人，也不善于应对 uncertainty。遗憾的是，这条信息在我脑海里造成了一种相当不健康的 dynamic。我感觉自己必须独自弄清“AI Safety 是否是一个值得投入工作的 real problem”的 conclusive truth，_以及_哪些 research directions 有用、哪些没有用，这样才能接着去做那个 optimal 的 direction。而且，我觉得这一切都是我自己的责任，仅仅因为聪明人 endorsed 某件事就去做，是不好的，也是 low-status 的。

这很困难，也令人不堪重负，因为 agendas _非常多_，而且许多聪明人持有不同、甚至有些 contradictory 的 views。所以，这感觉基本不可能做到。但我_同时_又觉得，自己必须在真正开始任何 permanent research positions 之前（也就是毕业之前）solve 这件事，以免搞砸，去做了某个 sub-optimal 的事情。因此，我必须 solve 这个 empirically 看来大多数聪明人肯定都没搞对的 problem，而且还要在毕业之前全部完成。这看起来基本不可能，也在探索 AI Safety 这件事周围形成了一个巨大的 [ugh field](https://medium.com/@robertwiblin/ugh-fields-or-why-you-can-t-even-bear-to-think-about-that-task-5941837dac62)。而这件事本来就已经相当让人抗拒，因为它涉及 re-skilling，要在 PhDs 和直接进入 industry 等一系列不同道路之间作出选择，而且总体上没有一条清晰的进入路径。

### 我的经历

那么，我实际上经历了什么？我在 undergrad 的最后一年开始认真看待 AI Safety。那时，我接受了支持 AI Safety 的 heuristic arguments（比如，比我们更聪明的东西很吓人），但对于在这个 field 工作究竟是什么样子，我的了解基本只有：“我猜 MIRI 的人在 prove theorems，而且我知道顶尖 AI labs 里有人在做 safety 方面的事情？”我开始和许多在这个 field 工作的人交谈，逐渐获得了关于实际情况的 data。这一切都相当令人困惑、压力很大，而且还要与进入 quant finance 这条道路竞争——后者是一条安全、轻松的默认道路，我已经知道自己会喜欢它。

毕业之后，我意识到自己拥有的灵活性比原先想象的多得多。我拿出了一年的时间，设法争取到了三段接连进行的 AI Safety internships。一个重大的 update 是，我可以在不承担太多风险的情况下探索 AI Safety——如果不顺利，一两年后我随时可以回到 finance。我分别在 FHI、DeepMind 和 CHAI interned，对应从事 mathematical/theoretical safety work、基于 empirical ML 且涉及 fairness 和 bias 的工作，以及 empirical interpretability work。我还参加了 AGI Fundamentals course，并与自己工作过的各个 orgs 以及 conference 上的许多人交谈。我试着向遇到的每位 researchers 询问他们的 theory of change，也就是他们的 research 究竟如何产生意义。有一件事对我帮助很大：我和 OpenAI 的一位 researcher 聊天，他说自己刚开始时也没有清晰的 inside views。但随着时间推移，他相当自然而然地 form 了这些 inside views，只要花时间思考，并置身于 professional research environment 中，就足够了。

那一年结束时，我拿到了几个 offers，最后加入了 Anthropic，与 Chris Olah 一起从事 interpretability。我不确定这是不是最佳选择，但我对 interpretability 非常兴奋，而且它看起来是最好的 bet。几个月之后，很明显，这是一个_非常棒_的决定，我也对这份工作非常兴奋。不过，即使我认定这份工作并不太有用，或者并不适合自己，也不会是世界末日；我预计，自己完全可以在几个月内离开，而不伤和气。随着我开展 research，并与 Chris + 这里的其他人交谈，我开始对 interpretability 的实际情况，以及它和 Anthropic 工作的 theory of impact，form 更清晰的 views。不过，我的理解仍有很大的空缺，有些地方我依然困惑，或者仍在对他人 deferring。这也没关系！我不认为这在很大程度上妨碍了我在 short-term 产生 impact，而且随着时间推移，我正在 forming 更清晰的 views。

## 我对思考和形成 Inside Views 的建议

### 为什么要形成它们？

我认为，重视形成 inside views 主要有四个理由：

-   **Truth-tracking（追踪真相）** \- 产生 impact 很难！拥有 true beliefs 非常重要，而找到它们的最佳方式，就是努力形成自己的 views，并确保它们与 truth 之间 correlate。如果你信任了错误的人，就很容易在 deferring 上出错。

    -   这个理由并没有让我很信服——找到比我聪明、对每个 problem 思考的时间都比我长的人，然后相信他们所相信的东西，似乎并不那么难。尤其是当我对多个聪明人的 beliefs 进行 average 时

        -   例如，我对 biosecurity 没有思考太多，但在这个话题上，我很乐意 defer 给 Greg Lewis 这样的人！

-   **Ensuring good community epistemic health（确保社群拥有良好的认知健康状况）** \- 也许你个人的 inside view 在 track the truth 方面不如最好的 researchers。但它们并不是 perfectly correlated！如果你努力独自寻找 truth，你可能会注意到其他人遗漏的 ideas，能够对流行的 arguments 进行 poke holes，等等。而这会让整个 community 的状况更好

    -   这个理由相当站得住脚，但似乎也没有_那么_了不得。就是说，重要，当然重要，但我最多也不会为它投入超过 5% 的精力

    -   避免 information cascades 似乎尤其重要：我因为 Alice 认为某件事重要而去做它，接着 Bob 本来对 Alice 一个人的判断有些怀疑，但他观察到 Alice _和_我都认为它重要，于是更加努力地做这件事，然后 Charlie 又看到了我、Alice 和 Bob，等等。这是我努力 distinguish 以下两者的一个主要原因：我在 all things considered 时相信什么（包括其他人的 views），以及我凭 by my own lights 相信什么（依据自己的 intuitions + 关于世界的 models）

-   **Motivation（动机）** \- 做自己并不相信的事情，真的很难！

    -   我个人容易想得太多，这个理由对我非常重要！但人与人不同——这更多是关于 personal psychology 的事实，而不是关于如何产生 impact 的 abstract statement

-   **Research quality（研究质量）** \- 做好 research，需要对 research 为什么重要、实际究竟发生了什么拥有良好的 intuitions 和 research taste，有时这也被称为 inside view。这个 conceptual framework 会 guides 你作为 researcher 每天作出的许多细小 decisions 和 trade-offs

    -   我认为这非常重要，但值得将它与“这个 research agenda 最终是否 useful”区分开来。例如，在 pure maths research 中，单就做好 research 而言，这一点依然重要；而在 AI Safety 的某些 areas 中，你可以做出“good research”，却并不实际 reducing x-risk 的 probability。

        -   Toy example：假设世界上有十位优秀的 AI Safety researchers，他们各自相信不同的东西。我的 all-things-considered view 应该给每个人的 view 赋予 10% 的 credence。但如果我 randomly pick 一个人，完全 adopt 他的 views，再投入他的 research agenda，我就能完成_多得多_的 research。因此，即使只有一位 researcher 是 correct，后一种 strategy 的 expected value 也要好得多。

    -   这是 mentorship 如此关键的主要原因之一。因为随时可以向 Chris 请教建议、intuitions 和 direction，我已经成为了一个 effective 得_多_的 interpretability researcher。而我最优先的事情之一，就是尽可能多地吸收他的 conceptual frameworks

        -   更一般地说，在我看来，research mentor 的意义在于将自己的 conceptual frameworks 借给你，指导你如何作出正确的 decisions 和 trade-offs。你通过 supervised learning 慢慢吸收他们的 frameworks，并随着自己作为 researcher 的成长，在这些 frameworks 上继续 build on 和 add to

这些理由相当不同，弄清楚自己在意的是哪些理由非常重要！就我个人而言，我主要在意的是 motivation > research quality = impact >> community epistemics

### 如何形成它们？

-   **与人交流！尝试吸收_他们的_ inside views，并将其变成你自己的**

    -   重要的是，目标不是 defer 给他们，而是理解他们相信什么，_以及为什么_。

    -   我为此使用的主要方法，是提出很多问题，然后进行 **paraphrase**——用自己的话向对方 summarise 我的理解，再问哪里不对，或我遗漏了什么。

        -   我通常会问：“那么，具体来说，你的 research direction 为什么能够 reduce existential risk from AI？”

        -   或者：“你与其他 researchers 最大的 disagree 在哪些方面？”或者：“你为什么不做 X？”

    -   我真的、真的很喜欢 paraphrasing！它很棒的几个理由是：

        -   它迫使你在当下 actively listen 和 process

        -   correct 比 teach 容易得多——对方很容易在你的 paraphrase 中 identity issues 并 correct 它们

        -   它让我清楚地看见自己是否 confused、是否没有理解某件事，或者是否在某些 points 上 deferring——把 confused 的话说出来，会让人很尴尬！

        -   一旦这套方法奏效，我就已经将他们的 mental model 下载进了自己的脑海，并可以摆弄它

        -   一旦你下载了多个人的 models，就可以 compare 它们，看看它们如何 differ，等等

    -   一种变体——关注 **cruxes**，也就是那些 key claims：如果他们对这些 claims 改变了想法，就会改变自己要做什么的想法。

        -   这非常重要——有些人做某个 direction，是因为他们认为它最重要；另一些人做它，则是因为，例如，它是一个很好的 personal fit，或者他们觉得它有趣。这应该是_完全不同_的谈话

    -   一种变体——写一份 google doc 来 summarising 一次谈话，之后发给对方，请他们评论。如果你觉得当场 summarise 很难，这种方式可能效果很好，还能产出一份适合发布或分享的好材料——我很希望更多人和我这样做

-   **你有权 disagree**（即使对方非常酷，而且 high-status）

    -   这对我来说是一个很大的 update！一个人聪明、能干，只意味着他们更经常是对的，而不是他们总是对的

    -   降低自己提出傻问题的门槛，真的很有帮助——如果你对所有可能出错的地方都追问一番，那么 90% 的时候对方是对的，你能学到东西；而 10% 的时候，是他们遗漏了什么

    -   例如，我过去做过一些 research，事后回看，我认为它们并没有特别 useful。而这完全没问题！

    -   Empirically，有很多聪明人相信不同、甚至 contradictory 的东西！他们不可能全都对，所以你_必须_与其中一些人 disagree。将自己可以这样做这件事 Internalising，对能够 think clearly 非常重要

-   **不要当隐士** \- 你是通过走进现实世界、实际做事来形成 inside view 的，而不只是靠躲起来苦思冥想

    -   例如，直接试着做 research！花 10 hours 探索某件事，写一篇 blog post，经历失败、成功，听取 criticism，看看自己学到了什么，再作出 updates

    -   多和人交流！

    -   过好自己的生活，看看会发生什么——随着时间推移，我的想法自然会改变很多

    -   花_一些_时间阅读和思考是有价值的，但如果你只做这些，我认为那就是个错误

-   **从 first principles 出发思考**（有时如此）

    -   Concrete exercise：打开一份空白 google doc，设一个一小时的计时器，然后开始写下你的 case，说明为什么 AI Safety 是最值得着手的 problem。用足这一小时；如果你写不下去了，就回过头检查，追问每一处让你觉得 confusing、不对劲或可疑的地方。写出所有你能想到的 counter-arguments，再重复这个过程

    -   这当然不是你应该做的_全部_事情，但我认为，对于任何 confusing 的事情，这都是一个非常 useful 的 exercise！

-   **不要只是一味加倍努力** \- 我有一种 failure mode，称为[按下 Try Harder 按钮](https://www.neelnanda.io/blog/mini-blog-post-6-stop-pressing-the-try-harder-button)：我把某件事标记为重要，然后就试图把大量意志力和紧迫感倾注到它身上。不要这样做！这需要很长时间，而当你思考、与人交流、做 research 时，很多事情会自然发生。

    -   如果你觉得这让你压力很大，我允许你放松一下，暂时不把它当作优先事项！

    -   我发现，随着时间推移，我的 inside views 会相当自然地发展很多

-   **Inside vs outside views 是一个 spectrum** \- 独立思考与 deferring 之间没有清晰的 division。形成 inside views 是从 deferring 开始的，然后随着时间推移，慢慢形成越来越 detailed 的 models，说明我在哪些地方 deferring，以及为什么

    -   我的 views 相当自然地从一些 naive stories，比如“AGI 似乎很可怕，因为 intelligence 很重要，而且聪明人认为这件事重要”，发展成更加 detailed 的 stories，比如“我认为 AGI 可怕的一个原因是 inner misalignment。因为 neural networks 的 base optimiser 是 stochastic gradient descent，network 最后可能成为一个拥有不同 mesa-objective 的 mesa-optimiser。而这可能 create 一个追求 power seeking 的 instrumental incentive”。后一个 story 要 detailed 得多，但仍然包含大量 implicit deferring——例如，我们究竟会不会得到 AGI，它是否会通过 deep learning 实现，mesa-optimisers 究竟是否存在，是否存在促使 power seeking 的 instrumental incentive，等等。但像这样 expanding concepts 的 tree，正是进步的样子！

    -   又或者，从“我应该做 AI，因为 AGI 终究会发生——既然自然界做到了，我们也能做到”，发展到“AGI 是 compute constrained。使用 bioanchors method 与 human brain 的 size 建立 link，会得到针对 human-level AI 的 30-ish year AI timelines。我相信 AGI 是 compute constrained，是因为一些关于 empirical trends 的 heuristic arguments，也因为很多聪明人相信这一点”

    -   走到这一步的过程，就像把其他人的 gears level models 下载进自己的脑海，然后慢慢 combining 它们，deleting 你 disagree 的 parts，adding 你自己的 ideas，等等

### 杂项

-   **要 Defer intelligently** \- 不要仅仅因为某人 charismatic、high status 或 well-credentialed，就把他们的 opinions 当作自己的。想想你_为什么_认为他们的 opinions 比你自己的更能 track the truth，以及你愿意在哪些 areas 对他们 defer。弄清楚他们对这件事思考得有多深入，以及他们是否认真对待过这个 belief

    -   一个关键 question 是，他们从世界中获得了多少 feedback——如果他们错了，他们会知道吗？我认为，有些 fields 在这方面比其他 fields 好得多——相比于例如与大多数 algebraic geometers 意见不同，我更能坦然地与许多 moral philosophy professors 意见不同，并做一个坚定的 consequentialist。据我所知，Mathematicians 能以 moral philosophy 所没有的方式，就他们的 proofs 是否 work 获得 feedback

    -   而且要 domain specific——对于 mathematical facts，我会 defer 于一位 Cambridge maths professor，但对于‘如何最好地向 undergraduates teach maths’这样的话题，我不会——他们显然没有做过足够的 experimentation，无法判断自己是否错过了好得多的 methods

-   **没有 inside view，你也可以行动**

    -   Forming 一个好的 inside view 需要很长时间！过去一年多里，我一直在做 full-time safety research，但我仍然非常困惑

        -   一个 analogy——PhD 本质上是一个 training program，让人们对某个特定 research area 拥有 inside view。而这需要好几年！在我看来，像‘AGI 是否是 existential risk’这样的 question，比大多数 thesis topics 难得多；不投入那么多努力，你就别指望能_真正_理解它

    -   你总可以以后再改变想法并 pivot！根据你当时所知道的，以及未来可能获得的 information，做出最好的 expected value bet

    -   Gathering information 是有 costs 的！有时，对一个 problem 更加苦思冥想其实是 analysis paralysis，直接按你的 best guess 去做反而值得

    -   我认为，长期把大约 10% 的时间花在 high-level thinking、strategy、forming inside views 等事情上是好的——你应该把大量时间花在真正做事上！

        -   不过，在早期面临选择哪条 career path 之类的重大 decisions 时，投入更高 percentage 的时间也没问题。

-   **你并不_必须_ form 一个 inside view** \- Forming 能 track the truth 的 inside views 是_困难的_，而且这是一项 skill。你可能就是不擅长，或者觉得这件事压力太大。这也没关系！仅仅照着比你更 competent 的人的建议去做，不应该是 low-status 或 guilt-inducing 的事

    -   没有清晰的 inside view，你也可以成为出色的 research assistant、ops person、engineer 等——只需找一个你信任的聪明人，解释你的处境，然后照他们认为最好的方式去做

        -   我认为，这不是个好主意的主要原因是 motivational，而不是真正关乎 truth-tracking。至于你在 motivationally 这方面有多在意，则由你自己决定！

        -   一个 analogy：我认为，基本上所有对 agenda 有想法的 AI Safety researchers 都应该 get funded，即使我个人认为他们的 agenda 是胡扯。同样，我希望他们都有足够的 labour，能够在自己的 agenda 上 execute well——选择与你最为 personal fit 的 agenda，然后就 deferring，是在实践中 implement 这一点的好办法。

-   **目标要高，但没达到也没关系** \- _尝试_ forming inside views 是有价值且重要的 practice，但这也相当困难！感到吃力、没有取得多少 progress，也没关系

    -   在我看来，尝试独立思考是很好的 training——它会帮助你更清晰地思考，更不容易受骗，成为更好的 researcher，等等。

    -   Outside view：世界上绝大多数人认为 AI Safety 是无稽之谈，而且为它投入的 resources 很少。这值得认真对待！你不应该在事先没有认真思考的情况下，把自己的一生耗在一个古怪且 controversial 的 idea 上

    -   这是在 motivation 与 truth-tracking 之间进行 trade-off 的好办法——只要我努力独立思考，即使知道自己可能没有很好地 tracking truth，我在 motivationally 这方面也能感觉坦然

        -   在实践中，我努力 form 自己的 views，但在做重大 decisions 时，我会大量 deferring，并 forming 一个 all-things-considered view，我预期它能更好地 track truth

    -   如果你没有在做 full-time research，要对事情 form 清晰的 views 就会难得_多_！你正在尝试做的，确实是一件很难的事

-   **要 Convey mindsets，而不是 inside views** \- 如果你在与别人谈论这些事情，例如在 community building 时，重要的是尝试 convey forming inside views 的 _spirit_ 和 _mindset_，而不只是你实际的 views。尝试 convey 你脑中所有的 gears-level models，但要说明白，它们只是 models！尝试 convey 其他人相信什么。

    -   我努力说明白：哪些 beliefs 是我有信心的，哪些是 controversial 的，哪些 points 上我在 deferring，以及哪些事情是我深入思考过的。我认为，这对于 avoiding information cascades 和 building 一个 healthy community 很重要

    -   与此相关，如果你主要在做 community building，那么对 AI Safety 这样困难的 technical questions 没有 inside views，完全没关系！你的 goal 更多是帮助 community 中的人 form 他们自己对事情的 views——有自己的 views 会有帮助，但并非 essential。

---

## 原文

**_Disclaimer_**_: I work as a researcher at Anthropic, but this post entirely represents my own views, rather than the views of my own employer_

## Introduction

I’ve spent the past two years getting into the field of AI Safety. One important message I heard as I was entering the field was that I needed to “form an inside view about AI Safety”, that I needed to form my own beliefs and think for myself rather than just working on stuff because people smarter than me cared about it. And this was incredibly stressful! I think the way I interpreted this was pretty unhealthy, caused me a lot of paralysing uncertainty and anxiety, and almost caused me to give up on getting into the field. But I feel like I’ve now reached a point I’m comfortable with, and where I somewhat think I have my own inside views on things and understand how to form them. 

In this post, I try to explain the traps I fell into and why, what my journey actually looked like, and my advice for how to think about inside views, now I’ve seen what _not_ to do. This is a complex topic and I think there are a lot of valid perspectives, but hopefully my lens is novel and useful for some people trying to form their own views on confusing topics (AI Safety or otherwise)! (Note: I don’t discuss why I _do_ now think AI Safety is important and worth working on - that’s a topic for a future post!)

## The Message of Inside Views

First, context to be clear about what I mean by **inside views**. As I understand it, this is a pretty fuzzily defined concept, but roughly means “having a clear model and argument in my head, starting from some basic and reasonable beliefs about the world, that get to me to a conclusion like ‘working on AI Safety is important’ without needing to rely on deferring to people”. This feels highly related to the concept of [gears-level models](https://www.lesswrong.com/posts/gvK5QWRLk3H8iqcNy/gears-vs-behavior). This is in comparison to **outside views**, or **deferring** to people, where the main reason I believe something is because smart people I respect believe it. In my opinion, there’s a general vibe in the rationality community that inside views are good and outside views are bad (see Greg Lewis’ [In Defence of Epistemic Modesty](https://forum.effectivealtruism.org/posts/WKPd79PESRGZHQ5GY/in-defence-of-epistemic-modesty) for a good argument for the importance of outside views and deferring!). Note that this is _not_ the Tetlockian sense of the words, used in forecasting, where outside view means ‘look up a base rate’ and inside view means ‘use my human intuition, which is terribly calibrated’, where the standard wisdom is outside view > inside view.

Good examples of this kind of reasoning: Buck Shlegeris’ [My Personal Cruxes for Working on AI Safety](https://forum.effectivealtruism.org/posts/Ayu5im98u8FeMWoBZ/my-personal-cruxes-for-working-on-ai-safety), Richard Ngo’s [AGI Safety from First Principles](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ), Joseph Carlsmith’s report on [Existential Risk from Power-Seeking AI](https://www.lesswrong.com/posts/HduCjmXTBD4xYTegv/draft-report-on-existential-risk-from-power-seeking-ai). Note that, while these are all about the question of ‘is AI Safety a problem at all’, the notion of an inside view also applies well to questions like ‘de-confusion research/reinforcement learning from human feedback/interpretability is the best way to reduce existential risk from AI’, arguing for specific research agendas and directions.

## How _I_ Interpreted the Message of Inside Views

I’m generally a pretty anxious person and bad at dealing with uncertainty, and sadly, this message resulted in a pretty unhealthy dynamic in my head. It felt like I had to figure out for myself the conclusive truth of ‘is AI Safety a real problem worth working on’ _and_ which research directions were and were not useful, so I could then work on the optimal one. And that it was my responsibility to do this all myself, that it was bad and low-status to work on something because smart people endorsed it.

This was hard and overwhelming because there are a _lot_ of agendas, and a lot of smart people with different and somewhat contradictory views. So this felt basically impossible. But it _also_ felt like I had to solve this before I actually started any permanent research positions (ie by the time I graduated) in case I screwed up and worked on something sub-optimal. And thus, I had to solve this problem that empirically most smart people must be screwing up, and do it all before I graduated. This seemed basically impossible, and created a big [ugh field](https://medium.com/@robertwiblin/ugh-fields-or-why-you-can-t-even-bear-to-think-about-that-task-5941837dac62) around exploring AI Safety. Which was already pretty aversive, because it involved re-skilling, deciding between a range of different paths like PhDs vs going straight into industry, and generally didn’t have a clean path into it. 

### My Journey

So, what actually happened to me? I started taking AI Safety seriously in my final year of undergrad. At the time, I bought the heuristic arguments for AI Safety (like, something smarter than us is scary), but didn’t really know what working in the field looked like beyond ‘people at MIRI prove theorems I guess, and I know there are people at top AI labs doing safety stuff?’ I started talking to lots of people who worked in the field, and gradually got data on what was going on. This was all pretty confusing and stressful, and was competing with going into quant finance - a safe, easy, default path that I already knew I’d enjoy.

After graduating, I realised I had a lot more flexibility than I thought. I took a year out, and managed to finagle my way into doing three back-to-back AI Safety internships. The big update was that I could explore AI Safety without risking too much - I could always go back into finance in a year or two if it didn’t work out. I interned at FHI, DeepMind and CHAI - working on mathematical/theoretical safety work, empirical ML based stuff to do with fairness and bias, and working on empirical interpretability work respectively. I also did the AGI Fundamentals course, and chatted to a lot of people at the various orgs I worked at and at conference. I tried to ask all the researchers I met about their theory of change for how their research actually matters. One thing that really helped me was chatting to a researcher at OpenAI who said that, when he started, he didn’t have clear inside views. But that he’d formed them fairly organically over time, and just spending time thinking and being in a professional research environment was enough.

At the end of the year, I had several offers and ended up joining Anthropic to work on interpretability with Chris Olah. I wasn’t sure this was the best option, but I was really excited about interpretability, and it seemed like the best bet. A few months in, this was clearly a _great_ decision and I’m really excited about the work, but it wouldn’t have been the end of the world if I’d decided the work wasn’t very useful or a bad fit, and I expect I could have left within a few months without hard feelings. As I’ve done research and talked to Chris + other people here, I’ve started to form clearer views on what’s going on with interpretability and the theory of impact for it and Anthropic’s work, but there’s still big holes in my understanding where I’m confused or deferring to people. And this is fine! I don’t think it’s majorly holding me back from having an impact in the short-term, and I’m forming clearer views with time.

## My Advice for Thinking About & Forming Inside Views

### Why to form them?

I think there are four main reasons to care about forming inside views:

-   **Truth-tracking** \- having an impact is hard! It’s really important to have true beliefs, and the best way to find them is by trying hard to form your own views and ensuring they correlate with truth. It’s easy to get deferring wrong if you trust the wrong people.

    -   I’m pretty unconvinced by this one - it doesn’t seem that hard to find people smarter than me, who’ve thought about each problem for longer than I have, and just believing whatever they believe. Especially if I average multiple smart people’s beliefs

        -   Eg, I haven’t thought too much about biosecurity, but will happily defer to people like Greg Lewis on the topic!

-   **Ensuring good community epistemic health** \- Maybe your personal inside view will track the truth less well than the best researchers. But it’s not perfectly correlated! If you try hard to find the truth on your own, you might notice ideas other people are missing, can poke holes in popular arguments, etc. And this will make the community as a whole better off

    -   This one is pretty legit, but doesn’t seem _that_ big a deal. Like, important, sure, but not something I’d dedicate more than 5% of my effort towards max

    -   It seems particularly important to avoid information cascades where I work on something because Alice thinks it matters, and then Bob is a bit skeptical of Alice alone but observes that both Alice _and_ I believe it matters, and works on it even harder, Charlie sees me, Alice and Bob, etc. This is a main reason I try hard to distinguish between what I believe all things considered (including other people’s views) and what I believe by my own lights (according to my own intuitions + models of the world)

-   **Motivation** \- It’s really hard to work on something you don’t believe in! 

    -   I personally overthink things, and this one is really important to me! But people vary - this is much more a fact about personal psychology than an abstract statement about how to have an impact

-   **Research quality** \- Doing good research involves having good intuitions and research taste, sometimes called an inside view, about why the research matters and what’s really going on. This conceptual framework guides the many small decisions and trade-offs you make on a daily basis as a researcher

    -   I think this is really important, but it’s worth distinguishing this from ‘is this research agenda ultimately useful’. This is still important in eg pure maths research just for doing good research, and there are areas of AI Safety where you can do ‘good research’ without actually reducing the probability of x-risk. 

        -   Toy example: Let’s say there are ten good AI Safety researchers in the world, who all believe different things. My all-things-considered view should put 10% credence on each person’s view. But I’ll get _much_ more research done if I randomly pick one person and fully adopt their views and dive into their research agenda. So, even if only one researcher is correct, the latter strategy is much better in expected value.

    -   This is one of the main reasons that mentorship is so key. I have become a _way_ more effective interpretability researcher by having ready access to Chris to ask for advice, intuitions and direction. And one of my top priorities is absorbing as many of his conceptual frameworks as I can

        -   More generally, IMO the point of a research mentor is to lend you their conceptual frameworks to advise you on how to make the right decisions and trade-offs. And you slowly absorb their frameworks by supervised learning, and build on and add to them as you grow as a researcher

These are pretty different, and it’s really important to be clear about which reasons you care about! Personally, I mostly care about motivation > research quality = impact >> community epistemics 

### How to form them?

-   **Talk to people! Try to absorb _their_ inside views, and make it your own**

    -   Importantly, the goal is not to defer to them, it’s to understand what they believe _and why_.

    -   My main tool for this is to ask lots of questions, and then **paraphrase** - summarise back my understanding in my own words, and ask what’s wrong or what I’m missing.

        -   My default question is ‘so, why, concretely, does your research direction reduce existential risk from AI?’

        -   Or, ‘what are the biggest ways you disagree with other researchers?’ Or ‘why aren’t you working on X?’

    -   I really, really love paraphrasing! A few reasons it’s great:

        -   It forces you to actively listen and process in the moment

        -   It’s much easier to correct than teach - the other person can easily identity issues in your paraphrase and correct them

        -   It makes it obvious to myself if I’m confused or don’t understand something, or if I’m deferring on any points - it’s awkward to say things that are confused!

        -   Once I get it working, I have now downloaded their mental model into my head and can play around with it

        -   Once you’ve downloaded multiple people’s models, you can compare them, see how they differ, etc

    -   A variant - focus on **cruxes**, key claims where if they changed their mind on that they’d change their mind about what to work on.

        -   This is really important - some people work on a direction because they think it’s the most important, other people work on it because eg it’s a good personal fit or they find it fun. These should be _completely different_ conversations

    -   A variant - write a google doc summarising a conversation and send it to them afterwards for comments. This can work great if you find it hard to summarise in the moment, and can produce a good artefact to publish or share - I’d love it if people did this more with me

-   **You have permission to disagree** (even with really cool and high-status people)

    -   This was a big update for me! Someone being smart and competent just means they’re right more often, not that they’re always right

    -   It really helps to have a low bar for asking dumb questions - if you poke at everything that might be wrong, 90% of the time they’re right and you learn something, and 10% of the time they missed something

    -   For example, I’ve done research in the past that, in hindsight, I don’t think was particularly useful. And this is totally fine! 

    -   Empirically, there’s a lot of smart people who believe different and contradictory things! It’s impossible for all of them to be right, so you _must_ disagree with some of them. Internalising that you can do this is really important for being able to think clearly

-   **Don’t be a monk** \- you form an inside view by going out in the world and doing things - not just by hiding away and thinking really hard

    -   Eg, just try doing research! Spend 10 hours pursuing something, write up a blog post, fail, succeed, hear criticism, see what you learn and make updates

    -   Talk to lots of people!

    -   Live your life, and see what happens - my thoughts naturally change a lot over time

    -   It’s valuable to spend _some_ time reading and thinking, but if this is all you do I think that’s a mistake

-   **Think from first principles** (sometimes)

    -   Concrete exercise: Open a blank google doc, set a one hour timer, and start writing out your case for why AI Safety is the most important problem to work on. Spend the full hour on this, and if you run out of steam, go back through and poke at everything that feels confusing, or off, or dodgy. Write out all the counter-arguments you can think of, and repeat

    -   This definitely isn’t _all_ you should do, but I think this is a really useful exercise for anything confusing!

-   **Don’t just try harder** \- I have a failure mode I call [pushing the Try Harder button](https://www.neelnanda.io/blog/mini-blog-post-6-stop-pressing-the-try-harder-button) where I label something as important and just try to channel a lot of willpower and urgency towards it. Don’t do that! This takes a long time, and a lot will happen naturally as you think, talk to people, and do research. 

    -   If you find this really stressful, you have my permission to chill and not make it a priority for a while!

    -   I’ve found my inside views develop a lot over time, fairly organically

-   **Inside vs outside views is a spectrum** \- there’s no clear division between thinking for yourself and deferring. Forming inside views starts out by deferring, and then slowly forming more and more detailed models of where I’m deferring and why over time

    -   My views have gone fairly organically from naive stories like ‘AGI seems scary because intelligence is important and smart people think this matters’ to more detailed ones like ‘I think one reason AGI is scary is inner misalignment. Because neural networks have the base optimiser of stochastic gradient descent, the network may end up as a mesa-optimiser with a different mesa-objective. And this may create an instrumental incentive for power seeking’. The latter story is way more detailed, but still includes a lot of implicit deferring - eg that we’ll get AGI at all, that it’ll be via deep learning, that mesa-optimisers are a thing at all, that there’s an instrumental incentive for power seeking, etc. But expanding the tree of concepts like this is what progress looks like!

    -   Or, ‘I should work on AI because AGI will happen eventually - if nature did it, so can we’ to ‘AGI is compute constrained. Using the bioanchors method to link to the size of the human brain gives 30-ish year AI timelines for human-level AI. I believe AGI is compute constrained because of some heuristic arguments about empirical trends, and because lots of smart people believe this’

    -   Getting here looks like downloading other people’s gears level models into your head, and slowly combining them, deleting parts you disagree with, adding ideas of your own, etc

### Misc

-   **Defer intelligently** \- Don’t just adopt someone’s opinions as your own because they’re charismatic, high status, or well-credentialed. Think about _why_ you think their opinions track the truth better than your own, and in which areas you’re willing to defer to them. Figure out how hard they’ve thought about this, and whether they’ve taken the belief seriously

    -   One key question is how much feedback they get from the world - would they know if they were wrong? I think some fields score much better on this than others - I’m a lot more comfortable disagreeing with many moral philosophy professors and being a committed consequentialist than I am with eg disagreeing with most algebraic geometers. Mathematicians get feedback re whether there proofs work in a way that, as far as I can tell, moral philosophy doesn’t

    -   And be domain specific - I’d defer to a Cambridge maths professor about mathematical facts, but not on a topic like ‘how best to teach maths to undergraduates’ - they clearly haven’t done enough experimentation to tell if they’re missing out on vastly better methods

-   **You can act without an inside view**

    -   Forming a good inside view takes a really long time! I’ve been doing full-time safety research for the past year and a bit and I’m still very confused

        -   An analogy - a PhD is essentially a training program to give people an inside view for a specific research area. And this takes several years! IMO a question like ‘is AGI an existential risk’ is much harder than most thesis topics, and you don’t have a hope of _really_ understanding it without that much work

    -   You can always change your mind and pivot later! Make the best expected value bet given what you know at the time, and what information you might get in future

    -   Gathering information has costs! Sometimes thinking harder about a problem is analysis paralysis, and it’s worth just running with your best guess

    -   I think it’s good to spend maybe 10% of your time long-term on high-level thinking, strategy, forming inside views, etc - a lot of your time should be spent actually doing stuff!

        -   Though it’s OK to spend a higher percentage early on when you have major decisions like what career path to go down.

-   **You don’t _have_ to form an inside view** \- Forming inside views that track the truth is _hard_, and it’s a skill. You might just be bad at it, or find it too stressful. And this is fine! It shouldn’t be low-status or guilt-inducing to just do what people more competent than you recommend

    -   You can be a great research assistant, ops person, engineer etc without having a clear inside view - just find someone smart who you trust, explain your situation, and do what they think is best

        -   I think the main reason this is a bad idea is motivational, not really about truth-tracking. And it’s up to you how much you care about this motivationally!

        -   An analogy: I think basically all AI Safety researchers who have ideas for an agenda should get funded, even if I personally think their agenda is BS. Likewise, I want them all to have enough labour available to execute well on their agenda - picking the agenda you’re the best personal fit for and just deferring is a good way to implement this in practice.

-   **Aim high, but be OK with missing** \- It’s valuable and important practice to _try_ forming inside views, but it’s also pretty hard! It’s OK to struggle and not make much progress

    -   IMO, trying to think for yourself is great training - it’ll help you think more clearly, be harder to con, become a better researcher, etc.

    -   Outside view: The vast majority of the world thinks AI Safety is nonsense, and puts very few resources towards it. This is worth taking seriously! You shouldn’t throw your life away on a weird and controversial idea without thinking seriously about it first

    -   This is a good way to trade-off between motivation and truth-tracking - so long as I try hard to think for myself, I feel OK motivationally, even if I know that I may not be tracking truth well

        -   In practice, I try hard to form my own views, but then make big decisions by deferring a lot and forming an all-things-considered view, which I expect to track truth better

    -   If you aren’t doing full-time research, it’s _much_ harder to form clear views on things! This is a really hard thing you’re trying to do

-   **Convey mindsets, not inside views** \- If you’re talking to someone else about this stuff, eg while community building, it’s important to try to convey the _spirit_ and _mindset_ of forming inside views, more so than your actual views. Try to convey all of the gears-level models in your head, but make it clear that they’re just models! Try to convey what other people believe in.

    -   I try hard to be clear about which beliefs I’m confident in, which are controversial, which points I’m deferring on, and which things I’ve thought hard about. I think this is important for avoiding information cascades, and building a healthy community

    -   Relatedly, if you’re mostly doing community building, it’s totally fine to not have inside views on hard technical questions like AI Safety! Your goal is more to help people in your community form their own views on things - having views of your own is helpful but not essential.