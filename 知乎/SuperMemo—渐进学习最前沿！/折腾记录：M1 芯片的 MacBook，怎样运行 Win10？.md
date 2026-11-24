# 折腾记录：M1 芯片的 MacBook，怎样运行 Win10？

> [原文链接](https://zhuanlan.zhihu.com/p/376462606)

> 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布
## 预备知识

zsh 命令行基础

## 我的方案

用 Parallels Desktop 虚拟机运行 Win10 on ARM。

## 详细流程

### Win10 on ARM

推荐使用 UUP 下载，如果是通过官网下载，很可能只能下到英文版，装中文语言支持会有问题（说多了都是泪）

UUP 是什么？可以参考这篇文章：[https://www.bilibili.com/read/cv4534273](https://link.zhihu.com/?target=https%3A//www.bilibili.com/read/cv4534273)

UUP 地址：[https://uupdump.net](https://link.zhihu.com/?target=https%3A//uupdump.net)

进入 UUP 后，选择 Latest Dev Channel build 的 arm64，其他版本可能装不上（说多了还是泪）

![](https://pic3.zhimg.com/v2-6a5bfbd7b52453eca3e15488e05640e8_1440w.png)
Image

然后选择语言 Chinese (Simplified)、版本 Windows 10 Pro。

最后按照默认的 Download and convert to ISO 就行了，点击 Create download package。

![](https://pic3.zhimg.com/v2-b41c4f2fd33589e1ac1502848add1da4_1440w.png)
Image

3 GB 多一点，比官网的预览版小多了。当然别高兴的太早，这里下载下来的是下载脚本：

![](https://pic3.zhimg.com/v2-8679efe2e2702a9e7c24f90b370d726a_1440w.png)
Image

运行 uup_download_macos.sh 就会开始自动下载了，前提是你安装了 homebrew 和 aria2，如果没有按照，脚本会提示，这个部分网上教程很多，我就不赘述了。

下载好后会得到一个 ISO 文件，接下来就可以用 Parallels Desktop 来创建虚拟机了。

### Paralles Desktop

Paralles Desktop 是 Mac 平台上的一款虚拟机软件，其官网地址是：[https://www.parallels.cn](https://link.zhihu.com/?target=https%3A//www.parallels.cn)

有了上面那个 ISO 文件，这边的配置就不必细说了，按照软件自身的引导教程来即可：

![](https://picx.zhimg.com/v2-9aeb7bb9ee1fcbfac2b039930927dd77_1440w.png)
Image

在此处把之前下好的 ISO 拖进来即可。

## 效果展示

### 资源管理器

![](https://pica.zhimg.com/v2-7c445e80cbaa4692ce68fcab42a86e72_1440w.png)
Image

### Galgame

![](https://picx.zhimg.com/v2-3d0821e724a42a8afd32fb66fadeffdb_1440w.png)
Image

### SuperMemo/Quicker

![](https://pic3.zhimg.com/v2-e5b2414c4784f2c7e36bc1c877d2e814_1440w.png)
Image

## 总结

以上就是我在 M1 芯片的 MacBook 上运行 Win10 的折腾经验了，希望对有需要的朋友有所帮助。

遇到什么问题，欢迎在评论区交流！
