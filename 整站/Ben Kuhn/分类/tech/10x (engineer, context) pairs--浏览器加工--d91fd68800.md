People have big arguments over whether “10x engineers” exist or not. [Pro](http://worrydream.com/refs/Brooks-NoSilverBullet.pdf):

> The differences are not minor—it is rather like Salieri and Mozart. Study after study shows that the very best designers produce structures that are faster, smaller, simpler, cleaner, and produced with less effort. The differences between the great and the average approach an order of magnitude.

[Contra](https://web.archive.org/web/20131205042721/https://medium.com/about-work/6aedba30ecfe):

> There is no conclusive body of scientific research to suggest that the 10x engineer is in fact a real phenomenon, much less one that provides us with a deep and actionable understanding of the factors, conditions and stipulations of their existence. Yet we continue to regurgitate the mythology, over and over again.

The “anti 10x engineer” crowd is right that the quality of the cited research is highly dubious. It’s especially dubious if you imagine that these supposed 10x engineers are literally writing the same code 10x faster, as [many non-technical people seem to think](https://twitter.com/skirani/status/1149302828420067328). (“Their keyboard keys such as i, f, x are usually worn out than of \[sic\] a, s, and e (email senders).” Come on, really?!)

On the other hand, you don’t need studies to know that engineers do vary by orders of magnitude in their output of _all-things-considered usefulness_. Compare say, my output with that of [Jeff Dean and Sanjay Ghemawat](https://www.newyorker.com/magazine/2018/12/10/the-friendship-that-made-google-huge): they’ve clearly been at least 10x more productive, probably more like 100x. But that’s not just because Jeff and Sanjay can solve individual problems 100x faster than I can. Instead, they gave themselves leverage by [choosing the right problems to solve](https://news.ycombinator.com/item?id=5496914):

> For those who work inside Google, it’s well worth it to look at Jeff & Sanjay’s commit history and code review dashboard. They aren’t actually all that much more productive in terms of code written than a decent SWE3 who knows his codebase.
> 
> The reason they have a reputation as rockstars is that they can apply this productivity to things that really matter; they’re able to pick out the really important parts of the problem and then focus their efforts there, so that the end result ends up being much more impactful than what the SWE3 wrote.

Most importantly, Jeff and Sanjay also found the right context—a hypergrowth company blocked on infrastructure scaling problems—where the things they were great at building were extremely useful.

Posts on topics like “how to find a 10x engineer” encode the assumption that “10x-ness” is some immutable trait of a person. But in reality, that type of leverage can be incredibly specific to one particular set of circumstances. Jeff and Sanjay weren’t 100x more productive than average at their previous companies; there was something about the Google environment that worked particularly well for them. Anecdotally, I’ve seen many people go through similar swings in productivity at different times:

-   My first job out of college was at a company that ran a hedge fund investing in [peer-to-peer loans](https://en.wikipedia.org/wiki/Peer-to-peer_lending); my second was at [Sendwave](https://www.sendwave.com/). A main goal of both of those products was saving money for our users (hedge fund investors and East African immigrants, respectively), but my work at Sendwave led to saving well over 10x more money, because the scale of the product was bigger and the improvements I was making were more important.
    
-   I did know one person who literally seemed to write code 10x as me or anyone else I knew. I took an operating systems course with him. Despite his prodigious ability to crank out code, he ended up pulling just as many all-nighters as the rest of us because he got distracted by building (amazingly impressive) development tools in his favorite programming language instead of directly solving the problems in the assignments. After graduating, he took a job where he could spend all his time building amazingly impressive development tools in his favorite programming language; I’m confident he’s a “10x engineer” there.
    
-   The CEO of Wave had two previous jobs, in which he (1) sold a few hundred thousand handcarts in rural Tanzania, and (2) built ~12 failed social mobile apps. Then he started an extremely successful remittance company.[1](https://www.benkuhn.net/10x/#fn:1) He’s not (primarily) an engineer, but the variation in effectiveness here is probably more like 3-5 orders of magnitude.
    

-   A coworker of mine was ordinarily an extremely quick and productive engineer. He also derived a lot of motivation from working with teammates, and could be easily distracted when it wasn’t well-defined what his next task was. I foolishly asked him to work on a project that didn’t overlap with most of the rest of the company’s work, and involved a lot of ill-defined design work and troubleshooting with a long feedback loop. That reduced his productivity by about a factor of 10 for a couple months.

The common thread in these examples is that your actual output depends on a lot more than just how quickly you finish a given programming task. It also requires you to:

-   Identify the right problem
-   Build up enough context that you can try to solve it
-   Identify the best way to solve the problem
-   Convince any relevant people that your solution is the best one
-   Build the solution \[this one step is where the actual programming happens\]
-   Get the solution into production
-   Learn what was wrong with your solution
-   Come up with improvements based on what you’ve learned
-   Explain the solution to other people
-   Maintain the solution over time as your needs change

All the other non-programming tasks on this list depend deeply on the way you interact with the organization around you:

-   Whether you’re in an environment that motivates and energizes you, or saps your will to live
-   Whether the problems that you care about are aligned with your team’s goals / mandate
-   Whether your high-level problem-solving approach meshes well with the needs of your team
-   Whether you’re able to build enough credibility to be trusted to solve important problems
-   Whether you’re able to successfully navigate the environment (technical and organizational) that you’re trying to build the solution within
-   Whether the people that need to use your solution to the problem are incentivized to adopt it
-   Whether you’re able to explain your solution to the people that need to use it

These factors can vary hugely between different roles, even within the same team and company. The “10x more productive” people I’ve seen have been both (a) very conscious of the above factors, and (b) adept at navigating towards situations where those factors push them towards success rather than failure. Similarly, as a manager I view one of my most important jobs as helping people find the role and environment that works the best for them.

* * *

1.  You could argue that the failed social mobile apps were equally productive in expected-value terms, but I actually don’t think that’s true; consumer apps are underrepresented in the top YC companies and the pivot to doing something “real” increased productivity in two ways: it improved morale, and it helped the founders navigate more effectively towards a business with high potential. [↩︎](https://www.benkuhn.net/10x/#fnref:1)