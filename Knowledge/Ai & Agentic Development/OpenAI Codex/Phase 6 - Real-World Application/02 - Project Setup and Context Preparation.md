# 02 — Project Setup and Context Preparation

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 6 — Real-World Application
> **Lesson:** 6.2 — Project Setup and Context Preparation
> **Difficulty:** Intermediate to Advanced
> **Learning Mode:** Theory First
> **Prerequisite:** [[01 - Project Selection and Scope Definition]]
> **Next Lesson:** [[03 - Feature Planning with Codex]]
> **Status:** Theory Documented

---

# 1. Introduction

After selecting a project and defining its scope, prepare its development environment before generating implementation. Preparation includes repository organization, tools, version control, documentation, architecture, coding standards, dependencies, environment configuration, security, tests, and baseline verification.

Without preparation, Codex may misunderstand the repository, create files in the wrong place, add unnecessary dependencies, duplicate functionality, violate architecture, change unrelated files, expose secrets, or overstate verification.

## Core Principle

**A Codex-ready project has clear instructions, reliable context, controlled dependencies, and verifiable development workflows.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Explain setup prerequisites and new versus existing project setup.
2. Inspect and initialize Git repositories safely.
3. Follow framework conventions and avoid speculative structure.
4. Prepare a reproducible development environment.
5. Understand dependency manifests and lockfiles.
6. Organize AGENTS.md and project documentation.
7. Document architecture and coding standards.
8. Manage environment variables and secrets safely.
9. Establish testing foundations and record a baseline.
10. Define safe Codex operating boundaries.
11. Prepare Android, Laravel, and Next.js projects.
12. Evaluate project readiness without overengineering.

---

# Part II — Understanding Project Preparation

## 3. What Is Project Setup?

Project setup prepares the technical environment and repository required to develop, test, and maintain an application.

~~~text
Approved Scope → Repository → Environment → Structure
              → Dependencies → Documentation and AGENTS.md
              → Tests → Baseline Verification → Feature Planning
~~~

Setup should support the approved scope, not introduce unrelated architecture or tooling.

---

## 4. Project Setup vs Feature Implementation

| Project Setup | Feature Implementation |
|---|---|
| Establishes development foundations | Delivers user functionality |
| Configures tools | Implements business rules |
| Organizes project structure | Creates feature components |
| Establishes coding standards | Applies those standards |
| Prepares tests | Writes feature tests |
| Defines agent instructions | Executes scoped development tasks |

For example, configuring a Laravel development database is setup; implementing product inventory validation is feature work.

---

# Part III — Setup Prerequisites

## 5. Required Information Before Setup

| Category | Information |
|---|---|
| Project identity | Name and purpose |
| Scope | Approved MVP requirements |
| Platform | Web, mobile, desktop, or hybrid |
| Technology | Languages and frameworks |
| Repository | Existing or new |
| Database | Required storage |
| Integrations | External APIs or services |
| Development tools | IDEs and build tools |
| Testing | Verification approach |
| Constraints | Technical and organizational rules |

Example:

~~~markdown
## Project Information

Project Name: Student Task Manager
Platform: Android
Language: Java
UI Technology: XML
Backend: Firebase
Repository: Git
Development Tool: Android Studio

Constraints:
- Follow the approved MVP.
- Do not introduce unnecessary libraries.
- Preserve existing conventions.
~~~

---

## 6. New Project vs Existing Project

**New project:** select the approved stack, initialize the framework and Git, create required directories, configure dependencies, document conventions, and verify the initial build.

**Existing project:** inspect repository and documentation, identify stack and architecture, inspect Git status, verify dependencies, run baseline checks, and add only missing setup components.

**Do not recreate or restructure an existing project before understanding what is present.**

---

# Part IV — Repository Initialization

## 7. What Is a Repository?

A repository is a version-controlled collection of project files. Git provides history, branches, collaboration, comparison, recovery, and reviewable development records. A repository can contain one or several related applications.

---

## 8. Initializing Git

~~~powershell
git --version
Set-Location "C:\path\to\project"
git init
git status
git rev-parse --show-toplevel
~~~

