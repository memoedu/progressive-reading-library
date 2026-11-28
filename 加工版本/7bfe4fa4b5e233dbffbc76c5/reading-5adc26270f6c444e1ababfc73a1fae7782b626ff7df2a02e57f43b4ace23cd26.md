# Linux 提交记录的 TF-IDF｜TF-IDF linux commits

---

## 正文

我很好奇不同的人在 Linux 中做些什么，所以尝试从当前的 git repository 中 grabbing data，看看能否从 commit message data 中提取这些信息。这不包括他们 switched 到 git 之前的历史，所以只能追溯到 2005 年，但仍然是一段相当可观的历史。

下面列出了最频繁的前四位 committers 最常用的 words（在 commit messages 中），其中 users 按 commits 的数量 ordered。

| User | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- |
| viro | to | in | of | and | the |
| tiwai | alsa | the | \- | for | to |
| broonie | the | to | asoc | for | a |
| davem | the | to | in | and | sparc64 |

好吧，他们最频繁使用的 words 是 `to`、`alsa`、`the` 和 `the`。原来，Takashi Iwai (tiwai) 经常从事 audio (alsa) 方面的工作，而沿着这个 list 往下看，我们可以发现 David Miller (davem) 使用频率第五高的 term 是 `sparc64`，这很好地表明他做了大量 sparc 方面的工作。但这个 table 大多是 noise。人们当然总会使用 `to`、`in` 和其他 common words！把这些放进 table 并不能提供任何 information。

有不少 standard techniques 可以处理这个问题。其中一种是 explicitly filter out “stop words”，也就是我们不关心的 common words。遗憾的是，如果没有 manual intervention，这种方法在这个 dataset 上效果不佳。Standard stop-word lists 会漏掉 `Signed-off-by` 和 `cc` 之类没什么趣味的东西。我们可以针对 commit messages 中的 common words 使用某个 threshold，generate 一个 custom list of stop words，但任何高到足以捕获全部 noise 的 threshold，也会捕获 `null` 和 `driver` 这类常用但有趣的 terms。

幸运的是，手动完成这件事只需要大约一分钟。做完之后，结果是不同 committers 的许多 top words 都相同。我就不再列出按 committer 划分的 top words 的 table 了，因为它只是把许多相同的 words 重复了很多次。下面改为给出 removed 了 stop words 的 top words 的 table（按使用该 word 的 commit messages 数量 ranked，而不是按 raw count）。它包含相同的 data，却没有按 committer 分开所带来的额外 noise。

| Word | Count |
| --- | --- |
| driver | 49442 |
| support | 43540 |
| function | 43116 |
| device | 32915 |
| arm | 28548 |
| error | 28297 |
| kernel | 23132 |
| struct | 18667 |
| warning | 17053 |
| memory | 16753 |
| update | 16088 |
| bit | 15793 |
| usb | 14906 |
| bug | 14873 |
| register | 14547 |
| avoid | 14302 |
| pointer | 13440 |
| problem | 13201 |
| x86 | 12717 |
| address | 12095 |
| null | 11555 |
| cpu | 11545 |
| core | 11038 |
| user | 11038 |
| media | 10857 |
| build | 10830 |
| missing | 10508 |
| path | 10334 |
| hardware | 10316 |

好吧，看来大家在 `arm` 上做了大量工作，也做了很多与 `memory`、`null`、`pointer` 等有关的事情。但如果想看看每个人具体在做什么，我们就需要别的办法。

