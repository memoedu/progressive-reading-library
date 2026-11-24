最近刚好需要给我的私人服务器提个速，简单记录一下如何使用Clash for Linux达到这个目的，让你的访问github直接起飞。

> 如果你的服务器不是私有的，而是云服务器或者实验室的公共服务器，我不建议你这么做，容易被封IP。我的做了隧道，相对而言比较安全。

## 什么是Clash For Linux ?

可以在服务器上使用的魔法。

## 安装教程

运行如下脚本：

```bash
mkdir -p ~/package && cd ~/package
git clone https://github.com/wanhebin/clash-for-linux
# 网络不行也可以 wget -c https://kirigaya.cn/files/linux_tools/clash-for-linux.zip
cd clash-for-linux
vim .env
```

进入后将`CLASH_URL`修改为你的运营商提供的Clash订阅地址，另外一个`CLASH_SECRET`不需要写。

然后下载订阅地址：

```bash
bash start.sh
```

后续操作确保你拥有服务器的最高权限，否则请参考文章末尾的做法。

启动clash客户端：

```bash
source /etc/profile.d/clash.sh
```

开启规则代理：

```bash
proxy_on
```

验证是否代理成功：

```bash
curl https://www.google.com -I
```

如果输出信息开头为如下信息（code为200），则说明代理成功：

```bash
HTTP/1.1 200 Connection established

HTTP/2 200
...
...
```

为了方便使用，可以在`~/.zshrc`或者`~/.bashrc`中加入如下命令来在每个终端开启时自动开启代理：

```bash
source /etc/profile.d/clash.sh
proxy_on
```

每次运行完成上述命令，都会出现：

```bash
[√] 已开启代理
```

说明常规代理成功。

---

## 如果没有 sudo 权限怎么办？

感谢 [如果没有权限生成clash.sh文件请看这里 #72](https://github.com/wanhebin/clash-for-linux/issues/72) 给出的解决方法：

> 对于没有管理员权限的用户而言，无法在/etc文件下生成文件clash.sh，这就导致没法载入新的环境变量并调用proxy_on函数。 一个补救方法是在clash-for-linux所在的当前文件夹下自己建立clash.sh文件，并且在文件里输入以下内容：

```bash
function proxy_on() {
    export http_proxy=http://127.0.0.1:7890
    export https_proxy=http://127.0.0.1:7890
    export no_proxy=127.0.0.1,localhost
    echo -e "\033[32m[√] 已开启代理\033[0m"
}

function proxy_off(){
    unset http_proxy
    unset https_proxy
    unset no_proxy
    echo -e "\033[31m[×] 已关闭代理\033[0m"
}
```

source完上述的代码后，直接输入 `proxy_on` 开启代理。