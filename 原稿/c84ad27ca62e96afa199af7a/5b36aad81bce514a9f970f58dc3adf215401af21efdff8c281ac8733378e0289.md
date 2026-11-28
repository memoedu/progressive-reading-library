当我们把 Mirai 成功启动后，接下来就需要给 Mirai 前一个靠谱的前端了。

最终大致的结构如下图所示：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-289dd9e1113aa661f3b2942de019a7dc_1440w.png" style="width: 80%;"/>
</div>

## 安装 Mirai 的通信器——mirai-api-http

因为我的目的是做一个知识库机器人，毫无疑问地需要用到 Python，所有我希望可以基于 Python 进行开发。

Mirai 本身是用 Java 方言 Kotlin 写的，因此我们很自然地想到可以基于 http
或者 websocket 的方式把 Mirai 的数据发出去，再处理。好在有人已经用 Kotlin 给我们写好了这个插件：[mirai-api-http](https://github.com/project-mirai/mirai-api-http)，具体安装方法可以进入项目主页查看。因为上篇文章中，我用的是 Mirai2 （只要你是最近下载的，都是 Mirai2），所以可以运行如下的命令安装：

```bash
# $MIRAI_HOME 是 Mirai 的安装路径
$ cd $MIRAI_HOME
$ ./mcl --update-package net.mamoe:mirai-api-http --channel stable-v2 --type plugin
$ ./mcl -u
```

安装完成后退出 Mirai Console，此处应该有一个文件`./config/net.mamoe.mirai-api-http/setting.yml`会自动生成，编辑这个文件：

```yaml
adapters:
  - ws
enableVerify: true
verifyKey: 1234567890
debug: false
singleMode: false
cacheSize: 4096

adapterSettings:
  ws:
    host: localhost
    port: 8092
    reservedSyncId: -1
```

> websocket 比 http 更加稳定，所以此处使用 websocket 作为通信方法，更多的配置详情，请移步 [mirai-api-http](https://github.com/project-mirai/mirai-api-http) 项目主页

其中，我们上次文章中，`8091`端口已经给了签名服务器，所以这次我们给http-api `8092`这个端口。

然后重新运行 Mirai 客户端：

```bash
$ ./mcl -u
```

打开新的终端，查看端口占用：

```bash
$ netstat -l | grep 8092
tcp6       0      0 127.0.0.1:8092          [::]:*                  LISTEN
```

不为空就是正常，说明现在我们可以通过 `8092` 端口与 Mirai 取得联系，从而获取 QQ 机器人的信息。

## 安装 nonebot2 前端

接下来就是基于 `8092` 端口与 Mirai 建立 Websockets 连接。但是问题是我懒得去看 `mirai-http-api` 的文档，不想自己去抽象数据结构做QQ信息的结构化处理。幸好社区已经有不少的使用于 Python 的 API了，其中规模最大的就是 nonebot2 这个项目了。这个项目提供了大量的通信媒体后端的适配器，当然，nonebot2 也支持 mirai 作为后端。诸位如果对 Python 熟悉，可以直接移步他们的官网： [nonebot.dev](https://nonebot.dev/)。

首先安装 nonebot2 库，因为 nonebot2 需要脚手架才能启动，所以需要借助 `pipx` 下载脚手架软件：

```bash
$ pip install --user pipx
$ pipx ensurepath
$ pipx install nb-cli
```

> 如果你是 zsh 用户，那么 pipx ensurepath 返回的路径需要手动加入到 环境变量 `$PATH` 中。

然后我们创建一个项目：

```bash
$ cd ~/Project
$ nb create
```

下面几个选项：

```
[?] 选择一个要使用的模板: bootstrap (初学者或用户)
[?] 项目名称: TestBot
[?] 要使用哪些驱动器? FastAPI (FastAPI 驱动器) & websockets (websockets 驱动器)
[?] 要使用哪些适配器? mirai2 (为nonebot2 添加mirai_api_http2.x的兼容适配器)
[?] 立即安装依赖? (Y/n) y
[?] 创建虚拟环境? (Y/n) n
[?] 要使用哪些内置插件? echo
```

等待安装完成，进入项目，并修改配置：

```bash
$ cd TestBot
# 根据版本不同，你的项目里面可能不是 .env，反正就是一个类似的东西
$ vim .env.prod
```

写入如下内容：

> 我们把 `./config/net.mamoe.mirai-api-http/setting.yml` 简称为 setting

```toml
DRIVER=~fastapi+~websockets    # 自动生成的
MIRAI_HOST=localhost           # setting 中 adapterSettings.ws.host
MIRAI_PORT=8092                # setting 中 adapterSettings.ws.port
VERIFY_KEY=xxx                 # setting 中的 verifyKey
MIRAI_QQ=[xxx]                 # Mirai 登录的 QQ 号，虽然支持监听多个，但是我不建议这么多
HOST=0.0.0.0                   # 就这么写，别问我为什么
PORT=8093                      # 挑一个不冲突的
NICKNAME=["Tiphereth", "Bot"]  # 机器人的昵称
```

然后在当前目录下创建一个 `bot.py`：
```python
import nonebot
from nonebot.adapters.mirai2 import Adapter as Mirai2Adapter  # 避免重复命名

# 初始化 NoneBot
nonebot.init()

# 注册适配器
driver = nonebot.get_driver()
driver.register_adapter(Mirai2Adapter)

# 在这里加载插件
nonebot.load_builtin_plugins("echo")  # 内置插件
# nonebot.load_plugin("thirdparty_plugin")  # 第三方插件
# nonebot.load_plugins("awesome_bot/plugins")  # 本地插件

if __name__ == "__main__":
    nonebot.run()
```


## 运行 nonebot2 前端

以上工作准备好后，直接运行前端即可：

```bash
$ nb run
```

如果一切配置正确，此时就能在控制台看到这个机器人好友发送的消息：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-79e172dd2ad7088bd369d1fb92bf0bae_1440w.png" style="width: 80%;"/>
</div>

因为我们装了 `echo` 插件，可以简单地通过 `/echo xxx`的方法来让机器人来复读我们的话：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-5e904e4f651df5398ee70cb7c745f037_1440w.png" style="width: 70%;"/>
</div>

下篇文章就简单讲讲如何通过编写自己的 nonebot2 插件来自定义输入一些语句，甚至接入本地大模型实现长对话。