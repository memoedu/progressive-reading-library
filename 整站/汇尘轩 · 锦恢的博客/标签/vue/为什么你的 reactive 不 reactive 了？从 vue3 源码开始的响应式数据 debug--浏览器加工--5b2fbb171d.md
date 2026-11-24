## 前言

最近业余时间在疯狂抽时间开发 openmcp，由于我设计的底层数据结构，考虑到需要同时编译到不同类型的终端（web，插件，nodejs sdk，electron），还需要借助 vue3 的响应式来尽可能减轻开发负担，所以，设计得稍微有那么一点点小复杂。

就在最近增加的一个 feature 后，我发现，诶？一个函数修改了某个数据结构后，这个数据结构绑定的 DOM，它不更新了！牙白，这可不行，于是乎我给这个数据结构套上了一层 reactive，然后就重新变得「可响应」了，但是我肯定是不满足的，因为这个数据结构它会被 push 到一个 reactive 的数组中，push 后的它应该就已经是响应式的变量了呀！

于是，在 openmcp 0.1.0 发布后，我决定复习一下 vue3 reactive 的源代码，然后对我上面遇到的问题进行一次最小复现。

---

## 最小复现

直接讲 reactive 源代码估计一堆人又要睡着了，所以祖宗之法可以变，先做一次最小复现吧。打开命令行，光速创建一个 vue3 项目：

```bash
cd ~/code/nodejs
# 记得选择 typescript
npm create vue@latest
```

创建完成后，在 App.vue 的同级目录下创建 ads.ts，写入如下代码：

```typescript
// ads.ts
import { reactive } from "vue";

export class Ad {
    constructor(
        public id: number,
        public title: string,
        public description: string,
        public price: number
    ) {}
}

export class Ads {
    public ads: Ad[] = [];
    constructor() {}
}

export const AdManager = reactive(new Ads());
```

这是一段响应式的全局变量，注册了一个全局广告管理器 `AdManager`，我们通过它来实现广告对象的增删改查和全项目任意组件的自由访问。由于直接整个套上了 reactive，所以 AdManager 下的所有属性和属性的属性理应被 vue 绑定在了使用它们的 DOM 上，我们的任何对 AdManager 的任意深度的修改，都应该自动更新 DOM。

现在，我们在 App.vue 中创建最小复现的内容。我们实现一个如下的内容：页面上存在一个按钮，点击这个按钮，就会自动生成一条广告，并且，这个广告自己会每隔 1秒 刷新自己的价格。

App.vue 代码如下：

```html
<template>
	<div>
		<button @click="addRandomAd">Add Random Ad</button>
	</div>
	<br>
	<div v-if="AdManager.ads.length > 0">
		<h3>All Ads:</h3>
		<ul>
			<li v-for="ad in AdManager.ads" :key="ad.id">
				{{ ad.title }} - {{ ad.description }} (${{ ad.price }})
			</li>
		</ul>
	</div>
</template>

<script setup lang="ts">
import { Ad, AdManager } from './ads';

function generateRandomAd(): Ad {
    const id = Math.floor(Math.random() * 1000);
    const titles = ['Special Offer', 'Limited Time', 'New Arrival'];
    const descriptions = ['Best deal ever!', 'Don\'t miss out!', 'Exclusive offer'];
    return new Ad(
        id,
        titles[Math.floor(Math.random() * titles.length)],
        descriptions[Math.floor(Math.random() * descriptions.length)],
        Math.floor(Math.random() * 100)
    );
}

function addRandomAd() {
    const newAd = generateRandomAd();
    AdManager.ads.push(newAd);

	// 设置一个句柄，每隔一秒随机更新广告的价格
	const intervalId = setInterval(() => {
		newAd.price = Math.floor(Math.random() * 100);		
	}, 1000);
}

</script>

<style scoped>
header {
	line-height: 1.5;
}

.logo {
	display: block;
	margin: 0 auto 2rem;
}

button {
	background-color: #007bff;
	color: white;
	border: none;
	cursor: pointer;
}

@media (min-width: 1024px) {
	header {
		display: flex;
		place-items: center;
		padding-right: calc(var(--section-gap) / 2);
	}

	.logo {
		margin: 0 2rem 0 0;
	}

	header .wrapper {
		display: flex;
		place-items: flex-start;
		flex-wrap: wrap;
	}
}
</style>
```
代码比较简单，每次我们点击蓝色按钮的时候，新的广告都会加入 AdManager 这个全局变量中被托管，同时，DOM也会更新，将最新的广告渲染上去。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-7f174641c042f31fa2d53c57d36ab014_1440w.png" style="width: 100%;"/>
</div>

