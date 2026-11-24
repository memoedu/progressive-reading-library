_A friend of mine who’s taking an introductory machine learning course recently emailed me with a bunch of questions about my experience doing machine learning. I thought I’d post parts of my response here to get other people’s opinions on them. Here’s one fragment._

When I’m trying to build a good predictive model for a dataset, I usually find it quite hard to do better than a fairly off-the-shelf [gradient boosted decision tree](https://en.wikipedia.org/wiki/Gradient_boosting) machine. These work, in brief, as follows:

1.  Start by fitting a constant model to minimize some loss function (e.g. squared error for regression, log loss for classification).
    
2.  Fit a decision tree to the _gradient_ of the loss function at each data point (optionally, on a random subset of the data). Use line-search to find the optimal value to put in each leaf of the tree.
    
3.  Add that decision tree, times some shrinkage parameter known as the “learning rate,” to your estimator. Repeat a number of times.
    

Gradient trees have a number of great attributes.

-   They train relatively quickly compared to most things with competitive performance.
    
-   Like many tree-based methods, they’re very robust to parameter settings.
    
-   It’s very easy to regularize by turning up the shrinkage, and compensate for this by adding more trees to the model.
    
-   Even though the individual trees are piecewise-constant, the machine can pick up fairly complex response shapes because it adds together a bunch of different trees.
    
-   If you allow the trees to be 2-3 levels deep you can pick up on fairly complex interactions.
    
-   Although the algorithm that I just described doesn’t parallelize, there are distributed and parallelized variants that scale fairly well.
    

Even in competitions, gradient boosting often does quite well (though it seems you still need more complex algorithms to get that extra 0.1% performance boost). For instance, a recent implementation called [xgboost](https://github.com/tqchen/xgboost) got [very competitive results](https://no2147483647.wordpress.com/2014/09/17/winning-solution-of-kaggle-higgs-competition-what-a-single-model-can-do/) on the Higgs boson detection competition.

The fact that GBMs work so well seems fairly well-known to machine learning practitioners, but I haven’t found much discussion of it online. In fact, it seems to be generally hard to find discussion about the practicalities of machine learning; people seem much more interested in explaining how algorithms work, or giving tutorials on software libraries, than talking about the actual process they use when trying to answer data-related questions. I’d be interested to hear other folks’ perspective on their “workhorse” models and how they go about analyzing data.