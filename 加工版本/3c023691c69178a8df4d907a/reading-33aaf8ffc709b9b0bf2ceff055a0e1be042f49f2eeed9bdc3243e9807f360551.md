# Was World War II good for economic growth?

---

## 正文

World War II 带来了 US federal R&D spending 的 substantial increase，可以将其视为一个 imperfect\[1\] natural experiment，用来观察当一个国家从相对「laissez faire」（有许多保留条件）的 scientific support model 转向有 heavy government involvement 的模式时，会发生什么。此外，WWII 可能还意味着一项全社会被迫参与的 learn by doing program。有人认为，WWII caused GDP growth 高于没有它时的水平。在这里，我将考察这些 claims。

下面这张 chart 展示了 US 的 federal R&D expenditures 的 evolution，再下面的一张则展示了各个 fund research 的 sectors 的 expenditures 占 GDP 的 %。

![Function%3B.jpg (961Ã720)](https://nintil.com/old_assets/2019/03/06-1.jpg) 来源：AAAS

![U.S. R&D by GDP.png（美国 R&D 占 GDP 的比例）](https://nintil.com/old_assets/2019/03/06-2.png)

1953 年以前的 time series 很难找到，但 _[Research - A national resource（研究——一种国家资源）](https://archive.org/details/researchanationa1940unitrich)_（1939）中说，「Federal government 在 1937 年 spent 124 million dollars」，1938 年则为 108（Table 1，第 66 页）。

![2019-02-25 20-55-43 的截图.png](https://nintil.com/old_assets/2019/03/06-3.png)

根据 [BEA](https://apps.bea.gov/iTable/iTable.cfm?reqid=19&step=2#reqid=19&step=2&isuri=1&1921=survey)，1937 年的 GDP（current dolars，gross）为 93bn dollars，因此 federal R&D expenditure 应为 GDP 的 0.13%，这 substantially less than AAAS 给出的 1953 年的 figure（0.71%）。

作为比较，[在 Bush (1945) 中](https://twitter.com/james_ough/status/1100283875182501888)，可以根据 aggregate industry+federal expenditures 得到 relative numbers：1940 年的 GDP 为 102.9bn，因此得到 0.3%，而在 1953 年，这一 number 为 1.4%。在 industry/federal split 均等的 assumption 下，1940 年的 federal expenditures 应当也在大致相同的 order of magnitude（即使我们 assume 60% 的 share，也会得到一个 similar number），这意味着 expenditure 的 increase 发生在战争的较晚阶段，而且来得相对突然。

即使 GDP 或 R&D figures 可能略有偏差，5.46x 的 difference 也很难掩盖，除非在什么 counts、什么不 counts 为 R&D 的 accounting methodology 上存在重大 differences，因此，发生过 sharp increase 这一事实看来是真的。

然而，R&D funding 在 16 年间的 5x increase（即使以相对于 GDP 的 relative % 衡量），并没有对 GDP growth 或 innovation 起到多大作用——至少我将如此论证。

可以依据 Robert Gordon 的_The Rise and Fall of American Growth_，为相反的 view——即 war 有一些 good effects——建立 case。书中有一张 plot（感谢 Matt Clancy 提醒），展示了 US GDP 和 output 的变化，似乎表明它们持续 departure 于 pre-war trend。

![gordon\_gdp.png](https://nintil.com/old_assets/2019/03/06-4.png) 来源：The Rise and Fall of American Growth (Gordon, 2016)

千篇一律的 endogenous growth models 的 predictions 恰好会 predict 这一点：R&D funding 的 step-change 会 lead to rate of growth 的 gradual increase，随后 stabilisation。

然而，如果我们选择不同的 time frame 来做 regression，就会得到不同的图景；

![usa\_gdp\_growth（美国 GDP 增长）](https://nintil.com/old_assets/2019/03/06-5.png)

World War II 是 pre-war trend 的 continuation，还是战前那些年本就 unusual？战前那些年是 [the roaring twenties（咆哮的二十年代）](https://en.wikipedia.org/wiki/Roaring_Twenties)，恰逢一场以 Great Depression 告终的 economic bubble，因此，上面的 figure 可能不太公平，但也不算太不公平：那些年也有许多重大的 discoveries 和 inventions，如果它们最终在 WWII 期间及之后得到 application，也会暂时 raised growth。

war 开始时，economy 正从 bust 中 recovering，而战后时期恰逢 [trade 的 expansion](https://ourworldindata.org/trade-and-globalization)：许多国家在 WWII 之后也 grew faster。如果这些 counfounders 还不够，那段时期也正是 baby boom 时期。其他地方的这种 expansion，是否源于 US 的 diffusion of technology，而后者又是 war preparation 的结果？

仅仅观察没有其他 controls 的 time series，无法解决这个问题，因此，也许应该转而看看 TFP。

那么，war 对 TFP 做了什么？Gordon 还认为，World War II caused TFP 的 rate of growth 的 increase：

![gordon\_tfp（Gordon 的 TFP）](https://nintil.com/old_assets/2019/03/06-6.png) 来源：The Rise and Fall of American Growth (Gordon, 2016)

为什么 war 会 cause 更多 TFP growth？

Robert Gordon 在书中认为，WWII 教会了整个 economy 如何 operate efficiently，而 manufacturing sector 在大量生产 Liberty ships 和其他 war material 时，也通过实践学到了 better practices。war 结束后，这些 knowledge 得以 retained，随后 applied 到 civilian sector。Gordon 在这里毫不含蓄，他甚至说：

> 本章最新颖之处，在于它提出了这样一个 assertion：World War II 本身或许是 Great Leap 最重要的 contributor。\[Great Leap 是指 1920 至 1970 年间 strong TFP growth 的时期\]

Gordon 承认，在他的书出版之前，已有一些 arguments 被用来支持相反的 view。尤其是，war _restricted civil investment，并 retarded private capital accumulation。_

[Tyler Cowen (2014)](https://www.nytimes.com/2014/06/14/upshot/the-lack-of-major-wars-may-be-hurting-economic-growth.html) 认为，war 会给 governments 注入紧迫感，以至于：

> 仅仅是 war 的 possibility，就会让 governments 将注意力集中在做对一些 basic decisions 上——无论是 investing in science，还是仅仅 liberalizing the economy。这种专注最终会 improving 一个国家的 longer-run prospects。$$...$$ nuclear power、computer 和 modern aircraft 等 Fundamental innovations，都曾受到一个急于 defeat Axis powers、或后来急于赢得 Cold War 的 American government 的推动。Internet 最初被 designed 来帮助这个国家 withstand 一场 nuclear exchange，而 Silicon Valley 则起源于 military contracting，而不是今天的 entrepreneurial social media start-ups。Soviet 对 Sputnik satellite 的 launch 激发了 American 对 science and technology 的兴趣，从而有益于后来的 economic growth。

我上次研究这个话题[是在 2015 年](https://nintil.com/2015/08/28/is-war-necessary-for-economic-growth-a-review-and-some-aviation-history-love/)，当时漏掉了上面这篇文章。读到这里，我的 conclusion 应该不会让你惊讶：war 或 defense R&D 并不利于 economic growth，这也是 Handbook of the Economics of Innovation 总体上表达的 view。此后，没有人再对 literature 进行 _systematic_ 考察，也没有关于 war、military R&D expenditures 与 productivity 或一般意义上的 growth 之间 relation 的相关 papers。不过，Alexander Field 有一系列 papers 值得研究，稍后再谈。

在我 2015 年的文章中，我讨论了 Gordon 引用的一些 examples，尤其是与 aerospace 有关的那些；例如，jet engines 在战前就已处于 development 之中，而与 pre-war trend 相比，airplanes 在战争期间[并没有 get better](https://nintil.com/2016/02/09/aircraft-progress-before-and-after-world-war-ii/)，Field 在他的 paper 中也指出了这一点。另一个 example 是 Rockets，同样可以认为，war 将 rocketry 的 development 推向了 larger（military satellites 通常 larger 且 heavier）、也 more expensive（defense contractors 按 cost plus basis 获得报酬，这并不 incentivise cost savings）的 rockets，而不是着重于对 commercial purposes 更 crucial 的 cost reductions；我认为，这是 subsidising R&D 导致 negative consequences 的一个奇特 case……当然，前提是不计 military benefits！

Nuclear power 是另一个很好的 example，说明为什么 war 不利于 innovation：由于其 military origins，Nuclear power 会让人联想到 mushroom clouds 和 radiation poisoning，而同样的 military origin 可能也使这项 technology 的 development 偏向更 suited for nuclear submarines、而非 large scale power generation 的 designs，正如我关于 nuclear power 的 progress 的文章[末尾](https://nintil.com/progress-in-nuclear-energy/)所引用的文章中提到的那样。设想另一种 counterfactual：nuclear power 的 developed 速度更慢，但更重视 safety 和 cost。adoption 难道不会更 widespread 吗？

虽然我并不反对 Cowen 提出的 argument——事实上，在我最近关于 [skyscraper construction speeds](https://nintil.com/2018/10/07/building-skyscrapers-and-spending-on-major-projects/) 的文章中，我接受了一个 closely related argument——但也必须 factor in war 可能 induce 的其他 effects，它们可能对 productivity 和 innovation 是 deleterious 的。

[Alexander J. Field (2019)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3110832) 的一篇 paper 持相反的立场。这篇 paper 已经写了相当一段时间（我每次看，它都还在变长），如今已有 83 页；他在其中 analyses 同一 time period 内 TFP 的 growth，发现 1929-1941 年间的 TFP growth 比 Gordon 所说的 greater（Table 3）（3% vs 1.82%），而在 1941-1948 年的战争时期则 far lower（2.01% vs 3.39%））。两位 authors 都不只是 calculating 相关 value，也都努力\_解释原因， \_此外，Field 还解释了通过哪些具体方式，可以从他的 numbers 得到 Gordon 的 numbers，反之亦然。

我为什么站在 Field 这一边？有一系列 heuristics 对他有利：

-   这篇 paper 发表得更晚，因此他有时间 incorporate 在 Gordon 的书出版后发表的一些 work。
-   目前没有可见的 criticisms 针对它
-   Gordon 本人也帮助了 Field 撰写这篇 paper，与他分享了自己的 calculation（也降低了误解 Gordon 的 risk）
-   Field 还解释了 lead to 他的 result 的具体 methodological differences，并逐一给出理由
-   此外，我基于此前对相关领域的 research，对这个问题已有 strong prior view，尤其是关于 military spinoffs 向 civilian sector 的 transferability 的 research，以及总体上我在关于 war 和 growth 的文章中写过的内容。
-   本文下一节与他的 arguments 相吻合

我认为他有多正确？Gordon 或其他人有多大可能会证明 WWII 实际上有利于 growth？我认为可能性不大；用 numbers 来说，我会给予「Field 的 assessment 大体正确」80% 的 credence。

### 直接考察 outputs

Nintil 的老读者会记得我的「[没有 Great Technological Stagnation](https://nintil.com/2016/04/25/no-great-technological-stagnation/)」，我在其中主张，各种 technologies 的 rate of improvements 所呈现的 trends 似乎都表现良好。不过，我们也可以考察，1939-1945 年间是否出现过 acceleration，或者 1939 年之前的时期相较于 1945 年之后的时期是否出现过 acceleration。能够追溯到那么久以前的 time series 并不多，但许多在那段时期都照常延续：batteries、energy transportation、automotive engines，以及 electricity 向 light、heat 或 motive power 进行 conversion 的 efficiency。可以论证，Moore's law 在 units of compute per $ 方面代表了一次 trend break：早期的 computers 在战争前后出现，在战争期间投入使用，并在战后获得了大量投资。但是：如果考察足够多的 trends，你必然会发现至少有一个在你所关注的时期有所改善。

有人可能会回应：但这完全没有告诉我们 breakthroughs 的情况，internet 并没有这样的 trends，或者说，touchscreens 的出现也没有。对这些进行 Measuring 更困难，但那些尝试进行 measuring 的研究，例如 [Huebner (2005)](http://accelerating.org/articles/InnovationHuebnerTFSC2005.pdf)，并没有发现任何类似 acceleration 的现象。

或许应该考察对 health 的 impact，因为那笔额外 funding 中有很大一部分流向了 health research。但即便如此，我们也没有发现 life expectancy 的 [increase](https://ourworldindata.org/life-expectancy)，或 cancer rates 的 [rate of decline](https://ourworldindata.org/cancer#cancer-over-the-long-run) 出现变化（这方面的 data 很少，因此我们能说的也不多）。

Robert Gordon 在书中最终表示，经过 broadly measured 的 innovativeness 已经 declined；根据如何 count 这种 innovativeness，也可以得出它保持不变的结论，但如果把他书中的 aggregate evidence 考虑在内，在我看来，acceleration 似乎是这些选项中最不太不可能的一个。

[Patents](https://www.yalelawjournal.org/article/the-first-patent-litigation-explosion) 可以是另一种——highly imperfect 的——indirect measure，而我们再次发现，相较于此前的时代，战争时期并没有 induce 变化。

那么，为什么那些额外的 R&D 没有 translate into growth？

可以主张，一开始就不应该指望这样的 increase 对 TFP 产生多大作用，因为其中很大一部分是 defense R&D，而非 civilian R&D（[Mowery & Rosenberg, 1998](https://books.google.co.uk/books?id=hwAOh0ZliBoC&pg=PA26&lpg=PA26&dq=us+r%26d+before+wwii&source=bl&ots=9GjFUt6L4-&sig=ACfU3U2YMN7JVFaItChXlXFXJUziaihOMQ&hl=en&sa=X&ved=2ahUKEwjU6PaU6tfgAhV0ShUIHdqpDusQ6AEwCHoECAYQAQ#v=onepage&q=us%20r%26d%20before%20wwii&f=false)）。在我看来，这是 explanation 的关键，与 Field 在其 paper 中的论述及 consensus view 一致。Military R&D 与 civilian R&D 为 resources 而 competes，却没有多少有用的 spillovers。当人们被问及 military R&D 时，或许会想到 ARPANET，但从全局来看，那些 projects 规模很小，也很便宜。不妨想想那些大型 defense contractors，它们在 developing 更快的 fighter jets 和更强大的 bombs。现在，想象所有那些 engineers 和 scientists 都在 building cars 和更好的 batteries。

又或者，某些情况下，向 research 投入更多 money，并不会 result in 更多或更好的 research，而只会让 engineers 和 scientists 获得更高报酬，正如 [Goolsbee (1998)](https://www.nber.org/papers/w6532) 所发现的。这是说得通的：如果不是每个人都有能力从事 high level research，那么，只有在相关 research 受到 material resources 的 constraint 时，往里面投入 money 才会有所帮助。但如果并非如此，你得到的就会是更高的 prices，而不是更大的 amount。这种 phenomenon 是否依然存在，是另一篇文章的话题。

又或者，那是一种 demographic effect。endogenous growth models 的一个有趣 predictions 是，economy 的 rate of growth 与 population 的 rate of growth 之间是 strongly tied 的（更多 people，更多 ideas）。WWII 之后的那些年，也是 [baby boom](https://en.wikipedia.org/wiki/Baby_boom#/media/File:US_Birth_Rates.svg) 的那些年，而且从 1940 到 1970 年，americans 的 [median age](https://www.census.gov/newsroom/cspan/1940census/CSPAN_1940slides.pdf) 实际上 decreased 了。近来，恰恰是 demographics 正被作为近期 TFP growth slowdown 的一个 potential cause 来探索（最近在 Nintil 的链接汇总中介绍过的 [Hopenhayn et al., 2018](https://www.nber.org/papers/w25382)，以及 [Vandenbroucke](https://www.stlouisfed.org/publications/regional-economist/fourth-quarter-2017/baby-boomers-productivity) 和 [Ozimek et al.](https://twitter.com/ModeledBehavior/status/1102922678904782848)）

我没能找到一篇跨 countries、明确对我提到的所有因素进行 controls 的 paper，因此，我们只有我在上面呈现的那些零散 evidence。总体而言，我确实认为，我原先关于 WWII（以及 war、preparation for war 和 military R&D spending）不利于 innovation 的 prior belief 依然成立；不过，如果你不同意，请在评论中告诉我，并说明原因，我会据此修订这篇文章。

（上面那个 plot 的 code 位于 [Open Nintil](https://github.com/jlricon/open_nintil) repo 中）

\[1\] 之所以是 Imperfect，是因为很难对上面所有 factors 进行 controlling。另外，那也是一次非常 idiosyncratic 的 R&D expenditure boost。另一个同样值得 studying 的相关 question 是，NIH budget 的 increases 对各种 metrics 产生了什么作用。

t

-   **[2019 年 3 月 11 日当周链接汇总 – Verywhen](https://verywhen.wordpress.com/2019/03/10/links-for-the-week-of-march-11th-2019/)** 2019-03-10T16:27:38Z

    > $$…$$ World War II 对 economic growth 有益吗？$$…$$

-   **[周六杂项链接 - Marginal REVOLUTION](https://marginalrevolution.com/marginalrevolution/2019/03/saturday-assorted-links-202.html)** 2019-03-09T18:01:07Z

    > $$…$$ 1. World War II 对 longer-run economic growth 有益吗？$$…$$

-   **[Rational Feed（理性信息流）– deluks917](https://deluks917.wordpress.com/2019/03/07/rational-feed-532/)** 2019-03-07T17:49:44Z

    > $$…$$ Artir 的《World War II 对 Growth 有益吗》——WW2 很可能并未 significantly boost economic growth。$$…$$

-   ![notpeerreviewed](https://www.gravatar.com/avatar/8bb0658b2da42a3537166164c8b5d5f1?d=identicon) **[notpeerreviewed](http://notpeerreviewed.wordpress.com/)** 2019-03-07T02:20:22Z

    > 我能稍微澄清一下这个问题吗？如果我漏掉了什么，先说声抱歉。我看过的每一张 GNP chart 都显示，它在 Great Depression 期间 plummets，并在 WWII 期间 recovers。那么，在某种意义上，WWII 期间的 growth rates 显然比此前更好，但这也可能是 depression 恰好发生在战争之前所造成的 artifact。那么，问题是不是在于，GNP 在战后是否走上了新的 trajectory？

-   ![Artir](https://www.gravatar.com/avatar/77fac16b1eefeb9c6dcd9d0cfa2ea692?d=identicon) **[Artir](http://)** 2019-03-07T19:14:09Z

    > 嗯，正如你所说，讨论的是 WWII 的 net causal effect；可以主张，这种 effect 会因 starting conditions 而不同。

-   ![Per Kurowski](https://www.gravatar.com/avatar/17ba50b52ee3b3014a9adb42803b58dd?d=identicon) **[Per Kurowski](http://perkurowski.wordpress.com/)** 2019-03-09T18:48:28Z

    > 对我而言，我的父亲是一名波兰士兵，乘坐第一班前往奥斯维辛的火车抵达那里，身上刺有 #245，后来由美国人在布痕瓦尔德解放，而不是由俄国人在奥斯维辛解放。作为他的儿子，我完全可以证明，这在各个方面都有益于我的成长。

-   ![Steohen](https://www.gravatar.com/avatar/6add8a8cce1c764711fad6004bd49258?d=identicon) **Steohen** 2019-03-09T22:05:27Z

    > “World War II 对 economic growth 有益吗？”
    > 
    > 我真的认为你需要重新表述这个问题。它对 USA 的 economic growth 有益吗？
    > 
    > 它对 USA 以外世界其他地区的 economic growth 有益吗（是的，这些地方确实存在）。加拿大？拉丁美洲？南非？埃及？印度？澳大利亚？瑞典？可能上述所有地方都是，还不止这些。
    > 
    > 英国？法国？意大利？德国？苏联？中国？日本？对这些地方都没有益处，而且还不止这些。
    > 
    > 应当追问，USA 的 economic growth 在多大程度上 dependent on 世界其他地区的 economic deterioration。

-   ![Anny Omous](https://www.gravatar.com/avatar/f85096104474ca9ef5620405026bc5fa?d=identicon) **Anny Omous** 2019-03-07T12:33:18Z

    > 你考察过 war displacement 中的 civilian 和 military 人员吗？这些 numbers，以及这些人此前接受的 education 对 research and development 的贡献，或许会为你提供另一个视角，并改变你的 numbers。

_Ricón, José Luis, “Was World War II good for economic growth?”, Nintil (2019-03-06), available at https://nintil.com/was-world-war-ii-good-for-growth/._

---

## 原文

World War II supposed a substantial increase in US federal R&D spending, and it may be regarded as an imperfect\[1\] natural experiment of what happens when you transition a country from a relatively "laissez faire" (with many caveats) model of scientific support to one where there is heavy government involvement. In addition to that, WWII may have implied a societywide forced learn by doing program. It has been argued that WWII caused GDP growth to be higher than it would otherwise have been. Here I examine those claims.

This chart below shows the evolution of  federal R&D expenditures in the US, and the one below that shows expenditures as a % of GDP for the various sectors that fund research.

![Function%3B.jpg (961Ã720)](https://nintil.com/old_assets/2019/03/06-1.jpg) Source: AAAS

![U.S. R&D by GDP.png](https://nintil.com/old_assets/2019/03/06-2.png)

It's hard to find time series from before 1953, but in _[Research - A national resource](https://archive.org/details/researchanationa1940unitrich)_ (1939), says that "the Federal government spent in 1937 124 million dollars", and 108 in 1938 (Table 1, pg. 66)

![Screenshot from 2019-02-25 20-55-43.png](https://nintil.com/old_assets/2019/03/06-3.png)

From [BEA](https://apps.bea.gov/iTable/iTable.cfm?reqid=19&step=2#reqid=19&step=2&isuri=1&1921=survey), GDP (current dolars, gross) in 1937 was 93bn dollars, and so federal R&D expenditure would have been 0.13% of GDP, which is substantially less than the figure from AAAS for 1953 (0.71%).

As a comparison, [in Bush (1945)](https://twitter.com/james_ough/status/1100283875182501888), aggregate industry+federal expenditures from which we can get relative numbers, in 1940 GDP was 102.9bn, so we get 0.3%, while in 1953, that number was 1.4%. Under an assumption of an even industry/federal split, federal expenditures would have been around the same order of magnitude in 1940 (even if we assume a 60% share, we get a similar number), meaning that the increase in expenditure happened later in the war, in a relatively sudden way.

Even when GDP or R&D figures may be slightly off, the 5.46x difference is hard to hide except with heavy differences in accounting methodology for what counts and doesn't as R&D, so the fact that there was a sharp increase seems true.

Yet, a 5x increase during 16 years in R&D funding in (Even as a relative % over GDP), did not do much to GDP growth or innovation, or so I'll argue.

A case for the opposite view, that war had some good effects can be built on top of Robert Gordon's_The Rise and Fall of American Growth_. The book has a plot (ht/ to Matt Clancy) that shows what happened with US GDP and output, seemingly showing that there was a consistent departure from the pre-war trend.

![gordon\_gdp.png](https://nintil.com/old_assets/2019/03/06-4.png) Source: The Rise and Fall of American Growth (Gordon, 2016)

The predictions of cookie-cutter endogenous growth models would predict exactly this: that a step-change in R&D funding would lead to a gradual increase and then stabilisation in the rate of growth.

However, if we choose a different time frame to do our regression we get a different picture;

![usa\_gdp\_growth](https://nintil.com/old_assets/2019/03/06-5.png)

Was World War II a continuation of the pre-war trend, or were the pre-war years unusual? The pre-war years were [the roaring twenties](https://en.wikipedia.org/wiki/Roaring_Twenties), which coincided with an economic bubble that ended in the Great Depression, so the figure above may be unfair, but not too unfair: those were also years where many great discoveries and inventions were made, if they eventually found application during and after WWII that would also have raised growth, temporarily.

The war began as the economy was recovering from the bust, and the post-war period coincided with a period of expansion of [trade:](https://ourworldindata.org/trade-and-globalization) many countries also grew faster after WWII. And if that's not enough counfounders, that period was also the period of the baby boom. Was this expansion elsewhere due to the diffusion of technology from the US, result in turn of war preparation?

This cannot be settled just by looking at time series without other controls, so one might want to look at TFP instead.

And so what did war do for TFP? Gordon also argues that World War II caused an increase in the rate of growth of TFP:

![gordon\_tfp](https://nintil.com/old_assets/2019/03/06-6.png) Source: The Rise and Fall of American Growth (Gordon, 2016)

Why would the war cause more TFP growth?

Robert Gordon in his book argues that WWII taught the entire economy to operate efficiently, and the manufacturing sector learned better practices (by doing) as Liberty ships and other war material was being churned out. After the war, that knowledge was retained, and then applied to the civilian sector. Gordon is not shy here, he goes as far as saying that

> The most novel aspect of this chapter is its assertion that World War II itself was perhaps the most important contributor to the Great Leap.\[The Great Leap being a period of strong TFP growth between 1920 and 1970\]

Gordon acknowledges arguments that, prior to his book, were put forward to support the opposite view. In particular, that war_restricted civil investment, and retarded private capital accumulation._

[Tyler Cowen (2014)](https://www.nytimes.com/2014/06/14/upshot/the-lack-of-major-wars-may-be-hurting-economic-growth.html) argued that war instils a sense of urgency into governments such that

> the very possibility of war focuses the attention of governments on getting some basic decisions right — whether investing in science or simply liberalizing the economy. Such focus ends up improving a nation’s longer-run prospects.\[...\] Fundamental innovations such as nuclear power, the computer and the modern aircraft were all pushed along by an American government eager to defeat the Axis powers or, later, to win the Cold War. The Internet was initially designed to help this country withstand a nuclear exchange, and Silicon Valley had its origins with military contracting, not today’s entrepreneurial social media start-ups. The Soviet launch of the Sputnik satellite spurred American interest in science and technology, to the benefit of later economic growth.

Last time I looked at this topic [was in 2015](https://nintil.com/2015/08/28/is-war-necessary-for-economic-growth-a-review-and-some-aviation-history-love/), and I missed the article above. My conclusion won't surprise you at this stage: war or defense R&D is not good for economic growth, that was the overall view expressed by the Handbook of the Economics of Innovation in general. Since then, there has been no additional _systematic_ looks at the literature, or relevant papers on the relation between war, military R&D expenditures, and productivity or growth in general. But there is a series of papers from Alexander Field that are worth looking into, more in that in a second.

In my 2015 article I addressed some of the examples cited, by Gordon particularly those related to aerospace; as one example jet engines were in development before the war, and airplanes [did not get better](https://nintil.com/2016/02/09/aircraft-progress-before-and-after-world-war-ii/) during the war, compared to the pre-war trend, which is also pointed out by Field in his paper. Rockets, another example, again one could argue that war tilted the development of rocketry towards larger (military satellites tend to be larger and heavier), and more expensive (defense contractors are paid on a cost plus basis which doesn't incentivise cost savings) rockets instead of focusing on cost reductions, more crucial for commercial purposes; an odd case where I think subsidising R&D led to negative consequences... if one discounts the military benefits, of course!

Nuclear power is another good example of why war is bad for innovation: Nuclear power evokes mushroom clouds and radiation poisoning because of its military origins, and that same military origin may have also biased the development of the technology towards designs that are better suited for nuclear submarines, and not for large scale power generation, as mentioned in the article cited [at the end of my article](https://nintil.com/progress-in-nuclear-energy/) on progress in nuclear power. Imagine instead a counterfactual where nuclear power is developed more slowly, but with more attention paid to safety and cost. Wouldn't adoption be more widespread?

While I don't reject the argument that Cowen makes - in fact I embraced a closely related argument in my recent article on [skyscraper construction speeds,](https://nintil.com/2018/10/07/building-skyscrapers-and-spending-on-major-projects/) one also has to factor in other effects that may be deleterious for productivity and innovation that war may induce.

The opposite side is taken by a paper by [Alexander J. Field (2019)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3110832) that has been in the making for some time now (it keeps growing every time I have a look), counting 83 pages long now, where he analyses growth in TFP in the same time period, finding that TFP growth was greater (Table 3) than what Gordon suggests (3% vs 1.82%) in the 1929-1941 period, and far lower in the war period (2.01% vs 3.39%) in 1941-1948). Both authors do not limit themselves to calculating the relevant value, they also make an effort to \_explain why, \_and in addition Field also explains in which specific ways one gets from his numbers to Gordon's or viceversa.

Why do I side with Field? There are a series of heuristics in his favour:

-   The paper came later, so he has had time to incorporate some work released since Gordon's book.
-   There are no criticisms of it available
-   Gordon himself has helped Field with his paper, sharing with him his calculation (And also decreasing the risk of misunderstanding Gordon)
-   Field also explains the specific methodological differences that lead to his result and justifies each of them
-   On top of that, I had a strong prior view on this based on my pre-existing research on related areas, especially on the transferability of military spinoffs to the civilian sector, and in general on what I wrote for my article about war and growth.
-   The next section of my post coheres with his arguments

How right I think he is? How likely is it that Gordon or others will show that actually WWII was good for growth? I think it's unlikely, to put it in numbers, I would give "Field is broadly right in his assessment" a credence of 80%.

### Looking directly at outputs

Long time readers of Nintil will remember my "[No Great Technological Stagnation](https://nintil.com/2016/04/25/no-great-technological-stagnation/)" wherein I argued that trends in the rate of improvements of diverse types of technologies seem to be doing fine. But one can also look at whether or not there was an acceleration in 1939-1945, or if there was an acceleration in the pre 1939 period vs post 1945 period. There aren't that many time series going back into the past, but many continued as usual during that period: batteries, energy transportation, automotive engines, the efficiency of conversion of electricity into light, heat, or motive power. One can make a case for Moore's law representing a trend break with respect of units of compute per $: Early computers arose around the time of the war, were put to use during it, and heavily invested in after it. But: If one looks at enough trends, you are bound to find at least one that gets better in the period you are interested in.

One may reply: But this tells us nothing of breakthroughs, there are no trends for the internet, or say, the appearance of touchscreens. Measuring those is harder, but those attempts are measuring it, like [Huebner (2005)](http://accelerating.org/articles/InnovationHuebnerTFSC2005.pdf) do not find anything like an acceleration.

Perhaps one should look at the impact on health, as a large chunk of that extra funding has gone to health research. But even then, we don't find an [increase](https://ourworldindata.org/life-expectancy)  on life expectancy, a change in the [rate of decline](https://ourworldindata.org/cancer#cancer-over-the-long-run) on cancer rates (there is little data on this one, so we can't say that much).

Robert Gordon in his book ends up saying that innovativeness broadly measured has declined; one could conclude that it has stayed the same depending on how you count that innovativeness, but an acceleration seems the least unlikely of the options to me, if we count the aggregate evidence in his book.

[Patents](https://www.yalelawjournal.org/article/the-first-patent-litigation-explosion) could be another -highly imperfect- indirect measure, and again we find that the war period did not induce a change with respect to the previous era.

So why didn't that extra R&D translate into growth?

It could be argued, that such an increase shouldn't have been expected to do much on TFP to begin with, because large part of it was in defense R&D, not civilian R&D ([Mowery & Rosenberg, 1998](https://books.google.co.uk/books?id=hwAOh0ZliBoC&pg=PA26&lpg=PA26&dq=us+r%26d+before+wwii&source=bl&ots=9GjFUt6L4-&sig=ACfU3U2YMN7JVFaItChXlXFXJUziaihOMQ&hl=en&sa=X&ved=2ahUKEwjU6PaU6tfgAhV0ShUIHdqpDusQ6AEwCHoECAYQAQ#v=onepage&q=us%20r%26d%20before%20wwii&f=false)). This is to me the key of the explanation, in line with what Field argues in his paper and the consensus view. Military R&D competes with civilian R&D for resources, without many useful spillovers. When one is prompted to think of military R&D perhaps ARPANET come to mind, but in the grand scheme of things those projects were small, and cheap. Think instead of large defense contractors, developing faster fighter jets, and more powerful bombs. Now imagine all those engineers and scientists building cars and better batteries.

Or, perhaps there are situations where more money into research do not result in more or better research, but on better paid engineers as scientists, as [Goolsbee (1998)](https://www.nber.org/papers/w6532) found. This makes sense, if not everyone is capable of doing high level research, then pouring money into it will help insofar as the relevant research is constraint by material resources. But if it is not, you'll get higher prices, not a greater amount. Whether or not this phenomenon is still around is a topic for another post.

Or, perhaps it was a demographic effect. One of the fun predictions of endogenous growth models is that the rate of growth of the economy is strongly tied to the rate of growth of population (more people, more ideas). The post WWII years were also the years of the [baby boom](https://en.wikipedia.org/wiki/Baby_boom#/media/File:US_Birth_Rates.svg), and from 1940 to 1970 the [median age](https://www.census.gov/newsroom/cspan/1940census/CSPAN_1940slides.pdf) of americans actually decreased. In recent times, it is precisely demographics thas is being explored as a potential cause of the recent TFP growth slowdown (recently featured in the Nintil links, [Hopenhayn et al., 2018](https://www.nber.org/papers/w25382)  , but also [Vandenbroucke](https://www.stlouisfed.org/publications/regional-economist/fourth-quarter-2017/baby-boomers-productivity) and [Ozimek et al.](https://twitter.com/ModeledBehavior/status/1102922678904782848))

I could not find a paper that controls for everything I've mentioned explicitly across countries, so we only have the disparate pieces of evidence I've presented above. In the aggregate I do think that my prior belief that WWII (and war, preparation for war, and military R&D spending) was not good for innovation holds, but let me know in the comments if you disagree, and why, I will revise this post accordingly.

(The code for the plot above is in the [Open Nintil](https://github.com/jlricon/open_nintil) repo)

\[1\] Imperfect for the difficulty of controlling for all the factors above. Also if was a very idiosyncratic boost in R&D expenditure.  A related question that is also worth studying is what did increases of NIH budget do to diverse metrics .

t

-   **[Links for the Week of March 11th, 2019 – Verywhen](https://verywhen.wordpress.com/2019/03/10/links-for-the-week-of-march-11th-2019/)** 2019-03-10T16:27:38Z

    > \[…\] Was World War II good for economic growth? \[…\]

-   **[Saturday assorted links - Marginal REVOLUTION](https://marginalrevolution.com/marginalrevolution/2019/03/saturday-assorted-links-202.html)** 2019-03-09T18:01:07Z

    > \[…\] 1. Was World War II good for longer-run economic growth? \[…\]

-   **[Rational Feed – deluks917](https://deluks917.wordpress.com/2019/03/07/rational-feed-532/)** 2019-03-07T17:49:44Z

    > \[…\] Was World War II Good For Growth by Artir – WW2 probably did not significantly boost economic growth. \[…\]

-   ![notpeerreviewed](https://www.gravatar.com/avatar/8bb0658b2da42a3537166164c8b5d5f1?d=identicon) **[notpeerreviewed](http://notpeerreviewed.wordpress.com/)** 2019-03-07T02:20:22Z

    > Can I clarify the question slightly? And I apologize if I’ve missed something. Every chart I’ve seen of GNP shows that it plummets during the Great Depression and recovers during WWII. In some sense, then, growth rates were obviously better during WWII than before, but also, that might be an artifact of the depression happening right before the war. Is the question then, whether GNP took on a new trajectory after the war?

-   ![Artir](https://www.gravatar.com/avatar/77fac16b1eefeb9c6dcd9d0cfa2ea692?d=identicon) **[Artir](http://)** 2019-03-07T19:14:09Z

    > Well, it is as you say, as the net causal effect of WWII; one might argue that the effect will be different depending on the starting conditions.

-   ![Per Kurowski](https://www.gravatar.com/avatar/17ba50b52ee3b3014a9adb42803b58dd?d=identicon) **[Per Kurowski](http://perkurowski.wordpress.com/)** 2019-03-09T18:48:28Z

    > For me, as the son of a Polish soldier that arrived tattooed #245 on the first train to Auschwitz and that was then liberate by Americans from Buchenwald, and not by Russians from Auschwitz, I can definitely attest it was good for my growth in all respects.

-   ![Steohen](https://www.gravatar.com/avatar/6add8a8cce1c764711fad6004bd49258?d=identicon) **Steohen** 2019-03-09T22:05:27Z

    > "Was World War II good for economic growth?"
    > 
    > I really think you need to rephrase that question. Was it goof for economic growth in the USA?
    > 
    > Was it good for economic growth in parts of the world outside the USA (yes, they do exist). Canada? Latin America? South Africa? Egypt? India? Australia? Sweden? Possibly all these and more.
    > 
    > UK? France? Italy? Germany? USSR? China? Japan? For none of these, and more.
    > 
    > Query how far economic growth in the USA was dependent on economic deterioration in other parts of the world.

-   ![Anny Omous](https://www.gravatar.com/avatar/f85096104474ca9ef5620405026bc5fa?d=identicon) **Anny Omous** 2019-03-07T12:33:18Z

    > Have you looked at war displacement civilian and military . Those numbers and previous education adding to research and development may give you another perspective and change your numbers.

_Ricón, José Luis, “Was World War II good for economic growth?”, Nintil (2019-03-06), available at https://nintil.com/was-world-war-ii-good-for-growth/._