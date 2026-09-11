> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/codex/cli.md)。

<a id="codex-cli"></a>

# Codex CLI 入门

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="inspect-edit-and-run-code-from-your-terminal"></a>

## 从终端检查、编辑和运行代码

无需离开终端即可检查代码、进行更改、运行命令并自动执行可重复的工作。

<a id="start-here"></a>

### 从这里开始

- [安装Codex](#getting-started)
- [CLI 参考](../developer-commands.zh-CN.md)

<a id="why-use-codex-cli"></a>

### 为什么使用 Codex CLI

- **针对您的本地仓库工作：** 让 Codex 检查文件、进行编辑并运行计算机上已安装的工具。
- **保持控制：** 选择适合任务的模型、推理工作、权限和命令。
- **使用脚本和 CI 进行编写：** 交互使用 Codex 或从可重复的工作流程和管道中调用 codex exec。

<a id="getting-started"></a>

## 开始使用

**开始使用 Codex CLI。**

安装 Codex、登录并从项目目录运行您的第一个任务。

<a id="1-install-codex"></a>

### 1.安装Codex

选择以下安装方法之一：

<a id="macoslinux"></a>

#### macOS/Linux

使用适用于 macOS 和 Linux 的独立安装程序安装 Codex CLI。

**安装：**

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

**更新：**

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

<a id="windows"></a>

#### 窗户

使用适用于 Windows 的独立安装程序安装 Codex CLI。

**安装：**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

**更新：**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

<a id="npm"></a>

#### 新项目管理

您还可以使用 npm 安装 Codex CLI。

**安装：**

```bash
npm install -g @openai/codex
```

**更新：**

```bash
npm install -g @openai/codex
```

<a id="homebrew"></a>

#### 自制

您还可以使用 Homebrew 安装 Codex CLI。

**安装：**

```bash
brew install --cask codex
```

**更新：**

```bash
brew upgrade --cask codex
```

<a id="2-run-codex-and-sign-in"></a>

### 2.运行Codex并登录

打开项目目录并运行 `codex`。首次运行 Codex 时，请选择 **使用 ChatGPT 登录** 或其他可用的登录方法。

[查看身份验证选项](../auth.zh-CN.md)

<a id="3-start-your-first-task"></a>

### 3. 开始你的第一个任务

描述您想要实现的目标。例如，要求 Codex 解释该项目、进行重点更改或帮助调试问题。

```text
告诉我这个项目
```

在任务之前和之后创建 Git 检查点，以便您可以恢复更改。请参阅 [最佳实践](https://learn.chatgpt.com/guides/best-practices)。

<a id="next-steps"></a>

### 后续步骤

- [探索 CLI 参考](../developer-commands.zh-CN.md)
- [配置Codex](../configuration.zh-CN.md)
- [使用 Codex exec 实现自动化](../non-interactive-mode.zh-CN.md)

<a id="see-what-codex-cli-can-do"></a>

## 看看 Codex CLI 可以做什么

使用一个集中的终端循环进行交互式工作、自动化、审查和委派。

- [将编码循环保留在终端中](../developer-commands.zh-CN.md)：在仓库中启动 Codex 来探索不熟悉的代码、计划更改、编辑文件并运行本地开发工具。控制主动对话轮次，检查出现的命令和差异，并在同一会话中进行后续工作。
- [使用技能和插件](../skills-and-plugins.zh-CN.md)：将可重复的指令打包为技能，然后添加插件以将 Codex 连接到团队的工具和数据，而无需离开 CLI。
- [在发货前查看更改](../code-review.zh-CN.md)：针对未提交的更改、提交或基础分支运行专门的审查。 Codex 在不修改工作树的情况下报告优先结果，因此您可以在提交或打开拉取请求之前解决风险。

<a id="build-a-terminal-workflow-around-codex"></a>

## 围绕Codex构建终端工作流程

了解可用于恢复会话、添加视觉和 Web 上下文、分解复杂工作以及将 Codex 连接到您的开发工具的 CLI 功能。

- [**返回已保存的聊天记录**](../developer-commands.zh-CN.md#codex-resume) — `codex resume`：从当前仓库重新打开最近的聊天，或者在需要返回旧工作时在本地聊天中搜索。
- [**将视觉上下文带入提示中**](../image-inputs.zh-CN.md) — `codex --image`：通过第一个提示传递错误屏幕截图、架构图或设计参考，或将图像粘贴到交互式编辑器中。
- [**拆分更大规模的调查**](../agent-configuration/subagents.zh-CN.md) — `subagents`：要求 Codex 将重点工作委托给专门智能体，然后将他们的发现带回主终端会话。
- [**搜索当前上下文**](../web-search.zh-CN.md) — `codex --search`：当任务依赖于当前版本、文档或外部行为时，将运行切换为实时网络搜索。搜索活动在记录中保持可见。
- [**将工作转移到 Codex 云**](https://learn.chatgpt.com/docs/cloud#use-codex-cloud-from-the-cli) — `codex cloud`：浏览活动和已完成的聊天，将工作提交到配置的环境，并将结果从终端应用到本地仓库。
- [**连接外部工具与 MCP**](../extend/mcp.zh-CN.md) — `codex mcp`：添加本地或远程 MCP 服务器，在需要时进行身份验证，并在 Codex 使用当前会话之前检查可用的工具。
- [**设置每次运行的边界**](../agent-approvals-security.zh-CN.md) — `/permissions`：选择 Codex 何时可以编辑文件或运行命令而无需询问，并在继续之前检查活动沙箱和可写根。
- [**将 Codex 安装到您的终端**](../cli-customization.zh-CN.md) — `codex completion`：为您的 shell 生成补全，选择语法主题，并在 VISUAL 或 EDITOR 配置的编辑器中打开较长的提示。

<a id="use-codex-cli-when"></a>

## 在以下情况下使用 Codex CLI：

- [您从终端工作](../developer-commands.zh-CN.md)：在一个集中循环中探索、编辑和运行仓库。
- [您需要脚本或 CI](../non-interactive-mode.zh-CN.md)：在可重复的工作流程中运行非交互式命令。
- [您想要本地代码审查](../code-review.zh-CN.md)：在提交或打开拉取请求之前检查更改。
- [您想将工作交给云端](https://learn.chatgpt.com/docs/cloud#use-codex-cloud-from-the-cli)：启动云聊天，稍后返回终端。