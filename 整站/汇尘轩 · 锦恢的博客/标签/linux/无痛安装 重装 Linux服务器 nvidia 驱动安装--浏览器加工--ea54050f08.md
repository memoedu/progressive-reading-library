最近apt upgrade一下把哥们服务器的nvidia驱动搞炸了。这不是我第一次遇到这种情况，但是之前遇到这种情况都有其他的工程师帮忙解决，这次因为任务比较紧，只能靠我们自己。而且我之前其实也遇到过这种情况，于是整理一个较为通用的解决方法。

需要注意，安装驱动需要 sudo 权限，没有的同学先去向管理员要。

---

## 卸载旧驱动

运行如下脚本：

```bash
sudo apt-get --purge remove "*cublas*" "cuda*" -y
sudo apt-get --purge remove "*nvidia*" -y
sudo apt-get --purge remove "nvidia-*" -y
sudo apt-get purge "nvidia*" -y
sudo apt-get purge "libnvidia*" -y
```

## 查看本机显卡驱动版本号

```bash
$ cat /proc/driver/nvidia/version
```
输入如下：
```
NVRM version: NVIDIA UNIX x86_64 Kernel Module  515.105.01  Tue May 30 05:11:37 UTC 2023
GCC version:  gcc version 11.4.0 (Ubuntu 11.4.0-1ubuntu1~22.04)
```

可以看到，我的驱动版本号为 `515.105.01` ，记住`515`这个大版本号。说明我们的驱动需要安装 `515` 这个版本，也就是名为 `nvidia-driver-515` 的软件包。

## 查看目前驱动

```bash
$ dpkg --list | grep libnvidia
```
```
ii  libnvidia-cfg1-525:amd64                   525.125.06-0ubuntu0.22.04.1             amd64        NVIDIA binary OpenGL/GLX configuration library
ii  libnvidia-common-525                       525.125.06-0ubuntu0.22.04.1             all          ...
...
```

如果存在版本低于 `515` 的驱动，那么想办法卸载。

## 安装新的驱动程序

```bash
$ sudo apt-get install nvidia-driver-515 nvidia-settings nvidia-prime -y
```
等待安装完毕。

## 重启
安装驱动后，请和同学们商量好后，重启服务器：

```bash
$ sudo reboot
```

等待重启后，再次运行 `nvidia-smi`，就可以正常使用了：
```bash
$ nvidia-smi
Mon Sep 11 12:43:37 2023
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.125.06   Driver Version: 525.125.06   CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA RTX A6000    Off  | 00000000:31:00.0 Off |                  Off |
| 30%   34C    P8    26W / 300W |      6MiB / 49140MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA RTX A6000    Off  | 00000000:4B:00.0 Off |                  Off |
| 30%   32C    P8    29W / 300W |      6MiB / 49140MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   2  NVIDIA RTX A6000    Off  | 00000000:B1:00.0 Off |                  Off |
| 30%   30C    P8    33W / 300W |      6MiB / 49140MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   3  NVIDIA RTX A6000    Off  | 00000000:CA:00.0 Off |                  Off |
| 30%   32C    P8    29W / 300W |      6MiB / 49140MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      3098      G   /usr/lib/xorg/Xorg                  4MiB |
|    1   N/A  N/A      3098      G   /usr/lib/xorg/Xorg                  4MiB |
|    2   N/A  N/A      3098      G   /usr/lib/xorg/Xorg                  4MiB |
|    3   N/A  N/A      3098      G   /usr/lib/xorg/Xorg                  4MiB |
+-----------------------------------------------------------------------------+
```

最后奉劝诸位，上了新的服务器后，不要随意使用 `sudo apt upgrade`。