# 06 — Configuration Troubleshooting

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 4 — Codex Configuration
> **Lesson:** 4.6 — Configuration Troubleshooting
> **Difficulty:** Intermediate
> **Environment:** Windows 10/11, PowerShell, VS Code, Codex CLI
> **Prerequisite:** [[05 - Global vs Project-Specific Configuration]]
> **Next Phase:** Phase 5 — Advanced Codex
> **Status:** Theory Documented

---

## 1. Introduction

Earlier lessons covered Codex configuration, including config.toml, model and reasoning settings, sandbox and approval policies, global and project-specific configuration, precedence, and trusted repositories.

Developers must also know how to diagnose problems when configuration does not behave as expected. Common examples include startup failures after editing config.toml, unsupported settings, unavailable models, ignored project settings, profile errors, blocked writes or network commands, repeated approval requests, and incorrect Windows paths.

This lesson introduces a systematic troubleshooting workflow.

**Core Principle: Diagnose the configuration layer responsible for a problem before changing settings.**

---

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Follow a configuration troubleshooting process.
2. Distinguish TOML syntax errors from unsupported settings.
3. Diagnose model, reasoning, precedence, trust, and profile problems.
4. Understand sandbox, approval, and network-related failures.
5. Inspect configuration with PowerShell.
6. Validate TOML syntax and recover configuration safely.
7. Prepare a concise troubleshooting report.

---

## 3. What Is Configuration Troubleshooting?

**Configuration troubleshooting** identifies why settings are invalid, ignored, conflicting, or producing unexpected behavior.

~~~text
Codex Problem
     |
     +-- TOML syntax
     +-- Configuration keys and values
     +-- Model compatibility
     +-- Configuration precedence
     +-- Project trust and working directory
     +-- Sandbox, approvals, and network
     +-- Windows environment and installation
~~~

The goal is to identify the root cause and apply the smallest reliable correction, not merely hide an error.

---

## 4. The Troubleshooting Mindset

1. Observe the error before changing settings.
2. Reproduce it where possible.
3. Isolate one configuration layer or setting at a time.
4. Apply a minimal correction.
5. Verify the result and record the resolution.

~~~text
Observe → Collect evidence → Classify → Identify source
        → Test one hypothesis → Correct → Verify → Document
~~~

**Key Principle: Troubleshoot from evidence, not random configuration changes.**

---

## 5. Check the Installed Codex Version

~~~powershell
codex --version
~~~

Configuration support can change between releases. Record the CLI version, installation method, operating system, and terminal. Do not assume a setting documented for another release is supported by the installed client.

---

## 6. Check CLI Availability

~~~powershell
Get-Command codex -ErrorAction SilentlyContinue
where.exe codex
~~~

If PowerShell cannot find Codex, possible causes include a missing or failed installation, an executable directory missing from PATH, or a terminal that has not picked up updated environment variables. An installation or PATH problem is not necessarily a config.toml problem.

---

## 7. Inspect CLI Help

~~~powershell
codex --help
~~~

Use the installed CLI help to verify command-line options. A command copied from an older tutorial may not be supported by the installed version.

---

## 8. Locate the Effective Codex Home Directory

The default user configuration is usually ~/.codex/config.toml. On Windows this commonly resolves to C:\Users\<user>\.codex\config.toml. CODEX_HOME can change the location.

~~~powershell
$env:CODEX_HOME

$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}

$configPath = Join-Path $codexHome "config.toml"
$codexHome
$configPath
Test-Path $configPath
~~~

Always confirm which file Codex is expected to read before editing it.

---

## 9. Inspect Existing Configuration

~~~powershell
if (Test-Path $configPath) {
    Get-Content $configPath
} else {
    Write-Output "No user config.toml found."
}
~~~

Configuration files may contain sensitive integration details. Do not share their full contents without reviewing them; record only settings relevant to the problem.

---

## 10. What Is a TOML Syntax Error?

A TOML syntax error means the file does not follow TOML grammar. Common causes include missing quotes, invalid booleans, duplicate keys, unclosed strings, malformed arrays, and incorrect table declarations.

---

## 11. Error — Missing String Quotes

Incorrect:

~~~toml
sandbox_mode = workspace-write
~~~

Correct:

~~~toml
sandbox_mode = "workspace-write"
~~~

Text values require valid TOML string syntax.

---

## 12. Error — Incorrect Boolean

Incorrect:

