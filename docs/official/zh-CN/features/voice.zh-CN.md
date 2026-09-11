> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/features/voice.md)。

<a id="chatgpt-voice"></a>

# ChatGPT 语音

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT Voice 由 GPT-Live 提供支持，可让您在 ChatGPT 桌面应用程序中的聊天、工作和 Codex 中讨论想法并协调任务。无需切换回打字即可开始工作、检查进度或改变方向。

ChatGPT 语音可在 ChatGPT 桌面应用程序中使用 ChatGPT Plus、Pro、Business、Edu 和 Enterprise 计划。 Enterprise 和 Edu 的可用性从两周的早期访问期开始，然后该功能默认可用。手机与桌面主机配对后，您还可以通过[iOS 上的远程](../remote-connections.zh-CN.md#set-up-mobile-access)使用ChatGPT语音。可用性还取决于推出状态和工作区设置。参见 [功能可用性](../pricing.zh-CN.md#feature-availability)。



> 插图：带有麦克风和扬声器控制的交互式 ChatGPT 语音对话。



<a id="start-talking"></a>

## 开始说话

1. 在 ChatGPT 桌面应用程序中打开您要讨论的 Codex 任务，或开始新的聊天或任务。
2. 在现有任务中选择 **开始语音聊天**，或在新聊天或任务中选择 **开始新的语音聊天**。
3. 首次开始语音聊天时，请允许访问麦克风、选择语音并查看 macOS 上的屏幕上下文。
4. 开始说话。完成后选择 **停止语音聊天**。

现有 Codex 任务中的语音正在推出。如果可用，您可以开始在以键入消息开始的任务中交谈。语音使用该任务的对话和选定的模型来执行您的请求，因此您可以讨论其进度或改变方向，而无需开始另一项任务。

如果 **开始语音聊天** 在现有任务中不可用，请更新桌面应用程序和运行该任务的 Codex 主机。可用性还取决于您的帐户和工作区。您仍然可以在支持的情况下开始新的语音聊天，或使用 [语音听写](../prompting.zh-CN.md#use-voice-dictation) 输入提示文本。要恢复之前的语音聊天，请打开它并选择 **开始语音聊天**。

您可以在**设置 > 语音 > 语音聊天热键**中设置快捷方式。

<a id="have-a-conversation"></a>

## 进行对话

ChatGPT语音支持自然轮流。您可以在回复期间打断 ChatGPT、询问后续情况或改变方向。如果 ChatGPT 开始工作，请继续讲话以检查进度或引导任务。

<a id="delegate-and-coordinate-work"></a>

## 委派和协调工作

ChatGPT 语音可以为较长的工作启动单独的任务、检查现有任务并发送后续指令。它将进度、障碍和结果带回到您的语音对话中，以便您可以在继续工作的同时继续交谈。

例如：

- “查看今天的发布简报并总结需要批准的决定。”
- “启动 Codex 任务来运行测试并调查任何未通过的内容。”
- “检查活动任务并总结任何阻碍进度的事情。”

您还可以要求与另一个 Codex 任务对话，然后要求返回到上一个任务。例如，说“让我谈谈审查测试的任务”，然后“带我回到上一个任务”。目标任务必须支持语音并且可在连接的主机上使用。

ChatGPT语音跟随相同[权限](../permission-modes.zh-CN.md)作为它在聊天、工作和中指导的任务Codex在ChatGPT桌面应用程序。

<a id="show-chatgpt-what-you-see"></a>

## 显示 ChatGPT 您所看到的

在 macOS 上，打开 **设置 > 语音** 中的 **屏幕上下文**，然后说“看看这个”。 ChatGPT 可以采用最前面窗口的 [应用截图](../appshots.zh-CN.md#permissions-and-safety) 并将其用作上下文。您的组织可以禁用此功能。

应用程序快照可以包含窗口的图像和可访问的文本，包括可见滚动区域之外的内容。 macOS 可能会请求 **屏幕和系统音频录制** 和 **无障碍** 权限。避免共享包含敏感信息的窗口，包括可见滚动区域之外的文本。

<a id="chatgpt-voice-and-voice-dictation"></a>

## ChatGPT 语音及语音听写

使用 ChatGPT 语音与 ChatGPT 进行实时对话。当您只想在发送之前将语音转换为提示文本时，请使用 [语音听写](../prompting.zh-CN.md#use-voice-dictation)。

<a id="limits-and-troubleshooting"></a>

## 限制和故障排除

ChatGPT 桌面应用程序一次只能激活一个语音聊天。语音对话使用单独的、取决于计划的津贴，以滚动的五小时窗口为单位进行衡量。通过语音启动的任务将继续使用您的 Codex 使用预算。当您达到任一限制时，ChatGPT 会通知您。参见 [语音定价和限制](../pricing.zh-CN.md#chatgpt-voice-in-desktop)。

如果您无法开始语音聊天，请确认 ChatGPT 语音可用于您的计划、部署和工作区。然后检查麦克风权限以及语音聊天是否已在另一个应用程序窗口中处于活动状态。如果屏幕上下文不可用，请检查 **设置 > 语音**、Appshots 权限和您的组织的限制。