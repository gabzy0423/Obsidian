# 03 — Planning Before Implementation

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 3 — Agentic Coding Workflow  
> **Lesson:** 3.3 — Planning Before Implementation  
> **Difficulty:** Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[02 - Codebase Exploration and Analysis]]  
> **Learning Approach:** Theory, planning frameworks, real-world examples, and practical exercises

---

## 1. Introduction

One of the most common mistakes in AI-assisted software development is allowing an agent to begin modifying code before the task has been properly understood.

A developer may provide a short instruction such as:

> Implement task management.

Although the objective sounds straightforward, the instruction leaves many important questions unanswered.

For example:

- Which task management features are required?
- Which user roles can create tasks?
- How are tasks stored?
- Which architectural conventions should be followed?
- What existing functionality must remain unchanged?
- Which files need modification?
- How will the implementation be verified?

An AI coding agent may attempt to resolve these uncertainties independently.

However, assumptions made during implementation can lead to unnecessary features, incorrect architectural decisions, and changes outside the approved scope.

**Planning Before Implementation** addresses this problem.

It is the process of transforming requirements and codebase findings into a structured, reviewable approach before modifying source code.

In agentic software development, planning provides a connection between understanding the existing system and implementing the requested changes.

**Core Principle: A good implementation plan explains what will change, why it must change, how the change will be performed, and how success will be verified.**

---

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Explain the purpose of implementation planning.
2. Distinguish exploration, planning, and implementation.
3. Analyze functional and non-functional requirements.
4. Identify missing or ambiguous requirements.
5. Define implementation scope and non-goals.
6. Create file-level change plans.
7. Evaluate dependencies and architectural constraints.
8. Perform basic risk assessment.
9. Write measurable acceptance criteria.
10. Establish approval checkpoints.
11. Sequence implementation tasks logically.
12. Create verification and recovery plans.
13. Use Codex CLI for planning-only workflows.
14. Apply implementation planning to Android and Laravel projects.
15. Review and approve an implementation plan before coding.

---

## 3. What Is Implementation Planning?

**Implementation Planning** is the process of deciding how a software change should be performed before making that change.

It translates requirements into actionable development steps.

A useful plan identifies:

- The intended outcome.
- The current implementation.
- The relevant requirements.
- The expected affected files.
- The proposed technical approach.
- Architectural constraints.
- Potential risks.
- Acceptance criteria.
- Verification procedures.
- Approval requirements.

### 3.1 Example

Requirement:

> Improve the alignment of the Android login screen.

A basic implementation plan might contain:

```text
Objective:
Improve spacing and alignment.

Affected Components:
Existing login XML layout.

Proposed Changes:
Adjust constraints, margins, and padding.

Constraints:
Preserve view IDs and authentication logic.

Verification:
Build the application and inspect the UI.

Completion:
Alignment requirements are satisfied.
```

### 3.2 Why Planning Matters

Planning reduces uncertainty before implementation.

It also gives the developer an opportunity to reject unnecessary changes before they are introduced into the codebase.

---

## 4. Exploration vs Planning vs Implementation

These stages have different responsibilities.

| Stage | Main Question | Expected Output |
|---|---|---|
| Exploration | What currently exists? | Codebase findings |
| Planning | What should change? | Implementation plan |
| Implementation | How do we make the approved change? | Modified source code |
| Verification | Does the change satisfy the requirements? | Test results and evidence |

### 4.1 Example

A Laravel endpoint rejects valid registration requests.

**Exploration**

Inspect routes, validation, controller logic, database constraints, and tests.

**Planning**

Identify the root cause and propose the smallest necessary correction.

**Implementation**

Apply the approved correction.

**Verification**

Execute relevant tests and review the changed files.

### 4.2 Key Principle

**Exploration produces understanding. Planning produces decisions. Implementation produces changes.**

---

## 5. Why Planning Is Important in Agentic Coding

AI coding agents can perform multiple development operations within a single task.

Without planning, an agent may:

- Modify unnecessary files.
- Introduce new dependencies.
- Duplicate existing functionality.
- Change established architecture.
- Misinterpret requirements.
- Skip important validation.
- Continue beyond the approved scope.

A reviewed implementation plan helps prevent these problems.

### 5.1 Benefits

**Clarity**

The expected outcome is defined before coding.

**Predictability**

The developer understands the proposed changes.

**Architectural Consistency**

The agent can follow existing component responsibilities.

**Risk Reduction**

Potential problems are identified early.

**Reviewability**

The plan provides a reference for evaluating the final implementation.

**Verification**

Acceptance criteria are connected to observable evidence.

### 5.2 Important Limitation

A plan does not guarantee successful implementation.

New findings may require the plan to be revised.

The agent should report significant deviations rather than silently changing the approved approach.

---

## 6. When Should You Create a Plan?

Not every task requires a detailed planning document.

