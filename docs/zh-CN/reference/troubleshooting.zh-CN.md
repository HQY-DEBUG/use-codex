> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/reference/troubleshooting.md)。

<a id="troubleshooting"></a>

# 故障排查

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="frequently-asked-questions"></a>

## 常见问题解答

<a id="files-appear-in-the-side-panel-that-codex-didnt-edit"></a>

### 侧面板中出现 Codex 未编辑的文件

如果您的项目位于 Git 仓库中，审核面板会根据项目的 Git 状态自动显示更改，包括 Codex 未进行的更改。

在审阅窗格中，您可以在已暂存的更改和尚未暂存的更改之间切换，并将您的分支与主分支进行比较。

如果您只想查看最后一次 Codex 回合的更改，请将差异窗格切换到 **最后一回合** 视图。

[详细了解如何使用审阅窗格](../code-review.zh-CN.md)。

<a id="remove-a-project-from-the-sidebar"></a>

### 从侧边栏删除项目

要从侧边栏中删除项目，请将鼠标悬停在项目名称上，单击三个点并选择“删除”。要恢复它，请使用 **聊天记录** 旁边的 **添加新项目** 按钮或使用

<kbd>Cmd</kbd>+<kbd>O</kbd>.

<a id="find-archived-threads"></a>
<a id="find-archived-tasks"></a>

<a id="find-archived-chats"></a>

### 查找存档的聊天记录

存档的聊天记录可以在 [设置](codex://settings) 中找到。当您取消存档聊天时，它会重新出现在其原始侧边栏位置。

<a id="only-some-threads-appear-in-the-sidebar"></a>
<a id="only-some-tasks-appear-in-the-sidebar"></a>

<a id="only-some-chats-appear-in-the-sidebar"></a>

### 仅部分聊天内容出现在侧边栏中

侧边栏可让您根据项目的状态过滤聊天。如果您缺少聊天，请选择 **聊天记录** 旁边的过滤器图标，然后选择 **时间顺序**。如果您仍然看不到聊天，请打开 [设置](codex://settings) 并检查 **存档的聊天记录**。

<a id="code-doesnt-run-on-a-worktree"></a>

### 代码不在工作树上运行

工作树在不同的目录中创建，并继承默认签入 Git 的文件。根据您管理项目的依赖项和工具的方式，您可能必须使用 [当地环境](../environments/local-environment.zh-CN.md) 在工作树上运行安装脚本，或使用 [`.worktreeinclude`](../environments/git-worktrees.zh-CN.md#copy-ignored-local-files-into-managed-worktrees) 复制忽略的安装文件。或者，您可以检查常规本地项目中的更改。请参阅 [工作树文档](../environments/git-worktrees.zh-CN.md) 了解更多信息。

<a id="app-doesnt-pick-up-a-teammates-shared-local-environment"></a>

### 应用程序无法获取队友的共享本地环境

本地环境配置必须位于项目根目录的 `.codex` 文件夹内。如果您在包含多个项目的单一仓库中工作，请确保在包含 `.codex` 文件夹的目录中打开该项目。

<a id="codex-asks-to-access-apple-music"></a>

### Codex 要求访问 Apple Music

根据您的任务，Codex 可能需要导航文件系统。 macOS 上的某些目录（包括音乐、下载或桌面）需要用户的额外批准。如果 Codex 需要读取您的主目录，macOS 会提示您批准对这些文件夹的访问。

<a id="automations-create-many-worktrees"></a>

<a id="scheduled-tasks-create-many-worktrees"></a>

### 计划任务创建许多工作树

随着时间的推移，频繁的计划任务可能会创建许多工作树。归档您不再需要的计划运行，并避免固定运行，除非您打算保留其工作树。

<a id="recover-a-prompt-after-selecting-the-wrong-target"></a>

### 选择错误目标后恢复提示

如果您意外地与错误的目标（**本地**、**工作树** 或 **云**）开始聊天，您可以通过按编辑器中的向上箭头键取消当前运行并恢复之前的提示。

<a id="feature-is-working-in-the-codex-cli-but-not-in-the-chatgpt-desktop-app"></a>

### 该功能在 Codex CLI 中有效，但在 ChatGPT 桌面应用程序中无效

ChatGPT 桌面应用程序和 Codex CLI 可以包含不同的 Codex 版本，因此功能可能会先于一个使用界面到达另一个使用界面。实验性功能也可能首先登陆 Codex CLI。

要获取系统上 Codex CLI 的版本，请运行：

```bash
codex --version
```

要获取与 ChatGPT 桌面应用程序捆绑的 Codex 版本，请使用保留的 `Codex.app` 兼容性捆绑路径：

```bash
/Applications/Codex.app/Contents/Resources/codex --version
```

<a id="feedback-and-logs"></a>

## 反馈和日志

类型<kbd>/</kbd>进入消息编辑器，为团队提供反馈。如果您在现有聊天中触发反馈，您可以选择共享现有会话以及您的反馈。提交反馈后，您将收到一个可以与团队共享的会话 ID。

报告问题：

1. 在 Codex GitHub 仓库中查找 [现有问题](https://github.com/openai/codex/issues)。
2. [打开一个新的 GitHub 问题](https://github.com/openai/codex/issues/new?template=2-bug-report.yml&steps=Uploaded%20thread%3A%20019c0d37-d2b6-74c0-918f-0e64af9b6e14)

以下位置提供了更多日志：

- 应用程序日志 (macOS)：`~/Library/Logs/com.openai.codex/YYYY/MM/DD`
- 会话成绩单：`$CODEX_HOME/sessions`（默认值：`~/.codex/sessions`）
- 存档会话：`$CODEX_HOME/archived_sessions`（默认值：`~/.codex/archived_sessions`）

如果您共享日志，请先查看它们以确认它们不包含敏感信息。

<a id="stuck-states-and-recovery-patterns"></a>

## 卡住状态和恢复模式

如果聊天出现卡住：

1. 检查Codex是否正在等待审批。
2. 打开终端并运行 `git status` 等基本命令。
3. 使用更小、更集中的提示开始新的聊天。

如果您错误地取消了工作树创建并丢失了提示，请按编辑器中的向上箭头键将其恢复。

<a id="terminal-issues"></a>

## 终端问题

**终端似乎卡住了**

1. 关闭终端面板。
2. 重新打开它<kbd>Ctrl</kbd>+<kbd>`</kbd>.
3. 重新运行 `pwd` 或 `git status` 等基本命令。

如果命令的行为与预期不同，请首先验证终端中的当前目录和分支。

如果仍然卡住，请等到活动聊天完成并重新启动应用程序。

**字体渲染不正确**

Codex 对审阅窗格、集成终端和应用程序内显示的任何其他代码使用相同的字体。您可以将 [设置](codex://settings) 窗格内的字体配置为 **代码字体**。