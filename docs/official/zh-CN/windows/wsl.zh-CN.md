> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/windows/wsl.md)。

<a id="wsl"></a>

# WSL 环境

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

当您使用 WSL2 时，Codex 在 Linux 环境中运行，而不是使用本机 [Windows沙箱](windows-sandbox.zh-CN.md)。当您需要 Linux 本机工具、您的仓库和开发人员工作流程已存在于 WSL2 中，或者本机 Windows 沙箱模式都不适合您的环境时，请选择 WSL2。

WSL1 通过 Codex `0.114` 支持。从 Codex `0.115` 开始，Linux 沙箱移至 `bubblewrap`，因此不再支持 WSL1。

<a id="launch-vs-code-from-inside-wsl"></a>

## 从 WSL 内部启动 VS Code

有关分步说明，请参阅 [官方 VS Code WSL 教程](https://code.visualstudio.com/docs/remote/wsl-tutorial)。

<a id="prerequisites"></a>

### 先决条件

- 安装了 WSL 的 Windows。要安装 WSL，请以管理员身份打开 PowerShell，然后运行 ​​`wsl --install`（Ubuntu 是常见选择）。
- 安装了 [WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 的 VS Code。

<a id="open-vs-code-from-a-wsl-terminal"></a>

### 从 WSL 终端打开 VS Code

```bash
# 从您的 WSL shell
cd ~/code/your-project
code .
```

这将打开 WSL 远程窗口，根据需要安装 VS Code 服务器，并确保集成终端在 Linux 中运行。

<a id="confirm-youre-connected-to-wsl"></a>

### 确认您已连接到 WSL

- 查找显示“WSL:”的绿色状态栏<distro>`.
- 集成终端应显示Linux路径（例如`/home/...`）而不是`C:\`。
- 您可以通过以下方式验证：

```bash
  echo $WSL_DISTRO_NAME
```

这将打印您的发行版名称。

如果在状态栏中没有看到“WSL：...”，请按 `Ctrl+Shift+P`，选择 `WSL: Reopen Folder in WSL`，并将仓库保留在 `/home/...`（而不是 `C:\`）下，以获得最佳性能。

如果 Windows 应用程序或项目选择器未显示您的 WSL 仓库，请在文件选择器或资源管理器中键入 `\\wsl$`，然后导航到发行版的主目录。

<a id="use-codex-cli-with-wsl"></a>

## 将 Codex CLI 与 WSL 结合使用

从提升的 PowerShell 或 Windows 终端运行这些命令：

```powershell
# 安装默认的 Linux 发行版（如 Ubuntu）
wsl --install

# 在适用于 Linux 的 Windows 子系统中启动 shell
wsl
```

然后从 WSL shell 运行这些命令：

```bash
# 在 WSL 中安装并运行 Codex
curl -fsSL https://chatgpt.com/codex/install.sh | sh
codex
```

<a id="work-on-code-inside-wsl"></a>

## 在 WSL 中处理代码

- 在 `/mnt/c/...` 这样的 Windows 安装路径中工作可能比在 Windows 本机路径中工作慢。将仓库保存在 Linux 主目录下（例如 `~/code/my-app`），以加快 I/O 速度并减少符号链接和权限问题：
```bash
  mkdir -p ~/code && cd ~/code
  git clone https://github.com/your/repo.git
  cd repo
```
- 如果您需要 Windows 访问文件，它们位于资源管理器中的 `\\wsl$\Ubuntu\home\&lt;user&gt;` 下。

<a id="troubleshooting-and-faq"></a>

## 故障排除和常见问题解答



<a id="large-repositories-feel-slow-in-wsl"></a>

### 大型仓库在 WSL 中感觉很慢



- 确保您不在 `/mnt/c` 下工作。将仓库移至 WSL（例如，`~/code/...`）。
- 如果需要，增加 WSL 的内存和 CPU；将 WSL 更新到最新版本：
```powershell
  wsl --update
  wsl --shutdown
```







<a id="vs-code-in-wsl-cannot-find-codex"></a>

### WSL 中的 VS Code 找不到 codex



验证二进制文件是否存在并且位于 WSL 内的 `PATH` 上：

```bash
which codex || echo "codex not found"
```

如果未找到二进制文件，请按照 [Codex CLI 设置说明](#use-codex-cli-with-wsl) 进行操作。