> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/config-file/config-advanced.md)。

<a id="advanced-configuration"></a>

# 高级配置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

当您需要对提供商、策略和集成进行更多控制时，请使用这些选项。如需快速入门，请参阅 [配置基础知识](config-basic.zh-CN.md)。

有关项目指导、可重用功能、自定义斜杠命令、子智能体工作流程和集成的背景信息，请参阅 [定制化](../customization/overview.zh-CN.md)。有关配置密钥，请参阅 [配置参考](config-reference.zh-CN.md)。

<a id="profiles"></a>

## 型材

配置文件允许您保存命名的配置层并通过 CLI 在它们之间进行切换。当传递`--profile profile-name`时，Codex加载`~/.codex/config.toml`，然后覆盖`~/.codex/profile-name.config.toml`。配置文件名称可以包含字母、数字、连字符和下划线。

为每个配置文件创建一个单独的 TOML 文件。使用配置文件中的顶级配置键；不要将它们嵌套在 `[profiles.profile-name]` 下。

```toml
# 〜/.codex/deep-review.config.toml
model = "gpt-5.5"
model_reasoning_effort = "xhigh"
approval_policy = "on-request"
model_catalog_json = "/Users/me/.codex/model-catalogs/deep-review.json"
```

```shell
codex --profile deep-review
codex exec --profile deep-review "review this change"
```

由于配置文件位于基本用户配置之上、项目和 CLI 配置之下，因此它只需要与基本配置不同的值。配置文件还可以覆盖`model_catalog_json`；当两个文件都设置它时，Codex 使用配置文件值。

在Codex 0.134.0及更高版本中，`--profile`不再从`config.toml`读取`[profiles.profile-name]`，并且不再支持顶级`profile = "profile-name"`选择器。将旧配置文件设置移至 `~/.codex/profile-name.config.toml`，然后从 `config.toml` 中删除匹配的 `[profiles.profile-name]` 表和 `profile = "profile-name"` 选择器。

<a id="one-off-overrides-from-the-cli"></a>

## 从 CLI 一次性覆盖

除了编辑 `~/.codex/config.toml` 之外，您还可以从 CLI 覆盖单次运行的配置：

- 首选专用标志（如果存在）（例如，`--model`）。
- 当您需要覆盖任意键时，请使用 `-c` / `--config`。

示例：

```shell
# 专用旗帜
codex --model gpt-5.6-terra

# 通用键/值覆盖（值是 TOML，而不是 JSON）
codex --config model='"gpt-5.6-terra"'
codex --config sandbox_workspace_write.network_access=true
codex --config 'shell_environment_policy.include_only=["PATH","HOME"]'
```

注意事项：

- 键可以使用点表示法来设置嵌套值（例如，`mcp_servers.context7.enabled=false`）。
- `--config` 值被解析为 TOML。如有疑问，请引用该值，以便 shell 不会将其分割为空格。
- 如果该值无法解析为 TOML，则 Codex 会将其视为字符串。

<a id="config-and-state-locations"></a>

## 配置和状态位置

Codex 将其本地状态存储在 `CODEX_HOME` 下（默认为 `~/.codex`）。

您可能会在那里看到常见文件：

- `config.toml`（您的本地配置）
- `auth.json`（如果您使用基于文件的凭证存储）或您的操作系统钥匙串/钥匙圈
- `history.jsonl`（如果启用历史记录持久化）
- 其他每用户状态，例如日志和缓存

有关身份验证详细信息（包括凭证存储模式），请参阅 [认证](../auth.zh-CN.md)。有关配置密钥的完整列表，请参阅 [配置参考](config-reference.zh-CN.md)。

