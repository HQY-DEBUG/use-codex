> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/work-admin-faq.md)。

<a id="chatgpt-work-admin-faq"></a>

# Work 管理员常见问题

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT Work 将 Codex 背后的技术引入 ChatGPT，以执行更长、多步骤的任务。它可以从聊天、文件、工作区资源和连接的系统中收集上下文；使用经批准的工具；并创建可供审阅的输出。访问、上下文、操作、网络行为和信用使用因计划、工作区设置、源权限和界面而异。

<a id="overview"></a>

## 概述

ChatGPT Work 允许用户将更长的、多步骤的任务委托给 ChatGPT。它可以从连接的来源收集信息，跨步骤进行推理，创建文档、演示文稿或分析，并返回结果以供审核。

ChatGPT Work 可在支持的 Web、移动和桌面界面上使用，适用于符合条件的计划和工作区。在支持的情况下，工作区所有者或授权管理员可以通过不同的权限管理 Work Cloud、Work Local 和 Codex Local。对于符合条件的 Enterprise 和 Edu 工作区，默认工作区角色包括“工作”，除非授权管理员将其关闭。浏览器和网络控制进一步限制了 Work Cloud，可用性取决于角色、计划、工作区和区域。参见 [ChatGPT Work 和 Codex](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)。

此常见问题解答解释了管理员如何管理 ChatGPT Work：访问和数据控制、合规性和可见性、使用和支出、事件响应和推出实践。有关托管执行模型和安全边界，请参阅 [ChatGPT Work概述](chatgpt-work-overview.zh-CN.md)。

<a id="core-administrative-controls"></a>

## 核心管理控制

管理员通过以下控制层管理 ChatGPT Work：

