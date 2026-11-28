# 分支预测｜Branch prediction

---

## 正文

_这是一份关于 branch prediction 的演讲的近似实录，该演讲于 8/22/2017 在 Two Sigma 举行，为 [RC](https://www.recurse.com/scout/click?t=b504af89e87b77920c9b60b2a1f6d5e8) 组织的演讲系列「localhost」拉开序幕。_

你们有多少人在自己的 code 中 use branches？如果你 use if statements 或 pattern matching，能否请你举手？

`Most of the audience raises their hands`

接下来的部分我不会请你们举手，但我猜，如果我问，你们有多少人觉得自己充分理解了 CPU 在 executes 一个 branch 时会做什么，以及这会带来什么 performance implications，又有多少人觉得自己能读懂一篇关于 branch prediction 的现代 paper，那么举手的人会更少。

这次演讲的目的是解释 CPUs 如何以及为什么进行「branch prediction」，然后充分介绍 classic branch prediction algorithms，让你能够阅读关于 branch prediction 的现代 paper，并基本明白其中在讲什么。

在讨论 branch prediction 之前，我们先谈谈为什么 CPUs 要进行 branch prediction。为此，我们需要先了解一点 CPUs 如何 work。

在这次演讲中，你可以把自己的 computer 看作一个 CPU 加上一些 memory。instructions 存放在 memory 中，而 CPU 会 executes 来自 memory 的一系列 instructions，其中 instructions 就是诸如「add two numbers」、「把一块 data 从 memory move 到 processor」这样的操作。通常，在 executing 一个 instruction 之后，CPU 会 execute 位于下一个 sequential address 的 instruction。不过，有一些称为「branches」的 instructions，让你能够 change 下一个 instruction 所来自的 address。

这里有一张 CPU 正在 executing 一些 instructions 的 abstract diagram。x-axis 是 time，而 y-axis 用来 distinguishes 不同的 instructions。

![按顺序执行的指令（Instructions executing sequentially）](https://danluu.com/images/branch-prediction/1-pipeline.webp)

这里，我们 execute instruction `A`，接着是 instruction `B`，然后是 instruction `C`，最后是 instruction `D`。

design 一个 CPU 的一种方式，是让 CPU 完成一个 instruction 的全部工作，然后转到下一个 instruction，完成下一个 instruction 的全部工作，以此类推。这并没有什么不妥；许多较早的 CPUs 就是这样做的，一些现代的 very low-cost CPUs 仍然如此。但如果你想制造一个 faster CPU，可以让 CPU 像 assembly line 那样工作。也就是说，把 CPU break 成两个部分，让一半 CPU 处理一个 instruction 的「前半部分」工作，同时另一半 CPU 处理一个 instruction 的「后半部分」工作，就像 assembly line 一样。这通常称为 pipelined CPU。

![执行过程相互重叠的指令（Instructions with overlapping execution）](https://danluu.com/images/branch-prediction/2-pipeline.webp)

如果这样做，execution 可能就会像上图那样。在 instruction A 的前半部分完成后，CPU 可以处理 instruction A 的后半部分，同时 instruction B 的前半部分 runs。而当 A 的后半部分完成时，CPU 可以同时开始处理 B 的后半部分和 C 的前半部分。在这张 diagram 中，你可以看到，pipelined CPU 在每个 unit time 内能够 execute 的 instructions 数量，是上面 unpipelined CPU 的两倍。

没有理由认为 CPU 只能被 broken up 成两个部分。我们可以把 CPU break 成三个部分，得到 3x speedup，或者分成四个部分，得到 4x speedup。这并不 strictly true；一般来说，three-stage pipeline 带来的 speedup 小于 3x，4-stage pipeline 带来的 speedup 小于 4x，因为把 CPU break 成更多部分并采用 deeper pipeline 会产生 overhead。

overhead 的一个来源，是如何 handled branches。对于一个 instruction，CPU 首先必须做的事情之一就是 get 这个 instruction；为此，它必须知道 instruction 在哪里。例如，考虑下面的 code：

```
if (x == 0) {
  // Do stuff
} else {
  // Do other stuff (things)
}
  // Whatever happens later
```

这可能会变成类似下面这样的 assembly

```
branch_if_not_equal x, 0, else_label
// Do stuff
goto end_label
else_label:
// Do things
end_label:
// whatever happens later
```

在这个 example 中，我们 compare `x` 与 0。`if_not_equal`，那么我们就 branch 到 `else_label`，并 execute else block 中的 code。如果这个 comparison fails（也就是说，如果 `x` 是 0），我们就 fall through，execute `if` block 中的 code，然后 jump 到 `end_label`，以避免 executing `else` block 中的 code。

对 pipelining 来说有问题的那一段 instructions sequence 是

```
branch_if_not_equal x, 0, else_label
???
```

CPU 不知道这将会是

```
branch_if_not_equal x, 0, else_label
// Do stuff
```

还是

```
branch_if_not_equal x, 0, else_label
// Do things
```

直到 branch 完成（或接近完成）executing 才能知道。由于 CPU 处理一个 instruction 时首先需要做的事情之一，是从 memory get 这个 instruction，而我们不知道 `???` 将会是哪一个 instruction，因此在前一个 instruction 接近完成之前，我们甚至无法开始处理 `???`。

之前我们说，3-stage pipeline 会带来 3x speedup，20-stage pipeline 会带来 20x speedup，这是假设你能够在每个 cycle 开始一个新的 instruction，但在这里，这两个 instructions 几乎是 serialized 的。

![因 branch stall 而不重叠的执行（non-overlapping execution）](https://danluu.com/images/branch-prediction/3-branch-stall.png)

绕过这个问题的一种方式是 use branch prediction。当一个 branch 出现时，CPU 会猜测这个 branch 是 taken 还是 not taken。

![对 branch result 进行推测（speculating）](https://danluu.com/images/branch-prediction/4-branch-speculation.webp)

在这里，CPU predicts 这个 branch 不会被 taken，并在 executing 这个 branch 的后半部分时，开始 executing `stuff` 的前半部分。如果 prediction 是 correct，CPU 就会 execute `stuff` 的后半部分，并且可以在 executing `stuff` 的后半部分时开始另一个 instruction，就像我们在第一张 pipeline diagram 中看到的那样。

![correct prediction 之后的重叠执行（overlapped execution）](https://danluu.com/images/branch-prediction/5-branch-correct-prediction.webp)

如果 prediction 是 wrong，那么当 branch 完成 executing 时，CPU 会丢弃来自 `stuff.1` 的 result，并开始 executing correct instructions，而不是 wrong instructions。由于如果没有 branch prediction，我们本来就会 stalled processor，而不会 executed 任何 instructions，因此我们的处境并不比没有作出 prediction 时更差（至少在我们目前考察的细节层次上如此）。

![已中止的预测（aborted prediction）](https://danluu.com/images/branch-prediction/6-branch-misprediction.webp)

这样做的 performance impact 是什么？为了作出 estimate，我们需要一个 performance model 和一个 workload。在这次演讲中，我们的 CPU 简化 model 将是一个 pipelined CPU，其中 non-branches 平均为 one instruction per clock，unpredicted 或 mispredicted branches 需要 20 cycles，而 correctly predicted branches 需要 one cycle。

如果我们考察「workstation」integer workloads 最常用的 benchmark——SPECint，其构成可能是 20% branches 和 80% 其他 operations。如果没有 branch prediction，我们预期一个「average」instruction 需要 `branch_pct * 1 + non_branch_pct * 20 = 0.2 * 20 + 0.8 * 1 = 4 + 0.8 = 4.8 cycles`。如果有 perfect、100% accurate 的 branch prediction，我们预期 average instruction 需要 0.8 \* 1 + 0.2 \* 1 = 1 cycle，也就是 4.8x speedup！另一种看法是，如果我们的 pipeline 有 20-cycle branch misprediction penalty，那么仅仅是 branches，就会让我们相对于 ideal pipelining speedup 产生接近 5x overhead。

让我们看看对此能做些什么。我们将从人们可能采用的最 naive 的做法开始，逐步探索更好的方法。

### Predict taken（预测为跳转）

我们可以不进行随机 predicting，而是看看所有 programs 的 execution 中的所有 branches。这样做的话，我们会发现 taken 和并非 not-taken 的 branches 并不完全 balanced -- taken branches 比 not-taken branches 多得多。其中一个原因是，loop branches 经常是 taken。

如果我们 predict 每个 branch 都是 taken，就可能得到 70% accuracy，这意味着我们要为 30% 的 branches 付出 misprediction cost，使一个 average instruction 的 cost 成为 `(0.8 + 0.7 * 0.2) * 1 + 0.3 * 0.2 * 20 = 0.94 + 1.2. = 2.14`。如果我们将始终 predicting taken 与不进行 prediction 以及 perfect prediction 进行 compare，就会发现，尽管始终 predicting taken 是一种非常 simple algorithm，它仍能获得 perfect prediction 的很大一部分收益。

![每条指令 2.14 个周期（2.14 cycles per instruction）](https://danluu.com/images/branch-prediction/7-always-taken-cpi.webp)

### Backwards taken forwards not taken (BTFNT)（向后跳转预测为跳转，向前跳转预测为不跳转）

将 branches 进行 Predicting 为 taken 对 loops 效果很好，但对所有 branches 而言就没那么好了。如果我们根据 branch 是 forward（skips over code）还是 backwards（回到之前的 code），考察 branches 是否为 taken，就会发现 backwards branches 比 forward branches 更经常是 taken，因此我们可以尝试一种 predictor，它 predicts backward branches 是 taken，而 forward branches 不是 taken (BTFNT)。如果我们在 hardware 中 implement 这种 scheme，compiler writers 就会和我们配合，arrange code，使 compiler 认为会是 taken 的 branches 成为 backwards branches，而 compiler 认为不会是 taken 的 branches 成为 forward branches。

这样做的话，我们可能得到大约 80% prediction accuracy，使我们的 cost function 成为 `(0.8 + 0.8 * 0.2) * 1 + 0.2 * 0.2 * 20 = 0.96 + 0.8 = 1.76` cycles per instruction。

![每条指令 1.76 个周期（1.76 cycles per instruction）](https://danluu.com/images/branch-prediction/8-btfnt-cpi.webp)

#### 被以下处理器采用

-   PPC 601(1993)：还 uses compiler generated branch hints
-   PPC 603

### One-bit（单比特）

到目前为止，我们看过的 schemes 都不 store 任何 state，也就是说，这些 schemes 中的 prediction ignores program 的 execution history。在文献中，它们被称为 _static_ branch prediction schemes。这些 schemes 的优点是 simple，但缺点是不擅长 predicting 那些 behavior 随时间 change 的 branches。如果你想看一个 behavior 随时间 changes 的 branch 的例子，可以想象如下这样的 code

```
if (flag) {
  // things
  }
```

在 program 运行期间，可能有一个 program 的 phase，其中 flag 是 set，branch 是 taken；而在 program 的另一个 phase 中，flag 并非 set，branch 也不是 taken。static scheme 无法为这样的 branch 做出良好的 predictions，因此让我们考虑 _dynamic_ branch prediction schemes，其中 prediction 可以根据 program history 而 change。

我们可以采取的最 simple 的办法之一，是根据 branch 上一次的 result 做出 prediction，也就是说，如果 branch 上一次是 taken，我们就 predict taken；如果 branch 上一次不是 taken，我们就 predict not taken。

由于为每个 possible branch 都保留一个 bit 会产生太多 bits，无法切实地 store，我们会保留一个 table，记录一定数量的、我们见过的 branches 及其上一次的 results。在这次演讲中，让我们将 `not taken` store 为 `0`，将 `taken` store 为 `1`。

![包含 1-bit entries 的 prediction table，通过 branch address 的 low bits 进行 indexed](https://danluu.com/images/branch-prediction/9-1bit.webp)

这里，只是为了让图能放得下，我们使用一个 64-entry table，这意味着可以用 6 bits 来 index into 这个 table，因此我们用 branch address 的 low 6 bits 来 index into 这个 table。在 execute 一个 branch 后，我们会 update prediction table 中的 entry（如下方高亮所示）；下次这个 branch 再次被 executed 时，我们会 index into 同一个 entry，并使用 updated value 来进行 prediction。

![进行 update 时，indexed entry 会 changes](https://danluu.com/images/branch-prediction/10-1bit-update.webp)

我们可能会观察到 aliasing，即两个不同 locations 中的两个 branches 会 map 到同一个 location。这并不理想，但 table 的 speed 和 cost 与 size 之间存在 tradeoff，实际上 limits 了 table 的 size。

如果使用 one-bit scheme，我们可能得到 85% accuracy，cost 为 `(0.8 + 0.85 * 0.2) * 1 + 0.15 * 0.2 * 20 = 0.97 + 0.6 = 1.57` cycles per instruction。

![每条指令 1.57 个周期（1.57 cycles per instruction）](https://danluu.com/images/branch-prediction/11-1bit-cpi.webp)

#### 被以下处理器采用

-   DEC EV4 (1992)
-   MIPS R8000 (1994)

### [Two-bit（双比特）](https://courses.cs.washington.edu/courses/cse590g/04sp/Smith-1981-A-Study-of-Branch-Prediction-Strategies.pdf)

one-bit scheme 对 `TTTTTTTT…` 或 `NNNNNNN…` 这样的 patterns 效果不错，但对于一个大多是 taken、却有一个 branch 不是 taken 的 branches 的 stream，就会出现 misprediction，`...TTTNTTT...` 这个问题可以通过为每个 address adding 第二个 bit，并 implementing 一个 saturating counter 来 fixed。让我们任意约定：branch 是 not taken 时就 count down，是 taken 时就 count up。如果看看 binary values，我们就会得到：

```
00: predict Not
01: predict Not
10: predict Taken
11: predict Taken
```

saturating counter 中的“saturating”意味着，如果从 `00` 开始 count down，我们不会 underflowing，而是保持在 `00`；类似地，从 `11` 开始 counting up 时，也会保持在 `11`。这种 scheme 与 one-bit scheme 是 identical，区别只在于 prediction table 中的每个 entry 是两个 bits，而不是一个 bit。

![与 1-bit 相同，只是 table 有 2 bits](https://danluu.com/images/branch-prediction/12-2bit.webp)

与 one-bit scheme 相比，在相同的 size/cost 下，two-bit scheme 的 entries 数量可以是其一半（如果我们只考虑 storage 的 cost，而忽略 saturating counter 的 logic 的 cost），但即便如此，对于大多数合理的 table sizes，two-bit scheme 仍能提供更好的 accuracy。

尽管 simple，这种方法的效果相当好，我们可以预期 two bit predictor 会有大约 90% accuracy，对应的 cost 是 1.38 cycles per instruction。

![每条指令 1.38 个周期（1.38 cycles per instruction）](https://danluu.com/images/branch-prediction/13-2bit-cpi.webp)

一个很自然的做法是将这种 scheme 进行 generalize，得到 n-bit saturating counter，但事实证明，adding 更多 bits 对 accuracy 的影响相对较小。我们还没有真正讨论 branch predictor 的 cost，但将每个 branch 的 2 bits 增加到 3 bits，会使 table size increases 到 1.5x，却收效甚微，因此在大多数情况下都不值得付出这样的 cost。two-bit scheme 无法很好地 predict 的最 simple、最 common 的情况，是 `NTNTNTNTNT...` 或 `NNTNNTNNT…` 这样的 patterns，但改用 n-bits 也无法让我们很好地 predict 这些 patterns！

#### 用于

-   LLNL S-1 (1977)
-   CDC Cyber? (80 年代初)
-   Burroughs B4900 (1982)：state 被 stored 在 instruction stream 中；hardware 会 over-write instruction 来 update branch state
-   Intel Pentium (1993)
-   PPC 604 (1994)
-   DEC EV45 (1993)
-   DEC EV5 (1995)
-   PA 8000 (1996)：实际上是带有 majority vote 的 3-bit shift register

### [Two-level adaptive, global（两级自适应，全局）](http://www.seas.upenn.edu/~cis501/papers/two-level-branch-pred.pdf) (1991)

如果我们考虑这样的 code

```
for (int i = 0; i < 3; ++i) {
  // code here.
  }
```

这段 code 会 produce 像 `TTTNTTTNTTTN...` 这样的 branches 的 pattern。

如果我们知道这个 branch 最近三次的 executions，就应该能够 predict 这个 branch 的下一次 execution：

```
TTT:N
TTN:T
TNT:T
NTT:T
```

我们之前考虑过的 schemes 使用 branch address 来 index into 一个 table，这个 table 根据最近的 history 告诉我们，这个 branch 更有可能是 taken 还是 not taken。这告诉我们这个 branch 是 biased towards 哪个 direction，却无法告诉我们自己正处于一个 repetitive pattern 中。为了解决这一点，我们将 store 最近的 branches 的 history，以及一个 predictions 的 table。

![使用 global branch history 和 branch address 来 index into prediction table](https://danluu.com/images/branch-prediction/14-global.webp)

在这个 example 中，我们将 branch history 的 4 bits 与 branch address 的 2 bits 进行 concatenate，来 index into prediction table。和之前一样，prediction 来自一个 2-bit saturating counter。我们不想只使用 branch history 来 index into 我们的 prediction table，因为如果那样做，任何两个具有相同 history 的 branches 都会 alias 到同一个 table entry。在实际的 predictor 中，我们很可能会有一个更大的 table，并使用 branch address 的更多 bits，但为了将 table 放进一张幻灯片中，我们的 index 只有 6 bits 长。

下面，我们将看看在 execute 一个 branch 时，哪些内容会被 updated。

![Update 会 changes index，因为 index 使用 branch history 中的 bits](https://danluu.com/images/branch-prediction/15-global-update.webp)

加粗的部分就是被 updated 的部分。在这张 diagram 中，我们将 branch history 的新 bits 从右向左 shift 进来，从而 updating branch history。由于 branch history 被 updated，prediction table 的 index 的 low bits 也被 updated，因此下次我们再次 take 同一个 branch 时，将使用 table 中不同的 entry 来作出 prediction；这与之前那些 index 由 branch address 来 fixed 的 schemes 不同。原先那个 entry 的 value 会被 updated，这样下次我们以相同的 branch history 再次 take 同一个 branch 时，就会有 updated prediction。

由于这个 scheme 中的 history 是 global 的，它会 correctly predict inner loops 中像 `NTNTNTNT…` 这样的 patterns，但对于 higher-level branches，可能并不总能正确地 make predictions，因为 history 是 global 的，会被来自其他 branches 的 information 所 contaminated。不过，这里的 tradeoff 是，保留一份 global history 比保留一个 local histories 的 table 更便宜。此外，使用 global history 能让我们 correctly predict correlated branches。例如，我们可能会有这样的情况：

```
if x > 0:
  x -= 1
if y > 0:
  y -= 1
if x * y > 0:
  foo()
```

如果第一个 branch 或接下来的 branch 中任何一个不是 taken，那么第三个 branch 就肯定不会是 taken。

采用这个 scheme，我们可能会获得 93% accuracy，也就是 1.27 cycles per instruction。

![每条指令 1.27 个周期（1.27 cycles per instruction）](https://danluu.com/images/branch-prediction/16-global-cpi.webp)

#### 用于

-   Pentium MMX (1996)：采用 4-bit global branch history

### [Two-level adaptive, local（两级自适应，局部）](http://www.cse.iitd.ac.in/~srsarangi/col_718_2017/papers/branchpred/alternative-impl.pdf) \[1992\]

如上所述，global history scheme 的一个问题是，那些本可以被 predicted cleanly 的 local branches，其 branch history 会被其他 branches 所 contaminated。

获得良好的 local predictions 的一种方法，是为不同的 branches 保留各自的 branch histories。

![保留一个 per-branch histories 的 table，而不是一份 global history](https://danluu.com/images/branch-prediction/17-local.webp)

我们不再保留单独一份 global history，而是保留一个 local histories 的 table，通过 branch address 来 index。这个 scheme 与我们刚才看到的 global scheme 是 identical 的，区别只是我们保留了多份 branch histories。可以这样理解：拥有 global history 是 local history 的一个 special case，其中我们 keep track of 的 histories 的 number 是 `1`。

采用这个 scheme，我们可能会获得大约 94% accuracy，对应的 cost 是 1.23 cycles per instruction。

![每条指令 1.23 个周期（1.23 cycles per instruction）](https://danluu.com/images/branch-prediction/18-local-cpi.webp)

#### 用于

-   Pentium Pro (1996)：[采用 4 bit local branch history，PC 的 low bits 被用于 index](http://www.ece.uah.edu/~milenka/docs/milenkovic_WDDD02.pdf)。请注意，这一点存在一些争议，Agner Fog 声称 PPro 及后续的 processors 使用的是 4-bit global history
-   Pentium II (1997)：与 PPro 相同
-   Pentium III (1999)：与 PPro 相同

### gshare（全局共享）

global two-level scheme 必须作出的一个 tradeoff 是，对于一个 fixed size 的 prediction table，bits 必须被分配给 branch history 或 branch address 中的一个。我们希望给 branch history 更多 bits，因为这既允许跨越更大“distance”的 correlations，也允许 tracking 更 complicated patterns；同时，我们也希望给 branch address 更多 bits，以避免 unrelated branches 之间的 interference。

我们可以尝试通过 hashing branch history 和 branch address，而不是 concatenating 它们，来兼得两者的好处。一种最简单且合理的做法，也是最早提出的 mechanism，就是将它们一起进行 [`xor`](https://en.wikipedia.org/wiki/XOR_gate)。这个将 bits 一起进行 `xor` 的 two-level adaptive scheme 被称为 `gshare`。

![对 branch address 和 branch history 进行 hash，而不是 appending](https://danluu.com/images/branch-prediction/19-gshare.webp)

采用这个 scheme，我们可能会看到大约 94% accuracy。这与我们刚才看到的 local scheme 所获得的 accuracy 相同，但 gshare 避免了必须保留一个庞大的 local histories 的 table；在只需 track 更少 state 的同时获得相同的 accuracy，是一项显著的改进。

#### 用于

-   MIPS R12000 (1998): [2K entries，11 bits 的 PC，8 bits 的 history](https://courses.cs.washington.edu/courses/csep548/06au/lectures/branchPred.pdf)
-   UltraSPARC-3 (2001): [16K entries，14 bits 的 PC，12 bits 的 history](https://courses.cs.washington.edu/courses/csep548/06au/lectures/branchPred.pdf)

### [agree（同意）](http://meseec.ce.rit.edu/eecc722-fall2006/papers/branch-prediction/5/agree_isca24.pdf) (1997)

branch mispredictions 的一个原因是不同 branches 之间的 interference，这些 branches 会 alias 到同一个 location。有许多方法可以 reduce 那些 alias 到同一个 predictor table entry 的 branches 之间的 interference。事实上，这次演讲之所以只讲到 90 年代发明的 schemes，是因为当时提出了各种各样的 interference-reducing schemes，数量太多，半小时内讲不完。

我们来看一种 scheme，它或许能让你了解 interference-reducing scheme 可以是什么样子，这就是“agree” predictor。当两个 branch-history pairs collide 时，它们的 predictions 要么 match，要么不 match。如果它们 match，我们就称之为 neutral interference；如果不 match，就称之为 negative interference。其思路是，大多数 branches 往往是 strongly biased 的（也就是说，如果我们在 predictor table 中使用 two-bit entries，我们预期，在没有 interference 的情况下，大多数 entries 在大多数时候会是 `00` 或 `11`，而不是 `01` 或 `10`）。对于 program 中的每个 branch，我们会 store 一个 bit，称之为“bias”。这个 predictions 的 table 不再 storing absolute branch predictions，而是 store prediction 是否 matches 或 does not match 这个 bias。

![predict 一个 branch 是否 agrees 它的 bias，而不是它是否 taken](https://danluu.com/images/branch-prediction/20-agree.webp)

看看它如何工作就会发现，这个 predictor 与 gshare predictor 是 identical 的，区别只是我们作出了上面提到的改动——prediction 是 agree/disagree，而不是 taken/not-taken，并且我们有一个由 branch address indexed 的 bias bit，为 agree 或 disagree 提供了参照。在最初的 paper 中，作者提议用首次看到的结果作为 bias，而其他人则提出使用 profile-guided optimization（基本上就是 running program，再把 data feeding 回 compiler）来 determine bias。

注意，当我们 execute 一个 branch，之后又回到同一个 branch 时，我们会使用同一个 bias bit，因为 bias 是由 branch address indexed 的；但我们会使用不同的 predictor table entry，因为它同时由 branch address 和 branch history indexed。

![updating 使用相同的 bias，但使用不同的 meta-prediction table entry](https://danluu.com/images/branch-prediction/21-agree-update.webp)

如果你觉得这种做法居然会有效很奇怪，我们来看一个具体例子。假设我们有两个 branches，其中 branch A 以 90% probability taken，branch B 以 10% probability taken。如果这两个 branches alias，并且我们假定每个 branch taken 的 probabilities 是 independent 的，那么它们 disagree 并 negatively interfere 的 probability 就是 `P(A taken) * P(B not taken) + P(A not taken) + P(B taken) = (0.9 * 0.9) + (0.1 * 0.1) = 0.82`。

如果使用 agree scheme，我们可以重新做上面的 calculation，但这时两个 branches disagree 并 negatively interfere 的 probability 是 `P(A agree) * P(B disagree) + P(A disagree) * P(B agree) = P(A taken) * P(B taken) + P(A not taken) * P(B taken) = (0.9 * 0.1) + (0.1 * 0.9) = 0.18`。另一种理解方式是，要出现 destructive interference，其中一个 branch 就必须 disagree 它的 bias。根据 definition，如果我们已 correctly determined 这个 bias，这种情况就不可能很容易发生。

使用这个 scheme，我们可能获得大约 95% accuracy，也就是 1.19 cycles per instruction。

![1.19 cycles per instruction（每条指令 1.19 个周期）](https://danluu.com/images/branch-prediction/22-agree-cpi.webp)

#### 用于

-   PA-RISC 8700 (2001)

### [Hybrid（混合式）](http://www.hpl.hp.com/techreports/Compaq-DEC/WRL-TN-36.pdf) (1993)

正如我们已经看到的，local predictors 能很好地 predict 某些种类的 branches（例如 inner loops），global predictors 也能很好地 predict 某些种类的 branches（例如某些 correlated branches）。一种尝试兼得两者优点的方法，是同时拥有这两种 predictors，再配备一个 meta predictor，由它 predicts 应该使用 local 还是 global predictor。一种简单的做法是，让 meta-predictor 使用与上面的 two-bit predictor 相同的 scheme，区别只是它不再 predicting `taken` 或 `not taken`，而是 predicts `local predictor` 或 `global predictor`

![predict 两个 predictors 中哪一个是 correct，而不是 predicting branch 是否 taken](https://danluu.com/images/branch-prediction/23-hybrid.webp)

正如可能的 interference-reducing schemes 有很多种，上面的 `agree` predictor 只是其中之一，可能的 hybrid schemes 也有很多种。我们可以使用任意两个 predictors，而不只是一个 local 和一个 global predictor，甚至可以使用两个以上的 predictors。

如果使用一个 local 和一个 global predictor，我们可能获得大约 96% accuracy，也就是 1.15 cycles per instruction。

![1.15 cycles per instruction（每条指令 1.15 个周期）](https://danluu.com/images/branch-prediction/24-hybrid-cpi.webp)

#### 用于

-   DEC EV6 (1998): local（1k entries，10 history bits，3 bit counter）与 global（4k entries，12 history bits，2 bit counter）predictors 的 combination
-   IBM POWER4 (2001): local（16k entries）与 gshare（16k entries，11 history bits，与 branch address 进行 xor，16k selector table）
-   IBM POWER5 (2004): bimodal（未涉及）与 two-level adaptive 的 combination
-   IBM POWER7 (2010)

### 未涉及的内容

这次演讲还有很多内容没有涉及！正如你可能预料到的，我们没有涉及的内容集合，远比涉及的内容大。我会简要介绍几项没有涉及的内容，并附上 references，这样，如果你有兴趣了解更多，就可以自行查阅。

我们没有讨论的一个重要问题是[如何 predict branch target](https://en.wikipedia.org/wiki/Branch_target_predictor)。注意，即使对于某些 unconditional branches（也就是不需要 directional prediction 的 branches，因为它们总是 taken），也需要做这件事，因为[（某些）unconditional branches 有 unknown branch targets](http://www.engr.uconn.edu/~zshi/course/cse5302/ref/bray91btb_tr.pdf)。

Branch target prediction 的 cost 足够高，以至于一些早期 CPUs 采用的 branch prediction policy 是“始终 predict not taken”，因为当你 predict branch 不会 taken 时，就不需要 branch target！始终 predicting not taken 的 accuracy 很差，但仍然比完全不作 prediction 更好。

我们没有讨论的 interference reducing predictors 包括 [bi-mode](https://people.eecs.berkeley.edu/~kubitron/courses/cs152-S04/handouts/papers/p4-lee.pdf)、[gskew](http://meseec.ce.rit.edu/eecc722-fall2002/papers/branch-prediction/7/michaud97trading.pdf) 和 [YAGS](http://web.eecs.umich.edu/~tnm/papers/yags.pdf)。非常简略地说，bi-mode 有点像 agree，因为它尝试根据 direction 将 branches seperate out，但 bi-mode 使用的 mechanism 是保留多个 predictor tables，并用第三个基于 branch address 的 predictor 来 predict，对于特定的 branch 与 branch history combination，应该使用哪个 predictor table。Bi-mode 似乎比 agree 更成功，因为它得到了更广泛的使用。使用 gskew 时，我们保留至少三个 predictor tables，并使用不同的 hash 来 index into 每个 table。其思路是，即使两个 branches alias，它们也只会在其中一个 table 中 alias，因此我们可以使用 vote，让另外两个 tables 的 result override 那个 aliasing table 可能不佳的 result。我不知道该如何非常简短地介绍 YAGS :-)。

由于我们没有讨论 speed（这里指 latency），有一种 prediction strategy 也没有讲到：使用一个 small/fast predictor，当一个 slower 且 more accurate 的 predictor computes 出它的 result 时，就可以由它 overridden 前者。

一些现代 CPUs 采用完全不同的 branch predictors；AMD Zen (2017) 和 AMD Bulldozer (2011) chips 似乎使用 [perceptron based branch predictors](https://www.cs.utexas.edu/~lin/papers/hpca01.pdf)。Perceptrons 是 single-layer neural nets。

[有人认为](https://hal.inria.fr/hal-01100647/document)，Intel Haswell (2013) 使用了 [TAGE predictor](http://www.irisa.fr/caps/people/seznec/JILP-COTTAGE.pdf) 的一种 variant。TAGE 代表 TAgged GEometric history length predictor。如果考察我们已经介绍过的 predictors，再查看 programs 的 actual executions，看看哪些 branches 没有被我们 predicting correctly，就会发现，其中一个主要类别是需要大量 history 的 branches——相当多的 branches 需要数十或数百 bits 的 history，有些甚至需要超过一千 bits 的 branch history。如果我们只有一个 predictor，甚至有一个 combines 了几个不同 predictors 的 hybrid predictor，保留一千 bits 的 history 反而会适得其反，因为这会让那些只需要相对少量 history 的 branches 的 predictions 变得更差（尤其是相对于 cost 而言），而这类 branches 占了大多数。TAGE predictor 的一个思路是，通过保留一个 history lengths 的 geometric series，让每个 branch 都能使用合适的 history。这就解释了 GE。TA 部分指的是 branches 是 tagged 的，这是 predictor 用来 track 哪些 branches 应当使用哪组 history 的一种 mechanism，我们在这里不作讨论。

现代 CPUs 往往有 specialized predictors，例如，在 generalized branch predictor 无法合理地 store 足够 history、为 loop 的每次 iteration 作出 perfect predictions 的情况下，loop predictor 可以 accurately predict loop branches。

我们完全没有讨论使用更多 space 与获得更好 predictions 之间的 tradeoff。改变 table 的 size，不仅会改变 predictor 的 performance，也会改变不同 predictors 彼此相比孰优孰劣。

我们也完全没有讨论不同 workloads 如何 affect 不同 branch predictors。Predictor performance 不仅随 table size 而 varies，也随具体 run 的 program 而 varies。

我们谈论 branch misprediction cost 时，也一直把它当作固定不变的东西，[但它并不是](http://users.elis.ugent.be/~leeckhou/papers/ispass06-eyerman.pdf)；而且，non-branch instructions 的 cost 在不同 workloads 之间也 varies 很大。

我尽量避免引入不能不言自明的 terminology，所以，如果你阅读 literature，看到的 terminology 会略有不同。

### 结论

我们已经看过多种 classic branch predictors，并非常简短地讨论了几个较新的 predictors。我们讨论过的一些 classic predictors 如今仍用于 CPUs；如果这场演讲有一个小时，而不是半小时，我们本可以讨论 state-of-the-art predictors。我觉得，很多人认为 CPUs 神秘而难以理解，但我认为 CPUs 实际上比 software 更容易理解。我可能有些偏见，因为我以前从事 CPUs 相关工作，但我认为，这并非源于我的偏见，而是某种根本性的特点。

如果你想一想 software 的 complexity，就会发现 complexity 的主要 limiting factor 是你的想象力。只要你能把某样东西想象得足够详细，以至于能够将它写下来，你就能把它做出来。当然，有些情况下，limiting factor 并不是这个，而是某种更实际的因素（例如 large scale applications 的 performance），但我认为，我们大多数人的大部分时间都在 writing software，而其中的 limiting factor 是 create 和 manage complexity 的能力。

Hardware 与此很不相同，因为存在一些抵制 complexity 的力量。你 implement 的每一块 hardware 都要花钱，因此你希望 implement 的 hardware 尽可能少。此外，performance 对大多数 hardware 都很重要（无论是 absolute performance，还是每 dollar、每 watt 或每份其他 cost 所获得的 performance），而 adding complexity 会使 hardware 变得 slower，从而 limits performance。如今，你花 \$300 就能买到一个 off-the-shelf CPU，并将它 overclocked 至 5 GHz。在 5 GHz 下，一个 unit of work 是 one nanosecond 的 one-fifth。作为参照，light 在 one nanosecond 内 travels 大约 one foot。另一个 limiting factor 是，如果 CPUs 不能始终 work perfectly，人们会相当不满。尽管 [CPUs 确实有 bugs](https://danluu.com/cpu-bugs/)，但 bugs 的 rate 比几乎所有 software 都低得多，也就是说，对它们进行 verified/tested 所遵循的 standard 要高得多。Adding complexity 会让事物更难 test 和 verify。由于 CPUs 所遵循的 correctness standard 比 [大多数 software](https://danluu.com/everything-is-broken/) 更高，adding complexity 会给 CPUs 带来高得多的 test/verification burden，因此即使忽略我们讨论过的其他因素，在 hardware 中 adding 类似数量的 complexity，也比在 software 中昂贵得多。

这些抵制 chip complexity 的因素带来的一个附带结果是：任何一个特定的“high-level” general purpose CPU feature，通常在概念上都足够 simple，可以在半小时或一小时的演讲中讲清楚。CPUs 比许多 programmers 想象的更 simple！顺便说一下，我说“high-level”，是为了排除诸如 transistors 和 circuit design 之类的内容，因为理解这些内容可能需要相当多的 low-level（physics 或 solid-state）背景知识。

### CPU internals 系列

-   [80s 以来新增的 CPU features](https://danluu.com/new-cpu-features/)
-   [real code 中 branches 和 integer overflow checking 的 cost](https://danluu.com/integer-overflow/)
-   [CPU bugs（CPU 缺陷）](https://danluu.com/cpu-bugs/)
-   [branch prediction 简史](https://danluu.com/branch-prediction/)
-   [为什么 CPU development 很难](https://danluu.com/hardware-unforgiving/)
-   [Verilog 很糟糕，第 1 部分](https://danluu.com/why-hardware-development-is-hard/)
-   [Verilog 很糟糕，第 2 部分](https://danluu.com/pl-troll/)

_感谢 Leah Hanson、Hari Angepat 和 Nick Bergson-Shilcock 审阅这场演讲的试讲版本，也感谢 Fred Clausen Jr 找出了本文中的一处笔误。本文写得有些草率，为此向大家致歉——我匆忙写下它，是为了让参加演讲的人能够在不久之后查阅这份“文字稿 ”并查找 references，但这也意味着，其中的错误可能比平常更多，组织安排也不如一篇正常的博客文章那么妥当。尤其是，演讲中通过一系列 animations 解释的内容，在这里并没有以同样的详细程度加以解释；而且在快速浏览本文时，我注意到，对于每个 predictor 不善于 handle 哪些 branches，解释得比较少，因此对每个 predictor 的 motivation 也交代得较少。我也许会尝试回头补充更多 motivation，但我不太可能彻底重组本文，并生成一套新的 graphics，以便在文字旁只有几幅 still graphics 的情况下更好地传达 concepts。感谢 Julien Vivenot、Ralph Corderoy、Vaibhav Sagar、Mindy Preston、Stefan Kanthak 和 Uri Shaked 找出了这篇仓促写成的文章中的笔误。_

---

## 原文

_This is a pseudo-transcript for a talk on branch prediction given at Two Sigma on 8/22/2017 to kick off "localhost", a talk series organized by [RC](https://www.recurse.com/scout/click?t=b504af89e87b77920c9b60b2a1f6d5e8)._

How many of you use branches in your code? Could you please raise your hand if you use if statements or pattern matching?

`Most of the audience raises their hands`

I won’t ask you to raise your hands for this next part, but my guess is that if I asked, how many of you feel like you have a good understanding of what your CPU does when it executes a branch and what the performance implications are, and how many of you feel like you could understand a modern paper on branch prediction, fewer people would raise their hands.

The purpose of this talk is to explain how and why CPUs do “branch prediction” and then explain enough about classic branch prediction algorithms that you could read a modern paper on branch prediction and basically know what’s going on.

Before we talk about branch prediction, let’s talk about why CPUs do branch prediction. To do that, we’ll need to know a bit about how CPUs work.

For the purposes of this talk, you can think of your computer as a CPU plus some memory. The instructions live in memory and the CPU executes a sequence of instructions from memory, where instructions are things like “add two numbers”, “move a chunk of data from memory to the processor”. Normally, after executing one instruction, the CPU will execute the instruction that’s at the next sequential address. However, there are instructions called “branches” that let you change the address next instruction comes from.

Here’s an abstract diagram of a CPU executing some instructions. The x-axis is time and the y-axis distinguishes different instructions.

![Instructions executing sequentially](https://danluu.com/images/branch-prediction/1-pipeline.webp)

Here, we execute instruction `A`, followed by instruction `B`, followed by instruction `C`, followed by instruction `D`.

One way you might design a CPU is to have the CPU do all of the work for one instruction, then move on to the next instruction, do all of the work for the next instruction, and so on. There’s nothing wrong with this; a lot of older CPUs did this, and some modern very low-cost CPUs still do this. But if you want to make a faster CPU, you might make a CPU that works like an assembly line. That is, you break the CPU up into two parts, so that half the CPU can do the “front half” of the work for an instruction while half the CPU works on the “back half” of the work for an instruction, like an assembly line. This is typically called a pipelined CPU.

![Instructions with overlapping execution](https://danluu.com/images/branch-prediction/2-pipeline.webp)

If you do this, the execution might look something like the above. After the first half of instruction A is complete, the CPU can work on the second half of instruction A while the first half of instruction B runs. And when the second half of A finishes, the CPU can start on both the second half of B and the first half of C. In this diagram, you can see that the pipelined CPU can execute twice as many instructions per unit time as the unpipelined CPU above.

There’s no reason that a CPU can only be broken up into two parts. We could break the CPU into three parts, and get a 3x speedup, or four parts and get a 4x speedup. This isn’t strictly true, and we generally get less than a 3x speedup for a three-stage pipeline or 4x speedup for a 4-stage pipeline because there’s overhead in breaking the CPU up into more parts and having a deeper pipeline.

One source of overhead is how branches are handled. One of the first things the CPU has to do for an instruction is to get the instruction; to do that, it has to know where the instruction is. For example, consider the following code:

```
if (x == 0) {
  // Do stuff
} else {
  // Do other stuff (things)
}
  // Whatever happens later
```

This might turn into assembly that looks something like

```
branch_if_not_equal x, 0, else_label
// Do stuff
goto end_label
else_label:
// Do things
end_label:
// whatever happens later
```

In this example, we compare `x` to 0. `if_not_equal`, then we branch to `else_label` and execute the code in the else block. If that comparison fails (i.e., if `x` is 0), we fall through, execute the code in the `if` block, and then jump to `end_label` in order to avoid executing the code in `else` block.

The particular sequence of instructions that’s problematic for pipelining is

```
branch_if_not_equal x, 0, else_label
???
```

The CPU doesn’t know if this is going to be

```
branch_if_not_equal x, 0, else_label
// Do stuff
```

or

```
branch_if_not_equal x, 0, else_label
// Do things
```

until the branch has finished (or nearly finished) executing. Since one of the first things the CPU needs to do for an instruction is to get the instruction from memory, and we don’t know which instruction `???` is going to be, we can’t even start on `???` until the previous instruction is nearly finished.

Earlier, when we said that we’d get a 3x speedup for a 3-stage pipeline or a 20x speedup for a 20-stage pipeline, that assumed that you could start a new instruction every cycle, but in this case the two instructions are nearly serialized.

![non-overlapping execution due to branch stall](https://danluu.com/images/branch-prediction/3-branch-stall.png)

One way around this problem is to use branch prediction. When a branch shows up, the CPU will guess if the branch was taken or not taken.

![speculating about a branch result](https://danluu.com/images/branch-prediction/4-branch-speculation.webp)

In this case, the CPU predicts that the branch won’t be taken and starts executing the first half of `stuff` while it’s executing the second half of the branch. If the prediction is correct, the CPU will execute the second half of `stuff` and can start another instruction while it’s executing the second half of `stuff`, like we saw in the first pipeline diagram.

![overlapped execution after a correct prediction](https://danluu.com/images/branch-prediction/5-branch-correct-prediction.webp)

If the prediction is wrong, when the branch finishes executing, the CPU will throw away the result from `stuff.1` and start executing the correct instructions instead of the wrong instructions. Since we would’ve stalled the processor and not executed any instructions if we didn’t have branch prediction, we’re no worse off than we would’ve been had we not made a prediction (at least at the level of detail we’re looking at).

![aborted prediction](https://danluu.com/images/branch-prediction/6-branch-misprediction.webp)

What’s the performance impact of doing this? To make an estimate, we’ll need a performance model and a workload. For the purposes of this talk, our cartoon model of a CPU will be a pipelined CPU where non-branches take an average of one instruction per clock, unpredicted or mispredicted branches take 20 cycles, and correctly predicted branches take one cycle.

If we look at the most commonly used benchmark of “workstation” integer workloads, SPECint, the composition is maybe 20% branches, and 80% other operations. Without branch prediction, we then expect the “average” instruction to take `branch_pct * 1 + non_branch_pct * 20 = 0.2 * 20 + 0.8 * 1 = 4 + 0.8 = 4.8 cycles`. With perfect, 100% accurate, branch prediction, we’d expect the average instruction to take 0.8 \* 1 + 0.2 \* 1 = 1 cycle, a 4.8x speedup! Another way to look at it is that if we have a pipeline with a 20-cycle branch misprediction penalty, we have nearly a 5x overhead from our ideal pipelining speedup just from branches alone.

Let’s see what we can do about this. We’ll start with the most naive things someone might do and work our way up to something better.

### Predict taken

Instead of predicting randomly, we could look at all branches in the execution of all programs. If we do this, we’ll see that taken and not not-taken branches aren’t exactly balanced -- there are substantially more taken branches than not-taken branches. One reason for this is that loop branches are often taken.

If we predict that every branch is taken, we might get 70% accuracy, which means we’ll pay the the misprediction cost for 30% of branches, making the cost of of an average instruction `(0.8 + 0.7 * 0.2) * 1 + 0.3 * 0.2 * 20 = 0.94 + 1.2. = 2.14`. If we compare always predicting taken to no prediction and perfect prediction, always predicting taken gets a large fraction of the benefit of perfect prediction despite being a very simple algorithm.

![2.14 cycles per instruction](https://danluu.com/images/branch-prediction/7-always-taken-cpi.webp)

### Backwards taken forwards not taken (BTFNT)

Predicting branches as taken works well for loops, but not so great for all branches. If we look at whether or not branches are taken based on whether or not the branch is forward (skips over code) or backwards (goes back to previous code), we can see that backwards branches are taken more often than forward branches, so we could try a predictor which predicts that backward branches are taken and forward branches aren’t taken (BTFNT). If we implement this scheme in hardware, compiler writers will conspire with us to arrange code such that branches the compiler thinks will be taken will be backwards branches and branches the compiler thinks won’t be taken will be forward branches.

If we do this, we might get something like 80% prediction accuracy, making our cost function `(0.8 + 0.8 * 0.2) * 1 + 0.2 * 0.2 * 20 = 0.96 + 0.8 = 1.76` cycles per instruction.

![1.76 cycles per instruction](https://danluu.com/images/branch-prediction/8-btfnt-cpi.webp)

#### Used by

-   PPC 601(1993): also uses compiler generated branch hints
-   PPC 603

### One-bit

So far, we’ve look at schemes that don’t store any state, i.e., schemes where the prediction ignores the program’s execution history. These are called _static_ branch prediction schemes in the literature. These schemes have the advantage of being simple but they have the disadvantage of being bad at predicting branches whose behavior change over time. If you want an example of a branch whose behavior changes over time, you might imagine some code like

```
if (flag) {
  // things
  }
```

Over the course of the program, we might have one phase of the program where the flag is set and the branch is taken and another phase of the program where flag isn’t set and the branch isn’t taken. There’s no way for a static scheme to make good predictions for a branch like that, so let’s consider _dynamic_ branch prediction schemes, where the prediction can change based on the program history.

One of the simplest things we might do is to make a prediction based on the last result of the branch, i.e., we predict taken if the branch was taken last time and we predict not taken if the branch wasn’t taken last time.

Since having one bit for every possible branch is too many bits to feasibly store, we’ll keep a table of some number of branches we’ve seen and their last results. For this talk, let’s store `not taken` as `0` and `taken` as `1`.

![prediction table with 1-bit entries indexed by low bits of branch address](https://danluu.com/images/branch-prediction/9-1bit.webp)

In this case, just to make things fit on a diagram, we have a 64-entry table, which mean that we can index into the table with 6 bits, so we index into the table with the low 6 bits of the branch address. After we execute a branch, we update the entry in the prediction table (highlighted below) and the next time the branch is executed again, we index into the same entry and use the updated value for the prediction.

![indexed entry changes on update](https://danluu.com/images/branch-prediction/10-1bit-update.webp)

It’s possible that we’ll observe aliasing and two branches in two different locations will map to the same location. This isn’t ideal, but there’s a tradeoff between table speed & cost vs. size that effectively limits the size of the table.

If we use a one-bit scheme, we might get 85% accuracy, a cost of `(0.8 + 0.85 * 0.2) * 1 + 0.15 * 0.2 * 20 = 0.97 + 0.6 = 1.57` cycles per instruction.

![1.57 cycles per instruction](https://danluu.com/images/branch-prediction/11-1bit-cpi.webp)

#### Used by

-   DEC EV4 (1992)
-   MIPS R8000 (1994)

### [Two-bit](https://courses.cs.washington.edu/courses/cse590g/04sp/Smith-1981-A-Study-of-Branch-Prediction-Strategies.pdf)

A one-bit scheme works fine for patterns like `TTTTTTTT…` or `NNNNNNN…` but will have a misprediction for a stream of branches that’s mostly taken but has one branch that’s not taken, `...TTTNTTT...` This can be fixed by adding second bit for each address and implementing a saturating counter. Let’s arbitrarily say that we count down when a branch is not taken and count up when it’s taken. If we look at the binary values, we’ll then end up with:

```
00: predict Not
01: predict Not
10: predict Taken
11: predict Taken
```

The “saturating” part of saturating counter means that if we count down from `00`, instead of underflowing, we stay at `00`, and similar for counting up from `11` staying at `11`. This scheme is identical to the one-bit scheme, except that each entry in the prediction table is two bits instead of one bit.

![same as 1-bit, except that the table has 2 bits](https://danluu.com/images/branch-prediction/12-2bit.webp)

Compared to a one-bit scheme, a two-bit scheme can have half as many entries at the same size/cost (if we only consider the cost of storage and ignore the cost of the logic for the saturating counter), but even so, for most reasonable table sizes a two-bit scheme provides better accuracy.

Despite being simple, this works quite well, and we might expect to see something like 90% accuracy for a two bit predictor, which gives us a cost of 1.38 cycles per instruction.

![1.38 cycles per instruction](https://danluu.com/images/branch-prediction/13-2bit-cpi.webp)

One natural thing to do would be to generalize the scheme to an n-bit saturating counter, but it turns out that adding more bits has a relatively small effect on accuracy. We haven’t really discussed the cost of the branch predictor, but going from 2 bits to 3 bits per branch increases the table size by 1.5x for little gain, which makes it not worth the cost in most cases. The simplest and most common things that we won’t predict well with a two-bit scheme are patterns like `NTNTNTNTNT...` or `NNTNNTNNT…`, but going to n-bits won’t let us predict those patterns well either!

#### Used by

-   LLNL S-1 (1977)
-   CDC Cyber? (early 80s)
-   Burroughs B4900 (1982): state stored in instruction stream; hardware would over-write instruction to update branch state
-   Intel Pentium (1993)
-   PPC 604 (1994)
-   DEC EV45 (1993)
-   DEC EV5 (1995)
-   PA 8000 (1996): actually a 3-bit shift register with majority vote

### [Two-level adaptive, global](http://www.seas.upenn.edu/~cis501/papers/two-level-branch-pred.pdf) (1991)

If we think about code like

```
for (int i = 0; i < 3; ++i) {
  // code here.
  }
```

That code will produce a pattern of branches like `TTTNTTTNTTTN...`.

If we know the last three executions of the branch, we should be able to predict the next execution of the branch:

```
TTT:N
TTN:T
TNT:T
NTT:T
```

The previous schemes we’ve considered use the branch address to index into a table that tells us if the branch is, according to recent history, more likely to be taken or not taken. That tells us which direction the branch is biased towards, but it can’t tell us that we’re in the middle of a repetitive pattern. To fix that, we’ll store the history of the most recent branches as well as a table of predictions.

![Use global branch history and branch address to index into prediction table](https://danluu.com/images/branch-prediction/14-global.webp)

In this example, we concatenate 4 bits of branch history together with 2 bits of branch address to index into the prediction table. As before, the prediction comes from a 2-bit saturating counter. We don’t want to only use the branch history to index into our prediction table since, if we did that, any two branches with the same history would alias to the same table entry. In a real predictor, we’d probably have a larger table and use more bits of branch address, but in order to fit the table on a slide, we have an index that’s only 6 bits long.

Below, we’ll see what gets updated when we execute a branch.

![Update changes index because index uses bits from branch history](https://danluu.com/images/branch-prediction/15-global-update.webp)

The bolded parts are the parts that were updated. In this diagram, we shift new bits of branch history in from right to left, updating the branch history. Because the branch history is updated, the low bits of the index into the prediction table are updated, so the next time we take the same branch again, we’ll use a different entry in the table to make the prediction, unlike in previous schemes where the index is fixed by the branch address. The old entry’s value is updated so that the next time we take the same branch again with the same branch history, we’ll have the updated prediction.

Since the history in this scheme is global, this will correctly predict patterns like `NTNTNTNT…` in inner loops, but may not always correct make predictions for higher-level branches because the history is global and will be contaminated with information from other branches. However, the tradeoff here is that keeping a global history is cheaper than keeping a table of local histories. Additionally, using a global history lets us correctly predict correlated branches. For example, we might have something like:

```
if x > 0:
  x -= 1
if y > 0:
  y -= 1
if x * y > 0:
  foo()
```

If either the first branch or the next branch isn’t taken, then the third branch definitely will not be taken.

With this scheme, we might get 93% accuracy, giving us 1.27 cycles per instruction.

![1.27 cycles per instruction](https://danluu.com/images/branch-prediction/16-global-cpi.webp)

#### Used by

-   Pentium MMX (1996): 4-bit global branch history

### [Two-level adaptive, local](http://www.cse.iitd.ac.in/~srsarangi/col_718_2017/papers/branchpred/alternative-impl.pdf) \[1992\]

As mentioned above, an issue with the global history scheme is that the branch history for local branches that could be predicted cleanly gets contaminated by other branches.

One way to get good local predictions is to keep separate branch histories for separate branches.

![keep a table of per-branch histories instead of a global history](https://danluu.com/images/branch-prediction/17-local.webp)

Instead of keeping a single global history, we keep a table of local histories, index by the branch address. This scheme is identical to the global scheme we just looked at, except that we keep multiple branch histories. One way to think about this is that having global history is a special case of local history, where the number of histories we keep track of is `1`.

With this scheme, we might get something like 94% accuracy, which gives us a cost of 1.23 cycles per instruction.

![1.23 cycles per instruction](https://danluu.com/images/branch-prediction/18-local-cpi.webp)

#### Used by

-   Pentium Pro (1996): [4 bit local branch history, low bits of PC used for index](http://www.ece.uah.edu/~milenka/docs/milenkovic_WDDD02.pdf). Note that is under some dispute and Agner Fog claims that the PPro and follow-on processors use 4-bit global history
-   Pentium II (1997): same as PPro
-   Pentium III (1999): same as PPro

### gshare

One tradeoff a global two-level scheme has to make is that, for a prediction table of a fixed size, bits must be dedicated to either the branch history or the branch address. We’d like to give more bits to the branch history because that allows correlations across greater “distance” as well as tracking more complicated patterns and we’d like to give more bits to the branch address to avoid interference between unrelated branches.

We can try to get the best of both worlds by hashing both the branch history and the branch address instead of concatenating them. One of the simplest reasonable things one might do, and the first proposed mechanism was to [`xor`](https://en.wikipedia.org/wiki/XOR_gate) them together. This two-level adaptive scheme, where we `xor`the bits together is called `gshare`.

![hash branch address and branch history instead of appending](https://danluu.com/images/branch-prediction/19-gshare.webp)

With this scheme, we might see something like 94% accuracy. That’s the accuracy we got from the local scheme we just looked at, but gshare avoids having to keep a large table of local histories; getting the same accuracy while having to track less state is a significant improvement.

#### Used by

-   MIPS R12000 (1998): [2K entries, 11 bits of PC, 8 bits of history](https://courses.cs.washington.edu/courses/csep548/06au/lectures/branchPred.pdf)
-   UltraSPARC-3 (2001): [16K entries, 14 bits of PC, 12 bits of history](https://courses.cs.washington.edu/courses/csep548/06au/lectures/branchPred.pdf)

### [agree](http://meseec.ce.rit.edu/eecc722-fall2006/papers/branch-prediction/5/agree_isca24.pdf) (1997)

One reason for branch mispredictions is interference between different branches that alias to the same location. There are many ways to reduce interference between branches that alias to the same predictor table entry. In fact, the reason this talk only runs into schemes invented in the 90s is because a wide variety of interference-reducing schemes were proposed and there are too many to cover in half an hour.

We’ll look at one scheme which might give you an idea of what an interference-reducing scheme could look like, the “agree” predictor. When two branch-history pairs collide, the predictions either match or they don’t. If they match, we’ll call that neutral interference and if they don’t, we’ll call that negative interference. The idea is that most branches tend to be strongly biased (that is, if we use two-bit entries in the predictor table, we expect that, without interference, most entries will be `00` or `11` most of the time, not `01` or `10`). For each branch in the program, we’ll store one bit, which we call the “bias”. The table of predictions will, instead of storing the absolute branch predictions, store whether or not the prediction matches or does not match the bias.

![predict whether or not a branch agrees with its bias as opposed to whether or not it's taken](https://danluu.com/images/branch-prediction/20-agree.webp)

If we look at how this works, the predictor is identical to a gshare predictor, except that we make the changes mentioned above -- the prediction is agree/disagree instead of taken/not-taken and we have a bias bit that’s indexed by the branch address, which gives us something to agree or disagree with. In the original paper, they propose using the first thing you see as the bias and other people have proposed using profile-guided optimization (basically running the program and feeding the data back to the compiler) to determine the bias.

Note that, when we execute a branch and then later come back around to the same branch, we’ll use the same bias bit because the bias is indexed by the branch address, but we’ll use a different predictor table entry because that’s indexed by both the branch address and the branch history.

![updating uses the same bias but a different meta-prediction table entry](https://danluu.com/images/branch-prediction/21-agree-update.webp)

If it seems weird that this would do anything, let’s look at a concrete example. Say we have two branches, branch A which is taken with 90% probability and branch B which is taken with 10% probability. If those two branches alias and we assume the probabilities that each branch is taken are independent, the probability that they disagree and negatively interfere is `P(A taken) * P(B not taken) + P(A not taken) + P(B taken) = (0.9 * 0.9) + (0.1 * 0.1) = 0.82`.

If we use the agree scheme, we can re-do the calculation above, but the probability that the two branches disagree and negatively interfere is `P(A agree) * P(B disagree) + P(A disagree) * P(B agree) = P(A taken) * P(B taken) + P(A not taken) * P(B taken) = (0.9 * 0.1) + (0.1 * 0.9) = 0.18`. Another way to look at it is, to have destructive interference, one of the branches must disagree with its bias. By definition, if we’ve correctly determined the bias, this cannot be likely to happen.

With this scheme, we might get something like 95% accuracy, giving us 1.19 cycles per instruction.

![1.19 cycles per instruction](https://danluu.com/images/branch-prediction/22-agree-cpi.webp)

#### Used by

-   PA-RISC 8700 (2001)

### [Hybrid](http://www.hpl.hp.com/techreports/Compaq-DEC/WRL-TN-36.pdf) (1993)

As we’ve seen, local predictors can predict some kinds of branches well (e.g., inner loops) and global predictors can predict some kinds of branches well (e.g., some correlated branches). One way to try to get the best of both worlds is to have both predictors, then have a meta predictor that predicts if the local or the global predictor should be used. A simple way to do this is to have the meta-predictor use the same scheme as the two-bit predictor above, except that instead of predicting `taken` or `not taken` it predicts `local predictor` or `global predictor`

![predict which of two predictors is correct instead of predicting if the branch is taken](https://danluu.com/images/branch-prediction/23-hybrid.webp)

Just as there are many possible interference-reducing schemes, of which the `agree` predictor, above is one, there are many possible hybrid schemes. We could use any two predictors, not just a local and global predictor, and we could even use more than two predictors.

If we use a local and global predictor, we might get something like 96% accuracy, giving us 1.15 cycles per instruction.

![1.15 cycles per instruction](https://danluu.com/images/branch-prediction/24-hybrid-cpi.webp)

#### Used by

-   DEC EV6 (1998): combination of local (1k entries, 10 history bits, 3 bit counter) & global (4k entries, 12 history bits, 2 bit counter) predictors
-   IBM POWER4 (2001): local (16k entries) & gshare (16k entries, 11 history bits, xor with branch address, 16k selector table)
-   IBM POWER5 (2004): combination of bimodal (not covered) and two-level adaptive
-   IBM POWER7 (2010)

### Not covered

There are a lot of things we didn’t cover in this talk! As you might expect, the set of material that we didn’t cover is much larger than what we did cover. I’ll briefly describe a few things we didn’t cover, with references, so you can look them up if you’re interested in learning more.

One major thing we didn’t talk about is [how to predict the branch target](https://en.wikipedia.org/wiki/Branch_target_predictor). Note that this needs to be done even for some unconditional branches (that is, branches that don’t need directional prediction because they’re always taken), since [(some) unconditional branches have unknown branch targets](http://www.engr.uconn.edu/~zshi/course/cse5302/ref/bray91btb_tr.pdf).

Branch target prediction is expensive enough that some early CPUs had a branch prediction policy of “always predict not taken” because a branch target isn’t necessary when you predict the branch won’t be taken! Always predicting not taken has poor accuracy, but it’s still better than making no prediction at all.

Among the interference reducing predictors we didn’t discuss are [bi-mode](https://people.eecs.berkeley.edu/~kubitron/courses/cs152-S04/handouts/papers/p4-lee.pdf), [gskew](http://meseec.ce.rit.edu/eecc722-fall2002/papers/branch-prediction/7/michaud97trading.pdf), and [YAGS](http://web.eecs.umich.edu/~tnm/papers/yags.pdf). Very briefly, bi-mode is somewhat like agree in that it tries to seperate out branches based on direction, but the mechanism used in bi-mode is that we keep multiple predictor tables and a third predictor based on the branch address is used to predict which predictor table gets use for the particular combination of branch and branch history. Bi-mode appears to be more successful than agree in that it's seen wider use. With gskew, we keep at least three predictor tables and use a different hash to index into each table. The idea is that, even if two branches alias, those two branches will only alias in one of the tables, so we can use a vote and the result from the other two tables will override the potentially bad result from the aliasing table. I don't know how to describe YAGS very briefly :-).

Because we didn't take about speed (as in latency), a prediction strategy we didn't talk about is to have a small/fast predictor that can be overridden by a slower and more accurate predictor when the slower predictor computes its result.

Some modern CPUs have completely different branch predictors; AMD Zen (2017) and AMD Bulldozer (2011) chips appear to use [perceptron based branch predictors](https://www.cs.utexas.edu/~lin/papers/hpca01.pdf). Perceptrons are single-layer neural nets.

[It’s been argued that](https://hal.inria.fr/hal-01100647/document) Intel Haswell (2013) uses a variant of a [TAGE predictor](http://www.irisa.fr/caps/people/seznec/JILP-COTTAGE.pdf). TAGE stands for TAgged GEometric history length predictor. If we look at the predictors we’ve covered and look at actual executions of programs to see which branches we’re not predicting correctly, one major class of branches are branches that need a lot of history -- a significant number of branches need tens or hundreds of bits of history and some even need more than a thousand bits of branch history. If we have a single predictor or even a hybrid predictor that combines a few different predictors, it’s counterproductive to keep a thousand bits of history because that will make predictions worse for the branches which need a relatively small amount of history (especially relative to the cost), which is most branches. One of the ideas in the TAGE predictor is that, by keeping a geometric series of history lengths, each branch can use the appropriate history. That explains the GE. The TA part is that branches are tagged, which is a mechanism we don’t discuss that the predictor uses to track which branches should use which set of history.

Modern CPUs often have specialized predictors, e.g., a loop predictor can accurately predict loop branches in cases where a generalized branch predictor couldn’t reasonably store enough history to make perfect predictions for every iteration of the loop.

We didn’t talk at all about the tradeoff between using up more space and getting better predictions. Not only does changing the size of the table change the performance of a predictor, it also changes which predictors are better relative to each other.

We also didn’t talk at all about how different workloads affect different branch predictors. Predictor performance varies not only based on table size but also based on which particular program is run.

We’ve also talked about branch misprediction cost as if it’s a fixed thing, [but it is not](http://users.elis.ugent.be/~leeckhou/papers/ispass06-eyerman.pdf), and for that matter, the cost of non-branch instructions also varies widely between different workloads.

I tried to avoid introducing non-self-explanatory terminology when possible, so if you read the literature, terminology will be somewhat different.

### Conclusion

We’ve looked at a variety of classic branch predictors and very briefly discussed a couple of newer predictors. Some of the classic predictors we discussed are still used in CPUs today, and if this were an hour long talk instead of a half-hour long talk, we could have discussed state-of-the-art predictors. I think that a lot of people have an idea that CPUs are mysterious and hard to understand, but I think that CPUs are actually easier to understand than software. I might be biased because I used to work on CPUs, but I think that this is not a result of my bias but something fundamental.

If you think about the complexity of software, the main limiting factor on complexity is your imagination. If you can imagine something in enough detail that you can write it down, you can make it. Of course there are cases where that’s not the limiting factor and there’s something more practical (e.g., the performance of large scale applications), but I think that most of us spend most of our time writing software where the limiting factor is the ability to create and manage complexity.

Hardware is quite different from this in that there are forces that push back against complexity. Every chunk of hardware you implement costs money, so you want to implement as little hardware as possible. Additionally, performance matters for most hardware (whether that’s absolute performance or performance per dollar or per watt or per other cost), and adding complexity makes hardware slower, which limits performance. Today, you can buy an off-the-shelf CPU for $300 which can be overclocked to 5 GHz. At 5 GHz, one unit of work is one-fifth of one nanosecond. For reference, light travels roughly one foot in one nanosecond. Another limiting factor is that people get pretty upset when CPUs don’t work perfectly all of the time. Although [CPUs do have bugs](https://danluu.com/cpu-bugs/), the rate of bugs is much lower than in almost all software, i.e., the standard to which they’re verified/tested is much higher. Adding complexity makes things harder to test and verify. Because CPUs are held to a higher correctness standard than [most software](https://danluu.com/everything-is-broken/), adding complexity creates a much higher test/verification burden on CPUs, which makes adding a similar amount of complexity much more expensive in hardware than in software, even ignoring the other factors we discussed.

A side effect of these factors that push back against chip complexity is that, for any particular “high-level” general purpose CPU feature, it is generally conceptually simple enough that it can be described in a half-hour or hour-long talk. CPUs are simpler than many programmers think! BTW, I say “high-level” to rule out things like how transistors and circuit design, which can require a fair amount of low-level (physics or solid-state) background to understand.

### CPU internals series

-   [New CPU features since the 80s](https://danluu.com/new-cpu-features/)
-   [The cost of branches and integer overflow checking in real code](https://danluu.com/integer-overflow/)
-   [CPU bugs](https://danluu.com/cpu-bugs/)
-   [A brief history of branch prediction](https://danluu.com/branch-prediction/)
-   [Why CPU development is hard](https://danluu.com/hardware-unforgiving/)
-   [Verilog sucks, part 1](https://danluu.com/why-hardware-development-is-hard/)
-   [Verilog sucks, part 2](https://danluu.com/pl-troll/)

_Thanks to Leah Hanson, Hari Angepat, and Nick Bergson-Shilcock for reviewing practice versions of the talk and to Fred Clausen Jr for finding a typo in this post. Apologies for the somewhat slapdash state of this post -- I wrote it quickly so that people who attended the talk could refer to the “transcript ” soon afterwards and look up references, but this means that there are probably more than the usual number of errors and that the organization isn’t as nice as it would be for a normal blog post. In particular, things that were explained using a series of animations in the talk are not explained in the same level of detail and on skimming this, I notice that there’s less explanation of what sorts of branches each predictor doesn’t handle well, and hence less motivation for each predictor. I may try to go back and add more motivation, but I’m unlikely to restructure the post completely and generate a new set of graphics that better convey concepts when there are a couple of still graphics next to text. Thanks to Julien Vivenot, Ralph Corderoy, Vaibhav Sagar, Mindy Preston, Stefan Kanthak, and Uri Shaked for catching typos in this hastily written post._