Only initialize Git if the directory is not already in the intended repository. Git initialization inside an existing project can create an unintended nested repository.

---

## 9. Existing Repository Inspection

~~~powershell
git status
git branch --show-current
git log --oneline -5
git remote -v
~~~

Check the branch, uncommitted work, history, remote, and whether the repository is initialized. Agents must recognize and preserve pre-existing work.

---

# Part V — Repository Structure

## 10. What Is Project Structure?

Project structure organizes source, configuration, tests, and documentation so developers can locate entry points, features, shared functionality, configuration, and verification.

~~~text
project/
├── src/
├── tests/
├── docs/
├── scripts/
├── AGENTS.md
├── README.md
└── .gitignore
~~~

This is conceptual; framework conventions take priority.

---

## 11. Framework Conventions

Android commonly uses app/src/main, app/src/test, and app/src/androidTest. Laravel commonly includes app, bootstrap, config, database, public, resources, routes, storage, and tests. Next.js commonly uses app, components, lib, public, and package.json; some projects use src/.

**Follow the actual framework and repository conventions instead of imposing a generic structure.**

---

## 12. Avoiding Premature Folder Creation

Do not create speculative folders for unapproved analytics, payments, recommendations, plugins, or microservices. Add directories when required by the framework, approved features, existing boundaries, or essential docs and tests.

**Structure should reflect real responsibilities, not speculative features.**

---

# Part VI — Git Configuration and Workflow

## 13. Git Branching

Branches allow isolated work on coherent objectives.

~~~text
main
 ├── feature/sign-in-ui
 ├── fix/task-validation
 └── docs/architecture
~~~

~~~powershell
git switch -c feature/sign-in-ui
git status
git diff
~~~

---

## 14. Git Commit Practices

Use focused, understandable, reviewable commits. Examples:

~~~text
docs: add project architecture overview
chore: configure initial testing environment
feat: implement sign-in screen
fix: correct schedule validation
~~~

Prefixes are conventions unless required by the project. A commit should represent a coherent objective.

---

## 15. Git Ignore Rules

~~~gitignore
.env
.env.local
node_modules/
vendor/
.gradle/
build/
.idea/
~~~

Retain framework-generated ignore files where appropriate. Ignore rules vary by project; some IDE files may be intentionally tracked. Adding a secret to .gitignore does not remove an already tracked file or erase Git history.

---

# Part VII — Development Environment Preparation

## 16. What Is a Development Environment?

It includes the operating system, language runtime, framework, IDE, package manager, database, emulator, environment variables, and local services used to build and test software.

~~~text
Windows
  ├── VS Code
  ├── Git
  ├── Node.js
  ├── PHP and Composer
  └── PostgreSQL
~~~

---

## 17. Environment Reproducibility

A reproducible environment lets another developer create a sufficiently equivalent setup. Document runtimes, track manifests and appropriate lockfiles, record setup commands, provide safe configuration examples, document external services, and verify from a clean environment when practical.

Without reproducibility, builds and tests can differ across computers.

---

## 18. Tool Version Verification

~~~powershell
git --version
node --version
npm --version
php --version
composer --version
java -version
~~~

Take version requirements from project and framework compatibility rules; newest is not always compatible.

---

# Part VIII — Dependency Management

## 19. What Is a Dependency?

A dependency is a package, library, framework, or service an application requires, such as AndroidX, Firebase, Laravel packages, React, Next.js, or database drivers.

---

## 20. Dependency Manifests

| Ecosystem | Common Manifest |
|---|---|
| Android / Gradle | build.gradle.kts or build.gradle |
| JavaScript | package.json |
| PHP / Composer | composer.json |

Examples of lockfiles include package-lock.json and composer.lock. Manifests describe requirements; lockfiles help reproduce resolved versions.

---

## 21. Installing Existing Dependencies

~~~powershell
npm ci
composer install
~~~

Gradle resolves declared dependencies during supported synchronization or build operations. Use the package manager and lockfile already established by the repository. Do not switch package managers without justification.

---

## 22. Dependency Evaluation

