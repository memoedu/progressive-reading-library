## 前言

最近在把玩树莓派，作为纯净 ssh 用户，自然需要一个基于 bash 缓冲区的好用的代码编辑器， vscode 起了 LSP 有点大， vim 又太简单，neovim 刚刚好。

预备工作，我们需要先去 nodejs 官网找到 Debian 上的 nodejs 安装包，然后加入环境变量，这就不说了。

其次，就是安装 neovim 本体了。我们都知道，树莓派的架构为 arm64 Debian，neovim 预编译的 镜像文件并不能在这种架构中执行，虽然你可以这么安装 Debian 中的 neovim：

```bash
sudo apt install neovim
```

但是这么安装得到的 `neovim` 版本太老，很多插件用不了。（你在安装插件时会遇到无数完全搜不到的错误）

<div align=center>
<img src="https://pica.zhimg.com/80/v2-751e2f395c2046f378dfcada944f02bf_1440w.jpeg" style="width: 30%;"/>
</div>

那么，剩下来只剩一个路径了：自己编译！树莓派 B5 的性能非常不错，4 核 8 G 比我的云服务器还高了，编译一个软件自然简单。

当然，我已经编译好了一份上传到了网盘中

```
链接：https://pan.baidu.com/s/1ogBXzsYkimZSQTUVhGKUow?pwd=1234
提取码：1234
```

---

## cmake 启动！

接下来，我们来编译 neovim

```bash
cd ~/Downloads
git clone https://github.com/neovim/neovim
cd neovim
sudo apt install cmake gettext
make CMAKE_BUILD_TYPE=Release -j4
```

最后一步需要从 github 上下载依赖，如果你的网络比较差，可能会一直失败，不要灰心，重新运行即可。

编译结束后，我们可以生成安装包，并安装了

```bash
cd build && cpack -G DEB
sudo dpkg -i nvim-linux64.deb
```

如果你之前安装过了 neovim，此处安装会失败，你需要先卸载原本的 neovim ：

```bash
sudo apt remove neovim neovim-runtime
```

---

## 安装插件

接下来就是安装 neovim 插件了，在我的往期教程里面，我做了一个整合包，即开即用：

```bash
git clone https://github.com/LSTM-Kirigaya/neovim-config ~/.config/nvim
nvim
```

首次运行便会开始安装，如果网不好，多试几次就行。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-a469f640d23e6ab5c7484f5af93fa118_1440w.png" style="width: 60%;"/>
</div>

如果遇到错误，说什么什么已经存在，那就去 lazy 下删个干净：

```bash
cd ~/.local/share/nvim/lazy
rm -rf *
```

然后，就可以愉快地在 neovim 上打代码了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e2e3812f06be9b78e1926c0df4e358ea_1440w.png" style="width: 60%;"/>
</div>



比如用 python 写个 MJPG 给树莓派的摄像头往 内网 去推流：

```bash
npm install -g pyright
```

然后进入工作目录，输入 `nvim`：

<div align=center>
<img src="https://pica.zhimg.com/80/v2-e6afccaeeeb7694b6eaee5472f113000_1440w.png" style="width: 60%;"/>
</div>

完美，开玩！