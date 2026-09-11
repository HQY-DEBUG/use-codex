> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/windows/windows-sandbox.md)。

<a id="windows-sandbox"></a>

# Windows 沙箱

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

在 Windows 上将 Codex 与本机 [ChatGPT 桌面应用程序](windows-app.zh-CN.md)、[命令行界面](../codex/cli.zh-CN.md) 或 [IDE扩展](../codex/ide.zh-CN.md) 一起使用。

Windows 上的 ChatGPT 桌面应用程序支持核心工作流程，例如并行聊天、工作树、计划任务、Git 功能、内置浏览器、文件预览、插件和技能。

该应用程序可以通过 Windows 沙箱在 PowerShell 中本机运行，而不需要 WSL 或虚拟机。这使 Codex 保持在 Windows 本机工作流程中，同时强制执行有限的文件系统和网络权限。


  

> 插图：ChatGPT 桌面应用程序 Windows 沙箱设置提示位于消息编辑器上方






  <CodexCallout
    href="windows-app.zh-CN.md"
    title="在 Windows 上使用 ChatGPT 桌面应用程序"
    description="使用本机 Windows 应用程序在一个地方跨项目工作、运行并行聊天并查看结果。"
    iconSrc="/images/codex/codex-banner-icon.webp"
  />



本机Windows沙箱有两种模式：

- Windows 上原生具有更强大的 `elevated` 沙箱，
- 原生地在 Windows 上使用后备 `unelevated` 沙箱。





<a id="configure-the-windows-sandbox"></a>

## 配置Windows沙箱

当您在 Windows 上本机运行 Codex 时，智能体模式使用 Windows 沙箱来阻止工作文件夹外部的文件系统写入，并在未经您明确批准的情况下阻止网络访问。

本机 Windows 沙箱支持包括两种可以在 `config.toml` 中配置的模式：

```toml
[windows]
sandbox = "elevated" # or "unelevated"
```

`elevated` 是首选的本机 Windows 沙箱。它使用专用的低权限沙箱用户、文件系统权限边界、防火墙规则以及沙箱中运行的命令所需的本地策略更改。

`unelevated` 是后备本机 Windows 沙箱。它使用从当前用户派生的受限 Windows 令牌运行命令，应用基于 ACL 的文件系统边界，并使用环境级脱机控制而不是专用脱机用户防火墙规则。它比 `elevated` 弱，但当管理员批准的设置被本地或企业策略阻止时它仍然有用。

如果两种模式都可用，请使用 `elevated`。如果默认的本机沙箱在您的环境中不起作用，请在对设置进行故障排除时使用 `unelevated` 作为后备。

