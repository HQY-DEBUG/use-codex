> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/plugins.md)。

<a id="plugins"></a>

# 插件

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="overview"></a>

## 概述

插件将功能捆绑到 ChatGPT 和 Codex 中的可重用工作流程中。它们可以包括技能和 MCP 服务器。这两种产品都使用一个通用插件目录，因此可以从其支持的界面中发现相同的公共插件。

插件可在 Web、桌面和移动设备上的 ChatGPT 的聊天和工作中以及 ChatGPT 桌面应用程序中的 Codex 中使用。 Codex CLI 还具有适用于 Codex 环境的插件浏览器。 IDE 扩展不支持插件。

在移动设备上，您可以在聊天或工作中使用您的帐户可用的插件。

标记为 **仅限桌面版** 的插件需要 ChatGPT 桌面应用程序。您可以在网络上找到这些插件，但必须打开 ChatGPT 桌面应用程序才能安装和使用它们。它们在移动设备上不可用。

<ContentModeSwitch group="codex-surface" id="app">

打开 **插件** 选项卡以浏览并安装插件。安装后，您可以在ChatGPT或Codex的聊天或工作中使用插件。安装的插件可以将技能和MCP工具添加到新的聊天中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

打开 **插件** 选项卡以浏览并安装插件。安装后，您可以在聊天或工作中使用插件。插件可以在其工具可用之前提示您连接外部服务。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在 Codex CLI 中，输入 `/plugins` 打开插件浏览器。从配置的市场安装插件，然后在使用其捆绑的技能或工具之前启动新会话。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="plugin-directory-in-the-ide-extension"></a>

<a id="use-plugins-from-a-supported-surface"></a>

### 使用受支持使用界面的插件

插件在 IDE 扩展中不可用。要浏览和安装 Codex 的插件，请使用 ChatGPT 桌面应用程序或 Codex CLI。

</ContentModeSwitch>

扩展ChatGPT和Codex的功能，例如：

- 安装 Codex Security 插件来扫描授权代码并确认看似合理的漏洞发现。
- 安装 Gmail 插件以使用 Gmail。
- 安装 Google Drive 插件以跨云端硬盘、文档、表格和幻灯片工作。
- 安装 Slack 插件来汇总频道或草稿回复。

插件可以包含以下一个或多个部分：

- **技能：** 针对特定类型工作的可重复使用指令。 ChatGPT 和 Codex 可以在需要时加载它们，以便它们遵循正确的步骤并为任务使​​用正确的引用或帮助程序脚本。
- **MCP服务器：** 服务，将 ChatGPT 和 Codex 连接到 GitHub、Slack 或 Google Drive 等系统中的工具和信息。他们定义工具、强制身份验证、返回结构化数据并对外部系统执行操作。它们可以选择包含自定义 UI。
- **浏览器扩展：** 插件工作流程所需的浏览器功能。
- 在 Codex 运行时中配置的生命周期点运行的 **挂钩：** 命令，包括 ChatGPT Work 和 Codex。 Hook脚本必须在执行环境中可用；在网络上安装插件不会部署这些脚本。企业管理员可以通过移动设备管理（MDM）部署所需的脚本。在运行之前检查并信任插件挂钩。有关设置和托管挂钩策略，请参阅 [挂钩](hooks.zh-CN.md)。

