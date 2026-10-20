# 03 — Codex CLI Installation and Environment Setup

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 1 — Codex Fundamentals  
> **Lesson:** 1.3 — Codex CLI Installation and Environment Setup  
> **Difficulty:** Beginner  
> **Platform:** Windows 10/11  
> **Terminal:** Windows PowerShell  
> **Status:** In Progress  
> **Prerequisite:** [[02 - Codex Architecture and Agentic Coding Concepts]]

> **Study note:** This lesson is a reference and learning guide. If Codex is already installed and working, there is no need to reinstall it. We will not install or run anything until we choose to do the practice together.

## 1. Introduction

**Codex CLI** is OpenAI’s coding agent for working from a terminal. It can help explain a project, inspect files, make changes, or run commands, depending on the task, tools, and permissions available.

To understand CLI setup on Windows, it helps to know about PowerShell, installation methods, Node.js and npm, PATH, authentication, and the project directory. This lesson explains those pieces and how to check an existing setup.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Identify the parts of a Windows environment used with Codex CLI.
2. Explain why Node.js and npm are needed for npm installation.
3. Explain how PATH helps PowerShell locate commands.
4. Recognize the supported installation options.
5. Verify an existing Codex installation and authentication state.
6. Navigate to a project directory in PowerShell.
7. Understand how to start and inspect an interactive session.
8. Recognize common setup issues.

## 3. The Development Environment

A development environment is the set of software, tools, settings, and files used for development.

| Component | Purpose |
|---|---|
| **Windows** | Operating system |
| **PowerShell** | Terminal and command shell |
| **Codex CLI** | Coding agent used from the terminal |
| **Node.js and npm** | Required when installing Codex through npm |
| **Git** | Tracks project history and changes |
| **Editor or IDE** | Opens and edits project files |
| **Project directory** | Contains the application and its files |

Not every installation method needs every component. For example, Node.js and npm are needed for npm-based installation, but not for the standalone Windows installer.

### How the pieces fit together

```text
Windows
   └── PowerShell
        └── Codex CLI
             └── Project directory and available development tools

Optional for npm installation:
Node.js ── npm ── Codex CLI
```

## 4. What Is a CLI?

A **Command-Line Interface (CLI)** lets a person use software by typing commands in a terminal.

Examples in PowerShell:

```powershell
Get-Location
```

Shows the current working directory.

```powershell
Get-ChildItem
```

Lists files and folders in the current directory.

Codex CLI fits into terminal workflows such as reading project information, using development tools, and inspecting Git changes. Commands that modify files or run programs should be understood before they are allowed.

## 5. System and Setup Requirements

For this learning track, the platform is Windows 10/11 with PowerShell. Codex CLI is also documented for other environments, including macOS, Linux, and WSL; setup instructions can differ by environment.

You will generally need:

- A supported environment and a terminal
- An internet connection for sign-in and model access
- A project folder if you want Codex to work with a local project
- Node.js and npm only if you choose npm installation

Codex features and installation options can change, so use the official documentation for the current instructions.

## 6. Node.js and npm

### Node.js

**Node.js** is a runtime that can execute JavaScript outside a web browser. It is used by many development tools and command-line applications.

### npm

**npm (Node Package Manager)** installs and manages JavaScript packages. It is commonly installed with Node.js.

Codex CLI is published as the npm package `@openai/codex`. Node.js and npm are needed for this installation method, but not for every Codex installer.

To check whether they are available:

```powershell
node --version
npm --version
```

The commands print the installed versions. The version numbers change over time, so these examples do not specify a required version.

## 7. Codex CLI Installation Options

**If Codex is already installed and working, skip this section.** Do not install another copy just to follow the lesson.

