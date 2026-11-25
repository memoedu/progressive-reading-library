# Performance（性能）

---

## 正文

> Moore's law 势不可挡 - [Jim Keller](https://www.wired.com/story/intels-new-chip-wizard-plan-bring-back-magic/)

在其 [original formulation](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/05/moores-law-electronics.pdf) 中，Moore's law[1](https://nintil.com/progress-semicon/#moore) 讲的是在不断 decreasing 的 surfaces 中 cramming 更多 transistors；按这个 metric，Moore's law 仍在毫不停歇地[继续](https://medium.com/predict/moores-law-is-alive-and-well-eaa49a450188)。不过，这还不是最有意思的事情。尽管这是一项了不起的 engineering 成就，大多数人感兴趣的还是 semiconductor 世界的 end-product：performance。

在谈这个之前，先看一些 charts，它们展示了 original Moore's law 意义上的 progress（感谢 [Sam Zeloof](https://twitter.com/szeloof) 提供 data）。这里值得注意的是，progress 并不只是让 transistors 变得 smaller（尽管近来 "marketing" node name 与实际 feature size 之间出现了 disconnect；7nm process 并不一定意味着 7 nm transistors）。[Interconnect layers](https://en.wikipedia.org/wiki/Interconnects_\(integrated_circuits\)) 指的是 chip 用于 route signals 的多个 stacked layers。

![半导体](https://nintil.com/images/2020-03-23-links-34/semi.svg)

## Performance（性能）

你可能已经注意到，我们用上的 processors 有越来越多的 cores；Intel 最新的产品 (i9-10900K) 或 AMD 最新的产品 (Threadripper 3900X) 分别有 10 和 64 个 cores。与此同时，frequencies 似乎并没有 increasing exponentially。我用过的第一台 computer 以几百 MHz 的频率 ran；多年以后，大概在 2008 年左右，我记得自己买了一台 desktop，配备的是当时首款发布的 quad core processor，也就是 Q6600，它以 2.4 GHz 的频率 ran。最新的 i9 cores 在所有 cores 都是 active 时可以达到略低于 5 GHz，而只有一个 core 时则可以略高于这个数值。因此，在我作为 computer user 的这些年里，raw frequency 已经有了 ~10x 的 improvement。这听起来都很好，但如果看近些年的情况，达到 5 GHz 以上似乎比达到 1 GHz 要困难得多。

认为 adding 更多 cores 没那么了不起的 argument 是，许多 programs 并没有被 designed 成能够 take advantage of 如此充裕的 cores；而且，虽然我们现在可以同时浏览网页、看视频、听音乐和玩游戏，但人们通常不会这么做。比如，如果你的 web browser tab 只 uses 一个 thread，那么 adding 更多 cores 并不会让 websites 瞬间 load（事实上，支撑 Node.js 和 Chrome 的 V8 javascript execution engine 就 runs 在 single thread 上；同样，Chrome 的 Blink rendering engine 也是 single threaded；Mozilla 正在 developing 一个 multithreaded [browser engine](https://servo.org/)）。归根结底，只要有一个 task 无法被 parallelized，它就会成为 application 的 bottleneck（[Amdahl's Law](https://en.wikipedia.org/wiki/Amdahl%27s_law)）。因此，这个 argument 提出了一个用于 measuring progress 的更公平或更准确的 benchmark：Single threaded performance (STP)，它不受 cores 的 number 影响；从这里看，似乎没有那么多 progress，因为 STP 与 frequency 是 directly tied 的，而 frequencies 并没有 increasing 那么多。

关于 Amdahl's law 的 argument 是成立的；但它更 naive 的说法忽略了这样一个事实：即便 frequencies 没有 increasing，semiconductors 仍然可能有 progress。例如，CPU 可以在每个 cycle 中 fetch、decode 和 execute 多个 instructions（[Pipelining](https://en.wikipedia.org/wiki/Instruction_pipelining)、[Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor)），尝试 predict 接下来会被 executed 的内容（[Branch prediction](https://en.wikipedia.org/wiki/Branch_predictor)），在 CPU 确定需要某些 code 之前就 executing 它们（[Speculative execution](https://en.wikipedia.org/wiki/Speculative_execution)），introducing instructions 来 parallelize 某些种类的 array operations（[SIMD](https://en.wikipedia.org/wiki/SIMD)），或者 reorder 这些 [order of execution of the instructions](https://en.wikipedia.org/wiki/Out-of-order_execution)。wikipedia 甚至还有一个关于 [_Megahertz myth_](https://en.wikipedia.org/wiki/Megahertz_myth) 的条目，讲的就是试图仅看 frequency 来 compare performance。

因此，如果我们[以](https://browser.geekbench.com/processor-benchmarks) Pentium 4 540 (3.2 GHz) 为例，它在 Geekbench 中取得的 performance 为 153。相比之下，i9-9900KS（full boost 时为 5 GHz）获得的 score 是 1413，这说明，只凭 1.6x higher frequency，就有可能得到 9.2x 的提升（至少按那个 benchmark 来 measured 是如此）。Multi-core performance 当然显示出更大的 improvement（44x faster）。

让 software go faster 也有其他办法；毫不奇怪，writing 更好的 software 可以让它 run faster；只不过，CPUs 不断 increasing 的 performance 使 software engineers 依赖 Moore's law 来获得 speed，也为 Jonathan Blow 那无穷无尽、抱怨 software 的 slowness 的[牢骚](https://www.youtube.com/watch?v=De0Am_QcZiQ)提供了源源不断的素材。

不过，这一点很重要，这类 improvements 都是一次性的：Branch prediction 不可能比 100% 更好，或者 SIMD 带来的 gains 最终会被 code 中无法被 vectorized 的 parts 所 offset。

Moore's law 已经持续了一段时间；如果愿意从 abstract 的意义上考虑 performance，让我们能够将 manual calculations 与 vacuum tube computers 和 integrated circuits 进行 compare，就会得到下图（来自 [Nordhaus](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.330.1871&rep=rep1&type=pdf)）

![image-20200411110142492](https://nintil.com/images/2020-03-23-links-34/image-20200411110142492.png)

作为参考，第一个能工作的 transistor 于 1947 年在 Bell labs 被 built；第一台 using transistors 的 [computer](https://en.wikipedia.org/wiki/TRADIC) 于 1954 年被 built，而 Moore 在 [1965](https://drive.google.com/file/d/0By83v5TWkGjvQkpBcXJKT1I1TTA/view) 年提出了后来被称作 "Moore's Law" 的说法。

至于确切的 numbers，要找到更新过的 benchmarks，难度出人意料地大；不过，我找到的一份 2012 年的资料声称，尽管 progress 最近已经 halved，但它仍以每年 ~20% 的 rate [持续](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/)。如果我自己来做（参见 github [repo](https://github.com/jlricon/open_nintil) 中的配套 notebook），使用 SPEC2017 benchmark，选取 2016 年最快的 CPU (Intel Xeon E5-2699A v4) 和这个 benchmark 中最快的 CPU (Intel Xeon Platinum 9282)，得到的 CAGR 会是 27%。这些都是 server-grade processors，因此我又用 Geekbench 对 i7 和 i9 系列做了同样的事（同样参见 repo），得到的结果是 ~9%。

或者，也可以看看 Apple 的 A [cores](https://browser.geekbench.com/ios-benchmarks) 的 improvements。从 iPhone 5s 到 iPhone 11 Pro max 的 progress 来看，annualized improvement 约为 ~31%。在 Android 世界中（考察 Samsung Galaxy S series，从 S6 到 S20 Ultra 5G），我们同样得到约 ~37% 的 rate。

**对于“CPUs 正在变得 faster 多少”这个问题，大致的数字可能就是每年 9-30%，而这只有过去的一半**。仍在 exponentially improving，只是没有那么快了。**广义上的 Moore's law 并没有消亡**（CPUs 随时间推移正在 exponentially faster）。

### 让 software 变得 fast

[这里有](https://twitter.com/WEREMSOFT/status/1246884854656126984)最近的一条 twitter thread，展示了只要 design 得当，无论使用什么 programming language，software 都能变得多么 faster，也许能达到 2 或 3 个 order of magnitude（有一个案例只 changing 了 [code 中的 2 个 characters](https://github.com/rustsim/nalgebra/pull/355)）。这也许是一个刻意构造且经过充分研究的 example（采用 row 与 column-wise ordering 的 Matrix multiplication），但总体要点是，理解 computer 如何 works，能够在 performance 上带来巨大收益。另一个例子是，譬如，iterating over 一个由 numbers 构成的 array，与 iterating over 一个由持有 numbers 的 structs 构成的 array 并 accessing 它们。Algorithmically，从朴素的 Computer Science 意义上说，complexity 是相同的；然而 performance 却可能截然不同。一个实际的 example 见[这里](https://rustsim.org/blog/2020/03/23/simd-aosoa-in-nalgebra/)。

### 不过，frequency 究竟如何呢

我前面说过，frequency 并没有随着时间推移变得快多少，而 higher clock rates 是让 CPUs 变得 faster 的一种方式，那么为什么我们没有看到这方面有多少 improvement 呢？或者更准确地说，为什么过去十年间我们曾看到 improvement 呢？这是因为 Dennard scaling。

Dennard scaling 指的是通过 shrinking transistors 带来的 performance increases。这里的想法是，smaller transistor 可以在 higher frequency 和 lower voltage 下 operated；最终，对于给定的 surface area，power consumption 保持不变，但你会得到更多、也更 faster 的 transistors。因此，有一段时间，manufacturers 不断 shrinking transistors 并 increasing frequencies（而不 reducing voltage），直到大约 2006 年，到了难以 effectively dissipate 由此产生的 power densities 的地步，这使 Intel 无法继续推进其 [Tejas and Jayhawk](https://en.wikipedia.org/wiki/Tejas_and_Jayhawk) architectures。显而易见的问题是，为什么不直接 drop voltages 呢？这就是 overclocking 领域中所谓的 _undervolting_，但问题在于，对于给定的 frequency，transistor 要 operate，就需要一个 minimum voltage；一旦低于它，transistors 就无法以所需的 speed 进行 switch，最终造成 system crashes。

[Power dissipation](https://software.intel.com/en-us/blogs/2014/02/19/why-has-cpu-frequency-ceased-to-grow)，也就是 CPUs 中的 power consumption，对于给定的 design，会随着 [frequency](https://software.intel.com/en-us/blogs/2014/02/19/why-has-cpu-frequency-ceased-to-grow) 的 cube 而 increases[2](https://nintil.com/progress-semicon/#freq)。到某个时候，power densities 会与 nuclear [reactor](https://www.quora.com/Why-we-use-multiple-processors-in-supercomputer-instead-of-one-powerful-processor) 的相媲美，而 dissipating 所有这些 heat 就会变得极其复杂。即使[使用 liquid nitrogen](https://valid.x86.fr/records.html) 来 cool processors，也只能达到约 ~9 GHz，而给 phones 或 laptops 配备这样的 system 似乎很困难。

除了 power dissipation，frequency 一直没有 increasing 还有另一个原因：wire delay。在 CPU 内部，一个 transistor 将其 state 以 signal 传给下一个 transistor 所需的 time 是 nonzero 的；因此，当 transistors 的 chain 足够 large 时，signals 进行 propagate 所需的 time 就可能 exceed 实际的 clock time；这为整个 CPU 的 frequency 设定了 limit：它不能 higher than 一个 signal 沿 chip 中 longest path 行进所需的 time。

第三个、较不重要的原因是，即使设法将 frequency increase 到荒唐的水平，也不会看到这一切立刻转化为更多 performance：DRAM 的 Frequency 也必须 increase；而现在，它正 [lagging](https://people.cs.pitt.edu/%7Echo/cs1541/current/handouts/lect-memh_4up.pdf) 于 CPUs 的 frequency。举例来说，如果 CPU 想从 main memory 获取某项内容，等到该项内容被 fetched 并 sent 过来时，CPU 可能已经完成了数百个 cycles。希望它在这段时间里一直在做别的事情（对可能被采用的 code branches 进行 speculating），但它仍不会像能够 instant access 这些 data 时那么 fast，这就是 CPUs 拥有 caches 的原因。

根据 [Hennessy & Patterson](https://www.amazon.co.uk/Computer-Architecture-Quantitative-Approach-Kaufmann/dp/012383872X) 的说法，如今 CPU architects 为了 extract 更多 performance 所面临的两个主要问题，首先是 heat dissipation，其次是 wire delay：

> 如今，对于几乎每一类 computer，energy 都是 computer designer 面临的最大挑战。首先，power 必须被引入并 distributed 到 chip 各处，而如今的 microprocessors 仅仅为了 power 和 ground，就要使用数百个 pins 和多个 interconnect layers。其次，power 会以 heat 的形式 dissipated，且必须被移除。$$...$$ 考虑到这些 heat 必须从一个边长约为 1.5 cm 的 chip 上 dissipated，我们已接近通过 air 进行 cooled 所能达到的 limit，而我们在这里已经停滞了近十年
> 
> $$...$$
> 
> 然而，总体而言，与 transistor performance 相比，wire delay 的 scales poorly，这给 designer 带来了额外挑战。除了 power dissipation limit，wire delay 已经成为 large integrated circuits 的一个主要 design 障碍，而且往往比 transistor switching delay 更为关键。clock cycle 中越来越大的 fractions 已被 wires 上 signals 的 propagation delay 所消耗，**但如今 power 所起的作用甚至超过了 wire delay。**

### 接下来会怎样

更多 cores。不要指望 core count 会在短期内停滞。AMD 刚刚 [released](https://www.theverge.com/2020/1/6/21054050/amd-64-core-processor-threadripper-3990x-release-date-pricing-ces-2020) 了一款 64 core chip。如果到 2030 年我们拥有了 128 core general purpose chips，我也不会感到惊讶。不过，请记住，这只对能够 parallelised 的 problems 有帮助。这对 scientific computing 和 video/image rendering 来说是好消息，但对于日常使用，相关性并没有那么大。

除了更多 cores，还有几项正在筹备中的事情，Jim Keller 在[这里](https://www.youtube.com/watch?v=oIG9ztQw2Gc)讨论了它们，目的是尝试继续 shrinking features（从而 Enabling 更多 performance）。另一方面，也许解决办法是不同的 instruction sets。如今的 chips 要么 run on x86-64，要么 run on ARM。不过，也有一些新出现的替代方案，例如 [RISC-V](https://en.wikipedia.org/wiki/RISC-V)，未来也许能让我们获得额外的 performance，谁知道呢。ARM——mobile devices 上的大多数 processors 都是 ARM-based——也一直在取得很大的 improvements；mobile phones 中 processors 的进步速度超过 desktops 中 processors 的进步速度，就证明了这一点。

如果你想要一个[综合考虑各方面的视角，那就是这张图](https://www.karlrupp.net/2018/02/42-years-of-microprocessor-trend-data/)：

![42 年的 Microprocessor Trend Data（微处理器趋势数据）](https://nintil.com/images/2020-03-23-links-34/42-years-processor-trend-625x396.png)

1

Moore 本人并没有创造“Moore's law”这个 term；而且，这个 law 的含义从一开始就不明确。因此，Moore's law 可能有许多不同的含义，没有哪一种含义显然更 valid。[2](https://nintil.com/progress-semicon/#freq)：通常用来 model power 与 frequency 之间 relation 的一个 equation 是 P~CV^2 \*f，因此，看起来 power consumption 是 depends linearly on frequency 的。但正如我在正文段落中链接的文章所指出的，最终你需要通过 raise voltage 来 raise frequency，因此 f~V，最终就得到 P~f^3 。例如，参见[这里](https://www.sciencedirect.com/science/article/pii/B9780128041895000028)（p.46）

_Ricón, José Luis, “Progress in semiconductors, or Moore's law is not dead yet”, Nintil (2020-04-11), available at https://nintil.com/progress-semicon/._

---

## 原文

> Moore's law is relentless - [Jim Keller](https://www.wired.com/story/intels-new-chip-wizard-plan-bring-back-magic/)

In its [original formulation](https://newsroom.intel.com/wp-content/uploads/sites/11/2018/05/moores-law-electronics.pdf), Moore's law[1](https://nintil.com/progress-semicon/#moore) was about cramming more transistors in ever decreasing surfaces; by that metric Moore's law [continues](https://medium.com/predict/moores-law-is-alive-and-well-eaa49a450188) unabated. However that's not the most interesting thing. As much of a feat of engineering it is, most people are interested in the end-product of the semiconductor world: performance.

Before moving onto that, here are some charts that show progress in the original Moore's law sense (ht [Sam Zeloof](https://twitter.com/szeloof) for the data). Of note here is that progress has not been just on making transistors smaller (Even though there is a recent disconnect between the "marketing" node name and the real feature size; 7nm process doesn't mean necessarily 7 nm transistors). [Interconnect layers](https://en.wikipedia.org/wiki/Interconnects_\(integrated_circuits\)) refer to the multiple stacked layers that a chip has to route signals.

![semi](https://nintil.com/images/2020-03-23-links-34/semi.svg)

## Performance

You may have noticed that we are getting processors with more and more cores; the latest from Intel (i9-10900K) or AMD (Threadripper 3900X) have 10 and 64 cores respectively. At the same time, frequencies don't seem to be increasing exponentially. The first computer I used ran at a few hundred MHz; years later, maybe around 2008 I recall getting a desktop with the first quad core processor that was released, the Q6600, which ran at 2.4 GHz. The latest i9 cores can go slightly below 5 GHz when all cores are active, and slightly above when it's only just one core. So in raw frequency during my lifetime as a computer user we have gotten a ~10x improvement. This sounds all nice and good, but when you look at recent times, it looks like getting above 5 GHz has been substantially harder than 1 GHz.

The argument made that adding more cores is not that impressive is that a lot of programs have not been designed to take advantage of this abundance of cores; and while now we could browse the web, watch videos, listen to music, and play games all at the same time, people don't tend to do this. If, say, your web browser tab only uses one thread then adding more cores is not going to make websites load instantaneously (And indeed the V8 javascript execution engine -which powers Node.js and Chrome- runs on a single thread; likewise Chrome's Blink rendering engine is single threaded; Mozilla is developing a multithreaded [browser engine](https://servo.org/)). Ultimately if there is just one task that cannot be parallelized, that will become the bottleneck for the application ([Amdahl's Law](https://en.wikipedia.org/wiki/Amdahl%27s_law)). Thus, the argument identifies a more fair or accurate benchmark for measuring progress: Single threaded performance (STP), which is not affected by the number of cores; here it would seem like there is not that much progress, as STP is directly tied to frequency, and frequencies have not been increasing that much.

The argument about Amdahl's law is true; but its more naive form overlooks the fact that there can be progress in semiconductors even when frequencies are not increasing. For example, the CPU can fetch, decode, and execute multiple instructions per cycle ([Pipelining](https://en.wikipedia.org/wiki/Instruction_pipelining), [Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor)), attempt to predict what's going to be executed next ([Branch prediction](https://en.wikipedia.org/wiki/Branch_predictor)), executing code before the CPU is sure it is needed ([Speculative execution](https://en.wikipedia.org/wiki/Speculative_execution)), introducing instructions to parallelize certain kinds of array operations ([SIMD](https://en.wikipedia.org/wiki/SIMD)), or reorder the [order of execution of the instructions](https://en.wikipedia.org/wiki/Out-of-order_execution). There is even a wikipedia entry for the [_Megahertz myth_](https://en.wikipedia.org/wiki/Megahertz_myth); trying to compare performance just by looking at frequency.

As a result, if we [take](https://browser.geekbench.com/processor-benchmarks) the Pentium 4 540 (3.2 GHz), it achieves a performance of 153 in the Geekbench. In contrast, the i9-9900KS (5 GHz at full boost) gets a score of 1413, showing that it is possible to get 9.2x better (At least as measured by that benchmark) with just a 1.6x higher frequency. Multi-core performance of course shows greater improvement (44x faster).

There are other ways of making software go faster too; unsurprisingly writing better software can make it run faster; only that the ever increasing performance of CPUs lead software engineers to rely on Moore's law for speed, providing endless fodder for Jonathan Blow's endless collection of [rants](https://www.youtube.com/watch?v=De0Am_QcZiQ) on the slowness of software.

However, and this is important, these kinds of improvements are one-off: Branch prediction cannot get better than 100%, or gains from SIMD eventually are offset from the parts of the code that cannot be vectorized.

Moore's law has been going on for a while; if one is willing to consider performance in an abstract sense that allows us to compare manual calculations with vacuum tube computers and integrated circuits we get this (From [Nordhaus](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.330.1871&rep=rep1&type=pdf))

![image-20200411110142492](https://nintil.com/images/2020-03-23-links-34/image-20200411110142492.png)

For reference, the first working transistor was built in 1947 in Bell labs; the first [computer](https://en.wikipedia.org/wiki/TRADIC) using transistors was built in 1954, and Moore stated what would later become "Moore's Law" in [1965](https://drive.google.com/file/d/0By83v5TWkGjvQkpBcXJKT1I1TTA/view).

As for the exact numbers, it's surprisingly hard to come up with updated benchmarks; but one I found from 2012 claims that while progress has halved recently, it [keeps going](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/) at a rate of ~20% per year. If I do it myself (See the companion notebook in the github [repo](https://github.com/jlricon/open_nintil)) using the SPEC2017 benchmark and take the fastest CPU in 2016 (Intel Xeon E5-2699A v4) and the fastest CPU in the benchmark (Intel Xeon Platinum 9282) the CAGR would be 27%. These are server-grade processors, so I did the same with Geekbench with the i7 and i9 ranges (Again see the repo) and I got ~9%.

Or one could also look at improvements in Apple's A [cores](https://browser.geekbench.com/ios-benchmarks). Looking at progress from the iPhone 5s to the iPhone 11 Pro max represents a ~31% annualized improvement. In the Android world (Looking at Samsung Galaxy S series, from the S6 to the S20 Ultra 5G) we similarly get a ~37% rate.

**That may be the ballpark figure for the question "How much faster are CPUs getting", 9-30% every year, and this is half of what it used to be**. Still exponentially improving, but not as fast. **Moore's law in a broad sense is not dead** (CPUs getting exponentially faster over time).

### Making software fast

[Here's](https://twitter.com/WEREMSOFT/status/1246884854656126984) a recent twitter thread that exemplifies how much faster can software get with the right design, no matter the programming language, maybe 2 or 3 order of magnitude (With just changing [2 characters in the code](https://github.com/rustsim/nalgebra/pull/355) in one case). That may be a contrived and well studied example (Matrix multiplication in row vs column-wise ordering), but the general point is that understanding how the computer works can yield great benefits in performance. Another instance of this is, say, iterating over an array of numbers vs iterating over an array of structs that hold numbers and accessing them. Algorithmically, in the naive Computer Science sense, the complexity is the same; however performance can be wildly different. See [here](https://rustsim.org/blog/2020/03/23/simd-aosoa-in-nalgebra/) for a practical example of this.

### But what about frequency, really

So I said that frequency is not getting all that much faster over time and that higher clock rates is a way to make CPUs faster, so why aren't we seeing much of an improvement there?. Or rather, why were we seeing an improvement during the past decade? Because of Dennard scaling.

Dennard scaling refers to increases in performance derived from shrinking the transistors. The idea here is that a smaller transistor can be operated at a higher frequency and at a lower voltage; ultimately for a given surface area power consumption stays the same, but you end up with more, and faster, transistors. So for a while manufacturers kept shrinking transistors and increasing frequencies (without reducing voltage) until around 2006 when a point was reached that it was hard to effectively dissipate the resulting power densities, which stopped Intel from pushing forward with their [Tejas and Jayhawk](https://en.wikipedia.org/wiki/Tejas_and_Jayhawk) architectures. The obvious question is why not just drop voltages? This is what is known as _undervolting_ in the overclocking world, but the problem is that for a given frequency there is a minimum voltage required for the transistor to operate, go lower than that and transistors are not able to switch at the required speed, ultimately causing system crashes.

[Power dissipation](https://software.intel.com/en-us/blogs/2014/02/19/why-has-cpu-frequency-ceased-to-grow), power consumption in CPUs, for a given design, increases with the cube of [frequency](https://software.intel.com/en-us/blogs/2014/02/19/why-has-cpu-frequency-ceased-to-grow)[2](https://nintil.com/progress-semicon/#freq) At some point you end up with power densities rivalling those of a nuclear [reactor](https://www.quora.com/Why-we-use-multiple-processors-in-supercomputer-instead-of-one-powerful-processor) and dissipating all that heat becomes incredibly complicated. Even [using liquid nitrogen](https://valid.x86.fr/records.html) to cool processors, one can only go up ~9 GHz, and fitting phones or laptops with such system seems hard.

There is another reason, besides power dissipation, why frequency has not been increasing: wire delay. Inside the CPU, the time it takes for a transistor to signal its state to the next transistor is nonzero; so with a sufficiently large chain of transistors the time it takes for the signals to propagate can exceed the actual clock time; this puts a limit on the frequency of the entire CPU: It cannot be higher than the time it takes for a signal to travel the longest path in the chip.

A third, less important reason is that even if one manages to increase frequency to absurd levels, one is not going to see all that immediately being translated into more performance: Frequency of DRAM would also have to increase; and right now it is [lagging](https://people.cs.pitt.edu/%7Echo/cs1541/current/handouts/lect-memh_4up.pdf) behind that of CPUs. As one example,if the CPU wants to get something from main memory, by the time the item is fetched and sent the CPU may have completed hundreds of cycles. Hopefully it will have been doing something else with that time (speculating about branches of code that will possibly be taken), but it won't be as fast had it had instant access to the data, which is why CPUs have caches.

According to [Hennessy & Patterson](https://www.amazon.co.uk/Computer-Architecture-Quantitative-Approach-Kaufmann/dp/012383872X) heat dissipation followed by wire delay are the two main issues that CPU architects are facing these days in order to extract more performance:

> Today, energy is the biggest challenge facing the computer designer for nearly every class of computer. First, power must be brought in and distributed around the chip, and modern microprocessors use hundreds of pins and multiple interconnect layers just for power and ground. Second, power is dissipated as heat and must be removed. \[...\] Given that this heat must be dissipated from a chip that is about 1.5 cm on a side, we are near the limit of what can be cooled by air, and this is where we have been stuck for nearly a decade
> 
> \[...\]
> 
> In general, however, wire delay scales poorly compared to transistor performance, creating additional challenges for the designer. In addition to the power dissipation limit, wire delay has become a major design obstacle for large integrated circuits and is often more critical than transistor switching delay. Larger and larger fractions of the clock cycle have been consumed by the propagation delay of signals on wires, **but power now plays an even greater role than wire delay.**

### What's next

More cores. Don't expect core count to stall anytime soon. AMD just [released](https://www.theverge.com/2020/1/6/21054050/amd-64-core-processor-threadripper-3990x-release-date-pricing-ces-2020) a 64 core chip. I wouldn't be surprised if by 2030 we have 128 core general purpose chips. However, remember that this only helps for problems that can be parallelised. This is good news for scientific computing and video/image rendering, but for day to day usage, it's not that relevant.

Besides more cores, there are a few things in the pipeline that Jim Keller discusses [here](https://www.youtube.com/watch?v=oIG9ztQw2Gc) to try to keep shrinking features (Enabling more performance). On other end maybe the solution is different instruction sets. Modern chips either run on x86-64 or ARM. But there are some emerging alternatives like [RISC-V](https://en.wikipedia.org/wiki/RISC-V) that may make it possible to get extra performance in the future, who knows. ARM - most processors on mobile devices are ARM-based - has also been making great improvements, as evidenced by the rate of progress of the processors found in mobile phones, greater than that of desktops.

If you want an [all-things considered view, this is it](https://www.karlrupp.net/2018/02/42-years-of-microprocessor-trend-data/):

![42 Years of Microprocessor Trend Data](https://nintil.com/images/2020-03-23-links-34/42-years-processor-trend-625x396.png)

1

Moore himself didn't coin the term "Moore's law"; and the meaning of the law was unclear since the beginning. Hence Moore's law may mean many different things and no meaning is obviously more valid. [2](https://nintil.com/progress-semicon/#freq): An equation commonly given to model the relation between power and frequency is P~CV^2 \*f, so it would seem like power consumption depends linearly on frequency. But as pointed out in the article I linked in the main paragraph, eventually you need to raise voltage to raise frequency, So f~V and you end up with P~f^3 . See, for example [here](https://www.sciencedirect.com/science/article/pii/B9780128041895000028) (p.46)

_Ricón, José Luis, “Progress in semiconductors, or Moore's law is not dead yet”, Nintil (2020-04-11), available at https://nintil.com/progress-semicon/._