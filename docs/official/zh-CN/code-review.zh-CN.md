> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/code-review.md)。

<a id="code-review"></a>

# 代码审查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

在提交或推送代码更改之前，使用 ChatGPT 或 Codex 检查代码更改。

<a id="start-a-review"></a>

## 开始评论

<ContentModeSwitch group="codex-surface" id="web">

在 ChatGPT Work 中，上传您要审核的代码或通过已安装的源 [插件](plugins.zh-CN.md) 提供该代码。在提示中，确定拉取请求、分支、提交、文件和审核条件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="review-in-the-app"></a>

### 在应用程序中查看

打开审核窗格以了解更改的内容，提供特定于行的反馈，并决定暂存、恢复、提交或推送哪些内容。

要要求 Codex 检查更改，请在编辑器中键入 `/review`。选择**针对基础分支进行审查**或**审查未提交的更改**。 Codex 报告优先发现结果，而无需更改工作树。

审阅窗格需要 Git 仓库内的项目。如果您的项目还不是 Git 仓库，应用程序会提示您创建一个。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

键入 `/review` 打开 CLI 查看预设。 Codex 启动一个专门的审阅器，读取所选的差异并报告优先的、可操作的发现，而无需更改您的工作树。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

在 IDE 扩展编辑器中输入 `/review`。选择**针对基础分支进行审查**或**审查未提交的更改**。 Codex 报告优先发现结果，而无需更改工作树。

仅当打开的项目位于 Git 仓库内时，才会出现 `/review` 命令。

</ContentModeSwitch>

<a id="choose-a-review-scope"></a>

## 选择审核范围

<ContentModeSwitch group="codex-surface" id="web">

命名要在提示中检查的拉取请求、分支、提交或文件。要查看无法通过已安装的源插件获得的本地文件，请将它们上传到聊天室。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="what-changes-it-shows"></a>

### 它显示了什么变化

审阅窗格反映了 Git 仓库的状态，而不仅仅是 Codex 编辑的内容。它包括 Codex 所做的更改、您自己所做的更改以及仓库中任何其他未提交的更改。

默认情况下，审阅窗格显示 **未上演** 更改。使用 **上演** 作为 Git 索引，使用 **提交** 作为选定的提交，使用 **分公司** 作为与基础分支的差异，或者使用 **最后一回合** 作为最近的助理轮次。

<a id="review-multiple-repositories"></a>

### 审查多个仓库

