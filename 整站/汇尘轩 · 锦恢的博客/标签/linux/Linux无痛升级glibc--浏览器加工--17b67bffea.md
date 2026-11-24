最近在准备搭建一个私有的签名服务器，正好趁这个机会升级一下我的服务器的glibc版本。简单翻了一下网上的博客，果然，依托答辩，所以我就简单写一下如何用最短的时间优雅地升级glibc。

我使用的是Ubuntu，Centos应该会更加省事。

---

## step.1 查看glibc版本

首先，先看一下你的glibc版本：
```bash
strings /lib64/libc.so.6 | grep GLIBC_
```

:::info
如果是 arm 上的 Ubuntu，则输入：

```bash
strings /lib/aarch64-linux-gnu/libc.so.6 | grep GLIBC_
```

可能有人的服务器并没有链接过libc.so.6，运行上面的指令会报错找不到“/lib64/libc.so.6”，那就先链接一下：
```bash
sudo ln -s /lib/x86_64-linux-gnu/libc.so.6 /lib64/libc.so.6
```

:::



然后再运行第一行的命令就可以了：

```bash
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_PRIVATE
```

看来，版本确实有点老了，像现在的话，应该到`GLIBC_2.30`才对，所以我们升级一下好了。

---


## step.2 升级glibc

直接上命令不废话，其中，第二行wget的链接看你的版本，我下载的是2.19，如果你要下载2.20，就把2.19改成2.20就行：
```bash
mkdir -p ~/download/glibc && cd ~/download/glibc
wget --no-check-certificate https://ftp.gnu.org/gnu/glibc/glibc-2.19.tar.gz
tar -xvf glibc-2.19.tar.gz && cd glibc-2.19
mkdir build && cd build
sudo ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
sudo make && sudo make install
```
中间如果错误，可能是软件包没有及时更新，去`sudo apt install`即可。

安装完成后，检查一下版本号：
```bash
strings /lib64/libc.so.6 | grep GLIBC_
```

看到输入如下：
```bash
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_2.14
GLIBC_2.15
GLIBC_2.16
GLIBC_2.17
GLIBC_2.18
GLIBC_2.22
GLIBC_2.23
GLIBC_2.24
GLIBC_2.25
GLIBC_2.26
GLIBC_2.27
GLIBC_2.28
GLIBC_2.29
GLIBC_2.30
GLIBC_PRIVATE
```
bingo! 打星际去了。

---


## 拓展


如果您使用的 anaconda，那么还有一个方法可以更加方便地解决 glibc 版本的问题：

```bash
conda install -c conda-forge libstdcxx-ng=12
```

这一句可以让你当前的 bash 运行时的 glibc 版本直接支持到 3.4.30，具体请看下面这个帖子： ['GLIBCXX_3.4.30' not found for librosa in conda virtual environment (after trying out a lot of solutions)?](https://stackoverflow.com/questions/72540359/glibcxx-3-4-30-not-found-for-librosa-in-conda-virtual-environment-after-tryin)