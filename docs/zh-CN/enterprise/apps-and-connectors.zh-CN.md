> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/apps-and-connectors.md)。

<a id="plugin-controls"></a>

# 插件控制

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

插件打包可重用的工作流程，并可以包括连接到其他工具的技能和 MCP 服务器。 ChatGPT 和 Codex 在受支持的界面上使用相同的公共插件目录，而管理员则决定其工作区中可用的插件。了解有关 [插件](../plugins.zh-CN.md)、[技能](../skills-and-plugins.zh-CN.md) 和 [连接服务](https://help.openai.com/en/articles/11487775) 的更多信息。

在本指南中，**应用程序** 和 **MCP服务器** 指的是相同的连接集成，并且是可互换的术语。我们在散文中使用 **MCP服务器**，但在 UI 标签（例如 **工作区应用程序** 和 **应用程序权限**）以及 CSV 列名称中保留 **应用程序**。

仅当插件和 MCP 服务器可供其角色使用并且可以访问连接的服务时，成员才能使用 MCP 服务器的功能。

插件可在 Web、桌面和移动设备上的 ChatGPT 上的聊天和工作中、在 ChatGPT 桌面应用程序中的 Codex 中以及通过 Codex CLI 插件浏览器中使用。它们在 IDE 扩展中不可用。

要了解这些控件如何适应工作区角色和权限，请参阅 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="understand-the-capability-chain"></a>

## 了解能力链

插件可以跨越这些控制层：

| 层 | 它决定什么 | 在哪里管理它 |
| ----------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| 可用性 | 插件包是否可供用户使用 | [工作区设置](https://chatgpt.com/admin/settings) 用于受支持的 Web 和桌面界面； CLI | 的 CLI 插件浏览器
| 包含的技能 | 安装的插件提供哪些可重用指令 | 插件包和 [技能控制](skills.zh-CN.md) |
| MCP 服务器访问 | 用户是否可以使用 MCP 服务器的功能 | [工作区应用程序](https://chatgpt.com/admin/ca) 和 [权限和角色](https://chatgpt.com/admin/settings) |
| 操作和权限 | 用户可以运行哪些操作以及 ChatGPT 在使用其工具之前何时询问 | [工作区应用程序](https://chatgpt.com/admin/ca) 中的连接的 **动作控制** 和 **应用程序权限** |
| 服务授权 | 经过身份验证的身份可以访问哪些外部数据和操作 | 连接的服务及其身份提供者 |
| 运行时权限 | 智能体收到数据或工具后可以做什么 | 活动使用界面的运行时、沙箱和审批控制 |

使用这些层作为两步部署：首先提供正确的插件，然后配置每个工作流所需的功能和权限。

<a id="step-1-enable-plugin-availability"></a>

## 第 1 步：启用插件可用性

对于受支持的 Web 和桌面使用界面，工作区插件控件确定哪些角色可以使用或安装插件。 Codex CLI 使用自己的插件浏览器进行安装。包装和分发请参见 [构建插件](https://developers.openai.com/plugins/build/plugins)。

要从 GitHub 导入工作区插件并使其保持最新，请参阅 [插件管理](plugin-management.zh-CN.md)。

<a id="export-the-public-catalog-for-review"></a>

### 导出公共目录以供审核

符合条件的 ChatGPT Enterprise 工作区所有者和管理员可以下载其工作区可用的公共插件的 CSV。在更改插件可用性之前，使用导出来检查插件、MCP 服务器和技能元数据。

1. 打开[管理 > 插件](https://chatgpt.com/admin/plugins)。
2. 选择**公共**。
3. 选择页面标题中的下载图标 (**导出 CSV**)。

下载使用文件名 `public-plugins-security-review.csv` 并包括：

- 插件元数据：`Plugin Name`、`Plugin Description`、`Date Added (UTC)`、`OpenAI Verified`、`Developer Name` 和 `Version`。
- MCP 服务器元数据：`App Name(s)` 和 `App Description(s)`。
- 聊天技能元数据：`Skill Name(s)` 和 `Skill Description(s)`。

当插件包含多个 MCP 服务器或技能时，用分号分隔相应的值。导出使用可能长达 48 小时的公共目录快照，仅包含当前工作区可见的公共插件，并且不包含为该工作区创建的插件。它在 FedRAMP 工作区中不可用。

<a id="step-2-manage-capabilities"></a>

## 第 2 步：管理能力

<WarningTip>
使 MCP 服务器或插件在 ChatGPT 中可用并不授予对连接服务中的文件、记录或操作的访问权限。在排除故障或扩展访问权限之前，请检查成员的工作区角色和批准的操作设置。然后确认经过身份验证的帐户或共享连接在连接的服务中具有预期的权限。
</WarningTip>

ChatGPT 和 Codex 中的插件可以包括搜索、检索、同步或作用于外部系统的 MCP 服务器连接。插件可用性以及授予每个连接的访问​​权限和操作是单独的控制。

从 [工作区应用程序](https://chatgpt.com/admin/ca) 和 [权限和角色](https://chatgpt.com/admin/settings) 管理 MCP 服务器功能。可用的控件允许管理员：

- 启用 MCP 服务器连接并按工作区角色分配访问权限。
- 对于支持 **动作控制** 的连接，允许只读操作或批准的自定义集，包括工作区如何处理新添加的操作。
- 设置 **应用程序权限** 以确定 ChatGPT 在使用连接之前何时询问。
- 将访问保持在每个连接的服务和经过身份验证的用户授予的范围和权限内。

有关当前可用性和程序，请参阅 [应用程序中的管理控制、安全性和合规性](https://help.openai.com/en/articles/11509118)。

<a id="choose-a-starting-set-of-apps"></a>

<a id="choose-a-focused-initial-set"></a>

## 选择一个有重点的初始集

从支持明确业务需求的插件开始。决定是否让每个插件可供所有人使用，将其限制为某个角色或试点组，或者需要进一步审查。

对于每个连接的服务，记录业务所有者、允许的数据、批准的读取或写入操作、身份验证方法以及支持或删除联系人。

在启用写入操作或发布新的连接功能之前，请验证其角色范围并使用仅在连接服务中具有预期权限的帐户进行测试。

要进行广泛的部署，请从团队日常使用的类别开始，例如电子邮件、日历以及文件或文档系统。使用 [插件目录](https://chatgpt.com/apps) 确认受支持的 ChatGPT 和 Codex 使用界面的当前可用性和功能。

无论初始设置是什么，都从读取操作开始。在启用写入操作之前，请确定插件所有者、检查 MCP 服务器范围和服务权限、确认数据访问并记录外部影响和恢复路径。

<a id="understand-data-flow-and-security"></a>

## 了解数据流和安全性

当 ChatGPT 使用插件中包含的 MCP 服务器时，它会向连接的服务发送请求，并返回该服务中经过身份验证的用户权限允许的数据或操作结果。

ChatGPT 通过两种方式处理来自连接服务的数据：

- **不同步：** ChatGPT 瞬时处理来自聊天和深度研究的数据，并且不对其建立索引。
- **已同步：** ChatGPT 预先索引选择的连接内容。您可以在其插件页面上查看连接是否支持同步。

该模式改变了 ChatGPT 索引连接内容的方式；它不会取代正常的聊天保留控件。使用这些连接的 ChatGPT 对话仍然可以通过合规性 API 使用。

OpenAI 的连接服务指南记录了传输中和静止时的加密、每用户授权、角色和操作控制、使用这些连接的对话的受限网络访问，并且没有针对商业、企业和教育客户通过这些连接访问的信息进行模型培训。当请求到达连接的服务时，该服务的范围、保留、数据驻留和其他策略也适用。

有关当前数据处理的详细信息，请参阅 [互联服务的安全性和合规性](https://help.openai.com/en/articles/11509118) 和 [与同步的连接](https://help.openai.com/en/articles/10847137)。对于 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中本地配置的 MCP 服务器，请参阅 [Codex MCP 配置](../extend/mcp.zh-CN.md)。

<a id="use-current-procedures-and-references"></a>

## 使用当前的程序和参考资料

- [应用程序中的管理控制、安全性和合规性](https://help.openai.com/en/articles/11509118)
- [ChatGPT 中的应用](https://help.openai.com/en/articles/11487775)
- [具有同步功能的应用程序](https://help.openai.com/en/articles/10847137)
- [管理工作区设置](https://help.openai.com/en/articles/8411955)
- [插件](../plugins.zh-CN.md)
- [技能和插件](../skills-and-plugins.zh-CN.md)
- [构建插件](https://developers.openai.com/plugins/build/plugins)
- [管理员推出指南](admin-setup.zh-CN.md)