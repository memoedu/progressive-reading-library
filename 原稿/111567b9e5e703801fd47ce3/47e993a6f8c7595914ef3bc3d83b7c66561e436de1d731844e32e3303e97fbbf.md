## 前言
在使用 vue 和 react 时，模块化带来了变量作用域的割裂，使得我们需要花费额外的手段来达到原本轻而易举的域内通信。

虽然这些框架都提供了一些通信函数，但是不好用，更不通用。

## mitt

mitt 是一个很好用的轻量级 JavaScript 通信框架，安装如下：

```bash
npm install mitt
```

使用如下：

```js
import mitt from 'mitt';

const emitter = mitt();

// listen to an event
emitter.on('foo', e => console.log('foo', e) );

// listen to all events
emitter.on('*', (type, e) => console.log(type, e) );

// fire an event
emitter.emit('foo', { a: 'b' });

// clearing all events
emitter.all.clear();

// working with handler references:
function onFoo() {}
emitter.on('foo', onFoo)   // listen
emitter.off('foo', onFoo)  // unlisten
```

其中， emitter 对象最好全局唯一。为了达到这个效果，不建议在组件中直接调用 `mitt`，而是在一个工具类文件中先初始化唯一的 emitter 对象：

```js
import mitt from 'mitt';

const emitter = mitt();
export {
    emitter
}
```

外部需要通信时，通信双方同时 `import` 这个工具类文件中的 `emitter` 对象，然后调用 `emit` （发送信息）和 `on` （监听并接受信息）即可。