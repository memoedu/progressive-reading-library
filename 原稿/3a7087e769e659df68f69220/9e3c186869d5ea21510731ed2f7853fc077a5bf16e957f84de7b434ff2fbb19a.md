## 前言

以 opengl 和 web 端的 webgl 为代表的光栅化引擎，是每个学习计算机图形学的小朋友都无法绕开的技术栈。在之前一段时间的 webgl2 实践后，我想要花点时间整理一下这个我曾经完全不了解的领域。

学习 webgl 和 GLSL（GL shader language，GL 着色器语言） 的道路是曲折陡峭的，请精心挑选一个心平气和的好天气学习。想要系统学习 webgl 或者 webgl2，全网最好的教程莫过于 webgl fundamental 系列了：

- webgl：https://webglfundamentals.org/webgl/lessons/zh_cn/webgl-fundamentals.html
- webgl2：https://webgl2fundamentals.org/webgl/lessons/zh_cn/webgl-fundamentals.html

需要澄清的是，webgl2 完全向前兼容 webgl，所以本系列只从 webgl2 开始讲解。通过在互联网上进行搜索，我发现现在简中互联网上还没有一个很好的，结合理论和 编程接口 讲解 webgl2 使用的文章，于是乎，我打算花点时间，写几篇系列教程，算是抛砖引玉。望后来者斧正。

---

## 认识计算机渲染的硬件模型

### 现代所谓的“电脑”——显示器→GPU→主板

在讲解 webgl2 渲染之前，非常非常非常有必要让我亲爱的读者有一个对于 **计算机是如何渲染图形** 的一个 **自上而下** 的认知。而在计算机的世界，但凡需要建立自上而下认知的地方，就必须要讲解计算机的硬件组成。假设，我们的软件现在想要把一个紫色的三角形渲染到屏幕上，硬件层面上，计算机是如何处理并渲染这段数据的呢？我们不妨首先看一下现代 计算机-显示器 的一个非常简单的组成结构：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-14cac675e76c8d582da7d36ca3189bdf_1440w.png" style="width: 70%;"/>
</div>


相信每一个装过电脑的朋友对上图应该都不会陌生。我们的电脑主机内，GPU和主板通过 PCIE 插槽直接相连，然后 GPU 向外暴露 HDMI 接口；然后 GPU 通过 HDMI 线和显示器连接在一起，从而组成了我们现在日常使用的现代计算机系统。

> 大部分学校的机房电脑比较老旧，用的可能是 VGA 连接主机（CPU 上的集成显卡）和显示器；一些场景下，人们还会使用 DP 连接 GPU 和显示器；大部分的现代 GPU 都会向外暴露好几个接口，有 HDMI 和 DP，本文为了讲解方便，只画了一条 HDMI。

而当我们想要在显示器上渲染一个三角形的时候，CPU 会通过图形驱动（OpenGL 或者 Direct X 或者 Vulkan 等）向 GPU 发送指令，如下图：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-1a22cf1faf244469d18c5dba895fbf46_1440w.png" style="width: 70%;"/>
</div>

当我们想要画一个三角形时，应用程序层一般都要我们传入三角形的三个点在某个软件层面抽象出的坐标系中的坐标。然后，应用程序层会把绘制三角形的任务交给图形驱动（Linux 是 OpenGL，Windows 上是 Direct X），图形驱动会把绘制给定三个坐标三角形的任务翻译成 GPU 能听得懂的 GPU 指令（这一步可以绕过操作系统）；GPU在接收到 GPU 指令后，会通过计算将这段指令翻译成显示器能听懂的信号，这段信号通过 HDMI 传递给显示器，显示器的内置驱动（硬件）会根据这段信号的指示，正确地、并行地点亮屏幕上的每一个像素点。

### 光栅化

对于上面的过程，一些对于显示器构造不太了解的同学就会问：为什么不能直接把这个点坐标直接渲染到屏幕上呢？这就涉及到光栅化这件事了，且听我娓娓道来。

我们的屏幕本质上是一堆排列成矩阵的“小灯泡”组成的，比如一般的 4k 显示器的分辨率是 3840×2160，你就可以认为我的显示器上有 3840×2160 个小灯泡，屏幕上的每一帧都是这些小灯泡按照固定的颜色排列后形成的样子。

