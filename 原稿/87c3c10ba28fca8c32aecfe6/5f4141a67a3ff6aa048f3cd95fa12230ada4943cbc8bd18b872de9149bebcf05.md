相信在实验室从事深度学习研究的朋友们经常会遇到一个问题，那就是需要复现的文章或者公开的数据集，模型checkpoints是需要从 Google Driver 上下载的，然后因为谷歌云盘该死的 token 只能持续一个小时，所以在网速不好的情况下往往无法很顺利地下载 Google Driver 上的大文件，加之大家都是在 Linux 服务器上玩的实验，那为什么不直接在服务器上完成大文件的下载呢？

<div align=center>
<img src="https://pic3.zhimg.com/80/v2-9ba7d9f525d5b4ff888cf15d5c065ae2_1440w.webp" style="width: 50%;"/>
<p class="img-caption">而且这样本地的网络带宽就不会受到影响，你还可以边看论文边看动漫，岂不美哉？</p>
</div>

这篇文章就简单讲讲如何直接在你的服务器上下载 Google Driver 上的大文件。

---

## 如果你的服务器有 VPN 请跳过这一章

首先我们测试网络状态：
```bash
$ curl https://www.google.com -I
```

如果很快就返回了结果，而且第一行是这样的：
```
HTTP/1.1 200 Connection established
```

那么恭喜你，你的服务器能够通过合法信道访问国外资源。请直接移步下一章。

> 敲重点：通过非法信道访问国外资源是违法行为，诸位道友请遵纪守法。

如果不行，那么就需要自己搭建合法信道了，相信诸位研究生朋友应该都有自己的 VPN，那么你就应该对 Clash 订阅链接不陌生，拿好你的 Clash 订阅链接，然后参考[这篇文章](https://kirigaya.cn/blog/article?seq=101)。

---

## 两句话下载 Google Driver 大文件

先找到你需要下载的文件的ID：

<div align=center>
<img src="https://pic3.zhimg.com/v2-974f039b611b54ce72ceaae7265f6dee_r.jpg" style="width: 90%;"/>
</div>

打开服务器终端，进入你的数据集文件夹，然后直接输入如下两行命令带走：

```bash
$ export FILE_ID=<刚刚复制的文件ID>
$ wget -c --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=$FILE_ID' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=$FILE_ID" && rm -rf /tmp/cookies.txt
```
> /tmp/cookies.txt 这个文件不一定需要存在

<div align=center>
<img src="https://pic4.zhimg.com/80/v2-d22a2047e880ea132c18c7200f301987_1440w.webp" style="width: 90%;"/>
</div>

为了保险起见，你可以使用 screen 将任务放到后天运行，可以看[这篇文章](https://kirigaya.cn/blog/article?seq=105)

bingo！现在你可以关闭服务器，去健身房或者和邻座的博士一起打星际了！