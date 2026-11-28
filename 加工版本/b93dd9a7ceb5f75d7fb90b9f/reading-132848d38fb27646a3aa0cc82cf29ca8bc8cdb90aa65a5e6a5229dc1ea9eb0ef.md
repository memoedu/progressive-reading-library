# 被广泛引用的鼠标与键盘效率对比研究完全是胡说八道｜The widely cited studies on mouse vs. keyboard efficiency are completely bogus

---

## 正文

keyboard 和 mouse，哪个更 faster？许多 programmers 认为，keyboard 对所有（programming-related）tasks 来说都更 faster。然而，AskTog 上有几篇被广泛 cited 的网页声称，Apple studies 表明，做任何事情时，使用 mouse 都比使用 keyboard 更 faster，而那些认为使用 keyboard 更 faster 的人只是在自欺欺人。这听起来可能很极端，但举个例子，其中一篇网页说，作者“从未见过 \[keyboard\] outperform mouse”。

但 mouse 不可能做任何事情都更 faster——几乎没有人用 mouse 在 on-screen keyboard 上 clicking，会比在 physical keyboard 上 typing 更 faster。反过来，有些 tasks 确实是 mice 比 keyboards 更 better suited 的（例如，在 FPS games 中 aiming）。对于没有预设立场的人来说，问题不应该是哪个在所有 tasks 上都更 faster，而应该是：哪些 tasks 使用 keyboard 更 faster，哪些使用 mouse 更 faster，哪些在两者一起使用时更 faster？

你可能会问，这些事情重要吗？这要看情况！我认识的最优秀的 programmrers 之一是个 hunt-and-peck typist，因此，显然，即使没有特别 quick 的 input speed，也可能成为出色的 programmer。但我目前正在做一项简单的 data munging task，我需要 type 大量枯燥的 code，而这项 task 受限于我 type 的 speed。如果我能更 quicker，这项 task 就能更 quicker 完成，而且有些我现在不做的 tasks，我也可能会去做。我的 type 速度可以达到 > 100 wpm，这还不错，但我说话可以达到 > 400 wpm，而我思考的速度又远远快于说话。即使说话时，我也经常是 rate limited 的；typing 就更糟了，而我在 navigation 上这里花半秒、那里花一秒，显然也无济于事。刚进入 tech 行业时，我担任过一个平凡的 test/verification/QA 职位，主要工作是 triage test failures。甚至在开始 automating tasks 之前，我每天能够 triage 的 bugs 就已经接近同一职位其他人的两倍，因为我很重视 basic navigation tasks 的 efficient 程度。如今，我的工作不再有 90% 都是机械重复的内容，但我估计，我在 computer 前花的时间中，大约三分之一都用在了不需要动脑的 tasks 上，而这些 tasks 都被我的 input 和 navigation speed 所 rate-limited。如果我能在这些平凡的 tasks 上更 faster，少花些时间在它们上面，多花些时间做有趣的事情，那就太好了。

