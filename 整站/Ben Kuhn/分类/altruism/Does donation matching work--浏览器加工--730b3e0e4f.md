_Note: in April 2015, 50% of the impact of this post was [purchased](http://impactpurchase.org/2015/04/10/first-round-results/) by Paul Christiano and Katja Grace._

In the effective altruism community, [donation](https://www.facebook.com/groups/effective.altruists/permalink/798392703550422/) [matches](http://harvardea.org/fundraisers/winter2013/) are [becoming](http://www.charityscience.com/donation-match.html) [very](http://christmas.causevox.com/) [popular](http://www.goodventures.org/about-us/press/good-ventures-announces-5-million-in-matching-funds-for-givedirectly). Some matchers have gone as far as [tripling](https://www.againstmalaria.com/Fundraiser.aspx?FundraiserID=7021) or even [quadrupling](https://www.againstmalaria.com/Fundraiser.aspx?FundraiserID=7019) each dollar donated, not just doubling. But I started to wonder if the matching multiple—or even matching at all—has any impact on the money you raise. In this post, I’ll take a look at some of the academic literature on donation matching to see whether such matches are justified.

_Thanks to Anders Huitfeldt and Elizabeth Santorella for looking over this review before publication. Any errors are entirely my own._

_Note: this got fairly long. If you trust my analysis (which you shouldn’t!) and want the TL;DR, you can jump straight to the [conclusion](https://www.benkuhn.net/matching/#conclusion)._

Contents

-   [Background and terminology](https://www.benkuhn.net/matching/#background-and-terminology)
-   [Research methodology](https://www.benkuhn.net/matching/#research-methodology)
    -   [Literature search](https://www.benkuhn.net/matching/#literature-search)
    -   [Analysis](https://www.benkuhn.net/matching/#analysis)
-   [Priors](https://www.benkuhn.net/matching/#priors)
    -   [Does matching amount matter?](https://www.benkuhn.net/matching/#does-matching-amount-matter)
    -   [Does matching help at all?](https://www.benkuhn.net/matching/#does-matching-help-at-all)
-   [Results](https://www.benkuhn.net/matching/#results)
    -   [Does matching help?](https://www.benkuhn.net/matching/#does-matching-help)
    -   [Does level of matching matter?](https://www.benkuhn.net/matching/#does-level-of-matching-matter)
-   [Conclusion](https://www.benkuhn.net/matching/#conclusion)
-   [Appendix: Overview of individual studies](https://www.benkuhn.net/matching/#appendix-overview-of-individual-studies)
    -   [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#eckel-and-grossman-2007)
    -   [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#eckel-and-grossman-2008)
    -   [Karlan and List 2007](https://www.benkuhn.net/matching/#karlan-and-list-2007)
    -   [Karlan et al. 2011](https://www.benkuhn.net/matching/#karlan-et-al-2011)
    -   [Martin and Randal 2009](https://www.benkuhn.net/matching/#martin-and-randal-2009)
    -   [Meier 2006](https://www.benkuhn.net/matching/#meier-2006)
    -   [Rondeau and List 2008](https://www.benkuhn.net/matching/#rondeau-and-list-2008)
-   [Sources](https://www.benkuhn.net/matching/#sources)
-   [Footnotes](https://www.benkuhn.net/matching/#footnotes)

* * *

## Background and terminology

Like most research questions, it’s not totally clear what we actually want to measure.

The thing we ultimately care about is probably **average revenue,** the average amount of money you receive (not counting the match) per person you ask for donations. For instance, if you send a letter to 100 people, 10 of them donate $10 each, and 90 of them don’t respond, then your average revenue is $1.

Average revenue is the most important number for a given campaign, because it’s directly related to the amount of money you raise. However, as the discussion above suggests, it’s sometimes helpful to decompose average revenue as follows:

`$$\textrm{average revenue} = \textrm{probability of donating} \times \textrm{average size of a donation}$$`

We’ll call the terms on the right-hand side **propensity to donate** and **average gift size.**

Since different campaigns have different base rates (without the match) for propensity and gift size, I prefer to measure _relative_ rather than absolute differences in propensity and gift size, to get a more apples-to-apples comparison.[1](https://www.benkuhn.net/matching/#fn:1)

For propensity, I will sometimes report the [**relative risk**](http://en.wikipedia.org/wiki/Relative_risk)—that is, the probability of donating with the match, divided by the probability of donating without the match. For gift size, I will report the **gift size ratio**, which is what it sounds like. In either case, I will report a point estimate followed by a two-standard-error interval in parentheses, as in 1.23 (1.07-1.39).

## Research methodology

### Literature search

Studies on donation matching fall into two broad categories: _lab studies_ and _field studies_. Here I focus on field studies, because they tend to have larger samples, be less susceptible to publication bias, and have more realistic conditions. This can be important: for instance, “matching” donations by rebating half of each donor’s gift seems to be more effective in the lab (where the rebate is instantaneous) than in the field (where it takes longer and is more of a hassle).

I didn’t do a formal meta-analysis-style literature search, because I wanted to actually finish this post. However, I did search Google Scholar for the queries `nonprofit donation matching`, `donation matching -kidney`, and variants, and looked through the first three pages of results for randomized field studies of donation matching that had a freely-available PDF somewhere. After this I felt I was hitting diminishing returns on the literature search, so I stopped. (If you know of any additional studies that look at this phenomenon, please [send me references](https://www.benkuhn.net/contact/) and I’ll update this post!)

### Analysis

One issue with donation matching is that the baseline propensity to donate with no match is very low (typically on the order of 1%). This means that studies need to have very large sample sizes (tens of thousands) in order to have tight confidence intervals around their estimated effect size, and some of the field studies ended up with very low power.[2](https://www.benkuhn.net/matching/#fn:2)

Another issue is that different fundraising campaigns are highly heterogeneous, that is, they have very different baseline propensities to donate and gift sizes. As a result, it’s most informative to look at relative effects of a matching challenge, like the relative risk for propensity to donate, or the percentage difference in gift size.

Unfortunately, most of the papers in my sample did not discuss their statistical power, or report their results in a way that made it easy to investigate. As a result, I had to back out many of these measures myself from tables in the papers. To calculate the relative risk and confidence interval, I used [this spreadsheet](https://docs.google.com/spreadsheets/d/1mpoC3hgClGt5-0WyeJ37Ux4SbbJSfRsvWyxo_YAgZsA/edit?usp=sharing).

## Priors

For transparency, I should state what I expected to find going into this study. Obviously, I tried not to let it bias my interpretation of the evidence, but I can’t make any promises. I wrote most of this section before conducting the review, but accidentally deleted the first section during editing and had to rewrite it. I also edited the second section for style after conducting the review.

### Does matching amount matter?

A naive economic model would predict that yes, the matching amount matters: a donation is essentially purchasing a good, and matching decreases the price of the good, so it should change demand and hence total consumption.[3](https://www.benkuhn.net/matching/#fn:3)

However, I think this is unlikely to be true for a couple reasons.

-   People suffer from [scope neglect](http://en.wikipedia.org/wiki/Scope_neglect) in donations, and so don’t really think about what the “price” of a donation is per se.
    
-   Donations are sometimes used as costly signals, in which case it’s how much you pay that matters, not how much you buy.
    

As a result, I expect not to see very large effects from changing the matching ratio.

### Does matching help at all?

Despite the above, I do expect to see effects from the _existence_ of a match, because of [social proof](http://en.wikipedia.org/wiki/Social_proof) and urgency effects. I expect this mostly to manifest in larger numbers of donations, rather than larger amounts per person.

However, I expect a lot of heterogeneity in results/effect sizes, because it matters a lot how the match is presented. Properly used, you can probably get a lot of leverage from a match, but it’s also possible to mess up the presentation and have it not be that influential.

## Results

I found seven studies of field experiments on donation matching. Unfortunately, many of them had significant flaws, either in design, implementation or analysis. Three ([Meier 2006](https://www.benkuhn.net/matching/#source-m06), [Karlan et al. 2011](https://www.benkuhn.net/matching/#source-kls11), and [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08)) had potential randomization issues. Two ([Meier 2006](https://www.benkuhn.net/matching/#source-m06) and [Karlan et al. 2011](https://www.benkuhn.net/matching/#source-kls11)) raised concerns of specification searching. One ([Rondeau and List 2008](https://www.benkuhn.net/matching/#source-rl08)) was seriously underpowered. Two ([Meier 2006](https://www.benkuhn.net/matching/#source-m06) and [Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09)) suffered from unusual designs that made the results hard to compare to other studies. Only two trial ([Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) and [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07)) did not raise significant concerns.

The table below summarizes general notes on each study. I go into more detail on individual studies after this section. “Weight” indicates how important I consider that study in drawing general conclusions (higher is better). For complete justification of these weights, check the appendix below.

| Study | Setting | N | Matches | Weight |
| --- | --- | --- | --- | --- |
| [Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09) | Art museum donation box | 72 clusters | 100% | 3 |
| [Meier 2006](https://www.benkuhn.net/matching/#source-m06) | Time series of student donations | 1,000 | 25%, 50% | 2 |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) | Direct mail | 50,000 | 100%-300% | 4 |
| [Karlan et al. 2011](https://www.benkuhn.net/matching/#source-kls11) | Direct mail | 20,000 | 25%, 100% | 1 |
| [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) | Direct mail | 15,000 | 25%, 33% | 4 |
| [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) | Direct mail | 400,000 | 25%, 33% | 2 |
| [Rondeau and List 2008](https://www.benkuhn.net/matching/#source-rl08) | Direct mail | 1,500[4](https://www.benkuhn.net/matching/#fn:4) | 100% | 2 |

### Does matching help?

There seems to be **moderate evidence** that the existence of a donation match has an effect, but **that effect is often lower than one would naively expect**.

People’s general impression of matching seems to be that it about doubles non-match revenue. For instance, Charity Science wrote in [their recap of a matching campaign](http://www.charityscience.com/operations-details/the-power-of-donation-matching):

> Overall the match was extremely valuable. We feel that about half of all the donations made were significantly affected by the match. Many people actively commented that they were donating because of it. Because of this we will likely aim to have a match running for every major event in the future.

(I don’t mean to pick on Charity Science here! I quote them only because they stuck their necks out the farthest by naming a specific percentage. I think this view is not uncommon. I probably would have guessed something similar before reading the literature, although unfortunately I didn’t commit myself to an effect size when writing down my priors. Lesson learned!)

By contrast, the following table summarizes the effects I found. Unless otherwise noted, I pooled all match levels and all non-matching treatments (the differences between them were generally small).

| Study | Baseline propensity | Propensity ratio | Baseline gift size | Gift size ratio |
| --- | --- | --- | --- | --- |
| [Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09) | 1.9% | 1.07 (no CI available) | $2.11 | 1.2 (no CI available) |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) | 1.8% | 1.22 (1.07-1.39) | $46 | 0.96 (0.84-1.09) |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), blue states | 2% | 1.05 (0.89-1.24) | $45 | 0.95 (0.8-1.1) |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), red states | 1.5% | 1.53 (1.22-1.93) | $47 | 0.97 (0.76-1.17) |
| [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) | 3.7% | 0.99 (0.83-1.18) | $51 | 1.42 (1.32-1.52) |
| [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) | 0.5% | 1.05 (0.92-1.2) | $47 | 1 (1-1.01) |
| [Rondeau and List 2008](https://www.benkuhn.net/matching/#source-rl08)[5](https://www.benkuhn.net/matching/#fn:5) | 4.6% | 1.05 (0.66-1.68) | $28 | 1.23 (0.9-1.56) |

The three studies I give the most weight—[Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09), [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), and [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07)—all found significant effects of matching on revenue, but more like 20-50% than 100%. The latter two studies had enough power to strongly rule out a 100% increase.

Interestingly, though, they found it through opposite avenues. Martin and Randal and [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) found no effect on propensity to donate, but a 20-50% effect on gift size; the confidence interval for the latter was 1.42 (1.32-1.52).[6](https://www.benkuhn.net/matching/#fn:6) Meanwhile, [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) found no effect on gift size (estimated gift size ratio 1.04 (0.97-1.11), ruling out an effect size of 1.2 with something like 99% confidence).

On the other hand [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) found a donation risk ratio effect of 1.22 (1.07-1.39). The confidence intervals for [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) included the low end of this range.

Of course, some and potentially all of this is explained by heterogeneity. Martin and Randal’s experiment used a donation box in an art museum, whereas [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) tested a direct mail campaign for a liberal nonprofit. Notably, even within the [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), the benefit from matching was entirely absent in blue states (where Kerry won in 2004; risk ratio 1.05 (0.89-1.24)) but a very large effect in red states (risk ratio 1.53 (1.22-1.93)).

Of the shakier studies, one other piece of evidence stands out: [Meier 2006](https://www.benkuhn.net/matching/#source-m06) found that donation matching increased donations in the short run, but _decreased_ donations in the long run, so that the net effect of the match on money moved was negative. This is a little bit worrying, but [Meier 2006](https://www.benkuhn.net/matching/#source-m06) had a number of statistical issues and the matching scheme was unusual (see below for details), so I don’t put too much weight on this finding. I think it’s interesting and would love to see further research, but for now I’m not too worried that donation matching is net negative.

Two other direct mail studies found lower or no effects of matching: [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) and [Karlan et al. 2011](https://www.benkuhn.net/matching/#source-kls11). However, both of these studies had potential randomization issues (quite severe in the latter case) and the former tested only small matches, on the order of 30%. Additionally, in the [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08), many donors’ donations were exactly equal to the organization’s membership fee, which was unaffected by the match, suggesting that donation amounts were largely influenced by personal benefits, rather than the good done by the donation. As a result I have serious doubts about how well these results would generalize.

### Does level of matching matter?

The evidence here is much more ambiguous. The following table summarizes the findings of the four relatively sound studies that analyzed this (I exclude [Meier 2006](https://www.benkuhn.net/matching/#source-m06) because of my concerns about design and analysis). In each case, “baseline” refers to the lowest nonzero match group, and I compare to the highest (i.e., 300% vs 100% for [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) and 33% vs 25% for Eckel and Grossman’s studies).

| Study | Baseline propensity | Propensity ratio | Baseline gift size | Gift size ratio |
| --- | --- | --- | --- | --- |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), all states | 2.1% | 1.1 (0.92-1.31) | $45 | 0.92 (0.68-1.15) |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), blue states | 2.1% | 1 (0.79-1.27) | $43 | 0.93 (0.73-1.12) |
| [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07), red states | 2.1% | 1.24 (0.94-1.63) | $48 | 0.91 (0.64-1.17) |
| [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) | 3.4% | 1.17 (0.95-1.45) | $75 | 0.93 (0.86-0.99) |
| [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) | 0.6% | 0.76 (0.61-0.96) | $50 | 0.9 (0.89-0.9) |

The most confident inference I can draw from these results is that there’s **strong evidence of heterogeneity**, caused in part by the different match levels, different organizations and different subject groups. Only [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) tested reasonably large differences in matching schemes, so I place the most weight on their findings (and am surprised that [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) observed such large effects).

There is **little evidence on response rates in either direction**. While [Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) and [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) found higher point estimates (the latter only in red states), both intervals included no effect; meanwhile, [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) found a significant _negative_ effect on response rates, but were using such a small difference in matching that I doubt it’s generalizable. (They had other unexplained issues with response rates as well; see below.)

One thing that stands out is that **no study found an increase in average gift amount** and both of Eckel and Grossman’s studies found a significant decrease. Because of the issues with membership fees that I mentioned before, I don’t place that much weight on this finding, but I think it’s notable nonetheless. This suggests that, to the extent the economic explanation for giving under matches is correct (that is, the influence of the match comes because it changes the price of giving), the price elasticity of donations is less than one—that is, increasing matching is likely to displace more giving than it creates.

## Conclusion

I don’t think the evidence here is strong enough either to categorically rule out or categorically support donation matching schemes as a use of money. But I do think we can draw some conclusions from it.

-   **The effect of donation matching is relatively small,** on the order of 20% (with high uncertainty). No studies observed effects greater than 50% in any subgroup they investigated. On the other hand, the methodologically sound studies do largely find effects of matching.
    
-   **There are probably diminishing returns to matching above 1:1.** The evidence for increased response rates is weak, and the increases are small if they exist. There’s (similarly weak) evidence for _decreased_ average gift size.[7](https://www.benkuhn.net/matching/#fn:7)
    

-   **Donation matching is not guaranteed to be positive.** No studies found strong, generalizable negative effects of donation matching. However, several found worrying hints: [Meier 2006](https://www.benkuhn.net/matching/#source-m06) found that may have decreased net long-run donations, [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) found that it decreased response rate among some donors (probably due to the study’s unfamiliar-looking fundraising appeal), and there’s weak evidence that higher matching levels can actually crowd out some donations.
    
-   **The effect of donation matching is likely swamped by other sources of variation.** Before you start donation matching, make sure you’re raising money from the right audience and have optimized your fundraising appeals, because both of these made more of a difference than the match in several studies. In fact, the director of marketing in [Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) said as much:
    
    > From conversations with Mr. Al Anderson, Director of Membership Marketing, an earlier change that was designed to make rejoining easier (merely adding a 1-800 number to the pledge form) resulted instead in a significant drop in their response rate of about the magnitude we observed for the treatment groups. Mr. Anderson’s hypothesis about the impact of the 800 number is that potential donors set the pledge card aside intending to call, then did not.
    
    [Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09) similarly found that, for their museum donation box, whether the donation was requested on Sunday made much more difference than matching, with a 50% increase in donation size.
    
-   **The secondary effects of donation matching are ambiguous and poorly studied.** [Meier 2006](https://www.benkuhn.net/matching/#source-m06) found that the net effect of their donation match on revenue was marginally significantly _negative_. We would also expect that part of the effect of matching fundraisers is to shift gifts away from unmatched fundraisers, but I couldn’t find any field studies of this effect.
    
-   **There may be other better ways for large donors to leverage their money.** Here I studied matching, but there are other ways of using large donations in fundraising. For instance, a lab study [suggests](http://www.sciencemag.org/content/346/6209/632.short) that using the large donation to “cover the charity’s overhead” (so that one can claim, e.g., “100% of your donation goes directly to people in need”) is more effective than offering a match. Evidence from [Rondeau and List 2008](https://www.benkuhn.net/matching/#source-rl08) weakly suggested that announcing the big donation as seed money had a larger effect than matching, as did simply asking for more money (although the results were not significant because their power was very low).
    
-   **If you’re matching, study your own matching scheme!** There was a huge amount of heterogeneity in these studies, and the effects of donation matching varied widely as a result. In particular, most of the studies were on relatively impersonal direct mail campaigns, but many matches “in the wild” are spread by word-of-mouth. To the extent to which matches are driven by social proof, we would expect changing the social context to change people’s response to the match (although it’s unclear in what direction). Anyway, the upshot is that information about how your own donor base would react to a matching offer is still really valuable.
    

All things considered, I would recommend **looking into other ways of using large donations to raise funds** (although there’s not enough evidence to compare them right now). I would also recommend **not matching donations at a rate greater than 1:1**, instead using the additional funds to run more 1:1 matching campaigns (at minimum) or run experiments on different fundraising strategies (ideally).

(Also, I would recommend writing up the results of any experiments you run! Evidence is thin on the ground here, so every bit helps!)

## Appendix: Overview of individual studies

### Eckel and Grossman 2007

[Eckel and Grossman 2007](https://www.benkuhn.net/matching/#source-eg07) studied a direct mail campaign to 15,000 prospective donors to Lutheran Social Service of Minnesota. They offered a match of either 25% or 33% (or an equivalent rebate, or a control offer). I did not analyze the rebate arms of the study.

The data showed no significant difference between response rates of either matching arm or the control, or between the matching arms. The risk ratio of donation given any match (pooling the matching treatments) was 0.99 (0.83-1.18). The data suggested a possible (non-significant) increase in response rate for the 33% match over the 25% match; the risk ratio between the two was 1.17 (0.95-1.45).

The data showed a strong difference in gift size between each arm and the control; the estimated effect when combining the matching arms was 1.42 (1.32-1.52). Interestingly, there was a slight but significant _decrease_ in donation size from the 33% arm compared to the 25% arm; the estimated effect was 0.93 (0.86-0.99).

I didn’t see any major issues with the design, implementation or analysis of the study. One possible source of heterogeneity is the fact that the charity was religious (in light of the heterogeneity reported by [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07)). The authors found a _negative_ relationship between religious attendance and likelihood of donation,[8](https://www.benkuhn.net/matching/#fn:8) but did not investigate its interaction with the matching offer.

### Eckel and Grossman 2008

[Eckel and Grossman 2008](https://www.benkuhn.net/matching/#source-eg08) offered a match of either 25% or 33% (or an equivalent rebate, or a control offer) to about 400,000 potential donors of Minnesota Public Radio. I did not analyze the rebate arms of this study.

Unfortunately, this study also showed randomization issues. Current donors to MPR were not equally allocated between the treatment and control arms. Furthermore, these donors showed a significant _decrease_ in response rate in all treatment arms. The authors hypothsized:

> \[T\]here are two factors that may have contributed. First, the treatment group received a mailing that was different from what they were accustomed to seeing. From conversations with Mr. Al Anderson, Director of Membership Marketing, an earlier change that was designed to make rejoining easier (merely adding a 1-800 number to the pledge form) resulted instead in a significant drop in their response rate of about the magnitude we observed for the treatment groups. Mr. Anderson’s hypothesis about the impact of the 800 number is that potential donors set the pledge card aside intending to call, then did not. In our case, because of their loyal support of the organization, a substantial portion of the Continuing target group may have felt an obligation to consider the subsidy carefully as well as to complete the online survey, and may have set the mailing aside, fully intending to complete it at a later time. This is in contrast to the other two groups (Lapsed Members and Prospects), who exhibit no parallel difference in response rates for the treatment v. control groups, and who likely have no parallel ’loyalty’ impulse.

Whatever the reason, it seems likely that findings in this study from continuing donors would not generalize, so I restricted my analysis to individuals who had never before donated to MPR (the largest group in the sample, about 300,000 subjects). Unfortunately, this group was subject to another confounding factor, which is that MPR required a donation of $42 (regardless of matching level) to qualify for membership. About 75% of new donors donated exactly $42, so this suggests that donors may have been primarily motivated by the benefits of membership, which were not subject to the match.

Comparing the combined treatment arms to the control group of no match, the propensity risk ratio was 1.05 (0.92-1.2). The gift size ratio was 1 (1-1.01), mostly due to the large number of $42 donations. Puzzlingly, I found that the 33% match had a significantly _lower_ response rate and gift size than the 33% match (risk ratio 0.76 (0.61-0.96)). I’m concerned that this may be due to poor copy writing or something, though.

### Karlan and List 2007

[Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) mailed 50,000 previous donors to a liberal political nonprofit with a matching offer of 0, 100%, 200%, or 300%. They found fairly large and significant effects of matching on propensity and revenue. The propensity risk ratio for all treatment groups combined was 1.22 (1.07-1.39).

Breaking down by matching level, Karlan and List found no significant relationship between match amount and either propensity, revenue or gift size. Unfortunately, they didn’t report confidence intervals on effect sizes for dollars donated, or test for a dose-response relationship. For donation size, the difference between matching levels is no more than 10% in any case, and there is no trend in the means. So it look like this isn’t just an issue of low statistical power. The propensity risk ratio between the 300% and 100% match groups was 1.1 (0.92-1.31), which limits the plausible effect sizes there.

Interestingly, Karlan and List found that the treatment effect only existed in red states. (This is relevant since they were testing donations to a political organization. The control group had a significantly lower response rate in red states—1.5% versus 2%—while the treatment group had about equal response rates of 2.1% vs 2.3%.) I’m not sure what the best explanation for this is; the authors don’t really advance any. It does suggest that response to matching could be more situation-dependent than one would expect. When restricting to red states, the risk ratio of a 300% match compared to a 100% match was 1.24 (0.94-1.63)—still not significant, but this doesn’t rule out a reasonably large effect. On the other hand, for blue states that risk ratio was 1.00 (0.79-1.27).[9](https://www.benkuhn.net/matching/#fn:9)

### Karlan et al. 2011

[Karlan et al. 2011](https://www.benkuhn.net/matching/#source-kls11) compared 33% and 100% matches to a control group of no match for a sample of 20,000 previous donors to a “liberal organization that… focuses on civil justice issues.”

They found overall no effect of either match compared to the control, on either propensity or gift size. Breaking the donors down into recent vs. non-recent donors (with a cutoff of 10 months), they found that the match made recent donors more likely to donate, and non-recent donors less likely to donate, with the effect being driven by the 33% match. The authors also reported several effects based on interactions between multiple factors:

> \[W\]e find that when the example matching amount is $25 instead of $1 (i.e., “For example, if you give $75, the matching donor will give $25” versus “For example, if you give $3, the matching donor will give $1.), then the ratio of the match does matter, in particular for those who have not given before. In this case, the larger example amount actually causes harm, whereas the aggregate effect is nil.

I’m mildly skeptical of the various heterogeneity findings, since it’s possible that the authors tested other heterogeneity hypotheses but didn’t report the insignificant ones. This makes it likely that `$p < 0.05$` is not a sufficiently strict threshold to limit false positives. Furthermore, they found a stronger effect (`$p < 0.01$`) on total donations _after_ the match was supposedly over (although this effect disappeared when predicting log-donations instead of donations, which is otherwise a more reasonable specification). This raises some concerns that something went wrong with their mailing or something, which the authors don’t really investigate.

The study’s randomization was also _completely bonkers_, with 5 of their 8 baseline variables significantly correlated with the “random” group assignment. The authors don’t discuss what caused this abject failure of their randomization scheme, which raises additional red flags. Ultimately, because of these statistical issues, I give this study limited weight, and mostly consider only the main finding.

Because of the randomization issues, the authors had to control for all of these factors in their subsequent analysis, which meant I wasn’t able to extract confidence intervals on the propensity or gift size effects.

### Martin and Randal 2009

[Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09) was a cluster-randomized trial of donations in an art museum. They left a donation box seeded with either $50 or $200 in the museum, with a sign saying “thank you” or informing visitors their donation would be matched (or no sign as a control). Unfortunately, they didn’t report confidence intervals, only p-values and sometimes point estimates. They found no significant effect on donation propensity, but highly significant effects on average donation size and average revenue per visitor (they didn’t give point estimates for the relative increase of these two, but it looks like it would be on the order of 0.1 to 0.3). Unfortunately, due to the cluster-randomized study design, I was unable to extract confidence intervals on either donation risk ratio or gift size ratio.

### Meier 2006

[Meier 2006](https://www.benkuhn.net/matching/#source-m06) studied students at the University of Zurich, who have the option to contribute to two funds every semester while paying tuition. The students were offered no match or a match of 25% or 50%.

The experiment was unique in that the students had the chance to donate every semester, so the authors were able to look at the long-run effects of the match. They found that treatment increased short-run contributions, but decreased long-term contributions—apparently, students decreased their donation after the match ended to levels lower than before it began. Breaking down by match level, they found that the 25% match was not effective, but the 50% match was.

This match had a slightly weird structure: students were asked to donate to two different funds (each one by checking off a checkbox on their tuition form), but the match only applied if they donated to both. Because this ended up discretizing the problem weirdly, I’m not sure how well the study will generalize (for instance, if there had been no maximum contribution, or if going from $0 to $4 had allowed you to take advantage of the match, students might have increased their donations more).

Additionally, contribution rates before the match were already high (almost 70%) at baseline, making the likely effect smaller than a typical field study. For instance, it would have been literally impossible for Meier to observe a relative increase as large as the one [Karlan and List 2007](https://www.benkuhn.net/matching/#source-kl07) observed in red states.

Another caveat is that even though the study was randomized, it appears to have been confounded by mismatched contribution levels between the study groups at baseline. Thus the authors resorted to a difference-in-differences analysis of time series, rather than a naive model. Although in theory this should be fine (and in fact lower-variance than a naive model), it raises concerns about specification search. Meier reported some results from the naive model and some results from the differences-in-differences model, but none of their results appeared to be robust between the two.

Surprisingly, Meier claimed to find that in the long run the match had a net _negative_ effect on average donation per student. However, this result was only significant in the (confounded) naive model; it was just marginally significant (`$p < 0.1$`) in the differences-in-differences model. So we should interpret the result cautiously.

In the end, because of various issues with a strange matching structure and possibly poor randomization/specification searching, I place almost no weight on this study.

### Rondeau and List 2008

[Rondeau and List 2008](https://www.benkuhn.net/matching/#source-rl08) studied a direct mail campaign to 3,000 supporters of the British Columbia Sierra Club. The study’s power was too low to be informative, unfortunately: it couldn’t have distinguished a 30% increase in donations from noise.

The study had four experimental groups, two control and two intervention (a match and a “challenge”). In the “low control group” they asked for the same amount as in the intervention groups ($2500), which meant they were asking for _half_ the total amount they claimed they wanted in the other groups (since the matching/challenging donor was providing half the funds); in the “high control group” they asked for twice as much (so the entire amount that they told the other groups they wanted to raise).

In order to disentangle the effect of matching from the effect of the larger “ask,” I compared the matching group to the “low control” group. Here the propensity risk ratio was 1.05 (0.66-1.68), and the gift size ratio was 1.23 (0.9-1.56).

Interestingly, the study found that both the challenge gift _and the “high control” group_ had significantly higher gift sizes than the “low control” group. (The revenue difference between high and low control was not significant, because non-significantly fewer people donated in the low control group; nor was either group significantly different from the matching group.) This suggests that there may be less costly ways to increase donations than matching, but heterogeneity and wide confidence intervals mean that we shouldn’t put too much weight on that result.

Note that this study involved multiple comparisons (6 different tests for differences in pairwise means), and hence _p_\-values should be taken with a grain of salt. Furthermore, and more egregiously, my version of the paper described the results of a Student’s _t_ test as “Probability that the Mean of Individual Contributions in the \[Two\] Treatments are Equal,” giving me _extremely serious doubts_ about their statistical acumen. As far as I can tell they didn’t make any actual mistakes in their analysis, but this kind of thing makes me pretty nervous that I missed something.

## Sources

**Karlan, Dean and John A. List.** 2007. “Does Price Matter in Charitable Giving? Evidence from a Large-Scale Natural Field Experiment.” _American Economic Review_, 97(5), 1774-1793. \[[PDF](https://s3.amazonaws.com/bknet/matching/karlan2007.pdf), [NBER](http://www.nber.org/papers/w12338), [notes](https://www.benkuhn.net/matching/#karlan-and-list-2007)\]

**Eckel, Catherine C. and Philip J. Grossman.** 2008. “Subsidizing charitable contributions: a natural field experiment comparing matching and rebate subsidies.” _Experimental Economics_, 11(3), 234-252. \[[PDF](https://s3.amazonaws.com/bknet/matching/eckel2008.pdf), [SSRN](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=1843396), [notes](https://www.benkuhn.net/matching/#eckel-and-grossman-2008)\]

**Eckel, Catherine C. and Philip J. Grossman.** 2007. “Encouraging Giving: Subsidies in the Field.” Working paper. \[[PDF](https://s3.amazonaws.com/bknet/matching/eckel2007.pdf), [SSRN](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=1883772), [notes](https://www.benkuhn.net/matching/#eckel-and-grossman-2007)\]

**Rondeau, Daniel and John A. List.** 2008. “Matching and challenge gifts to charity: evidence from laboratory and natural field experiments.” _Experimental Economics_, 11(3), 253-267. \[[PDF](https://s3.amazonaws.com/bknet/matching/rondeau2008.pdf), [NBER](http://www.nber.org/papers/w13728), [notes](https://www.benkuhn.net/matching/#rondeau-and-list-2008)\]

**Meier, Stephan.** 2006. “Do Subsidies Increase Charitable Giving in the Long Run? Matching Donations in a Field Experiment.” FRB of Boston Working Paper #06-18. \[[PDF](https://s3.amazonaws.com/bknet/matching/meier2007.pdf), [SSRN](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=960959), [notes](https://www.benkuhn.net/matching/#meier-2006)\]

**Martin, Richard and John Randal.** 2009. “How Sunday, price, and social norms influence donation behaviour.” _The Journal of Socio-Economics_, 38(5), 722-727. \[[PDF](https://s3.amazonaws.com/bknet/matching/martin2009.pdf), [notes](https://www.benkuhn.net/matching/#martin-and-randal-2009)\]

_Karlan, Dean, John A. List, and Eldar Shafir._\* 2011. “Small Matches and Charitable Giving: Evidence from a Natural Field Experiment.” _Journal of Public Economics_, 95(5-6), 344-350. \[[PDF](https://s3.amazonaws.com/bknet/matching/karlan2011.pdf), [notes](https://www.benkuhn.net/matching/#karlan-et-al-2011)\]

## Footnotes

* * *

1.  Of course, ratios are not completely apples-to-apples; it’s easier to convince someone to double a gift of $1 than a gift of $100. So I also report base rates and try to determine whether lower base rates are associated with higher effect sizes. [↩︎](https://www.benkuhn.net/matching/#fnref:1)
    
2.  Although _absolute_ confidence intervals are smaller for very low (or very high) base rates, _relative_ confidence intervals are larger, so for a fixed relative risk, low base rates make differences harder to detect. For instance, it’s a lot easier to detect a risk ratio of 2 if the base rate is 25% than if the base rate is 0.1%. For the first one you can easily see the difference with 20 subjects in each arm, but for the second one you need 600 subjects in each arm to have even odds of getting even one positive case. [↩︎](https://www.benkuhn.net/matching/#fnref:2)
    
3.  One can produce models where this isn’t true—if the price elasticity of donations lines up just right with the consumer’s utility function, then donations getting cheaper can be exactly offset by diminishing marginal returns to donations, such that if the price of a donation falls by (say) 50% then the consumer purchases exactly twice as many donations, and net spending stays exactly the same. However, this would be a surprising coincidence. [↩︎](https://www.benkuhn.net/matching/#fnref:3)
    
4.  The study’s full sample was 3,000, but I only looked at two of their four subgroups; see the study section below for more details. [↩︎](https://www.benkuhn.net/matching/#fnref:4)
    
5.  This compares only the matching arm to the “low control” arm; see study notes for details. [↩︎](https://www.benkuhn.net/matching/#fnref:5)
    
6.  I couldn’t calculate a confidence interval for [Martin and Randal 2009](https://www.benkuhn.net/matching/#source-mr09) because the trial was cluster-randomized and did not report confidence intervals (only _p_\-values). [↩︎](https://www.benkuhn.net/matching/#fnref:6)
    
7.  Note that this doesn’t necessarily mean that each individual donor’s contribution was decreased by the match. Instead, it could be the case that a 2:1 match has no effect on gift size, but causes some additional people to donate at a lower gift size who wouldn’t have donated at all in a 1:1 match. [↩︎](https://www.benkuhn.net/matching/#fnref:7)
    
8.  The authors found a positive relationship between attendance and reported annual giving, though, suggesting that donations to Lutheran Social Services were crowded out by donations to the church. [↩︎](https://www.benkuhn.net/matching/#fnref:8)
    
9.  It’s exactly equal to 1 because I’m backing out the donation counts from reported response rates, which were exactly equal between the two groups. [↩︎](https://www.benkuhn.net/matching/#fnref:9)