> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/windows/windows-app.md)。

<a id="chatgpt-desktop-app-for-windows"></a>

# Windows 桌面应用

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

[ChatGPT Windows 桌面应用程序](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) 为您提供了一个界面，用于跨项目工作、运行并行聊天和查看结果。 Windows 应用程序支持核心工作流程，例如工作树、计划任务、Git 功能、内置浏览器、文件预览、插件和技能。它使用 PowerShell 和 [Windows沙箱](windows-sandbox.zh-CN.md#windows-sandbox) 在 Windows 上本机运行，或者您可以将其配置为在 [适用于 Linux 的 Windows 子系统 2 (WSL2)](#windows-subsystem-for-linux-wsl) 中运行。


  

> 插图：适用于 Windows 的 ChatGPT 桌面应用程序显示项目侧边栏、活动聊天和审阅窗格




<a id="download-the-chatgpt-desktop-app"></a>

## 下载 ChatGPT 桌面应用程序

下载适用于 Windows 的 [ChatGPT 桌面应用程序](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi)。

然后按照[快速入门](../quickstart.zh-CN.md)开始操作。

有关企业安装和更新选项，请参阅 [部署 Windows 应用程序](../enterprise/windows-deployment.zh-CN.md)。

如果您更喜欢命令行安装路径，请运行：

```powershell
winget install --id 9PLM9XGG6VKS -s msstore
```

<a id="native-sandbox"></a>

## 原生沙箱

当智能体在 PowerShell 中运行时，Windows 上的 ChatGPT 桌面应用程序支持本机 [Windows沙箱](windows-sandbox.zh-CN.md#windows-sandbox)，并在 [适用于 Linux 的 Windows 子系统 2 (WSL2)](#windows-subsystem-for-linux-wsl) 中运行智能体时使用 Linux 沙箱。要在任一模式下应用沙箱保护，请在将消息发送到 Codex 之前选择编辑器下方的 **请求批准**。

在完全访问模式下运行 Codex 意味着 Codex 不仅限于您的项目目录，并且可能会执行可能导致数据丢失的无意破坏性操作。保持沙箱边界不变，并使用 [规则](../agent-configuration/rules.zh-CN.md) 进行有针对性的例外，或者根据您的 [审批和安全设置](../agent-approvals-security.zh-CN.md) 设置 [将审批策略设为 `never`](../agent-approvals-security.zh-CN.md#run-without-approval-prompts)，让 Codex 尝试解决问题而不要求升级权限。

<a id="customize-for-your-dev-setup"></a>

## 为您的开发设置进行定制

<section class="feature-grid">




<a id="preferred-editor"></a>

### 首选编辑

选择 **打开** 的默认应用程序，例如 Visual Studio、VS Code 或其他编辑器。您可以覆盖每个项目的选择。如果您已从 **打开** 菜单中为项目选择了不同的应用程序，则该项目特定的选择优先。





  

> 插图：ChatGPT 桌面应用程序设置显示 Windows 上的默认“在应用程序中打开”




</section>

<section class="feature-grid inverse">




<a id="integrated-terminal"></a>

### 综合终端

您也可以选择默认的集成终端。根据您安装的内容，选项包括：

- 电源外壳
- 命令提示符
- git 重击
- 世界SL

此更改仅适用于新的终端会话。如果您已经打开了集成终端，请重新启动应用程序或开始新的聊天，然后再等待新的默认终端出现。





  

> 插图：ChatGPT 桌面应用程序设置显示 Windows 上的集成终端选择




</section>

<a id="windows-subsystem-for-linux-wsl"></a>

## 适用于 Linux 的 Windows 子系统 (WSL)

默认情况下，ChatGPT 桌面应用程序使用 Windows 本机 Codex 智能体。这意味着智能体在 PowerShell 中运行命令。该应用程序仍然可以在需要时使用 `wsl` CLI 来处理 Windows Subsystem for Linux 2 (WSL2) 中的项目。

如果要从 WSL 文件系统添加项目，请单击 **添加新项目** 或按<kbd>Ctrl</kbd>+<kbd>O</kbd>，然后在文件资源管理器窗口中键入 `\\wsl$\`。从那里，选择您的 Linux 发行版和您想要打开的文件夹。

如果您打算继续使用 Windows 原生智能体，最好将项目存储在 Windows 文件系统上，并通过“/mnt/”从 WSL 访问它们<drive>/...`。此设置比直接从 WSL 文件系统打开项目更可靠。

如果您希望智能体本身在 WSL2 中运行，请打开 **[设置](codex://settings)**，将智能体从 Windows 本机切换到 WSL 和 **重新启动应用程序**。更改在重新启动后才会生效。重新启动后您的项目应保持不变。

WSL1 通过 Codex `0.114` 支持。从 Codex `0.115` 开始，Linux 沙箱移至 `bubblewrap`，因此不再支持 WSL1。


  

> 插图：ChatGPT 桌面应用程序设置显示具有 Windows 本机和 WSL 选项的智能体选择器




您可以独立于智能体来配置集成终端。有关终端选项，请参阅 [为您的开发设置进行定制](#customize-for-your-dev-setup)。您可以将智能体保留在 WSL 中并仍然在终端中使用 PowerShell，或者同时使用 WSL，具体取决于您的工作流程。

<a id="useful-developer-tools"></a>

## 有用的开发者工具

当已经安装了一些常见的开发工具时，Codex 的工作效果最佳：

- **git**：为 ChatGPT 桌面应用程序中的审阅面板提供支持，并允许您检查或恢复更改。
- **Node.js**：智能体用来更有效地执行任务的常用工具。
- **蟒蛇**：智能体用来更有效地执行任务的常用工具。
- **.NET SDK**：当您想要构建本机 Windows 应用程序时很有用。
- **GitHub CLI**：为 ChatGPT 桌面应用程序中的 GitHub 特定功能提供支持。

通过将其粘贴到 [综合终端](../integrated-terminal.zh-CN.md) 或要求 Codex 安装它们，使用默认的 Windows 包管理器 `winget` 安装它们：

```powershell
winget install --id Git.Git
winget install --id OpenJS.NodeJS.LTS
winget install --id Python.Python.3.14
winget install --id Microsoft.DotNet.SDK.10
winget install --id GitHub.cli
```

安装 GitHub CLI 后，运行 `gh auth login` 以在应用程序中启用 GitHub 功能。

如果您需要不同的 Python 或 .NET 版本，请将包 ID 更改为您想要的版本。

<a id="troubleshooting-and-faq"></a>

## 故障排除和常见问题解答

<a id="run-commands-with-elevated-permissions"></a>

### 使用提升的权限运行命令

如果您需要 Codex 以提升的权限运行命令，请以管理员身份启动 ChatGPT 桌面应用程序本身。安装后，打开“开始”菜单，找到该应用程序，然后选择“**以管理员身份运行**”。 Codex 智能体继承该权限级别。

<a id="powershell-execution-policy-blocks-commands"></a>

### PowerShell 执行策略阻止命令

如果您以前从未在 PowerShell 中使用过 Node.js 或 `npm` 等工具，则 Codex 智能体或集成终端可能会遇到执行策略错误。

如果 Codex 为您创建 PowerShell 脚本，也可能会发生这种情况。在这种情况下，您可能需要一个限制较少的执行策略，然后 PowerShell 才能运行它们。

错误可能如下所示：

```text
npm.ps1 cannot be loaded because running scripts is disabled on this system.
```

常见的修复方法是将执行策略设置为 `RemoteSigned`：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

有关详细信息和其他选项，请在更改策略之前检查 Microsoft 的 [执行政策指南](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies)。

<a id="local-environment-scripts-on-windows"></a>

### Windows 上的本地环境脚本

如果您的 [当地环境](../environments/local-environment.zh-CN.md) 使用跨平台命令（例如 `npm` 脚本），您可以为每个平台保留一个共享设置脚本或一组操作。

如果您需要特定于 Windows 的行为，请创建特定于 Windows 的安装脚本或特定于 Windows 的操作。

操作在集成终端使用的环境中运行。参见 [为您的开发设置进行定制](#customize-for-your-dev-setup)。

本地设置脚本在智能体环境中运行：如果智能体使用 WSL，则运行 WSL，否则运行 PowerShell。

<a id="share-config-auth-and-sessions-with-wsl"></a>

### 与 WSL 共享配置、身份验证和会话

Windows 应用程序使用与 Windows 上本机 Codex 相同的 Codex 主目录：`%USERPROFILE%\.codex`。

如果您还在 WSL 中运行 Codex CLI，则 CLI 默认使用 Linux 主目录，因此它不会自动与 Windows 应用程序共享配置、缓存的身份验证或会话历史记录。

要共享它们，请使用以下方法之一：

- 将 WSL `~/.codex` 与文件系统上的 `%USERPROFILE%\.codex` 同步。
- 通过设置 `CODEX_HOME` 将 WSL 指向 Windows Codex 主目录：

```bash
export CODEX_HOME=/mnt/c/Users/<windows-user>/.codex
```

如果您希望在每个 shell 中进行该设置，请将其添加到 WSL shell 配置文件中，例如 `~/.bashrc` 或 `~/.zshrc`。

<a id="git-features-are-unavailable"></a>

### Git 功能不可用

如果您没有在 Windows 上本机安装 Git，则该应用程序无法使用某些功能。从 PowerShell 或 `cmd.exe` 中使用 `winget install Git.Git` 进行安装。

<a id="git-isnt-detected-for-projects-opened-from-wsl"></a>

### 从 `\\wsl$` 打开的项目未检测到 Git

目前，如果您想将 Windows 本机智能体与可从 WSL 访问的项目一起使用，最可靠的解决方法是将项目存储在本机 Windows 驱动器上，并通过“/mnt/”在 WSL 中访问它<drive>/...`.

<a id="cmder-isnt-listed-in-the-open-dialog"></a>

### `Cmder` 未在打开的对话框中列出

如果 `Cmder` 已安装但未显示在 Codex 的打开对话框中，请将其添加到 Windows 开始菜单：右键单击 `Cmder` 并选择 **添加到开始**，然后重新启动 Codex 或重新启动。