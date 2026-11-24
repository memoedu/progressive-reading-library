## 前言

这篇文章，简单讲讲如何使用 VIA 修改客制化键盘的按键映射。你需要：

1. 一把支持 VIA 的键盘。
2. 这把键盘的设计文件，通常是一个 json 文件。

:::info
我的 Alice 键盘的设计文件：[我的人体工学机械键盘映射文件](https://kirigaya.cn/blog/article?seq=138)
:::

---

## 安装软件与打开设计文件

先下载 VIA 软件，release 为 https://github.com/the-via/releases/releases/tag/v3.0.0

下载完成后，打开，先在【setting】中打开【Show Design tab】选项，此时，上面的菜单按钮会从三个按钮变成四个按钮，多出来一个【DESIGN】。

在 【DESIGN】 中点击 【Load】（根据情况，可能要需要点击下方的 【Use V2 definition】）：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-b8a113fb5ede84eed295e8db903926c4_1440w.png" style="width: 70%;"/>
</div>

此时，如上图，可以看到键盘的按键布局和默认的映射两组信息都加载到了画面中。点击 【CONFIGURE】板块即可开始改键。如下图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4c23ac68d2eb43c60b537995305638ad_1440w.png" style="width: 70%;"/>
</div>

---

## 基础改键

基本的按键修改很简单，比如你需要把 W 位置上的按键修改为 A，只需要点击上面键盘中的按键 W，然后再点击下方 【BASIC】 中的A即可。VIA 改键是实时的，修改完成就会立刻生效。

除了基本的字母，数字外，VIA还提供了一些系统事件，比如播放音乐，增加音量等等，如果你有多余的按键，可以把这些系统事件绑定到那些空余按键上。在 【MEDIA】 中就是和媒体相关的系统事件。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-fb644d4a015095917d388e94a4604657_1440w.png" style="width: 70%;"/>
</div>


:::info
需要注意，对于数字键，下图中的 红框 和 蓝框 的效果是不一样的！请根据你的操作系统和键盘来。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-23cbbfa26621d170714f4e87e5f08935_1440w.png" style="width: 70%;"/>
</div>
一般的设计都是用的红框里面的数字键。
:::

## 层与组合键绑定

大部分的客制化键盘考虑到成本和优雅程度，都会设计成比较小巧的配列，比如上面的几张图中就是我使用的键盘的配列，只有65个按键。你可以看到，它没有F区，那么要如何才能让这种小键盘打出F1~F12呢？对于编程开发，F区还是很常用的。甚至你打星际也需要F2A呢！

VIA 提供了层，基于层功能我们可以轻松绑定出组合键，从而在只有65个按键的键盘上打开远超65按键的按键映射。

我们首先需要选择一个按键作为组合键的入口，在我的键盘上，我选择了我的键盘中方向键左侧的 Fn 作为组合键入口。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-74f0659706c860e1c64826605fb46323_1440w.jpeg" style="width: 70%;"/>
</div>

我想要让 Fn + <数字> 的组合键被映射到 F<数字>。先解释一下层的概念，VIA提供的层相当于不同的映射方案组，默认的组为 0，也就是 VIA 软件【CONFIGURE】左上角显示的，可以看到，默认的 0 层的映射方案就是我们最常用的字母和数字键等等，0 层的按键是直接触发的，不需要入口按键。

对于 1，2，3 层的按键，我们需要为它指定一个入口按键。这里以 1 层为例子，我们先把 Fn 指定为 1 层的入口，点击上图中的 Fn，然后在下方的 LAYERS 选项卡中选择 MO(1)，代表这是 1 层的入口。

然后我们设置 1 层的按键映射方案，点击左上角的 LAYER 中的 1，可以看到，我已经设置了几个媒体按键。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-217f9537e704299607df418eb2e5b4fa_1440w.png" style="width: 70%;"/>
</div>

接下来，只需要把 `1` ~ `+` 的位置映射成 `F1` ~ `F12` 即可。这样就完成了！

<div align=center>
<img src="https://picx.zhimg.com/80/v2-b5cc777dca088fbec871ec0d7b13215d_1440w.png" style="width: 70%;"/>
</div>