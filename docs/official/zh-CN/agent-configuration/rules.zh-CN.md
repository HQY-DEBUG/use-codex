> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../en/agent-configuration/rules.md)。

<a id="rules"></a>

# 命令规则

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用规则来控制哪些命令 Codex 可以在沙箱外运行。

规则是实验性的，可能会发生变化。

<a id="create-a-rules-file"></a>

## 创建规则文件

1. 在活动配置层旁边的 `rules/` 文件夹下创建 `.rules` 文件（例如 `~/.codex/rules/default.rules`）。
2. 添加规则。此示例在允许 `gh pr view` 在沙箱外运行之前进行提示。

```python
   # 在沙箱外运行前缀为 `gh pr view` 的命令之前进行提示。
   prefix_rule(
       # 要匹配的前缀。
       pattern = ["gh", "pr", "view"],

       # Codex 请求运行匹配命令时要采取的操作。
       decision = "prompt",

       # 这条规则存在的可选理由。
       justification = "Viewing PRs is allowed with approval",

       # `match` 和 `not_match` 是可选的“内联单元测试”，您可以在其中
       # 提供应该（或不应该）匹配此规则的命令示例。
       match = [
           "gh pr view 7888",
           "gh pr view --repo openai/codex",
           "gh pr view 7888 --json title,body,comments",
       ],
       not_match = [
           # 不匹配，因为 `pattern` 必须是精确的前缀。
           "gh pr --repo openai/codex view 7888",
       ],
   )
```

3. 重新启动Codex。

Codex 在启动时扫描每个活动配置层下的 `rules/`，包括 [团队配置](../enterprise/admin-setup.zh-CN.md#step-4-standardize-local-configuration-with-team-config) 位置和 `~/.codex/rules/` 的用户层。 `下的项目本地规则<repo>/.codex/rules/` load only when the project `.codex/` 层是可信的。

当您将命令添加到 TUI 中的允许列表时，Codex 会写入 `~/.codex/rules/default.rules` 处的用户层，以便将来的运行可以跳过提示。

启用智能审批（默认）后，Codex 可能会在升级请求期间为您建议 `prefix_rule`。在接受建议的前缀之前请仔细查看。

管理员还可以强制执行来自 [`requirements.toml`](../enterprise/managed-configuration.zh-CN.md#admin-enforced-requirements-requirementstoml) 的限制性 `prefix_rule` 条目。

<a id="understand-rule-fields"></a>

## 了解规则字段

`prefix_rule()` 支持以下字段：

- `pattern` **（必填）**：定义要匹配的命令前缀的非空列表。每个元素是：
  - 文字字符串（例如，`"pr"`）。
  - 用于匹配该参数位置处的替代项的文字联合（例如，`["view", "list"]`）。
- `decision` **（默认为 `"allow"`）**：规则匹配时采取的操作。当多个规则匹配时，Codex 应用最严格的决策（`forbidden` > `prompt` > `allow`）。
  - `allow`：在沙箱外运行命令而不提示。
  - `prompt`：每次匹配调用之前提示。
  - `forbidden`：阻止请求而不提示。
- `justification` **（可选）**：非空、人类可读的规则原因。 Codex 可能会在批准提示或拒绝消息中出现。当您使用 `forbidden` 时，请在适当的情况下在理由中包含推荐的替代方案（例如，`"Use \`rg\` instead of \`grep\`."`）。
- `match` 和 `not_match` **（默认为 `[]`）**：Codex 在加载规则时进行验证的示例。使用它们可以在规则生效之前发现错误。

当 Codex 考虑运行某个命令时，它会将该命令的参数列表与 `pattern` 进行比较。在内部，Codex 将命令视为参数列表（就像 `execvp(3)` 接收的那样）。

<a id="shell-wrappers-and-compound-commands"></a>

## Shell 包装器和复合命令

某些工具将多个 shell 命令包装到单个调用中，例如：

```text
["bash", "-lc", "git add . && rm -rf /"]
```

由于这种命令可以在一个字符串中隐藏多个操作，因此 Codex 会特别对待 `bash -lc`、`bash -c` 及其 `zsh` / `sh` 等效命令。

<a id="when-codex-can-safely-split-the-script"></a>

### 当Codex可以安全地分割脚本时

如果 shell 脚本是仅由以下部分组成的线性命令链：

- 明文（无变量扩展，无`VAR=...`、`$FOO`、`*`等）
- 由安全操作员加入（`&&`、`||`、`;` 或 `|`）

然后 Codex 解析它（使用树保姆）并将其拆分为单独的命令，然后再应用您的规则。

上面的脚本被视为两个单独的命令：

- `["git", "add", "."]`
- `["rm", "-rf", "/"]`

然后，Codex 根据您的规则评估每个命令，最严格的结果获胜。

即使您允许 `pattern=["git", "add"]`，Codex 也不会自动允许 `git add . && rm -rf /`，因为 `rm -rf /` 部分是单独评估的，并且会阻止整个调用被自动允许。

这可以防止危险命令与安全命令一起偷运进来。

<a id="when-codex-does-not-split-the-script"></a>

### 当Codex不分割脚本时

如果脚本使用更高级的 shell 功能，例如：

- 重定向（`>`、`>>`、`<`）
- 替换（`$(...)`、`...`）
- 环境变量（`FOO=bar`）
- 通配符模式（`*`、`?`）
- 控制流（带分配的 `if`、`for`、`&&` 等）

那么 Codex 不会尝试解释或拆分它。

在这些情况下，整个调用被视为：

```text
["bash", "-lc", "<full script>"]
```

并且您的规则将应用于该 **单身** 调用。

通过这种处理，您可以在安全时获得每个命令评估的安全性，在不安全时获得保守的行为。

<a id="test-a-rule-file"></a>

## 测试规则文件

使用 `codex execpolicy check` 测试您的规则如何应用于命令：

```shell
codex execpolicy check --pretty \
  --rules ~/.codex/rules/default.rules \
  -- gh pr view 7888 --json title,body,comments
```

该命令发出 JSON，显示最严格的决策和任何匹配规则，包括匹配规则中的任何 `justification` 值。使用多个 `--rules` 标志来组合文件，并添加 `--pretty` 来格式化输出。

<a id="understand-the-rules-language"></a>

## 理解规则语言

`.rules` 文件格式使用 `Starlark`（请参阅 [语言规范](https://github.com/bazelbuild/starlark/blob/master/spec.md)）。它的语法类似于Python，但它被设计为可以安全运行：规则引擎可以运行它而不会产生副作用（例如，接触文件系统）。