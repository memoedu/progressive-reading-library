_Update 2019-02-01: the 2018 tax code changes have changed the calculus substantially due to a higher standard deduction of $24,000 and a higher donation deduction limit of 60%. If my math is correct, this makes bunching more attractive if donating between $15k and 30% of your income, and less important otherwise. Thanks to Jeff for pointing out the update._

In my post on [giving away money](https://www.benkuhn.net/giving-101), I wrote about the strategy of “donation bunching:”

> Because there’s essentially a floor of $6,000 on your tax deduction for a single year, it can be advantageous to shuffle your deductions around. For instance, if you plan on donating $5,000 every year, then you could alternate between donating it in late December and early January, so that you donate twice in one calendar year and not at all in the next. During the years you don’t donate, you’ll take the $6,000 standard deduction, and during the years you do, you can take a $10,000 itemized deduction, decreasing your net taxable income by $4,000.

> In fact, as long as you’re donating less than 25% of your income, this will probably be useful (although it will force you to space your donations out more than you otherwise might).

Eitan Fischer pointed out that state or local income and property taxes are also deductible, which limits the effectiveness of bunching. (In fact, if you’re single and earn more than about $80k in California or NYC, or $120k in Massachusetts, your state and local taxes are probably above $6k by themselves, which means you should itemize every year, and probably avoid the hassle of bunching. Thanks to Holden for pointing out that this limit is more severe than I realized.)

Andy Chrismer pointed out that you can donate the money to a donor-advised fund, allowing you to distribute it whenever you want and mitigating one of the major downsides. Meanwhile, I realized on my own that bunching is helpful to people whose total deduction (including donations) exceeds the standard deduction, which somehow hadn’t occurred to me before.

But the mechanics of donation bunching are a bit confusing, so I think this deserves a little bit of math. How much exactly will donation bunching decrease your taxable income? Suppose you donate `$D$` dollars in donations and pay `$T$` dollars in other deductible expenses (e.g. taxes) in a given year. Then we’ll find out how much donation bunching reduces taxable income (over a given two-year period) by calculating the difference between two years of deductions when bunching (one “on” year where you donate twice, and one “off” year where you don’t donate at all) and when not bunching (two years where you donate once).

-   If you would ordinarily itemize (that is, if `$D + T \ge 6,000$`), then your deduction in an “on” year is `$2D + T$` and in an “off” year is `$6,000$`, and your deduction if you don’t bunch is `$D + T$`. So the difference is `$(2D + T) + (6,000) - 2(D + T)$`, which works out to an extra deduction of `$6,000 - T$`.[1](https://www.benkuhn.net/bunching/#fn:1)

-   If you would ordinarily pay the standard deduction every year (that is, if `$D + T < 6,000$`), then your deduction in on-years is `$2D + T$` and in off years is `$6,000$`, and the deduction if you don’t bunch is `$6,000$` as well. So the gain from bunching is `$(2D + T) + (6,000) - 2(6,000)$`, which works out to an extra deduction of `$2D + T - 6,000$`.

This analysis ignores a couple of corner cases:

-   If `$D$` is greater than one-quarter of your adjusted gross income, then you may not want to bunch because in your “on” years you will hit the deduction cap: you can’t deduct charitable donations of more than 50% of your AGI.
    
-   If you’re on the border of a tax bracket, such that the higher deductions in your “on” years move you down to a lower marginal tax rate, then this will save you less money than it appears to. However, there are very few cases in which this would actually make the difference between bunching and not bunching. If in doubt, you can do the math for your specific situation.
    

To figure out how much you save over a two-year bunching period, you can multiply the decrease in taxable income calculated above by your marginal tax rate. For instance, if you’re single and make $50,000 a year, pay $3,000 in other deductible expenses and donate $5,000, then bunching donations would reduce your taxable income by $3,000 every two years. Since you’re in the 25% tax bracket, this translates into a savings of $750 every two years or $325 a year. Not bad! And in optimum circumstances the yearly savings could go as high as $1,200.[2](https://www.benkuhn.net/bunching/#fn:2) If you’re okay with the caveats above about timing and corner cases, bunching your donations is definitely something to consider.

* * *

1.  This goes negative if `$T > 6,000$`, which reflects our assumption that you take the standard deduction in “off” years. If you pay more than $6,000 in non-charity deductible expenses, then you should never take the standard deduction, and bunching doesn’t make any sense. [↩︎](https://www.benkuhn.net/bunching/#fnref:1)
    
2.  Based on a 40% marginal tax rate, no other deductible expenses and a yearly donation of $6,000 or more. This translates into a $6,000 decrease in taxable income every two years, for a $1,200 yearly savings. [↩︎](https://www.benkuhn.net/bunching/#fnref:2)