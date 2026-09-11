> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/workspace-model-availability.md)。

<a id="workspace-model-availability"></a>

# 工作区模型可用性

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

可供某人使用的模型取决于产品界面及其登录方式。ChatGPT 工作区中的模型设置不会自动应用于 ChatGPT 桌面应用程序、Codex CLI、IDE 扩展、Codex 云或 OpenAI API 中的 Codex。

有关完整的管理模型，请参阅 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。

<a id="identify-the-model-boundary"></a>

## 识别模型边界

| 产品或认证边界 | 模型访问遵循 | 电流源 |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| ChatGPT 工作区 | 工作区规划、成员访问、工作区设置和支持的角色权限 | [ChatGPT Enterprise 和 Edu 模型和限制](https://help.openai.com/en/articles/11165333-chatgpt-enterprise-models-limits) |
| ChatGPT 桌面应用程序中的 Codex、Codex CLI 和 ChatGPT 登录的 IDE 扩展 | 特定客户端支持的模型以及登录的 ChatGPT 身份可用的访问权限 | [Codex模型](../models.zh-CN.md) 和当前工作区指南|
| Codex 云 | 托管 Codex 工作流程支持的模型以及登录的 ChatGPT 身份 | [Codex模型](../models.zh-CN.md) 和 [Codex云](../cloud.zh-CN.md) | 可用的访问权限
| ChatGPT 桌面应用程序、Codex CLI 和具有 API 密钥身份验证的 IDE 扩展中的 Codex | 与密钥 | [认证](../auth.zh-CN.md) 和 [OpenAI API平台](https://platform.openai.com/docs/overview) | 关联的 OpenAI API 组织和项目

检查用户实际使用的使用界面的当前源。不要复制模型目录或假设 ChatGPT 模型选择器设置对 ChatGPT 桌面应用程序、Codex CLI、IDE 扩展、Codex 云和 API 平台中的 Codex 具有相同的效果。

<a id="set-a-clear-starting-experience-for-employees"></a>

## 为员工设定清晰的起始体验

在邀请试点小组之前，请先查看您的工作区的 [模型设置](https://help.openai.com/en/articles/8411955)。工作区所有者和管理员可以为聊天、工作和 Codex 配置单独的启动默认值。在支持的情况下，为聊天、工作和本地 Codex 界面选择起始模型、推理级别、速度和新聊天行为。

将这些选择视为默认选项，而不是权限。可用模型仍然取决于成员的席位、角色、工作区或 API 身份、强制工作区要求以及他们正在使用的特定使用界面。启动默认值不会授予对不可用模型的访问权限或覆盖这些要求。 Codex云不支持更改默认模型。

快速模式的可用性取决于工作区、产品使用界面以及 [`requirements.toml`](../config-file/config-reference.zh-CN.md#requirementstoml) 中任何强制的 `features.fast_mode` 设置。此设置可以为受管理的本地 Codex 客户端打开或关闭快速模式；它不是起始默认值，并且不能覆盖工作区或产品可用性。

<a id="gpt-6-astra-in-enterprise"></a>

## 企业中的 GPT-6 Astra

在初始部署期间，您的组织必须具有 Daybreak 访问权限，管理员才能启用 Astra。 ChatGPT Enterprise 在发布后的前两周默认关闭 Astra。符合条件的工作区中的管理员可以为聊天、工作和 Codex 中的用户或组启用 Astra。现有产品资格仍然适用。检查您的 [工作区模型设置](https://help.openai.com/en/articles/8411955) 并确认您的试点组使用的每个客户端的可用性。

启用访问和选择起始模型是单独的决定。在将 Astra 设置为默认值之前，请检查适用的席位、角色和计费安排。有关津贴和计费指南，请参阅 [定价](../pricing.zh-CN.md)；有关暂停审核的任务，请参阅 [安全监控](../agent-approvals-security.zh-CN.md#safety-monitoring-and-paused-tasks)。

对于 API 密钥登录，Astra 访问遵循与密钥关联的 API 组织和项目。在 ChatGPT 工作区中启用 Astra 不会授予 API 访问权限。使用 API 密钥进行早期访问还需要客户端配置；请向您的 OpenAI 客户团队询问设置说明。选择模型或更改本地配置本身并不授予访问权限。

<a id="prepare-for-the-gpt-54-retirement"></a>

## 为 GPT-5.4 退役做好准备

2026 年 8 月 31 日，对于使用 ChatGPT 登录的用户，GPT-5.4 和 GPT-5.4 mini 从 Codex 中退出。在此之前更新受影响的工作区默认值、保存的模型设置、托管配置、自定义智能体和计划任务：

- 将 `gpt-5.4` 替换为 `gpt-5.6-terra` (GPT-5.6 Terra)。
- 将 `gpt-5.4-mini` 替换为 `gpt-5.6-luna` (GPT-5.6 Luna)。

使用您自己的 API 密钥进行身份验证的 OpenAI API 和 Codex 不受影响。有关迁移详细信息，请参阅 [Codex模型](../models.zh-CN.md#deprecated-codex-models) 和 [托管配置](managed-configuration.zh-CN.md)。

<a id="separate-access-from-runtime-permissions"></a>

## 将访问权限与运行时权限分开

模型访问确定模型是否可供经过身份验证的用户在受支持的使用界面上使用。本地权限配置文件和托管要求决定智能体在本地运行启动后可以执行哪些操作，例如可以更改哪些文件或可以到达哪些网络目标。

权限配置文件无法授予模型访问权限。模型访问也不能削弱适用于运行的沙箱、审批策略、网络控制或源系统权限。

<a id="troubleshoot-model-access"></a>

## 解决模型访问问题

如果用户无法选择期望的模型：

- 确认产品使用界面及签到方式。
- 确认 ChatGPT 工作区或平台 API 组织和项目。
- 查看该身份验证边界的当前访问控制。
- 检查选择的本地客户端或Codex云是否支持该模型。

<a id="current-sources"></a>

## 当前来源

- [ChatGPT Enterprise 和 Edu 模型和限制](https://help.openai.com/en/articles/11165333-chatgpt-enterprise-models-limits)
- [管理工作区设置](https://help.openai.com/en/articles/8411955)
- [基于角色的访问控制](https://help.openai.com/en/articles/11750701-rbac)
- [Codex模型](../models.zh-CN.md)
- [Codex 功能可用性（按计划）](../pricing.zh-CN.md#feature-availability)
- [认证](../auth.zh-CN.md)

<a id="related-docs"></a>

## 相关文档

- [管理员推出指南](admin-setup.zh-CN.md)
- [组和配置](groups-and-provisioning.zh-CN.md)
- [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)
- [受管配置](managed-configuration.zh-CN.md)