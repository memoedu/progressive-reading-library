## 需求

在使用 web 技术编写一些大型软件时，我们希望可以自定义 ctrl + 鼠标滚轮 的页面行为，比如做一个可以左右移动的钢琴卷帘。默认下， ctrl + 鼠标滚轮会被浏览器默认进行缩放的操作。

---

## 解决方案

在综合了网上的答案后，我找到了一种可行的方法来实现，基本代码如下：

```js
document.addEventListener('wheel', event => {
    if ((event.wheelDelta && event.ctrlKey) || event.detail) {
        event.preventDefault();
    }
    if (event.ctrlKey) {
    // ctrl + 鼠标滚轮 的行为
    } else if (event.shiftKey) {
    // shift + 鼠标滚轮 的行为
    } 
}, { capture: false, passive: false });
```

> 把事件 `wheel` 替换为 `mousewheel` 也是可以的，但是这么做会导致 firefox 不兼容。 真正起到作用的是 `event.preventDefault()` 和 `{ capture: false, passive: false }`，这两个设置缺一不可。

- 其中 wheel 事件 event 的数据类型为 [WheelEvent](https://developer.mozilla.org/en-US/docs/Web/API/WheelEvent)，可以快速查到本身携带的数据。
- 如果替换行为你只是希望某个组件，把 document 修改为那个组件本身的 Node 实例即可。

---

## WheelEvent 数据

如果希望获得滚动量，可以使用 `event.deltaX` 和 `event.deltaY`：

- `event.deltaX`：鼠标的横向滚动量（罗技的 Master 系列鼠标有横向滚轮），正数就是向左滚动，负数就是向右滚动。
- `event.deltaY`：鼠标的纵向滚动量，正数就是向下滚动，负数就是向上滚动。



在 edge ， chrome 和 firefox 上，这几个参数的正负一致，但是具体的数值有点不一样：

|              | edge 上滚 | edge 下滚 | edge 左滚 | edge 右滚 |
| ------------ | --------- | --------- | --------- | --------- |
| event.deltaX | 0         | 0         | 150       | -150      |
| event.deltaY | -150      | 150       | 0         | 0         |

|              | chrome 上滚 | chrome 下滚 | chrome 左滚 | chrome 右滚 |
| ------------ | ----------- | ----------- | ----------- | ----------- |
| event.deltaX | 0           | 0           | 100         | -100        |
| event.deltaY | -100        | 100         | 0           | 0           |


|              | firefox 上滚 | firefox 下滚 | firefox 左滚 | firefox 右滚 |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| event.deltaX | 0            | 0            | 48           | -48          |
| event.deltaY | -126         | 126          | 0            | 0            |