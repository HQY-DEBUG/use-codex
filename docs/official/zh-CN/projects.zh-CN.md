> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/projects.md)。

<a id="projects-and-chats"></a>

# 项目与对话

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" id="app">

使用项目组织相关聊天并为 ChatGPT 提供所需的上下文。 ChatGPT 桌面应用程序中的 **项目** 视图包括 ChatGPT 项目和连接到计算机上的文件夹的本地项目。

<a id="choose-a-project-or-start-without-one"></a>

## 选择一个项目或从没有项目开始

当工作将持续一段时间、产生多个输出或依赖相同的文件和源时，创建一个项目。当工作是独立的并且不需要共享项目上下文时，在没有项目的情况下开始聊天。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

使用项目将相关的聊天、文件、说明和源放在一起。同一项目可以包含使用 Chat 或 ChatGPT Work 启动的聊天。

<a id="choose-a-project-or-chat-without-one"></a>

## 选择一个项目或在没有项目的情况下聊天

当工作将持续一段时间、产生多个输出或依赖相同的文件和源时，创建一个项目。当工作是独立的并且不需要共享项目上下文时，在没有项目的情况下开始聊天。

每个项目都有一个列出项目聊天的 **聊天记录** 部分和一个用于上传文件和连接上下文的 **来源** 部分。项目说明适用于其聊天。 ChatGPT 项目不提供对计算机上文件夹的直接访问，因此请上传或连接您希望 ChatGPT 使用的源。

使用任一选项，从项目开始新的聊天以使用其共享文件和说明，然后返回到 **聊天记录** 下的聊天。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

Codex CLI 将您启动它的目录视为聊天项目。从您希望 Codex 工作的目录运行 `codex`，或传递 `--cd<directory>` (`-C`) 来显式设置它。 CLI 不公开 ChatGPT 项目视图。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

IDE 扩展将 IDE 中打开的文件夹或工作区视为本地项目。在多根工作区中，选择聊天的工作区根。该扩展不会从 Web 或桌面应用程序公开 ChatGPT 项目视图。

</ContentModeSwitch>

<a id="work-in-a-project"></a>

<ContentModeSwitch group="codex-surface" id="app">

<a id="work-in-a-project"></a>

## 在项目中工作

**项目** 视图将 ChatGPT 项目和本地项目集中到一处。 ChatGPT 项目在相关聊天中携带项目文件和上下文。本地项目允许聊天访问您计算机上的一个或多个文件夹，例如一组源文件或代码库。

为每个不同的结果启动单独的聊天，以便其消息和结果保持重点，同时项目保持相关工作井井有条。


  

> 插图：ChatGPT 桌面应用程序在侧边栏中显示多个项目并在主窗格中显示聊天




</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="work-in-a-project"></a>

## 在项目中工作

ChatGPT 项目允许其聊天访问相同的上传文件、项目说明和连接的源。使用 Chat 进行快速聊天，或使用 ChatGPT Work 获得更大的交付成果；两者都在项目的 **聊天记录** 部分中显示为聊天。为每个不同的结果启动单独的聊天，以便其消息和结果保持重点，同时项目保留共享上下文。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

<a id="work-in-a-project-directory"></a>

## 在项目目录中工作

从应提供聊天文件上下文的目录启动 Codex。使用 `/new` 为每个不同的结果启动单独的聊天。在 Codex 打开时使用 `/resume`，或运行 `codex resume`，以继续保存的聊天。

聊天保留其记录和记录的工作目录，而 Codex 从当前工作树读取文件。在 `AGENTS.md` 或签入文档中保留持久的项目指导，以便将来的聊天可用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

<a id="work-in-a-workspace"></a>

## 在工作区工作

打开应提供聊天文件上下文的文件夹或工作区。针对每个不同的结果开始新的聊天，然后从 **最近的聊天记录** 中选择它以继续。同一项目中的聊天可以使用相同的文件，而每个聊天都保留自己的记录。

当前选择和打开的文件为当前回合提供上下文。在 `AGENTS.md` 或签入文档中保留持久的项目指导，以便将来的聊天可用。

</ContentModeSwitch>

