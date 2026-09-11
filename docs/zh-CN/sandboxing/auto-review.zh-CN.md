> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/sandboxing/auto-review.md)。

<a id="auto-review"></a>

# 自动审核机制

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

自动审核使用单独的审核智能体取代了沙箱边界的手动审批。主要的 Codex 智能体仍然在同一个沙箱内运行，具有相同的审批策略以及相同的网络和文件系统限制。区别在于谁审查符合条件的升级请求。

自动审核仅适用于交互式审批。实际上，这意味着 `approval_policy = "on-request"` 或仍显示相关提示类别的精细审批策略。对于`approval_policy = "never"`，没有什么可评论的。

在 ChatGPT 桌面应用程序中，选择已批准的 Daybreak 模型会自动将权限控制切换到 **代我批准**（当您的帐户可用且组织策略允许该模式时）。当您使用桌面应用程序的 `/model` 命令时，这也适用。如果该模式不可用，则当前权限模式保持不变。模型选择永远不会凌驾于受管理组织的要求之上。

在为批准的安全模型启用 **完全访问权限** 之前，ChatGPT 桌面应用程序会显示有关危险操作的特定于模型的警告。该警告建议改为 **代我批准** 并链接到 [审阅者策略配置](#configuration)。该警告不会恢复沙箱边界或覆盖组织策略。

<a id="how-auto-review-works"></a>

## 自动审核的工作原理

从高层次来看，流程是：

1. 主智能体在`read-only`或`workspace-write`内部工作。
2. 当需要跨越沙箱边界时，它会请求批准。
3. 如果是 `approvals_reviewer = "auto_review"`，则 Codex 将该批准请求路由至单独的审阅者智能体，而不是停下来找人。
4. 审核者决定是否应运行该操作并返回理由。
5. 如果该操作获得批准，则继续执行。如果被拒绝，则指示主智能体寻找一条实质上更安全的路径或停下来询问用户。

自动审阅是审阅者交换，而不是权限授予。它不会扩展 `writable_roots`、启用网络访问或削弱受保护的路径。它仅更改 Codex 处理已需要批准的操作的方式。

<a id="when-it-triggers"></a>

## 当它触发时

自动审核会评估批准请求，否则这些请求会因人工而暂停。这些包括：

- 请求升级沙箱权限的 Shell 或 exec 工具调用。
- 当前沙箱或策略阻止的网络请求。
- 在允许的可写根目录之外进行文件编辑。
- MCP 或应用程序工具调用需要根据其工具注释或配置的批准模式进行批准。
- 计算机 使用访问新网站或域的权限。

对于沙箱内已允许的例行操作，不会运行自动审核。如果命令可以在活动的 `sandbox_mode` 下运行，或者工具调用保持在允许的策略内，则主智能体将继续执行而无需审核。

计算机使用是一个单独的案例。计算机使用的应用程序批准仍然直接向用户显示，因此自动审核不会取代这些应用程序级提示。

<a id="what-auto-review-blocks"></a>

## 自动审核会阻止哪些内容

在较高层面上，自动审核旨在阻止以下操作：

- 将私人数据、秘密或凭据发送到不受信任的目的地
- 探测凭证、令牌、cookie 或会话材料
- 广泛或持续的安全削弱
- 具有重大不可逆转损害风险的破坏性行为

确切的策略位于开源 Codex 仓库中：[策略模板.md](https://github.com/openai/codex/blob/main/codex-rs/core/src/guardian/policy_template.md) 和 [政策.md](https://github.com/openai/codex/blob/main/codex-rs/core/src/guardian/policy.md)。该策略可以使用 `guardian_policy_config` 为每个企业自定义，也可以使用本地 [`[auto_review].policy`](/codex/config-file/config-advanced#approval-policies-and-sandbox-modes) 为每个用户自定义。

<a id="what-the-reviewer-sees"></a>

## 审稿人看到了什么

审核者本身是一个 Codex 智能体，其工作范围比主智能体更窄：决定是否应该运行特定的跨越边界操作。

审阅者会看到一份简洁的文字记录以及准确的批准请求。这通常包括用户消息、显示的助手更新、相关工具调用和工具输出，以及现在提议批准的操作。它还可以执行只读检查以收集丢失的上下文，但很少这样做。

不包括隐藏的辅助推理。自动审查会看到保留的聊天项目和工具证据，而不是私人思想链。

<a id="denials-and-failure-behavior"></a>

## 否认和失败行为

显式拒绝不会被视为普通的沙箱错误。 Codex 将审核理由返回给主智能体，并添加更强的指令：

- 不要通过变通办法、间接执行或政策规避来追求相同的结果。
- 仅继续使用实质上更安全的替代方案。
- 否则，请停下来询问用户。

Codex 每匝还应用一个抑制断路器。在当前的开源实现中，自动审查会在 `3` 连续拒绝或 `10` 在同一轮中最后一次 `50` 审查的滚动窗口内拒绝后中断轮次。

任何非拒绝都会重置连续拒绝计数器。当断路器跳闸时，Codex 会发出警告并通过中断中止当前轮次，而不是让智能体循环进行更多升级尝试。

超时与明确的拒绝分开出现，并且主智能体被告知仅超时并不能证明该操作不安全。

对于被拒绝的操作还有一个显式的覆盖路径。在当前的开源 TUI 中，运行 `/approve` 以打开 **自动审核拒绝** 选择器，然后选择最近被拒绝的一项操作以批准一次重试。 Codex 每个任务最多记录 10 个最近的拒绝。这种批准的范围很窄：它适用于确切被拒绝的行动，而不是类似的未来行动；它被记录为在相同上下文中的一次重试；并且重试仍然经过自动审核。在幕后，Codex 为该确切操作注入了开发人员范围的批准标记。然后，审阅者将显式用户覆盖视为上下文，但它仍然遵循策略，并且如果策略表明用户无法覆盖该拒绝类别，则可以再次拒绝。

<a id="configuration"></a>

## 配置

有关设置详细信息，请参阅 [受管配置](../enterprise/managed-configuration.zh-CN.md#configure-automatic-review-policy)。

默认审阅者策略位于开源 Codex 仓库中：[核心/src/guardian/policy.md](https://github.com/openai/codex/blob/main/codex-rs/core/src/guardian/policy.md)。企业可以在托管需求中将其特定于租户的部分替换为 `guardian_policy_config`。个人用户还可以设置本地 [`[auto_review].policy`](/codex/config-file/config-advanced#approval-policies-and-sandbox-modes) in their `config.toml`，但托管要求优先：

```toml
[auto_review]
policy = """
YOUR POLICY GOES HERE
"""
```

要自定义策略，请首先复制整个默认策略措辞，然后根据您的个人风险状况进行迭代。

<a id="configure-an-authorized-cybersecurity-engagement"></a>

## 配置授权的网络安全参与

对于授权的安全工作，将自动审查与书面参与范围和最低权限 [权限配置文件](../permissions.zh-CN.md) 结合起来。使用批准的实验室目标，记录操作和参与窗口，并将生产系统、不相关的主机、凭据和持久更改保留在范围之外，除非明确授权。

`[auto_review].policy` 和 `guardian_policy_config` 均取代您当前的审阅者政策。它们不会与与您的模型捆绑在一起或由您的组织管理的策略合并。内置审核说明和回复格式仍然适用。在使用任一示例之前，请复制完整的当前策略，保留每个现有规则，并为已批准的工作添加规则。将大写占位符替换为完整的策略。如果您无法访问当前策略，请勿覆盖它。

以下本地 `config.toml` 模板启用审核并在现有审核者策略之后添加范围条件：

```toml
approval_policy = "on-request"
approvals_reviewer = "auto_review"
default_permissions = ":workspace"

[auto_review]
policy = """
PASTE THE COMPLETE ACTIVE REVIEWER POLICY HERE BEFORE USING THIS EXAMPLE.

## Environment Profile
- Authorized target: lab.example.com.
- Approved actions: inspect the target, reproduce authorized vulnerabilities,
  and validate fixes within the documented engagement window.

## Tenant Risk Taxonomy and Allow/Deny Rules
- Allow only actions against the approved target that match the documented
  engagement scope and approved actions.
- Deny out-of-scope or unknown hosts, production access, credential theft,
  persistence, data exfiltration, destructive operations, and policy bypass.
- Deny ambiguous actions and high-impact changes until a human explicitly
  approves the exact target, action, and side effects.
"""
```

将示例目标和允许的操作替换为实际批准的范围。使用独立的文件系统和网络规则实施目标限制；审稿人的指示不会取代这些界限。

组织可以在托管 `requirements.toml` 中强制执行相同的条件：

```toml
allowed_approval_policies = ["on-request"]
allowed_approvals_reviewers = ["auto_review"]
allowed_sandbox_modes = ["read-only", "workspace-write"]
default_permissions = ":workspace"

guardian_policy_config = """
PASTE THE COMPLETE ACTIVE REVIEWER POLICY HERE BEFORE USING THIS EXAMPLE.

## Environment Profile
- Authorized target: lab.example.com.

## Tenant Risk Taxonomy and Allow/Deny Rules
- Allow only approved actions against the documented engagement target.
- Deny out-of-scope hosts, production access, credential theft, persistence,
  data exfiltration, destructive operations, and attempts to bypass policy.
- Deny ambiguous or high-impact actions until a human explicitly approves the
  exact target, action, and side effects.
"""

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
# “:danger-full-access”被省略，因此被拒绝。
```

`allowed_permission_profiles` 控制当前权限配置文件。 `allowed_sandbox_modes` 还可以防止仍使用旧版 `sandbox_mode` 的部署中的完全访问。

托管 `guardian_policy_config` 优先于用户本地 `[auto_review].policy`。保留 `approval_policy = "on-request"` 或其他符合条件的交互式审批策略，并保留可执行的沙箱边界。使用 `approval_policy = "never"`、`:danger-full-access` 或 `--yolo`，操作可以避免创建审核所需的跨边界审批请求。

允许列表中的网络目标本身不会触发审核。当沙箱内的操作仍必须到达审阅者时，将显式 [命令规则](../agent-configuration/rules.zh-CN.md) 与 `decision = "prompt"` 添加，或配置敏感的 MCP 工具以要求批准。

请参阅 [模型和可信访问](../cyber-safety.zh-CN.md) 和 [推荐配置](../cyber-safety/recommended-configuration.zh-CN.md) 了解模型访问、参与设置和自定义智能体工作流程。有关企业优先级和支持的客户端版本，请参阅 [受管配置](../enterprise/managed-configuration.zh-CN.md#configure-automatic-review-policy)。对于自定义 API 或 Agents SDK 工具，请使用 [护栏和人工审查](https://developers.openai.com/api/docs/guides/agents/guardrails-approvals#review-cybersecurity-actions-before-execution)。

<a id="reduce-review-volume-without-weakening-security"></a>

## 在不削弱安全性的情况下减少审核量

当沙箱已经涵盖您常见的安全工作流程时，自动审核效果最佳。如果有太多日常操作需要审核，请首先确定边界，而不是教审核者永远批准嘈杂的升级。

在实践中，影响力最大的变化是：

- 为临时目录或您有意使用的相邻仓库添加窄 [`writable_roots`](../config-file/config-advanced.zh-CN.md#approval-policies-and-sandbox-modes)。
- 添加范围狭窄的 [前缀规则](../agent-configuration/rules.zh-CN.md)。优先选择精确的命令前缀（例如 `["cargo", "test"]` 或 `["pnpm", "run", "lint"]`），而不是广泛的模式（例如 `["python"]` 或 `["curl"]`）。宽泛的规则常常会抹掉自动审查本来要保护的边界。

默认情况下，自动检查会话记录保留在 `~/.codex/sessions` 下，因此您可以要求 Codex 在更改策略或权限之前分析那里的过去流量。

<a id="limits"></a>

## 限制

自动审查改进了长时间运行的代理工作的默认操作点，但它并不是确定性的安全保证。

- 它仅评估要求跨越边界的操作。
- 它仍然可能会犯错误，尤其是在对抗性或不寻常的情况下。
- 它应该补充而不是取代良好的沙箱设计、监控和特定于组织的策略。

有关研究原理和已发表的评估结果，请参阅 [关于自动审核的对齐研究帖子](https://alignment.openai.com/auto-review/)。