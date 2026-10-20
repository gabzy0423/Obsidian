# 02 — Codebase Exploration and Analysis

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 3 — Agentic Coding Workflow
> **Lesson:** 3.2 — Codebase Exploration and Analysis
> **Difficulty:** Intermediate
> **Platform:** Windows / PowerShell / Codex CLI
> **Status:** In Progress
> **Prerequisite:** [[01 - Understanding the Agentic Development Lifecycle]]
> **Learning Approach:** Concepts, repository inspection, practical examples, and hands-on exercises

---

## 1. Introduction

Before an AI coding agent can reliably modify an existing software project, it must understand how that project is organized.

A repository may contain hundreds or thousands of files, multiple modules, external dependencies, configuration files, and established architectural conventions. Making changes without first understanding these elements can introduce defects, duplicate existing functionality, or violate the intended architecture.

This is why **Codebase Exploration and Analysis** is an essential stage of agentic software development.

Codebase exploration is the systematic process of inspecting a repository to understand its structure, technologies, entry points, dependencies, components, and execution flow.

Codebase analysis goes further by examining how those components interact and identifying the specific implementation relevant to a development task.

OpenAI Codex can assist with both activities by inspecting files, searching for symbols, tracing relationships, and explaining existing behavior. However, the developer must still evaluate the accuracy of the agent's findings.

**Core Principle: Understand the existing implementation before deciding how to change it.**

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define codebase exploration and codebase analysis.
2. Explain repository reconnaissance.
3. Identify important repository files and directories.
4. Use targeted searches instead of reading every file.
5. Identify application entry points.
6. Analyze project dependencies.
7. Discover architectural patterns.
8. Trace code execution and data flow.
9. Distinguish documented behavior from implemented behavior.
10. Use Codex CLI for read-only repository exploration.
11. Apply exploration techniques to Android Java and Laravel projects.
12. Produce a concise, evidence-based codebase analysis report.

## 3. What Is Codebase Exploration?

**Codebase Exploration** is the process of systematically inspecting an existing software repository to understand its organization and implementation.

It focuses on answering questions such as:

- What does this application do?
- Which programming languages does it use?
- Which frameworks are installed?
- Where are the main source files?
- How is the application started?
- Where is a particular feature implemented?
- Which components depend on one another?

### 3.1 Example

Consider an unfamiliar Android repository. Before changing its login screen, a developer needs to identify the relevant Activity or Fragment, its XML layout, authentication-related components, navigation behavior, existing styles and resources, and relevant tests.

Without this exploration, the developer may modify the wrong file or duplicate existing functionality.

### 3.2 Exploration vs. Analysis

| Activity | Primary question |
|---|---|
| Exploration | What exists in this repository? |
| Analysis | How does it work, and why is it relevant? |
| Planning | What should be changed? |
| Implementation | How should the change be made? |
| Verification | Did the change work correctly? |

Exploration and analysis are closely related but have different purposes.

## 4. What Is Repository Reconnaissance?

**Repository Reconnaissance** is the initial inspection of a codebase to understand its overall structure and development environment. It is the first pass before investigating specific features.

### 4.1 Main Objectives

- Identify the project type.
- Identify the technology stack.
- Inspect repository organization.
- Locate project instructions.
- Identify build and dependency files.
- Check the Git working tree.
- Locate documentation and tests.

### 4.2 Recommended Reconnaissance Sequence

```text
Repository Root
      |
      v
Project Instructions
      |
      v
README / Documentation
      |
      v
Build and Dependency Files
      |
      v
Top-Level Source Structure
      |
      v
Application Entry Points
      |
      v
Relevant Feature Modules
```

### 4.3 Why This Order?

It allows the agent to understand the project at a high level before inspecting detailed implementation code.

### 4.4 Important Principle

**Start with the repository map, not the entire repository contents.**

## 5. Inspecting the Repository with PowerShell

### 5.1 Navigate to the Project