Before adding a package, ask whether an approved requirement needs it, whether existing framework functionality is enough, whether it is maintained and compatible, whether security or licensing is acceptable, and whether its complexity is justified.

**Every new dependency creates a maintenance responsibility.**

---

# Part IX — Context Engineering

## 23. What Is Context Engineering?

Context engineering supplies the information an agent needs: project goals, approved requirements, architecture, coding standards, existing implementation, feature specs, tests, and constraints.

~~~text
Requirements + AGENTS.md + Architecture + Feature Specs
                  + Relevant Source → Codex → Scoped Work
~~~

---

## 24. Context Quality vs Context Quantity

More information is not always better. Excessive context consumes tokens, includes irrelevant details, and can create conflicts.

Use progressive disclosure:

1. Read project instructions.
2. Identify the relevant feature.
3. Read feature-specific documentation.
4. Inspect relevant source.
5. Expand only when necessary.

---

# Part X — AGENTS.md

## 25. What Is AGENTS.md?

AGENTS.md provides durable guidance for coding agents: project purpose, architecture, conventions, test commands, scope, and workflow expectations. It should be actionable and should not duplicate the entire docs directory.

---

## 26. Recommended AGENTS.md Structure

~~~markdown
# Agent Instructions

## Project Overview
## Technology Stack
## Architecture
## Coding Standards
## Scope Rules
## Development Workflow
## Testing
## Documentation
## Security
~~~

---

## 27. Example AGENTS.md

~~~markdown
# AGENTS.md

## Project
Student project management application.

## Technology
- Android Studio
- Java
- XML layouts
- Firebase

## Scope
Implement only approved MVP features.

## Architecture
Follow existing project structure and responsibilities.

## Dependencies
Do not add libraries unless required and justified.

## Workflow
1. Read relevant docs.
2. Inspect existing code.
3. Plan a minimal change.
4. Modify only necessary files.
5. Run relevant verification.
6. Report results and limitations.

## Security
Never expose credentials or private keys.
~~~

This is illustrative; real instructions must match the project.

---

# Part XI — Documentation Organization

## 28. Why Documentation Matters

Documentation supports onboarding, agent orientation, architectural consistency, requirements traceability, testing, and maintenance.

---

## 29. Suggested Documentation Structure

~~~text
docs/
├── overview.md
├── architecture.md
├── requirements.md
├── coding-standards.md
├── workflow.md
└── testing.md
~~~

These names are suggestions. Small projects may need fewer documents. Organize around information developers need.

---

## 30. README.md vs AGENTS.md vs docs/

| Source | Main Purpose |
|---|---|
| README.md | Human-oriented overview and setup |
| AGENTS.md | Durable agent instructions |
| docs/ | Detailed project knowledge |
| Source code | Implemented behavior |
| Tests | Executable verification evidence |

Docs describe intended behavior; source shows implementation; tests provide evidence. Investigate disagreements rather than assuming they match.

---

# Part XII — Architecture Documentation

## 31. What Is an Architecture Document?

It describes major components, responsibilities, data flow, and relationships.

~~~text
User Interface → Application Logic → Data Access → Database
~~~

Common topics include overview, modules, entry points, dependencies, external integrations, and security boundaries.

---

## 32. Architecture Decision Records

An Architecture Decision Record (ADR) documents a significant technical choice.

~~~markdown
# ADR-001 — Use Java for Android

## Status
Accepted

## Context
The project requires native Android development.

## Decision
Use Java and XML layouts.

## Consequences
Follow the existing Java/XML structure.
~~~

ADRs suit choices such as database, framework, module boundaries, or deployment architecture—not every minor detail.

---

# Part XIII — Coding Standards

## 33. What Are Coding Standards?

Conventions for naming, formatting, file organization, error handling, validation, documentation, testing, and dependencies.

~~~markdown
## Coding Standards
- Use descriptive names.
- Follow existing conventions.
- Keep functions focused.
- Avoid duplicate business logic.
- Handle errors explicitly.
- Avoid unjustified abstractions.
~~~

---

## 34. Architecture-Specific Standards

