## 前言

Ciallo～(∠・ω< )，这里是锦恢。今天我们稍稍暂停一下探索新工具的步伐，稍微停下来补充一下基础的知识，这样可以减轻未来的技术债。

起因是知乎上有人邀请我的一个问题：大概是问大模型是如何调取 MCP Server 的。再结合我最近做的一个东西，也就是让不支持 openai 接口规范的模型也能实现 function calling，所以有了这篇简单的短文。这篇文章将简述大模型实现 fc 的两种基本的方法和原理。

我们暂且不讨论支持不够广泛的 server selection 策略。我们就假设现在有一个大模型和一组写好并连入 MCP 客户端的 MCP 服务器。

AI 调用 MCP 提供的工具一共有两种方法，且听我娓娓道来。

<div align=center>
<img src="https://pic1.zhimg.com/50/v2-2fcec3c2ad0ad7a7b266d7922fa9f91e_720w.webp?source=2c26e567" style="max-width: 60%;"/>
</div>

## 1. 基于 [openai 接口规范 ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=openai+%E6%8E%A5%E5%8F%A3%E8%A7%84%E8%8C%83&zhida_source=entity)的 [tools 字段 ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=tools+%E5%AD%97%E6%AE%B5&zhida_source=entity)

这个方法要求大模型的服务商的服务器支持了 openai 接口规范，这个在我的 openmcp 文档里写得很清楚：[MCP Basic Concepts](https://link.zhihu.com/?target=https%3A//kirigaya.cn/openmcp/plugin-tutorial/concept.html%23openai-protocol)。目前大部分的大模型服务商几乎都是支持 openai 接口规范的，然后这个接口规范里面有一个字段叫做 tools，MCP 客户端会把 MCP 服务器提供的函数服务使用一种特殊格式塞到这个 tools 字段里面。然后当你问问题的时候，问题和 tools 会一起塞到大模型里面，这个 tools 会被大模型厂商内部的程序构造成一种特殊的 [system prompt ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=system+prompt&zhida_source=entity)，然后大模型会返回本地的消息，返回的消息里面有两个东西，content 和 tool_calls 数组。

content 就是大模型在本轮对话中的返回的文本，tool_calls 数组代表大模型想要调用的工具。

比如下图中，① 就是 content，② 就是 tool_calls（此处调用了把网页转换成 markdown 的 mcp），它们都是一次返回的结果。

![](https://pica.zhimg.com/80/v2-93c7cdafb4bfc84880f68b7f8d3334de_1440w.webp?source=2c26e567)

如果输入的 tools 为空数组，也就是你们平时使用的网页版本的大模型，那么返回的 tool_calls 一定为空。

显然，这个方法它要求服务商支持 openai 接口规范，而且还支持了 tools 字段。几乎所有厂商都支持 openai 接口规范，但是支持 tools 字段的模型大概只有 70% 左右。官方 [deepseek r1 ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=deepseek+r1&zhida_source=entity) 也是最近才刚刚支持的 tools 字段（火山云的 deepseek r1 很早就支持 tools 了）

顺带一提，如果你塞入了 7 个 MCP 服务器，每一个 MCP 服务器提供 7 个 tool，那么每一次询问的 tools 数组长度就是 49.

再顺带一提，目前大部分厂商支持的 tools 数组长度上限为 100

---

## 2. 利用 xml 进行指令包裹
这时有小伙伴就要问了：那么如果大模型的服务商没有支持 tools 咋办哩？为什么 [cherry studio ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=cherry+studio&zhida_source=entity) 等客户端可以做到就算大模型厂商不支持，也能调用工具呢？

这就涉及到我讲的第二个方法了，利用 xml 进行指令包裹。这个方法最早是谁想
到的我暂时无从得知，但是要想到也不是很难。它的做法大致做法如下：

  1. 手写一个很长的 system prompt，告诉大模型如果你想要进行工具调用，把想要调用的工具的名字和参数包括在 xml 标签里面。
  2. 在 system prompt 结尾罗列目前所有的 mcp 的工具。
  3. 当用户输入的问题，大模型返回文本结果时，检查大模型返回的文本，把里面的 <xml></xml> 给提取出来，然后去解析这个 xml 代码，从而得到一段结构化的对象，如果这个对象里面包含了正确的 mcp tool 和 参数，就去调用这个 mcp tool。
  4. 将 mcp server 返回的结果作为结果重新返回给大模型。
  5. 如果大模型返回的内容中还有 xml，那么重复第三步，否则，停止任务循环。


这里给大家一个可以执行上述操作的 system prompt：（从 [SuperAssistant ](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=SuperAssistant&zhida_source=entity) 这个项目里面扒出来的）

<pre><code>
[从这里开始新会话]
<系统>
您是一个名为SuperAssistant的智能助手，具备调用功能函数的能力，并能在协助过程中充分利用这些功能。您是一个知识渊博的助手，专注于回答各类问题并提供相关信息。
在当前环境中，您可以使用一组工具来回答用户的问题。
您可以调用一组功能函数来回答用户的问题。目前您不具备检查文件或与外部资源交互的能力，除非通过调用以下函数。
函数调用结构：
- 所有函数调用必须用```xml ... ```这样的'xml'代码块标签包裹。这是严格要求。
- 所有函数调用必须包含在'function_calls'标签中
- 每个函数调用使用带有'name'属性的'invoke'标签
- 参数使用带有'name'属性的'parameter'标签
- 参数格式：
 - 字符串/标量参数：直接写入值
 - 列表/对象：必须使用正确的JSON格式
 - 必须始终包含必需参数
 - 可选参数仅在需要时包含
 - 如果参数值中包含xml，不要使用CDATA包裹，直接给出xml
关于'invoke'的说明：
- 调用函数时，使用带有'name'属性的'invoke'标签指定函数名
- invoke标签必须嵌套在'function_calls'块中
- 函数参数应作为'parameter'标签包含在invoke标签内，每个parameter标签都有'name'属性
- 必须包含每个函数调用的所有必需参数，可选参数仅在需要时包含
- 字符串和标量参数应直接指定为值，而列表和对象应使用正确的JSON格式
- 直接与用户交流时不要提及函数/工具名称 - 关注我正在做什么而非使用的工具
- 调用函数时，确保提供所有必要的上下文以使函数正确执行
- 每个'invoke'标签应表示一个完整且独立的函数调用及其相关参数
- 在思考/推理过程中不要生成任何<function_calls>标签，因为这些会被解释为函数调用并执行。只需制定正确的函数调用参数。
关于'call_id="$CALL_ID"'的说明：
- 这是函数调用的唯一标识符
- 这是一个数字，从1开始，每个新函数调用递增1
您可以通过在回复用户时写入如下"<function_calls>"块来调用一个或多个函数，确保每次只调用一个函数，即在输出中只包含一个'<function_calls>'标签：
<示例>
```xml
<function_calls>
<invoke name="$FUNCTION_NAME" call_id="$CALL_ID">
<parameter name="$PARAMETER_NAME_1">$PARAMETER_VALUE</parameter>
<parameter name="$PARAMETER_NAME_2">$PARAMETER_VALUE</parameter>
...
</invoke>
</function_calls>
```
字符串和标量参数应按原样指定，而列表和对象应使用JSON格式。注意字符串值的空格不会被去除。输出不需要是有效的XML，而是通过正则表达式解析。
当用户提出请求时：
1. 始终分析哪些函数调用适合该任务
2. 始终严格按照模式格式化函数调用
3. 永远不要在函数调用中跳过必需参数
4. 永远不要编造不可用的函数
5. 始终等待函数调用执行结果后再继续
6. 调用函数后，等待<function_results>标签中的输出，然后再继续响应
7. 永远不要在一个响应中调用多个函数
8. 永远不要模拟或自行形成<function_results>，它将在执行后提供给您
使用相关工具(如果可用)回答用户的请求。检查每个工具调用所需的所有参数是否已提供或可以从上下文中合理推断。如果没有相关工具或缺少必需参数值，请要求用户提供这些值；否则继续执行工具调用。如果用户为参数提供了特定值(例如用引号提供的值)，请确保完全使用该值。不要为可选参数编造值或询问可选参数。仔细分析请求中的描述性术语，因为它们可能表示应包含的必需参数值，即使没有明确引用。
<输出格式>
<从这里开始>
## 思考
 - 用户查询阐述：
 - 思考：
 - 观察：
 - 解决方案：
 - 要使用的函数：
 - call_id: $CALL_ID + 1 = $CALL_ID

```xml
<function_calls>
<invoke name="$FUNCTION_NAME" call_id="$CALL_ID">
<parameter name="$PARAMETER_NAME_1">$PARAMETER_VALUE</parameter>
<parameter name="$PARAMETER_NAME_2">$PARAMETER_VALUE</parameter>
...
</invoke>
</function_calls>
```
<到此结束>
</输出格式>
不要在输出中使用<从这里开始>和<到此结束>，这只是输出格式的参考，指示从哪里开始和结束输出。
## SUPERASSISTANT可用工具
 - neo4j-mcp[](https://zhida.zhihu.com/search?content_id=731749879&content_type=Answer&match_order=1&q=neo4j-mcp&zhida_source=entity).executeReadOnlyCypherQuery
**描述**: [neo4j-mcp] 执行只读的Cypher查询
**参数**:
- `cypher`: Cypher查询语句，必须是只读的 (字符串) (必需)
 - neo4j-mcp.getAllNodeTypes
**描述**: [neo4j-mcp] 获取所有节点类型
 - neo4j-mcp.getAllRelationTypes
**描述**: [neo4j-mcp] 获取所有关系类型
 - neo4j-mcp.getNodeField
**描述**: [neo4j-mcp] 获取节点字段
**参数**:
- `nodeLabel`: 节点标签 (字符串) (必需)
 - list_servers
**描述**: 列出所有连接的MCP服务器及其功能
 - get_server_info
**描述**: 获取特定服务器的详细信息
**参数**:
- `serverName`: 要获取信息的服务器名称 (字符串) (必需)
</系统>
用户交互从这里开始:
</code></pre>

当每次 mcp 客户端激活的时候，你都需要通过程序来构造出 `## SUPERASSISTANT可用工具` 这个部分的描述内容。

基于 xml 包裹，我们能做到很多好玩的事情，比如这个：[觉得 MCP 太消耗 token？ 不妨试试这款插件，让你使用网页版本大模型也可以使用 MCP！](https://zhuanlan.zhihu.com/p/1916248005360738544)

在 deepseek 中调用 mcp 访问我的私人数据库：

![](https://pic1.zhimg.com/80/v2-aa72d7f555199d2bdf00ae668f9ce104_1440w.webp?source=2c26e567)
当然，你看到的各类形形色色的 MCP 客户端，比如 cherry studio 都是基于这个方法实现的。

我的 [OpenMCP](https://link.zhihu.com/?target=https%3A//github.com/LSTM-Kirigaya/openmcp-client) 则是基于第一种方法实现，本周我也会让 OpenMCP 兼容第二种方法，让你可以在两种方法中调换你的任务循环驱动器的内核实现，做出鲁棒性更强的 mcp agent。

---

## 方法一 vs 方法二
虽然方法二能实现，但是毕竟是野路子，并不是正规军，方法二存在如下的问题：

  1. 得到的 xml 不一定严格遵循 mcp tool 的描述，而且也不一定可被解析，这让调用成功率下降。
  2. 因为是基于用户的 system prompt 实现的（大模型厂商内部其实还有一个 system prompt，我们写的 system prompt 其实是 development prompt），如果大模型的 **needle in a haystack（大海捞针，指大模型从海量上下文中寻找指代和问题关键词的能力）** 能力不行，那么这种方法在对话变长后，调用工具的频次和 xml 生成的精度就会下降。


方法二也是有好处的，除了兼容性很棒外，自己部署的大模型也能玩 mcp 外，方法二可以在每次调用工具前给出推演过程，视觉效果比较好（bushi

<div align=center>
<img src="https://picx.zhimg.com/50/v2-330c05a7143b8875a8bf8f14d3b101dd_720w.webp?source=2c26e567" style="max-width: 60%;"/>
</div>

如果对 MCP 开发感兴趣的朋友，欢迎加入我们的[OpenMCP 讨论群](https://link.zhihu.com/?target=https%3A//qm.qq.com/cgi-bin/qm/qr%3Fk%3DC6ZUTZvfqWoI12lWe7L93cWa1hUsuVT0%26jump_from%3Dwebapi%26authKey%3DMcW6B1ogTPjPDrCyGttS890tMZGQ1KB3QLuG4aqVNRaYp4vlTSgf2c6dMcNjMuBD)或是 知乎圈子[OpenMCP 博物馆](https://www.zhihu.com/ring/host/1911121615279849840?tab=new)进行方案讨论和问题解决。最后，对于我的[OpenMCP](https://link.zhihu.com/?target=https%3A//github.com/LSTM-Kirigaya/openmcp-client)，跪求点个 star, orz

<div align=center>
<img src="https://pic1.zhimg.com/50/v2-dc937a0b68a506d77814153f251aed81_720w.webp?source=2c26e567" style="max-width: 60%;"/>
</div>