> 对于液晶显示器（LCD），小灯泡就是液晶单元；对于有机发光二极管显示器（OLED），小灯泡就是发光二极管。

现在，我们要渲染紫色的三角形，那么三角形覆盖区域内的小灯泡就要亮紫色，其他区域的就要亮白色，而这些小灯泡按照这样的规则亮一次，就能得到我们要的最终的呈现效果，因此，我们一般认为的“渲染”（render），可以认为是计算出小灯泡亮的规则，并将这部分规则传递给显示器的内置驱动并生效的过程。而对于非矩形的图形，如何把它映射到我们的小灯泡阵列上不是一件容易的事情，将任意一个图形映射到矩形方阵上被我们称为图形的 **光栅化（Rasterization）**，如下图所示。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-52312c1d472d882b02d4cac0b42133dc_1440w.png" style="width: 70%;"/>
</div>

光栅化涉及到大量计算机图形学的知识，此处就不再赘述了，很多高中和本科打 ACM 的小朋友，做的一些计算机图形学相关的题目，其实就是光栅化驱动程序工程化实现的某个技术难点的抽象。



### 图形渲染的基础数据——图元

（请耐心看完本章，其中会涉及到后续 webgl 中的概念，彼时我不会再次说明）

拥有编程基础和编程语言设计经验都很清楚，工程化的一个很重要的技巧就是单位化，描述一个对象很复杂，但是抽象出 int，float，bool 这些基础数据，再用这些基础数据去构造更加复杂的数据结构就能描述复杂对象了。对于图形渲染，也是类似的。

在纷繁复杂的图形世界中，实现任意图形的光栅化程序是非常非常复杂的。因此，在图形渲染中，一个非常 nice 的想法就是抽象出一些非常基本的基础图形，然后实现这些基础图形的光栅化。当我们想要渲染更加复杂的图形时，我们就能用这些基础图形去组合出这些复杂图形。而这些基础图形，在计算机图形学中被称为图元（primitive），比如我们上图中的三角形，就是一种非常常用的图元。图元的数量一般是比较少的，比如 OpenGL 中提供的图元就这么9 种（在 webgl 中是 7 种）：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a412a9c4793f81cfb25d06bbe220c2b0_1440w.png" style="width: 50%;"/>
</div>

图元是图像渲染中的基础数据结构。当我们在操作 webgl 或者 opengl 想要渲染某个图形时，不能怀揣曾经的点线面的思维模式，在图形渲染中，基础绘图工具不是点和线，而是图元。

在计算机图形学中，三角形算是最常用的图元之一了，因为它在数学上有很多很好的性质，比如它是最简单的单纯形。也有不少图形学算法和它相关，比如 Delaunay三角剖分，可以将任意一个平面点集变成一堆三角形的拼凑的集合，这个算法在很多moba和RTS游戏（英雄联盟，星际争霸）的寻路算法中被广泛使用。这些都得益于三角图元的良好性质。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-5b41b546bbaefb4a28d32d736ac8ba7e_1440w.png" style="width: 70%;"/>
<p class="img-caption">暴雪娱乐 于 GDC2011 上对于星际争霸寻路算法的讲解</p>
</div>



### 顶点、片元、像素

从上图中，我们不难看出，定义一个图元往往只需要固定它的几个点，比如两点确定一个线段图元，三点确定一个三角形图元。这些用于确定图元的点，被我们称为 **顶点（vertex）**。在后续的 webgl 编程中，顶点的渲染数据是我们直接在 JavaScript 曾经定义的，而顶点是如何计算出在 webgl 的裁剪空间中的最终值则是由 **顶点着色器（vertex shader）** 定义的。

而在图像渲染中，我们还需要给图形上色，比如之前的三角形就是紫色的，甚至的有的还需要根据光线等诸多复杂因素，计算出一个和一大堆参数有关的颜色方程。而每个顶点和它的颜色纹理等相关信息统称为一个 **片元（fragment）**。而颜色和坐标如何对应就是 **片元着色器（fragment shader）** 要干的事情。

