# Slashdot 和 Sourceforge｜Slashdot and Sourceforge

---

## 正文

如果你在过去一周（2015 年 5 月 24 日所在的那一周）关注过任何 tech news aggregator，你大概已经看到那则报道，讲的是 [SourceForge](https://plus.google.com/+gimp/posts/cxhB1PScFpe) 如何 taking over 现有 projects 的 admin accounts，并在 GIMP 等 packages 的 installers 中 injecting adware。对于没有关注这则报道的人来说，SourceForge 提供带有 adware 的 installers 已有很长历史，但它们过去是 opt-in 的。如今看来，这一 process 对许多 projects 已经是 mandatory 的了。

自从 SourceForge 添加了一项允许 projects 对 adware bundling 进行 opt-in 的 feature，人们就一直对它心存戒备，但你至少还可以说，projects 是自愿这么做的。然而，如今 SourceForge 显然正在采取 malicious 行为，已经把十六年 operating 期间积累起来的所有 user trust 消耗殆尽。任何明白其中门道的人，都不会再从 SourceForge download 任何东西了。如果 search engines 开始因 SourceForge distributing adware 而对它进行 penalizing，那么它们甚至无法从尚未看到这则报道的人那里获得 traffic，这基本上会抹去它们的全部 value。

每当听到这样的事情，我都会惊讶于 destroy user trust 竟然可以如此迅速，以及 destroy 一个 brand 比 create 一个 brand 容易得多。说到这里，看到 Slashdot（它与 SourceForge 同属一家公司）也试图 destroy 自己的 brand，实在有些好笑。它们是唯一一家没有报道此事的主要 tech news aggregator，这是因为它们把别人提交的[每一则报道都 buried 了](http://www.reddit.com/r/programming/comments/37xbzt/goodbye_sourceforge/crqpnzo)。这促使人们开始在其他报道下 submitting 关于此事的 comments。

![Slashdot 上一条关于 SourceForge、获得大量 upvoted 的 comment](https://danluu.com/images/slashdot-sourceforge/slashdot_sourceforge.webp)

我觉得这实在难以置信。怎么会有人在某个地方认为，在 Slashdot 上 censoring SourceForge 的 adware bundling，会给拥有 Slashdot 和 SourceForge 的 holding company——Slashdot Media——带来 net positive？在 Google 或 Google News 上稍作 search 就能发现，多家主要 tech publications 已经报道了此事，因此即使在 best case 下，suppressing 这则报道的 value 也几乎是 zero。而在 worst case 下，这种 censorship 会制造另一个 [Digg moment](http://yro.slashdot.org/story/06/04/20/1538256/growing-censorship-concerns-at-digg)[1](https://danluu.com/slashdot-sourceforge/#fn:1)：读者不再信任 moderators，转而前往没有受到如此严重 censored 的 sites。这么做基本没有 upside，却存在相当大的 downside risk。

我能理解拥有 Slashdot Media 的 holding company——DHI——为什么想做点什么。它们上一份 earnings report 表明，Slashdot Media 的情况并不好，而它们最不需要的，就是 bad publicity 把人们从 Slashdot 赶走：

> 截至 March 31, 2015 的 quarter，Corporate & Other segment 的 revenues decreased 了 6%，至 \$4.5 million，反映出 Slashdot Media 某些 revenue streams 的 decline。

把这个数字与它们在 Q4 2012 的 post-acquisition revenue 作 Compare，那是 DHI purchased Slashdot Media 后的第一个 quarter：

> 来自 Slashdot Media acquisition 的 Revenues totaled 为 \$52.7 . . . including \$4.7 million

“Corporate & Other”似乎不只涵盖 Slashdot Media。尽管如此，而且它们还在竭尽所能地从 SourceForge 榨取 short-term revenue，整个“Corporate & Other”的表现依然不如 2012 年单独一个 Slashdot Media[2](https://danluu.com/slashdot-sourceforge/#fn:2)。它们最初宣称，对 SourceForge 和 Slashdot 的计划是[“让它们基本保持原样，\[因为我们\]非常在意，不要 disrupting users 使用它们的方式 . . .”](http://www.theverge.com/2012/9/18/3351970/dice-holdings-geeknet-slashdot-careers-news)，但没过多久，它们就意识到这行不通；以下是它们 2013 年 earnings report 中的一段：

> 过去一年中，advertising revenue 已经 declined，而且预计 Slashdot Media 的 underlying advertising business 在未来的 financial performance 上不会有 improvement。因此，与 Slashdot Media 相关的 \$7.2 million of intangible assets and \$6.3 million goodwill 被 reduced to zero。

我记得就在那之后不久，SourceForge 开始为那些 opted in 的 projects 尝试 adware/malware bundlers，不知怎么就一步步走到了今天这般境地。

我能理解想做点什么来帮助 Slashdot Media 的愿望，但很难看出，permanently damaging Slashdot 的 reputation 能有什么帮助。据我所见，它们已经退回到这个经典的 syllogism：“我们必须做点什么。这就是点什么。我们必须做这件事。”

_更新：_ Sourceforge/GIMP 的报道如今终于登上了 Slashdot，比它在其他各处出现晚了一周，也比本文写成晚了一天。报道还附了一段说明，说 editor 刚过完周末回来，就发现人们“因为我们在‘burying’这则报道而大惊小怪”，如此轻描淡写，仿佛要不是周末，这则报道早就 posted 了。这个借口并不怎么令人信服：自 Sourceforge/GIMP 的消息上周三传出以来，各位 editors 已经 posted 了数十则报道，其中就包括最终撰写 Sourceforge/GIMP 那篇 post 的人。这个“周末”的借口尤其站不住脚，因为到了下一个周三，Sourceforge/nmap 的消息传出时，Slashdot 正因先前的延误受到严密审视，它们却能几乎立即在当天 publish 那则报道，尽管上一次消息在周三传出时，那天还被算作“周末”的开始。此外，Slashdot 的报道[十分小心](http://www.reddit.com/r/programming/comments/386c75/sourceforge_locked_in_projects_of_fleeing_users/crstuof)地使用了“modified binary”和“present third party offers”之类的 terms，而没有使用“malware”或“adware”。

当然，这一切也可能只是一场无心的误会，我也怀疑我们是否有朝一日能掌握足够的信息，确知究竟是哪种情况。但 Slashdot posted 的借口，确实不怎么能让人产生信心。

* * *

1.  讽刺的是，如果你打开那个 link，就会看到 Slashdot 的创始人 CmdrTaco 反对“content 因批评 sponsors 而 getting removed”。这并不是说 Slashdot 当年没有 biased；它过去因自己的 pro-Linux pro-open source anti-MS anti-commercial bias 而臭名昭著。如果你读完那个 link 中的 comments，就会看到，很多人在 upvoting 某个违背 Slashdot 固有 bias 的 viewpoint 后，失去了 voting abilities。但也正是 Slashdot 的 bias，才让它遗漏这则报道这件事如此引人注目。这恰恰是 Slashdot 的 readers 和 moderators 平时会大做文章的那类事情。但 CmdrTaco 已经离开多年，昔日的 Slashdot 也早已不在了。[\[返回\]](https://danluu.com/slashdot-sourceforge/#fnref:1)
2.  如果你想 compare YoY results，Slashdot Media 在 Q1 2013 获得了 $4M。[\[返回\]](https://danluu.com/slashdot-sourceforge/#fnref:2)

---

## 原文

If you've followed any tech news aggregator in the past week (the week of the 24th of May, 2015), you've probably seen the story about how [SourceForge](https://plus.google.com/+gimp/posts/cxhB1PScFpe) is taking over admin accounts for existing projects and injecting adware in installers for packages like GIMP. For anyone not following the story, SourceForge has a long history of adware laden installers, but they used to be opt-in. It appears that the process is now mandatory for many projects.

People have been wary of SourceForge ever since they added a feature to allow projects to opt-in to adware bundling, but you could at least claim that projects are doing it by choice. But now that SourceForge is clearly being malicious, they've wiped out all of the user trust that was built up over sixteen years of operating. No clueful person is going to ever download something from SourceForge again. If search engines start penalizing SourceForge for distributing adware, they won't even get traffic from people who haven't seen this story, wiping out basically all of their value.

Whenever I hear about a story like this, I'm amazed at how quickly it's possible to destroy user trust, and how much easier it is to destroy a brand than to create one. In that vein, it's funny to see Slashdot (which is owned by the same company as SourceForge) also attempting to destroy their own brand. They're the only major tech news aggregator which hasn't had a story on this, and that's because they've [buried every story](http://www.reddit.com/r/programming/comments/37xbzt/goodbye_sourceforge/crqpnzo) that someone submits. This has prompted people to start submitting comments about this on other stories.

![A highly upvoted comment about SourceForge on Slashdot](https://danluu.com/images/slashdot-sourceforge/slashdot_sourceforge.webp)

I find this to be pretty incredible. How is it possible that someone, somewhere, thinks that censoring SourceForge's adware bundling on Slashdot is a net positive for Slashdot Media, the holding company that owns Slashdot and SourceForge? A quick search on either Google or Google News shows that the story has already made it to a number of major tech publications, making the value of suppressing the story nearly zero in the best case. And in the worst case, this censorship will create another [Digg moment](http://yro.slashdot.org/story/06/04/20/1538256/growing-censorship-concerns-at-digg)[1](https://danluu.com/slashdot-sourceforge/#fn:1), where readers stop trusting the moderators and move on to sites that aren't as heavily censored. There's basically no upside here and a substantial downside risk.

I can see why DHI, the holding company that owns Slashdot Media, would want to do something. Their last earnings report indicated that Slashdot Media isn't doing well, and the last thing they need is bad publicity driving people away from Slashdot:

> Corporate & Other segment revenues decreased 6% to $4.5 million for the quarter ended March 31, 2015, reflecting a decline in certain revenue streams at Slashdot Media.

Compare that to their post-acquisition revenue from Q4 2012, which is the first quarter after DHI purchased Slashdot Media:

> Revenues totaled $52.7 . . . including $4.7 million from the Slashdot Media acquisition

“Corporate & Other” seems to encompass more than just Slashdot Media. And despite that, as well as milking SourceForge for all of the short-term revenue they can get, all of “Corporate & Other” is doing worse than Slashdot Media alone in 2012[2](https://danluu.com/slashdot-sourceforge/#fn:2). Their original stated plan for SourceForge and Slashdot was ["to keep them pretty much the same as they are \[because we\] are very sensitive to not disrupting how users use them . . ."](http://www.theverge.com/2012/9/18/3351970/dice-holdings-geeknet-slashdot-careers-news), but it didn't take long for them realize that wasn't working; here's a snippet from their 2013 earnings report:

> advertising revenue has declined over the past year and there is no improvement expected in the future financial performance of Slashdot Media's underlying advertising business. Therefore, $7.2 million of intangible assets and $6.3 million of goodwill related to Slashdot Media were reduced to zero.

I believe it was shortly afterwards that SourceForge started experimenting with adware/malware bundlers for projects that opted in, which somehow led us to where we are today.

I can understand the desire to do something to help Slashdot Media, but it's hard to see how permanently damaging Slashdot's reputation is going to help. As far as I can tell, they've fallen back to this classic syllogism: “We must do something. This is something. We must do this.”

_Update:_ The Sourceforge/GIMP story is now on Slashdot, the week after it appeared everywhere else and a day after this was written, with a note about how the editor just got back from the weekend to people "freaking out that we're 'burying' this story", playing things down to make it sound like this would have been posted if it wasn't the weekend. That's not a very convincing excuse when tens of stories were posted by various editors, including the one who ended up making the Sourceforge/GIMP post, since the Sourceforge/GIMP story broke last Wednesday. The "weekend" excuse seems especially flimsy since when the Sourceforge/nmap story broke on the next Wednesday and Slashdot was under strict scrutiny for the previously delay, they were able to publish that story almost immediately on the same day, despite it having been the start of the "weekend" the last time a story broke on a Wednesday. Moreover, the Slashdot story is [very careful](http://www.reddit.com/r/programming/comments/386c75/sourceforge_locked_in_projects_of_fleeing_users/crstuof) to use terms like "modified binary" and "present third party offers" instead of "malware" or "adware".

Of course this could all just be an innocent misunderstanding, and I doubt we'll ever have enough information to know for sure either way. But Slashdot's posted excuse certainly isn't very confidence inspiring.

* * *

1.  Ironically, if you follow the link, you'll see that Slashdot's founder, CmdrTaco, is against “content getting removed for being critical of sponsors”. It's not that Slashdot wasn't biased back then; Slashdot used to be notorious for their pro-Linux pro-open source anti-MS anti-commercial bias. If you read through the comments in that link, you'll see that a lot of people lost their voting abilities after upvoting a viewpoint that runs against Slashdot's inherent bias. But it's Slashdot's bias that makes the omission of this story so remarkable. This is exactly the kind of thing Slashdot readers and moderators normally make hay about. But CmdrTaco has been gone for years, as has the old Slashdot. [\[return\]](https://danluu.com/slashdot-sourceforge/#fnref:1)
2.  If you want to compare YoY results, Slashdot Media pulled in $4M in Q1 2013. [\[return\]](https://danluu.com/slashdot-sourceforge/#fnref:2)