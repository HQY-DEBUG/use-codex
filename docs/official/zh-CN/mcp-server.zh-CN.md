> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/mcp-server.md)。

<a id="codex-mcp-server-removal"></a>

# 旧 MCP 服务器命令移除

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

`codex mcp-server` 命令和独立的 `codex-mcp-server` 二进制文件已被删除。启动任一命令的集成必须在升级 Codex 之前进行迁移。不再支持此页面上之前的 MCP 工具参考和 Agents SDK 示例。

<a id="use-the-codex-app-server"></a>

## 使用 Codex 应用服务器

使用 [Codex 应用服务器](app-server.zh-CN.md) 进行需要身份验证、对话历史记录、批准和流式智能体事件的集成。

应用服务器使用自己的[JSON-RPC协议](app-server.zh-CN.md#protocol)。它不是 MCP 服务器或 MCP 客户端的直接替代品：更新您的集成以使用应用程序服务器协议而不是 MCP 工具调用。 app-server 命令是实验性的，不支持生产工作负载。

<a id="connect-codex-to-mcp-tools"></a>

## 将 Codex 连接到 MCP 工具

Codex 继续支持 [外部 MCP 服务器](extend/mcp.zh-CN.md)。使用 `codex mcp` 来管理这些连接。删除会影响将 Codex 作为 MCP 服务器托管。