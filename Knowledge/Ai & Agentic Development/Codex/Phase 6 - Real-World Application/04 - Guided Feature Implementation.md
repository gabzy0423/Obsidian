# 04 — Guided Feature Implementation

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 6 — Real-World Application
> **Lesson:** 6.4 — Guided Feature Implementation
> **Difficulty:** Intermediate to Advanced
> **Learning Mode:** Theory First
> **Prerequisite:** [[03 - Feature Planning with Codex]]
> **Next Lesson:** [[05 - Debugging, Testing, and Quality Assurance]]
> **Status:** Theory Documented

---

# 1. Introduction

After completing project selection, repository preparation, and feature planning, the next stage of agentic software development is **guided feature implementation**.

Guided feature implementation is the process of executing an approved development plan with an AI coding agent while maintaining human oversight, architectural consistency, and verifiable progress.

The objective is not simply to ask Codex to generate code. Instead, the developer coordinates a structured workflow in which Codex:

1. Understands the approved task.
2. Inspects the relevant implementation.
3. Follows established project instructions.
4. Modifies only necessary components.
5. Performs appropriate verification.
6. Reports changes and limitations.
7. Stops when the approved task is complete.

This approach is especially important when developing applications with multiple components, external integrations, and existing business rules.

## Core Principle

**Agentic implementation should be incremental, scoped, reviewable, and verified—not a single uncontrolled request to build an entire feature or application.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Explain guided feature implementation.
2. Understand the transition from planning to execution.
3. Perform repository preflight checks.
4. Recognize the importance of working-tree safety.
5. Prepare implementation-ready Codex prompts.
6. Apply incremental development.
7. Execute tasks in an appropriate sequence.
8. Maintain architectural boundaries.
9. Understand controlled file modifications.
10. Evaluate dependency changes.
11. Apply test-driven development concepts.
12. Perform build and test verification.
13. Understand Git diff reviews.
14. Recognize unexpected changes.
15. Handle implementation failures.
16. Use checkpoints and approval gates.
17. Understand implementation rollback.
18. Avoid scope drift.
19. Manage incomplete implementation.
20. Distinguish implementation from verification.
21. Apply concepts to Android development.
22. Apply concepts to Laravel development.
23. Apply concepts to Next.js development.
24. Prepare implementation reports.
25. Establish a reliable handoff to quality assurance.

---

# Part II — Understanding Guided Implementation

## 3. What Is Guided Feature Implementation?

Guided feature implementation is a development approach in which a human developer directs an AI coding agent through bounded implementation tasks.

The human remains responsible for:

- Approving requirements.
- Establishing scope.
- Reviewing technical decisions.
- Authorizing significant changes.
- Evaluating verification evidence.
- Accepting or rejecting completed work.

Codex assists with:

- Repository exploration.
- Code generation and modification.
- Test creation.
- Error investigation.
- Running permitted development commands.
- Summarizing implementation results.

### Conceptual Workflow

```text
Approved Feature Plan
          |
          v
Repository Preflight
          |
          v
Select One Task
          |
          v
Provide Relevant Context
          |
          v
Codex Implementation
          |
          v
Inspect Changes
          |
          v
Run Verification
          |
          v
Review Results
          |
     +----+----+
     |         |
   Pass       Fail
     |         |
     v         v
Checkpoint   Investigate
     |         |
     |         +----> Fix and Reverify
     |
     v
Next Approved Task
```

### Important Principle

The implementation process should preserve visibility into what Codex is changing and why.

---

## 4. Planning vs Implementation

| Planning | Implementation |
|---|---|
| Determines what should change | Makes approved changes |
| Identifies affected files | Edits necessary files |
| Defines acceptance criteria | Implements expected behavior |
| Identifies dependencies | Uses or integrates dependencies |
| Proposes verification | Executes verification |
| Identifies risks | Manages risks during execution |
| Requires approval where appropriate | Operates within approved boundaries |

### Example

**Planning instruction:**

> Analyze the existing sign-in screen and propose a UI implementation plan without modifying files.

**Implementation instruction:**

> Implement the approved sign-in XML layout using the existing Android project structure. Modify only relevant UI resources and navigation code. Do not change authentication behavior.

### Key Principle

**Do not confuse permission to prepare a plan with permission to implement it.**

---

# Part III — Implementation Readiness

## 5. What Must Exist Before Implementation?

