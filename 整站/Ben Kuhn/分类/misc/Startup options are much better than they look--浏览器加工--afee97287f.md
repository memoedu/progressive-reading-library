People often make several complaints about how employment works at startups:

1.  They don’t pay well. They claim to make up the difference with stock options, but the options are worthless and you can earn more by working for a big stable company.
    
2.  They prey on starry-eyed, young, naive people by selling them on unrealistic visions of the company’s success.
    
3.  Job turnover is incredibly high (1-2 years), because companies are too greedy to give employees raises (even when the employee is worth it), forcing them to change jobs if they want to earn more.
    

What if, instead of easily-duped employees and stupid evil companies, everyone in this story was acting in a basically rational and decent way?

I don’t claim that’s _always_ the case; there are certainly some evil companies and naive employees. But I do think that that you don’t _need_ to invoke to greed and stupidity to explain the low-looking offers and super-high job turnover. There are rational reasons you might have both of those, and they have to do with how you value stock options.

## The normal way of valuing options

Here’s the simplest way of trying to figure out whether an option package is a good deal (if you’re risk neutral!):

1.  Estimate the valuation of the company. (For our purposes it doesn’t matter how you do this; people often start by taking the post-money valuation of their last round of venture funding, and discounting it to reflect the fact that option holders get a worse class of stock.)
    
2.  Multiply by the fraction of the company you have options on.
    
