# 01 — Project Selection and Scope Definition

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 6 — Real-World Application
> **Lesson:** 6.1 — Project Selection and Scope Definition
> **Difficulty:** Intermediate to Advanced
> **Learning Mode:** Theory First
> **Prerequisite:** [[07 - Advanced Codebase Management]]
> **Next Lesson:** [[02 - Project Setup and Context Preparation]]
> **Status:** Theory Documented

---

# 1. Introduction

Before developing software, teams need to agree on the problem, users, desired outcomes, essential functionality, constraints, and evidence of success. These decisions matter when working with AI coding agents such as OpenAI Codex. Codex can inspect repositories, plan work, write code, identify defects, and run checks, but it cannot infer the correct product requirements from an ambiguous request.

Vague scope can lead to unnecessary features, unsupported assumptions, excessive dependencies, overcomplicated architecture, unrelated changes, or effort spent solving the wrong problem.

## Core Principle

**Before asking Codex to build a system, define the problem, the necessary functionality, and the evidence that will demonstrate success.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Evaluate and select a feasible project.
2. Write a problem statement and identify stakeholders.
3. Define measurable objectives and an MVP.
4. Identify functional and non-functional requirements.
5. Write user stories and testable acceptance criteria.
6. Define in-scope and out-of-scope functionality.
7. Assess technical, operational, schedule, and resource feasibility.
8. Choose technologies based on project needs.
9. Document constraints, assumptions, dependencies, and risks.
10. Prevent scope creep and overengineering.
11. Prepare approved requirements and verification boundaries for Codex.
12. Assess readiness for implementation planning.

---

# Part II — Project Selection

## 3. What Is Project Selection?

Project selection identifies and evaluates candidate projects before development resources are committed. A suitable project is useful, feasible with available skills and resources, appropriately scoped, and verifiable.

```text
Candidate ideas → problem and user analysis → feasibility review
               → comparison → selection → scope definition
```

Select projects because they address a meaningful problem, not solely because their technology stack seems interesting.

---

## 4. Project Idea vs Project Definition

**Project idea:**

> Build a student productivity application.

This does not specify the users' problem, necessary behavior, or how success will be evaluated.

**More actionable definition:**

> Develop a student task management application that lets students create tasks, assign due dates, update completion status, and review outstanding work.

This identifies a user group and initial capability boundary. Requirements and stakeholder approval are still needed before implementation.

**Key Principle: An idea becomes actionable when its problem, users, scope, and success criteria are defined.**

---

## 5. Project Evaluation Criteria

| Criterion | Guiding question |
|---|---|
| Problem relevance | Does the project address a meaningful problem? |
| User value | Will the intended users benefit? |
| Technical feasibility | Can the team build it with available skills and tools? |
| Scope clarity | Are requirements clear enough to plan? |
| Time feasibility | Can the approved MVP be completed in the available period? |
| Resources | Are tools, data, devices, and services available? |
| Testability | Can expected behavior be verified? |
| Maintainability | Can the team understand and maintain the result? |
| Learning value | Does it support the intended learning goals? |
| Risk | Are important uncertainties manageable? |

A strong project is not necessarily ambitious. It is one whose essential outcomes can be delivered and verified within its constraints.

---

## 6. Weighted Evaluation Matrix

A weighted matrix can help compare candidate projects. The following weights and scores are illustrative; adapt them to the actual project.

| Criterion | Example weight |
|---|---:|
| Problem relevance | 20% |
| Technical feasibility | 20% |
| Scope clarity | 15% |
| Time feasibility | 15% |
| Testability | 15% |
| Learning value | 10% |
| Resource availability | 5% |
| **Total** | **100%** |

Rate each project from 1 (poor) to 5 (excellent):

\[
\text{Weighted Score} = \sum_{i=1}^{n} w_i r_i
\]

where \(w_i\) is the criterion weight as a decimal and \(r_i\) is the rating from 1 to 5.

| Hypothetical project | Example score |
|---|---:|
| Android student task app | 4.40 / 5 |
| Laravel inventory system | 4.15 / 5 |
| Full marketplace platform | 3.10 / 5 |

