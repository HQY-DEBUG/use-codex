<a id="prompting"></a>

# 提示词

> 本文根据本地英文原文 [prompting.md](../en/prompting.md) 翻译，为非官方简体中文译文。

> 完整文档索引请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。在文档页面的网址末尾添加 `.md`，即可获取该页面的 Markdown 版本。

<a id="prompts"></a>

<a id="prompting-overview"></a>

## 提示词概述

提示词是你告诉 ChatGPT 自己想了解、创建或修改什么的方式。提示词可以是问题、指令，也可以是目标。你不需要技术语法或固定公式。先用自己的话表达，查看回答，再通过后续消息逐步调整结果。

简短的提示词通常就足够了。对于规模更大或更重要的任务，可以补充以下关键内容：

- **目标：**你希望 ChatGPT 做什么？
- **上下文：**哪些信息或来源会有帮助？
- **输出：**你需要什么格式、长度或详细程度？
- **边界：**哪些内容必须保持不变？ChatGPT 应避免什么，或在采取哪些行动前先与你确认？

只需使用有帮助的部分。你不必填满每一项，也不必遵循某种规定格式。

<a id="describe-the-result-you-need"></a>

## 描述你需要的结果

从结果开始说明，不必一上来就列出详细步骤。如果受众或格式会影响 ChatGPT 应生成的内容，也请说明。

```text
将这些会议记录整理成一份发给项目团队的简短进展更新。
把决策和后续步骤放在最前面。
```

这条提示词说明了要创建什么，以及由谁阅读。如果流程本身很重要，就描述流程；否则，可以给 ChatGPT 留出空间，让它搜索、比较信息并调整方法。

<a id="context"></a>

<a id="add-useful-context"></a>

## 补充有用的上下文

提供可能影响结果的信息。只添加相关来源，并说明希望 ChatGPT 从每个来源中获取什么。

- 当你希望 ChatGPT 总结、比较、转换内容，或[创建供审阅的文件](artifacts-viewer.zh-CN.md)时，附上文档、电子表格、演示文稿或 PDF 文件。
- 当任务依赖视觉上下文时，添加屏幕截图、示意图或其他[图像输入](image-inputs.zh-CN.md)。指出关键区域，不要仅依赖图像本身传达全部信息。
- 当答案依赖最新信息时，请让 ChatGPT 使用[网页搜索](web-search.zh-CN.md)；如果需要核查结果，也请它提供来源。
- 当相关对话需要共享文件、来源或本地文件夹时，使用[项目](projects.zh-CN.md)。

<a id="use-connected-sources"></a>

### 使用已连接的信息来源

如果 ChatGPT 可以访问已连接的信息来源，请说明它应去哪里查找，以及要找到什么。你不需要逐一描述它应执行的搜索。

```text
使用 Drive 中最新的项目计划，以及该项目 Slack 频道中的相关决策和更新，
准备一份状态更新。
```

使用已连接的信息来源需要相应插件；是否可用还可能取决于你的套餐和工作区设置。

<a id="use-plugins"></a>

### 使用插件

插件为 ChatGPT 和 Codex 提供可复用的指令，以及与 Google Drive、Gmail、Slack 和 GitHub 等工具的连接。两个产品都从同一个通用目录获取公开插件。提出你需要的结果，让当前使用的界面从可用工具中进行选择。在 ChatGPT 中，在输入框键入 `@`，即可选择特定插件。

