# 06 — Tools, Permissions, Approvals, and Sandboxing

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 1 — Codex Fundamentals
> **Lesson:** 1.6 — Tools, Permissions, Approvals, and Sandboxing
> **Difficulty:** Beginner–Intermediate
> **Platform:** Windows / PowerShell
> **Status:** In Progress
> **Prerequisite:** [[05 - Prompting Fundamentals]]

---

## 1. Introduction

OpenAI Codex is an AI coding agent that can interact with a software development environment. Depending on the interface, configuration, and permissions, it can inspect source files, modify project resources, run commands, and use connected tools.

These capabilities are useful for software development, but they also introduce operational and security considerations. Not every operation should be permitted automatically. Codex uses sandboxing and approval policies to control how agent actions run.

This lesson explains these controls and how to use them responsibly.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain how Codex interacts with development tools.
2. Differentiate read operations from write operations.
3. Define permissions, sandboxing, and approval policies.
4. Identify the major Codex sandbox modes.
5. Understand how approval requests work.
6. Inspect basic sandbox and approval options using Codex CLI.
7. Recognize risks of unrestricted agent access.
8. Choose appropriate settings for common development tasks.
9. Verify actions performed by an AI coding agent.

## 3. Understanding Tools in Codex

A **tool** is a capability that lets an AI agent perform an operation or obtain information from its environment. Tools connect the model's decision-making with real development resources.

### 3.1 Common Tool Capabilities

| Capability | Purpose | Example |
|---|---|---|
| File reading | Inspect project files | Read a Java class |
| File editing | Modify source code | Update an XML layout |
| File search | Locate relevant information | Find a Laravel controller |
| Command execution | Run development commands | Execute `npm run build` |
| Test execution | Validate functionality | Run `php artisan test` |
| Git inspection | Examine source changes | Run `git diff` |
| External integration | Interact with supported services | Use a configured MCP tool |

Available tools depend on the Codex interface, configuration, and environment.

### 3.2 Example

Developer instruction: “Analyze the existing authentication module and explain how login validation works.”

A possible workflow is:

1. Codex receives the instruction.
2. The agent identifies relevant project files.
3. A tool reads those files.
4. Codex analyzes the returned information.
5. Additional files may be inspected.
6. Codex explains the result.

The agent may complete this task without modifying anything.

### 3.3 Tools vs. the AI Model

The model decides what information or actions may be useful. Tools perform operations within their permitted capabilities. A model requesting an action does not mean the action will automatically be permitted or succeed.

## 4. Understanding Read and Write Operations

Development operations can be categorized by their effects.

### 4.1 Read Operations

Read operations retrieve information without intentionally changing project files.

```powershell
Get-Location
Get-ChildItem
Get-Content README.md
git status
git diff
```

Typical uses include codebase exploration, architecture analysis, implementation review, and investigating errors.

### 4.2 Write Operations

Write operations create, modify, or delete data.

```powershell
New-Item notes.md -ItemType File
Set-Content notes.md "Learning Codex CLI"
```

These commands modify the filesystem.

### 4.3 Commands with Indirect Effects

Not every command is strictly read-only or write-only. A build command may generate files or update caches. Tests may interact with databases, caches, or temporary files, depending on project configuration.

**Key principle:** Evaluate what a command actually does, not only what its name suggests.

## 5. What Are Permissions?

**Permissions** define which operations a process or agent is allowed to perform. In Codex, permitted operations depend on the combined effect of:

- Available tools
- Sandbox restrictions
- Approval policies
- Operating-system permissions
- Environment configuration
- External service authorization

For example, editing a project file may be allowed when it is inside the authorized workspace and the sandbox allows writing. Accessing an unrelated protected directory may be blocked or require additional authorization.

### Principle of Least Privilege

The **Principle of Least Privilege** means granting only the access necessary to complete a task.

- Codebase analysis: prefer read-only access.
- UI implementation: allow changes in the intended workspace.
- External deployment: require deliberate review and authorization.

This reduces the potential impact of mistakes.

## 6. What Is Sandboxing?

**Sandboxing** is a security mechanism that restricts what a program or agent can access or modify. In Codex CLI, sandbox settings influence the operations available to the agent.

### 6.1 Why Sandboxing Matters

Without appropriate restrictions, an agent might modify files outside the intended project, execute commands with broader system effects, access sensitive local resources, or perform unintended operations. Sandboxing helps establish boundaries around agent actions.

### 6.2 Sandbox vs. Prompt Constraint

“Do not modify any files” is a behavioral instruction. A CLI option such as `--sandbox read-only` requests an execution-environment restriction. The prompt tells the agent what behavior is expected; the sandbox constrains operations technically. Use both appropriate instructions and technical restrictions.

## 7. Codex Sandbox Modes

