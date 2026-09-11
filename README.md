# Codex 中英文学习资料

个人的 ChatGPT / Codex 使用说明与中英文学习资料仓库。

资料快照日期：2026-09-11。来源：[ChatGPT Learn 文档](https://learn.chatgpt.com/docs)。

**想知道每个文件的作用，请打开 [文档用途总览](docs/official/README.md)。** 它按主题解释全部 148 篇文档，并提供中英文链接。

## 目录结构

```text
README.md                     仓库入口
docs/
├── official/                 官方资料原文、非官方译文与学习指南
│   ├── README.md             每个文件的用途、中英文对照和阅读建议
│   ├── learning-guide-zh-CN.md 中文学习指南：摘要与练习
│   ├── learning-guide-en.md   英文学习指南
│   ├── en/                   148 篇英文原文及 INDEX.md
│   └── zh-CN/                148 篇中文译文及 INDEX.md
└── rules/                    个人全局与项目规则模板（中英双版本）
```

中英文目录保留相同的子目录结构。例如，`docs/official/en/config-file/config-basic.md` 对应 `docs/official/zh-CN/config-file/config-basic.zh-CN.md`。

## 阅读入口

| 入口 | 适合做什么 |
| --- | --- |
| [中英文规则模板](docs/rules/README.md) | 获取全局与 FPGA／上位机项目规则，了解分类和安装方式。 |
| [每个文件的用途](docs/official/README.md) | 根据问题选择文档，区分名称相似的指南与技术参考。 |
| [全部中文文档索引](docs/official/zh-CN/INDEX.md) | 按主题浏览全部译文。 |
| [全部英文原文索引](docs/official/en/INDEX.md) | 查阅原文和官网来源。 |
| [中文学习指南](docs/official/learning-guide-zh-CN.md) | 阅读基础摘要并完成练习。 |
| [英文学习指南](docs/official/learning-guide-en.md) | 对照中文指南学习。 |

## 建议先读的四篇基础文档

| 主题 | 中文全文 | 英文原文 |
| --- | --- | --- |
| 提示词 | [提示词](docs/official/zh-CN/prompting.zh-CN.md) | [Prompting](docs/official/en/prompting.md) |
| 个性化 | [个性化 ChatGPT](docs/official/zh-CN/personalize.zh-CN.md) | [Personalize ChatGPT](docs/official/en/personalize.md) |
| 技能与插件 | [技能与插件](docs/official/zh-CN/skills-and-plugins.zh-CN.md) | [Skills & Plugins](docs/official/en/skills-and-plugins.md) |
| 权限 | [权限模式](docs/official/zh-CN/permission-modes.zh-CN.md) | [Permissions](docs/official/en/permission-modes.md) |

## 资料与翻译说明

学习指南是独立编写的摘要和练习；`docs/official/zh-CN/` 存放文档全文的非官方译文。四篇基础译文沿用此前版本，其余 144 篇采用机器翻译并经过术语和格式抽查，未逐句人工校订。代码、命令、配置键名和路径保留原样，说明文字与可识别的代码注释译为中文；技术细节可通过每篇顶部的英文链接核对。

文档为 UTF-8 Markdown。部分官方原文包含 MDX 组件与表格数据；译文保留这些结构并翻译其说明字段，普通 Markdown 阅读器可能显示组件源码。图片、视频和交互演示需要访问官网。

`codex-manual.md` 是较大的综合手册，内容与独立文档有所重叠，适合搜索查阅。价格、版本、功能可用性和政策均以资料快照为准。仓库不包含下载或校验脚本，更新原文后也需要同步核对译文。

官方资料的权利归原权利人；本仓库不为原文附加开源授权。
