## 遇到的问题

最近在开发 vcd render，需要做一个让校准线随着光标移动的功能，自然而然地想到了使用 vue 自带的事件钩子 `@mousemove`，但是使用后发现，这个方法的速度好像很慢。


## 解决方法

使用 DOM 原生的 onmousemove 函数解决即可，比如可以这么写：

```javascript
document.onmousemove = event => {
    const x = event.x;
    const y = event.y;
    // your code here
}
```

---

## 原因

对于高频发生的 mousemove 事件，其实不建议使用 `@mousemove` 钩子，在 [这篇文章](https://geek-docs.com/vuejs/vue-js-questions/992_vuejs_how_do_i_get_the_mousemove_event_to_function_correctly_inside_a_vue_component.html) 中简单讲解了原因，解决方法也很简单，那就是使用 `@mousemove.stop` 代替。从而阻止其他组件也跟着冒泡。