# Agentic RL 训练用 sandbox 后端汇总

sandbox 是目前业界 Agentic LLM 训练使用的关键设施，无论是制作 sft 数据还是进行 rl 训练，都离不开 sandbox 的支持。关于具体的 Agentic LLM 训练流程一览，我会在后续单独写一篇文章介绍。

## 01 什么是 sandbox

先给 sandbox 下个定义吧，至少在 Agentic LLM 训练场景中，sandbox 一般指的是一种受控的执行环境。它允许运行程序，同时限制程序能访问的文件、网络和设备，能使用的计算资源，以及它对宿主机和其他任务产生的影响。

虚拟化技术已经非常普及了，我们购买的云服务器实例（cloud server instance）就属于典型的一个 sandbox，虽然看起来用起来它就是一台独立的 Linux 服务器，但是实际上，它是一个更加庞大的服务器集群上的一个虚拟化容器。至于为啥要搞虚拟化技术，主要的虚拟化技术和容器编排技术的关系是什么，这都不知道的人可以去重修虚拟化技术了。（好吧，我又该吐槽一下如今的计算机教育了，很多大学居然连现代虚拟化技术都不教。）

虚拟化技术在 agent 时代成为了新的核心基础设施。在 Agentic LLM 训练和推理中，sandbox 可以进一步被定义为：

> Sandbox 是承载 Agent 与任务交互的隔离执行环境。它为 rollout 提供可配置的工作区、依赖和资源，使 Agent 能执行命令、修改文件或运行测试，并允许训练系统在任务结束后回收环境。

## 02 为啥需要 sandbox

不同于传统的深度学习模型，比如识别一个猫狗或者生成一张图片或者做一次 zero shot 的 QA，Agentic LLM 需要驱动一套 harness （变成一个 agent）在一台电脑，服务器或者什么类似的玩意儿里面帮你干活，比如创建删除文件，运行代码，或者是编辑海量的数字资产（画图，视频剪辑，编辑 Excel 等等）。

这些操作本身需要需要独立的文件系统和隔离环境。还是拿传统深度学习模型做对比，传统模型识别一个猫狗，1000 次操作可以共享一个文件系统，第 1 次识别猫狗的结果不会影响第 1000 次识别猫狗的结果，毕竟这个模型输入只有一张图片而已，当然这是不会影响的。

但是 Agentic LLM 时代不一样，假设 1000 个任务都在同一台计算机同一套文件系统中，进行就会发生如下的事情：

