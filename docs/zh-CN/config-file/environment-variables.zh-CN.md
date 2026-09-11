> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/config-file/environment-variables.md)。

<a id="environment-variables"></a>

# 环境变量

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 使用 `config.toml` 进行耐用设置。使用环境变量进行 shell 范围的覆盖、自动化机密、安装程序行为或诊断。

本页列出了 Codex 直接读取的稳定公共环境变量。它不会列出您使用 [`env_key`](config-advanced.zh-CN.md#custom-model-providers) 自行选择的内部开发变量、测试变量或特定于提供商的秘密名称。

<a id="core-locations"></a>

## 核心地点

| 变量 | | 使用的默认值 | 说明 |
| ------------------- | ------------------------------------------ | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CODEX_HOME` | CLI、IDE 扩展、应用程序服务器、安装程序 | `~/.codex` | 设置 Codex 状态的根，包括配置、身份验证、日志、会话、技能和独立包元数据。如果设置它，该目录必须已经存在。 |
| `CODEX_SQLITE_HOME` | CLI 和应用程序服务器状态 | `CODEX_HOME` | 设置 SQLite 支持的状态的存储位置。 `sqlite_home` 配置选项优先。相对路径从当前工作目录解析。           |

有关 `CODEX_HOME` 下存储的文件的更多信息，请参阅 [配置和状态位置](config-advanced.zh-CN.md#config-and-state-locations)。

<a id="installer-variables"></a>

## 安装程序变量

这些变量适用于 `https://chatgpt.com/codex/install.sh` 和 `https://chatgpt.com/codex/install.ps1` 提供的独立安装脚本。

| 变量 | 默认值 | 描述 |
| ----------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CODEX_NON_INTERACTIVE` | `false` | 设置为 `1`、`true` 或 `yes` 以跳过安装程序提示。提示使用默认响应，因此将其用于脚本化安装和更新，而不是首次运行安装。 |
| macOS/Linux 上的 `CODEX_INSTALL_DIR` | `~/.local/bin`； Windows 上的 `%LOCALAPPDATA%\Programs\OpenAI\Codex\bin` | 更改可见 `codex` 命令的安装位置。独立包缓存仍然位于 `CODEX_HOME/packages/standalone` 下。                        |

对于无人值守安装，请在运行下载的安装程序的 shell 上设置 `CODEX_NON_INTERACTIVE=1`：

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | CODEX_NON_INTERACTIVE=1 sh
```

```powershell
$env:CODEX_NON_INTERACTIVE=1; irm https://chatgpt.com/codex/install.ps1 | iex
```

<a id="authentication-and-network"></a>

## 身份验证和网络

| 变量 | | 使用的变量 说明 |
| ---------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `CODEX_API_KEY` | 执行、审查、TypeScript SDK、远程执行服务器 | 为非交互式 Codex 进程提供 API 密钥。运行仓库控制的代码时，将其设置为内联而不是作业范围。             |
| `CODEX_ACCESS_TOKEN` | CLI、应用程序服务器、可信自动化 | 为可信自动化提供 ChatGPT 或 Codex 访问令牌。对于持久登录，请将其通过管道传输到 `codex login --with-access-token`。             |
| `OPENAI_FEDERATION_RULE_ID` | 工作负载身份 | 选择为工作负载配置的联合规则。                                                                                        |
| `OPENAI_IDENTITY_TOKEN_FILE` | 工作负载标识 | 指向包含当前 OIDC 令牌或 SPIFFE JWT-SVID 的文件的绝对路径。                                                |
| `OPENAI_WORKLOAD_IDENTITY_CONTEXT` | 工作负载身份 | 可以选择为客户端报告的审计归因提供有界 JSON 标识符。它不会影响身份验证或授权。         |
| `CODEX_CA_CERTIFICATE` | HTTPS、登录和 WebSocket 客户端 | 指向具有公司 TLS 拦截或私有根证书的环境的 PEM CA 捆绑包。优先于 `SSL_CERT_FILE`。 |
| `SSL_CERT_FILE` | HTTPS、登录和 WebSocket 客户端 | 未设置 `CODEX_CA_CERTIFICATE` 时的后备 PEM CA 捆绑路径。                                                                               |

对于提供程序 API 密钥，请在模型提供程序配置中设置 [`env_key`](config-advanced.zh-CN.md#custom-model-providers)。 Codex 读取该配置命名的变量，因此变量名称本身不是固定的 Codex 环境变量。

对于自动化秘密处理，请参阅 [使用 API 密钥身份验证](../non-interactive-mode.zh-CN.md#use-api-key-auth)。有关访问令牌设置，请参阅 [访问令牌](../enterprise/access-tokens.zh-CN.md)。有关工作负载身份设置，请参阅 [工作负载身份联合](../enterprise/workload-identity.zh-CN.md)。

<a id="diagnostics"></a>

## 诊断

| 变量 | | 使用的变量 说明 |
| ---------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| `RUST_LOG` | CLI 和应用程序服务器 | 控制 Rust 日志过滤和详细程度。 `codex exec` 默认为 `error` 输出，除非您设置更详细的值。 |

`RUST_LOG` 接受 `error`、`warn`、`info`、`debug` 和 `trace` 等值。它还接受更有针对性的 Rust 日志过滤器，例如 `codex_core=debug,codex_tui=debug`。

默认情况下，交互式 CLI 将诊断记录在有界本地存储中，但明文 `codex-tui.log` 文件是可选的。当您需要明文日志进行故障排除时，显式设置 `log_dir`：

```bash
RUST_LOG=debug codex -c log_dir=./.codex-log
tail -F ./.codex-log/codex-tui.log
```

在非交互模式下，`codex exec` 内联打印消息，而不是写入单独的 TUI 日志文件。