Android example:

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
```

Laravel example:

```powershell
cd C:\xampp\htdocs\ecommerce\backend
```

### 5.2 Confirm the Working Directory

```powershell
Get-Location
```

### 5.3 List Top-Level Files

```powershell
Get-ChildItem
```

Include hidden items:

```powershell
Get-ChildItem -Force
```

Be careful not to display or share sensitive configuration contents.

### 5.4 Inspect Git Status

```powershell
git status
```

### 5.5 Identify the Repository Root

```powershell
git rev-parse --show-toplevel
```

### 5.6 List Git-Tracked Files

```powershell
git ls-files
```

This command is often more useful than recursively listing every file because it focuses on files tracked by Git. It does not include untracked files.

### 5.7 Inspect Top-Level Directories

```powershell
Get-ChildItem -Directory
```

### 5.8 Important Warning

Avoid beginning with an unrestricted recursive search through directories such as `node_modules/`, `vendor/`, `.git/`, `build/`, and `.gradle/`. These directories can contain large amounts of generated or third-party content. Inspect them only when necessary.

## 6. Discovering Project Instructions

Before exploring implementation details, identify the applicable project instructions.

### 6.1 Common Instruction Sources

- Global `AGENTS.md`
- Repository-root `AGENTS.md`
- Nested `AGENTS.md`
- Referenced project documentation

### 6.2 Find AGENTS.md Files

From the repository root:

```powershell
Get-ChildItem -Path . -Filter AGENTS.md -Recurse -File
```

For a large repository, a tracked-file search may be faster:

```powershell
git ls-files '*AGENTS.md'
```

The second command finds tracked files matching the pattern but may omit untracked instruction files.

### 6.3 Read the Root Instructions

```powershell
Get-Content AGENTS.md
```

Only run this if the file exists.

### 6.4 Important Consideration

Finding every instruction file does not mean every instruction applies to the current task. Codex uses instruction discovery and directory scope.

Refer to: **[[03 - Working with AGENTS.md]]**

### 6.5 Example Codex Prompt

“Identify the applicable AGENTS.md instructions for the current working directory. Summarize the project rules and relevant documentation references. Do not modify files.”

## 7. Understanding Repository Structure

Repository structure provides clues about the application's architecture.

### 7.1 Example — Android

```text
GroupTrack/
├── AGENTS.md
├── docs/
├── app/
│   ├── build.gradle
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml
│       │   ├── java/
│       │   └── res/
│       └── test/
├── gradle/
├── settings.gradle
└── gradlew.bat
```

This is illustrative. Projects using Gradle Kotlin DSL may use `.gradle.kts` files instead.

### 7.2 Example — Laravel

```text
backend/
├── app/
│   ├── Http/
│   ├── Models/
│   └── Providers/
├── bootstrap/
├── config/
├── database/
├── public/
├── resources/
├── routes/
├── tests/
├── artisan
└── composer.json
```

### 7.3 What Can Structure Reveal?

It may indicate framework conventions, application modules, separation of concerns, test organization, dependency management, and build configuration.

### 7.4 Important Limitation

Directory names alone do not prove architectural behavior. Inspect the actual files and relationships before making conclusions.

## 8. Targeted File Searching

**Targeted Searching** means looking for files or code related to a specific question rather than reading the entire repository.

### 8.1 Why Targeted Searches Matter

They reduce unnecessary context consumption, search noise, repeated file reading, and time spent on unrelated modules.

### 8.2 Find Files by Extension

Java:

```powershell
Get-ChildItem -Path app\src -Recurse -Filter *.java -File
```

XML:

```powershell
Get-ChildItem -Path app\src\main\res -Recurse -Filter *.xml -File
```

PHP:

```powershell
Get-ChildItem -Path app -Recurse -Filter *.php -File
```

### 8.3 Search by Filename

```powershell
Get-ChildItem -Recurse -Filter '*Login*' -File
```

### 8.4 Search File Contents

PowerShell:

```powershell
Get-ChildItem -Path app -Recurse -Filter *.php -File |
    Select-String -Pattern 'function store'
