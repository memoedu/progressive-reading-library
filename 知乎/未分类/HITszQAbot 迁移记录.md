# HITszQAbot 迁移记录

> [原文链接](https://zhuanlan.zhihu.com/p/363342119)

> 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布
## 前情提要

在大一大二期间维护招生机器人小哈也算我的一项业余爱好，这个项目的具体内容请见：

[叶峻峣：HITszQAbot 项目记录](https://zhuanlan.zhihu.com/p/158186707)然而去年暑假 TX 杀了一批 qq 机器人，小哈所依赖的酷Q也未能幸免于难：

[如何看待继晨风机器人后，各机器人宣布关闭？](https://www.zhihu.com/question/411468332/answer/1378155870)介于当时招生活动已经进入尾声，我也没有太多精力将小哈的依赖转移到其它 qq 机器人（似乎那个时候只有 mirai 存活）。

大半年过去了，一年一度的招生季又要来了，我也正好抽空看看有没有较好的迁移方案。

这次我选择了 nonebot 的升级版——[nonebot2](https://link.zhihu.com/?target=https%3A//v2.nonebot.dev/) 作为 qq 机器人开发框架，[go-cqhttp](https://link.zhihu.com/?target=https%3A//github.com/Mrs4s/go-cqhttp) 作为 qq 机器人客户端，实现招生问答功能。

代码在文末。

## 配置 go-cqhttp

总之首先要访问一下 [go-cqhttp](https://link.zhihu.com/?target=https%3A//github.com/Mrs4s/go-cqhttp)，去 release 里下载对应版本的安装包，我是直接在 windows 上测试的，所以就下载了 [go-cqhttp_windows_amd64.exe](https://link.zhihu.com/?target=https%3A//github.com/Mrs4s/go-cqhttp/releases/download/v1.0.0-beta2/go-cqhttp_windows_amd64.exe)。

建一个文件夹，把 exe 文件放进去，执行一下，具体安装方法请见：

[开始 | go-cqhttp 帮助中心](https://link.zhihu.com/?target=https%3A//docs.go-cqhttp.org/guide/quick_start.html)搞定后应该是这样的：

![](https://pica.zhimg.com/v2-b68f34b037794cbb286441fc86deecb4_1440w.png)
Image

config.yml 具体怎么配下面有说。

## 从 NoneBot 迁移到 NoneBot2

由于 NoneBot2 和 NoneBot 的主体差异不大，理论上把 NoneBot 的插件搬过去，稍微改亿点点就行了。我选择直接用 NoneBot2 的 nb-cli 新建一个项目，然后开始复用以前写的代码。

NoneBot2 具体怎么安装，请见：[https://v2.nonebot.dev/guide/](https://link.zhihu.com/?target=https%3A//v2.nonebot.dev/guide/)

这里要注意的是，由于我选择 go-cqhttp 作为 qq 客户端，所以 NoneBot2 这里要用 CQHTTP 适配器。详情见：[https://v2.nonebot.dev/guide/cqhttp-guide.html](https://link.zhihu.com/?target=https%3A//v2.nonebot.dev/guide/cqhttp-guide.html)，里面也说明了 config.yml 如何配置。

这里重点说一下迁移中我改了啥：

NoneBot：

from nonebot import on_command, CommandSession
from nonebot import on_natural_language, NLPSession, IntentCommand
from nonebot.helpers import context_id, render_expression
在之前的 NoneBot 里，有一个专用的自然语言处理模块，但是 NoneBot2 里它没了。于是我把它们换成了：

from nonebot import on_command
from nonebot.rule import to_me
from nonebot.typing import T_State
from nonebot.adapters import Bot, Event
import nonebot.adapters.cqhttp.message as message
这样可能很不直观，我简单说说哪些功能的函数被替换了：

@on_command(&#39;faq_local&#39;)
async def faq_local(session: CommandSession):
    raw_question = session.state.get(&#39;message&#39;)
# 替换为
@faq.handle()
async def faq_local(event: Event):
    raw_question = str(event.get_message())
    
reply = add_at(reply, session.ctx.get(&#39;user_id&#39;))
session.send(reply)
# 替换为
reply = add_at(reply, event.get_user_id())
faq.send(message.Message(reply)) #这里有坑，不用 Message 构造回复会使 CQ 码失效
具体怎么替换，还是建议多看看[https://v2.nonebot.dev/api/](https://link.zhihu.com/?target=https%3A//v2.nonebot.dev/api/)，api 熟悉了就方便实现旧功能。

## 测试

左边是 NoneBot，右边是 go-cqhttp

![](https://pica.zhimg.com/v2-8e83d05108149f70fb60b3b1eb2fd10e_1440w.jpg)
那个小哈它又回来啦！

![](https://pica.zhimg.com/v2-717145dd4dbbe2b1de607f145c9483f4_1440w.png)
Image## 结语

由于迁移挺仓促的，有很多坑都还没填，凑合用用，代码已经传到 GitHub 了，想学习的可以随便拿去看看：

[https://github.com/L-M-Sherlock/HITszQAbot](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/HITszQAbot)具体怎么部署我会慢慢更到 README 里面，先摸了。
