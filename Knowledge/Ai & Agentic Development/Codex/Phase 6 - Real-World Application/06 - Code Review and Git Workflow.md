# 06 — Code Review and Git Workflow

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 6 — Real-World Application  
> **Lesson:** 6.6 — Code Review and Git Workflow  
> **Difficulty:** Intermediate to Advanced  
> **Learning Mode:** Theory First  
> **Prerequisite:** [[05 - Debugging, Testing, and Quality Assurance]]  
> **Next Lesson:** [[07 - Final Project Evaluation]]  
> **Status:** Theory Documented

---

# 1. Introduction

After implementing, debugging, and testing a feature, the next responsibility is to review the changes and manage them safely through version control.

An AI coding agent may produce working code, but that does not automatically mean the changes are correct, secure, maintainable, consistent with project architecture, within the approved scope, or ready to merge.

Code review evaluates the quality and correctness of proposed changes. Git provides a structured way to track, organize, compare, share, and recover code changes. Together, they help developers maintain control over AI-assisted software development.

## Core Principle

**AI-generated code should be treated as a proposed contribution that must be inspected, verified, and approved before becoming part of the project's trusted codebase.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Explain the purpose of code review.
2. Understand the responsibilities of a reviewer.
3. Review Codex-generated changes critically.
4. Inspect Git repository status.
5. Understand Git's three main states.
6. Read Git diffs.
7. Review staged and unstaged changes.
8. Apply selective staging.
9. Create meaningful commits.
10. Understand branching strategies.
11. Explain pull request workflows.
12. Evaluate pull request descriptions.
13. Understand Git merge conflicts.
14. Explain Git worktrees.
15. Coordinate multiple coding agents.
16. Identify security issues during review.
17. Identify architectural violations.
18. Evaluate test changes.
19. Detect unnecessary dependencies.
20. Understand rollback strategies.
21. Distinguish revert, restore, and reset.
22. Understand branch protection.
23. Apply review concepts to Android.
24. Apply review concepts to Laravel.
25. Apply review concepts to Next.js.
26. Prepare a review-ready development handoff.

---

# Part II — Code Review Fundamentals

## 3. What Is Code Review?

Code review is the systematic evaluation of source code changes before they are accepted into a shared codebase. The reviewer examines whether proposed changes satisfy intended requirements while maintaining software quality.

Code review is not limited to syntax errors. Compilers, linters, and testing tools detect many mechanical problems. Reviewers focus on broader questions:

- Does the implementation solve the correct problem?
- Does it follow the architecture?
- Does it introduce security risks?
- Are business rules preserved?
- Are tests meaningful?
- Is the change maintainable?
- Is anything unnecessary?

## 4. Why Code Review Matters in Agentic Development

Codex can modify multiple files and generate substantial implementations. AI-generated changes may contain incorrect assumptions, missing edge cases, unnecessary abstractions, duplicated business logic, incomplete authorization checks, misleading tests, unrelated modifications, or unexpected dependency changes. A successful build cannot detect every problem.

```text
Approved Requirement -> Codex Implementation -> Automated Verification -> Code Review
                                                               |                 |
                                                               +---- Approved / Revision
```

**Automated verification and code review complement each other. Neither completely replaces the other.**

## 5. Code Review Objectives

| Dimension | Review Question |
|---|---|
| Correctness | Does the code implement the required behavior? |
| Security | Does the change preserve access control and data protection? |
| Architecture | Does it follow established responsibility boundaries? |
| Maintainability | Can future developers understand and modify it? |
| Scope | Does every change support the approved task? |

Additional dimensions include performance, test quality, accessibility, and compatibility.

---

# Part III — The Review Mindset

## 6. Reviewing Code Is Not Just Reading Code

Understand intended behavior before evaluating implementation details.

1. Read the requirement.
2. Review acceptance criteria.
3. Understand the feature's purpose.
4. Inspect the list of changed files.
5. Read the diff.
6. Trace relevant behavior.
7. Inspect tests.
8. Evaluate risks.
9. Decide whether changes are acceptable.

**Review the implementation against the requirement, not merely against whether the code looks reasonable.**

## 7. Review Findings vs Preferences

| Finding | Classification |
|---|---|
| User can edit another user's restricted record | Security defect |
| Valid input is rejected | Correctness defect |
| Repeated business logic appears in several components | Maintainability concern |
| Reviewer prefers another variable name | Potential style preference |

**Prioritize consequential findings over subjective preferences.**

---

# Part IV — Understanding Git

## 8. What Is Git?

Git is a distributed version control system used to track changes in source code and related project files. It supports version history, branching, merging, collaboration, change inspection, recovery, and experimentation. Git records project snapshots through commits.

## 9. Git's Three Main Areas

```text
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Local Repository
       |
       | git push
       v
Remote Repository
```

