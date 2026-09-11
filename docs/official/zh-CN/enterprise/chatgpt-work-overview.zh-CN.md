> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/chatgpt-work-overview.md)。

<a id="chatgpt-work-overview"></a>

# Work 企业概览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT Work 和 Codex 共享核心执行、隔离和权限机制，并且属于 ChatGPT 商业或企业协议一部分的相同安全边界。每种体验可用的功能和控件取决于任务是在本地运行还是在云中运行、其可用工具以及适用的工作区策略。

ChatGPT Work 可以使用授权工作区成员可用的信息、文件、应用程序和工具来完成多步骤任务。在网络上，这些任务在云中运行，而不是在成员的设备上运行。

本概述解释了执行边界、网络和应用程序控制、数据处理以及如何在网络上使用 ChatGPT Work 安全地执行任务。可用性和管理控制取决于您的计划和工作区配置。

有关托管执行、连接帐户权限、浏览器和网络设置、保留和审核可见性的重点审查，请参阅 [ChatGPT Work 云安全](chatgpt-work-cloud-security.zh-CN.md)。

有关设备访问、本地浏览器会话、托管策略和本地数据处理的信息，请参阅 [ChatGPT Work 本地安全](chatgpt-work-local-security.zh-CN.md)。

<a id="execution-isolation-files-and-device-access"></a>

## 执行隔离、文件和设备访问

ChatGPT Work 可用的文件和工具取决于 Work 的运行位置、用户权限和管理配置。

<a id="local-work"></a>

### 本地工作

本地工作通过用户设备上的 ChatGPT 桌面应用程序运行任务。它可以访问本地文件、应用程序和其他可用资源，但须遵守用户权限、适用的工作区控制和设备安全策略。与 Web 上的工作不同，本地工作可以对计算机上保留的资源进行操作，而无需将文件上传到云对话。

<a id="cloud-work"></a>

### 云工作

Cloud Work 可在受支持的 Web、移动和桌面使用界面上使用。它在 OpenAI 管理的基础设施上的隔离环境中运行 Codex 线束。云对话可以在这些界面之间同步，并且支持的任务可以在用户离开对话时继续进行。

在 Web 上工作无法直接访问用户计算机上的文件、应用程序或打开浏览器选项卡。用户可以通过上传文件、将文件添加到支持的项目或使用授权的连接应用程序来提供文件。桌面体验通过其自己的权限控制本地文件和应用程序访问。

