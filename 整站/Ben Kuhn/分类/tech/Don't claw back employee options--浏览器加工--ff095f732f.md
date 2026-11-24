Scott Kupor of Andreessen Horowitz recently[1](https://www.benkuhn.net/clawback/#fn:1) [suggested](http://a16z.com/2016/06/23/options-timing/) an alternative structure for employee stock options:

> But, a way to truly compete for the very best and long-term oriented employees would be to offer even greater amounts of employee options grants. For example, why not offer stock option grants that are 50% more than the nearest competitor’s — but with the provision that a departing employee cannot exercise his or her stock options unless there has been a liquidity event? If you stay, you’re a serious owner, but if you don’t want to be part of the company for any reason you won’t be an owner. This solves all of the issues: cash rich vs. poor; competitive offers; and the bad incentive problem (e.g., encouraging employees to quit to build their own diversified stock portfolios).

Adam D’Angelo [already pointed out](https://dangelo.quora.com/10-Year-Exercise-Periods-Make-Sense?share=1) some good counterarguments to Scott’s points. But I disagree with Scott strongly enough that I’d like to add some more of my own. Adam wasn’t very explicit about this, but the proposed scheme is _incredibly_ employee-unfriendly, and destroys a lot of value compared to friendlier schemes. Here are some reasons why:

## 1\. “Dead” equity isn’t dead

Scott coins a term “dead equity” to describe what happens when an employee leaves but holds onto their options. He coins it by analogy to a thing called “dead money” in the NFL:

> For example, a signing bonus for a player, although paid out up front, must be allocated for salary cap purposes across the term of the player’s contract. Thus, a team that elects to cut a player after year three of a five-year contract gets charged the remaining 2/5ths of the signing bonus for those final two years even though the player is gone from the team.

I actually find it hard to explain all the different confusions underlying both the analogy, and the claim that dead equity is bad. First, Scott seems to imply that “dead money” is an artifact of some silly NFL rule and it should go away. But it’s not! The player’s signing bonus _has_ been paid, so it _should_ get fully counted against the salary cap at some point. If you were going to get rid of the “dead money” problem you’d do it by assessing the entire signing bonus against the first-year salary cap, but that results in exactly the same total money, just distributed differently through time.

More importantly, we already have provisions analogous to taking back dead equity. It’s called vesting! The way in which equity is granted to employees is _not at all analogous_ to a football signing bonus—instead of being front-loaded, it’s slightly back-loaded due to the vesting cliff. By the time you actually own the options, you’ve _earned_ them for the work you _already did_. When an employee’s already-vested options expire, it’s less like axing their signing bonus, and more like clawing back cash you already paid them for their salary.

Scott seems to be under the impression that an employee’s options should only increase in value to reward them for work they’re doing right now. But that’s not how options work! The reason options are worth anything at all when they’re granted is that you think that the underlying stock has some probability of going up in the future. When it actually _does_ go up, that’s just the already-vested options realizing the value you already placed on them. It’s part of the compensation for stuff you already did, not for what you’re doing now.

## 2\. Equity clawbacks benefit founders and investors, not employees

Scott characterizes dead equity as “a direct wealth transfer from the employees who choose to remain at the company and build future shareholder value, to former employees who are no longer contributing to building the business/its ultimate value.” This is wrong.

More precisely, it’s only true in a certain very unrealistic model of the world, in which the size of the employee stock option pool is completely fixed—independent of how much equity the management wants to grant to employees. If that’s the case, then yes, every time a former employee’s option expires, it frees up exactly one option to be granted to a new employee.

However, in the real world, management can change the size of the employee option pool! If managers always size the option pool appropriately to how much equity they want to grant to new hires, then the options “freed up” when an employee leaves won’t go straight to new hires—they’ll simply cause fewer shares to be issued the next time the option pool is expanded, which will reduce everyone else’s dilution proportionally to the size of their equity stake.[2](https://www.benkuhn.net/clawback/#fn:2)

That means those who benefit most from clawing back the options are the ones with the most existing equity: the founders and investors. (So it makes sense that venture capitalists would be in favor of it!)

## 3\. The math doesn’t work

Scott suggests increasing the total equity package by 50% in exchange for requiring no exercise until a liquidity event. The no-exercise-before-IPO rule has roughly the same effect as increasing the vesting period by a factor of 2 for early employees (since they will have to stay at the company ~8 years to get anything from the options, instead of 4 or less with the status quo).[3](https://www.benkuhn.net/clawback/#fn:3) That means that, per unit time, employees are earning _less_ equity under Scott’s proposal! Yikes.

You could fix this by pumping up the multiplier from 50% to, say, 200%, but that would seriously dilute the remaining shareholders. The fundamental problem is that Scott’s option structure destroys value by making the options dramatically riskier. Even at a 200% multiplier I probably wouldn’t take Scott-options over normal options with a 10-year horizon, and I don’t think it’s possible to salvage the structure by tweaking the multipliers.

## 4\. The ability to change jobs is incredibly valuable

Scott’s proposal essentially prevents people from leaving the company before it exits (since they can’t exercise their options until that happens, and the options expire when they leave). Scott might have somehow not realized, but pre-committing to work at one company until IPO is _incredibly_ costly for an engineer because it absolutely wrecks their negotiating leverage.

On their own, tech companies are much less likely to give employees large raises, than to pay new hires lots more than they made at their old job. That means the fastest way for an employee to get a raise is to apply to other jobs, get a high offer, take it back to their current company and use it as [negotiating leverage](https://en.wikipedia.org/wiki/Best_alternative_to_a_negotiated_agreement) to get a larger raise.

Of course, if leaving the company would cause you to lose tens or hundreds of thousands of dollars through equity clawbacks, then that goes completely out the window. You’d never be able to get an offer good enough that it’s better than staying at your current place, even if the current place never gives you a raise. And so you’ll never be able to make a credible threat to leave if you’re undercompensated or otherwise unhappy.

Essentially, by agreeing to Scott’s terms, you’re granting the company management control over the majority of your net worth for the next 6-10 years if the company does well. I wish most founders were trustworthy and well-meaning enough for that to be a good idea.

And this is without even mentioning all the reasons I might want to leave a job other than a promotion. What if I decide I hate the Bay Area and my company won’t let me work remotely? What if I get bad carpal tunnel and can’t program anymore? What if a coworker starts harassing me? In these situations, I’d be utterly at the mercy of the founders to grant me a “special exception” to the anti-exercise rule. And of course if I were fired—not that uncommon of an occurrence!–I’d be utterly out of luck.

## 5\. Equity clawbacks make zombie employees even zombier

Late-stage startups with short option expiry horizons often have a problem with “zombie employees” who are phoning it in because they don’t want to work there anymore but can’t leave without their options expiring, and can’t exercise their options without incurring a large tax liability. Not only do these employees barely contribute themselves, but they’re a drain on morale and productivity for everyone else, too.

Currently, there are some ways to mitigate the tax liability by having a third party cover it, for a fee—for example, you can work out a deal with the [ESO Fund](http://employeestockoptions.com/), although I’m told they’ll rip your face off on pricing. This allows zombie employees to quit, albeit at a significant haircut. But Scott’s proposed structure completely rules that out. It also rules out all other anti-zombie measures, like early exercise or pre-IPO liquidity for employees, unless you get a special exception from the founders. A special exception involving a bunch of legal legwork, which they have no incentive to give you (except for the goodness of their hearts), and plenty of disincentive.

In fact, if zombie employees aren’t fully vested yet, then allowing them to leave actually costs less! For instance, suppose I get bored after year 3 our of a 4-year vesting schedule, but my equity is 1% of a $1b company. With normal options, I could quit and take home 0.75% of the company. With Scott’s options, I’d have been granted 1.5% originally (to make up for the less favorable terms), and after zombie-ing it out for another 5 years, I’d walk away with twice as much equity, having done about the same amount of work. This is a terrible trade for both me (who would have been willing to walk away with half the amount five years ago) and the company (who lost an extra 75 basis points of equity).

## 6\. You don’t want to filter for 8-year employees

At least not the way this scheme does!

Scott argues that his option scheme selects for the “best and most long-term-oriented employees.” It’s a great idea to try to find employees who would be willing to work for your company until after the IPO. But Scott’s idea selects for people who are _willing to commit to that up front_ on pain of losing their equity\*.\* Eight years is an incredibly long time, and when you commit to stay at a job for that long, you put yourself at an enormous risk of things not turning out as well as you’d hoped.

No one should have to make such a huge commitment up front based on a few interviews with the company—and your best potential employees will refuse to. Hell, many _marriages_ last less long than a company takes to IPO, and people take years to make that kind of commitment! It’s unreasonable to expect an employee to make a similar call after a mere week or two of interviewing.

## 7\. Egalitarianism is a red herring

Scott argues that the current option compensation scheme is inegalitarian because exercising options requires an up-front cash investment. I’m piqued that he tried to deploy this argument while arguing in favor of such an employee-unfriendly scheme. Under Scott’s option structure, employees would lose their ability to quit without losing most of their wealth, giving management and investors almost complete control over their career. And if, as described above, the option pool wasn’t a fixed size, the expired options would mainly counteract dilution for the largest equity holders—again, management and investors. The proposal would increase inequality between founders/investors and employees just as much as it would decrease inequality among employees.

Furthermore, since Scott suggests that management can make special exceptions to give certain employees liquidity or allow them to leave, it’s quite possible that the scheme won’t be any more egalitarian at all. If the founders and investors don’t treat their employees impeccably fairly, it just replaces the unfairness of who can afford the exercise bill, with the unfairness of who can get in the founders’ good graces enough to get special dispensation for their next career move or realize some of their gains.

## Conclusion

An option scheme in which options are clawed back whenever employees leave is the most employee-unfriendly compensation scheme I’ve ever heard of. It achieves this in three main ways:

-   It transfers wealth from employees to founders and investors, by clawing back equity when employees inevitably leave anyway, and by destroying almost all employee-side leverage when employees negotiate with founders over equity.
-   It creates an immense amount of downside risk for employees out of thin air, by exposing their wealth to any random shocks that causes them to lose their job.
-   It increases job market friction a lot, which destroys a huge amount of value.

You could argue that one can compensate for all of these by increasing the size of your equity grants to compensate. However, since a lot of the unfriendliness is due to value being _destroyed_—not just transferred from employees to investors—it would require an unreasonably large increase in grant size to compensate.

For instance, right now I think it’s very likely I’ll keep working at Wave until it either fails or has a liquidity event. But even if Wave tripled my already substantial option grant, I wouldn’t be willing to sign onto this scheme and risk losing my all my equity if something unexpected changes my mind (or forces my hand) sometime in the next 6 years. Simply put, a clawback scheme like this is dead on arrival.

_Thanks to Dan Luu for prodding me to publish this and reading a draft!_

* * *

1.  OK, not so recently anymore, but I held off publishing this for several months because I was sure I’d misunderstood something about the proposal. [↩︎](https://www.benkuhn.net/clawback/#fnref:1)
    
2.  Also, in the real world the option strike price changes, which means the new option to be re-granted will be less valuable than the old one that expired (and the differential gets paid to the company in cash, so it’s absorbed proportionally by all equity holders). However, this effect is relatively small compared to the effect of being able to resize the option pool. [↩︎](https://www.benkuhn.net/clawback/#fnref:2)
    
3.  Of course, if you join midway along the path to IPO under Scott’s rules you might have to wait less than 8 years, but on the other hand, if there’s normal vesting, you get part of your stock after much less than 4 years, so the caveats cut both ways. [↩︎](https://www.benkuhn.net/clawback/#fnref:3)