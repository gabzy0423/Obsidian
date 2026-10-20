# 07 — Git Integration and Code Review

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 3 — Agentic Coding Workflow  
> **Lesson:** 3.7 — Git Integration and Code Review  
> **Difficulty:** Intermediate  
> **Environment:** Windows, PowerShell, Git, GitHub, Codex CLI  
> **Prerequisite:** [[06 - Testing and Validation]]  
> **Status:** In Progress

---

## 1. Introduction

In traditional software development, developers write code, test it, review the changes, and commit approved work to a repository.

Agentic development follows the same principles, but an AI coding agent can perform many of these operations.

Codex can inspect repository history, identify modified files, analyze diffs, help create focused changes, run authorized verification, assist with commit messages, review for defects, and prepare pull request summaries.

However, an AI agent can misunderstand requirements, modify unrelated files, introduce unnecessary dependencies, or produce changes that appear correct but contain defects.

Git tracks changes and history. Code review evaluates whether proposed changes should be accepted.

**Core Principle: AI-generated changes should be inspected, verified, and approved before they become part of the project's trusted codebase.**

---

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Explain the purpose of Git in agentic development.
2. Understand repositories, commits, branches, and remotes.
3. Differentiate working directory, staging area, and commit history.
4. Inspect repository status before starting Codex.
5. Create feature and bug-fix branches.
6. Read and interpret Git diffs.
7. Stage specific files or changes.
8. Write meaningful commit messages.
9. Understand GitHub pull requests.
10. Review Codex-generated changes systematically.
11. Detect scope creep and unintended modifications.
12. Verify code changes before committing.
13. Understand safe rollback strategies.
14. Distinguish `git restore`, `git revert`, and `git reset`.
15. Handle merge conflicts carefully.
16. Apply Git workflows to Android and Laravel projects.
17. Establish approval checkpoints for AI-assisted development.

---

# Part I — Git Fundamentals

## 3. What Is Git?

**Git** is a distributed version control system that tracks changes to files over time. It allows developers to maintain project history and collaborate without relying on a single shared working directory.

Git helps answer: What changed? Who committed it? When and why? Which version introduced a defect? How can an earlier state be recovered?

### Example

```text
Commit A — Initial Android project
    |
    v
Commit B — Add login screen
    |
    v
Commit C — Add registration validation
```

Each commit represents a recorded project state. If registration validation introduces a problem, developers can inspect Commit C.

### Key Principle

Git provides a reviewable history, but does not automatically determine whether a change is correct.

---

## 4. Git vs GitHub

| Git | GitHub |
|---|---|
| Version control system | Repository hosting and collaboration platform |
| Works locally | Provides remote hosting |
| Tracks commits | Hosts Git repositories |
| Supports branches | Provides pull requests |
| Supports merging | Provides review and collaboration tools |
| Can work offline | Most hosted collaboration features require connectivity |

For example, Git manages a local project at `C:\Users\Acer\AndroidStudioProjects\GroupTrack`; GitHub can host a remote copy.

**Git manages version history. GitHub helps teams share, review, and collaborate on that history.**

---

## 5. Understanding a Git Repository

A **repository** is a project directory whose changes are managed by Git.

```text
GroupTrack/
├── .git/
├── .gitignore
├── AGENTS.md
├── docs/
├── app/
├── build.gradle
└── settings.gradle
```

The `.git` directory stores repository metadata and history. The `.gitignore` file specifies patterns Git should normally leave untracked.

Do not manually edit or delete `.git` during a normal development task; it can damage repository metadata or remove local history.

---

## 6. The Three Main Git Areas

Git workflows involve the working directory, staging area, and local repository history.

```text
Working Directory --git add--> Staging Area (Index)
        Modified files          Selected changes
                                     |
                                     | git commit
                                     v
                               Local Repository
                               Recorded history
                                     |
                                     | git push
                                     v
                               Remote Repository
```

### 6.1 Working Directory

The files currently present in the project folder. When Codex edits a file, the working directory changes.

### 6.2 Staging Area

A preparation area for the next commit. Only staged changes are included in that commit.

### 6.3 Local Repository

Contains recorded commits and Git history.

### 6.4 Remote Repository

A separately hosted repository, commonly on GitHub.

**Editing, staging, committing, and pushing are distinct operations.**

---

## 7. Essential Git Terminology

