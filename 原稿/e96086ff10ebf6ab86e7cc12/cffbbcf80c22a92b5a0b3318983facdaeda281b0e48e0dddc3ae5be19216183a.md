<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=1298093855&auto=1&height=66"
></iframe>
<p class="img-caption">你的名字</p>
</div>






## 前言

为了开发Digital-IDE，我们进行了项目重构。其中解析器这一块需要使用C++下的Antrl4构建转wasm的技术路线，但是在转wasm进行js端调用时出现了一些问题。

但是我这边需要parser进行依赖项解析，所以暂时性地使用他们做好的parser的可执行文件。由于是在Ubuntu22.04上编译的，而我本地的WSL2是Ubuntu20.04，因此，需要使用Dockerfile。简单记录一下Dockerfile的使用方法。

---

## 使用

windows上的安装就不用多说了，直接下载安装包一通点点点。重启电脑后，cd进你的朋友给你的Dockerfile的目录。

先进行镜像的编译，得到容器
```bash
docker build -f Dockerfile
```
output

```bash
[+] Building 703.6s (6/6) FINISHED
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 345B                                       0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for docker.io/library/ubuntu:22.04            3.3s
 => [1/2] FROM docker.io/library/ubuntu:22.04@sha256:4b1d0c4a2d2aaf63b371  6.5s
 => => resolve docker.io/library/ubuntu:22.04@sha256:4b1d0c4a2d2aaf63b371  0.0s
 => => sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e 1.42kB / 1.42kB  0.0s
 => => sha256:817cfe4672284dcbfee885b1a66094fd907630d610cab32 529B / 529B  0.0s
 => => sha256:a8780b506fa4eeb1d0779a3c92c8d5d3e6a656c7581 1.46kB / 1.46kB  0.0s
 => => sha256:e96e057aae67380a4ddb16c337c5c3669d97fdff6 30.43MB / 30.43MB  5.7s
 => => extracting sha256:e96e057aae67380a4ddb16c337c5c3669d97fdff69ec537f  0.7s
 => [2/2] RUN apt-get update     && apt-get install -y build-essential   691.7s
 => exporting to image                                                     2.0s
 => => exporting layers                                                    2.0s
 => => writing image sha256:add8dbf62ebfe0c14737494c1bfe598759da847ad0920  0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```


编译完成后，先查看一下你当前可用的镜像ID：

```bash
docker images -q
```

output:

```
add8dbf62ebf
```

可以看到，我当前可用的镜像ID只有一个（毕竟是在这台机子上第一次用）

复制镜像ID后，输入如下的命令进入镜像实例化的容器：

```bash
docker run -it add8dbf62ebf
```

进入后，你会发现默认的/mnt是空的：

```bash
root@3dd12c37169e:/# ls /mnt/ -a
.  ..
```

这意味在沙盒中，你无法访问宿主的资源，如果你需要将好朋友的资源在沙盒中编译或者执行，那么这样就很麻烦了。好在docker提供了-v参数来允许沙盒访问宿主的磁盘。比如我们要把E盘挂载在沙盒中，好让沙盒可以访问E盘中的资源，那么可以这么做：

```bash
docker run -v /e/:/mnt/e -it add8dbf62ebf
```

再次查看挂载磁盘，会发现，出现了E盘：

```bash
root@7e8eb5d60b6b:/# ls /mnt/ -a
.  ..  e
```






$$
\sum
$$