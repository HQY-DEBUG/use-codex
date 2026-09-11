> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/sandboxing.md)。

<a id="sandbox"></a>

# 沙箱

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

沙箱是让智能体自主行动的边界，而不会让它不受限制地访问您的机器。当本地聊天在 **ChatGPT 桌面应用程序**、**Codex CLI** 或 **IDE扩展** 中运行命令时，这些命令在受限环境中运行，而不是默认以完全访问权限运行。

该环境定义了智能体可以自行执行的操作，例如可以修改哪些文件以及命令是否可以使用网络。当任务停留在这些边界内时，智能体可以继续移动而无需停下来进行确认。当需要超出这些范围时，审批流程就会接管。

沙箱和批准是一起工作的不同控件。沙箱定义了技术边界。批准政策决定智能体在穿越它们之前何时必须停下来询问。

<a id="what-the-sandbox-does"></a>

## 沙箱有什么作用

沙箱适用于生成的命令，而不仅仅是内置文件操作。如果智能体运行 `git`、包管理器或测试运行程序等工具，这些命令将继承相同的沙箱边界。

Codex 在每个操作系统上使用平台本机强制执行。 macOS、Linux、WSL2 和本机 Windows 之间的实现有所不同，但各个平台的想法是相同的：为智能体提供一个有界的工作场所，以便例行任务可以在明确的限制内自主运行。

<a id="why-it-matters"></a>

## 为什么这很重要

沙箱减少了审批疲劳。智能体可以在您已批准的范围内读取文件、进行编辑并运行常规项目命令，而不是要求您确认每个低风险命令。

它还为您的智能体工作提供了更清晰的信任模型。您不仅相信智能体人的意图；而且还相信智能体人的意图。您相信智能体在强制限制内运行。这使得智能体可以更轻松地独立工作，同时仍然知道何时会停止并寻求帮助。

<a id="getting-started"></a>

## 开始使用

默认权限模式自动应用沙箱。

<a id="prerequisites"></a>

### 先决条件

在 **macOS** 上，沙箱使用内置 Seatbelt 框架开箱即用。

