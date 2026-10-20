# 02 — Understanding config.toml

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 4 — Codex Configuration
> **Lesson:** 4.2 — Understanding config.toml
> **Difficulty:** Intermediate
> **Environment:** Windows 10/11, PowerShell, VS Code, Codex CLI
> **Prerequisite:** [[01 - Introduction to Codex Configuration]]
> **Next Lesson:** [[03 - Model Selection and Reasoning Settings]]
> **Status:** In Progress

---

## 1. Introduction

Codex configuration determines important aspects of how the coding agent operates. This lesson examines the Codex CLI configuration file, config.toml, which stores persistent preferences instead of requiring the same command-line options every time.

Supported settings can include model selection, reasoning effort, sandbox restrictions, approval policies, project behavior, MCP integrations, and other Codex features. An invalid setting can prevent Codex from starting, while an overly permissive setting can expose the development environment to unnecessary risks.

**Core Principle: Understand each configuration key, accepted values, and scope before adding it to config.toml.**

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Explain the purpose of config.toml.
2. Locate Codex configuration files on Windows.
3. Understand TOML syntax and common data types.
4. Identify commonly used Codex configuration keys and accepted values.
5. Write basic and nested TOML configurations.
6. Differentiate global and project-level configuration.
7. Understand configuration precedence, CLI overrides, and profiles.
8. Inspect and edit configuration using PowerShell.
9. Validate TOML syntax and troubleshoot common mistakes.
10. Apply safe configuration practices to real projects.

## 3. What Is config.toml?

config.toml is a TOML configuration file read by Codex. It stores supported settings that influence how the Codex client operates.

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
~~~

These settings request an interactive approval policy and workspace-restricted sandbox. They do not describe the programming task.

~~~text
Codex CLI
  ├── config.toml: model, reasoning, sandbox, approvals, integrations
  ├── AGENTS.md: project rules, architecture, constraints
  └── Task prompt: current objective, requirements, acceptance criteria
~~~

Configuration, project instructions, and task prompts have different responsibilities.

## 4. Default Configuration Location

The standard user-level file is ~/.codex/config.toml. On Windows, tilde normally represents the user's home directory, for example C:\Users\Acer\.codex\config.toml.

Codex can use the CODEX_HOME environment variable to locate its home directory, so the effective location may differ. Other Codex-managed files may exist there; do not delete unfamiliar files just because they are not part of this lesson.

## 5. Inspecting the Configuration Location

~~~powershell
codex --version
$HOME
$env:CODEX_HOME
~~~

Determine the expected path:

~~~powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}
$configPath = Join-Path $codexHome "config.toml"
$configPath
Test-Path $configPath
~~~

False means the file does not exist at that location; Codex can still operate using defaults.

## 6. What Is TOML?

TOML means Tom's Obvious, Minimal Language. It is a readable configuration format that commonly uses key = value.

~~~toml
project_name = "GroupTrack"
~~~

This demonstrates TOML syntax; project_name is not asserted to be a supported Codex key. A file may be valid TOML but contain keys Codex does not recognize.

## 7. TOML Data Types

| Type | Example | Meaning |
|---|---|---|
| String | "hello" | Text |
| Integer | 10 | Whole number |
| Float | 0.75 | Decimal |
| Boolean | true | True or false |
| Array | ["a", "b"] | Ordered collection |
| Table | [section] | Group of related settings |
| Inline table | { x = 1 } | Compact grouped values |
| Date/time | 2026-10-20 | Date representation |

TOML supports these types, but Codex accepts only keys and value types defined by its configuration schema.

## 8. Strings, Booleans, Numbers, and Arrays

Strings use quotes:

~~~toml
sandbox_mode = "workspace-write"
~~~

Booleans are lowercase and unquoted:

~~~toml
enabled = true
~~~

"true" is a string, not a boolean; True is not valid TOML boolean syntax.

~~~toml
retry_count = 3
threshold = 0.75
languages = ["Java", "PHP", "TypeScript"]
~~~

These are syntax examples, not Codex keys unless documented.

## 9. Comments, Tables, and Nested Tables

Comments begin with # and are ignored by the parser:

~~~toml
# Interactive approval behavior
approval_policy = "on-request"
~~~

A table groups related keys:

~~~toml
[development]
language = "Java"
~~~

This is TOML syntax, not a supported Codex section. A practical nested Codex table is:

~~~toml
[sandbox_workspace_write]
network_access = false
~~~

Here network_access belongs to the sandbox table.

## 10. Dotted Keys and Table Scope

TOML supports dotted keys such as application.database.port = 5432. Mixing dotted keys and table declarations carelessly can cause duplicate-definition errors. Explicit table headers are easier to inspect.

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
~~~

Here approval_policy is top-level and network_access belongs to the nested table. If approval_policy appears after the table header, it belongs to that table instead.

**Always verify which table a key belongs to.**

## 11. Common Codex Configuration Keys

| Key | Purpose | Example |
|---|---|---|
| model | Default model identifier | Supported model ID |
| model_reasoning_effort | Reasoning effort | "medium" |
| approval_policy | Approval behavior | "on-request" |
| sandbox_mode | Execution sandbox | "workspace-write" |
| sandbox_workspace_write.network_access | Workspace network access | false |
| mcp_servers | MCP server configurations | TOML table |
| profiles | Named configuration profiles | TOML table |

This is a selected subset, not the complete schema. Keys, defaults, and accepted values can change between Codex versions.

## 12. Model and Reasoning Configuration

The model key selects a default model identifier when explicitly configured:

~~~toml
model = "SUPPORTED_MODEL_ID"
~~~

Replace the placeholder only with a model supported by the client and account. Models can differ in capability, reasoning options, performance, availability, and usage limits. If the default works, there is no need to set it manually.

The model_reasoning_effort key requests a supported reasoning level:

~~~toml
model_reasoning_effort = "medium"
~~~

Common levels include low, medium, and high; other values may be model- or version-specific. Confirm support for the selected model.

## 13. Sandbox, Approval, and Network Settings

Read-only sandbox:

~~~toml
sandbox_mode = "read-only"
~~~

Useful for exploration, review, and planning.

Workspace-write sandbox:

~~~toml
sandbox_mode = "workspace-write"
~~~

Useful for authorized edits within permitted workspace boundaries. Codex also documents danger-full-access, which removes major restrictions and is not a routine development default.

Approval policy:

~~~toml
approval_policy = "on-request"
~~~

The value never is also documented but disables interactive approval prompts; it does not mean safe read-only operation. Approval policy and sandbox mode are separate controls.

Workspace network setting:

~~~toml
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
~~~

Some tasks, such as installing dependencies, may need network connectivity. Enable only what the approved workflow requires.

## 14. Minimal Configuration Examples

Start small:

~~~toml
# Codex user configuration
approval_policy = "on-request"
sandbox_mode = "workspace-write"
~~~

Add reasoning effort only when there is a reason:

~~~toml
model_reasoning_effort = "medium"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
~~~

Only configure settings you have a clear reason to override.

## 15. Global Configuration

User-level configuration applies reusable settings across Codex projects, commonly at C:\Users\Acer\.codex\config.toml.

~~~toml
model_reasoning_effort = "medium"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
~~~

Global settings provide consistent defaults but may not suit every project or task.

## 16. Project-Level Configuration

A project may contain .codex/config.toml:

~~~text
GroupTrack/
├── .codex/config.toml
├── AGENTS.md
├── docs/
└── app/
~~~

Example project override:

~~~toml
model_reasoning_effort = "medium"
~~~

Project-level configuration is loaded only for trusted projects, and some keys cannot be overridden there. A file existing on disk does not guarantee Codex uses it.

## 17. Configuration Responsibilities in Projects

| File | Responsibility |
|---|---|
| Global config.toml | General Codex operating defaults |
| Project config.toml | Supported project-specific overrides |
| AGENTS.md | Architecture, coding standards, scope |
| Task prompt | Current requested change |

For Android, instructions might specify Java/XML conventions, MVP scope, dependency limits, and Gradle checks. For Laravel, they might specify architecture, validation, database safety, and tests. For Next.js, they might specify TypeScript, App Router, component reuse, and checks.

