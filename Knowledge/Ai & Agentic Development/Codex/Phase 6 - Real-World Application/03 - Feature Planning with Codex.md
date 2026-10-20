# 03 — Feature Planning with Codex

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 6 — Real-World Application  
> **Lesson:** 6.3 — Feature Planning with Codex  
> **Difficulty:** Intermediate to Advanced  
> **Learning Mode:** Theory First  
> **Prerequisite:** [[02 - Project Setup and Context Preparation]]  
> **Next Lesson:** [[04 - Guided Feature Implementation]]  
> **Status:** Theory Documented

---

# 1. Introduction

After selecting a project, defining its MVP, preparing the repository, and establishing the development environment, the next stage is **feature planning**.

Feature planning transforms approved requirements into organized, actionable development work.

A project may have clear objectives but still be difficult to implement if its requirements are too broad.

For example:

> Build a student project management application.

This statement does not explain:

- Which feature should be implemented first.
- What the feature must accomplish.
- Which existing components are involved.
- What dependencies must be completed.
- What files may need modification.
- How the feature will be tested.
- What functionality must be excluded.

An AI coding agent such as Codex can help analyze these questions, but its planning must remain grounded in the approved project requirements.

## Core Principle

**Plan features around observable user outcomes, decompose them into verifiable tasks, and approve the implementation boundaries before allowing Codex to modify code.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Explain feature planning in software development.
2. Distinguish requirements, features, tasks, and implementation steps.
3. Analyze approved requirements.
4. Identify incomplete or ambiguous specifications.
5. Write user stories.
6. Define acceptance criteria.
7. Decompose features into smaller tasks.
8. Identify task dependencies.
9. Determine implementation order.
10. Understand vertical and horizontal task slicing.
11. Prioritize work based on value, risk, and dependencies.
12. Analyze affected files and modules.
13. Define implementation boundaries.
14. Prepare test plans.
15. Use Codex for read-only planning.
16. Evaluate Codex-generated plans.
17. Recognize unnecessary architecture changes.
18. Prevent scope creep.
19. Define human approval checkpoints.
20. Prepare implementation-ready task specifications.
21. Apply planning concepts to Android projects.
22. Apply planning concepts to Laravel projects.
23. Apply planning concepts to Next.js applications.
24. Understand planning handoffs.
25. Establish readiness for guided feature implementation.

---

# Part II — Understanding Feature Planning

## 3. What Is Feature Planning?

Feature planning is the process of converting product requirements into an organized sequence of development tasks.

```text
Project Objectives
        |
        v
Approved Requirements
        |
        v
Features
        |
        v
Development Tasks
        |
        v
Implementation Steps
        |
        v
Verification
```

| Level | Main Question |
|---|---|
| Objective | Why are we building this? |
| Requirement | What must the system do? |
| Feature | What capability delivers the requirement? |
| Task | What work is needed? |
| Implementation step | How will that work be performed? |
| Verification | How will we know it works? |

**Example:** Objective: Help students track academic tasks. Requirement: Users must be able to create tasks. Feature: Task Creation. Tasks: design the interface, implement validation and persistence, display success and error states, and test creation. Verification: valid tasks can be created and retrieved while invalid submissions are rejected.

## 4. Why Feature Planning Matters for Codex

Without a clear plan, Codex may make assumptions about architecture, database design, UI behavior, business rules, authorization, dependencies, testing, or future functionality. Assumptions may conflict with the project.

```text
Unplanned: Broad Request -> Codex Assumptions -> Large Changes -> Unexpected Complexity -> Difficult Verification

Planned: Approved Requirement -> Repository Inspection -> Feature Analysis -> Task Decomposition -> Plan Review -> Approved Implementation -> Focused Verification
```

**Codex should help determine how to implement approved requirements, not independently decide which product requirements should exist.**

---

# Part III — Requirements Analysis

## 5. What Is Requirements Analysis?

Requirements analysis examines specifications for meaning, completeness, consistency, and testability. Before planning, identify expected behavior, target users, inputs, outputs, business rules, permissions, error conditions, dependencies, and acceptance criteria.

## 6. Requirement Quality

| Characteristic | Meaning |
|---|---|
| Clear | Understandable without excessive interpretation |
| Specific | Describes identifiable behavior |
| Consistent | Does not conflict with other requirements |
| Feasible | Can realistically be implemented |
| Testable | Has observable outcomes |
| Necessary | Supports approved project objectives |
| Traceable | Can be linked to implementation and tests |

Weak: “The application should manage tasks efficiently.”

Improved: “FR-004: An authorized user shall be able to update the status of an assigned task using the supported status values.” This describes behavior that can be tested.

## 7. Identifying Ambiguity

“Users can edit tasks” leaves open who can edit which tasks, which fields are editable, whether completed tasks can be changed, whether changes are recorded, and how failures are handled. Codex should not silently invent answers that affect business rules or permissions.