但是，这里有一个 bug，你会发现，虽然添加广告的 DOM 更新是正常的，但是，每隔 1s 更改价格的操作并没有被更新到 DOM 里面，页面是静止不动的。如果你在 `setInterval` 中打上 log 的话，你会发现，价格在内存中其实是在变化的，只是没有被渲染到 DOM 中。

这说明了一件事，AdManager 是 reactive 的，但是我们创建的 ad 它不 reactive 了！这是为什么呢？让我们慢慢来看。

---

## 为什么 reactive 对象的任意成员也是 reactive 的？

想要理解上面的问题，我们不妨更进一步吧！既然「AdManager 是 reactive 的，但是我们创建的 ad 它不 reactive 了」，我们不妨反过来问：为什么 AdManager 是 reactive 的，它的成员变量 ads 数组就应该是 reactive 的呢？

想要回答这个问题，我们就需要探索一下 reactive 的源代码了。

> 为了叙述简单，这篇文章暂时不讨论 reactive 的数据修改为什么会自动修改 DOM，这个涉及到 effect 任务和 reactive get 的捕获机制，和我们本文的讨论内容并无直接关系。

在那之前，希望阁下能了解一下 es6 标准提出的 Proxy 这个 javascript 内置对象的作用。

### Proxy 是什么？

Proxy 是 javascript 标准中提出的一个对象，它可以用来拦截和定义对象的操作。比如，我们可以通过 Proxy 来拦截对象的读取、写入、删除等操作，从而实现一些自定义的行为。

下面是一个基本的例子：

```javascript
const target = { name: "Alice", age: 25 };

// 定义拦截器
const handler = {
  get(target, prop, receiver) {
    console.log(`Reading property: ${prop}`);
    return Reflect.get(target, prop, receiver);
  }
  set(target, prop, value, receiver) {
    console.log(`Setting property: ${prop} to ${value}`);
    return Reflect.set(target, prop, value, receiver);
  }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // 输出: "Reading property: name" → "Alice"
console.log(proxy.age);  // 输出: "Reading property: age" → "25 years old"

proxy.name = "Bob";     // 输出: "Setting property: name to Bob"
proxy.age = 30;         // 输出: "Setting property: age to 30"
```

### reactive 本质是 Proxy 的高层封装

经常调试 vue 项目的朋友应该知道，被 reactive 包裹的对象，通过 console.log 输出的时候，其实就是一个 Proxy 对象。也就是说，reactive 本质是对 Proxy 的更加高层次的封装。

因为 Proxy 是可以拦截对象的访问和赋值的，所以，你应该能看懂下面这个例子和它的注释。

```html
<!-- 此处触发 data 的 get，data 对象被加入渲染的函数中 -->
<div>{{ data.name }}</div>

<script setup lang="ts">
const data = reactive({
    name: "Alice",
    age: 25
});

// 此处触发 data 的 set，data 对象被重新渲染
// 由于在第一行中，data 因为 get 被加入了渲染函数中，
// 所以，此处的 set 会触发重新渲染第一行的 DOM 的句柄函数
data.name = "Bob";
</script>
```

此处不讨论数据和DOM的依赖方向，你只要知道，通过 Proxy，我们创造了一个机会，一个数据和DOM可以在一个作用域相互访问到彼此变量的机会，这样它们就可以把彼此加入各自的调度队列中，实现渲染和赋值的解耦合。

---

## reactive 的基本实现

如果你能理解 Proxy 的工作原理，以及 reactive 和 Proxy 的基本关系，那么下面对于 reactive 的基本讲解，你就应该比较明白了。

vuejs 的源代码在这里：https://github.com/vuejs/core/blob/main/packages/reactivity/src/reactive.ts  ，我知道你 1000% 不会去看，产品级代码总是包含了大量影响我们理解基础逻辑的 corner cases，所以我就简单浓缩了一下 reactive 实现。

### reactive 简化实现

