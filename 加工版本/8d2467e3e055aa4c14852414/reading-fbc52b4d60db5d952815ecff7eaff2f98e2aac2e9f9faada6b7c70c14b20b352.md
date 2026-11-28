# 数据对齐与缓存｜Data alignment and caches

---

## 正文

这里是关于 page-aligned vs. mis-aligned accesses 的一个 [toy benchmark](https://github.com/danluu/setjmp-longjmp-ucontext-snippets/tree/master/benchmark/cache-conflict)[1](https://danluu.com/3c-conflict/#fn:A) 的 graph；它展示了不同 working set sizes 下两者的 performance ratio。如果这个 benchmark 看起来有些刻意，它实际上来自一个真实案例，展示了[在实际 system 中使用规整的 power of 2 alignment 或 page alignment 所带来的灾难性 performance implications](https://groups.google.com/forum/#!msg/comp.arch/_uecSnSEQc4/mvfRnOvIyzUJ)[2](https://danluu.com/3c-conflict/#fn:M)。

![Sandy Bridge Performance 的 Graph](https://danluu.com/images/3c-conflict/sandy.webp) ![Westmere Performance 的 Graph](https://danluu.com/images/3c-conflict/westmere.webp)

除了非常 small working sets (1-8) 之外，unaligned version 都明显比 page-aligned version 更 faster，而且在 working set size 达到 512 之前，有很大一段 region 中的 performance ratio 相当 stable，不过这种稳定性在我们的 Sandy Bridge chip 上比在 Westmere chip 上更明显。

要理解这里发生了什么，我们必须看看 caches 如何 organize data。打个比方，设想一个能停放 1,000 辆车、却发放了 10,000 张停车证的停车场。采用 direct mapped scheme（你也可以称它为 1-way associative[3](https://danluu.com/3c-conflict/#fn:1)）时，具有相同 3 个 least significant digits 的十张停车证都会被分配到同一个车位，也就是说，停车证 0618、1618、2618 等等，都只允许停在 618 号车位。如果你来到自己的车位，发现已经有人停在那里，你就把他们赶走，他们只能开车回家。下次他们被叫来上班时，就必须再一路开车回到停车场。

另一种做法是，如果每辆车的停车证允许它停在一个具有十个可选车位的 set 中，我们就把这称为 10-way set associative scheme，这样就有 100 个 sets，每个都有十个车位。现在，每个 set 都由最后 2 个 significant digits 来 defined，而不是最后 3 个。例如，拿着 2618 号停车证，你可以停在 set {018, 118, 218, …, 918} 中的任何一个车位。如果这些车位都满了，你就像之前一样，赶走一个倒霉的车主，占据他们的车位。

让我们结束这个比喻，回到我们的 benchmark。主要区别在于，这里不只有一个 garage-cache，而是存在一个 hierarchy，从 smallest（因而也是 fastest）的 L1[4](https://danluu.com/3c-conflict/#fn:B) 到 L2 和 L3。车里的每个座位都 corresponds to 一个 address。在 x86 上，每个 addresses 都 points to 一个特定的 byte。在我们运行所用的 Sandy Bridge chip 中，有一个 32kB L1 cache，具有 64-byte line size、64 个 sets，以及 8-way set associativity。在我们的比喻中，64 的 line size 就 correspond to 一辆有 64 个座位的车。我们总是以 64-byte chunks 来 transfer 东西，而一个 address 的 bottom log₂(64) = 6 bits 则 refer to 一个 cache line 中的特定 byte offset。接下来的 log₂(64) = 6 bits 决定一个 address 会 falls into 哪个 set[5](https://danluu.com/3c-conflict/#fn:L)。这些 sets 中的每一个都可以 contain 8 个不同的东西，因此我们有 64 sets \* 8 lines/set \* 64 bytes/line = 32kB。如果我们 optimally 使用 cache，就可以 store 32,768 个 items。但是，由于我们正在 accessing 的东西是 page (4k) aligned 的，我们实际上 lose 了 bottom log₂(4k) = 12 bits，这意味着每次 access 都 falls into 同一个 set，因此我们只能 loop through 8 个东西，之后 working set 就会 too large to fit in L1！但如果我们把 data misaligned 到不同的 cache lines，就能 effectively 使用 8 \* 64 = 512 个 locations。

同样，我们的 chip 有一个具有 512 个 set 的 L2 cache，其中 8 个 sets 对我们的 page aligned accesses 是 useful 的，还有一个具有 12288 个 set 的 L3 cache，其中 192 个 sets 对 page aligned accesses 是 useful 的，分别为我们提供了 8 sets \* 8 lines / set = 64 和 192 sets \* 8 lines / set = 1536 个 useful cache lines。对于 misaligned 了一个 cache line 的 data，我们额外拥有 6 bits 的 useful address，这意味着我们的 L2 cache 现在有 32,768 个 useful locations。

在上面的 Sandy Bridge graph 中，64 到 512 之间有一段 stable relative performance 的 region，因为 page-aligned version version 正在从 L3 cache 运行，而 unaligned version 正在从 L1 运行。当 working set 超过 512 时，relative ratio 对 aligned version 变得更有利，因为现在比较的是 L2 access vs. L3 access。我们的 Westmere graph 看起来略有不同，因为它的 L3 只有 3072 个 sets，这意味着 aligned version 只能在 working set size 不超过 384 时 stay in L3。在那之后，我们就能看到 spilling into main memory 带来的糟糕 performance，这也解释了为什么两个 graphs 在 384 以上的 shape 不同。

为了对这一点进行 visualization，你可以把一个 32 bit pointer 想象成在我们的 L1 和 L2 caches 看来具有如下形式：

`TTTT TTTT TTTT TTTT TTTT SSSS SSXX XXXX`

`TTTT TTTT TTTT TTTT TSSS SSSS SSXX XXXX`

bottom 6 bits 会被 ignored，接下来的 bits 决定我们会 falls into 哪个 set，而 top bits 则是一个 tag，让我们知道那个 set 中实际有什么。请注意，对东西进行 page aligning，也就是将 address setting 为

`???? ???? ???? ???? ???? 0000 0000 0000`

在我们的 benchmark 中仅仅是为了方便。不仅 aligning 到任何较大的 power of 2 都会导致问题，generating 彼此之间具有 power of 2 offset 的 addresses 也会导致同样的问题。

如今，大家对 caches 重要性的理解已经足够充分，因此，当有人请我查看一个 cache related performance bug 时，它通常都是由我们刚才谈到的这类事情引起的：conflict misses，使我们无法 effectively 使用完整的 cache[6](https://danluu.com/3c-conflict/#fn:4)。这并不是发生这种情况的唯一方式——bank conflicts 和 false dependencies 也是常见问题，不过我会留到另一篇博客文章中再谈。

#### 参考资源

想进一步了解 memory 上的 caches，请参阅 [每位 Programmer 都应该了解的 Memory 知识](http://www.akkadia.org/drepper/cpumemory.pdf)。如果想读涉及范围更广的内容，可以看看[这篇较“短”的博客文章](https://danluu.com/new-cpu-features/)，或者阅读篇幅相当于一本书的 [Modern Processor Design](http://www.amazon.com/gp/product/1478607831/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1478607831&linkCode=as2&tag=abroaview-20&linkId=HXBNFFJ3CIXMWUZP)。如果想要更广的范围（上面那两个链接侧重于 CPUs 和 memory），请参阅 [Computer Architecture: A Quantitative Approach](http://www.amazon.com/gp/product/012383872X/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=012383872X&linkCode=as2&tag=abroaview-20&linkId=ZHRJAO77SUQ6V7GL)，它讨论了整个 system，一直到 datacenter level。

* * *

1.  这里的 Sandy Bridge 是一颗 i7 3930K，而 Westmere 是一颗 mobile i3 330M。[\[返回\]](https://danluu.com/3c-conflict/#fnref:A)
2.  或者，任何在具有 2-way associative 或 direct mapped cache 的 chip 上运行时，在包含两个 source arrays 和一个 destination 的 calculation 中，把 data aligned 得过于规整的人，都可能遇到这种情况。当你为了做 cache blocking 而以某种规整的方式设置 arrays 时，如果不小心，这种情况会常见得令人吃惊。[\[返回\]](https://danluu.com/3c-conflict/#fnref:M)
3.  别这么叫它。别人会用奇怪的眼神看你，就像你把 SQL 念成 squeal 或 squll 时一样。[\[返回\]](https://danluu.com/3c-conflict/#fnref:1)
4.  在这篇文章中，L1 指的是 l1d。由于我们只关心 data，l1i 在这里并不 relevant。抱歉，这里对 terminology 的使用不够严谨。[\[返回\]](https://danluu.com/3c-conflict/#fnref:B)
5.  如果你觉得将 least significant available address bits 用作 set index 很奇怪，那是因为 computer architecture 的首要原则是 make the common case fast——Google Instant 会将“make the common”自动补全为“make the common case fast”、“make the common case fast mips”和“make the common case fast computer architecture”。绝大多数 accesses 都 close together，因此将 set index bits 向上 moving 会导致更多 conflict misses。你或许可以采用一个并非简单使用 least significant bits 的 hash function，但大多数被提出的 schemes 在增加额外 complexity 的同时，带来的损害与帮助差不多一样多。[\[返回\]](https://danluu.com/3c-conflict/#fnref:L)
6.  Cache misses 经常使用 3C model 来 described：conflict misses，由我们刚刚谈到的那类 aliasing 引起；compulsory misses，由对一个 memory location 的首次 access 引起；以及 capacity misses，由 working set 对某个 cache 而言 too large 引起，即便没有 conflict misses 也是如此。像这样的 Page-aligned accesses 还会使 compulsory misses 更糟，因为 prefetchers 不会 prefetch 越过 page boundary 的内容。不过，如果你的 data 已经多到需要将东西 aligning 到 page boundaries，你大概也没有多少办法能改变这一点。[\[返回\]](https://danluu.com/3c-conflict/#fnref:4)

---

## 原文

Here's the graph of [a toy benchmark](https://github.com/danluu/setjmp-longjmp-ucontext-snippets/tree/master/benchmark/cache-conflict)[1](https://danluu.com/3c-conflict/#fn:A) of page-aligned vs. mis-aligned accesses; it shows a ratio of performance between the two at different working set sizes. If this benchmark seems contrived, it actually comes from a real world example of the [disastrous performance implications of using nice power of 2 alignment, or page alignment in an actual system](https://groups.google.com/forum/#!msg/comp.arch/_uecSnSEQc4/mvfRnOvIyzUJ)[2](https://danluu.com/3c-conflict/#fn:M).

![Graph of Sandy Bridge Performance](https://danluu.com/images/3c-conflict/sandy.webp) ![Graph of Westmere Performance](https://danluu.com/images/3c-conflict/westmere.webp)

Except for very small working sets (1-8), the unaligned version is noticeably faster than the page-aligned version, and there's a large region up to a working set size of 512 where the ratio in performance is somewhat stable, but more so on our Sandy Bridge chip than our Westmere chip.

To understand what's going on here, we have to look at how caches organize data. By way of analogy, consider a 1,000 car parking garage that has 10,000 permits. With a direct mapped scheme (which you could call 1-way associative[3](https://danluu.com/3c-conflict/#fn:1)), each of the ten permits that has the same 3 least significant digits would be assigned the same spot, i.e., permits 0618, 1618, 2618, and so on, are only allowed to park in spot 618. If you show up at your spot and someone else is in it, you kick them out and they have to drive back home. The next time they get called in to work, they have to drive all the way back to the parking garage.

Instead, if each car's permit allows it to park in a set that has ten possible spaces, we'll call that a 10-way set associative scheme, which gives us 100 sets of ten spots. Each set is now defined by the last 2 significant digits instead of the last 3. For example, with permit 2618, you can park in any spot from the set {018, 118, 218, …, 918}. If all of them are full, you kick out one unlucky occupant and take their spot, as before.

Let's move out of analogy land and back to our benchmark. The main differences are that there isn't just one garage-cache, but a hierarchy of them, from the L1[4](https://danluu.com/3c-conflict/#fn:B), which is the smallest (and hence, fastest) to the L2 and L3. Each seat in a car corresponds to an address. On x86, each addresses points to a particular byte. In the Sandy Bridge chip we're running on, we've got a 32kB L1 cache with 64-byte line size and, 64 sets, with 8-way set associativity. In our analogy, a line size of 64 would correspond to a car with 64 seats. We always transfer things in 64-byte chunks and the bottom log₂(64) = 6 bits of an address refer to a particular byte offset in a cache line. The next log₂(64) = 6 bits determine which set an address falls into[5](https://danluu.com/3c-conflict/#fn:L). Each of those sets can contain 8 different things, so we have 64 sets \* 8 lines/set \* 64 bytes/line = 32kB. If we use the cache optimally, we can store 32,768 items. But, since we're accessing things that are page (4k) aligned, we effectively lose the bottom log₂(4k) = 12 bits, which means that every access falls into the same set, and we can only loop through 8 things before our working set is too large to fit in the L1! But if we'd misaligned our data to different cache lines, we'd be able to use 8 \* 64 = 512 locations effectively.

Similarly, our chip has a 512 set L2 cache, of which 8 sets are useful for our page aligned accesses, and a 12288 set L3 cache, of which 192 sets are useful for page aligned accesses, giving us 8 sets \* 8 lines / set = 64 and 192 sets \* 8 lines / set = 1536 useful cache lines, respectively. For data that's misaligned by a cache line, we have an extra 6 bits of useful address, which means that our L2 cache now has 32,768 useful locations.

In the Sandy Bridge graph above, there's a region of stable relative performance between 64 and 512, as the page-aligned version version is running out of the L3 cache and the unaligned version is running out of the L1. When we pass a working set of 512, the relative ratio gets better for the aligned version because it's now an L2 access vs. an L3 access. Our graph for Westmere looks a bit different because its L3 is only 3072 sets, which means that the aligned version can only stay in the L3 up to a working set size of 384. After that, we can see the terrible performance we get from spilling into main memory, which explains why the two graphs differ in shape above 384.

For a visualization of this, you can think of a 32 bit pointer looking like this to our L1 and L2 caches:

`TTTT TTTT TTTT TTTT TTTT SSSS SSXX XXXX`

`TTTT TTTT TTTT TTTT TSSS SSSS SSXX XXXX`

The bottom 6 bits are ignored, the next bits determine which set we fall into, and the top bits are a tag that let us know what's actually in that set. Note that page aligning things, i.e., setting the address to

`???? ???? ???? ???? ???? 0000 0000 0000`

was just done for convenience in our benchmark. Not only will aligning to any large power of 2 cause a problem, generating addresses with a power of 2 offset from each other will cause the same problem.

Nowadays, the importance of caches is well understood enough that, when I'm asked to look at a cache related performance bug, it's usually due to the kind of thing we just talked about: conflict misses that prevent us from using our full cache effectively[6](https://danluu.com/3c-conflict/#fn:4). This isn't the only way for that to happen -- bank conflicts and and false dependencies are also common problems, but I'll leave those for another blog post.

#### Resources

For more on caches on memory, see [What Every Programmer Should Know About Memory](http://www.akkadia.org/drepper/cpumemory.pdf). For something with more breadth, see [this blog post for something "short"](https://danluu.com/new-cpu-features/), or [Modern Processor Design](http://www.amazon.com/gp/product/1478607831/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1478607831&linkCode=as2&tag=abroaview-20&linkId=HXBNFFJ3CIXMWUZP) for something book length. For even more breadth (those two links above focus on CPUs and memory), see [Computer Architecture: A Quantitative Approach](http://www.amazon.com/gp/product/012383872X/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=012383872X&linkCode=as2&tag=abroaview-20&linkId=ZHRJAO77SUQ6V7GL), which talks about the whole system up to the datacenter level.

* * *

1.  The Sandy Bridge is an i7 3930K and the Westmere is a mobile i3 330M [\[return\]](https://danluu.com/3c-conflict/#fnref:A)
2.  Or anyone who aligned their data too nicely on a calculation with two source arrays and one destination when running on a chip with a 2-way associative or direct mapped cache. This is surprisingly common when you set up your arrays in some nice way in order to do cache blocking, if you're not careful. [\[return\]](https://danluu.com/3c-conflict/#fnref:M)
3.  Don't call it that. People will you look at you funny the same way they would if you pronounced SQL as squeal or squll. [\[return\]](https://danluu.com/3c-conflict/#fnref:1)
4.  In this post, L1 refers to the l1d. Since we're only concerned with data, the l1i isn't relevant. Apologies for the sloppy use of terminology. [\[return\]](https://danluu.com/3c-conflict/#fnref:B)
5.  If it seems odd that the least significant available address bits are used for the set index, that's because of the cardinal rule of computer architecture, make the common case fast -- Google Instant completes “make the common” to “make the common case fast”, “make the common case fast mips”, and “make the common case fast computer architecture”. The vast majority of accesses are close together, so moving the set index bits upwards would cause more conflict misses. You might be able to get away with a hash function that isn't simply the least significant bits, but most proposed schemes hurt about as much as they help while adding extra complexity. [\[return\]](https://danluu.com/3c-conflict/#fnref:L)
6.  Cache misses are often described using the 3C model: conflict misses, which are caused by the type of aliasing we just talked about; compulsory misses, which are caused by the first access to a memory location; and capacity misses, which are caused by having a working set that's too large for a cache, even without conflict misses. Page-aligned accesses like these also make compulsory misses worse, because prefetchers won't prefetch beyond a page boundary. But if you have enough data that you're aligning things to page boundaries, you probably can't do much about that anyway. [\[return\]](https://danluu.com/3c-conflict/#fnref:4)