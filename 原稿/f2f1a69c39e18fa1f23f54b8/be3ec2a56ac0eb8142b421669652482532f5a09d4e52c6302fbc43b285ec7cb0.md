最近需要一个开源的system verilog仿真器用于验证DIDE的部分功能，从而了解到了目前top 1级别的svlog开源仿真器：verilator。

这个仿真器几乎已经能够完成普通的svlog仿真的前仿。而且这个项目有着夸张的背书量：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7e1529a0c4435fa73f5356d19138ada6_720w.png" style="width: 60%;"/>
<p class="img-caption">夸张的背书</p>
</div>

所以我觉得还是很有必要了解一下基本使用的，这篇文章就简单记录一下verilator的把玩经历。

---

## 安装

如果你是Ubuntu用户，恭喜你，一行就能完成安装：

```bash
sudo apt install verilator 
```

但是作为一名成熟的程序员，要倾向于源码安装，从而体验到最新的feature，[官网的安装教程](https://verilator.org/guide/latest/install.html)也是这么推荐的：

```bash
# 一些依赖包
sudo apt install git autoconf flex bison help2man -y
# 源码安装
git clone https://github.com/verilator/verilator
cd verilator
autoconf 
./configure
make -j 8
sudo make install 
```

> 遇到问题请见官网安装教程：https://verilator.org/guide/latest/install.html

刷新一下终端，然后验证安装是否成功：

```
(base) ➜  ~ verilator
Usage:
        verilator --help
        verilator --version
        verilator --binary -j 0 [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --cc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --sc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --lint-only -Wall [source_files.v]...
```
---

## 基本使用

verilator的使用可以参考 USTC 计算机体系的课程主页的教程，我个人觉得写得还是非常好的： https://soc.ustc.edu.cn/CECS/lab2/verilator

### Linter

verilator 可以用于检测当前文件是否存在语法错误和依赖错误（总的来说，它的检测是我见过的第三方纠错器中最严格的）。

verilator 只能用于 verilog 和 systemverilog 的文件纠错。比如我们现在想要检测 `./parent.v` 这个文件的语法问题，命令如下：

```bash
$ verilator ./parent.v --lint-only -Wall -bbox-sys --bbox-unsup -DGLBL
```

输出如下：

```bash
%Warning-EOFNEWLINE: test/unofficial/parent.v:66:3: Missing newline at end of file (POSIX 3.206).
                                                  : ... Suggest add newline.
   66 | */
      |   ^
                     ... For warning description see https://verilator.org/warn/EOFNEWLINE?v=5.017
                     ... Use "/* verilator lint_off EOFNEWLINE */" and lint_on around source to disable this message.
%Error: test/unofficial/parent.v:9:10: Cannot find include file: child_1.v
    9 | `include "child_1.v"
      |          ^~~~~~~~~~~
%Error: test/unofficial/parent.v:9:10: This may be because there's no search path specified with -I<dir>.
    9 | `include "child_1.v"
      |          ^~~~~~~~~~~
        ... Looked in:
             child_1.v
             child_1.v.v
             child_1.v.sv
             obj_dir/child_1.v
             obj_dir/child_1.v.v
             obj_dir/child_1.v.sv
%Error: test/unofficial/parent.v:10:10: Cannot find include file: child_2.v
   10 | `include "child_2.v"
      |          ^~~~~~~~~~~
%Error: Exiting due to 3 error(s), 1 warning(s)
```


to be continued ...