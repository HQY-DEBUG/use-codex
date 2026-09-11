> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/environments/cloud-environment.md)。

<a id="cloud-environments"></a>

# 云端环境配置

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用环境来控制 Codex 在云聊天期间安装和运行的内容。例如，您可以添加依赖项、安装 linter 和格式化程序等工具以及设置环境变量。

在[Codex设置](https://chatgpt.com/codex/settings/environments)中配置环境。

<a id="how-codex-cloud-tasks-run"></a>

<a id="how-codex-cloud-chats-run"></a>

## Codex云聊天如何运行

以下是您提交提示时会发生的情况：

1. Codex 创建一个容器并在选定的分支或提交 SHA 处签出您的仓库。
2. 当缓存的容器恢复时，Codex 运行您的设置脚本以及可选的维护脚本。
3. Codex 应用您的互联网访问设置。安装脚本通过 Internet 访问运行。默认情况下，智能体 Internet 访问处于关闭状态，但您可以根据需要启用有限或无限制的访问。参见 [智能体互联网接入](../cloud/internet-access.zh-CN.md)。
4. 智能体循环运行终端命令。它编辑代码、运行检查并尝试验证其工作。如果您的仓库包含 `AGENTS.md`，智能体将使用它来查找项目特定的 lint 和测试命令。
5. 当智能体完成时，它会显示其答案以及它更改的任何文件的差异。您可以打开 PR 或提出后续问题。

<a id="default-universal-image"></a>

## 默认通用镜像

Codex 智能体在名为 `universal` 的默认容器映像中运行，该映像预装了通用语言、软件包和工具。

在环境设置中，选择 **设置包版本** 以固定 Python、Node.js 和其他运行时的版本。

有关已安装内容的详细信息，请参阅 [openai/通用法典](https://github.com/openai/codex-universal) 以获取参考 Dockerfile 以及可以在本地拉取和测试的映像。

虽然 `codex-universal` 预装了语言以提高速度和便利性，但您还可以使用 [设置脚本](#manual-setup) 将其他软件包安装到容器中。

<a id="environment-variables-and-secrets"></a>

## 环境变量和秘密

**环境变量** 设置为整个聊天期间（包括设置脚本和智能体阶段）。

**秘密** 与环境变量类似，不同之处在于：

- 它们以额外的加密层存储，并且仅在任务执行时解密。
- 它们仅可用于设置脚本。出于安全原因，在智能体阶段开始之前会删除机密。

<a id="automatic-setup"></a>

## 自动设置

对于使用通用包管理器（`npm`、`yarn`、`pnpm`、`pip`、`pipenv` 和 `poetry`）的项目，Codex 可以自动安装依赖项和工具。

<a id="manual-setup"></a>

## 手动设置

如果您的开发设置更复杂，您还可以提供自定义设置脚本。例如：

```bash
# 安装类型检查器
pip install pyright

# 安装依赖项
poetry install --with test
pnpm install
```

安装脚本在与智能体不同的 Bash 会话中运行，因此 `export` 之类的命令不会持续到智能体阶段。要保留环境变量，请将它们添加到 `~/.bashrc` 或在环境设置中配置它们。

<a id="container-caching"></a>

## 容器缓存

Codex 将容器状态缓存长达 12 小时，以加快新的聊天和跟进速度。

当环境被缓存时：

- Codex 克隆仓库并签出默认分支。
- Codex 运行设置脚本并缓存结果容器状态。

当缓存的容器恢复时：

- Codex 检查为聊天指定的分支。
- Codex 运行维护脚本（可选）。当安装脚本在较旧的提交上运行并且依赖项需要更新时，这非常有用。

如果更改设置脚本、维护脚本、环境变量或机密，Codex 会自动使缓存失效。如果您的仓库更改导致缓存状态不兼容，请在环境页面上选择 **重置缓存**。

对于商业和企业用户，缓存在有权访问该环境的所有用户之间共享。使缓存失效将影响工作区中环境的所有用户。

<a id="internet-access-and-network-proxy"></a>

## 互联网接入和网络代理

在安装脚本阶段可以访问 Internet 以安装依赖项。在智能体阶段，Internet 访问默认关闭，但您可以配置受限或不受限的访问。参见 [智能体互联网接入](../cloud/internet-access.zh-CN.md)。

环境在 HTTP/HTTPS 网络代理后面运行，以实现安全和防止滥用目的。所有出站互联网流量都通过此代理。