- **Android:** follow Java naming, organize XML resources, preserve lifecycle behavior, reuse established patterns.
- **Laravel:** follow framework conventions, validate untrusted input, authorize access, preserve service boundaries, use parameterized operations.
- **Next.js:** follow TypeScript conventions, respect server/client boundaries, use consistent API access, preserve routes, avoid unnecessary client dependencies.

Standards should reinforce actual architecture, not impose unrelated preferences.

---

# Part XIV — Environment Variables

## 35. What Are Environment Variables?

They store configuration that varies by environment, such as database host, API endpoint, application environment, and service credentials.

~~~dotenv
APP_ENV=local
DB_HOST=127.0.0.1
DB_DATABASE=example_database
~~~

Values above are illustrative.

---

## 36. Environment Configuration Files

Common files include .env, .env.example, and .env.local.

Keep real secrets out of Git, provide safe examples, document required variable names, validate required configuration, use environment-specific credentials, and restrict production access.

---

## 37. Important Next.js Security Rule

Next.js variables prefixed with NEXT_PUBLIC_ can be exposed to browser-accessible code when referenced appropriately.

~~~dotenv
NEXT_PUBLIC_API_URL=http://localhost:8000/api
~~~

A public API URL may be appropriate; secret API keys are not.

**Never put secrets in browser-exposed environment variables.**

---

# Part XV — Secrets Management

## 38. What Is a Secret?

Sensitive authentication or authorization information includes API keys, database passwords, service-account private keys, tokens, and signing credentials.

Do not commit secrets or paste them into unnecessary prompts. Use secure storage, restrict access, rotate exposed credentials, and separate development from production credentials.

---

## 39. Secret Exposure Risks

Deleting a secret from the latest file does not erase it from earlier Git commits.

If exposed, revoke or rotate it, assess access, follow provider incident guidance, remove it from appropriate locations, and review history and sharing exposure.

**Credential rotation matters more than merely deleting the visible copy.**

---

# Part XVI — Testing Foundations

## 40. Why Establish Testing Early?

Early tests verify setup, detect configuration problems, prevent regressions, define expected behavior, and help evaluate agent changes.

---

## 41. Testing Categories

| Category | Purpose |
|---|---|
| Unit | Verify isolated functions or classes |
| Integration | Verify component interactions |
| UI | Verify interface behavior |
| Contract | Verify system interfaces |
| End-to-end | Verify complete workflows |
| Static analysis | Identify certain code issues |
| Build verification | Confirm compilation or bundling |

---

## 42. Android Testing Foundations

Typical locations are app/src/test and app/src/androidTest. Local unit tests usually run on the JVM; instrumented tests run on a device or emulator.

~~~powershell
.\gradlew.bat test
.\gradlew.bat assembleDebug
~~~

Actual tasks depend on the Gradle project.

---

## 43. Laravel Testing Foundations

~~~powershell
php artisan test
php artisan test --filter=AuthenticationTest
~~~

Match test names and database configuration to the project. Use an isolated test environment where appropriate.

---

## 44. Next.js Testing Foundations

Projects may use ESLint, TypeScript, unit/component tests, and end-to-end tools.

~~~powershell
npm run lint
npx tsc --noEmit
npm run build
~~~

The lint script must exist. A successful build does not replace functional tests.

---

# Part XVII — Baseline Verification

## 45. What Is a Baseline?

A baseline records the project before implementation: Git status, branch, dependencies, build/test results, warnings, and known failures. It distinguishes existing problems from regressions.

---

## 46. Baseline Verification Workflow

~~~text
Inspect Git → Confirm Dependencies → Verify Environment
           → Run Build and Tests → Record Failures → Document Baseline
~~~

Do not attribute a failure to a new change without checking whether it existed first.

---

## 47. Baseline Report Template

~~~markdown
# Baseline Verification Report

## Repository
Branch:
Commit:
Working Tree Status:

## Environment
Operating System:
Runtime Versions:
Framework Versions:

## Dependencies
Installation Status:

## Build
Command:
Result:

## Tests
Command:
Result:

## Known Issues
List pre-existing failures.

## Readiness
Ready / Conditionally Ready / Blocked
~~~