3.  Subtract the _opportunity cost_, that is, the lost wages from working at a startup instead of a big stable company.[1](https://www.benkuhn.net/optopt/#fn:1)
    

For instance, if the company is worth $1m, and you are granted options on 1% of the company, then this basic model says that the expected value of the options is 1m * 0.01 = 10k. But if Google offers you $110k/year and the startup offers you $100k/year, then the opportunity cost is $10k/year. Since you’d have to work at the company for four years for all your options to vest, the total opportunity cost is $40k—way more than the face value of the options. That makes it look like a pretty bad deal.

## But wait!

This model neglects an extremely important factor, which is that _you are allowed to quit your job._

Why is this important? Startups are very volatile, so after you accept the job, the company’s expected valuation is likely to go up a lot (if things go well) or down a lot (if things go poorly).

-   If it goes down a lot, you can quit, which lowers the opportunity cost a lot. This will stop your options from vesting, but since they’re unlikely to be worth much if the company’s valuation has fallen, that’s not so bad.
    
-   If it goes up a lot, then your options are a lot more valuable than they looked like beforehand, so the upside is much higher.
    

Either way, you’re much better off than in the model above, where we didn’t take into account that you can quit early.[2](https://www.benkuhn.net/optopt/#fn:2)

How much of a difference does this make? Depending on your assumptions, it increases the option value by between 30% and 100%. More importantly, it means that the startup offer is _better_ than the bigco offer in many cases where you wouldn’t naively think so.

You can actually compute this by writing down a model for how to price the options, and calculating their expected value. I did this with the following assumptions:

-   You receive a job offer from a startup currently valued at $10m, with a strike price of $2.5m. They offer you options on 1% of the company, vesting monthly over 4 years with a 1-year cliff. You sacrifice $50,000/year by accepting the offer instead of working at a larger company.
    
-   The startup has an expected annual growth rate of 8% and annualized volatility of 100%.[3](https://www.benkuhn.net/optopt/#fn:3)
    
-   You have an annualized discount rate of 2%.
    
-   7 years after you join, the company exits, so you exercise the options (or don’t if the valuation is lower than the strike price).
    

Computing the expected value of this offer sounds hard, but actually, it turns out you can use a [lattice model](https://en.wikipedia.org/wiki/Lattice_model_%28finance%29) (a.k.a. a [dynamic programming algorithm](https://en.wikipedia.org/wiki/Dynamic_programming)) to evaluate the price fairly efficiently. I wrote [a model in Python](https://github.com/benkuhn/option-val) and compared this to a naive model where you always decide to work for the entire 4-year vesting period (this is just a normal option trade, so basically a [Black-Scholes model](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_model)).

In the naive model, your net gain from accepting the startup job offer instead of the big company’s is -$50,000—that is, you lose $50,000 by taking the startup offer. (Since the total opportunity cost is $200,000, that means the expected value of the option grant, at the time it’s granted, is $160,000.)

But once you factor in the ability to quit if the startup does badly, the net gain increases to _positive_ $35,000—an increase of $85,000 or over 50% of the face value of the options. That’s a big difference!

## Optimal quitting time

Of course, if you want to actually achieve the maximum expected value of your options, you need to know when the optimal time to quit is. Here’s a plot of the likelihood of various valuations, and whether you should quit:

[![](https://www.benkuhn.net/optopt/trajectory_hu988a157925965ba7693423fe52e82964_88659_1280x960_resize_box_3.png)](https://www.benkuhn.net/optopt/trajectory.png "full size")

Note that the dense part of the graph gets _lower_ over time! Even though the expected value of the company increases, that’s mostly driven by the few top companies; the typical company’s valuation goes down over time.

As you get close to the 1-year mark, you become more and more likely to want to stay at the company because of the vesting cliff. If you’re at month 10, staying for 2 more months will give you 12 months’ worth of options—for that trade not to be worth it, the company’s prospects have to be pretty dim. After the cliff, the min-valuation-to-stay also has a “toothy” pattern, which is because your stock vests in monthly increments, which gives you a bunch of “mini-cliff” effects on month boundaries.

After your cliff, the valuation required for you to stay rises, but pretty slowly. In reality, you might want to be more aggressive with quitting than this. A company whose valuation has stagnated for 3 years seems bad enough to quit. If the model incorporated something like, say, variance going down over time, or fatter tails, then it might reproduce that effect.

At 4 years, the model tells you always to quit, because your vesting is over, which means there’s no point in sacrificing the higher earnings from the bigco job. Obviously in real life startups do things like refresher option grants etc. that make it more worthwhile to stay.

Notice also that, even though the model thinks you should take the startup job, your expected tenure there is pretty short—the median rational time to quit is 12 months and you’ll almost certainly have quit by the end of your vesting period.

## Sensitivity

We can learn even more about this model by testing the sensitivity of its conclusions as we vary the parameters.

The most interesting parameter to vary is the opportunity cost:

[![](https://www.benkuhn.net/optopt/sensitivity_opp_cost_hu7ef8d177d8fda3c2607188e71de447de_75575_1280x640_resize_box_3.png)](https://www.benkuhn.net/optopt/sensitivity_opp_cost.png "full size")

When the opportunity cost of the startup is low—that is, when the big company’s cash offer isn’t much better—the net value is high under both models. That makes sense: the lower the big company’s offer is, the less valuable it is to quit the startup, even if things aren’t going well, so it doesn’t really matter whether you consider quitting or not.

When the opportunity cost of the startup increases, then the naive no-quitting model rapidly flips to the startup offer being net loss (at around $40,000/year). Meanwhile, the optimal-quitting model still thinks the offer is a very good idea at that point. In fact, the optimal-quitting model says it’s worth at least trying out working at the startup if the opportunity cost is less than $100k a year. Of course, the higher the opportunity cost gets, the more likely it is that you’ll quit very quickly—but the upside is still high enough to make it worthwhile to take a shot. (After $100k, the offer value goes to 0, which means the model thinks you should quit at month 0, i.e. not accept the offer in the first place.)

Increasing the opportunity cost by a factor of 2 is the same as _decreasing_ the option package value by a factor of 2 (by halving either the ownership percentage, or the company’s valuation). So the sensitivity analysis for those parameters is pretty much the same. The other interesting parameters to look at are the volatility, the length of time before exit, the average rate of growth and the strike price:

[![](https://www.benkuhn.net/optopt/sensitivity_others_hu1b958df80ccd358306a003f4b4d55712_170000_1280x1280_resize_box_3.png)](https://www.benkuhn.net/optopt/sensitivity_others.png "full size")

Three observations:

1.  Volatility hardly affects the naive model at all, because the startup options are already _in the money_—most of their expected value comes from the fact that the company’s valuation is already bigger than their strike price. But the optimal-quitting model is super sensitive to volatility because high volatility means “winner” and “loser” companies separate faster, so you’ll know sooner whether you’re working for one or the other.
    
2.  Exit timing, growth rate and strike price all affect the naive model somewhat more than the optimal-quitting model, because the value for optimal quitting is driven more by outliers where the growth rate/strike price/exit timing don’t matter as much as the fact that the number is really huge.
    
3.  The optimal-quitting model almost always shows that the trade is at least $50k more valuable than you would naively expect—that’s 33% of the face value of the options. In some cases the delta is more like $100k. The only times in which it says not to take the offer is if the opportunity cost is huge, or if the volatility is really low (0.25 is the upper end of the [volatility range of the S&P 500](https://finance.yahoo.com/quote/%5EVIX/))—although it will frequently tell you to take the offer and quit relatively soon afterwards.
    

## Conclusion

What would it look like if people took this model seriously?

All the calculations above assumed that you were risk neutral, and adding risk aversion would decrease the offer value by a huge amount—even more than you’d think, since even more of the expected value comes from the few biggest exits. So startups would have a disproportionate ability to attract people with less risk aversion—mostly young people without dependents.

People would be pretty excited about accepting offers from startups, even if the total option package didn’t (at face value) look like a great deal compared to the extra salary from working at a more established company. But they wouldn’t stay at each individual startup very long, unless it did much better than expected. Instead, they’d jump from company to company, typically staying until a bit after their cliff—which would also have the nice benefit of diversifying their exposure among many different companies.

People who were following this strategy especially strongly might explicitly try to select riskier companies, in spaces where lots of companies fail but there are a few incredibly big hits, even if those companies had lower expected growth on average. They might look for riskier companies by focusing on spaces with winner-takes-all dynamics, e.g. social apps with strong network effects; or spaces that require higher up-front investment before they start making money, e.g. free consumer products.

Hmm. This all sounds suspiciously like what actually happens!

Of course, I doubt anyone is going through this reasoning process explicitly. But I do wonder if many people have some kind of intuition in this vein. If so, that could be part of a why startup employees seem OK with being paid “below market,” and why they job-hop so frequently.

## Caveats

This analysis is based on a single model with a huge number of assumptions, so please don’t take it literally at all. Here are some ways it could go badly wrong:

-   Most importantly: the uncertainty in your initial estimate of the startup’s valuation is probably bigger than the factor of 2 that this improved model gives you. (That doesn’t mean you should _ignore_ the better model, but it does mean that you should spend more effort trying to value the startup accurately, than trying to run fancy option-pricing models!)
    
-   Everything here is calculated just with expected values; it assumes you’re completely risk neutral. This is obviously wrong for most people (though maybe not if you [plan to donate your extra income](http://reducing-suffering.org/when-should-altruists-be-financially-risk-averse/)).
    
-   The model used a log-normal distribution for startup outcomes, which is probably wrong: they probably have fatter tails than that. (This should increase the value of optimal quitting, since fatter tails mean that losers and winners separate faster.)
    
-   It assumes that there’s no transaction cost from quitting to go work at a bigger company, and in particular that there’s no cost to only staying at the startup for a little while. (For instance, it might look bad on your resume to have <1-year job-hops.)
    
-   It assumes your estimate of the company’s valuation is unbiased and is easy to continuously update.
    
-   It ignores any risk of dilution, being fired, legal hijinks or otherwise getting screwed out of your options.
    

## Footnotes

* * *

1.  You may be asking yourself, “what about the strike price”? The answer is that, because almost all of the expected value of the options comes from a very small fraction of scenarios in which you exercise them, the strike price doesn’t contribute very much to the expected value. See [here](http://cdixon.org/2009/08/18/options-on-early-stage-companies/) for a more thorough explanation. [↩︎](https://www.benkuhn.net/optopt/#fnref:1)
    
2.  Another way to think about this is that when you agree to work for a startup, you’re not actually trading your your time for options in the startup. The startup is giving you (right when you join) a “meta-option” to trade your time for stock options, for _up to_ the length of the vesting period. Because the options themselves are highly volatile, this meta-option is actually quite valuable, even if it’s “out of the money” to start with (i.e., trading your time for stock options right now looks like a bad deal). [↩︎](https://www.benkuhn.net/optopt/#fnref:2)
    
3.  Based on the estimate of [Cochrane 2001](http://www.nber.org/papers/w8066). This level of volatility predicts that after 7 years, the 99th-percentile venture-backed company would have grown by a [factor of 1,280](http://www.wolframalpha.com/input/?i=99th+percentile+of+lognormal%281,+sqrt%287%29%29). Given that [Facebook’s 7-year multiple was 50k from 2004-11](https://dealbook.nytimes.com/2012/02/01/tracking-facebooks-valuation/) this seems plausible. [↩︎](https://www.benkuhn.net/optopt/#fnref:3)