Planning effort should be proportional to task complexity.

### 6.1 Low-Complexity Tasks

Examples:

- Correct a typo.
- Update a static label.
- Adjust a simple margin.
- Rename a local variable.

These tasks may require only a brief understanding of the affected code.

### 6.2 Medium-Complexity Tasks

Examples:

- Add a form.
- Implement an API endpoint.
- Fix a validation bug.
- Update a database query.
- Add a new Android screen.

These usually benefit from a concise implementation plan.

### 6.3 High-Complexity Tasks

Examples:

- Change authentication architecture.
- Introduce a new module.
- Modify multiple related database entities.
- Refactor a major subsystem.
- Integrate several external services.

These may require a detailed, reviewable plan with milestones and recovery steps.

### 6.4 Planning Decision Table

| Task Complexity | Recommended Planning |
|---|---|
| Low | Brief analysis |
| Medium | Structured task plan |
| High | Detailed plan with checkpoints |

### 6.5 Key Principle

**Plan enough to control risk without turning simple changes into unnecessary documentation work.**

---

## 7. Stage 1 — Requirements Analysis

**Requirements Analysis** identifies what the system must accomplish and the constraints under which it must operate.

Before implementation, Codex should understand the intended behavior.

### 7.1 Functional Requirements

Functional requirements describe what a system should do.

Examples:

- Users can create tasks.
- Users can sign in.
- Administrators can assign schedules.
- An API can return filtered products.

### 7.2 Non-Functional Requirements

Non-functional requirements describe qualities or constraints.

Examples:

- Preserve existing security controls.
- Maintain acceptable response times.
- Support required screen sizes.
- Follow existing architecture.
- Avoid unnecessary dependencies.

### 7.3 Example — Android

Task:

> Implement a task creation form.

Functional requirements:

- Accept a task title.
- Accept a description.
- Validate required fields.
- Save the task using the existing data layer.
- Display success or error feedback.

Non-functional requirements:

- Use Java and XML.
- Follow existing UI conventions.
- Preserve navigation behavior.
- Avoid additional libraries.

### 7.4 Important Principle

**Requirements should describe intended behavior without unnecessarily dictating implementation details.**

---

## 8. Identifying Ambiguous Requirements

A requirement is ambiguous when multiple reasonable interpretations exist.

### 8.1 Example

> Make the dashboard faster.

Questions:

- Which dashboard?
- Which operation is slow?
- What performance metric matters?
- What is the current baseline?
- What improvement is expected?

### 8.2 Better Requirement

> Reduce the response time of the admin dashboard's existing report query while preserving its output and filtering behavior.

This is more specific, although measurable performance targets may still need clarification.

### 8.3 Handling Ambiguity

Codex should:

1. Identify missing information.
2. Inspect existing conventions.
3. Distinguish facts from assumptions.
4. Ask for clarification when the decision materially affects implementation.
5. Avoid inventing major requirements.

### 8.4 Example Planning Note

```markdown
## Open Question

The task requires task editing, but the
approved specification does not define
whether completed tasks may be edited.

Implementation should not proceed on this
behavior until the requirement is clarified.
```

### 8.5 Key Principle

**Do not convert uncertainty into an undocumented architectural decision.**

---

## 9. Stage 2 — Define the Implementation Scope

Scope establishes the boundaries of the planned work.

### 9.1 In Scope

Changes required to complete the task.

### 9.2 Out of Scope

Changes explicitly excluded from the task.

### 9.3 Example — Login UI

| In Scope | Out of Scope |
|---|---|
| XML alignment | Authentication redesign |
| Spacing improvements | New login providers |
| Relevant styles | Firebase configuration changes |
| Layout verification | Unrelated screen redesign |

### 9.4 Example Scope Statement

```markdown
## Scope

Improve the existing login screen layout.

## In Scope

- Input alignment
- Component spacing
- Relevant XML resources

## Out of Scope

- Authentication logic
- Navigation architecture
- Database configuration
- New dependencies
```

### 9.5 Why Scope Matters

A plan without boundaries can expand into unrelated implementation work.

---

## 10. Stage 3 — Identify Affected Files

**File-Level Planning** identifies which source files are expected to change and why.

This makes implementation more predictable and easier to review.

### 10.1 Example — Android

| File | Planned Change | Reason |
|---|---|---|
| `activity_login.xml` | Adjust constraints and spacing | UI alignment |
| Shared dimensions resource, if used | Reuse or adjust spacing values | Consistency |
| Login Activity | No planned change | Preserve authentication behavior |

The actual filenames must be confirmed through repository exploration.

### 10.2 Example — Laravel

| File or Component | Planned Change | Reason |
|---|---|---|
| Registration validation | Correct email rule | Fix invalid behavior |
| Registration tests | Add regression coverage | Verify fix |
| Controller | Change only if necessary | Preserve existing flow |
| Database migrations | No planned change | Outside scope |