---

# Part XVIII — Safe Codex Operating Boundaries

## 48. Why Operating Boundaries Matter

Clear boundaries reduce unintended changes. Instruct Codex to inspect before editing, preserve existing changes, modify only relevant files, protect private data, avoid unjustified dependencies and architecture changes, run checks, and report failures accurately.

---

## 49. Codex Sandbox and Approvals

Sandbox and approval settings govern accessible resources and operations requiring permission.

~~~text
Request → Instructions → Sandbox / Approval Boundaries
       → Tool Execution → Verification
~~~

AGENTS.md does not replace operating-system permissions, sandbox restrictions, or other security controls.

---

## 50. Planning-Only Codex Prompt

~~~text
Read AGENTS.md and relevant documentation.
Inspect repository structure, dependencies,
configuration, and existing tests.
Do not modify files.

Identify the stack, entry points, modules,
architecture, missing setup, verification commands,
security concerns, and potential overengineering.

Return a concise readiness report.
Separate confirmed findings from assumptions.
~~~

---

## 51. Setup Implementation Prompt

~~~text
Prepare the project for development using the approved scope.

Read AGENTS.md and existing documentation.
Inspect Git status before editing.
Do not recreate existing setup.
Implement only missing setup requirements.
Preserve architecture and avoid unrelated dependencies.
Do not expose secrets.
Run relevant baseline verification.

Report files changed, commands, results, and remaining issues.
~~~

This prompt is for a future hands-on session, not a requirement to modify a project in this theory lesson.

---

# Part XIX — Practical Android Example

## 52. GroupTrack — Project Setup

Technology: Android Studio, Java, XML, Firebase, Gradle, and Git.

~~~text
GroupTrack/
├── app/src/main/java/
├── app/src/main/res/
├── app/src/main/AndroidManifest.xml
├── app/src/test/
├── app/src/androidTest/
├── docs/
├── AGENTS.md
├── README.md
├── settings.gradle.kts
└── gradlew.bat
~~~

Verify the real project structure rather than assuming it matches the illustration.

---

## 53. GroupTrack Setup Priorities

1. Inspect existing architecture; do not create modules without need.
2. Verify docs cover MVP, architecture, data, standards, workflow, and testing.
3. Locate layouts, images, colors, and strings.
4. Check Firebase configuration without exposing private credentials.
5. Confirm a baseline build before feature work.

---

## 54. Android Resource Organization

~~~text
app/src/main/res/
├── drawable/
├── layout/
├── mipmap/
└── values/
~~~

drawable contains drawable resources, layout contains XML layouts, mipmap contains launcher icons, and values contains strings, colors, dimensions, and styles. Follow Android conventions.

---

## 55. GroupTrack Codex Setup Prompt

~~~text
Inspect the existing GroupTrack Android project.
Read AGENTS.md and docs.
Do not modify files.

Identify modules, Java source and XML resource structure,
Firebase integration, Gradle configuration, tests,
and build verification commands.

Preserve the approved MVP scope.
Do not recommend unnecessary libraries or architecture changes.
Return a project readiness report.
~~~

---

# Part XX — Practical Laravel Example

## 56. GoPasig — Laravel Setup

Technology may include Laravel, PHP, Composer, a relational database, Vite, and mapping integration.

~~~text
GoPasig/
├── app/
├── bootstrap/
├── config/
├── database/
├── docs/
├── public/
├── resources/
├── routes/
├── storage/
├── tests/
├── AGENTS.md
├── artisan
├── composer.json
├── package.json
└── .env.example
~~~

---

## 57. Laravel Environment Preparation

~~~powershell
php --version
composer --version
composer install
npm ci
php artisan about
~~~

Commands assume prerequisites and dependencies are available. Do not run migrations or destructive operations against shared or production databases without explicit authorization.

---

## 58. Laravel Configuration Considerations

Review database, application URL, authentication, session, cache, queues, external API credentials, and mapping configuration.

Configuration should vary by environment while application code remains portable.

---

## 59. GoPasig Context Preparation