These scores are examples, not evaluations of existing projects. A high score does not override a critical blocker such as unavailable data or unacceptable risk.

---

# Part III — Problem and Stakeholder Definition

## 7. Write a Problem Statement

A problem statement describes the issue the project intends to address. It should identify who experiences it, what happens today, why it matters, and what improvement is desired—without prematurely deciding on a technology solution.

**Weak:**

> Students need a better application.

**More useful:**

> Students managing requirements across several courses may struggle to track task deadlines when assignments and status updates are scattered across notes and messaging platforms.

### Problem Definition Template

```markdown
## Target Users
Who experiences the problem?

## Current Situation
How is the task currently performed?

## Existing Difficulty
What prevents users from reaching their goal?

## Consequences
What happens because of this difficulty?

## Desired Improvement
What outcome should a solution enable?
```

**Problem vs solution:** “The school has no mobile app” describes an absent product. “Students have difficulty tracking assignments and deadlines across courses” describes the underlying problem. Define the problem before deciding whether an app is the right solution.

---

## 8. Identify Stakeholders

A stakeholder is a person or organization that affects or is affected by the system. Stakeholders may include end users, administrators, project owners, developers, maintainers, or organizations supplying required data.

| Stakeholder | Primary need | Possible concern |
|---|---|---|
| Student | View assigned work | Ease of use |
| Group leader | Organize assignments | Accurate progress |
| Maintainer | Operate the application | Reliability and security |
| Project owner | Achieve project objectives | Time and resource limits |

Different stakeholders may have different requirements, but do not create distinct roles or dashboards unless responsibilities and permissions require them.

---

## 9. Define Project Objectives

A project objective describes an outcome. Prefer outcomes over technology statements.

**Less useful:**

> Build an advanced application using modern technologies.

**More useful:**

> Enable students to create, organize, and track academic tasks through a centralized application.

### SMART Objectives

SMART means **Specific, Measurable, Achievable, Relevant, and Time-bound**.

Example:

> By the end of the approved MVP period, implement and verify task creation, task listing, and completion-status updates against the agreed acceptance tests.

Measurability should come from observable outcomes. Do not invent numerical business targets without supporting stakeholder evidence.

---

# Part IV — MVP and Scope

## 10. What Is an MVP?

A **Minimum Viable Product (MVP)** is an initial version with enough functionality to deliver its core value and support meaningful evaluation. It is not merely an incomplete application; it should support a coherent user workflow.

```text
Create task → view task → update status → review progress
```

The approved MVP should come from project requirements, not a generic feature list.

| MVP | Broader product |
|---|---|
| Core user value | Additional workflows and capabilities |
| Limited, coherent scope | Wider functionality |
| Easier to evaluate | More integration and validation work |
| Focused verification | Broader verification |

Possible future enhancements might include calendar integration, custom notifications, recommendations, or analytics. Their presence in an idea list does not make them approved MVP requirements.

---

## 11. Minimum Does Not Mean Low Quality

An MVP still needs appropriate input validation, error handling, authentication and authorization when required, data integrity, security, accessibility, and testing.

**Reduce unnecessary features, not essential quality safeguards.**

---

## 12. Define Project Scope

Project scope sets the boundaries of approved work.

```text
Project scope
  ├── In-scope features
  ├── Out-of-scope features
  ├── Constraints and dependencies
  ├── Deliverables
  └── Acceptance criteria
```

### In-Scope Example

```markdown
## In Scope
- User authentication.
- Task creation.
- Task listing.
- Task status updates.
- Basic progress summary.
```

Include a feature only when it contributes to the approved objective.

### Out-of-Scope Example

```markdown
## Out of Scope
- AI task recommendations.
- External calendar synchronization.
- Real-time chat.
- Payment processing.
- Advanced reporting.
```

Explicit exclusions reduce the chance that related ideas are treated as implicit requirements.

---

## 13. Scope Creep and Change Control

**Scope creep** is uncontrolled expansion of requirements. For example, a task-list request may grow to include chat, recommendations, calendar integrations, gamification, and analytics without an approved change.

When a new feature is proposed:

