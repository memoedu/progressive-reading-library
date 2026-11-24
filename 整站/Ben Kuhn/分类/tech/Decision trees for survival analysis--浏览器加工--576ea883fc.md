[Survival analysis](https://en.wikipedia.org/wiki/Survival_analysis) is an interesting problem in machine learning, but it doesn’t get nearly as much attention as the usual classification and regression tasks, so there aren’t as many tools for it. Here I describe a nifty reduction that allows us to bring more traditional machine-learning tools to bear on the problem. Combined with the view of decision trees as [greedy piecewise-constant loss-minimizing classifiers](https://www.benkuhn.net/tree-imp), it enables a number of powerful and flexible algorithms for large-scale discrete survival analysis.

Contents

-   [Setup: discrete survival analysis](https://www.benkuhn.net/survival-trees/#setup-discrete-survival-analysis)
-   [The hazard function](https://www.benkuhn.net/survival-trees/#the-hazard-function)
-   [A loss function](https://www.benkuhn.net/survival-trees/#a-loss-function)
-   [The trick](https://www.benkuhn.net/survival-trees/#the-trick)
-   [The punchline](https://www.benkuhn.net/survival-trees/#the-punchline)
-   [Appendix: pseudocode for the reduction](https://www.benkuhn.net/survival-trees/#appendix-pseudocode-for-the-reduction)

* * *

## Setup: discrete survival analysis

Discrete survival analysis is motivated by problems like disease prognosis. Suppose we’re trying to predict survival times from a rare diseas. We enroll a bunch of subjects who were just diagnosed, and we do check up on them monthly until they die. Then we try to predict the time until recurrence, `$Y$`, from data about the patient, `$X$`.

We could use standard regression algorithms, but there are a couple wrinkles:

-   We only know the recurrence times up to the month, because that’s how often we check up on them, so `$Y$` isn’t actually continuous. This is where the “discrete” part of the name comes from. (We could probably get the exact death date if we tried, but that’s too complex for us right now. Plus, it’ll make the analysis harder later, as we’ll see.)
    
-   `$Y$` is known to be greater than zero. Most regression algorithms don’t have a natural way to take this into account.
    
-   Most importantly, **we won’t be able to observe a recurrence for everyone.** Some people will be lost to follow-up. Some people will be cured. Even some of those who die might outlast the study period. This leads to a problem called _right-censored data_, where for certain patients we know that, say, `$Y > 60$` (months), but we don’t know the exact value of `$Y$`.
    

We could get arount the last one by discarding all data points that don’t experience a recurrence, but this would introduce severe selection bias. Plus, it would throw away a lot of info on healthy people. So instead we’ll do something more clever.

## The hazard function

If we were doing ordinary regression, where we had observed the `$Y$` for each `$X$`, we would try to learn a function `$f(X) = E(Y|X)$` (or sometimes some other property of the conditional distribution of `$Y$`, like the median). But we don’t have enough info about `$Y$` for that to work.

So instead of thinking in terms of `$E(Y|X)$`, we’ll instead think about something more granular called the _hazard function_. The hazard function is `$$h(X, t) = P(Y=t | Y \ge t, X),$$` —the probability that, if someone has survived up until month `$t$`, they will die in that month.

In fact, learning `$h$` will tell us _more_ about the disease than learning `$f$`, because it will give us not only the expected survival time but the entire distribution of survival times! You can verify for yourself (it’s not hard) that `$$P(Y = t|X) = h(X, t)\prod_{i=1}^{t-1} (1 - h(X, i)),$$` so we can recover the entire probability mass function.

## A loss function

So we want to learn some approximation `$\hat h(X, t) \approx h(X, t)$`. But how do we decide what a good approximation is? The obvious thing to try is maximizing the expected log-likelihood.

The other wrinkle is that we don’t actually see `$Y$`. Instead we see some related variables—how long each subject was in the study (`$T$`), and whether they exited by dying or being censored (`$D$`). (`$D$` is a Boolean variable, so `$D=1$` if `$T=Y$`, and `$D=0$` if `$T < Y$`.)

We could try to evaluate the log-likelihood of the exact data we see—that is, `$P(T, D|X)$`. But that would require us to build a model not only of the time of death but also the time of censorship (because we’d need the likelihood of getting censored at time `$T$`). Usually the censorship distribution is a property of the data-collection mechanism, not the underlying reality—for instance, in clinical studies it’s a property of the follow-up time. So a prediction of it probably won’t generalize well to other settings.

For that reason, instead of evaluating the likelihood of the entire data point `$(T, D)$`, we pretend that censorship just exogenously removes some information about `$Y$`. So we’ll split our likelihood into two pieces: `$P(Y=T)$` for uncensored data, and `$P(Y > T)$` for censored data. Given this, we can write down the log-likelihood of seeing a particular datum if `$\hat h$` were correct:

`$$L(T, D|X, h = \hat h) = \log \left[ \prod_{i=1}^{T-1}(1 - \hat h(X, i)) \right]\left[\hat h(X, T)^D (1 - \hat h(X, T))^{1-D}\right]$$`

where the first factor in the log is the probability that the patient survives until month `$T$`, and the second is `$\hat h(X, T) = P(Y=T|X)$` if `$D=1$` (the probability under `$\hat h$` that the patient died, if we observed them to die at `$T$`) and `$(1 - \hat h(X, T))$` if `$D=0$` (the probability under `$\hat h$` that the patient survived, if we observed them to survive at `$T$`, i.e., they exited by censoring).

Now that we know what the log-likelihood of a particular datum looks like, we can use our dataset to calculate the empirical expected log-likelihood of `$\hat h$` by taking the average log-likelihood over each data point. With some simplification, this becomes:

`$$L(\hat h) = \frac1N \sum_{i=1}^N \left[ D_i \log \hat h(X_i, T_i) + (1 - D_i) \log (1 - \hat h(X_i, T_i)) + \sum_{t=0}^{T_i-1} \log (1 - \hat h(X_i, T_i)) \right]$$`

## The trick

Of course, _finding_ the `$\hat h$` that maximizes this log-likelihood loss function is still a challenge. But we can make one key observation, which is that _if you treat `$T$` as another covariate, `$\hat h$` and `$L(\hat h)$` have the exact same structure as the classifier function and loss, `$\hat f$` and `$L(\hat f)$`, of a maximum-likelihood estimator for a classifier._ In other words, this problem is just classification-in-disguise! Each datapoint in the hidden classification problem is the combination of an `$X_i$` in our original dataset plus some month `$t$`, and the classification problem is “did point `$X_i$` die in month `$t$`”. We’ll call this new variable `$D_{it}$`, and rewrite our loss function in terms of it:

`$$L(\hat h) = \frac1N \sum_{i=1}^N \sum_{t=1}^{T_i} D_{it} \log \hat h(X_i, t) + (1 - D_{it}) \log (1 - \hat h(X_i, t))$$`

It’s now obvious that this is proportional to the classifier likelihood function I [wrote down earlier](https://www.benkuhn.net/tree-imp#information-gainlog-likelihood). This means that we can transform our original data set into a new one, with one row for each month that each `$X_i$` is in the sample; train a standard maximum-likelihood classifier on this new dataset with `$D_{it}$` as the target; and extract a perfectly good survival model for our original dataset!

## The punchline

This reduction will work with any classifier-training algorithm that tries to maximize the likelihood of the data. Combined with my earlier reframing of [decision trees as greedy piecewise-constant maximum-likelihood approximations](https://www.benkuhn.net/tree-imp), this means that decision trees will work with it—and by extension, any algorithm that uses decision trees as a building block, like like [random forests](https://en.wikipedia.org/wiki/Random_forest) or [gradient machines](https://en.wikipedia.org/wiki/Gradient_boosting). But you could also use the same transformation with [logistic regression](https://en.wikipedia.org/wiki/Logistic_regression) or any other maximum-likelihood method.

Some caveats need to be applied, though. With bagging-based methods like random forests or subsampled gradient trees, the default behaviour will be to bag the transformed dataset, but you probably want to make sure that each bag either contains _every_ month’s point for each `$X_i$`, or no points at all for that `$X_i$`. With logistic regression, you should make sure to include interaction terms between `$t$` and the other covariates; otherwise the hazard function will be forced to have the same general shape for every datapoint.

Once those caveats are taken care of, though, this reduction is extremely helpful for developing survival models quickly. Survival analysis doesn’t get as much attention as standard classification or regression tasks, so it’s convenient that so many more frequently-used models can be essentially reused wholesale.

## Appendix: pseudocode for the reduction

```
def train(X, T, D)
    // X, T, D are the original dataset
    X' = []
    D' = []

    // the transformation
    for each index i in X:
        for t=1 to T[i]:
            new_D = (0 if t < T[i], else D[i])
            append new_D to D'
            new_X = (X[i], t)
            append new_X to X'

    return a decision tree trained on (X', D')

def pmf(h, X)
    // X is a single datapoint
    // returns an array A where A[i] = P(Y = i | X)
    A = []
    p_so_far = 1 // this is p(T >= t | X)
    for t = 1 to (the last month where h has any data):
        // h knows p(T = t | T >= t, X), we call this p_cur
        p_cur = h's prediction for (X, t)
        append (p_so_far * p_cur) to A
        p_so_far *= (1 - p_cur)
```