| Term | Meaning |
|---|---|
| Repository | Git-managed project |
| Working Tree | Files currently checked out |
| Index | Staging area |
| Commit | Recorded snapshot of changes |
| Branch | Movable reference to a development history |
| HEAD | Reference to the current checkout |
| Remote | Another repository location |
| Origin | Conventional default remote name |
| Main | Common primary branch name |
| Diff | Comparison of file content |
| Merge | Integration of development histories |
| Pull Request | Proposal to merge changes |
| Conflict | Changes requiring manual reconciliation |
| Revert | New commit reversing earlier changes |
| Stash | Temporary storage of working changes |

---

# Part II — Repository Inspection

## 8. Establishing a Git Baseline

Before allowing Codex to edit files, inspect the repository.

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
git --version
git status
git branch --show-current
git diff
git diff --cached
```

A repository may already contain developer changes. Codex must not assume every modified file belongs to the current task.

### Baseline Checklist

- [ ] Repository exists.
- [ ] Current branch identified.
- [ ] Existing modifications inspected.
- [ ] Staged changes identified.
- [ ] Untracked files identified.
- [ ] Project instructions located.
- [ ] Current task scope understood.

**Establish the initial repository state before evaluating Codex-generated changes.**

---

## 9. Understanding Git Status

Example:

```text
On branch feature/login-ui

Changes not staged for commit:
    modified: app/src/main/res/layout/activity_login.xml

Untracked files:
    app/src/main/res/drawable/login_illustration.png
```

**Modified:** An existing tracked file changed.  
**Untracked:** A file exists but is not currently tracked.  
**Staged:** A change was added to the index for a future commit.

```powershell
git status --short
```

| Symbol | Meaning |
|---|---|
| `??` | Untracked |
| ` M` | Modified in working tree |
| `M ` | Modified in index |
| `A ` | Added to index |
| ` D` | Deleted in working tree |

The two-character status representation distinguishes index state from working-tree state. Git status is one of the first and last commands developers should use during an agentic coding task.

---

## 10. Understanding Git History

```powershell
git log --oneline -10
git show a1b2c3d
git show --stat a1b2c3d
```

History helps understand previous decisions, identify recent changes, investigate regressions, and review a feature's origin.

**Repository history is useful context, but current requirements and code must still be verified.**

---

# Part III — Branching Strategies

## 11. What Is a Git Branch?

A **branch** allows developers to work on changes without immediately integrating them into the primary branch.

```text
main:    A --- B --- C
                \
feature:         D --- E
```

Branches help isolate features, bug fixes, refactoring, and experiments.

**Use branches to keep changes focused and reviewable.**

---

## 12. Creating a Feature Branch

```powershell
git branch --show-current
git switch -c feature/login-ui
git status
```

Common branch names:

```text
feature/login-ui
feature/task-creation
fix/registration-validation
fix/api-response
refactor/task-service
```

Convention: `type/short-description`.

Create a branch from the correct base and review existing changes before switching. Git may refuse a switch if local changes would be overwritten. Do not force the operation to bypass that protection.

---

## 13. Branching Workflow with Codex

```text
Identify Approved Task → Inspect Git Status → Create Task Branch
        → Run Codex → Review Changes → Run Tests
        → Stage Approved Changes → Commit → Push Branch → Pull Request
```

Example task: Improve spacing and alignment on the GroupTrack login screen. Branch: `feature/login-ui-alignment`.

Scope: existing login XML layout, necessary related resources, relevant UI verification.

Non-goals: Firebase authentication changes, new UI library, navigation redesign, unrelated screens.

**One focused task should normally produce one focused set of changes.**

---

# Part IV — Understanding Git Diffs

## 14. What Is a Git Diff?

A **diff** shows differences between file versions or repository states.

```diff
- android:text="Login"
+ android:text="Sign In"
```

Lines beginning with `-` were removed; lines beginning with `+` were added. Diffs can reveal unexpected deletion, incorrect logic, unrelated formatting, new dependencies, removed validation, hardcoded values, and accidental configuration changes.

**A change summary is not a substitute for inspecting the actual diff.**

---

## 15. Essential Git Diff Commands

```powershell
git diff
git diff --cached
git diff --name-only
git diff --stat
git diff -- app/src/main/res/layout/activity_login.xml
git diff --check
git diff main...feature/login-ui
```

The three-dot form compares the merge base with the feature branch tip. Ordinary `git diff` does not show untracked file contents; inspect `git status --short` and review new files separately.

---

## 16. Reading a Unified Diff

```diff
diff --git a/LoginActivity.java b/LoginActivity.java
--- a/LoginActivity.java
+++ b/LoginActivity.java
@@ -20,3 +20,4 @@
 setContentView(R.layout.activity_login);