1. Record the request and the user value it may provide.
2. Estimate effects on scope, schedule, testing, and dependencies.
3. Decide whether it is required for the MVP.
4. Obtain approval from the appropriate project owner.
5. Update the scope and acceptance criteria if approved.
6. Otherwise, record it for future consideration.

```text
New request → assess MVP need and impact → approve and update scope
                                  └──────→ defer to future work
```

An AI agent should not expand project scope because an enhancement seems useful.

---

# Part V — Requirements

## 14. Functional Requirements

Functional requirements describe what the system must do.

Examples:

- The system shall allow users to sign in.
- The system shall allow authorized users to create tasks.
- The system shall display tasks assigned to a user.
- The system shall allow an authorized user to update task status.

Use identifiers for traceability:

| ID | Requirement | Priority |
|---|---|---|
| FR-001 | Users can sign in | Must Have |
| FR-002 | Authorized users can create tasks | Must Have |
| FR-003 | Users can view assigned tasks | Must Have |
| FR-004 | Users can update task status | Must Have |
| FR-005 | Users can export task reports | Could Have |

Strong requirements are clear, specific, testable, necessary, and feasible.

**Vague:** “The system should manage tasks efficiently.”

**Testable:** “FR-003: The system shall display the signed-in user's assigned tasks, including title, due date, and current status.”

---

## 15. Non-Functional Requirements

Non-functional requirements describe quality attributes and operating constraints such as performance, security, reliability, usability, accessibility, maintainability, and compatibility.

| ID | Category | Example |
|---|---|---|
| NFR-001 | Security | Users must not access another user's private tasks without authorization |
| NFR-002 | Reliability | Failed operations must display an understandable error |
| NFR-003 | Usability | Required task actions must have clear labels |
| NFR-004 | Maintainability | Code must follow documented project conventions |
| NFR-005 | Compatibility | The application must work in its declared supported environment |

Avoid vague requirements such as “the application must be fast.” If performance is a real requirement, agree on test environment, dataset, measurement, and target threshold.

---

## 16. User Stories

A user story expresses a need from the user's perspective:

```text
As a [user],
I want [capability],
so that [benefit].
```

Example:

> As a student, I want to view my assigned tasks so that I can identify outstanding work.

User stories describe value. Functional requirements describe expected system behavior:

> FR-004: The system shall allow an authorized user to change an assigned task's status to one of the supported values.

```text
User need → user story → functional requirement → acceptance criteria → test
```

---

## 17. Acceptance Criteria

Acceptance criteria define the conditions that must be satisfied for a requirement to be accepted. They answer: “How do we know the feature is complete?”

**Vague:** “The login feature works correctly.”

**Observable criteria:**

- Valid credentials allow access.
- Invalid credentials produce an appropriate error.
- Required fields cannot be submitted empty.
- Signed-out users cannot access protected content.

### Given–When–Then

```text
Given [initial condition]
When [action occurs]
Then [expected result]
```

Example:

```text
Given a signed-in student
When the student creates a task with valid data
Then the system saves the task
And displays it in the task list
```

Negative scenario:

```text
Given a signed-in student
When the student submits a task without a title
Then the system rejects the submission
And displays a validation message
```

Acceptance criteria define success; a test case defines steps used to verify it.

---

## 18. Definition of Done

The Definition of Done is a shared checklist for deciding when work is complete.

```markdown
- [ ] Approved requirements are implemented.
- [ ] Acceptance criteria are satisfied.
- [ ] Relevant tests pass.
- [ ] No known critical regression remains.
- [ ] Code follows project conventions.
- [ ] Relevant documentation is updated.
- [ ] Git diff is reviewed.
- [ ] Verification limitations are reported.
```

```text
Implemented ≠ Verified ≠ Accepted
```

Codex reporting completion is not, by itself, evidence that the feature meets its requirements.

---

# Part VI — Feasibility and Technology

## 19. Feasibility Analysis

Feasibility evaluates whether the project can be delivered under its constraints.

| Dimension | Questions |
|---|---|
| Technical | Are the stack, APIs, data model, and test tools available? |
| Operational | Can users and maintainers actually use and support it? |
| Schedule | Can the scope fit the time available, including testing and integration? |
| Resources | Are devices, accounts, data, services, and infrastructure available? |
| Economic | Are hosting and service costs acceptable? |
| Legal/ethical | Are data use, access, and user impacts acceptable? |

