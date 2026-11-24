## 前言

最近实验室的小伙伴在使用 vscode 登录服务器时都遇到了连接失败的问题，作为一个 vscode 老油条，在下自然是帮助大家在 10s 内解决了问题。考虑到授人以鱼不如授人以渔，这篇文章我就讲讲 vscode-ssh 连接 服务器失败的简单原理和解决方案，这样就算阁下遇到其他类似的错误，也不必像现在这样狼狈地翻找博客了。

> 如果阁下急于寻找解决方案，可以参考[这篇文章](https://blog.csdn.net/chongbin007/article/details/126958840)，只不过在了解了原理后，阁下就会知道为什么我能在 10s 内而不是 1分钟 内帮大家解决问题了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9c3869a16b95784e605ffa8cc9a4b516_1440w.jpeg" style="width: 30%;"/>
</div>

---

## vscode 连接服务器的简易过程

### 善用 remote-ssh 的输出面板

我们之所以能够使用 vscode 连接 服务器实际上是依靠了 vscode 自带的插件 `Remote - SSH`，它自带一个同名的 输出 窗口，如下图：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-b04104455ca30d45cf86cd88e9d4a4cd_1440w.png" style="width: 60%;"/>
</div>

如果阁下知道这个输出窗口，就能直接看到当前 SSH 的连接状态，结合你那聪明的脑袋，就能猜出为什么会连接失败或者成功。这是解决 vscode SSH 连接失败最重要的工具，如果你找到这玩意儿的存在，很多博客阁下实则无需借鉴。

### 不单纯的 ssh 连接： vscode-server

vscode 的 SSH 之所以能够拥有比传统 SSH 更加强大的功能，是因为当你的 vscode 连接到远程服务器后，vscode会自动在 服务器上下载对应版本的 `vscode-server`，这个 server 会代劳一些基本的操作。这个 `server` 默认会下载在 `~/.vscode-server/bin/{commit-id}/` 文件夹下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2a44caf4d43ab001d2a51c7eb9d29130_1440w.png" style="width: 80%;"/>
</div>

上面的 af28b 打头的那一串东西就是 vscode 的 commit ID，这个 ID 必须和当前正在连接的 vscode 客户端的 ID 完全一致，你可以在你的 PC 端的命令行输入如下指令查看当前本机的 vscode 版本号和 ID ：

> 为啥要一致呢？因为 vscode 团队这两年经常在组件 API 上整花活，为了你在本地的插件也能在远程使用，所以尽量保持版本一致吧。。。

```bash
$ code --version
```

我的电脑上的输出：

```
1.85.0
af28b32d7e553898b2a91af498b1fb666fdebe0c
x64
```

第一行代表 vscode 的版本号，第二行代表 vscode 的 commit ID，第三行代表本地指令集。可以看到，我的commit ID 和服务器上的这个 server 的文件夹名是一致的。如果不一致呢？我们重新登录一台服务器，然后进入这台服务器的 `~/.vscode-server/bin` 文件夹下：

```bash
~/.vscode-server/bin $ ls
1a5daa3a0231a0fbba4f14db7ec463cf99d7768e
```

可以看到，这台服务器的 server 的 commit ID 和我的 PC 上的不一样，这个时候连接会出现什么情况呢？我们这个时候就可以打开之前提到的 remote-ssh 的输出面板了：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-855a93401938e8d39cfde10b68e660ff_1440w.png" style="width: 60%;"/>
</div>

可以看到，vscode的ssh客户端检测到版本不一致了，就会在远程服务器用 `wget` 去下载最新版本的 vscode server。然后安装（解压），初始化（配置参数，启动 code-server 和 自带的 nodejs 运行时）

>  因为我们实验室服务器网速太快了，右下角一开始应该是弹出“正在下载 Vs Code 服务器”

这个真实的下载链接，你完全可以通过 `ps ano | grep <用户名>` 查看 哪个 `wget` 进程里面看着像是 vscode-server 下载链接找到，考虑到一些朋友很懒，连 `ps` 都懒得敲，我就简单记录一下这个下载链接：

```
https://update.code.visualstudio.com/commit:${commit_id}/server-linux-x64/stable
```

比如我的当前客户端 commit ID 是 `af28b32d7e553898b2a91af498b1fb666fdebe0c`，那么，对应的 `code-server` 下载链接为：

```
https://update.code.visualstudio.com/commit:af28b32d7e553898b2a91af498b1fb666fdebe0c/server-linux-x64/stable
```

> 对了，忘记说了，上面这个链接是指向 CDN 服务器的，所以会被重定向，别想打歪主意。

我们都知道 vscode 的 CDN 服务器一直很迷，所有有的朋友在连接服务器时，如果赶上你们服务器网速不佳，就会让上面这个链接的下载变得很慢很慢，导致你的连接界面一直卡在等待中。

- 如果这个时候你信奉重启解决 99% 的问题，那么对不起，上面的这个下载 server 的 wget 进程是被挂载在后台的，并且它留下了一个 lock 文件，这个时候你再次连接服务器时，客户端发现这个 lock 文件在，就知道它的 前辈 连接下载未半而中道崩殂，于是就摆烂，你的右下角就会显示重试：1，2，3。（具体还得看 remote-ssh 的输出面板里面的内容）remote-ssh 的输出面板会不断刷新“下载进程正在进行，请重试”差不多这样的话。这个时候你如果去看 `~/.vscode-server/bin/{commit_id}/vscode-server.tar.gz` 的大小，你会发现，这个文件其实会慢慢变大。（大小大概是 49 MB 左右，你不确定就 发一个 head 请求）
- 如果你信奉重装解决 100% 的问题，那么如果你重装的版本恰好等于服务器上已经下载好的一个 server 的 commit ID，你在重装后就能成功进入，否则，还是出现和重启类似的错误。
- 如果你的 vscode 自动更新了，commit ID 自然也就变了，那么第二天重新登录服务器时自然也就需要重新下载服务器上的 server 了。所以绝对不是有人黑了你的用户目录！虽然我也有方法就是了嘿嘿。

以上就是 vscode-ssh 的连接原理和一些可能出现的现象了，如果阁下理解了上述的问题，那么自然也就知道如何解决它们了。

---

## 解决方案

### 最快的解决方案：移花接木

如果你确定你的 vscode 和你同一个服务器局域网的其他朋友的 vscode 都是同一个版本或者最新版本，那么最快的解决方案就是直接 NTR 你朋友的 vscode-server，简单直接：

```bash
$ scp -r yifanchang@172.16.120.2:/data/yifanchang/.vscode-server/bin/af28b32d7e553898b2a91af498b1fb666fdebe0c ~/.vscode-server/bin/
```

复制完后直接进入就ok了。



### 手动下载：浏览器版本

实在不行就只能自己手动下载喽，把上面的链接 `https://update.code.visualstudio.com/commit:af28b32d7e553898b2a91af498b1fb666fdebe0c/server-linux-x64/stable` 复制进浏览器（记得用你自己的commit ID），等待下载完成后，扔到你的服务器上解压，保证 `~/.vscode-server/bin/af28b32d7e553898b2a91af498b1fb666fdebe0c` 下有这么几个东西：

```bash
bin  extensions  LICENSE  node  node_modules  out  package.json  product.json
```



### 手动下载：服务器版本

有些追求优雅的同学不喜欢传来传去，很好！就喜欢追求优雅的朋友，如果你不希望在浏览器里翻来翻去，还要scp到服务器，你可以在服务器上直接下载，记得加上 `-c` 参数，它保证了就算下载中途失败，后续下载也不需要重新开始（其实就是文件字节数扔到请求头的range参数里面）：

```bash
wget -c https://update.code.visualstudio.com/commit:af28b32d7e553898b2a91af498b1fb666fdebe0c/server-linux-x64/stable
```

下载会得到一个 `vscode-server.tar.gz`，和上面一样，解压后放到正确的位置。

---


## 2024.04.10 日志
新版本的 vscode-ssh 把下载路径变成了 /tmp 下的一个随机路径，导致之前的方法失效。解决方法：把 remote-ssh 插件回退到 版本 v0.107.2023091115 ，这个版本比较稳定。点击下方下载：

[https://marketplace.visualstudio.com/_apis/public/gallery/publishers/ms-vscode-remote/vsextensions/remote-ssh/0.107.2023091115/vspackage](https://marketplace.visualstudio.com/_apis/public/gallery/publishers/ms-vscode-remote/vsextensions/remote-ssh/0.107.2023091115/vspackage)