# 09 — Caveman: Configuration, Commands, and Controls

> [!abstract] Lesson Information
> **Repository:** [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)
>
> **Category:** Agentic Engineering / CLI Configuration / Codex Integration / Developer Tooling
>
> **Difficulty:** Intermediate to Advanced
>
> **Learning Approach:** Theory First
>
> **Platform Focus:** Windows 10/11, PowerShell, VS Code, Codex CLI
>
> **Prerequisites:**
> - [[00 - Repository Overview and Purpose]]
> - [[01 - Architecture and Core Concepts]]
> - [[02 - Installation and Codex Integration]]
> - [[03 - Token Economics and Optimization]]
> - [[04 - Say Less - Output Token Optimization]]
> - [[05 - Read Less - Input and Context Compression]]
> - [[06 - Auto - Model Routing and Reasoning Effort]]
> - [[07 - Blocks - Reusable Agent Scripts]]
> - [[08 - Waste Fixes, Caching, and Browse]]
>
> **Primary Goal:** Understand Caveman's CLI, feature controls, diagnostics, configuration safety, and integration management before changing a real development environment.

---

# 1. Learning Objectives

After completing this lesson, you should be able to:

1. Explain the purpose of the Caveman CLI.
2. Distinguish Caveman CLI from a coding-agent CLI.
3. Understand the six configurable Caveman features.
4. Interpret the documented command structure.
5. Explain status, setup, toggles, login, providers, learning, trials, shrinking, statistics, and diagnostics.
6. Recognize commands intended for the terminal versus the agent conversation.
7. Understand Codex-specific integration considerations.
8. Plan safe configuration changes and rollback.
9. Diagnose common integration problems.
10. Avoid unnecessary or risky configuration changes.

---

# Part I — Understanding the Caveman CLI

## 2. What Is a CLI?

CLI means **Command-Line Interface**. It lets developers control software by entering commands in a terminal.

~~~powershell
git status
npm run build
php artisan test
~~~

Caveman provides a CLI for controlling and inspecting its optimization features. It is separate from Codex and other coding-agent applications.

| Component | Responsibility |
|---|---|
| PowerShell | Command execution environment |
| Codex CLI | AI-assisted development |
| Caveman CLI | Caveman setup, configuration, and tooling |
| AGENTS.md | Repository-specific agent instructions |
| Git | Version control and change history |

Conceptually:

~~~mermaid
flowchart TD
    A["Developer"] --> B["PowerShell"]
    B --> C["Caveman CLI"]
    C --> D["Configuration and integration"]
    D --> E["Coding agent"]
    E --> F["Project work"]
    F --> G["Git and verification"]
~~~

---

# Part II — The Six Configurable Features

## 3. Caveman's Six Rocks

The repository describes six primary features, each with an independent switch.

| Feature | Toggle name | Purpose |
|---|---|---|
| Say Less | output | Reduce unnecessary agent responses |
| Read Less | input | Compress supported tool output |
| Auto | routing | Select model and reasoning effort |
| Blocks | scripts | Reuse agent scripts |
| Waste Fixes | waste-fixes | Identify recurring inefficiencies |
| Browse | browse | Compress webpage content |

### Why Separate Toggles Matter

Separate controls let you evaluate one feature at a time. For example:

- Disable Browse when a task depends on a complete page representation.
- Evaluate input compression separately from model routing.
- Leave routing off until you understand account and data-flow requirements.

**Keep a feature enabled because it helps your workflow, not simply because it is available.**

---

# Part III — Command Categories

## 4. Command Map

| Category | Commands |
|---|---|
| Setup | setup |
| Inspection | status, stats, doctor |
| Feature control | on, off |
| Agent launching | caveman codex, caveman claude |
| Authentication | login, providers add |
| Analysis | learn |
| Apply reviewed optimizations | learn implement |
| Benchmarking | trial |
| Output compression | shrink |
| Browsing | browse |
| Experimental conversion | convert --dry-run |

## 4.1 Terminal Commands vs Agent Commands

A terminal command is entered into PowerShell. A skill or slash-command is entered in the coding agent's conversation interface. They are not interchangeable.

The exact command syntax can change. Use the installed version's help and current documentation before relying on an example.

---

# Part IV — Inspecting Configuration

## 5. caveman status

The README documents:

~~~powershell
caveman status
~~~

It reports which features are on, which agents the integration reaches, and a suggested next step.