Before starting a feature, confirm:

- Approved requirement.
- Defined feature scope.
- Explicit non-goals.
- Acceptance criteria.
- Relevant architecture documentation.
- Known dependencies.
- Implementation task breakdown.
- Verification strategy.
- Appropriate repository access.

### Readiness Checklist

```markdown
## Implementation Readiness

- [ ] Requirement is approved.
- [ ] Feature scope is defined.
- [ ] Non-goals are documented.
- [ ] Relevant architecture is understood.
- [ ] Dependencies are identified.
- [ ] Acceptance criteria are testable.
- [ ] Repository status is understood.
- [ ] Verification commands are known.
- [ ] Required approvals are available.
```

### Important Note

Not every minor change requires a formal document. The level of planning should be proportional to the complexity and risk of the task.

---

# Part IV — Repository Preflight

## 6. What Is Repository Preflight?

Repository preflight is the inspection performed immediately before modifying a codebase. Its purpose is to confirm the current repository state and avoid unintended changes.

### Preflight Workflow

```text
Confirm Repository
       |
       v
Inspect Git Status
       |
       v
Identify Current Branch
       |
       v
Read Agent Instructions
       |
       v
Inspect Relevant Files
       |
       v
Confirm Baseline
       |
       v
Begin Approved Task
```

---

## 7. Essential Git Commands

### Identify Repository Root

```powershell
git rev-parse --show-toplevel
```

### Inspect Working Tree

```powershell
git status
```

### Identify Branch

```powershell
git branch --show-current
```

### Inspect Existing Changes

```powershell
git diff
```

### Inspect Staged Changes

```powershell
git diff --cached
```

### Inspect Recent Commits

```powershell
git log --oneline -5
```

If files already contain uncommitted changes, Codex must avoid overwriting unrelated work.

### Key Principle

**Pre-existing changes belong to the working tree and must be preserved unless the user explicitly authorizes modifying them.**

---

## 8. Clean vs Dirty Working Tree

### Clean Working Tree

No tracked changes are pending, and no untracked files are present. This makes new modifications easier to identify.

### Dirty Working Tree

The repository contains staged, unstaged, or untracked changes. This does not automatically mean work must stop. Codex should:

1. Identify existing changes.
2. Determine whether they overlap with the task.
3. Preserve unrelated changes.
4. Avoid destructive cleanup.
5. Ask for direction if conflicts cannot be resolved safely.

Never use destructive Git commands merely to create a clean working tree.

---

# Part V — Context Preparation

## 9. Relevant Context for Implementation

An implementation task may require:

- `AGENTS.md`.
- Feature specification.
- Architecture documentation.
- Coding standards.
- Relevant source files.
- Existing tests.
- API contracts.
- Database schema.
- Approved design references.

### Context Hierarchy

```text
Project-Level Instructions
          |
          v
Feature Requirements
          |
          v
Relevant Architecture
          |
          v
Existing Implementation
          |
          v
Acceptance Criteria
          |
          v
Verification Commands
```

**Implementation context should be sufficient for correctness without unnecessarily including the entire repository.**

---

## 10. Progressive Context Discovery

Instead of loading every file:

1. Identify the feature entry point.
2. Inspect directly related components.
3. Follow relevant dependencies.
4. Read tests for expected behavior.
5. Expand investigation when evidence requires it.

### Example

For a Laravel product creation feature:

```text
Route
  |
  v
Controller
  |
  v
Validation
  |
  v
Service / Model
  |
  v
Database
```

Codex should inspect the actual project structure rather than assume this exact layering exists.

---

# Part VI — Scoped Implementation Prompts

## 11. What Is a Scoped Implementation Prompt?

A scoped implementation prompt defines a specific task and its operating boundaries.

A strong prompt identifies:

- Objective.
- Relevant requirement.
- Allowed scope.
- Explicit exclusions.
- Existing architecture.
- Acceptance criteria.
- Verification.
- Expected report.

---

## 12. General Implementation Prompt