### 10.3 File-Level Planning Questions

1. Does the file exist?
2. What responsibility does it have?
3. Why must it change?
4. What other components depend on it?
5. Can the task be completed without modifying it?

### 10.4 Important Principle

**List expected affected files, but do not assume the initial list is complete until the codebase has been inspected.**

---

## 11. Stage 4 — Analyze Dependencies

Before implementation, identify dependencies between components.

### 11.1 Internal Dependencies

Examples:

- Activity → Repository
- Controller → Service
- Service → Model
- Frontend → API contract

### 11.2 External Dependencies

Examples:

- Firebase SDK
- AndroidX libraries
- Laravel packages
- PostgreSQL drivers

### 11.3 Example

A Laravel API response field is renamed.

Potentially affected components:

```text
Controller / Resource
        |
        v
API Response
        |
        v
Next.js Frontend
        |
        v
UI Components
```

The change may require coordinated updates.

### 11.4 Dependency Planning Questions

- What existing components can be reused?
- Which interfaces must remain stable?
- Will the change affect external consumers?
- Are new packages actually necessary?
- Are compatibility constraints documented?

### 11.5 Key Principle

**Understand the dependency impact before changing shared behavior.**

---

## 12. Stage 5 — Architectural Constraints

Architectural constraints define how the implementation must fit within the existing system.

### 12.1 Android Example

```markdown
## Architecture Constraints

- Preserve existing Java and XML conventions.
- Follow established UI and data boundaries.
- Reuse current Firebase integration patterns.
- Do not introduce unnecessary layers.
- Do not modify unrelated modules.
```

### 12.2 Laravel Example

```markdown
## Architecture Constraints

- Preserve existing route conventions.
- Follow established controller patterns.
- Reuse existing validation mechanisms.
- Keep business logic within the project's
  established components.
- Do not introduce unnecessary dependencies.
```

### 12.3 Important Consideration

Do not impose a new architecture simply because it is commonly recommended.

The plan should reflect the actual project's design and requirements.

---

## 13. Stage 6 — Risk Assessment

**Risk Assessment** identifies potential problems that may arise during implementation.

A risk is a possible negative outcome, not necessarily an existing defect.

### 13.1 Common Risk Categories

- Functional regressions
- Architectural inconsistency
- Data integrity
- Dependency compatibility
- Security
- Performance
- Integration failures
- Testing limitations

### 13.2 Simple Risk Matrix

| Likelihood | Impact | Suggested Attention |
|---|---|---|
| Low | Low | Basic verification |
| High | Low | Focused mitigation |
| Low | High | Preventive checks |
| High | High | Strong controls and review |

### 13.3 Example — Android

Risk:

> Updating the login layout could break references to existing view IDs.

Mitigation:

- Preserve IDs.
- Inspect Java references.
- Run the Android build.
- Verify the screen visually.

### 13.4 Example — Laravel

Risk:

> Changing registration validation could reject legitimate users.

Mitigation:

- Review existing rules.
- Add tests for valid and invalid inputs.
- Preserve response conventions.

### 13.5 Risk Register Example

```markdown
## Risks

### Risk 1
Existing login view IDs may be changed.

Likelihood: Medium
Impact: High

Mitigation:
Preserve existing IDs and verify Java references.

### Risk 2
Layout changes may behave differently
on smaller screens.

Likelihood: Medium
Impact: Medium

Mitigation:
Test supported screen sizes.
```

### 13.6 Key Principle

**Identify risks early enough to influence the implementation plan.**

---

## 14. Stage 7 — Acceptance Criteria

**Acceptance Criteria** define observable conditions that determine whether implementation is complete.

### 14.1 Weak Criteria

> The registration screen works well.

### 14.2 Better Criteria

- Required fields are displayed.
- Required fields reject empty input.
- Existing view IDs remain compatible.
- Successful submission follows the approved flow.
- Relevant build checks pass.
- Remaining device verification is reported.

### 14.3 Backend Example

Feature:

> Add product search.

Acceptance criteria:

- A name query returns matching products.
- Empty input follows the approved behavior.
- Existing product endpoints remain functional.
- Response formatting matches existing conventions.
- Relevant tests pass.

### 14.4 Acceptance Criteria vs Implementation Steps

Acceptance criterion:

> Duplicate emails are rejected.

Implementation step:

> Update the existing registration validation rule.

The first describes the expected result.

The second describes how to achieve it.

### 14.5 Key Principle

**Acceptance criteria describe success; implementation steps describe the path to success.**

---

## 15. Stage 8 — Implementation Sequencing

**Implementation Sequencing** determines the order in which development changes should be performed.

### 15.1 Why Sequence Matters

Some changes depend on others.

For example, a frontend feature may depend on an existing backend API contract.

### 15.2 Example — Laravel Feature

