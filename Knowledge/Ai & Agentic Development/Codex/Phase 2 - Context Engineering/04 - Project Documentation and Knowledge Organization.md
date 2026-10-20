# 04 — Project Documentation and Knowledge Organization

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 2 — Context Engineering
> **Lesson:** 2.4 — Project Documentation and Knowledge Organization
> **Difficulty:** Intermediate
> **Platform:** Windows / PowerShell / Codex CLI
> **Status:** In Progress
> **Prerequisite:** [[03 - Working with AGENTS.md]]

---

## 1. Introduction

AI coding agents such as OpenAI Codex can inspect existing software projects, analyze source code, implement features, and execute development tools.

However, a coding agent does not automatically understand every business requirement, architectural decision, or development convention within a repository. Some information is visible in source code. Other information exists only in project requirements, architectural discussions, technical specifications, or development decisions.

If this knowledge is not documented and accessible, the agent may make assumptions that do not align with the project's intended design.

This is why **Project Documentation and Knowledge Organization** are essential parts of Context Engineering.

Project documentation provides structured, persistent information about a software system. Knowledge organization determines how that information is categorized, connected, maintained, and retrieved.

Together, they help both developers and coding agents understand what the system should do, how it is designed, and which requirements must be respected.

The objective is not to produce the largest possible collection of documents.

**The objective is to create a reliable, maintainable, and easily navigable knowledge base that supports software development.**

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define project documentation and knowledge organization.
2. Explain why structured documentation matters in agentic coding.
3. Understand documentation architecture.
4. Apply the single-source-of-truth principle.
5. Design a practical `docs/` directory.
6. Differentiate project instructions from technical documentation.
7. Create basic architecture, feature, schema, and testing documents.
8. Organize documentation for Android and Laravel projects.
9. Apply progressive disclosure to context retrieval.
10. Maintain documentation without unnecessary complexity.
11. Recognize common documentation problems.
12. Design a documentation structure appropriate to project scope.

## 3. What Is Project Documentation?

**Project Documentation** is the collection of written information describing a software project's purpose, requirements, architecture, implementation conventions, and development procedures.

It helps developers understand how a system should be built, maintained, tested, and extended.

### 3.1 Common Documentation Categories

- Project overview
- Software architecture
- Feature requirements
- Database or data model
- API specifications
- Coding standards
- Development workflow
- Testing procedures
- Technical decisions

### 3.2 Example

Consider an Android application that uses Java, XML layouts, and Firebase. The source code may show how an Activity communicates with Firebase, but it may not explain:

- Why Firebase was selected.
- Which features belong to the MVP.
- Which user roles are supported.
- Which architectural conventions the team agreed to follow.
- Which future features are intentionally excluded.

These decisions should be documented when they materially affect development.

### 3.3 Documentation vs. Source Code

Source code explains the implementation. Documentation provides additional information about requirements, design intent, and development decisions. Both are important.

**Documentation should complement the source code rather than unnecessarily duplicate it.**

## 4. What Is Knowledge Organization?

**Knowledge Organization** is the practice of structuring information so it can be located, understood, maintained, and reused efficiently.

In software development, this includes directory organization, document naming, categorization, cross-referencing, ownership, version control, and information retrieval.

### 4.1 Unorganized Knowledge

```text
project/
├── notes1.md
├── final-notes.md
├── final-notes-v2.md
├── architecture-new.md
├── requirements-old.md
├── random-ideas.md
└── AGENTS.md
```

Problems include unclear document responsibilities, duplicate information, outdated versions, difficult navigation, and uncertain sources of truth.

### 4.2 Organized Knowledge

```text
project/
├── AGENTS.md
├── README.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── features.md
│   ├── schema.md
│   ├── coding-standards.md
│   └── testing.md
└── src/
```

This structure provides clearer responsibilities. Each document has a defined purpose.

### 4.3 Key Principle

**Organize information according to responsibility, not simply according to how much documentation exists.**

## 5. Why Documentation Matters in Agentic Coding

### 5.1 Reduces Unsupported Assumptions

Codex can inspect existing code, but it may not know the intended requirements behind that code. Documentation provides additional decision-making context.

### 5.2 Supports Architectural Consistency

An architecture document helps the agent understand existing component responsibilities and dependencies.

### 5.3 Preserves Requirements Across Sessions

Important project decisions should not depend exclusively on conversation history.

