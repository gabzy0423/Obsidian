# 05 — Coding Standards and Architectural Constraints

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 2 — Context Engineering  
> **Lesson:** 2.5 — Coding Standards and Architectural Constraints  
> **Difficulty:** Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[04 - Project Documentation and Knowledge Organization]]

---

## 1. Introduction

AI coding agents such as OpenAI Codex can inspect, modify, and generate source code across different programming languages and frameworks.

However, technically correct code does not always mean architecturally appropriate code.

A solution may work while introducing unnecessary dependencies, duplicating business logic, violating established component boundaries, or creating maintainability problems.

For example, an Android application may already separate user interface responsibilities from Firebase data operations.

If an agent implements an entire feature directly inside an Activity, the application may still function, but the implementation could become difficult to test and maintain.

Similarly, a Laravel application may follow an established controller-service-model structure.

Placing complex business rules directly inside controllers could violate that architecture.

This is why **Coding Standards and Architectural Constraints** are important components of Context Engineering.

Coding standards define how source code should be written.

Architectural constraints define how software components should be organized, how responsibilities are distributed, and which design boundaries must be preserved.

Together, they help coding agents produce changes that are consistent with the existing project.

**Core Principle:** The goal is not simply to generate working code. The goal is to produce correct, maintainable code that respects the project's architecture and approved scope.

---

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define coding standards and architectural constraints.
2. Explain the difference between coding conventions and architectural rules.
3. Understand Separation of Concerns.
4. Recognize the Single Responsibility Principle.
5. Understand architectural boundaries and dependencies.
6. Explain MVC and layered architectures.
7. Define dependency policies.
8. Apply architectural constraints to Android Java projects.
9. Apply architectural constraints to Laravel applications.
10. Document coding standards for Codex.
11. Recognize architectural drift and overengineering.
12. Validate whether AI-generated code follows project conventions.

---

## 3. What Are Coding Standards?

**Coding Standards** are rules and conventions that guide how developers write, organize, and maintain source code.

They help establish consistency across a project.

### 3.1 Common Coding Standards

- Naming conventions
- Formatting
- File organization
- Error handling
- Input validation
- Documentation conventions
- Testing expectations
- Dependency usage

### 3.2 Example — Java Naming

```java
public class TaskManager {

    private String taskTitle;

    public void createTask() {
        // Task creation behavior
    }
}
```

Common Java conventions include:

| Element | Convention | Example |
|---|---|---|
| Class | PascalCase | `TaskManager` |
| Method | camelCase | `createTask()` |
| Variable | camelCase | `taskTitle` |
| Constant | UPPER_SNAKE_CASE | `MAX_TASKS` |
| Package | Lowercase | `com.example.grouptrack` |

### 3.3 Example — PHP Naming

```php
class TaskController extends Controller
{
    public function store(Request $request)
    {
        // Handle task creation
    }
}
```

Typical Laravel/PHP conventions include:

- PascalCase class names.
- camelCase method names.
- Descriptive controller and model names.
- Consistent namespaces.
- Framework-compatible file organization.

### 3.4 Why Standards Matter

Consistent code is easier to:

- Read
- Review
- Debug
- Maintain
- Extend
- Test

**Important:** Existing project conventions should normally take priority over arbitrary stylistic preferences.

---

## 4. What Are Architectural Constraints?

**Architectural Constraints** are rules that limit how a system can be designed or modified.

They define acceptable component relationships and implementation boundaries.

### 4.1 Examples

- Business logic belongs in established service or domain components.
- UI components should not directly manage database persistence.
- API controllers must preserve existing response contracts.
- Database schema changes require approval.
- New dependencies must be justified.
- Existing module boundaries must be respected.

### 4.2 Example

A Laravel project follows:

```text
Route
  |
  v
Controller
  |
  v
Service
  |
  v
Model / Database
```

An architectural constraint might state:

"Keep complex business logic in existing service classes rather than introducing it directly into controllers."

### 4.3 Coding Standard vs Architectural Constraint

| Aspect | Coding Standard | Architectural Constraint |
|---|---|---|
| Focus | Code consistency | System structure |
| Example | Use camelCase methods | Separate UI and data responsibilities |
| Scope | Usually code-level | Component or system-level |
| Purpose | Readability and consistency | Maintainability and design integrity |
| Violation | Inconsistent naming | Incorrect responsibility placement |

### 4.4 Key Principle

