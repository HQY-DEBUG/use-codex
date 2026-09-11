# 文档用途总览

这里逐篇说明仓库中 **148 篇英文文档及其对应中文译文** 的用途。路径以 `docs/official/en/` 为基准；中文版本位于 `docs/official/zh-CN/`，保留相同子目录，文件名增加 `.zh-CN`。

点击“中文”阅读译文，点击文件路径对照英文。四篇基础译文沿用此前版本，其余 144 篇为机器翻译并经过术语和格式抽查，未逐句人工校订。程序代码、命令、标识符、路径和配置值保留原样；说明文字及可识别的代码注释译为中文。

## 如何开始

- 初次学习：快速开始 → 提示词 → 个性化 → 技能与插件 → 权限模式。
- Windows 本地使用：Windows 桌面应用 → 项目与对话 → Windows 沙箱；需要 Linux 工具时再读 WSL。
- 配置开发环境：配置基础 → AGENTS.md → 配置项参考；高级配置按需查阅。
- 编写集成程序：CLI → 非交互模式 → SDK 或 app-server。
- 管理企业工作区：企业部署指南 → 角色与工作区权限 → 受管配置。
- 使用安全扫描：Codex Security 概览 → 插件或 CLI 快速开始 → 扫描与修复流程。

## 容易混淆的文件

| 文件 | 区别 |
| --- | --- |
| `permission-modes.md` / `permissions.md` | 前者介绍界面上的权限模式；后者是权限配置技术参考。 |
| `reference/settings.md` / `developer-settings.md` | 前者介绍日常应用偏好；后者介绍开发设置及本地配置关系。 |
| `reference/commands.md` / `developer-commands.md` | 前者侧重桌面导航快捷键；后者侧重开发命令、CLI 参数和交互操作。 |
| `security-administration.md` / `security.md` | 前者是访问控制与隔离的安全管理入口；后者介绍查找和修复漏洞的 Codex Security 产品。 |
| `sandboxing.md` / `sandboxing/auto-review.md` | 前者介绍执行隔离边界；后者介绍跨越边界时的自动审核机制。 |
| `codex-manual.md` / 其他独立文档 | 综合手册按主题汇总内容，与独立文档有重叠；保留用于集中检索。 |

## 仓库辅助文件

| 文件或目录 | 作用 |
| --- | --- |
| [根 README](../../README.md) | 仓库入口、目录结构、阅读顺序和资料说明。 |
| [本文件 README](README.md) | 逐篇解释文档用途，提供全部中英文链接。 |
| [中文学习指南](learning-guide-zh-CN.md) | 独立编写的基础知识摘要、练习和 Git 入门。 |
| [英文学习指南](learning-guide-en.md) | 中文学习指南对应的英文版。 |
| [英文索引](en/INDEX.md) | 按官方标题列出英文原文及在线来源。 |
| [中文索引](zh-CN/INDEX.md) | 按主题列出全部中文译文，并链接对应英文文件。 |
| [.gitattributes](../../.gitattributes) | 设置换行符处理；英文归档文件不做 Git 文本转换。 |
| [.gitignore](../../.gitignore) | 忽略系统临时文件、本地编辑器配置、Python 缓存与虚拟环境，以及本地环境变量文件；保留示例模板。 |
| `.git/` | Git 内部版本数据，无须逐个阅读或手动修改。 |

