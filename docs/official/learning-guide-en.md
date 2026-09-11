# Learning Codex: Foundations

Snapshot date: 2026-09-11. This is an independently written learning summary, not a reproduction of the official documentation. The Chinese companion translates this guide. Official English downloads are available in [the local source index](en/INDEX.md).

## 1. Prompting

Describe the outcome, provide relevant material, specify the deliverable, and mention important limits. For code tasks, include a way to verify success. Refine the result with follow-up instructions. Steering affects ongoing work; queuing saves a request for a later run. A task can start small and grow after you review its first result. [Official source](https://learn.chatgpt.com/docs/prompting) · [Local original](en/prompting.md)

## 2. Personalization

Use personal instructions for recurring preferences and project instructions for requirements specific to a repository. Memories can carry useful context between conversations; essential project rules belong in explicit project guidance. Personality changes communication style. Computer History is an optional macOS feature, so it is not a Windows setup step. Review available controls in the app's personalization settings. [Official source](https://learn.chatgpt.com/docs/personalize) · [Local original](en/personalize.md)

## 3. Skills and plugins

A skill describes a repeatable process and can include resources. A plugin bundles reusable capabilities and may connect tools through MCP servers. Choose a skill for a focused workflow; consider a plugin when that workflow needs packaged tools or service connections. Codex can match skills to requests, or you can invoke one with a `$` mention. Test a reusable workflow before depending on it. [Official source](https://learn.chatgpt.com/docs/skills-and-plugins) · [Local original](en/skills-and-plugins.md)

## 4. Permissions

Sandbox settings determine accessible files and network resources; approval settings determine how requests are reviewed. Automatic review does not itself widen sandbox access. Enabling a permission mode in settings only makes it selectable. Available choices depend on local and organizational configuration. The documentation suggests starting with approval requests for most work. Read the actual permissions shown for your task. [Official source](https://learn.chatgpt.com/docs/permission-modes) · [Local original](en/permission-modes.md)

## 5. Practice in this repository

The following exercises are original suggestions for this learning folder. They have not been executed as part of preparing the archive.

### Exercise A: Ask for a concrete deliverable

Send this request to Codex:

```text
Read docs/official/en/prompting.md. Create practice/my-notes.md with
five questions I should be able to answer after studying it. Give each
question a blank answer area and a link to the relevant source section.
Check that the links point to headings that exist in the local file.
```

Review the saved file. Then ask it to make one question easier and one more challenging. Compare the two versions.

### Exercise B: Draft project guidance

```text
Draft a proposed AGENTS.md for this learning repository. My preferences:
explain concepts in Chinese, preserve official English downloads, and put
my exercises in practice/. Show me the proposed text before creating it.
```

Decide whether those preferences should apply to every future task in this folder. This archive does not install project instructions for you.

### Exercise C: Identify a reusable workflow

Complete three learning notes manually with Codex. Compare their structures. If the same inputs, steps, and outputs keep recurring, describe that process as a candidate skill. Keep its first version limited to one kind of note.

### Exercise D: Inspect changes

```text
Inspect this repository's Git status. Explain which files are new and which
tracked files changed. For each change, tell me how I can check it. Do not
create a commit as part of this inspection.
```

## 6. Git basics for this folder

This folder uses Git for version control. Initialization creates repository metadata; it does not upload files or create a saved revision. Use the commands below to inspect the current branch and changes.

Run these commands from `E:\myfile\use_codex` when you want to inspect the repository:

```powershell
git status
git diff
```

`git status` lists untracked files and changes. `git diff` shows unstaged changes to tracked files; it does not display the contents of new, untracked files. Open those files directly.

After reviewing the files, you can save a revision:

```powershell
git add .
git diff --cached --stat
git commit -m "Add local documentation and learning guides"
```

If Git requests an identity, configure your own name and email for this repository before committing. Use your actual preferred details; this task does not invent an identity. A local commit does not publish anything to GitHub.

## 7. Reading route and offline limits

Start with the four Foundations originals, in the order above. English documentation is stored in `en/`, and all 148 full Chinese translations are stored in `zh-CN/`. Use [the local index](en/INDEX.md) for the broader English documentation set. See the [Chinese index](zh-CN/INDEX.md) for translations and the [file guide](README.md) for a Chinese explanation of each document.

This repository retains Markdown documents for reading. Images, videos, linked external sites, and interactive demonstrations are not fully available offline. Some official Markdown exports contain component placeholders; open the original webpage for those elements.

The documents are based on material downloaded on 2026-09-11; later product behavior may differ. The full Chinese translations are unofficial, and the learning guides are independently written summaries.
