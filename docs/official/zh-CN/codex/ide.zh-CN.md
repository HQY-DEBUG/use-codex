> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/codex/ide.md)。

<a id="codex-ide-extension"></a>

# Codex IDE 扩展

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="build-with-the-context-already-in-your-editor"></a>

## 使用编辑器中已有的上下文进行构建

在您的代码旁边使用 Codex。将打开的文件和选择带入提示中，检查适当的编辑，并在不中断流程的情况下移交较长的工作。

> 插图：代码编辑器旁边的交互式 Codex IDE 扩展

<a id="start-here"></a>

### 从这里开始

- [安装扩展](https://marketplace.visualstudio.com/items?itemName=openai.chatgpt)
- [扩展快速入门](#getting-started)

<a id="why-use-codex-ide-extension"></a>

### 为什么使用Codex IDE扩展

- **使用已经打开的上下文：** 直接从输入框参考打开的文件、选定的代码和最近的聊天记录。 Codex 从您已经看过的代码开始，因此您可以花更少的时间重述问题。
- **查看代码旁边的更改：** 阅读摘要，检查重点差异，并在同一聊天中跟进。仅保留您想要的更改，同时源和理由保持可见。
- **当任务增长时进行委派：** 在本地保持快速迭代，或者在任务需要更多时间和空间时连接 Codex 网络。从同一编辑器工作流程返回可审阅的结果。

<a id="getting-started"></a>

## 开始使用

**开始使用您的 IDE。**

安装或启用 Codex、登录并与编辑器中已打开的上下文开始聊天。

<a id="1-install-or-enable-codex"></a>

### 1.安装或启用Codex

选择您的 IDE。 VS Code 和兼容的编辑器使用 Codex 扩展； Xcode 和 JetBrains IDE 提供自己的集成。

- [视觉工作室代码](vscode:extension/openai.chatgpt)
- [光标](cursor:extension/openai.chatgpt)
- [风帆冲浪](windsurf:extension/openai.chatgpt)
- [Visual Studio 代码内部人士](https://marketplace.visualstudio.com/items?itemName=openai.chatgpt)
- [Xcode](https://developer.apple.com/documentation/Xcode/setting-up-coding-intelligence)
- [JetBrains IDE](https://www.jetbrains.com/help/ai-assistant/codex-agent.html)

<a id="2-open-codex"></a>

### 2.打开Codex

**VS Code、光标或 Windsurf：** 选择 Codex 图标。如果它不可见，请打开命令面板并运行 **Codex：打开Codex侧边栏**。

**代码：** 打开编码助手，开始新的聊天，选择 Codex 作为智能体。

**JetBrains IDE：** 打开AI聊天并选择Codex。

<a id="3-start-your-first-chat"></a>

### 3. 开始第一次聊天

打开一个项目并要求 Codex 解释代码库、进行有针对性的更改或帮助您调试问题。在任务之前和之后创建 Git 检查点，以便您可以恢复更改。

[阅读最佳实践](https://learn.chatgpt.com/guides/best-practices)

<a id="next-steps"></a>

### 后续步骤

- [提示编辑器上下文](../prompting.zh-CN.md#use-editor-context)
- [探索 IDE 命令](../developer-commands.zh-CN.md)
- [配置扩展](../developer-settings.zh-CN.md)

<a id="see-what-codex-can-do-in-your-ide"></a>

## 查看 Codex 在您的 IDE 中可以做什么

在 Codex 解释、编辑、审查和委托时，请密切关注代码。

- [使用已经打开的上下文](../prompting.zh-CN.md#use-editor-context)：向输入框添加打开的文件、选择或最近的聊天，然后要求 Codex 解释或编辑已附加上下文的代码。
- [查看代码旁边的更改](../prompting.zh-CN.md)：无需额外的导航窗格即可查看简明摘要和更改的行。检查两个受影响的文件，保留所需的编辑，并要求从同一视图进行后续操作。
- [当任务变大时委派](https://learn.chatgpt.com/docs/cloud#delegate-from-the-ide-extension)：选择本地工作进行快速实践迭代，或连接 Codex 网络来委派较长的任务。当您返回查看结果时，聊天仍然可用。

<a id="use-codex-ide-extension-when"></a>

## 在以下情况下使用 Codex IDE 扩展...

- [您正在进行集中编辑](../prompting.zh-CN.md#use-editor-context)：将相关文件与Codex保持在同一视图中。
- [你正在学习不熟悉的代码](../prompting.zh-CN.md#use-editor-context)：询问编辑器中已打开的文件和符号。
- [您想要查看已发生的更改](../prompting.zh-CN.md)：检查源代码并应用编辑。
- [您想要委派更大的任务](https://learn.chatgpt.com/docs/cloud#delegate-from-the-ide-extension)：从IDE启动云工作并返回结果。