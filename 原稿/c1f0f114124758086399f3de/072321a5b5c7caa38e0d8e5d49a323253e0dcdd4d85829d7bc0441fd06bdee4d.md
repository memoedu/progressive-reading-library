## wasm是什么？

其实玩`wasm`已经很长一段时间了，简单介绍一下，`wasm`是web assembly的缩写，是一种新的前端负载技术，使用wasm写的程序可以以更快的速度运行在浏览器或者基于浏览器内核开发的软件上。

> 因为`wasm`的运行时没有GC，也没有类型检查等工作。

通常情况下`wasm`不会被单独拿来使用，而是会作为负载的模块被JavaScript所调用。这一点类似于使用C语言编写的Python模块。

作为一种新型的汇编语言，我们并不会直接的去开发`wasm`，而是会使用一种常用的高级语言作为源语言去编译得到`wasm`。目前最常用的`wasm`编译器由`Emscripten`这个组织所开发emcc套件，支持C语言和Rust作为源语言。

官方给出的emcc安装教程：[https://emscripten.org/docs/getting_started/downloads.html](https://emscripten.org/docs/getting_started/downloads.html)

而作为Windows用户，我们经常会在 WSL2下完成 emcc工具链的部署，并且通过 WSL进行交叉编译来得到`wasm`文件。所以下面简单讲一讲如何在WSL2上安装与激活emcc。

---

## 安装

直接下载emscripten的项目：

```bash
git clone https://github.com/emscripten-core/emsdk.git
```

把下载的emsdk文件夹路径注册为`EMCC_HOME`：
```bash
# .bashrc或者.zshrc
export EMCC_HOME=<emsdk文件夹路径，包含emsdk>
```

安装工具链（包括但不限于nodejs解释器、gcc套件、emcc套件）：
```bash
cd emsdk
./emsdk install latest
```

> 所以本地运行有nodejs的朋友不建议全局激活emcc套件，因为emcc自带的nodejs会覆盖原本的node解释器。

然后初始化一下emcc工具链，生成一些必要的配置文件：
```bash
sudo $EMCC_HOME/emsdk activate latest
```

最后，激活emcc：
```bash
source $EMCC_HOME/emsdk_env.sh
```

---

## 验证

上面的命令跑完后，运行以下的指令验证是否安装成功：

跑自带的node
```bash
node
```

跑emcc：
```bash
emcc
```


---

## 激活

每次运行都使用如下语句进行激活：
```bash
source $EMCC_HOME/emsdk_env.sh
```

如果你觉得麻烦，也可以把上面的语句放在bashrc末尾。