**Coding standards influence how code is written. Architectural constraints influence where code belongs and how components interact.**

---

## 5. Separation of Concerns

**Separation of Concerns (SoC)** is a software engineering principle that divides a system into components with distinct responsibilities.

Each component should focus on a clearly defined concern.

### 5.1 Example

Consider an Android task management application.

The application needs to:

1. Display a task creation form.
2. Validate user input.
3. Save task information.
4. Display success or error feedback.

These responsibilities should be organized according to the project's architecture.

### 5.2 Poor Separation

```text
TaskActivity
├── UI rendering
├── Input handling
├── Complex validation
├── Firebase queries
├── Business rules
└── Data transformations
```

This can make the Activity difficult to maintain.

### 5.3 Better Separation

```text
TaskActivity
├── UI rendering
└── User interaction

TaskRepository
└── Data operations

Task model
└── Task information
```

For more complex applications, additional components may be appropriate.

However, not every application needs multiple abstraction layers.

### 5.4 Benefits

- Easier testing
- Clearer responsibilities
- Reduced duplication
- Better maintainability
- More focused modifications

### 5.5 Important Consideration

Separation of Concerns does not mean creating a separate class for every line of code.

The amount of separation should match the complexity of the application.

---

## 6. Single Responsibility Principle

The **Single Responsibility Principle (SRP)** suggests that a software component should have one primary reason to change.

It encourages cohesive responsibilities.

### 6.1 Example

A class responsible for sending notifications should not also be responsible for calculating unrelated financial reports.

### 6.2 Poor Design

```java
public class TaskHelper {

    public void saveTask() {
        // Save task
    }

    public void sendNotification() {
        // Send notification
    }

    public void generateReport() {
        // Generate report
    }
}
```

This class combines several potentially unrelated responsibilities.

### 6.3 Improved Design

```text
TaskRepository
└── Save and retrieve tasks

NotificationService
└── Handle notifications

ReportService
└── Generate reports
```

These components should only be introduced if the application actually requires those responsibilities.

### 6.4 SRP vs Overengineering

SRP encourages meaningful responsibility boundaries.

It does not require excessive classes, interfaces, or layers for simple operations.

**Key Principle:** Separate responsibilities when doing so improves clarity, reuse, or testability.

---

## 7. Understanding Architectural Boundaries

An **Architectural Boundary** defines which responsibilities and dependencies belong to particular components.

### 7.1 Example — Full-Stack Application

```text
Next.js Frontend
       |
       | HTTP Requests
       v
Laravel Backend
       |
       | Database Operations
       v
PostgreSQL
```

### 7.2 Frontend Responsibilities

- User interface
- User interactions
- Client-side validation
- Presentation logic
- API communication

### 7.3 Backend Responsibilities

- Server-side validation
- Authentication and authorization
- Business rules
- Database operations
- API responses

### 7.4 Database Responsibilities

- Persistent data storage
- Relational constraints
- Data integrity

### 7.5 Example Constraint

"Do not implement authoritative server-side business rules exclusively in the frontend."

Client-side validation improves user experience, but server-side validation remains necessary for enforcing backend requirements.

---

## 8. Understanding MVC Architecture

**Model–View–Controller (MVC)** is an architectural pattern that separates an application's responsibilities.

### 8.1 Model

Represents application data and related domain behavior.

### 8.2 View

Displays information to users.

### 8.3 Controller

Handles incoming requests or user interactions and coordinates the appropriate response.

### 8.4 Conceptual Diagram

```text
User
  |
  v
Controller
  |
  +----> Model
  |
  v
View / Response
```

### 8.5 MVC in Laravel

Laravel commonly uses:

```text
app/
├── Http/
│   └── Controllers/
├── Models/
└── ...
```

Blade templates may serve as views in server-rendered Laravel applications.

For API applications, controllers commonly return JSON responses instead.

### 8.6 MVC and Service Layers

A service layer may be introduced to organize business logic when appropriate.

```text
Controller
    |
    v
Service
    |
    v
Model
```

A service layer is not a mandatory part of every Laravel application.

**Important:** Follow the architecture already established by the project instead of automatically introducing new layers.

---

## 9. Layered Architecture

A **Layered Architecture** organizes software responsibilities into logical layers.

### 9.1 Example

```text
Presentation Layer
        |
        v
Application / Business Layer
        |
        v
Data Access Layer
        |
        v
Database
```

### 9.2 Presentation Layer

