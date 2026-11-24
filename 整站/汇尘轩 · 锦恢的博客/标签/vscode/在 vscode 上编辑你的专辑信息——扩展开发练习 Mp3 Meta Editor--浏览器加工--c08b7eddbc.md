## 前言

一个非常简单的小项目，很适合 vscode 扩展开发的 webview 使用。事情是这样，我之前部署了一个本地的 NAS，顺便就整理整理本地的一些文件，结果翻到了曾经的专辑，大部分乐曲都是我从网上下载下来的，没有封面和专辑介绍，但是在我的一位挚友送我的 EVA 原声母带就有这些基本信息：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-2e15ee023701cecce0320bcba6194b7e_1440w.png" style="width: 70%;"/>
</div>

上面这个是 flac 格式的音乐，我手头的大部分是 mp3 格式的音乐，简单科普一下，mp3 不仅是一种音频压缩格式，而且还支持 标注 乐曲的标签，比如作者，专辑，曲子编号，封面图片等等，这些被我们称为 ID3 标签信息。所以我打算简单写一个用于编辑 ID3 信息的 vscode 扩展，来方便地编辑我的音乐的基本信息。于是花了两个小时后，解决了无数 bug 后，我开发出了这个简单的 vscode 扩展 Mp3 Meta Editor。

你可能会问我为啥不用 Windows 自带的工具，主要原因有两个：

1. Windows 自带工具不好用
2. 部分乐曲已经上传到 NAS 了，我需要一个可以在服务器进行远程修改的工具，我不喜欢 Xorg，我更喜欢 vscode + ssh，所以 服务器 图形化工具我会优先选择 vscode 扩展。

---

## 安装

在 vscode 的扩展商店中搜索 Mp3 Meta Editor 就可以搜到：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-91601c5f3496afe8d2539ce7917bbb4a_1440w.png" style="width: 70%;"/>
</div>

点击安装即可。

---

## 使用方法

我们先用 jijidown 随便下载一首曲子的 MP3 和封面图片：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d29b5e1779fe3561c94e90d556227b67_1440w.png" style="width: 50%;"/>
</div>

在下载的文件夹中打开 vscode，在文件区域或者mp3文件上右击，选择在当前文件夹打开 Mp3 Meta Editor，就可以打开了。

<div align='center'>
<iframe
height=400 width=80%
allowfullscreen=true
src=https://vdn6.vzuu.com/HD/4a63ff2e-4f07-11ef-8426-8a789b32b303-v8_f2_t1_Z7StwCs9.mp4?pkey=AAVKjK4eSDUPM4AE_ESJLBFYRvSEh6gXexwy-IA7xDZAVhp8IJgeIvh9TX5PAnxVYmzNHisE60huIT9spEXaw34e&bu=1513c7c2&c=avc.8.0&expiration=1722415852&f=mp4&pu=e59e796c&v=ks6&pp=ChMxNDAxNjIzODY1NzM5NTc5MzkyGGMiC2ZlZWRfY2hvaWNlMhMxMzY5MDA1NjA4NTk5OTA0MjU3PXu830Q%3D&pf=Web&pt=zhihu
></iframe>
</div>

一开始左侧是黑色的，代表没有封面图片，可以点击左下角的【修改封面】进行封面修改，此处选择【从本机选择图片】

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d61346e2231ce4d5201c7fe82a8be77b_1440w.png" style="width: 50%;"/>
</div>

然后选择刚刚下载的图片即可：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-0594968c726c20762e8a1e32ea18b001_1440w.png" style="width: 50%;"/>
</div>

编辑完成后，点击保存即可。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1a576fb3b984e70d43ee5241a5242df0_1440w.png" style="width: 50%;"/>
</div>

Then, enjoy ~

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9d871959fa1db1d845c27185e1f4be80_1440w.png" style="width: 50%;"/>
</div>

最后，还是推荐这首练声曲，真的很不错：[「Can you hear the cosmos in my throat？」练声曲【未抒＆永夜Minus】](https://www.bilibili.com/video/BV1Lw411V7Jm)

