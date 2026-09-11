> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/groups-and-provisioning.md)。

<a id="groups-and-provisioning"></a>

# 用户组与预配

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

群组将人员组织在 ChatGPT 工作区中，并且可以承担自定义角色。组成员身份不会取代席位分配、自行授予工作区功能权限、覆盖本地运行时策略或提供对平台 API 或连接系统的访问权限。

完整的控制模型请参见[角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="compare-membership-sources"></a>

## 比较会员来源

将群组用于具有共享访问需求的人员，例如试点群组、工作区操作员或需要相同支持功能的成员。

<a id="create-a-group-for-a-shared-access-need"></a>

### 创建一个组来满足共享访问需求

工作区所有者和管理员可以创建和管理组。为小型或临时受众创建手动管理的组，或者当成员资格应遵循您的目录时从身份提供商同步已建立的组。

每个群组都有一个权威的会员来源：

| 团体类型 | 会员来源 | 适用时 |
| ------------------------- | ----------------------------------- | -------------------------------------------------------------------------------- |
| 手动管理 | ChatGPT 工作区管理 | 该组较小、临时或不通过目录同步进行管理 |
| 身份提供商管理 | 您通过 SCIM 的身份提供商 | 成员身份应遵循组织的目录和成员删除流程 |

手动组和身份提供商管理的组可以共存。对于同步组，身份提供者是成员资格源；稍后的配置更新可能会覆盖工作区端的更改。帮助中心拥有当前 SCIM 行为、支持的属性和设置步骤。

<a id="understand-the-access-boundary"></a>

## 了解访问边界

组成员资格本身并不授予工作区功能权限。

<a id="connect-a-group-to-the-right-permissions"></a>

### 将组连接到正确的权限

工作区所有者可以将自定义角色分配给组，或者直接分配给成员（如果可用）。检查每个适用的角色：任何角色中的显式 **关闭** 都会拒绝该权限，即使另一个角色授予该权限也是如此。会员的座位类型和产品资格仍然适用。

SCIM 规定工作区成员身份和组分配。它不会在 GitHub、Google Drive、Slack 或其他连接的系统中授予权限。它也不会取代本地运行时要求或平台 API 组织访问权限。

工作区 RBAC 和本地运行时要求是单独的控制系统。一个组可以与两者相关，但不要从工作区组顺序推断托管需求匹配或优先规则。使用 [受管配置](managed-configuration.zh-CN.md) 作为记录的交付和本地优先规则。

<a id="use-current-setup-procedures"></a>

## 使用当前设置程序

工作区管理详细信息可能会更改。使用这些来源了解当前的 UI 步骤、可用性和限制：

- [管理成员、席位类型、角色和访问权限](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise)
- [管理群组](https://help.openai.com/en/articles/9083985-group-permissions-in-gpts)
- [SCIM 集成常见问题解答](https://help.openai.com/en/articles/10011769-openai-platform-scim-integration-faq)
- [管理工作区设置](https://help.openai.com/en/articles/8411955)

<a id="verify-joiners-movers-and-leavers"></a>

### 验证加入者、移动者和离开者

- **木工：** 确认成员接受任何待处理的工作区邀请并收到预期席位、组成员身份、权限和支持的功能。
- **搬运工：** 更新权威成员资格来源并验证成员在所有适用角色中的有效权限。
- **离校者：** 通过身份提供者删除 SCIM 托管成员的访问权限，并确认该成员无法再访问工作区。如果您仅从工作区中删除该成员，稍后的同步可以恢复访问权限。

<a id="related-docs"></a>

## 相关文档

- [用户生命周期管理](user-lifecycle.zh-CN.md)
- [认证](../auth.zh-CN.md)
- [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)
- [受管配置](managed-configuration.zh-CN.md)
- [管理员推出指南](admin-setup.zh-CN.md)