```text
Inspect existing endpoint
          |
          v
Confirm requirements
          |
          v
Update validation or business logic
          |
          v
Add relevant tests
          |
          v
Run verification
          |
          v
Review diff
```

### 15.3 Example — Android UI

```text
Inspect existing screen
          |
          v
Identify layout resources
          |
          v
Apply XML changes
          |
          v
Build application
          |
          v
Inspect layout
          |
          v
Review changes
```

### 15.4 Sequencing Rules

- Complete prerequisites before dependent work.
- Keep changes small enough to verify.
- Avoid unrelated parallel changes.
- Reassess the plan when new evidence appears.

### 15.5 Important Principle

**Order tasks according to dependencies and verification needs, not merely file location.**

---

## 16. Stage 9 — Approval Checkpoints

An **Approval Checkpoint** identifies a decision requiring developer authorization before implementation proceeds.

### 16.1 Common Approval Checkpoints

- Adding a major dependency.
- Changing database schema.
- Redesigning authentication.
- Modifying unrelated modules.
- Expanding approved feature scope.
- Performing destructive operations.
- Deploying to production.

### 16.2 Example

```markdown
## Approval Checkpoints

Stop and request approval before:

- Adding new dependencies.
- Changing the database schema.
- Modifying files outside approved scope.
- Introducing major architectural changes.
```

### 16.3 Approval Request Format

```markdown
## Proposed Change

Describe the required change.

## Reason

Explain why the existing approach is insufficient.

## Affected Components

List relevant files and modules.

## Risks

Explain possible consequences.

## Decision Required

State what approval is needed.
```

### 16.4 Important Distinction

Prompt-level approval checkpoints are workflow instructions.

Codex CLI's technical sandbox and approval settings are configured separately.

---

## 17. Stage 10 — Verification Planning

Verification should be planned before implementation begins.

### 17.1 Verification Questions

- What behavior must be tested?
- Which automated tests exist?
- What build commands are relevant?
- Are manual checks required?
- What evidence will demonstrate success?
- What limitations must be reported?

### 17.2 Android Verification

Potential checks:

```powershell
.\gradlew.bat assembleDebug
```

```powershell
.\gradlew.bat test
```

Run only tasks supported by the actual project.

Additional UI verification may require an emulator or physical device.

### 17.3 Laravel Verification

```powershell
php artisan test
```

For targeted tests, use the test file or filter supported by the project's test setup.

### 17.4 Git Verification

```powershell
git diff --check
git diff
git status
```

### 17.5 Verification Matrix

| Requirement | Verification |
|---|---|
| Android UI builds | Gradle build |
| Layout displays correctly | Emulator/device inspection |
| Laravel validation works | Feature tests |
| Existing behavior preserved | Regression tests |
| No unrelated modifications | Git diff review |

### 17.6 Important Principle

**Verification is stronger when each acceptance criterion has a corresponding check.**

---

## 18. Stage 11 — Recovery and Rollback Planning

Some changes may fail or produce unexpected results.

A recovery plan explains how to return to a safe state.

### 18.1 Low-Risk Changes

For a small UI adjustment, recovery may involve reviewing and reverting the specific changes after confirming ownership of the affected files.

### 18.2 Higher-Risk Changes

For database or integration changes, planning may require:

- Backups.
- Migration review.
- Data preservation.
- Compatibility considerations.
- Controlled deployment procedures.

### 18.3 Important Warning

Do not use destructive Git commands such as `git reset --hard` as a generic recovery strategy.

Such commands can discard uncommitted work.

### 18.4 Recommended Practice

Before implementation:

```powershell
git status
```

After implementation:

```powershell
git diff
```

Preserve unrelated user changes.

### 18.5 Key Principle

**A reliable plan considers not only how to make a change, but also how to respond if it fails.**

---

## 19. Anatomy of a Good Implementation Plan

A practical implementation plan should contain the following sections.

```text
Implementation Plan
│
├── Objective
├── Current State
├── Requirements
├── In Scope
├── Out of Scope
├── Affected Files
├── Proposed Approach
├── Implementation Sequence
├── Architectural Constraints
├── Risks and Mitigations
├── Acceptance Criteria
├── Verification Plan
├── Approval Checkpoints
├── Recovery Considerations
└── Completion Conditions
```

### 19.1 Why This Structure Works

It connects the requirements to implementation decisions and verification evidence.

### 19.2 Is Every Section Mandatory?

No.

Small tasks may use a shorter plan.

Complex tasks may require more detailed documentation.

**The plan should be as detailed as necessary, but no more complex than the task requires.**

---

## 20. Reusable Implementation Plan Template

Use this template for medium or complex Codex tasks.

