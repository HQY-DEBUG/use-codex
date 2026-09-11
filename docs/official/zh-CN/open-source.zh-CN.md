> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/open-source.md)。

<a id="open-source"></a>

# 开源项目

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

OpenAI 公开开发Codex 的关键部件。该工作位于 GitHub 上，因此您可以跟踪进度、报告问题并做出改进。

如果您维护一个广泛使用的开源项目或想要提名维护人员来管理重要项目，您还可以使用 [申请Codex OSS计划](https://developers.openai.com/community/codex-for-oss) 获取 API 额度，使用 ChatGPT Pro 获取 Codex，以及选择性访问 Codex Security。

<a id="open-source-components"></a>

## 开源组件

| 组件 | 哪里可以找到 | 注释 |
| ----------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Codex CLI | [openai/法典](https://github.com/openai/codex) | Codex 开源开发的主要主页 |
| Codex SDK | [openai/codex/codex-sdk](https://github.com/openai/codex/tree/main/sdk) | SDK 源代码位于 Codex 仓库 | 中
| Codex Security CLI | [openai/codex-安全](https://github.com/openai/codex-security) | 用于查找和验证安全漏洞的 CLI |
| Codex Security TypeScript SDK | [openai/codex-security/sdk/typescript](https://github.com/openai/codex-security/tree/main/sdk/typescript) | 用于运行 Codex Security 扫描 | 的 TypeScript SDK
| Codex 应用服务器 | [openai/codex/codex-rs/应用程序服务器](https://github.com/openai/codex/tree/main/codex-rs/app-server) | 应用服务器源位于 Codex 仓库 |
| 技能 | [开放/技能](https://github.com/openai/skills) | 可重复使用的技能，扩展 ChatGPT 和 Codex |
| 插件 | [openai/插件](https://github.com/openai/plugins) | ChatGPT 和 Codex | 可重复使用的插件
| IDE 扩展 | - | 未开源 |
| Codex 云 | - | 未开源 |
| 通用云环境 | [openai/通用法典](https://github.com/openai/codex-universal) | Codex 云 | 使用的基础环境

<a id="where-to-report-issues-and-request-features"></a>

## 在哪里报告问题和请求功能

使用适当的 GitHub 仓库来获取错误报告和功能请求：

- Codex 错误报告和功能请求：[openai/法典/问题](https://github.com/openai/codex/issues)
- Codex Security CLI 和 TypeScript SDK 错误报告和功能请求：[openai/codex-安全/问题](https://github.com/openai/codex-security/issues)
- 讨论论坛：[openai/法典/讨论](https://github.com/openai/codex/discussions)

当您提交问题时，请包括您正在使用的组件（CLI、SDK、IDE 扩展、Codex 云或 Codex Security）以及版本（如果可能）。