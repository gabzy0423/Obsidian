# 07 — Advanced Codebase Management

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 5 — Advanced Codex  
> **Lesson:** 5.7 — Advanced Codebase Management  
> **Difficulty:** Advanced  
> **Environment:** Windows, PowerShell, VS Code, Git, Codex CLI  
> **Prerequisite:** [[06 - Context and Token Efficiency]]  
> **Next Phase:** Phase 6 — Real-World Application  
> **Status:** Theory Documented

---

# 1. Introduction

As software projects grow, managing the codebase becomes more challenging. A small application might contain a few controllers, models, screens, and configuration files. A larger application may contain multiple modules, shared services, complex database relationships, frontend and backend applications, third-party integrations, multiple testing layers, architectural constraints, deployment configurations, and extensive documentation.

Even a small change may affect several parts. Changing a Laravel API response could affect the controller, API resource, frontend TypeScript interfaces, UI components, tests, and API documentation.

An AI coding agent must understand these relationships before modifying the system.

**Advanced Codebase Management** is the practice of exploring, understanding, modifying, and maintaining complex repositories while preserving architecture and existing behavior.

## Core Principle

**A successful code change is not simply one that works locally. It must also respect the surrounding architecture, dependencies, contracts, and verification requirements.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Understand the challenges of large codebases.
2. Navigate unfamiliar repositories systematically.
3. Create repository architecture maps.
4. Identify module boundaries.
5. Understand dependency relationships.
6. Analyze change impact.
7. Differentiate monorepos and multi-repository architectures.
8. Understand multi-module projects.
9. Design safe refactoring workflows.
10. Preserve existing behavior during refactoring.
11. Coordinate Git changes.
12. Understand Git worktrees.
13. Maintain documentation consistency.
14. Design appropriate testing strategies.
15. Recognize architectural drift.
16. Prevent unnecessary abstractions.
17. Apply context-efficient repository exploration.
18. Use Codex for codebase investigation.
19. Understand cross-module integration risks.
20. Apply advanced codebase management to Android, Laravel, and Next.js projects.

---

# Part II — Understanding Large Codebases

## 3. What Is a Codebase?

A **codebase** is the collection of source code and related artifacts used to develop and maintain a software system. It may include application source files, configuration, dependency manifests, database migrations, tests, documentation, build scripts, and development tooling.

```text
application/
├── app/
├── config/
├── database/
├── docs/
├── tests/
├── AGENTS.md
└── README.md
```

A repository is a version-controlled location containing some or all of these artifacts. A single software system may span multiple repositories.

---

## 4. What Makes a Codebase Complex?

Complexity is not determined solely by file count.

| Factor | Description |
|---|---|
| Size | Number of files and modules |
| Dependencies | Relationships between components |
| Coupling | Degree to which components depend on one another |
| Architecture | Organization of responsibilities |
| Integration | Communication between subsystems |
| Business rules | Domain-specific constraints |
| State | Shared application or database state |
| History | Accumulated implementation decisions |
| Testing | Difficulty verifying changes |
| Documentation | Quality of recorded system knowledge |

A 50-file tightly coupled app may be harder to maintain than a 200-file app with clear boundaries.

**Complexity depends on relationships and responsibilities, not simply repository size.**

---

# Part III — Repository Navigation

## 5. Why Repository Exploration Comes First

Before implementation, understand what the project does, its technologies and structure, where functionality lives, applicable instructions, available tests, and existing modified files.

```text
Developer Request → Read Project Instructions → Inspect Structure
→ Locate Relevant Module → Trace Dependencies → Review Tests
→ Plan Minimal Change
```

**Explore enough to understand the task without reading the entire repository unnecessarily.**

---

## 6. Repository Reconnaissance

Repository reconnaissance is the initial inspection of a codebase.

Ask: Where is the application entry point? Where are business rules, models, integrations, tests, build commands, and project-specific coding rules?

| File | Typical Purpose |
|---|---|
| `README.md` | Project overview |
| `AGENTS.md` | Agent instructions |
| `package.json` | JavaScript dependencies and scripts |
| `composer.json` | PHP dependencies and autoloading |
| `build.gradle` / `build.gradle.kts` | Gradle build configuration |
| `settings.gradle` / `settings.gradle.kts` | Gradle modules |
| `.env.example` | Environment variable examples |
| `docs/` | Architecture and feature documentation |
| `tests/` | Automated verification |

Not every project contains all these files. Inspect the actual repository.

---

## 7. Windows PowerShell Repository Inspection

```powershell
Get-Location
Get-ChildItem
git status
git rev-parse --show-toplevel
git ls-files
rg -n "ScheduleController" app
rg --files | rg "Schedule|Route"
```

Targeted searches reduce unnecessary context loading and focus investigation.

