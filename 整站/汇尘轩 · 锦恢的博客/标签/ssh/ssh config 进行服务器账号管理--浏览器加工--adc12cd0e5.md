## 前言

ssh 作为最最常用的登录服务器的方式之一，有时候我们总会遇到要同时登录多个服务器的场景，这个时候就希望可以做到可以方便地管理我们的服务器。ssh 本身就提供了最为简单的管理功能。

## 使用 ~/.ssh/config 来管理

比如我现在想要登录我们实验室的服务器：

```bash
ssh -p 22 ubuntu@bacteria.tech
```

有的时候名字太长记不住？我们可以把上面的登录用户名和服务器域名/IP记录在 `~/.ssh/config` 中，我们输入 `vim ~/.ssh/config`，输入如下：

```bash
Host miracle
    HostName bacteria.tech      # 替换为你的服务器 IP 地址或者域名
    User ubuntu                 # 替换为你的用户名
    Port 22                     # 替换为你的 SSH 端口，默认为 22
    IdentityFile ~/.ssh/id_rsa  # 替换为你的私钥文件路径（如果有）
```

这样，我们想要登陆这台服务器就只需要输入：

```bash
ssh miracle
```

如果你需要登录多台服务器，这是一个很省事省心的方法。阁下可能会问，我有 Windows Terminal 呀，我有 XShell 呀，为啥还要用如此原始的方式保存服务器密码，我看了一眼我的 WT，确实如此，你说得对，但是，倘若你现在必须登录一台服务器，然后在这台远程的服务器上再登录N台有 mDNS 的内网服务器，请问阁下再不开N个 frpc 的情况下又该如何应对呢？

<div align=center>
<img src="https://pic1.zhimg.com/v2-9a6bba2cd3ed5d2b4df465a47c35e026_b.webp"/>
</div>