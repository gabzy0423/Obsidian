# 04 — Sandbox and Approval Policies

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 4 — Codex Configuration  
> **Lesson:** 4.4 — Sandbox and Approval Policies  
> **Difficulty:** Intermediate  
> **Environment:** Windows, PowerShell, VS Code, Codex CLI  
> **Prerequisite:** [[03 - Model Selection and Reasoning Settings]]  
> **Next Lesson:** [[05 - Global vs Project-Specific Configuration]]  
> **Status:** Theory Documented

---

## 1. Introduction

Codex can inspect repositories, modify files, and execute development commands. These capabilities are useful, but a command can affect unintended files, access the network, or use a database or other resource in ways the task did not require.

Codex uses two related controls:

1. **Sandbox policies** define the filesystem and network boundaries for command execution.
2. **Approval policies** determine when Codex pauses to request authorization.

These controls are separate. A prompt states the intended task, the sandbox limits available command capabilities, and the approval policy governs applicable authorization prompts.

**Core principle:** Give Codex only the access needed for the approved task, and review operations with meaningful consequences.

---

## 2. Learning Objectives

By the end of this lesson, learners should be able to:

1. Explain sandboxing and the principle of least privilege.
2. Identify Codex sandbox modes and their general uses.
3. Distinguish sandbox restrictions from approval prompts.
4. Understand permission escalation and network controls.
5. Configure sandbox and approval settings in `config.toml`.
6. Use temporary CLI options and permission controls where supported.
7. Apply the concepts to Android, Laravel, and Next.js tasks.
8. Perform a basic permission and change audit.

---

# Part I — Security Fundamentals

## 3. What Is a Sandbox?

A **sandbox** is an execution environment that limits the resources available to a process. In Codex, sandbox settings govern command execution capabilities such as filesystem access and, where supported, network access.

```text
Task prompt → Codex tool request → sandbox policy
                                  ├─ filesystem boundary
                                  ├─ network policy
                                  └─ permitted command execution
```

The prompt “Do not modify files” communicates intent. A read-only sandbox provides an additional technical restriction for command execution. Neither replaces review of outputs or other tools that may have different controls.

**Key principle:** Instructions guide behavior; sandboxing enforces execution boundaries.

---

## 4. Why Sandboxing Matters

Unlike a code suggestion that a developer manually copies, an agentic workflow can run commands in a development environment. Consider:

- Which files and directories can commands read or modify?
- Can commands reach the network?
- Could a task touch another repository or a production service?
- Can a command create caches or other side effects?
- What happens if an operation fails partway through?

A technically correct code change does not justify unnecessary or unauthorized access. Select execution permissions according to the actual task.

---

## 5. Principle of Least Privilege

The **principle of least privilege** means granting only the capabilities required to complete the task.

| Task | Likely starting point |
|---|---|
| Analyze authentication architecture | Read-only |
| Inspect a Git diff | Read-only |
| Plan a UI change | Read-only |
| Implement an approved UI change | Workspace-write |
| Run a build that writes local artifacts | Workspace-write, if needed |

Start with the least access that can do the job. Increase access only when a specific necessary operation cannot proceed and its impact is understood.

---

# Part II — Codex Sandbox Modes

## 6. Available Sandbox Modes

The current Codex configuration reference lists three `sandbox_mode` values:

| Mode | General purpose | Relative exposure |
|---|---|---|
| `read-only` | Inspection and analysis | Lower |
| `workspace-write` | Development within configured writable boundaries | Moderate |
| `danger-full-access` | Command execution without the normal sandbox boundary | Highest |

Exact behavior can depend on client version, operating system, additional settings, and organizational policy. `danger-full-access` is the name of the sandbox mode; “full access” in the interface may involve additional approval settings as well.

---

## 7. Read-Only Sandbox

Configuration:

```toml
sandbox_mode = "read-only"
```

CLI example:

```powershell
codex --sandbox read-only
```

Read-only mode is useful for codebase exploration, architecture analysis, dependency review, planning, and diagnosis before making changes.

```text
Read AGENTS.md and relevant documentation. Inspect the login workflow.
Identify relevant files, current behavior, constraints, and possible issues.
Do not modify files. Stop after reporting findings.
```

