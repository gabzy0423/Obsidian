# 01 — Introduction to Codex Configuration

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 4 — Codex Configuration  
> **Lesson:** 4.1 — Introduction to Codex Configuration  
> **Difficulty:** Intermediate  
> **Environment:** Windows, PowerShell, Codex CLI  
> **Prerequisite:** [[07 - Git Integration and Code Review]]  
> **Next Lesson:** [[02 - Understanding config.toml]]

---

## 1. Introduction

The earlier phases covered installing Codex, providing project context, exploring codebases, planning changes, and reviewing results. This lesson introduces **configuration**: reusable settings that affect how Codex operates in a development environment.

Configuration can set a default model, reasoning effort, sandbox behavior, approval policy, and integrations. Codex CLI uses TOML configuration files. The user-level file is commonly `~/.codex/config.toml`; on Windows with the default home directory, this is typically `C:\Users\Acer\.codex\config.toml`.

**Core principle:** Configuration controls Codex’s operating settings; project instructions explain how Codex should work within that project.

---

## 2. Learning Objectives

By the end of this lesson, learners should be able to:

1. Explain what Codex configuration controls and why it matters.
2. Locate the user-level configuration file and understand TOML basics.
3. Distinguish configuration from prompts and `AGENTS.md`.
4. Recognize model, reasoning, sandbox, approval, project, and integration settings.
5. Explain global and project configuration scope and precedence.
6. Inspect configuration safely and recognize common mistakes.

---

## 3. What Is Codex Configuration?

**Codex configuration** is a set of settings that guides how the local Codex client operates. It can establish reusable defaults so developers do not need to restate the same execution preferences for every session.

```text
Codex settings       Project instructions        Current prompt
model, permissions   AGENTS.md, conventions       objective, scope
       \                    |                         /
        +------------- Codex session ---------------+
```

Configuration can affect model/provider selection, reasoning effort, sandbox and approval behavior, and supported integrations. The effective behavior also depends on the installed client, project trust, account/workspace availability, and any enforced policies.

---

## 4. Why Configuration Matters

Configuration helps make sessions consistent, establishes reusable defaults, and lets developers choose execution restrictions that fit the task. For example, read-only settings may suit code exploration, while workspace write access may be needed for an approved implementation.

Use the least access that can complete the task. More permissive settings do not make an implementation better, and configuration does not replace review or project-specific safeguards.

---

## 5. TOML Basics

