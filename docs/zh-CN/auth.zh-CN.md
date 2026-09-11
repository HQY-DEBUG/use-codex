> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/auth.md)。

<a id="authentication"></a>

# 身份验证

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="openai-authentication"></a>

## OpenAI认证

<a id="sign-in-with-chatgpt"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

Codex 在使用 OpenAI 模型时支持两种登录方式：

- 使用 ChatGPT 登录以进行订阅访问
- 使用 API 密钥登录以进行基于使用情况的访问

ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展支持本地工作的两种登录方法。 Codex云需要使用ChatGPT登录。

您的登录方法还决定了适用哪些管理控制和数据处理策略。

- 当您使用 ChatGPT 登录时，Codex 的使用遵循您的 ChatGPT 工作区权限、基于角色的访问控制 (RBAC) 以及 ChatGPT Enterprise 保留和驻留设置。
- 对于 API 密钥，使用情况将遵循 API 组织的保留和数据共享设置。

对于托管工作区，身份验证只是一层访问。工作区成员身份和配置决定了谁可以登录，而席位和工作区角色则决定了他们可以使用哪些产品界面和功能。对于 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中的本地工作，权限配置文件限制智能体可以在设备上执行的操作。请参阅 [组和配置](enterprise/groups-and-provisioning.zh-CN.md) 和 [角色和工作区权限](enterprise/roles-and-workspace-permissions.zh-CN.md) 来规划这些控制。

<a id="sign-in-with-chatgpt"></a>

### 使用 ChatGPT 登录

当您从 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中使用 ChatGPT 登录时，登录流程将打开一个浏览器窗口。登录后，浏览器会将您的凭据返回到 Codex。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="chatgpt-web"></a>

### ChatGPT 网页

