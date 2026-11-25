# The www prefix

---

## 正文

在为 redesign 而 updating 我的 DNS 时，我必须决定，我的 canonical URL 应该是 `benkuhn.net` 还是 `www.benkuhn.net`。就像所有无关紧要的 website 相关决定一样，这件事已经演变成了某种激烈的争斗。

像 [`no-www`](http://no-www.org/)、[`yes-www`](http://www.yes-www.org/)，甚至 [`extra-www`](http://www.www.extra-www.org/) 这样的 Sites，在这场争论的双方（全部三方？）都有强烈的看法。据 `no-www` 所说，反对 using `www` prefix 的理由是，它已经“deprecated”了，不管那是什么意思——我猜问题在于，它其实并非必需，还会 adds 一些额外的 URL cruft。

另一方面，`yes-www` 提出了一个真正的 technical argument，即 1) 你不能在 naked domain 上 use CNAME record，2) naked domain 上的 cookies 会被 sent 到所有 subdomains，这会搞乱 static files 的 caching。这两点都不算什么大问题（我现在不需要 CNAME record，而且我可以为我的 static media 而 use AWS），但我不想牺牲这种 flexibility。因此，我让 naked domain redirect 到 `www.benkuhn.net`，并将后者设为了 canonical URL。

看过其他 sites 后，似乎很多大公司都 use `www`，比如 [google](http://www.google.com/)、[apple](http://www.apple.com/) 和 [facebook](http://www.facebook.com/)（例外是 [twitter](http://twitter.com/)）。但[有](http://alistapart.com/)[很多](http://jacquesmattheij.com/)[的](http://billyjanitsch.com/)[较小](http://worrydream.com/)[sites](http://xkcd.com/)（做了 random sampling）并不 use `www`。所以总体而言，哪种方式大概都没问题。

---

## 原文

While updating my DNS for the redesign, I had to decide whether my canonical URL should be `benkuhn.net` or `www.benkuhn.net`. Like all irrelevant website-related decisions, this has erupted into some sort of pitched battle.

Sites like [`no-www`](http://no-www.org/), [`yes-www`](http://www.yes-www.org/) and even [`extra-www`](http://www.www.extra-www.org/) have strong opinions on both (all three?) sides of the debate. According to `no-www`, the argument against using the `www` prefix is that it’s “deprecated”, whatever that means—I assume the issue is that it’s not really needed and adds some extra URL cruft.

On the other hand, `yes-www` presents an actual technical argument, which is that 1) you can’t use a CNAME record on a naked domain and 2) cookies on the naked domain get sent to all subdomains, which screws up caching for static files. Both of these are fairly minor (I don’t need a CNAME record right now, and I could use AWS for my static media), but I don’t want to sacrifice that flexibility. So I made the naked domain redirect to `www.benkuhn.net` and made that the canonical URL.

Going through other sites, it looks like lots of big companies use `www`, like [google](http://www.google.com/), [apple](http://www.apple.com/), and [facebook](http://www.facebook.com/) (the exception is [twitter](http://twitter.com/)). But [a](http://alistapart.com/) [lot](http://jacquesmattheij.com/) [of](http://billyjanitsch.com/) [smaller](http://worrydream.com/) [sites](http://xkcd.com/) (random sampling) use no `www`. So either way is probably fine overall.