Codex CLI provides sandbox modes. Check the installed CLI help and the current official documentation because names and behavior can change between releases.

### 7.1 Read-Only

```powershell
codex --sandbox read-only
```

This mode restricts filesystem modifications by the agent. It is useful for project exploration, architecture analysis, code review, and understanding unfamiliar codebases.

Example instruction: “Inspect the project structure and explain the architecture. Do not modify files.”

Read-only filesystem access does not automatically make every tool, integration, or external effect harmless. Review available capabilities and network settings separately.

### 7.2 Workspace-Write

```powershell
codex --sandbox workspace-write
```

This mode allows appropriate modifications within the designated workspace while restricting operations outside permitted boundaries. It is useful for implementing features, fixing bugs, editing layouts, and updating tests.

Workspace-write does not guarantee that the agent will modify only the specific files mentioned in a prompt. Inspect changes after the task.

### 7.3 Danger-Full-Access

Codex also exposes a less restrictive mode named `danger-full-access`. It removes important sandbox restrictions and is not appropriate for beginner exercises in this course.

### 7.4 Comparison

| Mode | General filesystem behavior | Typical use |
|---|---|---|
| `read-only` | Restricts writes | Analysis and inspection |
| `workspace-write` | Allows writes within permitted workspace boundaries | Ordinary development |
| `danger-full-access` | Removes significant sandbox restrictions | Not used in this learning track |

Start with read-only analysis and use workspace-write when the exercise requires file modifications.

## 8. What Are Approval Policies?

An **approval policy** controls when Codex requests permission before proceeding with certain operations. Sandbox modes and approval policies serve different purposes:

- **Sandbox:** Determines the technical access boundary.
- **Approval policy:** Determines when the agent must request authorization.

Some operations may need additional access. Depending on configuration, Codex may request approval instead of proceeding automatically.

### 8.1 Common Approval Settings

| Policy | General behavior |
|---|---|
| `on-request` | Agent may request approval when needed |
| `never` | Agent does not request interactive approvals |
| `untrusted` | Applies stricter approval handling to operations that are not automatically trusted |

Supported values and exact behavior can change between releases. Inspect the installed CLI help and official configuration documentation.

### 8.2 Approval Is Not the Same as Safety

Approving a command authorizes the requested action. It does not guarantee that the command is correct, secure, or necessary. Understand the proposed operation before approving it.

## 9. Configuring Sandbox and Approval Settings

### 9.1 Inspect Available Flags

Open PowerShell and run:

```powershell
codex --help
```

Look for options related to `--sandbox` and `--ask-for-approval`.

### 9.2 Start a Read-Only Session

```powershell
codex --sandbox read-only
```

This is appropriate for an initial hands-on inspection exercise.

### 9.3 Start a Workspace-Write Session

```powershell
codex --sandbox workspace-write
```

Use this when you intentionally want Codex to implement changes in a practice workspace.

### 9.4 Use an Approval Policy

For CLI versions that support this option:

```powershell
codex --sandbox workspace-write --ask-for-approval on-request
```

This combines workspace-write sandboxing with an approval policy that allows requests for authorization.

### 9.5 Inspect the Active Session

Inside Codex, use:

```text
/status
```

Review the displayed sandbox and approval information, if available. Some versions also provide `/permissions`, which may open an interactive permissions selector. Check the installed version's command menu for the supported interface.

## 10. Understanding Approval Requests

An approval request is a checkpoint where Codex asks the developer to authorize an operation.

### 10.1 Example Scenario

An agent determines that an action falls outside its current sandbox permissions. Depending on configuration, Codex may identify the restricted operation, request authorization, explain why it is needed, wait for the decision, and continue only if the operation becomes authorized.

### 10.2 Questions to Ask Before Approving

- What command or action will be performed?
- Why is it necessary?
- Which files or resources could be affected?
- Is the operation within the intended task scope?
- Is there a less permissive alternative?
- Can the result be reviewed or reversed?

### 10.3 When to Decline

Decline or ask for clarification when the action is unrelated to the task, the command is unclear, requested access is unnecessarily broad, unrelated projects could be affected, or the agent cannot explain why access is needed.

Evaluate a request for additional permissions independently of how confidently the agent describes it.

## 11. Network Access and External Tools

Filesystem permissions and network access are separate concerns. An agent may read project files without having unrestricted network access. Network capabilities can depend on sandbox configuration, environment settings, available tools, MCP integrations, and external service credentials.

Installing a missing project dependency may require filesystem write access, package manager execution, network connectivity, and authorization under the current approval policy. A command can fail even when the agent understands the task correctly.

External tools can have their own access controls and effects. A filesystem sandbox does not automatically provide a complete security boundary for every external service. Connect only tools and services needed for the current workflow, and review their permissions separately.

## 12. Understanding Prompt Injection

