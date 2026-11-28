## 前言

相信每一个写 rust 的朋友在初学 Rust 时都遇到过如下的这样的写法：

```rust
#[derive(Debug)]
struct Point {
    x: f64,
    y: f64
}

fn main() {
    let point = Point {
        x: 0.4,
        y: 0.5
    };

    println!("这是一个点: {:?}", point); // 这是一个点: Point { x: 0.4, y: 0.5 }
}
```

可以看到，原本不能被print打印的结构体，加上一个 `#[derive(Debug)]` 后，就能够打印出来了。在 C/C++ 中，我们可做不到直接打印一个结构体（或是更加复杂的数据结构）的操作。

Rust 编译器是如何实现这一点的呢？这就不得不引申出今天的主题，过程宏 (proc-macro) 了。

---

## 什么是过程宏 proc-macro

在 C语言中，我们都学习过编译的过程，也就是从源代码，通过预编译把用户的宏翻译成真实代码，然后再进行后续的编译，链接 balabala。

```mermaid
graph LR

source(main.v)
precomple_source(main.v)

source -->|预编译| precomple_source
precomple_source -->|编译| ba(balabala...)
ba --> main.exe
```

所谓过程宏，指代这样一套机制。允许用户自定义【预编译插件】，这个插件是一个函数，输入 AST，输出 AST，用户可以通过这套逻辑来自定义预编译之前的 AST 要如何变化。比如之前的 Debug 这个 trait 就是 Rust 内置的一个过程宏，它能递归地去将一个结构体，实现 `std::fmt::Debug` 这个 trait 中的 `fmt` 方法，这个方法类似 python 中的魔法方法 `__str__`，允许你直接定义要如何打印一个自定义对象到【标准输出】上面。

比如对于上述的代码，它等价于下面这个不使用 Debug trait 的代码：

```rust
struct Point {
    x: f64,
    y: f64
}

impl std::fmt::Debug for Point {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        f.debug_struct("Point")
            .field("x", &self.x)
            .field("y", &self.y)
            .finish()
    }
}

fn main() {
    let point = Point {
        x: 0.4,
        y: 0.5
    };

    println!("这是一个点: {:?}", point); // 这是一个点: Point { x: 0.4, y: 0.5 }
}
```

显然，使用 Debug trait 可以让你的代码更加简单易懂，减少程序员的心智开销。而 Debug 这个过程宏则自动在预编译阶段帮你完成了上面 `impl std::fmt::Debug for Point` 这段代码的生成。



### 感觉这样的场景眼熟？

事实上，过程宏的使用方法可能会让一些读者感觉眼熟。是的，从功能性上而言，python 和 typescript 中的装饰器（decorator），或者 java 中的注解（annotation）都是可以通过过程宏实现的，过程宏提供的抽象粒度更细。

```rust
#[route(GET, "/")]
fn index() {
    println!("Hello, world!");
}
```

如果读者写过 python 中 Flask 或者 java 的 springboot，那么对于上面这个写法一定眼熟到爆炸。通过过程宏，就可以实现其他编程语言里面的【注册器】这种设计模式，使得大规模网络API开发得以在 Rust 中优雅展开。而且由于你可以操作 rust 代码的抽象语法树，得以挖掘更加有趣的细粒度操作。

有些老登还徘徊在 Oracle ProC 的温柔乡中止步不前，Rust 的过程宏不仅可以装饰一个结构体或者函数，还能直接装饰一段表达式：

```rust
let query = sql!(SELECT * FROM users WHERE id = 1);
```

:::info

当然，我承认，设计成下面这样子也很好：

```rust
let query = sql!("SELECT * FROM users WHERE id = 1");
```

反正 LSP 作者都会提刀赶来（保命。

:::



### 过程宏的分类

根据装饰对象的不同，Rust 中的过程宏分为三类，实现这些过程宏的过程宏函数的函数签名也不尽相同。

1. **派生宏（Derive Macros）**

- 通过 `#[derive(MyMacro)]` 的形式使用。

- 主要用于为结构体或枚举自动实现 trait。

- 输入是结构体或枚举的 AST（抽象语法树），输出是实现特定 trait 的代码。

- 示例：

  ```rust
  #[derive(Debug)]
  struct MyStruct {
      field: i32,
  }
  ```

  这里的 `Debug` 就是一个派生宏。

2. **属性宏（Attribute Macros）**

- 通过 `#[my_macro]` 的形式使用。

- 可以附加到函数、结构体、枚举、模块等项（item）上，用于修改或扩展这些项的行为。

- 输入是附加了宏属性的项的 AST，输出是修改后的代码。

- 示例：

  ```rust
  #[route(GET, "/")]
  fn index() {
      println!("Hello, world!");
  }
  ```

  这里的 `route` 是一个属性宏。

3. **函数式宏（Function-like Macros）**

- 通过 `my_macro!(...)` 的形式使用。