```

### 8.5 Search Git-Tracked Files

```powershell
git grep -n 'TaskController'
```

`git grep` is useful for searching tracked repository content.

### 8.6 Using ripgrep

If `rg` is installed:

```powershell
rg -n "TaskController" app
```

Find filenames:

```powershell
rg --files app
```

Search for authentication-related code:

```powershell
rg -n "login|authenticate|signIn" app
```

### 8.7 Choosing a Search Tool

| Tool | Best use |
|---|---|
| `Get-ChildItem` | Directory and filename discovery |
| `Select-String` | PowerShell content searches |
| `git grep` | Searching tracked repository files |
| `rg` | Fast targeted content and filename searches |
| VS Code Search | Interactive repository navigation |

### 8.8 Important Principle

**Search for the smallest relevant set of files before reading their full contents.**

## 9. Search Strategy: Broad to Narrow

An effective exploration strategy gradually narrows the investigation.

### 9.1 Level 1 — Repository Overview

Identify the framework, languages, major directories, documentation, and build tools.

### 9.2 Level 2 — Feature Location

Search for feature names, routes, Activities, controllers, models, and relevant resources.

### 9.3 Level 3 — Component Relationships

Inspect imports, method calls, dependencies, navigation, and data access.

### 9.4 Level 4 — Specific Behavior

Trace the actual execution path.

### 9.5 Example

Task: investigate duplicate-email validation.

```text
Repository Overview
       |
       v
Authentication Routes
       |
       v
Registration Controller
       |
       v
Validation Rules
       |
       v
User Model / Database Constraints
       |
       v
Related Tests
```

### 9.6 Benefits

This approach is more efficient than inspecting every file containing the word `email`.

## 10. Identifying Application Entry Points

An **Entry Point** is a location where execution or an application workflow begins. Different frameworks use different entry-point mechanisms.

### 10.1 Examples

| Technology | Typical entry point |
|---|---|
| Android | Manifest-declared launcher component |
| Laravel HTTP | `public/index.php` |
| Laravel route handling | Registered route definitions |
| Next.js App Router | Route files such as `app/page.tsx` |
| CLI application | Main command or executable |

### 10.2 Why Entry Points Matter

They help developers trace how an external action reaches application logic.

### 10.3 Example

```text
HTTP Request
      |
      v
Framework Entry Point
      |
      v
Route
      |
      v
Controller
      |
      v
Application Logic
```

### 10.4 Important Principle

An application can have multiple entry points for different workflows.

## 11. Android Entry-Point Discovery

Android applications declare components and intent filters through `AndroidManifest.xml`.

### 11.1 Locate the Manifest

Typical path:

```text
app/src/main/AndroidManifest.xml
```

### 11.2 Inspect the Manifest

```powershell
Get-Content app\src\main\AndroidManifest.xml
```

### 11.3 Look For

- Activity declarations
- Launcher intent filters
- Services
- Receivers
- Permissions

### 11.4 Example

```xml
<activity
    android:name=".MainActivity"
    android:exported="true">

    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>

</activity>
```

This example shows a launcher Activity declaration.

### 11.5 Trace the Flow

```text
App Launch
     |
     v
Launcher Activity
     |
     v
Layout / UI
     |
     v
User Interaction
     |
     v
Application Logic
```

### 11.6 Important Consideration

Modern Android applications may use one Activity with multiple destinations. Do not assume every screen has a separate Activity. Inspect the actual navigation architecture.

## 12. Laravel Entry-Point Discovery

Laravel handles incoming HTTP requests through its framework bootstrap and routing system.

### 12.1 Main HTTP Entry Point

```text
public/index.php
```

### 12.2 Relevant Directories

```text
routes/
app/Http/Controllers/
app/Models/
```

Other application-specific directories may also exist.

### 12.3 Inspect Registered Routes

```powershell
php artisan route:list
```

This command boots the Laravel application and may execute application initialization logic. Use it only in a trusted local project with an appropriate environment.

### 12.4 Inspect Route Files

```powershell
Get-ChildItem routes
```

### 12.5 Example Route

```php
use App\Http\Controllers\TaskController;

Route::post('/tasks', [TaskController::class, 'store']);
```

### 12.6 Trace the Request

```text
POST /tasks
     |
     v
Route Definition
     |
     v
