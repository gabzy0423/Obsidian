# 03 — Superpowers: Skills and Features

> [!abstract] Lesson Information
> **Repository:** [obra/superpowers](https://github.com/obra/superpowers)
>
> **Category:** Agentic Engineering / Skills / Software Development Lifecycle
>
> **Difficulty:** Intermediate
>
> **Prerequisites:** [[00 - Repository Overview]], [[01 - Architecture and Core Concepts]], [[02 - Installation and Setup]]
>
> **Learning Approach:** Theory First
>
> **Primary Goal:** Understand Superpowers' core skills, their triggers, workflows, dependencies, limitations, and applications in real software projects.
>
> **Repository Reference Date:** October 20, 2026

---

# 1. Learning Objectives

After completing this lesson, you should be able to:

1. Identify the core Superpowers skills and their responsibilities.
2. Distinguish skill discovery, selection, activation, and execution.
3. Understand relationships between skills and the overall development workflow.
4. Explain the meta skills.
5. Compare sequential and parallel agent execution.
6. Apply appropriate skills to engineering tasks.
7. Recognize activation problems and host limitations.
8. Use Superpowers without losing control of scope.

---

# Part I — Understanding the Skills System

## 2. What Is a Superpowers Skill?

A Superpowers skill is a reusable set of instructions that guides a coding agent through a particular software engineering activity. The agent performs the work; the skill provides the methodology.

Examples:

- **brainstorming** — clarify the problem and design.
- **writing-plans** — define implementation steps.
- **test-driven-development** — implement through tests.
- **systematic-debugging** — identify root causes.
- **verification-before-completion** — confirm results before reporting.

    Developer Request → Coding Agent ← Superpowers Plugin
                                      └─ Skills Library
                                         └─ Relevant Skill
    Agent → Development Tools → Project Files, Git, Test Runner

> [!important] Core Principle
> **Skills define the process. The agent executes the process. The development environment provides the tools.**

A skill does not automatically run a server, compile source code, change a database, create a branch, or execute terminal commands. Those actions require tools supplied by the host.

## 2.1 Why Specialized Skills?

Without focused workflows, an agent may code before understanding requirements, skip tests, guess at fixes, miss dependencies, change unrelated files, or claim completion without evidence. Superpowers addresses these risks with explicit procedures for different tasks.

---

# Part II — Complete Skills Library

## 3. Skill Categories

The official README lists **15 core skills** in four categories:

### Testing

| Skill | Responsibility |
|---|---|
| test-driven-development | Implement behavior using RED-GREEN-REFACTOR |

### Debugging

| Skill | Responsibility |
|---|---|
| systematic-debugging | Investigate failures using a structured root-cause process |
| verification-before-completion | Require evidence before claiming success |
| diagnosing-superpowers | Investigate unexpected Superpowers behavior in a session |

### Collaboration and Development

| Skill | Responsibility |
|---|---|
| brainstorming | Clarify requirements and agree on a design |
| writing-plans | Convert designs into executable tasks |
| executing-plans | Execute tasks in the current session |
| subagent-driven-development | Delegate tasks to fresh agents with staged review |
| dispatching-parallel-agents | Coordinate independent concurrent work |
| requesting-code-review | Request structured review |
| receiving-code-review | Evaluate and respond to feedback |
| using-git-worktrees | Create isolated development workspaces |
| finishing-a-development-branch | Complete branch and integration workflows |

### Meta

| Skill | Responsibility |
|---|---|
| using-superpowers | Guide skill discovery and usage |
| writing-skills | Create and test reusable skills |

---

# Part III — Skill Discovery and Activation

## 4. How Does an Agent Know Which Skill to Use?

Superpowers is designed to make the agent check for relevant skills before undertaking work. For example, investigate a failing login endpoint with systematic-debugging; design a new inventory module with brainstorming; implement an approved plan with executing-plans or subagent-driven-development.

    Receive Request → Understand Task → Check Skills
       → Load Relevant Instructions → Execute Workflow → Verify

### Discovery, Selection, Activation, Execution

- **Discovery:** The host can see that a skill exists.
- **Selection:** The agent determines that it applies.
- **Activation:** The skill instructions are applied.
- **Execution:** The agent performs the workflow.

These stages are distinct. The README describes mandatory workflow expectations, but a skill does not outrank user permissions, higher-priority instructions, or host security controls. Actual execution depends on host capabilities.

---

# Part IV — Meta Skill: using-superpowers

## 5. Purpose

using-superpowers introduces the skills system and encourages the agent to load relevant skills. It acts as a bootstrap or coordination skill so that an agent with access to many workflows does not simply skip them.

    Session Starts → Superpowers Available → Task Arrives
        → Check Relevant Skills → Load Skill → Follow Workflow

It is relevant throughout a session, but host integration determines how its guidance is delivered. Its presence does not guarantee perfect compliance; verify behavior in the installed host.

**Source:** [using-superpowers/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/using-superpowers/SKILL.md)

---

# Part V — Brainstorming

## 6. Skill: brainstorming

**Purpose:** Clarify requirements and establish an agreed design before implementation.

**Typical trigger:** A developer proposes a new application, feature, workflow, or integration.

The skill explores existing project context, asks relevant questions, identifies constraints, compares approaches, presents a design, and seeks approval before implementation.

    Idea → Inspect Context → Clarify Requirements → Explore Options
         → Present Design → Approval? → Revise or Plan

For “Add task assignment,” clarify who can assign tasks, whether multiple assignees are allowed, which statuses exist, whether notifications are in scope, and how the data is stored. The agent should not invent answers.

Example request:

    Help me design task assignment.
    Inspect the existing architecture and MVP scope.
    Clarify requirements before proposing implementation.
    Do not modify files yet.

Brainstorming involves interaction and planning overhead. Match the depth to the task while respecting the skill's workflow.

**Source:** [brainstorming/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md)

---

# Part VI — Implementation Planning

## 7. Skill: writing-plans

**Purpose:** Turn an approved design into clear implementation tasks.

The README describes small tasks, roughly 2–5 minutes each, with specific file paths, concrete changes, and verification steps. This is a planning guideline, not a promised execution time.

A weak plan says “Implement the feature. Test everything.” A useful plan names the file, behavior, test, command, and expected result.

Example for a status filter:

1. Add tests for completed and pending tasks.
2. Implement the filtering logic.
3. Integrate it into the existing UI.
4. Add empty-state behavior.
5. Run regression checks and review.

    Approved Design → Identify Components → Find Dependencies
      → Break Work Into Tasks → Define Tests → Define Verification

Avoid vague tasks that force the implementation agent to redesign the feature.

**Source:** [writing-plans/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/writing-plans/SKILL.md)

---

# Part VII — Plan Execution

## 8. Skill: executing-plans

**Purpose:** Execute an approved plan in the current session.

    Approved Plan → Read Tasks → Execute Inline → Verify
                  → Review Whole Branch

The README describes this as the lower-cost alternative to subagent-driven development. It has less delegation overhead and suits smaller sequential plans. A long session can accumulate context, and review is generally less frequent than per-task review.

A small Next.js search feature with a few dependent tasks may fit inline execution.

**Source:** [executing-plans/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/executing-plans/SKILL.md)

---

# Part VIII — Test-Driven Development

## 9. Skill: test-driven-development

**Purpose:** Implement behavior through RED-GREEN-REFACTOR.

    Choose Behavior → Write Test → Run and Observe Expected Failure
       → Implement Minimum Code → Run and Pass → Refactor → Recheck

- **RED:** Confirm the new test fails for the expected reason.
- **GREEN:** Add the minimum production code to pass.
- **REFACTOR:** Improve code while keeping tests green.

Illustrative JavaScript test:

    import { expect, it } from "vitest";
    import { isCompleted } from "./task";

    it("recognizes completed tasks", () => {
      expect(isCompleted({ status: "completed" })).toBe(true);
    });

Possible implementation:

    export function isCompleted(task) {
      return task.status === "completed";
    }

Confirm the test fails first, and that it fails meaningfully. Then implement, rerun, and refactor. Avoid writing production code before the test, tests that cannot fail, tests of irrelevant internals, and needless mocking. UI or platform behavior may also need integration or manual checks.

**Source:** [test-driven-development/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/test-driven-development/SKILL.md)

---

# Part IX — Systematic Debugging

## 10. Skill: systematic-debugging

**Purpose:** Find the root cause instead of repeatedly guessing.

Typical triggers include failing tests, unexpected behavior, runtime errors, build failures, and integration issues. The repository describes a four-phase root-cause process; in practical terms, investigate, analyze evidence, test a hypothesis, then implement and verify the fix.

    Observe Failure → Reproduce → Collect Evidence → Trace Cause
      → Test Hypothesis → Focused Fix → Regression Test

For a failed Laravel schedule creation, reproduce the request, inspect input and validation, trace business rules, inspect relevant data, identify the cause, add a regression test, and verify the fix. Do not change several unrelated variables at once.

Supporting techniques include root-cause tracing, defense in depth, and condition-based waiting.

**Source:** [systematic-debugging/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md)

---

# Part X — Verification Before Completion

## 11. Skill: verification-before-completion

**Purpose:** Support completion claims with actual evidence.

Use before saying a bug is fixed, a feature is complete, a build passes, tests succeeded, or a task is ready for review.

    Identify Checks → Execute Checks → Inspect Results
      → Pass? → Report Verified Completion or Explain Gaps

Example commands, when configured:

    Android: .\\gradlew.bat assembleDebug
    Laravel: php artisan test
    Next.js: npm run build

A good report distinguishes implemented work, checks actually run, skipped checks, and remaining risk. An agent's confidence is not the same as a successful test run.

**Source:** [verification-before-completion/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/verification-before-completion/SKILL.md)

---

# Part XI — Subagent-Driven Development

## 12. Skill: subagent-driven-development

**Purpose:** Execute an implementation plan using fresh subagents for individual tasks, when the host supports the workflow.

    Main Agent → Approved Plan → Task Implementer
       → Specification Review → Code Quality Review → Next Task

A specification review checks whether the task meets requirements. A code quality review checks implementation quality and risks. Fresh task contexts can reduce irrelevant history and make task boundaries clearer.

Tradeoffs include more tokens, coordination, agent calls, and execution overhead. Do not use multiple agents for a tiny change that one agent can handle safely.

**Source:** [subagent-driven-development/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/subagent-driven-development/SKILL.md)

---

# Part XII — Parallel Agents

## 13. Skill: dispatching-parallel-agents

**Purpose:** Coordinate independent tasks that can safely run concurrently.

    Main Agent → Independent Task A ─┐
                Independent Task B ──┼→ Combine → Integration Check
                Independent Task C ──┘

Parallelize when tasks do not depend on each other and host capacity supports it. Avoid it when agents edit the same files, share changing state, or depend on unfinished work. Coordination can cost more than the time saved.

**Source:** [dispatching-parallel-agents/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md)

---

# Part XIII — Code Review

## 14. requesting-code-review

**Purpose:** Request structured review of implementation changes, such as after a task, milestone, or before branch completion. Review requirements, correctness, security, maintainability, tests, performance, and complexity. Provide the relevant plan and changed code.

**Source:** [requesting-code-review/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/requesting-code-review/SKILL.md)

## 15. receiving-code-review

**Purpose:** Evaluate feedback carefully before applying it. Review feedback is evidence to assess, not an automatic order to implement.

If a reviewer requests a repository abstraction, check whether there is a real requirement, whether the project already has a suitable layer, and whether the abstraction improves maintainability without needless complexity.

    Receive Feedback → Understand → Inspect Evidence → Valid?
       → Explain with Evidence or Apply Focused Fix → Verify

Avoid blindly accepting suggestions from another agent.

**Source:** [receiving-code-review/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/receiving-code-review/SKILL.md)

---

# Part XIV — Git Worktrees

## 16. Skill: using-git-worktrees

**Purpose:** Create an isolated development workspace. A Git worktree lets multiple working directories share repository history while checking out different branches.

    Inspect Git State → Choose Location → Create Worktree/Branch
       → Set Up Project → Verify Baseline → Implement

Example commands:

    git worktree list
    git worktree add -b feature/task-filter ..\\task-filter main

The second example assumes a main branch and a destination directory that does not exist. Inspect existing changes, preserve uncommitted work, choose a useful branch name, and verify baseline tests. Do not remove a worktree containing valuable work.

**Source:** [using-git-worktrees/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/using-git-worktrees/SKILL.md)

---

# Part XV — Branch Completion

## 17. Skill: finishing-a-development-branch

**Purpose:** Give completed work a clear outcome after implementation and verification. Options described by the workflow include merge, pull request, keep the branch, or discard work.

    Implementation Complete → Verify → Present Options
       → Merge / Pull Request / Keep / Discard

Review destructive outcomes and get appropriate authorization. A finished feature should not be left with an unclear branch or worktree state.

**Source:** [finishing-a-development-branch/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/finishing-a-development-branch/SKILL.md)

---

# Part XVI — Diagnosing Superpowers

## 18. Skill: diagnosing-superpowers

**Purpose:** Investigate session issues such as a skill not activating, an unexpected skill activating, the agent ignoring a plan, repeated work, or unexpectedly high token use.

    Unexpected Behavior → Inspect Session Evidence → Compare Expected
       → Find Divergence → Report Evidence → Recommend Correction

The README describes transcript-based diagnosis and a scrubbed bug-report bundle. Review logs for API keys, passwords, private source, personal data, or confidential information before sharing them.

Example request:

    Figure out what went wrong with Superpowers in this session.
    Identify which skill should have been used and the evidence.

**Source:** [diagnosing-superpowers/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/diagnosing-superpowers/SKILL.md)

---

# Part XVII — Creating Skills

## 19. Skill: writing-skills

**Purpose:** Create and test reusable skills for recurring work. A team-specific Laravel API review skill, for example, could describe route inspection, authorization, validation, query review, tests, and reporting.

A simple skill package may contain a SKILL.md and optional reference material. Keep triggers clear, instructions specific, avoid repeating generic rules, define verification, and test whether it activates appropriately. A custom skill is not part of the official Superpowers repository unless contributed; install it using a supported host mechanism.

The YAML below is an example skill header, not frontmatter for this Obsidian note:

    ---
    name: laravel-api-review
    description: Review Laravel endpoints for correctness, authorization, and validation.
    ---

**Source:** [writing-skills/SKILL.md](https://github.com/obra/superpowers/blob/main/skills/writing-skills/SKILL.md)

---

# Part XVIII — Complete Workflow Composition

## 20. How the Skills Connect

    Feature Request → brainstorming → Design Approval
       → using-git-worktrees → writing-plans
       → executing-plans OR subagent-driven-development
       → test-driven-development → requesting-code-review
       → receiving-code-review → verification-before-completion
       → finishing-a-development-branch

Supporting workflows:

- systematic-debugging — when problems occur.
- dispatching-parallel-agents — for independent concurrent work.
- diagnosing-superpowers — when the skill system misbehaves.
- using-superpowers — skill discovery and usage.
- writing-skills — reusable skill development.

Not every skill runs exactly once. Debugging may repeat; TDD covers each behavior; verification happens throughout; review timing varies by execution approach.

---

# Part XIX — Choosing the Right Skill

## 21. Skill Selection Matrix

| Situation | Primary skill |
|---|---|
| New feature idea | brainstorming |
| Approved design needs tasks | writing-plans |
| Plan runs in the current session | executing-plans |
| Plan needs task-level delegation | subagent-driven-development |
| Independent tasks can run concurrently | dispatching-parallel-agents |
| Implementing behavior | test-driven-development |
| Unexpected bug | systematic-debugging |
| Independent review needed | requesting-code-review |
| Review feedback received | receiving-code-review |
| Isolated branch workspace needed | using-git-worktrees |
| Ready to report completion | verification-before-completion |
| Development branch is finished | finishing-a-development-branch |
| Superpowers behaves unexpectedly | diagnosing-superpowers |
| A reusable skill is needed | writing-skills |

Multiple skills may apply to one task.

---

# Part XX — Codex CLI Integration

## 22. How Skills Work with Codex

Codex is the execution environment. Superpowers provides methodology. The project supplies requirements and architecture.

    Developer → Codex CLI ← Superpowers skills
                      ↑
                 AGENTS.md
                      ↓
              Inspect → Plan → Implement
                      → Test → Review → Verify

The README documents plugin installation through the Codex interface:

1. Open /plugins.
2. Search for superpowers.
3. Select Install Plugin.

Start a fresh session, confirm availability, try a relevant task, and observe skill use. Different hosts have different skill interfaces, subagent support, hooks, permissions, and session behavior. Verify the actual integration.

---

# Part XXI — Practical Applications

## 23. Android: GroupTrack Status Filter

Clarify available statuses, filter placement, persistence, and MVP scope. Plan which Java classes, XML layouts, models, and tests are affected. Write a filtering test first, implement using existing components, run the Gradle checks, and review correctness and UI consistency.

Example build check:

    .\\gradlew.bat assembleDebug

## 24. Laravel: GoPasig Route Filtering

Clarify route selection, authorization, and responses. Plan changes to routes, controllers, models, and tests. Add a feature test, implement using current conventions, run tests, and review validation, query correctness, authorization, and API compatibility.

    php artisan test

## 25. Next.js: Product Search

Clarify client-side vs server-side search, data size, searchable fields, pagination, and URL behavior. Plan components, types, data flow, and tests. Reuse existing UI, write behavior tests, then build and lint using configured scripts.

    npm run build
    npm run lint

---

# Part XXII — Common Mistakes

## 26. Mistakes to Avoid

| Mistake | Better practice |
|---|---|
| Ignoring skill discovery | Check relevant skills before work |
| Coding before design agreement | Clarify with brainstorming |
| Writing vague plans | Define concrete tasks and checks |
| Skipping RED in TDD | Observe the expected test failure |
| Guessing during debugging | Investigate the root cause |
| Using subagents for every tiny task | Compare overhead with benefit |
| Parallelizing dependent tasks | Respect task dependencies |
| Blindly applying review feedback | Verify findings with evidence |
| Claiming success without tests | Report actual verification |
| Discarding branches without review | Preserve valuable work |
| Sharing raw diagnostic logs | Check for sensitive data |
| Assuming identical host behavior | Verify host capabilities |

---

# Part XXIII — Practical Exercises

## 27. Exercise 1 — Skill Identification

Match each scenario to a skill: unclear feature requirements; unknown bug cause; approved design needing tasks; completed implementation needing review; independent issues to investigate; branch ready to merge; unexpected activation behavior; team needs a reusable workflow.

**Expected outcome:** Recognize skill responsibilities.

## 28. Exercise 2 — Design a Workflow

For “Build a task tracking feature,” map brainstorming, writing-plans, TDD, requesting-code-review, and verification-before-completion. Explain why each applies.

## 29. Exercise 3 — TDD Practice

Create a task completion predicate. Write a test first, confirm the expected failure, implement the smallest function, confirm it passes, then refactor if useful.

## 30. Exercise 4 — Debugging Practice

In a disposable project, introduce a harmless logic error. Reproduce it, collect evidence, find the cause, add a regression test, fix it, and verify.

## 31. Exercise 5 — Compare Execution Strategies

Compare executing-plans with subagent-driven-development for context use, token overhead, review frequency, task complexity, host requirements, and coordination cost.

## 32. Exercise 6 — Map Skill Dependencies

Draw a Mermaid diagram connecting brainstorming, planning, worktrees, implementation, TDD, review, verification, branch completion, and debugging.

---

# Part XXIV — Knowledge Check

## 33. Review Questions

1. What is a Superpowers skill?
2. How many core skills are listed in the current README?
3. What is using-superpowers for?
4. What does brainstorming produce?
5. What does writing-plans produce?
6. How does executing-plans differ from subagent-driven development?
7. What is RED-GREEN-REFACTOR?
8. Why observe a test fail before implementation?
9. What does systematic debugging emphasize?
10. Why verify before claiming completion?
11. What review stages are described for subagent-driven work?
12. When should parallel agents be used?
13. Why evaluate review feedback?
14. What does a Git worktree provide?
15. What does branch completion do?
16. What is diagnosing-superpowers?
17. What is writing-skills?
18. How do skills compose into a lifecycle?
19. Why does host compatibility matter?
20. How does Superpowers differ from Ponytail?

---

# Part XXV — Key Takeaways

> [!summary] Lesson Summary
> - The current README documents 15 core skills in Testing, Debugging, Collaboration, and Meta categories.
> - using-superpowers guides skill discovery and use.
> - brainstorming clarifies requirements and design.
> - writing-plans creates actionable tasks.
> - executing-plans runs inline; subagent-driven-development delegates tasks with staged review.
> - dispatching-parallel-agents coordinates independent work.
> - test-driven-development uses RED-GREEN-REFACTOR.
> - systematic-debugging prioritizes root-cause investigation.
> - verification-before-completion requires evidence.
> - Review skills support assessment and careful feedback handling.
> - Git worktrees isolate development; branch completion gives work a clear outcome.
> - diagnosing-superpowers investigates session problems; writing-skills supports reusable skill creation.
> - Host capabilities affect execution.

    Superpowers → Skills → Understand → Design → Plan → Implement
       → Test → Review → Verify → Complete

Superpowers is not a collection of unrelated prompts. Its skills are designed to work together as a development methodology.

---

# Part XXVI — Official References

- [Official GitHub Repository](https://github.com/obra/superpowers)
- [Official README](https://github.com/obra/superpowers/blob/main/README.md)
- [Skills Directory](https://github.com/obra/superpowers/tree/main/skills)
- [using-superpowers](https://github.com/obra/superpowers/blob/main/skills/using-superpowers/SKILL.md)
- [brainstorming](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md)
- [writing-plans](https://github.com/obra/superpowers/blob/main/skills/writing-plans/SKILL.md)
- [executing-plans](https://github.com/obra/superpowers/blob/main/skills/executing-plans/SKILL.md)
- [test-driven-development](https://github.com/obra/superpowers/blob/main/skills/test-driven-development/SKILL.md)
- [systematic-debugging](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md)
- [verification-before-completion](https://github.com/obra/superpowers/blob/main/skills/verification-before-completion/SKILL.md)
- [subagent-driven-development](https://github.com/obra/superpowers/blob/main/skills/subagent-driven-development/SKILL.md)
- [dispatching-parallel-agents](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md)
- [requesting-code-review](https://github.com/obra/superpowers/blob/main/skills/requesting-code-review/SKILL.md)
- [receiving-code-review](https://github.com/obra/superpowers/blob/main/skills/receiving-code-review/SKILL.md)
- [using-git-worktrees](https://github.com/obra/superpowers/blob/main/skills/using-git-worktrees/SKILL.md)
- [finishing-a-development-branch](https://github.com/obra/superpowers/blob/main/skills/finishing-a-development-branch/SKILL.md)
- [diagnosing-superpowers](https://github.com/obra/superpowers/blob/main/skills/diagnosing-superpowers/SKILL.md)
- [writing-skills](https://github.com/obra/superpowers/blob/main/skills/writing-skills/SKILL.md)
- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [Git Documentation](https://git-scm.com/docs)
- [Agent Skills Specification](https://agentskills.io/specification)

> [!note] Documentation Scope
> Skill names, categories, workflow, and repository structure were checked against the official README. Diagrams, prompts, code examples, and exercises are educational interpretations; hosts may differ in activation and subagent behavior. Consult each skill's SKILL.md for current instructions.

---

# Part XXVII — Next Lesson

**[[04 - Brainstorming and Planning]]**

Topics: brainstorming in depth; requirement discovery; stakeholders; functional and non-functional requirements; design alternatives and approval; design specifications; writing-plans; task decomposition, dependencies, and execution order; acceptance criteria; TDD-ready plans; scope control; Android, Laravel, and Next.js examples; exercises.

**Learning Progression:**

Repository Overview → Architecture and Core Concepts → Installation and Setup → **Skills and Features** → Brainstorming and Planning → Test-Driven Development and Debugging → Subagent-Driven Development → Codex Integration → Git Worktrees and Code Review → Best Practices and Exercises