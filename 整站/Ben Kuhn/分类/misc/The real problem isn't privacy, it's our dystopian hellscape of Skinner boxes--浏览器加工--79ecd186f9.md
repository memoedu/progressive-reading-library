“If you’re not paying, you’re the product!” People [love](https://lifehacker.com/5697167/if-youre-not-paying-for-it-youre-the-product) to [repeat](https://www.reddit.com/r/explainlikeimfive/comments/2m3f05/eli5_if_something_is_free_you_are_the_product/) [this](http://blogs.harvard.edu/futureoftheinternet/2012/03/21/meme-patrol-when-something-online-is-free-youre-not-the-customer-youre-the-product/) [complaint](https://techcrunch.com/2013/10/11/you-make-ads-better/):

> We pay, for example, for the awesomeness of Google’s free array of services by helping Google build an enormous database filled with our billions of searches and personal information. … This week as you scour the internet and stores for holiday deals, keep in mind that when you’re not paying with cash you’re paying with your personal information.

> What it means is that if a website is spending its time and resources to deliver content to you without asking for anything in return, then they are probably selling information about you to others to make money.

> free services usually make money by extracting lots of data from users — and then selling that data, or using it for targeted availability of those users for advertising, to advertisers.

> if you’re not paying to use a service, you can expect that your data will be used to make money in some way.

To be honest, I think much of this fretting about privacy is misdirected.

Yes, there are plenty of groups that should be worried about protecting their or their users’ privacy—say, reporters, dissidents, Equifax, or companies operating in authoritarian political regimes. In these cases the path from non-privacy to harm is relatively obvious. But many privacy activists treat it as self-evident that any compromise of privacy is a deal with the devil. The results are uninspiring.

It’s silly, for instance, when Richard Stallman [suggests that people swap Charlie Cards](https://stallman.org/archives/2012-sep-dec.html#18_October_2012_%28San_Francisco_clipper_cards%29) to fool MBTA tracking, or when people [worry about who knows their browsing history](https://www.eff.org/deeplinks/2017/04/heres-how-protect-your-privacy-your-internet-service-provider), or [fret about software with opt-out telemetry](https://github.com/dotnet/cli/issues/3093), or when the EU makes _every single website_ give you an annoying pop-up about how you’re opting into cookies,[1](https://www.benkuhn.net/skinner/#fn:1) or when Mozilla [warns every Firefox user that “today’s gifts don’t come with \*privacy not included”](https://advocacy.mozilla.org/en-US/privacynotincluded/).

(Does GDPR fall into the “silly” category? I’m mostly suspending judgment. I’m worried because I think it had the same questionable motivations as the above, but I could easily imagine changing my mind.)

None of these examples cited any reasons why the privacy violations were worth worrying about. The [blog post](https://advocacy.mozilla.org/en-us/privacynotincluded/why-we-made) explaining Mozilla’s “privacy not included” campaign did claim that their reviewers asked the question “what could happen if something goes wrong”—but I couldn’t find any reviews that actually answered this question! Probably because they had some trouble coming up with a plausible story of someone nefariously abusing Adidas’s smart soccer ball telemetry.[2](https://www.benkuhn.net/skinner/#fn:2) In these cases, it’s not worth worrying much about whether you’re the product.

* * *

However, there’s a _much worse_ sense in which “if you’re not paying, you’re [the product](https://www.benkuhn.net/product)” is true. People have started worrying more about it recently, but still not nearly enough.

The problem isn’t just that GoogFace let advertisers know what you’re interested in. The problem is that this is _their only job_, as in, the only thing that makes them money. Any way in which GoogFace make you happy, solve your problems, or improve your life is more or less accidental.

In a well-functioning world, this wouldn’t be true, or at least wouldn’t be important. If Facebook didn’t make its users happy, they could leave and go use better products, and then the advertisers would leave and then Facebook would make less money. In fact, our world used to be closer to this situation, because Facebook didn’t know how to keep people using it except by making something they actually liked.

But then Facebook hired a bunch of psychologists and the psychologists taught them about [Skinner boxes](https://en.wikipedia.org/wiki/Operant_conditioning_chamber). Or maybe their userbase stopped growing and they started having to wring more usage from old users to keep the charts going up. Or maybe they realized they had enough of a network effect lock-in that they could start abusing their position. Anyway, they discovered a slew of ways to get more usage without actually having a better product.

For instance, you can pad the news feed with worse stories. People have to scroll past these stories to find the ones they actually want to see, so this increases engagement. It also creates what’s known in psychology as a [variable-reward schedule](https://www.verywell.com/what-is-a-variable-ratio-schedule-2796012)—basically, the action you take (scrolling down in the feed) is sometimes randomly rewarded and sometimes not. Variable-reward schedules lead to much more compulsive behavior, perhaps because they make the reward seem scarcer.

(If you Google “variable reward schedule” the top result is an article by a “growth hacker” and its literal title is: “Want to Hook Users? Drive Them Crazy”)

Or you can send people lots of notifications that aren’t actually important. If they ignore the notifications, no big deal; if they click on them, then you get more traffic and serve more ads. Or you can make the notification icon bright red so it’s impossible for people to ignore their notifications.

Up to a point, when you try to optimize for “what keeps users on my site,” the answer you’ll get is “make them happier.” But that’s just a correlation, and [at the extremes, the correlation breaks down](http://lesswrong.com/lw/km6/why_the_tails_come_apart/). Once you’ve plucked all the low-hanging fruit of actually solving people’s problems, the only thing left is [dark patterns](https://darkpatterns.org/).

* * *

Most people don’t notice how deeply things like Facebook have been compromised by their allegiance to advertisers, because they’re not in the habit of noticing when products they use could be better. I only started paying attention to this recently, because I started working on user experience at my job. But once I started looking for advertising-induced brokenness, it was everywhere.

Off the top of my head, here are a few examples that are obviously terrible for users but great for time-spent-on-site and great for advertisers. (My examples are from Facebook because it’s the ad-supported product I’ve used most recently. Other ones, except perhaps for Google search, seem equally bad but are less fresh in my mind.)

-   The news feed somehow fails to filter out random photos from people I friended four years ago and have never interacted with since.
    
-   It even fails to filter out text-only posts in languages I don’t understand (this was true before they had inline translations too).
    
-   The “see less of this type of post” news feed option simply doesn’t work—you can ask to see less of random photos as many times as you like with no results.
    
-   The news feed is often randomly reordered if you refresh, so that you lose your place and can’t tell when you’ve scrolled down far enough that you won’t see anything new anymore.
    
-   Despite knowing exactly where I am at all times, Facebook still notifies me about parties that are happening 3,000 miles away.
    
-   They frequently add new notification types with aggressive defaults that can’t be globally adjusted. (The most recent offender was fundraisers.)
    
-   If you’re logged out and you visit facebook.com, it _still displays your notification badge_ over your profile photo in the “recent logins” section. (Just to make sure you don’t miss any of those very important parties on the other coast!)
    

You might plead Hanlon’s Razor—that these decisions are incompetence, not malice, and Facebook just had some bad product managers. But you can see that’s not true by how Facebook behaves when its own users try to fix these problems themselves: it [responds](http://www.fbpurity.com/news/important-news-facebooks-legal-team-have-told-me-i-am-banned-from-facebook-because-of-f-b-purity/) by [banning](http://socialfixer.com/blog/2013/10/05/facebook-requires-social-fixer-browser-extension-to-remove-key-features/) them—and censoring links to discussion of the ban. The crap in your news feed is _sacred_ crap, and if you question it you will be excommunicated.

* * *

OK, so what? Plenty of companies do things that aren’t in their customers’ best interests. Facebook’s Skinner box certainly isn’t comparable to the things, say, cigarette companies have done!

Except… is that actually true? Suppose that time spent on Facebook is, on average, wasted, as if that time just disappeared. (Obviously, some time on Facebook isn’t wasted—say, if you find an interesting article or learn that one of your friends just got engaged—but some of it is also worse than wasted, say if you get sucked into an argument with a troll that just makes you angry.) The average Facebook user spends [50 minutes a day](https://www.nytimes.com/2016/05/06/business/facebook-bends-the-rules-of-audience-engagement-to-its-advantage.html) on Facebook properties—multiply by 2 billion active users for a total of 100 billion minutes lost per day.

On the other hand, the world smokes [16 billion](http://www.tobaccoatlas.org/topic/cigarette-use-globally/) cigarettes per day; each one shortens the lifespan by [11 minutes](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1117323/) for a total of 176 billion minutes lost. So on the back of the envelope, Facebook could be on the same order of badness as the global cigarette industry.

Of course, the back of the envelope comparison is obviously hyperbolic. In particular, Facebook time is probably somewhat valuable on net. (Then again, cigarettes aren’t pure negative either!)

How valuable? It’s hard to say. If people are voluntarily doing something, we should generally presume that they value it. But my point is that Facebook is sufficiently coercive and manipulative that they _don’t_ deserve (as much of) that presumption. At least some of Facebook’s users came for the event-invite monopoly and stayed for the dark patterns. Those users would be better off if Facebook didn’t exist.

How many of those users are there? I have no idea, but probably a lot more than have been materially harmed by Facebook’s cavalier treatment of data privacy. Don’t get me wrong, it’s great that people are finally starting to worry about privacy. But we should focus our spleen where the actual harm is: in ad funding that incentivizes addictiveness and user-hostile design.

* * *

1.  Did you know you can block these notices with uBlock Origin? I learned this while writing this and it made me a lot happier! Now if only I could block them on mobile, where they take up half my screen… [↩︎](https://www.benkuhn.net/skinner/#fnref:1)
    
2.  Sure, it could happen, but it’s not high on my list of “things worth launching awareness campaigns over.” [↩︎](https://www.benkuhn.net/skinner/#fnref:2)