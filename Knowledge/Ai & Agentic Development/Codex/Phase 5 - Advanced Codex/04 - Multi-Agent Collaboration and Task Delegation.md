# 04 — Multi-Agent Collaboration and Task Delegation

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 5 — Advanced Codex  
> **Lesson:** 5.4 — Multi-Agent Collaboration and Task Delegation  
> **Difficulty:** Intermediate to Advanced  
> **Environment:** Windows, PowerShell, Git, VS Code, Codex CLI  
> **Prerequisite:** [[03 - Agent Skills and Reusable Workflows]]  
> **Next Lesson:** [[05 - Codex Automation and Non-Interactive Execution]]  
> **Status:** Theory Documented

---

# 1. Introduction

Traditional agentic coding workflows typically involve one AI agent handling development from beginning to end: inspect code, understand requirements, plan, modify files, test, review, and report.

This works well for small and medium tasks. Larger tasks may contain independent activities that can run concurrently, such as frontend architecture analysis, backend API analysis, database inspection, and test coverage assessment.

These activities can sometimes be delegated to separate agents. This is called **multi-agent collaboration**.

## Core Principle

**Multi-agent development divides a larger task into bounded responsibilities, allowing specialized agents to work independently while a coordinating agent integrates and verifies results.**

The goal is not to maximize agent count, but to improve execution through appropriate delegation.

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Define multi-agent collaboration.
2. Explain the difference between a main agent and subagent.
3. Understand agent orchestration.
4. Identify suitable tasks for delegation.
5. Differentiate parallel and sequential execution.
6. Explain context isolation.
7. Define clear agent responsibilities.
8. Understand shared workspace risks.
9. Prevent file modification conflicts.
10. Coordinate dependent tasks.
11. Understand agent communication.
12. Design integration checkpoints.
13. Verify subagent outputs.
14. Understand token and performance tradeoffs.
15. Recognize when a single agent is preferable.
16. Design Android, Laravel, and Next.js multi-agent workflows.
17. Apply least-privilege and security principles.
18. Evaluate whether delegation improves development quality.

---

# Part II — Multi-Agent Fundamentals

## 3. What Is Multi-Agent Collaboration?

Multi-agent collaboration is a workflow in which multiple AI agents contribute to a shared objective, each with a specific responsibility, coordinated by a main agent.

```text
                  Developer
                      |
                      v
                  Main Agent
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
      Agent A     Agent B      Agent C
      Frontend    Backend      Database
          |           |           |
          +-----------+-----------+
                      |
                      v
              Consolidated Report
```

For example, a main agent could delegate independent frontend, backend, and database reviews, then integrate findings.

Multi-agent collaboration does not require multiple agents to edit simultaneously. Independent analysis is often safer to start with.

---

## 4. Single-Agent vs Multi-Agent Workflows

| Aspect | Single Agent | Multi-Agent |
|---|---|---|
| Task execution | One agent handles workflow | Tasks distributed |
| Context | Primarily one context | Separate agent contexts |
| Coordination | Simpler | Requires orchestration |
| Parallel execution | Limited | Possible for independent work |
| File conflicts | Easier to manage | Greater risk with shared writes |
| Token usage | Often lower | May be higher |
| Best use | Small or dependent tasks | Independent, bounded subtasks |
| Verification | One workflow | Individual and integrated |

**Use a single agent by default unless delegation offers a clear advantage.**

---

# Part III — Multi-Agent Architecture

## 5. Main Agent

The main agent (also called root, coordinator, or orchestrator) coordinates the overall task.

Responsibilities:

1. Understand the request.
2. Read project instructions.
3. Determine scope.
4. Identify independent subtasks.
5. Assign responsibilities.
6. Coordinate dependencies.
7. Review subagent outputs.
8. Integrate results.
9. Validate the deliverable.
10. Report completion accurately.

The main agent retains responsibility for the overall result. Delegation does not remove the need for integration and verification.

---

## 6. Subagents

A **subagent** is an agent assigned a bounded task. Examples: inspect a controller, review XML layouts, analyze database relationships, identify missing tests, compare API contracts, or investigate one error.

Good task:

```text
Inspect the existing login XML layout.
Identify constraint, alignment, and accessibility issues.
Do not modify files. Return findings with file references.
```

Poor task: `Improve the entire application.` It lacks boundaries and completion criteria.

---

## 7. Agent Orchestration

**Orchestration** coordinates agents and their responsibilities.

```text
User Request → Main Agent Analysis → Task Decomposition
→ Delegate Independent Tasks → Collect Results → Resolve Conflicts
→ Integrate Findings or Changes → Validate Final Output
```

Orchestration should make the workflow predictable, not add needless complexity.

---

## 8. Agent Hierarchies

Some systems support hierarchical delegation:

```text
Main Agent
    +-- Frontend Agent
    |       +-- Accessibility Reviewer
    +-- Backend Agent
    +-- Testing Agent
```

Deeper hierarchies add coordination overhead. Most student and small-team projects can start with one main agent and one or two justified subagents.

---

# Part IV — Task Delegation

## 9. What Is Task Delegation?

Delegation assigns one part of a larger task to another agent. For an authentication review, one agent might inspect frontend UI and another backend logic; the main agent compares findings.

Delegate independent responsibilities, not arbitrary groups of files.

---

## 10. Characteristics of a Good Delegated Task

A delegated task needs a specific objective, clear scope, relevant context, explicit constraints, expected deliverables, and verification requirements.

```markdown
## Task
## Objective
## Relevant Files
## Required Context
## Allowed Actions
## Prohibited Actions
## Expected Output
## Completion Criteria
```

Example:

```text
Task: Review Laravel schedule validation.
Scope: schedule controller, request validation, related service.
Read applicable AGENTS.md. Do not modify files.
Identify validation gaps with file references.
Deliver a concise report. State limits and what was inspected.
```

---

## 11. Task Decomposition

Task decomposition breaks a larger task into manageable responsibilities.

```text
Feature: Product Listing
    +-- Frontend UI
    +-- Backend API
    +-- Database Queries
    +-- Integration Testing
```

Not all subtasks should run simultaneously; order depends on dependencies.

**Decompose by responsibility and dependency, not just to increase agent count.**

---

# Part V — Parallel vs Sequential Execution

## 12. Parallel Execution

Parallel execution overlaps work on independent tasks.

```text
Time →
Agent A: [Frontend Review------]
Agent B: [Backend Review--------]
Agent C: [Database Review-------]
Main:                          [Integrate]
```

Suitable tasks include independent exploration, reviewing separate modules, unrelated bug investigations, comparing approaches, and assessing distinct test suites.

Benefits include lower elapsed time in suitable cases, independent analysis, and broader coverage. Risks include higher token use, duplicate work, conflicting findings, shared-write conflicts, and coordination overhead.

---

## 13. Sequential Execution

Sequential execution starts a task after its prerequisite finishes.

```text
Requirements → API Contract → Backend Implementation
             → Frontend Integration → Integration Tests
```

Use for strict dependencies: define a contract before client integration, diagnose before fixing, or test after implementation.

**Parallelize independent work. Sequence dependent work.**

---

## 14. Hybrid Execution

Most real workflows combine parallel and sequential tasks.

```text
Requirements → Planning → +→ UI Analysis ───+
                          +→ API Analysis ──+→ Contract Review
                                             → Implementation → Tests
```

Parallel investigation can coexist with sequential implementation dependencies.

---

## 15. Decision Matrix

| Situation | Recommended Approach |
|---|---|
| Small UI alignment fix | Single agent |
| Investigating independent bugs | Parallel subagents may help |
| API must be defined before UI integration | Sequential |
| Reviewing separate modules | Parallel analysis |
| Editing one shared config file | Single writer |
| Large audit across independent modules | Parallel investigation, central synthesis |
| Small documentation update | Single agent |
| Testing completed feature | Sequential after implementation |

---

# Part VI — Context Isolation

## 16. What Is Context Isolation?

Context isolation means each subagent has a separate working context.

```text
Main Agent Context
    +-- UI Agent Context
    +-- Backend Agent Context
    +-- Testing Agent Context
```

