'Friendly AI' is a field of research with the goal of ensuring that if and when a general purpose artificial intelligence (especially with greater than human intelligence) is developed, it won't be harmful to us.

I think there are some problems with this. I write this post in part to attract people who want to say why I am wrong, and why Friendly AI research would actually be useful.

First, if you want an introduction to what this is all about, read Superintelligence by Nick Bostrom. He explains the concept of smarter than human intelligence (this could come by means other than AI), and the many problems it presents. No perfect solution is given for them, and they are said to be hard. From Bostrom's (ch. 9), here's a list of some proposed solutions to control a Superintelligence.

> **Capability control**
> 
> **Boxing methods** The system is confined in such a way that it can affect the external world only through some restricted, pre-approved channel. Encompasses physical and informational containment methods.
> 
> **Incentive methods** The system is placed within an environment that provides appropriate incentives. This could involve social integration into a world of similarly powerful entities. Another variation is the use of (cryptographic) reward tokens. “Anthropic capture” is also a very important possibility but one that involves esoteric considerations.
> 
> \*\*Stunting \*\*Constraints are imposed on the cognitive capabilities of the system or its ability to affect key internal processes.
> 
> **Tripwires** Diagnostic tests are performed on the system (possibly without its knowledge) and a mechanism shuts down the system if dangerous activity is detected.
> 
> **Motivation specification**
> 
> **Direct specification** The system is endowed with some directly specified motivation system, which might be consequentialist or involve following a set of rules.
> 
> **Indirect normativity** Indirect normativity could involve rule-based or consequentialist principles, but is distinguished by its reliance on an indirect approach to specifying the rules that are to be followed or the values that are to be pursued.
> 
> **Augmentation** One starts with a system that already has substantially human or benevolent motivations, and enhances its cognitive capacities to make it superintelligent.

The problems for trying to make AI safe are many.

First, let us think about how the first Superintelligence will come into existence. Let us further assume that it will be an Artificial Intelligence.

I see this happening either in an institutional setting or in a small team of tinkerers. If the former, Friendly AI research can be useful. If the latter, Friendly AI research won't be of much use, because there is no guarantee every single developer will be using FAI research in their own project.

Then, even if an institutional team who follows FAI recommendations is first in achieving this, a second team not following the guidelines may also develop a Superintelligence. We are assuming that the first team's Superintelligence will be controlled somehow, so it wouldn't be able to control the teams that came after it. Even if it were programmed to do that, there would be some time between Superintelligence appearance and Superintelligence deploying effective surveillance all around the world.