### 5.4 Prevents Scope Creep

Feature specifications clarify which capabilities belong to the approved scope.

### 5.5 Improves Team Collaboration

Documentation gives developers a shared reference for technical decisions.

### 5.6 Supports Testing

Acceptance criteria and testing procedures help the agent determine how to verify implementation work.

### 5.7 Reduces Repeated Explanations

Reusable documentation can reduce the need to restate the same project context in every prompt.

### 5.8 Important Limitation

Documentation improves the conditions for reliable agentic coding, but it does not guarantee correctness. The agent must still inspect relevant source code, run appropriate checks, and report uncertainty.

## 6. Understanding Documentation Architecture

**Documentation Architecture** is the design of how project information is divided, structured, and connected.

It answers questions such as:

- Which documents should exist?
- What information belongs in each document?
- How are documents related?
- Which documents are authoritative?
- How should developers navigate them?
- When should documentation be updated?

### 6.1 Documentation Architecture Example

```text
docs/
├── overview.md
├── architecture.md
├── features.md
├── schema.md
├── coding-standards.md
├── workflow.md
└── testing.md
```

### 6.2 Responsibilities

| Document | Responsibility |
|---|---|
| `overview.md` | Project purpose, users, scope, and goals |
| `architecture.md` | Components, layers, and system relationships |
| `features.md` | Functional requirements and acceptance criteria |
| `schema.md` | Data entities, relationships, and constraints |
| `coding-standards.md` | Coding and implementation conventions |
| `workflow.md` | Development process and collaboration practices |
| `testing.md` | Validation procedures and test expectations |

### 6.3 Is This Structure Mandatory?

No. This is a recommended starting structure. A small project may need fewer documents. A large project may require additional specifications.

**Documentation architecture should reflect actual project complexity.**

## 7. The Single Source of Truth Principle

A **Single Source of Truth (SSOT)** is an authoritative location for a particular type of information. The goal is to avoid maintaining conflicting copies of the same requirement.

### 7.1 Example Problem

Suppose the following documents contain different descriptions of user roles:

`overview.md`:

> Supported roles: Admin and Student.

`features.md`:

> Supported roles: Admin, Student, and Teacher.

`AGENTS.md`:

> Supported roles: Student only.

The agent may struggle to determine which information is correct.

### 7.2 Better Approach

Define the authoritative location for role requirements. For example:

```text
docs/features.md
```

Other documents should reference that source rather than independently redefining the same requirements.

### 7.3 Example

Inside `AGENTS.md`:

```markdown
## Feature Scope

Follow the approved feature requirements
documented in docs/features.md.

Do not introduce additional user roles or
features without explicit approval.
```

### 7.4 Source of Truth by Responsibility

| Information | Authoritative source |
|---|---|
| Approved feature requirements | `features.md` |
| Architectural decisions | `architecture.md` |
| Data model design | `schema.md` |
| Coding conventions | `coding-standards.md` |
| Testing procedures | `testing.md` |
| Actual implemented behavior | Source code and executable tests |

### 7.5 Important Distinction

Documentation describes intended behavior. Source code and tests provide evidence about actual implementation. When they disagree, developers must investigate rather than automatically assume one is correct.

### 7.6 Key Principle

**One responsibility, one authoritative source, with references from related documents.**

## 8. AGENTS.md vs. Project Documentation

These files serve different purposes.

### 8.1 AGENTS.md

Answers: “How should Codex work in this repository?”

Typical contents include project rules, architectural constraints, scope boundaries, documentation references, and verification expectations.

### 8.2 Project Documentation

Answers: “What is this project, how is it designed, and what must it do?”

Typical contents include project objectives, feature specifications, data models, component relationships, and development procedures.

### 8.3 Comparison

| Aspect | AGENTS.md | Project documentation |
|---|---|---|
| Primary purpose | Agent instructions | Project knowledge |
| Scope | Behavioral and development guidance | Technical and functional information |
| Detail | Usually concise | As detailed as necessary |
| Example | Avoid unnecessary dependencies | Explain the existing architecture |
| Usage | Guide agent behavior | Inform implementation decisions |

### 8.4 Recommended Relationship

```text
AGENTS.md
    |
    +--> docs/overview.md
    |
    +--> docs/architecture.md
    |
    +--> docs/features.md
    |
    +--> docs/testing.md
```

