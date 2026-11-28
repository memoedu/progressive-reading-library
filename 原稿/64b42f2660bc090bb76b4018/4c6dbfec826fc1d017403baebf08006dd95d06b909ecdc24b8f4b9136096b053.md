## bug: <input /> 可以被聚焦，但是不能被键入

今天遇到一个非常恶心的问题，起因是这样的：我准备发布最新版本的扩展了，但是发现里面一个功能的搜索框用不了，具体而言是，它可以被 focus 到，但是你无法往里面输入任何字符。

网上搜了一圈，全都是废话，大部分文章讲的都是无法 focus 的问题，这个很好解决，看一下图层就行了。我现在的问题是可以聚焦到，但是无法输入任何文本。

总结下来，一共三种情况会造成：

- input框设置了readonly属性
- css 的 `pointer-events: none`
- css 的 `user-select：none`
- js 监听了 keydown 或者其他按键事件，并设置了 `preventDefault`

## 问题解决

我的问题是最后一个，引发问题的代码如下：


```js
/**
 * 
 * @param {WaveElements} element 
 * @param {Pstate} pstate 
 * @param {Deso} deso 
 * @param {Record<EventHandlerKind, Handler>} eventHandler 
 * @param {KeyboardEvent} event 
 */
function windowsKeydown(element, pstate, deso, eventHandler, event) {
    if (event.ctrlKey) {
        if (event.key === '=') {
            eventHandler['scaleUp'].fn(pstate);
            deso.render();
        } else if (event.key === '-') {
            eventHandler['scaleDown'].fn(pstate);
            deso.render();
        }
    }

    if (event.shiftKey) {
        switch (event.key) {
        case 'ArrowLeft':
            eventHandler['scrollLeft'].fn(pstate);
            deso.render();
            break;
        case 'ArrowRight':
            eventHandler['scrollRight'].fn(pstate);
            deso.render();
            break;
        case 'ArrowUp':
            eventHandler['scrollUp'].fn(pstate);
            deso.render();
            break;
        case 'ArrowDown':
            eventHandler['scrollDown'].fn(pstate);
            deso.render();
            break;
        default:
            break;
        }
    }

    event.preventDefault();
}

```

为了省事，我粗暴地把 `event.preventDefault();` 直接放在了所有条件结束的最后，最好的做法是在每一个 `deso.render()` 后加 `event.preventDefault();`。需要自定义行为的分支再去 preventDefault，下次设计时不能再如此粗暴地一刀切了。