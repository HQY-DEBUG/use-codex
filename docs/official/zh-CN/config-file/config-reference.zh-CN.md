> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/config-file/config-reference.md)。

<a id="configuration-reference"></a>

# 配置项参考

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用此页面作为 Codex 配置文件的可搜索参考。有关概念指导和示例，请从 [配置基础知识](config-basic.zh-CN.md) 和 [高级配置](config-advanced.zh-CN.md) 开始。

<a id="configtoml"></a>

## `config.toml`

用户级配置位于 `~/.codex/config.toml` 中。您还可以在 `.codex/config.toml` 文件中添加项目范围的覆盖。仅当您信任项目时，Codex 才会加载项目范围的配置文件。

项目范围的配置无法覆盖计算机本地提供程序、身份验证、主机拥有的应用程序请求元数据、通知、配置文件选择或遥测路由密钥。 Codex 忽略 `openai_base_url`、`chatgpt_base_url`、`apps_mcp_product_sku`、`model_provider`、`model_providers`、`notify`、`profile`、`profiles`、`experimental_realtime_ws_base_url` 和`otel` 当它们出现在项目本地 `.codex/config.toml` 中时；将提供程序、通知和遥测密钥放在用户级配置中。将[配置文件](config-advanced.zh-CN.md#profiles)配置为`$CODEX_HOME/profile-name.config.toml`旁边的`config.toml`；选择带有 `--profile profile-name` 的一项。

对于沙箱和批准密钥（`approval_policy`、`sandbox_mode` 和 `sandbox_workspace_write.*`），请将此参考与 [沙箱和批准](../agent-approvals-security.zh-CN.md#sandbox-and-approvals)、[可写根中的受保护路径](../agent-approvals-security.zh-CN.md#protected-paths-in-writable-roots) 和 [网络接入](../agent-approvals-security.zh-CN.md#network-access) 配对。有关 beta 权限配置文件，请参阅 [权限](../permissions.zh-CN.md)。

Codex 和 ChatGPT Work 不再支持 `approval_policy = "untrusted"`。删除该设置或选择支持的策略。仍然支持用户级别 `~/.codex/config.toml` 中带有 `trust_level = "untrusted"` 的项目条目。有关示例和批准权衡，请参阅 [从已停用的 `untrusted` 审批策略迁移](../agent-approvals-security.zh-CN.md#migrate-from-the-retired-untrusted-approval-policy)。

<ConfigTable
  options={[
    {
      key: "model",
      type: "string",
      description: "要使用的模型（例如，`gpt-5.5`）。",
    },
    {
      key: "review_model",
      type: "string",
      description:
        "`/review` 使用的可选模型覆盖（默认为当前会话模型）。",
    },
    {
      key: "model_provider",
      type: "string",
      description: "来自 `model_providers` 的提供商 ID（默认值：`openai`）。",
    },
    {
      key: "openai_base_url",
      type: "string",
      description:
        "内置 `openai` 模型提供程序的基本 URL 覆盖。",
    },
    {
      key: "model_context_window",
      type: "number",
      description: "可用于活动模型的上下文窗口标记。",
    },
    {
      key: "model_auto_compact_token_limit",
      type: "number",
      description:
        "触发自动历史记录压缩的令牌阈值（未设置使用模型默认值）。",
    },
    {
      key: "model_auto_compact_token_limit_scope",
      type: "total | body_after_prefix",
      description:
        "控制自动压缩阈值是计算完整的活动上下文（`total`，默认值）还是仅计算携带压缩窗口前缀（`body_after_prefix`）之后的增长。",
    },
    {
      key: "model_catalog_json",
      type: "string (path)",
      description:
        "启动时加载的 JSON 模型目录的可选路径。选定的 `$CODEX_HOME/profile-name.config.toml` 配置文件可以覆盖每个配置文件的此设置。",
    },
    {
      key: "oss_provider",
      type: "lmstudio | ollama",
      description:
        "使用 `--oss` 运行时使用的默认本地提供程序（如果未设置，则默认提示）。",
    },
    {
      key: "approval_policy",
      type: "on-request | never | { granular = { sandbox_approval = bool, rules = bool, mcp_elicitations = bool, request_permissions = bool, skill_approval = bool } }",
      description:
        "控制 Codex 在执行命令之前何时暂停以供批准。您还可以使用 `approval_policy = { granular = { ... } }` 允许或自动拒绝特定提示类别，同时保持其他提示交互。不支持 `untrusted`，不推荐使用 `on-failure`；使用 `on-request` 进行交互式运行，或使用 `never` 进行非交互式运行。",
    },
    {
      key: "approval_policy.granular.sandbox_approval",
      type: "boolean",
      description:
        "当`true`时，允许出现沙箱升级批准提示。",
    },
    {
      key: "approval_policy.granular.rules",
      type: "boolean",
      description:
        "当`true`时，允许显示由execpolicy `prompt`规则触发的批准。",
    },
    {
      key: "approval_policy.granular.mcp_elicitations",
      type: "boolean",
      description:
        "当 `true`、MCP 诱导提示被允许出现而不是被自动拒绝时。",
    },
    {
      key: "approval_policy.granular.request_permissions",
      type: "boolean",
      description:
        "当 `true` 时，允许出现来自 `request_permissions` 工具的提示。",
    },
    {
      key: "approval_policy.granular.skill_approval",
      type: "boolean",
      description:
        "当 `true` 时，允许出现技能脚本批准提示。",
    },
    {
      key: "approvals_reviewer",
      type: "user | auto_review",
      description:
        "谁根据 `on-request` 或精细审批策略审核合格的审批提示。默认为`user`； `auto_review` 使用审阅者子智能体。此设置不会更改沙箱或审查沙箱内已允许的操作。",
    },
    {
      key: "auto_review.policy",
      type: "string",
      description:
        "用于自动审核的本地 Markdown 政策说明。受管理的 `guardian_policy_config` 优先。空白值将被忽略。",
    },
    {
      key: "allow_login_shell",
      type: "boolean",
      description:
        "允许基于 shell 的工具使用登录 shell 语义。默认为`true`；当`false`、`login = true`请求被拒绝并省略时，`login`默认为非登录shell。",
    },
    {
      key: "sandbox_mode",
      type: "read-only | workspace-write | danger-full-access",
      description:
        "命令执行期间文件系统和网络访问的沙箱策略。",
    },
    {
      key: "sandbox_workspace_write.writable_roots",
      type: "array<string>",
      description:
        '`sandbox_mode = "workspace-write"` 时附加可写根。',
    },
    {
      key: "sandbox_workspace_write.network_access",
      type: "boolean",
      description:
        "允许工作区写入沙箱内的出站网络访问。",
    },
    {
      key: "sandbox_workspace_write.exclude_tmpdir_env_var",
      type: "boolean",
      description:
        "在工作区写入模式下从可写根中排除 `$TMPDIR`。",
    },
    {
      key: "sandbox_workspace_write.exclude_slash_tmp",
      type: "boolean",
      description:
        "在工作区写入模式下从可写根中排除 `/tmp`。",
    },
    {
      key: "windows.sandbox",
      type: "unelevated | elevated",
      description:
        "在 Windows 上本机运行 Codex 时仅限 Windows 的本机沙箱模式。",
    },
    {
      key: "windows.sandbox_private_desktop",
      type: "boolean",
      description:
        "默认情况下，在本机 Windows 上的私有桌面上运行最终的沙箱子进程。设置 `false` 仅是为了与旧版 `Winsta0\\\\Default` 行为兼容。",
    },
    {
      key: "browser_use.allow_history_access",
      type: "boolean",
      description:
        "设置为 `false` 以限制浏览器历史记录访问。托管需求可以强制执行此限制。",
    },
    {
      key: "browser_use.default_origin_policy",
      type: "table",
      description:
        "后备浏览器来源限制。支持`access`、`uploads`、`downloads`和`full_cdp_access`，分别设置为`allow`或`deny`。",
    },
    {
      key: "browser_use.origins.<origin>",
      type: "table",
      description:
        "每个源浏览器限制与 `browser_use.default_origin_policy` 具有相同的字段。包括 HTTP 或 HTTPS 方案和可选端口；省略路径、查询和片段。地方价值观不能放松有管理的否认。",
    },
    {
      key: "computer_use.default_app_access",
      type: "allow | deny",
      description:
        "计算机使用的后备本机应用程序访问策略。应用程序特定的条目可以提供策略；本地配置不能放松托管限制。",
    },
    {
      key: "computer_use.macos.bundle_ids",
      type: "map<string, allow | deny>",
      description: "本机 macOS 应用程序访问由捆绑包标识符键入。",
    },
    {
      key: "computer_use.windows.aumids",
      type: "map<string, allow | deny>",
      description:
        "打包的 Windows 应用程序访问由应用程序用户模型 ID (AUMID) 键入。",
    },
    {
      key: "computer_use.windows.exes",
      type: "array<table>",
      description:
        "Windows 可执行访问规则。每个规则需要 `publisher_name`、`product_name` 和 `access`（`allow` 或 `deny`）； `binary_name` 是可选的。",
    },
    {
      key: "computer_use.windows.always_allowed_app_ids",
      type: "array<string>",
      description:
        "计算机使用可以在没有提示的情况下打开的 Windows 应用程序标识符。不在列表中的应用程序需要批准；从 ChatGPT 桌面应用程序的计算机使用设置中删除已保存的条目。",
    },
    {
      key: "notify",
      type: "array<string>",
      description:
        "为通知调用的命令；从 Codex 接收 JSON 有效负载。",
    },
    {
      key: "check_for_update_on_startup",
      type: "boolean",
      description:
        "启动时检查 Codex 更新（仅当更新集中管理时设置为 false）。",
    },
    {
      key: "feedback.enabled",
      type: "boolean",
      description:
        "启用通过 `/feedback` 跨本地客户端提交反馈（默认值：true）。",
    },
    {
      key: "analytics.enabled",
      type: "boolean",
      description:
        "启用或禁用此计算机/配置文件的分析。未设置时，将应用客户端默认值。",
    },
    {
      key: "instructions",
      type: "string",
      description:
        "保留以供将来使用；更喜欢 `model_instructions_file` 或 `AGENTS.md`。",
    },
    {
      key: "developer_instructions",
      type: "string",
      description:
        "注入会话的其他开发人员说明（可选）。",
    },
    {
      key: "log_dir",
      type: "string (path)",
      description:
        "Codex写入日志文件的目录；默认为 `$CODEX_HOME/log`。显式设置此选项还会启用该目录中的选择加入纯文本 TUI 日志 `codex-tui.log`。",
    },
    {
      key: "sqlite_home",
      type: "string (path)",
      description:
        "Codex 存储智能体作业和其他可恢复运行时状态使用的 SQLite 支持的状态数据库的目录。",
    },
    {
      key: "compact_prompt",
      type: "string",
      description: "历史压缩提示的内联覆盖。",
    },
    {
      key: "model_instructions_file",
      type: "string (path)",
      description:
        "替换内置指令而不是 `AGENTS.md`。",
    },
    {
      key: "personality",
      type: "none | friendly | pragmatic",
      description:
        "宣传 `supportsPersonality` 的模型的默认通讯方式；可以按线程/转或通过 `/personality` 进行覆盖。",
    },
    {
      key: "service_tier",
      type: "string",
      description:
        "新轮次的首选服务级别。使用 `fast` 或现用模型宣传的其他级别； `fast` 映射到请求值 `priority`。",
    },
    {
      key: "experimental_compact_prompt_file",
      type: "string (path)",
      description:
        "从文件加载压缩提示覆盖（实验性）。",
    },
    {
      key: "skills.max_context_tokens",
      type: "integer (positive)",
      description:
        "可用技能目录的Token预算。默认为模型上下文窗口的 2%。显式值的上限为 `10000` 令牌。",
    },
    {
      key: "skills.config",
      type: "array<object>",
      description: "每个技能的启用覆盖存储在 config.toml 中。",
    },
    {
      key: "skills.config.<index>.path",
      type: "string (path)",
      description: "包含 `SKILL.md` 的技能文件夹的路径。",
    },
    {
      key: "skills.config.<index>.enabled",
      type: "boolean",
      description: "启用或禁用引用的技能。",
    },
    {
      key: "apps.<id>.enabled",
      type: "boolean",
      description:
        "通过 id 启用或禁用特定应用程序/连接器（默认值：true）。",
    },
    {
      key: "apps._default.enabled",
      type: "boolean",
      description:
        "所有应用程序的默认应用程序启用状态，除非每个应用程序被覆盖。",
    },
    {
      key: "apps._default.destructive_enabled",
      type: "boolean",
      description:
        "默认允许/拒绝 `destructive_hint = true` 的应用程序工具。",
    },
    {
      key: "apps._default.open_world_enabled",
      type: "boolean",
      description:
        "默认允许/拒绝 `open_world_hint = true` 的应用程序工具。",
    },
    {
      key: "apps._default.approvals_reviewer",
      type: "user | auto_review",
      description:
        "应用程序工具批准提示的默认审阅者，除非按应用程序覆盖。省略时，应用程序将继承顶级 `approvals_reviewer` 值。",
    },
    {
      key: "apps._default.default_tools_approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "应用程序工具的默认审批行为，无需按应用程序或按工具覆盖。",
    },
    {
      key: "apps.<id>.destructive_enabled",
      type: "boolean",
      description:
        "允许或阻止此应用程序中宣传 `destructive_hint = true` 的工具。",
    },
    {
      key: "apps.<id>.open_world_enabled",
      type: "boolean",
      description:
        "允许或阻止此应用程序中宣传 `open_world_hint = true` 的工具。",
    },
    {
      key: "apps.<id>.default_tools_enabled",
      type: "boolean",
      description:
        "除非存在每个工具的覆盖，否则此应用程序中工具的默认启用状态。",
    },
    {
      key: "apps.<id>.approvals_reviewer",
      type: "user | auto_review",
      description:
        "此应用程序的工具批准提示的审阅者。覆盖 `apps._default.approvals_reviewer`。",
    },
    {
      key: "apps.<id>.default_tools_approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "除非存在每个工具的覆盖，否则此应用程序中工具的默认批准行为。",
    },
    {
      key: "apps.<id>.tools.<tool>.enabled",
      type: "boolean",
      description:
        "应用程序工具的每个工具启用覆盖（例如 `repos/list`）。",
    },
    {
      key: "apps.<id>.tools.<tool>.approval_mode",
      type: "auto | prompt | writes | approve",
      description: "单个应用程序工具的每个工具审批行为覆盖。",
    },
    {
      key: "tool_suggest.discoverables",
      type: "array<table>",
      description:
        '允许针对其他可发现的连接器或插件提供工具建议。每个条目使用 `type = "connector"` 或 `"plugin"` 和 `id`。',
    },
    {
      key: "tool_suggest.disabled_tools",
      type: "array<table>",
      description:
        '禁用针对特定可发现连接器或插件的建议。每个条目使用 `type = "connector"` 或 `"plugin"` 和 `id`。',
    },
    {
      key: "features.apps",
      type: "boolean",
      description:
        "启用应用程序（连接器）集成（稳定；默认启用）。应用程序和连接器流量不受沙箱命令网络代理或其域白名单控制。",
    },
    {
      key: "features.hooks",
      type: "boolean",
      description:
        "启用从 `hooks.json` 或内联 `[hooks]` 配置加载的生命周期挂钩。 `features.codex_hooks` 是已弃用的别名。",
    },
    {
      key: "features.code_mode.enabled",
      type: "boolean",
      description:
        "启用代码模式功能配置。此功能正在开发中，默认情况下处于关闭状态。",
    },
    {
      key: "features.code_mode.excluded_tool_namespaces",
      type: "array<string>",
      description:
        "工具命名空间代码模式排除了嵌套代码模式工具指导和执行器暴露。",
    },
    {
      key: "features.code_mode.direct_only_tool_namespaces",
      type: "array<string>",
      description:
        "工具命名空间代码模式只能通过直接工具调用来使用。",
    },
    {
      key: "features.context_management.experimental_mode",
      type: "boolean",
      description:
        "启用实验性上下文管理（默认情况下关闭）。它不是重复将上下文压缩为单个摘要，而是使用注释和可搜索历史记录来保留累积的详细信息。需要在 Plus、Pro 或 Pro Lite 上登录 ChatGPT。",
    },
    {
      key: "features.rollout_budget.enabled",
      type: "boolean",
      description:
        "启用部署预算跟踪。此功能正在开发中，默认情况下处于关闭状态。启用后，需要 `features.rollout_budget.limit_tokens`。",
    },
    {
      key: "features.rollout_budget.limit_tokens",
      type: "integer",
      description:
        "推出预算跟踪的正Token限制。启用部署预算时需要。",
    },
    {
      key: "features.rollout_budget.reminder_interval_tokens",
      type: "integer",
      description:
        "推出预算提醒之间的正令牌间隔。默认为 `limit_tokens` 的 10%，最少 1 个Token。",
    },
    {
      key: "features.rollout_budget.sampling_token_weight",
      type: "number",
      description:
        "推出预算会计中采样令牌的有限非负乘数。默认为 `1.0`。",
    },
    {
      key: "features.rollout_budget.prefill_token_weight",
      type: "number",
      description:
        "推出预算会计中预填充Token的有限非负乘数。默认为 `1.0`。",
    },
    {
      key: "hooks",
      type: "table",
      description:
        "在 `config.toml` 中内联配置的生命周期挂钩。使用与 `hooks.json` 相同的事件模式；有关示例和支持的事件，请参阅 Hooks 指南。",
    },
    {
      key: "hooks.<Event>",
      type: "array<table>",
      description:
        "用于挂钩事件的匹配器组，例如 `PreToolUse`、`PermissionRequest`、`PostToolUse`、`PreCompact`、`PostCompact`、`SessionStart`、`SessionEnd`、`SubagentStart`、`SubagentStop`、 `UserPromptSubmit`、`Stop` 或 `Interrupt`。",
    },
    {
      key: "hooks.<Event>[].hooks",
      type: "array<table>",
      description:
        "匹配器组的挂钩处理程序。支持命令和 MCP 工具挂钩，同时解析但跳过提示和智能体挂钩处理程序。",
    },
    {
      key: "hooks.<Event>[].hooks[].async",
      type: "boolean",
      description:
        "在后台运行命令挂钩，不会延迟触发操作。默认为`false`； `SessionEnd` 始终同步运行。参见 [在后台运行钩子](../hooks.zh-CN.md#run-hooks-in-the-background)。",
    },
    {
      key: "hooks.<Event>[].hooks[].additionalContextLimit",
      type: "integer",
      description:
        "将超大 `additionalContext` 保存到磁盘并向模型显示较短预览的每个处理程序令牌的近似阈值。默认为`2500`； `0` 将完整上下文直接传递给模型。参见 [大钩输出](../hooks.zh-CN.md#large-hook-output)。",
    },
    {
      key: "hooks.<Event>[].hooks[].commandWindows",
      type: "string",
      description:
        "命令挂钩的仅限 Windows 命令覆盖。 TOML 别名 `command_windows` 也被接受。",
    },
    {
      key: "features.memories",
      type: "boolean",
      description:
        "启用 [回忆](../customization/memories.zh-CN.md)（默认关闭）。",
    },
    {
      key: "mcp_optional_startup_grace_ms",
      type: "integer (milliseconds)",
      description:
        "构建初始工具目录时共享等待可选的 MCP 服务器。默认为 `1000`。设置为 `0` 以等待每个服务器的 `startup_timeout_sec`。",
    },
    {
      key: "mcp_servers.<id>.command",
      type: "string",
      description: "MCP stdio 服务器的启动器命令。",
    },
    {
      key: "mcp_servers.<id>.args",
      type: "array<string>",
      description: "传递给 MCP stdio 服务器命令的参数。",
    },
    {
      key: "mcp_servers.<id>.env",
      type: "map<string,string>",
      description: "环境变量转发到 MCP stdio 服务器。",
    },
    {
      key: "mcp_servers.<id>.env_vars",
      type: 'array<string | { name = string, source = "local" | "remote" }>',
      description:
        '将 MCP stdio 服务器列入白名单的其他环境变量。字符串条目默认为 `source = "local"`；仅将 `source = "remote"` 与执行程序支持的远程 stdio 一起使用。',
    },
    {
      key: "mcp_servers.<id>.cwd",
      type: "string",
      description: "MCP stdio 服务器进程的工作目录。",
    },
    {
      key: "mcp_servers.<id>.url",
      type: "string",
      description: "MCP 可流式 HTTP 服务器的端点。",
    },
    {
      key: "mcp_servers.<id>.auth",
      type: "oauth | chatgpt",
      description:
        "配置承载令牌和授权标头后，MCP HTTP 服务器的身份验证回退。 `oauth`（默认）使用存储的 MCP OAuth 凭据（如果可用）。 `chatgpt` 将当前 ChatGPT 会话用于受信任的第一方 ChatGPT 源，然后回退到存储的 OAuth。如果没有解析凭据源，两种模式都可以在无需身份验证的情况下进行连接。",
    },
    {
      key: "mcp_servers.<id>.oauth.client_id",
      type: "string",
      description:
        "预注册的 OAuth 客户端 ID，用于与此 MCP 服务器进行授权和令牌交换。",
    },
    {
      key: "mcp_servers.<id>.oauth.callback_url",
      type: "string",
      description:
        "服务器特定的 OAuth 回调。当支持颁发者标识或 URL 已以服务器特定的回调 ID 结尾时，预注册的客户端会重用它。否则，Codex 使用附加该 ID 的全局或默认回调。没有预先注册 ID 的客户端在客户端注册期间使用此回调。",
    },
    {
      key: "mcp_servers.<id>.oauth.callback_port",
      type: "integer",
      description:
        "修复了此 MCP 服务器的 OAuth 回调侦听器端口。覆盖 `mcp_oauth_callback_port`。对于具有显式 URL 端口的直接环回回调，请配置相同的侦听器端口。",
    },
    {
      key: "mcp_servers.<id>.bearer_token_env_var",
      type: "string",
      description:
        "为 MCP HTTP 服务器获取不记名令牌的环境变量。",
    },
    {
      key: "mcp_servers.<id>.http_headers",
      type: "map<string,string>",
      description: "每个 MCP HTTP 请求中包含静态 HTTP 标头。",
    },
    {
      key: "mcp_servers.<id>.http_headers_helper",
      type: "string (command)",
      description:
        "打印 HTTP 标头名称和值的 JSON 对象的本地命令。仅支持本地连接的 HTTP MCP 服务器。显式承载令牌和 OAuth 凭据优先于帮助程序提供的授权标头。",
    },
    {
      key: "mcp_servers.<id>.env_http_headers",
      type: "map<string,string>",
      description:
        "从 MCP HTTP 服务器的环境变量填充的 HTTP 标头。",
    },
    {
      key: "mcp_servers.<id>.enabled",
      type: "boolean",
      description: "禁用 MCP 服务器而不删除其配置。",
    },
    {
      key: "mcp_servers.<id>.required",
      type: "boolean",
      description:
        "当为 true 时，如果此启用的 MCP 服务器无法初始化，则启动/恢复失败。",
    },
    {
      key: "mcp_servers.<id>.startup_timeout_sec",
      type: "number",
      description:
        "覆盖 MCP 服务器的默认 10 秒启动超时。",
    },
    {
      key: "mcp_servers.<id>.startup_timeout_ms",
      type: "number",
      description: "`startup_timeout_sec` 的别名（以毫秒为单位）。",
    },
    {
      key: "mcp_servers.<id>.tool_timeout_sec",
      type: "number",
      description:
        "覆盖 MCP 服务器默认的每个工具 60 秒超时。",
    },
    {
      key: "mcp_servers.<id>.enabled_tools",
      type: "array<string>",
      description: "MCP 服务器公开的工具名称的允许列表。",
    },
    {
      key: "mcp_servers.<id>.disabled_tools",
      type: "array<string>",
      description:
        "在 `enabled_tools` 之后为 MCP 服务器应用拒绝列表。",
    },
    {
      key: "mcp_servers.<id>.default_tools_approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "除非存在每个工具覆盖，否则此服务器上 MCP 工具的默认批准行为。",
    },
    {
      key: "mcp_servers.<id>.tools.<tool>.approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "此服务器上一个 MCP 工具的每工具审批行为覆盖。",
    },
    {
      key: "mcp_servers.<id>.tools.<tool>.output_token_limit",
      type: "integer (positive)",
      description:
        "一种 MCP 工具输出的Token预算（在标准 20% 序列化津贴之前）。覆盖该工具的模型默认输出截断预算。",
    },
    {
      key: "mcp_servers.<id>.scopes",
      type: "array<string>",
      description:
        "向 MCP 服务器进行身份验证时请求的 OAuth 范围。",
    },
    {
      key: "mcp_servers.<id>.oauth_resource",
      type: "string",
      description:
        "MCP 登录期间要包含的可选 RFC 8707 OAuth 资源参数。",
    },
    {
      key: "mcp_servers.<id>.experimental_environment",
      type: "local | remote",
      description:
        "MCP 服务器的实验放置。 `remote`通过远程执行器环境启动stdio服务器；未实现可流传输的 HTTP 远程放置。",
    },
    {
      key: "agents",
      type: "table",
      description:
        "多智能体设置和自定义角色声明。标量设置名称是保留的，不能用作自定义角色名称。",
    },
    {
      key: "agents.enabled",
      type: "boolean",
      description: "启用或禁用多智能体工具（默认值：true）。",
    },
    {
      key: "agents.max_concurrent_threads_per_session",
      type: "number",
      description:
        "可以同时打开的生成智能体线程的最大数量（不包括主线程）。未设置时，Codex 选择默认值。",
    },
    {
      key: "agents.max_threads",
      type: "number",
      description:
        "`agents.max_concurrent_threads_per_session` 的旧别名。",
    },
    {
      key: "agents.default_subagent_model",
      type: "string",
      description:
        "生成智能体的默认模型。显式生成模型优先。",
    },
    {
      key: "agents.default_subagent_reasoning_effort",
      type: "string",
      description:
        "生成智能体的默认推理工作。明确的生成努力优先。",
    },
    {
      key: "agents.interrupt_message",
      type: "boolean",
      description:
        "当智能体轮流中断时记录模型可见的消息（默认值：true）。",
    },
    {
      key: "agents.<name>.description",
      type: "string",
      description:
        "选择和生成该智能体类型时向 Codex 显示角色指南。",
    },
    {
      key: "agents.<name>.config_file",
      type: "string (path)",
      description:
        "该角色的 TOML 配置层的路径；相对路径从声明角色的配置文件解析。",
    },
    {
      key: "memories.generate_memories",
      type: "boolean",
      description:
        "当 `false` 时，新创建的线程不会存储为内存生成输入。默认为 `true`。",
    },
    {
      key: "memories.use_memories",
      type: "boolean",
      description:
        "当 `false` 时，Codex 会跳过将现有内存注入到未来会话中。默认为 `true`。",
    },
    {
      key: "memories.disable_on_external_context",
      type: "boolean",
      description:
        "当 `true` 时，使用外部上下文（例如 MCP 工具调用、Web 搜索或工具搜索）的线程将不参与内存生成。默认为 `false`。旧别名：`memories.no_memories_if_mcp_or_web_search`。",
    },
    {
      key: "memories.max_raw_memories_for_consolidation",
      type: "number",
      description:
        "保留最大程度的近期原始记忆以进行全球整合。默认为 `256`，上限为 `4096`。",
    },
    {
      key: "memories.max_unused_days",
      type: "number",
      description:
        "自上次使用内存到不符合合并条件之前的最长天数。默认为 `30`，并固定为 `0`-`365`。",
    },
    {
      key: "memories.max_rollout_age_days",
      type: "number",
      description:
        "考虑用于内存生成的线程的最大年龄。默认为 `30`，并固定为 `0`-`90`。",
    },
    {
      key: "memories.max_rollouts_per_startup",
      type: "number",
      description:
        "每个启动阶段处理的最大推出候选数。默认为 `16`，上限为 `128`。",
    },
    {
      key: "memories.min_rollout_idle_hours",
      type: "number",
      description:
        "线程被考虑用于内存生成之前的最小空闲时间。默认为 `6`，并固定为 `1`-`48`。",
    },
    {
      key: "memories.min_rate_limit_remaining_percent",
      type: "number",
      description:
        "在内存生成开始之前，Codex 速率限制窗口中所需的最小剩余百分比。默认为 `25`，并固定为 `0`-`100`。",
    },
    {
      key: "memories.extract_model",
      type: "string",
      description: "用于每线程内存提取的可选模型覆盖。",
    },
    {
      key: "memories.consolidation_model",
      type: "string",
      description: "用于全局内存整合的可选模型覆盖。",
    },
    {
      key: "features.unified_exec",
      type: "boolean",
      description:
        "使用统一的 PTY 支持的执行工具（稳定；默认启用，Windows 除外）。",
    },
    {
      key: "features.shell_snapshot",
      type: "boolean",
      description:
        "快照 shell 环境可加速重复命令（稳定；默认开启）。",
    },
    {
      key: "features.multi_agent",
      type: "boolean",
      description:
        "启用多智能体协作工具（`spawn_agent`、`send_input`、`resume_agent`、`wait_agent` 和 `close_agent`）（稳定；默认启用）。",
    },
    {
      key: "features.goals",
      type: "boolean",
      description:
        "启用持久目标和自动延续（稳定；默认启用）。",
    },
    {
      key: "features.remote_plugin",
      type: "boolean",
      description: "启用远程插件目录（稳定；默认启用）。",
    },
    {
      key: "features.personality",
      type: "boolean",
      description:
        "启用个性选择控件（稳定；默认打开）。",
    },
    {
      key: "features.network_proxy",
      type: "boolean | table",
      description:
        "启动沙箱命令的网络代理（实验性的；默认情况下关闭）。需要强制执行权限配置文件域规则，除非启用管理员管理的 `experimental_network` 要求启动代理。设置功能级策略选项（例如 `domains`）时使用表。不过滤网络搜索、应用程序、MCP 或其他托管工具。",
    },
    {
      key: "features.network_proxy.enabled",
      type: "boolean",
      description:
        "启用命令网络访问时启动沙箱命令网络代理。默认为`false`；代理关闭时，不会强制执行权限配置文件域规则。",
    },
    {
      key: "features.network_proxy.domains",
      type: "map<string, allow | deny>",
      description:
        "沙箱网络的域策略。默认情况下未设置，这意味着在添加 `allow` 规则之前不允许任何外部目标。支持精确主机、仅适用于子域的 `*.example.com`、适用于顶级子域的 `**.example.com` 以及全局 `*` 允许规则；更喜欢范围规则，因为 `*` 广泛开放公共出站访问。为被阻止的目的地添加 `deny` 规则； `deny` 在冲突中获胜。",
    },
    {
      key: "features.network_proxy.unix_sockets",
      type: "map<string, allow | deny>",
      description:
        "用于沙箱网络的 Unix 套接字策略。默认取消设置；为允许的套接字添加 `allow` 条目。",
    },
    {
      key: "features.network_proxy.allow_local_binding",
      type: "boolean",
      description:
        "允许更广泛的本地/专用网络访问。默认为`false`；精确的本地 IP 文字或 `localhost` 允许规则仍然可以允许特定的本地目标。",
    },
    {
      key: "features.network_proxy.enable_socks5",
      type: "boolean",
      description: "公开 SOCKS5 支持。默认为 `true`。",
    },
    {
      key: "features.network_proxy.enable_socks5_udp",
      type: "boolean",
      description: "允许 UDP 通过 SOCKS5。默认为 `true`。",
    },
    {
      key: "features.network_proxy.allow_upstream_proxy",
      type: "boolean",
      description:
        "允许通过环境中的上游代理进行链接。默认为 `true`。",
    },
    {
      key: "features.network_proxy.dangerously_allow_non_loopback_proxy",
      type: "boolean",
      description:
        "允许非环回侦听器地址。默认为`false`；启用它可以公开本地主机之外的代理侦听器。",
    },
    {
      key: "features.network_proxy.dangerously_allow_all_unix_sockets",
      type: "boolean",
      description:
        "允许任意 Unix 套接字目标，而不是仅允许访问。默认为`false`；仅在严格控制的环境中使用。",
    },
    {
      key: "features.network_proxy.proxy_url",
      type: "string",
      description:
        '沙箱网络的 HTTP 侦听器 URL。默认为 `"http://127.0.0.1:3128"`。',
    },
    {
      key: "features.network_proxy.socks_url",
      type: "string",
      description:
        'SOCKS5 侦听器 URL。默认为 `"http://127.0.0.1:8081"`。',
    },
    {
      key: "features.web_search",
      type: "boolean",
      description:
        "已弃用的旧版切换；更喜欢顶级 `web_search` 设置。",
    },
    {
      key: "features.web_search_cached",
      type: "boolean",
      description:
        '已弃用的旧版切换。当 `web_search` 未设置时，true 映射到 `web_search = "cached"`。',
    },
    {
      key: "features.web_search_request",
      type: "boolean",
      description:
        '已弃用的旧版切换。当 `web_search` 未设置时，true 映射到 `web_search = "live"`。',
    },
    {
      key: "features.shell_tool",
      type: "boolean",
      description:
        "启用默认的 `shell` 工具来运行命令（稳定；默认启用）。",
    },
    {
      key: "features.enable_request_compression",
      type: "boolean",
      description:
        "在支持时使用 zstd 压缩流请求主体（稳定；默认情况下打开）。",
    },
    {
      key: "features.skill_mcp_dependency_install",
      type: "boolean",
      description:
        "允许提示并安装缺少的 MCP 技能依赖项（稳定；默认开启）。",
    },
    {
      key: "features.fast_mode",
      type: "boolean",
      description:
        "在 TUI 中启用模型目录服务层选择，包括活动模型公布快速层命令时的命令（稳定；默认启用）。",
    },
    {
      key: "features.prevent_idle_sleep",
      type: "boolean",
      description:
        "防止机器在轮流主动运行时休眠（实验性；默认关闭）。",
    },
    {
      key: "suppress_unstable_features_warning",
      type: "boolean",
      description:
        "抑制启用正在开发的功能标志时出现的警告。",
    },
    {
      key: "model_providers.<id>",
      type: "table",
      description:
        "自定义提供者定义。内置提供商 ID（`openai`、`ollama` 和 `lmstudio`）已保留且无法覆盖。",
    },
    {
      key: "model_providers.<id>.name",
      type: "string",
      description: "自定义模型提供者的显示名称。",
    },
    {
      key: "model_providers.<id>.base_url",
      type: "string",
      description: "模型提供者的 API 基本 URL。",
    },
    {
      key: "model_providers.<id>.env_key",
      type: "string",
      description: "提供提供商 API 密钥的环境变量。",
    },
    {
      key: "model_providers.<id>.env_key_instructions",
      type: "string",
      description: "提供商 API 密钥的可选设置指南。",
    },
    {
      key: "model_providers.<id>.experimental_bearer_token",
      type: "string",
      description:
        "提供商的直接不记名令牌（不鼓励；使用 `env_key`）。",
    },
    {
      key: "model_providers.<id>.requires_openai_auth",
      type: "boolean",
      description:
        "提供商使用 OpenAI 身份验证（默认为 false）。",
    },
    {
      key: "model_providers.<id>.wire_api",
      type: "responses",
      description:
        "提供商使用的协议。 `responses` 是唯一受支持的值，省略时为默认值。",
    },
    {
      key: "model_providers.<id>.query_params",
      type: "map<string,string>",
      description: "附加到提供者请求的额外查询参数。",
    },
    {
      key: "model_providers.<id>.http_headers",
      type: "map<string,string>",
      description: "添加到提供者请求的静态 HTTP 标头。",
    },
    {
      key: "model_providers.<id>.env_http_headers",
      type: "map<string,string>",
      description:
        "HTTP 标头由环境变量（如果存在）填充。",
    },
    {
      key: "model_providers.<id>.request_max_retries",
      type: "number",
      description:
        "向提供商发送 HTTP 请求的重试次数（默认值：4）。",
    },
    {
      key: "model_providers.<id>.stream_max_retries",
      type: "number",
      description: "SSE 流中断的重试计数（默认值：5）。",
    },
    {
      key: "model_providers.<id>.stream_idle_timeout_ms",
      type: "number",
      description:
        "SSE 流的空闲超时以毫秒为单位（默认值：300000）。",
    },
    {
      key: "model_providers.<id>.supports_websockets",
      type: "boolean",
      description:
        "该提供程序是否支持 Responses API WebSocket 传输。",
    },
    {
      key: "model_providers.<id>.supports_standalone_web_search",
      type: "boolean",
      description:
        "宣传对兼容的独立 Web 搜索端点的支持（默认值： false）。独立搜索仍在开发中，默认情况下处于关闭状态；仅靠提供商兼容性并不能实现这一点。",
    },
    {
      key: "model_providers.<id>.auth",
      type: "table",
      description:
        "自定义提供程序的命令支持的不记名令牌配置。请勿与 `env_key`、`experimental_bearer_token` 或 `requires_openai_auth` 组合使用。",
    },
    {
      key: "model_providers.<id>.auth.command",
      type: "string",
      description:
        "当 Codex 需要不记名令牌时运行的命令。该命令必须将令牌打印到标准输出。",
    },
    {
      key: "model_providers.<id>.auth.args",
      type: "array<string>",
      description: "传递给令牌命令的参数。",
    },
    {
      key: "model_providers.<id>.auth.timeout_ms",
      type: "number",
      description:
        "最大令牌命令运行时间（以毫秒为单位）（默认值：5000）。",
    },
    {
      key: "model_providers.<id>.auth.refresh_interval_ms",
      type: "number",
      description:
        "Codex 主动刷新令牌的频率（以毫秒为单位）（默认值：300000）。设置为 `0` 仅在身份验证重试后刷新。",
    },
    {
      key: "model_providers.<id>.auth.cwd",
      type: "string (path)",
      description: "令牌命令的工作目录。",
    },
    {
      key: "model_providers.amazon-bedrock.aws.profile",
      type: "string",
      description:
        "内置 `amazon-bedrock` 提供商使用的 AWS 配置文件名称。",
    },
    {
      key: "model_providers.amazon-bedrock.aws.region",
      type: "string",
      description: "内置 `amazon-bedrock` 提供商使用的 AWS 区域。",
    },
    {
      key: "model_reasoning_effort",
      type: "minimal | low | medium | high | xhigh",
      description:
        "调整支持模型的推理工作（仅限响应 API；`xhigh` 取决于模型）。",
    },
    {
      key: "plan_mode_reasoning_effort",
      type: "none | minimal | low | medium | high | xhigh",
      description:
        "计划模式特定的推理覆盖。取消设置时，计划模式将使用其内置预设默认值。",
    },
    {
      key: "model_reasoning_summary",
      type: "auto | concise | detailed | none",
      description:
        "选择推理摘要详细信息或完全禁用摘要。",
    },
    {
      key: "model_verbosity",
      type: "low | medium | high",
      description:
        "可选的 GPT-5 响应 API 详细程度覆盖；取消设置时，将使用所选模型/预设默认值。",
    },
    {
      key: "model_supports_reasoning_summaries",
      type: "boolean",
      description: "强制 Codex 发送或不发送推理元数据。",
    },
    {
      key: "shell_environment_policy.inherit",
      type: "all | core | none",
      description:
        "生成子进程时的基线环境继承。",
    },
    {
      key: "shell_environment_policy.ignore_default_excludes",
      type: "boolean",
      description:
        "在其他过滤器运行之前保留包含 KEY、SECRET 或 TOKEN 的变量（默认值：true）。设置为 false 以应用自动秘密名称排除。",
    },
    {
      key: "shell_environment_policy.filters",
      type: "map<string, include | exclude>",
      description:
        "规范的不区分大小写的环境变量模式过滤器。包含条目会创建允许列表，并且无法恢复排除的值。显式 `set` 值在排除后应用。请勿将滤波器与旧版 `exclude` 或 `include_only` 阵列组合在同一层中。",
    },
    {
      key: "shell_environment_policy.exclude",
      type: "array<string>",
      description:
        "旧版环境变量排除模式。使用`shell_environment_policy.filters`进行新配置；不要将两种形式组合在同一层中。",
    },
    {
      key: "shell_environment_policy.include_only",
      type: "array<string>",
      description:
        "环境变量模式的旧许可名单。使用`shell_environment_policy.filters`进行新配置；不要将两种形式组合在同一层中。",
    },
    {
      key: "shell_environment_policy.set",
      type: "map<string,string>",
      description:
        "排除后注入显式环境值；包含过滤器仍然可以删除它们。",
    },
    {
      key: "shell_environment_policy.experimental_use_profile",
      type: "boolean",
      description: "生成子进程时使用用户 shell 配置文件。",
    },
    {
      key: "project_root_markers",
      type: "array<string>",
      description:
        "项目根标记文件名列表；在搜索项目根目录的父目录时使用。",
    },
    {
      key: "project_doc_max_bytes",
      type: "number",
      description:
        "构建项目指令时从 `AGENTS.md` 读取的最大字节数。",
    },
    {
      key: "project_doc_fallback_filenames",
      type: "array<string>",
      description: "当 `AGENTS.md` 丢失时要尝试的其他文件名。",
    },
    {
      key: "history.persistence",
      type: "save-all | none",
      description:
        "控制Codex是否将会话记录保存到history.jsonl。",
    },
    {
      key: "tool_output_token_limit",
      type: "number",
      description:
        "用于在历史中存储单个工具/功能输出的Token预算。",
    },
    {
      key: "background_terminal_max_timeout",
      type: "number",
      description:
        "空 `write_stdin` 轮询（后台终端轮询）的最大轮询窗口（以毫秒为单位）。默认值：`300000`（5 分钟）。替换旧的 `background_terminal_timeout` 密钥。",
    },
    {
      key: "history.max_bytes",
      type: "number",
      description:
        "如果设置，则通过删除最旧的条目来限制历史文件大小（以字节为单位）。",
    },
    {
      key: "file_opener",
      type: "vscode | vscode-insiders | windsurf | cursor | none",
      description:
        "用于打开 Codex 输出引文的 URI 方案（默认值：`vscode`）。",
    },
    {
      key: "otel.environment",
      type: "string",
      description:
        "应用于发出的 OpenTelemetry 事件的环境标记（默认值：`dev`）。",
    },
    {
      key: "otel.exporter",
      type: "none | otlp-http | otlp-grpc",
      description:
        "选择 OpenTelemetry 导出器并提供任何端点元数据。",
    },
    {
      key: "otel.trace_exporter",
      type: "none | otlp-http | otlp-grpc",
      description:
        "选择 OpenTelemetry 跟踪导出器并提供任何端点元数据。",
    },
    {
      key: "otel.metrics_exporter",
      type: "none | statsig | otlp-http | otlp-grpc",
      description:
        "选择 OpenTelemetry 指标导出器（默认为 `statsig`）。",
    },
    {
      key: "otel.log_user_prompt",
      type: "boolean",
      description:
        "选择使用 OpenTelemetry 日志导出原始用户提示。",
    },
    {
      key: "otel.exporter.<id>.endpoint",
      type: "string",
      description: "OTEL 日志的导出器端点。",
    },
    {
      key: "otel.exporter.<id>.protocol",
      type: "binary | json",
      description: "OTLP/HTTP 导出器使用的协议。",
    },
    {
      key: "otel.exporter.<id>.headers",
      type: "map<string,string>",
      description: "OTEL 导出器请求中包含静态标头。",
    },
    {
      key: "otel.trace_exporter.<id>.endpoint",
      type: "string",
      description: "跟踪 OTEL 日志的导出器端点。",
    },
    {
      key: "otel.trace_exporter.<id>.protocol",
      type: "binary | json",
      description: "OTLP/HTTP 跟踪导出器使用的协议。",
    },
    {
      key: "otel.trace_exporter.<id>.headers",
      type: "map<string,string>",
      description: "OTEL 跟踪导出器请求中包含静态标头。",
    },
    {
      key: "otel.exporter.<id>.tls.ca-certificate",
      type: "string",
      description: "OTEL 导出器 TLS 的 CA 证书路径。",
    },
    {
      key: "otel.exporter.<id>.tls.client-certificate",
      type: "string",
      description: "OTEL 导出器 TLS 的客户端证书路径。",
    },
    {
      key: "otel.exporter.<id>.tls.client-private-key",
      type: "string",
      description: "OTEL 导出器 TLS 的客户端私钥路径。",
    },
    {
      key: "otel.trace_exporter.<id>.tls.ca-certificate",
      type: "string",
      description: "OTEL 跟踪导出器 TLS 的 CA 证书路径。",
    },
    {
      key: "otel.trace_exporter.<id>.tls.client-certificate",
      type: "string",
      description: "OTEL 跟踪导出器 TLS 的客户端证书路径。",
    },
    {
      key: "otel.trace_exporter.<id>.tls.client-private-key",
      type: "string",
      description: "OTEL 跟踪导出器 TLS 的客户端私钥路径。",
    },
    {
      key: "desktop.custom_file_handlers.<id>",
      type: "table",
      description:
        "仅限用户级别。为 ChatGPT 桌面应用程序定义附加 **打开于** 目标。有关示例和处理程序 ID 限制，请参阅 [添加自定义文件处理程序](config-advanced.zh-CN.md#add-custom-file-handlers)。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.label",
      type: "string",
      description: "**打开于** 菜单中显示的显示名称。必需的。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.icon",
      type: "string",
      description:
        "捆绑的资源路径、Base64 编码的 `data:image/...` URL、文件 URI 或处理程序图标的绝对本地路径。必需的;不受支持的源使用默认的 VS Code 图标。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.command",
      type: "string",
      description:
        "要检测和启动的可执行路径或命令名称。必需的。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.args",
      type: "array<string>",
      description:
        "在命令和文件输入之间插入的参数（默认值：`[]`）。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.input",
      type: "path | json_argument | json_stdin",
      description:
        "应用程序如何将文件输入发送到处理程序（默认值：`path`）。",
    },
    {
      key: "desktop.custom_file_handlers.<id>.supports_ssh",
      type: "boolean",
      description:
        "为 SSH 工作区中的文件提供处理程序（默认值：`false`）。",
    },
    {
      key: "tui",
      type: "table",
      description:
        "TUI 特定选项，例如启用内联桌面通知。",
    },
    {
      key: "tui.notifications",
      type: "boolean | array<string>",
      description:
        "启用 TUI 通知；可选择限制特定事件类型。",
    },
    {
      key: "tui.notification_method",
      type: "auto | osc9 | bel",
      description:
        "终端通知的通知方式（默认：自动）。",
    },
    {
      key: "tui.notification_condition",
      type: "unfocused | always",
      description:
        "控制 TUI 通知是否仅在终端未聚焦时或无论焦点如何时触发。默认为 `unfocused`。",
    },
    {
      key: "tui.animations",
      type: "boolean",
      description:
        "启用终端动画（欢迎屏幕、闪烁、旋转）（默认值：true）。",
    },
    {
      key: "tui.alternate_screen",
      type: "auto | always | never",
      description:
        "控制 TUI 的备用屏幕使用（默认值：自动；自动在 Zellij 中跳过它以保留回滚）。",
    },
    {
      key: "tui.resume_cwd",
      type: "current | session",
      description:
        "恢复或分叉会话时使用的工作目录。取消设置时，Codex 会要求您选择当前目录是否与会话的保存目录不同。",
    },
    {
      key: "tui.vim_mode_default",
      type: "boolean",
      description:
        "在 Vim 正常模式而不是插入模式下启动编辑器（默认值： false）。您仍然可以使用 `/vim` 在每个会话中切换它。",
    },
    {
      key: "tui.raw_output_mode",
      type: "boolean",
      description:
        "以原始回滚模式启动 TUI，以便进行复制友好的终端选择（默认值： false）。您可以使用 `/raw` 或默认的 `alt-r` 键绑定来切换它。",
    },
    {
      key: "tui.show_tooltips",
      type: "boolean",
      description:
        "在 TUI 欢迎屏幕中显示入门工具提示（默认值：true）。",
    },
    {
      key: "tui.status_line",
      type: "array<string> | null",
      description:
        "TUI 页脚状态行项目标识符的有序列表。 `null` 禁用状态行。",
    },
    {
      key: "tui.terminal_title",
      type: "array<string> | null",
      description:
        '终端窗口/选项卡标题项标识符的有序列表。默认为`["spinner", "project"]`； `null` 禁用标题更新。',
    },
    {
      key: "tui.theme",
      type: "string",
      description:
        "语法突出显示主题覆盖（短横线主题名称）。",
    },
    {
      key: "tui.keymap.<context>.<action>",
      type: "string | array<string>",
      description:
        "TUI 操作的键盘快捷键绑定。支持的上下文包括 `global`、`chat`、`composer`、`editor`、`vim_normal`、`vim_operator`、`vim_text_object`、`pager`、`list` 和 `approval`。选定的输入框动作回落到匹配的 `tui.keymap.global` 绑定；如果支持，则上下文特定的绑定优先。",
    },
    {
      key: "tui.keymap.<context>.<action> = []",
      type: "empty array",
      description:
        "取消绑定该键盘映射上下文中的操作。键名称使用规范化字符串，例如 `ctrl-a`、`shift-enter`、`page-down` 或 `minus`。",
    },
    {
      key: "marketplaces.<name>.source_type",
      type: "git | local",
      description:
        "配置的插件市场的源类型。市场可以在系统、云管理、用户或可信项目 config.toml 中定义。",
    },
    {
      key: "marketplaces.<name>.source",
      type: "string",
      description:
        "Git 仓库位置或本地市场根目录。对本地源使用绝对路径；该目录包含 .agents/plugins/marketplace.json。",
    },
    {
      key: "marketplaces.<name>.ref",
      type: "string",
      description: "市场的可选 Git 分支、标签或提交。",
    },
    {
      key: "marketplaces.<name>.sparse_paths",
      type: "array<string>",
      description:
        "Git 市场的可选稀疏结帐路径。包括市场目录及其引用的任何本地插件目录。",
    },
    {
      key: "plugins.<plugin>.enabled",
      type: "boolean",
      description:
        "使用 `plugin-name@marketplace-name` 密钥启用或禁用本地市场插件。读取有效的合并配置；受信任项目设置可以覆盖用户、云管理和系统默认设置。即使禁用后，市场刷新也可以安装或刷新配置的插件。这不会覆盖工作区管理的启用状态。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.enabled",
      type: "boolean",
      description:
        "启用或禁用已安装插件捆绑的 MCP 服务器，而无需更改插件清单。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.default_tools_approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "提供插件的 MCP 服务器上工具的默认批准行为。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.enabled_tools",
      type: "array<string>",
      description:
        "允许从插件提供的 MCP 服务器公开的工具列表。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.disabled_tools",
      type: "array<string>",
      description:
        "在 `enabled_tools` 之后为提供插件的 MCP 服务器应用拒绝列表。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.tools.<tool>.approval_mode",
      type: "auto | prompt | writes | approve",
      description:
        "针对插件提供的 MCP 工具的每个工具批准行为覆盖。",
    },
    {
      key: "tui.model_availability_nux.<model>",
      type: "integer",
      description: "由模型 slug 键入的内部启动工具提示状态。",
    },
    {
      key: "hide_agent_reasoning",
      type: "boolean",
      description:
        "抑制 TUI 和 `codex exec` 输出中的推理事件。",
    },
    {
      key: "show_raw_agent_reasoning",
      type: "boolean",
      description:
        "当活动模型发出原始推理内容时，将其呈现出来。",
    },
    {
      key: "disable_paste_burst",
      type: "boolean",
      description: "在 TUI 中禁用突发粘贴检测。",
    },
    {
      key: "windows_wsl_setup_acknowledged",
      type: "boolean",
      description: "跟踪 Windows 登录确认（仅限 Windows）。",
    },
    {
      key: "chatgpt_base_url",
      type: "string",
      description: "覆盖 ChatGPT 登录流程中使用的基本 URL。",
    },
    {
      key: "cli_auth_credentials_store",
      type: "file | keyring | auto | ephemeral",
      description: "控制 CLI 存储缓存凭据的位置。",
    },
    {
      key: "mcp_oauth_credentials_store",
      type: "auto | file | keyring",
      description: "MCP OAuth 凭据的首选存储。",
    },
    {
      key: "mcp_oauth_callback_port",
      type: "integer",
      description:
        "MCP OAuth 登录期间使用的本地 HTTP 回调服务器的可选全局固定端口。服务器特定的 `oauth.callback_port` 优先。当两者均未设置时，Codex 绑定到操作系统选择的临时端口。",
    },
    {
      key: "mcp_oauth_callback_url",
      type: "string",
      description:
        "MCP OAuth 登录的可选基本回调 URL，例如 devbox 入口 URL。当授权服务器支持发行者识别时，新添加的预注册客户端不改变使用该URL；没有保存回调的现有客户端会附加服务器特定的回调 ID。如果没有发行者支持，任何配置的回调缺少所需 ID 的预注册 MCP 服务器都会回退到附加了 ID 的此 URL。回调 URL 端口不选择侦听器端口。",
    },
    {
      key: "experimental_use_unified_exec_tool",
      type: "boolean",
      description:
        "用于启用统一执行的旧名称；更喜欢 `[features].unified_exec` 或 `codex --enable unified_exec`。",
    },
    {
      key: "tools.web_search",
      type: 'boolean | { context_size = "low|medium|high", allowed_domains = [string], location = { country, region, city, timezone } }',
      description:
        "可选的网络搜索工具配置。对象表单可以设置搜索上下文大小、允许的搜索域和大致用户位置。这些搜索域过滤器与沙箱命令网络域规则分开，并且不限制连接器或 MCP 服务器。",
    },
    {
      key: "tools.view_image",
      type: "boolean",
      description: "启用本地图像附件工具`view_image`。",
    },
    {
      key: "web_search",
      type: "disabled | cached | indexed | live",
      description:
        'Web 搜索模式（默认：`"cached"`；缓存使用 OpenAI 维护的索引，无需外部 Web 访问；索引仅在搜索索引门控时允许外部访问；如果您使用 `--yolo` 或其他完全访问沙箱设置，则默认为 `"live"`）。使用 `"live"` 进行不受限制的实时检索，或使用 `"disabled"` 删除该工具。',
    },
    {
      key: "default_permissions",
      type: "string",
      description:
        "应用于沙箱工具调用的默认权限配置文件的名称。内置有 `:read-only`、`:workspace` 和 `:danger-full-access`；自定义配置文件名称需要匹配 `[permissions.<name>]` 表。请勿与 `sandbox_mode` 或 `[sandbox_workspace_write]` 组合使用。",
    },
    {
      key: "permissions.<name>.description",
      type: "string",
      description:
        "此命名配置文件的人类可读的描述。配置文件不会通过 `extends` 继承其父级的描述。",
    },
    {
      key: "permissions.<name>.extends",
      type: "string",
      description:
        "在此命名配置文件之前应用可选的父配置文件。将其设置为另一个命名配置文件，`:read-only`或`:workspace`； `:danger-full-access`、未定义的父项和循环被拒绝。",
    },
    {
      key: "permissions.<name>.workspace_roots",
      type: "table",
      description:
        "配置文件定义的工作区根，与会话的运行时工作区根一起接收 `:workspace_roots` 文件系统规则。",
    },
    {
      key: "permissions.<name>.workspace_roots.<path>",
      type: "boolean",
      description:
        "当 `true` 时，选择进入配置文件工作区根集的路径。禁用的条目保持不活动状态。",
    },
    {
      key: "permissions.<name>.filesystem",
      type: "table",
      description:
        "命名文件系统权限配置文件。每个键都是绝对路径或特殊标记，例如 `:minimal` 或 `:workspace_roots`。",
    },
    {
      key: "permissions.<name>.filesystem.glob_scan_max_depth",
      type: "number",
      description:
        "在沙箱启动前快照匹配的平台上扩展拒绝读取全局模式的最大深度。设置时必须至少为 `1`。",
    },
    {
      key: "permissions.<name>.filesystem.<path-or-glob>",
      type: '"read" | "write" | "deny" | table',
      description:
        '授予对该根下的路径、全局模式或特殊标记或范围嵌套条目的直接访问权限。使用 `"deny"` 拒绝读取匹配路径。',
    },
    {
      key: 'permissions.<name>.filesystem.":workspace_roots".<subpath-or-glob>',
      type: '"read" | "write" | "deny"',
      description:
        '相对于每个有效工作区根目录的范围内文件系统访问。使用 `"."` 作为根本身； glob 子路径（例如 `"**/*.env"`）可以拒绝使用 `"deny"` 进行读取。',
    },
    {
      key: "permissions.<name>.network.enabled",
      type: "boolean",
      description:
        "对此权限配置文件中的命令启用网络访问。这不会启动网络代理。如果没有 `features.network_proxy` 或启用管理员管理的网络要求，则命令网络访问是直接的，并且不会强制执行配置文件域规则。",
    },
    {
      key: "permissions.<name>.network.proxy_url",
      type: "string",
      description:
        "此权限配置文件启用沙箱网络时使用的 HTTP 侦听器 URL。",
    },
    {
      key: "permissions.<name>.network.enable_socks5",
      type: "boolean",
      description:
        "当此权限配置文件启用沙箱网络时，公开 SOCKS5 支持。",
    },
    {
      key: "permissions.<name>.network.socks_url",
      type: "string",
      description: "此权限配置文件使用的 SOCKS5 代理端点。",
    },
    {
      key: "permissions.<name>.network.enable_socks5_udp",
      type: "boolean",
      description: "启用后允许通过 SOCKS5 侦听器使用 UDP。",
    },
    {
      key: "permissions.<name>.network.allow_upstream_proxy",
      type: "boolean",
      description:
        "允许沙箱网络通过另一个上游代理进行链接。",
    },
    {
      key: "permissions.<name>.network.dangerously_allow_non_loopback_proxy",
      type: "boolean",
      description:
        "允许沙箱网络侦听器使用非环回绑定地址。启用它可以暴露本地主机之外的侦听器。",
    },
    {
      key: "permissions.<name>.network.dangerously_allow_all_unix_sockets",
      type: "boolean",
      description:
        "允许任意 Unix 套接字目标而不是默认的限制集。仅在严格控制的环境中使用。",
    },
    {
      key: "permissions.<name>.network.mode",
      type: "limited | full",
      description: "用于子进程流量的网络代理模式。",
    },
    {
      key: "permissions.<name>.network.domains",
      type: "table",
      description:
        "沙箱命令的域规则。仅当 `features.network_proxy` 或启用的管理员管理的网络要求激活代理时才强制执行。支持精确主机、`*.example.com`、`**.example.com`、全局`*`允许规则； `deny` 获胜。不限制 Web 搜索、应用程序或 MCP 服务器。",
    },
    {
      key: "permissions.<name>.network.domains.<pattern>",
      type: "allow | deny",
      description:
        "允许或拒绝确切的主机或范围通配符模式，例如 `*.example.com` 或 `**.example.com`。",
    },
    {
      key: "permissions.<name>.network.unix_sockets",
      type: "table",
      description:
        "沙箱网络的 Unix 套接字白名单覆盖。使用套接字路径作为键； `allow` 添加路径，`deny` 拒绝它。",
    },
    {
      key: "permissions.<name>.network.unix_sockets.<path>",
      type: "allow | deny",
      description:
        "使用 `allow` 将绝对 Unix 套接字路径添加到有效允许列表中，或使用 `deny` 拒绝它。被拒绝的条目将从有效允许列表中删除。",
    },
    {
      key: "permissions.<name>.network.allow_local_binding",
      type: "boolean",
      description:
        "通过沙箱网络允许更广泛的本地/专用网络访问。当其保持 `false` 时，精确的本地 IP 文字或 `localhost` 允许规则仍然可以允许特定的本地目标。",
    },
    {
      key: "projects.<path>.trust_level",
      type: "string",
      description:
        '将项目或工作树标记为可信或不可信 (`"trusted"` | `"untrusted"`)。不受信任的项目会跳过项目范围的 `.codex/` 层，包括项目本地配置、挂钩和规则。',
    },
    {
      key: "notice.hide_full_access_warning",
      type: "boolean",
      description: "跟踪完全访问警告提示的确认。",
    },
    {
      key: "notice.hide_world_writable_warning",
      type: "boolean",
      description:
        "跟踪 Windows 全局可写目录警告的确认。",
    },
    {
      key: "notice.hide_rate_limit_model_nudge",
      type: "boolean",
      description: "跟踪速率限制模型切换提醒的选择退出。",
    },
    {
      key: "notice.hide_gpt5_1_migration_prompt",
      type: "boolean",
      description: "跟踪 GPT-5.1 迁移提示的确认。",
    },
    {
      key: "notice.hide_gpt-5.1-codex-max_migration_prompt",
      type: "boolean",
      description:
        "跟踪 gpt-5.1-codex-max 迁移提示的确认。",
    },
    {
      key: "notice.model_migrations",
      type: "map<string,string>",
      description: "将已确认的模型迁移跟踪为旧->新映射。",
    },
    {
      key: "forced_login_method",
      type: "chatgpt | api",
      description: "将 Codex 限制为特定的身份验证方法。",
    },
    {
      key: "forced_chatgpt_workspace_id",
      type: "string (uuid)",
      description: "将 ChatGPT 登录限制为特定工作区标识符。",
    },
  ]}
  client:load
/>

您可以找到 `config.toml` [这里](https://learn.chatgpt.com/docs/config-schema.json) 的最新 JSON 架构。

要在 VS Code 或 Cursor 中编辑 `config.toml` 时获得自动完成和诊断功能，您可以安装 [更好的 TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) 扩展并将此行添加到 `config.toml` 的顶部：

```toml
#:schema https://developers.openai.com/codex/config-schema.json
```

注意：将 `experimental_instructions_file` 重命名为 `model_instructions_file`。 Codex 弃用旧密钥；将现有配置更新为新名称。

<a id="requirementstoml"></a>

## `requirements.toml`

`requirements.toml` 是管理员强制执行的配置文件，它限制用户无法覆盖的安全敏感设置。有关详细信息、位置和示例，请参阅 [管理员强制要求](../enterprise/managed-configuration.zh-CN.md#admin-enforced-requirements-requirementstoml)。

对于 ChatGPT 商业和企业用户，Codex 还可以应用云获取的要求。有关优先级详细信息，请参阅安全页面。

在 `requirements.toml` 中使用 `[features]` 通过 `config.toml` 使用的相同规范键来固定运行时功能标志。要求还可以包括记录的不属于 `config.toml` 的仅应用程序密钥。省略的键仍然不受约束。

某些托管要求强制执行精确的配置值而不是允许列表。用户无法覆盖强制路径、更新首选项、登录 shell 策略、反馈设置或 Windows 私人桌面设置。

托管权限配置文件白名单需要 Codex 0.138.0 或更高版本。 Codex 0.137.0 及更早版本忽略 `allowed_permission_profiles` 和托管 `default_permissions`。

将 `allowed_sandbox_modes` 与 `sandbox_mode` 一起使用。对于权限配置文件部署，请将 `allowed_permission_profiles` 与托管 `default_permissions` 结合使用。

当项目使用 `trust_level = "untrusted"` 时，`allowed_approval_policies` 中的 `untrusted` 条目对于 Codex 派生的更严格的审批行为仍然有效。它不允许显式设置 `approval_policy = "untrusted"`。

`[models.new_thread]` 表提供托管默认值，而不是强制执行。来自专用 CLI 标志或 `--config` 覆盖的显式启动选择优先。显式模型或推理工作覆盖会跳过两个托管模型字段； `service_tier` 是独立的。

浏览器要求涵盖三个独立的使用界面。 `in_app_browser` 控制一个人直接打开和使用的浏览器窗格。 `browser_use` 在浏览器中控制智能体驱动的工作。 `computer_use` 控制本机桌面应用程序中智能体驱动的工作。

嵌套的浏览器使用和计算机使用策略值本身并不授予访问权限。特定于源或应用程序的 `allow` 可以覆盖同一策略源的回退，但正常功能、批准和其他策略检查仍然适用。如果托管要求和 `config.toml` 同时适用，则任一者的 `deny` 获胜。

<ConfigTable
  options={[
    {
      key: "sqlite_home",
      type: "string (path)",
      description:
        "强制执行 Codex 存储 SQLite 支持的运行时状态的目录。",
    },
    {
      key: "log_dir",
      type: "string (path)",
      description: "强制执行 Codex 写入本地日志文件的目录。",
    },
    {
      key: "model_catalog_json",
      type: "string (path)",
      description: "强制执行 Codex 在启动时使用的 JSON 模型目录。",
    },
    {
      key: "check_for_update_on_startup",
      type: "boolean",
      description: "强制Codex启动时是否检查更新。",
    },
    {
      key: "allow_login_shell",
      type: "boolean",
      description: "强制 shell 工具是否可以启动登录 shell。",
    },
    {
      key: "allowed_login_methods",
      type: "array<string>",
      description:
        "允许 `chatgpt`、`api` 或两者。如果省略，此设置不会限制登录方法。如果设置，该列表必须至少包含一种方法。 `api` 允许 API 身份验证，包括 Amazon Bedrock。通过本地系统要求文件或 macOS MDM 设置。云管理的值将被忽略。",
    },
    {
      key: "allowed_chatgpt_workspaces",
      type: "array<string>",
      description:
        "将 ChatGPT 登录（包括 Codex 访问令牌）限制为列出的工作区 ID。空列表禁用 ChatGPT 登录；如果允许，API 身份验证仍然可用。通过本地系统要求文件或macOS MDM设置；云管理的值将被忽略。",
    },
    {
      key: "cli_auth_credentials_store",
      type: "file | keyring | auto | ephemeral",
      description:
        "在加载身份验证之前强制执行 CLI 凭据存储。 `file` 使用 `CODEX_HOME/auth.json`； `keyring` 需要操作系统凭证存储；如果凭证存储不可用，`auto` 将回退到文件； `ephemeral` 将当前进程的凭据保留在内存中。通过本地系统要求文件或macOS MDM设置；云管理的值将被忽略。",
    },
    {
      key: "chatgpt_base_url",
      type: "string",
      description:
        "在身份验证和云策略检索之前强制执行 ChatGPT 服务基 URL。这不会配置每个 Codex 网络目标。通过本地系统要求文件或macOS MDM设置；云管理的值将被忽略。",
    },
    {
      key: "feedback",
      type: "table",
      description: "托管反馈设置。",
    },
    {
      key: "feedback.enabled",
      type: "boolean",
      description:
        "强制用户是否可以跨 Codex 客户端提交反馈。",
    },
    {
      key: "allowed_approval_policies",
      type: "array<string>",
      description:
        "允许的审批策略，例如`on-request`、`never`和`granular`。包含 `untrusted` 以允许源自不受信任项目的更严格的策略；不能直接用`approval_policy` 选择。",
    },
    {
      key: "allowed_approvals_reviewers",
      type: "array<string>",
      description:
        "`approvals_reviewer` 的允许值，例如 `user` 和 `auto_review`。",
    },
    {
      key: "guardian_policy_config",
      type: "string",
      description:
        "用于自动审核的托管 Markdown 策略说明。这优先于本地 `[auto_review].policy`。空白值将被忽略。",
    },
    {
      key: "allowed_permission_profiles",
      type: "table<boolean>",
      description:
        "允许的权限配置文件的完整列表。允许将配置文件设置为 `true`。忽略或设置为 `false` 的配置文件将被拒绝，包括在未来版本中添加的配置文件。组合需求源时，条目将按配置文件名称进行匹配。",
    },
    {
      key: "allowed_permission_profiles.<name>",
      type: "boolean",
      description:
        "允许或拒绝在加载的配置或需求源中定义的内置或自定义权限配置文件。较晚的、较高优先级的要求源可以使用 `false` 来关闭较早的、较低优先级的源所允许的配置文件。",
    },
    {
      key: "default_permissions",
      type: "string",
      description:
        "托管默认权限配置文件。该配置文件必须得到 `allowed_permission_profiles` 的允许。明确设置此项以实现可预测的行为；如果省略，则仅当明确允许 `:workspace` 和 `:read-only` 时，Codex 默认为 `:workspace`。",
    },
    {
      key: "enforce_residency",
      type: "string",
      description:
        "需要 Codex 服务流量才能使用支持的数据驻留。目前接受 `us`。",
    },
    {
      key: "models",
      type: "table",
      description:
        "新线程的托管模型默认值。这些值优先于用户和项目默认值，但新线程的显式选择可以覆盖它们。",
    },
    {
      key: "models.new_thread",
      type: "table",
      description:
        "默认在新的本地线程启动时应用。每个模型设置都是可选的。",
    },
    {
      key: "models.new_thread.model",
      type: "string",
      description:
        "新线程的默认模型。显式 `--model` 或模型/推理 `--config` 覆盖优先。",
    },
    {
      key: "models.new_thread.model_reasoning_effort",
      type: "string",
      description:
        "新线程的默认推理工作。显式模型或推理工作覆盖会跳过两个托管模型字段。",
    },
    {
      key: "models.new_thread.service_tier",
      type: "string",
      description:
        "新线程的默认服务层。显式服务层覆盖优先，与模型字段无关。",
    },
    {
      key: "permissions",
      type: "table",
      description:
        "管理员定义的权限配置文件由配置文件名称键入。使用与 `config.toml` 相同的配置文件字段。",
    },
    {
      key: "permissions.<name>",
      type: "table",
      description:
        "管理员定义的权限配置文件。该名称不能以 `:` 开头、使用保留名称 `filesystem` 或从加载的配置中复制配置文件。使用与 `config.toml` 相同的配置文件字段；有关完整的配置文件架构，请参阅权限指南。",
    },
    {
      key: "allowed_sandbox_modes",
      type: "array<string>",
      description: "`sandbox_mode` 的允许值。",
    },
    {
      key: "windows",
      type: "table",
      description: "本机 Windows 沙箱要求。",
    },
    {
      key: "windows.allowed_sandbox_implementations",
      type: "array<string>",
      description:
        "允许 `windows.sandbox`（`elevated` 和 `unelevated`）的本机 Windows 沙箱实现。该列表不能为空。当两者都允许且未选择任何模式时，Codex 优先选择 `elevated`。",
    },
    {
      key: "windows.sandbox_private_desktop",
      type: "boolean",
      description:
        "强制本机 Windows 沙箱是否在私有桌面上启动其子进程。",
    },
    {
      key: "remote_sandbox_config",
      type: "array<table>",
      description:
        "主机特定的沙箱要求。 `hostname_patterns` 与解析的主机名匹配的第一个条目将覆盖该需求源的顶级 `allowed_sandbox_modes`。特定于主机的条目当前仅覆盖沙箱模式。",
    },
    {
      key: "remote_sandbox_config[].hostname_patterns",
      type: "array<string>",
      description:
        "不区分大小写的主机名模式。支持任何字符序列的 `*` 和一个字符的 `?`。",
    },
    {
      key: "remote_sandbox_config[].allowed_sandbox_modes",
      type: "array<string>",
      description:
        "当此特定于主机的条目匹配时，允许应用沙箱模式。",
    },
    {
      key: "allowed_web_search_modes",
      type: "array<string>",
      description:
        "`web_search`（`disabled`、`cached`、`indexed`、`live`）的允许值。 `disabled` 始终允许；空列表实际上只允许 `disabled`。",
    },
    {
      key: "allow_managed_hooks_only",
      type: "boolean",
      description:
        "当 `true` 时，Codex 会跳过用户、项目、会话和插件挂钩，同时仍然允许来自 `requirements.toml` 和其他托管配置层的托管挂钩。",
    },
    {
      key: "allow_appshots",
      type: "boolean",
      description:
        "设置为 `false` 以禁用托管用户的 Appshots。如果省略，Appshots 将不受要求的限制并遵循正常的产品可用性。",
    },
    {
      key: "allow_remote_control",
      type: "boolean",
      description:
        "设置为 `false` 以禁用受管理用户的设备远程控制。如果省略，设备远程控制将不受要求的限制并遵循正常的产品可用性。",
    },
    {
      key: "allow_browser_and_computer_use",
      type: "boolean",
      description:
        "设置为 `false` 可阻止智能体驱动的浏览器使用和本机应用程序计算机使用。将其设置为 `true` 或忽略它不会启用任一功能；其余的功能、策略和批准检查仍然适用。",
    },
    {
      key: "features.plugin_sharing",
      type: "boolean",
      description:
        "在云管理的 `requirements.toml` 中设置为 `false` 以禁用本地构建的插件的工作区共享。",
    },
    {
      key: "features",
      type: "table",
      description:
        "固定特征值。使用 `config.toml` 中的规范名称来表示运行时功能；此处还支持记录的仅应用程序要求密钥。",
    },
    {
      key: "features.<name>",
      type: "boolean",
      description:
        "需要记录运行时或应用程序功能以保持启用或禁用状态。",
    },
    {
      key: "features.apps",
      type: "boolean",
      description:
        "为托管用户打开或关闭应用程序集成可用性。",
    },
    {
      key: "features.in_app_updates",
      type: "boolean",
      description:
        "在 `requirements.toml` 中设置为 `false` 以禁用应用内更新。当忽略此要求时，更新默认保持启用状态。",
    },
    {
      key: "features.in_app_browser",
      type: "boolean",
      description:
        "在`requirements.toml`中设置为`false`以禁用用户直接打开和控制的内置浏览器窗格。",
    },
    {
      key: "features.browser_use",
      type: "boolean",
      description:
        "在 `requirements.toml` 中设置为 `false` 以禁用智能体驱动的浏览器使用。",
    },
    {
      key: "features.browser_use_external",
      type: "boolean",
      description:
        "在 `requirements.toml` 中设置为 `false`，以防止 Codex 通过 ChatGPT 浏览器扩展操作支持的浏览器，包括现有选项卡和登录会话。",
    },
    {
      key: "features.browser_use_full_cdp_access",
      type: "boolean",
      description:
        "在 `requirements.toml` 中设置为 `false` 以禁用本地运行时中的完整 Chrome DevTools 协议访问，包括浏览器开发人员模式，并阻止 ChatGPT 桌面应用程序启用相应的设置。如果省略，则适用正常的产品可用性。",
    },
    {
      key: "features.fast_mode",
      type: "boolean",
      description:
        "为托管用户打开或关闭规范 `fast_mode` 功能。",
    },
    {
      key: "features.guardian_approval",
      type: "boolean",
      description:
        "为托管用户启用或关闭 Guardian 批准可用性。",
    },
    {
      key: "features.memories",
      type: "boolean",
      description: "为托管用户打开或关闭内存可用性。",
    },
    {
      key: "features.multi_agent",
      type: "boolean",
      description: "为托管用户打开或关闭多智能体可用性。",
    },
    {
      key: "features.plugins",
      type: "boolean",
      description: "为托管用户启用或关闭插件可用性。",
    },
    {
      key: "features.remote_plugin",
      type: "boolean",
      description:
        "为托管用户启用或关闭远程插件目录可用性。",
    },
    {
      key: "features.computer_use",
      type: "boolean",
      description:
        "在 `requirements.toml` 中设置为 `false` 以禁用计算机使用、Record & Replay 以及相关安装或启用流程。",
    },
    {
      key: "features.workspace_dependencies",
      type: "boolean",
      description:
        "为托管用户打开或关闭捆绑的工作区依赖运行时可用性。",
    },
    {
      key: "in_app_browser",
      type: "table",
      description:
        "对内置浏览器窗格的要求。这些设置不控制智能体驱动的浏览器使用。",
    },
    {
      key: "in_app_browser.allow_external_browser_settings_import",
      type: "boolean",
      description:
        "设置为 `false` 可防止用户将设置或浏览数据从外部浏览器导入到内置浏览器中。将其设置为 `true` 或省略它会使导入在其他产品检查允许时可用。这是仅受管理的设置，没有 `config.toml` 覆盖。",
    },
    {
      key: "browser_use",
      type: "table",
      description: "智能体驱动的浏览器使用的托管要求。",
    },
    {
      key: "browser_use.allow_history_access",
      type: "boolean",
      description:
        "设置为 `false` 以防止浏览器使用读取浏览器历史记录。将其设置为 `true` 或忽略它会保留正常的历史记录设置和可用性检查。",
    },
    {
      key: "browser_use.disable_auto_review",
      type: "boolean",
      description:
        "设置为 `true` 可跳过浏览器使用的自动审核，而是请求用户批准。将其设置为 `false` 或忽略它会在其他设置允许时保留自动审核功能。",
    },
    {
      key: "browser_use.allow_global_persistent_approval",
      type: "boolean",
      description:
        "设置为 `false` 可防止浏览器使用创建或遵守覆盖每个站点的 `Always allow` 批准，例如允许从任何站点下载。现有的已保存批准将被忽略，而不是被删除。将其设置为 `true` 或忽略它不会创建批准。",
    },
    {
      key: "browser_use.default_origin_policy",
      type: "table",
      description:
        "当 `browser_use.origins` 下没有匹配条目定义每个浏览器使用设置时，该设置的回退。匹配的源规则会替换该源的后备规则。 Codex 然后应用来自托管要求和用户配置的更严格的结果。",
    },
    {
      key: "browser_use.default_origin_policy.access",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止在使用回退的源上使用浏览器。被拒绝的来源还会阻止上传、下载、完整的浏览器调试访问以及自动审查。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.default_origin_policy.downloads",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止使用回退源的浏览器使用下载。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.default_origin_policy.uploads",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止浏览器使用在使用回退的源上上传。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.default_origin_policy.full_cdp_access",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止对使用回退的源的完整 Chrome DevTools 协议 (CDP) 访问。 `allow` 只允许正常的选择加入和批准检查继续。",
    },
    {
      key: "browser_use.default_origin_policy.auto_review",
      type: "allow | deny",
      description:
        "使用 `deny` 跳过对使用回退的源的自动审核，并请求用户批准。当其他设置允许时，`allow` 保留自动审核可用。",
    },
    {
      key: "browser_use.default_origin_policy.persistent_approval",
      type: "boolean",
      description:
        "设置为 `false` 以防止浏览器使用在使用后备的源上保存或遵守 `Always allow` 批准。当前轮次或线程的批准仍然适用。 `true` 在另有许可的情况下使 `Always allow` 可用，但不创建批准。",
    },
    {
      key: "browser_use.default_origin_policy.access_approval_lifetime",
      type: "turn | thread",
      description:
        "设置非持久站点访问批准的持续时间：`turn` 将其限制为当前轮次，`thread` 将其保留到当前线程的其余部分。 `persistent_approval`单独控制`Always allow`是否可用。产品默认为`thread`。",
    },
    {
      key: "browser_use.origins",
      type: "map<string, table>",
      description:
        '特定于源的浏览器使用策略。密钥使用 `<scheme>://<host-pattern>[:<port>]` 和 `http` 或 `https`。使用确切的主机，仅对子域使用 `*.example.com`，或对基本域及其子域使用 `**.example.com`。其他 `*` 通配符可以跨越点，因此 `region*.example.com` 也匹配 `region.api.example.com`； `*` 的主机与该方案的每个主机相匹配。方案和非默认端口很重要；显式默认端口被规范化。路径、查询、嵌入的用户名或密码以及通配符方案或端口无效。引用 TOML 中的模式，例如 `[browser_use.origins."https://**.example.com"]`。',
    },
    {
      key: "browser_use.origins.<pattern>",
      type: "table",
      description:
        "与此模式匹配的来源的策略。如果多个模式匹配，则 Codex 对每种功能使用最严格的值：`deny` 高于 `allow`、`false` 高于 `true` 以及 `turn` 高于 `thread`。",
    },
    {
      key: "browser_use.origins.<pattern>.access",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止匹配源上的浏览器使用。拒绝还会阻止上传、下载、完整的浏览器调试访问以及自动审查。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.origins.<pattern>.downloads",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止浏览器使用匹配来源的下载。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.origins.<pattern>.uploads",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止浏览器使用匹配源上的上传。 `allow` 只允许正常的审批和策略检查继续。",
    },
    {
      key: "browser_use.origins.<pattern>.full_cdp_access",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止匹配源上的完整 Chrome DevTools 协议 (CDP) 访问。 `allow` 只允许正常的选择加入和批准检查继续。",
    },
    {
      key: "browser_use.origins.<pattern>.auto_review",
      type: "allow | deny",
      description:
        "使用 `deny` 跳过对匹配来源的自动审核，并请求用户批准。当其他设置允许时，`allow` 保留自动审核可用。",
    },
    {
      key: "browser_use.origins.<pattern>.persistent_approval",
      type: "boolean",
      description:
        "设置为 `false` 以防止浏览器使用保存或遵守匹配源的 `Always allow` 批准。当前轮次或线程的批准仍然适用。 `true` 在另有许可的情况下使 `Always allow` 可用，但不创建批准。",
    },
    {
      key: "browser_use.origins.<pattern>.access_approval_lifetime",
      type: "turn | thread",
      description:
        "设置匹配源的非持久站点访问批准的持续时间：`turn` 将其限制为当前回合，`thread` 将其保留到当前线程的其余部分。 `persistent_approval`单独控制`Always allow`是否可用。",
    },
    {
      key: "computer_use",
      type: "table",
      description:
        "管理本机桌面应用程序中智能体驱动工作的要求。托管应用程序规则和 `config.toml` 应用程序规则均强制执行；每个策略源都必须允许应用程序。",
    },
    {
      key: "computer_use.allow_locked_computer_use",
      type: "boolean",
      description:
        "设置为 `false` 可防止用户在托管 macOS 设备上启用锁定使用。此要求删除了启用控制；如果已启用“锁定使用”，则不会将其关闭。如果省略，则适用正常的产品可用性。",
    },
    {
      key: "computer_use.allow_persistent_approval",
      type: "boolean",
      description:
        "设置为 `false` 以删除跨会话保存应用程序批准的选项。当前会议的批准仍然可用。将其设置为 `true` 或忽略它不会批准应用程序。",
    },
    {
      key: "computer_use.default_app_access",
      type: "allow | deny",
      description:
        "对与特定于平台的规则不匹配的本机应用程序的后备访问。 `deny` 阻止访问。 `allow` 只允许正常的审批和策略检查继续。产品默认为`allow`。",
    },
    {
      key: "computer_use.macos",
      type: "table",
      description: "计算机 使用适用于 macOS 的应用程序规则。",
    },
    {
      key: "computer_use.macos.bundle_ids",
      type: "map<string, allow | deny>",
      description:
        "将确切的 macOS 包标识符映射到 `allow` 或 `deny`。匹配规则替换同一策略源中的 `computer_use.default_app_access`。来自托管要求或用户配置的拒绝仍然会阻止访问。",
    },
    {
      key: "computer_use.macos.bundle_ids.<bundle-id>",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止确切的包标识符。 `allow` 仅覆盖此策略源的默认设置，但仍需要任何其他策略源和正常审批流程才能允许该应用程序。",
    },
    {
      key: "computer_use.windows",
      type: "table",
      description:
        "计算机 对打包和未打包的 Windows 应用程序使用应用程序规则。",
    },
    {
      key: "computer_use.windows.aumids",
      type: "map<string, allow | deny>",
      description:
        "将签名的打包应用程序的准确注册应用程序用户模型 ID (AUMID) 映射到 `allow` 或 `deny`。匹配规则替换同一策略源中的 `computer_use.default_app_access`。",
    },
    {
      key: "computer_use.windows.aumids.<aumid>",
      type: "allow | deny",
      description:
        "使用 `deny` 阻止确切的打包应用程序身份。 `allow` 仅覆盖此策略源的默认设置，但仍需要任何其他策略源和正常审批流程才能允许该应用程序。",
    },
    {
      key: "computer_use.windows.exes",
      type: "array<table>",
      description:
        "已签名、未打包的 Windows 可执行文件的规则。规则匹配可执行文件的已验证发布者和签名版本信息，而不是其路径或当前文件名。匹配拒绝优先于匹配允许。未签名的可执行文件使用`computer_use.default_app_access`；无法明确验证签名身份的可执行文件将被阻止。",
    },
    {
      key: "computer_use.windows.exes[].publisher_name",
      type: "string",
      description:
        "需要来自可执行文件的受信任签名证书的准确发布者名称，格式为 Windows X.500 可分辨名称。",
    },
    {
      key: "computer_use.windows.exes[].product_name",
      type: "string",
      description:
        "需要从可执行文件的签名版本信息中获取准确的 `ProductName`。",
    },
    {
      key: "computer_use.windows.exes[].binary_name",
      type: "string",
      description:
        "来自可执行文件的签名版本信息的可选 `OriginalFilename`。匹配不区分大小写。如果匹配的发布者和产品规则需要此值，但可执行文件未提供该值，则“计算机使用”会阻止该可执行文件。",
    },
    {
      key: "computer_use.windows.exes[].access",
      type: "allow | deny",
      description:
        "匹配可执行文件所需的访问决策。 `deny` 阻止访问。 `allow` 仅覆盖此策略源的默认设置，但仍需要任何其他策略源和正常审批流程才能允许该应用程序。",
    },
    {
      key: "experimental_network",
      type: "table",
      description:
        "沙箱本地命令的管理员管理网络要求，从 `requirements.toml` 强制执行。启用后，这些要求无需 `features.network_proxy` 即可启动命令网络代理。浏览器工具单独检查托管网络拒绝和独占允许列表。这些要求不会通过代理路由浏览器流量或控制 Web 搜索、应用程序、MCP 服务器、本机应用程序流量或 Codex 云网络。",
    },
    {
      key: "experimental_network.enabled",
      type: "boolean",
      description:
        "启用沙箱网络要求。当活动沙箱保持命令网络关闭时，这不会授予网络访问权限。",
    },
    {
      key: "experimental_network.http_port",
      type: "integer",
      description:
        "用于满足 `[experimental_network]` 要求的环回 HTTP 侦听器端口。",
    },
    {
      key: "experimental_network.socks_port",
      type: "integer",
      description:
        "用于满足 `[experimental_network]` 要求的环回 SOCKS5 侦听器端口。",
    },
    {
      key: "experimental_network.allow_upstream_proxy",
      type: "boolean",
      description:
        "允许沙箱网络通过环境中的上游代理进行链接。",
    },
    {
      key: "experimental_network.dangerously_allow_non_loopback_proxy",
      type: "boolean",
      description:
        "允许非环回侦听器地址以满足 `[experimental_network]` 要求。启用它可以暴露本地主机之外的侦听器。",
    },
    {
      key: "experimental_network.dangerously_allow_all_unix_sockets",
      type: "boolean",
      description:
        "允许任意 Unix 套接字目标，而不是仅允许访问。仅在严格控制的环境中使用。",
    },
    {
      key: "experimental_network.domains",
      type: "map<string, allow | deny>",
      description:
        "用于沙箱网络的地图形管理员域策略。支持精确主机，仅支持子域的 `*.example.com`，支持顶级子域的 `**.example.com`，以及全局 `*` 允许规则；更喜欢范围规则，因为 `*` 广泛开放公共出站访问。 `deny` 在冲突中获胜。请勿将其与 `experimental_network.allowed_domains` 或 `experimental_network.denied_domains` 结合使用。",
    },
    {
      key: "experimental_network.allowed_domains",
      type: "array<string>",
      description:
        "启用托管网络代理时，管理员允许沙箱命令网络规则。这些规则不适用于网络搜索、应用程序或 MCP 服务器。请勿将其与 `experimental_network.domains` 结合使用。",
    },
    {
      key: "experimental_network.denied_domains",
      type: "array<string>",
      description:
        "沙箱网络的列表形管理员拒绝规则。请勿将其与 `experimental_network.domains` 结合使用。",
    },
    {
      key: "experimental_network.managed_allowed_domains_only",
      type: "boolean",
      description:
        "当 `true` 时，只有管理员管理的允许规则在沙箱网络要求处于活动状态时保持有效；添加的用户白名单将被忽略。如果没有托管允许规则，用户添加的域允许规则将不会保持有效。",
    },
    {
      key: "experimental_network.unix_sockets",
      type: "map<string, allow | deny>",
      description:
        "用于沙箱网络的管理员管理的 Unix 套接字策略。",
    },
    {
      key: "experimental_network.allow_local_binding",
      type: "boolean",
      description:
        "允许沙箱网络更广泛的本地/专用网络访问。当其保持 `false` 时，精确的本地 IP 文字或 `localhost` 允许规则仍然可以允许特定的本地目标。",
    },
    {
      key: "hooks",
      type: "table",
      description:
        "管理员强制执行的托管生命周期挂钩。需要托管挂钩目录并使用与 `config.toml` 中的内联 `[hooks]` 相同的事件架构。",
    },
    {
      key: "hooks.managed_dir",
      type: "string (absolute path)",
      description:
        "包含 macOS 和 Linux 上的托管挂钩脚本的目录。 Codex 验证它是绝对的并且在加载托管挂钩之前存在。",
    },
    {
      key: "hooks.windows_managed_dir",
      type: "string (absolute path)",
      description:
        "包含 Windows 上托管挂钩脚本的目录。 Codex 验证它是绝对的并且在加载托管挂钩之前存在。",
    },
    {
      key: "hooks.<Event>",
      type: "array<table>",
      description:
        "挂钩事件的匹配器组，例如 `PreToolUse`、`PermissionRequest`、`PostToolUse`、`PreCompact`、`PostCompact`、`SessionStart`、`SessionEnd`、`SubagentStart`、`SubagentStop`、 `UserPromptSubmit`，或`Stop`。",
    },
    {
      key: "hooks.<Event>[].hooks",
      type: "array<table>",
      description:
        "匹配器组的挂钩处理程序。支持命令和 MCP 工具挂钩，同时解析但跳过提示和智能体挂钩处理程序。",
    },
    {
      key: "hooks.<Event>[].hooks[].async",
      type: "boolean",
      description:
        "在后台运行命令挂钩，不会延迟触发操作。默认为`false`； `SessionEnd` 始终同步运行。参见 [在后台运行钩子](../hooks.zh-CN.md#run-hooks-in-the-background)。",
    },
    {
      key: "hooks.<Event>[].hooks[].additionalContextLimit",
      type: "integer",
      description:
        "将超大 `additionalContext` 保存到磁盘并向模型显示较短预览的每个处理程序令牌的近似阈值。默认为`2500`； `0` 将完整上下文直接传递给模型。参见 [大钩输出](../hooks.zh-CN.md#large-hook-output)。",
    },
    {
      key: "hooks.<Event>[].hooks[].commandWindows",
      type: "string",
      description:
        "命令挂钩的仅限 Windows 命令覆盖。 TOML 别名 `command_windows` 也被接受。",
    },
    {
      key: "permissions.filesystem.deny_read",
      type: "array<string>",
      description:
        "管理员强制执行文件系统读取拒绝。条目可以是路径或全局模式，用户不能通过本地配置削弱它们。",
    },
    {
      key: "mcp_servers",
      type: "table",
      description:
        "可能启用的 MCP 服务器的白名单。服务器名称 (`<id>`) 及其标识必须与要启用的 MCP 服务器匹配。任何不在允许列表中（或身份不匹配）的已配置 MCP 服务器都将被禁用。",
    },
    {
      key: "mcp_servers.<id>.identity",
      type: "table",
      description:
        "单个 MCP 服务器的身份规则。设置 `command` (stdio) 或 `url` (流式 HTTP)。",
    },
    {
      key: "mcp_servers.<id>.identity.command",
      type: "string | table",
      description:
        "通过精确的命令字符串允许 MCP stdio 服务器，或使用匹配器表来要求精确的可执行文件和有序参数匹配器。字符串形式不检查参数 `cwd`、`env` 或 `env_vars`。",
    },
    {
      key: "mcp_servers.<id>.identity.command.executable",
      type: "string",
      description:
        "stdio 服务器配置的 `command` 的可执行文件必须完全匹配。",
    },
    {
      key: "mcp_servers.<id>.identity.command.args",
      type: "array<table>",
      description:
        "stdio 服务器的有序参数匹配器。配置的参数列表必须具有相同的长度，并且每个位置必须匹配。命令匹配器不检查 `cwd`、`env` 或 `env_vars`。",
    },
    {
      key: "mcp_servers.<id>.identity.command.args[].match",
      type: "exact | prefix | regex",
      description: "该参数位置的匹配操作。",
    },
    {
      key: "mcp_servers.<id>.identity.command.args[].value",
      type: "string",
      description: "`exact` 或 `prefix` 参数匹配器使用的值。",
    },
    {
      key: "mcp_servers.<id>.identity.command.args[].expression",
      type: "string",
      description:
        "`regex` 参数匹配器使用的正则表达式。表达式必须有效并与完整的参数值匹配。",
    },
    {
      key: "mcp_servers.<id>.identity.url",
      type: "string | table",
      description:
        "通过精确的 URL 字符串允许 MCP 可流式 HTTP 服务器，或使用 `exact`、`prefix` 或 `regex` 值匹配器表。",
    },
    {
      key: "mcp_servers.<id>.identity.url.match",
      type: "exact | prefix | regex",
      description: "对配置的 MCP 服务器 URL 进行匹配操作。",
    },
    {
      key: "mcp_servers.<id>.identity.url.value",
      type: "string",
      description: "`exact` 或 `prefix` URL 匹配器使用的值。",
    },
    {
      key: "mcp_servers.<id>.identity.url.expression",
      type: "string",
      description:
        "`regex` URL 匹配器使用的正则表达式。该表达式必须有效并与完整的 URL 值匹配。",
    },
    {
      key: "plugins",
      type: "table",
      description:
        "特定于插件的 MCP 服务器允许列表由插件标识符键入。当此表存在时，没有匹配插件和服务器条目的插件捆绑服务器将被禁用。",
    },
    {
      key: "plugins.<plugin>.mcp_servers",
      type: "table",
      description:
        "与一个插件捆绑在一起的 MCP 服务器的白名单。插件服务器要求使用与顶级 `mcp_servers` 要求相同的确切身份和匹配器形式。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity",
      type: "table",
      description:
        "一台捆绑插件的 MCP 服务器的身份规则。设置 `command` (stdio) 或 `url` (流式 HTTP)。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command",
      type: "string | table",
      description:
        "通过精确的命令字符串允许插件的 stdio MCP 服务器，或使用匹配器表来要求精确的可执行文件和有序参数匹配器。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command.executable",
      type: "string",
      description:
        "插件捆绑的 stdio 服务器的配置命令必须完全匹配的可执行文件。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command.args",
      type: "array<table>",
      description:
        "插件捆绑的 stdio 服务器的有序参数匹配器。配置的参数列表必须具有相同的长度，并且每个位置必须匹配。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command.args[].match",
      type: "exact | prefix | regex",
      description: "该参数位置的匹配操作。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command.args[].value",
      type: "string",
      description: "`exact` 或 `prefix` 参数匹配器使用的值。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.command.args[].expression",
      type: "string",
      description:
        "`regex` 参数匹配器使用的正则表达式。表达式必须与完整的参数值匹配。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.url",
      type: "string | table",
      description:
        "通过精确的 URL 字符串允许插件的可流式 HTTP MCP 服务器，或使用 `exact`、`prefix` 或 `regex` 值匹配器表。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.url.match",
      type: "exact | prefix | regex",
      description: "插件捆绑的 MCP 服务器 URL 的匹配操作。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.url.value",
      type: "string",
      description: "`exact` 或 `prefix` URL 匹配器使用的值。",
    },
    {
      key: "plugins.<plugin>.mcp_servers.<server>.identity.url.expression",
      type: "string",
      description:
        "`regex` URL 匹配器使用的正则表达式。该表达式必须与完整的 URL 值匹配。",
    },
    {
      key: "marketplaces",
      type: "table",
      description:
        "插件市场源的管理要求。规则在`restrict_to_allowed_sources`为`true`时生效。",
    },
    {
      key: "marketplaces.restrict_to_allowed_sources",
      type: "boolean",
      description:
        "当 `true` 时，要求配置的市场源与 `allowed_sources` 匹配，以进行市场添加、插件安装、刷新和运行时加载。 OpenAI 管理的 Git 目录（包括 API 密钥目录）也必须匹配允许列表。捆绑和远程安装的工作区插件与此策划的 Git 源策略是分开的。",
    },
    {
      key: "marketplaces.allowed_sources",
      type: "table",
      description:
        "由管理员选择的规则名称键入的允许的市场来源。不同的名称在需求层中累积；相同名称下的字段使用正常的层优先级。",
    },
    {
      key: "marketplaces.allowed_sources.<name>",
      type: "table",
      description:
        "一项允许的源规则。需求合并后的最终 `source` 值决定 Codex 解释哪些同级字段。",
    },
    {
      key: "marketplaces.allowed_sources.<name>.source",
      type: "git | host_pattern | local",
      description:
        "市场源匹配器类型。对一个仓库使用 `git`，对正则表达式匹配的 Git 主机使用 `host_pattern`，对一个目录使用 `local`。",
    },
    {
      key: "marketplaces.allowed_sources.<name>.url",
      type: "string",
      description:
        '`source = "git"` 时需要 Git 仓库 URL。 Codex 在要求精确的仓库匹配之前规范配置和允许的 URL。',
    },
    {
      key: "marketplaces.allowed_sources.<name>.ref",
      type: "string",
      description:
        "`git` 规则的可选精确 Git 引用。省略时，该规则允许匹配仓库的任何引用。",
    },
    {
      key: "marketplaces.allowed_sources.<name>.host_pattern",
      type: "string",
      description:
        '`source = "host_pattern"` 时需要正则表达式。 Codex 将其与从 HTTPS、SSH 或 SCP 样式 Git 源解析的小写主机名进行匹配。使用 `^` 和 `$` 要求整个主机匹配。',
    },
    {
      key: "marketplaces.allowed_sources.<name>.path",
      type: "string (absolute path)",
      description:
        '`source = "local"` 时需要本地市场目录。 Codex 需要绝对路径并比较标准化后的路径。',
    },
    {
      key: "apps",
      type: "table",
      description:
        "按应用程序标识符键入的托管应用程序要求。要求可以禁用应用程序或限制单个工具的批准行为。",
    },
    {
      key: "apps.<id>.enabled",
      type: "boolean",
      description:
        "设置为 `false` 以禁用应用程序。当合并多个需求源时，禁用的需求仍然具有限制性。",
    },
    {
      key: "apps.<id>.tools.<tool>.approval_mode",
      type: "auto | prompt | writes | approve",
      description: "为一个应用工具设置托管审批模式。",
    },
    {
      key: "rules",
      type: "table",
      description:
        "管理员强制执行的命令规则与 `.rules` 文件合并。需求规则必须是限制性的。",
    },
    {
      key: "rules.prefix_rules",
      type: "array<table>",
      description:
        "强制执行的前缀规则列表。每条规则必须包含 `pattern` 和 `decision`。",
    },
    {
      key: "rules.prefix_rules[].pattern",
      type: "array<table>",
      description:
        "以模式标记表示的命令前缀。每个令牌设置 `token` 或 `any_of`。",
    },
    {
      key: "rules.prefix_rules[].pattern[].token",
      type: "string",
      description: "此位置有一个文字标记。",
    },
    {
      key: "rules.prefix_rules[].pattern[].any_of",
      type: "array<string>",
      description: "此位置允许的替代令牌的列表。",
    },
    {
      key: "rules.prefix_rules[].decision",
      type: "prompt | forbidden",
      description:
        "必填。需求规则只能提示或禁止（不允许）。",
    },
    {
      key: "rules.prefix_rules[].justification",
      type: "string",
      description:
        "可选的非空理由出现在批准提示或拒绝消息中。",
    },
  ]}
  client:load
/>