`AGENTS.md` acts as a navigation entry point. The supporting documents provide detailed information.

## 9. Designing a Practical docs/ Folder

### 9.1 Minimal Structure

For a small project:

```text
project/
├── AGENTS.md
├── README.md
├── docs/
│   ├── architecture.md
│   └── features.md
└── src/
```

This may be sufficient for a simple application.

### 9.2 Standard Structure

For a growing project:

```text
project/
├── AGENTS.md
├── README.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── features.md
│   ├── schema.md
│   ├── coding-standards.md
│   ├── workflow.md
│   └── testing.md
└── src/
```

### 9.3 Extended Structure

For larger projects:

```text
docs/
├── index.md
├── architecture/
│   ├── overview.md
│   └── decisions/
├── features/
│   ├── authentication.md
│   └── task-management.md
├── data/
│   └── schema.md
├── development/
│   ├── coding-standards.md
│   └── workflow.md
└── testing/
    └── strategy.md
```

### 9.4 Choosing the Right Structure

Use the smallest structure that clearly represents the project's requirements. Avoid creating empty folders or documents without a concrete purpose.

### 9.5 Naming Conventions

Recommended practices:

- Use descriptive filenames.
- Apply consistent casing.
- Avoid ambiguous names.
- Avoid unnecessary version suffixes.
- Use stable paths where possible.

Good examples:

```text
architecture.md
features.md
coding-standards.md
```

Less useful examples:

```text
notes-final-v3.md
important-stuff.md
new-document.md
```

## 10. Writing a Project Overview Document

The project overview introduces the application and its intended purpose.

### 10.1 Recommended Contents

- Project name
- Problem statement
- Target users
- Primary objectives
- Technology stack
- Scope
- Non-goals

### 10.2 Example

```markdown
# Project Overview

## Project Name
GroupTrack

## Purpose
A student project management and contribution
tracking Android application.

## Target Users
Students participating in group projects.

## Technology Stack
- Android Studio
- Java
- XML layouts
- Firebase

## Scope
Implement the approved MVP features.

## Non-Goals
Do not introduce features outside the
documented MVP without approval.
```

### 10.3 Why It Matters

This document helps the agent understand the project's purpose before making implementation decisions.

## 11. Writing Architecture Documentation

**Architecture Documentation** explains how major system components are organized and interact.

### 11.1 Recommended Contents

- Architectural approach
- Major components
- Component responsibilities
- Dependency relationships
- Data flow
- Important design constraints

### 11.2 Example — Laravel

```markdown
# System Architecture

## Technology Stack
- Laravel
- PHP
- PostgreSQL

## Component Responsibilities

### Routes
Define API endpoints.

### Controllers
Handle HTTP requests and responses.

### Services
Contain business logic where the existing
architecture uses a service layer.

### Models
Represent application data.

### Database
Stores persistent application information.

## Development Rules
- Preserve existing component boundaries.
- Avoid placing unrelated business logic
  directly in controllers.
```

### 11.3 Example Request Flow

```text
Client Request
      |
      v
API Route
      |
      v
Controller
      |
      v
Service
      |
      v
Model / Database
      |
      v
API Response
```

This is a simplified example, not a universal Laravel architecture.

### 11.4 Why It Matters

Architecture documentation helps Codex identify where changes belong and which components should remain unaffected.

## 12. Writing Feature Specifications

A **Feature Specification** defines the expected behavior of a particular capability.

### 12.1 Recommended Contents

- Feature name
- Purpose
- User role
- Functional requirements
- Validation rules
- Acceptance criteria
- Non-goals

### 12.2 Example

```markdown
# Feature: Task Creation

## Purpose
Allow authorized users to create tasks
within an existing project.

## Functional Requirements
- Enter a task title.
- Enter a task description.
- Save the task using the existing data layer.

## Validation
- Task title is required.
- Invalid input must produce clear feedback.

## Acceptance Criteria
- Valid tasks can be created.
- Invalid submissions are rejected.
- Existing project behavior remains intact.

## Non-Goals
- AI task generation
- Advanced analytics
- Additional collaboration features
```

### 12.3 Why Acceptance Criteria Matter

Acceptance criteria establish observable conditions for successful implementation. Without them, Codex may complete a technically plausible feature that does not fully satisfy the intended requirements.

## 13. Writing Data Model Documentation

Data model documentation describes the entities and relationships used by an application.

