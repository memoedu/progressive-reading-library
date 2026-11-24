# Anki 超强插件推荐

> [原文链接](https://zhuanlan.zhihu.com/p/49331137)

> 已收录于[「一周年」专栏整理](https://zhuanlan.zhihu.com/p/57637220)
## Enhance main window

[Enhance main window](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/877182321)

↑这个是插件主页

增强主页面！可以让你对每天的学习更加清晰！

效果图

![](https://pic3.zhimg.com/v2-7a849af784cc1e022ce2c33227376892_1440w.jpg)
我的配置：

{
    &#34;book symbol&#34;: &#34;{&#34;,
    &#34;cap value&#34;: null,
    &#34;color empty&#34;: &#34;green&#34;,
    &#34;color empty descendant&#34;: &#34;green&#34;,
    &#34;color marked&#34;: true,
    &#34;columns&#34;: [
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;red&#34;,
            &#34;description&#34;: &#34;Number of review you will see today (new, review and learning)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;today&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;green&#34;,
            &#34;description&#34;: &#34;Review cards cards you will see today (and the ones you will not see today)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;review&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: true,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;orange&#34;,
            &#34;description&#34;: &#34;Cards in learning (either new cards you see again, or cards which you have forgotten recently. Assuming those cards didn&#39;t graduate)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;learning card&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;orange&#34;,
            &#34;description&#34;: &#34;Review which will happen later. Either because a review happened recently, or because the card have many review left.&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;learning later&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;orange&#34;,
            &#34;description&#34;: &#34;Cards in learning which are due now. If there are no such cards, the time in minutes or seconds until another learning card is due&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;learning now&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;orange&#34;,
            &#34;description&#34;: &#34;Cards in learning which are due now (and in parenthesis, the number of reviews which are due later)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;learning all&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: true,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;green&#34;,
            &#34;description&#34;: &#34;Review cards which are due today (not counting the one in learning)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;review due&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;green&#34;,
            &#34;description&#34;: &#34;Review cards you will see today&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;review today&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;blue&#34;,
            &#34;description&#34;: &#34;Cards that have never been answered&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;unseen&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;blue&#34;,
            &#34;description&#34;: &#34;Unseen+ cards+ you will see today (what anki calls +new cards&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;new&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;blue&#34;,
            &#34;description&#34;: &#34;Unseen cards you will see today (and those you will not see today)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;unseen new&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: true,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;grey&#34;,
            &#34;description&#34;: &#34;number of buried cards, (cards you decided not to see today)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;buried&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;brown&#34;,
            &#34;description&#34;: &#34;number of suspended cards, (cards you will never see unless you unsuspend them in the browser)&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;suspended&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;black&#34;,
            &#34;description&#34;: &#34;Number of cards in the deck&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;cards&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;black&#34;,
            &#34;description&#34;: &#34;Number of cards/note in the deck&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;notes/cards&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: true,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;black&#34;,
            &#34;description&#34;: &#34;Number of cards/note in the deck&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;notes&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;purple&#34;,
            &#34;description&#34;: &#34;Number of cards reviewed, not yet due&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;undue&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;&#34;,
            &#34;description&#34;: &#34;Number of cards reviewed, with interval at least 3 weeks&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;mature&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;pink&#34;,
            &#34;description&#34;: &#34; Number of cards reviewed, with interval less than 3 weeks,&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;young&#34;,
            &#34;overlay&#34;: null,
            &#34;percent&#34;: false,
            &#34;present&#34;: false,
            &#34;subdeck&#34;: true
        },
        {
            &#34;absolute&#34;: true,
            &#34;color&#34;: &#34;purple&#34;,
            &#34;description&#34;: &#34;Number of marked note&#34;,
            &#34;header&#34;: null,
            &#34;name&#34;: &#34;marked&#34;,
            &#34;percent&#34;: false,
            &#34;present&#34;: true,
            &#34;subdeck&#34;: true
        }
    ],
    &#34;end symbol&#34;: &#34;;&#34;,
    &#34;ended marked background color&#34;: &#34;yellow&#34;,
    &#34;given up symbol&#34;: &#34;/&#34;,
    &#34;marked backgroud color&#34;: &#34;powderblue&#34;,
    &#34;option&#34;: true,
    &#34;pause symbol&#34;: &#34;=&#34;,
    &#34;refresh rate&#34;: 30
}## Speed Focus Mode auto-alert auto-reveal auto-fail

[Speed Focus Mode (auto-alert, auto-reveal, auto-fail)](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/1046608507)

↑这个是插件主页

![](https://pic2.zhimg.com/v2-e296908a46c43cbd8aa9913302e2b82b_1440w.jpg)
计时提醒，当你在一张卡片上停留时间超过你设置第一个的值，就会发出提醒声音，超过第二个值则会显示答案，超过第三个值则会自动帮你选 again 。不得不服，这个插件让我的反应速度重回 6s/card 。

## Image Occlusion Enhance Alpha for Anki 2.1

[Image Occlusion Enhanced Alpha for Anki 2.1](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/1374772155)

↑这个是插件主页

这个软件可以让用户在 Anki 中制作遮挡卡片，效果图如下

![](https://pic4.zhimg.com/v2-462c143c8807a87614d02f18491b97b1_1440w.jpg)
## Review Heatmap

[Review Heatmap - AnkiWeb](https://link.zhihu.com/?target=https%3A//ankiweb.net/shared/info/1771074083)

↑这个是插件主页

这个软件可以让用户在 Anki 中直观感受自己的学习记录，效果图如下

![](https://pica.zhimg.com/v2-10cb6daa29d82cb9fc6289bead76611e_1440w.jpg)

![](https://picx.zhimg.com/v2-074eb378189f2010db9cc221b01e5fa9_1440w.jpg)

![](https://pica.zhimg.com/v2-c375531c4f1e18fca945811a7ed9ab28_1440w.jpg)
(上面是本人从2017年4月22日至今的学习记录)

暂时推荐这三个插件。写教程是不可能写教程的，只有自己学习才能维持一下生活。。。

希望这三个插件可以帮到大家。

注：如果 Anki 这个软件对大家有帮助，希望大家可以自发的宣传一下，平时发发自己的统计截图也好。最近我要开始准备 ACM竞赛了，也不知道能不能顾得上专栏写作，我尽可能一周一更吧。
