# 【实战】Anki 生物专题快速制卡

> [原文链接](https://zhuanlan.zhihu.com/p/104370779)

最近发现了个不错的知乎专栏，整理了不少高中生物课本上的知识点。并且已经有同学做出了牌组。这里我研究了一个能够快速制卡的方法。

所需工具：Typora（或任意 markdown 编辑器）、nodepad++（或任意支持正则表达式替换的文本编辑器）、Anki。

资料来源：

[高中生物学教学](https://zhuanlan.zhihu.com/c_129069059)## 第一步 复制

随便找篇文章，比如《[Biology-quan：【教材精读】通过神经系统的调节](https://zhuanlan.zhihu.com/p/104202929)》

![](https://pic4.zhimg.com/v2-17d532cdb84e2703c82293e2ee589799_1440w.jpg)
复制到 Typora

![](https://pica.zhimg.com/v2-4fd56b99841299715a437611883f6f04_1440w.jpg)
复制其 markdown 代码到 notepad++

![](https://pic1.zhimg.com/v2-c38b83471aab1846fdb1ce41f1534f74_1440w.jpg)
## 第二步 文本预处理

做一些文本预处理

先把查找模式改成正则表达式：

![](https://pic1.zhimg.com/v2-af6d470c71b40ca314b9bd947f54c2d8_1440w.jpg)
这里分享几个小技巧：

*去掉空行*：

\r\n\r

![](https://pic4.zhimg.com/v2-31577669d0380430446dbffa2343a1fd_1440w.jpg)
或

![](https://pica.zhimg.com/v2-b8855c7ef5f02e5d6e16bc4276b185b0_1440w.jpg)
*去掉图片引用*：

\!\[img\]\(.*?\)

![](https://pica.zhimg.com/v2-61b76e225063725d5320b0b32f52e378_1440w.png)
然后一些小瑕疵手动改一改，就可以了。效果如下：

![](https://pica.zhimg.com/v2-ca30d830c4d1c94942e7d3b321c4eeec_1440w.jpg)
## 第三步 挖空

欸？挖空不是应该在 Anki 里操作吗？

不懂了吧[滑稽]，批量挖空才够爽！

以下代码可以把之前文本中加粗的部分全部转换成挖空代码

\*\*(.*?)\*\*

{{c1::\1}}

![](https://pic1.zhimg.com/v2-559f2bb778e72e158d6490ec16190282_1440w.jpg)
效果如下：

![](https://pic1.zhimg.com/v2-c2670860c68ddee70b076d782037e126_1440w.jpg)
## 第四步 导入 Anki

先将处理好的文本保存为 UTF-8 格式

![](https://pic3.zhimg.com/v2-ac0ee495930829b8a365915624ffed68_1440w.jpg)
保存

![](https://pic4.zhimg.com/v2-052ebdcfccb71c6c6bd79c5ff7729937_1440w.png)
导入：

![](https://picx.zhimg.com/v2-0b2044feb2f1738029501ec9198db1eb_1440w.jpg)
设置导入格式：

类型改为填空题（或 Cloze），字段由制表符分割

![](https://pic2.zhimg.com/v2-ea4e0c0879913a1d86579fdc1862c87b_1440w.jpg)
效果：

![](https://pic3.zhimg.com/v2-ce5c9b04dc3c0b893780b7b0d5dd4de6_1440w.jpg)

![](https://pica.zhimg.com/v2-fd4c875dc613df050a4abef1aa1a6156_1440w.jpg)
如果觉得一次性同时挖这么多空很难背，可以到卡片浏览器中修改，把清一色的 c1 改成 c1、c2、c3、……、cn：

![](https://pic3.zhimg.com/v2-ca6cd599fc733df5cfe5d7e0f570404c_1440w.jpg)
这样就是一句话对应七张卡片，每张卡片只挖一个空。比如：

![](https://picx.zhimg.com/v2-580bf615dfa2123dc97ca892ce663de5_1440w.jpg)

![](https://pic1.zhimg.com/v2-ba95c06cc6d4475e774ba722dc545d92_1440w.jpg)
具体如何挖空可以自己调整，这里就不细讲了。

感谢 [@Biology-quan](https://www.zhihu.com/people/45ecabfa7c60b15ba4938f4dd30c162e) 的材料。

为了避免侵权我就不把牌组分享出来了，相信大家看了这篇文章后，也有能力自己动手批量制作自己需要的卡片了吧！
