## 问题背景
`LD_LIBRARY_PATH` 虽然是一个小学知识点，但是我仍然简单记录一下，今天帮朋友修服务器环境时遇到了这个问题。在她的服务器上（记作 `user she`）输入 `git clone`，出现了 ffi.so 版本不对的问题。我在相同服务器的其他用户（记作 `user common`）上使用 `git clone` 却可以正常使用。既然是动态链接库出了问题，我下意识查看了两个用户下的 `LD_LIBRARY_PATH`:

```bash
# user she
$ echo $LD_LIBRARY_PATH
xxx/anaconda/lib:xxx/anaconda/cuda/lib

# user common
$ echo $LD_LIBRARY_PATH

```
可以看到，`user common` 的 `LD_LIBRARY_PATH` 是空的，那么解决方案也就很粗暴啦：

```bash
 # user she
$ export LD_LIBRARY_PATH=""
$ git clone xxx
success!
```

瞬间解决了问题，我们可以快乐去做数学题了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-244739e67dd9d70fbea1e4a7e48300da_1440w.jpeg" style="width: 400px;"/>
</div>

最终成功解决了问题，想要知道为什么这样就能解决问题，就不得不了解一下 `LD_LIBRARY_PATH` 这个环境变量了。

---


## 什么是 `LD_LIBRARY_PATH`

想要知道 `LD_LIBRARY_PATH` 是什么，首先需要知道 Linux 上执行一个程序的过程。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d0e1772dfef7d33fc0edeec302565d86_1440w.jpeg" style="width: 400px;"/>
</div>


许多程序的执行都会在被载入内存并执行到关键函数时调取对应函数实现的动态链接库（.so 文件）。默认情况下 Linux 会从默认路径去找这些 so 文件（默认路径看下一个 section），如果你的软件是正常安装的，那么对应的 so 文件应该静静躺在这些默认搜索路径上。

但是在一些特殊的情况下，比如你的环境一团乱麻，或者在使用公共服务器（此时假设你对默认搜索路径没有写权限时），我们有时候总希望 Linux 可以搜一搜别的路径，这个时候你就可以设置环境变量 `LD_LIBRARY_PATH`。

`LD_LIBRARY_PATH` 是 Linux 用户的一个环境变量，其中用 `:` 作为分割记录了一组用户的自定义路径，当在 Linux 上执行程序时， Linux 会**优先**从这些路径中去找可执行程序需要的 so 文件。

---

## 补充：Linux 和 GCC 默认的动态链接库搜索路径

Linux 模型的动态链接库搜索路径为：

- `/lib`
- `/usr/lib`
- `/usr/local/lib`

> 就算添加了环境变量 `LD_LIBRARY_PATH`， Linux 仍然会去这些默认路径中寻找，但是 `LD_LIBRARY_PATH` 中的动态链接库拥有被调度的更高的优先级。比如，如果你设置的 `LD_LIBRARY_PATH` 中有一个 `hello.so`，恰巧在 `/usr/lib` 中也存在 `hello.so`，那么 Linux 会选择前者作为调用程序调取的真实动态链接库。

但是不同的服务器情况不太一样，我们有时可以通过 gcc 编译器来查看当前服务器的动态链接库的可能的默认搜索路径：

```bash
$ echo "" |  gcc -v -x c -E -
```

out:

```
...
...
gcc version 9.4.0 (Ubuntu 9.4.0-1ubuntu1~20.04.2)
COLLECT_GCC_OPTIONS='-v' '-E' '-mtune=generic' '-march=x86-64'
 /usr/lib/gcc/x86_64-linux-gnu/9/cc1 -E -quiet -v -imultiarch x86_64-linux-gnu - -mtune=generic -march=x86-64 -fasynchronous-unwind-tables -fstack-protector-strong -Wformat -Wformat-security -fstack-clash-protection -fcf-protection
ignoring nonexistent directory "/usr/local/include/x86_64-linux-gnu"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/9/include-fixed"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/9/../../../../x86_64-linux-gnu/include"
#include "..." search starts here:
#include <...> search starts here:
 /usr/lib/gcc/x86_64-linux-gnu/9/include
 /usr/local/include
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
# 1 "<stdin>"
# 1 "<built-in>"
# 1 "<command-line>"
# 31 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 32 "<command-line>" 2
# 1 "<stdin>"
COMPILER_PATH=/usr/lib/gcc/x86_64-linux-gnu/9/:/usr/lib/gcc/x86_64-linux-gnu/9/:/usr/lib/gcc/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/9/:/usr/lib/gcc/x86_64-linux-gnu/
LIBRARY_PATH=/usr/lib/gcc/x86_64-linux-gnu/9/:/usr/lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/9/../../../../lib/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/9/../../../:/lib/:/usr/lib/
COLLECT_GCC_OPTIONS='-v' '-E' '-mtune=generic' '-march=x86-64'
```

其中 `#include <...> search starts here` 下的四行代表了 gcc 默认找头文件的地方：
- /usr/lib/gcc/x86_64-linux-gnu/9/include
- /usr/local/include
- /usr/include/x86_64-linux-gnu
- /usr/include

`LIBRARY_PATH` 中用 : 分割的若干路径就是 gcc 找 so 的地方：
- /usr/lib/gcc/x86_64-linux-gnu/9/
- /usr/lib/gcc/x86_64-linux-gnu/9/../../../x86_64-linux-gnu/
- /usr/lib/gcc/x86_64-linux-gnu/9/../../../../lib/
- /lib/x86_64-linux-gnu/
- /lib/../lib/
- /usr/lib/x86_64-linux-gnu/
- /usr/lib/../lib/
- /usr/lib/gcc/x86_64-linux-gnu/9/../../../
- /lib/
- /usr/lib/

有时，这也是一种判断当前环境中动态链接库搜索路径的方式。