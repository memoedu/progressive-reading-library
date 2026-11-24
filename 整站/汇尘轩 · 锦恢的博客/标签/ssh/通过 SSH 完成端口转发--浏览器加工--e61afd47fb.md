## 前言

最近在字节 seed 实习，做算法落地相关的工作，遇到一个需求，需要在一台内网服务器上完成前后端的开发，这就遇到一个问题了呀：内网起的 SPA 开发服务器要如何在我本地浏览器访问呢？

:::info
特别是 ip a 了一把后，看到除了回环外，内网云服务器只有一个 ipv6 地址，心直接凉了一半。而且这个内部基于 Merlin 平台起的 gpu 服务器是基于 Kerberos 做的鉴权才能通过一个非常长的地址 ssh 到的，这里面涉及到了太多我不了解的技术，我完全没有信心可以完成一个端口转发。
:::

我就想，算了，大不了我自己用 webview 写一个，在 vscode 里面做在线渲染，所以先启动了 vite 的开发服务器，结果，里面展示了如下的信息：

```bash
  VITE v6.1.0  ready in 979 ms

  ➜  Local:   http://localhost:5173/
  ➜  press h + enter to show help
```

我下意识地点击了 `http://localhost:5173/` 后，居然可以访问到，真是神奇：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-94515a5c7fe3a7a034030f2288ffdf75_1440w.png" style="width: 95%;"/>
</div>

然后我就很好奇，vscode 是如何做到的端口转发。

---

## ssh 端口转发

### Remote SSH 的端口转发

通过简单的搜索，我发现端口转发是 vscode 上 remote ssh 插件实现的，原来它不仅可以帮我们 ssh 到远程服务器完成开发，也可以帮我们完成端口转发。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-869dc0b624fa8a1dd90995d9c070f397_1440w.png" style="width: 95%;"/>
</div>

这就让我对背后的原理感兴趣了起来。

### ssh 的 -N 参数

在 [介绍下最近比较实用的端口转发](https://nicksxs.me/2021/11/14/%E4%BB%8B%E7%BB%8D%E4%B8%8B%E6%9C%80%E8%BF%91%E6%AF%94%E8%BE%83%E5%AE%9E%E7%94%A8%E7%9A%84%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91/) 这篇文章中，介绍了 ssh 用于进行端口转发的用法。我简单复述一下：

假设你可以通过 `ssh user1@199.199.199.199` 登录一台服务器，并且你在这台服务器上的 `127.0.0.1:3306` 有一个数据库服务，你现在想要在本地的 3307 端口访问它（这种 p2p 的方法基本用于开发），命令可以如此：

```bash
ssh -CfNg -L 3307:127.0.0.1:3306 user1@199.199.199.199
```

简单介绍下这个命令

- `-C` 表示的是压缩数据包
- `-f` 表示后台执行命令
- `-N` 是表示不执行具体命令只用于端口转发
- `-g` 表示允许远程主机连接本地转发端口
- `-L` 则是具体端口转发的映射配置

上面的命令就是将远程主机 `user1@199.199.199.199` 的 `127.0.0.1:3306` 对应转发到本地 `3307`。

### ssh config 配置转发

还可以在 `~/.ssh/config` 配置中通过 `LocalForward` 属性添加对应的转发配置

```bash
Host host1
  HostName 199.199.199.199
  User user1
  IdentityFile  /Users/user1/.ssh/id_rsa
  ServerAliveInterval 60
  LocalForward 3310 127.0.0.1:3306
```