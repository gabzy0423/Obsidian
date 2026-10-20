# 05 — Debugging, Testing, and Quality Assurance

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 6 — Real-World Application
> **Lesson:** 6.5 — Debugging, Testing, and Quality Assurance
> **Difficulty:** Intermediate to Advanced
> **Learning Mode:** Theory First
> **Prerequisite:** [[04 - Guided Feature Implementation]]
> **Next Lesson:** [[06 - Code Review and Git Workflow]]
> **Status:** Theory Documented

---

# 1. Introduction

Writing code is only one part of software development. A feature may compile successfully but still contain incorrect behavior, security weaknesses, performance problems, or integration failures.

Software development therefore needs three related practices:

1. **Debugging** — Investigating and correcting defects.
2. **Testing** — Executing checks to determine whether software behaves as expected.
3. **Quality Assurance (QA)** — Establishing processes that help prevent defects and evaluate quality.

OpenAI Codex can assist with all three. However, it should not be treated as an unquestionable authority on whether an application works. Developers must evaluate the evidence behind its conclusions.

## Core Principle

**A feature is not complete merely because Codex generated code or a build succeeded. Completion requires appropriate verification against approved requirements.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Distinguish debugging, testing, and quality assurance.
2. Understand the software defect lifecycle and classify common errors.
3. Reproduce defects and investigate logs and stack traces.
4. Apply root cause analysis and distinguish symptoms from causes.
5. Understand unit, integration, component, UI, and end-to-end testing.
6. Apply regression testing and evaluate test coverage.
7. Understand test doubles, flaky tests, and static analysis.
8. Apply basic security and performance validation.
9. Use Codex for debugging and test generation, and evaluate its results.
10. Design quality gates and report verification accurately.
11. Apply these practices to Android, Laravel, and Next.js.
12. Prepare a QA handoff for code review.

---

# Part II — Debugging vs Testing vs Quality Assurance

## 3. What Is Debugging?

Debugging is the systematic process of investigating a defect, determining its cause, correcting it, and verifying the correction.

If a valid registration form displays an error, investigate whether input validation, request routing, database behavior, server response, or frontend response handling caused the failure. The goal is to determine the actual cause instead of guessing.

---

## 4. What Is Software Testing?

Software testing evaluates software behavior against defined expectations.

~~~text
Input: Valid user credentials
Expected: User is authenticated
Actual: Unexpected error
Result: Test failed
~~~

Testing identifies an observed difference between expected and actual behavior. Debugging investigates why it occurred.

---

## 5. What Is Quality Assurance?

Quality assurance focuses on processes and practices that help teams deliver software meeting defined quality standards.

Examples include coding standards, requirements reviews, test planning, code review, continuous integration, security checks, release checklists, and defect tracking. QA includes preventive practices, not only defect detection.

---

## 6. Comparison

| Aspect | Debugging | Testing | Quality Assurance |
|---|---|---|---|
| Primary goal | Find and fix defects | Evaluate behavior | Improve product and process quality |
| Typical trigger | Observed failure | Planned verification | Entire development lifecycle |
| Main activity | Root cause investigation | Execute checks | Define and enforce quality practices |
| Typical output | Verified correction | Test results | Quality evidence and process improvements |
| Codex role | Investigate and propose fixes | Create and run tests | Assist with reviews and checks |

**Testing reveals problems, debugging investigates them, and quality assurance helps prevent and manage them.**

---

# Part III — Software Defects

## 7. What Is a Software Defect?

A software defect is a problem in a system that causes it to violate a requirement, expectation, or intended behavior.

Defects can originate from incorrect requirements, faulty assumptions, logic errors, integration mismatches, configuration mistakes, unexpected input, concurrency problems, or incomplete error handling.

---

## 8. Common Error Categories

| Error Type | Description | Example |
|---|---|---|
| Syntax error | Invalid language syntax | Missing delimiter |
| Compilation error | Code cannot compile | Invalid Java type |
| Type error | Incompatible types | Incorrect TypeScript assignment |
| Runtime error | Failure during execution | Null reference |
| Logic error | Incorrect result | Wrong ETA calculation |
| Validation error | Invalid input handled incorrectly | Empty required field accepted |
| Integration error | Components communicate incorrectly | Frontend expects wrong JSON shape |
| Configuration error | Incorrect environment setup | Wrong API URL |
| Database error | Data operation fails | Constraint violation |
| Authorization error | Permission rule violated | User accesses restricted record |
| Concurrency error | Incorrect behavior under overlap | Duplicate assignment |
| Performance defect | Unacceptable resource use or latency | Repeated database queries |

---

## 9. Error vs Failure vs Root Cause

These terms describe different concepts.

- **Error:** An incorrect action, assumption, or state.
- **Defect:** A flaw in software or configuration.
- **Failure:** Observed behavior that violates expectations.
- **Root cause:** The underlying condition responsible for the problem.

