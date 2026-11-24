今天遇到一个怪事，其实是一个困扰了我很久的问题，在今天终于得到了解决。

## 问题描述

先补充一下我的问题背景：我们在使用 `ssh` 时都习惯使用公钥进行免密登录，而且大部分人的第一个公钥可能都是在Windows上生成的，当你把你的 `id_rsa` 和 `id_rsa.pub` 移动到`服务器A`（当然是 Linux服务器）上，希望通过 `服务器A` 登陆 `服务器B`（这两个服务器都在一个局域网内，而你只能访问到其中一个服务器的IP，姑且认为这个服务器是跳板机吧）时，可能会出现如下的错误：

```bash
$ ssh yifanchang@172.16.120.2                                                                                                                                                           
```

输出：

```
The authenticity of host '172.16.120.2 (172.16.120.2)' can't be established.
ECDSA key fingerprint is SHA256:nmlKHUy9hl89EKrg3i8Ado6AJgl+vj21uTAOTcEeDZM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.120.2' (ECDSA) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0664 for '/data/zhelonghuang/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/data/zhelonghuang/.ssh/id_rsa": bad permissions
```

相信不少在Windows上先生成 公钥 的同学也都遇到过类似的问题，那么要如何解决呢？

## 问题解决

其实在报错里面已经把问题说明白了，因为 `id_rsa` 文件的权限太高了，ssh 觉得不安全，所以给你中断了公钥验证。本质上这是因为Windows和Linux的文件权限管理不太一样导致的，解决方法很简单：

```bash
$ cd ~/.ssh
$ chmod 600 id_rsa
```

恢复权限后，再次输入 ssh，成功登录！