**Working Directory** contains files currently being edited. **Staging Area**, also called the index, contains the proposed snapshot for the next commit. **Local Repository** stores commits and history. **Remote Repository** is a separate repository for collaboration and synchronization, commonly hosted on GitHub.

## 10. Why the Staging Area Matters

The staging area lets developers choose which changes belong in a commit. Codex may modify a Java class, XML layout, test, and unrelated documentation; do not automatically include all modifications. Selective staging keeps commits focused.

---

# Part V — Inspecting Repository Status

## 11. Essential Git Commands

```powershell
git status
git status --short
git branch --show-current
git rev-parse --show-toplevel
git log --oneline -10
```

These inspect repository status, compact status, current branch, root, and recent commits.

**Inspect repository status before and after Codex performs implementation work.**

## 12. Understanding Git Status

Example:

```text
 M app/src/main/java/.../SignInActivity.java
 M app/src/main/res/layout/activity_sign_in.xml
?? app/src/main/res/drawable/sign_in_illustration.png
```

` M` indicates a tracked file with unstaged modifications. `??` indicates an untracked file. Git short status uses two columns for staged and working-tree states. Untracked files are not displayed by ordinary `git diff`; inspect them separately.

---

# Part VI — Understanding Git Diffs

## 13. What Is a Git Diff?

A Git diff shows differences between file versions or repository states.

```powershell
git diff
git diff --cached
git diff --stat
git diff --check
```

`git diff` shows unstaged tracked changes; `--cached` shows staged changes; `--stat` summarizes unstaged changes; `--check` checks for certain whitespace errors.

## 14. Reading Diff Symbols

```diff
- String message = "Welcome";
+ String message = "Welcome to GroupTrack";
```

Lines beginning with `-` were removed; lines beginning with `+` were added. Unprefixed context helps show surrounding code. A diff is not necessarily the full behavior of a program; inspect surrounding functions, dependencies, and callers when needed.

## 15. File-Level Review

```powershell
git diff --stat
git diff -- app/src/main/res/layout/activity_sign_in.xml
git diff --cached -- app/src/main/res/layout/activity_sign_in.xml
```

Start with the overall change footprint, then investigate individual files and behavior.

## 16. Change Footprint Analysis

A change footprint describes the number and kinds of files affected.

**Approved task:** Update one sign-in layout.

| File | Change |
|---|---|
| Sign-in XML | Layout improvement |
| Sign-in Java class | UI event adjustment |
| Strings XML | New text resources |
| Authentication service | Entire rewrite |
| Gradle configuration | New dependencies |

The first three may be justified. The authentication rewrite and dependency changes need investigation. Unexpected change size is a review signal, not automatic proof of an error.

---

# Part VII — Reviewing Codex-Generated Code

## 17. Codex Review Workflow

```text
Codex Completes Task -> Inspect Git Status -> Review Changed Files -> Compare Requirements
             -> Inspect Business Logic -> Inspect Tests -> Review Security and Architecture
             -> Run Relevant Verification -> Accept / Revise / Reject
```

## 18. Reviewing Correctness

Ask whether the implementation satisfies acceptance criteria; handles invalid inputs and failures; enforces required permissions; preserves existing behavior; calculates correctly; and uses valid state transitions. A scheduling feature must respect approved rules, not merely save records successfully.

## 19. Reviewing Architecture

Look for business logic in the wrong layer, duplicated validation, unnecessary coupling, direct database access from presentation code, unapproved module creation, and inconsistent patterns.

**The simplest implementation is not always the shortest code, but it should avoid unnecessary complexity.**

## 20. Reviewing Security

Review authentication, authorization, input validation, sensitive data handling, secret management, API restrictions, and database access controls. Laravel endpoints must enforce permissions at the appropriate trusted boundary; frontend controls alone are insufficient.

## 21. Reviewing Tests

Ask whether tests exercise production behavior, use correct expectations, would detect regressions, cover important failure cases, use realistic fixtures, and control external dependencies. A test that duplicates a production calculation instead of calling production code may pass when the application method is broken.

---

# Part VIII — Using Codex as a Reviewer

## 22. Codex-Assisted Review

Codex can analyze a diff and identify possible defects, but findings require human evaluation. OpenAI documents Codex review workflows and repository-specific guidance through `AGENTS.md`.

```text
Task:
Review the current implementation changes.

Context:
Read applicable AGENTS.md instructions
and approved feature requirements.

Review Focus:
1. Correctness defects.
2. Authorization and security risks.
3. Architectural violations.
4. Regression risks.
5. Missing edge cases.
6. Inadequate tests.
7. Unnecessary dependencies.
8. Scope expansion.

Instructions:
- Do not modify files.
- Inspect the relevant diff.
- Read surrounding code where necessary.
- Prioritize actionable findings.
- Avoid subjective style comments.
- Distinguish confirmed defects from potential risks.

Output each finding with severity, file/location,
evidence, impact, and recommended correction.
If none are significant, say so without claiming
the code is guaranteed correct.
```

