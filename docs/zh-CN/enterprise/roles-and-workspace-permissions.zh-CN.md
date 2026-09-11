> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/roles-and-workspace-permissions.md)。

<a id="roles-and-workspace-permissions"></a>

# 角色与工作区权限

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

不同的设置涵盖组织 ChatGPT 体验的不同部分。授予某人在一个区域的访问权限并不会自动授予他们在另一区域的访问权限。使用此页面查看六个控制边界如何协同工作，然后按照当前设置步骤的链接指南进行操作。

在工作区设置中，**Codex 和工作本地** 结合了本地 Codex 和 **允许成员使用 Codex 并在本地工作** 下的工作访问。其他工作区将 **Codex 本地** 和 **本地工作** 分成独立的部分。在该布局中，**允许会员本地使用Codex** 授予本地 Codex 访问权限，**在本地使用工作** 授予本地工作访问权限。启用其中一个并不授予另一个访问权限。这些标签标识工作区权限，而不是单独的产品或客户端。令牌权限和凭证生命周期限制显示在 **访问令牌** 部分或本地访问部分，具体取决于工作区。托管配置是一个单独的层，它限制这些客户端中涵盖的功能所支持的运行时行为。功能和有效要求可能因客户端和版本而异。

<a id="understand-the-control-boundaries"></a>

## 了解控制边界

