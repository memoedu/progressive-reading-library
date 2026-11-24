People sometimes tell me that they want to join a startup, so that they can learn how it works, and eventually start one themselves. I usually end up suggesting that they skip straight to step 2 and start one themselves.

Why is that? Isn’t it better to learn from someone else’s mistakes than to have to make all of them yourself? At least for me, the answer’s been sometimes yes, but sometimes no.

* * *

For my first few years as an engineer, I was lucky to work with some mentors who taught me a ton about engineering. Things like:

-   How to name things well, and why that’s so important.
    
-   How to organize code so that it’s easy for future readers to navigate.
    
-   How to factor code into cohesive, decoupled modules.
    
-   How to keep my software designs simple and understandable.
    
-   How to write documentation that other people can understand.
    

By giving me fast, frequent feedback on these things, they taught me much more quickly than I could learn on my own. Eventually I’d end up building models of particular people in my head—I could ask myself “what would Ping think of this name?” or “how would Abeer factor this code?”

Of course, my predictions wouldn’t be perfect, because I only partially understood the [mental models](https://fs.blog/mental-models/) that my mentors were applying when they gave me feedback. But I’d predict correctly, say, 95% of the time, which was enough to help me become a much better engineer.

* * *

After a few years, though, I ended up in a situation where I didn’t have access to those mentors. We’d split Wave into two divisions, and I ended up in the smaller division (“mobile money”) where I was usually the most technically opinionated engineer.

I spent a long time building the mobile money team and codebase without much feedback from more-experienced engineers. That required me to make a bunch of higher-stakes, longer-term decisions on my own:

-   How to prioritize between different potential large-scale code improvement projects.
    
-   What kind of hiring bar is reasonable, and how to build an interview process that reliably upholds that bar.
    
-   How to decide whether a non-[boring technology](http://boringtechnology.club/) is worth adopting.
    
-   How to staff teams so that they have the right mix of skills and personalities to gel and build great things together.
    
-   When to keep working on the plan for a tricky technical project, and when to bias towards action and ship it.
    
-   How to help coworkers navigate towards the role that resonates with them the most.
    

Because the stakes are higher and it takes longer to see the results, all these decisions require what I’d call _conviction_: the confidence that your idea is good enough that it’s worth throwing a lot of effort behind.

If you’re designing a hiring process, or investing in a particular code migration, or moving people between teams, you’re committing to spending a lot of resources following through: training new employees, finishing the migration, etc. In order for that commitment to make sense, you want to be confident that your plan is optimal before you pull the trigger.

It turned out that, even though I was good at lower-stakes engineering decisions, I was still pretty bad at ones that required conviction—I couldn’t get confident enough to commit to anything.

* * *

My first conviction-requiring decision involved designing and prioritizing some large-scale improvements to our codebase. I tried to make a plan by asking myself “what would my mentors do?” But every time I asked, I’d come up with something uninspiring, or just draw a blank.

I knew exactly which parts of our codebase they’d point out as the biggest problems, but that wasn’t the only decision I faced—I also needed to envision what the problem parts should ultimately look like, and then find the fastest path to get from here to there, and then spend months executing the plan. There were too many decisions, and the stakes were too high, for my 95%-accurate simulated guesses to be good enough.

In a normal time, I probably would have ended up just doing nothing, since it was so hard to make a plan I had conviction in. Right then, though, our business was temporarily shut down (long story), so codebase improvements were all I had to do.

Since I had plenty of free time, I spent it thinking through my plans from first principles. Instead of pattern-matching on advice I’d been given before, I spent weeks (part-time) iterating on designs, exploring their implications, and bouncing them off other people.

Then we actually implemented the ideas. Some of them turned out to be net-negative—for instance, we tried changing the way we wrote data models, and ended up reversing course later. Others turned out to be way more helpful than I expected—for example, introducing a rational directory structure made it tremendously easier for new hires to navigate our codebase.

Overall, I probably did a pretty bad job. But, importantly, I was able to see my mistakes play out in the real world. Instead of modeling what other people would tell me to do, I built a model of the problem directly. So when I got negative feedback, it wasn’t “Mentor X thinks this plan is bad” but “the world works differently than you expected.”

When you’re implementing a bad plan yourself, instead of having a mentor bail you out by fixing it, a few really useful things happen:

-   You learn many more details about why it was a bad idea. If someone else tells you your plan is bad, they’ll probably list the top two or three reasons. By actually following through, you’ll also get to learn reasons 4–1,217.
    
-   You spend about 100x more time thinking about how you’ll avoid ever making that type of mistake again, i.e., digesting what you’ve learned and integrating it into your overall decision-making.
    

By watching my mistakes and successes play out well or badly over the course of months, I was able to build much more [detailed, precise models](https://www.lesswrong.com/posts/B7P97C27rvHPz3s9B/gears-in-understanding) about what does and doesn’t matter for long-term codebase health. Eventually, that let me make architectural decisions with much more conviction.

This pattern repeated itself across lots of different types of hard decision. I’d start out too uncertain to act with conviction; I’d procrastinate or implement bad plans; but after enough iterations of that, I’d end up understanding a lot more about the problem space. Ultimately, I’d end up with a broad base of [tacit knowledge](https://en.wikipedia.org/wiki/Tacit_knowledge) and heuristics that were richer than anything I could get from reading books or talking to people. At that point, I’d finally be able to build the conviction I needed to make good calls.

* * *

This applies to any domain that’s open-ended and requires a lot of complex decisions with long time horizons.

Take the original example of running a company. By far the most important part of the CEO’s job is the high-conviction decisions. What product should we build? What strategy should we pursue? Who should we hire? And so on.

If you join a company where someone else is already making those decisions well, you’ll never get the type of practice that you need in order to build your own models and heuristics. You’ll end up with a good, but not perfect, model of “what would my boss do?”—a model that can make the 95% of easy decisions, but not the 5% of hard ones that add the most value.

Having mentors can help you quickly go from okay to great. But to get from great to exceptional, you’ll need to make good decisions when the stakes are higher and the consequences are longer-term. For that, you need a kind of conviction that you can’t learn from mentors—only from your own mistakes. So get out there and make some!

_Thanks to Eve Bigaj for reading a draft of this post._