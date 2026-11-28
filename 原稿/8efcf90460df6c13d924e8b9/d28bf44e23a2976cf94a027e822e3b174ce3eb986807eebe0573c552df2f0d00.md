简单说一下我使用的powershell主题插件oh-my-posh3的使用方法和美化流程。

进入oh-my-posh3，就可以看到官方的安装攻略：[Windows | Oh My Posh](https://ohmyposh.dev/docs/installation/windows)

不过在真实的win11环境下安装会有一些小坑，所以我就以最简短的笔体来描述一下安装过程。

> oh-my-posh3比较影响性能，如果你的CPU不是很好，不是很推荐使用。

---

## 准备

下载Nerd字体：[Nerd Fonts - Iconic font aggregator, glyphs/icons collection, & fonts patcher](https://www.nerdfonts.com/font-downloads)

我使用的是Caskaydia Cove Nerd Font

windows terminal终端配置：改字体（必须是nerd系列字体）+改背景+改图标（这个教程非常多，相信大部分玩家也会操作）

---

## 流程

以管理员权限打开windows terminal，执行下面的指令

### step.1 配置 powershell，安装powershell插件

```bash
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

### step.2 安装oh-my-posh和主题

1. 安装oh-my-posh

```bash
winget install JanDeDobbeleer.OhMyPosh -s winget
```

如果该方法失效，移步[oh-my-posh windows set up](https://ohmyposh.dev/docs/installation/windows)查看最新安装方法

2. 下载oh-my-posh主题

```bash
git clone https://github.com/JanDeDobbeleer/oh-my-posh
```

将里面的theme文件夹保留即可。

3. 测试
```bash
oh-my-posh init pwsh | Invoke-Expression
```

如果终端变成了下图就说明安装成功：

<div align=center>
<img src="https://pic2.zhimg.com/v2-f50eab39a6f8e19dbc15d87c17bbb45d_r.jpg" style="width: 80%;"/>
<p class="img-caption">这是oh-my-posh3的默认主题</p>
</div>


### step.3 配置整体运行文件

```bash
notepad $profile
```

将以下文本写入其中：

```bash
oh-my-posh init pwsh --config <主题路径（json文件）> | Invoke-Expression
Import-Module posh-git
Import-Module Terminal-Icons
Set-PSReadLineOption -PredictionSource History
Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete
cls
```

重新打开terminal查看安装效果。

其中的<主题路径（json文件）>就是之前clone的那个github仓库中theme文件夹下的json文件的路径。而我使用的是atomic.omp.json这个文件。

<div align=center>
<img src="https://pic2.zhimg.com/v2-617d9c2875b562ce8de70548b781b5e1_r.jpg" style="width: 80%;"/>
<p class="img-caption">atomic主题展示</p>
</div>

---

## 更强大的powershell自动补全

powershell还支持更加强大的列表自动补全：

<div align=center>
<img src="https://pic2.zhimg.com/80/v2-905fdf47a7d3d66b025ccc70c9c7cf51_1440w.webp" style="width: 80%;"/>
<p class="img-caption">ListView模式</p>
</div>



想要实现这个效果，只需要`PredictionSource`参数设置为`ListView`：

```bash
Set-PSReadLineOption -PredictionSource History -PredictionViewStyle ListView
```