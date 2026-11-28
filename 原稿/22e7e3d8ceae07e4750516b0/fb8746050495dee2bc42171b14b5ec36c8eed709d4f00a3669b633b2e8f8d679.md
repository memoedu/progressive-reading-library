## 前言

最近在自己部署 openmemory，被 Docker 的折腾差点气死，赶紧写篇博客来压压惊。

此处特指 snap docker 这个分支。

---

## docker 源设置

docker 是不走 http 代理的，所以就算你设置了全局 http，docker pull 照样可能会失效。最好的方法还是设置源。

snap docker 的配置文件在 `/var/snap/docker/current/config/daemon.json`，写入如下内容进行配置：


```json
{
  "log-level": "error",
  "storage-driver": "overlay2",
  "registry-mirrors": [
    "https://do.nark.eu.org",
    "https://dc.j8.work",
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://docker.nju.edu.cn"
  ]
}
```

然后重启 docker：


```bash
sudo snap restart docker
```

运行 docker info，可以看到如下信息：

```
...
 Registry Mirrors:
  https://do.nark.eu.org/
  https://dc.j8.work/
  https://docker.m.daocloud.io/
  https://dockerproxy.com/
  https://docker.mirrors.ustc.edu.cn/
  https://docker.nju.edu.cn/
...
```

说明配置成功，此时运行 docker build 应该可以成功拉取镜像。

---

## Dockerfile 配置

你以为这就结束了？怎么可能，当你运行 docker build 或者 docker compose build 的时候，每一个容器默认也不会走你设置的代理，你的 Dockerfile 可能会进行 pip install 或者 curl 某一个外网的软件包，那么这个时候，就可以考虑在对应的 Dockerfile 上面增加一些配置。

比如 [openmemory 的 api 层](https://github.com/mem0ai/mem0/blob/main/openmemory/api/Dockerfile) 的 Dockerfile 如下：

```
FROM python:3.12-slim
LABEL org.opencontainers.image.name="mem0/openmemory-mcp"

WORKDIR /usr/src/openmemory

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY config.json .
COPY . .

EXPOSE 8765
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8765"]
```

这段 docker 在执行时可能会运行 pip install 超时或者找不到对应的库时，你可以考虑在开头加上下面这段：

```
# 使用国内 pip 源
RUN mkdir -p /root/.pip \
  && echo "[global]\nindex-url = https://pypi.tuna.tsinghua.edu.cn/simple" > /root/.pip/pip.conf

# 设置代理环境变量（从 build args 传入）
ENV HTTP_PROXY=http://localhost:7890
ENV HTTPS_PROXY=http://localhost:7890
```

这样就没问题了。