Unknown feasibility should be recorded as uncertainty, not assumed to be solved.

### Schedule Planning

Reserve time for setup, debugging, integration, testing, documentation, and review—not only coding.

### Dependency Readiness

A feature that depends on unavailable data, API credentials, devices, or external services is not implementation-ready until the dependency is resolved or an explicitly approved substitute is defined.

---

## 20. Technology Selection

Choose technology based on requirements, team expertise, ecosystem, maintainability, compatibility, security, deployment, cost, and testability—not trends alone.

### Android Example

```text
Android Studio → Java → XML layouts → Firebase
```

This may fit when native Android, Java, and existing Firebase conventions are established. Do not replace an existing stack without a clear technical reason.

### Laravel Example

```text
Laravel → PHP → Eloquent → relational database → Blade or API
```

This may fit when server-side business logic, relational data, and Laravel expertise are required.

### Next.js and Laravel Example

```text
Next.js frontend → Laravel API → PostgreSQL
```

Separate frontend/backend architecture may be appropriate, but adds API contract, authentication, CORS, and integration work. Use separation when it serves a real requirement.

---

# Part VII — Constraints, Assumptions, Risks

## 21. Constraints and Preferences

A constraint is a mandatory limitation; a preference is a favored approach that may be reconsidered.

**Constraint:** “The application must use Java because the project specification requires it.”

**Preference:** “Prefer simple layouts over introducing a UI framework.”

Changing a constraint requires appropriate approval. Keep constraints visible in project documentation and Codex instructions.

---

## 22. Assumptions

An assumption is an unverified premise used for planning.

Example: “A test device will be available during integration testing.”

Track assumptions so they can be validated before they become critical dependencies:

| ID | Assumption | Validation needed |
|---|---|---|
| A-001 | Test device will be available | Confirm access |
| A-002 | API credentials can be obtained | Verify provider access |
| A-003 | Sample data reflects the required workflow | Validate with project owner |

Do not present unverified assumptions as confirmed requirements.

---

## 23. Dependencies

Dependencies are internal or external systems, resources, or decisions required by the project. Examples include an API contract, database, test account, device, dataset, map service, or stakeholder approval.

Record the owner, status, and impact of each critical dependency. A missing dependency may block implementation or require an approved mock/stub strategy; it should not be silently hidden.

---

## 24. Risk Analysis

A project risk is an uncertain event or condition that could affect project objectives. Assess its probability and impact, then prioritize mitigation.

| Risk | Probability | Impact | Priority |
|---|---|---|---|
| Missing API access | Medium | High | High |
| Minor UI adjustment | High | Low | Low |
| Database integration failure | Medium | High | High |
| Documentation delay | Medium | Medium | Medium |

Risk priority depends on project context. Common responses include:

- **Avoid:** remove an unnecessary risky activity.
- **Reduce:** add safeguards that lower likelihood or impact.
- **Transfer:** assign appropriate responsibility to a qualified provider or party.
- **Accept:** document and monitor a risk within approved tolerance.

Example: if an external API may be delayed, use approved mock data for development while retaining a requirement to verify the real integration before acceptance. Do not report a missing integration as complete.

---

# Part VIII — Scope Quality

## 25. Avoid Overengineering

Overengineering adds more complexity than the requirements justify. Examples include microservices for a small MVP, unnecessary abstraction layers, libraries for trivial functionality, speculative future features, or complex state management without a demonstrated need.

For a simple task application, a straightforward application architecture may be sufficient. Choose the least complex design that meets requirements while preserving correctness, security, and maintainability.

### YAGNI

**YAGNI** means “You Aren't Gonna Need It.” Avoid implementing a feature solely because it might be useful later.

### KISS

**KISS** means “Keep It Simple, Stupid.” Prefer straightforward solutions when they satisfy the requirements.

Neither principle justifies cutting necessary quality safeguards.

---

# Part IX — Preparing Requirements for Codex

## 26. Why Codex Needs Approved Requirements

