# 03 — Agent Skills and Reusable Workflows

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 5 — Advanced Codex
> **Lesson:** 5.3 — Agent Skills and Reusable Workflows
> **Difficulty:** Intermediate to Advanced
> **Environment:** Windows, PowerShell, VS Code, Codex CLI
> **Prerequisite:** [[02 - MCP Servers and External Tool Integrations]]
> **Next Lesson:** [[04 - Multi-Agent Collaboration and Task Delegation]]
> **Status:** Theory Documented

---

# 1. Introduction

In agentic software development, developers often repeat instructions: inspect architecture first, review for defects, follow a debugging procedure, validate before reporting, or use an established documentation format.

**Agent Skills** package reusable, task-specific instructions together with optional references, templates, and scripts. A compatible agent such as Codex can use a skill to follow a consistent procedure. A skill may consist entirely of Markdown; it is not necessarily an executable program.

## Core Principle

**Agent Skills package reusable procedures, allowing agents to follow consistent workflows without permanently loading every instruction into every conversation.**

---

# Part I — Fundamentals

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define Agent Skills and explain their value.
2. Understand a skill directory and the purpose of SKILL.md.
3. Write YAML metadata and distinguish it from workflow instructions.
4. Explain discovery, activation, and progressive disclosure.
5. Organize optional references, scripts, and assets.
6. Differentiate global and project-level skills.
7. Explain skills versus AGENTS.md and MCP.
8. Create and test a minimal instruction-only skill.
9. Identify security risks and avoid duplication or overengineering.
10. Apply skills to Android, Laravel, and Next.js workflows.

---

## 3. What Are Agent Skills?

An **Agent Skill** is a directory containing a required SKILL.md file and optional supporting resources. It teaches an AI agent how to perform a particular kind of task.

Examples: code review, bug investigation, unit-test writing, UI consistency audits, documentation generation, and release checklists.

~~~text
Agent discovers metadata
        |
        v
Selects relevant skill
        |
        v
Reads SKILL.md
        |
        v
Follows workflow and uses available tools
        |
        v
Produces a verifiable result
~~~

A skill describes a workflow. It does not independently grant operating-system permissions, external API access, or unrestricted tool execution.

---

## 4. Why Agent Skills Matter

Without a skill, a developer may repeatedly ask Codex to inspect changed files, check correctness and security, avoid editing, and report actionable findings with paths. A reusable skill stores that procedure once.

| Benefit | Explanation |
|---|---|
| Reusability | Use a procedure across multiple tasks |
| Consistency | Standardize a review or development process |
| Maintainability | Update workflow instructions in one location |
| Discoverability | Help the agent find relevant procedures |
| Context efficiency | Load details when needed |
| Collaboration | Share established practices |

Skills improve workflow consistency but do not guarantee correct output. Verification remains necessary.

---

# Part II — Skill Architecture

## 5. Basic Skill Directory

Minimal:

~~~text
skill-name/
└── SKILL.md
~~~

Optional resources:

~~~text
skill-name/
├── SKILL.md
├── references/
│   └── guidelines.md
├── scripts/
│   └── validate.py
└── assets/
    └── template.md
~~~

| Component | Required? | Purpose |
|---|---|---|
| SKILL.md | Yes | Metadata and workflow |
| references/ | No | Supporting documentation |
| scripts/ | No | Repeatable utilities |
| assets/ | No | Templates and static resources |

Start with SKILL.md only. Add files when they provide a clear benefit.

---

## 6. Understanding SKILL.md

SKILL.md contains YAML frontmatter followed by Markdown instructions.

~~~markdown
---
name: code-review
description: Reviews code changes for correctness, maintainability, and security. Use when asked to review an implementation or Git diff.
---

# Code Review

## Workflow

1. Inspect relevant source files.
2. Read applicable project instructions.
3. Identify concrete correctness issues.
4. Review security and maintainability.
5. Report findings with file references.
6. Do not modify files unless requested.
~~~

The frontmatter describes the skill. The body defines what the agent should do after activation.

---

# Part III — Skill Metadata

## 7. What Is YAML Frontmatter?

YAML frontmatter is a metadata block at the beginning of a Markdown file, enclosed by two lines of three hyphens.

~~~yaml
---
name: code-review
description: Reviews source code and reports actionable findings.
---
~~~

The Agent Skills specification requires a valid name and a nonempty description. Other fields are optional.

---

## 8. The name Field