[了解插件

      <Plugin />

    在 ChatGPT 和 Codex 中查找、安装和使用插件。](https://learn.chatgpt.com/docs/plugins)

<a id="personalize-chatgpt"></a>

### 个性化 ChatGPT

将需要跨对话生效的偏好，作为自定义指令保存在**设置 > 个性化（Settings > Personalization）**中。只与当前对话有关的细节，放在提示词里即可。

[查看个性化设置

      <Settings />

    设置默认个性、自定义指令及其他应用偏好。](https://learn.chatgpt.com/docs/reference/settings#personalization)

<a id="set-boundaries-that-prevent-real-problems"></a>

## 设置能避免实际问题的边界

边界是少量必要指令，用来防止 ChatGPT 带来额外工作，或采取你并未打算让它执行的行动。如果改错某个细节会导致结果无法使用，或者你希望在内容影响他人之前先进行审阅，就应增加一条边界要求。

- 保持已批准的日期和预算数字不变。
- 只使用提供的来源。标明缺失信息，不要猜测。
- 推荐方案不得超出指定预算。
- 将消息准备为草稿，不要发送。

聚焦最重要的一两条边界即可。你不需要控制 ChatGPT 的每一步操作。

<a id="make-the-result-ready-to-use"></a>

## 让结果可以直接使用

告诉 ChatGPT 你打算如何使用结果。这有助于它选择合适的长度、详细程度和组织方式。

- 将其整理为一页摘要，供总监在会前快速浏览。把需要作出的决策和后续步骤放在最前面。
- 将这些笔记整理成一封跟进邮件，包含决策、负责人和截止日期。
- 创建一张清晰的表格，对比计划支出和实际支出，并突出显示超过 10% 的差异。

对于重要工作，请让 ChatGPT 做最后检查，例如确认每项行动事项都有负责人和截止日期，或标明无法核实的信息。随后，在使用或分享结果之前，你也应亲自审阅。

<a id="improve-the-result-with-follow-up-messages"></a>

## 通过后续消息改进结果

第一条提示词不必完美。查看结果后，再提出你希望进行的具体修改。

```text
让开头更直接，保留证据，并将建议移到背景部分之前。
```

你可以补充遗漏的来源、纠正方向、要求其他选项，或调整详细程度，无须重新开始。

<a id="steering-and-queuing"></a>

### 调整当前任务与排队（Steer 和 Queue）

当 Codex 已经在工作时，你无须等待当前运行结束，就可以发送另一条消息：

- **Steer（调整当前任务）**会将消息加入当前运行。适合用来调整方向、补充遗漏的细节或提供新信息。
- **Queue（排队）**会保存消息，留待下一次运行处理。适合需要等当前工作完成后再执行的后续请求。

在 ChatGPT 桌面应用中，可在[**设置 > 常规 > 后续消息行为（Settings > General > Follow-up behavior）**](reference/settings.zh-CN.md#general)中选择默认行为。排队的消息会显示在输入框上方，你可以编辑、调整顺序、发送或删除这些消息。该设置还会显示一个快捷键，让你在不更改默认设置的情况下，为某一条消息使用另一种行为。

在 Codex CLI 中，当 Codex 正在工作时，按 <kbd>Enter</kbd> 可调整当前轮次，按 <kbd>Tab</kbd> 则会将消息排队到下一轮。详情请参阅[交互快捷键](https://learn.chatgpt.com/docs/developer-commands?surface=cli#cli-interactive-shortcuts)。

<a id="put-the-pieces-together"></a>

## 将各部分组合起来

对于需要使用已连接信息来源的项目更新，一条完整提示词可以这样写：

```text
为周一的领导层会议准备一页项目状态更新。使用 Drive 中最新的项目计划，
以及该项目 Slack 频道中的相关决策和更新。

先列出需要领导层作出的决策和后续步骤。概述进展、风险、负责人和截止日期。
保持已批准的日期和预算数字不变。标明任何冲突或缺失的信息，不要发送或发布任何内容。

完成前，检查每个后续步骤是否都有负责人和截止日期。
```

这条提示词涵盖了**目标**、**上下文**、**输出**和**边界**，并要求进行最终检查，而没有逐一规定所有步骤。

<a id="use-voice-dictation"></a>

## 使用语音听写

在 ChatGPT 桌面应用中，当输入框可见时，按 <kbd>Ctrl+Shift+D</kbd>，然后开始说话。ChatGPT 会将语音转写到输入框中，以便你在发送提示词之前检查和编辑。

> 插图：输入框中的语音听写指示器，以及转写后的提示词。

<a id="threads"></a>
<a id="chats"></a>

<a id="prompting-examples-for-chat"></a>

## Chat 的提示词示例

使用 Chat 处理问题、想法、草稿和日常决策。先说明你想要的结果，再补充会影响答案的细节。

<a id="understand-a-topic"></a>

### 理解一个主题

```text
向一个从未投资过的人解释复利的运作方式。
使用一个具体例子，并解释你提到的所有金融术语。
```

<a id="draft-and-refine-writing"></a>

### 起草和润色文字

```text
起草一封语气友好的邮件，以我届时要出行为由婉拒这次邀请。
将邮件控制在 120 词以内，并表达未来有机会参加活动的意愿。
```

<a id="compare-options"></a>

### 比较选项

```text
为一位每年出国旅行两次的用户比较这两个手机套餐。
用表格展示重要差异，然后推荐其中一个，并解释取舍。
```

<a id="make-a-practical-plan"></a>

### 制订实用计划

```text
安排五顿工作日晚餐，每顿烹饪时间少于 30 分钟。避免使用花生，
在不同餐食之间重复利用食材，最后给出一份合并后的购物清单。
```

<a id="prompting-for-work"></a>
<a id="prompting-in-work-mode"></a>

<a id="prompting-for-chatgpt-work"></a>

## 为 ChatGPT Work 编写提示词

使用 Chat 处理快速提问、简短改写、头脑风暴和简单草稿。对于需要调用不同信息来源或工具、涉及多个步骤、需要作出修改，或要生成较大交付物的任务，使用 ChatGPT Work。

在 ChatGPT Work 中，描述你需要的结果，提供源材料，说明受众，并解释你将如何审阅成果。让 ChatGPT 制订计划、收集所需信息、创建文件，并在完成前进行检查。

<a id="use-work-efficiently"></a>
<a id="use-work-mode-efficiently"></a>

<a id="use-chatgpt-work-efficiently"></a>

### 高效使用 ChatGPT Work

ChatGPT Work 适合耗时或重复性任务，也适合生成可重复使用的成品文件。即使某项任务消耗更多额度，只要能节省时间、提高质量或帮助你作出重要决策，仍可能值得。

从一个可以审阅的结果开始：

- 只包含相关来源，并在适当时限定日期范围。
- 明确受众、输出格式和期望长度。
- 区分必做工作与可选改进或润色。
- 如果方法很重要，就要求先提供计划。要求 ChatGPT 在发送、发布或修改他人依赖的信息之前获得你的批准。
- 如果任务开始执行你已不再需要的工作，就缩小范围或停止任务。

审阅首次结果，完善指令，并在流程有效时重复使用。

<a id="turn-source-material-into-finished-files"></a>

### 将源材料转化为成品文件

```text
使用附上的季度报告，创建一份领导层简报和一份六页幻灯片演示文稿。

受众是高管团队。先列出他们需要作出的三项决策，区分报告中的事实和你的分析，
为每个数字注明来源文件，并在完成前检查简报与幻灯片内容是否一致。
```

<a id="research-a-decision"></a>

### 为决策开展研究

```text
为一家 50 人的公司研究三个客户支持平台。使用最新来源，比较价格、安全性、
集成能力和迁移工作量。提交一份建议备忘录，包含链接、假设，
以及我们在签订合同前应回答的问题。
```

<a id="coordinate-a-launch"></a>

### 协调产品发布

```text
根据附上的产品简报创建发布计划。包含时间安排、负责人、依赖关系、风险、
公告草稿、客户常见问题解答，以及发布当天的检查清单。
在生成最终文件之前，标明所有尚未作出的必要决策。
```

对于重复性工作，先在普通对话中完善提示词。等输出稳定可靠后，再[在该对话中安排定时任务](automations.zh-CN.md#schedule-a-task-inside-a-chat)。如果每次定时运行都应开启一个新对话，则创建独立的定时任务。

<a id="use-editor-context"></a>

<a id="prompting-codex"></a>

## 为 Codex 编写提示词

当你希望 ChatGPT 处理代码、代码库或开发者工具时，使用 Codex。一条实用的 Codex 提示词应说明期望行为，指向相关代码或复现步骤，明确需要保留的重要约束，并说明如何验证修改。

<a id="goal-mode"></a>

对于多步骤任务，如果你希望 Codex 在编辑前先调查并提出方案，可以在应用输入框中输入 `/plan`。当[目标模式（Goal mode）](long-running-work.zh-CN.md)可用时，可以在计划完成后使用 `/goal` 设置持续推进的目标。当前命令列表请参阅[应用斜杠命令](https://learn.chatgpt.com/docs/reference/slash-commands)。

<a id="how-to-read-these-examples"></a>

### 如何阅读这些示例

每个工作流程都包含：

- **适用场景**，以及最适合的 Codex 使用界面（IDE、CLI 或云端）。
- **操作步骤**，包含用户提示词示例。
- **上下文说明**：哪些内容 Codex 会自动看到，哪些需要你附上。
- **验证方式**：如何检查输出。

> **注意：**IDE 扩展会自动将已打开的文件纳入上下文。在 CLI 中，请明确提及路径，或使用 `/mention` 和 `@` 路径自动补全来附加文件。

Codex 在[沙箱](sandboxing.zh-CN.md)内运行本地命令，沙箱会限制文件和网络访问。如果任务需要越过这一边界，Codex 会在继续之前遵循你的审批策略。

<a id="explain-a-codebase"></a>

### 解释代码库

当你刚加入项目、接手服务，或需要理解协议、数据模型或请求流程时，使用此流程。

<a id="ide-extension-workflow-fastest-for-local-exploration"></a>

#### IDE 扩展工作流程（本地探索最快的方式）

<WorkflowSteps>

1. 打开最相关的文件。
2. 选中你关心的代码（可选，但建议这样做）。
3. 向 Codex 输入提示词：

```text
   解释请求如何流经选中的代码。

   包含：
   - 简要概述所涉及的每个模块的职责
   - 验证了哪些数据，以及在哪里进行验证
   - 修改这部分代码时需要留意的一两个易错点
```

</WorkflowSteps>

验证方式：

- 要求提供你可以核查的示意图或检查清单：

```text
用编号步骤概述请求流程，然后列出涉及的文件。
```

<a id="cli-workflow-good-when-you-want-a-transcript--shell-commands"></a>

#### CLI 工作流程（适合需要对话记录和 shell 命令的情况）

<WorkflowSteps>

1. 启动交互式会话：

```bash
   codex
```

2. 附加文件（可选），然后输入提示词：

```text
   我需要理解该服务使用的协议。阅读 @foo.ts @schema.ts，解释数据结构以及请求/响应流程。重点说明必填与可选字段，以及向后兼容规则。
```

</WorkflowSteps>

上下文说明：

- 你可以在输入框中使用 `@` 插入工作区文件路径，或使用 `/mention` 附加特定文件。

<a id="fix-a-bug"></a>

### 修复缺陷

当你遇到可以在本地复现的异常行为时，使用此流程。

<a id="cli-workflow-tight-loop-with-reproduction-and-verification"></a>

#### CLI 工作流程（快速反复复现与验证）

<WorkflowSteps>

1. 在仓库根目录启动 Codex：

```bash
   codex
```

2. 向 Codex 提供复现步骤，以及你怀疑有问题的文件：

```text
   缺陷：在设置页面点击“保存（Save）”后，有时会显示“已保存（Saved）”，但修改并未持久保存。

   复现步骤：
   1) 启动应用：npm run dev
   2) 进入 /settings
   3) 切换“启用提醒（Enable alerts）”开关
   4) 点击“保存（Save）”
   5) 刷新页面：开关恢复原状

   约束：
   - 不要改变 API 的结构。
   - 尽量缩小修复范围，并在可行时添加回归测试。

   先在本地复现缺陷，再提出补丁并运行检查。
```

</WorkflowSteps>

上下文说明：

- 由你提供：复现步骤和约束（这些比笼统描述更重要）。
- 由 Codex 提供：命令输出、找到的调用位置，以及运行中触发的任何堆栈跟踪。

验证方式：

- Codex 应在修复后重新执行复现步骤。
- 如果你有标准检查流程，可以要求它运行：

```text
修复后，运行 lint 和最小范围的相关测试套件。报告所执行的命令及结果。
```

<a id="ide-extension-workflow"></a>

#### IDE 扩展工作流程

<WorkflowSteps>

1. 打开你认为存在缺陷的文件，以及最直接调用它的代码文件。
2. 向 Codex 输入提示词：

```text
   找出导致界面显示“已保存（Saved）”却没有持久保存修改的缺陷。提出修复方案后，告诉我如何在界面中验证。
```

</WorkflowSteps>

<a id="write-a-test"></a>

### 编写测试

当你希望明确定义测试范围时，使用此流程。

<a id="ide-extension-workflow-selection-based"></a>

#### IDE 扩展工作流程（基于选中内容）

<WorkflowSteps>

1. 打开包含目标函数的文件。
2. 选中定义该函数的代码行。在命令面板中选择“Add to Codex Thread（添加到 Codex 对话）”，将这些行加入上下文。
3. 向 Codex 输入提示词：

```text
   为这个函数编写单元测试，遵循其他测试中使用的约定。
```

</WorkflowSteps>

上下文说明：

- “Add to Codex Thread”命令提供的内容：选中的代码行（即按行号限定的范围），以及已打开的文件。

<a id="cli-workflow-path--line-range-described-in-prompt"></a>

#### CLI 工作流程（在提示词中描述路径和行范围）

<WorkflowSteps>

1. 启动 Codex：

```bash
   codex
```

2. 在提示词中指定函数名：

```text
   为 @transform.ts 中的 invert_list 函数添加测试，覆盖正常流程和边界情况。
```

</WorkflowSteps>

<a id="prototype-from-a-screenshot"></a>

### 根据截图制作原型

当你希望将设计稿、截图或界面参考转化为可运行原型时，使用此流程。

<a id="cli-workflow-image--prompt"></a>

#### CLI 工作流程（图像与提示词）

<WorkflowSteps>

1. 将截图保存在本地（例如 `./specs/ui.png`）。
2. 运行 Codex：

```bash
   codex
```

3. 将图像文件拖入终端，将其附加到提示词中。

4. 继续补充约束和结构要求：

```text
   根据这张图片创建一个新的仪表盘。

   约束：
   - 使用 react、vite 和 tailwind。使用 typescript 编写代码。
   - 尽可能贴近图片中的间距、字体排版和布局。

   输出：
   - 一个呈现该界面的新路由/页面
   - 所需的小型组件
   - README.md，包含本地运行说明
```

</WorkflowSteps>

上下文说明：

- 图像提供视觉要求，但你仍需要说明实现约束（框架、路由、组件风格）。
- 用文字补充图像没有展示的行为，例如悬停状态、验证规则或键盘交互。

验证方式：

- 要求 Codex 启动开发服务器（如果允许），并明确告诉你在哪里查看：

```text
启动开发服务器，并告诉我查看原型所需的本地 URL/路由。
```

<a id="ide-extension-workflow-image--existing-files"></a>

#### IDE 扩展工作流程（图像与现有文件）

<WorkflowSteps>

1. 在 Codex 对话中附加图像（拖放或粘贴）。
2. 向 Codex 输入提示词：

```text
   创建一个新的设置页面，以附上的截图作为目标界面。
   遵循本项目其他文件中的设计和视觉模式。
```

</WorkflowSteps>

<a id="iterate-on-ui-with-live-updates"></a>

### 通过实时更新迭代界面

当你希望在 Codex 编辑代码时，快速进行“设计 → 微调 → 刷新 → 再微调”的循环，使用此流程。

<a id="cli-workflow-run-vite-then-iterate-with-small-prompts"></a>

#### CLI 工作流程（运行 Vite，然后用简短提示词迭代）

<WorkflowSteps>

1. 启动 Codex：

```bash
   codex
```

2. 在单独的终端窗口中启动开发服务器：

```bash
   npm run dev
```

3. 用提示词让 Codex 修改：

```text
   为着陆页提出 2–3 项样式改进建议。
```

4. 选定方向后，使用简短、具体的提示词逐步迭代：

```text
   采用方案 2。

   只修改页头：
   - 让字体排版更有杂志编辑设计的风格
   - 增加留白
   - 确保在移动端仍然美观
```

5. 使用聚焦的请求继续迭代：

```text
   下一轮迭代：减少视觉干扰。
   保持布局不变，但简化配色并移除多余边框。
```

</WorkflowSteps>

验证方式：

- 在 Codex 更新代码时，在浏览器中检查变化。
- 提交你认可的修改，撤销你不满意的修改。
- 如果你撤销或调整了某项编辑，请告诉 Codex，以免它在处理下一条提示词时覆盖你的修改。

<a id="delegate-refactor-to-the-cloud"></a>

### 将重构委派给云端

当你希望利用本地上下文设计方案，再将耗时的实现工作委派给可并行运行的云端对话时，使用此流程。

<a id="local-planning-ide"></a>

#### 本地规划（IDE）

<WorkflowSteps>

1. 确保当前工作已提交，或至少已通过 stash 暂存，以便清晰比较修改。
2. 要求 Codex 制订重构计划。如果有可用的 `$plan` 技能，请明确调用：

```text
   $plan

   我们需要重构身份验证子系统，以实现：
   - 拆分职责（令牌解析、会话加载和权限处理）
   - 减少循环导入
   - 提高可测试性

   约束：
   - 不改变用户可见的行为
   - 保持公共 API 稳定
   - 提供逐步迁移计划
```

3. 审阅计划，并讨论需要的调整：

```text
   修改计划，补充：
   - 明确每个里程碑中具体移动哪些文件
   - 提供回滚策略
```

</WorkflowSteps>

上下文说明：

- 当 Codex 可以在本地扫描当前代码（入口点、模块边界、依赖关系图线索）时，规划效果最好。

<a id="cloud-delegation-ide--cloud"></a>

#### 云端委派（IDE → 云端）

<WorkflowSteps>

1. 如果尚未配置，请先设置 [Codex 云端环境](environments/cloud-environment.zh-CN.md)。
2. 点击提示词输入框下方的云朵图标，并选择你的云端环境。
3. 输入下一条提示词时，Codex 会在云端创建一个新对话，带上现有对话上下文（包括计划和本地源代码修改）。

```text
   实现计划中的里程碑 1。
```

4. 审阅云端差异，必要时继续迭代。

5. 直接从云端创建 PR（拉取请求），或将修改拉取到本地进行测试并完成收尾。

6. 按计划继续迭代其他里程碑。

</WorkflowSteps>

委派到云端的任务在隔离环境中运行。除非你为该环境启用互联网访问，否则在智能体执行阶段无法访问互联网。更多信息请参阅[云端互联网访问](cloud/internet-access.zh-CN.md)。

<a id="do-a-local-code-review"></a>

### 进行本地代码审查

当你希望在提交代码或创建 PR 之前获得另一份审查意见时，使用此流程。

<a id="cli-workflow-review-your-working-tree"></a>

#### CLI 工作流程（审查工作区中的代码）

<WorkflowSteps>

1. 启动 Codex：

```bash
   codex
```

2. 运行审查命令：

```text
   /review
```

3. 可选：提供自定义的审查重点：

```text
   /review 重点检查边界情况和安全问题
```

</WorkflowSteps>

验证方式：

- 根据审查反馈修复问题，然后重新运行 `/review`，确认问题已解决。

<a id="review-a-github-pull-request"></a>

### 审查 GitHub 拉取请求

当你希望在不将分支拉取到本地的情况下获取审查反馈时，使用此流程。

使用之前，需要在仓库中启用 Codex 的**代码审查（Code review）**。请参阅[代码审查](third-party/github.zh-CN.md)。

<a id="github-workflow-comment-driven"></a>

#### GitHub 工作流程（通过评论触发）

<WorkflowSteps>

1. 在 GitHub 上打开拉取请求。
2. 发表评论，提及 Codex 并明确审查重点：

```text
   @codex review
```

3. 可选：提供更明确的指令。

```text
   @codex review 检查安全漏洞和安全隐患
```

</WorkflowSteps>

<a id="update-documentation"></a>

### 更新文档

当你需要准确、清晰地修改文档时，使用此流程。

<a id="ide-or-cli-workflow-local-edits--local-validation"></a>

#### IDE 或 CLI 工作流程（本地编辑与本地验证）

<WorkflowSteps>

1. 确定要修改的文档文件，并打开它们（IDE），或使用 `@` 提及它们（IDE 或 CLI）。
2. 输入提示词，明确范围和验证要求：

```text
   更新“高级功能（advanced features）”文档，补充身份验证故障排查指南。验证所有链接是否有效。
```

3. Codex 起草修改后，审阅文档，并按需继续迭代。

</WorkflowSteps>

验证方式：

- 阅读渲染后的页面。
