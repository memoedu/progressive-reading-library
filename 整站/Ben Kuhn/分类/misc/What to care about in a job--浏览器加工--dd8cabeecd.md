Someone recently asked me if I had any updates on my [2014 post on job hunting](https://www.benkuhn.net/job). I haven’t done a systematic job search since writing that post (I decided to join Wave without considering any other offers at the same time), but I did learn more about what matters to me in a job. If I were doing another job search, here’s how I’d update the criteria and ranking from the [“Filtering” section](https://www.benkuhn.net/job#filtering) of that post:

Contents

-   [1\. Growth](https://www.benkuhn.net/job2/#1-growth)
-   [2\. Alignment](https://www.benkuhn.net/job2/#2-alignment)
-   [3\. Impact](https://www.benkuhn.net/job2/#3-impact)
-   [4\. Mentorship](https://www.benkuhn.net/job2/#4-mentorship)
-   [5\. Responsibility](https://www.benkuhn.net/job2/#5-responsibility)
-   [6\. Work environment](https://www.benkuhn.net/job2/#6-work-environment)
-   [Coda: Anti-priorities](https://www.benkuhn.net/job2/#coda-anti-priorities)

* * *

## 1\. Growth

When I joined Wave, I thought I’d mostly work on adding new countries to our mobile app for international remittances. Instead, I rewrote our core transaction processing code (twice), built a way to transfer to bank accounts, designed an accounting system (twice), worked on a [USSD](https://en.wikipedia.org/wiki/Unstructured_Supplementary_Service_Data) app for domestic money transfer, lived in Ethiopia (twice), built a bunch of anti-fraud tools, managed a team (twice), and lots more.

The reason I got to wear so many different hats is that Wave grew really quickly. This makes more of a difference than you would think, because [your intuitions about compound growth are wrong](http://cdixon.org/2015/05/12/exponential-curves-feel-gradual-and-then-sudden/). It never seems crazy to add 10% more users in a week—until suddenly your support queue is three days long and all your engineers are on fire. To stay ahead, you’re forced to improve more and faster than you think is possible.

Growth makes everything else better: it increases your impact, makes your job more important, and presents new problems for you to learn how to solve. So (sustainable) fast growth is the number one thing I’d look for.

## 2\. Alignment

In the comments of my previous post [I wrote](https://www.benkuhn.net/job#comment-3):

> I actually don’t think my decision was very strongly influenced by personal fit with the company. That is—I knew I liked the founders and got along well with them, but I get along well with many people and fit in well at many companies. None of the places I interviewed stood out particularly in that respect.

In my experience since then, fit has actually mattered a ton, but in ways that I wasn’t thinking about at all in 2014.

I still think the tech-bro sense of “culture fit” (“would I have a beer with this person?”) is silly, but there’s a more subtle kind of fit that seems to be really important for how effective people are. Something like—how well do you understand how your company prioritizes, makes decisions, or generally models the world? The closer you are to your coworkers on these dimensions, the more trust and autonomy you’ll get, and the better you’ll feel about the direction of your work. Conversely, if you disagree about some deep value judgment, it’ll be a constant source of friction that’s probably impossible to fix.

I tried to think of good concrete examples of the kind of fit I’m talking about, but they all needed an essay’s worth of explanation. The best I can do might be to point to [GiveWell’s write-up of their “deep value judgments and worldview characteristics”](https://blog.givewell.org/2013/04/04/deep-value-judgments-and-worldview-characteristics/) as the kind of thing it’s important to be aligned on.

## 3\. Impact

When I did my first job search, I didn’t think there were good opportunities in software to work on things that directly did good in the world. Instead, I focused on building skills that seemed broadly useful. (In my case, I picked machine learning/data science.)

Since then, great software jobs have come up in global poverty (Wave, GiveDirectly, Segovia), AI risk (OpenAI, MIRI, DeepMind), and EA advocacy (CEA, 80,000 Hours). I’m sure that if I actively looked I could find lots more openings. I think the difference is that the effective altruism community is bigger and more mature now, so there are more and bigger EA projects, which means more room for specialists like programmers.

## 4\. Mentorship

In school, the feedback I got on my work was mostly limited to a few marginal scrawls and an A minus. (Thanks, grade inflation!) So I was in for a surprise when I had my first code review at Theorem: my reviewer had left about 10 comments on my 20-line patch.

In retrospect, it’s obvious why he cared that much—he was actually on the hook if my code was bad. So he gave actual feedback. And since he was a great programmer, I got better really quickly. The same thing happened even more with skills nobody teaches in college, like figuring out how a feature should work, prioritizing between projects, or explaining code to coworkers.

In 2014, I’d never worked with anyone who was much better at software work than me and cared intensely about my success. (In school, nobody cares that much about your success, except for in limited senses like not failing out of courses. Even at internships, you probably won’t work on something that’s critical to the company.) Because of that, I underestimated how much of a difference this could make.

Unfortunately, I don’t have a good idea of how to find this kind of mentor. I lucked into working with several such people at Theorem, and even more so Wave, but even when I took the job, I didn’t have much of a sense of how awesome it would be.

## 5\. Responsibility

In 2014 I wanted to work for the smallest company possible so I could do important things right away. While I still value having lots of job responsibility, I’ve changed my mind about the best way to achieve it.

My first big project at Wave was to allow people to send money to bank accounts in Kenya. Unfortunately, this was my first time _ever_ shipping a big new user-facing feature. And so the CEO, Drew, had to hold my hand through every decision, including things you might think would be obvious.

(For instance: when banks are involved, you probably need to test more than once. In fact, you should test on different days and ideally different weeks, because the reconciliation process that you thought was automated might actually be done by some guy who occasionally goes on vacation without telling you beforehand. Ahem.)

Needless to say, I learned a lot, but I didn’t have any real responsibility; I was writing the script for the computers but Drew was, effectively, writing a script for me.

A year later, we had a big wave of fraud attacks. Drew was really busy with other things, so our conversation went (simplified for brevity):

> **Drew:** Can you solve this fraud problem?
> 
> **Ben:** Signs point to yes.
> 
> _\[time passes\]_
> 
> **Ben:** Okay, how about we \[redacted\].
> 
> **Drew:** Sounds great, ship it.

What changed? We were 4x bigger, so you’d expect responsibility to be more divided. But I’d also shipped enough good features to be trusted with way more autonomy. So my effective responsibility was much higher.

Working at a small company will give you some responsibility by default. But it’s more important that the company is growing fast and has a fast promotion/transfer cycle. You’ll learn way more as a “late” junior engineer at the next Google than employee #1 of a company that never gets off the ground.

## 6\. Work environment

I feel like this shouldn’t be that hard to optimize. It’s the 21st century—surely we’ve solved the problem of physical spaces that are unpleasant? But then I think back to Theorem and how many people I nearly punched because they were making too much noise.

Every job I had before working at Wave was in an open-plan office, which was [insanely distracting](https://www.benkuhn.net/oo). My day-to-day productivity and happiness literally doubled when I started working remotely instead. (For me, a private room in an office would probably be about as good as remote work—even better, if the commute is short.)

I’m actually very confused by how many well-funded companies have open-plan offices given the widespread hate for them. It might make sense as a way to cut costs, but surely that’s not important to places like [Facebook](https://www.designweek.co.uk/issues/6-12-april-2015/inside-facebooks-new-office-the-largest-open-floor-plan-in-the-world/)? Anyway, it seems weirdly hard to find companies whose space is actually nice to work in.

## Coda: Anti-priorities

It’s easy to say that you care about things, but prioritization also means deciding what you _don’t_ care about. So here are some things that I don’t try to optimize for. (As a disclaimer, these priorities are what work for me, right now, [at the margin](http://www.econlib.org/library/Topics/College/margins.html), because I’ve been super lucky—I could easily imagine having to change them!)

-   **Hard technical problems.** I originally thought I really wanted to work on hard technical problems because otherwise I would get bored. This was based on [my experience in school](https://www.benkuhn.net/h) where non-math courses all felt really slow. But in the real world [there is no speed limit](https://sivers.org/kimo), so I get to use my whole brain on everything. I mostly haven’t been bored—even when I was doing accounting.

-   **Income.** I have a pretty low cost of living, so if I make more money, it’ll go towards donations. But it seems like most of my impact comes from my work itself, not my donations, so I’m better off trying to optimize that. (If I wanted to do something expensive like buy a house or start a family, I might start prioritizing this more.)
    
-   **Location.** I originally thought I really wanted to live in the Bay Area, but after moving there I got kind of [annoyed](https://www.benkuhn.net/nonprog) with that “scene,” so I don’t have a strong city preference anymore. Or a country preference—I like the US more than Africa (all else equal), but in big cities the difference isn’t nearly as big as I expected.
    
-   **Security.** I don’t have dependents and the software job market is really good right now, so I’m okay with a relatively high risk of my job ending.
    
-   **Work-life balance.** I really like programming, so I’m happy working long hours on things that I care about. I still need time to myself, but I don’t feel a need to be strict with boundaries. (When I’ve worked on things that are less intrinsically satisfying to me, like accounting or management, I’ve prioritized work-life balance more.)