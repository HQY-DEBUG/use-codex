> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/build-skills.md)。

<a id="build-skills"></a>

# 创建技能

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用智能体技能来扩展 ChatGPT 和 Codex 的任务特定功能。技能包说明、资源和可选脚本，因此任一产品都可以可靠地遵循工作流程。技能以 [开放智能体技能标准](https://agentskills.io) 为基础。

技能是可重用工作流程的创作格式。插件通过 ChatGPT 和 Codex 共享的通用插件目录分发可重用的技能和连接器。插件可在 Web、桌面和移动设备上的 ChatGPT、ChatGPT 桌面应用程序中的 Codex 以及通过 Codex CLI 中的聊天和工作中使用。使用技巧来设计工作流程本身，然后在您希望其他人安装时将其打包为 [插件](https://developers.openai.com/plugins/build/plugins)。

ChatGPT 桌面应用程序、Codex CLI 和 IDE 扩展中提供了独立技能。插件中捆绑的技能也可在 Web、桌面和移动设备上的 ChatGPT 的聊天和工作中使用。

在 ChatGPT 桌面应用程序中，打开侧边栏中的 **技能** 以查看和探索跨项目创建的技能。


  

> 插图：技能选择器显示 ChatGPT 桌面应用程序中的可用技能




技能使用 **渐进式披露** 来有效管理上下文。 ChatGPT 和 Codex 从每个技能的名称和描述开始，然后在决定使用该技能时加载完整的 `SKILL.md` 指令。

在Codex中，初始列表还包括每个技能的文件路径。为了避免挤占提示的其余部分，此列表最多使用模型上下文窗口的 2%，或者当上下文窗口未知时使用 8,000 个字符。如果安装了很多技能，Codex首先会缩短技能描述。对于大型技能集，Codex 可能会从初始列表中省略一些技能并显示警告。

此预算仅适用于初始技能列表。当 Codex 选择一项技能时，它仍然会读取该技能的完整 SKILL.md 说明。

技能是一个包含 `SKILL.md` 文件以及可选脚本和参考的目录。 `SKILL.md` 文件必须包含 `name` 和 `description`。

<FileTree
  class="mt-4"
  tree={[
    {
      name: "my-skill/",
      open: true,
      children: [
        {
          name: "SKILL.md",
          comment: "必需：说明+元数据",
        },
        {
          name: "scripts/",
          comment: "可选：可执行代码",
        },
        {
          name: "references/",
          comment: "可选：文档",
        },
        {
          name: "assets/",
          comment: "可选：模板、资源",
        },
        {
          name: "agents/",
          open: true,
          children: [
            {
              name: "openai.yaml",
              comment: "可选：外观和依赖项",
            },
          ],
        },
      ],
    },

]}
/>

<a id="how-codex-uses-skills"></a>

<a id="how-chatgpt-and-codex-use-skills"></a>

## ChatGPT和Codex如何使用技能

ChatGPT和Codex可以通过两种方式激活技能：

1. **显式调用：** 直接在提示中包含技能。在ChatGPT中，输入`@`来选择技能。在 Codex CLI 或 IDE 扩展中，运行 `/skills` 或输入 `$` 来提及技能。
2. 当您的任务与技能 `description` 匹配时，**隐式调用：**、ChatGPT 或 Codex 可以选择技能。

由于隐式匹配依赖于 `description`，因此请编写简洁的描述，并具有明确的范围和边界。预先加载关键用例和触发词，这样即使描述被缩短，主持人仍然可以匹配技能。

<a id="create-a-skill"></a>

## 创建技能

如果您已经了解工作流程并且展示比描述更容易，请使用 [录制与回放](extend/record-and-replay.zh-CN.md)。记录员捕获工作流程，检查步骤，并从演示中起草可重复使用的技能。

如果您想描述该技能，请使用内置创建器。在ChatGPT Work中，将其调用为`@skill-creator`。在 Codex 中，将其调用为：

```text
$skill-creator
```

创建者会询问该技能的作用、何时触发以及是否应该仅包含指令或包含脚本。默认情况下仅指示。

您还可以通过创建包含 `SKILL.md` 文件的文件夹来手动创建技能：

```md
---
name: 技能名称
描述：准确解释该技能何时应该触发、何时不应该触发。
---

要遵循的 ChatGPT 或 Codex 的技能说明。
```

Codex自动检测技能变化。如果未出现更新，请重新启动 Codex。

<a id="where-to-save-skills"></a>

<a id="where-codex-loads-local-skills"></a>

## Codex在哪里加载本地技能

Codex 从仓库、用户、管理员和系统位置读取技能。对于仓库，Codex 扫描从当前工作目录到仓库根目录的每个目录中的 `.agents/skills`。如果两个技能共享相同的`name`，则Codex不会将它们合并；两者都可以出现在技能选择器中。

| 技能范围 | 位置 | 建议使用 |
| :---------- | :-------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `REPO` | `$CWD/.agents/skills`<br />当前工作目录：启动 Codex 的位置。                           | 如果您位于仓库或代码环境中，团队可以签入与工作文件夹相关的技能。例如，仅与微服务或模块相关的技能。                              |
| `REPO` | `$CWD/../.agents/skills`<br />当您在 Git 仓库中启动 Codex 时，CWD 上方的文件夹。         | 如果您位于包含嵌套文件夹的仓库中，组织可以签入与父文件夹中的共享区域相关的技能。                                                                       |
| `REPO` | `$REPO_ROOT/.agents/skills`<br />当您在 Git 仓库中启动 Codex 时，最顶层的根文件夹。 | 如果您位于包含嵌套文件夹的仓库中，组织可以签入与使用该仓库的每个人相关的技能。这些作为根技能可用于仓库中的任何子文件夹。 |
| `USER` | `$HOME/.agents/skills`<br />签入用户个人文件夹中的任何技能。                         | 用于管理与用户相关的技能，这些技能适用于用户可能使用的任何仓库。 |
| `ADMIN` | `/etc/codex/skills`<br />在共享系统位置签入机器或容器的任何技能。 | 用于 SDK 脚本、自动化以及检查计算机上每个用户可用的默认管理技能。                                                                                     |
| `SYSTEM` | 由 OpenAI 与 Codex 捆绑。                                                                             | 与广大受众相关的有用技能，例如技能创建者和计划技能。每个人在启动 Codex 时都可以使用。                                                                   |

Codex 支持符号链接技能文件夹，并在扫描这些位置时遵循符号链接目标。

这些位置用于创作和本地发现。当您想要将可重用技能分发到单个仓库之外，或者选择将它们与连接器捆绑在一起时，请使用 [插件](https://developers.openai.com/plugins/build/plugins)。

<a id="distribute-skills-with-plugins"></a>

## 通过插件分发技能

直接技能文件夹最适合本地创作和仓库范围的工作流程。如果您想要分发可重用技能、将两个或多个技能捆绑在一起，或者与连接器一起发送技能，请将它们打包为 [插件](https://developers.openai.com/plugins/build/plugins)。

插件可以包含一项或多项技能。他们还可以选择将注册的 MCP 服务器连接、捆绑的 MCP 服务器配置和演示资源捆绑在一个包中。

<a id="install-curated-skills-for-local-use"></a>

## 安装精选技能以供本地使用

要为您自己的本地 Codex 设置添加内置技能之外的精选技能，请使用 `$skill-installer`。例如，要安装 `$linear` 技能：

```bash
$skill-installer linear
```

您还可以提示安装程序从其他仓库下载技能。 Codex自动检测新安装的技能；如果没有出现，请重新启动Codex。

使用它进行本地设置和实验。为了可重复使用您自己的技能，最好使用插件。

<a id="enable-or-disable-local-codex-skills"></a>

## 启用或禁用本地 Codex 技能

使用 `~/.codex/config.toml` 中的 `[[skills.config]]` 条目禁用技能而不删除它：

```toml
[[skills.config]]
path = "/path/to/skill/SKILL.md"
enabled = false
```

更改`~/.codex/config.toml`后重新启动Codex。

<a id="optional-metadata"></a>

## 可选元数据

添加 `agents/openai.yaml` 以在 [ChatGPT 桌面应用程序](app.zh-CN.md) 中配置 UI 元数据、设置调用策略并声明工具依赖项，以获得更无缝的技能使用体验。

```yaml
interface:
  display_name: "Optional user-facing name"
  short_description: "Optional user-facing description"
  icon_small: "./assets/small-logo.svg"
  icon_large: "./assets/large-logo.png"
  brand_color: "#3B82F6"
  default_prompt: "Optional surrounding prompt to use the skill with"

policy:
  allow_implicit_invocation: false

dependencies:
  tools:
    - type: "mcp"
      value: "openaiDeveloperDocs"
      description: "OpenAI Docs MCP server"
      transport: "streamable_http"
      url: "https://developers.openai.com/mcp"
```

`allow_implicit_invocation`（默认：`true`）：当`false`、Codex时，不会根据用户提示隐式调用技能；显式 `$skill` 调用仍然有效。

<a id="best-practices"></a>

## 最佳实践

- 让每项技能都专注于一项工作。
- 优先使用指令而不是脚本，除非您需要确定性行为或外部工具。
- 编写具有明确输入和输出的命令式步骤。
- 根据技能描述测试提示以确认正确的触发行为。

有关更多示例，请参阅 [GitHub CI修复](https://github.com/openai/skills/tree/main/skills/.curated/gh-fix-ci)、[PDF](https://github.com/openai/skills/tree/main/skills/.curated/pdf)、[线性](https://github.com/openai/skills/tree/main/skills/.curated/linear)、[开放/技能](https://github.com/openai/skills) 和 [智能体技能规范](https://agentskills.io/specification)。对于可安装的发行版，首选 [插件](https://developers.openai.com/plugins/build/plugins)。