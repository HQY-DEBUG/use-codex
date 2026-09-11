> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/enterprise/plugin-management.md)。

<a id="plugin-management"></a>

# 插件市场管理

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

<a id="before-you-begin"></a>

## 开始之前

工作区管理员可以从 GitHub 导入插件市场，并从仓库中保持其插件最新。市场是一个 JSON 目录，列出了要导入的插件。

此页面涵盖工作区导入和同步。要通过云托管或系统 `config.toml` 直接在本地客户端上配置市场，请参阅 [配置插件市场和默认值](managed-configuration.zh-CN.md#configure-plugin-marketplaces-and-defaults)。要启用或禁用特定项目的插件，请参阅 [启用或禁用仓库的插件](https://developers.openai.com/plugins/build/plugins#enable-or-disable-a-plugin-for-a-repo)。

使用可以读取市场仓库及其引用的任何其他仓库的 GitHub 帐户。支持公共和私有 GitHub 仓库。在导入之前完成仓库访问所需的任何 GitHub 组织批准。

导入前检查仓库内容。新插件从 **可用** 安装和安装时身份验证开始。新市场已启用每日自动同步。导入会处理所有有效条目，并且将来的同步会自动在仓库中添加任何新插件。

<a id="configure-a-marketplace-sync"></a>

## 配置市场同步

1. 打开 **管理员** > **插件** 并选择 **添加** > **进口市场**。
2. 在“**来源**”中，输入仓库 URL，例如 `https://github.com/example/team-plugins`。仅使用仓库 URL，而不使用分支或文件夹 URL。
3. 如果市场位于子目录中，请在 **路径** 中输入该目录。例如，使用 `team-tools` 作为 `team-tools/.agents/plugins/marketplace.json`。将仓库根目录的 **路径** 留空。不要输入清单文件名。
4. （可选）输入 **分支、标记或提交**。将此留空以使用仓库的默认分支。使用分支接收未来的提交；固定提交保留在该修订版本中。
5. 选择 **进口市场** 并在出现提示时授权 GitHub 访问。对于非常大的市场，初始导入可能需要长达一个小时。随后的每日同步通常需要几分钟。
6. 查看 **导入结果**，然后打开每个导入的插件以配置其安装策略和任何所需的应用程序。

要请求更新而不等待每日同步，请打开 **管理员** > **插件** > **市场** 下的市场，然后选择 **立即同步**。

<a id="supported-formats"></a>

## 支持的格式

所选目录必须包含以下文件之一：

| 文件 | 格式 |
| ---------------------------------- | -------------------------------------------------------------------- |
| `.agents/plugins/marketplace.json` | 具有 `plugins` 阵列的 Codex 市场。                          |
| `.claude-plugin/marketplace.json` | 具有 `plugins` 阵列的 Claude 兼容市场。              |
| `.claude-plugin/plugin.json` | 一个独立的 Claude 插件，当市场清单不存在时。 |

在市场中，条目可以引用带有 `.codex-plugin/plugin.json` 的本机插件、Claude 兼容插件、Agent Plugins 1.0 包或支持的技能包。

对于 Codex 市场，请使用同一仓库中插件的本地路径：

```json
{
  "name": "team-plugins",
  "interface": {
    "displayName": "Team plugins"
  },
  "plugins": [
    {
      "name": "team-tools",
      "source": {
        "source": "local",
        "path": "./plugins/team-tools"
      }
    }
  ]
}
```

该路径是相对于所选市场根的，而不是相对于 `.agents/plugins/`。

与 Claude 兼容的市场可以为每个本地插件使用路径字符串：

```json
{
  "name": "team-plugins",
  "plugins": [
    {
      "name": "team-tools",
      "source": "./plugins/team-tools"
    }
  ]
}
```

Codex 市场条目还支持 GitHub 仓库根目录中的插件的 `source: "url"` 和 GitHub 子目录中的插件的 `source: "git-subdir"`。例如：

```json
{
  "name": "team-tools",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/example/team-tools.git",
    "path": "./plugins/team-tools",
    "ref": "main"
  }
}
```

Git 源可以选择 `ref` 或完整的 40 个字符提交 `sha`。授权 GitHub 帐户必须能够读取每个引用的仓库。工作区导入当前仅支持 GitHub 仓库。

<a id="configure-workspace-access"></a>

## 配置工作区访问

GitHub 导入和同步不应用仓库安装或身份验证策略，包括 `AVAILABLE`、`INSTALLED_BY_DEFAULT`、`NOT_AVAILABLE`、`ON_INSTALL` 和 `ON_USE`。工作区管理员为每个插件配置这些设置。将更新同步或将现有插件移动到 GitHub 管理会保留其工作区策略。

使用 **安装政策** 为每个符合条件的角色选择 **可用** 或 **已安装**。还必须启用所需的应用程序，并且成员必须有权访问连接的服务。导入插件不会授予应用程序访问权限或连接成员帐户。有关角色、应用程序和操作控件，请参阅 [插件控件](apps-and-connectors.zh-CN.md)。

<a id="move-an-existing-plugin-to-github-management"></a>

## 将现有插件移至 GitHub 管理

将 `pluginId` 添加到现有插件的市场条目：

```json
{
  "name": "team-tools",
  "pluginId": "plugin_0123456789abcdef0123456789abcdef",
  "source": {
    "source": "local",
    "path": "./plugins/team-tools"
  }
}
```

从 **管理员** > **插件** 打开插件，然后复制其 URL 中 `/admin/plugins/` 后面的 ID。将 `pluginId` 放在市场条目中的 `name` 和 `source` 旁边。现有插件必须位于同一工作区中。

这会将已上传或其他非托管工作区插件移至 GitHub 管理。该插件保留其 ID、共享和工作区策略。未来的更新来自GitHub；存档上传不能再替代托管插件。已由另一个 GitHub 源管理的插件无法以这种方式接管。

<a id="desktop-only-plugins"></a>

## 仅桌面插件

任何在 `mcp.json` 或 `.mcp.json` 中声明 MCP 服务器的导入插件都标记为 **仅限桌面版**，并且仅在 ChatGPT 桌面应用程序中工作。这包括使用远程 HTTPS URL 的服务器。同样的限制适用于其他支持的 MCP 配置形式，例如内联服务器声明。

<a id="reference-an-existing-app-with-appjson"></a>

## 使用 `.app.json` 引用现有应用程序

在插件根目录添加 `.app.json`。文件名包含一个前导点；不支持不带点的 `app.json`。

```json
{
  "apps": {
    "team-tools": {
      "id": "asdk_app_example",
      "required": true
    }
  }
}
```

将 `asdk_app_example` 替换为现有应用程序的 ID。支持的应用程序 ID 以 `asdk_app_`、`connector_` 或 `templated_apps_` 开头。使用应用程序 ID，而不是 `plugin_...` ID。例如，包含 `plugin_asdk_app_example` 的插件 URL 代表应用程序 `asdk_app_example`。

密钥 `team-tools` 命名此文件中的引用。当插件依赖于应用程序时，将 `required` 设置为 `true`。您可以添加更多条目来引用其他现有应用程序。

对于本机插件，请将 `.codex-plugin/plugin.json` 中的 `apps` 设置为 `./.app.json`。以下是此示例的完整清单：

```json
{
  "name": "team-tools",
  "version": "1.0.0",
  "description": "Use the team's approved tools.",
  "author": {
    "name": "Example team"
  },
  "apps": "./.app.json",
  "interface": {
    "displayName": "Team tools",
    "shortDescription": "Use approved team tools",
    "longDescription": "Connect to the team's existing app.",
    "developerName": "Example team",
    "category": "Productivity",
    "capabilities": ["Read"]
  }
}
```

将文件保留在以下布局中：

```text
team-plugins/
├── .agents/plugins/marketplace.json
└── plugins/team-tools/
    ├── .codex-plugin/plugin.json
    └── .app.json
```

该引用不会创建应用程序或授予权限。管理员必须使应用程序可供预期角色使用，并且成员必须完成任何所需的身份验证。现有的应用程序权限、操作控制和服务访问权限仍然适用。

<a id="keep-plugins-up-to-date"></a>

## 保持插件最新

新市场每天都会检查更新。打开 **管理员** > **插件** > **市场**，选择市场，然后选择 **立即同步** 请求更新，无需等待自动同步。

同步可以添加新的市场条目并更新现有插件。在合并之前检查对仓库的更改，因为自动同步将导入任何新插件。

同步后，查看状态和保存的报告。 **已完成 — N 个错误** 表示通行证已完成，但某些插件无法处理。如果现有插件的更新无效，则保留其最后的工作版本。修复 GitHub 中报告的问题，然后选择 **立即同步** 重试。

从仓库中删除条目不会删除其导入的工作区副本。其标记为 **不再在源中**。删除 ChatGPT 中的市场会删除从中导入的所有插件。

<a id="reconnect-or-change-github-access"></a>

## 重新连接或更改 GitHub 访问权限

对于 **重新连接GitHub访问**，首先确认用于导入的 GitHub 帐户仍然有权访问仓库和任何引用的仓库。最初导入市场的管理员应在 ChatGPT 中打开 GitHub 插件并重新连接其帐户，因为市场同步使用该管理员的 GitHub 连接。

对于 **转让给新主人**，新工作区管理员应打开 **管理员** > **插件** > **添加** > **进口市场** 并使用相同的 **来源**、**路径** 和 **分支、标记或提交** 值导入相同的市场。未来的同步将使用其 GitHub 连接。

不要只是为了重新连接或更改所有权而删除市场：删除还会删除其导入的插件。