This can reduce context interference and focus task instructions. However, separate contexts do not necessarily mean separate filesystems; agents may share a workspace.

---

## 17. Context Isolation vs Filesystem Isolation

| Concept | Meaning |
|---|---|
| Context isolation | Separate conversation or reasoning context |
| Filesystem isolation | Separate files or working directories |
| Process isolation | Separate execution processes |
| Permission isolation | Different authorized capabilities |

```text
Agent A Context ---+
                   +--- Shared Repository
Agent B Context ---+
```

Agents can have separate contexts but access the same files and conflict when both edit one file.

**Never assume separate contexts prevent file conflicts.**

---

## 18. Passing Context to Subagents

Provide enough context: objective, applicable instructions, relevant paths, architecture constraints, output format, and prohibited actions.

Avoid unrelated history, unnecessary docs, credentials, irrelevant files, and vague requests.

```text
Inspect the GroupTrack sign-in XML layout.
Read AGENTS.md and relevant UI documentation.
Focus on structure, constraints, and accessibility.
Do not modify files. Return actionable findings.
```

---

# Part VII — Responsibility and Ownership

## 19. Agent Responsibilities

| Agent | Responsibility | Output |
|---|---|---|
| Main Agent | Coordination and integration | Final result |
| UI Agent | UI analysis | UI findings |
| Backend Agent | API analysis | Backend findings |
| Test Agent | Coverage analysis | Test recommendations |

Each task needs a clear owner. Overlapping responsibilities should be intentional and coordinated.

---

## 20. File Ownership

File ownership identifies which agent may modify specific files.

```text
Frontend Agent: frontend/components/, frontend/app/
Backend Agent: backend/app/Http/, backend/app/Services/
Main Agent: integration decisions and shared configuration
```

Potential shared files: package manifests, `.env.example`, shared schemas, migrations, Git config, and project documentation. Assign one writer for shared files; others can review and recommend.

---

## 21. Single-Writer Principle

Only one agent should own modifications to a shared resource at a time.

```text
Agent A: owns LoginActivity.java
Agent B: reviews LoginActivity.java
Main Agent: approves integration
```

This reduces conflicts and clarifies accountability.

**Parallel reading is generally easier to coordinate than parallel writing.**

---

# Part VIII — Preventing File Conflicts

## 22. Common Conflict Scenarios

- **Same file:** two agents edit a controller, overwriting or conflicting logic.
- **Shared dependency manifest:** simultaneous edits create dependency/build conflicts.
- **Database schema:** migrations rely on incompatible assumptions or order.
- **API contract:** frontend and backend expect different response fields.

---

## 23. Conflict Prevention Strategies

1. Delegate read-only analysis first.
2. Assign file ownership.
3. Agree on shared contracts before implementation.
4. Add integration checkpoints.
5. Review Git diffs and test integrated changes.
6. Use separate worktrees or branches when independent implementation truly requires isolation.

---

## 24. Git Worktrees

Git worktrees provide separate working directories associated with a repository.

```text
repository/     Main working tree
repository-ui/  UI branch
repository-api/ API branch
```

Worktrees help isolate files but do not resolve logical conflicts; results still require integration and verification. They may be unnecessary for small tasks.

---

# Part IX — Git and PowerShell Diagnostics

## 25. Check Repository Status

```powershell
git status
```

Identify modified, staged, and untracked files before parallel work. Do not assume existing changes belong to an agent.

---

## 26. Inspect Current Branch

```powershell
git branch --show-current
```

Confirm the active branch before work begins.

---

## 27. Inspect Changes

```powershell
git diff
git diff --cached
```

The first shows unstaged changes; the second shows staged changes. Review modifications before combining agent outputs.

---

## 28. List Worktrees

```powershell
git worktree list
```

Additional worktrees are optional and should be justified by the task.

---

## 29. Windows Project Inspection

GroupTrack:

```powershell
Set-Location "C:\Users\Acer\AndroidStudioProjects\GroupTrack"
git status
git branch --show-current
```

E-commerce directories:

```powershell
Set-Location "C:\xampp\htdocs\ecommerce\frontend"
Get-ChildItem
Set-Location "C:\xampp\htdocs\ecommerce\backend"
Get-ChildItem
```

