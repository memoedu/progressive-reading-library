# 如何在 QQ 上快速检索自己创作的知乎内容？我写了个检索机器人。

> [原文链接](https://zhuanlan.zhihu.com/p/377726671)

> 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布
最近在群里问问题的朋友又多了不少，老是要我打开知乎找自己写过的文章怪麻烦的，故把自己硬盘里吃灰的机器人拿出来改改。

由于之前有写过爬虫、搜索引擎和 QQ 机器人的经验，这次花了不到两小时就把机器人上线了。

## 本项目涉及的 GitHub 项目

知乎文章备份：[https://github.com/L-M-Sherlock/zhihubackup](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/zhihubackup)

哈工深新闻搜索：[https://github.com/L-M-Sherlock/hitsz-news-search-engine](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/hitsz-news-search-engine)

招生问答机器人小哈：[https://github.com/L-M-Sherlock/HITszQAbot](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/HITszQAbot)

## 本项目涉及的一些工具

搜索引擎：elasticsearch

爬虫：没有用框架，代码大部分是 fork [njzjz](https://link.zhihu.com/?target=https%3A//github.com/njzjz) 大佬的

机器人：nonebot + go-cqhttp，和之前[迁移记录](https://zhuanlan.zhihu.com/p/363342119)里的一致

## 实现方案

通过爬虫，将自己所写的回答和文章下载到本地，将对应的 url、title 和 content 存入 elasticsearch，并使用 elasticsearch 的 ik 中文分词器，来优化索引和查询。最后使用 nonebot 调用 elasticsearch 的查询接口，通过 go-cqhttp 与 QQ 通信。

目前暂时没做全文索引，等我部署到服务器再考虑。理论上还可以将语雀、FAQ 等内容一起纳入搜索范围，实现在 QQ 群快速查找文章，帮群友答疑解惑。

## 效果展示

![](https://pic2.zhimg.com/v2-4b593153b2cd672a4daff87b95afd681_1440w.jpg)
Image
![](https://picx.zhimg.com/v2-2be544bacf650f24070960db4f2aed49_1440w.jpg)
Image## 结语

能将自己学过的技术用于帮助他人，真是在快乐不过的事了。虽然小哈那个项目最终迫于答辩压力终止了，但是它依然活在我心中。

另外，做自己想做的事，生产力是真滴高。
