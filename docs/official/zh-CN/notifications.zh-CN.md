> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/notifications.md)。

<a id="notifications"></a>

# 通知

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

通知让您知道工作何时需要注意。它们的控制和输送渠道因使用界面而异。

<ContentModeSwitch group="codex-surface" id="app">

<a id="configure-desktop-notifications"></a>

## 配置桌面通知

打开 [**设置**](codex://settings) 以选择是从不出现回合完成警报、仅当 ChatGPT 在后台时出现还是始终出现。单独的控件可让您打开或关闭权限和问题通知。您的操作系统可能会要求您向 ChatGPT 桌面应用程序授予通知权限。

<a id="follow-chats-in-activity-view"></a>

### 在活动视图中关注聊天

当 **活动** 可用时，选择侧边栏中的铃铛即可查看未读、正在运行或等待您回复的聊天。您还可以使用以下命令打开或关闭活动视图<kbd>Cmd</kbd>+<kbd>Option</kbd>+<kbd>U</kbd>在 macOS 或<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>U</kbd>在 Windows 上。

使用视图的选项来选择显示哪些聊天。根据您当前的使用界面，选项可能包括 **工作**、**聊天**、**已固定** 和 **预定**。您还可以选择“**全部标记为已读**”来清除未读项目。

<a id="follow-task-activity-with-a-pet"></a>

<a id="follow-chat-activity-with-a-pet"></a>

### 关注与宠物的聊天活动

在 ChatGPT 桌面应用程序中，浮动宠物是您在其他应用程序中工作时跟踪聊天活动的另一种方式。当聊天为 **跑步**、**需要输入**、**准备好** 或 **被阻止** 时，它可以显示。

请参阅 [宠物](pets.zh-CN.md) 选择宠物、了解其状态或创建自己的宠物。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="configure-web-notifications"></a>

## 配置网络通知

打开 **设置 > 通知** 管理您帐户可用的通知类别和渠道。根据类别和帐户，渠道可以包括推送、电子邮件或短信。使用任务通知设置中的 **管理任务** 打开 **预定**。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="configure-cli-notifications"></a>

## 配置 CLI 通知

终端和外部通知请参见高级配置指南中的[通知](config-file/config-advanced.zh-CN.md#notifications)。您可以选择 TUI 何时发出通知以及 Codex 在回合完成时是否运行外部程序。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="follow-task-activity-in-the-ide"></a>

<a id="follow-chat-activity-in-the-ide"></a>

## 关注 IDE 中的聊天活动

IDE 扩展不提供单独的通知控件。保持聊天打开以跟踪其活动。要在一轮完成时运行外部程序，请在连接的 Codex 主机上配置 `notify`。请参阅高级配置指南中的 [通知](config-file/config-advanced.zh-CN.md#notifications)。

</ContentModeSwitch>

<a id="related-docs"></a>

## 相关文档

- [长时间运行的工作](long-running-work.zh-CN.md)
- [计划任务](automations.zh-CN.md)
- [宠物](pets.zh-CN.md)