---

# Part IV — Architecture Mapping

## 8. What Is Architecture Mapping?

Architecture mapping documents major system components and their relationships.

```text
User Interface → API Endpoints → Controller
      → Business Service → Data Access → Database
```

This conceptual layered architecture helps identify responsibilities, data flow, dependency direction, integration points, and potential change impact. Actual applications may differ.

---

## 9. Architecture Map vs Folder Tree

A folder tree describes where files live; an architecture map describes how components interact.

```text
app/
├── Controllers/
├── Models/
└── Services/
```

```text
Controller → Service → Model → Database
```

Folder organization alone does not prove actual architecture; inspect dependencies.

---

## 10. Architecture Mapping Workflow

1. Identify entry points: Android Activity, Laravel route, or Next.js page/route handler.
2. Trace execution through controllers, services, repositories, models, and APIs.
3. Identify where state is stored or modified.
4. Inspect function signatures, API structures, database relationships, and shared interfaces.
5. Record a concise map supported by source files.

**Document the implementation that exists, not the architecture an agent assumes exists.**

---

# Part V — Module Boundaries

## 11. What Is a Module?

A module is a cohesive group of functionality with a defined responsibility.

```text
Application
├── Authentication
├── Product Management
├── Inventory
└── Reporting
```

Clear modules support navigation, lower coupling, focused tests, ownership, and safer changes.

---

## 12. High Cohesion and Low Coupling

**Cohesion** describes how related a module's responsibilities are. **Coupling** describes dependencies between modules; lower coupling generally makes independent changes easier.

```text
Authentication Module
├── Login
├── Logout
└── Session Validation
```

Avoid mixing unrelated inventory, reporting, and scheduling responsibilities into authentication.

**Group related responsibilities while minimizing unnecessary dependencies.**

---

## 13. Module Ownership

Identify the owner before changing a feature.

| Concern | Possible Owner |
|---|---|
| Login validation | Authentication module |
| Product pricing | Product or pricing service |
| Bus assignment | Scheduling module |
| Route geometry | Route management module |
| UI alignment | UI resources or components |

Do not place logic in the first convenient file. Extend the existing responsibility boundary consistently.

---

# Part VI — Dependency Analysis

## 14. What Is a Dependency?

A dependency exists when one component relies on another.

```text
ProductPage → ProductApiClient → Laravel API → ProductService → Product Model
```

A change may affect downstream consumers.

---

## 15. Dependency Types

| Type | Example |
|---|---|
| Code dependency | Controller calls service |
| Data dependency | Model relies on schema |
| API dependency | Frontend expects backend response |
| Build dependency | Project requires a library |
| Configuration dependency | Feature needs an environment setting |
| Runtime dependency | Application uses external service |
| Documentation dependency | Implementation follows documented contract |

Some dependencies exist through runtime behavior, configuration, or contracts, not visible imports.

---

## 16. Direct vs Indirect Dependencies

Direct: `A → B`. Indirect: `A → B → C`; A may be affected by C without importing it directly.

```text
Next.js Product Page → API Client → Laravel Product Endpoint → Database Query
```

A database field change may eventually affect the frontend.

**Impact analysis must consider direct and indirect dependencies.**

---

# Part VII — Change Impact Analysis

## 17. What Is Change Impact Analysis?

It identifies system parts that may be affected by a proposed change.

For example, renaming a response field can affect:

```text
Laravel API Resource → JSON Response → TypeScript Interface
→ Frontend Component → Frontend Tests
```

Its purpose is to prevent unintended regressions.

---

## 18. Change Impact Analysis Workflow

```text
Requested Change → Target Component → Direct Dependencies
→ Downstream Consumers → Contracts → Tests → Risk → Plan
```

Ask which files change, who consumes the behavior, whether API/schema changes are involved, whether users are affected, and what tests/docs need updates.

---

## 19. Change Impact Matrix

| Change | Potential Impact | Risk |
|---|---|---|
| Button padding | UI layout | Usually low |
| Rename internal method | Call sites and tests | Low to medium |
| Change API field | Frontend consumers | Medium to high |
| Database migration | Models, queries, data | Medium to high |
| Authentication change | Security and user access | High |
| Shared service refactor | Multiple modules | Potentially high |

Risk levels are illustrative and depend on the application.

---

## 20. Blast Radius

**Blast radius** describes how much of the system a change may affect.

```text
Small: One UI Component
Large: Shared Authentication Service → Admin, Staff, Customer
```

**The larger the blast radius, the stronger the need for planning, tests, and review.**

---

# Part VIII — Multi-Module Projects

## 21. What Is a Multi-Module Project?

A multi-module project contains organized units within a larger application.

```text
android-project/
├── app/
├── core/
└── feature-auth/
```

