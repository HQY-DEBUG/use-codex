> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/third-party/slack.md)。

<a id="use-codex-in-slack"></a>

# Slack 集成

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Slack 中的 Codex 从通道和线程开始编码工作。通过提示提及 `@Codex`，Codex 会创建云聊天并回复结果。



  
    

> 插图：Codex Slack 集成实际应用


  






<a id="set-up-the-slack-app"></a>

## 设置 Slack 应用程序

1. 设置 [Codex 云聊天](../cloud.zh-CN.md)。您需要一个 Plus、Pro、Business、Enterprise 或 Edu 计划（请参阅 [ChatGPT 定价](https://chatgpt.com/pricing)）、一个已连接的 GitHub 帐户以及至少一个 [环境](../environments/cloud-environment.zh-CN.md)。
2. 转至 [Codex设置](https://chatgpt.com/codex/settings/connectors) 并为您的工作区安装 Slack 应用程序。根据您的 Slack 工作区策略，管理员可能需要批准安装。
3. 将 `@Codex` 添加到频道。如果您尚未添加，当您提及时，Slack 会提示您。

<a id="start-a-task"></a>

<a id="start-a-chat"></a>

## 开始聊天

1. 在频道或话题中，提及 `@Codex` 并包含您的提示。 Codex 可以引用线程中较早的消息，因此您通常不需要重述上下文。
2. （可选）在提示中指定环境或仓库，例如：`@Codex fix the above in openai/codex`。
3. 等待 Codex 做出反应 (👀) 并回复聊天链接。完成后，Codex 会发布结果，并根据您的设置在线程中发布答案。

<a id="how-codex-chooses-an-environment-and-repo"></a>

### Codex 如何选择环境和仓库

- Codex 会检查您有权访问的环境并选择最符合您的要求的环境。如果请求不明确，它将回退到您最近使用的环境。
- 聊天针对该环境的仓库映射中列出的第一个仓库的默认分支运行。如果您需要不同的默认或更多仓库，请更新 Codex 中的仓库映射。
- 如果没有合适的环境或仓库可用，Codex 将在 Slack 中回复，并说明如何在重试之前解决问题。

<a id="enterprise-data-controls"></a>

### 企业数据控制

默认情况下，Codex 在线程中回复答案，其中可以包含来自其运行环境的信息。为防止这种情况，企业管理员可以清除 [ChatGPT 工作区设置](https://chatgpt.com/admin/settings) 中的 **允许 Codex Slack 应用在任务完成时发布答案**。当管理员关闭答案时，Codex 仅回复聊天链接。

<a id="data-usage-privacy-and-security"></a>

### 数据使用、隐私和安全

当您提及 `@Codex` 时，Codex 会收到您的消息和线程历史记录，以了解您的请求并创建聊天。数据处理遵循OpenAI的[隐私政策](https://openai.com/privacy)、[使用条款](https://openai.com/terms/)和其他适用的[政策](https://openai.com/policies)。有关安全性的更多信息，请参阅 Codex [安全文档](../agent-approvals-security.zh-CN.md)。

Codex 使用可能会出错的大型语言模型。始终查看答案和差异。

<a id="tips-and-troubleshooting"></a>

### 提示和故障排除

- **缺少连接**：如果 Codex 无法确认您的 Slack 或 GitHub 连接，它会回复一个重新连接的链接。
- **意想不到的环境选择**：在帖子中回复您想要的环境（例如`Please run this in openai/openai (applied)`），然后再次提及`@Codex`。
- **长螺纹或复杂螺纹**：总结最新消息中的关键详细信息，以便 Codex 不会错过线程中较早埋藏的上下文。
- **工作区发布**：某些企业工作区限制发布最终答案。在这些情况下，请打开聊天链接以查看进度和结果。
- **更多帮助**：请参阅 [OpenAI 帮助中心](https://help.openai.com/)。