> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/linux/linux-app.md)。

<a id="chatgpt-desktop-app-for-linux"></a>

# Linux 桌面应用

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

适用于 Linux 的 ChatGPT 桌面应用程序现已推出预览版。安装适用于您的 Linux 发行版和处理器架构的软件包，然后使用您的 ChatGPT 帐户登录以处理项目、本地文件和 Codex。

<a id="supported-distributions-and-architectures"></a>

## 支持的发行版和架构

该预览版支持以下 Linux 发行版的桌面版本：

- Ubuntu 24.04 LTS 和 26.04 LTS
- Debian 13
- Fedora 43 和 44

每个受支持的发行版都有适用于 x64 和 ARM64 处理器的软件包。要检查您的处理器架构，请运行：

```bash
uname -m
```

输出 `x86_64` 标识 x64 处理器。输出 `aarch64` 或 `arm64` 标识 ARM64 处理器。

<a id="download-the-right-package"></a>

## 下载正确的包

对于 Ubuntu 或 Debian 选择 `.deb`，对于 Fedora 选择 `.rpm`：

| 发行版 | 架构 | 下载 |
| ---------------- | ------------ | ----------------------------------------------------------------------------------------------------------------- |
| Ubuntu 或 Debian | x64 | [下载 x64 版 `.deb`](https://persistent.oaistatic.com/codex-app-prod/linux/deb/latest/chatgpt_amd64.deb) |
| Ubuntu 或 Debian | ARM64 | [下载适用于 ARM64 的 `.deb`](https://persistent.oaistatic.com/codex-app-prod/linux/deb/latest/chatgpt_arm64.deb) |
| Fedora | x64 | [下载 x64 版 `.rpm`](https://persistent.oaistatic.com/codex-app-prod/linux/rpm/latest/chatgpt.x86_64.rpm) |
| Fedora | ARM64 | [下载适用于 ARM64 的 `.rpm`](https://persistent.oaistatic.com/codex-app-prod/linux/rpm/latest/chatgpt.aarch64.rpm) |

<a id="install-on-ubuntu-or-debian"></a>

## 在 Ubuntu 或 Debian 上安装

下载适合您的处理器架构的 `.deb` 软件包。然后打开终端，切换到包含该包的目录，并使用 `apt` 安装它：

```bash
cd ~/Downloads
sudo apt install ./chatgpt_amd64.deb
```

对于 ARM64，将 `chatgpt_amd64.deb` 替换为 `chatgpt_arm64.deb`。

从应用程序菜单中打开 **ChatGPT**，或在终端中运行 `chatgpt`。使用您的 ChatGPT 帐户登录并关注 [桌面应用程序快速入门](../quickstart.zh-CN.md)。

<a id="install-on-fedora"></a>

## 在 Fedora 上安装

下载适合您的处理器架构的 `.rpm` 软件包。然后打开终端，切换到包含该包的目录，并使用 `dnf` 安装它：

```bash
cd ~/Downloads
sudo dnf install ./chatgpt.x86_64.rpm
```

对于 ARM64，将 `chatgpt.x86_64.rpm` 替换为 `chatgpt.aarch64.rpm`。

从应用程序菜单中打开 **ChatGPT**，或在终端中运行 `chatgpt`。使用您的 ChatGPT 帐户登录并关注 [桌面应用程序快速入门](../quickstart.zh-CN.md)。

<a id="update-the-app"></a>

## 更新应用程序

该软件包在安装过程中配置签名的 OpenAI 软件包仓库。使用您的发行版的包管理器来安装以后的更新。

在 Ubuntu 或 Debian 上，运行：

```bash
sudo apt update
sudo apt install --only-upgrade chatgpt
```

在 Fedora 上，运行：

```bash
sudo dnf upgrade --refresh chatgpt
```

<a id="compatibility-and-limitations"></a>

## 兼容性和限制

该预览版支持 [支持的发行版和架构](#supported-distributions-and-architectures) 中列出的桌面发行版。其他 Linux 发行版可能可以工作，但不受正式支持。

某些功能有单独的平台要求。例如，[电脑使用](../computer-use.zh-CN.md) 可在 macOS 和 Windows 上使用，但尚未在 Linux 预览版中使用。未来的版本将添加 Linux 支持。

<a id="wayland-support"></a>

## 韦兰支持

Native Wayland 支持处于实验阶段，并将继续改进。在 Wayland 会话中，应用程序会使用可用的 XWayland。要显式选择本机 Wayland，请完全退出应用程序并从终端启动它：

```bash
chatgpt --ozone-platform=wayland
```

当原生 Wayland 支持成熟时，某些功能（例如浮动窗口、窗口定位、焦点和键盘快捷键）可能无法完全运行。

<a id="next-steps"></a>

## 后续步骤

- 遵循 [桌面应用程序快速入门](../quickstart.zh-CN.md)。
- 设置 [Chrome 扩展程序](../chrome-extension.zh-CN.md) 以进行浏览器集成。
- 查看 [权限](../permissions.zh-CN.md) 以了解本地项目和命令。