TaskController::store()
     |
     v
Validation / Service Logic
     |
     v
Model / Database
     |
     v
HTTP Response
```

### 12.7 Important Consideration

Laravel 12 does not require every project to contain an `api.php` route file by default. Inspect the actual project configuration and registered routes.

## 13. Dependency Analysis

**Dependency Analysis** is the process of identifying the external packages, frameworks, libraries, and internal components that a project relies on.

### 13.1 External Dependencies

Examples include Firebase SDK, Laravel packages, React libraries, and AndroidX libraries.

### 13.2 Internal Dependencies

Examples include a controller depending on a service, an Activity using a repository, a component importing another component, and a model referenced by multiple services.

### 13.3 Why Analyze Dependencies?

Dependencies affect compatibility, maintenance, architecture, testing, build behavior, and change impact.

## 14. Android Dependency Analysis

### 14.1 Important Files

Common files include:

```text
settings.gradle
build.gradle
app/build.gradle
gradle/libs.versions.toml
```

Depending on the project, these may use `.gradle.kts` extensions.

### 14.2 Inspect Gradle Configuration

```powershell
Get-Content app\build.gradle
```

Only if that file exists.

### 14.3 Identify

- Android Gradle Plugin version
- Java compatibility
- Compile SDK
- Minimum SDK
- Target SDK
- Firebase dependencies
- AndroidX dependencies

### 14.4 Example

```gradle
dependencies {
    implementation("androidx.appcompat:appcompat:1.7.0")
}
```

This is an illustrative dependency declaration, not a recommendation to install that version.

### 14.5 Dependency Graph

For supported Gradle configurations:

```powershell
.\gradlew.bat :app:dependencies
```

This resolves dependency configuration and may perform Gradle initialization or network access. Do not assume it is a strictly read-only operation outside the repository.

### 14.6 Key Principle

Inspect existing dependencies before proposing additional libraries.

## 15. Laravel Dependency Analysis

### 15.1 Important Files

```text
composer.json
composer.lock
```

### 15.2 Inspect Composer Configuration

```powershell
Get-Content composer.json
```

### 15.3 List Installed Packages

```powershell
composer show
```

This command reports installed package information when dependencies are available.

### 15.4 Important Distinction

`composer.json` defines declared dependency requirements. `composer.lock` records resolved package versions. The installed environment may differ if dependencies have not been installed or synchronized.

### 15.5 Dependency Questions

- Which Laravel version is required?
- Which PHP version is supported?
- Which packages are already available?
- Is a proposed package necessary?
- Could an existing framework feature solve the problem?

## 16. Architecture Discovery

**Architecture Discovery** is the process of identifying how a software system organizes its components and responsibilities.

### 16.1 Common Patterns

- MVC
- Layered architecture
- Repository pattern
- Service layer
- Feature-based organization
- Modular architecture

### 16.2 Discovery Questions

1. Where is presentation logic located?
2. Where are business rules implemented?
3. How is data accessed?
4. How do components communicate?
5. Which components are shared?
6. Are there existing architectural conventions?
7. Do the actual files match the documentation?

### 16.3 Example — Laravel

```text
Controller
    |
    v
Service
    |
    v
Model
    |
    v
Database
```

This pattern is appropriate only if it reflects the actual project.

### 16.4 Example — Android

```text
Activity / UI
      |
      v
Existing Application Logic
      |
      v
Repository / Data Access
      |
      v
Firebase
```

### 16.5 Important Principle

**Discover the architecture that exists rather than imposing an architecture that the agent prefers.**

## 17. Tracing Execution Flow

Execution-flow analysis examines the sequence of operations involved in a particular behavior.

### 17.1 Example — Android Login

```text
User taps Login
        |
        v
Click Listener
        |
        v
Input Validation
        |
        v
Authentication Call
        |
        v
Success / Failure Callback
        |
        v
Navigation or Error Message
```

### 17.2 Questions to Investigate

- Where is the click listener registered?
- Which method receives the event?
- How is input validated?
- Which authentication API is called?
- How are errors handled?
- What happens after successful login?

### 17.3 Example — Laravel Registration

```text
POST /register
       |
       v
