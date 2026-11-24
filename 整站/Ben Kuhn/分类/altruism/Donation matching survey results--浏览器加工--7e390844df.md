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