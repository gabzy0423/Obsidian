# 00 — Superpowers: Repository Overview

> [!abstract] Lesson Information
> **Repository:** [obra/superpowers](https://github.com/obra/superpowers)
>
> **Author:** Jesse Vincent (obra), with contributors
>
> **Category:** Agentic Engineering / AI Coding Skills / Software Development Methodology
>
> **Difficulty:** Beginner to Intermediate
>
> **Learning Approach:** Theory First
>
> **Prerequisites:** Basic Git, software development lifecycle, and familiarity with AI coding agents
>
> **Primary Goal:** Understand what Superpowers is, why it exists, how its development methodology works, and how its skills improve AI-assisted software engineering.

---

# 1. Introduction

## 1.1 What Is Superpowers?

**Superpowers** is an open-source agentic skills framework and software development methodology for AI coding agents. It helps an agent follow a structured engineering process instead of immediately generating code after receiving a request.

The repository provides reusable skills for:

- Understanding requirements and brainstorming solutions.
- Designing software and writing implementation plans.
- Creating isolated development environments.
- Implementing functionality with tests.
- Debugging, reviewing, and verifying changes.
- Completing development branches.

Superpowers is not a programming language, application framework, or replacement for an AI coding agent. It is a **workflow and instruction layer** that operates through supported coding agents.

Without a structured methodology:

    Developer Request → AI Generates Code → Developer Checks Output

With Superpowers:

    Request → Understand → Brainstorm and Design → Approve
    → Plan → Implement with Tests → Review → Verify → Complete

The difference is the emphasis on engineering discipline before and during implementation.

---

# 2. Why Was Superpowers Created?

## 2.1 The Problem with Unstructured AI Coding

AI coding agents can generate code quickly, but speed alone does not ensure quality. Without enough guidance, an agent may misunderstand requirements, code before designing, add unrequested features, modify unrelated files, skip useful tests, patch symptoms instead of root causes, claim success without verification, or lose track of a multi-step plan.

For a request such as “Create a task management feature,” important questions remain: Who can create tasks? Can they be assigned? Which statuses are required? Are notifications in scope? Superpowers encourages resolving such questions before implementation.

## 2.2 The Superpowers Solution

Superpowers has specialized skills for different development activities.

| Development problem | Example skill |
|---|---|
| Requirements are unclear | brainstorming |
| No implementation plan | writing-plans |
| Coding without tests | test-driven-development |
| Guesswork during debugging | systematic-debugging |
| Unverified completion | verification-before-completion |
| Unreviewed changes | requesting-code-review |
| Complex implementation | subagent-driven-development |
| Isolated branch coordination | using-git-worktrees |

Each skill addresses part of the workflow instead of relying on one broad instruction such as “write good code.”

---

# 3. Core Philosophy

Superpowers is built around four engineering principles.

## 3.1 Test-Driven Development

Write a test that demonstrates required behavior before implementing production code. The cycle is **RED → GREEN → REFACTOR**:

- **RED:** Write a test and confirm it fails for the expected reason.
- **GREEN:** Write the minimum production code needed to pass.
- **REFACTOR:** Improve the code while keeping tests passing.

## 3.2 Systematic over Ad Hoc

Use a repeatable investigation process rather than guessing, especially when debugging. Gather evidence, find the root cause, and verify the fix.

## 3.3 Complexity Reduction

Avoid unnecessary features, abstractions, and dependencies. Superpowers explicitly draws on YAGNI (You Aren’t Gonna Need It), DRY (Don’t Repeat Yourself), and simple, maintainable implementations.

## 3.4 Evidence over Claims

An agent should not declare a task complete merely because it believes the code is correct. Use evidence such as passing tests, successful builds, verified behavior, and reviewed changes.

> [!important] Engineering Principle
> A convincing explanation is not proof that software behaves correctly. Verification provides evidence that the implementation meets its requirements.

---

# 4. How Superpowers Works

## 4.1 Skills-Based Architecture

A skill is a reusable set of instructions for a particular activity. The repository separates responsibilities into individual skills instead of placing every rule in one enormous prompt.

    Developer Request → Coding Agent ← Superpowers Integration
                                      └─ Skill Library
                                         ├─ Brainstorming
                                         ├─ Planning
                                         ├─ Implementation
                                         ├─ Testing
                                         └─ Review

The host agent executes development operations using its tools. Superpowers supplies the methodology.

## 4.2 Composable Skills

Skills can be combined into a larger workflow:

    brainstorming → writing-plans → test-driven-development
                   → requesting-code-review
                   → verification-before-completion

Each skill has its own responsibility. Together, they support a more complete development process.

## 4.3 Skill Activation

The official repository describes instructions that make the agent check for relevant skills before tasks. A compatible host can make this part of the normal workflow. Discovery, activation, and tool support still depend on the agent and its integration; installing the repository does not guarantee identical behavior in every host.

---

# 5. The Official Development Workflow

## 5.1 Workflow Overview

    Development request → Brainstorming → Design approval
    → Git worktree → Implementation plan
    → Subagent-driven development or executing-plans
    → Test-driven development → Code review
    → Verification → Finish development branch

This is a learning diagram. Exact activation and review timing depend on the execution approach and host.

## 5.2 Stage 1 — Brainstorming

**Skill:** brainstorming

Clarify what the developer wants through questions about requirements, users, constraints, expected behavior, and design decisions. The output is an agreed design. Small changes may be agreed in conversation; larger projects may produce a written design document.

Example: before coding a student task tracker, clarify the user roles, task lifecycle, assignment rules, and which features are out of scope.

## 5.3 Stage 2 — Git Worktrees

**Skill:** using-git-worktrees

A Git worktree creates another working directory attached to the same repository, often on a separate branch. It can isolate feature work, reduce interference between tasks, and make review easier. Superpowers describes checking a clean baseline and setting up the project before implementation.

    Repository
      ├── Main working directory
      ├── Feature development worktree
      └── Another isolated worktree

## 5.4 Stage 3 — Implementation Planning

**Skill:** writing-plans

Convert the approved design into small, specific tasks. The README describes tasks sized around 2–5 minutes, with exact file paths, implementation instructions, and verification steps.

Example plan:

1. Create the task model.
2. Add task validation.
3. Implement task creation.
4. Add the task list UI.
5. Add regression tests.
6. Verify behavior.

The breakdown should fit the real project; the aim is manageable, reviewable work.

## 5.5 Stage 4 — Plan Execution

The README describes two execution approaches:

| Aspect | Subagent-driven development | Executing plans |
|---|---|---|
| Execution | Fresh subagent per task | Current session works through the plan |
| Review | Each task receives review | Whole branch reviewed at the end |
| Context separation | Greater | Less |
| Coordination overhead | Higher | Lower |
| Typical fit | Complex, separable work | Smaller or sequential plans |

The README presents subagent-driven development as the thorough path and executing-plans as a lower-cost alternative. Choose based on host support, task complexity, and resources.

## 5.6 Stage 5 — Test-Driven Development

**Skill:** test-driven-development

Apply RED-GREEN-REFACTOR: write a failing test, observe the expected failure, add minimal code, observe it pass, then refactor while tests remain green. Repeat for the next behavior.

Illustrative JavaScript example:

    const assert = require("node:assert/strict");
    assert.equal(isCompleted({ status: "completed" }), true);

The test should fail before the function exists. A minimal implementation might be:

    function isCompleted(task) {
      return task.status === "completed";
    }

This demonstrates the sequence, not a complete project test setup.

## 5.7 Stage 6 — Code Review

**Skills:** requesting-code-review and receiving-code-review

Review can check plan compliance, correctness, maintainability, tests, security, and unnecessary complexity. Evaluate findings rather than blindly applying every suggestion. The workflow can review between tasks or at the end depending on the execution approach.

## 5.8 Stage 7 — Finish Development Branch

**Skill:** finishing-a-development-branch

After implementation and verification, the workflow offers outcomes such as merge, create a pull request, keep the branch, or discard the work. Worktree cleanup depends on the chosen outcome. Review destructive actions and obtain appropriate authorization.

---

# 6. Actual Repository Structure

The following is a simplified view of top-level files and folders in the official repository:

    superpowers/
    ├── .agents/plugins/
    ├── .antigravity-plugin/
    ├── .claude-plugin/
    ├── .codex-plugin/
    ├── .cursor-plugin/
    ├── .devin-plugin/
    ├── .hermes-plugin/
    ├── .kimi-plugin/
    ├── .muse-plugin/
    ├── .opencode/
    ├── .pi/extensions/
    ├── assets/
    ├── docs/
    ├── hooks/
    ├── scripts/
    ├── skills/
    ├── tests/
    ├── AGENTS.md
    ├── GEMINI.md
    ├── README.md
    ├── RELEASE-NOTES.md
    ├── LICENSE
    ├── package.json
    └── index.js

| Component | Purpose |
|---|---|
| skills/ | Reusable engineering skills |
| docs/ | Supporting documentation |
| hooks/ | Lifecycle integration components |
| scripts/ | Supporting executable scripts |
| tests/ | Plugin and integration tests |
| .codex-plugin/ | Codex-specific plugin integration |
| .claude-plugin/ | Claude Code integration |
| .cursor-plugin/ | Cursor integration |
| .opencode/ | OpenCode integration |

Important files include README.md (project introduction), AGENTS.md (repository instructions for coding agents), LICENSE (licensing terms), RELEASE-NOTES.md (release changes), and package.json (Node.js package metadata and configuration).

> [!note] Repository vs Application
> This is the Superpowers project structure, not the architecture of an app built with Superpowers. Your Android, Laravel, or Next.js project keeps its own architecture.

---

# 7. Superpowers Skills Library

The README groups documented skills by purpose.

| Category | Skill | Purpose |
|---|---|---|
| Testing | test-driven-development | RED-GREEN-REFACTOR development |
| Debugging | systematic-debugging | Structured root-cause investigation |
| Debugging | verification-before-completion | Verify before reporting success |
| Debugging | diagnosing-superpowers | Diagnose unexpected behavior in a session |
| Collaboration | brainstorming | Clarify intent before building |
| Collaboration | writing-plans | Create implementation plans |
| Collaboration | executing-plans | Execute a plan in the current session |
| Collaboration | dispatching-parallel-agents | Coordinate independent concurrent work |
| Collaboration | requesting-code-review | Request review |
| Collaboration | receiving-code-review | Evaluate and respond to feedback |
| Collaboration | using-git-worktrees | Isolate branch work |
| Collaboration | finishing-a-development-branch | Choose how to complete branch work |
| Collaboration | subagent-driven-development | Delegate tasks with staged review |
| Meta | using-superpowers | Introduce the skills system |
| Meta | writing-skills | Create and test reusable skills |

> [!note] Skill Availability
> These names are documented by the repository. Actual invocation, automatic activation, and subagent support depend on the host integration.

---

# 8. Supported Coding Agents

The official README describes integrations for Claude Code, Codex App and CLI, Cursor, Antigravity, Devin CLI, Factory Droid, Gemini CLI, GitHub Copilot CLI, Grok Build CLI, Kimi Code, OpenCode, Pi, Qwen Code, Hermes Agent, and Muse.

A coding-agent host is the application or runtime that executes the agent. It may provide file and terminal access, Git operations, skill discovery, subagents, permissions, and session management. Superpowers supplies workflow instructions through host-specific integration mechanisms.

**Supported integration does not mean identical behavior across hosts.** A workflow relying on subagents may act differently when a host has limited delegation support.

---

# 9. Superpowers and Codex CLI

## 9.1 Installation Overview

The current README documents installing through the Codex plugin interface:

1. Open the plugin search interface with /plugins.
2. Search for superpowers.
3. Select Install Plugin.

The Codex App also offers the plugin in its Plugins panel. Installation and verification belong in [[02 - Installation and Setup]].

## 9.2 Conceptual Codex Integration

    Developer → Codex CLI ← Superpowers plugin
                      ↑
                Project AGENTS.md
                      ↓
                Skill selection
                      ↓
              Development tools
              ├── Read and edit files
              ├── Run tests
              └── Git operations
                      ↓
              Verified implementation

| Codex CLI | Superpowers |
|---|---|
| Coding-agent runtime | Skills and methodology |
| Reads and modifies code | Guides how the agent should work |
| Executes available commands | Defines recommended workflows |
| Uses host tools | Coordinates skill-based practices |
| Enforces configured permissions | Does not replace host security controls |

Project AGENTS.md instructions may define architecture, scope, technology, coding standards, and testing requirements. Superpowers should complement these instructions, not override legitimate project constraints.

---

# 10. Superpowers vs Ponytail

Both projects aim to improve AI-assisted software engineering, with different emphases.

| Aspect | Ponytail | Superpowers |
|---|---|---|
| Primary emphasis | Avoid unnecessary complexity | Structure the development lifecycle |
| Core approach | Simplicity-oriented engineering guidance | Composable workflow skills |
| Planning | Encourages focused implementation | Dedicated brainstorming and planning skills |
| Testing | Appropriate verification | Explicit test-first workflow |
| Debugging | Review and audit practices | Dedicated systematic debugging skill |
| Subagents | Host-dependent guidance | Explicit subagent development workflow |
| Git worktrees | Not a central feature | Dedicated worktree skill |
| Review | Review and audit workflows | Review integrated into development |

For “Add a task status filter,” Ponytail emphasizes the smallest complete solution, existing code, and avoiding unneeded dependencies. Superpowers emphasizes clarifying behavior, agreeing on a design, planning, testing, reviewing, and verifying. Their approaches can complement each other, but combining integrations may duplicate or conflict in instructions; test compatibility.

Use Ponytail when excess complexity is the main concern. Use Superpowers when lifecycle consistency is the main concern. A full multi-agent workflow may add overhead for a tiny change; planning and review may pay off on complex work.

---

# 11. Practical Application: Android

## Scenario — Task Status Feature

A Java Android app needs a task status indicator.

**Brainstorm:** Which statuses exist? Where should they appear? Is status editable? Which UI patterns already exist?

**Plan:** Identify relevant Java classes, XML layouts, data models, and tests.

**Implement:** Reuse the app's existing architecture.

**Verify:** Build and test the application, then inspect the screen.

Example commands, if configured:

    .\\gradlew.bat assembleDebug
    .\\gradlew.bat testDebugUnitTest

---

# 12. Practical Application: Laravel

## Scenario — Schedule Filtering

A Laravel application needs schedules filtered by route.

**Brainstorm:** Which users may access the endpoint? Which filters and pagination rules apply? What business rules already exist?

**Plan:** Locate routes, controllers, models, services, policies, and feature tests.

**Implement:** Follow the current application architecture without unnecessary layers.

**Verify:** Run the relevant test suite and inspect authorization and database queries.

Example command:

    php artisan test

---

# 13. Practical Application: Next.js

## Scenario — Product Search

A Next.js application needs product search.

**Brainstorm:** Is search client-side or server-side? How large is the dataset? Which fields are searchable? Is pagination or URL state required?

**Plan:** Identify existing components, data fetching, TypeScript types, API integration, and tests.

**Implement:** Reuse project components and dependencies.

**Verify:** Run the configured build and lint scripts.

    npm run build
    npm run lint

---

# 14. Strengths of Superpowers

- **Structured development:** Repeatable process rather than improvised coding.
- **Requirement discovery:** Clarifies the problem before implementation.
- **Explicit planning:** Breaks large work into manageable tasks.
- **Testing discipline:** Promotes test-driven development.
- **Systematic debugging:** Focuses on evidence and root causes.
- **Integrated review:** Makes review part of development.
- **Multi-agent collaboration:** Supports delegation and independent review.
- **Multi-host support:** Works across several coding-agent environments.

---

# 15. Limitations and Tradeoffs

- **Workflow overhead:** Brainstorming, planning, tests, and review take time; tiny changes may not need every stage.
- **Token use:** Plans, subagents, and repeated reviews can consume more tokens.
- **Host dependencies:** Some skills rely on subagents, discovery, or Git tooling.
- **Test infrastructure:** TDD needs a usable test environment.
- **Agent reliability:** A structured process does not guarantee correct code.
- **Developer responsibility:** People still approve design, review changes, protect credentials, and verify sensitive behavior.

> [!warning] Important
> A methodology can improve the process, but it cannot replace engineering judgment.

---

# 16. Security Considerations

Review third-party agent plugins before use. Pay attention to executable hooks, shell scripts, filesystem permissions, network requests, instructions embedded in repository content, and tool permissions.

- [ ] Verify the repository source.
- [ ] Inspect plugin manifests and executable hooks.
- [ ] Preserve Codex sandbox and approval settings.
- [ ] Avoid exposing credentials.
- [ ] Use Git to track changes.
- [ ] Test unfamiliar integrations in a disposable project.
- [ ] Review changes before committing.

Superpowers provides development instructions; it is not a security boundary.

---

# 17. Practical Exercises

## Exercise 1 — Explain the Repository

Answer: What is Superpowers? What problem does it solve? Is it a programming framework? How does it interact with coding agents? Why does it use multiple skills?

**Expected outcome:** Explain the repository's purpose.

## Exercise 2 — Identify Skills

Match each problem with a skill:

| Problem | Skill |
|---|---|
| Requirements are unclear | ? |
| Bug root cause is unknown | ? |
| Feature needs an implementation plan | ? |
| Tests should be written before code | ? |
| Branch is ready for completion | ? |
| Session behaves unexpectedly | ? |

**Expected outcome:** Recognize major skill responsibilities.

## Exercise 3 — Design a Workflow

For “Build a task management module,” describe requirements, design, planning, implementation, testing, review, verification, and branch completion.

**Expected outcome:** Understand the development lifecycle.

## Exercise 4 — Compare Ponytail and Superpowers

Create an Obsidian comparison table for simplicity, planning, testing, debugging, review, multi-agent work, token overhead, and suitability for a small MVP.

**Expected outcome:** Explain how the methodologies differ.

## Exercise 5 — Plan an Existing Project Feature

Choose a small Android, Laravel, or Next.js feature. Without changing code, write the requirement, stakeholders, acceptance criteria, relevant files, implementation plan, and verification steps.

**Expected outcome:** Practice planning before implementation.

---

# 18. Knowledge Check

1. What is the primary purpose of Superpowers?
2. What does composable mean for skills?
3. Why brainstorm before implementation?
4. What is design approval?
5. What does writing-plans produce?
6. What is the purpose of Git worktrees?
7. What is RED-GREEN-REFACTOR?
8. How does subagent-driven development work?
9. How does it differ from executing-plans?
10. What does systematic debugging emphasize?
11. Why is code review important?
12. What does verification-before-completion mean?
13. What is using-superpowers for?
14. Why does host compatibility matter?
15. How does Superpowers differ from Ponytail?

---

# 19. Key Takeaways

> [!summary] Lesson Summary
> - Superpowers is an agentic skills framework and software development methodology.
> - It provides composable skills for different engineering activities.
> - It emphasizes requirement discovery before implementation.
> - Its workflow includes brainstorming, worktrees, planning, implementation, testing, review, and branch completion.
> - Test-driven development and systematic debugging are central practices.
> - Verification is expected before declaring success.
> - Subagent-driven development supports delegation and staged review.
> - The repository integrates with multiple coding agents, including Codex.
> - Host capabilities affect how skills operate.
> - Superpowers complements project instructions such as AGENTS.md.
> - A structured process can improve reliability while adding overhead.
> - Human review and judgment remain essential.

    Superpowers → composable skills → structured development
    → Understand → Design → Plan → Implement → Test
    → Review → Verify → Complete

**Remember:** Superpowers is not mainly about making an agent write code faster. It helps the agent follow a disciplined process for building, testing, and verifying software.

---

# 20. Official References

## Primary Sources

- [Superpowers GitHub Repository](https://github.com/obra/superpowers)
- [Official README](https://github.com/obra/superpowers/blob/main/README.md)
- [Skills Directory](https://github.com/obra/superpowers/tree/main/skills)
- [Documentation Directory](https://github.com/obra/superpowers/tree/main/docs)
- [Codex Plugin Directory](https://github.com/obra/superpowers/tree/main/.codex-plugin)
- [Repository AGENTS.md](https://github.com/obra/superpowers/blob/main/AGENTS.md)
- [Release Notes](https://github.com/obra/superpowers/blob/main/RELEASE-NOTES.md)

## Related Documentation

- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [Git Documentation](https://git-scm.com/docs)
- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)
- [Android Developers](https://developer.android.com/)
- [Laravel Documentation](https://laravel.com/docs)
- [Next.js Documentation](https://nextjs.org/docs)

> [!note] Documentation Accuracy
> Repository purpose, current host list, skill names, workflow, philosophy, and top-level structure were checked against the official Superpowers README and repository on October 20, 2026.
>
> Diagrams and practical examples are educational interpretations, not outputs from a running installation. Repository contents and integrations may change.

---

# 21. Next Lesson

**[[01 - Architecture and Core Concepts]]**

Topics:

1. Internal architecture and SKILL.md structure.
2. Skill discovery, activation, and using-superpowers.
3. Plugin adapters and host integration.
4. Hooks and session initialization.
5. Skill composition and dependencies.
6. Instruction hierarchy and AGENTS.md.
7. Context engineering and progressive disclosure.
8. Differences between skills, commands, hooks, and agents.
9. Codex-specific architecture.
10. Android, Laravel, and Next.js architecture examples.
11. Practical exercises and key takeaways.

**Learning Progression:**

Repository Overview → Architecture and Core Concepts → Installation and Setup → Skills and Features → Brainstorming and Planning → Test-Driven Development and Debugging → Subagent-Driven Development → Codex Integration → Git Worktrees and Code Review → Best Practices and Exercises