This is illustrative, not a recommendation to restructure a simple project.

Benefits include boundaries, potential build isolation, reuse, and ownership. Tradeoffs include configuration, dependency management, build complexity, and coordination.

**Use modules when the architecture benefits, not simply because the project grows.**

---

## 22. Single-Module vs Multi-Module Android

| Aspect | Single Module | Multi-Module |
|---|---|---|
| Setup | Simpler | More complex |
| Navigation | Good for small apps | Useful for larger codebases |
| Build config | Less | More |
| Dependency boundaries | Fewer formal boundaries | Can enforce separation |
| Suitable for | Small MVPs | Larger apps with clear boundaries |

A single module can be appropriate for a small Android MVP.

---

# Part IX — Monorepos vs Multi-Repositories

## 23. What Is a Monorepo?

A **monorepo** is one version-controlled repository containing multiple projects or packages.

```text
ecommerce/
├── frontend/
├── backend/
├── shared/
├── docs/
└── .git/
```

Potential benefits: centralized history, coordinated changes, shared tooling, easier cross-project review. Challenges: repository size, CI complexity, broad permissions, dependency management.

---

## 24. What Is a Multi-Repository Architecture?

Separate projects or services use separate Git repositories.

```text
ecommerce-frontend/.git/
ecommerce-backend/.git/
```

Benefits: independent versioning and release cycles, repository-level ownership, smaller repositories. Challenges: cross-repository coordination, synchronized API contracts, separate CI, tracking dependencies.

---

## 25. Monorepo vs Multi-Repository Comparison

| Aspect | Monorepo | Multi-Repository |
|---|---|---|
| Git history | Centralized | Separate |
| Cross-project changes | Easier to coordinate | Require coordination |
| CI/CD | Shared or scoped | Usually separate |
| Access control | Potentially broader | Repository-specific |
| Dependency management | May be centralized | Often independent |
| Release cycles | Can be independent | Naturally separate |
| Complexity | Tooling | Coordination |

Neither is universally better; choose based on ownership, deployment, tooling, and collaboration.

---

## 26. Identifying the Actual Git Structure

```powershell
git rev-parse --show-toplevel
```

Run from frontend and backend directories. Matching roots may indicate one repository; different roots indicate separate repositories. A parent folder containing two apps does not prove they share a Git repository.

# Part X — Safe Refactoring

## 27. What Is Refactoring?

Refactoring improves internal code structure while preserving intended external behavior.

Examples include extracting repeated logic, renaming unclear internal variables, separating responsibilities, simplifying methods, and removing unnecessary duplication.

Adding a feature, changing an API contract, altering business rules, or changing user-visible behavior is not pure refactoring. Those changes need separate requirements and verification.

## 28. Refactoring vs Rewriting

| Refactoring | Rewriting |
|---|---|
| Incremental improvements | Replaces substantial implementation |
| Preserves intended behavior | May introduce new architecture |
| Smaller changes | Larger scope |
| Easier incremental verification | Requires broader verification |
| Usually lower risk | Potentially higher risk |

**Prefer focused refactoring when it solves the problem without replacing working systems unnecessarily.**

## 29. Safe Refactoring Workflow

```text
Identify Problem
       |
       v
Understand Existing Behavior
       |
       v
Identify Tests
       |
       v
Create Minimal Plan
       |
       v
Make Small Change
       |
       v
Run Focused Tests
       |
       v
Review Diff
       |
       v
Run Broader Validation
```

Recommended rules:

1. Preserve public contracts unless explicitly approved.
2. Separate behavior changes from structural changes where practical.
3. Make small, reviewable edits.
4. Avoid unrelated cleanup.
5. Verify each meaningful change.
6. Update documentation when necessary.

## 30. Characterization Tests

A characterization test records existing observable behavior. Before refactoring a scheduling algorithm, identify representative inputs, record expected outputs, add tests around those behaviors, refactor, and confirm the tests still pass.

Existing behavior is not automatically correct. If a known bug exists, document it separately and distinguish the bug fix from behavior-preserving refactoring.

---

# Part XI — Git Coordination

## 31. Why Git Matters

Git records source changes and helps developers inspect modifications, compare implementations, review changes, coordinate branches, recover earlier versions, and trace regressions.

**Version control supports verification, but it does not replace testing or architectural review.**

## 32. Essential Git Commands

```powershell
git status
git branch --show-current
git diff
git diff --cached
git diff --check
git log --oneline -10
git diff --name-only
```

These inspect status, branch, unstaged and staged changes, whitespace errors, recent history, and changed files. Before an agent edits files, identify existing uncommitted changes and preserve work unrelated to the task.

## 33. Focused Changes