Responsible for displaying information and handling user interaction.

### 9.3 Application or Business Layer

Coordinates use cases and applies business rules.

### 9.4 Data Access Layer

Manages communication with persistent storage or external data sources.

### 9.5 Advantages

- Clearer responsibilities
- Easier testing
- Better separation
- More predictable dependencies

### 9.6 Trade-Offs

Additional layers introduce more files and abstractions.

For small projects, unnecessary layering can reduce development efficiency.

Use only the layers justified by the application's requirements and existing architecture.

---

## 10. Dependency Policies

A **Dependency Policy** defines when and how external libraries or packages may be introduced into a project.

Dependencies can provide useful functionality, but they also introduce maintenance responsibilities.

### 10.1 Potential Costs

- Larger dependency graphs
- Version compatibility issues
- Security maintenance
- Build complexity
- Increased onboarding requirements

### 10.2 Example Policy

```markdown
## Dependency Policy

- Reuse existing dependencies when appropriate.
- Do not introduce packages for trivial functionality.
- Explain the purpose of proposed dependencies.
- Check compatibility with the current project.
- Request approval before adding significant dependencies.
```

### 10.3 Android Example

A developer wants to align components in an XML layout.

This usually does not justify introducing a new UI framework.

Existing Android layout capabilities may already satisfy the requirement.

### 10.4 Laravel Example

A developer needs basic request validation.

Laravel already provides validation facilities.

A third-party validation package should not be introduced without a concrete need.

### 10.5 Key Principle

**Use dependencies when they provide meaningful value, not merely because they are available.**

---

## 11. Architectural Constraints in Android Java

Android applications involve several component types, including Activities, layouts, data models, and data access components.

Google's Android architecture guidance emphasizes Separation of Concerns and clear responsibilities between UI and data layers.

### 11.1 Example Structure

```text
app/src/main/
├── java/com/example/grouptrack/
│   ├── ui/
│   │   └── TaskActivity.java
│   ├── data/
│   │   └── TaskRepository.java
│   └── model/
│       └── Task.java
└── res/
    └── layout/
        └── activity_task.xml
```

This is an illustrative structure, not a requirement to reorganize an existing project.

### 11.2 Activity Responsibility

An Activity can:

- Initialize the screen.
- Handle user interactions.
- Observe or receive UI state.
- Display feedback.
- Coordinate navigation.

### 11.3 Repository Responsibility

A repository may:

- Retrieve task data.
- Save task data.
- Encapsulate Firebase operations.
- Expose data operations to other components.

### 11.4 Model Responsibility

A model represents task information.

### 11.5 Example Java Model

```java
public class Task {

    private String title;
    private String description;

    public Task() {
    }

    public Task(String title, String description) {
        this.title = title;
        this.description = description;
    }

    public String getTitle() {
        return title;
    }

    public String getDescription() {
        return description;
    }
}
```

This is a simplified example and not a complete Firebase data model.

### 11.6 Android Architectural Rules

```markdown
## Android Architecture

- Follow the existing Java and XML structure.
- Keep UI responsibilities focused on presentation.
- Preserve established data-access patterns.
- Avoid unnecessary libraries and abstraction layers.
- Do not rewrite unrelated Activities.
- Keep changes within approved MVP requirements.
```

### 11.7 Important Consideration

Android's official architecture recommendations describe UI and data layers, with an optional domain layer.

A domain layer should be added only when the application's complexity or reuse requirements justify it.

---

## 12. Real-World Android Example — Login Screen

### Scenario

The existing Android application has a login screen.

The developer wants to improve component spacing.

### Poor Codex Prompt

"Improve the login system."

This could lead to unnecessary changes in authentication, navigation, or data handling.

### Better Prompt

**Task**

Improve the visual alignment and spacing of the existing login screen.

**Context**

The application uses Java and XML layouts.

**Constraints**

- Modify only relevant XML layout resources.
- Preserve existing view IDs.
- Do not change Java authentication logic.
- Do not modify Firebase configuration.
- Do not add dependencies.
- Preserve existing navigation.

**Acceptance Criteria**

- Input fields are consistently aligned.
- Spacing is visually consistent.
- The layout remains usable on supported screens.
- Existing login functionality is unchanged.

**Verification**

Inspect the modified XML and run available build checks. Report any visual verification that still requires a device or emulator.

### Why This Works

The prompt defines the intended responsibility boundary.

