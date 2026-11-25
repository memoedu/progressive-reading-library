# 1\. “Dead” equity 并没有死

---

## 正文

Andreessen Horowitz 的 Scott Kupor 最近[1](https://www.benkuhn.net/clawback/#fn:1) [suggested](http://a16z.com/2016/06/23/options-timing/) 一种用于 employee stock options 的 alternative structure：

> 但是，要真正 compete 以争取最优秀、最 long-term oriented 的 employees，一种办法是提供数量更多的 employee options grants。例如，为什么不提供比最接近的 competitor 多 50% 的 stock option grants——但附带这样一项 provision：departing employee 不能 exercise 自己的 stock options，除非已经发生了 liquidity event？如果你留下，你就是一个认真的 owner；但如果你出于任何原因不想再成为 company 的一员，你就不会是 owner。这就 solves 了所有这些 issues：cash rich vs. poor；competitive offers；以及 bad incentive problem（例如，encouraging employees 去 quit，以 build 自己的 diversified stock portfolios）。

Adam D’Angelo [已经指出了](https://dangelo.quora.com/10-Year-Exercise-Periods-Make-Sense?share=1) 一些针对 Scott 观点的有力 counterarguments。但我对 Scott 的分歧如此强烈，以至于还想补充一些自己的看法。Adam 对这一点说得不太明确，但所提出的 scheme 是 _极其_ employee-unfriendly 的，而且与更友好的 schemes 相比，它 destroys 了大量 value。原因有以下几点：

## 1\. “Dead” equity 并没有死

Scott 创造了一个 term，叫作“dead equity”，用来描述 employee leaves 却仍 holds onto 自己的 options 时的情形。他是通过与 NFL 中一种叫作“dead money”的东西作 analogy 来创造这个说法的：

> 例如，player 的 signing bonus 虽然是预先 paid out 的，但为了 salary cap 的计算，必须在 player 的 contract 的整个 term 内 allocated。因此，如果一个 team 选择在一份 five-year contract 的第 three 年后 cut 一名 player，那么即使这个 player 已经离开 team，该 team 仍会在最后 two 年被 charged 剩余 2/5ths 的 signing bonus。

说实话，我觉得很难解释清楚这个 analogy，以及 dead equity 不好的 claim，背后所有不同的混淆。首先，Scott 似乎在暗示，“dead money”只是某条愚蠢的 NFL rule 的产物，应该被取消。但并不是这样！player 的 signing bonus _已经_ paid 了，所以它在某个时候 _就应该_ 被 fully counted against salary cap。如果你想消除“dead money”这个 problem，做法会是把整个 signing bonus 都 assessing 到 first-year salary cap 上，但这样得到的 total money 完全相同，只不过是在时间上的 distributed 不同罢了。

更重要的是，我们已经有与 taking back dead equity 类似的 provisions。它叫作 vesting！equity 被 granted 给 employees 的方式，与 football signing bonus _完全不 analogous_——它不是 front-loaded 的，而是由于 vesting cliff 而略微 back-loaded。等到你真正 own 这些 options 时，你已经通过自己 _已经完成_ 的工作 _earned_ 了它们。当 employee 的 already-vested options expire 时，这与其说像是 axing 他们的 signing bonus，不如说更像是 clawing back 你已经作为 salary paid 给他们的 cash。

Scott 似乎认为，employee 的 options 应该只为了 reward 他们眼下正在做的工作而 increase in value。但 options 并不是这样 work 的！options 在 granted 时之所以有任何价值，是因为你认为 underlying stock 在未来有一定的 probability 会 going up。当它真的 _确实_ go up 时，那不过是 already-vested options 在 realizing 你已经赋予它们的 value。它是你已经做过的事情所对应的 compensation 的一部分，而不是为了你现在正在做的事情。

## 2\. Equity clawbacks 会 benefit founders 和 investors，而不是 employees

Scott 将 dead equity 描述为“从选择留在 company 并 build future shareholder value 的 employees，向已不再为 building business/其 ultimate value 作出贡献的 former employees 进行的 direct wealth transfer。”这是错的。

更准确地说，它只在某种非常不切实际的世界 model 中才是 true：在这个 model 中，employee stock option pool 的 size 是完全 fixed 的——independent of management 想向 employees grant 多少 equity。如果是这种情况，那么没错，每当一个 former employee 的 option expires，就恰好 frees up 一个 option，可以 granted 给一名新的 employee。

然而，在现实世界中，management 可以 change employee option pool 的 size！如果 managers 总是根据自己想向 new hires grant 多少 equity 来适当地 size option pool，那么 employee leaves 时所“freed up”的 options 就不会直接交给 new hires——它们只会使下一次 option pool 被 expanded 时 issued 的 shares 更少，从而按其他所有人的 equity stake 的 size 成比例地 reduce 他们的 dilution。[2](https://www.benkuhn.net/clawback/#fn:2)

这意味着，从 clawing back options 中 benefit 最多的，是那些拥有最多现有 equity 的人：founders 和 investors。（所以，venture capitalists 会支持它，也就说得通了！）

## 3\. 这个 math 算不通

Scott 建议把 total equity package increasing 50%，以换取在 liquidity event 之前不得 exercise 的要求。对于 early employees，no-exercise-before-IPO rule 的 effect 大致相当于把 vesting period increasing 为原来的 2 倍（因为他们必须在 company 待上 ~8 年才能从 options 中得到任何东西，而在 status quo 下则是 4 年或更短）。[3](https://www.benkuhn.net/clawback/#fn:3) 这意味着，在 Scott 的 proposal 下，按 per unit time 计算，employees earning 的 equity 反而 _更少_！真要命。

你可以把 multiplier 从 50% 提高到比如 200%，以解决这个问题，但那会 seriously dilute 其余 shareholders。根本 problem 在于，Scott 的 option structure 通过让 options 变得 dramatically riskier 来 destroys value。即使 multiplier 达到 200%，与有着 10-year horizon 的 normal options 相比，我大概仍不会选择 Scott-options，而且我不认为通过微调 multipliers 就能挽救这个 structure。

## 4\. change jobs 的 ability 极其 valuable

Scott 的 proposal 实质上 prevents 人们在 company exits 之前 leaving company（因为在那之前他们不能 exercise 自己的 options，而 options 又会在他们 leaves 时 expire）。Scott 不知怎么可能没意识到，但对于一个 engineer 来说，预先承诺在同一家 company 工作直到 IPO，是 _极其_ costly 的，因为这会彻底毁掉他们的 negotiating leverage。

tech companies 主动给 employees 大幅 raises 的可能性，远低于给 new hires 支付比他们在上一份 job 中高得多的报酬的可能性。这意味着，employee 获得 raise 最快的办法，就是申请其他 jobs，拿到一个高 offer，再把它带回当前的 company，作为 [negotiating leverage](https://en.wikipedia.org/wiki/Best_alternative_to_a_negotiated_agreement)，以获得更大的 raise。

当然，如果 leaving company 会通过 equity clawbacks 让你 lose 数万乃至数十万美元，那这一切就完全行不通了。你永远不可能拿到一个足够好的 offer，使它优于留在当前的地方，即便当前的地方从不给你 raise。因此，如果你 undercompensated 或因其他原因感到不满，你就永远无法作出一个要 leaving 的 credible threat。

实质上，同意 Scott 的 terms，就意味着如果 company 发展顺利，你就在未来 6-10 年内把自己大部分 net worth 的 control 交给了 company management。我真希望大多数 founders 都足够值得信赖、心怀善意，能让这成为一个好主意。

这甚至还没有提到，除了 promotion 之外，我可能想离开一份 job 的所有其他原因。如果我发现自己讨厌 Bay Area，而我的 company 又不允许我 work remotely，怎么办？如果我患上严重的 carpal tunnel，再也不能 program 了，怎么办？如果一位 coworker 开始 harassing 我，怎么办？在这些情况下，我就只能完全听凭 founders 的意思，看他们是否愿意针对 anti-exercise rule 给我一个“special exception”。当然，如果我被 fired——这种事可一点也不少见！——那我就彻底没辙了。

## 5\. Equity clawbacks 让 zombie employees 变得更加 zombier

具有 short option expiry horizons 的 Late-stage startups 往往面临“zombie employees”的问题：这些人敷衍了事，因为他们不想再在那里工作，却无法在不导致自己的 options expiring 的情况下离职，也无法在不 incurring 大额 tax liability 的情况下 exercise 自己的 options。这些 employees 不仅自己几乎不 contribute，还会消耗其他所有人的 morale 和 productivity。

目前，可以通过付费让 third party 承担 tax liability，以此 mitigate 它——例如，你可以与 [ESO Fund](http://employeestockoptions.com/) 达成协议，不过听说他们的 pricing 会狠狠宰你一刀。这让 zombie employees 能够 quit，尽管要承受显著的 haircut。但 Scott 提出的 structure 完全 rules that out。它也 rules out 所有其他 anti-zombie measures，例如 early exercise 或为 employees 提供 pre-IPO liquidity，除非你从 founders 那里获得 special exception。这种 special exception 涉及大量 legal legwork，而他们没有 incentive 给你这种例外（除非出于好心），却有很多 disincentive。

事实上，如果 zombie employees 还没有 fully vested，那么允许他们离职实际上 costs less！例如，假设我在一个 4-year vesting schedule 的第 3 年之后感到厌倦，但我的 equity 是一家 $1b company 的 1%。如果持有 normal options，我可以 quit，带走 company 的 0.75%。如果持有 Scott 的 options，我原本会被 granted 1.5%（以弥补较不 favorable 的 terms），而在再熬过 5 年的 zombie-ing 之后，我会带着两倍的 equity 离开，所做的工作却差不多。这对我（五年前就愿意带着一半的数量离开）和 company（额外 lost 了 75 basis points 的 equity）而言，都是糟糕的 trade。

## 6\. 你不会想要 filter for 8-year employees

至少不会想用这个 scheme 的方式！

Scott 主张，他的 option scheme 会 selects for“best 和 most long-term-oriented employees”。尝试寻找愿意为你的 company 工作直到 IPO 之后的 employees，是个好主意。但 Scott 的想法所 selects for 的，是那些 _愿意事先作出这一承诺_ 的人，否则就会 losing 自己的 equity\*。\* 八年是极其漫长的时间，当你承诺在一个岗位上待那么久时，你就让自己承担了极大的 risk：事情可能不会像你期望的那样顺利。

任何人都不应该仅凭与 company 的几次 interviews，就必须事先作出如此重大的承诺——而你最好的 potential employees 会拒绝这样做。见鬼，许多 _婚姻_ 持续的时间都比一家 company 从成立到 IPO 所需的时间短，而人们会花上数年才作出那种承诺！指望一个 employee 仅仅经过一两周的 interviewing 就作出类似决定，是不合理的。

## 7\. Egalitarianism 是个转移注意力的幌子

Scott 主张，现行的 option compensation scheme 是 inegalitarian，因为 exercising options 需要 up-front cash investment。他竟然在支持这样一种 employee-unfriendly scheme 时使用这个 argument，这让我很恼火。在 Scott 的 option structure 下，employees 将失去在不 losing 自己大部分 wealth 的情况下 quit 的能力，让 management 和 investors 几乎完全 control 自己的 career。而且，如上所述，如果 option pool 不是 fixed size，那么 expired options 主要会为最大的 equity holders counteract dilution——又是 management 和 investors。这个 proposal 在 decrease employees 之间的 inequality 的同时，也会同等程度地 increase founders/investors 与 employees 之间的 inequality。

此外，既然 Scott 认为 management 可以作出 special exceptions，给某些 employees 提供 liquidity 或允许他们离职，那么这个 scheme 完全有可能根本不会更加 egalitarian。如果 founders 和 investors 不能极其公平地对待自己的 employees，它就只是把谁能负担 exercise bill 的不公平，替换为另一种不公平：谁能足够讨 founders 欢心，从而为自己的下一次 career move 获得 special dispensation，或 realize 自己的一部分 gains。

## 结论

只要 employees 离职就会将 options clawed back 的 option scheme，是我听说过的最 employee-unfriendly 的 compensation scheme。它主要通过三种方式做到这一点：

-   它将 wealth 从 employees transfers 给 founders 和 investors：一方面，在 employees 终究不可避免地离职时 clawing back equity；另一方面，在 employees 与 founders 就 equity 进行 negotiate 时，destroying 几乎所有 employee-side leverage。
-   它让 employees 的 wealth 暴露于任何会导致他们失去工作的 random shocks，从而凭空为他们 creates 巨量的 downside risk。
-   它大幅 increases job market friction，从而 destroys 巨量的 value。

你可以主张，通过 increasing 你的 equity grants 的 size，可以 compensate 上述一切。然而，由于这些不友好之处有很大一部分源于 value 被 _destroyed_——而不只是从 employees transferred 给 investors——因此需要对 grant size 作出不合理的大幅 increase 才能 compensate。

例如，我现在认为自己很可能会继续在 Wave 工作，直到它 fails 或发生 liquidity event。但即使 Wave 将我已经相当可观的 option grant 增至三倍，我也不愿接受这个 scheme，去承担这样的 risk：在未来 6 年的某个时候，如果意料之外的事情改变了我的想法（或迫使我采取行动），我就会 losing 自己的全部 equity。简而言之，这样的 clawback scheme 从一开始就行不通。

_感谢 Dan Luu 督促我发表这篇文章，并阅读了草稿！_

* * *

1.  好吧，现在已经不能说是最近了，但我把发表推迟了好几个月，因为我当时确信自己一定误解了这个 proposal 的某些地方。 [↩︎](https://www.benkuhn.net/clawback/#fnref:1)

2.  另外，在现实世界中，option strike price 会 changes，这意味着将被 re-granted 的新 option 会比已经 expired 的旧 option 更不 valuable（而且 differential 会以 cash 支付给 company，因此由所有 equity holders proportionally absorbed）。不过，与能够 resize option pool 的 effect 相比，这种 effect 相对较小。 [↩︎](https://www.benkuhn.net/clawback/#fnref:2)

3.  当然，在 Scott 的 rules 下，如果你在通往 IPO 的途中加入，你可能不必等满 8 年；但另一方面，如果有 normal vesting，你在远不到 4 年时就能得到一部分自己的 stock，因此这些 caveats 对两边都适用。 [↩︎](https://www.benkuhn.net/clawback/#fnref:3)

---

## 原文

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