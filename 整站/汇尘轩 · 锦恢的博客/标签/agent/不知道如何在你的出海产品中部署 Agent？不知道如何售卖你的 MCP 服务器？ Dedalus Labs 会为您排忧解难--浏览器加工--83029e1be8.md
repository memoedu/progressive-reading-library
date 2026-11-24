## AI Agent 的部署痛点

提到 Agent 就不得不提到非常爆火的三项技术了：MCP，SKILLS 和 PTC。最近爆火的小龙虾就是这三项技术的综合体。相信经常关注我频道的朋友都知道我曾经写过一些和 MCP 相关的博客，并且也是 OpenMCP 的作者。虽然我的OpenMCP可以帮你自动化的完成MCP相关的验证测试，但是最终我们仍然需要将它部署到agent里面。而且在部署这样的 Agent 的时候，我们还需要额外部署MCP服务器，并且通过各种各样的环境变量来管理里面的依赖项，包括但不限于大模型的API、MCP 服务器的 url 之类的。

长此以往，我们总觉得这样做太麻烦了。那么有没有什么样的框架和平台可以让我们专注于实现agent，而避免这些部署时复杂的配置呢？有的兄弟有的，今天想给大家介绍的就是这款最近我发现的宝藏网站 [Dedalus Labs](https://www.dedaluslabs.ai/)，它极大的简化了部署agent的过程中，需要用户操心的各类变量和依赖项的管理。而且很重要的是，它的官网做的实在太漂亮了。

通过下图的官网里面的示例代码，你就能知道利用这个平台的能力开发agent有多么方便了。

![图片](https://kirigaya.cn/api/storage/image/703ba40c-bc17-4397-9483-4da32808f194.webp)

并且 Dedalus 可以很自由的使用市面上几乎所有的模型。如果你想要制作出海的 Agent 产品，我想这会是一个非常适合的平台。

---

## 前置工作

* 使用 github 注册 Dedalus（很重要，后面有妙用）

* 在 [API Keys](https://www.dedaluslabs.ai/dashboard/api-keys) 中注册获取一个 Key

* 本地至少有一个 vibe coding 工具（我演示使用的是 kimi k2）

## 使用 Dedalus 优雅极速地创建 Agent

Dedalus 基于现在的大模型和已有的成熟 MCP 构建一个agent速度非常快，下面来给大家做一个简单的演示。

我们打开工作区目录

```
cd ~/code/dedalus-tutorial
```

然后我们进入 Dedalus 的 [Overview](https://www.dedaluslabs.ai/dashboard/overview)，根据你的需求，寻找现成的 MCP 服务器。

![图片](https://kirigaya.cn/api/storage/image/f8468361-0f9e-42dd-8a3d-17c80ae1cb09.webp)

在 overview 中提供了很多的工具，比如我们今天的任务是做一个可以全网搜索、回答问题的 Agent，就用第一个工具 exa。我们点击进入，然后找到下面这个按钮，点击复制

![图片](https://kirigaya.cn/api/storage/image/300762d0-2658-4463-ad04-bd783516663b.webp)

回到我们的 vibe coding 页面，输入下面的 prompt 来一键实现 agent：

```
现在我需要创建一个能够访问推测内容的agent使用的一个工具文档如下，请帮我初始化并完成这个项目。

## 我的 Dedalus API Key
<粘贴你创建的 Dedalus API>

## 参考资料
<粘贴刚刚复制的 readme>

## 测试内容
通过 “搜索最近关于 AI 的热门推文” 作为 prompt 来进行测试
```

运行结束之后，你应该能看到你已经实现了一个可以进行初步搜索的agent。

稍微瞄一眼 AI 生成的代码，你大概能看到长得像下面这样的东西。

```python
async def search_ai_tweets():
    # 运行搜索任务
    result = await runner.run(
        input=prompt,
        model="openai/gpt-5-nano",
        mcp_servers=["tsion/exa"]
    )
```

这个代码的意思也非常的好理解，`model` 就是我们默认使用的模型，`mcp_servers` 就是注册在 Dedalus 里面的 MCP 服务器。`tsion/exa` 就是我们刚刚提到的平台内置的 exa 的 MCP服务器。那么在我们的业务里面，很多时候我们可能有自己业务专门的MCP服务器，我们能不能部署和使用呢？

那是当然，Dedalus 也提供了部署 MCP 服务器的功能。

## 使用 Dedalus 部署自定义的 MCP 服务器

假设你通过 OpenMCP 或者是 vibe coding 完成了一个新的MCP服务器，那就可以非常轻易的来部署它。

在 Dedalus 的 [Servers 界面](https://www.dedaluslabs.ai/dashboard/servers)，点击添加服务器之后，你就能看到当前你github下的所有项目，选择你已经实现好的MCP的项目，就可以立刻部署。

![image](https://cdn3.linux.do/original/4X/9/e/4/9e4107a2afd08da4de22d042c3a02552a873279c.png)

需要注意的是，Dedalus 平台允许部署的MCP服务器必须得按照他们的规范来实现。当然，这非常非常简单，这并不需要你手动来完成。你只需要在vibe coding的时候，额外增加一个如下的prompt就能实现：

```
请按照如下规范改造 MCP 代码 https://docs.dedaluslabs.ai/dmcp
```

![图片](https://kirigaya.cn/api/storage/image/cd5bed01-6466-4372-9050-48b1e40fc182.webp)

如果当前的这个仓库确实符合他们的规范，那么deploy按钮就会可以被点击，点击之后就能将你的MCP服务器部署在他们的云端了。

部署成功，并且编译成功之后，你就可以加上仓库名来快速实现你的agent了。比如可以就通过下面聊聊几行代码，就完成我上面这个MCP服务器的加载和对应agent的实现。

```python
async def search_weather():
    # 运行查询天气任务
    result = await runner.run(
        input=prompt,
        model="openai/gpt-5-nano",
        mcp_servers=["lstm-kirigaya/mcp-weather-server"]
    )
```

## 想成为 MCP 供应商？

除此之外，Dedalus 提供了一个很棒的功能。如果你觉得你的MCP服务器做的非常棒，但不知道如何商业化。那么在你部署完成你的MCP服务器之后，可以在设置部分找到pricing这个板块来为你的MCP服务器定价。你可以决定每一次调用你的MCP服务器，对方要支付多少钱，当然默认就是免费了。

![图片](https://kirigaya.cn/api/storage/image/df3d5641-d360-42f7-9c92-078ea7c2fc38.webp)

在 [Seller](https://www.dedaluslabs.ai/dashboard/seller) 板块你可以连接上你的stripe账号来查看当前的收入。

## 总结

总而言之，Dedalus 是一个相当棒的平台。如果你谋求 Agent 产品的出海，或者说你想要让你的MCP服务器能够实现商业化，那么这个平台绝对都是绝佳之选。