## 前言

软件和文档的翻译相信是大部分朋友绕不开的一个点，专注于技术的我们从来不愿意在这上面花费时间。

其实锦恢从去年就做了一套用大模型进行 i18n 的软件半自动化翻译工具 i18n haru，但是迫于如下两个原因我一直没有宣传：

- 我需要足够的时间（至少三个月）来完成我的用户体验和最佳实践
- 我懒，没时间宣传

今天算是一个契机吧，可能是因为阳光太辣，可能是因为咖啡太苦，总之，请允许我总结一下，AI时代下，我是如何在软件开发和文档编写中享受AI的红利。


## i18n haru

我们都知道，软件开发时，有一个东西叫做 i18n，也就是让你的软件可以支持多国语言。大家平时用的软件，在「用户偏好」这个栏目一般都会有这个选项。

锦恢开发的软件几乎都是自动支持多国语言的，比如 openmcp：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-485ee94b5d7572f34a6040944fae51ff_1440w.png" style="width: 100%;"/>
</div>

比如 Digital IDE 的 vcd 渲染器：

<div align=center>
<img src="https://document.kirigaya.cn/assets/introduction.i18n-example-f0c6df63.png" style="width: 100%;"/>
</div>

如你所见，这些软件都是支持 i18n 的，切换语言后，整个软件所有字样都会同步为你选中的语言。很神奇是不是，你猜猜我是如何优雅快速地实现的？

考虑下面这部分的文字：

```javascript
let message = '今天我们去了 拳击猫 吃了 菠萝披萨 这道菜，味道好极了';
```

最傻瓜的做法就是，你打开翻译软件，然后一个个语言翻译，然后在复制粘贴回来。试想一下，我们开发软件本来就已经很累了，结果还需要自己手动对一个简单的字符串一直这样 ctrl c ctrl v，你可要知道，一个软件里面需要翻译的字段可能上千个。这样做效率也太低了。

所以我开发了 i18n haru 插件，一款利用大模型帮你半自动进行软件开发 i18n 辅助的插件。想要安装的话，直接在 vscode 里面搜索 i18n haru 就能搜到：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-f2eb3026c379d493ac576c87d9eb4224_1440w.png" style="width: 100%;"/>
</div>

> cursor 和 trae 里面也都是支持的！

仓库链接如下：https://github.com/LSTM-Kirigaya/i18n-Haru
官方文档如下：https://document.kirigaya.cn/blogs/i18n-haru/main.html

基本使用非常简单，直接看这里就知道了：[通过大模型完成翻译](https://document.kirigaya.cn/docs/i18n-haru/usage.translation.html#%E9%80%9A%E8%BF%87%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%AE%8C%E6%88%90%E7%BF%BB%E8%AF%91)

对着你需要翻译的文本，ctrl i 一下就自动翻译了，整个过程2秒就能完成，而且大模型翻译软件中的专业术语翻译精度也比普通翻译软件好很多。

## ompipe 文档自动翻译管线

我们开发软件总是需要做配套的文档网站的是不是，特别是不少朋友的产品都要出海，那么基本上文档至少都得是中英双语，甚至更多。

我自己的 openmcp 文档网站 https://kirigaya.cn/openmcp/ 也是，目前是自动支持中英日三国语言：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9e52be804ef5218e2a28bfc92291e919_1440w.png" style="width: 100%;"/>
</div>

这依赖于我装载在我的助手 TIP 身上的自动 diff 翻译管线，每当我修改文档（我只写中文文档）后，如果需要部署，TIP 会调用我开发的 ompipe 文档自动翻译管线，这个管线会自动对比当前文档和上一次 commit 文档的差异，然后将差异部分翻译到英文和日文上去，然后完成部署。下面展示了我测试翻译管线时的场景。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-acc5a024316a5177b04bbf70485fadfc_1440w.png" style="width: 100%;"/>
</div>

顺带一提，这个翻译管线使用的可重入任务管理框架 ompipe 也是我开发的，欢迎 star 呀：https://github.com/LSTM-Kirigaya/ompipe

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-bc7ef83bccc5c8b344ae5bfde830d79e_1440w.jpeg" style="max-width: 100%;"/>
</div>

这里是锦恢，欢迎查看我的微信公众号「汇尘轩」获取实时的AI相关的技术咨询，你也可以关注我的个人网站[汇尘轩](https://kirigaya.cn/home)，如果你想要品味我的设计美学的话。不过我的 RSS 还没做，请等等。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-618caa8ee76a399c4f2d31bb8bbf84c8_1440w.png" style="max-width: 100%;"/>
</div>

## 总结

如你所见，AI 时代，我的 AI Agent 帮我自动完成了软件的 i18n 和文档的翻译。未来越来越多的重复劳动会被我开发的 AI 工具进行优化，如果对 AI Agent 开发感兴趣，欢迎为我的 OpenMCP 点个 star：https://github.com/LSTM-Kirigaya/openmcp-client

最近刚刚完成了自定义拓扑的工具测试回路

<div align=center>
<img src="https://pica.zhimg.com/80/v2-e2b4bd692386e72a2ecdf076880bfdbf_1440w.png" style="max-width: 100%;"/>
</div>

它在我开发各类 AI Agent 的初期起到了非常重要的作用，有关详情，欢迎加入 OpenMCP正式级技术组 来一起交流。如何加入「OpenMCP正式级技术组」？这是第一关，聪明的孩子总能找到我的踪迹。
或许巴别塔在这个时代真的可以再次建立起来。游戏开始越来越有趣了，不是吗？

---

## 为我的项目点个 star

最后如果喜欢的话，请为我的项目点赞，谢谢！

🔗 github：https://github.com/LSTM-Kirigaya/i18n-Haru
📄 i18n haru 文档：https://document.kirigaya.cn/blogs/i18n-haru/main.html