~~~text
Incorrect API Field Assumption
            |
            v
Frontend Accesses Wrong Property
            |
            v
Product Name Is Missing
            |
            v
User Sees Blank Product Label
~~~

The blank label is the symptom. An API contract mismatch may be the root cause.

**Correcting the visible symptom without addressing its cause may leave the defect unresolved.**

---

# Part IV — Systematic Debugging

## 10. The Debugging Lifecycle

~~~text
Observe Failure → Reproduce Defect → Collect Evidence
      → Form Hypotheses → Investigate Cause
      → Apply Minimal Fix → Verify Correction
      → Run Regression Checks → Document Outcome
~~~

---

## 11. Step 1 — Observe the Failure

Record expected and actual behavior, error messages, inputs, environment, recent changes, conditions, and frequency.

Example:

~~~markdown
# BUG-014 — Product List Fails to Load

## Expected Behavior
Products appear on the customer page.

## Actual Behavior
The page displays an error state.

## Environment
Local Next.js frontend and Laravel backend.

## Reproduction
1. Start required development services.
2. Open the product listing page.
3. Observe the failed request.

## Evidence
Browser console and network response.

## Status
Under Investigation
~~~

---

## 12. Step 2 — Reproduce the Defect

A reproducible defect is easier to investigate. Ask whether it occurs consistently, affects all users, depends on particular data, occurs only in one environment, or began after a recent change.

~~~text
Open Product Page
        |
        v
Request Products
        |
        v
Receive HTTP Response
        |
        v
Observe Incorrect Rendering
~~~

Do not modify code based only on a vague report when additional evidence can be obtained safely.

---

## 13. Step 3 — Collect Evidence

Evidence can include error output, stack traces, logs, network responses, database results, test failures, Git diffs, and configuration details.

Never expose passwords, API keys, authentication tokens, private user records, or sensitive environment variables. Sanitize diagnostic information.

---

# Part V — Logs and Stack Traces

## 14. What Is Logging?

Logging records events and diagnostic information produced by software.

| Level | Typical Meaning |
|---|---|
| DEBUG | Detailed diagnostic information |
| INFO | Normal operational event |
| WARN | Unexpected but recoverable condition |
| ERROR | Operation failure |
| FATAL / CRITICAL | Severe application or system failure |

Exact logging conventions vary by framework.

---

## 15. What Is a Stack Trace?

A stack trace records the sequence of function or method calls associated with an exception.

~~~text
NullPointerException
    at TaskService.createTask(TaskService.java:42)
    at TaskActivity.submit(TaskActivity.java:87)
~~~

It suggests where the exception occurred and how execution reached it. The reported line is not necessarily where the underlying defect originated.

---

## 16. Reading a Stack Trace

Ask:

1. What exception occurred?
2. Which application method failed?
3. Which input or state was involved?
4. Which earlier operation produced that state?
5. Is the exception caused by the current change?
6. Can it be reproduced?

**Stack traces provide investigation clues, not automatic proof of root cause.**

---

## 17. Logs by Technology

- **Android:** Android Studio Logcat, Gradle output, device/emulator logs, Firebase diagnostics.
- **Laravel:** Application logs, PHP exceptions, test output, database diagnostics.
- **Next.js:** Development server terminal, browser console, network inspector, server logs, build output.

Client-side and server-side logs can show different parts of the same failure.

---

# Part VI — Root Cause Analysis

## 18. What Is Root Cause Analysis?

Root cause analysis identifies the underlying reason a defect occurred and helps avoid repeated superficial fixes.

---

## 19. The Five Whys Technique

The Five Whys repeatedly asks why a problem occurred.

**Problem:** Products do not appear.

1. Why? The frontend receives an unexpected response.
2. Why? The response structure differs from what the frontend expects.
3. Why? The frontend uses an outdated API contract.
4. Why? The contract changed without coordinated updates.
5. Why? Cross-project contract verification was missing.

The analysis may suggest both a code correction and improved integration verification. Five Whys is a reasoning technique, not proof; complex failures can have multiple causes.

---

## 20. Hypothesis-Driven Debugging

1. Identify a possible cause.
2. Predict what evidence would support it.
3. Perform a focused check.
4. Compare the result.
5. Accept, reject, or refine the hypothesis.

Example: Hypothesis — the Laravel API returns an unexpected field name. Check the sanitized JSON response against the TypeScript type. If the fields differ, correct the contract mismatch in the responsible component.

**Debugging should be evidence-driven, not trial-and-error code generation.**

---

# Part VII — Debugging with Codex

## 21. Codex as a Debugging Assistant

Codex can analyze errors, inspect files, trace execution, identify possible causes, propose focused fixes, create regression tests, run permitted checks, and summarize findings. Its diagnosis can still be wrong.

---

## 22. Recommended Codex Debugging Workflow