## 01 基础入门与综合参考

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [codex-manual.md](en/codex-manual.md) | [Codex 综合手册](zh-CN/codex-manual.zh-CN.md) | 按主题汇总多个功能与技术参考；篇幅较大，与独立文档有内容重叠，适合全文检索。 |
| [feature-maturity.md](en/feature-maturity.md) | [功能成熟度](zh-CN/feature-maturity.zh-CN.md) | 解释实验、预览等成熟度标记及其稳定性和支持预期。 |
| [features.md](en/features.md) | [功能总览](zh-CN/features.zh-CN.md) | 按工作流程、能力、命令和设置介绍可用功能。 |
| [get-started-with-work.md](en/get-started-with-work.md) | [ChatGPT Work 入门](zh-CN/get-started-with-work.zh-CN.md) | 介绍何时使用 Work，以及如何开始和审阅多步骤工作。 |
| [glossary.md](en/glossary.md) | [术语表](zh-CN/glossary.zh-CN.md) | 解释跨桌面端、CLI、IDE、云端及 SDK 的核心术语。 |
| [permission-modes.md](en/permission-modes.md) | [权限模式操作指南](zh-CN/permission-modes.zh-CN.md) | 介绍界面上的请求批准、自动审核与完全访问模式，以及如何启用和选择。 |
| [personalize.md](en/personalize.md) | [个性化 ChatGPT](zh-CN/personalize.zh-CN.md) | 介绍个性、自定义指令、记忆及 Computer History 等个性化功能。 |
| [pricing.md](en/pricing.md) | [价格与额度](zh-CN/pricing.zh-CN.md) | 记录资料快照中的套餐、计费、额度及用量规则，查询当前价格时需对照官网。 |
| [prompting.md](en/prompting.md) | [提示词](zh-CN/prompting.zh-CN.md) | 介绍目标、上下文、输出和边界的写法，并提供日常及编码任务示例。 |
| [quickstart.md](en/quickstart.md) | [快速开始](zh-CN/quickstart.zh-CN.md) | 帮助选择网页、桌面端、CLI 等入口并开始首个任务。 |
| [skills-and-plugins.md](en/skills-and-plugins.md) | [技能与插件基础](zh-CN/skills-and-plugins.zh-CN.md) | 解释技能与插件的区别、适用场景，以及创建和复用工作流程的方式。 |
| [use-chatgpt.md](en/use-chatgpt.md) | [使用 ChatGPT](zh-CN/use-chatgpt.zh-CN.md) | 介绍用自然语言提出任务、提供上下文并改进结果的基本方法。 |
| [whats-new.md](en/whats-new.md) | [近期功能更新](zh-CN/whats-new.zh-CN.md) | 按周记录功能变化、使用示例和延伸阅读，内容对应本地资料快照。 |

## 02 使用界面、运行环境与日常设置

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [app.md](en/app.md) | [ChatGPT 桌面应用](zh-CN/app.zh-CN.md) | 介绍桌面端的项目、文件、并行任务与持续工作入口。 |
| [cloud.md](en/cloud.md) | [Codex 云端](zh-CN/cloud.zh-CN.md) | 介绍在隔离云端环境执行编码任务，以及从网页和协作工具启动工作的入口。 |
| [cloud/internet-access.md](en/cloud/internet-access.md) | [云端互联网访问](zh-CN/cloud/internet-access.zh-CN.md) | 说明智能体阶段的网络开关、访问范围以及启用网络后的风险。 |
| [code-review.md](en/code-review.md) | [代码审查](zh-CN/code-review.zh-CN.md) | 说明如何在提交或推送前审查本地代码修改并处理反馈。 |
| [codex/cli.md](en/codex/cli.md) | [Codex CLI 入门](zh-CN/codex/cli.zh-CN.md) | 介绍在终端中检查、编辑和运行代码的基本工作流程。 |
| [codex/ide.md](en/codex/ide.md) | [Codex IDE 扩展](zh-CN/codex/ide.zh-CN.md) | 介绍利用编辑器中的文件和选中代码开展任务、审阅修改及转交工作。 |
| [environments/cloud-environment.md](en/environments/cloud-environment.md) | [云端环境配置](zh-CN/environments/cloud-environment.zh-CN.md) | 说明如何配置云端任务依赖、安装工具、设置脚本和环境变量。 |
| [environments/git-worktrees.md](en/environments/git-worktrees.md) | [Git 工作树](zh-CN/environments/git-worktrees.zh-CN.md) | 介绍在同一项目中隔离多个任务，以及工作树的使用和管理。 |
| [environments/local-environment.md](en/environments/local-environment.md) | [本地环境配置](zh-CN/environments/local-environment.zh-CN.md) | 说明如何为工作树设置初始化步骤和项目常用操作。 |
| [environments/modes.md](en/environments/modes.md) | [运行环境选择](zh-CN/environments/modes.zh-CN.md) | 解释本地、工作树和云端等任务执行位置的选择。 |
| [features/codex-micro.md](en/features/codex-micro.md) | [Codex Micro 硬件](zh-CN/features/codex-micro.zh-CN.md) | 介绍专用键盘与任务状态、语音、常用操作和技能的交互。 |
| [features/voice.md](en/features/voice.md) | [ChatGPT 语音](zh-CN/features/voice.zh-CN.md) | 介绍在桌面端通过语音交流想法、启动工作和调整任务。 |
| [integrated-terminal.md](en/integrated-terminal.md) | [集成终端](zh-CN/integrated-terminal.zh-CN.md) | 介绍对话内终端的打开方式、项目范围和使用方法。 |
| [linux/linux-app.md](en/linux/linux-app.md) | [Linux 桌面应用](zh-CN/linux/linux-app.zh-CN.md) | 说明预览版在不同发行版和处理器架构上的安装与使用。 |
| [projects.md](en/projects.md) | [项目与对话](zh-CN/projects.zh-CN.md) | 说明如何组织相关对话、共享项目上下文并管理不同项目类型。 |
| [reference/commands.md](en/reference/commands.md) | [桌面命令与快捷键](zh-CN/reference/commands.zh-CN.md) | 查询日常导航、视图和任务操作的命令及平台快捷键。 |
| [reference/settings.md](en/reference/settings.md) | [桌面应用设置](zh-CN/reference/settings.zh-CN.md) | 查询个性化和日常偏好设置，以及打开设置的方法。 |
| [reference/troubleshooting.md](en/reference/troubleshooting.md) | [故障排查](zh-CN/reference/troubleshooting.zh-CN.md) | 汇总常见问题的解释与处理方式。 |
| [remote-connections.md](en/remote-connections.md) | [远程连接](zh-CN/remote-connections.zh-CN.md) | 说明如何从其他设备访问和继续运行在已连接电脑上的工作。 |
| [remote.md](en/remote.md) | [Codex 远程使用](zh-CN/remote.zh-CN.md) | 介绍通过手机启动、指导、审批和审阅运行在电脑上的编码任务。 |
| [web.md](en/web.md) | [ChatGPT 网页版](zh-CN/web.zh-CN.md) | 介绍在浏览器中研究、分析、处理文件和开展多步骤任务。 |
| [windows/windows-app.md](en/windows/windows-app.md) | [Windows 桌面应用](zh-CN/windows/windows-app.zh-CN.md) | 介绍 Windows 客户端的安装及项目、任务和审阅能力。 |
| [windows/windows-sandbox.md](en/windows/windows-sandbox.md) | [Windows 沙箱](zh-CN/windows/windows-sandbox.zh-CN.md) | 解释原生 Windows 下桌面端、CLI 和 IDE 的隔离与权限机制。 |
| [windows/wsl.md](en/windows/wsl.md) | [WSL 环境](zh-CN/windows/wsl.zh-CN.md) | 说明何时选择 WSL2，以及如何在 Linux 环境中使用 Codex。 |

