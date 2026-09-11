> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/security/threat-model.md)。

<a id="improving-the-threat-model"></a>

# 威胁模型

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

了解什么是威胁模型以及对其进行编辑如何改进 Codex Security 的建议。

<a id="what-a-threat-model-is"></a>

## 什么是威胁模型

威胁模型是关于仓库工作方式的简短安全摘要。在 Codex Security 中，您将其编辑为 `project overview`，系统将其用作扫描上下文以供将来扫描、优先级排序和审核。

Codex Security 根据代码创建初稿。如果调查结果感觉不对，这是首先要编辑的事情。

一个有用的威胁模型指出：

- 入口点和不受信任的输入
- 信任边界和授权假设
- 敏感数据路径或特权操作
- 您的团队希望首先审查的领域

例如：

> 用于帐户更改的公共 API。接受 JSON 请求和文件上传。使用内部身份验证服务进行身份检查并通过内部服务写入账单更改。重点审查身份验证检查、上传解析和服务到服务的信任边界。

这为 Codex Security 未来的扫描和查找优先级提供了更好的起点。

<a id="improving-and-revisiting-the-threat-model"></a>

## 改进和重新审视威胁模型

如果您想改进结果，请先编辑威胁模型。当发现的结果缺少您关心的区域或出现在您意想不到的地方时，请使用它。威胁模型会改变未来的扫描上下文。

一些用户将当前的威胁模型复制到 Codex 中，根据他们想要更仔细审查的区域使用聊天来改进它，然后将更新的版本粘贴回 Web UI 中。

<a id="where-to-edit"></a>

### 在哪里编辑

要查看或更新威胁模型，请转到 [Codex Security 扫描](https://chatgpt.com/codex/security/scans)，打开仓库，然后单击 **编辑**。

<a id="related-docs"></a>

## 相关文档

- [Codex Security 云设置](setup.zh-CN.md) 涵盖仓库设置和结果审查。
- [Codex Security 概览](../security.zh-CN.md) 给出了产品概述。
- [Codex Security云常见问题解答](faq.zh-CN.md) 涵盖常见的云问题。