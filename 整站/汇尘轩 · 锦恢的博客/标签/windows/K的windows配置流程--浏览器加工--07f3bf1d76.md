<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=22722803&auto=1&height=66"
></iframe>
</div>

> There is no end though there is a start in space. ---Infinity.
It has own power, it ruins, and it goes though there is a start also in the star. ---Finite.
Only the person who was wisdom can read the most foolish one from the history.
The fish that lives in the sea doesn't know the world in the land.
It also ruins and goes if they have wisdom.

由于经常换电脑，记录一下简单的电脑配置流程。按照这个顺序配置即可。

## Windows
必须是win11，因为我需要这个版本上面的一些特性，或是功能性的，或是装饰性的。

### 一、登录微软账号

1. 同步部分OS配置
2. 同步edge
3. 同步应用商店

### 二、转移基本的静态文件

1. 所有的图片（壁纸+图标+光标）
2. 所有的额外字体文件（Jetbrain+Nerd等宽字体+几款我喜欢的中文字体）
3. `.ssh`文件夹（用于登录服务器）
4. windows terminal 的配置文件`setting.json`，并替换本机windows terminal配置文件，将部分的图标路径进行修改。

### 三、下载应用软件

1. 微软应用商店：Dolby Access, ScreenMix
2. 下载前往魔法地址获取SSR订阅，下载并配置CFW。
3. 网上下载：Google Chrome, FireFox, Tim, 微信, 飞书, 百度网盘, bandizip, postman, office套件, 向日葵




### 四、配置开发环境

1. 删除和python默认起冲突的可执行文件。使用管理员权限打开powershell，输入：
    ```bash

    cd C:\Users\<用户名>\AppData\Local\Microsoft\WindowsApps
    rm python*
    ```
