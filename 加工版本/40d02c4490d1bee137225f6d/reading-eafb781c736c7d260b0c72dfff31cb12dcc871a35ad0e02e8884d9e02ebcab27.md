# The solution to Pascal's mugging

---

## 正文

Pascal's _Wager_ 是一种 scenario，其中存在获得 infinite reward（heaven）的可能，而这会 overrides 所有其他 consideration。

Pascal's _Mugging_ 是一种 scenario，其中 theological considerations 被替换成了更日常的情境：我们面对一个声称拥有 supernatural powers 的 being，如果我们预先付给它一小笔钱，它就会给我们丰厚的报偿。我想，我在网上能找到的这一特定 case 的最早 example，是 [Alex Tabarrok](https://mason.gmu.edu/%7Eatabarro/Pascal'sWager.pdf) 的这篇 2000 年 paper，其中提出了一个类似的 case。

[Bostrom (2009)](https://nickbostrom.com/papers/pascal.pdf) 在网上有一篇写得很好的介绍，可能是这一 scenario 的 canonical explainer。

### Pascal's mugging（帕斯卡勒索）

Pascal's mugging 的核心，是避开思考 voluntary belief 或 infinities 时的困难，转而向我们提出一个简单的 decision-theoretical question：

如果有人向你提出上述 deal，你应该 accept 吗？

起初，这个 problem 看起来确实很容易：设想别人承诺给你的 amount of money 是 £1000。那么，看起来确实有相当多的人能付给你这笔钱。随着 amount increases，那个人确实拥有这笔钱就变得越来越 unlikely。最终，如果承诺的 amount 比 Jeff Bezos 的 wealth 还要 larger，那么就必须 account for 更复杂的花招，其中涉及 multi-millionaires、governments 和 central banks 之间的 conspiracies；对于这些情形，我们完全可以 assign 更加 minute 的 probabilities。

我们也可以考虑正在讨论的这类 event 的 base rate：人们有多经常收到这种 deal？又有多经常在收到这种 deal 后，deal 确实成功兑现？因为，如果我们确实生活在这样一个世界里：有人提出这种 deal，而且其中有 nontrivial fraction 的人是诚实的，那么情况就不同了，此时 accept 可能是 rational 的；而在这种 situation 中，我们的 intuition 与 calculation 是 aligned 的。

在我看来，这一 reasoning 能够处理大多数 plausible Pascal muggings：low base rate+拥有 higher amounts of money 的 lower probability。

接下来，我们可以想想，遇到 Eliezer Yudkowsky 在一篇 [Less Wrong](https://www.lesswrong.com/posts/a5JAiTdytou3Jg749/pascal-s-mugging-tiny-probabilities-of-vast-utilities) 帖子中提出的那种 situation 时，会发生什么：

> 现在，suppose 有人来对我说：“给我五美元，否则我就会使用来自 Matrix 之外的 magic powers，来 run 一个 Turing machine，它会 simulates 并 kills 3^^^^3 个人。”

或者，Bostrom 的 paper 中写道：

> 看看我苍白的面容、漆黑的双眼；还要注意，我从头到脚都穿着黑色。这些都是 Operator of the Seventh Dimension 的一些明显征兆。我就来自那里，而 magic work 也在那里完成。$$...$$ 如果你把钱包交给我，我就会 perform magic，让你额外获得 1,000 quadrillion 个 happy days of life

在第一个 case 中，我们可以使用一个 metaphysical argument：并不存在 simulating consciousness 这种事，正如 water 并不是 simulated water 一样。但这并不能让我们走多远，因为我们可以把 simulating 那些人替换为普通人，以及要 murder 他们的威胁。

我们可以 simplify 这些 cases，以提炼出其中的 issue。设想正在讨论的 cases 只涉及一个人，而不是 3^^^^3 个人，或者只涉及额外 1 年的 happy life。这似乎不会太多地 alter 我们对这个 case 的 evaluation。

与下面两个 cases 作 Compare：

-   如果预先 pays £5，就有 10%（guaranteed）的 chance 能 earning £100k
-   如果预先 pays £5，就有 10%（guaranteed）的 chance 能 earning £10

这里，第二个 gamble 不值得参与。probability 是相同的，我们 changing 的是 rewards，而我们的 actions 也随之 changing；但在上面的 case 中，如果我们 change 的是 probabilities，我们似乎却不会这样做。

因此，这些 examples 中的 issue，不在于涉及的 quantities 是 high，而在于这些 examples 的 metaphysical dubiousness。真正 drives 这些 examples 的并不是 high reward，而是某人来自 Matrix 之外、或是 Seventh Dimension 的 Operator 这件事的 low likelihood。

所以，一种出路是直接作出规定：在我们关于世界的 mental model 中，我们生活在 real world 中，并不存在 Seventh dimension；这些事情为 true 的 probability，与 2+2 equalling 4 以外任何数的 probability 完全相同，也就是 zero。

这需要更多 justification：像 2+2 这样的 a priori proposition 是 necessarily true 的，无论我们对世界知道些什么；但关于 empirical world 的 knowledge 是 probabilistic 的。我们能否以 complete certainty 说，不存在 Seventh Dimension，也不存在某种 laws of physics 的 loophole，使 extra-dimensional beings 能够 interact？恐怕不能。

但如果我们 accept 这种 possibility，就又回到了原来的 situation。[Gwern (2011)](https://www.gwern.net/mugging) 讨论了这种 approach 的 problems：

> 尝试摆脱 mugging 的一种方法，是单方面宣布：所有 below 某个 small probability 的 probabilities，都将被 treated as zero。只要 lower limit 和 mugger’s credibility 的搭配恰当，mugging 就不会发生。
> 
> 但这种 _ad hoc_ method 会 violates probability theory 的常见 axioms，因此我们可以预料到会有后果。事实证明，即使不靠 mugging 本身，也很容易把这样的人变成 money pump。
> 
> Suppose 你的朋友采取了这个 position，并且明确表示，任何 less than or equal to 1/20 的 probabilities 都是 0。于是你提议玩一个 game；你们两个将 roll 一枚 d20 die，如果 die 的结果为 1-19，你就付给他一便士；如果 die 的结果为 20，他就付给你一美元——不，是 _bazillion_ 美元。
> 
> 于是，你的朋友 calculates：他有 19/20 的 chance 会 win 一点钱，也有 1/20 的 chance 会 lose 很多钱——但等等，1/20 太 small 了，不值得在意！按照他的 rule，这就是 zero chance。所以，他在这个 game 中不可能 lose money，只可能 make money。
> 
> 他当然错了，在第 5th 次 roll 时，你就带着他拥有的一切离开了。（而且，你想这样做多少次都可以。）
> 
> 仅仅 shrinking probability 也无法挽救它；能够击败 1/20 这个 limit 的同一个 game，很容易就能被 generalized 到 1/n，而且事实上，我们还能让这个 game 更糟：不再把 1 到 n-1 设为 winning，而是把 0 和 1 define 为 winning，把每一次 increment 都设为 losing。随着 n increases，instant losses 的 large percentage 也会 increases。
> 
> 所以，显然这种 modification 行不通。

超出原来的 problem 去思考，或许才是 solve 它的关键。我们可以不去 solving Pascal's mugging，而是 solve tiny probabilities 的 problem，看看这会对原来的 problem 产生什么影响。

### 小概率（Small probabilities）

想想关闭你现在正在阅读的文档这个行为。可以设想，如果你这样做，Dimension N 中的一个 being 就会杀死你，因为不让 Nintil 的页面始终保持打开是一种亵渎。同样，在 dimension L 中，认为 Nintil 不是世界上最好的博客是一种令人发指的行为，即使对我们这些 regular universe 的居民，也会处以死刑。这种情况的 probability 是多少？此外，你应该关闭 Nintil 吗？你应该胆敢认为 Nintil 不是有史以来最好的博客吗？

如果我们已经说过，不能像对待 2+2=5 那样，用 apriorism 的重锤来 rule them out，那么仍有一条路可走，那就是 _mu_ answer：认为这个 probability 出于另一种原因而是 undefined。

Probabilistic reasoning 将 possible events 的 space 进行 portions out，其中每个 slice 都有某个 probability。例如，在 rolling 一个 6-face dice 时，可以将 outcome 进行 model，使其具有 6 个 possible states，其中每个 state 都具有相同的 probability。我们通常不在意这个 dice 实际上是否有 7 个 faces，或其他某种 unlikely possibility。对于你关闭 Nintil 后发生的 events，那个 set 中可能有什么？其中一个是“什么也没有，具有 high likelihood，也许还有“某件出乎意料但无关紧要的事”。在这个 model 下，没有容纳 Dimension N 中发生的古怪 events 的余地；event“你被一个来自 Dimension N 的 being 杀死”的 probability 要么是 undefined，要么是 zero，取决于你决定如何处理它。

这个回答在我听来确实令人满意：它描述了我们在世上做着当时正在做的事情时，究竟发生了什么：我们对什么是 possible 有一个 mental model，在其中 assign probabilities，并想清楚该做什么；我们并不是每时每刻都在思考所有可能发生的事情。

现在，我能想象有人会认为，对 possible 的事物拥有一个 mental model，这整套做法听起来并不怎么 computable，因而很可疑；而且，这种 method 可能听起来含糊又 hacky，也许对于人类这样 imperfect limited reasoner 来说还可以，但并不是 perfect reasoner 会做的事。

关于 perfect reasoners，我要提出一个 claim：考虑每一种 possibility 根本不 possible：_必须 use 一个 model，并 leave things out_。大自然厌恶 infinities，而 constructing 一个能够同时（或 sequentially，在 finite time 内）考虑 infinite non-parameterized[1](https://nintil.com/pascals-mugging/#param) hypothesis 的 system，在 principle 上是 impossible 的。

当然，可以承认这一点，但在 mugger case 中，我们仍然只是在考虑 set 中的两个 items：{Mugger 来自 7th dimension，而且拥有惊人的 powers，Mugger 是假消息}。这里没有 infinities。

上面的意思是，你可以在自己的 model 中放入任何你想要的东西，包括一个 single element，在这种情况下，它的 probability 就是 1。由于 event“Mugger 来自 7th dimension”甚至不在 model 中，所以在 choosing an action 时它就会被 ignored。问题解决了。

### 选择模型（Choosing models）

这产生了一个问题：它似乎让我们陷入了 epistemic 版本的 bipolar disorder：我们相信 mugger 来自另一个 dimension 的 probability 是 nonzero，同时却又在行动上_仿佛它是_ zero。

我们在其他 domains 中已经这样做了，至少我是如此：正如上文提到的，一个 deductively valid logic proposition 为 true 的 probability 是 one，a priori knowledge 也是如此（例如 2+2=4）。然而，我也知道自己可能会错；有人可能问我 232433443 是否是 prime number。它是或不是的 probability 都是 1，但我不知道；在这种情况下，我最终既有一个要么是 0 要么是 1 的 probability，也有另一个在 0 和 1 之间的 probability。

我不打算在这里解决这些问题，也不知道是否已经有人解决了它们[2](https://nintil.com/pascals-mugging/#logical)。我只想笼统地指出，人必须选择将什么放进 model，而这归根结底依赖 heuristics，同时暂不排除可以想出一种 mechanical way 来做这件事。

### 还有其他办法吗？

#### 将低于 threshold 的 probabilities 进行 Round 至 zero

由于 Gwern 概述的那些原因，这行不通

#### 对 prior probability 进行 Penalize（最初的 Hansonian solution）

这很有吸引力，而且与常规的 bayesian framework 配合得很好。但归根结底，很难看出该如何 construct 一个 prior，使其 avoids mugging situation。我在上文讨论过，有充分的理由相信，money 实际被 payed 的 probability 会随 amount 而 decrease（能 pay 的人更少），但一旦到了 7th dimensional mugger 这个层次，就不再如此了。Conditional on mugger 拥有 superpowers，它们具有 paying 的 capability 的 probability 似乎不会随 amount 而 change，至少变化不大。

#### 对进入 calculation 的 maximum utility 进行 Bound

即使接受它——它让人感觉是 ad hoc 的——也不能让问题消失；如果将 reward 进行 splits，分成 smaller rewards，或采用类似的把戏，mugging 就会再次出现。

#### 凯利准则（Kelly criterion）

正如 Gwern 在[这里](https://twitter.com/gwern/status/1164632749157900289)建议的，不要 maximizing the mean，而要 maximize the median；在这些古怪的 low probability cases 中，这只会建议照常继续。这会在其他地方造成问题吗？这确实似乎是这个问题的一个不错而 elegant 的 solution。

### 如果你仍不同意

人类迄今构想出的 largest number 出现在 Michael Huemer 的 _Approaching Infinity_ 中，是 $✳^{8✳}8$，让 3↑↑↑3 显得像个小婴儿。（当然，现在你可以想到一个 larger number。）此时此地，我向你提出如下条件：如果你给我 \$1, then there is some chance that in a parallel universe $✳^{8✳}8$ 个 lives worth living 就会被 created。也有一个类似的 chance，同样 number 的 lives 会在别处被 tortured。明智地选择吧！同意我的观点，或者付钱！（或者解释我哪里错了[3](https://nintil.com/pascals-mugging/#wrong)，如果我觉得你说得有说服力，我就付钱给你！）

请注意，在文章开头，我说过自己对这里 reasoning 的 confidence 并没有那么高，所以如果我错了，也不会那么惊讶。

### 更新记录（Changelog）

-   2019/08/22 应 Gwern 的建议，添加了 Kelly criterion。

1

因为当然，读到这里，有人可能会认为，一个 solves 这个 system of equations "x=x-2" 的 system 正在 considering 一个 infinite set 的 hypothesis（x 的 values）。即使按这种 interpretation，我们所处理的也只是一个 single structure，仅仅是在一个 well defined continuum 上 changing 这些 parameters。这对 predicting 某人接下来会说什么之类的事情并不奏效，因为 possible utterances 的 number 是 unbounded。

2

[Logical induction](https://intelligence.org/2016/09/12/new-paper-logical-induction/) 并不能 solve 这个 problem，它只是旨在为得出第二种 probability 提供一个更好的 tool，而没有 resolving 其 underlying issue。

3

如果我错了，那么 correct answer 很可能会涉及这样一个事实：这里正在进行一个 recursive game，其中 mugger 知道我们会给他是 legit 这件事赋予一个 low probability，并会相应地 adjust，但我们又知道他知道，如此等等。

---

## 原文

Pascal's _Wager_ is an scenario where there is a potential for an infinite reward (heaven) and that overrides every other consideration.

Pascal's _Mugging_ is an scenario where the theological considerations are replaced with a more mundane setting, where we are faced with a being who claims to have supernatural powers, that will pay us greatly if we pay them a small sum in advance. I think the earliest example I could find online of this particular case was this 2000 paper from [Alex Tabarrok](https://mason.gmu.edu/%7Eatabarro/Pascal'sWager.pdf), who presents a similar case.

[Bostrom (2009)](https://nickbostrom.com/papers/pascal.pdf) has a nice writeup online, and is probably the canonical explainer for the scenario.

### Pascal's mugging

Pascal's mugging, at its core does away with the difficulties of thinking about voluntary belief or infinities and instead presents us with a simple decision-theoretical question:

If someone offers you the deal described, should you accept?

At first, the problem does seem easy: Imagine the amount of money you are offered is £1000. Then, it does seem like quite a lot of people could pay you. As the amount increases, it becomes more and more unlikely that the person in question actually has the money. Ultimately if the amount offered is larger than Jeff Bezos' wealth, one then has to account for even more complicated shenanigans involving conspiracies between multi-millionaires, governments, and central banks, to which one can safely assign even more minute probabilities.

One can also consider the base rate for the kind of event we are considering: How often are people offered the deal? How often are people offered the deal and then the deal happens successfully? For it would be different if indeed we lived in a world where there are people offering such deal, and a nontrivial fraction of them are honest, then it could be rational to accept; and in this situation our intuition is aligned with our calculation.

This reasoning, it seems to me, deals with most plausible Pascal muggings: low base rate+lower probability of having higher amounts of money.

Then, we can think about what happens with situations like the one presented by Eliezer Yudkowsky in a [Less Wrong](https://www.lesswrong.com/posts/a5JAiTdytou3Jg749/pascal-s-mugging-tiny-probabilities-of-vast-utilities) post:

> Now suppose someone comes to me and says, "Give me five dollars, or I'll use my magic powers from outside the Matrix to run a Turing machine that simulates and kills 3^^^^3 people."

Or in Bostrom's paper:

> Look at my pale countenance, my dark eyes; and note that I’m dressed in black from top to toe. These are some of the telltale signs of an Operator of the Seventh Dimension. That’s where I come from and that’s where the magic work gets done. \[...\] If you hand me your wallet, I will perform magic that will give you an extra 1,000 quadrillion happy days of life

In the first case one could deploy a metaphysical argument: There is no such thing as simulating consciousness, no more than water is simulated water. But this doesn't get us very far, because one can substitute simulating those people with just regular people and a thread to murder them.

One can simplify those cases to distill what's the issue there. Imagine that the cases being discussed merely involve one person instead of 3^^^^3, or 1 extra year of happy life. This doesn't seem to alter much our evaluation of the case.

Compare with this two cases:

-   A 10% (guaranteed) chance of earning £100k, if one pays £5 in advance
-   A 10% (guaranteed) chance of earning £10, if one pays £5 in advance

Here the second gamble is not worth it. The probability is the same, what we are changing are the rewards, and we are changing our actions, but we are seemingly not doing so if what we change are the probabilities in the case above.

Thus, the issue in those examples is not that the quantities involved are high, but the metaphysical dubiousness of the examples. What really drives those examples is not a high reward, but the low likelihood of someone being from outside the Matrix, or of being an Operator from the Seventh Dimension.

So, one way out is to declare, by fiat, that in our mental model of the world we live in the real world, and that there is no Seventh dimension, that the probability of those things being true is exactly that of 2+2 equalling anything other than 4, that is, zero.

This would require more justification: An a priori proposition like 2+2 is necessarily true, no matter what we know about the world, but knowledge about the empirical world is probabilistic. Can we say, with complete certainty, that there is no Seventh Dimension, that perhaps there is a loophole to the laws of physics that allows extra-dimensional beings to interact? Probably not.

But if we accept that possibility, we get back the situation again. [Gwern (2011)](https://www.gwern.net/mugging) discussed the problems with this approach:

> One way to try to escape a mugging is to unilaterally declare that all probabilities below a certain small probability will be treated as zero. With the right pair of lower limit and mugger’s credibility, the mugging will not take place.
> 
> But such a _ad hoc_ method violates common axioms of probability theory, and thus we can expect there to be repercussions. It turns out to be easy to turn such a person into a money pump, if not by the mugging itself.
> 
> Suppose your friend adopts this position, and he says specifically that any probabilities less than or equal to 1/20 are 0. You then suggest a game; the two of you will roll a d20 die, and if the die turns up 1-19, you will pay him one penny and if the die turns up 20, he pays you one dollar - no, one _bazillion_ dollars.
> 
> Your friend then calculates: there is a 19/20 chance that he will win a little money, and there is a 1/20 chance he will lose a lot of money - but wait, 1/20 is too small to matter! It is zero chance, by his rule. So, there is no way he can lose money on this game and he can only make money.
> 
> He is of course wrong, and on the 5th roll you walk away with everything he owns. (And you can do this as many times as you like.)
> 
> Nor can it be rescued simply by shrinking the probability; the same game that defeated a limit of 1/20 can easily be generalized to 1/n, and in fact, we can make the game even worse: instead of 1 to n-1 winning, we define 0 and 1 as winning, and each increment as losing. As n increases, the large percentage of instant losses increases.
> 
> So clearly this modification doesn’t work.

Thinking beyond the original problem is probably the key to solve it. Instead of solving Pascal's mugging, we can solve the problem of tiny probabilities, and see what that does to the original problem.

### Small probabilities

Consider the act of closing the document you are now reading. It is conceivable that a being in Dimension N will kill you if you do so, for not having a permanently open view of Nintil is sacrilegious. What's the probability of that? Similarly, in dimension L, thinking that Nintil is not the best blog in the world is a hideous act, and is punished by death, even for us inhabitants of the regular universe. What's the probability of that? Furthermore, should you close Nintil? Should you dare think Nintil is not the best blog ever?

If we have said that we can't rule them out with the hammer of apriorism, as we would do with 2+2=5, there is still one avenue, which is the _mu_ answer: To say that the probability is undefined for a different reason.

Probabilistic reasoning portions out the space of possible events, where each slice has some probability. For example, when rolling a 6-face dice one can model the outcome as having 6 possible states, where each state has the same probability. We don't usually care about whether or not the dice actually has 7 faces, or some other unlikely possibility. For the events that happen after you close Nintil, what could there be in that set? "Nothing is one of them, with high likelihood, and perhaps "Something unexpected but inconsequential". Under that model, there is no room for weird events happening in Dimension N, the probability of the event "You get killed by a being from Dimension N" is either undefined, or zero, depending on how you decide to treat it.

This answer does sound satisfying to me: it describes what is going on when we go about in the world doing whatever is is that we are doing at that moment: We have a mental model of what is possible, within that we assign probabilities, and work out what to do; we are not thinking about everything that could possibly happen, all the time.

Now, I can imagine someone thinking that this whole business of having a mental model of the possible doesn't sound very computable, and in turn that's fishy; also the method may sound wishy washy and hacky, perhaps okay for an imperfect limited reasoner like a human being, but not what a perfect reasoner would do.

On perfect reasoners, I will make the claim that it is just not possible to consider every possibility: _one must use a model and leave things out_. Nature abhors infinities, and constructing a system that can consider infinite non-parameterized[1](https://nintil.com/pascals-mugging/#param) hypothesis at once (or sequentially, in finite time) is impossible in principle.

Sure, one can concede, but then still in the mugger case we are are only considering two items in the set: {Mugger is from the 7th dimension and has awesome powers, Mugger is fake news}. There are no infinities here.

The point above is that you can have whatever you want in your model, including a single element, in which case that'll have a probability of 1. As the event "Mugger is from the 7th dimension" is not even in the model, it gets ignored when choosing an action. Problem solved.

### Choosing models

This creates a problem: That it seems to condemn us the epistemic version of bipolar disorder: We are believing that the probability of the mugger being from another dimension is nonzero, and at the same time we are acting _as if it were_ zero.

We already do this in other domains, or at least I do: As I mentioned above, the probability that a deductively valid logic proposition is true is one, likewise for a priori knowledge (e.g. 2+2=4). However, I also know that I can be wrong; I can be asked if 232433443 is a prime number. It is or it is not with probability 1, but I don't know; in that case I end up with a probability that is either 0 or 1, and also another one that is between 0 and 1.

I don't intend to solve these issues here, and I don't know if anyone has solved them yet[2](https://nintil.com/pascals-mugging/#logical). I will just handwave towards the fact that one has to choose what to put in the model, and that it boils down to heuristics, without rejecting for now that one can come up with a mechanical way of doing so.

### Is there any other way?

#### Round probabilities below a threshold to zero

Doesn't work for the reasons outlines by Gwern

#### Penalize the prior probability (The original Hansonian solution)

This is alluring, and it plays nicely with a regular bayesian framework. but ultimately it's hard to see how to construct a prior in such a way that it avoids the mugging situation. I discussed above that there are good reasons to believe that the probability of the money actually being payed decrease with the amount (less people can pay it), but once we reach the level of the 7th dimensional mugger, it does not. Conditional on the mugger having superpowers, the probability that they have the capability of paying does not seem to change with the amount, at least not by much.

#### Bound the maximum utility that goes into the calculation

Even accepting it -it feels ad hoc-, it doesn't make the problem go away; the mugging reoccurs if one splits the reward in smaller rewards, or a similar trick.

#### The Kelly criterion

As suggested by Gwern [here](https://twitter.com/gwern/status/1164632749157900289), rather than maximizing the mean, maximize the median; which in these odd low probability cases just recommend to keep going as usual. Does this cause problems elsewhere? It does seem a nice elegant solution to the problem.

### If you still disagree

The largest number ever devised by mankind appears in Michael Huemer's _Approaching Infinity_, is $✳^{8✳}8$, leaving 3↑↑↑3 as a little baby. (Sure, now you can think of a larger number). Here and now I offer you the following: If you give me $1, then there is some chance that in a parallel universe $✳^{8✳}8$ lives worth living will be created. There is also a similar chance that that same number of lives will be tortured elsewhere. Choose wisely! Agree with me or pay! (Or explain where I went wrong[3](https://nintil.com/pascals-mugging/#wrong) and if I find you convincing, I'll pay you!)

Note that at the beginning of the post, I state that my confidence in the reasoning here is not that high, so I won't be that surprised if I'm wrong

### Changelog

-   2019/08/22 Added Kelly criterion at Gwern's suggestion.

1

Because of course, upon reading that someone might think that a system that solves the system of equations "x=x-2" is considering an infinite set of hypothesis (values of x). Even under that interpretation, it is a single structure we are working with, merely changing the parameters across a well defined continuum. That doesn't work with, say, predicting what someone will say next, the number of possible utterances is unbounded.

2

[Logical induction](https://intelligence.org/2016/09/12/new-paper-logical-induction/) does not solve this problem, it just aims to provide a better tool of coming up the the second kind of probability, without resolving the underlying issue.

3

If I'm wrong, the correct answer will probably involve the fact that there is a recursive game going on here, whereby the mugger knows that we'll give a low probability to him being legit and will adjust accordingly, but then we know that he knows, and so on.