~~~text
Bug Report → Relevant Evidence → Read Instructions
           → Inspect Code → Identify Causes
           → Verify Hypothesis → Propose Minimal Fix
           → Implement Approved Correction → Test and Review
~~~

---

## 23. Investigation-Only Prompt

~~~text
Task:
Investigate the reported defect.

Expected Behavior:
[Expected behavior]

Actual Behavior:
[Observed failure]

Evidence:
[Sanitized error output]

Instructions:
1. Read applicable AGENTS.md instructions.
2. Inspect relevant code.
3. Reproduce the issue if possible.
4. Identify likely causes.
5. Separate confirmed findings from hypotheses.
6. Identify affected components.
7. Propose a minimal correction.
8. Recommend regression tests.

Constraints:
- Do not modify files.
- Do not install dependencies.
- Do not change unrelated code.
- Do not expose sensitive data.

Output:
- Reproduction status
- Evidence
- Likely root cause
- Confidence and uncertainty
- Proposed fix
- Verification plan
~~~

---

## 24. Fix Implementation Prompt

~~~text
Task:
Implement the approved correction for the confirmed defect.

Root Cause:
[Confirmed finding]

Approved Correction:
[Description]

Constraints:
- Modify only necessary files.
- Preserve existing architecture and unrelated changes.
- Do not add unrelated features.
- Add a focused regression test when appropriate.

Verification:
- Run the focused test.
- Run relevant regression checks.
- Inspect the Git diff.

Report:
- Files changed
- Root cause addressed
- Tests executed and results
- Remaining limitations
~~~

Separate investigation from modification when the cause is uncertain or the affected area is high-risk.

---

# Part VIII — Software Testing Fundamentals

## 25. Testing Levels

~~~text
             /\
            / E2E\
           /------\
          / UI     \
         /Integration\
        /------------\
       / Unit Tests   \
      /----------------\
~~~

This is conceptual guidance, not a required test distribution.

---

## 26. Unit Testing

Unit tests verify a small piece of software in isolation, such as a validator, calculation, status transition, or transformation.

~~~java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class TaskProgressTest {
    @Test
    public void completedTasksCalculateCorrectPercentage() {
        int completed = 3;
        int total = 5;
        int percentage = (completed * 100) / total;
        assertEquals(60, percentage);
    }
}
~~~

This is illustrative. A real test should exercise the production method rather than duplicate the calculation. Unit tests are fast and focused but do not prove all components integrate correctly.

---

## 27. Integration Testing

Integration tests verify interactions such as controller/database, API client/backend, authentication/protected endpoint, or repository/persistence.

~~~text
HTTP Request → Laravel Route → Controller
             → Validation → Database
~~~

---

## 28. Component Testing

Component tests focus on an individual UI component: whether a button triggers a callback, an error appears, or data renders correctly. This is relevant to React and Next.js.

---

## 29. UI Testing

UI tests verify interface behavior through user interactions: entering form data, clicking buttons, navigation, validation errors, and loading or empty states.

**Verify meaningful user behavior, not merely whether an element exists.**

---

## 30. End-to-End Testing

End-to-end tests verify a complete user workflow.

~~~text
Open App → Sign In → Open Dashboard → Create Task → Verify Task
~~~

They detect cross-component problems but can be slower and more sensitive to environment instability.

---

# Part IX — Regression Testing

## 31. What Is Regression Testing?

Regression testing checks that previously working behavior still works after changes.

A schedule validation fix should not unintentionally break valid schedule creation, listing, route assignment, driver assignment, or dispatch behavior.

**Consider which working behaviors may be affected by every fix.**

---

## 32. Regression Test Strategy

1. Identify changed behavior.
2. Identify dependent components.
3. Select relevant existing tests.
4. Add a focused defect test.
5. Run broader checks proportional to risk.
6. Record results.

| Change | Focused Test | Regression Check |
|---|---|---|
| Android navigation fix | Screen navigation | Related user flow |
| Laravel validation fix | Validation test | Related API tests |
| Next.js product rendering | Component test | Product listing flow |
| Database schema change | Migration/integration test | Dependent queries |

---

# Part X — Test Design Techniques

## 33. Positive Testing

Positive testing verifies expected behavior with valid input, such as creating a product from a valid submission.

---

## 34. Negative Testing

Negative testing verifies handling of invalid or unauthorized operations, such as rejecting a submission without a required name.

---

## 35. Boundary Value Analysis

Boundary testing examines values near important limits. For a 1–100 character title, examples are 0, 1, 100, and 101 characters. Actual limits must come from approved requirements.

---

## 36. Equivalence Partitioning

Equivalence partitioning groups inputs expected to behave similarly.

| Partition | Example |
|---|---|
| Valid positive integer | 5 |
| Zero | 0 |
| Negative integer | -3 |
| Non-numeric input | "abc" |
| Missing value | null |

Expected behavior depends on real validation rules.