企业管理员可以通过 [`requirements.toml`](../enterprise/managed-configuration.zh-CN.md#admin-enforced-requirements-requirementstoml) 限制 Codex 可以使用哪些本机沙箱实现：

```toml
[windows]
allowed_sandbox_implementations = ["elevated"]
```

此示例需要 `elevated` 沙箱并防止用户回退到 `unelevated`。要允许任一实现，请包含这两个值；当未选择模式时，Codex 优先选择 `elevated`。有关支持的值，请参阅 [`requirements.toml`参考](../config-file/config-reference.zh-CN.md#requirementstoml)。

默认情况下，两种沙箱模式还使用私有桌面来实现更强的 UI 隔离。仅当您需要较旧的 `Winsta0\\Default` 行为以实现兼容性时，才设置 `windows.sandbox_private_desktop = false`。

<a id="sandbox-permissions"></a>

### 沙箱权限

在完全访问模式下运行 Codex 意味着 Codex 不仅限于您的项目目录，并且可能会执行可能导致数据丢失的无意破坏性操作。为了更安全地实现自动化，请保留沙箱边界并针对特定异常使用 [规则](../agent-configuration/rules.zh-CN.md)，或者根据 [审批和安全设置](../agent-approvals-security.zh-CN.md) 设置 [将审批策略设为 `never`](../agent-approvals-security.zh-CN.md#run-without-approval-prompts)，让 Codex 尝试解决问题而不要求升级权限。

<a id="windows-version-matrix"></a>

### Windows 版本矩阵

| Windows 版本 | 支持级别 | 注释 |
| -------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows 11 | 推荐 | Windows 上 Codex 的最佳基准。如果您正在标准化企业部署，请使用此选项。                                                                                       |
| 最近，完全更新的 Windows 10 | 尽力而为 | 可以工作，但不如 Windows 11 可靠。对于 Windows 10，Codex 依赖于现代控制台支持，包括 ConPTY。实际上，需要 Windows 10 版本 1809 或更高版本。 |
| 较旧的 Windows 10 版本 | 不推荐 | 更有可能错过 ConPTY 等必需的控制台组件，并且更有可能在企业设置中失败。                                                                          |

其他环境假设：

- `winget` 应该可用。如果缺少，请在设置 Codex 之前更新 Windows 或安装 Windows 包管理器。
- 推荐的本机沙箱取决于管理员批准的设置。
- 即使操作系统版本本身可以接受，某些企业管理的设备也会阻止所需的设置步骤。

<a id="grant-sandbox-read-access"></a>

### 授予沙箱读取权限

当命令因 Windows 沙箱无法读取目录而失败时，请使用：

```text
/sandbox-add-read-dir C:\absolute\directory\path
```

该路径必须是现有的绝对目录。命令成功后，沙箱中运行的后续命令可以在当前会话期间读取该目录。





默认情况下使用本机 Windows 沙箱。当您需要 Linux 本机工具、您的工作流程已存在于 WSL2 中或本机 Windows 沙箱模式都无法满足您的需求时，请选择 [世界SL](wsl.zh-CN.md)。

<a id="troubleshooting-and-faq"></a>

## 故障排除和常见问题解答

如果您要对托管 Windows 计算机进行故障排除，请从本机沙箱模式、Windows 版本以及 Codex 显示的任何策略错误开始。大多数本机 Windows 支持问题来自沙箱设置、登录权限或文件系统权限，而不是编辑器本身。



<a id="my-native-sandbox-setup-failed"></a>

### 我的本机沙箱设置失败


如果 Codex 无法完成 `elevated` 沙箱设置，最常见的原因是：

- Windows UAC 或管理员提示被拒绝，
- 该计算机不允许创建本地用户或组，
- 机器不允许更改防火墙规则，
- 该机器阻止沙箱用户所需的登录权限，
- 或其他企业策略阻止部分设置流程。

尝试什么：

1. 再次尝试 `elevated` 沙箱设置，并在您的环境允许的情况下批准管理员提示。
2. 如果您公司的笔记本电脑阻止此操作，请询问您的 IT 团队，该计算机是否允许管理员批准的本地用户/组创建设置、防火墙配置以及所需的沙箱用户登录权限。
3. 如果默认设置仍然失败，请使用 `unelevated` 沙箱，以便您可以在调查问题时继续工作。







<a id="codex-switched-me-to-the-unelevated-sandbox"></a>

### Codex 将我切换到未升高的沙箱


这意味着 Codex 无法在您的计算机上完成更强大的 `elevated` 沙箱设置。

- Codex仍然可以在沙箱模式下运行。
- 它仍然应用基于 ACL 的文件系统边界，但它不使用 `elevated` 中单独的沙箱用户边界，并且网络隔离较弱。
- 这是一个有用的后备方案，但不是首选的长期企业配置。

如果您使用的是受管理的企业笔记本电脑，最佳的长期解决方案通常是在 IT 团队的帮助下让 `elevated` 沙箱正常工作。







<a id="i-see-windows-error-1385"></a>

### 我看到 Windows 错误 1385


如果沙箱命令失败并出现错误 `1385`，则 Windows 拒绝沙箱用户启动该命令所需的登录类型。

实际上，这通常意味着 Codex 成功创建了沙箱用户，但 Windows 策略仍然阻止这些用户启动沙箱命令。

该怎么做：

1. 询问您的 IT 团队设备策略是否向 Codex 创建的沙箱用户授予所需的登录权限。
2. 如果问题仅影响某些计算机或团队，请比较组策略或 OU 差异。
3. 如果您需要立即继续工作，请在调查策略问题时使用 `unelevated` 沙箱。
4. 发送 `CODEX_HOME/.sandbox/sandbox.log` 以及您的 Windows 版本和故障的简短描述。







<a id="codex-warns-that-some-folders-are-writable-by-everyone"></a>

### Codex 警告某些文件夹可供所有人写入


Codex 可能会警告某些文件夹可由 `Everyone` 写入。

如果您看到此警告，则这些文件夹的 Windows 权限过于广泛，沙箱无法完全保护它们。

该怎么做：

1. 查看警告中列出的文件夹 Codex。
2. 如果适合您的环境，请从这些文件夹中删除 `Everyone` 写入权限。
3. 更正这些权限后，重新启动 Codex 或重新运行沙箱设置。

如果您不确定如何更改这些权限，请向您的 IT 团队寻求帮助。







<a id="sandboxed-commands-cannot-reach-the-network"></a>

### 沙箱命令无法到达网络


某些 Codex 聊天有意在没有出站网络访问权限的情况下运行，具体取决于所使用的权限模式。

如果任务因无法到达网络而失败：

1. 检查任务是否应该在禁用网络的情况下运行。
2. 如果您希望访问网络，请重新启动 Codex 并重试。
3. 如果问题持续发生，请收集沙箱日志，以便团队可以检查计算机是否处于部分或损坏的沙箱状态。







<a id="sandboxing-worked-before-and-then-stopped"></a>

### 沙箱以前有效，然后停止了


这可能发生在以下时间之后：

- 移动仓库或工作区，
- 更改机器权限，
- 改变Windows政策，
- 或其他系统配置更改。

尝试什么：

1. 重新启动Codex。
2. 再次尝试 `elevated` 沙箱设置。
3. 如果这不能解决问题，请使用 `unelevated` 沙箱作为临时后备。
4. 收集沙箱日志以供审核。







<a id="i-need-to-send-diagnostics-to-openai"></a>

### 我需要将诊断发送到 OpenAI


如果仍有问题，请发送：

- `CODEX_HOME/.sandbox/sandbox.log`

包括以下内容也很有帮助：

- 对您想要做的事情的简短描述，
- `elevated`沙箱是否失败或使用了`unelevated`沙箱，
- 应用程序中显示的任何错误消息，
- 无论您看到 `1385` 还是其他 Windows 或 PowerShell 错误，
- 以及您使用的是 Windows 11 还是 Windows 10。

请勿发送：

- `CODEX_HOME/.sandbox-secrets/` 的内容







<a id="the-ide-extension-is-installed-but-unresponsive"></a>

### IDE 扩展已安装但无响应



您的系统可能缺少 C++ 开发工具，某些本机依赖项需要这些工具：

- Visual Studio 构建工具（C++ 工作负载）
- Microsoft Visual C++ 可再发行组件 (x64)
- 使用`winget`，运行`winget install --id Microsoft.VisualStudio.2022.BuildTools -e`

安装后完全重新启动 VS Code。