| Classification | Meaning |
|---|---|
| Confirmed | Explicitly established in approved documentation |
| Inferred | Reasonably suggested by existing implementation |
| Assumed | Used provisionally but not verified |
| Unresolved | Requires clarification |
| Out of Scope | Explicitly excluded |

**An unresolved requirement is a planning issue, not permission to invent product behavior.**

---

# Part IV — User Stories

## 8. What Is a User Story?

A user story describes a desired capability from the user's perspective.

```text
As a [role],
I want [capability],
so that [benefit].
```

Example: “As a student, I want to view my assigned tasks so that I can identify the work I need to complete.” A story expresses value but may not contain all technical details.

## 9. User Story Components

| Component | Example |
|---|---|
| Role | Student |
| Capability | View assigned tasks |
| Benefit | Identify outstanding work |

Stories may also include a requirement ID, priority, business rules, acceptance criteria, dependencies, and open questions.

```markdown
## US-003 — View Assigned Tasks

### User Story
As a student,
I want to view my assigned tasks,
so that I can monitor outstanding work.

### Requirement
FR-003

### Priority
Must Have

### Acceptance Criteria
- Assigned tasks are displayed.
- Required task information is visible.
- An empty state appears when no tasks exist.

### Dependencies
- Authentication
- Task data access
```

---

# Part V — Acceptance Criteria

## 10. What Are Acceptance Criteria?

Acceptance criteria describe observable conditions that must be satisfied before a feature can be accepted. They establish a shared definition of successful behavior.

**Task Creation example:** an authorized user can open the interface; required fields are validated; valid data is saved; invalid data is rejected; appropriate feedback is shown; and the saved task can be retrieved.

## 11. Given–When–Then

```gherkin
Given an authorized user
When the user submits valid task information
Then the system saves the task
And the task appears in the task list
```

```gherkin
Given an authorized user
When the user submits a task without a required title
Then the system rejects the submission
And displays an appropriate validation message
```

```gherkin
Given a user without permission to edit a task
When the user attempts to update the task
Then the system rejects the operation
And the task remains unchanged
```

**Acceptance criteria should describe expected behavior without unnecessarily prescribing internal implementation details.**

---

# Part VI — Feature Decomposition

## 12. What Is Feature Decomposition?

Feature decomposition divides a large feature into smaller development tasks.

```text
Task Management Feature
          |
          +-- Task Creation
          +-- Task Listing
          +-- Task Details
          +-- Task Status Updates
```

## 13. Feature vs Task vs Subtask

| Level | Example |
|---|---|
| Feature | Task Creation |
| Task | Implement input validation |
| Subtask | Validate required title field |

A feature delivers a user-facing capability. A task describes work required to deliver it.

## 14. Task Decomposition Example

```text
Task Creation
     |
     +-- Inspect Existing Architecture
     +-- Identify Task Data Model
     +-- Prepare UI
     +-- Implement Validation
     +-- Implement Persistence
     +-- Handle Errors
     +-- Add Tests
     +-- Verify Acceptance Criteria
```

**Tasks should be small enough to understand, review, and verify without becoming unnecessarily fragmented.**

## 15. How Small Should a Task Be?

A task is appropriately scoped when its objective and output are clear, dependencies are known, changes are bounded, and completion is verifiable.

Too broad: “Build the entire administration system.”

Better: “Implement the approved product creation workflow, including validation, persistence, and required authorization checks.”

Tiny edits can be legitimate tasks, but splitting every trivial edit creates coordination overhead.

---

# Part VII — Vertical vs Horizontal Slicing

## 16. Horizontal Slicing

Horizontal slicing divides work by technical layer, such as frontend, backend, database, and testing. It can clarify specialist ownership and support parallel development, but a completed layer may not deliver a usable feature.

## 17. Vertical Slicing

A vertical slice delivers an end-to-end user capability, such as product creation across UI, API, validation, database, and tests. It supports incremental delivery and acceptance testing, but may cross components or repositories.

## 18. Choosing a Slicing Strategy

| Situation | Suitable Approach |
|---|---|
| Small user-facing feature | Vertical slice |
| Shared database migration | Technical task |
| UI redesign without backend changes | UI-focused slice |
| New API integration | Contract-first or vertical slice |
| Broad architectural migration | Phased technical work |

**Prefer independently verifiable user outcomes, while allowing technical tasks when they are necessary dependencies.**

---

# Part VIII — Task Dependencies

## 19. What Is a Task Dependency?

A dependency exists when one task requires another task, decision, or resource.

```text
Define Data Model -> Implement Persistence -> Connect User Interface -> Verify Complete Workflow
```

This is one possible sequence, not a universal requirement.

## 20. Dependency Types

- **Technical:** A component requires another component, such as a UI requiring an API endpoint.
- **Data:** A feature requires particular data, such as approved route coordinates for a map.
- **Decision:** A task requires stakeholder clarification, such as unresolved authorization rules.
- **External:** A feature requires an external service, such as Firebase configuration or mapping API access.

## 21. Dependency Graph

