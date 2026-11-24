
最近由于需要更换网站About部分的头图，所以简单入手了一下novelai，发现配合new bing会很好用，今天就简单讲一讲如何愉快地让赛博牛马帮你进行画作生成。

## step.0 魔法上网

这个就不说了，除了colab外，new bing本身也是需要魔法才能使用的。

## step.1 上线colab在线服务

为了使用novelai，网上有不少的教程，B站上大部分视频都要入群或者使用自建的服务，感觉很不靠谱。我之前记得有人做了colab的笔记，去知乎上翻了一下，果然翻到了，详细可以看[这篇文章](https://zhuanlan.zhihu.com/p/575320094)。简单讲就是，点开下面的链接

```
https://link.zhihu.com/?target=https%3A//colab.research.google.com/drive/1_Ma71L6uGbtt6UQyA3FjqW2lcZ5Bjck-
```

然后运行前面的0-3步。其中，0-2步为下载模型和推理程序，视colab服务器的繁忙程度，需要7-20分钟；第3步为启动4GB模型，当然，如果你需要更高质量的生成，可以运行第4步的脚本。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-eb9918ed054af2bf2e158d677cc8a2ae_1440w.png" style="width: 80%;"/>
<p class="img-caption">运行colab的server</p>
</div>

> 如果你的本地机器满足8GB以上的显存，你也可以把这个colab 笔记下载到本地的WSL2或者Linux系统中进行本地部署。

运行完成第3步后，当console内打印出来上图红框框中的信息时，点击红框框中的蓝色链接，就可以进入novelai的前端网页中：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-43dd0b08d171042f9df32c41f43d14f0_1440w.png" style="width: 80%;"/>
<p class="img-caption">NAIFU前端UI</p>
</div>

随便输入一个prompt来进行简单的测试：输入girl, best quality, purple hair。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-e88f4dba0e65d97b4aa8e6842d96dfbd_1440w.png" style="width: 80%;"/>
<p class="img-caption">purple hair girl</p>
</div>



## step.2 Novelai基础使用

novelai存在一些人体绘制上的错误，为了抑制这些错误，需要输入部分负标签：

```
nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
``` 

可以加入这里：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-fc914f8f4eb7546f1608f0df97483375_1440w.png" style="width: 60%;"/>
<p class="img-caption">Undesired Content</p>
</div>

软件本身的设置，包括生成图片数量，图像尺寸，就不再赘述了，请不要随意调整参数`Steps`。

---

## step 3. 询问new bing

虽然novelai提供了文字到图像的跨模态生成，但是prompts的选择对于生成图像的质量有很大的关系。

对于一个存在丰富上下文的人物，想要精准地用prompts来刻画TA，需要大量的精力，我们完全可以把prompts生成的任务交给new bing。但是请不要直接让new bing生成prompts，一定要先通过对话让new bing认识到你先要刻画的场景或者人物是什么。

比如我想要生成一个穿着汉服的星尘，那么需要先让new bing在多轮对话中加入“星尘是虚拟歌姬”的上下文：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-4b4aea12105589042777dab3ab71e125_1440w.png" style="width: 80%;"/>
<p class="img-caption">对话诱导</p>
</div>

然后就可以询问来获取prompts了：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d1441498b0d16d332c98ea5a160de7d7_1440w.png" style="width: 80%;"/>
<p class="img-caption">获取的prompts</p>
</div>

当然，生成的效果不一定满足你的要求，这时，就可以根据自己的要求进行微调或者去和new bing对线（虽然你不一定对得过）：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-566a312cb6e4b51a40a0e4a386413075_1440w.png" style="width: 80%;"/>
<p class="img-caption">和new bing对线</p>
</div>

最终也是生成了还算凑合的图，星尘厨看了不要打我：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2da2a882e304862417b18e0537c68802_1440w.png" style="width: 80%;"/>
<p class="img-caption">汉服星尘</p>
</div>


---

## 后话 加一个超分辨率重建

最终，我也是通过new bing + novalai + esr-gan获得了新的About Cover：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e43df8be54990caadc334b7fffdd3af3_1440w.png?source=d16d100b" style="width: 80%;"/>
<p class="img-caption">新的About Cover</p>
</div>


---

## 参考资料

- [一键搭建NovelAI云端版（使用colab免费服务）](https://zhuanlan.zhihu.com/p/575320094)
- [关于Novel AI 的prompt tag的一点理解（tag分享）](https://www.bilibili.com/read/cv19019708/)