当 [图书馆](https://help.openai.com/en/articles/20001052-file-storage-and-library-in-chatgpt) 可用时，符合条件的上传或生成的文件可以保存在那里。管理员可以控制ChatGPT是否自动引用保存的库文件。禁用自动引用不会阻止用户显式访问或附加他们有权使用的文件。

请参见 [代码和 shell 沙箱](../sandboxing.zh-CN.md)、[创建和编辑文档、电子表格和演示文稿](https://help.openai.com/en/articles/20001278-creating-and-editing-documents-spreadsheets-and-presentations-with-chatgpt-work) 和 [ChatGPT中的文件存储和库](https://help.openai.com/en/articles/20001052-library-for-chatgpt)。

<a id="network-access-and-external-destinations"></a>

## 网络访问和外部目的地

工作使用代码/shell执行和云浏览器等工具来完成任务。这些工具中的每一个都具有可配置的权限。

- **代码和 shell 命令**：公共互联网访问取决于适用的工作区策略和个人工作网络设置。当不允许公共互联网访问时，命令仍然可以到达 Work 正常运行所需的 OpenAI 批准的目的地。这控制网络目标，而不是可以运行哪些命令。
- **网页搜索**：搜索具有独立于工作代码和 shell 网络设置的控件。

如果可用，单独的代码和 shell 设置将显示在 **设置** > **数据控制** > **工作网络接入** 下。打开 **允许公共互联网访问** 不会覆盖适用的管理员限制。关闭它会将代码和 shell 命令限制到托管白名单上所需的目的地；它不会禁用连接的应用程序、网络搜索或云浏览器。

对代码和 shell 网络设置的更改在当前运行完成并且 Work 刷新其执行环境后生效。请参见 [代码和 shell 沙箱](../sandboxing.zh-CN.md) 和 [工作访问控制](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)。

传出交互控制与 [工作区IP访问限制](https://help.openai.com/en/articles/12111596-ip-allowlisting-for-chatgpt) 分开，这限制了对 ChatGPT 工作区或合规性 API 的传入访问。

<a id="cloud-browser-and-website-access"></a>

## 云浏览器和网站访问

[云浏览器](https://help.openai.com/en/articles/20001280-using-cloud-browser-in-chatgpt) 是 ChatGPT Work 可以使用的工具之一，与 [应用内浏览器](https://help.openai.com/en/articles/20001277-using-the-built-in-browser-in-the-chatgpt-desktop-app) 不同。它远程操作并使用与用户本地浏览器分开的浏览器会话。它无法访问本地选项卡、扩展程序、浏览历史记录、保存的密码或经过身份验证的本地会话。

云浏览器可以导航公共网站，将信息输入支持的公共表单，并将来自批准的应用程序的相关信息与网站任务结合起来。 Enterprise 或 Edu 工作区中不支持通过云浏览器登录网站。浏览器可用性取决于您的计划、区域、部署和工作区权限。对于企业工作区，除了工作访问之外，管理员还必须启用云浏览器访问。

网站访问和操作具有单独的控制：

- 默认情况下，ChatGPT 在访问新网站之前会询问。如果可用，用户可以选择 **总是问**、**自动批准** 或 **始终允许**，并允许或阻止单个网站。 **自动批准** 应用自动风险检查。 **始终允许** 删除交互式网站访问审核。管理员具有相同的能力来限制用户的审批设置（例如，在工作区范围内禁用 **始终允许**）。
- 允许网站并不批准该网站上的所有操作。 ChatGPT 可以在可能创建财务、法律、账户或其他后果性承诺的行动之前请求单独确认。

用户可以在工作对话中检查可用的页面屏幕截图和浏览器重播。这些用户可见的记录不会建立合规性 API 导出或完整的管理员可见的执行历史记录。

请参见 [在ChatGPT中使用云浏览器](https://help.openai.com/en/articles/20001280-using-cloud-browser-in-chatgpt) 和 [浏览器](../browser.zh-CN.md)。

<a id="connected-applications-credentials-and-permissions"></a>

## 连接的应用程序、凭据和权限

连接的应用程序或插件只能通过工作区允许的集成以及为该连接授予的权限来提供工作访问权限。管理员可以在管理仪表板中控制插件和应用程序的可用性、工作区角色访问、外部授权、操作设置和源系统权限。

对于 Enterprise 和 Edu 工作区，插件及其底层应用程序默认处于关闭状态。对于业务工作区，插件和应用程序默认处于启用状态。使插件可用并不会自动启用其所需的应用程序或授予帐户访问权限。在 ChatGPT Work 可以访问之前，必须为个人、共享或智能体拥有的帐户授权所需的连接。共享或智能体拥有的连接使用连接帐户的源系统权限，该权限可能与请求用户的权限不同。

在支持的情况下，管理员可以将应用程序限制为只读操作或一组批准的操作。应用程序权限设置还可以确定 ChatGPT 在使用应用程序、进行更改或执行重要操作之前是否进行询问。并非每个应用程序都支持相同的操作控件，也并非每个操作都需要单独的人工确认。

对于同步的应用程序，对源内容或权限的更改可能需要一段时间才会显示。断开应用程序的连接不会自动删除已保存在对话、生成的文件或具有其自己的保留策略的记录中的信息。

请参见 [插件和应用程序的管理控制、安全性和合规性](https://help.openai.com/en/articles/11509118-admin-controls-security-and-compliance-in-apps-enterprise-edu-and-business)、[插件控件](apps-and-connectors.zh-CN.md)、[Google Workspace 管理员管理的设置](https://help.openai.com/en/articles/10929079-google-workspace-admin-managed-setup)、[ChatGPT 具有同步功能的应用程序](https://help.openai.com/en/articles/10847137-chatgpt-apps-with-sync)。

<a id="privacy-and-data-handling"></a>

## 隐私和数据处理

ChatGPT Work 遵循适用于您的 ChatGPT 工作区的隐私、安全和数据处理政策。对话、上传的文件、生成的文件、连接的应用程序和浏览器数据可以有不同的保留和删除规则。

详细信息请参见[企业隐私](https://openai.com/enterprise-privacy/)、[聊天和文件保留策略](https://help.openai.com/en/articles/8983778-chat-and-file-retention-policies-in-chatgpt)、[数据驻留和推理驻留](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt)和[ChatGPT Work 管理员常见问题解答](work-admin-faq.zh-CN.md)。

<a id="retention-depends-on-the-data-type"></a>

### 保留取决于数据类型

- **工作对话：** 遵循适用的 ChatGPT 工作区对话保留和删除设置。
- **保存到库的文件：** 遵循适用的文件和工作区保留规则。删除对话不会删除存储在库中的文件。
- **项目文件：** 保留在项目中直至删除，但须遵守适用的删除规则和例外情况。
- **库外的瞬时上传：** 对于企业版，瞬时上传可能会在 48 小时后过期，除非应用不同的保留设置。
- **启用后保存的记忆：** 遵循单独的内存控制。
- **云浏览器cookie：** 与本地浏览器数据保持分离。用户可以从云浏览器设置中清除它们。
- **合规日志平台记录：** 在平台上保留 30 天。导出的副本遵循接收系统的保留策略。
- **连接的应用程序数据：** 源记录遵循连接的应用程序的策略。保存在聊天、文件或同步索引中的副本也遵循适用的 OpenAI 存储和保留规则。

删除对话、结束工作任务、清除浏览器 cookie 和保留合规性记录是不同的操作。删除聊天会将其从视图中删除，并计划在 30 天内永久删除，但须遵守已发布的安全、法律和去识别化例外情况。

请参阅 [聊天和文件保留策略](https://help.openai.com/en/articles/8983778-chat-and-file-retention-policies-in-chatgpt)、[ChatGPT 中的内存](https://help.openai.com/en/articles/8590148-memory-in-chatgpt-faq) 和 [OpenAI 合规平台](https://help.openai.com/en/articles/9261474-compliance-api-for-chatgpt-enterprise-edu-and-chatgpt-for-teachers)。