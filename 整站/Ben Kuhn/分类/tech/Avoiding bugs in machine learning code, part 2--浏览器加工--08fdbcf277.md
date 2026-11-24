In [a previous post](http://www.benkuhn.net/ml-bugs) I explaind how hard it is to prevent, or find and fix, bugs in machine learning code via conventional strategies. In this installment, I’ll go over some strategies that _do_ work.

Over the past few months I’ve been tracking what kind of bugs I’ve run into and done some thinking about how they can be avoided. You can see the results below, organized by category of bug followed by a list of potential solutions.

Contents

-   [Switching `true` and `false`](https://www.benkuhn.net/ml-bugs-2/#switching-true-and-false)
    -   [Unit testing](https://www.benkuhn.net/ml-bugs-2/#unit-testing)
    -   [Keep to a convention](https://www.benkuhn.net/ml-bugs-2/#keep-to-a-convention)
-   [Switching variables of the same type](https://www.benkuhn.net/ml-bugs-2/#switching-variables-of-the-same-type)
    -   [Name your variables distinct things](https://www.benkuhn.net/ml-bugs-2/#name-your-variables-distinct-things)
    -   [Keyword arguments](https://www.benkuhn.net/ml-bugs-2/#keyword-arguments)
    -   [Richer type system](https://www.benkuhn.net/ml-bugs-2/#richer-type-system)
    -   [Hungarian notation](https://www.benkuhn.net/ml-bugs-2/#hungarian-notation)
-   [Unclear semantics](https://www.benkuhn.net/ml-bugs-2/#unclear-semantics)
    -   [Have less magic](https://www.benkuhn.net/ml-bugs-2/#have-less-magic)
    -   [Semantic cues in function names](https://www.benkuhn.net/ml-bugs-2/#semantic-cues-in-function-names)
-   [Writing down math wrong](https://www.benkuhn.net/ml-bugs-2/#writing-down-math-wrong)
    -   [Careful code review](https://www.benkuhn.net/ml-bugs-2/#careful-code-review)
    -   [Dimensional analysis](https://www.benkuhn.net/ml-bugs-2/#dimensional-analysis)
    -   [Automatic differentiation](https://www.benkuhn.net/ml-bugs-2/#automatic-differentiation)
-   [Using different estimators interchangeably](https://www.benkuhn.net/ml-bugs-2/#using-different-estimators-interchangeably)

* * *

## Switching `true` and `false`

One company at whose offices I worked briefly had the following diff taped to their wall:

```
+ if not is_credit_too_risky(user):
- if is_credit_too_risky(user):
      return rejection_notice(user)
  # issue a loan to the user
```

This is not very good for business. Unfortunately, Boolean conditionals are one area where an extra `not` can often slip past the type system, but also where using the wrong value can be extra-dangerous since it will take you down a whole different branch of code.

Potential solutions:

### Unit testing

This one is obvious (and hyped) enough that I probably don’t need to write too much about it. As I mentioned before, it can be hard to unit-test machine learning code—but the stuff above isn’t machine learning, it’s business logic. If you can separate your code’s concerns effectively so that the logic is testable, unit testing can still help.

### Keep to a convention

One reason the above code is error-prone: `is_credit_too_risky` breaks the usual pattern that `True` is good or desired or has positive emotional valence. Keeping your codebase to a convention like this might make incorrect logic stand out more.

## Switching variables of the same type

More than once in my [ipython](http://ipython.org/) session I’ve trained one model on our training set, and then asked it to predict some different test data to evaluate its performance—only to accidentally use another model for the evaluation instead.

Similarly, in various functions that compute metrics about our models, I’ve sometimes called `score(predicted, actual)` instead of `score(actual, predicted)`. This switches false negative errors, which we don’t care much about, with false positive errors, which we care about a lot—so basically it makes all our good models look bad and vice-versa. Yikes!

Solutions:

### Name your variables distinct things

Naming things `gradient_booster` and `extra_random_forest` and `logistic_regressor` is more verbose than `model1`, `model2`, `model3`… but on the other hand, it makes typos WAY more obvious. I’d go so far as to say that you should probably never have two variables whose names are the same except for a number—it’s just too easy to confuse them, because the numbers don’t have any meaning attached to help our brain parse the code.

Similarly, in `scikit-learn` classifiers, the usual names for parameters to the `fit()` method (which trains a model on some data) are `X` and `y`. This is a dumb remnant of conventions for mathematical notation that were designed in the 1700s in a setting that was far more general (“arbitrary things involving linear equations”) than classification or regression. Much better would be e.g. `data` and `classes`.

### Keyword arguments

Naming variables more different things won’t help with the problem of switching method arguments, though. A solution (in some languages) is to make potentially ambiguous parameters into keyword arguments. Instead of just `score(isRepaid, model.predict(data))`, you can write `score(actual=isRepaid, predicted=model.predict(data))`, making any transpositions much more obvious.[1](https://www.benkuhn.net/ml-bugs-2/#fn:1)

### Richer type system

While working at Jane Street, I dealt with some code that calculated improper integrals by applying a [u-substitution](http://en.wikipedia.org/wiki/Integration_by_substitution) to the integrand (to convert the integral to a proper one) and then using e.g. Simpson’s Rule. We wanted to make sure that we never used an un-substituted integrand where a substituted one was needed or vice-versa.

To accomplish this, we took advantage of the fact that in OCaml, _functions with differently-named keyword arguments have different types._ Our un-substituted functions had the type `x:float -> float`, while substituted functions had the type `u:float -> float`. So it became impossible to confuse them. With a rich enough type system, all manner of such trickery is possible: you could similarly have different types for “tainted” strings (strings that are user-supplied).[2](https://www.benkuhn.net/ml-bugs-2/#fn:2)

### Hungarian notation

If your language doesn’t have a rich enough type system, you can use the poor man’s version, Hungarian notation. The idea is that you store extra type information in variable names—for example, if you can’t make a separate type for unsafe strings, you would prefix every unsafe string value with `us`, so that it’s more noticeable that a query like `SELECT * FROM users WHERE name = $usName` has an injection vulnerability.

Joel on Software has an [excellent article](http://www.joelonsoftware.com/articles/Wrong.html) about the motivation and practice of Hungarian notation.[3](https://www.benkuhn.net/ml-bugs-2/#fn:3)

## Unclear semantics

In [Pandas](http://pandas.pydata.org/), you can add a column to a `DataFrame` like so:

```
In [1]: df = DataFrame({'foo': 1,2,3}); df
Out[1]:
   foo
0    1
1    2
2    3
```

You can assign to a subset of the column using a `Series` mask as an index:

```
In [2]: ix = Series([True, False, True]); df['foo'][ix] = [4, 5]; df
Out[2]:
   foo
0    4
1    2
2    5
```

You can also take a subset of the whole dataframe, not just a single column:

```
In [3]: df[ix]
Out[3]:
   foo
0    4
2    5
```

But if you try and assign to a column of the subset (rather than a subset of the column)…

```
In [4]: df[ix]['foo'] = [6, 7]`
Out[4]:
   foo
0    4
1    2
2    5
```

it won’t change the original dataframe.[4](https://www.benkuhn.net/ml-bugs-2/#fn:4)

This strange semantic issue caused half of our data-preprocessing code not to run in some circumstances. Fortunately we caught it before it affected anything in production.

Solutions:

### Have less magic

This gotcha comes from the Pandas library’s goal to have a “do what I mean” syntax and semantics. When you index a `DataFrame` using square brackets, pandas automagically decides what you mean based on the type of the thing in the square brackets. That is, indexing has a _totally different meaning_ depending on whether the indexer is:

-   a slice
-   a single `int`
-   a `list` containing a single `int`
-   a `tuple` containing a single `int`
-   a `string`
-   a `Series` object

Which seems reasonable, because you should be able to index a `DataFrame` in all of these different ways. You just shouldn’t use the same syntax for all of it. Otherwise some poor programmer is going to write a function that indexes a `DataFrame` with one of its arguments, and when it silently fails six months later because someone called `fn([0, 1])` instead of `fn([foo, bar])`, nobody will be able to figure out why.

The result is that Pandas is great for exploratory analysis, but not at all robust to errors of this kind. For code that goes into production, it might be a good idea to avoid magical “do-what-I-mean” functions.

### Semantic cues in function names

The Julia language has the following policy:

> By convention, function names ending with an exclamation point (`!`) modify their arguments. Some functions have both modifying (e.g., `sort!`) and non-modifying (`sort`) versions. [Julia standard library documentation](http://julia.readthedocs.org/en/latest/stdlib/base/)

This totally eliminates most confusion about when updates are or aren’t destructive. Languages without exclamation points sometimes try to do a similar thing by having destructive functions return `None`, but various libraries trample over that in an attempt to build [fluent interfaces](http://en.wikipedia.org/wiki/Fluent_interface).

## Writing down math wrong

One thing that I do sometimes is implement a custom [loss function](http://en.wikipedia.org/wiki/Loss_function) for a model I’m fitting. As you might be able to tell from the Wiki article, loss functions can be pretty mathematically thorny. And sometimes you have to take derivatives and then everything gets twice as bad.

Mathematical expressions are extra-easy to make bugs in, because many typos (e.g. misplaced parentheses or mistyped arithmetic operators) produce valid code. Plus, the bugs are extra-hard to detect and debug, since sometimes your program will appear to work perfectly fine. If your loss function is only approximately correct, your machine-learning algorithm won’t crash—it’ll just perform slightly worse than it would otherwise, and there may be no way for you to know that something is holding it back.

As such, it’s extra-important to have other processes for catching bugs from mis-specified math. Some potential solutions:

### Careful code review

I’m usually allergic to hoping that bugs get caught in code review. It’s a great backup against bugs that haven’t been caught or prevented by any of the other heuristics I list here. But human judgment seems too unreliable to be the only line of defense against any common class of bug.

That said, complex math is one area that might be an exception. Typically, when bugs slip through code review, it’s because the reviewer’s eyes glazed over and they skimmed the line in question. But math is noticeable enough that you can flag the relevant code for extra scrutiny to avoid this failure mode.

### Dimensional analysis

Physicists are familiar with this one: you can check the units of your math to make sure you wrote it down correctly. For instance, if you write down that the area of a circle is `math.PI * r` instead of `math.PI * r * r`, then you can tell that your formula is wrong because it has units of length, not area. This works best in a language like [F#](http://fsharp.org/) that supports [units as part of the type system](http://msdn.microsoft.com/en-us/library/dd233243.aspx), but you can check units by hand during code review as well.

There are large classes of errors this won’t catch—for instance, if you write down the area of the circle as `r ** 2`, dimensional analysis won’t catch the missing factor of pi. More generally, dimensional analysis will not catch math mistakes if (and only if) your formula is off by a dimensionless factor.

### Automatic differentiation

This one deserves a special mention because it’s really nifty. [Automatic differentiation](http://en.wikipedia.org/wiki/Automatic_differentiation) is a method by which, given any function specified in code, you can algorithmically compute its derivative analytically (i.e., not by approximating it with finite differences—so it’s much faster and more accurate).[5](https://www.benkuhn.net/ml-bugs-2/#fn:5)

The reason this prevents bugs is that a lot of the time, the reason you write down complicated math is the derivative of some criterion of interest. (For instance, [back-propagation in neural networks](http://en.wikipedia.org/wiki/Backpropagation) or [gradient descent in boosted decision trees](http://en.wikipedia.org/wiki/Gradient_descent) or or…) If you can have the computer write down the math for you, then you reduce your chance to make bugs in the first place. Plus, most of the time it’s easier to check that you’ve written down the actual function right than that you calculated the derivative correctly.

## Using different estimators interchangeably

This one is just beautiful.

```
In [1]: import pandas as pd; import numpy as np

In [2]: a = np.array([-1,-1,1,1])

In [3]: a.std()
Out[3]: 1.0

In [4]: pd.Series(a).std()
Out[4]: 1.1547005383792515
```

What the hell?!

If you read the [numpy docs for `std()`](http://docs.scipy.org/doc/numpy/reference/generated/numpy.std.html), you’ll notice it has a parameter `ddof=0`, which stands for [delta degrees of freedom](http://www.statsdirect.com/help/basics/degrees_of_freedom.htm). And if you read the [pandas docs](http://pandas.pydata.org/pandas-docs/dev/generated/pandas.Series.std.html) for the same function _really carefully_, you’ll notice that pandas also has this parameter—but it defaults to `ddof=1`. [Apparently](http://numpy-discussion.10968.n7.nabble.com/Standard-Deviation-std-Suggested-change-for-quot-ddof-quot-default-value-td37200.html) this is because “pandas is a statistics library, and numpy is a numerics library.” Whatever, it’s because the pandas developers hate sanity and consistency. I don’t even know what to do about this one.

* * *

1.  Unfortunately, in Python, keyword arguments can also be supplied via positional parameters—`score(a,b)` would be equivalent to `score(actual=a, predicted=b)` above—which makes this hard to enforce. It could conceivably be checked by some sort of static analysis tool, but I don’t know of any that are cool enough, and it might need static type annotations to get much out of it anyway. [↩︎](https://www.benkuhn.net/ml-bugs-2/#fnref:1)
    
2.  In fact, this one can also be accomplished in Python, although it’ll throw an error at runtime instead of compile time, and you have to make sure always to call the method with keyword arguments—e.g. `f(x=1)` or `f(u=1)`. Calling simply `f(1)` is legal and will never throw a type error, no matter what the keyword argument is. [↩︎](https://www.benkuhn.net/ml-bugs-2/#fnref:2)
    
3.  Note: Joel and I refer here to Apps Hungarian, not to be confused with Systems Hungarian. Apps Hungarian involves adding extra, richer type information prefixes to variable names. It’s useful and helps prevent bugs. Systems Hungarian involves adding redundant type information to variable names (namely, the compile-time type of the variable). It’s useless and annoying. Don’t use Systems Hungarian. [↩︎](https://www.benkuhn.net/ml-bugs-2/#fnref:3)
    
4.  For extra confusion, if you index with a slice instead of a Series, it _will_ change the original DataFrame. Augh! [↩︎](https://www.benkuhn.net/ml-bugs-2/#fnref:4)
    
5.  The idea of automatic differentiation is essentially to define a new numeric type that is a Taylor series (or a truncated Taylor series) and then define all your primitive functions on it. Then when you compute a new function that’s made up of primitive functions you’re just doing math with Taylor series, so you can recover the derivatives of the new function from the coefficients of the series that it outputs. See the Wiki article linked above for more detail. [↩︎](https://www.benkuhn.net/ml-bugs-2/#fnref:5)