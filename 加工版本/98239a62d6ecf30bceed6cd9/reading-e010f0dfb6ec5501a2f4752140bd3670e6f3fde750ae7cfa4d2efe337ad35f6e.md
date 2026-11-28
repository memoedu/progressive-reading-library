# 自20世纪80年代以来，CPU有哪些新变化？｜What's new in CPUs since the 80s?

---

## 正文

这是对 David Albert 提出的以下问题的回应：

> 我对 CPUs 的 mental model 还停留在 1980s：基本上就是一些做 arithmetic、logic、bit twiddling 和 shifting，以及在 memory 中 loading 和 storing 东西的盒子。我隐约知道一些较新的发展，比如 vector instructions (SIMD)，以及较新的 CPUs 对 virtualization 有 support 这件事（不过我不知道这在实际中意味着什么）。
> 
> 我错过了哪些很酷的发展？今天的 CPU 能做哪些去年的 CPU 做不了的事？与两年前、五年前或十年前的 CPU 相比呢？我最感兴趣的是那些 programmers 必须手动利用（或者 programming environments 必须经过 redesigned 才能利用）才能使用、因而可能还没有在使用的东西。我想这就 excludes 了 Hyper-threading/SMT 之类的东西，但说实话，我并不确定。我也对 CPUs 目前还做不到、但在不久的将来将能做到的事情感兴趣。

除非另有说明，下面的一切都指 x86 和 linux。历史往往会重演，许多对 x86 来说曾经是新事物的东西，对 supercomputing、mainframe 和 workstation 领域的人来说早已司空见惯。

### 现在

#### 杂项