A good development change relates to one objective and is understandable, reviewable, testable, and appropriately documented.

Good scope: `Fix schedule validation conflict.`

Poor scope: `Fix schedule validation, redesign the dashboard, rename all models, and restructure the project.`

**Small, coherent changes are easier to review and recover than unrelated modifications bundled together.**

---

# Part XII — Git Worktrees

## 34. What Is a Git Worktree?

A Git worktree allows multiple working directories to be associated with one repository. Each has its own working directory and checked-out branch, while sharing the underlying Git object database and many refs.

```text
GroupTrack/                 main branch
GroupTrack-ui-review/       review-ui branch
```

## 35. Inspect Worktrees

```powershell
git worktree list
```

Example, run from the main repository when the branch and target directory are available:

```powershell
git worktree add -b review-ui ..\GroupTrack-ui-review
git worktree list
```

Worktrees do not automatically merge changes or eliminate logical conflicts. They may have separate local build artifacts, while external services and databases may still be shared. Use them when isolation provides a real benefit.

## 36. Worktree Coordination

```text
Main Worktree
      |
      +-- UI Worktree
      |
      +-- Backend Worktree
```

For integration, review each branch, inspect changed files, resolve conflicts, run integrated tests, and confirm expected behavior.

**Worktree isolation reduces filesystem conflicts, not integration responsibility.**

---

# Part XIII — Regression Investigation

## 37. What Is a Regression?

A regression occurs when a change causes previously working behavior to fail. Causes include changed assumptions, missing test coverage, shared service modifications, API contract changes, and incorrect dependency handling.

## 38. Git Bisect

`git bisect` uses binary search across commits to help identify which change introduced a regression.

```text
Known Good Commit
        |
        v
Candidate Commits
        |
        v
Test Selected Commit
        |
        v
Mark Good or Bad
        |
        v
Narrow Search
        |
        v
Identify Regression Commit
```

Common commands:

```powershell
git bisect start
git bisect bad
git bisect good <known-good-commit>
git bisect reset
```

This is a conceptual sequence, not a command block to run without preparation. Use a clean working tree, identify a reliable good commit, and understand the tests first.

---

# Part XIV — Documentation Synchronization

## 39. Why Documentation Matters

Documentation helps developers and agents understand architecture, business rules, API contracts, database relationships, testing procedures, and development constraints. It can become outdated. For example, docs may say a controller validates requests after validation has moved to a Form Request.

## 40. Documentation Drift

Documentation drift occurs when written documentation no longer matches the implementation. Common causes include refactoring without doc updates, undocumented features, removed modules still referenced, changed API contracts, and outdated setup commands.

**Documentation should evolve alongside meaningful architectural and behavioral changes.**

## 41. Documentation Ownership

| Document | Primary Responsibility |
|---|---|
| `README.md` | Setup and project overview |
| `AGENTS.md` | Durable agent instructions |
| Architecture docs | System structure and boundaries |
| API docs | Request and response contracts |
| Database schema docs | Data relationships |
| Feature docs | Functional behavior |
| Testing docs | Validation procedures |

Avoid duplicating complete information across documents; reference the canonical source where possible.

## 42. Documentation Update Checklist

- [ ] Architecture documentation still matches the code.
- [ ] API contracts remain accurate.
- [ ] Database relationships are documented if changed.
- [ ] Setup commands remain valid.
- [ ] Testing instructions remain accurate.
- [ ] `AGENTS.md` still reflects project rules.
- [ ] Removed functionality is no longer described as active.
- [ ] No unnecessary duplicate documentation was introduced.

---

# Part XV — Testing Strategies

## 43. Why Testing Matters

An agent may produce code that appears correct but introduces regressions. Testing provides evidence that the implementation behaves as expected.

**Generated code is an implementation candidate until appropriately verified.**

## 44. Testing Levels

| Level | Purpose |
|---|---|
| Static analysis | Detect certain code issues without executing behavior |
| Unit tests | Verify individual functions or classes |
| Integration tests | Verify interactions between components |
| Contract tests | Verify interfaces between systems |
| End-to-end tests | Verify complete user workflows |
| Manual testing | Inspect behavior not fully covered by automation |
| Regression tests | Detect previously working behavior that breaks |

## 45. Testing Pyramid

```text
              /\
             /  \
            / E2E\
           /------\
          /Integration\
         /-------------\
        /   Unit Tests  \
       /-----------------\
```

Many projects benefit from numerous focused unit tests, appropriate integration tests, and a smaller set of end-to-end tests. The ideal distribution depends on the application and its risks.

## 46. Risk-Based Testing

| Change | Suggested Verification |
|---|---|
| Button spacing | UI preview and relevant device checks |
| Validation rule | Focused validation tests |
| API response | Contract and integration tests |
| Database migration | Migration and data-integrity checks |
| Authentication logic | Security, authorization, and integration tests |
| Shared service refactor | Focused and broader regression tests |

