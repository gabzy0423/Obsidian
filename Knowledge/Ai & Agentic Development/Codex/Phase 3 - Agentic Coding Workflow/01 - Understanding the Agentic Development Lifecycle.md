# 01 — Understanding the Agentic Development Lifecycle

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 3 — Agentic Coding Workflow  
> **Lesson:** 3.1 — Understanding the Agentic Development Lifecycle  
> **Difficulty:** Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** Phase 2 — Context Engineering  
> **Learning Approach:** Theory, practical examples, and workflow design

---

## 1. Introduction

Traditional AI-assisted programming often asks a model to generate a snippet that a developer then integrates and tests. An AI coding agent can take part in more of the development task: it can inspect project files, reason about requirements, modify permitted files, run available tools, and report results.

This lesson introduces the **Agentic Development Lifecycle**: a structured way to use an agent while keeping requirements, permissions, engineering judgment, and verification under human oversight.

The course framework is:

**Context → Explore → Plan → Implement → Verify → Review → Report**

This is a practical learning framework, not a mandatory internal sequence used by every Codex task. Simple tasks may combine stages; complex tasks may repeat them.

**Core principle:** Understand the task before changing code, verify work before claiming completion, and stay within the approved scope.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Define agentic software development and the lifecycle used in this course.
- Distinguish code generation from tool-using, multi-step agent work.
- Explain the purpose of each lifecycle stage.
- Describe the agent’s observe–act–evaluate loop.
- Explain how tools, permissions, and human review fit into the workflow.
- Apply the lifecycle to Android and Laravel examples.
- Identify common workflow mistakes and design a focused Codex workflow.

## 3. What Is Agentic Software Development?

**Agentic Software Development** is an approach where an AI agent works toward a defined development objective through multiple steps, potentially using tools to inspect a repository, change files, or run checks.

### Traditional AI-assisted coding

```text
Developer asks for code → Model generates a snippet → Developer integrates and verifies
```

### Agentic coding

```text
Developer defines a task → Agent gathers context and uses permitted tools
→ Agent makes or proposes changes → Results are checked → Developer reviews
```

Agentic does not mean unrestricted. The agent’s actions depend on its available tools, configured permissions, and developer instructions. The developer remains responsible for requirements, consequential decisions, and final acceptance.

## 4. Lifecycle Overview

| Stage | Main question | Typical result |
|---|---|---|
| Context | What goal, constraints, and project guidance apply? | Clear task boundaries |
| Explore | What does the current project actually contain? | Evidence about relevant files and behavior |
| Plan | What is the smallest suitable approach? | Proposed changes and checks |
| Implement | What approved changes should be made? | Focused modifications |
| Verify | What evidence shows the change behaves as expected? | Test/build/check results |
| Review | Are changes correct, maintainable, and in scope? | Accepted changes or findings |
| Report | What happened and what remains uncertain? | Accurate completion summary |

```text
Context → Explore → Plan → Implement → Verify → Review → Report
                      ↑                         │
                      └──── findings or failures ┘
```

The workflow is iterative. New findings can change a plan; a failed check can send work back to investigation or implementation. If requirements conflict or a consequential decision is missing, pause and ask rather than guessing.

## 5. Stage 1 — Context

Context establishes the task’s goal, project background, constraints, and success conditions.

### Questions to answer

- What result is requested?
- Which project and feature are involved?
- Which instructions and documentation apply?
- What should change, and what must stay out of scope?
- How will success be checked?
- Which decisions require human input?

### Sources

- User instructions and acceptance criteria
- Applicable `AGENTS.md` files
- Relevant project documentation
- Existing implementation and repository state
- Decisions from earlier work that still apply

**Example:** For a login-screen alignment task, relevant context might include the Android Java/XML stack, the existing login layout, UI conventions, and the constraint to preserve authentication behavior.

Phase 2 — Context Engineering explains how to organize and retrieve this information. Phase 3 applies it in an actual workflow.

## 6. Stage 2 — Explore

Explore the existing project before proposing significant changes. The aim is to understand actual files and behavior, not to assume how the repository is structured.

Common activities include:

