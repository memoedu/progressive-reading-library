最近心血来潮（大概是因为RWKV），特别想打代码，顺便升级一下我的“代码阅览器”Vim，记录一下配置流程。

## 下载Vim插件配置插件

你可以自己去下插件，但是配置起来比较麻烦，可以用插件管理插件vim-plug代劳：

```bash
mkdir -p ~/.vim/autoload
wget -P ~/.vim/autoload https://kirigaya.cn/files/linux_tools/plug.vim
```


> 第二行网络有问题的朋友可以去gitee上找镜像文件，或者在PC上手动下



## 配置vimrc

进入`~/.vimrc`，写入：

```bash
syntax on

" 设置tab宽度为4个空格
set tabstop=4
set shiftwidth=4
set expandtab

" 自动缩进
set autoindent

" 显示行号和列号
set number
set ruler

" 显示当前行
set cursorline

" 设置行号颜色为灰色
highlight LineNr ctermfg=gray

" 让vim可以用鼠标滚轮
set mouse=a

call plug#begin('~/.vim/plugged')
" Shorthand notation for plugin
Plug 'jiangmiao/auto-pairs'
Plug 'vim-airline/vim-airline'
Plug 'preservim/nerdtree'
call plug#end()

set laststatus=2  "永远显示状态栏
let g:airline_powerline_fonts = 1  " 支持 powerline 字体
let g:airline#extensions#tabline#enabled = 1 " 显示窗口tab和buffer
```

如果是Windows的话，请加入如下的几句：

```bash
set nocompatible
set backspace=indent,eol,start
set nobackup
set noswapfile
```


## 下载插件

### 方法一：一键下载

进入vim后，输入：
```
:PlugInstall
```
然后回车，vim-plug会自动去GitHub上找对应的插件并下载。然后完成下载。


### 方法二：手动下载

由于网络问题，国内直接运行vim-plug的下载指令会卡住，可以手动下载gitee的镜像。下面的只是示例，具体的链接请自行查找。需要注意，手动下载的插件不一定符合Windows系统。

```bash
mkdir ~/.vim/plugged && cd ~/.vim/plugged
git clone https://gitee.com/rulei_mirror/auto-pairs.git
git clone https://gitee.com/duyz1218/vim-airline.git
git clone https://gitee.com/archongum/nerdtree.git
```



然后一切大吉：
<div align=center>
<img src="https://picx.zhimg.com/80/v2-e0a6985d79f4cabe746a886736b39054_1440w.png" style="width: 80%;"/>
<p class="img-caption">vim插件安装完后的示意图</p>
</div>

通过在命令模式输入如下可以在右侧分裂出一个终端：

```
:rightbelow vert term
```