## 23. Review Severity Levels

| Severity | Meaning |
|---|---|
| Critical | Severe security or data-integrity risk |
| High | Major functional or security problem |
| Medium | Meaningful defect with limited impact |
| Low | Minor correctness or maintainability issue |
| Informational | Useful observation, not necessarily a defect |

Severity should consider likelihood, impact, and application context.

## 24. Evaluating Codex Findings

For each finding: inspect referenced code, confirm the described behavior, check requirements, determine reproducibility, evaluate severity, accept or reject the finding, and implement an approved correction if needed.

**An AI review finding is a hypothesis supported by evidence, not an automatic verdict.**

---

# Part IX — Selective Staging

## 25. What Is Selective Staging?

Selective staging allows developers to include only intended changes in a commit.

```powershell
git add app/src/main/res/layout/activity_sign_in.xml
git add app/src/main/res/layout/activity_sign_in.xml app/src/main/res/values/strings.xml
git add -p
```

Patch staging allows selection of individual hunks.

## 26. Why Avoid Blind Staging?

`git add .` stages eligible changes below the current directory. It is not inherently wrong, but can include unrelated modifications or unintended files.

```powershell
git status
git diff
git add -p
git diff --cached
```

**Review the exact staged snapshot before creating a commit.**

---

# Part X — Meaningful Commits

## 27. What Is a Git Commit?

A commit records a snapshot of staged changes in repository history. Each has a unique identifier, parent relationship, author and committer information, message, and tracked-content snapshot.

## 28. Characteristics of a Good Commit

A good commit is focused, understandable, relevant to one logical change, reviewable, accompanied by an informative message, and consistent with project conventions.

Weak: `fix`  
Better: `fix: prevent duplicate driver assignments`  
Another example: `feat: add sign-in screen layout`

## 29. Conventional Commits

Conventional Commits is an optional naming convention.

| Prefix | Purpose |
|---|---|
| `feat` | New feature |
| `fix` | Bug correction |
| `docs` | Documentation |
| `test` | Test changes |
| `refactor` | Internal restructuring |
| `chore` | Maintenance |
| `build` | Build-system changes |
| `ci` | CI configuration |
| `perf` | Performance improvement |

```powershell
git commit -m "feat: implement product listing UI"
```

Follow the repository's established conventions; Conventional Commits is not a Git requirement.

## 30. Atomic Commits

An atomic commit represents one coherent change. Separate unrelated login UI, inventory, scheduling, and dependency-upgrade work into their own changes. Focused commits improve review, history navigation, targeted reverts, and traceability.

**Commit boundaries should reflect logical changes, not arbitrary file counts.**

---

# Part XI — Branching Strategies

## 31. What Is a Git Branch?

A Git branch is a movable reference to a commit. Branches allow developers to work on changes without immediately modifying the main integration branch.

## 32. Feature Branch Workflow

```text
main
 |
 +--------------------------+
 |                          |
 v                          v
Stable Code          feature/sign-in-ui
                            |
                            v
                       Implementation
                            |
                            v
                          Tests
                            |
                            v
                       Pull Request
                            |
                            v
                          Review
                            |
                            v
                          Merge
```

## 33. Creating a Feature Branch

```powershell
git switch -c feature/sign-in-ui
git branch --show-current
```

Examples: `feature/sign-in-ui`, `feature/product-listing`, `fix/schedule-validation`, `docs/update-architecture`. Keep naming consistent without unnecessary complexity.

## 34. Common Branching Approaches

- **Trunk-based development:** frequent integration into the primary branch, usually with short-lived branches or other controls; requires strong discipline and verification.
- **Feature branch workflow:** isolated feature or fix branches before integration; long-lived branches may diverge.
- **Git Flow:** explicit `develop`, feature, release, and hotfix branches; offers release structure but adds process complexity.

| Approach | Strength | Tradeoff |
|---|---|---|
| Trunk-based | Frequent integration | Requires strong discipline and verification |
| Feature branches | Isolated development and review | Long-lived branches can diverge |
| Git Flow | Explicit release structure | Additional process complexity |

For small projects, a primary branch plus short-lived feature branches is often sufficient. Avoid complicated branching without a coordination need.

---

# Part XII — Pull Requests

## 35. What Is a Pull Request?

A pull request (PR) proposes merging changes from one branch into another. It provides a place for change descriptions, diff review, discussion, automated checks, reviewer feedback, approval, and merge decisions.

## 36. Pull Request Lifecycle

```text
Feature Branch -> Push Changes -> Open Pull Request -> Automated Checks -> Code Review
                                                              |                |
                                                              +-- Revise / Approve -> Merge
```

