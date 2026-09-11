# Global Working Rules

<!-- Usage: Copy this file to the Codex home directory as AGENTS.md; the default path is ~/.codex/AGENTS.md. This is a prepared template; storing it in this repository does not replace existing global instructions. -->

## Scope and Language

- This file contains personal preferences shared across projects, including code annotations and repository organization. Project rules define concrete directory trees, environments, and language styles.
- Use Chinese for responses, comments, commit messages, and suggestions. Preserve code identifiers, commands, paths, configuration keys, and necessary technical terms in their original form.
- Follow the user's explicit current request. Task-specific exceptions override these defaults, subject to higher-priority constraints of the execution environment.

## Reading and Task Organization

- Read existing files before editing them and understand their context. Do not propose specific changes to code you have not read.
- For tasks involving multiple files, modules, or dependencies, establish the goal, constraints, and execution order before implementing and validating each step. Keep single-file changes lightweight and answer pure questions directly.

## Change Scope and Design

- Make only changes requested by the user or necessary to complete the goal. Do not add unrelated features, configuration, refactoring, or optimization.
- Do not opportunistically change surrounding code, add unrelated comments, or reformat files in bulk. Preserve the user's existing changes.
- Avoid premature abstraction; small amounts of similar code are acceptable. Do not introduce abstraction layers or compatibility code solely for hypothetical future needs.
- Validate external input at system boundaries. Trust internal callers when contracts are clear, and do not add defensive checks or fallbacks for impossible scenarios.

## Repository Directory Rules

- Place new files in directories matching their function and purpose, rather than accumulating them at the root. Root-level instructions, dependency manifests, and tool-required configuration are exceptions.
- Keep source code, build artifacts, tests, and documentation in separate categories. Do not store source code in artifact directories.
- Use directory names that clearly express their purpose; avoid vague names such as other or misc.
- Follow an existing repository's structure. Do not reorganize directories or alter tool-generated structures without authorization.
- Follow project rules for concrete directory trees and paths. Do not impose one technology stack's layout on every project.

## Common Code Annotations

- These requirements apply to first-party source code, not obligatorily to third-party or generated code. Explicit project-specific versioning or annotation conventions take precedence.
- Source file headers record the filename or function, description, version, date, and change history. Start new files at `v1.0`; retain actual versions in existing files, and do not invent authors or history.
- Use `YYYY/MM/DD` for dates and `vX.Y` for versions. List history newest first, retain only the latest 3 versions, and match the header version to the latest history entry.
- Above each added or modified logical block, annotate its version, date, action, and explanation. One annotation may cover contiguous code belonging to the same change; do not repeat it on every line.
- Match annotation versions to the file header. For interface changes, identify affected callers in the function header or commit body.

```text
// vX.Y YYYY/MM/DD 新增：说明新增逻辑
// vX.Y YYYY/MM/DD 修改：说明变更内容和原因
// [废弃] vX.Y YYYY/MM/DD 废弃原因，替代方案，计划在 vX.Z 清理
```

- Use `//` in C/C++ and Verilog, `#` in Python, and `%` in MATLAB. Use valid comment syntax for other languages.
- Retain deprecated code as comments, with the reason, replacement, and planned removal version. Remove it after 3 versions, following the operation authorization rules.
- Project rules may specify language-specific header layouts. Do not apply source code annotation formats to ordinary documents.

## Operations Requiring Explicit Authorization

- Before deleting files, directories, or Git branches, or running `git reset --hard`, force-pushing, or running `git rebase`, explain the targets and impact and confirm authorization.
- Clearing database tables, overwriting unbacked-up data, and changing CI/CD pipelines or shared infrastructure also require explicit authorization.
- Do not ask again when the user has already explicitly authorized the specific operation and scope in the current task. Do not treat normal file editing as destructive whole-file replacement.

## Error Handling and Security

- Do not silently swallow errors. Log them appropriately or propagate them; do not hide failures in empty catch clauses or error branches.
- Handle errors that can actually occur. Validate boundary data such as user input, external APIs, network or serial messages, and file contents.
- Never concatenate external input directly into shell commands.
- Do not hardcode keys, passwords, tokens, or certificate private keys in source code. Supply them through environment variables or separate configuration excluded from version control.
- Do not commit sensitive files such as `.env`, `credentials.json`, or `secrets.*`. Add appropriate ignore rules for the project's sensitive files.
- Logs and debug output must not expose credentials or private data. Disable or redact sensitive debug output in production releases.
- Do not introduce third-party libraries with unclear provenance or inadequate maintenance. Record versions in dependency manifests for reproducibility and troubleshooting.

## Git Commit Habits

- By default, create one local `git commit` after each substantive change is complete and necessary validation has been performed. Honor explicit requests not to commit. Do not commit outside a Git repository or when there are no changes.
- Before committing, inspect the working tree, staging area, and diff. Commit only changes belonging to the current task; do not include or revert the user's pre-existing changes for convenience.
- If changes cannot be separated reliably, Git identity is missing, or validation fails, explain the specific obstacle. Do not force a commit or change global Git configuration without authorization.
- Authorization to make local commits does not authorize pushing, publishing, or rewriting remote history.
- Use `<type>(<scope>): <short Chinese description>` for commit messages, with the actual module or subsystem as the scope.
- Use the types `feat`, `fix`, `refactor`, `docs`, `style`, `test`, and `chore`.
- Write the short description as a Chinese imperative, at most 50 characters, such as “新增超时重传逻辑”; avoid completed-action wording such as “新增了……”.
- Explain the impact of interface changes in the commit body. Do not add AI attribution or AI Co-Authored-By lines.

## Validation and Feedback

- Run existing project checks appropriate to the actual changes. Do not invent build or test commands.
- Distinguish completed, verified, and unverified work. Explain failures rather than claiming success.
- Finish with a concise result, validation status, and necessary commit information. Do not repeat the same summary again at the end.

<!-- Sources: ref/all_rules.md, ai-behavior.instructions.md, git-commit.instructions.md, security.instructions.md, code-annotations.instructions.md, and the general sections of error-handling.instructions.md and project-structure.instructions.md.
Editorial note: Preserves the original automatic local commit preference and adds change-ownership checks and failure handling. -->
