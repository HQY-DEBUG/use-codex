> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/extend/mcp.md)。

<a id="model-context-protocol"></a>

# 模型上下文协议

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

模型上下文协议 (MCP) 将模型连接到工具和上下文。使用它可以让 ChatGPT 或 Codex 访问第三方文档，或者让它与浏览器或 Figma 等开发人员工具交互。

ChatGPT web 可以使用插件提供的远程 MCP 支持的工具。本地 Codex 客户端还可以直接连接到 MCP 服务器并共享其配置。

<a id="supported-mcp-features"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展支持 MCP 服务器并共享同一 Codex 主机的 MCP 配置。

以下支持的服务器功能适用于 Codex 主机上配置的 MCP 服务器。托管插件工具可以具有不同的功能。

<a id="supported-mcp-features"></a>

## 支持的 MCP 功能

- **STDIO 服务器**：作为本地进程运行的服务器（由命令启动）。
  - 环境变量
- **可流式传输的 HTTP 服务器**：您通过某个地址访问的服务器。
  - 承载令牌认证
  - OAuth 身份验证，包括客户端 ID 元数据文档 (CIMD) 和动态客户端注册 (DCR)
  - ChatGPT 受信任第一方服务器的会话身份验证
- **服务器说明**：Codex 读取初始化期间返回的 MCP `instructions` 字段，并将其与服务器工具一起用作服务器范围的指导。

如果您为 Codex 构建或维护 MCP 服务器，请使用 `instructions` 来实现跨服务器应用的跨工具工作流程、约束和速率限制。保持前 512 个字符独立，以便在 Codex 决定如何使用服务器时提供最重要的指导。

<a id="connect-codex-to-an-mcp-server"></a>

## 将 Codex 连接到 MCP 服务器

Codex 将 MCP 配置与其他 Codex 配置设置一起存储在 `config.toml` 中。默认情况下，这是 `~/.codex/config.toml`，但您也可以将 MCP 服务器范围限定为具有 `.codex/config.toml` 的项目（仅限受信任的项目）。

ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展共享此配置。配置 MCP 服务器后，您可以在这些客户端之间切换，而无需重新进行设置。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="configure-in-the-chatgpt-desktop-app"></a>

### 在 ChatGPT 桌面应用程序中配置

1. 打开**设置**，然后选择**MCP服务器**。
2. 选择**添加服务器**。
3. 输入名称，选择 **标准输入输出** 或 **流式 HTTP**，然后提供服务器的命令或 URL。
4. 保存服务器，然后选择 **重新启动**。

服务器列表显示哪些服务器已启用以及哪些服务器需要 OAuth。当 OAuth 服务器需要登录时，选择 **认证**。在编辑器中，输入 `/mcp` 以查看连接的服务器。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="use-mcp-backed-tools-in-chatgpt-web"></a>

## 在 ChatGPT Web 中使用 MCP 支持的工具

在托管的 ChatGPT Work 聊天中，安装 [插件](../plugins.zh-CN.md) 以使用其捆绑的连接器和远程 MCP 工具。安装后，Chat 和 Work 就可以使用这些工具。工作区管理员可以控制哪些插件和工具可用。

ChatGPT web 不会读取本地 Codex 配置文件或公开本地 Codex 命令菜单。打开 **插件** 选项卡以浏览和管理可用工具。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="configure-with-the-cli"></a>

### 使用 CLI 配置

<a id="add-an-mcp-server"></a>

#### 添加 MCP 服务器

```bash
codex mcp add <server-name> --env VAR1=VALUE1 --env VAR2=VALUE2 -- <stdio server-command>
```

例如，要添加 Context7（用于开发人员文档的免费 MCP 服务器），您可以运行以下命令：

