## 前言

最近需要启动一个批次下载程序，基本的程序我都写好了，但是因为网速的原因，进展很不顺利，恰巧实验室有几个好朋友的电脑网速很快（估计实验室的网线是穿行布局的），所以如果可以借助他们的电脑来下载，速度应该会快很多。所以就有了这篇文章来讲讲如何连接你好友的Windows电脑。

---

## 正文

> windows 上配置 SSH 服务端非常简单

首先争得你的好友的同意拥有TA的电脑的使用权。来到你的好友的Windows电脑，以管理员权限打开 windows terminal (win + x + a) 。

### 检查 OpenSSH 安装情况

首先查看当前openssh 安装情况：

```bash
$ Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

out:

```
Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

可以看到，这台 windows 电脑只有 SSH 的客户端，但是没有服务端，这意味着，我们只能在这台电脑上通过 SSH 连接其他的电脑，其他电脑却无法找到它。因此，我们接下来需要安装 SSH 服务器，

### 安装 SSH 服务器

安装 SSH 服务器的方法很简单：

```bash
$ Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

等待片刻后，输出：

```
Path          :
Online        : True
RestartNeeded : False
```

可以看到，SSH 服务端安装成功。

### 启动 SSH 服务器

然后我们启动 SSH 服务器：

```bash
$ Start-Service sshd
```

后续每次连接你的朋友的Windows电脑，都需要先启动 SSH 服务，如果你觉得麻烦，可以通过如下指令开机自动启动 SSH 服务器：

```bash
$ Set-Service -Name sshd -StartupType 'Automatic'
```

### 检查防火墙
启动完成后，我们查看一下防火墙是否让行了 SSH 服务器：

```bash
$ Get-NetFirewallRule -Name *ssh* | findstr Enable
```
out:
```
Enabled                       : True
```

Enabled 参数是 True，说明让行成功。

最后，记录一下你的好友的电脑IP：

```bash
$ ipconfig | findstr IPv4
```
out

```
   IPv4 ?? . . . . . . . . . . . . : 172.16.46.199
```

172.16.46.199 就是 <你的好友电脑的IP>

### 连接你的好友的 Windows 主机

现在回到你的电脑，打开终端，输入：

```
$ ssh <你的好友电脑用户名>@<你的好友电脑的IP>
```
回车后，如果有密码，则是你的好友电脑的开机密码，让TA来输入即可。