总之，我们先看看那些被 cited 的 studies，了解 mouse 到底在哪些情况下确实更 faster。web 上的大多数 references，如果一路追溯到底，都会指向 AskTog，这是 [Bruce Tognazzini](https://en.wikipedia.org/wiki/Bruce_Tognazzini) 的网站，他称自己是“human/computer interaction design 领域公认的领军人物”。

[这个话题上被 cited 最多的 AskTog 网页声称，他们在 R&D 上花了 $50M of R&D and done all kinds of studies](http://www.asktog.com/TOI/toi06KeyboardVMouse1.html); the page claims that, among other things, the $50M，结果表明：“Test subjects 一贯 report，keyboarding 比 mousing 更 faster”，以及“stopwatch 一贯 proves，mousing 比 keyboarding 更 faster。”这一 claim 认为，这既 proves 了 mouse 比 keyboard 更 faster，也解释了为什么 programmers 会认为 keyboard 比 mouse 更 faster，尽管它其实更 slower。然而，这个 result 是 unreproducible 的，因为“Tog”不仅没有 cite experiments 的 details，甚至根本没有 describe 这些 experiments，只是作出了一个笼统的 claim。

[第二篇被广泛 cited 的 AskTog 网页](http://www.asktog.com/TOI/toi22KeyboardVMouse2.html)是对上一篇网页的一篇回应所作的回应，它只是重复说，第一篇网页已经 showed，keyboard shortcuts 更 slower。虽然文中充斥着讽刺，例如：“也许这些年来，我们都被误导了。也许那些一再 show，Macintosh users 更 productive、能够更快学会、购买更多 software packages，等等、等等、等等的 independent studies，不知怎么全都有 flawed。也许……”但和之前一样，它没有 cite 任何 actual results。不过，它确实为 mouse 为什么比 keyboard 更 faster 提供了一个 psuedo-scientific explanation：

> Command Keys 并不更 Faster。正如你从我 August 的专栏中所知道的，decide upon 一个 command key 所需的时间，与 access mouse 所需的时间一样长。difference 在于，command-key decision 是一种 high-level cognitive function，不会 generated 任何 long-term memory。因此，在 subjectively 看来，keys 似乎更 faster，而事实上，它们使用起来通常花的时间一样长。
> 
> 由于 mouse acquisition 是一种 low-level cognitive function，user 在 acquisition period 期间不必 abandon primary task 上的 cognitive process。因此，mouse acquirer 能 achieves 更 greater 的 productivity。

这引出了一个问题：在 keyboard 上 typing，为什么就应该与使用 command keys 有所 different？确实，有些人并不 fluent 于 touch typing，他们 type 时必须想一想自己接下来要 press 哪个 key。这些人的确是很 slow 的 typists，甚至可能比那些熟练使用 mouse 通过 on screen keyboard 来 type 的人更 slower。但也有人对 keyboard 很 fluent，能够 type 而无须有意识地思考自己要 press 哪些 keys。这里的 implicit claim 是：人不可能像对用于 typing 的 keyboard 那样，对 command keys 也达到 fluent 的程度。这可能是真的，但我认为这个 claim 极其 implausible，无论从 principle 上看，还是从我观察过那些显然似乎对 command keys 很 fluent 的人的经历来看，都是如此，而且这个 claim 没有 supporting evidence。

[第三篇被广泛 cited 的 AskTog 网页 cite 了一个 experiment](http://www.asktog.com/SunWorldColumns/S02KeyboardVMouse3.html)：作者先 typed 一个 paragraph，然后必须使用 cursor keys 或 mouse，将每个“e”都 replace 为“|”。作者发现，使用 cursor keys 的 average time 是 99.43 seconds，而使用 mouse 的 average time 是 50.22 seconds。文中没有给出 paragraph 的 length 或“e”的 number 等 information。第三篇网页回应的是一位 user，这位 user cite 了一些具体的 editing examples，在这些 examples 中，这位 user 发现自己使用 keyboard 比使用 mouse 更 faster。

根据我做 benchmarking 的经验，绝大多数 microbenchmarks 都会产生 wrong 或 misleading 的 results，因为它们很难被 properly set up；即使已经 properly set up，要理解 microbenchmark results 如何 relate to real-world world results，也需要对这个 domain 有深入理解。因此，对于来自 microbenchmarks 的宽泛 claims，我深表怀疑，除非作者已经展现出对其特定 domain 的 benchmarking 的深入理解；即使如此，我仍会问，他们为什么相信自己的 result 能够 generalizes。microbenchmarks 很难被 properly interpret 这一观点，[在理解 benchmarking 的人中得到广泛认同](https://twitter.com/shipilev/status/709982588673388544)。

所描述的 `e -> |` replacement task 不仅是一个 microbenchmark，而且是一个 artificial 得离奇的 microbenchmark。

根据 result 中给出的 times，这项 task 要么针对的是非常 naive 的 users，要么不允许使用任何 search and replace functionality。这篇 AskTog 专栏回应的是一位提及 editing tasks 的 programmer，因此，除非那位 programmer 被困在一个不允许使用其 editor 的 basic functionality 的 experiment 中，否则这个 microbenchmark 就是 meaningless 的。此外，这项 replacement task 本身也很 unrealistic——人们有多经常会把 `e` replace 为 `|`？

在没有移除那条离奇的 no-search-and-replace restriction 的情况下，我对这项 task 进行了 timed，得到了以下 results：

-   使用 Keyboard shortcut：1.26s
-   M-x，“replace-string”（而不是使用 mapped keyboard shortcut）：2.8s
-   用 mouse 来 Navigate 到 search and replace：5.39s

第一个 result 来自使用 keyboard shortcut。第二个 result 对应的是我在别人的 emacs setup 中可能会采用的做法，因为那里 mapped 的 keyboard shortcuts 不同；emacs 允许你通过 hitting“M-x”，再 typing command 的完整 name，来 run 一个 command。这比直接使用 keyboard shortcut 更 slower 得多，但仍然比使用 mouse 更 faster（至少对这里的我来说如此）。这是否意味着 keyboards 很棒，而 mice 很糟？不，这个 result 几乎完全是 meaningless 的，因为我几乎不花时间做 single-character search-and-replace，因此 single-character search-and-replace 的 speed 是 irrelevant 的。

另外，由于我习惯使用 keyboard，这里的 mouse speed 很可能异常 slow。在这里尤其如此，因为我平时的 editor setup（`emacs -nw`）不允许 mouse usage，所以我最后使用了一个不熟悉的 editor，`TextEdit`，来做 mouse test。每项 task 我只做了一次，以免“练习”这项确切的 task，因为练习可能让 keyboard-shortcut version 不切实际地接近 instantaneous，毕竟非常 quickly 地 hit 一串练熟的 keys 很容易。然而，这也意味着，在使用 mouse 时，我用的是不熟悉的 mouse，操作的也是一组不熟悉的 menus。此外，和许多很久以前玩过 video games 的人一样，我习惯将“[mouse acceleration](https://www.google.com/search?q=mouse+acceleration)”turned off，但 Mac 是默认 turned on 的，而我没有费那番周折去 disable mouse acceleration。另外，我使用的 recording program（quicktime）让整台 machine 都变得 laggy，这对 mousing speed 的影响可能比对 keyboard speed 更大；而我碰巧使用的那个 program，其 menu setup 还迫使我 navigate 两 levels 的 menus。

话虽如此，尽管我不习惯使用 mouse，但如果想找一个我使用 mouse 比使用 keyboard 更 faster 的 microbenchmark，那很容易：让我试试 selecting 一块位于 screen 上、但不在我的 cursor 附近的 block of text：

-   使用 Keyboard：1.8s
-   使用 Mouse：0.7s

我在 emacs 中做 blocks 的 selection 时，通常会先 searching block 的 start 处的某些内容，setting 一个 mark，然后再 searching mark 的 end 处的某些内容。我通常会 type 三个 characters，以确保得到一个 unique chunk of text（如果我觉得三个 characters 不够，就会 type 更多）。这让 selection task 比 replacement task 略微 slower，因为 replacement task 使用的是 single characters，而这项 task 使用的是 multiple characters。

mouse 对 selecting a block of text 实在是更 better suited，以至于即便 mouse setup 不熟悉，导致我最后必须 make a correction，而无法用 one motion 完成 selection，mouse 仍然快了两倍多。但如果我想 select 的内容在 off screen，而且 selection 大到无法容纳在一个 screen 上，那么 keyboard time 不会 change，而 mouse time 会变得 slower 得多，使 keyboard 更 faster。

除了进行 measurements，我还（非正式地）polled 了一些人，询问他们认为在具体 tasks 上，keyboard 和 mouse 哪个会更 faster。search-and-replace 和 select-text 都是大多数人觉得 result 很明显的 tasks。但并非所有 tasks 的 result 都很明显；对于 scrolling，人们就没有强烈倾向任何一方的看法。我们来看看 scrolling，这是一项 keyboard 和 mouse 都很 well suited 的 task。为了具体一些，我们来看看 scrolling down 4 pages：

-   使用 Keyboard：0.49s
-   使用 Mouse：0.57s

虽然存在一些 difference，而且我怀疑，如果 repeated 这个 experiment 足够多次，我就能得到一个 statistically significant result，但这个 difference 小到并没有 practical significance。

与 Tog 的 result 相反——他的 result 是，尽管 mouse 更 faster，所有人却都认为 keyboard 更 faster——我发现，人们相当善于 estimating 哪种 device 对哪些 tasks 更 faster，也善于 estimate 两种 devices 何时会给出 similar result。一个可能的原因是，我 polling 的是 programmers，尤其是 [RC](https://www.recurse.com/scout/click?t=b504af89e87b77920c9b60b2a1f6d5e8) 的 programmers，他们与 Tog 在他的 studies 中可能 studied 的人，大概属于不同的 population。他所在的一个小组在 80s 研究如何为 general purpose computer design UI；在当时，若把 studying 的重点放在这样一些人身上，实际上并不合理：他们中的许多人从小就 using computers，随后又选择了整天都要 use computers 的职业。与之 equivalent 的 population 必须早在 60s 甚至更早就开始 using computers，但即便如此，当时的 input devices 也很不一样（ball mouse 直到 1972 才被发明，而且显然并不是一经发明就得到 wide use）。studying 那些在 using computer input devices 方面并不算 expert 的 populations 本身没有问题，但把那些 results generalizing 到相对 expert 的人身上，就有问题了。

与 keyboard 或 mouse 倡导者的 claims 不同，当我自己做 experiments 时，results 是 mixed。有些 tasks 在我 use keyboard 时会 substantially faster，另一些则在我 use mouse 时会 substantially faster。而且，大多数 results 都是 easily predictable 的（当 results 是 similar 时，prediction 就是它会很难 predict）。如果我们看看网上最广为 cited、最 authoritative 的 results，就会发现，它们十分强烈地 claims mouse 比 keyboard 要 faster 得多，但用来 back up 这一 claim 的，只有一个 bogus experiment。那些被大肆宣扬的 $50M R&D 经费，可能确实有一部分投入了 valid experiments，但那些 experiments 即便存在，也没有被 cited。

我花了一些时间 reviewing 这个主题的 literature，但没能找到任何 conclusive 的内容。我不打算对每项 study 做 point-by-point summary（[就像我在这里针对另一个有争议的话题所做的那样](https://danluu.com/empirical-pl/)），而是会提及使这些 studies 对我而言 irrelevant 的 high-level issues。我能找到的所有 studies，都至少有一个下列 issues；如果你有某项 study 的链接，而它不会因为以下某个原因而变得 irrelevant，我很愿意了解！

1.  study 的年代：一项来自 mid-80s、关于 interacting with computers 的 study，如何 transfers 到如今人们 interact with computers 的方式，尚不清楚。即便忽略 editing programs 的 differences，interface 也有很大的 differences。Mice 更 precise，一只像样的现代 optical mouse 可以以人手能达到的最快速度被 moved，而 tracking 不会变得 erratic；我试过的任何一只 80s 的 mouse 都做不到这一点，而 90s 的 high quality mice 也只有在 balls 刚清洁过、mouse 放在质量不错的 mousepad 上时，才能做到这一点。Keyboards 的改进没有那么大，但即便如此，我在现代 low-travel keyboard 上 type，也能比在我试过的任何一款 80s keyboard 上 substantially faster。
2.  Narrow microbenchmarking：这些并不全都像那个不允许 search and replace 的 `e -> |` task 那样 irrelevant，但即便是那些并非明显 irrelevant 的 tasks，也不清楚其 result 对我可能实际开展的工作有什么 impact。
3.  并非 keyboard vs. mouse：已 published studies 中，只有极小一部分研究 keyboard vs. mouse interaction。当一项 study 研究 device interaction 时，它通常研究的是某种新的 device，或一种新的 interaction model。
4.  Vague description：许多 studies 会说，它们发现了 7.8% 的 improvement，results 在 p < 0.005 下是 significant 的，诸如此类，却没有提供足够的 information，让人判断 results 是确实 significant，还是仅仅 statistically significant（回想一下，那个 practically insignificant 的 scrolling result，其 difference 为 0.08s，也可以被 reported 为 16.3% 的 improvement）。
5.  Unskilled users：在一篇典型的 paper 中，他们指出，users 将 mouse 从 screen 的一侧 move 到另一侧的 scrollbar，可能需要长达两秒。为了弄清哪些 interfaces 对使用起来最吃力的 users 而言最容易，对 unskilled users 开展 studies 自有其价值；但一项针对需要 2 seconds 才能把 mouse 移到 scrollbar 上的 users 的 study，似乎与我的 user experience 并不 relevant。我自己 timed 这项操作时，从 screen 另一侧到达 scrollbar 并 scroll 一小段距离，只花了 0.21s，尽管我用的是不熟悉的 mouse，其 sensitivity 也与我习惯的不同，而且还运行着一个 recording program，使 mousing 比平时更困难。
6.  看似 unreasonable 的 results：有些 studies claim，它们表明，从一种 device switching 到另一种时，overall productivity 会有很大的 improvements（例如，switching mice 的 types 能带来 20% 的 total productivity gain）。

#### Conclusion（结论）

Tog 的 org 花了 $50M 开展的那些神秘 studies，完全有可能 prove：除了 raw text input 之外，mouse 在所有 tasks 上都比 keyboard 更 faster，但现有 information 似乎不足以让人弄清实际开展了哪些 studies。关于 user input 的 public studies 有很多，但我没能找到任何一项，与我是否应该在 at the margin 更多或更少地 use mouse 这个问题 relevant。

当我自己考察各种 tasks 时，results 是 mixed，而且 mixed 的方式正如我所 polling 的大多数 programmers 所 predicted 的那样。这个 result 实在太无聊了，要不是有大批人相信 keyboard 总是比 mouse 更 faster，或者反过来，它几乎不值得一提。

如果这个主题有我应该阅读的 relevant studies，请告诉我！我不熟悉 relevant fields，所以有可能我 searching 时用错了 keywords，读的也是不对的 papers。

#### Appendix：给自己的备忘

我之前没有意识到，scrolling relative to searching 竟然这么快（这篇 blog post 中没有明确提到 searching，但它占了 text selection task 的 1/2）。我通常会使用 search 来 scroll 到 offscreen 的内容，但看来，当我不想把 cursor 放在某个特定的 position 时，我应该考虑改用 scrolling。

感谢 Leah Hanson、Quentin Pradet、Alex Wilson 和 Gaxun 对这篇文章提出的评论与修正，也感谢 Annie Cherkaev、Chris Ball、Stefan Lesser 和 David Isaac Lee 参与相关讨论。

---

## 原文

Which is faster, keyboard or mouse? A large number of programmers believe that the keyboard is faster for all (programming-related) tasks. However, there are a few widely cited webpages on AskTog which claim that Apple studies show that using the mouse is faster than using the keyboard for everything and that people who think that using the keyboard is faster are just deluding themselves. This might sound extreme, but, just for example, one page says that the author has “never seen \[the keyboard\] outperform the mouse”.

But it can’t be the case that the mouse is faster for everything — almost no one is faster at clicking on an on-screen keyboard with a mouse than typing at a physical keyboard. Conversely, there are tasks for which mice are much better suited than keyboards (e.g., aiming in FPS games). For someone without an agenda, the question shouldn’t be, which is faster at all tasks, but which tasks are faster with a keyboard, which are faster with a mouse, and which are faster when both are used?

You might ask if any of this matters. It depends! One of the best programmrers I know is a hunt-and-peck typist, so it's clearly possible to be a great programmer without having particularly quick input speed. But I'm in the middle of an easy data munging task where I'm limited by the speed at which I can type in a large amount of boring code. If I were quicker, this task would be quicker, and there are tasks that I don't do that I might do. I can type at > 100 wpm, which isn't bad, but I can talk at > 400 wpm and I can think much faster than I can talk. I'm often rate limited even when talking; typing is much worse and the half-a-second here and one-second there I spent on navigation certainly doesn't help. When I first got started in tech, I had a mundane test/verification/QA role where my primary job was to triage test failures. Even before I started automating tasks, I could triage nearly twice as many bugs per day as other folks in the same role because I took being efficient at basic navigation tasks seriously. Nowadays, my jobs aren't 90% rote anymore, but my guess is that about a third of the time I spend in front of a computer is spent on mindless tasks that are rate-limited by my input and navigation speed. If I could get faster at those mundane tasks and have to spend less time on them and more time doing things that are fun, that would be great.

Anyway, to start, let’s look at the cited studies to see where the mouse is really faster. Most references on the web, when followed all the way back, point to the AskTog, a site by [Bruce Tognazzini](https://en.wikipedia.org/wiki/Bruce_Tognazzini), who describes himself as a "recognized leader in human/computer interaction design".

[The most cited AskTog page on the topic claims that they've spent $50M of R&D and done all kinds of studies](http://www.asktog.com/TOI/toi06KeyboardVMouse1.html); the page claims that, among other things, the $50M in R&D showed “Test subjects consistently report that keyboarding is faster than mousing” and “The stopwatch consistently proves mousing is faster than keyboarding. ”. The claim is that this both proves that the mouse is faster than the keyboard, and explains why programmers think the keyboard is faster than the mouse even though it’s slower. However, the result is unreproducible because “Tog” not only doesn’t cite the details of the experiments, Tog doesn’t even describe the experiments and just makes a blanket claim.

The [second widely cited AskTog page](http://www.asktog.com/TOI/toi22KeyboardVMouse2.html) is in response to a response to the previous page, and it simply repeats that the first page showed that keyboard shortcuts are slower. While there’s a lot of sarcasm, like “Perhaps we have all been misled these years. Perhaps the independent studies that show over and over again that Macintosh users are more productive, can learn quicker, buy more software packages, etc., etc., etc., are somehow all flawed. Perhaps....” no actual results are cited, as before. There is, however, a psuedo-scientific explanation of why the mouse is faster than the keyboard:

> Command Keys Aren’t Faster. As you know from my August column, it takes just as long to decide upon a command key as it does to access the mouse. The difference is that the command-key decision is a high-level cognitive function of which there is no long-term memory generated. Therefore, subjectively, keys seem faster when in fact they usually take just as long to use.
> 
> Since mouse acquisition is a low-level cognitive function, the user need not abandon cognitive process on the primary task during the acquisition period. Therefore, the mouse acquirer achieves greater productivity.

One question this raises is, why should typing on the keyboard be any different from using command keys? There certainly are people who aren’t fluent at touch typing who have to think about which key they’re going to press when they type. Those people are very slow typists, perhaps even slower than someone who’s quick at using the mouse to type via an on screen keyboard. But there are also people who are fluent with the keyboard and can type without consciously thinking about which keys they’re going to press. The implicit claim here is that it’s not possible to be fluent with command keys in the same way it’s possible to be fluent with the keyboard for typing. It’s possible that’s true, but I find the claim to be highly implausible, both in principle, and from having observed people who certainly seem to be fluent with command keys, and the claim has no supporting evidence.

[The third widely cited AskTog page cites a single experiment](http://www.asktog.com/SunWorldColumns/S02KeyboardVMouse3.html), where the author typed a paragraph and then had to replace every “e” with a “|”, either using cursor keys or the mouse. The author found that the average time for using cursor keys was 99.43 seconds and the average time for the mouse was 50.22 seconds. No information about the length of the paragraph or the number of “e”s was given. The third page was in response to a user who cited specific editing examples where they found that they were faster with a keyboard than with a mouse.

My experience with benchmarking is that the vast majority of microbenchmarks have wrong or misleading results because they’re difficult to set up properly, and even when set up properly, understanding how the microbenchmark results relate to real-world world results requires a deep understanding of the domain. As a result, I’m deeply skeptical of broad claims that come from microbenchmarks unless the author has a demonstrated, deep, understanding of benchmarking their particular domain, and even then I’ll ask why they believe their result generalizes. The opinion that microbenchmarks are very difficult to interpret properly is [widely shared among people who understand benchmarking](https://twitter.com/shipilev/status/709982588673388544).

The `e -> |` replacement task described is not only a microbenchmark, it's a bizarrely artificial microbenchmark.

Based on the times given in the result, the task was either for very naive users, or disallowed any kind of search and replace functionality. This particular AskTog column is in response to a programmer who mentioned editing tasks, so the microbenchmark is meaningless unless that programmer is trapped in an experiment where they’re not allowed to use their editor’s basic functionality. Moreover, the replacement task itself is unrealistic — how often do people replace `e` with `|`?

I timed this task without the bizarre no-search-and-replace restriction removed and got the following results:

-   Keyboard shortcut: 1.26s
-   M-x, “replace-string” (instead of using mapped keyboard shortcut): 2.8s
-   Navigate to search and replace with mouse: 5.39s

The first result was from using a keyboard shortcut. The second result is something I might do if I were in someone else’s emacs setup, which has different keyboard shortcuts mapped; emacs lets you run a command by hitting “M-x” and typing the entire name of the command. That’s much slower than using a keyboard shortcut directly, but still faster than using the mouse (at least for me, here) Does this mean that keyboards are great and mice are terrible? No, the result is nearly totally meaningless because I spend almost none of my time doing single-character search-and-replace, making the speed of single-character search-and-replace irrelevant.

Also, since I’m used to using the keyboard, the mouse speed here is probably unusually slow. That’s doubly true here because my normal editor setup (`emacs -nw`) doesn’t allow for mouse usage, so I ended up using an unfamiliar editor, `TextEdit`, for the mouse test. I did each task once in order to avoid “practicing” the exact task, which could unrealistically make the keyboard-shortcut version nearly instantaneous because it’s easy to hit a practiced sequence of keys very quickly. However, this meant that I was using an unfamiliar mouse in an unfamiliar set of menus for the mouse. Furthermore, like many people who’ve played video games in the distant past, I’m used to having “[mouse acceleration](https://www.google.com/search?q=mouse+acceleration)” turned off, but the Mac has this on by default and I didn’t go through the rigmarole necessary to disable mouse acceleration. Additionally, recording program I used (quicktime) made the entire machine laggy, which probably affects mousing speed more than keyboard speed, and the menu setup for the program I happened to use forced me to navigate through two levels of menus.

That being said, despite not being used to the mouse, if I want to find a microbenchmark where I’m faster with the mouse than with the keyboard, that’s easy: let me try selecting a block of text that’s on the screen but not near my cursor:

-   Keyboard: 1.8s
-   Mouse: 0.7s

I tend to do selection of blocks in emacs by searching for something at the start of the block, setting a mark, and then searching for something at the end of the mark. I typically type three characters to make sure that I get a unique chunk of text (and I’ll type more if it’s text where I don’t think three characters will cut it). This makes the selection task somewhat slower than the replacement task because the replacement task used single characters and this task used multiple characters.

The mouse is so much better suited for selecting a block of text that even with an unfamiliar mouse setup where I end up having to make a correction instead of being able to do the selection in one motion, the mouse is over twice as fast. But, if I wanted select something that was off screen and the selection was so large that it wouldn’t fit on one screen, the keyboard time wouldn’t change and the mouse time would get much slower, making the keyboard faster.

In addition to doing the measurements, I also (informally) polled people to ask if they thought the keyboard or the mouse would be faster for specific tasks. Both search-and-replace and select-text are tasks where the result was obvious to most people. But not all tasks are obvious; scrolling was one where people didn’t have strong opinions one way or another. Let’s look at scrolling, which is a task both the keyboard and the mouse are well suited for. To have something concrete, let’s look at scrolling down 4 pages:

-   Keyboard: 0.49s
-   Mouse: 0.57s

While there’s some difference, and I suspect that if I repeated the experiment enough times I could get a statistically significant result, but the difference is small enough that the difference isn’t of practical significance.

Contra Tog’s result, which was that everyone believes the keyboard was faster even though the mouse is faster, I find that people are pretty good at estimating what’s which device is faster for which tasks and also at estimate when both devices will give a similar result. One possible reason is that I’m polling programmers, and in particular, programmers at [RC](https://www.recurse.com/scout/click?t=b504af89e87b77920c9b60b2a1f6d5e8), who are probably a different population than whoever Tog might’ve studied in his studies. He was in a group that was looking at how to design the UI for a general purpose computer in the 80s, where it would have been actually been unreasonable to focus on studying people, many of whom grew up using computers, and then chose a career where you use computers all day. The equivalent population would’ve had to start using computers in the 60s or even earlier, but even if they had, input devices were quite different (the ball mouse wasn’t invented until 1972, and it certainly wasn’t in wide use the moment it was invented). There’s nothing wrong with studying populations who aren’t relatively expert at using computer input devices, but there is something wrong with generalizing those results to people who are relatively expert.

Unlike claims by either keyboard or mouse advocates, when I do experiments myself, the results are mixed. Some tasks are substantially faster if I use the keyboard and some are substantially faster if I use the mouse. Moreover, most of the results are easily predictable (when the results are similar, the prediction is that it would be hard to predict). If we look at the most widely cited, authoritative, results on the web, we find that they make very strong claims that the mouse is much faster than the keyboard but back up the claim with nothing but a single, bogus, experiment. It’s possible that some of the vaunted $50M in R&D went into valid experiments, but those experiments, if they exist, aren’t cited.

I spent some time reviewing the literature on the subject, but couldn’t find anything conclusive. Rather than do a point-by-point summary of each study ([like I did here for here for another controversial topic](https://danluu.com/empirical-pl/)), I’ll mention the high-level issues that make the studies irrelevant to me. All studies I could find had at least one of the issues listed below; if you have a link to a study that isn’t irrelevant for one of the following reasons, I’d love to hear about it!

1.  Age of study: it’s unclear how a study on interacting with computers from the mid-80s transfers to how people interact with computers today. Even ignoring differences in editing programs, there are large differences in the interface. Mice are more precise and a decent modern optical mouse can be moved as fast as a human can move it without the tracking becoming erratic, something that isn’t true of any mouse I’ve tried from the 80s and was only true of high quality mice from the 90s when the balls were recently cleaned and the mouse was on a decent quality mousepad. Keyboards haven’t improved as much, but even so, I can type substantially faster a modern, low-travel, keyboard than on any keyboard I’ve tried from the 80s.
2.  Narrow microbenchmarking: not all of these are as irrelevant as the `e -> |` without search and replace task, but even in the case of tasks that aren’t obviously irrelevant, it’s not clear what the impact of the result is on actual work I might do.
3.  Not keyboard vs. mouse: a tiny fraction of published studies are on keyboard vs. mouse interaction. When a study is on device interaction, it’s often about some new kind of device or a new interaction model.
4.  Vague description: a lot of studies will say something like they found a 7.8% improvement, with results being significant with p < 0.005, without providing enough information to tell if the results are actually significant or merely statistically significant (recall that the practically insignificant scrolling result was a 0.08s difference, which could also be reported as a 16.3% improvement).
5.  Unskilled users: in one, typical, paper, they note that it can take users as long as two seconds to move the mouse from one side of the screen to a scrollbar on the other side of the screen. While there’s something to be said for doing studies on unskilled users in order to figure out what sorts of interfaces are easiest for users who have the hardest time, a study on users who take 2 seconds to get their mouse onto the scrollbar doesn’t appear to be relevant to my user experience. When I timed this for myself, it took 0.21s to get to the scrollbar from the other side of the screen and scroll a short distance, despite using an unfamiliar mouse with different sensitivity than I’m used to and running a recording program which made mousing more difficult than usual.
6.  Seemingly unreasonable results: some studies claim to show large improvements in overall productivity when switching from type of device to another (e.g., a 20% total productivity gain from switching types of mice).

#### Conclusion

It’s entirely possible that the mysterious studies Tog’s org spent $50M on prove that the mouse is faster than the keyboard for all tasks other than raw text input, but there doesn’t appear to be enough information to tell what the actual studies were. There are many public studies on user input, but I couldn’t find any that are relevant to whether or not I should use the mouse more or less at the margin.

When I look at various tasks myself, the results are mixed, and they’re mixed in the way that most programmers I polled predicted. This result is so boring that it would barely be worth mentioning if not for the large groups of people who believe that either the keyboard is always faster than the mouse or vice versa.

Please let me know if there are relevant studies on this topic that I should read! I’m not familiar with the relevant fields, so it’s possible that I’m searching with the wrong keywords and reading the wrong papers.

#### Appendix: note to self

I didn't realize that scrolling was so fast relative to searching (not explicitly mentioned in the blog post, but 1/2 of the text selection task). I tend to use search to scroll to things that are offscreen, but it appears that I should consider scrolling instead when I don't want to drop my cursor in a specific position.

Thanks to Leah Hanson, Quentin Pradet, Alex Wilson, and Gaxun for comments/corrections on this post and to Annie Cherkaev, Chris Ball, Stefan Lesser, and David Isaac Lee for related discussion.