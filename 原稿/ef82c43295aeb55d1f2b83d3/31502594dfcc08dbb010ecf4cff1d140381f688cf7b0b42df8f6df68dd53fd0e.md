锦恢最近除了秋招面试，还在写一篇有关 Agent 的技术博客，讲讲结构化输出的底层原理的，然后在写博客的时候，想要画一张流程图来解释一下大模型的基本工作原理，尽量让小白也能看懂的那种，但是我又懒得画流程图，所以我就在想，能不能用 AI 来帮我画可以被编辑的流程图。

> 敲黑板：必须生成可以编辑的流程图，Claude 生成的 HTML 很好看，但是我改起来太难受，不考虑。

然后我就灵机一动，drawio是开源的，那么肯定有 drawio 的 MCP 服务器，我一搜果然有，然后很快安装上让制作出的 Drawio Agent 帮我完成了讲解大模型的流程图的绘制：

![8bf8adf2e1e1f50831a33f20f6f101ed|690x295](https://linux.do/uploads/default/original/4X/c/c/f/ccfa2f6c326d58dbb5620c1f72b9a5852bf15990.png)

接下来我就分享一下如何使用我的同款效果吧！

---

## 安装 Drawio MCP


### 1. 下载 OpenMCP

先下载 openmcp，根据这里的教程：

https://kirigaya.cn/openmcp/zh/plugin-tutorial/quick-start/acquire-openmcp.html

### 2. 添加 Drawio MCP 服务器

然后打开 openmcp 的控制面板，点击 + 添加：

![image|554x500](https://linux.do/uploads/default/original/4X/c/3/b/c3bd2d0b0cc1e4a44e8cada4e409bf8332282495.png)

选择 STDIO，然后命令填入 `npx -y drawio-mcp-server`，工作目录留空就行。

### 3. 启动 Drawio MCP 服务器

然后点击这里启动：

![image|676x500](https://linux.do/uploads/default/original/4X/6/b/0/6b060cd0d89ab3c2e0c180ce4e82c69835d50dc9.png)

启动后，openmcp 会自动去连结 Drawio MCP 服务器，显示连接成功就没问题了：

![image|690x348](https://linux.do/uploads/default/original/4X/a/3/f/a3fdc111cd5a3f388b578e78a50359fdd15d2ad9.jpeg)

### 4. 安装 drawio 浏览器插件并准备绘画环境

- [Google 插件](https://chromewebstore.google.com/detail/drawio-mcp-extension/okdbbjbbccdhhfaefmcmekalmmdjjide)

- [Firefox 插件](https://addons.mozilla.org/en-US/firefox/addon/drawio-mcp-extension/)

安装后，打开网页版本 drawio：https://app.diagrams.net/

打开插件，如果显示 connected 代表连接成功：

![image|452x500](https://linux.do/uploads/default/original/4X/b/9/f/b9fdb7829ef3a669b33dbc8cb76da1795b7837f0.png)

### 5. 开始玩耍！

回到 openmcp，打开一个交互测试窗口，询问如下的问题：


```
我现在向大模型输入“今天还没吃饭，现在好”，大模型输出一个向量代表 logits，然后选中其中数值最大的一个，备选单词有“饿”，还有很多其他的，你需要帮我生成一个绘制当前场景的 html，表示一下这个逻辑，选择了logits最大的那个，然后那个单词是“饿”，然后拼接到上面的句子中。

你需要创建 logits 和每一个token单词，把它们放在一个方块内。

请使用从左到右的布局风格来绘制。
```

[配置一下你的大模型 API](https://kirigaya.cn/openmcp/zh/plugin-tutorial/usage/connect-llm.html)，按下回车，你的 Agent 就会开始工作啦！

![image|690x397](https://linux.do/uploads/default/original/4X/4/b/9/4b9f9fae8ed65f01e70589f078efb52ab8ac0af1.png)

过一会，就能看到结果了：

![c5186cdc4153a3006617441fc0d44833|690x292](https://linux.do/uploads/default/original/4X/5/8/a/58ab031d7e4c58533742ffe5f4497fe294703e9d.png)

感觉不太好看，我更喜欢 logits 颜色保持一致，我们继续把需求仍给 AI：

![395913bad8eb4d8a127652ae992d9383|690x372](https://linux.do/uploads/default/original/4X/2/3/d/23ded031009b5f50f9d6af193a15d24c16f497b4.png)

这下的效果就完美了：

![8bf8adf2e1e1f50831a33f20f6f101ed|690x295](https://linux.do/uploads/default/original/4X/c/c/f/ccfa2f6c326d58dbb5620c1f72b9a5852bf15990.png)

![97A2514D32FA4A1456B96B6496AA1843|535x500](https://linux.do/uploads/default/original/4X/5/d/b/5db184b0865ae192e4f94dea913f80410258f14e.jpeg)

最后，觉得好用的话，不要忘记给 OpenMCP 点个 star 呀：

https://github.com/LSTM-Kirigaya/openmcp-client/

入群交流方式在 readme 链接中，非常欢迎一起来讨论最新的 AI Agent 技术。这里是锦恢，期待一下《如何约束大模型严格输出指定内容？引导解码器 Guided Decoder 原理讲解 & 实战演示》这篇博客吧！