Codex can analyze and implement approved functionality. It should not be expected to invent stakeholder requirements, business rules, permissions, success metrics, constraints, or exclusions. Mark unresolved decisions explicitly instead of allowing assumptions to become features.

```text
Approved problem → objectives → MVP scope → requirements
                 → acceptance criteria → implementation plan
                 → Codex development → verification
```

**Codex should implement requirements, not silently create product requirements.**

---

## 27. Codex Planning Prompt

```text
Task:
Analyze the proposed software project.

Read the provided problem statement, requirements,
and scope constraints. Do not implement code.

Evaluate:
1. Requirement clarity.
2. MVP completeness.
3. Missing dependencies.
4. Technical feasibility.
5. Scope conflicts.
6. Major risks.
7. Testability of acceptance criteria.

Do not invent stakeholder decisions. Distinguish:
- Confirmed requirements
- Assumptions
- Unresolved questions
- Optional recommendations

Return a structured planning report.
```

### Scope Review Prompt

```text
Review the current project scope.

Identify features that do not directly support
the approved MVP objectives. Check whether functional
requirements have observable acceptance criteria.

Identify potential overengineering.
Do not add features or modify application code.
Recommend the smallest coherent scope that delivers
the intended user value.
```

---

# Part X — Scope Documentation

## 28. Choose a Proportionate Documentation Structure

A small project may use a single `docs/project-scope.md`. A larger project may separate overview, requirements, scope, architecture, and testing documentation. These are options, not mandatory files; do not split a simple document without a maintenance reason.

---

## 29. Project Scope Document Template

```markdown
# Project Scope Document

## Project Information
Project name, type, target platform, owner, team, status.

## Problem Statement
Describe the real-world problem.

## Target Users and Stakeholders
Identify intended users and decision makers.

## Project Objectives
List clear, measurable outcomes.

## MVP Definition
Describe the smallest complete workflow that delivers value.

## Functional Requirements
| ID | Requirement | Priority |
|---|---|---|

## Non-Functional Requirements
| ID | Category | Requirement | Verification |
|---|---|---|---|

## In-Scope Features
List approved functionality.

## Out-of-Scope Features
List explicit exclusions.

## Technology Stack
Frontend, backend, database, tools, testing.

## Constraints, Assumptions, and Dependencies
Record mandatory limits and unresolved premises.

## Risks
| Risk | Probability | Impact | Mitigation |
|---|---|---|---|

## Acceptance Criteria and Verification
Define observable success and evidence.

## Approval and Unresolved Questions
Record decisions and required follow-up.
```

---

# Part XI — Project Examples

## 30. GroupTrack Android

**Project type:** Native Android student project management application.
**Technology:** Android Studio, Java, XML layouts, Firebase.

**Problem example:** Student groups may struggle to organize assignments and monitor project contributions when coordination is scattered across messaging platforms.

The project's MVP must come from its approved requirements. Do not replace an established MVP with a newly invented feature list.

### Scope Review Questions

1. Does each feature support the stated problem?
2. Does the MVP provide a coherent workflow?
3. Are user permissions clear?
4. Are Firebase operations necessary and feasible?
5. Can it be tested on an emulator or device?
6. Are unnecessary libraries avoided?
7. Are UI and business logic responsibilities clear?

Example acceptance criterion:

```text
Given a user with valid credentials
When the user signs in successfully
Then the application navigates to the authorized screen
```

Potential risks include Firebase configuration, authentication integration, device compatibility, unclear role permissions, and scope expansion. Preserve documented project scope rather than designing around speculative features.

---

## 31. GoPasig Laravel

**Project type:** Web-based fleet management and GPS bus tracking system.
**Technology:** Laravel, PHP, relational database, mapping integration, web frontend.

Potential objectives may include bus location visibility, ETA information, dispatch monitoring, or operational reports. Align these with approved documentation and stakeholder requirements.

### Dependencies and Risks

| Dependency or risk | Potential impact | Mitigation example |
|---|---|---|
| Incomplete route data | Incorrect map behavior | Validate route information |
| GPS inaccuracies | Unreliable location reports | Document accuracy limitations |
| Network interruptions | Delayed updates | Define degraded behavior |
| Scheduling conflicts | Operational errors | Validate business rules |
| API quota limits | Service interruptions | Review service usage and limits |

