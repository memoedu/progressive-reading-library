<center>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1975516719&auto=1&height=66"></iframe>
</center>

## 前言

有的时候，我们会在前端 App 的初始化阶段进行大量计算，比如你在编写一个某种特殊格式的渲染器，这个渲染器需要在开头占用大量的V8的调度，从而阻塞了主线程的渲染，导致在用户开来，软件启动后在开头卡顿了一下。这样不仅体验非常差，而且如果用户手贱给尚未渲染完成的前端输入了什么事件，比如 click，move，可能会触发浏览器的保护机制，给你弹出一个未响应。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7007550ac5816a3e0fff09d9c1a37967_1440w.png" style="width: 80%;"/>
</div>

有的观众就会说，那么我们给处于初始化的App挂载一个mask遮罩做loading不就行了？代码如下

> 此处拿大家较为熟悉的 vue3 为例子

```html
<!-- App.vue -->
<script setup>
import { onMounted } from 'vue';

import { ElLoading } from 'element-plus';
// startupRender 是一个很费时的计算密集型函数
import { startupRender } from '@/hook/render';

onMounted(async () => {
    const loading = new ElLoading.service({ ... });
    const render = await startupRender({ ... });
    loading.close();
});
</script>
```

这么做也是没用的，因为当代码运行到 `await startupRender` 后，该函数作为宏任务会占用大量的V8的调度，导致原本的 loading 对象（其实是驱动一个 CSS 的animation）无法被分配到更多的调度进行加载动画渲染，导致用户看到的是一个静止不动的遮罩层。

> 下图中的“加载中”在 await startupRender 运行期间是静止不动的。

<div align=center>
<img src="https://pica.zhimg.com/80/v2-8e364bca535cc954d730d311e6b35b36_1440w.png" style="width: 80%;"/>
</div>

这样做的体验仍然很差，那有没有什么方法可以改变这一切呢？

根据我们上面的分析，这一切的罪魁祸首是 JavaScript 本身的单线程调度引起的，那么如果可以做到多线程运行费时函数，把主线程渲染DOM处理用户事件和计算密集型函数的调度进行隔离，那不就行了？问题简单了——如何在 V8 中做到多线程调度？

答：web worker

---

## Web Worker 基本使用

[MDN Web Worker API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API)

web worker 允许 JavaScript 的主线程开启新的线程用于开启多线程任务。



### 启动一个 worker

基本使用如下

```javascript
// 开启 worker
const worker = new Worker('worker.js');

// 终止 worker
worker.terminate();
```

其中 `Worker` 构造参数为字符串，可以是 worker 执行脚本文件相对于web根目录的相对路径，比如上面写的是 `worker.js`，那么 V8 在运行到这里后会向着 `http://localhost:8000/worker.js` 发起网络请求，将得到的文件以字符串的形式解析成 JavaScript 代码并在隔离的新的线程中运行。

当然，如果只能指定 web 已经存在的资源，那么这个做法未免有点太局限了，我们可以通过 `createObjectURL` 方法将一主线程内可以访问到的、可以构造出的 js 字符串转换成一个 blob url，来提供给 Worker 作为初始化参数：

```javascript
const blob = new Blob(['console.log("hello world from worker");'])
const url = URL.createObjectURL(blob);
const worker = new Worker(url);
```

运行结果（web 中）：

```
hello world from worker
```



### worker 的上下文

不同于主线程，worker 线程内很多上下文是用不了的，比如 `window`，`navigation`，这些在 worker 中都是 `undefined`。所以如果你想要让 worker 可以访问主线程的某个变量或者某个函数，通过全局变量 `window` 做中转是行不通的。

在主线程中，所有全局变量都自动挂载在 `window` 上，而在 worker 中所有全局变量都挂载在 `self` 中。比如，你可以在 worker 中直接调用 `importScripts`，这个做法和 `self.importScripts` 完全一致。



### worker 线程和主线程的数据通信

对于多线程调度而言，如何进行数据通信是非常重要的，多线程通信比多进程要简单多，省事多了，毕竟都在一个 PCB 块中。 worker 线程运行时的 `self` 和 主线程的 worker 对象都有两个完全一致的方法，可以用于发送和接受数据：