```typescript
export function reactive(target: object) {
  // 如果是只读的，那么直接返回
  // 也就是这个情况：
  // const data = reactive(readonly({ name: "Alice", age: 25 }));
  // 此时，reactive 有和没有一个样子
  if (isReadonly(target)) {
    return target
  }

  // 创建 reactive 的代理对象
  return createReactiveObject(
    target,         // 原始对象
    false,          // 是否只读
    mutableHandlers,            // target 是 Object 时的 handler
    mutableCollectionHandlers,  // target 是 Map Set 等这些常用集合时的 handler
    reactiveMap,    // 用来缓存的，堆地址相同的对象不会被创建两份 reactive
  )
}
```

### createReactiveObject 简化实现

然后来看核心函数 `createReactiveObject`：
```typescript
function createReactiveObject(
  target: Target,
  isReadonly: boolean,
  baseHandlers: ProxyHandler<any>,
  collectionHandlers: ProxyHandler<any>,
  proxyMap: WeakMap<Target, any>,
) {
  
  // ... 一些 corner case 的处理 ...
  //
  // reactive(reactive({})) 直接返回内层的 reactive
  // readonly(null) 或者 readonly(1) 返回内部变量，然后抛出警告
  // markRaw 标记过的对象不需要被代理，直接返回内部变量
  // 同一个对象被套了两次 reactive，那么第二次 reactive 复用第一次的代理结果
  //
  // ... 一些 corner case 的处理 ...

  // 创建代理对象，如果是 target 是 Object，那么就用 baseHandlers 拦截器
  const proxy = new Proxy(
    target,
    // reactive({ name: "Alice", age: 25 }) 会使用 baseHandlers
    // reactive(new Map()) 会使用 collectionHandlers
    targetType === TargetType.COLLECTION ? collectionHandlers : baseHandlers,
  )

  // 缓存代理对象，避免重复创建
  proxyMap.set(target, proxy)
  return proxy
}
```

> 其实上面部分讲的不太严谨，因为 reactive 是用的弱引用管理缓存的，如果第一次创建的代理对象的 RC 归零了，那么缓存就会失效。哎呀，反正你们进来看原理的，管这些有的没的干啥。

接下来的重头戏就是 handler 了，这里为了简单说明，毕竟大家平时肯定是 `reactive({ name: "Alice", age: 25 })` 这种用法用得多，所以我们就只看 `baseHandlers` 了。

通过上下文，你能找到 `baseHandlers` 的本体是 `MutableReactiveHandler`

```typescript
export const mutableHandlers: ProxyHandler<object> =
  /*@__PURE__*/ new MutableReactiveHandler()

class MutableReactiveHandler extends BaseReactiveHandler;
```

`MutableReactiveHandler` 中并没有实现 get，MutableReactiveHandler 的 get 方法继承自 `BaseReactiveHandler`。

而我们本来想要弄清楚的就是，为什么 reactive 对象的成员也是 reactive，而通过 Proxy 的基本知识，我们知道了访问 reactive 对象成员的过程是会调用 handler 的 get 的，因此，我们只需要看 `BaseReactiveHandler` 的 get 实现即可。

### BaseReactiveHandler 简化实现

`BaseReactiveHandler` 的简化实现版本如下：

```typescript
class BaseReactiveHandler implements ProxyHandler<Target> {
  constructor(
    protected readonly _isReadonly = false,
    protected readonly _isShallow = false,
  ) {}

  get(target: Target, key: string | symbol, receiver: object): any {
    // ... 一些 corner cases ...
    // 和 createReactiveObject 开头差不多，除此之外，定义了部分内置属性
    // 访问这些内置属性永远不会是 reactive
    // 对于 data = reactive([1,2,3]); data[0] 这种数组情况的处理

    const res = Reflect.get(
      target,
      key,
      // 如果 target 是 ref 创建的，走 ref 的代理，否则，走 BaseReactiveHandler 的
      isRef(target) ? target : receiver,
    )

    if (isSymbol(key) ? builtInSymbols.has(key) : isNonTrackableKeys(key)) {
      return res
    }

    if (!isReadonly) {
      // 如果不是只读的（也就是 reactive 创建的），那么需要跟踪它的变化
      // 当前这个 target 对象的变化会被全局 O(K) 地捕获到，然后决定后续是否更新 DOM 或是 watch，computed 这些
      track(target, TrackOpTypes.GET, key)
    }

    if (isShallow) {
      // shallowReactive 浅层代理，只需要代理第一层，后续的直接返回 raw 值 
      return res
    }

    if (isRef(res)) {
      // 差不多意思就是你 tnd 在 reactive({ name: ref("kirigaya") }) 这种情况
      // 此处的 res 就是 ref("kirigaya")，默认会返回 ref("kirigaya").value
      return targetIsArray && isIntegerKey(key) ? res : res.value
    }

    if (isObject(res)) {
      // 如果访问到的成员还是对象，那么递归嵌套 reactive
      return isReadonly ? readonly(res) : reactive(res)
    }

    return res
  }
}
```