Do not claim real-time tracking or ETA accuracy without suitable verification evidence.

---

## 32. Next.js and Laravel E-commerce

```text
Next.js frontend → Laravel REST API → PostgreSQL
```

Possible roles include customer, staff, and administrator. Define permissions explicitly rather than assuming roles from the application category.

An illustrative MVP may include authentication, product listing, product details, basic inventory management, and order creation. Actual scope depends on approved requirements. Recommendation engines, loyalty programs, or marketplace integrations may be deferred.

For separate frontend/backend applications, define:

- Endpoints and request/response formats.
- Authentication and authorization behavior.
- Error responses and validation.
- Data contracts and supported environments.

Example requirement:

> FR-010: The frontend shall display product data returned by the approved Laravel products endpoint.

Example acceptance criterion:

```text
Given that the products API returns a valid product list
When a customer opens the product listing page
Then the frontend displays product information
according to the agreed API contract
```

Consider contract mismatch, CORS, authentication coordination, schema changes, and integration tests as project risks.

---

# Part XII — Requirements Traceability and Readiness

## 33. Requirements Traceability

Traceability links objectives to requirements, features, acceptance criteria, implementation, and test evidence.

```text
Project objective → functional requirement → acceptance criterion
                  → implementation → test evidence
```

| Requirement | Feature | Acceptance criterion | Verification |
|---|---|---|---|
| FR-001 | Sign-in | Valid credentials allow access | Authentication test |
| FR-002 | Task creation | Valid task is saved | Creation test |
| FR-003 | Task listing | Assigned tasks are displayed | Listing test |
| FR-004 | Status update | Valid status is persisted | Update test |

Every essential MVP requirement should have a corresponding verification approach.

---

## 34. Project Readiness Assessment

A project is ready for implementation planning when its essential requirements and constraints are sufficiently clear—not when every minor detail is known.

- [ ] Problem is clearly defined.
- [ ] Target users and stakeholders are identified.
- [ ] Objectives are documented.
- [ ] MVP supports a coherent core workflow.
- [ ] Functional and relevant non-functional requirements are defined.
- [ ] In-scope and out-of-scope features are explicit.
- [ ] Critical dependencies are identified.
- [ ] Technology choices are justified.
- [ ] Critical assumptions are validated or tracked.
- [ ] Major risks and mitigations are documented.
- [ ] Acceptance criteria are testable.
- [ ] Verification methods are identified.
- [ ] Unresolved decisions are visible.
- [ ] Scope is approved.

Record critical unknowns as unresolved rather than treating them as settled decisions.

---

# Part XIII — Common Mistakes

## 35. Mistake: Choose a Project Only for Its Technology

**Problem:** The team selects a stack before understanding the user problem.

**Improvement:** Define the problem and requirements first.

## 36. Mistake: Make the MVP Too Broad

**Problem:** Too many features delay delivery and increase verification effort.

**Improvement:** Identify the smallest complete workflow that provides core value.

## 37. Mistake: Leave Exclusions Unwritten

**Problem:** Related capabilities become implicit requirements.

**Improvement:** List out-of-scope work explicitly.

## 38. Mistake: Use Vague Requirements

**Problem:** No one can objectively decide whether behavior is complete.

**Improvement:** Describe observable system behavior and conditions.

## 39. Mistake: Ignore Quality Requirements or Dependencies

**Problem:** The feature works in a narrow case but misses security, reliability, test, or integration needs.

**Improvement:** Identify relevant quality constraints and required resources before implementation.

## 40. Mistake: Treat Codex as Product Owner

**Problem:** The agent independently decides what the application should do.

**Improvement:** Use Codex to analyze and implement requirements; preserve human approval over product decisions.

---

# Part XIV — Practical Exercises

> These exercises are optional follow-up work. This lesson's current learning mode is theory first.

## 41. Exercise A — Evaluate Candidate Projects

Select three project ideas. Define each problem and users, evaluate technical and schedule feasibility, learning value, and testability, then compare them with a weighted matrix. Explain which project you would choose and why.

## 42. Exercise B — Write a Problem Statement

