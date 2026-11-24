Lincoln recently [analyzed](http://www.lincolnquirk.com/blosxom.cgi/rationality/expected-value.html) the [St. Petersburg Paradox](http://en.wikipedia.org/wiki/St._Petersburg_paradox).

> Let’s consider the following gamble: I pay you a fixed amount up-front. Then, I flip a fair coin repeatedly until it comes up heads. You pay me 2^N dollars where N is the number of flips I had to make — so if heads came up right away, then you pay $1; if the sequence was Tails-Tails-Heads, $4; and so on.
> 
> …
> 
> OK, so if you’re like me, you immediately broke out pen and paper and computed the expected value of the gamble. And, perhaps, a surprising thing occurred: you realized that the gamble is valued at negative infinity. (The probability of paying $1 is 1/2; $2, 1/4; $4, 1/8; multiplying this sequence of tiny probabilities by large payments gets you the sum of infinitely many $1/2.)
> 
> …
> 
> I wrote a program to simulate the gamble 10,000 times. The average per-play cost came out to $13. I ran it again, and it was $7. I kept running it — it was usually between about $6 and $18, except for the time it was $50. Whoops.
> 
> My analysis: it’s probably correct for most people to accept such gambles at a high enough price such that the take-home money would be life-changing. I would obviously accept it for a billion dollars, for instance. I’d probably accept it at a million. It’s really hard for our brains to understand the low chance of getting wiped out, though.

Lincoln, I’ll take you up on this gamble for $20. Following Lincoln’s excellent example, I’ll leave some space here so you can try to guess why before scrolling down.

This is an example of how picking a bad model can really be a doozy. As Lincoln noticed, if you just try to calculate the expected value you’ll get a ridiculous answer that isn’t helpful at all.

What’s bad about that model? you might ask. We just took the expected value! That’s what you _do_ with probabilities!

Ah, but: each sequence of `$n$` tails followed by one heads contributes $0.50 to the expected loss. So the _vast bulk_ of the expected loss comes from sequences with, e.g., more than 1000 tails.[1](https://www.benkuhn.net/petersburg/#fn:1)

If you flip 1000 tails and then a heads, I have to pay you `$2^{1000}$` dollars. That’s about 0.01 _centillion_ dollars,[2](https://www.benkuhn.net/petersburg/#fn:2) or larger than the _entire physically possible GDP of the known universe_ (adjusted for puchasing power parity, of course).

Once you flip past the number of tails that will bankrupt me, it doesn’t matter how many extra heads you flip, the loss to me is the same. Meanwhile, the probability of such events falls off exponentially fast. So for a good approximation to a more realistic expected value, we can just chop off the sum that Lincoln wrote about at the number of flips that will bankrupt me.

Suppose that I have `$d$` dollars; then if we flip `$n$` times, I have to pay `$2^n$` dollars. Equating, we get `$d = 2^n \implies n = \log_2 d$`. Since each term before that contributes 1/2 to the expected loss, the total expected loss is approximately `$\frac{\log_2 d}{2}$`. For instance, if I had a billion dollars (about `$2^{30}$`), my expected loss would be about $15.

Of course, this is only a slightly less simplistic model. In reality, a billionaire might be risk-averse: an extra $15 is basically meaningless, while the (very slim) chance of losing a significant chunk of his net worth is much worse. On the other hand, I’m still a starving student; the majority of my assets are in human capital, so the threshold where I become significantly risk-averse is far below mere monetary bankruptcy. That’s why I’d be happy to take this gamble for much less than “life-changing” money.

I suspect that finance companies ask this question to weed out the mathematician’s tendency to favor the elegant and legible over the intuitive and practical. If you ignore the intuitive ridiculousness (“what? nothing is infinity dollars!”), bite the bullet and say you would never take the gamble, you’ll leave a lot of money on the table—and conversely, if you’re willing to pay any price to take it from someone else, they’ll take you for a ride. So beware bad models, even the elegant ones!

* * *

1.  Or more than `$n$` for any finite `$n$`, but let’s pick 1000 for concreteness. [↩︎](https://www.benkuhn.net/petersburg/#fnref:1)
    
2.  1 centillion = `$10^{303}$`. [↩︎](https://www.benkuhn.net/petersburg/#fnref:2)