另一种办法可以是对更 common 的 words 进行 penalizing，而不 entirely eliminating 它们。用于 normalize 的一种 standard metric 是 inverse document frequency (IDF)，`log(# of messages / # of messages with word)`。因此，与其按 term count 或 term frequency 进行 ordering，不如尝试按 `(term frequency) * log(# of messages / # of messages with word)` 进行 ordering，后者通常称为 TF-IDF[1](https://danluu.com/linux-devs-say/#fn:H)。这样得到的 words，是某个人使用过、但其他人并不常用的 words。

下面列出了前 40 位 linux committers，以及根据 TF-IDF 得出的他们最常用的 words。

| User | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- |
| viro | switch | annotations | patch | of | endianness |
| tiwai | alsa | hda | codec | codecs | hda-codec |
| broonie | asoc | regmap | mfd | regulator | wm8994 |
| davem | sparc64 | sparc | we | kill | fix |
| gregkh | cc | staging | usb | remove | hank |
| mchehab | v4l/dvb | media | at | were | em28xx |
| tglx | x86 | genirq | irq | prepare | shared |
| hsweeten | comedi | staging | tidy | remove | subdevice |
| mingo | x86 | sched | zijlstra | melo | peter |
| joe | unnecessary | checkpatch | convert | pr\_ | use |
| tj | cgroup | doesnt | which | it | workqueue |
| lethal | sh | up | off | sh64 | kill |
| axel.lin | regulator | asoc | convert | thus | use |
| hch | xfs | sgi-pv | sgi-modid | remove | we |
| sachin.kamat | redundant | remove | simpler | null | of\_match\_ptr |
| bzolnier | ide | shtylyov | sergei | acked-by | caused |
| alan | tty | gma500 | we | up | et131x |
| ralf | mips | fix | build | ip27 | of |
| johannes.berg | mac80211 | iwlwifi | it | cfg80211 | iwlagn |
| trond.myklebust | nfs | nfsv4 | sunrpc | nfsv41 | ensure |
| shemminger | sky2 | net\_device\_ops | skge | convert | bridge |
| bunk | static | needlessly | global | patch | make |
| hartleys | comedi | staging | remove | subdevice | driver |
| jg1.han | simpler | device\_release | unnecessary | clears | thus |
| akpm | cc | warning | fix | function | patch |
| rmk+kernel | arm | acked-by | rather | tested-by | we |
| daniel.vetter | drm/i915 | reviewed-by | v2 | wilson | vetter |
| bskeggs | drm/nouveau | drm/nv50 | drm/nvd0/disp | on | chipsets |
| acme | galbraith | perf | weisbecker | eranian | stephane |
| khali | hwmon | i2c | driver | drivers | so |
| torvalds | linux | commit | just | revert | cc |
| chris | drm/i915 | we | gpu | bugzilla | whilst |
| neilb | md | array | so | that | we |
| lars | asoc | driver | iio | dapm | of |
| kaber | netfilter | conntrack | net\_sched | nf\_conntrack | fix |
| dhowells | keys | rather | key | that | uapi |
| heiko.carstens | s390 | since | call | of | fix |
| ebiederm | namespace | userns | hallyn | serge | sysctl |
| hverkuil | v4l/dvb | ivtv | media | v4l2 | convert |

这就像样多了。一些 common words 仍然会出现——如果用 manual stop words 来 remove `cc` 和 `of` 之类的东西，效果确实会更好。但总体而言，我们可以看出谁在做什么。Takashi Iwai (tiwai) 花了很多时间在 hda 领域以及 workig 于 codecs，David S. Miller (davem) 在 sparc64 上花了很多时间，Ralf Baechle (ralf) 做了大量与 mips 有关的工作，等等。不过话说回来，有些人——但不是所有人——如此频繁地对其他人进行 `cc`，以至于它即使被 `IDF` penalized，仍然出现在他们的 top 5 list 中，这或许也挺有意思。

我们还可以用它来看看，人们在 commit messages 中谈论的内容与他们 commit 的频率之间的 distribution。

![谁关心 null？这些人！](https://danluu.com/images/linux-devs-say/null-percentile.webp)

这个 graph 的 x-axis 表示人，y-axis 表示 relative word usage（按 TF-IDF ranked）。在 x-axis 上，最频繁的 committers 位于左侧，最不频繁的位于右侧。在 y-axis 上，如果某位 committer 更频繁地使用 word `null`，对应的 points 就更高；如果这个人使用 word `null` 的频率更低，对应的 points 就更低。

![谁关心 POSIX？几乎没人！](https://danluu.com/images/linux-devs-say/posix-percentile.webp)

相对而言，几乎没有人从事 `POSIX` compliance 方面的工作。你甚至可以逐个数出在 commit messages 中提到 `POSIX` 的人。

博客写到这里，你或许会期待某种总结，或者至少一个模糊的观点。抱歉，没这好事。我做这件事，只是因为 TF-IDF 是目前正在开设的 [Mining Massive Data Sets](https://www.coursera.org/course/mmds) 课程介绍的无数 concepts 之一，而我知道，如果不写些 code，就不可能真正记住它。

如果你非要一个结论，那就是 TF-IDF 有时很有用，而且 apply 起来极其容易。你应该在该用它的时候用它（当你想看看哪些 words 能 distinguish 不同的 documents/people 时），也不应该在不该用它的时候用它（当你想看看 documents/people 有什么共同之处时）。完。

我正在尝试减少每篇博客花费的时间，在一次 30-90 分钟的写作中一股脑地把东西写出来，从而多写一些。如果有什么不清楚或者明显错误的地方，请[告诉我](https://twitter.com/danluu)。我是认真的。这篇花的时间远远超出了计划，但主要是因为，啊，data、tables，还有 Julia 里的 bugs，而不是因为校对。我敢肯定还有 bugs！

感谢 Leah Hanson 找出这篇文章中的许多写作毛病，也感谢 Zack Maril 与我讨论未来或许可以如何 display 随时间发生的 change。

* * *

1.  我其实不明白，为什么这里的 standard 做法是 take the log。有时你想 take the log，是为了能处理更小的 numbers，或者为了能把一堆 multiplies convert 成一堆 adds，但这两种情况在这里都不成立。如果你觉得这显而易见，请[告诉我](https://twitter.com/danluu)。[\[返回\]](https://danluu.com/linux-devs-say/#fnref:H)

---

## 原文

I was curious what different people worked on in Linux, so I tried grabbing data from the current git repository to see if I could pull that out of commit message data. This doesn't include history from before they switched to git, so it only goes back to 2005, but that's still a decent chunk of history.

Here's a list of the most commonly used words (in commit messages), by the top four most frequent committers, with users ordered by number of commits.

| User | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- |
| viro | to | in | of | and | the |
| tiwai | alsa | the | \- | for | to |
| broonie | the | to | asoc | for | a |
| davem | the | to | in | and | sparc64 |

Alright, so their most frequently used words are `to`, `alsa`, `the`, and `the`. Turns out, Takashi Iwai (tiwai) often works on audio (alsa), and by going down the list we can see that David Miller's (davem) fifth most frequently used term is `sparc64`, which is a pretty good indicator that he does a lot of sparc work. But the table is mostly noise. Of course people use `to`, `in`, and other common words all the time! Putting that into a table provides zero information.

There are a number of standard techniques for dealing with this. One is to explicitly filter out "stop words", common words that we don't care about. Unfortunately, that doesn't work well with this dataset without manual intervention. Standard stop-word lists are going to miss things like `Signed-off-by` and `cc`, which are pretty uninteresting. We can generate a custom list of stop words using some threshold for common words in commit messages, but any threshold high enough to catch all of the noise is also going to catch commonly used but interesting terms like `null` and `driver`.

Luckily, it only takes about a minute to do by hand. After doing that, the result is that many of the top words are the same for different committers. I won't reproduce the table of top words by committer because it's just many of the same words repeated many times. Instead, here's the table of the top words (ranked by number of commit messages that use the word, not raw count), with stop words removed, which has the same data without the extra noise of being broken up by committer.

| Word | Count |
| --- | --- |
| driver | 49442 |
| support | 43540 |
| function | 43116 |
| device | 32915 |
| arm | 28548 |
| error | 28297 |
| kernel | 23132 |
| struct | 18667 |
| warning | 17053 |
| memory | 16753 |
| update | 16088 |
| bit | 15793 |
| usb | 14906 |
| bug | 14873 |
| register | 14547 |
| avoid | 14302 |
| pointer | 13440 |
| problem | 13201 |
| x86 | 12717 |
| address | 12095 |
| null | 11555 |
| cpu | 11545 |
| core | 11038 |
| user | 11038 |
| media | 10857 |
| build | 10830 |
| missing | 10508 |
| path | 10334 |
| hardware | 10316 |

Ok, so there's been a lot of work on `arm`, lots of stuff related to `memory`, `null`, `pointer`, etc. But if want to see what individuals work on, we'll need something else.

That something else could be penalizing more common words without eliminating them entirely. A standard metric to normalize by is the inverse document frequency (IDF), `log(# of messages / # of messages with word)`. So instead of ordering by term count or term frequency, let's try ordering by `(term frequency) * log(# of messages / # of messages with word)`, which is commonly called TF-IDF[1](https://danluu.com/linux-devs-say/#fn:H). This gives us words that one person used that aren't commonly used by other people.

Here's a list of the top 40 linux committers and their most commonly used words, according to TF-IDF.

| User | 1 | 2 | 3 | 4 | 5 |
| --- | --- | --- | --- | --- | --- |
| viro | switch | annotations | patch | of | endianness |
| tiwai | alsa | hda | codec | codecs | hda-codec |
| broonie | asoc | regmap | mfd | regulator | wm8994 |
| davem | sparc64 | sparc | we | kill | fix |
| gregkh | cc | staging | usb | remove | hank |
| mchehab | v4l/dvb | media | at | were | em28xx |
| tglx | x86 | genirq | irq | prepare | shared |
| hsweeten | comedi | staging | tidy | remove | subdevice |
| mingo | x86 | sched | zijlstra | melo | peter |
| joe | unnecessary | checkpatch | convert | pr\_ | use |
| tj | cgroup | doesnt | which | it | workqueue |
| lethal | sh | up | off | sh64 | kill |
| axel.lin | regulator | asoc | convert | thus | use |
| hch | xfs | sgi-pv | sgi-modid | remove | we |
| sachin.kamat | redundant | remove | simpler | null | of\_match\_ptr |
| bzolnier | ide | shtylyov | sergei | acked-by | caused |
| alan | tty | gma500 | we | up | et131x |
| ralf | mips | fix | build | ip27 | of |
| johannes.berg | mac80211 | iwlwifi | it | cfg80211 | iwlagn |
| trond.myklebust | nfs | nfsv4 | sunrpc | nfsv41 | ensure |
| shemminger | sky2 | net\_device\_ops | skge | convert | bridge |
| bunk | static | needlessly | global | patch | make |
| hartleys | comedi | staging | remove | subdevice | driver |
| jg1.han | simpler | device\_release | unnecessary | clears | thus |
| akpm | cc | warning | fix | function | patch |
| rmk+kernel | arm | acked-by | rather | tested-by | we |
| daniel.vetter | drm/i915 | reviewed-by | v2 | wilson | vetter |
| bskeggs | drm/nouveau | drm/nv50 | drm/nvd0/disp | on | chipsets |
| acme | galbraith | perf | weisbecker | eranian | stephane |
| khali | hwmon | i2c | driver | drivers | so |
| torvalds | linux | commit | just | revert | cc |
| chris | drm/i915 | we | gpu | bugzilla | whilst |
| neilb | md | array | so | that | we |
| lars | asoc | driver | iio | dapm | of |
| kaber | netfilter | conntrack | net\_sched | nf\_conntrack | fix |
| dhowells | keys | rather | key | that | uapi |
| heiko.carstens | s390 | since | call | of | fix |
| ebiederm | namespace | userns | hallyn | serge | sysctl |
| hverkuil | v4l/dvb | ivtv | media | v4l2 | convert |

That's more like it. Some common words still appear -- this would really be improved with manual stop words to remove things like `cc` and `of`. But for the most part, we can see who works on what. Takashi Iwai (tiwai) spends a lot of time in hda land and workig on codecs, David S. Miller (davem) has spent a lot of time on sparc64, Ralf Baechle (ralf) does a lot of work with mips, etc. And then again, maybe it's interesting that some, but not all, people `cc` other folks so much that it shows up in their top 5 list even after getting penalized by `IDF`.

We can also use this to see the distribution of what people talk about in their commit messages vs. how often they commit.

![Who cares about null? These people!](https://danluu.com/images/linux-devs-say/null-percentile.webp)

This graph has people on the x-axis and relative word usage (ranked by TF-IDF) y-axis. On the x-axis, the most frequent committers on the left and least frequent on the right. On the y-axis, points are higher up if that committer used the word `null` more frequently, and lower if the person used the word `null` less frequently.

![Who cares about POSIX? Almost nobody!](https://danluu.com/images/linux-devs-say/posix-percentile.webp)

Relatively, almost no one works on `POSIX` compliance. You can actually count the individual people who mentioned `POSIX` in commit messages.

This is the point of the blog post where you might expect some kind of summary, or at least a vague point. Sorry. No such luck. I just did this because TF-IDF is one of a zillion concepts presented in the [Mining Massive Data Sets](https://www.coursera.org/course/mmds) course running now, and I knew it wouldn't really stick unless I wrote some code.

If you really must have a conclusion, TF-IDF is sometimes useful and incredibly easy to apply. You should use it when you should use it (when you want to see what words distinguish different documents/people from each other) and you shouldn't use it when you shouldn't use it (when you want to see what's common to documents/people). The end.

I'm experimenting with blogging more by spending less time per post and just spewing stuff out in 30-90 minute sitting. Please [let me know](https://twitter.com/danluu) if something is unclear or just plain wrong. Seriously. I went way over time on this one, but that's mostly because argh data and tables and bugs in Julia, not because of proofreading. I'm sure there are bugs!

Thanks to Leah Hanson for finding a bunch of writing bugs in this post and to Zack Maril for a conversation on how to maybe display change over time in the future.

* * *

1.  I actually don't understand why it's standard to take the log here. Sometimes you want to take the log so you can work with smaller numbers, or so that you can convert a bunch of multiplies into a bunch of adds, but neither of those is true here. Please [let me know](https://twitter.com/danluu) if this is obvious to you. [\[return\]](https://danluu.com/linux-devs-say/#fnref:H)