**The wider the impact of a change, the broader the verification that may be necessary.**

---

# Part XVI — Preventing Architectural Drift

## 47. What Is Architectural Drift?

Architectural drift occurs when implementation gradually moves away from intended architectural rules.

Documented architecture may be `Controller -> Service -> Model`, while a drifted controller accumulates business rules, database queries, external API calls, and report generation.

## 48. Common Causes

- Implementing features without reading architecture docs.
- Adding logic to convenient but inappropriate files.
- Repeating business rules.
- Introducing unnecessary abstractions.
- Bypassing established services.
- Creating inconsistent naming conventions.
- Failing to review cross-module dependencies.

## 49. Preventing Architectural Drift

1. Document durable rules in `AGENTS.md`.
2. Define which modules own responsibilities.
3. Prefer small, focused changes.
4. Review changes against architectural boundaries.
5. Test important invariants.
6. Keep architecture documentation aligned with implementation.

**Architecture should guide implementation without becoming an excuse for unnecessary complexity.**

# Part XVII — Codex Workflow for Large Codebases

## 50. Recommended Workflow

```text
1. Understand Request
          |
          v
2. Read AGENTS.md
          |
          v
3. Inspect Git Status
          |
          v
4. Locate Relevant Module
          |
          v
5. Trace Dependencies
          |
          v
6. Analyze Change Impact
          |
          v
7. Create Minimal Plan
          |
          v
8. Implement Approved Scope
          |
          v
9. Run Focused Tests
          |
          v
10. Run Broader Validation
          |
          v
11. Review Git Diff
          |
          v
12. Update Relevant Docs
          |
          v
13. Report Verified Results
```

**Large codebases require disciplined investigation and verification, not necessarily larger implementations.**

## 51. Codex Exploration Prompt

```text
Read AGENTS.md and relevant project documentation.

Inspect the repository structure.

Identify the major modules, entry points,
and important dependency relationships.

Do not modify files.

Create a concise architecture map
supported by actual file references.

Clearly distinguish confirmed findings
from assumptions.
```

## 52. Codex Change Impact Prompt

```text
Analyze the potential impact of changing
the specified module.

Identify:
- Direct dependencies
- Indirect dependencies
- Public interfaces
- Database relationships
- Related tests
- Documentation affected

Do not modify files.

Return a risk assessment and
recommended verification plan.
```

## 53. Codex Refactoring Prompt

```text
Refactor the specified component.

Read applicable project instructions.

Preserve intended external behavior.

Do not modify unrelated modules.

Do not introduce new dependencies
unless necessary and approved.

Make the smallest coherent change.

Run relevant tests.

Review the diff and report
all changes and verification results.
```

---

# Part XVIII — Practical Android Example

## 54. GroupTrack — Android Codebase Management

GroupTrack uses Android Studio, Java, XML layouts, Firebase, and a documented MVP scope. Its actual structure should be verified from source files.

```text
Android UI
    |
    v
Activity / Screen Logic
    |
    v
Application Logic
    |
    v
Firebase Integration
```

## 55. Scenario — Adding UI Functionality

For an approved sign-in screen change, inspect `AGENTS.md`, UI documentation, the sign-in XML, related Java Activity, and referenced resources.

Potentially affected areas include layout constraints, resource references, UI event handling, and authentication navigation.

```text
Read AGENTS.md and relevant UI documentation.

Inspect the existing sign-in screen.

Identify the files and dependencies
required for the approved UI change.

Preserve Firebase authentication behavior.

Do not add libraries or unrelated features.

Implement the minimal UI change.

Verify XML resources and report
build or device-testing results.
```

A UI task should not automatically become an authentication architecture refactor.

## 56. GroupTrack Architecture Decision

Should GroupTrack be converted into a multi-module Android project? Evaluate the number of features, existing architecture, build complexity, team size, reusability needs, and MVP deadline.

For a small, strictly scoped MVP, retain the single-module architecture unless a concrete technical requirement justifies restructuring.

**Architectural sophistication is not the same as project quality.**

---

# Part XIX — Practical Laravel Example

## 57. GoPasig — Fleet Management Codebase

GoPasig functionality includes routes, route variants, bus stops, driver assignments, scheduling, trips, GPS tracking, ETA computation, and administrative reporting.

```text
Laravel Routes
      |
      v
Controllers
      |
      v
Application Services
      |
      v
Eloquent Models
      |
      v
Database
```

Confirm actual relationships in the codebase.

## 58. Scenario — Schedule Validation Change

For an approved validation change, consider whether the rule affects driver assignment, bus availability, route schedules, transactions, database constraints, and scheduling tests.

