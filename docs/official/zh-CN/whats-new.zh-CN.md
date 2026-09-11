> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/whats-new.md)。

<a id="whats-new"></a>

# 近期功能更新

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

本每周摘要重点介绍了可以改变您工作方式的 ChatGPT 和 Codex 功能，并提供示例和链接以了解更多信息。对于每个版本更新、错误修复和细微改进，请参阅 [Codex 变更日志](https://learn.chatgpt.com/docs/changelog)。

<a id="august-31september-4-2026"></a>

## 2026年8月31日至9月4日

<a id="take-on-demanding-work-with-gpt-6-astra"></a>

### 使用 GPT-6 Astra 承担高要求的工作

[GPT-6 阿斯特拉](models.zh-CN.md#gpt-6-astra) 结合了先进的推理、计算机使用和对 Codex 和 ChatGPT Work 中代码、应用程序和研究的复杂工作的更强判断力。使用它来执行工作流程、检查结果并生成适合您的模板和任务的文档、电子表格或演示文稿。

一旦 Astra 可用于您的帐户，请从模型选择器中进行选择。在开始大型任务之前请参阅 [使用和定价](pricing.zh-CN.md)。企业访问需要部署资格和管理员才能启用。

<a id="august-2428-2026"></a>

## 2026 年 8 月 24 日至 28 日

<a id="work-with-more-websites"></a>

### 与更多网站合作

- **使用您的浏览器：** 通过 ChatGPT 桌面应用程序在 [Edge、Brave、Opera 或 Vivaldi](chrome-extension.zh-CN.md) 和 Chrome 中工作。将打开的选项卡带入 ChatGPT Work 或 Codex 聊天并使用您已登录的网站。Opera 支持浏览器控制，但没有侧边聊天功能。

- **使用网站的工具：** 通过 [站点工具 (WebMCP)](webmcp.zh-CN.md)、ChatGPT Work 和 Codex 可以在桌面应用程序的内置浏览器中使用网站提供的操作。例如，文档编辑器可以提供用于查找部分或添加注释的工具。更新桌面应用程序并使用 GPT-5.6 Sol 或 GPT-5.6 Terra。 GPT-5.6 Luna 或 Enterprise 或 Edu 工作区中不提供站点工具。

- **通过云浏览器登录：** 在符合条件的计划中，继续需要 ChatGPT Work Web、iOS 或 Android 上的网站帐户的任务。按照 [登录请求](https://learn.chatgpt.com/docs/browser?surface=web#web-sign-in-to-a-website) 操作，并在登录流程中（而不是在聊天中）输入您的详细信息。这不会连接您的本地浏览器配置文件。网站登录不适用于 Enterprise 或 Edu 工作区。

可用性取决于部署和工作区设置。



**提示：**

```text
使用@Edge读取当前页面并将其变成简洁的清单。
```

[阅读 8 月 25 日浏览器发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-25-browser)。

<a id="run-scheduled-tasks-from-app-events"></a>

### 从应用程序事件运行计划任务

现在，当 Gmail、Slack 或 GitHub 中发生受支持的事件时，[计划任务](https://learn.chatgpt.com/docs/automations?surface=web#web-trigger-tasks-from-app-events) 可以启动。使用事件触发器来分类新电子邮件、总结渠道活动或根据拉取请求反馈采取行动，而无需按固定节奏进行轮询。

对于符合条件的计划，可在网络版和移动版 ChatGPT 中使用事件触发任务。连接相关应用程序并首先批准其请求的访问权限。在托管工作区中，管理员可以控制访问。



**提示：**

```text
当我在 <owner>/<repository> 中的拉取请求之一收到新的审核反馈时，总结反馈并准备修订计划。
```

[阅读 8 月 25 日的发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-25-event-triggers)。

<a id="august-1721-2026"></a>

## 2026 年 8 月 17 日至 21 日

<a id="work-with-more-of-your-apps-and-content"></a>

### 使用更多应用程序和内容

- **苹果消息：** [在 Mac 上查找聊天、总结消息、准备回复以及通过“消息”发送](https://learn.chatgpt.com/docs/plugins?surface=app#app-use-apple-messages-from-codex)。该插件适用于 macOS ChatGPT 桌面应用程序中的所有计划。在 ChatGPT Work 和 Codex 中使用它，而不是在常规 ChatGPT 聊天中使用它。默认情况下，ChatGPT 仅在您批准消息及其收件人后才发送消息。

- **网站共同编辑：** 如果有的话，[邀请工作区的活跃成员作为编辑](sites.zh-CN.md#collaborate-on-a-site)。编辑者可以在网站所有者首次发布网站后发布更新。受邀编辑可以读取网站的实时数据库数据；所有者保留对共享和设置的控制。

- **可编辑的站点 URL：** 如果 [为现有站点选择新的 ChatGPT 托管地址](sites.zh-CN.md#change-a-site-url) 可用，无需重新部署。之前的地址重定向到新地址。

- **Computer History 欧洲：** 在欧洲经济区、瑞士和英国使用 [电脑历史记录](customization/computer-history.zh-CN.md)。对于 macOS 上的 ChatGPT Pro、Business 和 Enterprise 用户，它默认保持关闭状态。业务和企业管理员必须首先启用访问。

- **共享线程快照：** [共享本地 Codex 线程的只读快照](use-chatgpt.zh-CN.md#share-a-read-only-snapshot-of-a-codex-thread) 来自适用于 macOS 的 ChatGPT 桌面应用程序。任何知道该链接的人都可以查看个人帐户链接；工作区帐户链接仅限于原始工作区。 Codex 会编辑已知的秘密模式，但在共享之前查看快照，因为可能会保留敏感内容。

- **统一固定螺纹：** 让您的 [固定聊天](https://learn.chatgpt.com/docs/projects?surface=app#app-organize-projects-and-chats) 在桌面和 iOS 之间保持同步。



**提示：**

```text
查找有关明天发布的最新消息对话，总结未解决的问题，并起草回复而不发送。
```

[阅读 8 月 20 日的发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-20-app)。



> Codex 和 ChatGPT Work 中的共享线程可让您通过只读链接显示构建背后的过程

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2090555241343418814) (2026-08-20)

<a id="work-with-gitlab-projects-in-codex-cloud"></a>

### 在 Codex 云中使用 GitLab 项目

[GitLab 支持](third-party/gitlab.zh-CN.md) 在所有 ChatGPT 计划中均提供测试版。连接项目、创建云环境、从问题启动任务或使用 `@codex` 合并请求，并请求一次性或自动合并请求审核。

该集成在 Codex 云中运行，托管工作区管理员可以禁用它。 GitLab 触发的活动需要配置适用 Webhook 的权限。 GitLab 自管理和 GitLab 专用连接需要工作区管理员设置； Webhook 活动需要 GitLab 19.0 或更高版本。

[阅读 8 月 19 日 GitLab 发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-19-gitlab)。

<a id="export-public-plugin-metadata-for-review"></a>

### 导出公共插件元数据以供审核

符合条件的 ChatGPT Enterprise 工作区所有者和管理员可以下载其工作区可见的公共插件的 CSV。在 [管理 > 插件](https://chatgpt.com/admin/plugins) 中，选择 **公共**，然后选择下载图标 (**导出 CSV**)。

导出列出了插件、应用程序和聊天技能的名称和描述，以及开发人员、版本、UTC 中添加的日期以及 OpenAI 验证元数据。它使用长达 48 小时的公共目录快照，并且不包括为工作区创建的插件。导出在 FedRAMP 工作区中不可用。

[阅读 8 月 17 日管理导出发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-17-admin-csv)。

<a id="august-1014-2026"></a>

## 2026 年 8 月 10 日至 14 日

<a id="find-earlier-work-with-computer-history"></a>

### 使用 Computer History 查找早期作品

[电脑历史记录](customization/computer-history.zh-CN.md) 将应用程序和网站上的活动转换为 ChatGPT 和 Codex 可以使用的可搜索时间线和记忆。仅当您想要共享该上下文时才将其打开，然后选择哪些应用程序和网站提供内容、暂停收集以及随时查看或删除您的历史记录。

Computer History 可在 macOS 上的 ChatGPT 桌面应用程序中为 ChatGPT Pro、Business 和 Enterprise 客户提供。业务和企业管理员必须首先启用访问。最初的可用性不包括欧盟、瑞士和英国。



**提示：**

```text
找到我之前查看的文档和 Slack 线程，然后总结我仍然需要采取行动的决定。
```



[手表：ChatGPT 中的 Computer History](https://www.youtube.com/watch?v=W-HhMUe9hOg)

<a id="use-the-chatgpt-desktop-app-on-linux"></a>

### 在 Linux 上使用 ChatGPT 桌面应用程序

[适用于 Linux 的 ChatGPT 桌面应用程序](linux/linux-app.zh-CN.md) 现已提供预览版。在受支持的 Ubuntu 或 Debian 发行版上安装 `.deb` 软件包，或在 Fedora 上安装 `.rpm` 软件包。软件包适用于 x64 和 ARM64 处理器。

使用您的 ChatGPT 帐户登录以处理项目、本地文件和 Codex。 Linux 预览版中尚未提供包括计算机使用在内的某些功能。



> 现已预览：适用于 Linux 的 ChatGPT 桌面应用程序。

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2087231350134980830) (2026-08-11)

<a id="bring-your-existing-agent-setup-and-work-with-you"></a>

### 带上您现有的智能体设置并与您合作

[导入说明、设置、技能、插件、项目和最近的工作](import.zh-CN.md) 从 **克劳德·科德**、**克劳德·科沃克** 或 **光标** 进入 ChatGPT 桌面应用程序。在 **设置 > 导入** 中打开自动更新，使导入的作品保持同步。

在 Codex CLI 中，使用 `/import` 将受支持的设置和最近的聊天从 Claude Code 或 Cursor 引入本地会话。

[阅读 8 月 11 日桌面和 CLI 发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-08-11-app)。



> 您现在可以使其他智能体的工作与 ChatGPT Work 和 Codex 保持同步。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2087242829076791392) (2026-08-11)

<a id="choose-the-right-access-for-defensive-security-work"></a>

### 为防御安全工作选择正确的访问权限

Daybreak 现在为经过批准的防御者提供两个级别。 **黎明蓝**支持一般防御工作，例如安全代码审查、事件响应和补丁验证。 **黎明红** 需要自己的批准，并提供对经过专门训练的模型进行授权安全评估的访问。

访问需要 [网络可信访问](cyber-safety.zh-CN.md#trusted-access-for-cyber)，并且仅适用于批准的身份、工作区或组织、模型和产品使用界面。

[阅读 8 月 10 日黎明公告](https://learn.chatgpt.com/docs/changelog#codex-2026-08-10-daybreak)。



> 我们正在扩大我们的网络安全计划 Daybreak

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2086864365379010729) (2026-08-10)

<a id="august-37-2026"></a>

## 2026 年 8 月 3 日至 7 日

<a id="talk-through-files-and-projects-with-chatgpt-voice"></a>

### 使用 ChatGPT 语音讨论文件和项目

[ChatGPT 语音](features/voice.zh-CN.md)现在支持上传文件和[ChatGPT 项目](projects.zh-CN.md)。在语音对话期间询问有关文档的问题，或使用最近的聊天、来源和说明继续项目。



**提示：**

```text
查看我上传的研究简介，大声解释主要的权衡，并将它们与该项目中已有的来源进行比较。
```

<a id="study-and-teach-with-dedicated-education-plugins"></a>

### 使用专用的教育插件进行学习和教学

三个新的 [插件](plugins.zh-CN.md) 为 ChatGPT Work 和 Codex 带来了课堂特定的工作流程。 **大学生** 创建学习指南、练习测验、抽认卡和交互式解释。 **大学教育家** 帮助制定课程计划、材料和评估。 **K-12 教育工作者**支持适合不同学习者的课程计划、课堂资源和材料。

这些插件可通过 ChatGPT Edu 和 ChatGPT 进行教师区部署。学校控制哪些工具和权限可用。阅读 [教育插件公告](https://openai.com/index/learn-teach-chatgpt-work-codex/)。

<a id="reuse-saved-files-and-find-past-work-faster"></a>

### 重复使用保存的文件并更快地找到过去的工作

在网络上，将保存的库文件添加到对话中而无需再次上传，在库中搜索并粘贴格式化文本而不会丢失标题、链接或列表。搜索还可以匹配网络、iOS 和 Android 上的文件夹和对话标题。

超过 10,000 个字符的粘贴现在成为每个 ChatGPT 计划（包括 Enterprise 和 Edu）的附件。如果您想将内容移回到消息中，请选择 **在文本字段中显示**。

阅读 [ChatGPT 发行说明](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)。

<a id="see-your-remaining-chatgpt-work-usage"></a>

### 查看您的剩余 ChatGPT Work 使用情况

个人套餐和 ChatGPT 商业版的合格用户可以直接在网页侧边栏中查看其剩余的 ChatGPT Work 使用情况。可用的额度选项取决于您的帐户和工作区权限。 ChatGPT Work 和 Codex 继续共享相同的 [使用限制和额度](pricing.zh-CN.md)。

<a id="choose-how-gpt-56-responds-in-chatgpt"></a>

### 选择 GPT-5.6 在 ChatGPT 中的响应方式

ChatGPT Plus 和 Pro 用户可以使用新滑块调整 GPT-5.6 Sol 投入响应的程度。更新后的模型还提供了更可靠的事实和更有针对性的答案。 GPT-5.6 Luna 成为 Free and Go 计划中的默认 ChatGPT 模型。

这些更改适用于 ChatGPT 对话。它们不会更改 ChatGPT Work 或 Codex 中的模型行为。阅读 [ChatGPT 发行说明](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)。

<a id="organize-work-and-switch-agents-in-codex-cli-01470"></a>

### 在Codex CLI 0.147.0中组织工作并切换智能体

[Codex CLI 0.147.0](https://github.com/openai/codex/releases/tag/rust-v0.147.0) 添加了持久的、手动排序的聊天部分和便携式智能体插件。跨本地、个人、工作区和远程插件目录或 [导入光标和克劳德代码设置](import.zh-CN.md) 进行搜索，而无需重复同步对话。

使用 `--approve-for-me` 为符合条件的请求启用 [自动审批审核](sandboxing/auto-review.zh-CN.md)，而无需扩展文件系统或网络权限。 Amazon Bedrock 会话还获得缓存的 Web 搜索和远程对话压缩。



[观看：智能体插件简介](https://www.youtube.com/watch?v=UaeWJK_vv-Y)

<a id="follow-and-resume-deeper-security-scans"></a>

### 跟踪并恢复更深入的安全扫描

托管的 Codex Security 插件版本 `0.1.16` 到 `0.1.18` 添加了实时扫描进度、测量的令牌使用情况、可恢复的深度扫描和可配置的发现限制。最新版本还支持对仓库扫描及其委派工作人员进行 Amazon Bedrock 身份验证。

使用 [Codex Security工作台](security/plugin/workbench.zh-CN.md) 查看扫描进度和结果，或者在需要更彻底的评估时使用 [配置深度扫描](security/plugin/deep-scans.zh-CN.md)。检查 [插件变更日志](security/plugin/changelog.zh-CN.md) 以确认您安装的版本支持哪些功能。

<a id="review-github-pull-requests-for-security-risks"></a>

### 检查 GitHub 拉取请求的安全风险

[Codex Security 评论](security/security-review.zh-CN.md) 分析拉取请求更改以及仓库上下文、威胁模型和安全指南。当拉取请求打开或收到新提交时配置自动审查，或直接使用 `@codex security review` 请求审查。

该功能在研究预览版中向符合条件的 ChatGPT Enterprise、Business、Edu 和 Pro 客户提供。 Plus 上不提供此功能，并且可能存在使用限制。



> 现在处于研究预览阶段：Codex Security 评论

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2085482310636560830) (2026-08-06)

<a id="july-2731-2026"></a>

## 2026 年 7 月 27 日至 31 日

<a id="use-gpt-56-terra-and-luna-at-lower-rates"></a>

### 以较低的费率使用 GPT-5.6 Terra 和 Luna

GPT-5.6 Terra 现在的成本降低了 20%，GPT-5.6 Luna 的成本降低了 80%。输入、缓存输入和输出速率按相同比例下降。更新后的 [使用限制和费率](pricing.zh-CN.md) 使 Terra 更适合日常工作，而 Luna 对于集中编码和大批量任务特别有用。



> 从今天开始，我们将 GPT-5.6 Luna 的价格降低 80%，将 GPT-5.6 Terra 的价格降低 20%

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2082878156483219672) (2026-07-30)

<a id="find-useful-context-across-your-browser-and-open-tabs"></a>

### 在浏览器中查找有用的上下文并打开选项卡

在 ChatGPT 桌面应用程序中，[内置浏览器](browser.zh-CN.md) 可以从浏览历史记录中查找页面或直接从其地址栏搜索 Google。当任务需要较早的上下文时，ChatGPT 还可以搜索您的浏览历史记录。

[Chrome 扩展程序](chrome-extension.zh-CN.md) 可让您提及打开的选项卡、将选定的页面文本带入侧边聊天、询问有关 YouTube 视频的问题或从页面的上下文菜单中选择 **询问 ChatGPT**。在 ChatGPT 将浏览器历史记录包含在任务中之前，请审核并批准使用浏览器历史记录的请求。



> 在侧边聊天中，询问 YouTube 视频、引用您打开的选项卡或突出显示页面上的文本并询问。

[在 X 上查看@ChatGPT](https://x.com/ChatGPT/status/2082970812584432115) (2026-07-30)

<a id="review-changes-across-repositories"></a>

### 查看跨仓库的更改

当 [本地项目包含多个文件夹](projects.zh-CN.md#use-local-projects-for-folders-and-codebases) 时，桌面应用程序会显示每个仓库以及每个仓库中更改的行。选择 **评论** 一起检查它们的差异，而无需在单独的审阅视图之间切换。



**提示：**

```text
在打开拉取请求之前，请检查此项目中每个仓库的更改，识别集成风险并总结所需的修复。
```

<a id="refine-generated-images-in-your-conversation"></a>

### 优化对话中生成的图像

在扩展查看器中打开生成的图像，然后在 **聚焦视图** 和 **画布视图** 之间切换。在图像中添加评论，选择要保留的版本，并要求进行有针对性的编辑，而无需离开聊天。了解有关 [图像生成](image-generation.zh-CN.md) 的更多信息。



> Codex 中的 ImageGen 刚刚获得了新的灯箱和画布。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2082944138635595782) (2026-07-30)

<a id="find-chats-that-need-your-attention"></a>

### 查找需要您关注的聊天内容

桌面应用程序的新 **活动视图** 汇集了您最近参与的聊天和需要您关注的工作。选择侧边栏中的铃铛以打开视图。

[阅读 7 月 30 日桌面版发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-07-30-app)。



> ChatGPT 桌面应用程序中的新活动视图汇集了需要您关注的对话

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2083288643310133716) (2026-07-31)

<a id="connect-partner-tools-with-sign-in-with-chatgpt"></a>

### 通过“使用 ChatGPT 登录”连接合作伙伴工具

**使用 ChatGPT 登录** 正在测试版中向支持的插件和合作伙伴网站推出，首先是 Airtable、GitLab、HubSpot、Notion、Supabase 和 Vercel。使用它以更少的步骤创建或链接合作伙伴帐户，然后开始在 ChatGPT 或 Codex 中使用该服务。

合作伙伴只会收到您的姓名、电子邮件地址和个人资料照片（如果有）。每个插件请求的访问权限仍需要单独的审核和批准。阅读 [7月29日签到公告](https://learn.chatgpt.com/docs/changelog#codex-2026-07-29)。

<a id="collaborate-in-a-dedicated-academic-research-workspace"></a>

### 在专门的学术研究工作区中进行协作

[ChatGPT 面向学术研究人员](https://openai.com/index/chatgpt-for-academic-researchers/) 为符合条件的教师和博士后研究人员提供 12 个月的免费使用专用 ChatGPT 工作区的机会。经批准的团队可以包括来自同一机构的最多五名经过验证的研究人员，并获得业务数据保护和 ChatGPT 专业级使用限制。参与者可以在 ChatGPT、ChatGPT Work 和 Codex 中使用 GPT-5.6 进行研究和编码工作流程。

该计划涵盖 ChatGPT 访问权限，而不是 OpenAI API 额度。资格要求为 [机构验证和合格的研究论文](https://help.openai.com/en/articles/20001406)。



[观看：我们将向 100,000 名学术研究人员免费提供我们的前沿模型](https://www.youtube.com/watch?v=MLehRytu9Zo)

<a id="continue-codex-tasks-more-reliably-on-ios"></a>

### 在 iOS 上更可靠地继续 Codex 任务

当您返回应用程序或使用面容 ID 解锁设备时，适用于 iOS 1.2026.202 的 ChatGPT 会更可靠地重新连接到任务。语音对话使用您选择的 ChatGPT 语音并显示使用限制警告，而输入框现在建议与桌面应用程序一致的已安装插件及其技能。

该版本还改进了目标、内联表和视觉主题、大型工作区差异、选定文本引用和模型恢复的暂停和恢复控制。阅读 [7 月 27 日 iOS 发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-07-27-mobile)。

<a id="compare-security-scans-and-manage-findings"></a>

### 比较安全扫描并管理结果

托管的 Codex Security 插件版本 `0.1.14` 和 `0.1.15` 添加了扫描比较、误报反馈、范围 `SECURITY.md` 策略以及更清晰的仓库和查找历史记录。您可以在线性或 GitHub 问题中选择跟踪结果，并在批准之前由 Codex 审核建议的操作。

使用现有的 [Codex Security工作台](security/plugin/workbench.zh-CN.md) 在桌面应用程序中查看保存的扫描、结果、仓库历史记录和修复。托管插件目录提供版本 `0.1.15`，而公共 CLI 插件市场提供版本 `0.1.11`。在依赖新功能之前检查 [Codex Security 插件变更日志](security/plugin/changelog.zh-CN.md)。

<a id="run-security-scans-from-the-terminal-ci-or-typescript"></a>

### 从终端、CI 或 TypeScript 运行安全扫描

公共 `@openai/codex-security` CLI 和 TypeScript SDK 达到版本 `0.1.5`，其发行号与 Codex Security 插件分开。使用 [从 CLI 运行扫描](security/cli.zh-CN.md) 包，查看拉取请求更改并在 [在 CI 中运行安全扫描](security/cli/ci.zh-CN.md) 中上传 SARIF 结果，或跨 GitHub 仓库或固定的 CSV 库存运行可恢复的 [批量扫描](security/cli/bulk-scans.zh-CN.md)。

[Security TypeScript SDK](security/sdk.zh-CN.md) 还允许您将扫描、进度报告、成本控制和取消构建到您自己的工具中。该软件包是公共的，但运行扫描仍然需要 Codex Security 访问权限。某些全仓库扫描还需要网络可信访问。



> 您现在可以使用它来扫描仓库、跟踪运行结果、验证修复并向 CI/CD 添加安全检查。

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2082263717916586117) (2026-07-29)

<a id="organize-sessions-and-extend-codex-cli-01460"></a>

### 组织会议并扩展 Codex CLI 0.146.0

[Codex CLI 0.146.0](https://github.com/openai/codex/releases/tag/rust-v0.146.0) 可让您使用 `/new release prep` 或 `/clear bug bash` 命名新聊天、固定重要线程以及在侧对话之间切换而不关闭它们。它还添加了临时对话分支、兼容自定义模型提供程序的独立网络搜索、执行者提供的技能以及对智能体插件清单、工作区插件发布和其他插件市场的支持。

对于自定义客户端，[应用服务器](app-server.zh-CN.md) 可以过滤固定线程、创建内存中分叉、检查已安装的连接器状态以及读取连接器元数据。实验性 WebSocket 支持还将应用程序服务器连接到远程代码模式主机。在公开远程连接之前查看 [应用服务器安全要求](app-server.zh-CN.md#connect-the-cli-terminal-ui)。该版本还改进了代理支持、MCP 重新连接、终端响应能力和 Windows 沙箱可靠性。

<a id="use-gpt-56-sol-for-hosted-codex-work"></a>

### 使用 GPT-5.6 Sol 进行托管 Codex 工作

[GPT-5.6溶胶](models.zh-CN.md#recommended-models) 现在为符合条件的客户提供 Codex 云代码审查和质量保证。 Sol 是用于复杂编码、研究、计算机使用和安全工作的旗舰 GPT-5.6 模型。 Codex云自动选择模型； Terra 和 Luna 在受支持的本地和 Web 使用界面上仍然可用。

<a id="prepare-for-the-gpt-54-model-retirement"></a>

### 为 GPT-5.4 模型退役做好准备

8 月 31 日，对于使用 ChatGPT 登录的用户，GPT-5.4 和 GPT-5.4 mini 将从 Codex 中退役。在工作区默认值、保存的模型设置、托管配置、自定义智能体和计划任务中，将 `gpt-5.4` 替换为 `gpt-5.6-terra`，将 `gpt-5.4-mini` 替换为 `gpt-5.6-luna`。

使用 API 密钥进行身份验证的 OpenAI API 和 Codex 会话不受影响。在截止前查看 [已弃用的 Codex 模型](models.zh-CN.md#deprecated-codex-models) 和 [工作区模型可用性](enterprise/workspace-model-availability.zh-CN.md)。

<a id="july-2024-2026"></a>

## 2026 年 7 月 20 日至 24 日

<a id="talk-through-work-with-chatgpt-voice"></a>

### 使用 ChatGPT 语音讨论工作

[ChatGPT 语音](features/voice.zh-CN.md) 由 GPT-Live 提供支持，可让您在 ChatGPT 桌面应用程序中的聊天、工作和 Codex 中讨论工作和协调任务。以语音模式启动新的聊天或任务，然后要求 ChatGPT 启动、检查或引导其他线程中的工作。

在 macOS 上，当 **屏幕上下文** 打开时，说“看看这个”即可共享最前面窗口的 [应用截图](appshots.zh-CN.md)。

语音适用于桌面应用程序中的 Plus、Pro、Business、Edu 和 Enterprise 计划以及通过 [iOS 上的远程](remote-connections.zh-CN.md#set-up-mobile-access)。



[观看：使用 ChatGPT 语音进行构建](https://www.youtube.com/watch?v=E0ZMOschrTU)

<a id="work-across-multiple-folders-in-one-local-project"></a>

### 跨一个本地项目中的多个文件夹进行工作

ChatGPT 桌面应用程序中的本地项目现在可以包含多个相关文件夹。选择用于新聊天、Git 操作以及自动发现 `AGENTS.md`、技能和 `config.toml` 的主文件夹。辅助文件夹仍可用于文件搜索、阅读和编辑。

打开**编辑项目**至[添加文件夹并选择主文件夹](projects.zh-CN.md#use-local-projects-for-folders-and-codebases)。

[阅读 7 月 23 日的发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-07-23-app)。



> 本地项目现在可以包含多个文件夹中的相关代码、文档和参考文件。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2080390328880951299) (2026-07-23)

<a id="july-1317-2026"></a>

## 2026 年 7 月 13 日至 17 日

<a id="keep-work-conversations-and-projects-together-on-desktop"></a>

### 在桌面上将工作对话和项目放在一起

ChatGPT 桌面应用程序现在在 ChatGPT 视图中将聊天和工作对话保持在一起。 Cloud Work 对话在 Web、移动设备和桌面上同步；本地工作对话保留在您的计算机上。 ChatGPT 项目可在桌面应用程序中使用。 Codex 为开发人员工作流程保留其专用视图和单独的历史记录。

[在桌面上比较 ChatGPT Work 和 Codex](use-chatgpt.zh-CN.md#compare-chatgpt-work-and-codex-on-desktop) 选择适合您任务的视图。



**提示：**

```text
打开启动项目，查看其文件和最近的对话，并从最新的工作对话继续启动计划。
```

<a id="control-parallel-codex-work-with-codex-micro"></a>

### 控制并联 Codex 与 Codex Micro 配合使用

7 月 15 日，OpenAI 和 Work Louder 推出了 [Codex微型](features/codex-micro.zh-CN.md)，这是 ChatGPT 桌面应用程序中 Codex 的有限运行物理控制界面。其智能体密钥可显示最多六个聊天的状态并在它们之间切换。可定制的命令键、模拟摇杆和转盘可以触发常见的操作或技能，启动即按即说，并无需离开键盘即可调整推理工作。



[观看：Codex Micro 简介](https://www.youtube.com/watch?v=m8uUUUsMD3Y)

<a id="use-gpt-56-through-amazon-bedrock"></a>

### 通过 Amazon Bedrock 使用 GPT-5.6

GPT-5.6 Sol、Terra 和 Luna 通过 Amazon Bedrock 全面上市。本地 ChatGPT Work 和 Codex 使用界面可以将内置 [`amazon-bedrock` 提供商](amazon-bedrock.zh-CN.md) 与 Bedrock API 密钥或 AWS 开发工具包凭证链结合使用。这包括 ChatGPT 桌面应用程序中的 Work 和 Codex、Codex CLI、IDE 扩展和 Codex SDK。

<a id="inspect-codex-task-visualizations-on-ios"></a>

### 在 iOS 上检查 Codex 任务可视化

适用于 iOS 1.2026.188 的 ChatGPT 向 Codex 任务添加了内联可视化，并改进了通过对话创建和管理任务，包括指向新创建任务的可靠链接。阅读 [7 月 13 日 iOS 发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-07-13-mobile)。

<a id="july-610-2026"></a>

## 2026 年 7 月 6 日至 10 日

<a id="take-on-ambitious-work-with-chatgpt-work"></a>

<a id="take-on-ambitious-work-in-chatgpt"></a>

### 在 ChatGPT 开展雄心勃勃的工作

ChatGPT 中的 [ChatGPT Work 入门](get-started-with-work.zh-CN.md) 可以从您的文件和 [插件](plugins.zh-CN.md) 中收集上下文，跨工作流程采取操作，并创建可审阅的文档、演示文稿、电子表格、网站和其他已完成的工作。它由 [模型选择](models.zh-CN.md) 提供支持，可以将目标分解为多个步骤，并在您跟踪其进度、回答问题、改变方向和批准重要行动的同时工作数小时。

[计划任务](automations.zh-CN.md) 可以在您外出时、按计划运行一次、在事件发生时或在监视变化时让工作继续进行。



**提示：**

```text
根据随附的研究和活动模板创建发布简介。在构建最终文档之前，向我展示计划并标记缺失的信息，然后将批准的概要调整为三个市场的资产。
```



[观看：认识 ChatGPT Work](https://www.youtube.com/watch?v=yRc5HcGJ-Cs)

<a id="choose-the-right-gpt-56-model"></a>

### 选择合适的 GPT-5.6 模型

[GPT-5.6家族](models.zh-CN.md#recommended-models) 提供 ChatGPT Work、ChatGPT 桌面应用程序、Codex CLI 和 Codex IDE 扩展的三种推荐模型。 Sol 是复杂编码、计算机使用、研究和安全工作的旗舰。 Terra 平衡了日常工作的能力和成本，而 Luna 是最快、成本最低的选择。默认的 **电源** 设置使用中等推理的 Sol。



[观看：认识 GPT-5.6](https://www.youtube.com/watch?v=-MPGU2a67Ls)

<a id="use-codex-in-the-chatgpt-desktop-app"></a>

### 在 ChatGPT 桌面应用程序中使用 Codex

7 月 9 日，Codex 应用程序合并到适用于 macOS 和 Windows 的 [ChatGPT 桌面应用程序](app.zh-CN.md)。 Codex 与 ChatGPT 的聊天和工作一起保留了其专用的编码体验。 Codex 体验包括差异中的内联编辑、侧面板中的拉取请求审查、由 GPT-5.6 提供支持的更快的 [电脑使用](computer-use.zh-CN.md) 以及多仓库项目。

现有 Codex 应用程序用户可以照常更新。您可以将 Codex 设置为默认视图，使用 Codex 徽标作为应用程序图标，并从 ChatGPT 移动应用程序访问桌面 Codex 项目。更新后的桌面应用程序可在全球范围内的每个 ChatGPT 套餐中使用，包括免费套餐。



[观看：面向工程团队的 Codex](https://www.youtube.com/watch?v=Ga792ftrBu4)

<a id="june-1519-2026"></a>

## 2026 年 6 月 15 日至 19 日

<a id="turn-demonstrated-workflows-into-reusable-skills"></a>

### 将演示的工作流程转化为可重用的技能

[录制与回放](extend/record-and-replay.zh-CN.md) 可让您在 macOS 上展示 ChatGPT 或 Codex 工作流程，并将演示转变为可重复使用的技能。将其用于更容易展示而不是描述的重复任务，然后完善生成的技能并使用新输入重播它。初始可用性不包括欧洲经济区、英国和瑞士，并且需要使用计算机。



[手表：Codex 中的 Record & Replay](https://www.youtube.com/watch?v=ZK3JhU73W18)

<a id="continue-a-task-on-another-host"></a>

<a id="continue-a-chat-on-another-host"></a>

### 在另一台主机上继续聊天

[聊天交接](remote-connections.zh-CN.md#hand-off-a-chat-between-hosts) 在本地计算机和连接的远程主机之间移动聊天及其 Git 状态。 Codex 可以在目标上创建或重用工作树、转移聊天并从匹配项目继续。

同一桌面版本向计划的运行历史记录添加了批量操作，因此您可以将每次运行标记为已读或一起归档符合条件的运行。

<a id="browse-and-review-workspaces-from-ios"></a>

### 从 iOS 浏览和查看工作区

在 ChatGPT 移动应用程序中，**远程** 添加了工作区文件浏览器、用于新聊天的目录选择器、差异的展开和折叠控件以及 iOS 上的每个聊天或跨聊天 MCP 批准选择。

Computer Use、Chrome 扩展、Memories 和 Chronicle 也开始向欧洲经济区、英国和瑞士推出。在这些地区，记忆功能默认处于关闭状态，而 Chronicle 是 macOS 上 ChatGPT Pro 订阅者可选择加入的研究预览。

阅读 [6 月 15 日 iOS](https://learn.chatgpt.com/docs/changelog#codex-2026-06-15-mobile)、[6 月 16 日可用](https://learn.chatgpt.com/docs/changelog#codex-2026-06-16-app) 和 [6 月 18 日应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-06-18-app) 发行说明。

<a id="june-812-2026"></a>

## 2026 年 6 月 8 日至 12 日

<a id="debug-web-apps-with-browser-developer-mode"></a>

### 使用浏览器开发者模式调试 Web 应用程序

[开发者模式](https://learn.chatgpt.com/docs/browser?surface=app#app-developer-mode) 使 Codex 能够受控地访问 Chrome 和内置浏览器中的 Chrome DevTools 协议功能。 Codex 在分析或调试您的应用程序时可以检查网络流量、控制台输出、运行时错误和页面状态。在 **设置** > **浏览器** 中的 **开发者模式** 下，打开 **启用完全 CDP 访问**。 Codex 在网站上使用该访问权限之前需要获得明确批准。

浏览器使用速度也提高了一倍，因为 CDP 和 DOM 快照优化减少了浏览器往返次数。


  

> 图：Codex 启用开发者模式的浏览器设置






**提示：**

```text
使用@Browser 重现检出目录缓慢的情况。检查网络时序和控制台错误，修复原因并验证结果。
```



[观看：在 Codex 中使用浏览器调试 Web 应用程序](https://www.youtube.com/watch?v=bhgYFRZLyKI)

<a id="bring-your-setup-to-codex"></a>

### 将您的设置带到 Codex

新的迁移流程可以在入职期间从其他编码智能体导入支持的设置。 Codex 应用程序还添加了用于创建项目说明的 `/init`，以及改进的插件管理、浏览器诊断和完整聊天摘要。

<a id="set-up-codex-tasks-from-ios"></a>

<a id="set-up-codex-chats-from-ios"></a>

### 从 iOS 设置 Codex 聊天

iOS 上的远程现在可以选择分支、创建工作树、运行环境设置脚本、管理目标以及添加内联审阅注释。

阅读 [6 月 9 日应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-06-09-app)、[6 月 9 日 iOS](https://learn.chatgpt.com/docs/changelog#codex-2026-06-09-mobile) 和 [6 月 11 日应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-06-11-app) 发行说明。

<a id="june-15-2026"></a>

## 2026 年 6 月 1 日至 5 日

<a id="build-and-deploy-websites-with-sites"></a>

### 使用站点构建和部署网站

[站点](sites.zh-CN.md) 允许 ChatGPT 创建、保存、部署和检查由 OpenAI 托管的网站、仪表板、内部工具、Web 应用程序和游戏。 Sites 在 Web 和桌面上的 ChatGPT 中有一个专用入口点，您可以在其中返回项目并管理托管环境值和机密，而无需组装单独的部署堆栈。



**提示：**

```text
使用站点从此项目构建响应式启动仪表板。在移动设备和桌面尺寸下进行验证，然后保存版本以供审核。在我批准保存的版本之前，请勿部署它。
```



[观看：Codex 中的站点介绍](https://www.youtube.com/watch?v=VRvC5smyzso)

<a id="use-codex-with-amazon-bedrock"></a>

### 将 Codex 与 Amazon Bedrock 结合使用

您可以通过 AWS 托管的身份验证、账户控制和计费来实现本地工作流程 [将 Codex 与 Amazon Bedrock 结合使用](amazon-bedrock.zh-CN.md)。 iOS 上的 Remote 还添加了可选的应用内锁定、后续行为设置、差异换行以及与 Windows 计算机的 SSH 连接。桌面应用程序在配置文件视图中添加了终端放置控件和活动洞察。

[阅读所有 2026 年 6 月发行说明](https://learn.chatgpt.com/docs/changelog#month-2026-06)。



> OpenAI 模型和 Codex 现已位于您的 AWS 工作流程中。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2061564710173224985) (2026-06-01)

<a id="may-2529-2026"></a>

## 2026 年 5 月 25 日至 29 日

<a id="use-windows-apps-and-control-codex-remotely"></a>

### 使用Windows应用程序并远程控制Codex

[电脑使用](computer-use.zh-CN.md#windows-foreground-use) 添加了对在 Windows 桌面应用程序中查看、单击和键入的支持。开始之前安装计算机使用插件。在 Windows 上，Codex 使用活动桌面并在任务运行时接管前台。远程连接也支持Windows。在 ChatGPT 移动应用程序中，打开 **远程** 以在 Windows 设备上开始工作，或使用运行 ChatGPT 桌面应用程序的 Mac 并从其他地方检查进度。



**提示：**

```text
使用 @Computer 打开 Windows 应用程序，重现导出失败，保存诊断文件，并总结触发问题的确切步骤。
```

iOS 上的 Remote 还添加了 Spotlight 和 Shortcuts 入口点、存档聊天浏览、`/side` 以及保存或复制渲染图像的选项。桌面应用程序添加了本地项目和工作树的聊天协调、过去聊天的内容和分支名称搜索以及后台子智能体的一致视觉标识符。

阅读 [5 月 25 日 iOS](https://learn.chatgpt.com/docs/changelog#codex-2026-05-25-mobile) 和 [5月29日应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-05-28-app) 发行说明。



[观看：Codex 的 Windows 计算机使用和移动访问](https://www.youtube.com/watch?v=MPIAB-8VmCo)

<a id="may-1822-2026"></a>

## 2026 年 5 月 18 日至 22 日

<a id="give-codex-context-from-any-mac-app-with-appshots"></a>

### 使用 Appshots 从任何 Mac 应用程序提供 Codex 上下文

当您同时按下两个 Command 键时，[应用截图](appshots.zh-CN.md) 会将最前面的应用程序窗口以及屏幕截图和可用文本发送到 Codex。 Codex 从设计工具、仪表板、文档和其他应用程序获取工作上下文，无需您复制、粘贴或描述屏幕上的内容。



**提示：**

```text
使用此应用程序快照作为视觉参考。匹配应用程序中选定的屏幕，然后打开预览并比较间距、版式和颜色。
```



[观看：Codex 中的应用程序快照简介](https://www.youtube.com/watch?v=QKYbGCvNpFo)

<a id="follow-long-running-goals"></a>

### 遵循长期目标

[目标模式](prompting.zh-CN.md#goal-mode) 处于实验状态，可在 Codex 应用程序、IDE 扩展和 CLI 中使用，以实现可能需要数小时或数天的目标。 [锁定使用](computer-use.zh-CN.md#locked-use) 允许 Codex 在 Mac 锁定后继续批准的计算机使用工作，包括通过 ChatGPT 移动应用程序中的 **远程**。 ChatGPT 业务工作区也可以 [与工作区成员共享可重用的插件包](https://developers.openai.com/plugins/build/plugins#share-a-local-plugin-with-your-workspace)。

[阅读 5 月 21 日的发布说明](https://learn.chatgpt.com/docs/changelog#codex-2026-05-21)。



[观看：使用目标在 Codex 中运行长任务](https://www.youtube.com/watch?v=rgh0hMYPcd0)

<a id="may-1115-2026"></a>

## 2026 年 5 月 11 日至 15 日

<a id="continue-desktop-work-from-mobile"></a>

### 从移动设备上继续桌面工作

在 ChatGPT 移动应用程序中，**远程** 连接到运行 ChatGPT 桌面应用程序的 Mac。由于工作在连接的主机上运行，​​因此当您从手机继续操作时，您的项目、文件、凭据、插件、技能和配置仍然可用。请参阅 [远程连接](remote-connections.zh-CN.md) 设置主机并从另一台设备接取工作。



> 现已预览：ChatGPT 移动应用程序中的 Codex。

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2055016850849993072) (2026-05-14)

<a id="automate-trusted-workflows"></a>

### 自动化可信工作流程

Hooks 已普遍可用，可在智能体生命周期的关键点运行自定义命令。 ChatGPT 企业管理员还可以为受信任的脚本、调度程序和私有 CI 运行程序启用 [Codex 访问令牌](enterprise/access-tokens.zh-CN.md)。企业指南扩展到涵盖 Codex 的托管设置和控制。

[阅读 5 月 14 日的发布说明](https://learn.chatgpt.com/docs/changelog#codex-2026-05-13-app)。



> Codex 围绕您的代码进行自动化和定制变得越来越容易。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2055032115964870838) (2026-05-14)

<a id="may-48-2026"></a>

## 2026 年 5 月 4 日至 8 日

<a id="work-across-browser-tabs-with-the-chrome-extension"></a>

### 使用 Chrome 扩展程序跨浏览器选项卡工作

[Chrome 扩展程序](chrome-extension.zh-CN.md) 可以在后台跨选项卡并行工作，而无需接管您的浏览器。您可以控制 Codex 可以使用哪些网站，从而可以将跨 Web 应用程序的研究、数据输入和验证结合到一项任务中。



**提示：**

```text
比较打开的产品页面，收集表中的计划限制，引用每个源选项卡，并标记需要手动检查的任何差异。
```

Codex 应用程序还添加了听写清理功能以及用于名称、文件路径和代码符号的自定义字典。 ChatGPT 企业工作区所有者可以允许成员为受信任的非交互式本地工作流程创建 [Codex 访问令牌](enterprise/access-tokens.zh-CN.md)。

阅读 [5月5日应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-05-05-app)、[5 月 5 日访问令牌](https://learn.chatgpt.com/docs/changelog#codex-2026-05-05) 和 [Codex 适用于 Chrome](https://learn.chatgpt.com/docs/changelog#codex-2026-05-07) 发布说明。



[观看：Codex 现在可以在 macOS 和 Windows 上直接使用 Chrome](https://www.youtube.com/watch?v=b6Mxcv1pyBU)

<a id="april-2024-2026"></a>

## 2026 年 4 月 20 日至 24 日

<a id="use-gpt-55-for-complex-work"></a>

### 使用 GPT-5.5 进行复杂的工作

[模型选择](models.zh-CN.md)到达Codex作为大多数任务的推荐模型，在实施、调试、测试、计算机使用、研究和完成的知识工作输出方面具有优势。



[观看：GPT-5.5 简介](https://www.youtube.com/watch?v=blGtYq9mL18)

<a id="let-codex-operate-the-browser-and-review-approvals"></a>

### 让Codex操作浏览器并审核批准

[电脑使用内置浏览器](https://learn.chatgpt.com/docs/browser?surface=app#app-computer-use-in-the-browser) 允许 Codex 单击本地开发服务器和文件支持的页面来重现问题并验证修复。符合条件的审批请求还可以通过 [自动审批审核](sandboxing/auto-review.zh-CN.md)，它会在操作运行之前显示审核状态和风险。



**提示：**

```text
使用@Browser打开本地应用程序，重现结帐失败，修复它，并验证流程端到端。
```

[阅读 4 月 23 日的发布说明](https://learn.chatgpt.com/docs/changelog#codex-2026-04-23)。



> 借助 GPT-5.5，Codex 现在可以跨浏览器、文件、文档和计算机完成更多工作。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2047381283358355706) (2026-04-23)

<a id="april-1317-2026"></a>

## 2026 年 4 月 13 日至 17 日

<a id="preview-and-operate-work-in-one-place"></a>

### 在一处预览和操作工作

[内置浏览器](browser.zh-CN.md) 添加了实时预览和页面评论，而 [电脑使用](computer-use.zh-CN.md) 让 Codex 查看和操作 macOS 应用程序。他们共同将可视化实现和端到端验证作为代码更改的同一任务的一部分。


  

> 插图：ChatGPT 桌面应用程序，并在内置浏览器中打开本地网页






[观看：Codex（几乎）所有内容](https://www.youtube.com/watch?v=Lm7-yFZ5fZQ)

<a id="start-with-a-task-and-keep-it-moving"></a>

<a id="start-with-a-chat-and-keep-it-moving"></a>

### 从聊天开始并持续进行

[独立聊天](projects.zh-CN.md#start-without-a-project) 无需选择项目文件夹即可开始。同一版本添加了 [聊天中的计划任务](automations.zh-CN.md#schedule-a-task-inside-a-chat)、拉取请求上下文、更丰富的文件预览以及用于跨聊天工作的 [回忆](customization/memories.zh-CN.md)。

[阅读 4 月 16 日 Codex 应用程序发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-04-16-app)。



> 自动化现在可以在同一线程中运行，因此 Codex 可以从中断处继续

[在 X 上查看@OpenAI](https://x.com/OpenAI/status/2044828148890812538) (2026-04-16)

<a id="april-610-2026"></a>

## 2026 年 4 月 6 日至 10 日

<a id="review-and-ship-pull-requests-in-the-app"></a>

### 在应用程序中审核并发送拉取请求

审阅体验添加了可折叠的内联注释、内联和分离审阅模式以及更清晰的 Git 和源上下文。然后，拉取请求活动、评论和推送选项与工作区文件选项卡一起移至应用程序中，因此您可以检查更改并做出响应，而无需切换工具。

阅读 [4 月 9 日](https://learn.chatgpt.com/docs/changelog#codex-2026-04-09-app) 和 [4月10日](https://learn.chatgpt.com/docs/changelog#codex-2026-04-10-app) Codex 应用程序发行说明，或了解如何 [查看应用程序中的更改](code-review.zh-CN.md)。

<a id="march-2327-2026"></a>

## 2026 年 3 月 23 日至 27 日

<a id="package-workflows-as-plugins"></a>

### 将工作流程打包为插件

[插件](plugins.zh-CN.md) 作为可安装的技能、连接器和 MCP 服务器捆绑包推出。它们使完整的工作流程更容易发现、安装和共享，而重新设计的插件和技能页面使其内容和状态更加清晰。那周也出现了对过去聊天记录的搜索。

阅读 [任务搜索](https://learn.chatgpt.com/docs/changelog#codex-2026-03-24-app)、[插件启动](https://learn.chatgpt.com/docs/changelog#codex-2026-03-25) 和 [Codex 应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-03-25-app) 发行说明。



> Codex 中有插件吗？我们找到你了。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2037604273434018259) (2026-03-27)

<a id="march-1620-2026"></a>

## 2026 年 3 月 16 日至 20 日

<a id="branch-earlier-and-choose-tools-from-the-composer"></a>

### 尽早分支并从 Composer 中选择工具

您可以从较早的消息中分叉聊天，从而更轻松地尝试新方法，而不会丢失原始路径。起草时可以使用模型和推理命令，启用的技能出现在 `@` 菜单中，GPT-5.4 mini 为较轻的任务和子智能体添加了更快的选项。

阅读 [GPT-5.4迷你](https://learn.chatgpt.com/docs/changelog#codex-2026-03-17)、[聊天控制](https://learn.chatgpt.com/docs/changelog#codex-2026-03-18-app) 和 [技能菜单](https://learn.chatgpt.com/docs/changelog#codex-2026-03-19-app) 发行说明。



> GPT-5.4 mini 比 GPT-5 mini 快 2 倍以上。针对编码、计算机使用、多模式理解和子智能体进行了优化。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2033953815834333608) (2026-03-17)

<a id="march-913-2026"></a>

## 2026 年 3 月 9 日至 13 日

<a id="schedule-work-with-the-right-environment"></a>

### 在合适的环境下安排工作

[计划任务](automations.zh-CN.md) 可以在本地运行，也可以在具有显式模型和推理级别的工作树中运行。可重复使用的模板使常见任务的配置速度更快，自定义主题使工作区更易于个性化。


  

> 图：ChatGPT 桌面应用程序中的计划任务设置






> 自动化现已全面上市。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2032222711032971548) (2026-03-12)

<a id="let-codex-inspect-terminal-output"></a>

### 让Codex检查终端输出

Codex 还学会了读取当前聊天的 [综合终端](integrated-terminal.zh-CN.md#run-and-validate-your-project)。它可以检查正在运行的开发服务器或直接构建输出，而不是要求您粘贴它。



**提示：**

```text
每个工作日，检查过去 24 小时的变化，找到一个可能的回归，在工作树中修复它，运行最小的相关测试，并报告证据。
```

阅读 [3月11日](https://learn.chatgpt.com/docs/changelog#codex-2026-03-11-app) 和 [3月12日](https://learn.chatgpt.com/docs/changelog#codex-2026-03-12-app) Codex 应用程序发行说明。

<a id="march-26-2026"></a>

## 2026 年 3 月 2 日至 6 日

<a id="run-codex-natively-on-windows"></a>

### 在 Windows 上本机运行 Codex

Codex 应用程序在 [窗户](windows/windows-app.zh-CN.md) 上启动，具有本机 PowerShell 和沙箱支持，以及工作树、计划任务和技能。 WSL 仍然可供喜欢 Linux 环境的开发人员使用。


  

> 插图：在 Windows 上本机运行的 Codex 应用程序






[观看：Codex 应用程序现已在 Windows 上运行](https://www.youtube.com/watch?v=8hNcRChDrNk)

<a id="move-tasks-between-local-and-worktree"></a>

<a id="move-chats-between-local-and-worktree"></a>

### 在本地和工作树之间移动聊天

[本地和工作树切换](environments/git-worktrees.zh-CN.md#working-between-local-and-worktree) 可以移动活动聊天，同时保留其上下文。 GPT-5.4 也于本周抵达 Codex，用于编码、计算机使用和较长上下文的工作流程。

阅读 [Windows 启动](https://learn.chatgpt.com/docs/changelog#codex-2026-03-04-app)、[工作树切换](https://learn.chatgpt.com/docs/changelog#codex-2026-03-03-app) 和 [GPT-5.4](https://learn.chatgpt.com/docs/changelog#codex-2026-03-05) 发行说明。

<a id="february-913-2026"></a>

## 2026 年 2 月 9 日至 13 日

<a id="iterate-in-real-time-and-branch-an-approach"></a>

### 实时迭代并分支方法

GPT-5.3-Codex-Spark 作为实时编码迭代的近乎即时模型进入研究预览阶段。该应用程序还添加了聊天分叉和浮动的、始终位于顶部的聊天窗口，因此您可以探索另一种方法或将 Codex 保留在编辑器或浏览器旁边。

阅读 [火花](https://learn.chatgpt.com/docs/changelog#codex-2026-02-12) 和 [Codex 应用程序](https://learn.chatgpt.com/docs/changelog#codex-2026-02-12-app) 发行说明，或查看当前的 [模型指南](models.zh-CN.md)。



> 隆重推出 GPT-5.3-Codex-Spark，这是我们专为实时编码而构建的超快速模型。

[在 X 上查看 @OpenAIDevs](https://x.com/OpenAIDevs/status/2022009906329739681) (2026-02-12)

<a id="february-26-2026"></a>

## 2026 年 2 月 2 日至 6 日

<a id="the-codex-app-launches-on-macos"></a>

### Codex 应用程序在 macOS 上发布

Codex 应用程序作为桌面工作区启动，用于并行项目聊天、内置 Git 审核、工作树、技能、计划任务和语音听写。这些功能现在位于 [ChatGPT 桌面应用程序](app.zh-CN.md) 的 Codex 中。


  

> 插图：原始 Codex 应用程序在 macOS 上显示并行项目聊天






[观看：Codex 应用程序简介](https://www.youtube.com/watch?v=HFM3se4lNiw)

<a id="steer-active-work-and-add-files"></a>

### 引导主动工作并添加文件

中转转向可以在不停止主动响应的情况下重定向 Codex，并且文件附件扩展到图像之外。这些模式成为 [引导和排队](prompting.zh-CN.md#steering-and-queuing) 后续内容和 Codex 需求的基础。

读取 [Codex 应用程序启动说明](https://learn.chatgpt.com/docs/changelog#codex-2026-02-02) 和 [2 月 5 日应用程序发行说明](https://learn.chatgpt.com/docs/changelog#codex-2026-02-05-app)。