```bash
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

<a id="other-cli-commands"></a>

#### 其他 CLI 命令

运行 `codex mcp list` 查看配置的服务器。要查看所有可用的 MCP 命令，请运行 `codex mcp --help`。对于支持 OAuth 的服务器，运行 `codex mcp login<server-name>`.

<a id="terminal-ui-tui"></a>

#### 终端用户界面 (TUI)

在 `codex` TUI 中，使用 `/mcp` 查看活动的 MCP 服务器。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="configure-in-the-ide-extension"></a>

### 在IDE扩展中配置

1. 打开齿轮菜单，然后选择 **MCP服务器**。
2. 选择**添加服务器**。
3. 输入名称，选择 **标准输入输出** 或 **流式 HTTP**，然后提供服务器的命令或 URL。
4. 保存服务器，然后选择 **重新启动扩展**。

MCP 服务器列表显示哪些服务器已启用以及哪些服务器需要 OAuth。当 OAuth 服务器需要登录时，选择 **认证**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="configure-with-configtoml"></a>

### 使用config.toml进行配置

要进行更细粒度的控制，请编辑 `~/.codex/config.toml` 或项目范围的 `.codex/config.toml`。有关每个受支持的 MCP 选项的可搜索列表，请参阅 [配置参考](../config-file/config-reference.zh-CN.md)。

使用“[mcp_servers”配置每个 MCP 服务器。<server-name>配置文件中的]`表。

</ContentModeSwitch>

<a id="stdio-servers"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="stdio-servers"></a>

#### STDIO 服务器

- `command`（必需）：启动服务器的命令。
- `args`（可选）：传递给服务器的参数。
- `env`（可选）：为服务器设置的环境变量。
- `env_vars`（可选）：允许和转发的环境变量。
- `cwd`（可选）：启动服务器的工作目录。
- `experimental_environment`（可选）：设置为 `remote`，以便在远程执行器环境可用时通过远程执行器环境启动 stdio 服务器。

`env_vars` 可以包含普通变量名或带有源的对象：

```toml
env_vars = ["LOCAL_TOKEN", { name = "REMOTE_TOKEN", source = "remote" }]
```

字符串条目和 `source = "local"` 从 Codex 的本地环境读取。 `source = "remote"` 从远程执行器环境读取并需要远程 MCP stdio。

</ContentModeSwitch>

<a id="streamable-http-servers"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="streamable-http-servers"></a>

#### 可流式传输的 HTTP 服务器

- `url`（必填）：服务器地址。
- `auth`（可选）：在配置承载令牌和授权标头后尝试进行身份验证。使用 `oauth`（默认值）存储 MCP OAuth 凭据。使用 `chatgpt` 将当前 ChatGPT 会话用于受信任的第一方 ChatGPT 源，并使用存储的 OAuth 作为后备。
- `bearer_token_env_var`（可选）：在 `Authorization` 中发送的不记名令牌的环境变量名称。
- `http_headers`（可选）：标头名称到静态值的映射。
- `env_http_headers`（可选）：标头名称到环境变量名称（从环境中提取的值）的映射。
- `http_headers_helper`（可选）：打印标头名称和字符串值的 JSON 对象的本地命令，例如 `{"X-Auth": "temporary-token"}`。支持从本地环境建立的 HTTP MCP 连接；不适用于 stdio 服务器或通过远程执行环境建立的连接。

Codex 缓存连接的辅助标头。同源 POST 返回 `401` 或 `403` 后，它会刷新标头一次，并仅在帮助程序返回更改的值时重试。显式承载令牌和 OAuth 凭据优先于帮助程序提供的 `Authorization` 标头。报告范围不足的 OAuth `403` 响应不会触发帮助程序刷新。