有关签入仓库或系统路径的共享默认值、规则和技能，请参阅 [团队配置](../enterprise/admin-setup.zh-CN.md#step-4-standardize-local-configuration-with-team-config)。

如果您只需将内置 OpenAI 提供程序指向 LLM 代理、路由器或启用数据驻留的项目，请在 `config.toml` 中设置 `openai_base_url`，而不是定义新的提供程序。这会更改内置 `openai` 提供程序的基本 URL，而无需单独的“model_providers”。<id>` 条目。

```toml
openai_base_url = "https://us.api.openai.com/v1"
```

<a id="project-config-files-codexconfigtoml"></a>

## 项目配置文件（`.codex/config.toml`）

除了您的用户配置之外，Codex 还会从仓库内的 `.codex/config.toml` 文件中读取项目范围的覆盖。 Codex 从项目根目录走到当前工作目录并加载它找到的每个 `.codex/config.toml`。如果多个文件定义相同的密钥，则最接近您的工作目录的文件获胜。

为了安全起见，Codex 仅当项目受信任时才加载项目范围的配置文件。如果项目不受信任，Codex 会忽略项目 `.codex/` 层，包括 `.codex/config.toml`、项目本地挂钩和项目本地规则。用户层和系统层保持分离并且仍然负载。

项目配置内的相对路径（例如，`model_instructions_file`）是相对于包含 `config.toml` 的 `.codex/` 文件夹进行解析的。

项目配置文件无法覆盖重定向凭据、更改主机拥有的应用程序请求元数据、更改提供程序身份验证、选择配置文件或运行计算机本地通知/遥测命令的设置。 Codex 会忽略项目本地 `.codex/config.toml` 中的以下键，并在看到它们时打印启动警告：`openai_base_url`、`chatgpt_base_url`、`apps_mcp_product_sku`、`model_provider`、`model_providers`、`notify`、 `profile`、`profiles`、`experimental_realtime_ws_base_url` 和 `otel`。在您的用户级别 `~/.codex/config.toml` 中设置提供商、通知和遥测密钥；选择带有 `--profile profile-name` 和 `~/.codex/profile-name.config.toml` 的配置文件。

<a id="hooks"></a>

## 挂钩

Codex 还可以从 `hooks.json` 文件或位于活动配置层旁边的 `config.toml` 文件中的内联 `[hooks]` 表加载生命周期挂钩。

实际上，四个最有用的位置是：

- `~/.codex/hooks.json`
- `~/.codex/config.toml`
- `<repo>/.codex/hooks.json`
- `<repo>/.codex/config.toml`

仅当项目 `.codex/` 层受信任时，才会加载项目本地挂钩。用户级挂钩保持独立于项目信任。

内联 TOML 挂钩使用与 `hooks.json` 相同的事件结构：

```toml
[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
```

如果单个图层同时包含 `hooks.json` 和内联 `[hooks]`，则 Codex 会加载两者并发出警告。更喜欢每层一个表示。

有关当前事件列表、输入字段、输出行为和限制，请参阅 [挂钩](../hooks.zh-CN.md)。

<a id="agent-roles-agents-in-configtoml"></a>

## 智能体角色（`config.toml` 中的 `[agents]`）

有关子智能体角色配置（`config.toml` 中的 `[agents]`），请参阅 [子智能体](../agent-configuration/subagents.zh-CN.md)。

<a id="project-root-detection"></a>

## 项目根检测

Codex 通过从工作目录向上直至到达项目根来发现项目配置（例如，`.codex/` 层和 `AGENTS.md`）。

默认情况下，Codex 将包含 `.git` 的目录视为项目根目录。要自定义此行为，请在 `config.toml` 中设置 `project_root_markers`：

```toml
# 当目录包含任何这些标记时，将其视为项目根目录。
project_root_markers = [".git", ".hg", ".sl"]
```

设置 `project_root_markers = []` 跳过搜索父目录并将当前工作目录视为项目根目录。

<a id="custom-model-providers"></a>

## 定制模型提供商

模型提供程序定义 Codex 如何连接到模型（基本 URL、wire API、身份验证和可选的 HTTP 标头）。自定义提供程序无法重复使用保留的内置提供程序 ID：`openai`、`ollama` 和 `lmstudio`。

定义其他提供程序并将 `model_provider` 指向它们：

```toml
model = "gpt-5.6-terra"
model_provider = "proxy"

[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "http://proxy.example.com"
env_key = "OPENAI_API_KEY"

[model_providers.local_ollama]
name = "Ollama"
base_url = "http://localhost:11434/v1"

[model_providers.mistral]
name = "Mistral"
base_url = "https://api.mistral.ai/v1"
env_key = "MISTRAL_API_KEY"
```

如果自定义提供程序支持独立 Web 搜索端点，请在其提供程序配置中宣传该功能：

```toml
[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "https://proxy.example.com/v1"
env_key = "OPENAI_API_KEY"
supports_standalone_web_search = true
```

对于自定义提供程序，该设置默认为 `false`。独立网络搜索正在开发中，默认情况下处于关闭状态。将提供程序功能设置为 `true` 不会启用它：提供程序必须支持兼容的端点，并且所选模型和运行时必须支持独立搜索。配置的 [`web_search`模式](../web-search.zh-CN.md) 和托管搜索限制仍然适用。

需要时添加请求标头：

```toml
[model_providers.example]
http_headers = { "X-Example-Header" = "example-value" }
env_http_headers = { "X-Example-Features" = "EXAMPLE_FEATURES" }
```

当提供商需要 Codex 从外部凭证助手获取不记名令牌时，请使用命令支持的身份验证：

```toml
[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "https://proxy.example.com/v1"
wire_api = "responses"

[model_providers.proxy.auth]
command = "/usr/local/bin/fetch-codex-token"
args = ["--audience", "codex"]
timeout_ms = 5000
refresh_interval_ms = 300000
```

auth 命令未接收到 `stdin`，并且必须将令牌打印到标准输出。 Codex 修剪周围的空白，将空标记视为错误，并在 `refresh_interval_ms` 处主动刷新；将 `refresh_interval_ms = 0` 设置为仅在身份验证重试后刷新。不要组合`[model_providers。<id>.auth]` with `env_key`, `experimental_bearer_token`, or `requires_openai_auth`。

<a id="amazon-bedrock-provider"></a>

### 亚马逊基岩提供商

Codex 包含一个内置的 `amazon-bedrock` 模型提供程序。直接设置为`model_provider`；与自定义提供程序不同，此内置提供程序仅支持嵌套的 AWS 配置文件和区域覆盖。

```toml
model_provider = "amazon-bedrock"
model = "<bedrock-model-id>"

[model_providers.amazon-bedrock.aws]
profile = "default"
region = "eu-central-1"
```

如果省略 `profile`，Codex 将使用标准 AWS 凭证链。将 `region` 设置为应处理请求的受支持的基岩区域。

有关完整的设置流程、身份验证选项、支持的模型和功能可用性，请参阅 [将 ChatGPT Work 和 Codex 与 Amazon Bedrock 结合使用](../amazon-bedrock.zh-CN.md)。

<a id="oss-mode-local-providers"></a>

## OSS模式（本地提供商）

当您通过 `--oss` 时，Codex 可以针对本地“开源”提供商（例如 Ollama 或 LM Studio）运行。选择一个用于 `--local-provider` 的单次运行，或将 `oss_provider` 设置为默认值。如果两者均未设置，交互式 CLI 会提示您进行选择； `codex exec` 退出并出现错误。

```toml
# 与 `--oss` 一起使用的默认本地提供商
oss_provider = "ollama" # or "lmstudio"
```

<a id="azure-provider-and-per-provider-tuning"></a>

## Azure 提供商和每个提供商的调整

```toml
[model_providers.azure]
name = "Azure"
base_url = "https://YOUR_PROJECT_NAME.openai.azure.com/openai"
env_key = "AZURE_OPENAI_API_KEY"
query_params = { api-version = "2025-04-01-preview" }
wire_api = "responses"
request_max_retries = 4
stream_max_retries = 10
stream_idle_timeout_ms = 300000
```

要更改内置 OpenAI 提供程序的基本 URL，请使用 `openai_base_url`；不要创建 `[model_providers.openai]`，因为您无法覆盖内置提供程序 ID。

<a id="api-organizations-using-data-residency"></a>

## 使用数据驻留的 API 组织

启用 [数据驻留](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt) 创建的项目可以创建模型提供程序以使用 [正确的前缀](https://developers.openai.com/api/docs/guides/your-data#which-models-and-features-are-eligible-for-data-residency) 更新 `base_url`。对于具有数据驻留的 ChatGPT 工作区，不需要自定义提供程序；当您使用 ChatGPT 登录时，Codex 会遵守工作区驻留设置。

```toml
model_provider = "openaidr"
[model_providers.openaidr]
name = "OpenAI Data Residency"
base_url = "https://us.api.openai.com/v1" # Replace 'us' with domain prefix
```

<a id="model-reasoning-verbosity-and-limits"></a>

## 模型推理、冗长和限制

```toml
model_reasoning_summary = "none"          # Disable summaries
model_verbosity = "low"                   # Shorten responses
model_supports_reasoning_summaries = true # Force reasoning
model_context_window = 128000             # Context window size
```

`model_verbosity` 仅适用于使用响应 API 的提供商。聊天完成提供商将忽略该设置。

<a id="approval-policies-and-sandbox-modes"></a>

## 审批策略和沙箱模式

选择批准严格性（影响 Codex 暂停时）和沙箱级别（影响文件/网络访问）。

有关编辑 `config.toml` 时要记住的操作细节，请参阅 [常见的沙箱和审批组合](../agent-approvals-security.zh-CN.md#common-sandbox-and-approval-combinations)、[可写根中的受保护路径](../agent-approvals-security.zh-CN.md#protected-paths-in-writable-roots) 和 [网络接入](../agent-approvals-security.zh-CN.md#network-access)。

Codex 和 ChatGPT Work 不再支持 `approval_policy = "untrusted"`。请参阅 [从已停用的 `untrusted` 审批策略迁移](../agent-approvals-security.zh-CN.md#migrate-from-the-retired-untrusted-approval-policy) 了解支持的设置和更严格的项目派生审批。

有关同时配置文件系统和网络访问的 beta 权限配置文件，请参阅 [权限](../permissions.zh-CN.md)。

您还可以使用精细审批策略 (`approval_policy = { granular = { ... } }`) 来允许或自动拒绝单个提示类别。当您希望对某些情况进行正常的交互式批准，但希望其他情况（例如 `request_permissions` 或技能脚本提示）无法自动关闭时，这非常有用。

设置 `approvals_reviewer = "auto_review"` 以通过自动审核路由符合条件的交互式审批请求。这改变了审阅者，而不是沙箱边界。

使用 `[auto_review].policy` 获取本地审阅者策略说明。受管理的 `guardian_policy_config` 优先。

```toml
approval_policy = "on-request"  # Other options: never or { granular = { ... } }
approvals_reviewer = "user"     # Or "auto_review" for automatic review
sandbox_mode = "workspace-write"
allow_login_shell = false       # Optional hardening: disallow login shells for shell tools

# 细粒度审批策略示例：
# approval_policy = { granular = {
#   sandbox_approval = true,
#   rules = true,
#   mcp_elicitations = true,
#   request_permissions = false,
#   skill_approval = false
# } }

[sandbox_workspace_write]
exclude_tmpdir_env_var = false  # Allow $TMPDIR
exclude_slash_tmp = false       # Allow /tmp
writable_roots = ["/Users/YOU/.pyenv/shims"]
network_access = false          # Opt in to outbound network

[auto_review]
policy = """
Use your organization's automatic review policy.
"""
```

<a id="named-permission-profiles"></a>

### 命名权限配置文件

有关内置配置文件、自定义配置文件语法以及完整的文件系统和网络配置模型，请参阅 [权限](../permissions.zh-CN.md)。

有关完整的密钥列表和要求约束，请参阅 [配置参考](config-reference.zh-CN.md) 和 [受管配置](../enterprise/managed-configuration.zh-CN.md)。

在工作区写入模式下，某些环境使 `.git/` 和 `.codex/` 保持只读状态，即使工作区的其余部分可写也是如此。这就是为什么像 `git commit` 这样的命令可能仍然需要批准才能在沙箱之外运行。如果您希望 Codex 跳过特定命令（例如，将 `git commit` 阻止在沙箱之外），请使用 [规则](../agent-configuration/rules.zh-CN.md)。

完全禁用沙箱（仅当您的环境已经隔离进程时才使用）：

```toml
sandbox_mode = "danger-full-access"
```

<a id="shell-environment-policy"></a>

## Shell环境政策

`shell_environment_policy` 控制 Codex 传递给生成命令的环境变量。使用 `inherit = "none"` 从空环境开始，或使用 `inherit = "core"` 继承修剪集。添加显式值和键控过滤器，以避免将不必要的秘密传递给生成的命令。

```toml
[shell_environment_policy]
inherit = "core"
set = { MY_FLAG = "1" }
ignore_default_excludes = false

[shell_environment_policy.filters]
"AWS_*" = "exclude"
"AZURE_*" = "exclude"
```

过滤器模式不区分大小写，并支持 `*` 和 `?`。使用 `"exclude"` 删除匹配的变量。当任何模式使用 `"include"` 时，Codex 仅保留与包含模式匹配的变量。包含不会恢复已排除的变量。过滤器键跨配置层合并，不区分大小写。

`ignore_default_excludes` 默认为 `true`，因此 Codex 不会自动删除包含 `KEY`、`SECRET` 或 `TOKEN` 的变量名称。将其设置为 `false` 以在显式过滤器运行之前应用这些自动排除。

Codex 首先应用自动排除，然后应用自定义排除、`set` 中的值，最后应用包含模式白名单。由于 `set` 在排除之后运行，因此它可以恢复排除的变量。包含模式允许列表仍然可以删除该恢复的值。

现有配置仍支持较旧的 `exclude` 和 `include_only` 阵列。不要将任一阵列与 `[shell_environment_policy.filters]` 组合在同一配置层中； Codex 拒绝该组合。

<a id="mcp-servers"></a>

## MCP服务器

配置详情请参见专用[MCP 文档](../extend/mcp.zh-CN.md)。

<a id="observability-and-telemetry"></a>

## 可观测性和遥测

启用 OpenTelemetry (OTel) 日志导出以跟踪 Codex 运行（API 请求、SSE/事件、提示、工具批准/结果）。默认禁用；通过 `[otel]` 选择加入：

```toml
[otel]
environment = "staging"   # defaults to "dev"
exporter = "none"         # set to otlp-http or otlp-grpc to send events
log_user_prompt = false   # redact user prompts unless explicitly enabled
```

选择出口商：

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

如果 `exporter = "none"` Codex 记录事件但不发送任何内容。导出器异步批处理并在关闭时刷新。事件元数据包括服务名称、CLI 版本、环境标签、对话 ID、模型、沙箱/批准设置和每个事件字段（请参阅 [配置参考](config-reference.zh-CN.md)）。

<a id="what-gets-emitted"></a>

### 发出什么

Codex 发出运行和工具使用情况的结构化日志事件。代表性事件类型包括：

- `codex.conversation_starts`（模型、推理设置、沙箱/审批策略）
- `codex.api_request`（尝试、状态/成功、持续时间和错误详细信息）
- `codex.sse_event`（流事件类型、成功/失败、持续时间以及 `response.completed` 上的令牌计数）
- `codex.websocket_request` 和 `codex.websocket_event`（请求持续时间加上每条消息的类型/成功/错误）
- `codex.user_prompt`（长度；内容经过编辑，除非明确启用）
- `codex.tool_decision`（批准/拒绝以及决定是否来自配置与用户）
- `codex.tool_result`（持续时间、成功、输出片段）

<a id="otel-metrics-emitted"></a>

### 发布的 OTel 指标

启用 OTel 指标管道后，Codex 会发出 API、流和工具活动的计数器和持续时间直方图。

下面的每个指标还包括默认元数据标签：`auth_mode`、`originator`、`session_source`、`model` 和 `app.version`。

| 公制 | 类型 | 字段 | 描述 |
| ------------------------------------- | --------- | ------------------- | ----------------------------------------------------------------- |
| `codex.api_request` | 计数器 | `status`、`success` | 按 HTTP 状态和成功/失败划分的 API 请求计数。             |
| `codex.api_request.duration_ms` | 直方图 | `status`、`success` | API 请求持续时间（以毫秒为单位）。                             |
| `codex.sse_event` | 计数器 | `kind`、`success` | SSE 事件计数（按事件类型和成功/失败）。                |
| `codex.sse_event.duration_ms` | 直方图 | `kind`、`success` | SSE 事件处理持续时间（以毫秒为单位）。                    |
| `codex.websocket.request` | 计数器 | `success` | WebSocket 请求按成功/失败计数。                       |
| `codex.websocket.request.duration_ms` | 直方图 | `success` | WebSocket 请求持续时间（以毫秒为单位）。                       |
| `codex.websocket.event` | 计数器 | `kind`、`success` | WebSocket 消息/事件计数（按类型和成功/失败）。        |
| `codex.websocket.event.duration_ms` | 直方图 | `kind`、`success` | WebSocket 消息/事件处理持续时间（以毫秒为单位）。      |
| `codex.tool.call` | 计数器 | `tool`、`success` | 按工具名称和成功/失败的工具调用计数。           |
| `codex.tool.call.duration_ms` | 直方图 | `tool`、`success` | 按工具名称和结果显示的工具执行持续时间（以毫秒为单位）。 |

有关遥测的更多安全和隐私指南，请参阅 [安全性](../agent-approvals-security.zh-CN.md#monitoring-and-telemetry)。

<a id="metrics"></a>

### 指标

默认情况下，Codex 定期将少量匿名使用情况和健康数据发送回 OpenAI。这有助于检测 Codex 何时无法正常工作，并显示正在使用哪些功能和配置选项，以便 Codex 团队可以专注于最重要的事情。这些指标不包含任何个人身份信息 (PII)。指标收集独立于 OTel 日志/跟踪导出。

如果您想在计算机上的 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中完全禁用指标收集，请在配置中设置分析标志：

```toml
[analytics]
enabled = false
```

每个指标都包含其自己的字段以及下面的默认上下文字段。

<a id="default-context-fields-applies-to-every-eventmetric"></a>

#### 默认上下文字段（适用于每个事件/指标）

- `auth_mode`：`swic` | `api` | `unknown`。
- `model`：所用模型的名称。
- `app.version`：Codex 版本。

<a id="metrics-catalog"></a>

#### 指标目录

每个指标都包含必填字段以及上面的默认上下文字段。下面的指标名称省略了 `codex.` 前缀。大多数指标名称集中在`codex-rs/otel/src/metrics/names.rs`中；该文件外部发出的特定于功能的指标也包含在此处。如果指标包含 `tool` 字段，则它反映所使用的内部工具（例如，`apply_patch` 或 `shell`），并且不包含 `codex` 尝试应用的实际 shell 命令或补丁。

<a id="runtime-and-model-transport"></a>

#### 运行时和模型传输

| 公制 | 类型 | 字段 | 说明 |
| ----------------------------------------------- | --------- | -------------------- | ------------------------------------------------------------ |
| `api_request` | 计数器 | `status`、`success` | 按 HTTP 状态和成功/失败划分的 API 请求计数。        |
| `api_request.duration_ms` | 直方图 | `status`、`success` | API 请求持续时间（以毫秒为单位）。                        |
| `sse_event` | 计数器 | `kind`、`success` | SSE 事件计数（按事件类型和成功/失败）。           |
| `sse_event.duration_ms` | 直方图 | `kind`、`success` | SSE 事件处理持续时间（以毫秒为单位）。               |
| `websocket.request` | 计数器 | `success` | WebSocket 请求按成功/失败计数。                  |
| `websocket.request.duration_ms` | 直方图 | `success` | WebSocket 请求持续时间（以毫秒为单位）。                  |
| `websocket.event` | 计数器 | `kind`、`success` | WebSocket 消息/事件计数（按类型和成功/失败）。   |
| `websocket.event.duration_ms` | 直方图 | `kind`、`success` | WebSocket 消息/事件处理持续时间（以毫秒为单位）。 |
| `responses_api_overhead.duration_ms` | 直方图 | | 响应 WebSocket 响应的 API 开销计时。      |
| `responses_api_inference_time.duration_ms` | 直方图 | | 响应 API 从 WebSocket 响应推断时序。     |
| `responses_api_engine_iapi_ttft.duration_ms` | 直方图 | | 响应 API 引擎 IAPI 时间到第一个令牌计时。        |
| `responses_api_engine_service_ttft.duration_ms` | 直方图 | | 响应 API 引擎服务时间到第一个令牌计时。     |
| `responses_api_engine_iapi_tbt.duration_ms` | 直方图 | | 响应 API 引擎 IAPI 令牌间时间计时。         |
| `responses_api_engine_service_tbt.duration_ms` | 直方图 | | 响应 API 引擎服务令牌间隔时间计时。      |
| `transport.fallback_to_http` | 计数器 | `from_wire_api` | WebSocket 到 HTTP 的回退计数。                            |
| `remote_models.fetch_update.duration_ms` | 直方图 | | 获取远程模型定义的时间。                      |
| `remote_models.load_cache.duration_ms` | 直方图 | | 加载远程模型缓存的时间。                         |
| `startup_prewarm.duration_ms` | 直方图 | `status` | 按结果列出的启动预热持续时间。                         |
| `startup_prewarm.age_at_first_turn_ms` | 直方图 | `status` | 当第一个真正的回合解决它时启动预热年龄。    |
| `cloud_requirements.fetch.duration_ms` | 直方图 | | 工作区管理的云要求获取持续时间。         |
| `cloud_requirements.fetch_attempt` | 计数器 | 请参阅注释 | 工作区管理的云要求获取尝试。         |
| `cloud_requirements.fetch_final` | 计数器 | 请参阅注释 | 最终工作区管理的云需求获取结果。    |
| `cloud_requirements.load` | 计数器 | `trigger`、`outcome` | 工作区管理的云需求负载结果。           |

`cloud_requirements.fetch_attempt` 指标包括 `trigger`、`attempt`、`outcome` 和 `status_code` 字段。 `cloud_requirements.fetch_final` 指标包括 `trigger`、`outcome`、`reason`、`attempt_count` 和 `status_code` 字段。

<a id="turn-and-tool-activity"></a>

#### 转动和工具活动

| 公制 | 类型 | 字段 | 描述 |
| -------------------------------------- | --------- | ------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `turn.e2e_duration_ms` | 直方图 | | 一整圈的端到端时间。                                                                                 |
| `turn.ttft.duration_ms` | 直方图 | | 回合中第一个令牌的时间。                                                                                  |
| `turn.ttfm.duration_ms` | 直方图 | | 回合中第一个模型输出项的时间。                                                                      |
| `turn.network_proxy` | 计数器 | `active`、`tmp_mem_enabled` | 受管网络代理在本轮是否处于活动状态。                                                       |
| `turn.memory` | 计数器 | `read_allowed`、`feature_enabled`、`config_use_memories`、`has_citations` | 每回合内存读取可用性和内存引用使用情况。                                                     |
| `turn.tool.call` | 直方图 | `tmp_mem_enabled` | 回合中的刀具调用次数。                                                                                |
| `turn.token_usage` | 直方图 | `token_type`、`tmp_mem_enabled` | 按Token类型划分的每回合Token使用情况（`total`、`input`、`cached_input`、`output`、或 `reasoning_output`）。          |
| `tool.call` | 计数器 | `tool`、`success` | 按工具名称和成功/失败的工具调用计数。                                                          |
| `tool.call.duration_ms` | 直方图 | `tool`、`success` | 按工具名称和结果显示的工具执行持续时间（以毫秒为单位）。                                                |
| `tool.unified_exec` | 计数器 | `tty` | 通过 TTY 模式调用统一执行工具。                                                                             |
| `approval.requested` | 计数器 | `tool`、`approved` | 工具批准请求结果（`approved`、`approved_with_amendment`、`approved_for_session`、`denied`、 `abort`）。 |
| `mcp.call` | 计数器 | 请参见注释 | MCP 工具调用结果。                                                                                      |
| `mcp.call.duration_ms` | 直方图 | 请参见注释 | MCP 工具调用持续时间。                                                                                    |
| `mcp.tools.list.duration_ms` | 直方图 | `cache` | MCP 工具列表持续时间，包括缓存命中/未命中状态。                                                          |
| `mcp.tools.fetch_uncached.duration_ms` | 直方图 | | MCP 工具获取未命中缓存的持续时间。                                                                |
| `mcp.tools.cache_write.duration_ms` | 直方图 | | Codex 应用程序 MCP 工具缓存写入的持续时间。                                                                    |
| `hooks.run` | 计数器 | `hook_name`、`source`、`status` | 按挂钩名称、源和状态划分的挂钩运行计数。                                                                 |
| `hooks.run.duration_ms` | 直方图 | `hook_name`、`source`、`status` | 挂钩运行持续时间（以毫秒为单位）。                                                                               |

`mcp.call` 和 `mcp.call.duration_ms` 指标包括 `status`；正常工具调用排放还包括 `tool`，以及 `connector_id` 和 `connector_name`（如果可用）。被阻止的 Codex 应用程序 MCP 调用可能仅与 `status` 一起发出 `mcp.call`。

<a id="threads-tasks-and-features"></a>

#### 线程、任务和功能

| 公制 | 类型 | 字段 | 说明 |
| --------------------------------- | --------- | --------------------- | -------------------------------------------------------------------------------- |
| `feature.state` | 计数器 | `feature`、`value` | 与默认值不同的功能值（每个非默认值发出一行）。         |
| `status_line` | 计数器 | | 会话以配置的状态行启动。                                   |
| `model_warning` | 计数器 | | 向模型发送警告。                                                       |
| `thread.started` | 计数器 | `is_git` | 创建的新线程，根据工作目录是否位于 Git 仓库中进行标记。    |
| `conversation.turn.count` | 计数器 | | 每个线程的用户/助理转数，记录在线程末尾。              |
| `thread.fork` | 计数器 | `source` | 通过分叉现有线程创建的新线程。                                |
| `thread.rename` | 计数器 | | 线程已重命名。                                                                  |
| `thread.side` | 计数器 | `source` | 已创建侧面对话。                                                       |
| `thread.skills.enabled_total` | 直方图 | | 新线程启用的技能数量。                                       |
| `thread.skills.kept_total` | 直方图 | | 提示渲染后保留的启用技能数量。                            |
| `thread.skills.truncated` | 直方图 | | 技能渲染是否截断了启用的技能列表（`1` 或 `0`）。          |
| `task.compact` | 计数器 | `type` | 每种类型的压实次数（`remote` 或 `local`），包括手动和自动。 |
| `task.review` | 计数器 | | 触发的审核数量。                                                     |
| `task.undo` | 计数器 | | 触发的撤消操作数。                                                |
| `task.user_shell` | 计数器 | | 用户 shell 操作数（例如 TUI 中的 `!`）。                       |
| `shell_snapshot` | 计数器 | 参见注释 | 是否成功拍摄 shell 快照。                                       |
| `shell_snapshot.duration_ms` | 直方图 | `success` | 拍摄 shell 快照的时间。                                                   |
| `skill.injected` | 计数器 | `status`、`skill` | 按技能划分的技能注入结果。                                               |
| `plugins.startup_sync` | 计数器 | `transport`、`status` | 策划的插件启动同步尝试。                                            |
| `plugins.startup_sync.final` | 计数器 | `transport`、`status` | 最终策划的插件启动同步结果。                                       |
| `multi_agent.spawn` | 计数器 | `role` | 智能体按角色生成。                                                            |
| `multi_agent.resume` | 计数器 | | 智能体恢复。                                                                   |
| `multi_agent.nickname_pool_reset` | 计数器 | | 智能体昵称池重置。                                                      |

`shell_snapshot` 指标包括 `success` 以及发生故障时的 `failure_reason`。

<a id="memory-and-local-state"></a>

#### 内存和本地状态

| 公制 | 类型 | 字段 | 描述 |
| ------------------------------ | --------- | ------------------------- | --------------------------------------------------------- |
| `memory.phase1` | 计数器 | `status` | 内存阶段 1 作业按状态计数。                      |
| `memory.phase1.e2e_ms` | 直方图 | | 内存阶段 1 的端到端持续时间。 |
| `memory.phase1.output` | 计数器 | | 存储器第 1 相输出已写入。                           |
| `memory.phase1.token_usage` | 直方图 | `token_type` | 按令牌类型划分的内存阶段 1 令牌使用情况。                 |
| `memory.phase2` | 计数器 | `status` | 内存阶段 2 作业按状态计数。                      |
| `memory.phase2.e2e_ms` | 直方图 | | 内存阶段 2 的端到端持续时间。 |
| `memory.phase2.input` | 计数器 | | 存储器第 2 相输入计数。                               |
| `memory.phase2.token_usage` | 直方图 | `token_type` | 按令牌类型划分的内存阶段 2 令牌使用情况。                 |
| `memories.usage` | 计数器 | `kind`、`tool`、`success` | 按种类、工具和成功/失败列出的内存使用情况。          |
| `external_agent_config.detect` | 计数器 | 请参阅注释 | 按迁移项目类型进行的外部智能体配置检测。  |
| `external_agent_config.import` | 计数器 | 请参阅注释 | 按迁移项目类型导入外部智能体配置。     |
| `db.backfill` | 计数器 | `status` | 初始状态 DB 回填结果（`upserted`、`failed`）。 |
| `db.backfill.duration_ms` | 直方图 | `status` | 初始状态 DB 回填的持续时间。                |
| `db.error` | 计数器 | `stage` | 状态 DB 操作期间出错。                        |

`external_agent_config.detect` 和 `external_agent_config.import` 指标包括 `migration_type`；技能移民还包括 `skills_count`。

<a id="windows-sandbox"></a>

#### Windows沙箱

| 公制 | 类型 | 字段 | 说明 |
| ------------------------------------------------ | --------- | ----------------------------------------- | ----------------------------------------------------- |
| `windows_sandbox.setup_success` | 计数器 | `originator`、`mode` | Windows 沙箱设置成功。                      |
| `windows_sandbox.setup_failure` | 计数器 | `originator`、`mode` | Windows 沙箱设置失败。                       |
| `windows_sandbox.setup_duration_ms` | 直方图 | `result`、`originator`、`mode` | Windows 沙箱设置持续时间。                       |
| `windows_sandbox.elevated_setup_success` | 计数器 | | 提升的 Windows 沙箱设置成功。             |
| `windows_sandbox.elevated_setup_failure` | 计数器 | 请参阅注释 | 提升的 Windows 沙箱设置失败。              |
| `windows_sandbox.elevated_setup_canceled` | 计数器 | 请参阅注释 | 已取消提升的 Windows 沙箱安装尝试。     |
| `windows_sandbox.elevated_setup_duration_ms` | 直方图 | `result` | Windows 沙箱设置持续时间延长。              |
| `windows_sandbox.elevated_prompt_shown` | 计数器 | | 显示提升的沙箱设置提示。                  |
| `windows_sandbox.elevated_prompt_accept` | 计数器 | | 已接受提升的沙箱设置提示。               |
| `windows_sandbox.elevated_prompt_use_legacy` | 计数器 | | 用户从提升的提示中选择了旧版沙箱。   |
| `windows_sandbox.elevated_prompt_quit` | 计数器 | | 用户从提升的提示符中退出。                   |
| 显示 `windows_sandbox.fallback_prompt_shown` | 计数器 | | 回退沙箱提示。                        |
| `windows_sandbox.fallback_retry_elevated` | 计数器 | | 用户从回退提示中重试提升的设置。 |
| `windows_sandbox.fallback_use_legacy` | 计数器 | | 用户从回退提示中选择了旧版沙箱。   |
| `windows_sandbox.fallback_prompt_quit` | 计数器 | | 用户从后备提示中退出。                   |
| `windows_sandbox.legacy_setup_preflight_failed` | 计数器 | 请参阅注释 | 旧版 Windows 沙箱设置预检失败。       |
| `windows_sandbox.setup_elevated_sandbox_command` | 计数器 | | 已调用提升的沙箱设置命令。               |
| `windows_sandbox.createprocessasuserw_failed` | 计数器 | `error_code`、`path_kind`、`exe`、`level` | Windows `CreateProcessAsUserW` 故障。              |

当 Windows 安装失败详细信息可用时，提升的安装失败指标包括 `code` 和 `message`，并且当从共享安装路径发出时可能包括 `originator`。当从共享设置路径发出时，`windows_sandbox.legacy_setup_preflight_failed` 指标包括 `originator`，但回退提示预检失败可能不包括任何字段。

<a id="feedback-controls"></a>

### 反馈控制

默认情况下，本地客户端允许用户从 `/feedback` 发送反馈。要禁用计算机上的 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展的反馈收集，请更新您的配置：

```toml
[feedback]
enabled = false
```

禁用后，`/feedback` 显示禁用消息，并且 Codex 拒绝反馈提交。

<a id="hide-or-surface-reasoning-events"></a>

### 隐藏或使用界面推理事件

如果你想减少嘈杂的“推理”输出（例如在 CI 日志中），你可以抑制它：

```toml
hide_agent_reasoning = true
```

如果您想在模型发出原始推理内容时显示原始推理内容：

```toml
show_raw_agent_reasoning = true
```

仅当您的工作流程可接受时才启用原始推理。某些模型/提供者（如 `gpt-oss`）不会发出原始推理；在这种情况下，此设置没有明显的效果。

<a id="notifications"></a>

## 通知

每当 Codex 发出支持的事件（当前仅 `agent-turn-complete`）时，使用 `notify` 触发外部程序。这对于桌面 toast、聊天 webhook、CI 更新或内置 TUI 通知未涵盖的任何旁路警报非常方便。

```toml
notify = ["python3", "/path/to/notify.py"]
```

与 `agent-turn-complete` 反应的示例 `notify.py`（截断）：

```python
#!/usr/bin/env python3
import json, subprocess, sys

def main() -> int:
    notification = json.loads(sys.argv[1])
    if notification.get("type") != "agent-turn-complete":
        return 0
    title = f"Codex: {notification.get('last-assistant-message', 'Turn Complete!')}"
    message = " ".join(notification.get("input-messages", []))
    subprocess.check_output([
        "terminal-notifier",
        "-title", title,
        "-message", message,
        "-group", "codex-" + notification.get("thread-id", ""),
        "-activate", "com.googlecode.iterm2",
    ])
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

该脚本接收单个 JSON 参数。常见字段包括：

- `type`（目前为`agent-turn-complete`）
- `thread-id`（会话标识符）
- `turn-id`（对话轮次标识符）
- `cwd`（工作目录）
- `input-messages`（导致对话轮次的用户消息）
- `last-assistant-message`（最后一个助手消息文本）

将脚本放置在磁盘上的某个位置并将 `notify` 指向它。

<a id="notify-vs-tuinotifications"></a>

#### `notify` 与 `tui.notifications` 对比

- `notify` 运行外部程序（适用于 webhook、桌面通知程序、CI 挂钩）。
- `tui.notifications` 内置于 TUI 中，并且可以选择按事件类型进行过滤（例如，`agent-turn-complete` 和 `approval-requested`）。
- `tui.notification_method` 控制 TUI 如何发出终端通知（`auto`、`osc9` 或 `bel`）。
- `tui.notification_condition` 控制 TUI 通知是否仅在终端为 `unfocused` 或 `always` 时触发。

在 `auto` 模式下，Codex 更喜欢 OSC 9 通知（某些终端将其解释为桌面通知的终端转义序列），否则会回退到 BEL (`\x07`)。

有关确切的按键，请参阅 [配置参考](config-reference.zh-CN.md)。

<a id="history-persistence"></a>

## 历史的坚守

默认情况下，Codex 将本地会话记录保存在 `CODEX_HOME` 下（例如 `~/.codex/history.jsonl`）。要禁用本地历史记录持久性：

```toml
[history]
persistence = "none"
```

要限制历史文件大小，请设置 `history.max_bytes`。当文件超出上限时，Codex 会删除最旧的条目并压缩文件，同时保留最新的记录。

```toml
[history]
max_bytes = 104857600 # 100 MiB
```

<a id="clickable-citations"></a>

## 可点击的引文

如果您使用支持它的终端/编辑器集成，Codex 可以将文件引用呈现为可单击的链接。配置 `file_opener` 以选择 Codex 使用的 URI 方案：

```toml
file_opener = "vscode" # or cursor, windsurf, vscode-insiders, none
```

示例：像 `/home/user/project/main.py:42` 这样的引文可以重写为可点击的 `vscode://file/...:42` 链接。

<a id="project-instructions-discovery"></a>

## 项目指令发现

Codex 读取 `AGENTS.md`（及相关文件），并在会话的第一轮中包含有限数量的项目指导。两个旋钮控制其工作原理：

- `project_doc_max_bytes`：从每个 `AGENTS.md` 文件读取多少内容
- `project_doc_fallback_filenames`：当目录级别缺少 `AGENTS.md` 时要尝试的其他文件名

有关详细演练，请参阅 [AGENTS.md 的自定义指令](../agent-configuration/agents-md.zh-CN.md)。

<a id="desktop"></a>

## 桌面

本节中的选项仅适用于 ChatGPT 桌面应用程序。

<a id="add-custom-file-handlers"></a>

### 添加自定义文件处理程序

在用户级别 `~/.codex/config.toml` 中，在 `desktop.custom_file_handlers` 下添加条目，以在 ChatGPT 桌面应用程序默认情况下不支持的编辑器或内部启动器中打开文件。每个条目都会向应用程序的 **打开于** 菜单添加一个编辑器目标。当 `command` 是现有绝对路径或从应用程序的 `PATH` 解析时，应用程序会列出目标。

以下示例显示了将文件传递给处理程序的三种方法：

```toml
# 直接在命令后面附加打开的路径。
[desktop.custom_file_handlers.vscodium]
label = "VSCodium"
icon = "/Users/you/.codex/icons/vscodium.png"
command = "codium"

# 将固定参数放在打开的路径之前。
[desktop.custom_file_handlers.textedit]
label = "TextEdit"
icon = "/Users/you/.codex/icons/textedit.png"
command = "/usr/bin/open"
args = ["-a", "TextEdit"]

# 附加一个 JSON 参数以及路径和编辑器上下文。
[desktop.custom_file_handlers.company_editor]
label = "Company Editor"
icon = "/opt/company/editor/icon.png"
command = "/opt/company/bin/editor"
input = "json_argument"
```

保存 `config.toml`，然后重新启动 ChatGPT 桌面应用程序。

处理程序 ID 是 TOML 表头的最后一段。它必须包含 1-64 个字符，以 ASCII 字母或数字开头，否则仅包含 ASCII 字母、数字、句点、下划线或连字符。应用程序公开带有 `custom:` 前缀的 ID；例如，`company_editor` 变为 `custom:company_editor`。引用包含句点的 ID，以便 TOML 不会将其解释为嵌套表。例如：

```toml
[desktop.custom_file_handlers."company.editor"]
label = "Company Editor"
icon = "/opt/company/editor/icon.png"
command = "/opt/company/bin/editor"
```

每个处理程序都支持这些字段：

| 字段 | 必填 | 说明 |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `label` | 是 | 在应用程序中显示名称。                                                                                                                                                 |
| `icon` | 是 | 捆绑的应用程序图标，例如 `apps/vscode.png`、base64 `data:image/...` URL、`file:` URI 或绝对本地图像路径。不受支持的源使用默认的 VS Code 图标。 |
| `command` | 是 | 要检测和启动的可执行路径或命令名称。                                                                                                                    |
| `args` | 否 | 在 `command` 和文件输入之间插入的字符串数组。默认为 `[]`。                                                                                            |
| `input` | 否 | 应用程序如何发送文件输入：`path`、`json_argument` 或 `json_stdin`。默认为 `path`。                                                                              |
| `supports_ssh` | 否 | 是否为 SSH 工作区中的文件提供处理程序。默认为 `false`。当处理程序需要远程主机和路径详细信息时，请使用 `json_stdin`。                     |

`input` 值控制 `args` 后面的内容：

- `path` 将路径附加为最终命令参数。
- `json_argument` 在 JSON 对象中附加 `target`、`path`、`appPath` 和 `location`。 `location` 值是具有从 1 开始的 `line` 和 `column` 值或 `null` 的对象。
- `json_stdin` 将 JSON 对象写入标准输入，而不是添加参数。它还包括`hostConfig`、`remoteWorkspaceRoot`和`remotePath`；当这些字段不适用时，它们是 `null`。

例如，当用户打开特定源位置时，`company_editor` 可以接收此参数：

```json
{
  "target": "custom:company_editor",
  "path": "/repo/src/index.ts",
  "appPath": null,
  "location": { "line": 12, "column": 3 }
}
```

选择自定义处理程序作为首选编辑器会像选择内置编辑器一样保留该选择，包括每个项目的首选项。

<a id="tui-options"></a>

## 途易选项

运行不带子命令的 `codex` 会启动交互式终端 UI (TUI)。 Codex 在 `[tui]` 下公开了一些特定于 TUI 的配置，包括：

- `tui.notifications`：启用/禁用通知（或限制特定类型）
- `tui.notification_method`：选择`auto`、`osc9`或`bel`进行终端通知
- `tui.notification_condition`：通知触发时选择 `unfocused` 或 `always`
- `tui.animations`：启用/禁用 ASCII 动画和闪光效果
- `tui.alternate_screen`：控制备用屏幕使用（设置为 `never` 以保持终端回滚）
- `tui.show_tooltips`：在欢迎屏幕上显示或隐藏入门工具提示

`tui.notification_method` 默认为 `auto`。在 `auto` 模式下，当终端似乎支持 OSC 9 通知（某些终端将其解释为桌面通知的终端转义序列）时，Codex 更喜欢 OSC 9 通知，否则则回退到 BEL (`\x07`)。

有关完整密钥列表，请参阅 [配置参考](config-reference.zh-CN.md)。