In any event, I'm expecting the Superintelligence not to appear because it has been proven to be safe and then implemented. Technological progress rarely, if ever, happens like that. The first Industrial Revolution itself got started without much input from Science, but by tinkering. I'm not saying that trial and error can get you anywhere: [it's not the case.](https://artir.wordpress.com/2015/10/26/linear-models-comments-on-ridley/) I'm making the weaker claim that 'fundamental grounding' as in simulating the full physics of a problem, or proving that something works in math or computer science, is not how things happen. Even basic science involves plenty of trial and error, and following promising results.

What will happen is that someone will be toying with some very advanced machine learning code, or trying to implement Schmidhuber's [Gödel Machine](http://people.idsia.ch/%7Ejuergen/goedelmachine.html), or some version of [AIXI](http://www.hutter1.net/ai/uaibook.htm) in a realistic and useful way, and then, somehow, the system will end up, unexpectedly, Superintelligent.

This leads us to my core concern, and it is that I don't see the possibility of formally proving or designing a Superintelligent system so that it is friendly, everywhere and always. Surely we can keep it contained, but that is the second best solution. Ideally, it would be free to do its function, but being friendly at the same time, without being excessively constrained.

Shane Legg, a researcher at Deepmind, proved [in his PhD thesis](http://www.vetta.org/documents/Machine_Super_Intelligence.pdf) that there exist no simple but powerful AI algorithms, and that the algorithms powerful enough to be of interest cannot be proven to work. From that, it's likely that the first Superintelligence will appear serendipitously, and not because of careful design.

![Captura.PNG](https://nintil.com/old_assets/2016/05/13-1.png)

> We have shown that there does not exist an elegant constructive theory of prediction for computable sequences, even if we assume unbounded computational resources, unbounded data and learning time, and place moderate bounds on the Kolmogorov complexity of the sequences to be predicted. Very powerful computable predictors are therefore necessarily complex. We have further shown that the source of this problem is the existence of computable sequences which are extremely expensive to compute. While we have proven that very powerful prediction algorithms which can learn to predict these sequences exist, we have also proven that, unfortunately, mathematical analysis cannot be used to discover these algorithms due to Gödel incompleteness.

Perhaps a similar proof could be provided for the impossibility of provably Friendly AI. Legg himself thought he had a sketch of a proof [ten years ago](https://web.archive.org/web/20120918213006/http://www.vetta.org/2006/09/unprovability-of-friendly-ai/), but in the end it wasn't solid.

To recap, the argument is:

1.  It won't be possible to design an AI that's provably Friendly (My core claim)
2.  Even if it were possible, there exist the possibility of someone designing an AI that doesn't follow that design
3.  Given the tradition of openness in AI research, it's likely that after the first Superintelligence is developed, others will follow
4.  There won't be time for the first Superintelligence to stop other researchers from developing their own.
5.  From the above, there will exist a Superintelligence that is not proved to be Friendly, regardless of Friendly AI research.
6.  Hence, Friendly AI research is possibly futile.

We should be careful not to think that just because my conclusion implies no certainty about a Superintelligence destroying us this has to mean that my conclusion is wrong. If with high certainty, the Superintelligence will be unfriendly (e.g. a [paperclip maximiser](https://en.wikipedia.org/wiki/Paperclip_maximizer)), then it follows that the human race will disappear as soon as that Superintelligence is created, and there's little we can do to stop it. This is an unfortunate implication, but its grimness does not affect what I'm saying here. Except perhaps in the way that even if there is a really really low probability of me being wrong, it still pays off to do Friendly AI research 'just in case'.

Now I relax and wait for someone to correct me. Perhaps there's something I've missed, or something that should lead us to expect that the problem is actually solvable.

\[Update: I'm more optimistic now about the possibility of Friendly AI than I was when I wrote this post\]

-   ![Frank Ch. Eigler](https://www.gravatar.com/avatar/34c49c1c354607b40f57e029b4cdee30?d=identicon) **Frank Ch. Eigler** 2016-05-22T20:38:23Z
    
    > Just a small observation that applies to AI as well as weapons research. Given the impossibility of rooting out evil-offensive-AI research by other parties, it becomes very important for civil players to invest in anti-AI-defence research, which would likely be an AI project of its own. The limitation on the scope of the defence might make tenable the problem of assuring the safety of a defensive AI.
    
-   ![jsevillamol](https://www.gravatar.com/avatar/111a7d8b594d7707bc1f5e3085c069ef?d=identicon) **[jsevillamol](http://plumacreativa.wordpress.com/)** 2016-05-13T13:10:11Z
    
    > Very interesting reflection!
    > 
    > From my so far very limited knowledge of computability and technical AI safety research I can try to answer your main claim while hopefully not embarrassing myself too much.
    > 
    > I have not read but the outline of Legg's thesis, but guiding myself by the proof outline which turned to be wrong and thinking that he has proven something similar, I see two big problems with it.
    > 
    > Firstly, it seems like the main argument comes from "we cannot prove that an AI will be powerful and safe since that would imply that the AI is powerful in the first place, and because of Gödelian reasons that is not possible". That is certainly discouraging, since as you remark that means it cannot exist an algorithm to systematically find powerful AI designs.
    > 
    > But that leaves out the very interesting possibility of proofs which take powerfullness as a given, and proceed to prove safety. That is, propositions of the form "if this AGI is powerful, then it is provably safe". Such a result would be an incredible advance in AI Safety.
    > 
    > For some research in this line, see https://intelligence.org/files/ProgramEquilibrium.pdf, in which MIRI researchers prove reflective cooperation of unbounded and uncomputable agents in prisoner's dilemma with mutual source code access. To spoil a bit, they prove that if a certain uber powerful (halting oracle level powerful) agent plays against a formally equivalent copy of itself, it will cooperate to achieve the best output (both cooperate). By the way, there's a new paper in which they actually bound the agents to the computable realm, see https://intelligence.org/2016/03/31/new-paper-on-bounded-lob/.
    > 
    > So this is actually a line of research of MIRI. We can approximate intelligence using agents with halting oracles or NP-oracles, and then prove things about them. Such agents are provably and almost certainly, respectively, more powerful than any real agent, so if we can induce friendliness on those we can almost certainly induce it on real AGIs (with some caveats).
    > 
    > But the real problem rises when we start thinking about whether it is really neccessary to prove safety to build safe AGI. To start with a comparison, we are not 100% sure that anything we have ever built is secure, since that is the nature of scientific induction, but hell, I am pretty damned sure the portatil is not going to burst into flames and burn my face while I write this. Achieving a similar level of confidence with an AGI would be impressive.
    > 
    > The problem here is that our math is not mature enough to properly express uncertainty about logical propositions, so we are stuck to thinking in terms of proven vs unproven. Now, it is fairly intuitive that not every math proposition is not exactly as consistent a priori. In some sense, it seems like I should be able to assign provabilities to math statements, such as "the digit number 3.000 of pi's decimal expansion is 3", before actually working out a proof of the statement or its logical negation.
    > 
    > To tackle the issue from another angle, suppose I prove that PA is inconsistent. That certainly would not make Google stop working, or cause planes to fall from the sky. Probably, most mathematicians in the world would keep using it as a proper formal framework for their math, and would reach meaningful results. What is going on? Well, it seems like there are certain degrees of inconsistence.
    > 
    > If we suppose 'upon observation, the sky is red', then I can deduce anything from that fact, since that is a logical counterfactual which contradicts the actual state of affairs of reality. But there are some conclusions which seem more pertinent than others which follow from it, such as 'then the wavelenghts of the sun which do not correspond to red have being blocked by a chemical with great certainty'. In contrast, deducing from it that 'then pigs fly' seems absurd, while it is logically coherent from the principle of explosion.
    > 
    > MIRI is also researching how to formalize this notion of Logical Uncertainty (and there are exciting new results! https://intelligence.org/2016/04/21/two-new-papers-uniform/). I think this is right now the biggest priority in AI Safety, and maybe plain AI too; it is too restrictive to think in terms of proven and unproven when modelling logical knowledge, and it seems a step further in the direction of integrating computability theory into general math deductions.
    > 
    > I think I should stop here before I start losing my concentration and start saying uncoherent things. Nice post, Artir.
    > 
    > tl;dr some lines of research in FAI are investigating the behaviour of suppossedly hyperintelligent agents and modelling mathematical uncertainty, and there are no reasons to think such a researc is provably impossible.
    
-   ![Artir](https://www.gravatar.com/avatar/77fac16b1eefeb9c6dcd9d0cfa2ea692?d=identicon) **[Artir](http://)** 2016-05-13T17:45:17Z
    
    > Thank you for your comment!
    > 
    > I'm vaguely aware of MIRI research, altough I haven't read the papers you mention. I have a huge backlog of things to read. The two on logical uncertainty are particularly interesting because not too long ago, I thought that the problem had been solved, but turns out it wasn't ( http://meaningness.com/probability-and-logic )
    > 
    > Reading the papers may alter my estimates of the feasability of the task at hand. However, right now I still see a big gap between proving results for simple agents in relatively simple situations (prisonner's dilemmas) and proving friendliness for a full AGI.
    > 
    > Regarding Legg's proof, the one he sketched a this blog is independent fom the one that he presents in his thesis (and given that is has gone through review, it's plausible to assume that the proof holds). I used the proof for a) Induce readers to read Legg's thesis :)) b) as an analogy for what I wanted to say. That is, perhaps one can prove friendliness for simple agents, but not for complex ones.
    > 
    > But, as you say, perhaps proof is not necessary, just reasonable,as in the laptop/portatil (:P) example. In engineering, for example, one calculates the loads of a bridge, then multiplies them by 1.5 or something, and then designs for that loads using a safety factor. That ensures >95% probability that the bridge will never ever fall. But doing that for machine intelligence seems more difficult. I could imagine a system of weights in the SI utility function that tries to push down hard the unfriendly options so that they are still there, but then there's always the meta-problem that the SI is going to represent itself as an object in the world, and so my still decide to change the weighting to make its paperclipping life easier.
    > 
    > As an example, eating the world-famous spanish omelette makes me puke. However, I would take a cheap pill that changes my reaction so that I don't. But I would never take a pill that makes me think that killing people to further my interests is good. And the thing is that there are certain cases in which I would kill, so it's not like I have a 100% ban on killing. But it would be advantageous for me to regard killing as just one more option rather than something to avoid even if i can get away with it. In the end, the problem seems tied to 'solving ethics and motivating a machine to abide by them so that it avoids self-modifying the values'. Then there's the problem of value change: if humanity's values change, what would the AI do? One option is to acquire them from a weighted average of whoever are the smartest and more accomplished humans on earth, which historically have tended to be the less evil guys. (I do know about CEV and its problems).
    > 
    > Ultimately, I think that there should be people researching this, but I would be willing to bet right now on my >86% suredness that X (where X is a reasonably way of making AI safe that excludes boxing it) won't be happening.
    
-   ![akarlin](https://www.gravatar.com/avatar/11c7a9262855d91316d620adedf79fa2?d=identicon) **[akarlin](http://akarlin.com/)** 2016-06-03T01:33:33Z
    
    > I am [very skeptical](http://www.unz.com/akarlin/skynet-no-worries/) about the possibility of a true superintelligence "explosion" so the issue of their "friendliness" is largely moot.
    > 
    > To augment intelligence you need to, as with all other forms of technology, to solve ever harder and harder problems, so the exponential aspect that so many worry about is balanced by a countervailing exponential aspect.
    > 
    > Furthermore, the higher level the ideas and methods that are developed, the greater a percentage of them have tended to benefit the entire system/noosphere as opposed to the individual agent making the innovations. It strains credulity that not only will one entity make a series of innovations that advances its general intelligence by several standard deviations above that of the closest competitors but that the methods and processes by which it does so do not at any point get leaked to the world at large.
    > 
    > That said, you do need formidable computing (hence, financial) resources to run superintelligences, so I don't see it as being very democratized/decentralized either. I think the likeliest scenario is one of steady (not runaway) progress amongst a sort of "oligopoly" of superintelligences.