<a id="manage-project-threads"></a>
<a id="organize-projects-and-chats"></a>

<ContentModeSwitch group="codex-surface" id="app">

<a id="organize-projects-and-tasks"></a>

<a id="organize-projects-and-chats"></a>

## 组织项目和聊天

保持正在进行的工作可见，并将已完成的工作移开：

- **固定项目** 将其保持在侧边栏顶部附近。您还可以从“项目”视图固定它。
- **固定聊天** 当您经常返回时，即使项目中出现了较新的聊天。
- **重命名聊天** 具有描述其结果的简短标题，例如“第三季度发布简介”或“检出目录辅助功能审查”。
- “项目”视图中的 **搜索项目**。当您记住短语或分支名称但不记得标题时，从侧边栏打开 **搜索聊天记录** 即可查找过去的聊天记录。搜索聊天没有默认快捷方式，但您可以在 **设置 > 键盘快捷键** 下分配一个快捷方式。
- **存档聊天记录** 当你完成工作时。从项目的菜单中，选择 **存档聊天记录** 将其聊天记录在一起。

固定不会添加上下文或更改 ChatGPT 可以访问的内容。它只会更改项目或聊天在侧边栏中的显示位置。

从 **设置 > 存档聊天记录** 恢复存档的聊天记录。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="organize-projects-and-tasks-1"></a>

<a id="organize-projects-and-chats"></a>

## 组织项目和聊天

保持正在进行的工作可见，并将已完成的工作移开：

- **固定项目** 将其保持在侧边栏顶部附近。您还可以从“项目”视图固定它。
- **固定聊天** 当您经常返回时，即使项目中出现了较新的聊天。
- **重命名聊天** 具有描述其结果的简短标题，例如“第三季度发布简介”或“检出目录辅助功能审查”。
- “项目”视图中的 **搜索项目**。搜索过去的聊天记录
  <kbd>Cmd</kbd>/<kbd>Ctrl</kbd>+<kbd>K</kbd>当您记住短语或分支名称但不记得标题时。
- **存档聊天记录** 当你完成工作时。

固定不会添加上下文或更改 ChatGPT 可以访问的内容。它只会更改项目或聊天在侧边栏中的显示位置。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

从 **设置 > 数据控制 > 存档聊天** 恢复存档的聊天记录。

</ContentModeSwitch>

<a id="use-local-projects-for-folders-and-codebases"></a>

<ContentModeSwitch group="codex-surface" id="app">

<a id="use-local-projects-for-folders-and-codebases"></a>

## 使用本地项目作为文件夹和代码库

当ChatGPT需要读取或更改计算机上的文件时，添加本地项目。项目不需要文件夹，但您可以根据需要附加文件夹。

要添加或更改文件夹，请打开项目的菜单并选择 **编辑项目**。选择 **添加文件夹** 以附加多个文件夹。 ChatGPT 可以读取和更改每个附加文件夹中的文件。要更改默认工作目录，请指向文件夹并选择 **设为主要**。

新的聊天在主文件夹中开始。 Codex 还使用该文件夹作为 Git 操作的默认文件夹以及 `AGENTS.md`、技能和 `config.toml` 的自动发现。辅助文件夹仍可用于文件搜索、读取和编辑，但 Codex 不会自动从辅助文件夹中发现这些项目文件。

当相关工作位于不同位置时（例如应用程序及其文档或网站及其后端），请使用多个文件夹。为不相关的工作或每次聊天只应访问仓库的一部分时创建单独的项目。这可以保持工作环境的重点。远程项目目前支持一个文件夹。

使用 [当地环境](environments/local-environment.zh-CN.md) 定义项目的设置操作和常用命令。 [审阅窗格](code-review.zh-CN.md) 可以显示附加到同一项目的仓库之间的更改。拉取请求和 [工作树](environments/git-worktrees.zh-CN.md) 操作针对主仓库。当您在工作树中开始聊天时，其他文件夹仍保持附加状态。

项目和工作树组织工作，但 [沙箱](sandboxing.zh-CN.md) 强制执行本地命令可以通过网络读取、更改或访问的内容。

</ContentModeSwitch>

<a id="start-without-a-project"></a>
<ContentModeSwitch group="codex-surface" id="app">

