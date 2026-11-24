# Orbit——让知识穿越时空，与你的读者相伴（内含部署教程）

> [原文链接](https://zhuanlan.zhihu.com/p/484118458)

## 简介

离上次介绍 LingQ[1] 过去两个多月了，这次想给大伙儿介绍一下，我们汉化组最近一直在翻译的「助记媒介[2]」实例——Orbit

先来个粗浅的定义：Orbit = 传统文章 + 复习卡片 + 间隔重复（如下图所示）

![](https://picx.zhimg.com/v2-6d5a507c9f433d00eec91f1903c44def_1440w.jpg)
可能这张图大家看得不是很懂，不过不要着急，在具体介绍助记媒介之前，我们先说说它是因何而生的。

## 为什么会有 Orbit？

为什么 Andy 创造了 Orbit？我们来做一个思想实验：

想象一下，你在阅读一篇专业课导论，里面有很多新颖的概念，读完后觉得自己学到了很多知识。但才过几天，当你想要回忆自己在那篇文章中所读内容时，却发现自己已经忘了大半。

这就是传统文章的缺陷：它只负责呈现内容，不管你是否能够记住。

再想一想，这篇传统文章来自于教科书，这里有部分习题可供你练习。你相信这些习题可以让你记住这些知识更长时间。但是又有更多的问题冒出来：你应该做哪些习题？什么时候做？这些习题想揭示什么深刻的见解？和文章中的哪个部分有关？

这是带习题的教科书的缺陷：理解和记忆的大多数工作，依然压在了你身上。

或许你该使用别人做好的 Anki/SuperMemo 牌组？只需要让这些间隔重复软件来帮我安排任务就行了？这确实是一个不错的想法，但学习别人的间隔重复记忆卡片通常是无效的[3]。

好吧，你可能觉得我在贩卖焦虑。说了这么多这不行那不行的，到底该怎办？

当然是解决这些问题，缺啥补啥：

既然传统文章缺少让读者记忆的能力，我们就往传统文章里塞复习卡片
既然卡片问题常常与文章脱节，我们就把卡片嵌到文章相关内容的附近
既然管理复习卡片的时间非常繁琐，我们就用一个间隔重复算法来安排

于是，就有了 Orbit：

![](https://pic2.zhimg.com/v2-3be45fe8c8a2b1c1e3df0a30ba7f9f43_1440w.jpg)
## Orbit 是什么？

Orbit 就是一个能够嵌入文章的复习卡片系统，并在合适的时机来给你发邮件，提醒你去复习：

![](https://pic4.zhimg.com/v2-52e35e7013e911cb065f77c933061017_1440w.jpg)
不过这样说还是太抽象了，推荐大家直接来我部署的 GitHub Pages 来体验一下：

[如何写出好卡片](https://link.zhihu.com/?target=https%3A//l-m-sherlock.github.io/thoughts-memo/post/how-to-write-good-prompts/)Orbit 这种设计，与我之前体验过的 Osmosis[4] 极为相似，都是为了克服理解到记忆之间的障碍而生的。

同时，它也是我之前的随笔所提到的间隔重复软件的两大进路[5]中的专家制卡一路。所以 Orbit 的主要使用者分为两类：利用助记媒介创作的作者，和阅读助记媒介文章并复习卡片的读者。Orbit 对读者是非常友好的，不需要像 Anki/SuperMemo 那样自己阅读、自己制卡、自己管理，只需要阅读现成的助记文本，复习作者提供的卡片，Orbit 会在合适的时机给读者发邮件，读者只需要点击复习地址即可开始复习。

我相信如果这种媒介能够得到更多创作者的使用，会对未来的教育产生巨大影响。所以接下来就让我介绍一下，我是如何部署 Orbit 的吧～

## 如何部署 Orbit

Orbit 的文档[6]非常简陋（因为还在内测开发阶段），所以我就简单说说 Andy 的原始用法：

  
    
  
  
    
      
      
      
    
  
直接调用 Andy 提供的 Web Component，然后在自己的网页里嵌入 orbit-reviewarea 即可。

![](https://pic3.zhimg.com/v2-e4477be57c7e51d85eecfc9d67664772_1440w.jpg)
Andy 你太残忍了，用你个 Orbit 还得建站，才能嵌入 Web Component 啊！怎么让只有基础开发知识的人用上？

我想到了很多建在 GitHub Pages 上的博客，部署起来还算简单，于是我选择了 Hugo[7] 来试试水。

### 安装 Hugo

安装官方教程一路搞到底就行了：

[Quick Start | Hugo (gohugo.io)](https://link.zhihu.com/?target=https%3A//gohugo.io/getting-started/quick-start/)### 部署 GitHub Pages

同样也是按官方教程，不过这里有个坑，如果你用了一些比较高级的博客模板，你需要把 GitHub action 脚本里的 *extended: true *给加上，不然会部署失败。

[Host on GitHub | Hugo (gohugo.io)](https://link.zhihu.com/?target=https%3A//gohugo.io/hosting-and-deployment/hosting-on-github/)### 嵌入 Orbit

Hugo 这类 blog 都是用 markdown 来写作，不需要直接编辑 html，是方便了不少，但是 Orbit 是 web component，肯定得在 markdown 里嵌入 html 的。这里我参考了下面这个方法：

[Simple Shortcode to Insert Raw HTML in Hugo · Ana Ulin](https://link.zhihu.com/?target=https%3A//anaulin.org/blog/hugo-raw-html-shortcode/)### 在 head 中插入 script

上面这个 script 嵌入到正文里也是没用的，得放到 Hugo 生成的 html 中的 head 里。Hugo 的原理其实就是把一堆 html 和 markdown 拼到一起去，所以找一找拼接的 html 有哪些，然后在目标位置插入上面这个脚本代码就行了：

![](https://picx.zhimg.com/v2-213c64e0bf83673c287633441ee12203_1440w.jpg)
### 太复杂了，能不能简单点？

确实，部署这玩意花了我一个周六的下午，踩了好几个坑。这里大家可以直接用我的最终成品：

[L-M-Sherlock/thoughts-memo: Thoughts Memo 小站 (github.com)](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/thoughts-memo)助记文章的编写方式可以直接参考我写好的内容：

![](https://pic4.zhimg.com/v2-6f4046371d04cbded27e733fadbb937f_1440w.jpg)
当然如果你还是嫌麻烦的话，可以直接把写好的助记媒介内容发给我（逃

之后应该也会陆续将一些传统文章翻译成助记媒介（什么叫知识翻译官啊 doge）

## 结尾

Orbit 还在成长中，我会继续跟进它的进展。想了解更多有关 Orbit 和助记媒介相关的内容，可以阅读我的专栏：

[助记媒介 & 思想工具](https://www.zhihu.com/column/c_1525784109008203776)