### 13.1 Common Contents

- Entity names
- Fields
- Data types
- Relationships
- Constraints
- Validation rules

### 13.2 Example

```markdown
# Data Model

## Entity: Task

### Fields
- id
- project_id
- title
- description
- status
- created_at

## Relationships
- A task belongs to a project.

## Constraints
- Title is required.
- Status must use supported values.
```

### 13.3 Important Consideration

A proposed data model should be clearly distinguished from an implemented schema. For example:

```markdown
## Status
Proposed — not yet implemented.
```

### 13.4 Why This Matters

Without implementation-status information, an agent may incorrectly assume that planned database fields already exist.

## 14. Writing Coding Standards Documentation

Coding standards define conventions that should remain consistent throughout a project.

### 14.1 Examples

- Naming conventions
- Formatting
- Component responsibilities
- Error handling
- Dependency usage
- Security practices

### 14.2 Example — Android

```markdown
# Coding Standards

## Language
Use Java for Android application code.

## UI
Use the existing XML layout approach.

## Naming
Follow existing Java and Android resource
naming conventions.

## Dependencies
Do not add libraries unless necessary
and approved.

## Implementation
- Prefer simple, maintainable solutions.
- Reuse existing components.
- Avoid unrelated refactoring.
```

### 14.3 Important Principle

Document conventions that the project actually follows. Do not invent architectural rules merely to make documentation appear more complete.

## 15. Writing Testing Documentation

Testing documentation describes how changes should be validated.

### 15.1 Recommended Contents

- Available test commands
- Relevant test types
- Required manual checks
- Build verification
- Known limitations
- Reporting expectations

### 15.2 Example — Laravel

```markdown
# Testing Guide

## Automated Tests

Run relevant tests using:

php artisan test

## Verification
- Confirm expected API behavior.
- Review validation errors.
- Check for regressions.
- Inspect the Git diff.

## Reporting
Report:
- Tests executed
- Tests passed or failed
- Tests not executed
- Remaining risks
```

### 15.3 Example — Android

```markdown
# Testing Guide

## Build Verification

Use the project's supported Gradle tasks.

## UI Verification
- Inspect layout behavior.
- Test supported screen sizes.
- Verify navigation.
- Check existing functionality.

## Reporting

Do not claim device verification unless
the application was actually tested.
```

### 15.4 Why This Matters

A coding agent should distinguish between successful automated checks and functionality that still requires manual testing.

## 16. Progressive Disclosure in Documentation

**Progressive Disclosure** is the practice of providing information in stages rather than presenting everything at once.

In Context Engineering, this means giving the agent a small entry point and allowing it to retrieve more detailed information as needed.

### 16.1 Example

Instead of placing every requirement inside `AGENTS.md`:

```text
AGENTS.md
    |
    v
docs/index.md
    |
    +--> architecture.md
    +--> features.md
    +--> schema.md
    +--> testing.md
```

### 16.2 Example Instruction

```markdown
## Documentation

Read documentation relevant to the task.

- Architecture changes: docs/architecture.md
- Feature changes: docs/features.md
- Data changes: docs/schema.md
- Verification: docs/testing.md
```

### 16.3 Benefits

- Reduced context overhead
- Easier navigation
- Less duplicated information
- More focused analysis
- Simpler maintenance

### 16.4 Important Principle

**Provide a map to the information, not the entire information repository in every prompt.**

## 17. Documentation Index Files

An index file helps developers and agents locate relevant documentation.

### 17.1 Example

```text
docs/
├── index.md
├── overview.md
├── architecture.md
├── features.md
└── testing.md
```

### 17.2 Example docs/index.md

```markdown
# Documentation Index

## Project Information
- [Overview](overview.md)

## Technical Architecture
- [Architecture](architecture.md)

## Requirements
- [Features](features.md)

## Verification
- [Testing](testing.md)
```

### 17.3 Why Use an Index?

An index provides a centralized navigation point. It becomes especially useful when the documentation directory grows.

### 17.4 Is an Index Always Necessary?

No. A small `docs/` folder with three clearly named documents may not require a separate index.

## 18. Real-World Example — GroupTrack Android

### 18.1 Project Context

GroupTrack is an Android application for student project management and contribution tracking. The technology stack includes Android Studio, Java, XML layouts, Firebase, and Gradle.

### 18.2 Suggested Documentation Structure

