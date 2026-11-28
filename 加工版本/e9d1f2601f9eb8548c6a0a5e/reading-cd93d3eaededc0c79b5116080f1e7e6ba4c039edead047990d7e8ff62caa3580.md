# （一些）优秀的企业技术博客是如何写成的｜How (some) good corporate engineering blogs are written

---

## 正文

我一直在与运营 corporate engineering blogs 的人交流心得，有一点让我觉得很奇怪：我的 personal blog 获得的 traffic 比一家 valuation 达到九到十位数的公司的整个 corp eng blog 还多，这是相当常见的事；而我的 blog 获得的 traffic 多出一个 order of magnitude，也并不少见。

我觉得这很奇怪，因为那个级别的 tech companies 往往拥有数百到数千名 employees。它们极有可能比我更有条件写出 compelling blog，而且 companies 从拥有 compelling blog 中获得的 value，也远比我多。

就前者而言，公司的 employees 做过的 engineering work 会比任何一个拥有 personal blog 的人更有趣，有更多好玩的故事，也拥有更 in-depth 的 knowledge。至于后者，我的 blog 帮助我 job searching，也帮助 companies hire。但我只需要一份 job，所以更多 exposure 充其量也只是让我得到一份稍好一点的 job；而我工作过的 tech company 中，除了一家以外，其余都迫切需要 hire，而且不断将 candidates 输给其他 companies。此外，我在 interview 时，其实并不是在与其他 candidates competing（即使我们为同一份 job 进行 interview，如果公司看中了我们中的不止一人，通常也只会增加更多 jobs）。这个 blog 对 job searching 而言的 high-order bit，在于那个 process 能否采纳重要的 non-interview feedback，还是[我会 fail the interview，因为他们采用 conventional interview](https://danluu.com/algorithms-interviews/)；而多一篇 post 对此的 marginal value 可能非常低。另一方面，companies 在 recruiting 时的 compete 相对直接，因此，相较另一家公司更 compelling，对它们是有 value 的；replicating Cloudflare 或 Segment 用于其 engineering "brands" 的 playbook，会带来显著的 recruiting advantage。这个 playbook 并不是秘密：这些 companies 向全世界 broadcast 它们的 output，而且通常很乐意谈论自己的 blogging process。

尽管拥有一个“好”的 corp eng blog 所带来的 benefits 似乎显而易见，但大多数 corp eng blogs 都充斥着 engineers 不想读的东西：含糊、high-level、空洞地吹嘘一切有多么了不起的内容，content marketing，围绕最新热门事物而含糊其辞的 posts（今天，可能是将 deep learning 用于 inappropriate applications；十年前，可能是将 "big data" 用于 inappropriate applications），等等。

为了试着理解拥有良好 corporate engineering blog 的 companies 有什么共同之处，我访谈了三家拥有 compelling corporate engineering blogs 的不同公司的人员（Cloudflare、Heap 和 Segment），也访谈了三家 corporate engineering blogs 很差的不同公司的人员（我不打算说出它们的名字）。

从整体来看，那些 compelling engineering blogs 的 processes 具有以下共同 properties：

-   approval process 很 Easy，不需要很多 approvals
-   只需要很少的 non-engineering approvals，或者完全不需要
-   对于 approvals，有 Implicit 或 explicit 的 fast [SLO](https://en.wikipedia.org/wiki/Service-level_objective)
-   Approval/editing process 主要使 posts 对 engineers 更 compelling
-   有 Direct、high-level（co-founder、C-level 或 VP-level）的 support，让 blog process 保持 lightweight

那些不那么 compelling 的 engineering blogs，其 processes 具有以下共同 properties：

-   approval process 很 Slow
-   需要很多 approvals
-   需要大量 non-engineering approvals
    -   Non-engineering approvals 会提出让 authors 感到沮丧的 changes
    -   来回往复可能持续数月
-   Approval/editing process 主要对 posts 进行 de-risks，removes 对 specifics 的 references，使 posts 更 vaguer，对 engineers 更不有趣
-   实际上没有对 blogging 的 high-level support
    -   Leadership 可能会在抽象层面同意 blogging 是好事，但它的 priority 还不够高，不足以促使其采取具体行动
    -   通过 Reforming process 让 blogging 更容易，非常困难；此前的尝试都失败了
    -   通过 Changing process 来 reduce overhead，需要所有 "stakeholders" sign off（其中一个案例有 14 位）
        -   任何一个 stakeholder 都可以 block
        -   没有任何一个 stakeholder 能够单独 approve
    -   Stakeholders 对 approving 任何会 reduces overhead 的事情都心存戒备
        -   Approving 意味着承担 perceived risk（万一发生了什么坏事呢），却没有他们能感知到的 benefit

一家拥有 compelling blog 的公司中，有人指出，只有一位 approver 和/或一位 primary approver 的 downside 是，如果那个人很忙，posts 可能要几周才能获得 approved。这话有道理，这是 centralized approval 的一个 downside。不过，与其他 processes 相比，有一家公司的人员指出，approvals 通常要花三到六个月，而 tail cases 可能要花一年。

对于习惯了行动迅速的公司的人来说，几周可能显得很久，但行动较慢的公司中的人，如果能有一个耗时仅为其两倍的 approval process，就会欣喜若狂。

以下是我访谈的三家公司向我描述的 processes（按 `sha512sum` 顺序呈现，碰巧也按公司 size 递增排列，从几百名 employees 到接近一千名 employees）：

#### Heap 公司

-   有人产生了写一篇 post 的想法
-   Writer（是一名 engineer）会与一位 "buddy" 进行 paired，由后者 edits，然后 approves 这篇 post
    -   Buddy 是一名 engineer，有过写出合理文章的 track record
    -   这可能需要几轮，也可能改变 post 的主旨
-   CTO 会 reads 并 approves
    -   通常只有 minor feedback
    -   可能会提出诸如“designer 可以让这个 graph 更好看”的建议
-   对 post 进行 Publish

最初的 editing phase 以前需要将 draft 发布到一个 slack channel，“所有人”都会在那里对 post 进行 comment。这是一种不愉快的体验，因为“所有人”都会提出 comments，因而需要大量 revision。现在这个 process 的设计，就是为了避免收到“太多”的 feedback。

#### Segment

-   有人有了写一篇 post 的想法
    -   想法往往来自：internal docs、external talk、shipped project、open source tooling（由 Segment built）
-   Writer（是一名 engineer）写出一份 draft
    -   可能会有一名 senior eng 与其一起写这份 draft
-   直到不久前，都没有人真正 owned 这个 feedback process
    -   Calvin French-Owen（co-founder）和 Rick（engineering manager）通常会提供大部分 feedback
    -   也可能从 manager 和 eng leadership 那里获得 feedback
    -   通常，第 3rd 份 draft 就被认为完成了
    -   现在有一名 full-time editor，负责 editing posts
-   还会在 eng team 中进行 socialize，从 15-20 人那里获得 feedback
-   PR 和 legal 会看一下，进行 lightweight approval

已经做出的一些改变包括

-   曾有一段时间，为了建立一个 "engineering brand"，将 in-depth technical posts 设为 top-level priority
-   举办过一次 "blogging retreat"，用一周时间写一篇 post
-   将 writing 和 speaking 加入 performance reviews 和 career ladders，作为获得奖励的 explicit criteria

虽然有 legal 和 PR approval，Calvin 指出：“总体而言，我们尽量让它保持相当 lightweight。我认为 blogging 更大的问题是缺少 posts，或 content 是 vague、high level 的，没什么意思，而不是 revealing 太多。”

#### Cloudflare

-   有人有了写一篇 post 的想法
    -   Internal blogging 是 culture 的一部分，有些 posts 来自 internal blog
-   John Graham-Cumming（CTO）会阅读每一篇 post，其他人也会阅读并 comment
    -   John 是 posts 的 approver
-   Matthew Prince（CEO）总体上也支持 blogging
-   legal approval process “非常快”，SLO 为 1 hour
    -   这个 process 非常 lightweight，以至于有一个人并没有真正把它当作 approval，另一个人则完全没有提到它（第三个人确实提到了这一步）
    -   Comms 通常不参与

需要注意的是，这只适用于 technical blog posts。Product announcements 的 process 更繁重，因为它们与 sales material、press releases 等有关联。

我觉得有趣的一点是，Marek 因为 Cloudflare 的 blog 而去那里 interviewed（[这篇 2013 年介绍他们的 4th generation servers 的 blog post 引起了他的注意](https://blog.cloudflare.com/a-tour-inside-cloudflares-latest-generation-servers/)），而现在他既是他们的一名 key engineer，也是精彩的 Cloudflare blog posts 的主要来源之一。到现在，Cloudflare blog 至少又带来了几代这样的人：他们因为看到一篇 blog post 而来 interviewed，如今则为这个 blog 撰写精彩的 posts。

#### 反面例子 #1

-   许多人建议我把这家公司作为正面例子，因为在早期，他们有一个类似上述的 semi-lightweight process
-   唯一让这个 process 变得 non-lightweight 的，是一位 founder 坚持要对 posts 进行 signing off，而且经常大幅 rewrite 它们，但这个 blog 很成功，也是 recruiting 的重要推动力
-   随着公司 scaled up，founder approval 花费的时间越来越长，导致 blog process 出现漫长的 delays
-   后来，由于 leadership 认为这件事很重要，便从外部聘请了一个人来接管 blog publishing process
-   此后，这个 process 充斥着典型的 anti-patterns，approval 要花几个月，还要经历许多轮 iterations 的 changes，让 engineers 感到沮丧，也使他们的 blog posts 不那么吸引人
    -   不止一个人告诉我，在为公司写过一篇 blog post 后，他们就发誓再也不写了，因为这个 process 太痛苦了
    -   好消息是，即使 blog 拥有合理 process 的时代已经过去很久，人们对它曾有过优质 output 的记忆，仍然让许多外部人士对这家公司及其 engineering 抱有正面印象

#### 反面例子 #2

-   我的一位朋友想 publish 一篇 blog post，结果 "comms" 花了六个月才 approve
-   在上述事情发生大约一年后，由于“反面例子 #1”的声誉，“反面例子 #2”聘请了在“反面例子 #1”负责那个 process 的人，让其担任 PR/comms 的 senior position，并负责这家公司的 blogging process。在“反面例子 #1”，此人接手时，那个 blog 开始不再是 engineers 愿意写作的地方；此人也是让 blog process 变得如此 onerous 的主要推动者，以至于 engineers 写过一篇 post 后，就发誓再也不写 blog post
-   聘请那个主导了“反面例子 #1”衰落的人来改善“反面例子 #2”的 process，并没有 streamline 这个 process，也没有让“反面例子 #2”产生更多或更好的 output

我认为，一个让人们[获得一点 feedback](https://danluu.com/p95-skill/) 的 corp eng blog，其 natural state 就是一个相当有趣的 blog。[真正的、in-depth、technical writing 很稀缺](https://twitter.com/rakyll/status/1043952902157459456)，这使得任何关于 technical work 的、还算过得去的、诚实的 public writing 都很有趣。

要让一个 blog 变得无聊，corporation 就得主动阻止 engineers 把有趣的 content 发布出来。不幸的是，大型 corporations 的 natural state 似乎倾向于 risk aversion，并阻止人们写作，以防引发 legal、PR 或其他问题。Individual contributors（ICs）可能会觉得，阻止 engineers 撰写 low-risk technical posts，而与此同时，C-level execs 和 VPs 却经常发表会演变成 PR disasters 的公开言论，这很荒唐；但大公司的 ICs 没有 authority，或者觉得自己没有 authority，仅仅因为某件事有道理就去做。而那些必须对 approving 一个 streamlined process 进行 sign off 的十四位 stakeholders，没有一个关心 streamlining 这个 process，因为它给公司带来的好处并不会真正影响到他们；尤其是当这看起来意味着，要为 streamlined process 所增加的 risk 承担责任时，无论这种 risk 多么小，他们就更不会关心。一位愿意 take a risk 的 exec 或 senior VP 可以为后果承担责任，而且，如果他们关心 engineering recruiting 或 morale，就可能找到这样做的理由。

我经常听到更 bureaucratic 的公司里的人说类似这样的话：“我们这个规模的公司都这样”，但事实并非如此。Cloudflare 是一家价值 $6B、接近 1k 名员工的公司，与许多 blogging process 繁重得多的公司处于同一 size class。corp eng blog 的情况，似乎与提供真正的 interview feedback 的情况相似。[interviewing.io 声称，这样做有显著的 upside，而 downside 很少](http://blog.interviewing.io/no-engineer-has-ever-sued-a-company-because-of-constructive-post-interview-feedback-so-why-dont-employers-do-it/)。有些公司确实会提供真正的 feedback，而且这样做的公司通常发现，这能让它们轻松获得 recruiting 方面的 advantage，几乎没有 downside；但绝大多数公司都不这样做，这些公司里的人还会声称，提供 feedback 是不可能的，因为你会被起诉，或者公司会被“封杀”，尽管提供 feedback 的公司通常并不会遭遇这些事情，甚至有些整个 industries 都普遍提供 interview feedback。含糊地声称存在某种 risk 很容易，而当这种关于 risk 的模糊说辞来自多个 orgs 时，很少有人有 authority 将其驳回。

虽然这里的 sample size 很小，而且从 small samples 进行过多 generalize 是危险的，但需要 high-level support 才能冲破 bureaucracy 这一想法，与我在其他领域的见闻一致：在那些领域，大多数大型公司都很难做到某些容易、价值明显却 diffuse 的事情。虽然这篇 post 恰好讲的是 blogging，但我在许多不同话题上，都听过结构相同的故事。

### 附录：compelling blog posts 的示例

下面是前面提到的 blogs 中的一些 blog posts，并附有简短评论，说明我为什么认为这些 post 是 compelling。这一次，按 reverse sha512 hash order 排列。

#### Cloudflare

-   [https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/](https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/)
    -   以相当深入的方式，讲述了一个 impacted 许多人的真实 technical problem
    -   很及时，在 outage 后仅八小时就 released，当时人们仍然很想了解发生了什么；大多数公司无法这么快地拿出一篇 compelling blog post，或者只有在特殊情况下才能做到，而 Cloudflare 能够时不时地写出及时的 posts
-   [https://blog.cloudflare.com/the-relative-cost-of-bandwidth-around-the-world/](https://blog.cloudflare.com/the-relative-cost-of-bandwidth-around-the-world/)
    -   对一些 data 的 Exploration
-   [https://blog.cloudflare.com/the-story-of-one-latency-spike/](https://blog.cloudflare.com/the-story-of-one-latency-spike/)
    -   一个 debugging 故事
-   [https://blog.cloudflare.com/when-bloom-filters-dont-bloom/](https://blog.cloudflare.com/when-bloom-filters-dont-bloom/)
    -   一个 debugging 故事，这次发生在 developing 一个 data structure 的过程中

#### Segment

-   [https://segment.com/blog/when-aws-autoscale-doesn-t/](https://segment.com/blog/when-aws-autoscale-doesn-t/)
    -   对一个 widely used service 中的 gotcha 所作的 Concrete explanation
-   [https://segment.com/blog/gotchas-from-two-years-of-node/](https://segment.com/blog/gotchas-from-two-years-of-node/)
    -   针对一个 widely used tool 中的 gotcha，给出了 Concrete example 和 explanation
-   [https://segment.com/blog/automating-our-infrastructure/](https://segment.com/blog/automating-our-infrastructure/)
    -   这篇 Post 包含关于一家公司如何 operates 的 specific details；理论上，任何公司都可以写这样的文章，但很少有公司这么做

#### Heap

-   [https://heap.io/blog/engineering/basic-performance-analysis-saved-us-millions](https://heap.io/blog/engineering/basic-performance-analysis-saved-us-millions)
    -   讲述了一个真实的 problem 及其 solution
-   [https://heap.io/blog/engineering/clocksource-aws-ec2-vdso](https://heap.io/blog/engineering/clocksource-aws-ec2-vdso)
    -   讲述了一个真实的 problem 及其 solution
    -   在 HN comments 中，engineers（malisper、kalmar）给出了包含真正 reasons 的 technical responses，而不只是你在大多数情况下看到的那种惯常的遮掩搪塞
-   [https://heap.io/blog/analysis/migrating-to-typescript](https://heap.io/blog/analysis/migrating-to-typescript)
    -   坦诚讲述了第一次尝试 driving 一项 company-wide technical change 是如何 failed 的

需要注意的是，这些 blogs 的 styles 都不相同。就我个人而言，我更喜欢 Cloudflare 的 blog 的 style，其中 "deep dive" technical posts 的 proportion 更高，但不同的人会喜欢不同的 styles。有很多 styles 都能奏效。

感谢 Marek Majkowski、Kamal Marhubi、Calvin French-Owen、John Graham-Cunning、Michael Malis、Matthew Prince、Yuri Vishnevsky、Julia Evans、Wesley Aptekar-Cassels、Nathan Reed、Jake Seliger、一位匿名评论者，以及来自我未点名的那些公司的消息提供者，感谢他们提供的评论、纠正和讨论；致谢中明确提到的所有人，都不是那些不那么 compelling 的 blogs 的信息来源

---

## 原文

I've been comparing notes with people who run corporate engineering blogs and one thing that I think is curious is that it's pretty common for my personal blog to get more traffic than the entire corp eng blog for a company with a nine to ten figure valuation and it's not uncommon for my blog to get an order of magnitude more traffic.

I think this is odd because tech companies in that class often have hundreds to thousands of employees. They're overwhelmingly likely to be better equipped to write a compelling blog than I am and companies get a lot more value from having a compelling blog than I do.

With respect to the former, employees of the company will have done more interesting engineering work, have more fun stories, and have more in-depth knowledge than any one person who has a personal blog. On the latter, my blog helps me with job searching and it helps companies hire. But I only need one job, so more exposure, at best, gets me a slightly better job, whereas all but one tech company I've worked for is desperate to hire and loses candidates to other companies all the time. Moreover, I'm not really competing against other candidates when I interview (even if we interview for the same job, if the company likes more than one of us, it will usually just make more jobs). The high-order bit on this blog with respect to job searching is whether or not the process can take significant non-interview feedback or if [I'll fail the interview because they do a conventional interview](https://danluu.com/algorithms-interviews/) and the marginal value of an additional post is probably very low with respect to that. On the other hand, companies compete relatively directly when recruiting, so being more compelling relative to another company has value to them; replicating the playbook Cloudflare or Segment has used with their engineering "brands" would be a significant recruiting advantage. The playbook isn't secret: these companies broadcast their output to the world and are generally happy to talk about their blogging process.

Despite the seemingly obvious benefits of having a "good" corp eng blog, most corp eng blogs are full of stuff engineers don't want to read. Vague, high-level fluff about how amazing everything is, content marketing, handwave-y posts about the new hotness (today, that might be using deep learning for inappropriate applications; ten years ago, that might have been using "big data" for inappropriate applications), etc.

To try to understand what companies with good corporate engineering blog have in common, I interviewed folks at three different companies that have compelling corporate engineering blogs (Cloudflare, Heap, and Segment) as well as folks at three different companies that have lame corporate engineering blogs (which I'm not going to name).

At a high level, the compelling engineering blogs had processes that shared the following properties:

-   Easy approval process, not many approvals necessary
-   Few or no non-engineering approvals required
-   Implicit or explicit fast [SLO](https://en.wikipedia.org/wiki/Service-level_objective) on approvals
-   Approval/editing process mainly makes posts more compelling to engineers
-   Direct, high-level (co-founder, C-level, or VP-level) support for keeping blog process lightweight

The less compelling engineering blogs had processes that shared the following properties:

-   Slow approval process
-   Many approvals necessary
-   Significant non-engineering approvals necessary
    -   Non-engineering approvals suggest changes authors find frustrating
    -   Back-and-forth can go on for months
-   Approval/editing process mainly de-risks posts, removes references to specifics, makes posts vaguer and less interesting to engineers
-   Effectively no high-level support for blogging
    -   Leadership may agree that blogging is good in the abstract, but it's not a high enough priority to take concrete action
    -   Reforming process to make blogging easier very difficult; previous efforts have failed
    -   Changing process to reduce overhead requires all "stakeholders" to sign off (14 in one case)
        -   Any single stakeholder can block
        -   No single stakeholder can approve
    -   Stakeholders wary of approving anything that reduces overhead
        -   Approving involves taking on perceived risk (what if something bad happens) with no perceived benefit to them

One person at a company with a compelling blog noted that a downside of having only one approver and/or one primary approver is that if that person is busy, it can takes weeks to get posts approved. That's fair, that's a downside of having centralized approval. However, when we compare to the alternative processes, at one company, people noted that it's typical for approvals to take three to six months and tail cases can take a year.

While a few weeks can seem like a long time for someone used to a fast moving company, people at slower moving companies would be ecstatic to have an approval process that only takes twice that long.

Here are the processes, as described to me, for the three companies I interviewed (presented in `sha512sum` order, which is coincidentally ordered by increasing size of company, from a couple hundred employees to nearly one thousand employees):

#### Heap

-   Someone has an idea to write a post
-   Writer (who is an engineer) is paired with a "buddy", who edits and then approves the post
    -   Buddy is an engineer who has a track record of producing reasonable writing
    -   This may take a few rounds, may change thrust of the post
-   CTO reads and approves
    -   Usually only minor feedback
    -   May make suggestions like "a designer could make this graph look better"
-   Publish post

The first editing phase used to involve posting a draft to a slack channel where "everyone" would comment on the post. This was an unpleasant experience since "everyone" would make comments and a lot of revision would be required. This process was designed to avoid getting "too much" feedback.

#### Segment

-   Someone has an idea to write a post
    -   Often comes from: internal docs, external talk, shipped project, open source tooling (built by Segment)
-   Writer (who is an engineer) writes a draft
    -   May have a senior eng work with them to write the draft
-   Until recently, no one really owned the feedback process
    -   Calvin French-Owen (co-founder) and Rick (engineering manager) would usually give most feedback
    -   Maybe also get feedback from manager and eng leadership
    -   Typically, 3rd draft is considered finished
    -   Now, have a full-time editor who owns editing posts
-   Also socialize among eng team, get get feedback from 15-20 people
-   PR and legal will take a look, lightweight approval

Some changes that have been made include

-   At one point, when trying to establish an "engineering brand", making in-depth technical posts a top-level priority
-   had a "blogging retreat", one week spent on writing a post
-   added writing and speaking as explicit criteria to be rewarded in performance reviews and career ladders

Although there's legal and PR approval, Calvin noted "In general we try to keep it fairly lightweight. I see the bigger problem with blogging being a lack of posts or vague, high level content which isn't interesting rather than revealing too much."

#### Cloudflare

-   Someone has an idea to write a post
    -   Internal blogging is part of the culture, some posts come from the internal blog
-   John Graham-Cumming (CTO) reads every post, other folks will read and comment
    -   John is approver for posts
-   Matthew Prince (CEO) also generally supportive of blogging
-   "Very quick" legal approval process, SLO of 1 hour
    -   This process is so lightweight that one person didn't really think of it as an approval, another person didn't mention it at all (a third person did mention this step)
    -   Comms generally not involved

One thing to note is that this only applies to technical blog posts. Product announcements have a heavier process because they're tied to sales material, press releases, etc.

One thing I find interesting is that Marek interviewed at Cloudflare because of their blog ([this 2013 blog post on their 4th generation servers caught his eye](https://blog.cloudflare.com/a-tour-inside-cloudflares-latest-generation-servers/)) and he's now both a key engineer for them as well as one of the main sources of compelling Cloudflare blog posts. At this point, the Cloudflare blog has generated at least a few more generations of folks who interviewed because they saw a blog post and now write compelling posts for the blog.

#### Negative example #1

-   Many people suggested I use this company as a positive example because, in the early days, they had a semi-lightweight process like the above
-   The one thing that made the process non-lightweight was that a founder insisted on signing off on posts and would often heavily rewrite them, but the blog was a success and a big driver of recruiting
-   As the company scaled up, founder approval took longer and longer, causing lengthy delays in the blog process
-   At some point, an outsider was hired to take over the blog publishing process because it was considered important to leadership
-   Afterwards, the process became filled with typical anti-patterns, taking months for approval, with many iterations of changes that engineers found frustrating, that made their blog posts less compelling
    -   Multiple people told me that they vowed to never write another blog post for the company after doing one because the process was so painful
    -   The good news is that, long the era of the blog having a reasonable process, the memory of the blog having good output still gave many outsiders a positive impression about the company and its engineering

#### Negative example #2

-   A friend of mine tried to publish a blog post and it took six months for "comms" to approve
-   About a year after the above, due to the reputation of "negative example #1", "negative example #2" hired the person who ran the process at "negative example #1" to a senior position in PR/comms and to run the the blogging process at this company. At "negative example #1", this person took over when the blog went from being something engineers wanted to write on and was the primary driver of the blog process being so onerous that engineers vowed to never write a blog post again after writing one post
-   Hiring the person who presided over the decline of "negative example #1" to improve the process at "negative example #2" did not streamline the process or result in more or better output at "negative example #2"

My opinion is that the natural state of a corp eng blog where people [get a bit of feedback](https://danluu.com/p95-skill/) is a pretty interesting blog. There's [a dearth of real, in-depth, technical writing](https://twitter.com/rakyll/status/1043952902157459456), which makes any half decent, honest, public writing about technical work interesting.

In order to have a boring blog, the corporation has to actively stop engineers from putting interesting content out there. Unfortunately, it appears that the natural state of large corporations tends towards risk aversion and blocking people from writing, just in case it causes a legal or PR or other problem. Individual contributors (ICs) might have the opinion that it's ridiculous to block engineers from writing low-risk technical posts while, simultaneously, C-level execs and VPs regularly make public comments that turn into PR disasters, but ICs in large companies don't have the authority or don't feel like they have the authority to do something just because it makes sense. And none of the fourteen stakeholders who'd have to sign off on approving a streamlined process care about streamlining the process since that would be good for the company in a way that doesn't really impact them, not when that would mean seemingly taking responsibility for the risk a streamlined process would add, however small. An exec or a senior VP willing to take a risk can take responsibility for the fallout and, if they're interested in engineering recruiting or morale, they may see a reason to do so.

One comment I've often heard from people at more bureaucratic companies is something like "every company our size is like this", but that's not true. Cloudflare, a $6B company approaching 1k employees is in the same size class as many other companies with a much more onerous blogging process. The corp eng blog situation seems similar to situation on giving real interview feedback. [interviewing.io claims that there's significant upside and very little downside to doing so](http://blog.interviewing.io/no-engineer-has-ever-sued-a-company-because-of-constructive-post-interview-feedback-so-why-dont-employers-do-it/). Some companies actually do give real feedback and the ones that do generally find that it gives them an easy advantage in recruiting with little downside, but the vast majority of companies don't do this and people at those companies will claim that it's impossible to do give feedback since you'll get sued or the company will be "cancelled" even though this generally doesn't happen to companies that give feedback and there are even entire industries where it's common to give interview feedback. It's easy to handwave that some risk exists and very few people have the authority to dismiss vague handwaving about risk when it's coming from multiple orgs.

Although this is a small sample size and it's dangerous to generalize too much from small samples, the idea that you need high-level support to blast through bureaucracy is consistent with what I've seen in other areas where most large companies have a hard time doing something easy that has obvious but diffuse value. While this post happens to be about blogging, I've heard stories that are the same shape on a wide variety of topics.

### Appendix: examples of compelling blog posts

Here are some blog posts from the blogs mentioned with a short comment on why I thought the post was compelling. This time, in reverse sha512 hash order.

#### Cloudflare

-   [https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/](https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/)
    -   Talks about a real technical problem that impacted a lot of people, reasonably in depth
    -   Timely, released only eight hours after the outage, when people were still really interested in hearing about what happened; most companies can't turn around a compelling blog post this quickly or can only do it on a special-case basis, Cloudflare is able to crank out timely posts semi-regularly
-   [https://blog.cloudflare.com/the-relative-cost-of-bandwidth-around-the-world/](https://blog.cloudflare.com/the-relative-cost-of-bandwidth-around-the-world/)
    -   Exploration of some data
-   [https://blog.cloudflare.com/the-story-of-one-latency-spike/](https://blog.cloudflare.com/the-story-of-one-latency-spike/)
    -   A debugging story
-   [https://blog.cloudflare.com/when-bloom-filters-dont-bloom/](https://blog.cloudflare.com/when-bloom-filters-dont-bloom/)
    -   A debugging story, this time in the context of developing a data structure

#### Segment

-   [https://segment.com/blog/when-aws-autoscale-doesn-t/](https://segment.com/blog/when-aws-autoscale-doesn-t/)
    -   Concrete explanation of a gotcha in a widely used service
-   [https://segment.com/blog/gotchas-from-two-years-of-node/](https://segment.com/blog/gotchas-from-two-years-of-node/)
    -   Concrete example and explanation of a gotcha in a widely used tool
-   [https://segment.com/blog/automating-our-infrastructure/](https://segment.com/blog/automating-our-infrastructure/)
    -   Post with specific details about how a company operates; in theory, any company could write this, but few do

#### Heap

-   [https://heap.io/blog/engineering/basic-performance-analysis-saved-us-millions](https://heap.io/blog/engineering/basic-performance-analysis-saved-us-millions)
    -   Talks about a real problem and solution
-   [https://heap.io/blog/engineering/clocksource-aws-ec2-vdso](https://heap.io/blog/engineering/clocksource-aws-ec2-vdso)
    -   Talks about a real problem and solution
    -   In HN comments, engineers (malisper, kalmar) have technical responses with real reasons in them and not just the usual dissembling that you see in most cases
-   [https://heap.io/blog/analysis/migrating-to-typescript](https://heap.io/blog/analysis/migrating-to-typescript)
    -   Real talk about how the first attempt at driving a company-wide technical change failed

One thing to note is that these blogs all have different styles. Personally, I prefer the style of Cloudflare's blog, which has a higher proportion of "deep dive" technical posts, but different people will prefer different styles. There are a lot of styles that can work.

Thanks to Marek Majkowski, Kamal Marhubi, Calvin French-Owen, John Graham-Cunning, Michael Malis, Matthew Prince, Yuri Vishnevsky, Julia Evans, Wesley Aptekar-Cassels, Nathan Reed, Jake Seliger, an anonymous commenter, plus sources from the companies I didn't name for comments/corrections/discussion; none of the people explicitly mentioned in the acknowledgements were sources for information on the less compelling blogs