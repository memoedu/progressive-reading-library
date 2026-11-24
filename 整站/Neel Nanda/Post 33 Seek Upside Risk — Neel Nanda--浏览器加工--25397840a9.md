When I meet somebody new, there are two interesting things that could happen. We could get on _super_ well, become great friends, and be part of each other’s lives for year to come. Or we don’t. Maybe we like each other, maybe we don’t, but we likely just go our separate ways. But the upside in the first case _massively_ outweighs the downside in the second case. Yet, we all spend far less time meeting new people than we could. Why?

Upside Risk

I call this general phenomena of a small chance of a massive win, vs a reasonable chance of a tiny loss, **upside risk**. And when I think about some of the _really awesome_ things in my life, a lot of it came from sources of upside risk:

-   I have a lot of _amazing_ friends, most of whom I went [out of my way to meet and keep in touch with](https://www.neelnanda.io/blog/mini-blog-post-23-taking-social-initiative)
    
-   Hobbies! I tried blogging, it might have been a bit of a waste of time, but [turned out to be fucking awesome](https://www.neelnanda.io/blog/27-retrospective). I could have lost a bit of time, but I now have a great new creative outlet!
    
-   Asking for advice and favours - I’m going to be spending the next 3 months doing a research project, and this would never have happened if I hadn’t been asking loads of people for advice and suggestions!
    

In general, I think there are a _lot_ of sources of upside risk in life, and that they’re extremely positive in expectation. And if you do enough of them, this pays out well. So I think it’s _extremely_ valuable to actively seek out upside risk, and to shape your life to welcome it.

There’s even a fairly solid mathematical justification for this! This is known as the **explore/exploit problem** in reinforcement learning - do I **exploit** and do what I currently think is the best idea, or do I **explore** and try something new, something that might give me **information** that I can use to inform future actions. The problem is essentially pricing the value of information.

And a pretty decent (and easy-to-compute) strategy is to take the [**Upper Confidence Bound**](https://lilianweng.github.io/lil-log/2018/01/23/the-multi-armed-bandit-problem-and-its-solutions.html#upper-confidence-bounds). We have uncertainty over each strategy, and pick the strategy with the highest 95% confidence bound. Essentially, it pays to be an optimist, to always take the action with the best chance of being _awesome_.

This post is going to be about why we _don’t_ seek upside risk as much as we could, and how we can change this.

Becoming emotionally OK with it

So, this is a pretty simple argument. Yet, I do this far less than I could. Why?

The root cause, is that my mind _sucks_ at thinking about expected values. When I try something new, the **cost** is clear, concrete and salient. The **benefit** is fuzzy, distant and unclear. And so if I try it and only get the cost, it feels like I **failed**. Hindsight bias kicks in, and it feels _obvious_ that it wasn’t worth trying. The subtle, insidious voice of insecurity in my head pushes me to play it safe, not stick my neck out, to not risk it. That it’s not worth the effort, and things will never work out well. I’m a loss averse person, and I can’t see a _guaranteed_ story for how this upside will pay out, so it feels not worth reaching for.

I think this is one of the most poisonous and damaging bugs currently in my life. I think this _significantly_ cuts off a lot of awesome potential in my life. So **fuck that.** I don’t _need_ a precise story for how to get this upside, I just need to expose myself to as much as possible. So how can I reframe this and _do_ something about it?

Some of my favourite strategies:

-   **_Make_ the upside concrete**: Try to flesh out a concrete scenario where this goes awesomely. Imagine it’s in the 90th percentile of awesome, what happened? Add as many specific details and ideas as possible. And if the creeping voice of insecurity in your head triggers, remember that it’s _explicitly_ a hypothetical where everything goes well, so you can ignore it!
    
    -   And now, at the end, do you feel any more excited about this upside?
        
-   **Think in expected value**: When I’m trying something new, I’m operating in a position of uncertainty. I need to choose based on the expected value (EV). But _afterwards_ I know the actualised value. And if I don’t stop myself, hindsight bias creeps in, and it feels _obvious_ that I should have known that all along.  
    But, if I’m trying to update what I do in future, this is dumb! All future choices are made based on EV, so that’s what I should actually care about. So keep score in EV! If you took a bunch of positive EV actions and are confident it was the right call, focus on that, not on the statistical noise of the results
    
    -   This is awesome, because it _redefines failure_. If I take a positive EV action that goes wrong, who cares! I still won
        
        -   I’ve thought about this mindset enough that I’m now happy if I ask somebody out and am rejected - the action was still positive EV!
            
    -   Alternate framing: Focus on the **value of information**. If I try swing dancing and it sucks, this is _great_! I learned something new about myself, and need never try swing dancing again
        
-   Strive to become [someone who Actually Does Things](https://www.neelnanda.io/blog/become-a-person-who-actually-does-things). Score things by how they shape my future actions and bring me closer to the kind of person I want to be
    
-   **Do it for the joy of novelty**: Trying new things is **fun!** I can learn new things! Have new experiences!
    
    -   I find it pretty easy to get joy just from doing something new - satisfying curiosity, learning something new
        
    -   If I notice my life in a bit of a rut, I find it super motivating if I can start pushing myself out of it by adding in some novelty
        
-   **Just do it**: Have a rule of just _doing_ any low cost, high upside idea. If you have a request you want to make of someone, just do it! If you want to invite a friend to start an ambitious project, just do it!
    
    -   For me, insecurities are most insidious when there is _uncertainty_. When there is a _choice_. Choices take energy and willpower, and it’s easy for insecurity to massively raise this cost. The beauty of a _policy_ is that [**changes the default**](https://www.neelnanda.io/blog/mini-blog-post-19-on-systems-living-a-life-of-zero-willpower). Figuring out whether it’s worth it isn’t worth the computational resources, just do it!
        
        -   Policies can build momentum - if you can push yourself through the first few times, it feels aversive to _not_ keep going, because you’re breaking a streak, and screwing over your future self!
            
    -   This also redefines failure - the goal is to follow the policy, because on average the policy works! Who cares if it doesn’t work sometimes?
        
-   **Opportunity framings**: If I’m asking a favour of somebody _else_, I find it super helpful to frame it as presenting them with an opportunity. If I ask somebody for a job and they say yes, they aren’t doing me a _favour_, they’re doing it because it’s helpful to them! So, asking in the first place is giving them **free option value**, they can always say no!
    
    -   So, when asking for something, think about how it can benefit the _other_ person!
        
        -   This can work even when there’s no direct benefit - I generally find it fun to give advice to people, even though I get no benefit from it. Which means at least some of the people who give advice to me enjoy it!
            
    -   This relies on the other person being comfortable saying no in a low cost way. Conveniently, this is normally true in the more intimidating scenarios, like asking for a job! And if I’m asking for somebody’s time, the busier and higher status they are, the more they’ll have practiced saying no to things and so the lower cost it’ll be to them.
        
    -   Conversely, I know that for some of my friends, saying no is hard. So if they say no to an offer I give them, I make a point of congratulating them on practicing the skill of declining things! And this utterly takes the sting out of rejection, and makes it feel lower cost to ask for favours next time :)
        

Implementing

So, say you now agree that upside risk is extremely valuable, and you’ve somewhat overcome the aversion. How can we make this _actionable_?

There are a few different ways of framing the core lesson:

-   Do things that are high variance!
    
-   Optimise for the best-case scenario, be an optimist!
    
-   Optimise for information value, what can _teach_ you the most? Do things where if it goes well, you can do it again and again
    
-   Seek novelty
    

I find seeking novelty to be particularly fruitful. A few ways to do this:

-   Be willing to try anything once, default to yes.
    
    -   I try to do this for life hacks/productivity advice - I’ve tried a lot of crap ones that I’ve discarded, but the ones that work well are now a _core_ part of my life. Eg, discovering the value of a consistent routine, and how to use financial accountability right have been phenomenally valuable to me
        
-   Make seeking novelty a **quest**. Set a bounded period of your life, say the next 1-2 months, where you are going to go out of your way to do novel things, and see what it’s like. You aren’t trying to evaluate what’s worthwhile or do a cost-benefit analysis, just doing whatever is most fun and feels shiniest
    
    -   When I decided to try this, I went to taster sessions for about 10 societies. And most were crap! But it turns out that gliding, pole-dancing, ceilidh dancing and trampolining are all _super_ awesome
        
        -   I also ended up donating blood, learning how to cycle, learning how to bake, among a bunch of other stuff. My life was _way_ more fun that term, and a few things have stuck!
            
-   If something is boring, think about how you could reframe it to _make_ it novel.
    
    -   For exams in my second year, I felt obliged to spend 2 months revising, but expected this to be a poor use of my time. So I reframed it to be about trying different ways of learning, and getting information about how I learned best.
        
    -   A few findings:
        
        -   I find [giving talks](https://www.youtube.com/channel/UCzoIa8e4vhfRIxL_Kj6yy0Q?view_as=subscriber) _super_ motivating and fun, and it holds me to a high standard
            
        -   I really enjoy teaching and tutoring, trying to explain content intuitively to other people
            
        -   [Spaced repetition](http://augmentingcognition.com/ltm.html) is fucking awesome
            
        -   [Making notes](https://dynalist.io/d/ToOhEKlz9qC2PmpjgyTpXQJz) with the intent of publishing holds me to a _way_ higher standard (and [has positive externalities](https://www.neelnanda.io/blog/mini-blog-post-10-seek-positive-externalities)!)
            
        -   There were a handful of failed experiments too, but I can’t really remember them. And really, I think that summarises this entire post
            
-   The idea of **Comfort Zone Expansion (CoZE)**. Finding things outside your comfort zone, that you’d normally find super aversive, and finding ways to try them in a safe, supportive environment.
    
    -   CFAR have a CoZE section at their workshops, and I found it super valuable every time! I’ve spent CoZE workshops:
        
        -   Going around a room asking people for sincere criticism, and not becoming defensive
            
        -   Asking people for favours (man, I suck at this)
            
        -   Having an open, honest and vulnerable conversation with a relative stranger, where we agreed to not deflect or use defence mechanisms (which was the start of becoming _way_ better at this)
            

Another mindset is to optimise for the best-case scenario, which I like to think of as **fishing for white swans**. Some strategies:

-   When doing anything, use pre-hindsight. Assume it went _phenomenally_ well, and try to explain what happened? And when you have an explanation, try to _make_ it happen
    
-   High-variance small talk! The social norm is normally for first meetings to be dull, and keep to safe topics. I generally try to skip to whatever I find interesting - digging into people’s motivations, dreams, stories and fascinations. Getting to something genuinely interesting rapidly [seems to be a trainable skill](https://www.neelnanda.io/blog/mini-blog-post-5-how-to-learn-from-conversations)
    
    -   I gather this makes me come across as somewhat direct and intense, but the world is full of people! This probably filters out some people I’d have gotten on well with, but has found me a lot of awesome close friends.
        
-   When asking for advice, it’s easy to focus on minor uncertainties, and to be scared of asking big questions. But advice is most useful when it _radically_ changes something
    
    -   “What’s a bit of life-changing advice you’ve received?”
        
    -   “What is the biggest problem with my current life plan?”
        
    -   “Suppose in 5 years you realise everything you just told me was wrong. What happened?”
        
    -   “What’s something it could make sense for me to try with _super_ high potential upside?”
        
-   Interestingly, this strategy is often _most_ useful when things are already going well for me.
    
    -   If I already have a bunch of great friends, I want to filter harder for _exceptionally_ awesome new friends
        
    -   If I have a great life plan, I want to try something wild and high-variance, since I’m optimising for the probability of finding something _better_
        

Some generic examples of things worth doing:

-   Going on dates
    
-   Meeting new people
    
-   Applying for jobs and problems that seem out of your league
    
-   Trying a hobby! An instrument, improv comedy, public speaking, blogging. Something outside your comfort zone
    
-   Notice the feeling of reluctance and procrastination, and check whether it feels justified
    
-   Follow the crazy ideas that pop into your head! If you have a wild new idea for a party or event, go ahead and organise it
    
    -   It helps to be in an environment that facilitates this kind of thing, I’ve found it pretty useful to be loosely involved in student societies for this
        

And these are just my examples! Upside risk is a function of _your_ life, _your_ values, and _your_ circumstances. What are some examples of this in _your_ life?

**Exercise:** Set a [5 minute timer](https://www.neelnanda.io/blog/post-28-on-creativity-the-joys-of-5-minute-timers), and think about the ideas in this post. What are sources of upside risk _you_ could seek out?

Conclusion

Obviously, I’m making an overly strong case in this post. We have finite time, and there are trade-offs. If you _always_ chase upside risk, you’re Pascal’s Mugging yourself out of any free time. Sometimes there are hidden big _downside_ risks. But I see a _lot_ of instances of safe, moderate actions with upside risk that aren’t taken. And, at least in my social circles, I think on the margin people should seek upside risk way more!

So if the ideas in this post resonated with you, **live it**! Keep score in terms of EV. Seek novelty. Try new things. Go outside your comfort zone. Ask for things. Notice the _massive_ pile of potential wins in your life, and ask whether you’re OK with leaving all of that on the table.

What’s a source of upside risk you could seek _right now_?