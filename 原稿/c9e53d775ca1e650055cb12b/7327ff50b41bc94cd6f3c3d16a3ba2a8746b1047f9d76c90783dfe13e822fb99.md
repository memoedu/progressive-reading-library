## 前言

最近在完成 3A6000 上的软件适配，顺便完成一些我之前就想要完成的评测。刚好整理一下常用的软件包，以便后来的使用。

---


## 软件清单

QQ: https://im.qq.com/linuxqq/index.shtml
微信：https://linux.weixin.qq.com/
wps：https://365.wps.cn/download365
龙芯浏览器：https://www.loongnix.cn/zh/api/lbrowser/

:::info
上述下载得到的一般都是 deb 文件，这是 Linux 上的安装包文件。如果你的操作系统无法直接安装它，那么可以在 deb 所在的文件夹打开命令行（ctrl + alt + T），然后输入：
```bash
sudo dpkg -i deb文件名
```
:::


通过下面的脚本可以一键安装。文件名： `install.sh`

```bash
wget -c https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.15_250110_loongarch64_01.deb
wget -c https://dldir1v6.qq.com/weixin/Universal/Linux/WeChatLinux_LoongArch.deb
wget -c https://ftp.loongnix.cn/browser/lbrowser/3.3.2001.5/la64/lbrowser_3.3.2001.5-1.stable.loongarch64.deb
wget -c https://wps-linux-365.wpscdn.cn/wps/download/ep/Linux365/19829/wps-office_12.8.2.19829.AK.preload.sw_loongarch64.deb
sudo dpkg -i https://dldir1.qq.com/qqfile/qq/QQNT/Linux/QQ_3.2.15_250110_loongarch64_01.deb
sudo dpkg -i https://dldir1v6.qq.com/weixin/Universal/Linux/WeChatLinux_LoongArch.deb
sudo dpkg -i https://ftp.loongnix.cn/browser/lbrowser/3.3.2001.5/la64/lbrowser_3.3.2001.5-1.stable.loongarch64.deb
sudo dpkg -i https://wps-linux-365.wpscdn.cn/wps/download/ep/Linux365/19829/wps-office_12.8.2.19829.AK.preload.sw_loongarch64.deb
```

---

## 开发者工具包

nodejs: https://www.loongnix.cn/zh/api/nodejs/nodejs-history/
rust: https://www.loongnix.cn/zh/toolchain/Rust/

:::info
上面的都是旧世界的。新世界的 rust 直接使用 rustup.rs 官网里面的（Rust 官方直接支持新世界的 Rust），nodejs 直接用 oma 安装。
:::

如果使用的是 AOSC 操作系统，那么系统自带的 `oma` （小熊猫管理器）就能完成常用开发工具的安装，非常好用～。

### oma 基础使用

打开命令行，输入 `oma`，初始化完成后会进入一个管理菜单，输入你想要下载的软件，就能看到结果，比如上 vscodium 的搜索结果如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-d9825546d28630eb5728f7bf55768d36_720w.png" style="width: 100%;"/>
</div>

通过上下移动移动到需要安装的软件，按下空格选中后，在右侧通过 ctrl U 标记需要更新，然后按下 esc 即可进入下载引导界面。根据指示即可完成安装。

:::info
clash-verge 的安装需要额外在 ~/.local/share/io.github.clash-verge-rev.clash-verge-rev 下载 Country.mmdb
:::

`oma` 给的软件非常齐全，nodejs 都支持到 22，vscodium 则是和 vscode 上游版本保持一致。甚至你可以直接下载到龙芯版本的 clash-verge 进行魔法（直接在龙芯上编译 clash-verge 是失败的，因为 mihono 内核没有龙芯版本的 build，快说，谢谢安和社区）。