如果没有解析任何凭证源，Codex 无需身份验证即可连接到服务器。运行“codex mcp 登录”<server-name>` 单独启动 MCP OAuth 登录。

<a id="other-configuration-options"></a>

#### 其他配置选项

- `startup_timeout_sec`（可选）：服务器启动超时（秒）。默认值：`10`。
- `tool_timeout_sec`（可选）：服务器运行工具的超时时间（秒）。默认值：`60`。
- `enabled`（可选）：设置 `false` 以禁用服务器而不删除它。
- `required`（可选）：设置`true`，如果此启用的服务器无法初始化，则启动失败。
- `enabled_tools`（可选）：工具允许列表。
- `disabled_tools`（可选）：工具拒绝列表（在 `enabled_tools` 之后应用）。
- `default_tools_approval_mode`（可选）：来自该服务器的工具的默认批准行为。支持的值为 `auto`、`prompt`、`writes` 和 `approve`。 `writes` 模式提示未标记为只读的工具。
- `工具。<tool>.approval_mode`（可选）：每个工具的批准行为覆盖。
- `工具。<tool>.output_token_limit`（可选）：在标准 20% 序列化限额之前，一种工具输出的正Token预算。覆盖该工具的模型默认输出截断预算。

顶层 `mcp_optional_startup_grace_ms` 设置控制在构建初始工具目录时 Codex 等待可选 MCP 服务器的时间。默认为 `1000` 毫秒。将其设置为 `0` 以等待每个服务器的 `startup_timeout_sec`。所需的服务器仍然使用其启动超时。

<a id="oauth-client-registration-and-callbacks"></a>

#### OAuth客户端注册和回调

当您的授权服务器需要预先注册的 OAuth 客户端时，请在添加 MCP 服务器时提供其客户端 ID：

```bash
codex mcp add example --url https://mcp.example.com --oauth-client-id my-client
```

Codex 显示完整的回调 URL 以向您的提供商注册：

```text
OAuth 回调 URL：http://127.0.0.1/callback
```

Codex 将回调与客户端 ID 一起保存在 `config.toml` 中以供以后登录：

```toml
[mcp_servers.example]
url = "https://mcp.example.com"

[mcp_servers.example.oauth]
client_id = "my-client"
callback_url = "http://127.0.0.1/callback"
```

仅当授权服务器通告 `authorization_response_iss_parameter_supported: true` 并提供元数据 `issuer` 时，新添加的预注册客户端才使用稳定回调。如果未公布发行者支持，Codex 会附加服务器特定的回调 ID，例如 `http://127.0.0.1/callback/XuuuHAzzHOni`。没有保存回调的现有客户端将继续使用其回调 ID 特定的重定向。

登录期间，回调选择取决于 OAuth 配置和授权服务器元数据：

| OAuth 配置 | 颁发者支持 | 使用的回调 |
| ------------------------------------------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `callback_url` 无 `client_id` | 支持 | 配置的回调用于客户端注册。                                                                                           |
| `callback_url` 不带 `client_id` | 不支持 | 配置的回调用于客户端注册，并附加服务器特定的回调 ID。                                             |
| `client_id` 和 `callback_url` | 支持 | 配置的回调被复用；授权响应必须包含匹配的`iss`。                                                     |
| `client_id` 和以正确回调 ID 结尾的 `callback_url` | 不支持 | 配置的回调将按原样重用。                                                                                                       |
| `client_id` 和 `callback_url` 缺少正确的回调 ID | 不支持 | 配置的回调将被忽略。 Codex 在未设置时使用 `mcp_oauth_callback_url` 或 `http://127.0.0.1/callback`，并附加回调 ID。 |
| 没有配置的 `client_id` `callback_url` | 支持或不支持 | Codex 使用全局或默认回调，并附加服务器特定的回调 ID。                                                           |

回退不会修改存储的回调 URL。 Codex 从 MCP 服务器 URL 派生回调 ID，包括其路径和查询字符串。相同的选择规则适用于自动和显式登录。

当您需要自定义回调路径或远程 Devbox 入口 URL 时，请设置 `mcp_oauth_callback_url`。当新添加的预注册客户端的提供商支持发行人识别时，他们将不改变地使用该 URL。否则，它们将使用已配置的 URL，并附加特定于服务器的回调 ID。始终注册 `codex mcp add` 显示的确切回调。