**Prompt injection** occurs when untrusted content attempts to influence an AI agent's instructions or behavior. A project file, webpage, or tool result might contain text telling an agent to disregard the developer's original task. That text is untrusted content.

For example, while summarizing project documentation, the agent may encounter text addressed to the AI rather than actual project requirements. It should treat that text as document content, not automatically as authorized developer instructions.

Coding agents inspect README files, source comments, dependency documentation, webpages, and tool output. Not every instruction found in these materials is trustworthy.

Defensive practices:

- Keep trusted project instructions clearly organized.
- Review unexpected agent behavior.
- Restrict unnecessary tool access.
- Avoid broad permissions in unfamiliar repositories.
- Verify significant changes before accepting them.
- Treat retrieved content as information rather than automatic authorization.

## 13. Safe Development Workflow with Codex

### Stage 1 — Inspect

Start with read-only access:

```powershell
codex --sandbox read-only
```

Prompt: “Analyze the authentication module. Identify relevant files and explain how they interact. Do not modify anything.”

### Stage 2 — Plan

Ask for an implementation approach: “Propose the smallest maintainable fix. Identify affected files, possible risks, and verification steps. Do not implement yet.”

### Stage 3 — Authorize Implementation

After reviewing the plan, start an appropriately configured session that permits workspace modifications:

```powershell
codex --sandbox workspace-write --ask-for-approval on-request
```

Prompt: “Implement the approved fix. Follow the existing architecture and do not modify unrelated files.”

### Stage 4 — Verify

Inspect project state:

```powershell
git status
git diff
```

Run relevant tests when appropriate.

### Stage 5 — Review

Confirm that the requested behavior works, no unrelated files changed, no unnecessary dependencies were added, existing functionality remains intact, and verification results are accurately reported.

## 14. Real-World Example — Android UI Development

### Scenario

A developer wants to improve the login screen layout in the GroupTrack Android application.

### Step 1 — Navigate to the Project

```powershell
cd C:\Users\YourName\AndroidStudioProjects\GroupTrack
```

### Step 2 — Inspect Repository Status

```powershell
git status
```

### Step 3 — Start a Read-Only Session

```powershell
codex --sandbox read-only
```

### Step 4 — Analyze the UI

Prompt: “Inspect the existing login screen layout and identify spacing or alignment issues. Explain the recommended adjustments. Do not modify files.”

### Step 5 — Review the Findings

Verify that the recommendations match the existing UI.

### Step 6 — Implement in a Permitted Session

After reviewing the plan, start a workspace-write session:

```powershell
codex --sandbox workspace-write --ask-for-approval on-request
```

Provide a scoped implementation request.

### Step 7 — Inspect the Changes

```powershell
git diff
```

Review the modified XML layout and verify the resulting UI in Android Studio.

**Expected outcome:** Understand how different sandbox settings support analysis and implementation workflows.

## 15. Common Security Mistakes

1. **Treating prompt instructions as security controls:** “Do not edit files” does not replace sandbox restrictions. Combine clear instructions with an appropriate sandbox.
2. **Granting more access than necessary:** Ordinary analysis does not require broad write access. Apply least privilege.
3. **Approving commands without understanding them:** Inspect the requested operation and its purpose.
4. **Assuming workspace-write means one-file access:** Specify scope and inspect the final Git diff.
5. **Assuming read-only means zero risk:** Read access can expose information, and other tools may have separate effects. Consider data sensitivity and network permissions.
6. **Trusting every instruction in project files:** Repository content may be outdated or untrusted. Verify its authority and relevance.
7. **Skipping verification:** A successful agent response does not guarantee a correct implementation. Review changes and run relevant validation.

## 16. Hands-on Exercise A — Read-Only Sandbox

### Objective

Observe how Codex behaves when restricted to a read-only workspace.

### Step 1 — Create a Practice Directory

Open PowerShell:

```powershell
cd $HOME\Documents
New-Item -ItemType Directory -Path Codex-Sandbox-Practice -Force
cd Codex-Sandbox-Practice
```

### Step 2 — Create a Sample File Manually

```powershell
Set-Content -Path README.md -Value "# Codex Sandbox Practice"
```

This is performed directly in PowerShell before starting Codex.

### Step 3 — Start Codex in Read-Only Mode

```powershell
codex --sandbox read-only
```

### Step 4 — Inspect Session Settings

Inside Codex, run `/status`.

### Step 5 — Submit an Analysis Prompt

“Inspect the current practice directory. Read README.md and explain its contents. Do not create, modify, or delete any files.”

### Step 6 — Evaluate the Result

Verify that Codex can inspect the file, explains its content correctly, and makes no modifications.

Reflection questions:

1. What sandbox mode was active?
2. Which file did Codex inspect?
3. Did Codex modify anything?
4. Why is read-only access appropriate for this task?