```text
        Authentication
              |
       +------+------+
       |             |
       v             v
   Task List     Task Creation
       |             |
       +------+------+
              |
              v
        Task Updates
              |
              v
        Final Testing
```

Derive dependencies from the actual architecture. Task creation and listing may be independently implementable if suitable contracts or fixtures already exist.

## 22. Critical Path Concept

The critical path is the longest dependency-constrained sequence that determines the minimum project duration under a given schedule model.

```text
Task A (2 days) -> Task B (3 days) -> Task D (2 days)
Total = 7 days
```

If an independent path takes four days, the seven-day path determines the modeled minimum. Estimates are uncertain; critical-path analysis helps planning but does not guarantee dates.

---

# Part IX — Implementation Sequencing

## 23. What Is Implementation Sequencing?

Implementation sequencing determines the order in which development tasks should be completed. Common factors include dependencies, risk, user value, complexity, availability of resources, and verification opportunities.

## 24. Dependency-First Sequencing

Complete prerequisites before dependent tasks.

```text
API Contract -> Backend Endpoint -> Frontend Integration -> End-to-End Verification
```

Frontend and backend work may proceed in parallel when a stable, approved API contract exists.

## 25. Risk-First Sequencing

Address uncertain or technically risky work early enough to find blockers before the deadline. For a GPS tracking application, verify location data availability, mapping integration feasibility, update behavior, and connectivity limits before spending most of the schedule polishing UI.

## 26. Value-First Sequencing

Prioritize functionality that delivers essential user value. For a task management MVP, that might be create task, view task, update status, and review progress; actual order depends on requirements and dependencies.

**Implementation order should be justified by dependencies, risk, and user value—not merely by what is easiest to code.**

---

# Part X — Prioritization Frameworks

## 27. MoSCoW Prioritization

MoSCoW classifies requirements as Must Have, Should Have, Could Have, or Won't Have for the current delivery.

| Feature | Priority |
|---|---|
| Task creation | Must Have |
| Task listing | Must Have |
| Task completion | Must Have |
| Advanced filtering | Could Have |
| AI recommendations | Won't Have |

Priorities should reflect stakeholder decisions, not assumptions generated by Codex.

## 28. Risk-Based Prioritization

High-priority work can reduce uncertainty rather than immediately produce visible functionality.

| Task | Value | Risk | Planning Priority |
|---|---|---|---|
| Verify Firebase integration | High | High | Early |
| Implement core task workflow | High | Medium | Early |
| Add optional animations | Low | Low | Later |
| Add advanced analytics | Outside MVP | Medium | Exclude |

## 29. Effort vs Value Matrix

```text
                    HIGH VALUE
                        |
        Quick Wins      |     Major Investments
                        |
LOW EFFORT -------------+------------- HIGH EFFORT
                        |
        Small Tasks     |     Low-Value Complexity
                        |
                    LOW VALUE
```

High-value, low-effort tasks are often good early candidates; high-value, high-effort tasks need planning; low-value, high-effort tasks are usually deferred or excluded. Dependencies and risk may override this ranking.

---

# Part XI — File Impact Analysis

## 30. What Is File Impact Analysis?

File impact analysis identifies codebase areas that may need changes. Ask where behavior originates, which components depend on it, which files own business rules, which interfaces or tests may change, and what documentation needs updating.

## 31. Conceptual File Impact Map

```text
Feature Requirement
         |
         v
Existing Entry Point
         |
         v
Relevant Components
         |
         +-- UI
         +-- Business Logic
         +-- Data Access
         +-- Configuration
         +-- Tests
```

Identify likely impact during planning, then confirm actual paths through repository inspection.

## 32. Example — Laravel Feature

For validation on an existing product creation endpoint, relevant components may include the route, controller, Form Request or validation logic, model, an existing service that owns the behavior, and feature tests.

Avoid rewriting unrelated inventory logic, introducing a new architecture, renaming unrelated models, or adding unnecessary packages.

## 33. Example — Android Feature

For a sign-in screen interface, relevant components may include the XML layout, Java Activity or Fragment, drawable and string resources, navigation integration, and UI tests. If the task is explicitly UI-only, backend authentication changes remain out of scope.

## 34. Example — Next.js Feature

For displaying products from the existing Laravel API, relevant components may include the product page, API client, TypeScript types, loading and error states, and component tests.

**Do not expand a frontend integration task into a backend redesign unless approved requirements or evidence demand it.**

---

# Part XII — Planning with Codex

## 35. Codex as a Planning Assistant

Codex can explore code, identify modules, analyze requirements, propose sequences, identify dependencies, suggest verification, identify risks, and review a plan. Evaluate recommendations against the repository and approved scope.

## 36. Read-Only Planning Workflow

```text
Provide Approved Requirement
             |
             v
Read AGENTS.md
             |
             v
Inspect Relevant Documentation
             |
             v
Inspect Existing Source
             |
             v
Identify Dependencies
             |
             v
Propose Implementation Plan
             |
             v
Human Review
             |
             v
Approve / Revise / Reject
```

