

## 前言

最近兴趣使然，想要看看 js 中的 `async/await` 是否还有别的实现方法。

## JS 中的异步执行

我们都知道， js 中提供的 await 使得我们可以把异步的 Promise 返回值按照同步的方式进行书写，比如最常见的，js 中的基于宏任务的 `Promise` + `setTimeout` 写法：

```js
function sleep(second) {
    const ms = second * 1000;
    return new Promise((resolve, reject) => {
        setTimeout(resolve, ms);
    });
}
```

使用起来，直接 `await` 即可：

```js
async function asyncMain() {
    await sleep(1);
    console.log('hello world');
    await sleep(0.2);
    console.log('hello world');
    await sleep(1);
    console.log('hello world');
}

asyncMain();
```

输出结果：

```
(1秒后)hello world
(0.2秒后)hello world
(1秒后)hello world
```



那么 await 是如何执行这段函数的，我们又有什么其他方法可以等效写出类似的东西？

## JS 中的生成器

在 python 中，我们都知道有生成器这么一个机制，它是 python 支持协程最直接的体现：

```python
def generator():
    yield 1
    yield 2
    yield 3

gen = generator()
next(gen)		# 返回 1
next(gen)		# 返回 2
next(gen)		# 返回 3
next(gen)		# Traceback ... StopIteration
```

在 js 中，也有类似的写法：

```js
function* generator() {
    yield 1;
    yield 2;
    yield 3;
}

gen = generator();
gen.next()		// 返回 { value: 1, done: false }
gen.next()		// 返回 { value: 2, done: false }
gen.next()		// 返回 { value: 3, done: false }
gen.next()		// 返回 { value: undefined, done: true }
```

通过生成器，我们就可以直接获取同步函数内部的 Promise 返回值，然后在外部通过一个小的递归函数执行，从而实现与 await 类型的效果。

## 基于生成器的 await

假设我们希望通过 `yield` 和生成器的 `next` 函数实现同步执行 `Promise`，那么我们可以在外部接受 yield 返回的东西时执行 then，然后通过递归调用的方式逐步完成内部所有 Promise 的执行。

一个非常简单的递归代码如下：

```js
function runGenerator(generator) {
    const gen = generator();
    const cb = (gen) => {
        const res = gen.next();
        if (!res.done) {
            res.value.then(() => {
                cb(gen);
            })
        }
    }
    cb(gen);
}
```

有了这个函数，我们就可以通过 `yield` 来实现 `await` 了：

```js
function* genMain() {
    yield sleep(1);
    console.log('hello world');
    yield sleep(0.2);
    console.log('hello world');
    yield sleep(1);
    console.log('hello world');
}

runGenerator(genMain);
```

输出结果：

```
(1秒后)hello world
(0.2秒后)hello world
(1秒后)hello world
```

是不是非常简单且巧妙，我之前怎么就没有想到这个方法呢？