## 37. Good Pull Request Description

A PR should explain what changed and why, which requirements it addresses, how it was tested, what risks remain, whether screenshots are relevant, and whether database or configuration changes are included.

## 38. Pull Request Template

```markdown
# Summary
Describe the change.

## Related Requirement
Feature ID:
Requirement ID:

## Changes
- Change 1
- Change 2

## Scope
Describe included functionality.

## Non-Goals
Describe excluded functionality.

## Verification
| Check | Result |
|---|---|
| Build | |
| Unit tests | |
| Integration tests | |
| Manual verification | |

## Risks
List relevant risks.

## Screenshots
Include when useful for UI changes.

## Review Checklist
- [ ] Scope verified
- [ ] Tests reviewed
- [ ] Security considered
- [ ] Architecture preserved
- [ ] No unintended files
```

---

# Part XIII — Branch Protection and CI

## 39. What Is Branch Protection?

Branch protection restricts operations on important branches. Settings may require PR approval, passing checks, resolved review conversations, signed commits, restricted direct pushes, or other rules.

## 40. Why Branch Protection Matters

Without controls, an unreviewed change may reach an important branch.

```text
Feature Branch -> Pull Request -> Required Checks -> Required Review -> Merge Allowed
```

**Automated checks and human review should enforce the project's acceptance standards.**

## 41. What Is Continuous Integration?

Continuous Integration (CI) is a practice of frequently integrating changes and automatically verifying them. A pipeline may run builds, unit and integration tests, linting, type checks, and security scans.

```text
Pull Request -> CI Pipeline -> Build / Unit Tests / Lint / Type Check -> Results
```

CI only checks what is configured. A green pipeline does not guarantee the absence of defects.

---

# Part XIV — Merge Conflicts

## 42. What Is a Merge Conflict?

A merge conflict occurs when Git cannot automatically reconcile competing changes. For example, two branches may change button text to “Sign In” and “Log In”.

## 43. Conflict Markers

```text
<<<<<<< HEAD
Sign In
=======
Log In
>>>>>>> feature/login-label
```

The markers identify competing versions; a developer must determine the correct final content.

**Conflict resolution is a requirements and correctness decision, not merely a text-editing task.**

## 44. Safe Conflict Resolution

1. Identify conflicting files.
2. Understand both changes.
3. Review relevant requirements.
4. Decide intended behavior.
5. Edit the conflict carefully.
6. Remove conflict markers.
7. Run relevant tests.
8. Review the resulting diff.
9. Complete the merge or rebase.

```powershell
git status
git diff --check
git add path/to/resolved-file
git merge --continue
```

Commands vary depending on whether the operation is a merge, rebase, or cherry-pick.

---

# Part XV — Git Worktrees

## 45. What Is a Git Worktree?

A Git worktree lets one repository have multiple working directories. This helps work on separate branches without repeatedly switching a single directory.

```text
Repository
   +-- Main Worktree (main)
   +-- Worktree A (feature/sign-in-ui)
   +-- Worktree B (fix/navigation)
```

Each worktree has its own working directory and index, while sharing repository history and most Git metadata.

## 46. Creating a Worktree

```powershell
git worktree add -b feature/sign-in-ui ../grouptrack-signin
git worktree list
git worktree remove ../grouptrack-signin
```

Git normally refuses to remove a worktree with uncommitted changes unless forced. Review and preserve work before removal.

## 47. When Worktrees Are Useful

- Parallel feature development.
- Reviewing another branch.
- Testing a hotfix.
- Separating agent tasks.
- Avoiding constant branch switching.

Worktrees do not eliminate merge conflicts. Changes still need coordination during integration.

---

# Part XVI — Multi-Agent Git Coordination

## 48. Multi-Agent Development

```text
Human Developer
       +-------------------+
       |                   |
       v                   v
Codex Agent A         Codex Agent B
       |                   |
       v                   v
Android UI             Unit Tests
       |                   |
       v                   v
Branch A                Branch B
       +---------+---------+
                 |
                 v
          Human Integration
                 |
                 v
             Verification
```

## 49. Multi-Agent Risks

- Overlapping file edits.
- Conflicting assumptions.
- Duplicate implementation.
- Incompatible interfaces.
- Shared environment changes.
- Uncoordinated migrations.
- Conflicting dependency updates.

## 50. Coordination Rules

1. Assign clear task ownership.
2. Define file and module boundaries.
3. Establish shared contracts.
4. Use separate branches or worktrees when appropriate.
5. Avoid concurrent edits to the same files.
6. Review each agent's output independently.
7. Integrate changes deliberately.
8. Run combined verification.

**Parallel agents can increase throughput, but coordination overhead and integration risk also increase.**

---

# Part XVII — Git Recovery and Rollback

## 51. Why Recovery Matters

Even reviewed changes can introduce defects. Git offers recovery mechanisms, but commands have different effects.