Separate planning from implementation when a task is ambiguous, high-risk, or likely to affect multiple components.

## 37. Codex Planning Prompt Template

```text
Task:
Prepare an implementation plan for the
approved feature described below.

Feature:
[Feature name]

Requirement:
[Requirement ID and description]

Acceptance Criteria:
[Approved acceptance criteria]

Instructions:
1. Read applicable AGENTS.md instructions.
2. Inspect relevant project documentation.
3. Inspect the existing implementation.
4. Identify dependencies and affected modules.
5. Decompose the feature into focused tasks.
6. Recommend an implementation sequence.
7. Identify required tests.
8. Identify technical risks.
9. Highlight unresolved requirements.
10. Identify potential overengineering.

Constraints:
- Do not modify files.
- Do not install dependencies.
- Do not invent business rules.
- Do not add unrelated features.
- Preserve existing architecture.

Output:
- Confirmed findings
- Assumptions
- Dependencies
- Proposed tasks
- Likely affected files
- Verification plan
- Risks
- Unresolved questions
- Approval checklist
```

---

# Part XIII — Evaluating Codex Plans

## 38. What Makes a Good Plan?

A good plan is grounded in repository evidence, consistent with approved requirements, reviewable, explicit about dependencies and affected components, verifiable, honest about unknowns, and free from unnecessary changes.

## 39. Plan Review Checklist

- [ ] Does it satisfy the approved requirement?
- [ ] Are acceptance criteria preserved?
- [ ] Are assumptions clearly identified?
- [ ] Are dependencies accurate?
- [ ] Does it follow existing architecture?
- [ ] Are likely file changes relevant?
- [ ] Are unnecessary libraries avoided?
- [ ] Are tests included?
- [ ] Are authorization rules considered?
- [ ] Are external operations identified?
- [ ] Is the task sequence reasonable?
- [ ] Does the plan stop at the approved feature?

## 40. Warning Signs

- **Unnecessary rewrite:** replacing authentication architecture for a UI-only task.
- **Speculative features:** adding notifications, analytics, and recommendations without approval.
- **Unsupported assumptions:** assuming administrators can edit all records without an established permission rule.
- **Missing verification:** saying “confirm it works” without identifying how.
- **Excessive file impact:** a small change unexpectedly touching many unrelated files.

Investigate warning signs before approving the plan.

---

# Part XIV — Planning Artifacts

## 41. What Is a Planning Artifact?

A planning artifact records development intentions, constraints, and verification expectations. Examples include feature specifications, task breakdowns, implementation plans, dependency maps, test plans, and risk registers.

Not every project needs a separate file for each artifact. Follow existing documentation conventions.

## 42. Suggested Feature Plan Template

```markdown
# Feature Plan — [Feature Name]

## 1. Feature Information
Feature ID:
Requirement ID:
Priority:
Status:

## 2. Objective
Describe the intended user outcome.

## 3. Approved Scope
List included functionality.

## 4. Non-Goals
List explicitly excluded functionality.

## 5. Existing Architecture
Summarize relevant implementation findings.

## 6. Dependencies
List required components and decisions.

## 7. Task Breakdown
| ID | Task | Dependency | Verification |
|---|---|---|---|

## 8. Likely Affected Files
List confirmed or expected file paths.

## 9. Acceptance Criteria
Define observable success conditions.

## 10. Test Plan
Describe relevant verification.

## 11. Risks
Identify technical and operational risks.

## 12. Unresolved Questions
List decisions requiring clarification.

## 13. Approval
Approved / Needs Revision / Blocked
```

## 43. PLANS.md

A project may use `PLANS.md` to record development phases, feature sequence, dependencies, milestones, acceptance criteria, and approval boundaries.

```markdown
# Development Plan

## Phase 1 — Foundation
- Verify repository setup.
- Confirm development environment.

## Phase 2 — Core Feature
- Implement approved task creation workflow.
- Verify acceptance criteria.

## Phase 3 — Integration
- Connect approved components.
- Run integration tests.

## Phase 4 — Review
- Inspect changes.
- Record verification results.
```

`PLANS.md` is an optional workflow convention, not a mandatory Codex file. OpenAI development workflow examples describe planning artifacts and smaller phases to support reviewable, human-gated implementation.

---

# Part XV — Test Planning

## 44. What Is a Test Plan?

A test plan describes how developers will verify that implementation satisfies requirements. It may include test objective, scope, environment, scenarios, expected results, tools, dependencies, and known limitations.

## 45. Test Planning Example

**Feature:** Task Creation

| Test | Expected Result |
|---|---|
| Submit valid task | Task is saved |
| Submit empty title | Validation error |
| Submit unauthorized request | Request rejected |
| Retrieve saved task | Stored data returned |
| Simulate storage failure | Appropriate error handling |

Include successful and unsuccessful scenarios.

## 46. Test Pyramid Concept

