## 前言

最近正在给龙芯的电脑进行软件适配，正好选择一个新的 LoongsonArch 的操作系统进行把玩，通过朋友的介绍，决定玩玩 AOSC OS。众所周知，LoongArch 有两套不兼容的软件体系，习惯上大家把它们叫作「旧世界」和「新世界」，而 AOSC 属于新世界。

:::info 关于新世界和旧世界
因为我习惯使用上游分支的 rust 编译器进行开发，所以就直接用新世界咯。旧世界的 OS 基本都是商业 OS。下面是具体区别：[https://areweloongyet.com/docs/old-and-new-worlds/](https://areweloongyet.com/docs/old-and-new-worlds/)
:::

---

## 什么是 AOSC

安同 OS（英译：AOSC OS）是一款以“简明可靠”为设计及维护目标的 Linux 发行版。本系统主要面向有一定 Linux 使用经验的用户，针对个人桌面设备优化体验，致力于为用户提供开箱即用和简便可靠的工作环境。

:::info
摘录自 AOSC 官网 [https://aosc.io/aosc-os](https://aosc.io/aosc-os)
:::

---

## 在龙芯主机上安装 AOSC


我使用的是官方给我的 3A6000 主机，它的具体配置可以在官网查到。老规矩，先制作启动介质，然后安装，你需要：

- 准备一个至少 10G 的 U盘
- 从官方下载对应的镜像：https://aosc.io/download#aosc-os-download
- 先打开或下载 rufus，选择上述的 U盘 并读入刚刚下载好的镜像，点击 start 制作。
- F12 进入 gnu grub
- 选择 Setup 那一项进入【龙芯 BIOS】
- 选择设备管理
- 选择你插入的 U盘（不知道名字可以返回上一级查看设备中的 USE设备）
- 选择 `Install AOSC OS`

后面就可以开始愉快地进行安装了！

特别好评的是， AOSC 安装每次就会让你反复确认计算好的安装配置：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-d3d68b4dd4f36891be139cf5f3143f36_1440w.jpeg" style="width: 100%;"/>
</div>