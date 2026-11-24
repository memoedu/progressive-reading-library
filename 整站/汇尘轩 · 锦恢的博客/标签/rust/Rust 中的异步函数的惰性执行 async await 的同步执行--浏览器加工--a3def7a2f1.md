## 前言

最近在用 rust 的 tokio 异步运行时开发新的 lsp 服务器，在进行消息通讯时遇到了一个小坑。这个问题的场景是这样的：lsp 收到一个前端的文件变动后，需要为当前文件创建一个用于进行 AST 解析的 move 线程进行动态解析，为了节省计算开支，我在这个 move 线程计算得到 AST 后需要复用一次构建一个 IR，并将这个IR 以 send notification 的方式发送给前端。

:::info request 和 notification

request 和 notification 都是 jsonrpc 协议中的消息类型（lsp 协议基于 jsonrpc 协议），request 消息类似于 Web 中的 GET，POST 这样的 Ajax 请求，请求方需要等待响应方的响应；notification 则不需要请求方等待响应，发送出去后，当前发送请求就会从调用栈中被剔除。

:::

由于我的线程是通过 `move || ()` 闭包构造的，并且将返回的线程句柄挂在了一个全局对象上，不太方便改成异步闭包。因此，闭包内部直接调用了 `client.send_notification`，rust 编译器也给出了警告，因为我没有对这个异步方法做 await。我当时也没在意，毕竟这个方法的 notification 本来就不需要响应，把 IR 发送给前端，我这边就什么也不需要管了。但是在进行前后联合调试时，我发现，这个 send_notification 一直没有被执行。

于是乎，我想起了当初学习 rust 时看到了异步惰性执行策略，遂做了一个做小复现：

文件名：src/main.rs

```rust
async fn async_print() {
    println!("info from async_print");
}

fn main() {
    let future = async_print();
    for i in 0 .. 5 {
        println!("number : {}", i);
    }
}
```

运行上面的代码，得到：

```
number : 0
number : 1
number : 2
number : 3
number : 4
```

你会发现，如果不去执行 `await`，rust 的异步方法永远不会被执行。果然和我记忆中的一样，rust 的 Future 是惰性执行的，只有在 await 的地方才会真正执行。但是 await 只能在异步函数中使用，类似于 js 和 python，但是 rust 的 main 函数默认情况下是不能写作异步的。那么怎么办呢？

## 解决方法

### 方法一：tokio 运行时

如果你在 tokio 的运行时下，那么此时就可以在 main 函数上套一个 async 了，这样就可以通过 await 执行异步函数了。

文件名：Cargo.toml

```toml
[package]
name = "learn
version = "0.1.0"
edition = "2021"

[dependencies]
tokio = { version = "1.29.1", features = ["macros", "io-std", "rt-multi-thread"] }
```

文件名：src/main.rs

```rust
async fn async_print() {
    println!("info from async_print");
}

#[tokio::main]
async fn main() {
    let future = async_print();
    for i in 0 .. 5 {
        println!("number : {}", i);
    }
    future.await;
}
```

输出结果：

```console
number : 0
number : 1
number : 2
number : 3
number : 4
info from async_print
```

### 方法二：手写异步执行器

在我之前的例子中，调用异步函数的场景在 一个 move 闭包内，无法通过场景一的方法解决。那还有什么方法吗？我们好好回想一下异步协程的实现方式，在 [JavaScript 使用生成器实现 await](https://kirigaya.cn/blog/article?seq=227) 中，我们基于 generator 实现了一个 await，实际上，rust 中的 await 也类似。

rust 中的 await 本质上是一个语法糖，在 rust 编译器的源代码中 [compiler/rustc_ast_lowering/src/expr.rs](https://github.com/rust-lang/rust/blob/7e966bcd03f6d0fae41f58cf80bcb10566ab971a/compiler/rustc_ast_lowering/src/expr.rs#L717) 可以看到，await 本质上会被编译成

```rust
// pseudo-rust code
match ::std::future::IntoFuture::into_future(<expr>) {
    mut __awaitee => loop {
        match unsafe { ::std::future::Future::poll(
            <::std::pin::Pin>::new_unchecked(&mut __awaitee),
            ::std::future::get_context(task_context),
        ) } {
            ::std::task::Poll::Ready(result) => break result,
            ::std::task::Poll::Pending => {}
        }
        task_context = yield ();
    }
}
```

既然是一个基于生成器的 loop，那么我们在同步函数中直接把上面的过程仿照一遍就能执行一个异步函数了，代码如下：

```rust
use std::future::Future;
use std::pin::Pin;
use std::task::{Context, Poll, Waker, RawWaker, RawWakerVTable};

fn run_future<F>(future: F) -> F::Output where F: Future {
    let future = Box::pin(future);
    run_pin_future(future)
}

fn run_pin_future<F>(mut future: F) -> F::Output
where
    F: Future + Unpin,
{
    let waker = create_waker();
    let mut context = Context::from_waker(&waker);

    loop {
        match Pin::new(&mut future).poll(&mut context) {
            Poll::Ready(output) => return output,
            Poll::Pending => {},
        }
    }
}

fn create_waker() -> Waker {
    unsafe { Waker::from_raw(create_raw_waker()) }
}

fn create_raw_waker() -> RawWaker {
    RawWaker::new(std::ptr::null(), &VTABLE)
}

const VTABLE: RawWakerVTable = RawWakerVTable::new(
    |_| create_raw_waker(), // clone
    |_| {},                  // wake
    |_| {},           // wake_by_ref
    |_| {},                  // drop
);

async fn async_print() {
    println!("info from async_print");
}

fn main() {
    let future = async_print();
    for i in 0 .. 5 {
        println!("number : {}", i);
    }
    run_future(future);
}
```

最终成功执行：

```
number : 0
number : 1
number : 2
number : 3
number : 4
info from async_print
```



### 方法三：futures::executor::block_on

想想就知道，肯定有人已经帮我们实现好了。futures 这个库就已经把上面方法二的过程都实现了一次

文件名 Cargo.toml

```toml
[package]
name = "learn"
version = "0.1.0"
edition = "2021"

[dependencies]
futures = "0.3"
```

文件名 src/main.rs

```rust
async fn async_print() {
    println!("info from async_print");
}

fn main() {
    let future = async_print();
    for i in 0 .. 5 {
        println!("number : {}", i);
    }

    futures::executor::block_on(future);
}
```

执行结果和上面的如出一辙。

---

## 总结

在 rust 中，所有的异步默认都是惰性执行，一定不能不管。通过上面的方法就可以自己执行一个异步执行器了。