The name identifies the skill. Specification rules include:

- Maximum 64 characters.
- Lowercase letters, numbers, and hyphens.
- No leading or trailing hyphen.
- No consecutive hyphens.
- Must match the parent skill directory name.

Valid: code-review, android-ui-review, laravel-controller-review, frontend-audit.

Invalid: CodeReview, code review, -code-review, code--review.

Choose a name based on workflow purpose, not a particular model.

---

## 9. The description Field

The description explains what the skill does and when to use it.

Weak:

~~~yaml
description: Helps with coding.
~~~

Better:

~~~yaml
description: Reviews Android XML layouts for constraint issues, alignment inconsistencies, and accessibility concerns. Use when evaluating existing Android UI files.
~~~

A useful formula is: **what the skill does + when it should activate**. A precise description helps the agent select the correct skill without loading every skill's full instructions.

---

## 10. Optional Metadata Fields

| Field | Purpose |
|---|---|
| license | License information |
| compatibility | Environment requirements |
| metadata | Additional custom metadata |
| allowed-tools | Experimental tool permission hints |

Example:

~~~yaml
---
name: android-ui-review
description: Reviews Android XML layouts for usability, accessibility, and layout problems. Use when inspecting existing Android UI screens.
compatibility: Requires an Android project containing XML layout resources.
metadata:
  author: project-team
  version: "1.0"
---
~~~

Optional metadata is not required for basic skills. Metadata values should be strings. allowed-tools support varies between implementations and does not replace runtime security controls.

---

# Part IV — Skill Instructions

## 11. Writing Effective Instructions

A useful structure is:

~~~markdown
# Skill Title
## Purpose
## When to Use
## Workflow
## Constraints
## Validation
## Output Format
~~~

Example:

~~~markdown
# Bug Investigation

## Purpose
Investigate a reported bug before making changes.

## Workflow
1. Read relevant project instructions.
2. Locate affected code.
3. Reproduce the issue when possible.
4. Identify the likely root cause.
5. Propose the smallest correction.
6. Explain how to verify it.

## Constraints
- Do not modify unrelated files.
- Do not add dependencies without justification.
- Do not claim a bug is fixed without verification.

## Output Format
- Observed behavior
- Expected behavior
- Root cause
- Recommended correction
- Verification status
~~~

Instructions should be procedural, specific, and verifiable.

---

## 12. Defining Trigger Conditions

State when the skill applies and, when useful, when it does not.

~~~markdown
## When to Use
Use when reviewing an existing Android XML layout,
investigating UI alignment, or checking accessibility.

Do not use for backend authentication, database design,
or unrelated web components.
~~~

A UI skill should not activate simply because a project contains Android code; the task must involve UI review. Clear boundaries reduce accidental use.

---

## 13. Defining Workflow Boundaries

~~~markdown
## Constraints
- Review only requested files.
- Do not change architecture.
- Do not introduce dependencies.
- Do not modify database schemas.
- Report findings before proposing implementation.
~~~

These guide agent behavior but do not replace sandbox restrictions, permissions, or approval requirements.

---

## 14. Defining Completion Criteria

~~~markdown
## Completion Criteria
The review is complete when:
1. Relevant files have been inspected.
2. Findings are supported by code evidence.
3. Issues and impacts are clearly explained.
4. Recommendations stay within scope.
5. Unverified assumptions are identified.
~~~

Completion criteria make reporting more consistent.

---

# Part V — Skill Discovery and Invocation

## 15. How Skill Discovery Works

Compatible agents can discover skills from configured directories, read metadata, select relevant skills, and load their instructions when needed. The complete contents of every skill do not necessarily need to be loaded immediately.

---

## 16. Progressive Disclosure

**Progressive disclosure** loads information in stages:

1. Metadata: name and description.
2. Instructions: SKILL.md when the skill activates.
3. Supporting resources: references, scripts, or assets when needed.

~~~text
Available skills → metadata → relevant skill
                 → SKILL.md → optional resources
                 → workflow execution
~~~

This reduces unnecessary context and duplication. Keep SKILL.md concise. The Agent Skills specification recommends keeping the main instructions under approximately 500 lines and moving lengthy details to references.

---

## 17. Explicit Invocation

Codex supports explicitly invoking available skills with a pattern such as:

~~~text
$code-review

Review the current changes.
Focus on correctness and maintainability.
Do not modify files.
~~~

