## wasm 太大了

之前在 yosys 下提了一个 issue：https://github.com/YosysHQ/yosys/issues/4822

大概意思就是，编译出的 yosys.wasm 特别大，之前只有 10+ MB，现在编译出来有 240多MB，严重影响网络分发。

在项目成员 [whitequark](https://github.com/whitequark) 的帮助下，我找到了解决了这个问题的方法。下面是解决思路。

---

## 问题原因

首先先查看 wasm 的 section headers：

```bash
wasm-objdump -h yosys.wasm
```

输出：

```
yosys.wasm:     file format wasm 0x1
module name: <yosys.wasm>

Sections:

     Type start=0x0000000b end=0x000002e5 (size=0x000002da) count: 78
   Import start=0x000002e8 end=0x0000056f (size=0x00000287) count: 17
 Function start=0x00000573 end=0x00004bc1 (size=0x0000464e) count: 17995
    Table start=0x00004bc3 end=0x00004bca (size=0x00000007) count: 1
   Memory start=0x00004bcc end=0x00004bcf (size=0x00000003) count: 1
   Global start=0x00004bd2 end=0x00004ec4 (size=0x000002f2) count: 108
   Export start=0x00004ec6 end=0x00004ed9 (size=0x00000013) count: 2
     Elem start=0x00004edd end=0x0000953d (size=0x00004660) count: 1
     Code start=0x00009542 end=0x00eb30b1 (size=0x00ea9b6f) count: 17995
     Data start=0x00eb30b5 end=0x00f7ee25 (size=0x000cbd70) count: 2
   Custom start=0x00f7ee2a end=0x031374f4 (size=0x021b86ca) ".debug_loc"
   Custom start=0x031374f8 end=0x03220f35 (size=0x000e9a3d) ".debug_abbrev"
   Custom start=0x03220f3a end=0x09ca796e (size=0x06a86a34) ".debug_info"
   Custom start=0x09ca7973 end=0x0c1c64b7 (size=0x0251eb44) ".debug_str"
   Custom start=0x0c1c64bc end=0x0dea8ced (size=0x01ce2831) ".debug_line"
   Custom start=0x0dea8cf2 end=0x0ed5ffb0 (size=0x00eb72be) ".debug_ranges"
   Custom start=0x0ed5ffb5 end=0x0f10d094 (size=0x003ad0df) "name"
   Custom start=0x0f10d097 end=0x0f10d135 (size=0x0000009e) "producers"
   Custom start=0x0f10d137 end=0x0f10d170 (size=0x00000039) "target_features"
```

whitequark 的意思是，从上表可以看出， wasm 的主要大小都来源于 debug 信息，只需要在编译时输入 `--strip-all` 参数即可。

---

## 解决方法

既然是 strip 掉 debug 信息，那么 [wabt](https://github.com/WebAssembly/wabt) 工具集的 `wasm-strip` 也能凑效。于是从 https://github.com/WebAssembly/wabt 的 release 下载工具后，通过下面的命令就可以直接把 `yosys.wasm` 瘦身到只有 27MB

```bash
wasm-strip yosys.wasm
du yosys.wasm -h
```

输出

```
27M     yosys.wasm
```

这下舒服了~