Documentation may cover fleet, drivers, routes/stops, scheduling, dispatch, GPS, ETA, permissions, and reports.

Keep project-wide instructions concise and business rules in feature documentation.

~~~text
AGENTS.md → docs/
             ├── architecture.md
             ├── scheduling.md
             ├── route-management.md
             └── testing.md
~~~

This is illustrative, not a requirement to restructure an existing project.

---

# Part XXI — Practical Next.js and Laravel Example

## 60. E-commerce — Separated Frontend and Backend

~~~text
ecommerce/
├── frontend/
├── backend/
└── docs/
~~~

The repositories may be separate or combined; inspect the real setup.

---

## 61. Frontend Setup

Typical files include app/, public/, package.json, package-lock.json, tsconfig.json, and .env.local.

~~~powershell
npm ci
npx tsc --noEmit
npm run build
~~~

---

## 62. Backend Setup

Typical files include app/, config/, database/, routes/, tests/, composer.json, and .env.example.

~~~powershell
composer install
php artisan about
php artisan test
~~~

---

## 63. API Contract Preparation

Frontend and backend should agree on endpoint paths, methods, request and response fields, types, error formats, authentication, and authorization.

~~~json
{
  "id": 1,
  "name": "Sample Product",
  "price": 1200
}
~~~

This response is illustrative. Separate applications need coordinated contracts even if each builds alone.

---

## 64. Cross-Project Context Engineering

When working across applications, clarify which repository owns the change, which application consumes the interface, where the contract is documented, which tests verify integration, and whether cross-repository changes are authorized.

~~~text
Inspect frontend and backend structure.
Identify API integration patterns.
Do not modify files.

Report frontend API configuration, backend routes,
authentication, shared contracts, and integration tests.
Do not propose unrelated architecture changes.
~~~

---

# Part XXII — Setup Verification Matrix

## 65. Cross-Platform Verification

| Area | Android | Laravel | Next.js |
|---|---|---|---|
| Toolchain | Java, Android SDK | PHP, Composer | Node.js, npm |
| Manifest | Gradle files | composer.json | package.json |
| Build | Gradle build | Application bootstrap/build steps | Next.js build |
| Tests | JUnit / Android tests | Laravel tests | Project-configured tests |
| Environment | SDK and service config | .env | .env.local |
| Agent guidance | AGENTS.md | AGENTS.md | AGENTS.md |

Verification must match the actual framework and repository.

---

# Part XXIII — Common Mistakes

## 66. Mistake: Reinitializing an Existing Repository

**Problem:** An unintended nested Git repository.
**Improvement:** Inspect the repository root first.

## 67. Mistake: Creating Too Many Directories

**Problem:** Unnecessary structure.
**Improvement:** Add directories for real responsibilities.

## 68. Mistake: Ignoring Lockfiles

**Problem:** Different dependency resolutions.
**Improvement:** Follow the established package manager and lockfile.

## 69. Mistake: Oversized AGENTS.md

**Problem:** Instructions become hard to maintain.
**Improvement:** Keep rules concise and link detailed docs.

## 70. Mistake: Duplicating Documentation

**Problem:** Conflicting copies.
**Improvement:** Establish canonical sources.

## 71. Mistake: Exposing Credentials

**Problem:** Secrets enter source control or prompts.
**Improvement:** Use secure storage and safe examples.

## 72. Mistake: Skipping Baseline Tests

**Problem:** Existing failures are confused with regressions.
**Improvement:** Record initial build/test results.

## 73. Mistake: Overengineering Setup

**Problem:** Unnecessary tooling and architecture.
**Improvement:** Use the simplest setup satisfying real requirements.

---

# Part XXIV — Practical Exercises

> Exercises are optional and may be completed during the hands-on phase.

## 74. Exercise A — Repository Inspection

Identify the repository root, Git status, branch, remote, stack, source directories, and test directories. Deliver a repository inspection report.

## 75. Exercise B — Project Structure Review

Evaluate clear responsibilities, framework conventions, unnecessary folders, test organization, and documentation discoverability.

## 76. Exercise C — AGENTS.md Draft

Write concise overview, stack, architecture, scope, coding, testing, and security guidance without duplicating entire documentation files.

