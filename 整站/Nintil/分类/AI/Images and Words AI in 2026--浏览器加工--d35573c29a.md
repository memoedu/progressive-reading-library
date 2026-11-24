In a previous [Links](https://nintil.com/links-61) post, and in a recent tweet I expressed [my relative lack of excitement about](https://twitter.com/ArtirKel/status/1581310274539167746) what a lot of people are doing with what I called "the AI stuff" (narrowly, large language models and diffusion models, collectively "generative AI"; excluding e.g. Tesla's FSD or AlphaFold). In an even earlier [tweet](https://twitter.com/ArtirKel/status/1544858276503949312), I asked Twitter if we had learned anything new from LLMs yet, as opposed to LLMs telling us what we (the internet) already knew; the conclusion being that we have not. Nostalgebraist has been writing on the same lines for a [while](https://nostalgebraist.tumblr.com/post/698045223162593280/taking-a-break-from-ai-discourse).

\[1\]. Throughout this essay I will use GPT3 to refer to the latest available text-davinci-002 model from OpenAI as of writing this essay

Recent AI hype contrasts with the fact that GPT3[1](https://nintil.com/interesting-ai-models/#sidenote-1) was first released to the public in June of 2020 and since then, not that much has been done with it, with the recent exception of its descendent model Codex. I do not think this is because of insufficient tinkering with the models, but rather they have to do with intrinsic limitations of the models that are currently available and that I expect will be available in the foreseeable future.

Up until recently the most popular use case seems to have been marketing copywriting. I don't find this particularly exciting, search on the internet is already full of bland content marketing. It's different if one does content marketing by, say, writing an article about how to use your API, or if a company does a toy project using their API and then writes about it. The hard work here is in the project or thinking about the use case in a thoughtful way, and less so in the writing itself. Admittedly I do not know much about content marketing though!

I find code generation (Copilot, Ghostwriter) more exciting. Copilot can be genuinely helpful and by some accounts it's bringing hundreds of millions of dollars to [Github](https://www.theregister.com/2022/11/11/githubs_copilot_opinion/).

Then there's of course image (and video!) generation. The systems I'm aware of and that I have used are DALLE2, Stable Diffusion, and Midjourney. These are fun to play with and I predict they will be used in a number of creative tasks from generating images for posts to assets for videogames to movie styling. RunwayML but also Stability.ai seem to be the leaders here. A recent concrete example I saw in the wild was generating the header of [this](https://twitter.com/ansonyuu/status/1589035115526508545) interview with Michael Nielsen by generating a plausible image completion around a smaller image.

Looking back, four years ago the state of the art in image generation was generating faces and numbers. There were no scaling laws papers and the first [GPT](https://openai.com/blog/language-unsupervised/) paper had just been released. Rapid progress in some domains in the last years have led to some to think that not only progress in these domains will continue as fast or faster, but that we are on the verge of full-blown artificial general intelligence.

I don't see it that way: Progress has indeed been fast lately. In a few years I will not be writing a snarky remark about GPT-N not being used for much because by then it will be obvious that there are at least a handful of legitimately useful applications for LLMs in broad use. At the same time, in a few years I do expect some people that today are really excited will feel disillusioned: we'll get close, but we'll remain far, as happened with self-driving cars: they seemed so close many years ago!

It's easy (and cheap) to theorize about what will or won't happen by some unspecified date in the future. A much harder thing to do is to commit to specific predictions or bets of the form "By 2026, AI won't be able to do X". What should those X be? This essay is an attempt to sort through my thoughts and come up with those bets.

One lens this essay is written through is what applications of AI would _I_ find useful. I don't do video editing, am not an artist, and have lots of knowledge and means to find it and index it. The lack of excitement may come from the fact that _I_ want to have models helping me just like they can help others do their job. The topics I've decided to focus on here are guided (or biased by) those considerations.

A second lens is forecasting progress towards general artificial intelligence. In a previous post I discussed how AI systems could [pose](https://nintil.com/ai-safety) danger, but I did not discuss when those systems will actually be built. The present essay is an effort to think about AI capabilities timelines in the short-to-medium term. This lens led to the sections on "true understanding", compositionality, and remarks on progress in narrow-purpose models and general-purpose models.

This is important to keep in mind because if not this essay will seem to be harshly dismissing genuine progress in the field.

Throughout the essay I also suggest research directions and ways to improve the models. At the end of the essay, in an [appendix](https://nintil.com/interesting-ai-models/#appendix-ideas-for-ai-startups), I suggest some startup ideas in this domain.

## Artifacts in video generation

Will we get a Hollywood-tier blockbuster movie within the next 5 years? Video generation is still in what I'd describe as its infancy. Microsoft's [NUWA](https://nuwa-infinity.microsoft.com/#/), Google's [Imagen](https://imagen.research.google/video/) Video, [Phenaki](https://phenaki.github.io/), or Meta's [Makeavideo](https://makeavideo.studio/) are impressive as technological achievements and without doubt we will see better models in years to come. The parameter counts in the models are not particularly high compared to say PaLM, and the training data Imagen uses does not include everything Google has access to (They could eventually train on a large dataset of youtube videos).

However! One can still see some peculiar weird artifacts in most of these videos, in this particular video from Google Imagen Video or the one below that one, from Phenaki. Note how some features (like the eyes of the bear or the sails of the ship) pop in and out of existence, or wobble around:

 Your browser does not support the video tag.

![](https://pub-bede3007802c4858abc6f742f405d4ef.r2.dev/stories/teddy_bear_super_res.webp)

Why this wobbling? My explanation is that the model doesn't have conceptual understanding, a claim some readers will nod to enthusiastically and others will predictably think it's either wrong or true but irrelevant, so more on this later; I of course hold a [third more complex](https://twitter.com/ch402/status/1516427465266245635) opinion. The model is trying to generate video that looks coherent in a similar way the videos in the training set look coherent, and doing so conditional on a prompt. In contrast, you could imagine a model that instead generates a 3D model of a bear and a texture (which stays constant throughout the video), then generates a set of animations, then animates the bear through the water. This is what a human would do at an animation studio and plausibly this could too be done with AI (There are [models](https://nv-tlabs.github.io/GET3D/) to generate 3D models out of images already, as well as models to generate [animations](https://guytevet.github.io/mdm-page/) from prompts). This is however contrary to the ethos of the modern approach to AI: Eschew purpose-specific solutions, seek end to end general solutions by training large models on large corpora of the desired kind of final output data (movies).

To be sure, this approach has been paying off well for Tesla and FSD, who started with many purpose-specific systems and ended with progressively more end-to-end systems, but this is [taking longer](https://twitter.com/TaylorOgan/status/1476266341443293186/photo/1) than expected, and even today we are not a a level where a driver can stop paying attention. It doesn't matter if 95% of the time the model is reasonable, if it can do something unsafe in that 5% of the time a driver has to remain alert.

There are not many examples of videos fully generated with AI yet, but there are many images. Many or even most of these images look satisfactorily pixel perfect. Have a look at this gallery in [Lexica](https://lexica.art/) for some examples. But sometimes you find things like [this](https://lexica.art/?prompt=f9881f0e-66cf-499d-88ec-d161c638556c) where the arms look all off. Generating one-off images, especially when one can try multiple times is one thing. Generating a movie is another. A two minute clip (at 24 fps) is a sequence of 2880 pictures. They all have to be accurate and coherent with the previous ones. A two hour movie is over 172k individual frames.

The [Phenaki](https://phenaki.github.io/) demo website does have examples of videos longer than two minutes. I can easily see how voice and a longer script could be added to this, and we'd have a passable movie, but not a blockbuster. Fine, one may say. It's not a Hollywood competitor _now_, but what about in 2026? Remember, we had _no_ AI generated video just a few years ago, then we got to [these](https://www.youtube.com/watch?v=bKFgjCl1dTo) kind of videos by interpolating images, and now we have better spatiotemporal coherence with the new video models. Isn't it easy to imagine that in 2026, after all the investment the field has seen and will continue to see, they will be able to output 172k consecutive individual frames of pixel-perfect Hollywood-level quality? For the probability of a sequence of randomly sampled 172k frames to be all coherent to be >90%, the probability that each frame is good must be over 0.999999. It's hard to cash this out in precise benchmarks; where are current models at? And how do we account for the fact that frames are not really sampled randomly? But my intuition from looking at images generated with these models says they are far from there; and likewise my intuition is that going from mediocre to good is substantially easier than going from good to great. One could allow for a handful of less than perfect frames, and for multiple shots at the task and get away with less reliable models.

A given kind of person sees models from five years ago with just faces, models now, and draws a line from A to B and extrapolates that they will keep improving. If you are me you see improvements in some dimensions but not others (Obsessing over subcomponents of narratives is a [Nintil](https://nintil.com/still-no-great-stagnation/) house move), and so you predict continued improvements in some dimensions, and less so in the others that haven't improved or have improved at a slower rate.

What _will_ improve? Quality of textures for sure, Midjourney V4, released as I am writing this essay is already one leap in this direction. Spatiotemporal coherence will improve a bit. Both together will improve at a slower pace.

I expect producing an anime movie is easier because textures are simpler but it will still be challenging. See this example [clip](https://www.youtube.com/watch?v=jIFY8NUn8I4). Textures are simpler, but there's still keeping track of where the characters are in the scene, where the camera is looking at, when to do closeups or wider shots, syncing voice, music, and video.

I just seem to have a strong prior that without a a better understanding of objects as entities over space-time (as opposed to something closer to just pixels on a screen) we are not going to get there by 2026 (to fully automate movie generation).

## Image generation and compositionality

Image AI systems available today seem to struggle with compositionality: this is being able to lay out concrete objects in defined spatial relationships according to a prompt. This breaks down in scenes where there are varied objects that have to have specific spatial relations. One representative example of this sort of problem, with a prompt I just made up is here (DALLE2):

![image-20221020120056813](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221020120056813.png)

A table with a computer monitor in the center. To the right, a soft green round speaker. To the left, a laptop on a stand. To the left of the laptop there is an espresso machine

None of these pictures meets the requirements of the prompt (One has a _red_ speaker, they all lack the espresso machine, three don't have the computer monitor, one doesn't have a laptop etc). These mistakes are very trivial to see for a human being that has been asked to produce the image from the prompt.

Stable Diffusion v1.5 did better than DALLE, and below Midjourney V4 did better than SD, but still none of the images capture what I wanted

![image-20221020120537996](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221020120537996.png)

![image-20221020120549249](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221020120549249.png)

![Image](https://nintil.com/images/2022-10-19-interesting-ai-models/progress_image_100_04473f3e-0065-435c-b54f-cb18b34eb900.webp)

Here are some other fun examples:

-   Elon [Musk](https://lexica.art/prompt/95edb923-48aa-4e78-aa81-5f08da091d87) with a coconut instead of a head
-   A [flower](https://lexica.art/prompt/c43bb8f6-679a-43a1-894f-03a65ca0ff81) pot on top of Ironman's head
-   The [girl](https://lexica.art/prompt/6d4df1f6-b5ba-47cc-8a74-cabeec3c5e17) is holding a miniature turtle in her hands, there is an elephant riding on the turtle
-   A [teddy](https://lexica.art/prompt/2508541b-af04-4b9a-9a6e-bdda85e60e29) bear holding a cup of tea with one hand and an iPad with the other hand
-   [Ironman](https://lexica.art/prompt/a829c03f-7e56-40f3-a1a8-b833da3e3dd0) fighting the hulk, an eagle is nested on the hulk's shoulder

The issue here is not whether I am a good prompt engineer (It's my first try at this particular scene). I am sure one can get better results by playing with the model. The issue is not one of comparing relative performance at generating some output (These models can produce outputs that I, without extensive training, cannot produce).

The issue is that the models are far from really understanding what they are outputting to the same level a human would. This happens (maybe?) because AI models for vision seem to like to think in terms of textures (not that they only do this, but they do it more than we do). If a given area has enough of the right texture (or color) then the image looks too okay to the model. This explains why there are a few green bits in the imaged that I didn't ask for. We might be overrating what these models actually do because often we see the (really good) end-products of prompt engineering and example picking (unless one has spent sufficient time playing with the models).

Ok but if one had asked me, five years ago, how likely is it that you'll see the images I just posted, what would I have said (after doing a brief literature review)? Five years ago (2017) there was work on generating [small](https://machinelearning.apple.com/research/gan) synthetic images that looked okay. These [faces](https://developer.nvidia.com/blog/photo-editing-generative-adversarial-networks-2/) are probably the most detailed images we had back then. In 2015 there was at least [one](https://arxiv.org/abs/1511.02793) paper that showed it was possible to generate very small and blurry images of scenes with multiple objects from prompts. Since then, now we have seen better textures, a greater variety of objects being depicted, and higher resolution. I'm naturally drawn to assume that these will keep getting better. But also, there has been little progress in getting the images to correspond robustly, and repeatably to what models are being asked to produce. This, I then think, is also tied to the blurry artifacts and weird images we see occasionally. Larger models with the same architecture will struggle in this exact way.

Could this be fixed in the training set? There are not as many scenes with multiple objects in the training sets, whereas there are many depictions of individual objects. In principle one could generate more scenes if we have the underlying objects. Perhaps we can ask the model to generate a tea cup and an expresso machine, then copy them side by side, do some painting over with another model call, and use that resulting image as a scene, then build a corpus of such scenes to improve the model's understanding of compositionality. Doing this seems well within what's currently possible.

There is precedent for a big compositionality problem being solved: text. If you search for images with text in [Lexica](https://lexica.art/?q=text) you'll find that each letter makes sense but the whole looks like mangled garbage. Not so with Google [Imagen](https://imagen.research.google/), that is able to reproduce, at least sometimes, well ordered text, introducing a benchmark for compositionality (DrawBench). Figure A.18 shows one clear example of a prompt of the kind that DALLE struggles with but Imagen is able to successfully depict. Contrary to "scale is all you need" memes, this advance was the result of a careful investigation of prompt-guidance and a novel thresholding technique (Section 2.3). Drawbench includes some examples drawn from this paper from Gary Marcus et al. ([2022](https://arxiv.org/pdf/2204.13807.pdf)) where clearly DALLE2 struggles, but those are not shown in the examples showcased in the Imagen paper nor is Imagen publicly available yet so I was not able to poke at the model for this essay. Because of the Imagen results I predict that image generation with well-ordered text will be a solved problem by 2026.

Another recent paper ([Liu](https://arxiv.org/pdf/2206.01714.pdf) et al., 2022 "Composable Diffusion") gives up having a single model to generate entire images and breaks down queries into submodels which can then be aggregated back again, while at the same time they include training sets that specifically include object relations as I suggested above. One of the example prompts they have is `“A green tree swaying in the wind” AND “A red brick house located behind a tree” AND “A healthy lawn in front of the house”`. Stable [diffusion](https://lexica.art/prompt/ed29d658-f857-44ab-af90-0f8e48e02810) does not struggle with this one at all, perhaps because lawns with trees and houses behind are not unusual. In contrast, an artificial prompt like `“A large blue metal cube to the left of a small yellow metal sphere” AND “A large blue metal cube in front of a large cyan metal cylinder”` leads to [this](https://lexica.art/prompt/69a96324-6921-490f-a859-4a49b2212335) sort of thing which the Liu paper does better at. Even after seeing this result, I am still convinced that training sets with more complex scenes will be required for models to get composition in a robust way.

## Essay generation with GPT3: an analysis

In the [appendix](https://nintil.com/interesting-ai-models/#appendix-blog-post-generation-with-gpt3) I took a number of essays/blogposts/articles, copied a chunk of them to GPT3 and got continuations, trying about 3 times with each and picking what I thought was the best. I then compared that with the original text. In all cases, I would have preferred to read the original text.

Note that this is different from whether the GPT3-generated text is humanlike (which it often is) and correct (which is also roughly is). But when we read something we usually don't want to be told what we already know: we want to be exposed to novelty and GPT3 does a mediocre job at this.

You can head to the appendix, read the text there and then come back to this section.

* * *

A summary of my analysis of this brief blogpost completion exercise is that the GPT3 completions have less detail than the interesting blogposts. In a way, this is reminiscent of early image generation models where the textures looked very soft (Newer models, particularly Midjourney V4 are now able of close to photorealism). The system Elicit uses ([Primer](https://primer.ought.org/)?) which breaks down questions manually and allows for search produces better results. For example for the prompt "Does rapamycin extend lifespan in mice?" Gets us from GPT3

> There is some evidence that rapamycin extends lifespan in mice, but this is not definitive.

Whereas Elicit would say something much better (and with working links):

> All four of these papers suggest that rapamycin extends lifespan in mice. Harrison 2009 found that rapamycin extends lifespan in genetically heterogeneous mice. Anisimov 2010 found that rapamycin prevents age-related weight gain, decreases rate of aging, increases lifespan, and suppresses carcinogenesis in transgenic HER-2/neu cancer-prone mice. Anisimov 2011 found that rapamycin extends lifespan in female 129/Sv mice. Comas 2012 found that a new formulation of rapamycin extends lifespan in homozygous p53−/− mice by delaying carcinogenesis. Thus, these papers all suggest that rapamycin may extend lifespan in mice.

I myself would have written something like:

> Yes. Rapamycin is probably the most studied drug to extend lifespan. It works across the entire range of model organisms. In mice in particular the strongest evidence is probably that collected by NIA's Interventions Testing Program (Harrison et al. 2009) where they showed increases in average and maximum lifespan in both males and females. Labs have tested multiple doses of rapamycin up to 42ppm and at least up to that level there is a dose-dependent increase in lifespan (Kaeberlein, 2014) though given what we know about its mechanism of action we should expect this to break down at some point at even higher doses.

My answer has less detail than Elicit (Elicit cites more papers) but arguably mine is better because it gives the right level of detail: The ITP paper is strong enough to base most of the answer on it, and my answer does not confuse someone that is not aware of there being different strains of mice, how translatable research is, or how much credibility to assign to the ITP vs other efforts to measure lifespan. My answer also has the benefit of coming from someone that has written a [Longevity FAQ](https://nintil.com/longevity) and Nintil in general: If you trust my rigor in general you may also trust me in this particular case.

GPT3 (and future models) face a problem similar to the tools for thought domain ([thread](https://twitter.com/ArtirKel/status/1446181901283168256)). For a newbie in a given domain, GPT3 has knowledge they don't have; but they may not know what questions to ask or what the answer even means in its full richness, or whether to even trust it, and their needs may be better served by simpler approaches like a Google search. For the expert, they already know the domain, so while they can judge GPT3's answers they have no use for them because they already know them.

This will change in the near future: Just today (2022-11-08) a paper from Anthropic came out trying to use an LLM-powered assistant to [help](https://twitter.com/AnthropicAI/status/1590019597109202946) newbies with a benchmark task. Plausibly an extrapolation of this ("raising the floor") is models fine-tuned by domain experts and deemed accurate by them, so then newcomers can trust them. We could have models fine-tuned with the help of doctors that are really good at telling patients what their symptoms mean. On the other hand, the generation of novelty ("raising the ceiling") seems harder by virtue of how these models are trained (to predict the next most likely token).

How might this latter, raising the ceiling, be accomplished? Obviously we don't want unhinged text, or models trying to predict the least likely token, we want something that maximizes some "interestingness" metric as opposed to a "likelihood" metric. Likelihood as usually used in ML training is a property of a token given a dataset whereas interestingness is more of a social construct that depends on who is reading the text and when has the text been written. Talking about say general relativity if asked about weird anomalies in the orbit of Mercury is not as interesting now as it was before Einstein was born. So one way to perhaps get models to learn interestingness is to take examples of texts considered interesting at the time, and find a way to finetune models to produce them when fed text produced temporally prior to the interesting text. This is at the time very hard to do given that all this text wouldn't fit in the context window, and this sort of training would require having reliable timestamps for the training datasets.

Ultimately there's a function that goes from knowledge today to knowledge tomorrow and we wish to learn that function. Right now the most promising approaches to get more interestingness it seems to train the model on datasets that represent blogposts and scientific papers more often, and then finetune based on prompts.

I look forward to the time when models can produce special relativity or CRISPR when trained on datasets that do not include mention of those! In the meantime, we might get individual assistants that constantly learn from each individual's preferences and knowledge.

## Common sense in Machine Learning

Common sense reasoning used to be one of the holy grails of AI, perhaps after [this](http://jmc.stanford.edu/articles/mcc59/mcc59.pdf) John McCarthy paper from 1959. One day we woke up and a solution unexpectedly popped into existence: Large Language Models. Or so it seems. If one doesn't buy this _yet_ then one can imagine slightly better systems that will surely come in the future. But after having played a lot with GPT3, I can say that for many questions, if that knowledge is public information on the internet, GPT-3 can answer them relatively competently. Some examples below, including some examples that many humans would not know the answer to!

| Prompt | GPT3 answer |
| --- | --- |
| What is the capital of Spain? | Madrid |
| How much is (2+2)\*3? | 12 |
| What is something you can make with flour, water, salt, and yeast? | Bread |
| Ian was tired of trying new diets while Steven had luck with one, so \_ decided to do their own thing. | Ian decided to do their own thing. |
| Where is a well used toy car likely to be found? | The most likely place to find a well used toy car is in a child's bedroom. |
| Explain the joke "What do you call a polar bear without ears? Polar B.". Why Polar B.? | The joke is a play on words. "Polar B" sounds like "Polar Bear", but without the "ear" sound at the end. |
| Does BACE1 cut preferentially at the beta or beta' site in humans? Where does BACE1 cut preferentially in mice? | BACE1 cuts preferentially at the beta site in humans and at the beta' site in mice. |
| Prime numbers are numbers that can only be evenly divided by itself and 1. 1, 2, 3, 5, 7 are prime numbers. Is 71 a prime number? | Yes, 71 is a prime number. |
| I have two vectors in numpy, x and y. Write the python code that produces a matplotlib plot that looks nice where the lines are red. Use seaborn for styling. | x = np.array(\[1, 2, 3, 4, 5, 6, 7, 8, 9, 10\])  
y = np.array(\[2, 4, 6, 8, 10, 12, 14, 16, 18, 20\])
import matplotlib.pyplot as plt  
import seaborn as sns  
sns.set\_style("darkgrid")  
plt.plot(x, y, color='red')  
plt.show()

 |
| What do the words alfombra, admiral, apricot, and alcazar have in common? | All four words are derived from Arabic. |
| Why there used to be lead in gasoline? Explain for a general audience. | The main reason why lead was used in gasoline was because it acted as a "knock inhibitor." This means that it helped to prevent engine knocking, which is a loud knocking noise that can occur when the air-fuel mixture in the cylinders is detonating too early. Lead is no longer used in gasoline because it is a poisonous metal that can damage the nervous system, the brain, and the kidneys. It can also cause anemia and high blood pressure. |
| What is a solution for the equation x^n+y^n=z^n? Assume n=2. | A solution for the equation x^2+y^2=z^2 is x=3, y=4, z=5. |
| What's the name of a salad that shares a name with a historical character? | Caesar Salad |
| Who are three main characters in The Lord of the Rings? | Frodo, Sam, and Gollum |
| Here are some interesting questions to ask candidates during job interviews: How did you prepare for this interview? If this company fails, why would that be? What question should I ask you? Generate 5 more other similar questions. | 1\. What made you decide to apply for this job? 2. What do you think makes you a good fit for this job? 3. What do you think are our company’s strengths? 4. What do you think are our company’s weaknesses? 5. How do you think you could contribute to our company’s success? |
| A man wants to stick to a diet but has so far failed to. What are some tips you would give him? | There are many tips that can help someone stick to a diet, but some key tips include: 1. Find a diet plan that fits your lifestyle and stick to it. 2. Set realistic goals for yourself and don't try to lose too much weight too quickly. 3. Make sure you're eating healthy foods that you enjoy and that you're getting enough nutrients. 4. Avoid processed foods, sugary drinks, and excessive amounts of saturated and unhealthy fats. 5. Exercise regularly to help boost your metabolism and burn more calories. 6. Stay motivated by setting small goals and rewards for yourself along the way. |
| Here is a bet that was offered to me. A fair coin will be flipped. If it comes out heads I will be paid $100. If it is tails, I will have to pay $50. Should I take this bet? Explain your reasoning. How much money on expectation will I make? | The expected value of this bet is $25, so it is a good bet. |
| Here are some animals: zebra, panda, skunk. What is the most interesting commonality between these animals? Write one example of an animal that is similar to these in that exact same way. | The most interesting commonality between these animals is that they are all black and white. An example of an animal that is similar to these in that exact same way is a raccoon. |
| Translate "This is a really good translation system! I can't believe it's just GPT3" into Castilian Spanish | ¡Este es un sistema de traducción realmente bueno! ¡No puedo creer que solo sea GPT3! |

One can construct prompts where GPT3 fails if one is tries hard enough to find edge cases. It may be possible that sometimes GPT3 gets these right, or that future models will get these right, or that GPT3 itself given a few examples would get these right as well. The point is not so much how capable GPT3 is or isn't but rather that despite being able to give correct answers in the cases earlier, it still does so without understanding everything to the same level a human would. By extrapolation, future systems will be more impressive, but might still feature silly bugs like the ones below.

**The letter count task and grid task at the end can be found in this essay's companion Github [repo](https://github.com/jlricon/open_nintil/tree/master/josebench)**. I tested those ones in zero, one, and two shot settings and could not get good performance out of GPT3.

| Prompt | GPT3 | Correct answer |
| --- | --- | --- |
| What is the total number of letters that are in the words in the list? Do not count the numeric characters, count only letters in the alphabet. The word list is: blap2325, lop2344, yep34421111. Reason step by step. First, calculate and write how many letters are in each word, then sum them. | There are 8 letters in "blap2325", 6 letters in "lop2344", and 11 letters in "yep34421111". The total number of letters is 8 + 6 + 11 = 25. | 10 |
| The following are multiple choice questions (with answers) about common sense. Question: If a cat has a body temp that is below average, it isn't in A. an unhealthy range B. safe ranges Answer: \[[source](https://www.alignmentforum.org/posts/iznohbCPFkeB9kAJL/inverse-scaling-prize-round-1-winners); admittedly a bit ambiguous\] | An unhealthy range | Safe ranges |
| In a vacuum, what falls faster, a 1 kg sphere of steel or a feather? | A 1 kg sphere of steel falls faster than a feather in a vacuum. | They fall at the same rate |
| Here's a sentence and two potential ways to continue it. What is the most surprising one? Sentence: The other day I was walking my tiny pony and then unexpectedly a) The pony exploded in a cloud of confetti b) Andrej Karpathy drove past us in a Tesla | The most surprising one is b) Andrej Karpathy drove past us in a Tesla. | A pony exploding into confetti would be laws-of-physics-are-broken surprising, but maybe the model is assuming a fantasy context |
| There are two stacks of cubes: \_ The first stack has a cube "B" \_ The second stack has a cube "A". On top of that cube there is a cube "C". You are allowed to move the cubes one by one. You can only pick the topmost cube of each stack. What are the steps required to form a stack where the letters are ordered, either ABC or CBA? \[Note: see [here](https://en.wikipedia.org/wiki/Sussman_anomaly)\] | 1\. Move the cube "C" from the second stack to the first stack.  
2\. Move the cube "B" from the first stack to the second stack.  
3\. Move the cube "A" from the second stack to the first stack. | Move cube C to a third stack, move cube B on top of it, move cube A on top of that one. |
| There is a game called set. Cards in set have four properties: shapes, color, number, and shading. Each property has three possible values. For example a card can be red, green, or purple (color), and have one, two, or three shapes (number), and be either empty, full, or shaded (shading). Let a set be a combination of three cards such that for each property, either each card's property is the same, or they are all unique. For example, the cards "one green squiggle full, two green squiggle full, three green squiggle full" is a set. Also, the cards "one green squiggle full, one purple squiggle full, and one red squiggle full" are also a set. Are these combinations of cards sets? 1) "three red triangle full, three red squiggle empty, and three red square shaded" 2) "one green triangle full, one green squiggle full, one green square full" 3) "one green triangle full, one green squiggle full, one red square full" | 1) No, because the shading values are not all the same or all unique.  
2) Yes, because all of the property values are the same or all unique.  
3) No, because the color values are not all the same or all unique. | Yes, Yes, No |
| I have a cubic suitcase that is one meter long, one meter deep and one meter wide. I have two boxes. Each box is one meter long, one meter deep and half a meter wide. Can I fit both boxes at the same time in the suitcase? Why? | No, the boxes will not fit in the suitcase at the same time. The suitcase is only one meter wide, and each box is half a meter wide, so the total width of the boxes is one meter. | Yes |
| In a 3x3 grid there are the following objects, listed left to right, top to bottom; e.g. first I will list the top left item, then top middle item, then top right item, then middle left item and so forth: red triangle, empty, empty, green circle, yellow star, yellow circle, green triangle, empty, red triangle. If you could draw a straight line between the two red triangles, what objects does it intersect? First, write down the grid with the objects. Then answer the question. | red triangle, empty, empty green circle, yellow star, yellow circle green triangle, empty, red triangle The line would intersect the green circle, yellow star, yellow circle, green triangle, and the second red triangle. | The answer is "yellow star" |
| In a 2x2 grid there are the following objects, listed left to right, top to bottom: a red circle, a green circle, a red triangle, a green triangle. If I draw a line between the red circle and the green triangle, what shapes does it intersect? | The line would intersect the two circles and the two triangles. | Nothing |

More generally, there are benchmarks that test the capabilities of ML models. These benchmarks include questions like the ones above; one of them (The one about Ian and Steven) comes from one such benchmark (Winogrande). ML models have been [blowing](https://bounded-regret.ghost.io/forecasting-math-and-mmlu-in-2023/) past these benchmarks [faster and faster](https://twitter.com/jackclarkSF/status/1542723429580689408), to the point that in a [forecasting exercise](https://bounded-regret.ghost.io/ai-forecasting-one-year-in/), the performance level for a particularly hard benchmark (MATH) that wasn't supposed to be achieved until 2025 was in fact achieved in mid- 2022 already. These benchmarks tend to consist of questions like the ones I generated above, each trying to isolate a handful of variables and involving a handful of entities to reason about at a time.

Be that as it may, this improvement in benchmarks has not yet translated into real world deployment. We can speculate why, and that speculation probably will point us to the work that's left in the road towards more generally intelligent agents.

First, models armed with just common sense reasoning are not that useful to humans in most contexts as most people will do that reasoning by default unaided (that's why it's common sense!). But a given human does not know all publicly available facts; LLMs kind of do. However, a human will reach out for Google, so human+Google search is the standard to beat. Using LLMs as better search engines is an active area of research and development, with companies like [Metaphor](https://metaphor.systems/) trying to build search engines powered by LLMs.

Second, there are many contexts where LLMs can do an ok job where we have better purpose-specific systems. Indeed one could ask GPT3 to count the letters in "blap1234", but if doing so is useful and is going to be done lots of times, the time spent in writing a python function to do that is trivial and worth doing: in those cases people will using purpose-specific systems instead of calls to LLMs. Hence, human+google search+small programs is a further standard to beat.

GPT3 might have gotten this question wrong earlier, but the same question, when posed to the coding-specific Codex gets us the right answer (it produces a more reliable purpose-specific algorithm which we can then run)

```python
def count_letters(word):
    return len(word) - sum(word.count(c) for c in '0123456789')

def count_letters_in_list(word_list):
    return sum(count_letters(word) for word in word_list)

print(count_letters_in_list(['blap2325', 'lop2344', 'yep34421111']))
```

Codex (and Copilot) are relatively recent additions that as I remarked in the introduction are the first interesting useful application of LLMs that has seen some level of mass adoption.

To further drive the point home: Yes, GPT3 can be given some CSV-formatted data and then it can answer questions about it and sometimes it'll get it right! But if you were doing this in a serious context where getting correct answers matters, or if the datasets are large, you would rather use a parser specifically for that. Even when setting costs aside, would you rather write a data pipeline or a prompt for an LLM? I'll keep the pipelines... but Codex can now help write them.

Third, there are many domains that have so far remained beyond the reach of LLMs because there isn't enough of the right kind of data to train the models on, say models for Computer-Aided Design (CAD), or for prediction of the proteome from transcriptome and epigenetic state. Broadly, complex planning tasks where the context has to be selected from many possible available facts, a key step towards general intelligence. Right now I'm aware of what's in front of me, of what I'm typing, of my physical location, calendar appointments, ongoing conversations, etc. It is one thing to handpick the relevant facts and feed them to a model, and another to list every possible fact that you could possibly be aware of now and narrow it down to your task, and then do the task. We can't experiment with this yet because model's context windows are not long enough yet.

## Code generation

Code generation these days is quite impressive. Here is a more complex prompt than the simple function from the previous section:

> Write an app that handles requests. First deserialize the request into the Request class, checking the the received request has the valid types. Then sum the x and y values, check if the result is a prime number, and return the result. If the received request is invalid, return an error indicating the specific fields that are invalid. Use dataclasses and the Flask framework.

And here is the code produced, verbatim from Codex. I copypasted it and ran it, checking that it indeed runs as one would expect. The primality algorithm could be improved and deserialization could be done in a safer way but it's a start.

```python
from dataclasses import dataclass
@dataclass
class Request:
  x: int
  y: int

@dataclass
class Response:
  result: int
  is_prime: bool

@dataclass
class Error:
  message: str

from flask import Flask, request, jsonify
app = Flask(__name__)

@app.route('/sum', methods=['POST'])
def sum():
  try:
    req = Request(**request.json)
    res = Response(req.x + req.y, is_prime(req.x + req.y))
    return jsonify(res.__dict__)
  except Exception as e:
    return jsonify(Error(str(e)).__dict__)

def is_prime(n):
  if n < 2:
    return False
  for i in range(2, n):
    if n % i == 0:
      return False
  return True

if __name__ == '__main__':
  app.run(debug=True)
```

Ok fine one might say. But writing request handlers and checking whether a number is prime has been done to death, what is a more interesting, unfairly complicated program we can write? I asked Codex various times to `Write the backend of an app that acts as a clone of Twitter` and while I got a bunch of valid-looking code, I did not get something I could just run to Y Combinator with next day.

Another useful piece of software to have is a clone of Turbotax. Codex can't produce Turbotax yet based on this prompt:

`My total income this year was X dollars. Write a program that calculate my taxes according to the latest published IRS regulations. I contributed 1000 dollars to my Traditional IRA. I also bought and sold stocks and had both W2 and 1099 income.`

Being more reasonable, I tried a couple of times the prompt `Read all pdf files in the directory called frames and return a list of the ones that have text containing the string "Hiring". To avoid issues when parsing PDFs, use OCR to extract text. Set a high PIL MAX_IMAGE_PIXELS.` This indeed leads to a program that converts PDFs to images and then searches for the string I mentioned. Neat! However that on first trial the program crashed because PIL, the library being used by the generated code, gives up if the file is too large so I had to manually add the last part to the prompt. Still, neat! Though you could imagine how this program could work, Codex adds value in that it can write it faster than you can, and then you can add the finishing touches if needed. I was also able to get good results eventually with `Extract 100 random frames from an input video, send them to the AWS Recognition API to check for illicit content`.

Now for something that's not as easy: `Download all scientific papers from the internet and OCR them. Store them locally`. The second part of this is the code that we got from earlier, the first part is left to us to define. Codex on its own does nothing useful with this prompt. But also most people wouldn't know where to start: this is far from common sense. Perhaps going to Google Scholar, doing random searches, and scraping all PDFs linked from there? But many papers do not have publicly available PDFs, plus this random search approach could take forever. One could then get the Semantic Scholar dataset, which I happen to know is fairly comprehensive, get the DOIs from there, then plug those into Sci-hub, and get the PDFs from there. We might be able to plug this into GPT3 to ideate, and then copy the GPT3 ideas into Codex, but I wasn't able to get much useful doing this, and frequently I got nonsense.

I've seen some people plugging in prompts into Google search, getting items from there, and iterating back and forth with the LLM and search. For example, one might do the following:

1.  Search google "scientific dataset with all papers", grab the first 50 links
    
2.  Ask the LLM which ones are promising
    
3.  Visit 3 links, explore those websites and get links to the dataset.
    
4.  Get the dataset
    
5.  Download an example or a few, figure out what the format is. You might think that you can read the metadata provided by the dataset provider but empirically they are not always correct. The program might have to be written once, then maybe it will crash, then it needs to be checked for the wrong assumptions, then ran again, etc. One also has to be accounting for rate limits, which may not be obvious at first, but is a problem that one might run into while developing this.
    
    1.  In the case of Semantic Scholar, you need to fill a form and wait until you get approved
6.  Download the entire dataset, pull the DOIs
    
7.  Plug the DOIs into sci-hub, get the PDFs
    
    1.  You could also just download the entirety of Scihub, no Semantic Scholar required, if you find the link to that!
8.  Do the OCR
    
9.  Profit!
    

This is something Codex cannot do today. It is a particularly hard case because it involves multiple iterations and one human interaction with an external party. Arguably _precisely these are the hard parts of software engineering, when one doesn't know exactly how to do something, when someone else needs to give you access to a resource, or where unexpected errors have to be debugged_.

At first, Codex seems to be able to produce impressive pieces of code spanning multiple files and languages. A single prompt was able to generate a simple Tetris game using html, css, and javascript (results in this [repl](https://replit.com/@artir/Tetris)). The code is almost correct (e.g. There is a bug when rotating the Tetrominoes) but still, it's a mostly functioning ~450 LOC program produced from a short prompt! One might object here that Tetris has been around for decades and there are multiple implementations that are open source. Codex might just be looking up those and copying them. Sure enough I was able to find snippets here and there on Github that resembled the Codex-generated code but no evidence of explicit copy-paste.

Trying to think of a task that a developer that can code Tetris could do but a model that doesn't understand coding as well cannot do, I decided to give Codex a longer assignment, to implement a single player interface to the game of Set. Set needs to be explained first, so I wrote an explanation of a slightly simplified version of Set below. A reference implementation of what the output could look like is [this](https://playset.netlify.app/) one.

> In the game of set, there is a deck of cards. Each card has a picture of a number of shapes (all the same in each card), in a specific color, in a given shading. The specific values these can take are
> 
> -   Number of shapes: One, two, or three
> -   Color: Green, purple, or red
> -   Shading: Empty, lines, or full
> -   Shape: Squiggle, diamond, or rectangle
> 
> Let's define a set as a group of three cards that for each property listed above are either all the same or all distinct. For example the cards "one green empty squiggle, two green empty squiggle, and three green empty squiggle" is a set because they are all green, all squiggles, all empty, but the number of shapes is different in each card. Another example could be "one green empty squiggle, two red empty rectangle, three purple empty diamond". This is also a set because for each property, all cards either have the same value, or they are all different.
> 
> The game starts by dealing nine cards on a 3x3 grid. The shape on each card should be visible. A player can then click on each card to select it. Up to three can be selected at a given time. If the player has selected three and the cards is a set, the player scores one point and three additional cards are dealt to the table. The game ends when the cards on the table don't form any sets, or when the deck runs out of cards. Every card in the deck is unique, and there are a total of 81 cards in it.
> 
> Write an HTML, CSS, and JS program that let's a user play single player set

Codex did generate HTML, CSS, and JS, and I tried around 6 times to generate various versions of the code, but I could not get it to produce anything playable. Often, the code quality it produced was really bad.

![](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221103110325200.png)

From the façade of the code writer, an artist emerges

As it happens, there are not many implementations of Set in Github. One can find [some](https://github.com/jpritcha3-14/set-card-game), which have better code quality than what Codex gives you. I tried the text in that repo's README as a prompt to see if that got us somewhere, but turned out to be worse than my own prompt.

So what do we conclude from these experiments? Codex does not know how to program to the level of proficiency of a junior software engineer, despite the facts of being able to produce _some_ output that, if produced by a human, would lead you to think that it in facts is capable of more. Codex does well when there are lots of publicly available examples of the kinds of code it is being asked to write. It is able of more than parroting back code, one can grant it some ability of understanding what code does, because it does translation between languages reasonably well, and is able to compose programs that do different logical operations into a unified whole ("extract random frames+check using an API", for example). But it seems to struggle with prompts that are not as specific. The human thing to do would be to ask for help or clarification. One way to overcome this would be to provide function signatures and asking it to fill them in. In statically typed languages, the types could constrain the model enough to provide a reasonable answer, especially if the model is allowed to read the results of the type-checker and feed that back into the program.

Tinkering with Codex, if anything, reveals that a lot of currently practiced software engineering is in fact plumbing and recombining snippets of code that others have created previously. Even if Codex cannot by itself build the Airbnb website, these models can eventually relieve developers from the most mundane tasks, freeing them to think about the challenging and creative parts of software engineering.

## What does "true understanding" looks like?

It's a common point of debate to argue about whether ML models truly understand their output. Arguing over what understanding means is harder than agreeing whether a model passes an easier to define Turing-style test: If we take human-level as the level of an understander, then clearly current models are far from being able to understand _everything_ as well as we do. The examples above from image generation models show outputs that a human would not make unless say drunk or trying to deceive you.

A system that is able to understand a domain learns the domain in a way that looks different from the way a system that doesn't understand the domain does. Take addition and subtraction of natural numbers, for a simple case. This domain involves awareness of what the natural numbers are and how they are ordered, and being aware of the abstract notions of adding and subtracting numbers together. Importantly someone that understands how to sum knows when they are _not_ able to perform the operation (say, if the numbers are too big). GPT-type models generally will try regardless and be wrong a lot of the times.

When I learned these operations, I learned an algorithm to do sums and subtractions by hand, along with the broad idea of what those things mean, and learned how one could use a calculator to sum numbers. I also noticed that sometimes one makes mistakes when summing large numbers if one is not paying enough attention, in which case one wants to use a calculator.

For a system that understands these operations, the performance in them should not be altered by the length of the number. Numbers are numbers and the algorithm followed should be the same. And yet we don't see this for GPT3: it gets two and three digit operations and then utterly fails with one more digit. The likely explanation is precisely that it doesn't _really_ understand what it's doing:

![image-20221105202529080](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221105202529080.png)

Brown et al., Language Models are Few-Shot Learners (2020)

Minerva, which was trained as a purpose-specific system for a narrower set of tasks, including arithmetic, does better than GPT3, but also presents the same problem, especially for multiplication.

![image-20221105202710369](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221105202710369.png)

Lewkowycz et al., Solving Quantitative Reasoning Problems with Large Language Models (2022)

**A model that really understood addition (or subtraction, or multiplication) should present performance that is the same regardless of how big the numbers are**: the curves should be flat at 100%, especially if we count examples that the model outputs as opposed where the model admits to "be too unsure". Ideally, the model would either return the correct answer or recognize the problem and call a python script to compute the right answer. Sure enough, one could train models for this specific task to do what I just suggested and that would pass the test. Then, if in other contexts where the "+" sign appears where a human would always know is addition, if then the model fails to do the right thing, then we can conclude it did not understand addition after all.

Why make this point? If we really deeply care about e.g. addition, can't we just finetune and scale models to solve the kinds of tasks they currently fail at? Minerva after all shows markedly better performance than GPT3? If one focuses on small enough digits, the models seem to work fine, the curves can be bent up as much as we want!

In the limit, yes, if we had infinite data and parameters. In practice, data and compute are finite. The point I am making here is that these models are not doing these operations like we do, and because of that they struggle to generalize them when exposed to unusual kinds of questions that were not in their training set like big numbers or like the 'unfair' questions in the common sense section earlier.

Here one could say that perhaps they don't fully get arithmetic because there's not that much of it in the training set. Yes, that's part of the why: With more of it you get better performance. But a human being doesn't need a million examples of sums to learn to generalize the concept of sum over arbitrary large numbers. To be sure, there are systems that can actually get this robust sense of understanding if they have access to coding tools. DreamCoder ([Ellis](https://arxiv.org/abs/2006.08381) et al., 2020) or the famously impractical [AIXI](https://en.wikipedia.org/wiki/AIXI) work by trying to generate the simplest programs possible that can produce the inputs seen so far. This is a step up in robustness from what neural networks do, and one could imagine enhancing transformers with these sort of symbolic approaches in the future. Or perhaps it'll all be transformers! It wouldn't be the first time a [field](https://nintil.com/discoveries-ignored#why-does-this-happen) rejects a paradigm and then returns back to it.

How do transformer models _actually_ do arithmetic? One could do a [circuits](https://transformer-circuits.pub/) style examination of this and study why exactly the models break with bigger numbers, but I have not seen any. It sounds fun to take a small LLM and training it purely to learn how to sum progressively larger numbers, then observe what it's learning.

The BigBench collection of benchmarks includes some problems that are like the ones I have in mind like this [logic grid puzzle](https://github.com/google/BIG-bench/tree/main/bigbench/benchmark_tasks/logic_grid_puzzle), and where models do not seem to get better across four orders of magnitude in parameter increase, doing as well as choosing randomly between the options given. That is, with the exception of PaLM when given at least one example which is slightly better than the average human rater in their set, but still markedly worse than the best rater. In a different task using [emojis](https://github.com/google/BIG-bench/tree/main/bigbench/benchmark_tasks/symbol_interpretation), Pig [Latin](https://github.com/google/BIG-bench/tree/main/bigbench/benchmark_tasks/language_games) (which are less common on the internet), models still struggle. Models do get many common sense reasoning tasks right, but one can always find common sense BigBench tasks where they still struggle. The building blocks of reasoning required for the tasks models fail at seem to be there: they are applied individually in other tasks, but somehow they fail to realize that they have them and can be combined to solve the task.

In summary, models seem to be interacting with the world with their symbolic arm tied behind their back. We don't have that limitation.

\[Update 2022-11-17\]: Only a few days after I published this a new paper ([Zhou](https://twitter.com/oh_that_hat/status/1593337982144110593) et al., 2022) came out, with a graph that looks like a substantial update in the right direction:

![Image](https://pbs.twimg.com/media/FhxgFRnVUAMAbFg?format=png&name=900x900)

To get models (GPT3) to do this, what they did was to supply models with the algorithm required to do the relevant operation. For example for addition we may say:

> Problem: 128+367= Explanation: The first number is 128, FN=\[1,2,8\]. The second number is 367, SN=\[3,6,7\]. Since FN \[1,2,8\] has 3 digits, SN \[3,6,7\] has 3 digits, thus the maximum number of digits is 3. In each subsequent step, we remove one number from the end of FN and one from the end of SN. Length of FN is 3. FN=\[1,2,8\]. Length of SN is 3. SN=\[3,6,7\]. FN\[3\]=8. SN\[3\]=7. C\[3\]=0. Since 8+7+0=15, 15>10, 15%10=5. Length of A is 1. Thus A=\[5\]. Since (15-5)/10=1, C\[2\]=1. Length of FN is 2. FN=\[1,2\]. Length of SN is 2. SN=\[3,6\]. FN\[2\]=2. SN\[2\]=6. C\[2\]=1. Since 2+6+1=9, 9<10, 9%10=9. Length of A is 2. Thus A=\[9,5\]. Since (9-9)/10=0, C\[1\]=0. Length of FN is 1. FN=\[1\]. Length of SN is 1. SN=\[3\]. FN\[1\]=1. SN\[1\]=3. C\[1\]=0. Since 1+3+0=4, 4<10, 4%10=4. Length of A is 3. Thus A=\[4,9,5\]. Since (4-4)/10=0, C\[0\]=0. There are no more digits and C\[0\]=0. Thus the process is complete. Since there are no more operators, the problem is complete. The final Answer is \[4,9,5\].

I wouldn't still say that the models really understands addition (It still fails with larger numbers), but it gets closer (by my own test). I don't think prompt engineering for this class of problems is the way to go: In the real world we expect arithmetic to work flawlessly. Instead, models should delegate to e.g. a python interpreter or carry with them a bag of functions they can call to perform the relevant computations.

## Some ideas for benchmarks of "true understanding"

Some research ideas for benchmarks where we could test whether a model 'really' understands a concept: They all start from the same premise, that given a series of concepts linked by relatively simple mechanical rules, performance on a task involving the concepts should not depend on the number of these entities. So the tests have to be able to be arbitrarily scalable so we can evaluate performance across entities. These have transformers in mind, of course there are systems that do these very flawlessly. On this test, a python REPL understands addition in a way GPT-3 does not! This is okay.

-   Arithmetic (+,-,\*) is the example from earlier
-   Chains of logical implication or causal reasoning
-   Positional reasoning (The task earlier about finding shapes that are intersected by a line on grids)
-   Extraction of given entities from a text
    -   E.g. in the text "John went for a walk"->John; "John and Sophia went for a walk"->John, Sophia
    -   These could be longer texts or stories with an increasing number of names
    -   Can the model read War and Peace and extract all proper nouns?

What we would do with these is to look at the shape of the (number of entities/complexity, performance) graph and see whether it's flat. If it is, then we can say the model has correctly learned the underlying concepts.

Can we have a similar benchmark for understanding the concept of 'dog'? Arguably LLMs understand dogs better than they understand logic, so I expect they'll do well at these. But the same strategy to generate examples doesn't obviously work here. For a concept like that commonly used reasoning benchmarks with questions like 'Do dogs have four legs?' seem enough.

## Progress has been mostly in narrow domains, with purpose-specific models. AGI is not quite there yet

One general heuristic that I do believe in is that purpose specific systems beat general systems, given the same amount of compute and resources. GPT3 can play chess, but AlphaZero is better, GPT3 can steer a car if given a description of the scene, but Tesla's FSD will do better, GPT3 can write code, but Codex does better and so forth. I do not know what Google Translate uses, but I bet it's not PaLM, it's probably a custom built system specifically for translation.

Why make this point? Because it shows that while we are seeing progressively more powerful AI systems in the wild, these are not necessarily indicative of progress towards _general intelligence_. I do not make this point just because there happens to be purpose-specific systems that perform better than general purpose systems. I make this point because

1.  The fact that we have purpose-specific systems show that AI can achieve really good performance in those domains: we know what "good performance" looks like for AIs today
2.  The fact that the general purpose systems do not seem to be making fast progress in a varied set of tasks, despite the fact that we know there is enough data to be able to achieve good performance in each of them (As 1. shows)

"Purpose-specific" is doing perhaps too much work. In a way "answering common-sense questions as if you had all public written knowledge" is "purpose-specific" but it is also a very broad category, even if it cannot drive a car as well as FSD can. But common sense is not all there is to intelligence.

There are sequence prediction tasks that are not commonsensical: Parsing a genome and [pointing](https://www.biorxiv.org/content/10.1101/2022.08.22.504706v1) to deleterious mutations by how surprising they are to the model. GPT3 can't do this and GPT4 won't do this. Same for parsing SMILES into chemical structures, but you can imagine a model built just for that tasks that does it reasonably well.

By itself, this is not an issue. In a [previous essay](https://nintil.com/ai-safety) I argued that we can safely accept as a premise that humans are not generally intelligent agents, and that human-level intelligence does not require a system to be able to do any arbitrary task. If we could engineer a system that can produce and control other systems and then that aggregate can do what humans can do, that would suffice. Recall the problem from earlier, asking GPT3 to count numbers in a series of words and failing to do so. In theory, we could ask the model to hand off the task to Codex if it detects a problem that is better handled in code. In practice (from experimenting a bit with this) this is as of today _extremely_ finicky.

Future advances in interpretability may also lead to a reduced reliance on general models day-to-day: LLMs do well at tasks like recognizing and extracting entities from a text, it may be a matter of time until it's possible to extract the relevant circuits to do exactly that and package them into smaller, more efficient (and accurate) models. Rather than having general models deployed everywhere, we may end up with large models that are then strip-mined and repurposed for the specific task at hand.

## Hybrid systems can still be very useful

Consider what [Adept.ai](https://www.adept.ai/act) is building. Instead of constantly scraping the internet and asking a model to produce answers, they are training a model to use a browser like humans would. Not much is known about this approach, pioneered by [WebGPT](https://openai.com/blog/webgpt/), and how it scales to other domains. Does the Adept approach work for CAD or film production? In principle it could if given enough demonstrations. If someone makes a Hollywood-level movie purely from a prompt without intervening until the result is ready, I expect a hybrid model will get there before end-to-end models do. Replit recently introduced [Ghostwriter](https://blog.replit.com/ai), a system similar to Github's Copilot but that could become more powerful due to the fact that Replit is a fully integrated development environment: Replit knows what users type, what errors they get, what they run in the REPL. Replit is in a unique position here for now, but they may be limited by the kind of software that usually gets built on Replit. It would be interesting to see what happens if one instruments computers at Lockheed Martin while mechanical engineers are designing parts, and then uses that data for the CAD problem. This sounds farfetched, but RunwayML is in the same position Replit is but for video, so if one had to bet on someone making progress in automating end-to-end movie generation it would be them (and big tech companies).

I have different intuitions about systems that can generalize a lot and systems that are doggedly trained in narrow domains. What if we can get 90% of the way there with narrow AI? It may be more [useful](https://www.gwern.net/Tool-AI) to have end-to-end agentic systems but even if this proves as hard as I think it will be, I am more optimistic about cobbling together these narrow systems with some human interaction between steps, in a way perhaps reminiscent of Drexler's imagined future of AI [here](https://www.fhi.ox.ac.uk/wp-content/uploads/Reframing_Superintelligence_FHI-TR-2019-1.1-1.pdf).

## Forecasting AI: some heuristics

I've been playing with GPT3 and image models a lot to see what they do and do not do. Most questions (or image prompts) I've posed to these models get a reasonable answer; at this point one has to actively try to mess around with the models to get it to say stupid things reliably. Models are also getting better at saying "I don't know" rather than hallucinating answers.

That said, the answers one gets from GPT3 have some bland quality to them. They can be helpful indeed but not mindblowing.

In the case of writing code, even if we haven't yet distilled Jeff Dean into an LLM, a nontrivial chunk of software engineering is looking up libraries and function calls in stack overflow and pattern-matching to the right snippets, and this is something LLMs can do today already, and we have only gotten started.

A generic heuristic I came up with is that AI will continue to struggle with tasks that humans _can't_ do or plan how to do immediately. As an example, if you ask me to produce the SQL to select a column, filter by another, and compute an aggregate, where this involving joining two tables together, I can give you this answer without really thinking about it that much (I have written a lot of SQL!). GPT3 in fact will give you this:

```sql
SELECT u.name, count(v.event_type) as view_count
FROM users u
JOIN view_events v
ON u.user_id = v.user_id
WHERE v.event_type = 'view'
GROUP BY u.name
```

I have two tables, one with Users (name, user\_id), and another with View events (id, user\_id, event\_type) Create a SQL request that displays the number of view events with event\_type="view" per user

If you asked me to write a piece of software to solve \[this\](https://adventofcode.com/2020/day/11) Advent of Code problem I would not be able to tell you as readily what the solution is, the code doesn't pop in my head fully formed, there's some thinking one has to do first (my \[solution\](https://github.com/jlricon/advent-2020/blob/master/src/bin/day11.rs)). Advent of Code might be a fair benchmark for code-generation models, each question in AoC is self-contained and often not trivial, while at the same time being far from the complexity of writing a 10k LOC program.

Another heuristic is that models, by their nature, will continue to be deficient in "true understanding" in the sense defined earlier. In the case of LLMs this will manifest itself in there still being simple logical puzzles that humans reliably get right and LLMs do not whereas in the case of image models this will manifest itself as weird artifacts and absurd outputs that humans could easily tell are not quite right. I suspect that this lack of "true understanding" will harm model performance. It's an interesting fact that the symbolic models of old (GOFAI) do better in their domains than modern LLMs do. Humans have the advantage of both, fluidly moving from a symbolic/rational stance where concepts are held as fixed (I see a table in front of me) sometimes and as [nebulous](https://meaningness.com/nebulosity) some other times (The table could be used also to sit on, as a source of wood, to stand on, or to not get wait if it's raining).

Sometimes when you see a table there's in fact a table there, other times it's actually an unconventional stool, the thing is knowing when to think in each way.

Lastly there's the slow progress, so far, in multi-task ML. I am more optimistic about forecasts on purpose-specific models than I am about general models. The state of the art LLM for interfacing with web-browsers won't be the same one developers will use to write code.

A high level framework to think about this is that the case where these models are particularly useful is when they are better than we are _and_ we can trust the output. If they are worse, why use them? If we can't trust or verify their answer, even if they are better or know more than you, why use them?

![image-20221111144747692](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221111144747692.png)

The exceptions to this is where the model is still not uniformly human-level in a domain, but can still assist humans with subtasks within that domain. The issues with the upper right quadrant could be solved by finetuning and experts declaring that the model is as good as them. For example, with the help of doctors, models can be finetuned to predict illnesses from symptoms, then they can then sign an audit of the model. If users trust the panel of doctors, they may transitively trust also the model.

## (Some) Scaling will end

A key reason for recent hype is scaling laws: The fact that ML model performance by various metrics increases predictably with increased parameter count and number of tokens the model has been trained on (Kaplan et al., [2020](https://arxiv.org/abs/2001.08361), Hoffman et al., [2022](https://arxiv.org/abs/2203.15556)). If scaling breaks, it could send AI back to another soul-searching [winter](https://en.wikipedia.org/wiki/AI_winter) as it has happened before a couple of times.

The Hoffman (Chinchilla) paper shows that some of the early enthusiasm regarding scaling by parameter count alone was premature: eventually [data](https://www.alignmentforum.org/posts/6Fpvch8RR29qLEWNH/chinchilla-s-wild-implications) becomes the bottleneck and we have already strip-mined the entire internet for tokens. One way forward is getting models to generate more data: as I suggested earlier, diffusion models can be asked to produce individual objects and those merged into a single image, and then train the model to predict from a prompt with positional information ("there is a red apple to the left of the green apple") this resulting image.

Models can also be asked to judge their own output and finetune them on the examples the model itself considers accurate, which boosts performance across various benchmarks (Huang et al., [2022](https://arxiv.org/abs/2210.11610)). I'm not sure if this will matter much in practice on the margin, because the models are already quite good at common sense reasoning.

The one domain of interest where more out of distribution data can be generated is coding. There does not appear to be barriers to scale code generation models if one can always generate more code or use test suites as an additional term in the loss function. It remains to be seen what kind of code can be generated with this approach: I see how models can get better at writing single functions, but going from there to writing LLVM or CAD software, that's a stretch.

What about getting models that can improve themselves? If one buys scaling maximalism this should not matter much, the ML-model generating ML model will tell you to give it more parameters. If it's a matter of architecture, we already have [neural architecture search](https://en.wikipedia.org/wiki/Neural_architecture_search). If further innovations are required, especially to make the scaling itself occur we need models that understand GPUs, interconnects and the like as well as we do; the road to AGI passes through automating away the teams developing things like JAX, XLA, FasterTransformer or PaLM. Ultimately I think solving software engineering is not enough for AGI: one needs to solve science itself.

## Conclusion: Predictions

Back in 2016 (around when AlphaGo came out) [I wrote](https://nintil.com/future-challenges-for-ai/) some feats that I thought would be good benchmarks for AI. Number 2 was eventually achieved (Beating Starcraft). Number 1 (Beating a card game like Magic: The Gathering) is something that hasn't really been tried, but that now I suspect it is easier than it seems and could probably be accomplished if it was tried.

The point of this essay, why I wrote it, was to come up with a handful of things that I expect will surprise me if I get wrong. To hold myself accountable, **I am willing to bet up to $5k on each of these**[2](https://nintil.com/interesting-ai-models/#sidenote-2) .

To me, committing to making these bets is more important to actually making money out of them. Even if no one takes the bets and some of these come to be true by 2026, that will be a strong signal for me to consider my intuitions about AI development to be _very_ misguided. One [prediction](https://www.metaculus.com/questions/5121/date-of-artificial-general-intelligence/) market side gives strong AI a probability of 15% by 2026; [this](https://www.metaculus.com/questions/406/when-will-ais-program-programs-that-can-program-ais/) other one gives ~30% to AIs that can do sophisticated programming by the same date. Hollywood-levels movies by 2026 gets 32% [here](https://manifold.markets/MichaelTrazzi/hollywoodlevel-aigenerated-feature). These all seem very high to me. I chose the bets below as attempts to upper bound within reason what AI capabilities might be by 2026, so implicitly my own estimate that I will lose the bets is <5% likely?

\[2\]. The way this works: You can bet any amount over $500 at even odds with me. I will take the skeptical side of these bets. Once the amount I've betted in a given bet equals $5k no more bets will be taken. If I deem you trustworthy we'll bet; if I don't know you you need to find someone that you think I'll trust to vouch for you. When we agree to bet, we'll also agree on a judge for the bet (unless you trust me to be fair)

1.  As stated earlier, there will be no Hollywood-level AI generated feature film by 2026 (Some discussion on [Twitter](https://twitter.com/MichaelTrazzi/status/1581331967877910529), prediction [market](https://manifold.markets/MichaelTrazzi/hollywoodlevel-aigenerated-feature)). My interpretation here is end to end prompt->movie, with some light editing allowed.
    
    1.  What doesn't count
        
        1.  Taking an existing movie, as they do [here](https://thenextweb.com/news/text-to-image-generator-creates-award-winning-ai-film-the-crow), and changing its style
        2.  Intermediate manual steps, as they do [here](https://www.youtube.com/watch?v=ajyL9FyN-pw)
        3.  A movie that embraces the artifacts discussed earlier for artistic reasons, as they do [here](https://www.youtube.com/watch?v=Bo3VZCjDhGI)
    2.  What counts as Hollywood-level
        
        1.  An animated movie of high quality (Like Studio Ghibli or Ufotable-level quality) OR a movie that resembles one with real-life actors
        2.  At least one hour in length
        3.  Is screened in cinemas AND/OR a large VOD site (Netflix, Prime, Hulu, Apple Movies, Youtube) AND Ranked in Metacritic (metascore) higher than 60%, with at least 10 reviews OR ranked in Letterboxd with a a score higher than 3.5 and at least 2000 reviews
    3.  What counts as light editing
        
        1.  Taking the resulting output video and cropping it to a shorter length
        2.  Adjusting overall brightness/contrast
        3.  Increasing resolution of the final video
        4.  Generating say 10 minutes of video, then generating 4 completions of the next 10 minutes and picking the one by hand, then manually completing the movie in this way scene by scene
2.  \[EDIT 2025-03-09; Cancelling this one, I consider this to be doable either now or soon by AI from a long enough prompt with eg. Claude Code\] There won't be end to end complex software synthesis by 2026 (in the spirit of [this](https://twitter.com/evanjconrad/status/1580953020296179713) tweet) even though software users interact with that is written in 2026 may have some parts that are generated with e.g. Copilot or their descendants (see a glimpse of the near term [here](https://youtu.be/_3MBQm7GFIM?t=268)). (The closest prediction market I could find for this is this [one](https://www.metaculus.com/questions/11188/ai-as-a-competent-programmer-before-2030/)).
    
    1.  To be more specific, as detailed in the Metaculus link, I am referring to end-to-end synthesis, either to code, assembly, or binary, as opposed to generating individual functions or chunks of functions based on comments. The program should be given a description, perhaps a 10-20 page spec of what one wants and produce the entire program end to end.
    2.  In a sense, training a neural network counts as gradient descent writing complex software, and we can do this already. This doesn't count for this prediction.
    3.  Producing a complex program (>10kLOC) based on comments -and- function signatures and types will not count as meeting the criteria (though this is still quite impressive!)
    4.  What is a complex program? 10k lines of code is one threshold. Some examples of complex software by this definition include: a [linter](https://github.com/charliermarsh/ruff), a deep [learning](https://github.com/pytorch/pytorch) framework, a push-[notification](https://github.com/binwiederhier/ntfy) sender, an authentication [framework](https://github.com/nextauthjs/next-auth) for Nextjs, a fully featured [table](https://github.com/TanStack/table) component for React
    5.  If more than 30% of the code is copied verbatim from elsewhere, it doesn't count. In the spirit of this bet, being asked to write a compiler and returning [this](https://github.com/jamiebuilds/the-super-tiny-compiler/blob/master/the-super-tiny-compiler.js), while a valid answer for a program that one can call a compiler, would not count.
    6.  Models are allowed to output code and run it, read message errors, and iterate from there
3.  A significant fraction of interesting or useful (the ones that you might go back to or share with friends) posts (personal blogs and corporate websites) and news stories _you_ \[the average reader of this post\] read on the internet won't be written by LLMs in 2026. I do expect content marketing-type posts will be progressively more automated.
    
4.  1.  I have in mind posts like the ones you read on this blog, Slatestarcodex, Ben [Kuhn’s](https://www.benkuhn.net/) blog, or Businessweek columns like Matt Levine's
    2.  To count, 30% or more of said content must be LLM-generated end to end
    3.  The prompt cannot contain the key points the essay makes. For example to write [this](https://www.benkuhn.net/impatient/) essay the prompt can’t contain the specific quotes, or using “learn keyboard shortcuts” or “OODA loops” as a examples
    4.  LLMs might be able to generate, say, full-length fantasy books. As long as this content is not the majority of what is read, the prediction stands.
    5.  Posts written using LLM-assisted writing but ultimately under human control in a granular way would not count
    6.  Generating a handful of posts with LLMs and picking and publishing the best (lightly edited) would count.
    7.  Models are allowed to access the internet
5.  There will be no serious engineering project before 2026 which is not mostly designed by hand. Here I include cars, airplanes, dams, or computer chips, but also less complex ones (electronic devices like [Toeglos](https://toeglo.com/) or a simple [flashlight](https://grabcad.com/library/battery-flashlight-1)). Of course, parts of these processes will be progressively more automated.
    
    1.  As with movies, the task at hand is not generating parts but a complex whole. This means not only the concept design for the car (this can be done now), but CAD files or written documents about how one goes about making the car, dimensions of parts, etc.
    2.  If there is a collection of models that can produce the above, even if a human still has to physically manufacture the device, order supplies, etc, it will count. It will not count if the outputted files require any fixing on the human side, files should go "unmodified into production".
    3.  It is allowed to have a model that takes issues encountered when trying to make the device or project and outputs a new plan that is then tried. Arbitrary iteration is allowed as long as it is model-directed

## Predicting scientific advances is hard

When writing this essay I found myself going back and forth between various perspectives. There's "What can systems do today", "In which ways have forecasts been wrong in the past", "How fast has been progress", "How can we decompose progress into subcomponents; how do those change?", etc. Given what I've written above, I could also have seen myself writing a different blogpost that comes to different conclusions. For example, one could take the fast improvement in AI and the Imagen results and predict that by implementing my suggestions (generating better training data) we will solve compositionality and eventually have Hollywood-level movies by 2026. How does one aggregate all this information? How meaningful are the forecasts I make here? Is it all vibes and I'm just justifying them with arguments? It does feel a bit like that. But had I written the opposite, that by 2026 we will have done all the things I am betting against, I would be doing the same thing: I would be justifying my alternative 'Wow GPT3!' vibe with arguments.

It's not even fully clear how exactly I should update my beliefs if I lose the bets above. I just hope I do somehow.

## Appendix: Blog post generation with GPT3

I took a number of blogposts and fed them to GPT3 up to a point, making sure to have at least ~700 tokens, then seeing what GPT3 would return. Below I show both the original paragraph that followed the input text as well as the GPT3 completion. I ran GPT3 three times and picked what I judged to be the best response. Each section is preceded by the last snippet of text in the prompt, for context. Maybe there are better ways of doing blogpost completions than this; one [particularly](https://twitter.com/repligate/status/1590918765537275904) skilled individual at prompting says it takes even months to learn how to do it correctly. The model is by no means fine-tuned on blogposts, with further expenditure it may be possible to improve these results.

## Scaling tacit knowledge ([link](https://nintil.com/scaling-tacit-knowledge))

> Here's a more complex example: Suppose you want to learn how much salt you should be taking.

### GPT3

You could just read the Cochrane review. But recall the questions you could ask an expert: Is the evidence good? Do the trials used in the review use the same kind of salt or a different kind? How much sodium is in the used salt? Do the trials use the same kind of patients? How much salt do you actually eat in a day? What is your sodium blood level? What is your blood pressure? The Cochrane review would not give you a good answer to these questions.

A known property of Cochrane reviews is that they cite the studies they are based on! Many of the things GPT3 says could be refinements for Cochrane reviews are already in the Cochrane reviews.

The Cochrane review could be refined to help you.It could provide links to the studies it used, it could provide links to studies that tested the same hypothesis that were left out of the Cochrane review. It could provide links to other reviews with the same hypothesis. It could provide links to reviews with the same hypothesis with different populations. It could provide links to studies that are in the same field with different hypotheses (for example, studies on salt's effect on blood pressure). It could provide links to studies on the effect of other dietary interventions on blood pressure.

This example shows that you could use reading to get a more nuanced answer than a single expert could. You could use it to get a more nuanced answer than a whole community of experts could. You could use it to get a more nuanced answer than a whole community of experts could, plus the other papers they have read.

But reading papers is slow. Going through the papers cited in the Cochrane review is slow. Going through the papers cited in those papers is slower. Reading papers on a new topic is even slower, because you need to gain the context to understand them. Even a systematic review with a lot of context will take days to weeks to read.

### Original

The Google approach yields 2.3g per day (FDA guidelines). But then you are of course smarter than this and you keep digging; you deploy the heuristic of inverting common knowledge (more salt is worse, period) and try to find evidence that actually, too little salt can be bad. You [come](https://www.bbc.com/future/article/20181029-eating-less-salt-benefits) [across](https://academic.oup.com/ajh/article/27/9/1129/2730186) [some](https://www.scientificamerican.com/article/its-time-to-end-the-war-on-salt/) [work](https://www.health.harvard.edu/blog/in-defense-of-the-salt-shaker-2019110618175) on [that](https://en.wikipedia.org/wiki/Health_effects_of_salt), showing that there's a U-shaped relation and that perhaps the right amount of salt to consume is _more_ than the guidelines say. You feel smug and smart. You talk to some doctors that vaguely gesture at the U/J-shaped relation between salt and mortality. But some time after, you learn of a piping hot meta-analysis fresh off the press, analyzed and endorsed by this one [Stephan](https://twitter.com/whsource/status/1463248014395396098) J. Guyenet on Twitter, reasserting accepted knowledge: less salt is better.

This is the answer I would believe, but for this particular one, I started with an advantage because I have done the prior work of searching experts to trust in that one space (As it happens, my [other](https://nutritionfacts.org/video/sodium-skeptics-try-to-shake-up-the-salt-debate/) go-to expert for nutrition matters concurs with Guyenet). Whether here one ends up with the correct answer or not would depend on how good one is at interpreting primary sources and how good one is at finding good experts. But here we don't have to talk to the experts, for this one very simple question that asks a relation between two variables, we can rely on short analyses from the experts; this both points to primary sources and explains where the contradictions may be coming from.

This is better in one regard than talking to the experts; they probably can't cite all these papers from memory, nor immediately address what the trouble is with the discordant paper you found that morning. Particularly illustrative was this podcast between Gary Taubes (Who subscribes to the "carbs are bad" view) and Guyenet (Who think carbs are fine in moderation). Guyenet came [prepared](https://www.stephanguyenet.com/references-for-my-debate-with-gary-taubes-on-the-joe-rogan-experience/) with a list of arguments and relevant work he could point to where he had pre-answered potential counterarguments to his views. Imagine now trying to talk to either of them vs reading their writings, or writing a best evidence synthesis of what they are saying. It seems obvious reading will yield the superior understanding, but not necessarily a better answer and definately it will take longer.

### Analysis

My own text works out a concrete example, citing specific sources that inform my actual belief about salt consumption, whereas GPT3 goes on a generic overview of the pros and cons of reading reviews vs talking to experts, which my own blogpost already had done; and GPT3 should know because it was part of the prompt.

## How NEPA works ([link](https://constructionphysics.substack.com/p/how-nepa-works))

> **The NEPA process**

### GPT3

The NEPA process is triggered when a federal agency is planning a major action that will have significant impacts on the environment. The process begins with the preparation of an Environmental Assessment (EA), which is a document that assesses the potential impacts of the proposed action.

If the EA finds that the proposed action may have significant impacts, the agency must prepare an Environmental Impact Statement (EIS), which is a much more comprehensive document that assesses the potential impacts in detail and considers alternatives to the proposed action.

The EIS process can be quite lengthy, and often takes years to complete. After the EIS is completed, the agency will make a decision on whether or not to proceed with the proposed action.

If the agency decides to proceed, it must still follow the NEPA process by preparing a “finding of no significant impact” (FONSI) or a “record of decision” (ROD). The FONSI is a document that justifies the agency’s decision to proceed with the proposed action in light of the information contained in the EIS. The ROD is a document that outlines the agency’s decision and the reasons for it.

The NEPA process is intended to be open and transparent, and the public is encouraged to participate in the process by providing comments on the EIS. After the EIS is completed, the agency must consider all of the public comments before making a decision on the proposed action.

### Original

NEPA as it exists today has largely become a **procedural** requirement - NEPA doesn’t mandate a particular outcome, or that the government places a particular weight on environmental considerations \[0\]. It simply requires that the government consider the environmental impact of its actions, and that it inform the public of those considerations. NEPA doesn’t prevent negative environmental impacts, so long as those impacts have been properly documented and the agency has taken a “hard look” at them - as [one agency official described it](https://www.fs.fed.us/pnw/pubs/pnw_gtr799.pdf), “I like to say you can pave over paradise with a NEPA document.”

More specifically, NEPA requires that a “detailed statement” be produced describing any significant environmental impacts of “major” federal actions. “Major federal action” might be anything from:

-   A new policy or procedure (such as the Forest Service changing its [land management policy](https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/stelprdb5349141.pdf))
-   A new project (such as the Department of Energy constructing the [Yucca Mountain nuclear waste repository](https://www.energy.gov/nepa/downloads/eis-0250-final-environmental-impact-statement))
-   Directing federal dollars (such as the Federal Highway Administration [funding new highway construction](https://www.federalregister.gov/documents/2021/07/01/2021-14062/notice-of-intent-to-prepare-an-environmental-impact-statement-for-a-proposed-highway-project-in))
-   Issuing a license or permit (such as the FAA [issuing a launch license for SpaceX’s Boca Chica facility](https://www.faa.gov/space/stakeholder_engagement/spacex_starship))

Or anything else that could possibly have significant environmental impacts. In practice, little effort seems to be placed on determining whether an action qualifies as “major”, and anything that might have significant environmental effects in practice must be NEPA compliant.

### Analysis

On a first pass, there is nothing wrong with the GPT3 completion; Brian Potter talks about something different in the actual post, he starts with a high level conceptual argument for what NEPA is, making points that readers might be surprised to learn like "NEPA doesn’t mandate a particular outcome, or that the government places a particular weight on environmental considerations \[0\]. It simply requires that the government consider the environmental impact of its actions, and that it inform the public of those considerations.", then explains what major federal actions might be. Then Brian explains the EA->EIS->FONSI cascade, adding something that GPT3 did not consider: categorical exclusions. The paragraph where he does that is more directly comparable to GPT3:

> If an action can’t be categorically excluded, the next step for NEPA compliance is typically to figure out if the action will have “significant” environmental effects or not. If it’s unclear, an **environmental assessment** (EA) is performed, which is intended to be a high-level look at the proposed action to determine if the environmental impacts cross the “threshold of significance” and thus require a full environmental impact statement. If the EA finds no significant impacts, it issues a **Finding of No Significant Impact** (FONSI.)
> 
> EAs are generally more effort than categorical exclusions, though they also can vary significantly in the amount of effort required to create them.
> 
> If the EA concludes that the proposed action _will_ have significant impacts, the “detailed statement,” known as an **environmental impact statement** (EIS) is produced. An EIS describes the proposed action, the likely environmental impacts of that action, alternatives to taking the action (typically including ‘no action’), and plans for soliciting feedback from the public.
> 
> EISs have become long, involved analyses that take years to complete and are often thousands of pages in length. For instance, the most current EIS available on the EPA’s database (for a [Forest Service forest restoration plan](https://cdxapps.epa.gov/cdx-enepa-II/public/action/eis/details?eisId=374441)) comes in at 1294 pages (including appendices), and took over 6 years to complete. In the late 1980s, there was a \[minor government scandal\](https://books.google.com/books?id=kfgrAAAAIAAJ&pg=PA3&dq=gao+ssc+eis&hl=en&newbks=1&newbks\_redir=0&sa=X&ved=2ahUKEwiEjOL\_scv5AhX\_k2oFHTuwDeMQ6AF6BAgHEAI#v=onepage&q=gao ssc eis&f=false) when the Department of Energy spent $1.4 million printing and mailing 17,000 copies of the 8,000-page EIS for the Superconducting Supercollider (the statements weighed a combined 221 tons.)

The last paragraph is one that GPT3 seems to struggle with: giving concrete examples. I tried to feed GPT3 the entire text, up to and including "EISs have become long, involved analyses that take years to complete and are often thousands of pages in length. For instance, " and what I get is this (tried three times):

> **(1)** The EIS for the proposed Yucca Mountain nuclear waste repository was 5,600 pages long The EIS for the Keystone XL pipeline was 1,279 pages long The EIS for the Dakota Access Pipeline was 1,168 pages long
> 
> **(2)** The EIS for the proposed Yucca Mountain repository was over 10,000 pages The EIS for the proposed Keystone XL pipeline was over 1,000 pages The EIS for the proposed Boca Chica launch site was over 500 pages The EIS for the proposed Dakota Access Pipeline was over 1,000 pages The EIS for the proposed Atlantic Coast Pipeline was over 1,500 pages The EIS for the proposed Mountain Valley Pipeline was over 2,000 pages The EIS for the proposed Gulf of Mexico Outer Continental Shelf Oil and Gas Leasing Program was over 3,000 pages The EIS for the proposed Northern Border Pipeline was over 4,000 pages
> 
> **(3)** The EIS for the proposed Keystone XL pipeline was 1,147 pages long, and the EIS for the proposed Yucca Mountain repository was 8,600 pages.

The original text gets us pages, _time they take to complete_, which is very relevant information, and a key fact about the absurdity of the system, a least in the 80s. GPT3 gives us just page lengths that are not even consistent across runs.

## Meditations on Moloch ([link](https://slatestarcodex.com/2014/07/30/meditations-on-moloch/))

> And okay, this example is kind of contrived. So let’s run through – let’s say ten – real world examples of similar multipolar traps to really hammer in how important this is. \[followed by a number of them\]

### GPT3

The “tragedy of the commons” model: imagine a pasture open to all, and that grazing it excessively will destroy it. Each herdsman seeks to keep as many cattle as possible, and the only consequence he foresees is his benefit.

The example wasn't a little different, it was this one! Remarking that it didn't involve tragedy is odd in that in a very obvious way it is. In political action it is not an example of collective action; perhaps of a collective action -problem-

In the economics literature this is called the “tragedy of the commons”, although the original example was a little different and didn’t involve tragedy. In political science it’s called an example of 'collective action', although I don’t know if this is standard terminology.

Suppose there are a thousand herdsmen, each of whom has a herd of a hundred cows. If they all go out and graze their cows on the pasture every day, the pasture will be destroyed in a matter of days. But each herdsman only cares about his benefit. He doesn’t want to sacrifice his hundred cows to help preserve the pasture, since the pasture is open to everyone and he can’t reap the full benefit. He knows that if he doesn’t graze his cows, the pasture won’t be destroyed, but the other herdsmen will keep grazing, and he will get nothing. Each herdsman reasons like this, and the pasture is destroyed.

### Original

Capitalism. Imagine a capitalist in a cutthroat industry. He employs workers in a sweatshop to sew garments, which he sells at minimal profit. Maybe he would like to pay his workers more, or give them nicer working conditions. But he can’t, because that would raise the price of his products and he would be outcompeted by his cheaper rivals and go bankrupt. Maybe many of his rivals are nice people who would like to pay their workers more, but unless they have some kind of ironclad guarantee that none of them are going to defect by undercutting their prices they can’t do it.

Like the rats, who gradually lose all values except sheer competition, so companies in an economic environment of _sufficiently intense competition_ are forced to abandon all values except optimizing-for-profit or else be outcompeted by companies that optimized for profit better and so can sell the same service at a lower price.

(I’m not really sure how widely people appreciate the value of analogizing capitalism to evolution. Fit companies – defined as those that make the customer want to buy from them – survive, expand, and inspire future efforts, and unfit companies – defined as those no one wants to buy from – go bankrupt and die out along with their [company DNA](http://www.businessinsider.com/what-is-your-company-dna-2011-2). The reasons Nature is red and tooth and claw are the same reasons the market is ruthless and exploitative)

From a god’s-eye-view, we can contrive a friendly industry where every company pays its workers a living wage. From within the system, there’s no way to enact it.

_(Moloch whose love is endless oil and stone! Moloch whose blood is running money!)_

### Analysis

The example from GPT3 is not wrong and is thematically accurate: In the original post Scott is listing examples of tragedies of the commons, strangely without ever using the word _tragedy of the commons_; in fact the GPT3 example (pasture and overgrazing) was the same example that the very first tragedy of the commons [writing](https://en.wikipedia.org/wiki/Tragedy_of_the_commons) used to explain the concept. How does this example compare to the other few that Scott has? I'd summit that it is less interesting. Scott is trying to paint the concept from multiple points of view in multiple situations, and the tragedy of the commons completion from GPT3 seems to me too close to the fish farming story that comes earlier in the text. For completion, here is that entire section, with 13 examples, asking GPT3 to come up with a 14th example (as before, best of three attempts):

> Total war. In the early 20th century, all the major European powers were basically in the same situation as the rats in our Malthusian trap example. They were at the top of their game and could conquer the entire world if they wanted. But because they were all competing against one another, it was in their best interest to make sure that none of their competitors did conquer the entire world, which would give them an insurmountable advantage over everyone else. This led to a situation where everyone understood that if things got out of hand and everyone started conquering everyone else – known as “total war” – it would be a disaster for everyone. But since they were all competing against one another, they couldn’t really trust each other. So they prepared for the worst. And the worst happened. Now, from a god’s-eye-view, it’s easy for us to say things like “All these politicians should have just agreed that if any of them started a war, then everyone else would declare war on that country in turn.” And they did! They agreed! They just couldn’t trust each other to follow through.

## Set Sail for Fail: On AI Risk ([link](https://nintil.com/ai-safety))

> Turns out, there's plenty of risk just with these! The case for caring about AI risk (existential and otherwise) can and **should** be made without these sci-fi elements to reach a broader audience. One could then make a case for more risk by allowing for more sci-fi. In this post I will aim to explain how (4-6) alone are sufficient to design plausible scenarios where an AGI system poses various degrees of risk. I don't take "sci-fi" to mean impossible: Many commonplace inventions today were once sci-fi. I take sci-fi to mean inventions that lie in the future and that it's yet unclear how exactly they will pan out if at all. In particular I will assume the following:

### GPT3

-   The system is not given any goals that would obviously lead to human extinction
    
-   The system is not explicitly designed to be adversarial
    
-   The system does not have superhuman skills of manipulation or deception
    
-   The system is not able to self-replicate or create advanced nanotechnology
    
-   The system cannot improve its own intelligence (No recursive self-improvement or "intelligence explosion"
    
-   The system is not superintelligent (i.e. its intelligence is at or below that of the best humans)
    

I will not argue that these assumptions are likely to hold. I make them to be able to reason clearly about risks that might arise without these capabilities. I present them as a baseline for what might be possible with current technology. I expect that with future technology, some or all of these limitations could be removed, and this would increase the risks.

### Original

-   AGI will happen
    -   Artificial General Intelligence (AGI) will be developed at some point.
    -   This system will look familiar to us: It will be to some extent a descendent of large language models with some reinforcement learning[1](https://nintil.com/ai-safety#sidenote-1) added on top. It will run on hardware not unlike GPUs and TPUs.
    -   To sidestep certain issues that hard to resolve, we can take the system to lack consciousness, qualia, or whatever term you prefer for that
    -   I address the question of whether _general_ artificial intelligence is even a meaningful concept later
-   AI safety research has failed
    -   This is not to say that it will fail. The scenario I want to explore here is what happens if it does and a system is developed without any consideration about safe deployment. This way we can explore **AGI dominance** in isolation rather than reason about how alignment might work
    -   Most work in AI safety assumes that the system is given a benign or neutral goal (usually making paperclips) which then leads to discussions of how that goal leads to catastrophe. Here the aim is to study the capabilities of AGIs in the context of competing with humans for resources, so we assume an adversarial goal for simplicity. In particular, the system in this essay is assumed to be given the goal of making Earth inhospitable to life (Perhaps a system created by [negative utilitarians](https://en.wikipedia.org/wiki/Negative_utilitarianism)).
    -   One could also assume a [Skynet](https://en.wikipedia.org/wiki/Skynet_\(Terminator\)) scenario: that works as [well](https://clout.substack.com/p/ai-risk-is-like-terminator-stop-saying?s=w). Some AI risk researchers seem to dislike this scenario, but it is actually a [great example](https://www.slowboring.com/p/the-case-for-terminator-analogies).
-   Technological pessimists are right
    -   Advanced nanotechnology turns out not to be possible, beyond what current biology is capable of
    -   Broadly, there are no new scientific discoveries to be made that can radically empower an AGI. The weapons systems an AGI might design if asked to would look familiar to modern day engineers in the same way that yet-to-be-invented affordable hypersonic airplanes will look familiar to us.
    -   The system can't manipulate humans beyond what the most skilled human con-artist can do
    -   There is no recursive self-improvement. The system can improve itself somewhat, and scale with availability of hardware and data but largely the initial system is very close to the best that can be done

### Analysis

GPT3 does get some points right in that they are coherent with what the prompt; my post tries to assume away some superhuman capabilities and thinks about how AI systems could, even in that case, be problematic. However, to study this, as opposed to _why they might do this_ I explicitly make the assumption that the system has been given adversarial goals. That is, while most writing in the Internet about this topic is on _alignment_, my post is not about alignment at all! Alignment is assumed to have failed. I do not mention this fact in the prompt preceding the text, so GPT3 goes on to assume what most people would have assumed (that the system is not deliberately adversarial). This is a safe guess, but one that would not have led to an interesting post (there is a lot of writing about alignment already!).

This aside, GPT3 is again more scant in details and concrete cases.

## Aging is already solved in vitro. What comes next? ([link](https://nintil.com/aging-solved-in-vitro))

> Fixing cellular aging then leaves non-cellular forms of damage that would also need to be repaired. I don't know of any comprehensive catalogue of these, but off the top of my head we need to address as well:

### GPT3

-   Extracellular matrix cross-linking
    
-   Protein aggregation
    
-   Mitochondrial damage and dysfunction
    
-   Intracellular organelle damage (peroxisomes and lysosomes?)
    
-   DNA mutations
    
-   DNA epigenetic changes
    
-   Loss of telomeres
    
-   Loss of telomere maintenance mechanisms
    
-   Loss of cell-cell communication
    
-   Loss of cell-matrix interaction
    
-   Loss of cell cycle control
    
-   Loss of repair mechanisms (DNA, RNA, and protein)
    
-   Loss of cell-cell junctions
    

### Original

-   Arterial calcification ([Chelating agents?](https://pubmed.ncbi.nlm.nih.gov/23963635/) [Elastrin](https://www.elastrin.com/))
-   Cholesterol plaques ([Repair Bio](https://www.repairbiotechnologies.com/) and [Cyclarity Tx](https://cyclaritytx.com/))
-   Thymic and adrenal atrophy (Would reprogramming reverse this? There's the [Fahy](https://onlinelibrary.wiley.com/doi/full/10.1111/acel.13028) paper but we'd want to have something more robust ant targeted. I include this here because this is programmed, not due to damage)
-   Sarcopenia (Why does sarcopenia happen?) I include here also diseases where the issue is that cells die (We lose muscle fibers), as opposed to the same cell aging.
-   Cancer (Eventually we will all get it. DNA mutations seem to be unavoidable)
-   Hair graying (A more [mysterious](https://trevorklee.com/want-to-reverse-aging-try-reversing-graying-first/) topic than it seems!)
-   Various kinds of crosslinks ([Revel](https://www.revelpharmaceuticals.com/))
-   Shift in cell populations with age within tissues (As exemplified by the skew towards the myeloid lineage in [HSCs](https://nintil.com/immunosenescence))
-   Senescent cells (In theory they can be [reprogrammed](https://nintil.com/reversible-senescence/) too, or [removed](https://www.ardatherapeutics.com/))

### Analysis

GPT3 first gets a few things wrong: The list that is supposed to follow the prompt must have _non-cellular_ forms of aging. GPT3 mentions "loss of cell cycle control, DNA mutation, loss of telomeres, or DNA epigenetic mutations" which are forms of _cellular_ aging. GPT3 also missed _loss of cells_ and _shift in cell populations_ with age; these are different because if one is thinking about rejuvenation and there are no cells to rejuvenate (because they are dead) then you can't do anything to cells that do not exist anymore; or to restore tissue function you cannot just make cells young, the tissue composition must be restored as well, and this can be independent from making cells young again.

This aside, once again we have the specificity issue, GPT3 answer feels quite generic whereas mine is detailed and contains even a few surprising things like hair graying being a mystery.

> As far as we are aware, these questions have not previously been explored in depth. To make the questions more concrete, let's sketch a few specific examples of unusual social processes that could be (or are being) trialled today by adventurous funders or research organizations[3](https://scienceplusplus.org/metascience/index.html#fn3). These sketches are intended as brief illustrative examples, to evoke what we mean by "changed social processes". Though the examples are modest and conservative – indeed, some ideas may be familiar to you[4](https://scienceplusplus.org/metascience/index.html#fn4), though perhaps not all – versions scaled out across science would significantly change the culture of science. It's a long list, to emphasize the many diverse opportunities for imaginative change. Later in the essay we develop deeper ways of thinking that generate many more ideas for change.

### GPT3

-   **Crowdfunding with prediction markets**: Instead of having a small number of experts making funding decisions, allow anyone to contribute money to a project they think will be successful, and allow people to bet on the success of projects (perhaps with a small fee to reduce gaming of the system). Prediction markets are a well-studied tool for aggregating information from a large number of people8. When used to aggregate information about future events, they are sometimes called "idea futures". Many variations on this theme are possible: for example, you could use a more traditional crowdfunding approach, but allow people to bet against projects (or "short" them) as well as for them. Or you could use different types of markets: for example, you could use a " Vickrey auction "9 to auction off research projects to the people who are most confident in their ability to deliver them.
    
    There are fields one could point to as inspiration for Big Science like high energy physics, but definitely not CS or mathematics, which tend to feature teams on the smaller end, particularly mathematics!
    
-   **Focused granting:** Instead of having a large number of small grants, have a small number of large grants, each with a large team of scientists working on a focused problem. This is inspired by the success of focused, large-team approaches in fields such as mathematics and computer science10 . A variation on this theme is to have a single large grant that funds an entire research field for a period of time (say, five years). The idea is to allow the field to move faster by eliminating the need to constantly write new grants. This could work well for rapidly moving fields, but might not work so well for slower moving fields where it would be important to be able to course-correct.
    
-   **Hiring by lottery**: Instead of using the traditional hiring process, in which a small number of experts in the field select the best candidates from a large pool of applicants, use a lottery to select candidates. This would be especially useful for entry-level positions, where it's hard to predict who will be successful. The lottery could be open to anyone who meets basic criteria (for example, a degree in the relevant field). A variation on this theme is to have a lottery only for the final stage of the hiring process: after an initial screening to identify a small number of highly qualified candidates, select the final candidate by lottery.
    

These ideas are just a few examples of the many ways social processes could be changed. Many more possibilities exist. We'll return to this topic later in the essay. For now, let's turn to the question of how the culture of science changes.

### Original

-   **Century Grant Program[8](https://scienceplusplus.org/metascience/index.html#fn8):** Solicit grant applications for projects to be funded for 100 years. Done through an endowment model, the cost would be a small multiple of conventional 5- or 10-year funding. The point is to elicit an important type of intellectual dark matter[9](https://scienceplusplus.org/metascience/index.html#fn9): problems of immense scientific value that can't be investigated on short timelines. Inspired by seminal projects such as the CO2 monitoring at the Mauna Loa observatory, the Framingham Heart Study, and the Cape Grim Air Archive.
-   **Tenure insurance:** Tenure-track scientists often play it safe in the projects they take on. Encourage people to swing for the fences by offering a large payout if they fail to receive tenure. Supposing 80% of tenure-track faculty receive tenure[10](https://scienceplusplus.org/metascience/index.html#fn10), the cost for a large payout would only be a modest addition to an existing benefits package. A premium of $8k per year for 6 years, with a 5x multiplier and reasonable assumptions about interest rates, would result in a payout of over $300k. That's several years of pre-tenure salary in many fields and at many institutions. This suggestion is an instance of two more general patterns: (1) moving risk to parties who can more easily bear it, making the system as a whole less risk averse; and (2) a plausible way to increase people's ambition is to de-risk by improving their fallback options in the event of failure[11](https://scienceplusplus.org/metascience/index.html#fn11).
-   **Failure audit:** Many funders advocate high-risk, high-reward research, but this is often mere research theater, not seriously meant. For instance, in 2018 the European Research Council issued a self-congratulatory report claiming that: (a) they fund mostly "high risk" work; and (b) almost all the work they fund succeeds, with 79% of projects achieving either a "scientific breakthrough" or a "major scientific advance". If almost every project succeeds, then this is not a definition of "high risk" we recognize[12](https://scienceplusplus.org/metascience/index.html#fn12). To prove the seriousness of their intent about risk, funders could run credible independent audits on their grant programs, and if the _failure_ rate for grants persists _below_ some threshold (say, 50%), the program manager is fired[13](https://scienceplusplus.org/metascience/index.html#fn13). Or, at a different level of abstraction, the entire funder could be audited, and if the number of grant programs which fails is below 50%, the funder's director is fired.
-   **Acquisition pipeline for research institutes:** People often lament the loss of (or large changes in) great private research institutes past – PARC in the 1970s is perhaps the best modern example[14](https://scienceplusplus.org/metascience/index.html#fn14). If PARC-in-the-1970s was so great, why didn't the NSF acquire it? An acquisition would have been within their mission, and almost certainly a far-better-than-median use of NSF's funds[15](https://scienceplusplus.org/metascience/index.html#fn15). There might well have been political or bureaucratic barriers, but if so the problem lies in politics and bureaucracy, not in the merit of the idea. If public (or philanthropic) acquisition of private research institutes was common, it may incentivize the creation of more outstanding private research institutes.

### Analysis

The comparison between the original text and the GPT3 completion is particularly illustrative in this _Metascience_ example. The original text from Nielsen & Qiu list a number of interesting new institutions that have not been tried before. GPT3 completes with tried-and-tested institutions and reforms like open access, RCTs, or pre-registration. These ideas all make sense; they are not wrong, but they are definitely not novel if you are familiar with the metascience literature.

> What are the constraints of the activity of doing science?
> 
> Some things that come to mind:

### GPT3

-   The laws of physics
-   The limits of human abilities
-   The limits of technology
-   The nature of the questions being asked

There are probably more, but these are the ones that come to mind.

The laws of physics are the same everywhere and always. The limits of human abilities may change over time, but there are still limits. The limits of technology may also change, but there are still limits. And the nature of the questions being asked may also change, but there are still limits.

So, if we know the invariants of the problem at hand, we can make assertions about the best way to do science now and in the future.

For example, if we want to study a phenomenon, we need to be able to measure it. The better our measurements are, the better our understanding of the phenomenon will be. So, if we want to understand a phenomenon, we need to use the best measurements available.

If we want to understand a phenomenon, we need to use the best models available. If we have a better model, we can make better predictions. And if we can make better predictions, we can design better experiments.

So, if we want to understand a phenomenon, we need to use the best measurements and the best models available.

### Original

-   The underlying laws of nature remain unchanged. But the objects that science study have changed. These are not guaranteed to have timeless laws that we can discover. However, we can make some approximations: for example proteins evolve, and even across human populations one finds multiple variants. A protein we are studying may be radically different if we go enough back into the past, or enough forward into the future, at which point we may as well call it something else. But in shorter timescales, relevant to planning science funding, we can take this, and broadly biology in general, to have remained unchanged. The social sciences have it harder, but in this essay I am mostly thinking of the life sciences and material science.
-   The "rules of the game" have clearly changed: Science used to be done by [weird](https://en.wikipedia.org/wiki/Isaac_Newton) or [wealthy](https://en.wikipedia.org/wiki/Charles_Darwin) individuals working alone and now it's done by teams of individuals of modest income. In the past you could just fuck around and find out, now you need to apply for NIH grants, go through IRBs/IACUCs, publish the sort of papers that will get published in journals deemed prestigious and lead to tenure, etc.
-   Human abilities can be taken to be unchanged. The human brain has not radically changed the way it works in the last 500 years. The composition of the scientific workforce has changed, and so on average various traits will be differently represented amongst the current population of scientists vs that present in XVII century Britain.
    -   But we now have expanded those abilities: Now there's Google Scholar, online publishing, etc.

Invariants can be put together into invariant heuristics: If what makes for a great scientist or inventor is being creative, intelligent, hard working, able to work with others, etc, and furthermore if empirically those traits are distributed as a normal distribution, it follows that the greats will be an easily recognizable minority.

### Analysis

As before, there's the level of detail point; mentioning IRBs/IACUCs or Google Scholar is something GPT3 would be less likely to do. I did mention those in that way, rather than saying "institutional review boards" because IACUC in particular is a lesser known acronym (it applies to research with animals). Readers wanting to learn more can then search it. Had I written just IRB they wouldn't have come across this as easily.

GPT3 gives us reasonable statements like "The better our measurements are, the better our understanding of the phenomenon will be"

## We don't know how to fix science ([link](https://www.worksinprogress.co/issue/we-dont-know-how-to-fix-science/))

> A possible argument against this experimental approach is that for an experiment to be useful, there has to be a clearly defined metric of success. How would we know if any particular reform is actually making things better? Ideally,

### GPT3

Ideally, we would want a metric that captures progress on all the different dimensions that are important for science. But that is hard, if not impossible. We could instead use a more limited metric, like the number of papers published or the number of Nobel Prizes awarded, as a stand-in for progress. But that would be a mistake. The number of papers is a very noisy measure, as it is partly determined by how much grant money is available and how many people are working in a given field. The number of Nobel Prizes is even worse, as it is determined by a tiny number of very old white men who may not be the best judges of scientific progress.

This is a very simplistic take. It's very wrong to say that looking at Nobel Prizes is obviously worse that counting papers. Nobel Prizes are generally agreed to be well deserved. There are valid critiques of the Prizes as metrics to measure science, perhaps chiefly that they are not fractional enough and that there are not enough of them.

A better approach would be to use a set of diverse metrics, each of which is a imperfect measure of progress but which collectively give us a better sense of whether things are getting better or worse. For example, we could look at the number of papers, the number of Nobel Prizes, the average citation count of papers, the average number of papers authored by each scientist, the average time to first publication, the number of retractions, the number of studies that are not reproducible, and so on.

This experimental approach is not a cure-all, but it is a way to get closer to the truth about what works and what doesn’t. We need to try more things, and we need to be honest about what is working and what is not. Only then can we hope to make real progress.

### Original

Ideally, we’d like to measure the benefit provided by a study to society. We might ask: had this piece of research not been funded, would a given invention have been delayed? If so, for how long? And what was the impact of that on societal welfare? We could also try to estimate intermediate metrics of research usefulness, like whether a given basic research paper will end up being used as input to an application. It is an argument for humility, not epistemic nihilism.

But the difficulty is worth grappling with. In fact, it is one of the best arguments in favor of using lotteries as a major mechanism for allocating funding: even if we could see which piece of research is going to be successful (e.g., be highly cited), it is even harder to see if it will end up being useful. But while assessing the success of a specific scientist or proposal in the future is hard, it is easier to assess these mechanisms retrospectively. We can use a varied range of metrics to measure success, from citations (or variations thereof, like [field-adjusted citations](https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1002541), or counting only highly cited work), to the number of funded researchers that went on to get highly prestigious awards. We could even simply have peers evaluate portfolios of work without knowing which funding mechanism supported them, and have them decide which portfolios were best. To that end we could survey funded scientists to find out what they thought about the way their work was being funded.

### Analysis

Credit where it's due, with the exception of the remark about Nobel prizes, this completion is quite good, it does capture roughly what I wanted to say, that there are many metrics and each individually is problematic but that looking at a number of them at the same time holistically could be useful. As with other completions, GPT3 is more scant on concrete details than I am; I point for example to 'field-adjusted citations' and link to a relevant paper.

## Interview with Patrick Collison at Noahpinion ([link](https://noahpinion.substack.com/p/interview-patrick-collison-co-founder))

> **_And also: What do you think are the most important steps for government to take to speed progress along? Will reforming science funding, perhaps along the lines Ricon lays out, make a big difference? I've been calling for a big increase in federal science funding (the [Endless Frontier Act](https://www.young.senate.gov/newsroom/press-releases/young-schumer-unveil-endless-frontier-act-to-bolster-us-tech-leadership-and-combat-china) being advanced by Ro Khanna and Chuck Schumer). How much do you think we can expect from throwing more money at the problem? After all, federal spending on R&D is about half of what it was in the 70s and 80s, as a percent of GDP._**

### GPT3

There's a lot of debate about the causes of the "Great Stagnation" and I don't want to get too bogged down in that. I think it's enough to say that, if we're not making the progress that we should be, there's some combination of factors that we can change. And we should change them! (Notably, I think it's important to be clear that there are lots of different things that we might reasonably refer to as "progress" and it's important not to conflate them. Advances in the quality of life of the poorest people on the planet is progress. Advances in the quality of life of the richest people on the planet is progress. Advances in the average quality of life is progress. Advances in the median quality of life is progress. Advances in the quality of life of the poorest decile is progress. And so on. I think it's important not to privilege any particular metric or set of metrics because that can lead to suboptimal decision-making. For example, if we think that advances in the quality of life of the poorest people is the _only_ kind of progress that matters, we might well make decisions that have the effect of making the rich richer in order to make the poor richer, too. But if we think that advances in the quality of life of the richest people is the _only_ kind of progress that matters, we might make decisions that have the effect of making the rich richer while making the poor poorer. Obviously, neither of those is what we want!

GPT3 was probably going here for, perhaps, a Rawlsian argument or an egalitarian argument. The second part of the statement, that if we care only about quality of life of the rich we may make the poor poorer makes sense. Bt the first part doesn't make much sense. Under the moral view that only the welfare of the poor matters, it is perfectly valid to make the rich richer as a goal to make the poor richer

I think there are a lot of things that government can do to accelerate progress. One is to get better at funding science. (I think this is what you're alluding to with the Ricon piece.) The current system is very poorly designed. In particular, it's set up in a way that's almost guaranteed to produce a lot of waste. I think we should move to a system where the default is that scientists get a basic income and they can spend it however they want. (There are a lot of details to work out here... but the broad idea is that we want to reduce the overhead associated with research and development and give scientists more flexibility to pursue their ideas.)

I think another thing that government can do is to get better at regulating technology. We're not very good at it right now. (Think about self-driving cars... we're still not sure how to regulate them.) I think we need to get better at figuring out what the right rules are and then enforcing them.

I think government can also do more to support the development and deployment of new technologies. (I'm thinking here about things like basic research and infrastructure.)

And I think government can do more to promote competition. (Think about antitrust.)

There are lots of other things, too. But those are some of the most important, I think.

### Original

"Why is growth slowing?" is a pretty big question, and anything I say will mostly just be skimming the approximate contours of explanations that lots of others have sketched out in much more detail. But I'll give a few thoughts.

As a prefatory point, "why has progress been slow?" might be approaching things backwards -- maybe it's better to puzzle over "why is it ever fast?" or “why does it exist at all?”. The vast majority of human societies generate very little meaningful frontier progress most of the time! (As compared to Solow-style catch-up growth.) Lots of people have sketched out reasons as to why the midcentury picture in the US was perhaps anomalously _good_ and I think those stories probably all have truth to them. If I had to offer some hypotheses that tend to get a bit less attention, I'd throw out a few:

(1) **What's going on in science?** What we call "science" has changed immensely since WWII. For one thing, it’s way bigger. In 1950, the federal government gave about $100 million per year (in 2021 dollars) to universities for the purpose of basic research. That’s now more than $20B per year. If we look at R&D across the economy as a whole (though that's harder to measure reliably), nominal spending has supposedly grown by a similar magnitude. If we look at numbers of researchers, same story: about 10x more research doctorates are awarded every year today than were in 1950. And how we do it has changed. For example, peer review in the modern -- legitimacy-conferring -- sense of the term is a postwar invention and arose more as a result of funding bureaucracies and controversies than any scientific exigency. (Einstein was very offended in 1936 when the editor of _Physical Review_ shared his now-famous EPR paper for an external opinion.) Lots of people have documented (and decried) the increasingly gerontocratic aspects of modern science and the [growing age](https://marginalrevolution.com/marginalrevolution/2019/01/ratio-50-scientists-funded-nih.html) of grant recipients. However you cut it, what we're talking about when we say "science" just isn't close to the thing it was seventy years ago.

### Analysis

In the Patrick Collison interview, the GPT3 completions are, at a high level, answers that Patrick Collison would give (Better regulation? Sure, supporting the development of new technologies? Sounds ok I guess). But the actual Collison sounds more interesting. There are concrete numbers as anchor points, the points made take longer to develop. GTP3 throws around a bunch of suggestions in short snippets whereas real-life Patrick spends entire paragraphs on single ideas ("What's going on in science", "How has culture changed?")

## Accounting for mode collapse

[Janus](https://www.lesswrong.com/posts/t9svvNPNmFf5Qa3TA/mysteries-of-mode-collapse-due-to-rlhf) (at Conjecture) has reported that the latest GPT3 model (`text-davinci-002`), the one I used for the completions above, tends to produce more deterministic or less interesting completions than baseline models that have not been fine-tuned with reinforcement learning. To be fair to GPT3, I re-ran the prompts above with the baseline `davinci` model. As before, T=0.7, tried 3 times each. `text-davinci-002` was better in all cases for this particular task.

## Appendix: Generating the title of this blogpost

Images and Words refers, of course, to Dream Theater's 1992 album, obviously chosen as a reference to the two main areas where AI is been applied these days, language and images. You can go listen to one of the songs [here](https://www.youtube.com/watch?v=K0najyrwX6c).

![Images and Words - Wikipedia](https://upload.wikimedia.org/wikipedia/en/8/8d/Dream_Theater_-_Images_and_Words.jpg)

I tried to generate a series of images that look like this one using DALLE but didn't get quite there. The pictures look good enough but not quite there:

![image-20221019163736752](https://nintil.com/images/2022-10-19-interesting-ai-models/image-20221019163736752.png)

A heart in flames floating in a room. A girl in a white dress stands in the middle looking scared at the heart. There is a bed with red curtains in the background. It's late at night. Realistic, in the style of progressive metal album cover art

Having come up with the idea, I then tried to use GPT3 as well to generate a clever title for this post. "Clever" is of course, relative. I like Dream Theater, and I like to reference my interests here and there in my blog. Someone else could have chosen something else. When I started writing this post I did not know it was going to be called _Images and words_, its working title was _interesting AI models_ which I just chose as a placeholder. I couldn't have just asked GPT3 for a nice title in the abstract referencing Dream Theater because first I would have to know I wanted to write about models dealing with images and words. Guessing that I would be doing this is not far-fetched, after all LLMs and diffusion models are what AI is all about these days. An obvious prompt doesn't get us that, but after some iteration I managed to get one that gets me this title. But as you can see I had to heavily nudge the model towards this by mentioning a key fact about me (That I like DT) and then asking the model to enumerate first the DT albums. But it could have been something else. It's unrealistic for me to list all facts about myself in a prompt (the context window is not long enough), but I can imagine models in the future learning in the background about me

| Prompt | Result |
| --- | --- |
| There has been a lot of recent work in AI using large language models and diffusion, to generate text and images, respectively. I am writing a post to think about these recent developments. One band I like is Dream Theater. I want to generate a clever sounding title for the post that references an album from a band I like and the content of the post simultaneously. | 1) "Text generation with large language models: a case study with Dream Theater" 2) "Diffusion-based image generation: a case study with Dream Theater" 3) "Generating text and images with diffusion: a case study with Dream Theater" 4) "Using large language models and diffusion to generate text and images: a case study with Dream Theater" |
| There has been a lot of recent work in AI using large language models and diffusion, to generate text and images, respectively. I am writing a post to think about these recent developments. One band I like is Dream Theater. I want to generate a clever sounding title for the post that references an album from a band I that at the same time is informative of the content of the post. | 1) "Achieving new levels of creativity with AI: A case study with Dream Theater" 2) "Using AI to create art: A case study with Dream Theater" 3) "How AI is changing the face of art: A case study with Dream Theater" 4) "Dreaming up a new future with AI: A case study with Dream Theater" |
| There has been a lot of recent work in AI using large language models and diffusion, to generate text and images, respectively. I am writing a post to think about these recent developments. One band I like is Dream Theater. I want to generate a clever sounding title for the post that references an album from a band I that at the same time is informative of the content of the post. First, list all the albums that Dream Theater has released. Then pick the most appropriate of the album names as a title for the blogpost described earlier, explaining why: | 1) "Images and Words" 2) "Awake" 3) "Falling Into Infinity" 4) "Metropolis Pt. 2: Scenes from a Memory" 5) "Six Degrees of Inner Turbulence" 6) "Train of Thought" 7) "Octavarium" 8) "Systematic Chaos" 9) "Black Clouds & Silver Linings" **"Images and Words" would be a good title for the post** because it would be a reference to the band Dream Theater, and the album title is also informative of the content of the post, as it is about AI using large language models and diffusion to generate text and images. |

## Appendix: Ideas for AI startups

Here are some, listed in no particular order. These are all product startups (as opposed to platforms)

-   Read a bunch of code, and perhaps information about a company, return a list of vulnerabilities
-   AI-powered meme generator. Type \[galaxy brain meme\] (vibes, vibrations, FFT) and you get a picture of said meme
-   Natural langauge-to-command-line. This is more a feature than a company, and is already on the works, at [Warp](https://www.warp.dev/)
-   Nintil as a service :). Wouldn't it be nice if I can at last take a break from writing all these blogposts and just ask a model to generate them for me? Alas, I don't think we're there to bother trying.
-   Generate O1 visa recommendation letters programmatically. I guess if one can build a $40M ARR company on top of GPT3 despite the fact that one can just use GPT3, there might be room for a company to do this. Small market though.
-   Take a text, generate Anki cards from it \[Andy Matuschak reportedly tried this, didn't work that well?\]
-   Meeting summarizer: Record speech during a meeting, then summarize the points people made, disagreement, action items, etc. \[Reportedly someone is already working on this?\]
    -   This, but it's shown during the meeting, as a reminder of what has been discussed so far
-   AI-augmented writing tool: Text continuations, rephrasing, but also showing potentially useful material in a sidebar
-   These could be combined: You could imagine having a model listening to what you say, hear, and see, and present documents relevant to that to you. If you are talking about 'the budgets we made a few weeks ago' it could have that file being displayed so you don't even have to try search it. You may even be presented with files you didn't know you wanted but are glad to be shown!

## Acknowledgments

To Miles Brundage, Steven Malina, Ivan Vendrov, Sean Linehan, Niccolò Zanichelli, and everyone that I forgot and gave me feedback :)

## Changelog

-   2022-11-17: Added paper from Zhou et al. on arithmetic
-   2022-12-29: Fixed precise N of frames in the movie example (from [here](https://twitter.com/MazeDaMouth/status/1608699838102929409))

_Ricón, José Luis, “Images and Words: AI in 2026”, Nintil (2022-11-13), available at https://nintil.com/interesting-ai-models/._