Route
       |
       v
Controller
       |
       v
Validation
       |
       v
User Creation
       |
       v
Response
```

### 17.4 Important Consideration

Real workflows may contain middleware, events, asynchronous callbacks, or other components. Trace the actual code instead of relying only on simplified diagrams.

## 18. Understanding Data Flow

**Data Flow Analysis** examines how information moves between components.

### 18.1 Example — Android

```text
Text Input
    |
    v
Activity
    |
    v
Validation
    |
    v
Data Access
    |
    v
Firebase
```

### 18.2 Example — Laravel

```text
HTTP Request
     |
     v
Validation
     |
     v
Business Logic
     |
     v
Eloquent Model
     |
     v
Database
```

### 18.3 Questions

- Where does data originate?
- Where is it validated?
- Where is it transformed?
- Where is it stored?
- Who can access it?
- How are failures handled?

### 18.4 Why It Matters

Data-flow understanding helps prevent changes that break validation, persistence, or component communication.

## 19. Understanding Change Impact

**Change Impact Analysis** identifies the components that may be affected by a proposed modification.

### 19.1 Example

A developer wants to rename a field used by the Laravel API. Potentially affected components include validation rules, models, API resources, controllers, frontend consumers, tests, and documentation.

### 19.2 Recommended Process

1. Identify the target symbol or behavior.
2. Search for references.
3. Inspect direct dependencies.
4. Identify external consumers.
5. Review relevant tests.
6. Determine the minimum required changes.

### 19.3 Useful Search

```powershell
git grep -n 'task_status'
```

### 19.4 Important Principle

A small code edit can have a large behavioral impact. Always investigate relevant dependencies before changing shared interfaces.

## 20. Documentation vs. Implementation

Project documentation describes intended behavior. Source code reveals the implemented behavior. These may not always match.

### 20.1 Example

Documentation: “Authentication is handled by Firebase.”

Actual implementation: “Authentication is temporarily simulated using local values.”

### 20.2 Correct Response

The agent should report the discrepancy. It should not automatically assume that the documentation or implementation is correct.

### 20.3 Evidence Hierarchy

For understanding current behavior, prioritize:

1. Relevant executable code.
2. Relevant tests and observed runtime behavior.
3. Current configuration.
4. Maintained project documentation.
5. Unverified assumptions.

This is a practical investigation guideline, not an absolute rule for deciding product requirements. Approved specifications remain authoritative for intended behavior.

### 20.4 Key Principle

**Distinguish what the system should do from what the system currently does.**

## 21. Read-Only Exploration with Codex CLI

A read-only exploration session is useful when the developer wants analysis without repository modifications.

### 21.1 Navigate to the Repository

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
```

### 21.2 Inspect Git Status

```powershell
git status
```

### 21.3 Start Codex

```powershell
codex --sandbox read-only
```

### 21.4 Submit a Repository Orientation Prompt

```text
Perform a read-only exploration of this repository.

Read applicable AGENTS.md instructions and
relevant project documentation.

Identify:
1. Project purpose
2. Technology stack
3. Main directories
4. Application entry points
5. Major architectural components
6. Dependency configuration
7. Testing structure

Do not modify files.

Support important findings with actual
repository paths.

Clearly distinguish observed facts
from assumptions.
```

### 21.5 Expected Result

A structured repository overview.

### 21.6 Important Limitation

Read-only sandboxing limits filesystem modifications by the agent, but it does not mean every possible command is harmless. Avoid executing unfamiliar scripts, installation commands, migrations, or commands with external side effects during an exploration-only task.

## 22. Targeted Feature Exploration with Codex

After repository reconnaissance, narrow the investigation to the relevant feature.

### 22.1 Android Prompt

```text
Inspect the existing login feature.

Identify:
- Activity or Fragment
- XML layout
- Relevant view IDs
- Authentication dependencies
- Navigation flow
- Related tests

Explain the execution flow from
user interaction to authentication result.

Do not modify files.

Do not propose unrelated refactoring.
```

### 22.2 Laravel Prompt

