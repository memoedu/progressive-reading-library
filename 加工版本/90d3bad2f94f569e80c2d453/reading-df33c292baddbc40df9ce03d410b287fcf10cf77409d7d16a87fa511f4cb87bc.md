# Essays on programming I think about a lot

---

## 正文

每隔一段时间，我都会读到一篇文章，之后反复想起它，也反复在交谈中引用它。

这里列出了我能记起的所有这类文章！以后每当想起更多，我都会尽量更新这份索引。

* * *

Nelson Elhage，[Computers 可以被 understood](https://blog.nelhage.com/post/computers-can-be-understood/)。这篇文章体现的态度，是对我作为 engineer 的 effectiveness 影响最大的因素之一：

> 我对待 software 时，始终深信 computers 和 software systems 是可以被 understood 的。…
> 
> 从某些方面说，这种信念在今天显得 radical。Modern software 和 hardware systems 在许多 distinct layers 之间包含着几乎难以想象的 complexity，每个 layer 都 building atop 其他 layer。…
> 
> 面对这种 complexity，人们很容易觉得要学的东西实在太多，进而采取一种思维捷径：我们使用的 systems 最好被 treated as black boxes，不必在任何 detail 上 understood 它们。
> 
> 我反对这种做法。你永远不可能 understand 那个 stack 上每个 level 的 implementation 的每个 detail；但你可以在某个 level of abstraction 上 understand 它们全部，也可以为了任何目的，将任何特定 layer understand 到基本上所需的任何 depth。

* * *

Dan McKinley，[请选择 Boring Technology](https://mcfunley.com/choose-boring-technology)。当别人问我，我们在 Wave 如何做 technical decisions 时，我就把这篇文章发给他们。它为我省去的心碎与后悔，恐怕比其他任何文章都多：

> 假设每家 company 都有大约三个 innovation tokens。你可以随心所欲地花掉它们，但在很长一段时间里，供给都是 fixed 的。在达到一定程度的 stability 和 maturity 之后，你也许能再得到几个，但人们通常会高估自己钱包里的存货。显然，这个 model 是 approximate 的，但我觉得它很有帮助。
> 
> 如果你选择用 NodeJS 来 write 你的 website，你就花掉了一个 innovation tokens。如果你选择 use MongoDB，你就花掉了一个 innovation tokens。如果你选择 use 仅存在了一年或更短时间的 service discovery tech，你就花掉了一个 innovation tokens。如果你选择 write 自己的 database，天哪，你有麻烦了。

* * *

Sandy Metz，[错误的 Abstraction](https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction)。这篇文章让我相信，“don’t repeat yourself”（DRY）并不是一句好格言。作为建议，它还可以，但正如 Metz 指出的，如果你在 DRYing up 时没有 choose 正确的 interface boundaries，产生的 abstraction 就可能很快变得 unmaintainable：

> 4.  时间流逝。
>     
> 5.  一个新的 requirement 出现了，当前的 abstraction 对它而言几乎是 perfect 的。
>     
> 6.  Programmer B 被安排来 implement 这个 requirement。
>     
>     _Programmer B 觉得自己有义务 retain 现有的 abstraction，但由于它对每个 case 而言并不 exactly the same，他们便 alter the code，让它 take a parameter…._
>     
> 7.  … 进行 Loop，直到 code 变得 incomprehensible。
>     
> 8.  你大约在这里出现在故事中，生活随之急转直下。
>     

* * *

Patrick McKenzie，[Programmers 对 Names 信以为真的 Falsehoods](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)。在 programming 时，从“invariants”的角度思考很有帮助，也就是那些我们 assume 会始终为 true 的 properties。我把这篇文章看作最有力的提醒：_现实中没有 invariants_：

> 32.  人们的 names 是在出生时 assigned 的。
> 33.  好吧，也许不是出生时，但至少离出生很近。
> 34.  好吧，好吧，在出生后大约一年之内。
> 35.  五年？
> 36.  你在开玩笑，对吧？

* * *

Thomas Ptacek，[关于 Hiring 的文章](https://sockpuppet.org/blog/2015/03/06/the-hiring-post/)。这篇文章促使我在 Wave 的 work-sample interview 上投入大量精力，回报极其丰厚——我们 hired 到了比预期强得多的 team，速度也远比预期快。它也很好地提醒了我：大多数人做的大多数事情都毫无道理：

> Alex 的经历中，没有任何迹象表明会发生这样的事。他有 Walter White 的 resume，却有 Heisenberg 的 aptitude。我们谁都没有预料到。我叫 Thomas Ptacek，我认可这个糟糕的双关语。Alex 才是那个 nonced 的人。
> 
> 几年前，Matasano 不可能 hired Alex，因为我们依靠 interviews 和 resumes 来 hire。后来我们做了一些改变，变成了一台能够发现并 recruited 像 Alex 这样的人的机器：在 insurance companies 工作的 line of business .NET developers，查看 code 的第一个小时就找出了 Rails core CVEs。为 phone chipsets 的 assembly firmware 进行 hardware-reversed 的 Sysadmins。顿悟：人才就在那里，但你无法从 resume 上找到他们。
> 
> 我们这个 field 使用一种还不如看鸡内脏占卜的 process 来 selects engineers。和 interviews 一样，家禽肠子几乎无法告诉你是否应该 hire 某个人。但吃它们的体验比 lunch interview 更愉快。

* * *

Gergely Orosz，[具有 Product-Minded 特质的 Engineer](https://blog.pragmaticengineer.com/the-product-minded-engineer/)。我经常把这篇文章发给同事——它极其出色地描述了哪些 traits 能帮助你作为 engineer 在 startup 取得成功：

> 对 product ideas/opinions 持 Proactive 态度 • 对 business、user behavior 及相关 data 有 Interest • 对“为什么？”抱有 Curiosity 和浓厚兴趣 • 是 Strong communicators，并与 non-engineers 保持 great relationships • 提前 Offering product/engineering tradeoffs • 对 edge cases 进行 Pragmatic handling • 采用 Quick product validation cycles • 拥有 End-to-end product feature ownership • 通过反复的 cycles of learning 培养 Strong product instincts

* * *

tef，[Write 易于 delete 的 code，而不是易于 extend 的 code](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to)。_错误的 Abstraction_ 认为，reusable code 除非经过 carefully designed，否则就会变得 unmaintainable。tef 顺理成章地更进一步：为 disposability 而 design，而不是为 maintainability。这篇文章给了我许多有用的 mental models，用来 evaluating software designs。

> 如果我们把‘lines of code’看作‘lines spent’，那么当我们 delete lines of code 时，就是在 lowering maintenance 的 cost。我们不应 building re-usable software，而应该尝试 build disposable software。

> Business logic 是一种 code，其特征是永无止境的一连串 edge cases 和 quick and dirty hacks。这没什么问题。我能接受。其他 styles，比如‘game code’或‘founder code’，也都一样：通过 cutting corners 来节省大量时间。
> 
> 原因呢？有时候，delete 一个大 mistake 比尝试 delete 18 个较小且 interleaved 的 mistakes 更容易。很多 programming 都是 exploratory 的，先弄错几次再 iterate，比费心想着第一次就做对更快。

tef 还写了一篇后续文章，[请 Repeat yourself，做不止一件事，并 rewrite everything](https://programmingisterrible.com/post/176657481103/repeat-yourself-do-more-than-one-thing-and)，他认为这篇把同样的观点讲得更清楚——不过我更喜欢原文，因为“易于 delete”是一个 unifying principle，让整篇文章衔接得非常好。

* * *

Joel Spolsky，[关于 Leaky Abstractions 的 Law](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)。虽然年代久远，但仍然极具影响力——“这个 abstraction 在哪里、如何 leak”是我 evaluate designs 时采用的主要视角之一：

> 回到 TCP。之前为了 simplicity，我撒了一个小谎，有些读者此刻恐怕已经气得七窍生烟，因为这个谎让你们抓狂。我说 TCP guarantees 你的 message 会 arrive。其实它并不 guarantees。如果你的宠物蛇咬断了通向你 computer 的 network cable，导致任何 IP packets 都无法通过，那么 TCP 也无能为力，你的 message 就不会 arrive。如果你曾对公司的 system administrators 态度生硬，他们为了惩罚你，把你接到了一个 overloaded hub 上，那么就只有一部分 IP packets 能通过，TCP 仍会 work，但一切都会非常 slow。
> 
> 这就是我所说的 leaky abstraction。TCP 尝试为 underlying unreliable network 提供一个 complete abstraction，但有时候，network 会 leaks through abstraction，让你感受到那些 abstraction 无法完全保护你免受其影响的事情。这只是我称为 Law of Leaky Abstractions 的一个 example：
> 
> **所有 non-trivial abstractions 在某种程度上都是 leaky 的。**
> 
> Abstractions 会 fail。有时程度轻，有时程度重。会有 leakage。事情会出错。只要有 abstractions，这种情况就会到处发生。下面是一些 examples。

* * *

[关于 software performance 的思考](https://blog.nelhage.com/post/reflections-on-performance/)，作者是 Nelson Elhage，他是这个列表中唯一有两篇不同文章入选的作者！Nelson 的想法帮助我明确了自己关于 tool design 的理念，也促成了[我对不耐烦的看法](https://www.benkuhn.net/impatient/)。

> users 更喜欢 faster software，而且如果 tools 是 faster 而不是 slower，他们在 performing 某个 task 时就会有更好的 experience，这大概相当直观。
> 
> 或许不那么明显的是，拥有 faster tools 会 changes users 如何 use 一个 tool 或 perform 一个 task。Users 几乎总有 multiple strategies 可用于 pursue 一个 goal——包括决定去做完全不同的事情——而他们会越来越频繁地选择 use faster tools。Fast tools 不只是让 users 能够更快地 accomplish tasks；它们还让 users 能够以全新的方式 accomplish 全新 types 的 tasks。在开发 Sorbet 和 Livegrep 时，我都清楚地见过这种现象…

* * *

Brandur Leach 关于 using databases 来 ensure correct edge-case behavior 的系列文章：[用 ACID 和 Constraints 来 Building Robust Systems](https://brandur.org/acid)、[通过 Using Atomic Transactions 来 Power 一个 Idempotent API](https://brandur.org/http-transactions)、[Postgres 中的 Transactionally Staged Job Drains（以事务方式暂存的任务排空）](https://brandur.org/job-drain)、[在 Postgres 中 Implementing Stripe-like Idempotency Keys](https://brandur.org/idempotency-keys)。

通常，文章标题以“在 \[technology\] 中”结尾不是好兆头，但 Brandur 的文章并非如此。即使你从未 used Postgres，那些展示如何依靠 relational databases 来 enforce correctness 的 examples 也会让你豁然开朗。

> 我想让你相信，ACID databases 是现有 tools 中最重要的一类，能够在 big production systems 中 ensuring maintainability 和 data correctness。让我们先深入了解赋予它们这个名称的每一项 guarantees。

> HTTP request 和 database 的 transaction 之间存在一种令人惊讶的 symmetry。与 transaction 一样，HTTP request 也是一个 transactional unit of work——它有明确的 beginning、end 和 result。client 通常 expects 一个 request 会 execute atomically，并会按它确实如此的方式 behave（当然，这会因 implementation 而 varies）。这里我们将查看一个 example service，看看 HTTP requests 和 transactions 如何很好地相互 apply。

> 在 APIs 中，_idempotency_ 是一个 powerful concept。idempotent endpoint 可以被 called 任意多次，同时 guaranteeing side effects 只会 occur 一次。在这个混乱的世界里，clients 和 servers 偶尔可能 crash，或者其 connections 在 request 进行到一半时 drop，这对让 systems 更 robust to failure 有极大帮助。如果 Clients 不确定一个 request 是 succeeded 还是 failed，就可以不断 retrying 它，直到得到 definitive response。

* * *

Jeff Hodges，[写给新人的 Distributed Systems 笔记](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/)。这是一组极好的指引，帮助你对 distributed systems 做合理的事（还要注意，尽管你或许能暂时忽略这一点而不出问题，任何 uses the network 的 app 都是一个 distributed system）。其中许多观点如果各自写成文章，单独就足以入选这份清单——我会定期重读它，而且总能发现一些此前应该更加重视的新建议。

> Distributed systems 是不同的，因为它们经常 fail • 在整个 system 中 Implement backpressure • 寻找能够 partially available 的方法 • Use percentiles，而不是 averages • 学会 estimate 你的 capacity • Feature flags 是 infrastructure 进行 rolled out 的方式 • 明智地 Choose id spaces • 将 cached data 进行 Writing，写回 persistent storage，是不好的 • Extract services。

* * *

J.H. Saltzer, D.P. Reed and D.D. Clark, [End-to-End Arguments in System Design](http://web.mit.edu/Saltzer/www/publications/endtoend/endtoend.pdf). Another classic. The end-to-end principle has helped me make a lot of designs much simpler.

> 这篇 paper 提出了一项 design principle，帮助指导 functions 在 distributed computer system 的 modules 之间的 placement。这项被称为 end-to-end argument 的 principle 指出，placed 在 system 的 low levels 的 functions，与在那个 low level 提供它们的 cost 相比，可能是 redundant 的，或没有多少价值。paper 中讨论的 Examples 包括 bit error recovery、security using encryption、duplicate message suppression、recovery from system crashes，以及 delivery acknowledgement。用于 support 这些 functions 的 Low level mechanisms，只有作为 performance enhancements 才是 justified 的。

* * *

Bret Victor，[以 Principle 为出发点进行 Inventing](https://vimeo.com/906418692)：

> 这些年来，我花了很多时间 making creative tools、using creative tools，也对它们进行了大量思考，并逐渐相信了一点：Creators 需要与他们正在 creating 的东西建立 immediate connection。

我实在没法摘录那些实际 demos 中的内容，而它们恰恰是精彩之处。所以我就直接推荐它：这场演讲大幅提升了我对 programming tools（以及一般意义上的 tools）能够达到何种水平的期待，而且这种提升很有益。去看看吧，你会大为惊叹。

* * *

把那些你反复重读的文章发到评论区吧！

---

## 原文

Every so often I read an essay that I end up thinking about, and citing in conversation, over and over again.

Here’s my index of all the ones of those I can remember! I’ll try to keep it up to date as I think of more.

* * *

Nelson Elhage, [Computers can be understood](https://blog.nelhage.com/post/computers-can-be-understood/). The attitude embodied in this essay is one of the things that has made the biggest difference to my effectiveness as an engineer:

> I approach software with a deep-seated belief that computers and software systems can be understood. …
> 
> In some ways, this belief feels radical today. Modern software and hardware systems contain almost unimaginable complexity amongst many distinct layers, each building atop each other. …
> 
> In the face of this complexity, it’s easy to assume that there’s just too much to learn, and to adopt the mental shorthand that the systems we work with are best treated as black boxes, not to be understood in any detail.
> 
> I argue against that approach. You will never understand every detail of the implementation of every level on that stack; but you can understand all of them to some level of abstraction, and any specific layer to essentially any depth necessary for any purpose.

* * *

Dan McKinley, [Choose Boring Technology](https://mcfunley.com/choose-boring-technology). When people ask me how we make technical decisions at Wave, I send them this essay. It’s probably saved me more heartbreak and regret than any other:

> Let’s say every company gets about three innovation tokens. You can spend these however you want, but the supply is fixed for a long while. You might get a few more after you achieve a certain level of stability and maturity, but the general tendency is to overestimate the contents of your wallet. Clearly this model is approximate, but I think it helps.
> 
> If you choose to write your website in NodeJS, you just spent one of your innovation tokens. If you choose to use MongoDB, you just spent one of your innovation tokens. If you choose to use service discovery tech that’s existed for a year or less, you just spent one of your innovation tokens. If you choose to write your own database, oh god, you’re in trouble.

* * *

Sandy Metz, [The Wrong Abstraction](https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction). This essay convinced me that “don’t repeat yourself” (DRY) isn’t a good motto. It’s okay advice, but as Metz points out, if you don’t choose the right interface boundaries when DRYing up, the resulting abstraction can quickly become unmaintainable:

> 4.  Time passes.
>     
> 5.  A new requirement appears for which the current abstraction is almost perfect.
>     
> 6.  Programmer B gets tasked to implement this requirement.
>     
>     _Programmer B feels honor-bound to retain the existing abstraction, but since isn’t exactly the same for every case, they alter the code to take a parameter…._
>     
> 7.  … Loop until code becomes incomprehensible.
>     
> 8.  You appear in the story about here, and your life takes a dramatic turn for the worse.
>     

* * *

Patrick McKenzie, [Falsehoods Programmers Believe About Names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/). When programming, it’s helpful to think in terms of “invariants,” i.e., properties that we assume will always be true. I think of this essay as the ultimate reminder that _reality has no invariants_:

> 32.  People’s names are assigned at birth.
> 33.  OK, maybe not at birth, but at least pretty close to birth.
> 34.  Alright, alright, within a year or so of birth.
> 35.  Five years?
> 36.  You’re kidding me, right?

* * *

Thomas Ptacek, [The Hiring Post](https://sockpuppet.org/blog/2015/03/06/the-hiring-post/). This essay inspired me to put a lot of effort into Wave’s work-sample interview, and the payoff was huge—we hired a much stronger team, much more quickly, than I expected to be able to. It’s also a good reminder that most things that most people do make no sense:

> Nothing in Alex’s background offered a hint that this would happen. He had Walter White’s resume, but Heisenberg’s aptitude. None of us saw it coming. My name is Thomas Ptacek and I endorse this terrible pun. Alex was the one who nonced.
> 
> A few years ago, Matasano couldn’t have hired Alex, because we relied on interviews and resumes to hire. Then we made some changes, and became a machine that spotted and recruited people like Alex: line of business .NET developers at insurance companies who pulled Rails core CVEs out of their first hour looking at the code. Sysadmins who hardware-reversed assembly firmware for phone chipsets. Epiphany: the talent is out there, but you can’t find it on a resume.
> 
> Our field selects engineers using a process that is worse than reading chicken entrails. Like interviews, poultry intestine has little to tell you about whether to hire someone. But they’re a more pleasant eating experience than a lunch interview.

* * *

Gergely Orosz, [The Product-Minded Engineer](https://blog.pragmaticengineer.com/the-product-minded-engineer/). I send this essay to coworkers all the time—it describes extremely well what traits will help you succeed as an engineer at a startup:

> Proactive with product ideas/opinions • Interest in the business, user behavior and data on this • Curiosity and a keen interest in “why?” • Strong communicators and great relationships with non-engineers • Offering product/engineering tradeoffs upfront • Pragmatic handling of edge cases • Quick product validation cycles • End-to-end product feature ownership • Strong product instincts through repeated cycles of learning

* * *

tef, [Write code that is easy to delete, not easy to extend](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to). _The Wrong Abstraction_ argues that reusable code, unless carefully designed, becomes unmaintainable. tef takes the logical next step: design for disposability, not maintainability. This essay gave me lots of useful mental models for evaluating software designs.

> If we see ‘lines of code’ as ‘lines spent’, then when we delete lines of code, we are lowering the cost of maintenance. Instead of building re-usable software, we should try to build disposable software.

> Business logic is code characterised by a never ending series of edge cases and quick and dirty hacks. This is fine. I am ok with this. Other styles like ‘game code’, or ‘founder code’ are the same thing: cutting corners to save a considerable amount of time.
> 
> The reason? Sometimes it’s easier to delete one big mistake than try to delete 18 smaller interleaved mistakes. A lot of programming is exploratory, and it’s quicker to get it wrong a few times and iterate than think to get it right first time.

tef also wrote a follow-up, [Repeat yourself, do more than one thing, and rewrite everything](https://programmingisterrible.com/post/176657481103/repeat-yourself-do-more-than-one-thing-and), that he thinks makes the same points more clearly—though I prefer the original because “easy to delete” is a unifying principle that made the essay hang together really well.

* * *

Joel Spolsky, [The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/). Old, but still extremely influential—“where and how does this abstraction leak” is one of the main lenses I use to evaluate designs:

> Back to TCP. Earlier for the sake of simplicity I told a little fib, and some of you have steam coming out of your ears by now because this fib is driving you crazy. I said that TCP guarantees that your message will arrive. It doesn’t, actually. If your pet snake has chewed through the network cable leading to your computer, and no IP packets can get through, then TCP can’t do anything about it and your message doesn’t arrive. If you were curt with the system administrators in your company and they punished you by plugging you into an overloaded hub, only some of your IP packets will get through, and TCP will work, but everything will be really slow.
> 
> This is what I call a leaky abstraction. TCP attempts to provide a complete abstraction of an underlying unreliable network, but sometimes, the network leaks through the abstraction and you feel the things that the abstraction can’t quite protect you from. This is but one example of what I’ve dubbed the Law of Leaky Abstractions:
> 
> **All non-trivial abstractions, to some degree, are leaky.**
> 
> Abstractions fail. Sometimes a little, sometimes a lot. There’s leakage. Things go wrong. It happens all over the place when you have abstractions. Here are some examples.

* * *

[Reflections on software performance](https://blog.nelhage.com/post/reflections-on-performance/) by Nelson Elhage, the only author of two different essays in this list! Nelson’s ideas helped crystallize my philosophy of tool design, and contributed to [my views on impatience](https://www.benkuhn.net/impatient/).

> It’s probably fairly intuitive that users prefer faster software, and will have a better experience performing a given task if the tools are faster rather than slower.
> 
> What is perhaps less apparent is that having faster tools changes how users use a tool or perform a task. Users almost always have multiple strategies available to pursue a goal — including deciding to work on something else entirely — and they will choose to use faster tools more and more frequently. Fast tools don’t just allow users to accomplish tasks faster; they allow users to accomplish entirely new types of tasks, in entirely new ways. I’ve seen this phenomenon clearly while working on both Sorbet and Livegrep…

* * *

Brandur Leach’s series on using databases to ensure correct edge-case behavior: [Building Robust Systems with ACID and Constraints](https://brandur.org/acid), [Using Atomic Transactions to Power an Idempotent API](https://brandur.org/http-transactions), [Transactionally Staged Job Drains in Postgres](https://brandur.org/job-drain), [Implementing Stripe-like Idempotency Keys in Postgres](https://brandur.org/idempotency-keys).

Normally, article titles ending with “in \[technology\]” are a bad sign, but not so for Brandur’s. Even if you’ve never used Postgres, the examples showing how to lean on relational databases to enforce correctness will be revelatory.

> I want to convince you that ACID databases are one of the most important tools in existence for ensuring maintainability and data correctness in big production systems. Lets start by digging into each of their namesake guarantees.

> There’s a surprising symmetry between an HTTP request and a database’s transaction. Just like the transaction, an HTTP request is a transactional unit of work – it’s got a clear beginning, end, and result. The client generally expects a request to execute atomically and will behave as if it will (although that of course varies based on implementation). Here we’ll look at an example service to see how HTTP requests and transactions apply nicely to one another.

> In APIs _idempotency_ is a powerful concept. An idempotent endpoint is one that can be called any number of times while guaranteeing that the side effects will occur only once. In a messy world where clients and servers that may occasionally crash or have their connections drop partway through a request, it’s a huge help in making systems more robust to failure. Clients that are uncertain whether a request succeeded or failed can simply keep retrying it until they get a definitive response.

* * *

Jeff Hodges, [Notes on Distributed Systems for Young Bloods](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/). An amazing set of guardrails for doing reasonable things with distributed systems (and note that, though you might be able to get away with ignoring it for a while, any app that uses the network is a distributed system). Many points would individually qualify for this list if they were their own article—I reread it periodically and always notice new advice that I should have paid more attention to.

> Distributed systems are different because they fail often • Implement backpressure throughout your system • Find ways to be partially available • Use percentiles, not averages • Learn to estimate your capacity • Feature flags are how infrastructure is rolled out • Choose id spaces wisely • Writing cached data back to persistent storage is bad • Extract services.

* * *

J.H. Saltzer, D.P. Reed and D.D. Clark, [End-to-End Arguments in System Design](http://web.mit.edu/Saltzer/www/publications/endtoend/endtoend.pdf). Another classic. The end-to-end principle has helped me make a lot of designs much simpler.

> This paper presents a design principle that helps guide placement of functions among the modules of a distributed computer system. The principle, called the end-to-end argument, suggests that functions placed at low levels of a system may be redundant or of little value when compared with the cost of providing them at that low level. Examples discussed in the paper include bit error recovery, security using encryption, duplicate message suppression, recovery from system crashes, and delivery acknowledgement. Low level mechanisms to support these functions are justified only as performance enhancements.

* * *

Bret Victor, [Inventing on Principle](https://vimeo.com/906418692):

> I’ve spent a lot of time over the years making creative tools, using creative tools, thinking about them a lot, and here’s something I’ve come to believe: Creators need an immediate connection to what they’re creating.

I can’t really excerpt any of the actual demos, which are the good part. Instead I’ll just endorse it: this talk dramatically, and productively, raised my bar for what I think programming tools (and tools in general) can be. Watch it and be amazed.

* * *

Post the essays you keep returning to in the comments!