- Inspecting relevant directories and files
- Locating existing functionality and its callers
- Reading relevant tests and documentation
- Checking dependencies and configuration when related to the task
- Inspecting Git status before changes
- Identifying conventions and possible risks

Example PowerShell commands, for later reference:

```powershell
git status
Get-ChildItem
Get-ChildItem -Recurse -Filter "*.java"
git log -5 --oneline
```

### Read-only exploration prompt

> Read applicable project instructions and relevant documentation. Inspect the existing login-screen implementation. Identify relevant files, current behavior, architectural constraints, and likely risks. Do not modify files.

**Principle:** Explore the existing implementation before deciding how to change it. Keep exploration focused; inspect more areas when the task or evidence requires them.

## 7. Stage 3 — Plan

Planning describes a suitable approach before implementation. Plan in proportion to the task: a small, clear fix may need only a short approach, while a multi-file or high-risk change benefits from an explicit plan.

### Planning questions

- What is the problem or desired behavior?
- Which files or components are likely affected?
- Which existing patterns can be reused?
- What is the smallest maintainable change?
- What could regress or require approval?
- Which checks provide useful evidence?

### Example plan

```markdown
# Login UI Alignment Plan

## Objective
Improve spacing and alignment in the existing login screen.

## Likely scope
- Existing login layout
- Existing dimension or style resources, if relevant

## Constraints
- Preserve view IDs and authentication behavior.
- Do not add dependencies or redesign unrelated screens.

## Verification
- Inspect the layout changes.
- Run the available build check.
- Review the Git diff; perform visual review when available.
```

Do not create planning documents for every minor edit. Planning should reduce uncertainty, not add process without value.

## 8. Stage 4 — Implement

Implementation applies the agreed requirements using the existing architecture and conventions.

Keep changes focused, reuse appropriate project components, and avoid new dependencies or abstractions unless the task requires them. Before expanding scope, changing architecture, or making a decision that affects data or external systems, stop for developer input.

For a login-layout adjustment, changing relevant XML constraints or spacing may be in scope. Replacing authentication, adding a UI framework, or rewriting unrelated Activities would not be.

## 9. Stage 5 — Verify

Verification checks requirements against observable evidence. The correct checks depend on the repository and should be confirmed rather than invented.

Possible verification methods:

- Build or compile checks
- Automated tests
- Linting or type checking
- Static analysis
- Manual UI or API checks
- Inspection of test output and logs

Examples for later reference:

```powershell
.\gradlew.bat assembleDebug
.\gradlew.bat test
php artisan test
npm run lint
npm run build
```

Use commands that the project actually supports. For example, `npm run lint` depends on a matching script in `package.json`.

Distinguish what was tested from what was not. “The Android debug build passed; visual behavior was not checked on a device” is more useful than claiming that the app “works perfectly.”

**Principle:** Only claim verification that was actually performed.

## 10. Stage 6 — Review

Review checks whether the changes satisfy the task and remain understandable, maintainable, and within scope.

Useful questions:

- Did only relevant files change?
- Were existing architecture and behavior preserved?
- Are dependencies and abstractions justified?
- Do the changes meet the acceptance criteria?
- What regressions or documentation updates should be considered?

Useful Git commands:

```powershell
git status
git diff --name-only
git diff
git diff --check
```

`git diff` does not show untracked file contents, so inspect newly created files separately. Testing checks specific behavior; review also considers scope, architecture, and maintainability.

## 11. Stage 7 — Report

Report what changed, what was checked, and what remains uncertain. Keep it concise and evidence-based.

```markdown
## Completed
Describe the implemented behavior.

## Files changed
List relevant files.

## Verification
List checks performed and their results.

## Not verified or blocked
State limitations or remaining blockers.
```

A completion report helps the developer evaluate the result and provides useful context for future work. Do not claim a test, build, or device check succeeded unless it actually ran.

## 12. The Agentic Feedback Loop

An agent can repeat a cycle of observing information, choosing an action, and evaluating the result:

```text
Observe → Decide → Act → Evaluate
   ↑                     │
   └── investigate again ┘
```

For example, after a build fails, Codex can inspect the error, find the relevant code, make an authorized correction, and run the check again. If the same failure persists without new evidence, or a fix would exceed the approved scope, stop and report the blocker instead of retrying indefinitely.