Frontend/backend may be separate repos or one parent repository. Inspect actual Git structure before coordinating branches or worktrees.

---

# Part X — Agent Communication

## 30. Why Communication Matters

A backend agent could find an API response like:

```json
{ "product_id": 15, "product_name": "Keyboard" }
```

while frontend code expects:

```json
{ "id": 15, "name": "Keyboard" }
```

Communicate cross-module findings before integration. Important discoveries should be reported early.

---

## 31. Structured Agent Reports

A useful report contains:

```markdown
# Subagent Report
## Assigned Task
## Files Inspected
## Findings
## Evidence
## Changes Made
## Tests Executed
## Unresolved Issues
## Recommended Next Action
```

Structured reports help the main agent integrate results without reconstructing the entire investigation.

---

## 32. Communication Protocol

```text
Main Agent assigns task → Subagent investigates → Subagent reports
→ Main Agent reviews → Accept / Clarify / Reject → Integrate
```

The main agent should not blindly accept every subagent conclusion.

---

# Part XI — Integration and Verification

## 33. Integration Responsibility

Integration combines results into a coherent outcome: merge module findings, reconcile contracts, review separate changes, run integrated tests, and prepare a consolidated report.

**Completed subtasks are not automatically a completed feature.**

---

## 34. Verification Levels

1. **Individual:** Did each subagent finish its assigned task?
2. **Cross-task:** Do findings or implementations agree?
3. **System:** Does the complete application behave correctly?

```text
Agent A Result ─┐
Agent B Result ─┼→ Integration Review → System Validation → Final Report
Agent C Result ─┘
```

---

## 35. Validation Checklist

- [ ] Delegated tasks have clear outcomes.
- [ ] File ownership rules were followed.
- [ ] No unrelated files were modified.
- [ ] API contracts are consistent.
- [ ] Relevant tests were executed.
- [ ] Test failures are documented.
- [ ] Unverified assumptions are identified.
- [ ] Git changes were reviewed.
- [ ] Security constraints remain intact.
- [ ] Final output matches the request.

---

# Part XII — Performance and Cost Tradeoffs

## 36. Why More Agents Can Cost More

Subagents may consume tokens for instructions, exploration, reasoning, tools, and output.

```text
Total Usage ≈ Main Agent Usage + Subagent Usage + Coordination Overhead
```

This is conceptual, not an exact billing formula. Parallelism may reduce elapsed time without reducing total usage.

---

## 37. Wall-Clock Time

Wall-clock time is elapsed duration.

Sequential example: A takes 5 minutes, B 6, C 4 = 15 minutes.

Idealized parallel example: these tasks may take about as long as the longest task (6 minutes), before overhead. Actual duration may be longer because of coordination and integration.

These are hypothetical, not measured Codex results.

---

## 38. Performance Tradeoff Table

| Factor | Potential Benefit | Potential Cost |
|---|---|---|
| Parallel execution | Lower elapsed time | More concurrent work |
| Context isolation | Better focus | Repeated context loading |
| Specialized tasks | Clearer responsibility | Coordination overhead |
| Independent review | Broader coverage | Duplicate findings |
| Parallel implementation | Faster independent changes | Merge conflicts |
| More agents | Concurrent investigation | Higher token usage |

---

## 39. When Not to Use Multiple Agents

Avoid delegation when the task is small, has strict dependencies, several agents would edit the same file, coordination costs exceed benefit, the objective is unclear, or one agent can handle it efficiently.

Example: `Fix the padding of one Android button.` One agent is enough.

---

# Part XIII — Practical Project Examples

## 40. GroupTrack — Android UI Workflow

GroupTrack uses Android Studio, Java, XML, Firebase, and an approved MVP scope. Scenario: review welcome, sign-in, and create-account screens.

```text
Main Agent
    +-- UI Reviewer: XML constraints and alignment
    +-- Resource Reviewer: colors, strings, drawables, themes
    +-- Main Agent: compare, deduplicate, prioritize, plan
```

Both reviewers inspect and report without editing. The main agent consolidates findings.