当一切准备就绪后， webgl + GPU 会将最终的渲染结果传递给显示器，而最终的渲染结果的基本组成单元就是我们熟知的 **像素（pixel）** 了。



### 常见的图形应用层软件和驱动层软件

好啦！最振奋人心的认知时刻到了，给我的读者几秒时间思考，我们日常使用的游戏制作也好，网页渲染也罢，哪些 软件/API/库 属于上图的应用程序，哪些属于 OpenGL/Direct X（驱动层） 这个层面的？

我简单给些例子：

- Web 上面的各种 DOM 操作， SVG 生成， canvas 图形绘制，都是应用程序层面的，它们被浏览器的图形驱动解释并渲染（Chrome 的渲染引擎是 Blink）。
- GUI 框架——比如 GTK+、Qt、Tk、FLTK、wxWidgets，也是应用程序层面的，用户在使用它们时，不需要操作驱动层。
- 虚幻引擎，Unity 3D都是驱动层的，它们都通过某一个转换层向 Direct X / OpenGL / Vulkan 甚至 GPU 进行通信。
- Python 著名的绘图库 Matplotlib，应用程序层面的，它的底层基于 GTK+ 和 Qt。

而本系列文章需要讲解的 WebGL2，自然是驱动层的组件，它的使用将有别于你曾经使用过的任何一种图形渲染库（比如Pixel.js，Three.js），使用 webgl 需要翻译应用层传递的各类坐标，然后驱动显卡去工作，因此，webgl 也被称为光栅化引擎，它只负责翻译各类坐标和颜色，并不负责各家顶层的操作。

当然，这个过程往往需要一定的浅薄的图形学的知识，所以有人对 webgl 进行了一定的封装，比如刚刚提到的 Pixel.js，Three.js。

根据之前的论述，大家应该知道了，使用 webgl 画图可以直接绕过浏览器的 Blink 渲染器，直接操作 GPU 去进行图形渲染。因此，对于有高性能要求的网络图形渲染而言， webgl 不可或缺。

---

## 环境搭建

令人高兴的是，每个人的电脑上都已经自带了 webgl 驱动库，因为每个人的电脑上都有浏览器，而 webgl 标准早就已经被各大浏览器厂商支持，这意味着，每个人的电脑的浏览器内核中都含有 webgl 的驱动。因此，在编程语言层面，你什么都不需要下载。

但是，我们需要一个得心应手的代码编辑器，虽然浏览器的 F12 控制台提供了一个很不错的代码编辑区，但是对于一个完整的 JavaScript 项目而言，不够完美。我推荐大家下载 vscode 作为这次的代码编辑器。

下载完成后，推荐下载几个插件。

### WebGL GLSL Editor

这个插件提供了 glsl 语言的语法高亮和自动补全，可以加快打 glsl 代码的速度和体验。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-76b9fc82a72f1cd99a9e096a02d4b91a_1440w.png" style="width: 70%;"/>
</div>

除了 LSP 服务，后续编写片元着色器时，插件还会显示颜色的渲染结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-f43383ae32a297bd6c4b8fef8c1a79eb_1440w.png" style="width: 60%;"/>
</div>



### HTML Preview

这个插件可以在 vscode 内直接以 server 的方式预览一个 HTML 文件，允许 HTML 通过 fetch 读取当前工作区的文件。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4870fd782eaf98fe8a5661bc86e60e30_1440w.png" style="width: 70%;"/>
</div>

效果，打开一个 html 文件，点击右上角的 “Open Preview to the Side” 就能在 侧板展示 html 的渲染结果。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-18a4bd23e8dedd581574ff786eb0c5cd_1440w.png" style="width: 70%;"/>
</div>

---

## 良好的编程习惯：JSDoc

最后，简单讲一个良好的编程习惯，因为我相信看这篇文章的应该都是计算机萌新，还没有养成 JavaScript 老油条的可恶的编程习惯。为了减少未来的业界毒瘤，我觉得我有义务普及一下 JSDoc。

如果你并不喜欢使用 TypeScript 进行 web编程，那么我强烈推荐你使用 JSDoc 进行 习惯良好的 JavaScript 编程。 vscode 默认自带 JavaScript 的 AST 解析器和配套的代码补全等 LSP 服务，而且对 JSDoc 支持得非常好。JSDoc 的类型计算基本上完全兼容 TypeScript。

