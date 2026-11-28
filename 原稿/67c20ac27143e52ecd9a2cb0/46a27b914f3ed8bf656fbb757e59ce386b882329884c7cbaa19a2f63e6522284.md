## 前言

当我们在实现一个用于展示图片的块级元素时，往往会遇到一个问题，那就是图片尺寸规格不一导致的展示问题。比如我们写博客，往往喜欢用各种不同大小的图片作为封面插图，但是这么做往往会导致下面的问题：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-6f9318423e084fa527e3225680ba029d_1440w.jpeg" style="width: 50%;"/>
</div>

可以看到，因为封面图大小规格不一，封面展示的时候有的图片能把区域撑满，但是有的图片（比如图中《药理学要点》这篇文章的封面图）却撑不满，感觉很难看。那么有什么方法可以优雅地让封面图自适应地撑满区域吗？

## 实现方法 object-fit

这个需求是可以只通过 CSS 实现的！html 如下：

```html
<div class="display-image-wrapper">
    <img src="https://picx.zhimg.com/v2-070c52e2cbdbe3a59b136cf73f5a3386_r.jpg" :class="display-image" />
</div>
```

我们下面实现一个让 img 自适应撑满整个 div，img 的长与宽至少有一个会和父级元素 div 的长与宽相同，另外一个维度会超过父级元素被隐藏，并且保持居中对齐。

父级元素 div 的 css 如下：

```css
.display-image-wrapper {
    position: relative;
    overflow: hidden;
    height: 100%;
    width: 100%;
}
```

其中的 `height` 和 `width` 根据块级元素的渲染需求去自行控制，我这里 div 外层有其他元素控制 size，所以全部设置为 100% .

img 的 css 如下：

```css
.display-image {
    position: absolute;
    top: 50%;
    height: 100%;
    width: 100%;
    object-fit: cover;
    transform: translateY(-50%);
}
```

其中把 `height` 和 `width` 都设置为 100% 加上 `object-fit: cover` 保证了图片保持原本的形状，不变形地撑满整个区域； `top: 50%` 的绝对定位加上 `  transform: translateY(-50%)` 的位移保证了如果高溢出，它会居中对齐（宽默认就是居中对齐的）。

看一下最终的效果吧！

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-2a2c32d6f7e2f1cc0f17d758b13f7278_1440w.jpeg" style="width: 50%;"/>
</div>

marvelous！