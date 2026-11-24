Not that many links this months, but there's an special on AI safety at the end

Top forecasters vs [domain](https://forum.effectivealtruism.org/posts/qZqvBLvR5hX9sEkjR/comparing-top-forecasters-and-domain-experts) experts

An introduction to high-speed (i.e. hypersonic and supersonic) [flight](https://www.primemoverslab.com/ideas/high-speed-flight)

Givewell (Labs, which later became [OpenPhilantrophy](https://www.openphilanthropy.org/blog/meta-research)) has an old [report](https://blog.givewell.org/2013/06/06/meta-research-update/) on meta-research (as in science of science or meta-science). They started by taking to _the leads we had – contacts at [Cochrane](https://blog.givewell.org/2012/09/27/us-cochrane-center-uscc-gets-our-first-quick-grant-recommendation/) as well as individuals suggested by John Ioannidis – and get referrals from them to other people he should be speaking with._. Whereas I just read a bunch of papers and didn't talk to anyone. It seems like we found the same sort of issues. But I couldn't find the final report to see if we came to different conclusions. I could find [this](https://www.openphilanthropy.org/early-open-philanthropy-thinking-science-funding). It's good in that it seems they came to the same rough picture that I came to. It's bad in that they did so 7 years ago, and not that much has changed. In the interim, OpenPhil's investigations led to funding some interesting science, including Ed Boyden's work in [expansion microscopy](https://twitter.com/eboyden3/status/757232074407481346).

I came across a [critique](https://arcade.stanford.edu/rofl/deceit-desire-and-literature-professor-why-girardians-exist) of Peter Thiel's favorite philosopher, Rene Girard.

[Study](https://elifesciences.org/articles/28434) fails to replicate another study (as part of the Reproducibility in Cancer Biology project) showing that a certain kind of bacteria is implicated in colon cancer. Naively, this would lead one to discard that hypothesis. But the paper is wiser than that: One has to evaluate the body of evidence as a whole. My take after reading that is that we need some more managed science: Taking a series of half-answered unclear questions, getting various groups to fund well powered and designed experiments (including replications of each) and seeing what can be said about this question. I might follow up with a post about this.

DeepMind's _Advancing mathematics by guiding human intuition with AI_, [explained](https://www.youtube.com/watch?v=UPCI1-ZvwOg) by Tim Nguyen.

NIHR (UK) is launching their own '[Fast Grants](https://www.nihr.ac.uk/blog/a-faster-way-to-accelerate-innovation/30079)'

Science, [philantrophy](https://kill-the-newsletter.com/alternates/q8cyp2xqeelufuvb.html), and tech (and crypto)

The potential of ML for hardware [design](https://www.youtube.com/watch?v=FraDFZ2t__A)

Supercharging biomedical science at the [National Institutes of Health](https://www.dayoneproject.org/post/supercharging-biomedical-science-at-the-national-institutes-of-health)

AI-powered drug discovery is also AI-powered [bioweapon](https://www.nature.com/articles/s42256-022-00465-9?fbclid=IwAR11_V1cd9SUxEvUfwrWMA7TUcroyYIY1nBDUL3KaS-8B4rG5MIqZCmjm0M) discovery

To prove that its funding system is fucked up, NIH rejects grant because of a [slight](https://twitter.com/ns_whit/status/1499887361894801415) margin issue

10% of people have an accessory [spleen](https://en.wikipedia.org/wiki/Accessory_spleen)

[List of unusual biological names](https://en.wikipedia.org/wiki/List_of_unusual_biological_names), featuring proteins like Lunatic Fringe or MAP kinase kinase kinase kinase

Haven't looked into this in a very long time. I got curious after talking to some of the people that work in the area so I thought it was time to see what's going on there these days. Here are some things I've read this month that fall under the broad umbrella of designing AI systems that are safe (by various definitions of safe). They are all introductory material/general explainers. My initial impression of the field (at least as of [2016](https://nintil.com/on-the-possible-futility-of-friendly-ai-research/); though the end of the post notest that at some point I became more optimistic) was that they were trying to design something that seemed like _provably_ safe AI systems which is hopeless (This was perhaps a misunderstanding on my part on what the early AI safety people were trying to do, but _Agents Foundations_, which was deemed a failure, seems to be literally this). Now the field seems to have merged, or at least taken inspiration from, the explainable ML community and AI safety is nowadays an eminently empirical field centered around understanding the kinds of models, like Transformers, that seem promising, and trying to devise new ways to train them that lead to desired behaviours, for example [trying to get language models to output benign completions to a given prompt](https://www.alignmentforum.org/posts/k7oxdbNaGATZbtEg3/redwood-research-s-current-project). There's more tool building, more concrete tractable problems and less theorising about arbitrarily general intelligent systems.

-   [Steve Byrnes' essays](https://sjbyrnes.com/agi.html)
-   Scott Alexander on the difficulty of predicting when we'll get [AGI](https://astralcodexten.substack.com/p/biological-anchors-a-trick-that-might?s=r)
-   Richard Ngo's [AGI safety from first principles](https://www.alignmentforum.org/s/mzgtmmTKKn5MuCzFJ)
-   Paul Christiano: [Current work](https://forum.effectivealtruism.org/posts/63stBTw3WAW6k45dY/paul-christiano-current-work-in-ai-alignment) in AI alignment (And [research agenda](https://www.alignmentforum.org/posts/Djs38EWYZG8o7JMWY/paul-s-research-agenda-faq))
-   Why AI alignment could be hard with [modern](https://www.cold-takes.com/why-ai-alignment-could-be-hard-with-modern-deep-learning/) deep learning
-   Attempted Gears Analysis of AGI intervention Discussion With [Eliezer](https://www.lesswrong.com/posts/xHnuX42WNZ9hq53bz/attempted-gears-analysis-of-agi-intervention-discussion-with-1)
-   AI Alignment and [Safety](https://www.vincentweisser.com/ai-alignment)

I think the field has evolved positively since I last checked on it, and is now less of a weird niche where smart people go get high on math and more in dialogue with leading edge ML research where code (and readable essays) is the main output.

(Edit 2022/03/23): Dan Roberts sends me a link to a [book](https://www.cambridge.org/core/books/principles-of-deep-learning-theory/3E566F65026D6896DC814A8C31EF3B4C) ([draft](https://arxiv.org/abs/2106.10165), [twitter thread](https://twitter.com/danintheory/status/1405913049308057600)) on using [effective theory](https://en.wikipedia.org/wiki/Effective_theory) to understand deep neural networks: theorizing about deep neural nets was not so hopeless as I thought, if one is willing to restrict the model to deep neural networks instead of very abstract descriptions of algorithms that an AGI could plausibly run.

Let me know if there's anything else that belongs in this section! -> artir \[at\] nintil \[dot\] com