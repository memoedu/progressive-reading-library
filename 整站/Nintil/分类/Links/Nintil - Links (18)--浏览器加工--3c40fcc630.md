Collection of papers and articles that I’ve spotted since my previous links post that seem interesting.

Philosophy

-   Sabine Hossenfelder wrote two years ago that "[Free will is dead, let's bury it](https://backreaction.blogspot.com/2016/01/free-will-is-dead-lets-bury-it.html)"
    
    -   Now she [has written](https://fqxi.org/community/forum/topic/3065) that physics actually does not necessarily rule it out
        
    -   She has even writen about a hypothetical "[free will function"](https://arxiv.org/pdf/1202.0720.pdf) that would be neither deterministic nor random. (HTF can something be neither!)
        
    -   > Here is an example: Consider an algorithm that computes some transcendental number, τ, unknown to you. Denote with τn the n-th digit of the number after the decimal point. This creates an infinitely long string of digits. Let tN be a time very far to the future, and let F be the function that returns τN−i for the choice the agent makes at time ti . This has the following consequence: The time evolution of the agent’s state is now no longer random. It is determined by F, but not (forward) deterministic: No matter how long you record the agent’s choices, you will never be able to predict, not even in principle, what the next choice will be (1b). If you constrain yourself to recording his choices for a finite amount of time you wouldn’t be able to predict his next move even if you knew τ. You could go and ask Laplace’s demon to tell you all the degrees of freedom of the whole universe at any one time and still you would not be able to figure out what the agent will be doing next (1’). But if you only knew the agent’s free will function, there would be nothing ambiguous about his actions (2). In some sense you could say, the agent “is” his free will function. Any time our agent faces a decision, it calls upon its free will function and asks for the next number, which corresponds to it “making a choice.”
        
    -   This seems to fail because the function would have to have a physical instantiation and so you could know about it.
        
-   A talk [on consciousness](https://www.consciousentities.com/2018/07/secrets-of-consciousness/) featuring Philip Goff and Susan Blackmore.
    
    -   This leaves more confused about what Susan Blackmore actually thinks. At times she seems like a full on, Tomasikian eliminativist, at other times she seems to think there is a universal consciousness from which one's is instantiated (the bit where she says she agrees with panpsychism a bit), but that the idea of a separate consciousness or a self is an illusion. (?)
-   [Are ethical asymmetries from property rights?](https://meteuphoric.com/2018/07/01/are-ethical-asymmetries-from-property-rights/)
    
    -   > My guess is that these ethical asymmetries—which are confusing, because they defy consequentialism \[lmao\]
        
-   [Does liberalism destroy liberty?](http://www.econlib.org/library/Columns/y2018/Klingdestroy.html)
    
-   In the[trolley problem,](https://jasoncollins.blog/2018/07/04/hypotheticals-versus-the-real-world-the-trolley-problem/) what people say the will do doesn't match perfectly with what they actually do.
    
    -   Also, two of Greene's (Of "Utilitarianism-is-rational-and-deontology-is-not" fame) papers on the matter don't replicate
    -   My personal view at the moment is that such a paradigm is likely not true. High rationality and intelligence lead not to utilitarianism, but to moral minimalism and multifaceted moral judgements. (See [here](https://www.ncbi.nlm.nih.gov/pubmed/24976510) or [here](https://www.taylorfrancis.com/books/e/9781315460161/chapters/10.4324%2F9781315460178-10))
-   [Friedrich Nietzsche: The truth is terrible](https://www.the-tls.co.uk/articles/public/friedrich-nietzsche-truth-terrible/)
    
    -   > And what if, as Nietzsche argues, a morality of equality – and altruism and pity for suffering – were, in fact, an obstacle to human excellence? What if being a “moral” person makes it impossible to be Beethoven? Nietzsche’s conclusion is clear: if moral equality is an obstacle to human excellence, then so much the worse for moral equality. This is the less familiar and often shockingly anti-egalitarian Nietzsche. \[...\] If Nietzsche’s speculative psychology is correct, then we arrive at a startling conclusion. In a hedonistic and sympathetic culture, which devalues suffering and prioritizes its relief, the glorious spectacle of human genius will be missing from the world: no Beethovens, Nietzsches or Goethes. But absent these creative geniuses, Nietzsche thinks we cannot respond to Schopenhauer’s existential challenge.
        
    -   [Pinker is not amused](https://twitter.com/sapinker/status/1013154868901621760)
        
    -   If Nietzsche were right (Ideally one can have all the nice things!) and we have to pick between not having suffering and having the glorious spectacle of human genious, I'd rather side the the spectable. This may be an interesting topic for further investigation, because I expect disagreement here. To see why, the mot obvious way to side with the anti-suffering side is to say that pleasure and pain is all that matters, etc. If that is so, one would have to be okay with tiling the universe with brains in vats experiencing orgasms. Compare that to a universe filled with conscious beings engaging in arts, culture, philosophy and games of a diverse variety. What do you prefer?
        

Psychology

-   The Mind is flat, a talk [at Google](https://www.youtube.com/watch?time_continue=1&v=vspX6NaLxdc)
-   An old finding is that getting experiences (instead of buying material stuff) makes you happier. [Paper](http://journals.sagepub.com/doi/abs/10.1177/0956797617736386) argues that this is true for higher social class individuals, but not lower class.
-   [Paper claims physical  beauty](https://twitter.com/SteveStuWill/status/1006444586758098944) is universal-ish (as in attractiveness, not everything is about string theory, what were you thinking!)

Tech

-   How to beat Montezuma Revenge, the super difficult Atari game, with RL? [By playing it "backwards"!](https://blog.openai.com/learning-montezumas-revenge-from-a-single-demonstration/) Even though they say a single demonstration was involved, the agent still had access to the underlying game to run lots of iterations. Rather than one-shot learning this is more like seeing a sniper shoot, record it in video, and then practice for a day, watch it, and repeat for 50 years.
    
-   OpenAI did some impressive work on beating DOTA 2.
    
    -   [But how impressive is it really?](https://twitter.com/mtrc/status/1011506644113592320)
        
    -   > The bots have human-authored item and skill builds that never change, are forced to lanes and not allowed to leave them for a while, many items and game features are disabled (including crucial concepts like vision) and most importantly: every match is a mirror matchup of the same five heroes. Not only do mirror matchups not exist in DOTA 2, the heroes chosen are about as simple as it gets, and don’t have a lot of interesting synergy on offer (although there’s a little here or there). Open AI explain that their bots have a reaction time of about 80ms, which they note is faster than a human’s. But it’s not just faster than a normal human reaction – clicking a key to react to an attack, for example. In that 80ms the system doesn’t just react to a single event – it can parse the entirety of the game map, from top to bottom, using a perfectly precise API that describes the game in a series of numbers. They’re not just fast, they’re processing more information than any human will ever be able to. Many tasks that humans struggle at – such as ‘armlet toggling’, which became a point of controversy this week as it emerged a pro player was using scripts to perform it – are as trivial to an AI as walking down a lane or hitting a creep.
        
    -   > The reason I’m belabouring this point is because Open AI’s bot _is already superhuman_, even without playing the whole of DOTA 2. It’s unsurprising that a system with such perfect access to information and the ability to react quickly is able to outplay humans. The interesting things here are that it _learned_ these basic skills, not _how well_ it’s able to execute them. That part is very obvious – and depending on how you view it, isn’t really comparable to how humans play anyway. How good would a player like Dendi be if they could read the entire map and combat log for every 80ms of gameplay?
        
-   [Understanding the working of Universal Language Model Finetuning](https://yashuseth.blog/2018/06/17/understanding-universal-language-model-fine-tuning-ulmfit/)
    

Economics

-   It is often argued that the high medical expenditure that occurs in the last months of life is waste, but this dosn't seem to be so given that death is [highly unpredictable](https://twitter.com/EricTopol/status/1012402104504160256) (for that population).
    
-   David Graeber said that bullshit jobs surround us. But how many people actually think that their job is "socially useless". [Only 8% think so, and an additional 17% have doubts.](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3162569)
    
    -   There is some variation by country. Poland, Japan, and Israel led in perception of uselessness, while Mexico, Slovakia and Switzerland residents tended to think so to a lesser degree.
-   Paper argues that, contrary to previous evidence, [women ask for salary raises](https://hbr.org/2018/06/research-women-ask-for-raises-as-often-as-men-but-are-less-likely-to-get-them?utm_medium=social&utm_source=twitter&utm_campaign=hbr) as often as men. But they are less likely to get them.
    
-   [Emerging trends in peer review - a survey](https://www.frontiersin.org/articles/10.3389/fnins.2015.00169/full)
    
    -   Including objections to peer review
-   In the US, perception of discrimination has risen substantially among progressives, but there is no evidence that there is more discrimination. Argues [some twitter account.](https://twitter.com/ZachG932/status/1011481776571781120)
    
-   Why Mariana Mazzucato [is wrong](https://unherd.com/2018/06/mariana-mazzucato-wrong-entrepreneurial-state/) about the Entrepreneurial state
    
    -   Besides all the posts I already wrote :)
-   [The lightship in economics](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3157100)
    

Other

-   [Challenges to Bayesian confirmation theory](https://www.pitt.edu/%7Ejdnorton/papers/Challenges_final.pdf)
-   STEM doesn't seem to have [more (sexual) harassment](https://twitter.com/NoamJStein/status/1006903473164705793) compared to other academic fields. I have to say that stuff like saying "bitch" or "don't be a pussy" do not even sound denigrating to me (except to the person at the receiving end, but same for any other insults). This ilustrates to an extent that the moral outlook can tint scientific research. Different views lead to different definitions, and while we are all entitled to our definitions, then those are carried into the wild by journalists, and people don't always read what the paper precisely meant.
-   [Putanumonit](https://putanumonit.com/). I found a new blog to follow.
-   Sometimes I'm asked: How do you take notes, organise thoughts, links, and information? Do you use any specific software or technique?
    -   I have to disappoint you: I don't do anything :-)
    -   I read and remember only what seemed surprising to me from the reading. Usually not that many things are surprising. When they are, I fact-check, and the process of fact-checking helps consolidate that memory (or prepare a critical post :)
    -   My memory is quite bad, but I still manage to hold mental links and procedures to find where the information is. This is much more efficient, instead of remembering the history of Europe, I know that for a generic introduction, I can do "the history of Europe" on Google. This way I only need to have in store basic summaries.
    -   This also help with thinking. Poor memory makes you revisit things over and over, and makes it difficult to get attached to ideas or even have a [large identity](http://www.paulgraham.com/identity.html) that can mess with your rationality.
    -   In terms of just reading, I reduce the amount to what really is interesting by saving and storing content for a month, and then doing as much reading as I can in 1-3 days. This is what produces the Links posts. It happens that troughout the month I may think that something is cool, but then upon reflection is not worth reading, or sometimes something happens, but then days later it gets completed by other posts. Monthly reviewing allows for focusing reading time.

-   ![Tl7](https://www.gravatar.com/avatar/84d1fc8bd052e237aac9eb5c9570c221?d=identicon) **[Tl7](http://lilo298151568.wordpress.com/)** 2018-07-08T20:03:32Z
    
    > Since i can't respond to your comment on the other page https://nintil.com/2018/06/03/links-17/ i will respond here.I haven't understood clearly what computationalists mean when they say that consciousness is information processing,how does consciousness emerge from processing data?Indeed,How can a program be conscious?I mean,programs are purely abstract notions.Maybe what they mean when they say this is that when a program is implemented the internal state of the computer changes and this change 'causes' mental states.Do you think this is possible? By the way,when people like bostrom talk about conscious programs inside of conscious programs(simulation inside simulation),which could be possible according to computationalists,then this would open the door to the possibility of dreams inside dreams ,since what only matters is the right program.Wouldn't this make their view ridiculous?
    
-   **[Rational Feed – deluks917](https://deluks917.wordpress.com/2018/07/09/rational-feed-362/)** 2018-07-09T14:26:58Z
    
    > \[…\] Links by Artir – Link Post: Philosophy, Tech, Economics, Other. \[…\]
    
-   ![Peregrino Nuzkwamia](https://www.gravatar.com/avatar/8bf04d62dfb9d074e7dbfb740d408fd6?d=identicon) **[Peregrino Nuzkwamia](http://nuzkwamia.wordpress.com/)** 2018-07-09T04:07:18Z
    
    > Maybe Steven Pinker is upset because the article author, who claims to be writing a book about Nietzsche, seems to be unaware that current expert opinion is that Nietzsche's fatal illness was a brain tumor, not syphilis.
    
-   ![Artir](https://www.gravatar.com/avatar/77fac16b1eefeb9c6dcd9d0cfa2ea692?d=identicon) **[Artir](http://)** 2018-07-10T08:53:30Z
    
    > ! That's certainly something I didn't know.