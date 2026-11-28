## 文本输入框
一个老问题了，在 HTML 中，我们往往通过如下代码生成并修饰一个文本输入框：

```html
<input type="text" placeholder="我是一个文本输入框"/>

<style>
input[type="text"] {
    /* CSS here */
}
</style>
```

效果如下：

<input type="text" placeholder="我是一个文本输入框"/>

这个文本输入框是已经通过我的网站渲染出的，沿用了我的私人 CSS 修饰。有关文本输入框的 CSS 的文章网站很少，或者说，大部分的表述都不准确，导致你根本搜不到，ChatGPT 和 MDN 也给不出正确的答案。

---


## 文本输入框的 CSS

### 普通模式下

```css
.tree-view-search[type="text"] {
    color: var(--input-foreground);
    background-color: var(--input-background);
    border-radius: var(--input-radius);
    border: .5px solid var(--input-border);
    font-size: 20px;
    padding: 5px;
    width: 90%;
}
```

### 光标 hover

被伪类 `:hover` 定义。

```css
.tree-view-search[type="text"]:hover {
    background-color: var(--input-hover);
}
```


### 被聚焦时（输入文本）

被伪类 `:focus` 定义。

```css
.tree-view-search[type="text"]:focus {
    color: var(--input-active-foreground);
    background-color: var(--input-active-background);
    border: .5px solid var(--input-active-border);
    outline: none;
}
```

> 这里有一个非常坑的点，默认情况下，文本输入框的 border 是 `outline-style: double` ，也就是有两层，而设置 `border` 只会修改内层，导致你根本看不出变化。所以当修改 border 相关的属性时，建议设置 `outline: none`。