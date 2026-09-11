> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/remote-connections.md)。

<a id="remote-connections"></a>

# 远程连接

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

远程连接使您可以访问另一台设备或计算机上运行的工作。在 ChatGPT 移动应用程序中，打开 **远程** 以在连接的 Mac 或 Windows 设备上处理 ChatGPT 或 Codex 聊天。您还可以从运行 ChatGPT 桌面应用程序的另一台受支持的设备继续工作，或将该应用程序连接到 SSH 主机上的项目。

远程访问使用连接主机的项目、聊天、文件、凭据、权限、插件、计算机使用、浏览器设置和本地工具。

<a id="what-you-can-do-remotely"></a>

## 您可以远程做什么

- 在主机上的项目中开始新的聊天，或继续现有的聊天。
- 发送后续指示、回答问题并指导积极的工作。
- 批准命令和其他行动。
- 查看输出、差异、测试结果、终端输出和屏幕截图。
- 当 ChatGPT 完成任务或需要您注意时收到通知。
- 在连接的主机和聊天之间切换。

接下来的部分介绍在 ChatGPT 移动应用程序中打开 **远程** 以访问桌面主机。要将 Codex 连接到 SSH 主机上的项目，请参阅 [连接到 SSH 主机](#connect-to-an-ssh-host)。



  
    

> 插图：ChatGPT 移动应用程序中的远程设置屏幕


  



<a id="before-you-set-up-mobile-access"></a>

<a id="before-you-set-up-remote"></a>

## 设置远程之前

Remote 支持在 macOS 和 Windows 上运行 ChatGPT 桌面应用程序的主机。您可以在 iOS 或 Android 上从 ChatGPT 控制主机，或者在 **控制其他设备** 可用时从其他 Mac 或 Windows 设备控制主机。可用性可能因部署而异。

确保您有：

- 您要使用的 ChatGPT 帐户和工作区中的 Codex 访问权限。
- iOS 或 Android 设备上的最新 ChatGPT 移动应用程序。如果应用程序中没有出现**远程**，请先更新ChatGPT。
- 适用于 macOS 或 Windows 的最新 ChatGPT 桌面应用程序在处于唤醒状态、在线并登录到同一帐户和工作区的主机上运行。移动设置从应用程序开始；您无法从 Codex CLI 或 IDE 扩展进行设置。
- 该帐户或工作区所需的任何多重身份验证、SSO 或密钥配置。

如果您通过 ChatGPT 工作区使用 Codex，您的管理员可能需要启用远程控制访问，然后您才能从手机进行连接。

<a id="set-up-mobile-access"></a>

<a id="set-up-remote"></a>

## 设置远程

在要连接的主机上的 ChatGPT 桌面应用程序中启动。设置流程支持该主机的远程访问，然后显示可以从手机扫描的二维码。二维码将该电话与该主机配对。将每部手机或支持的桌面应用程序设备与您希望其控制的每台主机配对。

自 2026 年 6 月 8 日起使用的现有连接仍保持配对状态。如果您自 2026 年 6 月 8 日起未使用现有连接，请更新这两个应用程序并再次配对设备。

<WorkflowSteps variant="headings">

1. 开始远程设置。

在主机上打开ChatGPT桌面应用程序。转至 **设置** > **连接** > **控制这台 Mac 或 PC**，然后选择 **设置** 或 **添加**。批准远程访问并完成任何请求的验证。

2. 扫描二维码。

使用手机扫描应用程序显示的二维码。该代码将打开 ChatGPT，以便您可以完成移动应用程序与主机的连接。

3. 在ChatGPT中完成设置。

ChatGPT 打开远程设置流程。确认相同的 ChatGPT 帐户和工作区，然后完成任何所需的多重身份验证、SSO 或密钥步骤。设置成功后，主机会出现在手机的“远程”中。

4. 检查主机设置。

在主机上的应用程序中，使用 **设置** > **连接** 来管理连接的设备。您还可以选择是否让计算机保持唤醒状态、启用计算机使用或安装 Chrome 扩展程序。

</WorkflowSteps>



> 插图：连接控件允许设备控制此 Mac 并使其保持唤醒状态。



<a id="choose-what-to-connect"></a>

## 选择要连接的内容

从您已使用 ChatGPT 的笔记本电脑或台式机开始。当您需要连续访问或不同的环境时，添加始终在线的计算机或 SSH 主机。

### 

<Desktop width={17} height={17} />

您的笔记本电脑或台式机



连接已安装桌面应用程序的 Mac 或 Windows PC。这可以远程访问您已使用的相同项目、聊天、凭据、插件和本地设置。

如果该计算机处于睡眠状态、失去网络访问权限或关闭应用程序，远程访问就会停止，直到再次可用为止。如果您使用此计算机作为主机设备，请保持其插入状态并使用主机的连接设置使其在可用时保持唤醒状态。

在 Mac 笔记本电脑上，即使盖子打开并连接电源，也可以进行远程访问。盖上盖子后，还可以连接外部显示器。选择 **睡眠** 仍会停止远程访问。

在 Windows 主机上，保持会话解锁并可用于使用 [电脑使用](computer-use.zh-CN.md) 的任务。 Windows 上的“计算机使用”在前台运行，因此远程控制最适合在您将主机桌面专用于任务时开始或检查工作。

### 

<Storage width={17} height={17} />

专用的永远在线的计算机



当您希望 ChatGPT 能够进行长时间运行的工作时，请使用专用的始终在线的 Mac 或 Windows PC。

安装 ChatGPT 或 Codex 应在该计算机上使用的项目、凭据、MCP 服务器、技能和工具。

### 

<Terminal width={17} height={17} />

远程开发环境



当项目已位于远程环境中时，使用 SSH 主机或托管远程开发环境。首先将桌面应用程序主机连接到该环境； your phone still connects to the same host, and ChatGPT works in the remote environment with its dependencies, security policies, and compute resources.

有关 SSH 设置详细信息，请参阅 [连接到 SSH 主机](#connect-to-an-ssh-host)。

对于始终在线的计算机或远程主机上的浏览器或桌面任务，请启用计算机使用并在该主机上安装 Chrome 扩展程序。

<a id="what-comes-from-the-connected-host"></a>

## 来自连接的主机的内容

您的手机会向 ChatGPT 发送提示、批准和后续消息。连接的主机提供ChatGPT使用的环境。

这意味着：

- 仓库文件和本地文档来自连接的主机。
- Shell 命令在该主机或远程环境上运行。
- MCP 服务器、技能、浏览器访问和计算机使用来自该主机的配置。
- 仅当主持人可以访问登录的网站和桌面应用程序时，它们才可用。
- 沙箱设置、安全控制和操作批准仍然适用于连接的会话。

安全中继层让你已授权的 ChatGPT 设备能够访问受信任的计算机，而无须将这些计算机直接暴露在公共互联网上。

<a id="pick-up-work-from-another-device"></a>

## 从另一台设备接取工作

您可以从运行 ChatGPT 桌面应用程序并支持远程控制的另一台登录设备继续工作。 For example, if your laptop is unavailable, you can start a chat from your phone on an always-on host, then later open the app on your laptop and continue that same chat there.

在支持该功能的 Mac 或 Windows 设备上，使用 **设置 > 连接 > 控制其他设备** 添加其他主机。一个设备可以同时允许远程访问和控制另一个设备。



> 插图：用于从此 Mac 控制另一台设备的连接设置卡。



<a id="connect-to-an-ssh-host"></a>

## 连接到 SSH 主机

在 ChatGPT 桌面应用程序中，从 SSH 主机添加远程项目并针对远程文件系统和 shell 运行聊天。远程项目聊天在远程主机上运行命令、读取文件和写入更改。

远程主机应采用与常规 SSH 访问相同的安全要求：使用受信任的密钥、遵循最小权限原则的账户，并且不开放未经身份验证的公网监听端口。

<WorkflowSteps variant="headings">

1. 将主机添加到您的 SSH 配置中，以便 Codex 可以自动发现它。

```text
   主机开发盒
     主机名 devbox.example.com
     用户你
     IdentityFile ~/.ssh/id_ed25519
```

Codex 从 `~/.ssh/config` 读取具体主机别名，使用 OpenSSH 解析它们，并忽略仅模式主机。

2. 确认您可以从运行应用程序的计算机通过 SSH 连接到主机。

```bash
   ssh devbox
```

3. 在远程主机上安装并验证 Codex。

该应用程序使用远程用户的登录 shell 通过 SSH 启动远程 Codex 应用程序服务器。确保 `codex` 命令在该 shell 中远程主机的 `PATH` 上可用。

4. 在应用程序中，打开 **设置 > 连接**，添加或启用 SSH 主机，然后选择远程项目文件夹。

</WorkflowSteps>



> 插图：与三个远程主机的连接 SSH 列表。



<a id="hand-off-a-thread-between-hosts"></a>
<a id="hand-off-a-chat-between-hosts"></a>
<a id="hand-off-a-task-between-hosts"></a>

<a id="hand-off-a-chat-between-hosts"></a>

## 移交主持人之间的聊天

切换会在本地计算机和连接的远程主机之间移动现有的聊天及其 Git 状态。使用它在本地开始工作，在远程计算机上的工作树中继续，并稍后恢复聊天。

在结束聊天之前，请连接目标主机并为该主机上的同一 Git 仓库保存项目。如果项目是仓库的子目录，请在两台主机上保存相同的子目录。 Codex 仅显示具有匹配的已保存项目的目的地。

要转交聊天：

1. 在桌面应用程序中打开聊天。
2. 在聊天页脚中，选择当前运行位置，然后选择目标主机。将远程聊天交回本地计算机时选择 **这台电脑**。
3. 查看目标和分支，然后选择 **放手**。

Codex 在目标主机上创建或重用工作树，传输聊天和 Git 状态，并将聊天切换到该主机。如果聊天正在运行，切换会在传输之前中断当前响应。

您还可以在另一个聊天中要求 Codex 将命名聊天移交给连接的主机。 Codex 无法移交发出请求的聊天，并且不支持移交到 Codex 云环境。

<a id="authentication-and-network-exposure"></a>

## 身份验证和网络暴露

远程连接使用 SSH 来启动和管理远程 Codex 应用服务器。不要直接在共享或公共网络上公开应用程序服务器传输。

如果您需要访问当前网络之外的远程计算机，请使用 VPN 或网状网络工具，而不是将应用程序服务器直接暴露到互联网。

<a id="troubleshooting"></a>

## 故障排除

<a id="you-dont-see-the-host-on-your-phone"></a>

### 您在手机上看不到主持人

确认桌面应用程序正在主机上运行，您已启用 **允许其他设备连接**，并且两台设备使用相同的 ChatGPT 帐户和工作区。如果您自 2026 年 6 月 8 日起未使用该连接，请更新这两个应用程序并再次配对设备。

<a id="remote-control-is-off-after-you-sign-back-in"></a>

### 重新登录后远程控制关闭

注销 ChatGPT 会关闭 **远程控制**，但不会删除您现有的设备配对。重新登录后，打开 **远程控制** 以恢复之前的连接状态。

如果在打开 **远程控制** 并选择 **添加** 后看到错误，请在主机上重新启动 ChatGPT 桌面应用程序，然后重试。

<a id="the-approval-request-doesnt-appear"></a>

### 未出现批准请求

在ChatGPT移动应用程序中，打开**远程**。确认手机和主机使用相同的 ChatGPT 帐户和工作区，然后再次扫描二维码或从主机重新启动设置。如果您使用 ChatGPT 工作区，请要求管理员确认他们已启用远程控制访问。

<a id="the-remote-session-disconnects"></a>

### 远程会话断开

检查主机是否进入睡眠状态、失去网络访问权限或关闭应用程序。 ChatGPT 工作时保持主机唤醒并连接。

<a id="authentication-blocks-setup"></a>

### 身份验证块设置

完成设置过程中显示的帐户或工作区身份验证提示。如果您的组织需要 SSO、多重身份验证或密钥，请先完成该流程，然后重试。如果设置仍然失败，请要求工作区管理员确认他们已启用远程控制访问。

<a id="see-also"></a>

## 另请参阅

- [ChatGPT 桌面应用程序](app.zh-CN.md)
- [特点](features.zh-CN.md)
- [ChatGPT 桌面应用程序设置](reference/settings.zh-CN.md)
- [电脑使用](computer-use.zh-CN.md)
- [Chrome 扩展程序](chrome-extension.zh-CN.md)
- [命令行选项](developer-commands.zh-CN.md)
- [认证](auth.zh-CN.md)