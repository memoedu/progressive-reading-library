# Anki 复习数据清洗与分析(附源码和数据)

> [原文链接](https://zhuanlan.zhihu.com/p/337200754)

最近开始测试一些间隔重复算法，需要收集一些复习数据，如果大家感兴趣，可以按照本文的教程分析你的复习数据。

## 获取原始数据

### 数据库查看工具

Anki 的数据库用的是 SQLite，推荐使用 DB Browser for SQLite 来查看和导出复习数据，支持 Win 和 Mac。

下载地址：

[Downloads - DB Browser for SQLite](https://link.zhihu.com/?target=http%3A//www.sqlitebrowser.org/dl/)### 打开 Anki 数据库

下载并安装好上面这个工具之后，按照以下步骤即可打开 Anki 数据库（记得先把 Anki 关了）：

打开 DB Browser，文件 > 打开数据库

![](https://pic2.zhimg.com/v2-47141dfd16a70f8167a71458911ce305_1440w.jpg)
找到你 Anki 本地数据库的位置，通常这个位置是 C:\Users\你电脑的用户名\AppData\Roaming\Anki2\你 Anki 本地配置的名称\ 
记得把右下角这个改成所有文件，因为 Anki 的数据库后缀不是 db 也不是 sqlite。

![](https://pica.zhimg.com/v2-5efc00c8818bcbdd8cef43835c4d74b8_1440w.jpg)
改好后如果你找到的位置是对的，那么就能看到 collection.anki2 的文件了

2. 提取数据

打开后转到执行 SQL，输入以下 SQL 语句：

SELECT id, cid, ease, lastIvl, type
FROM revlog
WHERE type != 2
;然后点

![](https://pic4.zhimg.com/v2-d7ac423ef8c5db79e516eff96f0ec10f_1440w.jpg)
就能得到以下结果

![](https://pic3.zhimg.com/v2-fc747a7ae56a272c845f9054d801e5ba_1440w.jpg)
再点击

![](https://pic2.zhimg.com/v2-9443ee112755973af29f7e26f9482fb1_1440w.jpg)
就能将数据导出成 csv 格式，用 excel 打开就能看到这样的数据：

![](https://pic2.zhimg.com/v2-d8f2f2a856b19e7001312618101cf007_1440w.jpg)
里面没有关于卡片内容的敏感数据，大家可以安心分享。

欢迎将你的 .csv 文件发送到我的邮箱地址：jarrett.ye@outlook.com，

或者在专栏读者群(869469893)分享，也可以发送你的网盘分享链接。

PS: 当然，也可以导出带学习数据的 apkg 文件发送给我，如果你有调整复习参数，请详细说明。

作为回报，我会将以下分析数据反馈给你。（如果你有想要分析的其它指标，也可以跟我提，如果基于目前的数据可以分析，我也会反馈给你）

## Anki 数据分析

### 不同反馈对应的记录数量

![](https://pica.zhimg.com/v2-624b389de77179d3b302576f52bf7eee_1440w.jpg)
### 总体遗忘/保留情况

![](https://pica.zhimg.com/v2-2d4cfc6bc4cbe384a9bd2210fba09322_1440w.jpg)
### 不同间隔下的复习记录数量

![](https://pica.zhimg.com/v2-7b653027cde82cfe13d0b47255c00e74_1440w.jpg)
### 不同间隔区间对应的保留率

![](https://pic3.zhimg.com/v2-1f3013fe4b60bd1c2e182bd67e4db0f2_1440w.jpg)
### 不同复习次数对应的保留率

![](https://pic3.zhimg.com/v2-5c4e8b0428c91a332fb3af31f39351b6_1440w.jpg)
### 不同累计遗忘次数对应的记录数量

![](https://pic3.zhimg.com/v2-62048b3b6fbf33595ce8b8357111df0c_1440w.jpg)
还有一些其它的指标，如果你有任何建议或者意见，欢迎留言。

另外，以上代码和我自己的复习数据都开源在我的 GitHub 仓库：

[L-M-Sherlock / anki_revlog_analysis](https://link.zhihu.com/?target=https%3A//github.com/L-M-Sherlock/anki_revlog_analysis)如果想要自己分析可以直接下载源码运行。

感谢大家的支持！

2020 年 12 月 15 日

叶峻峣
