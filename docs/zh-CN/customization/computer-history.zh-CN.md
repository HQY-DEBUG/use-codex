> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/customization/computer-history.md)。

<a id="computer-history"></a>

# 电脑历史记录

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Computer History 是 **默认关闭**，适用于 macOS 上 ChatGPT 桌面应用程序中的 ChatGPT Pro、Business 和 Enterprise 用户。专业用户可以选择打开它。对于商业和企业工作区，管理员必须明确授予访问权限，然后每个成员才能选择打开它。 Computer History 还需要 [回忆](memories.zh-CN.md)，并且不能通过 API 密钥或 Amazon Bedrock 获得。它在受支持的区域提供，包括欧洲经济区 (EEA)、瑞士和英国。

Computer History 将您跨应用程序和网站的活动转化为 ChatGPT 和 Codex 可以参考的记忆和时间线。您可以询问有关最近工作的自然问题，从上次停下的地方继续，了解工作模式，并将重复的工作流程转化为技能或自动化。

您的历史记录仅在您选择打开后才开始。您可以控制哪些应用程序和网站进行贡献，可以从 macOS 菜单栏查看和暂停收集，并且可以随时检查或删除您的历史记录。

Computer History 取代了早期的 Chronicle 研究预览，但它是重建的系统而不是重命名。它使用交互事件以及通过 macOS 辅助功能提供的文本和其他上下文来创建您可以查看和删除的摘要。它不包含您的历史记录中的屏幕截图或录制音频，并且绝不会包含私人模式的 Web 浏览活动。



> 插图：Computer History 时间线显示活动摘要、贡献应用程序以及建议的技能和自动化



<a id="how-computer-history-helps"></a>

## Computer History 如何提供帮助

Computer History 提供最近的活动作为上下文。当文件、Slack 对话、Google Doc 或其他来源更适合执行任务时，ChatGPT 和 Codex 可以使用历史记录来识别该来源，然后直接读取它。

<section class="feature-grid mt-4">




<a id="pick-up-where-you-left-off"></a>

### 从上次停下的地方继续

询问休息前您在做什么，而无需重新构建每个打开的应用程序、文档和下一步。




<ComputerHistoryThreadDemo client:load scenario="resume" />

</section>

<section class="feature-grid inverse">




<a id="find-recent-work"></a>

### 查找最近的工作

按照您记忆中的方式引用文档、对话或任务。 Computer History 可以使用活动时间线来识别您所指的来源。




<ComputerHistoryThreadDemo client:load scenario="find" />

</section>

<section class="feature-grid">




<a id="reuse-workflows"></a>

### 重用工作流程

当 Computer History 注意到可重复的工作时，时间线条目可以建议技能或自动化。查看建议，然后要求 Codex 根据记录的工作流程创建它。




<ComputerHistoryThreadDemo client:load scenario="workflow" />

</section>

<a id="how-computer-history-works"></a>

## Computer History 的工作原理

Computer History 从允许的应用程序和网站创建交互事件流。事件可以包括单击、键入、键盘快捷键、应用程序切换以及 macOS 通过其辅助功能系统公开的上下文。 Computer History定期将这些事件转化为文本摘要和本地内存文件。

Computer History 不包含历史记录中的屏幕截图或记录麦克风输入或系统音频。私人模式的网页浏览活动永远不包括在内。

在 **设置 > 计算机历史记录 > 历史记录** 中，时间线按日期和时间对摘要进行分组。每个项目可以显示：

- 活动的标题和文本摘要。
- 对摘要做出贡献的应用程序。
- 当 ChatGPT 识别可重复工作时建议的技能或自动化。
- 在 Finder 中显示内存文件或删除项目的操作。

选择 **询问您的历史** 开始与 Computer History 聊天，或使用如下提示：

- “上次休息之前我在做什么？”
- “我在哪里可以找到我今天早些时候寻找的提案文件？”
- “给我一份我今天完成的任务及其状态的列表。”
- “准备一份我昨天为站立会议所做的事情的总结。”

<a id="permissions-and-access"></a>

## 权限和访问

Computer History 使用单独的控件来控制工作区访问、个人选择加入、记忆以及历史记录中包含的应用程序或网站：

