> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/workspace-analytics.md)。

<a id="workspace-analytics"></a>

# 工作区分析

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 ChatGPT 工作区分析来实现工作区的广泛采用。使用 Codex 分析来生成以 Codex 为中心的报告。使用 Analytics API 进行编程聚合，使用合规性 API 进行可审核记录。

这些报告界面不授予产品访问权限或设置运行时策略。有关管理边界，请参阅 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="choose-a-reporting-surface"></a>

## 选择报告界面

| Surface | 用于 | 合同所有者 |
| --------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| ChatGPT 工作区分析 | 交互式、工作区范围内的采用和参与度报告 | [工作区分析帮助中心指南](https://help.openai.com/en/articles/10875114) |
| Codex 分析 | 专注于 Codex 采用和活动的交互式报告 | 经过验证的 [Codex 分析仪表板](https://admin.openai.com/analytics/codex) |
| 分析 API | 编程式聚合 Codex 报告 | [Codex 分析 API 参考](https://chatgpt.com/public/admin/api-reference#tag/Codex%20Enterprise%20Analytics) |
| 合规性 API | 审计、安全、法律和调查记录 | [管理 API 参考](https://chatgpt.com/public/admin/api-reference) |

<a id="review-chatgpt-workspace-analytics"></a>

## 查看 ChatGPT 工作区分析

ChatGPT 工作区分析提供了支持的工作区功能的采用和参与的交互式视图。可用性、角色、仪表板部分、新鲜度、隐私行为和导出格式可能会发生变化。使用 [ChatGPT Enterprise 和 Edu 的工作区分析](https://help.openai.com/en/articles/10875114) 了解当前的覆盖范围和程序。

将下载的报告视为可识别的组织数据。应用组织的访问、存储和保留策略，而不是假设导出具有与聚合仪表板相同的隐私特征。

<a id="review-codex-analytics"></a>

## 查看 Codex 分析

经过验证的 [Codex 分析仪表板](https://admin.openai.com/analytics/codex) 专注于 Codex 报告。将其用于交互式探索，而不是作为稳定的模式契约。仪表板类别、字段、过滤器和导出格式可以独立于此页面进行更改。

对于自动报告，请使用 [分析API](analytics-api.zh-CN.md) 并遵循其 API 参考。对于可审计记录，请使用 [合规API](compliance-api.zh-CN.md)。

<a id="interpret-reporting-data"></a>

## 解读报告数据

请记住这些界限：

- ChatGPT 工作区分析和 Codex 分析涵盖不同的产品范围。
- 聚合分析和审计记录有不同的用途，并且具有单独的合同。
- 分析描述活动；它不会授予访问权限或更改运行时权限。
- [ChatGPT 使用限制和支出控制](usage-limits.zh-CN.md) 是一个独立的、依赖于计划的工作区边界。