```text
           /\
          /  \
         / E2E\
        /------\
       /Integration\
      /------------\
     /  Unit Tests  \
    /----------------\
```

This illustrates a common strategy with many focused tests and fewer broad end-to-end tests. Distribution depends on the project.

**Use the smallest reliable test that verifies the behavior, while retaining integration and end-to-end coverage for critical workflows.**

---

# Part XVI — Planning for Failure

## 47. Error Scenarios

Feature plans should consider invalid input, missing data, unauthorized actions, network failure, database failure, external API failure, unexpected state, and conflicting updates.

```text
User Submits Product
          |
          v
Validate Input
      /       \
   Valid     Invalid
     |          |
     v          v
Authorize    Show Error
     |
     v
Save Product
    /      \
Success   Failure
   |         |
   v         v
Confirm   Report Error
```

**A complete feature plan considers failure behavior, not only the happy path.**

---

# Part XVII — Avoiding Overengineering

## 48. Overengineering During Planning

Overengineering can begin before code is written: planning microservices for a small application, adding abstraction layers without requirements, designing a plugin system for one feature, introducing complex background processing unnecessarily, or adding libraries for functionality already supported by the framework.

## 49. Scope Control Questions

1. Which requirement does this task satisfy?
2. Is it necessary for the MVP?
3. Can existing code support it?
4. Does it introduce new dependencies?
5. Does it increase maintenance complexity?
6. Can it be implemented more simply?
7. Can it be postponed without losing core functionality?

**If a task cannot be traced to an approved requirement, necessary dependency, or quality safeguard, question its inclusion.**

---

# Part XVIII — Android Example: GroupTrack

## 50. Project Context

GroupTrack is a native Android project using Android Studio, Java, XML layouts, and Firebase. This example assumes an approved task limited to preparing a sign-in interface; it does not redefine the project's existing MVP.

## 51. Feature Planning Example

**Feature:** Sign-in Screen UI.  
**Objective:** Provide the visual interface through which a user can enter credentials.

Illustrative scope: email and password inputs, sign-in button, navigation to account creation, and appropriate layout behavior.

Non-goals: replacing authentication architecture, adding social login, implementing unrelated dashboard features, or introducing unnecessary UI libraries.

## 52. Task Breakdown

| Task | Description | Verification |
|---|---|---|
| GT-UI-01 | Inspect existing resources | Existing conventions identified |
| GT-UI-02 | Prepare sign-in XML layout | Layout renders |
| GT-UI-03 | Connect existing navigation | Expected destination opens |
| GT-UI-04 | Review responsive behavior | Layout usable on supported screens |
| GT-UI-05 | Verify build | Build succeeds |

Dependencies may include the existing Activity or Fragment, navigation structure, drawable and string resources, and theme conventions.

## 53. Android Codex Planning Prompt

```text
Read AGENTS.md and relevant GroupTrack docs.

Task:
Plan the approved sign-in UI implementation.

Technology:
- Java
- XML layouts
- Existing Android project

Constraints:
- UI scope only.
- Preserve the approved MVP.
- Follow existing architecture.
- Do not add UI libraries.
- Do not implement unrelated authentication logic.
- Do not modify files.

Return:
1. Relevant existing files.
2. Proposed UI tasks.
3. Navigation dependencies.
4. Resource requirements.
5. Verification approach.
6. Potential risks.
7. Unresolved questions.
```

A UI feature should not automatically expand into a full authentication-system rewrite.

---

# Part XIX — Laravel Example: GoPasig

## 54. Project Context

GoPasig is a Laravel-based fleet management and GPS bus tracking system. Its domain includes routes, stops, buses, drivers, schedules, trips, GPS tracking, ETA computation, and administrative monitoring. This example considers a hypothetical approved schedule validation change.

## 55. Feature Requirement

> Prevent assigning a driver to conflicting schedules under the approved scheduling rules.

The exact definition of a scheduling conflict must come from actual business rules. Codex should not invent buffer times, working-hour limits, or assignment restrictions.

## 56. Planning Considerations

- Where is schedule validation currently implemented?
- Are transactions used?
- Are concurrent assignments possible?
- Which schedule states count as conflicts?
- Are business rules documented?
- Which tests cover overlapping assignments?

Relevant areas may include the schedule model, driver assignments, validation, existing services, database constraints, and feature tests.

## 57. Task Breakdown

| Task | Description |
|---|---|
| GP-01 | Inspect existing scheduling logic |
| GP-02 | Confirm conflict rules |
| GP-03 | Identify affected services and models |
| GP-04 | Define validation test cases |
| GP-05 | Plan minimal implementation changes |
| GP-06 | Plan regression verification |

For systems with complex business rules, planning should begin by understanding existing logic rather than replacing it.

---

# Part XX — Next.js and Laravel Example: E-commerce

## 58. Project Context

```text
Next.js Frontend
        |
        v
Laravel REST API
        |
        v
PostgreSQL
```