```markdown
# Implementation Plan — [Feature Name]

## 1. Objective

Describe the intended outcome.

## 2. Current State

Summarize the relevant existing implementation.

## 3. Requirements

### Functional
- Requirement 1
- Requirement 2

### Non-Functional
- Constraint 1
- Constraint 2

## 4. Scope

### In Scope
- Approved change 1
- Approved change 2

### Out of Scope
- Excluded change 1
- Excluded change 2

## 5. Affected Files

| File | Proposed Change | Reason |
|---|---|---|
| path/to/file | Description | Justification |

## 6. Proposed Approach

Explain the technical solution.

## 7. Implementation Sequence

1. Step one
2. Step two
3. Step three

## 8. Architectural Constraints

- Preserve existing architecture.
- Reuse existing components.
- Avoid unnecessary dependencies.

## 9. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Risk 1 | Medium | Mitigation |

## 10. Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## 11. Verification Plan

- Relevant build checks
- Automated tests
- Manual verification
- Git diff review

## 12. Approval Checkpoints

Request approval before significant
scope or architecture changes.

## 13. Recovery Considerations

Describe safe recovery steps if needed.

## 14. Completion Conditions

Stop when the approved acceptance criteria
are satisfied and verification results
have been reported.

## 15. Open Questions

List unresolved requirements or assumptions.
```

---

## 21. Where Should Implementation Plans Be Stored?

Not every implementation plan needs a permanent file.

### 21.1 Option A — Conversation-Only Plan

Best for small tasks, short investigations, and one-session changes. The plan can remain in the Codex conversation.

### 21.2 Option B — Project Documentation

For longer tasks:

```text
docs/
└── plans/
    └── login-ui-alignment.md
```

Useful when the plan needs to be reviewed or resumed later.

### 21.3 Option C — Existing Project Planning System

If the repository already uses `PLANS.md` or another planning convention, follow it rather than introducing a competing structure.

### 21.4 Important Principle

**Do not create planning files simply because Codex can create them. Use persistent plans when they provide real value.**

---

## 22. Planning-Only Workflow with Codex CLI

A planning-only workflow allows the developer to review the proposed approach before implementation.

### Step 1 — Navigate to the Repository

Android example:

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
```

### Step 2 — Inspect Git Status

```powershell
git status
```

### Step 3 — Start a Read-Only Session

```powershell
codex --sandbox read-only
```

### Step 4 — Provide a Planning Prompt

```text
Read the applicable AGENTS.md instructions
and relevant project documentation.

Inspect the existing login screen
implementation.

Do not modify files.

Create an implementation plan containing:

1. Objective
2. Current state
3. Requirements
4. In-scope changes
5. Out-of-scope changes
6. Expected affected files
7. Proposed approach
8. Implementation sequence
9. Risks and mitigations
10. Acceptance criteria
11. Verification plan
12. Approval checkpoints
13. Open questions

Use actual repository evidence.

Do not implement the plan.
```

### Step 5 — Review the Plan

Check whether the plan uses actual file paths, matches approved requirements, preserves architecture, avoids unnecessary dependencies, identifies realistic risks, and includes verification procedures.

### Step 6 — Revise if Necessary

```text
Revise the plan.

Keep the implementation limited to the
existing XML layout and required resources.

Do not change authentication logic.

Remove unnecessary dependencies.