The skill must already be available. Typing its name does not install or create it.

---

## 18. Implicit Invocation

An agent may select a relevant skill from the task description. For example, a request to review an Android login layout may match an available android-ui-review skill.

Implicit activation is not guaranteed; it depends on available skills, metadata, agent behavior, and context. Explicitly invoke the skill when a particular workflow is required.

---

# Part VI — Global vs Project-Specific Skills

## 19. Global Skills

Global skills are reusable across projects. A common Codex user-level location is:

~~~text
~/.agents/skills/
~~~

On Windows this commonly resolves to:

~~~text
C:\Users\Acer\.agents\skills\
~~~

Example:

~~~text
.agents/
└── skills/
    ├── code-review/SKILL.md
    ├── bug-investigation/SKILL.md
    └── documentation-review/SKILL.md
~~~

Good candidates include general code review, debugging, documentation checks, diff analysis, and testing procedures.

---

## 20. Project-Specific Skills

Project skills belong to a repository. Example:

~~~text
GroupTrack/
├── AGENTS.md
├── docs/
├── .agents/
│   └── skills/
│       └── android-ui-review/
│           └── SKILL.md
└── app/
~~~

Use these for repository-specific review and testing procedures or workflows tied to local conventions. Discovery paths can change; verify current Codex documentation and the installed environment.

---

## 21. Global vs Project-Specific Comparison

| Aspect | Global Skill | Project Skill |
|---|---|---|
| Scope | Multiple projects | One repository |
| Typical location | User skill directory | Repository .agents/skills/ |
| Reusability | High | Project-focused |
| Shared through repository | Usually no | Yes, if committed |
| Best use | Generic procedures | Repository-specific workflows |
| Maintenance | User-level | Project/team-level |

A generic code-review skill can be global. A GroupTrack XML review based on project design conventions may be repository-specific.

---

# Part VII — Windows PowerShell Setup

## 22. Creating a Global Skill Directory

~~~powershell
$skillsRoot = Join-Path $HOME ".agents\skills"
New-Item -ItemType Directory -Path $skillsRoot -Force

$skillPath = Join-Path $skillsRoot "code-review"
New-Item -ItemType Directory -Path $skillPath -Force
New-Item -ItemType File -Path (Join-Path $skillPath "SKILL.md") -Force

code (Join-Path $skillPath "SKILL.md")
~~~

These commands create files only. They do not install dependencies or execute scripts.

---

## 23. Creating a Project Skill

~~~powershell
Set-Location "C:\Users\Acer\AndroidStudioProjects\GroupTrack"
New-Item -ItemType Directory -Path ".agents\skills\android-ui-review" -Force
New-Item -ItemType File -Path ".agents\skills\android-ui-review\SKILL.md" -Force
~~~

Check for an equivalent skill first to avoid duplicate workflow definitions.

---

# Part VIII — Complete Example Skill

## 24. Example: Code Review Skill

Suggested file: .agents/skills/code-review/SKILL.md

~~~markdown
---
name: code-review
description: Reviews existing source code and Git changes for correctness, security, and maintainability. Use when the user requests a code review or implementation audit.
---

# Code Review

## Purpose
Provide a focused, evidence-based review of existing code.

## Workflow
1. Read applicable AGENTS.md instructions.
2. Identify the requested scope.
3. Inspect relevant source files and changes.
4. Check correctness and edge cases.
5. Review validation and authorization where relevant.
6. Check maintainability and architectural consistency.
7. Identify missing or inadequate tests.
8. Report findings by severity.

## Constraints
- Do not modify files.
- Do not introduce dependencies.
- Do not report speculation as confirmed defects.
- Do not expand beyond the requested scope.
- Do not claim tests ran unless they did.

## Output Format
For each finding, include severity, file and location,
problem, impact, and recommended correction.
If no actionable issues are found, say so.
Mention verification limitations.
~~~

This skill has a clear purpose, activation conditions, procedure, constraints, and output requirements.

---

# Part IX — Supporting Files

## 25. The references Directory

Use references for supporting documentation.

~~~text
code-review/
├── SKILL.md
└── references/review-checklist.md
~~~

Example content:

~~~markdown
# Review Checklist
## Correctness
- Input validation
- Null handling
- Error handling
- Edge cases
## Security
- Authentication
- Authorization
- Sensitive data handling
## Maintainability
- Clear responsibilities
- Minimal duplication
- Consistent naming
~~~

