> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/environments/git-worktrees.md)。

<a id="worktrees"></a>

# Git 工作树

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

Worktrees让Codex在同一个项目中运行多个独立的聊天，而不会互相干扰。仓库、工作树和命令保留在包含项目的计算机或远程开发环境中。您可以直接在 ChatGPT 桌面应用程序中工作，或使用 ChatGPT 移动应用程序中的 [远程](../remote.zh-CN.md) 在连接的计算机上启动、指导、批准和查看工作树聊天。

对于 Git 仓库，[计划任务](../automations.zh-CN.md) 可以在专用后台工作树上运行，因此它们不会与您正在进行的工作发生冲突。在非版本控制的项目中，计划任务直接在项目目录中运行。您还可以在工作树中手动启动聊天，并使用切换在本地和工作树之间移动聊天。

工作树不在您的手机上本地运行。通过远程，移动应用程序可以控制连接的计算机上的 Codex（仓库和工作树保留在其中）或计算机使用的远程开发环境中。以下特定于桌面的说明适用于连接的计算机。

<a id="whats-a-worktree"></a>

## 什么是工作树

工作树仅适用于属于 Git 仓库的项目，因为它们在底层使用 [Git 工作树](https://git-scm.com/docs/git-worktree)。工作树允许您创建仓库的第二个副本（“签出”）。每个工作树都有自己的仓库中每个文件的副本，但它们都共享有关提交、分支等的相同元数据（`.git` 文件夹）。这允许您并行签出并在多个分支上工作。

<a id="terminology"></a>

## 术语

- **本地检出目录**：您创建的仓库。有时在 ChatGPT 桌面应用程序中简称为 **本地**。
- **工作树**：从 ChatGPT 桌面应用程序中的本地检出目录创建的 [Git 工作树](https://git-scm.com/docs/git-worktree)。
- **切换**：在本地和工作树之间移动聊天的流程。 Codex 处理在它们之间安全移动工作所需的 Git 操作。

<a id="why-use-a-worktree"></a>

## 为什么使用工作树

1. 与 Codex 并行工作，不会干扰您当前的本地设置。
2. 当您专注于前台时，将后台工作排队。
3. 当您准备好更直接地检查、测试或协作时，请将聊天移至本地。

<a id="getting-started"></a>

## 开始使用

工作树需要 Git 仓库。确保您选择的项目位于其中。

<WorkflowSteps variant="headings">

1.  选择“工作树”

在新的聊天视图中，选择编辑器下的 **工作树**。或者，选择 [当地环境](local-environment.zh-CN.md) 来运行工作树的设置脚本。

2.  选择起始分支

在编辑器下方，选择工作树所基于的 Git 分支。这可以是您的 `main` / `master` 分支、功能分支或具有未暂存本地更改的当前分支。

3.  提交您的提示

提交您的提示，Codex 将根据您选择的分支创建 Git 工作树。默认情况下，Codex 在 [“分离的头”](https://git-scm.com/docs/git-checkout#_detached_head) 中工作。

4.  选择继续工作的地方

准备好后，您可以继续直接在工作树上工作，也可以将聊天交给本地检出目录。与本地之间的切换会移动您的聊天_和_代码，以便您可以继续进行其他检出目录。

</WorkflowSteps>

<a id="working-between-local-and-worktree"></a>

## 在本地和工作树之间工作

工作树的外观和感觉很像您本地的结帐处。不同之处在于它们适合您流程的位置。您可以将Local视为前景，将Worktree视为背景。切换可让您在他们之间移动聊天。

在底层，Handoff 处理在两个检出目录之间安全移动工作所需的 Git 操作。这很重要，因为 **Git 只允许一次在一个地方签出一个分支**。如果您检出工作树上的分支，则您会同时在本地检出中检出 **不能**，反之亦然。

在实践中，有两种常见的路径：

1. [专门在工作树上工作](#option-1-working-on-the-worktree)。当您可以直接在工作树上验证更改时，此路径效果最佳，例如，因为您使用 [本地环境设置脚本](local-environment.zh-CN.md) 安装了依赖项和工具。
2. [将聊天交给本地](#option-2-handing-a-chat-off-to-local)。当您想要将聊天置于前台时，例如因为您想要检查常用 IDE 中的更改或者只能运行应用程序的一个实例，请使用此选项。

<a id="option-1-working-on-the-worktree"></a>

### 选项 1：在工作树上工作







如果您想将更改仅保留在工作树上，请使用聊天标题中的 **在这里创建分支** 按钮将工作树转变为分支。

从这里您可以提交更改，将分支推送到远程仓库，并在 GitHub 上打开拉取请求。

您可以使用标题中的“打开”按钮将 IDE 打开到工作树、使用集成终端或需要从工作树目录执行的任何其他操作。





  

> 插图：带有分支控件和工作树详细信息的工作树聊天视图







请记住，如果您在工作树上创建分支，则无法在任何其他工作树中检出它，包括本地检出。

<a id="option-2-handing-a-thread-off-to-local"></a>
<a id="option-2-handing-a-chat-off-to-local"></a>
<a id="option-2-handing-a-task-off-to-local"></a>

<a id="option-2-handing-a-chat-off-to-local"></a>

### 选项 2：将聊天交给本地







如果要将聊天置于前台，请在聊天标题中选择 **放手** 并将其移至 **本地**。

当您想要在常用的 IDE 窗口中读取更改、运行现有的开发服务器或在您日常使用的同一环境中验证工作时，此路径非常有效。

Codex 处理在工作树和本地检出目录之间安全移动聊天所需的 Git 步骤。

随着时间的推移，每个聊天都会保留相同的关联工作树。如果您稍后将聊天交回工作树，Codex 会将其返回到相同的后台环境，以便您可以从上次中断的地方继续。





  

> 插图：切换对话框将聊天从工作树移至本地







你也可以走另一个方向。如果您已经在本地工作并且想要释放前台，请使用 **放手** 将聊天移动到工作树。当您希望 Codex 继续在后台工作，同时将注意力转移回本地其他事情时，这非常有用。

由于 Handoff 使用 Git 操作，因此 `.gitignore` 文件中的任何文件都不会随聊天一起移动，除非 Codex 使用 `.worktreeinclude` 将它们复制到本地托管工作树中。

<a id="advanced-details"></a>

## 高级细节

<a id="codex-managed-and-permanent-worktrees"></a>

### Codex 管理的永久工作树

默认情况下，聊天使用Codex- 管理工作树。这些旨在感觉轻便且一次性。一个Codex-托管工作树通常专用于一次聊天，并且Codex如果您稍后将其交回那里，则将该聊天返回到同一工作树。

如果您想要一个长期存在的环境，请从侧边栏项目上的三点菜单创建永久工作树。这将创建一个新的永久工作树作为其自己的项目。永久工作树不会自动删除，您可以从同一工作树启动多个聊天。

<a id="how-codex-manages-worktrees-for-you"></a>

### Codex 如何为您管理工作树

Codex 在 `$CODEX_HOME/worktrees` 中创建工作树。起始提交是您开始聊天时选择的分支的 `HEAD` 提交。如果您选择具有本地更改的分支，Codex 也会将未提交的更改应用到工作树。工作树不作为分支检出。它处于 [分离头](https://git-scm.com/docs/git-checkout#_detached_head) 状态。这允许 Codex 创建多个工作树，而不会污染您的分支。

<a id="copy-ignored-local-files-into-managed-worktrees"></a>

### 将忽略的本地文件复制到托管工作树中

本地 Codex 管理的工作树从 Git 签出开始，因此跟踪的文件已经存在。如果您的仓库忽略新工作树所需的本地设置文件，请将 `.worktreeinclude` 文件添加到仓库根目录，并列出忽略的路径或 `.gitignore` 样式模式以在 Codex 创建托管工作树时进行复制。

将此用于 Git 故意忽略的文件，例如 `.env`、`.env.local` 或 `config/secrets.json`。 Codex 只复制与 `.worktreeinclude` 匹配的忽略文件；它不会复制 Git 不跟踪的其他本地文件。不要列出跟踪的文件。

Codex 会自动将忽略的 `AGENTS.override.md` 复制到本地托管工作树中，因此您无需将其列在 `.worktreeinclude` 中。

```text
# .worktreeinclude
.env
.env.local
config/secrets.json
```

Codex 会跳过源符号链接，并且不会覆盖新签出中已存在的文件。此行为适用于本地 ChatGPT 桌面应用程序托管工作树，而不是远程工作树或您从命令行自行创建的 Git 工作树。

<a id="branch-limitations"></a>

### 分支机构限制

假设 Codex 完成了工作树上的一些工作，并且您选择使用 **在这里创建分支** 在其上创建 `feature/a` 分支。现在，您想在本地结帐处尝试一下。如果您尝试签出该分支，您将收到以下错误：

```
fatal: 'feature/a' is already used by worktree at '<WORKTREE_PATH>'
```

要解决此问题，您需要检查工作树上的另一个分支而不是 `feature/a`。

如果您计划在本地签出分支，请使用 Handoff 将聊天移至本地，而不是尝试同时在两个位置签出同一分支。

<ToggleSection title="为什么存在这个限制">
Git 阻止同一分支同时在多个工作树中检出，因为一个分支代表一个可变引用（`refs/heads/<name>`) 其含义是工作树的“当前签出状态”。

当签出分支时，Git 将其 HEAD 视为该工作树所拥有，并期望提交、重置、变基和合并等操作以定义良好的序列化方式推进该引用。允许多个工作树同时签出同一分支会产生歧义和竞争条件，工作树的操作会围绕该条件更新分支引用，从而可能导致提交丢失、索引不一致或冲突解决方案不明确。

通过强制每个工作树一个分支规则，Git 保证每个分支都有一个权威的工作副本，同时仍然允许其他工作树通过分离的 HEAD 或单独的分支安全地引用相同的提交。

</ToggleSection>

<a id="worktree-cleanup"></a>

### 工作树清理

工作树可能会占用大量磁盘空间。每个都有自己的一组仓库文件、依赖项、构建缓存等。因此，ChatGPT 桌面应用程序尝试将工作树的数量保持在合理的限制内。

默认情况下，Codex 保留最近的 15 个 Codex 管理的工作树。如果您希望自己管理磁盘使用情况，可以更改此限制或在设置中关闭自动删除。

Codex 尝试避免删除仍然重要的工作树。在以下情况下，Codex 管理的工作树不会自动删除：

- 固定的聊天与其绑定
- 聊天仍在进行中
- 工作树是永久工作树

在以下情况下，Codex 管理的工作树会自动删除：

- 您存档关联的聊天记录
- Codex 需要删除较旧的工作树以保持在配置的限制内

在删除 Codex 管理的工作树之前，Codex 会保存其上的工作快照。如果您在删除工作树后打开聊天，您将看到恢复它的选项。

<a id="frequently-asked-questions"></a>

## 常见问题

<ToggleSection title="我可以控制工作树的创建位置吗？">
是的。默认情况下，Codex 在 `$CODEX_HOME/worktrees` 下创建托管工作树。要选择其他位置，请打开 **设置 > 工作树** 并更改 **工作树根**。
</ToggleSection>

<a id="can-i-move-a-chat-between-local-and-worktree"></a>

<ToggleSection title="我可以在 Local 和 Worktree 之间移动聊天吗？">
是的。在聊天标题中使用 **放手** 在本地结帐和工作树之间移动聊天。 Codex 处理在环境之间安全移动聊天所需的 Git 操作。如果您稍后将聊天交回工作树，Codex 会将其返回到相同的关联工作树。
</ToggleSection>

<a id="what-happens-to-chats-if-a-worktree-is-deleted"></a>

<ToggleSection title="如果删除工作树，聊天会发生什么？">
即使底层工作树目录被删除，聊天也可以保留在您的历史记录中。对于 Codex 管理的工作树，Codex 在删除工作树之前保存快照，并在您重新打开关联的聊天时提供恢复快照。当您存档永久工作树的聊天时，不会自动删除它们。
</ToggleSection>