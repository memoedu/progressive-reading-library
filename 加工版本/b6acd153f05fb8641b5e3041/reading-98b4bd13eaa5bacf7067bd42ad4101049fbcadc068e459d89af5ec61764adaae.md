# Academic salaries 和 incentives（学术薪酬与激励）

---

## 正文

当下，meta-science 领域最热门的话题之一，是使用 lotteries 来 fund research。简而言之，其 rationale 是，很难判断谁或什么会 successful，而尝试做出这种判断也非常 costly。依赖 peer review 的 system 要求 researchers 花时间 write grants 并 review 它们。我之前曾 reviewed 过 peer review 在 grant awards 的背景下，[figuring](https://nintil.com/grant-peer-review) out “什么是 good”方面的 effectiveness，发现目前实行的 peer review 在 predicting future impact 方面确实 substantially better than chance，尤其是对于最 impactful 的 papers；但与此同时，它也远非 perfect，仍有大量 variance 未获 explained。我的看法是，reviewers 能够 identify 什么显然不 great，以及什么显然 great（也许 identifying 后者比前者更容易）。但还有一个“messy middle”，其中该 fund 什么并不明确。

Peer review systems 并不 cheap。以 Australia 为例，Gordon 和 Poulin（[2009](https://pubmed.ncbi.nlm.nih.gov/19247851/)）说，peer review 的 cost 比直接给每个人一笔 grant 还要 greater。情况还不止如此。Guthrie（[2018](https://f1000research.com/articles/6-1335)）reports：

> _peer review system 的 burden 很 high，而且主要落在 applicants 身上。_据 estimated，包括 application preparation 在内，peer review system 的 overall monetised cost 占到了 **allocated budget 的多达 20–35 per cent**（[Gluckman, 2012](https://f1000research.com/articles/6-1335#ref-46)）。[Graves _et al._ (2011)](https://f1000research.com/articles/6-1335#ref-48) report，NHMRC 的 application system 的 monetised costs 为 \$14,000 per grant, whilst extrapolating RCUK ([Research Councils UK, 2006](https://f1000research.com/articles/6-1335#ref-88)) estimates suggests that the costs of the application process are 10–17 per cent of the total cost of research. An evaluation of the CIHR Operating Open Grants Program (OOGP) found the application cost of OOGP grants to be Can$14,000（[Peckham _et al._, 2012](https://f1000research.com/articles/6-1335#ref-83)）。一项关于为 NIH 的 nursing research 准备 grants 的 detailed review 也 reports，institutions 承担着同样 high 的 costs（[Kulage _et al._, 2015](https://f1000research.com/articles/6-1335#ref-124)）。个别 researchers 在提供 congressional testimony 时 estimated，自己多达 60 per cent 的时间用于 seeking funding（[Fang & Casadevall, 2009](https://f1000research.com/articles/6-1335#ref-34)）。

这可能看起来有些夸张，但我们可以做一个简单的 calculation，说明这并非不切实际。Assume 只有 PIs 参与 grant application 和 reviews。这里的 H 代表 time，S 则是 success rates（指 applying for a grant 或让 paper 得以 published）。假定一份工作每周 40 hour（这与实际情况相去甚远），2080 就是可用于 research 的 hours。我从[这里](https://nexus.od.nih.gov/all/2018/03/07/how-many-researchers-revisited-a-look-at-cumulative-investigator-funding-rates/)取得了 PIs 的 total number，从[这里](https://www.nih.gov/about-nih/what-we-do/impact-nih-research/our-knowledge#:%7E:text=In%202016%20alone%2C%20over%20115%2C000%20articles%20acknowledged%20NIH%20grant%20support.&text=Each%20R01%20grant%2C%20NIH's%20most,of%207.36%20published%20research%20articles.)取得了 published papers 的 number。write a grant 所需的时间在 170 到 270 hours 之间，average 为 200（Boyack et al., [2018](https://www.researchgate.net/profile/Kevin_Boyack/publication/321581221_Toward_predicting_research_proposal_success/links/5a562ad2aca272bb6963c12e/Toward-predicting-research-proposal-success.pdf)）。在这些 assumptions 下，或者对其作任何 plausible 的 changes，我们都能看到，average researcher 确实会将 60% 的时间用于 seeking funding。即使把每周 40 hour 增加到每周 60 hour（3120 hours），仍意味着有 40% 的时间花在 applying for grants 上。peer review 的 cost 在很大程度上由 applying for grants 主导，而非 peer reviewing。我的 model assumes，average PI 每年会 reviewing 13 篇 papers。这些都是 averages；我的 model implies，on average，一位 PI 每年在 writing 5 份 grants。这里有一个[例子](https://twitter.com/michaelzlin/status/1350933138055294976)：花 120h 来 write a grant，而且 on average 必须 apply 7 次。关键在于，如果这些 numbers 大致 correct，就无法回避这一 conclusion：scientists 必须花大量时间 applying 和 reviewing grants，以及 reviewing papers。

$$
H=H_{research}+H_p+H_{other}+H_g=2080\\

H_g=\frac{H_{one\space grant}}{S_{grant}}=\frac{200}{0.2}=1000\\

H_p=\frac{N_{papers}}{N_{scientists}}*\frac{H_{{one\space review}}}{S_{{journal}}}*N_{reviews \space per \space paper}=\frac{115000}{30000}*\frac{5}{0.3}*4=256
$$

当然，这并不与 peer review 所耗去的 researcher 时间的 hours 数直接 related，这一点很重要，必须指出；也就是说，即使 PIs 花了一半时间 applying for grants，假如没有 peer review，实际开展 science 的 total hours 也不会 double。这是因为 research workforce 的成员不只有 PIs。如果 average lab 有 7 名成员（包括 PI，Conti & Liu, [2014](https://www.sciencedirect.com/science/article/abs/pii/S0048733315000037)；Cook et al., [2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4465944/)），而且他们都工作相同的 hours，那么彻底取消 grant system 并不会使 science 的 hours 达到 2x，而只会达到 1.16x。但是！至少有两个原因表明，这样会 undervaluing 其 benefits：a) PI role 可能是 special；他们的一小时可能比 graduate student 的一小时更有价值；更重要的是，b) 不受 grant system 约束，意味着 labs 可能 explore riskier ideas。我预计，这会是不实行 peer reviewed 所带来的 benefits 得以 accrue 的 key channel，而这些 benefits 必须足以 compensate 对那些不那么 good 的 work 增加 funding 所带来的影响（这些 work 在 peer review system 下本来不会获准）。

## Academic salaries 和 incentives（学术薪酬与激励）

Academia 很奇怪。在大多数工作中，你领取的 salary 按每个工作小时累积。在 academia，许多 PIs 担任的是 _soft money_ positions；他们只有一年中的部分时间能领到薪酬，其余时间则需要靠 get grants 来解决。此外，如果他们不能 get grants，就会被踢出这个 system。即便是 tenured professors，如果不能 get grants，也很可能受到消极抵制而被迫离开。还需要 Grants 来 support 这个 lab；university 支付 basic salary，但 running a lab，尤其是在 life sciences，涉及 hiring postdocs、graduate students，以及 buying reagents 和 equipment。因此，academics 无论为了 progress，还是为了留在这个 system 中，都 _必须 get grants_。也许 getting a grant 比这个 grant 的用途更重要，这就导致 PIs 声称自己会 work on X，实际上却 do Y，因为他们认为后者更值得做。理论上，这不应该有影响：Funding agencies 重视 novelty，但在实践中，重要的不是你认为哪些东西是 novel、valuable 和 worthwhile，而是你预计 panel 会将哪些东西 judge 为如此。

为什么会这样？为什么 science 会以这种方式得到 funded，至少在 US 是这样？Bourne (2018) [说](https://www.pnas.org/content/115/35/8647)（没有提供 citation），早在 70s，75% 的 salaries 由 university 直接支付，而如今这个数字可能更接近 35%，所以事情并非一直如此。要深入讨论这个话题，需要一篇更长的文章。

## Scientific egalitarianism 和 grant efficiency（科学平等主义与资助效率）

Science 是 highly unequal 的（[Peifer, 2017](https://www.molbiolcell.org/doi/pdf/10.1091/mbc.e17-07-0462)）：10% 的 PIs 获得 40% 的 funding，20% 的人获得其中的 56%，而 1% 的人获得 11% 。这不一定是坏事，有些 projects 可能比其他 projects 需要更多 funding。Peifer 认为，grant money 存在 diminishing returns，因此，如果想 produce 一组在 aggregate 上获得最多 citations 的 papers，就应使 funding 更加 egalitarian。不过，也可以说，更重要的是 most cited papers，而不是 maximizing 每篇 paper 的 average；在这种情况下，funding inequality 就可能得到 justified。Peifer 记录指出，在 producing most cited work 的 labs 中，超过一半在 [Grant Support Index](https://www.sciencemag.org/news/2017/06/updated-nih-abandons-controversial-plan-cap-grants-big-labs-creates-new-fund-younger) 上不足 21 points（GSI；也就是 3 R01s 或更少）。据一则轶事，Ed Boyden 无法为自己的 expansion microscopy work 获得 funding，原因之一是他已经持有“[too many grants](https://twitter.com/ArtirKel/status/1344351782869221376)”，因此 NIH 正式或非正式地正在实行某种类似做法（他的 lab 可能是 average lab 的 6x 大，因此不一定是他们在做非常 expensive projects；他们也可能只是在 parallel 地做很多 projects）。

Wahls（[2018](https://elifesciences.org/articles/34965)）得出了同样的 concludes：PIs 每人应获得 400k$ to maximize return on $，以 total citation count 来 measured 所花费的金额（巧的是，这与一个 R01 相差不远）。更宽松的 cap，即每人 1M$ per investigator, coupled with additional grants of 200k$，会腾出资金来 support 额外 10000 名 investigators（这意味着 funded NIH researchers 的 total number 增加 38%）。

Wahls 没有说明这是否会 reduce impactful publications 的 number。更 impactful research 是否可能需要更 expensive grants？Wahls cites 了几篇 papers，来支持“更便宜的 grants 是更 efficient 的”这一观点。Lorsch（[2017](https://www.molbiolcell.org/doi/10.1091/mbc.E14-06-1163)）提到一项较早针对 NIGMS（National Institute of General Medical Sciences）开展的 study，显示就 impact 和 impact factor（指 papers 最终发表的 journals）而言，peak 对应的金额远高于 R01；这个 peak 似乎位于 600-1000k$ range 中的某处。奇怪的是，在 figure 中，超出这个 range 后，productivity 出现 sharp drop（paper 中没有解释）。

![image-20210118183409361](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118183409361.png)

Basson et al.（[2016](https://loop.nigms.nih.gov/2016/07/revisiting-the-dependence-of-scientific-productivity-and-impact-on-funding-level/)）确实考察了 2011-2015 期间的 citations（采用 normalized measure；同样针对 NIGMS），这里似乎没有什么明显的 trend；事实上，大多数 very highly cited papers 似乎 concentrated 在 sub-500k$ range。

![](https://loop.nigms.nih.gov/wp-content/uploads/2016/07/fig6_bg.jpg)

Lauer et al.（[2017](https://www.biorxiv.org/content/10.1101/142554v2)）includes 了 1996-2014 期间所有 NIH-funded investigators（71k PIs），他们 published 了 1M papers。Table 1 中有一些按 GSI 进行 bucketed 的 summary statistics（这是衡量 PI 获得 funding 程度的 index；一个 R01 是 equivalent to GSI=7 的）；随着 GSI 更高，relative citation ratio 的（mean, median,max）显著 _up_。下面我特别标出了 maximum，因为我尤其关心将它作为 outstanding research 的 proxy；mean 上升了 2x，但 maximum 上升了 10x，这相当 significant。所有 impact-related metrics 都上升这一事实，为此增加了 robustness。

![image-20210118184651767](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118184651767.png)

这篇 paper 还将我们最喜欢的 researchers——HHMI [investigators](https://nintil.com/hhmi-and-nih)——单独拿出来 considers；他们比其他人更 productive（无论是因为 award，还是由于其 intrinsic capabilities）。不出所料，无论看 mean、median，还是 maximum，他们的 work 都更 impactful。事实上，如果取 HHMIs 中 lowest quartile 的 average maximum RCT（5.0），它比 non HHMIs 的 _median_ maximum RCR 还高。这里看 median 会非常 misleading；HHMIs 看起来并没有那么显著地更 productive，他们的 outperformance 并不来自 publishing 更多，而是来自 publishing 更多 extremely well cited papers。

![image-20210118191453754](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118191453754.png)

如下所示，在 general sample 中，producing most highly cited work 存在 decreasing marginal returns。超过 21 后，似乎没有多少 increase。

![image-20210118191810074](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118191810074.png)

按 HHMI status 将 sample 拆开，也得到同样的结果。不过，两个 axes 都是 log-transformed 的，因此很难直观体会两个 groups 之间的 differences。

![image-20210118192109202](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118192109202.png)

mean 或 median 的 curves 看起来相似，因此，如果资金总额 fixed，并且希望 maximize 每位 PI 的 marginal productivity，就应该在 peak 附近的 range 内 fund，即 0.25 到 1 million 之间。这就是上面那条建议的由来。

![图片](https://iiif.elifesciences.org/lax:34965%2Felife-34965-fig2-v2.tif/full/1500,/0/default.jpg)

但是！请注意，marginal productivity 并不会变成 negative，而是 tends towards zero。资金更多时，productivity 仍然 continues to increase，这一点依然成立，只不过它会下降。因此，虽然我们可以说，如果只是想得到更多 papers，就应对 grants 实行 capped，但不能据此对 maximizing 每位 investigator 的 maximum RCR 必然得出同样的说法。此外，任何特定 project 所需的金额都可能是 project-dependent 的；想要 maximize RCR 的 sum total，甚至按 RCR 排在 top 1% 的 papers 的 number，是一回事，而 maximizing 所完成的“good science”则是另一回事。在 field A 中得到 high RCR papers，可能比在 field B 中更便宜，但如果两个 fields 都相当重要，那么这意味着，按 unit of science 来看（无论这意味着什么），B projects 会更 expensive，其原因并非任何 inefficiencies，而是那个 field 的 intrinsic higher costs。

这里的另一点是，即使接受 decreasing marginal returns，我们也必须问，这是 single labs 随着 scale 扩大而变得更加 inefficient 所 driven 的，还是 high RCR papers 本身就更 expensive；labs 变大后可能引入 inefficiencies，这是 plausible 的（反过来也同样可能，必须 tested empirically）。但似乎同样成立的是，如果 high performing labs 之所以 higher performing，是由于它们的 organizational characteristics 或在那里工作的特定人员，而不是因为 [Matthew](https://nintil.com/newton-hypothesis) effect，那么以更多 grants 来 rewarding 它们，并不是坏事。

Aagard et al.（[2020](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002)）review 了有关 funding 应该 dispersed，还是 concentrated 于 elite researchers 的 literature。他们指出，_支持 increased dispersal 的 arguments 占有明显优势_。我们来逐项看看：

首先，支持 concentration 的 arguments，他们列出了以下几类：

-   Efficiency：更加 concentrating funding 可能带来 economies of scale（一个 single lab 可以 reuse equipment 或 knowledge，否则这些东西可能需要 duplicated）
-   Epistemic factors：向少数 elite scientists Giving 更多 resources，应当带来更好的 science
-   Organizational conditions：通过 pooling resources，labs 就可能 hedge their bets，并且能够 engage in riskier projects，而不在此过程中 risking 这个 lab 的 reputation。（比如，如果有 10 个 labs 做 10 个 projects，它们可能会选择 10 个 safe projects；如果是 1 个 lab 做 10 个 projects，它可能做 5 个 safe projects 和 5 个 risky projects，即便其中 4 个 fail，它仍然可以用其余的成果来“展示自己的工作”，并 justify 整个 portfolio。）

其次，反对 concentration、支持 dispersion（“egalitarianism”）的 arguments 有：

-   Efficiency：如本文前面 reviewed 的 articles 所强调的，更多 funding 可能导致 decreasing marginal returns。此外，concentrating grant funding 可能使 PIs 变成 managers，将全部时间用于 applying for grants，而不是 doing research 或 mentoring students。
-   Epistemic factors：与 efficiency 类似，spreading funding 可能带来更多、且 impact 更高的 outputs。Funding 多个 labs 会 increase 得到尝试的 approaches 的 number，从而 increasing 其中某个 approach 发现 impactful 成果的 chances。
-   Organizational issues：更多 labs 意味着 students 有更多、也更 diverse 的 opportunities
-   Peer review：Concentrating funding 需要决定谁应该获得 funding，这意味着需要一个 peer review system。如果这个 system 是 biased、unreliable 和 risk-averse 的，这就是 concentration 的一项不利之处。这也可以换一种方式来表达：即便 scientific skill 是 highly unequally distributed 的，如果我们无法 measure 那个 distribution（或许 ex-post 除外），也不妨 fund randomly。

到目前为止，我认为所有这些 arguments 都取决于 talent 的 distribution 究竟是什么样，以及 ideas 是什么的 a function of；如果 coming up with ideas 只是时间问题，那么人越多越好；反过来，如果这还取决于 individual scientist 有多优秀，那么更多人的 benefits，就可能被每位 individual 更高的 breakthrough chance 所 offset。这也取决于我们感兴趣的是 maximizing 什么。仅仅是 published 的 papers 吗？如果我们认为，只有一小部分 science 才重要，而且这类 science 可能需要更多 resources，那么，即便账面上它位于 curve 中 decreasing marginal returns 的部分，对其 funding 仍然是正确的做法。

大多数研究 funding levels 与 outputs（publications、patents、citations）之间 correlation 的 papers 发现，marginal returns 要么是 constant，要么是 decreasing，但这些 results 对于这些 decreasing returns 开始出现的 cutoff 在哪里，似乎是 heterogeneous 的。authors 还指出了一点，在我看来，这使得 drawing conclusions 远非易事，而我之前也提到过：

> 然而，把 funding size 的问题归结为一个简单的、关于支持还是反对 concentration 的 evidence 的问题，就是对一个 complex 且 multifaceted 的问题进行 oversimplify。research funding 的 concentration 与 dispersal 之间的“proper” balance，或许更准确地说是程度问题：过小和过大的 grant sizes，在 economic 和 epistemic 两方面似乎都是 inefficient 的。尽管如此，现有 research 表明，为 achieve 一个“critical mass”所需的 funding levels 未必非常高。因此，一个关键问题是，concentration 与 dispersal 之间的 balance 的“sweet spot”（或 preferred region）究竟在哪里（[Page, 2014](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002#)）。考虑到 funding dispersal 在 diversity 方面被认为具有的 benefits，迫切需要更 thorough 且 systematic 的 examinations，以确定多少 diversity、哪些 forms 的 diversity 能够支持一个更加 robust、innovative 且 forward-moving 的 scientific system（[Page, 2014](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002#)）。**然而，optimal balances 很可能 dependent on field-specific characteristics，以及与 national funding systems 的 overall configuration 相关的 factors 这两方面。**

所以，虽然我完全可以同意 authors 所说的类似“All else equal，funding 应当是 dispersed 的”这样的观点，但问题恰恰就在这个 all else equal。

作为一个 thought experiment，Vaesen & Katzav（[2017](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0183967&utm_content=bufferd2ffe&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer)）给出了在 Us、UK 和 Netherlands 对所有人 equally funding 会产生的实际 numbers。这里我重点讨论 US 的情况。这个 calculation 基于 US 中 NIH、DoE、DoD、NASA、USDA 或 NSF 向 universities 提供的 total federal R&D funding（36.68 bn$), and the total number of faculty employed at universities (317k) plus 200k PhD students and 62k postdocs. Given this they estimate that each faculty member that's doing research would get 599k$，覆盖五年，另外再加 $418k，用于支付 hiring 和 materials。这相当于给每个人 2-4 个 NIH R01s。

我们可以只针对 NIH 重新做这个 calculation；NIH [spends](https://www.niaid.nih.gov/grants-contracts/2019-extramural-research-overview#:%7E:text=As%20the%20blog%20post%20details,extramural%20grants%20to%202%2C738%20organizations.) 29bn$ per year on 55k grants; from the Lauer 2017 dataset above there may be 30k PIs funded per year (Coherent with [this](https://nexus.od.nih.gov/all/2018/03/07/how-many-researchers-revisited-a-look-at-cumulative-investigator-funding-rates/)). So we get roughly 1M$，按每位 PI _每年_计算？这大大超过了上文 Vaesen & Katzav 提到的 599+418，因为那个数额覆盖的是 5 年。这可能是因为，他们包括了所有受雇于 US 的 university、从事任何一种 research 的人（317k），而我的 PI calculation 只包括了在某一年确实得到 funded 的 PIs（30k）。我们仍然可以做到每年 500k，同时还让 NIH 正在 funding 的 PIs 数量 double。

再回到那个 Lauer dataset，假设 funding 按这种方式运作，我们可以 compute 一下，如果给每个人 1M 会发生什么，并非常天真地假设，cost 超过这个数额的 projects 就不会发生；那么，NIH 在任何一年仍然能够 fund 84-90% 的 PIs。其余那些 PIs，on average 还需要另外 1M$ 的 grant money，才能 complete 他们实际获得的所有 grants。

但这些 results 可能是 biased 的，因为某些 fields 或 grants 可能 intrinsically 比其他的更 expensive。所以，要看清 correlation 的真实情况，我们需要在 Lauer et al. 使用的 models 中 introduce 类似 PI-level fixed effects 的东西（假设一个 PI 在某个给定的 field 工作）。

Fortin & Currie（[2013](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065263)）在一定程度上 overcome 了这个 field-dependent 问题，因为他们按 field 来 split 其 analysis。他们考察了 Canada 的主要 funding authority（NSERC）所 funded 的那组 researchers 获得的 grants（每个 field 最多 139 个），并考察了若干 impact metrics（publications、citations、most cited article、number of highly cited articles）。他们发现，funding 与 output 之间几乎没有 correlation，而 outputs 的 variability 如此之大，以至于一位 highly funded researcher 的 most cited article，比一对各自获得一半 funding 的 random researchers 的 most cited article 少被 cited 14%。所以在这里，显然，funding 应当 spread thin 这一 conclusion 确实似乎是 justified 的。另外，考察那些 funding 得到 increased 的 researchers，也没有发现这会带来更多 publications；相反，researcher performance 似乎是那位 researcher 的 intrinsic 属性，

> 我们 postulate，用 bibliometric indices 来 measured 的 scientific impact，一般只是 weakly money-limited 的（尽管 grant support 的其他重要 impacts，例如 students 的 training，很可能确实会随 funding closely scale）。Nobel Prize winning research 可能是 highly funded 的，也可能完全没有 funded $$[18$$](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065263#pone.0065263-Tatsioni1)。目前，我们只能猜测，为什么 researchers 之间的 impact varies 如此之大：也许是 training、career stage、其他 responsibilities（teaching 和 administrative）、institutional priorities 方面的 differences，也许是某种难以言说、类似 talent 的东西的 differences。无论是什么 drives impact，它在 individual researchers 身上基本都会随时间 persists

这个特定的 result 在 NIH research 的情况下并不成立；如果我们采用 Lauer 的 dataset（Code 可在 [Nintil repo](https://github.com/jlricon/open_nintil/tree/master/funding_lotteries) 获取），sample 2 位 funded 数额在 median 附近的 PIs，以及 1 位 funded 数额为 median 的 2x 的 PI，从这两个 groups 分别取 max RCR，并 repeat 这个 process 50000 次，那么约有一半的时候，那位 highly funded PI 会有更高的 maximum RCR。但在 quantifying 那个 difference 时，如果看 high 和 low funded researchers 的 RCRs 之间 ratio 的 mean，这个 number 是 2.56。所以，并不是 Fortin & Currie 发现的 egalitarianism 的 14% advantage；采用类似的 methodology，Lauer dataset 显示，NIH funded researchers 的这个 number 是 256%。也许这是因为这个 sample 比 Fortin & Currie 的更大（给 outliers 留下了更多空间），或者这是 RCR 作为 bibliometric indicator 的一个特殊之处，又或者 NIH papers 总体上就是具有 high variance。我做的另一个 check（见 notebook），是看当我们从 distribution 中 draw 更多 researchers 时，max RCR 会如何变化。sampling 更多获得 median 附近 funding 的 researchers，是否会 increases maximum RCR（on average）？如果我们是从 normal distribution 中 sampling，答案会是否定的，我们之前见过这一点（[这里](https://nintil.com/grant-peer-review)的注 2），但在这里，我们得到的是 increasing maximum RCR，所以通过 spreading funding 来购买更多 variance，似乎是有回报的。

但是！如果我们考察按 RCR 排在 top 1% 的 work（>59.29），就会发现，这些 projects 的 median funding 几乎是 1M$（超过 median 的两倍），因此，我们面临的情况与我对 Li & Agha paper 的评论类似：high funding 并不 imply high impact，但与此同时，high impact 往往 necessitates high funding。不同 impact categories 的 pattern 都类似（Upper 0.1% RCRT quantile，或 0.01% RCR quantile）。

![图片](https://nintil.com/images/2021-01-14-funding-lotteries/27587981-383A-4DED-ADF5-40D357747E61.png)

所以，与其采用 Fortin & Currie 的做法，我们也可以问，如果实施一项 policy，规定任何 PI 获得的 funded 数额都不超过某个给定的 amount，会产生多少 top papers；首先，我从每个 bucket 中 sample 100k PI-years。我发现，funding cap 会 decrease 按 RCR 排在 top 的 papers 的数量。

![图片](https://nintil.com/images/2021-01-14-funding-lotteries/4D7978BA-6F5B-4AC8-96E5-84C4ECC8075C.png)

这对 cap 并不公平，因为 cap 意味着我们可以用给定的 budget 来 fund 更多 grants，所以，让我们 repeat 之前的 exercise（Sampling 1 位 highly funded PI，或者 N 位各自获得 k 倍 funding 的 PIs），N=1/k，但不再看谁获得了更高的 RCR，而是看我们在每个 bucket 中得到了多少 highly cited papers。在 k=2 时（也就是说，我们 fund 50k 个数额为 median 两倍的 grants，对比 funding 100k 个数额在 median 附近的 grants），答案是，highly funded group produces 更多按 RCR 排在 top 1% 的 papers（多 22%）。

暂且不论 RCR 作为 metric 的 particularities，以及 field-specificity 的问题，这说明 science 中的 optimal funding structure，取决于人们想要实现的 metrics 是什么。如果只是追求 average higher impact，那么 funding caps 是合理的；但如果你想要更多 top papers（如果你假设只有 [top counts](https://nintil.com/newton-hypothesis)，这就是合理的），那么就不应有 caps。反对这一点的一种 argument 是，即使 funding 更少，你仍然能获得 top results；给某些 PIs 更多钱，会让他们去使用更花哨的 tools；他们可能不会更有创意地使用已有的 equipment 和 tools 来达到相同目的，而是会购买最新的 kit，尽管它相较于前一款只 marginally improves。再一次，这只能通过 experiment 来解决。

Janssens et al.（[2017](https://peerj.com/preprints/3106.pdf)）指出，虽然拥有一个按 field 和 year 来 normalizes citations 的 metric 值得赞许，但 RCR 远非 ideal：

> researchers’ portfolios 的 annual weighted RCR 的 calculation，assumes 一篇 scientific article 的 RCR 是 constant over time 的，但事实并非如此：8 当一篇 article 过了它的全盛期，RCR 就会 decreases。当一篇 article 的 number of citations ‘stabilizes’，而年数不断 increasing 时，它的 citation rate（RCR equation 中的 numerator）就会 decreases over time。expected citation rate（RCR equation 中的 denominator）是那些 ‘field’ articles 所 published 的 journals 的 normalized 2- year citation rate。Using 一个 2-year citation rate，就 assumes 一篇 article 的 citation rate 是 constant over time 的，而且始终与最初两年一样高，这对于较早的 articles 来说是不切实际的。一个 decreasing numerator 和 overestimated denominator，会使大多数较早的 articles 得到 reduced RCRs。当 scientific articles 的 RCRs 随时间 decrease 时，annual weighted RCR 就不是衡量 mid- and late-career researchers 当前或近期 scientific influence 的 suitable indicator，因为他们较早的 work 会 pulls down 其 portfolio RCRs。他们的 annual weighted RCR 的 reduction，可能至少在一定程度上 explain 了 NIH data 中 observed 的 diminishing returns，因为具有更高 GSI scores 的 researchers 拥有长得多的 grant funding 历史（GSI>21 者的 median 为 19 年，而 GSI≤7 者为 3 年）

更糟的是，NIH 似乎并未考虑过 experimenting；他们不该只是提出 instituting 一个 cap，而应当把 PIs randomized 到有 cap 或无 cap 的 conditions 中，然后 measure outputs。这似乎从未被纳入考虑，但本来应该如此。

## 理性思考 science 中的 funding allocation

设想一个 lab 希望获得 1M$ for a study and two labs want 0.5M$，分别用于另一个不同 field 的 study。上述考量会倾向于 funding 两个 labs，而不是一个。但现在假设，这一个 lab 有 producing very highly cited（而且受到 experts 赞誉的）work 的 track record，而另外两个没有。或者假设这些 labs 同样 capable，但 lab A 所在的 field 往往需要进行更 expensive 的 experiments。weighting 应该在 lab 的 level 还是 field 的 level 上进行？

现在考虑，第一个 lab 工作在 oncology 一个新 subfield 的 forefront，过去曾 produced basic research，带来了一些 insights，现有 drugs 就是以这些 insights 为基础的；而第二个则试图更好地 understand 一种 ultra-rare disease。你会 fund 什么？

对于这第二种情形，有一条更简单的 rule of thumb：根据每种 disease 的 prevalent 程度，对 targeting 该 disease 的 research 进行 Fund，并按其 serious 程度进行 weighted。因此，如果 disease A 导致的 QALY/DALY lost 是 disease B 的 2x，那么前者就应获得两倍的 funding。一个 funder 可能选择给 1M$ to one field and 0.5$ 给另一个。但在同一个 field 内，per-lab allocation 的问题依然存在。

上述关于 efficiency 的 studies 可能想表达的是这样一种怀疑：_如果我们能够 control for field-specific costs_，那么有些 labs 会比其他 labs 更 efficient；而我推断，如果能做到这一点，我们就可以据此 tailor grants；如果 lab A 进行一个 scRNA-seq study，在 mice 中 examining liver，其 costs 为 C，那么，如果 lab B 提议做同样的 study，只不过对象是 pancreas，而费用要多 40%，或许 B 就不应获得任何 money，而应把这笔钱给那些达到某个 efficiency threshold 的 researchers。

Shahar Avin（你好，Shahar！）在 [2015](https://link.springer.com/chapter/10.1007/978-3-319-23015-3_9) 和 [2019](https://www.sciencedirect.com/science/article/pii/S0039368118300190) 的 articles 中 reviewed peer review 和 lotteries 的 effectiveness，而[他的 PhD thesis](https://www.repository.cam.ac.uk/handle/1810/247434) 则更广泛地考察了 science 的 funding；他指出，维持一个 peer review system 的 costs 可能达到 total funding 的 14% 左右，而且这个 system 远非 perfect（我在[这里](https://nintil.com/grant-peer-review) reviewed 了 grant peer review 背后的 evidence）。他还讨论了 lottery system 的一个潜在弊端（或益处）：在 pure lottery 下，scientists 无法指望获得比如 10 或 20 年的 continuous funding，他们不知道自己会不会在下一个 period 抽中 ticket，而且无论做什么都无法改变这一点。

他提出了一个此前尚未讨论的观点（我原本希望以后专门写一篇讨论 bibliometrics 的文章）：人们可以想看多少 citations 和 RCRs 就看多少。但我们应该关心的是 science 的 social value：Science 太重要了，不能只交给 scientists，而 science 的 structured 方式，只是追求 society 赋予 science 的任务的一种手段。按大多数人的 values，这意味着 produce 与改善人们生活的 innovation 相关的 research；相较之下，纯粹为了 knowledge 本身而 broaden 其 frontiers 的重要性要小得多。如果我们无法知道哪些 research 会是 socially beneficial，那么无论对 highly cited research 做什么 analysis 都无关紧要，我们还是会默认回到 randomizing funding。funding 第 nth 个 -seq（如今已经有[这么多](https://en.wikipedia.org/wiki/List_of_single_cell_omics_methods)了），会比 funding 一篇 cancer research paper 更 worthwhile 吗？即便我们预期前者会获得更多 citations，也远不能确定，它就是最有可能促进那些最终让人们受益的 products 的 development 的途径。

## Lottery proposals：lottery 方案

在 Fund People, not Projects 系列中，我曾陆续提到各种可以用来改变 science funding 方式的 experiments。Ioannidis（[2011](https://www.nature.com/articles/477529a)）、Gross & Bergstrom（[2019](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3000065)）、Fang & Casadevall（[2016](https://mbio.asm.org/content/7/2/e00422-16)）提出了一组很好的方案：

-   对所有人 equally 进行 Fund
-   对一组 grants 进行 at random 的 Fund
-   对一组 grants 进行 at random 的 Fund，同时询问 reviewers 该 grant 是否没有 major flaws
-   对一组 grants 进行 at random 的 Fund，同时询问 reviewers 该 grant 是否是 meritorious

我还可以加上自己的一些想法，

-   与上述相同，但使用 bibliometrics-based model 来“给骰子加权”；例如，如果你是一位 accomplished researcher，就能在 lottery 中获得更多 chances。
-   设置持续 1,5,10, 和 20 年的 open-ended 项目，然后对它们使用 lotteries
-   与前述相同，但以持续 10 年的 grant 为例，让一半 researchers 是“successful”的（由 bibliometrics 证明），另一半则真正从一个 random pool 中 selected，同时接受 minimum review。

## science funding 的 politics

我们看到的 funding modalities experiments 可能比期望的少，其中一个原因是 politics。Barnett（[2016](https://mbio.asm.org/content/7/4/e01369-16)）报告了一家 Australian funding agency 对为何尚未尝试 lotteries 的回答：不是因为这种方式不可行，而是因为_这会让我们看起来不知道自己在做什么_；该 agency 担心丢脸，而这似乎妨碍了 experimentation。Beattie（[2020](https://www.nature.com/articles/d41586-020-00133-1)）在给 _Nature_ 的一封信中抱怨，lotteries 只不过是 inefficient bureaucracy 试图应对 science 运作方式的一种办法，并呼吁 _Academia \[应\]抵制那些出于好意却 scientifically inept 的 bureaucrats 对 research 和 publishing 的这种 bureaucratization_；而 Vindin（[2020](https://www.nature.com/articles/d41586-020-00136-y)）则反对 grants 这一想法，理由是它不会有利于 merit，也不会有利于需要 longer term funding 的 work。

总体而言，Scientists 对 lotteries 这一想法意见不一。Philipps（[2020](https://academic.oup.com/rev/advance-article/doi/10.1093/reseval/rvaa027/6017979?login=true)）interviews 了 Germany 的 32 位 scientists，其中既有 senior 也有 junior，涵盖 life and physical sciences；虽然他们支持 peer review 的想法，但也认为应该给 lotteries 一个机会，只是或许不应采用 fully randomized 的方式；例如，其中一人说，如果完全是 random，就可能导致 researchers 为了在 lottery 中获得更多 tickets，而 write 大量 lower quality grants。这凸显了 lotteries 既可以在 researcher level，也可以在 project level 上进行；Lotteries 在“Fund people”和“Fund projects”这两种世界中都能运作。

Liu（[2020](https://researchintegrityjournal.biomedcentral.com/articles/10.1186/s41073-019-0089-z)）是另一项与 Philipps (2020) 思路相同、但更广泛的 survey，地点在 New Zealand，有 126 位 scientists 参与。在这里，scientists 似乎更支持对 exploratory grants 使用 randomization（63% 的人支持），但对于其他 grants，总体上几乎各占一半。反对 lotteries 的人提出的一些具体 arguments 似乎不太有力；例如，“我不认为 randomisation process 比某个 reviewer 找个 minor reason，让一个 great project 无法获得 funded 更不 fair。”；假设这两个 processes 同样 random，lotteries 仍然具有更 cheaper 的优势，因为它们不需要任何 reviewer time（或者需要得更少）。对于一种“messy middle”的 lottery funding model，似乎存在一定共识：其中“obviously good”的 applications 获得 funded，“obviously bad”的则被 rejected，其余全部 at random 地获得 funded。

## 对 lottery system 的 Critiques

关于 lotteries 的大多数 research work 都持赞成态度。我试着寻找明确 criticisms 使用 funding lotteries 这一想法的 research。

除了 Beattie 和 Vindin 的两篇评论，我唯一能找到的是 Bedessem ([2019](https://academic.oup.com/rev/article-abstract/29/2/150/5678703?redirectedFrom=fulltext))，他似乎认为，支持 lotteries 的一个 argument 是错误的，即我们（或者某个 field 的 scientists）无法知道什么会是 good science，因为 success 的发生非常 randomly；原因在于

> 它低估了这样一个事实：scientific practice 的 systems 的 structure 会 strongly constraints（或者比 pro-lottery literature 中所假定的更 strongly）哪些 project 可以被认为是（相对）interesting——也就是说，对（相对）大量的 scientific practices 有用，包括 technical、experimental 或 theoretical activities。由于这些 interconnections，似乎有理由作如下 suppose：1. 并不存在大量在 theoretically、experimentally 和 technically 方面与现有 systems of practices strongly disconnected 的 ‘exploratory’ projects；以及 2. 如果我们 take into account 它们所追求的各种不同种类的 objectives，就至少在 theoretically 上有可能对 ‘equally good’ projects 的 value 进行更 precise 的 comparison。

尽管这一点是对的，pro-lotteries 一方仍可以回应：a) 即便承认大多数 science 是 exploratory，在任何时候，promising 的 non-exploratory projects 都多于能够获得 funded 的数量。因此仍然存在如何在它们之间 deciding 的问题；b) 这确实有可能，但 cost 是多少？Predicting success 很难！

所以，尽管他在抽象层面是对的，他的顾虑仍须与 pro-lottery 阵营提出的同样 valid 的 arguments 一起 weighted。

## 时间这一 factor

假设采用 lottery system，就必须决定 funding period。相对于现状，这种 system 会让那些确实需要更多时间才能 succeed 的 projects 更加困难。[Braben](https://ideamachinespodcast.com/donald-braben) 说，Planck 在 thermodynamics 的 foundations 上工作了 20 年，才形成 postulating the quantization of energy 的想法。不管这是否完全属实，确实可以设想，research 的 quality 可能是 scientist 在其职位上有多安稳的 function。

scientists 一般需要多少时间？如果大多数 science，以及大多数 useful science，都能在 5 年内完成，那么在 NIH 中实行 randomized R01 lottery 就不会受到这一 objection。如果只有很少一部分人会从超长期 funding 中受益，那么可以将这种 lottery 与另一种能够提供更长期 support 的 lottery 结合起来。

## Conclusion（结论）

这里 reviewed 的 evidence 支持将 lotteries 用作 science funding ecosystem 的一部分。lotteries 的支持者可能低估了 peer review 能够 detect impactful work 的程度，也低估了 citations 能够与 downstream practical applications 联系起来的程度，但总体而言，他们的 cost-benefit analysis 是合理的。我相信可以 designed 出一种吸引 funders 和 scientists 的 system，从而让我获得支持，但这不会是一种 pure lottery。显然，我必须以呼吁开展 testing 各种 lotteries 的 experiments 来结束这篇文章；不过，如果由我负责 implementing 任何此类 system，我会采取如下做法：

-   Part A "The HHMI-lite"（funding 的 85%）
    -   Proposals 采用 HHMI style，篇幅相对较短，proposing 一个 broad research idea，而不是 R01 style 的 narrow project。它们只需被 sorted（或许由一个 review section 负责）到三个 buckets 中：必须 fund、可以 fund，以及不 fund。必须 fund 的 papers 都会得到 funded。如果 funding needs exceed 可用的 funding，就对它们进行 chosen at random。Reviewers 在其 review 的 papers 中，只能将少量 papers designation 为“必须 fund”。可以 fund 的 papers 都以 at random 的方式得到 funded。
    -   要求的 funding 超过某个 threshold 的 Projects，需要更长的 proposals，并接受额外 scrutiny
    -   Funding 期限为 5 年
-   Part B "The Planck club"（funding 的 5%）
    -   Proposals 与 Part A 中相同，但 reviewers 会被要求只 select 最优秀的 applicants。在这批人中，按照 funding 所允许的数量，chosen 一个 random set。
    -   Funding 期限为 20 年，在第 10 年进行一次 review（只有在出现 substantial underperformance 的情况下，才不会 awarded 另外 10 年），并且有额外资金可用于 equipment。
    -   必须在 proposal 中 justified 更长期 funding 的必要性
    -   对某个人只能 granted 一次
-   Part C "The PseudoARPA"（funding 的 10%）
    -   这里由一个 permanent program managers 组成的 panel 来 designs research programs，然后致力于 execute 这些 programs，为多个 labs 中的 scientists 提供 funding，或引入 industry expertise，等等。
    -   PMs 有权酌情 increase、decrease 或 completely cut 某个 project
    -   PMs 会被要求 fund 那些 single labs 不会从事的 opportunities，原因可能是他们所设想的 project 需要 coordination，也可能是其 funding requirements 所致

此外，禁止将 grant money 用作 salary，并要求 universities 向 grant 的 recipient 支付 full salary。这项 proposal 中使用的 lottery 不会是 truly random；所申请的资金数额将被赋予 inverse weight。

_Ricón, José Luis, “Fund people, not projects IV: Scientific egalitarianism and lotteries”, Nintil (2021-01-20), available at https://nintil.com/funding-lotteries/._

---

## 原文

One of the hottest topics right now in the world of meta-science is using lotteries to fund research. In a nutshell, the rationale is that it's hard to tell who or what will be successful, and it is very costly to try to do so as well. A system that relies on peer review demands of researchers time to write grants and to review them. I have reviewed before the effectiveness of peer review at [figuring](https://nintil.com/grant-peer-review) out "what's good" in the context of grant awards, finding that peer review as currently practiced does substantially better than chance at predicting future impact, especially for the most impactful of the papers; but at the same time is is far from perfect, leaving plenty of variance unexplained. The way I see this is that reviewers can identify what's obviously not great and what is obviously great (Perhaps identifying the latter is easier than the former). But there is a "messy middle" where it is unclear what to fund.

Peer review systems are not cheap. In the case of Australia, Gordon and Poulin ([2009](https://pubmed.ncbi.nlm.nih.gov/19247851/)) say that the cost of peer review is greater than just giving everyone a grant. It gets worse. Guthrie ([2018](https://f1000research.com/articles/6-1335)) reports that:

> _The burden of the peer review system is high and falls primarily on the applicants._ The overall monetised cost of the peer review system, including application preparation, has been estimated to account for **as much as 20–35 per cent of the allocated budget** ([Gluckman, 2012](https://f1000research.com/articles/6-1335#ref-46)). [Graves _et al._ (2011)](https://f1000research.com/articles/6-1335#ref-48) report that the monetised costs of the application system for NHMRC are $14,000 per grant, whilst extrapolating RCUK ([Research Councils UK, 2006](https://f1000research.com/articles/6-1335#ref-88)) estimates suggests that the costs of the application process are 10–17 per cent of the total cost of research. An evaluation of the CIHR Operating Open Grants Program (OOGP) found the application cost of OOGP grants to be Can$14,000 ([Peckham _et al._, 2012](https://f1000research.com/articles/6-1335#ref-83)). A detailed review of preparing grants for NIH for nursing research reports similarly high costs to institutions ([Kulage _et al._, 2015](https://f1000research.com/articles/6-1335#ref-124)). When providing congressional testimony individual researchers have estimated that as much as 60 per cent of their time is devoted to seeking funding ([Fang & Casadevall, 2009](https://f1000research.com/articles/6-1335#ref-34)).

This may seem excessive, but we can do a quick calculation to show that this is not farfetched. Assume that there are only PIs participating in grant application and reviews. The H's here represent time, with S being success rates (In applying for a grant or getting a paper published). 2080 is how many hours of research would there be assuming (This is far from true) a 40 hour week job. I have taken the total number of PIs from [here](https://nexus.od.nih.gov/all/2018/03/07/how-many-researchers-revisited-a-look-at-cumulative-investigator-funding-rates/) and number of published papers from [here](https://www.nih.gov/about-nih/what-we-do/impact-nih-research/our-knowledge#:%7E:text=In%202016%20alone%2C%20over%20115%2C000%20articles%20acknowledged%20NIH%20grant%20support.&text=Each%20R01%20grant%2C%20NIH's%20most,of%207.36%20published%20research%20articles.). The time it takes to write a grant varies between 170 to 270 hours, with the average being 200 (Boyack et al., [2018](https://www.researchgate.net/profile/Kevin_Boyack/publication/321581221_Toward_predicting_research_proposal_success/links/5a562ad2aca272bb6963c12e/Toward-predicting-research-proposal-success.pdf)). With these assumptions, or any plausible changes to them, we see that the average researcher would indeed be spending 60% of their time seeking funding. Increasing the 40 hour week to a 60 hour week (3120 hours) would still imply 40% the time spent on applying for grants. The cost of peer review is largely dominated by applying for grants, not peer reviewing. My model assumes that the average PI would be reviewing 13 papers a year. This is all averages; my model implies that on average a PI is writing 5 grants a year. Here's a [case](https://twitter.com/michaelzlin/status/1350933138055294976) of spending 120h to write a grant, and having to apply 7 times on average. The point is that if the numbers are roughly correct, the conclusion that scientists have to spend a lot of time applying and reviewing for grants and reviewing papers is inescapable.

$$
H=H_{research}+H_p+H_{other}+H_g=2080\\

H_g=\frac{H_{one\space grant}}{S_{grant}}=\frac{200}{0.2}=1000\\

H_p=\frac{N_{papers}}{N_{scientists}}*\frac{H_{{one\space review}}}{S_{{journal}}}*N_{reviews \space per \space paper}=\frac{115000}{30000}*\frac{5}{0.3}*4=256
$$

This is not, to be sure, and it is important to remark so, directly related to the number of hours of researcher time that are lost to peer review; that is if there were no peer review the total hours of science being done wouldn't double despite PIs spending half their time applying for grants. This is because the research workforce has more members than just PIs. If an average lab has 7 members (including the PI, Conti & Liu, [2014](https://www.sciencedirect.com/science/article/abs/pii/S0048733315000037); Cook et al., [2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4465944/)) and they all work the same number of hours, removing the grant system altogether would not yield 2x the hours of science, but just 1.16x more. But! This would be undervaluing the benefits for at least two reasons: a) The PI role may be special; an hour of them may be worth more than an hour of the graduate student and above all b) Not being bound by the grant system means that labs might explore riskier ideas. This, I expect, would be the key channel through which the benefits of not having peer reviewed would accrue, and this would have to compensate for an an increase in funding for work that wasn't as good (And that wouldn't have made it under the peer review system).

## Academic salaries and incentives

Academia is weird. In most jobs you get paid a salary that accrues for every hour worked. In academia many PIs are on _soft money_ positions; they are only paid for part of the year and the rest they are expected to get grants for. Moreover if they don't get grants they get kicked out of the system. Even tenured professors, if they don't get grants could probably be passive-aggressively forced out. Grants are also needed to support the lab, the university pays the basic salary but running a lab, especially in the life sciences involves hiring postdocs, graduate students, and buying reagents and equipment. So academics, both to progress and stay in the system _have to get grants_. Perhaps getting a grant may be more important than what the grant is for, leading to a situation where PIs say they will work on X where they actually do Y which they consider more worthwhile. In theory that shouldn't matter: Funding agencies value novelty, but in practice what matters is not what you think is novel, valuable and worthwhile but what you expect the panel to judge as so.

Why is this the case? Why is science funded like this, at least in the US? Bourne (2018) [says](https://www.pnas.org/content/115/35/8647) (without citation) that back in the 70s, 75% of the salaries were paid directly by the university, whereas now that number may be more like 35%, so it's not like it has always been like that. It would take a longer post to go in depth on this topic.

## Scientific egalitarianism and grant efficiency

Science is highly unequal ([Peifer, 2017](https://www.molbiolcell.org/doi/pdf/10.1091/mbc.e17-07-0462)): 10% of PIs get 40% of the funding, 20% of them get 56% of it, and 1% gets 11% . This is not necessarily a bad thing, some projects may need more funding than others. Peifer argues that there are diminishing returns to grant money, so if one wanted to produce a set of papers that in the aggregate get the most citations, funding should be made more egalitarian. One could still say that what matters more is the most cited papers, not maximizing the average per paper and in that case funding inequality could be justified. Peifer documents that more than half of the labs producing the most cited work have less than 21 points on the [Grant Support Index](https://www.sciencemag.org/news/2017/06/updated-nih-abandons-controversial-plan-cap-grants-big-labs-creates-new-fund-younger) (GSI; That is, 3 R01s or less). Anecdotally, a reason why Ed Boyden couldn't get funding for his work on expansion microscopy was that he already was holding "[too many grants](https://twitter.com/ArtirKel/status/1344351782869221376)", so the NIH is formally or informally doing some version of this (His lab may be 6x as large as the average lab, so it's not necessarily the case that they do very expensive projects; they may be just doing lots of them in parallel).

Wahls ([2018](https://elifesciences.org/articles/34965)) concludes the same, that PIs should each get 400k$ to maximize return on $ spent as measured by total citation count (As it happens that's not too far from an R01). A more generous cap, of 1M$ per investigator, coupled with additional grants of 200k$ each would free up money to support 10000 additional investigators (This implies increasing the total number of funded NIH researchers by 38%).

Wahls doesn't say if this would reduce the number of impactful publications. Plausibly more expensive grants are required for more impactful research? Wahls cites a couple of papers to support the "cheaper grants are more efficient view". Lorsch ([2017](https://www.molbiolcell.org/doi/10.1091/mbc.E14-06-1163)) refers to an earlier study done on NIGMS (National Institute of General Medical Sciences) that shows that the peak in terms of impact and impact factor (of the journals where the papers end up) is substantially higher than R01; the peak seems to be somewhere in the 600-1000k$ range. In the figure, oddly there is a (unexplained, in the paper) sharp drop in productivity beyond that range.

![image-20210118183409361](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118183409361.png)

Basson et al. ([2016](https://loop.nigms.nih.gov/2016/07/revisiting-the-dependence-of-scientific-productivity-and-impact-on-funding-level/)) does look at (a normalized measure of) citations (Again, NIGMS) in the period 2011-2015 and here there does not seem to be much of a trend; in fact it seems most of the very highly cited papers are concentrated in the sub-500k$ range.

![](https://loop.nigms.nih.gov/wp-content/uploads/2016/07/fig6_bg.jpg)

Lauer et al. ([2017](https://www.biorxiv.org/content/10.1101/142554v2)) includes all NIH-funded investigators in the period 1996-2014 (71k PIs) that published 1M papers. Table 1 has some summary statistics bucketed by GSI (An index of how well funded a PI is; an R01 is equivalent to GSI=7); the (mean, median,max) relative citation ratio goes _up_ substantially with a higher GSI, below I highlight the maximum as I'm particularly interested in that as a proxy for outstanding research; the mean goes up by 2x, but the maximum goes up by 10x which is quite significant. The fact that all impact-related metrics go up adds robustness to this.

![image-20210118184651767](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118184651767.png)

The paper also takes our favorite researchers, HHMI [investigators](https://nintil.com/hhmi-and-nih), who are (either because of the award or due to their intrinsic capabilities) more productive than the rest and considers them separately. Unsurprisingly no matter mean, median, or maximum, their work is more impactful. In fact, if you take the average maximum RCT from the lowest quartile of HHMIs (5.0), that's higher than the _median_ maximum RCR of non HHMIs. Looking at the median would be highly misleading here; HHMIs don't seem that much more productive, their outperformance does not come from publishing more, it comes from publishing more extremely well cited papers.

![image-20210118191453754](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118191453754.png)

There are decreasing marginal returns to producing the most highly cited work In the general sample, as shown below. Beyond 21 there doesn't seem to be much of an increase.

![image-20210118191810074](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118191810074.png)

The same is true by breaking the sample by HHMI status. Both axes are log-transformed so it's hard to appreciate the differences between both groups though.

![image-20210118192109202](https://nintil.com/images/2021-01-14-funding-lotteries/image-20210118192109202.png)

The curves look similar for the mean or the median, so if one had a fixed amount of money and wanted to maximize marginal productivity per PI one would fund in the range that's around the peak, between 0.25 and 1 million. This is where the recommendation above comes from.

![img](https://iiif.elifesciences.org/lax:34965%2Felife-34965-fig2-v2.tif/full/1500,/0/default.jpg)

But! Note that the marginal productivity doesn't become negative, it tends towards zero. It still remains true that productivity continues to increase with more money, it's just that it goes down, so whereas we can say that if we want to get more papers period grants should be capped we can't necessarily say the same for maximizing the maximum RCR per investigator based on this. Additionally, it can be that the amounts required for any given project are project-dependent; it is one thing to want to maximize the sum total of RCR or even the number of papers in the top 1% by RCR but another one is maximizing "good science" done. Getting high RCR papers in field A may be cheaper than doing to in field B, but if both fields are reasonably important then this means that by unit of science (whatever that may mean), B projects are going to be more expensive not due to any inefficiencies but due to the intrinsic higher costs of that field.

Another point here is that accepting the decreasing marginal returns, we have to wonder if this is driven by single labs becoming more inefficient with scale, or if high RCR papers are by their nature more expensive; it is plausible that as labs get bigger they may introduce inefficiencies (The opposite is also true, it has to be tested empirically), but it also seems true that rewarding high performing labs with more grants is not a bad thing if they are higher performing due to their organizational characteristics or the specific people that work there rather than because of a [Matthew](https://nintil.com/newton-hypothesis) effect.

Aagard et al. ([2020](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002)) review the literature on whether funding should be dispersed or concentrated on elite researchers. They point to _a strong inclination toward arguments in favor of increased dispersal_. Let's break that down:

First, in favor of concentration they point to the following kinds of arguments:

-   Efficiency: More concentrating funding may lead to economies of scale (A single lab may reuse equipment or knowledge that would otherwise be duplicated)
-   Epistemic factors: Giving more resources to a small number of elite scientists should lead to better science
-   Organizational conditions: By pooling resources, it's possible for labs to hedge their bets and be able to engage in riskier projects without risking the reputation of the lab in the process. (Say if you have 10 labs doing 10 projects they may choose 10 safe projects; if there is 1 doing 10 projects they may fo 5 safe projects and 5 risky ones, even if 4 of those fail they can still 'show their work' with the rest and justify the portfolio as a whole.)

Second, against concentration and in favor or dispersion ("egalitarianism"), we have:

-   Efficiency: As highlighted in the articles reviewed before in this post, more funding can lead to decreasing marginal returns. Also, concentrating grant funding can make PIs into managers that spend all their time applying for grants instead of doing research or mentoring students.
-   Epistemic factors: As with efficiency, spreading funding may lead to more and higher impact outputs. Funding multiple labs will increase the number of approaches that get tried, increasing the chances that one of them finds something impactful.
-   Organizational issues: More labs mean more and more diverse opportunities for students
-   Peer review: Concentrating funding requires deciding who should get the funding, implying a peer review system. To the extent that this system is biased, unreliable, and risk-averse, this is a negative of concentration. This is another way of expressing the idea that even if scientific skill is highly unequally distributed, if we can't measure that distribution (except perhaps ex-post), we may as well fund randomly.

So far I see all these arguments as hinging on what the distribution of talent looks like, and what ideas are a function of; if coming up with ideas is just a matter of time then the more people the better; conversely if that also depends on how good the individual scientist is then the benefits of having more heads can be offset with a higher chance of a breakthrough per individual. It also depends on what are we interested in maximizing. Is it just papers published? If we think that it is only a small part of the science that matters, and that such science can require more resources then even when on paper it is in the decreasing marginal returns part of the curve, funding that is still the right thing to do.

Most of the papers studying the correlation between funding levels and outputs (publications, patents, citations) find either constant or decreasing marginal returns, but these results seem to be heterogeneous as to where the cutoff for the onset of those decreasing returns would be. The authors also note something that to me make drawing conclusions not at all straightforward and that I mentioned earlier:

> However, reducing the issue of funding size to a simple question of evidence for or against concentration would be to oversimplify a complex and multifaceted problem. The “proper” balance between concentration and dispersal of research funding may be more accurately described as a matter of degree: Both too small and too large grant sizes appear to be inefficient in both economic and epistemic terms. Notwithstanding, the available research suggest that the funding levels needed to achieve a “critical mass” may not necessarily be very high. Hence, a key question concerns where the “sweet spot” (or preferred region) in the balance between concentration and dispersal is to be found ([Page, 2014](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002#)). Given the presumed benefits of funding dispersal with respect to diversity, there is an urgent need for more thorough and systematic examinations of how much diversity and which forms of diversity that could accommodate a more robust, innovative, and forward-moving scientific system ([Page, 2014](https://www.mitpressjournals.org/doi/full/10.1162/qss_a_00002#)). **The optimal balances are, however, likely to be dependent on both field-specific characteristics and factors related to the overall configuration of national funding systems.**

So whereas I have no problem in agreeing with the authors on something like "All else equal funding should be dispersed", the problem lies in that all else equal.

As a thought experiment, Vaesen & Katzav ([2017](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0183967&utm_content=bufferd2ffe&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer)) have the actual numbers that would result from funding everyone equally in the Us, the UK, and the Netherlands. I focus here in the US case. The calculation is based on total federal R&D funding in the US to universities from NIH, DoE, DoD, NASA, USDA or NSF (36.68 bn$), and the total number of faculty employed at universities (317k) plus 200k PhD students and 62k postdocs. Given this they estimate that each faculty member that's doing research would get 599k$ for a five year period, plus another $418k to pay for hiring and materials. This would be equivalent as giving everyone 2-4 NIH R01s.

We can redo the calculation for NIH alone; NIH [spends](https://www.niaid.nih.gov/grants-contracts/2019-extramural-research-overview#:%7E:text=As%20the%20blog%20post%20details,extramural%20grants%20to%202%2C738%20organizations.) 29bn$ per year on 55k grants; from the Lauer 2017 dataset above there may be 30k PIs funded per year (Coherent with [this](https://nexus.od.nih.gov/all/2018/03/07/how-many-researchers-revisited-a-look-at-cumulative-investigator-funding-rates/)). So we get roughly 1M$ per PI _per year_? This is substantially more than the 599+418 that Vaesen & Katzav mentions above, as that is for 5 years. This may be due to them including literally everyone that is employed by a university in the US doing research of any kind (317k) and my PI calculation is including just PIs (30k) that did get funded in a given year. We could still get to 500k per year and still double the number of PIs NIH Is funding.

Going to that Lauer dataset again, assuming funding worked like this we can compute what would happen had we given everyone 1M and assuming, very naively, that projects that cost more than this would not have happened; then NIH would have still been able to fund 84-90% of PIs on any given year. Out of the remainder, on average those PIs would have needed another 1M$ in grant money to be able to complete all the grants they actually got.

But these results may be biased due to the fact that some fields or grants may be intrinsically more expensive than others. So to see what the correlation really looks like we would need to introduce something like PI-level fixed effects (Assuming that a PI works in a given field) in the models Lauer et al. are using.

Fortin & Currie ([2013](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065263)) to some extent overcome the field-dependent problem, as they split their analysis by field. They consider grants (up to 139 per field) given to the set of researchers funded by the main funding authority in Canada (NSERC), and consider a number of impact metrics (publications, citations, most cited article, number of highly cited articles). They find that there is almost no correlation between funding and output, and variability in outputs is so the most cited article of a highly funded researcher was 14% less cited than the most cited article from a pair of random researchers that received half the funding each. So here, clearly, the conclusion that funding should be spread thin does seem justified. Also, taking researchers that had their funding increased did not lead to more publications; rather researcher performance seems to be intrinsic to that researcher,

> We postulate that scientific impact, as measured by bibliometric indices, is generally only weakly money-limited (although other important impacts of grant support such as training of students probably do scale closely with funding). Nobel Prize winning research may be highly funded, or not funded at all \[[18\]](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0065263#pone.0065263-Tatsioni1). At present, we can only speculate about why impact varies so greatly among researchers: perhaps differences in training, in career stage, in other responsibilities (teaching and administrative), in institutional priorities, or perhaps differences in something ineffable, akin to talent. Whatever drives impact, it largely persists through time in individual researchers

This particular result is not true in the case of NIH research; if we take Lauer's dataset (Code available at the [Nintil repo](https://github.com/jlricon/open_nintil/tree/master/funding_lotteries)) and sample 2 PIs that were funded around the median and 1 PI that was funded 2x the median, we take the max RCR from both of those groups and repeat the process 50000 times, about half of the times the one highly funded PI has a higher maximum RCR. But when quantifying that difference, looking at the mean of the ratio between the RCRs of the high and low funded researchers that number is 2.56. So rather than the 14% advantage for egalitarianism that Fortin & Currie found, the Lauer dataset shows that the number is 256% for NIH funded researchers using a similar methodology. Maybe this is because this sample is larger than Fortin & Currie's (More space for outliers), or it's a quirk of the RCR as a bibliometric indicator, or maybe NIH papers just have high variance in general. Another check I did (See notebook) was to see how max RCR behaves as we draw more researchers from the distribution. Does sampling more researchers that got funding around the median increases the maximum RCR (on average)? If we were sampling from a normal distribution the answer would be no, we saw this before (Note 2 [here](https://nintil.com/grant-peer-review)), but here we get increasing maximum RCR, so buying more variance by spreading the funding seems to pay off.

But! If we take work in the top 1% by RCR (>59.29), we see that the median funding for those projects was almost 1M$ (More than twice the median), and so we have a situation similar to my comments on the Li & Agha paper: That high funding does not imply high impact while at the same time high impact more often than not necessitates high funding. The pattern is similar across impact categories (Upper 0.1% RCRT quantile, or 0.01% RCR quantile).

![img](https://nintil.com/images/2021-01-14-funding-lotteries/27587981-383A-4DED-ADF5-40D357747E61.png)

So rather than doing Fortin & Currie do, we could wonder as well how many top papers would result if we institute a policy where no PI gets funded with more than a given amount; First I sample 100k PI-years from each bucket. I find that a funding cap would decrease the number of papers in the top by RCR.

![img](https://nintil.com/images/2021-01-14-funding-lotteries/4D7978BA-6F5B-4AC8-96E5-84C4ECC8075C.png)

This is unfair to the cap because a cap means we can fund more grants for a given budget, so let's repeat the exercise from before (Sampling either 1 highly funded PI or N PIs with k the funding each), N=1/k but instead of seeing who got a higher RCR, seeing how many highly cited papers we get in each bucket. With k=2 (That is, we fund 50k grants at twice the median vs funding 100k at around the median), the answer is that the highly funded group produces more (22% more) papers in the top 1% by RCR.

Setting aside the particularities of the RCR as a metric and the problem of field-specificity, this illustrates that the optimal funding structure in science depends on what the metrics one wants to achieve are. If it's just average higher impact, then funding caps make sense, but if you want more top papers (Which makes sense if you assume that only the [top counts](https://nintil.com/newton-hypothesis)) then there should not be caps. An argument against this would that even with less funding you can still get top results, that giving more money to some PIs will lead them to play with fancier tools; rather than being more creative in using equipment and tools already available to achieve the same ends, they may buy the latest kit that only marginally improves on the previous one. Again it's only a question that can be settled by experiment.

Janssens et al. ([2017](https://peerj.com/preprints/3106.pdf)) note that the RCR, laudable as it may be to have a metric that normalizes citations by field and year is far from ideal:

> The calculation of the annual weighted RCR of researchers’ portfolios assumes that the RCR of a scientific article is constant over time, but this is not the case: 8 the RCR decreases when an article is past its heyday. The citation rate of an article (the numerator in the RCR equation) decreases over time when its number of citations ‘stabilizes’ and the number of years keeps increasing. The expected citation rate (the denominator in the RCR equation) is a normalized 2- year citation rate of the journals in which the ‘field’ articles were published. Using a 2-year citation rate assumes that the citation rate of an article is constant over time and always as high as in the first two years, which is unrealistic for older articles. A decreasing numerator and overestimated denominator result in reduced RCRs for most older articles. When the RCRs of scientific articles decrease over time, the annual weighted RCR is not a suitable indicator for the current or recent scientific influence of mid- and late-career researchers, as their older work pulls down their portfolio RCRs. The reduction of their annual weighted RCR might, at least in part, explain the diminishing returns that were observed in the NIH data as researchers with higher GSI scores had substantially longer histories of grant funding (median of 19 years among GSI>21 as compared to 3 years among GSI≤7)

And what is worse, NIH did not seem to have considered the idea of experimenting; rather than proposing instituting a cap they should have randomized PIs to a cap or no cap conditions and then measure outputs. That seemingly was never on the table but should have.

## Thinking rationally about funding allocation in science

Imagine a lab wants 1M$ for a study and two labs want 0.5M$ each for another study in a different field. The considerations above would lean towards funding two labs instead of one. But now suppose these one lab has a track record of producing very highly cited (And acclaimed by experts) work whereas the other two are not. Or suppose the labs are equally capable, but the field where lab A works in tends to do more expensive experiments. Should the weighting be done at the level of the lab or of the field?

Now consider that the first lab works at the forefront of a new subfield of oncology and has in the past produced basic research leading to insights upon which available drugs are based whereas the second is trying to better understand an ultra-rare disease. What would you fund?

For this second scenario there is a simpler rule of thumb: Fund research targeting each disease depending on how prevalent that disease is, weighted by how serious it is. So if disease A leads to 2x the QALY/DALY lost than disease B then the former should get twice the funding. A funder might choose to give 1M$ to one field and 0.5$ to another. But within one field the question of per-lab allocation would remain.

What the studies above on efficiency are probably getting at is the suspicion that _if we could control for field-specific costs_ then some labs are more efficient than others, and I infer that if this could be done then we could use that to tailor grants; if lab A does a scRNA-seq study in mice examining the liver and that costs C then perhaps if lab B proposes to do the same study but in the pancreas for 40% more then B shouldn't get any money, giving that instead fo researchers that pass some threshold of efficiency.

Shahar Avin (Hi Shahar!) reviewed the effectiveness of peer review and lotteries in an [2015](https://link.springer.com/chapter/10.1007/978-3-319-23015-3_9) and [2019](https://www.sciencedirect.com/science/article/pii/S0039368118300190) articles and more broadly [his PhD thesis](https://www.repository.cam.ac.uk/handle/1810/247434) considers funding of science more broadly; he notes that the costs of having a peer review system could be in the order of 14% of total funding, and that this system is far from perfect (I reviewed the evidence behind grant peer review [here](https://nintil.com/grant-peer-review)). And he also discusses a potential downside (or upside) of a lottery system: Under a pure lottery scientists wouldn't be able to rely on say 10 or 20 years of continuous funding, they won't know if they will draw a ticket in the next period, and nothing they can do can change that.

A point, so far undiscussed (I was hoping to have a post that deals exclusively with bibliometrics at some later time) that he raises is that one can look at citations and RCRs as much as one wants. But what we should care about is the social value of science: Science is too important to be left to the scientists and the way science is structured is just a means to pursuit of whatever is it that society tasks science with. Given most people values, this is to produce research that is relevant for innovation that improve people's lives, and to a far lesser extent, to broaden the frontiers of knowledge for its own sake. If we can't know what research is going to be socially beneficial, it doesn't matter what analysis we do on highly cited research, we are back to randomizing funding by default. Is funding the nth -seq (There are [so many](https://en.wikipedia.org/wiki/List_of_single_cell_omics_methods) of them now) more worthwhile than funding a cancer research paper? Even if say we expect the former will get more citations, it's far from clear that it will be the most likely way to contribute to the development of products that people will then benefit from.

## Lottery proposals

During the Fund People, not Projects series I have mentioned here and there various experiments one could do to change the way the funding of science is done. A good set of them is Ioannidis ([2011](https://www.nature.com/articles/477529a)), Gross & Bergstrom ([2019](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3000065)), Fang & Casadevall ([2016](https://mbio.asm.org/content/7/2/e00422-16)):

-   Fund everyone equally
-   Fund a set of grants at random
-   Fund a set of grants at random, asking reviewers if the grant has no major flaws
-   Fund a set of grants at random, asking reviewers if the grant is meritorious

And I can add some more of my own,

-   As above but use a bibliometrics-based model to "load the dice"; e.g. if you are an accomplished researcher you get more chances in the lottery.
-   Have open-ended running for 1,5,10, and 20 years, then use lotteries for those
-   As before, but for say the grant running for 10 years, have half of the researchers be "successful" (As proven by bibliometrics) and the other half be truly selected from a random pool subject to a minimum review.

## The politics of science funding

A reason why we may see less experiments with funding modalities than we would like is politics. Barnett ([2016](https://mbio.asm.org/content/7/4/e01369-16)) reports an answer from an Australian funding agency as to why they haven't tried lotteries: not that it wouldn't work but that _it would make it look like we don’t know what we’re doing_, the agency is worried about losing face and that seemingly gets in the way of experimentation. Beattie ([2020](https://www.nature.com/articles/d41586-020-00133-1)) in a letter to to _Nature_ complain that lotteries are just a way for an inefficient bureaucracy to try to cope with how science this work, calling for _Academia \[to\] resist this bureaucratization of research and publishing by well-meaning but scientifically inept bureaucrats_ whereas Vindin ([2020](https://www.nature.com/articles/d41586-020-00136-y)) rejects the idea of grants on the grounds that it won't favor merit or work that requires longer term funding.

Scientists in general have mixed opinions on the idea of lotteries. Philipps ([2020](https://academic.oup.com/rev/advance-article/doi/10.1093/reseval/rvaa027/6017979?login=true)) interviews 32 scientists in Germany both senior and junior and spread across the life and physical sciences, whereas they express support for the idea of peer review, they also think that lotteries should be given a chance, but perhaps not in a fully randomized way; for example one of them said that if it's truly random that may lead to researchers to write lots of lower quality grants just to have more tickets in the lottery. This highlights that lotteries could be done at the researcher level or at the project level; Lotteries work both in a "Fund people" and a "Fund projects" world.

Liu ([2020](https://researchintegrityjournal.biomedcentral.com/articles/10.1186/s41073-019-0089-z)) is another, broader survey in the same spirit as Philipps (2020) but in New Zealand, and with 126 scientists. Here, scientists seem more (63% of them) in favor of using randomization for exploratory grants but in general for other grants they are almost evenly split. Some of the specific arguments given by those who opposed lotteries don't seem that good; for example "I don't think a randomisation process is any less fair than an individual reviewer finding some minor reason for a great project not to be funded."; assuming that both processes are as random, lotteries still have the advantage that they are cheaper as they don't demand any (or less) reviewer time. There seems to be some agreement with a "messy middle" lottery funding model, where "obviously good" applications get funded, "obviously bad" ones get rejected and everything else gets funded at random.

## Critiques of the lottery system

Most research work on lotteries is favorable. I tried looking for research that explicitly criticisms the notion of using funding lotteries.

The only one I could find, other than the two comments from Beattie and Vindin is Bedessem ([2019](https://academic.oup.com/rev/article-abstract/29/2/150/5678703?redirectedFrom=fulltext)) seems to suggest that one argument for lotteries, that we (Or scientists in a field) can't know what will be good science because success occurs very randomly, is mistaken because

> it underestimates the fact that the structure of the systems of scientific practice constraints strongly (or more strongly that assumed in pro-lottery literature) the kind of project that may be considered (relatively) interesting-that is to say, useful for a (relatively) large number of scientific practices, including technical, experimental, or theoretical activities. Because of these interconnections, it seems reasonable to suppose: 1. that there are not a large number of ‘exploratory’ projects that are strongly disconnected theoretically, experimentally, and technically from existing systems of practices; and 2. that it is at least theoretically possible to obtain a more precise comparison of the value of ‘equally good’ projects if we take into account the various kinds of objectives of different kinds they pursue.

And whereas this is true, the pro-lotteries side could reply with a) Granting that most science is exploratory, at any point in time there are more promising non-exploratory projects that can be funded. There is then a question of deciding between these and b) It is indeed possible, but at what cost? Predicting success is hard!

So while he is right in the abstract, his concerns have to be weighted with the equally valid arguments the pro-lottery camp makes.

## The time factor

Assuming a lottery system, one has to decide the funding period. Relative to status quo the system would make it more difficult for projects that do need more time to succeed. [Braben](https://ideamachinespodcast.com/donald-braben) says that Planck needed 20 years working on the foundations on thermodynamics to arrive at the idea of postulating the quantization of energy. Regardless of whether that's literally true or not, it's true that it can conceivably be the case that the quality of the research can be a function of how secure a scientist is in their position.

How much time do scientists in general need? If most science, and most useful science, can be done in 5 years then a randomized R01 lottery in the context of NIH wouldn't be subject to this objection. If only a small minority are the ones that would benefit from super-long funding then the lottery could be coupled with another lottery that could offer longer term support.

## Conclusion

The evidence reviewed here supports the use of lotteries as part of a science funding ecosystem. Proponents of lotteries may have underplayed the extent to which peer review can detect impactful work, and the extent to which citations can be tied to downstream practical applications but overall their cost-benefit analysis is reasonable. I believe a system could be designed that appeals to funders and scientists such that I get buy in, but this won't be a pure lottery. Whereas obviously I have to end this post with a call for experiments testing various kinds of lotteries, if I were in charge of implementing any such system, the way I would do it would be as follows:

-   Part A "The HHMI-lite" (85% of the funding)
    -   Proposals are HHMI style, relatively short, proposing a broad research idea rather than an R01 style narrow project. They are to be sorted (perhaps by a review section) just into three buckets: Must fund, May fund, and don't fund. Must fund papers are all funded. If the funding needs exceed funding available, they are chosen at random. Reviewers are limited to a small number of "Must fund" designation for the papers they review. May fund papers are all funded at random.
    -   Projects requiring funding above some threshold need longer proposals and receive additional scrutiny
    -   Funding is for 5 years
-   Part B "The Planck club" (5% of the funding)
    -   Proposals are as in Part A, but instead reviewers are instructed to select only the best of the best applicants. Within that pool of people, a random set (As funding allows) is chosen.
    -   Funding is for 20 years, with a 10 year review (Only in case of substantial underperformance would the other 10 years not be awarded), and additional money is available for equipment.
    -   The need for longer term funding has to be justified in the proposal
    -   Can only be granted once to a given person
-   Part C "The PseudoARPA" (10% of the funding)
    -   Here a panel of permanent program managers designs research programs and then aim to execute on them, funding scientists across multiple labs, or bringing in industry expertise, etc.
    -   PMs have discretion to increase, decrease, or completely cut a given project
    -   PMs are instructed to fund opportunities that single labs won't do either due to the coordination or funding requirements needed by the project they have in mind

Additionally, prohibit the use of grant money as salary, and impose on universities as a requirement that they pay full salary to the recipient of the grant. The lottery used in this proposal wouldn't be truly random; inverse weight would be given to how much money is asked for.

_Ricón, José Luis, “Fund people, not projects IV: Scientific egalitarianism and lotteries”, Nintil (2021-01-20), available at https://nintil.com/funding-lotteries/._