```text
Read AGENTS.md and the scheduling architecture docs.

Investigate the approved schedule
validation change.

Inspect ScheduleController and
directly related services and models.

Identify affected business rules.

Preserve existing route and
dispatch behavior.

Do not refactor unrelated modules.

Implement only the approved correction.

Run focused scheduling tests and
report remaining verification gaps.
```

## 59. Laravel Validation Commands

```powershell
php artisan test
php artisan test tests/Feature/ScheduleTest.php
php artisan route:list
```

The test path is illustrative; replace it with a real file. Run commands in the correct Laravel directory and a suitable test environment. Do not run state-changing database commands against production or shared data without authorization.

---

# Part XX — Practical Next.js and Laravel Example

## 60. E-commerce — Frontend and Backend Integration

```text
Next.js Frontend
        |
        v
Laravel REST API
        |
        v
PostgreSQL Database
```

Changing the product response may affect the Laravel controller, API resource, JSON response, Next.js API client, TypeScript interface, and product component.

## 61. Contract-First Development

Define the expected interface before implementing a cross-system change.

```json
{
  "id": 15,
  "name": "Keyboard",
  "price": 1250.00,
  "available": true
}
```

This is illustrative. Decide which fields are required, their types, nullability, error format, pagination behavior, and authentication requirements.

**Frontend and backend should implement the same agreed contract rather than making independent assumptions.**

## 62. Recommended Integration Workflow

```text
Define Contract
       |
       v
Review Existing Consumers
       |
       v
Implement Backend Change
       |
       v
Verify API Response
       |
       v
Update Frontend Types
       |
       v
Update Components
       |
       v
Run Integration Tests
       |
       v
Review Final Diff
```

```text
Investigate the product listing API integration.

Read applicable project instructions.

Inspect the Laravel API response
and Next.js consumer.

Identify the current contract
and all affected dependencies.

Do not change field names without
documenting affected consumers.

Implement only the approved correction.

Run available backend and frontend
verification commands.

Report the results and limitations.
```

---

# Part XXI — Common Mistakes

## 63. Mistake: Reading Everything

Loading the entire repository unnecessarily wastes context. Use targeted searches and progressive disclosure.

## 64. Mistake: Refactoring Unrelated Code

A small task should not trigger widespread cleanup. Keep modifications within the approved scope.

## 65. Mistake: Ignoring Existing Changes

An agent may overwrite work already in the working tree. Inspect `git status` and relevant diffs before editing.

## 66. Mistake: Breaking API Contracts

A backend field may change without updating consumers. Perform change impact analysis and contract verification.

## 67. Mistake: Overengineering

New layers, modules, and dependencies may be unnecessary. Follow existing architecture and justify structural changes.

## 68. Mistake: Treating Tests as Optional

Visual inspection alone does not verify behavior. Run appropriate tests and document verification limitations.

## 69. Mistake: Outdated Documentation

Docs may describe behavior that no longer exists. Synchronize documentation with meaningful implementation changes.

## 70. Mistake: Assuming Parallel Work Is Safe

Agents may modify shared files or services without coordination. Assign ownership and use isolation when justified.

---

# Part XXII — Practical Exercises

## 71. Exercise A — Repository Reconnaissance

Identify the repository root, read applicable project instructions, list top-level directories, identify the technology stack and application entry points, locate tests, and record major modules.

**Deliverable:** A concise repository overview.

## 72. Exercise B — Architecture Mapping

Create a map from actual source code.

```markdown
# Architecture Map

## System Overview
## Entry Points
## Major Modules
## Dependency Relationships
## Data Flow
## External Integrations
## Evidence
## Unknowns
```

Do not invent modules or dependencies unsupported by source evidence.

## 73. Exercise C — Dependency Analysis

Trace one feature across its actual files, for example:

```text
Frontend Page
    |
    v
API Client
    |
    v
Backend Route
    |
    v
Controller
    |
    v
Service
    |
    v
Database
```

## 74. Exercise D — Change Impact Matrix

Evaluate direct and indirect dependencies.

| Component | Dependency | Potential Impact | Verification |
|---|---|---|---|
| Component A | Component B | Describe risk | Test |
| Component B | Database | Describe risk | Test |
| Component C | API | Describe risk | Test |

## 75. Exercise E — Safe Refactoring Plan

Include the current problem, existing behavior, proposed structural change, affected files, dependencies, tests, rollback considerations, and completion criteria.

## 76. Exercise F — Monorepo vs Multi-Repository

For a Next.js frontend, Laravel backend, and shared API documentation, compare repository and CI benefits, contract coordination, and fit to project requirements.

## 77. Exercise G — Git Review

