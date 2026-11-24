# Interlude: A Mechanistic Interpretability Analysis of Grokking

I left my job at Anthropic a few months ago and since then I’ve been taking some time off and poking around at some independent research. And I’ve just published my first set of interesting results! I used mechanistic interpretability tools to investigate what’s up with the ML phenomena of grokking - where models trained on simple mathematical operations like addition mod 113 and given 30% of the data will initially memorise the data, but then if trained for a long time will abruptly generalise - with a training curve like this:

![](https://images.squarespace-cdn.com/content/v1/5ed9272a58c63e6402d3fefb/0dfd0e4b-61c2-4ad4-b388-2023b7e7c559/w_1270.png?format=1000w)

This blog isn’t really the best place to host technical write-ups, but if you’re interested you can check it out here:

[Summary Tweet Thread](https://twitter.com/NeelNanda5/status/1559060507524403200)

[Technical write-up on the Alignment Forum](https://www.alignmentforum.org/posts/N6WM6hs7RQMKDhYjB/a-mechanistic-interpretability-analysis-of-grokking)

[Colab notebook with code + technical details](https://bit.ly/neelgrokking)