The agent is asked to modify presentation details without altering unrelated application behavior.

---

## 13. Architectural Constraints in Laravel

Laravel provides a structured framework for building web applications and APIs. Developers still decide how responsibilities should be organized within their projects.

### 13.1 Example Structure

```text
app/
├── Http/
│   ├── Controllers/
│   └── Requests/
├── Models/
└── Services/
```

The `Services` directory is a project convention, not a mandatory Laravel directory.

### 13.2 Component Responsibilities

- **Controllers** handle incoming requests, coordinate application components, and return HTTP responses.
- **Form Requests** can encapsulate input validation and request authorization when validation deserves a dedicated class.
- **Services**, where the project uses them, may handle business rules, workflows, and reusable operations.
- **Eloquent models** represent and interact with application data.

### 13.3 Example

```php
<?php

namespace App\Http\Controllers;

use App\Http\Requests\StoreTaskRequest;
use App\Services\TaskService;
use Illuminate\Http\JsonResponse;

class TaskController extends Controller
{
    public function store(
        StoreTaskRequest $request,
        TaskService $taskService
    ): JsonResponse {
        $task = $taskService->create(
            $request->validated()
        );

        return response()->json([
            'data' => $task,
        ], 201);
    }
}
```

This conceptual example assumes compatible `StoreTaskRequest` and `TaskService` classes already exist. Do not create a service or Form Request for every trivial endpoint automatically.

---

## 14. Real-World Laravel Example — Registration Validation

An application fails to reject duplicate email addresses during registration.

**Poor prompt:** "Fix authentication and improve the code." This is broad and may cause unrelated refactoring.

**Better prompt:**

**Task:** Investigate duplicate-email validation in the registration endpoint.

**Context:** The backend uses Laravel and PostgreSQL. Follow existing validation and controller conventions.

**Constraints:** Preserve authentication architecture and API response conventions. Do not modify unrelated endpoints, introduce packages, or change the database schema without approval.

**Acceptance criteria:** Duplicate email addresses are rejected, valid registration continues to work, and existing authentication behavior remains intact.

**Verification:** Run relevant tests and report the root cause, changed files, and any unverified behavior.

---

## 15. Coding Standards vs Automated Enforcement

Written standards guide developers and agents. Automated tools verify some rules more consistently.

| Tool or Practice | Purpose |
|---|---|
| ESLint | JavaScript and TypeScript linting |
| TypeScript compiler | Type checking |
| Laravel Pint | PHP code formatting |
| PHPUnit / Pest | PHP automated testing |
| Android Lint | Android static analysis |
| Gradle build | Android build verification |
| Git diff | Change inspection |
| Code review | Architectural and behavioral evaluation |

Automated formatting can identify style violations, but it cannot prove every architectural decision is correct. A formatter may verify indentation without detecting inappropriate business responsibilities in an Activity.

Combine written standards, automated checks, architectural review, and developer verification.

---

## 16. Documenting Standards for Codex

Store coding standards in durable project documentation.

```text
project/
├── AGENTS.md
├── docs/
│   ├── architecture.md
│   ├── coding-standards.md
│   └── testing.md
└── src/
```

`AGENTS.md` can give concise references:

```markdown
## Engineering Standards

Follow the existing project architecture.
Read docs/coding-standards.md when modifying application code.
Read docs/architecture.md before changing component boundaries.
Avoid unrelated refactoring and unnecessary dependencies.
```

`coding-standards.md` can document naming, formatting, error handling, dependencies, and testing. `architecture.md` can document component responsibilities, dependency boundaries, constraints, and approved exceptions.

Avoid duplicating detailed rules in `AGENTS.md`; reference the supporting documents instead.

---

## 17. Understanding Architectural Drift

**Architectural Drift** occurs when a system gradually moves away from its intended architecture.

Common causes include unreviewed changes, repeated shortcuts, inconsistent patterns, unnecessary dependencies, poorly scoped refactoring, and outdated documentation.

For example, a Laravel project may start with business logic in services, then gradually move complex logic into controllers. The resulting inconsistency makes maintenance, testing, and component responsibilities less predictable.

Prevent drift by maintaining accurate documentation, reviewing significant changes, using focused prompts, applying existing conventions, verifying dependency boundaries, and updating documentation when approved decisions change.

---

## 18. Preventing Overengineering

**Overengineering** introduces more complexity than the requirements justify.

