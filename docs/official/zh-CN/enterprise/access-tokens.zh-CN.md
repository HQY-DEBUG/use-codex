> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/access-tokens.md)。

<a id="access-tokens"></a>

# 访问令牌

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Codex 访问令牌是 ChatGPT 工作区凭证，范围为 Codex 权限。它们使用 ChatGPT 工作区身份验证受信任的非交互式本地工作流程，包括 Codex CLI 和基于应用程序服务器的自动化。当脚本、计划作业或 CI 运行程序需要可重复的本地访问时，请使用它们。

ChatGPT 商业和企业工作区当前支持 Codex 访问令牌。

在位于 [访问令牌](https://chatgpt.com/admin/access-tokens) 的 ChatGPT 管理控制台中创建个人访问令牌。每个令牌都属于其创建者和该用户的 ChatGPT 工作区。令牌充当编程本地工作流程的智能体身份。对于从专用非人类工作区身份的详细信息页面创建的令牌，请参阅 [服务账户](service-accounts.zh-CN.md)。

如果平台 API 密钥适用于您的自动化，请继续使用 API 密钥身份验证。当受信任的本地工作流特别需要 ChatGPT 工作区访问权限、工作区管理的权利或企业控制时，请使用 Codex 访问令牌。

需要从您自己的系统触发已发布的 ChatGPT 工作区智能体？该工作流程需要 **工作区智能体** 访问权限。仅 Codex 令牌无法验证工作区智能体触发器调用。如果您的令牌对话框提供 **范围**，请选择 **工作区智能体** 作为智能体触发器，选择 **Codex** 作为 Codex 自动化。仅当工作流需要每个范围时才授予多个范围。参见 [使用工作区智能体访问令牌进行身份验证](https://developers.openai.com/workspace-agents/authentication)。

<a id="how-access-tokens-work"></a>

## 访问令牌如何工作

当 Codex CLI 或应用程序服务器客户端需要在用户无需完成浏览器登录的情况下运行时，请使用访问令牌。该令牌代表创建它的 ChatGPT 工作区用户，因此运行可以使用该用户的访问权限并显示在工作区治理数据中。

客户端在运行开始时检查令牌，并将运行与该工作区标识联系起来。像对待任何其他自动化机密一样对待令牌：将其存储在机密管理器中，将其保留在日志之外，并根据组织的策略轮换它。

将访问令牌用于：

- 通过可信自动化运行的 `codex exec` 作业。
- 需要可重复、非交互式 Codex CLI 运行的本地脚本。
- 基于可信应用程序服务器的自动化。
- 将使用情况与 ChatGPT 工作区用户而不是 API 组织密钥关联的企业工作流。

应避免的主要风险：

- **泄露的秘密：** 任何拥有该令牌的人都可以作为令牌创建者通过 Codex CLI 或应用程序服务器客户端启动本地运行。将令牌存储在秘密管理器中，使它们远离日志，并根据组织的策略轮换它们。
- **跑者信任：** 公共 CI、分叉拉取请求或共享计算机可能会将令牌暴露给工作区之外的人员。仅在受信任的运行者上使用访问令牌。
- **共享身份：** 一个人的Token在不相关的团队中重复使用，使得所有权和审计跟踪变得不太清晰。为特定工作流程所有者创建令牌。
- **过时的凭证：** 长期令牌可以在工作流程更改后保持活动状态。优先选择有时间限制的令牌并撤销不再使用的令牌。
- **范围或凭证类型错误：** Codex 自动化需要 Codex 访问权限，工作区智能体触发器需要工作区智能体访问权限，一般 OpenAI API 调用需要平台 API 密钥。如果出现 **范围**，则仅授予工作流所需的权限。

<a id="enable-access-token-creation"></a>

## 启用访问令牌创建

使用工作区设置中的访问令牌权限为允许的成员启用访问令牌创建。

访问令牌权限控制令牌创建。它不会授予对 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展的访问权限，也不会更改成员的席位类型、内置工作区角色或本地运行时权限配置文件。经过令牌验证的 Codex CLI 和应用程序服务器工作流程还需要用户的本地 Codex 权限。

这些控件之间的关系请参见[角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。


  

> 插图：ChatGPT 工作区 RBAC 设置中的访问令牌访问权限




1. 让工作区所有者打开 [工作区设置 > 权限和角色](https://chatgpt.com/admin/permissions)。
2. 如果出现 **访问令牌** 部分，请启用 **允许用户创建个人访问令牌**。如果该部分不可用，请在 **Codex 和工作本地** 或 **Codex 本地** 中启用 **允许成员使用 Codex 访问令牌**。
3. 为工作流所有者启用相应的本地Codex权限：**Codex 和工作本地**中的**允许成员使用 Codex 并在本地工作**，或**Codex 本地**中的**允许会员本地使用Codex**。当 **本地工作** 有自己的部分时，**在本地使用工作** 控制工作，而 Codex 令牌不需要。

仅允许了解令牌存储位置、预期自动化和轮换计划的人员或服务所有者创建访问令牌。

禁用本地 Codex 权限会暂停受影响成员拥有的活跃 Codex Token；它不会撤销它们。恢复本地 Codex 访问会重新激活这些令牌。当必须永久终止访问权限时，撤销令牌。

<a id="set-an-access-token-expiration-limit"></a>

## 设置访问令牌过期限制

工作区所有者可以设置成员可以为新访问令牌选择的最长有效期窗口。打开[工作区设置 > 权限和角色](https://chatgpt.com/admin/permissions)。如果出现 **访问令牌** 部分，请在那里设置 **访问令牌过期限制**。否则，请在 **Codex 和工作本地** 或 **Codex 本地** 中查找该设置。


  

> 插图：ChatGPT 工作区权限设置中的访问令牌过期限制




该限制适用于新的访问令牌。现有Token保留其当前的有效期窗口。

<a id="create-an-access-token"></a>

## 创建访问令牌

使用访问令牌页面命名令牌、查看任何可用的产品范围并选择适当的有效期窗口。

1. 转到 [访问令牌](https://chatgpt.com/admin/access-tokens)。
2. 选择**创建**。


  

> 插图：使用“创建”按钮访问令牌页面




3. 输入描述性名称，例如 `release-ci` 或 `nightly-docs-check`。


  

> 插图：使用名称和过期字段创建访问令牌模式




4. 如果对话框显示 **范围**，请选择 **Codex**。仅当同一工作流还需要触发工作区智能体时，才选择 **工作区智能体**。如果对话框没有范围选择器，它将创建仅 Codex 的令牌。
5. 选择有限的有效期窗口，例如 7、30、60 或 90 天。范围内的个人访问令牌必须过期。早期的仅 Codex 对话框可以提供 **没有过期时间**；除非您的组织批准并按定义的时间表轮换令牌，否则请避免使用该选项。
6. 选择**创建**。
7. 立即复制生成的访问令牌。关闭对话框后您将无法再次查看它。
8. 将令牌存储在您的秘密管理器或 CI 秘密存储中。

最短的自定义有效期窗口为一天。您不能使用已撤销或过期的令牌来启动新的经过身份验证的运行。

<a id="use-an-access-token-with-codex-cli"></a>

## 通过 Codex CLI 使用访问令牌

如果令牌创建对话框列出了所需的 Codex CLI 版本，请在使用令牌之前将 CLI 更新到该版本或更高版本。

对于临时自动化，请将令牌存储在 `CODEX_ACCESS_TOKEN` 中并正常运行 Codex CLI：

```bash
export CODEX_ACCESS_TOKEN="<access-token>"
codex exec --json "review this repository and summarize the top risks"
```

对于持久本地登录，将令牌通过管道传输到 `codex login --with-access-token`：

```bash
printf '%s' "$CODEX_ACCESS_TOKEN" | codex login --with-access-token
codex exec "summarize the last release diff"
```

`codex login --with-access-token` 将智能体身份凭证存储在 Codex CLI 身份验证存储中。如果您不想在计算机上保留凭据，请改用 `CODEX_ACCESS_TOKEN` 环境变量。

`codex app-server` 可以通过 `CODEX_ACCESS_TOKEN` 或使用 `codex login --with-access-token` 创建的登录名使用相同的凭证来验证其 OpenAI 请求。该凭证与客户端到应用程序服务器的传输身份验证是分开的。对于远程 WebSocket 连接，请配置单独的承载或能力令牌，如 [应用服务器](../app-server.zh-CN.md) 中所述；不要将 Codex 访问令牌重复用作传输令牌。参见 [身份验证和网络环境变量](../config-file/environment-variables.zh-CN.md#authentication-and-network)。

<a id="rotate-or-revoke-a-token"></a>

## 轮换或撤销令牌

轮换访问令牌的方式与轮换其他自动化机密的方式相同：

1. 创建替换令牌。
2. 更新运行程序、调度程序或秘密管理器中的秘密。
3. 使用新令牌运行冒烟测试。
4. 从 [访问令牌](https://chatgpt.com/admin/access-tokens) 撤销旧令牌。

在访问令牌页面中，工作区所有者和管理员可以撤销任何工作区令牌。具有访问令牌权限的成员只能撤销他们创建的令牌。

<a id="permission-model"></a>

## 权限模型

工作区访问令牌权限控制令牌创建。根据工作区布局，**Codex 和工作本地** 中的 **允许成员使用 Codex 并在本地工作** 或 **Codex 本地** 中的 **允许会员本地使用Codex** 控制本地 Codex 访问。如果 **本地工作** 有自己的部分，则 **在本地使用工作** 控制工作并且不授予 Codex 访问权限。成员需要本地 Codex 访问权限和访问令牌权限才能进行令牌验证的 Codex 工作流程。成员无需创建访问令牌的权限即可拥有本地 Codex 访问权限。

| 能力 | 工作区所有者和管理员 | 具有访问令牌权限的成员 | 不具有访问令牌权限的成员 |
| ------------------------------------------------------------- | ------------------------------------------------ | --------------------------------------------- | -------------------------------------- |
| 打开 [访问令牌](https://chatgpt.com/admin/access-tokens) | 有 | 有 | 无 |
| 创建访问令牌 | 是，用于自己的 ChatGPT 工作区身份 | 是，用于自己的 ChatGPT 工作区身份 | 否 |
| 列出访问令牌 | 工作区列表，包括每个令牌的创建者 | 仅他们创建的令牌 | 否 |
| 从访问令牌页面撤销访问令牌 | 工作区中的任何令牌 | 仅他们创建的令牌 | 无页面访问权限 |
| 授予或删除访问令牌权限 | 仅限工作区所有者 | 否 | 否 |
| 管理其他本地客户端或 Codex 云设置 | 是，基于工作区管理员权限 | 否，除非所有者授予访问权限 | 否 |

简而言之：工作区所有者和管理员管理工作区级别的访问权限。成员需要访问令牌权限来创建和管理自己的令牌，但该权限既不授予管理权限，也不授予对其他成员令牌的访问权限。

<a id="troubleshooting"></a>

## 故障排除

<a id="the-access-tokens-page-returns-404-or-forbidden"></a>

### 访问令牌页面返回 404 或禁止

请工作区所有者确认您的角色包括 **允许用户创建个人访问令牌** 或 **允许成员使用 Codex 访问令牌**，具体取决于可用的接口。对于经过令牌验证的 Codex 工作流程，还要确认 **允许成员使用 Codex 并在本地工作** 或 **允许会员本地使用Codex** 处于活动状态。

<a id="codex-login---with-access-token-fails"></a>

### `codex login --with-access-token` 失败

确认您复制的是生成的访问令牌，而不是浏览器会话令牌或平台 API 密钥。另请确认令牌处于活动状态、尚未过期，并且属于具有所需本地 Codex 权限的用户。

<a id="related-docs"></a>

## 相关文档

- [认证](../auth.zh-CN.md)
- [服务账户](service-accounts.zh-CN.md)
- [非交互模式](../non-interactive-mode.zh-CN.md)
- [管理员推出指南](admin-setup.zh-CN.md)
- [组和配置](groups-and-provisioning.zh-CN.md)
- [用户生命周期管理](user-lifecycle.zh-CN.md)
- [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)
- [治理](governance.zh-CN.md)