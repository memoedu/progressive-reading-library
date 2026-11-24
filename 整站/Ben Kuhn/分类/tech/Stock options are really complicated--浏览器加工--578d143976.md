_Originally written for some coworkers at [Wave](https://wave.com/) who were thinking about early exercise._

Most employees at startups get some of their pay in stock options. This is often framed to the employee as “you get _x_ percentage of equity in the company.” Unfortunately, stock options are a lot more complex than true equity.

Most companies seem to do a really bad job at making their employees aware of the tax issues surrounding stock options, probably because they’re complicated, boring and don’t make a lot of sense. However, they’re very important and can make a 10-30% difference in how much equity in the company you ultimately walk away with! (Not to mention that it’s possible to get stuck in some pretty unfortunate situations if you play your cards badly enough.)

It seems like a lot of people miss out on a lot of value by not understanding deeply enough how their options work when they join a company. So this post (and the accompanying [spreadsheet](https://docs.google.com/spreadsheets/d/1HKSFDU5JNoQHfCnFBKYVLtHJgczGOjXXc12zxlUsnkw/edit?usp=sharing)) are an attempt to explain the rules governing option exercise, and how you might make decisions about your own stock options.

Contents

-   [Giant-ass disclaimer](https://www.benkuhn.net/options/#giant-ass-disclaimer)
-   [High-level summary](https://www.benkuhn.net/options/#high-level-summary)
-   [Background: why options?](https://www.benkuhn.net/options/#background-why-options)
-   [What happens when you exercise options?](https://www.benkuhn.net/options/#what-happens-when-you-exercise-options)
    -   [In which alternative minimum tax is really dumb](https://www.benkuhn.net/options/#in-which-alternative-minimum-tax-is-really-dumb)
    -   [Golden handcuffs](https://www.benkuhn.net/options/#golden-handcuffs)
-   [Some scenarios](https://www.benkuhn.net/options/#some-scenarios)
-   [Recap](https://www.benkuhn.net/options/#recap)
-   [The actual decision](https://www.benkuhn.net/options/#the-actual-decision)
-   [Further reading/sources](https://www.benkuhn.net/options/#further-readingsources)
-   [Footnotes](https://www.benkuhn.net/options/#footnotes)

* * *

## Giant-ass disclaimer

I am not a tax lawyer, this is not tax advice, it involves a whole lot of money, and it’s plausible that I’m missing some important piece of this. If you are thinking about option exercise, you are making a decision with very high stakes. If you, say, exercise early and then your company fails, _you will have lost a lot of money._ I take no responsibility if this post causes you to lose all your money, drives you mad, bores you to tears, or causes demons to fly out of your nose. Please, please, _please_ think carefully about this decision and get professional opinions from professional people rather than just relying on some guy’s internet rant!

Also, if you discover that I am missing something important, please [email me](https://www.benkuhn.net/contact/) so that I can correct it. But, like, a nice email, not a nasty one. Thanks!

## High-level summary

Owning stock options is different (and worse) than directly owning equity in a company. That’s because you need to _exercise_ them—i.e., pay the company money—in order to exchange the option for actual shares. Exercising the options costs a lot of money, so all else equal you would want to wait until the you’re sure the stock is valuable to exercise. (For instance, you might wait until the company goes public, so you know you can sell the stock on the stock market.)

But there are two downsides to that approach. First, depending on your company’s policies, you might have to give up your options if you leave or are fired. Second, if you sell the stock too soon after you exercise the options, you’ll get much worse tax treatment. That means you’ll be caught between (a) paying twice as much in taxes as an equity holder, or (b) risking losing most of your gains because the stock price moved against you at the wrong time.

So from a tax perspective, the best time to exercise the options is as soon as they’re granted to you (before they even vest). This is known as “early exercise” or an “83(b) election.” Unfortunately, early exercise can cost a lot of money up front, and if the company doesn’t succeed, you’ll lose it all. That makes the decision process kind of complicated.

OK, now to explain all that in more detail.

PS: I made [a spreadsheet model](https://docs.google.com/spreadsheets/d/1HKSFDU5JNoQHfCnFBKYVLtHJgczGOjXXc12zxlUsnkw/edit?usp=sharing) of the various different possibilities. It probably won’t make that much sense until you read the article, but you might enjoy having it open in another tab and following along.

## Background: why options?

Options are complicated, so why do companies give you options instead of shares? The answer is taxes. (That will also be the answer to all other “why” questions in this post.)

If an employer gives you straight-up shares, then the IRS will tax the shares (at ordinary income rates) when they vest. But the shares probably aren’t _liquid_—that is, it’s hard to sell them for cash—because the company isn’t publicly traded yet. If the shares are worth enough, you might end up with a large tax bill and no way to cover it. For instance, if your stock turned out to be worth[1](https://www.benkuhn.net/options/#fn:1) $10 million when it vested, you would immediately owe ~$4m in tax on it, and if the stock wasn’t liquid you would have no way of paying the tax bill. Plus, if the stock then fell later, you might end up losing money overall![2](https://www.benkuhn.net/options/#fn:2)

Instead, startups have tried to figure out a better way of compensating their employees. They have two main goals here:

-   Not have the problem where the employee has a giant tax bill and nothing convenient to pay it with.
-   Allow employees to pay long-term capital gains taxes on their equity. (Long-term capital gains tax is ~20%, whereas ordinary income[3](https://www.benkuhn.net/options/#fn:3) is ~40%, so this is a big difference.)

These are the motivating problems behind all of the different tax contortions below. Unfortunately, solving them both at once is pretty hard.

One part of the solution is that startups give options instead of stock. If you own an option, then you have the right, but not the obligation, to purchase one share of stock for $x (called the strike price) at any time while you hold the option. This purchase is called _exercising_ the option. Under certain conditions, the IRS doesn’t tax options until you exercise them, which allows you to defer the tax burden.

## What happens when you exercise options?

By default, what happens is that you owe (income) tax on the money that you made by exercising the option.

What exactly is “the money you made”? It’s the difference between the amount you paid—the strike price—and the current “fair market value” of the stock that you just bought. The fair market value is determined by something called a [409a valuation](https://en.wikipedia.org/wiki/Internal_Revenue_Code_section_409A).

(Note: 409a valuations are often much lower than the valuation that venture capitalists give the company. One reason is that venture capitalists get a different, more valuable type of stock; another is that 409a valuations are not as optimistic about growth assumptions as venture capitalists; a third is that the people who provide 409a valuations are paid by the company, and the company wants to look less valuable to the IRS so that they have to pay less in taxes.)

Usually, stock options are issued with a strike price equal to the stock’s fair market value when the options are issued.[4](https://www.benkuhn.net/options/#fn:4) That means that if you exercise right when your options are granted, the “money you made” is $0 for tax purposes. On the other hand, if your company’s fair market valuation goes from $1 per share to $10 before you exercise, then you will have “made” $9 per share from the IRS’s point of view, and they will try to tax you on it.

You might notice a problem with this, which is that when you are getting taxed, you still have stock and not money, so you can’t necessarily pay the tax. It’s the same problem as in the first section! Options allow you to defer the taxes for a while, but they don’t completely solve the problem of getting taxed before you have money.

Of course, you could just sell the stock on the same day you exercise the options, but then your earnings would be ordinary income tax instead of long-term capital gains, which is the other problem we’re trying to avoid.

The IRS noticed this issue, so they invented a special set of kind of option, called an “incentive stock option” or ISO, that can get better tax treatment. ISOs are taxed as follows:

-   When you exercise, no tax;
-   When you sell the shares, you pay long-term capital gains tax…
-   As long as you sell the shares at least 2 years after the options were granted[5](https://www.benkuhn.net/options/#fn:5) and 1 year after the options were exercised.

Which is a much better deal! But then the IRS put their foot in it:

### In which alternative minimum tax is really dumb

The Alternative Minimum Tax is a second tax that you calculate, and pay if it’s larger than your normal tax. You calculate it using a much lower percentage (28% federally; some states have AMTs also) and much higher personal exemption ($40k+ at the bottom of the range), but it allows fewer deductions. It was designed to prevent rich people from taking advantage of deductions to pay nothing in taxes.

Unfortunately, one of the things that is not exempt from the alternative minimum tax, is gains from exercised but un-sold stock options.[6](https://www.benkuhn.net/options/#fn:6) Even the special IRS-designed ones! So if you exercise your options after the stock has gone up a lot, you will _still_ have the problem where you have tax, and a bunch of valuable stock, but don’t want to sell the stock to pay the tax.

This situation has a pretty big tail risk. Here’s a hypothetical situation:

1.  You are granted 100,000 options at a strike price of $1 per share.
2.  You exercise the options when the company’s stock is $11 per share.
3.  You hold the stock through the end of the tax year, waiting for it to qualify for capital gains treatment.
4.  After the end of the tax year, the stock declines from $11 per share to $3 per share.

In this situation, you’ll still have to pay the AMT on $10 per share or $1m in profits. That would work out to about $280,000. But the profit you’ve actually realized is only $2 per share or $200,000. Your stock sale has left you $80,000 in the red![7](https://www.benkuhn.net/options/#fn:7) [8](https://www.benkuhn.net/options/#fn:8)

In this case, you could, again, sell the stock to pay the alternative minimum tax, but then you would end up selling it less than a year after exercise, so it would violate the IRS’s special tax-friendly rules and you would pay ordinary income tax on the gains.[9](https://www.benkuhn.net/options/#fn:9)

### Golden handcuffs

On their own, these tax issues wouldn’t be that bad. At worst, you could just resign yourself to paying ordinary income tax, and when your company exited, your giant pile of cash would be somewhat less giant, but still the same order of magnitude. However, there’s another wrinkle that combines with the tax issues to make options especially difficult.

That wrinkle is that options can _expire_; that is, you can lose your right to purchase the underlying stock. The default stock option policy is that if you leave the company—voluntarily or not—the options will expire in three months. (I believe that the policy is this way for complicated legal reasons, though apparently surmountable ones since a growing number of companies don’t do this.)

So here’s another hypothetical:

1.  You are granted 100,000 options when the company is worth $1 per share.
2.  The company’s fair market value rises to $11 per share (but it’s still private and illiquid).
3.  You are fired because of stupid corporate politics.

If you don’t exercise the options, you’ll leave $10 per share or $1m sitting on the table. If you do exercise the options, you’ll get $1m of income that counts toward the alternative minimum tax, so you’ll be looking at a $280k tax bill. But the shares still won’t be liquid, so you’ll have literally no way to pay. And even if you do come up with the cash, you’ll have the same problem as above where the stock could fall and leave you in the red.

This is obviously really bad. As a result of this dynamic, lots of early startup employees are unable to leave without giving up most of their compensation, a problem known as “golden handcuffs.”

You can get out of this bind at least partially, by making arrangements with organizations like the [Employee Stock Option Fund](http://www.esofund.com/), which will cover your tax bill in exchange for taking a share of your stock (or doing something with an equivalent payout structure). I don’t know what kind of rates the ESO Fund charges, but I hear they’re terrible. That’s not too surprising, since people with golden handcuffs don’t have many other options.

Some companies have a different policy that allows you to keep your options for 7-10 years after you leave. Even with these more generous policies, though, once you leave the options lose their special status with the IRS (they “convert from ISOs to NSOs”), and you have to pay ordinary income tax on the gains as soon as you exercise them. (So you might as well just hold them until the stock is liquid and exercise them only when you want to immediately sell the underlying stock.) This is still not great, but obviously a lot better than being bankrupted by a tax bill.

## Some scenarios

That’s all the facts about how option exercise works. To summarize, there are a few different option-exercise scenarios you might find yourself in.

-   You exercise your options right before you sell the underlying stock. You pay ordinary income tax on the entire gain.
    
-   You exercise your options, then wait a year before selling so that you can pay long-term capital gains. In the year that you exercise, you need to pay AMT on the “money” you made at exercise. After that, this splits into two further scenarios:
    
    -   In the happy world, the stock stays mostly flat or goes up over that year. You sell the stock, and don’t pay very much additional taxes (if any) because of the giant AMT credit you got last year.
    -   In the sad world, then the stock craters. If it craters in the same tax year as you exercised, you can sell it (for not very much, paying ordinary income tax rates) to avoid paying AMT. Otherwise, you are still on the hook for the AMT liability, even if you don’t make much money from the sale. You’ll have the AMT credit, but depending on how much you paid, it could be large enough that you may never recover all of it.
    
    (There are things like the Employee Stock Option Fund that will help you mitigate the downside risk in the sad world, but they take a bunch of upside as compensation. I don’t really know the details here.)
    
-   You exercise your options at some random time because you just left the company and the alternative is forfeiting your equity. You pay alternative minimum tax on the “money” you made at exercise, and the stock isn’t liquid so you need to come up with the cash some other way.
    
-   You exercise the options early, right when they’re granted. Now you have stock that you can just sell whenever, not incur a tax liability until you actually have cash, and pay long-term capital gains on your gains. But if the company goes under, you’re out by however much you paid to exercise.
    

## Recap

If you exercise your options when they are granted, then:

-   You will pay a lot of money at that time. (Tens of thousands of dollars is not uncommon.) You will not get this money back if the company fails.[10](https://www.benkuhn.net/options/#fn:10)
-   You will not owe any taxes at that time.
-   If you leave the company, you will be able to keep all of your vested equity, and get back the money you paid to exercise any equity that hasn’t vested yet.
-   When you sell the underlying stock, you will pay long-term capital gains taxes (~25%).[11](https://www.benkuhn.net/options/#fn:11)

If you do not exercise your options when they are granted, then:

-   If you leave the company, you may be forced to either exercise your options or forfeit them. Even if not, you’ll pay ordinary income tax rates on the gains.
-   When you exercise the options, if the stock has appreciated, you will owe a lot of money in taxes (specifically, the ~28% Alternative Minimum Tax), which you may not be able to pay without selling the stock.
-   If you can’t sell the stock (for instance, because you left the company while it was still private), you are SOL. Because of this, _many employees of successful startups have to stay there until the stock becomes liquid_.
-   If you sell the stock immediately, you will owe ordinary income tax (~50%) on the gains.
-   If you wait a year to sell the stock, you will pay long-term capital gains on it, except you probably already paid more than that when you exercised, so you won’t pay anything extra.
-   If you exercise when the stock price is high, and then the price tanks before you sell, you will still owe the alternative minimum tax of 28% of the gains at exercise time. If the stock tanks hard enough, you might owe more in AMT than you make when you finally sell.

If you don’t early exercise, you’ll end up paying a bit more taxes (probably) since you’ll be paying AMT rates instead of capital gains rates on some of the gains. But that’s not the main issue. The main thing to worry about if you don’t early exercise is the possibility of golden handcuffs.

Even if your company’s options take long enough to expire that you don’t have to worry about golden handcuffs, there’s one other thing to worry about, which is the logistical difficulty of exercising them once the stock is liquid. Unless you sell them right when you exercise (forgoing the favorable tax treatment entirely), you will end up incurring a tax liability before you have money to pay it. That (a) can be scary and (b) has a tail risk of going badly if the stock goes down at the wrong time (in a different tax year from when you exercise)—unless you trade some of your upside for downside protection from something like the Employee Stock Option Fund.

## The actual decision

This article probably comes off as recommending that you exercise your options early. If so, that’s unsurprising, since that’s what I chose to do!

The way I thought about it was that early exercise was basically trading cash for more equity in the company. It’s not literally more equity—the number of shares that you get stays the same. But it’s more equity in the sense that, for any given valuation that the company exits at, you’ll come out owning a larger percentage of that valuation after taxes. I played with a bunch of scenarios in a spreadsheet and decided that I was happy with trading that much money for that much extra effective equity. You can find a sanitized version of that spreadsheet [here](https://docs.google.com/spreadsheets/d/1HKSFDU5JNoQHfCnFBKYVLtHJgczGOjXXc12zxlUsnkw/edit?usp=sharing).

Despite my personal conclusions, there are some pretty compelling reasons not to exercise early:

-   It costs a lot of money! If your company fails, you will not get that money back. **Don’t early exercise if you can’t afford it.**
-   Even though I spent a lot of time talking about tail risks like getting wiped out by an AMT bill you can’t pay, there’s a completely predictable way of avoiding that risk, which is to sell the stock immediately after you exercise the options. Yes, you’ll pay more in taxes, but your downside will be capped.
-   That means that early exercise is a “win-more” thing. If your company succeeds, you’re going to do pretty well whether or not you exercised early. If the company fails, the tens of thousands you paid for the strike price will matter a lot more!
-   Even if your options have a short expiry window when you leave, you might be able to get around it via some kind of deal with the Employee Stock Option Fund.
-   If you’re planning to donate a large amount of your options (but not enough to run into the charitable tax deduction cap), this reduces the importance of the tax rate you pay. Your donation will also be deductible for AMT purposes.

If you’re deciding yourself, I highly recommend running some actual numbers to see what they payoffs look like in different scenarios!

## Further reading/sources

-   Another good overview of ISOs and early exercise: [Alex Flint’s Quora explanation of early exercise](https://www.quora.com/What-are-the-advantages-of-exercising-ISO-options-early-to-take-advantage-of-long-term-capital-gains-How-much-does-that-actually-save-considering-you-need-to-pay-AMT/answer/Alex-Flint)
-   Lots more useful detail: Wealthfront on [Improving Tax Results for Your Stock Option or Restricted Stock Grant, Part 3](https://blog.wealthfront.com/improving-tax-results-stock-options-restricted-stock-grants-2/)
-   On donations: [Fidelity on donating appreciated stock to charity](http://www.fidelitycharitable.org/giving-strategies/tax-estate-planning/appreciated-securities.shtml)
-   Financing for covering your losses to tax/strike price: the [Employee Stock Option Fund (alternative way of avoiding the tax-liability problems if you exercise late)](http://www.esofund.com/)
-   Collaborative, badly-organized but large article: [The Open Guide to Equity Compensation](https://github.com/jlevy/og-equity-compensation#common-scenarios)
-   Blog post by Alex McCaw: [An Engineer’s Guide to Stock Options](http://blog.alexmaccaw.com/an-engineers-guide-to-stock-options#exercising)
-   On non-qualified stock options: [Fairmark on exercising non-qualified stock options (what your options will turn into 3 months after you leave)](http://www.fairmark.com/execcomp/nqoexer.htm)

_Thanks to all [my coworkers](https://wave.com/about/) who helped me work through this and gave feedback on drafts!_

## Footnotes

* * *

1.  How to figure out the worth of illiquid stock is a thorny question that I’d rather postpone for now. [↩︎](https://www.benkuhn.net/options/#fnref:1)
    
2.  If the stock fell, you would get to write off the loss against future income. BUT: (a) it would be a capital loss, so you would only get to write it off against capital gains income; (b) you would only get to use the full write-off if you made another $10m in capital gains, which is improbable. [↩︎](https://www.benkuhn.net/options/#fnref:2)
    
3.  You might have heard people referring to “short-term capital gains tax” also. Short-term capital gains are taxed the same as ordinary income, so I’m just rolling the two together here. [↩︎](https://www.benkuhn.net/options/#fnref:3)
    
4.  This is because it’s required by the IRS in order for the options to qualify for deferred tax treatment. [↩︎](https://www.benkuhn.net/options/#fnref:4)
    
5.  I don’t know whether “granted” means when the options vested, or when the vesting started. This guide says that “there is a murky part of the rules… that implies” that the holding period starts at vesting. Sigh, tax law. [↩︎](https://www.benkuhn.net/options/#fnref:5)
    
6.  If you exercise the options but then sell them during the same tax year, the exercise _doesn’t_ count towards AMT; you’ll just pay ordinary income taxes on the price of the stock when you sell. [↩︎](https://www.benkuhn.net/options/#fnref:6)
    
7.  When you incur a large AMT bill, you technically get an equal tax credit that can be applied against some taxes in future years. However, depending on the AMT you incur in this scenario, you might take a long time to use up the credit, or even never use it up. [↩︎](https://www.benkuhn.net/options/#fnref:7)
    
8.  These numbers are worst-case—most companies don’t decline 70% in value that quickly. But they do happen. Groupon was worth $26 at IPO in November 2011; a year later. Anyone who exercised and then tried to wait a year to qualify for better tax treatment would have been royally screwed—although it would have been more prudent to exercise in January (see below). [↩︎](https://www.benkuhn.net/options/#fnref:8)
    
9.  Theoretically, you could exercise your options in e.g. January 2023, then sell the underlying stock in February 2024 to pay the AMT bill due in April 2024, and still have your stock sale qualify for capital gains treatment. But this leaves you with a ton of exposure to random timing issues with the stock, since you’re forced to sell within a very small window. For instance, if you tried to do this with Groupon stock you probably would have lost everything. [↩︎](https://www.benkuhn.net/options/#fnref:9)
    
10.  You will get some of it back if you leave before all your stock is vested. [↩︎](https://www.benkuhn.net/options/#fnref:10)
     
11.  As long as you sell them at least a year after they were granted. [↩︎](https://www.benkuhn.net/options/#fnref:11)