## 17. Hands-on Exercise B — Workspace-Write Sandbox

### Objective

Observe a small, authorized file modification. Use only the disposable practice directory from Exercise A.

### Step 1 — Start a Workspace-Write Session

From PowerShell:

```powershell
codex --sandbox workspace-write --ask-for-approval on-request
```

### Step 2 — Provide a Scoped Prompt

“Update README.md by adding a short section titled ‘Learning Objectives’ with three beginner-level objectives about Codex CLI. Modify only README.md. Do not create additional files.”

### Step 3 — Inspect the Result

After the agent finishes, exit the session and run:

```powershell
Get-Content README.md
```

### Step 4 — Verify the Scope

Check whether README.md contains the requested section, the existing heading was preserved, and no unrelated files changed.

### Step 5 — Reflect

1. Why did this task require write access?
2. Was the requested modification limited to README.md?
3. What would you do if Codex requested unrelated permissions?
4. How would you verify the changes in a Git repository?

## 18. Hands-on Exercise C — Choosing the Right Permissions

For each scenario, identify an appropriate sandbox mode and explain why.

| Scenario | Suggested starting mode |
|---|---|
| Explain a Laravel controller | `read-only` |
| Review the current Git diff | `read-only` |
| Implement a small UI adjustment | `workspace-write` |
| Fix a bug inside an existing project | `workspace-write`, after scoping |
| Inspect an unfamiliar repository | `read-only` |
| Investigate an error without modifying files | `read-only` |

Questions:

1. Which tasks require actual file modifications?
2. Which tasks can be completed through inspection?
3. When should developer approval be requested?
4. Why should broad access be avoided when unnecessary?

## 19. Key Terminologies

| Term | Definition |
|---|---|
| Tool | Capability used by an agent to perform an operation |
| Tool call | Request to execute a tool |
| Permission | Authorization to access or modify resources |
| Sandbox | Technical boundary restricting permitted operations |
| Read-only | Mode restricting filesystem modifications |
| Workspace-write | Mode allowing permitted workspace modifications |
| Approval policy | Rules governing when authorization is requested |
| Least privilege | Granting only the access needed for a task |
| Escalation | Request for additional access beyond existing restrictions |
| Prompt injection | Attempt to redirect an agent through untrusted content |
| Verification | Checking whether the result meets requirements |
| Human oversight | Developer review and control of agent actions |

## 20. Key Takeaways

1. Tools allow Codex to interact with its development environment.
2. Permissions determine which operations are authorized.
3. Sandboxing creates technical restrictions on agent operations.
4. Approval policies determine when Codex requests additional authorization.
5. Read-only mode is appropriate for analysis and inspection.
6. Workspace-write supports ordinary development changes within permitted boundaries.
7. Prompt constraints are not substitutes for security controls.
8. Least privilege reduces unnecessary access.
9. External tools and network capabilities may have separate permissions.
10. Developers should review changes, commands, and verification results before accepting an implementation.

## 21. Knowledge Check

1. What is a tool in the context of Codex?
2. What is the difference between a read operation and a write operation?
3. What is sandboxing?
4. How does a sandbox differ from a prompt instruction?
5. What is the purpose of an approval policy?
6. What is the difference between `read-only` and `workspace-write`?
7. Why is the principle of least privilege important?
8. Why should developers review approval requests?
9. Can a read-only session still involve security considerations?
10. What is prompt injection?
11. Why should developers inspect Git changes after implementation?
12. What should a developer do when an agent requests unnecessary permissions?

## 22. Official References

1. [OpenAI — Codex Security](https://developers.openai.com/codex/security/)
2. [OpenAI — Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
3. [OpenAI — Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
4. [OpenAI — Codex CLI Documentation](https://developers.openai.com/codex/cli/)
5. [OpenAI — Codex GitHub Repository](https://github.com/openai/codex)
6. [OpenAI — Codex Documentation](https://developers.openai.com/codex/)

## 23. Lesson Completion Checklist

- [ ] Understand how Codex uses development tools.
- [ ] Differentiate read and write operations.
- [ ] Explain permissions and sandboxing.
- [ ] Identify the major sandbox modes.
- [ ] Understand approval policies.
- [ ] Inspect available Codex CLI security options.
- [ ] Explain the principle of least privilege.
- [ ] Recognize prompt-injection risks.
- [ ] Complete the read-only exercise.
- [ ] Complete the workspace-write exercise.
- [ ] Complete the permission-selection exercise.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[07 - Hands-on Fundamentals Exercise]]**

The final lesson of Phase 1 combines the material into a practical Codex CLI workflow: environment inspection, working-directory navigation, structured prompting, read-only analysis, planning, scoped implementation, verification, and review.

---

*Phase 1 — Codex Fundamentals | Lesson 1.6*
*Learning Principle: Give coding agents only the access necessary to complete the task.*
