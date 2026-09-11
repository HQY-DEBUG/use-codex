> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/compliance-api.md)。

<a id="compliance-api-and-audit-events"></a>

# 合规 API 与审计事件

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

将合规性 API 用于需要可审核记录的安全、法律、治理和调查工作流程。使用分析而不是合规记录来衡量采用情况和趋势。

[管理 API 参考](https://chatgpt.com/public/admin/api-reference) 是当前访问要求、事件覆盖范围、路线、模式、过滤器、保留和请求行为的真实来源。

有关可用合规使用界面和常见集成模式的概述，请参阅 [合规平台指南](https://help.openai.com/en/articles/9261474-compliance-api-for-chatgpt-enterprise-edu-and-chatgpt-for-teachers)。

<a id="when-to-use-the-compliance-api"></a>

## 何时使用合规性 API

当您需要执行以下操作时，合规性 API 是合适的选择：

- 将支持的记录导出到审计或调查系统中。
- 应用组织保留和合法保留流程。
- 将 Codex 活动与其他安全或身份数据相关联。
- 支持批准的安全、法律或治理调查。

它不是生产力仪表板。不要用它来推断代码质量或个人性能。使用 [工作区分析](workspace-analytics.zh-CN.md) 或 [分析API](analytics-api.zh-CN.md) 进行采用报告。

<a id="get-started"></a>

## 开始使用

1. 打开 [管理 API 参考](https://chatgpt.com/public/admin/api-reference) 并确认您的管理员角色可以访问您所需的合规性资源。
2. 使用仅附加合规性日志流进行持续收集。检查 API 参考以了解当前支持的资源和检索模式。
3. [下载日志文件](#download-logs) 并测试摄取到非生产安全信息和事件管理 (SIEM) 系统或数据湖中。
4. 安排连续收集并对导出的记录应用组织的访问、保留和合法保留控制。不要假设源保留窗口会取代您组织的保留策略。

例如，安全团队可以将不可变的合规性事件流式传输到其 SIEM 中进行调查，或将这些事件路由到批准的电子发现工作流程中。使用当前路由和架构的 API 参考，而不是从本指南复制端点协定。

<a id="download-logs"></a>

### 下载日志

下载 [bash脚本](https://developers.openai.com/downloads/compliance-api/download_compliance_files.sh) 或 [PowerShell 脚本](https://developers.openai.com/downloads/compliance-api/download_compliance_files.ps1)。两者都会在给定时间戳之后列出并下载每个可用日志文件，遵循分页，并将 JSONL 写入标准输出。错误转到标准错误。

将 `COMPLIANCE_API_KEY` 设置为您的企业合规性 API 密钥。替换 `<workspace_or_org_id>` with your ChatGPT workspace ID or API Platform organization ID, and `<after>` with an ISO 8601 timestamp that includes a time zone. This example retrieves `AUTH_LOG` 文件，一次 100 个。

在 macOS 或 Linux 上，安装 Bash、`curl` 和 `jq`，然后运行：

```bash
bash ./download_compliance_files.sh "<workspace_or_org_id>" AUTH_LOG 100 "<after>" > output.jsonl
```

Windows 脚本支持 PowerShell 5.1 或更高版本。查看下载的文件。如果 Windows 阻止它并且您组织的执行策略允许它，请运行 `Unblock-File -Path .\download_compliance_files.ps1`。此示例使用 PowerShell 7 保存没有字节顺序标记的 UTF-8：

```powershell
.\download_compliance_files.ps1 "<workspace_or_org_id>" AUTH_LOG 100 "<after>" |
  Set-Content -Encoding utf8NoBOM output.jsonl
```

<a id="confirm-the-administration-boundaries"></a>

## 确认行政边界

合规性覆盖范围遵循 ChatGPT 工作区和当前 API 参考中代表的产品。平台 API 组织数据遵循其自己的 API 数据和管理控制。

API 参考拥有当前路由、事件覆盖范围、架构、过滤器、保留行为、权限要求和请求机制。此页面不重复该合同。

<a id="related-docs"></a>

## 相关文档

- [工作区分析](workspace-analytics.zh-CN.md)
- [管理员推出指南](admin-setup.zh-CN.md)
- [治理](governance.zh-CN.md)
- [分析API](analytics-api.zh-CN.md)