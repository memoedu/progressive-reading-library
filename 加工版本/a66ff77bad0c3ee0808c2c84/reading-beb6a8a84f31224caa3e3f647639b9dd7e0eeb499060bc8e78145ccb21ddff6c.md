# 关于原始 paper 的笔记

---

## 正文

[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

[Michael Nielsen](https://michaelnotebook.com/)

[Astera Institute](https://astera.org/)

2024 年 1 月 13 日

Differential technological development (DTD)[1](https://michaelnotebook.com/dtd/index.html#fn1) 这一 idea 主张，humanity 应着重 reducing dangerous technologies 的 development，并 accelerate safety-enhancing technologies 的 development。这个 term（当然不是这个 concept）源自 Nick Bostrom 在 2002 年的一篇 paper[2](https://michaelnotebook.com/dtd/index.html#fn2)。近年来，在涉及若干 advanced technologies，包括 synthetic biology 和 artificial superintelligence (ASI)[3](https://michaelnotebook.com/dtd/index.html#fn3) 的讨论中，它受到了特别关注。在较早的一组笔记[4](https://michaelnotebook.com/dtd/index.html#fn4)中，我曾勾勒出 Alignment Problem 的一种 reformulation，将其表述为“使 liberal society 的 values 和 institutions（其中至关重要的包括 market）与 differential technology development 相 aligning 的 problem”。这种 alignment 的目的，是让我们获得 technology 的巨大 benefits，同时 greatly reducing existential risk，并 preserving 或 enhancing 我们最珍视的 values。

在这些笔记中，我将：

-   Examine DTD 的 definition，identifying 若干 ambiguities，并思考 resolving 它们的方式。背后的 question 是：DTD 是否是一个对 guiding decision-making 有用的 category？
-   Strengthen 上述 Alignment Problem 的 reformulation，并更充分地解释它与 AI 和 ASI safety 的 connection。
-   关于 DTD，可以做哪些 work？这里远非一份 full survey，只是简短且 _ad hoc_ 地看看我觉得有意思的几个 approaches。

有一个我不会讨论、但很重要的 question：DTD 在多大程度上 different from 较早的 ideas——这些 ideas 来自从 safety engineering 到 sustainability 等诸多 fields？显然，它们存在相当多的 overlap，但我不会在这里 systematically 探讨这个 question。

这些笔记属于一个系列，我在其中梳理自己对 technology 带来的 existential and catastrophic risk 的思考，其中包括（但不限于）ASI 和 biotech。这个系列的其他文章可在[这里](https://michaelnotebook.com/tag/xrisk.html)找到。（坦白说，目前这组笔记尤其深陷细节，主要着眼于 identifying 关键 problems，但在 solving 它们方面进展少得令人沮丧。）截至目前，这些都只是粗略的初步思考。它们主要供我自己使用，不过有几个人很友善地说，这些笔记对他们有所帮助。我希望自己的理解最终能成熟到值得写一篇更精练、论述更充分的 overview；如果真能如此，它也会出现在上面的链接中！

## 关于原始 paper 的笔记

Bostrom 在 2002 年原始 paper 中对 DTD 的 description 简短而有趣，值得完整 quoting 并加以 commenting。我只省略了原文的 inline citations；当然，可以在原始 paper 中找到它们。

> 如果某项 feasible technology 具有 large commercial potential，那么要 prevent 它被 developed，很可能是不可能的。至少在今天这个存在许多 autonomous powers、而 surveillance 又 relatively limited 的世界里，至少对于那些不 rely on rare materials 或 large manufacturing plants 的 technologies，要让一项 ban 做到 100% 滴水不漏，会极其困难。对于某些 technologies（比如 ozone−destroying chemicals），imperfectly enforceable regulation 可能就已足够。但对于其他 technologies，例如在 natural environment 中 self−replicate 的 destructive nanobots，即使只有一次 breach，也可能是 terminal。technological bans 的 limited enforceability restricts 了我们可以选择的 feasible policies 的 set。

在我看来，这一段（及其后续段落）过于轻视 bans on harmful technologies 这种 partial solution。我家的锁挡不住一个铁了心要闯入的窃贼，但装锁仍然大有好处；它们是一种 partial solution。正如这一段正确指出的，Technological bans 当然是 imperfect，但仍值得作为 DTD strategy 的一部分予以考虑。虽然 opposition 并未阻止 nuclear power 的整体发展，但某些 types 的 nuclear technology 尽管具有 large commercial potential，却已被 opposition 阻止（或大幅 delayed），这似乎是 plausible 的。当然，这对 humanity 是好是坏，是另一个 question——我只是把它作为一个 plausible example：一项具有 large commercial potential 的 feasible technology 受到 _de facto_ ban。值得注意的是，这并非通过 direct legislative ban 实现，而是通过 technological development 的整体 slowing 实现——possible technologies 的 space 实际上远没有得到它本来可能获得的那么充分的 explored。换句话说，这种 approach 不是寻求 direct ban，而是利用 opposition，使 developing 某些 technologies 的 _cost_ 足够高，以至于它们实际上不会被 developed。你可能会反驳说，这意味着它们并不具有“large commercial potential”，因而不是这一段 argument 的 counterexample。但从实质上说，它是一个 counterexample：这一点可以重新表述为，commercial potential 远非 technology 所 intrinsic 的东西，而是可能被其他事物，包括 public sentiment，radically affected。实际上，我认为 bans 值得作为 DTD armamentarium 的一部分加以考虑。

Bostrom 在下一段中 defines DTD。我在这里添加了强调，以使这个 definition 更醒目：

> 我们确实有能力 affect 的（能 affect 到什么 extent，取决于我们如何 define “我们”），是各种 technologies 的 rate of development，以及可能还有 feasible technologies 被 developed 和 implemented 的 sequence。我们应当着眼于我想称为 _differential technological development:_ 的事情，即_尝试 retard dangerous_ _technologies 的 implementation，并 accelerate beneficial technologies 的 implementation，_ _尤其是那些 ameliorate 其他 technologies 所 posed 的 hazards 的 technologies。_

我将在下一节回到这个 definition，并 identify 几个重要的 ambiguities。

> 就 nanotechnology 而言，desirable sequence 应当是，在 offensive capabilities 对许多 independent powers 变得 available 之前，就 deployed defense systems；因为一个 secret 或一项 technology 一旦被许多人 shared，就极难 prevent 进一步的 proliferation。

确实如此，不过有时会存在重要的 monopolies，至少在一段时间内 prevent proliferation。当前一个有趣的例子是 ASML 公司，它对 advanced photolithography 拥有 _de facto_ monopoly。US 利用了这一 monopoly，在 2018 年向 ASML 施压，要求它 restrict 对 China 的 sales；类似 restrictions 在 2024 年仍然持续[5](https://michaelnotebook.com/dtd/index.html#fn5)。几位从事 semiconductor manufacturing 的人告诉我，ASML 在这方面的 advantage 极其巨大，而且出人意料地难以 replicate。我并不怀疑它是_可以_被 replicated 的，但 timescale 可能远超十年，甚至可能是几十年。若能更好地理解促成这种 monopoly 的 conditions，会很有意思；几个人都曾信心十足地告诉我“原因”；然而，他们告诉我的是_不同的_原因，这说明事情并没有那么显而易见。

> 就 biotechnology 而言，我们应当寻求 promote 对 vaccines、anti−bacterial 和 anti−viral drugs、protective gear、sensors 和 diagnostics 的 research，并尽可能 delay biological warfare agents 及其 vectors 的 development（和 proliferation）。那些同等 advance offense 和 defense 的 Developments，从 security perspective 看是 neutral 的，除非它们由我们认定为 responsible 的 countries 完成；在这种情况下，只要它们 increase 了我们相对于 potential enemies 的 technological superiority，它们就是 advantageous 的。这些“neutral”的 developments 也有助于 reducing natural hazards 带来的 threat，当然，它们还可能具有与 global security 并不 directly related 的 benefits。

正如 Bostrom 所说，这些“neutral”的 developments 确实具有相当大的 benefits。但它们也 implicitly 站在 technocracy 一边，与 non-technocrats 相对；这本身就是 society _内部_的 redistribution of power。换句话说，DTD 背后的一个 assumption 是，technology 的 cure 是更多的 technology。我的大多数读者大概都是 technocrats，有些人或许不觉得这是个问题[6](https://michaelnotebook.com/dtd/index.html#fn6)。不过，我认为，这种 power 向 technology 的转移，是 DTD 中一个重要却研究不足的 element。在这一点上，支持 DTD 的人，与支持 technology 几乎完全 unimpeded advance 的人非常相似。

下一段颇为引人注目，尤其是考虑到近期（截至 2024 年）关于 ASI 的讨论：

> 有些 technologies 似乎尤其值得 promoting，因为它们能够帮助 reducing 范围广泛的 threats。Superintelligence 就是其中之一。虽然它有自身的 dangers（前面几节已有阐述），但无论如何，我们终究必须面对这些 dangers。不过，尽早获得 superintelligence 是 desirable 的，因为它有助于 diminish 其他 risks。一个 superintelligence 可以在 policy 方面为我们提供建议。Superintelligence 会让 nanotechnology 的 progress curve 变得 much steeper，从而 shortening 从 dangerous nanoreplicators 的 development 到 adequate defenses 的 deployment 之间的 period of vulnerability。$$ _这一点完全不显然——如果 ASI 对 offenses 的帮助多于对 defenses 的帮助，那么_ _ASI 只会使局面更糟_。$$ 相比之下，在 superintelligence 之前获得 nanotechnology，对于 diminish superintelligence 的 risks 几乎没有帮助。主要的 possible exception 是，我们认为通过 uploading 而不是通过 artificial intelligence 达到 superintelligence 很重要。Nanotechnology 将 greatly facilitate uploading。

简而言之，这个 argument 是：关于 ASI 的 work _就是_ DTD，因为它 reduces 来自 nanotech 的 risk！这篇 paper 同时 cited 了 Eric Drexler 和 Bill Joy；acknowledgments 中也感谢了 Joy。我猜今天很少有人会把关于 ASI 的 work 视为 DTD，不过也许我在这一点上错了。我想知道，availability heuristic 在多大程度上 determines 了什么被视为 DTD，什么不被视为 DTD？也就是说，在 2002 年前后，许多人公开担忧 nanotech risk，而担忧 ASI risk 的人较少；今天则恰好相反，因此人们对什么算 DTD 的看法也发生了变化。就我自己而言，我逐渐开始认为，为 DTD 而 developing 和 using ASI 可能是绝对 essential 的，这至少是 plausible 的。这是个令人不安的前景；我想起了 Mario Andretti 的一句话：“如果一切似乎都在掌控之中，那就是你还不够快。”

> 其他具有广泛 risk−reducing potential 的 technologies 包括 intelligence augmentation、information technology 和 surveillance。它们能够使我们 individually 和 collectively 都变得 smarter，也能够使 enforce 必要的 regulation 更加 feasible。因此，有很强的 prima facie case 支持尽可能积极地 pursuing 这些 technologies。如前所述，我们还可以 identify technology 之外、在几乎所有 scenarios 中都是 beneficial 的 developments。Peace 和 international cooperation 显然是值得追求的 goals，培育有助于 democracies prosper 的 traditions 也是如此。

看到 surveillance 被列为一种 risk-reducing technology，颇为耐人寻味，因为许多人将它视为 tyranny 的 tool。我曾在别处简要讨论过 _provably beneficial surveillance_[7](https://michaelnotebook.com/dtd/index.html#fn7) 的可能性，但目前那还只是一种 speculative design possibility。我预计，默认情况下 surveillance 将 increase risk，而不是 decrease 它。Bostrom 至少在 2002 年似乎相信相反的情况。这 underscores 了什么算作 DTD 所存在的 considerable ambiguity。

## DTD 是一个有用的 category 吗？

回顾一下，Bostrom 在 2002 年对 DTD 的 definition 是：尝试 retard dangerous technologies 的 implementation，并 accelerate beneficial technologies 的 implementation，尤其是那些 ameliorate 其他 technologies 所 posed 的 hazards 的 technologies。他在 2019 年关于 Vulnerable World Hypothesis 的 paper[8](https://michaelnotebook.com/dtd/index.html#fn8) 中给出了一个类似、稍为详尽的 definition：

> 对 dangerous 和 harmful technologies 的 development 进行 Retard，尤其是那些 raise existential risk 的 level 的 technologies；并 accelerate beneficial technologies 的 development，尤其是那些 reduce nature 或其他 technologies 所 posed 的 existential risks 的 technologies。

人们经常将这些 definitions 简写为类似这样的说法：优先从事 defensive technologies 而非 offensive 的工作。起初这似乎相当直接，但正如上一节的讨论已经表明的，往往很难说什么算作 DTD，什么不算。一个人眼中的 offensive technology，在另一个人眼中就是 defensive technology。我已经简要提及 ASI。让我再举五个 examples 来 illustrate 这种 ambiguity。你本人不必接受这些 ambiguities；事实上，在讨论下面这样的 examples 时，我常常发现人们拒绝任何关于 ambiguity 的 claim。但关键在于，不同的人出于不同的 reasons 拒绝这种 claim。有些人会说“brain-computer interface (BCI) 显然是 defensive”；另一些人则说“BCI 显然是 offensive”。我的经验是，在见多识广、深思熟虑的人之间，这样的 disagreement _非常多_，这表明有必要探究这类 disagreement 的表面 reasons 之外的东西。

1.  **Nuclear retaliatory second-strike capability:** second-strike capability 是 offensive 还是 defensive？你可能会 argue 说它“显然”是 offensive，因为它 enables 一个 country 对另一个 country wreak devastation；但也可以提出同样有力的 argument，说它“显然”是 defensive，因为 second-strike capability 的 purpose 是 dissuade 其他 countries ever using nukes。事实上，offense 还是 defense 占 preponderant，可能取决于一些 subtleties：拥有 second-strike capability 的 country 的 institutions、它们关于 use of force 的 doctrine、这种 capability 被 repurposed for first strikes 的 ease、它们与其他 countries 的 relationship 的 nature，等等。这完全不是 clear cut 的。
2.  **Missile shield:** Ronald Reagan 提出的“Star Wars” program，旨在为 US 提供抵御 long-range missiles 的 shield。它似乎是一种 defensive technology，因此也是 DTD 的一个 example。但许多 analysts claimed，它实际上会 disrupt mutually assured destruction 的 stability，使 US 对 second strike 的 vulnerable 程度大幅降低；从而更有能力、也更有可能 engage in a first strike；甚至可能因此更容易 provoke Soviet Union 采取出人意料、或许考虑不周的 action。因此，这样的 missile shield 是否是 DTD 的一个 example，绝非显而易见。
3.  **BCI:** 如上所述，我听过有人说 BCI“显然”是一种 DTD，意思是它可能帮助 amplify human cognition，让我们与未来的 ASIs 处于更 equal footing。但 BCI 也可能成为 companies、governments 或 third parties[9](https://michaelnotebook.com/dtd/index.html#fn9) 对 individuals exert influence 甚至 dominance 的一种方式。再说一次：BCI 是否 contributes to risk 的 reduction，存在 considerable ambiguity。
4.  **Vaccines:** 相比上述 examples，Vaccines 或许更“显然”是一种 defensive technology。然而，它们也可以被 used offensively。最明显的是，通过 denying 某个 group access to a vaccine，一种 vaccine 实际上可能成为 technology of offense。虽然并不完全相同，但历史上有若干 instances，据称 invaders 曾 deliberately infected native populations with smallpox，同时依靠 invaders 已经具有的 considerable pre-existing immunity 作为 protection[10](https://michaelnotebook.com/dtd/index.html#fn10)。可以想象，未来的 biowarfare 中会使用类似的 strategies。我倾向于认为，vaccines 的 net out 结果显然是 DTD，但也能想象有人会在 specific instances 中对此提出异议，例如 vaccines 被 developed 为某种 biowarfare strategy 的_一部分_时。
5.  **Geoengineering:** Bostrom 在 2019 年的 paper 中，将它作为一种显然是 offensive(!) technology 的 example，建议说：“不要 develop 那些会 empower random individuals 去 unilaterally make drastic alterations to the Earth’s climate 的 geoengineering forms”。我怀疑这至少会让一些人感到意外，因为他们将 geoengineering 视为 technologies 的 arsenal 的一部分，可能有助于 mitigate climate change 的 impact。不过，另一些人则以它过于 risky 为由，argued against geoengineering。Alan Robock 是研究 aerosols 对 stratosphere 的 impact 的 leading expert，他 identified 了许多与 geoengineering associated 的 risks，包括 disrupting the Indian monsoon 的可能性，从而 affecting 世界 population 中相当一部分人的 food security[11](https://michaelnotebook.com/dtd/index.html#fn11)。

这些 concerns 中至少有一些（也许是全部）可能显得是在咬文嚼字、吹毛求疵。我不这样认为：我认为它们是一个 major problem。越来越多的 funding decisions 正在为 DTD 工作提供 fund。如果这些 decisions 无意间 create 的 problems 比它们 ameliorate 的更多，那将是（极其）不幸的。制定良好、有用的 definitions 确实很重要。糟糕的 conceptual foundations 往往 lead to bad outcomes[12](https://michaelnotebook.com/dtd/index.html#fn12)。在我看来，三个 major problems 是：

-   _这个 risk sign problem_：对于某种给定的 technology 究竟 increases 还是 decreases risks，可能存在难以解决的 disagreement。这个 problem 之所以 arises，是因为我们对这类 risks 往往只有 developed 得相对不足的 models，或者它们 depend upon 许多难以预料的 contingencies。
-   _Risk 不是 technology 的 intrinsic property；但 technologies 确实仍然具有某些 natural tendencies of use：_几十年来，好为人师者一直重复着类似这样的陈词滥调：“technology 并没有 good 或 bad 之分，它取决于你如何 use 它”。在某种非常 literal 的意义上，这是 true 的，但 Marshall McLuhan 曾恰如其分地 responded[13](https://michaelnotebook.com/dtd/index.html#fn13)，指出 particular technologies（或者在 McLuhan 的 context 中，media）往往更 naturally 被用于 good 或 bad ends。他把“technology-as-intrinsically neutral”的 point of view 描述为“当前梦游状态的声音。假设我们说…… \`The smallpox virus is in itself neither good nor bad; it is the way it is used that determines its value.' Again, \`Firearms 本身既非 good 也非 bad；是它们被 used 的方式 determines 它们的 value。’……我并不是故意唱反调。\[这类\] statement 中根本没有任何经得起 scrutiny 的东西，因为它 ignores 了 medium 的 nature，也 ignores 了任何及所有 media 的 nature，完全是一副 Narcissus 的模样：一个人被自身存在以一种新的 technical form 所经历的 amputation 和 extension 催眠了。”要使 DTD 成为一个有用的 category，处于这种中间状态是极其不幸的。
-   _对谁 controls 某种 technology 的 Dependence：_一种 technology 是 increases 还是 decreases risk，在很大程度上 depends on 你认为谁在 controlling 这种 technology。如果是你的朋友或 allies，你就更有可能认为它在 decreasing risk。不过，从足够长的时间来看，technology 很可能会 spread widely，假定只有你和你的 allies 会拥有它，是不明智的。

这些 issues 让我不禁思考：DTD 的 defined 程度，是否足以让它成为一个有用的 category？当我们谈论当前的 technologies 时，一种 technology 是否 increases 或 decreases risk，往往相当清楚。例如，广泛 investment 于 flame retardant building materials，或 aircraft safety engineering 的 improvements，显然都是 DTD 的 examples。然而，引入 DTD 这个 term 的 purpose，是帮助思考 investing in particular new technologies 的广泛 long-run effects。在那些情况下，什么算作 DTD、什么不算，往往是一个非常复杂的 question。人很容易两手一摊，说：“有这么多 contingencies，尝试这样的 long-rang foresight 太愚蠢了。不如采用 just-in-time approach，responding to 迫在眉睫的 problems，或我们拥有真正有力的 models 的 problems”。

在这里，我没有有力、令人满意的 conclusion。但我确实有两个 partial conclusions。第一，我应该花些时间寻找其他 abstract categories，它们或许能实现当初引入 DTD 所要实现的许多相同 purposes，同时不受这些 shortcomings 中某些问题的困扰。我在这方面取得了一些进展，但目前还过于纠缠细节，不宜在此报告。第二，是对 DTD 的使用保持一种总体上的谨慎。某一 line of work 是否属于 DTD，需要被 regularly re-evaluated。这需要来自 diverse sources 的 independent evaluations。值得思考的是，prediction markets 这样的 tools 是否能有助于对未来 risks 进行 evaluation。此外，也应该直接开展更多 in-depth historical surveys，弄清哪些 technologies 带来了令人意外的 positive 或 negative consequences。

## 重新 Reformulating“那个”Alignment Problem

正如 introduction 中提到的，在我关于 Vulnerable World Hypothesis 的笔记中，我曾将 Alignment Problem 粗略地 reformulation 为“将 liberal society 的 values 和 institutions（其中至关重要地包括 market）与 differential technology development 进行 aligning 的 problem”。我想根据上面的讨论重新审视这个 reformulation。我会做一些小幅改进，然后考虑它与 AI alignment 的 conventional formulation 之间的 relationship。虽然这个 reformulation 看起来与 AI 中 alignment 的 standard formulations 相当不同，但正如我下面将解释的，它有一些显著的 advantages：(1) 它 automatically entails standard AI formulation 的 solution；(2) 它 _也_ addresses standard AI formulation 未 addresses 的一些关键 problems；(3) 它通过更 correctly identifying underlying causal factors 来做到这一点。以下是几点 observations：

1.  我已经强调了 deciding 某项 technology 是否 contributes to DTD 的 challenge。一种可能的回应是，将 Alignment 重新 reframe 为“对 liberal society 的 values 和 institutions（其中至关重要地包括 market）进行 aligning，以 ensure net risks remain manageable 的 problem”。这样一来，它就变得 vacuous，而这点颇有意思。把 DTD 变成一个 noun——姑且认为它是某些 technologies 而非其他 technologies 的 concrete property——使这一表述更加鲜明，并隐含着这样一个 assertion：当前许多 technological development _并不是_ DTD。
2.  一种更鲜明、更好的 statement 是：“对 liberal society 进行 aligning，使其 strongly prioritizes DTD，同时 preserving liberal values 的 problem”。这隐含地 asserts，许多 liberal societies 并不特别 prioritize DTD，而这是一个有待 solved 的 problem，需要通过 modifying 它们的 institutions 和 values 来解决，但方式必须 preserves fundamental liberal values。
3.  这仍然是一个 weak statement，因为它没有确切说明 prioritization 应该有多 strong，也没有说明如何 assessment 哪些 technologies qualify as DTD。下面我会再谈这些问题。不过，我确实想马上指出，我曾尝试过这样的 variations：“对 liberal society 进行 aligning，使其 absolutely prioritizes \[或 enforces\] DTD，同时 preserving liberal values 的 problem”。这是一个颇能激发思考的 variation 方向。原则上，它应当允许大量 technological experimentation；但在实践中，我怀疑它会逐步滑向一种 absolutely prioritizes safety 的 culture，最终损害自身的 long-run 利益。

让我们考虑它与 AI alignment 的 relationship。Stuart Russell 在 2014 年给出了 AI Alignment Problem 的一个早期 statement[14](https://michaelnotebook.com/dtd/index.html#fn14)，他的表述如下：

> 这个 field 中没有人在呼吁对 basic research 进行 regulation；考虑到 AI 对 humanity 的 potential benefits，这样做似乎既 infeasible，又 misdirected。正确的回应似乎是改变这个 field 本身的 goals；我们需要 build 的不是 pure intelligence，而是 provably aligned with human values 的 intelligence。出于 practical reasons，即便对于那些在 human environment 中 operate 的 relatively unintelligent AI systems，我们也需要 solve value alignment problem。如果我们理解到，这个问题是 AI 的 intrinsic part，就像 containment 是 modern nuclear fusion research 的 intrinsic part 一样，那么我们就有理由乐观。世界不一定会走向悲剧。

在这个 formulation 提出后不久，Nate Soares 和 Benya Fallenstein 发表了“Agent Foundations for Aligning Machine Intelligence with Human Interests: A Technical Research Agenda”[15](https://michaelnotebook.com/dtd/index.html#fn15)：

> 我们把 reliably pursues beneficial goals 的 smarter-than-human system 称为“aligned with human interests”，或简称为“aligned.”。要确信某个 agent 以这种方式 aligned，仅仅有一个看似能够应对上述 challenges 的 practical implementation 是不够的。我们还必须对这种 confidence 为什么 justified 获得扎实的 theoretical understanding。这份 technical agenda argues，今天已有可着手开展的 foundational research，能够让未来 develop aligned systems 变得更容易，并 describes 了围绕其中一些 problems 正在开展的工作。

今天许多（并非所有）从事 AI alignment 的人，其 projects 都明显落在这些 formulations 的 scope 之内（或与之紧密相邻）。当然，这些 formulations 存在许多 problems，从事该 problem 的人和 critics 都对此十分了解。其中一大类 problems 涉及这样一种困难：难以用任何 precise form 来 specifying human values 或 human interests，并就其达成一致。当然，对于什么才算 human values 或 human interests，cultures 之间、individuals 之间，甚至（确实如此）individuals 自身内部，都存在相当大的 disagreement。简而言之：ASI 究竟应该 _与什么_ aligned？

第二大类 problems，是我们或许可以称作 stability problem 的问题：即使我们能够为某个特定的 AI system satisfactorily solve Alignment Problem，也不意味着它会对所有 AI systems 都得到 solved。仅仅因为你 construct 的 AI system respects human values，并不意味着你的敌人 construct 的 AI systems 也会 respected 它们。这一点尤其重要，因为今天需要庞大 teams 和 resources 的 frontier AI systems，往往在几年后就会被 resources 有限的小型 teams outperformed。考虑到 military organizations 对 AI 的浓厚兴趣——事实上，许多 pioneering AI work 都由 DARPA 之类的 organizations funded——所有 advanced AI systems 都会 respect human values，似乎不太可能。

现在，我将 Alignment Problem formulation 为“对 liberal society 进行 aligning，使其 strongly prioritizes DTD，同时 preserving liberal values 的 problem”，乍看之下似乎几乎与 AI risk 无关。但它有许多 benefits，而且（如果 correctly formulated）将会：(a) entail AI Alignment Problem 的一个 solution；(b) address AI Alignment Problem 的 conventional formulation 存在的许多 issues；(c) 还会 address AI 之外的 issues，包括 bio risk、climate risk、nuclear risk、security risk，以及我预计会出现的许多目前尚未知晓的 emergent classes of risk。尤其是，successful AI alignment work 就是 DTD 的一个 example。而只要我们的 society prioritizes DTD，它就必须 prioritize AI alignment，并对那些 hurt AI alignment 的 capabilities 进行 slow down 或 pauses。但它还不止于此：如果 stability problem 实际上才是 main threat，那就意味着我们实际上必须 prioritize stability problem，使其优先于其他任何事情，包括（conventional）AI Alignment Problem[16](https://michaelnotebook.com/dtd/index.html#fn16)。当然，如何做到这一点，完全还不清楚！正如前面提到的，我曾在别处基于 provably beneficial surveillance 的 idea，提出过一个或许只是 partial 的 solution。

出于所有这些原因，我开始将这个 formulation 视为 _strong Alignment Problem_。请注意，它仍然存在一些 issues：(1) 对于究竟应当多么 strongly 地 prioritize DTD，它是 ambiguous 的；(2) 它没有 specify，liberal society 的哪些 values 应当是 immutable 的（而不是仅仅碰巧出现在我们 society 中的 values）；(3) 它仍然存在我已在 DTD 中 identified 的 ambiguities。这三点最终都可以（也应当）得到 addressed。尽管如此，我发现这个 reformulation 极有帮助。

## 我们何时把 DTD 做得好；何时把 DTD 做得差；Alignment 与 Safety Loop

有些种类的 DTD，人类确实做得很好，我们的 society 在这些方面已经 achieved alignment。而另一些种类，我们还远未做到。DTD 隐含的 viewpoint 与 conventional market view 存在某种 tension。按照那种 view，新的 technologies 会被 developed，而 _safety 随后会为响应 consumer demand 而被 supplied_。世界上第一款 commercial jet aircraft 是 de Havilland Comet。第一年就有 Three Comet aircraft crashed；虽然 Comet 又勉强维持了一段时间，但它当然从未真正恢复过来。不过，其他 jet manufacturers 密切关注，并迅速学会了制造 safer 得多的 aircraft。同样，从使用 ammonia 的 refrigerators 转向 CFCs，是 market 有效 reduce risk 的一个例子（不过遗憾的是，并未 eliminate risk）。在这些情况下，有一个 strong 且 rapid 的 _safety loop_ 在 operating：当 risks 由 consumer 立即且十分明显地 borne 时，market 就与 safety 相 aligned，而我们的 society 也就把 DTD 做得很好。这就是作为 safety amplifier 的 capitalism。当 danger 像 Comet 的情况那样，立即且触目惊心地 impacts consumer 时，我们在 safety 上做得最好。modern airlines 成为 safety 方面非凡的典范，或许并不令人意外[17](https://michaelnotebook.com/dtd/index.html#fn17)。

相比之下，当没有 consumer 在 bearing 明显的 immediate cost 时，我们的 society 往往缺少足够好的 institutions 来开展 DTD。在这些情况下，markets 可能会 amplify 那些 risk exceeds 其 benefits 的 technology。这会发生在以下情况下：risks 涉及 externalities；long time lines；是 illegible 或 hard to measure 的；它们往往被 diffusely shared，可能 impacting commons；难以针对 harm establish 任何形式的 property rights。具有 economic mindset 的人有时会把这些简化为“externalities”，但这实在是过度的 oversimplification。尽管如此，只要 collective safety 可以被（不完美地）modeled 为一种 public good，我们就会预期它会被 market undersupplied。

我从小就被告知，自己很可能会死于[18](https://michaelnotebook.com/dtd/index.html#fn18)：nuclear war；climate change；acid rain；ozone hole；或其他几种灾祸之一。在很大程度上，这些问题并未（或只是部分地）通过 market mechanisms 得到 addressed。相反，我们不得不 build 专门的 non-market mechanisms 来 close safety loop。Economists 往往青睐 Pigouvian taxes——而且可以设想，用 general safety taxes 来 address Alignment Problem，使 market 与 DTD 相 aligning[19](https://michaelnotebook.com/dtd/index.html#fn19)——但迄今许多实际的 solutions 都来自其他途径，往往是 governmental 与 open-source-bazaar style 的某种 hybrid[20](https://michaelnotebook.com/dtd/index.html#fn20)。一个 society 最多地 amplifies 什么，就会得到什么。就像 evolution-by-natural-selection 一样，这似乎是 near-tautological 的，却极其重要。即便我们 amplify 一种事物与另一种事物的程度只有微小差异，也会产生重大影响，而我们许多最重要的 institutions 都是 amplifiers。Science 会 tests 并 amplifies 那些帮助我们更好地理解世界的 ideas。Capitalism 会 tests 并 amplifies 那些制造人们所需之物的 companies。Media 会 amplifies 人们觉得有趣的东西。我们怎样才能更好地 amplify differential technological development？

## 我们已经开始 solving Alignment Problem：Great Stagnation 实际上是 Great Realignment 的开端

economists 已经广泛注意到，worldwide economic growth 在 1970 年前后似乎改变了性质。Tyler Cowen 将这一现象称为“Great Stagnation”，这个说法令人难忘[21](https://michaelnotebook.com/dtd/index.html#fn21)。Great Stagnation reflected 在许多方面，包括 global total factor productivity (TFP) growth 大幅 reduced；economists 常将其 interpreted 为 technological innovation 的 rate 正在 slowing down。Great Stagnation 的原因仍有争论。Robert Gordon\[fn:Gordon\] 曾提出，一个重要的 contributor 是，最重要的 platform technologies 只能被 invented 一次，而其中大多数恰好是在 1870 至 1970 这段时期被 invented（或变得 widespread）的。这包括 internal combustion engine、refrigeration、aeroplanes 等。

另一个可能的 contributor，是那些确实会 slow down innovation 的 institutions 和 norms 逐渐兴起。lead-in-gasoline、Asbestos、DDT（不要与 DTD 混淆！）和 CFCs 等事物，起初都作为神奇的新 technologies 被 introduced，受到的 pushback 或 oversight 相对很少。如今，许多 domains 都有难得多的 regulatory gauntlets 需要闯过。这或许以 US FDA 和 EPA 的情况最为出名，但世界各地、多个 domains 都有体现。下面列出几个关键事件——这里我采取了以美国为中心的视角，因为它在过去 80 年里一直是占主导地位的世界强国；不过，有趣的是，其中许多事件在其他不少国家都有（大致同时期的）对应事件：

-   1960 年，thalidomide 被 rejection 于 US market 之外。当时 FDA 只有 11 人在 reviewing drugs
-   Cuban Missile Crisis（古巴导弹危机）(1962)
-   US Clean Air Act（美国清洁空气法）(1963)，以及后来 US EPA 的成立 (1970)
-   Nuclear Nonproliferation Treaty（核不扩散条约）(1970)
-   1972 年对 DDT 的 banning
-   1973 年的 OPEC oil crisis
-   对 asbestos 的 banning：US EPA 于 1973 年开始 impose restrictions。如今它已被 near-fully eliminated
-   对 lead-in-gasoline 的 banning：US EPA 于 1973 年开始 impose restrictions，并于 1996 年在美国实现 full elimination。
-   对 CFCs 的 banning，主要通过 1987 年的 Montreal Protocol 实现，这是第一个由所有国家 signed 的 treaty（其 predecessor Vienna Convention 也一样；后者始于 1985 年，针对同一问题，但没有那么 comprehensive）

Great Stagnation 实际上可能是 Great Realignment 的 initial stage[22](https://michaelnotebook.com/dtd/index.html#fn22)，在这一过程中，我们逐渐走向 solving Alignment Problem。我们可能会 trade off 一定程度的 growth，换取更好地 dealing with risk 的 institutions。极其粗略地说，我们或许可以认为，直到 1970 年前后，society 对 safety 的总体态度都相当漫不经心。此后，我们逐渐越来越多地走向一种 managing safety 与 technological innovation 之间的 tradeoffs 的局面[23](https://michaelnotebook.com/dtd/index.html#fn23)。

人们很容易把这些 processes 看作彼此对立。有时，那些把“safety”视为敌人的人，天真地呼吁“accelerate”，便体现了这一点；另一些人似乎轻视 acceleration 的任何 benefit，把 technological progress 视为敌人，天真地呼吁“safety”，也同样如此。这里_确实有_真正的 tension，但理想情况下，它应当是 positive sum 的；关键是找到 managing 这种 tension 的方法，从而从两者中获得 maximal benefit。我相信，我们尚未找到 managing 这种 tension 的最佳 institutions。

## 任何 posthuman successor 也都需要 solve 这个 Alignment Problem：我们无法 evade 这个 Alignment Problem

对于 AI safety problem，一种有时被提出的“solution”是放弃 human chauvinism，并为 posthumanity 的 emergence，甚至可能是最终的 dominance 而欢呼。“我个人倒是欢迎我们新的 Posthuman Overlords”是一种常见的半开玩笑的说法；“欢迎我们新的 ASI Overlords”这个变体也是如此。Larry Page 和 Juergen Schmidhuber 等人都曾表达过这类立场。例如，_The New York Times_ 在这里[讲述了](https://www.nytimes.com/2023/12/03/technology/ai-openai-musk-page-altman.html) Larry Page 与 Elon Musk 的一次对话，其中 Page 表达了这样的观点：favoring humanity over AI 是“specieist”：

> 他 \[Page\] 说，Humans 最终会 merge with artificially intelligent machines。总有一天，会有许多种 intelligence competing for resources，而最好的会 win。
> 
> Musk 先生说，如果那样的事情发生，我们就完了。那些 machines 会 destroy humanity。
> 
> Page 先生带着沮丧，声音沙哑地坚持说，应该追求他的 utopia。最后，他称 Musk 先生为“specieist”，也就是一个 favors humans over 未来的 digital life-forms 的人。

我怀疑，有些说这类话的人主要是为了显得离经叛道。我也怀疑，有些人确实相信这些说法，有时还相当坚定；事实上，我对这一观点的某些部分也有一些认同[24](https://michaelnotebook.com/dtd/index.html#fn24)。值得认真对待这个观点，看看它能否站得住脚。

关键在于：**任何 posthuman successors 都需要 solve 这个 Alignment Problem**。也就是说，他们将面对与我们今天所面对的几乎完全相同的 problem。实际上，他们很可能面对一个更具挑战性的版本，因为他们的 environment 中可能存在更多 powerful technologies。这并不确定：也许会有一些极其 powerful 的 safety-enhancing technologies 被 developed，使我们的 posthuman successors 所面对的局面容易得多。但我怀疑，他们很可能会面对比我们今天更大的挑战。总体而言，下面这张出自 [25](https://michaelnotebook.com/dtd/index.html#fn25) 的 heuristic diagram，呈现了这一 fundamental issue 的某些方面：

![](https://michaelnotebook.com/dtd/assets/graph.png)

我觉得这个 heuristic 很有说服力。不过，大量研究表明，在过去几个世纪中，世界已逐渐变得 _更加_ safe[26](https://michaelnotebook.com/dtd/index.html#fn26)，这让我有些迟疑：murder rates 更低，disease 和 malnutrition 带来的 burden 更低，甚至 warfare 也在逐渐减少。Naive extrapolation 表明，在拥有更 powerful technologies 的未来，情况很可能会变得 _更好_。我不知道该如何 reconcile 这两种观点；但这样做似乎很重要。

## 现在可以开展哪些 practical DTD 工作？

2019 年，我[在 Twitter 上询问](https://twitter.com/michael_nielsen/status/1204129299369877504)大家，各自在为应对 climate change 做些什么。我收到了大约 150 人的回复，其中大多数都经过深思熟虑。令人意外的是，很多人说，尽管他们非常关心 climate change，但除了 voting 之外，他们个人并没有直接做什么。有几个人给出了这样的论证，其大意是：“当然，我担心 climate change，但既然减少我个人的 carbon impact 对 address 那些 underlying causal factors 毫无作用，我为什么要这样做呢？那些因素实际上是 systemic，而非 individual 的。我宁愿只做 voting 之类的事情，因为这些事情有助于改变 systemic factors。”按照这种观点，许多常见的行动，例如减少 flying 或 automobile use，都是在付出 personal cost，却只是在 treating 一个 symptom，而非其背后的“disease”。

这个回答令人印象深刻。尽管我当然并不完全同意，但它凸显了 DTD 总体上的一个重要之处：尝试 identify 并 address 那些 underlying causal systemic factors，而不仅仅是 symptoms。具体而言，有三个非常值得提出的问题：

1.  今天可以开展哪些类型的 DTD 工作，使其 aligned with 当今的 institutions 和 values（尤其是 market，但也包括我们的 research、not-for-profit 和 governmental ecosystems）？
2.  哪些类型的 DTD 很重要，却没有得到当今 institutions 和 values 的 adequately supported？为什么它们没有得到 adequately supported？
3.  需要哪些改变，才能更好地 align 我们的 values 和 institutions with DTD？具体而言，这种 alignment 有哪些 barriers，又如何才能实现这些改变？

下面让我针对这三个问题的不同组合，谈一些零散的看法[27](https://michaelnotebook.com/dtd/index.html#fn27)。这当然远非完整的讨论。尤其明显的遗漏是 conventional safety engineering。

1.  **Space colonies：**具体而言，着眼于在 solar system 的其他地方建立 permanent、self-sustaining（或接近 self-sustaining）的 colonies，并最终扩展到其他 solar systems。这些 colonies 将与 earth 上的某些 destructive technologies（尽管不是全部）处于 isolated 的状态，从而提供一定程度的 insulation from risk。（它们也会带来巨大的 benefits。）这还不是 Effective Altruism 中一个已确立的 cause area，让我颇感意外。的确，要真正在别处 create 一个 colony，超出了当今 earth 上除规模最大的 economic entities 之外所有实体的能力。但这并不意味着无法以低得多的成本作出 major contributions，尤其是在 underfunded niches 中。人们经常批评这种 colonization 的想法“不可能”，或者 economically infeasible。一个经常用来说明这一点的例子是 Antarctica：几乎没有人生活在 Antarctica，然而，与 solar system 中几乎所有其他地方相比，Antarctica 很可能是一个 accessible 得多、hospitable 得多的 environment。从 10 年，甚至可能是 50 年的角度来看，这似乎是对的。从 thousand-year timescale 来看，这几乎肯定是错的。事实上，撇开 safety 不谈，即使仅出于 economic reasons，我也相信其他地方存在极其 valuable 的 niches，而这些 niches 将 drive colonization。比如 asteroid belt；near-earth asteroids；Jupiter；moon；zero g；hard vacuum。也许其中一个，甚至几个，最终会是 economic bust。但从长远来看，我相信其中有几个将各自帮助 create（至少）tens of trillions of dollars 的 value，而且也会大大 improve human life。我们会找到让 space 为我们共同的 humanity produce wealth 的方法，space 的 colonization 也会蓬勃发展，为 humanity 带来 long-run benefit。
2.  **DTD VC：**思考一个专注于 DTD 的 VC 能取得什么成果，是件有意思的事。它在建立某些类型的 expertise、founder relationships 和 LP relationships 方面，可能具有一些 advantages。把 Pigouvian taxes 作为 fund 的 inputs 来考虑，也很有意思。这与使用 carbon taxes 来 fund carbon removal 的提议有些类似。
3.  **AI Safety Watch：**仿照 [Retraction Watch](https://retractionwatch.com/)，建立一个有洞察力（且坚持不懈）的 blog，monitors 并 analyzes 所有 AI safety incidents，并提升 awareness、建立 online presence。从长远来看，理想情况下，它将建立一个 incidents 的 database，就像 security incidents 领域已经在做的那样。
4.  **Increasing the rate of research field creation：提高 research field creation 的 rate：**要将许多 risk 和 xrisk 领域确立为值得 research 的 subjects，一直很有挑战性。在 metascience 领域，已经有人提出 interventions，以 modify science，使其 increases the rate of research field creation[28](https://michaelnotebook.com/dtd/index.html#fn28)。这些想法可以进一步 developed，并付诸行动。
5.  **Brain-computer interfaces：**在这里，它有多 beneficial（或并非如此），似乎很可能取决于 technology development 过程中作出的某些关键选择。谁 owns 它？它有多 centralized？是否有 open source versions？在多大程度上存在 single points of failure？它会在多大程度上 amplify inequality？它会在多大程度上使 companies 或 governments 能够 control 或 influence 我们的 thinking？它会有多 secure？今天作出的选择，可能会极大地影响这些问题的最终答案。Cvitkovic 已写了一篇关于 differential neurotechnology development 的深思熟虑的文章[29](https://michaelnotebook.com/dtd/index.html#fn29)；另参见 McCaleb[30](https://michaelnotebook.com/dtd/index.html#fn30)。
6.  **Mechanisms for solving public goods and collective action problems：用于 solving public goods 和 collective action problems 的 Mechanisms：**包括 dominant assurance contracts；Vickrey-Clarke-Groves (Liberal Radical) mechanism；以及 Ostrom 关于 common pool resources 的工作。
7.  **ASI-for-DTD：**我在前面提到过这样一个想法：ASI 作为开展 DTD 工作的 enabler，可能是 essential 的。思考创办一家专注于 ASI-for-DTD 的公司，或者专注于 ASI-for-science-and-technology 等相关 subjects 的公司，是件有意思的事。DeepMind 及其 spinoff——Isomorphic Labs——在一定程度上正朝这个方向发展。

## 要点

写这些笔记时，我多少有些沮丧。其中与其说有 solutions，不如说有表述得更好的 problems。不过，这仍有价值，而且我在 solutions 方面也取得了一点进展。值得记住的要点：

-   strong Alignment Problem：对 liberal society 进行 aligning，使其 strongly prioritizes DTD，同时 preserving liberal values 的 problem。这不仅 automatically entails AI alignment problem 的 solution，也能更好地 addresses 背后的 causal 和 systemic issues，包括 ASI safety 的 stability problem（例如，即使你能 align 一个 ASI，那也并不 prevent 其他 non-aligned ASI's 的 easy creation）。
-   值得进一步 development、并与 strong Alignment Problem 的 solution 有关的 Ideas：provably beneficial surveillance；DTD VC；AI Safety Watch；DTD taxes；如何让 research field production 的 rate 达到 10x；以及一般意义上的 public goods production 和 collective action problems
-   Space colonization 作为 DTD 被低估了
-   ASI-for-DTD 是 essential route 吗？
-   Great Stagnation 真的是 Great Realignment 的开端吗？
-   任何 posthuman successors 也都需要 solve strong Alignment Problem；事实上，它很可能只会变得更糟；我们无法逃避 strong Alignment Problem，最多只能把它推迟到以后
-   DTD 这个 notion 存在一些非常重大的 problems，尤其是：risk sign problem（判断一种 technology 是 increases 还是 decreases risk，困难程度令人意外），以及它只是部分属于 technology 的 property、而非其他 contingent factors 这一事实。是否有更好的 replacement notion？
-   一个 society 最多地 amplifies 什么，就会得到什么。我们如何才能更好地 amplify differential technological development？

## January 19, 2024 补记

这则补记是为了明确一个关键点：在 January 13, 2024 首次发布这些笔记时，我对它的提炼还不够鲜明。我们的 civilization 没有足够 strongly 地 prioritize DTD——因而将无法 solve Alignment Problem——因为 safety loop 在许多重要的 risk classes 上运转不佳。当 consumers 承担 highly legible risks 时，它运转良好。但当一种 technology damages commons、affects third parties，或需要应对 long-term、illegible、diffuse 或 hard-to-measure risks 时，它往往运转不佳。这就是我讨论 collective action 的 tools、provably beneficial surveillance 以及相关 ideas 的原因：它们是朝着这个方向的 partial solutions。如果一个 society 最多地 amplifies 什么就会得到什么，那么我们就需要找到更有力地 amplifying DTD 的办法，以 address 那些 safety loop 运转不佳的 risks。这正是我们 address Alignment Problem 所需要的。因此，再问一次：我们如何才能快得多地 amplify differential technological development？

## 引用信息

在 research work 中，请按以下方式引用本文：Michael Nielsen, "Notes on Differential Technological Development", [https://michaelnotebook.com/dtd/index.html](https://michaelnotebook.com/dtd/index.html) (2024).

## 致谢

感谢 Josh Albrecht、Fawaz Al-Matrouk、Alexander Berger、David Bloomin'、Vitalik Buterin、Brian Christian、Patrick Collison、Susan Danziger、Laura Deming、Anastasia Gamick、Tim Hwang、Andy Matuschak、Hannu Rajaniemi 和 Albert Wenger 与我就这些主题进行交流。

## 脚注

* * *

1.  在这些笔记中，我大部分时候会使用 abbreviation DTD。我注意到自己和其他一些人倾向于说 "differential technology development"，而不是 "differential technological development"，或许是因为 "technology" 是 concrete noun（尽管在这里被用作 adjectival phrase 的一部分来 modify "development"），而 "technological" 是相当 abstract 的 adjective。既然我使用 DTD，这一点或许已经无关紧要，但我必须承认，我强烈偏好 "differential technology development" 这个 term。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref1)

2.  Nick Bostrom, "Existential Risks: Analyzing Human Extinction Scenarios and Related Hazards" (2002).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref2)

3.  相关工作实在太多，甚至无法在这里作一个简短的 overview。我尤其喜欢的一篇涵盖面广的文章是：Vitalik Buterin, "My Techno-Optimism" ,[https://vitalik.eth.limo/general/2023/11/27/techno\_optimism.html](https://vitalik.eth.limo/general/2023/11/27/techno_optimism.html) (2023)。我的笔记在：[https://michaelnotebook.com/vbto/index.html](https://michaelnotebook.com/vbto/index.html)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref3)

4.  Michael Nielsen, "Notes on the Vulnerable World Hypothesis", [https://michaelnotebook.com/vwh/index.html](https://michaelnotebook.com/vwh/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref4)

5.  例如，参见：Cagan Koc and Jennifer Jacobs, "US Pressured Netherlands to Block China-Bound Chip Machinery", [https://www.bloomberg.com/news/articles/2024-01-01/us-pushed-asml-to-block-chinese-sales-before-january-deadline](https://www.bloomberg.com/news/articles/2024-01-01/us-pushed-asml-to-block-chinese-sales-before-january-deadline) (2024).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref5)

6.  关于这一点，一个很好的讨论是：Neil Postman, "Technopoly" (1992)。请注意，这种[对 technocracy 的 sympathy](https://twitter.com/michael_nielsen/status/1744783594424500721)广泛存在于那些对 ASI 的看法表面上截然不同的人之中：“e/acc、doomers、EAs、VCs、researchers、safety people、AI startups，_等等_。将他们联系在一起的，是对 technology 的 agency 感和广泛的信念，即使他们对如何才能最好地 developed 它的细节存在分歧。然而，世界上许多人并没有这种对 technology 的 agency 感；在某些情况下，他们几乎没有 agency，technology 是别人施加于他们的东西”。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref6)

7.  Michael Nielsen, "Notes on the Vulnerable World Hypothesis", [https://michaelnotebook.com/vwh/index.html](https://michaelnotebook.com/vwh/index.html) (2023)。另请参见 Albert Wenger 的演讲 [https://www.youtube.com/watch?v=lBUPEv6KlnM](https://www.youtube.com/watch?v=lBUPEv6KlnM)、David Brin 的 "The Transparent Society" (1998)，以及正在进行的关于 sousveillance 的工作。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref7)

8.  Nick Bostrom, "The Vulnerable World Hypothesis", [https://nickbostrom.com/papers/vulnerable.pdf](https://nickbostrom.com/papers/vulnerable.pdf) (2019).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref8)

9.  Brain hackers 将会成为现实。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref9)

10.  关于这一点有大量 literature。一篇简短且近期的通俗 survey 是：Alicea Hibbard, [Investigating the Smallpox Blanket Controversy](https://asm.org/articles/2023/november/investigating-the-smallpox-blanket-controversy) (2023)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref10)

11.  Robock 和 collaborators 写过许多文章，讨论对 geoengineering 的担忧。一篇有帮助且相对较新的 summary 是：Alan Robock, "Benefits and Risks of Stratospheric Solar Radiation Management for Climate Intervention (Geoengineering)" (2020)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref11)

12.  例如，我此前曾论证，probability of doom 这一 notion 可能会错误地使人们——包括核心人物——对 ASI development 产生 fatalistic 的看法。我也和许多人一样论证过，某些 AI alignment 和 safety 工作很可能 sped up 了 ASI，并且可能产生了 negative impact。这两点都在以下文章中有所讨论：Michael Nielsen, "Notes on Existential Risk from Artificial Superintelligence", [https://michaelnotebook.com/xrisk/index.html](https://michaelnotebook.com/xrisk/index.html) (2023)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref12)

13.  Marshall McLuhan, "Understanding Media: The Extensions of Man" (1964).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref13)

14.  Stuart Russell，见：[https://www.edge.org/conversation/jaron\_lanier-the-myth-of-ai](https://www.edge.org/conversation/jaron_lanier-the-myth-of-ai) (2014)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref14)

15.  Nate Soares and Benya Fallenstein, "Agent Foundations for Aligning Machine Intelligence with Human Interests: A Technical Research Agenda" (2014).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref15)

16.  AI companies 经常论证，要在 alignment 上取得进展，就必须由它们 build 并 align 更好的 systems。对于 AI Alignment Problem 的 conventional formulation 而言，它们是对的。但这也很可能具有强烈的 destabilizing 作用，因为它将带来能力强得多的 non-aligned systems。从这个意义上说，可以认为它们的工作正在阻碍我对 Alignment Problem 的 reformulation 的 solution。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref16)

17.  当我这样说时，人们有时会举出近期备受关注的问题实例来反驳，例如 Boeing 接连出现的问题。这无意间恰好证明了我的观点。Boeing 正受到极其严格的 standard 的要求（理应如此！），以至于即使是没有造成生命损失的 incidents，也会得到极其广泛而持久的报道。相比之下，automobile manufacturers 几乎被轻轻放过，因此 automobiles 要 dangerous 得多。说清楚一点：它们_也_受益于这个 safety feedback loop，只是它没有那么 strong。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref17)

18.  本段改编自：Michael Nielsen, "Notes on Existential Risk from Artificial Superintelligence", [https://michaelnotebook.com/xrisk/index.html](https://michaelnotebook.com/xrisk/index.html) (2023)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref18)

19.  Elinor Ostrom 等人强调过，面对 hard-to-measure-or-demarcate harms 时，这样做存在困难，见 Elinor Ostrom 的 "Governing the Commons" (1990)。感谢 Albert Wenger 提醒我注意这个问题。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref19)

20.  人们很容易想用 animal species 的 extinction rates 来思考 humans 的 extinction risk。但 humans 是_唯一_拥有某种 non-trivial safety loop 的 species。在我看来，这是 humans 与其他 species 之间最有意思的 differences 之一。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref20)

21.  Tyler Cowen, "The Great Stagnation" (2011)。另请参见 Peter Thiel, "The End of the Future" [https://www.nationalreview.com/2011/10/end-future-peter-thiel/](https://www.nationalreview.com/2011/10/end-future-peter-thiel/) (2011)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref21)

22.  这种 framing 部分来自 Patrick Collison 给我的建议。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref22)

23.  Stewart Brand 对此有一段精彩的描绘：[https://www.youtube.com/watch?v=YIZw5cT84ZM&ab\_channel=AhmedKabil](https://www.youtube.com/watch?v=YIZw5cT84ZM&ab_channel=AhmedKabil)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref23)

24.  当然，我不希望看到 society 停滞在现状；我喜欢 change 和 growth 的想法。正如 Douglas Adams 曾诙谐地指出的，可以说 automobile 已经是 Earth 上的 dominant lifeform。我们可以设想一些 posthuman futures：当今 humanity 的 descendants 幸福地生活在一个 society of minds 之中，其多样性远胜于今天，是多种 posthumanities 的共存。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref24)

25.  Michael Nielsen, "Brief remarks on some of my creative interests", [https://michaelnotebook.com/ti/index.html](https://michaelnotebook.com/ti/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref25)

26.  例如，参见：Steven Pinker, "The Better Angels of Our Nature" (2011)。[↩︎](https://michaelnotebook.com/dtd/index.html#fnref26)

27.  另请参见：AJ Kourabi, "What is differential technological development?", [https://web.archive.org/web/20230224162351/https://www.ajkourabi.com/writings/what-is-differential-technological-development](https://web.archive.org/web/20230224162351/https://www.ajkourabi.com/writings/what-is-differential-technological-development)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref27)

28.  Michael Nielsen and Kanjun Qiu, "A Vision of Metascience", [https://scienceplusplus.org/metascience/index.html](https://scienceplusplus.org/metascience/index.html) (2022).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref28)

29.  Milan Cvitkovic, "Cause Area: Differential Neurotechnology Development", [https://forum.effectivealtruism.org/posts/Qhn5nyRf93dsXodsw/cause-area-differential-neurotechnology-development](https://forum.effectivealtruism.org/posts/Qhn5nyRf93dsXodsw/cause-area-differential-neurotechnology-development) (2022)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref29)

30.  Jed McCaleb, "We Have to Upgrade", [https://www.lesswrong.com/posts/vEtdjWuFrRwffWBiP/we-have-to-upgrade](https://www.lesswrong.com/posts/vEtdjWuFrRwffWBiP/we-have-to-upgrade) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref30)

## 评论

---

## 原文

[![](https://michaelnotebook.com/assets/home.png)](https://michaelnotebook.com/index.html)

[Michael Nielsen](https://michaelnotebook.com/)

[Astera Institute](https://astera.org/)

January 13, 2024

Differential technological development (DTD)[1](https://michaelnotebook.com/dtd/index.html#fn1) is the idea that humanity should focus on reducing the development of dangerous technologies, and accelerate the development of safety-enhancing technologies. The term (though of course not the concept) originates in a 2002 paper by Nick Bostrom[2](https://michaelnotebook.com/dtd/index.html#fn2). It's been the focus of especial interest in recent years in connection with several advanced technologies, including synthetic biology and artificial superintelligence (ASI)[3](https://michaelnotebook.com/dtd/index.html#fn3). In an earlier set of notes[4](https://michaelnotebook.com/dtd/index.html#fn4) I sketched a reformulation of the Alignment Problem as the "problem of aligning the values and institutions of a liberal society (including, crucially, the market) with differential technology development". The purpose of such an alignment is to give us the enormous benefits of technology, while greatly reducing existential risk, and also preserving or enhancing the values we hold most dear.

In these notes I will:

-   Examine the definition of DTD, identifying several ambiguities, and reflect on ways of resolving them. The underlying question: is DTD a useful category for guiding decision-making?
-   Strengthen the above reformulation of the Alignment Problem, and more fully explain the connection to AI and ASI safety.
-   What kind of work can be done on DTD? This won't be anything like a full survey, just a brief and _ad hoc_ look at a few approaches I find interesting.

A question I don't address, but which is important: to what extent is DTD different from older ideas – from fields ranging from safety engineering to sustainability and so on? Obviously there is considerable overlap, but I shan't engage systematically with the question.

The notes are part of a series in which I work out my thinking about existential and catastrophic risk from technology, including (but not only) ASI and biotech. Other entries in this series may be found [here](https://michaelnotebook.com/tag/xrisk.html). (This current set of notes is, frankly, particularly in-the-weeds, mostly focused on identifying crucial problems, but making frustratingly little progress on solving them.) To date, these are all rough preliminary thinking. They're intended principally for my own use, although a few people have been kind enough to say they've benefited from the notes. I hope my understanding will eventually mature enough to make it worth writing a more polished and better developed overview; if so, it will also appear at the above link!

## Notes on the original paper

The description of DTD in Bostrom's original 2002 paper is brief and interesting enough to be worth quoting and commenting on in full. I omit only the inline citations in the original text; they may of course be found in the original paper.

> If a feasible technology has large commercial potential, it is probably impossible to prevent it from being developed. At least in today’s world, with lots of autonomous powers and relatively limited surveillance, and at least with technologies that do not rely on rare materials or large manufacturing plants, it would be exceedingly difficult to make a ban 100% watertight. For some technologies (say, ozone−destroying chemicals), imperfectly enforceable regulation may be all we need. But with other technologies, such as destructive nanobots that self−replicate in the natural environment, even a single breach could be terminal. The limited enforceability of technological bans restricts the set of feasible policies from which we can choose.

This paragraph (and its sequel) appears to me overly dismissive of the partial solution of bans on harmful technologies. The locks on my house won't stop a determined burglar, but there is still considerable benefit in having the locks; they are a partial solution. Technological bans are certainly imperfect, as the paragraph correctly points out, but they are worth consideration as part of a DTD strategy. Although nuclear power overall wasn't stopped by opposition, it seems plausible that certain types of nuclear technology have been stopped (or considerably delayed) by the opposition, despite having large commercial potential. Of course, whether this is good or bad for humanity is another question – I merely offer it as a plausible example of a _de facto_ ban of a feasible technology with large commercial potential. Strikingly, it wasn't achieved by a direct legislative ban, but rather by an overall slowing of technological development – the space of possible technologies simply hasn't been explored nearly as much as it might have been. Put another way, the approach hasn't been to go for a direct ban, but rather to use opposition to make the _cost_ of developing certain technologies sufficiently high that they in practice aren't developed. You might retort that this means they don't have "large commercial potential", and thus aren't a counterexample to the argument of the paragraph. But in spirit it's a counterexample: the point may be restated as being that commercial potential is far from intrinsic to the technology, but can be radically affected by other things, including public sentiment. In practice, I think bans are worth considering as part of the DTD armamentarium.

Bostrom defines DTD in the next paragraph. I've added emphasis here to make the definition more prominent:

> What we do have the power to affect (to what extent depends on how we define “we”) is the rate of development of various technologies and potentially the sequence in which feasible technologies are developed and implemented. Our focus should be on what I want to call _differential technological development:_ _trying to retard the implementation of dangerous_ _technologies and accelerate implementation of beneficial technologies,_ _especially those that ameliorate the hazards posed by other technologies._

I shall return to this definition in the next section, and identify several important ambiguities.

> In the case of nanotechnology, the desirable sequence would be that defense systems are deployed before offensive capabilities become available to many independent powers; for once a secret or a technology is shared by many, it becomes extremely hard to prevent further proliferation.

True, although sometimes there are important monopolies that prevent proliferation, at least for some time period. An interesting current example is the company ASML, which has a _de facto_ monopoly over advanced photolithography. This monopoly has been exploited by the US, who pressured ASML to restrict sales to China in 2018; similar restrictions continue in 2024[5](https://michaelnotebook.com/dtd/index.html#fn5). Several people involved in semiconductor manufacturing have told me that ASML's advantage here is enormous, and surprisingly hard to replicate. I don't doubt that it _can_ be replicated, but the timescale might be well over a decade, perhaps several. It would be interesting to better understand the conditions responsible for creating such a monopoly; several people have confidently told me "the reason"; however, they've told me _different_ reasons, which suggests it's not so obvious.

> In the case of biotechnology, we should seek to promote research into vaccines, anti−bacterial and anti−viral drugs, protective gear, sensors and diagnostics, and to delay as much as possible the development (and proliferation) of biological warfare agents and their vectors. Developments that advance offense and defense equally are neutral from a security perspective, unless done by countries we identify as responsible, in which case they are advantageous to the extent that they increase our technological superiority over our potential enemies. Such “neutral” developments can also be helpful in reducing the threat from natural hazards and they may of course also have benefits that are not directly related to global security.

Such "neutral" developments do have, as Bostrom says, considerable benefits. But they also implicitly take the side of technocracy against non-technocrats; it's an inherent redistribution of power _within_ a society. Put another way: an assumption underlying DTD is that the cure for technology is more technology. Most of my readers are probably technocrats, and some perhaps don't see this as an issue[6](https://michaelnotebook.com/dtd/index.html#fn6). Still, I think this movement of power into technology is an important underexamined element of DTD. It's a way in which people favoring DTD are very similar to people favoring almost totally unimpeded advance in technology.

The next paragraph is remarkable, especially in light of recent (as of 2024) discussion of ASI:

> Some technologies seem to be especially worth promoting because they can help in reducing a broad range of threats. Superintelligence is one of these. Although it has its own dangers (expounded in preceding sections), these are dangers that we will have to face at some point no matter what. But getting superintelligence early is desirable because it would help diminish other risks. A superintelligence could advise us on policy. Superintelligence would make the progress curve for nanotechnology much steeper, thus shortening the period of vulnerability between the development of dangerous nanoreplicators and the deployment of adequate defenses. \[ _not obvious at all – if the offenses are helped more by ASI than the defenses, then_ _ASI simply makes the situation worse_.\] By contrast, getting nanotechnology before superintelligence would do little to diminish the risks of superintelligence. The main possible exception to this is if we think that it is important that we get to superintelligence via uploading rather than through artificial intelligence. Nanotechnology would greatly facilitate uploading.

In a nutshell, the argument is: work on ASI _is_ DTD, because it reduces risk from nanotech! Both Eric Drexler and Bill Joy are cited in the paper; Joy is thanked in the acknowledgments. I suspect few people today would regard work on ASI as DTD, though perhaps I am wrong about that. I wonder how much the availability heuristic determines what is regarded as DTD and what is not? I.e., lots of people circa 2002 were worrying publicly about nanotech risk, fewer about ASI risk; today, the reverse is true, and so opinions about what is DTD have changed. For myself, I'm gradually coming to think it at least plausible that developing and using ASI for DTD may be absolutely essential. It's an unsettling prospect; I'm reminded of Mario Andretti's remark that "If everything seems under control, you're not going fast enough."

> Other technologies that have a wide range of risk−reducing potential include intelligence augmentation, information technology, and surveillance. These can make us smarter individually and collectively, and can make it more feasible to enforce necessary regulation. A strong prima facie case therefore exists for pursuing these technologies as vigorously as possible. As mentioned, we can also identify developments outside technology that are beneficial in almost all scenarios. Peace and international cooperation are obviously worthy goals, as is cultivation of traditions that help democracies prosper.

It's fascinating to see surveillance listed as a risk-reducing technology, since so many see it as a tool of tyranny. I have briefly discussed elsewhere the possibility of _provably beneficial surveillance_[7](https://michaelnotebook.com/dtd/index.html#fn7), but that is currently a speculative design possibility. I expect that by default surveillance will increase risk, not decrease it. Bostrom, at least in 2002, appeared to believe the reverse. This underscores the considerable ambiguity in what counts as DTD.

## Is DTD a useful category?

Recapping, Bostrom's 2002 definition of DTD: trying to retard the implementation of dangerous technologies and accelerate implementation of beneficial technologies, especially those that ameliorate the hazards posed by other technologies. He gave a similar, slightly more elaborate definition in his 2019 paper on the Vulnerable World Hypothesis[8](https://michaelnotebook.com/dtd/index.html#fn8):

> Retard the development of dangerous and harmful technologies, especially ones that raise the level of existential risk; and accelerate the development of beneficial technologies, especially those that reduce the existential risks posed by nature or by other technologies.

People often abbreviate these definitions as something like: work on defensive technologies in preference to offensive. This seems quite straightforward at first, but as the discussion in the last section already suggests, it's often hard to say what counts as DTD and what does not. One person's offensive technology is another person's defensive technology. I've already briefly mentioned ASI. Let me give five additional examples to illustrate the ambiguity. You don't need to personally accept the ambiguities; indeed, in talking over examples like those below, I've often found people rejecting any claim of ambiguity. But, crucially, different people reject the claim for different reasons. Some people will say "obviously a brain-computer interface (BCI) is defensive"; others "obviously BCI is offensive". My experience is that there's a _lot_ of disagreement like this, from well-informed and thoughtful people, which suggests digging beyond surface reasons for such disagreement.

1.  **Nuclear retaliatory second-strike capability:** Is a second-strike capability offensive or defensive? You might argue that it is "obviously" offensive, since it enables one country to wreak devastation on another; but a comparably strong argument can be made that it is "obviously" defensive, since the purpose of a second-strike capability is to dissuade other countries from ever using nukes. Indeed, whether offense or defense is preponderant may depend on subtleties about the institutions of the country with the second-strike capability, their doctrine on the use of force, the ease with which it may be repurposed for first strikes, the nature of their relationship with other countries, and so on. It is not clear cut at all.
2.  **Missile shield:** Ronald Reagan's proposed "Star Wars" program was to provide the US with a shield against long-range missiles. It would seem to be a defensive technology, and thus an example of DTD. But many analysts claimed that it would in fact disrupt the stability of mutually assured destruction, making the US much less vulnerable to a second strike; and so more able and more likely to engage in a first strike; and perhaps even as a result more provocative of surprising and perhaps poorly conceived action by the Soviet Union. Thus it is by no means obvious whether such a missile shield is an example of DTD.
3.  **BCI:** As noted above, I've heard people say BCI is "obviously" a DTD, in the sense that it may help amplify human cognition, and put us on a more equal footing with future ASIs. But BCI is also potentially a way for companies or governments or third parties[9](https://michaelnotebook.com/dtd/index.html#fn9) to exert influence or even dominance over individuals. Again: there is considerable ambiguity on whether BCI contributes to a reduction in risk.
4.  **Vaccines:** Vaccines perhaps seem more "obviously" to be a defensive technology than the examples above. And yet they can also be used offensively. Most obviously, by denying some group access to a vaccine, a vaccine may become effectively a technology of offense. It's not exactly the same, but there are several historical instances where invaders supposedly have deliberately infected native populations with smallpox, relying on the considerable pre-existing immunity of the invaders as protection[10](https://michaelnotebook.com/dtd/index.html#fn10). One can imagine similar strategies being used in future biowarfare. I tend to think vaccines net out as clearly DTD, but can imagine someone disputing that in specific instances, e.g., when vaccines are developed as _part_ of a biowarfare strategy.
5.  **Geoengineering:** In his 2019 paper Bostrom gave this as an example of something that is obviously an offensive(!) technology, advising: "don't develop forms of geoengineering that would empower random individuals to unilaterally make drastic alterations to the Earth’s climate". I suspect this would surprise at least some people, who see geoengineering as part of the arsenal of technologies that may help mitigate the impact of climate change. Other people have, however, argued against geoengineering on the grounds that it is too risky. Alan Robock, a leading expert on the impact of aerosols on the stratosphere, has identified many risks associated to geoengineering, including the possibility of disrupting the Indian monsoon, and so affecting food security for a considerable portion of the world's population[11](https://michaelnotebook.com/dtd/index.html#fn11).

At least some (and maybe all) of these concerns may seem like pedantic nitpicking. I do not think so: I think they are a major problem. Increasingly, funding decisions are being made to fund work on DTD. It would be (extremely) unfortunate if those decisions inadvertently create more problems than they ameliorate. Making good, useful definitions really matters. Poor conceptual foundations often lead to bad outcomes[12](https://michaelnotebook.com/dtd/index.html#fn12). Three major problems seem to me to be:

-   _The risk sign problem_: there can be hard-to-resolve disagreement over whether a given technology increases or decreases risks. This problem arises because we often have relatively poorly developed models of such risks, or they depend upon many hard-to-anticipate contingencies.
-   _Risk is not a property intrinsic to a technology; but technologies do nonetheless have certain natural tendencies of use:_ The sententious have for decades stated bromides along the lines of "a technology isn't good or bad, it depends upon how you use it". It's in some very literal sense true, but Marshall McLuhan responded aptly[13](https://michaelnotebook.com/dtd/index.html#fn13), pointing out that particular technologies (or media, in McLuhan's context) are often much more naturally used for good or bad ends. He described the "technology-as-intrinsically neutral" point of view as "the voice of the current somnambulism. Suppose we were to say… \`The smallpox virus is in itself neither good nor bad; it is the way it is used that determines its value.' Again, \`Firearms are in themselves neither good nor bad; it is the way they are used that determines their value.'… I am not being perverse. There is simply nothing in \[this kind of\] statement that will bear scrutiny, for it ignores the nature of the medium, of any and all media, in the true Narcissus style of one hypnotized by the amputation and extension of his own being in a new technical form." For DTD to be a useful category this is an extremely unfortunate inbetween position to be in.
-   _Dependence on who controls a technology:_ Whether a technology increases or decreases risk depends a lot on who you perceive as controlling the technology. If it's friends or allies of yours you're much more likely to perceive it as decreasing risk. Still, over a long enough run it is likely technology will spread widely, and it is unwise to assume only you and your allies will have it.

These issues make me wonder: is DTD well enough defined to be a useful category? When we're talking about current technologies, it's often reasonably clear whether or not a technology increases or decreases risk. For instance, widespread investment in flame retardant building materials, or in improvements in aircraft safety engineering, are clearly examples of DTD. However, the purpose of introducing the term DTD is to help think about the broad long-run effects of investing in particular new technologies. In those cases, what counts as DTD and what does not is often a very complicated question. It's tempting to throw up one's hands and say: "It's foolish to attempt long-rang foresight like this, with so many contingencies. Better to adopt a just-in-time approach, responding to imminent problems, or problems where we have really strong models".

I don't have a strong, satisfying conclusion here. I do have two partial conclusions. The first is that I should spend some time searching for other abstract categories that might serve many of the same purposes DTD was introduced to address, but not suffering some of these shortcomings. I've made some progress on this, but it's as yet too in-the-weeds to report here. The second is a general caution around the use of DTD. Whether a given line of work is DTD needs to be regularly re-evaluated. It requires independent evaluations from diverse sources. It's interesting to ponder whether tools like prediction markets can be useful in making the evaluation of future risks. And simply to do more in-depth historical surveys, to figure out which technologies had surprising positive or negative consequences.

## Reformulating "the" Alignment Problem

As mentioned in the introduction, in my notes on the Vulnerable World Hypothesis, I gave a rough reformulation of the Alignment Problem as "the problem of aligning the values and institutions of a liberal society (including, crucially, the market) with differential technology development". I want to revisit that reformulation in the light of the above discussion. I'll make some modest improvements, and then consider the relationship to the conventional formulation of AI alignment. While this reformulation appears quite different from standard formulations of alignment in AI, as I shall explain below it has some significant advantages: (1) it automatically entails the solution of the standard AI formulation; (2) it _also_ addresses some crucial problems not addressed in the standard AI formulation; and (3) it does this by more correctly identifying the underlying causal factors. A few observations:

1.  I've highlighted the challenge of deciding whether or not a technology contributes to DTD. One possible response is to reframe Alignment as "the problem of aligning the values and institutions of a liberal society (including, crucially, the market) to ensure net risks remain manageable". It becomes interestingly vacuous. Making DTD a noun – presumptively, a concrete property of some technologies and not others – makes it starker, and carries the implied assertion that much current technological development is _not_ DTD.
2.  A starker and better statement is: "the problem of aligning a liberal society so it strongly prioritizes DTD, while preserving liberal values". Implicitly this asserts that there are many liberal societies which do not especially prioritize DTD, and that this is a problem to be solved, by modifying their institutions and values, but in way that preserves fundamental liberal values.
3.  It's still a weak statement, in that it does not say exactly how strong the prioritization should be, nor how to make the assessment of which technologies qualify as DTD. I'll come back to these points below. I do want to note immediately that I toyed with variations along the lines of: "the problem of aligning a liberal society so it absolutely prioritizes \[or enforces\] DTD, while preserving liberal values". This is a stimulating line of variation. In principle it ought to allow a lot of technological experimentation; in practice, I suspect there is a slippery slide to a culture which absolutely prioritizes safety, to its own long-run detriment.

Let's consider the relationship to AI alignment. An early statement of the AI Alignment Problem was by Stuart Russell in 2014[14](https://michaelnotebook.com/dtd/index.html#fn14), where he stated it as:

> No one in the field is calling for regulation of basic research; given the potential benefits of AI for humanity, that seems both infeasible and misdirected. The right response seems to be to change the goals of the field itself; instead of pure intelligence, we need to build intelligence that is provably aligned with human values. For practical reasons, we will need to solve the value alignment problem even for relatively unintelligent AI systems that operate in the human environment. There is cause for optimism, if we understand that this issue is an intrinsic part of AI, much as containment is an intrinsic part of modern nuclear fusion research. The world need not be headed for grief.

Shortly after that formulation, Nate Soares and Benya Fallenstein published "Agent Foundations for Aligning Machine Intelligence with Human Interests: A Technical Research Agenda"[15](https://michaelnotebook.com/dtd/index.html#fn15):

> We call a smarter-than-human system that reliably pursues beneficial goals "aligned with human interests" or simply "aligned." To become confident that an agent is aligned in this way, a practical implementation that merely seems to meet the challenges outlined above will not suffice. It is also necessary to gain a solid theoretical understanding of why that confidence is justified. This technical agenda argues that there is foundational research approachable today that will make it easier to develop aligned systems in the future, and describes ongoing work on some of these problems.

Many (not all) the people working on AI alignment today are working on projects that fall recognizably within (or closely adjacent to) the scope of these formulations. There are, of course, many problems with these formulations, well known both to people working on the problem, and to critics. One broad class of problems relates to the difficulty in specifying and agreeing human values or human interests in any precise form. There is, of course, considerable disagreement between cultures, between individuals, and (indeed) even within individuals about what counts as human values or human interests. In a nutshell: what should an ASI be aligned _with_?

A second broad class of problems is what we might call the stability problem: even if we can satisfactorily solve the Alignment Problem for some particular AI system, that does not mean it will be solved for all AI systems. Just because the AI system you construct respects human values does not mean they will also be respected by the AI systems your enemies construct. This is especially important since today's frontier AI systems, requiring enormous teams and resources, can often be outperformed a few years later by tiny teams with limited resources. Given the very considerable interest in AI from military organizations – indeed, much pioneering AI work was funded by organizations such as DARPA – it seems unlikely that all advanced AI systems will respect human values.

Now, my formulation of the Alignment Problem as "the problem of aligning a liberal society so it strongly prioritizes DTD, while preserving liberal values" seem superficially almost unrelated to AI risk. But it has many benefits, and (correctly formulated) would: (a) entail a solution of the AI Alignment Problem; (b) address many of the issues with the conventional formulation of the AI Alignment Problem; and (c) would also address issues beyond AI, including bio risk, climate risk, nuclear risk, security risk, and, I expect, many emergent classes of risk, as yet unknown. In particular, successful AI alignment work is an example of DTD. And insofar as our society prioritizes DTD, it must prioritize AI alignment and slow down or pauses capabilities that hurt AI alignment. But it goes further than that: if the stability problem is actually the main threat, then it means we must actually prioritize the stability problem over anything else, including the (conventional) AI Alignment Problem[16](https://michaelnotebook.com/dtd/index.html#fn16). Of course, how to achieve this is not at all clear! As mentioned earlier, I've elsewhere proposed a solution, perhaps partial, based on the idea of provably beneficial surveillance.

For all these reasons, I've taken to thinking of this formulation as the _strong Alignment Problem_. Note that it still suffers some issues: (1) it's ambiguous about just _how_ strongly to prioritize DTD; (2) it doesn't specify what values of a liberal society ought to be immutable (as opposed to ones that merely happen to appear in ours); (3) it suffers from the ambiguities I've identified in DTD. All three of these can (and should) eventually be addressed. Still, I've found this reformulation extremely helpful.

## When we do DTD well; when we do DTD poorly; Alignment and the Safety Loop

There are certain kinds of DTD humanity does really well, where our society has already achieved alignment. And there are other kinds where we are far from achieving it. The implicit viewpoint of DTD is in some tension with the conventional market view. In that view, new technologies are developed, and _safety is then supplied in response to consumer demand_. The world's first commercial jet aircraft was the de Havilland Comet. Three Comet aircraft crashed in the first year; while Comet lingered for a while, of course it never really recovered. However, other jet manufacturers played close attention, and rapidly learned to make much safer aircraft. Similarly, the switch from refrigerators using ammonia to CFCs was a case of the market working well to reduce (though, unfortunately, not eliminate) risk. In these cases there is a strong and rapid _safety loop_ operating: when risks are borne immediately and very obviously by the consumer, the market is aligned with safety, and our society does DTD well. It's capitalism as a safety amplifier. We do safety best of all when the danger immediately and spectacularly impacts the consumer, as in the case of Comet. It is perhaps unsurprising that modern airlines are extraordinary paragons of safety[17](https://michaelnotebook.com/dtd/index.html#fn17).

By contrast, when no consumer is bearing an obvious immediate cost, our society tends to have much poorer institutions for doing DTD. In those cases markets may amplify technology whose risk exceeds it's benefits. This occurs when the risks involve: externalities; long time lines; are illegible or hard to measure; when they tend to be diffusely shared, perhaps impacting the commons; when it is hard to establish any kind of property rights in the harm. Those of economic mindset will sometimes simplify this down to "externalities", but this is far too great an oversimplification. Nonetheless, insofar as collective safety can (imperfectly) modeled as a public good, we'd expect it to be undersupplied by the market.

I grew up being told I'd likely die due to[18](https://michaelnotebook.com/dtd/index.html#fn18): nuclear war; climate change; acid rain; the ozone hole; or one of several other ailments. For the most part these issues were not (or only partly) addressed through market mechanisms. Instead, we've had to build bespoke non-market mechanisms to close the safety loop. Economists tend to favor Pigouvian taxes – and one can imagine general safety taxes being used to address the Alignment Problem, aligning the market with DTD[19](https://michaelnotebook.com/dtd/index.html#fn19) – but many of the actual solutions so far have come via other means, often some hybrid of governmental and open-source-bazaar style[20](https://michaelnotebook.com/dtd/index.html#fn20). A society gets what it amplifies most. Like evolution-by-natural-selection this seems near-tautological, but is extremely important. Even tiny differences in how much we amplify one thing versus another matter a great deal, and many of our most important institutions are amplifiers. Science tests and amplifies the ideas that help us better understand the world. Capitalism tests and amplifies companies that make things people want. Media amplifies things people find entertaining. How can we better amplify differential technological development?

## We've begun solving the Alignment Problem: the Great Stagnation is really the beginning of the Great Realignment

It has been widely noted by economists that worldwide economic growth appeared to change character circa 1970. This has been memorably dubbed "the Great Stagnation" by Tyler Cowen[21](https://michaelnotebook.com/dtd/index.html#fn21). The Great Stagnation is reflected in many ways, including in much reduced global total factor productivity (TFP) growth, often interpreted by economists to mean that the rate of technological innovation is slowing down. The reason for the Great Stagnation is debated. Robert Gordon\[fn:Gordon\] has argued that a big contributor is that the most important platform technologies can only be invented once, and it just so happens most were invented (or became widespread) over the period 1870 through 1970. This includes things like the internal combustion engine, refrigeration, aeroplanes, and so on.

Another possible contributor is the gradual rise of institutions and norms which actually slow down innovation. Things like lead-in-gasoline, Asbestos, DDT (not to be confused with DTD!), and CFCs were all introduced as miraculous new technologies, with relatively little pushback or oversight. Today, in many domains there are much more challenging regulatory gauntlets to be run. This is perhaps most famous in the case of the US FDA and EPA, but it is reflected around the world, and across multiple domains. Here's a list of a few key events — I've taken a US-centric view here, since it's been the dominant world power over the last 80 years; it is, however, interesting that many of these have (roughly contemporaneous) analogues in many other countries:

-   The rejection of thalidomide from the US market in 1960. At the time the FDA had just 11 people reviewing drugs
-   The Cuban Missile Crisis (1962)
-   The US Clean Air Act (1963), and the later creation of the US EPA (1970)
-   The Nuclear Nonproliferation Treaty (1970)
-   The banning of DDT in 1972
-   The OPEC oil crisis in 1973
-   The banning of asbestos: the US EPA began to impose restrictions in 1973. It is now near-fully eliminated
-   The banning of lead-in-gasoline: the US EPA began to impose restrictions in 1973, with full elimination in the US in 1996.
-   The banning of CFCs, mostly through the 1987 Montreal Protocol, the first treaty signed by every country (along with its predecessor, the Vienna Convention, dating to 1985, and aimed at the same problem, though less comprehensive)

It seems plausible that the Great Stagnation is really the initial stage of a Great Realignment[22](https://michaelnotebook.com/dtd/index.html#fn22), in which we gradually move toward solving the Alignment Problem. We may trade off some degree of growth for better institutions for dealing with risk. As a very rough caricature, we might say that up to around 1970 the general societal attitude toward safety was rather blase. And since then we've been gradually moving more and more toward a situation managing tradeoffs between safety and technological innovation[23](https://michaelnotebook.com/dtd/index.html#fn23).

It is tempting to view these processes as being in opposition. One sees this sometimes expressed in naive calls to "accelerate", from people who view "safety" as the enemy; and also in naive calls for "safety", from people who seem to discount any benefit in acceleration, and view technological progress as the enemy. There _is_ genuine tension there, but ideally it's positive sum; the trick is to find ways of managing that tension, so you get maximal benefit from both. I believe we have not yet found the best institutions for managing the tension.

## Any posthuman successor will also need to solve the Alignment Problem: we don't get to evade the Alignment Problem

One sometimes-stated "solution" to the AI safety problem is to give up on human chauvinism, and to celebrate the emergence and perhaps even the eventual dominance of posthumanity. "I for one welcome our new Posthuman Overlords" is a common tongue-in-cheek sentiment; so too is the variant "Welcome to our new ASI Overlords". Positions along these lines have been espoused by people such as Larry Page and Juergen Schmidhuber. Here, for example, is _The New York Times_ [recounting](https://www.nytimes.com/2023/12/03/technology/ai-openai-musk-page-altman.html) a conversation between Larry Page and Elon Musk, in which Page espoused the view that favoring humanity over AI was "specieist":

> Humans would eventually merge with artificially intelligent machines, he \[Page\] said. One day there would be many kinds of intelligence competing for resources, and the best would win.
> 
> If that happens, Mr. Musk said, we’re doomed. The machines will destroy humanity.
> 
> With a rasp of frustration, Mr. Page insisted his utopia should be pursued. Finally he called Mr. Musk a “specieist,” a person who favors humans over the digital life-forms of the future.

I suspect some of the people saying things along these lines are saying it mostly to be edgy. I suspect also that some believe it, sometimes rather strongly; indeed, I have some sympathy for elements of the view[24](https://michaelnotebook.com/dtd/index.html#fn24). It's worth taking the viewpoint seriously, and seeing if it can be made to hold up.

The key point: **any posthuman successors will need to solve the Alignment Problem**. That is, they will face almost exactly the same problem we face today. Actually, they will likely face a more challenging version, since their environment may contain many more powerful technologies. This is not certain: perhaps some extremely powerful safety-enhancing technologies will be developed, which make the situation much easier for our posthuman successors. But I suspect it's likely they will face even greater challenges than we do today. In general, the following heuristic diagram, from [25](https://michaelnotebook.com/dtd/index.html#fn25), captures something about the fundamental issue:

![](https://michaelnotebook.com/dtd/assets/graph.png)

I find this heuristic compelling. That said: I'm given some pause by the considerable amount of work suggesting the world has gradually become _more_ safe over the past few centuries[26](https://michaelnotebook.com/dtd/index.html#fn26): lower murder rates, lower burden from disease and malnutrition, and even a gradual drop in warfare. Naive extrapolation suggests that things are likely to get _better_ in a future of yet more powerful technologies. I don't know how to reconcile these two points of view; it seems important to do so.

## What practical DTD can one work on now?

In 2019 I [asked on Twitter](https://twitter.com/michael_nielsen/status/1204129299369877504) what people were individually doing about climate change. I received (mostly) very thoughtful responses from roughly 150 people. A surprising number of people said that while they were very concerned about climate change, they were not personally doing anything directly beyond voting. Several offered arguments whose gist was: "Sure, I'm worried about climate change, but why should I cut down on my personal carbon impact, when that does nothing to address the underlying causal factors, which are actually systemic, not individual? I'd rather limit myself to doing things like voting, which contribute to changing the systemic factors." In this view, many common actions, like reducing flying or automobile use, are imposing a personal cost, but only treating a symptom, not the underlying "disease".

It's a striking response. While I certainly don't completely agree, it highlights an important point about DTD in general: to try to identify and address the underlying causal systemic factors, not merely symptoms. In particular, three very interesting questions to ask:

1.  What types of DTD can be worked on today that are aligned with today's institutions and values (notably, the market, but also our research and not-for-profit and governmental ecosystems)?
2.  What types of DTD would be important, but are not being adequately supported by today's institutions and values? Why are they not being adequately supported?
3.  What changes are needed to better align our values and institutions with DTD? In particular, what are the barriers to that alignment, and how can those changes be achieved?

Let me make a few higgledy-piggledy observations, addressed at some mix of these three questions[27](https://michaelnotebook.com/dtd/index.html#fn27). This is (of course) not at all complete. Particularly acute is the omission of conventional safety engineering.

1.  **Space colonies:** In particular, with a view toward establishing permanent, self-sustaining (or near self-sustaining) colonies elsewhere in the solar system, and eventually other solar systems. Such colonies will be isolated from some (though not all) destructive technologies on earth, and so provide a degree of insulation from risk. (They will also bring enormous benefits.) I am rather surprised this is not an established cause area for Effective Altruism. It's true that to actually create a colony elsewhere is beyond the means of all but the very largest economic entities on earth today. But that does not mean that there aren't major contributions to be made far more inexpensively, especially in underfunded niches. People often criticize the idea of such colonization as "impossible", or economically infeasible. A common example to make the point is Antarctica: almost no-one lives in Antarctica, and yet Antarctica is likely a far more accessible and for more hospitable environment than almost anywhere else in the solar system. From a 10- and perhaps even a 50-year viewpoint that seems correct. From a thousand-year timescale it seems almost certainly false. Indeed, safety aside, even for purely economic reasons I believe there are extremely valuable niches elsewhere, niches that will drive colonization. The asteroid belt; near-earth asteroids; Jupiter; the moon; zero g; hard vacuum. Perhaps one or even several of these will be an economic bust. But over the long run I believe several will each help create (at least) tens of trillions of dollars in value, and also simply greatly improve human life. We will find ways of making space produce wealth for our common humanity, and the colonization of space will thrive, to the long-run benefit of humanity.
2.  **DTD VC:** It's interesting to think about what a VC focused on DTD could achieve. It may have some advantages in building certain types of expertise, founder relationships, and LP relationships. Also interesting to think about Pigouvian taxes as inputs to the fund. Somewhat analogous to the proposals to use carbon taxes to fund carbon removal.
3.  **AI Safety Watch:** Along the lines of [Retraction Watch](https://retractionwatch.com/), an insightful (and relentless) blog that monitors and analyzes all AI safety incidents, and builds awareness and an online presence. Over the long-term ideally it would build a database of incidents, much as is already done for security incidents.
4.  **Increasing the rate of research field creation:** It's been challenging to establish many fields of risk and xrisk as worthwhile subjects of research. In the field of metascience, interventions have been proposed to modify science so that it increases the rate of research field creation[28](https://michaelnotebook.com/dtd/index.html#fn28). These ideas could be further developed and pushed into action.
5.  **Brain-computer interfaces:** Here, it seems likely that how beneficial (or not) this is depends on certain crucial choices made during technology development. Who owns it? How centralized is it? Are there open source versions? To what extent are there single points of failure? To what extent will it amplify inequality? To what extent will it enable companies are governments to control or influence our thinking? How secure will it be? Choices made today may make a large difference to the eventual answers to these questions. A thoughtful piece on differential neurotechnology development has been prepared by Cvitkovic[29](https://michaelnotebook.com/dtd/index.html#fn29); cf. also McCaleb[30](https://michaelnotebook.com/dtd/index.html#fn30).
6.  **Mechanisms for solving public goods and collective action problems:** Including dominant assurance contracts; the Vickrey-Clarke-Groves (Liberal Radical) mechanism; and Ostrom's work on common pool resources.
7.  **ASI-for-DTD:** I earlier above the idea that ASI may be essential as an enabler of work on DTD. It's interesting to ponder starting a company focused on ASI-for-DTD, or perhaps related subjects like ASI-for-science-and-technology. DeepMind and its spinoff, Isomorphic Labs, have some orientation this way.

## Takeaways

These notes were somewhat frustrating to write. They contain not so much solutions as better-formulated problems. Still, that's valuable, and I made a little progress toward solutions. Key points to take away:

-   The strong Alignment Problem: the problem of aligning a liberal society so it strongly prioritizes DTD, while preserving liberal values. This automatically entails not just a solution of the AI alignment problem, but also better addresses the underlying causal and systemic issues, including the stability problem for ASI safety (e.g., even if you can align an ASI that doesn't prevent the easy creation of other non-aligned ASI's).
-   Ideas deserving more development, and bearing on the solution of the strong Alignment Problem: provably beneficial surveillance; DTD VC; AI Safety Watch; DTD taxes; how to 10x the rate of research field production; public goods production and collective action problems in general
-   Space colonization is undervalued as DTD
-   Is ASI-for-DTD the essential route?
-   Is the Great Stagnation really the beginning of the Great Realignment?
-   Any posthuman successors will also need to solve the strong Alignment Problem; indeed, if anything it will likely get worse; we don't get to evade the strong Alignment Problem, at best we can punt it down the road
-   There are some very significant problems with the notion of DTD, notably: the risk sign problem (it's surprisingly hard to tell whether a technology increases or decreases risk), and the fact that it's only in part a property of a technology, as opposed to other contingent factors. Is there a better replacement notion?
-   A society gets what it amplifies most. How can we better amplify differential technological development?

## Note added January 19, 2024

This note is to make a crucial point which was not distilled sharply enough in the first release of these notes, on January 13, 2024. Our civilization doesn't prioritize DTD strongly enough – and so will fail to solve the Alignment Problem – because the safety loop is working poorly for many important classes of risk. It works well when consumers bear highly legible risks. But it often works poorly when a technology damages the commons, affects third parties, or when dealing with long-term, illegible, diffuse, or hard-to-measure risks. That's why I discussed tools for collective action, provably beneficial surveillance, and related ideas, as partial solutions in that direction. If a society gets what it amplifies most, then we need to find ways of amplifying DTD much more forcefully, to address risks where the safety loop works poorly. That's what is required for us to address the Alignment Problem. So, again: how can we much more rapidly amplify differential technological development?

## Citation information

In research work, please cite this as: Michael Nielsen, "Notes on Differential Technological Development", [https://michaelnotebook.com/dtd/index.html](https://michaelnotebook.com/dtd/index.html) (2024).

## Acknowledgments

Thanks to Josh Albrecht, Fawaz Al-Matrouk, Alexander Berger, David Bloomin', Vitalik Buterin, Brian Christian, Patrick Collison, Susan Danziger, Laura Deming, Anastasia Gamick, Tim Hwang, Andy Matuschak, Hannu Rajaniemi, and Albert Wenger for conversations about these subjects.

## Footnotes

* * *

1.  I will use the abbreviation DTD for the most part through these notes. I've noticed in myself and some others a tendency to say "differential technology development" rather than "differential technological development", perhaps because "technology" is a concrete noun (albeit being used as part of an adjectival phrase to modify "development"), where "technological" is a rather abstract adjective. Given my use of DTD the point is perhaps moot, but I must admit a strong preference for the term "differential technology development".[↩︎](https://michaelnotebook.com/dtd/index.html#fnref1)

2.  Nick Bostrom, "Existential Risks: Analyzing Human Extinction Scenarios and Related Hazards" (2002).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref2)

3.  There is far too much work to even give a brief overview here. A broad piece that I am especially partial to is: Vitalik Buterin, "My Techno-Optimism" ,[https://vitalik.eth.limo/general/2023/11/27/techno\_optimism.html](https://vitalik.eth.limo/general/2023/11/27/techno_optimism.html) (2023). My notes are at: [https://michaelnotebook.com/vbto/index.html](https://michaelnotebook.com/vbto/index.html).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref3)

4.  Michael Nielsen, "Notes on the Vulnerable World Hypothesis", [https://michaelnotebook.com/vwh/index.html](https://michaelnotebook.com/vwh/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref4)

5.  See, e.g., Cagan Koc and Jennifer Jacobs, "US Pressured Netherlands to Block China-Bound Chip Machinery", [https://www.bloomberg.com/news/articles/2024-01-01/us-pushed-asml-to-block-chinese-sales-before-january-deadline](https://www.bloomberg.com/news/articles/2024-01-01/us-pushed-asml-to-block-chinese-sales-before-january-deadline) (2024).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref5)

6.  A good discussion of this is: Neil Postman, "Technopoly" (1992). Note that this [sympathy to technocracy](https://twitter.com/michael_nielsen/status/1744783594424500721) is widely spread among people with apparently very different views of ASI: "e/acc, doomers, EAs, VCs, researchers, safety people, AI startups, _etc_. All are united by a sense of agency over, and broad belief in, technology, even when they disagree on details of how it is best developed. And yet many people in the world don't have that same sense of agency over technology; in some cases, they have little agency, technology is something done to them".[↩︎](https://michaelnotebook.com/dtd/index.html#fnref6)

7.  Michael Nielsen, "Notes on the Vulnerable World Hypothesis", [https://michaelnotebook.com/vwh/index.html](https://michaelnotebook.com/vwh/index.html) (2023). Cf. also Albert Wenger's talk [https://www.youtube.com/watch?v=lBUPEv6KlnM](https://www.youtube.com/watch?v=lBUPEv6KlnM), David Brin's "The Transparent Society" (1998), and ongoing work on sousveillance.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref7)

8.  Nick Bostrom, "The Vulnerable World Hypothesis", [https://nickbostrom.com/papers/vulnerable.pdf](https://nickbostrom.com/papers/vulnerable.pdf) (2019).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref8)

9.  Brain hackers will be a thing.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref9)

10.  There is a largw literature on this. A brief and recent lay survey is: Alicea Hibbard, [Investigating the Smallpox Blanket Controversy](https://asm.org/articles/2023/november/investigating-the-smallpox-blanket-controversy) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref10)

11.  Robock and collaborators have written many articles on concerns about geoengineering. A helpful and relatively recent summary is: Alan Robock, "Benefits and Risks of Stratospheric Solar Radiation Management for Climate Intervention (Geoengineering)" (2020).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref11)

12.  I have, for example, previously argued that the notion of a probability of doom may incorrectly bias people – including central people – toward a fatalistic view of ASI development. I've also been one of many people arguing that certain AI alignment and safety work has plausibly sped up ASI, and may have had a negative impact. Both points are discussed in: Michael Nielsen, "Notes on Existential Risk from Artificial Superintelligence", [https://michaelnotebook.com/xrisk/index.html](https://michaelnotebook.com/xrisk/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref12)

13.  Marshall McLuhan, "Understanding Media: The Extensions of Man" (1964).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref13)

14.  Stuart Russell, at: [https://www.edge.org/conversation/jaron\_lanier-the-myth-of-ai](https://www.edge.org/conversation/jaron_lanier-the-myth-of-ai) (2014).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref14)

15.  Nate Soares and Benya Fallenstein, "Agent Foundations for Aligning Machine Intelligence with Human Interests: A Technical Research Agenda" (2014).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref15)

16.  AI companies often argue that to make progress on alignment it is necessary for them to build and align better systems. They're correct for the conventional formulation of the AI Alignment Problem. But this is also likely to be strongly destabilizing, since it will lead to much more capable and non-aligned systems. In this sense, they are arguably working against the solution of my reformulation of the Alignment Problem.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref16)

17.  When I say this, people sometimes retort with comments pointing to recent high profile instances of problems, e.g., the run of problems Boeing has had. This unwittingly demonstrates the point. Boeing is (correctly!) being held to an extraordinarily high standard, one so that even incidents resulting in no loss of life receive very wide and extended coverage. By contrast, automobile manufacturers receive almost a free pass by comparison, and consequently automobiles much more dangerous. To be clear: they _also_ benefit from this safety feedback loop, but it's not as strong.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref17)

18.  This paragraph adapted from: Michael Nielsen, "Notes on Existential Risk from Artificial Superintelligence", [https://michaelnotebook.com/xrisk/index.html](https://michaelnotebook.com/xrisk/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref18)

19.  Elinor Ostrom, among others, has emphasized the difficulty of doing this for hard-to-measure-or-demarcate harms: Elinor Ostrom's, "Governing the Commons" (1990). Thanks to Albert Wenger for reminding me of this issue.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref19)

20.  It's tempting to use extinction rates among animal species to think about extinction risk for humans. But humans are the _only_ species that has any kind of non-trivial safety loop. It is, in my opinion, one of the most interesting differences between humans and other species.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref20)

21.  Tyler Cowen, "The Great Stagnation" (2011). Cf also Peter Thiel, "The End of the Future" [https://www.nationalreview.com/2011/10/end-future-peter-thiel/](https://www.nationalreview.com/2011/10/end-future-peter-thiel/) (2011).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref21)

22.  This framing was suggested to me in part by Patrick Collison.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref22)

23.  A lovely evocation of this is by Stewart Brand: [https://www.youtube.com/watch?v=YIZw5cT84ZM&ab\_channel=AhmedKabil](https://www.youtube.com/watch?v=YIZw5cT84ZM&ab_channel=AhmedKabil)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref23)

24.  Certainly, I don't want to see society frozen as is; I love the idea of change and growth. As Douglas Adams has wrily observed, it's arguable that the automobile is already the dominant lifeform on Earth. We can imagine posthuman futures in which current humanity's descendants live happily as part of a society of minds vastly more diverse than today, a plurality of posthumanities.[↩︎](https://michaelnotebook.com/dtd/index.html#fnref24)

25.  Michael Nielsen, "Brief remarks on some of my creative interests", [https://michaelnotebook.com/ti/index.html](https://michaelnotebook.com/ti/index.html) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref25)

26.  See, e.g., Steven Pinker, "The Better Angels of Our Nature" (2011).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref26)

27.  Cf. also: AJ Kourabi, "What is differential technological development?", [https://web.archive.org/web/20230224162351/https://www.ajkourabi.com/writings/what-is-differential-technological-development](https://web.archive.org/web/20230224162351/https://www.ajkourabi.com/writings/what-is-differential-technological-development)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref27)

28.  Michael Nielsen and Kanjun Qiu, "A Vision of Metascience", [https://scienceplusplus.org/metascience/index.html](https://scienceplusplus.org/metascience/index.html) (2022).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref28)

29.  Milan Cvitkovic, "Cause Area: Differential Neurotechnology Development", [https://forum.effectivealtruism.org/posts/Qhn5nyRf93dsXodsw/cause-area-differential-neurotechnology-development](https://forum.effectivealtruism.org/posts/Qhn5nyRf93dsXodsw/cause-area-differential-neurotechnology-development) (2022)[↩︎](https://michaelnotebook.com/dtd/index.html#fnref29)

30.  Jed McCaleb, "We Have to Upgrade", [https://www.lesswrong.com/posts/vEtdjWuFrRwffWBiP/we-have-to-upgrade](https://www.lesswrong.com/posts/vEtdjWuFrRwffWBiP/we-have-to-upgrade) (2023).[↩︎](https://michaelnotebook.com/dtd/index.html#fnref30)

## Comments