Examples include adding a library for a simple XML layout adjustment, creating multiple service layers for a trivial operation, introducing event-driven architecture without a need, replacing stable architecture during a minor bug fix, or implementing unapproved future features.

```markdown
## Implementation Simplicity

Prefer the simplest maintainable solution that satisfies the approved requirements.
Do not introduce new layers, frameworks, dependencies, or features without a demonstrated need.
```

Simple does not mean careless: preserve correct behavior, validation, readable code, error handling, relevant tests, and maintainability.

**Key Principle:** Use the minimum necessary complexity, not the minimum possible effort.

---

## 19. Practical Codex Workflow — Architecture Review

1. Open the project, for example:

   ```powershell
   cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
   ```

2. Inspect Git status with `git status`.
3. Start a read-only session with `codex --sandbox read-only`.
4. Ask Codex to read applicable `AGENTS.md`, architecture, and coding standards documentation; inspect the structure; identify conventions and inconsistencies; and not modify files.
5. Check whether findings match the repository, cite actual files, and stay within scope.
6. Request a plan listing the smallest maintainable approach, affected files, constraints, and verification steps.
7. Implement only after review.
8. Verify with `git diff --check`, `git diff`, `git status`, and relevant project checks.

---

## 20. Practical Exercise A — Identify Responsibilities

An Android Activity currently performs UI rendering, input validation, Firebase retrieval, task persistence, and navigation. Identify which responsibilities should remain in the Activity and which may belong in existing data-access or other components.

1. Which responsibilities are presentation-related?
2. Which involve persistent data?
3. Which may need reuse?
4. Would additional classes improve maintainability?
5. Which changes are unnecessary for a small MVP?

**Expected outcome:** Distinguish component responsibilities without automatically creating excessive architecture.

---

## 21. Practical Exercise B — Laravel Architecture Review

A controller contains request validation, database queries, business calculations, and JSON response formatting. Evaluate whether the existing architecture would benefit from separating responsibilities.

1. Which responsibilities belong to HTTP coordination?
2. Which operations are business rules?
3. Would a Form Request improve validation organization?
4. Does the project already use service classes?
5. Which changes would be unnecessary refactoring?

**Expected outcome:** Propose changes based on existing conventions rather than introducing layers automatically.

---

## 22. Practical Exercise C — Create Coding Standards

Create `docs/coding-standards.md` and include programming language conventions, naming, formatting, error handling, dependency policy, and testing requirements.

```markdown
# Coding Standards

## Language
Use the project's existing programming language.

## Naming
Follow established naming conventions.

## Dependencies
Do not add unnecessary packages.

## Architecture
Preserve existing component responsibilities.

## Verification
Run relevant checks and report results.
```

Reflect: Are the instructions actionable and project-specific? Are any rules too restrictive? Is information duplicated from `AGENTS.md`? Can Codex verify compliance?

---

## 23. Practical Exercise D — Architecture-Constrained Prompting

A Laravel application needs a product search endpoint. Write a prompt with task, context, constraints, acceptance criteria, and verification.

Requirements: use existing API architecture, preserve response conventions, avoid unnecessary dependencies, do not modify unrelated endpoints, and provide relevant tests.

**Sample prompt**

**Task:** Implement product search in the existing product API.

**Context:** The application uses Laravel and PostgreSQL. Follow existing controller, validation, and service conventions.

**Constraints:** Do not add dependencies or modify unrelated endpoints. Preserve response structure. Do not change the schema without approval.

**Acceptance criteria:** Products can be searched by name; empty input follows approved behavior; existing endpoints remain functional.

**Verification:** Run relevant tests and report changed files, results, and remaining risks.

---

## 24. Common Mistakes

1. **Treating style as architecture:** Naming and formatting do not replace architectural boundaries.
2. **Creating layers automatically:** Not every feature needs repositories, services, interfaces, and abstractions.
3. **Ignoring existing patterns:** New implementations should normally follow established conventions.
4. **Adding dependencies without justification:** Libraries introduce maintenance costs.
5. **Refactoring unrelated modules:** This increases review and regression risk.
6. **Duplicating business logic:** Repeated rules can create inconsistent behavior.
7. **Assuming a successful build proves architecture:** A build does not prove maintainability.
8. **Allowing documentation to go stale:** Update it when approved decisions change.
9. **Writing excessive rules:** Rigid instructions can reduce flexibility and introduce contradictions.
10. **Trusting generated code without review:** Inspect and verify AI-generated code.

