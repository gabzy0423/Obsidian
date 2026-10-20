# 06 — Superpowers: Subagent-Driven Development

> [!abstract] Lesson Information
> **Repository:** [obra/superpowers](https://github.com/obra/superpowers)
>
> **Category:** Agentic Engineering / Multi-Agent Systems / Task Delegation / Code Review
>
> **Difficulty:** Intermediate to Advanced
>
> **Learning Approach:** Theory First
>
> **Prerequisites:** [[03 - Skills and Features]], [[04 - Brainstorming and Planning]], [[05 - Test-Driven Development and Debugging]]
>
> **Primary Skills:** subagent-driven-development, dispatching-parallel-agents, executing-plans
>
> **Primary Goal:** Understand how Superpowers coordinates implementation agents, review agents, context isolation, task dependencies, and Git-based verification.

---

# 1. Learning Objectives

After completing this lesson, you should be able to:

1. Explain what a subagent is.
2. Distinguish the main agent from delegated agents.
3. Understand fresh-context task execution.
4. Describe the Superpowers subagent workflow.
5. Explain implementation agents and independent reviews.
6. Understand specification and code quality reviews.
7. Describe correction loops and progress ledgers.
8. Distinguish sequential delegation from parallel execution.
9. Identify tasks suitable for delegation.
10. Understand context packaging, Git coordination, and model tradeoffs.
11. Recognize Codex host limitations.
12. Apply these concepts to Android, Laravel, and Next.js.

---

# Part I — Multi-Agent Fundamentals

## 2. What Is a Subagent?

A **subagent** is an AI execution context delegated to perform a specific task. A main agent assigns work and coordinates results; the subagent focuses on its responsibility.

A subagent may receive its own context, specific instructions, permitted tools, a task, and a reporting contract. It should not automatically inherit the main agent's entire conversation history. The controller prepares the relevant context.

| Main agent / controller | Subagent |
|---|---|
| Coordinates the overall workflow | Executes the assigned task |
| Maintains project-wide context | Receives task-specific context |
| Tracks dependencies and progress | Reports results or blockers |
| Prepares task briefs | Works within assigned scope |
| Coordinates review and integration | Implements or reviews changes |

    Developer → Main Agent / Controller → Implementer
                       ↑                     ↓
                       └── Reviewers ← Report
                       ↓
                  Verified Result

Exact capabilities depend on the coding-agent host.

---

# Part II — Why Use Subagents?

## 3. The Problem with One Long Agent Session

A long feature session can collect requirements, architecture exploration, test logs, changes, debugging history, feedback, and tool output. Relevant information can become harder to distinguish from stale context. This is sometimes called **context pollution**.

A task implementing an Android layout does not necessarily need the full history of Firebase configuration.

Fresh task contexts can improve focus and separate coordination from implementation. They also add agent calls, context preparation, reviews, coordination, latency, and token cost.

> [!important] Key Principle
> Use subagents when isolated execution and review are worth their coordination cost.

---

# Part III — subagent-driven-development

## 4. Purpose of the Skill

The official subagent-driven-development skill executes an implementation plan by dispatching a fresh implementer for each task, reviewing each task for specification compliance and quality, and performing a broad review of the whole branch at the end.

    Approved Plan → Prepare Workspace and Ledger → Preflight Plan
      → Dispatch Implementer → Task Review → Correct if Needed
      → Record Completion → Next Task → Whole-Branch Review
      → Branch Completion

The workflow is sequential for implementation tasks. It does not dispatch multiple implementation subagents in parallel, because they may conflict.

### 4.1 Preconditions

The skill's decision process asks:

1. Is there an implementation plan?
2. Are the tasks sufficiently independent for delegation?
3. Does the host support subagents, and has inline execution been chosen?

Without a plan, brainstorm or plan first. For tightly coupled tasks, reconsider the task breakdown. If the user chooses inline execution or the host lacks subagents, use executing-plans.

    Plan exists? → Tasks delegable? → Subagents available and selected?
       No → Brainstorm or plan first
       Tightly coupled → Rework plan
       Inline chosen / no tool → executing-plans
       Yes → subagent-driven-development

---

# Part IV — The Controller Agent

## 5. Responsibilities of the Main Agent

The main agent acts as the **controller**. It coordinates rather than implementing every task itself.

It reads the approved specification and plan, checks dependencies, prepares the isolated workspace, maintains a progress ledger, prepares briefs, dispatches implementers and reviewers, records rulings, tracks completion, and coordinates final review.

    Controller: What must happen next?
    Implementer: How do I complete this assigned task?
    Reviewer: Does the result satisfy the requirements?

Separating coordination from implementation preserves independent task execution and review. The official workflow says the controller should not silently implement review fixes itself; delegated fixes return through the review loop.

---

# Part V — Task Briefs and Context Packaging

## 6. What Is a Task Brief?

A task brief is a focused package containing the information needed for one assignment:

- Task objective and full requirements.
- Exact relevant file paths.
- Interfaces and earlier decisions.
- Constraints and global requirements.
- Test expectations and report location.

Illustrative brief:

    Task 2 — Implement Task Filtering
    Objective: Filter tasks by status.
    Files: TaskFilter.java, TaskFilterTest.java
    Interface: filterByStatus(tasks, selectedStatus)
    Requirements: Preserve task objects; return an empty list if none match.
    Verification: Run the relevant unit tests.
    Constraint: Add no dependencies.

This example is educational, not an official generated brief.

## 6.1 Why Not Give Every Subagent the Whole Plan?

A plan may include unrelated tasks. Sending all of it can waste tokens, increase scope drift, and confuse ownership. Give the implementer the task brief and only the cross-task interfaces or rulings it needs.

The current skill provides helper scripts to create plan-specific workspaces, task briefs, and review packages. It tells the controller not to make implementers read the entire plan. Large artifacts are passed by file path so their content does not need to be pasted repeatedly into the controller's context.

---

# Part VI — Implementation Subagents

## 7. What Does the Implementer Do?

An implementation subagent reads the task brief and project instructions, inspects relevant files, follows existing architecture, writes tests where required, implements the behavior, runs verification, self-reviews, commits when the workflow asks, and reports the result.

    Task Brief → Inspect → Test → Implement → Verify
                 → Self-Review → Commit/Report

The exact steps depend on task scope and applicable skills. Implementers do not dispatch their own reviewers in this workflow; the controller arranges task review after receiving the report.

---

# Part VII — Status Reports

## 8. Implementation Statuses

The current skill defines four implementation statuses:

| Status | Meaning | Controller response |
|---|---|---|
| DONE | Task completed | Prepare a review package and dispatch review |
| DONE_WITH_CONCERNS | Completed with questions or concerns | Read concerns; resolve scope or correctness doubts, then review |
| NEEDS_CONTEXT | Required information is missing | Provide it and redispatch |
| BLOCKED | Cannot proceed under current conditions | Change context, model, task size, or plan as appropriate |

A blocker may mean missing context, insufficient reasoning capability, an oversized task, or a defective plan. Do not force the same agent to repeat the same failed attempt without changing something.

Example report:

    Task 2 — Status: DONE
    Changes: Added status filtering and unit tests.
    Verification: Targeted tests passed.
    Commit: abc1234
    Concerns: None.

This is a simplified example.

---

# Part VIII — Specification Compliance Review

## 9. Did the Implementation Meet the Approved Requirements?

Specification compliance compares the task output with its brief and binding constraints.

For a requirement “filter existing statuses only,” adding an unrequested archived status violates scope even if the new behavior works.

Review questions:

- Were all required behaviors implemented?
- Were requirements omitted or extra features added?
- Were interface contracts and project constraints followed?
- Are the specified tests present?

    Approved Requirements + Implementation Diff → Spec Reviewer
                                               → Compliance Findings

---

# Part IX — Code Quality Review

## 10. Is the Implementation Technically Sound?

Code quality review evaluates correctness, security, error handling, maintainability, tests, and unnecessary complexity.

A task filter may satisfy requirements but duplicate the same filtering logic across screens. The spec review may pass while the quality review identifies maintainability risk.

| Specification review | Code quality review |
|---|---|
| Did we build the required behavior? | Is the implementation sound and maintainable? |
| Checks requirements and scope | Checks technical risks and quality |
| Finds missing or extra behavior | Finds duplication or fragile logic |

The current workflow requires both verdicts. Implementer self-review does not replace independent task review.

---

# Part X — Review and Correction Loop

## 11. What Happens When Review Finds a Problem?

Significant findings trigger a correction dispatch and scoped re-review. The current skill sets a maximum of five correction rounds per task.

- Rounds 1–3 generally resume the original implementer.
- Rounds 4–5 use a fresh implementer on a more capable model.
- Each round includes a fix, relevant tests, an updated report, and a scoped re-review.
- After round five, remaining findings are explicitly adjudicated and recorded.

    Review → Significant Finding? → Fix Dispatch → Scoped Re-review
       No → Record Completion
       Yes → Repeat within five rounds → Adjudicate remaining findings

Minor observations are recorded for the final branch review; they do not enter the fix loop. Findings that conflict with the plan require a recorded ruling, with the specification treated as the binding authority. A correction cap does not justify hiding a serious issue. The workflow records open findings and only stops if every way forward would be a guess.

The three-failed-fixes checkpoint in systematic-debugging is a different workflow from this five-round SDD task limit.

---

# Part XI — Progress Ledger

## 12. What Is a Progress Ledger?

A progress ledger is a persistent recovery record for a plan. It tracks completed tasks, commits, review findings, correction rounds, decisions, and deferred issues.

Conversation memory may be compacted or lost. Without a ledger, a controller might repeat already completed tasks. With it, the next session can resume at the first task without a completion entry.

The current skill keeps a separate workspace per plan under .superpowers/sdd/ at the repository root. It stores the plan's progress.md ledger, task briefs, reports, and review artifacts. The first line identifies the plan. Git commits remain the source of the actual code history; the ledger helps recover workflow state.

Illustrative ledger:

    # SDD ledger — plan: docs/plans/task-filter.md
    Task 1: complete — commit abc1234
    Task 2: complete — commit def5678
    Task 3: pending

This is a simplified example. Never use destructive cleanup such as git clean -fdx without understanding it; it can remove ignored workspaces. Recovery may require reading Git history.

---

# Part XII — Sequential vs Parallel Execution

## 13. Are Subagents Always Parallel?

No. Delegation is not the same as concurrency. The current subagent-driven-development skill explicitly says not to dispatch multiple implementation agents in parallel.

    Task 1 implementer → Task 1 review → Task 2 implementer
      → Task 2 review → Task 3 implementer → Task 3 review

Tasks may share files, interfaces, tests, or Git state. Sequential dispatch reduces edit conflicts and integration ambiguity.

---

# Part XIII — dispatching-parallel-agents

## 14. When Is Parallel Execution Appropriate?

The separate dispatching-parallel-agents skill handles independent work that can be investigated or completed concurrently, such as separate failing suites, unrelated module bugs, or independent research.

    Main Agent → Independent Task A ─┐
                Independent Task B ──┼→ Combine → Integration Check
                Independent Task C ──┘

Parallelism is a stronger fit when tasks have clear boundaries, do not depend on unfinished work, avoid shared-file edits, and can be evaluated independently. Avoid it when tasks are coupled, share mutable state, or coordination costs exceed savings.

**Difference:** SDD is sequential task delegation with per-task review; parallel-agent dispatch is concurrent work for independent tasks.

---

# Part XIV — Task Dependencies

## 15. Why Map Dependencies?

A dependency exists when one task needs another task's output. For example, a repository depends on the data model; a UI may depend on repository interfaces; integration tests depend on both.

    Define Data Contract → Implement Repository → Build UI → Integration Tests

Without dependency analysis, agents may invent incompatible interfaces, duplicate models, create workarounds, or edit the same files. The SDD workflow performs a preflight scan for task conflicts, shared files and interfaces, and contradictions with global constraints before dispatching Task 1. The findings and rulings go into the ledger.

---

# Part XV — Git Coordination and Review Packages

## 16. Why Does Git Matter?

Git records implementation changes and supports per-task diffs, commits, review, recovery, and whole-branch inspection.

Useful commands:

    git status
    git log --oneline -5
    git diff
    git worktree list

The official workflow calls for an isolated worktree and warns against beginning on main or master without explicit consent. It records a BASE commit before each implementer dispatch. Review packages should compare BASE to the task's current HEAD; using only HEAD~1 can omit earlier commits from a multi-commit task.

## 16.1 Review Packages

A review package gives the reviewer the task brief, implementation report, changed-file summary, commit range, and contextual diff. The controller passes artifact paths instead of pasting a long diff into its own context.

Task review checks both specification and quality. The final whole-branch review checks integration across tasks: shared interfaces, duplicate logic, regressions, and deferred findings.

---

# Part XVI — Final Whole-Branch Review

## 17. Why Review the Entire Branch?

Individual task reviews can miss failures that appear only after integration. Examples include inconsistent interfaces, duplicated logic, incompatible assumptions, and shared behavior regressions.

    Per-task reviews → Whole-branch review → Final verification → Branch outcome

The current skill calls for a final reviewer using the most capable available model, then a bounded final correction and re-review process. Once the final review is clean and the plan workspace is handled, proceed to finishing-a-development-branch.

---

# Part XVII — Token, Context, and Model Efficiency

## 18. Why Can Subagents Cost More?

Each implementer and reviewer consumes context and tool calls. A conceptual cost model is:

    Total effort = controller + implementation + review + corrections

This is not a provider billing formula. Waste can come from repeating the whole plan, including irrelevant files, verbose reports, unnecessary test reruns, or too many agents on trivial edits.

Use focused briefs, files for large artifacts, concise reports, and a persistent ledger. Batch same-shape mechanical edits into one dispatch when appropriate; reserve one-dispatch-per-task for work requiring its own judgment, tests, or review surface.

### 18.1 Model Selection

The current skill recommends the least powerful model that can handle each role, with the model explicitly specified when dispatching. Mechanical, fully specified tasks can use a faster lower-cost model; multi-file integration needs stronger judgment; architecture and final whole-branch review call for the most capable available model. Review capability should match the diff's complexity and risk. If an implementer remains stuck after three rounds, rounds four and five escalate to a more capable fresh agent.

A lower price per token does not guarantee lower total cost if the model takes more turns. Not every Codex host supports per-subagent model selection, so verify before relying on it.

---

# Part XVIII — Codex Compatibility

## 19. How Does This Apply to Codex?

Codex supplies the execution environment; Superpowers supplies methodology. Full delegation requires the host to create agent contexts, pass task briefs, receive reports, coordinate tool access, and manage shared or isolated workspaces.

Installing Superpowers does not guarantee every subagent feature behaves identically in Codex CLI, Codex App, Claude Code, or other hosts. Check the installed integration.

Example planning request:

    Read AGENTS.md and the approved plan.
    Check whether tasks suit subagent-driven development.
    Identify dependencies, reviews, and host limitations.
    Do not begin implementation yet.

This is an example prompt, not a special Codex command.

---

# Part XIX — Android Case Study

## 20. GroupTrack: Task Assignment UI

A possible breakdown:

1. Confirm the existing task assignment contract.
2. Implement the XML layout.
3. Connect it through the existing Java application logic.
4. Verify with tests and device or emulator inspection.

The brief should include project instructions, relevant files, resource IDs, assignment behavior, scope limits, and verification steps. Watch for mismatched resource names, duplicated UI logic, unnecessary dependencies, scope creep, and unverified device behavior.

**Lesson:** Subagents should preserve the Android architecture instead of independently redesigning it.

---

# Part XX — Laravel Case Study

## 21. GoPasig: Schedule Management

A possible breakdown is API contract, feature tests, service behavior, controller integration, and regression checks. Review authorization, schedule conflicts, validation, database integrity, service conventions, and query performance.

Example check:

    php artisan test

Passing one controller test does not replace cross-module review of scheduling rules.

---

# Part XXI — Next.js Case Study

## 22. E-Commerce Product Search

A possible breakdown: confirm the API contract, define frontend types, implement search state, integrate API requests, implement loading and empty states, and verify.

    Search UI → Frontend State → API Client → Laravel Backend → Database

The frontend API client depends on the backend contract; the UI depends on types and search behavior. Review type correctness, API errors, loading states, accessibility, responsiveness, and project conventions.

Example build check:

    npm run build

Run configured tests and lint checks where applicable.

---

# Part XXII — Common Anti-Patterns

## 23. Mistakes to Avoid

| Anti-pattern | Why it causes trouble |
|---|---|
| Delegating without a plan | Requirements and ownership are unclear |
| Passing the whole conversation | Wastes context and invites scope drift |
| Concurrent work on shared files | Creates conflicts |
| Skipping task reviews | Removes a quality gate |
| Trusting self-review alone | Reduces independent scrutiny |
| Ignoring NEEDS_CONTEXT or BLOCKED | Hides unresolved conditions |
| Repeating an unchanged failed attempt | Wastes time and resources |
| Losing the ledger | Risks duplicating work after compaction |
| Using one model for every task | Can waste cost or reduce quality |
| Skipping whole-branch review | Misses integration defects |
| Editing main without consent | Risks shared project state |
| Silently discarding findings | Hides known issues |

---

# Part XXIII — Practical Exercises

## 24. Exercise 1 — Role Identification

Explain controller, implementer, task reviewer, and final branch reviewer responsibilities. Draw how information moves between them.

## 25. Exercise 2 — Task Brief

Write a brief for task status filtering. Include objective, files, interface, requirements, tests, constraints, and reporting instructions.

## 26. Exercise 3 — Dependency Mapping

Given tasks to create a model, implement a repository, build a list UI, add filtering, and run integration tests, draw the dependency graph. Identify which tasks must wait.

## 27. Exercise 4 — Review Classification

Classify each as specification compliance, code quality, or both: missing required status; duplicated filtering logic; unauthorized task update; unnecessary dependency; unclear function name. Explain why.

## 28. Exercise 5 — Sequential or Parallel?

Choose execution for: two independent docs reviews; repository implementation depending on a new model; separate bugs in unrelated modules; two agents editing one controller; independent API research. Explain dependencies and risks.

## 29. Exercise 6 — Progress Recovery

Design a ledger that records the active plan, completed tasks, commits, remaining review findings, and next task after context compaction.

## 30. Exercise 7 — Cost Comparison

Compare one agent executing a whole plan with a fresh implementer and reviewer per task. Evaluate tokens, clarity, review coverage, coordination, latency, and integration risk. Choose for a small UI change and a multi-module feature.

---

# Part XXIV — Knowledge Check

## 31. Review Questions

1. What is a subagent?
2. What does the controller do?
3. Why use fresh context?
4. What is context pollution?
5. When is SDD appropriate?
6. Why does it require an approved plan?
7. What belongs in a task brief?
8. Why avoid irrelevant history?
9. What does the implementer report?
10. What do NEEDS_CONTEXT and BLOCKED mean?
11. What is specification compliance review?
12. What is code quality review?
13. Why are both needed?
14. What is a correction loop?
15. What is the five-round limit?
16. What is a progress ledger?
17. Why does context compaction matter?
18. What is sequential delegation?
19. When is parallel execution suitable?
20. Why do shared files create risk?
21. What is a review package?
22. Why perform whole-branch review?
23. How does model choice affect total cost?
24. Why verify host capabilities?

---

# Part XXV — Key Takeaways

> [!summary] Lesson Summary
> - Subagents are delegated AI execution contexts; the controller coordinates them.
> - Fresh context focuses work but costs extra dispatches and review.
> - SDD starts from a plan and uses sequential implementation delegation.
> - Each task gets a focused brief and reports completion, concerns, missing context, or a blocker.
> - Specification compliance and code quality are separate review dimensions.
> - The current skill caps task correction at five rounds and records rulings and residual findings.
> - Per-plan progress ledgers support recovery after context compaction.
> - Git commits and review packages provide evidence.
> - Parallel agents are for independent tasks; SDD implementers are not run concurrently.
> - Whole-branch review finds cross-task integration problems.
> - Model selection should match task complexity, and host support must be verified.

    Approved Plan → Isolated Workspace → Task Brief → Fresh Implementer
      → Implement/Test/Report → Spec + Quality Review → Correct/Record
      → Next Task → Whole-Branch Review → Verify → Complete Branch

Subagent-driven development is a coordinated engineering workflow, not simply assigning work to many agents.

---

# Part XXVI — Official References

- [Superpowers Repository](https://github.com/obra/superpowers)
- [Subagent-Driven Development Skill](https://github.com/obra/superpowers/blob/main/skills/subagent-driven-development/SKILL.md)
- [Dispatching Parallel Agents](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md)
- [Executing Plans](https://github.com/obra/superpowers/blob/main/skills/executing-plans/SKILL.md)
- [Writing Plans](https://github.com/obra/superpowers/blob/main/skills/writing-plans/SKILL.md)
- [Requesting Code Review](https://github.com/obra/superpowers/blob/main/skills/requesting-code-review/SKILL.md)
- [Receiving Code Review](https://github.com/obra/superpowers/blob/main/skills/receiving-code-review/SKILL.md)
- [Using Git Worktrees](https://github.com/obra/superpowers/blob/main/skills/using-git-worktrees/SKILL.md)
- [Finishing a Development Branch](https://github.com/obra/superpowers/blob/main/skills/finishing-a-development-branch/SKILL.md)
- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [Git Documentation](https://git-scm.com/docs)

> [!note] Documentation Scope
> Workflow details, the five-round correction limit, per-plan ledger, task briefs, sequential implementation rule, and whole-branch review were checked against the current official skill on October 20, 2026. Diagrams and project examples are educational adaptations. Actual behavior depends on the installed plugin and host.

---

# Part XXVII — Next Lesson

**[[07 - Codex Integration]]**

Topics: Superpowers in Codex, skill discovery, installation verification, AGENTS.md, instruction hierarchy, session management, host capabilities, subagent support, tool permissions, context engineering, prompt patterns, troubleshooting, and Android/Laravel/Next.js workflows.

**Learning Progression:**

Repository Overview → Architecture and Core Concepts → Installation and Setup → Skills and Features → Brainstorming and Planning → Test-Driven Development and Debugging → **Subagent-Driven Development** → Codex Integration → Git Worktrees and Code Review → Best Practices and Exercises