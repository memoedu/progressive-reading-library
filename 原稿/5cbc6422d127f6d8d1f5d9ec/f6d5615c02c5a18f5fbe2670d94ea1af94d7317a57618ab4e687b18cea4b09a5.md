## 前言

最近因为一项恼人的 CV 作业，我实现了一个算法，加上之前写了一个分布式爬虫程序，想要打包成 python 的库上传到 PyPi以供后续使用。但是发现简中网上不少的关于上传 Python 包的博客都过期了，于是决定更新一下简中互联网上关于 Python 库的制作与上传，这样别人就可以通过 `pip install xxx` 的形式来下载你的库了。

这里有一份 PyPi 官方的指导链接 https://packaging.python.org/en/latest/tutorials/packaging-projects 。倘若我的博客也过期了，还希望读者可以优先参考这份。


## 从一个例子出发

下面从一个例子出发，简单讲讲如何搭建一个用于上传 PyPi 的项目。例子的项目代码在：https://github.com/LSTM-Kirigaya/localaplace 读者可以下载尝试。

项目 localaplace 的文件夹结构如下：

```
|-images
|-localaplace
    |-__init__.py
    |- ...
|-test.py
|-.gitignore
|-LICENSE
|-README.md
|-pyproject.toml
```

`localaplace` 就是我们想要上传的包的源代码文件夹，官方其实建议建立一个名为 `src` 的文件夹来放置源代码，但是只要知道如何配置，其实无所谓。

> 不同于之前，现在的 Python 不再通过 `setup.py`，而是通过 `pyproject.toml` 文件来配置打包流程。官方也不推荐再次使用 `setup.py`，想想也是有道理的，万一有作者往 `setup.py` 里面加入恶意代码呢？

### 第一步，打包

发布库的第一步永远是打包库。我们可以通过 python 的 build 模块来打包库代码，先升级一下这个工具：

```bash
pip install --upgrade build
```

我们此处使用的安装工具是 `setuptools` ，细节读者不需要搞清楚。只需要知道，当 build 运行时，它会根据 `pyproject.toml` 中的配置进行打包。因此，我们需要一个容易复用的 `pyproject.toml` 模板。

假设在上面的例子中，`.gitignore`， `images` 和 `test.py` 都是我不希望打包进入 whl 的文件或文件夹，一个正常可用的 `pyproject.toml` 的示例如下所示：

```python
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
include = ["localaplace"]
exclude = ["images", "test.py", ".gitignore"]

[project]
name = "localaplace"
version = "0.0.1"
urls = { text = "https://github.com/LSTM-Kirigaya" }
authors = [
    { name = 'lstm-kirigaya', email = 'xxxxxxxx@qq.com' }
]
description = "implement of local laplace filter algorithm"
readme = "README.md"
requires-python = ">=3.5"
license = { text = "Apache 2.0" }
keywords = [
    'local laplace filter',
    'local-laplace-filter',
    'computer vision',
    'tone mapping',
    'image process'
]
classifiers = [
    'Intended Audience :: Developers',
    'Operating System :: OS Independent',
    'Programming Language :: Python :: 3'
]
dependencies = [
    'numpy',
    'opencv-python',
    'tqdm'
]
```

编写好后，直接打包：

```bash
python -m build
```

打印信息：

```
* Creating venv isolated environment...
* Installing packages in isolated environment... (setuptools)
* Getting build dependencies for sdist...
running egg_info
writing localaplace.egg-info\PKG-INFO
...
...
removing build\bdist.win-amd64\wheel
Successfully built localaplace-0.0.1.tar.gz and localaplace-0.0.1-py3-none-any.whl
```

最后一行代表打包成功了，此时，我们可以执行 pip 来看看我们的打包结果是否可以使用：

```bash
pip install ./dist/localaplace-0.0.1-py3-none-any.whl
```

验证一下：
```bash
python -c "import localaplace as llp; print(dir(llp))"
```

输出：
```
['__all__', '__author__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__', '__version__', 'laplace', 'local_laplace_filter']
```

看来安装成功，那么打包结果也没问题，剩下来要做的事情，就是把 dist 下的 whl 文件上传到 PyPi 上了。

### 第二步 上传

我们先安装 PyPi 的上传工具 `twine`：

```bash
pip install twine
```

然后去 PyPi 上注册账号：[https://pypi.org](https://pypi.org)，为了后续不必要的麻烦，建议将 F2A 安全验证开启。具体教程网上很多，因为前一阵子 Github 强烈给用户“安利”了这项服务。

去 PyPi 生成上传用的 API Token：https://pypi.org/manage/account/token

创建 `~/.pypirc` 文件用于让 twine 读取 PyPi 的账号信息：

```
[distutils]
index-servers=
        pypi
 
[pypi]
username: __token__
password: <你的token>
```



上传：

```bash
twine upload dist/*
```

稍等片刻，输出：
```
Uploading distributions to https://upload.pypi.org/legacy/
Uploading localaplace-0.0.1-py3-none-any.whl
100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16.1/16.1 kB • 00:01 • ?
Uploading localaplace-0.0.1.tar.gz
100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 15.7/15.7 kB • 00:00 • ?

View at:
https://pypi.org/project/localaplace/0.0.1/
```

于是，我们就完成了库的上传：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-9a35c1f845947555a84c498ebc352a16_1440w.png" style="width: 60%;"/>
</div>

---

 最后就可以用 pip 直接安装了：
```python
pip install localaplace
```

 然后，体验一下一行实现多核并行局部拉普拉斯，enjoy ~
<div align=center>
<img src="https://picx.zhimg.com/80/v2-0cfbb6ab818b92546d92d725cd6964d2_1440w.png" style="width: 80%;"/>
</div>