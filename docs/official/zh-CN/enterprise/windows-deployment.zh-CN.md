> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/windows-deployment.md)。

<a id="deploy-the-windows-app"></a>

# Windows 应用部署

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

用户可以自行安装 ChatGPT 桌面应用程序，或者您的 IT 团队可以使用企业管理工具进行部署。该应用程序经过商店签名，但用户无需打开 Microsoft Store 即可安装或更新它。

<a id="let-users-install-and-update-the-app"></a>

## 让用户安装和更新应用程序

如果用户可以管理自己的应用程序，请将他们定向到 [网络安装程序](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi)。安装程序提供标准安装和自动更新体验。 Microsoft Store 组件可能会在安装或更新过程中出现，但用户不需要自己浏览 Store。

您还可以从命令行安装该应用程序：

```powershell
winget install --id 9PLM9XGG6VKS -s msstore
```

<a id="deploy-the-app-with-an-enterprise-management-tool"></a>

## 使用企业管理工具部署应用程序

如果您的组织集中管理软件，请使用 Microsoft Intune 或其他兼容的移动设备管理 (MDM) 或软件部署平台。如果你的平台支持 Microsoft Store 应用部署，请在应用商店应用流程中从 OpenAI 搜索 ChatGPT，或使用此应用商店产品 ID：

```text
9PLM9XGG6VKS
```

有关设置详细信息，请参阅以下 Microsoft 文档：

- [企业部署指南](https://1drv.ms/b/c/123ec1ed6c72a14a/IQDVdo5pE5P3QKg5r0eieSvfAeE7cW0yy58ncBFW7OYajwU?e=dGH94F)
- [Intune 部署指南](https://1drv.ms/b/c/123ec1ed6c72a14a/IQDh_5o31T6XT7bUn5RPldEJAZX58gEuRr8YnJD7d2IMpec?e=nByKw6)
- [MECM部署指南](https://1drv.ms/b/c/123ec1ed6c72a14a/IQB829f_TSbkR7-H9qA4Q9ntAa9D2He3qMjXksWi2ozdeg8?e=GTKgAl)
- [将 Microsoft Store 应用添加到 Microsoft Intune](https://learn.microsoft.com/en-us/intune/app-management/deployment/add-microsoft-store)

<a id="manage-in-app-updates"></a>

<a id="manage-app-updates"></a>

### 管理应用程序更新

有关设置说明和部署指南，请参阅 [管理应用程序更新](manage-app-updates.zh-CN.md)。

<a id="install-without-microsoft-distribution-services"></a>

## 无需 Microsoft 分发服务即可安装

如果你的环境无法使用 Microsoft 应用分发服务进行初始安装，请下载适用于每个设备体系结构的应用商店签名的 MSIX 包：

| 器件架构 | 封装 |
| ------------------- | ---------------------------------------------------------------------------------------- |
| x64 | [ChatGPT-x64.msix](https://persistent.oaistatic.com/codex-app-prod/ChatGPT-x64.msix) |
| Arm64 | [ChatGPT-arm64.msix](https://persistent.oaistatic.com/codex-app-prod/ChatGPT-arm64.msix) |

这些稳定的链接指向每个架构最新发布的商店签名包。对于需要许可证文件的离线部署工作流程，还需下载 [离线许可证（`ChatGPT-License.xml`）](https://persistent.oaistatic.com/codex-app-prod/ChatGPT-License.xml)。将适当的 MSIX 以及许可证文件（如果需要）提取到 MDM 或软件部署平台中。

初始安装后，可以到达 `persistent.oaistatic.com` 的设备可以自动安装更新，除非托管配置禁用应用程序的内置更新程序。如果您禁用应用内更新，请通过 MDM 或软件部署工具部署较新的软件包。

此部署路径：

- 支持在受限环境中进行初始安装。
- 支持 x64 和 Arm64 设备。
- 不提供独立的 MSI 或非商店 EXE。

<a id="related-resources"></a>

## 相关资源

- [管理应用程序更新](manage-app-updates.zh-CN.md)
- [ChatGPT Windows 桌面应用程序](../windows/windows-app.zh-CN.md)