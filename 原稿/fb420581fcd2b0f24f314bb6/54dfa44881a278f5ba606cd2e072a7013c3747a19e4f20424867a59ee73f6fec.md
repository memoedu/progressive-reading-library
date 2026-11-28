## 前言
相信大家在 GitHub 上维护一个项目的时候遇到过这样的问题：很多用户提交 issue 总是不提供额外的信息，比如只说出了什么问题，但是不说是什么平台下运行出的问题，导致我们每次帮助用户解决问题时还要反复确认这些前置条件。

作为开发者，我们自然是知道很多前置条件的，比如什么操作系统，什么芯片，特定组件的版本号等等，但是这些关键信息用户是不知道的。为了弥合这样的信息差，节省我们和用户的时间，GitHub 提供了一个功能，issue template，这个功能允许开发者为仓库的 issue 提问提供预先设置的模板，比如【报告 Bug】，【建议新特性】等等。并且通过 配置文件 来自定义这些提问模板的 Web 表单。

---

## 具体做法

GitHub 官方的原文在这里：[Configuring issue templates for your repository](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)

我们先在自己的项目根目录建立文件夹 `.github/ISSUE_TEMPLATE `，然后在里面新建一个 yaml 文件，比如我的就叫做 `bug-report.cn.yaml`，代表这是一个用看汇报 Bug 的中文模板，内容如下：


```yaml
name: 报告 Bug
description: 向我们报告 Bug。
title: "[Bug]: "
labels: ["bug"]
body:
  - type: markdown
    attributes:
      value: |
        投我以木桃，报之以琼瑶。
        填写之前请阅读 [nc-ai | 关于反馈](https://nc-ai.cn/article/cxnqr517/#%E5%85%B3%E4%BA%8E%E5%8F%8D%E9%A6%88)
  - type: dropdown
    id: os
    attributes:
      label: 操作系统
      description: 您在什么操作系统上运行？
      options:
        - Windows
        - Mac
        - Linux
        - 其他
      default: 0
    validations:
      required: true
  - type: dropdown
    id: chip
    attributes:
      label: 芯片架构
      description: 您在什么架构的芯片上运行？
      options:
        - x86
        - arm
        - loongson
        - 其他
      default: 0
    validations:
      required: true
  - type: dropdown
    id: connect
    attributes:
      label: 运行方式
      description: 您采用何种方式运行？
      options:
        - 桌面运行
        - SSH 远程连接
        - WSL2
        - 虚拟机
        - 其他
      default: 0
    validations:
      required: true
  - type: input
    id: vscode-version
    attributes:
      label: Vscode / Vscodium 版本
      placeholder: "1.96.2"
    validations:
      required: true
  - type: input
    id: dide-version
    attributes:
      label: Digital IDE 版本
      placeholder: "0.4.0"
    validations:
      required: true
  - type: textarea
    id: meet-question
    attributes:
      label: 你遇到的问题
    validations:
      required: true
  - type: textarea
    id: expect
    attributes:
      label: 你希望的运行结果
    validations:
      required: false
  - type: textarea
    id: actual
    attributes:
      label: 实际的运行结果
    validations:
      required: false
  - type: textarea
    id: log
    attributes:
      label: 运行日志
      render: shell
    validations:
      required: false
```

然后我们把它提交到 GitHub 上，如果你的模板写的没有语法错误，那么该文件在 github 中会被渲染成对应的表单，并且上方会出现蓝色的字样，代表编译无误：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-2c25bc5502813516c9e750b473dc235e_1440w.png" style="width: 100%;"/>
</div>

此时在提 issue，选择 name 为 “报告 Bug” 的，就可以看到了如下的效果了：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-6f26ffbb9a2bb95e225c906d605c845a_1440w.png" style="width: 100%;"/>
</div>



相信这样可以让我们和用户都能获取一段快乐的 CI 体验！