# 05 — Global vs Project-Specific Configuration

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 4 — Codex Configuration
> **Lesson:** 4.5 — Global vs Project-Specific Configuration
> **Difficulty:** Intermediate
> **Environment:** Windows, PowerShell, VS Code, Codex CLI
> **Prerequisite:** [[04 - Sandbox and Approval Policies]]
> **Next Lesson:** [[06 - Configuration Troubleshooting]]
> **Status:** Theory Documented

---

## 1. Introduction

Codex can read settings from several places. A developer may use it across Android, Laravel, Next.js, and learning projects, and not every setting should apply everywhere.

The main configuration layers are:

1. **User configuration** — personal defaults shared across projects.
2. **Project configuration** — supported overrides in a trusted repository.
3. **Profiles** — reusable named configuration variants.
4. **CLI overrides** — temporary settings for one invocation.

Codex combines applicable settings according to precedence rules. Project instructions such as `AGENTS.md` remain a separate layer.

**Core Principle: Keep reusable preferences global, supported project-specific settings local, and temporary changes on the command line.**

---

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define global and project-specific configuration.
2. Explain configuration hierarchy and precedence.
3. Locate user-level and project-level configuration.
4. Explain trusted and untrusted projects.
5. Understand nested project configurations and working-directory effects.
6. Use CLI overrides and profiles.
7. Distinguish configuration settings from project instructions.
8. Identify managed defaults and enforced requirements.
9. Inspect configuration files using PowerShell.
10. Troubleshoot configuration conflicts.
11. Design a maintainable multi-project configuration strategy.

---

## 3. Configuration Hierarchy

Codex can receive settings from built-in defaults, system or cloud configuration, user configuration, profiles, trusted project files, and command-line options.

```text
CLI flags and -c/--config overrides
                ↓
Trusted project .codex/config.toml files
                ↓
Selected profile
                ↓
User config.toml
                ↓
Cloud-managed defaults
                ↓
System configuration
                ↓
Built-in defaults
```

