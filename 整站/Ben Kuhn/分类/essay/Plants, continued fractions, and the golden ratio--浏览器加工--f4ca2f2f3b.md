You are a plant. You have a stem, and you can sprout off leaves from the stem in various directions. You want to maximize the amount of sunlight that hits your leaves, i.e., you want the angular distance between your leaves to be as wide as possible. However, as a plant, you aren’t very smart. You can only grow in patterns with a _constant angle_ between any two successive leaves—so if your first leaf points north and your second points east, the third one must point south and the fourth one west.[1](https://www.benkuhn.net/cf-plants/#fn:1)

You may be a dumb plant, but you _have_ been fine-tuned by evolution for hundreds of millions of years, so your solution had better be good. What do you do?

It turns out there’s a very nice way to solve this problem, but first we need to take a detour to talk about continued fractions.

## Preliminaries

Call the angle between successive leaves `$\alpha$`. For simplicity, we’ll consider `$\alpha$` in units of fractions of a circle—e.g., if `$\alpha = \frac14$`, this corresponds to an angle of `$\pi/2$` radians or 90 degrees.

The real disaster when picking such an `$\alpha$` would be if you picked a rational number with a small denominator—for instance, `$\alpha = \frac12$`. In that case, every other leaf would completely cover up a previous leaf! That’s an enormous waste of growth. Similarly, if `$\alpha \approx \frac12$`—say `$\alpha = \frac1{2 + \epsilon}$` for some small `$\epsilon$`—then the third leaf will shade almost all of the first leaf, and so on.

Naively, one might then pick the `$\alpha$` that is farthest away from the known “bad values” of 0 and `$\frac12$`, and try `$\alpha = \frac14$`. But that’s little better—with this `$\alpha$` you’ll only get to leaf number four before you start growing useless leaves! In fact, if you choose `$\alpha$` to be _any_ rational number with some denominator `$q$`, then after `$q$` leaves all your new growth will be useless.

You might try to get around this by picking `$\alpha$` to have a very large denominator or even to be irrational, but there are further pitfalls. If `$q$` is too large, you run the risk of choosing something like `$\frac{4999}{10000}$` that’s very _close_ to a low-denominator rational number, and then your third leaf will shade _most_ (though not all) of your first leaf, and so on. Even if you require `$\alpha$` to be irrational, you could still make a bad choice like `$\frac12 + \frac{\sqrt{2}}{10000}$`.

So what you really want is an angle which is as far away as possible from _all_ rational numbers with small denominators. But how do we study that?

## Continued fractions

A _continued fraction_ is an expression of the form `$$a_0 + \cfrac1{a_1 + \cfrac1{a_2 + \cfrac1\ddots}}$$` for natural numbers `$a_i$`, which may or may not terminate. Since writing such fractions is repetitive and unwieldy, as a shorthand we write `$[a_0; a_1, a_2, \dots]$` to mean the same thing.

It turns out that this always converges, even if the series `$\{a_i\}$` is infinite.[2](https://www.benkuhn.net/cf-plants/#fn:2) Of course, if the series is finite, the value it converges to is rational—you can just do a bunch of fraction-addition and inversion to figure it out. On the other hand, if the series is infinite, the value it converges to is irrational (I’m not going to show that here).

What’s more, if we truncate the expression at a particular level (by removing one of the plus signs and everything after it), the result is a _best rational approximation_ to the value `$\alpha$` of the original expression, in the sense that it’s closer to `$\alpha$` than any other rational number with a smaller denominator.

Because of this, we introduce the following notation and vocabulary for truncating a continued fraction: if `$[a_0; a_1, a_2, \dots]$` is a continued fraction (possibly one that terminates), we define its `$n$`th _convergent_ as `$[a_0; a_1, \dots, a_n] := \frac{p_n}{q_n}$` (where `$p_n$` and `$q_n$` are the numerator and denominator, respectively, in lowest terms).

## How do continued fractions help?

You might have gotten some sense of the answer to this question when I mentioned “best rational approximations” above. In fact, that’s the key to how continued fractions inform plant growth. Using some machinery that I’m about to introduce, we can answer the question: what number is _least well-approximated_ by its low-denominator rational approximations?

We can answer this question by finding an error bound on the convergents of an infinite continued fraction. In particular, if `$[a_0; a_1, a_2, \dots]$` is the continued fraction expansion of some number `$\alpha$`, then it can be shown (although I won’t show it here) that the error in the `$n$`th convergent, `$$\left|\frac{p_n}{q_n} - \alpha\right| \le \frac{1}{a_n q_n^2}.$$` This suggests that the continued fractions with lower values for `$a_i$` are less well-approximated. Intuitively, this makes sense: `$\cfrac1{5 + \cfrac1{10000}}$` is much closer to `$\frac15$` than is `$\cfrac1{5 + \cfrac1{2}}$`. So perhaps the number we’re looking for is `$[1; 1, 1, \dots]$`, the number with the lowest values of all.[3](https://www.benkuhn.net/cf-plants/#fn:3)

That leaves only one complication: what’s the value of `$\alpha = [1; 1, 1, \dots]$`? Recall that this means `$$\alpha = 1 + \cfrac1{1 + \cfrac1{1 + \cfrac1\ddots}}.$$` Now we can perform a clever trick to evaluate this by noticing that this expression is _self-similar_. If we write down `$\alpha - 1$` and `$1/\alpha$`, we notice that they look exactly the same! This gives us an equation defining `$\alpha$`. `$$\alpha - 1 = \frac1\alpha$$`

If you know some geometry, you might recognize this equation, because it is also the equation that defines the golden ratio `$\phi$`! `$\phi$` is also defined by self-similarity: when you draw a `$\phi \times 1$` rectangle, and chop off a `$1 \times 1$` square, you get back a similar rectangle (in the technical geometric sense).1φ1–φ As the figure shows, since the two rectangles `$\phi \times 1$` and `$1 \times 1 - \phi$` are similar, the ratios between the corresponding sides are equal, and `$\phi - 1 = \frac1\phi$`.

Well, that’s an exciting prediction: plants will tend to grow their leaves with an angular distance of `$\phi$` between them! A fraction `$\phi$` of the circle corresponds to an angle of about 137.5 degrees (or equivalently, 222.5 degrees, 497.5 degrees, etc.; by convention we talk about the positive angle that is less than 180 degrees). If you want, you can verify this: the vast majority of plants in the wild do sprout their branches at angles of 137.5 degrees from each other:

> Based on a survey of the literature encompassing 650 species and 12500 specimens, R. Jean (1994) estimated that, among plants displaying spiral or multijugate phyllotaxis about 92% of them have Fibonacci phyllotaxis.
> 
> — Pau Atela, [Phyllotaxis](http://www.math.smith.edu/phyllo/About/fibogolden.html)

## Further reading

My overview of continued fractions was based (very loosely) on section 15.2 of Hasselblatt and Katok’s [_A First Course in Dynamics_](http://www.amazon.com/First-Course-Dynamics-Panorama-Developments/dp/0521587506), the textbook I learned them from.

There’s a related phenonenon known as _Fibonacci phyllotaxis_ in which spiral plants grow in Fibonacci numbers of seeds, leaves or petals. Roger Jean’s [_Phyllotaxis: a Systemic Study in Plant Morphogenesis_](http://books.google.com/books?hl=en&lr=&id=9pCn3W0gfFAC&oi=fnd&pg=PP1&dq=Phyllotaxis:+A+systematic+study+in+plant+morphogenesis&ots=IUiVYP6Woh&sig=gpG3lVDjChqOF3pLD4Ezv3jDq2Q#v=onepage&q&f=false) has the scoop. Pau Atela at Smith College also has a [site](http://www.math.smith.edu/phyllo/About/index.html) devoted to it with some models, papers, and talks.

R. Knott at the University of Surrey [also discusses continued fractions](http://www.maths.surrey.ac.uk/hosted-sites/R.Knott/Fibonacci/cfINTRO.html) in a fairly accessible style. He also has a page on [Fibonacci Numbers and Nature](http://www.maths.surrey.ac.uk/hosted-sites/R.Knott/Fibonacci/fibnat.html) which discusses the connection to plants.

_Thanks to Ruthie Byers and Nat Kuhn for reading a draft of this post._

* * *

1.  Actually, it turns out that you don’t have to be very dumb to be restricted to a constant angular distance between successive leaves. You can show that constant angular distance is implied by a weaker property, namely that the angle that each leaf grows out at (call it `$\theta_i$`) depends only on the angle of the previous leaf (that is, `$\theta_{i-1}$`).
    
    Let `$f$` be the function with which you calculate `$\theta_i$` from `$\theta_{i-1}$`. As a plant, you can’t have a preferred orientation (to a first approximation), so `$f$` should be _rotation-invariant_—that is, `$f(x + \omega) = f(x) + \omega$`.
    
    But this immediately implies that `$f(\theta_{i-1}) = \theta_{i-1} + f(0)$`—that is, `$f$` is rotation by an angle `$f(0) =: \alpha$`. This corresponds to putting an angle of `$\alpha$` between every two successive leaves. [↩︎](https://www.benkuhn.net/cf-plants/#fnref:1)
    
2.  We can define how to evaluate such an infinite continued fraction as follows: `$$[a_0; a_1, a_2, \dots] = \lim_{i \to \infty} \frac{p_i}{q_i}$$` You can see that this converges by noting that `$$[a_0; a_1, \dots, a_{i-1}] < \frac{p_i}{q_i} = [a_0; a_1, \dots, a_{i+1}] < [a_0; a_1, \dots, a_{i-1} + 1]$$` if `$i$` is even, and the reverse if `$i$` is odd. Furthermore, the distance between the two bounds goes to zero as `$i$` increases. So the sequence `$\left\{\frac{p_i}{q_i}\right\}$` is Cauchy and hence the limit converges. [↩︎](https://www.benkuhn.net/cf-plants/#fnref:2)
    
3.  Of course, since this is an upper bound on the error, we can’t be sure that `$[1; 1, 1, \dots]$` has the largest error: we can only say that our theorem allows it to have a larger error than any other continued fraction. As it happens, you can prove a tighter bound that is actually achieved by `$[1; 1, 1, \dots]$`, which does show that it has the largest error, but that’s outside the scope of this post, which is already too long. [↩︎](https://www.benkuhn.net/cf-plants/#fnref:3)