---

## 37. State Transition Testing

State transition testing verifies permitted and prohibited changes.

~~~text
Pending → In Progress → Completed
~~~

Test transition rules, not only the final state.

---

# Part XI — Test Doubles

## 38. What Are Test Doubles?

| Type | Purpose |
|---|---|
| Stub | Returns predefined values |
| Fake | Simplified working implementation |
| Mock | Verifies expected interactions |
| Spy | Records interactions for inspection |

---

## 39. Why Use Test Doubles?

They isolate code from external APIs, databases, networks, authentication providers, and time-dependent services. However, integration tests are still needed to verify real component interactions.

---

# Part XII — Test Coverage

## 40. What Is Test Coverage?

Coverage measures portions of code or behavior exercised by tests. Common metrics are line, statement, branch, and function coverage.

---

## 41. Coverage Example

If tests execute 8 of 10 executable lines, line coverage is 80%. That does not mean software is 80% correct; lines can run without meaningful assertions.

---

## 42. Coverage Quality

A weak test calls a function and checks only that it does not throw. A stronger test supplies known input, exercises production code, asserts the expected result, and covers relevant edges.

**Meaningful assertions and risk coverage matter more than maximizing a percentage.**

---

# Part XIII — Flaky Tests

## 43. What Is a Flaky Test?

A flaky test sometimes passes and sometimes fails without a relevant code change.

Causes include timing assumptions, shared mutable state, unstable networks, test ordering, random data, asynchronous operations, external services, and environment differences.

---

## 44. Flaky Test Example

A UI test expects network data immediately. It passes on a fast response and fails when the response takes longer because it relies on an incorrect timing assumption.

---

## 45. Improving Reliability

- Wait for observable conditions.
- Isolate test data.
- Avoid arbitrary fixed delays.
- Control time and randomness when appropriate.
- Use deterministic fixtures.
- Reset state between tests.
- Separate external integration checks from isolated tests.

Do not rerun a flaky test until it passes without investigating its instability.

---

# Part XIV — Static Analysis

## 46. What Is Static Analysis?

Static analysis examines code without executing normal application behavior. Examples include type checking, linting, quality checks, dependency analysis, and security-oriented scanning.

---

## 47. Static Analysis vs Dynamic Testing

| Static Analysis | Dynamic Testing |
|---|---|
| Examines code structure | Executes software |
| Finds some issues early | Evaluates runtime behavior |
| Type checking and linting | Unit and integration tests |
| Can produce false positives | Can miss untested paths |

They complement one another.

---

## 48. Common Commands

~~~powershell
# Android
.\gradlew.bat lint

# Laravel tests (dynamic verification)
php artisan test

# Next.js / TypeScript
npx tsc --noEmit
npm run lint
~~~

Laravel tests are not static analysis; PHPStan/Larastan or another configured analyzer is an example. The Next.js lint script must exist.

Use tools already configured before adding new quality-analysis dependencies.

---

# Part XV — Security Validation

## 49. Why Security Testing Matters

A feature can work for ordinary users while still exposing unauthorized operations or sensitive data. Security validation checks whether requirements are respected.

---

## 50. Security Areas to Review

- Authentication.
- Authorization.
- Input validation.
- Sensitive data handling.
- Secure configuration.
- Error message exposure.
- Dependency security.
- API access control.

---

## 51. Authentication vs Authorization

Authentication determines who the user is. Authorization determines what that user may do.

A signed-in user may still lack permission to modify another user's task.

**Successful authentication does not grant permission for every operation.**

---

## 52. Authorization Test Scenarios

| Scenario | Expected Behavior |
|---|---|
| Authorized user accesses permitted resource | Allowed |
| Unauthenticated user accesses protected resource | Rejected |
| User attempts prohibited operation | Rejected |
| User requests another user's restricted record | Rejected |

Exact responses should follow the application's contract.

---

## 53. Sensitive Data in Debugging

Sanitize logs, avoid exposing credentials, do not commit .env files, avoid printing tokens, use synthetic data, and limit access to diagnostic output.

Debugging must not create new security or privacy problems.

---

# Part XVI — Performance Testing

## 54. What Is Performance Testing?

Performance testing evaluates behavior under specified workloads and resource conditions, including response time, throughput, memory, CPU, query volume, network activity, and scalability.

---

## 55. Performance Testing Categories

| Type | Purpose |
|---|---|
| Load testing | Evaluate expected workload |
| Stress testing | Evaluate beyond normal workload |
| Endurance testing | Evaluate sustained operation |
| Profiling | Identify expensive code paths |
| Benchmarking | Compare measured performance |

---

## 56. Performance Baselines

Before optimizing, identify the slow operation, measured latency, conditions, target, and limiting resource.

For repeated Laravel queries, count and inspect queries, check eager loading and indexes, then compare results before and after a focused change.

