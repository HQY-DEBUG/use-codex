> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/config-file/config-basic.md)。

<a id="config-basics"></a>

# 配置基础

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 从多个位置读取配置详细信息。您的个人默认设置位于 `~/.codex/config.toml` 中，您可以使用 `.codex/config.toml` 文件添加项目覆盖。为了安全起见，Codex 仅在您信任项目时才加载项目 `.codex/` 层。

<a id="codex-configuration-file"></a>

## Codex配置文件

Codex 将用户级配置存储在 `~/.codex/config.toml` 中。要将设置范围限制到特定项目或子文件夹，请在仓库中添加 `.codex/config.toml` 文件。

要从 Codex IDE 扩展打开配置文件，请选择右上角的齿轮图标，然后选择 **Codex 设置 > 打开 config.toml**。

CLI 和 IDE 扩展共享相同的配置层。您可以使用它们来：

- 设置默认模型和提供商。
- 配置[审批策略和沙箱设置](../agent-approvals-security.zh-CN.md#sandbox-and-approvals)。
- 配置[MCP服务器](../extend/mcp.zh-CN.md)。

<a id="configuration-precedence"></a>

## 配置优先级

Codex 按以下顺序解析值（优先级最高的在前）：

1. CLI 标志和 `--config` 覆盖
2. 项目配置文件：`.codex/config.toml`，从项目根目录到当前工作目录（最近的获胜；仅限受信任的项目）
3. 使用 `--profile profile-name` (`~/.codex/profile-name.config.toml`) 选择的 [公司简介](config-advanced.zh-CN.md#profiles) 文件
4. 用户配置：`~/.codex/config.toml`
5. 为登录工作区交付时，默认为云托管 `config.toml`
6. 系统配置（如果存在）：Unix 上的 `/etc/codex/config.toml`
7. 内置默认值

使用该优先级在 `config.toml` 中设置共享默认值，并使 [配置文件](config-advanced.zh-CN.md#profiles) 专注于不同的值。

云管理和系统配置可以定义插件市场并设置是否默认启用插件。这些与强制执行的 `requirements.toml` 策略是分开的。参见 [配置插件市场和默认值](../enterprise/managed-configuration.zh-CN.md#configure-plugin-marketplaces-and-defaults)。

如果将项目标记为不受信任，Codex 会跳过项目范围的 `.codex/` 层，包括项目本地配置、挂钩和规则。用户和系统配置仍然加载，包括用户/全局挂钩和规则。

对于通过 `-c`/`--config` 的一次性覆盖（包括 TOML 引用规则），请参阅 [高级配置](config-advanced.zh-CN.md#one-off-overrides-from-the-cli)。

在托管计算机上，您的组织还可以通过 `requirements.toml` 强制实施约束（例如，禁止 `approval_policy = "never"` 或 `sandbox_mode = "danger-full-access"`）。请参见 [受管配置](../enterprise/managed-configuration.zh-CN.md) 和 [管理员强制要求](../enterprise/managed-configuration.zh-CN.md#admin-enforced-requirements-requirementstoml)。

<a id="common-configuration-options"></a>

## 常用配置选项

以下是人们最常更改的一些选项：

<a id="default-model"></a>

#### 默认模型

选择CLI和IDE中默认使用的模型Codex。

```toml
model = "gpt-5.6"
```


<a id="approval-prompts"></a>

#### 批准提示

控制 Codex 在运行生成的命令之前何时暂停询问。

```toml
approval_policy = "on-request"
```

有关 `on-request` 和 `never` 之间的行为差异，请参阅 [运行时无批准提示](../agent-approvals-security.zh-CN.md#run-without-approval-prompts) 和 [常见的沙箱和审批组合](../agent-approvals-security.zh-CN.md#common-sandbox-and-approval-combinations)。如果现有配置使用 `approval_policy = "untrusted"`，请参阅 [从已停用的 `untrusted` 审批策略迁移](../agent-approvals-security.zh-CN.md#migrate-from-the-retired-untrusted-approval-policy)。

<a id="sandbox-level"></a>

#### 沙箱级别

调整 Codex 在执行命令时拥有的文件系统和网络访问权限。

```toml
sandbox_mode = "workspace-write"
```

有关逐个模式的行为（包括受保护的 `.git`/`.codex` 路径和网络默认值），请参阅 [沙箱和批准](../agent-approvals-security.zh-CN.md#sandbox-and-approvals)、[可写根中的受保护路径](../agent-approvals-security.zh-CN.md#protected-paths-in-writable-roots) 和 [网络接入](../agent-approvals-security.zh-CN.md#network-access)。

<a id="permission-profiles"></a>

#### 权限配置文件

Codex 还支持可重用文件系统和网络策略的命名权限配置文件。内置配置文件为 `:read-only`、`:workspace` 和 `:danger-full-access`。自定义配置文件使用“[ 权限”。<name>]` tables and a matching `default_permissions`值。参见 [权限](../permissions.zh-CN.md)。

<a id="windows-sandbox-mode"></a>

#### Windows 沙箱模式

在 Windows 上本机运行 Codex 时，在 `windows` 表中将本机沙箱模式设置为 `elevated`。仅当您没有管理员权限或提升的安装失败时才使用 `unelevated`。

```toml
[windows]
sandbox = "elevated"   # Recommended
# sandbox = "unelevated" # Fallback if admin permissions/setup are unavailable
```

<a id="web-search-mode"></a>

#### 网页搜索模式

Codex 默认启用本地聊天的 Web 搜索，并提供来自 Web 搜索缓存的结果。缓存是 OpenAI 维护的 Web 结果索引，因此缓存模式返回预先索引的结果，而不是获取实时页面。这可以减少来自任意实时内容的提示注入的风险，但您仍应将 Web 结果视为不可信。如果您使用的是 `--yolo` 或其他 [完全访问沙箱设置](../agent-approvals-security.zh-CN.md#common-sandbox-and-approval-combinations)，网络搜索默认为实时结果。使用 `web_search` 选择模式：

- `"cached"`（默认）提供来自网络搜索缓存的结果。
- `"indexed"` 仅当搜索索引限制请求时才允许外部 Web 访问。
- `"live"` 从网络获取最新数据（与 `--search` 相同）。
- `"disabled"` 关闭网络搜索工具。

```toml
web_search = "cached"  # default; serves results from the web search cache
# web_search = "indexed" # gate external web access through the search index
# web_search = "live"  # fetch the most recent data from the web (same as --search)
# web_search = "disabled"
```

<a id="reasoning-effort"></a>

#### 推理努力

调整模型在支持时应用的推理工作量。

```toml
model_reasoning_effort = "high"
```

<a id="communication-style"></a>

#### 沟通方式

为支持的模型设置默认通信方式。

```toml
personality = "friendly" # or "pragmatic" or "none"
```

您可以稍后在使用 `/personality` 的活动会话中或使用应用程序服务器 API 时按线程/回合覆盖此设置。

<a id="tui-keymap"></a>

#### TUI 键盘映射

自定义`tui.keymap`下的终端快捷方式。选定的输入框动作回落到匹配的 `tui.keymap.global` 绑定；如果支持，则上下文特定的绑定优先。空列表会解除操作的绑定。

```toml
[tui.keymap.global]
open_transcript = "ctrl-t"

[tui.keymap.composer]
submit = ["enter", "ctrl-m"]

[tui.keymap.chat]
interrupt_turn = "f12"
```

<a id="command-environment"></a>

#### 命令环境

控制哪些环境变量 Codex 转发到生成的命令。使用键控过滤器仅保留您需要的变量：

```toml
[shell_environment_policy]
ignore_default_excludes = false

[shell_environment_policy.filters]
"PATH" = "include"
"HOME" = "include"
```

`ignore_default_excludes` 默认为 `true`，它会跳过对包含 `KEY`、`SECRET` 或 `TOKEN` 的变量名称的自动过滤。当您需要自动过滤时，将其设置为 `false`。有关排除规则、优先级和旧配置，请参阅 [Shell环境政策](config-advanced.zh-CN.md#shell-environment-policy)。

<a id="log-directory"></a>

#### 日志目录

覆盖 Codex 写入本地日志文件的位置。显式设置 `log_dir` 还会在该目录中启用选择加入纯文本 TUI 日志 `codex-tui.log`。

```toml
log_dir = "/absolute/path/to/codex-logs"
```

对于一次性运行，您还可以从 CLI 进行设置：

```bash
codex -c log_dir=./.codex-log
```

<a id="feature-flags"></a>

## 功能标志

使用 `config.toml` 中的 `[features]` 表来切换可选和实验功能。

<a id="common-feature-flags"></a>

### 常见功能标志

| 密钥 | 默认 | 到期日 | 描述 |
| -------------------- | :-------------------: | ------------ | ---------------------------------------------------------------------------------------- |
| `apps` | true | 稳定 | 启用应用程序（连接器）集成 |
| `goals` | true | 稳定 | 启用持久目标和自动延续 |
| `hooks` | true | 稳定 | 启用 `hooks.json` 或内联 `[hooks]` 的生命周期挂钩。参见 [挂钩](../hooks.zh-CN.md)。 |
| `fast_mode` | true | 稳定 | 启用快速模式选择和 `service_tier = "fast"` 路径 |
| `memories` | 假 | 实验 | 启用 [回忆](../customization/memories.zh-CN.md) |
| `multi_agent` | true | 稳定 | 启用子智能体协作工具 |
| `personality` | 真实 | 稳定 | 启用个性选择控件 |
| `remote_plugin` | true | 稳定 | 启用远程插件目录 |
| `shell_snapshot` | true | 稳定 | 对 shell 环境进行快照以加速重复命令 |
| `shell_tool` | true | 稳定 | 启用默认 `shell` 工具 |
| `unified_exec` | `true` Windows 除外 | 稳定版 | 使用统一的 PTY 支持的执行工具 |
| `web_search` | true | 已弃用 | 旧版切换；更喜欢顶级 `web_search` 设置 |
| `web_search_cached` | false | 已弃用 | 未设置时映射到 `web_search = "cached"` 的传统切换 |
| `web_search_request` | false | 已弃用 | 未设置时映射到 `web_search = "live"` 的传统切换 |

此表列出了常见的面向用户的标志，而不是每个内部或正在开发的功能。成熟度列使用“实验”、“测试版”和“稳定”等标签。请参阅 [功能成熟度](../feature-maturity.zh-CN.md) 了解如何解释这些标签。

省略功能键以保留其默认值。

生命周期钩子配置请参见[挂钩](../hooks.zh-CN.md)。

<a id="enabling-features"></a>

### 启用功能

- 在`config.toml`中，在`[features]`下添加`feature_name = true`。
- 从 CLI 运行 `codex --enable feature_name`。
- 要启用多个功能，请运行 `codex --enable feature_a --enable feature_b`。
- 要禁用某项功能，请将密钥设置为 `config.toml` 中的 `false`。