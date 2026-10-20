# 04 — CLI Navigation and Essential Commands

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 1 — Codex Fundamentals  
> **Lesson:** 1.4 — CLI Navigation and Essential Commands  
> **Difficulty:** Beginner  
> **Platform:** Windows / PowerShell  
> **Status:** In Progress  
> **Prerequisite:** [[03 - Codex CLI Installation and Environment Setup]]

---

## 1. Introduction

Codex CLI gives developers a terminal-based way to work with an AI coding agent. You can start a session, provide instructions, inspect session information, and review changes.

There are three command categories to recognize:

1. **PowerShell commands** manage the operating system and folders.
2. **Codex CLI commands and flags** start or configure Codex from PowerShell.
3. **Slash commands** control an active Codex session.

This lesson is a reference for understanding the interface. The examples are for later practice; no commands need to be run now. Codex changes over time, so check `codex --help` and the in-session command picker for your installed version.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Tell PowerShell commands, CLI commands, and slash commands apart.
- Navigate folders and identify the current working directory.
- Understand how to start, inspect, and exit a Codex session.
- Recognize common CLI flags and session commands.
- Explain interactive versus non-interactive use.
- Understand why Git is useful when reviewing agent changes.

## 3. Three Command Categories

### PowerShell commands

These run in the terminal and work with Windows or the local environment:

```powershell
Get-Location
Get-ChildItem
Set-Location C:\Projects
```

### Codex CLI commands

These also run in PowerShell, but start or manage Codex:

```powershell
codex
codex --help
codex --version
codex resume --last
```

### Interactive slash commands

These are entered inside an active Codex session:

```text
/status
/model
/review
```

| Category | Where it runs | Purpose |
|---|---|---|
| PowerShell | Terminal | Navigate or manage the local environment |
| Codex CLI | Terminal | Start and configure Codex |
| Slash command | Active Codex session | Control the current session |

## 4. PowerShell Navigation Essentials

| Command | Purpose |
|---|---|
| `Get-Location` or `pwd` | Show the current directory |
| `Get-ChildItem` or `ls` | List files and folders |
| `Set-Location <path>` or `cd <path>` | Change directory |
| `cd ..` | Move to the parent directory |
| `Get-Content <file>` | Read a text file |
| `Clear-Host` or `cls` | Clear the terminal display |

For example, check the current location before starting Codex:

```powershell
Get-Location
```

The **working directory** is the folder from which a command runs. Starting Codex in the intended project folder helps it find the right project context.

## 5. Starting Codex and Using CLI Flags

Start an interactive session from the current directory:

```powershell
codex
```

You can also set a project directory and provide an initial prompt:

```powershell
codex -C "C:\Projects\Sample" "Explain this project's structure. Do not modify files."
```

`codex --help` lists options supported by the installed version. Common global flags include:

| Flag | Purpose |
|---|---|
| `--help` | Show command help |
| `--version` | Show the installed version |
| `-C`, `--cd` | Set the working directory |
| `-m`, `--model` | Choose an available model |
| `-s`, `--sandbox` | Select a sandbox policy, such as `read-only` |
| `-a`, `--ask-for-approval` | Set when Codex asks before running commands |
| `-c`, `--config` | Override a configuration value for this invocation |

Flag values and combinations can change. Confirm them with `codex --help` and the official CLI reference. For learning and analysis, a restrictive sandbox such as `read-only` is appropriate when available.

## 6. Interactive Session Commands

Type `/` in the Codex composer to open and filter the slash-command picker. Available commands depend on the Codex version, interface, account, and configuration.

| Slash command | Purpose |
|---|---|
| `/status` | Show session details such as chat ID, context usage, or rate limits |
| `/model` | Choose the active model, where available |
| `/plan` | Switch to planning mode |
| `/review` | Review uncommitted changes or compare with a base branch |
| `/compact` | Summarize older context to make room in the conversation |
| `/exit` | Leave the interactive session |

Some versions also provide `/diff` to inspect working-tree changes. Check the command picker for availability. For CLI flags and subcommands, use `codex --help` in PowerShell; slash commands are not PowerShell commands.

## 7. Interactive and Non-Interactive Use

### Interactive

```powershell
codex
```

Use an ongoing conversation to explore a project, ask follow-up questions, or review an agent’s actions. This is the recommended starting point for learning.

### Non-interactive

```powershell
codex exec "Explain this repository. Do not modify files."
```

`codex exec` runs a task without opening the interactive terminal UI. It is useful for scripted or CI workflows and is less suitable when you want to guide each step as a beginner.

## 8. Resuming Sessions

Resume an interactive session with a picker:

```powershell
codex resume
```

Resume the most recent session for the current working directory:

```powershell
codex resume --last
```

You can also resume by session ID where supported:

```powershell
codex resume SESSION_ID
```

Session history stores conversation context; it is separate from project files and Git history. Resuming a chat does not restore an earlier file version. The `--last` option normally selects from the current working directory; consult help for options such as `--all`.

## 9. Git as a Change Review Tool

When working in a Git repository, these commands help you understand the project state:

```powershell
git status
git diff
git log --oneline -5
git branch --show-current
```

`git status` shows changed files, and `git diff` shows edits to tracked files. Review changes before deciding whether to keep or commit them. A successful test run alone does not prove every behavior is correct.

## 10. Example Read-Only Workflow

1. Check the working directory with `Get-Location`.
2. Check repository changes with `git status` if the folder is a Git repository.
3. Start Codex with `codex`.
4. Use `/status` to inspect the session.
5. Ask: “Analyze the project structure and explain its main directories and entry points. Do not create, modify, or delete files.”
6. Review the explanation against the project.
7. Exit with `/exit` and check `git status` again.

This is a reference workflow for a future practice session; it does not need to be performed as part of this lesson.

## 11. Common Mistakes

### Entering a slash command in PowerShell

`/status` belongs inside an active Codex session. In PowerShell, use `codex --help` to see CLI options.

### Starting from the wrong directory

Check the location with `Get-Location` before launching Codex.

### Assuming every command is available

Commands can vary by version and environment. Use `codex --help` and type `/` in the session to check.

### Confusing session history with project history

Codex resume continues a conversation. Git tracks project changes and commits.

### Assuming a review or test guarantees correctness

Treat agent output and test results as evidence to review, not as a guarantee.

## 12. Key Takeaways

1. PowerShell commands, Codex CLI commands, and slash commands run in different places.
2. The working directory determines which project Codex starts with.
3. Use `codex --help` and the slash-command picker to check version-specific features.
4. Interactive sessions are useful for guided exploration; `codex exec` is intended for non-interactive tasks.
5. Resuming a session does not roll back or restore project files.
6. Git helps inspect and track changes made during development.

## 13. Knowledge Check

1. Where do PowerShell commands, Codex CLI commands, and slash commands run?
2. How can you check the current working directory?
3. What does the `-C` / `--cd` flag do?
4. How do you open the interactive slash-command picker?
5. How does `codex exec` differ from `codex`?
6. What does `codex resume --last` do?
7. Why should you inspect Git changes after a coding-agent task?

## Official References

- [Codex CLI guide](https://learn.chatgpt.com/docs/codex/cli)
- [Codex CLI command-line reference](https://learn.chatgpt.com/docs/cli/reference)
- [Codex CLI developer commands](https://learn.chatgpt.com/docs/developer-commands)

## Next Lesson

**[[05 - Prompting Fundamentals]]**

---

*Phase 1 — Codex Fundamentals | Lesson 1.4*  
*Learning Principle: Understand the interface before delegating complex development tasks.*
