---
title: October 2020 News
description: October 2020 Gwern.net newsletter with links on AI scaling, Euclid; further site reorganization & improvement.
created: 2019-12-26
status: finished
confidence: log
css-extension: dropcaps-de-zs
backlink: False
...

October 2020's [Gwern.net](/newsletter/2020/10 "'October 2020 News', Gwern 2019") [newsletter](https://gwern.substack.com/ "'Gwern.net newsletter (Substack subscription page)', Gwern 2013") is now out; previous, [September 2020](/newsletter/2020/09 "'September 2020 News', Gwern 2019") ([archives](/doc/newsletter/index)).
This is a collation of links and summary of major changes, overlapping with my [Changelog](/changelog); brought to you by my donors on [Patreon](https://www.patreon.com/cw/gwern).

# Writings

- more [Gwern.net fixes](/design "'Design Of This Website', Gwern 2010"): line-breaking on slashes, hyphenation & justified text on Chrome desktop browsers using Edward Kmett's [`hyphenation`](https://hackage.haskell.org/package/hyphenation "'hyphenation: Configurable Knuth-Liang hyphenation', Kmett 2012") at compile-time; width-full mobile images; rewrote GoodReads conversion for much more readable [book review page](/review/book "'Book Reviews', Gwern 2013"); browsable directories; popup annotations now render more like body
- compiled all [movie](/review/movie "'Movie Reviews', Gwern 2014") & [anime reviews](/review/anime)

# Links

## AI

- ["Human-Level Performance in No-Press Diplomacy via Equilibrium Search"](https://arxiv.org/abs/2010.02923#facebook), Gray et al 2020
- ["DreamerV2: Mastering Atari with Discrete World Models"](https://arxiv.org/abs/2010.02193#deepmind "'Mastering Atari with Discrete World Models', Hafner et al 2020"), Hafner et al 2020
- ["Taxonomy of Real Faults in Deep Learning Systems"](https://arxiv.org/abs/1910.11015), Humbatova et al 2019 (on the painfully primitive & error-prone tooling of AI research (such a contrast to [Lisp machines](https://en.wikipedia.org/wiki/Lisp_machine), where the tooling was superior to today's DL ecosystem but the AI research was useless)---"neural networks want to work", so how many serious bugs do we never realize? Every researcher has a story, like [R2D2](https://openreview.net/forum?id=r1lyTjAqYX#deepmind "'R2D2: Recurrent Experience Replay in Distributed Reinforcement Learning', Kapturowski et al 2018"), of how their code had a fatal bug but worked anyway... Just not nearly as well as it *should* have. Recently in Tensorfork we learned our BigGAN samples didn't work---because the Batch Norm statistics weren't being used at runtime like we thought!)
- ["Artificial Intelligence versus Maya Angelou: Experimental evidence that people cannot differentiate AI-generated from human-written poetry"](https://arxiv.org/abs/2005.09980), Köbis & Mossink 2020 (humans preferred screened GPT-2 poem completions 43% of the time)
- ["Tensorflow Neural Network Playground"](https://playground.tensorflow.org/ "'A Neural Network Playground', Smilkov & Carter 2016") (explorable: train small NNs in-browser to watch how their decision boundaries evolve)

[Matters Of Scale](https://www.reddit.com/r/mlscaling/ "'ML Scaling subreddit', Gwern 2020"):

- ["Scaling Laws for Autoregressive Generative Modeling"](https://arxiv.org/abs/2010.14701#openai), Henighan et al 2020 ([Kaplan talk](https://www.youtube.com/watch?v=QMqPAM_knrE?t=2380 "'Neural Scaling Laws and GPT-3', Jared Kaplan"); GPT-3 was not a fluke nor language-specific: all modalities tested---math, video, images, text, combined---scale cleanly and in the same way where bigger models = better; the unsupervised/pretrained models then transfer to supervised learning, like image classification. *GPT-3 all the things!*)
- ["Vision Transformer (ViT): An Image is Worth 16×16 Words: Transformers for Image Recognition at Scale"](https://arxiv.org/abs/2010.11929#google "'Vision Transformer: An Image is Worth 16×16 Words: Transformers for Image Recognition at Scale', Dosovitskiy et al 2020"), Anonymous et al 2020 (as expected, pure Transformers surpass CNN SOTA when scaled appropriately: as fast, better, simpler with fewer parameters *and* cheaper to train, with improvements to come: "Finally, [we plan] to further scale ViT, given that the performance does not seem yet to be saturating with the increased model size..." [lucidrains's implementation](https://github.com/lucidrains/vit-pytorch))
- ["Meta-trained agents implement Bayes-optimal agents"](https://arxiv.org/abs/2010.11223#deepmind), Mikulik et al 2020; ["One Epoch Is All You Need"](https://arxiv.org/abs/1906.06669), Komatsuzaki 2019 (are bigger deep models better because they [meta-learn Bayes-optimal inference](/backstop#deep-bayes)?)
- NMT: ["mT5: A massively multilingual pre-trained text-to-text transformer"](https://arxiv.org/abs/2010.11934#google), Xue et al 2020 (13b-parameters); ["M2M-100: Beyond English-Centric Multilingual Machine Translation"](https://arxiv.org/abs/2010.11125#facebook "'Beyond English-Centric Multilingual Machine Translation', Fan et al 2020"), Fan et al 2020 ([blog](https://ai.meta.com/blog/introducing-many-to-many-multilingual-machine-translation/ "The first AI model that translates 100 languages without relying on English data"); 15b-parameters, using ZeRO)
- ["Pushing the Limits of Semi-Supervised Learning for Automatic Speech Recognition"](https://arxiv.org/abs/2010.10504#google), Zhang et al 2020 (pretraining enables scaling to 1b-parameter [efficient-attention](/doc/ai/nn/transformer/attention/index "'Efficient Attention: Breaking The Quadratic Transformer Bottleneck', Gwern 2020") [Conformers](https://arxiv.org/abs/2005.08100#google "'Conformer: Convolution-augmented Transformer for Speech Recognition', Gulatti et al 2020"))

## Genetics:

Everything Is Heritable:

- ["Multivariate genomic analysis of 1.5 million people identifies genes related to addiction, antisocial behavior, and health"](https://www.biorxiv.org/content/10.1101/2020.10.16.342501.full), Linner et al 2020; ["Happiness and Wellbeing; the value and findings from genetic studies"](https://osf.io/preprints/psyarxiv/zvu8j), Van de Weijer et al 2020
- ["Combined Utility of 25 Disease and Risk Factor Polygenic Risk Scores for Stratifying Risk of All-Cause Mortality"](https://www.medrxiv.org/content/10.1101/2020.03.13.20035527.full), Meisner et al 2020
- ["Exome sequencing and characterization of 49,960 individuals in the UK Biobank"](https://www.nature.com/articles/s41586-020-2853-0), Van Hout et al 2020 ("2% of this population has a medically actionable variant")
- ["Exploring the variance in complex traits captured by DNA methylation assays"](https://www.biorxiv.org/content/10.1101/2020.10.09.333542.full), Battram et al 2020 (epigenome heritability/variance-components: ~0 across 400 traits)

Engineering:

- ["The second decade of synthetic biology: 2010--2020"](https://www.nature.com/articles/s41467-020-19092-2), Meng & Ellis 2020
- [Emmanuelle Charpentier](https://en.wikipedia.org/wiki/Emmanuelle_Charpentier) & [Jennifer Doudna](https://en.wikipedia.org/wiki/Jennifer_Doudna) [win 2020 Nobel Prize for](https://www.nobelprize.org/prizes/chemistry/2020/press-release/ "'Press release: The Nobel Prize in Chemistry 2020', Sciences 2020") [CRISPR](https://en.wikipedia.org/wiki/CRISPR)

## Statistics/Meta-Science

- ["Publication rate in preclinical research: a plea for preregistration"](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8647586/), van der Naald et al 2020 ([media](https://www.science.org/news/2020/10/millions-animals-may-be-missing-scientific-studies "Millions of animals may be missing from scientific studies"); ~75% of sacrificed animals yield no published result; why do we have IRBs & 'ethics boards' supposedly ensuring animal welfare when they cannot even ensure the data animals are killed for gets published?)
- ["The Empirical Metamathematics of Euclid and Beyond"](https://writings.stephenwolfram.com/2020/09/the-empirical-metamathematics-of-euclid-and-beyond/), Stephen Wolfram (analyzing [_Elements_](https://en.wikipedia.org/wiki/Euclid%27s_Elements) as a DAG); ["Singing Euclid: the oral character of Greek geometry"](https://intellectualmathematics.com/blog/singing-euclid-the-oral-character-of-greek-geometry/), Viktor Blåsjö (see also ["Having Had No Predecessor to Imitate, He Had No Successor Capable of Imitating Him"](https://www.fantasticanachronism.com/2020/01/17/having-had-no-predecessor-to-imitate/index.html))

## Politics/Religion

- *Immanentizing the Eschaton*, [Anabaptist](https://en.wikipedia.org/wiki/Anabaptism) edition: [Münster rebellion](https://en.wikipedia.org/wiki/M%C3%BCnster_rebellion); [the wreck of the _Batavia_](https://en.wikipedia.org/wiki/Batavia_\(1628_ship\))

## Psychology/Biology

- ["Effects of caloric restriction on human physiological, psychological, and behavioral outcomes: highlights from CALERIE phase 2"](/doc/longevity/fasting/2020-dorling.pdf), Dorling et al 2020
- ["Questions About Trees"](http://bit-player.org/2020/questions-about-trees), Brian Hayes ("the paradox of the plankton": what maintains diversity in forests?)
- ["The hearing aid dilemma: amplification, compression, and distortion of the neural code"](https://www.biorxiv.org/content/10.1101/2020.10.02.323626.full), Armstrong et al 2020 (towards hearing aids designed by blackbox optimization to make neural patterns match patterns of normal hearing?)
- ["The role of cat eye narrowing movements in cat-human communication"](https://www.nature.com/articles/s41598-020-73426-0), Humphrey et al 2020
- [Encephalitis lethargica](https://en.wikipedia.org/wiki/Encephalitis_lethargica) (["A viral infection of the mind? The curious case of encephalitis lethargica"](https://theconversation.com/a-viral-infection-of-the-mind-the-curious-case-of-encephalitis-lethargica-660))
- ["Brain Training Habits Are Not Associated With Generalized Benefits to Cognition: An Online Study of Over 1000 'Brain Trainers'"](/doc/dual-n-back/2020-stojanoski.pdf "'Brain training habits are not associated with generalized benefits to cognition: An online study of over 1000 &#39;brain trainers&#39;', Stojanoski et al 2020"), Stojanoski et al 2020
- ["C~60~ in olive oil causes light–dependent toxicity and does not extend lifespan in mice"](/doc/longevity/2020-grohn.pdf "'C<sub>60</sub> in olive oil causes light-dependent toxicity and does not extend lifespan in mice', Grohn et al 2020"), Grohn et al 2020 (Back in 2012, I found the original result unbelievable for many reasons[^C60]; but in case anyone still takes it seriously...)

[^C60]: Aside from extremely strong priors about life extension effects (typically much smaller) and the [low quality of animal research](/replication#animal-models "'The Replication Crisis: Flaws in Mainstream Science § Animal models', Gwern 2010"), some of the specific problems with the original result included:

    - contradictory median/lifespan figures
    - duplicate image
    - small sample
    - doses of C~60~ small enough that the direct antioxidant activity can't be responsible
    - bizarre intervention, whose justifying cites were not published when experiment started
    - the C~60~ was administered for brief period ('imagine taking a supplement only during your 40s and doubling your lifespan')
    - the massive life extension observed in the olive-oil-only rats---not doubling, but still implausible

## Technology

- [LibGen/Sci-Hub fulltext search engine](https://z-lib.is/fulltext): search 5.5m+ downloadable books/77.5m+ papers (great alternative to Google Books/Scholar)
- ["Profiling a warehouse-scale computer"](/doc/cs/hardware/2015-kanev.pdf#google), Kanev et al 2015 (previously: [Ren et al 2010](/doc/cs/hardware/2010-ren.pdf#google "Google-Wide Profiling: A Continuous Profiling Infrastructure for Data Centers"))
- ["Expert judgment on markers to deter inadvertent human intrusion into the Waste Isolation Pilot Plant"](/doc/technology/1993-trauth.pdf), Trauth et al 1993 (the [Sandia](https://en.wikipedia.org/wiki/Sandia_National_Laboratories) [long-term nuclear waste warning message](https://en.wikipedia.org/wiki/Long-term_nuclear_waste_warning_messages) expert report for the [WIP](https://en.wikipedia.org/wiki/Waste_Isolation_Pilot_Plant))
- ["The Elusive Peril of Space Junk: Millions of human artifacts circle the Earth. Can we clean them up before they cause a disaster?"](https://www.newyorker.com/magazine/2020/09/28/the-elusive-peril-of-space-junk "'The Elusive Peril of Space Junk', Khatchadourian 2020") (the [Kessler syndrome](https://en.wikipedia.org/wiki/Kessler_syndrome) never went away)
- ["The rise and fall of Adobe Flash: Before Flash Player sunsets this December, we talk its legacy with those who built it"](https://arstechnica.com/information-technology/2020/07/the-rise-and-fall-of-adobe-flash/); ["How Flash Games Shaped The Video Game Industry: Flash is dead. But the influence of Flash games on modern gameplay is inescapable"](https://www.flashgamehistory.com/), Jonas Richner; ["Reliving Flash Game History"](https://www.alexirpan.com/2021/02/18/flash-games.html), Alex Irpan (why were [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) [browser games](https://en.wikipedia.org/wiki/Browser_game) so creative? The Flash scene benefited from easy entry using an integrated programming environment, small-scale games enabling rapid iteration, many exemplars to imitate, envy and admiration driving imitators, the conviction that improvements were possible, a global talent pool with lots of free time, and game websites like [Newgrounds](https://en.wikipedia.org/wiki/Newgrounds) to curate games to create a sophisticated & demanding userbase which could drive mega-traffic to hits, in a virtuous cycle of recruitment/competition/feedback/improvement. See also [Bakewell](/review/bakewell "'Origins of Innovation: Bakewell & Breeding', Gwern 2018").)
- ["This page is a truly naked, Brutalist HTML quine."](https://secretgeek.github.io/html_wysiwyg/html.html "'<code>&lt;h1&gt;This page is a truly naked, brutalist html quine&lt;/h1&gt;</code>', Bambrick 2019"); [A self-building website](https://www.strml.net/ "'STRML: Projects and Work', Reed 2014") (cf. [naked objects](/doc/cs/algorithm/2001-pawson.pdf "‘Naked objects: a technique for designing more expressive systems’, Pawson & Matthews 2001"))

## Fiction

- ["My Dad, the Pornographer"](https://www.nytimes.com/2015/02/08/magazine/my-dad-the-pornographer.html) (on [Andrew J. Offutt](https://en.wikipedia.org/wiki/Andrew_J._Offutt))
- [The Monkees](https://en.wikipedia.org/wiki/The_Monkees) (from passing references, I was always confused if 'The Monkees' were a TV show or band---apparently they were *both*! Best-selling ones, too.)