```powershell
git status
git diff --name-only
git diff --check
```

Determine which files changed, whether they match approved scope, whether unrelated edits exist, whether whitespace errors appear, and which tests should run.

## 78. Exercise H — Documentation Drift Audit

Select a module, read its documentation, inspect implementation, identify discrepancies, and propose corrections. Deliver a short consistency report.

## 79. Exercise I — GroupTrack Architecture Review

Inspect the single-module structure, identify UI and application responsibilities, locate Firebase integration, check documentation consistency, and identify unnecessary abstractions. Do not introduce modules.

## 80. Exercise J — GoPasig Change Impact Review

For a schedule-related change, identify controllers, services, models, route dependencies, relevant tests, and estimate the blast radius.

## 81. Exercise K — E-commerce API Contract Review

Inspect one Laravel endpoint and its Next.js consumer; compare field names and types, identify error handling, and recommend contract tests.

---

# Part XXIII — Knowledge Review

## 82. Knowledge Check

1. What is a codebase?
2. What makes a codebase complex?
3. Why is repository reconnaissance important?
4. What is architecture mapping?
5. How does an architecture map differ from a folder tree?
6. What is a module?
7. What are cohesion and coupling?
8. Why do module boundaries matter?
9. What is dependency analysis?
10. What are direct and indirect dependencies?
11. What is change impact analysis?
12. What is blast radius?
13. Why are public contracts important?
14. What is a multi-module project?
15. What are monorepos and multi-repository architectures?
16. What are their tradeoffs?
17. What is refactoring, and how does it differ from rewriting?
18. What is a characterization test?
19. Why document behavior before refactoring?
20. Why inspect `git status` before editing?
21. What does `git diff --check` do?
22. What is a Git worktree, and does it resolve conflicts?
23. What is a regression?
24. What is `git bisect`?
25. What are documentation drift and architectural drift?
26. How can code review prevent architectural drift?
27. Why should test scope reflect change risk?
28. Why are API contract tests important?
29. Why should Codex avoid unnecessary abstractions?
30. How would you investigate a GroupTrack UI change?
31. How would you analyze a GoPasig scheduling change?
32. How would you coordinate a Next.js/Laravel API change?
33. What makes a codebase modification safe and maintainable?

---

## 83. Key Terminologies

| Term | Definition |
|---|---|
| Codebase | Application code and supporting artifacts |
| Repository | Version-controlled collection of files |
| Repository Reconnaissance | Initial codebase inspection |
| Architecture Mapping | System components and relationships |
| Module | Cohesive unit of functionality |
| Cohesion | Relatedness of responsibilities within a module |
| Coupling | Dependency between components |
| Dependency Analysis | Examination of component relationships |
| Direct Dependency | Immediate reliance on another component |
| Indirect Dependency | Reliance through intermediate components |
| Change Impact Analysis | Identification of affected components |
| Blast Radius | Scope of possible effects from a change |
| Multi-Module Project | Project organized into multiple modules |
| Monorepo | Single repository containing multiple projects |
| Multi-Repository | System distributed across separate repositories |
| Refactoring | Structural improvement preserving intended behavior |
| Rewriting | Replacing substantial implementation |
| Characterization Test | Test recording existing observable behavior |
| Regression | Previously working behavior broken by a change |
| Git Worktree | Additional working directory attached to a repository |
| Git Bisect | Binary search for a regression-introducing commit |
| API Contract | Agreed interface between systems |
| Contract Test | Test validating an interface |
| Documentation Drift | Docs becoming inconsistent with code |
| Architectural Drift | Implementation deviating from architectural rules |
| Regression Testing | Verification against unintended behavior changes |
| Risk-Based Testing | Test effort determined by change impact |
| Integration Verification | Checking combined component behavior |

---

## 84. Key Takeaways

1. Large codebases require systematic exploration.
2. Repository size alone does not determine complexity.
3. Architecture maps reveal relationships that folder trees cannot.
4. Module boundaries help organize responsibilities.
5. High cohesion and controlled coupling improve maintainability.
6. Dependencies may exist through code, APIs, databases, and configuration.
7. Change impact analysis should precede consequential modifications.
8. Consider direct and indirect consumers.
9. Blast radius influences testing and review requirements.
10. Multi-module architectures bring benefits and overhead.
11. Monorepos and multi-repositories have different coordination tradeoffs.
12. Refactoring should preserve intended behavior.
13. Small, testable changes are easier to maintain.
14. Inspect Git status before modifying a repository.
15. Git diffs help verify change scope.
16. Worktrees can isolate independent tasks but do not eliminate integration conflicts.
17. Git bisect can help identify regression-introducing commits.
18. Documentation must reflect meaningful implementation changes.
19. Testing should be proportional to change risk.
20. Detect architectural drift early.
21. Unnecessary abstractions increase maintenance costs.
22. Follow existing architecture unless a change is justified.
23. Agent-generated changes require independent verification.
24. Good codebase management prioritizes correctness, maintainability, and controlled change.

