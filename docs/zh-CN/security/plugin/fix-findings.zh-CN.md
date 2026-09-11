> 非官方简体中文机器译文。译自本地英文资料快照；代码、命令、配置键名和路径保留原样。术语与格式已抽查，未逐句人工校订。[英文原文](../../../en/security/plugin/fix-findings.md)。

<a id="fix-and-verify-security-findings"></a>

# 修复与验证发现

> 有关完整的文档索引，请参阅 [llms.txt](https://learn.chatgpt.com/llms.txt)。通过将 `.md` 附加到页面 URL 即可获得文档页面的 Markdown 版本。

使用 Codex Security 将已接受的安全发现转变为有针对性的、经过验证的补丁。您可以在安全工作台中工作，或通过提示、命令行或 CI/CD 运行修复工作流。 Codex 验证该问题，并在测试安全实用时添加重点回归测试，该测试在修复之前失败并在修复之后通过。它还检查合法行为是否仍然有效。如果回归测试不安全或不可行，Codex 会记录证明差距并提供最强的可重复验证工件。

从一项已接受的发现开始，审查提议的补丁和验证证据。如果工作流程符合您的标准，请在单独的 Codex 任务或 CI/CD 作业中一次处理其他已接受的结果。保持每个任务的范围可以使其代码更改和证据更容易审查。

<a id="fix-a-finding-in-the-ui"></a>

## 修复 UI 中的一个发现

打开 **研究结果** 中已接受的结果或 **扫描** 中已完成的扫描。检查其证据，然后使用 **补丁** 生成、检查、应用和验证一项重点修复。

<WorkflowSteps variant="headings">

1. 生成重点补丁

打开结果，选择 **补丁** 选项卡，然后选择 **生成补丁**。 Codex 在可行时验证或重现问题，并在不修改所选签出的情况下编写补丁工件。

2. 查看建议的差异

阅读每个更改的源代码、回归测试和验证工件。拒绝广泛的重构、不相关的清理或削弱其他安全控制的更改。

3. 在本地应用补丁

仅在差异可接受后才选择 **应用补丁**。 Codex 将准确生成的补丁应用于工作树并记录该状态。在继续之前查看工作树差异。

4. 验证修复情况

选择**验证修复**。 Codex 重新运行原始再现器或最强的可用漏洞检查。如果回归测试安全且实用，Codex 会在修复之前检查它是否失败并在修复之后通过。如果测试不安全或不可行，Codex 会记录证明差距并提供最强的可重复验证工件。它还检查合法行为、附近的绕过以及相关的仓库测试。

5. 明确关闭该发现

验证不会自动结束调查结果。查看命令、结果和剩余的证据差距，然后以准确的理由结束发现结果或将其保留以进行更多工作。

</WorkflowSteps>

<figure className="not-prose my-8">
  <CodexScreenshot
    alt="本机 Codex Security 工作台显示为已接受的发现生成的补丁"
    lightSrc={fixFindingPatch.src}
    darkSrc={fixFindingPatchDark.src}
    maxHeight="460px"
  />
  <figcaption className="mt-3 text-sm text-secondary">
在将生成的安全修复程序应用于您的检出目录之前，请检查它。
  </figcaption>
</figure>

<a id="fix-a-finding-from-the-cli"></a>

## 修复 CLI 中的发现

使用 Codex CLI 从扫描、票证、咨询、披露、安全评估或内部审查中获取已接受的结果。

在运行这些命令之前，请在 `codex exec` 使用的 `CODEX_HOME` 中安装 Codex Security。默认情况下，新的 CI 运行程序不包含市场插件。

```text
使用 $codex-security:fix-finding 修复从 <report-path> 查找 <finding-id> 的问题。验证问题，进行最小的安全更改，并添加在修复之前失败并在修复之后通过的重点回归测试。如果该测试不安全或不可行，请记录证明差距并提供最强的可重复验证工件。验证该问题不再重现。
```

包括已知的源、接收器、攻击者输入、影响、预期不变量、重现器、受影响的文件和验证命令。 Codex 可以检查仓库是否缺少技术细节。在假设产品策略或预期的安全不变性之前应该询问。

对于自动运行，请检查代码，提供结果报告，然后在运行者的 `CODEX_HOME` 中安装插件。然后启用工作区写入并将提示传递给 `codex exec`：

```bash
codex exec --sandbox workspace-write 'Use $codex-security:fix-finding to fix finding <finding-id> from <report-path>. Validate the issue, make the smallest safe change, and add a focused regression test that fails before the fix and passes after it. If that test is unsafe or infeasible, record the proof gap and provide the strongest repeatable validation artifact instead. Verify that the issue no longer reproduces.'
```

<a id="scan-and-fix-findings-in-cicd"></a>

## 扫描并修复 CI/CD 中的发现结果

在调用任一技能之前，将 Codex Security 安装在跑步者的 `CODEX_HOME` 中。以下命令使用已安装的插件；他们不安装它。

在 CI/CD 中，将更改扫描与修复分开，并要求扫描保持结帐不变。将已完成的扫描目录保留为作业工件，查看结果，并为接受修复的每个结果启动单独的 Codex 任务或作业。

默认情况下，`codex exec` 使用只读沙箱。使用 `--sandbox workspace-write` 运行更改扫描和修复。扫描需要该权限来保存临时工件，但其提示仍必须需要 `Do not modify the checkout`。修复需要相同的权限来编写重点补丁和验证证据。参见 [权限和安全](../../non-interactive-mode.zh-CN.md#permissions-and-safety)。

对于每次扫描和接受的发现：

1. 解决变更的基础和头部修订。
2. 针对该差异运行 `$codex-security:security-diff-scan`，而不修改结帐。
3. 保留完整的扫描目录并选择要修复的结果。
4. 对于每个接受的结果调用 `$codex-security:fix-finding` 一次，传递其结果 ID 和完成的扫描目录。
5. 生成一个重点补丁并添加一个回归测试，该测试在修复之前失败并在修复之后通过。如果该测试不安全或不可行，请记录证明差距并使用最强的可重复验证工件。
6. 验证原始问题和合法行为。独立返回每个补丁、测试或后备验证工件、验证命令和任何证明差距。

首先，扫描找零，不修改结帐：

```bash
codex exec --sandbox workspace-write 'Use $codex-security:security-diff-scan to review changes from <base-revision> to <head-revision> for security regressions. Do not modify the checkout.'
```

然后从已完成的扫描中修复一项已接受的发现：

```bash
codex exec --sandbox workspace-write 'Use $codex-security:fix-finding to fix finding <finding-id> from <completed-scan-directory>. Validate the finding, generate one minimal patch, and add a focused regression test that fails before the fix and passes after it. If that test is unsafe or infeasible, record the proof gap and provide the strongest repeatable validation artifact instead. Verify that the issue no longer reproduces.'
```

对于每个剩余的已接受发现，在独立任务或作业中重复第二个命令。验证后，通过正常的代码审查和发布流程合并每个补丁。要在修复之前将调查结果交给其他团队，请参阅 [导出或跟踪结果](export-findings.zh-CN.md)。