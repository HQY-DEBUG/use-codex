> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../en/permissions.md)。

<a id="permissions"></a>

# 权限配置技术参考

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

贝塔。权限配置文件正在积极开发中，可能会发生变化。

权限配置文件不与旧的沙箱设置组合。配置 `default_permissions` 和 `[permissions]`，或 `sandbox_mode` / `sandbox_workspace_write`，但不能同时配置两者。如果 `sandbox_mode` 出现在任何加载的配置文件中，则传递 `--sandbox`，或者选定的配置文件集 `sandbox_mode`、Codex 使用那些较旧的沙箱设置而不是 `default_permissions`。

托管 `allowed_permission_profiles` 是例外：它使 Codex 使用权限配置文件。在部署托管配置文件白名单之前，删除旧设置，例如 `sandbox_mode` 和 `[sandbox_workspace_write]`。对于混合版本企业部署，您可以将托管 `allowed_sandbox_modes` 要求保留为临时兼容性约束，直到每个客户端都运行 Codex 0.138.0 或更高版本。

权限配置文件允许您将最小权限边界应用于代表您运行的本地命令 Codex。配置文件是一个命名策略，它将文件系统规则（定义哪些命令可以读取或写入）与网络规则（定义命令可以到达哪些目标）相结合。

配置文件的 `network.enabled = true` 允许命令网络访问，但不会启动网络代理。要强制执行配置文件域规则，还需在 `config.toml` 中设置 `features.network_proxy = true`，或使用启用的、管理员管理的 `[experimental_network]` 要求。如果没有活动代理，配置文件域规则不会限制直接网络访问。

使用配置文件为 Codex 提供足够的当前聊天访问权限，而无需授予对您的计算机或网络的广泛访问权限。例如，只读配置文件可以让 Codex 检查项目而不对其进行编辑，而可写配置文件可以限制对选定工作区根目录的编辑。