```text
Task:
Implement the approved feature described below.

Feature:
[Feature name]

Requirement:
[Requirement ID and description]

Acceptance Criteria:
[Approved criteria]

Context:
Read applicable AGENTS.md instructions
and relevant project documentation.

Before Editing:
1. Inspect Git status.
2. Inspect relevant existing code.
3. Identify existing patterns.
4. Confirm the required changes.

Implementation Rules:
- Follow existing architecture.
- Modify only necessary files.
- Preserve unrelated changes.
- Do not add unapproved features.
- Do not introduce unnecessary dependencies.
- Do not invent business rules.
- Do not expose secrets.
- Stop if a critical requirement is unresolved.

Verification:
Run relevant tests and build checks
that are available and permitted.

Final Report:
- Summary of changes
- Files modified
- Tests executed
- Results
- Remaining issues
- Assumptions
- Unverified behavior
```

**A good implementation prompt defines both the intended outcome and the limits of acceptable change.**

---

# Part VII — Incremental Development

## 13. What Is Incremental Development?

Incremental development delivers functionality through a sequence of smaller changes. Instead of implementing everything at once, developers complete and verify manageable units.

### Example

```text
Feature Plan
     |
     v
Task 1: Prepare UI
     |
     v
Verify UI
     |
     v
Task 2: Connect Logic
     |
     v
Verify Behavior
     |
     v
Task 3: Handle Errors
     |
     v
Verify Failure Cases
     |
     v
Complete Feature Review
```

---

## 14. Selecting the Next Task

Choose one task that is small enough to understand, implement, and verify. Prefer tasks that unblock later work or reduce a significant uncertainty.

Before starting a task, identify:

1. Its requirement and acceptance criteria.
2. The work it depends on.
3. The files and components likely involved.
4. The observable behavior to verify.
5. Any decision that still needs human input.

Do not let Codex automatically begin the next task when the current task is complete. Review the result and explicitly select the next approved unit of work.

---

## 15. Vertical and Horizontal Slicing

### Vertical Slice

A vertical slice implements one small user-visible path across the necessary layers.

```text
UI -> Application Logic -> API -> Persistence -> Verification
```

For example, create one product and show the saved product in the list.

### Horizontal Slice

A horizontal slice completes one layer across a broader area, such as building all database tables before implementing any user workflow.

Horizontal work can be appropriate when architecture or infrastructure must be established first, but it may delay end-to-end feedback.

### Choosing a Slice

- Prefer a small vertical slice when it can validate the main user workflow early.
- Use horizontal tasks when a real dependency or technical prerequisite requires them.
- Keep each slice tied to approved requirements.

---

# Part VIII — Architecture and Change Boundaries

## 16. Maintain Architectural Boundaries

Existing architecture often encodes decisions about data flow, authorization, validation, and ownership.

Before changing a component, understand:

- Its responsibility.
- What calls it.
- What it calls.
- Which data contracts it uses.
- Which tests cover it.
- Whether similar behavior exists elsewhere.

Codex should follow established patterns unless the approved task includes an architectural change or the existing pattern prevents the required behavior. In that case, it should explain the evidence and proposed impact before broadening scope.

---

## 17. Controlled File Modifications

The prompt may name expected files, but implementation can reveal that another file is necessary. Codex should explain any meaningful scope change and avoid unrelated edits.

During review, check:

- Whether each changed file supports the approved feature.
- Whether generated or temporary files were added accidentally.
- Whether configuration or lock files changed for a justified reason.
- Whether unrelated user changes remain intact.
- Whether any files were deleted or renamed unexpectedly.

### Useful Commands

```powershell
git status --short
git diff --stat
git diff --name-only
```

---

## 18. Evaluating Dependency Changes

Adding a library changes the project's dependency surface and may affect licensing, security, build size, compatibility, and maintenance.

Before accepting a dependency change, ask:

1. Is the capability already available in the project or platform?
2. Is the dependency required by an approved requirement?
3. Is it compatible with the current stack?
4. Does it add transitive packages or configuration changes?
5. Can the project maintain and update it?
6. Were lockfiles and tests updated appropriately?

Do not approve a dependency merely because it makes a small implementation shorter.

---

# Part IX — Test-Driven and Verification-Oriented Work

## 19. Test-Driven Development Concepts

Test-Driven Development (TDD) commonly uses a short cycle:

```text
Red -> Green -> Refactor
```

- **Red:** Add or identify a test that demonstrates the required behavior and fails for the current implementation.
- **Green:** Make the smallest implementation that satisfies the test.
- **Refactor:** Improve the implementation while keeping the test passing.

TDD is useful when the behavior can be expressed reliably in automated tests. Some visual, hardware-dependent, exploratory, or integration behavior may require other verification methods.

