# 招聘与柠檬市场｜Hiring and the market for lemons

---

## 正文

Joel Spolsky 有一篇关于“寻找 Great Developers”的经典 blog post，他在其中推广了 great developers 不可能找到这一 meme，其 corollary 是：如果你能找到某个人，那他就不是 great。Joel 写道，

> 那些 great software developers，事实上，各个领域最优秀的人，根本就从不在 market 上。
> 
> 一般的 great software developer，在整个 career 中，总共也许只会 apply for 四份 jobs。
> 
> ...
> 
> 如果你运气好，如果你真的运气很好，他们会在 open job market 上出现一次，比如，他们的配偶决定接受 Anchorage 的一份 medical internship，于是他们真的把自己的 resume 发给了他们认为自己愿意去工作的那几家 Anchorage 的单位。
> 
> 不过，大多数时候，great developers（而这几乎是个 tautology）就是，呃，great，（好吧，这就是个 tautology），而且，通常 prospective employers 很快就能 recognize 他们的 greatness，这基本上意味着，他们想去哪里工作就能去哪里，所以他们真的不会发出很多 resumes，也不会 apply for 很多 jobs。
> 
> 这听起来像是你想 hire 的那种人吗？应该是的。那条 rule——也就是 great 的人从不在 market 上的 rule——的 corollary 是：bad 的人——那些 seriously unqualified 的人——经常在 market 上。他们总是被 fired，因为他们无法完成自己的 job。他们的 companies 会 fail——有时候是因为任何愿意 hire 他们的 company，大概也会 hire 很多 unqualified programmers，所以最终累积成了 failure——但有时候，是因为他们确实太 unqualified，以至于毁掉了 company。没错，确有其事。
> 
> 谢天谢地，这些 morbidly unqualified 的人很少能得到 jobs，但他们确实一直在 applying，而当他们 apply 时，他们会去 Monster.com，一次勾选 300 或 1000 份 jobs，试图中彩票。
> 
> 我想，敏锐的读者会指出，我还漏掉了人数最多的群体，也就是那些 solid、competent 的人。他们出现在 market 上的频率比 great 的人高，但比 incompetent 的人低，总体而言，他们会少量出现在你那堆 1000 份 resume 中。不过，大多数时候，眼下 Palo Alto 几乎每位桌上堆着 1000 份 resumes 的 hiring manager，拿到的都是完全相同的那组 970 份 resumes，来自同一小群 970 个 incompetent 的人，他们正在 applying for Palo Alto 的每一份 job，而且大概一辈子都会如此；其中只有 30 份 resumes 甚至值得考虑，而在这之中，也许，极少的时候，有一个人是 great programmer。好吧，也许连一个都没有。