Some commands need to write temporary files, caches, or build artifacts and may not work under read-only restrictions. Check a command and the applicable environment before running it. Read-only sandboxing governs command execution; other tools and product features can have separate controls.

---

## 8. Workspace-Write Sandbox

Configuration:

```toml
sandbox_mode = "workspace-write"
```

CLI example:

```powershell
codex --sandbox workspace-write
```

Workspace-write is intended for authorized development such as implementing a feature, editing source or documentation, adding focused tests, and running local checks. Writes are constrained by configured writable boundaries and applicable protected-path rules.

```text
Implement only the approved login-screen alignment changes.
Preserve authentication logic, add no dependencies, and avoid unrelated files.
Run relevant checks and report changed files and actual results.
Do not commit or push.
```

Workspace-write does not mean that the prompt’s named files are the only files Codex could technically modify. The sandbox defines an access boundary; the task and project instructions define intended scope. Review the resulting changes.

---

## 9. Unrestricted Sandbox Access

`danger-full-access` removes the normal sandbox boundary for command execution. This can expose files, network resources, and system capabilities beyond the intended project. It is not a routine default for local development.

The overall “full access” configuration can pair `sandbox_mode = "danger-full-access"` with `approval_policy = "never"`, removing both sandbox restrictions and interactive approval pauses. Use such a configuration only when the workflow specifically requires it and the environment is intentionally controlled.

If an operation fails in a restricted mode, identify the exact missing capability first. Prefer a narrow, task-specific adjustment over broadly removing protections.

---

## 10. Comparing the Modes

| Capability | Read-only | Workspace-write |
|---|---|---|
| Inspect files within permitted access | Yes | Yes |
| Modify permitted workspace files | Restricted | Allowed within writable boundaries |
| Run commands | Subject to sandbox and approvals | Subject to sandbox and approvals |
| Add extra writable roots | Not the purpose of this mode | Supported by configuration |
| Network access for commands | Restricted by applicable policy | Configurable; see network setting |
| Code review and exploration | Usually suitable | Often more access than needed |
| Approved implementation | Usually unsuitable for writes | Usually suitable |

Choose the smallest permission set that supports the task.

---

# Part III — Approval Policies

## 11. What Is an Approval Policy?

An **approval policy** determines when Codex pauses for authorization before command execution or other supported operations.

```toml
approval_policy = "on-request"
```

Conceptually, Codex evaluates the requested operation against sandbox and policy controls. If the operation needs access beyond the current boundary and an approval path is available, it may pause for authorization. Exact prompts depend on the client and effective policy.

---

## 12. `on-request`

`on-request` is the interactive approval policy commonly used when a user should be able to review requests that need authorization.

```toml
approval_policy = "on-request"
```

For example, a command might need access outside the current workspace boundary or network access that is not currently allowed. Review the proposed operation, its reason, and its target before approving.

`on-request` does not guarantee a separate prompt for every file edit. Operations already allowed inside the active sandbox can proceed without another approval prompt. Review the diff after changes.

---

## 13. `never`

```toml
approval_policy = "never"
```

With `never`, Codex does not pause for interactive approval requests. This does not grant extra capabilities: the sandbox and other policies still apply. A blocked operation may fail instead of prompting.

This policy can suit deliberately designed non-interactive workflows, but it removes an interactive checkpoint. It is not appropriate when the workflow depends on reviewing each escalation request.

The current reference documents `on-request`, `never`, and granular approval settings. It says the old `untrusted` approval value is unsupported and `on-failure` is deprecated. Do not copy those old values from outdated tutorials.

---

## 14. Sandbox and Approval Are Different

| Sandbox | Approval policy |
|---|---|
| Sets execution boundaries | Controls authorization pauses |
| Governs filesystem/network access for commands | Governs whether applicable requests pause |
| Configured with `sandbox_mode` and related fields | Configured with `approval_policy` |
| Does not define the feature scope | Does not itself grant unrestricted access |

Example:

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

This combination supports development within the writable workspace and interactive approval requests when applicable. It does not automatically make each write require approval.

---

# Part IV — Permission Escalation and Boundaries

## 15. Permission Escalation

Permission escalation is a request to perform an operation outside the current permitted boundary. Before approving, consider:

1. What exact command or action will run?
2. Why is it needed for the approved task?
3. Which files, services, or hosts are affected?
4. Can the task proceed with narrower access?
5. Is the operation reversible?
6. Is this the correct development or test environment?

