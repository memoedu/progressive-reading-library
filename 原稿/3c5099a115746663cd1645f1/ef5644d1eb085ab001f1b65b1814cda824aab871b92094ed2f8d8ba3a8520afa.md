## 前言

最近打算把我的私人物理服务器做成 NAS，然后自然就需要有一个很优雅的文件服务器软件，此时，[filebrowser](https://github.com/filebrowser/filebrowser) 这个项目进入了我的眼帘。于是我简单把玩了片刻，发现确实是非常成熟的一个文件管理器，你完全可以把它当成一个私人云盘使用。

filebrowser 前端采用 vue，后端采用 go，是一个一体式的全栈项目，部署起来丝毫没有难度。

:::info

我的个人网站也是 vue + go 哦！

:::

---

## 安装

首先进入 filebrowser 的 release 部分，下载我们需要的版本，此处的服务器是 x86 芯片的 Ubuntu，自然就下载 [linux-amd64-filebrowser.tar.gz](https://github.com/filebrowser/filebrowser/releases/download/v2.30.0/linux-amd64-filebrowser.tar.gz) 了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-8eaf9afdc8e117364d236c025bd79944_1440w.png" style="width: 70%;"/>
</div>


此处我就直接在服务器上下载了：

```bash
mkdir -p package/filebrowser && cd package/filebrowser
wget -c https://github.com/filebrowser/filebrowser/releases/download/v2.30.0/linux-amd64-filebrowser.tar.gz
tar -xvf linux-amd64-filebrowser.tar.gz && rm *.tar.gz
```

完成后，你应该可以在你的服务器的当前文件夹上找到 `filebrowser` 这个可执行文件。

---

## 文件服务，启动！

然后直接启动文件服务即可，此处，因为我的服务器和我的 PC 在一个内网中，所以直接开在 0.0.0.0 的 8080 端口上了，端口请自行选取。然后我希望文件服务器的根目录在 `~/files/filebrowser` 上（否则默认就是在你的当前目录下，这里面包含着 `filebrowser` 可执行文件）. 于是，启动命令就变成了：

```bash
./filebrowser --address 0.0.0.0 --port 8080 --root ~/files/filebrowser
```

启动后，在 PC 的浏览器中输入 `http://<ip>:8080` 即可访问文件系统了。

:::info

初始情况下，账号和密码都为 `admin`

:::

如果希望挂载在后台工作，此处推荐使用 pm2 进程管理工具来放到后台。先在你的 服务器 上下载 nodejs，然后再全局安装 pm2：

```bash
npm config set registry https://registry.npmmirror.com
npm install pm2 -g
```

使用 `pm2` 启动上面的命令很简单：

```bash
pm2 start ./filebrowser --name filebrowser -- --address 0.0.0.0 --port 8080 --root ~/files/filebrowser
```

输出：

```
[PM2] Starting /home/kirigaya/package/filebrowser/filebrowser in fork_mode (1 instance)
[PM2] Done.
┌────┬────────────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id │ name           │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├────┼────────────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 0  │ filebrowser    │ default     │ N/A     │ fork    │ 12675    │ 0s     │ 0    │ online    │ 0%       │ 11.6mb   │ kirigaya │ disabled │
└────┴────────────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘
```

这样 filebrowser 就挂到后台运行了，你随时可以通过 `pm2 ls` 来查看所有的后台程序的状态；通过 `pm2 log filebrowser` 可以查看该进程的输出日志；并且，如果运行的程序发生了中断异常， `pm2` 自动会重启，上表中的 `↺` 展示了 pm2 为当年进程重启的次数。更多有关 pm2 的用法，可以参考：[汇尘轩 | 超级好用的进程管理器 pm2 使用教程](https://kirigaya.cn/blog/article?seq=233)。

---

## 修改账号密码

账号和密码都是 admin 太容易被黑了，所以启动完成后的第一件事情就是要先修改账号密码，点击左侧的 Settings，先把语言切换成 简体中文：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-68b04cd50a6b5298c3a017e858ca9740_1440w.png" style="width: 50%;"/>
</div>


然后点击 【用户管理】，点击第一个 admin 用户右侧的 修改符号：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-1e6d69545b354f1bc0a128a7d9bd2919_1440w.png" style="width: 50%;"/>
</div>


然后写入新的账号和密码，保存退出即可。

---

## 基本使用

filebrowser 的使用就和你使用百度网盘没啥区别了，点击右上角的【向上的箭头】可以上传文件或者文件夹，点击左侧可以新建文件夹。这些文件的真实地址都在 `~/files/filebrowser` 里面。比如我点击右上角的上传直接上传我珍藏多年的明日方舟图集，传输完成后效果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-0f39947bc5f9ea0d7bb496f7f7bb11ca_1440w.jpeg" style="width: 70%;"/>
</div>


基本的多媒体，比如图片，视频和音频，都是可以直接进行预览观看的：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-be75e5bb5f0d290eb69381ad5ff9ebd7_1440w.png" style="width: 70%;"/>
</div>


<div align=center>
<img src="https://picx.zhimg.com/80/v2-ce0a9b99f793409d6e1f56e2ade7ee11_1440w.png" style="width: 70%;"/>
</div>

总体而言，还是非常方便易用的一个文件服务器。赶紧用起来吧！无论是塔楼服务器，普通服务器还是树莓派，都可以使用 `filebrowser` 来快速构建你的文件服务器。

---

## 自定义标题和样式

现在的年轻人都喜欢个性化的服务，从而来彰显出这玩意儿是自己做出来的，而不是网上照搬照抄的。比如最基本的，把 filebrowser 上的图标呀，头像呀什么的换一下，会点 css 的，比如我，可能要修改一下元素的样式呀，加个圆角特效呀什么的。我们先随便进入一个文件夹，此处为了方便演示，我进入 `filebrowser` 的安装目录，创建一个名为 `branding` 的文件夹，然后在 `branding` 创建如下的文件结构：

```bash
branding
├── custom.css
└── img
    ├── icons
    │   ├── android-chrome-192x192.png
    │   ├── android-chrome-512x512.png
    │   ├── apple-touch-icon.png
    │   ├── browserconfig.xml
    │   ├── favicon-16x16.png
    │   ├── favicon-32x32.png
    │   ├── favicon.ico
    │   ├── mstile-150x150.png
    │   ├── safari-pinned-tab.svg
    │   └── site.webmanifest
    └── logo.svg
```



其中 `icons` 里面的文件你可以通过 [Real Favicon Generator](https://realfavicongenerator.net/) 这个网站来自动生成，你只需要准备一个长宽完全相同的位图（png 或者 jpg）即可。`logo.svg` 我是从生成的 `safari-pinned-tab.svg` 加了一个白色的矩形，从而搞了一个白色背景。svg 代码如下，`width` 和 `height` 改成你的 svg 的即可。

```xml
<rect width="1323pt" height="1323pt" fill="white" />
```

然后 `custom.css` 就是用来覆盖 `filebrowser` 原本样式的文件，此处我提供一下我写的：

```css
.credits>span {
    display: none;
}

#listing .item {
    border-radius: .8em !important;
    background: rgba(32, 41, 47, 0.7) !important;
    backdrop-filter: blur(5px);
}


body {
    background: url('https://picx.zhimg.com/80/v2-e54f847ba6bf5bff28bf0edefbada365_1440w.jpeg');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
}

nav {
    background-color: rgba(32, 41, 47, 0.8) !important;
    backdrop-filter: blur(5px);
    border-radius: 0 0 0.9em 0;
    padding-right: 20px;
}

.file-icons [data-dir=true] i {
}

.breadcrumbs {
    padding: 10px;
    border-radius: 0 0 .8em .8em;
    background: rgba(32, 41, 47, 0.8) !important;
    backdrop-filter: blur(5px);
}

header {
    border-bottom: unset;
    box-shadow: unset;
    background: rgba(32, 41, 47, 0.8);
    backdrop-filter: blur(5px);
}

#search.active {
    height: 100vh !important;
}

#search #result {
    background: rgba(32, 41, 47, 0.95) !important;
}

#search .boxes>div>div {
    border-radius: .5em;
    color: #20292f !important;
}

#search .boxes {
    border-radius: .6em;
}

#search .boxes i {
    color: #20292f !important;
}
```

想要更换自己的背景图片的，直接把 `https://picx.zhimg.com/80/v2-e54f847ba6bf5bff28bf0edefbada365_1440w.jpeg` 改成你的图像的链接即可。

然后进入 filebrowser，进入【设置】->【全局设置】，填写实例名称，这将成为你的文件服务器网站的标题，把刚刚创建的 branding 的绝对路径填入 【品牌信息文件路径】中，保存即可。 

刷新后，效果如下：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-4b85782b5489aac50fd9571b85042931_1440w.jpeg" style="width: 70%;"/>
</div>

:::info

在调试样式的时候，请打开 F12，找到【停用缓存】，然后勾一下，每当你修改了一个样式，但是前端没有生效时，记得多刷新几次。

:::

---

## 参考文档

- [filebrowser 官方文档](https://filebrowser.org/configuration/authentication-method)