## 安装

源码： https://github.com/v0re/dirb

```bash
# Ubuntu 不需要安装，大部分源应该是自带的
sudo apt install libcurl4-gnutls-dev
# CentOS
yum install libcurl-devel libcurl
```

```bash
./configure
sudo make
sudo make install
```

## 使用

直接使用：

```bash
dirb http://114.214.193.88
```

指定后缀名，只展示 js 后缀名：

```bash
dirb http://114.214.193.88 -X .js
```

给扫描指定一个 100ms 的延迟，防止泛洪的产生：

```bash
dirb http://114.214.193.88 -z 100
```

直接输入 `dirb` 展示所有默认参数，最后三行会展示使用方法。

默认的词表存储在 `/usr/share/dirb/wordlists` 中，在知道了更多目标计算机的信息后，可以尝试使用不同的词表策略进行爆破。

默认使用 `common.txt` ，我们可以改为更大的词表 `big.txt` 进行爆破：

```bash
dirb http://114.214.193.88/doc /usr/share/dirb/wordlists/big.txt
```