```text
Inspect the registration endpoint.

Identify:
- Registered route
- Controller method
- Validation logic
- Related models
- Database constraints
- Existing tests

Trace the request flow and identify
the components involved.

Do not modify files.

Report findings with file references.
```

### 22.3 Why This Works

The agent receives a focused investigation objective rather than an unrestricted request to analyze the entire application.

## 23. Writing a Codebase Analysis Report

A useful analysis report should explain the repository without overwhelming the developer.

### 23.1 Recommended Structure

```markdown
# Codebase Analysis Report

## 1. Project Overview
Purpose and technology stack.

## 2. Repository Structure
Important directories and responsibilities.

## 3. Entry Points
Where application execution begins.

## 4. Architecture
Major components and relationships.

## 5. Dependencies
Relevant frameworks and libraries.

## 6. Feature Flow
How the investigated behavior works.

## 7. Findings
Observed issues or inconsistencies.

## 8. Unknowns
Information not confirmed.

## 9. Recommended Next Steps
Focused investigation or planning tasks.
```

### 23.2 Evidence-Based Reporting

Instead of:

> Authentication is implemented securely.

Prefer:

> The registration controller uses the project's existing validation mechanism. Authentication security has not been comprehensively audited.

### 23.3 Why Evidence Matters

A report should make it possible for the developer to verify the agent's conclusions.

## 24. Real-World Example — GroupTrack Android

### Scenario

The developer wants to understand the existing Android UI architecture before implementing a new screen.

### 24.1 Repository Context

GroupTrack uses Android Studio, Java, XML layouts, Firebase, and Gradle.

### 24.2 Exploration Objectives

Identify:

1. Existing screen structure.
2. Relevant Activities.
3. XML layout conventions.
4. Drawable and resource organization.
5. Navigation behavior.
6. Firebase integration points.
7. Existing tests.

### 24.3 Example Codex Prompt

```text
Analyze the existing Android UI structure
in the GroupTrack repository.

Read applicable AGENTS.md instructions.

Inspect:
- AndroidManifest.xml
- Relevant Java UI classes
- XML layout resources
- Drawable resources
- Navigation-related code

Explain:
1. How screens are organized
2. How layouts connect to Java classes
3. How navigation currently works
4. Which UI conventions are reused
5. Which files are relevant to a new screen

Do not modify files.

Do not recommend new UI libraries
unless an actual requirement justifies them.
```

### 24.4 Expected Output

The agent should produce a map of the existing UI structure. It should not implement a new screen during this exploration stage.

## 25. Real-World Example — Laravel Backend

### Scenario

A Laravel application needs investigation before implementing a new API feature.

### 25.1 Exploration Objectives

Identify API route organization, controller conventions, validation patterns, service classes if present, Eloquent models, database migrations, and test structure.

### 25.2 Example Codex Prompt

```text
Perform a read-only analysis of the
existing Laravel backend architecture.

Inspect:
- routes/
- app/Http/Controllers/
- app/Http/Requests/
- app/Models/
- app/Services/ if present
- database/migrations/
- tests/

Identify:
1. API entry points
2. Controller responsibilities
3. Existing business logic patterns
4. Validation conventions
5. Database relationships relevant to APIs
6. Existing test coverage

Do not modify files.

Do not assume directories or components
exist without verifying them.
```

### 25.3 Expected Output

A grounded description of the existing backend architecture.

### 25.4 Important Principle

Do not automatically create new services, repositories, or abstractions simply because those patterns are common in other Laravel applications.

## 26. Common Codebase Exploration Mistakes

1. **Reading every file:** Excessive context consumption. Start with repository structure and targeted searches.
2. **Ignoring AGENTS.md:** Important project constraints may be missed. Read applicable instructions before investigating implementation.
3. **Assuming directory names prove architecture:** Folder structure may not reflect actual behavior. Inspect component relationships and code paths.
4. **Ignoring Git status:** Existing user changes may be confused with agent modifications. Inspect the working tree before beginning.
5. **Searching generated dependencies:** Search results become noisy and inefficient. Focus on application source directories.
6. **Assuming documentation is always current:** The agent may describe outdated behavior. Compare documentation with source code.
7. **Exploring without a question:** The investigation becomes unfocused. Define the specific information needed.
8. **Proposing architecture too early:** The agent may recommend patterns without understanding existing conventions. Discover the current architecture first.
9. **Treating read-only as permission to run anything:** Some commands may still access external systems or execute project initialization logic. Use safe inspection commands and review unfamiliar commands before execution.
10. **Reporting assumptions as facts:** Developers may make decisions based on unsupported conclusions. Include file references, evidence, and unresolved questions.

