
就在昨天，好久没有声音的 kimi 也是推出了他们的最新力作：kimi k2 模型，在多个榜单上又刷到了 SOTA。但是熟悉 锦恢 的朋友都知道，我不在乎榜单，我更在乎实际使用体验。

作为 Agent 开发者和 [OpenMCP](https://link.zhihu.com/?target=https%3A//openmcp.kirigaya.cn/) 作者，大模型工具调用能力和 planning 能力一直是我最关心的。所以简单做了一份不算权威的测试。

先说我用下来的结论：与 deepseek v3 相比，kimi k2 的响应速度更快，在 Agent 应用上有非常明显的性能提升。结合廉价的 API 定价，我单方面认为 kimi k2 或许是 2025 年国内最适合用来构建 AI Agent 应用的大模型基座（油管上甚至有人把 kimi k2 称为 Agentic AI 的 Deepseek 时刻）。关键是这玩意儿还开源，不过个人用的话，还是推荐购买 API。

- [kimi k2 官方技术博客](https://moonshotai.github.io/Kimi-K2/)
- [kimi k2 模型下载地址](https://huggingface.co/collections/moonshotai/kimi-k2-6871243b990f2af5ba60617d)

## #T1. 响应速度测试

![](https://pic1.zhimg.com/80/v2-0deddf2091f9d6cb3371ca512541a38d_1440w.webp?source=2c26e567)

可以对比一下 deepseek v3

![](https://picx.zhimg.com/80/v2-2ae66ed8e3f3ae6cacd43e3a7fe7f197_1440w.webp?source=2c26e567)

## #T2. 幻觉测试

在 openmcp 官网上，有一个很有趣的进行大模型 Agent 幻觉测试的例子：
[go 实现 neo4j 的只读 mcp 服务器 (SSE)​kirigaya.cn/openmcp/zh/plugin-tutorial/examples/go-neo4j-sse.html](https://link.zhihu.com/?target=https%3A//kirigaya.cn/openmcp/zh/plugin-tutorial/examples/go-neo4j-sse.html)
这个例子也就是让 Agent 来访问数据库，调用 mcp 的入参由大模型生成。可以先看一下 deepseek v3 的结果：
![](https://pica.zhimg.com/80/v2-d373394032164c98c7858c18cef111a3_1440w.webp?source=2c26e567)

你会发现返回为空，因为我数据库中代表评论的类型不叫 Comment，大模型在不知道的情况下，瞎编了一个 Comment，这属于幻觉。

k2 似乎很好地解决了这个问题，下面是 k2 的结果：

![](https://pica.zhimg.com/80/v2-c7551687119fa56d3c415a1c042d552e_1440w.webp?source=2c26e567)

有点过于完美了。。。我之前的部分 trick 都可以弃之不用了。

奥，对了，如果你在 kimi 上的充值额度不足 50，那么 RPM 会被限制在 3，请求太频繁就会出现下面的问题：

![](https://picx.zhimg.com/80/v2-3918790c0edcdb5c261549cd26b5abb9_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-ce7cd2eef4f05ad7bf5d6e5a016b6462_1440w.webp?source=2c26e567)

可恶，还是你们会做生意。先让我充值 50.

![](https://picx.zhimg.com/80/v2-9ea0bf4c0fe91bd4115d5cf23d2f7e57_1440w.webp?source=2c26e567)

## #T3. 长上下文工具调用测试

这里使用了我开发 [Slidev MCP](https://link.zhihu.com/?target=https%3A//github.com/LSTM-Kirigaya/slidev-mcp)，用于让 AI 根据输入语料或者外部链接来自动生成 ppt。

使用 Slidev MCP 的 prompt “guide”，然后加上如下的问题：

```
请帮我根据 https://kirigaya.cn/openmcp/zh/plugin-tutorial/examples/go-neo4j-sse.html 制作一份 ppt，作者是锦恢
```

先看一下 deepseek 的表现：

![](https://picx.zhimg.com/80/v2-5ecba0972995f04ae31a5cb71aa154ea_1440w.webp?source=2c26e567)

任务顺利完成，看一下生成的 ppt（随便选了几张）

![](https://pic1.zhimg.com/80/v2-3e1af468d5f522ce5a3f0523b1cec855_1440w.webp?source=2c26e567)

![](https://pic1.zhimg.com/80/v2-5bb22689bfd2b3aba43c51c67557600f_1440w.webp?source=2c26e567)

![](https://pic1.zhimg.com/80/v2-c34cca2037b0b84f837c9b5cb63acace_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-4911e71dc2ef3731349e9a847350827a_1440w.webp?source=2c26e567)

可以看到，有些地方还是不够完美的，在有的页面 v3 并没有很好的根据 guide prompt 中设定的规则生成 ppt 的布局内容（但是 r1 是可以的，但是 r1 的执行时间大概是 v3 的五倍左右）

我们可以看一下 kimi k2 的效果：

![](https://picx.zhimg.com/80/v2-033d87e5d95f15f2c47ec93d04e7b3b9_1440w.webp?source=2c26e567)

生成的 ppt 效果：

![](https://picx.zhimg.com/80/v2-378511dbb7c72ff57bc09f2c1e64a5b4_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-dd8fe586e48f109bc76edb9b304f5b09_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-83aaaba571cd842738cd92e2a7d83caa_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-fa4f5a9cb0a37f63f96ba0a1c6a50f1c_1440w.webp?source=2c26e567)

![](https://picx.zhimg.com/80/v2-68d0fcf1fa784308f2a868f19848acfb_1440w.webp?source=2c26e567)

![](https://pica.zhimg.com/80/v2-d3fc226f3a78d931d5d574c09c3fdb1b_1440w.webp?source=2c26e567)

可以看到，k2 生成的内容更加丰富，虽然也存在格式上的问题。

> 看来格式上的问题有待对 MCP Tool 进行进一步约束和优化来改进。

额外说一句，如果你是 claude code 的忠实用户，可以尝试通过 [https://github.com/LLM-Red-Team/kimi-cc](https://link.zhihu.com/?target=https%3A//github.com/LLM-Red-Team/kimi-cc) 这个项目来用 kimi k2 来驱动你的 cc，k2 底子的 cc 效果非常不错，比 cursor 在项目初期的代码上强不少。（非常抱歉，暂时没有时间做完整的系统实验，所以此处只是告知大家有这个方案可以选择）

![](https://pic1.zhimg.com/80/v2-7ba25352a976a5e8974588ee0c43efc0_1440w.webp?source=2c26e567)

kimi 团队确实不容易，被 ds 压了快一年，终于可以拿 k2 出一口气了，nb。压力这下转移到了 deepseek 这边。怎么说？

---

最后，对 AI Agent 技术感兴趣，希望一起交流的，欢迎加入 OpenMCP 交流群，加入方式就在 https://github.com/LSTM-Kirigaya/openmcp-client/ 这个项目的 README 里面。如果可以给我们点个 star，那就再好不过了。