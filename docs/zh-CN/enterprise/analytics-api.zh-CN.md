> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/analytics-api.md)。

<a id="analytics-api"></a>

# 分析 API

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex Analytics API 为 ChatGPT 工作区提供聚合的 Codex 使用情况和活动指标。

[Codex 分析 API 参考](https://chatgpt.com/public/admin/api-reference#tag/Codex%20Enterprise%20Analytics) 是当前访问需求、路由、请求和响应模式、指标、时间语义和分页的真实来源。

<a id="when-to-use-the-analytics-api"></a>

## 何时使用分析 API

当您需要执行以下操作时，Analytics API 是合适的选择：

- 自动生成定期 Codex 报告。
- 将聚合的 Codex 指标与内部组织数据相结合。
- 为批准的受众构建受控报告层。
- 避免将集成耦合到交互式仪表板。

它不是原始的审核日志界面。当工作流程需要可审核的活动记录时，请使用 [合规API](compliance-api.zh-CN.md)。

<a id="confirm-the-administration-boundaries"></a>

## 确认行政边界

分析 API 结果的范围仅限于 ChatGPT 工作区，但请求使用平台组织 API 密钥进行身份验证。密钥的组织必须与与工作区关联的组织相匹配。

API 参考拥有当前的密钥配置、范围要求、路由、模式、字段、时间语义和分页行为。此页面不重复该合同。

<a id="related-docs"></a>

## 相关文档

- [工作区分析](workspace-analytics.zh-CN.md)
- [管理员推出指南](admin-setup.zh-CN.md)
- [治理](governance.zh-CN.md)
- [合规API](compliance-api.zh-CN.md)