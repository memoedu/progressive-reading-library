# 【工具分享】PDF、图片、公式——阻碍我们知识数字化的敌人终于倒下了，吗？

> [原文链接](https://zhuanlan.zhihu.com/p/16167738894)

## 引言

从第一次我用同济版高等数学的 PDF 制作 Anki 卡片的时候，我就发现了一件让我深深地感到无语的事情：

**为什么这些 PDF 复制出来都是一坨？**

> 与收敛数列的性质相比较，可得函数极限的 一些相应的性质. 它们都可以根 据函数极限的定义，运用类似于证明收敛数列性质的方法加以证明. 由于函数极 限 的 定 义 按 自 变 量 的 变 化 过 程 不 同 有 各 种 形 式 ，下 面 仅 以 “ I i m f (x )” 这 种 形 式 为
> 代表给出关于函数极限性质的一些定理，并就其中的几个给出证明. 至于其他形 式的极限的性质及其证明 ，只要相应地做一些修改即可得出.
本来教材内容就已经是一坨了，我当年给这些教材内容制卡的时候[1]，又要在文本处理上再吃一坨。

当我有耐心的时候，制出来的卡是这样的[2]：

![](https://pic2.zhimg.com/v2-70d8576d043a84992b6fe3c543ecd7ef_1440w.jpg)
没耐心的时候是这样的：

![](https://pic1.zhimg.com/v2-685279a5b2b52d5318878091f09fa4ac_1440w.jpg)
摆烂的时候：

![](https://pic3.zhimg.com/v2-e39bfd804ec9dd7730003d26834eac1a_1440w.jpg)
而且想要在这种教材里面搜索内容，也是玄学：

![](https://pic1.zhimg.com/v2-2f713ab3842890482cf5c2facf4e77c2_1440w.jpg)
复制一看：

> 的 某 一 去 心 部
这能搜到就有鬼了。

这种糟糕的经历伴随了我大学四年。期间我也尝试了不少其他解决方法。比如

用 Abbyy 来 OCR 建立文本层
上网找非扫描版的 PDF
用 Mathpix 做数学公式识别

但这些工具要么贵，要么各种卡壳。常常导致我学习一小时，有半小时都在折腾卡壳的地方，极大地降低了我的效率。

我一直在想，就没有什么工具能够完美解决这些问题，不再让我操心？

最近，Thoughts Memo 汉化组（养老院）的朋友告诉我了一个好消息：在 2025 年，这都不是问题了！

## Doc2X

我对它的第一眼就让我印象深刻：

![](https://pica.zhimg.com/v2-37fe46a8e10d4c5a4eff3e5bcb6c3676_1440w.jpg)
你说这是去印刷厂里把原始的 LaTeX 代码偷出来渲染的我都信。

再看看这个公式识别的效果：

![](https://pic1.zhimg.com/v2-a8af024b760447604fe0304197c4ec30_1440w.jpg)

![](https://www.zhihu.com/equation?tex=f%5Cleft%28+x%5Cright%29++%3D+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Clambda+x%7D%5Cleft%5Clbrack++%7B%7BP%7D_%7Bl%7D%5Ccos+%7B%5Comega+x%7D+%2B+%7BQ%7D_%7Bn%7D%5Csin+%7B%5Comega+x%7D%7D%5Cright%5Crbrack+%5C%5C+%3D+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Clambda+x%7D%5Cleft%5Clbrack++%7B%7BP%7D_%7Bl%7D%5Cfrac%7B%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Comega+%5Cmathrm%7B%7Bxi%7D%7D%7D+%2B+%7B%5Cmathrm%7Be%7D%7D%5E%7B-%5Comega+%5Cmathrm%7B%7Bxi%7D%7D%7D%7D%7B2%7D+%2B+%7BQ%7D_%7Bn%7D%5Cfrac%7B%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Comega+%5Cmathrm%7B%7Bxi%7D%7D%7D+-+%7B%5Cmathrm%7Be%7D%7D%5E%7B-%5Comega+%5Cmathrm%7B%7Bxi%7D%7D%7D%7D%7B2%5Cmathrm%7Bi%7D%7D%7D%5Cright%5Crbrack+%5C%5C+%3D+%5Cleft%28+%7B%5Cfrac%7B%7BP%7D_%7Bl%7D%7D%7B2%7D+%2B+%5Cfrac%7B%7BQ%7D_%7Bn%7D%7D%7B2%5Cmathrm%7Bi%7D%7D%7D%5Cright%29+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Cleft%28+%7B%5Clambda++%2B+%5Comega+%5Cmathrm%7Bi%7D%7D%5Cright%29+x%7D+%2B+%5Cleft%28+%7B%5Cfrac%7B%7BP%7D_%7Bl%7D%7D%7B2%7D+-+%5Cfrac%7B%7BQ%7D_%7Bn%7D%7D%7B2%5Cmathrm%7Bi%7D%7D%7D%5Cright%29+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Cleft%28+%7B%5Clambda++-+%5Comega+%5Cmathrm%7Bi%7D%7D%5Cright%29+x%7D+%5C%5C++%3D+P%5Cleft%28+x%5Cright%29+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Cleft%28+%7B%5Clambda++%2B+%5Comega+%5Cmathrm%7Bi%7D%7D%5Cright%29+x%7D+%2B+%5Cbar%7BP%7D%5Cleft%28+x%5Cright%29+%7B%5Cmathrm%7Be%7D%7D%5E%7B%5Cleft%28+%7B%5Clambda++-+%5Comega+%5Cmathrm%7Bi%7D%7D%5Cright%29+x%7D%2C)
 

我甚至觉得它眼神比我好。

再试试我自己的草稿纸（我自己都看不懂了现在），意外地还行：

![](https://picx.zhimg.com/v2-9e9ea591b5bc84bdaed0ca76cce30c23_1440w.jpg)
而且 Doc2X 还接入了 DeepSeek-v3，哪里不会点哪里，省得我再去复制到其他 LLM 那里了：

![](https://pic3.zhimg.com/v2-b0abec1a953a2b817d7e99e1325ba4b2_1440w.jpg)
当然，有人可能会问，你不是早毕业了，还在看同济版高数？没有其他活了吗？

有的，而且还是个好活：

![](https://pic4.zhimg.com/v2-1ce41be7a38f6e04407768e9c4e03639_1440w.jpg)
做算法难免要经常看学术论文，而现在论文实在太多了，哪怕是挑着看，也要花不少时间。Doc2X 这个对照翻译直接让我看得飞快。

除了看论文，我业余还会做做教育研究，看点教育相关的书，回答教育问题。怎么高效地查找自己看过的内容，也让我头疼了很久。我之前会用 Calibre 管理电子书，但是搜索的时候有可能会记错关键词，导致老是找不到我看过的片段。Doc2X 的这个 AI 回答引用就相当于一个高级的模糊搜索，不仅能够根据原文内容回答，还可以帮我定位到对应的页面，省去不少检索资料的时间：

![](https://pic1.zhimg.com/v2-1ac5a05b8a51356e308d4629fe0f19ce_1440w.jpg)
## 总结

不得不感慨技术升级是如此之快，六年前我还在练习打字速度，给 LaTeX 公式设置各种快捷输入[3]，搞各种奇技淫巧，现在我只需要动动手指，把要处理的文件拖到 Doc2X 就完事了。

但是，工具在变得越来越好用的同时，使用工具和不用工具的人之间的差距也变得越来越大。是否使用间隔重复，就能拉开 20% 以上的差距[4]。各种 AI 导师也层出不穷。可是，现在已经 2025 年了，真正知道这些工具，并在日常学习中使用它们的人，又有多少呢？

也许，这并不是一个教育学的问题。

之后我会发布一些相关译文，讨论这个问题。

敬请期待。

恰个饭：

[Doc2X](https://link.zhihu.com/?target=https%3A//doc2x.noedgeai.com/%3FinviteCode%3DSYLTI8)通过这个链接，或使用邀请码（SYLTI8）注册，可以增加我的会员时长和页面解析额度～