| 边界 | 它控制什么 | 它不控制什么 | 电流源 |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatGPT 工作区 | 成员资格、席位、内置管理角色以及对支持的工作区功能的基于角色的访问 | 本地智能体权限、平台 API 组织访问权限或连接服务中的权限 | [ChatGPT 工作区访问](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise) 和 [RBAC](https://help.openai.com/en/articles/11750701-rbac) |
| 本地客户端 | ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能的运行时行为，包括批准、文件系统和网络访问、权限配置文件以及允许的集成 | ChatGPT 席位、功能或模型权利，或对外部数据的访问 | [受管配置](managed-configuration.zh-CN.md) 和 [权限](../permissions.zh-CN.md) |
| Codex 云 | 使用托管 Codex 工作流程和向用户提供的云环境的资格 | 本地运行时策略或源系统授予的仓库权限 | [云环境](../environments/cloud-environment.zh-CN.md) |
| 平台 API | 组织和项目成员资格、API 密钥、模型访问、使用情况以及 API 验证工作的计费 | ChatGPT 工作区成员资格、本地客户端访问或 Codex 云访问 | [OpenAI API平台](https://platform.openai.com/docs/overview) |
| 插件 | 插件可用性和安装、捆绑技能、连接器访问和支持的连接器操作 | 连接服务中的授权或更广泛的本地和云运行时权限 | [插件控件](apps-and-connectors.zh-CN.md) |
| 连接的系统 | 经过身份验证的帐户可以在源系统中访问哪些仓库、文件、消息和操作 | ChatGPT 工作区、插件、Codex 云或平台 API 权利 | 连接的服务的管理和访问控制 |

请求必须通过适用于它的每个边界。例如，工作区访问可以使插件可用，但连接的服务仍然决定登录帐户可以读取哪些数据。本地权限配置文件可以限制受支持的本地客户端中的运行，但不能授予工作区功能或模型。

<a id="assign-workspace-access"></a>

## 分配工作区访问权限

ChatGPT 工作区管理将产品访问与管理权限分开。

<a id="understand-the-difference-between-a-seat-an-admin-role-and-a-custom-role"></a>

### 了解席位、管理角色和自定义角色之间的区别

座位决定了会员可以访问哪些产品使用界面。根据工作区规划，可用座位类型可包括 ChatGPT 和 Codex 座位。

内置工作区角色决定管理权限。 **业主** 角色管理工作区范围的设置，**管理员** 角色管理支持的操作和组，**会员** 角色没有管理权限，**分析查看器** 角色可以访问工作区分析。

自定义角色定义成员可以使用哪些受支持的功能。它们不会替换席位或计划资格、在连接的系统中授予权限或更改本地运行时要求。



  <iframe
    src="https://player.vimeo.com/video/1215495812"
    title="基于角色的访问控制演练"
    loading="lazy"
    allow="autoplay; fullscreen; picture-in-picture"
    allowFullScreen
    referrerPolicy="strict-origin-when-cross-origin"
    class="h-full w-full border-0"
  ></iframe>



<a id="set-the-workspace-default-then-create-targeted-custom-roles"></a>

### 设置工作区默认值，然后创建有针对性的自定义角色

只有工作区所有者才能配置基于角色的访问控制 (RBAC) 并创建自定义角色。工作区设置为合格权限建立了基线。工作区所有者可以通过组或直接向支持的个人成员分配自定义角色。群组可以手动管理或 SCIM 同步，并且一名成员可以接收多个自定义角色。

对于符合条件的权限，**默认** 继承工作区设置，**开** 授予访问权限，**关闭** 显式拒绝访问。任何适用角色中的显式 **关闭** 都会阻止访问，即使另一个角色授予访问权限也是如此。可用的权限状态可能因功能而异。

<a id="review-work-local-and-work-cloud-permissions"></a>

### 查看工作本地和工作云权限

当您的工作区提供 **本地工作** 和 **工作云** 时，请检查工作区默认值和每个适用的自定义角色。工作仅适用于符合条件的工作区，并且可用的控件可能因计划、工作区配置和部署而异。角色无法扩展成员席位所允许的访问权限。

**工作云** 管理云中支持的 ChatGPT Work 任务。当控件独立时，没有 **工作云** 的 **本地工作** 允许在 ChatGPT 桌面应用程序中进行本地工作，但不允许成员启动云任务。本地Codex访问使用**Codex 本地**中的**允许会员本地使用Codex**。更改 **在本地使用工作** 不会更改本地 Codex 访问或替换本地运行时要求。

有些工作区反而显示组合的 **Codex 和工作本地** 部分。在该布局中，**允许成员使用 Codex 并在本地工作** 控制这两种产品。

有关当前资格和设置，请参阅 [ChatGPT Work 和 Codex](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)。

由于可用席位、角色和权限会随着产品和计划更新而变化，因此请使用帮助中心了解当前权限列表和设置过程：

- [管理成员、席位类型、角色和访问权限](https://help.openai.com/en/articles/8266401-managing-members-seat-types-roles-and-access-in-chatgpt-enterprise)
- [配置基于角色的访问控制](https://help.openai.com/en/articles/11750701-rbac)
- [管理群组](https://help.openai.com/en/articles/9083985-group-permissions-in-gpts)

<a id="control-computer-history-access"></a>

### 控制Computer History访问

对于商业和企业工作区，[电脑历史记录](../customization/computer-history.zh-CN.md) 默认处于关闭状态。在工作区所有者明确授予访问权限之前，成员无法将其打开。企业工作区所有者可以按角色授予访问权限：

1. 打开[**工作区设置 > 权限和角色**](https://chatgpt.com/admin/settings)。
2. 找到 **Computer History** 并选择应具有访问权限的工作区角色。
3. 为该角色打开 **启用Computer History**。

该权限仅允许指定成员开启Computer History；它不会为他们打开该功能。每个成员必须从 macOS 上的 ChatGPT 桌面应用程序中选择加入，并且可以选择哪些应用程序和网站提供贡献。没有所需工作区权限的成员无法通过本地设置启用该功能。

<a id="apply-local-runtime-policy"></a>

## 应用本地运行时策略

本地运行时策略限制 ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中涵盖的功能。云托管要求还取决于支持的 ChatGPT 登录和计划资格。权限配置文件和托管要求可以限制命令、文件系统访问、网络访问、批准和其他本地运行时行为。它们不会更改用户的席位、工作区角色、模型权利或外部系统中的权限。

当本地策略允许时，用户可以选择内置或自定义权限配置文件。管理员可以通过支持的托管配置渠道分发默认值和要求。有关配置文件行为，请参阅 [权限](../permissions.zh-CN.md)；有关要求、交付和优先级，请参阅 [受管配置](managed-configuration.zh-CN.md)。

<a id="related-docs"></a>

## 相关文档

- [管理员推出指南](admin-setup.zh-CN.md)
- [组和配置](groups-and-provisioning.zh-CN.md)
- [用户生命周期管理](user-lifecycle.zh-CN.md)
- [工作区模型可用性](workspace-model-availability.zh-CN.md)
- [访问令牌](access-tokens.zh-CN.md)
- [受管配置](managed-configuration.zh-CN.md)
- [认证](../auth.zh-CN.md)