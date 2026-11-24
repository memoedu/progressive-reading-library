(Attention conservation notice: only useful if you have a static site that you’d like to remove the Javascript from. I don’t normally find “how to do X with Blub and Glug” tech posts very useful, but this one took me a while to figure out how to do simply, and I’m hoping it will help other folks with static sites eliminate their Google Analytics dependency.)

I have the following website problem:

1.  I’d like some basic analytics so I can tell how many people read an article.
2.  I’d like to keep the client side of this site as simple as possible for people with [bad networks](https://danluu.com/web-bloat/), so I’d rather avoid client-side analytics e.g. Google Analytics.
3.  I want to keep using a CDN to serve the site with minimal latency. (If you have a static site and don’t do this, I recommend it–it’s quite easy to set up and you usually get automatic cert management for free.)

Unfortunately, most server-side analytics products for static sites are focused on the true “CDN” use case—answering questions like “which of my objects are costing me the most money?” I tried out a few, but I kept wanting to ask just slightly more complicated questions (e.g.: “which of my pages have the most hits _excluding bot traffic_?”) that they didn’t let me answer.

Eventually I concluded that if I wanted to use a CDN, I’d need to do at least a bit of custom work to support the analytics I wanted. Effectively, I’d need to stick the logs in a database somewhere and then query it. But I was reluctant to commit to anything because it seemed like I’d have to build an [ETL pipeline](https://en.wikipedia.org/wiki/Extract,_transform,_load) myself, and that was a lot of moving parts for grunt work.

I searched around for a while before realizing that one of AWS’s many, many database offerings is already really well-suited to this: [Athena](https://aws.amazon.com/athena/). Athena lets you query “databases” that are made up of many CSV files inside an S3 bucket. Even better, AWS also has a built-in tool for creating dashboards from various data sources including Athena, called [Quicksight](https://aws.amazon.com/quicksight/).

I realized this would let me do the whole thing without running any infrastructure myself: I could create an Athena “table” directly from the Cloudfront logs that AWS was already archiving for me, then create a Quicksight dashboard that queried the table, and I’d be in business.

AWS already has [instructions](https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html) for setting up Athena to query Cloudfront, so it ended up being about a 1-hour project to build the following dashboard:

[![](https://www.benkuhn.net/analytics/dashboard_hu38ccff6c98e7b5d64129d9e1d80e93a2_445537_1200x814_resize_box_3.png)](https://www.benkuhn.net/analytics/dashboard.png "full size")

The steps were:

1.  Create the Athena table, per the instructions above.
2.  I created a derived “view” that included some extra columns that were useful for analytics, like `is_bot` and `is_internal_referrer`. (You can [read the SQL](https://gist.github.com/benkuhn/2566af1568b67f4b2d4edb8637660f48) if curious.)
3.  I moved my old Cloudfront logs to a different directory that wasn’t included in the Athena table, because I had 3+ years of logs and it was making my queries super slow.
4.  I set up Quicksight and built some charts.

I’m pretty happy with this setup—I have a dashboard for my common queries, and if I want to do any sort of complicated analysis, I can write ad hoc SQL to do it myself. Quicksight seemed buggy enough that I wouldn’t use it for anything serious, but it’s fine for my purposes and doesn’t cost anything for personal use. (For professional use, at Wave we use Periscope Data which is a better but pricier version of the same thing, and it’s been relatively great.)

One minor downside is that the dashboard takes several seconds to load due to the Athena queries being slow. I don’t mind this because I only look at it a couple times a week. If I cared a lot, there’s some stuff I could do with [partitioning](https://medium.com/compass-true-north/fast-cloudfront-log-queries-using-aws-athena-and-serverless-ef117393c5a6) to make it faster at the expense of adding another moving part (a Lambda function to rename the Cloudfront log dumps).

Alternatives considered:

-   [S3stat](https://www.s3stat.com/): didn’t allow the type of detailed queries I wanted.
-   [Netlify Analytics](https://www.netlify.com/products/analytics/): I got so excited when it was released that I switched to Netlify, but it turned out to be useless—it gave the top ~10 referrers/sites and basically nothing else. It was also down for about 10 days with no communication at one point.
-   [GoAccess](https://goaccess.io/): Would have required a manual ETL step.
-   [Matomo](https://matomo.org/log-analytics/): Would have required a manual ETL step and a MySQL instance.
-   [Google Cloud CDN](https://cloud.google.com/cdn) dumping logs into [BigQuery](https://cloud.google.com/bigquery): this would probably have been just as good, and Cloud CDN seems like it performs better than CloudFront (and supports QUIC), but Google makes you pay for a load balancer if you want to use it, which would have doubled my hosting costs or more (it’s $18/mo).
-   I’m sure Azure has an equivalent setup but I’ve never used Azure so I didn’t look into it in depth.
-   [Cloudflare Analytics](https://www.cloudflare.com/analytics/): Looked mostly intended for monitoring, not user-focused stats. Also the Cloudflare free plan requires them to have DNS authority for your whole domain and I didn’t want to switch nameservers again.