# 天哪 ben，别让我读你那篇 4500 字的砖头长文了，直接告诉我该怎么做

---

## 正文

我每天有很大一部分时间都在进行 video calls。Wave 是一家 distributed company，因此它们是我们沟通的主要方式。但与当面交谈相比，它们让人感觉不自然：

-   大多数人的 microphones 和 webcams 都是 low-quality，让他们的画面和声音效果都很差。
-   从你说话到对方听见之间存在 lag，使得交谈中的 [turn-taking](https://en.wikipedia.org/wiki/Turn-taking) 很难把握。
-   如果你使用 headphones，就无法很好地听见自己的声音。
-   由于 [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation)，别人说话时，你往往无法同时说话，这会让交谈不那么流畅。

我开始好奇，如果解决这些问题，video calls 的体验能好上多少。于是，我花了太多时间折腾 gear 和 software。这篇文章总结了我的收获。

总的来说，这些建议带来了相当大的改善：与 setups 同样好的朋友一对一交谈时，我已经可以连续聊 4+ 小时而不觉得疲惫。

_Epistemic status：目前最好的猜测；并非专业人士；几乎肯定有些地方是错的。请通过评论或 [email](https://www.benkuhn.net/hi/) 告诉我哪些地方有错！_

目录

-   [天哪 ben，别让我读你那篇 4500 字的砖头长文了，直接告诉我该怎么做](https://www.benkuhn.net/vc/#omg-ben-dont-make-me-read-your-4500-word-doorstopper-just-tell-me-what-to-do)
-   [Network（网络）](https://www.benkuhn.net/vc/#network)
-   [Audio（音频）](https://www.benkuhn.net/vc/#audio)
    -   [离其他人远一点](https://www.benkuhn.net/vc/#get-away-from-other-people)
    -   [获得没有 echo 的 full-duplex audio](https://www.benkuhn.net/vc/#get-full-duplex-audio-with-no-echo)
    -   [把你的 wireless headset 扔进垃圾桶](https://www.benkuhn.net/vc/#throw-your-wireless-headset-in-the-trash)
    -   [用 open-back headphones 清楚地听见自己](https://www.benkuhn.net/vc/#hear-yourself-clearly-with-open-back-headphones)
    -   [不要 mute](https://www.benkuhn.net/vc/#dont-mute)
    -   [换一个更好的 microphone](https://www.benkuhn.net/vc/#get-a-better-microphone)
    -   [听听自己的声音](https://www.benkuhn.net/vc/#listen-to-yourself)
-   [Video（视频）](https://www.benkuhn.net/vc/#video)
    -   [使用 dedicated monitor](https://www.benkuhn.net/vc/#use-a-dedicated-monitor)
    -   [改善你的 lighting](https://www.benkuhn.net/vc/#improve-your-lighting)
    -   [使用你的 real background](https://www.benkuhn.net/vc/#use-your-real-background)
    -   [别在 webcams 上费心](https://www.benkuhn.net/vc/#dont-bother-with-webcams)
    -   [使用你的 smartphone……](https://www.benkuhn.net/vc/#use-your-smartphone)
    -   [……或者一台 real camera](https://www.benkuhn.net/vc/#or-a-real-camera)
    -   [关于购买 camera 的一点说明](https://www.benkuhn.net/vc/#a-note-on-camera-buying)
-   [Conclusion（结语）](https://www.benkuhn.net/vc/#conclusion)

* * *

## 天哪 ben，别让我读你那篇 4500 字的砖头长文了，直接告诉我该怎么做

如果给过去的自己提建议，我会按以下顺序排列优先级。（当然，根据你的情况，你自己的排序可能会不同。）

1.  ($depends) 不要在你的噪声会打扰别人、或别人的噪声会打扰你的地方工作。

2.  ($10-30) 如果你遇到过 network issues，就在 computer 与 router 之间接上[一根 cable](https://amzn.to/3luTVdV)。你可能需要一个 [adapter](https://amzn.to/2YOFis9)。（人们普遍认为 bad connection 是 ISP 的错，但事实恰恰相反，它[更有可能是 flaky wifi 造成的](https://www.benkuhn.net/wireless/)。）

3.  (~\$100) 购买 [open-back headphones](https://amzn.to/3bULynH)，它们能让你正常听见自己的声音，而且戴起来非常舒服。

4.  (~\$30) 把你的 built-in computer mic 换成 [headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones)（再加上 [pop filter](https://amzn.to/3hOU5JU)），声音效果会好得多，也会 pick up 更少的 noise。注意，这需要一个带 detachable cable 的 headset，例如我在上面给出链接的那款。

    现在，你可以一直保持 unmuted 了！如果对方也有 headphones，你们还可以同时说话。这两点都会让你们的交谈更流畅。

5.  (\$0) 与大多数替代方案相比，优先选择 Zoom；它的 sound quality 更高，echo cancellation 更好，莫名其妙的行为也更少。如果你有 headphones 和一个不错的 mic，就 enable “original sound”，以 turn off 一些不必要的 audio filtering。

6.  (~\$200) 再添一台 monitor 用来记笔记，这样你就可以在 main monitor 上让 Zoom 保持 full-screen。如果你随时都能看一眼大家的脸，就更容易投入交谈。（我使用 iPad 配合 [Sidecar](https://support.apple.com/en-us/HT210380) 来做这件事；如果要找 dedicated device，合适的 search term 是 [“portable monitor”](https://amzn.to/3iZRUVz)。另外，如果你的会议经常涉及 presentations 或 screensharing，也可以考虑添置第三台 monitor。）

7.  (\$0?) 调整你的 lighting，让大量 diffuse light 照在脸上，并移开所有直接照向 camera 的 lights。Lighting 对 image quality 的影响，比你使用什么 hardware 更大！

8.  (~$20-80，如果你有一台不错的 camera) 把你的 camera 当作 webcam 使用。[Canon](https://www.usa.canon.com/internet/portal/us/home/support/self-help-center/eos-webcam-utility)、[Fujifilm](https://fujifilm-x.com/en-us/support/download/software/x-webcam/)、[Nikon](https://downloadcenter.nikonimglib.com/en/products/548/Webcam_Utility.html) 和 [Sony](https://support.d-imaging.sony.co.jp/app/webcam/en/) 的 cameras 都有相应的 software。（你会希望能把 camera 接到 power source 上，这意味着你可能需要一块“dummy battery；”费用就花在这里。）

9.  (~$40，如果你的 smartphone 有不错的 camera) 通过 [Camo](https://reincubate.com/camo/) 把它当作 webcam 使用。

10.  (~\$350) 如果你没有不错的 camera，但想买一台，可以购买二手的 entry-level mirrorless camera + lens + dummy battery + boom arm。参见[下文的购买建议](https://www.benkuhn.net/vc/#a-note-on-camera-buying)。

下面是更详细的建议及其理由。

## Network（网络）

Connection problems 是让 video calls 最糟糕的因素。它们会以三种不同的方式造成影响：

1.  如果你的 connection 变得非常糟糕，你的 audio 就会 break up，听起来让人疲惫不堪，也会破坏交谈的 flow。

2.  即使没有糟糕到那个程度，poor connection 也会 increase _latency_，也就是从你说话到对方听见你说话之间的时间。High latency 正是导致那种令人头疼的“你先说，不，_你_先说”来回推让的原因。

3.  最后（也是最不重要的一点），bad connection 会 limits 你能 exchange 的 data 的 amount，迫使你使用 lower-quality video。如果你用的是 webcam，这其实无关紧要，但读到本文结尾时，你可能已经有了一台足够好的 camera，让这一点变得重要。

我另写过一整篇文章，讲如何为 video calls troubleshoot 你的 home network，但实际上，大多数 connection problems 都是因为 [wifi 很糟糕](https://www.benkuhn.net/wireless/)，而不用 wifi 就能避开这些问题。所以，先试着在你的 computer 和 router 之间接一条 [Ethernet cable](https://amzn.to/3luTVdV)。如果你仍然发现 high latency 或 connection dropping，或者确实因为某种原因无法接 cable，就[查看指南](https://www.benkuhn.net/vcnet/)，了解更多 troubleshooting 建议。

## Audio（音频）

Video improvements 很亮眼，也很容易察觉，但 audio 才是你进行 call 的原因，因此归根结底更重要。所以先说 audio。

### 离其他人远一点

这是下面所有内容的基本前提。如果你打算一整天都不出声，共享办公空间和咖啡馆都不错，但你会对 noise levels 顾虑重重，无法自在交谈，因而不可能进行 natural-feeling meetings。如果你要进行超过 5 minutes 的 call，就找一个自己的空间。

（如果你执意要在拥挤、嘈杂的空间里开会，就忽略 audio 部分余下的内容吧。这种情况下，你基本上注定只能忍受糟糕的 calls，不过买一个[不错的 headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones)，再 installing [krisp.ai](https://krisp.ai/get-krisp-now/?ref=benkuhn)，也许能减轻一些损害。）

如果与你交谈的人处在 noisy environment 中，你似乎也可以自己用 krisp.ai 来 filter 他们的 audio，不过我还没有试过。

### 获得没有 echo 的 full-duplex audio

要让 voice conversations 感觉“natural”，一个关键因素是双方都需要能够同时说话并听见对方说话（“full-duplex audio”）。Full-duplex audio 很重要，因为它让你能够与对方 talk simultaneously（“overlap”）。

你可能认为 overlap 应该很少发生，因为打断别人是不礼貌的。虽然 large-scale overlaps 确实如此，但我们经常用 small-scale overlaps 来 [negotiate conversational turn-taking](https://en.wikipedia.org/wiki/Turn-taking#Overlap)（例如，在说话者的声音渐渐停下来、但还没说完时开始说话），或表示我们正在认真听（“嗯哼”“对”）。

full-duplex audio 的难题在于，如果对方正在说话，他们的 voice 会从你的 computer 的 speakers 中播放出来，再进入你的 microphone。在这种情况下，如果你的 computer 让 microphone 保持开启，最终就会向对方 playing back 他们自己 voice 的“echo”，这极其烦人。所以 video call 会尝试 filter out 从你的 speakers 进入 microphone 的 feedback，这就叫 [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation)。

不幸的是，要从你的 microphone stream 中 _only_ removing speaker echo，实在很难做到。因此，software 往往会转而在对方说话时 completely muting 你的 mic。如果你曾经尝试与别人 micro-overlap，并注意到他们的 audio 短暂 cut out，原因就在这里。

如果听你说话的人无法与你 overlap，你就更难判断他们是否跟得上，也更难 negotiate 该轮到谁说话。这会让 conversation 感觉不那么 natural，尤其是人数较多时。

要获得 full-duplex audio，你需要 (a) 有一套不会 produce echoes 的 audio setup，然后 (b) 说服你的 video call app 不要尝试 suppress echoes。

(a)，“一套不会 produce echoes 的 audio setup”，意味着你的 microphone 不应 pick up 任何来自 speakers 的 sound。实际来说，这意味着你的“speakers”必须是 headphones。

(b)，“说服你的 video call app 不要尝试 suppress echoes”，在我尝试研究时，显得出乎意料地棘手，因为每个 video call app 都有自己的一套 heuristics，用来决定何时 engage echo-cancellation。

于是，我亲自对 Zoom、Skype，以及 Chrome 和 Firefox 中的 Hangouts 做了 echo cancellation 的 tests。我在两台 computers 之间发起了一次 chat，两台都接了 headphones——这样的 setup 应该完全不需要 echo cancellation。接着，我对着其中一台 computer 的 microphone 播放 music。在另一台上，我对着 microphone 说话，并听 music 是否变小声。

Zoom、Skype 和 Firefox 中的 Hangouts 似乎都会在我说话时 slightly decrease audio volume，表明存在 light echo cancellation。至于 Chrome 中的 Hangouts，每次我一说话，audio 都会 _completely_ cut out。在 Zoom 中，我通过 selecting“use original audio”这个 option，成功 eliminate 了所有 echo cancellation；你也可以针对特定 audio devices permanently enable 它——我建议这样做。

### 把你的 wireless headset 扔进垃圾桶

我在这篇指南里推荐的 gear 全都是 wired，而不是 Bluetooth。虽然 Bluetooth 看起来应该很好，但实际使用中，它存在[audio latency、quality 和 reliability 方面的严重问题](https://www.benkuhn.net/wireless/)。另外，我认为 wireless open-back headphones（见下文）并不存在。

如果读完上一段后，你仍然觉得使用 wireless audio gear 也能凑合，那就读读链接里的文章吧 :)

### 用 open-back headphones 清楚地听见自己的声音

大多数 headphones 都是 _closed-back_，这意味着它们会在你的耳朵周围形成 acoustic seal，从而 attenuates outside sound。这对你听 music 时的“noise isolation”很有帮助。但在 calls 中，这反而不好，因为它也会 isolates 你自己的 voice，让你听见自己的声音时觉得 muffled 而且 unnatural。（任何会形成 seal 的 earbuds 也都会出现同样的问题，也就是除了 EarPods 或 non-Pro AirPods 之外，几乎所有产品都会如此。）

就我个人而言，没有听见自己声音所带来的 feedback，我在 calls 中也容易开始越说越大声，甚至喊起来。这会让我的嗓子疲惫，也可能让被我~吼叫~交谈的那个人感到压力。

为避免这种情况，你可以买 _open-back headphones_，它们在耳朵外侧用的是 mesh，而不是 closed covering。我买了 [Philips SHP9500](https://amzn.to/3bULynH)，非常喜欢；我没有 tested 过其他款。（我选了一款 low-end 的，因为在 video calls 中，sound quality 主要会受人们的 microphones 所 limited；如果你还想用同一副来听 music，可能会想要一款 higher-end 的。）

还有一个额外的好处：open-back headphones 舒服得_多_，因为它们没那么热。我事先并没意识到这会带来多大的不同，但能戴着 headphones 一整天而耳朵不过热，实在太棒了！

请注意，open-back headphones 会“leak”sound，因此你身边的人也会听见 conversation 另一端的声音。如果你有自己的空间，这不成问题，但它们不适合共用空间。你可能会想，sound 会不会 leak 进你自己的 microphone，从而 cause an echo；但我 tested 过，除非你把 volume 调到高得令人不舒服，否则它太小声，不会造成这种情况。

### 不要 mute

这并不是在谈 equipment 本身，但它会影响你的 equipment choices。引用 [Matt Mullenweg](https://ma.tt/2020/03/dont-mute-get-a-better-headset/) 的话，他是最早且规模最大的 fully-distributed companies 之一的创始人：

> 当你以 distributed 方式工作时，对于 audio 和 video calls，我有一个不太主流的建议：如果可以，就不要 mute。当你对着一个 muted room 说话时，那种感觉既诡异又不自然——即使你能看到其他人的脸，也会感到孤独。那些让交谈顺畅进行的自发反应全都消失了。如果你问某人一个问题，或者对方想插话，他们就必须先等着 unmute。我也不太喜欢“通过 unmute 来举手”的做法，因为这容易让会议变成大家只是在等着轮到自己说话，而不是真正倾听。

我非常赞同这一点，也希望与我交谈的人保持 unmuted，除非他们的 mic 很差，会 picks up 大量 noise。只要你……，这就不会成为你的问题。

### 换一个更好的 microphone

大多数 non-standalone computer microphones，包括那些高级 headsets 上的 microphones，听起来都差得让人耳朵受罪。（2020 MacBook Pro microphone 还可以。）换一个更好的，就能让同事听到的你的声音悦耳得多。例如，比较一下我用以下 mics 朗读 [Edward Lear](https://www.nonsenselit.org/Lear/BoN/bon010.html) 时的效果：

| Mic | Recording | Commentary |
| --- | --- | --- |

| 2014 iMac | 
 | 听起来像个铁皮罐，因为它就是 |
| Jabra Evolve 70 |

 | Wirecutter 推荐；听起来像得了重感冒 |
| 2020 MacBook Pro 13" |

 | “Studio quality”个鬼；不如说是“中度感冒” |
| [V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) |

 | 名字很蠢，$30，实际上听起来还行 |

最好的“不会出错”的 microphone 选择是表格中的最后一个：[V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones)（配上 [foam windscreen](https://amzn.to/3hOU5JU)），它可以代替 standard 3.5mm audio cable，连接到你的 headphones 上。✻✻ 这意味着 BoomPro 要求 headphones 配有 detachable cable。如果你舍不得换掉另一副 headphones，[Antlion ModMic](https://amzn.to/2Zypzhq) 是一个更贵的选择，可以用于任何 headphones，代价是多一根 cable。它也有 wireless version，不过考虑到 latency 和 quality，你不应该在 video calls 中使用 wireless audio equipment。它听起来比任何 headset 的 built-in mic 都 clearer 得多，也没那么 muffled。由于更靠近你的嘴，它还会比 _任何_ mic 都 pick up 更少的 background noise（例如 typing），因此也更容易遵循上面“不要 mute”的建议。不过，它听起来不会像 non-headset mic 那样 natural。

如果你想要听起来更加 realistic 的设备，最好的选择是类似 [AT2005](https://amzn.to/3kh95SM) 这样的 microphone，用一个 [boom arm](https://amzn.to/3bVaDP5) 将它 positioned 在距离你的脸不到 six inches 的位置，而这个 boom arm 是 [clamped](https://amzn.to/3kb57uP) 在桌子上的。如果你不希望 mic 出现在画面里，可能需要对 camera setup 做一些 zooming/cropping，才能把它放得那么近。将它与 BoomPro 比较一下（你需要 headphones 才能清楚地听出区别）：

| BoomPro | AT2005 |
| --- | --- |

| 
 | 

 |

我 tested 了几种不同的 microphones，也听了更多 microphones 的 recordings。我没有专门训练自己去 detect 细微的 quality differences，不过我的暂定 conclusions 如下：

1.  到嘴的 Distance 几乎 dominates 一切。对于我 tested 的 mics，6 inches 与 12 inches 造成的差别，和 switching mics 一样大。

    （这是因为，从 microphone 的角度来看，doubling the distance 会让你的声音变得 4x quieter，或者 equivalently，让 room noise 变得 4x louder。它还会让诸如你声音的 echoes 变得 louder——导致 microphone produce 一种“boomy”的 sound。）

    下面是两个听起来相当不同的 microphones，分别放在 6 inches 与 12 的距离时的效果：

| Model | 6 inches | 12 inches |
| --- | --- | --- |

| Blue Yeti | 
 |

 |
| AT2005USB |

 | 

 |

2.  Low-end condenser microphones 听起来比 dynamics 稍微“fuller”或更 natural，而后者听起来可能有些“muffled”。例如，上面的 Blue Yeti 是 condenser，而 AT2005USB 是 dynamic。

    （有时有人说，Dynamic mics 比 condensers 更善于“reject noise”，但我没找到任何提出这一说法的人解释其中的原因。†† 把 dynamic microphone 放在紧靠嘴的位置更容易做到，这在某种程度上类似于 rejecting noise，但如果你想把 mic 放在 video 的 frame 之外，这一点就不 relevant。Dynamic mics 的 high-frequency 和“transient” response 也更少，这可以让某些类型的 room noise 不那么显眼。）

3.  低于 \$50 的 Mics，以及 headset mics，即使靠近嘴，听起来也明显很差。除此之外，sound quality 似乎同样甚至更多地取决于 microphone quality 以外的因素，比如你的空间有多少 echoes。（有一大类 YouTubers 用听起来很差的 setups 来 demo 昂贵的 mics，数量多得令人惊讶。）所以，在大多数 video call setups 中，higher-end microphones 很可能都是浪费。

基于这些，我建议选择 AT2005：它是一款被广泛推荐的 mic，属于最低的“听起来不会明显很差”的 price tier，而且不必费什么周折就能装在 boom arm 上。

其他可供进一步阅读的 microphone comparisons：

-   [Wirecutter 的 USB mic comparisons](https://soundcloud.com/wirecutter-audio/sets/2019-usb-microphone-testing)

-   [Marco Arment 谈 podcasting microphones](https://marco.org/podcasting-microphones)

-   [Matt Mullenweg：“不要 mute，换一个更好的 headset”](https://ma.tt/2020/03/dont-mute-get-a-better-headset/)

-   [rtings.com 的 headset recording quality tests](https://www.rtings.com/headphones/tests/microphone/recording-quality)

### 听听自己的声音

与现实生活不同，在 video calls 中，你听到的声音与交谈对象听到的并不相同。而且 mics 很难调好，因此你具体的 setup 和 mic technique 都非常重要。所以，听听自己的 audio 是什么效果，确实很有用。你可以用 [miccheck.me](https://www.miccheck.me/) 这样的 web app 来做这件事。最常见的 mic 问题是 [plosive pops](https://en.wikipedia.org/wiki/Pop_filter) 和 [harsh sibilants](https://en.wikipedia.org/wiki/De-essing)——挑选能涵盖这些情况的句子。[Harvard Sentences](https://www.cs.columbia.edu/~hgs/audio/harvard.html) 是一个不错的起点。

如果这些 consonants 听起来很差，你可能需要更好的 windscreen，或者改变 mic 的 positioned 方式。例如，如果你用的是 headset mic，就应该把它 position 在嘴角旁边——_而不是_正前方——这样就不会对着它呼气或喷口水。

## Video（视频）

### 使用专用的 dedicated monitor

我最近开始把正在进行的 call 以 full-screen mode 放在我的 primary (27") monitor 上，并使用第二个 monitor 记笔记或做其他事情。结果，这让 call 给人的 immersive 感受有了出乎意料的大幅提升。（也许，这个 feature 被称为“immersive mode”就该让我有所领悟了？）它在让我保持专注和投入方面效果惊人。可能的原因有：

-   在 windowed mode 下，Zoom 会把你的 preview 放在 screen 顶部一个单独的 bar 里，但在 fullscreen mode 下没有这个 bar，只有一个 floating preview window（你也可以 hide 它）。这意味着有更多的 screen 可以用于显示其他人的 video。

-   通过 Hiding 这些 window bezel、task bar 等，你通过 computer 交谈这件事就不再那么显眼，user interface 也更不容易分散你的注意力。

-   在 windowed mode 下，我有时会 tabbing away 去看另一个 window，然后懒得 tab back，就这么忘了。这样一来，call 中就会有很长时间我没有看着其他人，也觉得彼此不那么亲近。

至于用来记笔记的第二个 monitor，我使用的是搭配 [Sidecar](https://support.apple.com/en-us/HT210380) 的 iPad，不过你也可以买到价格低于 \$200 的专用“portable monitors”，同样能很好地满足这个用途。

### 改善你的 lighting

要让任何 camera 得到 sharper image，最好的办法都是让更多 light 进入 sensor。Laptop webcams 的 image quality 很糟糕，但具有良好 lighting 的 laptop webcam，效果会比 lighting 糟糕的高档 camera 更好：

[![](https://www.benkuhn.net/vc/imac%20cam%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_133549_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_252223_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")

左：被 MAXIMUM BACKLIGHT 毁掉效果的 2014 iMac webcam。中：高档的 Sony A6000 也应付不来。右：借助一个 [lumenator](https://www.benkuhn.net/lux/)，将光 bounce 到我面前的墙上后，iMac webcam 获得了极佳的 lighting。点击查看 fullsize。

lighting 的两条基本规则是：

1.  将大量 diffuse light Cast 到脸上，确保脸比 background 更 brighter。（另外，照到 scene 上的 light 越多，你的 image 看起来就越不 grainy。）

    最简单的办法是把桌子放在窗前；其次是让 artificial lights 在 camera 后面的 light-colored surface 上 bounce。如果这两种办法都不够，你还可以使用“ring light”或“softbox”（我没有具体推荐，因为我从未用过）。

2.  将 camera 的 field of view 内的 light sources Eliminate。与 human eye 相比，cameras 的 _dynamic range_ 更 lower——它们无法 capture brightness 的巨大差异。例如，这就是为什么在晴朗的白天，你的手机无法把以天空为背景的树木拍好。同样，如果你身后有一扇明亮的窗户，或者 camera 的 field of view 内有 light fixture，那部分看起来就会“blown out”，相比之下，其他一切都会显得 dark。

### 使用你的 real background

这可能会引起争议。这里我严格只从 immersiveness 的角度讨论——而不是例如展现你的个性、逗同事开心，或藏起身后那堆待洗衣物。这些都是想使用 backgrounds 的正当理由！只是要意识到，这样做会牺牲 immersiveness。

为什么？Zoom 的 background detection software 并不十分 accurate，它会不时 delete 你的部分头发或身体，让 background 从你的眼球中透出来，等等。而且，它非常不擅长 detecting boundaries，所以部分 real background 会从你的头发中透出来。

如果你有一台不错的 camera（如下文所述），以及自己的独立空间（见上文的[远离其他人](https://www.benkuhn.net/vc/#get-away-from-other-people)），那么不使用 fake background 会让你看起来更不容易分散别人的注意力，也更真实。

### 别在 webcams 上费心

laptop webcams 很糟糕，这大概显而易见。即使它们没有[莫名其妙地被 positioned 得朝着你的鼻孔拍](https://www.reddit.com/r/Dell/comments/5pkpa7/who_thought_this_is_the_perfect_place_to_put_a/)，它们的画面也会 grainy、blurry，而且 dynamic range 极小。

也许不那么明显的是，external webcams 也没有好多少。这让我很惊讶，因为像 [Logitech Brio](https://www.logitech.com/en-us/product/brio) 这样的 high-end webcam，价格达到一台二手 interchangeable-lens camera 的 2/3，而且它_只需要做好一件事_。‡‡ 说清楚一点，Brio 的效果[要好得多](https://randsinrepose.com/archives/good-meetings-are-jazz/)，胜过大多数其他 webcams——只是仍然远逊于真正的 camera。例如，我买了一台 Logitech C920，这是 Wirecutter 选出的“best webcam”，但它并没有明显好过我那用了六年的 iMac webcam，主要是因为它的 exposure / color balance settings _实在_令人质疑。§§ C920 确实允许你 tune 这些 settings，但我花了大约 2 小时才弄明白怎么做，最后还不得不买了一个糟糕的 third-party app。手动 tuning 这些 settings 还意味着，一天中每当 lighting 变化时，你都得相应更改，实在很烦。

[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")

左：2014 iMac camera；右：Logitech C920。我的肤色是挺白，但也没白到那个地步。

### 使用你的 smartphone…

（我没有大量使用 smartphone 作为 webcam，因为我直接就用上了完整的 camera，所以这些看法还不太确定。随着了解加深，我会更新。）

我原以为 webcams 会比 smartphones 更好，否则它们就没什么存在的理由。但事实证明我错了：webcams 实际上确实没什么存在的理由。Smartphones 在 sensor size 和 quality of materials 上都胜过它们。例如，iPhone 11 camera 的 [costs 是 \$73.50 in materials](https://www.cultofmac.com/658650/iphone-11-pro-max-production-cost-bill-materials-bom/), and has a [1/2.55" sensor](https://photographylife.com/reviews/iphone-11-pro-camera), while the Logitech C920 _retails_ for $80，而且有一个 [1/3" sensor](https://www.techtravels.org/modifying-logitech-c920/)。‖‖ 当然，如果你用的是 lower-end smartphone，C920 的 materials 可能比你的 smartphone 更好。但看起来，在 tradeoff-space 中，只有很小一片 region，让“买一台 webcam”相比“买一部更好的 smartphone”或“买一台 entry-level camera”而言是个好主意。所以，如果你想要 external webcam，就使用 smartphone。

[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")

左：Logitech C920；右：通过 Reincubate Camo 使用的 iPhone XR（crop）。

我简短试用了两款把 smartphone 用作 webcam 的 apps：[Camo](https://reincubate.com/camo/) 和 [EpocCam](https://www.kinoni.com/)。EpocCam 更便宜（\$8 vs $40），但似乎比 Camo 更 buggier 一些（两者偶尔都会出问题）。它们都有 free trials，会给你的 video 加 watermark，因此适合 testing，却不适合实际的 calls。

mount 一部 smartphone 最简单的办法，似乎是使用[这样的 gooseneck holder](https://amzn.to/3hvwyOf)。我原以为应该能找到一个小得多的 device，把手机 attached 到我的 monitor 上，但我能找到最好的也只是[这个夹子一样的东西](https://amzn.to/2RkLO5E)。它会遮住 laptops 的部分 screen，adjust 时的 increments 不够小，无法得到合适的 field of view，而且无法用于像我的 iMac 那样背面弯曲的 external monitors。如果你有更好的建议，请告诉我。

### …或者一台 real camera

即使是 lowest-end 的“real” cameras，在 image quality 上也会远胜大多数 smartphones。你会得到 sharper 得多的 image，配上赏心悦目的 blurred background，巧妙地把注意力引向你的脸，而不是那一堆堆脏衣服。

[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_289081_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20good.jpg "full size")

左：通过 Camo 使用的 iPhone XR；右：Sony A6000。这组对比在 full resolution 下看更有意义；Sony 的 dynamic range 稍差一些，但 image 要 sharper **得多**。

这大概是列表里最显眼的 improvement：我在 [!!con](http://bangbangcon.com/speakers.html#ben-kuhn) 上做 virtual conference talk 时开始使用 camera，得到了 ~20 位不同同事和 conference speakers 的称赞。（这么显眼未必是件好事，但我想，同事们大多早已知道我极其虚荣，也就没必要再试图掩饰了。）

遗憾的是，这些 cameras 也……相当 user-unfriendly，set up 起来可能要费些功夫。如果你决定 replicate 我的 setup（Sony A6000 + Elgato CamLink 4k），下面有一些不那么显而易见的 tips：

-   买一支 non-power-zoom lens，否则每次把 camera 关掉再打开，你的 zoom setting 都会被 reset。

-   如果你有 Sony A6000，务必把顶部的 dial 转到“video” mode。否则，HDMI output 的 quality 会更低，continuous focus 也不会正常 work。

-   使用尽可能 widest 的 aperture，以 minimize graininess，并获得好看的 blurry background effect。

### 关于 camera buying 的一点说明

如今这么多不同 manufacturers 都有了 webcam drivers，我不确定哪个 camera model 最好。在这些 webcam drivers 推出之前，Sony A6000 是用于 streaming 的 camera 的默认推荐，而且作为 entry-level mirrorless camera，它也普遍受到推荐；但看起来它根本无法与 Sony 的 webcam drivers 配合 work（它不在 supported models 的列表中，不过 A6100 及之后的型号在其中）。总之，你得自己做些研究。

最好从信誉良好的 used camera dealer（例如 [Keh](https://www.keh.com/)、[B&H](https://www.bhphotovideo.com/)、[Lens Authority](https://www.lensauthority.com/)）那里购买，因为 Amazon 等平台上随便找来的 resellers，可能不会认真检查 camera 是否正常 work。

这里列出我买的东西供参考，不过我并不特别推荐这套：

-   Sony A6000（相机）
-   16-50mm ƒ/3.5–5.6 PZ OSS lens（如上所述，我更推荐 non-PZ 18–55mm ƒ/3.5-5.6 OSS）
-   Elgato Camlink 4k（可能缺货；不知名 vendors 有更便宜的选择）
-   HDMI to Micro HDMI cable（我花了出乎意料的长时间，才弄清 A6000 用的是什么 type 的 HDMI port！）
-   随便买的杂牌 USB dummy battery（有时会出现“battery depleted” errors，所以这或许不是最好的主意）

## Conclusion（结语）

这番折腾给我最大的教训是，和 [wireless](https://www.benkuhn.net/wireless/) 很相似，video calls 到现在基本上还是不能好好 work。每件 equipment 都有棘手的微妙之处，让选对 gear 并正确使用它变得极其困难。Software 会做出蠢得不可思议的事情，除非你知道该留意什么，否则永远不会察觉。你从来听不到自己的 audio，所以即使它很糟糕，你也永远不会知道。而许多你确实注意到的问题，几乎不可能 debug，因为没有好的 diagnostics。

如果有一台 \$2000 的 device，能让这篇文章不再有存在的必要，我的雇主肯定会毫不犹豫地购买，大概其他每一家 distributed company 也都会如此。但并没有，所以我们只能这样了。

_感谢 [Dan Luu](https://danluu.com/)、[Sasha Illarionov](https://sdll.io/)、David Coletta、[Alexey Guzey](https://guzey.com/)、[Lincoln Quirk](https://www.lincolnquirk.com/)、[Eve Bigaj](https://evebigaj.com/) 和 Jessica Gambirasi 对本文草稿提出意见。_

---

## 原文

I spend a lot of my day on video calls. Wave is a distributed company, so they’re the main way we communicate. But compared to talking in person, they feel unnatural:

-   Most people have low-quality microphones and webcams that make them look and sound bad.
-   There’s a lag between when you say something and when the other person hears it, making it hard to navigate conversational [turn-taking](https://en.wikipedia.org/wiki/Turn-taking).
-   If you’re using headphones, you can’t hear your own voice very well.
-   Because of [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation), you often can’t talk when someone else is also talking, which makes the conversation flow less well.

I started wondering how much nicer video calls would feel if I fixed these problems. So I spent way too much time fiddling with gear and software. This post summarizes what I’ve learned.

Collectively, these recommendations have had a pretty big impact: when talking one-on-one to friends with equally good setups, I’ve been able to go 4+ hours without feeling fatigued.

_Epistemic status: best guess; not a professional; almost certainly contains wrong bits. Tell me which ones by comment or [email](https://www.benkuhn.net/hi/)!_

Contents

-   [omg ben don’t make me read your 4500 word doorstopper, just tell me what to do](https://www.benkuhn.net/vc/#omg-ben-dont-make-me-read-your-4500-word-doorstopper-just-tell-me-what-to-do)
-   [Network](https://www.benkuhn.net/vc/#network)
-   [Audio](https://www.benkuhn.net/vc/#audio)
    -   [Get away from other people](https://www.benkuhn.net/vc/#get-away-from-other-people)
    -   [Get full-duplex audio with no echo](https://www.benkuhn.net/vc/#get-full-duplex-audio-with-no-echo)
    -   [Throw your wireless headset in the trash](https://www.benkuhn.net/vc/#throw-your-wireless-headset-in-the-trash)
    -   [Hear yourself clearly with open-back headphones](https://www.benkuhn.net/vc/#hear-yourself-clearly-with-open-back-headphones)
    -   [Don’t mute](https://www.benkuhn.net/vc/#dont-mute)
    -   [Get a better microphone](https://www.benkuhn.net/vc/#get-a-better-microphone)
    -   [Listen to yourself](https://www.benkuhn.net/vc/#listen-to-yourself)
-   [Video](https://www.benkuhn.net/vc/#video)
    -   [Use a dedicated monitor](https://www.benkuhn.net/vc/#use-a-dedicated-monitor)
    -   [Improve your lighting](https://www.benkuhn.net/vc/#improve-your-lighting)
    -   [Use your real background](https://www.benkuhn.net/vc/#use-your-real-background)
    -   [Don’t bother with webcams](https://www.benkuhn.net/vc/#dont-bother-with-webcams)
    -   [Use your smartphone…](https://www.benkuhn.net/vc/#use-your-smartphone)
    -   […or a real camera](https://www.benkuhn.net/vc/#or-a-real-camera)
    -   [A note on camera buying](https://www.benkuhn.net/vc/#a-note-on-camera-buying)
-   [Conclusion](https://www.benkuhn.net/vc/#conclusion)

* * *

## omg ben don’t make me read your 4500 word doorstopper, just tell me what to do

Here’s how I would stack-rank my advice for my past self. (Of course, your personal ranking might be different depending on your situation.)

1.  ($depends) Don’t work in a space where your noise can bother other people, or vice versa.

2.  ($10-30) If you ever have network issues, run [a cable](https://amzn.to/3luTVdV) between your computer and router. You’ll probably need an [adapter](https://amzn.to/2YOFis9). (Contrary to popular belief that a bad connection is your ISP’s fault, it’s [more likely to be flaky wifi](https://www.benkuhn.net/wireless/).)

3.  (~$100) Buy [open-back headphones](https://amzn.to/3bULynH), which let you hear your own voice normally and are extremely comfortable.

4.  (~$30) Switch from your built-in computer mic to a [headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) (and [pop filter](https://amzn.to/3hOU5JU)), which will sound much better and pick up less noise. Note this requires a headset with detachable cable, like the one I linked above.

    You can now leave yourself unmuted! If the other person also has headphones, you can also talk at the same time. Both of these will make your conversations flow better.

5.  ($0) Prefer Zoom to most alternatives; it has higher sound quality, better echo cancellation, and fewer silly behaviors. If you have headphones and a good mic, enable “original sound” to turn off some unnecessary audio filtering.

6.  (~$200) Get a second monitor for notes so that you can keep Zoom full-screen on your main monitor. It’s easier to stay present if you can always glance at people’s faces. (I use an iPad with [Sidecar](https://support.apple.com/en-us/HT210380) for this; for a dedicated device, the right search term is [“portable monitor”](https://amzn.to/3iZRUVz). Also, if your meetings frequently involve presentations or screensharing, consider getting a third monitor too.)

7.  ($0?) Arrange your lighting to cast lots of diffuse light on your face, and move away any lights that shine directly into your camera. Lighting makes a bigger difference to image quality than what hardware you use!

8.  (~$20-80 if you have a nice camera) Use your camera as a webcam. There’s software for [Canon](https://www.usa.canon.com/internet/portal/us/home/support/self-help-center/eos-webcam-utility), [Fujifilm](https://fujifilm-x.com/en-us/support/download/software/x-webcam/), [Nikon](https://downloadcenter.nikonimglib.com/en/products/548/Webcam_Utility.html), and [Sony](https://support.d-imaging.sony.co.jp/app/webcam/en/) cameras. (You will want to be able to plug your camera into a power source, which means you’ll probably need a “dummy battery;” that’s what the cost is.)

9.  (~$40 if you have a smartphone with a good camera) Use that as a webcam via [Camo](https://reincubate.com/camo/).

10.  (~$350) If you don’t own a nice camera but want one, you can get a used entry-level mirrorless camera + lens + dummy battery + boom arm. See [buying tips below](https://www.benkuhn.net/vc/#a-note-on-camera-buying).

More detailed recommendations and justifications follow.

## Network

Connection problems are the thing that makes video calls suck the most. They do this in three different ways:

1.  If your connection ever gets really bad, your audio will break up, which is exhausting to listen to and ruins the flow.

2.  Even if it doesn’t get that bad, a poor connection will increase _latency_, or the time between when you speak and when the other person hears you. High latency is what causes the dreaded “you first, no _you_ first” dance.

3.  Finally (and least importantly), a bad connection limits the amount of data you can exchange, forcing you to use lower-quality video. This doesn’t really matter if you’re using a webcam, but by the end of this post, you might have a good enough camera that it matters.

I wrote a whole post of its own on how to troubleshoot your home network for video calls, but realistically, most connection problems are because [wifi sucks](https://www.benkuhn.net/wireless/) and you can avoid them by not using wifi. So, first try running an [Ethernet cable](https://amzn.to/3luTVdV) between your computer and your router. If you still notice high latency or your connection dropping, or if you really can’t run a cable for some reason, [check the guide](https://www.benkuhn.net/vcnet/) for more troubleshooting advice.

## Audio

Video improvements are flashy and noticeable, but audio is the reason you’re having the call, thus ultimately more important. So audio comes first.

### Get away from other people

This is a basic prerequisite for everything below. Coworking spaces and cafés are nice if you plan to be silent all day, but will make natural-feeling meetings impossible due to your crippling self-consciousness about noise levels. If you’re going to be on a call for more than 5 minutes, get your own space.

(If you are committed to taking your meetings in a crowded and noisy space, ignore the rest of the audio section. You’re mostly just doomed to crappy calls in this case, though you might be able to limit the damage by getting [a nice headset mic](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) and installing [krisp.ai](https://krisp.ai/get-krisp-now/?ref=benkuhn).)

If you’re talking to someone else who’s in a noisy environment, you can apparently also use krisp.ai to filter their audio yourself, though I haven’t tried this.

### Get full-duplex audio with no echo

One key ingredient to making voice conversations feel “natural” is that both participants need be able to talk and hear the other person talking at the same time (“full-duplex audio”). Full-duplex audio is important because it allows you to talk simultaneously (“overlap”) with the other person.

You might think that overlap should be rare, because interrupting someone else is rude. While that’s true of large-scale overlaps, we often use small-scale overlaps to [negotiate conversational turn-taking](https://en.wikipedia.org/wiki/Turn-taking#Overlap) (e.g. starting talking when the speaker is trailing off but hasn’t finished), or to signify that we’re paying attention (“uh-huh,” “yeah”).

The hard problem of full-duplex audio is that if someone else is talking, their voice is going to come out of your computer’s speakers and go back into your microphone. If your computer leaves the microphone on, in that case, it’ll end up playing back an “echo” of their own voice to them, which is extremely annoying. So video call tries to filter out feedback from your speakers into your microphone, which is called [echo cancellation](https://en.wikipedia.org/wiki/Echo_suppression_and_cancellation).

Unfortunately, removing _only_ the speaker echo from your microphone stream is really hard to do. So instead, the software often ends up completely muting your mic if someone else is talking. If you’ve ever tried to micro-overlap with someone and noticed that their audio cut out briefly, that’s what’s going on.

If your listeners can’t overlap with you, it’s harder to tell whether they’re following along, and it’s harder to negotiate whose turn it is to speak. This makes the conversation feel less natural, especially in larger groups.

To get full-duplex audio, you need to (a) have an audio setup that doesn’t produce echoes, then (b) convince your video call app not to try to suppress echoes.

(a), “an audio setup that doesn’t produce echoes,” means that your microphone should not pick up any sound from your speakers. In practice this means that your “speakers” must be headphones.

(b), “convince your video call app not to try to suppress echoes,” seemed surprisingly tricky when I tried to research it, because each video call app has its own heuristics for when to engage echo-cancellation.

So I did my own tests of echo cancellation Zoom, Skype, and Hangouts in Chrome and Firefox. I started a chat between two computers, both with headphones attached—a setup that should have required no echo cancellation. I then played music into the microphone of one computer. On the other, I spoke into the microphone and listened for whether the music got quieter.

Zoom, Skype and Hangouts in Firefox all seemed to slightly decrease the audio volume when I spoke, indicating light echo cancellation. For Hangouts in Chrome, the audio cut out _completely_ every time I said anything. In Zoom, I was able to eliminate all echo cancellation by selecting the “use original audio” option, which you can also permanently enable for particular audio devices—I’d recommend doing this.

### Throw your wireless headset in the trash

The gear I recommend in this guide is all wired, not Bluetooth. While Bluetooth seems like it should be great, in practice it has [horrible problems with audio latency, quality and reliability](https://www.benkuhn.net/wireless/). Also, I don’t think wireless open-back headphones (see below) exist.

If you finished the previous paragraph and still think you can get away with using wireless audio gear, read the post at the link :)

### Hear yourself clearly with open-back headphones

Most headphones are _closed-back_, which means they form an acoustic seal over your ear that attenuates outside sound. This is good for “noise isolation” when you’re listening to music. But it’s bad in calls because it also isolates you from your own voice, making you sound muffled and unnatural to yourself. (The same thing also happens with any earbuds that form a seal, i.e. pretty much everything except EarPods or non-Pro AirPods.)

Personally, without the feedback from hearing myself, I also tend to start speaking louder or shouting on calls. This tires out my voice, and can get stressful for whoever I’m ~shouting at~ talking to.

To avoid this, you can buy _open-back headphones_, which have mesh instead of a closed covering over your ears. I bought the [Philips SHP9500](https://amzn.to/3bULynH), which I like a lot; I haven’t tested any other pairs. (I chose a low-end pair because for video calls, sound quality will mostly be limited by people’s microphones; if you want to use the same ones to listen to music, you might want a higher-end pair.)

As an extra bonus, open-back headphones are _way_ more comfortable because they get less hot. I didn’t realize beforehand how much difference this would make, but it’s amazing to be able to wear headphones all day without my ears overheating!

Note that open-back headphones “leak” sound, so anyone near you will hear the other side of the conversation as well. This isn’t a problem if you have your own space, but they’re not suitable for shared spaces. You might think the sound could leak into your own microphone and cause an echo, but I tested and it’s too quiet for that unless you set the volume uncomfortably high.

### Don’t mute

This isn’t about equipment per se, but it has implications for your equipment choices. Quoting [Matt Mullenweg](https://ma.tt/2020/03/dont-mute-get-a-better-headset/), founder of one of the earliest and largest fully-distributed companies:

> One heterodox recommendation I have for audio and video calls when you’re working in a distributed fashion is not to mute, if you can help it. When you’re speaking to a muted room, it’s eerie and unnatural — you feel alone even if you can see other people’s faces. You lose all of those spontaneous reactions that keep a conversation flowing. If you ask someone a question, or they want to jump in, they have to wait to unmute. I also don’t love the “unmute to raise your hand” behavior, as it lends itself to meetings where people are just waiting their turn to speak instead of truly listening.

I strongly agree with this and prefer for the people I’m talking with to stay unmuted unless they have a crappy mic that picks up a lot of noise. Which won’t be your problem as long as you…

### Get a better microphone

Most non-standalone computer microphones, including ones on fancy headsets, sound ear-bleedingly bad. (The 2020 MacBook Pro microphone is okay.) You can sound way more pleasant to your colleagues by getting a nicer one. For instance, compare me reading [Edward Lear](https://www.nonsenselit.org/Lear/BoN/bon010.html) on the following mics:

| Mic | Recording | Commentary |
| --- | --- | --- |

| 2014 iMac | 
 | Sounds like a tin can, because it is |
| Jabra Evolve 70 | 

 | Wirecutter rec; sounds like a bad head cold |
| 2020 MacBook Pro 13" | 

 | “Studio quality” my foot; try “moderate head cold” |
| [V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) | 

 | Stupid name, $30, actually sounds ok |

The best “can’t mess up” microphone option is the last one in the table, the [V-Moda BoomPro](https://www.sweetwater.com/store/detail/BoomProMic--v-moda-boompro-microphone-communication-mic-for-headphones) (with a [foam windscreen](https://amzn.to/3hOU5JU)), which attaches to your headphones in place of a standard 3.5mm audio cable.✻✻ This means the BoomPro requires headphones with a detachable cable. If you’re wedded to a different pair, the [Antlion ModMic](https://amzn.to/2Zypzhq) is a more expensive option that works with any headphones, at the cost of a second cable. There’s also a wireless version, although you shouldn’t use wireless audio equipment for video calls due to latency and quality concerns. It sounds much clearer and less muffled than any headset’s built-in mic. It’ll also pick up less background noise (e.g. typing) than _any_ mic, by virtue of being closer to your mouth, which makes it easier to follow “don’t mute” above. However, it won’t sound quite as natural as a non-headset mic.

If you want something that sounds even more realistic, your best bet is something like an [AT2005](https://amzn.to/3kh95SM) positioned less than six inches from your face using a [boom arm](https://amzn.to/3bVaDP5) that’s [clamped](https://amzn.to/3kb57uP) to your desk. If you don’t want your mic to be visible, it may require some zooming/cropping of your camera setup to get it that close. Compare it to the BoomPro (you’ll need headphones to hear the difference well):

| BoomPro | AT2005 |
| --- | --- |

| 
 | 

 |

I tested a few different microphones, and listened to recordings of many more. I haven’t trained myself to detect small quality differences, but my tentative conclusions are:

1.  Distance to your mouth dominates nearly everything. For the mics I tested, 6 inches vs 12 inches made as big of a difference as switching mics.

    (That’s because from the microphone’s perspective, doubling the distance makes your voice 4x quieter, or equivalently, makes room noise 4x louder. It also makes louder e.g. the echoes of your voice—leading the microphone to produce a “boomy” sound.)

    Here’s two fairly different-sounding microphones at 6 inches vs 12:

| Model | 6 inches | 12 inches |
| --- | --- | --- |

| Blue Yeti | 
 | 

 |
| AT2005USB | 

 | 

 |

2.  Low-end condenser microphones sound somewhat “fuller” or more natural than dynamics, which can sound a little “muffled.” For example, the Blue Yeti above is a condenser while the AT2005USB is a dynamic.

    (Dynamic mics are sometimes said to “reject noise” better than condensers, but I couldn’t find anyone making this claim who explained why.†† It is easier to put a dynamic microphone right next to your mouth, which is sort of like rejecting noise, but not relevant if you want to place your mic outside the frame of a video. Dynamic mics also have less high-frequency and “transient” response, which can make some types of room noise less obtrusive.)

3.  Mics under $50, and headset mics, sound noticeably bad even when close to your mouth. Outside of that, it seemed like sound quality depended as much or more on things other than microphone quality, like how much your space echoes. (There’s a surprisingly large genre of YouTubers demoing expensive mics with bad-sounding setups.) So it seemed like higher-end microphones would probably be wasted in most video call setups.

Based on this, I suggest the AT2005 as a widely-recommended mic in the lowest “doesn’t sound noticeably bad” price tier, that can be stuck on a boom arm with minimal ceremony.

Other microphone comparisons for further reading:

-   [Wirecutter USB mic comparisons](https://soundcloud.com/wirecutter-audio/sets/2019-usb-microphone-testing)

-   [Marco Arment on podcasting microphones](https://marco.org/podcasting-microphones)

-   [Matt Mullenweg “Don’t mute, get a better headset”](https://ma.tt/2020/03/dont-mute-get-a-better-headset/)

-   [rtings.com headset recording quality tests](https://www.rtings.com/headphones/tests/microphone/recording-quality)

### Listen to yourself

In video calls, unlike real life, what you hear is not the same as what’s heard by the person you’re talking to. And mics are fiddly enough that your particular setup and mic technique matters a lot. So it’s really useful to listen to how your audio sounds. You can do this with a web app like [miccheck.me](https://www.miccheck.me/). The most common mic problems are [plosive pops](https://en.wikipedia.org/wiki/Pop_filter) and [harsh sibilants](https://en.wikipedia.org/wiki/De-essing)—pick sentences that will cover those. The [Harvard Sentences](https://www.cs.columbia.edu/~hgs/audio/harvard.html) are a good starting point.

If those consonants sound bad, you might need a better windscreen, or to change how your mic is positioned. For instance, if you have a headset mic, you should position it just beside the corner of your mouth—_not_ directly in front—so that you’re not breathing/spitting into it.

## Video

### Use a dedicated monitor

I recently started putting my active call in full-screen mode on my primary (27") monitor, and using a second monitor for notes or other activity. This turned out to make a surprisingly big difference to how immersive the call felt. (Maybe I should have been clued in by the fact that this feature is called “immersive mode”?) It’s amazing for keeping me focused and present. Possible reasons why:

-   In windowed mode, Zoom keeps your preview at the top of the screen in a bar of its own, but in fullscreen mode there’s no bar, just a floating preview window (which you can also hide). That means there’s a lot more screen available for other people’s video.

-   Hiding the window bezel, task bar, etc. make it much less salient that you’re talking through a computer, and the user interface is less likely to distract you.

-   In windowed mode, I’d sometimes end up tabbing away to look at another window, and lazily forget to tab back. Then I’d spend a lot of the call not looking at people and feeling less connected.

For the second monitor for notes, I use an iPad with [Sidecar](https://support.apple.com/en-us/HT210380), but you can also buy dedicated “portable monitors” for under $200 that would serve this purpose well.

### Improve your lighting

The best way to get a sharper image on any camera is to put more light into the sensor. Laptop webcams have terrible image quality, but a laptop webcam with good lighting will look better than a fancy camera with bad lighting:

[![](https://www.benkuhn.net/vc/imac%20cam%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_133549_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20bad_hu1afdd5c2d162e08ace9eebe225e08ba7_252223_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20bad.jpg "full size")[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_422x280_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")

Left: 2014 iMac webcam destroyed by MAXIMUM BACKLIGHT. Middle: Fancy Sony A6000 struggles too. Right: iMac webcam gloriously lit by a [lumenator](https://www.benkuhn.net/lux/) bounced off the wall in front of me. Click for fullsize.

The two basic rules of lighting are:

1.  Cast lots of diffuse light on your face to make sure it’s brighter than the background. (Also, the more light that’s hitting the scene, the less grainy your image will appear.)

    The easiest way to do this is to put your desk in front of a window; the second-easiest is to bounce artificial lights off of a light-colored surface behind the camera. If neither of those is enough, you can also use a “ring light” or “softbox” (no particular recommendations as I’ve never tried one).

2.  Eliminate light sources in the camera’s field of view. Compared to the human eye, cameras have lower _dynamic range_—they can’t capture large differences in brightness. That’s why, for instance, your phone can’t take good pictures of trees against the sky on a bright day. Similarly, if there’s a bright window behind you, or a light fixture in your camera’s field of view, that part will look “blown out” and everything else will look dark by comparison.

### Use your real background

Probably controversial. I’m speaking strictly from the point of view of immersiveness here—not e.g. expressing your individuality, making your coworkers laugh, or hiding the pile of laundry behind you. Those are all valid reasons to want to use backgrounds! Just be aware that you’re sacrificing immersiveness when you do.

Why? Zoom’s background detection software is not very accurate, and it’ll periodically delete parts of your hair/body, make the background show through your eyeballs, etc. Plus, it’s really bad at detecting boundaries so some of the real background will show through your hair.

If you have a decent camera (as below), and a space of your own (see [get away from other people](https://www.benkuhn.net/vc/#get-away-from-other-people) above), you’ll look less distracting and more real if you don’t use a fake background.

### Don’t bother with webcams

It’s probably obvious that laptop webcams suck. Even if they’re not [inexplicably positioned so that they look up your nose](https://www.reddit.com/r/Dell/comments/5pkpa7/who_thought_this_is_the_perfect_place_to_put_a/), they will be grainy, blurry and have a tiny dynamic range.

Maybe less obviously, external webcams aren’t that much better. This surprised me, since a high-end webcam like the [Logitech Brio](https://www.logitech.com/en-us/product/brio) costs 2/3 as much as a used interchangeable-lens camera and has _one job_.‡‡ To be clear, the Brio [looks a lot better](https://randsinrepose.com/archives/good-meetings-are-jazz/) than most other webcams—just still a lot worse than a real camera. For instance, I bought a Logitech C920, Wirecutter’s pick for “best webcam,” but it wasn’t obviously better than my six-year-old iMac webcam, mostly due to _really_ questionable exposure / color balance settings.§§ The C920 does allow you to tune these settings, but it took me about 2 hours to figure out how, and I ended up having to buy a crappy third-party app. Manually tuning the settings would also require you to change them whenever your lighting changes throughout the day, which is pretty annoying.

[![](https://www.benkuhn.net/vc/imac%20cam%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_156166_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/imac%20cam%20good.jpg "full size")[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")

Left: 2014 iMac camera; right: Logitech C920. I am pretty white, but I'm not THAT white.

### Use your smartphone…

(I haven’t used a smartphone as a webcam extensively because I jumped straight to using a full camera, so these are weakly held. I’ll update as I learn more.)

I expected webcams to be better than smartphones, because they wouldn’t have much reason to exist otherwise. But it turns out I was wrong: webcams do not, in fact, have much reason to exist. Smartphones beat them on sensor size and quality of materials. For example, the iPhone 11 camera [costs $73.50 in materials](https://www.cultofmac.com/658650/iphone-11-pro-max-production-cost-bill-materials-bom/), and has a [1/2.55" sensor](https://photographylife.com/reviews/iphone-11-pro-camera), while the Logitech C920 _retails_ for $80 and has a [1/3" sensor](https://www.techtravels.org/modifying-logitech-c920/).‖‖ Of course, if you have a lower-end smartphone, the C920 might have nicer materials than your smartphone. But it seems like there’s a very small region of tradeoff-space where “buy a webcam” is a good idea relative to “buy a nicer smartphone” or “buy an entry-level camera." So, if you want an external webcam, use a smartphone.

[![](https://www.benkuhn.net/vc/c920_hu1afdd5c2d162e08ace9eebe225e08ba7_348796_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/c920.jpg "full size")[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")

Left: Logitech C920; right: iPhone XR via Reincubate Camo (crop).

I briefly tried two apps for using your smartphone as a webcam, [Camo](https://reincubate.com/camo/) and [EpocCam](https://www.kinoni.com/). EpocCam is cheaper ($8 vs $40) but seemed somewhat buggier than Camo (both had occasional issues). Both of them have free trials that watermark your video, so suitable for testing but not actual calls.

The easiest way to mount a smartphone seems to be via a [gooseneck holder like this](https://amzn.to/3hvwyOf). It seemed like it should be possible to find a much smaller device that attached the phone to my monitor, but the best I could find was [this clip thing](https://amzn.to/2RkLO5E), which obscures part of the screen on laptops, doesn’t adjust in small enough increments to get the right field of view, and doesn’t work on external monitors with curved backs like my iMac. Let me know if you have a better suggestion.

### …or a real camera

Even the lowest-end “real” cameras will trounce most smartphones on image quality. You’ll get a much sharper image, with a pleasingly blurred background to subtly draw attention toward your face and away from your piles of dirty laundry.

[![](https://www.benkuhn.net/vc/camo%20phone_hu7fba9da1679ce4a56c592454604cb9c1_865840_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/camo%20phone.jpg "full size")[![](https://www.benkuhn.net/vc/sony%20good_hu1afdd5c2d162e08ace9eebe225e08ba7_289081_640x426_resize_q75_box.jpg)](https://www.benkuhn.net/vc/sony%20good.jpg "full size")

Left: iPhone XR via Camo; right: Sony A6000. This one makes more sense at full resolution; the Sony has a bit worse dynamic range, but the image is **much** sharper.

This is probably the most noticeable improvement in the list: I started using a camera when I gave a virtual conference talk at [!!con](http://bangbangcon.com/speakers.html#ben-kuhn) and got compliments from ~20 different coworkers and conference speakers. (Being this noticeable is not necessarily an advantage, but I figured my coworkers already mostly knew that I was extremely vain, and so there was no point in trying to hide it.)

Unfortunately, these cameras are also… pretty user-unfriendly and can take a bit of work to set up. Some non-obvious tips if you decide to replicate my setup (Sony A6000 + Elgato CamLink 4k):

-   Get a non-power-zoom lens, otherwise your zoom setting will get reset every time you turn the camera off and on again.

-   If you have a Sony A6000, make sure you turn the top dial to “video” mode. Otherwise the HDMI output will be lower quality and the continuous focus won’t work right.

-   Use the widest aperture possible to minimize graininess and get a nice blurry background effect.

### A note on camera buying

I’m not sure which camera model is best now that so many different manufacturers have webcam drivers. Before the webcam drivers came out, the Sony A6000 was the default recommendation for a camera for streaming, and it’s also generally well-recommended as an entry-level mirrorless camera; but it doesn’t look like it works with Sony’s webcam drivers at all (it’s not in the list of supported models, although the A6100 and later are). Basically, do your own research.

It’s best to buy from a reputable used camera dealer (e.g. [Keh](https://www.keh.com/), [B&H](https://www.bhphotovideo.com/), [Lens Authority](https://www.lensauthority.com/)) since random resellers on Amazon / etc. might not be good about checking that the camera works well.

For the record, here’s what I bought though I don’t particularly endorse it:

-   Sony A6000
-   16-50mm ƒ/3.5–5.6 PZ OSS lens (as mentioned above, I’d recommend the non-PZ 18–55mm ƒ/3.5-5.6 OSS instead)
-   Elgato Camlink 4k (may be out of stock; cheaper options available from no-name vendors)
-   HDMI to Micro HDMI cable (took me a surprisingly long time to figure out what type of HDMI port the A6000 has!)
-   Random off-brand USB dummy battery (sometimes gives “battery depleted” errors so maybe not the best idea)

## Conclusion

The main thing I learned from this adventure is that, much like [wireless](https://www.benkuhn.net/wireless/), video calls still basically don’t work. Every piece of equipment has tricky subtleties that make it super hard to select the right gear and use it correctly. Software does incredibly stupid things that you’ll never notice unless you know what to look for. You never hear your own audio, so if it sucks, you’ll never know. Many of the problems you do notice will be near-impossible to debug because there are no good diagnostics.

If there was a $2000 device that eliminated the need for this post to exist, it would be an automatic purchase for my employer and probably every other distributed company. But there isn’t, so here we are.

_Thanks to [Dan Luu](https://danluu.com/), [Sasha Illarionov](https://sdll.io/), David Coletta, [Alexey Guzey](https://guzey.com/), [Lincoln Quirk](https://www.lincolnquirk.com/), [Eve Bigaj](https://evebigaj.com/) and Jessica Gambirasi for commenting on a draft of this post._