> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/webmcp.md)。

<a id="site-tools"></a>

# 网站工具与 WebMCP

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

站点工具是 ChatGPT 所建议的 [WebMCP标准](https://webmachinelearning.github.io/webmcp/) 的实现。借助 WebMCP，网站可以在人们已经使用的界面旁边直接向 AI 智能体提供有用的操作。您和客服人员可以使用相同的实时页面和登录会话。

在 ChatGPT 桌面应用程序的 [内置浏览器](browser.zh-CN.md) 中，ChatGPT Work 和 Codex 可以发现并使用这些可用的工具。

使用 GPT-5.6 Sol 或 GPT-5.6 Terra 作为站点工具。 GPT-5.6 Luna 目前已禁用 WebMCP。将 ChatGPT 桌面应用程序更新到最新版本。站点工具在 Enterprise 或 Edu 工作区中不可用。可用性还取决于当前页面提供的推出和工具。

<a id="webmcp-vs-mcp"></a>

## WebMCP 与 MCP

[模型上下文协议 (MCP)](https://modelcontextprotocol.io/docs/learn/architecture) 将 AI 应用程序连接到本地或远程服务器。其工具可以独立于打开的网页工作，例如通过 API 搜索服务或管理记录。

[网络MCP](https://github.com/webmachinelearning/webmcp) 允许网站将其功能作为一组预定义工具提供给智能体。智能体可以在访问时发现它们，因此人们不需要安装单独的 MCP 服务器或设置另一个连接来使用这些功能。

当您和智能体需要查看相同的内容时（例如编辑画布或浏览仪表板时），此方法非常有用。 [带有 MCP 服务器的插件](build-plugins.zh-CN.md) 可以提供独立于打开页面工作的集成。一个网站可以同时支持两者。

<a id="how-it-works-in-the-browser"></a>

## 它在浏览器中的工作原理

在内置浏览器中打开网站并请求 ChatGPT Work 或 Codex 帮助完成任务。如果页面提供网站工具，智能体可以发现并使用您正在查看的网站中的相关操作。例如，文档编辑器可能会让智能体查找某个部分或留下评论供您查看。

在浏览器地址栏中选择 **现场工具** 以查看该网站提供的内容。选择 **可用的站点工具** 来检查各个工具。浏览器在网站执行每个请求之前检查每个请求，智能体可以检查页面以查看发生了什么变化。当最近的活动可用时，选择 **最近使用过** 打开 **来源** 并查看这些呼叫。

在本例中，展开**可用的站点工具**来检查[保证金](https://margin-local-docs.openai.chatgpt.site)提供的工具。



> 插图：ChatGPT 的内置浏览器显示保证金的网站工具菜单，其中包含 10 个可用工具。



工具属于提供它们的页面。关闭或离开页面可能会导致其工具不可用。如果没有合适的工具可用，智能体仍然可以使用其常规浏览器功能。

<a id="example-explore-openai-documentation"></a>

## 示例：浏览 OpenAI 文档

ChatGPT Learn 和 OpenAI Developers 提供用于查找和阅读文档的站点工具。在编辑器中选择 **在ChatGPT中打开**，以在桌面应用程序的浏览器中在新聊天旁边打开 Learn，并显示准备发送的提示。



**提示：**

```text
找到构建可重用技能的文档，打开相关页面，并解释何时应该将技能变成插件。
```

智能体可以使用这些工具来搜索、阅读和打开相关页面：

| 工具 | 它的作用 |
| ----------------------- | ------------------------------------------------------------------------ |
| `search_openai_docs` | 搜索 OpenAI 文档。                                           |
| `lookup_page` | 通过路径或 URL 读取文档页面。                               |
| `lookup_context` | 读取当前文档路径和选定的文本。                          |
| `navigate_to_page` | 在当前文档站点上打开匹配页面。                 |
| `generate_custom_guide` | 启动自定义构建或学习指南并返回其状态和链接。 |

Docs Agent 异步生成自定义指南。收到链接并不意味着生成已经完成。

<a id="security-and-user-controls"></a>

## 安全和用户控制

网站提供的工具定义和结果是不受信任的内容。工具的名称或声称它只读取数据并不能证明它的功能。网站说明并未授予智能体共享不相关信息或采取敏感操作的权限。

在内置浏览器中，每个工具调用在运行之前都会接受安全审查。正常的网站访问和确认政策仍然适用，包括发送消息、购买、删除数据或更改权限等后续操作。浏览器将每次调用与其原始页面和工具注册联系起来。这些检查可以降低风险；他们不会使网站或其输出值得信赖。

您可以在**设置 > 浏览器 > 权限**中关闭**启用站点工具**。在共享敏感信息或依赖更改之前，请检查站点、请求的操作和结果。

通过OpenAI的[安全漏洞赏金计划](https://bugcrowd.com/engagements/openai)报告安全漏洞。 AI安全风险请参见[安全漏洞赏金计划](https://openai.com/index/safety-bug-bounty/)。遵循每个计划的范围和提交说明。

<a id="limitations"></a>

## 局限性

ChatGPT 的内置浏览器当前支持 WebMCP API 的子集。不支持以下功能：

- **声明式 API：** 通过 HTML 表单属性定义的工具不可用作站点工具。
- **iframe 中的工具：** 浏览器无法发现在 iframe 内注册的工具，包括同源和跨源 iframe。

使用JavaScript在顶层页面注册工具，如[下一节](#add-webmcp-to-your-website)所示。 ChatGPT Work 和 Codex 仍可以使用常规浏览器功能与表单交互，但这些交互不是 WebMCP 工具调用。

WebMCP 规范和 Chrome 开发人员指南描述了更广泛的 API，包括内置浏览器当前不支持的功能。

<a id="add-webmcp-to-your-website"></a>

## 将 WebMCP 添加到您的网站

您可以要求 Codex 将 WebMCP 支持添加到您正在处理的 Web 应用程序或 [网站](sites.zh-CN.md)。描述智能体应该能够做什么，并要求 Codex 重用应用程序的现有逻辑和权限。

从您的应用程序已经支持的操作开始。例如：

- 仪表板可让智能体设置日期范围并检查图表背后的数据。
- 文档编辑器可让智能体查找某个部分、提出编辑建议或留下评论供您查看。
- 旅行规划器，让智能体可以在您检查地图时比较选项并更新行程。

您也可以自己编写代码。在页面的 JavaScript 模块中，检查浏览器支持并注册工具。此只读示例返回当前页面的标题：

```javascript
if (typeof document.modelContext?.registerTool === "function") {
  await document.modelContext.registerTool({
    name: "get_page_title",
    description: "Read the title of the current page.",
    inputSchema: {
      type: "object",
      properties: {},
      additionalProperties: false,
    },
    annotations: { readOnlyHint: true },
    execute: async () => ({ title: document.title }),
  });
}
```

兼容的智能体可以发现 `get_page_title` 并接收页面的当前标题。对于接受参数的工具，请在输入模式中描述它们，并在 `execute` 处理程序中使用它们来调用应用程序的现有逻辑。

保持输入范围窄，描述副作用，并返回足够的信息来验证结果。使用应用程序现有的身份验证、授权和输入验证。为不支持 WebMCP 的人和浏览器保留正常界面。

有关 API 详细信息和示例，请参阅 [WebMCP规范](https://webmachinelearning.github.io/webmcp/) 和 [Chrome 的开发者指南](https://developer.chrome.com/docs/ai/webmcp)。