macOS、Linux、WSL 和本机 Windows 支持本地权限配置文件。请参阅 [范围和执行](#scope-and-enforcement) 了解特定于平台的详细信息和注意事项。

Codex云网络设置请参见[互联网接入](cloud/internet-access.zh-CN.md)。

<a id="define-and-select-a-profile"></a>

## 定义并选择配置文件

Codex 包括三个内置权限配置文件：

- `:read-only` 将本地命令执行保持为只读。
- `:workspace` 允许在活动工作区根目录和系统临时目录内进行写入。
- `:danger-full-access` 消除了本地沙箱限制，并且仅应在有意进行广泛访问时使用。

在“[permissions”下创建一个命名配置文件。<name>]`, then set the top-level `default_permissions` key to that profile name or to one of the built-ins above. In this example, `project-edit` 是用户定义的配置文件名称，而不是内置值。

企业管理员可以定义配置文件并限制用户可以通过托管 `requirements.toml` 选择哪些配置文件。一旦 `allowed_permission_profiles` 存在，省略的配置文件将被拒绝，包括省略的内置文件和未来 Codex 版本中添加的配置文件。有关推荐的托管配置，请参阅 [控制可用的权限配置文件](enterprise/managed-configuration.zh-CN.md#control-available-permission-profiles)。

自定义配置文件使用两个相关的概念：

- `[ 权限。<name>.workspace_roots]` 添加应计为该配置文件的工作区根的具体目录。
- `[ 权限。<name>.filesystem.":workspace_roots"]` 定义了适用于每个有效工作区根内部的文件系统规则 Codex：当前会话的运行时工作区根加上上面配置文件定义的根。

配置文件也使用普通的配置层模型。较高优先级层可以添加或替换相同配置文件名称下的条目，而无需重新声明整个配置文件。

例如，组织级配置和用户级配置可以独立扩展同一配置文件：

```toml
# /etc/codex/config.toml
[permissions.server.workspace_roots]
"~/code/server" = true
```

```toml
# 〜/.codex/config.toml
[permissions.server.workspace_roots]
"~/code/mobile-app" = true
```

当 `server` 处于活动状态时，两个工作区根都参与有效配置文件。

```toml
default_permissions = "project-edit"

[features]
network_proxy = true

[permissions.project-edit.workspace_roots]
"~/code/app" = true
"~/code/shared-lib" = true

[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"
".devcontainer" = "read"
"**/*.env" = "deny"

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"api.openai.com" = "allow"
"objects.githubusercontent.com" = "allow"
"*.github.com" = "allow"
"tracking.example.com" = "deny"
```

此简介：

- 读取常见开发人员工具所需的最小运行时路径。
- 将相同的工作区根规则应用于当前会话和配置文件定义的根。
- 将每个根目录下的 IDE 相邻设置（例如 `.devcontainer/`）保持为只读。
- 拒绝使用 glob 规则匹配环境文件。
- 仅允许通过配置的域策略进行网络访问。

在活动配置文件内，即使较宽的路径可读或可写，较窄的拒绝规则仍然有效。例如，配置文件可以使工作区根目录可写，同时仍将匹配的 `.env` 路径设置为 `deny`。

<a id="extend-a-profile"></a>

## 扩展个人资料

当配置文件与内置配置文件或其他命名配置文件大部分相同时，请使用 `extends`。更喜欢扩展内置配置文件而不是从头开始，以便基线保护得以延续。例如，扩展 `:workspace` 会使工作区根目录的 `.codex` 目录保持只读状态，除非您明确覆盖它。设置父级一次，然后仅添加或覆盖不同的规则。

```toml
default_permissions = "project-edit"

[features]
network_proxy = true

[permissions.project-edit]
description = "Project editing with OpenAI API access."
extends = ":workspace"

[permissions.project-edit.filesystem.":workspace_roots"]
"**/*.env" = "deny"

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"api.openai.com" = "allow"
```

此配置文件以 `:workspace` 开头，保持匹配的 `.env` 文件被拒绝，并允许对 `api.openai.com` 的请求。配置文件可以扩展 `:read-only`、`:workspace` 或其他命名的配置文件。不能扩展`:danger-full-access`； Codex 还拒绝未知的父母和继承周期。

<a id="configuration-spec"></a>

## 配置规格

| 条目 | 类型/值 | 默认值 | 详细信息 |
| ----------------------------------------------------------------- | -------------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default_permissions` | 字符串配置文件名称 | 无 | 默认情况下应用的权限配置文件名称 Codex。它必须与 `[permissions]` 下的配置文件或 `:workspace` 等内置配置文件匹配。明确设置它以实现可预测的行为；仅当 `:workspace` 和 `:read-only` 均明确允许时，托管需求才可以忽略它。 Codex 使用较旧的沙箱设置，除非托管 `allowed_permission_profiles` 告诉它在此设置中使用权限配置文件。 |
| `[ 权限。<name>]`                                            | Table                      | None                    | Defines a named profile. `default_permissions`选择一个配置文件作为默认；其他权限配置文件设置也使用配置文件名称。                                                                                                                                                                                                                                                                               |
| `权限。<name>.description`                                  | String                     | None                    | Provides a human-readable description for the profile. A profile does not inherit its parent's description through `extends`。                                                                                                                                                                                                                                                                                                 |
| `权限。<name>.extends`                                      | String profile name        | None                    | Starts this profile from another named profile or the built-in `：只读` or `：工作区` profile. Codex rejects `：危险-完全访问`，未知的父母和继承周期。                                                                                                                                                                                                                                            |
| `[ 权限。<name>.workspace_roots]`                            | Table                      | None                    | Adds profile-defined workspace roots that receive `:workspace_roots` 文件系统规则与当前会话的运行时工作区根一起。                                                                                                                                                                                                                                                                                |
| `权限。<name>.workspace_roots。”<path>“`                     | Boolean                    | `false`                 | Adds the path to the profile's workspace root set when `true`. Entries set to `false` 保持不活动状态。                                                                                                                                                                                                                                                                                                                        |
| `[ 权限。<name>.filesystem]` | 表 | 无 | 映射文件系统路径以访问值或作用域子路径映射。丢失或空的文件系统表会限制文件系统访问并发出启动警告。                                                                                                                                                                                                                                                               |
| `权限。<name>.filesystem.glob_scan_max_depth`               | Number                     | None                    | Limits deny-read glob expansion on Linux, WSL, and native Windows when Codex snapshots matches before sandbox startup. Larger values can increase startup scanning work. Use a value of at least `1` when an unbounded `**`模式需要有界预扩展。                                                                                                                                                              |
| `[ 权限。<name>.文件系统]。”<path>“`                        | `read`, `write`, or `deny` | None                    | Grants direct access for a supported path. `deny` denies access and wins over equally specific `write` or `read`条目。 Codex 拒绝活动运行时无法强制执行的直接写入规则。                                                                                                                                                                                                                            |
| `[ 权限。<name>.文件系统。”<path>"]."<subpath>“`            | `读取`, `写入`, or `拒绝` | None                    | Grants access to a descendant of `<path>`. Use `.` for the base path. Other subpaths must be relative descendants and cannot contain `.` or `..` 组件。                                                                                                                                                                                                                                                                  |
| `[ 权限。<name>.network]`                                    | Table                      | None                    | Configures command network access and the policy that an active network proxy enforces. Enable `features.network_proxy`除非管理员管理的网络要求启动代理。                                                                                                                                                                                                                                    |
| `权限。<name>.network.enabled`                              | Boolean                    | `false` | 为配置文件中的命令启用网络访问。它不会启动网络代理；没有活动代理，命令可以直接连接，不受域限制。                                                                                                                                                                                                                                                  |
| `[ 权限。<name>.network.domains]`                            | Table                      | None                    | Maps host patterns to `allow` or `deny`. Rules apply only when the network proxy is active. The active proxy blocks domain requests if there are no `allow` 条目，拒绝条目会覆盖允许条目。                                                                                                                                                                                                                 |
| `权限。<name>.网络.域。”<pattern>“`                  | `allow` or `deny`          | None                    | Supports exact hosts, `*.example.com` for subdomains, `**.example.com` for apex plus subdomains, and `*`作为仅允许的全局通配符。主机模式通过修剪、小写、剥离来规范化尾随点，并剥离简单端口或括号 |。
| `[ 权限。<name>.network.unix_sockets]` | 表 | 无 | 映射 Unix 套接字白名单覆盖。仅用于本地集成，例如 Docker。                                                                                                                                                                                                                                                                                                                                         |
| `权限。<name>.network.unix_sockets。”<path>“`                | `allow` or `deny`          | None                    | Adds an absolute Unix socket path to the effective allowlist with `allow`, or rejects it with `deny`。被拒绝的条目将从有效允许列表中省略。|
| `权限。<name>.network.proxy_url`                            | URL string                 | `http://127.0.0.1:3128` | 用于 `HTTP_PROXY`、`HTTPS_PROXY`、websocket 代理变量以及相关工具代理环境变量的 HTTP 代理侦听器。                                                                                                                                                                                                                                                                                            |
| `权限。<name>.network.enable_socks5`                        | Boolean                    | `true`                  | Enables the SOCKS5 listener used for `ALL_PROXY`和FTP代理变量。                                                                                                                                                                                                                                                                                                                                                     |
| `权限。<name>.network.socks_url`                            | URL string                 | `http://127.0.0.1:8081` | SOCKS5监听器地址。                                                                                                                                                                                                                                                                                                                                                                                                      |
| `权限。<name>.network.enable_socks5_udp`                    | Boolean                    | `true` | 在启用 SOCKS5 侦听器时启用 SOCKS5 UDP 支持。                                                                                                                                                                                                                                                                                                                                                               |
| `权限。<name>.network.allow_upstream_proxy`                 | Boolean                    | `true`                  | Allows the network sandbox proxy to respect upstream `HTTP(S)_PROXY` and `ALL_PROXY` 出站请求设置。                                                                                                                                                                                                                                                                                                          |
| `权限。<name>.network.allow_local_binding`                  | Boolean                    | `false`                 | Disables the local/private-network guard when `true`. When `false`, exact local literals such as `localhost` or `127.0.0.1` 必须明确列入白名单，并且解析为本地或私有 IP 的主机名仍被阻止。                                                                                                                                                                                                |
| `权限。<name>.network.dangerously_allow_non_loopback_proxy` | Boolean                    | `false` | 允许代理侦听器绑定非环回地址。不为当地的普通发展做好准备。                                                                                                                                                                                                                                                                                                                            |
| `权限。<name>.network.dangerously_allow_all_unix_sockets`   | Boolean                    | `false` | 绕过支持 Unix 套接字代理的 Unix 套接字白名单。这是一个宽阔的当地逃生舱口。                                                                                                                                                                                                                                                                                                               |

<a id="filesystem-permissions"></a>

## 文件系统权限

文件系统条目使用 `read`、`write` 或 `deny`：

| 访问 | 含义 |
| ------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `read` | 允许命令读取文件并列出路径下的目录。命令无法在那里创建、修改、重命名或删除文件。 |
| `write` | 允许命令读取和修改路径下的文件，包括在操作系统允许的情况下创建、重命名和删除文件。  |
| `deny` | 拒绝该路径下的读取和写入。使用它从更广泛的 `read` 或 `write` 授权中开辟出被拒绝的子路径。         |

更具体的条目会覆盖更广泛的条目。当两个条目针对相同路径时，`deny` 优先于 `write`，`write` 优先于 `read`。

这种优先级让配置文件首先描述一个广泛的工作区域，然后划分出不可读的文件或目录：

```toml
[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"
".devcontainer" = "read"
"**/*.env" = "deny"
```

在此示例中，工作区根目录保持可写，`.devcontainer/` 保持可读但不变得可写，并且匹配的环境文件对沙箱命令仍然不可用。

更具体的路径还可以在更广泛的拒绝内重新打开更窄的子树：

```toml
[permissions.project-edit.filesystem]
"~/Documents" = "deny"
"~/Documents/codex" = "write"
```

支持的路径形式：

| 路径 | 含义 | 作用域子路径 |
| ------------------ | ------------------------------------------------------------------------------------------- | --------------- |
| `:root` | 文件系统根 | `.` 仅 |
| `:minimal` | 常用工具所需的平台和运行时路径 | 仅 `.` |
| `:workspace_roots` | 当前会话的工作区根加上任何启用的配置文件定义的工作区根 | 是 |
| `:tmpdir` | `$TMPDIR` 位置（当有一个可用时） | `.` 仅 |
| `:slash_tmp` | `/tmp` 文件夹，如果存在 | `.` 仅限 |
| `/absolute/path` | 平台绝对路径，例如 macOS/Linux/WSL 上的 `/path` 或本机 Windows 上的 `C:\path` | 是 |
| `~/path` | 当前用户主目录下的路径 | 是 |

在本机 Windows 上，主相对路径也可以使用反斜杠，例如 `~\work`。

仅当配置文件有意需要广泛的阅读覆盖范围时才使用 `:root`：

```toml
[permissions.audit.filesystem]
":root" = "read"
```

使用 `:workspace_roots` 下的嵌套条目来限制对工作区根目录相对子路径的访问：

```toml
[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"          # each workspace root
"docs" = "read"        # each workspace-root docs directory
"generated" = "deny"   # each workspace-root generated directory
```

嵌套子路径必须保留在其工作区根目录内。 `../other-repo` 等父级遍历被拒绝。

<a id="deny-reads-with-exact-paths-or-globs"></a>

### 拒绝使用精确路径或 glob 进行读取

对 Codex 不应读取的文件或子树使用 `deny`，即使更广泛的配置文件规则授予附近的访问权限也是如此。精确路径适用于 `~/.ssh` 等稳定位置。当配置文件需要覆盖一系列敏感文件（其确切位置因仓库而异）时，Glob 模式效果更好。

当 glob 位于 `:workspace_roots` 下时，Codex 会相对于每个有效工作区根来解释它。例如：

```toml
[permissions.project-edit.filesystem.":workspace_roots"]
"**/*.env" = "deny"
```

此规则拒绝读取在每个运行时或配置文件定义的工作区根下找到的匹配 `.env` 文件。当您想要保留正常的工作区写入，同时保持环境文件、生成的机密或类似的带有凭据的文件不可读时，请使用它。

支持 `deny` glob 模式作为拒绝读取规则。 `read` 或 `write` 全局变量在 Linux、WSL 和本机 Windows 沙箱上的可移植性较差，因此尽可能首选精确路径或子树规则，例如 `"docs/**" = "read"`。

在 Linux、WSL 和本机 Windows 上，无界 `**` 拒绝读取模式可能需要在沙箱启动之前进行有界预扩展。当您使用无界模式（例如 `"**/*.env" = "deny"`）时，设置 `glob_scan_max_depth`：

```toml
[permissions.project-edit.filesystem]
glob_scan_max_depth = 3

[permissions.project-edit.filesystem.":workspace_roots"]
"**/*.env" = "deny"
```

`glob_scan_max_depth` 必须至少为 `1`。较高的值在沙箱启动之前扫描得更深，这可以增加 Linux、WSL 和本机 Windows 上的启动工作。如果您不想使用有界扩展，请枚举显式深度，例如 `*.env`、`*/*.env` 和 `*/*/*.env`。

当相同的规则应用于多个当前会话根时，将可重用的工作区根添加到配置文件中：

```toml
[permissions.project-edit.workspace_roots]
"~/code/app" = true
"~/code/shared-lib" = true
```

当此配置文件处于活动状态时，Codex 将 `:workspace_roots` 规则应用于当前会话的运行时工作区根以及每个启用的配置文件定义的工作区根。

在本机 Windows 上，支持驱动器号路径（例如 `D:\work`）和 UNC 路径（例如 `\\server\share`）作为绝对路径。

<a id="network-permissions"></a>

## 网络权限

网络访问和网络过滤是单独的设置。设置`权限。<name>.network.enabled = true` to let commands access the network, and enable `features.network_proxy` 强制执行配置文件的域规则：

```toml
[features]
network_proxy = true

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"example.com" = "allow"      # exact host
"*.example.com" = "allow"    # subdomains only
"**.example.com" = "allow"   # apex and subdomains
"ads.example.com" = "deny"   # deny wins over allow
```

结果行为取决于这两个设置：

- 网络关闭：无论代理功能如何，命令都无法访问网络。
- 网络打开，代理关闭：命令具有直接、不受限制的网络访问权限。不强制执行权限配置文件中的域规则。
- 网络打开，代理打开：命令使用代理，该代理强制执行配置文件的域规则。如果活动代理没有允许的域，它将阻止外部目标。

添加 `[ 权限。<name>.network.domains]` or setting `权限。<name>.network.enabled = true` does not enable `features.network_proxy`. As an alternative, administrators can enable the proxy with `[experimental_network]` in `requirements.toml`。参见 [受管配置](enterprise/managed-configuration.zh-CN.md#configure-network-access-requirements)。

当活动时，网络沙箱代理默认绑定到本地侦听器：

```toml
[permissions.project-edit.network]
enabled = true
proxy_url = "http://127.0.0.1:3128"
enable_socks5 = true
socks_url = "http://127.0.0.1:8081"
enable_socks5_udp = true
```

将这些侦听器设置保留为默认值，除非您要与特定运行时集成。 `dangerously_*` 网络密钥是专用环境的逃生口，不应用于普通的本地开发。

<a id="local-and-private-networks"></a>

### 本地和专用网络

当网络代理处于活动状态时，Codex 默认情况下应用本地/专用网络防护，以防止 DNS 重新绑定和意外访问本地服务。要有意允许文本本地目标，请将确切的主机或 IP 文本列入白名单：

```toml
[permissions.project-edit.network.domains]
"localhost" = "allow"
"127.0.0.1" = "allow"
```

仅当配置文件必须到达解析为本地或私有地址的白名单主机名时，才设置 `allow_local_binding = true`：

```toml
[permissions.project-edit.network]
enabled = true
allow_local_binding = true

[permissions.project-edit.network.domains]
"localhost" = "allow"
```

<a id="unix-sockets"></a>

### Unix 套接字

Unix 套接字代理是 Docker 等工具的本地逃生口。谨慎使用它：

```toml
[permissions.project-edit.network.unix_sockets]
"/var/run/docker.sock" = "allow"
"/tmp/old.sock" = "deny"
```

使用 `deny` 拒绝套接字路径，包括继承的允许条目。被拒绝的套接字路径将从有效允许列表中省略。

启用 Unix 套接字后，将代理侦听器绑定到环回地址。

<a id="migrate-from-older-sandbox-settings"></a>

## 从旧的沙箱设置迁移

当您想要一个可重用配置文件来描述文件系统和网络行为时，权限配置文件将替换 `sandbox_mode` 和 `sandbox_workspace_write` 的旧组合。使用一个系统或另一个系统进行会话，而不是同时使用两个系统。

建议的起点：

- 对于只读工作流程，请使用内置 `:read-only` 配置文件或定义仅在需要时具有读取访问权限的自定义配置文件。
- 对于工作区编辑，请使用内置 `:workspace` 配置文件或定义通过 `:workspace_roots` 写入的自定义配置文件，并仅添加工作流所需的额外临时或缓存路径。
- 对于不受限制的本地执行，仅当您有意想要最广泛的本地访问模型时才使用 `:danger-full-access`。

配置文件描述了会话的本地默认状态。组织管理的要求仍然可以添加用户配置不应扩大的限制。有关管理员强制执行的文件系统和网络约束，请参阅 [受管配置](enterprise/managed-configuration.zh-CN.md)。

<a id="scope-and-enforcement"></a>

## 范围和执行

权限配置文件定义本地沙箱命令执行的边界。将它们与审批策略以及 Web 搜索、连接器、MCP 服务器、内置浏览器、计算机使用和 Codex 云的单独控件一起使用。

<a id="what-profiles-control"></a>

### 配置文件控制哪些内容

- **本地命令执行：** 权限配置文件管理在您的计算机上运行的沙箱命令。连接器、MCP 服务器、浏览器或计算机使用界面、Codex 云环境设置以及批准的升级使用自己的控件。
- **文件系统写道：** 可写入的配置文件可以创建持久更改。将脚本、构建步骤、包管理器挂钩、shell 启动文件和共享目录的写入视为敏感，因为后续工具或用户可以在原始沙箱上下文之外执行这些文件。
- **出境目的地：** 网络域规则限制沙箱命令流量仅在网络代理处于活动状态时才能到达的位置。他们无法确定允许的目的地是否值得信赖，并且通配符允许规则保持广泛。
- **本地服务：** 默认情况下，活动网络代理会阻止本地和专用网络目标。将 `localhost`、私有 IP、Unix 套接字列入白名单，或设置 `allow_local_binding = true` 显式打开对本地服务的访问。

<a id="what-the-network-proxy-does-not-control"></a>

### 网络代理无法控制的内容

网络代理仅过滤来自沙箱内运行的本地命令的流量。它不会将配置文件的域白名单应用于：

- **网络搜索：** 托管搜索工具使用其自己的访问设置。使用 `web_search` 来控制它，对于托管客户端，使用 `allowed_web_search_modes` 来控制它。 `tools.web_search.allowed_domains` 过滤搜索结果，而不是命令网络访问。
- **应用程序和连接器：** 连接器支持的工具使用自己的服务端连接、工作区权限以及应用程序或工具设置。
- **MCP服务器：** 本地和远程 MCP 服务器使用自己的进程或传输。使用 `mcp_servers` 配置和托管服务器白名单来控制它们。
- **浏览器和计算机使用：** 浏览器导航和计算机使用操作使用自己的功能和批准控件。
- **Codex服务流量：** 模型、身份验证和其他客户端服务请求使用客户端单独的 HTTP 和系统代理设置。
- **Codex云：** 这些任务使用其环境自己的 [互联网访问设置](cloud/internet-access.zh-CN.md)。

要限制这些使用界面，请直接配置每个功能。命令网络白名单并不是针对 Codex 可以执行的每个操作的全局网络策略。

<a id="how-enforcement-works"></a>

### 执法如何运作

- 在 macOS 上，Codex 使用 Seatbelt 沙箱配置文件。如果平台沙箱无法强制执行所选策略，Codex 将拒绝运行该命令，而不是在未沙箱的情况下静默运行该命令。
- 在 Linux 和 WSL 上，Codex 使用 [气泡膜](https://github.com/containers/bubblewrap) 和 [安全计算](https://www.kernel.org/doc/html/latest/userspace-api/seccomp_filter.html)，Landlock 可用于兼容性回退路径。最强大的执行路径取决于用户命名空间和内核支持；受限制的容器主机可以强制兼容路径，并且拒绝不支持的拆分策略。
- 在本机 Windows 上，[`elevated` 沙箱](windows/windows-sandbox.zh-CN.md#windows-sandbox) 最强，因为它可以使用专用的低权限沙箱用户、文件系统权限边界和防火墙规则。 `unelevated` 沙箱是一种后备方案，网络隔离较弱，无法强制执行每个拆分读/写剥离，因此不支持的策略将被拒绝。当您需要 Linux 沙箱模型时，请使用 WSL。

<a id="operational-guidance"></a>

### 操作指导

选择仍然可以完成任务的最窄配置文件，尤其是当您授予写入或出站网络访问权限时。保持审批策略、秘密处理和允许规则与该访问级别保持一致。

<a id="common-profiles"></a>

## 常用型材

<a id="read-only-with-network-allowlist"></a>

### 具有网络允许列表的只读状态

```toml
default_permissions = "readonly-net"

[features]
network_proxy = true

[permissions.readonly-net.filesystem]
":minimal" = "read"

[permissions.readonly-net.filesystem.":workspace_roots"]
"." = "read"

[permissions.readonly-net.network]
enabled = true

[permissions.readonly-net.network.domains]
"api.openai.com" = "allow"
```

<a id="file-access-limited-to-workspace"></a>

### 文件访问仅限于工作区

下面是一个权限配置文件的示例，该配置文件将使您的工作区文件夹可由 Codex 写入，同时拒绝读取文件系统的其余部分（有有限的例外情况，由 `:minimal` 确定）。

```toml
default_permissions = "workspace-only"

[permissions.workspace-only]
# 通过扩展 :workspace 配置文件，您可以获得 Codex 的保护措施以确保
# 工作区根目录中的子文件夹（例如 .codex/ 和 .git/）是只读的
# 而文件夹的其余部分是可写的。
extends = ":workspace"

[permissions.workspace-only.filesystem]
# 默认情况下，拒绝对磁盘上所有文件的读取访问。
":root" = "deny"

# 尽管在实践中，软件智能体需要能够读取以下文件夹：
# 包含完成工作的常用工具，例如 `/usr/bin`，因此授予访问权限
# 到由 Codex 确定的“最小”文件和文件夹集。
":minimal" = "read"

# 通过扩展 :workspace 配置文件， :tmpdir 和 :slash_tmp 被“写入”
# 默认情况下，但如果需要，您可以完全拒绝对它们的访问。
":tmpdir" = "deny"
":slash_tmp" = "deny"
```

<a id="workspace-write-without-network"></a>

### 工作区无网络写入

```toml
default_permissions = "project-edit"

[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"

[permissions.project-edit.network]
enabled = false
```

<a id="workspace-write-with-public-web-access"></a>

### 具有公共 Web 访问权限的工作区写入

```toml
default_permissions = "workspace-net"

[features]
network_proxy = true

[permissions.workspace-net.filesystem]
":minimal" = "read"

[permissions.workspace-net.filesystem.":workspace_roots"]
"." = "write"

[permissions.workspace-net.network]
enabled = true

[permissions.workspace-net.network.domains]
"*" = "allow"
```

仅当您打算允许公共网络访问时才使用全局 `"*"` 允许规则。拒绝规则可以缩小广泛的允许名单范围。