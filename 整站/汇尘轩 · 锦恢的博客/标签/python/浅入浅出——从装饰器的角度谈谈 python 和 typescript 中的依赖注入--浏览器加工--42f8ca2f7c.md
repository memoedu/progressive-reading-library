封面：樋口円香 作者：偶尤OYO大肥羊

## 前言 - 从路由注册来讲讲依赖注入

写过 python Flask 和 java springboot 的朋友应该都对基于装饰器的依赖注入有所了解，我不会讲什么是依赖注入，但是我会简单给阁下几个不同的例子，来体会一下一种设计上的模式。

比如后端开发中，路由的注册是非常常见的一个需求场景，比如 Flask 中关于路由注册的 python 代码，通过装饰器（decorator）实现：

```python
@app.route('/vecdb/similarity_search_with_score', methods=['post'])
def post_similarity_search_with_score():
    params = request.data.decode('utf-8')
    params: dict = json.loads(params)
    result_data = []
    
    query = params.get('query', None)
    ...
```

上述代码在 `/vecdb/similarity_search_with_score` 中定义了一个 post 方法。

springboot 中的 java 也是类似的，不过在 java 中，类似的写法并不叫装饰器，而叫注解（annotation），当时原理上，decorator 和 annotation 也完全不一样。

> 为了避免不必要的误会，需要说明，对于大部分 Java 程序员而言，

```java
...
	@PostMapping("/login")
    @ApiOperation("登录用户")
    public Result<UserLoginVO> userLogin(@RequestBody UserLoginDTO userLoginDTO) {
        try {
            Result<UserLoginVO> resultVO = userService.login(userLoginDTO);
            return resultVO;
        } catch (Exception e) {
            return Result.error(MessageConstant.LOGIN_FAILED);
        }
    }
...
```

这样定义了一个 `/login` 下的 post 请求。

python 和 java 的这两个写法实际上是把路由的注册+定义在一处完成了，这么做是非常优雅的。它们的基本实现逻辑都是通过 `@function` 帽子把下面的对象的控制权移交给框架，来让框架自己完成问题的解决（此处是路由的注册）。这样就完成了逻辑上的解耦。

为了体现出这么做的好处，我们来看看 go 语言中的 gin 框架是如何定义路由的：

```go
// router.go
...
	// 用户相关 登录注册
	userRouter := mainRouter.Group("/api/user")
	{
		userRouter.POST("/sign-in", user.SignIn)
	}
...
```

上述 go 在 `/api/user/sign-in` 定义了一个 POST 请求，具体实现在 user 这个 package 的 SignIn 函数中，这样做分离了路由的注册和定义。比较适合扁平化的小团队进行开发和管理。但是缺点就是，当你好久没有接触这个项目后，实现了一个功能后，往往会忘记去 router 中去注册对应的路由，导致测试失败。而且在大型项目中， router.go 会越写越大，到后期难以进行管理。

> 当然！我希望阁下认识到路由注册和实现的分离的坏处，也希望阁下可以知道它的好处。本文所说的设计理念只能适用于特定场合，具体是什么场合需要阁下身体力行地去求索。没有万精油或者“银弹”般神奇的模式，否则，架构师早就失业了。

像 python 和 java 那样，通过一个 `@function` 这样的帽子就能进行函数控制权移交的写法通常被我们称为依赖注入（这只是依赖注入的一种实现，依赖注入是一个比较大的概念）。它的好处不言而喻，可以非常好地实现逻辑上的解耦，对系统上的功能共性进行抽象。目的是减少代码量，更好地管理项目。

