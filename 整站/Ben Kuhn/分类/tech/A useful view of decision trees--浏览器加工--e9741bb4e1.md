## Introduction

When I took machine learning, one of the first learning algorithms we were taught was the [ID3](https://en.wikipedia.org/wiki/ID3_algorithm) algorithm for learning a _decision tree_ from a dataset. ID3 and related algorithms are easy to describe and implement, so they’re frequently taught early on in such courses. Unfortunately, they often end up being presented in an _ad hoc_, unmotivated way.

When I first learned about decision trees, I thought this _ad hoc_ presentation was just because the algorithm came more from the engineering side of machine learning than the theoretical one: it just worked, even if it wasn’t well-motivated by theory. But it actually turns out that there are fairly good theoretical ways to understand how ID3 and related algorithms came about. I’m writing it up here because I’ve seen it mentioned in remarkably few other places.

To understand this post, you should probably be familiar with [the basics of decision tree learning algorithms](http://www.cis.temple.edu/~giorgio/cis587/readings/id3-c45.html). It’ll also help if you know a bit about [maximum likelihood estimation](https://en.wikipedia.org/wiki/Maximum_likelihood) and the general idea of [learning by optimizing a loss function](http://vserver1.cscs.lsa.umich.edu/~crshalizi/weblog/1017.html).

## Summary

The particularly ad-hoc part of most introductions to decision tree learning is the tree’s splitting criterion. Why does it turn out that picking the split with the best [information gain](https://en.wikipedia.org/wiki/ID3_algorithm#Information_Gain) produces a good classifier? It’s sort of intuitive that this should work, but why not go with something else like the sum of minority class frequencies?

It turns out that most popular splitting criteria can be derived from thinking of decision trees as _greedily learning a piecewise-constant, expected-loss-minimizing approximation_ to the function `$f(X) = P(Y=1 | X)$`. For instance, the split that maximizes information gain is also the split that produces the piecewise-constant model that maximizes the expected log-likelihood of the data. Similarly, the split that minimizes the [Gini node impurity criterion](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity) is the one that minimizes the [Brier score](https://en.wikipedia.org/wiki/Brier_score) of the resulting model. [Variance reduction](https://en.wikipedia.org/wiki/Decision_tree_learning#Variance_reduction) corresponds to the model that minimizes mean squared error.

Here’s how it all works.

## Setup

Let `$X$` be a denote a data point living in some space `$S \subset \mathbb{R}^p$`, and let `$Y \in \{0, 1\}$` be its classification. Suppose `$Y$` is determined from `$X$` by some function `$f(X) = P(Y=1 | X)$`. You can think of the decision tree algorithm as greedily learning a piecewise-constant approximation to the function `$f$` (call it `$\hat f$`), as follows:

Start out with a totally constant function. Then you search through all features `$i$` and all thresholds `$t$`, and split `$S$` into the pieces `$S_- = \{s \in S: s_i < t\}, S_+ = \{s \in S : s_i \ge t\}$`, and evaluate how good a two-piece piecewise constant approximation with pieces `$S_+, S_-$` would be. Finally, you pick the best such `$i, t$` and recurse on the corresponding `$S_+, S_-$` (or quit if no split produces an approximation that’s sufficiently better than the one-piece approximation you started with). Finally, you stich all these pieces together into a global piecewise-constant approximation.

I’ve glossed over how you evaluate “how good a split is,” but that’s where the loss functions come in. In the traditional view of decision trees, you would evaluate a how good a split is by comparing the weighted average of the “impurity” of each leaf node to the “impurity” of a parent node. But talking about “impurity measures” is really just code for “how well the best constant function estimates the data distribution,” where this is measured by the expected value of some well-known loss function. (The expected loss of a piecewise-constant model is just the average of the loss on each constant piece, weighted by how many data points are in that piece.)

Below, I go over some examples showing how certain splitting criteria maximize the corresponding loss functions.

## Information-gain/log-likelihood

If your splitting criterion is information gain, this corresponds to a log-likelihood loss function. This works as follows.

If you have a constant approximation `$\hat f$` to `$f$` on some regions `$S$`, then the approximation that maximizes the _expected log-likelihood_ of the data (that is, the probability of seeing the data if your approximation is correct) is `$$L(\hat f) = E(\log P(Y=Y_{observed} | X, f = \hat f)) = \sum_{X_i} Y_i \log \hat f(X_i) + (1 - Y_i) \log (1 - \hat f(X_i))$$`

(where the thing inside the sigma is a fancy way of saying “`$\log \hat f(X_i)$` if `$Y_i$` is 1, or `$\log (1 - \hat f(X_i))$` if `$Y_i$` is zero).”

First we need to find the constant value `$\hat f(X) = f$` that maximizes this value. Intuitively, you want to say that the _predicted_ probability of a data point being positive is just the _observed frequency_ of positive instances in your data set. And indeed that’s what you get; here’s the proof:

Suppose that you have `$n$` total instances, `$p$` of them positive (`$Y = 1$`) and the rest negative. Suppose that you predict some arbitrary probability `$f$`—we’ll solve for the one that maximizes expected log-likelihood. So we take the expected log-likelihood `$E_X(\log P(Y=Y_{observed} | X))$`, and break up the expectation by the value of `$Y_{observed}$`:

`$$L(\hat f) = (\log P(Y=Y_{observed} | X, Y_{observed}=1)) P(Y_{observed} = 1) \\ + (\log P(Y=Y_{observed} | X, Y_{observed}=0)) P(Y_{observed} = 0)$$`

Substituting in some variables gives

`$$L(\hat f) = \frac{p}{n} \log f + \frac{n - p}{n} \log (1 - f)$$`

Setting `$\frac{\partial L(\hat f)}{\partial f} = 0$` gives

`$$0 = \frac{p}{nf} - \frac{n - p}{n(1-f)}$$`

to which the solution is `$f = \frac{p}{n}$`, exactly as we were hoping.

Let’s substitute this back into the likelihood formula and shuffle some variables around:

`$$L(\hat f) = \left(f \log f + (1 - f) \log (1 - f) \right)$$`

Something should look suspicious about this equation. Indeed, it’s the same as the ID3 algorithm’s [entropy](https://en.wikipedia.org/wiki/ID3_algorithm#Entropy) equation!

## Gini impurity/Brier score

A similar derivation shows that Gini impurity corresponds to a Brier score loss function. The Brier score for a candidate model `$\hat f$` is

`$$B(\hat f) = E((Y - \hat f(X))^2)$$`

(In this case, we want to minimize, not maximize—the Brier score _decreases_ as `$\hat f(X) \to Y$`.)

Like log-likelihood, the predictions that `$\hat f$` should make to minimize the Brier score are simply `$f = p/n$` (I won’t prove it this time since it’s basically the same manipulations as above). Now let’s take the expected Brier score and break up by `$Y$`, like we did before:

$$B(\\hat f) = (1 - \\hat f(X))^2 P(Y\_{observed} = 1)

-   \\hat f(X)^2 P(Y\_{observed} = 0)$$

Plugging in some values:

`$$B(\hat f) = (1 - f)^2 f + f^2(1 - f) = f(1-f)$$`

which is exactly (proportional to) the [Gini impurity](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity) in the 2-class setting. (A similar result holds for multiclass learning as well.)

## Variance reduction/mean squared error

Variance reduction is an impurity criterion used by regression trees, which learn a piecewise constant prediction for `$Y|X$`. This one can be proven purely verbally: the constant estimator for `$Y$` that minimizes the squared error is just the mean of `$Y$`, and the squared error is exactly the variance of `$Y$`, so minimizing the variance corresponds to minimizing the squared error of the prediction.

## Absolute error

This leads to a potentially useful modification of decision trees that I don’t see mentioned much. If you want them to be more robust to outliers, you can try to predict the _median_ of `$Y|X$` rather than the mean. But what impurity criterion to use?

Since the median is just the estimator for `$Y$` that minimizes the absolute value of the difference: `$E(|Y - m|)$`, you should choose the split which minimizes the [absolute deviation](https://en.wikipedia.org/wiki/Absolute_deviation) rather than variance. You’ll also want to predict the median, rather than the mean, in each leaf node.

## Conclusion

Clearly the connection between these impurity criteria and scoring rules is not an accident; the correspondence is too good. Yet this view of decision trees doesn’t seem to be discussed often, if at all. I find this puzzling, since as I’ve shown with absolute error above, thinking of decision trees in this way yields useful insights if you want to optimize a different loss function from the usual one.

Although I haven’t tried it, you should be able to do similar things for more exotic loss functions, too. All you need is to be able to optimize and evaluate the loss function in question on a constant model. You might even be able to do this in more complex settings (perhaps [survival analysis](https://en.wikipedia.org/wiki/Survival_analysis) or [learning to rank](https://en.wikipedia.org/wiki/Learning_to_rank)), if you have a suitable analog of the “constant model” in that case.

_Thanks to Bernardo Garcia del Río for pointing out some broken equations._