### Important Principle

Do not claim a test passed unless it was actually run and its result was observed.

---

## 20. Choosing Verification Checks

Verification should match the task and project conventions. Possible checks include:

- Focused unit or feature tests.
- Static analysis or linting.
- Type checking.
- Build or compilation.
- Integration tests.
- UI or emulator testing.
- Manual inspection of a documented workflow.

Start with the most relevant focused check, then run broader checks when the change or project risk warrants them.

If a check cannot run, report the exact reason and what remains unverified. Do not replace missing verification with a statement that the feature is correct.

---

## 21. Implementation Is Not Verification

These are separate states:

```text
Implemented != Verified != Accepted
```

- **Implemented:** Code or project files were changed.
- **Verified:** Relevant checks or observations produced evidence.
- **Accepted:** The responsible human reviewed the evidence and approved the result.

A successful build does not prove that every acceptance criterion is satisfied. A passing unit test does not prove that a real device layout, external API, or production configuration behaves correctly.

---

# Part X — Review, Checkpoints, and Recovery

## 22. Review the Git Diff

After implementation, review the exact change set:

```powershell
git status --short
git diff --check
git diff
```

For staged changes, also inspect:

```powershell
git diff --cached
```

Check that the diff:

- Satisfies the approved requirement.
- Does not introduce unrelated changes.
- Preserves existing user work.
- Includes necessary tests and documentation.
- Does not expose secrets or local environment data.
- Does not contain generated artifacts that should be excluded.

---

## 23. Checkpoints and Approval Gates

Use a human checkpoint when a task involves material uncertainty, new dependencies, architectural changes, sensitive data, external systems, database migrations, or a change beyond the approved scope.

At a checkpoint, Codex should summarize:

- What it found.
- What it proposes to change.
- Why the change is needed.
- Files or systems affected.
- Verification plan.
- Risks and unresolved decisions.

The developer can then approve, revise, or stop the work. A checkpoint should be tied to a real decision; adding approval pauses to every trivial edit can make the workflow less effective without improving review.

---

## 24. Handling Failures

When implementation or verification fails:

1. Preserve the current working tree.
2. Capture the exact error and command.
3. Identify whether the failure is caused by code, environment, dependency access, or test setup.
4. Compare the failure with the baseline when available.
5. Propose the smallest correction.
6. Re-run the focused check after a change.
7. Report remaining failures honestly.

Do not hide failing checks, delete tests to make a run pass, or change unrelated configuration without approval.

---

## 25. Incomplete Implementation

A feature can remain incomplete because of unresolved requirements, unavailable services, environment limitations, failing tests, or insufficient time.

When work is incomplete, report:

- Completed portion.
- Remaining portion.
- Files already changed.
- Checks run and results.
- Blocking dependency or decision.
- Safe next step.

Do not mark a task complete merely because some code was written.

---

## 26. Rollback and Recovery

Rollback restores an earlier state after a change proves incorrect or unsafe. Recovery should preserve unrelated work.

Preferred recovery process:

1. Identify the specific unwanted change in the diff.
2. Determine whether it is newly introduced or pre-existing.
3. Restore only the affected work when safe.
4. Re-check Git status and the diff.
5. Re-run relevant verification if the codebase changed.

Avoid broad reset or clean operations that can discard uncommitted or untracked user files. If the correct recovery boundary is unclear, stop and ask for guidance.

---

# Part XI — Real-World Development Examples

## 27. Android Example — GroupTrack UI Feature

### Scenario

Implement an approved sign-in screen alignment adjustment in an Android application that uses Java and XML layouts.

### Guided Workflow

1. Read `AGENTS.md` and the approved UI requirements.
2. Inspect the current layout, related resources, and navigation behavior.
3. Check `git status` and preserve pre-existing changes.
4. Confirm that the requested work does not alter authentication logic.
5. Update only necessary layout or resource files.
6. Run the relevant Gradle check if the environment supports it.
7. Review the diff and verify the screen in an emulator or device where available.
8. Report any device-specific behavior that was not checked.

### Example Prompt

```text
Implement the approved sign-in screen alignment changes
from the feature plan.

Read applicable AGENTS.md instructions and inspect the
existing Java/XML implementation before editing.

Preserve authentication behavior. Modify only the files
needed for the approved layout change. Do not add a
dependency or change unrelated screens.

Run the relevant available Gradle verification, inspect
the Git diff, and report files changed, observed results,
and anything that could not be verified on a device.
```

