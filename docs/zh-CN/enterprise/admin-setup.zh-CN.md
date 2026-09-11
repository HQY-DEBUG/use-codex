> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/admin-setup.md)。

<a id="admin-rollout-guide"></a>

# 企业部署指南

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用本指南来规划跨这些管理边界的 ChatGPT Enterprise 部署：

- 工作区访问。
- ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能的本地运行时策略。
- Codex云。
- 平台API访问。
- 插件和连接器访问。
- 连接系统中的权限。

完成这些步骤以进行新的部署，或使用链接页面更改一个边界。

在工作区设置中，**Codex 和工作本地** 结合了本地 Codex 和 **允许成员使用 Codex 并在本地工作** 下的工作访问。有些工作区反而提供独立的 **Codex 本地** 和 **本地工作** 部分。在该布局中，**允许会员本地使用Codex** 控制 Codex，**在本地使用工作** 控制工作。启用其中一个不会启用另一个。这些标签标识工作区权限，而不是单独的产品或客户端。令牌权限和凭证生命周期限制显示在 **访问令牌** 部分或本地访问部分，具体取决于工作区。托管配置是一个单独的策略层，可以限制这些客户端中涵盖的功能所支持的运行时行为。当行为或可用性不同时，本指南会对各个使用界面进行命名。

从 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md) 中的规范映射开始。使用当前 ChatGPT 工作区过程的帮助中心指南以及本地和托管运行时行为的链接开发人员文档。

<a id="enterprise-grade-security-and-privacy"></a>

