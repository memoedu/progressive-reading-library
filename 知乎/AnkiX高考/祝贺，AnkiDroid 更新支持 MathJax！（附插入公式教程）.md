# 祝贺，AnkiDroid 更新支持 MathJax！（附插入公式教程）

> [原文链接](https://zhuanlan.zhihu.com/p/69189678)

目前 AnkiDroid 的最新版本是 v2.9alpha71

一看到 Anki 安卓版更新了 MathJax 的支持，急匆匆地开始了制卡测试！

还记得上次 [@Acceleratedddd](https://www.zhihu.com/people/8bb79b58ce5aed33e8a041a90bbc0667) 发的文章吗？为了在手机上显示化学方程式和数学公式，特地研究了 Anki 的笔记模板，制作了支持 mathjax 渲染的模板（不过得额外下载 mathjax 的包，大部分读者没有精力去折腾这个）。

原文在此：[使用MathJax在AnkiDroid上显示数学公式，化学方程式](https://zhuanlan.zhihu.com/p/53776780)，现在已经不需要这么复杂了！任何模板都可以支持 mathjax 公式，也就是 latex 公式。

废话不多说，上图：

![](https://pica.zhimg.com/v2-632da5d02da1fbff3dae50907e83cbc8_1440w.jpg)
这次更新，总算是统一了 Anki2.1 和 AnkiDroid 的大部分功能，也减少了同一张卡片在不同平台上显示的差异。我也可以在手机是独立完成每天的复习任务了。

至于如何快速输入公式，这里推荐两个办法（都是在电脑上输入，再同步到手机）：

## 方法一：天若OCR + 电子版材料

之前在这篇有介绍：[告别低效~看一眼这里，我帮您省时间！](https://zhuanlan.zhihu.com/p/51407096)

天若OCR下载地址：

[https://pan.baidu.com/s/17T1MR6R7EQ4zvgeokTMFeA#list/path=%2F](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/17T1MR6R7EQ4zvgeokTMFeA%23list/path%3D%252F)使用方法：

在电子版材料上找到你制卡时需要的公式（如上图）

![](https://pic4.zhimg.com/v2-f7f320aecd3d6e0a189bebe3536b740b_1440w.jpg)
2. 运行天若OCR，并右键点击其窗口，在接口中选择公式

![](https://pic2.zhimg.com/v2-f6563bd6d05654664e683c6a00f6c403_1440w.jpg)
3. 双击任务栏里的小图标，然后在电子版材料上截图

![](https://pic4.zhimg.com/v2-080b03197b168266f2ea64fa7b2845b5_1440w.png)
4. 识别成功（如果公式太长，可以逐项识别）：

![](https://pic1.zhimg.com/v2-03693ada51e6ab0e081db4078b608028_1440w.jpg)
5. 将$$内的代码套进\(\)，然后复制进 Anki 制卡框，就可以了！（Anki2.1和AnkiDroid2.9alpha71内置支持）

比如：

![](https://pic2.zhimg.com/v2-1e337cb7c1ad6b04d5182ae9bd772049_1440w.png)
效果：

![](https://pic3.zhimg.com/v2-6021dde42a48e8e8f3b734d8a8dcc1a0_1440w.png)
## 方法二：学习 latex 语法 + 输入法快捷短语

找一份 latex 教程，比如：

![](https://pica.zhimg.com/v2-9447a83aa84b1b0b7341a3b5fbebb8e2_1440w.jpg)
2. 直接到数学公式这一章：

![](https://pica.zhimg.com/v2-6885288b1d9cce26b608c78c647e3176_1440w.jpg)
3. 开始学（复）习（制）：

![](https://picx.zhimg.com/v2-cfae538dbf1c89fc2e73fee898aaa80d_1440w.jpg)
4. 把常用的公式加入输入法快捷短语（我现在改用 win10 自带的微软拼音，还不错）

![](https://pic3.zhimg.com/v2-c92f941460f1913936c69d8fc6e6886a_1440w.jpg)
5. 随心所欲地输入任何你想输入的公式吧！

注：AnkiDroid 最新版会更新在读者群里，至于群号嘛。。。相信你能在专栏置顶里找到！（latex教程群里也有哦）

Q：为啥用公式？贴图片不好吗？

A：图片较大，影响同步时间，而且夜间模式对图片支持较差，而对公式渲染支持较好，如下图：

![](https://pic1.zhimg.com/v2-8dbd0340cf27ac557d7635ff1010c660_1440w.jpg)
Anki2.1
![](https://pica.zhimg.com/v2-86f6c5e636dc08acfe98ab8f28419258_1440w.jpg)
AnkiDroid 这次我也算是给 AnkiDroid 贡献了一个 issue，帮忙找到了一个 bug

![](https://pic4.zhimg.com/v2-d91e8f74483575d46f411a3e073ecb83_1440w.jpg)
开发团队不到半天就解决了这个问题，并很快更新了