## 03 文件、浏览器与工作能力

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [appshots.md](en/appshots.md) | [应用窗口快照](zh-CN/appshots.zh-CN.md) | 说明如何把当前应用窗口发送到对话中，提供当前工作的视觉上下文。 |
| [artifacts-viewer.md](en/artifacts-viewer.md) | [文件处理与预览](zh-CN/artifacts-viewer.zh-CN.md) | 说明如何提出文件交付要求，以及预览、审阅和处理生成的文件。 |
| [automations.md](en/automations.md) | [定时任务](zh-CN/automations.zh-CN.md) | 说明如何安排重复执行的任务、管理计划并查看运行结果。 |
| [browser.md](en/browser.md) | [内置浏览器](zh-CN/browser.zh-CN.md) | 介绍桌面应用中的浏览器能力、使用流程与权限控制。 |
| [chrome-extension.md](en/chrome-extension.md) | [浏览器扩展](zh-CN/chrome-extension.zh-CN.md) | 说明如何通过浏览器扩展使用已登录的网站及其相关权限。 |
| [computer-use.md](en/computer-use.md) | [电脑操作](zh-CN/computer-use.zh-CN.md) | 介绍通过视觉界面操作应用的能力、系统授权和适用平台。 |
| [image-generation.md](en/image-generation.md) | [图像生成](zh-CN/image-generation.zh-CN.md) | 说明如何生成和编辑图像，并将图像用于设计素材和开发任务。 |
| [image-inputs.md](en/image-inputs.md) | [图像输入](zh-CN/image-inputs.zh-CN.md) | 说明如何附加截图、图表或设计稿，并提供清晰的视觉任务要求。 |
| [long-running-work.md](en/long-running-work.md) | [长时间任务](zh-CN/long-running-work.zh-CN.md) | 介绍持续推进多步骤工作的目标、约束、规划和完成标准。 |
| [notifications.md](en/notifications.md) | [通知](zh-CN/notifications.zh-CN.md) | 说明工作完成或需要处理时，各界面的通知方式与设置。 |
| [pets.md](en/pets.md) | [桌面宠物](zh-CN/pets.zh-CN.md) | 说明动画伙伴的选择、显示位置和任务状态提示。 |
| [sites.md](en/sites.md) | [Sites 网站工具](zh-CN/sites.zh-CN.md) | 说明在 ChatGPT 中创建、保存和发布交互网站或应用的工作流程。 |
| [visualizations.md](en/visualizations.md) | [可视化](zh-CN/visualizations.zh-CN.md) | 介绍通过图表、地图、计算器和模拟等交互形式解释信息。 |
| [web-search.md](en/web-search.md) | [网页搜索](zh-CN/web-search.zh-CN.md) | 介绍搜索能力、信息来源以及对网页内容的处理要求。 |
| [webmcp.md](en/webmcp.md) | [网站工具与 WebMCP](zh-CN/webmcp.zh-CN.md) | 介绍网站如何通过 WebMCP 向智能体提供可调用的操作。 |

