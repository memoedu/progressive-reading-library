## vscode extension market

[vscode extension market](https://marketplace.visualstudio.com/vscode) 负责 vscode 上面的插件的下载和管理。通过下面的操作从零基于命令行工具发布你的 vscode 插件。

### 1. 创建账号

先注册开发者账号，进入 https://marketplace.visualstudio.com/vscode ，点击右上角的 sign in，注册账号，登陆。然后进入 https://dev.azure.com/ 创建组织，进入组织后，创建一个新的项目，比如我的就是：

- 组织名：1193466151（我的邮箱）
- 项目：vscode-extension

### 2. 获取 user token

然后点击下图这个地方，进入 token 管理界面。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-96f4ad3cd80beb0f7a74aa79bba1cb39_1440w.png" style="width: 100%;"/>
</div>

在 token 管理界面，创建新的 token，然后保存起来。

### 3. 安装 vsce 命令行工具

安装 vsce，这是打包 & 发布 vscode 插件的基本工具。

```bash
npm install vsce -g
```

### 4. 登陆 vscode 开发者账号

登陆开发者账号。

```bash
vsce login <user name>
```

此时会要求你输入 token，输入你刚刚记录下来的 token，然后回车，显示如下成功：

```
https://marketplace.visualstudio.com/manage/publishers/
Personal Access Token for publisher 'kirigaya': ************************************************************************************

The Personal Access Token verification succeeded for the publisher 'kirigaya'.
```

此时，vsce 会把你的 token 记录进入 `~/.vsce` 中，这样你后续发布和登陆都不需要再次输入凭证。

### 5. 发布插件

进入你的 vscode 插件项目，进行发布。

```bash
vsce publish
```

vsce publish 会自动进行打包和发布，如果只需要打包，使用 `vsce package` 即可。


---


## OpenVsx Registry

[OpenVsx Registry](https://open-vsx.org/) 是 eclipse 基金会维护的 vscode 插件的开源版本，上面发布的插件可用于 vscode 和所有基于 vscodium 内核开发的「类vscode编辑器」的插件系统，比如 [Vscodium](https://github.com/VSCodium/vscodium)，[Trae](https://www.trae.com.cn/)，[Cursor](https://www.cursor.com/) 等等。

:::info
[OpenVsx Registry](https://open-vsx.org/) 的插件生态广泛应用于 国产芯片Loognson64，国产操作系统 AOSC，开放麒麟，国产 AI IDE Trae 等等软件上，是一个不容小觑的生态圈。尽量积极适配吧。

热知识：vsix 插件本质就是一个 zip 压缩包，它的架构和 docx 与 pptx 差不多。
:::

### 1. 创建账号

先进入 https://open-vsx.org/ 创建账号，可以通过 GitHub 账号登陆。登陆后，进入 [账户界面](https://open-vsx.org/user-settings/profile)，你需要将当前账号绑定到你的 「eclipse 基金会账号」，为此，你还需要注册一个基金会账号，完成绑定和关联。完成后，你的 [账户界面](https://open-vsx.org/user-settings/profile) 应该如下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e846229521aaf18c56a093df1a9a8a8e_1440w.png" style="width: 100%;"/>
</div>

### 2. 创建命名空间

在 https://open-vsx.org/user-settings/namespaces 创建命名空间，空间名一定要和你的插件 package.json 内的作者名一致。

创建完成的 namespace 可能还需要进行 verify。进入 open-vsx 的 github 仓库提 issue 进行 namespace 的 verify，下面是我的名为 kirigaya 的 namespace 进行 verify 的一个例子：https://github.com/EclipseFdn/open-vsx.org/issues/3958

### 3. 安装 ovsx 命令行工具

open vsx 通过 `ovsx` 工具进行打包和发布，通过下面的命令安装：

```bash
npm install -g ovsx
```

### 4. 登陆你的命名空间

和 vscode 插件差不多，登陆你的命名空间，并从 https://open-vsx.org/user-settings/tokens 获取准入的 token

```bash
ovsx login <namespace>
```
登陆完成后，token 会被记录在 `~/.ovsx` 中。

### 5. 发布你的插件

```bash
# 打包 + 发布
ovsx publish

# 只发布
ovsx publish -i </path/to/{extension}.vsix>

# 只打包
ovsx package
```