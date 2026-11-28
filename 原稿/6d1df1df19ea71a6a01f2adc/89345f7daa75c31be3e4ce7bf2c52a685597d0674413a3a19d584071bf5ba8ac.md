## 下载 & 安装

最近在重装服务器，整理思绪。

这三年来，都是当年我自己编写的后端程序在帮助我操作数据库，所以对于我服务器上使用的数据库——neo4j，很多东西已经忘得差不多了；加之当年我的技术力有限，关于 neo4j 的部分功能没有配起来；网上大部分关于 neo4j 的教程，几乎全部都是在 Windows 上的，而且大部分都已经过时。因此，所以昨天就抓住机会自己好好重新安装并配置了一次 neo4j，整理了一份我个人认为比较完整且通俗易懂的 Linux 上安装和配置 neo4j 的教程。

首先我们去 neo4j 官网安装，通过后方链接进入部署中心：[https://neo4j.com/deployment-center/](https://neo4j.com/deployment-center/)

然后找到 “Graph Database Self-Managed”，点击 COMMUNITY，版本建议选择 4 开头的，比如下图中的 4.4.32；下载包选择 Linux / Mac Executable （也就是 tar 文件，解压就可以直接用）。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-fbd7ce4aab34a0f04183dfe87245d5d0_1440w.jpeg" style="width: 70%;"/>
</div>

:::info why 4.4 ?
解释一下上面步骤中的几个细节，不感兴趣的朋友可以直接跳过

1. neo4j 是收费软件，COMMUNITY 也就是社区版是免费的，虽然只能创建一个用户，但是自己用用完全足够了。
2. 为啥建议选择 4 （相对于 5 算是老版本），因为新版本的 neo4j 虽然特性多，但是你不一定能找到新版本关于你使用的编程语言的驱动，也就无法使用某种编程语言驱动它了。如果你通过搜索引擎知晓了，新版本的 neo4j 有你用的技术栈对应的驱动，那么可以选择下载 5 这个版本。
:::




将下载得到的 `neo4j-community-4.4.32-unix.tar.gz` 上传到服务器，然后解压：

```bash
tar -vxf neo4j-community-4.4.32-unix.tar.gz && cd neo4j-community-4.4.32-unix
```

将目录下 `bin` 的路径加入环境变量。

如果你选择了 4 开头的版本号，则需要在服务器上安装 java 11，Ubuntu 安装非常简单：

```bash
# Ubuntu
sudo apt install openjdk-11-jdk

# MacOS
brew install openjdk@11
echo 'export PATH="/opt/homebrew/opt/openjdk@11/bin:$PATH"' >> ~/.zshrc
sudo ln -sfn /opt/homebrew/opt/openjdk@11/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-11.jdk

# Windows
wget -O openlogic-openjdk-jre-11.0.27+6-windows-x64.msi https://builds.openlogic.com/downloadJDK/openlogic-openjdk-jre/11.0.27+6/openlogic-openjdk-jre-11.0.27+6-windows-x64.msi
start openlogic-openjdk-jre-11.0.27+6-windows-x64.msi
$env:JAVA_HOME='C:\Program Files\OpenLogic\jdk-11.0.27.6-hotspot'
```

当 java 安装完成后，就可以启动 neo4j 了：

```bash
neo4j start
```

:::warn
Windows 初次启动需要运行 neo4j install-service
:::

如果没有错误产生，安装部分结束。

---

## 设置密码 & 初次登入

neo4j 的初始密码和账号为 neo4j，安装完成后，先设置初始密码（设置的密码必须不是 neo4j）

```bash
neo4j-admin set-initial-password <密码>
```

使用 `cypher` 会话工具 `cypher-shell` 登入数据库，用户名默认是 `neo4j`

```bash
cypher-shell -a localhost -u <用户名> -p <密码>
```

登入成功说明问题不大，简单写点 cypher 测试一下数据库有没有问题：

```cypher
create (n:Person {name:"锦恢"}) return n;
```

返回正常说明没问题，输入 `:exit` 退出即可。

---

## 开启 https 和 bolt 服务

为了更加方便地查看数据，并且给我们留一条后路（这就不得不提我前几天因为升级 glibc 但是没注意操作系统内核版本，动态链接库链接不匹配，导致服务器 sf 惹 =_= ，幸好我当时给 neo4j 的前端管理界面开了端口，使得我仍然可以导出数据库数据然后重装系统），很有必要开启 neo4j 的 https 使能前端的数据管理界面。

Bolt 是 neo4j 内部的一种类似于 protobuf 的二进制数据交换格式，用于进行高效数据传输的，一定需要开启。默认的 Bolt 的配置就算被注释掉了，也仍然会在 http 协议下开启 7687 端口，从而为开发者后续的配置引发困扰。所以此处建议手动开启关于 Bolt 协议的所有配置。我们开启三个协议：

- Bolt 协议：开启端口 7687 用于进行二进制数据交互（both 非加密和非加密）。开启后，通过 `curl https://<域名>:7687` 验证是否开启。
- Http 协议：开启端口 7474 用于进行普通通信。开启后，通过 `curl http://<域名 或 IP>:7474` 验证是否开启。
- Https 协议：开启端口 8002 用于进行加密通信。开启后，通过 `curl https://<域名>:8002` 验证是否开启。

```bash
# Bolt connector
dbms.connector.bolt.enabled=true
dbms.connector.bolt.tls_level=OPTIONAL # bolt 可同时应用于 http 和 https 下
dbms.connector.bolt.listen_address=0.0.0.0:7687
dbms.connector.bolt.advertised_address=0.0.0.0:7687

# HTTP Connector. There can be zero or one HTTP connectors.
dbms.connector.http.enabled=true
dbms.connector.http.listen_address=0.0.0.0:7474
dbms.connector.http.advertised_address=0.0.0.0:7474

# HTTPS Connector. There can be zero or one HTTPS connectors.
dbms.connector.https.enabled=true
dbms.connector.https.listen_address=0.0.0.0:8002
dbms.connector.https.advertised_address=0.0.0.0:8002

# Bolt SSL configuration
dbms.ssl.policy.bolt.enabled=true
dbms.ssl.policy.bolt.base_directory=<SSL 证书目录绝对路径，默认是 neo4j 安装目录的 certificate 目录>
dbms.ssl.policy.bolt.private_key=kirigaya.cn.key
dbms.ssl.policy.bolt.public_certificate=kirigaya.cn_bundle.crt
dbms.ssl.policy.bolt.client_auth=NONE

# Https SSL configuration
dbms.ssl.policy.https.enabled=true
dbms.ssl.policy.https.base_directory=<SSL 证书目录绝对路径，默认是 neo4j 安装目录的 certificate 目录>
dbms.ssl.policy.https.private_key=kirigaya.cn.key
dbms.ssl.policy.https.public_certificate=kirigaya.cn_bundle.crt
dbms.ssl.policy.https.client_auth=NONE
```

配置完成后，重启 neo4j 服务：

```bash
neo4j restart
```

重启完后，保证下面三句命令没有错误返回（可能会返回空，但是只要没有什连接 refuse 之类的错误就没问题）：

```bash
curl https://<域名>:7687
curl http://<域名 或 IP>:7474
curl https://<域名>:8002
```

然后访问 `https://<域名>:8002` ，输入账号 neo4j 和 密码，就能访问数据库的管理界面了。

---

## （选做）代理端口到路由

<域名>:8002 看起来太丑了，而且时间久了也容易记不住，我们更喜欢在 `<域名>/<你喜欢的名字>` 下访问，能不能做到呢？答案是，用 nginx 轻轻松松。

简单配置一下代理吧！打开 nginx 配置文件，找到配置的关于 https 默认端口 443 的部分，加入关于 `/<你喜欢的名字>` 路由访问的拦截，此处我选用了 `/neo4j` 这个名字。让它指向 `https://<域名>:8002/browser`：

```nginx
server {
    listen       443 ssl;
    server_name  <域名>;
    root <存放网站 index.html 的目录绝对路径>;
    index index.html;

    ssl_certificate      <pem 文件绝对路径>;
    ssl_certificate_key  <key 文件绝对路径>;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    
    # 其他配置
    # ...

    location /neo4j {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'POST,GET,OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization';
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_set_header X-Forward-For $remote_addr;
        proxy_set_header X-real-ip $remote_addr;
        proxy_pass https://<域名>:8002/browser;
    }
    
    # 其他配置
    # ...
}
```

重启 nginx

```bash
sudo nginx -s reload 
```

此时访问 `https://<域名>/neo4j` 就能开心地访问数据库管理界面了。进入后，先修改 bolt 协议地址：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-e7cd5e4a5ddfa538f672f2b97bd518ae_1440w.png" style="width: 80%;"/>
</div>

然后输入用户名 neo4j 和之前修改的密码后，就能登入了：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-1fb298591a0624532a6cfbe1eabfbbc4_1440w.jpeg" style="width: 80%;"/>
</div>

enjoy ~