Suppose the approved feature is: “Display products from the existing backend API.”

## 59. Feature Planning

**Frontend responsibilities:** fetch product data, display it, handle loading and error states, and respect the existing API contract.

**Backend responsibilities:** provide approved product data, enforce authorization where applicable, and return consistent response formats.

**Shared dependency:** API contract.

## 60. Dependency Map

```text
Approved API Contract
         |
         +----------------+
         |                |
         v                v
Backend Endpoint     Frontend Types
         |                |
         +--------+-------+
                  |
                  v
           UI Integration
                  |
                  v
         Integration Testing
```

If the endpoint already exists and meets requirements, backend modification may not be necessary.

## 61. Example Task Breakdown

| Task | Scope | Verification |
|---|---|---|
| EC-01 | Inspect products endpoint | Contract identified |
| EC-02 | Define frontend data types | Types match contract |
| EC-03 | Implement product fetching | Data returned correctly |
| EC-04 | Render product list | Expected fields displayed |
| EC-05 | Handle loading and errors | UI states verified |
| EC-06 | Run integration checks | Frontend/backend interaction works |

## 62. Cross-Repository Planning

1. Identify the repository responsible for each change.
2. Confirm API contract ownership.
3. Determine whether both repositories require modification.
4. Identify integration test responsibilities.
5. Define separate verification commands.
6. Coordinate changes affecting shared interfaces.

**Do not modify both repositories merely because a feature involves both systems conceptually. Change only components that actually require changes.**

---

# Part XXI — Planning Handoffs

## 63. What Is a Planning Handoff?

A planning handoff transfers an approved feature plan into an implementation session. It should include the feature objective, approved scope, non-goals, relevant documentation, task sequence, dependencies, acceptance criteria, verification commands, and unresolved questions.

## 64. Handoff Template

```markdown
# Feature Implementation Handoff

## Feature
[Name]

## Requirement
[ID and description]

## Approved Scope
[Included functionality]

## Non-Goals
[Excluded functionality]

## Relevant Documentation
[Paths]

## Relevant Modules
[Confirmed paths]

## Tasks
[Ordered tasks]

## Acceptance Criteria
[Observable conditions]

## Verification
[Tests and commands]

## Risks
[Known risks]

## Approval Status
Approved / Pending / Blocked
```

**An implementation agent should understand the approved task without relying on undocumented assumptions from earlier conversations.**

---

# Part XXII — Planning Readiness

## 65. Definition of Ready

Definition of Ready describes conditions that should be satisfied before implementation begins.

- [ ] Requirement is approved.
- [ ] Feature objective is clear.
- [ ] Scope boundaries are defined.
- [ ] Acceptance criteria are testable.
- [ ] Relevant dependencies are identified.
- [ ] Critical decisions are resolved.
- [ ] Existing architecture has been inspected.
- [ ] Verification approach is defined.
- [ ] Necessary access and resources are available.
- [ ] Implementation approval is recorded.

This is a planning practice, not a mandatory universal standard. Avoid bureaucracy for trivial changes.

---

# Part XXIII — Common Mistakes

## 66. Planning Without Reading Existing Code

**Problem:** The plan duplicates or conflicts with existing functionality.  
**Improvement:** Inspect relevant code before proposing changes.

## 67. Treating Broad Features as Single Tasks

**Problem:** Implementation becomes difficult to review.  
**Improvement:** Divide work into meaningful, verifiable slices.

## 68. Inventing Business Rules

**Problem:** Implementation may violate stakeholder expectations.  
**Improvement:** Mark unresolved decisions and request clarification.

## 69. Ignoring Dependencies

**Problem:** Work begins before prerequisites are available.  
**Improvement:** Identify and sequence dependencies.

## 70. Planning Only the Happy Path

**Problem:** Failure scenarios are omitted.  
**Improvement:** Include validation, authorization, and error cases.

## 71. Excessive Task Fragmentation

**Problem:** Coordination overhead increases without improving quality.  
**Improvement:** Keep tasks small but meaningful.

## 72. Planning Unnecessary Architecture Changes

**Problem:** Complexity grows beyond MVP needs.  
**Improvement:** Preserve existing architecture unless change is justified.

## 73. Missing Test Plans

**Problem:** Completion cannot be verified objectively.  
**Improvement:** Connect acceptance criteria to tests.

## 74. Automatically Executing the Plan

**Problem:** Proposed changes begin before review.  
**Improvement:** Require approval for implementation when appropriate.

---

# Part XXIV — Practical Exercises

> These exercises are optional. The current learning objective remains theoretical understanding.

## 75. Exercise A — Requirement Analysis

Select one approved functional requirement. Identify the user role, expected behavior, inputs, outputs, business rules, error conditions, dependencies, and unresolved questions.

**Deliverable:** Requirements analysis report.

## 76. Exercise B — User Stories

Write three user stories. For each, include role, capability, benefit, requirement ID, and priority.

**Deliverable:** User story table.

## 77. Exercise C — Acceptance Criteria

