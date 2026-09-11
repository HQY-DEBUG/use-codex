> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/configuration.md)。

<a id="configuration"></a>

# 配置概览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<CodexDocsOverviewLanding
  title="配置"
  description="设置默认值、添加持久上下文并自定义 ChatGPT 和 Codex 开发人员工具的工作方式。"
  intro="配置决定了 ChatGPT 和 Codex 开发人员工具在聊天、仓库和机器上的行为方式。持久上下文、配置文件、仓库指南、子智能体、外部连接以及 Linux 和 Windows 设置协同工作，使个人和团队的工作流程保持一致。"
  primaryCta={{
    label: "探索定制",
    href:"customization/overview.zh-CN.md",
  }}
  hero={{
    illustration: "configuration",
    backgroundImage: "/images/codex/codex-wallpaper-1.webp",
    alt: "ChatGPT 设置导航、配置文件选项和个性控件",
  }}
  sections={[
    {
      title: "定制化",
      description:
        "调整体验并在聊天之间传递有用的上下文。",
      pages: [
        {
          title: "定制概览",
          description:
            "使用指导、技能、MCP 和子智能体自定义 ChatGPT 和 Codex。",
          href:"customization/overview.zh-CN.md",
          icon: "customize",
        },
        {
          title: "回忆",
          description: "让 ChatGPT 在聊天中保留有用的上下文。",
          href:"customization/memories.zh-CN.md",
          icon: "threads",
        },
        {
          title: "Computer History",
          description:
            "使用最近的计算机活动作为上下文并管理所包含的内容。",
          href:"customization/computer-history.zh-CN.md",
          icon: "stack",
        },
      ],
    },
    {
      title: "配置文件",
      description:
        "使用配置文件和变量控制模型、工具、环境和默认值。",
      pages: [
        {
          title: "配置基础知识",
          description:
            "了解配置层并创建配置文件。",
          href:"config-file/config-basic.zh-CN.md",
          icon: "settings",
        },
        {
          title: "高级配置",
          description:
            "使用配置文件、提供商、策略和高级选项。",
          href:"config-file/config-advanced.zh-CN.md",
          icon: "dataControls",
        },
        {
          title: "配置参考",
          description: "查找每个支持的配置键。",
          href:"config-file/config-reference.zh-CN.md",
          icon: "code",
        },
        {
          title: "环境变量",
          description: "设置跨系统和会话更改的值。",
          href:"config-file/environment-variables.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "配置示例",
          description:
            "从完整的带注释的配置示例开始。",
          href:"config-file/config-sample.zh-CN.md",
          icon: "folder",
        },
      ],
    },
    {
      title: "智能体配置",
      description: "塑造智能体的协作方式并遵循项目指导。",
      pages: [
        {
          title: "AGENTS.md",
          description: "为仓库提供 Codex 持久指令。",
          href:"agent-configuration/agents-md.zh-CN.md",
          icon: "folder",
        },
        {
          title: "子智能体",
          description: "将重点任务委托给专业智能体。",
          href:"agent-configuration/subagents.zh-CN.md",
          icon: "robot",
        },
        {
          title: "速度",
          description: "控制 Codex 工作的速度和深度。",
          href:"agent-configuration/speed.zh-CN.md",
          icon: "settings",
        },
        {
          title: "规则",
          description: "定义命令Codex可以自动运行。",
          href:"agent-configuration/rules.zh-CN.md",
          icon: "dataControls",
        },
      ],
    },
    {
      title: "扩展 ChatGPT 和 Codex",
      description: "打包知识、连接服务并添加功能。",
      pages: [
        {
          title: "Record & Replay",
          description:
            "向 ChatGPT 或 Codex 展示工作流程并将其转变为可重用的技能。",
          href:"extend/record-and-replay.zh-CN.md",
          icon: "tools",
        },
        {
          title: "MCP",
          description:
            "将 Codex 开发人员工具连接到外部工具和上下文。",
          href:"extend/mcp.zh-CN.md",
          icon: "connect",
        },
      ],
    },
    {
      title: "Linux",
      description: "在支持的 Linux 桌面上安装和更新 ChatGPT。",
      pages: [
        {
          title: "ChatGPT 桌面应用程序",
          description:
            "在 Ubuntu、Debian 或 Fedora 上安装 Linux 预览版。",
          href:"linux/linux-app.zh-CN.md",
          icon: "computerUse",
        },
      ],
    },
    {
      title: "窗户",
      description: "在 Windows 上或在 WSL 内本机运行 Codex。",
      pages: [
        {
          title: "ChatGPT 桌面应用程序",
          description:
            "将 ChatGPT 桌面应用程序与 PowerShell 或 WSL 工作流程结合使用。",
          href:"windows/windows-app.zh-CN.md",
          icon: "computerUse",
        },
        {
          title: "Windows沙箱",
          description:
            "使用本机文件系统和命令隔离运行 Codex。",
          href:"windows/windows-sandbox.zh-CN.md",
          icon: "lock",
        },
        {
          title: "世界SL",
          description: "在 Windows 管理的 Linux 环境中使用 Codex。",
          href:"windows/wsl.zh-CN.md",
          icon: "terminal",
        },
      ],
    },
  ]}
/>