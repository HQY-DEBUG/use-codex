> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/environments/local-environment.md)。

<a id="local-environments"></a>

# 本地环境配置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

本地环境允许您配置工作树的设置步骤以及项目的常见操作。

本地环境仅在 ChatGPT 桌面应用程序的 Codex 中可用。在配置或使用本地环境之前，请选择“**Codex**”。

您可以通过 [ChatGPT 桌面应用程序设置](codex://settings) 窗格配置本地环境。您可以将生成的文件签入项目的 Git 仓库中以与其他人共享。

Codex 将此配置存储在项目根目录的 `.codex` 文件夹中。如果您的仓库包含多个项目，请打开包含共享 `.codex` 文件夹的项目目录。

<a id="setup-scripts"></a>

## 设置脚本

由于工作树与本地聊天在不同的目录中运行，因此您的项目可能未完全设置，并且可能缺少未签入仓库的依赖项或文件。当 Codex 在新聊天开始时创建新工作树时，安装脚本会自动运行。

使用此脚本运行配置环境所需的任何命令，例如安装依赖项或运行构建过程。

例如，对于 TypeScript 项目，您可能需要安装依赖项并使用安装脚本进行初始构建：

```bash
npm install
npm run build
```

如果您的设置是特定于平台的，请定义 macOS、Windows 或 Linux 的设置脚本以覆盖默认值。

<a id="actions"></a>

## 行动

<section class="feature-grid">



使用操作来定义常见任务，例如启动应用程序的开发服务器或运行测试套件。这些操作显示在 ChatGPT 桌面应用程序顶部栏中，以便快速访问。这些操作在应用程序的 [综合终端](../integrated-terminal.zh-CN.md) 内运行。

操作有助于防止您键入常见操作，例如触发项目构建或启动开发服务器。如需一次性快速调试，您可以直接使用集成终端。





  

> 插图：ChatGPT 桌面应用程序设置中显示的项目操作列表




</section>

例如，对于 Node.js 项目，您可以创建一个包含以下脚本的“运行”操作：

```bash
npm start
```

如果操作的命令是特定于平台的，请为 macOS、Windows 和 Linux 定义特定于平台的脚本。

要识别您的操作，请选择与每个操作关联的图标。

<a id="use-built-in-git-tools"></a>

## 使用内置的 Git 工具







在 Codex 中，ChatGPT 桌面应用程序在每个本地项目和工作树旁边提供通用的 Git 控件。差异窗格显示当前结帐中的更改，并允许您添加 Codex 的内联注释以进行解决。您可以暂存或恢复单个块、暂存或恢复整个文件、提交更改、推送分支以及创建拉取请求，而无需离开应用程序。

将 [综合终端](../integrated-terminal.zh-CN.md) 用于应用程序中未公开的 Git 操作。要将并发更改与本地结帐隔离，请在 [工作树](git-worktrees.zh-CN.md) 中启动任务。






> 插图：Codex 环境摘要面板