## 52. Git Revert

`git revert` creates a new commit reversing an earlier commit.

```powershell
git revert <commit-hash>
```

This is suitable when a faulty change has already been committed and shared. It preserves the original and its reversal in visible history.

## 53. Git Restore

`git restore` restores file content from a selected source.

```powershell
git restore -- path/to/file
```

This discards unstaged changes to the specified tracked file. Use only after confirming the changes are not needed.

## 54. Git Reset

`git reset` can move the current branch reference and optionally modify the index and working tree, depending on mode. `git reset --hard` can permanently discard uncommitted tracked changes; do not use it casually.

## 55. Recovery Comparison

| Command | Typical Purpose | Main Risk |
|---|---|---|
| `git revert` | Reverse a commit with a new commit | Conflicts may require resolution |
| `git restore` | Restore file content | Can discard local changes |
| `git reset` | Move branch or adjust staging state | Some modes discard work |
| `git stash` | Temporarily save selected local changes | Forgotten or misapplied stashes |
| `git worktree` | Maintain separate working directories | Uncoordinated branch integration |

**Choose recovery operations based on repository state and whether changes have been shared.**

---

# Part XVIII — Security Review

## 56. Security Review Objectives

Security review evaluates whether a change introduces unacceptable risks. Common areas include authorization bypass, sensitive data exposure, unsafe input handling, secret leakage, insecure configuration, excessive permissions, and dependency vulnerabilities.

## 57. Example — Laravel Authorization

When a controller retrieves a record by ID, ask whether the user is authenticated and authorized for that record, whether ownership restrictions apply, and whether another user's record can be modified.

**Frontend restrictions are not a substitute for backend authorization.**

## 58. Example — Next.js Secrets

Verify server-side credentials are not exposed through client-delivered code. In Next.js, variables intended for browser access are typically prefixed with `NEXT_PUBLIC_`; that prefix does not make values secret.

**Never place private credentials in browser-exposed environment variables.**

## 59. Example — Android Configuration

Check that sensitive values are not hardcoded unnecessarily, debug logs do not expose user information, Firebase access rules are appropriate, permissions match actual needs, and release configuration is reviewed separately.

Client applications cannot reliably keep embedded secrets confidential. Sensitive operations should use appropriate trusted services and access controls.

---

# Part XIX — Android Example: GroupTrack

## 60. Project Context

GroupTrack uses Java, XML layouts, Android Studio, and Firebase, and follows a defined MVP. Assume Codex implemented the approved sign-in UI.

## 61. Review Checklist

**UI**

- [ ] Layout matches approved design.
- [ ] Required resources exist.
- [ ] Text is readable.
- [ ] Keyboard behavior is usable.
- [ ] Navigation is correct.

**Architecture**

- [ ] UI logic follows existing patterns.
- [ ] Authentication architecture is unchanged unless approved.
- [ ] No unnecessary dependencies.
- [ ] No unrelated screens modified.

**Verification**

- [ ] Build completed.
- [ ] Relevant UI checks completed.
- [ ] Runtime limitations documented.
- [ ] Diff inspected.

## 62. Example Git Workflow

```powershell
git status
git switch -c feature/sign-in-ui

# Codex performs the approved implementation.

git diff --stat
git diff
.\gradlew.bat assembleDebug
git add app/src/main/res/layout/activity_sign_in.xml
git diff --cached
git commit -m "feat: add sign-in screen layout"
```

This is illustrative, not an instruction to commit before all required files and verification are reviewed.

---

# Part XX — Laravel Example: GoPasig

## 63. Project Context

GoPasig contains scheduling, route management, GPS tracking, and administrative operations. A small change can affect multiple business workflows.

## 64. Review Scenario — Schedule Validation

**Approved Change:** Correct a scheduling validation defect.

Review whether the implementation follows approved business rules, preserves existing checks, enforces authorization, uses appropriate transactions, handles concurrent requests, includes meaningful regression tests, and avoids unrelated scheduling-rule changes.

## 65. Laravel Review Workflow

```text
Review Requirement -> Inspect Schedule Diff -> Review Business Rules -> Review Database Impact
          -> Review Tests -> Run Focused Verification -> Approve or Request Changes
```

```powershell
git diff --stat
git diff
php artisan test --filter=ScheduleValidationTest
```

**Business-rule changes require domain-level review, not only syntax and framework-level checks.**

---

# Part XXI — Next.js and Laravel Example

## 66. Project Context

```text
Next.js Frontend
       |
       v
Laravel API
       |
       v
PostgreSQL
```

Frontend and backend projects may have separate Git histories and review workflows.

## 67. Review Scenario — Product Listing

**Approved Change:** Display products using an existing Laravel API.

Frontend review: correct endpoint, types match actual response, loading and error states work, missing or empty data is handled, and no secrets are exposed.

