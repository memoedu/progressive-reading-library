# 从指标中获取更多价值的简单方法｜A simple way to get more value from metrics

---

## 正文

我们花了一天[1](https://danluu.com/metrics-analytics/#fn:D)来 building 一个 system，它立即 found 了一项 mid 7 figure optimization（后来最终 shipping 了）。在第一年，我们因此 shipped 了每年价值 mid 8 figures 的 cost savings。这个 system 引入的 key feature，是能够跨所有 hosts、所有 services，以及任意一段时间（自 inception 起）query metrics data，因此我们在内部把它叫作 LongTermMetrics (LTM)，因为我喜欢平淡、描述性的名字。

这件事始于我寻找一个入门 project 的时候：它既能帮助我理解 Twitter infra stack，又能有一些容易 quantifiable 的 value。Andy Wilcox 建议查看一些 large services 的 [JVM survivor space](https://stackoverflow.com/a/37102630/334816) utilization。如果你不熟悉 survivor space 是什么，可以把它想成 JVM 中的一个 configurable、fixed-size buffer（至少在你使用 Twitter 的 default GC algorithm 时如此）。当时，如果你查看一个随机的 large services，通常会发现以下两种情况之一：

1.  这个 buffer 太 small，导致 poor performance，在 high load 下有时会 catastrophically poor。
2.  这个 buffer 太 large，导致 wasted memory，也就是浪费钱。

但是，与其查看随机的 services，并没有什么根本理由使我们不能 query 所有 services，并得到一份 list，列出哪些 services 的 configuration 有改进空间，再按 performance degradation 或 cost savings 来 sorted。而且，如果我们为 JVM survivor space 写出这样的 query，那么其他 configuration parameters（例如其他 JVM parameters、CPU quota、memory quota 等）也同样适用。由于 data consistency 和 performance 问题交织在一起，写出一个适用于所有 services 的 query，结果比我原先希望的稍难一些。Data consistency 问题包括以下情况：

-   任意一个给定的 metric 都可能有 ~100 个 names，例如，我为 JVM survivor space 找到了 94 个不同的 names
    -   我怀疑还有更多，这些只是我通过一次 simple search 能找到的
-   同一个 metric name 对不同的 services 可能有不同的 meaning
    -   可能是 counter，也可能是 gauge
    -   可能有不同的 units，例如 bytes vs. MB 或 microseconds vs. milliseconds
-   Metrics 有时会被 tagged 上 incorrect service name
-   即使 cluster manager 已经 started up 了该 shard 的一个新 instance，Zombie shards 仍可能继续 operate 并 report metrics，从而使某个 shard name 出现 duplicate 和 inconsistent metrics

我们的 metrics database，[MetricsDB](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/metricsdb.html)，是专为 handle monitoring、dashboards、alerts 等而设计的，并不 support general queries。这完全合理，因为在 Maslow's hierarchy of observability needs 中，monitoring 和 dashboards 比 general metrics analytics 更基础。从与其他公司人员的私下讨论来看，围绕 MetricsDB 的整套 systems 似乎已经解决了许多困扰其他类似 scale 公司的问题，但这种 specialization 意味着我们无法针对 MetricsDB 中的 metrics 来 run arbitrary SQL queries。

另一种 query data 的方法，是使用被 written 到 [HDFS](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS) 中、采用 [Parquet](https://en.wikipedia.org/wiki/Apache_Parquet) format 的 copy，这让人们能够 run arbitrary SQL queries（也可以 write [Scalding](https://en.wikipedia.org/wiki/Cascading_\(software\)#cite_ref-27) (MapReduce) jobs 来 consume 这些 data）。

遗憾的是，由于 metric names 的数量太多，HDFS 上的 data 无法以每个 name 对应一个 column 的 columnar format 来 stored——如果你给 [Presto](https://en.wikipedia.org/wiki/Presto_\(SQL_query_engine\)) 太多 columns，它就会不高兴，而我们不同 metrics 的数量已经远远超过这个 limit。如果你不使用 columnar format（也不采用其他技巧），那么任何 non-trivial query 最终都会 reading 大量 data。结果就是，你无法跨所有 services 或所有 hosts 来 run 任何 non-trivial query（甚至许多 trivial queries），而不让它 time out。我们对 Scalding 没有类似的 timeouts，但 Scalding performance 差得多，一个针对一天 metrics 的 simple Scalding query 通常需要三到二十小时，具体取决于 cluster load，因此用 Scalding 做任何 exploratory data analysis 都不合理。

考虑到已有的 data infrastructure，同时解决这两个问题的一种简单方法，是 write 一个 Scalding job，store 我们在 performance 或 capacity 相关 queries 中关心的那 0.1% 到 0.01% 的 metrics data，并将其 re-write 为 columnar format。我猜至少 90% 的 metrics 都是几乎任何人在几乎任何情况下都不想看的东西，而在真正有人关心的 metrics 中，绝大多数又与 performance 无关。这样做的一个令人高兴的 side effect 是，由于 relevant 的 data 只占很小一部分，indefinitely store 它们的成本很低。标准的 metrics data dump 会在几周后被 deleted，因为它的体积足够大，indefinitely store 的成本将高得难以承受；更长的 metrics memory 对 capacity planning，或者其他倾向于使用 historical data 的 analyses 会有帮助。

我们正在 saving 的 data 包括（但不限于）每个 service 的每个 shard 的以下内容：

-   各种 buffers 的 utilizations 和 sizes
-   CPU、memory 及其他 utilization
-   threads 的 number、context switches、core migrations
-   各种 queue depths 和 network stats
-   JVM version、feature flags 等
-   [GC](https://en.wikipedia.org/wiki/Garbage_collection_\(computer_science\)) 的 stats
-   [Finagle](https://github.com/twitter/finagle) 的 metrics

而对于每个 host：

-   来自 [procfs](https://en.wikipedia.org/wiki/Procfs) 的各种内容，例如 `iowait` time、`idle` 等
-   这台 machine 属于哪个 cluster
-   host-level info，例如 NIC speed、host 上 cores 的 number、memory，
-   关于“health”问题的 host-level stats，例如 thermal throttling、machine checks 等
-   OS version、host-level software versions、host-level feature flags 等
-   [Rezolus](https://github.com/twitter/rezolus) 的 metrics

对于我们知道很少 change 的东西（例如 host NIC speed），我们每天 store 一次，但其中大多数都以与我们其他 metrics 相同的 frequency 和 granularity 来 stored。在某些情况下，这显然很浪费（例如 JVM tenuring threshold，它在一个 service 的每个 shard 上通常都是 identical，而且很少 changes），但考虑到我们围绕 metrics 已有的 infra，这是处理此事最简单的方法。

虽然这个 project 的起因是弄清哪些 services 的 JVM survivor space 存在 under 或 over configured 的情况，但它是从 GC 和 container metrics 开始的，因为这些显然是值得查看的东西，此后我们一直在 incrementally adding 其他 metrics。为了让你了解我们能 query 哪些东西，以及只要懂一点 SQL，queries 就能有多简单，下面给出一些 examples：

#### 非常高的 p90 JVM Survivor Space

这是最初寻找 under/over-provisioned services 这一目标的一部分。任何具有非常高的 p90 JVM survivor space utilization 的 service，在 survivor space 上都可能是 under-provisioned。同样，任何在 peak load 下具有非常低的 p99 或 p999 JVM survivor space utilization 的 service，都可能是 overprovisioned（这里没有展示 query，但我们可以将 query 的 scope 限定在 high load 的时段）。

针对所有 services 中非常高的 p90 survivor space 的 Presto query 如下：

```
with results as (
  select servicename,
    approx_distinct(source, 0.1) as approx_sources, -- number of shards for the service
    -- real query uses [coalesce and nullif](https://prestodb.io/docs/current/functions/conditional.html) to handle edge cases, omitted for brevity
    approx_percentile(jvmSurvivorUsed / jvmSurvivorMax, 0.90) as p90_used,
    approx_percentile(jvmSurvivorUsed / jvmSurvivorMax, 0.50) as p50_used,
  from ltm_service 
  where ds >= '2020-02-01' and ds <= '2020-02-28'
  group by servicename)
select * from results
where approx_sources > 100
order by p90_used desc
```

我们不必逐一查看一大堆 dashboards，只需获取一个 list，然后向相应团队发送带有 config changes 的 diffs，或者编写一个 script，接收 query 的 output 并 automatically writes the diff。上面的 query 为任何基本 utilization numbers 或 rates 提供了一个 pattern；你可以用类似的 queries 查看 memory usage、new or old gen GC frequency 等。有一次，我们发现一个 service 浪费的 RAM 所对应的费用足够支付我十年的工资。

我已经逐渐不再使用针对 simple percentiles 的 thresholds 来寻找 issues，但我在这里展示这个 query，是因为这是人们经常想做、而且有用的事情，我也不必花很多篇幅解释为什么这样做是合理的；至于我更倾向采用的方法，则超出了本文的范围，可能值得单独写一篇文章。

#### Network utilization（网络利用率）

上面的 query 面向所有 services，但我们也可以跨 hosts 进行 query。此外，我们还可以执行与 host 的 properties、feature flags 等进行 join 的 queries。

通过一组 queries，我们得以确定，有相当多的 services 正在触及 network limits，尽管 host-level network utilization 很低。随后，compute platform 团队对 network caps 的一项 change 进行了 gradual rollout，我们通过类似下面这样的 queries 进行 monitored，以确定没有出现任何 performance degradation（从理论上说，如果 increasing network caps 导致 hosts 或 switches 触及 network limits，就可能出现这种情况）。

随着这次 network change，我们观察到了更小的 queue depths、更小的 queue size（以 bytes 计）、更少的 packet drops 等。

为了简洁，下面的 query 仅展示 queue depths；要加入提到的所有 quantities，只需多输入一些 names。

概括来说，对于任何一次 platform 或 service-level feature 的 rollout，我们都可以查看它对实际 services 的 impact。

```
with rolled as (
 select
   -- rollout was fixed for all hosts during the time period, can pick an arbitrary element from the time period
   arbitrary(element_at(misc, 'egress_rate_limit_increase')) as rollout,
   hostId
 from ltm_deploys
 where ds = '2019-10-10'
 and zone = 'foo'
 group by ipAddress
), host_info as(
 select
   arbitrary(nicSpeed) as nicSpeed,
   hostId
 from ltm_host
 where ds = '2019-10-10'
 and zone = 'foo'
 group by ipAddress
), host_rolled as (
 select
   rollout,
   nicSpeed,
   rolled.hostId
 from rolled
 join host_info on rolled.ipAddress = host_info.ipAddress
), container_metrics as (
 select
   service,
   netTxQlen,
   hostId
 from ltm_container
 where ds >= '2019-10-10' and ds <= '2019-10-14'
 and zone = 'foo'
)
select
 service,
 nicSpeed,
 approx_percentile(netTxQlen, 1, 0.999, 0.0001) as p999_qlen,
 approx_percentile(netTxQlen, 1, 0.99, 0.001) as p99_qlen,
 approx_percentile(netTxQlen, 0.9) as p90_qlen,
 approx_percentile(netTxQlen, 0.68) as p68_qlen,
 rollout,
 count(*) as cnt
from container_metrics
join host_rolled on host_rolled.hostId = container_metrics.hostId
group by service, nicSpeed, rollout
```

#### 其他变得容易回答的问题

-   X 的 latency、CPU usage、CPI 或其他 performance impact 是什么？
    -   对我们为每个 container 所 monitor 的 performance counters 的数量进行 Increasing 或 decreasing
    -   对 kernel parameters 进行 Tweaking
    -   OS 或其他 releases
    -   对 host-level oversubscription 进行 Increasing 或 decreasing
    -   一般的 host-level load
    -   Retry budget exhaustion（重试预算耗尽）
-   对于上面相关的各项，X 在一般情况下或某些特定情况下的 distribution 是什么？
-   在 controlling for load 等之后，哪些 hosts 上的每个 service 都有异常差的 service-level performance？
    -   这通常最终被发现是由 hardware misconfiguration 或 fault 引起的
-   除了对 host-level load 的一般 impact 之外，哪些 services 还会与其他 services 相互干扰？
-   failover 或其他 high-load events 的 latency impact 是什么？
    -   考虑到未来的某次 high-load event 加上当前的 growth，我们应该预期未来会有怎样的 load 水平？
    -   哪些 services 在 failover 期间会遇到更多 load，哪些 services 会遇到 unchanged load，哪些则介于两者之间？
-   对于任何 fixed sized buffer 或 allocation，我们可以做出哪些 config changes，能够在不 increasing cost 的情况下 improve performance，或在不 degrading performance 的情况下 reduce cost？
-   对于某个特定的 host-level health problem，如果我们看到它出现 N 次，它 recurs 的 probability 是多少？
-   等等，如果你可以针对 historical metrics data 编写 arbitrary queries，就有很多问题会变得容易回答

#### Design decisions（设计决策）

LTM 大概已经是尽可能平淡无奇的 system 了。每个 design decision 都是选择阻力最小的路径所自然产生的结果。

-   为什么使用 Scalding？
    -   它是 Twitter 的标准选择，而且 integration 让一切都变得很简单。我试过 Spark，它有一些优势。不过在当时，我本来需要手动完成的 integration 工作，用 Scalding 就能直接获得，无须额外付出。
-   为什么使用 Presto，而不是像 Druid 这样允许 live slice & dice queries 的东西？
    -   [Rebecca Isaacs 和 Jonathan Simms 当时正在做与 tracing 相关的工作](https://danluu.com/tracing-analytics/)，我们知道自己会希望在 LTM 与他们创建的东西之间进行 joins。用 Presto 做这件事很简单，但如果使用 Druid 之类的东西，至少在当时，就需要更多规划和工作。
    -   George Sirois 将 data 的一个 subset imported 到 Druid 中，让我们可以试用，它提供的功能非常不错；或许值得以后再看看
-   为什么不使用 Postgres 或类似的东西？
    -   我们想要 store 的 data 量，意味着不投入大量精力就无法实现这一点；即便 data storage 的 cost 很低，这仍然是一个 "big data" problem
-   为什么使用 Parquet，而不是更 efficient 的 format？
    -   在标准 supported formats 中，它最合适（另一个主要的 suppported format 是 raw thrift），引入一个新的 format 会是一个比本项目大得多的项目
-   为什么这个 system 不是 real-time 的（delays 至少为一小时）？
    -   在 Twitter 的 batch job pipeline 上进行构建很容易，只需要读一些介绍其工作方式的教程，然后写一个类似的东西，只是使用不同的 business logic。
    -   在我加入 Twitter 前几年，有人写过一份很不错的提案，计划为 metrics data 构建一个 real-time analytics pipeline，但它始终没有建成，因为（据我估计）产出一个 MVP 需要一到四个季度的工作，而且不清楚哪个团队既有合适的职责来做这件事，又有 4 个季度可用的 headcount。但添加一个 batch job 只花了一天，你不需要为一天的工作召开 roadmap 和 planning 会议，可以直接做，然后 incrementally 完成后续工作。
    -   如果我们在寻找 misconfigurations 或 optimization opportunities，它们很少会在一小时内消失（即使消失了，它们的 total impact 也一定很小），而事实上，它们往往会持续数月乃至数年，因此放弃 real-time 并不会让我们损失太多（我们确实会失去将其 output 用于某些 monitoring use cases 的能力）
    -   real-time version 本来会是一个有着显著 operational cost 的 system，无法由一个人在不承受过重负担的情况下 operated。这个 system 的 operational/maintenance burden 比我希望的要多，平均每个月大概需要花费我 1-2 天的时间，到现在，这已经占了这个 system 的 total cost 中相当大的一部分，但它从不 pages，而且这些工作量一个人就能轻松应付。

#### 平淡无奇的 technology

我认为，撰文介绍这样的 systems，也就是那些平淡无奇的工作，实在被低估了。我读到的文章和听到的演讲中，介绍使用热门 technologies 的 systems 的比例高得不成比例。我并不反对热门的新 technologies，但许多有用的工作，来自把平淡无奇的 technologies 拼接起来，并且做显而易见的事。由于介绍平淡工作 的文章和演讲相对少见，我觉得，把这样的工作写出来，其用处大得出乎意料。

例如，几年前，在 Matt Singer 为与我们规模相当的公司组织的一次讨论 infrastructure 的本地聚会上（基本上就是比 FB/Amazon/Google 小的公司），我问有没有人在做与我们刚完成的工作类似的事。在场没有人在做（至少没有人承认），而且两家不同公司的 engineers 对我们能够 store 如此多的 data 感到震惊：不仅有每个 time period 的 average，还有一些 histogram information。这项工作太直接、太显而易见，算不上 novel；我相信，许多地方都有人 built 过类似的 systems。它真的就只是以一种允许 interactive queries 的 format，将 metrics data 无限期 storing 在 HDFS 上（或者，如果你喜欢更 general 的 term，就是 data lake）。

如果你算一算，在与我们规模相当的公司中，这样一个项目的 metrics data storage 的 cost，就会发现 storage cost 基本上只是个 rounding error。我们 shipped 过的单个 diffs，就足以轻松支付几十年的 storage cost。人们一直在 deploy 贵得多的 analytics 和 observability tools，我不觉得 storing 几年、甚至十年的 metrics 有什么值得震惊的。然而，这件事确实让人惊讶，部分原因就是人们不会把这么平淡的工作写出来。

另一个不相关的例子是，前些时候，我遇到一家规模相近公司的员工，他想从他们的 metrics data 中获得类似的 insights。他们没有从像这个项目一样一天就能完成的工作入手，而是从 deep learning 入手。虽然我认为，对 infra metrics 进行 applying ML and/or stats 是有价值的，但他们把一个几个 person-days 就能为公司带来显著价值的项目，变成了一个需要 person-years 的项目。而且，如果你只打算 _要么_ apply 由具备 infra 经验的人指导的 simple heuristics 和 simple statistical models，_要么_ naively apply deep learning，我认为前者的 ROI 高得多。将 sophisticated stats/ML _与_ practitioner guided heuristics 一起 Applying，能得到比单独使用任何一方更好的 results；但我认为，先从一天就能 build out、再花一两天就能开始 apply 的 simple project 入手，比从需要几个月乃至几年才能 build out 并开始 apply 的项目入手，要合理得多。不过，许多偏向都在促使人们选择更大的项目：它能成为更好看的简历条目（deep learning！），在许多地方也能成为更有力的晋升材料，而且，人们更愿意为使用 deep learning 的酷炫 system 做演讲或写博客文章。

上面讨论了为什么把工作写出来对整个行业有价值。[我们在之前的一篇文章中讨论过，为什么把工作写出来对撰写文章的公司有价值](https://danluu.com/corp-eng-blogs/)，所以这里就不再重复了。

#### 附录：我搞砸的事情

[我觉得，只有通过私下交流才能听说 systems 的缺点，是件遗憾的事](https://twitter.com/danluu/status/1220228489522974721)，所以下面是我做过的一些事，回头看都是相当明显的错误。如果以后回头看又发现了其他明显的错误，我会继续补充。

-   只有 final output tables，而没有 two-stage process
    -   这里的一个问题是，当我发现 bug 时，最后会 re-writing data in place，这种做法是 destructive 的，而且可能使以后无法 fixing 某些 kinds of bugs
    -   另一个问题是，我想让 data 尽可能 clean，但大多数想 add 一个 table 的人，都会 add 一个存在各种问题的 table（例如，本应 beween 0 和 1 的 value，会有百分之几的 values 落在这个 range 之外，而且经常是像 2^32 或更大这样的 very large values，导致 tail percentile 或 average 之类的 queries 变得 meaningless）。我拒绝了这些 table additions，并会请大家 clean up 这些 tables，让它们大致与现有 tables 一样 clean。更好的做法是让大家 add 一些 "beta" tables 之类的东西，之后我再 clean up。这也很契合那个我本来就应该采用的 two-stage approach
        -   有几个人愿意忍受我对 data 的吹毛求疵，并 contributed 了一些 tables；其中一个由 Kunal Trivedi contributed 的 table，是 impact 最高的 tables 之一（以针对该 table 的 queries 带来的 cost savings / profit 衡量），但大多数人都没兴趣把东西 cleaning 得足够好，以达到我设定的标准。如果允许一些 tables 先达到较低的标准，再由我以后 clean up（甚至永远不 clean up），很可能几乎不增加 cost，就能带来显著更高的 total impact
-   没有对几乎所有东西都 using 一个 double
    -   在理想的情况下，有些东西不是 doubles，但我们 metrics stack 中的所有东西，都会经过一个基本上每个 metric 都被 converted 成一个 double 的 stage
    -   我把大多数“应该”是 integral type 的东西都 stored 为 integral type，但从 `long -> double -> long` 进行 conversion，绝不会比直接进行 `long -> double` conversion 更 precise，而且还会引出其他问题
    -   我把一些不应该是 integral type 的东西 stored 为 integral type，这导致 small values 不必要地 lose precision
        -   幸运的是，这还没有对我做过的任何 actionable analysis 造成 serious errors，但有些 analyses 中它可能引发问题
-   使用 Using asserts，而不是把 bad entries writing 到某种 "bad entries" table 中
    -   由于一些超出本文范围的原因，在 Scalding jobs 中实际上没有合理的方式来 log errors 或 warnings，所以我 used asserts 来 catch 不该发生的情况，这导致每当意外情况发生时，整个 job 都会 die；更好的 solution 是把 bad input entries write 到一个 table 中，然后在该 table 不是 empty 时，把它通过电子邮件发出，作为 soft alert
        -   有个例子说明这样做本可以节省一些 operational overhead：我们曾出现过异常大的 clock skew（3600 年），导致 timestamp overflow。如果我有一个充当 bad entries 的 log 的 table，那个 bad entry 就会从 output 中被 omitted，这才是 correct behavior，而且还能省去一次打断，以及 push 一个 fix 并 re-deploy 该 job 的工作。
-   code 中的 Longterm 与 LongTerm
    -   最初写这些 code 时，我不确定应该怎样 capitalized；而在做出决定后，我没有对所有写错的地方进行 grep 和 squash，所以现在各处都存在这种毫无意义的不一致

当你迅速赶出一个东西，却没有充分想清楚时，就会出现这类问题。最后一项很容易 fix，也不算什么大问题，因为 Twitter 普遍使用 IDEs，这意味着基本上所有可能受影响的人，都会由他们的 IDE 提供 correct capitalization。

第一项的问题更大，一方面它确实可能导致 incorrect analyses，另一方面 fixing 它需要对我们拥有的所有 data 进行 migration。我猜，到现在这需要半周到一周的工作，而当初我只要多花三十秒想清楚自己在做什么，就能轻松避免。

第二项介于两者之间。综合第一项和第二项，我觉得自己大概在基本没有任何收益的情况下，给这个 system 承担了大约 double 的直接工作量（因此，不包括对 system 中的 data 进行 data analysis 所花的时间，只包括 build 这个 system 所花的时间）。

感谢 Leah Hanson、Andy Wilcox、Lifan Zeng 和 Matej Stuchlik 的评论、指正与讨论

* * *

1.  实际涉及的工作大约需要一天，但我用了一个星期才完成，因为我还得学习 Scala、Scalding，以及整体的 Twitter stack、metrics stack 等。

    一天也只是对最初那些 data sets 所需工作的估计。从那以后，我大概又做了几周的工作，Wesley Aptekar-Cassels 和 Kunal Trivedi 大概也又投入了一两周。opertional cost 大概相当于我每月 1-2 天的时间（on average），使 total cost 达到一两个月的量级。

    我也没有计算 using 这个 dataset 所花的时间，或 debugging issues 所花的时间，其中有很多时间我只能粗略估计。例如，compute platform team 根据一次大约花了一小时的 data analysis，changed 了 network egress limits，结果 exposed 了一个 latent mesos bug，大概耗费了 Ilya Pronin 一天的时间；David Mackey 也花了不少时间 tracking down 一些奇怪的问题，在这些情况下，data 显示有异常情况正在发生，但我们不知道具体是什么，等等。如果你想完整计算因对本文讨论的 data sets 进行某些 data analysis 而产生的工作所耗费的时间，我猜，加上 service-level teams，以及我们的 JVM、OS 和 HW teams 这样的 platform-level teams，我们大概已经投入了约 1 person-year 的时间。

    不过，因为最初 create 一个能 working 且有用的 system 所需的工作只是一天，再加上学习入门材料的时间，而这个 system 带来了七位数的回报，所以要证明所有这些额外时间投入的合理性一直很容易；如果需要先投入一年的工作，大概就不会如此。其余时间中的大部分，都不属于在创建 system 的 roadmap reviews 中通常会被“计入”的那类时间（users 花费的时间、operational overhead），但也许持续的 operational cost 应该在创建 system 时被“计入”（我认为，把 users 花费的时间“计入” system 并不合理，因为 system 越有用，users 就会花越多时间 using 它，这看起来实在不像一种 cost）。

    还有一些工作是在此基础上 build tools；Kunal Trivedi 花了不少时间在它之上 building 一个 layer，让 presentation 比 SQL queries 更 user friendly，这部分也可以说应该计入这个项目。

    [\[返回\]](https://danluu.com/metrics-analytics/#fnref:D)

---

## 原文

We spent one day[1](https://danluu.com/metrics-analytics/#fn:D) building a system that immediately found a mid 7 figure optimization (which ended up shipping). In the first year, we shipped mid 8 figures per year worth of cost savings as a result. The key feature this system introduces is the ability to query metrics data across all hosts and all services and over any period of time (since inception), so we've called it LongTermMetrics (LTM) internally since I like boring, descriptive, names.

This got started when I was looking for a starter project that would both help me understand the Twitter infra stack and also have some easily quantifiable value. Andy Wilcox suggested looking at [JVM survivor space](https://stackoverflow.com/a/37102630/334816) utilization for some large services. If you're not familiar with what survivor space is, you can think of it as a configurable, fixed-size buffer, in the JVM (at least if you use the GC algorithm that's default at Twitter). At the time, if you looked at a random large services, you'd usually find that either:

1.  The buffer was too small, resulting in poor performance, sometimes catastrophically poor when under high load.
2.  The buffer was too large, resulting in wasted memory, i.e., wasted money.

But instead of looking at random services, there's no fundamental reason that we shouldn't be able to query all services and get a list of which services have room for improvement in their configuration, sorted by performance degradation or cost savings. And if we write that query for JVM survivor space, this also goes for other configuration parameters (e.g., other JVM parameters, CPU quota, memory quota, etc.). Writing a query that worked for all the services turned out to be a little more difficult than I was hoping due to a combination of data consistency and performance issues. Data consistency issues included things like:

-   Any given metric can have ~100 names, e.g., I found 94 different names for JVM survivor space
    -   I suspect there are more, these were just the ones I could find via a simple search
-   The same metric name might have a different meaning for different services
    -   Could be a counter or a gauge
    -   Could have different units, e.g., bytes vs. MB or microseconds vs. milliseconds
-   Metrics are sometimes tagged with an incorrect service name
-   Zombie shards can continue to operate and report metrics even though the cluster manager has started up a new instance of the shard, resulting in duplicate and inconsistent metrics for a particular shard name

Our metrics database, [MetricsDB](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/metricsdb.html), was specialized to handle monitoring, dashboards, alerts, etc. and didn't support general queries. That's totally reasonable, since monitoring and dashboards are lower on Maslow's hierarchy of observability needs than general metrics analytics. In backchannel discussions from folks at other companies, the entire set of systems around MetricsDB seems to have solved a lot of the problems that plauge people at other companies with similar scale, but the specialization meant that we couldn't run arbitrary SQL queries against metrics in MetricsDB.

Another way to query the data is to use the copy that gets written to [HDFS](https://en.wikipedia.org/wiki/Apache_Hadoop#HDFS) in [Parquet](https://en.wikipedia.org/wiki/Apache_Parquet) format, which allows people to run arbitrary SQL queries (as well as write [Scalding](https://en.wikipedia.org/wiki/Cascading_\(software\)#cite_ref-27) (MapReduce) jobs that consume the data).

Unfortunately, due to the number of metric names, the data on HDFS can't be stored in a columnar format with one column per name -- [Presto](https://en.wikipedia.org/wiki/Presto_\(SQL_query_engine\)) gets unhappy if you feed it too many columns and we have enough different metrics that we're well beyond that limit. If you don't use a columnar format (and don't apply any other tricks), you end up reading a lot of data for any non-trivial query. The result was that you couldn't run any non-trivial query (or even many trivial queries) across all services or all hosts without having it time out. We don't have similar timeouts for Scalding, but Scalding performance is much worse and a simple Scalding query against a day's worth of metrics will usually take between three and twenty hours, depending on cluster load, making it unreasonable to use Scalding for any kind of exploratory data analysis.

Given the data infrastructure that already existed, an easy way to solve both of these problems was to write a Scalding job to store the 0.1% to 0.01% of metrics data that we care about for performance or capacity related queries and re-write it into a columnar format. I would guess that at least 90% of metrics are things that almost no one will want to look at in almost any circumstance, and of the metrics anyone really cares about, the vast majority aren't performance related. A happy side effect of this is that since such a small fraction of the data is relevant, it's cheap to store it indefinitely. The standard metrics data dump is deleted after a few weeks because it's large enough that it would be prohibitively expensive to store it indefinitely; a longer metrics memory will be useful for capacity planning or other analyses that prefer to have historical data.

The data we're saving includes (but isn't limited to) the following things for each shard of each service:

-   utilizations and sizes of various buffers
-   CPU, memory, and other utilization
-   number of threads, context switches, core migrations
-   various queue depths and network stats
-   JVM version, feature flags, etc.
-   [GC](https://en.wikipedia.org/wiki/Garbage_collection_\(computer_science\)) stats
-   [Finagle](https://github.com/twitter/finagle) metrics

And for each host:

-   various things from [procfs](https://en.wikipedia.org/wiki/Procfs), like `iowait` time, `idle`, etc.
-   what cluster the machine is a part of
-   host-level info like NIC speed, number of cores on the host, memory,
-   host-level stats for "health" issues like thermal throttling, machine checks, etc.
-   OS version, host-level software versions, host-level feature flags, etc.
-   [Rezolus](https://github.com/twitter/rezolus) metrics

For things that we know change very infrequently (like host NIC speed), we store these daily, but most of these are stored at the same frequency and granularity that our other metrics is stored for. In some cases, this is obviously wasteful (e.g., for JVM tenuring threshold, which is typically identical across every shard of a service and rarely changes), but this was the easiest way to handle this given the infra we have around metrics.

Although the impetus for this project was figuring out which services were under or over configured for JVM survivor space, it started with GC and container metrics since those were very obvious things to look at and we've been incrementally adding other metrics since then. To get an idea of the kinds of things we can query for and how simple queries are if you know a bit of SQL, here are some examples:

#### Very High p90 JVM Survivor Space

This is part of the original goal of finding under/over-provisioned services. Any service with a very high p90 JVM survivor space utilization is probably under-provisioned on survivor space. Similarly, anything with a very low p99 or p999 JVM survivor space utilization when under peak load is probably overprovisioned (query not displayed here, but we can scope the query to times of high load).

A Presto query for very high p90 survivor space across all services is:

```
with results as (
  select servicename,
    approx_distinct(source, 0.1) as approx_sources, -- number of shards for the service
    -- real query uses [coalesce and nullif](https://prestodb.io/docs/current/functions/conditional.html) to handle edge cases, omitted for brevity
    approx_percentile(jvmSurvivorUsed / jvmSurvivorMax, 0.90) as p90_used,
    approx_percentile(jvmSurvivorUsed / jvmSurvivorMax, 0.50) as p50_used,
  from ltm_service 
  where ds >= '2020-02-01' and ds <= '2020-02-28'
  group by servicename)
select * from results
where approx_sources > 100
order by p90_used desc
```

Rather than having to look through a bunch of dashboards, we can just get a list and then send diffs with config changes to the appropriate teams or write a script that takes the output of the query and automatically writes the diff. The above query provides a pattern for any basic utilization numbers or rates; you could look at memory usage, new or old gen GC frequency, etc., with similar queries. In one case, we found a service that was wasting enough RAM to pay my salary for a decade.

I've been moving away from using thresholds against simple percentiles to find issues, but I'm presenting this query because this is a thing people commonly want to do that's useful and I can write this without having to spend a lot of space explain why it's a reasonable thing to do; what I prefer to do instead is out of scope of this post and probably deserves its own post.

#### Network utilization

The above query was over all services, but we can also query across hosts. In addition, we can do queries that join against properties of the host, feature flags, etc.

Using one set of queries, we were able to determine that we had a significant number of services running up against network limits even though host-level network utilization was low. The compute platform team then did a gradual rollout of a change to network caps, which we monitored with queries like the one below to determine that we weren't see any performance degradation (theoretically possible if increasing network caps caused hosts or switches to hit network limits).

With the network change, we were able to observe, smaller queue depths, smaller queue size (in bytes), fewer packet drops, etc.

The query below only shows queue depths for brevity; adding all of the quantities mentioned is just a matter of typing more names in.

The general thing we can do is, for any particular rollout of a platform or service-level feature, we can see the impact on real services.

```
with rolled as (
 select
   -- rollout was fixed for all hosts during the time period, can pick an arbitrary element from the time period
   arbitrary(element_at(misc, 'egress_rate_limit_increase')) as rollout,
   hostId
 from ltm_deploys
 where ds = '2019-10-10'
 and zone = 'foo'
 group by ipAddress
), host_info as(
 select
   arbitrary(nicSpeed) as nicSpeed,
   hostId
 from ltm_host
 where ds = '2019-10-10'
 and zone = 'foo'
 group by ipAddress
), host_rolled as (
 select
   rollout,
   nicSpeed,
   rolled.hostId
 from rolled
 join host_info on rolled.ipAddress = host_info.ipAddress
), container_metrics as (
 select
   service,
   netTxQlen,
   hostId
 from ltm_container
 where ds >= '2019-10-10' and ds <= '2019-10-14'
 and zone = 'foo'
)
select
 service,
 nicSpeed,
 approx_percentile(netTxQlen, 1, 0.999, 0.0001) as p999_qlen,
 approx_percentile(netTxQlen, 1, 0.99, 0.001) as p99_qlen,
 approx_percentile(netTxQlen, 0.9) as p90_qlen,
 approx_percentile(netTxQlen, 0.68) as p68_qlen,
 rollout,
 count(*) as cnt
from container_metrics
join host_rolled on host_rolled.hostId = container_metrics.hostId
group by service, nicSpeed, rollout
```

#### Other questions that became easy to answer

-   What's the latency, CPU usage, CPI, or other performance impact of X?
    -   Increasing or decreasing the number of performance counters we monitor per container
    -   Tweaking kernel parameters
    -   OS or other releases
    -   Increasing or decreasing host-level oversubscription
    -   General host-level load
    -   Retry budget exhaustion
-   For relevant items above, what's the distribution of X, in general or under certain circumstances?
-   What hosts have unusually poor service-level performance for every service on the host, after controlling for load, etc.?
    -   This has usually turned out to be due to a hardware misconfiguration or fault
-   Which services don't play nicely with other services aside from the general impact on host-level load?
-   What's the latency impact of failover, or other high-load events?
    -   What level of load should we expect in the future given a future high-load event plus current growth?
    -   Which services see more load during failover, which services see unchanged load, and which fall somewhere in between?
-   What config changes can we make for any fixed sized buffer or allocation that will improve performance without increasing cost or reduce cost without degrading performance?
-   For some particular host-level health problem, what's the probability it recurs if we see it N times?
-   etc., there are a lot of questions that become easy to answer if you can write arbitrary queries against historical metrics data

#### Design decisions

LTM is about as boring a system as is possible. Every design decision falls out of taking the path of least resistance.

-   Why using Scalding?
    -   It's standard at Twitter and the integration made everything trivial. I tried Spark, which has some advantages. However, at the time, I would have had to do manual integration work that I got for free with Scalding.
-   Why use Presto and not something that allows for live slice & dice queries like Druid?
    -   [Rebecca Isaacs and Jonathan Simms were doing related work on tracing](https://danluu.com/tracing-analytics/) and we knew that we'd want to do joins between LTM and whatever they created. That's trivial with Presto but would have required more planning and work with something like Druid, at least at the time.
    -   George Sirois imported a subset of the data into Druid so we could play with it and the facilities it offers are very nice; it's probably worth re-visiting at some point
-   Why not use Postgres or something similar?
    -   The amount of data we want to store makes this infeasible without a massive amount of effort; even though the cost of data storage is quite low, it's still a "big data" problem
-   Why Parquet instead of a more efficient format?
    -   It was the most suitable of the standard supported formats (the other major suppported format is raw thrift), introducing a new format would be a much larger project than this project
-   Why is the system not real-time (with delays of at least one hour)?
    -   Twitter's batch job pipeline is easy to build on, all that was necessary was to read some tutorial on how it works and then write something similar, but with different business logic.
    -   There was a nicely written proposal to build a real-time analytics pipeline for metrics data written a couple years before I joined Twitter, but that never got built because (I estimate) it would have been one to four quarters of work to produce an MVP and it wasn't clear what team had the right mandate to work on that and also had 4 quarters of headcount available. But the add a batch job took one day, you don't need to have roadmap and planning meetings for a day of work, you can just do it and then do follow-on work incrementally.
    -   If we're looking for misconfigurations or optimization opportunities, these rarely go away within an hour (and if they did, they must've had small total impact) and, in fact, they often persist for months to years, so we don't lose much by givng up on real-time (we do lose the ability to use the output of this for some monitoring use cases)
    -   The real-time version would've been a system that significant operational cost can't be operated by one person without undue burden. This system has more operational/maintenance burden than I'd like, probably 1-2 days of mine time per month a month on average, which at this point makes that a pretty large fraction of the total cost of the system, but it never pages, and the amount of work can easily be handeled by one person.

#### Boring technology

I think writing about systems like this, that are just boring work is really underrated. A disproportionate number of posts and talks I read are about systems using hot technologies. I don't have anything against hot new technologies, but a lot of useful work comes from plugging boring technologies together and doing the obvious thing. Since posts and talks about boring work are relatively rare, I think writing up something like this is more useful than it has any right to be.

For example, a couple years ago, at a local meetup that Matt Singer organizes for companies in our size class to discuss infrastructure (basically, companies that are smaller than FB/Amazon/Google) I asked if anyone was doing something similar to what we'd just done. No one who was there was (or not who'd admit to it, anyway), and engineers from two different companies expressed shock that we could store so much data, and not just the average per time period, but some histogram information as well. This work is too straightforward and obvious to be novel, I'm sure people have built analogous systems in many places. It's literally just storing metrics data on HDFS (or, if you prefer a more general term, a data lake) indefinitely in a format that allows interactive queries.

If you do the math on the cost of metrics data storage for a project like this in a company in our size class, the storage cost is basically a rounding error. We've shipped individual diffs that easily pay for the storage cost for decades. I don't think there's any reason storing a few years or even a decade worth of metrics should be shocking when people deploy analytics and observability tools that cost much more all the time. But it turns out this was surprising, in part because people don't write up work this boring.

An unrelated example is that, a while back, I ran into someone at a similarly sized company who wanted to get similar insights out of their metrics data. Instead of starting with something that would take a day, like this project, they started with deep learning. While I think there's value in applying ML and/or stats to infra metrics, they turned a project that could return significant value to the company after a couple of person-days into a project that took person-years. And if you're only going to _either_ apply simple heuristics guided by someone with infra experience and simple statistical models _or_ naively apply deep learning, I think the former has much higher ROI. Applying both sophisticated stats/ML _and_ practitioner guided heuristics together can get you better results than either alone, but I think it makes a lot more sense to start with the simple project that takes a day to build out and maybe another day or two to start to apply than to start with a project that takes months or years to build out and start to apply. But there are a lot of biases towards doing the larger project: it makes a better resume item (deep learning!), in many places, it makes a better promo case, and people are more likely to give a talk or write up a blog post on the cool system that uses deep learning.

The above discusses why writing up work is valuable for the industry in general. [We covered why writing up work is valuable to the company doing the write-up in a previous post](https://danluu.com/corp-eng-blogs/), so I'm not going to re-hash that here.

#### Appendix: stuff I screwed up

[I think it's unfortunate that you don't get to hear about the downsides of systems without backchannel chatter](https://twitter.com/danluu/status/1220228489522974721), so here are things I did that are pretty obvious mistakes in retrospect. I'll add to this when something else becomes obvious in retrospect.

-   Only having the final output tables and not a two-stage process
    -   One issue is this is that, when I found a bug, I'd end re-writing data in place, which was destructive, and could prevent fixing some kinds of bugs later
    -   Another one was that I wanted to keep the data as clean as possible, but most people who wanted to add a table would add one with all sorts of issues (e.g., a value that's supposed to be beween 0 and 1 would have a few percent of values outside this range, often very large values like 2^32 or above, making queries like tail percentile or average meaningless). I rejected these table additions and would ask people to clean up the tables to be around as clean as the existing tables. A better way to do this would be to let people add "beta" tables or something like that, which I could clean up later. This also fits well with this two-stage approach that I should'be done anyway
        -   A couple people were willing to put up with my data nitpicking and contributed tables; one of the tables that was contributed by Kunal Trivedi, was one of the highest impact tables (in terms of cost savings / profit driven by queries against the table), but most people weren't interested in cleaning things up enough to meet the bar I had for this. Having some tables that meet a lower bar that I clean up later (or even never clean up) would've likely driven significantly higher total impact for essentially no cost
-   Not using a double for almost everything
    -   In an ideal world, some things aren't doubles, but everything in our metrics stack goes through a stage where basically every metric is converted to a double
    -   I stored most things that "should" be an integral type as an integral type, but doing the conversion from `long -> double -> long` is never going to be more precise than just doing the`long -> double` conversion and it opens the door to other problems
    -   I stored some things that shouldn't be an integral type as an integral type, which causes small values to unnecessarily lose precision
        -   Luckily this hasn't caused serious errors for any actionable analysis I've done, but there are analyses where it could cause problems
-   Using asserts instead of writing bad entries out to some kind of "bad entries" table
    -   For reasons that are out of scope of this post, there isn't really a reasonable way to log errors or warnings in Scalding jobs, so I used asserts to catch things that shoudn't happen, which causes the entire job to die every time something unexpected happens; a better solution would be to write bad input entries out into a table and then have that table emailed out as a soft alert if the table isn't empty
        -   An example of a case where this would've saved some operational overhead is where we had an unusual amount of clock skew (3600 years), which caused a timestamp overflow. If I had a table that was a log of bad entries, the bad entry would've been omitted from the output, which is the correct behavior, and it would've saved an interruption plus having to push a fix and re-deploy the job.
-   Longterm vs. LongTerm in the code
    -   I wasn't sure which way this should be capitalized when I was first writing this and, when I made a decision, I failed to grep for and squash everything that was written the wrong way, so now this pointless inconsistency exists in various places

These are the kind of thing you expect when you crank out something quickly and don't think it through enough. The last item is trivial to fix and not much of a problem since the ubiquitous use of IDEs at Twitter means that basically anyone who would be impacted will have their IDE supply the correct capitalization for them.

The first item is more problematic, both in that it could actually cause incorrect analyses and in that fixing it will require doing a migration of all the data we have. My guess is that, at this point, this will be half a week to a week of work, which I could've easily avoided by spending thirty more seconds thinking through what I was doing.

The second item is somewhere in between. Between the first and second items, I think I've probably signed up for roughly double the amount of direct work on this system (so, not including time spent on data analysis on data in the system, just the time spent to build the system) for essentially no benefit.

Thanks to Leah Hanson, Andy Wilcox, Lifan Zeng, and Matej Stuchlik for comments/corrections/discussion

* * *

1.  The actual work involved was about a day's work, but it was done over a week since I had to learn Scala as well as Scalding and the general Twitter stack, the metrics stack, etc.

    One day is also just an estimate for the work for the initial data sets. Since then, I've done probably a couple more weeks of work and Wesley Aptekar-Cassels and Kunal Trivedi have probably put in another week or two of time. The opertional cost is probably something like 1-2 days of my time per month (on average), bringing the total cost to on the order a month or two.

    I'm also not counting time spent using the dataset, or time spent debugging issues, which will include a lot of time that I can only roughly guess at, e.g., when the compute platform team changed the network egress limits as a result of some data analysis that took about an hour, that exposed a latent mesos bug that probably cost a day of Ilya Pronin's time, David Mackey has spent a fair amount of time tracking down weird issues where the data shows something odd is going on, but we don't know what is, etc. If you wanted to fully account for time spent on work that came out of some data analysis on the data sets discussed in the post, I suspect, between service-level teams, plus platform-level teams like our JVM, OS, and HW teams, we're probably at roughly 1 person-year of time.

    But, because the initial work it took to create a working and useful system was a day plus time spent working on orientation material and the system returned seven figures, it's been very easy to justify all of this additional time spent, which probably wouldn't have been the case if a year of up-front work was required. Most of the rest of the time isn't the kind of thing that's usually "charged" on roadmap reviews on creating a system (time spent by users, operational overhead), but perhaps the ongoing operational cost shlould be "charged" when creating the system (I don't think it makes sense to "charge" time spent by users to the system since, the more useful a system is, the more time users will spend using it, that doesn't really seem like a cost).

    There'a also been work to build tools on top of this, Kunal Trivedi has spent a fair amount of time building a layer on top of this to make the presentation more user friendly than SQL queries, which could arguably be charged to this project.

    [\[return\]](https://danluu.com/metrics-analytics/#fnref:D)