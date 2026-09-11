> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/workload-identity.md)。

<a id="workload-identity-federation"></a>

# 工作负载身份联合

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

工作负载身份联合允许可信自动化使用 Codex，而无需存储个人访问令牌或其他长期 OpenAI 凭证。您的工作负载提供了来自您已经运营的提供商的短期身份令牌。 OpenAI 验证该令牌并为托管 ChatGPT 工作区中的用户或服务帐户返回短期访问令牌。

将工作负载身份用于云平台、Kubernetes、CI 系统以及其他可以颁发 OIDC 令牌或 SPIFFE JWT-SVID 的环境中的无人值守 Codex 进程。有关共享信任模型和单独的 OpenAI API 流程，请参阅 [工作负载身份概述](https://developers.openai.com/api/docs/guides/workload-identity-federation)。

Codex 工作负载联合身份验证处于测试阶段，必须为您的工作区启用。要请求访问，请联系您的 OpenAI 代表或 [OpenAI 支持](https://help.openai.com/en/articles/6614161-how-can-i-contact-support)。

<a id="before-you-begin"></a>

## 开始之前

您需要：

- 在 OpenAI 管理门户中管理工作负载身份的权限。
- 托管 ChatGPT 工作区。
- 作为该工作区的活动成员的 ChatGPT 用户或服务帐户，或者在安装过程中创建的权限。
- 您了解其发行者、受众和识别声明的 OIDC 令牌或 SPIFFE JWT-SVID。
- 可以将该令牌保持在受保护文件中的绝对路径中的当前运行时。
- Codex 0.148.0 或更高版本。
- 有效的 Codex 身份验证策略，允许 ChatGPT 身份验证和联合规则选择的工作区。参见 [强制执行登录方法或工作区](../auth.zh-CN.md#enforce-a-login-method-or-workspace)。

OpenAI 在令牌交换期间不会创建主体或工作区成员身份。管理员在工作负载连接之前选择或创建主体。创建人类用户会占用工作区席位并遵循该工作区的成员资格规则。

在本机 Windows 上，使用 **高架** [Windows沙箱](../windows/windows-sandbox.zh-CN.md)。其他 Windows 沙箱模式无法保护身份令牌文件免受模型控制命令的影响。

<a id="get-an-identity-token"></a>

## 获取身份令牌

您的工作负载运行时获取并刷新上游身份令牌。 Codex 不会代表您调用云元数据服务或身份提供商客户端库。

| 运行时 | 推荐的令牌文件源 |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Kubernetes、AKS、EKS 或 GKE | 挂载预计的服务帐户令牌并将 Codex 指向该文件。平台旋转它。                                  |
| Microsoft Entra 托管标识 | 运行受信任的主机进程或 sidecar，从 Azure IMDS 请求令牌并在到期前替换文件。                |
| AWS 出站身份联合 | 运行受信任的主机进程，该进程调用区域 STS `GetWebIdentityToken` 并在到期前替换文件。                   |
| Google Cloud | 运行受信任的主机进程，该进程从元数据服务器请求身份令牌并在到期前替换文件。        |
| Oracle Cloud Infrastructure | 运行受信任的主机进程，该进程使用实例主体请求 IDCS 访问令牌并在到期前替换该文件。 |
| GitHub 操作 | 请求作业的 OIDC 令牌，将其写入受保护的文件，并在以后交换之前请求新令牌。                    |
| SPIFFE | 使用 SPIFFE 工作负载 API 或批准的帮助程序将当前 JWT-SVID 写入文件。                                      |
| 自定义 OIDC 提供商 | 使用颁发者的工作负载流获取 JWT，然后在 JWT 过期之前刷新受保护的文件。                            |

按照提供商的指南配置令牌发行并检查示例令牌：

- [微软Azure](https://developers.openai.com/api/docs/guides/workload-identity-federation/microsoft-azure)
- [AWS](https://developers.openai.com/api/docs/guides/workload-identity-federation/aws)
- [谷歌云](https://developers.openai.com/api/docs/guides/workload-identity-federation/google-cloud)
- [Oracle 云基础设施](https://developers.openai.com/api/docs/guides/workload-identity-federation/oracle-cloud)
- [GitHub 行动](https://developers.openai.com/api/docs/guides/workload-identity-federation/github-actions)
- [库伯内斯](https://developers.openai.com/api/docs/guides/workload-identity-federation/kubernetes)
- [斯皮夫](https://developers.openai.com/api/docs/guides/workload-identity-federation/spiffe)

在本地解码示例令牌并记录其 `iss`、`aud`、`sub` 以及您计划信任的任何其他声明。解码不验证签名。请勿将生产令牌粘贴到网站或将其写入日志。

<a id="connect-the-workload"></a>

## 连接工作负载

管理员在启动 Codex 之前创建提供程序和联合规则。

1. 在 OpenAI 管理门户中打开 [工作负载身份](https://admin.openai.com/workload-identity)，然后选择 **连接工作负载**。
2. 重复使用为 Codex 配置的提供程序，或创建一个。提供商预设填写 GitHub Actions、Microsoft Entra ID、Google Cloud、AWS、Kubernetes、SPIFFE 和自定义 OIDC 提供商的常见设置。
3. 选择 **Codex** 和工作负载可能使用的托管工作区。
4. 添加识别工作负载的最窄条件。匹配主题、确切声明、CEL 条件或组合。添加接受的受众以限制规则接受的令牌。每个配置的匹配器都必须通过。
5. 将规则映射到一个现有的 ChatGPT 用户或服务帐户，或在设置过程中创建一个。
6. 查看提供程序、条件、工作区、主体、范围和访问令牌生命周期。选择 **连接工作负载**，然后选择 **下载配置**。

下载的文件包含非秘密联合规则 ID 以及 Codex 将读取身份令牌的路径。它不包含凭据。

要自动设置，请使用 [工作负载身份管理 API](https://developers.openai.com/api/docs/guides/workload-identity-federation/admin-api)。有关匹配器行为和示例，请参阅 [联邦规则参考](https://developers.openai.com/api/docs/guides/workload-identity-federation/federation-rules)。

<a id="configure-the-codex-process"></a>

## 配置Codex进程

启动 Codex 的进程需要这两个工作负载标识变量：

```bash
export OPENAI_FEDERATION_RULE_ID="idpm_..."
export OPENAI_IDENTITY_TOKEN_FILE="/var/run/secrets/openai.com/identity-token"
```

`OPENAI_FEDERATION_RULE_ID` 不是秘密。令牌文件是。使用专用目录中的绝对路径，例如由模式为 `0700` 的工作负载帐户拥有的 `/var/run/secrets/openai.com`。只有受信任的主机进程才应该在那里写入。将目录保留在仓库和 Codex 工具可用的其他路径之外。将凭证保留在日志、shell 历史记录和构建工件之外。

<a id="add-audit-attribution"></a>

### 添加审核归因

当运行时实例共享联合规则时，您可以在令牌颁发审核事件中识别每个实例。将可选的 `OPENAI_WORKLOAD_IDENTITY_CONTEXT` 变量设置为编码为字符串的 JSON 对象：

```bash
export OPENAI_WORKLOAD_IDENTITY_CONTEXT='{
  "instance_id": "runner-42",
  "display_name": "payments-prod",
  "labels": {
    "environment": "production",
    "region": "us-west-2"
  }
}'
```

该对象需要 `instance_id`。它还可以包含 `display_name` 和最多八个标签。编码对象最多可达 1,024 字节。 `instance_id` 和 `display_name` 最多可达 128 个字符。标签键最多可包含 64 个字符，标签值最多可包含 256 个字符。

标识符必须以 ASCII 字母或数字开头。然后，值可以包含字母、数字、`.`、`_`、`:`、`/`、`@` 和 `-`。标签键支持字母、数字、`.`、`_` 和 `-`。

OpenAI 将此上下文视为客户端报告的审计归因，而不是经过验证的工作负载身份。它不会影响身份验证、授权、规则匹配、范围、速率限制、撤销、功能门或指标。请勿将凭据、机密、个人数据、提示、模型输出或其他客户内容放入其中。

对于有效的上下文，OpenAI 派生出一个稳定的归因 ID，其范围为租户、提供商、联合规则和 `instance_id`。对于归属，访问令牌包含 ID，但不包含上下文。成功的令牌颁发审核事件包含 ID 和规范化上下文。超出限制或违反此架构的上下文会使交换失败并显示 `invalid_grant`。

Codex 在进程启动时读取上下文，并且不将其、规则 ID 或令牌文件路径传递给模型控制的 shell、挂钩或 MCP 服务器。更改上下文后重新启动 Codex。

<a id="protect-and-rotate-the-token-file"></a>

### 保护和轮换令牌文件

对于托管 Linux、macOS 和 WSL 部署，请将整个令牌目录添加到托管要求中的 [`permissions.filesystem.deny_read`](managed-configuration.zh-CN.md#enforce-deny-read-requirements)：

```toml
[permissions.filesystem]
deny_read = ["/var/run/secrets/openai.com"]
```

这会阻止模型控制命令读取活动令牌或临时替换，而 Codex 主机进程仍然可以使用令牌进行交换。对于预计令牌卷，拒绝整个令牌挂载及其外部的任何后备或已解析的目标路径。仅文件模式和环境变量清理并不能保护凭据免受以同一用户身份运行的另一个进程的影响。在本机 Windows 上，使用上述提升的沙箱。

对于不投影文件的令牌源，请让受信任的主机进程将每个替换写入该受保护的目录中并将其重命名到位。原子重命名可防止 Codex 读取部分令牌。例如，使此主机拥有的刷新脚本适应您的提供商的令牌命令。在运行脚本之前配置目录：

```bash
set -eu
TOKEN_DIR="/var/run/secrets/openai.com"
TOKEN_FILE="$TOKEN_DIR/identity-token"
umask 077
TOKEN_TEMP="$(mktemp "$TOKEN_DIR/.identity-token.XXXXXX")"
trap 'rm -f -- "$TOKEN_TEMP"' EXIT
trap 'exit 1' HUP INT TERM
your-identity-provider-command > "$TOKEN_TEMP"
test -s "$TOKEN_TEMP"
mv -f -- "$TOKEN_TEMP" "$TOKEN_FILE"
```

在 Codex 可以控制的任何 shell 或工具之外运行刷新过程。在刷新和清理期间保持读取拒绝。即使强制停止留下临时文件，该文件也必须保留在被拒绝的目录中。不要将工作负载身份设置放在 `config.toml` 中。

<a id="verify-the-connection"></a>

## 验证连接

加载下载的环境并检查所选的身份验证方法：

```bash
. ./workload-identity-idpm_example.env
codex login status
```

在 PowerShell 中：

```powershell
$env:OPENAI_FEDERATION_RULE_ID = "idpm_..."
$env:OPENAI_IDENTITY_TOKEN_FILE = "C:\run\openai\identity-token"
codex login status
```

成功的支票将打印 `Logged in using workload identity`。这确认了 Codex 通过配置的联合规则交换了令牌。该命令不会打印已解析的工作区、主体或规则。在启动工作负载之前，请在管理门户中确认这些值。如果 Codex 报告另一种身份验证方法，则两个所需的 WIF 变量未到达该进程。

如果提供者使用 **防止断言重放** 并且断言具有 `jti` 声明，则此检查会消耗该 `jti`。在启动另一个 Codex 进程之前，使用新的 `jti` 写入新发出的断言。

从同一环境运行一个小请求：

```bash
codex exec "Reply with only: workload identity is working"
```

Codex 交换上游令牌并将 OpenAI 访问令牌保留在内存中。它不会将凭证写入 `auth.json`、系统密钥环或 `config.toml`。

<a id="keep-the-token-current"></a>

## 保持令牌最新

在上游令牌过期之前刷新身份令牌文件。Codex当需要另一个文件时重新读取该文件OpenAI访问令牌。这OpenAI令牌在上游令牌的到期时间或联合规则的生命周期中较早的时间到期，并且持续时间绝不会超过一小时。

当管理员打开重放保护时，每个上游 JWT 必须具有唯一的 `jti`。在每次交换之前使用新的 `jti` 写入新发出的断言，包括长时间运行的进程中的刷新。没有 `jti` 的断言不会受到重播保护。

Codex 在每个主机进程内共享一个内存中交换会话。该进程中的并发请求重用有效的 OpenAI 访问令牌，并在其过期时共享一次刷新。单独的进程执行单独的交换，因此它们需要提供者允许它们使用的断言。

<a id="credential-precedence"></a>

## 凭证优先级

两个必需的工作负载身份变量优先于所有其他凭证源：

1. 如果存在 `OPENAI_FEDERATION_RULE_ID` 或 `OPENAI_IDENTITY_TOKEN_FILE`，则 Codex 选择工作负载标识。
2. 如果仅存在一个必需的变量，Codex 将返回错误。它不会回退到 API 密钥、访问令牌或存储的登录信息。
3. `OPENAI_WORKLOAD_IDENTITY_CONTEXT` 本身不选择工作负载身份。
4. 当所需的 WIF 变量都不存在时，Codex 将应用该使用界面的正常凭证规则。对于允许 API 密钥身份验证的使用界面，`CODEX_API_KEY` 优先于 `codex exec`、`codex review`、TypeScript SDK 和 `codex exec-server --remote`。其他使用界面可以使用 `CODEX_ACCESS_TOKEN` 或存储的登录名。

SDK `apiKey` 选项变为 `CODEX_API_KEY`，但当任一必需的 WIF 变量存在时，WIF 仍然优先。使用 WIF 时忽略该选项，以便工作负载不携带未使用的长期凭据。

要在不停机的情况下移动现有工作负载，请在当前凭据仍然可用时配置 WIF。使用两个必需的 WIF 变量启动一个新进程；即使旧凭据仍然存在，WIF 仍优先。工作负载通过 WIF 成功后，从其运行时和机密存储中删除旧凭据，然后吊销它。在撤销之前，您可以通过删除所需的 WIF 变量并启动新进程来回滚。

<a id="supported-codex-surfaces"></a>

## 支持的 Codex 使用界面

在拥有 Codex 进程的计算机上配置工作负载身份。

| 使用界面 | 支持和主机边界 |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| 交互式 `codex`、`resume` 和 `fork` | 支持。在配置的环境中启动 CLI。                                                 |
| `codex exec`、`exec resume` 和 `codex review` | 支持。任一必需的 WIF 变量都会使 WIF 优先。                                      |
| TypeScript SDK | 支持。父进程提供所需的 WIF 变量和任何可选的归因上下文。 |
| `codex app-server` | 支持。在应用程序服务器主机上配置 WIF，而不是在远程客户端上。                                |
| `codex exec-server --remote` | 支持对远程环境注册表进行身份验证。在 exec-server 主机上配置 WIF。 |
| 本地 exec-server 进程操作 | 不使用 WIF 身份验证。它们通过本地执行服务器协议运行。                         |

远程应用程序服务器和执行服务器客户端永远不会通过其协议发送上游身份令牌。

<a id="change-or-remove-access"></a>

## 更改或删除访问权限

对规则主题、受众、声明、CEL 条件、范围或Token生命周期的更改适用于新的交易所。更改之前颁发的令牌可以保持有效，直到其生命周期结束。

禁用提供程序或规则以立即停止访问。禁用会阻止新的交换并撤销已通过该资源发行的 OpenAI 访问令牌。归档具有相同的访问效果并且无法撤消。更改提供商信任还会在新信任生效之前撤销已发行的令牌。

<a id="audit-changes"></a>

## 审计变更

提供者和联合规则的创建、更新和归档会生成审核事件。使用 [合规 API 和审核事件指南](compliance-api.zh-CN.md) 导出工作区支持的事件。将它们与身份提供商的颁发日志相关联，并且不要在任一系统中记录上游断言或 OpenAI 访问令牌。

当进程提供 `OPENAI_WORKLOAD_IDENTITY_CONTEXT` 时，成功的令牌发行审核事件还包含上述稳定归因 ID 和规范化上下文。

<a id="troubleshoot"></a>

## 故障排除

| 症状 | 检查 |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Codex 报告不完整的工作负载身份配置 | 在同一进程中设置两个必需的变量并使用绝对令牌文件路径。                               |
| Codex 报告其登录策略不允许工作负载身份 | 在有效策略中允许 ChatGPT 身份验证，并将规则的工作区包含在其允许的工作区中。 |
| Codex 报告另一个凭据 | 将两个所需的 WIF 变量加载到 Codex 进程中，然后启动新进程并重新运行 `codex login status`。  |
| OpenAI 拒绝工作负载上下文 | 检查其 JSON 形状、大小、允许的字符和字段限制。删除敏感内容或客户内容。            |
| OpenAI 拒绝令牌 | 将 `iss`、`aud`、过期时间、签名密钥和断言生命周期与提供程序配置进行比较。               |
| 规则与 | 不匹配 确认客户端使用预期的规则 ID 并且每个主题、受众、精确声明和 CEL 检查都通过。  |
| OpenAI 拒绝主体 | 确认用户或服务帐户处于活动状态并且是所选工作区的活动成员。                   |
| OpenAI 拒绝重复断言 | 使用新的 `jti` 获取新的 JWT；不要重试相同的重放保护断言。                                  |
| 长时间运行的进程停止刷新 | 确认主机刷新进程仍在到期前替换令牌文件。                                  |

有关提供商验证、限制和 CEL 详细信息，请参阅 [联邦规则参考](https://developers.openai.com/api/docs/guides/workload-identity-federation/federation-rules)。