> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/manage-app-updates.md)。

<a id="manage-app-updates"></a>

# 应用更新管理

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

ChatGPT 桌面应用程序通常会自行检查并安装更新。如果您的组织需要在用户收到新版本之前对其进行审核，您可以关闭应用程序的内置更新程序并通过设备管理平台部署批准的版本。

该应用程序的更新程序默认保持启用状态。关闭它不会阻止 Microsoft Store、Microsoft Intune、移动设备管理 (MDM)、包管理器或其他外部部署工具安装更新。

<a id="before-you-begin"></a>

## 开始之前

确认您拥有：

- Codex 管理员对您工作区的 [受管配置](https://chatgpt.com/codex/settings/managed-configs) 的访问权限。
- 适用于 macOS 或 Windows 的 ChatGPT 桌面应用程序版本，支持组织管理的更新。
- 可以在托管设备上安装批准的应用程序包的 MDM 或软件部署平台。
- 用于测试新版本、部署安全更新和跟踪已安装应用程序版本的流程。

如果您尚未在 Windows 上部署该应用程序，请从 [部署 Windows 应用程序](windows-deployment.zh-CN.md) 开始。

<a id="turn-off-in-app-updates"></a>

## 关闭应用内更新

<WarningTip>
当您关闭应用程序内更新时，您的组织负责及时部署新的应用程序版本和安全修复程序。延迟更新可能会使应用程序及其捆绑组件暴露于已知的安全漏洞。较旧的应用程序版本不会收到单独的安全补丁或扩展支持。
</WarningTip>

创建禁用桌面应用程序自己的更新程序的托管策略：

1. 打开[受管配置](https://chatgpt.com/codex/settings/managed-configs)。
2. 选择 **添加政策**，或为要管理的用户、组或平台打开现有策略。
3. 在“**目标**”下，选择“**添加目标**”以将策略分配给特定的 **团体**、**用户** 或 **平台**。如果可能的话，从一个小的试点小组开始。
4. 打开**原始TOML**并找到**需求.toml**编辑器。
5. 添加以下策略：

```toml
   [features]
   in_app_updates = false
```

如果您的策略已包含 `[features]` 表，请将 `in_app_updates = false` 添加到该表。不要添加第二个 `[features]` 表或将设置放入 **配置文件** 中。

6. 选择**保存更改**。
7. 要求受影响的用户完全退出并重新打开 ChatGPT 桌面应用程序。关闭应用程序窗口并不总是足以重新启动应用程序。

某些工作区显示策略列表编辑器而不是 **原始TOML** 选项卡。在该界面中，将相同的 TOML 块直接添加到适用的策略中，使用 **团体** 对其进行分配（如果可用），然后选择 **保存**。

有关托管策略下发和优先级的详细信息，请参阅[受管配置](managed-configuration.zh-CN.md)。

<a id="verify-the-managed-setting"></a>

## 验证托管设置

应用重新启动后，从受影响用户的设备验证策略：

1. 使用该策略涵盖的帐户登录 ChatGPT 桌面应用程序。
2. 打开 **设置** > **一般**。
3. 找到 **应用内更新** 并确认它显示 **托管** 和消息“您的组织已关闭应用内更新”。
4. 确认您的设备管理平台仍然可以部署已批准的应用程序版本。

即使策略阻止应用内更新，**检查更新** 菜单选项也可以保持可见。使用 **托管** 指示器来验证策略，而不是检查该菜单选项是否出现。

如果首次重新启动后未出现该指示器，则应用程序可能仍使用缓存的策略。允许刷新策略，然后完全退出并再次重新打开应用程序。在 **托管** 出现之前，请勿依赖更新限制。

<a id="deploy-approved-app-versions"></a>

## 部署批准的应用程序版本

关闭应用内更新后，使用现有的设备管理流程来交付新版本：

1. 选择您的组织计划部署的应用程序版本。
2. 获取适用于您的设备组中的每个操作系统和设备架构的受支持的安装包。
3. 与一小群有代表性的用户一起测试该版本。
4. 通过 Microsoft Intune、MDM 平台或其他软件部署工具部署批准的包。
5. 检查设备清单以确认您的平台安装了预期版本，然后将部署扩展到其他组。

您的管理平台决定了您如何暂存版本、选择版本以及在部署未完成时进行恢复。如果您的平台允许回滚，则返回到旧版本并不能扩展支持或保证服务兼容性。

对于 macOS，请下载 [ChatGPT 桌面应用程序安装程序](https://persistent.oaistatic.com/codex-app-prod/ChatGPT.dmg)。有关 Windows 安装方法和特定于体系结构的软件包，请参阅 [部署 Windows 应用程序](windows-deployment.zh-CN.md)。

<a id="turn-in-app-updates-back-on"></a>

## 重新打开应用内更新

要恢复应用程序的正常更新行为：

1. 确定关闭受影响用户更新的托管策略、系统 `requirements.toml` 文件和 MDM 配置文件。
2. 从每个适用的 `[features]` 表中删除 `in_app_updates = false`。
3. 保存策略更改并重新部署任何更新的设备管理要求。
4. 要求受影响的用户完全退出并重新打开 ChatGPT 桌面应用程序。
5. 检查 **设置** > **一般** 以确认 **应用内更新** 托管行不再出现。

当没有适用的策略设置 `in_app_updates = false` 时，应用程序的内置更新程序将遵循其正常行为。如果 **托管** 指示符仍然出现，请查看其他工作区策略、MDM 配置文件和系统 `requirements.toml` 文件。有关托管源的应用顺序，请参阅 [位置和优先级](managed-configuration.zh-CN.md#locations-and-precedence)。

<a id="understand-security-and-support-responsibilities"></a>

## 了解安全和支持责任

应用程序收到并应用它后，托管更新策略：

- 阻止桌面应用程序通过其自己的更新程序检查、下载或安装更新。
- 不提供 OpenAI 管理的版本固定、单独的发布通道或保证旧版本的服务兼容性。
- 适用于受支持的 macOS 和 Windows 版本上的 ChatGPT 桌面应用程序。它不管理移动应用程序、Codex CLI 或 IDE 扩展的更新。

<a id="troubleshoot-common-issues"></a>

## 解决常见问题

如果身份验证问题、连接问题或超时阻止应用程序检索或应用托管策略，则其内置更新程序可以保持启用状态。除非出现 **托管**，否则不要假设应用程序会阻止更新。

如果 **托管** 指示器未出现，请确认：

- 受影响的用户选择了预期的工作区。
- 该策略针对该用户、组或平台。
- 设备运行受支持的应用程序版本。
- 该应用程序可以连接到提供托管策略的服务。
- 设置在 **需求.toml** 中，而不是 **配置文件** 中。
- 保存策略后，用户完全退出并重新打开应用程序。

如果您无法打开托管配置或保存策略，请确认您拥有工作区的 Codex 管理员访问权限。

如果禁用应用内更新后应用版本发生变化，请检查 Microsoft Store、Intune、MDM、包管理器或其他部署系统是否安装了更新。该策略仅控制应用程序的内置更新程序。

<a id="related-docs"></a>

## 相关文档

- [受管配置](managed-configuration.zh-CN.md)
- [部署 Windows 应用程序](windows-deployment.zh-CN.md)
- [`requirements.toml`配置参考](../config-file/config-reference.zh-CN.md#requirementstoml)
- [管理员推出指南](admin-setup.zh-CN.md)