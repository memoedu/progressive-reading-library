## 前言

这个CSS特效我其实很早就实现了，但是每次都懒得去记，要去老的项目里去翻找，想着不如今天就直接自己实现了吧！

我们一般在实现 `:hover` 的特效时，会直接改变元素的背景颜色，如下面的 iframe 左侧的按钮，读者可以把光标移动上去。

<center>
<iframe src="https://kirigaya.cn/files/html/offset-hover" width="100%"></iframe>
</center>

可以看到，common hover 当光标移动上去后会出现一个过渡动画来过度变化背景颜色。读者还可以再把光标移动到右侧的 offset hover 上，看看效果。

右侧的效果应该为出现一个从左到右的过度条，看起来非常精美。下面就直接记录这两种的实现方式吧。

:::info

第一种非常简单，第二种我只是懒得每次都记那几个 checkpoint 的百分比，所以才写了这篇文章。

:::

---

## 代码

右边动画的基本原理为基于 `background-position` 来定义一个 keyframes，然后把 `background` 定义成一个渐变颜色 `linear-gradient` 并把背景长度定义成 400%，这样就变成了一个四色渐变灯带，我们只需要移动这个灯带就能实现对应的效果了，代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div class="container">
        <div class="common-hover">
            common hover
        </div>
    
        <div class="offset-hover">
            offset hover
        </div>
    </div>
</body>
</html>

<style>
.container {
    display: flex;
    justify-content: space-evenly;
    width: 100%;
}

.container > div {
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    width: fit-content;
    font-size: 1.2rem;
    border-radius: .5em;
    background-color: gray;
    padding: 10px;
    margin: 30px;
    cursor: pointer;
    transition: .3s all linear;
}

.common-hover:hover {
    background-color: blueviolet;
    transition: .3s all ease-in-out;
}

.offset-hover:hover {
    background: linear-gradient(
                    90deg, 
                    blueviolet 25%,
                    rgba(200, 200, 200, 1) 37%,
                    rgba(255, 255, 255, 0) 63%
                );
    background-size: 400% 100%;
    animation: loading-mask 1.0s cubic-bezier(0.23,1,0.32,1);
    -webkit-animation: loading-mask 1.0s cubic-bezier(0.23,1,0.32,1);
    transition: .3s all ease-in-out;
}

@keyframes loading-mask {
    0% {
        background-position: 100% 50%;
    }
    100% {
        background-position: 0 50%;
    }
}
</style>
```