**TOML** (Tom’s Obvious, Minimal Language) is a human-readable configuration format made of keys, values, and tables.

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[example]
enabled = true
name = "development"
```

Basic TOML values include strings (`"value"`), booleans (`true`), numbers (`10`), arrays (`["one", "two"]`), and tables (`[section]`). The `[example]` table above demonstrates TOML syntax; it is not a Codex setting.

Valid TOML alone is not enough: Codex must also recognize the setting and allow it at that configuration level.

---

## 6. Configuration File Locations

Codex stores user-level defaults at:

```text
~/.codex/config.toml
```

On Windows, the default is usually:

```text
C:\Users\Acer\.codex\config.toml
```

The actual location can depend on the home directory and `CODEX_HOME`. Codex also supports project-level overrides in `.codex/config.toml` within a trusted repository. Project configuration is skipped for untrusted projects.

PowerShell discovery commands:

```powershell
Get-ChildItem "$HOME\.codex"
Test-Path "$HOME\.codex\config.toml"
Get-Content "$HOME\.codex\config.toml"
```

Run `Get-Content` only if the file exists. The Codex CLI and IDE extension share configuration layers. Never replace an existing config without first inspecting it.

---

## 7. Configuration vs Prompting

| Configuration | Prompt |
|---|---|
| Reusable operating defaults | Current task request |
| Model, sandbox, approvals | Requirements, scope, expected result |
| Read from supported config layers | Provided in the current interaction |

Example configuration:

```toml
sandbox_mode = "workspace-write"
approval_policy = "on-request"
```

Example prompt:

```text
Inspect the Android login screen. Improve spacing only, preserve
authentication logic, follow AGENTS.md, and report changed files.
```

**Configuration answers how Codex is set up to operate; prompting says what it should do now.**

---

## 8. Configuration vs AGENTS.md

`config.toml` and `AGENTS.md` have different roles.

| `config.toml` | `AGENTS.md` |
|---|---|
| Operating settings | Agent-facing project guidance |
| Model and provider defaults | Architecture and coding conventions |
| Sandbox and approvals | Scope, testing, and workflow instructions |
| Supported integrations | Repository context and constraints |

Example project instructions:

```markdown
## Development Rules
- Follow the existing architecture.
- Avoid unnecessary dependencies.
- Implement only approved requirements.
- Run relevant checks and report failures.
```

Use configuration for execution controls and `AGENTS.md` for development guidance.

---

## 9. Major Configuration Categories

| Category | Purpose |
|---|---|
| Model | Selects a model available to the signed-in user/workspace |
| Reasoning effort | Requests a supported reasoning level |
| Sandbox | Sets command execution and filesystem/network restrictions |
| Approval policy | Controls which operations pause for authorization |
| Project settings | Overrides supported values for a trusted repository or subfolder |
| Integrations | Configures supported services such as MCP servers |

These categories are explored in later lessons. The set of keys and accepted values can change; consult the current configuration reference for the installed Codex version.

---

## 10. Model Selection

The `model` setting selects a model available to the signed-in account or workspace:

```toml
model = "SUPPORTED_MODEL_ID"
```

`SUPPORTED_MODEL_ID` is a placeholder, not a literal setting to copy. Model availability and supported capabilities vary. Check the available model list and official documentation before choosing a value; examples in older tutorials may no longer apply.

---

## 11. Reasoning Effort

Codex supports a `model_reasoning_effort` setting for supported models:

```toml
model_reasoning_effort = "medium"
```

Accepted levels are model- and client-dependent. Current references may include values such as `low`, `medium`, `high`, `xhigh`, `max`, or `ultra`. Higher effort is not necessary for every task; choose based on complexity, availability, and the workflow’s needs.

---

## 12. Sandbox Modes

A sandbox limits the environment in which Codex runs commands. The current configuration reference lists:

```toml
sandbox_mode = "read-only"
```

```toml
sandbox_mode = "workspace-write"
```

Codex also documents `danger-full-access`, a highly permissive mode. It is not an appropriate routine learning default. Choose the most restrictive mode that can complete the approved task. Network and filesystem behavior can depend on additional settings and platform support.

---

## 13. Approval Policies

An approval policy controls when Codex pauses for approval before executing commands. For interactive work, a common value is:

```toml
approval_policy = "on-request"
```

Current documented policies also include `never` and granular approval settings. Do not use retired or deprecated values copied from old tutorials; check the current reference.

Sandbox and approval settings are separate: the sandbox defines execution boundaries, while the approval policy governs prompts for authorization. Neither one is a substitute for clear task scope and change review.

---

## 14. Global and Project Configuration

User-level config establishes defaults across the user’s Codex environment:

```text
~/.codex/config.toml
```

Trusted repositories can add project or subfolder overrides:

```text
my-project/
└── .codex/
    └── config.toml