## 13. Tools, Permissions, and Human Oversight

Tools may let Codex read files, search a codebase, edit files, use Git, or run build and test commands. The available capabilities depend on the environment.

Codex CLI offers sandbox options such as `read-only` and `workspace-write`; actual behavior also depends on configuration and approval settings. The task prompt does not grant additional technical permissions.

### Human responsibilities

- Define and approve requirements.
- Decide consequential scope or architecture questions.
- Review important changes and evidence.
- Accept, request revision, or reject the result.

### Example approval checkpoints

- A database schema change was not included in the agreed task.
- A new major dependency appears necessary.
- The implementation would modify unrelated modules.
- The agent encounters a destructive or external action.

Human-in-the-loop development means keeping people involved in important decisions and final acceptance. Tool access enables actions; permissions and review help control them.

## 14. Agentic Workflow and the Traditional SDLC

The Software Development Life Cycle (SDLC) describes broader product and engineering work such as requirements, design, implementation, testing, release, and maintenance. The lifecycle in this lesson focuses on how an agent participates in a particular development task.

| Aspect | Traditional SDLC | Agentic workflow in this lesson |
|---|---|---|
| Scope | Product or system delivery | A defined agent-assisted task |
| Planning | Project and system planning | Task-level approach |
| Implementation | Human-led team work | Agent may assist within permissions |
| Verification | Team testing and QA | Agent checks plus developer review |
| Accountability | Shared engineering process | Developer remains responsible for acceptance |

Agentic workflows complement software engineering practices; they do not replace requirements, testing, security, release controls, or maintenance.

## 15. Example — GroupTrack Android Registration UI

**Scenario:** Add a registration screen UI to an Android app using Java and XML, following its existing architecture and approved MVP.

1. **Context:** Review the relevant instructions and registration requirements.
2. **Explore:** Inspect the existing login UI, resources, and navigation patterns.
3. **Plan:** Identify the UI elements, affected files, constraints, and checks.
4. **Implement:** Make the approved UI changes without expanding into an unrelated authentication redesign.
5. **Verify:** Run an available build check and inspect the layout; perform emulator/device review only when available.
6. **Review:** Check IDs, resources, navigation compatibility, and scope.
7. **Report:** Summarize files, checks, and any remaining visual verification.

## 16. Example — Laravel API Validation Bug

**Scenario:** A registration endpoint returns an unexpected validation response.

1. **Context:** Establish the Laravel API conventions and expected behavior.
2. **Explore:** Inspect the route, controller or request validation, related model, and tests relevant to the failure.
3. **Plan:** Use the evidence to identify the likely cause and smallest fix.
4. **Implement:** Change the relevant validation behavior only after the task is clear.
5. **Verify:** Run the relevant tests and check valid and invalid request behavior where possible.
6. **Review:** Inspect the diff and confirm unrelated authentication behavior was not changed.
7. **Report:** Explain the cause, files changed, evidence, and any remaining uncertainty.

The key is to debug from evidence rather than immediately rewriting the subsystem.

## 17. Example Codex CLI Workflow

These commands and prompts are for study or a future practice session; no project work needs to be run for this lesson.

Navigate to a project and inspect its state:

```powershell
Set-Location C:\Projects\GroupTrack
git status
```

Start with read-only permissions and investigate:

```powershell
codex --sandbox read-only
```

> Read the applicable project instructions and relevant documentation. Inspect the existing login screen. Identify the current implementation, relevant files, and smallest suitable improvement. Include risks and verification steps. Do not modify files.

After reviewing a plan and authorizing implementation, a write-capable session can use a sandbox policy such as:

```powershell
codex --sandbox workspace-write
```

The sandbox policy and approval behavior are separate settings; check the installed CLI help and current configuration before using them. After approved changes, review the diff and run project-supported checks.

## 18. Common Workflow Mistakes

| Mistake | Better approach |
|---|---|
| Implementing before understanding the code | Explore the relevant implementation first |
| Skipping planning for a complex task | Plan in proportion to complexity and risk |
| Expanding scope or overengineering | Follow acceptance criteria and existing conventions |
| Trusting generated code without checks | Verify behavior with relevant evidence |
| Ignoring existing Git changes | Inspect repository status before work |
| Repeating the same failed attempt | Reassess evidence or report a blocker |
| Claiming unperformed verification | State exactly what did and did not run |
| Continuing after acceptance criteria are met | Stop when complete or when developer input is needed |