```text
GroupTrack/
├── AGENTS.md
├── README.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── schema.md
│   ├── features.md
│   ├── coding-standards.md
│   ├── workflow.md
│   └── testing.md
└── app/
```

### 18.3 Document Responsibilities

| Document | GroupTrack responsibility |
|---|---|
| `overview.md` | Application purpose and MVP |
| `architecture.md` | Android application structure |
| `schema.md` | Firebase data model |
| `features.md` | Approved MVP requirements |
| `coding-standards.md` | Java and XML conventions |
| `workflow.md` | Development process |
| `testing.md` | Build and device validation |

### 18.4 Example Codex Prompt

“Read the applicable AGENTS.md instructions and the relevant feature and architecture documentation. Inspect the existing task management module and propose an implementation plan consistent with the approved MVP. Do not modify files.”

### 18.5 Expected Benefits

The agent can use the repository documentation to understand the MVP scope, identify relevant components, respect established architecture, avoid unnecessary dependencies, and plan verification.

### 18.6 Important Reminder

Do not create additional documentation merely because it appears in an example. Maintain the existing documentation structure when it already meets the project's needs.

## 19. Real-World Example — Laravel and Next.js

### 19.1 Project Context

An e-commerce system uses:

**Frontend**

- Next.js
- TypeScript
- Tailwind CSS

**Backend**

- Laravel
- PHP
- PostgreSQL

### 19.2 Suggested Structure

```text
ecommerce/
├── AGENTS.md
├── docs/
│   ├── overview.md
│   └── integration.md
├── frontend/
│   ├── AGENTS.md
│   └── docs/
│       ├── architecture.md
│       └── features.md
└── backend/
    ├── AGENTS.md
    └── docs/
        ├── architecture.md
        ├── api.md
        └── schema.md
```

This structure is suitable when frontend and backend documentation genuinely need separate ownership. For smaller projects, a single root `docs/` directory may be simpler.

### 19.3 Frontend Documentation

May describe route structure, component conventions, authentication UI, state management, and responsive behavior.

### 19.4 Backend Documentation

May describe API routes, controllers and services, validation, authentication, and database relationships.

### 19.5 Integration Documentation

May describe API base URL configuration, request and response contracts, authentication communication, and error handling conventions.

### 19.6 Example Codex Prompt

“Inspect the frontend and backend documentation relevant to product management. Identify the existing API contract and explain how the Next.js product page communicates with Laravel. Do not modify files.”

### 19.7 Why This Works

The agent can identify the responsibilities of each application layer without requiring a single oversized document.

## 20. Documentation Maintenance

Documentation is not a one-time setup activity. It must evolve with the project.

### 20.1 When to Update Documentation

Update relevant documentation when approved requirements change, architecture changes, database structures change, API contracts change, new development conventions are adopted, or existing instructions become inaccurate.

### 20.2 Example

A project initially uses local data storage. Later, the approved architecture changes to Firebase. The developer should review architecture documentation, data model documentation, relevant feature specifications, and testing procedures.

### 20.3 Documentation Drift

**Documentation Drift** occurs when written information no longer matches the current project. This can happen when code changes are implemented without updating relevant documentation.

### 20.4 Preventing Documentation Drift

- Update relevant docs alongside code changes.
- Review documentation during major refactoring.
- Avoid duplicating the same requirements.
- Remove obsolete instructions.
- Clearly label proposed and implemented behavior.
- Use Git to review documentation changes.

## 21. Documentation as Part of the Git Workflow

Documentation files should generally be version-controlled alongside project code when they belong to the repository.

### 21.1 Inspect Changes

```powershell
git status
```

### 21.2 Review Documentation Changes

```powershell
git diff -- docs/
```

### 21.3 Review Instruction Changes

```powershell
git diff -- AGENTS.md
```

### 21.4 Stage Documentation

```powershell
git add docs/ AGENTS.md
```

### 21.5 Commit Reviewed Changes

```powershell
git commit -m "docs: update project documentation"
```

Use these commands only after inspecting the intended changes.

### 21.6 Benefits

Version control provides change history, collaboration support, reviewable updates, and easier identification of outdated decisions. Git history does not automatically determine whether documentation is correct.

## 22. Common Documentation Mistakes