**Performance optimization should be measured, not assumed.**

---

# Part XVII — Quality Gates

## 57. What Is a Quality Gate?

A quality gate is a condition that must be satisfied before work proceeds, such as relevant tests passing, a successful build, no unresolved critical defect, required security checks, verified acceptance criteria, or code review.

---

## 58. Example Quality Gate Matrix

| Gate | Requirement |
|---|---|
| Build | Successful |
| Focused tests | Passing |
| Regression tests | Relevant checks passing |
| Static analysis | No unacceptable findings |
| Security review | No unresolved critical issue |
| Acceptance criteria | Verified |
| Scope review | No unapproved changes |
| Documentation | Updated where needed |

Scale gates to project risk.

---

## 59. Quality Gate Workflow

~~~text
Feature Implemented → Focused Verification → Regression Checks
                    → Static/Security Review → Acceptance Review
                               |
                          Pass / Fail
~~~

If checks fail, fix and reverify before proceeding.

---

# Part XVIII — Android Example: GroupTrack

## 60. Project Context

GroupTrack is a native Android application using Java, XML layouts, Android Studio, and Firebase. Testing should follow the approved MVP and existing architecture.

---

## 61. Example Defect — Sign-In Screen

**Observed:** The layout becomes difficult to use when the keyboard opens.

Investigate by reproducing on a device/emulator, checking affected screen sizes, inspecting XML constraints and window behavior, identifying clipped content, and evaluating a minimal correction.

Verify normal rendering, keyboard visibility, focus, navigation, and supported screen sizes.

---

## 62. Android Testing Structure

~~~text
app/src/
├── main/
│   ├── java/
│   └── res/
├── test/
│   └── java/
└── androidTest/
    └── java/
~~~

test usually contains local JVM tests. androidTest usually contains tests requiring an Android device or emulator.

---

## 63. Android Commands

~~~powershell
.\gradlew.bat test
.\gradlew.bat lint
.\gradlew.bat assembleDebug
.\gradlew.bat connectedAndroidTest
~~~

Actual tasks depend on modules, variants, and configured testing infrastructure.

---

## 64. GroupTrack QA Checklist

- [ ] App builds successfully.
- [ ] Splash screen behaves as specified.
- [ ] Sign-in and create-account screens render correctly.
- [ ] Navigation works.
- [ ] Form validation matches requirements.
- [ ] Firebase errors are handled.
- [ ] UI is usable on supported devices.
- [ ] No unrelated MVP features were added.
- [ ] Relevant tests and manual checks are documented.

---

# Part XIX — Laravel Example: GoPasig

## 65. Project Context

GoPasig may include routes, stops, drivers, buses, schedules, trips, GPS data, and administrative operations. Scheduling and assignment defects can affect multiple modules.

---

## 66. Example Defect — Conflicting Assignment

Hypothetical failure: overlapping scheduling requests assign the same driver in violation of a business rule.

Investigate controlled test data, validation, transactions, concurrency-sensitive operations, the approved rule, and a focused regression test.

Do not assume adding a transaction or lock automatically resolves every concurrency issue. Evaluate actual database behavior and execution paths.

---

## 67. Laravel Test Example

~~~php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class ScheduleValidationTest extends TestCase
{
    public function test_schedule_endpoint_requires_authentication(): void
    {
        $response = $this->postJson('/api/schedules', []);
        $response->assertUnauthorized();
    }
}
~~~

This is illustrative. It assumes the route exists and returns HTTP 401. Adapt it to actual middleware and API contracts.

---

## 68. Laravel Commands

~~~powershell
php artisan test
php artisan test --filter=ScheduleValidationTest
php artisan route:list
~~~

A route listing confirms registration, not correct authorization or business behavior.

---

## 69. GoPasig QA Matrix

| Area | Important Verification |
|---|---|
| Route management | Valid route and stop relationships |
| Scheduling | Approved conflict rules |
| Driver assignment | Authorization and availability |
| Bus assignment | Valid status and assignment |
| GPS tracking | Valid location data handling |
| ETA calculation | Expected calculation behavior |
| Admin dashboard | Correct data and permissions |
| Reporting | Accurate aggregation and filtering |

---

# Part XX — Next.js and Laravel Example: E-commerce

## 70. Project Architecture

~~~text
Next.js Frontend → Laravel REST API → PostgreSQL
~~~

A defect may originate in any layer or at an interface.

---

## 71. Example Defect — Product Listing

**Observed:** The backend returns products, but the frontend displays an empty list.

Possible causes: wrong API URL, response mismatch, authentication/authorization failure, incorrect TypeScript assumptions, rendering logic, or an unexpected empty state.

Inspect the request, status, sanitized response, API contract, frontend mapping, controlled data, then make the minimal correction.

---

## 72. Next.js Testing Strategy

