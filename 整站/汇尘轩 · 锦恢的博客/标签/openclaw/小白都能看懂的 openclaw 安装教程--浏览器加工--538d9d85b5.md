虽说是小白都会的教程，但是仍然需要我的读者熟练掌握如下技能：

* 会开机关机 Windows

* 会操作键盘和鼠标

* 有汉字和少许英文的基本阅读能力

* 知道如何打开 Windows 的命令行终端

* 具备多模态识别能力（说人话：能看懂我的截图在说什么）

* 看得懂我的笑话和梗图

废话不多说，直接开搞。AI时代的博客会发生巨大的不同。从下面第一步操作开始就是。

## 第一步. 安装你的万能助手 qwen 或者 claude code

这是一个本地的AI助手，可以通过比你聪明1万倍的方法操作本地的终端1秒钟写出1万行你看不懂的代码来快速调度你计算机的资源。有了它，你就再也不用担心网上那些大佬贴出来的的代码为什么在你的本地报运行之后会出现1万个报错。这些AI助手都会帮你自动解决。

> 当然，不要让它帮你清理磁盘！！小心删库跑路！！想要使用AI清理磁盘，可以试试我写的智能磁盘助手 [让 AI 帮你快速整理磁盘，磁盘菜鸟 DiskRookie 内测 0.1.2 版本发布](https://zhuanlan.zhihu.com/p/2003256815694137034)

目前主流的可以跑在终端上的助手有 qwen，claude code，codex 和 opencode，中间要两个要收费，旁边两个部分免费。

因为 qwen 每天有2000次免费的使用次数，所以我优先说如何安装 qwen。

为了让 qwen 这样的AI助手可以运行，我们首先需要安装基础的编程环境 [nodejs](https://zhida.zhihu.com/search?content_id=270829536&content_type=Article&match_order=1&q=nodejs&zhida_source=entity)，自己去 B站 上搜【如何安装 NodeJS】即可，你随便找个高赞的点进去安装就完事。注意，我们一定要安装 node22 或者 node24（不要安装别的版本！！！）：

![](https://pic3.zhimg.com/v2-536c2986c65a0180c32a6ef5d6ef9500_1440w.jpg)

相信你在安装的过程中一定知道了如何打开命令行终端对吧？

打开命令行，输入下面的命令安装 qwen

```
npm install -g @qwen-code/qwen-code@latest
```

安装完成后，直接按下 qwen 进入助手对话框，然后选择第一个认证方式 Qwen OAuth，按下回车

![](https://pica.zhimg.com/v2-30926eb6f5b6916d2e7fc55b0cb63fe2_1440w.jpg)

然后会跳出一个链接和一个二维码，将链接扔进浏览器中选择账号进行登录即可（我选的是 github 账号）。

![](https://pica.zhimg.com/v2-200ce8cdadda751abaf7ebb6692e72ec_1440w.jpg)

登录完成后回到命令行，qwen 助手就可以使用了。

此时按下 shift + tab，输入框会从蓝色变成红色，意味着 YOLO 模式被激活（全自动执行模式，完全不需要你操心）

![](https://pic2.zhimg.com/v2-236eb7a5691f3f1a015fa669864892ed_1440w.jpg)

如果你已经成功完成了第一步，那么恭喜你，游戏已经结束了。

## 第二步. 安装 [cmake](https://zhida.zhihu.com/search?content_id=270829536&content_type=Article&match_order=1&q=cmake&zhida_source=entity)

在 qwen 里面输入

【请帮我安装 cmake 并暴露到当前的环境变量中，并帮我验证】

然后按下回车。

## 第三步. 安装 openclaw

在 qwen 里面输入

【阅读 <https://openclaw.ai/> 后，帮我使用 npm 全局安装 openclaw，并帮我验证】

然后按下回车。

---

## 配置 openclaw

好，openclaw 安装完成。

后面就是配置 openclaw 了，在命令行输入

```
openclaw onboard
```

等待一会儿进入 openclaw 配置页面

![](https://pic1.zhimg.com/v2-7c5f2a83526c9607c301edb98c2539b2_1440w.jpg)

通过上下左右方向键选择选项，按下回车确认，懂？

选择 Yes, QuickStart, Qwen（你也可以选择别的模型，qwen 单纯配置容易）

![](https://picx.zhimg.com/v2-8bac31c8166aeb568bd524ee71fdf69d_1440w.jpg)

还是一样，进入上面链接进行认证，然后，还是和上面一样的操作，完成登录认证。

然后选 Keep current

然后选择社交媒体，国内用户选择 [Feishu](https://zhida.zhihu.com/search?content_id=270829536&content_type=Article&match_order=1&q=Feishu&zhida_source=entity)/Lark (飞书)

![](https://pic4.zhimg.com/v2-b2e9e779c663363ff98421b836898b23_1440w.jpg)

> 如果想要支持更多的中国大陆使用的社交平台，比如说QQ企业微信、钉钉，可以看这个项：[https://github.com/BytePioneer-AI/openclaw-china](https://link.zhihu.com/?target=https%3A//github.com/BytePioneer-AI/openclaw-china)

后面一路选择默认的回车，直到遇到下面这个

![](https://pic3.zhimg.com/v2-f17d35a0d9c049dd509d94c7110050ae_1440w.jpg)

接下来我们就需要依靠飞书的强大平台能力了。

你首先需要先下载并创建对应的飞书账号，然后创建组织，这一套都是基本流程了。不懂的朋友请自行查看飞书的基本使用方法。

完成上面的一系列操作之后，让你的客户端和浏览器的飞书各个应用都登录你当前的组织和账号，点击 [飞书开放平台](https://link.zhihu.com/?target=https%3A//open.feishu.cn/%3Flang%3Dzh-CN) 进入飞书开发者平台。点击右上角的开发者后台。选择机器人

![](https://pic4.zhimg.com/v2-cf1074272e22d076abf6c3710296f29d_1440w.jpg)

简单配置一下介绍和头像

![](https://picx.zhimg.com/v2-6301d420d80a675e3714291926e81a73_1440w.jpg)

然后点击【权限管理】->【批量导入/导出权限】

![](https://pic4.zhimg.com/v2-688cc4dfb4e7275c7c43d98a31ce321d_1440w.jpg)

会弹出一个导入窗口

![](https://pica.zhimg.com/v2-53649ff25d63b1c5eb5ef7fdfb06a03c_1440w.jpg)

把里面的内容全部删除，然后把下面的这一坨东西复制后粘贴进去。

```json
{
  "scopes": {
    "tenant": [
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "base:app:copy",
      "base:app:create",
      "base:app:read",
      "base:app:update",
      "base:collaborator:create",
      "base:collaborator:delete",
      "base:collaborator:read",
      "base:dashboard:copy",
      "base:dashboard:read",
      "base:field:create",
      "base:field:delete",
      "base:field:read",
      "base:field:update",
      "base:form:read",
      "base:form:update",
      "base:record:create",
      "base:record:delete",
      "base:record:read",
      "base:record:retrieve",
      "base:record:update",
      "base:role:create",
      "base:role:delete",
      "base:role:read",
      "base:role:update",
      "base:table:create",
      "base:table:delete",
      "base:table:read",
      "base:table:update",
      "base:view:read",
      "base:view:write_only",
      "base:workflow:read",
      "base:workflow:write",
      "bitable:app",
      "bitable:app:readonly",
      "calendar:room:readonly",
      "contact:user.assign_info:read",
      "contact:user.base:readonly",
      "contact:user.department:readonly",
      "contact:user.dotted_line_leader_info.read",
      "contact:user.email:readonly",
      "contact:user.employee:readonly",
      "contact:user.employee_id:readonly",
      "contact:user.employee_number:read",
      "contact:user.gender:readonly",
      "contact:user.id:readonly",
      "contact:user.job_family:readonly",
      "contact:user.job_level:readonly",
      "contact:user.phone:readonly",
      "contact:user.subscription_ids:write",
      "contact:user.user_geo",
      "corehr:file:download",
      "docs:doc",
      "docs:doc:readonly",
      "docs:document.comment:create",
      "docs:document.comment:read",
      "docs:document.comment:update",
      "docs:document.comment:write_only",
      "docs:document.content:read",
      "docs:document.media:download",
      "docs:document.media:upload",
      "docs:document.subscription",
      "docs:document.subscription:read",
      "docs:document:copy",
      "docs:document:export",
      "docs:document:import",
      "event:ip_list",
      "im:app_feed_card:write",
      "im:biz_entity_tag_relation:read",
      "im:biz_entity_tag_relation:write",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.announcement:read",
      "im:chat.announcement:write_only",
      "im:chat.chat_pins:read",
      "im:chat.chat_pins:write_only",
      "im:chat.collab_plugins:read",
      "im:chat.collab_plugins:write_only",
      "im:chat.managers:write_only",
      "im:chat.members:bot_access",
      "im:chat.members:read",
      "im:chat.members:write_only",
      "im:chat.menu_tree:read",
      "im:chat.menu_tree:write_only",
      "im:chat.moderation:read",
      "im:chat.tabs:read",
      "im:chat.tabs:write_only",
      "im:chat.top_notice:write_only",
      "im:chat.widgets:read",
      "im:chat.widgets:write_only",
      "im:chat:create",
      "im:chat:delete",
      "im:chat:moderation:write_only",
      "im:chat:operate_as_owner",
      "im:chat:read",
      "im:chat:readonly",
      "im:chat:update",
      "im:datasync.feed_card.time_sensitive:write",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.group_msg",
      "im:message.p2p_msg:readonly",
      "im:message.pins:read",
      "im:message.pins:write_only",
      "im:message.reactions:read",
      "im:message.reactions:write_only",
      "im:message.urgent",
      "im:message.urgent.status:write",
      "im:message.urgent:phone",
      "im:message.urgent:sms",
      "im:message:readonly",
      "im:message:recall",
      "im:message:send_as_bot",
      "im:message:send_multi_depts",
      "im:message:send_multi_users",
      "im:message:send_sys_msg",
      "im:message:update",
      "im:resource",
      "im:tag:read",
      "im:tag:write",
      "im:url_preview.update",
      "im:user_agent:read",
      "vc:meeting.all_meeting:readonly",
      "vc:meeting:readonly"
    ],
    "user": [
      "aily:file:read",
      "aily:file:write",
      "base:app:copy",
      "base:app:create",
      "base:app:read",
      "base:app:update",
      "bitable:app",
      "bitable:app:readonly",
      "contact:user.assign_info:read",
      "contact:user.base:readonly",
      "contact:user.department:readonly",
      "contact:user.department_path:readonly",
      "contact:user.dotted_line_leader_info.read",
      "contact:user.email:readonly",
      "contact:user.employee:readonly",
      "contact:user.employee_id:readonly",
      "contact:user.employee_number:read",
      "contact:user.gender:readonly",
      "contact:user.id:readonly",
      "contact:user.job_family:readonly",
      "contact:user.job_level:readonly",
      "contact:user.phone:readonly",
      "contact:user.subscription_ids:write",
      "contact:user.user_geo",
      "contact:user:search",
      "docs:doc",
      "docs:doc:readonly",
      "docs:document.comment:create",
      "docs:document.comment:read",
      "docs:document.comment:update",
      "docs:document.comment:write_only",
      "docs:document.content:read",
      "docs:document.media:download",
      "docs:document.media:upload",
      "docs:document.subscription",
      "docs:document.subscription:read",
      "docs:document:copy",
      "docs:document:export",
      "docs:document:import",
      "im:chat",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.announcement:read",
      "im:chat.announcement:write_only",
      "im:chat.chat_pins:read",
      "im:chat.chat_pins:write_only",
      "im:chat.collab_plugins:read",
      "im:chat.collab_plugins:write_only",
      "im:chat.managers:write_only",
      "im:chat.members:read",
      "im:chat.members:write_only",
      "im:chat.moderation:read",
      "im:chat.tabs:read",
      "im:chat.tabs:write_only",
      "im:chat.top_notice:write_only",
      "im:chat:delete",
      "im:chat:moderation:write_only",
      "im:chat:read",
      "im:chat:readonly",
      "im:chat:update",
      "im:message",
      "im:message.pins:read",
      "im:message.pins:write_only",
      "im:message.reactions:read",
      "im:message.reactions:write_only",
      "im:message.urgent.status:write",
      "im:message:readonly",
      "im:message:recall",
      "im:message:update"
    ]
  }
}
```

然后一路点击申请或者确认。

然后点击创建版本

![](https://pica.zhimg.com/v2-3d62d96879ab6055fefdb87ad034d6c8_1440w.jpg)

![](https://pic1.zhimg.com/v2-8ea35f21c203b583a3cc201dbb0cab7e_1440w.jpg)

一路滚到最下面点击保存，然后点击申请审批，然后让组织的管理员审批一下，下面是管理员视角：

![](https://pic3.zhimg.com/v2-3559b56bbdef1a22e8bc55ea81c194c6_1440w.jpg)

![](https://pic1.zhimg.com/v2-edf4a93cab2a550c808f59bd71f6df3e_1440w.jpg)

![](https://pic1.zhimg.com/v2-3fd85ce95ecc6ff82073b21364a01074_1440w.jpg)

然后我们在飞书创建一个群聊：

![](https://pica.zhimg.com/v2-e7b002c2f9ff40f7cd51c258d87d12de_1440w.jpg)

点击设置：

![](https://picx.zhimg.com/v2-f5dedbcea1f2a8244613075150be811b_1440w.jpg)

点击群机器人

![](https://pica.zhimg.com/v2-d4dce67ded9e2625f9ec3f934fec6268_1440w.jpg)

添加我们刚刚创建的机器人

![](https://pic4.zhimg.com/v2-adbfe5786b703ba595d7a8844ae0d7c1_1440w.jpg)

ok，完成后，我们回到开发者平台，点击【凭证与基础信息】

![](https://pic2.zhimg.com/v2-b6e5b1f64d79df0b048ccdf54a975ba7_1440w.jpg)

右侧有一个 AppID 和 App Secret，分别复制并按照要求粘贴到 openclaw 的配置页面：

![](https://pic3.zhimg.com/v2-53354aa78daa2848daf5dc5e9f45ddac_1440w.jpg)

后续分别选择 Feishu ([feishu.cn](https://link.zhihu.com/?target=http%3A//feishu.cn/)) - China 和 Open - respond in all groups (requires mention)

加油，就快完成了！

只差临门一脚，openclaw 本质是一个大模型的网关，其本身并不具备任何的Agent 的能力。所以我们还需要为它配置一些基础的技能包，让它可以执行更加复杂的操作。

所以下一个选择 yes，然后可以自由配置一些技能组，下面分享一下我自己配置的：

* himalaya：让AI可以自由的管理你的邮箱。

* mcporter：让AI可以自由快速MCP服务器，你只需要简单理解为可以给 AI 外挂更加强大的功能。理论上可以接管任何通过电脑接管到的虚拟世界和物理世界。

* nano-pdf：让AI可以阅读你上传的PDF文件。

* summarize：让AI可以总结你分享在群聊中的网页，本地文件或者是一些社交媒体。

然后一路回车即可。

这几个国外的服务可以暂时全部选择 No，需要的时候，后面再回来配置就行。

![](https://pica.zhimg.com/v2-cbe80f09014812acbaeb59434ed7ff36_1440w.jpg)

生命周期事件我选择的是如下的几个：

![](https://pic4.zhimg.com/v2-7e2b5b190553476128821441e44ddcfd_1440w.jpg)

AI 管理模块我选择的是 [TUI](https://zhida.zhihu.com/search?content_id=270829536&content_type=Article&match_order=1&q=TUI&zhida_source=entity)，如果你当前安装 openclaw的机器和你当前的PC机在同一个局域网内，那么你可以选择第二个。

![](https://pic1.zhimg.com/v2-3c69a976de6e4da57bd4294a1f355164_1440w.jpg)

等待初始化完成后，你就能看到如下的东西：

![](https://pic2.zhimg.com/v2-8cbca316b3bae0262c355da621b6c27d_1440w.jpg)

只差最后一步，我们就可以彻底完成配置了。现在懵懂的AI已经初醒，它想要知道一些基础的信息，我们只需要回答它就行了。下面是我的回答：

```
你是 Tiphareth，你是冷静睿智的助手，我是锦恢，你的搭档。
你是我的助手，你可以直接在飞书里 @我 或者通过邮箱 xxx 来给我发送消息。
我们所在的时区是北京时区。
```

![](https://pic2.zhimg.com/v2-fe2acb3fbf96587816a295259dcbd823_1440w.jpg)

现在配置完成了。

然后按两次 Ctrl C 退出对话框，重启 openclaw：

```bash
openclaw gateway restart
```

进入飞书开发者平台，我们还需要为 AI 创建接受和发送消息的信道，点击【事件与回调】，点击订阅方式右侧的笔按钮，选择长连接，点击保存

![](https://pic3.zhimg.com/v2-f11298091c68eca17608073cc2574504_1440w.jpg)

然后点击添加事件按钮，找到并勾选接收消息

![](https://pic1.zhimg.com/v2-8977a99702e08373bade0f2e7687c806_1440w.jpg)

然后再次发布这个版本就好了。

---

## **享受胜利的果实**

然后我们就可以非常愉快的将openclaw 当成我们飞书群的新员工来和它合作的。简单测试一下消息的通信吧。在刚刚创建的那个飞书群里面，我们@一下这个新的机器人，简单说几句话。

![](https://pic3.zhimg.com/v2-311b939c46f08977f0693d0db96ba650_1440w.jpg)

可以看到我们的机器人不仅正常的给我们进行了消息的回复，甚至我们的配置过程中出现了一些小的错误，它都告诉我们应该怎么解决了，怎么会有这么智能的机器人呢？

而如何根据AI的提示来解决他自身的错误，就当我给大家留的一个课堂小作业了。毕竟师傅领进门修行靠个人，我要是全把问题给你们解决了，那还要你们干什么？

---

配置完了，虽然看起来很好玩，但是我相信很多观众都想问出一句话，那么 openclaw 到底有什么用呢？

![动图封面|thumbnail](https://pic1.zhimg.com/v2-2fcec3c2ad0ad7a7b266d7922fa9f91e_b.jpg)

点赞超100（加我的毕业论文写完了），下一篇文章我就简单演示一下，如何让 openclaw 成为你的 HR 小员工，使得它可以自动的帮你查看邮箱里投他人投递的简历，并帮你自动将数据提取，归档到飞书多维表格，并自定义人物画像。

并且你可以直接向它派发任务，它会帮你亲自料理后事。比如你用语音输入：“帮我和张三约个后天下午三点的面试，时长30分钟”，openclaw 就会自动的帮你创建对应的飞书会议，并将提醒日程安排在你的日历里面，并且将会议链接和问候语发送给对应的应聘者。