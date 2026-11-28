## 前言

### 登录失效

最近因为要使用QQ机器人来给群友回答一些技术问题，所以在尝试运行之前我自己写的QQ机器人项目。但是，不愧是QQ，又一次破坏了登录规则，让我们之前的QQ登录程序全部失效，出现了大量的45错误码。

### 签名服务器 Sign Server

通过进一步的了解，目前的QQ会对发送的数据包进行验证，验证不通过就会爆出45错误码，次数过多甚至会直接封号。在Mirai社区诸位大佬的努力下，Mirai社区出现了[第三方签名插件](https://github.com/cssxsh/fix-protocol-version)和对应的[第三方签名服务器运行程序](https://github.com/fuqiuluo/unidbg-fetch-qsign/tree/1.1.9)。

### 为什么使用Mirai

我之前一直用的`go-cqhttp`，这是一款非常优秀的QQ后端，但是对于一些高并发的群消息，这个启动器有时候可能会吞消息，导致机器人的回复不及时。因此，新版本我打算使用mirai来作为后端。这篇文章简单记录一下签名服务器时代，如何正确在Linux服务器上配置和下载mirai。

---

## 环境准备

### JDK 版本 > 11

首先先确保你的服务器上有版本大于11的java运行时:

```bash
$ java -version
openjdk version "11.0.20" 2023-07-18
OpenJDK Runtime Environment (build 11.0.20+8-post-Ubuntu-1ubuntu120.04)
OpenJDK 64-Bit Server VM (build 11.0.20+8-post-Ubuntu-1ubuntu120.04, mixed mode, sharing)
```

我的服务器（openjdk "11.0.20"）是满足的。


### 设置环境变量 JAVA_HOME

检查你是否有环境变量`JAVA_HOME`：
```bash
$ echo $JAVA_HOME
/usr/lib/jvm/java-11-openjdk-amd64
```
我也是有的，没有的话，记得在`~/.bashrc`或者`~/.zshrc`中`export`一下。


### glibc 版本 > 2.17

然后检查你的glibc版本，保证glibc至少支持到2.17的版本：

```bash
$ strings /lib64/libc.so.6 | grep GLIBC_
GLIBC_2.2.5
...
...
GLIBC_2.30
GLIBC_PRIVATE
```

如果不满足的话，记得升级一下glibc，如何升级，请看[这篇博客](https://kirigaya.cn/blog/article?seq=102)。

因为下面要启动两个持久化服务，如果你没有后台运行软件，可以用用`screen`这个工具：
```bash
$ sudo apt install screen
```

---

## 安装签名服务器

在安装Mirai之前，一定要先安装好签名服务器，并运行，否则我们将无法正常进行登录。

### 下载项目 unidbg-fetch-qsign 的release

在`unidbg-fetch-qsign`项目的 [release](https://github.com/fuqiuluo/unidbg-fetch-qsign/releases) 中下载最新版本，比如我用的版本是这样的：

```bash
$ mkdir -p ~/Package/qsign && cd ~/Package/qsign
$ wget https://github.com/fuqiuluo/unidbg-fetch-qsign/releases/download/1.1.9/unidbg-fetch-qsign.3.zip
```

连续`unzip`两次得到文件夹，文件夹内部一共三个文件夹：`bin, lib, txlib`。进入`txlib`查看支持协议版本：

```bash
$ cd txlib
$ ls
3.5.1  3.5.2  8.9.63  8.9.68  8.9.71  8.9.73
```


### 选择协议号，修正配置文件

选择一个比较新的协议号，比如我选择了`8.9.63`，记住这个协议号，然后进入这个文件夹中，打开`config.json`，比如我的`config.json`如下：

```json
{
  "server": {
    "host": "0.0.0.0",
    "port": 8091
  },
  "key": "114514",
  "auto_register": true,
  "protocol": {
    "package_name": "com.tencent.mobileqq",
    "qua": "V1_AND_SQ_8.9.63_4194_YYB_D",
    "version": "8.9.63",
    "code": "4194"
  },
  "unidbg": {
    "dynarmic": false,
    "unicorn": true,
    "debug": false
  }
}
```

你需要确保如下的几个值的正确：
- `server.port`: 设置成一个不被占用的端口即可，比如我这边设置成8091
- `auto_register`: 设置成 true
- `protocol.qua`: 可以在[protocol-versions](https://github.com/RomiChan/protocol-versions)这个项目中根据你的设备类型和协议版本进行选择，我这边默认给的是对的，所以就不作修改了。
- `protocol.code`: 从`protocol.qua`中提取。
- `protocol.version`: 从`protocol.qua`中提取。


### 启动签名服务器

修改完后，重新进入`unidbg-fetch-qsign`的目录，根据你选择的协议号，我们开一个新的虚拟终端，并在后台运行签名服务器：

```bash
$ screen -S signServer
$ chmod +x bin/unidbg-fetch-qsign
$ bash bin/unidbg-fetch-qsign --basePath=txlib/8.9.63
```

运行成功的输出：
```
...
...
11:56:38.742 [main] DEBUG io.netty.buffer.ByteBufUtil - -Dio.netty.maxThreadLocalCharBufferSize: 16384
11:56:38.751 [DefaultDispatcher-worker-1] INFO ktor.application - Responding at http://0.0.0.0:8091
```

运行后，没有什么错误的话，`Ctrl + Alt + A` 退出虚拟终端。此时检查端口占用：

```bash
$ netstat -l | grep 8091
tcp6       0      0 [::]:8091               [::]:*                  LISTEN
```

可以看到，8091端口被占用，说明签名服务器已经开始运行。

---

## 安装 Mirai

部署完签名服务器后，就可以开始部署Mirai了。

### 下载 Mirai 项目

首先，先前往Mirai的 [release](https://github.com/iTXTech/mcl-installer/releases) 根据你的操作系统和指令集选择合适的安装包，比如大部分人的服务器估计都是x86的Linux，那么就可以这么选择：（下面的指令只是演示，请前往release下载最新版本哦！）

```bash
$ mkdir ~/Package/mirai && cd ~/Package/mirai
$ wget https://github.com/iTXTech/mcl-installer/releases/download/ae9f946/mcl-installer-ae9f946-linux-amd64
$ sudo chmod +x mcl-installer-ae9f946-linux-amd64
$ ./mcl-installer-ae9f946-linux-amd64
```

等待安装完成后，我们创建一个新的虚拟终端，初次运行文件夹下的`mcl`：
```
$ screen -S mirai
$ sudo chmod +x mcl
$ ./mcl
```

等待组件初始化后，你会看到一行绿字：`mirai-console started successfully`，说明mirai启动成功。我们会进入mirai-console控制台，我们先录入需要登录的账号：

```bash
> /login <qq号> <qq密码>
```
或者如果你嫌麻烦，可以录入一个自动登录的账号：

```bash
> /autoLogin add <qq号> <qq密码>
```

运行结束后，会遇到一些错误，不要担心，这些都是正常的，因为我们还没有给mirai指定签名服务器端口。

### 更新设备协议版本

先不要退出 Mirai 终端，还记得刚才的签名服务器的部署中，我们选择的协议版本号吗？比如我选择的是`8.9.63`，我们接下来需要让 Mirai 模拟设备的协议版本号和这个一致，在 Mirai 中，我们输入：

```bash
> protocol fetch ANDROID_PHONE 8.9.63
```

Mirai 会自动下载最新的协议参数到当前文件夹中。下载完成后，我们退出 Mirai：
```
> exit
```

### 配置 Mirai 签名服务

在尝试登录QQ后，Mirai 会出现错误码 45，因此，我们需要配置签名服务器。在目录下，你应该能够找到一个`KFCFactory.json`，这是 Mirai 用于配置签名服务器的文件。打开后，默认是这样的：

```json
{
    "0.0.0": {
        "base_url": "http://127.0.0.1:8080",
        "type": "fuqiuluo/unidbg-fetch-qsign",
        "key": "114514"
    },
    "0.1.0": {
        "base_url": "http://127.0.0.1:8888",
        "type": "kiliokuara/magic-signer-guide",
        "server_identity_key": "vivo50",
        "authorization_key": "kfc"
    },
    "8.8.88": {
        "base_url": "",
        "type": "TLV544Provider"
    }
}
```

这个文件的键值，比如`0.0.0`代表的是协议的版本号，比如默认配置文件的意思是，协议版本号为`0.0.0`的登录实例使用`http://127.0.0.1:8080`的签名服务，后面的参数是签名服务器的名字和对应的参数。

我们使用的是`unidbg-fetch-qsign`，所以使用第一个就行。将`0.0.0`修改成我们刚刚使用的协议号，比如我的是`8.9.63`，端口用的是8091，我的配置如下：

```json
{
    "8.9.63": {
        "base_url": "http://127.0.0.1:8091",
        "type": "fuqiuluo/unidbg-fetch-qsign",
        "key": "114514"
    },
    ...
    ...
}
```

修改完配置文件后，重新运行`mcl`，并且登录你需要的账号：

```
$ ./mcl
```

当你看到如下信息时，说明签名服务器配置成功了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-aa4bd6e18fc2bbfae66c5199292090c3_1440w.png" style="width: 80%;"/>
<p class="img-caption">很明显，作者喜欢《三体》</p>
</div>

### 手动抓包完成验证登录

在 Mirai 中，登录你的账号：

```bash
> /login <qq号> [qq密码]
```

> 第一次输入密码后，qq密码可以缺省了

忽略所有的 `alert` ，找到最后一个链接，复制。然后在浏览器中打开一个空白页面，按下 `F12` 打开开发者工具，找到 `网络/network` 这一项，并在`筛选器/Filter`中填入`cap_union_new_verify`：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2bd352cbc1ba0c8e79fd9a233a5e390d_1440w.png" style="width: 80%;"/>
</div>

把需要验证的链接放到当前空白页面中加载，并手动完成滑块验证，完成后，你会发现右侧出现了两个额外的请求，点击最后一个，选择`响应`，响应是一个很短的json数据包，找到里面的"ticket"键值包含的值，复制，粘贴到 Mirai 控制台中，然后回车，就能完成登录验证了。其中可能还会给这个QQ号绑定的手机发短信，记得接受然后输入就行。


## 登录 QQ 验证

在QQ中，给你的这个号发一些消息：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-894442092240dcf3e67b7bfa664fc845_1440w.png" style="width: 70%;"/>
</div>

控制台上应该会有完全一致的信息记录：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-238e41c32a62996d0cec7d997dff2dc1_1440w.png" style="width: 80%;"/>
</div>

这样就说明成功了，`Ctrl + Alt + A` 退出虚拟终端，后续就可以开始编写插件来让机器人自由回答问题了。

---

## 附录

重新进入签名服务器终端：

```bash
$ screen -r signServer
```

重新进入 Mirai 终端：

```bash
$ screen -r mirai
```