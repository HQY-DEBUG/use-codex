> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/config-file/config-sample.md)。

<a id="sample-configuration"></a>

# 配置示例

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用此示例配置作为起点。它包括 Codex 从 `config.toml` 读取的大多数键，以及默认行为、有用的推荐值和简短注释。

有关说明和指导，请参阅：

- [配置基础知识](config-basic.zh-CN.md)
- [高级配置](config-advanced.zh-CN.md)
- [配置参考](config-reference.zh-CN.md)
- [沙箱和批准](../agent-approvals-security.zh-CN.md#sandbox-and-approvals)
- [受管配置](../enterprise/managed-configuration.zh-CN.md)

使用下面的代码片段作为参考。仅将您需要的键和部分复制到 `~/.codex/config.toml`（或项目范围的 `.codex/config.toml`）中，然后调整您的设置的值。

```toml
# Codex 示例配置 (config.toml)
#
# 该文件列出了 Codex 从 config.toml 读取的主键以及默认值
# 行为、推荐示例和简明解释。根据需要进行调整。
#
# 注释
# - 根键必须出现在 TOML 中的表之前。
# - 默认为“未设置”的可选键显示为注释注释。
# - MCP 服务器、配置文件和模型提供程序是示例；删除或编辑。

################################################################################

# 核心模型选择

################################################################################

# Codex 使用的主要模型。为大多数用户推荐的示例：“gpt-5.6”。

model = "gpt-5.6"

# 支持的模型的通信方式。允许值：none | friendly | pragmatic

# personality = "pragmatic"

# /review 的可选模型覆盖。默认值：未设置（使用当前会话模型）。

# review_model = "gpt-5.6"

# 从 [model_providers] 中选择的提供商 ID。默认值：“openai”。

model_provider = "openai"

# --oss 会话的默认 OSS 提供程序。未设置时，会出现 Codex 提示。默认值：未设置。

# oss_provider = "ollama"

# 首选服务级别。使用快速或活动模型支持的其他层。

# service_tier = "fast"

# 可选的手动模型元数据。未设置时，Codex 使用模型或预设默认值。

# model_context_window = 128000 # tokens; default: auto for model

# model_auto_compact_token_limit = 64000 # tokens; unset uses model defaults

# model_auto_compact_token_limit_scope = "total" # total | body_after_prefix; default: total

# tool_output_token_limit = 12000 # tokens stored per tool output

# model_catalog_json = "/absolute/path/to/models.json" # optional startup-only model catalog override

# background_terminal_max_timeout = 300000 # ms; max empty write_stdin poll window (default 5m)

# log_dir = "/absolute/path/to/codex-logs" # log directory; setting explicitly enables codex-tui.log; default: "$CODEX_HOME/log"

# sqlite_home = "/absolute/path/to/codex-state" # optional SQLite-backed runtime state directory

################################################################################

# 推理和冗长（支持响应 API 的模型）

################################################################################

# 推理工作量：最小 | 低 | 中等 | 高 | xhigh

# model_reasoning_effort = "medium"

# Codex 在计划模式下运行时使用的可选覆盖： 无 | 最小 | 低 | 中 | 高 | xhigh

# plan_mode_reasoning_effort = "high"

# 推理总结：汽车 | 简洁 | 详细 | 无

# model_reasoning_summary = "auto"

# GPT-5 系列（响应 API）的文本详细程度：低 | 中 | 高

# model_verbosity = "medium"

# 强制启用或禁用当前模型的推理摘要。

# model_supports_reasoning_summaries = true

################################################################################

# 指令覆盖

################################################################################

# 在 AGENTS.md 之前注入额外的用户指令。默认值：未设置。

# developer_instructions = ""

# 历史压缩提示的内联覆盖。默认值：未设置。

# compact_prompt = ""

# 使用文件路径覆盖内置基本指令。默认值：未设置。

# model_instructions_file = "/absolute/or/relative/path/to/instructions.txt"

# 从文件加载紧凑提示覆盖。默认值：未设置。

# experimental_compact_prompt_file = "/absolute/or/relative/path/to/compact_prompt.txt"

################################################################################

# 通知

################################################################################

# 外部通知程序（argv 数组）。未设置时：禁用。

# notify = ["notify-send", "Codex"]

################################################################################

# 批准和沙箱

################################################################################

# 何时请求指挥部批准：

# - 根据请求：模型决定何时询问（默认）

# - 从不：从不提示（有风险）

# - { Granular = { ... } }：允许或自动拒绝选定的提示类别

approval_policy = "on-request"

# 谁审查符合条件的批准提示：用户（默认）| auto_review

# approvals_reviewer = "user"

# 细粒度策略示例：

# approval_policy = { granular = {

# sandbox_approval = true,

# rules = true,

# mcp_elicitations = true,

# request_permissions = false,

# skill_approval = false

# } }

# 当基于 shell 的工具请求 `login = true` 时，允许使用登录 shell 语义。

# 默认值：true。设置 false 以强制非登录 shell 并拒绝显式登录 shell 请求。

allow_login_shell = true

# 工具调用的文件系统/网络沙箱策略：

# - 只读（默认）

# - 工作区写入

# - 危险-完全访问（无沙箱；风险极高）

sandbox_mode = "read-only"

# 默认情况下应用的命名权限配置文件。内置：

# ：只读 | ：工作区 | ：危险-完全访问

# 仅当您还定义了 [permissions.workspace] 时，才使用自定义名称，例如“workspace”。

# default_permissions = ":workspace"

################################################################################

# 认证与登录

################################################################################

# CLI 登录凭据的保存位置：文件（默认） | 密钥环 | 自动

cli_auth_credentials_store = "file"

# ChatGPT 身份验证流程的基本 URL（不是 OpenAI API）。

chatgpt_base_url = "https://chatgpt.com/backend-api/"

# 内置 OpenAI 提供程序的可选基本 URL 覆盖。

# openai_base_url = "https://us.api.openai.com/v1"

# 将 ChatGPT 登录限制为特定工作区 ID。默认值：未设置。

# forced_chatgpt_workspace_id = "00000000-0000-0000-0000-000000000000"

# Codex 通常会自动选择时强制登录机制。默认值：未设置。

# 允许的值：chatgpt | api

# forced_login_method = "chatgpt"

# MCP OAuth 凭证的首选存储：自动（默认） | 文件 | 密钥环

mcp_oauth_credentials_store = "auto"

# 在构建初始工具目录之前共享等待可选的 MCP 服务器。

# 默认值：1000 毫秒。设置为 0 以等待每个服务器的startup_timeout_sec。

# mcp_optional_startup_grace_ms = 1000

# MCP OAuth 回调的可选全局固定端口：1-65535。默认值：未设置。

# 服务器特定的 oauth.callback_port 会覆盖此全局侦听器端口。

# mcp_oauth_callback_port = 4321

# MCP OAuth 登录的可选回调 URL 覆盖（例如，远程 devbox 入口）。

# 新添加的预注册客户端在授权服务器时使用此 URL 不变

# 宣传发行者支持并提供元数据发行者。如果没有发行人的支持，

# Codex 附加服务器特定的回调 ID。未保存的现有客户

# 回调保留该后缀。

# 如果没有发行人支持，任何预注册的 MCP 服务器回调必须已经

# 以正确的回调 ID 结尾。否则，Codex 会忽略该回调并使用

# 此全局 URL（或其默认值）附加了服务器特定的回调 ID。

# 格式错误的回调 URL 和不匹配的授权响应颁发者会失败。

# 如果发布发行者支持，则缺少元数据或响应发行者也会失败。

# 支持自定义回调路径。回调 URL 端口不选择

# 监听端口；配置 oauth.callback_port 或 mcp_oauth_callback_port。

# mcp_oauth_callback_url = "https://devbox.example.internal/callback"

################################################################################

# 项目文件控制

################################################################################

# 从 AGENTS.md 嵌入首轮指令的最大字节数。默认值：32768

project_doc_max_bytes = 32768

# 当目录级别缺少 AGENTS.md 时，有序回退。默认值：[]

project_doc_fallback_filenames = []

# 搜索父目录时使用的项目根标记文件名。默认值：[".git"]

# project_root_markers = [".git"]

################################################################################

# 历史记录和文件打开器

################################################################################

# 可点击引用的 URI 方案： vscode（默认） | vscode-insiders | 风帆 | 光标 | 无

file_opener = "vscode"

################################################################################

# UI、通知和其他

################################################################################

# 抑制输出中的内部推理事件。默认值：假

hide_agent_reasoning = false

# 显示可用的原始推理内容。默认值：假

show_raw_agent_reasoning = false

# 在 TUI 中禁用突发粘贴检测。默认值：假

disable_paste_burst = false

# 跟踪 Windows 登录确认（仅限 Windows）。默认值：假

windows_wsl_setup_acknowledged = false

# 启动时检查更新。默认值：true

check_for_update_on_startup = true

################################################################################

# 网页搜索

################################################################################

# Web 搜索模式：禁用 | 缓存 | 索引 | 实时。默认值：“缓存”

# 缓存提供来自网络搜索缓存的结果（OpenAI 维护的索引）。

# 缓存返回预先索引的结果；索引门外部网络访问通过

# 搜索索引； live 获取最新数据。

# 如果您使用 --yolo 或其他完全访问沙箱设置，则 Web 搜索默认为实时搜索。

web_search = "cached"

# 配置文件是 CODEX_HOME 下的单独文件。

# 示例：~/.codex/ci.config.toml，使用 codex --profile ci 选择。

# 抑制启用正在开发的功能标志时显示的警告。

# suppress_unstable_features_warning = true

################################################################################

# 智能体（多智能体角色和限制）

################################################################################

[agents]

# 启用或禁用多智能体工具。默认值：true

# enabled = true

# 最大并发打开生成智能体线程数，不包括主线程。未设置时，Codex 选择默认值。

# max_concurrent_threads_per_session = 6

# 生成智能体的默认模型。显式生成模型优先。

# default_subagent_model = "gpt-5.6-terra"

# 生成智能体的默认推理工作。明确的生成努力优先。

# default_subagent_reasoning_effort = "high"

# 当智能体轮流中断时记录模型可见的消息。默认值：true

# interrupt_message = true

# [agents.reviewer]

# description = "Find correctness, security, and test risks in code."

# config_file = "./agents/reviewer.toml" # relative to the config.toml that defines it

################################################################################

# 技能（每个技能覆盖）

################################################################################

# 可用技能目录的Token预算。默认值：模型上下文的 2%

# 窗口。显式值必须为正数，且上限为 10000 个标记。

# [skills]

# max_context_tokens = 2000

# 禁用或重新启用特定技能而不删除它。

[[skills.config]]

# path = "/path/to/skill/SKILL.md"

# enabled = false

################################################################################

# 沙箱设置（表）

################################################################################

# 仅当 sandbox_mode =“workspace-write”时使用额外设置。

[sandbox_workspace_write]

# 工作区之外的其他可写根 (cwd)。默认值：[]

writable_roots = []

# 允许沙箱内的出站网络访问。默认值：假

network_access = false

# 从可写根中排除 $TMPDIR。默认值：假

exclude_tmpdir_env_var = false

# 从可写根中排除 /tmp。默认值：假

exclude_slash_tmp = false

################################################################################

# 生成进程的 Shell 环境策略（表）

################################################################################

[shell_environment_policy]

# 继承：全部（默认） | 核心 | 无

inherit = "all"

# 跳过对包含 KEY/SECRET/TOKEN 的名称的自动过滤。默认值：true。

# 设置 false 以在应用显式过滤器之前删除这些变量。

ignore_default_excludes = false

# 显式键/值覆盖。包含过滤器仍然可以删除它们。默认值：{}

set = {}

# 实验性：通过用户 shell 配置文件运行。默认值：假

experimental_use_profile = false

# 规范的不区分大小写的过滤器。 “包含”条目创建一个白名单。

# 排除在显式设置值和包含允许列表之前应用。

# 不要将过滤器与旧版排除或

# 同一配置层中的 include_only 数组。

[shell_environment_policy.filters]

"AWS\_\*" = "exclude"

"AZURE\_\*" = "exclude"

################################################################################

# 沙箱网络设置

################################################################################

# 在配置沙箱网络规则之前启用该功能。

# 配置文件的network.enabled允许直接网络访问；它的域规则

# 仅在启用网络代理功能时适用。

# Web 搜索、应用程序、连接器和 MCP 服务器使用单独的控件。

# [features.network_proxy]

# enabled = true

# domains = { "api.openai.com" = "allow", "example.com" = "deny" }

#

# 精确的主机只匹配它们自己。

# “\*.example.com”仅匹配子域； “\*\*.example.com”匹配顶点加上子域。

# “\*”允许任何未被拒绝的公共主机，因此尽可能首选范围规则。

# `allow_local_binding = false` 默认阻止环回和专用目的地。

# 为一个目标添加精确的本地 IP 文字或 `localhost` 允许规则，或仅在需要更广泛的本地访问时将其设置为 true。

#

# 在启用此配置文件之前设置 `default_permissions = "workspace"`。

# 继承此配置文件的其他工作区根示例

# `:workspace_roots` 文件系统规则。

# [permissions.workspace.workspace_roots]

# “~/code/app”=真

# “~/code/shared-lib”=真

#

# 文件系统配置文件示例。使用 `"deny"` 拒绝读取确切路径或

# 全局模式。在需要预扩展全局匹配的平台上，设置

# 使用无界模式（例如 `\*\*`）时的 glob_scan_max_depth 。

# [permissions.workspace.filesystem]

# glob_scan_max_depth = 3

# “：workspace_roots”= {“。” = "写入", "\*\*/\*.env" = "拒绝" }

# “/absolute/path/to/secrets”=“拒绝”

#

# [permissions.workspace.network]

# enabled = true

# proxy_url = "http://127.0.0.1:43128"

# admin_url = "http://127.0.0.1:43129"

# enable_socks5 = false

# socks_url = "http://127.0.0.1:43130"

# enable_socks5_udp = false

# allow_upstream_proxy = false

# dangerously_allow_non_loopback_proxy = false

# dangerously_allow_non_loopback_admin = false

# dangerously_allow_all_unix_sockets = false

# mode = "limited" # limited | full

# allow_local_binding = false

#

# [permissions.workspace.network.domains]

# “api.openai.com”=“允许”

# “example.com”=“拒绝”

#

# [permissions.workspace.network.unix_sockets]

# “/var/run/docker.sock”=“允许”

################################################################################

# 历史（表）

################################################################################

[history]

# 全部保存（默认） | 无

persistence = "save-all"

# 历史文件的最大字节数；超过时，最旧的条目将被修剪。示例：5242880

# max_bytes = 5242880

################################################################################

# UI、通知和杂项（表）

################################################################################

[tui]

# 来自 TUI 的桌面通知：布尔值或过滤列表。默认值：true

# 示例： false | ["智能体完成"、"请求批准"]

notifications = false

# 终端警报通知机制：auto | osc9 | bel。默认值：“自动”

# notification_method = "auto"

# 当通知触发时：未聚焦（默认）| 始终

# notification_condition = "unfocused"

# 启用欢迎/状态/旋转动画。默认值：true

animations = true

# 在欢迎屏幕中显示入门工具提示。默认值：true

show_tooltips = true

# 控制备用屏幕的使用（在 Zellij 中自动跳过它以保留回滚）。

# alternate_screen = "auto"

# 恢复或分叉会话的工作目录：当前 | 会话。

# 当当前和保存的会话目录不同时，保持未设置即可选择。

# resume_cwd = "session"

# 页脚状态行项目 ID 的有序列表。未设置时，Codex 使用：

# ["model-with-reasoning", "context-remaining", "current-dir"].

# 设置为 [] 以隐藏页脚。

# status_line = ["model", "context-remaining", "git-branch"]

# 终端窗口/选项卡标题项 ID 的有序列表。未设置时，Codex 使用：

# ["spinner", "project"]. Set to [] to clear the title.

# 可用 ID 包括应用程序名称、项目、微调器、状态、线程、git-branch、模型、

# 和任务进度。

# terminal_title = ["spinner", "project"]

# 语法突出显示主题（kebab-case）。在TUI中使用/theme进行预览和保存。

# 您还可以在 $CODEX_HOME/themes 下添加自定义 .tmTheme 文件。

# theme = "catppuccin-mocha"

# 自定义键绑定。选定的输入框操作会回退到匹配的 [tui.keymap.global] 绑定。

# 使用 [] 解除绑定操作。

# [tui.keymap.global]

# open_transcript = "ctrl-t"

# open_external_editor = []

#

# [tui.keymap.composer]

# submit = ["enter", "ctrl-m"]

# [tui.keymap.chat]

# interrupt_turn = "f12"

# 由模型块键入的内部工具提示状态。通常由 Codex 管理。

# [tui.model_availability_nux]

# “gpt-5.6-terra”= 1

# 启用或禁用该计算机的分析。未设置时，Codex 使用其默认行为。

[analytics]
enabled = true

# 控制用户是否可以从`/feedback`提交反馈。默认值：true

[feedback]
enabled = true

# 产品内通知（大部分由 Codex 自动设置）。

[notice]

# hide_full_access_warning = true

# hide_world_writable_warning = true

# hide_rate_limit_model_nudge = true

# hide_gpt5_1_migration_prompt = true

# “hide_gpt-5.1-codex-max_migration_prompt”= true

# model_migrations = { "gpt-5.4" = "gpt-5.6-terra" }

################################################################################

# 集中式功能标志（首选）

################################################################################

[features]

# 将此表留空以接受默认值。设置显式布尔值以选择加入/退出。

# shell_tool = true

# apps = true

# hooks = false

# unified_exec = true

# shell_snapshot = true

# multi_agent = true

# remote_plugin = true

# personality = true

# network_proxy = true # required to enforce permission-profile domain rules

# fast_mode = true

# enable_request_compression = true

# skill_mcp_dependency_install = true

# prevent_idle_sleep = false

# 代码模式命名空间。此功能正在开发中，默认情况下处于关闭状态。

# [features.code_mode]

# enabled = true

# excluded_tool_namespaces = ["mcp__codex_apps"]

# direct_only_tool_namespaces = ["mcp__history"]

# 推出预算跟踪。此功能正在开发中，默认情况下处于关闭状态。

# 启用时需要 limit_tokens。

# 可选的reminder_interval_tokens默认为limit_tokens的10%。

# 令牌权重默认为 1.0。

# [features.rollout_budget]

# enabled = true

# limit_tokens = 100000

# reminder_interval_tokens = 10000

# sampling_token_weight = 1.0

# prefill_token_weight = 1.0

################################################################################

# 回忆（表）

################################################################################

# 使用 [features].memories 启用内存，然后在此处调整内存行为。

# [memories]

# generate_memories = true

# use_memories = true

# disable_on_external_context = false # legacy alias: no_memories_if_mcp_or_web_search

################################################################################

# 生命周期钩子可以在这里内联配置，也可以在同级 hooks.json 中配置。

################################################################################

# [hooks]

# [[hooks.PreToolUse]]

# matcher = "^Bash$"

#

# [[hooks.PreToolUse.hooks]]

# type = "command"

# command = 'python3 "/absolute/path/to/pre_tool_use_policy.py"'

# timeout = 30

# statusMessage = "Checking Bash command"

################################################################################

# 在此表下定义 MCP 服务器。留空以禁用。

################################################################################

[mcp_servers]

# --- 示例：STDIO 传输 ---

# [mcp_servers.docs]

# enabled = true # optional; default true

# required = true # optional; fail startup/resume if this server cannot initialize

# command = "docs-server" # required

# args = ["--port", "4000"] # optional

# env = { "API_KEY" = "value" } # optional key/value pairs copied as-is

# env_vars = ["ANOTHER_SECRET"] # optional: forward local parent env vars

# env_vars = ["LOCAL_TOKEN", { name = "REMOTE_TOKEN", source = "remote" }]

# cwd = "/path/to/server" # optional working directory override

# experimental_environment = "remote" # experimental: run stdio via a remote executor

# startup_timeout_sec = 10.0 # optional; default 10.0 seconds

# #startup_timeout_ms = 10000#启动超时的可选别名（毫秒）

# tool_timeout_sec = 60.0 # optional; default 60.0 seconds

# enabled_tools = ["search", "summarize"] # optional allow-list

# disabled_tools = ["slow-tool"] # optional deny-list (applied after allow-list)

# scopes = ["read:docs"] # optional OAuth scopes

# oauth_resource = "https://docs.example.com/" # optional OAuth resource

# [mcp_servers.docs.tools.search]

# output_token_limit = 30000 # positive token budget, before the 20% serialization allowance

# --- 示例：流式 HTTP 传输 ---

# [mcp_servers.github]

# enabled = true # optional; default true

# required = true # optional; fail startup/resume if this server cannot initialize

# url = "https://github-mcp.example.com/mcp" # required

# bearer_token_env_var = "GITHUB_TOKEN" # optional; Authorization: Bearer <token>

# http_headers = { "X-Example" = "value" } # optional static headers

# env_http_headers = { "X-Auth" = "AUTH_ENV" } # optional headers populated from env vars

# http_headers_helper = "company-auth mcp-headers" # local command that prints a JSON header map

# startup_timeout_sec = 10.0 # optional

# tool_timeout_sec = 60.0 # optional

# enabled_tools = ["list_issues"] # optional allow-list

# disabled_tools = ["delete_issue"] # optional deny-list

# scopes = ["repo"] # optional OAuth scopes

# [mcp_servers.github.oauth]

# client_id = "my-pre-registered-client" # OAuth client registered with the provider

# callback_url = "http://127.0.0.1/callback" # saved registered callback for this client

# callback_port = 4321 # server-specific listener port; overrides the global setting

# 对于http://127.0.0.1:4321/callback,，也设置callback_port = 4321。

################################################################################

# 模型提供者

################################################################################

# Built-ins include:

# - 开放伊

# - ollama

# - 工作室

# - amazon-bedrock

# These IDs are reserved.为自定义提供商使用不同的 ID。

[model_providers]

# --- 示例：内置 Amazon Bedrock 提供商选项 ---

# model_provider = "amazon-bedrock"

# model = "<bedrock-model-id>"

# [model_providers.amazon-bedrock.aws]

# profile = "default"

# region = "eu-central-1"

# --- 示例：具有显式基本 URL 或标头的 OpenAI 数据驻留 ---

# [model_providers.openaidr]

# name = "OpenAI Data Residency"

# base_url = "https://us.api.openai.com/v1" # example with 'us' domain prefix

# wire_api = "responses" # only supported value

# # require_openai_auth = true # 仅用于 OpenAI auth 支持的提供者

# # request_max_retries = 4 # 默认 4; max 100

# #stream_max_retries = 5 #默认5; max 100

# #stream_idle_timeout_ms = 300000 #默认300_000（5m）

# # support_websockets = true # 可选

# # support_standalone_web_search = true # 可选;搜索正在开发中，默认关闭

# # Experimental_bearer_token = "sk-example" # 可选的仅用于开发的直接不记名令牌

# # http_headers = { "X-示例" = "值" }

# # env_http_headers = { "OpenAI-组织" = "OPENAI_ORGANIZATION", "OpenAI-项目" = "OPENAI_PROJECT" }

# --- 示例：Azure/OpenAI 兼容提供程序 ---

# [model_providers.azure]

# name = "Azure"

# base_url = "https://YOUR_PROJECT_NAME.openai.azure.com/openai"

# wire_api = "responses"

# query_params = { api-version = "2025-04-01-preview" }

# env_key = "AZURE_OPENAI_API_KEY"

# env_key_instructions = "Set AZURE_OPENAI_API_KEY in your environment"

# #supports_websockets = false

# --- 示例：命令支持的不记名令牌身份验证 ---

# [model_providers.proxy]

# name = "OpenAI using LLM proxy"

# base_url = "https://proxy.example.com/v1"

# wire_api = "responses"

#

# [model_providers.proxy.auth]

# command = "/usr/local/bin/fetch-codex-token"

# args = ["--audience", "codex"]

# timeout_ms = 5000

# refresh_interval_ms = 300000

# --- 示例：本地 OSS（例如 Ollama 兼容）---

# [model_providers.local_ollama]

# name = "Ollama"

# base_url = "http://localhost:11434/v1"

# wire_api = "responses"

################################################################################

# 应用程序/连接器

################################################################################

# 可选的每个应用程序控件。

[apps]

# [_default] applies to all apps unless overridden per app.

# [apps._default]

# enabled = true

# destructive_enabled = true

# open_world_enabled = true

# approvals_reviewer = "user" # user | auto_review

# default_tools_approval_mode = "auto" # auto | prompt | writes | approve

#

# [apps.google_drive]

# enabled = false

# destructive_enabled = false # block destructive-hint tools for this app

# default_tools_enabled = true

# approvals_reviewer = "auto_review"

# default_tools_approval_mode = "prompt" # auto | prompt | writes | approve

#

# [apps.google_drive.tools."files/delete"]

# enabled = false

# approval_mode = "approve"

# Codex 可以提供安装连接器或插件的可选工具建议白名单。

# [tool_suggest]

# discoverables = [

# { type = "connector", id = "gmail" },

# { type = "plugin", id = "figma@openai-curated" },

# ]

# disabled_tools = [

# { type = "plugin", id = "slack@openai-curated" },

# { type = "connector", id = "connector_googlecalendar" },

# ]

################################################################################

# 配置配置文件（单独的文件）

################################################################################

# 要创建配置文件，请将覆盖放在 $CODEX_HOME 下的单独配置文件中。

# 使用 codex --profile ci 选择它。

# 例如，CI 配置文件可以位于 $CODEX_HOME/ci.config.toml：

# model = "gpt-5.6-terra"

# approval_policy = "on-request"

# sandbox_mode = "read-only"

# service_tier = "fast" # or another supported service tier id

# oss_provider = "ollama"

# model_reasoning_effort = "medium"

# plan_mode_reasoning_effort = "high"

# model_reasoning_summary = "auto"

# model_verbosity = "medium"

# personality = "pragmatic" # or "friendly" or "none"

# chatgpt_base_url = "https://chatgpt.com/backend-api/"

# model_catalog_json = "./models.json"

# model_instructions_file = "/absolute/or/relative/path/to/instructions.txt"

# experimental_compact_prompt_file = "./compact_prompt.txt"

# tools_view_image = true

# features = { unified_exec = false }

################################################################################

# 项目（信任级别）

################################################################################

[projects]

# 将特定工作树标记为可信或不可信。

# [projects."/absolute/path/to/project"]

# trust_level = "trusted" # or "untrusted"

################################################################################

# 工具

################################################################################

[tools]

# view_image = true

################################################################################

# OpenTelemetry (OTEL) - 默认禁用

################################################################################

[otel]

# 在日志中包含用户提示文本。默认值：假

log_user_prompt = false

# 适用于遥测的环境标签。默认值：“开发”

environment = "dev"

# 导出器：无（默认） | otlp-http | otlp-grpc

exporter = "none"

# 跟踪导出器：无（默认） | otlp-http | otlp-grpc

trace_exporter = "none"

# 指标导出器：无 | statsig | otlp-http | otlp-grpc

metrics_exporter = "statsig"

# OTLP/HTTP 导出器配置示例

# [otel.exporter."otlp-http"]

# endpoint = "https://otel.example.com/v1/logs"

# protocol = "binary" # "binary" | "json"

# [otel.exporter."otlp-http".headers]

# “x-otlp-api-key”=“${OTLP_TOKEN}”

# [otel.exporter."otlp-http".tls]

# ca-certificate = "certs/otel-ca.pem"

# client-certificate = "/etc/codex/certs/client.pem"

# client-private-key = "/etc/codex/certs/client-key.pem"

# OTLP/gRPC 跟踪导出器配置示例

# [otel.trace_exporter."otlp-grpc"]

# endpoint = "https://otel.example.com:4317"

# headers = { "x-otlp-meta" = "abc123" }

################################################################################

# 窗户

################################################################################

[windows]

# 本机 Windows 沙箱模式（仅限 Windows）：未提升的 | 已提升

sandbox = "unelevated"
```