```text
Read AGENTS.md and relevant GroupTrack docs.
Review welcome, sign-in, and create-account UI.
If supported, delegate independent read-only reviews:
1. XML constraints and alignment.
2. Shared resources and consistency.
Do not modify files. Consolidate findings, resolve overlaps,
and provide one prioritized report within MVP scope.
```

This avoids competing edits to the same files.

---

## 41. GoPasig — Laravel Schedule and Route Review

Review scheduling and route management independently, then compare shared rules.

```text
Main Agent
    +-- Schedule Reviewer: assignment conflicts and validation
    +-- Route Reviewer: variants, stops, geometry
    +-- Main Agent: cross-module consistency and synthesis
```

Schedule scope: controller, service, assignment validation, related models. Route scope: controller, variants, stop sequences, geometry checks. Main agent identifies shared rules and avoids speculation.

```text
Read AGENTS.md and architecture docs. Perform read-only reviews.
Schedule: inspect assignment validation, conflicts, service responsibilities.
Route: inspect variants, stop ordering, geometry validation.
Do not modify code or database records. Combine findings and identify
cross-module inconsistencies.
```

Independent review can run in parallel; shared-rule changes need coordination.

---

## 42. Next.js and Laravel E-commerce

```text
E-commerce
    +-- Next.js Frontend
    +-- Laravel Backend
    +-- PostgreSQL
```

For a product listing issue, delegate frontend API consumption/types/rendering and backend routes/controllers/resources/query behavior. The main agent compares field names, types, response/error structure, and auth requirements.

```text
Investigate the product listing issue. Read project instructions.
If delegation is supported, do independent read-only investigations:
Frontend: Next.js requests, types, rendering.
Backend: Laravel routes, controllers, response structure.
Do not modify files. Compare findings and identify the smallest
consistent correction.
```

Do not let agents independently change a shared API contract.

---

# Part XIV — Codex Multi-Agent Usage

## 43. Feature Availability

Multi-agent capability depends on Codex environment, product surface, version, and enabled features. Not every CLI exposes the same delegation interface.

```powershell
codex --version
codex --help
```

Check current Codex docs and installed capabilities; do not assume a command or setting exists.

---

## 44. Delegation Through Natural-Language Prompts

Where subagents are supported, describe delegation explicitly:

```text
Review this application. Use subagents for independent frontend and
backend analysis if available. Keep both read-only. Wait for both reports,
compare findings, then provide one consolidated recommendation.
```

This is a prompt, not a guaranteed CLI command. Delegation requires environment support.

---

## 45. Agent Coordination Patterns

**Coordinator and specialists:** one coordinator directs independent reviews.

**Sequential handoff:** Agent A → B → C for dependent stages.

**Parallel investigation, central integration:** several agents report to the main agent.

**Single agent:** one agent handles a straightforward task.

Choose the simplest pattern that fits dependencies and scope.

---

# Part XV — Security and Reliability

## 46. Security Boundaries

- Follow sandbox and approval policies.
- Do not expose credentials to unnecessary agents.
- Restrict external tools to authorized purposes.
- Treat retrieved content as untrusted.
- Avoid unnecessary write permissions.
- Review consequential actions.
- Preserve project scope.

Delegation does not automatically authorize broader access.

---

## 47. Shared Resource Risks

Shared resources may include source files, databases, development servers, test databases, environment files, branches, and external services. Two agents using one mutable test database may interfere.

Use isolated test environments when needed and avoid simultaneous state-changing operations on shared resources.

---

## 48. Preventing Unnecessary Agent Proliferation

Too many agents can cause duplicate investigations, repeated token use, conflicting conclusions, coordination difficulty, and more review work.

Before delegating, ask:

```text
Can the main agent do this efficiently?
Is the subtask independent?
Is the expected output clear?
Will delegation improve the result?
```

If unclear, keep the work with the main agent.

---

# Part XVI — Practical Exercises

## 49. Exercise A — Identify Delegatable Tasks

Classify each as single-agent, parallel, sequential, or hybrid, and explain:

1. Fix one button alignment.
2. Review frontend and backend independently.
3. Define API contract, then implement client.
4. Investigate three unrelated bugs.
5. Update one shared configuration file.
6. Audit a large codebase with independent modules.