- `postMessage`：发送数据
- `addEventListener('message', event => {})` 或者 `onmessage = event => {}`：接受数据 

> 这个和 vscode 内 webview 的通讯方法完全一致。你可以认为 vscode 内的 webview 都是运行在单独的 worker 中的。

下面是一个数据通信的简单例子：

```js
// 主线程
const worker = new Worker('worker.js');

worker.postMessage({x: 1, y: 2});

worker.addEventListener('message', event => {
	const data = event.data;
    console.log(data.result);
});

// worker.js
self.onmessage = event => {
	const data = event.data;
    const res = data.x + data.y;
    self.postMessage({result: res});
}
```

> 上面的 `addEventListener('message')` 和 `onmessage` 可以相互替换

输出：

```
3
```

需要注意，数据传输的对象必须是可被克隆的，函数，自定义类这些不可被克隆的对象无法被传递。



### worker 线程引用其他的库

由于 worker 的隔离性做得太强了，如果我需要将一个已经开发完成的计算负载模块放在 worker 上，比如一个很复杂的图像处理算法，那就很伤脑筋了。因为 `postMessage` 无法传递函数，我们就无法像 python 和 go 那样通过闭包的方法将复杂的负载代码扔到多线程上跑。

那要怎么做呢？

worker 提供了一个函数 `importScripts` 可以用于引入外部的库代码。比如：

```javascript
importScripts('vcd.js');
```

这行代码会让 worker 发起一个网络请求，索取 `http://localhost:8000/vcd.js` 这个文件，并解析运行其中的 js 代码。

`importScripts` 有一个很爽的点，它可以像 JSONP 那样发起非同源的请求，比如：

```js
importScripts('https://kirigaya.cn/files/web/worker.js');
```

这样 worker 的可玩性大大提升。

不过 `importScripts` 并没有返回值，这个时候有人就要问了，那我怎么才能拿到引用库里面的函数呢？毕竟光引用没有返回值我也用不了呀！

我在这里提供两种方法仅供大家参考。

方法一：通过 self 对象进行挂载

如果你正在开发自己的 js 库，可以在需要导出的库结尾加上这么几行代码：

```javascript
// func.js
async function myFunction( ... ) { ... }

if (self) {
    self.myFunction = myFunction;
}
```

上述代码打包后，当 worker 中通过 `impotScripts('func.js')` 后， `myFunction` 就全局可用，即：

```javascript
// worker.js
importScripts('func.js');

// 使用1
self.myFunction();

// 使用2
myFunction();
```

方法二：伪造 window 对象

很多的 CDN 库都会把库函数挂载在 window 对象上，如果你直接在 worker 中引用，控制台会报错说 window 找不到，这个时候直接在 import 之前创建一个伪 window 就好了：

```js
const window = {};
importScripts('vcd.js');

console.log(window.getVcdStream);
// 可以打印出一个有效函数
```

方法三：采用 module 模式

一部分新的 CDN 库会选择使用 `module.exports` 的方法导出库，这种情况方法一就不适用了。

通过 module 模式创建一个 worker 就能让用户在 worker 内使用 ES6 标准的 import 了：

```js
// 主线程
const worker = new Worker('worker.js', {
    type: 'module'			// 默认是 classic
});


// worker.js
import Vcd from 'vcd.js';

console.log(Vcd);
```

---

## 数据转移

我们使用 web worker 的场景往往是计算密集型的任务，这种任务的输入数据往往也很大（一段视频、一张高分辨率图片、一个3D的mesh）。web worker 的 `postMessage` 方法都是先拷贝再传递，对于大型数据输入这么做看起来非常不优雅，因此，`postMessage` 第二个参数 `transfer` 提供了优雅的解决方案。我们先看一段代码：

```javascript
// 主线程
const arrayBuffer = await window.readVcdFile();

console.log('进入 worker 前', arrayBuffer);

const worker = new Worker('worker.js');

worker.postMessage(arrayBuffer);

worker.addEventListener('message', () => {
    console.log('进入 worker 后', arrayBuffer);
});

// worker.js
self.onmessage = event => {
    const arrayBuffer = event.data;
    const uint8array = new Uint8Array(arrayBuffer);
	// ...
    self.postMessage('finish');
}
```