---

## 28. Laravel Example — API Validation Feature

### Scenario

Implement an approved validation correction in a Laravel API.

### Guided Workflow

1. Read project instructions and the approved business rule.
2. Inspect the route, controller, validation, service or model, and existing tests.
3. Check the working tree before editing.
4. Add or update a focused test for the accepted behavior where practical.
5. Make the smallest code change that satisfies the requirement.
6. Run the relevant Laravel test in the configured test environment.
7. Review migrations and database effects separately.
8. Inspect the Git diff and report any test or environment limitations.

### Example Prompt

```text
Implement only the approved API validation requirement.

Read AGENTS.md and inspect the existing route, controller,
validation rules, and related tests. Preserve existing
authorization and response conventions.

Do not run destructive database commands or change
unrelated schema. Add or update the focused test if the
project's test setup supports it. Run the relevant test,
review the diff, and report observed results and limitations.
```

### Important Principle

Workspace write access does not make database operations safe. Use an isolated test database and review migrations before execution.

---

## 29. Next.js Example — Product Data Mapping

### Scenario

Correct an approved frontend mapping between the API response and a Next.js product listing.

### Guided Workflow

1. Read the frontend instructions and API contract.
2. Inspect the page or component and related types.
3. Check whether the backend response is authoritative and current.
4. Add or update a focused test where the project has a suitable test pattern.
5. Make the narrow mapping correction.
6. Run relevant lint, type-check, tests, or build scripts listed by the project.
7. Review the diff and inspect the UI where practical.

### Example Prompt

```text
Implement the approved product response mapping fix.

Read AGENTS.md, the API contract, and the existing product
listing implementation. Preserve current component and
styling conventions. Do not change the backend contract,
add dependencies, or redesign the page.

Run the focused available checks, inspect the diff, and
report the files changed and any behavior not verified.
```

### Important Principle

Use the scripts declared by the repository instead of assuming that every Next.js project has identical lint, test, or build commands.

---

# Part XII — Implementation Handoff

## 30. What Is an Implementation Handoff?

An implementation handoff gives the next reviewer or QA activity enough evidence to continue without guessing.

### Handoff Template

```markdown
## Implementation Handoff

### Requirement
- ID and approved description:

### Changes
- Summary:
- Files changed:

### Verification
- Commands or manual checks run:
- Observed results:

### Remaining Work
- Known issues:
- Unresolved decisions:
- Unverified behavior:

### Review Notes
- Scope deviations:
- Dependency or data changes:
- Recommended next step:
```

Do not invent test results, device coverage, or external-service behavior. Report only observed evidence.

---

## 31. Final Implementation Review

Before handing off, confirm:

- The change maps to an approved requirement.
- The acceptance criteria have corresponding evidence.
- Only relevant files were changed.
- Existing work was preserved.
- Tests or checks and their results are recorded accurately.
- Known limitations and unresolved issues are visible.
- Git diff has been reviewed.
- No commit, push, deployment, or external write occurred without authorization.

---

# Part XIII — Common Mistakes

## 32. Asking Codex to Build Too Much at Once

### Problem

A broad request hides intermediate decisions and makes review difficult.

### Improvement

Break the feature into independently reviewable tasks with observable outcomes.

---

## 33. Starting Without Checking Git Status

### Problem

Codex may overwrite or mix changes with work that already existed.

### Improvement

Inspect status and diffs before editing, then preserve unrelated changes.

---

## 34. Treating the Plan as Authorization

### Problem

An implementation plan may contain options or assumptions that have not been approved.

### Improvement

Confirm the chosen approach, scope, and permission boundary before implementation.

---

## 35. Letting the Agent Expand Scope

### Problem

Helpful enhancements can introduce new requirements, dependencies, and review burden.

### Improvement

Keep optional suggestions separate from the approved implementation.

---

## 36. Accepting Changes Without Reviewing the Diff

### Problem

A summary can omit unexpected or unrelated edits.

### Improvement

Review the actual diff and changed-file list.

---

## 37. Treating a Passing Build as Complete Proof

### Problem

A build may succeed while acceptance criteria, authorization rules, or visual behavior remain wrong.

### Improvement

Match verification to the feature and document untested behavior.

