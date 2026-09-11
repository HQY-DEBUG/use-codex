> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/features.md)。

<a id="features"></a>

# 功能总览

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<CodexDocsOverviewLanding
  title="特点"
  description="探索在 ChatGPT 中工作的工作流程、功能、命令和设置。"
  intro="ChatGPT 将项目和长时间运行的聊天与网页浏览、文件、图像和插件结合在一起。命令、设置和故障排除参考完善了这些工作流程，从选择正确的工作流程到为每个聊天提供所需的上下文和工具。"
  primaryCta={{
    label: "探索项目和聊天",
    href:"projects.zh-CN.md",
  }}
  hero={{
    illustration: "features-menu",
    backgroundImage: "/images/codex/codex-wallpaper-1.webp",
    alt: "Codex 侧边栏和添加聊天、插件和内容工具的菜单",
  }}
  sections={[
    {
      title: "工作流程",
      description: "组织、委派和审查工作的方式。",
      pages: [
        {
          title: "项目和聊天",
          description: "保持相关的聊天、上下文并一起工作。",
          href:"projects.zh-CN.md",
          icon: "folder",
        },
        {
          title: "Codex 遥控器",
          description:
            "通过手机启动任务、批准操作并查看工作。",
          href:"remote.zh-CN.md",
          icon: "connect",
        },
        {
          title: "站点",
          description:
            "在 ChatGPT 中创建、保存和发布交互式网站和应用程序。",
          href:"sites.zh-CN.md",
          icon: "workspace",
        },
        {
          title: "可视化",
          description:
            "将想法和信息转化为交互式视觉解释。",
          href:"visualizations.zh-CN.md",
          icon: "sparkles",
        },
        {
          title: "计划任务",
          description: "安排重复性工作并审查已完成的结果。",
          href:"automations.zh-CN.md",
          icon: "calendar",
        },
        {
          title: "长时间运行的工作",
          description: "当您离开时，让 ChatGPT 继续工作。",
          href:"long-running-work.zh-CN.md",
          icon: "threads",
        },
        {
          title: "通知",
          description:
            "选择 ChatGPT 在工作需要注意时如何告诉您。",
          href:"notifications.zh-CN.md",
          icon: "chat",
        },
        {
          title: "宠物",
          description: "选择一个动画伴侣并关注聊天活动。",
          href:"pets.zh-CN.md",
          icon: "customize",
        },
        {
          title: "Codex微型",
          description:
            "通过 Work Louder 键盘监视和控制 ChatGPT 聊天。",
          href:"features/codex-micro.zh-CN.md",
          icon: "key",
        },
      ],
    },
    {
      title: "能力",
      description:
        "ChatGPT 工具可用于理解、创建和采取行动。",
      pages: [
        {
          title: "浏览器",
          description:
            "让 ChatGPT 浏览网站并采取行动，同时让您保持掌控。",
          href:"browser.zh-CN.md",
          icon: "webSearch",
        },
        {
          title: "电脑使用",
          description:
            "让ChatGPT通过可视化界面与应用程序进行交互。",
          href:"computer-use.zh-CN.md",
          icon: "computerUse",
        },
        {
          title: "ChatGPT 语音",
          description:
            "在 ChatGPT 桌面应用程序中的聊天、工作和 Codex 中尝试语音。",
          href:"features/voice.zh-CN.md",
          icon: "chat",
        },
        {
          title: "插件",
          description:
            "安装可重用的工作流程、连接的工具和共享上下文。",
          href:"plugins.zh-CN.md",
          icon: "plugin",
        },
        {
          title: "网页搜索",
          description:
            "查找当前信息并将来源引入任务中。",
          href:"web-search.zh-CN.md",
          icon: "webSearch",
        },
        {
          title: "图像生成",
          description: "创建和编辑图像作为您工作的一部分。",
          href:"image-generation.zh-CN.md",
          icon: "customize",
        },
        {
          title: "图像输入",
          description: "使用屏幕截图和图像作为 ChatGPT 的上下文。",
          href:"image-inputs.zh-CN.md",
          icon: "computerUse",
        },
        {
          title: "应用截图",
          description: "捕获应用程序状态以进行目视检查和调试。",
          href:"appshots.zh-CN.md",
          icon: "workspace",
        },
        {
          title: "浏览器扩展",
          description:
            "将 Chrome、Edge、Brave、Opera 或 Vivaldi 与 ChatGPT 结合使用。",
          href:"chrome-extension.zh-CN.md",
          icon: "connect",
        },
        {
          title: "处理文件",
          description:
            "创建、预览和优化文档及其他生成的文件。",
          href:"artifacts-viewer.zh-CN.md",
          icon: "stack",
        },
      ],
    },
    {
      title: "参考",
      description: "查找 ChatGPT 桌面应用程序的命令和设置。",
      pages: [
        {
          title: "命令",
          description: "使用应用程序命令、键盘快捷键和深层链接。",
          href:"reference/commands.zh-CN.md",
          icon: "terminal",
        },
        {
          title: "斜杠命令",
          description: "使用常见交互操作的快捷方式。",
          href:"https://learn.chatgpt.com/codex/reference/slash-commands",
          icon: "code",
        },
        {
          title: "设置",
          description: "配置 ChatGPT 桌面应用程序首选项。",
          href:"reference/settings.zh-CN.md",
          icon: "settings",
        },
        {
          title: "故障排除",
          description: "解决 ChatGPT 桌面应用程序中的常见问题。",
          href:"reference/troubleshooting.zh-CN.md",
          icon: "tools",
        },
      ],
    },
  ]}
/>