```

Project configuration is useful only for supported settings and trusted projects. Codex intentionally skips project-scoped `.codex/` configuration for untrusted repositories. Do not assume every user-level setting is valid in project configuration.

---

## 15. Configuration Precedence

Codex resolves settings from multiple sources. The current documented order, from higher to lower precedence, is generally:

1. CLI flags and `--config` overrides.
2. Trusted project config, from repository root toward the current directory; the closest applicable file wins.
3. Selected profile file.
4. User config.
5. Cloud-managed defaults, if supplied.
6. System config, if present.
7. Built-in defaults.

Enforced organizational requirements can constrain or override user choices. If the effective setting differs from the file you inspected, check the other applicable layers and current policy. A CLI override can be session-specific; it does not necessarily change the stored defaults.

---

## 16. Minimal Configuration Example

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

This illustrates two supported settings. It is not a universal recommendation for every task or managed environment. Use read-only mode for investigation when suitable, and consult your organization’s requirements where applicable. Add only settings you understand and need.

---

## 17. Inspecting Configuration Safely

1. Confirm Codex is installed:

   ```powershell
   codex --version
   ```

2. Inspect the config directory and check for the file:

   ```powershell
   Get-ChildItem "$HOME\.codex"
   Test-Path "$HOME\.codex\config.toml"
   ```

3. Read the existing file before editing:

   ```powershell
   Get-Content "$HOME\.codex\config.toml"
   ```

4. If it exists, make a backup before changes:

   ```powershell
   Copy-Item "$HOME\.codex\config.toml" "$HOME\.codex\config.toml.bak"
   ```

5. Change one setting at a time, then start a new session and confirm Codex accepts it.

These commands are instructional; do not run `Copy-Item` if the source file is absent. Keep secrets out of config files and backups.

---

## 18. Common Configuration Mistakes

| Mistake | Better approach |
|---|---|
| Invalid TOML, such as an unquoted string | Follow TOML syntax and validate through Codex |
| Unsupported key or value | Check the current official reference and CLI version |
| Model unavailable to the account | Select from currently available models |
| Excessively permissive sandbox | Use least privilege for the task |
| Treating `AGENTS.md` rules as config | Keep execution settings and project instructions separate |
| Editing global settings for one project | Use a supported trusted project override when appropriate |
| Assuming every key works at every level | Check scope restrictions in the reference |
| Assuming one config file determines effective behavior | Check precedence, profiles, CLI overrides, and enforced policy |

---

## 19. Android Configuration Context

For an Android Java/XML project, configuration can provide appropriate execution restrictions and reusable model defaults. Project documentation must still define the approved MVP scope, architecture, and verification expectations.

A sensible workflow is: select a suitable sandbox; read project instructions; inspect the codebase; plan the approved task; implement within scope; run available Gradle checks; and review the diff. Configuration alone does not define which app features are authorized.

---

## 20. Laravel Configuration Context

For a Laravel backend, tasks may involve routes, controllers, tests, and migrations. Confirm the test environment and keep production secrets outside committed files. Avoid destructive database commands and unapproved schema changes. Use project instructions to define architecture and configuration to set operating controls.

---

## 21. Next.js Configuration Context

A Next.js task may read and edit TypeScript components, inspect packages, and run lint or build commands. Some build tools write caches or artifacts, so read-only permissions may prevent a check from completing. Choose permissions based on the approved task and report any verification that could not run.

---

## 22. Configuration and Security

Configuration affects what Codex can access and execute. Apply:

- **Least privilege:** grant only the access needed.
- **Secret protection:** do not store credentials in prompts or committed files.
- **Reviewability:** inspect commands and generated changes.
- **Environment separation:** avoid exposing production systems during development.
- **Explicit scope:** approval and sandbox controls complement task boundaries.

Convenience should not override project security requirements.

---

## 23. Practical Exercise A — Configuration Discovery

Use the commands below in PowerShell to locate configuration; do not expose secrets if you inspect the file.

```powershell
codex --version
Get-ChildItem "$HOME\.codex"
Test-Path "$HOME\.codex\config.toml"
```

Record whether Codex is installed, whether the directory and file exist, and whether any settings are familiar. Do not modify the file in this exercise.

---

## 24. Practical Exercise B — Classify Instructions

Classify each as configuration, project instructions, or task prompt:

1. Use a workspace-restricted sandbox.
2. Follow the existing MVC architecture.
3. Implement the approved login screen.
4. Pause for approval when policy requires it.
5. Avoid unnecessary libraries.
6. Investigate the current validation bug.

Explain why each belongs in that layer.

---

## 25. Practical Exercise C — Restricted Investigation

After checking the installed CLI’s help, start a read-only session if supported:

```powershell
codex --sandbox read-only
```

Ask Codex to identify the project structure, technologies, architecture, and relevant test commands. Instruct it not to modify files and to stop after reporting. Compare its findings with the repository.

---

## 26. Practical Exercise D — Review a Minimal Config

Consider:

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

Explain what each setting controls, why this is not suitable for every task, when read-only mode is preferable, and which project instructions remain necessary.

---

## 27. Knowledge Check

1. What is Codex configuration, and why is it useful?
2. What is TOML?
3. Where is user-level configuration commonly stored?
4. How does configuration differ from a prompt and `AGENTS.md`?
5. What do model selection and reasoning effort control?
6. What is a sandbox?
7. How does an approval policy differ from a sandbox?
8. What is the difference between user-level and project-level configuration?
9. Why does project trust matter?
10. What determines configuration precedence?
11. Why should changes be incremental?
12. Why should permissions be limited to what a task needs?
13. Why must supported keys and values be checked against the current reference?

---

## 28. Key Terminologies

| Term | Definition |
|---|---|
| Configuration | Settings that control application behavior |
| TOML | Human-readable configuration format |
| `config.toml` | Codex configuration file |
| Model | AI model selected for a session |
| Reasoning Effort | Requested reasoning level, subject to model support |
| Sandbox | Restrictions on command execution and access |
| Approval Policy | Rules for when Codex requests authorization |
| User Configuration | Defaults in the Codex home directory |
| Project Configuration | Supported overrides in a trusted project’s `.codex/` layer |
| Configuration Precedence | Rules for resolving settings from multiple sources |
| CLI Override | Session-level setting supplied at launch |
| Least Privilege | Limiting access to what is needed |
| `CODEX_HOME` | Environment variable that can change Codex’s home location |

---

## 29. Key Takeaways

1. Codex configuration sets operating defaults; prompts define the current task.
2. `AGENTS.md` contains project guidance, not execution settings.
3. User defaults commonly live in `~/.codex/config.toml`; trusted projects may supply supported overrides.
4. Model and reasoning values depend on client, model, and account availability.
5. Sandbox and approval policies are distinct controls.
6. Precedence and enforced policies affect the effective configuration.
7. Inspect existing settings, make incremental changes, and use least privilege.

---

## 30. Official References

1. [OpenAI — Codex Documentation](https://developers.openai.com/codex/)
2. [OpenAI — Config Basics](https://developers.openai.com/codex/config-basic/)
3. [OpenAI — Advanced Configuration](https://developers.openai.com/codex/config-advanced/)
4. [OpenAI — Configuration Reference](https://developers.openai.com/codex/config-reference/)
5. [OpenAI — Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
6. [OpenAI — Codex Security](https://developers.openai.com/codex/security/)
7. [OpenAI — AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
8. [TOML — Specification](https://toml.io/en/)

---

## 31. Lesson Completion Checklist

- [ ] Understand why Codex configuration matters.
- [ ] Understand TOML basics and locate the user config file.
- [ ] Distinguish config, prompts, and `AGENTS.md`.
- [ ] Understand model and reasoning settings.
- [ ] Understand sandbox and approval controls.
- [ ] Understand global and trusted project configuration.
- [ ] Explain configuration precedence.
- [ ] Recognize common mistakes and least-privilege practices.
- [ ] Complete the practical exercises and knowledge check.

---

## Next Lesson

**[[02 - Understanding config.toml]]**

The next lesson examines `config.toml` structure, supported keys and values, tables, examples, validation, configuration conflicts, and troubleshooting.

---

*Phase 4 — Codex Configuration | Lesson 4.1*  
*Learning Principle: Configure the environment deliberately, and keep project requirements separate from execution settings.*