OpenAI documents a standalone Windows installer and npm installation. Check the [current Codex CLI setup guide](https://learn.chatgpt.com/docs/codex/cli) before installing, because instructions may change.

### Option A — Standalone Windows installer

The Windows installer is distributed as a PowerShell script. The documented command is:

```powershell
irm https://chatgpt.com/codex/install.ps1 | iex
```

This downloads a script and executes it in PowerShell. Only use the official URL and current official instructions. We will not run this command as part of the lesson.

### Option B — npm

If Node.js and npm are installed, Codex can be installed globally:

```powershell
npm install -g @openai/codex
```

To request the latest published package version:

```powershell
npm install -g @openai/codex@latest
```

Here, `-g` means a global npm installation. The command must still be discoverable through the shell’s PATH.

### Choosing an installation method

Use the current official instructions for a new installation. If Codex is already installed, first identify how it was installed. Avoid installing multiple copies by different methods unless you know which one PowerShell will run.

## 8. Environment Variables and PATH

An **environment variable** is a named setting that programs can read. Examples include `PATH`, `TEMP`, and `USERPROFILE`.

The **PATH** variable contains folders that Windows searches when you type a command without its full location. When you type `codex`, PowerShell looks for a matching command in locations it knows about.

View the PATH value for the current PowerShell session:

```powershell
$env:PATH
```

See which Codex command PowerShell resolves:

```powershell
Get-Command codex
```

To list all matching commands:

```powershell
Get-Command codex -All
```

You can also search executable paths with:

```powershell
where.exe codex
```

If PowerShell says that `codex` is not recognized, possible causes include an incomplete installation, a location missing from PATH, or a terminal that was opened before installation finished. First confirm the installation method and location; do not change PATH blindly.

## 9. Verify an Existing Installation

These checks help confirm what is installed without reinstalling it.

### Check the version

```powershell
codex --version
```

### View command-line options

```powershell
codex --help
```

### Locate the command

```powershell
Get-Command codex
```

### Check an npm installation

```powershell
npm list -g @openai/codex --depth=0
```

This only checks the global npm packages. A standalone installation will not necessarily appear in this list.

A useful verification confirms that PowerShell can find Codex, the command starts, and the installed version is visible.

## 10. Authentication

**Authentication** verifies which user or credential is being used. The available sign-in options depend on the current Codex setup and account.

For an interactive first run, start Codex and follow the sign-in prompt:

```powershell
codex
```

To check the active authentication mode from PowerShell:

```powershell
codex login status
```

Codex can support ChatGPT sign-in or API-key authentication in supported configurations. API keys are secret credentials: do not paste them into shared notes, commit them to Git, or share them in screenshots.

### Authentication and authorization

- **Authentication:** Who is signed in?
- **Authorization:** What is that user or agent allowed to do?

Signing in does not by itself grant unrestricted file or command access. Permissions and sandbox settings also matter.

## 11. The Working Directory

The **working directory** is the current folder for a terminal session. It affects which project files Codex can find and how relative paths are interpreted.

Check the current directory:

```powershell
Get-Location
```

Move to a folder:

```powershell
cd C:\Users\Acer\Documents
```

List its contents:

```powershell
Get-ChildItem
```

Start Codex from a project directory when you want it to understand that project:

```powershell
cd C:\path\to\your\project
codex
```

The path above is an example; replace it with the real project path. Before starting a task, make sure the terminal is in the intended directory. Actual access is still controlled by Codex’s environment and permissions.

## 12. Inspect an Interactive Session

When Codex is running, slash commands can show or change session information. The available commands can vary by version.

- `/status` shows the current session configuration.
- `/permissions` opens the available permission controls.
- `/model` selects the model and reasoning effort.
- `/exit` or `/quit` exits the CLI.
- `?` shows keyboard shortcuts in the interactive interface.

For general command-line options, use `codex --help` before entering the interactive session. Use `/status` to check the session once Codex is running.

## 13. Common Setup Problems

### “Codex is not recognized”

Check that the command is installed and discoverable:

```powershell
Get-Command codex -All
where.exe codex
```

Confirm the installation method and restart PowerShell after installation if needed.

### “npm is not recognized”

Check whether Node.js and npm are available:

```powershell
node --version
npm --version
```

This applies to npm installation; it is not a reason to install Node.js if you use the standalone installer.

### Authentication does not work

Check the status:

```powershell
codex login status
```

Then follow the sign-in instructions in Codex and consult the [authentication documentation](https://learn.chatgpt.com/docs/auth).

### Codex opens in the wrong project

Check the current directory:

```powershell
Get-Location
```

Navigate to the intended project directory before starting Codex.

### More than one installation is found

List all commands PowerShell can resolve:

```powershell
Get-Command codex -All
where.exe codex
```

Identify which path is being used before removing or changing an installation.

## 14. Optional Practice — Verify, Don’t Modify

This practice is optional. It is for a time when you are ready to use the CLI; you do not need to install or run Codex now.

1. Open PowerShell.
2. Check `node --version`, `npm --version`, and `codex --version`.
3. Check `Get-Command codex` to see which installation PowerShell finds.
4. Check `codex login status` without sharing any credential details.
5. Use `Get-Location` to confirm the current directory.
6. If you later choose to start Codex, use `/status` and `/permissions` to inspect the session.
7. Ask Codex to explain the current workspace and explicitly tell it not to create, edit, or delete files.

Reflection questions:

- Which installation method appears to be in use?
- Where is the Codex command located?
- Is the current directory the one you intended?
- What does the authentication status report?
- Did Codex respect the read-only instruction?

## 15. Key Terms

| Term | Plain-language meaning |
|---|---|
| **CLI** | A text-based way to use a program by entering commands |
| **Terminal** | An application for running command-line programs |
| **PowerShell** | A command shell and scripting environment for Windows |
| **Node.js** | A runtime for running JavaScript outside a browser |
| **npm** | A package manager for JavaScript software |
| **Global install** | An npm package install intended to make a command available across projects |
| **PATH** | A list of folders searched for commands |
| **Executable** | A program the operating system can run |
| **Authentication** | Verifying a user or credential |
| **Authorization** | Determining what actions are allowed |
| **Working directory** | The current folder used by commands |
| **Environment variable** | A named setting available to programs |

## 16. Key Takeaways

1. Codex CLI runs from a terminal and works with the environment available to it.
2. Node.js and npm are needed for npm installation, but not for every installer.
3. PATH helps PowerShell find the `codex` command.
4. Check an existing installation before reinstalling or changing settings.
5. `codex login status` checks the active authentication mode.
6. The working directory matters when Codex is asked to inspect a local project.
7. Sign-in and file/command permissions are separate concepts.
8. Start by understanding the environment and using read-only requests.

## 17. Knowledge Check

1. What does a CLI let you do?
2. What is the difference between Node.js and npm?
3. Is Node.js required for every Codex installation method?
4. What does `-g` mean in an npm installation command?
5. What is PATH used for?
6. How can you check the Codex version and executable location?
7. What does `codex login status` tell you?
8. How are authentication and authorization different?
9. Why should you check the working directory before starting Codex?
10. Why is a read-only request a useful way to begin?

## 18. Official References

- [OpenAI Docs — Codex CLI](https://learn.chatgpt.com/docs/codex/cli)
- [OpenAI Docs — CLI commands](https://learn.chatgpt.com/docs/developer-commands?surface=cli)
- [OpenAI Docs — Authentication](https://learn.chatgpt.com/docs/auth)
- [OpenAI Docs — Environment variables](https://learn.chatgpt.com/docs/config-file/environment-variables)
- [OpenAI Codex GitHub repository](https://github.com/openai/codex)
- [Node.js](https://nodejs.org/)
- [npm documentation](https://docs.npmjs.com/)
- [Microsoft PowerShell documentation](https://learn.microsoft.com/powershell/)

## Next Lesson

**[[04 - CLI Navigation and Essential Commands]]**

---

*Phase 1 — Codex Fundamentals | Lesson 1.3*  
*Learning principle: Understand and verify your environment before using an AI coding agent.*