This is the documented order for resolving configuration values. Some managed requirements enforce restrictions separately and cannot be bypassed by ordinary overrides. See the [Codex configuration basics](https://developers.openai.com/codex/config-basic/) and [configuration reference](https://developers.openai.com/codex/config-reference/).

**Key Principle: Effective configuration is not necessarily identical to any one configuration file.**

---

## 4. User-Level Configuration

The default user configuration is:

```text
~/.codex/config.toml
```

On Windows, this is commonly:

```text
C:\Users\Acer\.codex\config.toml
```

If `CODEX_HOME` is set, Codex uses that directory instead of the default `.codex` location.

Example personal defaults:

```toml
model_reasoning_effort = "medium"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

Keep user configuration for preferences that make sense across repositories. Avoid putting one project's architecture rules here.

---

## 5. Project-Specific Configuration

A trusted project can contain supported overrides in:

```text
.codex/config.toml
```

Example:

```text
GroupTrack/
├── .codex/
│   └── config.toml
├── AGENTS.md
├── docs/
└── app/
```

A project may set a supported value for its own work. Project configuration is intentionally limited: some machine-local provider, authentication, notification, telemetry, and profile-selection settings are ignored when placed in a project `.codex/config.toml`. Check the [configuration reference](https://developers.openai.com/codex/config-reference/) before copying user-level keys into a project file.

Project configuration is loaded only for trusted projects. It can be committed when the settings are appropriate for everyone working in that repository; do not commit personal paths, credentials, or machine-specific values.

---

## 6. Global vs Project Configuration

| Aspect | User Configuration | Project Configuration |
|---|---|---|
| Default location | `$CODEX_HOME/config.toml` or `~/.codex/config.toml` | `<project>/.codex/config.toml` |
| Scope | User environment | Trusted project and applicable descendants |
| Purpose | Reusable personal defaults | Supported repository-specific overrides |
| Trust requirement | No project trust needed | Project must be trusted |
| Shared through Git | Normally kept private | May be committed intentionally |
| Typical content | General model or permission defaults | A supported setting needed only for this project |

Global configuration is not a substitute for project documentation. Project configuration is not a place to duplicate all coding instructions.

---

## 7. Configuration Precedence

The current documented order, highest to lowest, is:

| Priority | Source |
|---|---|
| 1 | CLI flags and `-c` / `--config` overrides |
| 2 | Trusted project `.codex/config.toml` files; root toward current directory, closest applicable file wins |
| 3 | Selected profile file |
| 4 | User `config.toml` |
| 5 | Cloud-managed defaults, when supplied |
| 6 | System configuration, when present |
| 7 | Built-in defaults |

Enforced requirements are separate constraints. A higher-precedence setting does not automatically override an organization-enforced restriction.

### Example

User configuration:

```toml
model_reasoning_effort = "medium"
```

Trusted project configuration:

```toml
model_reasoning_effort = "high"
```

When that project setting is supported and no higher-priority override applies, the project value wins. A CLI override can temporarily take priority:

```powershell
codex -c 'model_reasoning_effort="low"'
```

Assume the selected model supports the requested value.

---

## 8. Trusted and Untrusted Projects

Project trust is a security decision. A repository can contain `.codex/` configuration, hooks, and rules that influence a Codex session.

For an untrusted project, Codex skips project-scoped `.codex/` layers, including local configuration, hooks, and rules. User and system configuration still apply. Trusting a project does not make its source code safe; it only permits the applicable project-scoped settings to load.

### Recommended workflow for an unfamiliar repository

1. Inspect its files and project configuration.
2. Review its tools and scripts.
3. Decide whether you understand and trust its contents.
4. Grant project trust only when there is a reason to load its local Codex settings.

Do not trust a repository merely because it contains `AGENTS.md` or `.codex/`.

---

## 9. Nested Project Configuration

Codex can load project configuration along the path from the project root to the current working directory. The closest applicable project configuration takes precedence over more distant project configuration.

```text
ecommerce/
├── .codex/config.toml
├── frontend/
│   └── .codex/config.toml
└── backend/
    └── .codex/config.toml
```

If Codex is started inside `frontend`, the root and frontend files may both apply, with the frontend file taking precedence for conflicting supported keys. Starting from the root can mean the nested frontend file is not on the applicable path.

Check the working directory with:

```powershell
Get-Location
```

Do not add nested configuration simply because folders exist. Use it only when a supported setting truly needs to vary by subproject.

---

## 10. CLI Overrides

CLI overrides are temporary and do not change saved configuration.

### Model

```powershell
codex --model SUPPORTED_MODEL_ID
```

Replace the placeholder with a model available to the signed-in account or workspace.

### Reasoning effort

```powershell
codex -c 'model_reasoning_effort="high"'
```

### Sandbox

```powershell
codex --sandbox read-only
```

Combine options when needed:

```powershell
codex --sandbox read-only -c 'model_reasoning_effort="high"'
```

This requests a read-only session with high reasoning effort. Supported values depend on the installed client and selected model. CLI options cannot bypass enforced requirements.

---

## 11. Configuration Profiles

A profile is a reusable configuration variant stored beside the user `config.toml`.

For example, if the Codex home is the default `~/.codex`:

```text
~/.codex/
├── config.toml
├── routine.config.toml
└── deep-review.config.toml
```

`deep-review.config.toml`:

```toml
model_reasoning_effort = "high"
```

Select it with:

```powershell
codex --profile deep-review
```

The profile file naming pattern is `$CODEX_HOME/profile-name.config.toml`. Profiles are useful for recurring alternatives; use a CLI override for a one-time exception. Trusted project settings and CLI overrides can take precedence over profile values. Do not use obsolete examples that place profiles in `[profiles.name]` tables in the main file.

---

## 12. Managed Defaults and Enforced Requirements

Organizations may supply configuration defaults or enforce requirements. Defaults can participate in normal configuration precedence. Enforced requirements restrict allowed behavior independently, and local settings may not weaken them.

If a setting appears not to apply, check whether it is being constrained by managed requirements before changing configuration files.

---

## 13. Configuration vs `AGENTS.md` vs Prompt

| Layer | Answers | Examples |
|---|---|---|
| `config.toml` | How Codex is configured to operate | Model, sandbox, approval defaults |
| `AGENTS.md` | How work should follow project conventions | Architecture, coding style, tests, scope |
| Task prompt | What should happen now | Fix this bug, update this screen, inspect these files |

Example project rule:

```markdown
- Follow the existing architecture.
- Avoid unnecessary dependencies.
- Run relevant tests after changes.
- Do not commit or push without authorization.
```

These are instructions, not TOML configuration keys.

---

## 14. Windows PowerShell: Inspect Configuration

Find the active Codex home and user configuration path:

```powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}