## 27. Practical Exercise A — Repository Reconnaissance

### Objective

Inspect the overall structure of an existing repository.

### Instructions

1. Open a local project.
2. Identify the repository root.
3. Inspect Git status.
4. Locate `AGENTS.md`.
5. Locate project documentation.
6. Identify build files.
7. Identify source directories.
8. Identify test directories.

### Suggested Commands

```powershell
Get-Location
git status
Get-ChildItem
git ls-files
```

### Deliverable

A short repository overview containing project type, technology stack, main directories, build system, and documentation locations.

## 28. Practical Exercise B — Targeted Search

### Objective

Locate the implementation of a particular feature.

### Android Task

Find the files responsible for the login screen. Possible searches:

```powershell
Get-ChildItem -Path app\src -Recurse -Filter '*Login*' -File
```

```powershell
rg -n "signIn|login|authenticate" app/src
```

Use `rg` only if installed.

### Laravel Task

Find the registration endpoint.

```powershell
git grep -n 'register'
```

### Questions

1. Which file defines the entry point?
2. Which component handles the main behavior?
3. Which other components are involved?
4. Which tests are relevant?

### Expected Outcome

The learner can locate a feature without reading the entire repository.

## 29. Practical Exercise C — Trace a Feature Flow

### Objective

Understand how a user action reaches application logic.

### Scenario

An Android user taps the login button.

### Instructions

Trace the XML view ID, Java event listener, input handling, authentication operation, success or error callback, and navigation or feedback.

### Deliverable

A simple execution-flow diagram.

```text
Login Button
     |
     v
Click Listener
     |
     v
Validation
     |
     v
Authentication
     |
     v
Result Handling
```

### Reflection

Which parts of the flow are confirmed by source code, and which remain assumptions?

## 30. Practical Exercise D — Read-Only Codex Audit

### Objective

Use Codex to produce an evidence-based repository analysis.

### Step 1 — Open the Project

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
```

### Step 2 — Start Codex

```powershell
codex --sandbox read-only
```

### Step 3 — Submit the Prompt

```text
Perform a read-only codebase exploration.

Read the applicable AGENTS.md instructions
and relevant project documentation.

Inspect the repository structure,
technology stack, build configuration,
entry points, and major modules.

Then analyze the existing login feature.

Produce a report containing:
1. Repository overview
2. Technology stack
3. Main directories
4. Application entry points
5. Relevant login files
6. Login execution flow
7. Dependencies involved
8. Existing tests
9. Confirmed findings
10. Unresolved questions

Use actual file paths as evidence.

Do not modify files.

