从HF上下载模型时而会出现抽风情况，比如笔者最近打算用chatglm2做一个个人项目，下方是chatglm2-6b-int4的官方的model card上给的demo代码：

```python
from transformers import AutoTokenizer, AutoModel
tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm2-6b-int4", trust_remote_code=True)
model = AutoModel.from_pretrained("THUDM/chatglm2-6b-int4", trust_remote_code=True).cuda()
model = model.eval()
response, history = model.chat(tokenizer, "你好", history=[])
print(response)
response, history = model.chat(tokenizer, "晚上睡不着应该怎么办", history=history)
print(response)
```

下载时遇到网络错误：

<div align=center>
<img src="https://pic2.zhimg.com/v2-f88113ae65e6f96ce1a4eddc9786b7d1_r.jpg" style="width: 70%;"/>
<p class="img-caption">网络问题</p>
</div>


相信不少读者无论尝试多少次，就算关闭魔法也无济于事，有的倒霉蛋甚至尝试了重装了transformers（要不然你们就不会顺着搜索引擎找到这篇文章了），总之全部无法解决问题。所以这篇文章就简单讲一下如何手动下载模型使得你的实验或者项目能够顺利开展。

---

## 解决方法：from_pretrained的第一个参数

from_pretrained的第一个参数除了可以是远程HF仓库的以“用户名”+“仓库名”的名字外，还可以是一个本地的文件夹，读者完全不需要知道如何去编写这个文件夹，你只需要知道如何下载这个文件夹即可。

还是以chatglm2-6b-int4这个项目为例，去huggingface上先找到这个项目：

<div align=center>
<img src="https://pic1.zhimg.com/v2-58a5b4674cb10db46ee66c080a9f8688_r.jpg" style="width: 70%;"/>
<p class="img-caption">Files and versions</p>
</div>

上图中的Files and versions就是需要下载的文件夹。如何下载呢？

### 方法一：一个个点

打开Files and versions，你会看到如下的画面：

<div align=center>
<img src="https://pic4.zhimg.com/v2-c68b9b9ba83030afe0dd88106d77324f_r.jpg" style="width: 70%;"/>
<p class="img-caption">方法一</p>
</div>


看到上图的红框框中的下载按钮了吗？一个个点过去下载就完事了。记得把它们放在同一个文件夹下，这个文件夹我就起和仓库一样的名字了——chatglm2-6b-int4。

### 方法二：使用git

方法一有点笨，怎么优雅下载这个项目呢？

不知道读者对于HF这个网站和它的后端有多少了解，反正笔者在第一次使用HF时就发现这玩意儿的仓库是基于git进行远程管理的，所以使用git clone直接一部到位。

```bash
git clone https://huggingface.co/THUDM/chatglm2-6b-int4
```
> git clone的仓库链接就是项目主页的链接，仿照我的指令去clone你的

HF使用了LFS对比较大的文件进行了管理，如果你的开发环境没有git-lfs，那么你可能无法顺利克隆这些较大的文件，解决方法参考这篇帖子：

https://stackoverflow.com/questions/50923093/downloading-failed-from-github-using-git-lfs


这种情况就乖乖手动下载吧。反正LFS托管的文件也就一两个。

> 这都不高兴手动点？你退群吧。

---

## 设置本地模型仓库路径

上述方法无论使用哪一个，都会得到一个名字叫chatglm2-6b-int4的文件夹，然后将源代码中from_pretrained中的第一个参数改为这个文件夹的路径：

```python
from transformers import AutoTokenizer, AutoModel
import torch

tokenizer = AutoTokenizer.from_pretrained('./chatglm2-6b-int4', trust_remote_code=True)
model = AutoModel.from_pretrained('./chatglm2-6b-int4', trust_remote_code=True).cuda()

with torch.no_grad():
    model = model.eval()
    response, history = model.chat(tokenizer, "你是谁？", history=[])
    print(response)
```

运行一下：（因为目前chatglm2-6b-int4的量化功能貌似没有兼容Windows，运行时会出现警报，反正我也不用量化，忽视就行）

output:

```
我是一个名为 ChatGLM2-6B 的人工智能助手，是基于清华大学 KEG 实验室和智谱 AI 公司于 2023 年共同训练的语言模型开发的。我的任务是针对用户的问题和要求提供适当的答复和支持。
```

have fun！