- 类似于声明宏（`macro_rules!`），但功能更强大，可以处理更复杂的语法。

- 输入是宏调用中的 token 流，输出是生成的代码。

- 示例：

  ```rust
  let sql = sql!(SELECT * FROM users WHERE id = 1);
  ```

  这里的 `sql!` 是一个函数式宏。

| 类型     | 使用形式             | 主要用途                               | 过程宏函数签名                                               |
| -------- | -------------------- | -------------------------------------- | ------------------------------------------------------------ |
| 派生宏   | `#[derive(MyMacro)]` | 自动为结构体或枚举实现 trait           | `fn my_derive_macro(input: TokenStream) -> TokenStream`      |
| 属性宏   | `#[my_macro]`        | 修改或扩展函数、结构体、模块等项的行为 | `fn my_attr_macro(attr: TokenStream, item: TokenStream) -> TokenStream` |
| 函数式宏 | `my_macro!(...)`     | 生成代码，类似于 `macro_rules!`        | `fn my_fn_macro(input: TokenStream) -> TokenStream`          |

---

## 第一个开发过程宏的小项目

### 简单解释一下为啥过程宏项目必须是一个独立的 lib

过程宏这个机制不同于其他的 rust 机制。阁下也看到了，实现过程宏的代码只是预编译器的一个插件，实现 `Debug` 的代码并不会被编译到最终的 `main.exe` 中，就好比一栋高楼建造完成后，就看不到庞大的起重机和造瘘机了。过程宏的代码会在编译阶段先编译成 dll，然后在后续编译其他代码时，rust 编译器会装载这个 dll 并使用内部的函数去预编译其他代码。

正因如此，过程宏的 build 是以一个独立的 dll （或者 so）存在的，所以实现一个过程宏的代码必须在一个独立的 rust 的 lib 类型的项目中实现，且该项目不能存在 `main`。

:::info

正因为过程宏的代码独立于最终代码，是一个需要执行的负载，所以在进行分发时，不一定要分发源代码（可能本地第一次编译非常耗时）。所以有人提出并实现了这个特性：https://internals.rust-lang.org/t/pre-rfc-sandboxed-deterministic-reproducible-efficient-wasm-compilation-of-proc-macros/19359

把过程宏编译成 wasi 协议的 wasm，这样可以做到跨平台分发还不需要用户首次执行编译源码。安全方面用 sandbox 来保证。

:::

知道这一点后，我们来尝试构建我们的第一个过程宏项目。

### 构建工作区

我习惯搞一个 `code` 文件夹，里面通常写不是项目的练手级代码： 

```bash
cd ~/code/rust
cargo new procs-macros
cd procs-macros
cargo new inner_procs --lib
```

通过上述操作，你应该能在当前工作区得到下面这样的代码：

```bash
📦procs-macros
 ┣ 📂inner_procs
 ┃ ┣ 📂src
 ┃ ┃ ┗ 📜lib.rs
 ┃ ┗ 📜Cargo.toml
 ┣ 📂src
 ┃ ┗ 📜main.rs
 ┣ 📜.gitignore
 ┣ 📜Cargo.lock
 ┗ 📜Cargo.toml
```

`inner_procs` 子项目就是我们编写过程宏的地方。我们现在给内层和外层的项目分别配置一下。

文件名：`Cargo.toml`

```toml
[package]
name = "procs-macros"
version = "0.1.0"
edition = "2021"

[dependencies]
inner_procs = { path = "./inner_procs" }
```



文件名：`inner_procs/Cargo.toml`

```toml
[package]
name = "inner_procs"
version = "0.1.0"
edition = "2021"

[lib]
proc-macro = true

[dependencies]
syn = { version = "2.0", features = ["full"] }
quote = "1.0"
```



:::info

目前各大 LSP 和编辑器对于过程宏的支持有限，如果遇到 LSP 宕机，直接重启编辑器即可。

:::



### case.1 编写你的第一个派生宏函数

okey dockey ~ 我们接下来先来初探一下派生宏。也就是 Debug 这样子的。Rust 中，派生宏是基于派生宏函数来实现，在之前我简单讲解了三大过程宏的实现函数的函数签名，派生宏的如下：

```rust
fn my_derive_macro(input: TokenStream) -> TokenStream
```

我们先给之前的结构体简单写一个派生宏吧！

文件名：`inner_procs/src/lib.rs`

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput, ItemFn};

#[proc_macro_derive(MyDebug)]
pub fn my_debug_derive(input: TokenStream) -> TokenStream {
    let ast = parse_macro_input!(input as DeriveInput);
    let name = &ast.ident;

    let gen = quote! {
        impl std::fmt::Debug for #name {
            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
                write!(f, "你好，我是结构体: {}", stringify!(#name))
            }
        }
    };

    gen.into()
}
```

通过 `quote!` 我们可以直接纂改用户的源代码，上面的 `gen.into()` 执行完成后，`quote!` 内的代码就会被复制粘贴到源代码的下方，以供后续编译。而 `fmt` 函数定义了 `{:?}` 这个表达式中，要如何打印我们的自定义结构体/枚举，此时就是用 `"你好，我是结构体: {#name}"` 作为 `{:?}` 的打印。

我们接下来在主函数中测试这个派生宏。

文件名：`src/main.rs`

```rust
use inner_procs::MyDebug;