+loginButton = findViewById(R.id.loginButton);
 loginButton.setOnClickListener(v -> submitLogin());
```

File headers identify the comparison. `---` and `+++` identify versions. `@@` marks a changed hunk. `+` means added, `-` removed, and unprefixed lines provide context.

Review whether the change solves the problem, is in the right location, follows conventions, affects unrelated behavior, and is tested.

---

## 17. File-Level Change Auditing

| File | Expected Change | Review |
|---|---|---|
| `activity_login.xml` | Adjust spacing | Expected |
| `colors.xml` | Reuse or adjust approved color resource | Review necessity |
| `LoginActivity.java` | No logic changes | Investigate if modified |
| `build.gradle` | No dependency changes | Investigate if modified |

A changed file is not automatically incorrect, but unexpected changes require explanation.

**Every modified file should have a justified relationship to the approved task.**

---

# Part V — Staging and Commits

## 18. What Is Git Staging?

Staging selects changes for the next commit.

```powershell
git add app/src/main/res/layout/activity_login.xml
git add -p
git diff --cached
```

`git add .` may stage more than intended. Prefer selecting approved files or hunks when practical.

**Stage what you have reviewed, not everything that happens to be modified.**

---

## 19. What Is a Commit?

A **commit** records a snapshot of staged changes in Git history.

```powershell
git commit -m "fix: initialize login button before use"
```

A good commit has a clear description, coherent purpose, reviewable scope, and meaningful history entry. It should represent a logical unit of work.

---

## 20. Writing Meaningful Commit Messages

Common convention: `type: short description`.

```text
feat: add task creation form
fix: reject empty task titles
refactor: simplify schedule validation
test: cover task creation errors
docs: update architecture notes
style: align login form elements
```

| Type | Purpose |
|---|---|
| `feat` | New functionality |
| `fix` | Bug correction |
| `refactor` | Structural improvement |
| `test` | Test changes |
| `docs` | Documentation |
| `style` | Formatting or presentation changes |
| `chore` | Maintenance work |

These are conventions, not built-in Git requirements. Weak: `update`. Better: `fix: prevent login crash from uninitialized button`.

**A commit message should explain the purpose of the change.**

---

## 21. Commit Review Workflow

```powershell
git status --short
git diff
git diff --cached
git diff --cached --check
git commit -m "fix: correct login button initialization"
git status
```

A clean working tree does not mean tests passed; it describes only the Git working-tree and index state.

---

# Part VI — GitHub Collaboration

## 22. What Is a Remote Repository?

A **remote repository** is another Git repository that exchanges commits with your local repository.

```powershell
git remote -v
git push -u origin feature/login-ui
```

Example remote URLs are illustrative. The `-u` option establishes upstream tracking.

**Pushing publishes commits to a remote; it does not automatically merge them into the primary branch.**

---

## 23. What Is a Pull Request?

A **Pull Request (PR)** proposes merging changes from one branch into another. It typically provides a description, commits, diffs, automated checks, review comments, and approval or requested changes.

```text
feature branch → Pull Request → Code Review → Verification → Merge → main
```

PRs create a review checkpoint before changes enter a shared branch.

**A pull request should communicate what changed, why it changed, and how it was verified.**

---

## 24. Writing a Good Pull Request

```markdown
# Pull Request

## Summary
Briefly describe the change.

## Related Task
Reference the approved feature or bug.

## Changes Made
- Change 1
- Change 2

## Out of Scope
List work intentionally excluded.

## Verification
- [ ] Relevant tests executed
- [ ] Build completed
- [ ] Manual verification completed
- [ ] Git diff reviewed

## Test Results
List actual commands and outcomes.

## Screenshots
Include relevant UI evidence if needed.

## Risks and Limitations
Document remaining concerns.