Approve a specific necessary action, not broad access without a clear reason.

---

## 16. When to Deny or Reconsider a Request

Deny or investigate further if an operation is unrelated to the task, requests broader access than necessary, has an unclear target, could expose credentials, affects production unexpectedly, or has a safer alternative.

Example: an Android spacing task does not justify changing an unrelated server configuration. Permission requests should be evaluated against the task’s original scope.

---

## 17. Filesystem Boundaries and Writable Roots

Workspace-write permits writes within the configured writable roots. Codex also applies additional controls to certain protected paths; being inside a workspace does not guarantee that every path is writable.

Codex supports additional writable roots under the workspace-write settings:

```toml
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
writable_roots = []
```

An empty array adds no extra roots. Adding paths expands where command execution may write; ordinary single-repository tasks usually do not need extra roots. Check the current reference for protected paths and platform-specific behavior.

---

## 18. Network Access

Build and development commands may need network access to retrieve dependencies or contact services. Network operations can also download code or transmit data, so enable only what the task requires.

The current reference documents `sandbox_workspace_write.network_access` as controlling outbound access inside the workspace-write sandbox:

```toml
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
```

With this setting disabled, commands inside that sandbox cannot use outbound network access as configured. Package downloads, remote Git operations, and external service calls may not work. This is not a universal network switch for every Codex product feature or integration; separate tools can have separate controls.

Before allowing network use, identify the command, purpose, and destination and protect credentials from being exposed.

---

# Part V — Windows and CLI Configuration

## 19. Inspecting Configuration

In PowerShell, check the installed CLI and its supported options:

```powershell
codex --version
codex --help
```

Find the config path while allowing for a custom `CODEX_HOME`:

```powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}

$configPath = Join-Path $codexHome "config.toml"

if (Test-Path $configPath) {
    Get-Content $configPath
} else {
    Write-Output "No config.toml found."
}
```

Check the file for secrets before sharing its contents. A config can contain integration details or other sensitive values.

---

## 20. Backing Up Configuration

Before editing an existing configuration, make a backup:

```powershell
if (Test-Path $configPath) {
    Copy-Item $configPath "$configPath.bak"
}
```

Do not copy a missing file. Keep the backup protected because it may contain sensitive configuration. Change one setting at a time and confirm Codex accepts it.

---

