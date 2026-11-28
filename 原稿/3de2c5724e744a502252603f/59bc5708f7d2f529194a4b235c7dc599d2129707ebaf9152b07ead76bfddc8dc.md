## 前言

最近连续两次遇到了朋友的 Linux 终端因为 zsh 插件问题无法使用。这种时候不要急着重装系统。作为折腾了服务器两年半的 rookie，我总结一下这种情况的应对经验。

## 常用指令用不了

如果常用指令，比如 `cp`, `rm` 无法使用了，三个方法解决。

### 方法一：使用 /usr/bin 下的 link

直接调用 `/usr/bin` 下的软链接：

```bash
/usr/bin/cp ~/.zshrc ~/.zshrc.backup
```

这样把出了问题的配置文件移动一下，然后重启终端，恢复常用命令使用权后再进行问题排查。

### 方法二：scp 暴力替换

虽然此时你无法使用部分指令，但是 sshd 是正常运作的，它的 runtime 不会被终端软件污染，因此，我们可以从外部写一个正常的 `.bashrc` 或者 `.zshrc`，然后在可以 ssh 服务器的PC上通过 `scp xxx.rc username@IP:/path/to/home` 的方式强制替换。

> scp 可以做到强制替换远程服务器路径上的文件，通过这个方法来解困。

### 方法三：通过云服务器厂商提供的登录工具

不少的云厂商都提供了一些登录服务器的后门程序，比如 VNC 或者自研协议，你可以通过它们登录服务器，然后使用常用指令对问题进行排查。



以上三种方法只有方法二鲁棒性最佳，如果你的服务器的 glibc 的链接库炸了，方法一和方法三都会失效。建议关键资源的服务器上多留几个数据导出后门，而且要隔离运行。



## 备案：bashrc

如果你实在没办法了，要删除 `.bashrc` 或者 `.zshrc`，那也没关系，我这里留一份 bashrc 和 zshrc 的备份，欢迎炸机后恢复来抄：

`.bashrc`（如果使用 zshrc，可以考虑把里面的 conda 初始化那几行删除了）

```bash
# ~/.bashrc: executed by bash(1) for non-login shells.
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

shopt -s checkwinsize

[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
        # We have color support; assume it's compliant with Ecma-48
        # (ISO/IEC-6429). (Lack of such support is extremely rare, and such
        # a case would tend to support setf rather than setaf.)
        color_prompt=yes
    else
        color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi


alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/data/yifanchang/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/data/yifanchang/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/data/yifanchang/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/data/yifanchang/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<

export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
source ~/.zshrc
```

`.zshrc`（可以把 plugins 这行删除了，如果你没有这几个插件的话）

```bash
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="af-magic"

plugins=(git zsh-syntax-highlighting zsh-autosuggestions fast-syntax-highlighting)

source $ZSH/oh-my-zsh.sh

# <<< conda initialize <<<
__conda_setup="$('/data/baizhiwang/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/data/baizhiwang/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/data/baizhiwang/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/data/baizhiwang/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup

```