## Review Notes
Highlight areas needing attention.
```

Example title: `style: improve GroupTrack login screen alignment`.

Example summary: Improves spacing and alignment of existing login screen elements while preserving authentication behavior.

Verification may include Android resource compilation, manual layout inspection, and review of changed XML resources. Do not claim checks passed unless performed.

---

## 25. Code Review Fundamentals

**Code Review** is the systematic examination of proposed changes before accepting them.

Objectives include detecting defects, verifying requirements, preserving architecture, identifying security risks, checking maintainability, and preventing unrelated modifications.

- **Correctness:** Does the code implement approved behavior?
- **Scope:** Are all changes relevant?
- **Architecture:** Does it follow existing conventions?
- **Testing:** Are relevant behaviors verified?
- **Security:** Are sensitive operations and data handled appropriately?
- **Maintainability:** Is it understandable and appropriately simple?

**Code review evaluates quality and suitability, not merely whether code compiles.**

---

# Part VII — Codex-Generated Change Auditing

## 26. Why AI-Generated Code Needs Review

Codex can generate code quickly, but speed does not eliminate oversight. Possible problems include incorrect interpretation, out-of-scope changes, unapproved dependencies, unnecessary abstractions, modified public interfaces, missing validation, removed behavior, inadequate tests, and unexpected configuration consequences.

**Treat Codex output as a proposed implementation that must be evaluated against project requirements.**

---

## 27. Read-Only Codex Code Review

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
git status
codex --sandbox read-only
```

Review prompt:

```text
Perform a read-only code review.
Read applicable AGENTS.md instructions and project documentation.
Review working-tree changes, staged changes, and relevant untracked files.

Approved Task: Improve the existing login screen alignment.

Review correctness, scope compliance, architecture, unintended modifications,
dependencies, security, regression risks, missing tests, and documentation.

For each finding provide severity, file and location, evidence, impact,
and recommended correction. Do not modify, stage, commit, or push.
Distinguish confirmed findings from potential concerns.
Stop after the review report.
```

Expected output: a structured review containing evidence-based findings.

---

## 28. Codex Review Severity Levels

| Severity | Meaning | Example |
|---|---|---|
| Critical | Severe security or data-integrity risk | Unauthorized access to sensitive records |
| High | Major functional defect | Authentication bypass |
| Medium | Meaningful correctness or maintainability problem | Incorrect validation behavior |
| Low | Minor issue | Unclear naming |
| Informational | Suggestion without confirmed defect | Optional readability improvement |

Severity should reflect actual impact and evidence. **Do not treat every stylistic suggestion as a blocking defect.**

---

## 29. Scope Creep Detection

**Scope Creep** occurs when work expands beyond approved requirements.

Approved task: Improve login screen spacing. Unexpected changes might include rewriting authentication, adding a UI library, changing Firebase configuration, or redesigning registration.

```text
Compare current Git changes against the approved scope. Identify expected
and unexpected modifications, new dependencies or abstractions, removed
behavior, and changes requiring approval. Do not edit files. Recommend
the smallest change set that satisfies the original task.
```

**More code is not necessarily better code.**

---

## 30. Reviewing Dependencies

Dependency changes deserve special attention.

| Project | Files to Inspect |
|---|---|
| Android | `build.gradle`, `build.gradle.kts`, `libs.versions.toml`, `settings.gradle` |
| Laravel | `composer.json`, `composer.lock`, `package.json`, `package-lock.json` |

Review whether a dependency was necessary, already available, compatible with the stack, approved, and recorded in the correct lockfiles.

**Do not accept dependency changes simply because Codex generated them.**

---
# Part V — Staging and Commits

## 18. What Is Git Staging?

Staging selects changes for the next commit.

```powershell
git add app/src/main/res/layout/activity_login.xml
git add -p
git diff --cached
```

`git add .` may stage more than intended. Prefer selecting approved files or hunks when practical.

**Stage what you have reviewed, not everything that happens to be modified.**

---

## 19. What Is a Commit?

A **commit** records a snapshot of staged changes in Git history.

```powershell
git commit -m "fix: initialize login button before use"
```

A good commit has a clear description, coherent purpose, and reviewable scope. It should represent a logical unit of work.

---

## 20. Writing Meaningful Commit Messages

Common convention: `type: short description`.

```text
feat: add task creation form
fix: reject empty task titles
refactor: simplify schedule validation
test: cover task creation errors
docs: update architecture notes
style: align login form elements
```

| Type | Purpose |
|---|---|
| `feat` | New functionality |
| `fix` | Bug correction |
| `refactor` | Structural improvement |
| `test` | Test changes |
| `docs` | Documentation |
| `style` | Formatting or presentation changes |
| `chore` | Maintenance work |

These are conventions, not Git requirements. Weak: `update`. Better: `fix: prevent login crash from uninitialized button`.

---

## 21. Commit Review Workflow

