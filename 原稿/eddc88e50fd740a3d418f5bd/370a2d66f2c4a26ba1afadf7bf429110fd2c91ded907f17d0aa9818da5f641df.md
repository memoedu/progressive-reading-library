## stack overflow 了怎么办

在编写复杂 rust 程序时会遇到 stack overflow 的问题，也就是爆栈了，栈空间能爆炸，一般情况下都是函数调用把栈也压满了，也就是无限递归了。但是问题是，我出现 stack overflow 的函数是一个第三方库的函数，不应该出现这个问题，因此，我们需要手动扩大rust的程序的栈大小。



## 方法一：注解

如果你是在 main 函数里面运行的 rust，可以通过 recursion_limit 注解来解决这个问题，在 `main.rs` 的开头加上一句话即可。

文件名：src/main.rs

```
#![recursion_limit = "256"]
```

这样最大调用层数就为 256。

## 方法二：环境变量

作为一名做过 HPC 的架构工程师，能想到的第一个方法肯定是修改环境变量，大部分编程语言的运行时都提供了一些用于控制运行行为的环境变量。rustc 自然提供了控制栈大小的环境变量 `RUST_MIN_STACK`，单位为字节，我们只需要修改这个环境变量即可：

```
# Linux
export RUST_MIN_STACK=100000000
# Windows
$env:RUST_MIN_STACK="100000000"
```