#[derive(MyDebug)]
struct Point {
    x: f64,
    y: f64
}

fn main() {
    let point = Point {
        x: 0.4,
        y: 0.5
    };

    println!("{:?}", point);  // 你好，我是结构体: Point
}
```



让我们更进一步吧！相信上面的 case 中，阁下已经对过程宏有一个感性认知了。那么对于最常见的 Debug，我们能不能实现一个简单版本的呢？为了方便开发，我们假设用户的结构体的每一个成员都是本来就可打印的内置数据结构。比如我们上面的 Point 就满足这个条件。实现代码如下。

文件名：`inner_procs/src/lib.rs`

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput, ItemFn, Data};

#[proc_macro_derive(MyDebug)]
pub fn my_debug_derive(input: TokenStream) -> TokenStream {
    // 解析输入为 DeriveInput
    let input = parse_macro_input!(input as DeriveInput);

    // 获取结构体的名称
    let name = &input.ident;

    // 处理结构体的字段
    let fields = match &input.data {
        Data::Struct(data_struct) => {
            match &data_struct.fields {
                syn::Fields::Named(fields) => {
                    fields.named.iter().map(|f| {
                        let field_name = &f.ident;
                        quote! {
                            .field(stringify!(#field_name), &self.#field_name)
                        }
                    }).collect::<Vec<_>>()
                }
                _ => unimplemented!("exit"),
            }
        }
        _ => unimplemented!("exit"),
    };

    // 生成 impl Debug 的代码
    let gen = quote! {
        impl std::fmt::Debug for #name {
            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
                f.debug_struct(stringify!(#name))
                    #(#fields)*
                    .finish()
            }
        }
    };

    // 返回生成的代码
    gen.into()
}
```

文件名 `src/main.rs`

```rust
use inner_procs::MyDebug;

#[derive(MyDebug)]
struct Point {
    x: f64,
    y: f64
}

fn main() {
    let point = Point {
        x: 0.4,
        y: 0.5
    };

    println!("{:?}", point);  // Point { x: 0.4, y: 0.5 }
}
```



### case.2 利用属性宏实现函数计时器

属性宏可以用来修饰一个函数，py 装饰器写多的朋友对于这个设计模式一定非常熟悉，我们可以通过装饰器在函数之前的前后分别完成一些事情，其中最常用的就是计算一个单线程函数执行的时间。Rust 里面通过属性宏，也能完成这一点。

我们先来看看属性宏的函数签名：

```rust
fn my_attr_macro(attr: TokenStream, item: TokenStream) -> TokenStream
```

下面是实现一个计时器的代码，介于时间，我不会详细讲解，你们应该看得懂（逃）

文件名：`inner_procs/src/lib.rs`

```rust
#[proc_macro_attribute]
pub fn time_loger(_attr: TokenStream, item: TokenStream) -> TokenStream {
    // 解析输入的函数
    // 下面这个 input 就是代表函数本身的字面量
    let input = parse_macro_input!(item as ItemFn);
    let fn_name = &input.sig.ident;

    // 生成一个唯一的内部函数名，直接调用 #input() 会发生无限递归
    let inner_fn_name = format!("__inner_{}", fn_name);
    let inner_fn_name = syn::Ident::new(&inner_fn_name, fn_name.span());

    // 提取函数体
    let fn_block = &input.block;

    // 生成代码
    let gen = quote! {
        // 将原始函数重命名为内部函数
        fn #inner_fn_name() #fn_block

        // 生成一个新的函数，名称与原始函数相同
        fn #fn_name() {
            let start = std::time::Instant::now();
            #inner_fn_name(); // 调用内部函数
            let duration = start.elapsed();
            println!("函数 {} 执行时间: {:?}", stringify!(#fn_name), duration);
        }
    };

    gen.into()
}
```

我们在主函数中测试一下。

文件名：`src/main.rs`

```rust
use inner_procs::time_loger;

#[time_loger]
fn my_function() {
    // 休眠 2 秒
    std::thread::sleep(std::time::Duration::from_secs(2));
}

fn main() {
    my_function();
}
```

打印的信息

```
函数 my_function 执行时间: 2.0009289s
```

---

## 总结

这篇文章简单带大家了解了一下 rust 中的过程宏，算是入了一个门，修行就要靠个人了。通过过程宏，你甚至可以在 Rust 中实现任何一个 DSL（domain specific language）。比如已经有人开发了 `python!`，`html!` 等等过程宏函数来便捷开发。

期待诸位能够利用过程宏做出更多有趣的玩具。