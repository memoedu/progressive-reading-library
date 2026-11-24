## 前言

最近在构建我的 TIP 系统的 CI 部分的功能模块，我希望我只需要在群里说一句话，TIP 就能帮我拉取最新的代码，并且自动编译，发布公告，发布 release 到 GitHub，发布 vsix 到 vscode 插件商城和 open vsx 插件商城全自动流程。

关于发布到插件商城的命令我在 [vscode 和 open vsx 插件发布流程](https://kirigaya.cn/blog/article?seq=315) 中已经讲过了，发布公告等功能也随着 Lagrange,Onebot 0.0.9 的发布被我解决了，所以只剩下一个通过命令的方式发布 release 包。

想要通过程序化的方式将本地的文件发布为 github release，只需要通过 gh 工具就行了。

---

## 1. 安装 gh

gh 是 GitHub 官方的命令行工具，我们直接安装它就行：

```bash
sudo apt install gh
```

---

## 2. 登陆

发布之前我们需要先通过 gh 登陆 github，在登陆前，我们先去 GitHub 上获取登陆 token，步骤如下：

1. 登录 GitHub。
2. 点击右上角的头像，选择 Settings。
3. 在左侧菜单中选择 Developer settings。
4. 选择 Personal access tokens，然后点击 Generate new token。
5. 选择所需的权限（至少需要 repo 权限）。
6. 生成令牌并保存到个人备忘录中，因为之后你将无法再次查看它。

回到命令行，通过下面的命令登陆：

```bash
gh auth login
```

然后根据下面的步骤进行：

1. 选择 Github.com
2. 选择 HTTPS 作为认证方式
3. 选择填入 token 并复制你刚刚的 token
4. 登陆成功

---

## 发布 release

gh 根据运行的当前目录的 `.git` 来获取目标仓库的链接，所以使用 gh 管理项目的时候不需要输入链接。进入我们想要发布 release 的项目的项目文件夹，输入 git remote -v，确保输出的链接的 origin 的域名是 github。gh 发布 release 的基本指令如下：

```bash
gh release create <tag> <files> --title "Release Title" --notes "Release Notes"
```

假设我们现在的 release 是当前目录下的 openmcp-0.0.8.vsix，那么我们可以这样发布 v0.0.8 版本的 release：

```bash
gh release create v0.0.8 openmcp-0.0.8.vsix --title "Release v0.0.8" --notes "Initial release of OpenMCP client."
```

显示发布 release 的链接，说明运行成功。