Do not use config.toml to replace architectural documentation.

## 18. Configuration Precedence

Configuration precedence determines which value takes effect when sources overlap. Sources can include CLI overrides, trusted project settings, profiles, user settings, managed/system layers, and enforced requirements.

~~~text
CLI Override -> Trusted Project Configuration -> Profile
-> User Configuration -> Other Defaults
~~~

This is simplified; consult current documentation for exact precedence.

## 19. CLI Overrides

A command-line option can override a setting for one invocation.

~~~powershell
codex --sandbox read-only
codex -c model_reasoning_effort="high"
~~~

Overrides are useful for temporary reviews or task-specific needs without changing everyday defaults.

## 20. Configuration Profiles

Profiles provide reusable named variants, such as routine development, read-only review, or complex debugging.

~~~toml
[profiles.review]
sandbox_mode = "read-only"
model_reasoning_effort = "medium"
~~~

Select a profile with:

~~~powershell
codex --profile review
~~~

Profiles follow current precedence rules and cannot bypass enforced restrictions.

## 21. Choosing a Configuration Layer

| Situation | Appropriate location |
|---|---|
| General personal preference | Global configuration |
| Supported repository-specific setting | Trusted project configuration |
| Temporary read-only inspection | CLI override |
| Reusable alternative workflow | Profile |
| Coding standards | AGENTS.md |
| Current feature requirement | Task prompt |

Use a CLI override for one-off inspection rather than changing a global default.

## 22. Inspecting Configuration with PowerShell

~~~powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}
$configPath = Join-Path $codexHome "config.toml"

Test-Path $configPath

if (Test-Path $configPath) {
    Get-Content $configPath
} else {
    Write-Output "No user config.toml found."
}

if (Test-Path $configPath) {
    code $configPath
}
~~~

Configuration may contain sensitive integration details. Review it before sharing or publishing.

## 23. Backing Up and Creating Configuration

Back up before editing:

~~~powershell
if (Test-Path $configPath) {
    Copy-Item $configPath "$configPath.bak"
}
~~~

Keep backups private and out of public repositories.

If you intentionally want to create a missing file without overwriting:

~~~powershell
New-Item -ItemType Directory -Path $codexHome -Force

if (-not (Test-Path $configPath)) {
    @'
approval_policy = "on-request"
sandbox_mode = "workspace-write"
'@ | Set-Content -Path $configPath -Encoding UTF8
}
~~~

The existence check avoids replacing an uninspected configuration.

## 24. Configuration Validation

There are two kinds of validation:

1. **Syntax validation:** Does the file follow TOML grammar?
2. **Application validation:** Does Codex accept the keys, types, and values?

unknown_setting = "hello" can be valid TOML but not a supported Codex key. Valid TOML does not automatically mean a valid Codex configuration.

## 25. Validating TOML with Python

Python 3.11 and later include tomllib.

~~~python
import sys
import tomllib
from pathlib import Path

path = Path(sys.argv[1])

try:
    with path.open("rb") as file:
        tomllib.load(file)
    print("Valid TOML syntax.")
except (tomllib.TOMLDecodeError, OSError) as error:
    print(f"Validation failed: {error}")
    sys.exit(1)
~~~

Run it with:

~~~powershell
python validate_toml.py "$configPath"
~~~

This checks TOML syntax only, not whether Codex recognizes the keys.

## 26. Validating Configuration with Codex

~~~powershell
codex --version
codex --help
codex --sandbox read-only
~~~

Check the current official configuration reference for accepted keys and values. Observe startup errors and verify effective behavior. Do not assume an older tutorial matches the installed version.

## 27. Safe Configuration Testing Workflow

~~~text
Inspect Existing Configuration -> Create Backup -> Change One Setting
-> Validate TOML -> Check Official Key Reference -> Start Codex
-> Verify Effective Behavior -> Keep or Revert
~~~

Changing one setting at a time makes errors easier to isolate. Treat configuration changes like small software changes: edit, validate, verify, and review.

## 28. Common Mistakes and Troubleshooting

### Missing Quotes

Incorrect: sandbox_mode = workspace-write
Correct: sandbox_mode = "workspace-write"

