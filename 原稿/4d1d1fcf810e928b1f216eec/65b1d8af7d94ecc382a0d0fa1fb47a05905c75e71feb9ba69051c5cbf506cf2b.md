## 前期准备

首先确保你的 wsl 是 wsl2，如果不是，在 powershell 中输入如下命令升级：

```bash
wsl --set-version Ubuntu 2
```

然后 wsl2 默认是不支持 systemd 的，我们需要通过 systemd 将 docker 托管在后台。通过下面的方法来开启 systemd：

```bash
sudo vim /etc/wsl.conf
```

打开的文件中写入：

```
[boot]
systemd=true
```

退出 wsl2，打开 powershell，重启 wsl2：

```bash
wsl --shutdown
```

然后再进入 wsl2.

---

## 1. 更新软件源

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```

## 2. 添加 Docker 官方 GPG key

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## 3. 添加软件源（以 Ubuntu 为例）

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 4. 安装 Docker

```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 5. 启动并测试

```bash
sudo systemctl enable docker
sudo systemctl start docker
sudo docker run hello-world
```

这一步会下载托管在平台的一个最小案例，如果网络不好会稍微花点时间，如果成功输出，那么说明没问题了，docker 安装成功，玩得愉快！