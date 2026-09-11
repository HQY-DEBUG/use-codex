> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/quickstart.md)。

<a id="quickstart"></a>

# 快速开始

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="where-to-use-chatgpt"></a>

## ChatGPT在哪里使用

ChatGPT 可用于不同的使用界面，包括 [ChatGPT 桌面应用程序](app.zh-CN.md) 和 [网络上的 ChatGPT](web.zh-CN.md)。选择适合您工作的选项。



> 插图：卡片比较 ChatGPT 桌面应用程序和网络上的 ChatGPT



如果您是开发人员并且想要在终端或代码编辑器中使用 Codex，请尝试 [Codex CLI 入门](codex/cli.zh-CN.md) 或 [Codex IDE扩展](codex/ide.zh-CN.md)。

<a id="setup"></a>

## 设置

{/* prettier-ignore */}
<Tabs
  id="codex-quickstart-setup"
  param="setup"
  defaultTab="web"
  size="md"
  tabs={[
    { id: "app", label: "桌面" },
    { id: "web", label: "网络" },
  ]}
>
  

ChatGPT 桌面应用程序适用于 macOS、Windows 和 Linux。将其用于项目、本地文件、较长的任务和快速聊天。有关支持的 Linux 发行版和软件包安装，请参阅 [Linux 桌面应用程序指南](linux/linux-app.zh-CN.md)。

<WorkflowSteps variant="headings">
1. <h3 id="setup-app-install">安装 ChatGPT 桌面应用程序</h3>

选择适合您的操作系统的版本：

    <CodexAppDownloadCta client:load className="mb-4" />

2.  <h3 id="setup-app-sign-in">打开 ChatGPT 桌面应用程序并登录</h3>

打开应用程序，然后使用您的 ChatGPT 帐户登录。

您还可以将 Codex 与 API 密钥一起使用。 [某些功能可能不可用](pricing.zh-CN.md#feature-availability)。

3.  <h3 id="setup-app-select-workspace">选择 ChatGPT 应该工作的位置</h3>

开始聊天、创建项目或打开文件夹。 ChatGPT可以读取和修改您选择的文件夹中的文件。 [了解有关聊天和项目的更多信息](projects.zh-CN.md)。

4.  <h3 id="setup-app-start-task">开始聊天</h3>

            


          


                - 对于研究、分析或可交付成果（例如文档、演示文稿、电子表格和网站），请选择 **ChatGPT**，然后切换到新聊天页面顶部、编辑器上方的 **工作**。
                - 对于使用代码库上下文和开发人员工具进行软件开发，请从 ChatGPT 下拉列表中选择 **Codex**。
                - 如需快速提问或聊天，请选择 **ChatGPT**，然后在新聊天页面顶部、输入框上方的切换器中选择 **聊天**。在 Codex 中，指向 **新聊天**，然后选择其右侧的 **快速聊天** 图标。

了解有关 [使用ChatGPT](use-chatgpt.zh-CN.md) 的更多信息。

              


          <ChatGPTModeDropdown client:visible />

    


5.  <h3 id="setup-app-send-message">发送您的第一条消息</h3>

描述您的目标并添加 ChatGPT 需要的任何文件或上下文。尝试一个例子：

    

**准备一个决定：**

```text
查看此项目中的报告和注释，比较选项，并创建一页决策备忘录，其中包含建议、风险、开放性问题和源链接。
```

**分析电子表格：**

```text
合并此文件夹中的电子表格，清理不一致的记录，确定最重要的趋势，并创建包含图表和简单英语要点的简洁报告。
```

**改进这个应用程序：**

```text
检查此应用程序，确定一项高影响力的可用性改进，实施它，更新相关测试，并在移动和桌面上验证结果。
```

了解更多 [用例](https://learn.chatgpt.com/use-cases)。

</WorkflowSteps>

  


  

ChatGPT 可在网络上获取，包括聊天和 ChatGPT Work。

<WorkflowSteps variant="headings">
1. <h3 id="setup-web-sign-in">打开ChatGPT并登录</h3>

转到 [聊天网站](https://chatgpt.com) 并使用您的 ChatGPT 帐户登录。

2.  <h3 id="setup-web-start-task">开始聊天</h3>

            


          


                - 选择 **聊天** 提出问题、探索想法并以对话方式解决某个主题。
                - 选择 **工作** 来研究、分析信息并创建文档、演示文稿、电子表格、网站或其他完成的作品。

了解有关 [使用ChatGPT](use-chatgpt.zh-CN.md) 的更多信息。

              


          <ChatWorkSegmentPicker client:visible />

    


3.  <h3 id="setup-web-select-workspace">选择 ChatGPT 应该工作的位置</h3>

开始聊天或选择一个项目。项目可以包括聊天、文件和说明。

4.  <h3 id="setup-web-send-message">发送您的第一条消息</h3>

描述您的目标并添加 ChatGPT 需要的任何文件或上下文。尝试一个例子：

    

**做出决定：**

```text
研究我是否应该 [decision]，比较最佳选项，解释针对我的情况的权衡，并推荐一个带有引用的选项。
```

**每日简报：**

```text
每个工作日上午 8:00，查看我关联的日历和最近的消息，然后向我发送一份简报，其中包含今天的优先事项、会议准备、我应回复的内容以及阻碍因素。
```

**计划一个活动：**

```text
帮助我计划我的活动。询问我有关场合、嘉宾、日期、地点、预算以及您需要的任何其他信息。然后创建时间表、预算、邀请副本和清单，并发布一个我可以用来邀请客人和收集回复的网站。
```

</WorkflowSteps>

  


</Tabs>



<a id="next-steps"></a>

## 后续步骤
[了解有关 ChatGPT 桌面应用程序的更多信息



      <OpenBook />
    

使用 ChatGPT 桌面应用程序处理您的本地项目。](https://learn.chatgpt.com/docs/app) [导入您的设置



      <CompareArrows />
    

将支持的设置、项目和最近的工作引入 ChatGPT.](https://learn.chatgpt.com/docs/import)