---

## 38. Removing Failing Tests or Weakening Checks

### Problem

This can conceal a regression instead of fixing its cause.

### Improvement

Investigate the failure and change tests only when the approved requirement or behavior has changed.

---

# Part XIV — Practical Exercises

## 39. Exercise A — Repository Preflight

### Objective

Perform a preflight before an approved feature task.

### Commands

```powershell
git rev-parse --show-toplevel
git status
git branch --show-current
git diff --stat
```

### Questions

1. Which repository and branch are active?
2. Are there existing changes?
3. Do any changes overlap with the task?
4. Which instructions and feature documents apply?

---

## 40. Exercise B — Scope an Implementation Prompt

### Objective

Turn an approved feature plan into a bounded implementation request.

Include:

- Requirement ID.
- Acceptance criteria.
- Relevant context.
- Allowed changes.
- Non-goals.
- Verification commands.
- Reporting requirements.

Do not add a feature that is absent from the approved plan.

---

## 41. Exercise C — Break Work into Increments

### Objective

Divide one feature into small implementation tasks.

For each task, record:

| Field | Description |
|---|---|
| Task | One bounded change |
| Dependency | What must be ready first |
| Acceptance | Observable expected result |
| Verification | Evidence to collect |
| Review point | Decision needed before continuing |

---

## 42. Exercise D — Review a Diff

### Objective

Review an implementation before accepting it.

### Commands

```powershell
git status --short
git diff --check
git diff --name-only
git diff
```

### Questions

1. Does every changed file serve the approved task?
2. Are user changes preserved?
3. Are dependencies and configuration changes justified?
4. Are tests included where appropriate?
5. Are there secrets, generated files, or unrelated edits?

---

## 43. Exercise E — Handle a Failed Check

### Objective

Respond safely when a verification command fails.

Document:

1. Exact command.
2. Exact error or failure output.
3. Whether the failure existed before the change.
4. Likely cause and evidence.
5. Smallest proposed correction.
6. Retest result or remaining blocker.

---

## 44. Exercise F — Android Implementation Workflow

Design a workflow for an approved GroupTrack UI change.

Requirements:

- Inspect applicable instructions and current layout.
- Preserve authentication behavior.
- Make a narrow change.
- Run an available Gradle check.
- Review the diff.
- State whether emulator or device verification occurred.

---

## 45. Exercise G — Laravel Implementation Workflow

Design a workflow for an approved API validation change.

Requirements:

- Inspect routes, validation, business logic, and relevant tests.
- Preserve authorization behavior.
- Use a safe test environment.
- Avoid unapproved database operations.
- Run a focused test and review changes.

---

## 46. Exercise H — Next.js Implementation Workflow

Design a workflow for an approved frontend mapping fix.

Requirements:

- Inspect the API contract and current components.
- Preserve existing UI conventions.
- Avoid changing the backend contract.
- Run checks declared by the project.
- Review the diff and report UI verification limits.

---

# Part XV — Knowledge Review

## 47. Knowledge Check

1. What is guided feature implementation?
2. How does implementation differ from planning?
3. What should be confirmed before implementation begins?
4. What is repository preflight?
5. Why should Git status be checked before editing?
6. How should pre-existing changes be handled?
7. What is progressive context discovery?
8. What makes an implementation prompt scoped?
9. What is incremental development?
10. What is a vertical slice?
11. When might a horizontal slice be useful?
12. Why should existing architecture be inspected?
13. How should dependency changes be evaluated?
14. What are the red, green, and refactor steps?
15. When might automated tests be insufficient?
16. How should verification be selected?
17. How do implementation, verification, and acceptance differ?
18. Why is Git diff review necessary?
19. When are approval checkpoints useful?
20. How should implementation failures be handled?
21. What should an incomplete implementation report include?
22. Why should broad rollback commands be avoided?
23. What evidence belongs in an implementation handoff?
24. How can Codex help with Android development?
25. How can Codex help with Laravel development?
26. How can Codex help with Next.js development?
27. Why should an agent stop after the approved task?

---

## 48. Key Terminologies

