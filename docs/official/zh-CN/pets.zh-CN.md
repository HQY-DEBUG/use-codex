> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/pets.md)。

<a id="pets"></a>

# 桌面宠物

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

宠物是后续工作中可选的动画伙伴。宠物出现的位置和显示的内容取决于您使用的界面。选择宠物会改变它的外观，而不是ChatGPT如何完成任务。



  

    <CodexPetsDemo client:load mobileAlignment="left" />
  


<ContentModeSwitch group="codex-surface" id="app">

<a id="use-a-floating-pet"></a>

## 使用漂浮宠物

在 ChatGPT 桌面应用程序中，宠物可以漂浮在其他应用程序窗口上方，并帮助您跟踪聊天中的活动。

<a id="choose-and-wake-a-pet"></a>

### 选择并唤醒宠物

1. 打开应用程序底部的配置文件菜单，然后选择 **宠物**。您也可以打开[**设置**](codex://settings)并转到**宠物**。
2. 选择内置或定制宠物。
3. 输入 `/pet`，或打开命令菜单并选择 **唤醒宠物**。

在**设置 > 宠物**或命令菜单中选择**收起宠物**，或再次输入`/pet`，隐藏宠物。当您重新打开应用程序时，您的选择和宠物的位置仍然存在。

当您选择自定义宠物时，它也会出现在您的 **公司简介** 视图中。

<a id="understand-pet-status"></a>

### 了解宠物状况

| 状态 | 含义 |
| --------------- | -------------------------------------------------------- |
| **跑步** | 聊天正在进行中。                              |
| **需要输入** | 聊天需要您的批准、答复或其他决定。 |
| **准备好** | 聊天已完成并且有未读活动。            |
| **被阻止** | 聊天失败或遇到系统错误。             |

当多个聊天有活动时，宠物会优先考虑需要输入的聊天，然后是阻止的、就绪的和正在运行的聊天。打开活动托盘以选择聊天。

选择宠物返回ChatGPT，或选择一个活动以打开其聊天。活动托盘与 [系统通知](notifications.zh-CN.md) 分开。

<a id="follow-computer-use"></a>

### 关注电脑使用情况

在 macOS 上，[电脑使用](computer-use.zh-CN.md) 画中画窗口可以连接到醒着的宠物。移动宠物，窗口就会随之移动。

<a id="create-a-custom-pet"></a>

### 创建自定义宠物

1. 打开**设置 > 宠物**并选择**创建你自己的宠物**。
2. 该应用程序安装捆绑的 `hatch-pet` 技能，重新加载技能，并打开新的聊天。
3. 描述您想要的宠物并发送提示。
4. 任务完成后，返回**设置 > 宠物**，选择**刷新**，然后选择你的新宠物。

在桌面应用程序中创建的自定义宠物存储在您的计算机本地。它们不会自动同步到 ChatGPT 网络。

<a id="reduce-animation"></a>

### 减少动画

宠物尊重操作系统的减少运动设置。启用减少运动后，宠物将使用静止帧而不是精灵动画。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="choose-a-pet-on-the-web"></a>

## 在网络上选择宠物

如果您的帐户和工作区可以使用宠物，请打开 **设置 > 个性化 > 宠物 > 选择宠物**。选择内置宠物，或者选择**默认**在没有宠物的情况下使用ChatGPT。

网络宠物出现在支持的 ChatGPT Work 聊天中。它不提供桌面应用程序的浮动覆盖、活动托盘或 `/pet` 命令。

<a id="upload-a-custom-pet"></a>

### 上传自定义宠物

选择 **上传宠物** 添加自定义精灵表。文件必须是透明的 PNG 或 WebP，大小正好为 1536 × 1872 像素，且不大于 20 MiB。您可以在同一设置中编辑、下载、刷新或删除上传的宠物。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="choose-a-terminal-pet"></a>

## 选择末期宠物

在交互式 Codex CLI 会话中：

- 输入 `/pets` 或 `/pet` 打开宠物选择器。
- 输入“/pets”<name>` 直接选择宠物。
- 输入 `/pets off` 以禁用终端宠物。

该选择器包括内置宠物和安装在您的计算机上的兼容自定义宠物。终端 pet 报告当前 CLI 会话的活动。它使用 **跑步**、**需要输入**、**准备好** 和 **被阻止** 状态，但不提供桌面应用程序的多聊天活动托盘。

终端宠物需要 iTerm2 3.6 或更高版本，或者支持 Kitty 图形或 Sixel 的终端。它们在 tmux 和 Zellij 中不可用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="pets-in-the-ide-extension"></a>

## IDE 扩展中的宠物

Codex IDE 扩展不提供宠物选择器或浮动宠物覆盖层。当您想使用自己的宠物时，请使用 ChatGPT 桌面应用程序或 Codex CLI。

</ContentModeSwitch>




<a id="related-docs"></a>

## 相关文档

- [通知](notifications.zh-CN.md)
- [长时间运行的工作](long-running-work.zh-CN.md)
- [ChatGPT 桌面应用程序设置](reference/settings.zh-CN.md#pets)