Use the target users, current situation, difficulty, consequences, and desired improvement template. Describe the problem before naming a technology.

## 43. Exercise C — Define an MVP

Identify the core user goal, necessary actions, essential features, and optional features. Explain why the remaining scope forms a complete workflow.

## 44. Exercise D — Write Requirements

Create five functional requirements with IDs, priorities, and observable outcomes. Add relevant non-functional requirements and verification approaches.

## 45. Exercise E — Acceptance Criteria

Write Given–When–Then criteria for a successful workflow, a validation failure, and an unauthorized access attempt. Avoid inventing permissions or business rules.

## 46. Exercise F — Feasibility and Technology

Assess technical, operational, schedule, and resource feasibility. Compare two technology approaches using requirements fit, team expertise, testability, and maintenance cost.

## 47. Exercise G — Risk Register

Identify at least five risks and document probability, impact, mitigation, and owner:

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|

## 48. Exercise H — Scope Creep Review

A task-management MVP receives requests for real-time chat, AI recommendations, calendar integration, and analytics. Decide what is essential, what can be deferred, what dependencies would be introduced, and which approval is needed.

## 49. Exercise I — Codex Requirements Review

Write a planning-only Codex prompt that checks clarity, feasibility, risks, scope conflicts, and acceptance-criteria testability. Require the agent to distinguish confirmed requirements, assumptions, unresolved questions, and optional recommendations. Do not let it implement code or invent stakeholder decisions.

## 50. Exercise J — Project Readiness

Prepare a short readiness report:

```markdown
# Project Readiness Review

## Confirmed Requirements
## Approved MVP Scope
## Technical Feasibility
## Dependencies
## Major Risks
## Unresolved Questions
## Recommendation
Ready / Conditionally Ready / Blocked
```

---

# Part XV — Knowledge Review

## 51. Knowledge Check

1. What is project selection?
2. Why evaluate a project before development?
3. What is a problem statement?
4. How does a problem differ from a solution?
5. Who are stakeholders?
6. What is a project objective?
7. What does SMART mean?
8. What is an MVP, and why is it not merely an incomplete product?
9. What is project scope?
10. Why document in-scope and out-of-scope functionality?
11. What is scope creep, and how should changes be controlled?
12. What are functional requirements?
13. What are non-functional requirements?
14. What is a user story?
15. What are acceptance criteria?
16. How does Given–When–Then help?
17. How do acceptance criteria differ from test cases?
18. What is Definition of Done?
19. What are technical, operational, schedule, and resource feasibility?
20. How should technology be selected?
21. What is a constraint?
22. What is an assumption, and why track it?
23. What is a project dependency?
24. How do probability and impact help assess risk?
25. What is risk mitigation?
26. What is overengineering?
27. What do YAGNI and KISS mean?
28. Why should Codex not invent stakeholder decisions?
29. What is requirements traceability?
30. What makes a project ready for implementation planning?

---

## 52. Key Terminologies

| Term | Definition |
|---|---|
| Project Selection | Choosing a project based on defined criteria |
| Project Evaluation | Assessing a project's suitability and feasibility |
| Problem Statement | Description of the problem to solve |
| Stakeholder | Person or organization affected by or influencing the system |
| Project Objective | Intended project outcome |
| SMART | Specific, Measurable, Achievable, Relevant, Time-bound |
| MVP | Minimum Viable Product; smallest coherent product delivering core value |
| Project Scope | Boundaries of approved work |
| In Scope | Functionality included in the approved work |
| Out of Scope | Functionality explicitly excluded from the current work |
| Scope Creep | Uncontrolled expansion of project requirements |
| Functional Requirement | Expected system behavior |
| Non-Functional Requirement | Quality attribute or operating constraint |
| User Story | User-centered statement of a need and benefit |
| Acceptance Criteria | Conditions required for a feature to be accepted |
| Test Case | Procedure used to verify expected behavior |
| Definition of Done | Shared quality and completion conditions |
| Feasibility | Practicality of delivering a project under its constraints |
| Constraint | Mandatory project limitation |
| Assumption | Unverified premise used for planning |
| Dependency | Resource, service, or decision required for project work |
| Project Risk | Uncertainty that may affect objectives |
| Risk Mitigation | Action intended to reduce risk likelihood or impact |
| Requirements Traceability | Links among requirements, implementation, and verification |
| Scope Approval | Authorization of project boundaries and changes |

