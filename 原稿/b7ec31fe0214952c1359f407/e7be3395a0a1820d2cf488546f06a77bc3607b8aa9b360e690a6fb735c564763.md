## 前言

最近好像中了乙流，主要症状为：头痛，四肢无力，身体发热量异常。既然不能去健身了，那就写写博客解闷了。

最近的实验需要一个大小为 200G 的数据集，但是这个数据集在百度网盘上，直接下载来下太占我的主机空间了，因为我最近在通 2077 和 尼尔机械纪元，而且还下了一堆 galgame，实在没空间了。所以我就想，能不能直接在数据服务器（Linux）上下载，这样不仅不需要占用本地的磁盘和网络资源，而且下载完成后使用也更方便（我们实验室的GPU服务器和数据服务器是直连的，传输速度可达 200 MB/s）。在一段时间的 google 和实践后，我找到了解决方案。

---

## 在 Linux 上下载百度网盘资源

### 下载 & 登录

这次使用到的工具为 `bypy`，是一个使用 python 编写的命令行百度网盘客户端。首先如下进行安装。

```bash
pip install bypy
```

安装完成后，我们先通过查看信息的方式完成初始化（也就是登录你的账号）：

```bash
bypy info
```

按下回车后，会出现如下的登录提示：

```
Please visit:
https://openapi.baidu.com/oauth/2.0/authorize?client_id=xxx
And authorize this app
Paste the Authorization Code here within 10 minutes.
Press [Enter] when you are done
```

点进提示的 https 链接，然后获得对应的认证码，再复制到命令行中，再回车。如果一切正常，则登陆成功，bypy 还会返回当前你的百度网盘的剩余空间：

```bash
$ bypy info
Quota: 8.005TB
Used: 237.540GB
```



### 目标下载链接入库

点开你需要下载的资源，选中文件夹或者文件后，点击“保存到网盘”

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-30811992489f79a515ef9997c7d64191_1440w.png" style="width: 70%;"/>
</div>

如果你之前的 bypy 初始化成功，那么此时能看到保存列表中多出了一个“我的应用数据”这个文件夹，点进去，你会发现 bypy 自动创建了一个名为 bypy 的文件夹。我们将我们要下载的资源保存到 `我的应用数据/bypy`，这个路径也是 bypy 的管理系统的根目录。我将 DeepLesion 这个数据集保存到了 bypy 文件夹的 data 文件夹下，这样用于区分。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-c6d180957a69c5f6e553c92a9d82e67c_1440w.png" style="width: 70%;"/>
</div>

我们回到服务器这边，使用 `bypy ls` 查看我们的数据是否成功导入：

```
bypy ls data
```

out:

```
D DeepLesion 0 2024-03-08, 22:05:07
D RegistrationDataset 0 2024-03-08, 22:05:07
D 数据集 0 2024-03-08, 22:05:07
```

可以看到， `/data` 文件夹下已经刷出了我们刚刚导入的 `DeepLesion`。入库完成。



### 下载

下载的语句非常简单，和 scp 一样，比如我需要将我们的 DeepLesion 数据集下载到本地的同名目录，使用如下语句即可：

```bash
bypy download /data/DeepLesion DeepLesion
```

<div align=center>
<img src="https://picx.zhimg.com/80/v2-75313cc05745579d9425e32fb8405361_1440w.png" style="width: 70%;"/>
</div>

玩得开心！