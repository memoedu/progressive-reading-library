_Conceptual background: [stock option](http://www.investopedia.com/terms/s/stockoption.asp), [strike price](http://www.investopedia.com/terms/s/strikeprice.asp), [post-money valuation](http://www.investopedia.com/terms/p/postmoneyvaluation.asp)._

Suppose you get a job offer for a senior engineering position at [Square](https://en.wikipedia.org/wiki/Square,_Inc.) in November 2014. Fresh off raising a huge round led by Singapore’s sovereign wealth fund, at a $6 billion [valuation](http://www.investopedia.com/terms/p/postmoneyvaluation.asp), they offer you 100,000 [options](http://www.investopedia.com/terms/s/stockoption.asp) at a [strike price](http://www.investopedia.com/terms/s/strikeprice.asp) of $10 a share.[1](https://www.benkuhn.net/terms/#fn:1) The $6 billion valuation implies a share price of $15, so your options are effectively worth $5 each or $500k total. That’s a lot better than anyone else can offer you! So you take the job.

A year later in November 2015, Square’s IPO prices at $9 per share, for a market capitalization of $2.9 billion.[3](https://www.benkuhn.net/terms/#fn:3) The stock pops to $12 on opening day, but by the time your [IPO lockup](https://en.wikipedia.org/wiki/Lock-up_period) expires in June, it’s back to $9 and your options are [no longer even worth exercising](http://www.investopedia.com/terms/o/outofthemoney.asp). Your $500k has essentially evaporated. What happened?

The obvious answer is that Square did really badly in 2015, and so their valuation went down by half. But actually, they seem to have done quite well. Their [IPO prospectus](https://www.sec.gov/Archives/edgar/data/1512673/000119312515343733/d937622ds1.htm) showed 50% year-over-year revenue growth—the same rate as the previous year. Plus, Square had entered the massive small business loan market with Square Capital, which was already making 5% of their revenue. If sustained core growth and successful market expansion wasn’t enough, what else did investors want to see Square accomplish in 2015? How did it fail? This answer isn’t too satisfying.

We can find a better explanation in [Gornall and Strebulaev (2017)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2955455), a fascinating recent paper on how private companies are valued. Gornall and Strebulaev trawled through a huge mess of data and legal documents from startup financing rounds, and their findings really startled me. Before we follow them into the weeds of venture capital, let’s look at their take on Square’s IPO, which hinges on a contract term granted to Singapore (and Square’s other Series E investors) called a [ratchet clause](https://www.law360.com/articles/729186/square-ipo-soars-but-ratchet-provision-worries-some).

* * *

The ratchet clause guaranteed Singapore at least a 20% return on their investment if Square went public—if the IPO price was below the price at which Square issued them shares, Square would give them more shares to make up the difference. As it happened, Square’s ratchet clause [triggered](https://blogs.wsj.com/digits/2015/11/18/square-pays-93-million-penalty-to-some-investors-in-ipo/) and forced them to issue $93 million worth of additional stock to the Series E investors.

Fortunately for you as a hypothetical Square employee, Singapore’s investment was relatively small (a 2.5% stake), so the ratchet wouldn’t have made that much difference to you. But it made a _huge_ difference to Singapore: it practically eliminated their risk of losing money in the deal. That meant that Singapore could be much more flexible on the other terms of the investment. In this case, they probably agreed to a much higher valuation than they would have otherwise.

The valuation of a deal is usually interpreted as “how much the whole company is worth.” But that’s not quite right—actually, it’s completely made-up. It’s made up by taking how much Singapore was willing to pay for their shares, and multiplying by the total number of _all_ shares outstanding.[4](https://www.benkuhn.net/terms/#fn:4) That would be valid if everyone else got the same kind of share as Singapore, but they very much did not. The other investors had worse shares, and so the headline valuation overstated Square’s true worth.

The problem for you was that you used the same valuation number as Singapore—$6 billion—when evaluating your option package, but you didn’t have Singapore’s deal terms. Your options were on _common stock_, which didn’t have a ratchet, and so the stock was worth a lot less.

* * *

How much less? It turns out you can calculate this, using the same kind of math [I used in a previous post](https://www.benkuhn.net/optopt) to figure out how much the option of quitting your job was worth. Roughly, the procedure is:

-   Make an assumption about how the company’s valuation will change over time (probably that it’s some kind a [geometric Brownian motion](https://en.wikipedia.org/wiki/Geometric_Brownian_motion)).

-   Figure out the probability distribution of when companies exit, and whether they exit via IPO or acquisition.
    
-   Answer the question: assuming the company’s fair valuation is $X today, what is the expected payoff of their most recent investment?
    
    -   For each exit type and valuation, figure out (a) how likely it is based on the assumptions above, and (b) how much money the most recent funder would make.
        
    -   Use these to compute the expected value.
        
-   Run the above on a bunch of different possible values for $X (the current fair valuation), and find the one where the expected payoff is equal to how much the investors paid for it.
    
-   Now that you’ve decided what the fair valuation of the whole company is, you can use the above to find the expected values of the other investors and the common stock as well.
    

The cool thing is that this model mostly only assumes the [efficient market hypothesis](https://en.wikipedia.org/wiki/Efficient-market_hypothesis). That’s a big (and iffy) assumption, but after that (and the distribution of future returns), you can use the company’s fundraising history to uniquely determine its fair market value.

Gornall and Strebulaev did just that for Square, and they found that, when Singapore invested at a headline valuation of $6 billion ($15/share) with an IPO ratchet, it actually implied that their valuation of the common stock would have been about $5.50 per share. Ouch! By blindly plugging in Singapore’s valuation number into your own calculations, you were off by almost a factor of 3. And that factor of 3 made the difference between your stock options being a windfall ($500k in the money) and a pittance (currently worth nothing and deeply underwater).

Funnily, the more accurate valuation suggests that 2015 actually wasn’t a bad year at all for Square—their common stock value went all the way from $5.50 a share in their Series E, to $9 in their IPO! Which, as mentioned above, makes much more sense given the material facts.

In fact, for the ratchet _not_ to kick in, Square would have had to either grow their fair market valuation something like 4x as fast as they historically had, or delay their IPO for three more years. Those are both so unlikely that they make Singapore’s “equity investment” look like fixed-payoff debt, which means the “valuation” was almost completely meaningless. Singapore’s predicted payoff would have looked almost the same whether it invested at a valuation of $6 billion or $60 billion.

* * *

If Gornall and Strebaulev (henceforth G&S) had just done this for Square, it already would have been a pretty cool paper. But they didn’t stop there. Instead, they did a comprehensive study of how the problem of sneaky fundraising terms applied to a huge sample of more than 200 companies valued at $1 billion or more. For each one, they figured out the terms their investors got, and used this to compute how overvalued each one would be if you used the headline valuation as the company valuation.

Square was near the top of the list, but there were lots of companies that were even more overvalued. The median company was overvalued by 36%, and the most-overvalued, [Box](https://en.wikipedia.org/wiki/Box_%28company%29), by a whopping 200%.

Overvaluation seemed relatively less common among “household name” startups, fortunately. G&S were kind enough to provide estimates for each individual company, so I looked up a bunch of the ones that most people might have heard of:

| Company | Overvaluation[5](https://www.benkuhn.net/terms/#fn:5) |
| --- | --- |
| Airbnb | 15% |
| Box | 200% (!) |
| Dropbox | 21% |
| Lyft | 11% |
| Palantir | 15% |
| Pinterest | 21% |
| Snap | 0% (!) |
| SolarCity | 178% |
| SpaceX | 65% |
| Square | 169% |
| Theranos | 31% |
| Twitter | 21% |
| Uber | 12% |
| Vox | 48% |
| Whatsapp | 60% |

In addition to IPO ratchets, they identify a few other common deal terms that make VC’s stock more valuable:

-   **Liquidation preference**. This means that if the company is sold (but not if it goes public!), the investors are guaranteed at least their full investment back before anyone else gets any money. For instance, if I invest $1m for a 20% stake in WidgetCorp and it later sells for $3m, I’ll get $1m, even though my stock only entitles me to $600k. (The remaining $1m will be split proportionally among the common stockholders.) Here’s what the payoffs would look like if there were 10 million shares in WidgetCorp:
    
    ![](https://www.benkuhn.net/terms/liqpref.svg)
    
-   **Seniority.** If you have a senior liquidation preference, you get paid even before all the other investors who also have liquidation preferences. This doesn’t affect the common stock price, but it still makes the last investor’s stock more valuable (at the expense of previous investors) so it still contributes to overvaluation. Here’s what the WidgetCorp payoffs would look like if they raised two $500k rounds with different seniorities:
    
    ![](https://www.benkuhn.net/terms/seniority.svg)
    
-   **Participation.** This is a crazy structure where, if the company is sold, you get your standard liquidation preference _and then also_ get a share of the money left over after liquidation preferences are satisfied. If we took the same deal as before ($1m for a 20% stake in WidgetCorp) but also gave me participation rights, then in a $2m exit I’d get $1m for my liquidation preference _plus_ another $200k for my 20% stake in the remainder. Participation usually has a “cap” of e.g. 2x, which means that the investor can’t get a return of more than 2x their original stake from participation, except that they always get at least as much money per share as the common stockholders.
    
    If this sounds silly and hard to understand, that’s because it is. Take a look at the WidgetCorp payoff diagram below for participation with a 2x cap. Then ask yourself why anyone would sell an instrument with such a wobbly payoff function. (I’m guessing the answer is because they don’t understand it.)
    
    ![](https://www.benkuhn.net/terms/participation.svg)
    
-   **Automatic conversion exemption**. Normally, when a company IPOs, the “preferred stock” (what VCs get) gets converted to common stock (what is sold on the public markets). Some investors are allowed to refuse to convert their shares; apparently, refusing to convert effectively makes an IPO unworkable and forces the company to get acquired instead. If the company’s valuation falls and an IPO would lose its investors money, they can force the company to get acquired instead, and then use their liquidation preference to get all their money back.
    

All of these provisions are basically hidden options that the company writes its investors. They’re not quite the same as stock options, because the VCs’ investments do have downside risk if the company does badly enough. But they have the same kind of kinked payoff function, and that means you can use the same type of option-pricing math to figure out how much they’re worth—which is what G&S did.

* * *

The thing that surprised me most was that G&S were actually able to collect enough data to run their models on. To calculate the implied valuation of a company, you need to know how much equity each investor owns in the company, how much they paid for that equity, and all the terms of their investment—something that I thought companies really wouldn’t want to reveal.

Fortunately, G&S are more resourceful (and have a larger research budget) than me. They bought the fundraising data from various private databases. More excitingly, they also found all the supposedly-confidential fundraising terms! It turns out that whenever a company issues a new kind of stock, they have to file a restated certificate of incorporation detailing the terms of the stock, including the terms they described above. G&S got a team of lawyers to read through all the restated certificates of corporation (yes, really), parse all the legalese, and record the terms in a standardized format for input into their models.

Of course, the authors didn’t publish their parsed data, which is really unfortunate since it would be a transparency godsend for employees and founders. Maybe it would violate the databases’ nondisclosure agreements or something?

* * *

When I described the modeling procedure above, I snuck in a huge assumption. I told you to “Figure out the probability distribution of when _companies_ exit”—implying that the distribution was the same for all companies and that exits were exogenous (independent of any other parameters like capital structure). But that’s probably wrong! The people with the most control over exit timing are the founders, and the capital structure has a huge effect on their incentives.

This is most obvious in the early stages. If a founder starts from nothing, raises $1m at a $5m valuation with no liquidation preference, and sells for $500k, that’s not a bad outcome for the founder (they’ve still made $400k) but it’s terrible for the investor (they’ve lost $900k). The incentives are misaligned because the founder is way less diversified and more risk-averse than the investor, so they’re much happier with small exits. This is probably why investors want liquidation preferences (and why they may often make the company more valuable): it helps align founders toward exits with higher expected value.

Of course, it doesn’t completely align incentives. If a founder raises $1m with liquidation preference, they won’t care about the difference between exiting at $1m (and making their investor whole) and plowing the company into bankruptcy. The optimal thing for incentive alignment is probably something in the middle, where the common stock gets a small share of the payout until the preferred’s liquidation preference is satisfied, and a large share afterward. But that would be complicated to structure.

(Another alternative for incentive alignment would be to let founders partly cash out. Venture capitalists are extremely reluctant to do this, possibly because it’s a bad signal when founders ask for it.)

That’s probably the biggest limitation of the paper, along with related assumptions about how investors vote to force liquidation instead of a money-losing IPO. Unfortunately, I don’t have a great intuition for how it affects their results.

* * *

While we’re talking about limitations, another awesome thing G&S did was to include a comprehensive series of robustness checks, where they run their model with other assumptions and see how much their conclusions change. The only one that leapt out at me was their assumption about leverage.

Leverage is the ratio of a company’s debt to its fair market value. Debt is senior to all shares, so if you’re investing in an indebted company, your liquidation preference only starts mattering at higher valuations. It’s less likely to exit at a higher valuation, so this reduces the impact of your liquidation preference, which reduces the difference between your shares and common stock, which reduces the company’s overvaluation.

The authors assumed 0% leverage by default. If all companies instead had 10% leverage, the median overvaluation would go from 36% to 26%. Lots of companies do raise debt, so this probably lowers the overvaluation somewhat. G&S argue that:

> In practice, VC-backed companies do not issue much debt and the debt that is issued generally has significant option-like components. This follows naturally from our volatility assumption… using 90% volatility, the median unicorn loses 85% of its value over the next five years. These value losses are not conducive to significant indebtedness.

A lot of unicorns do [raise](https://en.wikipedia.org/wiki/Uber_%28company%29#Financing) [at least](http://www.reuters.com/article/us-airbnb-financing/airbnb-gets-1-billion-debt-facility-from-u-s-banks-source-idUSKCN0Z20AJ) [some](https://www.crunchbase.com/funding_round/xiaomi-debt-financing--f6332a00) [debt](http://www.reuters.com/article/us-didi-chuxing-fundraising/chinas-didi-chuxing-raises-7-3-billion-in-new-funding-idUSKCN0Z123G), although it’s mostly only unicorns that look very likely to IPO (so unlikely to have liquidation preferences matter). Since G&S had access to a lot of databases of funding events, I’m surprised they didn’t try harder to test this assumption with real data instead of theory.

Other than assumptions, my biggest worry about the paper is data quality. While the authors did try pretty hard to collect correct data, they had to make some assumptions (for instance, about the number of shares actually issued in some funding rounds). Also, it’s not clear that certificates of incorporation always capture all important funding terms, or that their team of lawyers parsed the legalese correctly (as they note, this is hard!). I’d have liked to see them look more at how reliable their coding process was, and spot-check when they filled in data (e.g. about share counts) with more detailed research.

* * *

A puzzle that G&S don’t try to answer is: why do these terms happen?

When VCs agree to invest at a higher valuation in exchange for more downside protection, they’re reducing their payoff in best-case scenarios (because they own less of the company), and increasing it in the worst-case scenarios. But that doesn’t make sense for the incentives of either VCs or founders.

Venture capitalists usually get paid “[2-and-20](https://en.wikipedia.org/wiki/Venture_capital#Compensation),” i.e. 2% of the principal they manage plus 20% of the returns. In other words, if they don’t break even, they get paid the same whether they lose 10% of their investors’ money, or 90% of it. So you’d expect them not to care about worst-case scenarios, and care much more about improving the best case, 20% of which goes straight into their pockets.

Meanwhile, founders generally don’t have a lot of money. That means they probably have diminishing returns to additional money. In other words, the money they’re giving away to VCs in the worst case is probably worth relatively more than the additional money they’re getting in the best case from a higher valuation.

In other words, giving VCs a better worst-case, and a worse best-case, is the _exact opposite_ of the risk-sharing structure that makes sense. What gives? Here are some hypotheses:

1.  As mentioned above, the downside protection for VCs actually makes the company more valuable by removing a perverse incentive from the founders.
    
2.  Founders care a lot about valuation as a vanity number (or more charitably, a recruiting tool), and so they accept worse terms in order to cross psychologically important thresholds like $1 billion.
    
3.  Founders are more optimistic than VCs about the probability of best-case exits. When the founders and VCs compute the expected value of the valuation-for-protection trade, both of them come up with a positive number, because they’re plugging in different probabilities.
    
4.  Founders have no idea how valuable the downside protection they’re giving away is.
    

While the first three are certainly factors, I can’t help thinking there must be a lot of the fourth going on. Personally, I was astonished to know how much difference these terms made—I’m sure many founders would be too. VCs do enough of these deals for it to be worth modeling out the impact of these terms, but founders only ever do a few fundraises and are too busy doing other things to think deeply about terms. So there’s a huge asymmetry of information and savviness.

* * *

What does this mean if you’re thinking about working at a startup?

First, this is only one piece of the puzzle, and not nearly the most important one. (The most important option-related piece is the expiry window—if a company’s options expire less than 7 years after you leave and you can’t early exercise, they’re practically worthless because of how hard that will constrain your future choices.) There are [lots](https://www.benkuhn.net/options) of [other](https://www.benkuhn.net/optopt) things to think about when you look at option packages. My main goal with this post is to push compensation to be generally more transparent and less confusing, not because I think it has strong implications for individual people’s choices.

That said: I [used to think](https://www.benkuhn.net/late) that it was probably a pretty good deal to work as a relatively late employee at a really successful startup. In that analysis, I used the startups’ post-money valuation growth as a proxy for the returns you’d get as an employee. That probably overstates both the initial valuation, and the growth rate, of your equity by a lot. I haven’t redone the analysis yet, but I’d guess that late-stage startup offers are a lot less good than my previous take suggested.

The overvaluation problem isn’t as much of an issue if you’re looking at early stage startups, because they don’t have as many investors that they could have given bad terms to. (The paper’s [appendix](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2968003) detailed the terms given to each investor in each series for every company in their sample; only a few companies gave particularly strong protections to Series A investors.)[6](https://www.benkuhn.net/terms/#fn:6)

Either way, though, this underscores the importance of understanding a company’s full funding situation when you get a job there. Some companies apparently like to claim that this is too confidential for employees. That’s a _huge_ red flag—they’re essentially refusing to tell you the full payoff structure of the equity they’re paying you with—and you should let them know as much.

_Thanks to Ewa Bigaj, Dan Luu, Max Novendstern, and Yuri Vishnevsky for their thoughts on a draft of this post. Thanks to Zi Chong Kao for spotting typos._

* * *

1.  See Square’s [S-1](https://www.sec.gov/Archives/edgar/data/1512673/000119312515343733/d937622ds1.htm), page 103. If you joined in November, the options would have been granted in the December 17th batch, for which the exercise price was $10.06. They granted 3.4 million options in that batch, and 2.2 million in the previous month’s batch; if they were giving half the options to senior people and hiring 10-15 senior people per month, you’d end up with 100k options. Note that “senior” for engineers basically means “not your first job.” [↩︎](https://www.benkuhn.net/terms/#fnref:1)
    
2.  I’m not sure why it’s sometimes lower; I would have thought that the common stock was always the most-overvalued class of stock, since it’s always the least-protected. [↩︎](https://www.benkuhn.net/terms/#fnref:2)
    
3.  You’ll notice these numbers don’t line up with previous ones, which said Square was worth $6 billion at $15 per share. The investment did actually take place at $15 per share, but the $6 billion post-money number was based on a share count that included employee stock options, where the underlying share hadn’t actually been issued yet—whereas the market capitalization after the IPO didn’t include those shares. I’m charitably assuming that Square made this clear to you, otherwise you would have thought that the company was worth $18 per share when you joined, and been even more surprised at the IPO price. [↩︎](https://www.benkuhn.net/terms/#fnref:3)
    
4.  Plus some shares that are not currently outstanding, because they’re the underlying shares for stock options that haven’t been exercised yet. [↩︎](https://www.benkuhn.net/terms/#fnref:4)
    
5.  Note that these are how much the whole company is overvalued if you use the headline valuation, not how much its common stock is overvalued. G&S also estimate the latter, and it’s usually similar, sometimes higher and sometimes lower.[2](https://www.benkuhn.net/terms/#fn:2) [↩︎](https://www.benkuhn.net/terms/#fnref:5)
    
6.  You might worry that the company will become overvalued in the future. But to the extent that the founders make rational deals, this won’t actually _destroy_ value for you—it’ll just create the illusion that your equity has grown faster than it actually has, if you forget to correct for it. [↩︎](https://www.benkuhn.net/terms/#fnref:6)