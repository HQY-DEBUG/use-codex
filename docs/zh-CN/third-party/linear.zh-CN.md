> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/third-party/linear.md)。

<a id="use-codex-in-linear"></a>

# Linear 集成

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

在 Linear 中使用 Codex 来委派问题中的工作。将问题分配给 Codex 或在评论中提及 `@Codex`，Codex 会创建云聊天并回复进度和结果。

线性中的 Codex 可在付费计划中使用（请参阅 [定价](../pricing.zh-CN.md)）。

如果您使用的是企业计划，请要求 ChatGPT 工作区管理员在 [工作区设置](https://chatgpt.com/admin/settings) 中打开 Codex 云聊天，并在 [连接器设置](https://chatgpt.com/admin/ca) 中启用 **Codex 线性**。

<a id="set-up-the-linear-integration"></a>

## 设置线性积分

1. 通过连接 [Codex](https://chatgpt.com/codex) 中的 GitHub 并为您希望 Codex 工作的仓库创建 [环境](../environments/cloud-environment.zh-CN.md) 来设置 [Codex 云聊天](../cloud.zh-CN.md)。
2. 转至 [Codex设置](https://chatgpt.com/codex/settings/connectors) 并为您的工作区安装 **Codex 线性**。
3. 通过在 Linear 问题的评论线程中提及 `@Codex` 来链接您的 Linear 帐户。

<a id="delegate-work-to-codex"></a>

## 将工作委派给 Codex

您可以通过两种方式进行委托：

<a id="assign-an-issue-to-codex"></a>

### 将问题分配给 Codex

安装集成后，您可以将问题分配给 Codex，就像将问题分配给队友一样。 Codex 开始工作并将更新发布回问题。



  
    

> 插图：将 Codex 分配给线性问题


  



<a id="mention-codex-in-comments"></a>

### 在评论中提及 `@Codex`

您还可以在评论线程中提及 `@Codex` 来委托工作或提出问题。 Codex回复后，在帖子中跟进以继续相同的聊天。



  
    

> 插图：在 Linear 问题评论中提到 Codex


  



在 Codex 开始处理某个问题后，它会在 [选择环境和仓库](#how-codex-chooses-an-environment-and-repo) 中工作。要固定特定的仓库，请将其包含在您的评论中，例如：`@Codex fix this in openai/codex`。

跟踪进度：

- 打开该问题上的 **活动** 以查看进度更新。
- 打开聊天链接以了解更多详细信息。

Codex 完成后，它会发布摘要和已完成聊天的链接，以便您可以创建拉取请求。

<a id="how-codex-chooses-an-environment-and-repo"></a>

### Codex 如何选择环境和仓库

- Linear 根据问题上下文建议一个仓库。 Codex 选择最适合该建议的环境。如果请求不明确，它将回退到您最近使用的环境。
- 聊天针对该环境的仓库映射中列出的第一个仓库的默认分支运行。如果您需要不同的默认或更多仓库，请更新 Codex 中的仓库映射。
- 如果没有合适的环境或仓库可用，Codex 将以 Linear 方式回复，并说明如何在重试之前解决问题。

<a id="automatically-assign-issues-to-codex"></a>

## 自动将问题分配给 Codex

您可以使用分类规则自动将问题分配给 Codex：

1. 在线性中，转到 **设置**。
2. 在 **你的团队** 下，选择您的团队。
3. 在工作流程设置中，打开**分诊**并将其打开。
4. 在 **分诊规则** 中，创建规则并选择 **代表** > **Codex**（以及您要设置的任何其他属性）。

Linear 自动将进入分类的新问题分配给 Codex。当您使用分类规则时，Codex 使用问题创建者的帐户运行聊天。



  
    

> 插图：线性分类规则自动将问题分配给 Codex


  



<a id="data-usage-privacy-and-security"></a>

## 数据使用、隐私和安全

当您提及 `@Codex` 或为其分配问题时，Codex 会收到您的问题内容以了解您的请求并创建聊天。数据处理遵循OpenAI的[隐私政策](https://openai.com/privacy)、[使用条款](https://openai.com/terms/)和其他适用的[政策](https://openai.com/policies)。有关安全性的更多信息，请参阅 [Codex 安全文档](../agent-approvals-security.zh-CN.md)。

Codex 使用可能会出错的大型语言模型。始终查看答案和差异。

<a id="tips-and-troubleshooting"></a>

## 提示和故障排除

- **缺少连接**：如果 Codex 无法确认您的 Linear 连接，它会在问题中回复一个连接您帐户的链接。
- **意想不到的环境选择**：在线程中回复您想要的环境（例如，`@Codex please run this in openai/codex`）。
- **代码的错误部分**：在问题中添加更多上下文，或在 `@Codex` 评论中给出明确的说明。
- **更多帮助**：请参阅 [OpenAI 帮助中心](https://help.openai.com/)。

<a id="connect-linear-for-local-tasks-mcp"></a>

<a id="connect-linear-for-local-work-mcp"></a>

## 连接 Linear 进行本地工作 (MCP)

如果您使用 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展并希望其在本地访问线性问题，请配置线性模型上下文协议 (MCP) 服务器。

要了解更多信息，[查看 Linear MCP 文档](https://linear.app/integrations/codex-mcp)。

无论您使用 IDE 扩展还是 CLI，MCP 服务器的设置步骤都是相同的，因为两者共享相同的配置。

<a id="use-the-cli-recommended"></a>

### 使用 CLI（推荐）

如果您安装了 CLI，请运行：

```bash
codex mcp add linear --url https://mcp.linear.app/mcp
```

这会提示您使用 Linear 帐户登录并将其连接到 Codex。

<a id="configure-manually"></a>

### 手动配置

1. 在编辑器中打开 `~/.codex/config.toml`。
2. 添加以下内容：

```toml
[mcp_servers.linear]
url = "https://mcp.linear.app/mcp"
```

3. 运行`codex mcp login linear`登录。