<a id="start-a-task-without-a-project"></a>

<a id="start-a-chat-without-a-project"></a>

## 在没有项目的情况下开始聊天

当工作是独立的并且不需要共享项目文件、说明或文件夹访问权限时，请选择 **新聊天**。当多个聊天将依赖于相同的上下文时，首先创建一个项目。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="start-a-task-without-a-project-1"></a>

<a id="start-a-chat-without-a-project"></a>

## 在没有项目的情况下开始聊天

当聊天不需要共享项目文件、说明或源时，从 ChatGPT Home 开始聊天。您可以使用聊天或ChatGPT Work；在网络上，两者都会创建聊天。

如果工作量增加，请将其移至项目中，并为每个结果使用清晰的聊天名称。项目可以进行并行聊天以进行研究、起草、审查和跟进，而无需将每条消息混合到一个上下文中。

</ContentModeSwitch>

<a id="start-a-chat"></a>
<a id="start-a-standalone-chat"></a>
<ContentModeSwitch group="codex-surface" id="app">

<a id="use-quick-chat-for-a-quick-conversation"></a>

<a id="use-quick-chat-for-a-quick-question"></a>

## 使用快速聊天来快速提问

快速聊天打开普通的 ChatGPT 聊天。 ChatGPT 聊天不会出现在 Codex 侧栏中，其中包含您的 Codex 聊天和项目。

指向 **新聊天**，然后选择其右侧的 **快速聊天** 图标。您还可以按

<kbd>Cmd+Option+N</kbd>在 macOS 或<kbd>Ctrl+Alt+N</kbd>在 Windows 和 Linux 上。从 **新聊天** 中，您可以打开现有的 ChatGPT 聊天并将其添加到 Codex 聊天中。

</ContentModeSwitch>

<a id="bring-in-other-tools-and-context"></a>

## 引入其他工具和上下文

<ContentModeSwitch group="codex-surface" id="app">

- 当文件或 [图像输入](image-inputs.zh-CN.md) 仅适用于该请求时，将其直接附加到聊天中。
- 安装 [插件](plugins.zh-CN.md) 以引入其他服务的上下文和操作。
- 当您的组织或开发人员设置通过模型上下文协议公开工具时，配置 [模型上下文协议](extend/mcp.zh-CN.md) 服务器。
- 使用 [回忆](customization/memories.zh-CN.md)（如果可用）将过去工作中的有用上下文带入未来的聊天中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

- 当视觉上下文仅适用于该请求时，将 [图像输入](image-inputs.zh-CN.md) 传递给聊天。
- 安装 [插件](plugins.zh-CN.md) 以引入其他服务的上下文和操作。
- 当您的组织或开发人员设置通过模型上下文协议公开工具时，配置 [模型上下文协议](extend/mcp.zh-CN.md) 服务器。
- 使用 [回忆](customization/memories.zh-CN.md)（如果可用）将过去工作中的有用上下文带入未来的聊天中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

- 引用打开的文件或在编辑器中选择代码以添加当前回合的上下文。
- 当您的组织或开发人员设置通过模型上下文协议公开工具时，配置 [模型上下文协议](extend/mcp.zh-CN.md) 服务器。
- 使用连接的 Codex 主机中的 [回忆](customization/memories.zh-CN.md)（如果可用）将有用的上下文带入未来的聊天中。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

- 当文件和连接的源应该在其聊天中可用时，将它们添加到项目的 **来源** 部分。
- 当文件或 [图像输入](image-inputs.zh-CN.md) 仅适用于该聊天时，将其直接附加到该聊天。
- 在 ChatGPT Work 中，安装 [插件](plugins.zh-CN.md) 以引入来自其他服务的上下文和操作。
- 使用 [回忆](customization/memories.zh-CN.md)（如果可用）将过去工作中的有用上下文带入未来的聊天中。

</ContentModeSwitch>

<a id="next-steps"></a>

## 后续步骤

- [了解如何编写和完善提示](prompting.zh-CN.md)
- [了解如何使用 ChatGPT](use-chatgpt.zh-CN.md)
- [继续长期运行的工作](long-running-work.zh-CN.md)