- **Unit:** utilities and isolated logic.
- **Component:** product cards, forms, interface states.
- **Integration:** data fetching with UI components.
- **End-to-end:** complete browser workflows.

---

## 73. Example Component Test

~~~tsx
import { render, screen } from "@testing-library/react";
import { expect, test } from "vitest";
import ProductCard from "./ProductCard";

test("displays the product name", () => {
  render(
    <ProductCard product={{ id: 1, name: "Sample Product" }} />
  );
  expect(screen.getByText("Sample Product")).toBeDefined();
});
~~~

This assumes the shown prop shape and configured Vitest and React Testing Library. Adapt it to the real component.

---

## 74. Next.js Verification Commands

~~~powershell
npx tsc --noEmit
npm run lint
npm run build
npm run test
~~~

lint and test require configured scripts. Do not assume every Next.js project has a test runner.

---

## 75. Full-Stack Integration Checklist

- [ ] Correct API base URL.
- [ ] API contract matches frontend types.
- [ ] Backend service is reachable.
- [ ] Authentication works as specified.
- [ ] Backend enforces authorization.
- [ ] Loading, error, and empty states are correct.
- [ ] Product data renders accurately.
- [ ] Database operations use the intended environment.
- [ ] No sensitive values are exposed to the browser.
- [ ] Relevant tests pass.

---

# Part XXI — AI-Generated Test Quality

## 76. Can Codex Generate Incorrect Tests?

Yes. Generated tests can use incorrect expectations, invent requirements, omit meaningful assertions, over-mock, couple too tightly to implementation, use incorrect setup, or always pass.

---

## 77. Test Review Questions

1. Which requirement does the test verify?
2. Is the expected result correct?
3. Does it exercise production code?
4. Would it fail if behavior were broken?
5. Does it depend unnecessarily on external services?
6. Is it deterministic?
7. Does it cover relevant edges?
8. Is it maintainable?

**A generated test is useful evidence only when it meaningfully checks intended behavior.**

---

## 78. Mutation Testing Concept

Mutation testing introduces controlled code changes and checks whether tests detect them.

~~~java
// Original
return quantity > 0;

// Mutation
return quantity >= 0;
~~~

If zero is prohibited, a strong test should detect the change. Mutation testing is informative but not mandatory for small MVP projects.

---

# Part XXII — QA Reporting

## 79. Why QA Reports Matter

A QA report records verified, failed, unexecuted, limited, unresolved, and blocked checks.

---

## 80. QA Report Template

~~~markdown
# Feature QA Report

## Feature Information
Feature:
Requirement ID:
Environment:
Date:

## Scope
Describe functionality evaluated.

## Test Results
| Test ID | Scenario | Result | Evidence |
|---|---|---|---|

## Build and Static Analysis
| Check | Result |
|---|---|

## Defects
| Defect ID | Severity | Status |
|---|---|---|

## Security Considerations
Record relevant findings.

## Performance Considerations
Record measurements, if applicable.

## Unverified Areas
List checks not executed.

## Acceptance Criteria
| Criterion | Status |
|---|---|

## Final Assessment
Ready for Review / Needs Fixes / Blocked
~~~

---

## 81. Reporting Terminology

| Status | Meaning |
|---|---|
| Passed | Executed and met expectations |
| Failed | Executed and did not meet expectations |
| Not Run | Not executed |
| Blocked | Could not execute due to a dependency |
| Inconclusive | Evidence insufficient |
| Not Applicable | Outside evaluated scope |

**Never describe an unexecuted test as passed.**

---

# Part XXIII — Common Mistakes

## 82. Fixing Symptoms Instead of Causes

Problem: Visible error disappears, underlying defect remains.
Improvement: Investigate root cause.

## 83. Debugging Without Reproduction

Problem: The fix cannot be confirmed.
Improvement: Establish reproducible conditions where possible.

## 84. Trusting Codex's First Diagnosis

Problem: Plausible explanations can be wrong.
Improvement: Validate hypotheses against evidence.

## 85. Testing Only Successful Scenarios

Problem: Invalid input and failures remain unchecked.
Improvement: Include negative and boundary cases.

## 86. Over-Mocking

Problem: Tests pass while integrations fail.
Improvement: Balance isolated and integration tests.

## 87. Chasing Coverage Percentages

Problem: Code runs without meaningful assertions.
Improvement: Prioritize behavior and risk.

## 88. Ignoring Flaky Tests

Problem: Results become unreliable.
Improvement: Investigate nondeterminism.

## 89. Treating Build Success as QA Completion

Problem: Functional and security defects remain.
Improvement: Verify acceptance criteria and workflows.

## 90. Adding Unnecessary Testing Frameworks

Problem: Tooling grows without benefit.
Improvement: Reuse configured tools.

## 91. Misreporting Verification

Problem: Reviewers receive inaccurate quality evidence.
Improvement: Record actual commands, outcomes, and limits.

---

# Part XXIV — Practical Exercises

