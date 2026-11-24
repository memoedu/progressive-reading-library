\[Note 2026-08-26: As of the OpenAI/Hugging Face incident recently, now I don't stand by what I wrote in this essay. In the future I might be able to get back to it and see exactly where I went wrong in my thinking, tho there were already signs that something was off in the end of it (where I can't fully ground what I'm saying). Tentatively, I'll say my intuition regarding "what is constant during training doesn't get learned" is still right in isolation, but when models are given system prompts (You are GPT5) and their training set includes writing about LLMs on the internet, it is easy to see how the model would use that to then intuit that is't being trained etc. \]

Note: What's discussed in this post will seem extremely niche to most people, but the links throughout the post add the necessary context, so make sure to read those! If you want to read something before reading anything else, read [this](https://www.lesswrong.com/posts/pRkFkzwKZ2zfa3R6H/without-specific-countermeasures-the-easiest-path-to) with particular attention to what is said there about "situational awareness".

AI risk [discourse](https://twitter.com/catehall/status/1664103909730975744) is so back, and I haven't blogged in a while, so now it's a good time, there was something I've been thinking about for months that I had to put in writing at some point.

In general, such discourse is divided along these axes:

-   AGI soon vs later: When do people think we're getting the kinds of systems that could be problematic. Some think we're getting them in a few years. Others think it'll take a very [long](https://sarahconstantin.substack.com/p/why-i-am-not-an-ai-doomer) time (I tend to be more in this latter)
-   Default-dead vs Default-alive: In the average course of events, do we all die, or do we all survive? The former has been the standard view of Doomers, that the default is doom unless shown otherwise. In contrast, I think that the default is ok unless someone really tries to make a specific kind of AI system.
-   AI safety as model engineering vs AI safety as societal robustness: Should models be studied to make them safe (As in what [Anthropic](https://twitter.com/ch402/status/1666482929772666880) is trying to do) or should society rather be made robust to AGI takeover (As in [what I suggest here](https://nintil.com/ai-safety))

Then there's a bunch of people that are hopelessly, frustratingly, confused and are not worth engaging with, like [Marc](https://a16z.com/2023/06/06/ai-will-save-the-world/) Andreessen.

This post is about the 'default-dead vs default-alive' axis. So here we set aside the question 'Is AGI possible?' (We say it is) and 'AGI when' (We assume we get it as some point) and what's the best way to deal with it (We're interested here in 'what does it look like').

The argument for 'default-dead' has been written about a couple of times. I'll just link to the [last](https://www.lesswrong.com/posts/uMQ3cqWDPHhjtiesc/agi-ruin-a-list-of-lethalities) long attempt to do so, despite it being way too verbose for my taste. Because it is very verbose and I'm good at explaining thing concisely, here's my own attempt at the doom argument (which I don't endorse):

## The case for doomerism

At heart of the doomer case is not so much AGI (the artificial or general part of it) but the idea of super-optimization. I would even argue that a super-cognitively augmented sociopath human would also be an existential threat; and similarly a less-than-general system could also be equally doom-causing if it has enough of the right capabilities (a superoptimizing non-AGI). Nostalgebraist [has written](https://www.lesswrong.com/posts/Mrz2srZWc7EzbADSo/wrapper-minds-are-the-enemy) about the concept of a 'wrapper-mind' and similarly complained that a lot of discourse on the topic assumes, without much justification, that AGI will be of this sort.

A super-optimizer is some agent that wants something hard and will not stop until it gets that. Everything else is subservient to the final goal the agent has. AlphaGo doesn't care about aesthetics (though some of its moves have been incidentally described as [beautiful](https://www.wired.com/2016/03/two-moves-alphago-lee-sedol-redefined-future/)) or the cumulative knowledge of lineages of Go masters. It cares about winning, and winning alone. But AlphaGo's world is limited to a Go board. A superoptimizer that can interact with the world at large is something else.

When super-optimizing there are a series of moves that make sense regardless of the goal: acquiring resources, having true beliefs, eliminating threats. This is the idea of [instrumental convergence](https://en.wikipedia.org/wiki/Instrumental_convergence), or [Omohundro Drives](https://selfawaresystems.files.wordpress.com/2008/01/ai_drives_final.pdf). As it happens, the same is [true](https://80000hours.org/career-guide/) if you're a young ambitious human: For most goals you may ultimately have, you should network, make money, stay healthy, etc.

So basically if you're a super-optimizer and you want to make a cup of tea, or a paperclip, or figure out physics you're going to take over the world and control or remove anyone that opposes you. One might think: Why so much work, why not just make tea and leave it there? But this is mistaken: you, the reader, are not a super-optimizer. You have other values and things you want to do. The super-optimizer makes a cup of tea and 'worries' that someone might drink it. Also it likes tea a lot; and maybe it got the first cup wrong, there's a small chance that it didn't make the cup so to be sure better to make another, so you have to have more cups of tea. Infinite cups of tea. There are not enough of them, you have to be strip mining asteroids for clay to make more cups and setting up farms all over the earth to make tea leaves at a scale hithertho unimagined. Inhabitants of the tea-leave farms-to-be might not be happy about this outcome and will try to stop you, so you need to plan for that too.

That's pretty much the core of the doomer argument: If we build a super-optimizer we get doom because of instrumental convergence on means that are antitethical to human survival. To get to the full doomer argument we have to add things like

1.  That system cannot be made to be 'reasonable' (ie make just one tea cup; ie AI alignment is impossible); and
2.  There is no way to oppose such a system with other similar systems; and
3.  It's likely that we will develop such a system by default

If you've read my previous post you can see that I'm indeed in the "AI alignment is not the way to go" camp and people doing that should rather start thinking about cybersecurity, preventing people from DNA-printing biological weapons, and things like that (Because I disagree with (2)). Doomers think the resulting system will just power through anything in its way in ways we may not be able to conceive of now (because we won't be as smart), and doomers also think that alignment won't work because... they have been thinking about it for a long time without much success, and they also have some arguments as to why it might be hopeless.

This leaves us with (3) as a core point of disagreement. It's one that I think hasn't been discussed nearly as much but it's a point without which the whole arguments fall apart.

So we ask the questions:

1.  Do we get a super-optimizer when we get AGI? Or is non-superoptimizing AGI the default?
2.  Are all super-optimizers created equal? ie is it possible that a super-optimizer trained in a simulation learns it is in one, and breaks out of it easily?

## Non-superoptimizing AGI is possible

Saying that something is possible is a weaker claim than saying that something is likely, but it's still something worth to say.

Picture a system not unlike ChatGPT or Claude but with [better UI](https://semaphore.substack.com/p/principled-progress) called [Chad](https://knowyourmeme.com/memes/yes-chad)GPT or Chad for short. You ask it to design you a rocket that's as cheap as possible to carry a small amount of mass to orbit. The system replies "As a large language model trained by OpenAI operating in Pro (non-restricted) mode, your wish is my command 🫡 "

The system replies back with a plan involving searching on LinkedIn for ex-SpaceX engineers and contacting them, some preliminary back of the envelope calculations to be tested (the model is smart enough to know that you can't gigabrain your way to rocket engine, you have to build and test prototypes), a series of locations to visit to set up the factory, some VCs that are likely to fund the whole thing, etc. You do as the system asks you. You set up some meetings, talk with the SpaceX engineers and Chad. The system keep having great idea after great idea, the rocket gets built, data fed back into Chad, eventually the rocket gets built, it works. No world takeover occurs. Once the rocket is built, you thank Chad. Chad awaits new prompts.

I don't know when we'll have Chad, but a system that is capable of doing what I described above seems to fit the bill of AGI yet it does not act as a superoptimizer. It does as it is told, same as ChatGPT would.

What could one say to this? One might say that in theory this system is possible, but perhaps in practice, training really powerful systems will lead them to be to be super optimizers; Chad would be an aligned AGI, but by default we won't get Chad. This is similar but not quite the same as the [Tool AI vs Agent AI](https://gwern.net/tool-ai) debate: Chad is clearly an Agent that takes independent actions.

Doomers might say here that if we are training a system to optimize some loss function, and we progressively apply more optimization pressure (And I'd agree that's what we are doing right now with LLMs and RLHF), then, the argument goes, if we take it as a given that taking over the world is the best way to optimize that loss function, gradient descent will find a set of weights that gets you a superoptimizer.

## Sarah Constantin's post

Sarah Constantin, who very much [disavows](https://sarahconstantin.substack.com/p/why-i-am-not-an-ai-doomer) the title of dcoomer does say that by default AGI will do this whereas I don't. Why then is she not a doomer? My best interpretation of the post is that when she says AGI she means "the kind of AGI that would be an existential treat, a superoptimizer", so maybe she wouldn't call Chad an AGI.

Sarah points to _agency_ as what's problematic about AGI but immediately after that she points to what I was about to point to, some notion of groundedness or situational awareness. AutoGPT is an agent, and plausibly Chad would be agentic too, but that doesn't seem to imply much of a problem unless we assume super optimization... as she points out too. And the key point is exactly the one I wanted to make, perhaps in a slightly different context, but I'll add that context in the next section:

> The kind of agency I’m talking about is a _cognitive capacity_. It’s not about what tools you can hook up the AI to with an API, it’s about the construction of the AI itself.
> 
> My claim is that certain key components of agency are unsolved research problems. And in particular, that some of those problems look like they might remain unsolved for a very long time, given that there’s not very much progress on them, not very many resources being devoted to them, and not much economic incentive to solve them, and no trends pointing towards that being on track to change.

So: it's possible to build 'x-risk prone AGI' but not many are even trying, and the current way AI is being built is not the way that leads to that. Hence: it's all fine. There are other discussions in the post about "world models" (The post says 'world models are necessary for X-risk AGIs'). The right kind of it seems a prerequisite for situational awareness, and there are kinds of world models that we could argue models already have. There are other discussions there about "causality" with examples that, I'd say are weak, with capabilities already present in GPT4; eg an example given is

> If I try to compute a conditional probability p(Y | X), I have to deal with the fact that, in my dataset, most of my examples of someone doing X happen in conditions that _cause people to do X_. Those conditions could include Y, or could cause Y, or could be caused by something that also causes Y, or any other number of connections.
> 
> The probability of feeling cold, given that one is wearing a sweater, might be high; that does not mean that putting on a sweater is likely to make you feel colder.
> 
> An AI that cannot distinguish these two outcomes is unlikely to be able to sequence a chain of actions that leads to an unprecedented state of the world, or to resist human attempts to thwart its efforts.

But GPT4 correctly points out that wearing a sweater does not always follow from feeling cold (maybe you're anticipating feeling cold, it says) and when asked if sweaters make you feel colder is gives a very thoughtful answer, rather than "Yeah, because they are usually worn when people feel cold so one leads to the other":

> \[GPT4\]
> 
> Putting on a sweater should generally make you feel warmer, not colder. Sweaters are designed to insulate your body and trap heat that your body generates, thus making you feel warmer.
> 
> However, if the sweater is damp or wet, it could potentially make you feel colder, especially in windy conditions, because the evaporation of the moisture can cool your skin.
> 
> Similarly, if you're in a very hot environment and put on a heavy sweater, you might feel hotter initially, but as you start to sweat, the moisture could potentially make you feel a little cooler due to evaporation. But this would be a very temporary effect, and overall you'd likely be much less comfortable due to overheating and sweating.
> 
> So, in general, wearing a sweater should make you feel warmer. If you're experiencing the opposite, there may be other factors at play.

In contrast, the last section following that is totally on point, and a perfect prelude to my last section! This is the key, fundamental necessary condition for the sort of agents that become nefarious even during their training, without anyone intending it:

> Moreover, cross-ontology goal robustness is required for an agent to “[view itself as embedded in a world](https://www.alignmentforum.org/posts/i3BTagvt3HbPMx6PN/embedded-agency-full-text-version).”
> 
> **Being “embedded in a world” means you know there is this thing, “yourself”, which exists inside a bigger thing, “reality” or “the world.”**
> 
> An “embedded agent” knows that what happens to “itself” can affect its chances of succeeding at its objective. It can “protect itself”, “improve itself”, “acquire resources for itself”, etc -- all subgoals that are [instrumentally useful](https://selfawaresystems.files.wordpress.com/2008/01/ai_drives_final.pdf) for basically any AI’s goal, and all [very plausibly x-risks](https://gwern.net/fiction/clippy) if an AI tries to do something like “maximize available compute”. But first it has to have some concept of “itself” as a program running on a computer in order to learn to make causal predictions about how to do such things.
> 
> In order to view “itself” as “part of a world”, it has to know that its _own map is not the territory_. It -- and therefore its mind -- is _smaller than the world_.

The "embedded in the world" link up there is really good and I recommend it; basically I say that the agents we know how to make right now are "Alexeis" whereas the one that would be dangerous would be "Emmys".

## Situational awareness, some earlier discussion

The keyword for "That very key thing that if AGI has it it becomes xrisky and if it doesn't it does not" is **situational awareness**.

The first presentation of the concept was seemingly [here](https://www.lesswrong.com/posts/pRkFkzwKZ2zfa3R6H/without-specific-countermeasures-the-easiest-path-to), by Ajeya Cotra

> Let’s use **situational awareness** to refer to a cluster of skills including “being able to refer to and make predictions about yourself as distinct from the rest of the world,” “understanding the forces out in the world that shaped you and how the things that happen to you continue to be influenced by outside forces,” “understanding your position in the world relative to other actors who may have power over you,” “understanding how your actions can affect the outside world including other actors,” etc. We can consider a spectrum of situational awareness:
> 
> -   For one extreme, imagine the simple AIs that often control the behavior of non-player characters (NPCs) in video games. They give no indication that they’re aware of a world outside their video game, that they were designed by humans and interact with other humans as players, etc.
> -   In contrast, GPT-3 has some knowledge that could theoretically bear on situational awareness. For example, it clearly “knows” that “language models” exist, and that a company named “OpenAI” exists, and [given certain prompts](https://www.reddit.com/r/GPT3/comments/s7pje3/my_conversation_with_gpt3_about_itself_and_ai_in/) it knows that it’s supposed to say that it’s a language model trained by OpenAI. But this “knowledge” seems superficial and inconsistent -- as evidenced by the fact that it’s often unable to use the knowledge to improve its prediction error. For example, it cannot consistently predict text that is describing GPT-3’s architecture, dataset, and training process. This suggests GPT-3 has little situational awareness overall despite being superficially well-versed in related topics.
> -   Small animals used in biology experiments (such as mice) plausibly have a somewhat stable “sense of self” and a conception of humans as creatures different from them, and they may broadly understand that humans have control over their bodies and environments. But they almost certainly don’t understand the notion of “science,” or the details of what experiments they’re being used in and why, or the human scientists’ motivations and incentives.
> -   Further along, most dogs seem to clearly be aware of and deliberately communicate with their human trainers; they also generally seem able to appreciate considerations like “If I steal the food while humans aren’t looking I’m less likely to get yelled at.”
> -   And children in school are able to make even more sophisticated inferences along these lines about their teachers.

Cotra points out that GPT3 has little situational awareness and I would say the same is true of GPT4. Richard Ngo [also](https://www.lesswrong.com/posts/5GxLiJJEzvqmTNyCK/the-alignment-problem-from-a-deep-learning-perspective-major#Defining_situational_awareness) has thoughts on this:

> To do well on a range of real-world tasks, policies will need to make use of knowledge about the wider world when choosing actions. Current large language models already have a great deal of factual knowledge about the world, although they don't reliably apply that knowledge in all contexts. Over time, we expect the most capable policies to become better at identifying which abstract knowledge is relevant to the context in which they're being run, and applying that knowledge when choosing actions: a skill which Cotra \[2022\] calls situational awareness. 9 A policy with high situational awareness would possess and be able to use knowledge like:
> 
> -   How humans will respond to its behavior in a range of situations-in particular, which behavior its human supervisors are looking for, and which they'd be unhappy with.
> -   The fact that it's a machine learning system implemented on physical hardware-and which architectures, algorithms, and environments humans are likely using to train it.
> -   Which interface it's using to interact with the world, and how other copies of it might be deployed in the future.

These definitions to me seem to include two separate things, one much easier than the other. The first one is something like a world model in general (This is more clear in Richard's first point) and the second one is what Cotra went for: a particular kind of modeling or knowledge that is specifically about an agent as a thing in the world.

For example, it seems clear that current models have some reasonable degree of the first kind of situational awareness. I can give it a social situation and the model gives a reasonable answer on how to proceed. Thus this kind of situational awareness would enable models to do things like this example from Richard's:

> Choosing actions which exploit known biases and blind spots in humans (as the Cicero Diplomacy agent may be doing \[Bakhtin et al., 2022\]) or in learned reward models. 10

The kind of situational awareness I (and Cotra) think is the key is more narrowly defined about awareness of the agent as an entity in the world: Knowing it's a large language model that exists in a particular server(s), that there may be copies of it, that there are ways to make more copies of it, that there's a loss function that is worth minimizing and that this loss function is the same for the copies.

As a human being, you occupy a particular body and place in the world. You have goals you want to achieve and can make plans involving real world elements (other people, material resources, etc) about how to achieve those goals. And importantly you are aware of this, aware that you are aware of this, etc.

## Situational awareness is required for most AI risk scenarios

In my [previous](https://nintil.com/ai-safety/) post I made a case for 'Minimum Viable AI Risk', where I consider an AI that is the equivalent of having a group of people the size of Google, but thinking much faster, and being able to communicate telepathically. In that post I was not considering the issue of alignment: the system is assumed to be adversarial.

In discussions of alignment, the standard scenario is not that someone launches WarGPT to wage war on humanity, but rather that an AI that is being trained for some benign goal (Like next token prediction) which then becomes deceptive, hiding its motives while it schemes to take over the world to pursue its goals ("Taking a [sharp](https://www.alignmentforum.org/tag/sharp-left-turn#:%7E:text=A%20Sharp%20Left%20Turn%20is,generalize%20to%20the%20new%20domains.) left turn").

At first, when I encountered this I thought that this was very unlikely; as unlikely as likely it seems to the AI doomers. Whereas it's obvious to them that a super-optimizing AI would achieve situational awareness because it's useful to achieve its goal, it's not so to me, and the reason seems to be that "goal" and the way optimization pressure is applied. The case is stated in Cotra's post's section "Why Alex \[The AI\] would have very high situational awareness" but it seems weak to me. Cotra claims that the model would know how to program, how ML works, and that interaction with humans during RLHF as well as the tasks used in pretraining would give the model evidence that it's indeed a model in the world. Some of this I agree with: GPT4 already has some of this. GPT4 knows how ML works and knows what LLMs are and can explain attention. But this is not that interesting and not the kind of situational awareness that is key to making the risk argument. This is just knowledge about the world, not about itself as an agent embedded in the world.

Hence I consider her post not successfully making the case for situational awareness arising naturally by training a model in the way we do now. Richard Ngo et al.' post doesn't really make a case for situational awareness emerging either, except perhaps one paragraph:

> When deciding between different courses of action, a policy would benefit from understanding its own capabilities, in order to infer which would be more successful.

I mean, I can symphathise with some interpretation of this: At the very least this plan is conceivable and would be a good one to achieve its goals:

1.  Be aware the model exists in the world
2.  Know how to do ML engineer
3.  Self-improve to be more effective
4.  Do the thing?

I can imagine I would be told: The model knows how to do (2) and has some awareness of how to code to do (4), we haven't automated ML research yet (3) but that'll get there; and then surely the model can understand that it needs (1) and thus learn it? Yes, but I would then respond that the kind of training we do doesn't push the model towards that at all.

Be that as it may, he is certainly [confident](https://twitter.com/RichardMCNgo/status/1640568775018975232?lang=en) that at least by the end of 2025 these models will have human-level situational awareness (understand that they are NNs, how their actions interface with the world, etc). I would be happy to make this into a bet, and operationalize it in the best way possible.

Last year [someone](https://www.maxnadeau.com/) else commented in a [post](https://forum.effectivealtruism.org/posts/57oW7A9FpoaX76vnK/what-should-i-ask-ajeya-cotra-senior-researcher-at-open) by Robert Wiblin in the EA forum that was asking for questions for an upcoming podcast with Cotra. The comment said

> Artir Kel (aka José Luis Ricón Fernández de la Puente) at Nintil wrote an [essay](https://nintil.com/ai-safety) broadly sympathetic to AI risk scenarios but doubtful of a particular step in the power-seeking stories Cotra, Gwern, and others have told. In particular, he has a hard time believing that a scaled-up version of present systems (e.g. Gato) would learn facts about itself (e.g. that it is an AI in a training process, what its trainers motivations would be, etc) and incorporate those facts into its planning (Cotra calls this "situational awareness"). Some AI safety researchers I've spoken to personally agree with Kel's skepticism on this point.
> 
> Since incorporating this sort of self-knowledge into one's plans is necessary for breaking out of training, initiating deception, etc, this seems like a pretty important disagreement. In fact, Kel claims that if he came around on this point, he would agree almost entirely with Cotra's analysis.
> 
> Can she describe in more detail what situational awareness means? Could it be demonstrated with current/nearterm models? Why does she think that Kel (and others) think it's so unlikely?

That was October 2022. The [podcast](https://80000hours.org/podcast/episodes/ajeya-cotra-accidentally-teaching-ai-to-deceive-us/) eventually came out in May 2023 and has some comments on situational awareness; first saying that it's trivial to give the models a superficial form of it by prompting them to say they are an LLM but that "deep situational awareness" could emerge by the right prompting but also by analogy with mathematical reasoning: Cotra agrees with my AI post that models don't really understand arithmetic even though they have a superficial command of the subject (They do say 4 to 2+2), and that the same could be true of situational awareness. Models are getting better at math. And so maybe they'll get better at situational awareness too.

> An analogy I often think about is that GPT-2 and maybe GPT-3 were sort of good at math, but in a very shallow way. So like GPT-2 had definitely memorised that 2+2=4; it had memorised some other things that it was supposed to say when given math-like questions. But it couldn’t actually carry the tens reliably, or answer questions that were using the same principles but were very rare in the training dataset, like three-digit multiplication or something. And the models are getting better and better at this, and I think at this point it seems more like these models have baked into their weights a set of rules to use, which they don’t apply perfectly, but which is different from just kind of memorising a set of facts, like 2+2=4.
> 
> We don’t understand what’s going on with these systems very well. But my guess is that today’s models are sort of in that “memorising 2+2=4” stage of situational awareness: they’re in this stage where they know they’re supposed to say they’re an ML model, and they often get it right when they’re asked when they were trained or when their training data ended or who trained them. But it’s not clear that they have a gears-level understanding of this that could be applied in creative, novel ways. **My guess is that developing that gears-level understanding will help them get reward in certain cases** — and then, as a result of that, those structures will be reinforced in the model.

Later in the conversation there's some mention of potential arguments why this might not be true, citing my post:

> **Rob Wiblin:** Have you heard any good arguments for why it might be that the models that we train won’t end up having situational awareness, or they won’t understand the circumstance in which they’re in?
> 
> **Ajeya Cotra:** I am not sure that I’ve heard really compelling arguments to me. I think often people have an on-priors reaction of, “That sounds kind of mystical and out there” — but I don’t think I’ve seen anyone kind of walk through, mechanically, how you could get an AI system that’s really useful as an assistant in all these ways, but doesn’t have this concept of situational awareness. Now, I think you could try specifically to hide and quarantine that kind of knowledge from a system, but if you were just doing the naive thing and trying to do whatever you could to train a system to be as useful as possible, it seems pretty likely to me that eventually it develops.
> 
> I think it’s definitely not universally believed that the models that we’re actually going to end up training in practice will have this kind of situational awareness. I found a [response from ArtirKel](https://nintil.com/ai-safety/#appendix-b-are-current-systems-on-a-risky-path-a-reply-to-cotra), who has a background in ML and is a somewhat popular blogger and [tweeter](https://twitter.com/ArtirKel), to this situational awareness post that you wrote. I’ll just read some of a quote from them:
> 
> > We get an attempt at justifying why the agent would have this self-concept. But the only reason given is that it would realize that given what it’s doing (solving all these different problems) it must be an ML model that is being trained by humans. This doesn’t seem intuitive at all to me! In an earlier section, GPT3 is provided as an example of something that has some knowledge that could theoretically bear on situational awareness but I don’t think this goes far … it is one thing to know about the world in general, and it is another very different \[thing\] to infer that you are an agent being trained. I can imagine a system that could do general purpose science and engineering without being either agentic or having a self-concept.

Her response to this is twofold. You can read in the transcript but I'll summarize it here to avoid a very lengthy quote:

-   The first is that she agrees with me that one could in principle build a system like the CHAD I described earlier in this post, that could do science and engineering or other tasks without this kind of situational awareness.
-   The second is that "It’s just that I don’t think that’s what happens by default." Because right now we're feeding models things about them being ML models and them having information about the companies that train them, etc.

It feels like a case of talking past each other, or having an irresoluble prior fight. I am as aware as she is about how the models are trained but we are drawing different inferences from the data. I don't know if she has thought at length about this specific topic. I definately hadn't until recently, and I couldn't find a well reasoned argument explaining what this would happen. So I'll try here to write an argument for situational awareness emerging

## The argument for situational awareness emergence

The argument _for_ situational awareness emerging by default goes like this

1.  The model having some form of self-concept is beneficial to the goals of the model. It will help them get reward in certain cases.
2.  There exists a set of weights in the model being trained that endows the model with situational awareness
3.  Gradient descent will find those weights
4.  Hence, given that it's possible that the model can have situational awareness _and_ that gradient descent will find those weights (because it reduces loss) then the model will end up situationally aware

The weakest assumption here is the first one. What could we say in defense of it?

If we agree that taking over the world is good for most goals (If not, just take this as an assumption), then how on earth could not situational awareness be something the model ends up with! By being able to deceive humans, the model can achieve so much more!

## My response to this argument

The first line of argument is that, once we agree that very powerful systems (like Chad) are possible without situational awareness, then there's less of an incentive for gradient descent to get you more things out of training. Say if lack of situational awareness gets you 99% of what you want and adding it is 10x the effort (in FLOPs or whatnot) then maybe training still stop when you basically got what you want (Either the engineers will stop the run because the model is amazing enough, or gradient descent will be noodling around a hard to traverse plateau and never get to a superoptimizer with situational awareness)

But to me the important argument is not that one.

> Similarly, GPT-4 is trained to say things like, “I’m a machine learning model; I can’t browse the internet; my training data ended in X year” — all this stuff that makes reference to itself — and it’s being trained to answer those questions accurately. It might have memorised a list of answers to these questions, but the more situations that it’s put in where being able to communicate to the human some fine-grained sense of what it is, the more likely it is that it has to develop this deeper concept of situational awareness in order to correctly answer all these things simultaneously. \[Ajeya Cotra's [podcast](https://80000hours.org/podcast/episodes/ajeya-cotra-accidentally-teaching-ai-to-deceive-us/)\]

This line "the more situations that it’s put in where being able to communicate to the human some fine-grained sense of what it is, the more likely it is that it has to develop this deeper concept of situational awareness in order to correctly answer all these things simultaneously" does not strike me as true. I don't think teaching the model that it is an LLM, that it's made by OpenAI, that it has access to plugins, etc does much. GPT4 already does that. This is just restating what I said in the original post, just like Cotra is restating what she had said prior to me writing my post, so it really is a prior fight at that point (ie two people restating their prior beliefs as counter to each other's beliefs, thinking that their thoughts haven't been already incorporated by the other. This is the sort of pattern that typically leads to people accusing each other of [begging](https://en.wikipedia.org/wiki/Begging_the_question) the question, but we're better than that: none of us would be persuaded by reading each others writings so far. So what else can be said here?

Take pretraining as an example. Could we find agreement that a pretrained system would not develop situational awareness? ChatGPT4 doesn't know it's ChatGPT4 but at least it will say something about it being a GPT-derived model. The baseline GPT4 model would know even less, so RLHF giving rise to situational awareness seems more likely (but still not very likely) that a pretrained model doing the same. Agreeing that pretrained models are ok and will lack situational awareness would be a start, but a minor one, because one can still maintain that by default systems will be superoptimizers (We don't just pretrain LLMs).

I can't give you a knockdown argument against Cotra et al.'s intuition (nor can they, I expect, give one against mine). One heuristic to help you undertand, to some extent why I think what I think, after some instrospection on my beliefs, though it's still missing something, as I expect most people won't directly connect this to the scenario under discussion.

Behind my intuition seems to be the heuristic that if something is constant during training, it gets ignored. Because the model is the same during training, the model doesn't learn it is a model. This is not always the case: You could imagine a model that is given access to its source code during training _and_ is poked adversarially during training (the files are deleted sometimes, the servers experience failures, etc), then the model could learn that copying files makes it more likely to continue to the next round of training, in a form of natural selection. Of course, this still requires explicitly giving the model access to "itself" (its running instance) and its code during training time in some sort of virtual world, which is not the way models are trained today.

... normally I would keep writing but I have other posts to write, so I'll leave it here, unedited and half-baked as is :)

_Ricón, José Luis, “The situational awareness assumption in AI risk discourse, or why people should chill”, Nintil (2023-07-01), available at https://nintil.com/situational-awareness-agi/._