---

## 25. Knowledge Check

1. What are coding standards?
2. What are architectural constraints?
3. How do coding standards differ from architectural rules?
4. What is Separation of Concerns?
5. What is the Single Responsibility Principle?
6. What are architectural boundaries?
7. What is MVC?
8. What is a service layer?
9. Why are dependency policies important?
10. What responsibilities should an Android Activity typically handle?
11. What responsibilities should Laravel controllers typically handle?
12. What is architectural drift?
13. What is overengineering?
14. Why should Codex follow existing project conventions?
15. How can developers verify architectural compliance?

---

## 26. Key Terminologies

| Term | Definition |
|---|---|
| Coding Standards | Rules governing code consistency |
| Architectural Constraints | Restrictions governing system design |
| Separation of Concerns | Dividing software into distinct responsibilities |
| Single Responsibility Principle | Keeping components focused on cohesive responsibilities |
| Architectural Boundary | Limit between component responsibilities |
| MVC | Model–View–Controller architecture |
| Layered Architecture | Organization into logical application layers |
| Service Layer | Component layer for application or business operations |
| Repository | Component that abstracts or coordinates data access |
| Dependency Policy | Rules governing external package usage |
| Coupling | Degree of dependency between components |
| Cohesion | Degree to which a component's responsibilities belong together |
| Architectural Drift | Gradual deviation from intended architecture |
| Overengineering | Unnecessary complexity beyond requirements |
| Static Analysis | Automated inspection of code without executing its normal runtime behavior |

---

## 27. Key Takeaways

1. **Coding standards establish consistency in how source code is written.**
2. **Architectural constraints define how responsibilities and dependencies should be organized.**
3. **Separation of Concerns helps maintain clear component boundaries.**
4. **The Single Responsibility Principle encourages cohesive software components.**
5. **MVC and layered architecture are approaches, not universal requirements.**
6. **Dependency policies help prevent unnecessary complexity and maintenance overhead.**
7. **Android applications benefit from separating UI and data responsibilities.**
8. **Laravel applications should follow established controller, validation, and business-logic conventions.**
9. **AGENTS.md and supporting documentation can communicate durable engineering rules to Codex.**
10. **Architectural consistency requires code review and verification, not just written instructions.**
11. **Avoid overengineering by choosing the simplest maintainable solution appropriate to the project.**

---

## 28. Official References

### OpenAI Codex

1. [OpenAI — Codex Documentation](https://developers.openai.com/codex/)
2. [OpenAI — AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
3. [OpenAI — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
4. [OpenAI — Custom Code Review Rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)

### Android

5. [Android Developers — Guide to App Architecture](https://developer.android.com/topic/architecture)
6. [Android Developers — Data Layer](https://developer.android.com/topic/architecture/data-layer)
7. [Android Developers — Recommendations for Android Architecture](https://developer.android.com/topic/architecture/recommendations)

### Laravel

8. [Laravel 12.x — Controllers](https://laravel.com/docs/12.x/controllers)
9. [Laravel 12.x — Validation](https://laravel.com/docs/12.x/validation)
10. [Laravel 12.x — Testing](https://laravel.com/docs/12.x/testing)

### PHP

11. [PHP-FIG — PSR Standards](https://www.php-fig.org/psr/)

---

## 29. Lesson Completion Checklist

- [ ] Understand coding standards.
- [ ] Understand architectural constraints.
- [ ] Differentiate code style from architecture.
- [ ] Explain Separation of Concerns.
- [ ] Understand the Single Responsibility Principle.
- [ ] Recognize architectural boundaries.
- [ ] Understand MVC.
- [ ] Understand layered architecture.
- [ ] Define a dependency policy.
- [ ] Understand Android UI and data responsibilities.
- [ ] Understand Laravel controller and service responsibilities.
- [ ] Recognize architectural drift.
- [ ] Understand how to prevent overengineering.
- [ ] Create a basic coding standards document.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[06 - Task Scoping and Preventing Overengineering]]**

The next lesson focuses on controlling implementation scope when working with Codex. Topics include defining task boundaries, in-scope and out-of-scope requirements, non-goals, scope creep, approval checkpoints, minimal implementation strategies, and preventing unnecessary architecture and dependencies.

---

*Phase 2 — Context Engineering | Lesson 2.5*  
*Learning Principle: Preserve established architecture, enforce meaningful standards, and introduce only necessary complexity.*

---