## 04 个性化、技能、插件与扩展

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [agent-configuration/agents-md.md](en/agent-configuration/agents-md.md) | [AGENTS.md 自定义指令](zh-CN/agent-configuration/agents-md.zh-CN.md) | 解释全局和项目指令的发现顺序、作用范围与覆盖关系。 |
| [agent-configuration/rules.md](en/agent-configuration/rules.md) | [命令规则](zh-CN/agent-configuration/rules.zh-CN.md) | 说明如何创建规则，控制哪些命令可以在沙箱之外运行。 |
| [agent-configuration/speed.md](en/agent-configuration/speed.md) | [运行速度](zh-CN/agent-configuration/speed.zh-CN.md) | 介绍影响任务速度的选项，以及速度与用量之间的关系。 |
| [agent-configuration/subagents.md](en/agent-configuration/subagents.md) | [子智能体](zh-CN/agent-configuration/subagents.zh-CN.md) | 介绍如何拆分并行子任务、配置子智能体并汇总结果。 |
| [build-plugins.md](en/build-plugins.md) | [创建插件入口](zh-CN/build-plugins.zh-CN.md) | 指向插件创建、打包及提交所需的官方开发文档。 |
| [build-skills.md](en/build-skills.md) | [创建技能](zh-CN/build-skills.zh-CN.md) | 说明技能目录与指令结构，以及如何创建、测试和使用可复用技能。 |
| [cli-customization.md](en/cli-customization.md) | [命令行界面定制](zh-CN/cli-customization.zh-CN.md) | 介绍终端主题、语法高亮和命令输入等 CLI 使用偏好。 |
| [custom-prompts.md](en/custom-prompts.md) | [旧版自定义提示词](zh-CN/custom-prompts.zh-CN.md) | 记录已弃用的自定义提示词机制，并引导迁移到技能。 |
| [customization/computer-history.md](en/customization/computer-history.md) | [电脑历史记录](zh-CN/customization/computer-history.zh-CN.md) | 介绍 macOS 活动记录、记忆生成及暂停、排除和删除等控制。 |
| [customization/memories.md](en/customization/memories.md) | [记忆](zh-CN/customization/memories.zh-CN.md) | 解释如何保留跨对话的偏好和上下文，以及查看与管理记忆。 |
| [customization/overview.md](en/customization/overview.md) | [定制功能概览](zh-CN/customization/overview.zh-CN.md) | 汇总个性化、记忆和其他改变工作方式的定制入口。 |
| [extend/mcp.md](en/extend/mcp.md) | [模型上下文协议](zh-CN/extend/mcp.zh-CN.md) | 介绍通过 MCP 连接外部工具、数据和开发上下文。 |
| [extend/record-and-replay.md](en/extend/record-and-replay.md) | [录制与回放](zh-CN/extend/record-and-replay.zh-CN.md) | 说明如何在 macOS 上演示工作流程并转化为可复用技能。 |
| [hooks.md](en/hooks.md) | [生命周期钩子](zh-CN/hooks.zh-CN.md) | 介绍在智能体执行过程中的特定事件触发脚本或 MCP 工具的方法。 |
| [import.md](en/import.md) | [从其他智能体导入](zh-CN/import.zh-CN.md) | 介绍如何导入其他工具中的指令、设置、技能、插件、项目和近期工作。 |
| [plugins.md](en/plugins.md) | [插件](zh-CN/plugins.zh-CN.md) | 介绍插件目录、安装、调用、技能和 MCP 工具之间的关系。 |

