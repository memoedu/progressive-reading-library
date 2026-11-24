## 前言

mcp 虽然让大模型有了访问外部工具的能力，从而大幅度拓展了大模型应用的能力边界，但是有的小伙伴会说：mcp 消耗的 token 太多啦！就算便宜如 deepseek，大规模任务在高峰时段调用一天，估计一杯蜜雪冰城就要没了。

那么有没有什么方法可以不用花钱也能使用 mcp 呢？有的兄弟有的。

---

## 神奇的浏览器插件：MCP-SuperAssistant

今天要给大家隆重推荐的项目是 MCP-SuperAssistant，一款神奇的浏览器插件，它允许你在使用 deepseek, chatgpt 等等大模型的网页版本时也可以使用本地的 mcp。

🔗 github 仓库链接：https://github.com/srbhptl39/MCP-SuperAssistant

我们都知道网页版本的大模型消耗 token 都是不花钱的，所以有了这个插件，我们就可以不花钱使用 mcp 了！

---

## 准备工作

- chrome 浏览器
- 一个可以工作的 mcp
- nodejs 环境

---

## 安装基本的环境

首先大家需要保证自己的电脑上存在 nodejs 的环境，如果没有的话，可以去 [Download Node.js](https://nodejs.org/en/download) 上下载对应操作系统的 nodejs。

安装完成 nodejs 后，我们需要先下载 MCP-SuperAssistant 这个 chrome 浏览器的插件，先点击 [MCP SuperAssistant](https://chromewebstore.google.com/detail/mcp-superassistant/kngiafgkdnlkgmefdafaibkibegkcaef)，Add to Chrome 就可以下载。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-3d94c2f7e8f0d3e4765a487593d4e9c4_1440w.png" style="width: 100%;"/>
</div>

---

## 配置 MCP

MCP SuperAssistant 通过插件和本地的代理来实现让浏览器也能访问 MCP，所以我们还需要配置一下本地的 MCP，让 MCP-SuperAssistant 知道我们本地需要连接哪些 MCP，每一个 MCP 如何进行连接。

首先我们需要有一个自己的 MCP，你可以选择从网上直接下载现成的 MCP 服务器，也可以针对自己的业务进行开发，这里安利一下我开发的用于进行 MCP 开发 vscode 插件 OpenMCP：

- [🚀 OpenMCP 中文文档](http://kirigaya.cn/openmcp/zh)
- [🔗 Github 仓库](https://github.com/LSTM-Kirigaya/openmcp-client)

利用 OpenMCP，你可以直接在代码编辑器中就完成 MCP 的开发和验证与迭代，非常方便。

假设你已经获得了一个能用的 MCP 服务器，那么随便找一个目录，然后创建 `mcpconfig.json`，在 `mcpconfig.json` 中配置你的 MCP 服务器：

```json
{
  "mcpServers": {
    "desktop-commander": {
      "command": "npx",
      "args": [
        "-y",
        "@wonderwhy-er/desktop-commander"
      ]
    },
    "neo4j-mcp": {
      "url": "http://localhost:8083/sse"
    }
  }
}
```

然后在命令行中启动我们的 superassistant 的代理服务器从而让浏览器可以访问到我们的 MCP：

```bash
npx @srbhptl39/mcp-superassistant-proxy@latest --config ./mcpconfig.json
```

此时，如果出现了下面这个样子的输出，说明运行成功。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-bae57e7579fbf73170e1482c14ae94ef_1440w.png" style="width: 100%;"/>
</div>

现在，我们进入 https://chat.deepseek.com/ ，新建一个对话（如果是已经开启 deepseek 网页的朋友记得刷新一下页面，来让插件得以激活）。此时，你应该可以看到输入框的右下角出现了一个小图标「MCP」，这代表插件激活成功。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-1cc52f8be7a6303ed5abd20af6d10c74_1440w.png" style="width: 100%;"/>
</div>

点击 「MCP」，将下面的这四个选项全部开启：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4d9ba14c93b350866146458a59a8bc69_1440w.png" style="width: 100%;"/>
</div>

然后右侧会出现一个控制面板，检查一下面板内部是否存在你启动的 MCP 服务器对应的所有工具。如果存在，说明配置成功。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e624dc38b767135e2ddc89ddc98d2b3c_1440w.png" style="width: 100%;"/>
</div>

---

## 让大模型网页版本也可以使用 MCP

如果你通过了上面的步骤，那么说明你已经成功配置了 MCP-SuperAssistant，接下来，我们就可以让大模型网页版本也可以使用 MCP 了。

这里有一个非常重要的点，一定要记好了：每一次新的对话，一定要先通过下面的操作来让大模型明白，它要如何使用 MCP，点击「MCP」「Attach」来粘贴起头的引导文本，这一步非常关键，没有这一步大模型是不会去进行工具调用的。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-27e39ff52ef8d1cf9324f708d35cc4ac_1440w.png" style="width: 100%;"/>
</div>

OK，接下来，我们就可以轻松愉快地让大模型进行 MCP 使用了。比如我的 MCP 是访问我的数据库的 MCP，我可以直接问大模型：

```
请告诉我最新的 10 条评论
```

然后大模型就会自己思考，然后调用工具，获得结果，继续思考，调用工具，获得结果 ...

经过若干步骤的迭代后，大模型输出了如下的结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-135f6dab13accc12d8bc4414430ce2e9_1440w.png" style="width: 100%;"/>
</div>

更多好玩的 MCP 的开发案例，可以关注一下 OpenMCP 的文档站点，我在 B站 上也会更新一些开发 MCP 的应用案例，比如上面那个访问 neo4j 图数据库的教程，我就做成了视频：[OpenMCP 教程 | 优雅地开发 MCP（二）实现图数据库 MCP Agent & 工具的原子化扩展](https://www.bilibili.com/video/BV1g8TozyEE7/?spm_id_from=333.1387.homepage.video_card.click&vd_source=3f248073d6ebdb61308992901b606f24)

这个视频也有配套的文案，也是我原创的中英日三语版本：[go 实现 neo4j 的只读 mcp 服务器 (SSE)](https://kirigaya.cn/openmcp/zh/plugin-tutorial/examples/go-neo4j-sse.html)


---

如果对 MCP 开发感兴趣的朋友，欢迎加入我们的 [OpenMCP 讨论群](https://qm.qq.com/cgi-bin/qm/qr?k=C6ZUTZvfqWoI12lWe7L93cWa1hUsuVT0&jump_from=webapi&authKey=McW6B1ogTPjPDrCyGttS890tMZGQ1KB3QLuG4aqVNRaYp4vlTSgf2c6dMcNjMuBD) 或是 知乎圈子 [OpenMCP 博物馆](https://www.zhihu.com/ring/host/1911121615279849840?tab=new) 进行方案讨论和问题解决。最后，对于我的 [OpenMCP](https://github.com/LSTM-Kirigaya/openmcp-client) ，跪求点个 star, orz