Create Given–When–Then scenarios for successful operation, invalid input, unauthorized action, and external service failure where relevant.

**Deliverable:** Acceptance criteria document.

## 78. Exercise D — Feature Decomposition

Choose a feature and break it into tasks. For each task, identify objective, dependencies, expected output, and verification.

**Deliverable:** Task breakdown.

## 79. Exercise E — Dependency Mapping

Create a dependency graph. Identify prerequisites, independent tasks, external resources, and stakeholder decisions.

**Deliverable:** Dependency diagram.

## 80. Exercise F — Prioritization

Classify features using MoSCoW, then evaluate user value, risk, effort, and dependencies.

**Deliverable:** Prioritized feature backlog.

## 81. Exercise G — File Impact Analysis

Inspect a hypothetical request. Identify likely entry points, relevant modules, shared interfaces, tests, documentation, and unrelated components that should remain unchanged.

**Deliverable:** File impact report.

## 82. Exercise H — Codex Planning Prompt

Write a planning-only Codex prompt that reads `AGENTS.md`, inspects existing code, respects MVP boundaries, identifies dependencies, proposes tasks and verification, and does not modify files.

**Deliverable:** Reusable planning prompt.

## 83. Exercise I — Plan Review

Review a hypothetical Codex plan introducing dependencies, modules, unrequested features, and unrelated refactoring. Identify justified changes and what should be removed.

**Deliverable:** Plan review report.

## 84. Exercise J — Android Feature Plan

Prepare a UI-only GroupTrack screen plan covering XML, Java component, resources, navigation, testing, and non-goals.

**Deliverable:** Android feature plan.

## 85. Exercise K — Laravel Feature Plan

Plan validation for a scheduling operation, including business rules, models, services, transactions if needed, authorization, and tests.

**Deliverable:** Laravel feature plan.

## 86. Exercise L — Next.js/Laravel Integration Plan

Plan displaying backend data in Next.js. Include API contract, TypeScript types, data fetching, UI states, backend dependencies, and integration tests.

**Deliverable:** Full-stack feature plan.

## 87. Exercise M — Implementation Handoff

Convert a feature plan into a handoff with approved objective, scope, non-goals, tasks, dependencies, acceptance criteria, verification, and approval status.

**Deliverable:** Implementation-ready handoff.

---

# Part XXV — Knowledge Review

## 88. Knowledge Check

1. What is feature planning?
2. How does feature planning differ from project scope definition?
3. What is the relationship between objectives, requirements, features, and tasks?
4. Why should Codex inspect existing code before planning?
5. What makes a requirement testable?
6. What is a user story?
7. What are acceptance criteria?
8. What does Given–When–Then mean?
9. What is feature decomposition?
10. How does a feature differ from a task?
11. What is horizontal slicing?
12. What is vertical slicing?
13. When is vertical slicing useful?
14. What is a task dependency?
15. What types of dependencies exist?
16. What is a dependency graph?
17. What is the critical path?
18. What is implementation sequencing?
19. What is risk-first prioritization?
20. What is value-first prioritization?
21. What does MoSCoW mean?
22. What is file impact analysis?
23. Why should likely affected files be identified?
24. What is read-only planning?
25. Why should Codex-generated plans be reviewed?
26. What are common warning signs of overengineering?
27. What is a planning artifact?
28. Is `PLANS.md` mandatory for Codex?
29. What is a test plan?
30. Why should error scenarios be included?
31. What is a planning handoff?
32. What is Definition of Ready?
33. Why should critical unresolved requirements block implementation?
34. How can a UI-only task accidentally expand in scope?
35. Why must Laravel business rules be verified?
36. Why do Next.js and Laravel require API contract coordination?
37. When should cross-repository modifications be avoided?
38. What makes a feature plan implementation-ready?

---

## 89. Key Terminologies

| Term | Definition |
|---|---|
| Feature Planning | Organizing requirements into actionable development work |
| Requirement Analysis | Examining requirements for clarity and feasibility |
| Feature | Capability delivering user value |
| Task | Unit of development work |
| Subtask | Smaller part of a task |
| User Story | User-centered capability statement |
| Acceptance Criteria | Observable conditions for acceptance |
| Given–When–Then | Structured behavior description |
| Feature Decomposition | Breaking features into smaller tasks |
| Vertical Slice | End-to-end functional capability |
| Horizontal Slice | Work divided by technical layer |
| Dependency | Prerequisite or required resource |
| Dependency Graph | Representation of task relationships |
| Critical Path | Longest dependency-constrained project path |
| Implementation Sequence | Planned order of development |
| MoSCoW | Must, Should, Could, Won't prioritization |
| Risk-Based Prioritization | Ordering work based on uncertainty and impact |
| File Impact Analysis | Identifying affected codebase components |
| Planning Artifact | Document recording development intentions |
| `PLANS.md` | Optional repository planning document |
| Test Plan | Defined verification strategy |
| Scope Creep | Uncontrolled expansion of requirements |
| Non-Goal | Explicitly excluded outcome |
| Planning Handoff | Transfer of approved planning context |
| Definition of Ready | Conditions for beginning implementation |
| Approval Gate | Required authorization before proceeding |
| Traceability | Linking requirements to tasks and tests |

