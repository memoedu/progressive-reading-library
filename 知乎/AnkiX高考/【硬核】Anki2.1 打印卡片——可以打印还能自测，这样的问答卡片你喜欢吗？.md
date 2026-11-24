# 【硬核】Anki2.1 打印卡片——可以打印还能自测，这样的问答卡片你喜欢吗？

> [原文链接](https://zhuanlan.zhihu.com/p/137769105)

更新：感谢 circlestar 更新并汉化了插件

[circlestar：anki打印卡片插件（export deck to html）汉化+修bug](https://zhuanlan.zhihu.com/p/159263417)阅读须知：本文暂时只介绍问答卡片的打印方法（实际上是因为暂时不支持填空类型卡片）

效果：可以带图片、可以保留文本样式，保存为pdf后打印即可。并且问题和答案分为左右两列，方便对折自测。

![](https://pica.zhimg.com/v2-658ae60ecc11fdb813604bc0aaf3c124_1440w.jpg)

![](https://pic1.zhimg.com/v2-eb3d9a13826724f214db5c5508a7e01e_1440w.jpg)
工具：Anki插件Export deck to HTML、文本编辑器、浏览器

## 第一步：安装插件

插件页面

[Export deck to HTML](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/1897277426)插件怎么安装就不要问我了，百度一下你就知道。

## 第二步：筛选卡片

由于这个插件一次只能导出同一个笔记类型的卡片，所以如果你的牌组内混杂了不同类型的卡片，需要筛选出来移动到一个新的牌组中。

这里以 Anki 高考牌组为例

![](https://pic1.zhimg.com/v2-54adf96d12342159ad37447d4ebc75f0_1440w.jpg)
搜索方法：在对应的牌组后面加上 &#34;note:问答(Anki高考)&#34; 即可筛选出该牌组中所有的 问答(Anki高考) 卡片。

将选中的卡片更换牌组，这里以 分子与细胞 为例

![](https://pic2.zhimg.com/v2-b67af2d3c8ab0450d7730bf7f0f3629b_1440w.jpg)
## 第三步：设置导出格式

点击主页面-左上角-工具-Export deck to html

![](https://pic4.zhimg.com/v2-f1d9d5b449182eecdd6669fc9731158f_1440w.jpg)
选择对应的牌组

![](https://pica.zhimg.com/v2-70cb4ec7df77025a1455ce1f1ad27d72_1440w.jpg)
向 CSS 栏填入

table {
    font-family: verdana,arial,sans-serif;
    font-size:11px;
    color:#333333;
    border-width: 1px;
    border-color: #666666;
    border-collapse: collapse;
}
th {
    border-width: 1px;
    padding: 8px;
    border-style: solid;
    border-color: #666666;
    background-color: #dedede;
}
td {
    border-width: 1px;
    padding: 8px;
    border-style: solid;
    border-color: #666666;
    background-color: #ffffff;
}

img {
    max-width: 50%;
    height: auto;
}向 html 栏填入

{{问题}}
{{答案}}
然后 Save，可以保存导出样式便于多次使用。最后点击 Export，导出成一个 html 文件。

## 第四步：后处理

用文本编辑器打开 html 文件（系统默认用浏览器，这里要右键选择编辑）

![](https://pic4.zhimg.com/v2-d6369d25e46db1f63093604505f6b3eb_1440w.jpg)
然后找到文本中的  标签，在其下一行加入 

![](https://pic1.zhimg.com/v2-5f7d00f330c18165d318513a7c566a44_1440w.jpg)
然后找到  同理，在其上一行加入 

PS：嫌麻烦直接把body替换成table也可以

## 第五步：打印

这时候再用浏览器打开编辑好的 html 文件，右键后点击 Print

![](https://pica.zhimg.com/v2-cc71548c122506b902e01cf043960ba4_1440w.jpg)
这里可以保存为 pdf，如果你的电脑有连打印机也可以直接打印。

![](https://pica.zhimg.com/v2-2ad978f919c9f3174b559cb78c6f8584_1440w.jpg)
## 总结

本方法原理是利用插件，将卡片字段通过 html 和 css 重新组织为表格，然后导出后另存为 pdf 便于打印。根据不同的笔记模板，只需要制定对应的 html 和 css 即可。

## 可能遇上的问题

如果出现类似下面的错误，请看看你是否将 html 中的「问题」和「答案」改成你自己卡片模板对应的字段名称。

Anki 2.1.20 (47a1bf8b) Python 3.8.1 Qt 5.12.6 PyQt 5.12.3
Platform: Windows 10
Flags: frz=True ao=True sv=1
Add-ons, last update check: 2021-02-20 11:44:42

Caught exception:
Traceback (most recent call last):
  File &#34;lib\site-packages\anki\notes.py&#34;, line 144, in _fieldOrd
KeyError: &#39;问题&#39;

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File &#34;C:\Users\adminster\AppData\Roaming\Anki2\addons21\1897277426\ExportDeckToHtml.py&#34;, line 193, in _on_accept
    value = card.note()[field[2:-2]]
  File &#34;lib\site-packages\anki\notes.py&#34;, line 149, in __getitem__
  File &#34;lib\site-packages\anki\notes.py&#34;, line 146, in _fieldOrd
KeyError: &#39;问题&#39;## 补充

如果有卡片内含有 mathjax 格式的公式，请在第四步中继续添加以下代码至末尾：

MathJax.Hub.processSectionDelay = 0;
MathJax.Hub.Config({
messageStyle:&#34;none&#34;,
showProcessingMessages:false,
tex2jax:{
inlineMath: [[&#39;\\(&#39;,&#39;\\)&#39;]],
processEscapes:true
}
});

然后打开html后等待渲染即可，代码参考了《[ACRL：使用MathJax在AnkiDroid上显示数学公式，化学方程式](https://zhuanlan.zhihu.com/p/53776780)》的评论区，非常感谢。

效果：

![](https://picx.zhimg.com/v2-0d43b285e6c7e6056a8715d3a5040121_1440w.jpg)
## 再次补充

其实填空卡片也能打印，不过暂时无法做出填空的效果。篇幅所限，下次再说。

2020/5/12 更新：后续来了

[叶峻峣：【硬核】Anki2.1 打印卡片(续)——关于填空卡片转生成为复习提纲的那档事](https://zhuanlan.zhihu.com/p/138240220)## 再再再次补充（感谢 [@不忘初心](https://www.zhihu.com/people/a1fd216a0b1a8410d505c3ef3eb42b14) 提供沙雕版教程）

这里有位同学遇到了如下的问题

![](https://picx.zhimg.com/v2-fc448d80bada2c306ed47db8050d3511_1440w.jpg)
思考一下傻逼翻了什么罪

A. 没有认真阅读题干罪

B. 没有认真阅读材料罪

C. 没有认真阅读文章罪

无论你选哪一个，根据《关于对傻逼的处罚条例》，罪名都是成立的，我们来阅读一下总结部分（当你在阅读语文或者英语阅读时看末段也是很重要的帮你找出主旨的方法）

![](https://pic4.zhimg.com/v2-93904a1674ea2d6f4d518415e5771fcd_1440w.jpg)
不同的笔记模板，就要更改不同的html代码，以系统自带的“基础“”为例子，你制成的卡片是这样的

![](https://pica.zhimg.com/v2-4320deea88765e6b685b0fe6f120fff4_1440w.jpg)
那么你的html代码是这样的

![](https://pic1.zhimg.com/v2-94cc461e20fd1800696d69af0c43ef46_1440w.jpg)
你要根据每个子标题去更改双大括号里的内容，如果我表达的不太准确，大家也可以根据下面的这个例子来尝试举一反三一下

![](https://pic3.zhimg.com/v2-6ed5142d88445153f31ff73551e7b500_1440w.jpg)

![](https://pic1.zhimg.com/v2-e711e328d2bd6a912989e1fe9113cada_1440w.jpg)