```powershell
git status --short
git diff
git diff --cached
git diff --cached --check
git commit -m "fix: correct login button initialization"
git status
```

A clean working tree does not mean tests passed; it only describes the Git state.

---

# Part VI — GitHub Collaboration

## 22. What Is a Remote Repository?

A **remote repository** is another Git repository that exchanges commits with your local repository.

```powershell
git remote -v
git push -u origin feature/login-ui
```

The `-u` option establishes upstream tracking. Pushing publishes commits remotely; it does not automatically merge them into the primary branch.

---

## 23. What Is a Pull Request?

A **Pull Request (PR)** proposes merging changes from one branch into another. It typically provides a description, commits, diffs, automated checks, review comments, and approval or requested changes.

```text
feature branch → Pull Request → Code Review → Verification → Merge → main
```

**A PR should communicate what changed, why it changed, and how it was verified.**

---

## 24. Writing a Good Pull Request

```markdown
# Pull Request

## Summary
Briefly describe the change.

## Related Task
Reference the approved feature or bug.

## Changes Made
- Change 1
- Change 2

## Out of Scope
List work intentionally excluded.

## Verification
- [ ] Relevant tests executed
- [ ] Build completed
- [ ] Manual verification completed
- [ ] Git diff reviewed

## Test Results
List actual commands and outcomes.

## Risks and Limitations
Document remaining concerns.
```

Example title: `style: improve GroupTrack login screen alignment`. Do not claim verification passed unless it was performed.

---

## 25. Code Review Fundamentals

**Code Review** is the systematic examination of proposed changes before acceptance. Review for correctness, scope, architecture, testing, security, maintainability, and unrelated modifications.

**Code review evaluates quality and suitability, not merely whether code compiles.**

---

# Part VII — Codex-Generated Change Auditing

## 26. Why AI-Generated Code Needs Review

Codex can generate code quickly, but speed does not eliminate oversight. Problems can include incorrect interpretation, out-of-scope changes, unapproved dependencies, unnecessary abstractions, modified interfaces, missing validation, removed behavior, inadequate tests, and unexpected configuration consequences.

Treat Codex output as a proposed implementation to evaluate against project requirements.

---

## 27. Read-Only Codex Code Review

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
git status
codex --sandbox read-only
```

Review prompt:

```text
Perform a read-only code review. Read applicable AGENTS.md and project
instructions. Review working-tree changes, staged changes, and relevant
untracked files for correctness, scope, architecture, unintended edits,
dependencies, security, regressions, missing tests, and documentation.
For each finding provide severity, file/location, evidence, impact, and
recommended correction. Do not modify, stage, commit, or push. Distinguish
confirmed findings from potential concerns. Stop after the report.
```

---

## 28. Codex Review Severity Levels

| Severity | Meaning | Example |
|---|---|---|
| Critical | Severe security or data-integrity risk | Unauthorized access |
| High | Major functional defect | Authentication bypass |
| Medium | Meaningful correctness problem | Incorrect validation |
| Low | Minor issue | Unclear naming |
| Informational | Suggestion without confirmed defect | Optional readability improvement |

Severity should reflect evidence and impact. Not every style suggestion is a blocking defect.

---

## 29. Scope Creep Detection

**Scope Creep** expands work beyond approved requirements. A login spacing task should not unexpectedly rewrite authentication, add a UI library, change Firebase configuration, or redesign registration.

```text
Compare Git changes against approved scope. Identify expected and
unexpected edits, dependencies, abstractions, removed behavior, and
changes requiring approval. Do not edit. Recommend the smallest change set.
```

---

## 30. Reviewing Dependencies

| Project | Files to Inspect |
|---|---|
| Android | `build.gradle`, `build.gradle.kts`, `libs.versions.toml`, `settings.gradle` |
| Laravel | `composer.json`, `composer.lock`, `package.json`, `package-lock.json` |

Check if a dependency is necessary, already available, compatible, approved, and recorded in the correct lockfile.

---

## 31. Reviewing Sensitive Files

Files such as `.env`, `.env.local`, `google-services.json`, configuration, migrations, authentication code, and deployment settings deserve special caution. Risks include secret exposure, incorrect environments, data loss, auth regressions, and deployment failures.

Do not commit secrets. Review `.gitignore` and repository security practices. If credentials are exposed, follow the provider's rotation and incident-response guidance.

---

## 32. Review Findings vs Implementation Changes

```text
Codex Review → Findings Report → Developer Approval
             → Focused Corrections → Tests and Diff Review