现在请你打开你的 vscode，随便创建一个 js 文件，然后我们先创建一个用于生成着色器的函数：

```js
function makeShader(gl, type, source) {

}
```

此时，你在函数里写 `gl.` 是不会触发自动补全的，因为 vscode 不知道你这个 gl 是什么，为了让 vscode 知道 gl 是什么东西，我们给函数加上 JSDoc：

```js
/**
 * @param {WebGL2RenderingContext} gl 
 * @param {string} type 
 * @param {string} source
 */
function makeShader(gl, type, source) {

}
```

这样，vscode 就能正确地识别 gl 是一个 `canvas.getContext('webgl2')` 返回的对象了，你就可以使用自动补全了，这很关键，因为 webgl2 的 API 一般都很长。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e8a7cd76af9229375161280cc6f3bc87_1440w.png" style="width: 50%;"/>
</div>



现在，我们可以快乐地在主函数里面使用这个函数了：

```js
const vertexShader = makeShader(gl, 'VERTEX', vertexSource);
```

但是出现问题了：`makeShader` 的第二个参数实际上是固定的，只能是 `VERTEX_SHADER` 或者 `FRAGMENT_SHADER`，但是我们却写成了 `VERTEX`，而且这个单词太长了，我们在输入时很不方便。此时我们可以用 JSDoc 定义字面量数据。因为 type 这个参数只能是字符串 `VERTEX_SHADER` 或者 `FRAGMENT_SHADER` ，所以可以写为 `'VERTEX_SHADER' | 'FRAGMENT_SHADER'`：

```js
/**
 * @param {WebGL2RenderingContext} gl 
 * @param {'VERTEX_SHADER' | 'FRAGMENT_SHADER'} type 
 * @param {string} source
 */
function makeShader(gl, type, source) {
    
}
```

此时，在主函数中填写第二个参数时，就会出现自动补全：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-5c807f1c66199d1d07aea71fbedc3246_1440w.png" style="width: 50%;"/>
</div>

但是现在还有点美中不足，试想一下，当项目比较大时，你去看自己之前写的代码，发现 `makeShader` 返回的值 `vertexShader`，但是你却不知道这个玩意儿是什么数据类型，导致看自己之前的一段代码花了很长时间才看懂，从而延误了你和你的朋友在普罗旺斯发动大规模军事行动的计划，浪费了完美的假期，懊悔不已。为了避免这种悲剧，我们还需要标注返回值的类型，再加上 `@description` 用于描述函数的用途（这个可以不写，我基本不写注释），简直完美：

```js
/**
 * @description 创建着色器对象
 * @param {WebGL2RenderingContext} gl 
 * @param {'VERTEX_SHADER' | 'FRAGMENT_SHADER'} type 
 * @param {string} source
 * @return { WebGLShader | undefined }
 */
function makeShader(gl, type, source) {
    const shader = gl.createShader(gl[type]);
    gl.shaderSource(shader, source);
    gl.compileShader(shader);
    const ok = gl.getShaderParameter(shader, gl.COMPILE_STATUS)
    return ok ? shader : gl.deleteShader(shader);
}
```

此时，在主函数中，我们可以通过判断返回值为 undefined 结束函数的方式来 100% 地确定返回值是一个 `WebGLShader`，此时，vscode 的类型推断器也能做出正确的判断：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-bace9885d592467967db4d1ab52b46f7_1440w.png" style="width: 50%;"/>
</div>

可以看到，vscode的类型推断器在悬停提示上展示了正确的结果。

希望大家养成写 JSDoc 的好习惯，拒绝成为业界毒瘤，从我做起！我看有谁写项目用 JS 不写 JSDoc。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-4607d6f2215335de8ac4efd147ef7a7a_1440w.jpeg"/>
</div>



---

## 总结

这一章算是一个预热，简单介绍了计算机渲染图形的简略模型和计算机图形学的几个基本概念，并且介绍了后续进行编码的代码编辑器。下一章会开始讲解 webgl2 的基本使用。