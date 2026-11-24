# 如何在 Ankidroid 上指定特定字段文字转语音(TTS)？

> [原文链接](https://zhuanlan.zhihu.com/p/93874295)

之前发过一个想法，里面的代码还可以用，这里补上详细的教程。

[https://www.zhihu.com/pin/1090633274971422720](https://www.zhihu.com/pin/1090633274971422720)

阅读须知：本文仅针对已经在Ankidroid上使用TTS的读者，满足指定特定字段文字转语音而非整张卡片转语音的需求。

## 首先是安卓端的教程

打开 Ankidroid 2.9 的用户手册

[AnkiDroid 2.9 User Manual](https://link.zhihu.com/?target=https%3A//docs.ankidroid.org/manual.html)Ctrl + F，搜索 TTS

![](https://pic2.zhimg.com/v2-6314ad6afad53538b26560d2823bf449_1440w.jpg)
找到对应的手册章节

![](https://pica.zhimg.com/v2-4cdd97ac465dbacfe8fd7cef9b79a2ac_1440w.jpg)
阅读手册，对照自己使用的模板，我用的是划词助手Antimoon模板

![](https://picx.zhimg.com/v2-fe62fb4173c255b3550a762d0031993d_1440w.jpg)
编写满足自己需求的代码，我想要在卡片正面播放我的英文例句，于是在正面将 {{例句}} 改写成

	
	{{例句}}
	然后就可以了。

## 然后是和电脑端兼容的教程

继续看文档，发现可以和 AwesomeTTS兼容

![](https://pic2.zhimg.com/v2-0ebe2a9ff61581fabe92653617e961e7_1440w.jpg)
我之前使用的是微软的TTS，所以仿照文档编写代码

	
	{{例句}}
	
然后就实现了在电脑上使用 AwesomeTTS，在手机上使用 系统/Google 的TTS服务，并且可以针对特定字段。

注意：AwesomeTTS 官方版本已经停止更新，所以我建议使用这个版本：

[AwesomeTTS (Google Cloud Text-to-Speech) [unofficial]](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/814349176)插件安装代码：814349176

## 拓展阅读

如果想要了解如何在安卓手机上使用 TTS，看这篇

[余时行：【工具篇】TTS 妈妈再也不用担心我的软件发音](https://zhuanlan.zhihu.com/p/26006690)如果想要了解如何在电脑上使用 Anki 插件 AwesomeTTS 来对特定字段文字转语音，可以看这篇

[易小猫：AwesomeTTS高级技巧之标签一劳永逸](https://zhuanlan.zhihu.com/p/24868747)