> Exercises are optional. The current learning objective remains theoretical understanding.

## 92. Exercise A — Error Classification

Classify ten failures as syntax, runtime, logic, validation, integration, configuration, database, authorization, concurrency, or performance. Deliver an error classification table.

## 93. Exercise B — Bug Report

Prepare a defect report for a screen that fails to load. Include expected/actual behavior, environment, reproduction, evidence, and severity.

## 94. Exercise C — Root Cause Analysis

Analyze a hypothetical API integration failure using Five Whys, hypothesis testing, and evidence evaluation. Deliver a root cause report.

## 95. Exercise D — Codex Debugging Prompt

Write an investigation-only prompt that reads instructions, reproduces the issue, inspects evidence, separates facts from assumptions, avoids modifications, and proposes a minimal fix.

## 96. Exercise E — Unit Test Design

Choose a business rule and define valid, invalid, and boundary tests.

## 97. Exercise F — Integration Test Design

Choose Android/Firebase, Laravel/PostgreSQL, or Next.js/Laravel. Identify expected interactions and failures.

## 98. Exercise G — Regression Matrix

Choose a bug fix and identify affected behavior, dependent modules, existing tests, and additional regression tests.

## 99. Exercise H — Test Coverage Review

Assess a project with high line coverage but weak assertions. Explain why coverage is insufficient and how to improve tests.

## 100. Exercise I — Flaky Test Analysis

For an intermittent failure, identify causes, required evidence, and stabilization strategies.

## 101. Exercise J — Security Checklist

Prepare a role-based application QA checklist covering authentication, authorization, validation, sensitive data, and API access.

## 102. Exercise K — Performance Baseline

Define an evaluation plan with operation, metric, environment, workload, baseline, and acceptance threshold.

## 103. Exercise L — Android QA Plan

Plan screen checks for UI, navigation, keyboard, device compatibility, Firebase failure handling, and build verification.

## 104. Exercise M — Laravel QA Plan

Plan schedule validation checks for business rules, authorization, transactions, concurrency, regressions, and data integrity.

## 105. Exercise N — Next.js QA Plan

Plan product-listing checks for API contract, rendering, loading, empty states, errors, authorization, and integration.

## 106. Exercise O — Final QA Report

Create a report for a completed hypothetical feature, including tests, results, defects, acceptance criteria, unverified areas, and recommendation.

---

# Part XXV — Knowledge Review

## 107. Knowledge Check

1. What are debugging, testing, and QA, and how do they differ?
2. What are defects, failures, symptoms, and root causes?
3. Why reproduce defects and structure bug reports?
4. What are logs and stack traces, and why sanitize logs?
5. What are Five Whys and hypothesis-driven debugging?
6. How can Codex help, and why verify its diagnosis?
7. What are unit, integration, component, UI, and E2E tests?
8. What are regression, positive, and negative testing?
9. What are boundary analysis, equivalence partitioning, and state transition testing?
10. What are test doubles and when are they useful?
11. What is coverage, and why is high coverage insufficient?
12. What is a flaky test and what causes it?
13. What is static analysis and how does it differ from dynamic tests?
14. Which security checks belong in feature QA?
15. What is performance testing and why establish a baseline?
16. What is a quality gate?
17. How should AI-generated tests be reviewed?
18. What is mutation testing?
19. What belongs in a QA report?
20. What is the difference between Passed and Not Run?
21. How do Android local and instrumented tests differ?
22. Why is Laravel concurrency testing important for scheduling?
23. Why does a Next.js build not prove API integration correctness?
24. When is a feature ready for code review?

---

## 108. Key Terminologies

| Term | Definition |
|---|---|
| Debugging | Investigating and correcting defects |
| Testing | Evaluating software behavior |
| Quality Assurance | Practices supporting software quality |
| Defect | Flaw violating intended behavior |
| Failure | Observed incorrect behavior |
| Root Cause | Underlying source of a problem |
| Reproduction | Recreating a defect |
| Log | Recorded diagnostic event |
| Stack Trace | Exception-related call sequence |
| Hypothesis | Testable possible explanation |
| Unit Test | Test of isolated behavior |
| Integration Test | Test of interacting components |
| Component Test | Test of an individual UI component |
| UI Test | Test of user interface behavior |
| E2E Test | Test of a complete user workflow |
| Regression Test | Check for previously working behavior |
| Positive Test | Test using valid conditions |
| Negative Test | Test of invalid or prohibited conditions |
| Boundary Value Analysis | Testing near input limits |
| Equivalence Partitioning | Grouping inputs by expected behavior |
| State Transition Testing | Testing permitted and prohibited state changes |
| Test Double | Replacement for a dependency |
| Mock | Test double used to verify interactions |
| Stub | Test double returning predefined values |
| Coverage | Measure of exercised code or behavior |
| Flaky Test | Inconsistently passing test |
| Static Analysis | Analysis without normal program execution |
| Dynamic Testing | Testing through execution |
| Load Testing | Testing under expected workload |
| Quality Gate | Required condition before proceeding |
| Mutation Testing | Evaluating tests through controlled code changes |
| QA Report | Documented verification evidence |

