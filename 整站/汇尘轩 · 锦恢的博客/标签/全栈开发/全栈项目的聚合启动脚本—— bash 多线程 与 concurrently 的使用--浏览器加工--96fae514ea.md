## 前言

在进行全栈项目开发的时候，我们总会遇到需要在开发时同时启动好几个进程的场景。比如锦恢现在在开发的一个项目就是，需要每天同时启动：SSR 前端，后端和 nginx。我以前的做法都是在 vscode 中打开 3 个集成终端，然后每个单独启动。这么做有两个坏处：

1. 很麻烦。这是显然啦，每天都要打开 3 个终端，然后分别进入其中输入指令，不是很优雅。
2. 部署不友好。分别启动若干脚本来 launch 整个系统的方法对于部署端不是非常友好，因为大部分部署设施（比如字节的 TCE 平台）都是需要指定部署脚本的。有的设施对于一个全栈项目可能只放开了运行单个入口脚本的权限，分散启动就难以在这类设施中优雅展开。

所以，这篇文章，我简单说一下如何才能更加优雅地在你的全栈项目中制作多线程的启动脚本，从而一次性启动好几个服务。

此处就拿我最近在开发的 [openmcp](https://github.com/LSTM-Kirigaya/openmcp-client) 作为例子，正常情况下，运行这个项目的开发模式需要：

新开一个终端，然后
```bash
cd renderer && npm run serve
```

再开一个终端，然后
```bash
cd service && npm run service
```

下面我们来优化这个启动过程。

---

## 方法一：bash 终端

bash 终端通过 `() &` 语法就能开启一个线程，所以对于上面的例子，可以归并到如下一个脚本中：

文件名：dev.sh
```bash
(cd renderer && npm run serve | while read -r line; do echo -e "[renderer] $line"; done) &

(cd service && npm run serve | while read -r line; do echo -e "[service] $line"; done) &

wait
```

当然，Windows上没有 bash，用 powershell 也是类似的：

```bash
Start-Job -ScriptBlock {
    cd renderer
    npm run serve | ForEach-Object { "[renderer] $_" }
}

Start-Job -ScriptBlock {
    cd service
    npm run serve | ForEach-Object { "[service] $_" }
}

Get-Job | Wait-Job | Receive-Job -Wait
```


---

## 方法二：concurrently

对于全栈项目，我相信没人没用过 nodejs 吧，毕竟 nodejs 算是公认的最优全栈开发方案了，这意味着你的开发环境中一定存着 npm 和 npx，那么使用 `npx concurrently` 就可以非常方面地同时启动多个服务线程。

```bash
npx concurrently \
    -n "renderer,service" \
    -p " {name} " \
    -c "bgGreen,bgBlue" \
    --kill-others \
    "cd renderer && npm run serve" \
    "cd service && npm run serve"
```

`--kill-others` flag 保证了我们每次 ctrl c 中断项目，concurrently 都会中止所有启动的子进程，防止 address already used 这样的常见烦恼。

通过 `c "bgGreen,bgBlue"` 我们可以往不同进程的 stdio 的前缀添加颜色，比如上面的命令启动后效果就是这样：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-27cc2a24f7be6c4e410e3b4c39a17efd_1440w.png" style="width: 100%;"/>
</div>

---

## 和 pm2 的区别

有的人可能会问了，这玩意儿和 pm2 的区别是啥。在 [超级好用的进程管理器 pm2 使用教程](https://kirigaya.cn/blog/article?seq=233) 中，我简略讲解了 pm2，一个进程管理器的使用。

pm2 更多用于创建进程任务，对于需要每天若干次的一次性打开关闭 N 个服务进程的场景，pm2 的使用并不是很方便，而 concurrently 功能非常少，设计很简单，就是用于聚合启动项的，专为全栈项目的启动定制。