---

## 85. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
3. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
4. [Codex Security](https://developers.openai.com/codex/security/)
5. [Modernizing Your Codebase with Codex](https://developers.openai.com/cookbook/examples/codex/code_modernization)
6. [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
7. [Custom Code Review Rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)

### Git

8. [Git Documentation](https://git-scm.com/docs)
9. [Git Worktree](https://git-scm.com/docs/git-worktree)
10. [Git Diff](https://git-scm.com/docs/git-diff)
11. [Git Bisect](https://git-scm.com/docs/git-bisect)
12. [Git Merge](https://git-scm.com/docs/git-merge)

### Framework Documentation

13. [Android Developers](https://developer.android.com/)
14. [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)
15. [Laravel Documentation](https://laravel.com/docs)
16. [Next.js Documentation](https://nextjs.org/docs)
17. [TypeScript Documentation](https://www.typescriptlang.org/docs/)
18. [PostgreSQL Documentation](https://www.postgresql.org/docs/)
19. [PowerShell Documentation](https://learn.microsoft.com/powershell/)

> **Documentation Note:** Repository architecture, framework versions, and Codex capabilities vary. Inspect the actual codebase and installed tooling before applying architecture changes or executing commands.

---

## 86. Lesson Completion Checklist

- [ ] Understand large codebase challenges.
- [ ] Understand repository reconnaissance.
- [ ] Navigate repositories systematically.
- [ ] Identify application entry points.
- [ ] Create architecture maps.
- [ ] Understand module boundaries, cohesion, and coupling.
- [ ] Analyze direct and indirect dependencies.
- [ ] Understand change impact analysis and blast radius.
- [ ] Understand multi-module projects.
- [ ] Differentiate monorepos and multi-repositories.
- [ ] Understand safe refactoring and characterization tests.
- [ ] Understand Git status, diffs, worktrees, and bisect.
- [ ] Identify documentation drift and ownership.
- [ ] Design risk-based testing strategies.
- [ ] Recognize architectural drift.
- [ ] Prevent unnecessary abstractions.
- [ ] Apply codebase management to Android, Laravel, and Next.js.
- [ ] Complete practical exercises.
- [ ] Answer knowledge-check questions.

---

## 87. Phase 5 Completion Summary

### Phase 5 — Advanced Codex

The following lessons have been documented:

- [x] [[01 - Introduction to Model Context Protocol (MCP)]]
- [x] [[02 - MCP Servers and External Tool Integrations]]
- [x] [[03 - Agent Skills and Reusable Workflows]]
- [x] [[04 - Multi-Agent Collaboration and Task Delegation]]
- [x] [[05 - Codex Automation and Non-Interactive Execution]]
- [x] [[06 - Context and Token Efficiency]]
- [x] [[07 - Advanced Codebase Management]]

### Knowledge Acquired

You now have theoretical foundations for:

1. Extending Codex through MCP.
2. Integrating external tools and services.
3. Creating reusable Agent Skills.
4. Delegating tasks to subagents.
5. Coordinating multi-agent workflows.
6. Automating Codex execution and producing structured outputs.
7. Managing context efficiently.
8. Navigating complex repositories and analyzing dependencies.
9. Planning safe refactors and preventing architectural drift.
10. Verifying code changes systematically.

Completing the documentation establishes theoretical preparation. Practical proficiency still requires hands-on application, testing, and experience with real repositories.

---

# 88. Next Phase — Real-World Application

## Phase 6 — Real-World Application

The next phase applies the concepts learned throughout the roadmap to an actual software development project.

### Planned Lessons

1. **6.1 — Project Selection and Scope Definition**
2. **6.2 — Project Setup and Context Preparation**
3. **6.3 — Feature Planning with Codex**
4. **6.4 — Guided Feature Implementation**
5. **6.5 — Debugging, Testing, and Quality Assurance**
6. **6.6 — Code Review and Git Workflow**
7. **6.7 — Final Project Evaluation**

Choose a project with a clear objective, defined scope, existing or planned architecture, testable functionality, and manageable complexity.

```text
Requirements
     |
     v
Context Engineering
     |
     v
Codebase Exploration
     |
     v
Implementation Planning
     |
     v
Agentic Development
     |
     v
Testing and Verification
     |
     v
Git Review
     |
     v
Final Evaluation
```

---

*Phase 5 — Advanced Codex | Lesson 5.7*  
*Learning Principle: Understand the system before changing it, control the impact of modifications, and verify that the architecture and behavior remain consistent.*
