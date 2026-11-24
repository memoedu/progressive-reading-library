在之前的文章中 <https://zhuanlan.zhihu.com/p/2011616196106141860> ，我们成功的安装了openclaw，并在飞书中和它完成了交互，让它成为你的一个助手。但对于很多朋友来说，微信和QQ恐怕才是日常生活中最常用的两大通信工具，那么我们能不能让openclaw也能接入QQ或者是微信呢？

那自然是可以的。今天我就拿QQ为例子，简单讲一讲如何使用我发布的 openclaw 插件，来让你的openclaw 可以接入QQ。

## 为什么不用 QQ 开放平台接入

很多朋友可能和会和我反驳说，QQ官方不是提供了接口和机器人来让 openclaw 接入吗？这是当然。但是如果你想要让你的 QQ 机器人进入千人大群，那么 QQ 官方提供的方法是做不到的。因为 QQ 官方的机器人在没有审核的前提下，只能加入人数少于 20 人的群，而腾讯的审核又是极其繁琐，要交一堆材料。

## 将 openclaw 接入 QQ

### 第一步：安装 onebot 服务端

QQ的 onebot 服务端网上存在好几个社区维护的版本。我比较推荐的是 [napcat](https://github.com/NapNeko/NapCatQQ) 和 [mirai](https://github.com/mamoe/mirai)。

两者都提供了非常丰富的文档，尤其是napcat，甚至帮你把签名服务器都搞定了。你要做的就只是启动完之后扫个码，然后就结束了。下面是 napcat 的官方文档：        [napneko.github.io](https://napneko.github.io/)

### [NapCatQQ | 现代化的基于 NTQQ 的 Bot 协议端实现](https://napneko.github.io/)

基于NTQQ现代化协议端Bot框架

官方文档写的比我说的详细多了，我就不再赘述了。你根据文档上的流程，正常的安装完成，启动你的QQ号就可以了。

### 第二步：配置 onebot 协议

napcat 的配置有 TUI 和 WebUI 两种。我先讲 TUI 的，WebUI 的其实差不多。

sudo napcat 进入 TUI 后，选择【配置】：

![](https://picx.zhimg.com/v2-9bbe9492af23b384c3d665609e5da5ad_r.jpg?source=d16d100b)

选择你的账号，点【配置服务】，然后选择 WebSocket 服务端（正向ws）

![](https://picx.zhimg.com/v2-2a0dbbb1c520de500a42c6819e6412a7_r.jpg?source=d16d100b)

然后配置一下，按照我这样填，名称随便起，token也就是密码，你自己起一串，然后记住。

![](https://pica.zhimg.com/v2-689d921b10f539343c48e36027c3d6fe_r.jpg?source=d16d100b)

消息格式选择数组，启用选项用下面这组：

![](https://picx.zhimg.com/v2-a17f54dc16813d3506b056dab0c18d6e_r.jpg?source=d16d100b)

然后就配置完成了。

WebUI 配置大同小异，基本都是一样的，我就简单说一下入口，当你进入 WebUI，选择网络配置

![](https://pic1.zhimg.com/v2-182a5132124c3d647a72160a5acbd930_r.jpg?source=d16d100b)

点击【新建】，选择 WebSocket 服务器，然后会跳出来一张表单，这张表单的内容和上面的TUI的表单内容完全一致，我就不再赘述了。照着我上面说的填写就可以了。

### 第三步：扩充 openclaw 的频道

openclaw 默认的频道里是没有 onebot 的。我前天简单写了一个晚上，把onebot给支持了一下，也求大家多多点赞 [GitHub - LSTM-Kirigaya/openclaw-onebot: onebot adapter for openclaw · GitHub](https://github.com/LSTM-Kirigaya/openclaw-onebot)。

在 openclaw 里，通过下面一行命令就能完成安装配置：

```
openclaw plugins install @kirigaya/openclaw-onebot
openclaw onebot setup
```

完成这两行代码的运行之后，会出现TUI的一个表单。这个表单里填写的内容就是我们之前配置onebot 的 websocket 服务器时的内容，别的选项填写下面的👇，Access Token 就是你刚刚起的token 密码

![](https://pic1.zhimg.com/v2-3c8088f6ecbf80a71dce41d2d4ee2e22_r.jpg?source=d16d100b)

完成配置后，openclaw 会提示你重启，你只需要正常重启就好了：

```
openclaw gateway restart
```

### 验收成果！

在我的设计中，私聊，还有群聊的艾特都会触发机器人的正常回复，所以你这样测试就可以了：

![](https://pica.zhimg.com/v2-e2df44b749922c9be1b5318cd36d0fab_r.jpg?source=d16d100b)

可以看到我增加了一个小细节，当机器人刚收到消息的时候，他会为你的消息增加一个咖啡杯的表情，你也可以通过这一点来快速判断当前的机器人是否正常工作。

### （可以跳过）课外拓展——什么是 onebot 协议

> 我稍微解释一下什么是 onebot，以及什么是 onebot 服务端。onebot 其实是一种协议，这个协议是由社区倡导的，它规定了一个即时通信软件，需要实现哪些基本功能。比如说接收消息，发送消息、发送图片、上传文件等等。
>
> 比如QQ和微信里面，用户向群聊发送一个文本消息的函数都是不一样的。可能在QQ里就叫做 `qq_send_text`，在微信里就做就叫做 `wechat_send_text`。而且这两个函数的接口和返回值也不一样，这给开发者带去的一些困扰和麻烦。而在 onebot 协议里，发送群聊文本就是 `send_group_msg`，onebot 屏蔽了各大通信软件的实现差异，让开发者可以更加专注开发出可移植性强的机器人或者客户端软件。
>
> 只要一个通信软件它本身实现了onebot协议，那么我们就可以只访问这些标准的 onebot 的协议接口，而无需关心具体这个通信软件本身的底层实现，来实现基本的消息接收与发送。而实现了onebot的协议的通信程序就是onebot的服务端。而我们通过onebot提供的标准接口来操作通信软件，这样编写的一套程序，也就是onebot的客户端。
>
> 从上面的文字你也能看出来，社区编写的弯部的客户端主要就是两种用途：
>
> 1. 觉得官方实现的通讯软件太丑了，或者说有些功能无法实现。那么他们有的人就可能会基于one部协议做一个新的GUI客户端。而且假设QQ，微信，飞书都有人实现了onebot的服务端，那么你就可以只通过这样一个onebot的客户端软件界面来同时接收QQ微信和飞书的消息。
>
> 2. 实现一些自动化的机器人或者是工作流。比如说最常见的需求就是每天定期的往群里发晨报，或者定期的对群聊消息进行总结和热点分析，这也可以通过编写 onebot 的客户端程序来实现。
>
> onebot 协议在网上有很多版本，目前支持最广的是V11这个版本。你们可以通过下面这个项目来查看V11版本的具体协议细节。此处就不再赘述了。
>
> [GitHub - botuniverse/onebot-11: OneBot 11 标准 · GitHub](https://github.com/botuniverse/onebot-11)

## 结语

好了，相信通过今天的博客，大家都知道如何在QQ里自由的部署你的 openclaw 了，也都知道什么是 onebot 的协议了。显然，如果你能在黑市上找到有人实现的微信的 onebot 协议，你也可以完全按照上面一模一样的方法，来把微信接入你的 openclaw，实现你的一些私域的管理或者是自动化的营销。

不过可惜的是，微信的管控一直非常严格。如果你真的胆敢把实现好的微信onebot服务端公开到github上，相信你光是吃律师函就能吃到饱。

![动图](https://picx.zhimg.com/v2-1edb1fff0f4cafb06a0b1b4e356b8c76_720w.webp?source=d16d100b)

在后面我会持续更新一些 openclaw 相关的使用技巧。这里面能做的事情确实很多啊，不过眼下，我得先睡会觉了。