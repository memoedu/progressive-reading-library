_I originally posted this as a [comment on another blog](https://srconstantin.wordpress.com/2014/10/16/uncertainty-and-confidence/comment-page-1/#comment-115), but it recently came up again. I’m posting it here in edited form._

A common [Less Wrong](http://lesswrong.com/) meme is that you should try to put [subjective probabilities](http://predictionbook.com/) on any events that you care about, and that it’s important to [calibrate your sense of probability](http://rationality.org/calibration/) so that you can avoid being systematically over- or underconfident. For instance, a popular side activity at CFAR workshops is [prediction markets](http://rationality.org/2014/06/14/in-the-game-of-markets-you-win-or-you-lie/) that reward being able to correctly state probabilistic beliefs. Some people I’ve run into at meetups will go as far as making fun of you for not being willing to put a number on things. If you claim [Knightian uncertainty](https://en.wikipedia.org/wiki/Knightian_uncertainty)—that you’re too uncertain even to quantify your uncertainty—you probably won’t be taken seriously.

I think that claims of Knightian uncertainty are not only reasonable, but can be _the optimal move_, once you move from an abstract setting of “probabilities inside your head” to the more concrete environment of “probabilities that you would like to base actions on.” The rest of this post is an explanation of that position.

* * *

An interesting thing I’ve picked up from reading some literature on automated [market making](https://en.wikipedia.org/wiki/Market_maker) is the idea that agents can have a distinction between “internal probabilities”—the kind that you are forced to have by things like [Cox’s theorem](https://en.wikipedia.org/wiki/Cox%27s_theorem)[1](https://www.benkuhn.net/prob/#fn:1)—and “actionable probabilities” that you would be (for instance) willing to bet on.

In theory, if you’re a rational agent making a prediction market[3](https://www.benkuhn.net/prob/#fn:3) that, say, aliens exist, then you should be willing to buy that contract at probability `$p - \epsilon$` (giving you an expected profit of `$\epsilon$`) or sell it at probability `$p + \epsilon$` (for the same). In other words, unless the market agrees with you _very precisely_, you should rationally be willing to trade on it. But even if you’re sure that your probabilities are well-calibrated, you’re probably not about to write down a bunch of your subjective probabilities and start market-making on a prediction market. Why?

One possible reason is [adverse selection](https://en.wikipedia.org/wiki/Adverse_selection).[4](https://www.benkuhn.net/prob/#fn:4) If you’re making a market on whether aliens exist and you offer to buy at `$p - 0.0001$` and sell at `$p + 0.0001$`, but the market knows better than you do, then—no matter whether aliens exist or not—a lot more people will take you up on the correct side of the trade, and you’ll end up losing. So you have to make your `$\epsilon$` large enough that it compensates you for “sticking your neck out” by offering to both buy and sell the contract.

I suspect that this is part of what people find compelling about the idea of Knightian uncertainty: it’s a heuristic to avoid being adversely selected. Asking someone to name their credence in something is like asking them to buy and sell a prediction market contract at the same price: they’re sticking their neck out and getting nothing in return. If someone asks you to name your credence in the existence of aliens, it’s generally because they have thought more about it than you have, and so it’s unfavorable for you (on expectation) to commit yourself to _any_ number, even one that correctly represents your current epistemic state.

Another reason to claim Knightian uncertainty is bounded computation. Sometimes when I say “I have no opinion on X” I really mean “I think my opinion on X would be likely to change if I thought more about it.” In fact, in this case naming a number could be epistemically _dangerous_ for me—it would force me to commit to a position would then be subject to [status quo bias](https://en.wikipedia.org/wiki/Status_quo_bias).

I’ve had several quite frustrating conversations with people from the Less-Wrong-osphere where I tried and failed to explain that their questions about my internal probabilities weren’t going to be informative for these reasons. Not everyone thinks this way, certainly,[5](https://www.benkuhn.net/prob/#fn:5) but it’s an annoyingly pervasive attitude. In general, when someone claims Knightian uncertainty, I think it’s much more productive to treat that claim as reasonable, and figure out _why_ they’re claiming it—than to try to back them into a corner and try to wrench a probability out of them somehow. But unfortunately I see the latter response all too often.

* * *

1.  Actually, you’re only even forced to have consistent internal probabilities if you’re not computationally limited. Cox’s theorem doesn’t say anything about computational limitations that might be an obstacle to deriving subjective probabilities in practice. For instance, as far as I know, the question
    
    > How much should a computationally bounded rational agent bet that a given large instance of [3-SAT](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem#3-satisfiability) is solvable?
    
    is still open; Cox’s Theorem would tell you “bet nothing or everything, depending on whether the instance is solvable,” which isn’t very helpful. [↩︎](https://www.benkuhn.net/prob/#fnref:1)
    
2.  Actually, you’d pick some easier-to-define event, like paying out $1 if aliens are discovered by 2100, so that it has a definite end date. [↩︎](https://www.benkuhn.net/prob/#fnref:2)
    
3.  In a prediction market, you trade contracts where the seller pays the buyer, e.g., one dollar if aliens turn out to exist and nothing otherwise.[2](https://www.benkuhn.net/prob/#fn:2) Then if you’re risk-neutral and your subjective probability that aliens exist is _p_, you should be willing to buy a contract for any price less than _p_ dollars and sell for any price more than _p_ dollars. [↩︎](https://www.benkuhn.net/prob/#fnref:3)
    
4.  It might also be because of the incredibly sketchy nature and uncertain regulatory favor of most prediction markets, but let’s pretend we live in a hypothetical world where people recognize the value of prediction markets. You probably _still_ wouldn’t be jumpting to sign up. [↩︎](https://www.benkuhn.net/prob/#fnref:4)
    
5.  For instance, Eliezer’s [When (Not) To Use Probabilities](http://lesswrong.com/lw/sg/when_not_to_use_probabilities/):
    
    > To be specific, I would advise, in most cases, against using non-numerical procedures to create what appear to be numerical probabilities. Numbers should come from numbers.
    
     [↩︎](https://www.benkuhn.net/prob/#fnref:5)