### Incorrect Boolean Type

Incorrect: network_access = "false"
Correct: network_access = false

### Duplicate Keys

TOML does not permit duplicate keys in the same table. Define each key once.

### Incorrect Table Placement

~~~toml
approval_policy = "on-request"

[sandbox_workspace_write]
network_access = false
~~~

Top-level settings belong before the table header.

### Unsupported Keys or Values

Do not invent settings or assume a model or reasoning value is supported. Check current documentation.

### Wrong Configuration Layer

Changing the user config affects defaults across projects. Use a CLI override for one-session behavior or trusted project config for supported project settings.

### Project Config Not Loaded

Project config is subject to project trust. Confirm trust and effective settings.

### Excessive Configuration

Start small. Add profiles, providers, MCP servers, or advanced settings only when needed and understood.

## 29. config.toml vs. AGENTS.md

config.toml controls supported Codex operating settings. AGENTS.md communicates project-specific development instructions. The task prompt describes the current change.

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
~~~

~~~markdown
## Project Scope
- Implement only approved MVP features.
- Use existing Java and XML conventions.
- Avoid unnecessary libraries.
- Preserve existing architecture.
- Run relevant Gradle verification.
~~~

~~~text
Inspect the existing sign-in screen.
Improve spacing and alignment only.
Do not modify authentication logic.
Follow AGENTS.md and report changed files.
~~~

Use the correct layer for each instruction.

## 30. Configuration and Git

Global config is outside the repository. Project config may be at .codex/config.toml. Before committing project settings, confirm they are suitable for collaborators, contain no secrets, use supported keys, are intentionally shared, and have a reviewed diff.

~~~powershell
git status --short
git diff -- .codex/config.toml
~~~

## 31. Practical Exercise A — Configuration Discovery

1. Check the Codex version.
2. Identify the home directory.
3. Check CODEX_HOME.
4. Locate config.toml.
5. Inspect existing settings.

~~~powershell
codex --version
$HOME
$env:CODEX_HOME
~~~

Deliverable: a short configuration inventory.

## 32. Practical Exercise B — TOML Syntax Identification

~~~toml
name = "Codex"
enabled = true
count = 5
ratio = 0.75
languages = ["Java", "PHP"]
~~~

Identify the string, boolean, integer, float, and array values.

## 33. Practical Exercise C — Identify Invalid TOML

~~~toml
sandbox_mode = workspace-write
approval_policy = "on-request"
sandbox_mode = "read-only"
~~~

Identify the invalid string and duplicate key, rewrite the file, and explain the sandbox choice.

## 34. Practical Exercise D — Create a Minimal Configuration

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
model_reasoning_effort = "medium"
~~~

Create this in a practice file first if you do not intend to change active Codex settings.

## 35. Practical Exercise E — Nested Table Practice

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
~~~

Identify top-level keys, the nested key, the effect of false network access, and why approval appears before the table.

## 36. Practical Exercise F — Global vs. Project Configuration

Classify each as global config, project config, CLI override, or AGENTS.md:

- Same approval behavior across projects.
- Different reasoning preference for one trusted project.
- Temporary read-only review.
- Project rule against unnecessary dependencies.

## 37. Practical Exercise G — Configuration Validation

Create practice-config.toml, add valid settings, parse it with Python tomllib, introduce a duplicate key, validate again, correct the error, and confirm parsing succeeds.

## 38. Practical Exercise H — Configuration Review with Codex

~~~text
Read the current Codex configuration and applicable project instructions.

Perform a read-only audit. Identify existing settings, unsupported or suspicious keys, conflicts, sandbox and approval behavior, project overrides, and security considerations.

Do not modify files. Do not reveal credentials or tokens. Report evidence and recommendations, then stop.
~~~

## 39. Knowledge Check

