Someone recently asked me what resources I used to learn statistics.

The answer to this question may not be the most helpful, because I took a pretty idiosyncratic route: I did a lot of pure math plus introductory theoretical statistics, took one course on machine learning, and then got an internship in statistics which later turned into a full-time job. So I don’t make any claim that these resources are a good fit for basically anyone. Nevertheless, I do like them a lot, and perhaps you’ll find one or another that suits you.

Contents

-   [Courses](https://www.benkuhn.net/stats-material/#courses)
    -   [Harvard Statistics 110](https://www.benkuhn.net/stats-material/#harvard-statistics-110)
-   [Textbooks](https://www.benkuhn.net/stats-material/#textbooks)
    -   [Applied Predictive Modeling](https://www.benkuhn.net/stats-material/#applied-predictive-modeling)
    -   [Regression Modeling Strategies](https://www.benkuhn.net/stats-material/#regression-modeling-strategies)
    -   [Advanced Data Analysis from an Elementary Perspective](https://www.benkuhn.net/stats-material/#advanced-data-analysis-from-an-elementary-perspective)
-   [Other resources](https://www.benkuhn.net/stats-material/#other-resources)
    -   [Cross Validated](https://www.benkuhn.net/stats-material/#cross-validated)
    -   [GiveWell reports](https://www.benkuhn.net/stats-material/#givewell-reports)

* * *

## Courses

### Harvard Statistics 110

This is the only statistics course I took at Harvard, and it’s [available online](http://projects.iq.harvard.edu/stat110/home). Its main selling point is Professor Blitzstein’s excellent teaching choices. You won’t learn much about hypothesis testing here, but you _will_ learn enough about “the physics of numbers” that you’ll be able to figure all that stuff out yourself if you need to.

I was able to get away without attending lecture, because I had a stronger background in calculus and combinatorics than the professor asked for. Occasionally watching lecture videos and doing all of the homeworks turned out to be enough to get most of the value. If you have less of a math background, you should probably watch more of the videos.

Blitzstein’s notes have recently been collected into [a textbook](http://www.crcpress.com/product/isbn/9781466575578) by one of Blitzstein’s undergraduate teaching assistants, Jessica Hwang, for her thesis. I haven’t read it, but I’m sure it has many of the same characteristics as the course lectures.

## Textbooks

### Applied Predictive Modeling

Theorem’s CEO refers to [this book](http://www.amazon.com/Applied-Predictive-Modeling-Max-Kuhn/dp/1461468485/) as “the Bible,” and with good reason. It goes over almost all commonly-used machine learning algorithms for regression and classification. The book also gives excellent exposition of issues that aren’t about specific models _per se_, like data cleaning, overfitting and validation, feature selection, class imbalance, and so on. They also have a few case studies that help you get a feel for how this stuff works in the real world.

I haven’t read this book thoroughly, but it’s an excellent reference when I need to get started on a model or process that I haven’t used before.

### Regression Modeling Strategies

Where _Applied Predictive Modeling_ is written with a more modern “machine learning” attitude, Frank Harrell’s [_Regression Modeling Strategies_](http://www.amazon.com/Regression-Modeling-Strategies-Applications-Statistics/dp/1441929185/) leans more towards old-school statistics.

These get a bad rap among ML practitioners for making too many assumptions and having sub-optimal predictive accuracy. But _RMS_ mostly convinced me that this critique applies mostly to careless and superficial modeling. Harrell goes over a number of tools for weakening the strong assumptions of old-school statistical methods, complete with thorough case studies on real-world data (mostly biomedical). When I’m looking for a model that performs almost as well as a high-end assumption-light model, with all the interpretability and theory of a standard linear model, I reach for _RMS_.

### Advanced Data Analysis from an Elementary Perspective

These lecture notes, prepared by statistics legend [Cosma Shalizi](http://bactra.org/), cover a huge variety of statistical topics with Shalizi’s trademark clarity and wit. I got the most out of the chapters on (generalized) additive models; the bootstrap; and relative distributions and smooth tests, none of which were covered adequately in the books above. I haven’t read nearly as much of these as I want to, so hopefully I’ll be able to move them up the list later.

## Other resources

### Cross Validated

This is the Stack Exchange site for statistics. I’ve gotten a surprising amount of mileage out of looking up [the top users](http://stats.stackexchange.com/users) and reading their highest-voted answers, which frequently contain useful nuggets of statistical insight. Some of their [posts tagged “big list”](http://stats.stackexchange.com/questions/tagged/big-list) are also pretty great, like the [list of statistical sins](http://stats.stackexchange.com/questions/4551/what-are-common-statistical-sins).

### GiveWell reports

Most of the books I’ve described above are geared towards using technical or quantitative analysis to produce new insight from data. But some of the most important statistical work consists of synthesizing data that’s _already_ available into some sort of coherent picture of the world. This is the kind of work I did, for instance, in my literature review of [donation matching](https://www.benkuhn.net/matching).

This is the kind of work I did during my internship at [GiveWell](http://www.givewell.org/), and their intervention reports are shining examples of the level of rigor and scrutiny it’s possible to bring to this kind of messy synthesis problem. I highly recommend digging into, for example, their [report on deworming](http://www.givewell.org/international/technical/programs/deworming) to get a sense of the kind of analysis you can do.

Of course, you shouldn’t necessarily aspire to be as thorough as GiveWell, since that would take an inordinate amount of time. But it’s good to keep in your head as the gold standard while you try and compute some tractable approximation.