![图片](https://kirigaya.cn/api/storage/image/3f8393f5-6f4d-43f4-b1c0-a57d4fabd335.webp)

是的，Agentic LLM 的输入不仅仅是一个图片或者一段文字那么简单，Agentic LLM 的输入是整个操作系统的运行时状态和文件系统，这就对倒霉的 AI Infra 工程师提出了新的要求：我们需要保证任何一次与 Agentic LLM 活动有关的训练，推理，数据采集等等乱七八糟的玩意儿过程，都得支持「容器隔离」。

所以就有了 sandbox，懂了吗？

有了 sandbox，我们就可以保证每一次的 rollout，agent 能看到的东西都是彼此独立的，第 1000 次 rollout 不会受到第 1 次 rollout 半点影响，它就算是把容器搞崩溃了，也不会影响。

除此之外，sandbox 还带来了另一个好处：调度 rollout 和执行 rollout 解耦合了。这使得我们的训练或者推理系统或者造题系统在工作时，可以对 sandbox 的执行层进行横向拓展，说人话就是，我们可以同时运行几千几万个 sandbox，大幅度增加 rollout 效率，从而大幅度增加数据采集和训练效率。

DeepSeek 的 desc 模块就是一套极其强大的 agent infra 系统，结合 DeepSeek 的 colocate async 调度算法（我会在下一篇文章中详细介绍），DeepSeek 内部可以做到最大 10 万次/s 的 sandbox 执行调度。emm，我想想，说得更直白一点，假设这些产能全部用于 RL，你可以理解为同时有 10 万个 agent 在做题为 DeepSeek 的训练产出强化学习训练数据。

当然，当然，这显然是一套复杂的分布式系统。

## 03 工业界主流的 sandbox 后端

谈到 sandbox 与高性能编排调度，估计大家第一个想到的就是 docker 和 k8s，确实，Anthropic 在去年的某篇博客提到了他们内部的一部分 agent infra 是采用的 k8s 调度，不过除此之外，在 Agentic RL 场景下常见的容器还有不少，比如 firecracker，E2B 之类的。

下面我总结了一下被知名 Agentic RL 框架所采纳为技术选型的主流 sandbox 后端。

| Sandbox | 交付方式与开源范围 | 隔离机制 | 任务镜像与状态复用 | 启动到首条命令 | 并发与创建限速 | Agentic RL 使用案例 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Docker ** | 本地直控。Moby 核心 Apache-2.0 | Linux 容器，共享宿主内核。用 namespaces、cgroups 和安全策略隔离 | 直接运行 Docker/OCI 镜像。共享只读镜像层，每个实例有独立可写层 | 无统一值。2024 年单机论文测得平均 **871–888 ms**，按论文的容器启动口径 | 取决于宿主机和负载。同一论文验证 **100 个实例同时运行** | [rLLM + R2E-Gym（DeepSWE-Preview）](https://www.together.ai/blog/deepswe)、[Lego-RL](https://github.com/LegoX/Lego-RL)、[Uni-Agent](https://uni-agent.readthedocs.io/en/latest/quickstart/installation.html)、[SkyRL + Harbor](https://github.com/NovaSky-AI/SkyRL/blob/main/docs/content/docs/harbor/index.mdx)。[引擎](https://docs.docker.com/engine/) · [启动实测](https://doi.org/10.1016/j.jss.2024.111965) |
| **Apptainer** | 本地直控。BSD-3-Clause | Linux 容器，共享宿主内核。默认保留较多宿主资源访问，按需配置 namespace 和挂载 | 原生 **SIF**。可导入 OCI 镜像，多个实例共享只读 SIF，并使用各自的临时写层 | 前身 Singularity 在同一论文中使用预构建 SIF，平均 **108–141 ms**。当前版本需另测 | 取决于宿主机和存储。同一论文对 Singularity 验证了 **100 个实例同时运行** | [NeMo Polar + Slime](https://github.com/NVIDIA-NeMo/ProRL-Agent-Server/blob/stable/examples/swegym_slime_grpo/README.md)、[NeMo Gym（Nemotron SWE-RL）](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Super-Technical-Report.pdf)。 |
| **Firecracker** | 本地直控，需 Linux/KVM。Apache-2.0 | KVM microVM，每个实例有独立的 guest 内核 | 需要 guest 内核和 rootfs 块设备。OCI 镜像需经上层转换，支持 VM 快照 | 官方 **≤125 ms**，从 `InstanceStart` 到 guest `/sbin/init`，使用精简 VM | 同时运行数取决于宿主机。官方精简配置约 **5 个 microVM/物理核/秒** | [Miles + OpenEnv，经 AgentENV](https://github.com/radixark/miles/blob/main/examples/experimental/agentenv/README.md) |
| **E2B Cloud** | 托管 API。底层 runtime、控制面和 SDK 为 Apache-2.0 | Firecracker microVM | 从 Docker 镜像或 Dockerfile 构建模板，随后从预启动 VM 快照创建实例 | 官方客户案例约 **150 ms**，未披露延迟分位数 | **Hobby 20、Pro 100–1,100、Enterprise 1,100+**。Hobby / Pro 默认创建限速分别为 **1 / 5 次每秒** | [Uni-Agent + verl](https://uni-agent.readthedocs.io/en/latest/quickstart/amd-training.html)、[Open-R1](https://github.com/huggingface/open-r1#-training-with-a-code-interpreter)、[Miles + Harbor](https://github.com/radixark/miles/releases)、[SkyRL + Harbor](https://github.com/NovaSky-AI/SkyRL/blob/main/docs/content/docs/harbor/index.mdx) |
| **E2B Embed** | 自托管完整服务，Apache-2.0 | 自备 Linux/KVM 宿主机运行 Firecracker microVM | 同样用 E2B 模板和快照，模板由自建服务准备 | 未见可直接引用的单 sandbox 启动数据 | 官方在一台 arm64 裸机验证 **64 个并发沙箱**。实际容量取决于部署 | 暂未核实明确指定 Embed 的公开 RL 框架配方。[部署与并发验证](https://github.com/e2b-dev/runtime/blob/main/embed/compose/README.md) |
| **Daytona ** | 托管 API。历史服务端版本为 AGPL-3.0，当前核心开发转入私有仓库 | 默认 Linux 容器共享宿主内核。也提供独立内核的 Linux VM | 支持 registry 镜像、声明式 Image 和预构建 snapshot。首次使用新镜像可能先构建 snapshot | 官方公布默认 Linux 容器 **<90 ms**。首次镜像构建和 VM 路径另计 | 同时运行数由组织资源池和单实例规格决定。Tier 1–4 创建限速 **300–600 次/分钟** | [SkyRL + Harbor](https://github.com/NovaSky-AI/SkyRL/blob/main/docs/content/docs/harbor/index.mdx)、[TRL](https://www.daytona.io/docs/en/guides/reinforcement-learning/trl-grpo-training/)、[Miles + Harbor](https://github.com/radixark/miles/releases)、[rLLM](https://github.com/rllm-org/rllm) |
| **Modal ** | 托管 API。SDK 为 Apache-2.0，完整服务端未见公开 | **gVisor 容器**或**独立内核的 Linux VM** | 支持 registry 镜像、Dockerfile、Modal Image 和文件系统快照 | 官方 v2 压测从客户端发起创建到能运行用户代码，**P50 <0.5 s** | Starter **100**、Team **5,000** 个并发容器，Enterprise 自定义 | [Spindle](https://modal.com/docs/examples/swe_gym)、[Uni-Agent](https://github.com/verl-project/uni-agent)、[rLLM](https://github.com/rllm-org/rllm)、[SkyRL + Harbor](https://github.com/NovaSky-AI/SkyRL/blob/main/docs/content/docs/harbor/index.mdx)、[Miles](https://github.com/radixark/miles) |

## 04 训练对于 sandbox 的要求

Agentic LLM 模型的训练和 infra 调度算法天然对 sandbox 提出了新的要求。通过工程经验，我们将其总结成了如下几点。

* resume & save：可以对 sandbox 进行状态保存和镜像暂停。这对于调度层的 prewarm 策略和 colocate 策略很重要。这也利于未来的一些特性。

* snapshot：可以高性能地保存当前的系统快照（包括硬件资源的状态和文件系统的状态）。这将利于我们在进行更加快速、可复性、可归档的数据分析。

* quickstart：sandbox 从发布配置到启动越快越好。这将利于减小部分调度层的 bubble（在 DeepSeek async 调度的文章中我们会提到）。

* 完整的网络控制：sandbox 需要允许外部控制它的网络配置，比如有些题目是通过开源项目被造出来的，那么就必须禁用 Agentic LLM 对于特定域名的访问，甚至设置黑白名单。这将利于更加系统化的 anti-hack。

* 完整的权限模型：无论是造题还是 rl，我们发现单单只是一个 sandbox 作为局外配置远远不够。一个完整的 agent runtime 包含：sandbox、镜像、workspace、agent harness CLI、上游模型、配置（需要多少内存、需要多少时间、最大步数多少等等）。为了满足这个需求，sandbox 需要能够提供不同等级的权限角色，高权限角色进行的初始化对于低权限的真正执行 rollout 的 agent harness 而言是不可见的。

* 负载均衡的 sandbox router：sandbox 本身应该包含 sandbox router 和 sandbox container 两个组件，前者负责负载均衡，后者承受实际 rollout 负载和进行横向拓展。router 部分的负载均衡算法会是新的研究点。

目前各个大厂有的选择使用商业化的 sandbox，比如上面提到的 Daytona 和 Modal，实力强悍的 infra 团队会选择自己对成熟的项目，比如 docker，firecracker 进行魔改。比如 DeepSeek 内部就对 docker 进行了魔改。

---

## 附录：各大 sandbox 的 python sdk

代码是了解运作信息密度最高的语言。

### Docker

安装：`pip install docker`

创建示例：

```python
import docker

client = docker.from_env()
output = client.containers.run(
    "ubuntu:24.04",
    ["sh", "-c", "echo ready"],
    nano_cpus=2_000_000_000,
    mem_limit="2g",
    remove=True,
)
print(output.decode(), end="")
```

### Apptainer

apptainer 没有提供 python sdk，可以通过 CLI 使用，不过需要先准备 sif 镜像：

```bash
apptainer pull ubuntu.sif docker://ubuntu:24.04
```

然后执行：

```python
import subprocess

subprocess.run(
    ["apptainer", "exec", "--containall", "ubuntu.sif", "sh", "-c", "echo ready"],
    check=True,
)
```

### Firecracker

先启动 `firecracker --api-sock /tmp/firecracker.sock`，准备 `vmlinux` 和该 VM 专用的 `rootfs.ext4`

然后执行：

```python
from pathlib import Path
from urllib.parse import quote

import requests_unixsocket  # pip install requests-unixsocket

base = "http+unix://" + quote("/tmp/firecracker.sock", safe="")

with requests_unixsocket.Session() as session:
    def put(path, payload):
        response = session.put(base + path, json=payload, timeout=5)
        response.raise_for_status()

    put("/machine-config", {"vcpu_count": 2, "mem_size_mib": 2048})
    put("/boot-source", {
        "kernel_image_path": str(Path("vmlinux").resolve()),
        "boot_args": "console=ttyS0 reboot=k panic=1 pci=off",
    })
    put("/drives/rootfs", {
        "drive_id": "rootfs",
        "path_on_host": str(Path("rootfs.ext4").resolve()),
        "is_root_device": True,
        "is_read_only": False,
    })
    put("/actions", {"action_type": "InstanceStart"})
```

### E2B

安装：`pip install e2b`

E2B 官方提供了云服务器 E2B Cloud 和自持服务器 E2B Embed 两种，两者用同一个 Python 接口。

示例代码：

```python
from e2b import Sandbox, Template

Template.build(
    Template().from_image("ubuntu:24.04"),
    "rollout-ubuntu",
    cpu_count=2,
    memory_mb=2048,
)

with Sandbox.create("rollout-ubuntu", timeout=300) as sandbox:
    print(sandbox.commands.run("echo ready").stdout, end="")
```

### Daytona

安装：`pip install daytona`

需要先设置环境变量`DAYTONA_API_KEY`，示例代码：

```python
from daytona import CreateSandboxFromImageParams, Daytona, Resources

daytona = Daytona()
sandbox = daytona.create(
    CreateSandboxFromImageParams(
        image="ubuntu:24.04",
        resources=Resources(cpu=2, memory=2, disk=10),
    )
)
try:
    print(sandbox.process.exec("echo ready").result)
finally:
    daytona.delete(sandbox)
```

### Modal

安装： `pip install modal`

示例代码：

```python
import modal

app = modal.App.lookup("rollout-demo", create_if_missing=True)
sandbox = modal.Sandbox.create(
    app=app,
    image=modal.Image.from_registry("ubuntu:24.04"),
    cpu=2.0,
    memory=2048,
    timeout=300,
)
try:
    process = sandbox.exec("sh", "-c", "echo ready")
    print(process.stdout.read(), end="")
    process.wait()
finally:
    sandbox.terminate()
```