有关企业安全、隐私和运行时保护，请参阅 [智能体审批和安全](../agent-approvals-security.zh-CN.md) 和 [Codex 安全白皮书](https://trust.openai.com/?itemUid=382f924d-54f3-43a8-a9df-c39e6c959958&source=click)。

<a id="pre-requisites-determine-owners-and-rollout-strategy"></a>

<a id="step-1-assign-owners-and-choose-a-rollout"></a>

## 第 1 步：分配所有者并选择部署

为部署的每个部分分配一个所有者：

- **工作区访问：** 成员资格、席位、角色和支持的工作区功能。
- **本地运行时策略：** 支持的本地客户端的批准、权限配置文件、文件系统和网络访问以及其他要求。
- **Codex云：** 托管环境、仓库连接和云运行时策略。
- **连接系统：** 提供商端应用程序安装、帐户和权限。
- **报告和合规性：** 分析访问、审核导出和下游数据处理。

确定每个受众是否需要 ChatGPT 桌面应用程序、Codex CLI、IDE 扩展、Codex 云或组合中涵盖的本地功能。当工作流使用 API 密钥身份验证时，将平台 API 访问视为单独的组织和项目边界。

<a id="step-2-configure-workspace-access-and-identity"></a>

## 第 2 步：配置工作区访问和身份

使用 ChatGPT 工作区成员资格、席位、组和支持的 RBAC 权限向目标受众授予支持的工作区功能。根据当前工作区指南验证本地客户端和 Codex 云访问，而不是假设相同的角色控制每个使用界面。将内置管理角色仅限于管理工作区的人员。

工作区控件和标签随着时间的推移而变化。将这些来源用于当前程序：

- [管理成员、席位类型、角色和访问权限](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise)
- [配置基于角色的访问控制](https://help.openai.com/en/articles/11750701-rbac)
- [管理工作区设置](https://help.openai.com/en/articles/8411955)
- [组和配置](groups-and-provisioning.zh-CN.md)
- [用户生命周期管理](user-lifecycle.zh-CN.md)
- [认证](../auth.zh-CN.md)

在扩大部署之前，与代表成员一起测试登录和功能访问。工作区访问权限不会授予连接服务中的仓库、文件或操作访问权限。

<a id="step-3-configure-local-runtime-requirements"></a>

## 步骤 3：配置本地运行时要求

当用户在 ChatGPT 桌面应用程序、Codex CLI 或 IDE 扩展中启动受支持的本地运行时，本地要求会限制运行时行为。通过支持的云、设备或系统通道交付 `requirements.toml`。将此策略与 ChatGPT 工作区角色和组分开。

使用受支持的本地客户端的权限配置文件，而不是围绕旧的沙箱模式限制构建新的部署。例如：

```toml
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
```

要在支持的浏览器和桌面功能界面上禁用计算机使用，请限制参与体验的每个公共功能键：

```toml
[features]
browser_use = false
browser_use_full_cdp_access = false
browser_use_external = false
in_app_browser = false
computer_use = false
```

有关权威密钥列表、传递行为、优先级和更多示例，请参阅 [受管配置](managed-configuration.zh-CN.md) 和 [`requirements.toml`参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

<a id="team-config"></a>
<a id="step-4-standardize-local-configuration-with-team-config"></a>

<a id="step-4-standardize-repository-configuration"></a>

## 第 4 步：标准化仓库配置

使用仓库范围的配置来共享项目默认值、规则和技能，而无需为每个用户重复设置。根据功能的记录位置检查 `.codex` 或 `.agents` 中的配置：

| 类型 | 源 | 使用它到 |
| ------------- | ------------------------------------------------ | ---------------------------------------------------------- |
| 配置 | [配置基础知识](../config-file/config-basic.zh-CN.md) | 为支持的本地客户端设置仓库默认值 |
| 规则 | [规则](../agent-configuration/rules.zh-CN.md) | 需要沙箱外批准的控制命令 |
| 技能 | [培养技能](../build-skills.zh-CN.md) | 使仓库工作流程可供支持的客户端使用 |

仓库配置可以提供默认值和可重用的工作流程。它无法授予工作区、模型、平台 API 或连接系统访问权限。

<a id="step-5-configure-codex-cloud"></a>

## 步骤5：配置Codex云

Codex 云使用托管环境和连接的源仓库。规划每个边界：

1. 通过支持的工作区控件向目标受众 Codex 授予云访问权限。
2. 安装和配置支持的源系统集成。
3. 将源系统中的仓库访问限制为每个受众需要的仓库。
4. 为这些仓库配置云环境、机密和 Internet 访问。
5. 配置可选的托管工作流程，例如代码审查。
6. 使用具有预期工作区和仓库权限的代表用户进行测试。

Codex 云尊重所连接的源系统公开的仓库权限和保护。工作区访问不会绕过这些控制。有关 Codex 云设置和运行时指南，请参阅 [云环境](../environments/cloud-environment.zh-CN.md)、[GitHub 集成](../third-party/github.zh-CN.md) 和 [智能体审批和安全](../agent-approvals-security.zh-CN.md)。

<a id="step-6-configure-plugins-and-connected-capabilities"></a>

## 第 6 步：配置插件和连接功能

将插件安装、捆绑技能、连接器支持的功能、连接器操作和源系统授权作为单独的决策进行审核。禁用连接器支持的功能并不一定会卸载该插件或其捆绑的技能。

在推出插件或技能之前：

1. 确认其来源、负责人、目标受众和审核日期。
2. 查看捆绑技能、连接器、MCP 服务器、挂钩以及每个功能所需的数据和操作。
3. 使用非敏感数据和所需的最少访问权限对其进行测试。
4. 记录谁拥有重新审查和退休。

插件可在 Web、桌面和移动设备上的 ChatGPT 上的聊天和工作中、在 ChatGPT 桌面应用程序中的 Codex 中以及通过 Codex CLI 插件浏览器中使用。它们在 IDE 扩展中不可用。 ChatGPT和Codex共用一个通用公共插件目录；工作区控件确定成员可以访问哪些插件。

完整模型请参见 [插件控件](apps-and-connectors.zh-CN.md) 和 [技能控制](skills.zh-CN.md)。

<a id="step-7-set-up-governance-and-observability"></a>

## 第 7 步：设置治理和可观察性

选择与问题匹配的报告界面：

<a id="analytics-api-setup-steps"></a>
<a id="compliance-api-setup-steps"></a>

- 使用 [工作区分析](workspace-analytics.zh-CN.md) 进行交互式 ChatGPT 工作区分析和 Codex 分析。
- 使用 [分析API](analytics-api.zh-CN.md) 通过 Codex Analytics API 进行编程聚合报告。
- 使用 [合规API](compliance-api.zh-CN.md) 进行审计和调查记录。
- 当计划相关的 Codex 活动消耗符合条件的 ChatGPT 工作区额度时，请使用 [ChatGPT 使用限制和支出控制](usage-limits.zh-CN.md)。

使用经过身份验证的 API 参考来了解当前的访问要求、架构、字段、保留和请求行为。不要从本指南中复制的合同构建集成。

保护积分边界：

- 将 API 密钥和其他集成凭证存储在组织的秘密管理系统中。
- 限制对下游系统的访问并保留数据给批准的受众。
- 根据导出的合规性 API 记录的敏感性和组织的保留策略来保护导出的合规性 API 记录，并根据当前合同测试收集和删除工作流程。

<a id="step-8-verify-and-maintain-the-rollout"></a>

## 步骤 8：验证并维护部署

使用代表性身份验证每个适用的边界：

- ChatGPT 工作区成员资格、席位和支持的角色权限。
- 涵盖 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中的本地功能，包括登录和有效运行时要求。
- Codex 云访问、环境配置和仓库权限。
- API 密钥工作流程的平台 API 组织和项目访问。
- 插件安装、捆绑技能、连接器访问和支持的操作。
- 连接系统授权和数据访问。
- 负责管理员的分析和合规访问权限。

记录每个控件的所有者和当前程序源。该记录允许管理员在 UI 或策略更改时更新程序，而无需更改管理模型。

首次推出后，检查访问权限、连接功能、信用使用、支持反馈以及团队实际使用的工作流程。当这些信号发生变化时，调整推出范围和管理员指导。