首先，chips 有了 wider registers，也能 address 更多 memory。在 80s，你可能用过 8-bit CPU，但现在你的机器里几乎肯定有一个 [64-bit CPU](http://courses.cs.washington.edu/courses/csep590/06au/projects/history-64-bit.pdf)。这一点我不准备多谈，因为我假定你熟悉对 64-bit machine 进行 programming。除了提供更多 address space，64-bit mode 还提供更多 registers 和更 consistent 的 floating point results（通过避免经由 x87 floating point 为 32 和 64 bit operations pseudo-randomly 获得 80-bit precision）。自 80s 初以来引入 x86、而你很可能已经在使用的其他东西，还包括 paging / virtual memory、pipelining 和 floating point。

#### 冷门内容

我也不准备讨论如今已经无关紧要的东西（如 A20M），以及只有当你在 writing drivers、BIOS code、doing security audits，或做其他 unusually low-level 工作时才会影响你的东西（如 APIC/x2APIC、SMM、NX 或 [SGX](https://eprint.iacr.org/2016/086)）。

#### Memory / Caches（内存 / 缓存）

在剩下的话题中，最可能真正影响日常 programming 的，是 memory 如何工作。我的第一台 computer 是一台 286。在那台机器上，一次 memory access 可能需要几个 cycles。几年前，我用过一个 Pentium 4 system，其中一次 memory access 需要超过 400 cycles。Processors 的 sped up 幅度远大于 memory。解决 relatively slow memory 问题的办法一直是添加 caching 和 prefetching：前者为 frequently used data 提供 fast access，后者则在 access pattern 是 predictable 时，将 data preloads 到 caches 中。

几个 cycles 对比 400+ cycles，听起来非常糟糕；这可是 slower 了远超 100x。但如果我 write 一个简单的 loop，对一大块 64-bit (8-byte) values 进行 reads 和 operates on，CPU 就足够聪明，能在我需要之前 prefetch 正确的 data，让我在自己的 3GHz processor 上以大约 [22 GB/s](https://danluu.com/assembly-intrinsics/) 的速率 process。一个在 3GHz 下每个 cycle 能 consume 8 bytes 的 calculation，算下来也只有 24GB/s，所以达到 22GB/s 并不算糟。由于必须访问 main memory，我们损失的是大约 8% performance，而不是 100x。

作为 first-order approximation，使用 predictable memory access patterns，并对 smaller than 你的 CPU cache 的 data 块进行 operating，就能获得现代 caches 的大部分好处。如果你想尽可能挤出 performance，[这份文档](http://people.freebsd.org/~lstewart/articles/cpumemory.pdf)是个不错的起点。消化完那份 100 页的 PDF 后，你还需要熟悉你正在为之 optimizing 的 system 的 microarchitecture 和 memory subsystem，并学习如何用 [likwid](https://code.google.com/p/likwid/) 之类的工具，对你的 application 的 performance 进行 profile。

#### TLBs（地址转换后备缓冲器）

chip 上有许多服务于各种用途的小 caches，不只是针对 main memory。除非你真的要在 micro-optimizations 上全力以赴，否则不必了解 decoded instruction cache 和其他稀奇古怪的小 caches。主要的例外是 TLBs，它们是用于 virtual memory lookups 的 caches（在 x86 上通过一个 4-level page table structure 完成）。即使 page tables 位于 l1-data cache 中，每次 lookup 也要 4 cycles，也就是每次完成整个 virtual address lookup 需要 16 cycles。对于所有 user-mode memory accesses 都必需的操作来说，这完全不可接受，因此就有了用于 virtual address lookups 的 small、fast 的 caches。

因为 first level TLB cache 必须 fast，它的 size 受到严格限制（现代 chip 上可能只有 64 entries）。如果使用 4k pages，这就限制了你在不引发 TLB miss 的情况下能够 address 的 memory 数量。x86 也 supports 2MB 和 1GB pages；一些 applications 会从使用 larger page sizes 中获得很大好处。如果你有一个使用大量 memory 的 long-running application，这一点值得研究。

此外，first-level caches 通常受到 page size 乘以 [cache 的 associativity](https://danluu.com/3c-conflict/) 的限制。如果 cache 是 smaller than 这个值，那么无论查看的是 virtual address 还是 physical address，用来 index into cache 的 bits 都是相同的，因此在 indexing into cache 之前不必进行 virtual to physical translation。如果 cache 是 larger than 这个值，你就必须先做一次 TLB lookup 才能 index into cache（这至少会额外花费一个 cycle），或者构建一个 virtually indexed cache（这是可能的，但会增加 complexity 以及与 software 的 coupling）。在现代 chips 中就能看到这一 limit。Haswell 有一个 8-way associative cache 和 4kB pages。它的 l1 data cache 是 `8 * 4kB = 32kB`。

#### Out of Order Execution / Serialization（乱序执行 / 串行化）

几十年来，x86 chips 已经能够 speculatively execute 和 re-order execution（以避免在 single stalled resource 上 blocking）。这有时会导致[奇怪的 performance hiccups](https://docs.google.com/document/d/18gs0bkEwQ5cO8pMXT_MsOa8Xey4NEavXq-OvtdUXKck/pub)。不过，x86 有相当严格的要求：对于 single CPU，externally visible state，例如 registers 和 memory，必须像一切都 executed in order 那样被 updated。其 implementation 涉及确保：对于任何具有 dependency 的一对 instructions，这些 instructions 都以彼此之间正确的 order 来 execute。

这种要求一切看起来像是 executed in order 的 restriction 意味着，大多数情况下，你可以忽略 OoO execution 的存在，除非你正试图榨取尽可能好的 performance。主要的例外是，你需要确保某些东西不只是从外部看起来 executed in order，而是在内部也确实 executed in order。

举个你可能会在意的例子：你正试图使用 `rdtsc` 来 measure 一段 instructions 的 execution time。`rdtsc` 会 reads 一个 hidden internal counter，并把 result 放入 `edx` 和 `eax` 这两个 externally visible registers 中。

假设我们做类似这样的事

```
foo
rdtsc
bar
mov %eax, [%ebx]
baz
```

其中 foo、bar 和 baz 都不 touch `eax`、`edx` 或 `[%ebx]`。rdtsc 之后的 mov 会将 `eax` 的 value write 到 memory 中的某个 location；由于 `eax` 是一个 externally visible register，CPU 会保证 `mov` 直到 `rdtsc` 已经 executed 之后才 execute，从而让一切看起来都按 order 发生。

然而，由于 `rdtsc` 与 `foo` 或 `bar` 之间都没有 explicit dependency，`rdtsc` 可能在 `foo` 之前、在 `foo` 和 `bar` 之间，或在 `bar` 之后 execute。甚至可能出现 `baz` 在 `rdtsc` 之前 executes 的情况，只要 `baz` 不以任何方式影响 move instruction 即可。在某些情况下这样没有问题，但如果 `rdtsc` 是用来 measure `foo` 的 execution time，那就不行了。

要相对于其他 instructions 对 `rdtsc` 进行 precisely order，我们需要一个能 serializes execution 的 instruction。具体该如何做的精确细节，[Intel 在这份文档中提供了说明](http://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-32-ia-64-benchmark-code-execution-paper.pdf)。

#### Memory / Concurrency（内存 / 并发）

除了上文的 ordering restrictions——它们意味着对同一 location 的 loads 和 stores 不能相互 reordered——x86 的 loads 和 stores 还有其他一些 restrictions。具体来说，对于单个 CPU，stores 从不与其他 stores 发生 reordered，stores 也从不与更早的 loads 发生 reordered，无论它们是否针对同一 location。

然而，loads 可以与更早的 stores 发生 reordered。例如，如果你写下

```
mov 1, [%esp]
mov [%ebx], %eax
```

它可以被 executed，效果如同你写下的是

```
mov [%ebx], %eax
mov 1, [%esp]
```

但 converse 并不成立——如果你写下后者，它绝不可能被 executed 得如同你写下的是前者。

你可以通过 inserting 一条 serializing instruction，强制第一个 example 按照写出的方式 execute。但这要求 CPU 对所有 instructions 进行 serialize。而这很 slow，因为它实际上强制 CPU 等待 serializing instruction 之前的所有 instructions 完成，才能 executing 该 serializing instruction 之后的任何内容。如果你只关心 load/store ordering，还有一条只对 loads 和 stores 进行 serializes 的 `mfence` instruction。

我不打算讨论其他 memory fences，即 lfence 和 sfence，[但你可以在这里进一步了解它们](https://stackoverflow.com/q/20316124/334816)。

我们已经看过 single core ordering，其中 loads 和 stores 大多是 ordered 的，但还存在 multi-core ordering。上面的 restrictions 全都适用；如果 core0 正在 observing core1，它会看到所有 single core rules 都适用于 core1 的 loads 和 stores。然而，如果 core0 和 core1 发生 interact，则无法保证它们的 interaction 是 ordered 的。

例如，假设 core0 和 core 1 起初都将 `eax` 和 `edx` set 为 0，而 core 0 执行 executes

```
mov 1, [_foo]
mov [_foo], %eax
mov [_bar], %edx
```

同时 core1 执行 executes

```
mov 1, [_bar]
mov [_bar], %eax
mov [_foo], %edx
```

对于两个 cores，`eax` 都必须是 `1`，因为第一条 instruction 与第二条 instruction 之间存在 within-core dependency。然而，两个 cores 中的 `edx` 都有可能是 `0`，因为 core0 的第 3 行可以在 core0 看到 core1 的任何内容之前 execute，反过来也一样。

以上涵盖了 memory barriers，它们会对一个 core 内的 memory accesses 进行 serialize。由于 stores 必须在不同 cores 上被看作具有 consistent order，它们也能够、也确实会影响 cross-core concurrency，但要正确地推理这类事情相当困难。[对于使用 memory barriers 而不是 locking，Linus 是这样说的](http://yarchive.net/comp/linux/locking.html)：

> 不进行 locking 的 _真正_ cost，最终也往往是不可避免的 bugs。围绕 memory barriers 耍小聪明，几乎总是在埋下一个迟早会出现的 bug。要想清楚在十种不同 architectures、不同 memory ordering，以及缺少一处 barrier 的情况下可能发生的所有事情，实在是 _真的_ 很难。……事实是，只要有人设计出一种新的 locking mechanism，他们总会弄错。别这么做。

而事实证明，在现代 x86 CPUs 上，使用 locking 来 implement concurrency primitives，[往往比使用 memory barriers 更 cheaper](https://blogs.oracle.com/dave/resource/NHM-Pipeline-Blog-V2.txt)，所以我们来看看 locks。

如果我们将 `_foo` set 为 0，并让两个 threads 各自 execute `incl (_foo)` 10000 次，也就是用单条 instruction 对同一 location 进行 incrementing 共 20000 次，那么结果保证不会 exceed 20000，但它（theoretically）可能低至 2。如果你不清楚为什么 theoretical minimum 是 2 而不是 10000，那么弄清楚这一点是个不错的 exercise。如果你已经清楚了，我给你的 bonus exercise 是：任何合理的 CPU implementation 能得到这个 result 吗？还是说，这只是 spec 允许、却永远不会发生的某种荒唐情况？这篇文章里的信息不足以回答这个 bonus question，但我相信我已经链接了足够的信息。

我们可以用一个简单的 code snippet 来试试

```
#include <stdlib.h>
#include <thread>

#define NUM_ITERS 10000
#define NUM_THREADS 2

int counter = 0;
int *p_counter = &counter;

void asm_inc() {
  int *p_counter = &counter;
  for (int i = 0; i < NUM_ITERS; ++i) {
    __asm__("incl (%0) \n\t" : : "r" (p_counter));
  }
}

int main () {
  std::thread t[NUM_THREADS];
  for (int i = 0; i < NUM_THREADS; ++i) {
    t[i] = std::thread(asm_inc);
  }
  for (int i = 0; i < NUM_THREADS; ++i) {
    t[i].join();
  }
  printf("Counter value: %i\n", counter);
  return 0;
}
```

用 `clang++ -std=c++11 -pthread` 对上述内容进行 Compiling 后，我在自己的两台 machines 上得到了以下 results 的 distribution：

![Haswell 和 Sandy Bridge 上 non-determinism 的不同 distributions](https://danluu.com/images/new-cpu-features/write-contention.webp)

不仅每次 runs 的 results 会 vary，不同 machines 上 results 的 distribution 也不同。我们从未达到 2 这个 theoretical minimum，事实上也没有出现任何低于 10000 的值，但 final result 有一定概率落在 10000 到 20000 之间的任意位置。

尽管 `incl` 是单条 instruction，却不能保证它是 atomic 的。在内部，`incl` 被 implemented 为一个 load，接着一个 add，再接着一个 store。cpu0 上的一个 increment 有可能插进 cpu1 上的 load 与 store 之间 execute，反过来也一样。

Intel 对此提供的 solution 是 `lock` prefix，它可以被 added 到少数几条 instructions 上，使它们成为 atomic。如果我们把上述 code 中的 `incl` 改为 `lock incl`，得到的 output 就始终是 20000。

这就是我们让单条 instruction 成为 atomic 的方法。要让一个 sequence 成为 atomic，我们可以使用 `xchg` 或 `cmpxchg`，它们作为 compare-and-swap primitives 时始终是 locked 的。我不会详细说明其工作方式，但如果你好奇，可以看看 [David Dalrymple 的这篇文章](http://davidad.github.io/blog/2014/03/23/concurrency-primitives-in-intel-64-assembly/)。

除了让 memory transaction 成为 atomic，locks 相互之间还是 globally ordered 的，而 loads 和 stores 不会相对于 locks 发生 re-ordered。

关于 memory ordering 的 rigorous model，[请参阅 x86 TSO doc](http://www.cl.cam.ac.uk/~pes20/weakmemory/x86tso-paper.pdf)。

以上讨论的都是 concurrency 在 hardware 中如何运作。尽管 x86 对可以 re-order 的内容有所 limitations，compilers 却不一定有同样的 limitations。在 C 或 C++ 中，你需要 insert 适当的 primitives，以确保 compiler 不会 re-order 任何内容。[正如 Linus 在这里指出的](http://yarchive.net/comp/linux/memory_barriers.html)，如果你的 code 类似于

```
local_cpu_lock = 1;
// .. do something critical ..
local_cpu_lock = 0;
```

compiler 并不知道 `local_cpu_lock = 0` 不能被推到 critical section 的中间。Compiler barriers 与 CPU memory barriers 是 distinct 的。由于 x86 memory model 相对 strict，有些 compiler barriers 在 hardware level 是 no-ops，只是告诉 compiler 不要 re-order 内容。如果你使用的 language 比 microcode、assembly、C 或 C++ 更 higher level，那么你的 compiler 很可能会为你处理这一点，而不需要任何 annotation。

#### Memory / Porting（内存 / 移植）

如果你正在将 code porting 到其他 architectures，就要注意，x86 的 memory models 是你如今可能遇到的所有 architecture 中最 strongest 的之一。如果你没有想清楚就写出碰巧能工作的 code，再把它 port 到 guarantees 更 weaker 的 architectures（PPC、ARM 或 Alpha），几乎肯定会有 bugs。

考虑这个 example：

```
Initial
-----
x = 1;
y = 0;
p = &x;

CPU1         CPU2
----         ----
i = *p;      y = 1;
             MB;
             p = &y;
```

`MB` 是一个 memory barrier。在 Alpha 21264 system 上，这可能 result in `i = 0`。

Kourosh Gharachorloo 解释了其中的原因：

> CPU2 执行 y=1，这会使一个 "invalidate y" 被 sent 到 CPU1。这个 invalidate 进入 CPU1 的 incoming "probe queue"；你将会看到，问题之所以出现，是因为如果不在 CPU1 上执行 MB，这个 invalidate theoretically 可能一直留在 probe queue 中。此时，这个 invalidate 会立即被 acknowledged（也就是说，在 sending acknowledgment 之前，不会等到它真正 invalidate CPU1 的 cache 中的 copy）。因此，CPU2 可以通过它的 MB。接着，它继续执行对 p 的 write。现在 CPU1 继续 read p。read p 的 reply 在其 incoming path 上可以 bypass CPU1 上的 probe queue（这样 replies/data 就能迅速返回 21264，而不必等待先前 incoming probes 被 serviced）。现在，CPU1 可以 derefence p，以 read 它的 cache 中的 y 的 old value（CPU1 的 probe queue 中的 invalidate y 仍然留在那里）。
> 
> CPU1 上的 MB 是如何解决这个问题的？21264 在每个 MB 处都会 flushes 它的 incoming probe queue（也就是说，services 其中任何 pending messages）。因此，在对 p 的 read 之后，你执行一个 MB，就一定会 pulls in 对 y 的 invalidate。这样，你就再也不会看到 y 的 old cached value。
> 
> 尽管上述 scenario 是 theoretically possible，但由此观察到问题的概率极其微小。原因在于，即使你正确 setup 了 caching，CPU1 在 receives "read p" 的 data reply 之前，也很可能有充足的机会 service 它的 probe queue 中的 messages（即 invalidate）。不过，如果你遇到这样一种情况：在 CPU1 的 probe queue 中，对 y 的 invalidate 前面已经放入了许多东西，那么 p 的 reply 就可能返回并 bypass 这个 invalidate。不过，你很难 set up 这种 scenario 并实际 observe 这个 anomaly。

即使不谈其他 architectures，这篇文章也已经够长了，所以我不再详细展开。不过，如果你想知道为什么有人会制定允许这种 optimization 的 spec，不妨想想：在不断上涨的 fab costs 压垮 DEC 之前，它们的 chips 快到能够[在 emulation 中 run real workloads 的 industry standard x86 benchmarks，比 x86 chips natively run 相同 benchmarks 还要快](https://www.usenix.org/legacy/publications/library/proceedings/usenix-nt97/full_papers/chernoff/chernoff.pdf)。如果想进一步了解当时最 RISC-y 的 architecture 为什么作出那些决定，请参阅[这篇讨论 Alpha architecture 背后动机的 paper](https://danluu.com/dick-sites-alpha-axp-architecture.pdf)。

顺便说一句，这也是我对 Mill architecture 持怀疑态度的一个主要原因。先不讨论它们能否兑现 performance claims，technically excellent 本身并不是一种 business model。

#### Memory / Non-Temporal Stores / Write-Combine Memory（内存 / 非时间性存储 / 写合并内存）

上一节概述的那组 restrictions 适用于 cacheable（即 “write-back” 或 WB）memory。这本身在某个时期也是新事物。在那之前，只有 uncacheable（UC）memory。

UC memory 的一个有趣之处在于，所有 loads 和 stores 都应当到达 bus。对于没有 cache、且几乎没有 on-board buffering 的 processor 来说，这完全合理。其结果是，能够 access memory 的 devices 可以依赖这样的行为：对 UC memory regions 的所有 accesses 都会按顺序 creating separate bus transactions（因为某些 devices 会把 memory read 或 write 当作做某件事的 trigger）。这在 1982 年很好用，但如果你有一块只想接收最新 update 的 video card，就没那么好了。如果对同一个 UC location（或同一个 word 的不同 bytes）发生了 multiple writes，CPU 就必须为每个 write issue 一个 separate bus transaction，即使 video card 实际上并不在乎看到每个 intervening result。

解决办法是 create 一种叫作 write combine（WC）的 memory type。WC 是一种 eventually consistent UC。Writes 最终必须到达 memory，但它们可以在内部被 buffered。WC memory 的 ordering guarantees 也比 UC 更 weaker。

大多数情况下，除非你正在直接与 devices 通信，否则不必处理这些。唯一的例外是 “non-temporal” load 和 store operations。它们使特定的 loads 和 stores 表现得像是针对 WC memory 的操作，即使 address 位于一个被 marked WB 的 memory region 中。

如果你不想让某些东西 pollute 你的 caches，这就很有用。如果你正在进行[某种 streaming calculation](http://blogs.fau.de/hager/archives/2103)，并且知道某一份 data 不会被 use 超过一次，这通常很有用。

#### Memory / NUMA（内存 / NUMA）

Non-uniform memory access，也就是不同 processors 的 memory latencies 和 bandwidth 各不相同，已经如此常见，以至于我们基本不再谈论 NUMA 或 ccNUMA，因为它们太常见，已经被当作 default。

这里要记住的是，share memory 的 threads 应该位于同一个 socket，而 memory-mapped I/O heavy thread 应该确保自己位于距离它正在通信的 I/O device 最近的 socket 上。

我基本上一直避免解释事情背后的_为什么_，因为那会使这篇文章的篇幅至少再增加一个 order of magnitude。不过，我会极度 oversimplified 地解释一下为什么会有 NUMA systems，一部分是因为这个话题相对独立，比较容易解释，另一部分则是为了展示，相比于说明是什么，解释为什么要长多少。

从前，只有 memory。后来，CPUs 相对于 memory 变得足够 fast，人们便想加上 cache。如果 cache 与 backing store（memory）是 inconsistent，那就麻烦了，因此 cache 必须保留一些关于自己所持内容的 information，以便知道是否需要以及何时需要将东西 write 到 backing store。

这还不算太糟，但一旦有了 2 个各自拥有 caches 的 cores，事情就变得稍微复杂一些。要 maintain 与 no-cache 情况相同的 programming model，caches 就必须彼此 consistent，也必须与 backing store consistent。由于现有 load/store instructions 的 API 中没有任何东西允许它们说 `sorry! this load failed because some other CPU is holding onto the address you want`，最简单的做法就是让每个 CPU 在每次想 load 或 store 某些东西时，都向 bus send 一个 message。我们已经有了两个 CPUs 都 connected 的 memory bus，因此只需要求其他 CPUs 在自己的 cache 中拥有该 data 的 modified version 时，用该 data respond（并 invalidate 相应的 cache line）。

这运作得还不错。大多数时候，每个 CPU 只会 touches 另一个 CPU 不关心的 data，因此会有一些浪费的 bus traffic。不过这还不算太糟，因为一旦一个 CPU 发出 message，说 `Hi! I'm going to take this address and modify the data`，它就可以假定自己完全 owns 那个 address，直到另一个 CPU 请求它，而这很可能不会发生。而且，我们不必针对单个 memory address 进行操作，可以改为 operate on 比如具有 64 bytes 的 cache lines。因此，总体 overhead 相当 low。

对于 4 个 CPUs，这仍然运作得还不错，尽管 overhead 会稍高一些。但这种每个 CPU 都必须 respond 其他每个 CPU 的做法，很难 scale 到远超 4 个 CPUs 的规模，一方面是因为 bus 会变得 saturated，另一方面是因为 caches 也会变得 saturated（cache 的 physical size/cost 相对于所支持的 simultaneous reads 和 write 的数量是 `O(n^2)`，而 speed 与 size 是 inversely correlated）。

这个问题的一个“简单” solution 是设置一个 single centralized directory，负责 keeps track of 所有 information，而不是进行 N-way peer-to-peer broadcast。既然我们现在反正都在一个 chip 上放入 2-16 个 cores，那么每个 chip（socket）设置一个 single directory，来 tracks 一个 chip 上每个 core 的 caches 的 state，就是很自然的做法。

这只解决了每个 chip 内部的问题，我们还需要某种方式让 chips 彼此通信。不幸的是，当我们不断 scaling 这些 systems 时，bus speeds 已经快到很难将 signal drive 到足够远的距离，从而把一堆 chips 和 memory 全部 connect 在同一个 bus 上，即使对于 small systems 也是如此。最简单的 solution 是让每个 socket own 一个 memory 的 region，这样每个 socket 就不必 connected 到 memory 的每个部分。这也避免了需要 higher level directory of directories 所带来的 complexity，因为任何特定的一块 memory 由哪个 directory owns 都很清楚。

这种做法的缺点是，如果你位于一个 socket 中，却想要另一个 socket 所 owned 的 memory，就会承受显著的 performance penalty。为了简单起见，大多数“小型”（< 128 core）systems 使用 ring-like busses，因此 performance penalty 不只是为了到达 memory 而经过一连串额外 hops 所付出的直接 latency/bandwidth penalty；它还会消耗一种 finite resource（ring-like bus），并 slows down 其他 cross-socket accesses。

In theory，OS 会 transparently handles 这些，但它[往往是 inefficient 的](https://www.usenix.org/conference/atc14/technical-sessions/presentation/gaud)。

#### Context Switches / Syscalls（上下文切换 / 系统调用）

这里，syscall 指的是 linux system call，而不是 `SYSCALL` 或 `SYSENTER` 这两条 x86 instructions。

现代 cores 所具有的各种 caching 的一个 side effect 是，[context switches 的 cost 很高](http://blog.tsunanet.net/2010/11/how-long-does-it-take-to-make-context.html)，这也使得 syscalls 的 cost 很高。Livio Soares 和 Michael Stumm [在他们的论文中](http://www.cs.cmu.edu/~chensm/Big_Data_reading_group/papers/flexsc-osdi10.pdf)非常详细地讨论了这种 cost。下面我会使用他们的几张图。这里的一张 graph 展示了 Core i7 在 Xalan 上能达到多少 instructions per clock (IPC)；Xalan 是 SPEC CPU 的一个 sub-benchmark。

![syscall 所产生的 overhead 的长尾。14,000 cycles。](https://danluu.com/images/new-cpu-features/flexsc_ipc_recovery.webp)

在一次 syscall 之后经过了 14,000 cycles，code 仍未完全以 full speed 运行。

下表列出几种不同 syscalls 的 footprint，既包括 direct cost（以 instructions 和 cycles 计），也包括 indirect cost（来自 cache 和 TLB evictions 的数量）。

![stat、pread、pwrite、open+close、mmap+munmap 和 open+write+close 的 Cost](https://danluu.com/images/new-cpu-features/flexsc_cost.webp)

这些 syscalls 中有些会造成 40+ 次 TLB evictions！对于具有 64-entry d-TLB 的 chip 来说，这几乎会清空整个 TLB。cache evictions 也不是没有 cost 的。

syscalls 的 high cost 是人们在 high-performance code 中转而使用 syscalls 的 batched versions（例如 `epoll` 或 `recvmmsg`）的原因，也是需要 [very high performance I/O](https://danluu.com/clwb-pcommit/) 的人经常使用 user space I/O stacks 的原因。更一般地说，context switches 的 cost 使得 high-performance code 往往采用 thread-per-core（甚至在一个 pinned thread 上采用 single threaded），而不是 thread-per-logical-task。

这种 high cost 也是 [vDSO 背后的推动因素，它会将某些不需要任何 privilege escalation 的简单 syscalls 转变为简单的 user space library calls](http://www.linuxjournal.com/content/creating-vdso-colonels-other-chicken)。

#### SIMD（单指令多数据）

基本上所有现代 x86 CPUs 都 support SSE，即 128-bit wide vector registers 和 instructions。由于人们经常希望多次执行相同的 operation，Intel 增加了一些 instructions，让你能将一个 128-bit 的 data chunk 当作 2 个 64-bit chunks、4 个 32-bit chunks、8 个 16-bit chunks 等来 operate on。ARM 也 supports 相同的东西，只是名称不同（NEON），而且所 supported 的 instructions 非常相似。

使用 SIMD instructions 获得 2x-4x speedup 是很常见的；如果你有 computationally heavy workload，那绝对值得研究一下。

Compilers 在 recognizing 可被 vectorized 的常见 patterns 方面已经足够出色，因此，像下面这样的简单 code，在使用现代 compilers 时会自动使用 vector instructions

```
for (int i = 0; i < n; ++i) {
  sum += a[i];
}
```

但是，如果你不手写 assembly，compilers 就会[经常 produce non-optimal code](https://danluu.com/assembly-intrinsics/)，尤其是 SIMD code，因此，如果你确实在意获得尽可能好的 performance，就应该查看 disassembly，并检查是否存在 compiler optimization bugs。

#### Power Management（电源管理）

现代 CPUs 有许多精巧的 power management feature，能够在不同情形下 [optimize power usage](https://danluu.com/datacenter-power/)。其结果是，“race to idle”，也就是尽快完成工作，然后让 CPU 重新进入 sleep，是最 power efficient 的工作方式。

已有大量研究表明，特定的 microoptmizations 可以改善 power consumption，但[在真实 workloads 上应用这些 microoptimizations，所产生的收益往往小于预期](http://arcade.cs.columbia.edu/energy-oopsla14.pdf)。

#### GPU / GPGPU（图形处理器 / 通用图形处理器计算）

比起其他这些内容，我更没有资格谈论这个话题。幸好，Cliff Burdick 自愿撰写了关于 GPUs 的一节，下面就是他的内容。

在 2000 年代中期以前，Graphical Processing Units (GPUs) 被 restricted 于一种仅允许对 hardware 进行极有限 control 的 API。随着 libraries 变得更加 flexible，programmers 开始将这些 processors 用于更 general-purpose 的 tasks，例如 linear algebra routines。GPU 的 parallel architecture 可以通过 launching 数百个 simultaneous threads 来处理 matrix 的 large chunks。不过，code 必须使用传统的 graphics APIs，其能够 control 的 hardware 范围仍然有限。Nvidia 和 ATI 注意到了这一点，并发布了 frameworks，让 user 能通过 graphics industry 之外的人也熟悉的 API 来 access 更多 hardware。这些 libraries 逐渐普及，如今 GPUs 已与 CPUs 一起被广泛用于 high-performance computing (HPC)。

与 CPUs 相比，GPUs 上的 hardware 有几项主要差异，概述如下：

##### Processors（处理器）

在 top level，一个 GPU processor 包含一个或多个 streaming multiprocessors (SMs)。现代 GPU 上的每个 streaming multiprocessor 通常包含超过 100 个 floating point units，也就是 GPU 领域通常所说的 cores。每个 core 通常 clocked 在 800MHz 左右，不过，与 CPUs 一样，也有 clock rates 更高但 cores 更少的 processors。GPU processors 缺少其 CPU counterparts 的许多 features，包括 large caches 和 branch prediction。在 cores、SMs 和整体 processor 这些 layers 之间，communicating 会越来越慢。因此，能在 GPUs 上 perform well 的 problems 通常是 highly-parallel 的，但又有一定量的 data 可以在少量 threads 之间 shared。我们会在下面的 memory 一节中解释这是为什么。

##### Memory（内存）

现代 GPU 上的 Memory 分为 3 个主要类别：global memory、shared memory 和 registers。Global memory 就是 GPU 包装盒上宣传的 GDDR memory，其 size 通常约为 2-12GB，throughput 为 300-400GB/s。Global memory 可以被 processor 上所有 SMs 的所有 threads accessed，同时也是该 card 上最慢的一类 memory。Shared memory 顾名思义，就是在同一个 SM 内的所有 threads 之间 shared 的 memory。它通常至少是 global memory 的两倍快，但不同 SMs 上的 threads 之间不能相互 accessible。Registers 与 CPU 上的 registers 很相似，都是在 GPU 上 access data 的最快方式，但它们是 local per thread 的，其中的 data 对其他任何正在运行的 thread 都不 visible。shared memory 和 global memory 对如何 accessed 都有非常严格的规则，不遵守这些规则会有严重的 performance penalties。要达到上述 throughputs，同一个 thread group 内各 threads 的 memory accesses 必须是 completely coalesced 的。与 CPU reading 到单个 cache line 类似，GPUs 的 cache lines 的 size 使得只要 aligned properly，单次 access 就能服务一个 group 内的所有 threads。不过，在 worst case 下，如果一个 group 内的所有 threads 都 access 不同 cache line 中的 memory，就需要为每个 thread 单独进行一次 memory read。这通常意味着 cache line 中的大部分 data 都没有被该 thread 使用，而 memory 的 usable throughput 会下降。类似规则也适用于 shared memory，不过有几个例外，我们在这里不作讨论。

##### Threading Model（线程模型）

GPU threads 以 SIMT (Single Instruction Multiple Thread) 方式 run，而每个 thread 都在一个 group 中 runs，其 size 在 hardware 中是 pre-defined 的（通常为 32）。最后这一点有许多影响；那个 group 中的每个 thread 都必须同时处理同一个 instruction。如果一个 group 中的任何 threads 需要在 code 中走与其他 threads 不同的 divergent path（例如一个 if statement），那么所有不属于该 branch 的 threads 都会 suspend execution，直到该 branch 完成。举一个简单的 example：

```
if (threadId < 5) {
   // Do something
}
// Do More
```

在上面的 code 中，这个 branch 会使 group 中我们这 32 个 threads 里的 27 个 suspend execution，直到该 branch 完成。你可以想象，如果许多 groups 的 threads 都 run 这段 code，那么当大多数 cores 处于 idle 时，overall performance 就会受到很大影响。只有当整个 group 的 threads 都 stalled 时，hardware 才可以 swap in 另一个 group，在那些 cores 上 run。

##### Interfaces（接口）

如今的 GPUs 必须有一个 CPU，才能在 CPU 和 GPU memory 之间双向 copy data，并在 GPU 上 launch 和 code。在最高 throughput 下，一个具有 16 个 lanes 的 PCIe 3.0 bus 可以达到约 13-14GB/s 的 rates。这听起来可能很高，但与 GPU 本身的 memory speeds 相比，它们慢了一个 order of magnitude 以上。事实上，随着 GPUs 变得更 powerful，PCIe bus 正日益成为 bottleneck。要看到 GPU 相对于 CPU 的任何 performance benefits，就必须给 GPU 加载大量 work，使 GPU 用于 run 该 job 的时间显著超过双向 copy data 所花的时间。

较新的 GPUs 具有在 GPU code 中 launch work dynamically 而不必返回 CPU 的 features，但目前其使用仍相当有限。

##### GPU Conclusion（GPU 结论）

由于 CPUs 和 GPUs 之间存在重大的 architectural differences，很难想象任何一方会完全取代另一方。事实上，对于 parallel work，GPU 能很好地补充 CPU，并让 CPU 在 GPU running 时独立处理其他 tasks。AMD 正尝试通过其 "Heterogeneous System Architecture" (HSA) 将这两种 technologies merge，但拿到现有的 CPU code 并确定如何将其 split 到 processor 的 CPU 和 GPU 部分，不仅对 processor，而且对 compilers 都将是一个重大挑战。

#### Virtualization（虚拟化）

既然你提到了 virtualization，我就稍微谈一谈，但 Intel 对 virtualization instructions 的 implementation 通常不是你需要考虑的事情，除非你正在 writing 直接处理 virtualization 的非常 low-level code。

处理这些东西相当麻烦，你可以[从这段 code 中](https://github.com/vishmohan/vmlaunch)看出来。即使是那里展示的非常简单的情况，为使用 Intel 的 VT instructions 来 launch 一个 VM guest 而进行设置，也需要大约 1000 行 low-level code。

#### Virtual Memory（虚拟内存）

如果你看一下 Vish 的 VT code，就会注意到其中有相当一部分 code 专门用于 page tables / virtual memory。这是另一个“新” feature，除非你正在 writing 一个 OS 或其他 low-level systems code，否则不必操心。使用 virtual memory 比使用 segmented memory 简单得多，但这在如今已无关紧要，所以我就说到这里。

#### SMT / Hyper-threading（同时多线程／超线程）

既然你提到了，我也说说 SMT。正如你所说，这对 programmers 大体上是 transparent 的。在 single core 上 enabling SMT 的典型 speedup 约为 25%。这有利于 overall throughput，但意味着每个 thread 可能只能获得其原先 performance 的 60%。对于你非常在意 single-threaded performance 的 applications，disabling SMT 可能更好。不过，这在很大程度上取决于 workload，而且与其他任何改动一样，你应该针对自己的确切 workload run 一些 benchmarks，看看怎样最好。

chips（以及 software）中增加的所有这些 complexity 带来的一个 side effect 是，performance 远不如过去 predictable；在将要 run 的特定 hardware 上，对自己的确切 workload 进行 benchmarking，其相对重要性已经上升。

举个例子，人们常常拿 [Computer Languages Benchmarks Game](http://benchmarksgame.alioth.debian.org/) 的 benchmarks 来证明一种 language 比另一种更快。我自己尝试过 reproducing 这些 results，在我的 mobile Haswell 上（而不是那些 results 所使用的 server Kentsfield），我得到的 results 差异高达 2x（就 relative speed 而言）。对于在同一台 machine 上 Running 同一个 benchmark，Nathan Kurz 最近给我看了一个 example，其中 `gcc -O3` 比 `gcc -O2` 慢 25%。[改变 C++ programs 的 linking order 可以造成 15% 的 performance change](http://www-plan.cs.colorado.edu/klipto/mytkowicz-asplos09.pdf)。Benchmarking 是个难题。

#### Branches（分支）

过去的传统看法是，branches 很 expensive，应该不惜一切（或大多数）代价避免。在 Haswell 上，branch misprediction penalty 为 14 cycles。Branch mispredict rates 取决于 workload。在几种不同的事情（bzip2、top、mysqld、重新生成我的博客）上使用 `perf stat`，我得到的 branch mispredict rates 在 0.5% 到 4% 之间。如果我们说一个 correctly predicted branch 的 cost 为 1 cycle，那么 average cost 就在 `.995 * 1 + .005 * 14 = 1.065 cycles` 到 `.96 * 1 + .04 * 14 = 1.52 cycles` 之间。这并不算太糟。

实际上，从大约 1995 年起，这种说法就夸大了 penalty，因为 Intel 增加了 conditional move instructions，让你无需 branch 就能 conditionally move data。这个 instruction 曾遭到 [Linus 的猛烈批评](http://yarchive.net/comp/linux/cmov.html)，令人印象深刻，也使它名声不佳，但[相比 branches，使用 cmov 获得显著 speedups](https://github.com/logicchains/LPATHBench/issues/53#issuecomment-68160081) 是相当常见的。

extra branches 的 cost 的一个现实 example 是 enabling integer overflow checks。当使用 [bzip2 来 compress 某个 file 时，这会使 instructions 的数量增加约 30%（增加的部分全部来自 extra branch instructions），从而造成 1% 的 performance hit](https://danluu.com/integer-overflow/)。

Unpredictable branches 不好，但大多数 branches 都是 predictable 的。如今，在你的 profiler 告诉你存在 hot spot 之前，忽略 branches 的 cost 是相当合理的。过去十年里，CPUs 在 executing poorly optimized code 方面已经进步了很多，而 compilers 也越来越擅长 optimizing code，因此，除非你试图从某段 code 中榨出绝对最佳的 performance，否则 optimizing branches 并不值得花时间。

如果结果表明你确实需要这样做，那么使用 [profile-guided optimization](http://en.wikipedia.org/wiki/Profile-guided_optimization) 很可能比试图手动折腾这些东西更好。

如果你确实必须手动完成，可以使用 compiler directives 来说明某个 branch 是否 likely to be taken。如今的 CPUs 会 ignore branch hint instructions，但它们可以帮助 compiler 更好地 lay out code。

#### Alignment（对齐）

老一辈的传统看法是，你应该 pad out structs，并确保各项都是 aligned。但在 Haswell chip 上，几乎任何你能想到的、不 cross a page boundary 的 single-threaded 情况，其 mis-alignment 都是 zero。有些情况下它确实可能产生影响，但总的来说，这又是一种基本无关紧要的 optimization，因为 CPUs 在 executing bad code 方面已经进步了太多。当它 increases the memory footprint 却没有任何收益时，还会带来轻微的危害。

另外，[不要让各项 page aligned，也不要让它们 aligned 到 large boundaries，否则你会毁掉 caches 的 performance](https://danluu.com/3c-conflict/)。

#### Self-modifying code（自修改代码）

这又是一种如今已不太有意义的 optimization。使用 self-modifying code 来 decrease code size 或 increase performance，过去是有意义的，但由于如今的 caches 往往将 l1 instruction 和 data caches 分开，modifying running code 就需要在 chip 的 l1 caches 之间进行 expensive communication。

### 未来

下面是一些可能的变化，按推测程度从低到高排列。

#### Partitioning（分区）

如今显然有越来越多的 compute 正在迁往 large datacenters。有时这涉及在 VMs 上 running，有时涉及在某种 container 中 running，有时则涉及 running bare metal，但无论如何，individual machines 经常被 multiplexed，以 run 各种各样的 workloads。理想情况下，你应该能够 schedule best effort workloads 来充分利用 stranded resources，而不影响具有 SLA 的 latency sensitive workloads。事实证明，通过一些[相对直接的 hardware changes](https://danluu.com/intel-cat/)，你确实可以做到这一点。

![90% 的 overall machine utilization](https://danluu.com/images/new-cpu-features/google_heracles.webp)

如果 caches 能够被 partitioned，使 best effort workloads 不 impact latency sensitive workloads，那么 [David Lo 等人已经证明，你可以获得约 90% 的 machine utilization，而不 impacting latency SLAs](http://csl.stanford.edu/~christos/publications/2015.heracles.isca.pdf)。红色实线是普通 Google web search cluster 上的 load，绿色虚线则是进行适当 optimizations 后的结果。从酒吧里的闲聊来看，我的印象是，红色实线实际上已经比 Google 大多数竞争对手能做到的更好（更高）了。如果将 90% 的 optimized utilization 与通常为 10% 到 90% 的 server utilization 相比较，那么相较于 running 一个 naive、unoptimized 的 setup，cost per unit of work 就会产生巨大的差异。通过大量 hardware 方面的努力，Google 得以 avoid interference，但额外的 isolation features 可以让这件事以更少的努力、更高的 efficiency 来完成。

#### Transactional Memory 与 Hardware Lock Elision

IBM 的 POWER chips 中已经有这些 features。Intel 曾试图将它们加入 Haswell，但由于一个 bug，它们被 disabled 了。总的来说，如今的 CPUs 相当 complex，[而我们应该预期会看到比过去多得多的 bugs](https://danluu.com/cpu-bugs/)。

Transactional memory support 正如其名称所示：对 transactions 的 hardware support。这是通过三条新的 instructions 实现的，分别是 `xbegin`、`xend` 和 `xabort`。

`xbegin` 会 starts 一个新的 transaction。一个 conflict（或一个 `xabort`）会 causes processor 的 architectural state（包括 memory）被 rolled back 到紧接 `xbegin` 之前的 state。如果你通过 library 或 language support 来使用 transactional memory，这对你来说应该是 transparent 的。如果你正在 implementing library support，就必须想办法将这种 hardware support 连同其 limited hardware buffer sizes，convert 成能够 handle arbitrary transactions 的东西。

我不打算讨论 Hardware Lock Elision，只想说，在底层，它是用与 implement transactional memory 所用 mechanisms 十分相似的 mechanisms 来 implemented 的，而且它的设计目的是 speed up lock-based code。如果你想利用 HLE，请参阅[这份文档](http://mcg.cs.tau.ac.il/papers/amir-levy-msc.pdf)。

#### Fast I/O（快速输入输出）

无论是 storage 还是 networking，I/O bandwidth 都在上升，I/O latencies 都在下降。问题在于，I/O 通常通过 syscalls 来完成。正如我们已经看到的，syscalls 的 relative overhead 一直在上升。对于 storage 和 networking，答案都是转向 [user mode I/O stacks](http://research.microsoft.com/pubs/198366/Asplos2012_MonetaD.pdf)（把所有东西放在 kernel mode 中也行，但那更难让人接受）。在 storage 方面，这大体上仍然是个古怪的研究课题，但 HPC 和 HFT 领域的人在 networking 中这样做已经有一段时间了。而且我说的一段时间，并不是几个月。这里有[一篇 2005 年的论文，讨论了我接下来要谈的 networking 内容，以及一些我不打算讨论的内容（DCA）](http://web.stanford.edu/group/comparch/papers/huggahalli05.pdf)。

这终于开始逐渐进入非 supercomputing 领域。MS 宣传配备 infiniband networking 和 virtualized RDMA 的 Azure 已经一年多了，Cloudflare [也谈过使用 Solarflare NICs 来获得相同 capability](http://blog.cloudflare.com/a-tour-inside-cloudflares-latest-generation-servers/)，等等。最终，我们会看到 onboard 配有 fast Ethernet 的 SoCs，而且除非这仅限于 Xeon-type devices，否则它会逐渐普及到所有 devices。ARM devices 之间的竞争，很可能会促使至少一家 ARM device 制造商在其 commodity SoCs 上加入 fast Ethernet，这或许会迫使 Intel 采取行动。

其中 RDMA 这一点很重要；它让你能够完全 bypass CPU，并让 NIC respond to remote requests。几个月前，我完整学了一遍 Stanford/Coursera 的 Mining Massive Data Sets 课程。在最开始的几节课之一，他们给出了一个“典型” datacenter setup 的例子，其中使用 1Gb top-of-rack switches。如果你通过 non-RDMA NICs 来做 kernel TCP，那么用它来 processing “massive” data 并非不合理，因为仅仅尝试通过 linux 的 TCP stack 推送 1Gb/s，就可能耗尽整个 core 的处理能力。但在 Azure 上，MS 谈到从 single machine 获得 40Gb；也就是说，一台 machine 就获得了你可能预期整个 rack 才能提供的 40x bandwidth。他们还提到低于 2 us 的 latencies，比你通过 kernel TCP 能获得的低了多个 orders of magnitude。这并不算什么新想法。[这篇 2011 年的论文](http://www.scs.stanford.edu/~rumble/papers/latency_hotos11.pdf)预测了 network 方面迄今发生的一切，以及一些仍需时日的事情。

[这场 MS 演讲讨论了如何为 network storage 利用这种 bandwidth 和 latency](https://www.youtube.com/watch?v=8Kyoj3bKepY&feature=youtu.be&t=20m8s)。一个不必点开链接就能说明的具体例子是 Amazon 的 EBS。它让你可以在任意 AWS nodes 上使用一个 arbitrary size 的“elastic” disk。由于 spinning metal disk seek 的 latency 高于通过 kernel TCP 进行的 RPC，你几乎可以 transparently 获得 infinite storage。例如，假设你能从 network 获得 [100us (.1ms) 的 latency](http://www.scs.stanford.edu/~rumble/papers/latency_hotos11.pdf)，而 disk seek time 是 8ms。那么 remote disk access 就是 8.1ms，而不是 8ms，overhead 并不大。不过，对 SSDs 来说，这就不太行了，因为你可以[从 SSD 获得](http://www.anandtech.com/show/8104/intel-ssd-dc-p3700-review-the-pcie-ssd-transition-begins-with-nvme/3) 20 us (.02ms)。但 RDMA latency 足够低，因此可以为 SSDs 提供一个 transparent 的 EBS-like layer。

以上就是 networked I/O。如果比 flash 更快的下一代 storage technologies 开始 deployed，那么 disk 方面的 performance benefit 可能会更大。performance delta 如此之大，以至于 [Intel 正在 adding 新的 instructions，以跟上下一代 low-latency storage technology](https://danluu.com/clwb-pcommit/)。具体取决于你问谁，过去一二十年来，这些东西一直都说还差几年；这比 networking 方面的事情更不确定。但即使使用 flash，人们也已经在展示 latency 能降到 single microsecond range 的 devices，这已经是相当大的 improvement。

#### Hardware Acceleration（硬件加速）

和 fast networked I/O 一样，这在某些细分领域已经出现了。[DESRES 多年来一直在做 ASICs，以在 computational chemistry 中获得 100x-1000x speedup](http://www.deshawresearch.com/publications.html)。[Microsoft 已经谈到过用 FPGAs 来 speeding up search](http://research.microsoft.com/apps/pubs/default.aspx?id=212001)。人们[研究 accelerating memcached 和类似 systems 已经有一段时间了](http://web.eecs.umich.edu/~twenisch/papers/isca13.pdf)，[来自 Toshiba 和 Stanford 的 researchers 前些时候 demonstrated 了一个实际的 implementation](http://csl.stanford.edu/~christos/publications/2014.hwkvs.nvmw.slides.pdf)，而我最近也看到了一篇来自 Berkeley、讨论同一件事的 pre-print。有多家公司正在 making Bitcoin mining ASICs。[其他](http://www.neuflow.org/) [application](http://www.artificiallearning.com/products/) [areas](https://github.com/cambridgehackers/) 也是如此。

随着从 CPUs 获得 power/performance gains 变得越来越难，我们似乎应该会看到更多这样的情况。如果你认为 programming 是一项 software oriented 的工作，你可能会觉得这是在回避你的问题，但另一种看法是，program 某个东西的含义将会改变。将来，它可能意味着把 designing 像 FPGA 或 ASIC 这样的 hardware 与 writing software 结合起来。

##### 更新

现在是 2016，距离这篇文章最初发表已经过去一年，我们可以看到，各家公司正在投资 hardware accelerators。除了此前在 FPGA accelerated search 上的工作，Microsoft [还宣布正在使用 FPGAs 来 accelerate networking](https://www.youtube.com/watch?v=RffHFIhg5Sc&feature=youtu.be&t=23m30s)。Google 对 infrastructure 一直守口如瓶，这也是他们一贯的作风，不过，如果你查看 Tensorflow 最初的 release，就能看到一些明确 references FPGAs 的 code snippets，例如：

```
enum class PlatformKind {
  kInvalid,
  kCuda,
  kOpenCL,
  kOpenCLAltera,  // Altera FPGA OpenCL platform.
                  // See documentation: go/fpgaopencl
                  // (StreamExecutor integration)
  kHost,
  kMock,
  kSize,
};
```

以及

```
string PlatformKindString(PlatformKind kind) {
  switch (kind) {
    case PlatformKind::kCuda:
      return "CUDA";
    case PlatformKind::kOpenCL:
      return "OpenCL";
    case PlatformKind::kOpenCLAltera:
      return "OpenCL+Altera";
    case PlatformKind::kHost:
      return "Host";
    case PlatformKind::kMock:
      return "Mock";
    default:
      return port::StrCat("InvalidPlatformKind(", static_cast<int>(kind), ")");
  }
}
```

截至撰写本文时，Google 对 `+google +kOpenClAltera` 不 return 任何 results，因此这件事似乎还没有被广泛注意到。如果你不熟悉 Altera OpenCL，而且你在 google 工作，可以试试 comment 中建议的 internal go link，即 `go/fpgaopencl`。如果你和我一样，不在 Google 工作，那么，[这里有 Altera 的 docs](https://www.altera.com/products/design-software/embedded-software-developers/opencl/overview.html)。基本思路是，你可以拿 OpenCL code，也就是你可能会在 GPU 上 run 的同类东西，改为在 FPGA 上 run；而从 comment 来看，Google 似乎有某种 setup，让你能够借助 FPGAs 将 data stream 进出 nodes。

那段 FPGA-specific code 在 ddd4aaf5286de24ba70402ee0ec8b836d3aed8c7 中被 removed 了，该 commit message 以“TensorFlow: upstream changes to git.”开头，随后列出了正在被 upstreamed 的 internal google commits，并附有每个 internal commit 的说明。奇怪的是，其中完全没有提到 removing FPGA support，尽管这似乎是一件足够重要、按理应该有所说明的事，除非相关内容被刻意删去了。Amazon 对其 infrastructure 计划也一直相当保密，但看看他们一直在大量招揽哪些 hardware 人才，你就能作出一些合理的猜测。另有几家公司也在 hardware accelerators 上下了重注，不过，由于我是通过私下交谈了解到这些情况的，而不是通过意外公开的 public source code 或其他公开信息，我就留给你去猜是哪些公司了。

#### Dark Silicon / SoCs（暗硅 / 片上系统）

transistor scaling 如今的发展带来了一个有趣的 side effect：我们可以把大量 transistors pack 到一个 chip 上，但它们 generate 的 heat 如此之多，以至于如果你不想让 chip 熔化，普通的 transistor 在大多数时候都不能 switch。

这带来的一个结果是，加入那些很多时候并不会被使用的 dedicated hardware 变得更合理了。首先，这意味着我们会得到各种 specialized instructions，例如 [PCMP](http://www.felixcloutier.com/x86/PCMPESTRI.html) 和 [ADX](http://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-large-integer-arithmetic-paper.pdf) instructions。但这也意味着，我们正在得到一些 chips，其中 integrated 了过去本会位于 off-chip 的完整 devices。这包括 GPUs，以及用于 mobile devices 的 radios 等。

结合 hardware acceleration 的趋势，这也意味着，各家公司 design 自己的 chips，或者至少 design 自己 chips 的一部分，变得更合理了。Apple 从收购 PA Semi 中获益良多。起初，是向普通的标准 ARM architectures 添加小型 custom accelerators，随后则是向他们自己的 custom architecture 添加 custom accelerators。由于合适的 custom hardware 与经过周密考虑的 benchmarking 和 system design 相结合，iPhone 4 比我的旗舰 Android 手机还稍微 responsive 一些，而后者要新好几年，processor 也快得多，RAM 也更多。

Amazon 已经招揽了原 Calxeda 团队中相当一部分人，而且招聘规模足以组建一支颇具规模的 hardware design 团队。Facebook 已经招揽了少数几位 ARM SoC 人才，并正在与 Qualcomm 合作做某个项目。[Linus 曾公开表示，我们将会在各处看到更多 dedicated hardware](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146227)。诸如此类。

### Conclusion（结论）

x86 chips 已经增添了许多新的 features 和令人眼花缭乱的新玩意儿。大多数情况下，你不必知道它们是什么，就能利用它们。作为 first-order approximation，让你的 code 变得 predictable，并留意 memory locality，效果就相当不错。那些真正 low-level 的东西通常会被 libraries 或 drivers 隐藏起来，而 compilers 会尝试处理其余部分。例外情况是，你正在 writing 真正 low-level code，此时[这个世界已经变得混乱得多](https://github.com/vishmohan/vmlaunch/blob/master/vmlaunch_simple.c)；或者，你正在尝试从 code 中获得绝对最佳的 performance，此时[这个世界已经变得古怪得多](https://docs.google.com/document/d/18gs0bkEwQ5cO8pMXT_MsOa8Xey4NEavXq-OvtdUXKck/pub)。

另外，将来总会有事情发生。但大多数预测都是错的，所以谁知道呢？

### 参考资料

[这是 Matt Godbolt 的一场演讲](https://www.youtube.com/watch?v=hgcNM-6wr34)，涵盖了许多我没有深入讨论的 implementation details。要再深入一个层次的细节，请参阅 [Modern Processor Design, by Shen and Lipasti](http://www.amazon.com/gp/product/1478607831/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1478607831&linkCode=as2&tag=abroaview-20&linkId=HXBNFFJ3CIXMWUZP)。尽管 Amazon 上列出的日期是 (2013)，这本书其实已经相当老了，但它仍是我找到的关于 processor internals 的最好的书。它相当详细地描述了，要做出一个 P6-era high-performance CPU，你需要 implement 哪些东西。它还在给定不同组 assumptions 的情况下 derives theoretical performance limits，并讨论了许多不同的 engineering tradeoffs，对其中许多也解释了原因。

要再深入一个层次了解“为什么”，你可能需要读一本 VLSI 教材，它会解释 devices 和 interconnect 如何 scale，以及这如何影响 circuit design，进而影响 architecture。我很喜欢 [Weste & Harris](http://www.amazon.com/gp/product/B008VIXPI2/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B008VIXPI2&linkCode=as2&tag=abroaview-20&linkId=YOJVAWVH5XTIF6LN)，因为它们讲解清楚，exercises 也很好，而且你可以在网上找到 solutions；但如果你不打算做这些 problems，那么基本上任何一本 VLSI 教材都可以。要再深入一个层次了解这些事情的“为什么”，你会需要一本 solid state devices 教材，以及解释 transmission lines 和 interconnect 如何工作的资料。关于 devices，我很喜欢 [Pierret 的](http://www.amazon.com/gp/product/0201543931/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201543931&linkCode=as2&tag=abroaview-20&linkId=MXV5K7IJXWXJD446)[书](http://www.amazon.com/gp/product/013061792X/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=013061792X&linkCode=as2&tag=abroaview-20&linkId=N7YLCANVDPI3M35R)。我是通过 [Ramo, Whinnery & Van Duzer](http://www.amazon.com/gp/product/8126515252/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=8126515252&linkCode=as2&tag=abroaview-20&linkId=XZUQXDRDJVABGITB) 接触 E-mag 这些内容的，但 [Ida](http://www.amazon.com/gp/product/3319078054/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=3319078054&linkCode=as2&tag=abroaview-20&linkId=GTQSYCVTMIFZKSPZ) 是更好的入门教材。

关于当前这一代 CPUs 的具体情况，以及具体的 optimization techniques，[请参阅 Agner Fog 的网站](http://www.agner.org/optimize/)。关于[来自未来的 optimization tools，请看这篇文章](https://danluu.com/perf-tracing/)。[What Every Programmer Should Know About Memory](http://www.akkadia.org/drepper/cpumemory.pdf) 也是很好的背景读物。这些 docs 涵盖了许多重要内容，但如果你用 higher level language 编写程序，[还有许多其他事情需要牢记](http://mrale.ph/blog/2015/01/11/whats-up-with-monomorphism.html)。关于 Intel CPU 的更多历史，Xao-Feng Li [提供了一篇很好的概述](https://people.apache.org/~xli/presentations/history_Intel_CPU.pdf)。

要看一些有点出人意料的内容，请看[这篇讨论 **CPU backdoors** 的可能性的文章](https://danluu.com/cpu-backdoors/)。要看一些不那么出人意料的内容，请看[这篇讨论现代 CPUs 的 complexity 如何使**各种令人兴奋的 bugs** 成为可能的文章](https://danluu.com/cpu-bugs/)。

要看更多关于 locking 的 benchmarks，请看 [Aleksey Shipilev 的这篇文章](http://shipilev.net/blog/2014/on-the-fence-with-dependencies/)、[Paul Khuong 的这篇文章](http://www.pvk.ca/Blog/2015/01/13/lock-free-mutual-exclusion/)，以及他们的文章归档。

关于一般的 benchmarking，去年 [Aysylu Greenberg 在 Strange Loop 上关于 benchmarking 的演讲](https://www.youtube.com/watch?v=XmImGiVuJno) 很好地介绍了常见的陷阱。要看更进阶、但也更具体的内容，[Gil Tene 关于 latency 的演讲](https://www.youtube.com/watch?v=9MKY4KypBzg) 很棒。

要了解比我提到的所有内容都早得多的 computing 历史，请看 [IBM's Early Computers](http://www.amazon.com/gp/product/0262523930/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0262523930&linkCode=as2&tag=abroaview-20&linkId=NB4TSSXVLME77X4B) 和 [Design of a Computer](http://ygdes.com/CDC/DesignOfAComputer_CDC6600.pdf)，后者描述了 CDC 6600 的 design。[Readings in Computer Architecture](http://www.amazon.com/gp/product/1558605398/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1558605398&linkCode=as2&tag=abroaview-20&linkId=MCCNUSAAP4B6NZAP) 也很适合用来了解其中许多想法最初从何而来。

抱歉，这份清单很不完整。欢迎提出建议！

#### 小小的免责声明

我毫不怀疑自己遗漏了一些东西。如果我遗漏了你认为重要的内容，[请告诉我](https://twitter.com/danluu/)，我会更新本文。我努力在保留事情原貌的同时，把内容讲得尽可能简单，但我确信，有些地方我过度简化了，也有些事情我完全忘记提及。当然，如果你要求_真正_ precise，那么我做出的几乎每一个 generalization 都是错的。哪怕只挑我开头几句话的毛病，A20M 也并非在任何时候、任何地方都无关紧要（我的职业生涯中，大概有 0.2% 的时间花在处理它上面），x86-64 也并非 strictly superior to x86（在我处理过的一个 workload 上，额外 registers 带来的 performance benefit，被更长 instructions 的 cost 抵消后还绰绰有余；instruction stream 和 icache misses 成为一个 workload 的主要瓶颈的情况相当少见，但确实会发生），等等。问题最大的可能是我对 NUMA 的解释，因为 P6 busses 实际上可以用 defer 或 retry 来 respond 一个 request。避免使用类似的 mechanism 来 enforce coherency 是合理的，但我想不出一个不涉及多层解释、又能合理说明原因的解释。我说只要挖得足够深，几乎每一个 generalization 都会站不住脚，真的不是在开玩笑。我讨论的每一个 abstraction 都是 leaky。我努力附上至少再深入一个层次的 docs 链接，但我可能还是遗漏了一些领域。

#### 致谢

感谢 Leah Hanson 和 Nathan Kurz，他们的意见促成了大幅修改；感谢 Nicholas Radcliffe、Stefan Kanthak、Garret Reid、Matt Godbolt、Nikos Patsiouras、Aleksey Shipilev 和 Oscar R Moll，他们的意见促成了小幅修改；感谢 David Albert 允许我引用他的话，也感谢他在前些时候我们讨论这个话题时提出的一些有趣的后续问题。此外，感谢 Cliff Burdick 撰写了关于 GPUs 的章节，也感谢 Hari Angepat 发现了 TensorFlow 中的 Google kOpenCLAltera code。

* * *

---

## 原文

This is a response to the following question from David Albert:

> My mental model of CPUs is stuck in the 1980s: basically boxes that do arithmetic, logic, bit twiddling and shifting, and loading and storing things in memory. I'm vaguely aware of various newer developments like vector instructions (SIMD) and the idea that newer CPUs have support for virtualization (though I have no idea what that means in practice).
> 
> What cool developments have I been missing? What can today's CPU do that last year's CPU couldn't? How about a CPU from two years ago, five years ago, or ten years ago? The things I'm most interested in are things that programmers have to manually take advantage of (or programming environments have to be redesigned to take advantage of) in order to use and as a result might not be using yet. I think this excludes things like Hyper-threading/SMT, but I'm not honestly sure. I'm also interested in things that CPUs can't do yet but will be able to do in the near future.

Everything below refers to x86 and linux, unless otherwise indicated. History has a tendency to repeat itself, and a lot of things that were new to x86 were old hat to supercomputing, mainframe, and workstation folks.

### The Present

#### Miscellania

For one thing, chips have wider registers and can address more memory. In the 80s, you might have used an 8-bit CPU, but now you almost certainly have a [64-bit CPU](http://courses.cs.washington.edu/courses/csep590/06au/projects/history-64-bit.pdf) in your machine. I'm not going to talk about this too much, since I assume you're familiar with programming a 64-bit machine. In addition to providing more address space, 64-bit mode provides more registers and more consistent floating point results (via the avoidance of pseudo-randomly getting 80-bit precision for 32 and 64 bit operations via x87 floating point). Other things that you're very likely to be using that were introduced to x86 since the early 80s include paging / virtual memory, pipelining, and floating point.

#### Esoterica

I'm also going to avoid discussing things that are now irrelevant (like A20M) and things that will only affect your life if you're writing drivers, BIOS code, doing security audits, or other unusually low-level stuff (like APIC/x2APIC, SMM, NX, or [SGX](https://eprint.iacr.org/2016/086)).

#### Memory / Caches

Of the remaining topics, the one that's most likely to have a real effect on day-to-day programming is how memory works. My first computer was a 286. On that machine, a memory access might take a few cycles. A few years back, I used a Pentium 4 system where a memory access took more than 400 cycles. Processors have sped up a lot more than memory. The solution to the problem of having relatively slow memory has been to add caching, which provides fast access to frequently used data, and prefetching, which preloads data into caches if the access pattern is predictable.

A few cycles vs. 400+ cycles sounds really bad; that's well over 100x slower. But if I write a dumb loop that reads and operates on a large block of 64-bit (8-byte) values, the CPU is smart enough to prefetch the correct data before I need it, which lets me process at about [22 GB/s](https://danluu.com/assembly-intrinsics/) on my 3GHz processor. A calculation that can consume 8 bytes every cycle at 3GHz only works out to 24GB/s, so getting 22GB/s isn't so bad. We're losing something like 8% performance by having to go to main memory, not 100x.

As a first-order approximation, using predictable memory access patterns and operating on chunks of data that are smaller than your CPU cache will get you most of the benefit of modern caches. If you want to squeeze out as much performance as possible, [this document](http://people.freebsd.org/~lstewart/articles/cpumemory.pdf) is a good starting point. After digesting that 100 page PDF, you'll want to familiarize yourself with the microarchitecture and memory subsystem of the system you're optimizing for, and learn how to profile the performance of your application with something like [likwid](https://code.google.com/p/likwid/).

#### TLBs

There are lots of little caches on the chip for all sorts of things, not just main memory. You don't need to know about the decoded instruction cache and other funny little caches unless you're really going all out on micro-optimizations. The big exception is the TLBs, which are caches for virtual memory lookups (done via a 4-level page table structure on x86). Even if the page tables were in the l1-data cache, that would be 4 cycles per lookup, or 16 cycles to do an entire virtual address lookup each time around. That's totally unacceptable for something that's required for all user-mode memory accesses, so there are small, fast, caches for virtual address lookups.

Because the first level TLB cache has to be fast, it's severely limited in size (perhaps 64 entries on a modern chip). If you use 4k pages, that limits the amount of memory you can address without incurring a TLB miss. x86 also supports 2MB and 1GB pages; some applications will benefit a lot from using larger page sizes. It's something worth looking into if you've got a long-running application that uses a lot of memory.

Also, first-level caches are usually limited by the page size times [the associativity of the cache](https://danluu.com/3c-conflict/). If the cache is smaller than that, the bits used to index into the cache are the same regardless if whether you're looking at the virtual address or the physical address, so you don't have to do a virtual to physical translation before indexing into the cache. If the cache is larger than that, you have to first do a TLB lookup to index into the cache (which will cost at least one extra cycle), or build a virtually indexed cache (which is possible, but adds complexity and coupling to software). You can see this limit in modern chips. Haswell has an 8-way associative cache and 4kB pages. Its l1 data cache is `8 * 4kB = 32kB`.

#### Out of Order Execution / Serialization

For a couple decades now, x86 chips have been able to speculatively execute and re-order execution (to avoid blocking on a single stalled resource). This sometimes results in [odd performance hiccups](https://docs.google.com/document/d/18gs0bkEwQ5cO8pMXT_MsOa8Xey4NEavXq-OvtdUXKck/pub). But x86 is pretty strict in requiring that, for a single CPU, externally visible state, like registers and memory, must be updated as if everything were executed in order. The implementation of this involves making sure that, for any pair of instructions with a dependency, those instructions execute in the correct order with respect to each other.

That restriction that things look like they executed in order means that, for the most part, you can ignore the existence of OoO execution unless you're trying to eke out the best possible performance. The major exceptions are when you need to make sure something not only looks like it executed in order externally, but actually executed in order internally.

An example of when you might care would be if you're trying to measure the execution time of a sequence of instructions using `rdtsc`. `rdtsc` reads a hidden internal counter and puts the result into `edx` and `eax`, externally visible registers.

Say we do something like

```
foo
rdtsc
bar
mov %eax, [%ebx]
baz
```

where foo, bar, and baz don't touch `eax`, `edx`, or `[%ebx]`. The mov that follows the rdtsc will write the value of `eax` to some location in memory, and because `eax` is an externally visible register, the CPU will guarantee that the `mov` doesn't execute until after `rdtsc` has executed, so that everything looks like it happened in order.

However, since there isn't an explicit dependency between the `rdtsc` and either `foo` or `bar`, the `rdtsc` could execute before `foo`, between `foo` and `bar`, or after `bar`. It could even be the case that `baz` executes before the `rdtsc`, as long as `baz` doesn't affect the move instruction in any way. There are some circumstances where that would be fine, but it's not fine if the `rdtsc` is there to measure the execution time of `foo`.

To precisely order the `rdtsc` with respect to other instructions, we need to an instruction that serializes execution. Precise details on how exactly to do that [are provided in this document by Intel](http://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-32-ia-64-benchmark-code-execution-paper.pdf).

#### Memory / Concurrency

In addition to the ordering restrictions above, which imply that loads and stores to the same location can't be reordered with respect to each other, x86 loads and stores have some other restrictions. In particular, for a single CPU, stores are never reordered with other stores, and stores are never reordered with earlier loads, regardless of whether or not they're to the same location.

However, loads can be reordered with earlier stores. For example, if you write

```
mov 1, [%esp]
mov [%ebx], %eax
```

it can be executed as if you wrote

```
mov [%ebx], %eax
mov 1, [%esp]
```

But the converse isn't true — if you write the latter, it can never be executed as if you wrote the former.

You could force the first example to execute as written by inserting a serializing instruction. But that requires the CPU to serialize all instructions. But that's slow, since it effectively forces the CPU to wait until all instructions before the serializing instruction are done before executing anything after the serializing instruction. There's also an `mfence` instruction that only serializes loads and stores, if you only care about load/store ordering.

I'm not going to discuss the other memory fences, lfence and sfence, [but you can read more about them here](https://stackoverflow.com/q/20316124/334816).

We've looked at single core ordering, where loads and stores are mostly ordered, but there's also multi-core ordering. The above restrictions all apply; if core0 is observing core1, it will see that all of the single core rules apply to core1's loads and stores. However, if core0 and core1 interact, there's no guarantee that their interaction is ordered.

For example, say that core0 and core 1 start with `eax` and `edx` set to 0, and core 0 executes

```
mov 1, [_foo]
mov [_foo], %eax
mov [_bar], %edx
```

while core1 executes

```
mov 1, [_bar]
mov [_bar], %eax
mov [_foo], %edx
```

For both cores, `eax` has to be `1` because of the within-core dependency between the first instruction and the second instruction. However, it's possible for `edx` to be `0` in both cores because line 3 of core0 can execute before core0 sees anything from core1, and visa versa.

That covers memory barriers, which serialize memory accesses within a core. Since stores are required to be seen in a consistent order across cores, they can, they also have an effect on cross-core concurrency, but it's pretty difficult to reason about that kind of thing correctly. [Linus has this to say on using memory barriers instead of locking](http://yarchive.net/comp/linux/locking.html):

> The _real_ cost of not locking also often ends up being the inevitable bugs. Doing clever things with memory barriers is almost always a bug waiting to happen. It's just _really_ hard to wrap your head around all the things that can happen on ten different architectures with different memory ordering, and a single missing barrier. … The fact is, any time anybody makes up a new locking mechanism, THEY ALWAYS GET IT WRONG. Don't do it.

And it turns out that on modern x86 CPUs, using locking to implement concurrency primitives is [often cheaper than using memory barriers](https://blogs.oracle.com/dave/resource/NHM-Pipeline-Blog-V2.txt), so let's look at locks.

If we set `_foo` to 0 and have two threads that both execute `incl (_foo)` 10000 times each, incrementing the same location with a single instruction 20000 times, is guaranteed not to exceed 20000, but it could (theoretically) be as low as 2. If it's not obvious why the theoretical minimum is 2 and not 10000, figuring that out is a good exercise. If it is obvious, my bonus exercise for you is, can any reasonable CPU implementation get that result, or is that some silly thing the spec allows that will never happen? There isn't enough information in this post to answer the bonus question, but I believe I've linked to enough information.

We can try this with a simple code snippet

```
#include <stdlib.h>
#include <thread>

#define NUM_ITERS 10000
#define NUM_THREADS 2

int counter = 0;
int *p_counter = &counter;

void asm_inc() {
  int *p_counter = &counter;
  for (int i = 0; i < NUM_ITERS; ++i) {
    __asm__("incl (%0) \n\t" : : "r" (p_counter));
  }
}

int main () {
  std::thread t[NUM_THREADS];
  for (int i = 0; i < NUM_THREADS; ++i) {
    t[i] = std::thread(asm_inc);
  }
  for (int i = 0; i < NUM_THREADS; ++i) {
    t[i].join();
  }
  printf("Counter value: %i\n", counter);
  return 0;
}
```

Compiling the above with `clang++ -std=c++11 -pthread`, I get the following distribution of results on two of my machines:

![Different distributions of non-determinism on Haswell and Sandy Bridge](https://danluu.com/images/new-cpu-features/write-contention.webp)

Not only do the results vary between runs, the distribution of results is different on different machines. We never hit the theoretical minimum of 2, or for that matter, anything below 10000, but there's some chance of getting a final result anywhere between 10000 and 20000.

Even though `incl` is a single instruction, it's not guaranteed to be atomic. Internally, `incl` is implemented as a load followed by an add followed by an store. It's possible for an increment on cpu0 to sneak in and execute between the load and the store on cpu1 and visa versa.

The solution Intel has for this is the `lock` prefix, which can be added to a handful of instructions to make them atomic. If we take the above code and turn `incl` into `lock incl`, the resulting output is always 20000.

So, that's how we make a single instruction atomic. To make a sequence atomic, we can use `xchg` or `cmpxchg`, which are always locked as compare-and-swap primitives. I won't go into detail about how that works, but see [this article by David Dalrymple](http://davidad.github.io/blog/2014/03/23/concurrency-primitives-in-intel-64-assembly/) if you're curious..

In addition to making a memory transaction atomic, locks are globally ordered with respect to each other, and loads and stores aren't re-ordered with respect to locks.

For a rigorous model of memory ordering, [see the x86 TSO doc](http://www.cl.cam.ac.uk/~pes20/weakmemory/x86tso-paper.pdf).

All of this discussion has been how about how concurrency works in hardware. Although there are limitations on what x86 will re-order, compilers don't necessarily have those same limitations. In C or C++, you'll need to insert the appropriate primitives to make sure the compiler doesn't re-order anything. [As Linus points out here](http://yarchive.net/comp/linux/memory_barriers.html), if you have code like

```
local_cpu_lock = 1;
// .. do something critical ..
local_cpu_lock = 0;
```

the compiler has no idea that `local_cpu_lock = 0` can't be pushed into the middle of the critical section. Compiler barriers are distinct from CPU memory barriers. Since the x86 memory model is relatively strict, some compiler barriers are no-ops at the hardware level that tell the compiler not to re-order things. If you're using a language that's higher level than microcode, assembly, C, or C++, your compiler probably handles this for you without any kind of annotation.

#### Memory / Porting

If you're porting code to other architectures, it's important to note that x86 has one of the strongest memory models of any architecture you're likely to encounter nowadays. If you write code that just works without thinking it through and port it to architectures that have weaker guarantees (PPC, ARM, or Alpha), you'll almost certainly have bugs.

Consider this example:

```
Initial
-----
x = 1;
y = 0;
p = &x;

CPU1         CPU2
----         ----
i = *p;      y = 1;
             MB;
             p = &y;
```

`MB` is a memory barrier. On an Alpha 21264 system, this can result in `i = 0`.

Kourosh Gharachorloo explains how:

> CPU2 does y=1 which causes an "invalidate y" to be sent to CPU1. This invalidate goes into the incoming "probe queue" of CPU1; as you will see, the problem arises because this invalidate could theoretically sit in the probe queue without doing an MB on CPU1. The invalidate is acknowledged right away at this point (i.e., you don't wait for it to actually invalidate the copy in CPU1's cache before sending the acknowledgment). Therefore, CPU2 can go through its MB. And it proceeds to do the write to p. Now CPU1 proceeds to read p. The reply for read p is allowed to bypass the probe queue on CPU1 on its incoming path (this allows replies/data to get back to the 21264 quickly without needing to wait for previous incoming probes to be serviced). Now, CPU1 can derefence p to read the old value of y that is sitting in its cache (the invalidate y in CPU1's probe queue is still sitting there).
> 
> How does an MB on CPU1 fix this? The 21264 flushes its incoming probe queue (i.e., services any pending messages in there) at every MB. Hence, after the read of p, you do an MB which pulls in the invalidate to y for sure. And you can no longer see the old cached value for y.
> 
> Even though the above scenario is theoretically possible, the chances of observing a problem due to it are extremely minute. The reason is that even if you setup the caching properly, CPU1 will likely have ample opportunity to service the messages (i.e., invalidate) in its probe queue before it receives the data reply for "read p". Nonetheless, if you get into a situation where you have placed many things in CPU1's probe queue ahead of the invalidate to y, then it is possible that the reply to p comes back and bypasses this invalidate. It would be difficult for you to set up the scenario though and actually observe the anomaly.

This is long enough without my talking about other architectures so I won't go into detail, but if you're wondering why anyone would create a spec that allows this kind of optimization, consider that before rising fab costs crushed DEC, their chips were so fast that they could [run industry standard x86 benchmarks of real workloads in emulation faster than x86 chips could run the same benchmarks natively](https://www.usenix.org/legacy/publications/library/proceedings/usenix-nt97/full_papers/chernoff/chernoff.pdf). For more explanation of why the most RISC-y architecture of the time made the decisions it did, see [this paper on the motivations behind the Alpha architecture](https://danluu.com/dick-sites-alpha-axp-architecture.pdf).

BTW, this is a major reason I'm skeptical of the Mill architecture. Putting aside arguments about whether or not they'll live up to their performance claims, being technically excellent isn't, in and of itself, a business model.

#### Memory / Non-Temporal Stores / Write-Combine Memory

The set of restrictions outlined in the previous section apply to cacheable (i.e., “write-back” or WB) memory. That, itself, was new at one time. Before that, there was only uncacheable (UC) memory.

One of the interesting things about UC memory is that all loads and stores are expected to go out to the bus. That's perfectly reasonable in a processor with no cache and little to no on-board buffering. A result of that is that devices that have access to memory can rely on all accesses to UC memory regions creating separate bus transactions, in order (because some devices will use a memory read or write as as trigger to do something). That worked great in 1982, but it's not so great if you have a video card that just wants to snarf down whatever the latest update is. If multiple writes happen to the same UC location (or different bytes of the same word), the CPU is required to issue a separate bus transaction for each write, even though a video card doesn't really care about seeing each intervening result.

The solution to that was to create a memory type called write combine (WC). WC is a kind of eventually consistent UC. Writes have to eventually make it to memory, but they can be buffered internally. WC memory also has weaker ordering guarantees than UC.

For the most part, you don't have to deal with this unless you're talking directly with devices. The one exception are “non-temporal” load and store operations. These make particular loads and stores act like they're to WC memory, even if the address is in a memory region that's marked WB.

This is useful if you don't want to pollute your caches with something. This is often useful if you're doing [some kind of streaming calculation](http://blogs.fau.de/hager/archives/2103) where you know you're not going to use a particular piece of data more than once.

#### Memory / NUMA

Non-uniform memory access, where memory latencies and bandwidth are different for different processors, is so common that we mostly don't talk about NUMA or ccNUMA anymore because they're so common that it's assumed to be the default.

The takeaway here is that threads that share memory should be on the same socket, and a memory-mapped I/O heavy thread should make sure it's on the socket that's closest to the I/O device it's talking to.

I've mostly avoided explaining the _why_ behind things because that would make this post at least an order of magnitude longer than it's going to be. But I'll give a vastly oversimplified explanation of why we have NUMA systems, partially because it's a self-contained thing that's relatively easy to explain and partially to demonstrate how long the why is compared to the what.

Once upon a time, there was just memory. Then CPUs got fast enough relative to memory that people wanted to add a cache. It's bad news if the cache is inconsistent with the backing store (memory), so the cache has to keep some information about what it's holding on to so it knows if/when it needs to write things to the backing store.

That's not too bad, but once you get 2 cores with their own caches, it gets a little more complicated. To maintain the same programming model as the no-cache case, the caches have to be consistent with each other and with the backing store. Because existing load/store instructions have nothing in their API that allows them to say `sorry! this load failed because some other CPU is holding onto the address you want`, the simplest thing was to have every CPU send a message out onto the bus every time it wanted to load or store something. We've already got this memory bus that both CPUs are connected to, so we just require that other CPUs respond with the data (and invalidate the appropriate cache line) if they have a modified version of the data in their cache.

That works ok. Most of the time, each CPU only touches data the other CPU doesn't care about, so there's some wasted bus traffic. But it's not too bad because once a CPU puts out a message saying `Hi! I'm going to take this address and modify the data`, it can assume it completely owns that address until some other CPU asks for it, which will probably won't happen. And instead of doing things on a single memory address, we can operate on cache lines that have, say, 64 bytes. So, the overall overhead is pretty low.

It still works ok for 4 CPUs, although the overhead is a bit worse. But this thing where each CPU has to respond to every other CPU's fails to scale much beyond 4 CPUs, both because the bus gets saturated and because the caches will get saturated (the physical size/cost of a cache is `O(n^2)` in the number of simultaneous reads and write supported, and the speed is inversely correlated to the size).

A “simple” solution to this problem is to have a single centralized directory that keeps track of all the information, instead of doing N-way peer-to-peer broadcast. Since we're packing 2-16 cores on a chip now anyway, it's pretty natural to have a single directory per chip (socket) that tracks the state of the caches for every core on a chip.

This only solves the problem for each chip, and we need some way for the chips to talk to each other. Unfortunately, while we were scaling these systems up the bus speeds got fast enough that it's really difficult to drive a signal far enough to connect up a bunch of chips and memory all on one bus, even for small systems. The simplest solution to that is to have each socket own a region of memory, so every socket doesn't need to be connected to every part of memory. This also avoids the complexity of needed a higher level directory of directories, since it's clear which directory owns any particular piece of memory.

The disadvantage of this is that if you're sitting in one socket and want some memory owned by another socket, you have a significant performance penalty. For simplicity, most “small” (< 128 core) systems use ring-like busses, so the performance penalty isn't just the direct latency/bandwidth penalty you pay for walking through a bunch of extra hops to get to memory, it also uses up a finite resource (the ring-like bus) and slows down other cross-socket accesses.

In theory, the OS handles this transparently, but it's [often inefficient](https://www.usenix.org/conference/atc14/technical-sessions/presentation/gaud).

#### Context Switches / Syscalls

Here, syscall refers to a linux system call, not the `SYSCALL` or `SYSENTER` x86 instructions.

A side effect of all the caching that modern cores have is that [context switches are expensive](http://blog.tsunanet.net/2010/11/how-long-does-it-take-to-make-context.html), which causes syscalls to be expensive. Livio Soares and Michael Stumm discuss the cost in great detail [in their paper](http://www.cs.cmu.edu/~chensm/Big_Data_reading_group/papers/flexsc-osdi10.pdf). I'm going to use a few of their figures, below. Here's a graph of how many instructions per clock (IPC) a Core i7 achieves on Xalan, a sub-benchmark from SPEC CPU.

![Long tail of overhead from a syscall. 14,000 cycles.](https://danluu.com/images/new-cpu-features/flexsc_ipc_recovery.webp)

14,000 cycles after a syscall, code is still not quite running at full speed.

Here's a table of the footprint of a few different syscalls, both the direct cost (in instructions and cycles), and the indirect cost (from the number of cache and TLB evictions).

![Cost of stat, pread, pwrite, open+close, mmap+munmap, and open+write+close](https://danluu.com/images/new-cpu-features/flexsc_cost.webp)

Some of these syscalls cause 40+ TLB evictions! For a chip with a 64-entry d-TLB, that nearly wipes out the TLB. The cache evictions aren't free, either.

The high cost of syscalls is the reason people have switched to using batched versions of syscalls for high-performance code (e.g., `epoll`, or `recvmmsg`) and the reason that people who need [very high performance I/O](https://danluu.com/clwb-pcommit/) often use user space I/O stacks. More generally, the cost of context switches is why high-performance code is often thread-per-core (or even single threaded on a pinned thread) and not thread-per-logical-task.

This high cost was also the driver behind [vDSO, which turns some simple syscalls that don't require any kind of privilege escalation into simple user space library calls](http://www.linuxjournal.com/content/creating-vdso-colonels-other-chicken).

#### SIMD

Basically all modern x86 CPUs support SSE, 128-bit wide vector registers and instructions. Since it's common to want to do the same operation multiple times, Intel added instructions that will let you operate on a 128-bit chunk of data as 2 64-bit chunks, 4 32-bit chunks, 8 16-bit chunks, etc. ARM supports the same thing with a different name (NEON), and the instructions supported are pretty similar.

It's pretty common to get a 2x-4x speedup from using SIMD instructions; it's definitely worth looking into if you've got a computationally heavy workload.

Compilers are good enough at recognizing common patterns that can be vectorized that simple code, like the following, will automatically use vector instructions with modern compilers

```
for (int i = 0; i < n; ++i) {
  sum += a[i];
}
```

But compilers will [often produce non-optimal code](https://danluu.com/assembly-intrinsics/) if you don't write the assembly by hand, especially for SIMD code, so you'll want to look at the disassembly and check for compiler optimization bugs if you really care about getting the best possible performance.

#### Power Management

There are a lot of fancy power management feature on modern CPUs that [optimize power usage](https://danluu.com/datacenter-power/) in different scenarios. The result of these is that “race to idle”, completing work as fast as possible and then letting the CPU go back to sleep is the most power efficient way to work.

There's been a lot of work that's shown that specific microoptmizations can benefit power consumption, but [applying those microoptimizations on real workloads often results in smaller than expected benefits](http://arcade.cs.columbia.edu/energy-oopsla14.pdf).

#### GPU / GPGPU

I'm even less qualified to talk about this than I am about the rest of this stuff. Luckily, Cliff Burdick volunteered to write a section on GPUs, so here it is.

Prior to the mid-2000's, Graphical Processing Units (GPUs) were restricted to an API that allowed only a very limited amount of control of the hardware. As the libraries became more flexible, programmers began using the processors for more general-purpose tasks, such as linear algebra routines. The parallel architecture of the GPU could work on large chunks of a matrix by launching hundreds of simultaneous threads. However, the code had to use traditional graphics APIs and was still limited in how much of the hardware it could control. Nvidia and ATI took notice and released frameworks that allowed the user to access more of the hardware with an API familiar with people outside of the graphics industry. The libraries gained popularity, and today GPUs are widely used for high-performance computing (HPC) alongside CPUs.

Compared to CPUs, the hardware on GPUs have a few major differences, outlined below:

##### Processors

At the top level, a GPU processor contains one or many streaming multiprocessors (SMs). Each streaming multiprocessor on a modern GPU typically contains over 100 floating point units, or what are typically referred to as cores in the GPU world. Each core is typically clocked around 800MHz, although, like CPUs, processors with higher clock rates but fewer cores are also available. GPU processors lack many features of their CPU counterparts, including large caches and branch prediction. Between the layers of cores, SMs, and the overall processor, communicating becomes increasingly slower. For this reason, problems that perform well on GPUs are typically highly-parallel, but have some amount of data that can be shared between a small number of threads. We'll get into why this is in the memory section below.

##### Memory

Memory on modern GPU is broken up into 3 main categories: global memory, shared memory, and registers. Global memory is the GDDR memory that's advertised on the box of the GPU and is typically around 2-12GB in size, and has a throughput of 300-400GB/s. Global memory can be accessed by all threads across all SMs on the processor, and is also the slowest type of memory on the card. Shared memory is, as the name says, memory that's shared between all threads within the same SM. It is usually at least twice as fast as global memory, but is not accessible between threads on different SMs. Registers are much like registers on a CPU in that they are the fastest way to access data on a GPU, but they are local per thread and the data is not visible to any other running thread. Both shared memory and global memory have very strict rules on how they can be accessed, with severe performance penalties for not following them. To reach the throughputs mentioned above, memory accesses must be completely coalesced between threads within the same thread group. Similar to a CPU reading into a single cache line, GPUs have cache lines sized so that a single access can serve all threads in a group if aligned properly. However, in the worst case where all threads in a group access memory in a different cache line, a separate memory read will be required for each thread. This usually means that most of the data in the cache line is not used by the thread, and the usable throughput of the memory goes down. A similar rule applies to shared memory as well, with a couple exceptions that we won't cover here.

##### Threading Model

GPU threads run in a SIMT (Single Instruction Multiple Thread) fashion, and each thread runs in a group with a pre-defined size in the hardware (typically 32). That last part has many implications; every thread in that group must be working on the same instruction at the same time. If any of the threads in a group need to take a divergent path (an if statement, for example) of code from the others, all threads not part of the branch suspend execution until the branch is complete. As a trivial example:

```
if (threadId < 5) {
   // Do something
}
// Do More
```

In the code above, this branch would cause 27 of our 32 threads in the group to suspend execution until the branch is complete. You can imagine if many groups of threads all run this code, the overall performance will take a large hit while most of the cores sit idle. Only when an entire group of threads is stalled is the hardware allowed to swap in another group to run on those cores.

##### Interfaces

Modern GPUs must have a CPU to copy data to and from CPU and GPU memory, and to launch and code on the GPU. At the highest throughput, a PCIe 3.0 bus with 16 lanes can achieves rates of about 13-14GB/s. This may sound high, but when compared to the memory speeds residing on the GPU itself, they're over an order of magnitude slower. In fact, as GPUs get more powerful, the PCIe bus is increasingly becoming a bottleneck. To see any of the performance benefits the GPU has over a CPU, the GPU must be loaded with a large amount of work so that the time the GPU takes to run the job is significantly higher than the time it takes to copy the data to and from.

Newer GPUs have features to launch work dynamically in GPU code without returning to the CPU, but it's fairly limited in its use at this point.

##### GPU Conclusion

Because of the major architectural differences between CPUs and GPUs, it's hard to imagine either one replacing the other completely. In fact, a GPU complements a CPU well for parallel work and allows the CPU to work independently on other tasks as the GPU is running. AMD is attempting to merge the two technologies with their "Heterogeneous System Architecture" (HSA), but taking existing CPU code and determining how to split it between the CPU and GPU portion of the processor will be a big challenge not only for the processor, but for compilers as well.

#### Virtualization

Since you mentioned virtualization, I'll talk about it a bit, but Intel's implementation of virtualization instructions generally isn't something you need to think about unless you're writing very low-level code that directly deals with virtualization.

Dealing with that stuff is pretty messy, as you can see [from this code](https://github.com/vishmohan/vmlaunch). Setting stuff up to use Intel's VT instructions to launch a VM guest is about 1000 lines of low-level code, even for the very simple case shown there.

#### Virtual Memory

If you look at Vish's VT code, you'll notice that there's a decent chunk of code dedicated to page tables / virtual memory. That's another “new” feature that you don't have to worry about unless you're writing an OS or other low-level systems code. Using virtual memory is much simpler than using segmented memory, but that's not relevant nowadays so I'll just leave it at that.

#### SMT / Hyper-threading

Since you brought it up, I'll also mention SMT. As you said, this is mostly transparent for programmers. A typical speedup for enabling SMT on a single core is around 25%. That's good for overall throughput, but it means that each thread might only get 60% of its original performance. For applications where you care a lot about single-threaded performance, you might be better off disabling SMT. It depends a lot on the workload, though, and as with any other changes, you should run some benchmarks on your exact workload to see what works best.

One side effect of all this complexity that's been added to chips (and software) is that performance is a lot less predictable than it used to be; the relative importance of benchmarking your exact workload on the specific hardware it's going to run on has gone up.

Just for example, people often point to benchmarks from the [Computer Languages Benchmarks Game](http://benchmarksgame.alioth.debian.org/) as evidence that one language is faster than another. I've tried reproducing the results myself, and on my mobile Haswell (as opposed to the server Kentsfield that's used in the results), I get results that are different by as much as 2x (in relative speed). Running the same benchmark on the same machine, Nathan Kurz recently pointed me to an example where `gcc -O3` is 25% slower than `gcc -O2`. [Changing the linking order on C++ programs can cause a 15% performance change](http://www-plan.cs.colorado.edu/klipto/mytkowicz-asplos09.pdf). Benchmarking is a hard problem.

#### Branches

Old school conventional wisdom is that branches are expensive, and should be avoided at all (or most) costs. On a Haswell, the branch misprediction penalty is 14 cycles. Branch mispredict rates depend on the workload. Using `perf stat` on a few different things (bzip2, top, mysqld, regenerating my blog), I get branch mispredict rates of between 0.5% and 4%. If we say that a correctly predicted branch costs 1 cycle, that's an average cost of between `.995 * 1 + .005 * 14 = 1.065 cycles` to `.96 * 1 + .04 * 14 = 1.52 cycles`. That's not so bad.

This actually overstates the penalty since about 1995, since Intel added conditional move instructions that allow you to conditionally move data without a branch. This instruction was memorably [panned by Linus](http://yarchive.net/comp/linux/cmov.html), which has given it a bad reputation, but it's fairly common to [get significant speedups using cmov compared to branches](https://github.com/logicchains/LPATHBench/issues/53#issuecomment-68160081)

A real-world example of the cost of extra branches are enabling integer overflow checks. When using [bzip2 to compress a particular file, that increases the number of instructions by about 30% (with all of the increase coming from extra branch instructions), which results in a 1% performance hit](https://danluu.com/integer-overflow/).

Unpredictable branches are bad, but most branches are predictable. Ignoring the cost of branches until your profiler tells you that you have a hot spot is pretty reasonable nowadays. CPUs have gotten a lot better at executing poorly optimized code over the past decade, and compilers are getting better at optimizing code, which makes optimizing branches a poor use of time unless you're trying to squeeze out the absolute best possible performance out of some code.

If it turns out that's what you need to do, you're likely to be better off using [profile-guided optimization](http://en.wikipedia.org/wiki/Profile-guided_optimization) than trying to screw with this stuff by hand.

If you really must do this by hand, there are compiler directives you can use to say whether a particular branch is likely to be taken or not. Modern CPUs ignore branch hint instructions, but they can help the compiler lay out code better.

#### Alignment

Old school conventional wisdom is that you should pad out structs and make sure things are aligned. But on a Haswell chip, the mis-alignment for almost any single-threaded thing you can think of that doesn't cross a page boundary is zero. There are some cases where it can make a difference, but in general, this is another type of optimization that's mostly irrelevant because CPUs have gotten so much better at executing bad code. It's also mildly harmful in cases where it increases the memory footprint for no benefit.

Also, [don't make things page aligned or otherwise aligned to large boundaries or you'll destroy the performance of your caches](https://danluu.com/3c-conflict/).

#### Self-modifying code

Here's another optimization that doesn't really make sense anymore. Using self-modifying code to decrease code size or increase performance used to make sense, but because modern caches tend to split up their l1 instruction and data caches, modifying running code requires expensive communication between a chip's l1 caches.

### The Future

Here are some possible changes, from least speculative to most speculative.

#### Partitioning

It's now obvious that more and more compute is moving into large datacenters. Sometimes this involves running on VMs, sometimes it involves running in some kind of container, and sometimes it involves running bare metal, but in any case, individual machines are often multiplexed to run a wide variety of workloads. Ideally, you'd be able to schedule best effort workloads to soak up stranded resources without effecting latency sensitive workloads with an SLA. It turns out that you can actually do this with some [relatively straightforward hardware changes](https://danluu.com/intel-cat/).

![90% overall machine utilization](https://danluu.com/images/new-cpu-features/google_heracles.webp)

[David Lo, et. al, were able to show that you can get about 90% machine utilization without impacting latency SLAs](http://csl.stanford.edu/~christos/publications/2015.heracles.isca.pdf) if caches can be partitioned such that best effort workloads don't impact latency sensitive workloads. The solid red line is the load on a normal Google web search cluster, and the dashed green line is what you get with the appropriate optimizations. From bar-room conversations, my impression is that the solid red line is actually already better (higher) than most of Google's competitors are able to do. If you compare the 90% optimized utilization to typical server utilization of 10% to 90%, that results in a massive difference in cost per unit of work compared to running a naive, unoptimized, setup. With substantial hardware effort, Google was able to avoid interference, but additional isolation features could allow this to be done at higher efficiency with less effort.

#### Transactional Memory and Hardware Lock Elision

IBM already has these features in their POWER chips. Intel made an attempt to add these to Haswell, but they're disabled because of a bug. In general, modern CPUs are quite complex [and we should expect to see many more bugs than we used to](https://danluu.com/cpu-bugs/).

Transactional memory support is what it sounds like: hardware support for transactions. This is through three new instructions, `xbegin`, `xend`, and `xabort`.

`xbegin` starts a new transaction. A conflict (or an `xabort`) causes the architectural state of the processor (including memory) to get rolled back to the state it was in just prior to the `xbegin`. If you're using transactional memory via library or language support, this should be transparent to you. If you're implementing the library support, you'll have to figure out how to convert this hardware support, with its limited hardware buffer sizes, to something that will handle arbitrary transactions.

I'm not going to discuss Hardware Lock Elision except to say that, under the hood, it's implemented with mechanisms that are really similar to the mechanisms used to implement transactional memory and that it's designed to speed up lock-based code. If you want to take advantage of HLE, see [this doc](http://mcg.cs.tau.ac.il/papers/amir-levy-msc.pdf).

#### Fast I/O

I/O bandwidth is going up and I/O latencies are going down, both for storage and for networking. The problem is that I/O is normally done via syscalls. As we've seen, the relative overhead of syscalls has been going up. For both storage and networking, the answer is to move to [user mode I/O stacks](http://research.microsoft.com/pubs/198366/Asplos2012_MonetaD.pdf) (putting everything in kernel mode would work, too, but that's a harder sell). On the storage side, that's mostly still a weirdo research thing, but HPC and HFT folks have been doing that in networking for a while. And by a while, I don't mean a few months. Here's [a paper from 2005 that talks about the networking stuff I'm going to discuss, as well as some stuff I'm not going to discuss (DCA)](http://web.stanford.edu/group/comparch/papers/huggahalli05.pdf).

This is finally trickling into the non-supercomputing world. MS has been advertising Azure with infiniband networking with virtualized RDMA for over a year, Cloudflare [has talked about using Solarflare NICs to get the same capability](http://blog.cloudflare.com/a-tour-inside-cloudflares-latest-generation-servers/), etc. Eventually, we're going to see SoCs with fast Ethernet onboard, and unless that's limited to Xeon-type devices, it's going to trick down into all devices. The competition between ARM devices will probably cause at least one ARM device maker to put fast Ethernet on their commodity SoCs, which may force Intel's hand.

That RDMA bit is significant; it lets you bypass the CPU completely and have the NIC respond to remote requests. A couple months ago, I worked through the Stanford/Coursera Mining Massive Data Sets class. During one of the first lectures, they provide an example of a “typical” datacenter setup with 1Gb top-of-rack switches. That's not unreasonable for processing “massive” data if you're doing kernel TCP through non-RDMA NICs, since you can floor an entire core trying to push 1Gb/s through linux's TCP stack. But with Azure, MS talks about getting 40Gb out of a single machine; that's one machine getting 40x the bandwidth of what you might expect out of an entire rack. They also mention sub 2 us latencies, which is multiple orders of magnitude lower than you can get out of kernel TCP. This isn't exactly a new idea. [This paper from 2011](http://www.scs.stanford.edu/~rumble/papers/latency_hotos11.pdf) predicts everything that's happened on the network side so far, along with some things that are still a ways off.

[This MS talk discusses how you can take advantage of this kind of bandwidth and latency](https://www.youtube.com/watch?v=8Kyoj3bKepY&feature=youtu.be&t=20m8s) for network storage. A concrete example that doesn't require clicking through to a link is Amazon's EBS. It lets you use an “elastic” disk of arbitrary size on any of your AWS nodes. Since a spinning metal disk seek has higher latency than an RPC over kernel TCP, you can get infinite storage pretty much transparently. For example, say you can get [100us (.1ms) latency out of your network](http://www.scs.stanford.edu/~rumble/papers/latency_hotos11.pdf), and your disk seek time is 8ms. That makes a remote disk access 8.1ms instead of 8ms, which isn't that much overhead. That doesn't work so well with SSDs, though, since you can get 20 us (.02ms) [out of an SSD](http://www.anandtech.com/show/8104/intel-ssd-dc-p3700-review-the-pcie-ssd-transition-begins-with-nvme/3). But RDMA latency is low enough that a transparent EBS-like layer is possible for SSDs.

So that's networked I/O. The performance benefit might be even bigger on the disk side, if/when next generation storage technologies that are faster than flash start getting deployed. The performance delta is so large that [Intel is adding new instructions to keep up with next generation low-latency storage technology](https://danluu.com/clwb-pcommit/). Depending on who you ask, that stuff has been a few years away for a decade or two; this is more iffy than the networking stuff. But even with flash, people are showing off devices that can get down into the single microsecond range for latency, which is a substantial improvement.

#### Hardware Acceleration

Like fast networked I/O, this is already here in some niches. [DESRES has been doing ASICs to get 100x-1000x speedup in computational chemistry for years](http://www.deshawresearch.com/publications.html). [Microsoft has talked about speeding up search with FPGAs](http://research.microsoft.com/apps/pubs/default.aspx?id=212001). People have been [looking into accelerating memcached and similar systems for a while](http://web.eecs.umich.edu/~twenisch/papers/isca13.pdf), [researchers from Toshiba and Stanford demonstrated a real implementation a while back](http://csl.stanford.edu/~christos/publications/2014.hwkvs.nvmw.slides.pdf), and I recently saw a pre-print out of Berkeley on the same thing. There are multiple companies making Bitcoin mining ASICs. That's also true for [other](http://www.neuflow.org/) [application](http://www.artificiallearning.com/products/) [areas](https://github.com/cambridgehackers/).

It seems like we should see more of this as it gets harder to get power/performance gains out of CPUs. You might consider this a dodge of your question, if you think of programming as being a software oriented endeavor, but another way to look at it is that what it means to program something will change. In the future, it might mean designing hardware like an FPGA or ASIC in combination with writing software.

##### Update

Now that it's 2016, one year after this post was originally published, we can see that companies are investing in hardware accelerators. In addition to its previous work on FPGA accelerated search, Microsoft [has announced that it's using FPGAs to accelerate networking](https://www.youtube.com/watch?v=RffHFIhg5Sc&feature=youtu.be&t=23m30s). Google has been closed mouthed about infrastructure, as is typical for them, but if you look at the initial release of Tensorflow, you can see snippets of code that clearly references FPGAs, such as:

```
enum class PlatformKind {
  kInvalid,
  kCuda,
  kOpenCL,
  kOpenCLAltera,  // Altera FPGA OpenCL platform.
                  // See documentation: go/fpgaopencl
                  // (StreamExecutor integration)
  kHost,
  kMock,
  kSize,
};
```

and

```
string PlatformKindString(PlatformKind kind) {
  switch (kind) {
    case PlatformKind::kCuda:
      return "CUDA";
    case PlatformKind::kOpenCL:
      return "OpenCL";
    case PlatformKind::kOpenCLAltera:
      return "OpenCL+Altera";
    case PlatformKind::kHost:
      return "Host";
    case PlatformKind::kMock:
      return "Mock";
    default:
      return port::StrCat("InvalidPlatformKind(", static_cast<int>(kind), ")");
  }
}
```

As of this writing, Google doesn't return any results for `+google +kOpenClAltera`, so it doesn't appear that this has been widely observed. If you're not familiar with Altera OpenCL and you work at google, you can try the internal go link suggested in the comment, `go/fpgaopencl`. If, like me, you don't work at Google, well, [there's Altera's docs here](https://www.altera.com/products/design-software/embedded-software-developers/opencl/overview.html). The basic idea is that you can take OpenCL code, the same kind of thing you might run on a GPU, and run it on an FPGA instead, and from the comment, it seems like Google has some kind of setup that lets you stream data in and out of nodes with FPGAs.

That FPGA-specific code was removed in ddd4aaf5286de24ba70402ee0ec8b836d3aed8c7, which has a commit message that starts with “TensorFlow: upstream changes to git.” and then has a list of internal google commits that are being upstreamed, along with a description of each internal commit. Curiously, there's nothing about removing FPGA support even though that seems like it's a major enough thing that you'd expect it to be described, unless it was purposely redacted. Amazon has also been quite secretive about their infrastructure plans, but you can make reasonable guesses there by looking at the hardware people they've been vacuuming up. A couple other companies are also betting pretty heavily on hardware accelerators, but since I learned about that through private conversations (as opposed to accidentally published public source code or other public information), I'll leave you to guess which companies.

#### Dark Silicon / SoCs

One funny side effect of the way transistor scaling has turned out is that we can pack a ton of transistors on a chip, but they generate so much heat that the average transistor can't switch most of the time if you don't want your chip to melt.

A result of this is that it makes more sense to include dedicated hardware that isn't used a lot of the time. For one thing, this means we get all sorts of specialized instructions like the [PCMP](http://www.felixcloutier.com/x86/PCMPESTRI.html) and [ADX](http://www.intel.com/content/dam/www/public/us/en/documents/white-papers/ia-large-integer-arithmetic-paper.pdf) instructions. But it also means that we're getting chips with entire devices integrated that would have previously lived off-chip. That includes things like GPUs and (for mobile devices) radios.

In combination with the hardware acceleration trend, it also means that it makes more sense for companies to design their own chips, or at least parts of their own chips. Apple has gotten a lot of mileage out of acquiring PA Semi. First, by adding little custom accelerators to bog standard ARM architectures, and then by adding custom accelerators to their own custom architecture. Due to a combination of the right custom hardware plus well thought out benchmarking and system design, the iPhone 4 is slightly more responsive than my flagship Android phone, which is multiple years newer and has a much faster processor as well as more RAM.

Amazon has picked up a decent chunk of the old Calxeda team and are hiring enough to create a good-sized hardware design team. Facebook has picked up a small handful of ARM SoC folks and is partnering with Qualcomm on something-or-other. [Linus is on record as saying we're going to see more dedicated hardware all over the place](http://www.realworldtech.com/forum/?threadid=146066&curpostid=146227). And so on and so forth.

### Conclusion

x86 chips have picked up a lot of new features and whiz-bang gadgets. For the most part, you don't have to know what they are to take advantage of them. As a first-order approximation, making your code predictable and keeping memory locality in mind works pretty well. The really low-level stuff is usually hidden by libraries or drivers, and compilers will try to take care of the rest of it. The exceptions are if you're writing really low-level code, in which case [the world has gotten a lot messier](https://github.com/vishmohan/vmlaunch/blob/master/vmlaunch_simple.c), or if you're trying to get the absolute best possible performance out of your code, in which case [the world has gotten a lot weirder](https://docs.google.com/document/d/18gs0bkEwQ5cO8pMXT_MsOa8Xey4NEavXq-OvtdUXKck/pub).

Also, things will happen in the future. But most predictions are wrong, so who knows?

### Resources

[This is a talk by Matt Godbolt](https://www.youtube.com/watch?v=hgcNM-6wr34) that covers a lot of the implementation details that I don't get into. To down into one more level of detail, see [Modern Processor Design, by Shen and Lipasti](http://www.amazon.com/gp/product/1478607831/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1478607831&linkCode=as2&tag=abroaview-20&linkId=HXBNFFJ3CIXMWUZP). Despite the date listed on Amazon (2013), the book is pretty old, but it's still the best book I've found on processor internals. It describes, in good detail, what you need to implement to make a P6-era high-performance CPU. It also derives theoretical performance limits given different sets of assumptions and talks about a lot of different engineering tradeoffs, with explanations of why for a lot of them.

For one level deeper of "why", you'll probably need to look at a VLSI text, which will explain how devices and interconnect scale and how that affects circuit design, which in turn affects architecture. I really like [Weste & Harris](http://www.amazon.com/gp/product/B008VIXPI2/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B008VIXPI2&linkCode=as2&tag=abroaview-20&linkId=YOJVAWVH5XTIF6LN) because they have clear explanations and good exercises with solutions that you can find online, but if you're not going to work the problems pretty much any VLSI text will do. For one more level deeper of the "why" of things, you'll want a solid state devices text and something that explains how transmission lines and interconnect can work. For devices, I really like [Pierret's](http://www.amazon.com/gp/product/0201543931/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201543931&linkCode=as2&tag=abroaview-20&linkId=MXV5K7IJXWXJD446) [books](http://www.amazon.com/gp/product/013061792X/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=013061792X&linkCode=as2&tag=abroaview-20&linkId=N7YLCANVDPI3M35R). I got introduced to the E-mag stuff through [Ramo, Whinnery & Van Duzer](http://www.amazon.com/gp/product/8126515252/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=8126515252&linkCode=as2&tag=abroaview-20&linkId=XZUQXDRDJVABGITB), but [Ida](http://www.amazon.com/gp/product/3319078054/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=3319078054&linkCode=as2&tag=abroaview-20&linkId=GTQSYCVTMIFZKSPZ) is a better intro text.

For specifics about current generation CPUs and specific optimization techniques, [see Agner Fog's site](http://www.agner.org/optimize/). For something on [optimization tools from the future, see this post](https://danluu.com/perf-tracing/). [What Every Programmer Should Know About Memory](http://www.akkadia.org/drepper/cpumemory.pdf) is also good background knowledge. Those docs cover a lot of important material, but if you're writing in a higher level language there [are a lot of other things you need to keep in mind](http://mrale.ph/blog/2015/01/11/whats-up-with-monomorphism.html). For more on Intel CPU history, Xao-Feng Li [has a nice overview](https://people.apache.org/~xli/presentations/history_Intel_CPU.pdf).

For something a bit off the wall, see [this post on the possibility of **CPU backdoors**](https://danluu.com/cpu-backdoors/). For something less off the wall, see [this post on how complexity we have in modern CPUs enables **all sorts of exciting bugs**](https://danluu.com/cpu-bugs/).

For more benchmarks on locking, See [this post by Aleksey Shipilev](http://shipilev.net/blog/2014/on-the-fence-with-dependencies/), [this post by Paul Khuong](http://www.pvk.ca/Blog/2015/01/13/lock-free-mutual-exclusion/), as well as their archives.

For general benchmarking, last year's [Strange Loop benchmarking talk by Aysylu Greenberg](https://www.youtube.com/watch?v=XmImGiVuJno) is a nice intro to common gotchas. For something more advanced but more specific, [Gil Tene's talk on latency](https://www.youtube.com/watch?v=9MKY4KypBzg) is great.

For historical computing that predates everything I've mentioned by quite some time, see [IBM's Early Computers](http://www.amazon.com/gp/product/0262523930/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0262523930&linkCode=as2&tag=abroaview-20&linkId=NB4TSSXVLME77X4B) and [Design of a Computer](http://ygdes.com/CDC/DesignOfAComputer_CDC6600.pdf), which describes the design of the CDC 6600. [Readings in Computer Architecture](http://www.amazon.com/gp/product/1558605398/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1558605398&linkCode=as2&tag=abroaview-20&linkId=MCCNUSAAP4B6NZAP) is also good for seeing where a lot of these ideas originally came from.

Sorry, this list is pretty incomplete. Suggestions welcome!

#### Tiny Disclaimer

I have no doubt that I'm leaving stuff out. [Let me know](https://twitter.com/danluu/) if I'm leaving out anything you think is important and I'll update this. I've tried to keep things as simple as possible while still capturing the flavor of what's going on, but I'm sure that there are some cases where I'm oversimplifying, and some things that I just completely forgot to mention. And of course basically every generalization I've made is wrong if you're being _really_ precise. Even just picking at my first couple sentences, A20M isn't always and everywhere irrelevant (I've probably spent about 0.2% of my career dealing with it), x86-64 isn't strictly superior to x86 (on one workload I had to deal with, the performance benefit from the extra registers was more than canceled out by the cost of the longer instructions; it's pretty rare that the instruction stream and icache misses are the long pole for a workload, but it happens), etc. The biggest offender is probably in my NUMA explanation, since it is actually possible for P6 busses to respond with a defer or retry to a request. It's reasonable to avoid using a similar mechanism to enforce coherency but I couldn't think of a reasonable explanation of why that didn't involve multiple levels of explanations. I'm really not kidding when I say that pretty much every generalization falls apart if you dig deep enough. Every abstraction I'm talking about is leaky. I've tried to include links to docs that go at least one level deeper, but I'm probably missing some areas.

#### Acknowledgments

Thanks to Leah Hanson and Nathan Kurz for comments that results in major edits, and to Nicholas Radcliffe, Stefan Kanthak, Garret Reid, Matt Godbolt, Nikos Patsiouras, Aleksey Shipilev, and Oscar R Moll for comments that resulted in minor edits, and to David Albert for allowing me to quote him and also for some interesting follow-up questions when we talked about this a while back. Also, thanks for Cliff Burdick for writing the section on GPUs and for Hari Angepat for spotting the Google kOpenCLAltera code in TensorFlow.

* * *