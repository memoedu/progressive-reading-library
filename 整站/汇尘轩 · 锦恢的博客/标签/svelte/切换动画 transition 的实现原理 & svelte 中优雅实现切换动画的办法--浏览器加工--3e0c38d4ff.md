## 前言

最近在使用 svelte 构建一条数据管线的前端可视化平台，不得不说呀，svelte 给的工具真的非常齐全，虽然生态依托答辩，但是东西的质量都是远高于 vue 和 react 的。常看锦恢文章的朋友们应该知道，锦恢的理念是「优雅」，任何事情都得优雅且快速地完成，让所有人留下深刻的印象，这才是从前端开发中获得成就感的重要手段。

构建优雅的，具备高级感的网站时，元素之间的自由切换总是必备的，在 [vue3 路由动画爆炸的几个原因](https://zhuanlan.zhihu.com/p/476910604) 这篇古老的文章中，我讲过如何在 vue 中做到元素切换动画，这不过这个「元素」稍微有那么一点点大，是一整个 router-view，不过原理才是最重要的。

今天我们要尝试在 svelte 中利用 svelte 提供的强大的原生动画库来实现元素的切换动画。


## 什么是切换动画 transition

在开始之前，先给对前端技术不了解的朋友们科普一下什么是「切换动画」（transition），其实顾名思义，就是可以让元素A切换到元素B的动画就是切换动画。

实现它的方法很多，这里假设观众朋友们对于动画技术有一点点基础。大致原理就是，让A元素先退出（out），然后让B元素再进来（in），回退的时候恰恰相反，先让B元素退出（out），再让A元素进来（in）。也就是说，一前一后，一个完整的切换动画需要定义四个动画过程。

接下来就让我们看看如何在 svelte 中实现吧！本文的所有代码的完整版本都在文章结尾，你都可以直接在 playground 中直接运行哦！ https://svelte.dev/tutorial/svelte/in-and-out

## 实现的小麻烦

原理很简单，但是实际操作起来坑不少。假设你直接给如下的两个元素添加入场和退场动画：

文件名：stupid-transition.svelte (完整代码看文章结尾的附录)
```html
{#if show}
<span transition:fade>元素A</span>
{:else}
<span transition:fade>元素B</span>
{/if}
```

当 show 的数值发生改变的时候，元素A和元素B确实发生了切换，但是效果似乎有点奇怪？

<div align=center>
<img src="https://picx.zhimg.com/80/v2-95adbaaa66f588e34e6a95268600aa92_1440w.gif" style="max-width: 100%;"/>
</div>

为什么会这样呢？其实原因很简单，因为 show 这个 rune（svelte 的响应式变量都被称为 rune）发生变化时，元素A和元素B的动画是同时开始的，接下来的 500毫秒，A从有到无，B从无到有，但是这个 500ms，A 和 B 都同时存在于 DOM 上，此时画面中其实是有两个元素同时出现的，所以发生了挤压，出现了上图的情况。

## 不太那么优雅的解决方法：绝对布局

这个时候有小伙伴就说了：那么我们直接改为绝对布局不就行了？这样A和B的坐标计算就不再依赖兄弟元素的占位，而是完全依赖绝对坐标，这样就不会发生挤压了。不得不说，你很聪明，那我就勉为其难地试试吧。

文件名：absolute-transition.svelte
```html
{#if show}
<span transition:fade class="absolute">元素A</span>
{:else}
<span transition:fade class="absolute">元素B</span>
{/if}
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ce19582bb64ec2f7b9c898a66e510d96_1440w.gif" style="max-width: 100%;"/>
</div>

看来没问题，这方法看起来奇怪，但是你就说有没有完成吧！

不过我们仔细想想呀，其实也不太对，虽然视觉效果看起来勉强凑合，但是实际上元素A和元素B的退场和入场是同时开始的，讲道理嘛，应该让A先退场后，B再入场，从原理上才能说得通嘛！

而且如果是大型项目，你直接把元素变为绝对布局了，那么整个原本的布局也会发生很大的变化，有没有更加优雅的解决方案呢？有的兄弟有的。

强大的 svelte 动画库其实已经帮我们准备好了。

## 优雅的解决方法：svelte 元素的 in 和 out 宏

我们都知道 svelte 的后编译特性给予了元素更多的属性和宏。在 svelte 中，DOM 元素有两个用于精准控制动画的宏：in 和 out。顾名思义，in 控制元素退场的动画，out 控制元素退场的动画。

而且 svelte 提供的 transition 动画是可以设置参数的，所有动画都有两个公共参数：

- duration: 动画持续时间
- delay: 动画延迟时间

为了达到上面的效果，其实只需要让A退场后，B再进来就行了。也就是说，我们只需要让 B 的入场动画在 rune 响应后延迟 400ms 就行了（400ms 是 fade 的默认 duration），或者我们自己设置 **入场元素的 delay 等于 出场元素的 duration 即可**。

我们来试试吧：

文件名：elegant-transition.svelte

```html
{#if show}
<span in:fade={{ delay: 400 }} out:fade={{ duration: 400 }}>元素A</span>
{:else}
<span in:fade={{ delay: 400 }} out:fade={{ duration: 400 }}>元素B</span>
{/if}
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2e5f4e061f9dd2f602e5576d2458fa09_1440w.gif" style="max-width: 100%;"/>
</div>

wow! 优雅！这才是我想看到的效果，当时实际运作复杂元素的动画的时候，可以酌情把 delay 在 duration 的基础上再加个 50 - 100 ms，具体情况具体分析，你只要懂了基本原理，那么万变不离其宗了。

## 结语

通过本期教程，相信阁下应该学会给你的网站施加优雅动画了，从而让你更加有动力构建让观众印象深刻的艺术品了。

这里是锦恢，我们下次再见了，下次可能就是 AI Agent 的完整教程了，最近一直在开发它，有非常多的心得体会想要和你们分享。

---

## 附录

### 文件名：stupid-transition.svelte

```html
<script>
	import { fade } from 'svelte/transition';
	let show = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={show} />
	visible
</label>

{#if show}
<span transition:fade>元素A</span>
{:else}
<span transition:fade>元素B</span>
{/if}
```

### 文件名：absolute-transition.svelte

```html
<script>
	import { fade } from 'svelte/transition';
	let show = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={show} />
	visible
</label>

{#if show}
<span transition:fade class="absolute">元素A</span>
{:else}
<span transition:fade class="absolute">元素B</span>
{/if}

<style>
.absolute {
	position: absolute;
}
</style>
```

### 文件名：elegant-transition.svelte

```html
<script>
	import { fade } from 'svelte/transition';
	let show = $state(true);
</script>

<label>
	<input type="checkbox" bind:checked={show} />
	visible
</label>

{#if show}
<span in:fade={{ delay: 400 }} out:fade={{ duration: 400 }}>元素A</span>
{:else}
<span in:fade={{ delay: 400 }} out:fade={{ duration: 400 }}>元素B</span>
{/if}
```