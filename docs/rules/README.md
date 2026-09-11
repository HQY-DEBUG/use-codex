# Codex 规则模板 / Codex Rule Templates

这些模板整理自个人规则，分为全局偏好和 FPGA／上位机工程约定。中英文版本表达相同要求，选择一种语言安装即可。

These templates consolidate personal rules into global preferences and FPGA / host application project conventions. The Chinese and English versions express the same requirements; install either language.

| 规则 / Rules | 中文 / Chinese | English |
| --- | --- | --- |
| 全局 / Global | [AGENTS.global.md](AGENTS.global.md) | [AGENTS.global.en.md](AGENTS.global.en.md) |
| 项目 / Project | [AGENTS.project.md](AGENTS.project.md) | [AGENTS.project.en.md](AGENTS.project.en.md) |

## 分类 / Scope

- **全局**：中文交流、阅读与修改原则、通用目录组织、代码版本与变更标注、安全、Git 提交和结果反馈。
- **项目**：具体工程目录、系统 Python 环境、语言命名与风格、Qt 线程、文件头模板和项目文档格式。
- **Global**: Chinese communication, reading and editing principles, general repository organization, code versioning and annotations, security, Git commits, and feedback.
- **Project**: Concrete engineering directories, system Python, language naming and style, Qt threading, file-header templates, and project documentation format.

## 使用 / Installation

1. 选择一份全局文件，将内容合并到 Codex 主目录的 `AGENTS.md`；默认路径为 `~/.codex/AGENTS.md`，Windows 通常为 `%USERPROFILE%\.codex\AGENTS.md`。若设置了 `CODEX_HOME`，使用该目录。已有规则应先合并，避免直接覆盖。
2. 选择一份项目文件，合并到目标工程根目录的 `AGENTS.md`；将“项目名称”或 `<project-name>` 替换为实际 FPGA 工程目录名。
3. 两份文件配合使用。项目文件中的“全局规则”指已安装的全局 `AGENTS.md`，不依赖本模板目录的相对路径。重新启动会话并检查加载的规则来源。

1. Choose one global file and merge it into `AGENTS.md` in the Codex home directory: by default `~/.codex/AGENTS.md`, usually `%USERPROFILE%\.codex\AGENTS.md` on Windows. Use `CODEX_HOME` if configured. Merge existing instructions instead of overwriting them blindly.
2. Choose one project file and merge it into `AGENTS.md` at the target project root. Replace “项目名称” or `<project-name>` with the actual FPGA project directory name.
3. Use both files together. “Global rules” refers to the installed global `AGENTS.md`, not a relative path within this template directory. Restart the session and check the loaded instruction sources.

这些文件当前是模板，尚未安装到全局或目标工程。英文版仍要求使用中文回复、注释和提交信息，因此保留中文输出示例。全局模板保留“实质性修改完成并验证后自动本地提交”的个人偏好；推送需要另行授权。

These files are templates and are not installed globally or in a target project. The English version still requires Chinese responses, comments, and commit messages, so output examples remain in Chinese. The global template preserves the preference to commit substantive changes locally after completion and validation; pushing requires separate authorization.

修改规则时同步维护对应语言版本；全局条目不在项目文件重复展开，项目仅补充具体约定。

Keep both language versions in sync when editing. Do not duplicate global rules in the project file; add only project-specific details.

规则加载方式 / Instruction loading: [Official AGENTS.md documentation](https://learn.chatgpt.com/docs/agent-configuration/agents-md).
