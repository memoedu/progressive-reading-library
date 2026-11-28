前置技术知识点：[PTC 设计原则与 Agent Harness 标准架构](https://kirigaya.cn/blog/article?seq=394)

## 问题背景

在绝大部分的 Agent 系统中，Agent 通过发起 OpenAI 协议格式的 LLM Call 完成大模型推理，然后在 harness 中完成 agent loop。这样的设计能符合业务需求，但是无法彻底满足训练和开发需求。

根本原因在于 subagent 和 compact 这两个特性的引入，使得得到的 agent trajectory 从拓扑上不再是一条链。原本的朴素的 ReAct 得到的一条 trajectory 可能是这样：

![图片](https://kirigaya.cn/api/storage/image/7dc9516c-52f7-4fe4-aa04-fc2c9ae24860.webp)

然后在大模型训练和评测中，由于参与 rollout 的 harness 往往是黑盒的，而非白盒的，所以规范化收集这些数据只能通过 LLM Call 信息来获取。上面这条 trajectory 的最后一个 assistant 对应的 LLM Call 的输入——OpenAI 协议的请求体是：

```json
[
  {
    "role": "user",
    "content": "查询北京今天的天气，并判断是否适合户外跑步。"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "call_weather_001",
        "type": "function",
        "function": {
          "name": "get_weather",
          "arguments": "{\"city\":\"北京\"}"
        }
      }
    ]
  },
  {
    "role": "tool",
    "tool_call_id": "call_weather_001",
    "content": "{\"weather\":\"晴\",\"temperature\":26,\"humidity\":40}"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "call_air_quality_001",
        "type": "function",
        "function": {
          "name": "get_air_quality",
          "arguments": "{\"city\":\"北京\"}"
        }
      }
    ]
  },
  {
    "role": "tool",
    "tool_call_id": "call_air_quality_001",
    "content": "{\"aqi\":45,\"level\":\"优\"}"
  },
  {
    "role": "assistant",
    "content": "北京今天晴，26°C，湿度为 40%，空气质量为优，适合户外跑步。"
  }
]
```

嗯，你会发现，这两个玩意儿完全一样，这意味着我们可以从 LLM Call 输入的 messages 中完整还原出原本的 Agent 的 trajectory，从而根据 rollout 时拦截到的 logprob，mask 等数据来执行 PPO 或者 GRPO，优雅美丽地完成一次 RL。

但是如果当前存在 subagent 或者一次 compact，那么 LLM Call 输入的 messages 就不能完整还原出原本的 Agent 的 trajectory 了，我们先看 subagent 的例子。

### corner case：subagent

假设我们举一个让 agent 开设子智能体完成系统信息搜集然后再回答用户技术问题的例子。这个例子中，用户会询问 agent “redis 和 memcached 哪个更适合做登录会话的存储方案”，此时主智能体就会使用 task 工具开启子智能体，并在子智能体中调用不同的工具获取 redis 和 memcached 的技术方案信息，然后汇总得到结果 “redis 更适合”，再返回这个判断文本。主智能体最终根据子智能体的判断，综合回答用户的问题。

这个例子的 agent 执行拓扑如下：

![图片](https://kirigaya.cn/api/storage/image/aec245d3-124d-49c3-bc4f-af10f44b3c0c.webp)

我们再看一下主智能体的最后一条 LLM Call 的输入请求体：

```json
[
  {
    "role": "user",
    "content": "比较 Redis 和 Memcached，并告诉我哪个更适合用来存储用户登录会话。"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "call_task_001",
        "type": "function",
        "function": {
          "name": "task",
          "arguments": "{\"prompt\":\"请独立调研 Redis 和 Memcached 的功能、数据结构、持久化、高可用能力以及会话存储适用性，最后返回一份简洁的比较结论。\"}"
        }
      }
    ]
  },
  {
    "role": "tool",
    "tool_call_id": "call_task_001",
    "content": "Redis 支持字符串、哈希、列表、集合等多种数据结构，支持持久化、主从复制、哨兵和集群，并且可以通过 TTL 自动清理过期会话。Memcached 主要提供简单的内存键值缓存，不支持持久化，数据结构和高可用能力也更有限。对于需要保存用户登录状态、设置过期时间并考虑故障恢复的场景，Redis 通常比 Memcached 更合适。"
  }
]
```

你会发现，由于 task 只是一个工具，虽然它确实独立做了一次 ReAct，但是不会体现在最终的 messages 数组里面的，messages 数组永远只是一个线性的结构。基于这个 messages，我们只能还原出这样的结构信息：

![图片](https://kirigaya.cn/api/storage/image/957ff618-ec84-42a3-bce8-9b66e41bd975.webp)

通过这个 messages 数组，我们无从得知 call_task_001 这个 id 的 task 工具内部做了什么。这样，做 PPO 或评测时我们就无法构成输入数据的闭环。

此时你可能会反驳说，如果我们在 LLM 的推理引擎的地方做一个 map 或者 proxy 去拦截每一次请求，并根据前缀或者内部信息去抓取 call_task_001 实际的完整 LLM Call，不就行了吗？

但是这么做有一些工程上非常麻烦的问题。

首先，从 LLM 推理引擎（比如 sglang）的角度去看，call_task_001 这个 subagent 的 ReAct 最终 messages 是这样的：

```json
[
  {
    "role": "system",
    "content": "你是一个独立运行的调研子智能体。请使用可用工具完成任务，并向主智能体返回简洁、准确的最终结论。"
  },
  {
    "role": "user",
    "content": "请独立调研 Redis 和 Memcached 的功能、数据结构、持久化、高可用能力以及会话存储适用性，最后返回一份简洁的比较结论。"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "call_redis_001",
        "type": "function",
        "function": {
          "name": "get_redis_info",
          "arguments": "{}"
        }
      },
      {
        "id": "call_memcached_001",
        "type": "function",
        "function": {
          "name": "get_memcached_info",
          "arguments": "{}"
        }
      }
    ]
  },
  {
    "role": "tool",
    "tool_call_id": "call_redis_001",
    "content": "{\"data_structures\":[\"string\",\"hash\",\"list\",\"set\",\"sorted_set\"],\"ttl\":true,\"persistence\":[\"RDB\",\"AOF\"],\"high_availability\":[\"replication\",\"sentinel\",\"cluster\"]}"
  },
  {
    "role": "tool",
    "tool_call_id": "call_memcached_001",
    "content": "{\"data_structures\":[\"key-value\"],\"ttl\":true,\"persistence\":false,\"high_availability\":\"依赖客户端分片或外部基础设施\"}"
  }
]
```

推理引擎拿到了这个请求后，它是不知道这个东西是上一个智能体发起的子智能体的信息的，推理引擎做 continueous batching 时的基本处理单元只有一条条 LLM Call，它不负责区分它们本身的语义和拓扑上的逻辑关系。

唯一的做法就是，你用 task 工具开启子智能体默认注入的 system prompt + user prompt 去和主智能体发起 task 工具调用的 prompt 字段参数做前缀匹配，实际上，大部分开源的 agentic rl 框架，要么就是直接不管不支持 ppo，要么就是采用了这种前缀匹配的方法去做拓扑还原（比如 NVIDIA 的 Polar），在大部分情况下，这样做确实管用，我们不妨考察一个这样的情况：主智能体一共进行了 300 个 step，然后其中调用了 20 次 task 工具开启了 20 个独立的子智能体，通过前缀匹配加回复的后缀匹配确实可以形成一一映射。

![图片](https://kirigaya.cn/api/storage/image/ddcf57a8-6c4e-4697-a8a6-366dbc36897a.webp)

但是这个做法不仅丑陋，而且蕴含了潜在的风险——这个前缀匹配的做法在推理引擎的视角来看，不能保证唯一性。假设这 20 个 task 调用中，出现了三个 task 调用的输入和输出都是完全一样的（这在一些基础驻守型子任务中会经常出现）：

![图片](https://kirigaya.cn/api/storage/image/0d2c8b87-aae3-4aba-9229-7824d6c31ba6.webp)

其中有三个 task 的输入参数都是 `prompt: "检查校验码"`，输出都是 `123`，那么我们在推理引擎端做关系映射时，唯一性就无法得到保证，我们就不能知道当前的这三个messages 要如何映射到主智能体调用这三个 task 工具的具体内容上，从而组装成完整的 agent 调用拓扑。并且对于 agent 评测也是一样，评测工程师将无法看到完整的拓扑关系并进行 hacking 和 profile 的分析。

这是一种 corner case，也就是 subagent。我们再看下一个。

### corner case：compact

在 [PTC 设计原则与 Agent Harness 标准架构](https://kirigaya.cn/blog/article?seq=394) 中，我们提到了 compact 这个通用 harness 设计，但是有一点我们没提到，那就是 compact 的出现也会破坏原本的拓扑关系。

我们看一下下面这个中途做了上下文压缩的例子：

![图片](https://kirigaya.cn/api/storage/image/f273ad32-c020-4120-8b99-5ab896f54d72.webp)

在这个例子中，主智能体最终一次 LLM Call 的 messages 如下所示：

```json
[
  {
    "role": "system",
    "content": "你是一个能够使用命令行工具解决问题的智能体。"
  },
  {
    "role": "user",
    "content": "[Compact Summary]\n用户要求排查服务启动失败。此前已查看服务日志，发现服务因端口 8080 被占用而无法启动。下一步需要修改服务端口并验证服务能否正常启动。"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "call_bash_002",
        "type": "function",
        "function": {
          "name": "bash",
          "arguments": "{\"command\":\"sed -i 's/port = 8080/port = 8081/' app.conf && ./restart.sh && curl -f http://localhost:8081/health\"}"
        }
      }
    ]
  },
  {
    "role": "tool",
    "tool_call_id": "call_bash_002",
    "content": "{\"exit_code\":0,\"stdout\":\"service restarted\\nhealth check: ok\"}"
  }
]
```

可以看到，压缩前的执行链路变成了 `[Compact Summary]` 开头的一句话，我们无法根据这句话完整还原出压缩前的执行链路。

所以 compact 也会破坏原本的 agent 执行拓扑。

我们不清楚未来的 Agent 会如何设计，但是长程任务执行下的 Agent 拓扑，大概率在未来也不会是一条链这种简单的结构，对于含有层级的拓扑还原这个需求是会一直存在的。

---

## ATIF 协议

为了解决这个问题，我们需要一套规范的数据协议，用于根据 messages 信息重建出完整的 Agent 调用拓扑，Coding Agent 知名 Benchmark Terminal 系列的团队 harbor 推出了智能体轨迹交换协议（Agent Trajectory Interchange Format，简称 ATIF 协议）来对 sandbox 或推理引擎的 proxy 模块捕获的 LLM Call 数据的存储进行标准化。

该标准已经被 NVIDIA 采用，并在社区内出现了各类配套设施和阅览器，这对于 agent 评测至关重要；也可以和适配了 ATIF 协议的 harness 做 rollout，获取完整的轨迹信息执行 PPO 等 RL 训练，这可以避免你在使用一些开源 agentic rl 做训练时用你那蹩脚的工程直觉指挥 codex 写出一堆屎山代码。

ATIF 的官方文档在：[Agent Trajectory Format (ATIF)](https://www.harborframework.com/docs/agents/trajectory-format)

我们只需要先安装 harbor 的 sdk，然后就可以使用 harbor 为 ATIF 已经封装好的一组 pydantic 定义的数据模型就能接入这个规范了。

### ATIF 表示一次简单的 Agent Loop

比如正常的一次 trajectory 的数据模型被定义为：

```python
from harbor.models.trajectories import Trajectory, Agent, Step

trajectory = Trajectory(
    schema_version="ATIF-v1.8",
    session_id="session-123",
    agent=Agent(
        name="my-agent",
        version="1.0.0",
        model_name="claude-3-5-sonnet-20241022"
    ),
    steps=[
        # ... steps
		user_step,
		agent_step
    ]
)
```

你能看到，这个定义相当完备，agent 的名称，版本，模型名，session id 什么的都有定义，这些定义可以方便你在记录下这条 trajectory 后完备地重建出你所需要的任何信息。这些都算是一条 trajectory 的元信息。

具体记录的 steps 信息的定义如下：

```python
from harbor.models.trajectories import Step

# User step
user_step = Step(
    step_id=1,
    timestamp="2025-01-15T10:30:00Z",
    source="user",
    message="Create a file called hello.txt with 'Hello, world!' as the content."
)

# Agent step with tool calls
agent_step = Step(
    step_id=2,
    timestamp="2025-01-15T10:30:02Z",
    source="agent",
    model_name="claude-3-5-sonnet-20241022",
    message="I'll create the file for you.",
	reasoning_effort="medium",
    reasoning_content="The user wants a simple text file. I'll use the file_write tool.",
    tool_calls=[
        ToolCall(
            tool_call_id="call_1",
            function_name="file_write",
            arguments={"path": "hello.txt", "content": "Hello, world!"}
        )
    ],
    observation=Observation(
        results=[
            ObservationResult(
                source_call_id="call_1",
                content="File created successfully"
            )
        ]
    ),
    metrics=Metrics(
        prompt_tokens=520,
        completion_tokens=80,
        cached_tokens=200,
        cost_usd=0.00045
    )
)
```

用户消息比较简单，记录一下时间和 user prompt 就行。agent 执行 thought，action，observation 也就被浓缩为一个 agent_step 里面，thought 对应 reasoning_content，在标准的 ReAct 中，往往是没有 message 的，此处官方是为了方便展示 ATIF 的表达效果，所以加了这个，真实场景下执行 ReAct 时的这个 message 应该是空的状态；action 对应 tool_calls；observation 对应 observation。

想要把这些数据模型的全部或者任意一一个保存为 json，也非常简单，因为 ATIF 的全部数据模型都是通过 pydantic 定义的，所以使用 pydantic 的 `to_json_dict` 就能转换为 dict 对象，然后再使用 json 库就能导出为 json。

### ATIF 表示一次含 subagent 的 Agent Loop

对于含有 subagent 的更加复杂的 Agent Loop，ATIF 也能做到完备表示。假设我们让子智能体解决一道数学题，那么主智能体的轨迹就是调用 task 启动一个子智能体解决问题，使用 ATIF 描述主智能体的过程就是：

```python
parent = Trajectory(
    schema_version="ATIF-v1.8",
    session_id="run-001",
    trajectory_id="trajectory-parent-001",
    agent=Agent(
        name="parent-agent",
        version="1.0.0",
        model_name="qmodel",
    ),
    steps=[
        Step(
            step_id=1,
            source="user",
            message="计算前 20 个正整数之和",
        ),
        Step(
            step_id=2,
            source="agent",
            message="委托数学 subagent 计算。",
            tool_calls=[
                ToolCall(
                    tool_call_id="call-001",
                    function_name="task",
                    arguments={
                        "prompt": "计算 1 到 20 的和",
                    },
                )
            ],
            observation=Observation(
                results=[
                    ObservationResult(
                        source_call_id="call-001",
                        content="计算结果为 210",
                        subagent_trajectory_ref=[
                            SubagentTrajectoryRef(
                                trajectory_id="trajectory-subagent-001",
                                session_id="run-001",
                            )
                        ],
                    )
                ]
            ),
            llm_call_count=1,
        ),
    ],
    subagent_trajectories=[subagent],
)
```

通过 ObservationResult 的 subagent_trajectory_ref 字段，我们就可以将当前这个 observation 片段对应的子智能体的数据模型进行关联，这种关联通过两步完成：

1. 将完整的子智能体的轨迹通过 ATIF 数据模型表达后，加入到全局的 subagent_trajectories 数组。

2. 关联子智能体轨迹的 observation 片段通过 trajectory_id 来具体关联到 subagent_trajectories 数组中对应的子智能体的轨迹中。

我们就能再去定义 subagent 的轨迹了：

```python
subagent = Trajectory(
    schema_version="ATIF-v1.8",
    session_id="run-001",
    trajectory_id="trajectory-subagent-001",
    agent=Agent(
        name="math-subagent",
        version="1.0.0",
        model_name="qmodel",
    ),
    steps=[
        Step(
            step_id=1,
            source="user",
            message="计算 1 到 20 的和",
        ),
        Step(
            step_id=2,
            source="agent",
            message="20 × 21 ÷ 2 = 210",
            llm_call_count=1,
        ),
    ],
)
```

### ATIF 表示一次含有 compact 的 Agent Loop

表示之前，我们需要先明确一下，在 opencode，claude code，deepagents-sdk 等主流 agent 或 agent sdk 中，compact 往往是被设计为一个系统事件，在一些其他的 agent 中，它的 compact 是被设计为一个 subagent + new session 的。这两种 compact 设计的 ATIF 表示都不一样。我们此处只讨论前者。

ATIF 中，Step 数据模型预留了 source 字段用来描述一个 step 的来源，它可以是 user，代表一个新的 turn 的用户的输入；也可以代表 agent，代表这是 agent 的行为，比如回答你的问题，或者执行 ReAct；source 也能设置为 system，代表一次动用了模型和上下文有关的系统事件，比如此处的 compact。

以 opencode 为例，表示它的一次含有 compact 的 Agent Loop 的 trajectory 可以表示如下：

```json
from harbor.models.trajectories import (
    Agent,
    Observation,
    ObservationResult,
    Step,
    Trajectory,
)


checkpoint = (
    "<summary>\n"
    "目标：修复 refresh token 并补充测试。\n"
    "已确认：src/auth.py 存在并发刷新竞态。\n"
    "约束：保持公开 API 不变。\n"
    "下一步：加入互斥保护并补充并发测试。\n"
    "</summary>\n\n"
    "<recent>\n"
    "最近上下文：已完成问题定位，但尚未修改文件。\n"
    "</recent>"
)


trajectory = Trajectory(
    schema_version="ATIF-v1.8",
    session_id="run-opencode-001",
    trajectory_id="trajectory-opencode-main-001",
    agent=Agent(
        name="opencode",
        version="1.x",
        model_name="example-model",
    ),
    steps=[
        Step(
            step_id=1,
            source="system",
            message="You are a coding agent.",
        ),
        Step(
            step_id=2,
            source="user",
            message="修复 refresh token 的并发问题并补充测试。",
        ),
        Step(
            step_id=3,
            source="agent",
            model_name="example-model",
            message="已定位 src/auth.py 中的并发竞态。",
            llm_call_count=1,
        ),

        # OpenCode compact 系统事件
        Step(
            step_id=4,
            source="system",
            message="OpenCode context compaction completed",
            observation=Observation(
                results=[
                    ObservationResult(
                        # compact 后真正提供给模型的 checkpoint
                        content=checkpoint,
                    )
                ]
            ),

            # 这个系统操作内部执行了一次摘要 LLM call。
            # 因为没有展开成单独 trajectory，所以计入当前 system step。
            llm_call_count=1,

            extra={
                # ATIF 标准约定
                "context_management": {
                    "type": "compaction",
                    "boundary": "replace",
                },

                # OpenCode 特有信息
                "native": {
                    "system": "opencode",
                    "trigger": "auto",
                    "started_event": "session.compaction.started",
                    "ended_event": "session.compaction.ended",
                    "summary_generator": "hidden compaction system agent",
                    "summary_generator_mode": "primary",
                    "durable_prior_messages_deleted": False,
                    "effective_checkpoint_parts": [
                        "structured_summary",
                        "serialized_recent_tail",
                    ],
                },
            },
        ),

        # Compact 后继续执行
        Step(
            step_id=5,
            source="agent",
            model_name="example-model",
            message="根据 compact checkpoint，开始加入互斥保护。",
            llm_call_count=1,
        ),
        Step(
            step_id=6,
            source="agent",
            model_name="example-model",
            message="修改完成，并通过并发刷新测试。",
            llm_call_count=1,
        ),
    ],

    # 不应包含 subagent_trajectories
)
```

---

## ATIF 可视化工具

使用标准协议的好处就是可以无缝享受社区的生态。作为一项早期的协议，目前社区已经为 ATIF 支持了对应的基础设施，用于提高 Agentic LLM 训练、Agent/LLM 评测和 Agent 部署的可观测性。下面就给大家罗列几个。

### vscode 插件：ATIF Preview

插件安装地址：[ATIF Preview](https://marketplace.visualstudio.com/items?itemName=waldek.atif-visualizer)

对于符合 ATIF 协议并以 atif.json 结尾的文件，可以通过点击右上角的按钮直接可视化 trajectory，并行进行过滤，独立 agent 分组查询等功能。很适合独立分析单个的 agent trajectory。

![图片](https://kirigaya.cn/api/storage/image/9ea50b05-fcea-4c62-905b-00f9d9b384a3.webp)

### Harbor

作为 ATIF 的规范维护者，harbor 本身就提供了针对 atif 的阅览器，通过如下命令就可以打开：

```
harbor view jobs
```

效果如下：

![图片](https://kirigaya.cn/api/storage/image/f14bbc90-f411-4ed1-89b5-de3c5eba1a0d.webp)

### ATIF Trajectory Viewer

社区开源项目，地址为：[Slimshilin/ATIF-trajectory-viewer](https://github.com/Slimshilin/ATIF-trajectory-viewer)

基于 ATIF 协议对轨迹进行了可视化，并且还可以预览 Agent 本身执行这次 trajectory 时的工作区。预览图片，音频等会被 agent 处理的多媒体文件。

![图片](https://kirigaya.cn/api/storage/image/4c715594-567d-4cb3-bca6-015f65bb1561.webp)