Joel 的 claim 基本上是：“great” developers 相较于“bad” developers，不会有那么多 jobs，因为 companies 会试图留住“great” developers。Joel 还 posits，companies 可以轻易 recognize 潜在的“great” developers。但这两个 statements 很难 reconcile。如果 identify 潜在的“great” developers 这么容易，为什么不试着 recruit 他们呢？你同样可以轻易提出这样的论点：“great” developers 在 market 中是 overrepresented，因为他们有更好的 opportunities，而会死守自己 jobs 的反倒是“bad” developers。这种 adverse selection 在正在衰落的 companies 中很常见；除了其他地方，我在 IBM 的 intern cohort 中就见过这种情况[1](https://danluu.com/hiring-lemons/#fn:I)。

“good” developers 在 market 中应该是 overrepresented，还是 underrepresented？如果我们听听那些关于 hiring 的 anecdotal 抱怨，就可能会问：developers 的 market 是不是一个 market for lemons？这个想法可以追溯到 Akerlof 那篇获得 Nobel prize 的 1970 年 paper：“[‘Lemons’ 的 Market：Quality Uncertainty 与 Market Mechanism](http://www.econ.yale.edu/~dirkb/teach/pdf/akerlof/themarketforlemons.pdf)”。Akerlof 以 used car sales 为例，把 market 分为 good used cars 和 bad used cars（bad cars 被称为“lemons”）。如果无法 distinguish good cars 和 lemons，good cars 和 lemons 就会以相同的 price 进行 sell。由于 buyers 无法 distinguish good cars 和 bad cars，他们愿意支付的 price 就取决于 market 中 average 的 quality。由于 owners 知道自己的 car 是不是 lemon，non-lemons 的 owners 就不会 sell，因为 lemons 的存在会压低 average price。这会产生一个 feedback loop，导致可供选择的只剩下 lemons。

这个 model 显然不同于 Joel 的 model。Joel 的 model 假定“great” developers 是 sticky 的——也就是说，他们会在每份 job 上待很长时间。这来自两个 assumptions；第一，prospective employers 很容易 identify 谁是“great”；第二，一旦某个人被 identified 为“great”，其当前的 employer 就会想尽一切办法留住他（就像 market for lemons 中那样）。但仅仅第一个 assumption 就足以防止 developer job market 成为 market for lemons。如果你能看出一位 _potential_ employee 是 great，你只需要去 offer 他们当前收入的两倍即可（我确实见过这种事）。要 create 一个 market for lemons，你需要 information asymmetry，而 Joel 却 posits 并不存在 information asymmetry。

如果我们暂且放下 Joel 的 argument，看看 job market，就会发现存在 incomplete information，但当前和 prospective employers 都拥有 incomplete information，而且谁的 information 更好，差异很大。实际上，prospective employers 拥有比当前 employers 更好的 information，是相当常见的！

举个例子，我曾与一个人共事，姑且叫他 Bob，他做了那些必要的 grunt work，避免 project 彻底崩溃，挽救了两个不同的 projects。这两个 projects 都被宣布为 successes，promotions 也发放了，他们还进行了[一次大规模 PR blitz，其中包括向那些惯常的媒体投放文章；Wired、Fortune](http://www.paulgraham.com/submarine.html)，等等。这对那些擅长抢功的人来说结果很好，但对 Bob 就没那么好了。事实上，最近另一位曾与我共事的人告诉我，management 不断问他，为什么 Bob 做 simple tasks 要花那么长时间。答案是，Bob 正忙着确保自己负责的 services 在 launch 时不会发生 global outages，但在 Bob 的 org 中，这不是能让你获得认可的事情。其结果是，Bob 拥有一个知道他是 great 的 network，这让他很容易在其他任何地方以 market rate 得到一份 job。但他的 management chain 对此一无所知，而且根据我如今看到的 offers，他们付给他的钱大约只有他在其他地方能挣到的一半。有很多这样的例子：company 内部的 information transfer 如此糟糕，以至于 external management 比 internal management 更了解某个人的 productivity。我心里想到的是一个具体例子，但如果只考虑 Bob 这种 archetype，不用仔细回想，我就知道目前有四个人处于类似境况。我目前工作的 company 恰好因这种 dysfunctional 的表现而臭名昭著，这当然有助于我见到这些例子，但这种事到处都会发生。我在一家小 company 工作时，我们经常从大 companies 中 hired great engineers，因为那些大 companies 太糊涂，根本不知道自己拥有什么样的 talent。

“great” developers 是 sticky 的这一想法，还有一个问题，就是它假定 companies 能够随时按需 creating developers 想为之工作的 groups。通常并非如此。举个例子，我曾加入一个 team，那里的 TL 相当强烈地反对 using version control，也反对 having tests。由于这些（以及其他）practices，五名 devs 花了一年时间，才为一个 straightforward problem produce 出 10k lines 勉强能 working 的 code。此外，那里的气氛像高压锅，大家被要求每周工作 80+ hour，PM 会通过羞辱来迫使大家工作更长时间。不到一年，我加入时 team 中七个人里的三个人已经离开；其中两个人去了不同的 companies。company 不想失去那两个人，却没有能力 creating 一个能留住他们的 environment。

就在我加入那个 team 前后，我的一个朋友加入了一个真正 great 的 team。他们做的 work 能够实质性地影响世界，他们有 freedom 和 creativity 的空间，jobs 的很大一部分涉及 learning 新鲜有趣的事物，等等。每当听说有人在找工作，我就会把那个 team 推荐给他。那个 team 如今在可预见的未来都已满员，因为所有在 network 中认识那个 team 的人，都把人推荐到了那里。但如果你看看那个一年内七个人走了三个的 team，它正在 hiring。大量 hiring。这种 dynamic 的结果是，作为一名 dev，如果你加入一个 random team，就极有可能加入一个有着大量 churn 的 team。此外，如果你知道一个 good team，它很可能已经满员了。

Joel 的 model 隐含地假定，按比例而言，dysfunctional developers 远多于 dysfunctional work environments。

在我上次参加的 conference 上，我问了遇到的大多数人两个问题：

1.  你知道有什么 companies 不是 highly dysfunctional 的吗？
2.  你知道有哪些具体的 teams 是 great，而且正在 hiring 的吗？

没有任何一个人告诉我，他们的 company 符合 (1) 中的 criteria。少数人说，也许 Dropbox 还可以，或者，也许 Jane Street 还可以，但答案都是“我认识那里的几个人，还没听说过什么可怕的事，而且有时候还能听到些好事”，而不是“那家 company 是 great，你绝对应该去那里工作”。大多数人说，他们不知道有哪家 companies 不是[一团糟](https://danluu.com/wat/)。

少数人对 (2) 有些建议，但最常见的答案类似于“哈哈，没有，要是我知道，我早就去那里工作了”。第二常见的答案则是“我认识 Google Brain team 的一些人，听起来很棒”。有少数 teams 以适宜工作而闻名，但事实是，它们实在太稀少了，基本上不可能在其中某个 team 得到一份 job。少数人确实知道一些正在 hiring、而且他们会强烈推荐的 teams，但这种情况很少。远比找到一个我愿意与之共事、而且愿意考虑换工作的 developer 更少。如果我把问题反过来，问他们是否知道有什么正在找工作的 good developers，答案通常是“知道”[2](https://danluu.com/hiring-lemons/#fn:L)。

“great” developers 之所以不可能找到，是因为他们加入 companies 后就会留下来——这一想法还有一个问题，就是 developers（以及 companies）并不是 immutable 的。因为我很幸运，能够在允许人们真正 flourish 的 environments 中工作，所以我见过很多人从 unremarkable 变成 amazing。由于大多数 companies 在帮助人这件事上几乎毫无投入，你不需要付出太多努力，就可以在这方面做得很好。

另一方面，我也见过整个 teams 的 devs 因为 environment 变了而进入 market。举个例子，我以前认识 _很多_ 在 company X 的 [Marc Yun](https://www.linkedin.com/in/marcyun) 手下工作的人。那是一个 low attrition 的地方，因为大家真的很喜欢在那里工作。然后 Marc 离开了。接下来的两年里，我认识的在那里工作的每一个人，真的全都离开了。这一个变化，既在 searching-for-a-team job market 中制造了一个 lemon，又把一批 good developers 推向了 market。这种事一直都在发生，而且由于如今 acquisition-heavy 的 environment，现在发生得比过去还多。

开发者的 hiring 是一个 market for lemons 吗？嗯，这取决于你指的是什么。developers 和 hiring managers 都有 incomplete information。一个方向上的 market for lemons 会让另一个方向变得更好还是更糟，这并不明显。加入新 team 的 uncertain 性让 developers 不太愿意离开现有 teams，这使得 hire developers 更难。但 developers 经常加入自己不喜欢的 teams，这又使得 hire developers 更容易。这其中的 net effect 是什么？我不知道。

在我看来，找到一个 good manager/team 似乎真的很难，而我不知道有什么 replicable strategy 能做到这一点；我非常同情那些找不到 good fit 的人，因为我明白这有多难。但我确实见过用于 hiring 的 replicable strategies，所以对于那些抱怨 hiring “great” developers 不可能的 hiring managers，我就没有那么同情了。

当一个 hiring manager 抱怨 hiring 时，在我迄今见过的每一个案例中，这个 hiring manager 都有以下问题之一：

1.  他们的 pay 太少。上次我找工作时，我发现，可能会 hire 我的 companies 之间，_在同一个 geographic region_，compensation 有 6x 的 difference。基本上所有 companies 都认为自己是 competitive，即使它们处于 range 的 bottom end。我不知道这是怎么回事，但 companies 似乎总认为自己的 pay 很好，即使它们连合适的 range 都远远没达到。几乎每个和我交谈的人都告诉我，他们的 pay 不低于任何一家_合理的_ company。当然，有些 companies 的 pay 稍微高一些，但它们是在 overpaying！读 Joel 的文章，你其实就能看到这一点——当年他写我上面引用的那篇文章时，他谈到 Fog Creek 的 paid 有多好。几年后，他抱怨 Google 对没有 experience 的大学生是在 overpaying，而最近，[他几乎已经说，你不会想在 pay 很好的 companies 工作](https://twitter.com/danluu/status/784929052519837696)。

2.  他们会 pass on good 甚至 “great” developers[3](https://danluu.com/hiring-lemons/#fn:H)。前面我说过，我认识许多正在找工作的 good developers。你可能会问，既然有这么多 good developers 在找工作，为什么找到他们这么难？Joel 声称，在 1000 份 resumes 中，也许 30 人会是 “solid”，970 人会是 “incompetent”。在我看来，更像是 400 人会是 solid，20 人会是非常 good。只是几乎所有人都使用相同的 filters，所以最后大家都在争抢他们认为 solid 的那 30 人。当人们通过 randomized trials 研究究竟是什么导致 resumes 被 filtered out 时，结果往往表明，与 job performance 只有 tangentially related 或 unrelated 的 traits 会造成巨大 differences。例如，[在这项关于 law firm recruiting 的 study 中](http://www-2.rotman.utoronto.ca/facbios/file/RiveraTilcsik.pdf)，authors 发现，在没有任何其他 changes 的情况下，male 身份与 resume 上的 “high-class” signifiers（sailing、polo 和 classical music，而不是 track and field、pick-up soccer 和 country music）的 combination，导致 interview invites 出现了 4x increase。

    我工作的第一家 company，[Centaur](https://www.amazon.com/gp/product/B01FSZU6FK/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=abroaview-20&creative=9325&linkCode=as2&creativeASIN=B01FSZU6FK&linkId=d15e514c6ecefa224be8f05d4d5837e3)，其 onsite interview process 比 Google 和 Facebook 这类地方的 phone screen 还不 stringent。如果听 Joel 这类人的说法，你会以为 Centaur 里全是笨蛋，但在 industry 工作十多年后（包括在 Google 的经历），在我工作过的所有地方中，Centaur 的 developer productivity 的 mean 和 median level 都是最好的。

    Matasano 通过使用一套不同的 filters、招到一批不同的人，解决了自己的 hiring 问题，这件事广为人知。尽管他们的 strategy 取得了巨大 success，几乎所有人仍坚持沿用 standard strategy：挑选有 brand name pedigrees 的人，并运行与其他人基本相同的 interview process，[不断 bidding up 那些时髦人选的 price，而忽略其他所有人](http://danluu.com/programmer-moneyball/)。

    看看我认识的、如今处于 high-demand 的 developers，其中很大一部分人都经历过长达多年的时期，那时他们处于 underemployed 状态，几乎是在恳求得到有趣的工作。只要你能找到他们，这些人就非常容易 hire。

3.  他们试图 hire 具备某种 rare skills 的 combination 的人。眼下，如果你试图 hire 一个既有 deep learning 的 experience，又有——嗯——任何其他方面的 experience 的人，你会很不好受。

4.  他们的 dysfunctional 程度远超自己的认识。我认识一位 hiring manager，他抱怨 hire 有多难。他没有意识到的是，他的 team 上真的每个人都极其不开心，而且他的 team 中有相当一部分人会向朋友提供 anti-referrals，告诫他们远离这里。

    那是一个 extreme case，但 VP 或 founder 对 hiring 为什么这么难感到困惑，而 employees 却认为这个地方 mediocre 甚至 bad，这种情形相当常见。

在这些问题中，(1)，low pay，既是最常见的，也是最容易 fix 的。

过去几年里，Oracle 和 Alibaba 在 Seattle 建立了新的 cloud computing groups。这是一个相对 competitive 的 area，而且两家 companies 的 reputations 都会在 hiring 时对它们不利[4](https://danluu.com/hiring-lemons/#fn:O)。如果你相信那些关于 hire 有多难的抱怨，你就不会认为一家 company 能在 Seattle 建立完整的 cloud teams，更不用说两家了。两家 companies 都通过 paying 显著高于 competitors 为具有类似 experience 的人提供的金额，解决了这个问题。Alibaba 因如此慷慨的 offers 而闻名，以至于我 negotiating Microsoft 给我的 offer 时，MS 告诉我，除了 Alibaba，任何 company 的 offer 他们都会 match。我相信，Oracle 和 Alibaba 在过去几年里已经 hired 了数百名 engineers。

大多数 companies 需要 hire 的人数远不到数百人；它们可以 pay competitively，而不必 hiring 多到足以让整个 market moves upwards 的 developers，但它们仍然拒绝这样做，同时又抱怨 hire 有多难。

(2)，filtering out good potential employees，似乎是“从来没有人因为 hiring IBM 而被 fired”的现代版本。如果你 hire 了一个 background 时髦、擅长 traditional coding interviews 的人，而结果不如人意，谁能责怪你呢？而且，也不会有人注意到你错过的所有人。与 (1) 一样，几乎所有人都认为自己在这件事上做得很好，他们会说出这样的话：“我们得 lower our bar 才能 hire 更多人，而没人想那样。”但我从未在任何一个不会 filter out 大量后来在别处做出 great work 的人的地方工作过。我曾试图让 underrated programmers[5](https://danluu.com/hiring-lemons/#fn:U) 被我工作过的地方 hired，但我真的从未成功让其中任何一人被 hired。有一次，一个我没能帮忙促成 hired 的人，在大约四年 underemployed 之后，设法在 Google 找到了一份工作（而且在那里是明星）。后来，这个人又让我被 Google hired。没有 hiring 那个人，不仅让他们失去了我这位才华横溢的朋友，最终还让他们失去了我！

顺便说一句，这说明了 Joel 关于 “great” devs 从不 apply for jobs 的想法存在一个问题。“great” dev 往往会经历很长一段极难被 hired 的时期，即使通过那些知道他们很 great 的 network 也一样，因为他们看起来不像人们心目中的 “great” developers。此外，Google 对哪些 hiring channels 能产生 good results 做过大量研究，并发现 referrals 和 internal recommendations 实际上并不会 generate 多少 signal。人们会 refer “great” devs，但也会 refer 糟糕的 devs。大多数 companies 建立的 referral bonus scheme 会 skews incentives，使 referrals 比你预期的更差。由于这个以及其他问题，许多 companies 并不会对 referrals 赋予特别高的 weight，而 “great” developers 仍然要经历 normal hiring process，就像其他所有人一样。

(3)，需要奇怪的 skills 的 combination，可以通过 hiring 只具备所需 expertise 的一半或三分之一的人，再 training 他们来 solved。在这一点上，人们似乎不需要太多说服，而且我经常看到这样的事发生。

(4)，dysfunction [似乎很难 fix](https://danluu.com/learning-to-program/#fixing-totally-broken-danluu-com-wat-situations)。如果我知道怎么做，我就会是 manager 了。

作为一名 dev，在我看来，我所知道的那些确实有 good environments、且 pay 很好的 teams，并没有 hiring 问题，而那些 hiring 遇到困难的 teams 也能相当容易地 solve 那个问题。但我是 biased。我不是 hiring manager。可能有某个 hiring manager 正在想：“我认识的每个抱怨很难找到 good team 的 developer，都有这四个 obvious problems 之一；要是我的问题也这么容易 solve 就好了！”

感谢 Leah Hanson、David Turner、Tim Abbott、Vaibhav Sagar、Victor Felder、Ezekiel Smithburg、Juliano Bortolozzo Solanho、Stephen Tu、Pierre-Yves Baccou、Jorge Montero、Alkin Kaz、Ben Kuhn 和 Lindsey Kuper 提供意见与指正。

如果你喜欢这篇文章，你可能也会喜欢[另一篇文章，它讨论了声称 tech hiring 中绝不可能存在 discrimination 的说法有多么荒谬](http://danluu.com/tech-discrimination/)。

* * *

1.  留下来的人描述了一种 environment，其中基本缺少他们愿意共事的 mid-level people。有些 lifers 已经在那里待了很久，而且会一直待到 retirement，也有些 new grads 偶然来到了那里。但与他们的 competitors 相比，拥有 5-15 years 的 experience 的人相对很少。我认识的人中待得最久的那个，一直待到了第 8 year，但当他发现自己 team 上另一个 competent 的人正在 interviewing 时，他就开始为离开而 interviewing；两人都不想成为 team 上唯一干活的人，于是争着先走。 [\[返回\]](https://danluu.com/hiring-lemons/#fnref:I)
2.  这一节听起来有点像我在找工作。我并没有找工作，不过如果我的伴侣在 Seattle 以外接受一份工作，我可能最终会被迫找工作。 [\[返回\]](https://danluu.com/hiring-lemons/#fnref:L)
3.  [Moishe Lettvin 有一个我非常喜欢的演讲](https://www.youtube.com/watch?v=r8RxkpUvxK0)，其中他谈到，有一次他在 hiring committee 上，他们 rejected 了所有提交上来的 candidate，最后却发现，这些 “candidates” 实际上是他们自己的 interviews 的 anonymized versions！

    他最初在 Microsoft 开始 interviewing 时的那段经历，MS 的人听起来应该很熟悉。像经常发生的那样，他在没有预先通知、没有准备的情况下就被扔进了 interview。他不知道该做什么，结果写出的 interview feedback 并不好。“以典型的 Microsoft 风格”，他的 manager 把 interview feedback 转发给整个 team，并说“不要这样做”。“以典型的 Microsoft 风格”是 Moishe 的原话，但我也见过同样的事情。我想谈谈，我们多么倾向于做 _extremely blameful_ 的 postmortems，以及这如何 warps incentives，但那大概值得单独写一篇文章。

    好吧，我来讲个故事，纪念一下最近离开我以前的 team、去了 Google 的那个人。那家伙 joined 后不久，有个周末他正在办公室里（这在他的 team 很常见）。另一个 team 的一位 manager 通过聊天联系他，请他对那个 team 的一些 code 进行 sign off。这个新人想帮上忙，就对那些 code 进行了 signed off。星期一，新人和他的 mentor 聊了聊，他的 mentor 建议他不要这样帮助其他 teams。后来，发生了一次与那些 code 有关的 outage。按照典型的 Microsoft 风格，另一个 team 的那位 manager 成功地把那次 outage 的责任从自己的 team 推给了这个新人。

    请注意，这个人并没有被 included 在我的 3/7 stat 中，因为他是在我之后不久才 joined 的，而且我并不是想 cherry pick 一个 attrition 尽可能最高的 window。

    [\[返回\]](https://danluu.com/hiring-lemons/#fnref:H)
4.  有一段时间，Oracle 声称 Seattle office 的 culture 与 mainline-Oracle culture 完全不同，但据我所知，他们还是忍不住对 Seattle group 进行了 Oracle-ifying，因此那套 pitch 的这一部分已不再令人信服。 [\[返回\]](https://danluu.com/hiring-lemons/#fnref:O)
5.  这条 footnote 是对 Ben Kuhn 的回应，他问我，哪些类型的 devs 是 underrated 的，又该如何找到他们？我认为，这个 group 足够 diverse，不存在一种简单的方法就能找到他们。有些人像 "Bob" 一样，做着根本没有被注意到的 critical work。还有一些人就是特别不擅长 interviewing，比如 [Jeshua Smith](https://www.linkedin.com/in/jeshua-smith-1a873858)。我相信，他只有一次得到的 performance review 不是 excellent（那个学期，他的 manager 说自己只能给出一个 top rating，而只给他的两位 top performers 中的一位并不公平，所以他给了他们两人 average ratings）。在他工作过的每个地方，大家都知道，遇到 hard problems 或 questions 可以去找他，而 ranking 高得多的 engineers 也经常找他帮忙。我曾试着让自己工作过的两家不同 companies hired 他，但他两次 interviews 都 failed 了。他就是不擅长 interviews。据我了解，他的 interview performance 几乎让他没能得到现在的 job，但他的 references 数量如此之多、力度如此之强，以至于他现在的 company 最终还是决定冒险录用他。但他之所以有那些 references，只是因为他原来的 org 一直在 disintegrating。他的新 company 从他的旧 company 招来了_很多_人，所以新 company 有许多人认识他。几乎在其他任何地方，都没人愿意搭理他。我还曾试着让另一个人 hired，但也没有成功，姑且叫她 Ashley。她在 Google 的 recruiter screening phase 就被 rejected，理由是不够 technical，尽管我的 internal recommendation 说，她是我认识的最强的 programmers 之一。但她有着 "nontraditional" background，不符合那位 recruiter 对 programmer 应有模样的想象，于是事情就这么结束了。Nontraditional 是个有意思的 term，因为似乎大多数 programmers 都有 "nontraditional" background，不过你懂我的意思。

    这里的情况足够多样，因此不存在一种能找到所有这些人的方法。采用一个更像 Matasano、而不那么像 Google、Microsoft、Facebook，以及几乎任何你能叫出名字的 YC startup 等的 filtering process，或许是个不错的开始。

    [\[返回\]](https://danluu.com/hiring-lemons/#fnref:U)

---

## 原文

Joel Spolsky has a classic blog post on "Finding Great Developers" where he popularized the meme that great developers are impossible to find, a corollary of which is that if you can find someone, they're not great. Joel writes,

> The great software developers, indeed, the best people in every field, are quite simply never on the market.
> 
> The average great software developer will apply for, total, maybe, four jobs in their entire career.
> 
> ...
> 
> If you're lucky, if you're really lucky, they show up on the open job market once, when, say, their spouse decides to accept a medical internship in Anchorage and they actually send their resume out to what they think are the few places they'd like to work at in Anchorage.
> 
> But for the most part, great developers (and this is almost a tautology) are, uh, great, (ok, it is a tautology), and, usually, prospective employers recognize their greatness quickly, which means, basically, they get to work wherever they want, so they honestly don't send out a lot of resumes or apply for a lot of jobs.
> 
> Does this sound like the kind of person you want to hire? It should.The corollary of that rule--the rule that the great people are never on the market--is that the bad people--the seriously unqualified--are on the market quite a lot. They get fired all the time, because they can't do their job. Their companies fail--sometimes because any company that would hire them would probably also hire a lot of unqualified programmers, so it all adds up to failure--but sometimes because they actually are so unqualified that they ruined the company. Yep, it happens.
> 
> These morbidly unqualified people rarely get jobs, thankfully, but they do keep applying, and when they apply, they go to Monster.com and check off 300 or 1000 jobs at once trying to win the lottery.
> 
> Astute readers, I expect, will point out that I'm leaving out the largest group yet, the solid, competent people. They're on the market more than the great people, but less than the incompetent, and all in all they will show up in small numbers in your 1000 resume pile, but for the most part, almost every hiring manager in Palo Alto right now with 1000 resumes on their desk has the same exact set of 970 resumes from the same minority of 970 incompetent people that are applying for every job in Palo Alto, and probably will be for life, and only 30 resumes even worth considering, of which maybe, rarely, one is a great programmer. OK, maybe not even one.

Joel's claim is basically that "great" developers won't have that many jobs compared to "bad" developers because companies will try to keep "great" developers. Joel also posits that companies can recognize prospective "great" developers easily. But these two statements are hard to reconcile. If it's so easy to identify prospective "great" developers, why not try to recruit them? You could just as easily make the case that "great" developers are overrepresented in the market because they have better opportunities and it's the "bad" developers who will cling to their jobs. This kind of adverse selection is common in companies that are declining; I saw that in my intern cohort at IBM[1](https://danluu.com/hiring-lemons/#fn:I), among other places.

Should "good" developers be overrepresented in the market or underrepresented? If we listen to the anecdotal griping about hiring, we might ask if the market for developers is a market for lemons. This idea goes back to Akerlof's Nobel prize winning 1970 paper, "[The Market for 'Lemons': Quality Uncertainty and the Market Mechanism](http://www.econ.yale.edu/~dirkb/teach/pdf/akerlof/themarketforlemons.pdf)". Akerlof takes used car sales as an example, splitting the market into good used cars and bad used cars (bad cars are called "lemons"). If there's no way to distinguish between good cars and lemons, good cars and lemons will sell for the same price. Since buyers can't distinguish between good cars and bad cars, the price they're willing to pay is based on the quality of the average in the market. Since owners know if their car is a lemon or not, owners of non-lemons won't sell because the average price is driven down by the existence of lemons. This results in a feedback loop which causes lemons to be the only thing available.

This model is certainly different from Joel's model. Joel's model assumes that "great" developers are sticky -- that they stay at each job for a long time. This comes from two assumptions; first, that it's easy for prospective employers to identify who's "great", and second, that once someone is identified as "great", their current employer will do anything to keep them (as in the market for lemons). But the first assumption alone is enough to prevent the developer job market from being a market for lemons. If you can tell that a _potential_ employee is great, you can simply go and offer them twice as much as they're currently making (something that I've seen actually happen). You need an information asymmetry to create a market for lemons, and Joel posits that there's no information asymmetry.

If we put aside Joel's argument and look at the job market, there's incomplete information, but both current and prospective employers have incomplete information, and whose information is better varies widely. It's actually quite common for prospective employers to have better information than current employers!

Just for example, there's someone I've worked with, let's call him Bob, who's saved two different projects by doing the grunt work necessary to keep the project from totally imploding. The projects were both declared successes, promotions went out, they did [a big PR blitz which involves seeding articles in all the usual suspects; Wired, Fortune](http://www.paulgraham.com/submarine.html), and so on and so forth. That's worked out great for the people who are good at taking credit for things, but it hasn't worked out so well for Bob. In fact, someone else I've worked with recently mentioned to me that management keeps asking him why Bob takes so long to do simple tasks. The answer is that Bob's busy making sure the services he works on don't have global outages when they launch, but that's not the kind of thing you get credit for in Bob's org. The result of that is that Bob has a network who knows that he's great, which makes it easy for him to get a job anywhere else at market rate. But his management chain has no idea, and based on what I've seen of offers today, they're paying him about half what he could make elsewhere. There's no shortage of cases where information transfer inside a company is so poor that external management has a better view of someone's productivity than internal management. I have one particular example in mind, but if I just think of the Bob archetype, off the top of my head, I know of four people who are currently in similar situations. It helps that I currently work at a company that's notorious for being dysfunctional in this exact way, but this happens everywhere. When I worked at a small company, we regularly hired great engineers from big companies that were too clueless to know what kind of talent they had.

Another problem with the idea that "great" developers are sticky is that this assumes that companies are capable of creating groups that developers want to work for on demand. This is usually not the case. Just for example, I once joined a team where the TL was pretty strongly against using version control or having tests. As a result of those (and other) practices, it took five devs one year to produce 10k lines of kinda-sorta working code for a straightforward problem. Additionally, it was a pressure cooker where people were expected to put in 80+ hour weeks, where the PM would shame people into putting in longer hours. Within a year, three of the seven people who were on the team when I joined had left; two of them went to different companies. The company didn't want to lose those two people, but it wasn't capable of creating an environment that would keep them.

Around when I joined that team, a friend of mine joined a really great team. They do work that materially impacts the world, they have room for freedom and creativity, a large component of their jobs involves learning new and interesting things, and so on and so forth. Whenever I heard about someone who was looking for work, I'd forward them that team. That team is now full for the foreseeable future because everyone whose network included that team forwarded people into that team. But if you look at the team that lost three out of seven people in a year, that team is hiring. A lot. The result of this dynamic is that, as a dev, if you join a random team, you're overwhelmingly likely to join a team that has a lot of churn. Additionally, if you know of a good team, it's likely to be full.

Joel's model implicitly assumes that, proportionally, there are many more dysfunctional developers than dysfunctional work environments.

At the last conference I attended, I asked most people I met two questions:

1.  Do you know of any companies that aren't highly dysfunctional?
2.  Do you know of any particular teams that are great and are hiring?

Not one single person told me that their company meets the criteria in (1). A few people suggested that, maybe, Dropbox is ok, or that, maybe, Jane Street is ok, but the answers were of the form "I know a few people there and I haven't heard any terrible horror stories yet, plus I sometimes hear good stories", not "that company is great and you should definitely work there". Most people said that they didn't know of any companies that weren't [a total mess](https://danluu.com/wat/).

A few people had suggestions for (2), but the most common answer was something like "LOL no, if I knew that I'd go work there". The second most common answer was of the form "I know some people on the Google Brain team and it sounds great". There are a few teams that are well known for being great places to work, but the fact that they're so few and far between that it's basically impossible to get a job on one of those teams. A few people knew of actual teams that they'd strongly recommend who were hiring, but that was rare. Much rarer than finding a developer who I'd want to work with who would consider moving. If I flipped the question around and asked if they knew of any good developers who were looking for work, the answer was usually "yes"[2](https://danluu.com/hiring-lemons/#fn:L).

Another problem with the idea that "great" developers are impossible to find because they join companies and then stick is that developers (and companies) aren't immutable. Because I've been lucky enough to work in environments that allow people to really flourish, I've seen a lot of people go from unremarkable to amazing. Because most companies invest pretty much nothing in helping people, you can do really well here without investing much effort.

On the flip side, I've seen entire teams of devs go on the market because their environment changed. Just for example, I used to know _a lot_ of people who worked at company X under [Marc Yun](https://www.linkedin.com/in/marcyun). It was the kind of place that has low attrition because people really enjoy working there. And then Marc left. Over the next two years, literally everyone I knew who worked there left. This one change both created a lemon in the searching-for-a-team job market and put a bunch of good developers on the market. This kind of thing happens all the time, even more now than in the past because of today's acquisition-heavy environment.

Is developer hiring a market for lemons? Well, it depends on what you mean by that. Both developers and hiring managers have incomplete information. It's not obvious if having a market for lemons in one direction makes the other direction better or worse. The fact that joining a new team is uncertain makes developers less likely to leave existing teams, which makes it harder to hire developers. But the fact that developers often join teams which they dislike makes it easier to hire developers. What's the net effect of that? I have no idea.

From where I'm standing, it seems really hard to find a good manager/team, and I don't know of any replicable strategy for doing so; I have a lot of sympathy for people who can't find a good fit because I get how hard that is. But I have seen replicable strategies for hiring, so I don't have nearly as much sympathy for hiring managers who complain that hiring "great" developers is impossible.

When a hiring manager complains about hiring, in every single case I've seen so far, the hiring manager has one of the following problems:

1.  They pay too little. The last time I went looking for work, I found a 6x difference in compensation between companies who might hire me _in the same geographic region_. Basically all of the companies thought that they were competitive, even when they were at the bottom end of the range. I don't know what it is, but companies always seem to think that they pay well, even when they're not even close to being in the right range. Almost everyone I talk to tells me that they pay as much as any _reasonable_ company. Sure, there are some companies out there that pay a bit more, but they're overpaying! You can actually see this if you read Joel's writing -- back when he wrote the post I'm quoting above, he talked about how well Fog Creek paid. A couple years later, he complained that Google was overpaying for college kids with no experience, and more recently [he's pretty much said that you don't want to work at companies that pay well](https://twitter.com/danluu/status/784929052519837696).

2.  They pass on good or even "great" developers[3](https://danluu.com/hiring-lemons/#fn:H). Earlier, I claimed that I knew lots of good developers who are looking for work. You might ask, if there are so many good developers looking for work, why's it so hard to find them? Joel claims that out of a 1000 resumes, maybe 30 people will be "solid" and 970 will be "incompetent". It seems to me it's more like 400 will be solid and 20 will be really good. It's just that almost everyone uses the same filters, so everyone ends up fighting over the 30 people who they think are solid. When people do randomized trials on what actually causes resumes to get filtered out, it often turns out that traits that are tangentially related or unrelated to job performance make huge differences. For example, [in this study of law firm recruiting](http://www-2.rotman.utoronto.ca/facbios/file/RiveraTilcsik.pdf), the authors found that a combination of being male and having "high-class" signifiers on the resume (sailing, polo, and classical music instead of track and field, pick-up soccer, and country music) with no other changes caused a 4x increase in interview invites.

    The first company I worked at, [Centaur](https://www.amazon.com/gp/product/B01FSZU6FK/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=abroaview-20&creative=9325&linkCode=as2&creativeASIN=B01FSZU6FK&linkId=d15e514c6ecefa224be8f05d4d5837e3), had an onsite interview process that was less stringent than the phone screen at places like Google and Facebook. If you listen to people like Joel, you'd think that Centaur was full of bozos, but after over a decade in industry (including time at Google), Centaur had the best mean and median level of developer productivity of any place I've worked.

    Matasano famously solved their hiring problem by using a different set of filters and getting a different set of people. Despite the resounding success of their strategy, pretty much everyone insists on sticking with the standard strategy of picking people with brand name pedigrees and running basically the same interview process as everyone else, [bidding up the price of folks who are trendy and ignoring everyone else](http://danluu.com/programmer-moneyball/).

    If I look at developers I know who are in high-demand today, a large fraction of them went through a multi-year period where they were underemployed and practically begging for interesting work. These people are very easy to hire if you can find them.

3.  They're trying to hire for some combination of rare skills. Right now, if you're trying to hire for someone with experience in deep learning and, well, anything else, you're going to have a bad time.

4.  They're much more dysfunctional than they realize. I know one hiring manager who complains about how hard it is to hire. What he doesn't realize is that literally everyone on his team is bitterly unhappy and a significant fraction of his team gives anti-referrals to friends and tells them to stay away.

    That's an extreme case, but it's quite common to see a VP or founder baffled by why hiring is so hard when employees consider the place to be mediocre or even bad.

Of these problems, (1), low pay, is both the most common and the simplest to fix.

In the past few years, Oracle and Alibaba have spun up new cloud computing groups in Seattle. This is a relatively competitive area, and both companies have reputations that work against them when hiring[4](https://danluu.com/hiring-lemons/#fn:O). If you believe the complaints about how hard it is to hire, you wouldn't think one company, let alone two, could spin up entire cloud teams in Seattle. Both companies solved the problem by paying substantially more than their competitors were offering for people with similar experience. Alibaba became known for such generous offers that when I was negotiating my offer from Microsoft, MS told me that they'd match an offer from any company except Alibaba. I believe Oracle and Alibaba have hired hundreds of engineers over the past few years.

Most companies don't need to hire anywhere near a hundreds of people; they can pay competitively without hiring so many developers that the entire market moves upwards, but they still refuse to do so, while complaining about how hard it is to hire.

(2), filtering out good potential employees, seems like the modern version of "no one ever got fired for hiring IBM". If you hire someone with a trendy background who's good at traditional coding interviews and they don't work out, who could blame you? And no one's going to notice all the people you missed out on. Like (1), this is something that almost everyone thinks they do well and they'll say things like "we'd have to lower our bar to hire more people, and no one wants that". But I've never worked at a place that doesn't filter out a lot of people who end up doing great work elsewhere. I've tried to get underrated programmers[5](https://danluu.com/hiring-lemons/#fn:U) hired at places I've worked, and I've literally never succeeded in getting one hired. Once, someone I failed to get hired managed to get a job at Google after something like four years being underemployed (and is a star there). That guy then got me hired at Google. Not hiring that guy didn't only cost them my brilliant friend, it eventually cost them me!

BTW, this illustrates a problem with Joel's idea that "great" devs never apply for jobs. There's often a long time period where a "great" dev has an extremely hard time getting hired, even through their network who knows that they're great, because they don't look like what people think "great" developers look like. Additionally, Google, which has heavily studied which hiring channels give good results, has found that referrals and internal recommendations don't actually generate much signal. While people will refer "great" devs, they'll also refer terrible ones. The referral bonus scheme that most companies set up skews incentives in a way that makes referrals worse than you might expect. Because of this and other problems, many companies don't weight referrals particularly heavily, and "great" developers still go through the normal hiring process, just like everyone else.

(3), needing a weird combination of skills, can be solved by hiring people with half or a third of the expertise you need and training people. People don't seem to need much convincing on this one, and I see this happen all the time.

(4), dysfunction [seems hard to fix](https://danluu.com/learning-to-program/#fixing-totally-broken-danluu-com-wat-situations). If I knew how to do that, I'd be manager.

As a dev, it seems to me that teams I know of that are actually good environments that pay well have no problems hiring, and that teams that have trouble hiring can pretty easily solve that problem. But I'm biased. I'm not a hiring manager. There's probably some hiring manager out there thinking: "every developer I know who complains that it's hard to find a good team has one of these four obvious problems; if only my problems were that easy to solve!"

Thanks to Leah Hanson, David Turner, Tim Abbott, Vaibhav Sagar, Victor Felder, Ezekiel Smithburg, Juliano Bortolozzo Solanho, Stephen Tu, Pierre-Yves Baccou, Jorge Montero, Alkin Kaz, Ben Kuhn, and Lindsey Kuper for comments and corrections.

If you liked this post, you'd probably enjoy [this other post on the bogosity of claims that there can't possibly be discrimination in tech hiring](http://danluu.com/tech-discrimination/).

* * *

1.  The folks who stayed describe an environment that's mostly missing mid-level people they'd want to work with. There are lifers who've been there forever and will be there until retirement, and there are new grads who land there at random. But, compared to their competitors, there are relatively few people people with 5-15 years of experience. The person I knew who lasted the longest stayed until the 8 year mark, but he started interviewing with an eye on leaving when he found out the other person on his team who was competent was interviewing; neither one wanted to be the only person on the team doing any work, so they raced to get out the door first. [\[return\]](https://danluu.com/hiring-lemons/#fnref:I)
2.  This section kinda makes it sound like I'm looking for work. I'm not looking for work, although I may end up forced into it if my partner takes a job outside of Seattle. [\[return\]](https://danluu.com/hiring-lemons/#fnref:L)
3.  [Moishe Lettvin has a talk I really like](https://www.youtube.com/watch?v=r8RxkpUvxK0), where he talks about a time when he was on a hiring committee and they rejected every candidate that came up, only to find that the "candidates" were actually anonymized versions of their own interviews!

    The bit about when he first started interviewing at Microsoft should sound familiar to MS folks. As is often the case, he got thrown into the interview with no warning and no preparation. He had no idea what to do and, as a result, wrote up interview feedback that wasn't great. "In classic Microsoft style", his manager forwarded the interview feedback to the entire team and said "don't do this". "In classic Microsoft style" is a quote from Moishe, but I've observed the same thing. I'd like to talk about how we have a tendency to do _extremely blameful_ postmortems and how that warps incentives, but that probably deserves its own post.

    Well, I'll tell one story, in remembrance of someone who recently left my former team for Google. Shortly after that guy joined, he was in the office on a weekend (a common occurrence on his team). A manager from another team pinged him on chat and asked him to sign off on some code from the other team. The new guy, wanting to be helpful, signed off on the code. On Monday, the new guy talked to his mentor and his mentor suggested that he not help out other teams like that. Later, there was an outage related to the code. In classic Microsoft style, the manager from the other team successfully pushed the blame for the outage from his team to the new guy.

    Note that this guy isn't included in my 3/7 stat because he joined shortly after I did, and I'm not trying to cherry pick a window with the highest possible attrition.

    [\[return\]](https://danluu.com/hiring-lemons/#fnref:H)
4.  For a while, Oracle claimed that the culture of the Seattle office is totally different from mainline-Oracle culture, but from what I've heard, they couldn't resist Oracle-ifying the Seattle group and that part of the pitch is no longer convincing. [\[return\]](https://danluu.com/hiring-lemons/#fnref:O)
5.  This footnote is a response to Ben Kuhn, who asked me, what types of devs are underrated and how would you find them? I think this group is diverse enough that there's no one easy way to find them. There are people like "Bob", who do critical work that's simply not noticed. There are also people who are just terrible at interviewing, like [Jeshua Smith](https://www.linkedin.com/in/jeshua-smith-1a873858). I believe he's only once gotten a performance review that wasn't excellent (that semester, his manager said he could only give out one top rating, and it wouldn't be fair to give it to only one of his two top performers, so he gave them both average ratings). In every place he's worked, he's been well known as someone who you can go to with hard problems or questions, and much higher ranking engineers often go to him for help. I tried to get him hired at two different companies I've worked at and he failed both interviews. He sucks at interviews. My understanding is that his interview performance almost kept him from getting his current job, but his references were so numerous and strong that his current company decided to take a chance on him anyway. But he only had those references because his old org has been disintegrating. His new company picked up _a lot_ of people from his old company, so there were many people at the new company that knew him. He can't get the time of day almost anywhere else. Another person I've tried and failed to get hired is someone I'll call Ashley, who got rejected in the recruiter screening phase at Google for not being technical enough, despite my internal recommendation that she was one of the strongest programmers I knew. But she came from a "nontraditional" background that didn't fit the recruiter's idea of what a programmer looked like, so that was that. Nontraditional is a funny term because it seems like most programmers have a "nontraditional" background, but you know what I mean.

    There's enough variety here that there isn't one way to find all of these people. Having a filtering process that's more like Matasano's and less like Google, Microsoft, Facebook, almost any YC startup you can name, etc., is probably a good start.

    [\[return\]](https://danluu.com/hiring-lemons/#fnref:U)