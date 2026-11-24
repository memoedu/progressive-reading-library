# 【硬核】Anki2.1 打印卡片(续)——关于填空卡片转生成为复习提纲的那档事

> [原文链接](https://zhuanlan.zhihu.com/p/138240220)

更新：感谢 circlestar 更新并汉化了插件

[circlestar：anki打印卡片插件（export deck to html）汉化+修bug](https://zhuanlan.zhihu.com/p/159263417)前情概要：利用 Anki 插件 Export deck to HTML、文本编辑器和浏览器，我们实现了 Anki 问答卡片转换为可打印、可自测的表格

[叶峻峣：【硬核】Anki2.1 打印卡片——可以打印还能自测，这样的问答卡片你喜欢吗？](https://zhuanlan.zhihu.com/p/137769105)这回我们将同样结合使用以上三个工具，实现填空卡片到复习提纲的转化。

读者须知：本文默认您已经阅读了上一篇文章，知道如何安装插件和筛选卡片。

## 第一步：安装插件

略

## 第二步：筛选卡片

搜索方法：在对应的牌组后面加上 &#34;note:填空(Anki高考)&#34; 即可筛选出该牌组中所有的 填空(Anki高考) 卡片。

## 第三步：设置导出格式

点击主页面-左上角-工具-Export deck to html

![](https://pic4.zhimg.com/v2-f1d9d5b449182eecdd6669fc9731158f_1440w.jpg)
选择对应的牌组

![](https://pica.zhimg.com/v2-8b68f089867708a4fd56214550742412_1440w.jpg)
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

{{知识}}
然后 Save，可以保存导出样式便于多次使用。最后点击 Export，导出成一个 html 文件。

## 第四步：后处理

用文本编辑器打开 html 文件（系统默认用浏览器，这里要右键选择编辑）

![](https://pic4.zhimg.com/v2-d6369d25e46db1f63093604505f6b3eb_1440w.jpg)
然后找到文本中的  标签，在其下一行加入 

![](https://pic1.zhimg.com/v2-5f7d00f330c18165d318513a7c566a44_1440w.jpg)
然后找到  同理，在其上一行加入 

保存后打开 html 文件看看效果：

![](https://pic2.zhimg.com/v2-035e404106a93024dbab95bc21aa6d29_1440w.jpg)
这个挖空的代码有点碍眼，我们把它优化一下，先回到文本编辑器，用文本替换功能（记得打开正则表达式模式）

![](https://picx.zhimg.com/v2-f20ac1c6b234f89e3094a3c5cfca1195_1440w.jpg)
查找目标：{{c[0-9]::(.*?)}}

替换为：\1

保存后再打开 html 文件看看效果

![](https://pic3.zhimg.com/v2-9ca3e68da4a0637ffd6d082d51225e9e_1440w.jpg)
挖空代码变成了加粗样式，看起来好多了。

## 第五步：打印

略

## 总结

本方法原理是利用插件，将卡片字段通过 html 和 css 重新组织为表格，然后导出后另存为 pdf 便于打印。根据不同的笔记模板，只需要制定对应的 html 和 css 即可。

遗憾的是，没能做出 填空题|填空答案 这样的效果，不便于自测。不过要实现这个效果并不是不可能，只是光靠文本替换已经无法实现，需要写一些代码才行，有想法的同学可以自己尝试。

当然，能在 Anki 上记忆当然要比打印出来要好，本方法仅限于帮助无法在学校使用电子设备的同学。

## 题外话

我记得有一种自测的方法，叫**红膜自测。**原理是，如果填空内容是红色字体，题干是黑色字体，只需要用红色透光片盖住题目，就只能看见题干的文字，看不到填空内容，也能实现便于自测的效果。

这个方法是我以前在别的同学那边看来的，自己没有试过，不过感觉会对某些读者有用，就在这里提一下。

红色样式的代码： 

[@soneston](https://www.zhihu.com/people/9738c822b4f758fd6162b6f97622fa42) 同学补充：对橙色的遮挡效果更好，颜色码：#FFFFCD

附图（截去商标，免得有广告嫌疑）：

![](https://pica.zhimg.com/v2-dae2fa1d8e376899ad7f39591cd3aa32_1440w.jpg)