$userConfig = Join-Path $codexHome "config.toml"
$userConfig
Test-Path $userConfig
```

Read it only if it exists:

```powershell
if (Test-Path $userConfig) {
    Get-Content $userConfig
}
```

For project configuration, first navigate to the intended repository root:

```powershell
Set-Location "C:\Users\Acer\AndroidStudioProjects\GroupTrack"
$projectConfig = Join-Path (Get-Location) ".codex\config.toml"
Test-Path $projectConfig
```

Inspect existing files before editing. Back up an existing user configuration before changing it, and avoid committing personal backups. Use the current Codex documentation or installed client to confirm supported keys; parsing valid TOML alone does not prove a key is supported.

---

## 15. Multi-Project Configuration Examples

### GroupTrack Android

Keep Java/XML conventions, MVP boundaries, Gradle workflow, and architecture instructions in `AGENTS.md`. Add a project configuration only for a supported setting that should differ for this trusted project.

### Laravel backend

Keep Laravel architecture, validation conventions, database safety, and test instructions in `AGENTS.md`. Use read-only CLI settings for investigations and workspace-write only for approved edits. Do not put secrets in project configuration.

### Next.js frontend

Keep App Router, TypeScript, styling, and validation instructions in `AGENTS.md`. Use project configuration only for supported recurring overrides, not for the full frontend standards.

### Separate repositories

Check actual repository roots before assuming a shared parent directory provides a common project configuration. Separate Git repositories normally have separate project configuration and trust decisions.

---

## 16. Troubleshooting Configuration Conflicts

### A global setting appears ignored

Check, in order:

1. CLI flags and `-c` overrides.
2. Trusted project configuration along the current directory path.
3. Selected profile.
4. User configuration.
5. Managed or system settings and enforced requirements.

### Project configuration is ignored

Check that:

- The project is trusted.
- The file is named `.codex/config.toml` and is on the applicable path.
- The current working directory is where expected.
- The key is supported at project scope.
- The TOML syntax is valid.

### Profile value is not effective

Check the selected profile name and file at `$CODEX_HOME/profile-name.config.toml`, then check for project and CLI overrides.

### TOML syntax error

Strings need quotes:

```toml
model_reasoning_effort = "medium"
```

Do not define the same key twice in one table. After syntax is valid, confirm the key and value are supported by the installed Codex version.

---

## 17. Recommended Strategy

1. Keep user configuration small and reusable.
2. Add project configuration only for supported settings with a real project-specific need.
3. Review and trust project configuration deliberately.
4. Use profiles for recurring workflows and CLI overrides for one-off changes.
5. Put architecture, coding standards, and repository workflows in `AGENTS.md`.
6. Check current docs when behavior or supported settings may have changed.
7. Avoid committing machine-specific configuration or personal secrets.

---

## 18. Practical Exercises

### Exercise A — Classify the setting

Choose the appropriate place: user configuration, project configuration, profile, CLI override, or `AGENTS.md`.

1. A default reasoning effort shared by all repositories.
2. A supported setting needed only for one trusted project.
3. A temporary read-only investigation.
4. Android Java and XML conventions.
5. A recurring deep-review setup.
6. A one-time model selection.

### Exercise B — Work out precedence

Given a user value of `low`, a selected profile value of `medium`, a trusted project value of `high`, and a CLI override of `low`, identify the effective value. Then remove the CLI override and consider what changes if the project is untrusted. Assume the model supports all values and no enforced rule applies.

### Exercise C — Inspect the configuration paths

Use PowerShell to identify `CODEX_HOME`, check whether the user `config.toml` exists, confirm the current directory, and check for a project `.codex/config.toml`. Do not expose secrets when sharing the results.

### Exercise D — Decide whether to trust

Compare a repository you maintain, a reviewed team repository, and an unfamiliar downloaded repository. For each, explain whether project-scoped Codex settings need to load and what you would inspect first.

### Exercise E — Design a multi-project setup

Design a minimal setup for GroupTrack Android, GoPasig Laravel, and a Next.js frontend. Identify which settings are global, project-specific, temporary, and instructional. Avoid duplicating settings without a reason.

---

## 19. Knowledge Check

1. What is global configuration?
2. What is project-specific configuration?
3. What is the default user configuration path, and how can `CODEX_HOME` affect it?
4. What is configuration precedence?
5. Which source normally has the highest precedence?
6. What is a trusted project?
7. What happens to project-scoped `.codex/` files in an untrusted project?
8. Why does the working directory matter?
9. What is a CLI override?
10. Where are profile files stored and how are they selected?
11. Can every setting be overridden in a project configuration?
12. How do managed defaults differ from enforced requirements?
13. How does `AGENTS.md` differ from `config.toml`?
14. Why should global configuration remain minimal?
15. How would you investigate an unexpected effective setting?

---

## 20. Key Terminologies

| Term | Definition |
|---|---|
| User Configuration | User-level Codex settings, usually `config.toml` under `CODEX_HOME` |
| Project Configuration | Supported settings in a trusted project's `.codex/config.toml` |
| Configuration Precedence | Priority order used to resolve settings |
| Effective Configuration | Settings actually applied for a session |
| Trusted Project | Project allowed to load project-scoped Codex settings |
| Nested Configuration | Project settings found at more than one directory level |
| CLI Override | Temporary setting supplied for one invocation |
| Profile | Reusable configuration variant selected by name |
| Managed Default | Centrally supplied default setting |
| Enforced Requirement | Restriction that ordinary local overrides cannot weaken |
| `CODEX_HOME` | Environment variable selecting Codex's home directory |
| `AGENTS.md` | Project instructions for agent behavior and development conventions |

---

## 21. Key Takeaways

1. Codex combines settings from several configuration layers.
2. User configuration provides reusable personal defaults.
3. Trusted project configuration can override supported settings for that repository.
4. CLI flags and `-c` overrides have the highest ordinary precedence.
5. The closest applicable project configuration takes precedence over more distant project files.
6. Untrusted projects do not load project-scoped `.codex/` configuration, hooks, or rules.
7. Project configuration supports only a subset of settings.
8. Profiles are separate files under `$CODEX_HOME`, selected using `--profile`.
9. Managed enforced requirements are distinct from ordinary configuration precedence.
10. `AGENTS.md` holds project conventions; prompts describe the current task.
11. Use the working directory and trust status when troubleshooting unexpected settings.
12. Keep configuration small, intentional, and free of secrets.

---

## 22. Official References

1. [OpenAI — Codex Configuration Basics](https://developers.openai.com/codex/config-basic/)
2. [OpenAI — Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
3. [OpenAI — Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced/)
4. [OpenAI — Codex Environment Variables](https://developers.openai.com/codex/config-reference/#environment-variables)
5. [OpenAI — AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
6. [TOML Official Specification](https://toml.io/en/)

> **Documentation Note:** Configuration precedence, supported project keys, profile behavior, and trust handling can evolve. Confirm current behavior against the installed Codex version and official documentation.

---

## 23. Lesson Completion Checklist

- [ ] Understand global and project-specific configuration.
- [ ] Understand configuration precedence and effective settings.
- [ ] Locate user and project configuration files.
- [ ] Understand `CODEX_HOME`.
- [ ] Understand project trust and untrusted-project behavior.
- [ ] Understand nested project configuration.
- [ ] Use CLI overrides appropriately.
- [ ] Understand configuration profiles.
- [ ] Distinguish managed defaults from enforced requirements.
- [ ] Keep project instructions in `AGENTS.md`.
- [ ] Inspect configuration safely using PowerShell.
- [ ] Troubleshoot unexpected settings by checking precedence.
- [ ] Complete the practical exercises and knowledge check.

---

## Next Lesson

**[[06 - Configuration Troubleshooting]]**

The next lesson covers invalid TOML, unsupported keys, model compatibility, precedence conflicts, trust issues, sandbox and approval settings, Windows environment problems, and configuration recovery.

---

*Phase 4 — Codex Configuration | Lesson 4.5*
*Learning Principle: Place settings at the right configuration layer, understand precedence, and keep overrides intentional.*