## 05 配置、模型、权限与沙箱

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [agent-approvals-security.md](en/agent-approvals-security.md) | [智能体审批与安全](zh-CN/agent-approvals-security.zh-CN.md) | 解释沙箱、审批、网络访问和受保护路径，帮助理解本地执行的安全机制。 |
| [auth.md](en/auth.md) | [身份验证](zh-CN/auth.zh-CN.md) | 介绍 ChatGPT 登录、API 密钥及不同使用界面的认证方式。 |
| [config-file/config-advanced.md](en/config-file/config-advanced.md) | [高级配置](zh-CN/config-file/config-advanced.zh-CN.md) | 说明模型提供方、配置档、策略和集成等进阶选项。 |
| [config-file/config-basic.md](en/config-file/config-basic.md) | [配置基础](zh-CN/config-file/config-basic.zh-CN.md) | 介绍 config.toml 的位置、配置来源、项目覆盖和信任要求。 |
| [config-file/config-reference.md](en/config-file/config-reference.md) | [配置项参考](zh-CN/config-file/config-reference.zh-CN.md) | 逐项查询配置键名、类型、含义与相关限制。 |
| [config-file/config-sample.md](en/config-file/config-sample.md) | [配置示例](zh-CN/config-file/config-sample.zh-CN.md) | 提供带说明的 config.toml 示例，作为配置起点和对照资料。 |
| [config-file/environment-variables.md](en/config-file/environment-variables.md) | [环境变量](zh-CN/config-file/environment-variables.zh-CN.md) | 列出用于临时覆盖、自动化、安装和诊断的环境变量。 |
| [configuration.md](en/configuration.md) | [配置概览](zh-CN/configuration.zh-CN.md) | 帮助选择默认设置、持久上下文、项目指令及开发工具定制的相关文档。 |
| [developer-commands.md](en/developer-commands.md) | [开发者命令](zh-CN/developer-commands.zh-CN.md) | 汇总不同使用界面的开发命令、CLI 参数、交互命令与快捷操作。 |
| [developer-settings.md](en/developer-settings.md) | [开发者设置](zh-CN/developer-settings.zh-CN.md) | 解释不同界面中的开发设置，以及这些设置与本地配置的关系。 |
| [models.md](en/models.md) | [模型选择](zh-CN/models.zh-CN.md) | 介绍模型与推理强度的选择方式及不同界面的适用设置。 |
| [permissions.md](en/permissions.md) | [权限配置技术参考](zh-CN/permissions.zh-CN.md) | 详细解释权限配置文件、文件系统和网络规则，面向需要配置具体权限的用户。 |
| [sandboxing.md](en/sandboxing.md) | [沙箱](zh-CN/sandboxing.zh-CN.md) | 解释本地执行的文件与网络隔离，以及沙箱和审批的关系。 |
| [sandboxing/auto-review.md](en/sandboxing/auto-review.md) | [自动审核机制](zh-CN/sandboxing/auto-review.zh-CN.md) | 详细解释审批审核智能体的工作方式及其与现有沙箱边界的关系。 |

## 06 开发接口、自动化与第三方集成

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [amazon-bedrock.md](en/amazon-bedrock.md) | [通过 Amazon Bedrock 使用模型](zh-CN/amazon-bedrock.zh-CN.md) | 说明如何让本地 ChatGPT Work 和 Codex 通过 AWS 身份验证与 Bedrock 调用模型。 |
| [app-server.md](en/app-server.md) | [Codex 应用服务器](zh-CN/app-server.zh-CN.md) | 提供 app-server 集成接口，涵盖认证、对话历史、审批及流式事件，供开发自定义客户端使用。 |
| [codex-sdk.md](en/codex-sdk.md) | [Codex SDK](zh-CN/codex-sdk.zh-CN.md) | 说明如何通过程序控制 Codex，用于自动化、CI/CD 和自定义应用集成。 |
| [developers.md](en/developers.md) | [开发者入口](zh-CN/developers.zh-CN.md) | 按代码库、开发环境、自动化及团队工具组织开发相关文档。 |
| [github-action.md](en/github-action.md) | [GitHub Action](zh-CN/github-action.zh-CN.md) | 说明如何在 GitHub Actions 中运行 Codex，配置认证、权限和自动化任务。 |
| [mcp-server.md](en/mcp-server.md) | [旧 MCP 服务器命令移除](zh-CN/mcp-server.zh-CN.md) | 说明已移除的 codex mcp-server 命令及旧集成需要迁移的事项。 |
| [non-interactive-mode.md](en/non-interactive-mode.md) | [非交互模式](zh-CN/non-interactive-mode.zh-CN.md) | 说明如何用 codex exec 在脚本和 CI 中运行任务并处理输出。 |
| [open-source.md](en/open-source.md) | [开源项目](zh-CN/open-source.zh-CN.md) | 介绍 Codex 的开源组件、贡献入口与开源维护者相关计划。 |
| [third-party/github.md](en/third-party/github.md) | [GitHub 拉取请求审查](zh-CN/third-party/github.zh-CN.md) | 说明如何让 Codex 在 GitHub 中审查差异并发布代码审查反馈。 |
| [third-party/gitlab.md](en/third-party/gitlab.md) | [GitLab 合并请求审查](zh-CN/third-party/gitlab.zh-CN.md) | 说明如何让 Codex 在 GitLab 中审查差异并发布反馈。 |
| [third-party/linear.md](en/third-party/linear.md) | [Linear 集成](zh-CN/third-party/linear.zh-CN.md) | 介绍通过分派问题或提及 Codex 启动云端工作。 |
| [third-party/slack.md](en/third-party/slack.md) | [Slack 集成](zh-CN/third-party/slack.zh-CN.md) | 介绍在频道和消息串中提及 Codex，启动任务并接收结果。 |

