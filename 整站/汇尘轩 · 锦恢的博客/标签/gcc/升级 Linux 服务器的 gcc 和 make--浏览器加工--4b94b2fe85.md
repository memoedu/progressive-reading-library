## 前言

最近在编译 glibc-2.28 时发现服务器（CentOS 7） 的 gcc 和 make 版本不够（gcc version 9.3.1 & GNU Make 3.82），于是记录一下如何在服务器上升级 gcc 和 make。

---

## gnu-gcc

参考文章： [Linux升级gcc到最新版本gcc-11.2.0](https://blog.csdn.net/qq_41054313/article/details/119453611).

### 下载gcc与依赖

首先更新 yum 包环境，并下载 `bzip2`：

```bash
yum install bzip2
```

然后：下载源代码、解压进入、下载依赖包

```bash
cd ~/download
wget -c http://ftp.gnu.org/gnu/gcc/gcc-11.2.0/gcc-11.2.0.tar.gz
# 如果太慢可以尝试 wget -c https://mirrors.cloud.tencent.com/gnu/gcc/gcc-11.2.0/gcc-11.2.0.tar.gz
tar -xvf gcc-11.2.0.tar.gz && cd gcc-11.2.0
./contrib/download_prerequisites
```

下载依赖包的预期输出如下：

```
2024-03-28 20:05:14 URL:http://gcc.gnu.org/pub/gcc/infrastructure/gmp-6.1.0.tar.bz2 [2383840/2383840] -> "gmp-6.1.0.tar.bz2" [1]
2024-03-28 20:05:37 URL:http://gcc.gnu.org/pub/gcc/infrastructure/mpfr-3.1.6.tar.bz2 [1287202/1287202] -> "mpfr-3.1.6.tar.bz2" [1]
2024-03-28 20:05:52 URL:http://gcc.gnu.org/pub/gcc/infrastructure/mpc-1.0.3.tar.gz [669925/669925] -> "mpc-1.0.3.tar.gz" [1]
2024-03-28 20:06:27 URL:http://gcc.gnu.org/pub/gcc/infrastructure/isl-0.18.tar.bz2 [1658291/1658291] -> "isl-0.18.tar.bz2" [1]
gmp-6.1.0.tar.bz2: OK
mpfr-3.1.6.tar.bz2: OK
mpc-1.0.3.tar.gz: OK
isl-0.18.tar.bz2: OK
All prerequisites downloaded successfully.
```

### 配置编译指令

后面，我们准备构建目录，并配置：

```bash
mkdir build && cd build
../configure --prefix=/usr/local -enable-checking=release -enable-languages=c,c++ -disable-multilib
```

> 解释一下 `configure` 配置参数：
- `--prefix=/usr/local`：代表 `make install` 的安装目录在 `/usr/local` 
- `-enable-checking=release`：生成的编译器在编译过程中不做额外检查。
- `-enable-languages=c,c++`：生成的编译器支持 c 和 c++
- `-disable-multilib`：不生成交叉编译器。

我的构建是在生产环境的服务器上，所以就不需要交叉编译器了，需要的朋友可以去掉 `-disable-multilib`。

### 编译 gcc

然后我们直接编译，注意，编译时间非常长（双核服务器上编译需要两个小时），建议把下面这个指令开在后台：

```bash
make -j4 && make install
```

安装完成后，检查安装地点的 gcc 版本：

```bash
/usr/local/bin/gcc -v
```

输出：
```
...
gcc version 11.2.0 (GCC)
```

此时，刷新终端，如果输入 `gcc -v` 还是显示刚才的版本号，代表安装成功了。

### 替换全局 gcc 链接

如果一些读者的配置中，`/usr/bin/gcc` 才是默认使用的 gcc，那么还需要将这个软链接替换为我们刚刚编译完成的。

先把 yum 安装的默认 gcc 删除：

```bash
yum -y remove gcc g++
```

然后将我们刚刚安装的 gcc 链接到刚刚 `/usr/bin` 目录下：

```bash
ln -s /usr/local/bin/gcc /usr/bin/gcc
ln -s /usr/local/bin/g++ /usr/bin/g++
```

### 替换 gcc 的动态链接库

其实也就是更新一下我们新安装的 gcc 的 ABI 版本，先查看现在的：

```bash
strings /usr/lib64/libstdc++.so.6 | grep CXXABI
```

输出：

```
CXXABI_1.3
...
CXXABI_1.3.7
...
```

可以看到，ABI 版本号还比较低（应该要支持到 CXXABI_1.3.13），说明我们还没有更新动态库，现在删除老的，再把新的链接进去：

```bash
rm -f /usr/lib64/libstdc++.so.6
ln -s /usr/local/lib64/libstdc++.so.6.0.29 /usr/lib64/libstdc++.so.6
```

然后查看 ABI 版本：

```bash
strings /usr/lib64/libstdc++.so.6 | grep CXXABI
```

结果如下：

```
CXXABI_1.3
...
CXXABI_1.3.13
...
```

这下说明我们安装成功。

---

## gnu-make

make 的升级就简单多了，先下载源码，解压，创建

```bash
wget http://ftp.gnu.org/pub/gnu/make/make-4.4.tar.gz
tar -xvf make-4.4.tar.gz && cd make-4.4
mkdir build && cd build
```

确认默认 make 的安装地址：

```bash
which make
```

输出：

```
/usr/local/bin/make
```

这代表我们可以将 `prefix` 设置成 `/usr/local`，配置如下：

```bash
../configure --prefix=/usr/local
```

然后编译安装：

```bash
make -j 4 && make install
```

重启终端后，查看 `make` 版本：

```bash
make -v
```

输出：

```
GNU Make 4.4
...
```

说明安装成功了，如果版本号，不对，请查看目标处的 make，如果不是最新编译的，可以尝试将刚刚编译的 make 链接到这里。

然后，我们再去将默认的路径修改 make 链接：

```bash
mv /usr/bin/make /usr/bin/make.backup
ln -s /usr/local/bin/make /usr/bin/make
```

结束后，make 安装就结束了。