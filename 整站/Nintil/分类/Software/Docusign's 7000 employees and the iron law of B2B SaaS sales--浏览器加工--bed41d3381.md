Docusign has over 7000 employees. Someone on the internet periodically discovers this fact and wonder how could this be! What are those 7000 employees doing?

![image-20240317140100243](https://nintil.com/images/2024-03-17-docusign/image-20240317140100243.png)

![image-20240317140119707](https://nintil.com/images/2024-03-17-docusign/image-20240317140119707.png)

![image-20240317140137482](https://nintil.com/images/2024-03-17-docusign/image-20240317140137482.png)

The first part to the "What are they doing" question is "Sales", but that's not the full answer. Per their latest [10K](https://s22.q4cdn.com/408980645/files/doc_financials/2023/ar/docusign-inc-_annual-report_form-10-k.pdf) filing,

-   68% of their workforce is sales, marketing, and customer success so that's ~5000 of those
-   And then 1760 of the workforce, a 24% is engineering, product development and customer operations

Leaving sales aside, we can still ask: Do you need 1760 engineers to run this product?

Remember that in the heyday of Twitter, the whole company was 7500 people and post-Elon takeover it went down to ~1500. Twitter continues running and shipping features. Could Docusign run and do the same things it does with ~1500 people? ~100 people?

To me it's obvious that a site like Twitter is more complex than Docusign, but a fairer comparison is Docusign vs its alternatives.

Hellosign was acquired by Dropbox but before that they had in the order of low hundred employees \[[1](https://getlatka.com/companies/hellosign/team),[2](https://rocketreach.co/hellosign-profile_b5c8924af42e3552)\]. We don't know how many employees they have now but we do know that Dropbox itself, a superset of Hellosign, has ~2700 employees. I couldn't find data of Dropbox broken down by engineering vs sales by headcount but I did find data on relative spend:

-   At [Dropbox](https://dropbox.gcs-web.com/static-files/eaffdb7c-727a-40c6-9308-1c1c062d2b30), for every dollar spent on R&D (ie engineering) they spend 50 cents on sales
-   For [Docusign](https://investor.docusign.com/investors/financial-information/financial-reports/default.aspx), that's 2.58 dollars

How unusual is this? What about say, another company with a large salesforce like... Salesforce? What sort of company is Salesforce? A much bigger one: They have around 80,000 employees. For them, their sales to R&D ratio is as Docusign's, 2.67 dollars of sales spend per engineering dollar.

Another company that came to mind is [PagerDuty](https://www.pagerduty.com/). It is also [publicly](https://d18rn0p25nwr6d.cloudfront.net/CIK-0001568100/d17cc89a-ffc7-460f-a47d-08e1b5393dfa.pdf) traded and they also build SaaS software. They have ~1000 employees and also spend a nontrivial amount on sales: 1.4 dollars per dollars of R&D.

So in terms of the sales/engineering split, Docusign is probably not that unusual. I don't have a good model of how big sales should be; it's easier for me to reason about how many engineers, PMs, designers and so forth one should have to build the core product the business is selling. So the question then becomes: Is it reasonable for Docusign to have 1760 people building the product?

Consider yet another company that builds a product that, I'd argue is more complex than Docusign: Zoom. [Zoom](https://www.businesstoday.in/technology/news/story/zoom-lays-off-1300-employees-about-15-per-cent-of-its-workforce-ceo-takes-98-per-cent-pay-cut-369427-2023-02-08) has 6800 employees (less than Docusign!). Is that because of sales? Perhaps! Zoom's sales/R&D ratio is 1.91. How about headcount? Hard to tell from public data!

[Here](https://tomtunguz.com/saas-spend-allocation-benchmarks/#:%7E:text=When%20a%20company%20is%20founded,the%20life%20of%20the%20company.) Tomasz Tunguz, a VC, looked at this ratio back in 2013 and found that indeed expenses on sales and marketing tend to be ~2x that of engineering, almost regardless of how long the company has existed. Headcount, he argues, follows a similar story. Docusign fits this pattern reasonably well. Another interesting datapoint is that half of the revenue in SaaS goes to sales and marketing, with 25% for R&D.

That still leaves us with the question: Why 2:1, and more importantly what should the base numbers be? Why not 100 engineers or 1000?

[Here](https://www.saastr.com/what-your-first-100-hires-will-look-like/) they try to do the accounting of how many people are needed for a ~100 headcount company that does 10M$ ARR in a somewhat structured way, coming to a staff of 70 marketing+sales+CSM. Scale that times 70 to get to Docusign's 700M$ ARR and you get 70\*70=4900 people, roughly what Docusign has.

Intriguing.

Suppose you model sales as a lottery, where every sales call has some chance of progressing through the funnel, and eventually to a customer signing up. Then one could have a model where to get X dollars of revenue one needs Y hours of salespeople work, and translate that to people working regular hours, given a low enough chance of a sale, and that leads to needing a large sales staff.

This model could explain why, if a company believes it has a potential to make in revenue, it needs a staff of 7 people per $M to handle sales. But it doesn't explain why engineering count is what it is. One could imagine, for the sake of the argument, a team of 2 very capable engineers building a great product and a team of 1000 salespeople selling it all over the world.

Why is engineering headcount half of sales instead of much lower given how software scales?

A fun exercise: Suppose you have 7 sales people. Per the model earlier, the company has $1M in revenue. Half of that (per the averages earlier, but also with some guesses about salaries) will be their salaries. Assuming 2 engineers paid $160k (it's a startup) that's already $820k. Add the CEO's salary ($100k say) plus rent and infrastructure and we get to the revenue being fully utilized. The number of sales people sets a maximum on how many engineers you can have essentially, and that number might end up being 2 engineers per sale s person at the going salaries.

Great. So if you are a startup that has some extra cash to spare, what are you going to do? You could just keep the cash and enjoy your earnings, or you could try to make more revenue hiring even more sales people. Or you could hire more engineers. You could be fearing competition, driving a need to throw as many resources as possible to building the product as possible; so if this hypothetical company makes another $1M, they will be hiring in the same ratio to make it self-sustaining. If they did not do this, and were able to grow revenues without salespeople, or if they have a really efficient engineering team, then this multiple should change. But in a competitive market, if there are extra profits, these must be reinvested into the business, and that reinvestment means a ratio of 2 engineers and 1 extra sales person.

This seems true across the board in this [list](https://blossomstreetventures.medium.com/saas-spend-ratios-on-r-d-s-m-g-a-1a0b30931b0) of SaaS companies from 2022. The only companies that have more than 10 percentage points higher spend in R&D than sales+marketing are Dropox, Unity Software, Olo, DigitalOcean, Alkami.

Dropbox, Unity, and DigitalOcean I have heard about and I could imagine how, through word of mouth and a B2C segment, they can spend less on sales.

What about Spotify, a company that's chiefly B2C? Spotify seems to have over 9000 employees(!) which is more than I thought. Out of these, 4719 are in R&D, 633 in content production and customer service, and 2403 in sales and marketing. Even pooling these latter two together doesn't get us to over one salesperson per engineer, confirming what we'd expect for a B2C company. But surprisingly, their budgets are more even, 1725M€ for R&D and 1533M€ for sales and marketing, similar to B2B SaaS. Maybe here it's because instead of having salespeople calling people (which would raise headcount), Spotify relies more on paid billboards, social media, and ads. This is the same as with B2B SaaS: a dollar spent in such ads should bring some dollars in revenue that can support a number of engineers.

At this point my explation for why these companies have the headcount they have:

1.  Market size dictates sales force size and annual revenue
2.  Annual revenue, minus sales costs, plus competitive pressures dictates engineering spend and through it headcount

There's one more missing factor: The belief that the company is as productive as other companies or something like that. ie if you think you could build the same features your competitors can with 10x fewer engineers then you can hire less and profit more. Argueably this is now the situation with post-Elon Twitter if advertisers return. But most companies probably think they are normal. Perhaps, as companies scale, they have to be normal and as productive as any other company. But this is copium: Tesla (look at their net profit per car) and SpaceX (launches per employee?) show that this is simply not true.

One last idea I want to add to this post is that even if a company is well positioned in its space, its future and fat profits are always uncertain. Therefore, putting some resources into new products or features, just in case, makes sense. The cost of being overtaken is very high, an existential risk indeed, whereas slightly reduced profits is a minor trouble. Those extra thousand engineers are maybe there for that reason.