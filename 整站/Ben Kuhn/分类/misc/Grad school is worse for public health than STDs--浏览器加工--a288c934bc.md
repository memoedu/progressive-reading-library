_Epistemic status: deliberately provocative title. Caveats: “in the relevant age group,” “according to back of the envelope math,” “with some assumptions about severity definitions,” “if the correlation is causal”…_

For a long time, I have been totally mystified by the amount of human capital that is flushed down the toilet by graduate schools.

My partner is in her last year of a philosophy PhD, so I have front row seats on the train wreck. Most students meet with their adviser—the only other person who is (supposedly) invested in their work—less than once a month. (There is supposedly a “dissertation committee” composed of people that are also somewhat invested, but in practice, most committee members could be replaced with a small computer program.[1](https://www.benkuhn.net/grad/#fn:1)) And don’t get me started on the mandatory “presentation workshops” in which the “presenter” reads aloud an unintelligible paper for an hour. (Of course, this is what you’d expect since nobody has actually tried to teach the students to write or present well.) If her department worked at Wave, we would fire _literally everyone_.

Once, for a conference, we visited a different philosophy department, at a less prestigious university, housed in a structure which had set the record for the largest poured-concrete building in the US. The students seemed even more depressed than I would be if I spent most of my time in the largest poured-concrete building in the US. Eventually, I figured out that it was because they were all panicked about the job market. One of them mentioned to Eve that he worked on empathy. “What a great topic!” she replied enthusiastically. “One of my classmates is working on a paper on empathy!”

“Yeah,” he replied morosely, “empathy is really hot right now.”

(If you’re still not convinced, the way you can _really_ tell something is horribly wrong is that grad students find [PhD Comics](http://phdcomics.com/) darkly funny, not just dark.)

I’ve been low-key worried about this for a while, but it boiled over recently when Eve offhandedly mentioned a department survey that showed over half of her classmates struggling with depression or anxiety.[2](https://www.benkuhn.net/grad/#fn:2)

_Over half!_ These are some of the smartest people in their field, who I’m confident would thrive in any normally-supportive (or supportive-at-all) work environment. Instead, they’re riddled with anxiety and depression because they’ve been convinced to tie up their entire identity in being one of the lucky 10%[3](https://www.benkuhn.net/grad/#fn:3) that lands a tenure-track research job, then hung out to dry by the gatekeepers they probably thought would help them.

How the hell do people think this is reasonable?

* * *

As [often happens](https://www.benkuhn.net/nonprog), I decided to justify my frustration by quantifying _exactly how insane_ this effect is.

![](https://www.benkuhn.net/grad/dalys.svg)

First I looked for a bigger survey of graduate student depression and anxiety rates. It [wasn’t too hard to find one](https://www.nature.com/articles/nbt.4089), and the numbers were almost the same: 41% of graduate students had “moderate to severe” anxiety compared to 6% of the general population; 39% had moderate to severe depression compared to 6% of the general public.[4](https://www.benkuhn.net/grad/#fn:4)

To turn this into an absolute number, we need to know how many graduate students there are in any given year. There were 54k research doctorates awarded in 2017 ([source](https://ncses.nsf.gov/pubs/nsf19301/data) ), but each of those took multiple years. The median time to receive a PhD is 7.5 years in school, so there are about 400,000 total graduate students at any one time. Combining this with the percentages above, grad school causes 140,000 extra cases of anxiety, and 132,000 of depression.

We also need to know exactly how bad it is to be depressed or anxious. For that, we can use what’s known as a [disability-adjusted life year](https://en.wikipedia.org/wiki/Disability-adjusted_life_year) or DALY. DALYs are a way of putting all health problems on a common scale, by asking people to trade off e.g. 1 year of healthy life vs. 2 years of life with depression—if they (on average) choose the latter, that means a year of depression is (on average) worth at least half a year of healthy life.

The Global Burden of Disease project [calculated](http://ghdx.healthdata.org/record/ihme-data/gbd-2017-disability-weights) disability weights for anxiety and depression. It’s a bit hard to get an apples to apples comparison with the “moderate to severe” description from the survey above, but let’s be conservative and use the weights for moderate depression (0.396) and moderate anxiety (0.133).

That means the average graduate student loses 0.99 disability-adjusted life years to depression, 0.34 years to anxiety, and 6.17 years to studying irrelevant coursework, writing a dissertation [no one will read](https://arxiv.org/ftp/physics/papers/0701/0701012.pdf), wandering around [looking for free food](https://lists.csail.mit.edu/mailman/listinfo/vultures), etc.

* * *

Multiply these by the number of depressed/anxious people above and you get a grad-school-induced DALY burden of 71,000 per year. That is, every year, grad schools inflict an amount of suffering equal to shortening the healthy lives of 71,000 people by one year.

OK, that’s a big number, but how do we interpret it? The easiest way is by comparing it to more conventional public health issues. The Global Burden of Disease project also calculated the total DALY costs of many other health problems, and built a nice [tool](http://ghdx.healthdata.org/gbd-results-tool) to let you slice and dice the data however you want. We’ll look at the impact of other diseases on 20-30 year olds (this is a bit lower than the age range for graduate school, but they only offer age ranges in 5-year increments).

I gave the result away with my clickbait headline, but using [this query](http://ghdx.healthdata.org/gbd-results-tool?params=gbd-api-2017-permalink/d9fd21891d6d4890fdd2a2f011906e82), we can see the total burden of “HIV/AIDS and sexually transmitted infections” is 47,654—so grad school handily tops STDs as a source of suffering in the US.

* * *

1.  For instance, `python -c "import time; while True: time.sleep(60)"` [↩︎](https://www.benkuhn.net/grad/#fnref:1)
    
2.  Specifically, struggling with “mild to severe” depression or anxiety over the last two weeks. [↩︎](https://www.benkuhn.net/grad/#fnref:2)
    
3.  Around 500 PhDs are awarded per year ([source](https://humanitiesindicators.org/content/indicatordoc.aspx?i=56#fig270)). PhilJobs currently [shows](https://philjobs.org/jobQuery/tt) 113 tenure-track jobs, but many are outside the US or at non-research colleges; I’m ballparking 50% research jobs. [↩︎](https://www.benkuhn.net/grad/#fnref:3)
    
4.  The 6% general population figures do not appear to be age-adjusted. The Global Burden of Disease [data](http://ghdx.healthdata.org/gbd-results-tool?params=gbd-api-2017-permalink/6fe2cff6007e77cf82251c33fbf12a36) suggest that in the 20-30 age bracket, major depression has a prevalence of more like 4-5% and anxiety disorders have a prevalence of more like 7-8%, but these could also be due to differences in the scale used. Either way, both numbers are so far below the graduate school numbers that it doesn’t change the analysis much. [↩︎](https://www.benkuhn.net/grad/#fnref:4)