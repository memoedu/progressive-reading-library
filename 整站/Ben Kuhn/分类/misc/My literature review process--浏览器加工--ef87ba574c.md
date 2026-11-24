After I published my [review of the literature on donation matching](https://www.benkuhn.net/matching), some people asked me, basically, how I did it.

Being able to do a thorough review of the academic literature is a pretty useful skill, and there are lots of topics I’d like to see a review of. So I’ve written up some notes about my process in the hope that they inspire you to do some literature reviewing of your own.

Are there parts you’re still wondering about? Crucial steps I missed? Things you think could improve? Let me know in the comments!

Contents

-   [Set up your system](https://www.benkuhn.net/litreview/#set-up-your-system)
-   [Search for papers](https://www.benkuhn.net/litreview/#search-for-papers)
-   [Interpret each study](https://www.benkuhn.net/litreview/#interpret-each-study)
-   [Evaluate reliability](https://www.benkuhn.net/litreview/#evaluate-reliability)
-   [Synthesize](https://www.benkuhn.net/litreview/#synthesize)
-   [Examples of literature reviews](https://www.benkuhn.net/litreview/#examples-of-literature-reviews)
-   [Conclusion](https://www.benkuhn.net/litreview/#conclusion)

* * *

## Set up your system

It’s important to maintain your set of papers in some sort of organized format, so that you can easily keep track of the big picture, and quickly add new papers to your analysis. For instance, I kept a [spreadsheet](https://docs.google.com/spreadsheets/d/1mpoC3hgClGt5-0WyeJ37Ux4SbbJSfRsvWyxo_YAgZsA/edit?usp=sharing) of all the papers I found on donation matching so that I could just copy-paste some cells to run calculations on new papers.

In addition to a spreadsheet, I also kept PDFs and notes on the papers in Evernote. When I published the analysis, I hosted my own copy of all the papers I relied on, and linked to those in addition to external versions (see the [bibliography](http://www.benkuhn.net/matching#sources)). I strongly recommend this, since links to papers on other sites may break without your knowledge and make your analysis hard to verify or reproduce.[1](https://www.benkuhn.net/litreview/#fn:1)

If you know a programming language like R or Python and you want to create plots, it might be worth writing the entire analysis using something like [knitr](http://yihui.name/knitr/). Knitr makes it easy to interweave code output (from R, Python or numerous other languages) with rich text in Markdown or LaTeX format, so you have a single file that stores and explains the entire analysis and can be updated automatically when your code changes. I haven’t written a full lit review in them yet, but I probably will for my next one. For my donation-matching survey analysis, I used a [separate Python script](https://github.com/benkuhn/matching-survey) that produced a bunch of images as output.

Finally, if your literature review involves code or calculations that you perform yourself, you should make these available for review as well. If you’re willing to learn a new program, I’d recommend using Git and putting everything up open-source on [Github](https://github.com/). Git and Github are great general-purpose tools for managing different versions and updates to files: it’s easy to make changes, incorporate other people’s suggestions, and see how the analysis has changed over time.[2](https://www.benkuhn.net/litreview/#fn:2) This makes your analysis much more transparent.

## Search for papers

In formal, peer-reviewed meta-analyses, people will pre-specify a set of keywords, search on those keywords in curated databases, read the abstract of every result and decide whether to include it based on that.

Don’t do that. It’s incredibly time-consuming.[3](https://www.benkuhn.net/litreview/#fn:3)

I generally take a less systematic approach. I’ll look through the first few pages of Google Scholar for a bunch of relevant searches, taking note of any papers that look relevant, until I think I’m hitting diminishing returns. Then as I read those papers I’ll take note of any citations that seem like they might also be worth including. This seems to find most or all of the available papers. It’s somewhat more prone to bias, since it’s less well-defined than the keyword-search approach, but I don’t think that’s a huge issue if you’re diligent about it.

However, it does make you more likely to miss an important paper on the first time through. So if you take this approach, it’s important that you use spreadsheets or programs to make it easy to add more papers after your initial search.

## Interpret each study

The next step is to extract the important information from each study you’re including. I don’t have super good advice for doing this systematically, but here are some important considerations:

-   Be **very** careful to distinguish _no statistically significant effect_ from _no effect_. If a study has low statistical power, it may not find a statistically significant effect, but the result may still be consistent with a large effect. For this reason, whenever you look at a measurement of effect size, you should pay as much attention to the confidence interval as to the point estimate or whether the effect is significant. If the study doesn’t provide confidence intervals, you should compute them yourself.
    
-   It’s helpful to have measurements that are directly comparable across studies. This will help you synthesize information later, and also get a sense of any [heterogeneity](http://en.wikipedia.org/wiki/Heterogeneity_%28statistics%29). In my donation matching literature review, I focused a lot on the _relative risk of donation_ for this reason.
    
-   All else equal, it’s better to have metrics that are closer to what you actually care about. If you’re measuring the effects of clean water, it’s more exciting to see a drop in mortality than just a drop in diarrhea incidence. Of course, these metrics are generally noisier and harder to collect.
    
-   Note carefully any differences between the study designs. For instance, in the donation matching literature, the plurality of the studies were fundraisers by mail, but one used a donation box and one used a check-box on students’ tuition forms. These are all possible explanations for why results might differ between studies.
    

## Evaluate reliability

After you have a handle on what each study says, the next (and more difficult) step is to figure out how strongly to believe it.

This is a process you can pay basically endless amounts of attention to, and I can’t get into all of details here. But here are some important points to watch out for:

-   For making causal inferences, [randomized controlled trials](http://en.wikipedia.org/wiki/Randomized_controlled_trial) are the gold standard, and observational studies are substantially less reliable. However, since observational studies can often cover a wider variety of population, they may be more likely to generalize well than localized randomized trials.
    
-   There are ways of making causal inferences from observational data, like [instrumental variables](http://en.wikipedia.org/wiki/Instrumental_variable), [regression discontinuity](http://en.wikipedia.org/wiki/Regression_discontinuity_design), [difference-in-differences](http://en.wikipedia.org/wiki/Difference_in_differences), or [controlling for confounders](http://en.wikipedia.org/wiki/Controlling_for_a_variable). However, these methods rest on certain assumptions which almost never hold exactly in the real world and which you could debate all day. As such, while they provide some evidence of causation, it can be weaker—sometimes much weaker—than that from an RCT.
    

For more details on assessing study reliability, check out the [Cochrane Collaboration handbook of review guidelines](http://handbook.cochrane.org/). Part 2, chapter 8 is an excellent overview of how to assess studies (particularly randomized trials) for bias. Cochrane’s guidelines are quite strict—maybe a bit too strict in some cases—but it’s a good overview of the different risks to study validity.

## Synthesize

Hopefully, at this point you have some idea what “the big picture” is. Try to describe it clearly. It’s kind to your readers if this section can stand alone so that readers can skip the analysis and spot-check it later.

The big picture might be that we don’t have enough evidence to know much for sure! Heck, that was the main conclusion of my donation-matching post. But even still, you can probably draw some conjectures or potential questions for future research.

## Examples of literature reviews

This is nowhere near a complete description of what makes a good literature review. You can probably get a better sense by reading some high-quality ones. For instance:

-   GiveWell’s intervention reports are uniformly excellent. To pick one essentially at random, their review of the evidence on [deworming](http://www.givewell.org/international/technical/programs/deworming) is a great example of how to grapple with a fairly complicated intervention with multiple outcomes of interest and few directly comparable studies.
    
-   For a more systematic take on a review you could look at the Cochrane Collaboration. They publish formal meta-analyses and have a higher standard of rigor than it’s reasonable for an independent analyst to aim for, but it’s still quite instructive to read; their assessments of study quality are pretty much the gold standard. For again a semi-random example, you could check out their [review of Vitamin A supplementation](http://www.cochrane.org/CD008524/BEHAV_vitamin-a-supplementation-for-preventing-disease-and-death-in-children-6-months-to-five-years-of-age) and [compare to GiveWell’s take](http://www.givewell.org/international/technical/programs/vitamin-A).
    
-   On the other hand, you could go all the way to the non-systematic end and look at one of [Scott Alexander’s literature reviews](http://slatestarcodex.com/2014/11/25/race-and-justice-much-more-than-you-wanted-to-know/). Scott basically doesn’t work with apples-to-apples comparison metrics or many of the finer methodological details in favor of a more readable post discussing a broader range of different interventions. This is another reasonable route to take which I haven’t discussed much because I’m less qualified. (“Step 1: write as well as Scott…”)
    

## Conclusion

Obviously these notes are incomplete—it would take far longer to get into the weeds of study review or statistical meta-analysis techniques. Nevertheless, I hope they’re at least enough to get started. Go forth and review some literature!

And as I mentioned, please hit the comment section if you’re still wondering about anything, or if you have any suggestions for improvements.

* * *

1.  Also, make sure you back up your hosted copies of the papers; I didn’t, and almost lost them when I accidentally erased the Amazon S3 bucket in question. [↩︎](https://www.benkuhn.net/litreview/#fnref:1)
    
2.  The disadvantage is that Git can be somewhat cumbersome to learn, although in my opinion it’s worth the effort. [↩︎](https://www.benkuhn.net/litreview/#fnref:2)
    
3.  It also requires access to the databases in question, which is hard if you’re not currently at a university. Google Scholar, by contrast, often has convenient links to PDFs of preprints even when the final paper isn’t open-access. [↩︎](https://www.benkuhn.net/litreview/#fnref:3)