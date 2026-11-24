## screen

GNU的`screen`工具是Linux上常用的一款终端管理工具，如果需要在后台部署一个持久化的任务，那么`screen`是一个最常用的工具。

### 基本用法

```bash
# 创建终端并进入进行交互，按下Ctrl + Alt + A可以回到主终端
$ screen -S <name>

# 重新进入 / 恢复终端
$ screen -r <name>

# 创建终端但不进入终端交互环境
$ screen -dmS <name>

# 删除指定终端
$ screen -S <name> -X quit

# 展示所有终端
$ screen -ls

# 向指定终端发送命令并执行
$ screen -x -S <name> -p 0 -X stuff "command \n"
```

### 解决无法在 screen 窗口中使用鼠标滚轮

默认在 screen 中，鼠标滚轮事件被隐藏了，但是很多时候，我们希望能够查看 screen 窗口内产生的历史信息，就必须要上滚，为了解决这个问题，可以这么做：创建 `~/.screenrc` 并填写如下内容：

```vim
termcapinfo xterm* ti@:te@
```



再打开一个新的 screen 窗口就好了。


---

## tmux

tmux也是另一款常用的虚拟终端软件，因为不太好用，我简单记录一下最基本的用法：

```bash
# 创建终端并进入
$ tmux new -t <name>

# 展示所有终端
$ tmux ls

# 重新进入 / 恢复终端
$ tmux attach -t <name>
```
需要说明的是，正在执行任务的 tmux 虚拟终端无法通过 ctrl b d 的方式退出，可以选择直接关闭外部的终端软件， tmux 的虚拟终端仍然存在。