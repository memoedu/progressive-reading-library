I just finished reading Vinay Prasad's Malignant, a book on the current state of oncology, covering everything from the way clinical trials are run to the myriad of ways cancer therapies can be administered (as neoadjuvants, adjuvants, in a metastatic setting etc).

I jokingly described Prasad's take as "cancer therapies[nihilism](https://twitter.com/ArtirKel/status/1318235096885977090)" on Twitter in the same way that John Ioannidis has been described as a methodological terrorist. It is nihilistic in that relative to what one would hear from the pharma or biotech worlds, it looks really pessimistic; though all thing considered he seems closer to the truth (Of course everyone wants to be a 'realist') than others.

Prasad's key points can be summarised in a few points:

1.  Cancer drugs work less than one might expect, and where they do it is in en specific cancer-type and setting-dependent way
2.  This is because of the way clinical trials are designed:
    1.  They are tested on unrepresentative populations so that patients respond better than patients who will actually be getting the therapy in real life
    2.  Use endpoints (i.e. progression-free survival) that are weakly correlated with the endpoints that patients care about (survival, quality of life)
    3.  In the best scenario (With some exceptions) they increase survival by a meagre amount, in the order of 2 months
3.  In turn, clinical trials are designed in such way because:
    1.  The FDA lets Pharma design and run their own trials;
    2.  It has a very lax policy for what counts as "success"
    3.  The whole system is permeated by industry-money, with the concomitant perverse incentives that generates
    4.  Pharma can run lots of trials and run weak studies, a fluke in a study will still get FDA approval and be sent to the clinic. By the time later ("Phase IV") studies are done, revealing its ineffectiveness and removing it from the market (At best!), the company will have made enough money so as to justify the initial expenditures.
    5.  Pharma is allowed to run test of drug X vs substandard care instead of drub X vs best practice
4.  What follows from this setup are:
    1.  Ever more expensive cancer drugs that don't really add that many extra years of life to patients
    2.  Copycat drugs that don't do anything novel
    3.  R&D pipelines skewed towards tried and tested approaches that get drugs approved, regardless of how effective they are
5.  And he proposes a few potential solutions, including a mix of:
    1.  Letting the FDA (Or an independent agency) design and run trials on behalf of Pharma
    2.  Empowering the FDA to reject trials that are not sufficiently up to standard (relative to now)
    3.  Restrictions on pharma funding for practicing physicians, or patient advocacy groups
    4.  A cost-benefit based approval system (Akin to NICE in the UK)