- **访问企业工作区：** 身份和访问控制管理对工作区的身份验证和访问。根据计划和配置，管理员控制的身份功能可以包括 SSO、域验证、SCIM 配置、用户生命周期管理和身份组同步。 ChatGPT Business 中不包含 SCIM 和同步身份组。用户可以启用账户级OpenAI MFA。 ChatGPT 不提供工作区范围的 MFA 实施；需要它的组织应通过其身份提供商强制执行 SSO 和 MFA。管理 [全局管理控制台](https://help.openai.com/en/articles/12289294-admin-portal) 中的 SSO 和相关身份设置。参见 [多重身份验证](https://help.openai.com/en/articles/7967234-enabling-or-disabling-multi-factor-authentication-mfa)。
- **访问工作区中的 ChatGPT Work：** 在可用的情况下，Work Cloud 可以跨受支持的 Web、移动和桌面界面管理托管工作。 Work Local 管理本地桌面 Work，而 Codex Local 控件支持桌面、CLI 和 IDE 客户端中的本地 Codex 访问。云浏览器和网络设置进一步限制了 Work Cloud。基于角色的自定义访问控制 (RBAC) 和可用权限取决于计划和工作区。
- **团体会员：** 在支持 SCIM 的计划中，通过身份提供商同步组，以便在员工加入组织、更改角色或离开时访问更新。参见 [组和配置](groups-and-provisioning.zh-CN.md)。
- **工作区和成员角色：** 内置企业角色包括所有者、管理员、成员和分析查看者。在支持的计划上，自定义角色和成员 RBAC 控制对 ChatGPT Work、插件和其他功能的访问。如果座位类型适用，会员还需要包含 ChatGPT 的座位；仅限 Codex 的席位不授予对工作的访问权限。参见 [角色和工作区权限](roles-and-workspace-permissions.zh-CN.md)。
- **插件和应用程序：** 插件策略管理插件的可用性和安装。应用程序访问、操作控制和审批行为是单独配置的。工作区智能体有自己的控件（如果可用）。请参阅 [插件控件](apps-and-connectors.zh-CN.md)、[插件](../plugins.zh-CN.md) 和 [应用安全白皮书](https://cdn.openai.com/business-guides-and-resources/app-security-whitepaper.pdf)。
- **源系统权限：** 用户只能访问本机应用程序中帐户或共享连接允许的内容和操作。参见 [应用程序中的管理控制、安全性和合规性](https://help.openai.com/en/articles/11509118-admin-controls-security-and-compliance-in-apps-enterprise-edu-and-business)。
- **批准和行动限制：** 对于支持操作控制的应用程序，管理员可以允许所有操作、只读操作或自定义操作集，并决定如何处理新添加的操作。应用程序权限单独确定 ChatGPT 在使用应用程序之前何时询问。
- **额度：** ChatGPT Work 和 Codex 共享定价、额度和使用限制。符合条件的企业和教育管理员可以通过工作区默认值、组默认值和个人覆盖设置每月每用户限制。当工作区允许时，用户可以请求增加。企业遵循独立的信贷和支出控制模式。参见 [ChatGPT 使用限制和支出控制](usage-limits.zh-CN.md)。
- **分析和报告：** 全局管理控制台和工作区分析支持采用和信用使用分析。使用合规性 API 和 Codex 报告界面来记录其记录的事件和产品范围；在承诺覆盖特定提示、文件、批准、操作、错误或工具调用之前检查当前模式。参见 [治理](governance.zh-CN.md)。

<a id="access-data-systems-and-user-actions"></a>

## 访问、数据、系统和用户操作

<a id="how-are-access-to-data-systems-and-user-actions-protected"></a>

### 如何保护对数据、系统和用户操作的访问？

ChatGPT Work 由 ChatGPT 工作区中已建立的身份、访问和权限控制进行管理。管理员使用身份管理、工作区角色以及符合资格的计划的 [RBAC](https://help.openai.com/en/articles/11750701-rbac) 来确定谁可以使用 ChatGPT Work。

在支持的情况下，可以通过 [SCIM](https://help.openai.com/en/articles/10011769-openai-platform-scim-integration-faq) 和组同步与您的身份提供商同步访问。这使您可以在员工加入组织、更改角色或离开时集中管理访问和权限。

底层源系统强制执行用于操作的帐户或批准的共享连接的权限。个人连接使用该人的源系统访问权限。智能体拥有或共享的连接可以通过连接的帐户向授权智能体用户提供访问权限，包括他们自己的帐户无法访问的数据或操作。将连接的范围、可用操作和智能体受众限制为预期的业务需求。参见 [工作区智能体连接和权限](https://help.openai.com/en/articles/20001143-chatgpt-workspace-agents-for-enterprise-and-business)。

<a id="how-does-work-access-data-and-context"></a>
<a id="how-does-work-mode-access-data-and-context"></a>

<a id="how-does-chatgpt-work-access-data-and-context"></a>

### ChatGPT Work 如何访问数据和上下文？

ChatGPT Work 可以通过批准的应用程序和插件（如果适用）使用当前聊天、上传的文件、工作区资源和连接的系统。根据启用的功能和权限，这可以包括文档、仓库、票证、频道、电子邮件和日历。可以通过当前聊天、支持的项目、授权的库访问或启用的自动库引用来获取早期文件。保存的记忆遵循自己的工作区和用户控件。

每个上下文源都有自己的控制：用户提供聊天上下文，管理员管理工作区资源，连接的系统强制执行身份验证和权限。 ChatGPT Work 只能访问用户授权的信息或批准的共享连接。

ChatGPT Work 继承了适用的 ChatGPT 工作区保护。驻留、保留、日志记录和功能可用性因计划、区域、使用界面和连接的系统而异，因此请确认您的配置的覆盖范围。

<a id="what-high-impact-actions-are-restricted-or-require-review"></a>

### 哪些高影响力的行动受到限制或需要审查？

行动风险各不相同。阅读或起草的影响通常低于更改数据、共享信息或在外部系统中操作的影响。结合角色、缩小权限和凭据以及支持的批准，将影响较大的操作限制在受信任、经过审查的用途。

常见的操作类别包括：

- **阅读：** 在不更改基础数据的情况下访问、搜索或汇总来自批准来源的信息。
- **草案：** 准备文档、电子邮件、报告、代码或其他内容，供人员在使用前查看。
- **写：** 在连接的系统中创建、更新或删除记录，例如文档、票证、仓库或项目管理工具。
- **分享：** 发送、发布或以其他方式向更多人、系统或外部目的地提供信息。
- **时间表：** 在未来某个时间或按重复计划启动任务，无需用户启动每次运行。
- **执行：** 运行代码、shell 命令、浏览器自动化或其他直接与外部环境交互的工具驱动任务。

对于影响力更大的行动，请使用人工审查、受限凭证、缩小范围和支持批准。插件操作仍然遵循每个集成的权限和安全控制。

<a id="compliance"></a>

## 合规性

<a id="how-does-work-support-enterprise-privacy-and-data-commitments"></a>
<a id="how-does-work-mode-support-enterprise-privacy-and-data-commitments"></a>

<a id="how-does-chatgpt-work-support-enterprise-privacy-and-data-commitments"></a>

### ChatGPT Work如何支持企业隐私和数据承诺？

ChatGPT Work 使用适用于客户 ChatGPT 工作区的隐私、安全和数据承诺，具体取决于规划、配置、使用界面、功能和区域。对于 ChatGPT Enterprise，这包括 [默认不进行业务数据训练](https://help.openai.com/en/articles/8983130-what-if-i-want-to-keep-my-history-on-but-disable-model-training)、传输中和静态加密、工作区级访问控制以及支持的审核日志记录。

数据驻留、推理驻留、HIPAA 或业务伙伴协议的承保范围并不普遍。确认当前的 [数据和推理驻留指导](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt) 以及客户对所使用的功能和区域的协议。

互联服务有自己的保留、日志记录、访问、驻留和合规性要求。当 ChatGPT Work 使用插件、仓库或第三方系统时，评估 ChatGPT 工作区控件和连接系统的控件。

对于 Codex 活动，企业控制可以扩展到开发环境、仓库、配置工具和相关活动。查看 [管理员推出指南](admin-setup.zh-CN.md) 和 [治理](governance.zh-CN.md) 以及工作区控件。

<a id="what-data-is-stored-retained-or-deleted"></a>

### 存储、保留或删除哪些数据？

ChatGPT Work 的数据保留和删除由 ChatGPT 工作区计划、管理设置和使用的功能控制。 ChatGPT Work 访问的信息的保留情况可能有所不同。对话和合格的库文件遵循其适用的工作区设置。项目文件、瞬时上传、保存的内存、合规性事件、同步的应用程序数据和第三方记录可以具有单独的保留和删除规则。参见 [聊天和文件保留策略](https://help.openai.com/en/articles/8983778-chat-and-file-retention-policies-in-chatgpt)。

ChatGPT Work 可以创建聊天内容、上传或生成的文件、工件和执行元数据。 Codex 聊天还​​可以创建仓库或环境元数据、命令输出、差异和日志。检查当前产品和 [合规API](compliance-api.zh-CN.md) 文档，了解确切的数据类、保留期和删除路径。

查看 ChatGPT 工作区和连接的企业系统的保留要求，以便您组织的数据治理、合规性和记录保留策略适用于每个系统。

<a id="observability"></a>

## 可观测性

<a id="what-usage-data-is-available-to-admins-or-owners"></a>

### 管理员或所有者可以使用哪些使用数据？

管理员和所有者可以使用产品分析和合规性日志来获得不同类型的可见性。全局管理控制台提供受支持的 ChatGPT 和 Codex 采用和信用使用视图；可用的用户、产品、智能体和模型细分取决于分析使用界面和工作区。对于符合条件的工作区，合规性 API 提供涵盖的 ChatGPT 对话记录，包括支持的云工作活动。覆盖范围取决于产品、使用界面、权限、可用端点和记录的事件架构。请参阅 [工作区分析](workspace-analytics.zh-CN.md) 和 [合规API](compliance-api.zh-CN.md)。

<a id="are-prompts-outputs-files-actions-or-tool-calls-logged"></a>

### 是否记录了提示、输出、文件、操作或工具调用？

对于符合条件的企业和教育工作区，合规性日志平台提供工作用户提示和智能体响应。 [连接的应用程序调用单独记录](https://help.openai.com/en/articles/11509118-admin-controls-security-and-compliance-in-apps-enterprise-edu-and-business) 和符合条件的工作区可以通过支持的 [特定于库的合规性 API 端点](https://help.openai.com/en/articles/20001052-library-for-chatgpt) 访问活动库文件。这些记录不会为每个托管文件操作、shell 命令、浏览器交互、工具调用或批准建立完整的审核跟踪。确认经过身份验证的合规性 API 文档中的当前事件和产品覆盖范围。

合规日志平台将数据保留 30 天。当您的组织需要更长时间的保留时，将记录连续导出到经批准的电子发现、数据丢失防护、SIEM 或数据湖系统。请参阅 [OpenAI 合规平台指南](https://help.openai.com/en/articles/9261474-compliance-api-for-chatgpt-enterprise-edu-and-chatgpt-for-teachers)。

<a id="can-unusual-behavior-failures-or-usage-spikes-be-detected-quickly"></a>

### 是否可以快速检测到异常行为、故障或使用高峰？

工作区分析、合规性日志和连接的监控工具可帮助管理员查看使用情况并调查受支持的 ChatGPT、工作和 Codex 活动。根据所选的报告界面，信号可以包括活动用户、支持的消息、应用程序活动、智能体使用情况、身份验证或管理事件以及信用消耗。导出的日志可以支持电子发现、数据丢失防护、SIEM、审计和调查。检测质量取决于计划、事件覆盖范围、归因、新鲜度和配置的规则。

值得审查的信号包括使用量或信用消耗的意外增加、异常的用户或智能体活动、重复出现的操作错误以及相关的身份验证或管理事件。根据适用的分析、合规性和审核日志模式确认确切的信号。

对于 Codex 活动，Codex 分析和分析 API 提供支持的采用和活动指标。使用本地 Codex 客户端的组织可以选择 OpenTelemetry 导出 API 请求、错误、提示元数据、工具批准决策和工具结果等事件。除非 `otel.log_user_prompt = true` 作为单独的显式选择启用，否则提示内容将被编辑。参见 [监控和遥测](../agent-approvals-security.zh-CN.md#monitoring-and-telemetry)。此本地 Codex 遥测不提供 Web 上 ChatGPT Work 的 OpenTelemetry 导出。

<a id="governance"></a>

## 治理

<a id="how-can-admins-control-access-permissions-and-policies"></a>

### 管理员如何控制访问、权限和策略？

治理跨越三个相关但独立的层面：

- **ChatGPT Work 门禁控制器** 确定谁可以在每个使用界面上使用 ChatGPT Work。
- **工作区智能体控件** 确定谁可以在工作区智能体可用的情况下构建、发布、共享、计划或配置可重用智能体和共享连接。
- **Codex 托管配置** 管理覆盖的本地 Codex 运行时行为，并且不配置托管 ChatGPT Work。

托管配置限制支持的运行时行为。它不会授予工作区访问权限、替换 RBAC 或撤销用户的工作区访问权限。这些层不是一个统一的 ChatGPT Work 策略使用界面。分析和合规日志在其记录的产品和事件范围内提供了额外的可见性。

对于支持的本地Codex客户端，企业管理员可以应用[托管配置](managed-configuration.zh-CN.md)和[权限配置文件](../permissions.zh-CN.md)。这些本地客户端控件不会授予对托管 ChatGPT Work 的访问权限，也不会替换其工作区权限。

<a id="can-access-be-scoped-by-group-role-workspace-or-capability"></a>

### 访问权限是否可以按组、角色、工作区或能力来划分？

是的。在支持自定义成员 RBAC 的合格 Enterprise 和 Edu 计划中，ChatGPT Work 功能的范围可以包括工作区角色、身份组和管理员定义的权限。 ChatGPT Business 使用适用的工作区级别控制，但不包括自定义成员 RBAC 或 SCIM 组同步。根据业务需求和组织策略分配支持的功能。请参阅 [RBAC指南](https://help.openai.com/en/articles/11750701-rbac) 和 [RBAC 演练](https://vimeo.com/1207482321/d1286e4467?share=copy&fl=sv&fe=ci)。

在提供自定义 RBAC 的情况下，组织可以使用它来确定哪些用户可以访问 ChatGPT Work、管理工作区设置、配置批准的插件或使用支持的工作区智能体功能。对于符合条件的 Enterprise 和 Edu 工作区，每月使用限制可以支持通过工作区默认值、组默认值和用户覆盖来分阶段推出。

对连接系统的访问仍然是独立管理的。使用工作区权限、插件设置和源系统的控件，将插件、共享凭据、仓库和可写入操作的范围扩展到所需的最低受众。对于受支持的本地 Codex 客户端，托管配置可以进一步限制本地运行时功能。托管工作遵循其自己的工作区和特定于产品的控制。

<a id="how-are-runtime-and-network-boundaries-governed"></a>

### 如何管理运行时和网络边界？

ChatGPT Work 的安全边界取决于任务。标准聊天对话、连接的工作流程、计划任务和 Codex 聊天可以在具有不同权限、工具和网络访问权限的不同环境中运行。

通过适用的控制措施来管理每个执行环境。 Work Cloud 管理跨受支持的 Web、移动和桌面界面的托管工作。 Work Local 管理本地桌面 Work，Codex Local 控件支持桌面、CLI 和 IDE 客户端中的本地 Codex 访问。浏览器和 shell 网络权限进一步限制 Work Cloud。搜索、应用程序、插件、可用的工作区智能体和源系统权限仍然是单独的控制。适用的托管配置和本地运行时策略仅管理其支持的本地体验。这些控件不可互换。

对于 Codex 活动，本地运行在 ChatGPT 桌面应用程序、CLI 和 IDE 中，并使用操作系统沙箱和审批策略在用户计算机上执行。 Codex 云在隔离的 OpenAI 托管环境中运行聊天。对于支持的本地客户端，企业管理员可以使用托管要求来约束权限配置文件、批准、文件系统和网络访问、MCP 服务器、挂钩、命令规则和其他支持的运行时行为。

<a id="usage-and-cost"></a>

## 使用及费用

<a id="how-does-work-usage-translate-into-spend-over-time"></a>
<a id="how-does-work-mode-usage-translate-into-spend-over-time"></a>

<a id="how-does-chatgpt-work-usage-translate-into-spend-over-time"></a>

### 随着时间的推移，ChatGPT Work 的使用情况如何转化为支出？

[ChatGPT Work 和 Codex 共享定价、额度和使用限制](../pricing.zh-CN.md)。对于符合资格的基于额度的协议，请根据共享工作区额度分配检查员工的聊天和工作综合使用情况。消耗因模型、适用的推理或速度设置、处理的输入和输出以及合格的工具或功能而异。

使用承诺的额度不会自动增加您的发票。实际费用取决于剩余额度余额、合同费率、帐户超额资格和配置的工作区超额限制。有关规划示例、有效用户限制、报告范围和计费详细信息，请参阅 [ChatGPT Work：用途和成本](chatgpt-work-usage-and-cost.zh-CN.md)。

差异最大的模式通常是频繁运行、检索或处理大量信息、调用多个工具或应用程序、失败后重试或产生大量工件的工作流。成本敏感的示例包括计划或重复工作、大文件、跨企业源的广泛检索、重复的应用程序调用以及处理仓库、运行命令或使用云环境的 Codex 聊天。 Workspace Agent API 触发器还可以在可用的情况下添加使用情况。

使用支出控制、使用情况分析和报告来监控这些模式随时间的变化。检查当前分析使用界面支持的维度的使用情况，并根据业务价值调整限制或推出范围。不要将聚合分析视为每个工作流程的精确成本归因。

工作区分析、合规性日志和连接的监控工具可以帮助管理员查看使用情况并调查支持的活动。检测危险或异常行为的能力取决于计划、日志覆盖范围、归因、数据新鲜度以及监控系统中配置的规则。

<a id="what-usage-limits-alerts-or-caps-are-available"></a>

### 有哪些使用限制、警报或上限？

符合条件的企业和教育工作区可以使用每月每用户限制和工作区范围内的支出控制来进行基于信用的使用：

- **监控信贷消费：** 在全局管理控制台和工作区设置中查看支持的积分使用情况报告。
- **设置默认每月限额：** 为工作区建立默认的每用户信用限额。
- **应用特定于组的限制：** 为群组提供每月每个用户的默认值，以反映其工作流程、职责或推出阶段。
- **创建用户覆盖：** 为特定用户提供不同的限制，而不更改整个组的默认值。
- **审查增加请求：** 如果启用请求，用户可以请求更高的每月限额。批准会创建用户覆盖。
- **控制整体工作区暴露：** 在全局管理控制台中单独配置工作区信用警报和超额限制。警报通知收件人；超额限额控制承诺信用池用完后的合格使用情况。
- **导出使用数据：** 符合条件的企业管理员可以通过统一的成本 API 访问信用使用数据，以进行内部报告或监控。

用户可以查看自己的使用情况，如果启用，还可以请求更多额度，但他们无法更改分配的限制。请参阅 [管理使用限制和超额](https://help.openai.com/en/articles/20001001-manage-usage-limits-and-overages-in-chatgpt-enterprise-and-edu) 和 [支出控制演练](https://vimeo.com/1207484127/0f2029dd01?share=copy&fl=sv&fe=ci)。

<a id="incident-and-revocation-controls"></a>

## 事件和撤销控制

<a id="how-can-admins-stop-access-or-activity"></a>

### 管理员如何停止访问或活动？

在用户删除或事件审核期间，管理员可能需要停止访问、禁用应用程序、撤销共享凭据、暂停计划任务或撤销 Codex 凭据。

撤销路径包括：

- 删除用户的工作区或组访问权限。对于 SCIM 管理的用户，删除身份提供商的访问权限；否则，稍后的同步可以再次配置用户。
- 禁用或限制相关插件或应用程序。
- 通过其所属使用界面撤销共享连接、机器人或服务帐户。工作区所有者和管理员可以单独撤销 Codex 工作区访问令牌。
- 从发布中删除工作区智能体或通过其智能体所有者或工作区管理员将其删除。
- 禁用相关计划任务或 Workspace Agent API 触发器（如果可用）。
- 对于Codex访问，分别撤销相关的访问令牌、仓库连接和云环境访问。托管配置不是访问撤销机制。

<a id="additional-resources-for-your-teams"></a>

## 为您的团队提供的额外资源

| 主题 | 在解释 | 时使用此内容 了解 ChatGPT 页 |
| ------------------------ | ----------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| 工作概述 | 云执行、浏览器访问、网络策略和数据边界如何工作 | [ChatGPT Work概述](chatgpt-work-overview.zh-CN.md) |
| 工作区设置和 RBAC | 谁可以使用和管理 Codex | [管理员推出指南](admin-setup.zh-CN.md) |
| 身份验证 | ChatGPT 登录、API 密钥登录和工作区策略有何不同 | [认证](../auth.zh-CN.md) |
| 批准和沙箱 | Codex 如何控制文件、命令、网络和副作用工具操作 | [智能体审批和安全](../agent-approvals-security.zh-CN.md) |
| 托管策略 | 管理员如何强制执行 Codex 设置，用户无法覆盖 | [受管配置](managed-configuration.zh-CN.md) |
| 运行时环境 | Codex 云设置、机密、缓存和任务阶段如何工作 | [云环境](../environments/cloud-environment.zh-CN.md) |
| 互联网接入 | Codex 云域白名单和 HTTP 方法的工作原理 | [智能体互联网接入](../cloud/internet-access.zh-CN.md) |
| 权限 | 文件系统、网络和拒绝读取控件如何工作 | [权限](../permissions.zh-CN.md) |
| 可观察性 | 分析、报告和合规性导出的工作原理 | [治理](governance.zh-CN.md) |
| 自动化凭证 | 如何创建、限制、撤销和审核访问令牌 | [访问令牌](access-tokens.zh-CN.md) |

<a id="recommended-admin-actions"></a>

## 建议的管理员操作

- **确认谁应该首先访问。** 决定是限制对 ChatGPT Work 的访问、运行试点还是广泛推广。许多组织都是从高级用户、拥护者或具有明确用例的团队开始的。
- **查看角色和权限。** 在**权限和角色**中，确认哪些用户或组可以访问ChatGPT Work。将访问权限与业务需求、准备情况和治理期望相匹配。
- **查看插件和数据源。** ChatGPT Work 对于经批准的业务环境（例如文件、电子邮件、日历、Slack 或 CRM）最有用。检查已启用的插件、其受众以及应用程序策略是否仍符合用户应如何委派工作。
- **为适当的用例设定期望。** 将 ChatGPT Work 定位于多步骤、更高价值的任务，例如研究、综合、分析、文件创建、工作流程更新和可重复使用的输出。使用聊天功能进行快速提问、简单重写或集思广益。
- **审查额度和使用控制。** 由于 ChatGPT Work 可以执行运行时间较长的任务，因此它可以比标准聊天对话使用更多的额度。查看默认值、组默认值、用户覆盖以及有关将工作量与业务价值相匹配的内部指南。
- **确定您的第一个高价值工作流程。** 从清晰、可审查的结果开始，例如客户简报、定期报告、研究综合、跟踪器更新或精美的文档和幻灯片。
- **准备冠军和支持团队。** 首先为冠军、培训领导和支持团队提供资源，以便他们能够回答问题、收集反馈并建立有效授权的模型。
- **传达审核和批准的期望。** 提醒用户，在共享或使用输出之前，人们仍然有责任审查输出、验证重要声明并批准后续行动。
- **监控采用情况并进行调整。** 推出后审查使用情况、反馈、信用消耗和委派工作。使用调查结果来调整访问、指导、培训和扩展。