### 超级精简版本 reactive

相信对看懂 Proxy 的你而言，上面的代码应该很简单。假设我们是通过 reactive 创建的对象，而且你也不会做出什么 reactive 里面套 ref 的掺冰操作，你也不会访问 reactive 对象的一些神奇的内置属性和方法，那么上面的代码再简化一下，一个不考虑任何优化和边界条件的原理级的 reactive 代码如下：（set 就懒得写了，对说明最开头那个 bug 意义不大）

```typescript
class BaseReactiveHandler {
  constructor(
  ) {}

  get(target: Target, key: string | symbol, receiver: object): any {
    const res = Reflect.get(target, key, receiver);
    track(target, TrackOpTypes.GET, key);

    if (res !== null && typeof res === 'object') {
      return reactive(res);
    }

    return res;
  }
}

export const handler = new BaseReactiveHandler(...);

export function reactive(target: object) {
  const proxy = new Proxy(target, handler);
  return proxy;
}
```

你稍微有点算法基础都应该能看出来，这其实就是一个递归，边界条件是代理输出是不是 object。毕竟，只有 object 才有被 get 的意义。

### 回到上面的问题

回到上面的问题，为什么 reactive 的成员也是 reactive ？因为这段代码：

```typescript
    if (res !== null && typeof res === 'object') {
      return reactive(res);
    }
```

而只有 reactive 的对象在被 get 的时候，才会调用 `track` 在后续渲染中被侦听变动。所以回到我最初的那个问题，为什么广告对象每秒变换价格，但是内存上的变化并没有被渲染到 DOM 中呢？很简单，看一下之前代码你就知道：

```typescript
// ... exist code ...

function addRandomAd() {
    const newAd = generateRandomAd();
    AdManager.ads.push(newAd);

	// 设置一个句柄，每隔一秒随机更新广告的价格
	const intervalId = setInterval(() => {
		newAd.price = Math.floor(Math.random() * 100);		
	}, 1000);
}

// ... exist code ...
```

我们加入 setInterval 闭包中的 newAd 它只是 generateRandomAd 创建的一个普通对象，它并不是一个 reactive，自然它的变化也就没有被 trace 函数注册到全局。

---

## 解决方法

如果你能看到这里，而且还看懂了，恭喜你，你已经超越了 3.14% 的全栈工程师。结合对于 reactive 的认知，你能提出什么解决方案呢？Anyway，正确的思路一定是要把一个 reactive 加入 setInterval 的闭包中，为了实现这一点，有如下两个大类解决方法。

### 方法一：使用 AdManager

reactive 的源代码告诉我们，reactive 具备“传染性”，任何 reactive 对象，它的 handler 的 get 都保证了这种 reactive 会被继承到它的通过 `.` 访问所有成员中，而且是链式传导。

那么我们通过本来就是 reactive 的 AdManager 来获取创建后的广告对象即可。

```typescript
function addRandomAd() {
    const newAd = generateRandomAd();
    AdManager.ads.push(newAd);
    // AdManager 的 reactive 特性会被注入到它发起的任何访问结果
    const reactiveAd = AdManager.ads[AdManager.ads.length - 1];

	// 设置一个句柄，每隔一秒随机更新广告的价格
	const intervalId = setInterval(() => {
		reactiveAd.price = Math.floor(Math.random() * 100);		
	}, 1000);
}
```

### 方法二：提前 reactive

我们还可以在 push 进入数组前，就让我们创建的 newAd 是 reactive，通过 reactive 源代码，我们知道了 vue 内部对于 `reactive(reactive({}))` 只会返回内层，不会创建额外代理，所以这么做虽然看起来很傻，但是不会有性能损失。

```typescript
function addRandomAd() {
    const newAd = reactive(generateRandomAd());
    AdManager.ads.push(newAd);

	// 设置一个句柄，每隔一秒随机更新广告的价格
	const intervalId = setInterval(() => {
		newAd.price = Math.floor(Math.random() * 100);		
	}, 1000);
}
```

---

## 结局

就这样，我们完美解决了上面的问题，还做了知其然，且知其所以然。对于技术人员而言，这么做是减轻技术债的唯一途径。