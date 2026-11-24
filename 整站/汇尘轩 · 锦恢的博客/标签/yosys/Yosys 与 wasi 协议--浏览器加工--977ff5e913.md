

## 前言

Yosys 可以说是 IC 领域最知名的综合工具集了，形式验证，仿真，fsm检测等功能都可以执行。在原厂的支持下，Yosys 已经可以做到门路级的 RTL 电路综合。我们打算在 DIDE 中集成它。

- Yosys 使用手册 https://yosyshq.readthedocs.io/projects/yosys
- Yosys Github 官方 https://github.com/YosysHQ/yosys



---

## Yosys 基础安装

### 可执行文件

yosys 的基础编译非常简单：

```bash
git clone https://github.com/YosysHQ/yosys
cd yosys
git submodule update --init --recursive
sudo apt-get install build-essential clang lld bison flex \
	libreadline-dev gawk tcl-dev libffi-dev git \
	graphviz xdot pkg-config python3 libboost-system-dev \
	libboost-python-dev libboost-filesystem-dev zlib1g-dev
make -j 48
```

编译得到 `yosys` 可执行文件，测试是否能使用：

```bash
./yosys --version
```

除此之外，还会生成一个 `share` 文件夹，这里面都是基于 techmap 技术做到的 RTL 到门路级别电路的映射 verilog 文件，有了它们，综合器就能把抽象的 RTL 代码转换成更加具体的电路描述。这个文件夹的请记住，任何综合行为都要用到它。



###  支持 wasi 协议的 wasm

最新版本的 Yosys 丢弃了传统的 emcc，而是基于 wasi-sdk 支持 wasm，能够编译出支持 wasi 协议的 wasm。这意味着编译出的 wasm 在 wasi runtime 中可以受限地进行文件 IO 了！相比于之前只能基于 C++14 的虚拟文件进行挂载式的内存级伪IO，现在的 wasi wasm 可以更加快速地直接访问磁盘了。

:::info

wasi 只是一套介于 wasm 和 runtime 之间的协议，交互方式仍然基于 wasm，但是需要 runtime 语言根据 wasi 协议对特定的文件事件（比如读取目录下的文件名，读取文件）进行协议支持（序列化与反序列化）。python 的 `wasmtime` 和 nodejs 的 `node/wasi` 都实现了 wasi 协议，这意味着，你可以

:::

想要编译 wasi wasm 也非常简单，先安装 wasi：

```bash
cd ~/package
wget -c https://github.com/WebAssembly/wasi-sdk/releases/download/wasi-sdk-24/wasi-sdk-24.0-x86_64-linux.tar.gz
tar -xzf wasi-sdk-24.0-x86_64-linux.tar.gz

# 加入 zshrc
export WASI_SDK=$HOME/package/wasi-sdk-24.0-x86_64-linux
export WASI_SYSROOT=$WASI_SDK/share/wasi-sysroot
```

然后回到 yosys 项目后直接编译即可：

```bash
cd /path/to/yosys
make config-wasi
make -j 48
```

编译得到 `yosys.wasm`，可以直接用 `wasmtime` 执行：

```bash
# npm install wasmtime -g
wasmtime yosys.wasm
```

如果希望放入 nodejs 中运行，从而集成到自己的工具或者软件里面，更简单了，因为 nodejs 官方从 v12.0 开始就是支持 wasi 协议了，什么库都不用安装 nodejs 就可以直接运行 wasi wasm：

```javascript
// run.js
'use strict';
const fs = require('fs');
const { WASI } = require('wasi');
const { argv, env } = require('process');
const path = require('path');

const wasi = new WASI({
    version: 'preview1',
    args: [
        'yosys',
        '--version',
    ],
    env,
    preopens: {
        '/': '/'
    },
    stdin: process.stdin.fd,    // 绑定到 Node.js 的标准输入
    stdout: process.stdout.fd,  // 绑定到 Node.js 的标准输出
    stderr: process.stderr.fd,  // 绑定到 Node.js 的标准错误
});

(async () => {
    const wasmPath = path.join(__dirname, 'yosys.wasm');
    const binary = await fs.readFileSync(wasmPath);
    
    const wasm = await WebAssembly.compile(binary);
    const instance = await WebAssembly.instantiate(wasm, wasi.getImportObject());

    // 启动 WASI 实例，start 是一个同步方法，当内部执行完成后，返回 C 语言层面的返回码，比如 0 就是执行正常
    // start 启动的实例在最终最自动调用 exit 命令退出进程
    const exitCode = wasi.start(instance);
})();
```

运行：

```bash
node --no-warnings run.js
```

输出：

```
Yosys 0.48+5 (git sha1 7a362f1f7, clang++ 18.1.2-wasi-sdk -Oz)
```

如果想要在 nodejs 中模拟 stdio 的话，只需要自己创建 stream 然后绑定到 wasi 的启动项里面就好了。

---

## yosys.wasm 脚本使用

下面是一个例子，演示如何使用 `yosys.wasm` 来综合一段加法器的 RTL 级 verilog 代码到门路级 verilog 代码，并生成 netlist 的图描述结构。

文件名 `half_adder.ys`

```python
# 读取 Verilog 文件
read_verilog /root/half_adder.v

# 设置顶层模块
hierarchy -top half_adder

# 综合，启动！
synth -top half_adder

# 生成 netlist 图像描述文件
write_json /root/half_adder.json

# 生成 netlist
write_verilog -noattr /root/half_adder_synth.v
```

文件名 `half_adder.v`

```verilog
module half_adder(a, b, sum, carry);
  input a;
  input b;
  output carry;
  output sum;
  assign carry = a & b;
  assign sum = a ^ b;
endmodule
```

文件名 `run.js`

```javascript
'use strict';
const fs = require('fs');
const { WASI } = require('wasi');
const path = require('path');
const { Writable } = require('stream');

const wasi = new WASI({
    version: 'preview1',
    args: [
        'yosys',
        '-s',
        '/home/dide/project/yosys/half_adder.ys'
    ],
    env: process.env,
    preopens: {
        '/': '/',
        // 自定义映射，./ 必须是前面两个文件所在的路径
        '/root': './',
        // share 就是上文提到的编译得到的 share 文件夹
        '/share': '/path/to/share'
    },
    stdin: process.stdin.fd,    // 绑定到 Node.js 的标准输入
    stdout: process.stdout.fd,  // 绑定到 Node.js 的标准输出
    stderr: process.stderr.fd,  // 绑定到 Node.js 的标准错误
});

(async () => {
    const wasmPath = path.join(__dirname, 'yosys.wasm');
    const binary = await fs.readFileSync(wasmPath);
    
    const wasm = await WebAssembly.compile(binary);
    const instance = await WebAssembly.instantiate(wasm, wasi.getImportObject());

    // 启动 WASI 实例
    const exitCode = wasi.start(instance);

    console.log('执行结束， exit code:', exitCode);
})();


```

执行：

```bash
node --no-warnings run.js
```

会生成 `half_adder.json` 和 `half_adder_synth.v` 两个文件。通过编程渲染，就可以把 `half_adder.json` 渲染成一个综合成的可视化电路图。