## 07 企业部署与工作区管理

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [administration.md](en/administration.md) | [管理概览](zh-CN/administration.zh-CN.md) | 梳理工作区访问、本地运行策略、云端资格、API、插件和外部系统权限的管理边界。 |
| [enterprise/access-tokens.md](en/enterprise/access-tokens.md) | [访问令牌](zh-CN/enterprise/access-tokens.zh-CN.md) | 介绍非交互本地工作流如何使用工作区身份进行认证。 |
| [enterprise/admin-plugin.md](en/enterprise/admin-plugin.md) | [管理员插件使用指南](zh-CN/enterprise/admin-plugin.zh-CN.md) | 介绍管理员插件的常见任务、准备工作、审批和提示词示例。 |
| [enterprise/admin-setup.md](en/enterprise/admin-setup.md) | [企业部署指南](zh-CN/enterprise/admin-setup.zh-CN.md) | 帮助规划企业推广，明确访问范围、责任人、管理控制和验证步骤。 |
| [enterprise/analytics-api.md](en/enterprise/analytics-api.md) | [分析 API](zh-CN/enterprise/analytics-api.zh-CN.md) | 介绍通过程序获取工作区 Codex 用量和活动聚合指标。 |
| [enterprise/apps-and-connectors.md](en/enterprise/apps-and-connectors.md) | [插件控制](zh-CN/enterprise/apps-and-connectors.zh-CN.md) | 介绍管理员如何控制工作区插件的可用性及连接权限。 |
| [enterprise/chatgpt-work-cloud-security.md](en/enterprise/chatgpt-work-cloud-security.md) | [Work 云端安全](zh-CN/enterprise/chatgpt-work-cloud-security.zh-CN.md) | 说明托管执行、连接账户、网络、数据边界和审计可见性。 |
| [enterprise/chatgpt-work-local-security.md](en/enterprise/chatgpt-work-local-security.md) | [Work 本地安全](zh-CN/enterprise/chatgpt-work-local-security.zh-CN.md) | 说明本地文件、应用和浏览器访问所受的权限与设备策略控制。 |
| [enterprise/chatgpt-work-overview.md](en/enterprise/chatgpt-work-overview.md) | [Work 企业概览](zh-CN/enterprise/chatgpt-work-overview.zh-CN.md) | 解释 Work 与 Codex 共享的执行、隔离和安全边界。 |
| [enterprise/chatgpt-work-usage-and-cost.md](en/enterprise/chatgpt-work-usage-and-cost.md) | [Work 用量与成本](zh-CN/enterprise/chatgpt-work-usage-and-cost.zh-CN.md) | 解释多步骤任务的额度消耗、成本影响和采用规划。 |
| [enterprise/compliance-api.md](en/enterprise/compliance-api.md) | [合规 API 与审计事件](zh-CN/enterprise/compliance-api.zh-CN.md) | 介绍可审计记录及其在安全、治理和调查中的用途。 |
| [enterprise/governance.md](en/enterprise/governance.md) | [治理](zh-CN/enterprise/governance.zh-CN.md) | 帮助区分交互分析、程序化报表、用量控制和审计记录的用途。 |
| [enterprise/gpts-and-sharing.md](en/enterprise/gpts-and-sharing.md) | [GPT 与共享管理](zh-CN/enterprise/gpts-and-sharing.zh-CN.md) | 说明 GPT 创建、共享范围以及应用与自定义操作相关限制。 |
| [enterprise/groups-and-provisioning.md](en/enterprise/groups-and-provisioning.md) | [用户组与预配](zh-CN/enterprise/groups-and-provisioning.zh-CN.md) | 说明用户组、自定义角色、成员关系和席位分配之间的关系。 |
| [enterprise/manage-app-updates.md](en/enterprise/manage-app-updates.md) | [应用更新管理](zh-CN/enterprise/manage-app-updates.zh-CN.md) | 介绍如何关闭自动更新并集中部署经过审核的桌面应用版本。 |
| [enterprise/managed-configuration.md](en/enterprise/managed-configuration.md) | [受管配置](zh-CN/enterprise/managed-configuration.zh-CN.md) | 说明组织如何约束本地运行时行为，以及受管要求的作用范围。 |
| [enterprise/plugin-management.md](en/enterprise/plugin-management.md) | [插件市场管理](zh-CN/enterprise/plugin-management.zh-CN.md) | 说明如何从 GitHub 导入插件市场并同步更新。 |
| [enterprise/prisma-airs.md](en/enterprise/prisma-airs.md) | [Prisma AIRS 集成](zh-CN/enterprise/prisma-airs.zh-CN.md) | 介绍将企业安全策略应用于发送到模型之前的提示词。 |
| [enterprise/roles-and-workspace-permissions.md](en/enterprise/roles-and-workspace-permissions.md) | [角色与工作区权限](zh-CN/enterprise/roles-and-workspace-permissions.zh-CN.md) | 解释不同管理边界中的角色、访问权限和授权差异。 |
| [enterprise/service-accounts.md](en/enterprise/service-accounts.md) | [服务账户](zh-CN/enterprise/service-accounts.zh-CN.md) | 介绍为 CI、计划任务和共享集成提供独立工作区身份。 |
| [enterprise/skills.md](en/enterprise/skills.md) | [技能管理控制](zh-CN/enterprise/skills.zh-CN.md) | 区分工作区技能、本地文件系统技能和插件附带技能的管理方式。 |
| [enterprise/usage-limits.md](en/enterprise/usage-limits.md) | [用量上限与支出控制](zh-CN/enterprise/usage-limits.zh-CN.md) | 解释工作区可配置的用量和支出限制及其适用范围。 |
| [enterprise/user-lifecycle.md](en/enterprise/user-lifecycle.md) | [用户生命周期](zh-CN/enterprise/user-lifecycle.zh-CN.md) | 说明员工加入、职责变更和离开时的访问、席位与权限处理。 |
| [enterprise/windows-deployment.md](en/enterprise/windows-deployment.md) | [Windows 应用部署](zh-CN/enterprise/windows-deployment.zh-CN.md) | 介绍个人安装与 IT 集中部署桌面应用的方法。 |
| [enterprise/work-admin-faq.md](en/enterprise/work-admin-faq.md) | [Work 管理员常见问题](zh-CN/enterprise/work-admin-faq.zh-CN.md) | 汇总访问、数据、治理、用量和事件控制方面的问答。 |
| [enterprise/workload-identity.md](en/enterprise/workload-identity.md) | [工作负载身份联合](zh-CN/enterprise/workload-identity.zh-CN.md) | 说明自动化如何使用短期身份令牌，减少长期凭据存储。 |
| [enterprise/workspace-analytics.md](en/enterprise/workspace-analytics.md) | [工作区分析](zh-CN/enterprise/workspace-analytics.zh-CN.md) | 帮助选择工作区分析、Codex 分析、分析 API 或合规记录。 |
| [enterprise/workspace-model-availability.md](en/enterprise/workspace-model-availability.md) | [工作区模型可用性](zh-CN/enterprise/workspace-model-availability.zh-CN.md) | 解释登录方式、产品界面和工作区设置对可用模型的影响。 |

