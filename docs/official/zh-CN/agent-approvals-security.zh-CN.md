> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/agent-approvals-security.md)。

<a id="agent-approvals--security"></a>

# 智能体审批与安全

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 有助于保护您的代码和数据并降低误用的风险。

本页面介绍如何安全操作 Codex，包括沙箱、审批和网络访问。如果您正在寻找用于扫描连接的 GitHub 仓库的产品 Codex Security，请参阅 [Codex Security 概览](security.zh-CN.md)。

默认情况下，智能体在网络访问关闭的情况下运行。在本地，Codex 使用操作系统强制的沙箱来限制它可以接触的内容（通常是当前工作区），以及控制何时必须停止并在采取行动之前询问您的批准策略。

有关沙箱如何在 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中工作的高级说明，请参阅 [沙箱](sandboxing.zh-CN.md)。有关更广泛的企业安全概述，请参阅 [Codex 安全白皮书](https://trust.openai.com/?itemUid=382f924d-54f3-43a8-a9df-c39e6c959958&source=click)。

<a id="migrate-from-the-retired-untrusted-approval-policy"></a>

## 从已停用的 `untrusted` 审批策略迁移

Codex 和 ChatGPT Work 不再支持 `approval_policy = "untrusted"`。停用的设置可能会阻止任一客户端启动。从用户或项目配置、配置文件、启动脚本和托管默认值中将其删除。对于交互式、只读用途：

```toml
sandbox_mode = "read-only"
approval_policy = "on-request"
```

或者运行`codex --sandbox read-only --ask-for-approval on-request`。

使用 `on-request`，沙箱允许的命令可以在未经批准的情况下运行、读取可访问的文件以及使用网络访问（如果启用）。

要保留更严格的命令批准规则，请省略显式 `approval_policy` 并将项目条目添加到用户级别 `~/.codex/config.toml`：

```toml
[projects."/path/to/project"]
trust_level = "untrusted"
```

除非执行策略规则允许，否则命令需要批准。这也会禁用项目本地配置。显式设置 `on-request` 会覆盖项目派生策略；托管 `allowed_approval_policies` 必须包含 `untrusted` 才能允许它。

<a id="sandbox-and-approvals"></a>

## 沙箱和批准

Codex 安全控制来自两个协同工作的层：

- **沙箱模式**：Codex在执行模型生成的命令时在技术上可以做什么（例如，它可以在哪里写入以及是否可以到达网络）。
- **审批政策**：当 Codex 在执行操作之前必须询问您时（例如，离开沙箱、使用网络或运行受信任集之外的命令）。

Codex 根据运行位置使用不同的沙箱模式：

- **Codex云**：在隔离的 OpenAI 管理的容器中运行，防止访问主机系统或不相关的数据。使用两阶段运行时模型：安装程序在智能体阶段之前运行，并且可以访问网络来安装指定的依赖项，然后智能体阶段默认脱机运行，除非您为该环境启用 Internet 访问。为云环境配置的机密仅在设置期间可用，并在智能体阶段开始之前删除。
- **Codex CLI/IDE 扩展**：操作系统级机制强制执行沙箱策略。默认值包括无网络访问权限和仅限于活动工作区的写入权限。您可以根据您的风险承受能力配置沙箱、审批策略和网络设置。

在`Auto`预设（例如`--sandbox workspace-write --ask-for-approval on-request`）中，Codex可以自动读取工作目录中的文件、进行编辑和运行命令。

Codex 请求批准在工作区之外编辑文件或运行需要网络访问的命令。如果您想在不进行更改的情况下聊天或计划，请使用 `/permissions` 命令切换到 `read-only` 模式。

Codex 还可以引发对宣传副作用的应用程序（连接器）工具调用的批准，即使该操作不是 shell 命令或文件更改。当工具公布破坏性注释时，破坏性 app/MCP 工具调用始终需要批准（除非该工具公布读取注释，该注释具有优先权）。

<a id="safety-monitoring-and-paused-tasks"></a>

## 安全监控和暂停任务

GPT-6 Astra 包括 Codex 和 ChatGPT Work 中的安全监控。监控异步运行，如果检测到潜在的不安全模型行为，可以暂停任务。暂停可以在触发暂停的活动之后到达；监控不会取代沙箱、权限或结果审查。

如果任务暂停，请阅读通知并查看可用的结果。仅在检查任务可以安全继续后才恢复。如果通知显示任务已结束或不提供恢复选项，则您无法从该使用界面恢复。

| 使用界面和数据控制 | 结果和简历 |
| ------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Codex 和 ChatGPT Work 客户端具有结果和恢复流程，没有此处列出的数据控制 | 在恢复之前查看结果。                      |
| Codex CLI 和移动 | 无法获得完整的调查结果和简历。任务结束。 |
| 零数据保留、修改滥用监控或非美国数据存储驻留 | 无法获得完整的调查结果和简历。任务结束。 |

安全监控评估任务期间的模型行为。 [自动审批审核](sandboxing/auto-review.zh-CN.md) 评估在运行之前需要批准的各个操作。通过自动审批审核批准的操作仍然可以是稍后监控暂停的任务的一部分。

<a id="network-access"></a>

## 网络接入<ElevatedRiskBadge class="ml-2" />

对于 Codex 云，请参阅 [智能体互联网接入](cloud/internet-access.zh-CN.md) 以启用完全互联网访问或域允许列表。

对于 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展，默认的 `workspace-write` 沙箱模式会保持网络访问关闭，除非您在配置中启用它：

```toml
[sandbox_workspace_write]
network_access = true
```

<a id="network-isolation"></a>

### 网络隔离

网络访问通过适用于命令生成的脚本、程序和子进程的目标规则进行控制。当命令网络访问已启用时，打开 `network_proxy` 功能以将该流量限制为您配置的网络策略。添加域规则本身不会启用代理。

```toml
[features.network_proxy]
enabled = true
domains = { "api.openai.com" = "allow", "example.com" = "deny" }
```

对于一次性 CLI 会话，当您只需要切换时使用布尔简写，当您还设置策略选项时使用表格形式：

```bash
codex \
  -c 'features.network_proxy=true' \
  -c 'sandbox_workspace_write.network_access=true'

codex \
  -c 'features.network_proxy.enabled=true' \
  -c 'features.network_proxy.domains={ "api.openai.com" = "allow", "example.com" = "deny" }' \
  -c 'sandbox_workspace_write.network_access=true'
```

该功能改变了启用网络访问的实施方式；它本身不授予网络访问权限。使用 `sandbox_workspace_write.network_access` 和 `workspace-write` 配置来决定命令是否具有网络访问权限：

- 网络关闭 + `network_proxy` 打开：网络保持关闭，并且该功能不执行任何操作。
- 网络打开 + `network_proxy` 关闭：网络保持打开状态，不受限制的直接出站访问。
- Network on + `network_proxy` on：网络保持打开状态，出站流量受配置的网络策略限制。

代理功能也适用于 [权限配置文件](permissions.zh-CN.md#network-permissions)。配置文件的 `network.enabled = true` 授予命令网络访问权限，而 `features.network_proxy = true` 激活该配置文件的域规则的强制执行：

```toml
default_permissions = "project-edit"

[features]
network_proxy = true

[permissions.project-edit]
extends = ":workspace"

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"api.openai.com" = "allow"
```

如果在此示例中省略代理功能，命令将具有直接网络访问权限，并且 `api.openai.com` 允许规则不会限制其目的地。

管理员管理的 `experimental_network` 要求与用户功能切换是分开的。他们可以在没有 `features.network_proxy` 的情况下配置和启动沙箱网络，但当活动沙箱将其关闭时，他们不会打开网络访问。管理员端`requirements.toml`形状请参见[受管配置](enterprise/managed-configuration.zh-CN.md#configure-network-access-requirements)。

<a id="network-policy"></a>

#### 网络政策

域规则是白名单优先：

- 精确的主机只匹配它们自己。
- `*.example.com` 匹配 `api.example.com` 等子域，但不匹配 `example.com`。
- `**.example.com` 匹配顶点域和子域。
- 全局 `*` 允许规则匹配任何未被拒绝的公共主机。将 `*` 视为广泛的网络访问，并尽可能选择范围规则。
- `deny`总是胜过`allow`，全局`*`仅对allow规则有效。

<a id="local-and-private-destinations"></a>

#### 本地和私人目的地

默认情况下，`allow_local_binding = false` 阻止环回、链路本地和私有目标：

- 特定例外：当命令需要一个本地目标时，添加精确的本地 IP 文字或 `localhost` 允许规则。
- 更广泛的访问：仅当您有意想要更广泛的本地/私人访问时才设置 `allow_local_binding = true`。
- 通配符：通配符规则不计为显式本地例外。
- 已解析地址：解析为本地/私有 IP 的主机名即使与白名单匹配，仍会被阻止。

<a id="dns-rebinding-protections"></a>

#### DNS 重新绑定保护

在允许主机名之前，Codex 会尽力执行 DNS 和 IP 分类检查：

- 失败或超时的查找将被阻止。
- 解析为非公共地址的主机名将被阻止。
- 该检查可降低 DNS 重新绑定风险，但并不能消除风险。完全防止重新绑定需要通过传输层固定已解析的 IP。

如果敌对 DNS 在范围内，也在较低层实施出口控制。

<a id="dangerous-settings"></a>

#### 危险环境

有两种设置故意拓宽信任边界：

- `dangerously_allow_non_loopback_proxy = true` 可以在环回之外公开代理侦听器。
- `dangerously_allow_all_unix_sockets = true` 绕过 Unix 套接字白名单。

仅在严格控制的环境中使用它们。启用 Unix 套接字代理后，即使请求非环回绑定，侦听器也将保持仅环回状态，因此沙箱网络不会成为本地守护程序的远程桥梁。

`network_proxy` 默认关闭。当您启用它时：

| 设置 | 默认 | 行为 |
| -------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enabled` | `false` | 仅当命令网络访问已打开时启动沙箱网络。                                                                                                           |
| `domains` | 未设置 | 使用允许列表行为，因此在添加 `allow` 规则之前不允许任何外部目标。支持精确主机、范围通配符和全局 `*` 允许规则； `deny` 总是获胜。 |
| `unix_sockets` | unset | 在添加显式 `allow` 规则之前，不允许任何 Unix 套接字目标。                                                                                                         |
| `allow_local_binding` | `false` | 阻止本地和专用网络目标，除非您添加精确的本地 IP 文字或 `localhost` 允许规则，或明确选择更广泛的本地/专用访问。                |
| `enable_socks5` | `true` | 在策略允许时公开 SOCKS5 支持。                                                                                                                                         |
| `enable_socks5_udp` | `true` | 当 SOCKS5 可用时，允许通过 SOCKS5 进行 UDP。                                                                                                                                      |
| `allow_upstream_proxy` | `true` | 让沙箱网络尊重环境中的上游代理。                                                                                                               |
| `dangerously_allow_non_loopback_proxy` | `false` | 将侦听器端点保持在环回状态，除非您故意将它们公开到本地主机之外。                                                                                            |
| `dangerously_allow_all_unix_sockets` | `false` | 保持 Unix 套接字访问基于白名单，除非您故意绕过该保护。                                                                                              |

<a id="traffic-outside-the-command-network-proxy"></a>

### 命令网络代理外部的流量

网络代理过滤在本地命令沙箱内运行的脚本、程序和子进程。它不会过滤 Web 搜索、应用程序或连接器工具调用、MCP 服务器连接、浏览器或计算机使用活动、Codex 云任务或客户端的模型和身份验证请求。这些使用界面使用单独的服务连接、功能设置、工作区策略或环境控制。

浏览器工具在访问源之前单独检查托管网络拒绝和独占允许列表。浏览器来源策略可以进一步限制站点访问、上传、下载和开发人员工具。参见 [托管浏览器控件](enterprise/managed-configuration.zh-CN.md#control-browser-and-computer-use)。

对于托管用户，将命令网络策略与 `allowed_web_search_modes`、批准的 `mcp_servers` 等控件以及应用程序、插件、浏览器或计算机使用的功能要求相结合。参见 [受管配置](enterprise/managed-configuration.zh-CN.md)。

您还可以控制 [网络搜索工具](https://platform.openai.com/docs/guides/tools-web-search)，而无需授予对生成命令的完全网络访问权限。 Codex 默认使用网络搜索缓存来访问结果。缓存是 OpenAI 维护的 Web 结果索引，因此缓存模式返回预先索引的结果，而不是获取实时页面。这可以减少来自任意实时内容的提示注入的风险，但您仍应将 Web 结果视为不可信。如果您使用的是 `--yolo` 或其他 [完全访问沙箱设置](#common-sandbox-and-approval-combinations)，网络搜索默认为实时结果。使用 `--search` 或设置 `web_search = "live"` 以允许实时浏览，或将其设置为 `"disabled"` 以关闭该工具：

```toml
web_search = "cached"  # default
# web_search = "disabled"
# web_search = "live"  # same as --search
```

当外部 Web 访问应由搜索索引控制时，设置 `web_search = "indexed"`。在 Codex 中启用网络访问或网页搜索时请小心。提示注入可能会导致智能体获取并遵循不受信任的指令。

<a id="defaults-and-recommendations"></a>

## 默认值和建议

- 启动时，Codex 会检测该文件夹是否受版本控制并建议：
  - 版本控制文件夹：`Auto`（工作区写入+按需批准）
  - 非版本控制文件夹：`read-only`
- 根据您的设置，Codex 也可能在 `read-only` 中启动，直到您明确信任工作目录（例如，通过入门提示或 `/permissions`）。
- 工作区包括当前目录和 `/tmp` 等临时目录。使用 `/status` 命令查看工作区中有哪些目录。
- 要接受默认值，请运行 `codex`。
- 您可以明确设置这些：
  - `codex --sandbox workspace-write --ask-for-approval on-request`
  - `codex --sandbox read-only --ask-for-approval on-request`

<a id="protected-paths-in-writable-roots"></a>

### 可写根中的受保护路径

在默认的 `workspace-write` 沙箱策略中，可写根仍然包含受保护的路径：

- `<writable_root>/.git` 被保护为只读，无论它显示为目录还是文件。
- 如果`<writable_root>/.git` is a pointer file (`gitdir: ...`)，解析的 Git 目录路径也被保护为只读。
- `<writable_root>/.agents` 当作为目录存在时被保护为只读。
- `<writable_root>/.codex` 当作为目录存在时被保护为只读。
- 保护是递归的，因此这些路径下的所有内容都是只读的。

<a id="run-without-approval-prompts"></a>

### 运行时无批准提示

您可以使用 `--ask-for-approval never` 或 `-a never`（简写）禁用批准提示。

此选项适用于所有 `--sandbox` 模式，因此您仍然可以控制 Codex 的自主级别。 Codex 在您设定的限制范围内尽最大努力。

如果您需要 Codex 在没有批准提示的情况下通过网络访问读取文件、进行编辑和运行命令，请使用 `--sandbox danger-full-access`（或 `--dangerously-bypass-approvals-and-sandbox` 标志）。这样做之前请务必小心。

对于中间立场，`approval_policy = { granular = { ... } }` 允许您保持特定批准提示类别的交互性，同时自动拒绝其他类别。细粒度策略涵盖沙箱批准、执行策略规则提示、MCP 提示、`request_permissions` 提示和技能脚本批准。

<a id="automatic-approval-reviews"></a>

### 自动审批审核

默认情况下，批准请求会发送给您：

```toml
approvals_reviewer = "user"
```

当批准是交互式的时，自动批准审查适用，例如 `approval_policy = "on-request"` 或精细的批准策略。设置 `approvals_reviewer = "auto_review"` 在 Codex 运行请求之前通过审阅者智能体路由符合条件的审批请求：

```toml
approval_policy = "on-request"
approvals_reviewer = "auto_review"
```

有关完整的审阅者生命周期、触发条件、配置优先级和失败行为，请参阅 [自动审核](sandboxing/auto-review.zh-CN.md)。

审核者仅评估已经需要批准的操作，例如沙箱升级、阻止的网络请求、`request_permissions` 提示或副作用应用程序和 MCP 工具调用。留在沙箱内的操作将继续进行，无需额外的审核步骤。

审核者策略检查数据泄露、凭证探测、持续的安全削弱和破坏性操作。在政策允许的情况下，低风险和中等风险的行动可以进行。该政策拒绝采取重大风险行动。高风险操作需要足够的用户授权且没有匹配的拒绝规则。提示构建、审查会话和解析失败失败关闭。超时单独出现，但操作仍然不运行。

[默认审稿人政策](https://github.com/openai/codex/blob/main/codex-rs/core/src/guardian/policy.md) 位于开源 Codex 仓库中。企业可以在托管需求中将其特定于租户的部分替换为 `guardian_policy_config`。还支持本地 `[auto_review].policy` 文本，但托管要求优先。有关设置详细信息，请参阅 [受管配置](enterprise/managed-configuration.zh-CN.md#configure-automatic-review-policy)。

在 ChatGPT 桌面应用程序中，这些审核显示为自动审核项目，状态为“正在审核”、“已批准”、“已拒绝”、“已中止”或“已超时”。它们还可以包括针对已审核请求的风险级别和用户授权评估。

自动审核使用额外的模型调用，因此可以增加 Codex 的使用量。管理员可以使用 `allowed_approvals_reviewers` 对其进行约束。

<a id="common-sandbox-and-approval-combinations"></a>

### 常见的沙箱和审批组合

| 意图 | 标志/配置 | 效果 |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 自动（预设） | _无需标志_ 或 `--sandbox workspace-write --ask-for-approval on-request` | Codex 可以在工作区中读取文件、进行编辑和运行命令。 Codex 需要获得批准才能在工作区外进行编辑或访问网络。 |
| 安全只读浏览 | `--sandbox read-only --ask-for-approval on-request` | Codex 可以在只读沙箱内读取文件并运行命令。沙箱之外的操作可能需要批准。                            |
| 只读非交互式（CI） | `--sandbox read-only --ask-for-approval never` | Codex 可以在只读沙箱内读取文件并运行命令；它从不请求批准。                                                  |
| 自动审核模式 | `--sandbox workspace-write --ask-for-approval on-request -c approvals_reviewer=auto_review` 或 `approvals_reviewer = "auto_review"` | 与标准按需模式相同的沙箱边界，但合格的批准请求由自动审核审核，而不是呈现给用户。  |
| 危险完全访问 | `--dangerously-bypass-approvals-and-sandbox`（别名：`--yolo`） |<ElevatedRiskBadge />没有沙箱；没有批准_（不推荐）_ |

对于非交互式运行，请使用 `codex exec --sandbox workspace-write`； Codex 将旧的 `codex exec --full-auto` 调用保留为已弃用的兼容性路径并打印警告。

<a id="configuration-in-configtoml"></a>

#### `config.toml`中的配置

有关更广泛的配置工作流程，请参阅 [配置基础知识](config-file/config-basic.zh-CN.md)、[高级配置](config-file/config-advanced.zh-CN.md#approval-policies-and-sandbox-modes) 和 [配置参考](config-file/config-reference.zh-CN.md)。

```toml
# 使用只读沙箱进行交互式审批
approval_policy = "on-request"
sandbox_mode    = "read-only"
allow_login_shell = false # optional hardening: disallow login shells for shell-based tools

# 可选：允许网络处于工作区写入模式
[sandbox_workspace_write]
network_access = true

# 可选：精细的审批策略
# approval_policy = { granular = {
#   sandbox_approval = true,
#   rules = true,
#   mcp_elicitations = true,
#   request_permissions = false,
#   skill_approval = false
# } }
```

您还可以将预设保存为 [配置文件](config-file/config-advanced.zh-CN.md#profiles)，然后使用 `codex --profile profile-name` 选择它们：

```toml
# 〜/.codex/full_auto.config.toml
approval_policy = "on-request"
sandbox_mode    = "workspace-write"
```

```toml
# 〜/.codex/readonly_quiet.config.toml
approval_policy = "never"
sandbox_mode    = "read-only"
```

<a id="test-the-sandbox-locally"></a>

### 本地测试沙箱

要查看命令在 Codex 沙箱下运行时会发生什么，请使用以下 Codex CLI 命令：

```bash
# macOS
codex sandbox macos [--permissions-profile <name>] [--log-denials] [COMMAND]...
# Linux
codex sandbox linux [--permissions-profile <name>] [COMMAND]...
# 窗户
codex sandbox windows [--permissions-profile <name>] [COMMAND]...
```

`sandbox` 命令也可用作 `codex debug`，并且平台助手具有别名（例如 `codex sandbox seatbelt` 和 `codex sandbox landlock`）。

<a id="os-level-sandbox"></a>

## 操作系统级沙箱

Codex 根据您的操作系统以不同的方式强制实施沙箱：

- **macOS** 使用安全带策略，并使用 `sandbox-exec` 和与您选择的 `--sandbox` 模式对应的配置文件 (`-p`) 运行命令。当受限读取访问启用平台默认设置时，Codex 会附加精心策划的 macOS 平台策略（而不是广泛允许 `/System`）以保留常见工具兼容性。
- **Linux** 默认使用 `bwrap` 加 `seccomp`。
- **窗户** 在 [适用于 Linux 的 Windows 子系统 2 (WSL2)](windows/wsl.zh-CN.md) 中运行时使用 Linux 沙箱实现。通过 Codex `0.114` 支持 WSL1；从 `0.115` 开始，Linux 沙箱移至 `bwrap`，因此不再支持 WSL1。在 Windows 上本机运行时，Codex 使用 [Windows沙箱](windows/windows-sandbox.zh-CN.md#windows-sandbox) 实现。

如果您在Windows上使用Codex IDE扩展，它直接支持WSL2。在 VS Code 设置中进行以下设置，以便在可用时将智能体保留在 WSL2 中：

```json
{
  "chatgpt.runCodexInWindowsSubsystemForLinux": true
}
```

这可确保 IDE 扩展继承 Linux 沙箱语义以进行命令、批准和文件系统访问，即使主机操作系统是 Windows 也是如此。在 [WSL指南](windows/wsl.zh-CN.md) 中了解更多信息。

在Windows上原生运行时，在`config.toml`中配置原生沙箱模式：

```toml
[windows]
sandbox = "unelevated" # or "elevated"
# sandbox_private_desktop = true  # default; set false only for compatibility
```

详细信息请参见 [Windows 设置指南](windows/windows-sandbox.zh-CN.md#windows-sandbox)。

当您在 Docker 等容器化环境中运行 Linux 时，如果主机或容器配置阻止 Codex 所需的命名空间、setuid `bwrap` 或 `seccomp` 操作，则沙箱可能无法工作。

在这种情况下，配置 Docker 容器以提供所需的隔离，然后在容器内使用 `--sandbox danger-full-access`（或 `--dangerously-bypass-approvals-and-sandbox` 标志）运行 `codex`。

<a id="run-codex-in-dev-containers"></a>

### 在开发容器中运行 Codex

如果您的主机无法直接运行 Linux 沙箱，或者您的组织已经标准化了容器化开发，请使用 Dev Containers 运行 Codex，并让 Docker 提供外部隔离边界。这适用于 Visual Studio Code Dev Containers 和兼容工具。

使用 [Codex 安全开发容器示例](https://github.com/openai/codex/tree/main/.devcontainer) 作为参考实现。该示例安装 Codex、常用开发工具、`bubblewrap` 和基于防火墙的出站控制。

开发容器提供了实质性的保护，但它们并不能阻止每一次攻击。如果您在容器内运行 Codex 和 `--sandbox danger-full-access` 或 `--dangerously-bypass-approvals-and-sandbox`，则恶意项目可以窃取 devcontainer 内可用的任何内容，包括 Codex 凭据。仅将此模式用于受信任的仓库，并像在任何其他提升的环境中一样监视 Codex 活动。

参考实现包括：

- 安装了 Codex 和常用开发工具的 Ubuntu 24.04 基础镜像；
- 用于出站访问的白名单驱动的防火墙配置文件；
- 用于在容器中重新打开工作区的 VS Code 设置和扩展建议；
- 命令历史记录和 Codex 配置的持久安装；
- `bubblewrap`，因此当容器授予所需功能时，Codex 仍然可以使用其 Linux 沙箱。

尝试一下：

1. 安装 Visual Studio Code 和 [开发容器扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)。
2. 将 Codex 示例 `.devcontainer` 设置复制到您的仓库中，或直接从 Codex 仓库启动。
3. 在 VS Code 中，运行 **开发容器：打开容器中的文件夹...** 并选择 `.devcontainer/devcontainer.secure.json`。
4. 容器启动后，打开终端并运行 `codex`。

您还可以从 CLI 启动容器：

```bash
devcontainer up --workspace-folder . --config .devcontainer/devcontainer.secure.json
```

该示例包含三个主要部分：

- `.devcontainer/devcontainer.secure.json` 控制容器设置、功能、安装、环境变量和 VS Code 扩展。
- `.devcontainer/Dockerfile.secure` 定义了基于 Ubuntu 的映像和安装的工具。
- `.devcontainer/init-firewall.sh`应用出网策略。

参考防火墙有意作为一个起点。如果您依赖域白名单进行隔离，请实施适合您环境的 DNS 重新绑定和 DNS 刷新保护，例如 TTL 感知刷新或 DNS 感知防火墙。

在容器内，选择以下模式之一：

- 如果开发容器配置文件授予 `bwrap` 创建内部沙箱所需的功能，请保持 Codex 的 Linux 沙箱启用。
- 如果容器是您预期的安全边界，请在容器内运行 Codex 和 `--sandbox danger-full-access`，这样 Codex 就不会尝试创建第二个沙箱层。

<a id="version-control"></a>

## 版本控制

Codex 最适合版本控制工作流程：

- 在功能分支上工作并在委派之前保持 `git status` 干净。这使得 Codex 补丁更容易隔离和恢复。
- 与直接编辑跟踪文件相比，更喜欢基于补丁的工作流程（例如 `git diff`/`git apply`）。经常提交，以便您可以小幅度回滚。
- 像对待任何其他 PR 一样对待 Codex 建议：运行有针对性的验证、审查差异并记录提交消息中的决策以进行审核。

<a id="monitoring-and-telemetry"></a>

## 监控和遥测

Codex 支持通过 OpenTelemetry (OTel) 选择加入监控，以帮助团队审核使用情况、调查问题并满足合规性要求，而不会削弱本地安全默认设置。遥测默认关闭；在您的配置中明确启用它。

<a id="overview"></a>

### 概述

- Codex 默认关闭 OTel 导出以保持本地运行独立。
- 启用后，Codex 会发出结构化日志事件，涵盖聊天、API 请求、SSE/WebSocket 流活动、用户提示（默认情况下已编辑）、工具审批决策和工具结果。
- Codex 使用 `service.name`（发起者）、CLI 版本和环境标签来标记导出的事件，以分隔开发/登台/生产流量。

<a id="enable-otel-opt-in"></a>

### 启用 OTel（选择加入）

将 `[otel]` 块添加到 Codex 配置（通常为 `~/.codex/config.toml`），选择导出器以及是否记录提示文本。

```toml
[otel]
environment = "staging"   # dev | staging | prod
exporter = "none"          # none | otlp-http | otlp-grpc
log_user_prompt = false     # redact prompt text unless policy allows
```

- `exporter = "none"` 使仪器保持活动状态，但不向任何地方发送数据。
- 要将事件发送到您自己的收集器，请选择以下选项之一：

```toml
[otel]
exporter = { otlp-http = {
  endpoint = "https://otel.example.com/v1/logs",
  protocol = "binary",
  headers = { "x-otlp-api-key" = "${OTLP_TOKEN}" }
}}
```

```toml
[otel]
exporter = { otlp-grpc = {
  endpoint = "https://otel.example.com:4317",
  headers = { "x-otlp-meta" = "abc123" }
}}
```

Codex 批处理事件并在关闭时刷新它们。 Codex 仅导出其 OTel 模块生成的遥测数据。

<a id="event-categories"></a>

### 活动类别

代表性事件类型包括：

- `codex.conversation_starts`（模型、推理设置、沙箱/审批策略）
- `codex.api_request`（尝试、状态/成功、持续时间和错误详细信息）
- `codex.sse_event`（流事件类型、成功/失败、持续时间以及 `response.completed` 上的令牌计数）
- `codex.websocket_request` 和 `codex.websocket_event`（请求持续时间加上每条消息的类型/成功/错误）
- `codex.user_prompt`（长度；内容经过编辑，除非明确启用）
- `codex.tool_decision`（批准/拒绝，来源：配置与用户）
- `codex.tool_result`（持续时间、成功、输出片段）

相关 OTel 指标（计数器加持续时间直方图对）包括 `codex.api_request`、`codex.sse_event`、`codex.websocket.request`、`codex.websocket.event` 和 `codex.tool.call`（以及相应的 `.duration_ms` 仪器）。

有关完整的事件目录和配置参考，请参阅 [GitHub 上的 Codex 配置文档](https://github.com/openai/codex/blob/main/docs/config.md#otel)。

<a id="security-and-privacy-guidance"></a>

### 安全和隐私指导

- 保留 `log_user_prompt = false` 除非策略明确允许存储提示内容。提示可以包括源代码和敏感数据。
- 仅将遥测数据发送给您控制的收集器；应用符合您的合规性要求的保留限制和访问控制。
- 将工具参数和输出视为敏感的。尽可能在收集器或 SIEM 上进行编辑。
- 如果您不希望 Codex 将会话记录保存在 `CODEX_HOME` 下，请检查本地数据保留设置（例如，`history.persistence` / `history.max_bytes`）。请参见 [高级配置](config-file/config-advanced.zh-CN.md#history-persistence) 和 [配置参考](config-file/config-reference.zh-CN.md)。
- 如果您在关闭网络访问的情况下运行 CLI，OTel 导出将无法到达您的收集器。要导出，请允许 OTel 端点在 `workspace-write` 模式下进行网络访问，或者从 Codex 云导出（收集器域位于您批准的列表中）。
- 定期查看事件以了解批准/沙箱更改和意外的工具执行。

OTel 是可选的，旨在补充而不是取代上述沙箱和批准保护。

<a id="managed-configuration"></a>

## 受管配置

企业管理员可以为其 [受管配置](enterprise/managed-configuration.zh-CN.md) 中的工作区配置 Codex 安全设置。请参阅该页面了解设置和策略详细信息。