| Term | Definition |
|---|---|
| Guided Implementation | Human-directed, bounded AI-assisted development |
| Repository Preflight | Inspection of repository state before changes |
| Working Tree | Current tracked, staged, and untracked repository state |
| Scoped Prompt | Task request with clear objective and boundaries |
| Incremental Development | Delivering a feature through smaller verifiable changes |
| Vertical Slice | Small user workflow implemented across necessary layers |
| Horizontal Slice | Work focused on one technical layer across broader scope |
| Acceptance Criteria | Conditions required for a feature to be accepted |
| TDD | Test-Driven Development |
| Red-Green-Refactor | Test-driven cycle of failing test, implementation, and improvement |
| Verification | Checks or observations that provide evidence of behavior |
| Git Diff | Representation of changes between repository states |
| Checkpoint | Review point before continuing with consequential work |
| Rollback | Reversal of a specific change to restore an earlier state |
| Handoff | Summary and evidence enabling the next review or QA step |
| Scope Drift | Changes that extend beyond the approved requirement |

---

## 49. Key Takeaways

1. **Guided implementation turns an approved plan into controlled code changes.**
2. **Planning and implementation are separate activities.**
3. **Check repository status before editing.**
4. **Preserve pre-existing and unrelated work.**
5. **Give Codex the relevant context, not an indiscriminate dump of the repository.**
6. **Write prompts with explicit outcomes, boundaries, and verification.**
7. **Implement one manageable increment at a time.**
8. **Prefer vertical slices when they provide early end-to-end feedback.**
9. **Follow existing architecture unless an approved change requires otherwise.**
10. **Evaluate every dependency addition.**
11. **Use tests and checks appropriate to the behavior being changed.**
12. **Implementation is not proof of correctness.**
13. **Review the actual Git diff, not only the agent's summary.**
14. **Use human checkpoints for material decisions and risks.**
15. **Report failures, incomplete work, and unverified behavior accurately.**
16. **Recover narrowly and avoid destructive cleanup.**
17. **Do not allow Codex to expand scope or begin later tasks without approval.**
18. **A clear handoff connects implementation to quality assurance.**

---

# 50. Official References

1. **OpenAI — Iterating Development Workflows with Codex**
   https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex

2. **OpenAI — Codex Documentation**
   https://developers.openai.com/codex/

3. **OpenAI — Codex CLI Reference**
   https://developers.openai.com/codex/cli/reference/

4. **OpenAI — Codex Security**
   https://developers.openai.com/codex/security/

5. **Git Documentation**
   https://git-scm.com/docs

6. **Android Developers**
   https://developer.android.com/

7. **Laravel Documentation**
   https://laravel.com/docs

8. **Next.js Documentation**
   https://nextjs.org/docs

> **Documentation Note:** Codex capabilities and CLI behavior may change. Check the installed version's help and the current official documentation when exact commands, permissions, or behavior matter. Repository workflow files and prompt conventions are project choices; they are not all required Codex files.

---

## 51. Lesson Completion Checklist

- [ ] Understand guided feature implementation.
- [ ] Distinguish planning from implementation.
- [ ] Confirm implementation readiness.
- [ ] Perform repository preflight.
- [ ] Inspect and preserve existing Git changes.
- [ ] Gather relevant project context.
- [ ] Write scoped implementation prompts.
- [ ] Break work into increments.
- [ ] Understand vertical and horizontal slices.
- [ ] Preserve architectural boundaries.
- [ ] Review file and dependency changes.
- [ ] Apply TDD concepts where appropriate.
- [ ] Run relevant verification.
- [ ] Review the Git diff.
- [ ] Use appropriate approval checkpoints.
- [ ] Handle failures and incomplete work accurately.
- [ ] Understand narrow rollback and recovery.
- [ ] Prepare an implementation handoff.
- [ ] Apply the workflow to Android, Laravel, and Next.js examples.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

# 52. Next Lesson

## [[05 - Debugging, Testing, and Quality Assurance]]

The next lesson focuses on using Codex to investigate defects, run tests, evaluate failures, and verify software quality.

Topics include:

- Reproducing bugs.
- Reading error messages and logs.
- Forming and testing hypotheses.
- Unit, feature, integration, and UI tests.
- Regression testing.
- Test environment safety.
- Debugging Android applications.
- Debugging Laravel applications.
- Debugging Next.js applications.
- Reviewing verification evidence.
- Reporting limitations and residual risks.

---

*Phase 6 — Real-World Application | Lesson 6.4*
*Learning Principle: Implement one approved increment at a time, inspect the change, verify its behavior, and report exactly what the evidence supports.*
