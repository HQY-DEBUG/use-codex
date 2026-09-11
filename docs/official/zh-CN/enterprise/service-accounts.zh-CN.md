> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/service-accounts.md)。

<a id="service-accounts"></a>

# 服务账户

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

服务帐户允许您在整个组织中运行和扩展无头 Codex 工作流程，而无需依赖员工的帐户。每个持续集成 (CI) 运行程序、计划作业或共享集成都有自己的 ChatGPT 工作区身份，具有与您期望的人员相同的组、角色、访问控制和可审核性。

只有工作区所有者和管理员才能创建服务帐户。他们可以让其他人或团体管理帐户、配置插件或创建访问令牌。

服务帐户仅适用于即用即付计划。

服务帐户代表非人类工作区身份。 [个人访问令牌](access-tokens.zh-CN.md) 代表创建它的工作区成员。 API Platform 项目服务帐户和 API 密钥使用单独的项目访问和计费。

<a id="create-and-set-up-a-service-account"></a>

## 创建并设置服务帐户

此交互式演练使用 GitHub 作为示例：创建帐户、配置插件、创建令牌以及分配组和角色。

<ServiceAccountsDemo client:load guided />

1. 在工作区设置中打开 [服务账户](https://chatgpt.com/admin/service-accounts)。
2. 选择加号 (**+**) 按钮并输入描述性名称，例如 `release-automation`。
3. 选择**创建**。

<a id="connect-a-plugin"></a>

## 连接插件

为服务帐户本身配置插件。它不会继承创建者的插件或连接的应用程序。

1. 打开帐户的 **插件** 部分并选择 **添加插件**。
2. 选择一个插件并确认它显示为已配置或已启用。

**配置** 和 **经理** 角色可以设置插件。 **用户** 角色不能。

<a id="create-an-access-token"></a>

## 创建访问令牌

从服务帐户的详细信息页面创建令牌。令牌代表服务帐户，而不是创建它的人。

1. 打开账户，在**访问令牌**中选择**创建Token**。
2. 为令牌命名，确认 **Codex** 范围，然后选择到期时间。
3. 选择 **创建** 并将令牌保存在您的秘密管理器中。

完整令牌仅出现一次。工作区策略控制哪些到期时间可用。

<a id="assign-roles-and-groups"></a>

## 分配角色和组

服务帐户可以接收工作区角色并像工作区成员一样加入组。直接分配其访问权限；它不继承创建者的权限。

要让人员或组管理帐户，请选择 **分享**，然后选择 **添加人员或群组**，然后分配角色：

| 共享帐户角色 | 配置帐户及其插件 | 创建服务帐户访问令牌 |
| ------------------- | ------------------------------------- | ------------------------------------ |
| **用户** | 无 | 有 |
| **配置** | 是 | 否 |
| **经理** | 是 | 是 |

这些角色适用于管理帐户的人员。它们与分配给服务帐户的工作区角色和组是分开的。

**配置**和**经理**可以启用或禁用帐户。只有工作区所有者和管理员可以创建、删除或共享帐户。操作员在登录自己的 ChatGPT 帐户时管理共享帐户。

有关工作区权限的更多信息，请参阅 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="run-codex-without-signing-in"></a>

## 无需登录运行Codex

服务帐户访问令牌需要 Codex CLI 版本 `0.142.0` 或更高版本。设置`CODEX_ACCESS_TOKEN`并在不打开浏览器的情况下运行Codex：

```bash
export CODEX_ACCESS_TOKEN="<service-account-access-token>"
codex exec --json "Inspect this repository and summarize its current state."
```

在 CI 中，通过秘密管理器或运行者秘密提供令牌。

要在受信任的计算机上保存登录信息，请通过标准输入传递令牌：

```bash
printf '%s' "$CODEX_ACCESS_TOKEN" | codex login --with-access-token
codex exec "Summarize the changes in the current branch."
```

这会将凭证保存在本地。在共享或临时运行器上，使用 `CODEX_ACCESS_TOKEN` 而不保存登录信息。

<a id="provision-service-accounts-with-scim"></a>

## 使用 SCIM 配置服务帐户

如果您的工作区支持通过跨域身份管理系统 (SCIM) 协议配置服务帐户，请在身份提供程序中将 `userType` 设置为 `ServiceAccount`：

```json
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "userName": "svc-codex-release@company.example",
  "displayName": "Codex release automation",
  "active": true,
  "userType": "ServiceAccount"
}
```

将身份分配给工作区和所需组，然后同步。身份提供者管理帐户的名称、组成员资格和生命周期。 SCIM 管理的帐户无法在 ChatGPT 中重命名或删除。参见 [组和配置](groups-and-provisioning.zh-CN.md)。

<a id="manage-service-accounts-with-the-admin-api"></a>

## 使用 Admin API 管理服务帐户

如果您的工作区具有访问权限，请使用 ChatGPT 管理 API 密钥来管理帐户、令牌和共享。读操作需要`chatgpt.enterprise.service_account.read`；更改需要 `chatgpt.enterprise.service_account.write`。服务帐户令牌无法对管理 API 请求进行身份验证。

检查 [管理 API 参考](https://chatgpt.com/public/admin/api-reference) 的可用操作和当前请求路径。

<a id="accounts"></a>

### 账户

| 操作 | 方法 | 作用 |
| ---------------------------- | -------- | ------------------------------------------ |
| 列出帐户 | `GET` | 返回工作区服务帐户 |
| 创建帐户 | `POST` | 创建命名服务帐户 |
| 获取一个账户 | `GET` | 返回一个服务账户 |
| 启用或禁用帐户 | `PATCH` | 更新帐户的 `enabled` 值 |
| 删除账户 | `DELETE` | 删除账户并撤销其Token |

使用 `POST /v1/manage/workspaces/{workspace_id}/service-accounts` 创建帐户。帐户更新仅更改 `enabled`。

<a id="tokens"></a>

### Token

| 操作 | 方法 | 作用 |
| -------------- | -------- | ------------------------------------ |
| 列出Token | `GET` | 返回账户的Token元数据 |
| 创建令牌 | `POST` | 创建范围访问令牌 |
| 撤销一个Token | `DELETE` | 永久撤销一个Token |

例如，创建一个 30 天后过期的 Codex 令牌：

```json
{
  "name": "production-release-runner",
  "ttl": 2592000,
  "scopes": ["chatgpt.workspace.feature.allow-codex-local-access.access"]
}
```

`ttl` 是令牌生命周期（以秒为单位）。有限生命周期必须少于一年，并遵循工作区的过期政策。仅当创建令牌时才会返回完整的 `access_token`。

管理 API 还可以列出、添加、更新和删除共享帐户访问权限。其角色值为`manager`、`configurer`、`user`； `configurer` 在 ChatGPT 中显示为 **配置**。

<a id="secure-and-manage-service-accounts"></a>

## 保护和管理服务帐户

- 仅授予工作流所需的角色、组、插件和连接。
- 将令牌存储在秘密管理器中并使用受信任的运行者。
- 将凭据保留在日志、聊天消息和源代码控制之外。
- 设置有限的有效期并定期检查帐户访问和活动。
- 通过创建替换令牌、更新工作流程、验证访问权限以及撤销工作区或管理 API 中的旧令牌来轮换令牌。
- 立即撤销暴露的Token并调查该帐户的近期活动。
- 禁用或删除工作区或管理 API 中未使用的帐户。这两个操作都会撤销所有活动令牌。禁用的帐户可以使用新的令牌重新启用；删除无法撤消。

运行归因于服务帐户。可用的工作区分析和审计记录还可以识别谁创建了令牌或更改了帐户设置。确认 [管理 API 参考](https://chatgpt.com/public/admin/api-reference) 中的事件报道。

<a id="related-docs"></a>

## 相关文档

- [认证](../auth.zh-CN.md)
- [个人访问令牌](access-tokens.zh-CN.md)
- [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)
- [组和配置](groups-and-provisioning.zh-CN.md)
- [治理](governance.zh-CN.md)
- [合规性 API 和审核事件](compliance-api.zh-CN.md)
- [非交互模式](../non-interactive-mode.zh-CN.md)