当 [本地项目包含多个文件夹](projects.zh-CN.md#use-local-projects-for-folders-and-codebases) 由不同的 Git 仓库支持时，审阅窗格可以显示每个仓库的更改。打开审阅标题中的仓库选择器以检查另一个仓库并查看添加或删除的行，而无需离开当前审阅窗格。

选择 **最后一回合** 可查看助手在附加仓库中的最新更改。仓库选择器显示该视图的 **所有仓库**。其他审核范围（例如 **未上演**、**上演** 和 **分公司**）适用于您选择的仓库。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

选择以下 `/review` 示波器之一：

- **针对基础分支进行审查** 找到合并基础并检查您的分支差异。
- **审查未提交的更改** 包括暂存、未暂存和未跟踪文件。
- **审查提交** 检查所选提交的确切更改集。
- **自定义审核说明** 重点审查您提供的标准。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

选择以下 `/review` 示波器之一：

- **针对基础分支进行审查** 将您当前的分支与您选择的分支进行比较。
- **审查未提交的更改** 检查工作树中的更改。

</ContentModeSwitch>

<a id="work-with-review-results"></a>

## 处理审核结果

<ContentModeSwitch group="codex-surface" id="web">

审核结果显示在网络聊天中。要求提供证据，要求缩小范围的后续审查，或要求 ChatGPT 准备修订文件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="code-review-results"></a>

### 代码审查结果

审阅结果在审阅窗格中显示为内嵌注释。

默认情况下，评论在当前聊天中运行。在 **设置** > **一般** > **代码审查** 下，选择 **独立的** 开始单独的评论聊天。参见 [开发者设置](https://learn.chatgpt.com/docs/developer-settings?surface=app#app-code-review)。


  

> 插图：审阅窗格中显示的内联代码审阅注释




</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

评论在笔录中显示为轮流。当您希望审阅使用与当前会话不同的模型时，请在 `config.toml` 中设置 `review_model`。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

默认情况下，审核在当前聊天中运行。当您希望 `/review` 开始单独的评论聊天时，将 `chatgpt.reviewDelivery` 设置为 `detached`。请参阅 [IDE扩展设置参考](https://learn.chatgpt.com/docs/developer-settings?surface=ide#ide-editor-settings-reference)。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

如果您要求 ChatGPT 准备修订后的文件，则聊天可用的工具和工作区权限仍然适用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

如果您要求 Codex 应用它找到的修复程序，则正常的 [沙箱和审批设置](sandboxing.zh-CN.md) 将适用。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

<a id="navigating-the-review-pane"></a>

## 浏览审阅窗格

- 单击文件名通常会在您选择的编辑器中打开该文件。您可以选择[开发者设置](https://learn.chatgpt.com/docs/developer-settings?surface=app#app-project-and-terminal-behavior)中的默认编辑器。
- 单击文件名背景可展开或折叠差异。
- 按住时单击单行<kbd>Cmd</kbd>按下将在您选择的编辑器中打开该行。
- 如果您对更改感到满意，您可以 [暂存或恢复更改](#staging-and-reverting-files) 您不想要的。

<a id="inline-comments-for-feedback"></a>

## 内嵌评论以获取反馈

内联注释让您可以将反馈直接附加到差异中的特定行。这通常是指导 Codex 进行正确修复的最快方法。

要留下内嵌评论：

1. 打开审阅窗格。
2. 将鼠标悬停在您要评论的行上。
3. 选择出现的 **+** 按钮。
4. 写下您的反馈并提交。
5. 留下反馈后，将消息发送回聊天室。

由于注释是特定于行的，因此 Codex 可以比一般指令更精确地响应。

Codex 将内嵌注释视为审阅指导。留下评论后，发送一条后续消息，明确您的意图，例如“解决内联评论并保持最小范围”。

<a id="pull-request-reviews"></a>

## 拉取请求评论

当 Codex 对您的仓库具有 GitHub 访问权限并且当前项目位于拉取请求分支上时，ChatGPT 桌面应用程序可以帮助您处理拉取请求反馈，而无需离开应用程序。侧边栏显示拉取请求上下文和审阅者的反馈，审阅窗格显示差异旁边的评论，以便您可以要求 Codex 在同一聊天中解决问题。

安装 GitHub CLI (`gh`) 并使用 `gh auth login` 对其进行身份验证，以便 Codex 可以加载拉取请求上下文、查看评论和更改的文件。如果 `gh` 丢失或未经身份验证，拉取请求详细信息可能不会显示在侧边栏或审阅窗格中。

当您想要将完整的修复循环保留在一个位置时，请使用此流程：

1. 打开拉取请求分支上的审阅窗格。
2. 查看拉取请求上下文、注释和更改的文件。
3. 请 Codex 修复您想要处理的具体评论。
4. 在审阅窗格中检查生成的差异。
5. 准备好后，暂存、提交更改并将其推送到拉取请求分支。

对于 GitHub 触发的评论，请参阅 [在GitHub中使用Codex](third-party/github.zh-CN.md)。

<a id="staging-and-reverting-files"></a>

## 暂存和恢复文件

审阅窗格包含 Git 操作，因此您可以在提交之前调整差异。

您可以在以下级别暂存、取消暂存或恢复更改：

- **整个差异**：使用审阅标题中的操作按钮，例如 **舞台全部** 或 **全部恢复**。
- **每个文件**：暂存、取消暂存或恢复单个文件。
- **每个大块头**：暂存、取消暂存或恢复单个块。

当您想要接受部分工作时使用暂存，当您想要放弃它时使用恢复。

<a id="staged-and-unstaged-states"></a>

### 分阶段和非分阶段状态

Git 可以表示同一文件中的暂存和未暂存更改。发生这种情况时，窗格可以在两个视图中显示相同的文件。这是正常的 Git 行为。

</ContentModeSwitch>