---

## 50. Exercise B — Create a Delegation Plan

```markdown
# Multi-Agent Delegation Plan
## Main Objective
## Main Agent Responsibilities
## Subagent A
### Objective
### Scope
### Allowed Actions
### Prohibited Actions
### Expected Output
## Subagent B
### Objective
### Scope
### Allowed Actions
### Prohibited Actions
### Expected Output
## Shared Resources
## Dependencies
## Integration Plan
## Verification Criteria
```

---

## 51. Exercise C — Design a GroupTrack Review

Plan a read-only Android UI review covering XML and shared resources, following project instructions and returning one consolidated report. Ensure no overlapping write ownership.

---

## 52. Exercise D — Identify File Conflicts

```text
Agent A: Modify LoginActivity.java
Agent B: Modify LoginActivity.java
Agent C: Modify strings.xml
```

Identify overlapping ownership, possible conflicts, how to reassign work, which tasks can stay parallel, and who verifies integration.

---

## 53. Exercise E — Design a Laravel Workflow

Plan schedule validation review, route geometry review, and cross-module consistency. Only independent reviews run in parallel; cross-module verification follows both reports.

---

## 54. Exercise F — Design a Next.js/Laravel Workflow

Plan frontend and backend analysis, contract comparison, and a minimal correction plan without unapproved edits. Deliver a hybrid diagram and delegation plan.

---

## 55. Exercise G — Evaluate Performance

Hypothetical durations: frontend review 4 min, backend 6 min, database 5 min, integration 3 min.

1. Sequential total?
2. Idealized parallel investigation duration?
3. Idealized total including integration?
4. Why could reality take longer?
5. Why could token usage increase?

Parallelism may reduce elapsed time without eliminating coordination cost.

---

## 56. Exercise H — Integration Checklist

- [ ] Scope completed.
- [ ] Responsibilities respected.
- [ ] No conflicting modifications.
- [ ] Findings have evidence.
- [ ] Contracts validated.
- [ ] Relevant tests executed.
- [ ] Git diff reviewed.
- [ ] Security respected.
- [ ] Unresolved issues documented.
- [ ] Final output verified.

---

# Part XVII — Knowledge Review

## 57. Knowledge Check

1. What is multi-agent collaboration?
2. What is the main agent responsible for?
3. What is a subagent?
4. What is orchestration?
5. What makes a task suitable for delegation?
6. What is task decomposition?
7. What is parallel execution?
8. What is sequential execution?
9. What is hybrid execution?
10. Why do dependencies matter?
11. What is context isolation?
12. Does it guarantee filesystem isolation?
13. What is file ownership?
14. What is the single-writer principle?
15. Why are shared files risky?
16. What are Git worktrees?
17. Do worktrees eliminate logical conflicts?
18. Why is agent communication important?
19. What should a report contain?
20. Who integrates results?
21. Why test integrated changes?
22. What is wall-clock time?
23. Why might multi-agent workflows use more tokens?
24. When prefer one agent?
25. What is agent proliferation?
26. How can agents broaden analysis?
27. How can they add security risk?
28. Why coordinate consequential operations?
29. How delegate Android UI review?
30. How delegate Laravel module review?
31. How investigate Next.js/Laravel mismatch?
32. How measure delegation success?

---

## 58. Key Terminologies

| Term | Definition |
|---|---|
| Multi-Agent Collaboration | Multiple agents contributing to a shared objective |
| Main Agent | Agent responsible for overall coordination |
| Root Agent | Top-level agent in a hierarchy |
| Subagent | Agent assigned a bounded task |
| Orchestration | Coordination of agent activities |
| Delegation | Assigning a task to another agent |
| Task Decomposition | Dividing a task into smaller responsibilities |
| Parallel Execution | Overlapping execution of independent tasks |
| Sequential Execution | Ordered execution based on dependencies |
| Hybrid Workflow | Combination of parallel and sequential stages |
| Context Isolation | Separate working contexts |
| Filesystem Isolation | Separate file environments |
| File Ownership | Responsibility for modifying specific files |
| Single-Writer Principle | One writer per shared resource at a time |
| Integration | Combining individual results |
| Verification | Checking correctness and completion |
| Git Worktree | Additional working directory associated with a repository |
| Coordination Overhead | Resources spent managing collaboration |
| Wall-Clock Time | Actual elapsed workflow time |
| Token Usage | Model input and output consumption |
| Agent Proliferation | Excessive creation of agents |

