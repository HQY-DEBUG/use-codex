> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/reference/settings.md)。

<a id="settings"></a>

# 桌面应用设置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用设置面板来个性化应用程序并管理日常偏好。从应用程序菜单中打开 [**设置**](codex://settings) 或按

<kbd>Cmd</kbd>+<kbd>,</kbd>在 macOS 或<kbd>Ctrl</kbd>+<kbd>,</kbd>在 Windows 上。

<a id="general"></a>

## 一般

要求<kbd>Cmd</kbd>+<kbd>Enter</kbd>对于多行提示，或打开 **防止跑步时睡觉**，以便在您离开时本地聊天可以继续。在 **后续行为** 下，选择 ChatGPT 工作时发送的消息是否应引导当前运行或等待下一次运行。

<a id="profile"></a>

## 公司简介

使用 **公司简介** 查看活动见解、生命周期令牌、峰值令牌、连续次数、最长任务和令牌活动。您还可以更新您的个人资料详细信息，例如您的照片、显示名称和用户名，并保存包含使用情况亮点的个人资料卡。消费者 ChatGPT 计划提供共享个人资料卡。

符合条件的用户还可以从个人资料菜单发送 Codex 邀请。在符合条件的个人计划中选择 **邀请朋友**，或在符合条件的商务工作区中选择 **邀请同事**。请参阅 [邀请朋友和同事](../pricing.zh-CN.md#invite-friends-and-coworkers) 了解当前奖励、限制和资格。

<a id="keyboard-shortcuts"></a>

## 键盘快捷键

打开 **键盘快捷键** 来查看命令、更改绑定或将自定义快捷方式重置为其默认值。使用搜索字段按命令名称查找快捷方式，或切换到击键搜索并按组合键查找使用它的命令。

<a id="notifications"></a>

## 通知

选择何时显示回合完成通知，以及应用程序是否应提示通知权限。

<a id="appearance"></a>

## 外观

在 **设置** 中，您可以通过选择基本主题、调整强调色、背景色和前景色以及更改 UI 和代码字体来更改应用程序外观。您还可以与朋友分享您的自定义主题。


  

> 插图：ChatGPT 桌面应用程序 外观设置显示主题选择、颜色控制和字体选项




<a id="pets"></a>

## 宠物



  

宠物是该应用程序的可选动画伙伴。在**设置 > 宠物**中，选择内置或自定义宠物，然后使用`/pet`、**唤醒宠物**或**收起宠物**来控制浮动叠加。

请参阅 [宠物](../pets.zh-CN.md) 了解宠物状态、跟踪聊天活动或创建您自己的宠物。

  


  <CodexPetsDemo client:load />



<a id="browser-use"></a>

<a id="browser"></a>

## 浏览器

使用这些设置来安装或启用捆绑的浏览器插件、设置 [浏览器扩展](../chrome-extension.zh-CN.md) 以及管理允许和阻止的网站。 ChatGPT 在使用网站之前会询问，除非您允许。删除被阻止的站点可以让 ChatGPT 在浏览器中使用它之前再次询问。

有关浏览器预览、评论和计算机使用工作流程，请参阅 [内置浏览器](../browser.zh-CN.md)。

<a id="computer-use"></a>

## 电脑使用

检查您的计算机使用设置，以在设置后查看桌面应用程序访问和相关首选项。在 macOS 上，通过更新 macOS 隐私和安全设置中的屏幕录制或辅助功能权限来撤销系统级访问权限。

<a id="personalization"></a>

## 个性化

选择 **友善**、**务实** 或 **无** 作为您的默认个性。使用 **无** 禁用个性指令。您可以随时更新此内容。

您还可以添加自己的自定义指令。编辑自定义指令会更新您的 [`AGENTS.md` 中的个人说明](../agent-configuration/agents-md.zh-CN.md)。

<a id="suggested-prompts"></a>

## 建议的提示

使用上下文感知建议来显示您在开始或返回 ChatGPT 时可能想要恢复的后续行动和任务。

<a id="memories"></a>

## 回忆

启用内存（如果可用），让 ChatGPT 将过去聊天中的有用上下文带入未来的工作中。有关个人聊天的设置、存储和控制，请参阅 [回忆](../customization/memories.zh-CN.md)。

<a id="archived-tasks"></a>

<a id="archived-chats"></a>

## 存档的聊天记录

**存档的聊天记录** 部分列出了已存档的聊天记录以及日期和项目上下文。使用 **取消存档** 恢复聊天。

<a id="keep-an-app-task-near-your-work"></a>
<a id="keep-an-app-chat-near-your-work"></a>
<a id="keep-a-task-near-your-work"></a>

<a id="keep-a-chat-near-your-work"></a>

## 在工作地点附近聊天

在 ChatGPT 桌面应用程序中，将活动聊天弹出到单独的窗口中，并将其放置在浏览器、编辑器或设计预览旁边。如果您希望在使用其他应用程序时聊天保持可见，请打开 **始终位于顶部**。


  

> 插图：ChatGPT 桌面应用程序聊天显示在浮动弹出窗口中