Return the revised plan only.
```

### Step 7 — Approve the Approach

Only after the plan is accepted should the developer begin implementation using an appropriately authorized session.

### Important Note

Some Codex interfaces and versions provide a dedicated Plan mode.

The read-only workflow above is a portable way to request planning without repository edits.

Check the installed CLI's help and available commands rather than assuming every version exposes identical controls.

---

## 23. Real-World Example — GroupTrack Android

The developer wants to implement a Create Account screen UI in an Android Studio application using Java, XML layouts, and Firebase. The project has an approved MVP scope.

### 23.1 Objective

Create the registration screen UI while preserving the existing authentication architecture.

### 23.2 Requirements

Functional:

- Display required registration fields.
- Provide a Create Account action.
- Display appropriate input feedback.
- Support the approved navigation flow.

Non-functional:

- Follow existing Java/XML conventions.
- Reuse current resources.
- Avoid unnecessary libraries.
- Preserve existing functionality.

### 23.3 Scope

**In Scope:** Registration XML layout, relevant drawable and string resources, and minimal Java UI integration when required.

**Out of Scope:** New authentication providers, additional user roles, unapproved onboarding features, Firebase architecture redesign, and unrelated screen refactoring.

### 23.4 Expected Affected Files

The actual files must be identified through exploration. Potential examples:

```text
app/src/main/res/layout/activity_register.xml
app/src/main/java/.../RegisterActivity.java
app/src/main/res/values/strings.xml
```

These are examples, not confirmed GroupTrack paths.

### 23.5 Implementation Sequence

1. Inspect existing sign-in UI conventions.
2. Identify reusable resources.
3. Confirm required registration fields.
4. Create or update the approved XML layout.
5. Add minimal UI integration if needed.
6. Run relevant build checks.
7. Verify visual behavior.
8. Review the Git diff.

### 23.6 Risks

| Risk | Mitigation |
|---|---|
| Broken view IDs | Inspect Java references |
| Inconsistent UI | Reuse existing resources |
| Navigation regression | Preserve existing flow |
| Unnecessary dependencies | Use current Android components |

### 23.7 Acceptance Criteria

- [ ] Registration screen displays the approved fields.
- [ ] Existing UI conventions are followed.
- [ ] Navigation remains compatible.
- [ ] No unnecessary dependencies are introduced.
- [ ] Relevant build checks pass.
- [ ] Visual verification is completed or clearly reported as pending.

### 23.8 Approval Checkpoints

Request approval before changing Firebase authentication architecture, adding dependencies, expanding the registration feature, or modifying unrelated screens.

### 23.9 Key Lesson

The plan should keep Codex focused on the registration UI rather than expanding the task into an authentication-system redesign.

---

## 24. Real-World Example — Laravel Registration Validation

A Laravel backend using PostgreSQL fails to reject duplicate email addresses during user registration.

### 24.1 Objective

Correct duplicate-email validation without changing unrelated authentication behavior.

### 24.2 Current State

The actual registration route, validation mechanism, database constraints, and tests must first be inspected. Do not assume the root cause before exploration.

### 24.3 Requirements

Functional:

- Reject duplicate email addresses.
- Preserve valid registration behavior.
- Return the project's expected validation response.

Non-functional:

- Preserve existing architecture.
- Avoid unnecessary dependencies.
- Avoid unrelated schema changes.
- Maintain existing API conventions.

### 24.4 Scope

**In Scope:** Registration validation investigation, minimal correction, and relevant regression tests.

**Out of Scope:** Authentication redesign, new authentication providers, unrelated controller refactoring, and database schema redesign.

### 24.5 Potential Affected Components

```text
Registration route
Registration controller
Validation rules or Form Request
User model
Related feature tests
```

The exact files must be confirmed in the repository.

### 24.6 Implementation Sequence

1. Inspect registration route.
2. Inspect controller and validation.
3. Check relevant database constraints.
4. Review existing tests.
5. Identify the root cause.
6. Propose the smallest correction.
7. Implement after approval.
8. Run relevant tests.
9. Review the diff.

### 24.7 Risks

| Risk | Mitigation |
|---|---|
| Valid registration rejected | Positive-path test |
| Duplicate email still accepted | Negative-path test |
| API response changed | Response contract test |
| Unrelated authentication regression | Relevant regression tests |

### 24.8 Acceptance Criteria

- [ ] Duplicate emails are rejected.
- [ ] Valid registrations continue to work.
- [ ] Expected response conventions are preserved.
- [ ] Relevant tests pass.
- [ ] No unrelated files are modified.

### 24.9 Approval Checkpoints

Request approval before changing database schema, adding authentication packages, or refactoring unrelated authentication components.

### 24.10 Key Lesson

A focused plan helps Codex investigate the real cause rather than rewriting the entire authentication system.

---

## 25. Real-World Example — Next.js and Laravel Integration

A Next.js frontend needs to display product information from a Laravel API.

### 25.1 Planning Questions

- Does the API endpoint already exist?
- What is the response structure?
- How does the frontend configure its API base URL?
- Are authentication headers required?
- How are loading and error states handled?
- Which components already exist?

### 25.2 Example Dependency Flow

```text
Next.js Page
     |
     v
API Client / Fetch
     |
     v
Laravel API Route
     |
     v
Controller
     |
     v