2. 下载git
    1. 前往 [https://git-scm.com/download/win](https://git-scm.com/download/win)
    2. 配置信息：
    ```bash
    git config --global user.email xxx
    git config --global user.name xxx
    ```
     3. 设置代理：
     ```bash
     git config --global http.proxy http://127.0.0.1:<端口号>
     git config --global https.proxy https://127.0.0.1:<端口号>
     ```

3. 安装python 
    1. 去 [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) 寻找稳定版本的x64可执行文件下载
    2. 配置pip源：
    ```bash

    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
    pip config set global.proxy http://127.0.0.1:<端口号>
    ```
4. 安装MinGW [https://sourceforge.net/projects/mingw/](https://sourceforge.net/projects/mingw/) 

    > 忘记安装配置怎么选看[这篇博客](https://blog.csdn.net/QuantumYou/article/details/119676283)
5. 安装cmake [https://cmake.org/download/](https://cmake.org/download/)
> 如果安装了VS，可以查看路径`C:\Program Files\Microsoft Visual Studio\2022\Community\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin`，这里面存在VS构建的cmake，上面的cmake可以不用下载。

6. 安装node [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

    npm 换源：
    ```bash
    npm config set registry https://registry.npmmirror.com
    ```

7. 安装go 
    1. 找到安装 [https://golang.google.cn/dl/](https://golang.google.cn/dl/)
    2. 配置：
     ```bash

     go env -w GO111MODULE=on
     go env -w GOPROXY=https://goproxy.cn,direct
     ```
    3. 如果有必要，设置GOROOT和GOPATH环境变量
8. 安装Java：[https://www.oracle.com/hk/java/technologies/downloads/](https://www.oracle.com/hk/java/technologies/downloads/)
9. 测试以上编译器、解释器能否正常工。
10.  安装vscode（科学上网），安装后打开并登录GitHub账号进行配置与插件同步（等到左下角的“正在同步”消失后，就可以退出了）
11. 安装VS2022（反正是最新版的就行），安装windows通用开发和C++的（19G好像是）
12. 安装 glow: 

```bash
## windows
$ wget -c https://kirigaya.cn/files/windows_tools/glow_Windows_x86_64.zip
## Linux
$ wget -c https://kirigaya.cn/files/linux_tools/glow_Linux_x86_64.tar.gz
```

### 五、配置终端

以管理员权限打开windows terminal，执行下面的指令
1. 配置 powershell，安装powershell插件
```powershell
# 允许运行Install-Module脚本
set-executionpolicy remotesigned

# 更新最新版本的PSReadLine，为了自动补全
Install-Module PSReadLine -Force

# 创建powershell 的初始化脚本，点击确认创建即可
notepad $profile

# 安装几个插件
Install-Module posh-git
Install-Module Terminal-Icons
```

2. 安装并配置oh-my-posh
    1. 安装oh-my-posh

     ```powershell
     winget install JanDeDobbeleer.OhMyPosh -s winget
     ```
     > 如果该方法失效，移步[oh-my-posh windows set up](https://ohmyposh.dev/docs/installation/windows)查看最新安装方法

    2. 下载oh-my-posh主题
    ```bash
    git clone https://github.com/JanDeDobbeleer/oh-my-posh 
    ```
    将里面的theme文件夹保留即可。

    3. 测试
    ```powershell
    oh-my-posh init pwsh | Invoke-Expression
    ```

3. 配置整体运行文件

```powershell
notepad $profile
```
将以下文本写入其中：
```powershell
oh-my-posh init pwsh --config <主题路径（json文件）> | Invoke-Expression
Import-Module posh-git
Import-Module Terminal-Icons
Set-PSReadLineOption -PredictionSource History
Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete
cls
```

重新打开terminal查看安装效果

### 六、安装WSL2

以管理员身份打开windows terminal

```powershell
# 开启VM组件 开启后需要重启电脑
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform

# 安装Ubuntu
wsl --install -d Ubuntu

# 设置为wsl2
wsl --set-default-version 2
```

以上操作完成后，在win+Q中搜索ubuntu on Windows，打开，完成安装和账号注册（Unix账号必须是开头小写的单词）

完成后打开windows terminal的下拉菜单，会发现多出了一个Ubuntu的标签页。后面移步Linux的配置。


### 七、兴趣爱好

- cuDNN开发套件（用于开发kernel op和安装编译torch cuda） [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
- 暴雪战网 [https://starcraft2.blizzard.com/zh-tw/](https://starcraft2.blizzard.com/zh-tw/) 
> 下个星际争霸做强化学习不过分吧


- FL Studio [https://www.flstudiochina.com/xiazai.html](https://www.flstudiochina.com/xiazai.html)
- Synthesizer V [https://dreamtonics.com/en/synthesizerv/](https://dreamtonics.com/en/synthesizerv/)
- Steam [https://store.steampowered.com/about/](https://store.steampowered.com/about/)

卸载无用的组件：

```powershell
winget uninstall "Windows web experience Pack"
```

## Linux

### 一、给apt换源
```bash
cd /etc/apt/

# 留个source备份
sudo mv sources.list sources.list.backup

# 使用源
sudo vim sources.list
```

写入：（根据ubuntu版本号自己查，如果源的版本和ubuntu版本不一致，那么后续更新就会发生依赖问题。e.g. Ubuntu20.04对应focal-XXX的源）

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse multiverse
```

保存退出。

移除自带的包（因为可能和国内源的软件有冲突）
```bash
sudo apt remove ubuntu-advantage-tools
```

更新一下：
```bash
sudo apt update
sudo apt upgrade
```

### 二、安装基本的开发工具

```bash
# mingw套组
sudo apt install build-essential cmake 

# python3 pip 工具
sudo apt install python3-pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 三、美化终端（使用zsh）

安装
```bash
sudo apt install zsh
chsh -s $(which zsh)
```

在bashrc里面加入
```bash
export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
```

重新打开当前终端，就会进入zsh终端，接着安装oh-my-zsh主题框架。

```bash
cd ~
git clone https://gitee.com/mirrors/oh-my-zsh
sh oh-my-zsh/tools/install.sh
```

安装两个插件
```bash
cd ~/.oh-my-zsh/custom/plugins/

# 高亮关键词
git clone https://gitee.com/testbook/zsh-syntax-highlighting.git

# 自动补全
git clone https://gitee.com/qiushaocloud/zsh-autosuggestions

# 更加丰富的高亮
git clone https://gitee.com/wangl-cc/fast-syntax-highlighting

# 展示自动补全历史
git clone https://gitee.com/wenhuifu/zsh-autocomplete
```

然后进入`~/.zshrc`在plugins参数中添加`zsh-syntax-highlighting`和`zsh-autosuggestions`。也就是说你的.zshrc中必须要有一行长这样：

```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting zsh-autocomplete)
```

### 四、安装部分驱动程序

默认pip 版本较低，升级一下
```bash
pip install --upgrade pip
```

安装opencv的包
```bash
sudo apt install libopencv-dev
pip install opencv-python
```

安装torch cuda
```
sudo apt install nvidia-cuda-toolkit
```
然后去https://pytorch.org/找对应的安装指令。