---

## 53. Key Takeaways

1. Start project selection with a meaningful user problem.
2. Choose technology after understanding the requirements.
3. An idea is not a complete project definition.
4. Stakeholders help establish real requirements.
5. Objectives should describe outcomes, not technology choices.
6. An MVP must deliver a coherent core workflow.
7. Minimum scope does not mean minimum quality.
8. Make scope boundaries and exclusions explicit.
9. Functional requirements describe behavior; non-functional requirements describe quality and constraints.
10. Acceptance criteria make requirements testable; tests provide evidence.
11. Assess feasibility and external dependencies before implementation.
12. Track assumptions and risks rather than presenting them as facts.
13. Avoid speculative features and unjustified architecture.
14. Codex should analyze and implement approved requirements, not invent product decisions.
15. Link each essential requirement to verification evidence.
16. Project readiness requires clarity on critical decisions, not perfect certainty.
17. The best scope is the smallest approved scope that delivers and verifies the intended value.

---

## 54. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
3. [Using Goals in Codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)
4. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
5. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)

### Requirements and Software Engineering

6. [IEEE Computer Society — SWEBOK](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
7. [ISO/IEC/IEEE 29148 — Requirements Engineering](https://www.iso.org/standard/72089.html)
8. [Agile Alliance — Agile Glossary](https://agilealliance.org/agile101/agile-glossary/)
9. [Scrum Guide](https://scrumguides.org/scrum-guide.html)
10. [Agile Manifesto](https://agilemanifesto.org/)

### Security and Technology Documentation

11. [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
12. [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
13. [Android Developers](https://developer.android.com/)
14. [Laravel Documentation](https://laravel.com/docs)
15. [Next.js Documentation](https://nextjs.org/docs)
16. [TypeScript Documentation](https://www.typescriptlang.org/docs/)
17. [Firebase Documentation](https://firebase.google.com/docs)
18. [PostgreSQL Documentation](https://www.postgresql.org/docs/)

> **Documentation Note:** Requirements, feasibility, and technology decisions depend on project context. Framework documentation describes available capabilities; project owners and stakeholders decide which capabilities are required.

---

## 55. Lesson Completion Checklist

- [ ] Understand project selection and evaluation.
- [ ] Write a problem statement and identify stakeholders.
- [ ] Define objectives and understand SMART objectives.
- [ ] Define an MVP with a coherent workflow.
- [ ] Identify in-scope and out-of-scope functionality.
- [ ] Understand scope change control.
- [ ] Write functional and non-functional requirements.
- [ ] Create user stories and acceptance criteria.
- [ ] Understand Given–When–Then and Definition of Done.
- [ ] Evaluate project feasibility and technology choices.
- [ ] Identify constraints, assumptions, and dependencies.
- [ ] Analyze risks and define practical mitigations.
- [ ] Recognize overengineering, YAGNI, and KISS.
- [ ] Prepare approved requirements for Codex.
- [ ] Understand requirements traceability and project readiness.
- [ ] Complete theoretical exercises and knowledge check.

---

# 56. Next Lesson

## [[02 - Project Setup and Context Preparation]]

The next lesson covers repository and environment preparation, Git setup, `AGENTS.md`, documentation organization, coding standards, environment configuration, dependencies, testing foundations, secrets management, and baseline verification.

### Learning Progression

```text
6.1 Project Selection and Scope Definition
                ↓
6.2 Project Setup and Context Preparation
                ↓
6.3 Feature Planning with Codex
                ↓
6.4 Guided Feature Implementation
                ↓
6.5 Debugging, Testing, and Quality Assurance
                ↓
6.6 Code Review and Git Workflow
                ↓
6.7 Final Project Evaluation
```

---

*Phase 6 — Real-World Application | Lesson 6.1*
*Learning Principle: Define the problem, approve the scope, identify constraints, and establish verifiable success criteria before asking an AI agent to implement a solution.*
