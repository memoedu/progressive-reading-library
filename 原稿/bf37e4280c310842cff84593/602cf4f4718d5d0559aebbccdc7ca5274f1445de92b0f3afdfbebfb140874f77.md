## pip 源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```


## uv

1. `~/.uv.toml`
    ```toml
    [[index]]
    url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
    default = true
    ```

2. 当前项目 `pyproject.toml`
    ```toml
    [[tool.uv.index]]
    url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
    default = true
    
    [tool.uv.pip]
    index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
    ```


## npm 源

```bash
npm config set registry https://registry.npmmirror.com
```

## go 源

```bash
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```
---

## Ubuntu 20.04 focal 源

更多的版本和配置的对应关系可以查看： [LUG's repo file generator](https://mirrors.ustc.edu.cn/repogen/)

<iframe src="https://mirrors.ustc.edu.cn/repogen/" width="100%" height=600px></iframe>

下面则列举一些常用的。

### 阿里源

```bash
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

### 清华源

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse multiverse
```