## 21. Example Interactive Development Configuration

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
```

This example selects workspace-write, keeps applicable approvals interactive, and disables outbound networking for command execution in that sandbox. It may prevent dependency retrieval or remote Git operations. Adjust only when the approved task and environment require it.

---

## 22. Session-Specific CLI Overrides

For a read-only investigation:

```powershell
codex --sandbox read-only
```

For an approved workspace task with on-request approvals:

```powershell
codex --sandbox workspace-write --ask-for-approval on-request
```

CLI overrides let a session use different settings without permanently changing the user’s config. Confirm exact option names with the installed `codex --help` because CLI behavior can evolve.

---

## 23. Interactive Permission Controls

Supported Codex CLI versions provide an interactive permission selector, commonly opened with:

```text
/permissions
```

Use it to inspect or select an available permission mode/profile. Options depend on the installed client and configuration. Verify the active mode before proceeding with consequential work.

---

# Part VI — Project Examples

## 24. GroupTrack Android UI

Task: implement an approved sign-in screen alignment adjustment.

1. Start with read-only exploration and inspect `AGENTS.md` plus relevant UI documentation.
2. Identify the layout and navigation files; propose a focused plan without edits.
3. Review the plan and confirm the scope.
4. Use an appropriate workspace-write session for implementation.
5. Preserve authentication logic and avoid dependencies or unrelated screens.
6. Run available Gradle verification and review `git status`, `git diff --check`, and `git diff`.

Do not commit or push unless separately authorized. Use investigation permissions for exploration and controlled write permissions only for the approved implementation.

## 25. Laravel Backend

For an API validation bug, begin read-only: inspect the route, controller or Form Request, existing rules, and relevant tests. Propose the root cause and minimal fix before edits.

For approved changes, use a safe test environment. Avoid destructive database operations, production services, and unapproved schema changes. Run relevant tests and inspect the diff. Workspace-write does not make a database command safe by itself.

## 26. Next.js Frontend

For an API mapping issue, compare the component’s field mapping with the actual documented or observed API response. Diagnose read-only first, then use workspace-write for an approved correction. Run the project’s actual lint/build commands and report unverified browser behavior.

Build tools can create caches or output directories, so a read-only sandbox may block some checks. Verification commands need permissions appropriate to their side effects.

---

# Part VII — Security Practices

## 27. Protect Credentials

Projects may contain API keys, database passwords, service credentials, and tokens. Keep secrets out of source control, prompts, screenshots, and logs. Restrict access to sensitive files, review configuration before sharing, and rotate credentials if exposure is suspected.

A sandbox reduces some execution risks but does not replace secret-management practices.

## 28. Protect Git and Production Operations

Reading `git status`, reviewing a diff, staging, committing, and pushing have different effects. A useful project rule is:

```markdown
## Git Workflow
- Review changes before committing.
- Do not commit or push without explicit authorization.
- Do not modify unrelated files.
```

These are workflow instructions, not a guarantee that each Git command triggers an approval prompt. Keep development and production credentials separate, use isolated test databases, review migrations, and require deliberate authorization for production operations.

## 29. Avoid Permission Overreach and Blind Approval

Start read-only for review or analysis where possible. Before approving an operation, read the command, understand its target, confirm it matches the task, and look for a narrower alternative. Do not approve merely because Codex requested it.

---

# Part VIII — Common Mistakes

| Mistake | Better approach |
|---|---|
| Assuming workspace-write limits changes to filenames in the prompt | Treat the sandbox as a boundary and review actual changes |
| Assuming `on-request` prompts for every edit | Expect already-permitted operations to proceed; inspect the diff |
| Assuming read-only supports every command | Account for caches, temporary files, and build artifacts |
| Enabling broad access for convenience | Identify the exact blocked capability and adjust narrowly |
| Treating network access as harmless | Review command, destination, and data exposure |
| Assuming all platforms behave identically | Verify with the installed client and OS documentation |
| Using old policy names from tutorials | Check the current configuration reference |
| Running database or production actions in a dev workflow | Confirm environment, impact, and authorization first |

---

# Part IX — Practical Exercises

## 30. Exercise A — Classify Sandbox Modes

Choose a suitable starting mode and explain why:

| Task | Starting mode |
|---|---|
| Inspect Android architecture | Read-only |
| Review a Git diff | Read-only |
| Implement an approved UI change | Workspace-write |
| Analyze Laravel validation | Read-only |
| Add a focused regression test | Workspace-write |
| Inspect Next.js component structure | Read-only |

Consider when a test or build command might need to write files or use the network.

## 31. Exercise B — Permission Inventory

On your own development machine, inspect the Codex version and configuration. Record:

```markdown
## Codex Permission Inventory
- Codex version:
- Configuration location:
- Sandbox mode:
- Approval policy:
- Workspace network setting:
- Notes or unknowns:
```

Do not publish configuration secrets.

## 32. Exercise C — Read-Only Exploration

Start a read-only session if supported:

```powershell
codex --sandbox read-only
```

Ask Codex to read project instructions, identify project structure and technologies, and find relevant test commands. Tell it not to edit files. Compare its findings with the repository.

## 33. Exercise D — Approval Decisions

For each scenario, decide approve, deny, or investigate further:

1. Codex needs to read a controller while diagnosing a bug.
2. Codex asks to modify an unrelated repository.
3. Codex needs to download an approved dependency.
4. Codex proposes changing production database configuration during a UI task.

Explain task relevance, necessity, target, reversibility, and whether a narrower alternative exists.

## 34. Exercise E — Review Combined Settings

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
```

Identify the sandbox, approval policy, network behavior, write boundary, and whether every permitted write will pause for approval.

## 35. Exercise F — Android and Laravel Workflows

For an Android UI change and a Laravel API fix, outline when to use read-only investigation, when workspace-write is needed, which checks may create artifacts, how to protect test databases and credentials, and how to review the final diff.

## 36. Exercise G — Permission Audit

- [ ] Correct repository and environment selected.
- [ ] Applicable project instructions read.
- [ ] Task scope and non-goals stated.
- [ ] Sandbox mode matches the task.
- [ ] Approval policy understood.
- [ ] Network access justified.
- [ ] Credentials and production systems protected.
- [ ] Changes remain in scope.
- [ ] Relevant verification is completed and reported.
- [ ] Git diff reviewed.
- [ ] No unauthorized commit or push.
- [ ] Remaining risks or limitations documented.

