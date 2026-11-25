# In defense of blub studies

---

## 正文

有时有人问我，应该学些什么才能成为更好的 programmer。我觉得，这时的默认建议通常是一门冷僻的 programming language，或一本介绍 ML 之类强大 machinery 的教科书。因此，当我反而建议深入钻研你已经了解的东西时，我总觉得有些不好意思，也有些无趣：你的主要 programming language、web framework、object-relational mapper、UI library、version control system、database、Unix tools 等等。这并不光鲜，也不玄奥，但对我而言，为这些东西建立详细的 mental model（以及了解它们如何与 alternatives 比较），可能是对我作为 engineer 的 effectiveness 贡献最大的学习。

一位同事创造了“blub studies”这个说法，用来指这种平凡、看起来极其具体的 knowledge。“Blub”出自 Paul Graham 的一篇文章 [Beating the Averages](http://www.paulgraham.com/avg.html)，其中 Blub 是一种 hypothetical middlebrow language；当 Graham 断言 Lisp 是 superior 时，它的 programmers 就会产生防御心理。Blub studies 研究的是这些乏味、日常的 systems 内部究竟发生了什么——不是那种你靠 inventing 就能获得终身教职的东西，而是人们真正使用的东西。

Blub studies 是一条永无止境的 engineering know-how 跑步机。它涉及繁琐的 technical details：Git 如何 stores data，Postgres locking semantics 如何[导致你的 migration bring down prod](https://gocardless.com/blog/zero-downtime-postgres-migrations-the-hard-parts/)，或者为什么 `pip install` 在_这_一次 failed。它关心的是你的 computer 的锅炉房里发生了什么。这类东西似乎无穷无尽，到处都是让你栽跟头的 bespoke details，因此往往令人难以置信地沮丧。machine learning 这类光鲜领域的 Experts 会写出听起来同样光鲜的文章，例如 _[A theory of the learnable](https://blog.acolyer.org/2018/01/31/a-theory-of-the-learnable/)_；blub studies 的 experts 则会发出 [The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/) 和 [Programming Sucks](https://www.stilldrinking.org/programming-sucks) 这样的长篇牢骚。

简而言之，如果你寻找的是能够[随时间 compounds exponentially](https://fs.blog/2019/02/compounding-knowledge/) 的 generalizable knowledge，那么 blub studies 看起来就像你为了获得好东西而不得不蹚过的烂泥。因此，不难理解为什么人们会放弃理解身边的所有 blub，只了解完成工作所必需的部分。

但对我而言，相反的态度更有成效。[Computers 是可以被理解的](https://blog.nelhage.com/post/computers-can-be-understood/)——即使这很难，也需要一些时间。Blub studies 比表面看起来更加 generalizable，也有自己的方式随时间 compounding。这使它比你预期的有用得多。✻✻ 当然，blub studies 也有无用的部分：如果这篇文章让你兴奋地想去背诵一堆 command-line flags，不妨考虑[把这个建议反过来](https://slatestarcodex.com/2014/03/24/should-you-reverse-any-advice-you-hear/)。但以我的经验，忽略 blubs 中有用部分的情况，比过度关注琐事更常见。

* * *

blub expertise 最直接的好处，就是为你节省时间。[“如果你没有足够的 knowledge base 和意志力先闯过 `pip install`，就无法 apply 你从 SICP 学到的那些精彩 insights。”](https://twitter.com/geoffreylitt/status/1305214228991750144) 如果你知道 Git 的 internal model 如何 works，就能让你的 repository 脱离 borked state，而不必在 Stack Overflow 上花几个小时。

这种 effect 比看起来更大。如果你使用的是自己不理解的 system，就只能通过 guess-and-check 来 debugging，而这可能 arbitrarily slow。更 efficient 的 method 是[尽可能多地获取有关你的 program 的 execution 的 information](https://twitter.com/b0rk/status/1265360282513281025?lang=en)，然后用这些 information 来 exclude 大部分 hypothesis space。但这要求你既充分理解 system，也充分理解可用于 inspecting 它的 tools。比如，如果你正在 tracking down 一个 networking problem，盯着一些 `tcpdump` output 往往就能让你接近答案，但前提是你知道如何 interpret 它，以及要寻找什么。

如果你把一半 programming 时间花在 debugging 上，而成为 blub expert 能让你 debug 的速度快一倍，那么仅仅 blub expertise 带来的 speed gain，就能让你的 output 增加三分之一。†† 如果你觉得“一半 programming 时间用于 debugging”听起来太多，不妨想象一下：如果你的所有 code 第一次就 worked，你会快多少。

让 debugging speed 翻倍，可能还是个 conservative estimate——通过类似[“嗯，40 milliseconds 听起来像是 Nagle’s algorithm 的 timeout，试试 setting `TCP_NODELAY`”](http://rachelbythebay.com/w/2020/10/14/lag/) 这样的判断，你几乎可以节省无限的时间。我相当经常能以比同事快 5x+ 的速度 debug 棘手的东西，只因为我使用我们的 stack 已经很久，知道去哪里找 problems，以及如何 quickly test hypotheses。

这就足以证明，花大量时间盯着 `tcpdump` output 是值得的！但我从 blub studies 获益如此之多，还有更微妙的原因。它比我预期的更加 general，持续得更久，也有更强的 compounding effect。

* * *

Blub studies 的 applicable 范围广得令人意外，因为即使你学的是某个具体 blubby system 的 details，那个 system 的 design 中也会包含一个有料的 non-blubby core，其中有可以 extract 的 general principles。人们试图教给你的许多“general principles”未必如此，但你通过 blub studies 学到的那些，保证至少对一个 real-world system 很重要（就是你正在学习的那个）。而且，你会看到它们 realized 时的所有杂乱 detail，这往往是 academic presentations 省略的部分。

假设你选择的 blub 是 React。你可能担心，如果以后转向 stack 的其他部分，甚至只是另一种 web framework，那么学习那些繁琐 details 就没有用了。是的，其中一些确实会如此。但 React 的 core idea——writing pure render functions，使用 [reconciliation](https://reactjs.org/docs/reconciliation.html) 让 updates 变得 fast——极其 powerful，也极其 general。事实上，如今 iOS（[SwiftUI](https://developer.apple.com/xcode/swiftui/https://developer.apple.com/xcode/swiftui/)）和 Android（[Jetpack Compose](https://developer.android.com/jetpack/compose)）上的下一代 UI frameworks 都已经 copied 了它。学习 React 背后的 principles，会让学习其他那些 frameworks 更容易。事实上，它甚至可以成为有用的 ideas 来源，让你把 ideas 从一个“import”到另一个。例如，在 Wave，我们把 [Relay](https://relay.dev/) 中的 ideas importing 到我们的 mobile apps 中，获益良多。

这是一个很好的例子：据我所知，这种 idea 你_只能_通过 blub studies 学到。Academia 并没有给 React-style UI programming 太多关注。事实上，它似乎根本不把 user-interface programming paradigms 当作特别有趣的研究对象。偶尔确实有人就此 publish，但例如，我在 MIT 内容丰富的 course catalog 中找不到任何相关 courses。‡‡ 你可以说，这是因为 UI programming“太 applied”了，不应该指望 academic curriculum 涵盖它。但 computer science 涵盖了许多同样“applied”的其他领域，例如 networking、databases、operating systems 和 graphics。

* * *

Blub studies 的 compound 程度，也比你直觉预期的更大，具体有两种方式。首先，了解一种 blub，会让你更容易学习服务于相同目的的 alternative blubs——就像上面的 React/SwiftUI 例子。其次，对一种 blub 了解得更多，会帮助你更快地学习 stack 中 _adjacent_ 部分的 blubs。

有一次，我与一位资历较浅的同事一起 pair programming，我们正在 writing 一个 complicated SQLAlchemy query。我的同事使用了 `user.name`（这个 `name` field 所属的 object stored 在 `user` variable 中），而不是 `User.name`（这个 `name` field 所属的是 _class_ `User`），并且不明白为什么她的 query 给出了 wrong results。我试图解释其中的“magic”：为什么 `User.name` 是 `Column` 的一个 instance，而 `user.name` 却是一个 simple `str`。我绕了一会儿圈子，直到最后向她解释了 Python 的 [descriptor protocol](https://docs.python.org/3/howto/descriptor.html)（SQLAlchemy 用来 enable “declarative” ORM syntax 的 language feature）。这时，一切都豁然开朗了——我也意识到，Python 的 `__dunder__` methods 是 decoding 许多看起来“magical”的 code 的关键。如果你充分掌握 Python language features，许多 complicated libraries 就会容易理解得多。

我自己学习 Kubernetes 时也有过类似经历。第一次尝试学它时，眼前是一片令人困惑的 jargon 泥潭——仅仅为了让一个 simple HTTP server running，居然就需要那么多 namespaces、containers、Pods、Deployments、Services 和 Ingresses！后来我读了[一本 networking 教科书](http://intronetworks.cs.luc.edu/)，一切就明白多了。Kubernetes 中（可以说）最 complicated 的部分，是为了解决 networking-related problems 而存在的——让数百个 containers 在 hosted 于少得多的一组 computers 上时，仍能 independently 相互通信——因此，那本 networking 教科书给了我一个 schema，让我能把所有 Kubernetes 零碎知识挂在上面。一旦我知道 Linux 的 IP routing、iptables 和 network namespaces 如何 worked，就更容易理解“kube-proxy”这样的东西究竟在做什么了。

如果你了解足够多种不同的 blubs，最终甚至可以不必查资料，就推断出它们（可能）是如何 implemented 的。一位 experienced Python programmer 能立即猜出 SQLAlchemy 的“declarative” ORM 在内部如何 works。到了这个阶段，你的 blub expertise 才会真正开始 compounding——几乎就在你开始使用新东西的同时，你就会开始弄清它如何 works，并 extracting 那些具有普遍趣味的 ideas 的 kernel。

* * *

由于这种 compounding effect，成为 blub master 最重要的一步，就是想尽办法启动你的“blub flywheel”——blub accumulation 的良性循环。这意味着从最容易学、或最能激起学习动力的 blubs 入手，再从那里向外拓展。对我而言，最容易的起点一直是那些我在日常工作中已经使用的 blubs。我有几个策略，帮助自己从中获得最大收获。

首先，我会试着_比必要的程度钻得更深_。如果我真的很想 ship 某个东西，就很容易抵挡不住诱惑，比如 Google 一条 error message，从 Stack Overflow 上 copy-paste 一个 fix，然后继续忙自己的事。但真正读一读 error message，理解它的意思，再试着弄清那条 Stack Overflow 回答_为什么_ fixed 了我的 problem，往往并不需要多花多少时间。同样，如果我陷入棘手的 yak shave，我会倾向于避免“guess-and-check”式的 debugging，而选择更好地理解自己正在尝试 debug 的 system。例如，为了 track down 自己某个古怪的一次性 networking issue，深入阅读 `tcpdump` 和 `iptables` rules 的 docs，并不总让人觉得值得——但随着时间推移，我遇到的“古怪的一次性 networking issues”已经足够多，这些投入早已获得了许多倍回报。

我的 blub flywheel 的第二部分是 _留意 magic_。每当我使用新的东西时，我都会尝试不断 update 自己对它如何 implemented 的最佳推测 mental model。“好，docs 让我 create 一个 `Ingress`，我猜这大概就是那个 provisions 一个 load balancer 来与我的 backend containers 进行 talk 的 widget？”如果我发现自己错了，就会深入探究并 update。“嗯，我无法从 cluster 外部 ping 那些 pods，那么 load balancer 怎么能与它们 talking 呢？啊哈——它在与 nodes 进行 talking，而且还有一个 NodePort Service 作为第二层 indirection。”如果我完全不知道某个东西可能如何 work，那通常意味着该读一本书了。

为了让 blub studies 更容易，我希望其他 engineers 做的一件事，就是为他们的 software 编写更多、更好的“advanced” documentation。大多数 mature 且 widely-used 的 libraries 都有很好的 tutorials 和 introductory content，但能让人轻松越过“copy-pasting examples”阶段的却少得多。我认为这主要是 organization 和 navigation 的问题；比如，如果我仔细翻阅 [SQLAlchemy docs](https://docs.sqlalchemy.org/en/13/)，其实能找到足够的 information，拼出一个关于这个 system 的良好 mental model，但这些 information 散落在许多不同的 pages 中，而且我不清楚应该按什么顺序阅读。[React docs](https://reactjs.org/docs/hello-world.html) 大概是我所知道在这方面做得最好的，但仍然遗漏了很多内容。

* * *

随着时间推移，我不断探索自己使用的各种看似 magical 的东西的内部，逐渐积累了广泛的知识，了解各种 technical systems 如何 work。这在许多不同方面都帮助了我。它让我更容易跨越 stack 的许多 layers，track down 棘手的 bugs。我可以把新的 languages 和 libraries 与自己已经知道的东西进行 pattern-matching，从而快速学会它们。通过 imitating 我见过的其他 systems，或 reusing 我在不同背景下听说过的 ideas 或 tools，我能获得更好的 software designs 构思。也许最重要的是，它让我有信心：如果遇到棘手的问题，我可以学到足够的知识来 solve 它，而不是觉得自己只能任由一个过于 complex、根本不指望能理解的 system 摆布。

所以，如果你想学些能让自己成为更优秀、更快乐的 programmer 的东西，就问问自己：你最常用的 blub 中，哪些部分让你觉得 magical？然后尝试理解它们如何 work。你会学到比自己想象中更多的东西！

感谢 [David Golden](https://xdg.me/)、[Awais Hussain](https://awais.io/)、[Will Larson](https://lethain.com/)、[Dan Luu](https://danluu.com/)、[Kevin Lynagh](https://kevinlynagh.com/)、[Caleb Ontiveros](https://www.calebontiveros.com/) 和 [Lincoln Quirk](https://www.lincolnquirk.com/) 对本文草稿提出意见。

---

## 原文

Sometimes people ask me what they should learn to become a better programmer. I feel like the default recommendation here is usually an obscure programming language or a textbook on some high-powered machinery like ML. So I always feel a little bit embarrassed and boring when I instead suggest going really deep on what you already know: your main programming language, web framework, object-relational mapper, UI library, version control system, database, Unix tools, etc. It’s not shiny or esoteric, but for me, building a detailed mental model of those (and how they compare to alternatives) might be the learning that’s contributed most to my effectiveness as an engineer.

A coworker coined the phrase “blub studies” to refer to this sort of mundane, ultra-specific-seeming knowledge. “Blub” comes from a Paul Graham essay, [Beating the Averages](http://www.paulgraham.com/avg.html), in which Blub is a hypothetical middlebrow language whose programmers get defensive when Graham asserts that Lisp is superior. Blub studies is the study of what goes on in the guts of these boring, everyday systems—not the kind you get tenure for inventing, but the kind people actually use.

Blub studies is a never-ending treadmill of engineering know-how. It’s the fiddly technical details of how Git stores data, or how Postgres locking semantics [caused your migration to bring down prod](https://gocardless.com/blog/zero-downtime-postgres-migrations-the-hard-parts/), or why `pip install` failed _this_ time. It’s what goes on inside the boiler rooms of your computer. There’s a seemingly infinite amount of it, full of bespoke details for you to stumble over, and that makes it, often, unbelievably frustrating. Experts in shiny fields like machine learning write shiny-sounding articles like _[A theory of the learnable](https://blog.acolyer.org/2018/01/31/a-theory-of-the-learnable/)_; experts in blub studies emit screeds like [The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/) and [Programming Sucks](https://www.stilldrinking.org/programming-sucks).

In short, if you’re in search of generalizable knowledge that [compounds exponentially over time](https://fs.blog/2019/02/compounding-knowledge/), then blub studies looks like the crap you have to wade through to get to the good stuff. So it’s easy to see why people give up on understanding all the blub they’re surrounded by, except what they need to get the job done.

But for me, the opposite attitude has been more productive. [Computers can be understood](https://blog.nelhage.com/post/computers-can-be-understood/)—even if it’s hard and takes a while. Blub studies is more generalizable than it seems, and has its own way of compounding over time, too. That makes it a lot more useful than you’d expect.✻✻ Of course, there are useless parts of blub studies: if this essay gets you excited to memorize a bunch of command-line flags, consider [reversing this advice](https://slatestarcodex.com/2014/03/24/should-you-reverse-any-advice-you-hear/). But in my experience, it’s more common to neglect the useful parts of blubs, than to over-index on trivia.

* * *

The most straightforward benefit of blub expertise is that it saves you time. [“You can’t apply those brilliant insights you learned from SICP if you don’t have the knowledge base and emotional fortitude to fight through `pip install` first.”](https://twitter.com/geoffreylitt/status/1305214228991750144) If you know how Git’s internal model works, you can get your repository out of its borked state without spending hours on Stack Overflow.

This effect is larger than it might seem. If you’re working with a system you don’t understand, you’re limited to debugging via guess-and-check, which can be arbitrarily slow. A more efficient method would be to [get as much information as possible about your program’s execution](https://twitter.com/b0rk/status/1265360282513281025?lang=en) and then use that information to exclude most of the hypothesis space. But this requires a good understanding of both the system, and the tools available for inspecting it. If you’re tracking down, say, a networking problem, staring at some `tcpdump` output will often get you most of the way there, but only if you know how to interpret it and what to look for.

If you spend half your programming time debugging, and being a blub expert lets you debug twice as fast, then just the speed gain from blub expertise will let you increase your output by a third.†† If you think “half of programming time debugging” sounds high, imagine how much faster you’d be if all your code worked the first time.

Doubling debugging speed is probably a conservative estimate—you can save pretty much unlimited time via things like [“hmm, 40 milliseconds sounds like the timeout for Nagle’s algorithm, try setting `TCP_NODELAY`”](http://rachelbythebay.com/w/2020/10/14/lag/). I somewhat frequently debug tricky things 5x+ faster than coworkers, just because I’ve been working with our stack for a long time, so I know where to look for problems and how to quickly test hypotheses.

That justifies a lot of time staring at `tcpdump` output! But there are also more subtle reasons I’ve gotten so much from blub studies. It’s both more general, lasts longer, and has more of a compounding effect, than I expected.

* * *

Blub studies are surprisingly broadly applicable because, even if you’re learning about the details of some specific blubby system, that system’s design will contain a juicy non-blubby core of extractible general principles. Unlike many “general principles” people try to teach you, the ones you learn via blub studies are guaranteed to be important to at least one real-world system (the one you’re learning about). And you’ll see them realized in all their messy detail, which academic presentations often leave out.

Suppose your blub of choice is React. You might worry that learning the gory details will be useless if you ever move to a different part of the stack, or even a different web framework. And, yes, some of them will. But the core idea of React—writing pure render functions, using [reconciliation](https://reactjs.org/docs/reconciliation.html) to make updates fast—is extremely powerful and general. In fact, it’s now been copied by the next generation of UI frameworks on both iOS ([SwiftUI](https://developer.apple.com/xcode/swiftui/https://developer.apple.com/xcode/swiftui/)) and Android ([Jetpack Compose](https://developer.android.com/jetpack/compose)). Learning the principles behind React makes it easier to learn those other frameworks. In fact, it can even be a useful source of ideas to “import” from one to the other. At Wave, for instance, we’ve gotten a lot of mileage out of importing ideas from [Relay](https://relay.dev/) into our mobile apps.

This is a good example of an idea that, as far as I know, you can _only_ learn about through blub studies. Academia didn’t give much attention to React-style UI programming. In fact, it doesn’t seem to view user-interface programming paradigms as a particularly interesting object of study at all. People do sometimes publish on it but, for instance, I couldn’t find any courses on it in MIT’s extensive course catalog.‡‡ You could argue that this is because UI programming is “too applied” and one shouldn’t expect it to be covered in an academic curriculum. But computer science covers many other equally-“applied” areas, like networking, databases, operating systems, and graphics.

* * *

Blub studies also compound more than you’d naively expect, in two ways. First, knowing about one blub makes it easier to learn about alternative blubs that serve the same purpose—like the React/SwiftUI example above. Second, knowing more about one blub helps you learn blubs in _adjacent_ parts of the stack more quickly.

Once, while pair programming with a more junior coworker, we were writing a complicated SQLAlchemy query. My coworker used `user.name` (the `name` field of an object stored in the `user` variable) instead of `User.name` (the `name` field of the _class_ `User`) and was wondering why her query gave the wrong results. I tried to explain the “magic” by which `User.name` was an instance of `Column` while `user.name` was a simple `str`. I went around in circles for a little while until I eventually explained Python’s [descriptor protocol](https://docs.python.org/3/howto/descriptor.html) to her (the language feature SQLAlchemy uses to enable the “declarative” ORM syntax). At that point, everything clicked—and I realized that Python’s `__dunder__` methods are the key to decoding quite a lot of “magical” seeming code. If you learn the Python language features well, lots of complicated libraries will become a lot easier to understand.

I had a similar experience myself with Kubernetes. The first time I tried to learn it, it was a bewildering morass of jargon—all those namespaces and containers and Pods and Deployments and Services and Ingresses just to get a simple HTTP server running! Then I read [a networking textbook](http://intronetworks.cs.luc.edu/) and everything made much more sense. The (arguably) most complicated parts of Kubernetes exist to solve networking-related problems—allowing hundreds of containers to talk to each other independently while hosted on a much smaller set of computers—so the networking textbook gave me a schema onto which I could hang all my Kubernetes factoids. Once I knew how Linux’s IP routing, iptables, and network namespaces worked, it was much easier for me to understand what exactly something like “kube-proxy” was doing.

If you know enough different blubs, you can end up at the point where you don’t even need to look things up to figure out how they’re (probably) implemented. An experienced Python programmer can guess immediately how SQLAlchemy’s “declarative” ORM works under the hood. That’s the point when your blub expertise will really start compounding—almost as soon as you start working with something new, you’ll start figuring out how it works and extracting the kernel of generally-interesting ideas.

* * *

Because of this compounding effect, the most important step toward becoming a blub master is to kickstart your “blub flywheel”—the virtuous cycle of blub accumulation—however you can. That means starting with whichever blubs are the easiest or most motivating to learn, and branching out from there. For me, the easiest place to start has been with blubs I’m already using at my day job. I have a couple strategies for getting the most out of those.

First, I’ll try to _go deeper than necessary_. If I really want to ship something, it’s easy to give into temptation to, say, Google an error message, copy-paste a fix from Stack Overflow, and move on with my day. But it often doesn’t take that much longer to actually read the error message, understand what it means, and try to figure out _why_ that Stack Overflow answer fixed my problem. Similarly, if I’m stuck in a tricky yak shave, I’ll bias against “guess-and-check” style debugging in favor of getting a better understanding of the system I’m trying to debug. It doesn’t always feel worth it to, e.g., dive into the docs of `tcpdump` and `iptables` rules to track down my weird one-off networking issue—but over time I’ve run into enough “weird one-off networking issues” that it’s paid off many times over.

The second part of my blub flywheel is to _pay attention to magic_. Whenever I’m working with something new, I try to continuously update my best-guess mental model of how it’s implemented. “Okay, the docs are telling me to create an `Ingress`, I guess this is probably the widget that provisions a load balancer to talk to my backend containers?” If I realize I’m wrong, I’ll dig in and update. “Hmm, I can’t ping those pods from outside the cluster, so how could the load balancer be talking to them? Aha—it’s talking to the nodes, and there’s a NodePort Service as a second layer of indirection.” If I have no idea at all how something could work, that usually means it’s time to read a book.

One thing I wish other engineers would do to make blub studies easier is to produce more and better “advanced” documentation of their software. Most mature and widely-used libraries have great tutorials and introductory content, but far fewer make it easy to get past the “copy-pasting examples” stage. I think this is mostly a matter of organization and navigation; if I comb through, say, the [SQLAlchemy docs](https://docs.sqlalchemy.org/en/13/), I can actually find enough information to piece together a good mental model of the system, but it’s scattered among many different pages and it’s not clear what order I should read them in. The [React docs](https://reactjs.org/docs/hello-world.html) are probably the best I know of here, but still leave a lot out.

* * *

Over time, by consistently exploring the guts of anything I’m working with that seems magical, I’ve built up a broad base of knowledge about how various technical systems work. This helps me in tons of different ways. It makes it easier to track down tricky bugs across many layers of the stack. I can learn new languages and libraries quickly by pattern-matching them to what I already know. It gives me better ideas for software designs, by imitating other systems I’ve seen, or by reusing ideas or tools I’ve heard of in a different context. Maybe most importantly, it gives me the confidence that, if I run into a tricky problem, I can learn enough to solve it, instead of feeling like I’m at the mercy of a system too complex to hope to understand.

So if you’re looking to learn something that will make you a better, and happier, programmer, ask yourself which parts of your most-used blub seem magical to you, and try to understand how they work. You’ll learn more than you think!

Thanks to [David Golden](https://xdg.me/), [Awais Hussain](https://awais.io/), [Will Larson](https://lethain.com/), [Dan Luu](https://danluu.com/), [Kevin Lynagh](https://kevinlynagh.com/), [Caleb Ontiveros](https://www.calebontiveros.com/), and [Lincoln Quirk](https://www.lincolnquirk.com/) for commenting on a draft of this post.