在 **窗户** 上，Codex 在 PowerShell 中运行时使用本机 [Windows沙箱](windows/windows-sandbox.zh-CN.md#windows-sandbox)，在 WSL2 中运行时使用 Linux 沙箱实现。

在 **Linux 和 WSL2** 上，首先使用包管理器安装 `bubblewrap`：

<Tabs
  id="codex-sandboxing-prerequisites"
  param="sandbox-os"
  tabs={[
    { id: "ubuntu-debian", label: "Ubuntu/Debian" },
    { id: "fedora", label: "软呢帽" },
  ]}
>
  


```bash
sudo apt install bubblewrap
```

  


  


```bash
sudo dnf install bubblewrap
```

  

</Tabs>

Codex 使用在 `PATH` 上找到的第一个 `bwrap` 可执行文件。如果没有可用的 `bwrap` 可执行文件，Codex 将回退到捆绑的帮助程序，但该帮助程序需要支持非特权用户命名空间创建。安装提供 `bwrap` 的分发包可以保持此设置的可靠性。

当 `bwrap` 丢失或帮助程序无法创建所需的用户命名空间时，Codex 会显示启动警告。在限制此 AppArmor 设置的发行版上，最好加载 `bwrap` AppArmor 配置文件，以便 `bwrap` 可以继续工作，而无需全局禁用该限制。

**Ubuntu AppArmor 注意：** 在 Ubuntu 25.04 上，从 Ubuntu 的软件包仓库安装 `bubblewrap` 应该无需额外的 AppArmor 设置即可工作。 `bwrap-userns-restrict` 配置文件以 `apparmor` 封装形式发货，包装地址为 `/etc/apparmor.d/bwrap-userns-restrict`。

在 Ubuntu 24.04 上，安装 `bubblewrap` 后，Codex 可能仍会警告其无法创建所需的用户命名空间。复制并加载额外的配置文件：

```bash
sudo apt update
sudo apt install apparmor-profiles apparmor-utils
sudo install -m 0644 \
  /usr/share/apparmor/extra-profiles/bwrap-userns-restrict \
  /etc/apparmor.d/bwrap-userns-restrict
sudo apparmor_parser -r /etc/apparmor.d/bwrap-userns-restrict
```

`apparmor_parser -r` 将配置文件加载到内核中，无需重新启动。您还可以重新加载所有 AppArmor 配置文件：

```bash
sudo systemctl reload apparmor.service
```

如果该配置文件不可用或无法解决问题，您可以通过以下方式禁用 AppArmor 非特权用户命名空间限制：

```bash
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
```

</ContentModeSwitch>

<a id="how-permissions-work"></a>

## 权限如何运作

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

使用使用界面的权限控制来更改 Codex 处理本地操作的方式。

批准确定 Codex 在执行操作之前何时暂停，而沙箱确定命令可以访问哪些文件和网络资源。当批准提供不同的范围（例如批准一次或批准一次会话）时，请选择允许任务继续的最窄范围。保持项目边界为默认；使用单独的项目或工作树，而不是扩大对不相关仓库的访问。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

ChatGPT Work 在托管的隔离环境中运行代码和 shell 命令。工作区策略和特定于工具的控制决定了哪些功能可用。当该设置可用时，使用 **设置 > 数据控制 > 工作网络访问** 管理代码和 shell 命令的网络访问。打开 **允许公共互联网访问** 让这些命令到达公共互联网。当它关闭时，命令只能访问托管白名单中所需的主机名。

Web 搜索、插件和远程浏览器具有单独的控件。更改在当前代码或 shell 运行完成并且 Work 刷新其执行环境后生效。 ChatGPT Web 不会公开本地 Codex 沙箱或审批模式选择器。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="app">

在ChatGPT桌面应用程序中，使用composer下方的权限控制。根据您的配置，菜单可以包括 **请求批准**、适用于合格审批请求的 **代我批准**、**完全访问** 以及命名或自定义权限配置文件。

<PermissionModeSelectorDemo client:load />

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="cli">

在 CLI 中，输入 [`/permissions`](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-update-permissions-with-permissions) 打开权限选择器并更改活动权限配置文件。

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="ide">

在IDE扩展中，使用composer下的权限控制。根据您的配置，菜单可以包括 **请求批准**、适用于合格审批请求的 **代我批准**、**完全访问** 以及命名或自定义权限配置文件。



  
    

> 图：IDE 扩展中的 Codex 审批模式选择器


  



</ContentModeSwitch>

<a id="configure-defaults"></a>

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

<a id="configure-defaults"></a>

## 配置默认值

要每次都以相同的行为开始，请在 `config.toml` 中设置默认值。 [配置基础知识](config-file/config-basic.zh-CN.md) 解释了它的工作原理，[配置参考](config-file/config-reference.zh-CN.md) 记录了 `sandbox_mode`、`approval_policy`、`approvals_reviewer` 和 `sandbox_workspace_write.writable_roots` 的确切密钥。使用这些设置来决定智能体默认获得多少自主权、可以写入哪些目录、何时应暂停审批以及由谁审查合格的审批请求。

从较高层面来看，常见的沙箱模式有：

- `read-only`：智能体可以检查文件，但未经批准无法编辑文件或运行命令。
- `workspace-write`：智能体可以读取文件、在工作区中进行编辑以及在该边界内运行例行本地命令。这是本地工作的默认低摩擦模式。
- `danger-full-access`：智能体运行时不受沙箱限制。这消除了文件系统和网络边界，并且仅当您希望智能体以完全访问权限运行时才应使用。

常见的审批政策有：

- `on-request`：智能体默认在沙箱内工作，并询问何时需要超越该边界。
- `never`：智能体不会因批准提示而停止。

Codex 和 ChatGPT Work 不再支持 `untrusted` 作为可选审批策略。如果现有配置使用该值，请参阅 [从已停用的 `untrusted` 审批策略迁移](agent-approvals-security.zh-CN.md#migrate-from-the-retired-untrusted-approval-policy)。

当批准是交互式的时，您还可以选择使用 `approvals_reviewer` 进行审核的人员：

- `user`：向用户显示批准提示。这是默认设置。
- `auto_review`：符合条件的批准提示将发送至审核智能体（请参阅 [自动审核](sandboxing/auto-review.zh-CN.md)）。

完全访问意味着将 `sandbox_mode = "danger-full-access"` 与 `approval_policy = "never"` 一起使用。相比之下，风险较低的本地自动化预设是 `sandbox_mode = "workspace-write"` 和 `approval_policy = "on-request"`，或匹配的 CLI 标志 `--sandbox workspace-write --ask-for-approval on-request`。然后，您可以保留 `approvals_reviewer = "user"` 进行手动审批，或设置 `approvals_reviewer = "auto_review"` 进行自动审批审核。

如果您需要智能体在多个目录上工作，可写根允许您扩展它可以修改的位置，而无需完全删除沙箱。如果您需要更宽或更窄的信任边界，请调整默认沙箱模式和审批策略，而不是依赖一次性例外。

当工作流需要特定异常时，请使用 [规则](agent-configuration/rules.zh-CN.md)。规则允许您允许、提示或禁止沙箱外部的命令前缀，这通常比广泛扩展访问更合适。有关 IDE 特定设置入口点，请参阅 [Codex IDE扩展设置](developer-settings.zh-CN.md)。

自动审查（如果可用）不会更改沙箱边界。这是一种可能的 `approvals_reviewer`，用于该边界的批准请求，例如沙箱升级、阻止网络访问或仍需要批准的副作用工具调用。沙箱内已允许的操作无需额外审查即可运行。有关审阅者生命周期、触发器类型、拒绝语义和配置详细信息，请参阅 [自动审核](sandboxing/auto-review.zh-CN.md)。

平台详细信息位于特定于平台的文档中。有关本机 Windows 设置、行为和故障排除，请参阅 [窗户](windows/windows-sandbox.zh-CN.md)。有关沙箱和审批的管理要求和组织级限制，请参阅 [智能体审批和安全](agent-approvals-security.zh-CN.md)。

</ContentModeSwitch>