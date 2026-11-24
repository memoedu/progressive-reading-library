EDIT 2021-01-17: I had mistakenly said here that UBX0101 is the same as Navitoclax. Though there are some papers that report this, it's actually not the case.

Unity Biotechnology recently announced that their drug under development, UBX0101 failed a Phase II trial. Lots of people in the longevity space were hoping that it would succeed: it would be the first drug built on top of work on senolytics, and be also a therapy that does not merely slow down the progression of a disease, in this case osteoarthritis, but also reverses some of its causes (senescent cells).

The biology behind looks solid: senescent cells accumulate with age, they have been implicated in disease, and removing them leads to improvements. Senescent cells also accumulate in joints, and in vitro removing senescent cells seemed to improve their proliferation rate. Their Phase I data look promising. And yet they failed. Here I'll look at why, and if there are early signs that should make one expect something will fail or succeed.

As an intro, Eleanor Sheekey has a [great video](https://www.youtube.com/watch?v=hujnZCY1m2s) about precisely this topic, introducing senescent cells, senolytics, and the mechanism by which UBX0101 acts. Watch that first.

First, some definitions:

-   Arthritis is a family of diseases that all cauase inflammation in the joints. In particular, osteoarthritis is a kind of arthritis caused by pure mechanical tear and wear of the cartilage inbetween bones. In the extreme case, you end up with painful bone-on-bone contact
-   UBX0101: The compound they used, reported by some to be the same as Navitoclax, but it seems it's not. UBX0101 inhibits the inactivation of p53 by Mdm2. In either case, it can be said that they broadly make it easier for a cell to undergo apoptosis, so if the cell is damaged in some sense, it will be nudged towards apoptosis rather than sticking around. UXB0101 is injected in the joint of interest.
-   [WOMAC](https://www.physio-pedia.com/WOMAC_Osteoarthritis_Index) index: A composite measure of how severe arthritis is. It is based on a [questionnaire](https://www.yrmc.org/docs/default-source/medservices/womac-osteoarthritis-index.pdf?sfvrsn=0) that patients complete. It has three subscales, and the total WOMAC measure is calculated by summing them over OR by averaging each of the subscales. Being a self-administered questionnaire based it has room for variation and it will yield noisier results than an objective measure; however at the end of the day you are trying to improve subjective symptoms like "X is difficult to do" or "It hurts" so there is little choice but to use such measures. Unity used the average of each section instead of the sum. The original and latest [(NRS 3.1)](https://www.google.com/search?sxsrf=ALeKk023T7F0_XAS9S5tLlFDqO7dUKVk6A%3A1598180218061&ei=ektCX9udA4_hUOr5itAO&q=WOMAC+questionnaire+%28version+NRS+3.1%29&oq=WOMAC+questionnaire+%28version+NRS+3.1%29&gs_lcp=CgZwc3ktYWIQAzoECCMQJ1DKmQhYvZsIYOWdCGgAcAB4AIABfogB3wKSAQMwLjOYAQCgAQGqAQdnd3Mtd2l6wAEB&sclient=psy-ab&ved=0ahUKEwjbwryhlbHrAhWPMBQKHeq8AuoQ4dUDCAw&uact=5) score ranges are:
    -   WOMAC-A (Pain), with a range of 0-20 (0-100)
    -   WOMAC-B (Stiffness) with a range of 0-8 (0-100)
    -   WOMAC-C (Physical function) with a range of 0-68 (0-40)
-   Kellgren-Lawrence score: A subjective measure based on radiography of how deteriorated a joint is. 0 means no signs of arthritis, and 4 means very advanced arthritis.
-   Placebo effect in this context. UBX0101 is administered by injection into the joint in question, in this case the knee. The placebo used in the control group is a sham injection of saline. Surprisingly, this effect is very strong, and moreover it can generally last months, so either the mere fact of puncturing the joint does something clinically meaningful or you can indeed think your pain away. In a meta-analysis, [Previtali et al. (2020)](https://journals.sagepub.com/doi/abs/10.1177/1947603520906597) find that by 6 months, placebo injections cause a reduction of -3.3 on WOMAC-A, -1.1 in WOMAC-B, and -10.1 in WOMAC-C. Average reduction in total WOMAC was -10.5

With this in mind we can look at the results from the Phase 2 and understand what is going on. The trial consisted of a single injection of UXB0101 at different dosages with 45 patients per group, and looking at WOMAC-A at 12 weeks as a main endpoint.

![image-20200823105724861](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823105724861.png)

Here CFBL means change from baseline, which from other of their slides [here](https://ir.unitybiotechnology.com/static-files/6f0c8e98-ccf8-4079-8a42-d1372b865e33) was around 2. In other words, both placebo and their drug cuts moderate pain to around half of that (mild pain). The same is true for WOMAC-C. Unity has a relatively cursed figure here

![image-20200823113251373](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823113251373.png)

Where they compare the way most people do WOMAC and they way they do it (By doing a 0-4 average), I hope at least they used the actual data to come up with the equivalence. Here it seems they used the 0-100 scale and so the average effect may be a 17% reduction. Their placebo would have achieved a reduction of perhaps 28% in the chart but that doesn't seem to cohere with the previous slide where it looks more like 50%. Probably an artifact of using different scales, making it more difficult to compare.

Be it as it may, the effects were the same in placebo and intervention groups, and I'll take the equivalence of 28 points in the 0-100 WOMAC-A as similar to 1 point in the scale that Unity used. 28 points is like 2.8 in 0-10, and in turn 3.3 had been the average reduction seen from placebo in the meta-analysis, so perhaps what they say was a strong placebo there was actually a regular placebo effect.

If we look now at their [Phase 1](https://ir.unitybiotechnology.com/static-files/9f0381c6-ae2e-48c0-8751-8bf7aac9ef1b) study, the groups they looked at are similar to the Phase 2 ones, so any different effect is probably not due to looking at radically different groups. But to make things more confusing, it seems like here they are looking at the original WOMAC, given the values shown. For example the pain scales goes 0-20 and they have values around the middle, and likewise for the other scores.

![image-20200823120703647](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823120703647.png)

However when reporting the values they revert back to their averages:

![image-20200823120858061](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823120858061.png)

Seeing this, it may at first look promising: The high doses (4, 2, and 1 mg) seem clearly different from placebo; but oddly the lower doses seem worse!

If we look at other trials for osteoarthritis, we can see that the effects start to become more apparent _after week 12_, which means that Unity's trial may have been improperly designed.

For example this from Samumed ([Yazici et al., 2020](https://onlinelibrary.wiley.com/doi/abs/10.1002/art.41315)) which if you see the rest of the paper they also failed their Phase 2 in the general population (That doesn't deter them from going to [Phase](https://www.globenewswire.com/news-release/2019/05/02/1815229/0/en/Samumed-Launches-Phase-3-Lorecivivint-SM04690-Clinical-Program-in-Knee-Osteoarthritis.html) 3 though), but you can see that IF there is separation between the groups (not all figures in their paper show that), you can start seeing that after 12 months. 12 is the bare minimum you need I'd say. And also, that the placebo effect was around 45%, in fact way stronger than what the chart Unity has may make us believe. Given that the drug in this Samumed study showed similar results to placebo, this may point to cohort-dependent effects: That changes and starting points for WOMAC scores matter greatly.

![image-20200823122207464](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823122207464.png)

A Phase 3 study for a cell therapy ([Kim et al., 2018](https://www.liebertpub.com/doi/full/10.1089/humc.2017.249)) which was deemed successful, looking at total (not just pain) WOMAC looks like this:

![image-20200823122905613](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823122905613.png)

As it looks like this one may work, it's worth also mentioning that their [Phase II trial](https://online.boneandjoint.org.uk/doi/full/10.1302/0301-620X.97B7.35852) (Lee et al., 2015) did not show an improvement over placebo after 24 weeks (6 months); but WOMAC was not their main endpoint so they proceeded ahead, as the primary endpoint ([IKDC](https://www.hss.edu/secure/files/WSMC-ikdc.pdf)) actually showed a substantial improvement (2x better than placebo). As it happens, while WOMAC widely used and deemed acceptable ([Collins et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4336550/)), it is not better than alternatives like IKDC ([van den Graaf, 2014](https://pubmed.ncbi.nlm.nih.gov/24618098/)). And interestingly, their target (TGF-b) has recently been shown this year to work for osteoarthritis in rats in combination with Klotho ([Izpisua-Belmonte et al., 2020](https://link.springer.com/article/10.1007%2Fs13238-019-00685-7))

In any case going back to the Unity Phase 1 slides, they also show a decomposition by high/low treatment, which looks nicer

![image-20200823123353465](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823123353465.png)

Ok but to really understand what is going on we have to go further back in time. Each of those points is hiding an individual patient, so it's be worth to see what happened to each person that underwent treatment. Fortunately in some previous slides from [December 2019](https://ir.unitybiotechnology.com/static-files/c534b475-1455-4921-a5cd-4c2b9df74dd4) they do have that

![image-20200823123856517](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823123856517.png)

You can see that there is lots of variability here, but if one is optimistic, one may be inclined to think that the higher dose was actually working; note that the placebo patients are also aggregated, but it may well be that if you also plot the placebo patients you also get lots of variability, so seeing data like this I wouldn't have been very optimistic. For example they claim that variability goes down but the 0.2 and 1 mg responses look similarly variable, with an increase in the 2 mg one and a reduction in the 4 mg one.

If you are doing such small sample size trials and you want robustness, ALL your patients should show a good response. For example if one looks at the [TRIIM](https://onlinelibrary.wiley.com/doi/full/10.1111/acel.13028) trial (thymic regeneration in 9 individuals), they achieved either no response (in individuals with a healthier thymus) or a positive response, in any case following a reasonable relation, that the more atrophied the thymus was, the more scoped there was to regenerate it:

![image-20200823124654320](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823124654320.png)

Besides WOMAC, Unity also looked at a numerical rating of pain where again grouping by high/low/placebo showed significant effects. The starting score was between 5.9 and 6.76 and placebo reduced the score on average by 2 points, with more than 4 in the case of the highest dose. However, look at the actual picture:

![image-20200823134009349](https://nintil.com/images/2020-08-23-why-ubx0101-failed/image-20200823134009349.png)

How on earth can your 0.2mg dose yield results that are the second best at the end of the trial? This does look fishy.

Ok, so we have now some heuristics to see if a small sample Phase 1 will fail or succeed:

-   Are the measuring the right thing?
-   Is the response uniform, or driven by outliers?
-   Are different scales coherent with each other?
-   Do we see a dose-response pattern?

Also, it would make the study more robust and easier to analise if they release all the data and a pre-analysis plan to make sure they are not trying to milk the data for spurious effects.

**But.. why did it fail?**

In a study from 2017, [Eliseeff et al.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5785239/) look at induced ([post-traumatic](https://en.wikipedia.org/wiki/Post-traumatic_arthritis)) osteoarthritis in mice, treating them with UBX0101. There, they point to UBX0101 actually selectively eliminating senescent cells in vitro, and they tried repeated doses of UBX0101every 2 days in mice to determine if it worked. And it did indeed work. But note that it worked for an artificially caused injury (ACLT, see [Kuyinu et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4738796/)) where the injure the anterior cruciate ligament. So we can already see two reasons why this may not have worked: First the kind of arthritis was not the same, and second they dosed the mice repeatedly, while Unity did so just once, the reason being that UBX0101 has a low residence time, it will be cleared out fast enough that it may not have time to kill a sufficient number of senescent cells. You can see that in mice with a single dose it doesn't do anything for one of the tests they used, they needed at least 5 to see a significant effect.

![Screenshot 2020-08-23 at 13.14.37](https://nintil.com/images/2020-08-23-why-ubx0101-failed/lastpic.png)

So perhaps UBX0101 works after all, but maybe Unity didn't deliver the repeated doses that are necessary to see an effect, or maybe it only works in this particular mouse model due to some difference between ACLT and age-induced osteoarthritis, plus some components of OA that may only be present in humans in increased amounts, perhaps having to do with stiffening of the extracellular matrix, which senolytics may not revert. Unity also did a small [repeat-dose study](https://clinicaltrials.gov/ct2/show/NCT04229225) but the data is not out yet.

I think senolytics remain a valid and interesting therapeutic avenue, and the results of this test don't change that at all.