Backend review, only if changed: API contract remains correct, authorization is enforced, response fields are appropriate, and queries are sufficiently efficient for requirements.

## 68. Cross-Repository Review

If both repositories changed: review frontend changes, review backend changes, verify the shared API contract, run each repository's checks, test integration, and coordinate merge or release order.

**Passing checks in two repositories independently does not prove their combined integration works.**

---

# Part XXII — Documentation Synchronization

## 69. Why Documentation Matters During Review

Implementation changes may affect architecture diagrams, API contracts, database schemas, feature documentation, setup instructions, testing procedures, and `AGENTS.md`. Outdated documentation may mislead future developers and agents.

## 70. Documentation Review Questions

1. Did the architecture change?
2. Did an API contract change?
3. Were new environment variables introduced?
4. Did the database schema change?
5. Did commands or setup steps change?
6. Were new business rules approved?
7. Do agent instructions remain accurate?

**Update documentation when implementation changes documented facts—not merely to create more documentation.**

---

# Part XXIII — Review Handoffs

## 71. What Is a Review Handoff?

A review handoff summarizes completed implementation so another developer or agent can evaluate it. Include feature objective, approved scope, changed files, verification results, known limitations, risks, branch or commit details, and review requests.

## 72. Review Handoff Template

```markdown
# Code Review Handoff

## Feature
[Feature name]

## Requirement
[Requirement ID]

## Summary
Describe the implementation.

## Changed Files
List relevant files.

## Scope Compliance
Explain whether the changes remain within approved boundaries.

## Verification
| Check | Result |
|---|---|

## Known Limitations
List incomplete or unverified areas.

## Review Focus
Identify areas requiring attention.

## Git Information
Branch:
Commits:

## Status
Ready for Review / Needs Fixes / Blocked
```

---

# Part XXIV — Common Mistakes

## 73. Mistake: Trusting Codex Without Reviewing the Diff

**Problem:** Unnecessary or incorrect changes may be accepted.  
**Improvement:** Inspect all relevant modifications.

## 74. Mistake: Reviewing Only Formatting

**Problem:** Serious logic defects may be overlooked.  
**Improvement:** Prioritize behavior, security, and architecture.

## 75. Mistake: Blindly Running `git add .`

**Problem:** Unrelated files may be staged.  
**Improvement:** Review and selectively stage changes.

## 76. Mistake: Creating Vague Commits

**Problem:** History becomes difficult to understand.  
**Improvement:** Use meaningful commit messages.

## 77. Mistake: Keeping Feature Branches Open Too Long

**Problem:** Integration conflicts become more likely.  
**Improvement:** Prefer manageable, frequently integrated changes.

## 78. Mistake: Accepting Every AI Review Comment

**Problem:** False positives and subjective preferences may create unnecessary work.  
**Improvement:** Validate findings against evidence.

## 79. Mistake: Resolving Merge Conflicts Mechanically

**Problem:** Correct behavior may be lost.  
**Improvement:** Understand both changes before resolution.

## 80. Mistake: Using Destructive Recovery Commands

**Problem:** Uncommitted work may be lost.  
**Improvement:** Choose targeted recovery methods.

## 81. Mistake: Overcomplicating Branching

**Problem:** Workflow overhead exceeds project needs.  
**Improvement:** Use the simplest effective branching strategy.

## 82. Mistake: Treating CI Success as Final Approval

**Problem:** Untested requirements and architecture issues may remain.  
**Improvement:** Combine automated checks with appropriate review.

---

# Part XXV — Practical Exercises

> These exercises are optional. The current learning objective remains theoretical understanding.

## 83. Exercise A — Code Review Checklist

Create a checklist covering correctness, security, architecture, maintainability, tests, and scope.

**Deliverable:** Review checklist.

## 84. Exercise B — Git State Analysis

Explain the difference between working directory, staging area, local repository, and remote repository.

**Deliverable:** Git state diagram.

## 85. Exercise C — Diff Interpretation

Analyze a hypothetical diff. Identify added lines, removed lines, modified behavior, and potential regressions.

**Deliverable:** Diff review report.

## 86. Exercise D — Selective Staging

Design a staging plan for a repository containing feature code, tests, documentation, and unrelated local changes.

**Deliverable:** Staging strategy.

## 87. Exercise E — Commit Organization

Divide a hypothetical feature into logical commits and write meaningful commit messages.

**Deliverable:** Commit plan.

## 88. Exercise F — Branching Strategy

Compare trunk-based development, feature branches, and Git Flow. Choose a suitable strategy for a small student project and explain why.

**Deliverable:** Branching decision.

## 89. Exercise G — Pull Request Description

Prepare a PR description for an Android UI feature, including summary, requirements, changes, tests, screenshots, and risks.

**Deliverable:** PR draft.

