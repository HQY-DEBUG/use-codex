> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/browser.md)。

<a id="browser"></a>

# 内置浏览器

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<ContentModeSwitch group="codex-surface" ids="cli,ide">

浏览器在 Codex CLI 或 Codex IDE 扩展中不可用。打开 ChatGPT 桌面应用程序以使用内置浏览器。

</ContentModeSwitch>

浏览器可让 ChatGPT 打开网站、收集当前信息并在您保持控制的同时采取行动。使用它来比较选项、在网站上完成多步骤任务或查看您正在构建的页面。

浏览器可在 ChatGPT 网页版和 ChatGPT 桌面应用程序中使用。

[GPT-6 阿斯特拉](models.zh-CN.md#gpt-6-astra) 改进了任务的视觉判断，例如根据屏幕截图检查页面或完成跨站点的工作流程。在模型选择器中可用时选择它，并描述如何验证最终结果。

对于托管桌面环境，管理员可以限制浏览器来源、上传、下载和开发人员访问。参见 [托管浏览器控件](enterprise/managed-configuration.zh-CN.md#control-browser-and-computer-use)。

将页面内容视为不受信任的上下文。在共享敏感信息或允许 ChatGPT 采取行动之前，请先查看网站和建议的行动。

<ContentModeSwitch group="codex-surface" ids="app,cli,ide">

ChatGPT 桌面应用程序中的内置浏览器可让您和 ChatGPT 在聊天中共享网站和本地 Web 应用程序视图。使用它来预览页面、留下视觉反馈或让 ChatGPT 代表您与网站进行交互。

内置浏览器使用与常规浏览器不同的浏览器配置文件。它不会自动共享您现有的选项卡或浏览器会话。当任务需要帐户时，您可以直接登录。打开 **设置 > 浏览器** 来管理浏览器数据和设备上可用的任何配置文件导入功能。

默认情况下，浏览器下载会转到系统的“下载”文件夹。在**设置 > 浏览器**中，您可以选择其他下载位置，将其重置为系统默认值，或打开**询问在哪里保存下载的内容**。

当 ChatGPT 需要在现有 Chrome、Edge、Brave、Opera 或 Vivaldi 选项卡中工作或使用常规浏览器配置文件时，请使用 [浏览器扩展](chrome-extension.zh-CN.md)。

从工具栏打开内置浏览器，方法是单击 URL、手动导航或按<kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd>
(<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd>在 Windows 上）。


  

> 插图：ChatGPT 桌面应用程序在本地 Web 应用程序预览上显示浏览器评论




<a id="search-from-the-address-bar"></a>

## 从地址栏搜索

开始在内置浏览器的地址栏中输入内容，以从其浏览历史记录中查找页面。选择匹配的页面重新打开它，或者在没有历史记录结果匹配时输入搜索词来搜索 Google。

内置浏览器保留自己的配置文件和浏览历史记录。结果不会自动包含您的常规 Chrome 配置文件或其他浏览器中的页面。

<a id="manage-browsing-history"></a>

## 管理浏览历史记录

打开 **设置 > 浏览器** 来搜索内置浏览器的历史记录、重新打开访问过的页面或在组织允许的情况下删除历史记录条目。使用 **清除浏览数据** 选择时间范围和要删除的浏览数据类型。

如果可用，ChatGPT 可以要求搜索您的浏览历史记录以查找与当前任务相关的页面。在允许访问之前查看请求。浏览历史记录可以包括内部 URL、搜索词和其他敏感信息，因此仅当任务需要该上下文时才允许。

<a id="browser-use"></a>

<a id="computer-use-in-the-browser"></a>

## 计算机在浏览器中使用

在桌面应用程序中，“计算机使用”可让 ChatGPT Work 或 Codex 直接操作内置浏览器。所选体验可以打开页面、单击、键入、检查渲染状态、截取屏幕截图并验证其在页面中的工作结果。

浏览器包含在桌面应用程序中并自动安装。要求 ChatGPT 或 Codex 在任务中使用内置浏览器，或直接使用 `@Browser` 引用它。

例如：

```text
使用浏览器打开http://localhost:3000/settings,重现布局
bug，并仅修复溢出的控件。
```

ChatGPT 在使用网站之前会询问，除非您已经允许该网站。管理 **设置 > 浏览器** 中允许和阻止的站点。 ChatGPT 还要求在提交信息、购买、更改权限或删除数据等敏感操作之前进行确认。 ChatGPT 无法在内置浏览器中自动上传文件。

页面上的说明可能具有误导性或恶意。网站权限允许 ChatGPT 与该网站交互；它不会使网站的内容值得信赖或批准每项操作。

<a id="preview-a-page"></a>

## 预览页面

1. 在 [综合终端](integrated-terminal.zh-CN.md) 或 [地方环境行动](environments/local-environment.zh-CN.md#actions) 中启动应用程序的开发服务器。
2. 通过单击 URL 或在浏览器中手动导航来打开本地路由、文件支持的页面或公共页面。
3. 与代码差异一起查看渲染状态。
4. 在需要更改的元素或区域上留下浏览器评论。
5. 请 ChatGPT 解决这些意见并缩小范围。

例如：

```text
我在内置浏览器的定价页面上留下了评论。地址手机
布局问题并保持卡片结构不变。
```

<a id="comment-on-the-page"></a>

## 在页面发表评论

当错误仅在呈现的页面中可见时，使用浏览器注释为 ChatGPT 提供精确的反馈。

1. 打开**注释方式**。
2. 单击一个元素，或拖动以选择一个区域。
3. 写下并保存您的评论。
4. 在聊天中发送消息，要求 ChatGPT 处理评论。

当你说出问题和你想要的结果时，评论效果最好：

```text
此按钮在移动设备上溢出。如果合适的话，将标签保留在一行上，
否则在不改变卡片高度的情况下包裹它。
```

```text
该工具提示覆盖了光标下方的数据点。重新定位工具提示
它保持在图表范围内。
```

<section class="feature-grid">




<a id="styling-feedback"></a>

### 造型反馈

当您向页面上的某个部分添加注释时，选择文本输入旁边的 **调整** 以提供 ChatGPT 更精细的样式反馈。您可以更改字体、文本、间距和颜色等值，在页面上预览结果，然后发送具有更清晰目标的注释。





  

> 插图：ChatGPT 桌面应用程序显示内置浏览器注释样式控件




</section>

<a id="keep-browser-tasks-scoped"></a>

## 保持浏览器任务的范围

保持每个浏览器任务足够小，以便一次性查看。

- 命名页面、路由或 URL。
- 命名您关心的状态，例如加载、空、错误或成功。
- 对需要更改的具体元素或区域发表评论。
- ChatGPT 完成后再次查看该页面。
- 要求 ChatGPT 在打开本地页面之前启动或检查开发服务器。

对于仓库更改，请使用 [审阅窗格](code-review.zh-CN.md) 检查更改并留下评论。

<section class="feature-grid">




<a id="developer-mode"></a>

## 开发者模式

开发者模式适用于 Chrome 和内置浏览器中的计算机使用。它为 ChatGPT 提供对 Chrome DevTools 协议 (CDP) 的受控访问。使用它来分析 JavaScript、检查控制台输出和网络流量、检查 DOM 和应用的样式，或诊断实时浏览器中的问题。

要启用它，请打开 [**设置 > 浏览器**](codex://settings/browser-use)，然后在 **开发者模式** 下打开 **启用完全 CDP 访问**。如果您的组织已禁用此设置，您将无法在本地启用它。管理员可以在 [`requirements.toml`](enterprise/managed-configuration.zh-CN.md#pin-feature-flags) 中的 `[features]` 下设置 `browser_use_full_cdp_access = false`，以禁用完全 CDP 访问并阻止用户在 ChatGPT 桌面应用程序中启用相应设置。

完全 CDP 访问可能会暴露敏感的浏览器内部结构。 ChatGPT 在使用完整 CDP 检查网站之前要求明确批准。在批准之前检查站点、任务和请求的访问权限。

使用 `@Browser` 作为内置浏览器。要在 Chrome 中使用开发者模式，[设置 Chrome 扩展程序](chrome-extension.zh-CN.md) 并调用 `@Chrome`。

例如：

```text
这个应用程序很慢。使用 @Browser 捕获性能跟踪并检查
网络流量，然后识别瓶颈。
```





  

> 插图：ChatGPT 桌面应用程序 浏览器设置显示启用了完全 CDP 访问的开发人员模式




</section>

</ContentModeSwitch>

<ContentModeSwitch group="codex-surface" id="web">

<a id="use-chatgpt-work-to-get-things-done-across-the-web"></a>

## 使用 ChatGPT Work 通过网络完成工作

ChatGPT Work可以跨网站完成任务，包括需要登录的网站。

Work 使用自己的浏览器，在云中的单独计算机上运行，而不是手机或笔记本电脑上的浏览器。

在网络或移动设备上从 ChatGPT Work 启动任务，即使您离开并关闭计算机，ChatGPT 也可以继续工作。 Work 可以使用其计算机，通过阅读、单击和输入网页来完成互联网上的各种任务。根据您的请求，它可能会使用插件、浏览器或两者。

例如，ChatGPT可以帮助您：

- 查找并预约 DMV 预约。
- 登录您的公用事业帐户并比较计划。
- 查找并保存符合您条件的公寓。
- 在社交媒体上研究竞争对手。
- 关闭会计软件中的账簿。

您可以控制 ChatGPT 可以访问哪些网站，并且经过训练，它会在完成预订或付款等后续操作之前要求确认。如果 ChatGPT 因任何原因被阻止，您可以接管其计算机并在移动设备和桌面设备上自行使用。

Plus 和 Pro 计划的 Web 和移动设备上提供 ChatGPT Work 导航到需要身份验证的网站的功能。

可用性取决于部署。网站登录不适用于 Enterprise 或 Edu 工作区。

<a id="how-chatgpt-works-computer-works"></a>

## ChatGPT Work的电脑如何工作

当您的任务需要网站时，ChatGPT 使用自己的浏览器来导航页面、收集信息并在线完成步骤。

默认情况下，ChatGPT 在访问新网站之前会询问。您可以选择单独批准请求或调整您的设置，让 ChatGPT 自动批准与您的任务相关的网站。 ChatGPT Work 始终会在采取后续行动（例如提交您的信息进行预约或完成付款）之前要求您进行确认。

<a id="sign-in-to-a-website"></a>

## 登录网站

如果网站要求您登录，ChatGPT Work 会要求您登录。您验证后，它将继续在登录的网站上工作。您的会话将针对未来的任务保持活动状态，因此您无需每次都登录。

<a id="use-the-secure-sign-in-form"></a>

### 使用安全登录表单

ChatGPT 无法看到您的用户名或密码，并且模型永远不会看到它们或在模型训练中使用它们。 ChatGPT 不存储您的用户名或密码。您可以随时删除 **设置** > **云浏览器** > **浏览器数据** 中所有站点或单个站点的浏览历史记录，这将使您从该站点注销。

当 ChatGPT 遇到登录屏幕时，它会暂停并要求您根据需要输入凭据和两步验证码。在 iOS 上，您可以使用支持的密码管理器无缝登录。

使用 ChatGPT 提供的登录表格。不要在聊天中发送密码。

![iOS 上的 ChatGPT Work 暂停 DMV 任务并显示包含网站地址和屏蔽密码的安全登录表单。](https://developers.openai.com/images/codex/cloud-browser-auth/sign-in.webp)

<a id="sign-in-on-the-web-page"></a>

### 在网页上登录

如果提供，请选择 **请改为在网页上登录** 直接在云浏览器中登录。登录时任务会暂停。选择 **我完成了** 将控制权返回给 ChatGPT，或者跳过或取消请求。

<a id="start-a-browser-task"></a>
<a id="start-browser-work"></a>
<a id="web-start-browser-work"></a>

<a id="how-to-get-started-with-a-task-in-chatgpt-work"></a>

## 如何开始执行 ChatGPT Work 中的任务

1. 在网络或移动设备上打开 ChatGPT 并在 Work 中启动任务。
2. 描述您希望 ChatGPT 做什么。
3. 如果出现提示，请批准网站访问。
4. 如果网站需要，请直接登录。
5. 在对话中关注任务的进度。
6. 查看结果并批准任何后续行动。

您无需单独选择浏览器。 ChatGPT根据您的要求决定何时使用它。

有些网站会阻止访问。如果发生这种情况，ChatGPT 会通知您，并在可能的情况下尝试其他方法来完成任务。

<a id="website-permissions-and-confirmations"></a>
<a id="web-website-permissions-and-confirmations"></a>

<a id="security-and-user-controls"></a>

## 安全和用户控制

在ChatGPT设置中，打开**云浏览器**来管理网站权限。可用选项包括：

- **总是问**：手动审核每个网站访问请求。
- **自动批准**：让 ChatGPT 在检查网站与您的任务的相关性后自动批准访问。
- **始终允许**：允许网站访问，无需额外的审核步骤。我们提供此选项是为了最小化摩擦，但不推荐此选项。

![云浏览器设置显示始终询问、自动批准和始终允许网站权限选项。](https://developers.openai.com/images/codex/cloud-browser-auth/website-permissions.webp)

您还可以允许或阻止个别网站覆盖您的默认权限。

在 ChatGPT 要求您登录任何网站之前，额外的审核模型会检查登录请求以及您输入的信息是否存在网络钓鱼或欺骗迹象。我们测试智能体的风险，包括即时注入、网络钓鱼和意外操作。

为了完全透明，您将看到网站的地址及其登录表单的预览，并且您可以在继续之前检查实时网站。通过安全登录表单输入的凭据会直接进入浏览器，并且对模型不可见。

<a id="browser-data"></a>
<a id="web-browser-data"></a>

<a id="privacy-and-browser-data"></a>

## 隐私和浏览器数据

ChatGPT Work 的计算机与您设备上的浏览器分开运行。它维护自己的 cookie、浏览器数据和登录会话。 ChatGPT 在完成任务时使用的信息遵循您选择的 ChatGPT 数据控制设置。您可以在 ChatGPT 网络和移动设备中的 **设置** > **数据控制** 下查看这些内容。

它不会使用您个人浏览器的打开选项卡、浏览历史记录、保存的密码、cookie、扩展程序或现有的登录会话。

要清除浏览器数据，请转至 **设置** > **云浏览器** > **浏览器数据** > **全部清除**。这会将您从 ChatGPT Work 浏览器中的网站注销，因此您需要重新登录才能执行将来的任务。

![云浏览器设置，包含浏览器数据部分和 Cookie 控件，用于管理云浏览器保存的 cookie。](https://developers.openai.com/images/codex/cloud-browser-auth/browser-data.webp)

<a id="limitations"></a>

## 局限性

- 网站登录并非在每个工作区或部署中都可用。如果任务需要不受支持的登录方法，请自行完成该步骤或使用其他可用工具。
- 有些网站会阻止自动浏览器或需要验证码。 ChatGPT 可能无法在这些站点上完成任务。
- 云浏览的可用性可能取决于您的计划、工作区设置和部署。除了 Free and Go 之外，所有地区的付费套餐均提供云浏览功能。企业管理员必须为其工作区启用云浏览。

在推出期间，即使您的计划支持浏览器，浏览器也可能不会立即显示。

</ContentModeSwitch>