Model / Database
```

### 25.3 Planning Considerations

Frontend: identify the relevant route, reuse existing components, follow TypeScript conventions, and handle loading and error states.

Backend: preserve API response contracts, reuse existing endpoints, and avoid unnecessary schema changes.

Integration: confirm the expected data format, verify error handling, and test the full request flow.

### 25.4 Important Principle

**Cross-application features require planning around interface contracts, not just individual files.**

---

## 26. Reviewing an Implementation Plan

Before implementation, evaluate the plan using a structured checklist.

### 26.1 Requirements Review

- Are all approved requirements represented?
- Are important assumptions identified?
- Are non-goals clear?

### 26.2 Architecture Review

- Does the plan follow existing conventions?
- Are responsibilities placed appropriately?
- Are new dependencies justified?

### 26.3 Scope Review

- Are affected files identified?
- Are unrelated changes excluded?
- Are approval checkpoints defined?

### 26.4 Risk Review

- Are significant risks identified?
- Are mitigations realistic?
- Is recovery considered where necessary?

### 26.5 Verification Review

- Does each acceptance criterion have a verification method?
- Are relevant tests identified?
- Are manual checks distinguished from automated checks?

### 26.6 Decision

The developer may approve the plan, request revisions, ask for more investigation, or reject the approach.

---

## 27. Handling New Discoveries During Implementation

An implementation plan is not always final. New information may appear after coding begins.

For example, the plan expects a controller change, but Codex discovers that the behavior is implemented in a shared service.

Recommended response:

1. Explain the new finding.
2. Identify why the original plan is insufficient.
3. Propose a revised approach.
4. Describe additional affected files.
5. Request approval if the change expands scope.
6. Continue only after the necessary decision.

**Key Principle:** A plan should guide implementation without preventing necessary evidence-based revisions.

---

## 28. Common Planning Mistakes

1. **Planning without exploration:** The plan may reference nonexistent files or incorrect architecture. Inspect first.
2. **Vague objectives:** The expected outcome is unclear. Define specific behavior.
3. **Missing non-goals:** Adjacent features may be implemented. State meaningful exclusions.
4. **Overly detailed planning for trivial tasks:** Overhead exceeds task complexity. Plan proportionally.
5. **Assuming the root cause:** The plan may target the wrong component. Separate findings from hypotheses.
6. **Ignoring dependencies:** Connected components may break. Perform impact analysis.
7. **Missing acceptance criteria:** Completion cannot be measured. Define observable conditions.
8. **No verification plan:** The agent may finish without evidence. Identify checks before implementation.
9. **No approval checkpoints:** Significant changes may proceed without review. Define decision boundaries.
10. **Treating the plan as immutable:** New evidence cannot be incorporated. Revise when justified and approved.

---

## 29. Practical Exercise A — Requirements Analysis

### Objective

Practice transforming a vague request into clear requirements.

### Scenario

The developer requests:

> Improve the Android task creation screen.

### Instructions

Identify functional requirements, non-functional requirements, missing information, in-scope changes, and out-of-scope changes.

### Reflection Questions

- What does "improve" mean?
- Which behavior must remain unchanged?
- Are additional features authorized?
- What information must be clarified?

**Expected Outcome:** A clear requirements summary suitable for implementation planning.

---

## 30. Practical Exercise B — File-Level Planning

### Objective

Identify expected affected files before coding.

### Scenario

A Laravel endpoint needs improved validation.

### Instructions

Inspect or conceptually identify the route definition, controller, validation mechanism, related tests, and relevant model or database constraints.

Create a table:

| File | Planned Change | Reason |
|---|---|---|
| File A | Change | Reason |
| File B | Change | Reason |

Which files require modification, and which should only be inspected?

---

## 31. Practical Exercise C — Risk Assessment

### Objective

Identify potential implementation risks.

### Scenario

An Android login UI will be redesigned while preserving existing authentication.

### Instructions

Identify at least three risks. For each, provide a description, likelihood, impact, mitigation, and verification method.

```markdown
## Risk

Existing view IDs may change.

## Impact
Authentication event handlers may break.

## Mitigation
Preserve existing IDs.

## Verification
Inspect Java references and run the build.
```

**Expected Outcome:** A practical risk register for the proposed UI change.

---

## 32. Practical Exercise D — Planning-Only Codex Session

### Objective

Use Codex to produce an implementation plan without modifying code.

### Step 1 — Navigate to the Project

```powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
```

### Step 2 — Inspect Git Status

```powershell
git status
```

### Step 3 — Start Codex

```powershell
codex --sandbox read-only
```

### Step 4 — Submit the Prompt

```text
Perform a planning-only analysis.

Read applicable AGENTS.md instructions
and relevant project documentation.

Inspect the existing login screen.

Task:
Improve the layout alignment while
preserving current authentication behavior.

Produce a structured implementation plan:

1. Objective
2. Current state
3. Requirements
4. In scope
5. Out of scope
6. Expected affected files
7. Proposed technical approach
8. Implementation sequence
9. Risks and mitigations
10. Acceptance criteria
11. Verification plan
12. Approval checkpoints
13. Open questions

Do not modify files.

Do not introduce unnecessary features,
dependencies, or architecture.