您可以通过市场来源（例如项目或团队的回购市场）发布插件来共享插件。请参阅 [构建插件](https://developers.openai.com/plugins/build/plugins) 了解市场设置、打包和分发指南。

如果您正在构建集成，请从 [搭建MCP服务器](https://developers.openai.com/plugins/build/mcp-server) 开始。如果插件需要自定义 UI，请使用 [可选的用户界面指南](https://developers.openai.com/plugins/build/chatgpt-ui)。

<a id="use-and-install-plugins"></a>

## 使用和安装插件

<a id="plugin-directory-in-the-codex-app"></a>

<ContentModeSwitch group="codex-surface" ids="app,web">

<a id="universal-plugin-directory"></a>

### 通用插件目录

ChatGPT 和 Codex 使用相同的公共插件目录。在网络或 ChatGPT 桌面应用程序中，打开 **插件** 选项卡以浏览和安装插件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">


  

> 插图：ChatGPT 桌面应用程序中的插件目录




</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,web">

插件目录将插件组织到选项卡中：

- **OpenAI：** 插件由 OpenAI 构建。
- 您的工作区提供的 **您的工作区名称：** 插件。
- **个人：** 个人市场插件，包括 **由我创建** 和 **与我分享** 部分（当这些插件可用时）。

使用单独的 **已安装** 行查看已安装的插件。

工作区管理员可以为其团队导入和同步 GitHub 市场。有关设置和访问要求，请参阅 [插件管理](enterprise/plugin-management.zh-CN.md)。

<a id="install-and-use-a-plugin"></a>

### 安装并使用插件

打开插件目录后：

<WorkflowSteps>

1. 搜索或浏览插件，然后打开其详细信息。
2. 选择加号按钮来安装插件。
3. 如果插件需要 MCP 服务器连接，请在出现提示时连接。有些插件要求您在安装过程中进行身份验证。其他人会等到您第一次使用它们时才使用它们。
4. 安装后，开始新的聊天并要求ChatGPT或Codex使用该插件。

</WorkflowSteps>

<a id="connect-supported-partners-with-sign-in-with-chatgpt"></a>

### 通过使用 ChatGPT 登录来连接受支持的合作伙伴

**使用 ChatGPT 登录** 正在针对支持的插件和合作伙伴网站推出测试版，包括 Airtable、GitLab、HubSpot、Notion、Supabase 和 Vercel。当该选项可用时，在连接插件时选择 **使用 ChatGPT 登录** 以创建帐户或将您的帐户链接到该服务。

登录时仅与合作伙伴共享您的姓名、电子邮件地址和个人资料图片（如果有）。它不会授予插件访问您的数据的权限或自动批准操作。在使用连接之前，请作为单独的步骤查看并批准插件请求的权限。

安装插件后，可以在提示窗口中直接使用：

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">


  

> 插图：在Plugins页面安装插件




</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,web">



  

    
直接描述任务

    

询问您想要的结果，例如“汇总今天未读的 Gmail 线程”或“从 Google Drive 中提取最新的发布说明”。
    

    

当您希望 ChatGPT 为任务选择正确的安装工具时，请使用此选项。
    

  


  

    
选择特定插件

    

输入 `@` 显式调用插件或其捆绑技能之一。
    

    

当您想要具体了解 ChatGPT 应该使用哪个插件或技能时，请使用此选项。参见 [技能和插件](skills-and-plugins.zh-CN.md)。
    

  




</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="use-apple-messages-from-codex"></a>

### 使用来自 Codex 的 Apple Messages

Apple Messages 插件适用于 macOS 的 ChatGPT 桌面应用程序中的所有计划。在 Codex 和 ChatGPT Work 中，它可以读取和搜索 Mac 上的 iMessage、SMS 和 RCS 聊天记录，并通过“消息”应用程序代表您发送消息。它不允许您通过消息与 ChatGPT 远程交互，并且在常规 ChatGPT 聊天中不起作用。

对于此版本，消息插件仅包含在 ChatGPT 桌面应用程序的 Apple Silicon (arm64) 版本中。

<WorkflowSteps>

1. 打开**插件**，找到Apple Messages插件并安装。
2. 启动新的 Codex 或 ChatGPT Work 聊天并要求其查找、总结、起草或发送消息。
3. 在 ChatGPT 读取消息之前授予请求的 macOS 权限。
4. 在允许发送之前查看邮件及其收件人。

</WorkflowSteps>

默认情况下，ChatGPT 仅在您批准消息及其收件人后才发送消息。选择 **允许一次** 仅批准该发送。如果您选择 **始终允许发送至此聊天**，ChatGPT 可以将未来的消息发送到该消息聊天，而无需其他发送批准。

对可能包含不可信或误导性指令的聊天保留每次发送批准。持续批准会消除您在 ChatGPT 向您发送消息之前查看消息的最后机会。仅当您接受该风险时才使用它。

要恢复每次发送批准，请打开 **设置** > **电脑使用** 并选择 **留言** 旁边的 **管理**。在 **始终允许发送** 下，选择聊天旁边的垃圾桶图标，然后确认 **删除**。 ChatGPT 在再次发送到该聊天之前会询问。

**已知问题：** 如果您的任务设置为 **完全访问** 或以其他方式禁用批准提示，Apple Messages 可能无法显示发送所需的确认信息。切换到**请求批准**或**代我批准**并重试。

Apple Messages 在 Mac 上运行。它不能直接在 Web 或移动设备上的 ChatGPT、Codex CLI 或 IDE 扩展中使用。

在托管工作区中，管理员可以通过现有的计算机使用控件禁用 Apple Messages。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="plugin-directory-in-codex-cli"></a>

<a id="plugin-browser-in-codex-cli"></a>

### Codex CLI 中的插件浏览器

在 Codex CLI 中，运行以下命令打开插件浏览器：

```text
codex
/plugins
```


  

> 插图：Codex CLI 中的插件列表




CLI 插件浏览器按市场对插件进行分组。使用市场选项卡切换源、打开插件以检查详细信息、安装或卸载市场条目，然后按<kbd>Space</kbd>在已安装的插件上打开或关闭它。

</ContentModeSwitch>

<a id="api-key-availability"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli">

<a id="api-key-availability"></a>

### API 密钥可用性

如果您使用 [使用 OpenAI API 密钥登录 Codex](auth.zh-CN.md#sign-in-with-an-api-key)，则可以在 Codex CLI 中浏览、安装和管理受支持的 OpenAI 策划的插件，以及在 ChatGPT 桌面应用程序中浏览、安装和管理 Codex。某些插件无法使用 API 密钥身份验证，因为它们的连接流需要不受支持的 OAuth 功能。查看 [平台使用页面](https://platform.openai.com/usage) 上的插件使用情况。

</ContentModeSwitch>

<a id="how-permissions-and-data-sharing-work"></a>

### 权限和数据共享如何工作

<ContentModeSwitch group="codex-surface" id="web">

在 Web 上的 ChatGPT 中，聊天和工作使用该聊天可用的工作区权限和工具。 MCP 服务器仍然需要自己登录和访问。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli">

当插件功能通过 Codex 主机运行时，主机的 [沙箱和审批政策](agent-approvals-security.zh-CN.md) 适用。与外部服务的连接使用该服务自己的身份验证和访问控制。

</ContentModeSwitch>

- 当您在安装后开始新的聊天或 CLI 会话时，捆绑技能将可用。
- 如果插件包含 MCP 服务器，则它们可能需要额外的设置或身份验证才能使用它们。
- 当 ChatGPT 通过 MCP 服务器发送数据时，该服务的条款和隐私政策适用。

<a id="remove-a-plugin"></a>

### 删除插件

要删除插件，请从支持的插件浏览器中打开它，并在该操作可用时选择 **卸载插件**。工作区安装的插件或默认插件可能不提供该操作；相反，您的工作区管理员可以控制它们。

卸载插件将从 ChatGPT 或 Codex 环境中删除插件包。单独连接的 MCP 服务器集成在 ChatGPT 中保持连接，直到您在那里断开它们的连接。

<a id="build-your-own-plugin"></a>

## 构建您自己的插件

如果您想创建、测试或分发自己的插件，请参阅 [构建插件](https://developers.openai.com/plugins/build/plugins)。该页面涵盖本地脚手架、手动市场设置、工作区共享、插件清单和打包指南。

如果您的插件包含服务器支持的功能，请参阅 [搭建MCP服务器](https://developers.openai.com/plugins/build/mcp-server)。 MCP 工具可以在没有自定义 UI 的情况下工作，或者当可视化界面有助于工作流程时返回 UI。

当您的插件准备好接受审核时，请参阅 [提交插件](https://developers.openai.com/plugins/deploy/submission) 了解 OpenAI 平台提交流程、所需权限、审核材料、MCP 检查和测试用例要求。

<a id="plugin-guides"></a>

## 插件指南

- [录制与回放](extend/record-and-replay.zh-CN.md)：向ChatGPT展示一次工作流程并将其变成可重复使用的技能。
- [Codex Security插件](security/plugin.zh-CN.md)：扫描授权代码，确认结果并准备经过审核的修复程序。