## 90. Exercise H — Merge Conflict Analysis

Explain what changed in each branch, which requirement controls resolution, and how final behavior should be verified.

**Deliverable:** Conflict resolution plan.

## 91. Exercise I — Git Worktree Planning

Design a worktree setup for two independent tasks, including branch names, directories, task ownership, and integration plan.

**Deliverable:** Worktree plan.

## 92. Exercise J — Multi-Agent Coordination

Design a workflow where two Codex agents work on different features. Identify task boundaries, shared contracts, potential conflicts, verification, and human approval.

**Deliverable:** Coordination plan.

## 93. Exercise K — Recovery Strategy

Compare recovery options for an unstaged local modification, staged but uncommitted change, shared faulty commit, and conflicted merge.

**Deliverable:** Recovery decision table.

## 94. Exercise L — Android Code Review

Prepare a review checklist for a GroupTrack sign-in UI change.

**Deliverable:** Android review report.

## 95. Exercise M — Laravel Code Review

Prepare a review checklist for GoPasig scheduling validation, focusing on business rules, authorization, data integrity, concurrency, and tests.

**Deliverable:** Laravel review report.

## 96. Exercise N — Next.js/Laravel Review

Prepare a review plan for a product listing feature, including API contract, type safety, UI states, backend authorization, and integration verification.

**Deliverable:** Full-stack review plan.

## 97. Exercise O — Codex Review Prompt

Write a review-only Codex prompt that inspects the diff, follows `AGENTS.md`, prioritizes real defects, identifies evidence, does not modify files, and reports uncertainty.

**Deliverable:** Reusable review prompt.

---

# Part XXVI — Knowledge Review

## 98. Knowledge Check

1. What is code review?
2. Why is code review important for AI-generated code?
3. What dimensions should a review evaluate?
4. How does code review differ from testing?
5. What is the difference between a defect and a preference?
6. What is Git?
7. What is the working directory?
8. What is the staging area?
9. What is a commit?
10. What is a remote repository?
11. What does `git status` show?
12. What does `git diff` show?
13. What does `git diff --cached` show?
14. Why are untracked files important?
15. What is change footprint analysis?
16. How should Codex-generated changes be reviewed?
17. Why should AI review findings be verified?
18. What is selective staging?
19. What does `git add -p` do?
20. What makes a commit meaningful?
21. What is an atomic commit?
22. What are Conventional Commits?
23. What is a Git branch?
24. Why use feature branches?
25. What is trunk-based development?
26. What is Git Flow?
27. What is a pull request?
28. What belongs in a PR description?
29. What is branch protection?
30. What is Continuous Integration?
31. Why does a green CI pipeline not guarantee correctness?
32. What is a merge conflict?
33. How should conflicts be resolved?
34. What is a Git worktree?
35. Why are worktrees useful for parallel agents?
36. What risks exist in multi-agent Git workflows?
37. What does `git revert` do?
38. What does `git restore` do?
39. Why can `git reset --hard` be dangerous?
40. What should a security review examine?
41. Why must Laravel authorization be reviewed?
42. Why must Next.js secrets remain server-side?
43. What should Android reviewers inspect?
44. Why must documentation remain synchronized?
45. What belongs in a review handoff?

---

## 99. Key Terminologies

| Term | Definition |
|---|---|
| Code Review | Evaluation of proposed code changes |
| Reviewer | Person or agent examining changes |
| Diff | Representation of differences between code states |
| Working Directory | Files currently checked out |
| Staging Area | Proposed snapshot for the next commit |
| Index | Git's staging data structure |
| Commit | Recorded repository snapshot |
| Branch | Movable reference to a commit |
| Remote | Repository used for synchronization |
| Pull Request | Proposal to merge branch changes |
| CI | Continuous Integration |
| Branch Protection | Rules restricting branch modifications |
| Atomic Commit | Commit representing one coherent change |
| Conventional Commits | Optional commit-message convention |
| Selective Staging | Choosing specific changes for a commit |
| Merge Conflict | Changes Git cannot automatically reconcile |
| Worktree | Additional working directory linked to a repository |
| Revert | New commit reversing earlier changes |
| Restore | Restoring file content from a selected source |
| Reset | Adjusting branch, index, or working-tree state |
| Scope Review | Checking changes against approved boundaries |
| Security Review | Evaluating security-related risks |
| Architecture Review | Checking architectural consistency |
| Review Handoff | Summary prepared for reviewers |
| Regression Risk | Possibility of breaking existing behavior |

---

## 100. Key Takeaways

