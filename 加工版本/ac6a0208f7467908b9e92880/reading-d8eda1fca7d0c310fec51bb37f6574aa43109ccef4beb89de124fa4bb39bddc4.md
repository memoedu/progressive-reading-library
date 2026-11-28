# 数据驱动的缺陷发现｜Data-driven bug finding

---

## 正文

[我已经记不得上一次一整天都没有遇到 software bug 是什么时候了](https://danluu.com/everything-is-broken/)。有好几个星期，我都无法邀请任何人参加 Facebook events，因为一个 bug 导致 invite button 不在 invite screen 上 display。自从我搬到一座小城市以来，Google Maps 就一直给我 illegal、有时甚至 impossible 的 directions。而 Google Docs 经常在我 paste 一个 image 时 hangs，给我显示一个 busy icon，直到我 delete 那个 image。

bugs 逃过 testing 是可以理解的。Testing 很难。Integration testing 更难。End to end testing 就更难了。但还有一种更容易的办法。这类 bugs——我每天都会遇到的 bugs——有三分之一可以用 analytics 来 automatically found。

如果你觉得用 analytics 来 finding bugs 听起来很奇怪，不妨向做 hardware 的人问问 performance counters。无论它们是否 user accessible，每个 ASIC 都有 analytics，让 designers 能够弄清楚下一代 chip 需要做哪些 changes。既然人们反正都会查看 perf counters，他们就会注意到某条 [forwarding path](http://acg.cis.upenn.edu/papers/micro05_storeq.pdf) 从未被 used、[way prediction](http://infoscience.epfl.ch/record/135571/files/micro01-way.pdf) 有着奇怪的 distribution，或 prefetch buffer 从未 fills up。[analytics 中的 Unexpected distributions](https://danluu.com/discontinuities/) 是存在 misunderstanding 的迹象，而这往往又是存在 bug 的迹象[1](https://danluu.com/bugalytics/#fn:A)。

Facebook 会 logs 所有 user actions。这可以用来 determine user dead ends。Google Maps 会在“wrong” turns 之后 reroutes。这可以用来 determine 哪些 wrong turns 是 bad directions 的结果。Google Docs 可以 track 所有 undos[2](https://danluu.com/bugalytics/#fn:U)。这可以用来 determine users 何时遇到了 misfeatures 或 bugs[3](https://danluu.com/bugalytics/#fn:1)。

我理解，为什么借用 hardware practices 来进行 software development 可能让人感觉奇怪。在大多数方面，hardware tools 都比 software tools 落后几十年。例如：如今的 hardware tools 包括 Linux 上那些只从 windows 完成了一半 ported 的 simulators，结果一些 text boxes 要求 forward slashes，另一些却要求 backslashes；在使用 \`default\_nettype none 时 fail to compile 的 libraries[4](https://danluu.com/bugalytics/#fn:2)；以及随附 support engineers 的 components，因为人们预期它们太过 buggy，以至于任何一种具体用途如果没有全职人员 supporting 就无法 work。

但说到 testing，hardware 就远远领先于 software 了。我 write software 时，fuzzing 被认为是一种 state of the art technique。但在 hardware 领域，fuzzing 并没有专门的名字。它就是 testing，而且为什么要给“使用 randomness 的 testing”起一个专门的名字呢？这就像给“通过 running code 来进行 testing”起名字一样。早在十多年前，我就通过一个 tool 做过 [hardware testing](http://en.wikipedia.org/wiki/POWER6)：它对 inputs 使用 constrained randomness、使用 symbolic execution，并通过 structural analysis 进行 state reduction。对于 small units，这个 tool 能够 generate 一份 formal proof of correctness。对于 larger units，这个 tool 会 automatically generated 并使用 coverage statistics，再利用它们在尽可能 diverse 的 state space 中 exhaustively search。如果存在 bug，就会 produced 一个简短、容易 debug 的 counter example。而自那以后，hardware testing tools 又进步了许多。

但在 software 领域，我想 contribute 的随便一个 project 只要有 tests，就算我走运了。即使存在 tests，它们通常也是 handwritten，随之而来的所有 limitations 也都存在。极偶尔，我会惊喜地发现某个 software project 使用了[一个 test framework](http://hackage.haskell.org/package/QuickCheck)，其 functionality 达到了十年前 chip designs 中 standard 水平的 1%。

考虑到 [hardware bugs 与 software bugs](https://danluu.com/testing/) 的 relative cost，人们在 hardware testing 上投入多得多的精力，也就不太令人惊讶了。但这里有一种几乎不用额外费力的情况。你已经有 analytics，在 measuring 各种 user funnels 的 conversion rate。这里唯一的新想法是：clicking 一个 ad 或 making a purchase，并不是你应该 measure 的唯一一种 conversion。在 intersection 处 Following directions 是一种 conversion，在 pasting 一个 image 后不立即 deleting 它是一种 conversion，而在 opening 一个 modal dialogue box 后 using 它也是一种 conversion。

当然，无论是 ad click conversion rates 还是 cache hit rates，盲目 optimizing 单个 number 都会让你陷入一个 local optima，长远来看反而会伤害你；而为 conversion rates 设置应当向你发送 alert 的 thresholds 也并非易事。user actions 有一个 combinatorially large space，因此需要审慎使用 machine learning 才能弄清合理的 thresholds。这会耗费时间和精力。但想想你在 optimizing clicks 上投入的那些精力。你大概早在几年前就已经发现，[用巨大的 pancake buttons 替换乏味的 text，能让你获得 3x 的 clickthrough rate](http://www.kalzumeus.com/2009/03/07/how-to-successfully-compete-with-open-source-software/)；如今你能做的只是在这里 optimizing 1%、那里 optimizing 2%。这很好，说明你已经摘完了所有唾手可得的果实。但当一个 user 遇到一个 show-stopping bug，无法在 modal dialogue box 上取得任何 forward progress 时，你觉得未来的 clickthrough rate 会是多少？

如果这听起来工作量大得吓人，那就找一个你已经 fixed 的 known bug，然后在你的 logs data 中 grep 出遇到过那个 bug 的 users。提供一个严重 broken 的 product，让那些 users 离你而去，对 clickthrough rate 的影响远比一个难找的 checkout button 大；而当初让你做出那个巨大 checkout button 的同一套 process，也能 solve 你的另一个问题。大家都知道，增加 200ms 的 load time 就可能导致 20% 的 users close 那个 window。那么，让他们遇到一个需要 5,000ms 的 user interaction 才能 fix 的 bug，你觉得会有什么影响？

如果这值得 fixing，就拿出 [scalding](https://github.com/twitter/scalding)、[dremel](http://research.google.com/pubs/pub36632.html)、[cascalog](http://cascalog.org/)，或任何你最喜欢的 data processing tool。开始寻找那些不合情理的 user actions。开始寻找 bugs。

感谢 Pablo Torres 发现了本文中的一处笔误

* * *

1.  并不是所有 chip design teams 都会 systematically 这么做（尽管它们应该如此），而是人们反正都在查看 numbers，因此会看到 anomalies。[\[返回\]](https://danluu.com/bugalytics/#fnref:A)
2.  Undos 不仅仅是字面意义上的 undos；pasting 一个 image 后，因为它一直显示 busy icon 而又 deleting 它，也算在内。[\[返回\]](https://danluu.com/bugalytics/#fnref:U)
3.  这比听起来还要糟。除了在 doc 中一直 producing 一个 busy icon，它还会让那个 session 与 server disconnects，这也是另一件可以 detected 的事：如果某个 user action 之后总是跟着一次 disconnection，那就非常可疑了。

    此外，这两种 failure modes 本来都可以用 fuzzing 找出来，因为它们根本就不应该发生。Bugs 已经够难找了，因此 defense in depth 是唯一合理的 solution。

    [\[返回\]](https://danluu.com/bugalytics/#fnref:1)
4.  如果你和做 hardware 的人交谈，要把这个叫作 verification，而不是 testing，否则他们会以为你在说 DFT、针对 manufacturing defects 的 testing silicon，或其他某种没有 software analogue 的古怪东西。[\[返回\]](https://danluu.com/bugalytics/#fnref:2)

---

## 原文

[I can't remember the last time I went a whole day without running into a software bug](https://danluu.com/everything-is-broken/). For weeks, I couldn't invite anyone to Facebook events due to a bug that caused the invite button to not display on the invite screen. Google Maps has been giving me illegal and sometimes impossible directions ever since I moved to a small city. And Google Docs regularly hangs when I paste an image in, giving me a busy icon until I delete the image.

It's understandable that bugs escape testing. Testing is hard. Integration testing is harder. End to end testing is even harder. But there's an easier way. A third of bugs like this – bugs I run into daily – could be found automatically using analytics.

If you think finding bugs with analytics sounds odd, ask a hardware person about performance counters. Whether or not they're user accessible, every ASIC has analytics to allow designers to figure out what changes need to be made for the next generation chip. Because people look at perf counters anyway, they notice when a [forwarding path](http://acg.cis.upenn.edu/papers/micro05_storeq.pdf) never gets used, when [way prediction](http://infoscience.epfl.ch/record/135571/files/micro01-way.pdf) has a strange distribution, or when the prefetch buffer never fills up. [Unexpected distributions in analytics](https://danluu.com/discontinuities/) are a sign of a misunderstanding, which is often a sign of a bug[1](https://danluu.com/bugalytics/#fn:A).

Facebook logs all user actions. That can be used to determine user dead ends. Google Maps reroutes after “wrong” turns. That can be used to determine when the wrong turns are the result of bad directions. Google Docs could track all undos[2](https://danluu.com/bugalytics/#fn:U). That could be used to determine when users run into misfeatures or bugs[3](https://danluu.com/bugalytics/#fn:1).

I understand why it might feel weird to borrow hardware practices for software development. For the most part, hardware tools are decades behind software tools. As examples: current hardware tools include simulators on Linux that are only half ported from windows, resulting in some text boxes requiring forward slashes while others require backslashes; libraries that fail to compile with \`default\_nettype none[4](https://danluu.com/bugalytics/#fn:2); and components that come with support engineers because they're expected to be too buggy to work without full-time people supporting any particular use.

But when it comes to testing, hardware is way ahead of software. When I write software, fuzzing is considered a state of the art technique. But in hardware land, fuzzing doesn't have a special name. It's just testing, and why should there be a special name for "testing that uses randomness"? That's like having a name for "testing by running code". Well over a decade ago, I did [hardware testing](http://en.wikipedia.org/wiki/POWER6) via a tool that used constrained randomness on inputs, symbolic execution, with state reduction via structural analysis. For small units, the tool was able to generate a formal proof of correctness. For larger units, the tool automatically generated and used coverage statistics and used them to exhaustively search over as diverse a state space as possible. In the case of a bug, a short, easy to debug, counter example would be produced. And hardware testing tools have gotten a lot better since then.

But in software land, I'm lucky if a random project I want to contribute to has tests at all. When tests exist, they're usually handwritten, with all the limitations that implies. Once in a blue moon, I'm pleasantly surprised to find that a software project uses [a test framework](http://hackage.haskell.org/package/QuickCheck) which has 1% of the functionality that was standard a decade ago in chip designs.

Considering the relative cost of [hardware bugs vs. software bugs](https://danluu.com/testing/), it's not too surprising that a lot more effort goes into hardware testing. But here's a case where there's almost no extra effort. You've already got analytics measuring the conversion rate through all sorts of user funnels. The only new idea here is that clicking on an ad or making a purchase isn't the only type of conversion you should measure. Following directions at an intersection is a conversion, not deleting an image immediately after pasting it is a conversion, and using a modal dialogue box after opening it up is a conversion.

Of course, whether it's ad click conversion rates or cache hit rates, blindly optimizing a single number will get you into a local optima that will hurt you in the long run, and setting thresholds for conversion rates that should send you an alert is nontrivial. There's a combinatorially large space of user actions, so it takes judicious use of machine learning to figure out reasonable thresholds. That's going to cost time and effort. But think of all the effort you put into optimizing clicks. You probably figured out, years ago, [that replacing boring text with giant pancake buttons gives you 3x the clickthrough rate](http://www.kalzumeus.com/2009/03/07/how-to-successfully-compete-with-open-source-software/); you're now down to optimizing 1% here and 2% there. That's great, and it's a sign that you've captured all the low hanging fruit. But what do you think the future clickthrough rate is when a user encounters a show-stopping bug that prevents any forward progress on a modal dialogue box?

If this sounds like an awful lot of work, find a known bug that you've fixed, and grep your logs data for users who ran into that bug. Alienating those users by providing a profoundly broken product is doing a lot more to your clickthrough rate than having a hard to find checkout button, and the exact same process that led you to that gigantic checkout button can solve your other problem, too. Everyone knows that adding 200ms of load time can cause 20% of users to close the window. What do you think the effect of exposing them to a bug that takes 5,000ms of user interaction to fix is?

If that's worth fixing, pull out [scalding](https://github.com/twitter/scalding), [dremel](http://research.google.com/pubs/pub36632.html), [cascalog](http://cascalog.org/), or whatever your favorite data processing tool is. Start looking for user actions that don't make sense. Start looking for bugs.

Thanks to Pablo Torres for catching a typo in this post

* * *

1.  It's not that all chip design teams do this systematically (although they should), but that people are looking at the numbers anyway, and will see anomalies. [\[return\]](https://danluu.com/bugalytics/#fnref:A)
2.  Undos aren't just literal undos; pasting an image in and then deleting it afterwards because it shows a busy icon forever counts, too. [\[return\]](https://danluu.com/bugalytics/#fnref:U)
3.  This is worse than it sounds. In addition to producing a busy icon forever in the doc, it disconnects that session from the server, which is another thing that could be detected: it's awfully suspicious if a certain user action is always followed by a disconnection.

    Moreover, both of these failure modes could have been found with fuzzing, since they should never happen. Bugs are hard enough to find that defense in depth is the only reasonable solution.

    [\[return\]](https://danluu.com/bugalytics/#fnref:1)
4.  if you talk to a hardware person, call this verification instead of testing, or they'll think you're talking about DFT, testing silicon for manufacturing defects, or some other weird thing with no software analogue. [\[return\]](https://danluu.com/bugalytics/#fnref:2)