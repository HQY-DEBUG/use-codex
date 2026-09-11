> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/managed-configuration.md)。

<a id="managed-configuration"></a>

# 受管配置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

托管配置控件支持 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能的本地运行时行为。支持的要求可能因客户端和版本而异。托管配置不会授予 ChatGPT 工作区访问权限、分配席位或替换工作区基于角色的访问控制 (RBAC)。使用 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md) 进行工作区功能访问，使用此页面进行本地运行时策略。

企业管理员可以通过以下方式控制支持的本地客户端行为：

- **要求**：管理员强制执行的用户无法覆盖的约束。
- **配置默认值**：用户可以覆盖的系统或云管理的 `config.toml` 设置。
- **旧版托管默认值**：支持的客户端启动时应用 `managed_config.toml` 起始值。用户仍然可以在运行过程中更改设置；客户端在下次启动时重新应用这些默认值。

<a id="configure-plugin-marketplaces-and-defaults"></a>

## 配置插件市场和默认值

在系统 `config.toml` 或 [受管配置](https://chatgpt.com/codex/settings/managed-configs) 的 `config.toml` 部分中定义本地或 Git 市场和插件默认值。这些设置是默认设置，并非强制策略。

有关配置密钥，请参阅 [配置参考](../config-file/config-reference.zh-CN.md)；有关覆盖，请参阅 [配置优先级](../config-file/config-basic.zh-CN.md#configuration-precedence)；有关项目级配置，请参阅 [回购插件设置](https://developers.openai.com/plugins/build/plugins#enable-or-disable-a-plugin-for-a-repo)。 [工作区 GitHub 导入和同步](plugin-management.zh-CN.md) 是单独的。

<a id="admin-enforced-requirements-requirementstoml"></a>

## 管理员强制要求 (requirements.toml)

需求限制了安全敏感设置（审批策略、审批审阅者、自动审阅策略、沙箱模式、权限配置文件、Web 搜索模式、托管挂钩，其中MCP用户可以启用的服务器，以及他们可以使用哪些插件市场源）。解析配置时（例如从`config.toml`, [配置文件](../config-file/config-advanced.zh-CN.md#profiles)或 CLI 配置覆盖），如果值与强制规则冲突，本地客户端将回退到兼容值并通知用户。如果您配置`mcp_servers`允许名单，客户端启用MCP仅当其名称和身份均与批准的条目匹配时才提供服务器；否则，客户端将禁用它。

需求还可以通过`requirements.toml`中的`[features]`表来约束[特征标志](../config-file/config-basic.zh-CN.md#feature-flags)。请注意，功能并不总是安全敏感的，但企业可以根据需要固定值。省略的键仍然不受约束。

对于 Codex 0.138.0 或更高版本，首选 [权限配置文件](../permissions.zh-CN.md) 与 `allowed_permission_profiles` 和托管 `default_permissions`。仅将 `allowed_sandbox_modes` 用于仍配置 `sandbox_mode` 的旧部署。

有关确切的密钥列表，请参阅 [配置参考中的 `requirements.toml` 部分](../config-file/config-reference.zh-CN.md#requirementstoml)。

<a id="migrate-the-retired-untrusted-approval-policy"></a>

### 迁移已停用的 `untrusted` 审批策略

Codex 和 ChatGPT Work 不再支持 `approval_policy = "untrusted"`。将其从托管默认值、旧版 `managed_config.toml` 以及设置它的任何用户、项目、配置文件或启动配置中删除。

对于交互式只读使用，请选择具有只读沙箱或托管要求允许的权限配置文件的 `approval_policy = "on-request"`。该沙箱允许的命令无需批准即可运行。

为了保持更严格的命令批准，省略显式的 `approval_policy`，在用户级 `~/.codex/config.toml` 的项目条目中设置 `trust_level = "untrusted"`，并在 `allowed_approval_policies` 中保留 `untrusted`。这也会禁用项目本地配置。设置 `on-request` 显式覆盖该策略。有关示例和安全权衡，请参阅 [从已停用的 `untrusted` 审批策略迁移](../agent-approvals-security.zh-CN.md#migrate-from-the-retired-untrusted-approval-policy)。

<a id="locations-and-precedence"></a>

### 位置和优先级

每个受支持的本地客户端都按优先级从低到高的顺序排列要求：

1. 系统 `requirements.toml`（Unix 系统上为 `/etc/codex/requirements.toml`，包括 Linux 和 macOS，或 Windows 上为 `%ProgramData%\OpenAI\Codex\requirements.toml`）。
2. 云配置包中提供的企业管理的需求。
3. 本地客户端根据要求重新解释的旧版 `managed_config.toml` 字段。
4. 通过 `com.openai.codex:requirements_toml_base64` 交付的 macOS 托管首选项 (MDM)。

优先级较高的层会覆盖较低层中的普通标量和列表值。表按键合并，而规则、挂钩和文件系统限制等要求具有特定于字段的组合行为。对当前架构使用 [`requirements.toml`参考](../config-file/config-reference.zh-CN.md#requirementstoml)，而不是假设每个字段都以相同的方式合并。

为了向后兼容，受支持的本地客户端根据要求重新解释旧版 `approval_policy`、`approvals_reviewer` 和 `sandbox_mode` 字段。此转换在必要时添加了兼容性选择；使用 `requirements.toml` 明确允许列表。

<a id="cloud-managed-requirements"></a>

### 云管理要求

当用户在受支持的计划上使用 ChatGPT 登录时，受支持的本地客户端可以收到与工作区关联的管理员强制要求。这是 `requirements.toml` 兼容保单的交付渠道。它不会授予工作区访问权限或替换工作区 RBAC。身份验证要求必须是 [本地管理](#manage-authentication-locally)。

打开 [受管配置](https://chatgpt.com/codex/settings/managed-configs) 以创建和分配云托管需求。例如，此策略限制批准和沙箱选择，并在受支持的 shell 入口点运行之前进行提示：

```toml
allowed_approval_policies = ["on-request"]
allowed_sandbox_modes = ["read-only", "workspace-write"]

[rules]
prefix_rules = [
  { pattern = [{ any_of = ["bash", "sh", "zsh"] }], decision = "prompt", justification = "Require explicit approval for shell entry points" },
]
```

确认每个托管客户端版本都支持您选择的密钥，并在组织范围内分配之前与小组一起测试策略。使用当前架构的配置参考和当前分配行为的管理界面。

该服务选择适用于登录身份的企业管理的需求层。本地客户端使用 [位置和优先级](#locations-and-precedence) 中描述的其他需求源来评估这些层。使用当前管理界面进行工作区端创建和分配。不要依赖复制的组匹配算法；管理服务拥有该行为，并且可以独立于本地需求格式来更改它。

有关支持的按键和示例，请参阅 [示例需求.toml](#example-requirementstoml) 和 [`requirements.toml`参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

<a id="how-local-clients-apply-cloud-managed-requirements"></a>

#### 本地客户如何应用云托管要求

当用户启动受支持的本地客户端并在受支持的计划上使用 ChatGPT 登录时，客户端首先检查有效的、身份匹配的缓存条目。如果没有可用的有效条目，客户端会重试获取适用的捆绑包，并在成功时写入签名的缓存条目。如果请求失败或超时并且没有可用的有效缓存，云配置包加载将返回错误，而不是在没有云管理的需求层的情况下静默启动。

缓存解析后，客户端将云需求与上述其他需求层组合在一起。后台刷新可以更新缓存以供稍后启动；它不会替换已加载到当前流程中的需求。

<a id="confirm-the-admin-and-employee-experience"></a>

### 确认管理员和员工的体验

分配一个人来拥有每个托管策略，记录哪些用户或组应该接收它，并记录任何文件系统、网络、批准或权限配置文件限制的业务原因。

在扩展部署之前，请与代表用户一起测试已批准的工作流程和有意禁止的工作流程。验证受支持客户端中的有效设置，而不是假设工作区角色或组单独强制执行本地限制。

<a id="manage-authentication-locally"></a>

### 本地管理身份验证

在本地系统 `requirements.toml` 或 macOS MDM 要求中设置 `allowed_login_methods`、`allowed_chatgpt_workspaces`、`cli_auth_credentials_store` 和 `chatgpt_base_url`。 Codex 忽略云托管需求中的这四个字段。本地身份验证要求在凭据加载之前和 Codex 检索云策略之前适用。

要要求 ChatGPT 登录到批准的工作区并将凭据存储在操作系统凭据存储中，请使用：

```toml
allowed_login_methods = ["chatgpt"]
allowed_chatgpt_workspaces = ["00000000-0000-0000-0000-000000000000"]
cli_auth_credentials_store = "keyring"
```

`allowed_login_methods` 接受 `chatgpt`、`api` 或两者。如果省略，此设置不会限制登录方法。如果设置，该列表必须至少包含一种方法。 `api` 允许 API 身份验证，包括 Amazon Bedrock。工作区限制也适用于 [Codex 访问令牌](access-tokens.zh-CN.md)。

用户配置的`forced_login_method`和`forced_chatgpt_workspace_id`必须遵循要求。当用户选择工作区时，它也必须出现在托管工作区白名单中。如果没有匹配的工作区，则 ChatGPT 登录不可用。如果允许，API 身份验证仍然可用。如果没有可用的登录方法，Codex 将拒绝启动。

凭证存储模式和服务URL配置请参见[需求参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

<a id="example-requirementstoml"></a>

### 示例需求.toml

此示例阻止 `--ask-for-approval never` 和 `--sandbox danger-full-access`（包括 `--yolo`）：

```toml
allowed_approval_policies = ["untrusted", "on-request"]
allowed_sandbox_modes = ["read-only", "workspace-write"]
```

这里，`untrusted`保留了源自`trust_level = "untrusted"`的更严格的审批行为；它不会使 `approval_policy = "untrusted"` 成为受支持的显式设置。

<a id="disable-appshots"></a>

### 禁用应用程序快照

要为托管用户禁用 Appshot，请设置顶级 `allow_appshots` 要求：

```toml
allow_appshots = false
```

在应用程序快照可用的情况下，`allow_appshots = false` 会禁用它们。如果您省略该密钥，则要求不会限制 Appshots，并且会应用正常的产品可用性检查。通过`configRequirements/read`读取有效需求的应用服务器客户端受到与`allowAppshots`相同的限制；省略或 `null` `allowAppshots` 值不会禁用 Appshots。

<a id="disable-device-remote-control"></a>

### 禁用设备远程控制

要为托管用户禁用 [设备远程控制](../remote-connections.zh-CN.md#pick-up-work-from-another-device)，请设置顶级 `allow_remote_control` 要求：

```toml
allow_remote_control = false
```

在支持设备远程控制的情况下，`allow_remote_control = false` 会禁用它。如果省略密钥，则要求不会限制设备远程控制，并且适用正常的产品可用性检查。此要求不会禁用 SSH 远程连接。

<a id="control-available-permission-profiles"></a>

### 控制可用的权限配置文件

使用`allowed_permission_profiles`控制哪些内置和自定义[权限配置文件](../permissions.zh-CN.md)用户可以选择。这是与以下内容相对应的权限配置文件`allowed_sandbox_modes`;使用与用户选择权限的方式相匹配的允许列表。

权限配置文件允许列表需要 Codex 0.138.0 或更高版本。 Codex 0.137.0 及更早版本忽略 `allowed_permission_profiles` 和托管 `default_permissions`。

仅在每个托管客户端运行支持版本后才使用下面的权限配置文件示例。在队列升级完成之前，请勿部署托管自定义配置文件。

如果存在，该表是允许的配置文件的完整列表。它允许将配置文件设置为 `true`，并拒绝忽略或设置为 `false` 的配置文件，包括在未来的 Codex 版本中添加的内置程序。

<a id="allow-the-standard-profiles"></a>

#### 允许标准配置文件

此策略允许只读和工作区访问，但不允许完全访问：

```toml
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
# “:danger-full-access”被省略，因此被拒绝。
```

<a id="add-a-managed-least-privilege-default"></a>

#### 添加托管最低权限默认值

管理员可以在同一需求源中定义自定义配置文件。使用特定于组织的配置文件名称，这些名称不会与用户加载的配置中的名称冲突。自定义名称不能以 `:` 开头或使用保留的 `filesystem` 名称。

请勿将托管自定义配置文件部署到运行 Codex 0.137.0 或更早版本的客户端。这些客户端识别配置文件表，但不识别选择它的托管默认值。

例如：

```toml
default_permissions = "acme_review_only"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
acme_review_only = true
# 故意省略“:danger-full-access”，因此被拒绝。

[permissions.acme_review_only]
description = "Review code without modifying the workspace."
extends = ":read-only"
```

<a id="allow-only-enterprise-defined-profiles"></a>

#### 仅允许企业定义的配置文件

当用户应仅选择管理员定义的配置文件时，请忽略所有内置配置文件：

```toml
default_permissions = "acme_workspace"

[allowed_permission_profiles]
acme_workspace = true

[permissions.acme_workspace]
description = "Workspace access with sensitive files denied."
extends = ":workspace"

[permissions.acme_workspace.filesystem]
glob_scan_max_depth = 3

[permissions.acme_workspace.filesystem.":workspace_roots"]
"**/*.env" = "deny"
```

即使用户无法直接选择内置的 `:workspace` 配置文件，自定义配置文件也可以扩展 `:workspace`。

<a id="turn-off-a-profile-allowed-by-another-source"></a>

#### 关闭其他来源允许的配置文件

权限允许列表按配置文件名称组合。由于云需求的优先级高于系统需求，因此云需求可以使用 `false` 关闭系统文件允许的配置文件。

云要求：

```toml
default_permissions = ":read-only"

[allowed_permission_profiles]
":read-only" = true
":workspace" = false
```

系统要求：

```toml
[allowed_permission_profiles]
":read-only" = true
":workspace" = true  # Not honored because cloud requirements set this to false.
```

将 `default_permissions` 显式设置为允许的配置文件。如果省略，则仅当明确允许 `:workspace` 和 `:read-only` 时，本地运行时才默认为 `:workspace`。当 `allowed_permission_profiles` 不存在时，托管要求不会限制用户可以选择的配置文件名称。每个条目必须命名内置配置文件或在加载的配置或需求源中定义的自定义配置文件。在托管需求中定义自定义配置文件以集中控制其行为。

<a id="override-sandbox-requirements-by-host"></a>

### 覆盖主机的沙箱要求

当一项托管策略应在不同主机上应用不同的沙箱要求时，请使用 `[[remote_sandbox_config]]`。例如，您可以为笔记本电脑保留更严格的默认设置，同时允许在匹配的开发盒或 CI 运行器上进行工作区写入。特定于主机的条目当前仅覆盖 `allowed_sandbox_modes`：

```toml
allowed_sandbox_modes = ["read-only"]

[[remote_sandbox_config]]
hostname_patterns = ["*.devbox.example.com", "runner-??.ci.example.com"]
allowed_sandbox_modes = ["read-only", "workspace-write"]
```

本地运行时将每个 `hostname_patterns` 条目与尽力解析的主机名进行比较。它更喜欢可用的完全限定域名，并回退到本地主机名。匹配不区分大小写； `*` 匹配任意字符序列，`?` 匹配一个字符。

在相同的需求源中，第一个匹配的 `[[remote_sandbox_config]]` 条目获胜。如果没有条目匹配，则本地运行时保留顶级 `allowed_sandbox_modes`。主机名匹配仅用于策略选择；不要将其视为经过身份验证的设备证明。

您还可以限制网络搜索模式：

```toml
allowed_web_search_modes = ["cached"] # "disabled" remains implicitly allowed
```

`allowed_web_search_modes = []` 仅允许 `"disabled"`。例如，`allowed_web_search_modes = ["cached"]` 即使在 `danger-full-access` 会话中也会阻止实时 Web 搜索。

<a id="configure-network-access-requirements"></a>

### 配置网络访问要求

<WarningTip>
`[experimental_network]` 是实验性的，可能会发生变化。如果没有在用户运行的本地客户端版本和操作系统上验证这些要求，请勿在整个企业部署中广泛启用这些要求。 Windows 支持仍然有限；除非您已在您的环境中对其进行了测试，否则请避免将此策略应用于 Windows 用户。
</WarningTip>

当管理员应集中定义网络访问要求时，请在 `requirements.toml` 中使用 `[experimental_network]`。这些要求与用户 `features.network_proxy` 切换是分开的：他们可以在没有该功能标志的情况下配置沙箱网络，但当活动沙箱保持网络关闭时，他们不会授予命令网络访问权限。设置`experimental_network.enabled = true`以激活托管代理；单独的域规则不会使代理处于活动状态。

```toml
[experimental_network]
enabled = true
managed_allowed_domains_only = true

[experimental_network.domains]
"api.openai.com" = "allow"
"**.example.com" = "allow"
"blocked.example.com" = "deny"
"**.exfil.example.com" = "deny"
```

仅当您还在 `[experimental_network.domains]` 中定义管理员拥有的 `"allow"` 条目并希望这些规则具有排他性时，才使用 `experimental_network.managed_allowed_domains_only = true`。如果是没有托管允许规则的 `true`，则用户添加的域允许规则不会保持有效。请勿将规范 `domains` 映射与旧版 `allowed_domains` 或 `denied_domains` 列表结合起来。

`*.example.com` 仅匹配子域。 `**.example.com` 匹配顶级域及其子域。匹配的拒绝规则胜过允许规则。

域语法、本地/私有目标规则、拒绝超过允许行为和 DNS 重新绑定限制与 [智能体审批和安全](../agent-approvals-security.zh-CN.md#network-isolation) 中描述的沙箱网络行为相同。

代理路由在沙箱内运行的本地命令。浏览器工具还会在访问源之前检查托管网络拒绝和独占允许列表；这是一个单独的策略检查，不通过命令代理路由浏览器流量。它不会过滤 Web 搜索、应用程序和连接器、MCP 服务器、本机应用程序流量、Codex 服务请求或 Codex 云流量。使用每个使用界面的控件：

- 使用 `allowed_web_search_modes` 限制网页搜索。
- 使用 `features.apps = false` 禁用应用程序和连接器集成，并使用 `features.plugins = false` 禁用支持的插件。
- 使用受管理的 `mcp_servers` 批准列表来限制 MCP 服务器。
- 使用 `browser_use`、`in_app_browser` 和 `computer_use` 等功能要求来限制浏览器和计算机的使用功能。
- 在其云环境设置中配置Codex云网络访问。

命令域白名单不会取代这些特定于功能的控制。

<a id="control-browser-and-computer-use"></a>

### 控制浏览器和计算机的使用

使用 `requirements.toml` 中的 `[browser_use]` 和 `[computer_use]` 表来限制支持的桌面客户端。验证部署中客户端版本和操作系统的策略。配置的允许规则不会安装插件、授予操作系统权限或批准仍需要审核的操作。

对于浏览器访问，配置源站策略。源包括方案、主机和可选端口，例如 `https://example.com` 或 `https://*.example.com:8443`。请勿包含路径、查询或片段。与命令网络域规则不同，浏览器源规则区分 HTTP 和 HTTPS 并匹配端口。

此示例限制浏览器访问已批准的站点，并阻止上传和完整的 Chrome DevTools 协议 (CDP) 访问：

```toml
[browser_use]
allow_history_access = false
allow_global_persistent_approval = false

[browser_use.default_origin_policy]
access = "deny"

[browser_use.origins."https://example.com"]
access = "allow"
uploads = "deny"
downloads = "allow"
full_cdp_access = "deny"
persistent_approval = false
access_approval_lifetime = "turn"
```

匹配的来源规则按字段解析。匹配的拒绝获胜；否则，默认源策略会提供匹配规则未指定的字段。本地配置可以添加限制，但不能放宽托管拒绝。网络拒绝和独占托管网络允许列表仍然适用。

设置 `browser_use.disable_auto_review = true` 以禁用浏览器操作的自动审批审核，或在源策略上设置 `auto_review = "deny"` 以限制该源。这控制审批处理；它不会禁用模型安全监控。

对于本机应用程序，设置默认访问策略并识别允许的应用程序。例如，此 macOS 策略允许计算器并阻止保存的批准：

```toml
[computer_use]
default_app_access = "deny"
allow_persistent_approval = false

[computer_use.macos.bundle_ids]
"com.apple.calculator" = "allow"
```

Windows 策略可以识别带有 `computer_use.windows.aumids` 的打包应用程序或带有 `computer_use.windows.exes` 的可执行文件。可执行规则需要 `publisher_name`、`product_name` 和 `access`； `binary_name` 是可选的。使用应用程序的经过验证的身份，而不是单独使用其显示名称。

有关完整字段，请参阅 [配置参考](../config-file/config-reference.zh-CN.md#requirementstoml)；有关托管 macOS 设备，请参阅 [锁定使用限制](#restrict-locked-computer-use)。

<a id="pin-feature-flags"></a>

### 引脚功能标志

您还可以为接收托管 `requirements.toml` 的用户固定 [特征标志](../config-file/config-basic.zh-CN.md#feature-flags)：

```toml
[features]
personality = true
unified_exec = false

# 需要时禁用特定于使用界面的功能。
browser_use = false
browser_use_full_cdp_access = false
browser_use_external = false
in_app_browser = false
in_app_updates = false
computer_use = false
```

使用 `config.toml` 的 `[features]` 表中的规范功能键来获取运行时功能。本地运行时规范已识别的功能以满足这些引脚的要求，并拒绝对 `config.toml` 或配置文件功能设置的冲突写入。

<a id="disable-codex-feature-surfaces"></a>

- `in_app_browser = false` 禁用内置浏览器窗格。
- `in_app_updates = false` 在重新启动时禁用 ChatGPT 桌面应用程序自己的更新程序（如果支持）。它不会影响外部包部署或扩展对旧应用程序版本的支持。有关设置和部署指南，请参阅 [管理应用程序更新](manage-app-updates.zh-CN.md)。
- `browser_use = false` 禁用浏览器中的计算机使用和浏览器智能体可用性。
- `browser_use_full_cdp_access = false` 在本地运行时禁用完整的 CDP 访问，包括浏览器开发人员模式，并阻止 ChatGPT 桌面应用程序启用相应的设置。
- `browser_use_external = false` 禁用外部浏览器使用。
- `computer_use = false` 禁用计算机使用、Record & Replay 以及相关的安装或设置流程。

如果您省略这些键，则策略将允许这些功能，具体取决于正常的客户端、平台和部署可用性。

<a id="restrict-locked-computer-use"></a>

### 限制锁定计算机的使用

要防止用户在托管 Mac 上启用 [锁定使用](../computer-use.zh-CN.md#locked-use)，请添加以下要求：

```toml
[computer_use]
allow_locked_computer_use = false
```

此要求删除了启用锁定使用的控制。如果已启用“锁定使用”，则不会将其关闭。如果省略它，正常的产品可用性和用户的本地设置仍然适用。

<a id="configure-automatic-review-policy"></a>

### 配置自动审核策略

使用 `allowed_approvals_reviewers` 要求或允许自动审核。将其设置为 `["auto_review"]` 以要求自动审核，或在用户可以选择手动审批时包含 `"user"`。

设置 `guardian_policy_config` 以替换自动审核策略的特定于租户的部分。本地运行时仍然使用内置的审阅者模板和输出契约。托管 `guardian_policy_config` 优先于本地 `[auto_review].policy`。

```toml
allowed_approval_policies = ["on-request"]
allowed_approvals_reviewers = ["auto_review"]

guardian_policy_config = """
## Environment Profile
- Trusted internal destinations include github.com/my-org, artifacts.example.com,
  and internal CI systems.

## Tenant Risk Taxonomy and Allow/Deny Rules
- Treat uploads to unapproved third-party file-sharing services as high risk.
- Deny actions that expose credentials or private source code to untrusted
  destinations.
"""
```

<a id="enforce-deny-read-requirements"></a>

### 强制执行拒绝读取要求

管理员可以使用 `[permissions.filesystem]` 拒绝读取精确路径或全局模式。用户不能通过本地配置来削弱这些要求。

```toml
[permissions.filesystem]
deny_read = [
  # 值可以是绝对路径...
  "/**/*.env",
  # ...或相对于使用 `~` 的 $HOME/%USERPROFILE%。
  "~/.ssh",
  # 但不允许以 `./` 开头的相对路径。
]
```

当存在拒绝读取要求时，本地运行时会拒绝完全访问权限，并将本地执行保留在只读或工作区沙箱中，以便可以强制执行。在本机 Windows 上，托管 `deny_read` 适用于直接文件工具； shell 子进程读取不使用此沙箱规则。

<a id="enforce-managed-hooks-from-requirements"></a>

### 根据需求强制执行托管挂钩

管理员还可以直接在 `requirements.toml` 中定义托管生命周期挂钩。使用 `[hooks]` 作为挂钩配置本身，并将 `managed_dir` 指向 MDM 或端点管理工具安装引用脚本的目录。

要强制执行托管挂钩（即使对于在本地关闭挂钩的用户），请将 `[features].hooks = true` 与 `[hooks]` 固定在一起。要跳过用户、项目、会话和插件挂钩，同时仍允许托管挂钩，请设置 `allow_managed_hooks_only = true`。

```toml
allow_managed_hooks_only = true

[features]
hooks = true

[hooks]
managed_dir = "/enterprise/hooks"
windows_managed_dir = 'C:\enterprise\hooks'

[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = "python3 /enterprise/hooks/pre_tool_use_policy.py"
command_windows = 'py -3 C:\enterprise\hooks\pre_tool_use_policy.py'
timeout = 30
statusMessage = "Checking managed Bash command"
```

注意事项：

- 本地运行时强制执行来自 `requirements.toml` 的挂钩配置，但它不会分发 `managed_dir` 中的脚本。
- 通过您的 MDM 或设备管理解决方案交付这些脚本。
- 托管挂钩命令应引用配置的托管目录下的绝对脚本路径。
- `allow_managed_hooks_only = true` 跳过来自用户、项目、会话和插件源的挂钩，但仍然加载来自 `requirements.toml` 和其他托管配置层的挂钩。

<a id="enforce-command-rules-from-requirements"></a>

### 根据要求强制执行命令规则

管理员还可以使用 `[rules]` 表从 `requirements.toml` 强制执行限制性命令规则。这些规则与常规 `.rules` 文件合并，限制性最强的决定仍然获胜。

与 `.rules` 不同，需求规则必须指定 `decision`，并且该决策必须是 `"prompt"` 或 `"forbidden"`（不是 `"allow"`）。

```toml
[rules]
prefix_rules = [
  { pattern = [{ token = "rm" }], decision = "forbidden", justification = "Use git clean -fd instead." },
  { pattern = [{ token = "git" }, { any_of = ["push", "commit"] }], decision = "prompt", justification = "Require review before mutating history." },
]
```

要限制本地客户端可以启用哪些 MCP 服务器，请添加 `mcp_servers` 批准列表。对于stdio服务器，匹配`command`；对于流式 HTTP 服务器，匹配 `url`：

```toml
[mcp_servers.docs]
identity = { command = "codex-mcp" }

[mcp_servers.remote]
identity = { url = "https://example.com/mcp" }
```

`identity.command` 的字符串形式仅与配置的 `command` 匹配。它不检查 `args`、`cwd`、`env` 或 `env_vars`。

要约束完整的 stdio 调用，请匹配可执行文件和每个位置参数：

```toml
[mcp_servers.internal.identity]
command = { executable = "/usr/local/bin/codex-mcp", args = [
  { match = "exact", value = "serve" },
  { match = "prefix", value = "--workspace=" },
] }
```

可执行文件、参数计数和参数顺序必须匹配。参数和 URL 规则支持 `exact`、`prefix` 和全值 `regex` 匹配。结构化命令规则仍然不检查 ​​`cwd`、`env` 或 `env_vars`。插件捆绑的 MCP 服务器在“plugins”下使用相同的身份形状。<plugin>.mcp_服务器。<server>`.

如果 `mcp_servers` 存在但为空，则本地客户端将禁用所有 MCP 服务器。

<a id="control-plugin-availability"></a>

### 控制插件可用性

要关闭支持的本地客户端中的插件，请在 `requirements.toml` 中将 `features.plugins` 设置为 `false`：

```toml
features.plugins = false
```

当用户使用 API 密钥登录 Codex 时，此设置也适用。有关支持的配置，请参阅 [`features.plugins`参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

<a id="restrict-plugin-marketplace-sources"></a>

### 限制插件市场来源

要限制插件市场来源，请设置 `restrict_to_allowed_sources = true` 并定义一个或多个来源规则：

```toml
[marketplaces]
restrict_to_allowed_sources = true

[marketplaces.allowed_sources.company_plugins]
source = "git"
url = "https://github.com/example/company-plugins.git"
ref = "main"

[marketplaces.allowed_sources.internal_git]
source = "host_pattern"
host_pattern = '^git\.example\.com$'

[marketplaces.allowed_sources.local_plugins]
source = "local"
path = "/opt/company/codex-plugins"
```

Git 规则与规范化仓库 URL 以及精确的 `ref`（如果存在）匹配。主机模式是与小写 Git 主机匹配的正则表达式；使用 `^` 和 `$` 进行整个主机匹配。本地规则需要绝对的、规范化的路径。有关完整架构和合并行为，请参阅 [`requirements.toml`参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

这些要求拒绝不匹配的市场添加、插件安装和配置的 Git 市场刷新操作。他们还在运行时过滤配置的市场及其插件。

OpenAI 管理的 Git 市场（包括 API 密钥目录）也必须与源许可名单匹配。要允许它们，请包含以下没有 `ref` 约束的 Git 源：

```toml
[marketplaces.allowed_sources.openai_curated]
source = "git"
url = "https://github.com/openai/plugins.git"
```

要排除精选目录，请忽略该源并确保没有更广泛的主机规则允许它。捆绑插件和远程安装的工作区插件与此策划的 Git 源策略是分开的。

这些源限制仅适用于本地客户端支持插件市场操作的情况：桌面应用程序中的 ChatGPT 和 Codex 以及 Codex CLI。他们不控制 Web 或移动设备上 ChatGPT 中插件的使用，也不向 IDE 扩展添加插件。

<a id="managed-defaults-managed_configtoml"></a>

## 托管默认值 (`managed_config.toml`)

托管默认设置设置受支持的本地客户端启动时的配置。启动时，它们会覆盖用户的本地 `config.toml` 和任何 CLI `--config` 覆盖。用户仍然可以在当前运行期间更改这些设置，并且默认值会在客户端下次启动时再次应用。

如果使用 ChatGPT 登录的用户的托管默认值、macOS MDM 配置文件或保存的配置引脚 `gpt-5.4` 或 `gpt-5.4-mini`，请在 2026 年 8 月 31 日之前更新。将 `gpt-5.4` 替换为 `gpt-5.6-terra`，将 `gpt-5.4-mini` 替换为 `gpt-5.6-luna`。使用您自己的 API 密钥进行身份验证的 OpenAI API 和 Codex 不受影响。参见 [工作区模型可用性](workspace-model-availability.zh-CN.md#prepare-for-the-gpt-54-retirement)。

确保您的托管默认值满足您的要求；本地运行时拒绝不允许的值。

<a id="precedence-and-layering"></a>

### 优先级和分层

本地运行时按以下顺序组装有效配置（顶部覆盖底部）：

- 托管首选项（macOS MDM；最高优先级）
- `managed_config.toml`（系统/管理文件）
- `config.toml`（用户基本配置）

CLI `--config key=value` 覆盖适用于基础，但管理层覆盖它们。这意味着即使您提供本地标志，每次运行也会从托管默认值开始。

云 `config.toml` 使用 [正常配置优先级](../config-file/config-basic.zh-CN.md#configuration-precedence)，而不是上面的旧排序。云`requirements.toml`使用[需求优先级](#locations-and-precedence)。

<a id="locations"></a>

### 地点

- Linux/macOS (Unix)：`/etc/codex/managed_config.toml`
- Windows/非 Unix：`~/.codex/managed_config.toml`

如果文件丢失，本地运行时将跳过管理层。

<a id="macos-managed-preferences-mdm"></a>

### macOS 托管首选项 (MDM)

在 macOS 上，管理员可以将提供 base64 编码的 TOML 有效负载的设备配置文件推送到：

- 优先域：`com.openai.codex`
- 按键：
  - `config_toml_base64`（托管默认值）
  - `requirements_toml_base64`（要求）

本地运行时将这些“托管首选项”有效负载解析为 TOML。对于托管默认值 (`config_toml_base64`)，托管首选项具有最高优先级。对于需求 (`requirements_toml_base64`)，优先级遵循上述云管理需求顺序。相同的需求方`[features]`表在`requirements_toml_base64`中工作；在那里也使用规范的功能键。

<a id="mdm-setup-workflow"></a>

### MDM 设置工作流程

本地运行时支持标准 macOS MDM 负载，因此您可以使用 `Jamf Pro`、`Fleet` 或 `Kandji` 等工具分发设置。轻量级部署如下所示：

1. 构建托管有效负载 TOML 并使用 `base64` 对其进行编码（无包装）。
2. 将字符串拖放到 `com.openai.codex` 域下的 MDM 配置文件中，位于 `config_toml_base64`（托管默认值）或 `requirements_toml_base64`（要求）。
3. 推送配置文件，然后要求用户重新启动支持的本地客户端并确认启动配置摘要反映了托管值。
4. 撤销或更改策略时，更新托管负载；客户端在下次启动时读取刷新的首选项。

避免在有效负载中嵌入秘密或高变化的动态值。像更改控制下的任何其他 MDM 设置一样对待托管 TOML。

<a id="example-managed_configtoml"></a>

### 示例 Managed_config.toml

```toml
# 设置保守的默认值
approval_policy = "on-request"
sandbox_mode    = "workspace-write"

[sandbox_workspace_write]
network_access = false             # keep network disabled unless explicitly allowed

[otel]
environment = "prod"
exporter = "otlp-http"            # point at your collector
log_user_prompt = false            # keep prompts redacted
# 出口商详细信息位于出口商表下；请参阅上面的监控和遥测
```

<a id="recommended-guardrails"></a>

### 推荐护栏

- 首选 `workspace-write`，获得大多数用户的认可；保留受控容器的完全访问权限。
- 保留 `network_access = false`，除非您的安全审查允许您的工作流程所需的收集器或域。
- 使用托管配置固定 OTel 设置（导出器、环境），但保留 `log_user_prompt = false`，除非您的策略明确允许存储提示内容。
- 定期审核本地 `config.toml` 和托管策略之间的差异以捕捉偏差；管理层应该赢得本地标志和文件。