1. **Creating too many documents:** Excessive maintenance overhead. Start with the minimum useful documentation.
2. **Duplicating requirements:** Multiple copies may become inconsistent. Establish an authoritative source for each topic.
3. **Mixing instructions and specifications:** AGENTS.md becomes overloaded. Separate agent rules from detailed project knowledge.
4. **Writing vague requirements:** “The application should be user-friendly” is unclear. Define observable requirements and acceptance criteria.
5. **Documenting features that do not exist:** The agent may assume planned features are implemented. Label documents as proposed, approved, implemented, or deprecated where relevant.
6. **Outdated documentation:** The agent may follow obsolete information. Update documents when important decisions change.
7. **Poor naming conventions:** Documents become difficult to locate. Use descriptive and consistent filenames.
8. **No documentation navigation:** Relevant information is difficult to discover. Use AGENTS.md references or a documentation index.
9. **Treating documentation as proof:** Written specifications may not reflect actual behavior. Verify claims against code, tests, and runtime behavior.
10. **Overengineering the documentation system:** Documentation becomes more complex than the application itself. Keep the structure proportional to project requirements.

## 23. Hands-on Exercise A — Design a docs/ Folder

### Objective

Design a documentation structure for a small software project.

### Scenario

You are building an Android application using Java, XML, and Firebase. The application includes authentication and task management.

### Instructions

Create a proposed documentation structure containing only the files needed to describe project purpose, application architecture, feature requirements, data model, and testing procedures.

### Example

```text
docs/
├── overview.md
├── architecture.md
├── features.md
├── schema.md
└── testing.md
```

### Reflection Questions

1. Why does each document exist?
2. Which information belongs in AGENTS.md?
3. Which information belongs in features.md?
4. Which documents are authoritative for their respective topics?
5. Could any documents be combined without reducing clarity?

## 24. Hands-on Exercise B — Create a Documentation Workspace

### Objective

Practice creating a basic documentation directory using Windows PowerShell.

### Step 1 — Open PowerShell

Navigate to a disposable practice directory:

```powershell
cd $HOME\Documents
```

### Step 2 — Create the Project Folder

```powershell
New-Item -ItemType Directory -Path Codex-Docs-Practice -Force
```

### Step 3 — Navigate to the Project

```powershell
cd Codex-Docs-Practice
```

### Step 4 — Create the Documentation Directory

```powershell
New-Item -ItemType Directory -Path docs -Force
```

### Step 5 — Create Documentation Files

```powershell
New-Item -ItemType File -Path docs\overview.md
New-Item -ItemType File -Path docs\architecture.md
New-Item -ItemType File -Path docs\features.md
New-Item -ItemType File -Path docs\testing.md
```

### Step 6 — Inspect the Structure

```powershell
Get-ChildItem -Recurse
```

### Step 7 — Open in VS Code

```powershell
code .
```

### Step 8 — Add Basic Content

Populate each document with a short description of its responsibility.

### Expected Outcome

The learner can create and organize a basic project knowledge base.

## 25. Hands-on Exercise C — Documentation Review with Codex

### Objective

Use Codex to evaluate the organization of a practice documentation directory.

### Step 1 — Open the Practice Project

```powershell
cd $HOME\Documents\Codex-Docs-Practice
```

### Step 2 — Start Codex in Read-Only Mode

```powershell
codex --sandbox read-only
```

### Step 3 — Submit the Prompt

“Inspect the existing documentation directory. Explain the responsibility of each document, identify duplicated or missing information, and recommend only improvements that are necessary for a small project. Do not create or modify files.”

### Step 4 — Evaluate the Response

Check whether Codex identified the existing files correctly, understood their responsibilities, avoided unnecessary complexity, recommended relevant improvements, and respected read-only instructions.

### Step 5 — Reflection

1. Were the recommendations proportional to the project?
2. Did Codex suggest unnecessary documents?
3. Were any actual documentation problems identified?
4. Which recommendations would you accept?
5. Why is developer review still necessary?

## 26. Hands-on Exercise D — Source-of-Truth Analysis

### Objective

Identify and resolve conflicting documentation.

### Scenario

Three documents contain inconsistent user-role requirements.

**overview.md**

```markdown
Supported roles:
- Admin
- Student
```

**features.md**

```markdown
Supported roles:
- Admin
- Student
- Teacher
```

**AGENTS.md**

```markdown
Only Student users are supported.
```

### Instructions