- 默认情况下，**工作区访问：** Computer History 在商业和企业工作区中处于关闭状态，并且在管理员明确授予访问权限之前不可用。企业管理员可以使用 [**工作区设置 > 权限和角色**](https://chatgpt.com/admin/settings) 中的 **启用Computer History** 向适当的工作区角色授予访问权限。
- **个人选择加入：** 授予工作区访问权限仅允许成员选择打开 Computer History。它不会为任何人打开该功能。每个人都必须单独选择加入，包括 ChatGPT Pro 用户。
- **回忆：** Computer History 还需要 [回忆](memories.zh-CN.md)。使用 `/memories` 控制单个聊天是否可以使用本地记忆或贡献未来记忆。
- **应用程序和网站：** 您的应用程序和网站权限决定哪些源可以贡献交互事件。您可以仅允许特定来源或排除您不希望包含的应用程序和网站 URL。

如果您的工作区角色没有访问权限，则更改本地设置无法启用 Computer History。

<a id="turn-on-computer-history"></a>

## 开启Computer History

Computer History 默认关闭。如果您使用商业或企业工作区，请在打开之前请求管理员授予您访问权限。管理员批准不会选择您加入。

1. 在 macOS 上打开 ChatGPT 桌面应用程序。
2. 在“设置”中的“**集成**”下，选择“**计算机历史**”。
3. 选择 **打开** 并查看隐私、权限和本地存储信息。
4. 如果出现提示，请打开 **回忆**。 Computer History 需要内存，以便它可以跨聊天和任务使用活动上下文。
5. 选择哪些应用程序和网站可以为您的历史记录做出贡献，然后按照任何 macOS 权限提示进行操作。

Computer History 不需要屏幕录制权限。如果该设置未出现，请确认您的计划支持 Computer History 并且您的工作区管理员已启用它（如果适用）。

<a id="control-what-is-included"></a>

## 控制包含的内容

您可以控制哪些应用程序和网站对未来历史做出贡献，以及 Computer History 是否主动收集交互事件。

<a id="choose-apps-and-websites"></a>

### 选择应用程序和网站

在 **设置 > 计算机历史记录 > 权限** 下，选择 Computer History 可以包括哪些应用程序和网站：

- **排除这些应用程序** 和 **排除这些网站** 会阻止您指定的应用程序或 URL，同时允许其他受支持的来源。
- **仅包含这些应用程序** 和 **仅包含这些网站** 仅允许您明确选择的源。

您还可以在历史时间线项目中选择应用程序图标，以将该应用程序从未来的历史记录中排除。您可以稍后再次包含它。

私人模式的网页浏览活动永远不包括在内。更改应用程序或网站权限会影响未来的历史记录。要删除现有项目，请删除或清除它们。

<a id="pause-resume-or-stop-collection"></a>

### 暂停、恢复或停止收集

使用 Computer History 设置或 macOS 菜单栏来控制该功能何时收集活动：

- 选择 macOS 菜单栏中的 ChatGPT 图标，然后展开 Computer History 菜单以查看它捕获的活动并访问其控件。
- 选择 **暂停** 以停止收集新的交互事件，或在准备好重新开始时选择 **简历**。
- 关闭 Computer History 以停止将来的活动收集。

Computer History 可以包含来自通信应用程序和网站的交互事件。在与其他人交流时将其关闭，除非您事先得到他们的明确同意。考虑暂停或排除包含敏感健康、财务或个人信息的应用程序。

<a id="review-and-clear-history"></a>

## 回顾并清除历史记录

打开**设置 > 计算机历史记录 > 历史记录**查看Computer History总结的内容。您可以在 Finder 中显示摘要的本地内存文件，删除单个时间线项目，或清除过去 10 分钟、过去一小时、最后一天或所有历史记录。 macOS 菜单栏还可以让您清除最近使用的应用程序的最后一个会话。

清除历史记录会删除相关的交互事件以及由此创建的任何记忆。此操作无法撤消。

<a id="privacy-and-local-storage"></a>

## 隐私和本地存储

Computer History 将交互事件流临时存储在 Mac 上，以便 ChatGPT 和 Codex 可以生成内存并构建建议的工作流程。该流可以包括单击和打字等活动，以及通过 macOS 辅助功能提供的文本和其他上下文。 Computer History 不包含历史记录中的屏幕截图或记录麦克风输入或系统音频。私人模式的网页浏览活动永远不包括在内。

临时事件文件最多保留 48 小时。生成的内存文件将保留在您的文件系统上，直到您删除或清除它们，并且您可以从历史时间轴中显示这些文件。

<a id="where-does-computer-history-store-my-data"></a>

### Computer History 在哪里存储我的数据？

Computer History 将交互事件临时保存在您的 Mac 上。事件文件在 ChatGPT [应用组](https://developer.apple.com/documentation/xcode/protecting-local-app-data-using-containers) 内隔离，这可以防止其他应用程序在未经明确许可的情况下访问它们。 ChatGPT 和 Codex 会在 48 小时后删除这些事件文件。

Computer History 生成与 Codex 相同类型的本地内存：您可以读取和修改的纯文本 Markdown 文件。这些文件存储在 `$CODEX_HOME/memories/extensions/skysight/` 下，通常解析为 `~/.codex/memories/extensions/skysight/`。



  <Alert
    client:load
    color="danger"
    variant="soft"
    description="Computer History 文件可能包含敏感信息。它们未经过 Computer History 加密，以 macOS 用户身份运行的其他程序可能能够访问它们。保护您的 Mac 帐户并排除您不希望包含的来源。"
  />



<a id="what-data-gets-shared-with-openai"></a>

### 与 OpenAI 共享哪些数据？

Computer History 在本地捕获交互事件，然后定期启动可访问交互事件流的临时 Codex 会话，以将您的活动总结到内存中。

OpenAI 在其服务器上处理临时事件文件以生成内存，然后将其存储在本地 Mac 上。除非法律要求，OpenAI 不会在处理后保留这些事件文件，也不将其用于培训。

当ChatGPT或Codex在将来的聊天中使用存储器时，相关的存储器内容和交互事件可以被包括作为上下文。如果您的 [ChatGPT数据控件](https://help.openai.com/en/articles/7730893-data-controls-faq) 允许，此聊天内容可用于改进 OpenAI 模型。记忆也遵循同样的[与其他 Codex 存储器一样的聊天级控制](memories.zh-CN.md#control-memories-per-chat)。

<a id="prompt-injection-risk"></a>

### 提示注射风险

Computer History 增加了应用程序和网站内容提示注入的风险。例如，如果您访问包含恶意指令的网站，ChatGPT 或 Codex 可能会遵循这些指令。

<a id="token-usage"></a>

## Token使用

Computer History 在总结活动并创建记忆的同时使用Token。

<a id="troubleshooting"></a>

## 故障排除

如果 Computer History 可用但未启动：

1. 确认 **回忆** 已打开。
2. 打开 **设置 > 计算机历史记录** 并根据显示的状态选择 **完成设置**、**简历** 或 **再试一次**。
3. 如果设置仍然不可用，请退出并重新打开 ChatGPT 桌面应用程序。