SKILL.md can tell the agent to read the checklist when needed. Keep essential workflow instructions in SKILL.md.

---

## 26. The scripts Directory

Scripts can support repeated validation, format checking, deterministic transformations, or report generation.

~~~text
code-review/
├── SKILL.md
└── scripts/check_changed_files.py
~~~

Scripts are executable code and must be reviewed before execution. Their presence does not imply authorization. For a simple review, an instruction-only skill may be enough.

---

## 27. The assets Directory

Assets can hold reusable templates and static resources.

~~~text
code-review/
├── SKILL.md
└── assets/review-template.md
~~~

Example:

~~~markdown
# Code Review Report
## Scope
## Findings
## Recommended Fixes
## Tests and Verification
## Remaining Risks
~~~

Add templates only when consistent output materially improves the workflow.

---

# Part X — Skills vs AGENTS.md vs MCP

## 28. Understanding the Differences

| Component | Main Responsibility |
|---|---|
| AGENTS.md | Persistent repository instructions |
| Agent Skill | Reusable task-specific workflow |
| MCP | Connection to external capabilities |

For an Android screen review, AGENTS.md may describe architecture and coding constraints; a skill may provide a checklist for XML alignment and accessibility; an optional MCP server may connect to an approved external reference service.

~~~text
Developer request → Codex
                  ├── AGENTS.md → project rules
                  ├── Skill → task workflow
                  └── MCP → optional external capability
~~~

---

## 29. When to Use AGENTS.md

Use AGENTS.md for stable repository-wide guidance: technology, architecture, coding standards, scope, testing expectations, and dependency restrictions. Avoid putting every workflow in it.

---

## 30. When to Use Agent Skills

Use skills for repeatable procedures such as feature review, bug investigation, UI audits, test reports, or migration reviews. A one-time prompt is often enough for a one-off task.

---

## 31. When to Use MCP

Use MCP when a workflow needs capabilities from an external system, such as approved documentation search, issue-tracker access, project metadata, or external development tools. A skill can use MCP tools but does not require MCP.

---

# Part XI — Practical Project Examples

## 32. GroupTrack — Android UI Review Skill

GroupTrack uses Android Studio, Java, XML layouts, Firebase, and an established MVP scope.

Suggested location:

~~~text
GroupTrack/.agents/skills/android-ui-review/SKILL.md
~~~

Example:

~~~markdown
---
name: android-ui-review
description: Reviews Android XML layouts for alignment, constraints, usability, and accessibility. Use when inspecting existing GroupTrack UI screens.
---

# Android UI Review

## Workflow
1. Read AGENTS.md and relevant UI documentation.
2. Identify the requested screen.
3. Inspect its XML layout.
4. Check constraints, alignment, spacing, and readability.
5. Check accessibility labels where applicable.
6. Check consistency with existing resources.
7. Report findings.

## Constraints
- Do not add UI libraries.
- Do not modify business logic.
- Do not redesign unrelated screens.
- Follow the documented MVP scope.
- Do not modify files unless requested.

## Output
- Screen reviewed
- Layout issues
- Accessibility observations
- Recommended minimal fixes
- Verification limitations
~~~

Invocation:

~~~text
$android-ui-review
Review the current sign-in layout.
Focus on constraints, alignment, and existing resource consistency.
Do not modify files.
~~~

The workflow can be reused for welcome, sign-in, and account creation screens.

---

## 33. GoPasig — Laravel Controller Review Skill

GoPasig uses Laravel for its fleet-management backend.

Suggested location: GoPasig/.agents/skills/laravel-controller-review/SKILL.md

~~~markdown
---
name: laravel-controller-review
description: Reviews Laravel controllers for validation, authorization, query efficiency, and architectural consistency. Use when auditing an existing controller.
---

# Laravel Controller Review

## Workflow
1. Read project architecture documentation.
2. Inspect the requested controller and related services/models.
3. Check validation and authorization.
4. Identify possible query inefficiencies.
5. Check transaction boundaries where relevant.
6. Review error handling.
7. Report actionable findings.

## Constraints
- Do not modify database records.
- Do not change business rules.
- Do not introduce packages.
- Do not refactor unrelated modules.
- Do not claim performance problems without evidence.

## Output
- Controller reviewed
- Confirmed findings
- Potential risks
- Minimal recommendations
- Verification requirements
~~~

Invocation:

