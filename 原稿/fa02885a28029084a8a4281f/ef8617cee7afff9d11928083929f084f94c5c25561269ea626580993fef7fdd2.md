为了一个本地的 linter 测试，重新把 modelsim 安装一遍。

## 资源获取

目前提供版本为 2020.4

- 百度网盘：https://pan.baidu.com/s/1sJHqoj6VEwrmf6GBMLXshQ
- 提取码：161d

---

## 安装与破解

### 安装

解压完上面下载的 7z 文件后，文件如下：

- Crack 破解工具集
- modelsim-win64-2020.4-se.exe 安装程序

先点击安装程序，直到 `Install Hardware Security Key Driver` 这一步之前全点 是 即可，这一步点击 否，完成安装。

如果只是想要使用 modelsim 的命令行模拟器，到了这一步就已经安装完成了，打开命令行来进行验证：

```bash
vlog -help General
```

输出：
```
Model Technology ModelSim SE-64 vlog 2020.4 Compiler 2020.10 Oct 13 2020
------------------------------------General------------------------------------
General Category for option

--------------------------------------------------------------------------------
-32                             Run in 32-bit mode
...
...
```

### 破解

如果希望破解 modelsim，先将 Crack 文件夹下的所有文件覆盖到 modelsim 安装目录的 `win64` 文件夹下。它默认是 `C:\modeltech64_2020.4\win64`。

然后在当前目录打开命令行（管理员权限），输入：

```bash
attrib -r mgls64.dll; MentorKG.exe -patch . ; attrib +r mgls64.dll
```

稍等片刻后，操作系统会使用默认文本编辑器打开 `LICENSE.txt` ，将它另存为到 modelsim 的安装目录下。

然后新建名为 LM_LICENSE_FILE 的用户变量，值为 LICENSE.txt 的路径。默认为 `C:\modeltech64_2020.4\LICENSE.txt`