我们和之前一样，把一段数据（ArrayBuffer）传入了 worker 中，这段数据在主线程中不会被改变，因此，log 的打印结果如下：

```
进入 worker 前 ArrayBuffer(20269852)
进入 worker 后 ArrayBuffer(20269852)
```

但是根据之前的说明，我们知道当 web 在执行 `worker.postMessage(arrayBuffer)` 时，实际上会把 `arrayBuffer` 拷贝赋值，如果 `arrayBuffer` 本身很大，这么做无疑消耗了性能。所以我们可以这么做来阻止拷贝赋值：

```javascript
// 主线程
...

worker.postMessage(arrayBuffer, [arrayBuffer]);

...
```

这么允许web直接对这个数据进行转移，而不进行拷贝。转移后的数据将无法在主线程中被访问，可以看一下 log 的打印结果：

```
进入 worker 前 ArrayBuffer(20269852)
进入 worker 后 ArrayBuffer(0)
```

可以看到，worker生效后再去访问 arrayBuffer 只会得到一个空的，这意味着原本的 arrayBuffer 被转移走了，而并不会因为拷贝赋值发生性能浪费。

---

## worker 中引入 wasm 的一些小问题

对于 js 中的计算密集型业务，我们往往会用 C++ 或者 rust 编译 wasm 的方式进行开发，此处以大家常用的 emscripten 编译器为例，这种时候，worker 内要如何引入 wasm 及其胶水代码呢？

假设我有一个 wasm 文件 `fun.wasm` 和它对应的胶水代码 `fun.js`。

在一些情况下，如果直接使用 `importScripts` 导入 wasm 的胶水代码后，会发生如下的 `CompileError`：

```
WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 44 4f @+0). Build with -sASSERTIONS for more info.
```

这个错误大家应该很熟悉了，它代表 wasm 文件找不到。说明 worker 执行环境下，wasm 的路径搜索程序出现了问题。

我们深入胶水代码中会发现，问题出在 `scriptDirectory` 这个变量上，我个人怀疑这是 emscripten 的bug。当我们在 worker 内 import 胶水代码时，emscripten 创建的全局变量 `ENVIRONMENT_IS_WORKER` 会被激活，从而生成了 worker 语境下的 `scriptDirectory`，源代码如下：

```js
ENVIRONMENT_IS_WORKER?scriptDirectory=self.location.href:"undefined"!=typeof document&&document.currentScript&&(scriptDirectory=document.currentScript.src)
```

我把它翻译一下：

```js
let scriptDirectory = '';
if (ENVIRONMENT_IS_WORKER) {
	scriptDirectory=self.location.href;
} else if (document && document.currentScript) {
    scriptDirectory=document.currentScript.src;
}
```

后续的 wasm 文件路径构造代码如下：

```js
isDataURI(wasmBinaryFile="fun.wasm")||(path=wasmBinaryFile,wasmBinaryFile=Module.locateFile?Module.locateFile(path,scriptDirectory):scriptDirectory+path);
```

翻译成人话就是：

```js
let wasmBinaryFile = 'fun.wasm';
// 如果不是一个数据 url
if (!isDataURI(wasmBinaryFile)) {
    // 如果有用于自定义的路径拼接策略就用用户定义的，否则直接拼在一起
    if (Module.locateFile) {
        wasmBinaryFile = Module.locateFile(wasmBinaryFile, scriptDirectory);
    } else {
        wasmBinaryFile = scriptDirectory + wasmBinaryFile;
    }
}
```

`Module.locateFile` 是用户定义的，我没有定义，所以上述代码必定会走：

```js
wasmBinaryFile = scriptDirectory + wasmBinaryFile;
```

由于 worker 上下文并没有 Module 这个变量，为了省事，我直接修改了胶水代码：

```js
isDataURI(wasmBinaryFile="fun.wasm")||(path=wasmBinaryFile,wasmBinaryFile=wasmBinaryFile=self.location.href.replace('worker.js', 'fun.wasm'));
```

具体情况可以视情况而定。这样就可以正常在新的线程中运行高负载的 wasm 了。

---

## 参考资料

- https://www.ruanyifeng.com/blog/2018/07/web-worker.html
- https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers
- https://emscripten.org/docs/