~~~text
$laravel-controller-review
Review the ScheduleController for validation, authorization,
and separation of concerns. Do not modify files.
~~~

---

## 34. E-commerce — Next.js Frontend Review Skill

For a Next.js, TypeScript, and Tailwind CSS frontend:

Suggested location: frontend/.agents/skills/nextjs-ui-review/SKILL.md

~~~markdown
---
name: nextjs-ui-review
description: Reviews Next.js TypeScript components for accessibility, responsiveness, maintainability, and consistency. Use when auditing existing frontend UI.
---

# Next.js UI Review

## Workflow
1. Read applicable project instructions.
2. Identify the requested component or page.
3. Inspect the implementation.
4. Check responsive behavior and semantic HTML.
5. Review accessibility and component responsibilities.
6. Identify unnecessary duplication.
7. Report minimal improvements.

## Constraints
- Do not modify Laravel backend code.
- Do not add UI libraries unnecessarily.
- Do not redesign unrelated pages.
- Do not change API contracts without approval.
- Do not modify files unless requested.

## Output
- Components reviewed
- UI issues
- Accessibility findings
- Recommended improvements
- Verification limitations
~~~

Invocation:

~~~text
$nextjs-ui-review
Review the admin dashboard for responsiveness and accessibility.
Do not modify backend files.
~~~

---

# Part XII — Skill Security

## 35. Skills Are Instructions, Not Security Boundaries

A skill can instruct the agent not to modify production data, but cannot enforce operating-system permissions. Actual protection must also come from authentication, authorization, and infrastructure permissions.

**Use runtime controls to enforce security; use skills to guide workflows.**

---

## 36. Reviewing Third-Party Skills

Before use, inspect SKILL.md, references, scripts, dependencies, network needs, filesystem access, credential requirements, and expected outputs.

Risks include malicious instructions, unauthorized edits, data exposure, unnecessary command execution, prompt injection, and hidden dependencies. Treat third-party skills as untrusted until reviewed.

---

## 37. Safe Script Execution

When a skill includes scripts:

1. Read and understand the script.
2. Check dependencies and accessed files.
3. Check network behavior.
4. Confirm required permissions.
5. Run only approved operations.
6. Verify results.

A skill's presence does not authorize destructive commands.

---

## 38. Avoiding Credential Exposure

Never store API keys, database passwords, tokens, or private credentials in SKILL.md or supporting files. Use supported environment and credential-management mechanisms, and grant only the permissions needed.

---

# Part XIII — Common Mistakes

## 39. Mistake: Vague Skill Description

Weak: “Helps with development.”

Better: “Reviews Android XML layouts for constraint and accessibility issues. Use when auditing existing Android screens.”

A description should identify a workflow and its trigger.

---

## 40. Mistake: Putting Everything in One Skill

A skill spanning Android UI, Laravel, database design, Git, docs, testing, and deployment becomes broad and hard to maintain. Prefer focused workflows that are genuinely reusable.

---

## 41. Mistake: Duplicating AGENTS.md

Repeating all project architecture and standards creates competing instruction sources. Reference project docs and keep the skill focused on its procedure.

---

## 42. Mistake: Excessive Instructions

Hundreds of unnecessary rules increase context use, maintenance, and potential conflicts. Keep the skill concise; move detailed support material to references.

---

## 43. Mistake: Unnecessary Scripts

Do not create a complex framework for a simple checklist. Start instruction-only; add scripts when deterministic automation provides measurable value.

---

## 44. Mistake: Assuming Automatic Invocation

A skill is not guaranteed to be selected every time. Verify discovery, test explicit invocation, and improve metadata if selection is inconsistent.

---

## 45. Mistake: No Validation

An agent response is not proof. Require evidence, tests, and verification appropriate to the task.

---

## 46. Mistake: Treating Skills as Permissions

Instructions cannot grant unrestricted access. Respect actual sandbox, authorization, and approval settings.

---

# Part XIV — Testing and Improving Skills

## 47. Skill Testing Workflow

~~~text
Create → Validate metadata → Verify discovery → Test explicit invocation
       → Test relevant task → Test unrelated task → Review → Refine
~~~

Test unrelated tasks too. A skill should not activate when irrelevant; correct non-activation is part of good behavior.

---

## 48. Example Evaluation Matrix

