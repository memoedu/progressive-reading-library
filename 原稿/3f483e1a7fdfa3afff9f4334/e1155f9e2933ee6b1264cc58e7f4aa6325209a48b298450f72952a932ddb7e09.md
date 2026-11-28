## 前言

感谢 Mortal 的分享，langchain 最近发布了一篇讲解 agent test 的技术博客，试图给出一个 agent 测试的工业级的范式框架，我通读了文章，感觉写的很棒，所以简单用 deepseek 做了一个翻译并校对了一下术语，原文在 [Evaluating Deep Agents: Our Learnings](https://x.com/LangChainAI/article/2006589207196930109)，感兴趣的读者推荐阅读原文。

原文中推销了 LangSmith（他们的一款 agent 测试框架）的使用，对在公司做 agent 评测和 agent 系统闭环测试的朋友们应该会很有启发。

下面是翻译后的结果👇

---

在过去的一个月里，我们在 LangChain 基于 Deep Agents 框架推出了四款应用：

* [**DeepAgents CLI**](https://blog.langchain.com/introducing-deepagents-cli/)：一个 coding agent

* **LangSmith Assist**：一个应用内 agent，用于帮助处理 LangSmith 中的各种事务

* **个人电子邮件助手**：一个通过用户交互学习的电子邮件助手

* [**agent 构建器**](https://blog.langchain.com/langsmith-agent-builder-now-in-public-beta/)：一个由元 agent 驱动的无代码agent构建平台

构建并发布这些 agent 意味着需要为每个 agent 添加评估，我们在此过程中学到了很多！在这篇文章中，我们将深入探讨评估deep agent的以下模式。

1. deep agent需要为每个数据点设计定制的测试逻辑——每个测试用例都有其自身的成功标准。

2. 让deep agent运行单步操作非常适合在特定场景下验证决策（同时也能节省token！）。

3. 完整的agent运行轮次非常适合测试关于agent“最终状态”的断言。

4. 多轮agent交互可以模拟真实的用户互动，但需要保持在预期轨道上。

5. 环境设置很重要——deep agent需要干净、可复现的测试环境。

## 术语表

在深入探讨之前，我们先定义本文中将用到的一些术语。

**运行 agent 的方式：**

* **单步（Single Step）**：限制核心agent循环仅运行一轮，以确定agent将要采取的下一步行动。

* **完整轮次（Full Turn）**：让agent在单个输入上完整运行一次，这可能包括多次工具调用迭代。

* **多轮（Multiple Turns）**：让agent完整地运行多次。通常用于模拟agent和用户之间包含多次来回交互的“多轮”对话。

![Image](https://pbs.twimg.com/media/G9jRmh_aMAYOs1F?format=jpg&name=large)

**我们可以测试的内容：**

* **轨迹（Trajectory/Trace）**：agent 调用的工具序列，以及agent生成的特定工具参数，业界一般喜欢简称为 trace，我们后续沿用这个说法。

* **最终响应（Final response）**：agent 返回给用户的最终响应。

* **其他状态（Other state）**：agent 在运行时生成的其他值（例如文件、其他产物）。

![Image](https://pbs.twimg.com/media/G9jRqxNbkAAIcEU?format=jpg&name=large)

## #1：deep agent需要为每个数据点设计更多定制化测试逻辑（代码）

传统的 LLM 评估很直接：

1. 构建一个示例数据集

2. 编写一个评估器

3. 在数据集上运行您的应用程序以产生输出，并使用您的评估器对这些输出进行评分

每个数据点都采用相同的方式处理——通过相同的应用逻辑运行，由相同的评估器评分。 deep agent打破了这个假设。您需要测试的不仅仅是最终消息。“成功标准”也可能更加针对每个数据点，并且可能涉及针对agent轨迹和状态的特定断言。

![Image](https://pbs.twimg.com/media/G9jRwb9a0AA4cGx?format=jpg&name=large)

考虑这个例子： 我们有一个具有记忆用户偏好能力的日历安排 deep agent。用户要求他们的 agent “记住永远不要在上午 9 点前安排会议”。我们希望断言日历安排 agent 在其文件系统中更新了自己的记忆以记住此信息。 为了测试这一点，我们可能希望编写如下的断言来验证：

1. agent 在 `memories.md` 这个文件路径上调用了 `edit_file` 工具。

2. agent 在其最终消息中向用户传达了记忆更新。

3. `memories.md` 文件确实包含了关于不安排早会的信息。您可以：

   * 使用正则表达式查找类似 “上午9点” 这样的字眼。

   * 或者，使用带有特定成功标准的 LLM-as-judge（LLM 作为裁判）来对文件更新进行更全面的分析。

LangSmith 的 Pytest 和 Vitest 集成支持这种类型的定制测试。您可以为每个测试用例对agent的轨迹、最终消息和状态做出不同的断言。

```python
# 标记为 LangSmith 测试用例
@pytest.mark.langsmith
def test_remember_no_early_meetings() -> None:
    user_input = "I don't want any meetings scheduled before 9 AM ET"
    # 我们可以将agent的输入记录到 LangSmith
    t.log_inputs({"question": user_input})

    response = run_agent(user_input)
    # 我们可以将agent的输出记录到 LangSmith
    t.log_outputs({"outputs": response})

    agent_tool_calls = get_agent_tool_calls(response)
    # 我们断言agent调用了 edit_file 工具来更新其记忆
    assert any([tc["name"] == "edit_file" and tc["args"]["path"] == "memories.md" for tc in agent_tool_calls])

    # 我们记录来自 llm-as-judge 的反馈，判断最终消息是否确认了记忆更新
    communicated_to_user = llm_as_judge_A(response)
    t.log_feedback(key="communicated_to_user", score=communicated_to_user)

    # 我们记录来自 llm-as-judge 的反馈，判断记忆文件现在是否包含正确的信息
    memory_updated = llm_as_judge_B(response)
    t.log_feedback(key="memory_updated", score=memory_updated)
```

有关如何使用 Pytest 的通用代码片段，请查看[这些文档](https://docs.langchain.com/langsmith/pytest?ref=blog.langchain.com&ajs_aid=9b757f3a-8cab-47f2-b8f5-67efd14b7577&ajs_uid=ee125993-91df-4563-86e5-d03703accebd#how-to-run-evaluations-with-pytest-beta)。

LangSmith 会自动将所有测试用例记录到一个实验中，因此您可以查看失败测试用例的 trace（以调试出错原因）并跟踪一段时间内的结果。

## #2：单步评估很有价值且高效

![图片](https://kirigaya.cn/api/storage/image/f2985002-4573-4613-8cb8-1a2a4332b299.webp)

在为 agent 运行评估时，我们大约一半的测试用例看起来像单步评估，即在特定的输入消息序列之后，LLM 立即决定做什么。 这对于验证agent在特定场景中是否以正确的参数调用了正确的工具特别有用。常见的测试用例包括：

* 它是否调用了正确的工具来搜索会议时间？

* 它是否检查了正确的目录内容？

* 它是否更新了其记忆？

回归问题通常发生在单个决策点上，而不是完整的执行序列中。如果使用 LangGraph，其流式功能允许您在单次工具调用后中断agent以检查输出——这样您就可以在无需完整agent序列开销的情况下尽早发现问题。 在下面的代码片段中，我们在工具节点之前手动引入一个断点，允许我们轻松地让agent单步运行。然后，我们可以检查该单步之后的状态并做出断言。

```python
@pytest.mark.langsmith
async def test_single_step() -> None:
    state_before_tool_execution = await agent.ainvoke(
        inputs,
        # interrupt_before 指定在哪些节点之前停止
        # 在工具节点之前中断允许我们检查工具调用参数
        interrupt_before=["tools"]
    )
    # 我们可以查看agent的消息历史，包括最新的工具调用
    print(state_before_tool_execution["messages"])
```

## #3：完整的agent轮次让您获得完整图景

将单步评估视为您的“单元测试”，确保 agent 在特定场景下采取预期的行动。同时，完整的 agent 轮次也很有价值——它们向您展示了 agent 端到端行动的完整图景。

完整的 agent 轮次让您以多种方式测试 agent 行为：

1. **轨迹 trace**：评估完整 trace 的一种非常常见的方法是确保在行动过程中的某个时刻调用了特定的工具，但具体何时调用并不重要。在我们的日历调度器示例中，调度器可能需要多次工具调用来找到一个适合所有参与方的合适时间段。

2. **最终响应**：在某些情况下，最终输出的质量比agent采取的具体路径更重要。我们发现对于更开放式的任务（如编码和研究），情况确实如此。

3. **其他状态**：评估其他状态与评估 agent 的最终响应非常相似。有些 agent 会创建产物，而不是以聊天格式响应用户。通过检查 LangGraph 中 agent 的状态，可以轻松检查和测试这些产物。

   * 对于coding agent → 读取并测试 agent 编写的文件。

   * 对于research agent → 断言 agent 找到了正确的链接或来源。

完整的 agent 轮次让您全面了解 agent 的执行情况。LangSmith 使得将完整的 agent 轮次视为追踪变得非常容易，您可以查看延迟和 token 使用等高级指标，同时还可以分析具体的每个步骤，直到每次模型调用或工具调用。

## #4：让 agent 跨多轮运行可模拟完整的用户互动

某些场景需要测试 agent 在具有多个顺序用户输入的多轮对话中的表现。挑战在于，如果您天真地硬编码一系列输入，而 agent 偏离了预期路径，那么后续硬编码的用户输入可能就没有意义了。

我们通过在 Pytest 和 Vitest 测试中添加条件逻辑来解决这个问题。例如，我们会：

* 运行第一轮，然后检查 agent 输出。如果输出符合预期，则运行下一轮。

* 如果不符合预期，则尽早使测试失败。（这是可行的，因为我们有灵活性可以在每个步骤后添加检查。）

这种方法使我们能够运行多轮评估，而无需对每个可能的 agent 分支进行建模。如果我们想单独测试第二轮或第三轮，我们只需从该点开始设置一个具有适当初始状态的测试。

## #5：设置正确的评估环境很重要

agent 是有状态的，旨在处理复杂的长期运行任务——通常需要更复杂的环境来进行评估。

与更简单的 LLM 评估（其环境仅限于少数通常无状态的工具）不同，deep agent 需要为每次评估运行提供一个全新的、干净的环境，以确保结果可重现。

coding agent 清楚地说明了这一点。[Harbor](https://harborframework.com/?ref=blog.langchain.com) 为 TerminalBench 提供了一个在专用 Docker 容器或沙箱内运行的评估环境。对于 DeepAgents CLI，我们采用了一种更轻量级的方法：我们为每个测试用例创建一个临时目录并在其中运行agent。

更广泛的要点是：deep agent 评估需要每个测试都能重置的环境——否则您的评估将变得不稳定且难以复现。

**技巧：模拟您的 API 请求**

LangSmith Assist 需要连接到真实的 LangSmith API。针对实时服务运行评估可能缓慢且昂贵。相反，可以将 HTTP 请求记录到文件系统，并在测试执行期间重放它们。对于 Python，[vcr](https://github.com/vcr/vcr?ref=blog.langchain.com) 效果很好；对于 JS，我们通过 Hono 应用程序代理 fetch 请求。

模拟或重放 API 请求使得deep agent评估更快且更容易调试，尤其是当agent严重依赖外部系统状态时。

## 使用 LangSmith 评估deep agent

上述技术是我们在为基于deep agent的应用程序编写自己的测试套件时看到的常见模式。对于您的特定应用，您可能只需要上述模式的一个子集——因此，您的评估框架具备灵活性非常重要。如果您正在构建deep agent并开始进行评估，请查看 [LangSmith 的测试集成](https://docs.langchain.com/langsmith/pytest?ref=blog.langchain.com&ajs_aid=9b757f3a-8cab-47f2-b8f5-67efd14b7577&ajs_uid=ee125993-91df-4563-86e5-d03703accebd)！