Do not implement features or refactor code.
```

### Step 4 — Review the Report

Check whether the agent identifies real files, follows project instructions, avoids unsupported assumptions, distinguishes intended and implemented behavior, and keeps the analysis focused.

### Expected Outcome

The learner can guide Codex through a structured codebase investigation.

## 31. Knowledge Check

1. What is codebase exploration?
2. How does codebase analysis differ from exploration?
3. What is repository reconnaissance?
4. Why should developers inspect Git status first?
5. Why are targeted searches preferable to reading every file?
6. What is an application entry point?
7. Where are Android application components declared?
8. What is Laravel's main HTTP entry point?
9. What is dependency analysis?
10. What is architecture discovery?
11. What is execution-flow analysis?
12. What is data-flow analysis?
13. What is change impact analysis?
14. Why should documentation be compared with source code?
15. What makes a codebase analysis report reliable?

## 32. Key Terminologies

| Term | Definition |
|---|---|
| Codebase exploration | Systematic inspection of a software repository |
| Codebase analysis | Investigation of implementation behavior and relationships |
| Repository reconnaissance | Initial overview of project structure and environment |
| Targeted search | Search focused on relevant files or symbols |
| Entry point | Location where execution or a workflow begins |
| Dependency analysis | Investigation of internal and external dependencies |
| Architecture discovery | Identification of existing architectural patterns |
| Execution flow | Sequence of operations during a behavior |
| Data flow | Movement of information between components |
| Change impact analysis | Evaluation of components affected by a proposed change |
| Static analysis | Examination of code without normal application execution |
| Repository map | Summary of important project directories and components |
| Read-only exploration | Investigation without intentionally modifying project files |
| Evidence-based report | Findings supported by verifiable sources |
| Context efficiency | Using relevant information without unnecessary overhead |

## 33. Key Takeaways

1. **Codebase exploration is essential before modifying unfamiliar software.**
2. **Repository reconnaissance provides an initial understanding of project structure.**
3. **Applicable AGENTS.md instructions should be considered before development.**
4. **Targeted searches reduce unnecessary context consumption.**
5. **Entry-point discovery helps trace how application workflows begin.**
6. **Dependency analysis reveals important framework and component relationships.**
7. **Architecture discovery should reflect the actual codebase.**
8. **Execution-flow and data-flow analysis explain how features operate.**
9. **Change impact analysis helps prevent unintended regressions.**
10. **Documentation and source code must be compared when investigating current behavior.**
11. **Read-only Codex workflows support safe initial investigation.**
12. **Reliable analysis reports distinguish confirmed findings from assumptions.**

## 34. Official References

### OpenAI Codex

1. **OpenAI — Codex Documentation**
   https://developers.openai.com/codex/
2. **OpenAI — Codex CLI**
   https://developers.openai.com/codex/cli/
3. **OpenAI — Codex CLI Reference**
   https://developers.openai.com/codex/cli/reference/
4. **OpenAI — AGENTS.md Guide**
   https://developers.openai.com/codex/guides/agents-md/
5. **OpenAI — Codex Security**
   https://developers.openai.com/codex/security/
6. **OpenAI — Iterating Development Workflows with Codex**
   https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex
7. **OpenAI — Modernizing Your Codebase with Codex**
   https://developers.openai.com/cookbook/examples/codex/code_modernization

### Android

8. **Android Developers — Guide to App Architecture**
   https://developer.android.com/topic/architecture
9. **Android Developers — App Manifest Overview**
   https://developer.android.com/guide/topics/manifest/manifest-intro/

### Laravel

10. **Laravel 12.x — Directory Structure**
    https://laravel.com/docs/12.x/structure
11. **Laravel 12.x — Controllers**
    https://laravel.com/docs/12.x/controllers
12. **Laravel 12.x — Routing**
    https://laravel.com/docs/12.x/routing

### Git

13. **Git — Documentation**
    https://git-scm.com/doc

## 35. Lesson Completion Checklist

- [ ] Understand codebase exploration.
- [ ] Differentiate exploration from analysis.
- [ ] Understand repository reconnaissance.
- [ ] Inspect repository structure.
- [ ] Identify applicable AGENTS.md files.
- [ ] Use targeted file searches.
- [ ] Identify application entry points.
- [ ] Analyze Android project dependencies.
- [ ] Analyze Laravel project dependencies.
- [ ] Discover architectural patterns.
- [ ] Trace execution flow.
- [ ] Trace data flow.
- [ ] Perform change impact analysis.
- [ ] Compare documentation with implementation.
- [ ] Use Codex CLI in read-only mode.
- [ ] Produce an evidence-based analysis report.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[03 - Planning Before Implementation]]**

The next lesson focuses on creating reliable implementation plans before modifying source code.

Topics include:

- Planning strategies
- Requirements analysis
- File-level change planning
- Risk assessment
- Acceptance criteria
- Approval checkpoints
- Implementation sequencing
- Verification planning
- Codex planning workflows

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.2*
*Learning Principle: Explore broadly, investigate precisely, and base conclusions on actual repository evidence.*
