---
title: December 2018 News
description: December 2018 Gwern.net newsletter with links on genetic engineering, NLP/DRL, history of technology, online economics; 2 book and 3 movie reviews.
created: 2018-11-18
status: finished
confidence: log
css-extension: dropcaps-de-zs
backlink: False
...

This is the December 2018 edition of [the Gwern.net newsletter](https://gwern.substack.com/ "'Gwern.net newsletter (Substack subscription page)', Gwern 2013"); previous, [November](/newsletter/2018/11 "'November 2018 News', Gwern 2018") 2018 ([archives](/doc/newsletter/index)).
This is a collation of links and summary of major changes, overlapping with my [Changelog](/changelog); brought to you by my donors on [Patreon](https://www.patreon.com/cw/gwern).

# Writings

- ["Internet Search Tips"](/search): effective use of Google/Google Scholar/Libgen & other resources for papers/books/pages ([I'm also offering bounties for papers/books I haven't been able to get](/fulltext "Research Bounties On Fulltexts: A list of papers/books/materials I have failed to obtain, and financial bounties for anyone who can provide copies to me or the Internet."))
- ["Multi-Stage Selection Bean Machine Demo"](/doc/statistics/order/beanmachine-multistage/index.html "An interactive JS visualization of Galton quincunxes demonstrating the compounding power/ratchet effect of iterative selection rather than a single selection step.")
- ["Littlewood’s Law and the Global Media"](/littlewood "Selection effects in media become increasingly strong as populations and media increase, meaning that rare datapoints driven by unusual processes such as the mentally ill or hoaxers are increasingly unreliable as evidence of anything at all and must be ignored. At scale, anything that can happen will happen a small but nonzero times.")
- ["Evolution as Backstop for Reinforcement Learning"](/backstop "Speculation on links between Coase's theory of the firm, the socialist calculation debate, group selection, and meta reinforcement learning as two-layer optimization problems.")

# Media
## Links

**Genetics**:

- [Everything Is Heritable]{.smallcaps}:

    - ["Genomic Prediction of Complex Disease Risk"](https://www.biorxiv.org/content/10.1101/506600.full), Lello et al 2018 (followup to [Lello et al 2017](https://www.biorxiv.org/content/10.1101/190124.full "Accurate Genomic Prediction Of Human Height"): lasso for improved GWASes of many disease traits in UKBB)
    - ["Polygenic Risk Scores for Prediction of Breast Cancer and Breast Cancer Subtypes"](https://www.cell.com/ajhg/fulltext/S0002-9297\(18\)30405-1), Mavaddat et al 2018
    - ["Genetic variation in health insurance coverage"](/doc/genetics/heritable/2018-wehby.pdf), Wehby & Shane 2018
    - ["It Runs in the Family: A Study of Political Candidacy Among Swedish Adoptees"](https://link.springer.com/article/10.1007/s11109-017-9429-1), Oskarsson et al 2018
    - [23andMe GWAS on ice cream flavor preferences](https://blog.23andme.com/articles/genes-scream-for-ice-cream) (overlap with [chocolate preference GWAS](https://blog.23andme.com/articles/chocolate-its-complicated))
- [Engineering]{.smallcaps}:

    - ["China Shrinks From the _Gattaca_ Age"](https://www.bloomberg.com/news/articles/2018-12-05/china-fiercely-decries-he-jiankui-s-human-gene-editing "The government has fiercely decried a Shenzhen scientist's gene editing, in contrast to its push past ethical barriers in AI"); [He Jiankui under house arrest?](https://www.newsweek.com/gene-editing-chinese-scientist-he-jiankui-missing-house-arrest-1240749 "Chinese scientist He Jiankui goes missing, reported under house arrest") (see also: [_Wired_](https://www.wired.com/story/even-china-roundly-condemns-editing-the-genes-of-babies/), and previously, ["China's crackdown on genetics breaches could deter data sharing"](https://www.nature.com/articles/d41586-018-07222-2) & [highly fragmented competing datasets](https://www.nature.com/articles/534462a "China's bid to be a DNA superpower: First China conquered DNA sequencing. Now it wants to dominate precision medicine too")---why does China continue to abdicate genetics leadership?)
    - ["In vitro breeding: application of embryonic stem cells to animal production"](/doc/genetics/gametogenesis/2018-goszczynski.pdf), Goszczynski et al 2018 (a review of & proposal to start IES in cattle)
    - ["The Missing Response to Selection in the Wild"](https://www.cell.com/trends/ecology-evolution/abstract/S0169-5347(18)30039-9), Pujol et al 2018 (possible reasons for failure of the univariate breeder's equation in wild populations)
    - ["In vitro gametogenesis and reproductive cloning: Can we allow one while banning the other?"](/doc/genetics/gametogenesis/2018-segers.pdf), Segers et al 2018 (no)
    - ["The Fertility Clinic That Cut IVF Prices in Half"](https://www.wsj.com/articles/the-fertility-clinic-that-cut-ivf-prices-in-half-1536589667) (IVF doesn't have to be nearly as expensive as it is)
    - ["Rescue of American chestnut with extraspecific genes following its destruction by a naturalized pathogen"](https://link.springer.com/article/10.1007/s11056-016-9561-5), Steiner et al 2017 (success of GMO & cross-bred [blight](https://en.wikipedia.org/wiki/Chestnut_blight)-resistant [American chestnut trees](https://en.wikipedia.org/wiki/American_chestnut) expected in next 1--2 decades after [a century of work](https://en.wikipedia.org/wiki/American_chestnut#Attempts_at_revitalization))
    - ["Genetically Modified People Are Walking Among Us"](https://www.nytimes.com/2018/12/01/sunday-review/crispr-china-babies-gene-editing.html) (the [history of mitochondrial transfer](https://en.wikipedia.org/wiki/Mitochondrial_replacement_therapy#History))

**AI**:

- ["BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding"](https://www.reddit.com/r/MachineLearning/comments/a3ykzf/r_the_illustrated_bert_and_elmo_how_nlp_cracked/), Devlin et al 2018 ([blog](https://research.google/blog/open-sourcing-bert-state-of-the-art-pre-training-for-natural-language-processing/ "Open Sourcing BERT: State-of-the-Art Pre-training for Natural Language Processing"); ["The Illustrated BERT and ELMo (How NLP Cracked Transfer Learning)"](https://www.reddit.com/r/MachineLearning/comments/a3ykzf/r_the_illustrated_bert_and_elmo_how_nlp_cracked/))
- ["A general reinforcement learning algorithm that masters chess, shogi and Go through self-play"](/doc/reinforcement-learning/model/alphago/2018-silver.pdf#deepmind), Silver et al 2018 (expanded Alpha Zero paper; [link compilation](https://www.reddit.com/r/reinforcementlearning/comments/a3rbm3/alphazero_shedding_new_light_on_the_grand_games/))
- [Reward hacking examples in AI](/doc/reinforcement-learning/safe/2023-krakovna-specificationgamingexamplesinai-masterlist.html) (previously, [Lehman et al 2018](https://arxiv.org/abs/1803.03453 "The Surprising Creativity of Digital Evolution: A Collection of Anecdotes from the Evolutionary Computation and Artificial Life Research Communities"))
- ["Human-level performance in first-person multiplayer games with population-based deep reinforcement learning"](https://arxiv.org/abs/1807.01281#deepmind), Jaderberg et al 2018b ([blog](https://deepmind.google/discover/blog/capture-the-flag-the-emergence-of-complex-cooperative-agents/))
- ["StyleGAN: A Style-Based Generator Architecture for Generative Adversarial Networks"](https://arxiv.org/abs/1812.04948#nvidia "'A Style-Based Generator Architecture for Generative Adversarial Networks', Karras et al 2018"), Karras et al 2018 ([video](https://nvlabs.github.io/stylegan2/versions.html); [source](https://github.com/NVlabs/stylegan); ProGAN's successor: new style-transfer arch, more controllable, halves error for even more photorealistic faces with much-improved hair/eyes/backgrounds. ProGAN was already [hard for humans to distinguish](https://www.reddit.com/r/MachineLearning/comments/a8mpuc/p_results_identifying_real_vs_gangenerated_faces/) at much above chance levels, StyleGAN may knock it down to near-chance now. For kicks, compare StyleGAN's samples to the original [2014 GAN faces](https://arxiv.org/abs/1406.2661 "'Generative Adversarial Nets', Goodfellow et al 2014") 4 years ago.)
- ["MetaMimic: One-Shot High-Fidelity Imitation: Training Large-Scale Deep Nets with RL"](https://arxiv.org/abs/1810.05017#deepmind "'One-Shot High-Fidelity Imitation: Training Large-Scale Deep Nets with RL', Paine et al 2018"), Le Paine et al 2018
- ["An Introduction to Deep Reinforcement Learning"](https://arxiv.org/abs/1811.12560), Francois-Lavet et al 2018 (DQN, PG, exploration, benchmarking, POMDPs, transfer & meta-learning, multi-agent RL)
- ["Differentiable Image Parameterizations: A powerful, under-explored tool for neural network visualizations and art"](https://distill.pub/2018/differentiable-parameterizations/) (feature visualization & style transfer in 3D; an answer to the [VGG/style-transfer anomaly](https://www.reddit.com/r/MachineLearning/comments/7rrrk3/d_eat_your_vggtables_or_why_does_neural_style/)---[checkerboard](https://distill.pub/2016/deconv-checkerboard/) [artifacts](https://distill.pub/2017/feature-visualization/#frequency-artifacts)?)
- ["Go-Explore: a New Approach for Hard-Exploration Problems"](https://arxiv.org/abs/1901.10995#uber), Ecoffet et al 2019 ([preliminary announcement](https://www.uber.com/blog/go-explore/ "Montezuma's Revenge Solved by Go-Explore, a New Algorithm for Hard-Exploration Problems (Sets Records on Pitfall, Too)"); [discussion](https://www.reddit.com/r/MachineLearning/comments/a0nnp7/r_montezumas_revenge_solved_by_goexplore_a_new/))

**Statistics/Meta-Science**:

- [Clair Patterson's clean-room innovations, reducing bias in chemistry, led to his anti-lead crusade](https://www.mentalfloss.com/article/94569/clair-patterson-scientist-who-determined-age-earth-and-then-saved-it "'The Most Important Scientist You’ve Never Heard Of: For 60 years, American drivers unknowingly poisoned themselves by pumping leaded gasoline into their tanks. Here is the lifelong saga of Clair Patterson—a scientist who helped build the atomic bomb and discovered the true age of the Earth—and how he took on a billion-dollar industry to save humanity from itself.', Reilly 2017")
- ["A Rational Choice Framework for Collective Behavior"](https://people.csail.mit.edu/pkrafft/papers/krafft-thesis-final.pdf), Krafft 2017 (large noisy/mimicking human groups can approximate distributed Bayesian inference for [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling), as a kind of [particle filtering](https://en.wikipedia.org/wiki/Particle_filter))
- ["Exploration in the wild"](https://www.biorxiv.org/content/10.1101/492058.full), Schulz et al 2018 ([Deliveroo](https://en.wikipedia.org/wiki/Deliveroo) dataset: 1,613,967 meal orders 30,552 restaurants by 195,333 customers)
- ["Causal language and strength of inference in academic and media articles shared in social media (CLAIMS): A systematic review"](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0196346), Haber et al 2018
- ["Information Theory [its early history]"](https://web.mit.edu/6.933/www/Fall2001/Shannon2.pdf), Aftab et al 2001 (on psychology of innovation & computational constraints)
- ["The Unfavorable Economics of Measuring the Returns to Advertising"](/doc/economics/advertising/2015-lewis.pdf), Lewis & Rao 2015
- ["The war over supercooled water"](https://pubs.aip.org/physicstoday/Online/4628/The-war-over-supercooled-water)
- [Nash equilibria and strategies in 'Mafia'/'Werewolf' games](https://www.alexirpan.com/2015/08/25/perfectly-intelligent-mafia.html "Perfectly Intelligent Mafia")
- ["Poisson clumping"](https://en.wikipedia.org/wiki/Poisson_clumping) (why rare events can happen in 'clusters' even if there is no trend)
- [On the spinach/iron leprechaun](/scanners "'‘Scanners Live in Vain’ as realistic SF', Gwern 2013")

**Politics/religion**:

- ["The Mao Mango Cult of 1968 and the Rise of China's Working Class"](https://www.collectorsweekly.com/articles/the-mao-mango-cult-of-1968/); ["China's curious cult of the mango"](https://www.bbc.com/news/magazine-35461265)
- ["Axes of Evil": the DMZ tree murders](https://magazine.atavist.com/axes-of-evil-north-korea-dmz-tree-murders/)
- ["Things I Learned By Spending Five Thousand Years In An Alternate Universe"](https://slatestarcodex.com/2013/04/15/things-i-learned-by-spending-five-thousand-years-in-an-alternate-universe/) (online micro-nations)
- ["The Case For Cthulhu"](/doc/philosophy/religion/2012-rmp-thecaseforcthulhu.html), RMP

**Psychology/biology**:

- ["Academic achievement across the day: Evidence from randomized class schedules"](/doc/psychology/2018-williams-2.pdf), Williams & Shapiro 2018
- ["Clusters of Individual Experiences form a Continuum of Persistent Non-Symbolic Experiences in Adults"](https://nonsymbolic.org/PNSE-Article.pdf "'Clusters of Individual Experiences form a Continuum of Persistent Non-Symbolic Experiences [PNSE] in Adults', Martin 2013"), Martin 2013 (minimal and mostly negative effects of enlightenment-like experiences on cognition & daily life; see also ["Words"](https://radiolab.org/podcast/91725-words/transcript))
- ["Catnip, Valerian, Honeysuckle and other cat-attractant plants"](http://messybeast.com/catnip-valerian.htm), Hartwell 2008
- [OKCupid blog archives of online dating analyses](/doc/psychology/okcupid/index)
- ["Psychedelic Turk: A Platform for People on Altered States of Consciousness"](https://qualiacomputing.com/2018/10/28/psychedelic-turk-a-platform-for-people-on-altered-states-of-consciousness/) (truly diverse polling or worker groups)
- ["Night Vision: The forgotten theory of dreams that inspired Vladimir Nabokov"](https://newrepublic.com/article/146906/night-vision)
- ["Treasure Hunting"](https://www.ribbonfarm.com/2018/11/02/treasure-hunting/)
- ["Locomotion is not a privilege after birth: Ultrasound images of viviparous shark embryos swimming from one uterus to the other"](/doc/biology/2018-tomita.pdf), Tomita et al 2018
- ["Human Tails: Ownership and Control of Extended Humanoid Avatars in VR"](https://vr.cs.ucl.ac.uk/portfolio-item/human-tails/), Steptoe et al 2013
- ["Sex, Steroids, And Arnold: The Story Of The Gym That Shaped America"](https://deadspin.com/sex-steroids-and-arnold-the-gym-that-shaped-america-1828228786/)

**Technology**:

- ["Augmenting Human Intellect: A Conceptual Framework"](https://www.dougengelbart.org/pubs/papers/scanned/Doug_Engelbart-AugmentingHumanIntellect.pdf#page=82 "‘Augmenting Human Intellect: A Conceptual Framework § pg82’, Engelbart 1962 (page 82)"), [Engelbart](https://en.wikipedia.org/wiki/Douglas_Engelbart) 1962 (background for [The Mother of All Demos](https://en.wikipedia.org/wiki/The_Mother_of_All_Demos))
- [David Denkenberger interview on "ALLFED"](https://80000hours.org/podcast/episodes/david-denkenberger-allfed-and-feeding-everyone-no-matter-what/) (research into radical strategies for food production in case of global agricultural disaster to prevent mass starvation)
- ["11 Forgotten Women who Invented the British Industrial Revolution"](https://medium.com/@antonhowes/11-forgotten-women-who-invented-the-british-industrial-revolution-ea34e185f187)
- ["The Instant Pot of the 1600s Was Known as 'the Digester of Bones': The pressure cooker led directly to the invention of the steam engine"](https://www.atlasobscura.com/articles/who-invented-the-instant-pot)
- ["DRAM's Damning Defects---and How They Cripple Computers: An investigation into dynamic random-access memory chip failure reveals surprising hardware vulnerabilities"](https://spectrum.ieee.org/drams-damning-defects-and-how-they-cripple-computers)
- ["A Breakthrough for U.S. Troops: Combat-Ready Pizza"](https://www.nytimes.com/2018/09/20/us/army-pizza-mre-field-rations.html)
- ["Before Envelopes, People Protected Messages With 'Letterlocking': For centuries, senders used folds, slits, and wax seals to guard correspondence from prying eyes"](https://www.atlasobscura.com/articles/what-did-people-do-before-envelopes-letterlocking)
- [Some Google search examples](https://www.lesswrong.com/postsRspqaNmJKKBnXTqwk/open-thread-april-1-15-2013#comment-sWpsaRHPG4rtc8mxt)

**Economics**:

- ["Uber insiders describe infighting and questionable decisions before its self-driving car killed a pedestrian"](https://www.businessinsider.com/sources-describe-questionable-decisions-and-dysfunction-inside-ubers-self-driving-unit-before-one-of-its-cars-killed-a-pedestrian-2018-10) (_Challenger_ redux)
- ["Prime & Punishment: Dirty dealing in the \$175 billion Amazon Marketplace"](https://www.theverge.com/2018/12/19/18140799/amazon-marketplace-scams-seller-court-appeal-reinstatement) (the tactics used in seller wars)
- Deku-shrub & Besa Mafia: ["The unbelievable tale of a fake hitman, a kill list, a darknet vigilante... and a murder"](https://www.wired.com/story/kill-list-dark-web-hitmen/)
- ["Demon Underneath: John DeLorean and the invention of the future"](https://theoutline.com/post/6776/john-delorean-rise-and-fall-big-money-the-fbi-celebrities-and-cocaine) (on [John DeLorean](https://en.wikipedia.org/wiki/John_DeLorean); the thin line between automobile conman & visionary, and hustling & felonies---Elon Musk, if he were much more incompetent & unlucky)
- ["Just Desserts: Sandy Jenkins was a shy, daydreaming accountant at the Collin Street Bakery, the world's most famous fruitcake company. He was tired of feeling invisible, So he started stealing---and got a little carried away."](https://www.texasmonthly.com/true-crime/just-desserts/) (remarkably shabby & unimaginative ways of spending the bezzle)
- ["The fixed 5-cent price of Coca-Cola 1886–1959"](https://en.wikipedia.org/wiki/Fixed_price_of_Coca-Cola_from_1886_to_1959)

<!-- <strong>Philosophy</strong>:

- [Santayana on philosophy in academia](https://www.reddit.com/r/gwern/comments/a6vjq1/santayana_on_philosophy_in_academia_and_perhaps/); [Motoori Norinaga on the purpose of poetry](https://www.reddit.com/r/gwern/comments/a8v5az/motoori_norinaga_on_the_purpose_of_poetry/)
- ["Vectors 2.0: More Aphorisms and Ten-Second Essays"](https://www.reddit.com/r/gwern/comments/a5ojab/vectors_20_more_aphorisms_and_tensecond_essays/), Richardson 2004
-->

**Fiction**:

- ["The Cowboy Hávamál"](https://jacksonwcrawford.com/the-cowboy-havamal/), Crawford 2012
- ["'Tattúínárdǿla saga': If _Star Wars_ Were an Icelandic Saga"](https://tattuinardoelasaga.wordpress.com/2010/03/01/tattuinardoela-saga-if-star-wars-were-an-icelandic-saga/), Crawford 2010
- ["The Tatooine Cycle"](https://headofdonnbo.wordpress.com/2015/12/10/the-tatooine-cycle/), Donn Bó 2015 (if _Star Wars_ were an Irish epic)

**Misc**:

- ["The World in Its Extreme: It is the hottest place in the world, and the driest. It is home to thriving commerce and to desperate, hopeless poverty. It is the Sahara, an eternal source of fascination and terror"](https://www.theatlantic.com/past/docs/unbound/langew/extreme.htm):

    > After the radiator coolant was gone, the Belgians started sipping gasoline. You would too. Call it _petroposia_: Saharans have recommended it to me as a way of staying off battery acid. The woman wrote that it seemed to help. They drank their urine. She reported that it was difficult at first, but that afterward it wasn't so bad.
    >
    > The boy was the weakest, and was suffering terribly. In desperation they burned their car, hoping someone would see the smoke. No one did. They killed their son to stop his pain. Later the husband cut himself and the wife drank his blood. At his request she somehow broke his neck with a rock. Alone she no longer wanted to live. Still, the [Sahara](!W) was fabulous, she wrote, and she was glad to have come. She would do it again. She regretted only one thing---that she had not seen [Sylvester Stallone](!W) in [_Rambo III_](!W). Those were her last lines. The family's remains were found later, and returned to Tamanrasset.
- ["Please Take The 2019 SSC Survey"](https://slatestarcodex.com/2018/12/26/please-take-the-2019-ssc-survey/)

<!-- Back to 20? August 2012 -->

## Books

**Nonfiction**:

- [_Vectors: Aphorisms & Ten-Second Essays_](https://www.amazon.com/Vectors-Aphorisms-Ten-Second-James-Richardson/dp/0967266890), Richardson 2001 (see also [Richardson 2010](/doc/philosophy/2010-richardson-bythenumbers-vectors30 "Even More Aphorisms and Ten-Second Essays from Vectors 3.0"))
- [_The Midas Paradox_](https://www.amazon.com/Midas-Paradox-Financial-Government-Depression/dp/1598131508), [Scott Sumner](https://en.wikipedia.org/wiki/Scott_Sumner) ([review](/review/book#the-midas-paradox-sumner-2015))

**Fiction**:

- [_An Artist of the Floating World_](https://en.wikipedia.org/wiki/An_Artist_of_the_Floating_World), [Kazuo Ishiguro](https://en.wikipedia.org/wiki/Kazuo_Ishiguro) 1986 (strikingly like [_The Remains of the Day_](https://en.wikipedia.org/wiki/The_Remains_of_the_Day), and successful for similar reasons)
- [_The Buried Giant_](https://en.wikipedia.org/wiki/The_Buried_Giant), Ishiguro 2015 ([review](/review/book#the-buried-giant-ishiguro-2015))
- [_Nocturnes: Five Stories of Music and Nightfall_](https://en.wikipedia.org/wiki/Nocturnes_\(short_story_collection\)), Ishiguro (inoffensively meandering short stories about musicians and failing or succeeding)
- _The Alehouse at the End of the World_, Allred 2018 (peeked into as part of Christmas book exchange; dropped after 50 pages & skipping forward to check. Painfully unfunny and then descends into dirty-old-man skeevishness where he writes endlessly about sex in a manner nauseating enough to make one want to take a vow of celibacy. I'll stick with Neil Gaiman, thanks.)

## Film/TV

**Live-action**:

- <div id="they-shall-not-grow-old">[_They Shall Not Grow Old_](https://en.wikipedia.org/wiki/They_Shall_Not_Grow_Old), 2018 ([review](/review/movie#they-shall-not-grow-old))</div>

**Animated**:

- [_Spider-Man: Into the Spider-Verse_](https://en.wikipedia.org/wiki/Spider-Man:_Into_the_Spider-Verse) ([review](/review/anime#spider-man-into-the-spider-verse))
- [_Coco_](https://en.wikipedia.org/wiki/Coco_\(2017_film\)) ([review](/review/anime#coco))

## Music

**MLP**:

- ["Wandering (Sweet Apple Acres)"](https://www.youtube.com/watch?v=2kIH1aRW4DQ) (Equestria Electric Symphony Orchestra {2018}) [country]
- ["Winter Wrap Up (Symphonic Metal Cover)"](https://www.youtube.com/watch?v=llh775Qz-x4) (Rockin'Brony {2018}) [instrumental rock]
- ["Alone"](https://www.youtube.com/watch?v=u-ZCzGfxOVc) (Tre; _Scraps_ {2017}) [pop]
- ["Your Kindness"](https://www.youtube.com/watch?v=z3vhme5Z7AQ) (ertrii {2018}) [house]
- ["Another Way"](https://www.youtube.com/watch?v=BzWmZvYDbyE) (Lorris; [_Lollipop_](https://astateofsugar.bandcamp.com/album/doughnutalbum/lollipop) {2017}) [electronic/synthwave]
- ["Changes"](https://www.youtube.com/watch?v=rkEMytW5bG0) (StealingShad3z feat. GhostXb; [_Recollections_](https://poniesatdawn.bandcamp.com/album/recollections) {2018}) [Alt. Rock]
- ["Step! Buck! Leap! Touch!"](https://www.youtube.com/watch?v=UT4QB20OsW8) (Mane in Green; _Recollections_ {2018}) [folk/Celtic]
- ["Train Bells"](https://www.youtube.com/watch?v=BagSbopRDw8) (Wandering Artist; _Recollections_ {2018}) [orchestral]
- ["Not Much to Miss"](https://www.youtube.com/watch?v=sGD6JaKoVAY) (4EverfreeBrony feat. EileMonty; _Recollections_ {2018}) [pop]
- ["Spearmint"](https://www.youtube.com/watch?v=zUM3XJQnIEs) (Viricide Filly; _Lollipop_ {2017}) [electronic]
- ["Giggles & Gumdrops"](https://www.youtube.com/watch?v=52cuuWHNWIU) (4everfreebrony; [_Small Horses and Other Stuff_](https://garrisonulrich.bandcamp.com/album/small-horses-and-other-stuff)) [acoustic]