```

A review finding is not automatic permission to modify the repository.

---

# Part VIII — Safe Rollback and Recovery

## 33. What Is Rollback?

**Rollback** returns software or changes toward an earlier state. Choose the recovery method based on whether changes are uncommitted, committed, or shared.

---

## 34. Understanding `git restore`

Unstage without discarding working-tree edits:

```powershell
git restore --staged -- path/to/file
```

Discard unstaged edits to a tracked file:

```powershell
git restore --worktree -- path/to/file
```

The latter can permanently discard work. Inspect the diff and confirm ownership and intent first.

---

## 35. Understanding `git revert`

`git revert` creates a new commit reversing an earlier commit:

```powershell
git log --oneline
git revert <commit-hash>
```

It preserves history and is generally preferable for changes already shared. Later dependent changes can cause conflicts.

---

## 36. Understanding `git reset`

| Mode | General Effect |
|---|---|
| `--soft` | Moves HEAD; preserves index and working tree |
| `--mixed` | Moves HEAD and resets index; preserves files |
| `--hard` | Moves HEAD and resets index and working tree |

`git reset --hard` can permanently discard uncommitted work. Resetting shared commits can create collaboration problems. Prefer `git restore --staged` for accidental staging and `git revert` for shared commits. Avoid destructive history rewriting unless understood and approved.

---

## 37. Safe Recovery Decision Guide

| Situation | Preferred Starting Point |
|---|---|
| Accidentally staged a file | `git restore --staged` |
| Inspect earlier file version | `git show` |
| Reverse a shared commit | `git revert` |
| Discard uncommitted edit | Review diff, then consider `git restore` |
| Preserve unfinished work | Carefully reviewed stash or dedicated commit |
| Unsure who owns changes | Stop and inspect |

---

## 38. Git Stash

```powershell
git status
git stash push -m "WIP login UI"
git stash list
git stash show -p 'stash@{0}'
git stash apply 'stash@{0}'
```

Stash may omit untracked files; applying may conflict. Stash is temporary work management, not long-term reviewable history.

---

# Part IX — Merge Conflicts and Collaboration

## 39. What Is a Merge Conflict?

A **merge conflict** occurs when Git cannot automatically reconcile changes, such as two developers editing the same line.

```text
<<<<<<< HEAD
button.setText("Login");
=======
button.setText("Sign In");
>>>>>>> feature/login-ui
```

Understand both changes, confirm the requirement, choose or combine the right behavior, remove markers, test, review the resolved diff, and complete the repository workflow. Do not blindly select one side.

---

## 40. Codex and Merge Conflicts

```text
Analyze the current conflict. Read applicable AGENTS.md. Do not modify yet.
Explain conflicting files, changes on each side, intended behavior,
project constraints, possible resolutions, risks, and verification.
Do not choose without sufficient evidence. Stop after analysis.
```

Conflict resolution is a requirements and correctness decision, not merely text editing.

---

# Part X — Android and Laravel Workflows

## 41. Android Example — GroupTrack Login UI

Task: improve alignment of the existing login screen.

Approved scope: spacing, layout consistency, current navigation and authentication, and reuse of existing resources.

```powershell
git switch -c feature/login-ui-alignment
codex --sandbox workspace-write
```

Codex prompt:

```text
Improve spacing and alignment of the existing GroupTrack login screen.
Limit scope to its XML layout and necessary resources. Do not change
Firebase, authentication, navigation, or unrelated screens. Do not add
a UI library. Make the smallest necessary changes, run relevant builds,
and review the diff. Do not commit or push. Report modified files and
actual test results, then stop.
```

After Codex:

```powershell
git status --short
git diff
.\gradlew.bat :app:assembleDebug
```

After review, stage approved files, inspect the staged diff, then commit. UI changes should not unexpectedly alter authentication or add dependencies.

---

## 42. Laravel Example — Validation Bug Fix

Task: reject an empty task title according to the API requirement.

Approved scope: validation rule, regression test, existing response contract.

```powershell
git switch -c fix/task-title-validation
```

Codex should inspect the actual route, controller, Form Request, and tests first. Make the smallest justified fix, preserve API structure, avoid unrelated changes/dependencies/production data, run focused tests, confirm test database isolation, and review the diff. Do not commit or push. Report actual results.

Example, if the relevant test exists:

```powershell
php artisan test --filter=task_title
git diff --stat
git diff
```

---

## 43. Laravel and Next.js Monorepo Example

```text
ecommerce/
├── frontend/app/
├── backend/app/
├── backend/routes/
├── backend/tests/
└── .git/
```

For a product response mapping defect, determine whether the issue is in Laravel or Next.js, whether the API contract changed, whether both apps need edits, and which tests apply.

| Component | Proposed Change | Justification |
|---|---|---|
| Laravel response | None if contract is correct | Preserve API |
| Next.js mapping | Correct mismatched field | Address defect |
| Frontend test | Add regression coverage | Prevent recurrence |
| Database schema | None | Out of scope |

A multi-project repository does not justify modifying every application for every bug.

---

# Part XI — Approval and Automation Boundaries

## 44. What Codex Can Do vs What Should Require Approval

Technical capability is not authorization.

| Action | Suggested Control |
|---|---|
| Inspect status, diffs, history | Investigation |
| Edit approved files | After task authorization |
| Run safe local tests | After environment review |
| Stage or commit | Explicit authorization |
| Push branches | Explicit authorization |
| Merge pull requests | Human approval |
| Rewrite shared history | Special approval |
| Delete branches or files | Confirm intent and impact |

Separate permission to write code from permission to publish it.

---

## 45. Reusable AGENTS.md Git Rules

```markdown
## Git and Code Review