1. Identify the conflicting information.
2. Determine which document should own role requirements.
3. Propose a corrected structure.
4. Rewrite AGENTS.md to reference the authoritative document.
5. Explain how the change reduces future contradictions.

### Expected Outcome

The learner understands why documentation responsibilities and authoritative sources must be clearly defined.

## 27. Knowledge Check

1. What is project documentation?
2. What is knowledge organization?
3. Why does documentation matter in agentic coding?
4. What is documentation architecture?
5. What is the single-source-of-truth principle?
6. How does AGENTS.md differ from project documentation?
7. What information belongs in an architecture document?
8. What information belongs in a feature specification?
9. Why are acceptance criteria important?
10. What is progressive disclosure?
11. What is documentation drift?
12. Why should documentation be version-controlled?
13. Why is more documentation not always better?
14. How should developers handle contradictions between documentation and source code?

## 28. Key Terminologies

| Term | Definition |
|---|---|
| Project documentation | Written knowledge describing a software project |
| Knowledge organization | Structuring information for retrieval and maintenance |
| Documentation architecture | Design of documentation structure and relationships |
| Single source of truth | Authoritative location for particular information |
| AGENTS.md | Repository instructions for coding agents |
| Feature specification | Document describing required feature behavior |
| Architecture document | Document explaining system structure |
| Data model | Representation of application entities and relationships |
| Acceptance criteria | Observable conditions defining successful implementation |
| Progressive disclosure | Providing relevant information in stages |
| Documentation drift | Mismatch between documentation and the current system |
| Documentation index | Navigation document linking supporting information |
| Context retrieval | Obtaining relevant information when needed |
| Non-goal | Explicitly excluded project objective |

## 29. Key Takeaways

1. **Project documentation provides persistent knowledge about software requirements and architecture.**
2. **Knowledge organization makes information easier to locate, understand, and maintain.**
3. **Documentation architecture defines the responsibilities and relationships of documents.**
4. **The single-source-of-truth principle reduces conflicting requirements.**
5. **AGENTS.md should guide Codex toward relevant documentation rather than duplicate everything.**
6. **Feature specifications should contain clear requirements and acceptance criteria.**
7. **Architecture documentation helps preserve component responsibilities.**
8. **Progressive disclosure improves context efficiency by retrieving information when needed.**
9. **Documentation must evolve alongside implementation changes.**
10. **The best documentation structure is the smallest one that reliably supports the project.**

## 30. Official References

### OpenAI

1. **OpenAI — Harness Engineering: Leveraging Codex in an Agent-First World**
   https://openai.com/index/harness-engineering/

   Explains repository knowledge organization, documentation navigation, and treating the repository as a system of record.

2. **OpenAI — Codex Documentation**
   https://developers.openai.com/codex/

3. **OpenAI — Custom Instructions with AGENTS.md**
   https://developers.openai.com/codex/guides/agents-md/

4. **OpenAI — Iterating Development Workflows with Codex**
   https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex

5. **OpenAI — Codex GitHub Repository**
   https://github.com/openai/codex

### Additional References

6. **Git — Documentation**
   https://git-scm.com/doc

7. **Markdown Guide — Basic Syntax**
   https://www.markdownguide.org/basic-syntax/

## 31. Lesson Completion Checklist

- [ ] Understand project documentation.
- [ ] Understand knowledge organization.
- [ ] Explain documentation architecture.
- [ ] Apply the single-source-of-truth principle.
- [ ] Differentiate AGENTS.md from project documentation.
- [ ] Design a practical docs/ directory.
- [ ] Understand project overview documentation.
- [ ] Understand architecture documentation.
- [ ] Understand feature specifications.
- [ ] Understand data model documentation.
- [ ] Understand coding standards and testing documents.
- [ ] Apply progressive disclosure.
- [ ] Recognize documentation drift.
- [ ] Understand documentation maintenance.
- [ ] Review the Android and Laravel examples.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[05 - Coding Standards and Architectural Constraints]]**

The next lesson explores how to define and enforce technical rules that guide Codex implementations.

Topics include:

- Coding conventions
- Architectural boundaries
- Separation of concerns
- Dependency policies
- Validation requirements
- Preventing architectural drift
- Real-world implementation constraints

---

*Phase 2 — Context Engineering | Lesson 2.4*
*Learning Principle: Keep project knowledge structured, authoritative, discoverable, and maintainable.*
