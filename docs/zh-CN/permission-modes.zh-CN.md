<a id="permissions"></a>

# 权限模式操作指南

> 本文根据本地英文原文 [permission-modes.md](../en/permission-modes.md) 翻译，为非官方简体中文译文。

> 完整文档索引请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。在文档页面的网址末尾添加 `.md`，即可获取该页面的 Markdown 版本。

{/* vale Microsoft.FirstPerson = NO */}

<a id="permission-modes"></a>

## 权限模式

权限控制 ChatGPT（桌面应用）和 Codex（CLI 或 IDE）如何处理本地操作，例如编辑文件、运行命令和使用互联网。你选择的模式决定了 ChatGPT 可以自行执行哪些操作，以及哪些操作需要审核。

对于大多数工作，建议先使用**请求批准（Ask for approval）**。该模式允许 ChatGPT 在当前工作区内开展工作，并在超出这一边界之前暂停。

在下方选择不同模式，了解各模式的工作方式。

<PermissionModeSelectorDemo client:load />

<a id="enable-modes"></a>

## 启用模式

首次使用 ChatGPT 桌面应用时，你需要在应用设置中启用模式。

**请求批准（Ask for approval）**始终可用。要将**代我批准（Approve for me）**（在设置中称为**自动审核（Auto&#45;review）**）或**完全访问（Full access）**添加到权限菜单，请在 ChatGPT 桌面应用中打开**设置 > 常规（Settings > General）**，然后在**权限（Permissions）**下开启相应模式。启用某个模式只是让它出现在菜单中，并不会自动选中该模式，也不会更改现有对话。

> 插图：权限可见性控件，显示默认权限、自动审核和完全访问。

可用模式可能取决于你的本地配置和组织要求。不允许使用的模式会显示为禁用状态。

<a id="how-permissions-work"></a>

## 权限的工作方式

以下两种控制机制协同发挥作用：

- **沙箱（sandbox）**定义 ChatGPT 可以访问哪些文件和网络资源。
- **审批（approvals）**决定 ChatGPT 何时在执行操作前暂停，或何时将请求交给自动审核。

更改请求的审核方并不会扩大沙箱范围。例如，**代我批准（Approve for me）**与**请求批准（Ask for approval）**保持相同的工作区边界；它会将跨越该边界的请求交给自动审核。

在 ChatGPT 桌面应用或 IDE 扩展中，使用输入框下方的权限控件。

在 CLI 中，输入 `/permissions`。有关技术细节，请参阅[沙箱](sandboxing.zh-CN.md)、[自动审核](sandboxing/auto-review.zh-CN.md)或[权限配置](permissions.zh-CN.md)。