- Inspect Git status before modifying files.
- Preserve existing user changes.
- Work only within approved scope.
- Do not modify unrelated files.
- Do not stage, commit, or push unless requested.
- Do not force-push or rewrite shared history.
- Do not discard uncommitted changes.
- Review modified and untracked files.
- Run relevant verification and report actual results.
- Explain unexpected modifications.
- Stop if destructive operations are required.
```

Use as a reference; do not duplicate suitable repository guidance.

---

## 46. Reusable Codex Change Review Prompt

```text
Perform a read-only audit. Read applicable AGENTS.md and project docs.
Inspect status, unstaged/staged changes, relevant untracked files, test
results, and dependencies. Approved task: [Describe it.] Evaluate correctness,
scope, architecture, security, regressions, coverage, unnecessary edits,
and missing documentation. Report severity, evidence, file, and action.
Do not edit, stage, commit, or push. Stop after reporting.
```

---

## 47. Reusable Codex Commit Preparation Prompt

```text
Prepare the approved task for a developer-reviewed commit. Read AGENTS.md.
Do not stage or commit. Inspect status, changed/staged/untracked files,
and test results. Report summary, proposed files to stage, exclusions,
verification, suggested commit message, and concerns. Do not discard changes
or push. Stop after recommendations.
```

Let Codex prepare the evidence; let the developer approve the repository operation.

---

# Part XII — Practical Exercises

## 48. Exercise A — Repository Reconnaissance

Run:

```powershell
git status
git branch --show-current
git log --oneline -5
git diff --stat
```

Identify the branch, modified files, staged changes, untracked files, and latest commit. Deliver a short status report.

---

## 49. Exercise B — Create a Feature Branch

Inspect status, confirm the base, create a feature branch, and verify it:

```powershell
git switch -c feature/practice-ui
```

---

## 50. Exercise C — Diff Analysis

Codex modifies `activity_login.xml`, `LoginActivity.java`, and `build.gradle` for the task "Improve login layout spacing."

Which file is clearly related? Why review Java changes and dependency changes? What verification is needed? Should all files be staged automatically?

Deliver a file-level change audit.

---

## 51. Exercise D — Selective Staging

Make a harmless practice change, inspect it, stage only the approved file, inspect the index, then unstage if needed:

```powershell
git add path/to/approved-file
git diff --cached
git restore --staged -- path/to/approved-file
```

---

## 52. Exercise E — Commit Message Design

Write one commit message for each: fix registration validation; add task creation; improve login alignment; add missing unit tests.

---

## 53. Exercise F — Read-Only Codex Audit

Open a Git-managed project, inspect changes, start Codex read-only, submit the Section 46 review prompt, and evaluate its findings. Look for real file references, evidence, scope findings, honest test reporting, and no repository edits.

---

## 54. Exercise G — Pull Request Planning

For a small validation fix, write a PR description with summary, scope, files changed, verification commands, actual results, limitations, and reviewer notes.

---

## 55. Exercise H — Safe Recovery Analysis

Choose a recovery strategy for: an accidentally staged file; an incorrect uncommitted edit; a defective shared commit; a branch switch blocked by local changes; and a merge conflict. Identify commands, risks, inspection, and approval needs.

---

# Part XIII — Knowledge Review

## 56. Knowledge Check

1. What is Git?
2. How does Git differ from GitHub?
3. What is a repository?
4. What is the working directory?
5. What is the staging area?
6. What is a commit?
7. What is a branch?
8. Why use branches for Codex tasks?
9. What does `git status` show?
10. What does `git diff` show?
11. How do staged and unstaged diffs differ?
12. Why are untracked files important during review?
13. What is selective staging?
14. What makes a good commit message?
15. What is a remote repository?
16. What is a pull request?
17. What is code review?
18. Why review AI-generated changes?
19. What is scope creep?
20. Why inspect dependency changes?
21. What is `git restore` used for?
22. What is `git revert` used for?
23. Why can `git reset --hard` be dangerous?
24. What is a merge conflict?
25. Why should conflicts not be resolved blindly?
26. Which Git actions require explicit approval?
27. How should Codex report unexpected changes?
28. Why are Git review and automated testing complementary?

---

## 57. Key Takeaways

1. **Git provides traceable version history.**
2. **Git and GitHub serve different purposes.**
3. **Working directory, staging area, and commit history are distinct.**
4. **Inspect status before Codex edits files.**
5. **Use focused branches for approved tasks.**
6. **Review actual diffs, not only summaries.**
7. **Every changed file needs a task-related reason.**
8. **Stage only reviewed changes.**
9. **Commits should represent coherent work.**
10. **Pull requests provide review checkpoints.**
11. **Review correctness, scope, architecture, security, and tests.**
12. **Scrutinize dependencies and configuration changes.**
13. **Use `git revert` for shared commit reversals.**
14. **Avoid destructive Git operations without understanding them.**
15. **Codex must not stage, commit, push, or merge without authorization.**
16. **Tests and diffs provide complementary evidence.**
17. **Human review remains important.**

---

## 58. Official References

### OpenAI Codex

1. [OpenAI Codex Documentation](https://developers.openai.com/codex/)
2. [Codex CLI](https://developers.openai.com/codex/cli/)
3. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
4. [Codex Security](https://developers.openai.com/codex/security/)
5. [AGENTS.md Guidance](https://developers.openai.com/codex/guides/agents-md/)

### Git

6. [Git Documentation](https://git-scm.com/docs)
7. [Git Status](https://git-scm.com/docs/git-status)
8. [Git Diff](https://git-scm.com/docs/git-diff)
9. [Git Branch](https://git-scm.com/docs/git-branch)
10. [Git Switch](https://git-scm.com/docs/git-switch)
11. [Git Add](https://git-scm.com/docs/git-add)
12. [Git Commit](https://git-scm.com/docs/git-commit)
13. [Git Restore](https://git-scm.com/docs/git-restore)
14. [Git Revert](https://git-scm.com/docs/git-revert)
15. [Git Reset](https://git-scm.com/docs/git-reset)
16. [Git Stash](https://git-scm.com/docs/git-stash)

### GitHub

17. [About Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
18. [Reviewing Proposed Changes](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request)
19. [Pull Request Reviews](https://docs.github.com/en/pull-requests)

---

## 59. Lesson Completion Checklist

- [ ] Understand Git fundamentals.
- [ ] Differentiate Git and GitHub.
- [ ] Understand repositories and commits.
- [ ] Understand working directory and staging.
- [ ] Inspect Git status and history.
- [ ] Create a feature branch.
- [ ] Inspect diffs and untracked files.
- [ ] Perform file-level change audits.
- [ ] Detect scope creep and review dependencies.
- [ ] Stage specific files.
- [ ] Write meaningful commit messages.
- [ ] Understand remotes and pull requests.
- [ ] Perform systematic code reviews.
- [ ] Use Codex for read-only audits.
- [ ] Understand restore, revert, reset, and stash.
- [ ] Understand merge conflicts.
- [ ] Apply approval checkpoints.
- [ ] Complete practical exercises and knowledge check.

---

## 60. Next Lesson

**[[08 - Hands-on Feature Implementation]]**

The final Phase 3 lesson combines the development lifecycle, codebase exploration, planning, task decomposition, incremental implementation, debugging, testing, and Git review. The objective is to complete one small, approved feature using a disciplined agentic coding workflow.

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.7*  
*Learning Principle: Inspect, implement, verify, review, approve, and commit deliberately.*
