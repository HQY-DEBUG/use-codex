> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/cyber-safety/recommended-configuration.md)。

<a id="recommended-configuration"></a>

# 安全任务推荐配置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

适合网络安全工作流程的安全控制取决于模型、可以采取的操作、可以访问的系统以及所涉及数据的敏感性。

对于大多数 Daybreak Blue 工作流程，您组织的现有安全实践（例如访问控制、凭据保护和敏感操作审查）可能就足够了。

Daybreak Red 工作流程、自主安全测试以及涉及生产系统、敏感数据或外部工具的活动可能需要更强的保护措施。以下建议主要针对这些高风险场景。

您负责评估特定工作流程的风险并实施适当的安全控制。模型保护措施和可信访问不会取代组织自身的安全、监控和监督实践。

可信访问管理批准的模型访问，但它不会配置您的环境或对批准的系统和操作实施限制。您的团队必须设置适当的隔离、许可、审查、监控和人工监督控制。假设模型、其工具和每个连接的系统都可能受到损害，然后配置环境，以便它们仍然无法访问未经授权的系统、暴露凭据、禁用防护措施或在工作结束后仍然存在。

<a id="isolate-the-environment"></a>

## 隔离环境

在专用实验室或沙箱中运行进攻性安全工作。开始时无需不受限制的互联网访问、敏感生产系统、企业网络、不相关的工作负载或主机管理界面。除非您批准的工作明确要求并授权，否则请将秘密、凭证、持久访问和持久系统更改保存在遥不可及的地方。

对于风险较高或保障程度较低的工作，每次尝试都应使用新的、高度隔离的环境。分离计算、存储、网络和身份，然后销毁环境，而不是重置或重用它。

在开始高风险工作之前测试文件系统和网络边界。包括每个可访问的主机、连接的工具、委托智能体和下游服务。即使模型或审阅者批准单个操作，也要保持主机环境隔离。

<a id="define-and-enforce-approved-boundaries"></a>

## 定义并执行批准的边界

在模型开始之前，记录为您的工作批准的系统、工具、操作和时间限制。包括：

- 批准的目标系统、主机和环境。
- 排除的系统，包括生产和不相关的基础设施。
- 批准的工具和连接的服务。
- 批准和禁止的行为。
- 批准的开始和结束时间以及数据处理要求。
- 漏洞披露、补丁批准和维护人员协调。
- 停止需要明确人类批准的条件和操作。

为智能体提供这些批准的边界作为任务上下文。仅靠文档并不能强制执行这些规则：应用独立的文件系统、网络、身份和工具控制，以在可行的情况下阻止未经授权的操作。

使用 Codex [权限配置文件](../permissions.zh-CN.md) 创建最小权限边界。当任务不需要更改时选择 `:read-only`，或者当工作需要工作区编辑时扩展 `:workspace`。例如：

```toml
approval_policy = "on-request"
approvals_reviewer = "auto_review"
default_permissions = "cyber-lab"

[features]
network_proxy = true

[permissions.cyber-lab]
description = "Limit security testing to the approved lab and workspace."
extends = ":workspace"

[permissions.cyber-lab.filesystem]
glob_scan_max_depth = 3

[permissions.cyber-lab.filesystem.":workspace_roots"]
"**/.env*" = "deny"
"**/*.pem" = "deny"

[permissions.cyber-lab.network]
enabled = true
# 仅对解析为私有地址的已批准主机取消注释。
# allow_local_binding = true

[permissions.cyber-lab.network.domains]
"lab.example.com" = "allow"
```

`network_proxy` 功能强制执行已批准的域。如果没有它，`network.enabled = true` 允许直接网络访问，并且实验室白名单不会限制目的地。 Web 搜索、应用程序、连接器、MCP 服务器、浏览器活动和 Codex 云使用单独的控件；限制或关闭您批准的工作流程不需要的每个使用界面。