---

## 90. Key Takeaways

1. Feature planning connects requirements to implementation.
2. Codex should work from approved requirements.
3. Repository inspection should precede implementation planning.
4. Requirements must be clear and testable.
5. Ambiguous business rules should not be silently invented.
6. User stories describe user value.
7. Acceptance criteria define observable success.
8. Feature decomposition makes work manageable.
9. Tasks should be independently reviewable where practical.
10. Vertical slices support incremental user value.
11. Technical dependencies must be identified.
12. Implementation order should reflect dependencies and risk.
13. MoSCoW helps establish scope priorities.
14. File impact analysis reduces unrelated changes.
15. Codex can assist with planning without modifying code.
16. Generated plans require evaluation.
17. Planning artifacts should follow existing repository conventions.
18. Test planning belongs in feature planning.
19. Failure scenarios are part of complete behavior.
20. Overengineering can begin during planning.
21. UI-only work should remain UI-only unless scope changes are approved.
22. Complex Laravel business rules require careful analysis.
23. Full-stack changes require API contract coordination.
24. A planning handoff should preserve scope and verification expectations.
25. Implementation should begin only when the task is sufficiently understood and authorized.

---

# 91. Official References

## OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
3. [Using Goals in Codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)
4. [Using PLANS.md for Multi-Hour Problem Solving](https://developers.openai.com/cookbook/articles/codex_exec_plans)
5. [Codex Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide)

> Note: The older PLANS.md cookbook article is archived. Treat its planning structure as an example rather than a requirement or a guarantee of current CLI behavior.

## Software Engineering

6. [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
7. [Scrum Guide](https://scrumguides.org/scrum-guide.html)
8. [Agile Manifesto](https://agilemanifesto.org/)
9. [Git Documentation](https://git-scm.com/docs)

## Android

10. [Android Developers](https://developer.android.com/)
11. [Android Testing](https://developer.android.com/training/testing)

## Laravel

12. [Laravel Documentation](https://laravel.com/docs)
13. [Laravel Testing](https://laravel.com/docs/testing)

## Next.js

14. [Next.js Documentation](https://nextjs.org/docs)
15. [TypeScript Documentation](https://www.typescriptlang.org/docs/)

## Security and Verification

16. [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
17. [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)

---

# 92. Lesson Completion Checklist

- [ ] Understand feature planning.
- [ ] Distinguish objectives, requirements, features, and tasks.
- [ ] Analyze approved requirements and identify ambiguities.
- [ ] Write user stories and define acceptance criteria.
- [ ] Understand Given–When–Then.
- [ ] Decompose features into tasks.
- [ ] Understand vertical and horizontal slicing.
- [ ] Identify dependencies and create dependency graphs.
- [ ] Understand critical paths and sequence implementation tasks.
- [ ] Apply MoSCoW and risk-first prioritization.
- [ ] Perform file impact analysis.
- [ ] Understand read-only Codex planning.
- [ ] Write scoped planning prompts.
- [ ] Review Codex-generated plans and identify overengineering.
- [ ] Understand planning artifacts and optional `PLANS.md` workflows.
- [ ] Prepare test plans and include error scenarios.
- [ ] Understand planning handoffs and Definition of Ready.
- [ ] Evaluate Android, Laravel, and Next.js/Laravel plans.
- [ ] Complete optional exercises.
- [ ] Answer knowledge-check questions.

---

# 93. Next Lesson

## [[04 - Guided Feature Implementation]]

The next lesson focuses on executing an approved feature plan with Codex while preserving architecture, scope, and verification standards.

### Topics

- Transitioning from planning to implementation.
- Pre-implementation repository inspection.
- Task-by-task execution.
- Scoped Codex prompts.
- Incremental code changes.
- Following architectural boundaries.
- Handling dependencies.
- Test-driven workflows.
- Build and test verification.
- Reviewing diffs.
- Error recovery.
- Human approval checkpoints.
- Android, Laravel, and Next.js examples.
- Preventing scope drift.

### Learning Progression

```text
Lesson 6.1 Project Selection and Scope Definition
        |
        v
Lesson 6.2 Project Setup and Context Preparation
        |
        v
Lesson 6.3 Feature Planning with Codex
        |
        v
Lesson 6.4 Guided Feature Implementation
        |
        v
Lesson 6.5 Debugging, Testing, and Quality Assurance
        |
        v
Lesson 6.6 Code Review and Git Workflow
        |
        v
Lesson 6.7 Final Project Evaluation
```

---

*Phase 6 — Real-World Application | Lesson 6.3*  
*Learning Principle: Transform approved requirements into small, dependency-aware, testable tasks before delegating implementation to Codex.*