This all may sound all-too pessimistic, but the book explicitly mentions here and there a few examples of cancer drugs that do work, and do so well, chiefly [Gleevec](https://www.healthline.com/health-news/can-gleevec-treat-other-cancers)/Imatinib, that in many cases is a complete cure for a particular kind of cancer (Chronic Myeloid Leukemia) in the sense that it enables patients to live up to their regular life expectancy without major side effects. But most drugs, including newer modalities like CAR-Ts or NGS (sequencing)-based approaches to drug selection do not deliver as highly on the "curing cancer" front. Prasad is not discarding these directly, he is just saying that _right now_ they are not good enough, where good enough would mean adding years, not months, to a patient's life. Some of them belong in the lab and clinical trials, not in regular clinical use just yet. This is a similar conclusion to what Azra Raza talks about in [this podcast](https://peterattiamd.com/azraraza/); right now the focus for cancer should be on early detection. Many cancers, if caught early enough are curable, it is when they metastasize that they become problematic.

NGS approaches in particular suffer the problem that if the drugs don't work that well anyway in general, or for any subgroup with a particular mutation, sequencing will not change the clinician's decision; so it's not like Prasad would commit NGS to the dustbin of history, it's just that better drugs need to get here before it can be useful. In other chapter he mentions an example of this: cetuximab works for colon cancer if one has a regular _Ras_ gene and it doesn't if one harbors a certain mutation. So indeed with sequencing one could figure out if cetuximab will work for you or not. But, even if it works, it will still only increase survival by 5 months, on average. Prasad has an article [here](https://drive.google.com/file/d/1nrrlS0GLLDH9--CIILsqMbHrqv-78RY3/view) about one particular company doing NGS for cancer, Foundation Medicine. Derek Lowe seems to [agree](https://blogs.sciencemag.org/pipeline/archives/2016/09/12/precision-oncology-isnt-quite-there-yet) with his take. Criticisms aside, even Prasad [admits that the approach has been making progress](https://www.darkdaily.com/does-precision-oncology-offer-patients-superior-treatment-therapies-anatomic-pathologists-will-be-interested-to-learn-why-oncology-experts-disagree/) in the last decade, with the % of patients that could benefit for it increasing, albeit slowly. A [paper](https://jamanetwork.com/journals/jama/fullarticle/2730114) I was sent regarding NGS and cancer makes the point that as of 2018 there exists provable benefits from sequencing patients, at least for a particular kind of cancer (Non-small Cell Lung cancer); even there one can imagine what Prasad would say: For that particular case, at what cost, for what benefit.

## Progress in cancer therapy

How much progress has been made in cancer therapy, really? If one looks at say data from [OWID](https://ourworldindata.org/cancer-death-rates-are-falling-five-year-survival-rates-are-rising) one can see that there _seems_ to be a steady increase in survival that OWID attributes to genuine improvements in therapy, not just improvements in early detection.

Or,

> Blood cancers are probably the best examples to demonstrate how [#biotech](https://twitter.com/hashtag/biotech?src=hash&ref_src=twsrc%5Etfw) innovations have made huge impact on patient lives.
> 
> 5-Yr survival rate from 1970s to 2010s  
> CML: 22% -> 68%  
> NHL: 47% -> 73%  
> Myeloma: 25% -> 53% [pic.twitter.com/gQikY33dwT](https://t.co/gQikY33dwT)
> 
> — Andy Biotech (@AndyBiotech) [January 10, 2019](https://twitter.com/AndyBiotech/status/1083359015516676097?ref_src=twsrc%5Etfw)

The OWID chart has many kinds of cancers, and for some the reasons behind the improvements may be different. For prostate cancer it may be [improved testing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3540881/#s1title). For CML, NHL, or myeloma (All liquid cancers) it may be genuine advances in therapy (Like Gleevec) but if you have pancreatic cancer... tough luck.

Scott Alexander wrote two years ago [a review](https://slatestarcodex.com/2018/08/01/cancer-progress-much-more-than-you-wanted-to-know/) of this very same question, and he notes that 5 year survival rate increasing does not mean that drugs are becoming more effective; it may mean that more people with smaller tumors are being diagnosed and undergoing therapy, thus the population of patients may be changing, and given a constant effect of therapy, that would yield improvements. Still, he concludes

> So: age-adjusted cancer incidence rates and death rates have been going down since 1990, primarily due to better social policies like discouraging smoking. Five-year-survival rates have been gradually improving since at least 1970, on average by maybe about 10% though this depends on severity. Although some of this is confounded by improved screening, this is unlikely to explain more than about 20-50% of the effect. The remainder is probably a real improvement in treatment. Whether or not this level of gradual improvement is enough to represent “winning” the War on Cancer, it at least demonstrates a non-zero amount of progress.
> 
> I don’t want to frame this in terms of “here we DEMOLISH the pseudoscientific narrative that cancer progress is weak”. Many of the people I know who critique this research are from an older generation. They remember Nixon assuring them at the very beginning of the War on Cancer that we would have a cure within five years. If they’re really old, maybe they remember victories of that scale over polio and smallpox. If those were their hopes, it’s right for them to feel disappointed. But I come from a generation that doesn’t expect much, and I think the evidence suggests my low expectations have more or less been met.

## Universal cancer therapies?

Precision cancer therapy is something the field (This is more cancer biologists than oncologists) is heavily betting on. It seems to make sense at some level: After all cancers are very heterogeneous, not only every cancer is its own disease but also every kind of cancer harbors specific mutations, so finding a cure that's highly tailored to a patient and cancer seems easier than the undoable task of figuring out a single therapy that works for all cancer types. Is there something all cancers have in common? Yes, cells dividing a lot; but that's also the case for the gut lining, hematopoietic stem cells, or hair follicles, and precisely what chemotherapy does is target those rapidly dividing cells (hence the image of the bald cancer patient). But chemotherapy eventually is toxic and cannot be pushed beyond certain limits. As Azra Raza says in that podcast above, killing cancer cells is easy, the difficult part is not killing every other cell.

While there is no universal cancer therapy approved, or in any clinical stage, there are researchers trying to achieve such things, in particular by targeting a protein called Myc that is overexpressed in most (>70%) cancers. Myc downregulation is behind, at least in part, the anti-cancer effects of [rapamycin](https://www.nature.com/articles/s41392-018-0008-7). The problem with Myc is that for a long time it has been considered to be [undruggable](https://www.nature.com/articles/s41392-018-0008-7), but there are now a bunch of ways to target it like [Omomyc](https://www.mdpi.com/2073-4409/9/4/883/pdf), which inhibits the activity of Myc; and in doing so it can cause cells to undergo apoptosis. Cells that express Myc at normal levels are left untouched (This has some of the same side effects of chemotherapy in highly dividing cell populations). In [one study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6522349/) with 14 mice per group (control, omomyc, paclixatel, and omo+pax) mice that got both drugs all survived (Score one more point for the relevance of combined interventions):

![image-20201026170055765](https://nintil.com/images/2020-10-22-malignant-review/image-20201026170055765.png)

Admittedly a small study, in mice, and just that one. We'll see where this goes.

While the book itself is about actual treatments and does not go into the theory (What is cancer, what are its weak spots, how does chemotherapy work at the molecular level), and I don't know much about cancer specifically, after reading the book I end up with the idea that to tackle cancer _effectively_ (to cure it in most cases) one can go the easy way (very early detection, when surgery or just radiotherapy are possible) or the harder way which would be to identify in a more precise way what makes cancer cancer in general and target that. The current direction of the field seems to be admitting defeat on that front (cancer is more like cancer**s**, with each being its own world, making universal therapy hopeless), and trying instead to reduce the scope of the problem via NGS to one very specific tumor: more information, potentially more effective therapies for that particular case. But the problem with that is that cancer can mutate, and make the NGS-recommended drug insufficiently good for that patient. So is it even possible to achieve these universal anticancer drugs that no matter what can cure any cancer, wherever its stage and location in the body? What would they look like? An interesting question to ponder.