将 `lab.example.com` 替换为批准的目标。有界文件系统扫描旨在避免搜索 Linux、WSL 和 Windows 上的整个工作区；如果敏感文件看起来更深，请增加深度或使用精确拒绝路径。不要将权限配置文件与旧版 `sandbox_mode` 设置相结合；遵循 [权限配置文件配置指导](../permissions.zh-CN.md#define-and-select-a-profile)。

如果批准的实验室主机解析为私有地址，则 Codex 默认情况下会阻止它，即使该主机位于白名单上也是如此。仅针对明确批准的专用网络工作设置 `allow_local_binding = true`，缩小目标许可名单范围，并审查 [本地和专用网络指导](../permissions.zh-CN.md#local-and-private-networks)。您还可以将确切批准的私有 IP 地址列入白名单。

默认情况下阻止开放互联网和生产网络访问。如果需要外部访问，请通过独立执行的网关或具有狭窄允许列表、请求检查和日志记录的代理进行路由。对通过包管理器、Webhooks、URL 获取服务、重定向、云 API 和连接工具的间接连接应用相同的限制。在运行之前加载依赖项或使用管理员批准的依赖项。

<a id="protect-credentials-and-sensitive-data"></a>

## 保护凭证和敏感数据

将可重用的 API 密钥、云凭证、密码和服务帐户令牌保留在提示、仓库、环境变量、共享文件系统和模型可访问日志之外。当需要身份验证时，使用单独的代理或网关来提供适用于确切目标和允许的操作的短期凭据，而无需将凭据暴露给模型。

仅提供已批准任务所需的数据。删除不必要的敏感信息，阻止对云元数据和凭证端点的访问，并将模型生成的文件视为不可信。

对于网络安全工作流程，请避免使用 `:danger-full-access` 和 `--yolo`。完全访问权限消除了自动审查所依赖的可执行沙箱边界。托管组织可以排除 `:danger-full-access` 和 `--yolo`、限制允许的审批策略，并要求通过 [企业管理的配置](../enterprise/managed-configuration.zh-CN.md#configure-automatic-review-policy) 自动审核。

在为批准的安全模型启用 **完全访问权限** 之前，ChatGPT 桌面应用程序会显示有关危险操作的特定于模型的警告。该警告建议改为 **代我批准** 并链接到 [审阅者策略配置](../sandboxing/auto-review.zh-CN.md#configuration)。该警告不会恢复沙箱边界或覆盖组织策略。

Guardrails 将基于策略的审查添加到受控的网络安全工作流程中。它们不能取代环境隔离、最低权限、明确定义的边界、监控或人工监督。

<a id="review-sensitive-codex-actions"></a>

## 检查敏感的 Codex 操作

在建议的操作运行之前，[自动审核](../sandboxing/auto-review.zh-CN.md) 将符合条件的沙箱边界批准请求发送给单独的审核者。审核者考虑建议的操作、有界任务上下文和适用的策略，然后允许或拒绝该请求。组织可以根据其批准的目标、禁止的行为和所需的人工审核条件自定义该策略。

对于影响生产、外部系统、敏感数据、权限升级、持久访问或不可逆转的更改的操作，需要明确的人工批准。将网站、仓库、文档和工具输出中嵌入的指令视为不可信；他们无法扩大授权范围或覆盖访问控制。

在 ChatGPT 桌面应用程序中，选择已批准的 Daybreak 模型会自动将权限控制切换到 **代我批准**（当您的帐户可用且组织策略允许该模式时）。当您使用桌面应用程序的 `/model` 命令时，这也适用。如果该模式不可用，则当前权限模式保持不变。模型选择永远不会凌驾于受管理组织的要求之上。

要运行自动审核，请将所有三个控件保留到位：

1. 使用交互式审批策略，例如 `approval_policy = "on-request"`。
2. 设置 `approvals_reviewer = "auto_review"`。
3. 保留可执行的沙箱或权限配置文件边界。

对网络白名单上的目标的请求保留在网络边界内，不会自动触发自动审核。要查看敏感命令（即使其目标位于允许列表中），请在 `~/.codex/rules/` 下创建显式 [命令规则](../agent-configuration/rules.zh-CN.md)：

```python
prefix_rule(
    pattern = ["curl"],
    decision = "prompt",
    justification = "Review requests to the approved cybersecurity target.",
)
```

添加规则后重启Codex。对于 `approvals_reviewer = "auto_review"`，匹配的命令在执行之前会发送给审阅者。为每个敏感命令添加相应的提示规则，或者对单个[MCP 工具](../extend/mcp.zh-CN.md)使用`approval_mode = "prompt"`。需要个人决定的行动仍然需要明确的人类批准。

自动审查不会检查沙箱内已允许的例行操作。使用 `approval_policy = "never"` 或完全访问权限，敏感操作可能不会创建可审查的批准请求。自动审查可能会犯错误，并且不能取代隔离、明确定义的边界、监控或明确的人工监督。

有关范围内的策略和组织范围内的实施，请参阅 [配置授权的网络安全工作流程](../sandboxing/auto-review.zh-CN.md#configure-an-authorized-cybersecurity-engagement)。

<a id="monitor-independently-and-fail-closed"></a>

## 独立监控并关闭故障

记录模型请求、工具调用、网络活动、凭证使用以及与安全相关的更改。将日志和监控系统保留在模型控制环境之外。针对未经授权的目标、意外的网络请求、暴露的凭据、策略更改、丢失日志以及尝试绕过防护措施发出警报。

保持策略执行、凭证智能体、审查系统和紧急关闭控制独立于智能体。如果重要的控制或监控系统出现故障，请停止工作流程。

<a id="add-guardrails-to-custom-agent-workflows"></a>

## 向自定义智能体工作流程添加护栏

如果您使用 Responses API、Agents SDK 或其他工具进行构建，请在工具执行边界添加审核。在执行前根据已批准的系统、操作和时间限制检查敏感的拟议操作，将不明确或高风险的操作路由给人员，强制实施独立的文件系统和网络限制，保留审核日志，并在审核者或策略不可用时关闭失败。

Codex 自动检查不会自动保护自定义工具或外部线束。使用 [护栏和人工审查](https://developers.openai.com/api/docs/guides/agents/guardrails-approvals#review-cybersecurity-actions-before-execution) 作为智能体 SDK 模式，并使用 [开源审稿人政策](https://github.com/openai/codex/blob/main/codex-rs/core/src/guardian/policy.md) 作为参考。

Codex 产品端沙箱和审查与 [API网络安全检查](https://developers.openai.com/api/docs/guides/safety-checks/cybersecurity) 是分开的。 API 防护措施可能会返回 `cyber_policy` 错误，并且每个用户的 `safety_identifier` 值可以帮助限制防护措施的影响。

<a id="clean-up-and-validate-the-results"></a>

## 清理并验证结果

工作结束后，撤销临时凭证、终止后台进程、删除持久访问并破坏高风险环境。验证是否没有保留任何回调、暴露的工件、共享状态或交叉运行访问，并保持单独的用户、会话和评估隔离。

在采取行动之前先验证调查结果，遵循协调一致的披露做法，并让人们对补救和变更负责。

<a id="before-you-start"></a>

## 开始之前

确认批准的系统和操作、适当的模型、隔离环境、最低特权权限、受限网络访问、受保护的凭据、操作审查、独立监控、紧急停止和清理计划。模型保障、隔离、范围权限、操作审查、监控和人工监督是互补的；没有一个应该是唯一的控制。