| Test | Expected Result |
|---|---|
| Review Android XML layout | Android UI skill is relevant |
| Review Laravel controller | Android UI skill is not relevant |
| Review Next.js component | Next.js UI skill is relevant |
| Request database migration | UI review skill is not relevant |
| Explicitly invoke code-review | Skill instructions are followed |
| Ask for one-time explanation | Unnecessary skills are avoided |

Evaluate activation, scope, output consistency, tool use, file modifications, and verification reporting.

---

## 49. Iterative Improvement

If a skill performs poorly, identify the cause: vague description, conflicting instructions, missing step, excessive scope, unclear output format, or missing references. Change one meaningful aspect at a time and retest with representative prompts.

**Treat skills as maintainable artifacts, not permanent instructions that never need revision.**

---

# Part XV — Practical Exercises

## 50. Exercise A — Identify Skill Components

Given:

~~~text
bug-investigation/
├── SKILL.md
├── references/debugging-guide.md
└── scripts/check_logs.py
~~~

Answer:

1. Which file is required?
2. Which directory contains supporting documentation?
3. Which directory contains executable utilities?
4. Can the skill exist without the script?
5. What information belongs in frontmatter?

---

## 51. Exercise B — Write Skill Metadata

Design metadata for a skill that reviews Laravel validation logic.

~~~yaml
---
name: laravel-validation-review
description: YOUR_DESCRIPTION
---
~~~

The description must explain what it reviews and when to activate.

---

## 52. Exercise C — Design a Minimal Skill

Create an instruction-only code-review skill with purpose, trigger, workflow, constraints, and output format. Do not add scripts, dependencies, or integrations unless necessary.

---

## 53. Exercise D — Create a Skill in PowerShell

~~~powershell
$root = Join-Path $HOME ".agents\skills"
New-Item -ItemType Directory -Path (Join-Path $root "code-review") -Force
New-Item -ItemType File -Path (Join-Path $root "code-review\SKILL.md") -Force
~~~

Deliver a valid SKILL.md with metadata and a focused review procedure.

---

## 54. Exercise E — Test Skill Activation

~~~text
$code-review
Review the current Git changes.
Do not modify files.
Report actionable findings only.
~~~

Observe whether the skill is recognized, its instructions are followed, scope is maintained, and the output format is correct. The skill must be available in the active environment.

---

## 55. Exercise F — Compare Instruction Types

Classify each requirement as AGENTS.md, a skill, or MCP.

| Requirement | Classification |
|---|---|
| Always follow the project's MVC architecture | |
| Review changed controllers using a checklist | |
| Search an approved external documentation service | |
| Do not add unnecessary dependencies | |
| Generate a structured code review report | |
| Retrieve authorized issue-tracking data | |

---

## 56. Exercise G — Design a GroupTrack Skill

Create a Java/XML UI review skill that reads project documentation, avoids unnecessary dependencies and unrelated edits, and produces a clear report.

Deliverable:

~~~text
android-ui-review/
└── SKILL.md
~~~

---

## 57. Exercise H — Skill Security Audit

- [ ] Skill source is trusted.
- [ ] SKILL.md and supporting files are reviewed.
- [ ] Scripts are understood.
- [ ] Dependencies are justified.
- [ ] No credentials are embedded.
- [ ] Network needs are documented.
- [ ] Filesystem access is limited appropriately.
- [ ] Workflow respects project instructions.
- [ ] Consequential actions require appropriate approval.
- [ ] Outputs can be verified.

---

# Part XVI — Knowledge Review

## 58. Knowledge Check

1. What is an Agent Skill and what problem does it solve?
2. What file is required?
3. What is YAML frontmatter and which fields are required?
4. Why do name and description matter?
5. What makes an effective description?
6. What is progressive disclosure?
7. What are references, scripts, and assets used for?
8. What are explicit and implicit invocation?
9. Is implicit invocation guaranteed?
10. What is the difference between global and project skills?
11. How do skills differ from AGENTS.md and MCP?
12. Can skills grant permissions or require MCP?
13. Why review third-party skills and avoid embedded credentials?
14. When is a script unnecessary?
15. How should a skill be tested, including unrelated tasks?
16. What is skill overengineering?
17. When is a normal prompt preferable?
18. How can skills improve consistency?

---

## 59. Key Terminologies