Stop after presenting the plan.
```

### Step 5 — Review the Output

Check whether file paths are real, scope is correct, architecture is preserved, risks are identified, acceptance criteria are measurable, and verification is realistic.

**Expected Outcome:** A reviewable implementation plan that can be approved before coding.

---

## 33. Practical Exercise E — Evaluate a Bad Plan

Codex proposes the following approach for a simple login layout adjustment:

```text
1. Replace XML layouts with Jetpack Compose.
2. Rewrite authentication.
3. Introduce a new navigation framework.
4. Refactor the entire application.
5. Add animation libraries.
6. Adjust the login button spacing.
```

Identify which step directly addresses the task, which exceed scope, which dependencies are unnecessary, what risks this creates, and how to revise the plan.

**Expected Outcome:** Recognize overengineering before implementation begins.

---

## 34. Knowledge Check

1. What is implementation planning?
2. Why should planning follow codebase exploration?
3. What is requirements analysis?
4. What is the difference between functional and non-functional requirements?
5. How should ambiguous requirements be handled?
6. Why are scope boundaries important?
7. What is file-level planning?
8. Why should dependencies be analyzed?
9. What is risk assessment?
10. What is the difference between acceptance criteria and implementation steps?
11. What is implementation sequencing?
12. Why are approval checkpoints necessary?
13. What should a verification plan contain?
14. Why is recovery planning important?
15. When should an implementation plan be revised?

---

## 35. Key Terminologies

| Term | Definition |
|---|---|
| Implementation Planning | Designing the approach before changing code |
| Requirements Analysis | Identifying expected behavior and constraints |
| Functional Requirement | Behavior the system must provide |
| Non-Functional Requirement | Quality or operational constraint |
| Scope Boundary | Limit on approved changes |
| Non-Goal | Explicitly excluded outcome |
| File-Level Planning | Identifying files expected to change |
| Dependency Analysis | Examining component and package relationships |
| Risk Assessment | Identifying potential negative outcomes |
| Risk Mitigation | Action intended to reduce risk |
| Acceptance Criteria | Observable conditions defining success |
| Implementation Sequence | Ordered development steps |
| Approval Checkpoint | Point requiring developer authorization |
| Verification Plan | Strategy for demonstrating correctness |
| Recovery Plan | Approach for responding to failed changes |
| Planning-Only Workflow | Analysis and planning without implementation |

---

## 36. Key Takeaways

1. **Implementation planning transforms requirements into a structured development approach.**
2. **Planning should follow codebase exploration and rely on actual repository evidence.**
3. **Requirements must be clear enough to guide implementation.**
4. **In-scope and out-of-scope boundaries prevent unnecessary changes.**
5. **File-level planning improves predictability and reviewability.**
6. **Dependency analysis identifies potential change impacts.**
7. **Architectural constraints preserve existing design decisions.**
8. **Risk assessment helps identify problems before implementation.**
9. **Acceptance criteria define observable completion conditions.**
10. **Implementation sequencing organizes work according to dependencies.**
11. **Approval checkpoints preserve developer control over significant decisions.**
12. **Verification should be planned before code changes begin.**
13. **Recovery considerations are important for higher-risk changes.**
14. **Plans should be proportional to task complexity.**
15. **A good plan can be revised when new evidence justifies a different approach.**

---

## 37. Official References

### OpenAI Codex

1. [OpenAI — Codex Documentation](https://developers.openai.com/codex/)
2. [OpenAI — Codex CLI](https://developers.openai.com/codex/cli/)
3. [OpenAI — Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
4. [OpenAI — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex) — Structured development phases, scope restrictions, acceptance criteria, approval gates, and verification evidence.
5. [OpenAI — Using PLANS.md for Multi-Hour Problem Solving](https://developers.openai.com/cookbook/articles/codex_exec_plans) — An optional execution-planning convention for complex work.
6. [OpenAI — AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
7. [OpenAI — Codex Security](https://developers.openai.com/codex/security/)

### Android

8. [Android Developers — Guide to App Architecture](https://developer.android.com/topic/architecture)
9. [Android Developers — Testing](https://developer.android.com/training/testing)

### Laravel

10. [Laravel 12.x — Controllers](https://laravel.com/docs/12.x/controllers)
11. [Laravel 12.x — Validation](https://laravel.com/docs/12.x/validation)
12. [Laravel 12.x — Testing](https://laravel.com/docs/12.x/testing)

### Git

13. [Git — Documentation](https://git-scm.com/doc)

---

## 38. Lesson Completion Checklist

- [ ] Understand implementation planning.
- [ ] Differentiate exploration, planning, and implementation.
- [ ] Analyze functional requirements.
- [ ] Analyze non-functional requirements.
- [ ] Identify ambiguous requirements.
- [ ] Define in-scope changes.
- [ ] Define out-of-scope changes.
- [ ] Create file-level change plans.
- [ ] Analyze dependencies.
- [ ] Identify architectural constraints.
- [ ] Perform risk assessment.
- [ ] Write acceptance criteria.
- [ ] Sequence implementation steps.
- [ ] Define approval checkpoints.
- [ ] Create a verification plan.
- [ ] Understand recovery planning.
- [ ] Use Codex for planning-only analysis.
- [ ] Review Android and Laravel examples.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[04 - Task Decomposition and Feature Development]]**

The next lesson focuses on transforming an approved implementation plan into manageable development tasks.

Topics include:

- Breaking down complex features
- Task dependencies
- Vertical slicing
- Implementation milestones
- Incremental development
- Managing feature scope
- Coordinating related code changes
- Testing individual increments
- Codex feature implementation workflows

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.3*  
*Learning Principle: Explore first, plan deliberately, obtain necessary approvals, and implement only the agreed changes.*