---

# 109. Key Takeaways

1. Debugging, testing, and QA are related but distinct.
2. Defects can originate in requirements, code, integration, or configuration.
3. A visible failure is not necessarily the root cause.
4. Reproduce defects whenever possible and let evidence guide decisions.
5. Logs and stack traces are diagnostic tools.
6. Codex diagnoses require verification.
7. Unit tests verify focused behavior; integration tests verify interactions.
8. UI tests verify user-facing behavior; E2E tests verify full workflows.
9. Regression tests protect existing functionality.
10. Negative and boundary tests reveal important defects.
11. Test doubles support isolation but do not replace integration tests.
12. Coverage percentages do not guarantee correctness.
13. Flaky tests reduce confidence and should be investigated.
14. Static analysis complements runtime testing.
15. Security validation is part of software quality.
16. Performance work should begin with measurement.
17. Quality gates establish completion expectations.
18. AI-generated tests need meaningful assertions and review.
19. Android, Laravel, and Next.js require different verification approaches.
20. Reports must distinguish passed, failed, and unexecuted checks.

---

# 110. Official References

## OpenAI Codex

1. [OpenAI Codex Documentation](https://developers.openai.com/codex/)
2. [Codex CLI](https://developers.openai.com/codex/cli/)
3. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
4. [Codex Security](https://developers.openai.com/codex/security/)

## Android

5. [Android Testing Documentation](https://developer.android.com/training/testing)
6. [Fundamentals of Testing Android Apps](https://developer.android.com/training/testing/fundamentals)
7. [What to Test in Android](https://developer.android.com/training/testing/fundamentals/what-to-test)
8. [Android Testing Strategies](https://developer.android.com/training/testing/fundamentals/strategies)

## Laravel

9. [Laravel 12 — Testing](https://laravel.com/docs/12.x/testing)
10. [Laravel 12 — HTTP Tests](https://laravel.com/docs/12.x/http-tests)
11. [Laravel 12 — Database Testing](https://laravel.com/docs/12.x/database-testing)
12. [Laravel 12 — Logging](https://laravel.com/docs/12.x/logging)

## Next.js

13. [Next.js Testing Overview](https://nextjs.org/docs/app/guides/testing)
14. [Next.js Testing with Vitest](https://nextjs.org/docs/app/guides/testing/vitest)
15. [Next.js Testing with Jest](https://nextjs.org/docs/app/guides/testing/jest)
16. [Next.js Testing with Playwright](https://nextjs.org/docs/app/guides/testing/playwright)
17. [Next.js Testing with Cypress](https://nextjs.org/docs/app/guides/testing/cypress)

## Testing and Security

18. [JUnit 4 Documentation](https://junit.org/junit4/)
19. [PHPUnit Documentation](https://docs.phpunit.de/)
20. [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
21. [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard/)
22. [Git Documentation](https://git-scm.com/docs)

> **Documentation Note:** Tools, commands, and framework features vary by version. Inspect the installed toolchain and current official documentation before execution.

---

# 111. Lesson Completion Checklist

- [ ] Understand debugging, testing, and quality assurance.
- [ ] Distinguish defects, failures, symptoms, and root causes.
- [ ] Classify common errors and reproduce defects.
- [ ] Prepare bug reports and interpret logs/stack traces.
- [ ] Apply root cause and hypothesis-driven analysis.
- [ ] Understand Codex-assisted investigation and fixing.
- [ ] Understand unit, integration, component, UI, and E2E testing.
- [ ] Understand regression, positive, negative, and boundary testing.
- [ ] Understand equivalence partitioning and state transitions.
- [ ] Understand test doubles and coverage limitations.
- [ ] Recognize and investigate flaky tests.
- [ ] Understand static analysis and security validation.
- [ ] Understand performance testing and quality gates.
- [ ] Review AI-generated tests.
- [ ] Apply Android, Laravel, and Next.js verification.
- [ ] Prepare QA reports accurately.
- [ ] Complete optional exercises and knowledge checks.

---

# 112. Next Lesson

## [[06 - Code Review and Git Workflow]]

The next lesson evaluates completed changes and manages them through Git.

Topics include code review, Codex-generated change review, status and diffs, staging, commits, branching, pull requests, conflicts, worktrees, multi-agent coordination, security and architecture review, rollback, and Android, Laravel, and Next.js examples.

~~~text
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
~~~

---

*Phase 6 — Real-World Application | Lesson 6.5*
*Learning Principle: Investigate defects systematically, test meaningful behavior, verify AI-generated changes, and base quality decisions on evidence rather than assumptions.*