比如我在 [Python进阶笔记（一）装饰器实现函数/类的注册](https://zhuanlan.zhihu.com/p/350821621) 中提到的，深度学习的著名框架 `pytorch` 的80%的代码都是在对不同的深度学习算子进行kernel匹配，其中检索对应实现的逻辑就可以通过注册器实现，而 python 的注册器本质上就是一种依赖注入。如果你不写注册器，自己每写完一个算子，就去对应的字典静态地申明它，那么存在如下三个缺点：

1. 不利于团队协作，新成员实现了一个OP需要问你去哪里注册它。
2. 项目后期如果需要改动架构，会变得非常麻烦。
3. 复杂系统需要注册的地方往往不止一处，比如对于深度学习 OP 而言， cuda selection 需要它的注册，计算图管理模块可能也需要它的注册。

对于 java 的依赖注册，网上的文章很多了，虽然良莠不齐，但是仔细找一找还是能找到的。

但是对于 python 和 typescript 而言，如何实现基于装饰器的依赖注入，网上能找到的资源就有点捉襟见肘了。所以，本着交流学习的态度，鄙人拙笔为尘，抛砖引玉。

---

## python 中基于装饰器依赖注入

### 基本原理

关于 python 的装饰器如何使用，基于装饰器如何实现注册器，我在 [Python进阶笔记（一）装饰器实现函数/类的注册](https://zhuanlan.zhihu.com/p/350821621) 中写得比较详细了。此处书接上文，更多是给出应用的例子。

### 例子：RAG 的 path -> url 映射层

此处举的例子是我最近在开发的一个 拉格朗日后端的反向 websocket 接入层。我在其中实现了一个基于向量数据库的 RAG 系统。

RAG 的知识库来自于我精心搜集的来源于不同网站的知识，向量数据库只会返回 segment 的本地相对路径，为了给我的群友提供更好的信息，我需要写一个转换规则，把这些本地的知识库（markdown文件）路径映射到它来源于的网址。

> 当然，具体怎么爬取的就涉及到 CSR 爬虫服务器的实现了，在 [网络爬虫进阶（二）更加现代的无头浏览器 puppeteer 安装与使用](https://kirigaya.cn/blog/article?seq=230) 中，我提到了基本实现，感兴趣的朋友可以把玩片刻。

比如，如下的例子就是两个可能的映射：

```
docs/kirigaya.cn/129.md -> https://kirigaya.cn/blog/article?seq=129
docs/digital-document/guide/quick-start.md -> https://sterben.nitcloud.cn/zh/guide/quick-start.html
```

为了实现这样的映射，我们当然可以在系统中编写 if else，但是如果未来要加入其他的网站的数据，会让系统越来越长，越来越丑。

此处，我们可以用注册器完成对于映射层函数的依赖注入，将后续的开发者实现的 path -> url 映射函数自动加入系统中，我们再自己编写逻辑层去处理这些映射的匹配。

我实现的一个代码如下：

```python
from typing import Callable

SourceMappingFunction = Callable[[str], str]

class UrlMappingRegister:
    def __init__(self) -> None:
        self.startsWith_patterns: dict[str, SourceMappingFunction] = {}

    def startsWith(self, start: str) -> Callable[[SourceMappingFunction], SourceMappingFunction]:
        def register(start_name: str, func: SourceMappingFunction) -> SourceMappingFunction:
            self.startsWith_patterns[start_name] = func
            return func
        return lambda func: register(start, func)
    
    def url_from_mapping(self, source: str) -> str:
        # 此处只实现了 startswith 模式匹配
        for pattern in self.startsWith_patterns:
            func = self.startsWith_patterns[pattern]
            if source.startswith(pattern):
                try:
                    return func(source)
                except Exception as e:
                    return ''

urlmapping = UrlMappingRegister()
```

当主函数中，系统需要对 RAG 的文本路径做映射时，可以这么做：

```python
# source 的一个可能的值： docs/kirigaya.cn/129.md
source = urlmapping.url_from_mapping(source)
```

那么具体如何去实现 `startswith` 就是后面要做的了，这部分代码甚至可以自动生成：

```python
# 样例： docs/kirigaya.cn/129.md
# 目标： https://kirigaya.cn/blog/article?seq=129
@urlmapping.startsWith('docs/kirigaya.cn')
def kirigaya_cn(source: str) -> str:
    pass

# 样例： docs/digital-document/guide/quick-start.md
# 目标： https://sterben.nitcloud.cn/zh/guide/quick-start.html
@urlmapping.startsWith('docs/digital-document')
def digital_document(source: str) -> str:
    pass
```

这样瞬间就定义完成了对于 `kirigaya.cn` 和 `digital-document` 这两个文件夹中的知识库的映射函数。就算对业务完全不了解的程序员，知道基本的 IPO 后也能完成上面这两个函数。TDD 也很好做。

如果阁下学会了上述的写法，想要自己写一个 Flask 的架子也不是问题了。

---

## TypeScript 中基于装饰器依赖注入

### ts 装饰器

typescript 目前也支持了装饰器，而且因为其类型推导的体验。使用 typescript 进行装饰器的依赖注入可以取得介于 python 和 java 之间的开发体验。

不同于 python，typescript 中，装饰器只能包在 class 里面使用。

开发 Typescript 的工程师之一 **[DanielRosenwasser](https://github.com/DanielRosenwasser)** 在https://github.com/microsoft/TypeScript/issues/3661 里面提到：

> Decorators are not supported on function declarations. To quote [#2249](https://github.com/microsoft/TypeScript/issues/2249):
>
> - Decorating a function declaration is not allowed as it will block hoisting the function to the top of the scope, which is a significant change in semantics.
> - Decorating function expressions and arrow functions are not supported. The same effect can be achived by applying the decorator function as `var x = dec(function () { });`
> - Decorating function formal parameters is currently not part of the ES7 proposal.

介于熟悉 ts 的朋友应该远远少于 python，所以我下面会详细讲解一下 ts 中装饰器的使用。

首先没有 typescript 的朋友先安装一下：

```
npm install -g typescript
```

新建一个文件夹，创建 `tsconfig.json` 文件，写入：

```json
{
    "compilerOptions": {
        "target": "ES2015",
        "experimentalDecorators": true,
    },
    "files": [
        "main.ts"
    ]
}
```

创建 `main.ts`，写入

```typescript
interface CallLog {
    name: string
}

class Register {
    constructor() {

    }

    log(config: CallLog) {
        console.log('enter outer');
        return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
            console.log('enter inner');
            console.log(target);
            console.log(propertyKey);
            console.log(descriptor);
        }
    }
}

const register = new Register();
```

其中，ts装饰器函数必须返回一个签名如下的函数：

```typescript
function(target: any, propertyKey: string, descriptor: PropertyDescriptor): any {}
```

此处实现的 log 是一个用于记录函数调用的跟踪器，我们接下来把这个 log 装饰到随便一个函数上，比如装饰到一个简单的算加法的函数上：

```typescript
class Impl {
    @register.log({ name: 'test' })
    public add(a: number, b: number): number {
        return a + b;
    }
}
```

我们运行一下，看看结果是什么样子的：

```
tsc; node main.js
```

输出

```
enter outer
enter inner
{}
add
{
  value: [Function: add],
  writable: true,
  enumerable: false,
  configurable: true
}
```

可以看到，初次装饰后，运行时会调用装饰器的返回函数。实际上，ts 编译器的编译期间会把这个过程模拟成一个入栈出栈的过程。

### 装饰器的编译期执行顺序

首先， ts 会先按照从上到下的顺序扫描一遍当前函数 `add` 的所有装饰器，调用它们，然后将装饰器函数的返回值入栈；扫描结束后，编译器会从栈中依次取出装饰器函数的返回值，从上面的代码中看出，返回值也是一个函数，然后 ts 编译器会再去调用它们。因此，上述的执行顺序是先执行 `enter outer` 的输出，再执行 `enter inner` 的输出。

如果我们实现了两个 log 函数：

```typescript
class Register {
    constructor() {

    }

    log1(config: CallLog) {
        console.log('enter log1 outer');
        return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
            console.log('enter log1 outer');
        }
    }
    
    log2(config: CallLog) {
        console.log('enter log2 outer');
        return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
            console.log('enter log2 outer');
        }
    }
}
```

然后依次挂在 add 上面：

```typescript
class Impl {
    @register.log1({ name: 'test1' })
    @register.log2({ name: 'test2' })
    public add(a: number, b: number): number {
        return a + b;
    }
}
```

那么它们的输出就应该是：

```
enter log1 outer
enter log2 outer
enter log2 inner
enter log1 inner
```

### target, propertyKey, descriptor

再说剩下的三个返回值：

```
{}
add
{
  value: [Function: add],
  writable: true,
  enumerable: false,
  configurable: true
}
```

它们分别代表了 `target, propertyKey, descriptor`，它们的含义如下：

- `target`：被装饰对象的原型对象，此处为函数，函数的原型为 `{}`
- `propertyKey`：被装饰对象的名字，此处为函数名 `add`
- `descriptor`：一个关于被装饰对象的代理，它的接口如下

```typescript
interface PropertyDescriptor {
    configurable?: boolean;
    enumerable?: boolean;
    value?: any;
    writable?: boolean;
    get?(): any;
    set?(v: any): void;
}
```

其中，`get` 和 `set` 都是对于 `value` 的 getter 和 setter；`value` 是被装饰对象的引用。

### 案例：函数跟踪器

有了 getter 和 setter 之后，意味着我们能玩的操作就很多了。比如我们刚刚的函数跟踪器，就可以实现了，我们接下来把刚刚的函数跟踪器实现以下，它的基本功能如下：

- 当被装饰函数被调用时，可以打印出信息（IO 定向到终端或者磁盘上）
- 装饰器自动记录函数的花费的时间，并打印出返回值。
- 跟踪器可以根据输入的 `name` 配置基本的分组，默认为 `default`。
- 跟踪器可以将函数的调用记录（在什么时候调用了什么函数）记录下来。

一个我实现好的监视器装饰器代码如下：

```typescript
interface CallLog {
    name?: string
}

class Register {
    private invokeHistory: Map<number, string>;
    constructor() {
        this.invokeHistory = new Map<number, string>();
    }

    log(config?: CallLog) {
        const name = config?.name || 'default';
        const _this = this;
        return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
            const fn = descriptor.value;
            descriptor.value = function() {
                console.log(`[监视器编号 ${name}] 调用 ${propertyKey}`);
                _this.invokeHistory.set(Date.now(), propertyKey);
                const s0 = Date.now();
                const r0 = fn(...arguments);
                if (r0 instanceof Promise) {
                    return (async () => {
                        const result = await r0;
                        const s1 = Date.now();
                        console.log(`[监视器编号 ${name}] ${propertyKey} 执行完成，耗时 ${s1 - s0} ms，结果 ${result}`);
                        return result;
                    })();
                } else {
                    const s1 = Date.now();
                    console.log(`[监视器编号 ${name}] ${propertyKey} 执行完成，耗时 ${s1 - s0} ms，结果 ${r0}`);         
                    return r0;
                }
            }
        }
    }
}

const register = new Register();
```

我们接下来可以简单测试一下：

```typescript
function sleep(timeout: number) {
    return new Promise<void>(resolve => {
        setTimeout(() => {
            resolve();
        }, timeout);
    });
}

class Impl {
    @register.log()
    public add(a: number, b: number): number {
        return a + b;
    }

    @register.log({ name: 'server' })
    public async getImage(url: string) {
        const res = await fetch(url);
        const arraybuffer = await res.arrayBuffer();
        return arraybuffer;
    }

    @register.log({ name: 'server' })
    public async testSleep(ms: number) {
        await sleep(ms);
        return 'this is a test after 2s';
    }
}
```

我们此处装饰了三个函数，其中一个同步函数，两个异步函数。我们在主函数中调用它们：

```typescript
(async() => {
    const impl = new Impl();
    impl.add(1, 2);
    await impl.getImage('https://picx.zhimg.com/80/v2-e7b0e5e5b7a9a255b822806acb01ce20_1440w.jpeg');
    await impl.testSleep(2000);
})();
```

输出结果：

```bash
[监视器编号 default] 调用 add
[监视器编号 default] add 执行完成，耗时 0 ms，结果 3
[监视器编号 server] 调用 getImage
[监视器编号 server] getImage 执行完成，耗时 983 ms，结果 [object ArrayBuffer]
[监视器编号 server] 调用 testSleep
[监视器编号 server] testSleep 执行完成，耗时 2005 ms，结果 this is a test after 2s
```

可以看到，完美完成了任务。

---

## 总结

通过装饰器进行依赖注入在设计中是很常见的一种做法，本篇也只是展示了其中的冰山一角。更多的对于装饰器的理解需要阁下在项目中亲身体会。