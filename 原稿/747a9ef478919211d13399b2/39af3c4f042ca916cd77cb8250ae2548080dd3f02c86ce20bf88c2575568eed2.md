## 一键安装版

有 sudo

```bash
bash -c 'set -e; sudo apt update; sudo apt install -y zsh; chsh -s "$(which zsh)"; cd ~; [ -d oh-my-zsh ] || git clone https://gitee.com/mirrors/oh-my-zsh; RUNZSH=no sh oh-my-zsh/tools/install.sh --unattended; cd ~/.oh-my-zsh/custom/plugins/; [ -d zsh-syntax-highlighting ] || git clone https://gitee.com/testbook/zsh-syntax-highlighting.git; [ -d zsh-autosuggestions ] || git clone https://gitee.com/qiushaocloud/zsh-autosuggestions; [ -d fast-syntax-highlighting ] || git clone https://gitee.com/wangl-cc/fast-syntax-highlighting; if grep -q "^plugins=(" ~/.zshrc; then sed -i "s/^plugins=(.*)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)/" ~/.zshrc; else echo "plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)" >> ~/.zshrc; fi; if ! grep -q "export SHELL=" ~/.bashrc; then printf "\nexport SHELL=\`which zsh\`\n[ -z \"\$ZSH_VERSION\" ] && exec \"\$SHELL\" -l\n" >> ~/.bashrc; fi; echo "oh-my-zsh 及插件安装完成，请重新打开终端即可生效"'
```

无 sudo

```bash
bash -c 'set -e; apt update; apt install -y zsh; chsh -s "$(which zsh)"; cd ~; [ -d oh-my-zsh ] || git clone https://gitee.com/mirrors/oh-my-zsh; RUNZSH=no sh oh-my-zsh/tools/install.sh --unattended; cd ~/.oh-my-zsh/custom/plugins/; [ -d zsh-syntax-highlighting ] || git clone https://gitee.com/testbook/zsh-syntax-highlighting.git; [ -d zsh-autosuggestions ] || git clone https://gitee.com/qiushaocloud/zsh-autosuggestions; [ -d fast-syntax-highlighting ] || git clone https://gitee.com/wangl-cc/fast-syntax-highlighting; if grep -q "^plugins=(" ~/.zshrc; then sed -i "s/^plugins=(.*)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)/" ~/.zshrc; else echo "plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)" >> ~/.zshrc; fi; if ! grep -q "export SHELL=" ~/.bashrc; then printf "\nexport SHELL=\`which zsh\`\n[ -z \"\$ZSH_VERSION\" ] && exec \"\$SHELL\" -l\n" >> ~/.bashrc; fi; echo "oh-my-zsh 及插件安装完成，请重新打开终端即可生效"'
```

---

我其实在之前的两篇博客中都提到了如何安装oh-my-zsh，以及对应插件的安装，但是一直没有单独写一篇。于是乎，今天这篇算是单独写出来了，以供未来快速查阅。

## 关于zsh的前言

首先需要明确一个概念，我们的Linux默认的shell是bash，而zsh则是bash的加强版，兼容bash的所有命令，同时也提供了更加丰富的功能。

bash终端有一个东西叫做`.bashrc`，这个文件是bash的初始化文件，每当你打开bash终端，这个文件中的脚本就会被运行。同样的，zsh终端也有一个类似的东西，叫做`.zshrc`，每当zsh终端被启动后，`.zshrc`中的内容就会被运行。因此，我们一般会将

## 安装流程

### 安装zsh终端

```bash
sudo apt install zsh
chsh -s $(which zsh)
```

### 将zsh设置为默认终端

在bashrc里面加入

```bash
export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
```

重新打开当前终端，就会进入zsh终端。

### 安装oh-my-zsh主题框架

zsh本身提供的机制很多，我们自己改费时费力，所以可以使用专门为zsh设计的主体框架——`oh-my-zsh`，这个类似于`oh-my-posh3`和`powershell`的关系一样。

接着安装oh-my-zsh主题框架。

```bash
cd ~
git clone https://gitee.com/mirrors/oh-my-zsh
sh oh-my-zsh/tools/install.sh
```

### 安装插件

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

> ## autocomplete 目前是高危插件，建议删除！！！

然后进入`~/.zshrc`在plugins参数中添加`zsh-syntax-highlighting`和`zsh-autosuggestions`。也就是说你的.zshrc中必须要有一行长这样：

```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)
```

![](https://pic4.zhimg.com/v2-b6d75a215ad0a1a414da45ceaf34dbb7_r.jpg)

### 重新运行入口文件

当上面的配置完成后，你可以退出再进入终端。当然，和bash，zsh提供了更加优雅的方式快速刷新配置文件带来的改动：

```bash
zsh
```

这句话会让zsh终端重新运行`.zshrc`，当更新了`.zshrc`文件或者`oh-my-zsh`配置时，可以运行这句话来让改动生效。

查看效果：

![](https://pic1.zhimg.com/80/v2-6ae2c427e2d68d85e2a9705718f5d5c0_1440w.webp)