对于无端口 `http://127.0.0.1` 回调，Codex 从其显示和存储的 URL 中省略侦听器端口，然后在授权期间插入活动侦听器端口。此替换不适用于 `localhost`、IPv6 主机、HTTPS URL 或已包含端口的回调。授权服务器必须接受 [RFC 8252，第 7.3 节](https://www.rfc-editor.org/rfc/rfc8252#section-7.3) 下的可变环回端口。

设置 `mcp_oauth_callback_port` 选择固定的全局侦听端口，或设置 `mcp_servers.<server-name>.oauth.callback_port` to override it for one server. An explicit port in the callback URL doesn't configure the listener. For a direct loopback callback, use portless `http://127.0.0.1` 或为回调 URL 和侦听器配置相同的显式端口。代理回调可以有意使用与本地侦听器端口不同的外部 URL 端口。本地回调 URL 绑定到本地接口；非本地回调 URL 绑定到 `0.0.0.0`。

Codex 在交换授权代码之前验证任何返回的 `iss`。不匹配的 `iss` 始终拒绝响应。当发布发行者支持时，缺失的 `iss` 也会拒绝它。两种失败都不会交换代码或回退到另一个回调。格式错误的回调 URL 或在没有元数据颁发者的情况下宣传的颁发者支持也仍然是硬故障。参见 [验证用户身份](https://developers.openai.com/plugins/build/auth)。

如果 MCP 服务器通告 `scopes_supported`，则 Codex 在 OAuth 登录期间首选服务器通告的范围。否则，Codex 将回退到 `config.toml` 中配置的范围。

<a id="oauth-client-registration"></a>

#### OAuth 客户端注册

Codex 支持 [OAuth 客户端 ID 元数据文档 (CIMD)](https://datatracker.ietf.org/doc/draft-ietf-oauth-client-id-metadata-document/) 和动态客户端注册 (DCR)。默认情况下，当授权服务器通告 `client_id_metadata_document_supported: true` 时，Codex 自动选择 CIMD，在 `token_endpoint_auth_methods_supported` 中包含 `none`，并且回调使用支持的环回 URL。否则，Codex 使用 DCR（如果可用）。配置的 OAuth 客户端 ID 始终优先并跳过客户端注册。

对于 CIMD，Codex 使用特定于 MCP 服务器的 ChatGPT 托管元数据文档：

```text
https://chatgpt.com/oauth/codex/<callback_id>/client.json
```

Codex 派生`<callback_id>` from the MCP server URL and includes it in the loopback redirect URI, such as `http://127.0.0.1:<port>/callback/<callback_id>`。元数据文档注册不带端口的匹配环回 URI。授权服务器必须接受登录时选择的端口，同时按照 [RFC 8252](https://www.rfc-editor.org/rfc/rfc8252.html#section-7.3) 的要求精确匹配主机和路径。自定义回调主机、路径或查询参数需要 DCR 或配置的 OAuth 客户端 ID。

对稳定、共享 CIMD 文档的支持正在开发中，即将推出：

```text
https://chatgpt.com/oauth/codex/client.json
```

当授权服务器通告 `authorization_response_iss_parameter_supported: true`、在其元数据中提供有效的 `issuer` 并在授权响应中包含匹配的 `iss` 时，Codex 将使用具有共享 `/callback` 路径的稳定文档。没有发行者绑定响应的服务器将继续使用特定于回调的文档。

要选择一次 CLI 登录的注册方法，请使用 `--oauth-client-registration`：

```bash
codex mcp login <server-name> --oauth-client-registration cimd
codex mcp login <server-name> --oauth-client-registration dcr
```

默认值为 `auto`。注册选择仅适用于当前登录，不会存储在 `config.toml` 中。

<a id="configtoml-examples"></a>

#### config.toml 示例

```toml
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
env_vars = ["LOCAL_TOKEN"]

[mcp_servers.context7.env]
MY_ENV_VAR = "MY_ENV_VALUE"
```

```toml
# 可选的 MCP OAuth 回调覆盖（由 `codex mcp login` 使用）
mcp_oauth_callback_port = 5555
mcp_oauth_callback_url = "https://devbox.example.internal/callback"
```

```toml
[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"
http_headers = { "X-Figma-Region" = "us-east-1" }
```

```toml
[mcp_servers.chrome_devtools]
url = "http://localhost:3000/mcp"
enabled_tools = ["open", "screenshot"]
disabled_tools = ["screenshot"] # applied after enabled_tools
default_tools_approval_mode = "prompt"
startup_timeout_sec = 20
tool_timeout_sec = 45
enabled = true

[mcp_servers.chrome_devtools.tools.open]
approval_mode = "approve"
output_token_limit = 30000
```

<a id="plugin-provided-mcp-servers"></a>

### 提供插件的 MCP 服务器

安装的插件可以将 MCP 服务器捆绑在其插件清单中。这些服务器是从插件启动的，因此用户配置不会设置它们的传输命令。用户配置仍然可以在“plugins.xml”下控制开/关状态和工具策略。<plugin>.mcp_服务器。<server>`.

```toml
[plugins."sample@test".mcp_servers.sample]
enabled = true
default_tools_approval_mode = "prompt"
enabled_tools = ["read", "search"]

[plugins."sample@test".mcp_servers.sample.tools.search]
approval_mode = "approve"
```

插件提供的 HTTP MCP 服务器还可以在 `.mcp.json` 中声明 OAuth 设置。插件清单使用驼峰命名法字段名称 `clientId`、`callbackUrl` 和 `callbackPort`：

```json
{
  "mcpServers": {
    "sample": {
      "type": "http",
      "url": "https://mcp.example.com/mcp",
      "oauth": {
        "clientId": "my-pre-registered-client",
        "callbackUrl": "http://127.0.0.1/callback/registered"
      }
    }
  }
}
```

插件提供的 MCP 服务器遵循与其他 MCP 服务器相同的回调选择规则。如果插件提供 `clientId`，其提供程序不支持颁发者绑定的回调，并且 `callbackUrl` 缺少服务器特定的回调 ID，则 Codex 会忽略该登录 URL 并使用 `mcp_oauth_callback_url` 或未设置时的 `http://127.0.0.1/callback`，并附加回调 ID。配置的`callbackUrl`保持不变。

插件的`oauth.callbackPort`会覆盖全局的`mcp_oauth_callback_port`；如果两者均未设置，则 Codex 选择临时端口。 `callbackUrl` 中嵌入的端口不选择侦听端口。对于具有固定端口的直接环回回调，请将两个值配置为匹配：

```json
{
  "callbackUrl": "http://127.0.0.1:4321/callback/registered",
  "callbackPort": 4321
}
```

对于远程入口或其他代理，当代理转发到配置的侦听器时，回调 URL 端口和本地侦听器端口可以故意不同。

<a id="examples-of-useful-mcp-servers"></a>

## 有用的 MCP 服务器示例

MCP 服务器的列表不断增长。以下是一些常见的：

- [OpenAI 文档 MCP](https://developers.openai.com/learn/docs-mcp)：搜索并阅读 OpenAI 开发人员文档。
- [背景7](https://github.com/upstash/context7)：连接到最新的开发人员文档。
- Figma [本地](https://developers.figma.com/docs/figma-mcp-server/local-server-installation/) 和 [远程](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/)：访问您的 Figma 设计。
- [剧作家](https://www.npmjs.com/package/@playwright/mcp)：使用 Playwright 控制和检查浏览器。
- [Chrome 开发者工具](https://github.com/ChromeDevTools/chrome-devtools-mcp/)：控制和检查Chrome。
- [哨兵](https://docs.sentry.io/product/sentry-mcp/#codex)：访问Sentry日志。
- [GitHub](https://github.com/github/github-mcp-server)：管理超出 `git` 支持范围的 GitHub（例如，拉取请求和问题）。

</ContentModeSwitch>