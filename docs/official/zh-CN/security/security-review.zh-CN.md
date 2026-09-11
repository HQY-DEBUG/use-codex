> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/security-review.md)。

<a id="security-review"></a>

# 安全审查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex Security 评论可在研究预览中找到。适用于 ChatGPT Enterprise、Business、Edu 和 Pro 客户； Plus 上不提供此功能。推广期间，Codex Security 审核不消耗 ChatGPT 额度。使用限制可能适用。

Codex Security 审查是针对希望特别注意拉取请求中的安全问题的客户的额外审查。

通过分析拉取请求差异、支持仓库上下文以及配置的威胁模型或安全指南，Codex Security 审查比 [代码审查](../third-party/github.zh-CN.md) 更深入地分析特定于安全的风险。代码审查还可以识别与安全相关的问题，作为其一般审查的一部分，因此您可能会发现发现的结果之间偶尔有重叠。

<a id="before-you-start"></a>

## 开始之前

要配置自动 Codex Security 审核，您需要：

- Codex Security 查看您工作区的研究预览访问权限
- 使用连接的 GitHub 仓库设置 [Codex云](../cloud.zh-CN.md)
- GitHub 仓库设置的推送或管理员权限

现有的 Codex Security 扫描是可选的。

<a id="configure-security-review"></a>

<a id="configure-codex-security-review"></a>

## 配置 Codex Security 审查

1. 转到 [Codex设置](https://chatgpt.com/codex/settings/code-review)。
2. 在 **仓库首选项** 下，选择哪些拉取请求获得 Codex Security 审核：
   - **关注个人** 允许每个贡献者选择加入他们的个人 Codex Security 审核设置。
   - **审核所有 PR** 适用于仓库中的每个拉取请求。
   - **审核团队 PR**（如果可用）适用于由 ChatGPT 工作区成员（而不是 GitHub 团队成员）打开的拉取请求。
3. 选择 Codex Security 审核运行的时间：
   - 当拉取请求打开时，**公关开放中** 独立运行。
   - **每一次推动** 在推送新提交后独立运行。
   - **每当代码审查运行时** 需要代码审查并同时运行 Codex Security 审查。

<a id="add-threat-model-context"></a>

## 添加威胁模型上下文

您可以配置威胁模型，为 Codex 提供有关应用程序资产、信任边界、安全假设和仓库特定风险的上下文。如果仓库具有现有的 Codex Security 扫描配置，您可以使用其威胁模型。否则，请提供签入仓库的威胁模型文件的路径。如果您不指定来源，Codex 将为每次审核重新生成威胁模型。

<a id="set-reporting-thresholds"></a>

## 设置报告阈值

默认情况下，自动 Codex Security 审核报告 **高** 和 **关键** 结果，而手动请求的审核报告 **中等**、**高** 和 **关键** 结果。您可以独立更改自动和手动审核的最低严重性，并添加基于路径的覆盖。

发布到拉取请求的结果继承该拉取请求的 GitHub 可见性。任何可以查看拉取请求的人都可以查看这些发现，包括公共仓库或来自工作区之外的贡献者的拉取请求。为拉取请求评论可能广泛可见的仓库仔细选择报告阈值。报告阈值控制Codex向GitHub发布的内容；完整的 Codex Security 审查报告保留在 Codex 中。

<a id="request-a-security-review"></a>

<a id="request-a-codex-security-review"></a>

## 请求 Codex Security 审查

要手动请求 Codex Security 审核，请将此评论添加到拉取请求中：

`@codex security review`

Codex 在审核运行时做出反应，然后直接在拉取请求上发布满足手动报告阈值的结果。打开关联的 Codex 任务并选择 **安全报告** 选项卡以查看完整报告，包括严重性、攻击路径、支持证据、验证和补救指南。如果没有问题满足报告阈值，Codex 不会将发现结果发布到拉取请求。

<a id="related-docs"></a>

## 相关文档

- [使用 Codex 审查 GitHub 拉取请求](../third-party/github.zh-CN.md) 解释了代码审查和 GitHub 集成。
- [Codex Security 概览](../security.zh-CN.md) 给出了产品概述。
- [Codex Security 云设置](setup.zh-CN.md) 解释了仓库扫描和结果审查。
- [改进威胁模型](threat-model.zh-CN.md) 解释了如何调整仓库上下文。