## 77. Exercise D — Documentation Plan

| Document | Purpose | Required? |
|---|---|---|
| README | Project overview | Yes |
| AGENTS.md | Agent instructions | If repository-level guidance is needed |
| Architecture | Component boundaries | Depends on complexity |
| Testing | Verification commands | Depends on project needs |

## 78. Exercise E — Dependency Audit

Identify required and unused dependencies, compatibility, lockfiles, security concerns, and unnecessary packages.

## 79. Exercise F — Environment Configuration

Identify required variables, separate public config from secrets, create safe examples, document missing configuration, and define verification.

## 80. Exercise G — Baseline Verification

Record Git status, runtime versions, dependency state, build/test results, and known issues.

## 81. Exercise H — Android Setup Review

Evaluate Gradle, Java structure, XML resources, Firebase, docs, build verification, and MVP scope.

## 82. Exercise I — Laravel Setup Review

Evaluate PHP/Composer, configuration, database connectivity, dependencies, tests, mapping config, and secret protection.

## 83. Exercise J — Next.js/Laravel Setup Review

Evaluate frontend build, backend tests, PostgreSQL, API URL, authentication, contracts, and integration checks.

## 84. Exercise K — Codex Readiness Prompt

Write a planning-only prompt that reads instructions, inspects structure and dependencies, identifies setup gaps, avoids edits and unnecessary recommendations, and returns a readiness report.

---

# Part XXV — Knowledge Review

## 85. Knowledge Check

1. What is project setup, and how does it differ from feature implementation?
2. What information and prerequisites are needed?
3. How does setup differ for new and existing projects?
4. What are repositories, branches, commits, and .gitignore?
5. Does .gitignore remove tracked secrets?
6. Why follow framework conventions and avoid speculative folders?
7. What is environment reproducibility?
8. What are dependency manifests and lockfiles?
9. Why evaluate dependencies?
10. What is context engineering and progressive disclosure?
11. What belongs in AGENTS.md?
12. How does it differ from README.md and docs/?
13. What are architecture docs and ADRs?
14. What are coding standards?
15. What are environment variables and secrets?
16. Why are browser-exposed variables unsuitable for secrets?
17. Why establish testing early?
18. What is baseline verification and why record pre-existing failures?
19. What are Codex operating boundaries?
20. How should agents preserve uncommitted work?
21. How do Android, Laravel, and Next.js setup checks differ?
22. Why do separate frontend/backend projects need API contracts?
23. What is setup overengineering?
24. What makes a project Codex-ready?

---

## 86. Key Terminologies

| Term | Definition |
|---|---|
| Project Setup | Preparation of the development foundation |
| Repository | Version-controlled project collection |
| Git | Distributed version control system |
| Branch | Independent line of development |
| Commit | Recorded repository change |
| .gitignore | Git ignore pattern configuration |
| Development Environment | Tools and configuration used for development |
| Reproducibility | Ability to recreate an equivalent environment |
| Dependency | Required library, package, or service |
| Dependency Manifest | File describing dependency requirements |
| Lockfile | File recording resolved dependency information |
| Context Engineering | Supplying relevant information to an AI agent |
| Progressive Disclosure | Loading details only when needed |
| AGENTS.md | Repository instructions for coding agents |
| README.md | Project overview and setup documentation |
| Architecture Documentation | Description of system structure and relationships |
| ADR | Architecture Decision Record |
| Coding Standards | Conventions governing source code |
| Environment Variable | Configuration value supplied through the environment |
| Secret | Sensitive authentication or authorization information |
| Baseline | Recorded initial project state |
| Baseline Verification | Initial build and test assessment |
| Static Analysis | Code inspection without normal application execution |
| Integration Test | Test of interacting components |
| API Contract | Agreed interface between systems |
| Sandbox | Execution restriction boundary |
| Approval Policy | Rules governing actions requiring permission |
| Scope Boundary | Limit on authorized project changes |
| Project Readiness | Preparation state for implementation planning |

---

# 87. Key Takeaways