打开 [ChatGPT](https://chatgpt.com)，登录并选择您想要工作的工作区。 ChatGPT web 将经过身份验证的会话保留在您的浏览器中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="chatgpt-desktop-app"></a>

#### ChatGPT 桌面应用程序

在注销屏幕上，选择 **继续登录**，然后完成浏览器流程。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="codex-cli"></a>

#### Codex CLI

运行 `codex login`，然后完成浏览器流程。当没有有效会话可用时，这是默认身份验证路径。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="ide-extension"></a>

#### IDE扩展

在注销屏幕上，选择 **使用 ChatGPT 登录**，然后完成浏览器流程。

</ContentModeSwitch>

<a id="sign-in-with-an-api-key"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="sign-in-with-an-api-key"></a>

### 使用 API 密钥登录

您还可以使用 API 密钥登录 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展。从 [OpenAI 仪表板](https://platform.openai.com/api-keys) 获取您的 API 密钥。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="chatgpt-desktop-app"></a>

#### ChatGPT 桌面应用程序

在注销屏幕上，选择 **以其他方式登录**，输入密钥，然后选择 **继续**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="codex-cli"></a>

#### Codex CLI

通过 stdin 将密钥传输到 `codex login`：

```shell
printenv OPENAI_API_KEY | codex login --with-api-key
```

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="ide-extension"></a>

#### IDE扩展

在注销屏幕上，选择 **使用API密钥**，输入密钥，然后选择 **好的**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

OpenAI 通过您的 OpenAI 平台帐户按标准 API 费率对 API 密钥使用情况进行计费。请参阅 [API定价页面](https://openai.com/api/pricing/)。

API 密钥身份验证支持本地 Codex 工作流程，但某些依赖于 ChatGPT 工作区访问或云服务的功能受到限制或不可用。比较 [功能可用性](pricing.zh-CN.md#feature-availability) 中计划的支持。

在 Codex CLI 和 ChatGPT 桌面应用程序中的 Codex 中，API 密钥身份验证包括对受支持的 OpenAI 策划的插件的访问。某些插件不可用，因为它们的连接流需要不受支持的 OAuth 功能。参见 [使用插件](plugins.zh-CN.md#api-key-availability)。

当您使用 API 密钥登录时，Codex 使用标准 API 定价，而不是包含的 ChatGPT 计划额度。

对编程式 Codex CLI 工作流程（例如 CI/CD 作业）使用 API 密钥身份验证。不要在不受信任或公共环境中公开 Codex 执行。

</ContentModeSwitch>

<a id="check-authentication-or-sign-out"></a>

### 检查身份验证或退出

<ContentModeSwitch group="codex-surface" id="web">

打开配置文件菜单以确认活动帐户和工作区。要结束该浏览器中的 ChatGPT Web 会话，请选择 **退出**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

打开配置文件菜单以查看活动帐户或 API 密钥状态。选择 **退出** 以清除当前凭据。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

运行 `codex login status` 查看主动身份验证方法。对于存储的身份验证，请运行 `codex logout` 以清除当前凭据。当进程选择工作负载身份时，Codex 会拒绝 `codex login` 和 `codex logout`，因为进程环境控制身份验证。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

打开配置文件菜单以查看活动帐户或 API 密钥状态。选择 **退出** 以清除当前凭据。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="use-codex-access-tokens-for-enterprise-automation"></a>

### 使用 Codex 访问令牌实现企业自动化

在 ChatGPT Enterprise 工作区中，管理员可以授予访问令牌权限，以便允许的成员可以为受信任的非交互式 Codex 本地工作流程创建 Codex 访问令牌。当自动化需要 ChatGPT 工作区访问权限、ChatGPT 管理的 Codex 权利或无需浏览器登录的企业工作区控制时，请使用访问令牌。

访问令牌适用于受信任的脚本、调度程序和私有 CI 运行程序。对于一般 OpenAI API 调用，继续使用平台 API 密钥。

有关设置步骤、权限、轮换和撤销指南，请参阅 [访问令牌](enterprise/access-tokens.zh-CN.md)。

如果您的云平台、CI 系统或集群已颁发短期工作负载令牌，请使用 [工作负载身份联合](enterprise/workload-identity.zh-CN.md) 而不是存储 OpenAI 凭证。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

如果您的环境已提供 Codex 访问令牌，请将其通过管道传输到 CLI：

```shell
printenv CODEX_ACCESS_TOKEN | codex login --with-access-token
```

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="secure-your-codex-cloud-account"></a>

## 保护您的 Codex 云帐户

Codex 云直接与您的代码库交互，因此它需要比许多其他 ChatGPT 功能更强的安全性。启用多重身份验证 (MFA)。

如果您使用社交登录提供商（Google、Microsoft、Apple），则无需在 ChatGPT 帐户上启用 MFA，但您可以使用社交登录提供商进行设置。

有关设置说明，请参阅：

- [谷歌](https://support.google.com/accounts/answer/185839)
- [微软](https://support.microsoft.com/en-us/topic/what-is-multifactor-authentication-e5e39437-121c-be60-d123-eda06bddf661)
- [苹果](https://support.apple.com/en-us/102660)

如果您通过单点登录 (SSO) 访问 ChatGPT，则您组织的 SSO 管理员应为所有用户强制执行 MFA。

如果您使用电子邮件和密码登录，则必须先在您的帐户上设置MFA，然后才能访问Codex云。

如果您的帐户支持多种登录方式，并且其中一种是电子邮件和密码，则即使您以其他方式登录，您也必须在访问 Codex 之前设置 MFA。

</ContentModeSwitch>

<a id="login-caching"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="login-caching"></a>

## 登录缓存

当您使用 ChatGPT 或 API 密钥登录 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展时，您的登录详细信息将被缓存并重复使用。 CLI 和扩展共享相同的缓存登录详细信息。如果您从其中之一注销，则下次启动 CLI 或扩展时需要重新登录。

Codex 将登录详细信息本地缓存在 `~/.codex/auth.json` 的纯文本文件中或操作系统特定的凭证存储中。

对于使用 ChatGPT 会话登录，Codex 在使用过程中会在令牌过期之前自动刷新令牌，因此活动会话通常会继续，无需再次登录浏览器。

</ContentModeSwitch>

<a id="credential-storage"></a>
<a id="enforce-a-login-method-or-workspace"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="credential-storage"></a>

## 凭证存储

使用 `cli_auth_credentials_store` 控制 Codex CLI 存储缓存凭证的位置：

```toml
# 文件 | 钥匙圈 | 汽车 | 临时
cli_auth_credentials_store = "keyring"
```

- `file` 将凭证存储在 `CODEX_HOME` 下的 `auth.json` 中（默认为 `~/.codex`）。
- `keyring` 将凭据存储在操作系统凭据存储中，如果不可用，则会失败。
- `auto` 在可用时使用操作系统凭证存储，否则回退到 `auth.json`。
- `ephemeral` 仅在内存中保留当前进程的凭据。

有关完整的 `config.toml` 架构，请参阅 [配置参考](config-file/config-reference.zh-CN.md)。

管理员可以通过 [本地身份验证要求](enterprise/managed-configuration.zh-CN.md#manage-authentication-locally) 强制执行 `cli_auth_credentials_store` 和 `chatgpt_base_url`。用户无法通过 `config.toml` 或 CLI 覆盖来覆盖这些要求。

如果您使用基于文件的存储，请将 `~/.codex/auth.json` 视为密码：它包含访问令牌。不要提交、粘贴到票证中或在聊天中分享。

<a id="enforce-a-login-method-or-workspace"></a>

## 强制执行登录方法或工作区

在托管环境中，管理员可能会限制允许用户进行身份验证的方式：

```toml
# 仅允许 ChatGPT 登录或仅允许 API 密钥登录。
forced_login_method = "chatgpt" # or "api"

# 使用 ChatGPT 登录时，将用户限制在特定工作区。
forced_chatgpt_workspace_id = "00000000-0000-0000-0000-000000000000"
```

如果活动凭据与配置的限制不匹配，Codex 会注销用户并退出。

这些设置也可以通过旧版托管配置提供。有关管理员强制执行的登录限制，请参阅 [本地管理身份验证](enterprise/managed-configuration.zh-CN.md#manage-authentication-locally)。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="login-diagnostics"></a>

## 登录诊断

直接 `codex login` 运行在您配置的日志目录下写入专用的 `codex-login.log` 文件。当您需要调试浏览器登录或设备代码故障时，或者当支持人员要求提供特定于登录的日志时，请使用它。

<a id="custom-ca-bundles"></a>

## 自定义 CA 捆绑包

如果您的网络使用公司 TLS 代理或私有根 CA，请在登录前将 `CODEX_CA_CERTIFICATE` 设置为 PEM 捆绑包。当 `CODEX_CA_CERTIFICATE` 未设置时，Codex 回退到 `SSL_CERT_FILE`。相同的自定义 CA 设置适用于登录、正常 HTTPS 请求和安全 WebSocket 连接。

```shell
export CODEX_CA_CERTIFICATE=/path/to/corporate-root-ca.pem
codex login
```

<a id="login-on-headless-devices"></a>

## 在无头设备上登录

如果您使用 Codex CLI 登录 ChatGPT，在某些情况下基于浏览器的登录 UI 可能无法工作：

- 您正在远程或无头环境中运行 CLI。
- 您的本地网络配置会阻止 Codex 登录后用于将 OAuth 令牌返回到 CLI 的本地主机回调。

在这些情况下，首选设备代码身份验证（测试版）。在交互式登录界面中，选择**使用设备代码登录**，或者直接运行`codex login --device-auth`。如果设备代码身份验证在您的环境中不起作用，请使用其中一种后备方法。

<a id="preferred-device-code-authentication-beta"></a>

### 首选：设备代码身份验证（测试版）

1. 在 ChatGPT 安全设置（个人帐户）或 ChatGPT 工作区权限（工作区管理员）中启用设备代码登录。
2. 在运行 Codex 的终端中，选择以下选项之一：
   - 在交互式登录 UI 中，选择 **使用设备代码登录**。
   - 运行`codex login --device-auth`。
3. 在浏览器中打开链接，登录，然后输入一次性代码。

如果设备代码登录在您的环境中不可用，请使用以下后备方法之一。

<a id="fallback-authenticate-locally-and-copy-your-auth-cache"></a>

### 后备：在本地进行身份验证并复制您的身份验证缓存

如果您可以使用浏览器在计算机上完成登录流程，则可以将缓存的凭据复制到无头计算机。

1. 在可以使用基于浏览器的登录流程的计算机上，运行 `codex login`。
2. 确认 `~/.codex/auth.json` 处存在登录缓存。
3. 在无头机器上将`~/.codex/auth.json`复制到`~/.codex/auth.json`。

将 `~/.codex/auth.json` 视为密码：它包含访问令牌。不要提交、粘贴到票证中或在聊天中分享。

如果您的操作系统将凭据存储在凭据存储中而不是 `~/.codex/auth.json` 中，则此方法可能不适用。有关如何配置基于文件的存储，请参阅 [凭证存储](auth.zh-CN.md#credential-storage)。

通过 SSH 复制到远程计算机：

```shell
ssh user@remote 'mkdir -p ~/.codex'
scp ~/.codex/auth.json user@remote:~/.codex/auth.json
```

或者使用避免 `scp` 的单行：

```shell
ssh user@remote 'mkdir -p ~/.codex && cat > ~/.codex/auth.json' < ~/.codex/auth.json
```

复制到 Docker 容器中：

```shell
# 将 MY_CONTAINER 替换为容器的名称或 ID。
CONTAINER_HOME=$(docker exec MY_CONTAINER printenv HOME)
docker exec MY_CONTAINER mkdir -p "$CONTAINER_HOME/.codex"
docker cp ~/.codex/auth.json MY_CONTAINER:"$CONTAINER_HOME/.codex/auth.json"
```

有关受信任 CI/CD 运行程序上相同模式的更高级版本，请参阅 [在 CI/CD 中维护 Codex 帐户身份验证（高级）](https://learn.chatgpt.com/docs/auth/ci-cd-auth)。该指南解释了如何让 Codex 在正常运行期间刷新 `auth.json`，然后保留更新的文件以供下一个作业使用。 API 密钥仍然是推荐的自动化默认值。

<a id="fallback-forward-the-localhost-callback-over-ssh"></a>

### 后备：通过 SSH 转发 localhost 回调

如果您可以在本地计算机和远程主机之间转发端口，则可以通过隧道连接 Codex 的本地回调服务器（默认 `localhost:1455`）来使用基于标准浏览器的流程。

1. 从本地计算机启动端口转发：

```shell
ssh -L 1455:localhost:1455 user@remote
```

2. 在该 SSH 会话中，运行 `codex login` 并按照本地计算机上打印的地址进行操作。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="alternative-model-providers"></a>

## 替代模型提供商

当您在配置文件中定义 [定制模型提供商](config-file/config-advanced.zh-CN.md#custom-model-providers) 时，您可以选择以下身份验证方法之一：

- **OpenAI认证**：设置`requires_openai_auth = true`使用OpenAI认证。然后，您可以使用 ChatGPT 或 API 密钥登录。当您通过 LLM 代理服务器访问 OpenAI 模型时，这非常有用。当`requires_openai_auth = true`时，Codex忽略`env_key`。
- **环境变量认证**：设置`env_key =“<ENV_VARIABLE_NAME>“` to use a provider-specific API key from the local environment variable named `<ENV_VARIABLE_NAME>`.
- **没有认证**：如果您未设置 `requires_openai_auth`（或将其设置为 `false`）并且未设置 `env_key`，则 Codex 假定提供程序不需要身份验证。这对于本地模型很有用。

</ContentModeSwitch>