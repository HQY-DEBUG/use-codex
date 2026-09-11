> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/security.md)。

<a id="codex-security"></a>

# Codex Security 概览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex Security 是一款应用程序安全智能体，可帮助安全和工程团队发现、确认和修复漏洞。从终端、通过 TypeScript SDK 或连接的 GitHub 仓库在 Codex 中使用它。

<CtaPillLink
  href="https://chatgpt.com/plugins/share/676aca3811d54fa7bcdef5255236b3c4"
  label="在ChatGPT中安装插件"
  icon="external"
  class="mb-8 mt-2"
/>

对于规定的首次本地扫描，请从 [Codex Security 插件快速入门](security/plugin.zh-CN.md) 开始。

<a id="use-codex-security-in-the-desktop-app"></a>

## 在桌面应用程序中使用 Codex Security

在 ChatGPT 桌面应用程序中，打开 ChatGPT 下拉列表并选择 **Codex**。安装并启用 Codex Security 插件以在侧边栏中打开 **安全性**。安全工作台将您的扫描、结果和仓库保存在一个位置，而 Codex 在任务中运行每次扫描。

- 使用 **扫描** 开始扫描、跟踪进度并查看保存的结果。
- 使用 **研究结果** 检查已完成扫描中的问题和证据。
- 使用 **仓库** 查看仓库历史记录并打开结果。

请参阅 [使用安全工作台](security/plugin/workbench.zh-CN.md) 了解完整的桌面应用程序工作流程。

<a id="explore-plugin-use-cases"></a>

### 探索插件用例

- [运行安全扫描](security/plugin/scans.zh-CN.md) 用于仓库或一个范围文件夹。
- [运行深度安全扫描](security/plugin/deep-scans.zh-CN.md) 当您需要更广泛的审查并且可以等待更长时间才能完成时。
- [检查代码更改](security/plugin/code-changes.zh-CN.md) 在合并拉取请求或分支之前。
- [对积压订单进行分类](security/plugin/triage-backlog.zh-CN.md) 当您有现有的安全调查结果需要审查时。
- [修复并验证结果](security/plugin/fix-findings.zh-CN.md) 带有已批准发现的有界补丁。
- [导出或跟踪结果](security/plugin/export-findings.zh-CN.md) 作为便携式工件或经过批准的跟踪目的地。
- [撰写漏洞报告](security/plugin/vulnerability-reports.zh-CN.md) 来自提供的调查结果、披露说明、来源和 PoC。
- 来自扫描结果或其他安全证据的 [提出安全强化建议](security/plugin/security-hardening.zh-CN.md)。
- Codex Security 插件中的 [看看有什么新鲜事](security/plugin/changelog.zh-CN.md)。

桌面安全工作台和 Codex CLI 使用 Codex Security 插件。 Codex Security云通过Codex云扫描连接的GitHub仓库。有关 Codex 沙箱、审批、网络控制和管理设置，请参阅 [智能体审批和安全](agent-approvals-security.zh-CN.md)。

<a id="codex-security-cli-and-sdk"></a>

## Codex Security CLI 和 SDK

CLI 和 TypeScript SDK 作为公共 [`@openai/codex-security`](https://github.com/openai/codex-security) 包提供。使用 `npx` 运行 CLI：

```bash
npx @openai/codex-security --help
```

运行扫描需要 Codex Security 访问权限。为获得最佳结果，请使用经过 [网络可信访问](https://chatgpt.com/cyber) 验证的帐户。

跨仓库和随着时间的推移使用相同的扫描仪作为插件。 CLI 发现 GitHub 仓库、恢复批量扫描、跟踪扫描结果并记录误报反馈。添加您的架构和安全策略，设置估计成本限制，或者在 CI 中和提交之前运行检查。使用 TypeScript SDK 将扫描、进度报告和成本控制构建到应用程序或开发人员工具中。

- [从 CLI 快速入门开始](security/cli.zh-CN.md) 用于设置 CLI、预检仓库并运行本地扫描。
- [运行批量安全扫描](security/cli/bulk-scans.zh-CN.md) 用于发现 GitHub 仓库或从 CSV 库存运行可恢复的营销活动。
- [在 CI 中运行扫描](security/cli/ci.zh-CN.md) 用于审查拉取请求更改、保留工件、上传 SARIF 并设置严重性策略。
- [阅读 CLI 常见问题解答](security/cli/faq.zh-CN.md) 有关扫描历史记录、误报反馈、覆盖范围和修复验证的答案。
- [使用 CLI 参考](security/cli/reference.zh-CN.md) 检查支持的命令、标志、输出格式、工件和退出代码。
- [集成 TypeScript SDK](security/sdk.zh-CN.md) 用于选择目标、检查结果、跟踪进度并取消代码扫描。

<a id="codex-security-cloud"></a>

## Codex Security云

Codex Security 云目前处于研究预览阶段。它扫描连接的 GitHub 仓库是否存在可能的安全问题。

它可以帮助团队：

1. **查找可能的漏洞** 通过使用特定于仓库的威胁模型和真实代码上下文。
2. **降低噪音** 通过在审查调查结果之前对其进行验证。
3. **将发现的结果转向修复** 包含排名结果、证据和建议的补丁选项。

<a id="how-codex-security-cloud-works"></a>

## Codex Security云的工作原理

Codex Security 逐次扫描连接的仓库提交。它从您的仓库构建扫描上下文，根据该上下文检查可能的漏洞，并在发现问题之前在隔离环境中验证高信号问题。

您将获得专注于以下内容的工作流程：

- 特定于仓库的上下文而不是通用签名
- 有助于减少误报的验证证据
- 您可以在 GitHub 中查看建议的修复

<a id="codex-security-cloud-access-and-prerequisites"></a>

## Codex Security 云访问和先决条件

Codex Security 云通过 Codex 云与连接的 GitHub 仓库配合使用。如果仓库不可见，请确认该仓库在您的 Codex 云工作区中可用，或联系您的 OpenAI 客户团队。

<a id="related-docs"></a>

## 相关文档

- [Codex Security 插件快速入门](security/plugin.zh-CN.md) 逐步完成安装和首次本地扫描。
- [安全工作台](security/plugin/workbench.zh-CN.md) 解释了桌面应用程序中保存的扫描、结果、仓库和扫描活动。
- [Codex Security CLI 快速入门](security/cli.zh-CN.md) 逐步完成设置、预检和首次终端扫描。
- [运行批量安全扫描](security/cli/bulk-scans.zh-CN.md) 解释了 GitHub 发现、CSV 库存、营销活动结果和恢复行为。
- [Codex Security CLI 常见问题解答](security/cli/faq.zh-CN.md) 回答有关扫描、结果、覆盖范围和费用的常见问题。
- [Security TypeScript SDK](security/sdk.zh-CN.md) 解释了如何从应用程序或开发人员工具运行扫描。
- [Codex Security 云设置](security/setup.zh-CN.md) 详细介绍了设置、扫描和结果审查。
- [安全审查](security/security-review.zh-CN.md) 解释了如何对 GitHub 拉取请求进行深入的安全审查。
- [改进威胁模型](security/threat-model.zh-CN.md) 解释了如何调整范围、入口点和关键性假设。
- [Codex Security云常见问题解答](security/faq.zh-CN.md) 涵盖常见的云产品问题。