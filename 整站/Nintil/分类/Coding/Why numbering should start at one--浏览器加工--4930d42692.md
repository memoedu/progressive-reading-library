> To understand his reasoning, I think it is important to keep in mind that Dijkstra was a mathematician and that these people have a very special/peculiar sense of aesthetics. Let me explain what I think Dijkstra did not like about b), c) and d).
> 
> Assume you begin counting at 0, so the natural numbers are the set {0,1,2,3,...} and you want to denote the set {0,1}. If you go with b) you have to write -1 < i <= 1. Note that you have to use the number -1, which is not part of the natural numbers, to describe a subset of the natural numbers. As a mathematician, this might feel wrong or at the very least not very elegant.
> 
> This does not change if you begin counting at 1, i.e. if you define the natural numbers as the set {1,2,3,4,...}. Assume you want denote the set {1,2}. If you go with b) you have to write 0 < i <= 2. Here you have to use 0 as the lower bound, but we just said that the natural numbers start with 1, so again we need a non-natural number to describe a subset of the natural numbers.
> 
> For the empty set... well, say the natural numbers start with 1 and consider the sequence of subsets {1,2,3}, {1,2}, {1}, {}. If you include the upper bound and go with c), you have to describe them as 1 <= i <= 3 (looks good) 1 <= i <= 2 (still ok) 1 <= i <= 1 (ah... I see where this is going...) 1 <= i <= 0 (zero out of nowhere!)
> 
> I agree that most people may find it useless to talk about the empty set, but as I said, mathematicians are probably different here.

-   ![Artir](https://www.gravatar.com/avatar/77fac16b1eefeb9c6dcd9d0cfa2ea692?d=identicon) **[Artir](http://)** 2017-09-25T17:25:12Z
    
    > Yes, you are right: I got that reasoning wrong about half-closed intervals and his underlying motives.
    
-   **[Rational Feed – deluks917](https://deluks917.wordpress.com/2017/09/16/rational-feed-120/)** 2017-09-16T22:00:20Z
    
    > \[…\] Why Numbering Should Start At One by Artir – the author responds to many well known arguments in favor of 0-indexing. \[…\]
    
-   ![kaleberg](https://www.gravatar.com/avatar/5d094ab55778c2cd2ab5710eaf6eea29?d=identicon) **[kaleberg](http://kaleberg.wordpress.com/)** 2017-09-27T01:09:42Z
    
    > APL used to let you choose zero or one based arrays, but the choice applied to the entire workspace. PL/I let you specify the lower bound as any integer you chose as long it was less than the upper bound. The default was one. I get the impression that most modern languages start at zero. It really seems to be a matter of style and taste. The book 'Gravitation' does general relativity with subscripts 0,1,2,3 where 0 is the time dimension, as opposed to a spatial dimension. This actually made sense until physicists started thinking about spaces with more than one temporal dimension.