1. What is config.toml?
2. Why does Codex use TOML?
3. Where is the default user configuration stored?
4. What is CODEX_HOME?
5. What is a TOML key-value pair?
6. What is a string?
7. What is a boolean?
8. What is an array?
9. What is a TOML table?
10. What is a nested table?
11. Why does table scope matter?
12. What does model configure?
13. What does model_reasoning_effort configure?
14. What does sandbox_mode configure?
15. What does approval_policy configure?
16. What does network_access control in workspace-write?
17. What is global configuration?
18. What is project configuration?
19. What is configuration precedence?
20. What is a CLI override?
21. What is a configuration profile?
22. Why should configuration be backed up?
23. What does TOML syntax validation check?
24. Why is syntax validation insufficient by itself?
25. What happens when a key is duplicated?
26. Why should unsupported keys be avoided?
27. How does config.toml differ from AGENTS.md?
28. Why review project config before committing?

## 40. Key Terminologies

| Term | Definition |
|---|---|
| config.toml | Codex configuration file |
| TOML | Human-readable configuration format |
| Key | Named configuration property |
| Value | Data assigned to a key |
| String | Text value |
| Boolean | True/false value |
| Integer | Whole-number value |
| Array | Ordered collection |
| Table | Group of configuration keys |
| Nested table | Table within a hierarchy |
| Global configuration | User-level settings |
| Project configuration | Repository-specific settings |
| Configuration precedence | Order for resolving overlapping settings |
| CLI override | Command-line setting for an invocation |
| Profile | Named configuration variant |
| Sandbox | Restricted execution environment |
| Approval policy | Rules for requesting authorization |
| Syntax validation | Checking TOML grammar |
| Application validation | Checking settings against Codex support |
| CODEX_HOME | Codex home-directory environment setting |

## 41. Key Takeaways

1. **config.toml controls important Codex operating settings.**
2. **Codex uses TOML, a human-readable configuration format.**
3. **The standard user configuration is ~/.codex/config.toml.**
4. **CODEX_HOME can affect the configuration location.**
5. **TOML supports strings, booleans, numbers, arrays, and tables.**
6. **Correct data types and table placement matter.**
7. **Use only documented keys and supported values.**
8. **Model and reasoning support depend on the current client and model.**
9. **Sandbox and approval settings are separate controls.**
10. **Global settings provide defaults; trusted project config supports certain overrides.**
11. **CLI overrides are useful for temporary session settings.**
12. **Profiles provide reusable configuration variants.**
13. **Valid TOML does not guarantee a valid Codex configuration.**
14. **Back up and test configuration changes incrementally.**
15. **Configuration differs from AGENTS.md and task prompts.**
16. **Keep configuration as simple as the workflow allows.**

## 42. Official References

### OpenAI Codex

1. Codex Documentation — https://developers.openai.com/codex/
2. Basic Configuration — https://developers.openai.com/codex/config-basic/
3. Advanced Configuration — https://developers.openai.com/codex/config-advanced/
4. Configuration Reference — https://developers.openai.com/codex/config-reference/
5. Codex CLI Reference — https://developers.openai.com/codex/cli/reference/
6. Codex Security — https://developers.openai.com/codex/security/
7. AGENTS.md Guidance — https://developers.openai.com/codex/guides/agents-md/

### TOML and Python

8. TOML Official Specification — https://toml.io/en/
9. Python tomllib Documentation — https://docs.python.org/3/library/tomllib.html

### Git

10. Git Documentation — https://git-scm.com/docs

## 43. Lesson Completion Checklist

- [ ] Understand the purpose and location of config.toml.
- [ ] Understand CODEX_HOME.
- [ ] Understand TOML types, tables, nested tables, and scope.
- [ ] Identify common Codex keys and supported values.
- [ ] Understand model, reasoning, sandbox, approval, and network settings.
- [ ] Distinguish global and trusted project configuration.
- [ ] Understand precedence, CLI overrides, and profiles.
- [ ] Inspect and back up configuration with PowerShell.
- [ ] Validate TOML and application-level settings.
- [ ] Identify common mistakes.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge check.

---

## Next Lesson

**[[03 - Model Selection and Reasoning Settings]]**

Next, explore model selection, reasoning levels, performance tradeoffs, task-based selection, overrides, common mistakes, and practical workflows.

---

*Phase 4 — Codex Configuration | Lesson 4.2*
*Learning Principle: Configure only what you understand, verify what you change, and keep execution controls separate from project instructions.*