1. AI-generated code must be reviewed before acceptance.
2. Code review evaluates correctness, security, architecture, and scope.
3. Automated tests do not replace code review.
4. Git tracks changes through commits and branches.
5. The staging area controls the next commit's content.
6. Repository status should be inspected regularly.
7. Diff review reveals unexpected modifications.
8. Untracked files require separate inspection.
9. Codex review findings must be validated.
10. Selective staging improves commit quality.
11. Commits should represent coherent changes.
12. Meaningful messages improve repository history.
13. Feature branches isolate development work.
14. Branching complexity should match project needs.
15. Pull requests provide structured review and integration.
16. Branch protection can enforce required checks.
17. Merge conflicts require understanding both changes.
18. Git worktrees support parallel development.
19. Multi-agent workflows require coordination.
20. Recovery commands have different safety implications.
21. Security review should include authorization and secrets.
22. Android reviews should evaluate UI, resources, and runtime behavior.
23. Laravel reviews should preserve business rules and data integrity.
24. Next.js/Laravel reviews should verify API contracts.
25. Documentation and review handoffs preserve reliable development context.

---

# 101. Official References

## OpenAI Codex

1. [OpenAI Codex Documentation](https://developers.openai.com/codex/)
2. [OpenAI Codex CLI](https://developers.openai.com/codex/cli/)
3. [Codex Code Review](https://developers.openai.com/codex/code-review/)
4. [Codex AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
5. [Custom Code Review Rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)

## Git

6. [Git Documentation](https://git-scm.com/docs)
7. [Git Status](https://git-scm.com/docs/git-status)
8. [Git Diff](https://git-scm.com/docs/git-diff)
9. [Git Add](https://git-scm.com/docs/git-add)
10. [Git Commit](https://git-scm.com/docs/git-commit)
11. [Git Branch](https://git-scm.com/docs/git-branch)
12. [Git Worktree](https://git-scm.com/docs/git-worktree)
13. [Git Revert](https://git-scm.com/docs/git-revert)
14. [Git Restore](https://git-scm.com/docs/git-restore)
15. [Git Reset](https://git-scm.com/docs/git-reset)

## GitHub

16. [GitHub Pull Requests](https://docs.github.com/en/pull-requests)
17. [GitHub Pull Request Reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests)
18. [GitHub Branch Protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
19. [GitHub Actions](https://docs.github.com/en/actions)

## Application Development

20. [Android Developers](https://developer.android.com/)
21. [Laravel 12 Documentation](https://laravel.com/docs/12.x)
22. [Next.js Documentation](https://nextjs.org/docs)
23. [OWASP Code Review Guide](https://owasp.org/www-project-code-review-guide/)
24. [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

> **Documentation Note:** Codex review capabilities, GitHub integrations, and repository protection features may depend on configuration and access. Verify current product documentation before enabling or relying on a particular workflow.

---

# 102. Lesson Completion Checklist

- [ ] Understand code review fundamentals and why AI-generated code requires review.
- [ ] Distinguish defects from preferences.
- [ ] Understand Git's working directory, staging area, and commits.
- [ ] Inspect Git status, diffs, staged changes, and untracked files.
- [ ] Evaluate change footprints and Codex-generated changes.
- [ ] Understand selective staging and atomic commits.
- [ ] Write meaningful commit messages.
- [ ] Understand feature branches and compare branching strategies.
- [ ] Understand pull requests, branch protection, and CI.
- [ ] Understand merge conflicts and resolution.
- [ ] Understand Git worktrees and multi-agent coordination.
- [ ] Distinguish revert, restore, and reset; understand recovery risks.
- [ ] Perform security and architecture review.
- [ ] Understand Android, Laravel, and Next.js/Laravel code review.
- [ ] Synchronize documentation and prepare review handoffs.
- [ ] Complete optional exercises and answer knowledge-check questions.

---

# 103. Next Lesson

## [[07 - Final Project Evaluation]]

The final lesson in Phase 6 focuses on evaluating the completed project and determining whether it satisfies approved objectives.

### Topics

- Final project assessment.
- MVP scope verification.
- Functional requirements traceability.
- Non-functional requirements evaluation.
- Acceptance criteria audit.
- Code quality assessment.
- Architecture consistency.
- Testing evidence.
- Security and performance review.
- Documentation completeness.
- Technical debt assessment.
- Risk and limitations reporting.
- Final project presentation.
- Lessons learned and retrospective.
- Agentic development maturity.
- Android, Laravel, and Next.js examples.

### Learning Progression

```text
Lesson 6.1 — Project Selection
                |
                v
Lesson 6.2 — Project Setup
                |
                v
Lesson 6.3 — Feature Planning
                |
                v
Lesson 6.4 — Guided Implementation
                |
                v
Lesson 6.5 — Debugging and QA
                |
                v
Lesson 6.6 — Code Review and Git
                |
                v
Lesson 6.7 — Final Evaluation
```

---

*Phase 6 — Real-World Application | Lesson 6.6*  
*Learning Principle: Treat AI-generated code as a proposed change, use Git to preserve traceability, and require appropriate review and verification before integration.*
