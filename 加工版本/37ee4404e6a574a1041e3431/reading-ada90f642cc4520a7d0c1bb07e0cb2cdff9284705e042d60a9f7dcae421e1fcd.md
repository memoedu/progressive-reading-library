# 一些 jargon

---

## 正文

几周前，我做了一份 [survey](http://www.benkuhn.net/matching-survey)，想弄清楚人们对 donation matching 有什么看法。这份 survey 收到了约 60 份 responses，足以进行合理的 analysis，并从中发现了一些有意思的事情。以下是我的发现。

目录

-   [一些 jargon](https://www.benkuhn.net/matching-results/#some-jargon)
-   [Survey design：调查设计](https://www.benkuhn.net/matching-results/#survey-design)
-   [Research questions：研究问题](https://www.benkuhn.net/matching-results/#research-questions)
-   [Caveats：注意事项](https://www.benkuhn.net/matching-results/#caveats)
-   [Results：结果](https://www.benkuhn.net/matching-results/#results)
    -   [matches 是 counterfactually valid 吗？](https://www.benkuhn.net/matching-results/#are-matches-counterfactually-valid)
    -   [人们会（想要）因为 match incentive 而 give more 吗？](https://www.benkuhn.net/matching-results/#do-people-want-to-give-more-due-to-the-match-incentive)
    -   [对 counterfactual 的 belief 如何 affect donation reduction？](https://www.benkuhn.net/matching-results/#how-does-belief-about-counterfactual-affect-donation-reduction)
    -   [challenges 与 matches 相比如何？](https://www.benkuhn.net/matching-results/#how-do-challenges-compare-to-matches)
    -   [当 challenge 被 met 时，人们会如何 react？](https://www.benkuhn.net/matching-results/#how-do-people-react-when-the-challenge-is-met)
    -   [人们对 non-counterfactual donations 感到多大程度的 deceived？](https://www.benkuhn.net/matching-results/#how-deceived-did-people-feel-about-non-counterfactual-donations)
-   [给 matching donors 的 Conclusions](https://www.benkuhn.net/matching-results/#conclusions-for-matching-donors)
    -   [对你的 matching 保持 transparent](https://www.benkuhn.net/matching-results/#be-transparent-about-your-matching)
    -   [考虑 running 一个 challenge，而不是一个 match](https://www.benkuhn.net/matching-results/#consider-running-a-challenge-instead-of-a-match)
    -   [考虑 running 一个更大规模的 experiment](https://www.benkuhn.net/matching-results/#consider-running-a-larger-experiment)
-   [Appendix：Code 与 data](https://www.benkuhn.net/matching-results/#appendix-code-and-data)

* * *

## 一些 jargon

_matching campaign_ 是指一位 big donor 提出，对 donated 给某个 charity 的 money 进行 match，通常限定在某个 predefined window of time 内。

如果这些 money 在其他情况下本来_不会_被 given 给那个 charity，那么这个 matching campaign 就是 _counterfactually valid_（或简称为 _counterfactual_）。[1](https://www.benkuhn.net/matching-results/#fn:1)

_challenge campaign_ 是指一位 big donor 向某个 charity 做出一笔 large initial donation，并 challenges 公众在某个 deadline 前 donate 一个 equal amount。

## Survey design：调查设计

这份 survey 向 respondents 提供了三个 hypothetical situations，其中有一个 organization 正在 running 一个 matching campaign：可能是 [Good Ventures](http://www.goodventures.org/)、[Harvard College Effective Altruism](http://harvardea.org/)，也可能是他们的一位朋友。在每种情况下，我都问了以下问题：

-   如果你 participated，matcher 会比其他情况下多 donate 多少 _more money_？也就是说，这个 match 真正的 counterfactually-adjusted effect 是什么？

    如果 participant 认为这个 match 确实 doubled 了自己的 donation，我就把这份 response coded 为 _full_；如果 participant 认为 matcher contributed 了一些 extra，但没有达到 full match，我就 coded 为 _partial_；如果他们认为 donor 的全部 money 无论如何都会被 given，我就 coded 为 _none_。

-   如果他们确定这个 match _不会_被 completely filled，他们会 donate 多少？如果确定会呢？

-   如果 matcher 改为 running 一个“challenge”，将 entire matching amount 预先 donated，并 challenged 公众 beat 那个 amount，他们会 donate 多少？

-   最后，我询问了他们对所说的 organization 有多熟悉，以及他们认为该 organization 有多 honest。

我还问了一些 demographic questions，了解 respondents 对 effective altruism 有多熟悉；另有一个问题询问，如果他们发现在 match scenario 中，那些 money 无论如何本来都会被 given，他们会感到多大程度的 misled。你可以在[这里](https://s3.amazonaws.com/bknet/match-survey/survey.pdf)找到完整的 survey。

**注意：**由于这些 questions 全都涉及 hypothetical situations，而且我向每个人都询问了全部 hypotheticals，因此，把人们的 answers interpret 为他们在现实中、没有因思考其他 hypotheticals 而受到 primed/anchored 时_实际_会做的事，是非常危险的。在现实中，当人们没有机会明确 check 自己在不同 situations 中的 answers 是否 consistent 时，他们可能会做出更加 inconsistent 的 decisions——例如，即使他们相信 matching funds 无论如何都会被 donated，仍然会在 match 处于 active 状态时 donating more。

不过，人们在这里的 answers 确实可以有一种有用的 interpretation：在能够 check 自己的 consistency 的情况下，经过反思，他们_想要_做什么。由于这份 survey 的部分 aim 是弄清 non-counterfactual matches 是否 violate donors 的 preferences，因此考虑这种 interpretation 仍然是有用的。

## Research questions：研究问题

这份 survey 的 designed 目的是回答以下 questions：

-   大多数人认为 matching funds 无论如何都会被 donated，这是否像有时有人声称的那样属实？
-   人们会因为 match incentive 而 give more 吗？
-   如果人们相信 match 更加 counterfactually valid，他们会更多地 reduce 自己的 gift 吗？
-   相较于 matches，transparency concerns 更少的 challenge campaigns 会 reduce 人们的 donations 吗？
-   如果人们得知某个他们原以为是 counterfactually valid 的 matching campaign 其实并非如此，他们会如何 react？

## Caveats：注意事项

在我报告 survey results 之前，我应该提出几点可能 confound 这些 results 的 caveats。

-   **Sampling bias（抽样偏差）。**这份 survey sample 主要是一个 [convenience sample](http://en.wikipedia.org/wiki/Nonprobability_sampling)，这是委婉地说，我其实并没有努力让它成为一个很好的 sample。它应该能在_一定程度上_对许多为 effective charities 举办的 fundraisers 的 audience 具有 representative 性，但显然也有一些方面并不如此。

-   **令人困惑的 questions。**要精确表达一个 match 是或不是 counterfactually valid 这个意思，并不容易。我已经尽了最大努力，但 respondents 仍可能在某些方面误解了 questions。

-   **Survey fatigue（调查疲劳）。**大多数 respondents 完成这份 survey 用了 5 到 10 分钟，但 questions 有些重复。Respondents 在回答后面的 questions 时，可能不再像之前那样认真思考，而只是填入与之前的 answers 相同的 values。

-   **Anchoring（锚定）。**由于我向每位 subject 都询问了每一种 hypothetical situation，他们的 responses 很可能被自己对前面 questions 的 answers 所 anchored。在 field 中，我们应该预期他们对各种 situations 的 reactions 会有更大的 variance。

## Results（结果）

我已经通过计算来考察上面概述的五个问题。由于我在 survey 中还问了许多其他问题，当然还有其他方面可以考察，但我想限制这次 initial analysis 的 scope（这个项目已经花了几周时间）。如果你对 dataset 的其他具体问题感兴趣，请告诉我，我会尝试考察它们。

### matches 是 counterfactually valid 吗？

我通过考察两个问题之间的 difference 来回答这个问题：

-   如果你 gave \$10 to the charity and the match was active, how much extra money would the charity receive (compared to if you gave \$0)？
-   如果你 gave \$10 to the charity and the match was _not_ active, how much extra money would the charity receive (compared to if you gave \$0)？

如果它们之间的 difference 是 \$10 or more, I coded this as believing _full_ counterfactual validity; if it was between \$0 和 \$10 (exclusive), _partial_ counterfactual validity; and if $0，则_没有_ counterfactual validity。

![](https://www.benkuhn.net/matching-results/counterfactual.svg)

人们对于 counterfactual validity 的 beliefs，在不同 matchers 之间似乎相当 robust（尽管这可能是由于 survey fatigue）。在每种情况下，约有 38/57 位 respondents（67%）认为 match 是 fully counterfactually valid，其余 respondents 则大致 evenly split 于 partial 和 no validity 两者之间。

有意思的是，对于 Good Ventures 和 HCEA 的 matches，我们都可以对照实际情况来 check 人们的 beliefs。对于 Good Ventures，我们可以看看介绍该 match 的 [GiveWell 的博客文章](http://blog.givewell.org/2013/12/03/good-ventures-matching-gift-to-givedirectly-and-grants-to-top-charities/)：

> 如果 match 的 full amount 被 reached，我们认为 Good Ventures 今年对我们的 top charities 所 giving 的 total 将会比 match 不可能存在时更多。

“_如果_ full amount…被 reached”这一措辞表明，Good Ventures 事实上计划只 donate 被 matched 的 amount，因此至少 on the margin，这个 match 是 fully counterfactual。

对于 HCEA 的 match，很遗憾，我无法引用公开来源，但我认为这个 match 只是 partially counterfactually valid。虽然 donors 给 HCEA 的 gifts 很可能 influenced 我们的 anonymous backer 如何 allocated 其 funds，但我认为那些 funds 很可能无论如何都会被 donated，而且至少一部分会给同样的 charities。

显然，我无法评说任何人的具体朋友所做 matches 的 counterfactual validity。但我怀疑，至少在 effective altruism community 中，当人们向朋友 fundraise 时，出于同样的原因，match 往往只是 partially valid。

如此多的 respondents 认为这些 matches 是 fully counterfactually valid，而它们实际上可能并非如此，这表明 matching donors 和 matching fundraisers 应当注意，让自己的 rhetoric 更加 transparent，以避免 deceiving donors。

### 人们是否会（想要）由于 match incentive 而 give more？

为了回答这个问题，我针对每个 matching campaign 问了两个问题：

-   如果 match 即将 expire，而且刚好 under 其 limit，你会 donate 多少？
-   如果 match 即将 expire，而且刚好 _over_ 其 limit，你会 donate 多少？

这两个 quantities 之间的 difference estimates 了 incentive 在多大程度上 affects 人们的 donation size。请记住，这些是 hypothetical donations——人们的实际 actions 会在多大程度上 reflect 这些 preferences，目前并不清楚。

在这项 analysis 中，我 excluded 了那些表示如果 match 是 under 其 limit 就会 give \$0 的人，因为他们显然没有机会 reduce 自己的 donation。下面展示了 average donation 的情况（error bars 是 mean 的 bootstrapped 95% confidence interval）：

![](https://www.benkuhn.net/matching-results/match.svg)

尽管 confidence intervals 有大量 overlap，这并不意味着 donation reduction 是 insignificant，因为我使用了 [repeated measures design](http://en.wikipedia.org/wiki/Repeated_measures_design)。为了了解更多细节，我们来看看展示 individual donations 及其变化的 [violin plot](http://en.wikipedia.org/wiki/Violin_plot)：

![](https://www.benkuhn.net/matching-results/match_detail.svg)

对于朋友和 HCEA，donation reduction 在 0.05 level 上是 statistically significant，而对于 Good Ventures，则是 marginally significant（_p_ < 0.1）。朋友的 match 所对应的 reduction 是 largest，几乎达到了 baseline average donation 的 40%。（对于 HCEA，point estimate 更接近 10%，而对于 Good Ventures，则更接近 5%。）

请注意，尽管 average reduction 是 significantly negative，但这由相对少量的 large reductions 所 driven，而许多 individual donors 的 donation 则保持不变或相对接近原来的水平。

### 对于 counterfactual 的 belief 如何 affect donation reduction？

如果人们受到了 match incentive 的 affected，我们会 expect 这是因为他们认为 match 具有 counterfactual effect。这与 data 是否 consistent？

为了回答这个问题，我按照人们认为每个 match 是 fully、partly 还是完全不 counterfactually valid 来划分 group，并考察每个 group 的 donation reduction。当然，由于后两个 groups 中只有少数 subjects，所有 comparisons 都不是 significant；results 既 supported 朝 expected direction 的 trend，也 supported no trend。

![](https://www.benkuhn.net/matching-results/decrease_by_cf.svg)

有意思的是，似乎即使是那些认为 match 没有 counterfactual effect 的人，也可能在 reducing 自己的 donations（尽管 effect 只是 marginally significant，主要出现在朋友的情形中）。这表明，人们的 motivated 不仅来自 increasing 自己的 effective donation size，也来自看到 matcher 的 goal 被 met。这为“challenge” fundraisers 的 effectiveness 提供了一些 evidence，因为它们应当与 matches 共享这一 mechanism。

### challenges 与 matches 相比如何？

对于上述每个关于 matching campaign 的问题，我也针对 challenge campaign 问了同样的问题。利用这些信息，我们可以看看：如果 campaign 是 challenge-based 而非 match-based，人们是否认为自己会 reduce donation：

![](https://www.benkuhn.net/matching-results/mvsc.svg)

在这项 analysis 中，我将 match 是 below 其 limit 时的 average donation，与 equivalently-sized challenge 尚未被 fulfilled 时的 average donation 进行了 compared。

HCEA 情形中的 large difference 由一位 donor 所 driven，这位 donor 表示自己会向 challenge donate \$5000 to a match but $0。我并不很看重这个 data point，尽管它导致了较大的 average donation reduction（约 $150）。事实上，对于 HEA，average match donation 与 average challenge donation 之间的 difference 仍然不是 statistically significant。对于另外两个 organizations，相较于 average donation size，这一 difference 也相当 small。

![](https://www.benkuhn.net/matching-results/mvsc_detail.svg)

显然，这只能作为非常 weak evidence，表明 challenge fundraisers 在现实中能够与 matches competitive，因为——再说一次——我在 survey 中只询问了 hypothetical situations。此外，我的 analysis 具有 low statistical power；这些 data 也 compatible with 比我的 point estimates 稍大的 donation reductions。

不过，考虑到 challenges 相比 matches 所具有的 transparency benefits，我认为，这至少表明，对 challenges 做进一步 research 可能相当有价值。

### 当 challenge 被 met 时，人们会如何 react？

由于我针对 challenges 和 matches 提出了相同的 questions，我也能考察 respondents 是否认为，如果 challenge 被 met，他们就会 reduce 自己的 donations。

![](https://www.benkuhn.net/matching-results/challenge.svg)

再次强调，由于采用了 repeated-measures design，overlapping confidence intervals 并不意味着 non-significance：

![](https://www.benkuhn.net/matching-results/challenge_detail.svg)

朋友的 fundraiser 的 difference 比 Good Ventures 或 HCEA 的大得多。这为我上面的看法提供了更多 evidence：人们受到的 motivated 不仅来自可能 increasing 自己 donation 的 counterfactual impact，也来自看到 target hit。不过，同样地，这个 difference 是由一个 subgroup 大幅 reducing 自己的 donations 所 driven，而不是由每个人都略微 reducing 自己的 donations 所 driven。

### 对于 non-counterfactual donations，人们感到有多 deceived？

在 survey 的最后，我加入了以下 question：

> 如果我得知，为 matching campaign 提供的 funds，无论 match 是否被 fulfilled，都会被 donated——例如，Good Ventures 本来就会 given \$5 million to GiveDirectly even if they had raised only \$4 million 来自公众——我会感到……

response 采用 Likert scale，从 1（完全不觉得 deceived）到 5（觉得非常 deceived）。58 位 respondents 中有 21 位——超过 sample 的三分之一——回答了 4 或 5；另有 25 位回答了 2 或 3。只有 11 位 respondents 选择了“完全不觉得 deceived”。

这是一个引导性很强的 leading question，因此尚不清楚它能在多大程度上 tracks 人们在 field 中会如何 respond；尽管如此，它仍 suggests，我们确实应该重视让 donation matches 更加 transparent。

## 针对 matching donors 的 Conclusions

### 对你的 matching 保持 transparent

任何 unmatched funds 会去哪里？无论如何，它们都会被 donated 给同一个 organization 吗？会被 Given 给不同的 charity 吗？会被 Burned 吗？绝大多数人认为，所有 matches 都是 fully counterfactually valid，因此，如果你的 match 并非如此，你就应该说明。这可能 affect 人们 donate 多少，以及他们感到有多 deceived，因此，在这里做到完全 honest 非常重要。

有一件事我没有在这次 survey 中考察，但值得今后 research，那就是另一种“partial validity”：unmatched funds 不会流向同一个 charity，而是流向另一个 charity，有时还是一个相当 similar 的 charity。希望大家清楚，对于 foundations，这种情况 _总是_ 会发生；但对于 HEA 的 anonymous matcher 或朋友这样的 private donors，情况就不清楚了。在你的 fundraiser 中也对此保持 transparent，可能是明智之举。

除了对 honesty 的顾虑，我认为 matching 中的 transparency 还有其他很好的理由。在我看来，许多 EA fundraisers 最大的 benefits，远不只是它们能够 raise additional funds——最好的部分是其 flow-through effects：让人们更加公开自己的 giving，更多地讨论 effective altruism，并让朋友们产生兴趣。从这个角度看，利用 matches 向人们介绍 effective altruism 的两个 central practices——transparency 和 counterfactual reasoning——似乎大有裨益。这也会让这些 campaigns 更加区别于典型的 fundraisers。

### 考虑 running 一个 challenge，而不是 match

survey suggested，人们觉得 challenge fundraisers 与 matches 同样 compelling，而且当 challenge target 被 reached 时，他们似乎比 match expired 时更少 reduce 自己的 donations。此外，对于 challenges，counterfactual effects 要清楚得多：你的 money 去了哪里一目了然，funding dynamics 也更加 intuitive。

这与[我对 donation matching literature 的 interpretation](http://www.benkuhn.net/matching)是 consistent 的；我在那里写道，我预计 matches 主要通过 social proof 和 urgency effects 起作用，而不是通过让人们的 donations 变得 bigger（而且，与此 consistent，我发现 changing match 的 amount 往往并不重要）。Challenge fundraisers 不会像 matches 那样让人们的 donations 变得 bigger，但它们具有相同的 urgency effect，并发挥着 _stronger_ social proof 的作用。因此，它们同样有效并不令人意外。

### 考虑 running 一个 larger experiment

这次 survey 产生了一些有用的信息，但如果能开展实际的 field experiments（并有 larger sample size），就更好了。关于 matches 的 academic literature 已经相当 sparse，而关于 challenges 的 literature 则更加 sparse。因此，任何额外的 experiments 都可能大大增进我们的认识。

## 附录：Code 和 data

我已将自己的 code、data 和 survey 的 PDF 收集在[一个 Github repository](https://github.com/benkuhn/matching-survey) 中。欢迎查看它、对它进行 fork、reproduce 其中的 figures 等。如果你发现任何 bugs，_请_ report 它们（这是当然的）。

感谢阅读！

* * *

1.  就我的目的而言，我只关心 _针对完全相同的 charity_ 的 counterfactual validity——也就是说，如果 matching funds 之后会流向一个大致 similar 的 charity，我仍会将这个 match 称为 counterfactually valid。这是因为我认为，从 honesty/transparency 的角度看，这种情况远没有另一种情况令人担忧：donor 实际上早就计划把 unmatched funds give 给同一个 organization。 [↩︎](https://www.benkuhn.net/matching-results/#fnref:1)

---

## 原文

A couple weeks ago I made [a survey](http://www.benkuhn.net/matching-survey) to try to figure out what people thought about donation matching. The survey got about 60 responses, which is enough for a reasonable analysis that turned up some interesting things. Here’s what I found.

Contents

-   [Some jargon](https://www.benkuhn.net/matching-results/#some-jargon)
-   [Survey design](https://www.benkuhn.net/matching-results/#survey-design)
-   [Research questions](https://www.benkuhn.net/matching-results/#research-questions)
-   [Caveats](https://www.benkuhn.net/matching-results/#caveats)
-   [Results](https://www.benkuhn.net/matching-results/#results)
    -   [Are matches counterfactually valid?](https://www.benkuhn.net/matching-results/#are-matches-counterfactually-valid)
    -   [Do people (want to) give more due to the match incentive?](https://www.benkuhn.net/matching-results/#do-people-want-to-give-more-due-to-the-match-incentive)
    -   [How does belief about counterfactual affect donation reduction?](https://www.benkuhn.net/matching-results/#how-does-belief-about-counterfactual-affect-donation-reduction)
    -   [How do challenges compare to matches?](https://www.benkuhn.net/matching-results/#how-do-challenges-compare-to-matches)
    -   [How do people react when the challenge is met?](https://www.benkuhn.net/matching-results/#how-do-people-react-when-the-challenge-is-met)
    -   [How deceived did people feel about non-counterfactual donations?](https://www.benkuhn.net/matching-results/#how-deceived-did-people-feel-about-non-counterfactual-donations)
-   [Conclusions for matching donors](https://www.benkuhn.net/matching-results/#conclusions-for-matching-donors)
    -   [Be transparent about your matching](https://www.benkuhn.net/matching-results/#be-transparent-about-your-matching)
    -   [Consider running a challenge instead of a match](https://www.benkuhn.net/matching-results/#consider-running-a-challenge-instead-of-a-match)
    -   [Consider running a larger experiment](https://www.benkuhn.net/matching-results/#consider-running-a-larger-experiment)
-   [Appendix: Code and data](https://www.benkuhn.net/matching-results/#appendix-code-and-data)

* * *

## Some jargon

A _matching campaign_ is when a big donor offers to match money donated to a charity, usually for some predefined window of time.

A matching campaign is _counterfactually valid_ (or _counterfactual_ for short) if the money would _not_ have been given to that charity otherwise.[1](https://www.benkuhn.net/matching-results/#fn:1)

A _challenge campaign_ is when a big donor makes a large initial donation to a charity and challenges the public to donate an equal amount by some deadline.

## Survey design

The survey gave respondents three hypothetical situations in which an organization was running a matching campaign: either [Good Ventures](http://www.goodventures.org/), [Harvard College Effective Altruism](http://harvardea.org/), or one of their friends. In each case, I asked the following:

-   How much _more money_ would the matcher donate if you participated, than they would otherwise? That is, what is the true counterfactually-adjusted effect of the match?

    I coded this response as _full_ if the participant thought that the match really doubled their donation; _partial_ if the participant thought the matcher contributed some extra, but not the full match; and _none_ if they thought all the donor’s money would have been given anyway.

-   How much would they donate if they were sure the match _wouldn’t_ be completely filled? if it would be?

-   How much would they donate if the matcher were instead running a “challenge” in which the entire matching amount was donated up front and the public was challenged to beat that amount?

-   Finally, I asked how familiar they were with the organization in question, and how honest they perceived them to be.

I also asked some demographic questions about how familiar the respondents were with effective altruism, and one question about how misled they would feel if they found out the money would have been given anyway in the match scenario. You can find the full survey [here](https://s3.amazonaws.com/bknet/match-survey/survey.pdf).

**Note:** Because the questions were all about hypothetical situations, and because I asked everyone about all of the hypotheticals, it’s very dangerous to interpret people’s answers as what they would _actually_ do, in reality, when not primed/anchored by thinking about alternate hypotheticals. In reality, people might make more inconsistent decisions—for instance, donating more when a match is active even if they believe the matching funds would be donated anyway—when they didn’t have the opportunity to explicitly check that their answers in different situations were consistent.

However, people’s answers here do have a useful interpretation as what they would _like_ to do, on reflection, given the ability to check their consistency. Since part of the aim of this survey is to figure out whether non-counterfactual matches violate donors’ preferences, it’s still useful to consider this interpretation.

## Research questions

The survey was designed to answer the following questions:

-   Is it true, as sometimes claimed, that most people think matching funds would be donated anyway?
-   Do people give more due to the match incentive?
-   Do people reduce their gift more if they believe that the match is more counterfactually valid?
-   Would challenge campaigns, which have fewer transparency concerns than matches, reduce people’s donations?
-   How would people react to learning that some matching campaign wasn’t counterfactually valid when they thought it was?

## Caveats

Before I report the survey results, I should make a couple of caveats that might confound them.

-   **Sampling bias.** The survey sample was mostly a [convenience sample](http://en.wikipedia.org/wiki/Nonprobability_sampling), which is a polite way of saying I didn’t really try to make it a very good sample. It should be _somewhat_ representative of the audience of many fundraisers for effective charities, but there are obviously ways in which it isn’t.

-   **Confusing questions.** It’s hard to express precisely the idea that a match is or isn’t counterfactually valid. I tried my best, but it’s possible that respondents misunderstood the questions in some way.

-   **Survey fatigue.** The survey took most respondents between 5 and 10 minutes, but the questions were somewhat repetitive. Respondents may have stopped thinking as hard about their later answers and simply filled in the same values as for earlier answers.

-   **Anchoring.** Since I asked every subject about every hypothetical situation, their responses were likely anchored by their answers to previous questions. In the field, we should expect more variance in their reactions to the various situations.

## Results

I’ve run the numbers to look at the five questions I outlined above. Because I asked a number of other questions in the survey, there are certainly other things I could have looked at, but I wanted to limit the scope of my initial analysis (this project has already taken a few weeks). If you’re interested in specific other questions about the dataset, please let me know and I’ll try to look at them.

### Are matches counterfactually valid?

I answered this question by looking at the difference between two questions:

-   If you gave $10 to the charity and the match was active, how much extra money would the charity receive (compared to if you gave $0)?
-   If you gave $10 to the charity and the match was _not_ active, how much extra money would the charity receive (compared to if you gave $0)?

If the difference between these was $10 or more, I coded this as believing _full_ counterfactual validity; if it was between $0 and $10 (exclusive), _partial_ counterfactual validity; and if $0, _no_ counterfactual validity.

![](https://www.benkuhn.net/matching-results/counterfactual.svg)

People’s beliefs about counterfactual validity appear to be quite robust across different matchers (although this may be due to survey fatigue). In each case, about 38/57 respondents (67%) believed the match was fully counterfactually valid, with the remainder split about evenly between partial and no validity.

Interestingly, for both the Good Ventures and HCEA matches, we can check people’s beliefs against reality. For Good Ventures, we can look at [GiveWell’s blog post](http://blog.givewell.org/2013/12/03/good-ventures-matching-gift-to-givedirectly-and-grants-to-top-charities/) on the match:

> If the full amount of the match is reached, we believe Good Ventures will be giving more in total to our top charities this year than they would have been had a match not been a possibility.

The wording “_If_ the full amount… is reached” suggests that Good Ventures was in fact planning to donate only the amount matched, so at least on the margin the match was fully counterfactual.

For the HCEA match, I unfortunately can’t cite a public source, but I believe that the match was only partially counterfactually valid. While the gifts of donors to HCEA probably influenced how our anonymous backer allocated their funds, I believe that those funds would likely have been donated anyway, at least in part to the same charities.

Obviously I can’t speak to the counterfactual validity of anyone’s specific friends’ matches. But I suspect that, at least in the effective altruism community, when people fundraise from their friends, the match is often only partially valid, for the same reasons.

Since so many of the respondents believed the matches were fully counterfactually valid when they may not have been, this suggests that matching donors and matching fundraisers should be careful to be more transparent about their rhetoric to avoid deceiving donors.

### Do people (want to) give more due to the match incentive?

To answer this question, I asked two questions for each matching campaign:

-   How much would you donate if the match were about to expire and were just under its limit?
-   How much would you donate if the match were about to expire and were just _over_ its limit?

The difference between these two quantities estimates how much the incentive affects people’s donation size. Remember, these are hypothetical donations—it’s not clear how much people’s actions would actually reflect these preferences.

For this analysis, I excluded people who said they would give $0 if the match was under its limit, since they obviously didn’t have a chance to reduce their donation. Here’s what the average donation looked like (error bars are bootstrapped 95% confidence interval for the mean):

![](https://www.benkuhn.net/matching-results/match.svg)

Although the confidence intervals overlap a lot, this doesn’t mean the donation reduction is insignificant, because I used a [repeated measures design](http://en.wikipedia.org/wiki/Repeated_measures_design). For more detail, let’s look at a [violin plot](http://en.wikipedia.org/wiki/Violin_plot) of the individual donations and how they changed:

![](https://www.benkuhn.net/matching-results/match_detail.svg)

The donation reduction was statistically significant at the 0.05 level for the friend and HCEA, and marginally significant (_p_ < 0.1) for Good Ventures. The reduction was largest for the friend’s match, where it was almost 40% of the baseline average donation. (For HCEA the point estimate was more like 10%, and for Good Ventures more like 5%.)

Note that, although the average reduction was significantly negative, this was driven by a relatively small number of large reductions, while many individual donors stayed the same or relatively close.

### How does belief about counterfactual affect donation reduction?

If people were affected by the match incentive, we would expect that to happen because they believed that the match had a counterfactual effect. Is that consistent with the data?

To answer this question, I broke up the group by whether they believed each match was fully, partly or not at all counterfactually valid, and looked at the donation reduction in each group. Since there were only a few subjects in the latter two groups, of course, none of the comparisons were significant; the results supported a trend in the expected direction, but also no trend.

![](https://www.benkuhn.net/matching-results/decrease_by_cf.svg)

Interestingly, it looks like even people who thought that the match had no counterfactual effect may be reducing their donations (though the effect is only marginally significant, mainly in the friend case). This suggests that people are motivated not only by increasing their effective donation size, but also by seeing the matcher’s goal met. This is some evidence in favor of the effectiveness of “challenge” fundraisers, which should share this mechanism with matches.

### How do challenges compare to matches?

For each of the questions I mentioned above about a matching campaign, I asked the same question about a challenge campaign. Using this info, we can see if people thought they would reduce their donation if the campaign was challenge-based instead of match-based:

![](https://www.benkuhn.net/matching-results/mvsc.svg)

For this analysis, I compared the average donation when the match was below its limit to the average donation when the equivalently-sized challenge hadn’t yet been fulfilled.

The large difference for the HCEA case was driven by a single donor who said they would donate $5000 to a match but $0 to a challenge. I don’t place very much weight on this data point, even though it led to a large average donation reduction (about $150). Indeed, the difference between the average match and challenge donation still wasn’t statistically significant for HEA. For the other two organizations, it was also quite small compared to the average donation size.

![](https://www.benkuhn.net/matching-results/mvsc_detail.svg)

Obviously, this is only very weak evidence that challenge fundraisers would be competitive with matches in reality, since—once again—I only asked about hypothetical situations in the survey. Furthermore, my analysis had low statistical power; the data are compatible with donation reductions somewhat larger than my point estimates.

However, given the transparency benefits of challenges compared to matches, I think that at the very least this suggests that further research into challenges could be quite valuable.

### How do people react when the challenge is met?

Since I asked the same questions about challenges as about matches, I was also able to look at whether respondents thought they would reduce their donations if the challenge was met.

![](https://www.benkuhn.net/matching-results/challenge.svg)

Once again, due to the repeated-measures design, overlapping confidence intervals doesn’t mean non-significance:

![](https://www.benkuhn.net/matching-results/challenge_detail.svg)

The difference was much larger for a friend’s fundraiser than for Good Ventures or HCEA. This is more evidence for my suggestion above that people are motivated by seeing the target hit, not just by potentially increasing their donation’s counterfactual impact. Once again, though, the difference was driven by a subgroup reducing their donations a lot rather than everyone reducing them a little bit.

### How deceived did people feel about non-counterfactual donations?

At the end of the survey, I included the following question:

> If I learned that funds put up for a matching campaign would be donated regardless of whether the match was fulfilled—for instance, that Good Ventures would have given $5 million to GiveDirectly even if they had raised only $4 million from the public—I would feel…

The response was a Likert scale ranging from 1 (not at all deceived) to 5 (very deceived). 21 out of 58 respondents—more than a third of the sample—answered with a 4 or 5; an additional 25 answered 2 or 3. Only 11 respondents selected “not at all deceived.”

This is a very leading question, so it’s not clear how well it tracks how people would respond in the field; nevertheless, it suggests that we should indeed be worried about making donation matches more transparent.

## Conclusions for matching donors

### Be transparent about your matching

Where will any unmatched funds go? Will they be donated to the same organization anyway? Given to a different charity? Burned? A strong majority thought that all matches were fully counterfactually valid, so if this isn’t true of your match, you should say so. This could affect how much people donate, and how deceived they feel, so it’s very important to be totally honest here.

One thing that I didn’t look at in this survey, but that merits future research, is another type of “partial validity” in which unmatched funds don’t go to the same charity, but go to another charity, sometimes a quite similar one. It’s hopefully clear that this _always_ happens for foundations, but it’s not clear for private donors like HEA’s anonymous matcher or one’s friends. It’s probably wise to be transparent about this in your fundraiser as well.

Separately from concerns about honesty, I think transparency in matching is great for other reasons as well. It seems to me that by far the biggest benefits of many EA fundraisers are not just that they raise additional funds—the best part is the flow-through effects from getting people to be more public about their giving, to discuss effective altruism more, and to get their friends interested. From that standpoint, it seems like a huge win to use the matches to introduce people to two central practices of effective altruism, transparency and counterfactual reasoning. It would also make the campaigns stand out more from typical fundraisers.

### Consider running a challenge instead of a match

The survey suggested that people found challenge fundraisers just as compelling as matches, and seemed to reduce their donations less when the challenge target was reached than when a match expired. Furthermore, with challenges, the counterfactual effects are much clearer: it’s obvious where your money went, and the funding dynamics are much more intuitive.

This is consistent with [my interpretation of the donation matching literature](http://www.benkuhn.net/matching), where I wrote that I expected matches to work mostly through social proof and urgency effects rather than through making people’s donations bigger (and found, consistent with this, that changing the amount of the match tended not to matter). Challenge fundraisers don’t make people’s donations bigger like matches do, but they share the same urgency effect and function as _stronger_ social proof. So it’s not surprising that they work just as well.

### Consider running a larger experiment

This survey produced some useful info, but it would be even better to have actual field experiments (and a larger sample size). The academic literature on matches is sparse enough, and the literature on challenges even more so. So any additional experiments could add a lot to our knowledge.

## Appendix: Code and data

I’ve collected my code, data and a PDF of the survey in [a Github repository](https://github.com/benkuhn/matching-survey). Feel free to check it out, fork it, reproduce the figures, etc. If you find any bugs, _please_ report them (obviously).

Thanks for reading!

* * *

1.  For my purposes, I only care about counterfactual validity _for the exact same charity_—that is, I would still call a match counterfactually valid if the matching funds would go to a roughly similar charity later. This is because I think that situation is much less worrying from an honesty/transparency perspective than the situation where a donor was literally already planning to give unmatched funds to the same organization. [↩︎](https://www.benkuhn.net/matching-results/#fnref:1)