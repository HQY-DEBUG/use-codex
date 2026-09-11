> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/developers.md)。

<a id="developers"></a>

# 开发者入口

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<CodexDocsOverviewLanding
  title="开发商"
  description="将 Codex 与代码库、开发环境、自动化和团队工具结合使用。"
  intro="Codex 支持日常代码工作以及跨本地和云环境的更深入集成。其开发人员工作流程涵盖代码审查、集成终端、可重用技能和插件、SDK 和应用服务器的自动化、团队工具以及每个使用界面的参考材料。"
  video={{
    title: "Codex 为工程师提供的新功能",
    videoId: "eiQgljOrkWU",
  }}
  primaryCta={{
    label: "探索工作流程",
    href:"code-review.zh-CN.md",
  }}
  hero={{
    illustration: "developers",
    backgroundImage: "/images/codex/codex-wallpaper-1.webp",
    alt: "Codex Web 应用程序的输出、集成终端和代码审查",
  }}
  sections={[
    {
      title: "开发工作流程",
      description: "查看更改并使用 ChatGPT 中的开发工具。",
      pages: [
        {
          title: "代码审查",
          description: "在发货前查看更改并解决反馈。",
          href:"code-review.zh-CN.md",
          icon: "shieldCheck",
        },
        {
          title: "综合终端",
          description:
            "运行命令并检查 ChatGPT 桌面应用程序内的输出。",
          href:"integrated-terminal.zh-CN.md",
          icon: "terminal",
        },
      ],
    },
    {
      title: "扩展和自动化",
      description:
        "打包开发工作流程并运行确定性自动化。",
      pages: [
        {
          title: "培养技能",
          description:
            "ChatGPT 和 Codex 中可重复任务的软件包说明和资源。",
          href:"build-skills.zh-CN.md",
          icon: "tools",
        },
        {
          title: "构建插件",
          description: "适用于 ChatGPT 和 Codex 的软件包技能和 MCP 服务器。",
          href:"build-plugins.zh-CN.md",
          icon: "connect",
        },
        {
          title: "站点工具 (WebMCP)",
          description:
            "使用 WebMCP 为 AI 智能体提供直接使用您网站的方式。",
          href:"webmcp.zh-CN.md",
          icon: "tools",
        },
        {
          title: "挂钩",
          description: "当 Codex 发出生命周期事件时运行自定义命令。",
          href:"hooks.zh-CN.md",
          icon: "terminal",
        },
      ],
    },
    {
      title: "环境",
      description: "选择开发工作的运行位置以及隔离方式。",
      pages: [
        {
          title: "环境",
          description: "比较本地、云和其他运行任务的方式。",
          href:"environments/modes.zh-CN.md",
          icon: "workspace",
        },
        {
          title: "当地环境",
          description:
            "为项目和工作树配置安装脚本和操作。",
          href:"environments/local-environment.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "云环境",
          description: "将工作委派给已配置的云环境。",
          href:"environments/cloud-environment.zh-CN.md",
          icon: "storage",
        },
        {
          title: "Git 工作树",
          description: "将并行更改隔离在单独的工作树中。",
          href:"environments/git-worktrees.zh-CN.md",
          icon: "folder",
        },
      ],
    },
    {
      title: "使用 Codex 构建",
      description: "将 Codex 添加到产品、系统和自动化工作流程中。",
      pages: [
        {
          title: "Codex SDK",
          description: "从您的应用程序以编程方式控制 Codex。",
          href:"codex-sdk.zh-CN.md",
          icon: "code",
        },
        {
          title: "应用服务器",
          description: "与支持 Codex 客户端的协议集成。",
          href:"app-server.zh-CN.md",
          icon: "storage",
        },
        {
          title: "GitHub 动作",
          description: "从 GitHub 操作工作流程运行 Codex。",
          href:"github-action.zh-CN.md",
          icon: "github",
        },
        {
          title: "非交互模式",
          description: "从脚本和其他自动化系统运行 Codex。",
          href:"non-interactive-mode.zh-CN.md",
          icon: "terminal",
        },
      ],
    },
    {
      title: "第三方集成",
      description: "通过您的团队已使用的工具委派和跟踪工作。",
      pages: [
        {
          title: "GitHub",
          description:
            "分配工作、审查更改并转向拉取请求。",
          href:"third-party/github.zh-CN.md",
          icon: "github",
        },
        {
          title: "GitLab（测试版）",
          description:
            "连接项目、委派工作并审查合并请求。",
          href:"third-party/gitlab.zh-CN.md",
          icon: "connect",
        },
        {
          title: "Slack",
          description:
            "从外部讨论开始 Codex 聊天并返回结果。",
          href:"third-party/slack.zh-CN.md",
          icon: "chat",
        },
        {
          title: "线性",
          description:
            "将问题分配给 Codex 并跟踪工作直至交付。",
          href:"third-party/linear.zh-CN.md",
          icon: "threads",
        },
      ],
    },
    {
      title: "参考",
      description:
        "查找开发人员界面的命令、设置和插件提交错误。",
      pages: [
        {
          title: "CLI定制",
          description:
            "调整语法突出显示、主题和 shell 行为。",
          href:"cli-customization.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "开发者命令",
          description:
            "在桌面应用程序、Codex CLI 和 IDE 扩展中使用命令和斜杠命令。",
          href:"developer-commands.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "开发者设置",
          description:
            "配置桌面应用程序、Codex CLI 和 IDE 扩展以进行开发。",
          href:"developer-settings.zh-CN.md",
          icon: "settings",
        },
      ],
    },
  ]}
/>