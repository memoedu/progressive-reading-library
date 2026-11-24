## 前言

最近要给服务器续费了，顺便域名和SSL证书也要跟着搞了，发现腾讯云的证书居然只能免费申请3个月了，遂放弃腾讯云免费证书，转移阵地到 let's encrtypt。安装 let's encrtypt 的一个非常好用的客户端就是 certbot 了，纯命令行操作就能更新服务器的证书，而且还支持 nginx 相关配置的自动更新，方便得很呐！接下里，我就要进入服务器，更新的我的 SSL 证书了。

---

## 1. 安装系统依赖

首先进入服务器（Ubuntu 或者 Debian），安装基本的组件：

```bash
sudo apt update
sudo apt install python3 python3-venv libaugeas0
```

:::info

如果是 CentOS 或者 Fedora，输入：

```bash
sudo dnf install python3 augeas-libs
```

然后本机 python 最好使用最新的。 2025 年 certbot 将会失去对 python 3.8 的支持。

:::



## 2. 创建虚拟环境

为了不和主环境冲突，创建一个虚拟环境来安装 certbot

```bash
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```



## 3. 安装 certbot

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```



## 4. 创建 certbot 的快速访问

为了在全局都能直接访问到 certbot 的命令行软件，在 `/usr/bin` 下创建 certbot 的快捷方式

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```



## 5. 开始配置！

接下来就是愉快的配置时间了，启动 certbot 命令行客户端

```bash
sudo certbot --nginx
```

如果你的 nginx 配置文件不是安装在 `/etc/nginx/`，而是在 `/usr/local/nginx/`（举个例子），可以这么做

```bash
sudo certbot --nginx --nginx-server-root=/usr/local/nginx/conf
```

certbot 就会根据扫描到的 nginx 配置文件自动得到所有的域名并为你进行自动化配置，执行完这一步，SSL 证书就能自动配置完成！

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e81ee8d3d781cbc9e343a57b006cceb6_720w.jpeg" style="width: 90%;"/>
</div>

## 更新证书

如果希望更新证书，可以这么做：

```bash
sudo certbot renew
```

---

## 参考资料

https://certbot.eff.org/instructions?ws=nginx&os=pip