---

# 59. Key Takeaways

1. **Multi-agent collaboration divides complex tasks into bounded responsibilities.**
2. **The main agent coordinates the overall workflow.**
3. **Subagents handle specific delegated tasks.**
4. **Good delegation requires clear objectives and completion criteria.**
5. **Parallelize independent work; sequence dependent work.**
6. **Hybrid workflows are often practical.**
7. **Context isolation does not guarantee filesystem isolation.**
8. **Shared workspaces need explicit coordination.**
9. **File ownership and single-writer rules reduce conflicts.**
10. **Parallel read-only analysis is a safe starting point.**
11. **Worktrees help isolate files but do not eliminate logical conflicts.**
12. **Subagent findings need evidence; the main agent verifies integration.**
13. **Completed subtasks do not automatically make a completed feature.**
14. **Parallel work may reduce elapsed time but increase token usage.**
15. **Small tasks usually do not need delegation.**
16. **Security permissions still apply to delegated work.**
17. **Choose the simplest workflow that meets requirements.**
18. **Success is measured by verified results, not agent count.**

---

# 60. Official References

## OpenAI Codex and Agents

1. [OpenAI Agents Overview](https://developers.openai.com/api/docs/guides/agents)
2. [Agents API — Multi-Agent](https://developers.openai.com/api/docs/guides/agents-api/multi-agent)
3. [Responses API — Multi-Agent](https://developers.openai.com/api/docs/guides/responses-multi-agent)
4. [Agents SDK — Orchestration and Handoffs](https://developers.openai.com/api/docs/guides/agents/orchestration)
5. [Agents API Overview](https://developers.openai.com/api/docs/guides/agents-api/overview)
6. [Codex Workflow Cookbook](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
7. [Codex CLI and Agents SDK Workflows](https://developers.openai.com/cookbook/examples/codex/codex_mcp_agents_sdk/building_consistent_workflows_codex_cli_agents_sdk)
8. [Using PLANS.md for Multi-Hour Problem Solving](https://developers.openai.com/cookbook/articles/codex_exec_plans)

## Supporting References

9. [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)
10. [Git Merge Documentation](https://git-scm.com/docs/git-merge)
11. [PowerShell Documentation](https://learn.microsoft.com/powershell/)

> **Documentation Note:** Multi-agent behavior differs between Codex, Agents SDK, Agents API, and Responses API. A configuration option in one runtime is not automatically available in another.

---

## 61. Lesson Completion Checklist

- [ ] Understand multi-agent collaboration and single-agent tradeoffs.
- [ ] Understand main-agent, subagent, and orchestration roles.
- [ ] Identify delegatable tasks and define bounded work.
- [ ] Understand parallel, sequential, and hybrid workflows.
- [ ] Understand dependencies and context/filesystem isolation.
- [ ] Define ownership and single-writer rules.
- [ ] Identify shared-file risks and Git worktrees.
- [ ] Communicate structured, evidence-based findings.
- [ ] Integrate and verify results.
- [ ] Understand token and elapsed-time tradeoffs.
- [ ] Identify when not to delegate.
- [ ] Design Android, Laravel, and Next.js workflows.
- [ ] Respect security boundaries and avoid agent proliferation.
- [ ] Complete exercises and knowledge review.

---

# 62. Next Lesson

## [[05 - Codex Automation and Non-Interactive Execution]]

The next lesson explores repeatable non-interactive workflows: `codex exec`, structured outputs, CI/CD concepts, exit codes, error handling, logging, PowerShell scripting, safe automation boundaries, and practical development workflows.

---

*Phase 5 — Advanced Codex | Lesson 5.4*  
*Learning Principle: Delegate independent work, coordinate shared resources, and verify the integrated result before declaring completion.*