## 19. Practice Exercises

These exercises are for later study; no coding is required now.

### Exercise A — Identify lifecycle stages

Classify each action as Context, Explore, Plan, Implement, Verify, Review, or Report:

1. Read applicable `AGENTS.md` instructions.
2. Inspect the existing login Activity and layout.
3. Propose a focused fix.
4. Modify the relevant Java code.
5. Run the Android build.
6. Inspect the Git diff.
7. Summarize the result and limitations.

### Exercise B — Design a Laravel workflow

For adding search to a product API, define relevant context, files to inspect, scope boundaries, plan, verification, review criteria, and completion report. Assume no implementation until the requirements are clear.

### Exercise C — Read-only exploration prompt

Draft a prompt to explore an existing project and propose a small improvement without changing files. Include applicable instructions, files to inspect, risks, acceptance criteria, and verification steps.

## 20. Knowledge Check

1. What is agentic software development?
2. How does it differ from asking for a code snippet?
3. What are the seven stages in this course’s lifecycle?
4. Why should codebase exploration precede significant changes?
5. When is an explicit plan useful?
6. What should guide implementation scope?
7. How do verification and review differ?
8. Why should reports state limitations?
9. What is the observe–act–evaluate feedback loop?
10. How do tools and sandbox permissions relate?
11. What decisions should involve developer oversight?
12. When should an agent stop and report a blocker?

## 21. Key Terminologies

| Term | Definition |
|---|---|
| Agentic coding | AI-assisted development with tool use and multi-step tasks |
| Agentic Development Lifecycle | The task workflow framework used in this lesson |
| Context | Relevant task, project, and constraint information |
| Codebase exploration | Evidence-gathering about existing project files and behavior |
| Implementation plan | Proposed changes and checks for a task |
| Verification | Checking behavior or requirements using evidence |
| Code review | Evaluating correctness, scope, architecture, and maintainability |
| Feedback loop | Repeated observation, action, and evaluation |
| Human-in-the-loop | Human participation in important decisions and acceptance |
| Approval checkpoint | A point where developer input is needed before proceeding |
| Regression | Previously working behavior that becomes broken |
| Stop condition | A condition requiring the agent to pause or report a blocker |

## 22. Key Takeaways

1. Agentic coding lets an AI agent participate in multi-step development tasks using available tools.
2. The course lifecycle provides a clear way to guide and review that work.
3. Context and exploration ground decisions in requirements and existing code.
4. Planning should match task complexity and risk.
5. Implementation stays within approved scope and established architecture.
6. Verification and review provide different, complementary evidence.
7. Reports should distinguish completed checks from unverified behavior.
8. The agent can iterate, but should stop when it needs a decision or reaches a blocker.
9. Developers remain responsible for consequential decisions and final acceptance.

## Official References

- [OpenAI Codex CLI](https://developers.openai.com/codex/cli/)
- [OpenAI Codex CLI command reference](https://developers.openai.com/codex/cli/reference/)
- [OpenAI Cookbook — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
- [OpenAI Cookbook — Codex Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide)
- [Android Developers — Guide to App Architecture](https://developer.android.com/topic/architecture)
- [Laravel — Testing](https://laravel.com/docs/12.x/testing)
- [Git Documentation](https://git-scm.com/doc)

## Lesson Completion Checklist

- [ ] Understand agentic software development.
- [ ] Explain the seven lifecycle stages.
- [ ] Understand context, exploration, and planning.
- [ ] Understand implementation scope and verification.
- [ ] Distinguish verification from review.
- [ ] Explain the feedback loop and stop conditions.
- [ ] Understand tools, permissions, and human oversight.
- [ ] Review the Android and Laravel examples.
- [ ] Complete the practice exercises and knowledge check.

## Next Lesson

**[[02 - Codebase Exploration and Analysis]]**

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.1*  
*Learning Principle: Understand the context, explore the system, plan deliberately, implement within scope, verify with evidence, and report accurately.*