~~~toml
[sandbox_workspace_write]
network_access = True
~~~

Correct:

~~~toml
[sandbox_workspace_write]
network_access = true
~~~

TOML booleans are lowercase true and false. The value "false" is a string, not a boolean.

---

## 13. Error — Duplicate Keys

Incorrect:

~~~toml
model_reasoning_effort = "medium"
model_reasoning_effort = "high"
~~~

TOML does not permit duplicate keys in the same table. Keep one definition. Precedence between separate configuration files is different from duplicate definitions inside one file.

---

## 14. Error — Incorrect Table Placement

~~~toml
[sandbox_workspace_write]
network_access = false
approval_policy = "on-request"
~~~

This is valid TOML syntax, but approval_policy is inside the wrong table. Correct placement:

~~~toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
~~~

Always check whether a key belongs at the top level or in a specific table.

---

## 15. Error — Unclosed String

Incorrect:

~~~toml
model = "example-model
~~~

Strings must be closed. A model value must also be a currently supported identifier; do not copy a placeholder as if it were real.

---

## 16. Error — Malformed Array

Incorrect:

~~~toml
example_values = ["one", "two"
~~~

Correct:

~~~toml
example_values = ["one", "two"]
~~~

This is a syntax example only; example_values is not asserted to be a Codex setting.

---

## 17. Syntax Validation vs Codex Validation

| Validation | Purpose |
|---|---|
| TOML syntax validation | Checks whether the file can be parsed |
| Codex configuration validation | Checks whether Codex accepts the keys, values, and scope |

For example, imaginary_codex_setting = true can be valid TOML and still not be a supported Codex setting.

**Valid TOML does not guarantee valid Codex configuration.**

---

## 18. Validate TOML with Python

Python 3.11 and later include tomllib.

~~~powershell
python --version
~~~

Save this as validate_toml.py in a practice directory:

~~~python
import sys
import tomllib
from pathlib import Path

if len(sys.argv) != 2:
    print("Usage: python validate_toml.py <file>")
    sys.exit(2)

path = Path(sys.argv[1])

try:
    with path.open("rb") as file:
        tomllib.load(file)
    print("Valid TOML syntax.")
except (OSError, tomllib.TOMLDecodeError) as error:
    print(f"Validation failed: {error}")
    sys.exit(1)
~~~

Run it with:

~~~powershell
python validate_toml.py "$configPath"
~~~

This checks syntax only. Use the current official Codex reference to verify supported settings.

---

## 19. Unsupported Configuration Keys

An unsupported key is not recognized by the installed Codex client.

~~~toml
make_codex_faster = true
~~~

Check the installed version and official configuration reference, confirm the key and its scope, then remove or correct it. A setting with no visible effect may also be overridden by a higher-priority source.

---

## 20. Unsupported Key vs Unsupported Value

An unsupported key is unrecognized, for example unknown_model_option. An unsupported value uses a recognized key with a value outside its accepted options, for example:

~~~toml
model_reasoning_effort = "super-ultra"
~~~

Check both the setting name and the permitted values.

---

## 21. Error — Model Not Available

An unavailable model may result from an incorrect identifier, account access, client compatibility, or a model that has been retired or replaced.

Troubleshooting:

1. Check the exact identifier.
2. Check the CLI version and current model availability.
3. Verify account access.
4. Try the effective default model.
5. Set an explicit model only when its availability is confirmed.

If no particular model is required, omitting model lets Codex use its effective default.

---

## 22. Error — Unsupported Reasoning Effort

The selected model may not support the configured reasoning level. Check the model and its supported settings, then use a compatible value. Compatibility can depend on the model and client version.

---

## 23. Model and Reasoning Incompatibility

If Codex rejects a model/effort combination, check model availability and reasoning compatibility together. Do not assume a value supported by one model works with every model.

---

## 24. Configuration Precedence Conflicts

A developer may expect a user-level value while a CLI override or trusted project setting supplies another. Identify the source of the effective value before editing configuration.

---

## 25. Configuration Precedence Order

The documented resolution order can include CLI flags and -c overrides, trusted project configuration, selected profiles, user configuration, managed defaults, system configuration, and built-in defaults. Exact behavior and enforcement can depend on current Codex version and managed policy.

Check higher-priority sources before assuming a user setting is ignored.

---

## 26. Conflict — CLI Override Takes Priority

User configuration:

~~~toml
model_reasoning_effort = "medium"
~~~

Invocation:

~~~powershell
codex -c 'model_reasoning_effort="high"'
~~~

The command-line override requests a different value for that invocation, subject to compatibility and enforced restrictions. Inspect the actual launch command.

---

## 27. Conflict — Wrong Working Directory

Check:

~~~powershell
Get-Location
~~~

If needed, change to the intended project directory before launching Codex:

~~~powershell
Set-Location "C:\xampp\htdocs\ecommerce\frontend"
~~~

The current working directory affects which project configuration files are applicable.

---

## 28. Error — Project Configuration Is Ignored

Possible causes include an untrusted project, the wrong working directory, a higher-priority override, a key that is not supported at project scope, or invalid TOML. Confirm each before changing settings.

---

## 29. Trusted vs Untrusted Projects

Codex loads project-scoped .codex configuration only for trusted projects. Do not trust an unfamiliar repository just to make its configuration load; review its contents and settings first. Trust is a security boundary.

---

## 30. Windows Project Trust Paths

An illustrative entry may look like:

~~~toml
[projects.'C:\Users\Acer\AndroidStudioProjects\GroupTrack']
trust_level = "trusted"
~~~

Use the real project path and the trust workflow supported by the installed client. Review the repository before granting trust.

---

## 31. Error — Profile Not Found

A missing profile can mean the profile name is misspelled, the file is missing, or CODEX_HOME points to another directory. Check the effective Codex home and profile location.

~~~powershell
Get-ChildItem $codexHome -Filter "*.config.toml"
~~~

Confirm the naming convention against current Codex documentation.

---

## 32. Error — Profile Setting Is Overridden

A trusted project setting may override a profile value. Inspect project configuration, the selected profile, and CLI overrides to determine the effective value. Selecting a profile does not guarantee every profile value becomes effective.

---

## 33. Error — Permission Denied

Possible causes include read-only mode, a target outside writable roots, a protected path, operating-system permissions, or managed restrictions.

Ask:

1. What path and operation were targeted?
2. Is the operation necessary for the approved task?
3. Which sandbox is active?
4. Is the path writable under sandbox and OS rules?
5. Is a narrower permission available?

Do not remove sandbox restrictions before identifying the blocked operation.

---

## 34. Error — Read-Only Command Cannot Create Files

Builds and tests may create artifacts, reports, caches, or temporary files. Read-only mode can block those writes even when source files are not changed. Use read-only mode for inspection; use appropriately authorized write access for approved build or test operations.

---

## 35. Error — Network Access Blocked

Dependency installation or remote access may fail due to sandbox policy, DNS, proxy, firewall, registry availability, or dependency configuration.

1. Confirm the command needs network access.
2. Identify its destination.
3. Check sandbox policy and normal system connectivity separately.
4. Use the supported approval workflow if additional access is needed.
5. Avoid broad permission changes when a narrower solution works.

---

## 36. Error — Repeated Approval Requests

Find the exact operation prompting approval. Determine whether it can run within current boundaries, whether the task can be scoped differently, or whether a narrower permission should be approved. Repeated prompts call for workflow analysis, not automatic removal of controls.

---

## 37. Error — Unexpectedly No Approval Prompt

Workspace-write mode can permit some operations without asking each time. on-request does not mean approval for every file edit. Use clear task scope, project instructions, suitable sandbox restrictions, and Git diff review. Do not treat approval prompts as a substitute for reviewing changes.

---

## 38. Problem — Codex Command Not Recognized

~~~powershell
Get-Command codex -ErrorAction SilentlyContinue
where.exe codex
~~~

Confirm installation and PATH using the official installation instructions. Avoid changing unrelated system environment variables.

---

## 39. Problem — Incorrect Home Directory

~~~powershell
$HOME
$env:CODEX_HOME
~~~

Resolve the effective Codex home before editing configuration. CODEX_HOME may redirect Codex from the default .codex directory.

---

## 40. Problem — Incorrect Path in TOML

Windows paths contain backslashes. Use valid TOML string syntax and confirm the resolved path. Single-quoted TOML strings can preserve backslashes literally in appropriate contexts.

---

## 41. Problem — Wrong Repository

~~~powershell
Get-Location
git rev-parse --show-toplevel
~~~

Confirm both the working directory and repository root before starting a coding task.

---

## 42. Problem — File Permissions

~~~powershell
Get-Item $configPath | Format-List FullName,Attributes,Length
~~~

A read-only attribute is only one possible cause. ACLs, file locks, antivirus, or organization restrictions can also block writes. Diagnose the specific restriction rather than broadly changing permissions.

---

## 43. Safe Configuration Recovery

Preserve the current file, identify the suspected change, test minimal settings separately, restore a known-good state if needed, then reintroduce settings incrementally. Recovery should not destroy valid changes.

---

## 44. Step 1 — Back Up the Configuration

~~~powershell
if (Test-Path $configPath) {
    $backupPath = "$configPath.bak"
    if (-not (Test-Path $backupPath)) {
        Copy-Item $configPath $backupPath
    }
}
~~~

This avoids overwriting an existing .bak. Keep backups private and outside public repositories.

---

## 45. Step 2 — Validate TOML Syntax

Run the validator from Section 18. Fix parse errors before investigating more advanced configuration behavior.

---

## 46. Step 3 — Inspect Recent Changes

Ask what changed shortly before the problem began: a setting, profile, project trust, CODEX_HOME, CLI version, or working directory. A recent change is a useful hypothesis, not proof of the cause.

---

## 47. Step 4 — Test a Minimal Configuration

~~~toml
approval_policy = "on-request"
sandbox_mode = "read-only"
~~~

Do not overwrite the active file. Test minimal settings in an isolated environment when appropriate.

---

## 48. Step 5 — Isolated Configuration Test

~~~powershell
$testHome = Join-Path $env:TEMP "codex-config-practice"
New-Item -ItemType Directory -Path $testHome -Force

@'
approval_policy = "on-request"
sandbox_mode = "read-only"
'@ | Set-Content (Join-Path $testHome "config.toml") -Encoding utf8
~~~

Changing CODEX_HOME can also change where Codex expects authentication and other state. This is not guaranteed to be a fully isolated execution environment; understand those effects before using it.

---

## 49. Step 6 — Restore the Previous Configuration

~~~powershell
$backupPath = "$configPath.bak"
if (Test-Path $backupPath) {
    Copy-Item $backupPath $configPath
}
~~~

This replaces the current file. Inspect both files first so legitimate updates are not lost.

---

## 50. Step 7 — Reintroduce Changes Incrementally

Add one setting, validate TOML, verify the key and scope, start Codex, and check behavior before adding the next setting. One change at a time narrows the cause of new errors.

---

## 51. Systematic Troubleshooting Matrix

| Symptom | Likely area | First check |
|---|---|---|
| Codex command missing | Installation or PATH | Get-Command codex |
| Configuration fails to parse | TOML syntax | Python tomllib |
| Unknown setting | Codex schema | Configuration reference |
| Model unavailable | Model selection | Identifier and account support |
| Reasoning value rejected | Compatibility | Model-supported effort |
| Global setting ignored | Precedence | CLI and project configuration |
| Project setting ignored | Trust or scope | Trust and working directory |
| Profile not found | Profile location | Codex home and profile filename |
| Permission denied | Sandbox or OS | Target path and active sandbox |
| Build fails in read-only mode | Filesystem writes | Build output requirements |
| Network command fails | Sandbox or connectivity | Policy and destination |
| Repeated approvals | Permission workflow | Requested operation |
| Unexpected edits | Task scope | Git diff and instructions |

Classify the symptom before selecting a diagnostic command.

---

## 52. Example — GroupTrack Android

If Gradle cannot build in a read-only session, Gradle may be trying to create build outputs or caches. This can be an expected sandbox restriction rather than an Android defect.

1. Use read-only mode to inspect code and identify the needed build command.
2. Confirm build execution is required.
3. Run it with appropriately restricted, authorized write access.
4. Review repository changes:

~~~powershell
git status --short
git diff --check
~~~

---

## 53. Example — Laravel Backend

A database failure during a test may come from test database configuration, an unavailable service, migrations, permissions, or application code.

1. Inspect the exact error.
2. Confirm the test environment and database target.
3. Verify connectivity safely.
4. Review relevant Laravel configuration.
5. Avoid destructive database operations.
6. Distinguish application failures from sandbox restrictions.

Not every failure during a Codex session is caused by Codex configuration.

---

## 54. Example — Next.js Frontend

An npm command can fail due to a wrong working directory, missing dependencies, network restrictions, registry connectivity, an unsupported Node.js version, or build output restrictions.

~~~powershell
node --version
npm --version
Get-Location
~~~

Identify whether the failure comes from the application, local environment, or Codex permissions before changing configuration.

---

## 55. Exercise A — Identify Error Categories

Classify each scenario and state its likely cause, first diagnostic step, and safe correction:

1. config.toml contains an unclosed string.
2. A model identifier is unavailable.
3. Project configuration is skipped because the repository is untrusted.
4. A build cannot create output files.
5. A CLI flag overrides a global setting.

---

## 56. Exercise B — TOML Validation

Given:

~~~toml
model_reasoning_effort = medium
approval_policy = "on-request"
sandbox_mode = "read-only"
~~~

Identify and correct the syntax error, validate with tomllib, and explain why syntax validation alone is insufficient.

---

## 57. Exercise C — Unsupported Setting

Given imaginary_setting = true, determine whether the TOML syntax is valid, whether Codex documents the key, how to verify support, and the safest correction.

---

## 58. Exercise D — Precedence Investigation

Assume all effort values are supported and no enforced restrictions apply:

~~~toml
# Global
model_reasoning_effort = "low"
~~~

~~~toml
# Profile
model_reasoning_effort = "medium"
~~~

~~~toml
# Trusted project
model_reasoning_effort = "high"
~~~

~~~powershell
codex -c 'model_reasoning_effort="low"'
~~~

Identify the effective setting, what changes when the CLI override is removed, and what changes if the project is untrusted.

---

## 59. Exercise E — Windows Diagnostics

~~~powershell
codex --version
Get-Command codex -ErrorAction SilentlyContinue
$HOME
$env:CODEX_HOME
Get-Location
~~~

Prepare an environment inventory with OS, PowerShell and Codex versions, command path, Codex home, config path, working directory, repository root, and observed problem.

---

## 60. Exercise F — Sandbox Diagnosis

For a permission failure, identify the target path, active sandbox, necessity of the operation, writable-root and OS permissions, and the narrowest correction.

---

## 61. Exercise G — Profile Troubleshooting

Verify the profile name and location, effective Codex home, TOML syntax, project configuration, CLI overrides, and resulting effective setting.

---

## 62. Exercise H — Configuration Recovery

Design a recovery process that preserves the original file, validates syntax, checks recent changes, tests minimal settings, restores a known-good version when needed, reintroduces changes incrementally, and documents the resolution.

---

## 63. Exercise I — Troubleshooting Report

~~~markdown
# Codex Configuration Troubleshooting Report

## 1. Problem Summary
Describe the observed issue.

## 2. Environment
- Operating System:
- Codex Version:
- Working Directory:
- Configuration Sources:

## 3. Error Evidence
Include the relevant non-sensitive error message.

## 4. Investigation
List diagnostic steps performed.

## 5. Root Cause
Explain the confirmed cause.

## 6. Resolution
Describe the smallest correction applied.

## 7. Verification
Record commands and observed results.

## 8. Prevention
Explain how to avoid recurrence.

## 9. Remaining Issues
Document unresolved items.
~~~

A good report separates observations, hypotheses, confirmed causes, and completed fixes.

---

## 64. Knowledge Check

1. What is configuration troubleshooting?
2. Why check the installed Codex version?
3. What is a TOML syntax error?
4. What causes duplicate-key errors?
5. Why does table placement matter?
6. How does TOML validation differ from Codex validation?
7. What are unsupported keys and values?
8. Why might a model be unavailable or an effort rejected?
9. What is configuration precedence?
10. Why can a global setting appear ignored?
11. What is project trust?
12. How can working directory affect configuration?
13. What causes profile selection problems?
14. Why can read-only mode block builds?
15. What can cause permission or network errors?
16. Why can approval requests repeat?
17. Does on-request require approval for every edit?
18. What is CODEX_HOME?
19. Why back up configuration?
20. Why reintroduce settings incrementally?
21. How do application errors differ from configuration errors?
22. What belongs in a troubleshooting report?

---

## 65. Key Terminologies

| Term | Definition |
|---|---|
| Troubleshooting | Diagnosing and resolving technical problems |
| Root Cause | Underlying reason a problem occurs |
| TOML Parser | Software that reads TOML syntax |
| Syntax Error | Violation of TOML grammar |
| Unsupported Key | Setting not accepted by Codex |
| Unsupported Value | Invalid value for a supported setting |
| Model Compatibility | Whether model and settings work together |
| Configuration Precedence | Priority among configuration sources |
| Effective Configuration | Settings actually applied |
| Project Trust | Authorization to load project-scoped configuration |
| Profile | Reusable named configuration |
| Sandbox | Restricted execution environment |
| Approval Policy | Rules for authorization requests |
| Permission Denied | Operation blocked by permissions |
| Writable Root | Directory permitted for modification |
| CODEX_HOME | Environment variable specifying Codex home |
| Recovery | Returning configuration to a working state |
| Verification | Confirming a fix through observed results |
| Troubleshooting Report | Documented investigation and resolution |

---

## 66. Key Takeaways

1. Troubleshooting begins with evidence collection.
2. Check the installed Codex version and effective configuration path.
3. TOML syntax errors differ from unsupported Codex settings.
4. Valid TOML does not guarantee valid Codex configuration.
5. Model identifiers and reasoning values must be supported and compatible.
6. Precedence, CLI overrides, trust, and working directory explain many unexpected settings.
7. Profiles must be correctly located and selected.
8. Sandbox restrictions can block valid commands that need to write or access the network.
9. Approval prompts should be investigated, not automatically bypassed.
10. Windows paths and environment variables need careful inspection.
11. Backups and incremental changes make recovery safer.
12. Verify and document a confirmed fix.

---

## 67. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Basic Configuration](https://developers.openai.com/codex/config-basic/)
3. [Advanced Configuration](https://developers.openai.com/codex/config-advanced/)
4. [Configuration Reference](https://developers.openai.com/codex/config-reference/)
5. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
6. [Codex Security](https://developers.openai.com/codex/security/)
7. [Codex Models](https://developers.openai.com/codex/models/)
8. [AGENTS.md Guidance](https://developers.openai.com/codex/guides/agents-md/)

### Supporting References

9. [TOML Official Specification](https://toml.io/en/)
10. [Python tomllib Documentation](https://docs.python.org/3/library/tomllib.html)
11. [PowerShell Documentation](https://learn.microsoft.com/powershell/)
12. [Git Documentation](https://git-scm.com/docs)

> **Documentation Note:** Codex configuration options, trust behavior, model availability, and platform-specific execution restrictions may change. Verify against the installed CLI version and current official documentation.

---

## 68. Lesson Completion Checklist

- [ ] Understand the troubleshooting process.
- [ ] Check the installed Codex version and CLI availability.
- [ ] Locate the effective Codex home and inspect configuration safely.
- [ ] Identify TOML syntax, duplicate-key, and table-placement errors.
- [ ] Validate TOML and distinguish syntax from application validation.
- [ ] Identify unsupported keys, values, models, and effort settings.
- [ ] Understand precedence, CLI overrides, project trust, and profiles.
- [ ] Diagnose sandbox, approval, network, and Windows path issues.
- [ ] Back up configuration and recover safely.
- [ ] Apply troubleshooting to Android, Laravel, and Next.js projects.
- [ ] Complete the practical exercises and knowledge check.
- [ ] Prepare a troubleshooting report.

---

# 69. Phase 4 Summary

## Phase 4 — Codex Configuration

- [x] [[01 - Introduction to Codex Configuration]]
- [x] [[02 - Understanding config.toml]]
- [x] [[03 - Model Selection and Reasoning Settings]]
- [x] [[04 - Sandbox and Approval Policies]]
- [x] [[05 - Global vs Project-Specific Configuration]]
- [x] [[06 - Configuration Troubleshooting]]

### Knowledge Acquired

The theory documentation covered Codex configuration architecture, TOML syntax, keys and supported values, model and reasoning settings, sandbox and approval controls, global and project configuration, trust and precedence, profiles, CLI overrides, diagnosis, and recovery.

**Phase 4 Theory Status: Completed**

Practical exercises remain available for future hands-on reinforcement.

---

# 70. Next Phase

## Phase 5 — Advanced Codex

### Learning Roadmap

- **5.1** Introduction to Model Context Protocol (MCP)
- **5.2** MCP Servers and External Tool Integrations
- **5.3** Agent Skills and Reusable Workflows
- **5.4** Multi-Agent Collaboration and Task Delegation
- **5.5** Codex Automation and Non-Interactive Execution
- **5.6** Context and Token Efficiency
- **5.7** Advanced Codebase Management

### Next Lesson

**[[01 - Introduction to Model Context Protocol (MCP)]]**

---

*Phase 4 — Codex Configuration | Lesson 4.6*
*Learning Principle: Observe, isolate, correct, verify, and document—never troubleshoot by blindly removing safeguards.*