## 08 安全扫描、漏洞修复与安全管理

| 英文文件 | 中文 | 用途 |
| --- | --- | --- |
| [cyber-safety.md](en/cyber-safety.md) | [网络安全模型与受信任访问](zh-CN/cyber-safety.zh-CN.md) | 介绍 Daybreak 相关访问等级与获得授权的防御性安全工作范围。 |
| [cyber-safety/recommended-configuration.md](en/cyber-safety/recommended-configuration.md) | [安全任务推荐配置](zh-CN/cyber-safety/recommended-configuration.zh-CN.md) | 提供开展网络安全相关工作时的环境与权限配置建议。 |
| [security-administration.md](en/security-administration.md) | [安全管理概览](zh-CN/security-administration.zh-CN.md) | 汇总访问控制、工作隔离和安全敏感任务防护相关文档。 |
| [security.md](en/security.md) | [Codex Security 概览](zh-CN/security.zh-CN.md) | 介绍发现、验证和修复漏洞的产品能力，以及插件、CLI、SDK 和云端入口。 |
| [security/cli.md](en/security/cli.md) | [Security CLI 快速开始](zh-CN/security/cli.zh-CN.md) | 介绍安装、首次扫描和通过终端管理安全发现的基本流程。 |
| [security/cli/bulk-scans.md](en/security/cli/bulk-scans.md) | [批量安全扫描](zh-CN/security/cli/bulk-scans.zh-CN.md) | 说明如何发现或列出多个仓库并开展统一扫描。 |
| [security/cli/ci.md](en/security/cli/ci.md) | [在 CI 中运行安全扫描](zh-CN/security/cli/ci.zh-CN.md) | 介绍针对提交变更运行扫描、保存结果并设置失败阈值。 |
| [security/cli/ci/gitlab.md](en/security/cli/ci/gitlab.md) | [GitLab CI/CD 安全扫描](zh-CN/security/cli/ci/gitlab.zh-CN.md) | 说明如何扫描提交与受保护分支，并发布安全发现或准备修复合并请求。 |
| [security/cli/faq.md](en/security/cli/faq.md) | [Security CLI 常见问题](zh-CN/security/cli/faq.zh-CN.md) | 解答终端扫描、安装及安全发现管理方面的常见问题。 |
| [security/cli/reference.md](en/security/cli/reference.md) | [Security CLI 命令参考](zh-CN/security/cli/reference.zh-CN.md) | 查询命令、参数、输出格式和退出行为。 |
| [security/faq.md](en/security/faq.md) | [Security 云端常见问题](zh-CN/security/faq.zh-CN.md) | 汇总云端安全扫描相关问答，并指向本地扫描资料。 |
| [security/plugin.md](en/security/plugin.md) | [Security 插件快速开始](zh-CN/security/plugin.zh-CN.md) | 介绍通过插件扫描代码、验证发现并获得修复建议。 |
| [security/plugin/changelog.md](en/security/plugin/changelog.md) | [Security 插件更新记录](zh-CN/security/plugin/changelog.zh-CN.md) | 记录插件版本变更，便于核对功能、修复和兼容性。 |
| [security/plugin/code-changes.md](en/security/plugin/code-changes.md) | [代码变更安全审查](zh-CN/security/plugin/code-changes.zh-CN.md) | 说明如何检查某一次 Git 变更中的安全回归及直接关联代码。 |
| [security/plugin/deep-scans.md](en/security/plugin/deep-scans.md) | [深度安全扫描](zh-CN/security/plugin/deep-scans.zh-CN.md) | 介绍需要更彻底审查时的深度扫描流程和适用场景。 |
| [security/plugin/export-findings.md](en/security/plugin/export-findings.md) | [导出与跟踪发现](zh-CN/security/plugin/export-findings.zh-CN.md) | 说明如何导出 JSON、CSV、SARIF 或将发现转交到问题跟踪系统。 |
| [security/plugin/fix-findings.md](en/security/plugin/fix-findings.md) | [修复与验证发现](zh-CN/security/plugin/fix-findings.zh-CN.md) | 说明如何把已接受的安全问题转化为可验证的补丁。 |
| [security/plugin/scans.md](en/security/plugin/scans.md) | [标准安全扫描](zh-CN/security/plugin/scans.zh-CN.md) | 介绍首次或例行仓库评估时的标准扫描工作流程。 |
| [security/plugin/security-hardening.md](en/security/plugin/security-hardening.md) | [安全加固建议](zh-CN/security/plugin/security-hardening.zh-CN.md) | 说明如何根据扫描证据提出结构或架构层面的加固方案。 |
| [security/plugin/triage-backlog.md](en/security/plugin/triage-backlog.md) | [历史发现分诊](zh-CN/security/plugin/triage-backlog.zh-CN.md) | 介绍对积压发现进行只读静态复核，判断当前代码是否仍有相关问题。 |
| [security/plugin/vulnerability-reports.md](en/security/plugin/vulnerability-reports.md) | [漏洞报告](zh-CN/security/plugin/vulnerability-reports.zh-CN.md) | 说明如何根据发现、复现材料和代码整理独立完整的漏洞报告。 |
| [security/plugin/workbench.md](en/security/plugin/workbench.md) | [安全工作台](zh-CN/security/plugin/workbench.zh-CN.md) | 介绍在桌面应用中集中查看仓库、扫描和安全发现。 |
| [security/sdk.md](en/security/sdk.md) | [Security TypeScript SDK](zh-CN/security/sdk.zh-CN.md) | 说明如何从程序运行安全扫描并获取类型化发现与覆盖信息。 |
| [security/security-review.md](en/security/security-review.md) | [安全审查](zh-CN/security/security-review.zh-CN.md) | 介绍 Security Review 的适用方式、研究预览与访问条件。 |
| [security/setup.md](en/security/setup.md) | [Security 云端设置](zh-CN/security/setup.zh-CN.md) | 介绍从开通、配置到审阅扫描结果和修复拉取请求的流程。 |
| [security/threat-model.md](en/security/threat-model.md) | [威胁模型](zh-CN/security/threat-model.zh-CN.md) | 解释项目安全概述的作用，以及如何通过编辑威胁模型改善扫描。 |

资料快照日期为 2026-09-11。价格、可用功能、版本和政策相关内容反映该快照；需确认当前情况时，请查看原文中的官方链接。
