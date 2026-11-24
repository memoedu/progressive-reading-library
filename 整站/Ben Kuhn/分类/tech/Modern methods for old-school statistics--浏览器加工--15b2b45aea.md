Almost a year ago, I started working full-time in machine learning after graduating from studying pure math in college.

My thinking when I decided to major in math was that it would be more challenging than an applied field like statistics or computer science. To maximize the benefit of each course I took, I reasoned, I should take the classes that I’d be least able to self-teach later—which meant picking the abstract stuff I needed explained to me, and trusting I’d pick up the applied knowledge on the job. This turned out to work, but it would have been a lot harder without Frank E. Harrell’s [_Regression Modeling Strategies_](http://smile.amazon.com/Regression-Modeling-Strategies-Applications-Statistics/dp/1441929185/).

It turns out that while some statistics work _is_ just learning the abstract machinery of inference, a lot of it is instead centered on building up intuitions that have less to do with hardcore math and more to do with tacit, hard-to-explain priors about how models interact with real-world data—what kind of procedures are likely to work, what kind of visualizations and model checks will be useful, and so on. As I got deeper into statistics for my day job, _RMS_ became an indispensible source to me for this second, “fuzzier” kind of info.

## What RMS is

At a basic level, _Regression Modeling Strategies_ is just what it says on the tin: instructions for how to build models of continuous dependent variables. (Harrell includes logistic regression and discrete-time survival analysis in this class of models.)

The book is organized into sections on a few different types of models: linear regression, logistic regression (both categorical and ordinal), parametric survival models, and Cox models. Together, these cover most of the basic designs an analyst is likely to come across.

There are also sections at the beginning on tools common to all models, like imputation, splines for nonlinearity, exploratory analysis, feature selection, bootstrapping, and checking model specifications.

The third important part of the book is a thorough set of case studies on various datasets. Harrell works at a medical school, so most of the datasets are from clinical studies, but they seem to transfer well.

## Old-school

To someone used to doing things “machine learning” way, _RMS_ might sound old-school. And it pretty much is. I think this is a good thing, since “machine learning” frequently refers to throwing your features into a [gradient boosting machine](https://www.benkuhn.net/gbm) and blindly trying different preprocessing steps until the performance goes up.

I get the sense that many people think of modern machine-learning methods as better for prediction because they make fewer assumptions, without realizing that there are well-studied ways to weaken the assumptions of standard statistical models. For instance, one can use [restricted cubic splines](http://onlinelibrary.wiley.com/store/10.1002/9781118032619.app3/asset/app3.pdf?v=1&t=i8106boa&s=8eff0fceffff0c83ad6a0bddfce76797c4bf6b3f) for allowing linear/logistic regression to pick up on nonlinearities (or an additive model, to relax the assumptions even more). Multiple times I’ve had a linear or logistic regression with these assumption-weakening techniques perform almost as well as the more modern assumption-free techniques.

Meanwhile, assumption-free machine learning methods often sacrifice a lot in interpretability. With weaker assumptions, the model class contains much more complex models, but that means it’s hard to understand what the models do! There are [some](http://en.wikipedia.org/wiki/Random_forest#Variable_importance) [tools](http://colah.github.io/posts/2015-01-Visualizing-Representations/) for this kind of thing, but overall the results tend to be unenlightening. And that’s not even to mention things like goodness-of-fit checks or diagnostic plots. (If your support vector machine isn’t performing well, good luck figuring out what went wrong!)

## Other goodies

Harrell is careful to call out many pitfalls of inexperienced statisticians when he gets the opportunity. He saves some special wrath for the process of stepwise feature selection—one chapter features a seven-point list of reasons why it’s a terrible idea—but he’s also careful to make constructive suggestions of alternate, more principled approaches to the techniques he proscribes.

Another strength of _RMS_ is the case studies, which occupy almost a third of the text. Statistics sometimes like to pretend that it’s a nice, clean, cut-and-dried field, but many decisions an analyst makes seem to depend a lot on tacit or procedural knowledge about what’s likely to work. To that end, it’s really helpful to be able to watch over Harrell’s shoulder as he analyzes a number of different datasets.

The book also features an accompanying R packages, `rms`, which implements many of the tips described in the book—most notably restricted cubic splines, and also a nicer interface for performing hypothesis tests in the presence of nonlinearities and interactions. (One should never test the significance of only the linear term without including nonlinearities and interactions, or test the significance of only one dimension of a spline basis—but this is what R’s default significance printouts do.) I frequently use Harrell’s forks instead of the standard R `lm`/`glm`/`coxph` simply because the printouts are nicer.

## Wishlist

There are a few kinds of models I wish _RMS_ covered that it didn’t: additive models, time series and hierarchical/multilevel data. It’s understandable, as those would have made the book substantially longer, but I’d be interested in seeing Harrell’s perspective on them.

For instance, some folks I respect, like Cosma Shalizi, seem to [suggest](http://www.stat.cmu.edu/~ryantibs/advmethods/shalizi/ch09.pdf) that “there are very few situations in which it is actually better to do linear regression than to fit an additive model,” and “Linear regression is \[often\] employed for no better reason than that users know how to type `lm` but not `gam`.” Is Frank Harrell simply one of these people, or does he have principled objections?

## Summary

If you have a bit of background, _Regression Modeling Strategies_ is a great book to take you from essentially zero knowledge of applied statistics to a working knowledge. Highly recommended for its excellent set of tools for careful statistical analysis.

_Edits 4/18/2016: finished a number of embarrassingly incomplete sentences._