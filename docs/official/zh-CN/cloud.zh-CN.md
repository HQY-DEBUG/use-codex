> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/cloud.md)。

<a id="codex-cloud"></a>

# Codex 云端

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="run-coding-tasks-in-parallel-cloud-environments"></a>

## 在并行云环境中运行编码任务

在隔离的云环境中运行任务，并行工作，并从 Web、GitHub、GitLab、Linear 或 Slack 开始工作。

> 插图：Codex 云聊天编辑器和带有交互式存档的聊天列表

<a id="start-here"></a>

### 从这里开始

- [打开Codex云](https://chatgpt.com/codex)
- [设置Codex云](#getting-started)

<a id="why-use-codex-cloud"></a>

### 为什么使用Codex云

- **并行运行工作：** 为较长的任务提供专用环境，让它们在您处理其他事情时继续进行。
- **重现环境：** 配置每个仓库所需的依赖项、工具、变量和设置步骤。
- **合并前检查：** 检查摘要和差异、请求跟进或在结果准备好时打开拉取请求。

<a id="getting-started"></a>

## 开始使用

**设置Codex云。**

连接GitHub或GitLab，创建环境，开始您的第一次云聊天。

<a id="1-open-codex-and-sign-in"></a>

### 1.打开Codex并登录

转到 [Codex](https://chatgpt.com/codex) 并使用您的 ChatGPT 帐户登录。

<a id="2-connect-github-or-gitlab"></a>

### 2.连接GitHub或GitLab

出现提示时连接 GitHub 或 GitLab（测试版）。对于GitHub，选择Codex可以访问的仓库；对于GitLab，创建环境时选择项目。有关 GitLab 设置、Webhook 权限和合并请求审核，请参阅 [将 Codex 与 GitLab（测试版）结合使用](third-party/gitlab.zh-CN.md)。

<a id="3-create-an-environment"></a>

### 3. 创造环境

打开 [环境设置](https://chatgpt.com/codex/settings/environments) 并为您选择的仓库创建环境。配置任务所需的任何依赖项、工具、环境变量或机密。

详细配置请参见[云环境](environments/cloud-environment.zh-CN.md)。

<a id="4-start-your-first-task"></a>

### 4. 开始你的第一个任务

返回[Codex](https://chatgpt.com/codex)，选择您的环境，并描述您想要的结果。您可以查看任务日志或让任务在后台运行。

<a id="5-review-the-result"></a>

### 5. 检查结果

查看摘要和差异。要求 Codex 进行后续更改，或在工作准备就绪时打开拉取请求。

<a id="next-steps"></a>

### 后续步骤

- [定制云环境](environments/cloud-environment.zh-CN.md)
- [配置智能体互联网访问](cloud/internet-access.zh-CN.md)
- [将 Codex 与 GitHub 配合使用](third-party/github.zh-CN.md)
- [将 Codex 与 GitLab（测试版）结合使用](third-party/gitlab.zh-CN.md)
- [在线性中使用 Codex](third-party/linear.zh-CN.md)
- [在Slack中使用Codex](third-party/slack.zh-CN.md)

<a id="see-what-codex-cloud-can-do"></a>

## 看看Codex云能做什么

为每项任务提供所需的环境，然后按计划查看结果。

- [委派多项任务](environments/cloud-environment.zh-CN.md)：并行开始工作，并在每项任务达到可审查结果时返回。
- [构建可重现的环境](environments/cloud-environment.zh-CN.md)：配置仓库所需的依赖项、工具、变量和设置步骤。
- [来自您的集成的委托](developers.zh-CN.md)：从 GitHub 拉取请求、GitLab 合并请求和问题、线性问题或 Slack 通道和线程开始在 Codex 云中工作。

<a id="use-codex-cloud-when"></a>

## 在以下情况下使用 Codex 云...

- [工作需要在后台运行](environments/cloud-environment.zh-CN.md)：委派较长的任务并在准备好时返回。
- [您想要比较几次尝试](environments/cloud-environment.zh-CN.md)：并行运行任务，而无需占用本地计算机。
- [工作从 GitHub、GitLab、Linear 或 Slack 开始](developers.zh-CN.md)：使用集成来移交工作，而无需留下拉取请求、合并请求、问题、通道或线程。
- [您远离您的开发机器](environments/cloud-environment.zh-CN.md)：从 Web 或 Codex CLI 开始和检查工作。