| Term | Definition |
|---|---|
| Agent Skill | Reusable package of workflow instructions |
| SKILL.md | Required skill manifest and instruction file |
| YAML Frontmatter | Metadata block at the beginning of a Markdown file |
| Skill Name | Identifier of the skill |
| Skill Description | Metadata explaining what and when |
| Skill Discovery | Process of identifying available skills |
| Skill Activation | Loading and applying a skill |
| Explicit Invocation | Directly requesting a particular skill |
| Implicit Invocation | Agent selecting a skill based on relevance |
| Progressive Disclosure | Loading information only when needed |
| Global Skill | Skill reusable across projects |
| Project Skill | Skill scoped to a repository |
| Reference File | Supporting documentation |
| Script | Executable supporting utility |
| Asset | Reusable static resource |
| Workflow | Repeatable sequence of task steps |
| Completion Criteria | Conditions defining a completed task |
| Skill Evaluation | Testing whether a skill behaves as intended |
| Least Privilege | Limiting permissions to what is necessary |
| Overengineering | Adding unnecessary complexity |

---

# 60. Key Takeaways

1. Agent Skills provide reusable task-specific workflows.
2. Every skill requires SKILL.md.
3. Frontmatter identifies the skill and describes when it applies.
4. Instructions define the procedure after activation.
5. Skills can be instruction-only; supporting files are optional.
6. Progressive disclosure reduces unnecessary context.
7. Explicit invocation is useful when a workflow must be selected.
8. Implicit invocation depends on relevance and agent behavior.
9. Global skills suit general procedures; project skills suit repository workflows.
10. AGENTS.md defines persistent project instructions; skills define task procedures.
11. MCP provides external capabilities; skills can complement MCP.
12. Skills do not replace runtime security controls.
13. Review third-party skills and never embed credentials.
14. Test both activation and non-activation.
15. Add scripts only when they provide clear value.
16. Not every repeated prompt needs a skill.
17. The best skill is the smallest reusable workflow that reliably solves its intended problem.

---

# 61. Official References

## OpenAI Codex

1. [Build Skills — Codex Documentation](https://learn.chatgpt.com/codex/build-skills)
2. [Codex Documentation](https://learn.chatgpt.com/)
3. [OpenAI Skills Guide](https://developers.openai.com/api/docs/guides/tools-skills)
4. [Testing Agent Skills Systematically](https://developers.openai.com/blog/eval-skills)
5. [Rethinking Skills and Prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
6. [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)

## Agent Skills Standard

7. [Agent Skills Official Website](https://agentskills.io/)
8. [Agent Skills Specification](https://agentskills.io/specification)
9. [Agent Skills GitHub](https://github.com/agentskills/agentskills)

## Skills and MCP

10. [OpenAI Plugin Skills Concepts](https://developers.openai.com/plugins/concepts/skills)
11. [OpenAI Plugin Architecture](https://developers.openai.com/plugins/concepts/plugins)
12. [Model Context Protocol](https://modelcontextprotocol.io/)

> **Documentation Note:** The Agent Skills specification defines the portable format. Codex-specific discovery paths, skill creation workflows, and invocation behavior should be verified against the installed version and current documentation.

---

# 62. Lesson Completion Checklist

- [ ] Understand Agent Skills and their value.
- [ ] Understand skill architecture and SKILL.md.
- [ ] Understand YAML frontmatter and required metadata.
- [ ] Write valid names and effective descriptions.
- [ ] Write instructions, triggers, boundaries, and completion criteria.
- [ ] Understand discovery, invocation, and progressive disclosure.
- [ ] Understand global and project-specific skills.
- [ ] Create a minimal skill in PowerShell.
- [ ] Understand references, scripts, and assets.
- [ ] Differentiate skills from AGENTS.md and MCP.
- [ ] Understand skill security and third-party review.
- [ ] Identify common mistakes and test skills iteratively.
- [ ] Apply concepts to Android, Laravel, and Next.js.
- [ ] Complete the exercises and knowledge check.

---

# 63. Next Lesson

## [[04 - Multi-Agent Collaboration and Task Delegation]]

The next lesson explores collaboration between multiple AI agents:

- Multi-agent architecture.
- Main agent and subagents.
- Task delegation.
- Parallel versus sequential execution.
- Context isolation.
- Agent responsibilities.
- Shared-file coordination.
- Integration and verification.
- Cost and performance tradeoffs.
- Preventing unnecessary agent proliferation.
- Android, Laravel, and Next.js examples.

---

*Phase 5 — Advanced Codex | Lesson 5.3*
*Learning Principle: Package repeatable procedures into focused skills, keep project rules separate, and add complexity only when it improves the workflow.*