1. Project setup establishes the foundation for development.
2. Inspect existing repositories before modifying them.
3. Git status helps protect pre-existing work.
4. Follow framework conventions and avoid speculative directories.
5. Reproducible environments and lockfiles improve consistency.
6. New dependencies require justification.
7. Provide relevant context and keep AGENTS.md concise.
8. Keep detailed documentation in canonical sources.
9. Coding standards should reflect the actual project.
10. Keep secrets out of Git and browser-exposed variables.
11. Establish tests and record baseline failures early.
12. Respect Codex scope and operating boundaries.
13. Android, Laravel, and Next.js require different setup checks.
14. Separate frontend/backend applications need explicit API contracts.
15. Avoid setup overengineering.
16. A project is Codex-ready when environment, context, constraints, and verification are sufficiently clear.

---

# 88. Official References

## OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Codex CLI](https://developers.openai.com/codex/cli/)
3. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
4. [Codex Security](https://developers.openai.com/codex/security/)
5. [Codex AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
6. [Codex Development Workflows](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)

## Git

7. [Git Documentation](https://git-scm.com/docs)
8. [Git Ignore](https://git-scm.com/docs/gitignore)
9. [Git Branch](https://git-scm.com/docs/git-branch)
10. [Git Status](https://git-scm.com/docs/git-status)

## Android

11. [Android Developers](https://developer.android.com/)
12. [Android Build Documentation](https://developer.android.com/build)
13. [Android Testing](https://developer.android.com/training/testing)
14. [Gradle Documentation](https://docs.gradle.org/current/userguide/userguide.html)

## Laravel

15. [Laravel Documentation](https://laravel.com/docs)
16. [Laravel Configuration](https://laravel.com/docs/configuration)
17. [Laravel Testing](https://laravel.com/docs/testing)
18. [Composer Documentation](https://getcomposer.org/doc/)

## Next.js

19. [Next.js Documentation](https://nextjs.org/docs)
20. [Next.js Environment Variables](https://nextjs.org/docs/app/guides/environment-variables)
21. [TypeScript Documentation](https://www.typescriptlang.org/docs/)
22. [npm CLI Documentation](https://docs.npmjs.com/cli/)

## Security

23. [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
24. [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)

> **Documentation Note:** Commands, supported options, and framework conventions can change. Verify against the installed toolchain and applicable official documentation before execution.

---

# 89. Lesson Completion Checklist

- [ ] Understand setup and feature implementation differences.
- [ ] Identify project prerequisites and inspect existing repositories.
- [ ] Understand Git, structure, conventions, and ignore rules.
- [ ] Prepare reproducible environments and dependencies.
- [ ] Apply context engineering and progressive disclosure.
- [ ] Organize AGENTS.md and documentation.
- [ ] Understand architecture docs, ADRs, and coding standards.
- [ ] Protect environment configuration and secrets.
- [ ] Establish tests and baseline verification.
- [ ] Define safe Codex boundaries.
- [ ] Evaluate Android, Laravel, and Next.js setup.
- [ ] Prepare API contracts.
- [ ] Avoid overengineering.
- [ ] Complete optional exercises and knowledge check.

---

# 90. Next Lesson

## [[03 - Feature Planning with Codex]]

The next lesson converts approved requirements into actionable tasks.

Topics include feature decomposition, requirement analysis, user stories, acceptance criteria, dependencies, implementation sequencing, planning prompts, file impact, tests, risk prioritization, and Android, Laravel, and Next.js examples.

~~~text
Lesson 6.1: Project Selection and Scope
        |
        v
Lesson 6.2: Project Setup and Context Preparation
        |
        v
Lesson 6.3: Feature Planning with Codex
        |
        v
Lesson 6.4: Guided Feature Implementation
        |
        v
Lesson 6.5: Debugging, Testing, and QA
        |
        v
Lesson 6.6: Code Review and Git Workflow
        |
        v
Lesson 6.7: Final Project Evaluation
~~~

---

*Phase 6 — Real-World Application | Lesson 6.2*
*Learning Principle: Prepare the environment, document architecture, establish clear agent instructions, and verify the baseline before implementing new functionality.*