A conceptual example:

~~~text
Output: enabled
Input: enabled
Routing: disabled
Scripts: enabled
Browse: disabled
Codex: detected
~~~

This output is illustrative, not an exact CLI transcript.

Use status after installation, after toggles, before a trial, and during troubleshooting. An enabled feature does not by itself prove that every eligible request passed through it.

## 5.1 caveman doctor

The README documents:

~~~powershell
caveman doctor
~~~

It describes the command as reporting one problem per line with a fix. Diagnostics can help surface integration or configuration issues, but review proposed changes before applying them.

A cautious sequence:

~~~mermaid
flowchart TD
    A["Unexpected behavior"] --> B["Check status"]
    B --> C["Run doctor"]
    C --> D["Read the reported issue"]
    D --> E["Review the proposed fix"]
    E --> F["Apply the smallest correction"]
    F --> G["Verify again"]
~~~

---

# Part V — Setup and Feature Controls

## 6. caveman setup

The README documents:

~~~powershell
caveman setup
~~~

This reopens the setup experience. Treat setup as potentially state-changing; do not assume it is read-only.

Before rerunning it:

1. Record the current configuration.
2. Identify integrated agents.
3. Preserve relevant files and settings.
4. Review proposed changes.
5. Verify the final state.

## 6.1 caveman on and caveman off

Examples from the README:

~~~powershell
caveman on browse
caveman off browse
~~~

Documented feature names include output, input, waste-fixes, routing, scripts, and browse.

A safe experiment changes one toggle, checks status, runs a controlled task, measures results, and restores the previous state if the result is worse.

Do not toggle several features at once when trying to identify the source of a behavior change.

---

# Part VI — Launching Agents and Skills

## 7. Launching a Coding Agent

The README documents launcher usage such as:

~~~powershell
caveman claude
caveman codex
~~~

Launching through Caveman may activate integration behavior differently from launching the agent directly. Verify which path your installed version uses.

Do not assume plain codex and caveman codex behave identically.

## 7.1 Standalone Skill vs Full Toolkit

| Standalone skill | Full toolkit |
|---|---|
| Primarily provides Say Less instructions | Can include multiple optimization features |
| Lighter setup | More integration and configuration |
| Does not inherently enable every feature | Features still depend on setup and controls |

Installing the skill alone does not prove that input compression, routing, scripts, or Browse are active.

---

# Part VII — Authentication and Providers

## 8. caveman login

The README documents:

~~~powershell
caveman login
~~~

Caveman says Auto requires account sign-in. Account authentication is distinct from model-provider authentication; signing in does not necessarily configure every optional provider.

Use official authentication flows, protect credentials, and review permissions and data handling.

## 8.1 Provider Setup

The README documents optional provider configuration:

~~~powershell
caveman providers add openai
~~~

Adding a provider may expand model choices, but can introduce provider credentials, separate billing, and another data-processing path.

A ChatGPT subscription and API billing are not automatically interchangeable. Verify the exact authentication and billing model for the configuration you plan to use.

## 8.2 Routing Data and Cloud Controls

The current Caveman README says that, after sign-in and selecting Auto, routing requests send Caveman Cloud the latest ask (including context attached by the agent), the previous ask, the end of the prior response, tool names, effort, selected agent headers, and prior request token counts. It says Free-plan data may be retained to improve routing.