---

## 37. Knowledge Check

1. What is a sandbox, and why does it matter for agentic coding?
2. What is the principle of least privilege?
3. What does `sandbox_mode` control?
4. When is read-only mode a good starting point?
5. When is workspace-write appropriate?
6. Why is `danger-full-access` risky?
7. What does an approval policy control?
8. What do `on-request` and `never` mean?
9. How do sandbox restrictions differ from approval prompts?
10. What is permission escalation?
11. What should you check before approving a request?
12. What are writable roots and why should they be kept narrow?
13. What does `sandbox_workspace_write.network_access` control?
14. Why can a build command need write permission?
15. How should credentials and production systems be protected?
16. Why does workspace-write not guarantee scope compliance?
17. How can CLI overrides help?
18. Why should platform behavior be verified?

---

## 38. Key Terminologies

| Term | Definition |
|---|---|
| Sandbox | Restricted execution environment for commands |
| Sandbox Mode | Policy defining command access boundaries |
| Read-Only | Mode intended for inspection without workspace modification |
| Workspace-Write | Mode permitting writes within configured boundaries |
| Approval Policy | Rules that determine when authorization is requested |
| `on-request` | Interactive policy for applicable approval requests |
| `never` | Policy that does not pause for interactive approvals |
| Permission Escalation | Request for access beyond current permissions |
| Least Privilege | Granting only the access needed |
| Writable Root | Directory permitted for modifications |
| Protected Path | Path subject to additional restrictions |
| Network Access | Ability of command execution to reach external hosts |
| CLI Override | Invocation-specific setting |
| Scope Compliance | Staying within the approved task |

---

## 39. Key Takeaways

1. Sandboxing limits the environment available to command execution.
2. Approval policies determine whether applicable operations pause for authorization.
3. Read-only is useful for investigation; workspace-write supports controlled implementation.
4. `danger-full-access` removes normal sandbox restrictions and should not be routine.
5. `on-request` does not require a prompt for every operation already allowed inside the sandbox.
6. `never` removes interactive approval pauses but does not itself grant access.
7. Keep writable roots and network access narrow.
8. Protect credentials, databases, and production environments.
9. Project instructions define task scope; sandboxing sets technical boundaries.
10. Review changes and report actual verification results.

---

## 40. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Codex Security](https://developers.openai.com/codex/security/)
3. [Sandboxing](https://developers.openai.com/codex/sandboxing/)
4. [Configuration Basics](https://developers.openai.com/codex/config-basic/)
5. [Configuration Reference](https://developers.openai.com/codex/config-reference/)
6. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
7. [Advanced Configuration](https://developers.openai.com/codex/config-advanced/)

### Supporting Documentation

8. [OpenAI — Sandbox Security for Agents](https://developers.openai.com/api/docs/guides/agents-api/environments/security)
9. [Git Documentation](https://git-scm.com/docs)
10. [TOML Specification](https://toml.io/en/)

> **Documentation note:** Permission controls, platform behavior, and configuration keys can change. Confirm exact syntax with current documentation and the installed Codex version.

---

## 41. Lesson Completion Checklist

- [ ] Understand sandboxing and least privilege.
- [ ] Identify sandbox modes and their appropriate uses.
- [ ] Distinguish sandbox and approval controls.
- [ ] Understand permission escalation and approval decisions.
- [ ] Understand writable roots and network access.
- [ ] Inspect configuration safely in PowerShell.
- [ ] Use temporary CLI overrides where appropriate.
- [ ] Apply the concepts to Android, Laravel, and Next.js work.
- [ ] Protect credentials and production systems.
- [ ] Review Git changes and complete the exercises.
- [ ] Answer the knowledge check.

---

## Next Lesson

**[[05 - Global vs Project-Specific Configuration]]**

The next lesson covers global user settings, project-level overrides, trusted repositories, configuration precedence, CLI overrides, profiles, team settings, conflicts, and Windows examples.

---

*Phase 4 — Codex Configuration | Lesson 4.4*  
*Learning Principle: Restrict access by default, authorize only what is necessary, and verify every consequential change.*
