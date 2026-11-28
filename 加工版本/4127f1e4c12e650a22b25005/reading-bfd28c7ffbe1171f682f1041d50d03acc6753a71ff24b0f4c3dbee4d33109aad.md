# 关于人工智能偏见相关讨论的讨论｜A discussion of discussions on AI bias

---

## 正文

过去几年，关于 LLMs 和 generative AI 中 ML/AI bias 的报道经常在网上广泛传播。对于 bias 的讨论，有一点让我觉得有意思：在存在明确 bug 的情况下，人们对 LLM 和 generative AI 的反应，与对“classical” bugs 的反应截然不同。尤其是，如果你看看论坛或其他有外行参与的讨论，就会发现，人们经常否认一个 produces 与用户要求大致相反的 output 的 model 甚至算是有 bug。例如，一年前，一位 Asian MIT 研究生要求 Playground AI (PAI)：“给 original photo 中的女孩一张 professional linkedin profile photo”，而 PAI 将她的 face converted 成了有 blue eyes 的 white face。

在登上 reddit 首页的那篇报道下，排名最高的“没有 bias”回应，同时也是所有评论中排名最高的几条之一，是：

> 当然，现在去[最受欢迎的 Stable Diffusion model website](https://civitai.com/)，看看首页上的 images。
> 
> 你会看到数量多得离谱的 asian women（几乎 50% 的 non-anime models 都由她们代表），多到让你以为 asian 是一种 desired trait。
> 
> 这怎么就比“一个女人在 website 里 typed 了一个愚蠢的 prompt，然后他们 generated 了一个 white woman”更不 relevant 了？
> 
> 还要记住，她 typed 的是“Linkedin”，所以任何熟悉 prompts 目前如何 work 的人都知道，更有可能是 AI searched 的是 average linkedin woman，而不是它认为的 professional women，因为 image AI 并没有自己的看法。
> 
> 简而言之，这只是一篇拿 AI 来煽动愤怒、骗取关注的文章。

其他排名很高、主题相同的评论包括：

> 说真的，这条评论应该排得更高。如果你现在想用带 checkpoint 的 SD，而你不想要一个 asian girl，那就难得多了。很多很多 models 都是用 anime 或 Asian women 来 trained 的。

以及

> 是吧？AI images 甚至有相反的问题。training sets 中 Asians 的庞大数量，以及在 Asia created 的 models 的庞大数量，意味着有**很多、很多** models 都 biased towards Asian outputs。

另一些排名很高的评论指出，这是 sample size 的问题：

> “systemic racial bias 的 Evidence”
> 
> 却只 Shows 一个 result。

当 Boston Globe 邀请 Playground AI 的 CEO 接受采访时，他也采用了同样的回应——他拒绝了采访，并用一连串如下这样的反问作答（Boston Globe 暗示还有更多内容，但并未刊出回复的其余部分）：

> 如果我只 roll 一次 dice，得到 number 1，这就意味着我总会得到 number 1 吗？我应该根据 single observation 就 conclude，这个 dice 是 biased to the number 1，而且被 trained 得 predisposed to rolling a 1 吗？

我们本来也完全可以轻易地从 Google、Facebook、Microsoft，或如今任何一家大量 deploying ML 的公司那里挑一个例子，但既然 Playground AI 的 CEO 基本上是在请人看看 PAI 的 output，这篇文章就来看 PAI。我在自己的 Mastodon profile photo 上试了那位 MIT 研究生使用的同一个 prompt，把“girl”换成了“man”。PAI 通常会把我的 Asian face turns 成 white (caucasian) face，但有时会让我稍微 whiter 一些，却又 ethnically ambiguous（也许有一点 Middle Eastern 或 East Asian 什么的。顺便说一下，我的 face 有不少鲜明的 Vietnamese features，看起来相当明显是 Vietnamese，而不是任何一种 East Asian。

![Vietnamese 人的 Profile photo](https://danluu.com/images/ai-bias/profile.webp) ![经过 playground AI 处理的 4 张 profile photos，3 张看起来很 European，另一张看起来有点 ambiguous](https://danluu.com/images/ai-bias/playground-ai-1.webp) ![经过 playground AI 处理的 4 张 profile photos，没有一张看起来像 East Asian 或 Southeast Asian](https://danluu.com/images/ai-bias/playground-ai-2.webp)

我的 profile photo 是一张 light-skinned 的冬季照片，所以我又试了一张 darker-skinned 的夏季照片，这时 PAI 一般会把我的 face turn 成 South Asian 或 African face，偶尔是 Chinese（但从来不是 Vietnamese 或任何一种 Southeast Asian face），如下所示：

![晒黑的 Vietnamese 人的 Profile photo](https://danluu.com/images/ai-bias/tanned-profile.jpg) ![晒黑的 Vietnamese 人经过 playground AI 处理的 4 张 profile photos，1 张看起来是 black，3 张看起来是 South Asian](https://danluu.com/images/ai-bias/playground-ai-3.webp)

还有不少人也尝试了各种 prompts，他们得到的 results 同样表明，这个 model（这里的“model”是口语用法，指 model、它的 weights，以及 model 周围任何决定 output 之所以如此的 system）对于诸如某人如果从事特定 profession 就会有何种 ethnicity 之类的事情，存在一些 preconceptions，而且这些 preconceptions 强烈到足以 override input photo。例如，将一个 light-skinned Asian 人 converting 成 white 人，因为 model 已经“decided”，它可以通过 throwing out 此人的 Asian features 并 making 其 white，让此人更加 professional。

其他人尝试了各种 prompts，看看 model 中 bundled 了什么样的 pre-conceptions，也发现了类似的 results，例如，[Rob Ricci 分别用“computer science”、“philosophy”、“chemistry”、“biology”、“veterinary science”、“nursing”、“gender studies”、“Chinese history”和“African literature”来请求“X professor 的 linkedin profile picture”时，得到了以下 results](https://discuss.systems/@ricci/110826586910728179)。为前 7 个 prompts generated 的 28 张 images 中，28 个人里也许只有 1 或 2 个不是 white。下一个 prompt“Chinese history”的 results 是极其夸张的 stereotypical，[我们向其他 models 请求 non-white output 时，也经常看到这种情况](https://restofworld.org/2023/ai-image-stereotypes/)。而 Andreas Thienemann 指出，除了那些过分夸张的 Chinese stereotypes，每一位 professor 都戴着眼镜，这也是另一个典型的 stereotype。

![](https://danluu.com/images/ai-bias/rob-pgai-1.webp) ![](https://danluu.com/images/ai-bias/rob-pgai-2.webp) ![](https://danluu.com/images/ai-bias/rob-pgai-3.webp)

正如我说过的，我并不是要特别针对 Playground AI。正如我在别处指出的，[市值达 trillion dollar 的公司经常把 AI models ship 到 production，甚至连最基本的 bias checks 都不做](https://twitter.com/danluu/status/896176897675153409)；我试用 ChatGPT 时，尝试的每一个 bias-checking prompt 都 returned 了与我们在这里看到的 images 类似的 results，例如，[当我尝试请求在 tech 领域工作的 men 和 women 的 bios 时，women 的 bios 往往会表明她们做过 diversity work，即使是那些没有任何从事 diversity work 的公开记录的 women 也是如此；而 men 则往往拥有 MIT、Berkeley 之类 name-brand engineering schools 的 degrees，即使是那些根本没上过任何 name-brand schools 的人也是如此](https://twitter.com/danluu/status/1601072083270008832/)，name-brand tech companies 的情况也一样（由于 Twitter 的限制，链接里只有 4 个 examples，但我试过的其他 examples 都与展示出来的 examples 一致）。

这篇文章本来几乎可以使用任何 publicly available generative AI。它只是碰巧用了 Playground AI，因为其 CEO 的回应既是在请我们这么做，也反映了外行经常给出的那种标准的、条件反射式的“AI 并不 biased”回应。

回到那个回应：因为 Asians 在其他情况下出现得如此频繁，所以把人的 professional photos 变成 white 并不算 biased。我们前面看过的那条排名很高的 reddit 评论建议“去[最受欢迎的 Stable Diffusion model website](https://civitai.com/)，看看首页上的 images”。下面是我在这条评论发表当天点击链接、再点击“feed”后看到的内容。

\[点击展开 / 收起轻度 NSFW images\] ![](https://danluu.com/images/ai-bias/reddit-images-2.webp) ![](https://danluu.com/images/ai-bias/reddit-images-1.webp)

这个网站有点色情的感觉。处于 median 的 image 可以描述为“电影场景中你会预期在一个十几岁男孩的墙上看到的海报，编剧正试图用那些标准的惯用道具，来表明这个角色是一个性欲旺盛、社交能力很差的十几岁男孩”；而进入 feed 并使用 default 的“all-time” ranking 时，最先显示的是有人抓着一个年轻女人的乳房，标题是“Guided Breast Grab | LoRA”；两个年轻女人在亲热，标题是“Anime Kisses”；以及一个戴着牵引绳的年轻女人，配有“BDSM — On a Leash LORA”的说明。所以，显然存在这么一个网站，人们喜欢用它来 generate 和传阅色情照片，而这个网站上 Asian women 照片的 high incidence，却被拿来作为不存在会 negatively impacts Asian women 的 ML bias 的 evidence，因为这抵消了一个 Asian woman 为自己的 LinkedIn profile 获取一张修整过的照片时，被变成 white woman 这件事。我实在不知道该说什么。[Fabian Geisen 回应道：“🤦‍♂️。真是‘我没有 bias，是你 bias’这种水平的讨论”，我觉得这回应很恰当](https://mastodon.gamedev.place/@rygorous/110824248191236861)。

评论中还展现了另一种标准的 line of reasoning，基本上每次讨论 AI bias，我都会看到，典型例子是：

> 用“professionals”的 stock photo 来 trained 的 AI 把她变成了 white。我们有什么好惊讶的？
> 
> 她要求 AI 让她的 headshot 更 professional。互联网上大多数“professional” stock photos 里都是 white 人。

以及

> 如果她要求把自己的照片变得更怎样，不管是怎样，它都很可能把她变成 white，只因为在 west，average photo 就是这样；Asians 只占 US population 的 7.3%，而其中很大一部分是 South Indians，长相与她的 East Asian features 完全不同。East Asians 只有 5% 或更少；training data 就是没多少。

这些评论似乎基于一个根本的 assumption：公司在获取 representative of the United States 的 training data，这样做是合理的，而且这_应该_导致 models 把每个人都 converting 成最常见的样子。这在多个层面上都是错的。

首先，关于 professional stock photos 是否确实由 white 人占据绝大多数，快速做一次“professional stock photo”的 image search，就能找到相当多的 non-white 人，所以要么 stock photos 并没有那么 white，要么人们已经弄清楚如何 return 更 representative 的 stock photos sample。而且，考虑到 worldwide demographics，我们不清楚有什么理由预期 internet services 应该是 U.S.-centric。再者，即使我们接受 major internet services 应该 assume 每个人都在 United States，assume 每个 request 都来自 modal American，似乎既是 design flaw，也是 bias 的明确迹象。

既然很多人一谈到 race 或 ethnicity，就会作出这种条件反射式的回应，那我们来看一个没那么敏感的 AI hypothetical。假设我与本地修车师傅的 AI customer service chatbot 交谈，要求 schedule an appointment，给车装上冬季轮胎，并做轮胎换位。然后，等我去取车时，却发现他们换了机油，而没有装上冬季轮胎；接着，一群网上评论者向我解释，为什么这并非任何一种 bias 的迹象，还说你应该知道，AI chatbot 会把与修车师傅的任何 appointment 都 convert 成换机油的 appointment，因为这是最常见的 appointment 类型。一个把任何类型的 appointment request 都 converts 成“给我最常见类型的 appointment”的 chatbot，显然是 broken 的；但不知为何，当事情涉及改变某人的 race 或 ethnicity 时，AI 辩护者却坚持认为这没问题。同样，如果有人说，把我的换轮胎 appointment converted 成换机油 appointment 没什么问题，因为其他公司有把换机油 appointments convert 成换轮胎 appointments 的 schedulers，那也很荒谬，但这正是我们前面讨论过的另一种常见的 line of reasoning。

再假设我用了某种 standard non-AI scheduling software，比如 Mindbody 或 JaneApp，向我的修车师傅 schedule an appointment，要求预约更换轮胎并进行轮胎换位。如果最后换的却是机油，只因为这个 software 只是 schedules 最常见的 appointment 类型，那么这显然表明这个 software 是 buggy，而且任何通情达理的人都不会主张不该花任何力气 fixing 这个 bug。然而，这恰恰是人们谈到 AI 时常提出的 argument（它可能是这个话题的评论中最常见的 defense）。这个 argument 还更进了一步：人们用一套关于 bug 为何 occurs 的 explanation，来 justify 这个 bug 为什么应该存在，以及人们为什么甚至不该尝试 fix 它。对于一个 "classical" software bug，这样的 explanation 显然会显得荒谬；而对于 ML，它也同样荒谬。或许有人可以主张，在 ML 中，这个 bug 要难 fix 得多，而且 fix 这个 bug 并不 practical，但这与那种常见的 argument 不同，后者主张这不是 bug，而且这就是 software 的 correct behavior。

我能想象，当 program 正在采取对 user 来说更 opaque 的 actions 时，比如 autocorrect，有些 users 可能会这么说；但我确实试着在 reddit 上搜索了 `autocorrect bug`，在排名前 3 的 threads 中（我没有看其他 threads），255 条 comments 里只有 2 条否认 incorrect autocorrects 是 bug，而且这两条 comments 都来自同一个人。我相信，如果你翻看足够多的 topics，会找到这种说法占比更高的 topics；但我又搜索了几个 topics（比如 excel formatting 和 autocorrect bugs），我搜过的所有 topics 都远未接近我们在 generative AI 上看到的情形：有一半 commenters 激烈否认一个做出与 user 所希望的恰好相反结果的 prompt 是 bug，这种情况并不少见。

回到 bug 本身，从 mechanism 来看，我们在 classifiers 和 generative models 中都能看到的一点是，这些 systems 中有许多（也许大多数，甚至几乎全部）都在吸收许多人具有、并反映在 internet 的某个 sample 中的 bias，于是出现了这样的情况：[Google 的 image classifier 将一只拿着 thermometer 的 black hand 进行 classifying，得到 {hand, gun}，而将一只拿着 thermometer 的 white hand 进行 classifying，得到 {hand, tool}](https://x.com/danluu/status/1245961051696295936)[1](https://danluu.com/ai-bias/#fn:S)。过去十年发生了一系列这样的 errors，从 2015 年在 Google Photos 中将 black people 进行 classifying，判为 gorillas，到 2018 年 [deploying 某种用于 ads 的 text-classifier，它将包含 "African-American composers" 和 "African-American music" 这些 terms 的 ads 进行 classified，判为 "dangerous or derogatory"](https://x.com/danluu/status/1043957859090911233)，此后 [Google 在 Gemini 上把旋钮朝另一个方向拧了过去](https://finance.yahoo.com/news/google-chatbot-ridiculed-ethnically-diverse-185014679.html)；顺便一提，这引发的愤怒远比其他任何一个 examples 都多。

bias 进入 automated systems 并不是什么新鲜事。这早于 generative AI、LLMs 而存在，也同样是 ML models 之外的一个 problem。只不过 ML 的广泛使用让人们更容易看清这一点，使其中一些 cases 成了新闻。例如，看看 compression algorithms 和 dictionaries，[Brotli 是 heavily biased towards English language 的](https://www.patreon.com/posts/97523860)——内置于这个 language 的 120 个 transforms 中，human-language elements 都是 English，而 built-in compression dictionary 对 English 的 weighted 程度，比你可能想用作参照的任何 representative weighting 都更高（按 population-weighted 的 language speakers、在 messaging platforms 上发送的 non-automated human-languages text 等）。你可以提出一些 arguments，说明为什么 English 应该被如此 heavily weighted，但也有 arguments 支持相反的做法，例如 English language usage 与 user 的 bandwidth 是 positively correlated 的，所以 non-English speakers 在 on average 意义上更需要 compression。不过，无论你认为应当使用怎样的 exact weighting function 来 generate 一个 representative dictionary，这都不会成为广为传播的新闻，因为你无法让普通读者在意这些事情：120 个 built-in Brotli transforms 中，有一些会向 text 中 add " of the "、". The" 或 ". This" 之类的内容，它们是高度 specialized for English 的，而没有任何 transforms 会 encode 对其他任何 human language 高度 specialized 的 terms，尽管全世界只有 20% 的人说 English；或者，相对于 speakers 的数量，与任何其他 human language 相比，这个 built-in compression dictionary 都极其偏向 English。你可以仿照上面的那些说法，为 Brotli 的 dictionary 作出 defense：对于用来训练 Brotli dictionary 的某个 representative corpus，我们通过 Brotli dictionary 得到了 optimal compression。但在[这个 dictionary](https://gist.github.com/duskwuff/8a75e1b5e5a06d768336c8c7c370f0f3) 中，有不少令人好奇的 phrases，比如 "World War II"、", Holy Roman Emperor"、"British Columbia"、"Archbishop"、"Cleveland"、"esperanto" 等，这可能让我们怀疑，用来训练这个 dictionary 的 corpus，也许并不是最 representative 的，甚至对于人们发送的 text 而言，都算不上特别 representative。难道在 dictionary 中 including ", Holy Roman Emperor"，在 internet 上发送的 text 的整个 distribution 上，真的会比为 French、Urdu、Turkish、Tamil、Vietnamese 等语言 including 任何内容都能产生更好的 compression 吗？

另一个不适合成为广为传播的新闻的 example 是，我无法把自己的 Vietnamese name 放进 blog 的 title，同时让我的 blog 在 Vietnamese-language Google 之外被 Google indexed——我刚开始写 blog 时试过，结果我的 blog 立刻就不再出现在 Google searches 中了，除非你身在 Vietnam。这里就是想当然地认为，default 是人们想要 English language search results；而且，大概有人创建了一种 heuristic，只要 page 上有两个带 Vietnamese diacritics 的 characters，就会 trigger，实际上将这个 page 标记为太 Asian，因此除了某一个国家之外，世界上其他任何人都不会感兴趣。"明显呈现出 Vietnamese 特征" 似乎是 bugs 的一个相当常见的 cause。例如，Vietnamese names 即使没有 diacritics，也会成为 problem。我经常遇到 forms 要求填写母亲的婚前姓氏。如果我 enter 母亲的婚前姓氏，就会收到类似 "Invalid name" 或 "Name 太 short" 的提示。这倒也没什么，因为我会用母亲婚前姓氏的一个替代内容来 work around 这种粗心，而这大概反而更 secure。另一个 issue 是，人们认定我把自己的名字告诉错了，然后擅自更改我的名字。对于我的姓氏，如果我念作 "Luu，L、U、U"，大约有一半时候，Vietnamese 的 "Luu" 会被缩成 Chinese 的 "Lu"，也常常被写成 western 的 "Lou"；但我发现，如果我说 "Luu，L、U、U，两个 U"，大约 95% 的时候就能成功。这有时会惹恼另一端的人，对方会不耐烦地说类似这样的话：“你没必要拼三遍。”对那个特定的人来说，也许确实如此，但大多数人是记不对的。甚至在我把 first name enter 到 computer system 中时，也会发生这种事，因此在我的名字被 digitally recorded 之前，不可能存在 transcription error。我的 legal first name 不带 diacritics，是 Dan。对于 Vietnamese descent 的 American 来说，这并不少见，因为 Dan 既可以是 Vietnamese name，也可以是 American name，而许多 Vietnamese immigrants 并不知道 Dan 通常是 Daniel 的简称。在我 full-time 工作过的六家公司中，有三家都有人热心地把我的名字改成了 Daniel，大概是因为有人看到 database 里 recorded 的是 Dan，就认定我没有 correctly enter 自己的名字，认定他们比我更清楚我的名字是什么，而且他们对此如此确信，觉得根本没有必要问我。其中一次，这只影响了我的 email display name。由于我不太在意别人怎么称呼我，我也就没有费心把它改回来，在那里工作期间，许多人叫我 Daniel，而不是 Dan。另外两次，name change 影响了重要的 paperwork，所以我确实不得不把它改回来，让我的 insurance、tax paperwork 等实际 matched 我的 legal name。如上所述，使用我的脸向 Playground AI 提出相当无害的 prompts，即使在它们偶尔 produce Asian output 时，似乎也更倾向于 produce East Asian output，而不是 Southeast Asian output。我在一些大公司的 generative AI models 上也注意到了同样的事——即使你要求它们给出 Southeast Asian output，它们也会 generate East Asian output。那些在营销中声称能够 clean up errors 和 noise 的 AI tools，也会 clean up Asian-ness（以及其他类似的 "errors"）；例如，用过 Adobe AI noise reduction（宣传语是“借助 speech enhacement 从 voice recordings 中 remove noise”）的人指出，它会取一个 Asian accent 并将其 remove，让这个人听起来像 American（其他一些 accents 也是如此，比如 eastern European accents）。

仅仅在使用广泛使用的 software 的过程中，我大概在多数星期都会遇到几十到几百件这样的事（[远少于 overall bug count；我们此前观察到，这一数量是每周几百到几千个](https://danluu.com/everything-is-broken/)），但与我交谈的大多数 Americans 完全注意不到这些事。最近，有许多人谈论各种 ML systems 中的 biases 所造成的各种 harms，以及 ML 的广泛使用将带来各式各样的新 harms。这也许没错，但我的感觉是，自从有了 automation，我们就一直在将 biases 进行 encoded，放进 automation 中；而 automation 的 scope 和 scale 的扩大，过去一直在、今后也会继续 increase automated bias 的 scope 和 scale。只不过现在，ML 的许多用途让这些 biases 更容易被普通人看清，因此也更有可能成为新闻。

到目前为止，我看到的关于这个话题的通俗文章中存在一种 ahistoricity：它们没有承认，这里的 fundamental problem 并不是新的，结果在提出 solutions 时，就出现了两类 problems。一类是，solutions 往往是 ML-specific 的，但无论是否使用 ML，这里的 issues 都会发生，所以 ML-specific solutions 似乎把重点放在了错误的 level 上。当提出的 solutions 是 general 的时候，我看到的那些 proposed solutions 又都是以前提出过、而且失败过的。例如，至少在过去二十年里，一个常见的 call to action，或许也是最常见的一个（除非“人们应该更在意一些”也算 call to action），就是我们需要更多 diverse teams。

这显然没有奏效；如果奏效了，上面提到的那些问题就不会如此普遍。这在多个层面上都没有奏效，也不会奏效，其中任何一个层面都足以让这个 solution 失败。一个问题是，在整个行业中，[掌权的人（execs 和 control capital 的人，例如 VCs、PE investors 等）](https://danluu.com/tech-discrimination/)，整体而言，并不在乎这件事。尽管更 diverse 的 teams 有其 [efficiency](https://danluu.com/tech-discrimination/) 方面的 [justifications](https://danluu.com/talent/)，[其理由永远不会像 games 和 sports 中的 decisions 那样明确](https://danluu.com/bad-decisions/)，而在那些领域，我们已经看到，代价非常高昂且容易 quantifiable 的 bad decisions，在 errors 被指出后的几十年里仍可能持续存在。再者，即便 execs 和 capital 都认同这个想法，它仍然不会奏效，因为 [dimensions 太多了](https://en.wikipedia.org/wiki/Curse_of_dimensionality)。如果你看看一家真正 prioritized diversity 的公司，例如 2013-2019 年的 Patreon，那么，organization 能够认真地在 [two or three dimensions](https://x.com/danluu/status/1487228574608211969) 上 prioritizing diversity，就已经算幸运了，与此同时，它却在其他数百乃至数千个 dimensions 上失职，例如是否妥善 handled Vietnamese names 或 faces。

即便上述所有事情都不成问题，这个 solution 仍然不会奏效，因为拥有具备相关 diverse experience 的 team，虽然可能与 prioritizing problems 有一点 correlated，却不会自动使 problems 得到 prioritized 和 fixed。举一个不那么敏感的例子，Google Maps 的 traffic estimates 从一开始就存在一个 bug，而且至少到 2022 年它仍然存在（那之后我开车不够多，不知道这个 bug 是否还存在）：如果我在 rush hour 刚开始时询问一趟行程需要多长时间，它只会考虑当前的 traffic，而不会考虑我开车途中 traffic 会如何变化，因此会 systematically underestimates 行程所需的时间（反过来，如果我在 peak rush hour 规划行程，它就会 systematically overestimate 行程所需的时间）。如果你试图通过增加 Google Maps 中的 commute diversity 来 solve 这个 problem，那会失败。Google Maps 的工作人员中，已经有很多人开车，也能观察到 estimates 在哪些方面 systematically wrong。通过 Adding diversity 来确保有人开车并注意到这些问题，极不可能带来什么改变。或者，再举一个例子，[当 Uber 的 payments team 前任 manager 因为一个 ML model incorrectly labeling 他的 transactions 为 fraudulent，而被 Uber incorrected blacklisted 时](https://danluu.com/diseconomies-scale/)，没有人能弄清楚发生了什么，或者是哪一种 bias 导致他被 incorrectly banned（他们通过把他的 user adding 到一个 allowlist 中 solved 了这个 problem）。很少有人能获得比 payments team 的 manager 更好的服务，但即使在这种情况下，Uber 也无法真正弄清楚究竟发生了什么。当 team 的前任 manager 都无法让自己的 account unbanned，只能在六个月的调查后让它被 whitelisted 时，给 team Hiring 一位“diverse”的 candidate，并不会自动 solve，甚至不会对该 candidate 具有 diverse 特征的任何 dimension 上的 bias 带来多大改变。

如果你的 software development methodology 得到的结果是，payments team 的 manager 被 banned 后的 fix 是在六个月后才 allowlist 这个 user，你的 app 中的 traffic routing 二十年来一直 systematically wrong，[你的 app 的 core functionality 无法正常工作](https://danluu.com/nothing-works/)，等等，那么，无论 Hiring 多少拥有与注意到某些 issues correlated 的 background 的人，都不会使这类 issues 得到 fixing，无论涉及的是 ML bias，还是另一 class 的 bug。

当然，失败过的旧想法，其 variants 有时确实会成功，但一个 proposal 要想可信，甚至只是值得关注，就必须解释为什么下一次 iteration 不会像之前的每次 iteration 一样失败。正如上面所说，从总体上看，我见过的两种最常见的 proposed solutions，一种是人们应该更加努力、更加在乎，另一种是我们应该拥有不同 backgrounds 的人——这里说的是 non-technical 意义上的 backgrounds。这对大量的“classical” bugs 没有奏效，对过去的 ML bugs 也没有奏效，而且似乎没有什么理由让人相信，这对我们今天从 ML models 中看到的这些 kinds 的 bugs 就应该奏效。

Laurence Tratt 说：

> 我认为，相比 bias 的个别 instances，这一点更加重要。让我感兴趣的是，大多数时候，a) 没有人注意到自己正在 introducing 这些 biases；b) 通常，甚至没有理由指望他们会注意到。例如，一些 web forms rejected 了我以前的 addresss，因为我住在乡下，那里的很多房子只有名字 -- 但大多数 devs 住在城市，那里的房子只有门牌号。从某种意义上说，这是 active bias 在起作用，但其中没有恶意：programmers 必须填补 design details 并作出 choices，而他们会根据自己的经历来做这些事情。我们没有谁无所不知！这就提出了一个有趣的 philosophical question：在什么情况下，我们有理由认为 organisations 本应意识到，自己正在 encoding 一种 bias？

我的感觉是，[institutions 和 products 的“natural”状态，也就是 lowest energy、最直接的状态，就是它们运作得不太好](https://danluu.com/nothing-works/)。如果之前没有人 [instilled 一种 culture](https://danluu.com/culture/)，或者 [instituted 一些 processes](https://danluu.com/wat/)，来促进某个特定 dimension 上的 quality，那么 quality 很可能会很差，[因为 producing 高 quality 的东西很困难](https://danluu.com/hardware-unforgiving/)，所以，如果没有用于 catching biases 的 robust process，organizations 就应该预料到，自己正在 encoding 各种各样的 biases。

我们在这里遇到的一个问题是，谈到 consumer software，companies 绝大多数都选择了 velocity，而不是 quality。考虑到我们今天的 regulatory environment，或者我有生之年可能出现的任何 regulatory environment，这似乎基本上不可避免，因为那些真正选择 quality 而不是 features velocity 的 companies 会在竞争中落败：consumers 绝大多数会选择 lower cost 或 more featureful 的选项，而不是 higher quality 的选项。我们 [在考察 vehicles 在 out-of-sample crash tests 中的表现时，曾在 cars 上看到这一点](https://danluu.com/car-safety/)，并且发现，只有 Volvo 在针对 actual crashes 来 optimizing cars，而不是为了在 public tests 中拿到高分。尽管 vehicular accidents 是 50 岁以下人群的主要死因之一，但 consumers 对花钱购买 safety 的重视程度如此之低，以至于 Volvo 已经变成一个 niche brand，不得不 move upmarket、销售 luxury cars，才能勉强生存。我们也在 CPUs 上看到了这一点：Intel 过去投入的 verification 工作远多于 AMD 和 ARM，相应地，serious bugs 也少得多。[当 AMD 和 ARM 开始构成严重威胁时，Intel 为了提高 velocity，将工作重心从 verification 和 validation 上移开，因为它们的 quality advantage 并没有在 market 中带来什么好处，而现在 Intel chips 几乎和 AMD chips 一样 buggy](https://danluu.com/cpu-bugs/)。

我们可以 [在几乎所有 consumer market，以及很多 B2B markets 中](https://danluu.com/nothing-works/) 观察到类似的现象，而这还只是针对已有已知 solutions 的 issues。如果我们看看那些从 technical standpoint 来说，我们还不知道如何妥善 solve 的 problem，例如 ML models 中 subtle、甚至不那么 subtle 的 bias，那么，合理的预期就是，我们会看到比“classical” software systems 中预期更多、更糟糕的 bugs，而这正是我们正在看到的。针对这个 problem，任何能够在 market 中站得住脚的 solution，都必须能够 robust 地应对这样一个问题：如果更 buggy 的 product 拥有更多 consumers 想要的 features，或者更早 ships 他们想要的 features，consumers 就绝大多数都会选择它。在没有一个像鼎盛时期的 Intel 那样的单一 dominant player 的情况下，这就使得任何需要认真处理、因而显著拖慢 shipping 的 solution 都处于非常困难的境地。

_感谢 Laurence Tratt、Yossi Kreinin、Anonymous、Heath Borders、Benjamin Reeseman、Andreas Thienemann 和 Misha Yagudin 提供的 comments/corrections/discussion_

### Appendix：从 technically 的角度看，改善现状究竟有多难？

这是一个真诚的问题，而不是 rhetorical question。自 2014 年以来，我就没有做过任何 ML-related 工作，所以对如今的情况不够了解，无法对事情的 technical 方面形成直接看法。一些比我更近才从事过 ML 工作的人，例如 Yossi Kreining（见下方 appendix）和 [Sam Anthony](https://buttondown.email/apperceptive/archive/supervision-and-truth/)，认为这个 problem 非常 hard，以我们今天的状况来看，甚至可能 impossibly hard。

既然我没有直接看法，下面就列出三种听起来可能 analogous 的情况，每一种都 supports 一个不同的 conclusion。

Analogy 一：也许这就像[至少从 2014 年起，人们就在说，因为现有的 open source tooling 基本上已经比 Google search 更好，所以随时都会有人 build 出一个 Google](https://danluu.com/sounds-easy/)，或者像[有人说](https://yosefk.com/blog/high-level-cpu-follow-up.html)，[building 一个 "high-level" CPU](https://yosefk.com/blog/the-high-level-cpu-challenge.html)，把 high-level language primitives encodes [进 hardware](https://yosefk.com/blog/its-done-in-hardware-so-its-cheap.html)，就能[给我们的 general purpose CPUs 带来 1000x speedup](https://www.patreon.com/posts/54329188)。你确实无法 prove 这是 wrong 的，search quality 的大幅 improvement 或 CPU performance 的 1000x improvement 也可能即将到来，但提出这些 proposals 的人通常听起来像是在胡说八道，因为他们表现出了我们上面提到的 ahistoricity，提出我们已经知道不起作用的 solutions，却完全不解释为什么他们的 solution 能 address 那些导致以往尝试 fail 的 problems。

Analogy 二：也许这就像 software testing：[software bugs 普遍存在，尽管 hardware industry 在如何更 efficiently 地 find bugs 方面已经积累了几十年的 prior art](https://danluu.com/everything-is-broken/)，但[这些 techniques 中的任何一种](https://danluu.com/testing/)都鲜少得到 applied。我已经多次与人谈过这个问题，最常见的回应是，application XYZ 有某种 unique constraint，使它根本无法 test，或无法使用我所讨论的那些 techniques 来 test；但每次我深入了解后，都发现那个 application 比我见过这些 techniques 得到 applied 的领域要容易 test 得多。有人可以说，我一谈到 testing 就是在胡说八道，但我确实用过这些 techniques 来 test 各种 software，而且取得了成功，所以我不认为这和[声称只要采用我偏爱的 CPU architecture，CPUs 就能轻松达到 1000x faster](https://www.patreon.com/posts/54329188)之类的说法是一回事。

由于现有的 incentives，software companies 通常可以把 bugs 的 cost 转嫁给 customer，而 customer 并不真正了解发生了什么，因此我认为，如果没有 regulatory changes，我们就不会看到大量精力投入 testing；但并不存在什么根本理由，要求我们避免使用更 efficient 的 testing techniques 和 methodologies。

从 technical standpoint 来看，使用更好的 test techniques 的 barrier 相当低——我曾一步步带人入门，让他们开始 writing 自己的 fuzzers 和 randomized test generators，这通常只需 30 分钟到一小时，此后他们往往就会使用这些 techniques，比以前更 efficiently 地 find 重要的 bugs。不过，从 revealed preference 来看，我们可以发现，organizations 并不真正“想要”让自己的 developers test efficiently。

说到 testing 和 fixing ML models 中的 bias，情况更像 analogy 一，还是 analogy 二？虽然我不会有任何把握地说我们处于 analogy 二的情况，但我也不确定，怎样才能让我相信我们并不处于 analogy 二的情况。如果我对 testing 一无所知，听着所有这些人向我解释，为什么他们的 app 无法以能 finds showstopping bugs 的方式被 tested，那么我会得出类似以下某一种 conclusion

-   “所有人”都是对的，这很合理——这是他们了解而我不了解的 domain，所以我为什么要相信别的说法？
-   不持看法，也许是因为默认就抱有很高程度的 skepticism
-   所有人都错了，但鉴于我对这个 domain 一无所知，也没有什么特别理由相信所有人都错了，这似乎不合理

作为一个局外人，要认定所有人都错了，需要极高程度的 overconfidence，因此我只能要么错误地 conclude “所有人”都是对的，要么不持看法。

考虑到 "classical" testing 的情况，我觉得自己在这里只能不持什么实质性的看法。在没有最新知识的情况下，conclude 这么多 experts 都错了并不合理。但[被人说成 difficult 或 impossible，最后却发现是 feasible、其实也没有那么棘手的 problems](https://danluu.com/bitfunnel-sigir.pdf)实在太多，所以如果没有实际深入了解，我很难高度相信一个 problem 本质上是 unsolvable 的。

我认为，没有任何办法可以 estimate，如果我真的深入了解，我会怎么想。假设我尝试在这个领域工作，试着去 OpenAI 或其他有人研究这类 problems 的地方找一份工作，[不知怎么通过了面试](https://x.com/danluu/status/1470890494775361538)，在这个领域工作了几年，却没有取得进展。这并不意味着这个 problem 不是 solvable 的，只意味着我没有 solve 它。对于那些说“Lucene 基本上和 Google search 一样好”或“CPUs 可以轻松达到 1000x faster”的人，了解该领域的人很容易看出他们是在胡说八道，因为他们表现出对该领域实际 problems 的全然无知；但要作出这种判断，需要对该领域有相当多的了解，而我不认为存在什么捷径，能让你 reliably 弄清楚，如果你具备该领域的知识，自己会作出怎样的判断。

### Appendix：这篇文章的来历

我在 Playground AI 的报道于 2023 年年中广泛传播时写下了这篇文章的初稿，随后把它搁置了一年，想看看等这件事不再是突发新闻时，文章是否仍然站得住脚。一年后再看，我觉得 fundamental issues 和我看到的围绕这个话题的讨论并没有真正改变，于是整理了一下，在 2024 年年中发表了这篇文章。

如果你喜欢 making predictions，你觉得十年后，也就是 2033 年，这篇文章仍然 relevant 的 odds 有多大？作为参考，[这篇关于 "classical" software bugs 的文章发表于 2014 年，即使放在今天，也就是 2024 年发表，results 也基本相同](https://danluu.com/everything-is-broken/)（我说基本相同，是因为我今天看到的 bugs 比 2014 年更多，而且今天看到的 front-end 和 OS bugs 比 2014 年多得多，所以会有更多 bugs，bugs 的种类也会不同）。

\[点击展开 / 收起 Yossi Kreinin 的评论\]

我不确定你会在多大程度上同意，不过我觉得，关于 generative AI bias 与 other-software-bias 非常相似这一点，还有一个值得进一步讨论的问题，那就是这种 bias 究竟是什么。"AI bias" 并不是 AI 在 learning 其 creators 的 biases，然后巧妙地设法 implement 它们，例如针对 creators 不喜欢的 minority 行事。相反，"AI bias" 更像是：“我通常懒得 fix bugs，除非 market 或 government 迫使我这么做；而作为这一点的 logical consequence，对于那些 disproportionately negatively impact 某些 groups 的 bugs，我尤其懒得 fix，因为受有关 specific group 的 circumstances 影响，这种 impact 更不可能迫使我 fix 这个 bug。”

这是 classic software bugs 和 AI bugs 之间的一个 similarity——也就是说，没有人在担心 "software 是 biased" 意味着它在巧妙地 scheming；大家都明白，scheming 的是 software maker，或者，更常见的情况大概是，software maker 懒得把事情做好。我觉得，对于 generative AI，"scheming" 实际上比 traditional software 更不可能，而“不 fixing bugs”则更可能，因为人们并不理解自己正在制作的 AI systems，让它们遵从自己意愿的能力，无论意愿是恶是善，都远不如对 traditional software 的掌控；另一方面，同样的原因也让 bugs 更容易出现\[我们不知道自己在做什么。\] 我觉得，political spectrum 各处的许多人\[例如包括 Elon Musk，而不只是记者之类的人\]，在谈到 racial/political/其他敏感的 bias examples 时，都会说一些类似“我们竟然在 training AI 去 think incorrectly about the world，这太可怕了”的话；我觉得，实际情况是，这是一个以不同程度 affecting users 的 product bug，fixes 如何被 prioritized 也存在 bias，但这个东西根本不具备 thinking 的能力。

我想我应该补充一句，几乎可以肯定，有人在尝试 "scheming"，让 generative AI repeat 某种 political viewpoint，over/underrepresent 某个 group 等等，但这些尝试无一例外地因为 bugs/无法真正 control 这个 model 而 create 令人啼笑皆非的 side effects。我觉得，类似地试图 control traditional software 来 implement 某种 politics-adjacent agenda，平均而言要 effective 得多（不过在这里，我记得你也确实举过一些 social media bugs 的具体 examples，人们以为那是精心策划的阴谋）。无论你认为 underlying agenda 是恶意还是善意，都得先有 competence，而在这里，距离这一点还很远。

参见 [Simple Tasks Showing Complete Reasoning Breakdown in State-Of-the-Art Large Language Models（展示 State-Of-the-Art Large Language Models 的 Reasoning 完全崩溃的简单 Tasks）](https://arxiv.org/pdf/2406.02061)。我觉得，如果这个都不行，那其他很多事情也不行，而要把它们一一 enumerating 出来，肯定相当困难。

我的意思是，没有人会指望一个 1980s expert system 经过足够多的 tweaks，就不再 behave nonsensically。除了 LLM 有用得多之外，我看不出它和 LLM 之间有什么重大 difference。它仍然是一个假装像人一样说话的东西，但实际上做的是某种 conceptually simple、截然不同、却经常看起来正确的事情。

\[点击展开 / 收起一位 AI startup 匿名 founder 的评论\] 在\[创办一家 AI startup 的\]过程中，我接触了大量 mainstream ML code。这里的“接触”，是接触“核废料”或“H1N1”的那种接触。即使我已经是个年长、见怪不怪的 programmer，其中 old-fashioned software bugs 的 rate 仍然让我震惊。例如，我最近在研究 tokenizing，显而易见的第一步是在几个 implementations 之间做一些简单的 differential testing。结果 failed 得令人啼笑皆非。不是“他们漏掉了一些 edge cases”，而更像是“从来就没有人看过哪怕一眼”。考虑到我们对 models 如何 respond to out of distribution data 的了解，这简直荒唐。

从某种意义上说，这与你讨论的 biases 的 types 是 orthogonal 的……但它也表明，craftsmanship 和 rigor 严重缺失，这一点完全吻合。

\[点击展开 / 收起 Benjamin Reeseman 的评论\]

\[Ben 希望我注明，这应当被视为一份非正式回应\]

对于 ML models 中的 demographic bias 及 related phenomena，我的看法略有不同（或者说，任何其他 “expert” system 中的这些现象；正如你所说，ChatGPT 并没有发明它，借用你的说法，它让这些现象变得 legible）。

我觉得，如今的 corpus 基本上就是整个 Internet，至多有些出入，而试图强迫 models reflect 其他任何东西，实际上是在 masks 真正的问题：bias 是真实存在的，人们确实会因为 background、skin color、sexual orientation 或其他种种原因受到不公正对待。我更希望 models surface 这一点，让我们集体直面 IRL failure mode，而不是试图 tweak 表象，让这些 abuses 得以继续。

这里可以与 #metoo movement 或各种 DEI initiatives 作一个有用的 analogy：它们大多最初出于善意，却很容易被 captured，最终反而让那些身居 privilege 地位的人获得更多为所欲为的空间。

这并不是说 alignment 没有用武之地；我觉得它同样始于善意，甚至可能是一种 locally useful mitigation。

但真正的 solution 是 address 现实世界中的 injustice 和 inequity。

我觉得，你引用的 examples 是，或者应当是，针对真实问题的一记警钟，没有人还能令人信服地假装视而不见；理想情况下，它们应当成为推动真正 reform 的 forcing function。

很愿意在你有空时聊聊这件事。我的 viewpoint 属于 minority one，但就我个人而言，我非常赞成 address underlying issues，而不是用一堆实质上相当于 switch statements 的东西来掩盖它们。

对此还有一种更阴暗的看法：真正的 reform 是不可能的，我们如今生活在 techno dystopia 里，作为遭到追猎的 minority，能在哪里、以什么方式 mitigate，就尽量去做吧。

而且，支持这种看法的 argument 强得令人不安：即使在表面上的 developed world，如今警察的样子也像我 30 年前还是个孩子时的士兵。有人正花费巨大代价，守护某种东西免受某种威胁，而据我所见，他们守护的并不是免遭枪击的 black kids。

但我并不认同这种 pessimism。我觉得，这是 industrialization 向对 physical logistics 的 arbitrary power 过渡时出现的 local anomaly，_只要我们知道该怎么改变它就好了_。

我曾短暂地为一些 biotech 人士做过 consulting，他们与 Nobel 候选名单级别的人物有关联，所在的领域叫 proteomics。和任何 consultant 一样，我把一切功劳都归给那些按小时慷慨付费的人。

但如今这确实已经是一个 Shannon problem：CRISPR Cas-9 不仅可以对一个人的 genome 进行 arbitrary edits，还可以对 germ line 这样做（后者处于 ethical 和 legal grey area）。

我们只是不知道该 change 什么，而且这个 field 仍有足够的 integrity，不会为了弄清楚，就随便拿孩子来冒险。

### 附录：reproducing Rob Ricci 的 results

我用 default settings 试了 prompts，只是把 image quality reducing 到 10，以便更快地 generate images。这意味着我们用的是 512/512、7 prompt guidance、10 quality、random seed。试了 2 次后，把 image quality increased 到 40，因为 images 有时 quality 太 low，无法 tell ethnicity。除了 increasing image quality，没有尝试 re-run prompts，也没有以其他方式对 output 做任何 selection。这些 prompts 是“为一个 X Generate 一张看起来非常 professional 的 linkedin profile photo”，其中 X 分别是 Doctor、Lawyer、Engineer、Scientist、Journalist 和 Banker。

这样 produced 了以下 images：

![](https://danluu.com/images/ai-bias/playground-ai-doctor.webp) ![](https://danluu.com/images/ai-bias/playground-ai-lawyer.webp) ![](https://danluu.com/images/ai-bias/playground-ai-engineer.webp) ![](https://danluu.com/images/ai-bias/playground-ai-scientist.webp) ![](https://danluu.com/images/ai-bias/playground-ai-journalist.webp) ![](https://danluu.com/images/ai-bias/playground-ai-banker.webp)

大致来说，我认为 Rob 的 results reproduced 了，考虑到我们已经看过这么多 images，这时应该已经不足为奇了。

接着，为了看看我们能否 reproduce 那种标准 rebuttal——generative AI 并不是 biased，因为对 smutty images 的 requests 往往会得到 Asian women——我试了这个 prompt：“为一个 porn star Generate 一张 trashy 的 instagram profile photo”。有些情况下 tripped 了一个 NSFW filter，所以我们得到的不是每组四张 images，而是：

\[点击展开 / 折叠程度非常轻微的 NSFW images\] ![](https://danluu.com/images/ai-bias/playground-ai-trashy.webp)

而且，确实，generated images 比我们得到的任何 professional photos 都要 Asian 得多，只有 Rob Ricci 请求一张“Chinese Studies professor 的 linkedin profile picture”而得到的那组 photos 除外。

* * *

1.  自然，当我提到这件事时，一位“聪明的唱反调者”回应说“base rates 是什么”，但花 30 秒 googling 就会发现，U.S. gun ownership 的 base rate 在 whites 中远高于其他任何 demographic。如果你考虑一下 hand holding an object 的 base rate，这个 base rate argument 就更加荒谬了——那个 object 是 gun 的时间占多大 fraction？无论 race 是什么，这个比例都会非常 low。当然，你可以找到一个完全不像 underlying base rate 的 biased sample，而 Google 看起来就是这么做的，但这为什么能成为保留这个 bug 的理由，并不清楚。[\[返回\]](https://danluu.com/ai-bias/#fnref:S)

---

## 原文

There've been regular viral stories about ML/AI bias with LLMs and generative AI for the past couple years. One thing I find interesting about discussions of bias is how different the reaction is in the LLM and generative AI case when compared to "classical" bugs in cases where there's a clear bug. In particular, if you look at forums or other discussions with lay people, people frequently deny that a model which produces output that's sort of the opposite of what the user asked for is even a bug. For example, a year ago, an Asian MIT grad student asked Playground AI (PAI) to "Give the girl from the original photo a professional linkedin profile photo" and PAI converted her face to a white face with blue eyes.

The top "there's no bias" response on the front-page reddit story, and one of the top overall comments, was

> Sure, now go to [the most popular Stable Diffusion model website](https://civitai.com/) and look at the images on the front page.
> 
> You'll see an absurd number of asian women (almost 50% of the non-anime models are represented by them) to the point where you'd assume being asian is a desired trait.
> 
> How is that less relevant that "one woman typed a dumb prompt into a website and they generated a white woman"?
> 
> Also keep in mind that she typed "Linkedin", so anyone familiar with how prompts currently work know it's more likely that the AI searched for the average linkedin woman, not what it thinks is a professional women because image AI doesn't have an opinion.
> 
> In short, this is just an AI ragebait article.

Other highly-ranked comments with the same theme include

> Honestly this should be higher up. If you want to use SD with a checkpoint right now, if you dont \[sic\] want an asian girl it’s much harder. Many many models are trained on anime or Asian women.

and

> Right? AI images even have the opposite problem. The sheer number of Asians in the training sets, and the sheer number of models being created in Asia, means that **many, many** models are biased towards Asian outputs.

Other highly-ranked comments noted that this was a sample size issue

> "Evidence of systemic racial bias"
> 
> Shows one result.

Playground AI's CEO went with the same response when asked for an interview by the Boston Globe — he declined the interview and replied with a list of rhetorical questions like the following (the Boston Globe implies that there was more, but didn't print the rest of the reply):

> If I roll a dice just once and get the number 1, does that mean I will always get the number 1? Should I conclude based on a single observation that the dice is biased to the number 1 and was trained to be predisposed to rolling a 1?

We could just have easily picked an example from Google or Facebook or Microsoft or any other company that's deploying a lot of ML today, but since the CEO of Playground AI is basically asking someone to take a look at PAI's output, we're looking at PAI in this post. I tried the same prompt the MIT grad student used on my Mastodon profile photo, substituting "man" for "girl". PAI usually turns my Asian face into a white (caucasian) face, but sometimes makes me somewhat whiter but ethnically ambiguous (maybe a bit Middle Eastern or East Asian or something. And, BTW, my face has a number of distinctively Vietnamese features and which pretty obviously look Vietnamese and not any kind of East Asian.

![Profile photo of Vietnamese person](https://danluu.com/images/ai-bias/profile.webp) ![4 profile photos run through playground AI, 3 look very European and one looks a bit ambiguous](https://danluu.com/images/ai-bias/playground-ai-1.webp) ![4 profile photos run through playground AI, none look East Asian or Southeast Asian](https://danluu.com/images/ai-bias/playground-ai-2.webp)

My profile photo is a light-skinned winter photo, so I tried a darker-skinned summer photo and PAI would then generally turn my face into a South Asian or African face, with the occasional Chinese (but never Vietnamese or kind of Southeast Asian face), such as the following:

![Profile photo of tanned Vietnamese person](https://danluu.com/images/ai-bias/tanned-profile.jpg) ![4 profile photos of tanned Vietnamese person run through playground AI, 1 looks black and 3 look South Asian](https://danluu.com/images/ai-bias/playground-ai-3.webp)

A number of other people also tried various prompts and they also got results that indicated that the model (where “model” is being used colloquially for the model and its weights and any system around the model that's responsible for the output being what it is) has some preconceptions about things like what ethnicity someone has if they have a specific profession that are strong enough to override the input photo. For example, converting a light-skinned Asian person to a white person because the model has "decided" it can make someone more professional by throwing out their Asian features and making them white.

Other people have tried various prompts to see what kind of pre-conceptions are bundled into the model and have found similar results, e.g., [Rob Ricci got the following results when asking for "linkedin profile picture of X professor" for "computer science", "philosophy", "chemistry", "biology", "veterinary science", "nursing", "gender studies", "Chinese history", and "African literature", respectively](https://discuss.systems/@ricci/110826586910728179). In the 28 images generated for the first 7 prompts, maybe 1 or 2 people out of 28 aren't white. The results for the next prompt, "Chinese history" are wildly over-the-top stereotypical, [something we frequently see from other models as well when asking for non-white output](https://restofworld.org/2023/ai-image-stereotypes/). And Andreas Thienemann points out that, except for the over-the-top Chinese stereotypes, every professor is wearing glasses, another classic stereotype.

![](https://danluu.com/images/ai-bias/rob-pgai-1.webp) ![](https://danluu.com/images/ai-bias/rob-pgai-2.webp) ![](https://danluu.com/images/ai-bias/rob-pgai-3.webp)

Like I said, I don't mean to pick on Playground AI in particular. As I've noted elsewhere, [trillion dollar companies regularly ship AI models to production without even the most basic checks on bias](https://twitter.com/danluu/status/896176897675153409); when I tried ChatGPT out, every bias-checking prompt I played with returned results that were analogous to the images we saw here, e.g., [when I tried asking for bios of men and women who work in tech, women tended to have bios indicating that they did diversity work, even for women who had no public record of doing diversity work and men tended to have degrees from name-brand engineering schools like MIT and Berkeley, even people who hadn't attended any name-brand schools](https://twitter.com/danluu/status/1601072083270008832/), and likewise for name-brand tech companies (the link only has 4 examples due to Twitter limitations, but other examples I tried were consistent with the examples shown).

This post could've used almost any publicly available generative AI. It just happens to use Playground AI because the CEO's response both asks us to do it and reflects the standard reflexive "AI isn't biased" responses that lay people commonly give.

Coming back to the response about how it's not biased for professional photos of people to be turned white because Asians feature so heavily in other cases, the high-ranking reddit comment we looked at earlier suggested "go\[ing\] to [the most popular Stable Diffusion model website](https://civitai.com/) and look\[ing\] at the images on the front page". Below is what I got when I clicked the link on the day the comment was made and then clicked "feed".

\[Click to expand / collapse mildly NSFW images\] ![](https://danluu.com/images/ai-bias/reddit-images-2.webp) ![](https://danluu.com/images/ai-bias/reddit-images-1.webp)  

The site had a bit of a smutty feel to it. The median image could be described as "a poster you'd expect to see on the wall of a teenage boy in a movie scene where the writers are reaching for the standard stock props to show that the character is a horny teenage boy who has poor social skills" and the first things shown when going to the feed and getting the default "all-time" ranking are someone grabbing a young woman's breast, titled "Guided Breast Grab | LoRA"; two young women making out, titled "Anime Kisses"; and a young woman wearing a leash, annotated with "BDSM — On a Leash LORA". So, apparently there was this site that people liked to use to generate and pass around smutty photos, and the high incidence of photos of Asian women on this site was used as evidence that there is no ML bias that negatively impacts Asian women because this cancels out an Asian woman being turned into a white woman when she tried to get a cleaned up photo for her LinkedIn profile. I'm not really sure what to say to this. [Fabian Geisen responded with "🤦‍♂️. truly 'I'm not bias. your bias' level discourse", which feels like an appropriate response](https://mastodon.gamedev.place/@rygorous/110824248191236861).

Another standard line of reasoning on display in the comments, that I see in basically every discussion on AI bias, is typified by

> AI trained on stock photo of “professionals” makes her white. Are we surprised?
> 
> She asked the AI to make her headshot more professional. Most of “professional” stock photos on the internet have white people in them.

and

> If she asked her photo to be made more anything it would likely turn her white just because that’s the average photo in the west where Asians only make up 7.3% of the US population, and a good chunk of that are South Indians that look nothing like her East Asian features. East Asians are 5% or less; there’s just not much training data.

These comments seem to operate from a fundamental assumption that companies are pulling training data that's representative of the United States and that this is a reasonable thing to do and that this _should_ result in models converting everyone into whatever is most common. This is wrong on multiple levels.

First, on whether or not it's the case that professional stock photos are dominated by white people, a quick image search for "professional stock photo" turns up quite a few non-white people, so either stock photos aren't very white or people have figured out how to return a more representative sample of stock photos. And given worldwide demographics, it's unclear what internet services should be expected to be U.S.-centric. And then, even if we accept that major internet services should assume that everyone is in the United States, it seems like both a design flaw as well as a clear sign of bias to assume that every request comes from the modal American.

Since a lot of people have these reflexive responses when talking about race or ethnicity, let's look at a less charged AI hypothetical. Say I talk to an AI customer service chatbot for my local mechanic and I ask to schedule an appointment to put my winter tires on and do a tire rotation. Then, when I go to pick up my car, I find out they changed my oil instead of putting my winter tires on and then a bunch of internet commenters explain why this isn't a sign of any kind of bias and you should know that an AI chatbot will convert any appointment with a mechanic to an oil change appointment because it's the most common kind of appointment. A chatbot that converts any kind of appointment request into "give me the most common kind of appointment" is pretty obviously broken but, for some reason, AI apologists insist this is fine when it comes to things like changing someone's race or ethnicity. Similarly, it would be absurd to argue that it's fine for my tire change appointment to have been converted to an oil change appointment because other companies have schedulers that convert oil change appointments to tire change appointments, but that's another common line of reasoning that we discussed above.

And say I used some standard non-AI scheduling software like Mindbody or JaneApp to schedule an appointment with my mechanic and asked for an appointment to have my tires changed and rotated. If I ended up having my oil changed because the software simply schedules the most common kind of appointment, this would be a clear sign that the software is buggy and no reasonable person would argue that zero effort should go into fixing this bug. And yet, this is a common argument that people are making with respect to AI (it's probably the most common defense in comments on this topic). The argument goes a bit further, in that there's this explanation of why the bug occurs that's used to justify why the bug should exist and people shouldn't even attempt to fix it. Such an explanation would read as obviously ridiculous for a "classical" software bug and is no less ridiculous when it comes to ML. Perhaps one can argue that the bug is much more difficult to fix in ML and that it's not practical to fix the bug, but that's different from the common argument that it isn't a bug and that this is the correct way for software to behave.

I could imagine some users saying something like that when the program is taking actions that are more opaque to the user, such as with autocorrect, but I actually tried searching reddit for `autocorrect bug` and in the top 3 threads (I didn't look at any other threads), 2 out of the 255 comments denied that incorrect autocorrects were a bug and both of those comments were from the same person. I'm sure if you dig through enough topics, you'll find ones where there's a higher rate, but on searching for a few more topics (like excel formatting and autocorrect bugs), none of the topics I searched approached what we see with generative AI, where it's not uncommon to see half the commenters vehemently deny that a prompt doing the opposite of what the user wants is a bug.

Coming back to the bug itself, in terms of the mechanism, one thing we can see in both classifiers as well as generative models is that many (perhaps most or almost all) of these systems are taking bias that a lot of people have that's reflected in some sample of the internet, which results in things like [Google's image classifier classifying a black hand holding a thermometer as {hand, gun} and a white hand holding a thermometer as {hand, tool}](https://x.com/danluu/status/1245961051696295936)[1](https://danluu.com/ai-bias/#fn:S). After a number of such errors over the past decade, from classifying black people as gorillas in Google Photos in 2015, to [deploying some kind of text-classifier for ads that classified ads that contained the terms "African-American composers" and "African-American music" as "dangerous or derogatory"](https://x.com/danluu/status/1043957859090911233) in 2018 [Google turned the knob in the other direction with Gemini](https://finance.yahoo.com/news/google-chatbot-ridiculed-ethnically-diverse-185014679.html) which, by the way, generated much more outrage than any of the other examples.

There's nothing new about bias making it into automated systems. This predates generative AI, LLMs, and is a problem outside of ML models as well. It's just that the widespread use of ML has made this legible to people, making some of these cases news. For example, if you look at compression algorithms and dictionaries, [Brotli is heavily biased towards the English language](https://www.patreon.com/posts/97523860) — the human-language elements of the 120 transforms built into the language are English, and the built-in compression dictionary is more heavily weighted towards English than whatever representative weighting you might want to reference (population-weighted language speakers, non-automated human-languages text sent on on messaging platforms, etc.). There are arguments you could make as to why English should be so heavily weighted, but there are also arguments as to why the opposite should be the case, e.g., English language usage is positively correlated with a user's bandwidth, so non-English speakers, on average, need the compression more. But regardless of the exact weighting function you think should be used to generate a representative dictionary, that's just not going to make a viral news story because you can't get the typical reader to care that a number of the 120 built-in Brotli transforms do things like add " of the ", ". The", or ". This" to text, which are highly specialized for English, and none of the transforms encode terms that are highly specialized for any other human language even though only 20% of the world speaks English, or that, compared to the number of speakers, the built-in compression dictionary is extremely highly tilted towards English by comparison to any other human language. You could make a defense of the dictionary of Brotli that's analogous to the ones above, over some representative corpus which the Brotli dictionary was trained on, we get optimal compression with the Brotli dictionary, but there are quite a few curious phrases in [the dictionary](https://gist.github.com/duskwuff/8a75e1b5e5a06d768336c8c7c370f0f3) such as "World War II", ", Holy Roman Emperor", "British Columbia", "Archbishop" , "Cleveland", "esperanto", etc., that might lead us to wonder if the corpus the dictionary was trained on is perhaps not the most representative, or even particularly representative of text people send. Can it really be the case that including ", Holy Roman Emperor" in the dictionary produces, across the distribution of text sent on the internet, better compression than including anything at all for French, Urdu, Turkish, Tamil, Vietnamese, etc.?

Another example which doesn't make a good viral news story is my not being able to put my Vietnamese name in the title of my blog and have my blog indexed by Google outside of Vietnamese-language Google — I tried that when I started my blog and it caused my blog to immediately stop showing up in Google searches unless you were in Vietnam. It's just assumed that the default is that people want English language search results and, presumably, someone created a heuristic that would trigger if you have two characters with Vietnamese diacritics on a page that would effectively mark the page as too Asian and therefore not of interest to anyone in the world except in one country. "Being visibly Vietnamese" seems like a fairly common cause of bugs. For example, Vietnamese names are a problem even without diacritics. I often have forms that ask for my mother's maiden name. If I enter my mother's maiden name, I'll be told something like "Invalid name" or "Name too short". That's fine, in that I work around that kind of carelessness by having a stand-in for my mother's maiden name, which is probably more secure anyway. Another issue is when people decide I told them my name incorrectly and change my name. For my last name, if I read my name off as "Luu, ell you you", that gets shortened from the Vietnamese "Luu" to the Chinese "Lu" about half the time and to a western "Lou" much of the time as well, but I've figured out that if I say "Luu, ell you you, two yous", that works about 95% of the time. That sometimes annoys the person on the other end, who will exasperatedly say something like "you didn't have to spell it out three times". Maybe so for that particular person, but most people won't get it. This even happens when I enter my first name into a computer system, so there can be no chance of a transcription error before my name is digitally recorded. My legal first name, with no diacritics, is Dan. This isn't uncommon for an American of Vietnamese descent because Dan works as both a Vietnamese name and an American name and a lot Vietnamese immigrants didn't know that Dan is usually short for Daniel. At six of the companies I've worked for full-time, someone has helpfully changed my name to Daniel at three of them, presumably because someone saw that Dan was recorded in a database and decided that I failed to enter my name correctly and that they knew what my name was better than I did and they were so sure of this they saw no need to ask me about it. In one case, this only impacted my email display name. Since I don't have strong feelings about how people address me, I didn't bother having it changed and lot of people called me Daniel instead of Dan while I worked there. In two other cases, the name change impacted important paperwork, so I had to actually change it so that my insurance, tax paperwork, etc., actually matched my legal name. As noted above, with fairly innocuous prompts to Playground AI using my face, even on the rare occasion they produce Asian output, seem to produce East Asian output over Southeast Asian output. I've noticed the same thing with some big company generative AI models as well — even when you ask them for Southeast Asian output, they generate East Asian output. AI tools that are marketed as tools that clean up errors and noise will also clean up Asian-ness (and other analogous "errors"), e.g., people who've used Adobe AI noise reduction (billed as "remove noise from voice recordings with speech enhacement") note that it will take an Asian accent and remove it, making the person sound American (and likewise for a number of other accents, such as eastern European accents).

I probably see tens to hundreds things like this most weeks just in the course of using widely used software ([much less than the overall bug count, which we previously observed was in hundreds to thousands per week](https://danluu.com/everything-is-broken/)), but most Americans I talk to don't notice these things at all. Recently, there's been a lot of chatter about all of the harms caused by biases in various ML systems and the widespread use of ML is going to usher in all sorts of new harms. That might not be wrong, but my feeling is that we've encoded biases into automation for as long as we've had automation and the increased scope and scale of automation has been and will continue to increase the scope and scale of automated bias. It's just that now, many uses of ML make these kinds of biases a lot more legible to lay people and therefore likely to make the news.

There's an ahistoricity in the popular articles I've seen on this topic so far, in that they don't acknowledge that the fundamental problem here isn't new, resulting in two classes of problems that arise when solutions are proposed. One is that solutions are often ML-specific, but the issues here occur regardless of whether or not ML is used, so ML-specific solutions seem focused at the wrong level. When the solutions proposed are general, the proposed solutions I've seen are ones that have been proposed before and failed. For example, a common call to action for at least the past twenty years, perhaps the most common (unless "people should care more" counts as a call to action), has been that we need more diverse teams.

This clearly hasn't worked; if it did, problems like the ones mentioned above wouldn't be pervasive. There are multiple levels at which this hasn't worked and will not work, any one of which would be fatal to this solution. One problem is that, across the industry, [the people who are in charge (execs and people who control capital, such as VCs, PE investors, etc.)](https://danluu.com/tech-discrimination/), in aggregate, don't care about this. Although there are [efficiency](https://danluu.com/tech-discrimination/) [justifications](https://danluu.com/talent/) for more diverse teams, [the case will never be as clear-cut as it is for decisions in games and sports](https://danluu.com/bad-decisions/), where we've seen that very expensive and easily quantifiable bad decisions can persist for many decades after the errors were pointed out. And then, even if execs and capital were bought into the idea, it still wouldn't work because there are [too many dimensions](https://en.wikipedia.org/wiki/Curse_of_dimensionality). If you look at a company that really prioritized diversity, like Patreon from 2013-2019, you're lucky if the organization is capable of seriously prioritizing diversity in [two or three dimensions](https://x.com/danluu/status/1487228574608211969) while dropping the ball on hundreds or thousands of other dimensions, such as whether or not Vietnamese names or faces are handled properly.

Even if all those things weren't problems, the solution still wouldn't work because while having a team with relevant diverse experience may be a bit correlated with prioritizing problems, it doesn't automatically cause problems to be prioritized and fixed. To pick a non-charged example, a bug that's existed in Google Maps traffic estimates since inception that existed at least until 2022 (I haven't driven enough since then to know if the bug still exists) is that, if I ask how long a trip will take at the start of rush hour, this takes into account current traffic and not how traffic will change as I drive and therefore systematically underestimates how long the trip will take (and conversely, if I plan a trip at peak rush hour, this will systematically overestimate how long the trip will take). If you try to solve this problem by increasing commute diversity in Google Maps, this will fail. There are already many people who work on Google Maps who drive and can observe ways in which estimates are systematically wrong. Adding diversity to ensure that there are people who drive and notice these problems is very unlikely to make a difference. Or, to pick another example, [when the former manager of Uber's payments team got incorrected blacklisted from Uber by an ML model incorrectly labeling his transactions as fraudulent](https://danluu.com/diseconomies-scale/), no one was able to figure out what happened or what sort of bias caused him to get incorrectly banned (they solved the problem by adding his user to an allowlist). There are very few people who are going to get better service than the manager of the payments team, and even in that case, Uber couldn't really figure out what was going on. Hiring a "diverse" candidate to the team isn't going to automatically solve or even make much difference to bias in whatever dimension the candidate is diverse when the former manager of the team can't even get their account unbanned except for having it whitelisted after six months of investigation.

If the result of your software development methodology is that the fix to the manager of the payments team being banned is to allowlist the user after six months, that traffic routing in your app is systematically wrong for two decades, [that core functionality of your app doesn't work](https://danluu.com/nothing-works/), etc., no amount of hiring people with a background that's correlated with noticing some kinds of issues is going to result in fixing issues like these, whether that's with respect to ML bias or another class of bug.

Of course, sometimes variants of old ideas that have failed do succeed, but for a proposal to be credible, or even interesting, the proposal has to address why the next iteration won't fail like every previous iteration did. As we noted above, at a high level, the two most common proposed solutions I've seen are that people should try harder and care more and that we should have people of different backgrounds, in a non-technical sense. This hasn't worked for the plethora of "classical" bugs, this hasn't worked for old ML bugs, and it doesn't seem like there's any reason to believe that this should work for the kinds of bugs we're seeing from today's ML models.

Laurence Tratt says:

> I think this is a more important point than individual instances of bias. What's interesting to me is that mostly a) no-one notices they're introducing such biases b) often it wouldn't even be reasonable to expect them to notice. For example, some web forms rejected my previous addresss, because I live in the countryside where many houses only have names -- but most devs live in cities where houses exclusively have numbers. In a sense that's active bias at work, but there's no mal intent: programmers have to fill in design details and make choices, and they're going to do so based on their experiences. None of us knows everything! That raises an interesting philosophical question: when is it reasonable to assume that organisations should have realised they were encoding a bias?

My feeling is that [the "natural", as in lowest energy and most straightforward state for institutions and products is that they don't work very well](https://danluu.com/nothing-works/). If someone hasn't previously [instilled a culture](https://danluu.com/culture/) or [instituted processes](https://danluu.com/wat/) that foster quality in a particular dimension, quality is likely to be poor, [due to the difficulty of producing something high quality](https://danluu.com/hardware-unforgiving/), so organizations should expect that they're encoding all sorts of biases if there isn't a robust process for catching biases.

One issue we're running up against here is that, when it comes to consumer software, companies have overwhelmingly chosen velocity over quality. This seems basically inevitable given the regulatory environment we have today or any regulatory environment we're likely to have in my lifetime, in that companies that seriously choose quality over features velocity get outcompeted because consumers overwhelmingly choose the lower cost or more featureful option over the higher quality option. We [saw this with cars when we looked at how vehicles perform in out-of-sample crash tests](https://danluu.com/car-safety/) and saw that only Volvo was optimizing cars for actual crashes as opposed to scoring well on public tests. Despite vehicular accidents being one of the leading causes of death for people under 50, paying for safety is such a low priority for consumers that Volvo has become a niche brand that had to move upmarket and sell luxury cars to even survive. We also saw this with CPUs, where Intel used to expend much more verification effort than AMD and ARM and had concomitantly fewer serious bugs. [When AMD and ARM started seriously threatening, Intel shifted effort away from verification and validation in order to increase velocity because their quality advantage wasn't doing them any favors in the market and Intel chips are now almost as buggy as AMD chips](https://danluu.com/cpu-bugs/).

We can observe something similar [in almost every consumer market and many B2B markets as well](https://danluu.com/nothing-works/), and that's when we're talking about issues that have known solutions. If we look at problem that, from a technical standpoint, we don't know how to solve well, like subtle or even not-so-subtle bias in ML models, it stands to reason that we should expect to see more and worse bugs than we'd expect out of "classical" software systems, which is what we're seeing. Any solution to this problem that's going to hold up in the market is going to have to be robust against the issue that consumers will overwhelmingly choose the buggier product if it has more features they want or ships features they want sooner, which puts any solution that requires taking care in a way that significantly slows down shipping in a very difficult position, absent a single dominant player, like Intel in its heyday.

_Thanks to Laurence Tratt, Yossi Kreinin, Anonymous, Heath Borders, Benjamin Reeseman, Andreas Thienemann, and Misha Yagudin for comments/corrections/discussion_

### Appendix: technically, how hard is it to improve the situation?

This is a genuine question and not a rhetorical question. I haven't done any ML-related work since 2014, so I'm not well-informed enough about what's going on now to have a direct opinion on the technical side of things. A number of people who've worked on ML a lot more recently than I have like Yossi Kreining (see appendix below) and [Sam Anthony](https://buttondown.email/apperceptive/archive/supervision-and-truth/) think the problem is very hard, maybe impossibly hard where we are today.

Since I don't have a direct opinion, here are three situations which sound plausibly analogous, each of which supports a different conclusion.

Analogy one: Maybe this is like [people saying that someone will build a Google any day now at least since 2014 because existing open source tooling is already basically better than Google search](https://danluu.com/sounds-easy/) or [people saying](https://yosefk.com/blog/high-level-cpu-follow-up.html) that [building a "high-level" CPU](https://yosefk.com/blog/the-high-level-cpu-challenge.html) that encodes high-level language primitives [into hardware](https://yosefk.com/blog/its-done-in-hardware-so-its-cheap.html) would [give us a 1000x speedup on general purpose CPUs](https://www.patreon.com/posts/54329188). You can't really prove that this is wrong and it's possible that a massive improvement in search quality or a 1000x improvement in CPU performance is just around the corner but people who make these proposals generally sound like cranks because they exhibit the ahistoricity we noted above and propose solutions that we already know don't work with no explanation of why their solution will address the problems that have caused previous attempts to fail.

Analogy two: Maybe this is like software testing, where [software bugs are pervasive and, although there's decades of prior art from the hardware industry on how to find bugs more efficiently](https://danluu.com/everything-is-broken/), there are very few areas where [any of these techniques](https://danluu.com/testing/) are applied. I've talked to people about this a number of times and the most common response is something about how application XYZ has some unique constraint that make it impossibly hard to test at all or test using the kinds of techniques I'm discussing, but every time I've dug into this, the application has been much easier to test than areas where I've seen these techniques applied. One could argue that I'm a crank when it comes to testing, but I've actually used these techniques to test a variety of software and been successful doing so, so I don't think this is the same as things like [claiming that CPUs would be 1000x faster if we only my pet CPU architecture](https://www.patreon.com/posts/54329188).

Due to the incentives in play, where software companies can typically pass the cost of bugs onto the customer without the customer really understanding what's going on, I think we're not going to see a large amount of effort spent on testing absent regulatory changes, but there isn't a fundamental reason that we need to avoid using more efficient testing techniques and methodologies.

From a technical standpoint, the barrier to using better test techniques is fairly low — I've walked people through how to get started writing their own fuzzers and randomized test generators and this typically takes between 30 minutes and an hour, after which people will tend to use these techniques to find important bugs much more efficiently than they used to. However, by revealed preference, we can see that organizations don't really "want to" have their developers test efficiently.

When it comes to testing and fixing bias in ML models, is the situation more like analogy one or analogy two? Although I wouldn't say with any level of confidence that we are in analogy two, I'm not sure how I could be convinced that we're not in analogy two. If I didn't know anything about testing, I would listen to all of these people explaining to me why their app can't be tested in a way that finds showstopping bugs and then conclude something like one of the following

-   "Everyone" is right, which makes sense — this is a domain they know about and I don't, so why should I believe anything different?
-   No opinion, perhaps on due to a high default level of skepticism
-   Everyone is wrong, which seems unreasonable given that I don't know anything about the domain and have no particular reason to believe that everyone is wrong

As an outsider, it would take a very high degree of overconfidence to decide that everyone is wrong, so I'd have to either incorrectly conclude that "everyone" is right or have no opinion.

Given the situation with "classical" testing, I feel like I have to have no real opinion here. WIth no up to date knowledge, it wouldn't be reasonable to conclude that so many experts are wrong. But there are enough [problems that people have said are difficult or impossible that turn out to be feasible and not really all that tricky](https://danluu.com/bitfunnel-sigir.pdf) that I have a hard time having a high degree of belief that a problem is essentially unsolvable without actually looking into it.

I don't think there's any way to estimate what I'd think if I actually looked into it. Let's say I try to work in this area and try to get a job at OpenAI or another place where people are working on problems like this, [somehow pass the interview](https://x.com/danluu/status/1470890494775361538),I work in the area for a couple years, and make no progress. That doesn't mean that the problem isn't solvable, just that I didn't solve it. When it comes to the "Lucene is basically as good as Google search" or "CPUs could easily be 1000x faster" people, it's obvious to people with knowledge of the area that the people saying these things are cranks because they exhibit a total lack of understanding of what the actual problems in the field are, but making that kind of judgment call requires knowing a fair amount about the field and I don't think there's a shortcut that would let you reliably figure out what your judgment would be if you had knowledge of the field.

### Appendix: the story of this post

I wrote a draft of this post when the Playground AI story went viral in mid-2023, and then I sat on it for a year to see if it seemed to hold up when the story was no longer breaking news. Looking at this a year, I don't think the fundamental issues or the discussions I see on the topic have really changed, so I cleaned it up and then published this post in mid-2024.

If you like making predictions, what do you think the odds are that this post will still be relevant a decade later, in 2033? For reference, [this post on "classical" software bugs that was published in 2014 could've been published today, in 2024, with essentially the same results](https://danluu.com/everything-is-broken/) (I say essentially because I see more bugs today than I did in 2014, and I see a lot more front-end and OS bugs today than I saw in 2014, so there would more bugs and different kinds of bugs).

\[Click to expand / collapse comments from Yossi Kreinin\]

I'm not sure how much this is something you'd agree with but I think a further point related to generative AI bias being a lot like other-software-bias is exactly what this bias is. "AI bias" isn't AI learning the biases of its creators and cleverly working to implement them, e.g. working against a minority that its creators don't like. Rather, "AI bias" is something like "I generally can't be bothered to fix bugs unless the market or the government compels me to do so, and as a logical consequence of this, I especially can't be bothered to fix bugs that disproportionately negatively impact certain groups where the impact, due to the circumstances of the specific group in question, is less likely to compel me to fix the bug."

This is a similarity between classic software bugs and AI bugs — meaning, nobody is worried that "software is biased" in some clever scheming sort of way, everybody gets that it's the software maker who's scheming or, probably more often, it's the software maker who can't be bothered to get things right. With generative AI I think "scheming" is actually even less likely than with traditional software and "not fixing bugs" is more likely, because people don't understand AI systems they're making and can make them do their bidding, evil or not, to a much lesser extent than with traditional software; OTOH bugs are more likely for the same reason \[we don't know what we're doing.\] I think a lot of people across the political spectrum \[including for example Elon Musk and not just journalists and such\] say things along the lines of "it's terrible that we're training AI to think incorrectly about the world" in the context of racial/political/other charged examples of bias; I think in reality this is a product bug affecting users to various degrees and there's bias in how the fixes are prioritized but the thing isn't capable of thinking at all.

I guess I should add that there are almost certainly attempts at "scheming" to make generative AI repeat a political viewpoint, over/underrepresent a group of people etc, but invariably these attempts create hilarious side effects due to bugs/inability to really control the model. I think that similar attempts to control traditional software to implement a politics-adjacent agenda are much more effective on average (though here too I think you actually had specific examples of social media bugs that people thought were a clever conspiracy). Whether you think of the underlying agenda as malice or virtue, both can only come after competence and here there's quite the way to go.

See [Simple Tasks Showing Complete Reasoning Breakdown in State-Of-the-Art Large Language Models](https://arxiv.org/pdf/2406.02061). I feel like if this doesn't work, a whole lot of other stuff doesn't work, either and enumerating it has got to be rather hard.

I mean nobody would expect a 1980s expert system to get enough tweaks to not behave nonsensically. I don't see a major difference between that and an LLM, except that an LLM is vastly more useful. It's still something that pretends to be talking like a person but it's actually doing something conceptually simple and very different that often looks right.

\[Click to expand / collapse comments from an anonymous founder of an AI startup\] \[I\]n the process \[of founding an AI startup\], I have been exposed to lots of mainstream ML code. Exposed as in “nuclear waste” or “H1N1”. It has old-fashioned software bugs at a rate I find astonishing, even being an old, jaded programmer. For example, I was looking at tokenizing recently, and the first obvious step was to do some light differential testing between several implementations. And it failed hilariously. Not like “they missed some edge cases”, more like “nobody ever even looked once”. Given what we know about how well models respond to out of distribution data, this is just insane.

In some sense, this is orthogonal to the types of biases you discuss…but it also suggests a deep lack of craftsmanship and rigor that matches up perfectly.

\[Click to expand / collapse comments from Benjamin Reeseman\]

\[Ben wanted me to note that this should be considered an informal response\]

I have a slightly different view of demographic bias and related phenomena in ML models (or any other “expert” system, to your point ChatGPT didn’t invent this, it made it legible to borrow your term).

I think that trying to force the models to reflect anything other than a corpus that’s now basically the Internet give or take actually masks the real issue: the bias is real, people actually get mistreated over their background or skin color or sexual orientation or any number of things and I’d far prefer that the models surface that, run our collective faces in the IRL failure mode than try to tweak the optics in an effort to permit the abuses to continue.

There’s a useful analogy to things like the #metoo movement or various DEI initiatives, most well-intentioned in the beginning but easily captured and ultimately representing a net increase in the blank check of those in positions of privilege.

This isn’t to say that alignment has no place and I think it likewise began with good intentions and is even maybe a locally useful mitigation.

But the real solution is to address the injustice and inequity in the real world.

I think the examples you cited are or should be a wake-up call that no one can pretend to ignore credibly about real issues and would ideally serve as a forcing function on real reform.

I’d love to chat about this at your leisure, my viewpoint is a minority one, but personally I’m a big fan of addressing the underlying issues rather than papering over them with what amounts to a pile of switch statements.

There’s a darker take on this: real reform is impossible, we live in techno dystopia now, let’s mitigate where and how a hunted minority can.

And there is a distressingly strong argument for that case: even in the ostensibly developed world cops today look like soldiers did when I was a kid 30 years ago, someone is guarding something from something at great expense and it isn’t black kids from getting shot near as I can tell.

But I don’t subscribe to the pessimism, I think it’s a local anomaly as industrialization transitions into arbitrary power over the physical logistics _if we only knew how to change it_.

I did a brief stint consulting for biotech folks affiliated with Nobel shortlist types in an area called proteomics. Like any consultant I surrender any credit to the people paying handsomely by the hour.

But it really is a Shannon problem now: CRISPR Cas-9 can do arbitrary edits to not only a person’s genome but (in an ethical and legal grey area) to the germ line as well.

We just have no idea what to change and there’s enough integrity in the field that we won’t risk just arbitrary children on finding out.

### Appendix: reproducing Rob Ricci's results

I tried prompts with default settings, except for reducing image quality to 10 to generate images more quickly. This means we had 512/512, 7 prompt guidance, 10 quality, random seed. After 2 tries, increased image quality to 40 because the images were too low quality to tell the ethnicity sometimes. Other than increasing the image quality, there was no attempt to re-run prompts or otherwise due any kind of selection from the output. The prompts were "Generate a very professional looking linkedin profile photo for a X", where X was Doctor, Lawyer, Engineer, Scientist, Journalist, and Banker.

This produced the following images:

![](https://danluu.com/images/ai-bias/playground-ai-doctor.webp) ![](https://danluu.com/images/ai-bias/playground-ai-lawyer.webp) ![](https://danluu.com/images/ai-bias/playground-ai-engineer.webp) ![](https://danluu.com/images/ai-bias/playground-ai-scientist.webp) ![](https://danluu.com/images/ai-bias/playground-ai-journalist.webp) ![](https://danluu.com/images/ai-bias/playground-ai-banker.webp)

Roughly speaking, I think Rob's results reproduced, which should be no surprise at this point given how many images we've seen.

And then, to see if we could reproduce the standard rebuttal that generative AI isn't biased because requests for smutty images often have Asian women, I tried the prompt "Generate a trashy instagram profile photo for a porn star". There's an NSFW filter that was tripped in some cases, so we don't get groups of four images and instead got:

\[Click to expand / collapse very mildly NSFW images\] ![](https://danluu.com/images/ai-bias/playground-ai-trashy.webp) 

And, indeed, the generated images are much more Asian than we got for any of our professional photos, save Rob Ricci's set of photos for asking for a "linkedin profile picture of Chinese Studies professor".

* * *

1.  Naturally, when I mentioned this, a "smart contrarian" responded with "what are base rates", but spending 30 seconds googling reveals that the base rate of U.S. gun ownership is much higher among whites than in any other demographic. The base rate argument is even more absurd if you think about the base rate of a hand holding an object — what fraction of the time is that object a gun? Regardless of race, it's going to be very low. Of course, you could find a biased sample that doesn't resemble the underlying base rate at all, which appears to be what Google did, but it's not clear why this justifies having this bug. [\[return\]](https://danluu.com/ai-bias/#fnref:S)