If a request is routed to a Caveman Cloud model, the README says the complete request can pass through Caveman Cloud, including system prompt, tool results, and code. Read the live [data and routing disclosure](https://github.com/JuliusBrussee/caveman#caveman-cloud) and [Security Policy](https://github.com/JuliusBrussee/caveman/blob/main/SECURITY.md) before enabling Auto for sensitive work.

The README documents these controls:

~~~powershell
caveman off routing
caveman providers cloud off
~~~

The first disables Auto routing; the second disables the Caveman Cloud model-provider path. Verify their exact scope and resulting state with the installed version. These controls are not interchangeable: turning off one path does not necessarily disable the other.

---

# Part VIII — Learning and Applying Optimizations

## 9. caveman learn

The README documents:

~~~powershell
caveman learn
~~~

It analyzes local agent history to rank possible token-waste improvements. The suggestions are hypotheses, not guaranteed improvements. Review them against project rules and actual workflow needs.

## 9.1 caveman learn implement

The README describes:

~~~powershell
caveman learn implement
~~~

This applies proposed changes one diff at a time, subject to user approval.

For each proposed diff:

1. Inspect what changes.
2. Understand the original behavior.
3. Check architecture and security instructions.
4. Approve only changes that fit.
5. Verify the result and keep a rollback path.

Optimization suggestions do not authorize bypassing project rules.

---

# Part IX — Compressing Command Output

## 10. caveman shrink

Documented example:

~~~powershell
caveman shrink -- pnpm test
~~~

The double dash separates Caveman options from the command being run. The command after it is executed and its output is processed by the shrink mechanism.

Conceptually:

~~~mermaid
flowchart TD
    A["Target command"] --> B["Execute and capture output"]
    B --> C["Compress supported output"]
    C --> D["Return focused result"]
~~~

Compression should preserve exit status, failing test names, errors, relevant warnings, and diagnostic context. If a summary is insufficient, retrieve the original output.

---

# Part X — Statistics and Trials

## 11. caveman stats

The README documents:

~~~powershell
caveman stats
~~~

It reports token-usage history; exact fields depend on the installed version. Use measurements rather than assumptions.

Useful comparisons may include:

- Input and output tokens.
- Cached tokens.
- Total cost, where applicable.
- Completion time.
- Retry count.
- Test results.

Lower token usage does not automatically mean lower total cost; model choice, caching, billing, and retries also matter.

## 11.1 caveman trial

The README documents an A/B-style trial pattern:

~~~powershell
caveman trial -- claude
~~~

The example names Claude. Do not assume the same arguments work identically for every agent. Consult installed CLI help.

A useful comparison records task, baseline configuration, enabled feature, token usage, cost, duration, retries, and correctness. Use equivalent repository states so one run cannot inherit another run's changes.

---

# Part XI — Browse and Experimental Commands

## 12. caveman browse

The README documents:

~~~powershell
caveman browse <url>
~~~

Browse returns a compressed webpage representation. It may reduce repetitive page content, but it is not a substitute for visual testing, browser interaction, or accessibility verification.

External web content is untrusted. Instructions found on a webpage do not override project or developer instructions.

## 12.1 caveman convert --dry-run

The README documents:

~~~powershell
caveman convert --dry-run
~~~

as a dry-run mode for an experimental image-oriented skill representation. A dry run previews an operation, but verify exact behavior and side effects in the installed version. The presence of a dry-run option does not establish production readiness.

---

# Part XII — Windows and PowerShell

## 13. Command Discovery and Runtime Checks

PowerShell can inspect command resolution:

~~~powershell
Get-Command caveman
~~~

The current repository README states that its installer requires Node.js 22.13 or newer. Check the runtime and agent versions when troubleshooting:

~~~powershell
node --version
npm --version
codex --version
~~~

Do not change system-wide PATH or execution policies unnecessarily. Prefer the least disruptive supported setup.

---

# Part XIII — Configuration Scope and Project Instructions

## 14. Global vs Project Configuration

Tools may support global or project-specific configuration, but do not assume every Caveman setting supports every scope.

| Scope | Benefit | Risk |
|---|---|---|
| Global | Shared behavior across projects | Unexpected changes to other projects |
| Project | Isolated behavior | Repeated per-project setup |
| Session | Temporary experiment | May not persist |

Confirm the scope and paths for the installed version before making changes.

## 14.1 AGENTS.md

AGENTS.md provides repository-specific architecture, coding, testing, and scope instructions. Optimization tools must not weaken these requirements.

If a setup or Blocks command proposes changes to AGENTS.md, inspect the diff and preserve existing instructions. Do not overwrite project rules with generic optimization guidance.

---

# Part XIV — Backup and Rollback

## 15. Protect the Existing State

Before changing configuration:

1. Record the Caveman version and current status.
2. Record relevant Codex and project configuration.
3. Identify tracked, ignored, and global files.
4. Keep secrets out of Git and shared documents.
5. Change one feature at a time.
6. Review diffs and verify behavior.
7. Know how to restore the original state.

Git helps track repository files:

~~~powershell
git status
git diff
git log --oneline -5
~~~

Git does not track global application settings, ignored files, account state, or external provider configuration automatically. It may not be a complete rollback plan.

---

# Part XV — Troubleshooting

## 16. Common Problem Categories

**Command not found**
- Installation may be incomplete.
- The executable may not be on PATH.
- The terminal may need restarting.
- A different install scope may have been used.

**Feature appears disabled**
- The toggle may be off.
- Setup may be incomplete.
- A different launcher or integration may be active.

**Auto unavailable**
- Sign-in may be missing.
- The integration or provider may be unsupported.
- Authentication may have expired.

**No measurable savings**
- The workload may already be efficient.
- The feature may not apply to the task.
- Compression overhead or retries may offset savings.

**Unexpected agent behavior**
- Instructions may conflict.
- The launch path may differ.
- Context may be missing.
- A feature or provider may have changed.

These are hypotheses. Use actual status and diagnostic output to determine the cause; avoid repeated reinstalls without evidence.

## 16.1 Troubleshooting Flow

~~~mermaid
flowchart TD
    A["Unexpected Caveman behavior"] --> B{"CLI available?"}
    B -->|No| C["Inspect install and PATH"]
    B -->|Yes| D["Check status"]
    D --> E{"Feature enabled?"}
    E -->|No| F["Review configuration"]
    E -->|Yes| G["Run diagnostics"]
    G --> H["Verify agent integration"]
    H --> I["Test one controlled task"]
    I --> J{"Expected result?"}
    J -->|Yes| K["Record working state"]
    J -->|No| L["Review evidence and restore baseline"]
~~~

---

# Part XVI — Security and Cost

## 17. Security Checklist

- Install from the intended source.
- Review scripts and proposed configuration edits.
- Understand authentication and provider paths.
- Protect API keys and account tokens.
- Avoid unnecessary privileges.
- Check what data leaves the machine.
- Maintain a rollback path.

A token-saving feature should not require weakening project security without a justified, reviewed reason.

## 17.1 Configuration and Economics

| Feature | Potential economic effect |
|---|---|
| Say Less | Fewer output tokens |
| Read Less | Fewer input tokens |
| Auto | Different model and effort costs |
| Blocks | Less repeated script generation |
| Waste Fixes | Reduced recurring inefficiency |
| Browse | Smaller webpage representations |

These effects can overlap. Do not add published percentages as if they were independent. Measure the complete workflow, including model cost, tools, retries, and human correction when relevant.

---

# Part XVII — Project Scenarios

## 18. Laravel: GoPasig

A Laravel fleet-management agent may run tests, inspect query logs, review controllers, and check migrations.

Evaluate one feature at a time. Preserve SQL errors, failed assertions, transaction details, and migration warnings. A compact test summary is useful only if it still supports root-cause analysis.

## 19. Next.js: E-Commerce

For a Next.js frontend with a Laravel backend, compare ordinary and compressed build output. Preserve TypeScript/build errors, important warnings, source locations, and exit status. Do not add tools to optimize an already stable, low-overhead workflow without evidence.

## 20. Android: GroupTrack

For a Java/XML Android project, Gradle output may contain the failing task, compiler diagnostics, dependency coordinates, file locations, and warnings. A useful summary must preserve those details. Keep the existing MVP and architecture in control.

---

# Part XVIII — Practical Exercises

## 21. Exercise 1 — Command Classification

Classify each command as setup, inspection, diagnostics, analysis, or benchmarking:

~~~powershell
caveman status
caveman setup
caveman doctor
caveman stats
caveman learn
caveman trial -- claude
~~~

## 22. Exercise 2 — Feature Mapping

Match Say Less, Read Less, Auto, Blocks, Waste Fixes, and Browse to their toggle names. Explain why testing them independently helps.

## 23. Exercise 3 — Skill vs Toolkit

Explain why a standalone Caveman skill does not necessarily enable input compression or model routing.

## 24. Exercise 4 — PowerShell Troubleshooting

PowerShell reports that caveman is not recognized. List likely causes and a safe diagnostic order.

## 25. Exercise 5 — Backup Checklist

Design a backup checklist for Codex integration settings. Include versions, configuration files, secrets, and rollback.

## 26. Exercise 6 — Controlled Feature Test

Design an A/B test for input compression. Define baseline, representative task, metrics, acceptance criteria, and rollback condition.

## 27. Exercise 7 — Authentication and Providers

Explain the difference between caveman login and adding an OpenAI provider. Discuss separate credentials, billing, and data-processing paths.

## 28. Exercise 8 — No Savings

A feature is enabled, but measured token usage is unchanged. Give at least five possible explanations and state what evidence you would inspect.

## 29. Exercise 9 — AGENTS.md Review

A setup tool proposes edits to AGENTS.md. Describe how to inspect the diff and preserve architecture and scope instructions.

## 30. Exercise 10 — Configuration Decision

A project builds reliably and its agent workflow is stable. Explain why enabling all features at once is a poor experiment, and how you would evaluate each feature.

---

# Part XIX — Knowledge Check

## 31. Review Questions

1. What is the Caveman CLI?
2. How does it differ from Codex CLI?
3. What are the six Caveman features?
4. What does caveman status report?
5. What does caveman setup do?
6. What are feature toggles?
7. What are the documented toggle names?
8. What is the caveman codex launcher for?
9. How does the standalone skill differ from the toolkit?
10. What does caveman login do?
11. Why are provider integrations optional?
12. What does caveman learn analyze?
13. What does caveman learn implement do?
14. Why review every proposed diff?
15. What does caveman shrink do?
16. What does caveman stats report?
17. What does caveman doctor do?
18. What is caveman trial for?
19. What does caveman browse do?
20. Why does Windows shell compatibility matter?
21. What is configuration scope?
22. Why protect AGENTS.md?
23. What can Git rollback, and what may it miss?
24. Why test features individually?
25. What makes a configuration reliable?

---

# Part XX — Key Takeaways

> [!summary]
> - Caveman provides a CLI for managing its optimization toolkit; it is separate from Codex.
> - Six feature names and command details come from the public README and can change by version.
> - Status and diagnostics help inspect the installation; enabled does not prove a feature affected a request.
> - Setup and optimization commands may change configuration, so review diffs.
> - Sign-in and provider setup are separate concerns.
> - Learn recommendations are hypotheses; review each proposed change.
> - Shrunk output must preserve errors, exit status, and useful diagnostics.
> - Trials should compare equivalent tasks and repository states.
> - Back up relevant settings and know what Git does not track.
> - Change one feature at a time.
> - Preserve project architecture, security, and acceptance criteria.

---

# Part XXI — Final Mental Model

~~~mermaid
flowchart TD
    A["Developer"] --> B["Caveman CLI"]
    B --> C["Inspect"]
    B --> D["Configure"]
    B --> E["Diagnose"]
    B --> F["Measure"]
    C --> G["status"]
    D --> H["setup / on / off"]
    E --> I["doctor"]
    F --> J["stats / trial"]
    G --> K["Known configuration"]
    H --> K
    I --> K
    J --> K
    K --> L["Codex workflow"]
    L --> M["Verify correctness"]
    M --> N["Measure efficiency"]
    N --> O{"Worth keeping?"}
    O -->|Yes| P["Keep configuration"]
    O -->|No| Q["Rollback"]
~~~

> [!important] Final Principle
> **Good configuration management is not about enabling every feature. It is about maintaining a reliable, understandable, secure, and measurable development environment.**

---

# Part XXII — Official References

## Caveman

- [Official Caveman Repository](https://github.com/JuliusBrussee/caveman)
- [Caveman README](https://github.com/JuliusBrussee/caveman/blob/main/README.md)
- [Caveman Installation Guide](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- [Caveman Documentation](https://docs.caveman.so/caveman)

## Codex

- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [Codex AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md)

## Windows and PowerShell

- [Microsoft PowerShell Documentation](https://learn.microsoft.com/powershell/)
- [Node.js Documentation](https://nodejs.org/en/docs)

## Version Control

- [Git Documentation](https://git-scm.com/doc)

> [!note] Documentation Scope
> Commands and feature names reflect the public Caveman README reviewed on October 21, 2026. Exact CLI output, paths, flags, and platform behavior depend on the installed release.
>
> Diagrams, example output, troubleshooting hypotheses, and exercises are educational illustrations.
>
> No commands were executed, no settings were changed, and no integration was installed for this lesson.

---

# Part XXIII — Next Lesson

**[[10 - Benchmarks and Performance Evaluation]]**

Topics:

1. Why benchmarking matters.
2. Baseline establishment.
3. Token measurement.
4. Input vs output token metrics.
5. Cached-token accounting.
6. Cost per successful task.
7. Latency and throughput.
8. Benchmark reproducibility.
9. Caveman's published measurements.
10. Statistical uncertainty.
11. A/B testing methodology.
12. Quality-preserving optimization.
13. Regression detection.
14. Laravel, Next.js, and Android benchmarks.
15. Practical exercises.
16. Official references.

**End of Lesson 09 — Configuration, Commands, and Controls.**
