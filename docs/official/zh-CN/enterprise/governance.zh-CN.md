> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/governance.md)。

<a id="governance"></a>

# 治理

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 活动的治理涵盖交互式分析、程序化报告、相关 ChatGPT 使用控制和审计记录。选择与问题相匹配的使用界面；分析和合规性数据有不同的用途。

<a id="governance-and-observability"></a>
<a id="ways-to-track-codex-usage"></a>

| 如果需要 | 从 | 开始
| ------------------------------------------------------- | ------------------------------------------------------------------------- |
| 了解 ChatGPT | [工作区分析](workspace-analytics.zh-CN.md) | 的采用情况
| 以交互方式查看 Codex 采用情况和活动 | [Codex 分析](#analytics-dashboard) |
| 将聚合的 Codex 报告加载到另一个系统中 | [分析API](analytics-api.zh-CN.md) |
| 出口记录供审核或调查 | [合规API](compliance-api.zh-CN.md) |
| 检查计划相关的 ChatGPT 工作区信用控制 | [ChatGPT 使用限制和支出控制](usage-limits.zh-CN.md) |

<a id="open-the-administration-surfaces"></a>

## 打开管理界面

- 打开 [工作区分析](https://chatgpt.com/admin/usage) 进行交互式工作区报告。 [工作区分析指南](https://help.openai.com/en/articles/10875114-workspace-analytics-for-chatgpt-enterprise-and-edu) 描述了当前的角色和视图。
- 当您需要预定的程序化报告时，请打开 [Codex 分析 API 参考](https://chatgpt.com/public/admin/api-reference#tag/Codex%20Enterprise%20Analytics)。
- 打开 [管理 API 参考](https://chatgpt.com/public/admin/api-reference) 和 [合规平台指南](https://help.openai.com/en/articles/9261474-compliance-api-for-chatgpt-enterprise-edu-and-chatgpt-for-teachers) 进行审计和调查集成。

例如，使用工作区分析进行快速采用检查，使用分析 API 将聚合的 Codex 报告加载到商业智能系统中，使用合规性 API 将可审核记录发送到 SIEM 或电子发现工作流程。

<a id="analytics-dashboard"></a>

## 分析仪表板

<a id="dashboard-views"></a>
<a id="data-export"></a>

ChatGPT 提供工作区范围内的分析，以实现广泛采用和参与。 Codex 分析重点关注 Codex 活动。两者都是交互式报告界面，而不是原始审计日志。

使用 [工作区分析](workspace-analytics.zh-CN.md) 比较两种体验并找到其当前所有者维护的来源。您也可以直接打开[工作区分析](https://chatgpt.com/admin/usage)。不要根据仪表板标签或下载的报告字段构建持久的报告合同；这些可能会随着产品的发展而改变。

<a id="related-chatgpt-usage-controls"></a>

## 相关 ChatGPT 使用控制

ChatGPT 工作区使用控制与分析分开，并且不配置功能权利。根据计划，符合条件的 Codex 活动可能会消耗 ChatGPT 工作区额度，并且耗尽限制可能会暂停对符合条件的功能的访问。这些控件不会设置通用 Codex 限制或管理平台 API 计费。

有关持久边界和当前帮助中心源，请参阅 [ChatGPT 使用限制和支出控制](usage-limits.zh-CN.md)。

<a id="analytics-api"></a>

## 分析API

<a id="what-it-measures"></a>
<a id="endpoints"></a>
<a id="usage"></a>
<a id="code-review-activity"></a>
<a id="user-engagement-with-code-review"></a>
<a id="how-it-works"></a>
<a id="common-use-cases"></a>

使用 Analytics API 进行编程聚合 Codex 报告。它适用于不应依赖于交互式仪表板的数据仓库、商业智能系统和内部报告。

API 参考拥有访问要求、路由、模式、字段、报告窗口和分页。有关概念集成边界和规范参考链接，请参阅 [分析API](analytics-api.zh-CN.md)。

<a id="compliance-api"></a>

## 合规API

<a id="what-it-measures-1"></a>
<a id="what-you-can-export"></a>
<a id="activity-logs"></a>
<a id="metadata-for-audit-and-investigation"></a>
<a id="common-use-cases-1"></a>
<a id="what-it-does-not-provide"></a>

将合规性 API 用于需要可审核记录的安全、法律和治理工作流程。它不是采用率或生产力仪表板。

API 参考拥有事件覆盖范围、模式、权限、过滤器、保留和请求行为。有关概念集成边界和规范参考链接，请参阅 [合规API](compliance-api.zh-CN.md)。

<a id="recommended-pattern"></a>

要在这些使用界面上进行部署排序和验证，请使用 [管理员推出指南](admin-setup.zh-CN.md)。

<a id="related-docs"></a>

## 相关文档

- [管理员推出指南](admin-setup.zh-CN.md)
- [工作区分析](workspace-analytics.zh-CN.md)
- [分析API](analytics-api.zh-CN.md)
- [合规API](compliance-api.zh-CN.md)