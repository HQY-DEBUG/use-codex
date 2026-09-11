> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/chrome-extension.md)。

<a id="browser-extension"></a>

# 浏览器扩展

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 ChatGPT 浏览器扩展程序可通过 ChatGPT 桌面应用程序在 Google Chrome、Microsoft Edge、Brave、Opera 或 Vivaldi 中工作。 ChatGPT 可以在您已登录的网站（例如 LinkedIn、Salesforce、Gmail 或内部工具）上读取或执行操作。

所有五种浏览器都支持桌面应用程序的选项卡提及和浏览器控制。 Chrome、Edge、Brave 和 Vivaldi 也支持侧聊。 **Opera 不支持侧边聊天**;改为在桌面应用程序中启动其任务。

在设置另一个浏览器之前更新 ChatGPT 桌面应用程序。浏览器的可用性可能取决于部署和您的工作区设置。

要让 ChatGPT 控制其内置浏览器，请使用 `@Browser`。 [内置浏览器](https://help.openai.com/en/articles/20001277-using-the-built-in-browser-in-the-chatgpt-desktop-app) 支持登录并在 ChatGPT 内继续浏览工作，而无需使用常规浏览器配置文件。

ChatGPT 还可以根据任务需要在工具之间切换，在专用集成可用时使用插件，在需要登录浏览器上下文时使用浏览器，以及本地主机的内置浏览器。



  <Alert
    client:load
    color="warning"
    variant="soft"
    description="将页面内容视为不受信任的上下文，并在允许 ChatGPT 继续之前检查网站。"
  />



<a id="use-chatgpt-from-chrome"></a>

<a id="use-side-chat-in-your-browser"></a>

## 在浏览器中使用侧边聊天

侧边聊天可在 Chrome、Edge、Brave 和 Vivaldi 中使用。

打开您正在查看的页面旁边的 ChatGPT 以询问该页面或继续执行可将其上下文与本地文件和连接的应用程序一起使用的任务。当任务需要时，ChatGPT 可以使用打开的选项卡中的上下文。

1. 打开您想要使用的页面。
2. 从浏览器工具栏或 **扩展** 菜单中选择 ChatGPT。在 macOS 上，您还可以按<kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>.</kbd>.
3. 询问有关该页面的问题或给 ChatGPT 分配任务。

该面板与您打开它的选项卡保持一致。您在侧边聊天中开始的聊天可在 ChatGPT 应用程序中使用，并且您可以在侧边聊天中打开最近的 ChatGPT 聊天，以便您可以在任一位置继续工作。



> 插图：ChatGPT 在当前 Chrome 选项卡旁边打开。



<a id="bring-tabs-and-selected-text-into-a-chat"></a>

## 将选项卡和选定的文本带入聊天中

当您希望 ChatGPT 使用该页面作为上下文时，请在桌面应用程序中提及打开的浏览器选项卡。在具有侧边聊天功能的浏览器中，您还可以在其中提及选项卡，或突出显示页面上的文本并将选择带入聊天中以询问特定段落，而无需复制整个页面。

在支持侧边聊天的浏览器中，您还可以右键单击页面并选择 **询问 ChatGPT**。侧边聊天将打开并显示相关页面上下文，以便您可以在浏览器中继续请求。

<a id="ask-about-a-youtube-video"></a>

### 询问 YouTube 视频

打开 YouTube 视频，然后在支持的侧聊中提出有关该视频的问题。当字幕可用时，ChatGPT 可以使用视频的带时间戳的文字记录来解释、总结或回答有关内容的问题。

将网页内容、选定的文本和视频脚本视为不受信任的上下文。在要求 ChatGPT 使用该信息或对其采取行动之前，请检查该页面和任何请求的权限。

<a id="set-up-the-chrome-extension"></a>

<a id="set-up-your-browser"></a>

## 设置您的浏览器

在计算机上安装浏览器，然后在 ChatGPT 桌面应用程序中打开 **设置 > 计算机使用**。如果您的浏览器未显示在主列表中，请展开 **更多浏览器**。

1. 选择您的浏览器并按照任何提示安装所需的插件。
2. 选择浏览器旁边的 **安装** 打开其扩展商店页面。安装 ChatGPT 扩展并查看浏览器的权限提示。
3. 返回**电脑使用**，确认浏览器显示**管理**。
4. 开始 ChatGPT Work 或 Codex 聊天，然后选择包含 `@` 的浏览器。使用安装扩展程序的浏览器配置文件。

**电脑使用** 中的浏览器切换控制它是否出现在 `@` 提及菜单中。选择 **管理** 来更改网站权限。



> 插图：计算机使用设置显示通过 Chrome 扩展程序连接的 Google Chrome。



<a id="start-a-chrome-task-from-chatgpt"></a>

<a id="start-a-browser-task-from-chatgpt"></a>

## 从 ChatGPT 启动浏览器任务

设置后，开始新的 ChatGPT Work 或 Codex 聊天。从 `@` 提及菜单中选择 **镀铬**、**边缘**、**勇敢的浏览器**、**歌剧** 或 **维瓦尔第**，以选择 ChatGPT 使用的浏览器。例如：

```text
@Edge 打开 Salesforce 并从这些通话记录中更新帐户。
```

您还可以提及打开的选项卡以提供该页面中的 ChatGPT 上下文。 Opera 支持这些桌面工作流程，尽管它没有侧边聊天功能。

<a id="control-website-access"></a>

## 控制网站访问

默认情况下，ChatGPT 在与每个新网站交互之前都会进行询问。 ChatGPT 基于网站主机的提示，例如 `example.com`。

当ChatGPT要求使用网站时，您可以选择与任务和您的风险承受能力相匹配的选项：

- **允许一次** 让 ChatGPT 使用该网站一次。
- **允许该网站** 这样 ChatGPT 就可以再次使用该网站而无需询问。
- **允许所有网站** 因此 ChatGPT 无需询问即可使用网站。
- **拒绝** 阻止 ChatGPT 使用该网站。

<a id="manage-allowed-and-blocked-websites"></a>

### 管理允许和阻止的网站

在 ChatGPT 桌面应用程序中，转至 **设置** > **电脑使用**，然后选择浏览器旁边的 **管理** 以管理域的允许列表和阻止列表。允许列表包含可以使用的域 ChatGPT，无需再次询问。阻止列表包含 ChatGPT 不应使用的域。支持的浏览器共享这些网站权限。

从允许列表中删除域意味着 ChatGPT 在使用它之前会再次询问。从阻止列表中删除域意味着 ChatGPT 可以再次询问，而不是将该域视为已阻止。

<a id="allow-for-all-sites"></a>

#### 允许所有网站<ElevatedRiskBadge class="ml-2" />

如果您选择**允许所有网站**，ChatGPT在使用网站之前不再要求确认。仅当您信任 ChatGPT 使用浏览器中打开的任何网站时才选择此选项。

<a id="browser-history"></a>

#### 浏览器历史记录<ElevatedRiskBadge class="ml-2" />

浏览器历史记录可以包括敏感遥测、内部 URL、搜索词以及登录设备上浏览器会话的活动。如果您允许 ChatGPT 访问浏览器历史记录，相关历史记录条目可以成为 ChatGPT 用于任务的上下文的一部分。恶意或误导性页面内容可能会增加 ChatGPT 在无意中复制此数据的风险。

ChatGPT 询问何时要使用浏览器历史记录。 ChatGPT 限制请求的历史访问权限，并且历史记录没有始终允许的选项。

<a id="data-and-security"></a>

## 数据和安全

<a id="chrome-extension-permissions"></a>

<a id="browser-extension-permissions"></a>

### 浏览器扩展权限

安装扩展程序时，您的浏览器会要求您接受权限。例如，Chrome的权限提示可能包括：

- 访问页面调试器
- 读取和更改您在所有网站上的所有数据
- 读取和更改您在所有登录设备上的浏览历史记录
- 显示通知
- 阅读和更改您的书签
- 管理您的下载
- 与协作的本机应用程序通信
- 查看和管理您的选项卡组

这些扩展权限使其能够操作浏览器工作流程。在任务期间使用网站或浏览器历史记录之前，ChatGPT 仍然使用自己的确认、设置、允许列表和阻止列表。

<a id="memories"></a>

### 回忆

计算机使用遵循您的内存设置。如果内存打开，ChatGPT 可以在浏览器中工作时使用相关的已保存内存。如果“内存”关闭，浏览器控制将不使用内存。

<a id="what-openai-stores-from-browsing"></a>

### OpenAI 浏览时存储的内容

OpenAI不存储来自扩展程序的浏览器操作的单独完整记录。OpenAI仅当浏览器活动成为一部分时才存储它ChatGPT上下文，例如文本ChatGPT从页面、屏幕截图、工具调用、摘要、消息或聊天中包含的其他内容中读取内容。

您的 ChatGPT 数据控件适用于在上下文中处理的内容。避免通过浏览器任务发送机密或高度敏感的数据，除非需要它们并且您在场查看每个提示。

<a id="troubleshooting"></a>

## 故障排除

如果 ChatGPT 无法连接到您的浏览器，请首先确认 ChatGPT 尝试访问的网站不在“设置”中的阻止列表中。如果网站未被阻止，请进行以下检查：

1. 更新 ChatGPT 桌面应用程序。如果您安装了多个 ChatGPT 或 Codex 桌面应用程序，请更新每一个或删除不再使用的副本。
2. 重新启动浏览器。在 Chrome、Edge、Brave 或 Vivaldi 中，从工具栏或 **扩展** 菜单重新打开 ChatGPT 并确认侧面聊天加载。 Opera 没有侧边聊天功能；从桌面应用程序检查其连接。
3. 在 **设置 > 计算机使用** 中，确认您的浏览器出现并显示 **管理**。如果仍然显示 **安装**，请再次按照设置流程操作。如果 `@` 提及菜单中缺少浏览器，请打开其开关。
4. 确保您使用的是安装扩展程序的浏览器配置文件。如果您使用多个配置文件，请在活动配置文件中安装并启用该扩展。
5. 开始新的 ChatGPT Work 或 Codex 聊天，然后再次尝试浏览器任务。这可以清除特定于聊天的连接状态。
6. 重新启动 ChatGPT 桌面应用程序，然后重试。如果扩展仍然无法连接，请通过 **设置 > 计算机使用** 重新安装。
7. 如果 ChatGPT 仍然无法使用浏览器，请在应用程序中运行 `/feedback` 并在联系支持人员时提供聊天 ID。

<a id="upload-files"></a>

### 上传文件

如果 Chrome 任务需要从您的计算机上传文件，请允许 Chrome 扩展程序访问 Chrome 中的文件 URL：

1. 在 Chrome 中，打开工具栏中的扩展程序图标，然后单击 **管理扩展**。
2. 在分机卡上单击“**详情**”。
3. 打开**允许访问文件 URL**。

更改设置后，再次启动 Chrome 任务。