# 06 — Testing and Validation

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 3 — Agentic Coding Workflow
> **Lesson:** 3.6 — Testing and Validation
> **Difficulty:** Intermediate
> **Platform:** Windows / PowerShell / Codex CLI
> **Prerequisite:** [[05 - Debugging and Refactoring with Codex]]
> **Learning Approach:** Concepts, Android Java and Laravel examples, practical exercises, verification workflows

---

## 1. Introduction

Writing code is only one part of software development. After implementing a feature, developers must determine whether the software behaves according to its requirements.

A feature can compile successfully and still contain logical errors, invalid data handling, broken integrations, or unexpected behavior. For example:

- An Android application builds but crashes when a button is tapped.
- A Laravel API returns HTTP 200 even when invalid input is submitted.
- A login form works with valid credentials but fails to handle incorrect credentials.
- A database operation succeeds but creates duplicate records.
- A new feature works but breaks existing functionality.

These problems demonstrate why testing and validation are essential.

In agentic software development, Codex can assist with test discovery, test creation, execution, failure investigation, and verification reporting. However, Codex-generated code should not be considered correct solely because the agent reports that implementation is complete.

**Core Principle: Software changes must be evaluated through observable evidence, not assumptions or confidence statements.**

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define software testing, verification, and validation.
2. Explain the role of testing in agentic development.
3. Differentiate unit, integration, feature, and UI testing.
4. Design positive, negative, and boundary test cases.
5. Write meaningful assertions.
6. Understand test isolation and repeatability.
7. Evaluate test quality and coverage.
8. Run Android tests through Gradle.
9. Understand Android local and instrumented tests.
10. Write basic JUnit tests in Java.
11. Run Laravel tests using Artisan.
12. Differentiate PHPUnit and Pest.
13. Write Laravel unit and feature tests.
14. Understand database testing and test environments.
15. Use Codex for test planning and implementation.
16. Investigate failed tests systematically.
17. Perform regression testing.
18. Create verification reports.
19. Apply a practical Definition of Done.

## 3. What Is Software Testing?

**Software Testing** is the process of evaluating software behavior to determine whether it meets expected requirements and to discover defects. Testing can involve automated checks, manual interactions, or both.

### 3.1 Basic Example

Requirement: A calculator must correctly add two integers.

~~~text
First number: 5
Second number: 3
Expected output: 8
~~~

If the actual result is 8, the test passes for that case. If it is 2, the test fails.

### 3.2 Important Limitation

Passing one test does not prove that every possible input works. Addition should also be tested with zero, negative numbers, different operands, and relevant numerical limits.

### 3.3 Key Principle

**Tests provide evidence about the cases they evaluate, not proof that an application has no defects.**

## 4. Testing vs. Verification vs. Validation

| Concept | Main question | Example |
|---|---|---|
| Testing | Does this behavior produce the expected result? | Does task creation reject an empty title? |
| Verification | Does the implementation satisfy specified requirements? | Are approved acceptance criteria met? |
| Validation | Does the solution address the intended user need? | Can users complete the intended workflow? |

Verification checks conformance to requirements and specifications. Validation evaluates whether the system fulfills its intended purpose. These activities may overlap and happen throughout development.

~~~text
Requirements -> Implementation -> Testing
             -> Verification Against Requirements
             -> Validation Against User Needs
~~~

**Correct implementation and useful user behavior are related, but they are not identical.**

## 5. Why Testing Matters in Agentic Coding

Generated code may contain incorrect assumptions, missing validation, broken imports, invalid API contracts, incomplete edge-case handling, regressions, or unnecessary changes. Testing provides an independent way to evaluate implementation results.

~~~text
Approved Requirement -> Codex Inspects Context -> Implementation Plan
    -> Codex Implements Change -> Automated and Manual Checks
    -> Developer Reviews Evidence -> Accept, Revise, or Reject
~~~

Codex may write both implementation and tests. That does not guarantee the tests are correct. Review tests against the original requirements, not only against the generated implementation.

**A test should challenge the implementation, not simply confirm assumptions used to write it.**

## 6. The Software Testing Pyramid

~~~text
             / UI \
            /------\
           / Feature\
          /Integration\
         /-------------\
        /  Unit Tests   \
       /_________________\
~~~

The pyramid suggests many focused, inexpensive unit tests, fewer integration tests, and a targeted number of UI tests. It is guidance, not a mandatory ratio. UI tests can require devices, networks, and more setup; unit tests alone may miss database, API, navigation, and configuration problems.

**Use multiple testing levels to cover different kinds of risk.**

## 7. Unit Testing

A **Unit Test** evaluates a small, isolated unit of behavior.

~~~java
public class Calculator {
    public int add(int first, int second) {
        return first + second;
    }
}
~~~

~~~java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {
    @Test
    public void add_returnsCorrectSum() {
        Calculator calculator = new Calculator();
        int result = calculator.add(5, 3);
        assertEquals(8, result);
    }
}
~~~

This verifies the result for those inputs; it does not verify Android UI, database, or network behavior. Good unit tests are fast, deterministic, focused, readable, and explicit about expected results.

## 8. Integration Testing

**Integration Testing** evaluates how multiple components work together.

~~~text
HTTP Request -> Route -> Controller -> Validation -> Model -> Test Database
~~~

An integration test might verify that a valid request is accepted, validation succeeds, the database contains a new record, and the response matches the contract. Integration tests reveal problems that isolated unit tests may not detect.

## 9. Feature Testing

A **Feature Test** evaluates a meaningful application capability. In Laravel, feature tests commonly exercise HTTP requests and database interactions.

For task creation, test valid data, an empty title, authorization when required, and unexpected fields according to the contract.

| Unit test | Feature test |
|---|---|
| Isolated logic | Application behavior |
| Simple inputs | May exercise HTTP requests |
| Few dependencies | Multiple components |
| Fast feedback | Broader confidence |

## 10. UI Testing

**UI Testing** evaluates user interactions and interface behavior. An Android test might launch the login screen, leave required inputs empty, tap Login, verify feedback, and confirm the app does not crash.

UI tests can verify interactions, navigation, visible messages, form feedback, screen state, and accessibility behavior. They may depend on device state, animation timing, networks, or asynchronous operations. Prefer framework synchronization over arbitrary waits.

## 11. Positive Testing

**Positive Testing** verifies expected behavior with valid inputs. Valid credentials should be processed according to authentication requirements; valid task data should create a task.

## 12. Negative Testing

**Negative Testing** verifies handling of invalid inputs, failures, or unsupported conditions: empty fields, invalid email, incorrect password, missing authorization, invalid identifiers, or unavailable services.

For task creation, an empty title should be rejected with the appropriate validation response.

## 13. Boundary Value Testing

**Boundary Value Testing** checks values near accepted limits.

For a title of 1 to 100 characters:

| Input length | Expected result |
|---|---|
| 0 | Reject |
| 1 | Accept |
| 50 | Accept |
| 100 | Accept |
| 101 | Reject |

Errors often occur at limits because of incorrect comparison operators. Use actual approved limits; do not invent them.

## 14. Equivalence Partitioning

**Equivalence Partitioning** groups inputs expected to behave similarly. For integers from 1 to 10, groups are below 1, 1 through 10, above 10, and non-integers if applicable. Representative tests could use 0, 5, and 11.

## 15. Designing a Test Case

~~~markdown
## Test Case ID
TC-001
## Feature
Task Creation
## Objective
Verify required-title validation.
## Preconditions
The approved workflow is available.
## Input
Empty task title.
## Steps
1. Open task creation.
2. Leave title empty.
3. Submit the form.
## Expected Result
Submission is rejected with appropriate feedback.
## Actual Result
To be recorded after execution.
## Status
Not Run / Passed / Failed / Blocked
## Evidence
Relevant test output or observation.
~~~

An understandable, reproducible test case identifies the behavior, conditions, inputs, expected result, and success criteria.

## 16. Writing Meaningful Assertions

An **Assertion** checks whether an observed result matches an expected condition.

~~~java
assertEquals(8, calculator.add(5, 3));
~~~

~~~php
$response->assertStatus(422);
~~~

A test that only calls a method checks that it did not throw, not that its result is correct. Assertions should verify requirements.

## 17. Test Isolation

**Test Isolation** reduces unintended dependencies between tests. Each test should establish the state it needs, initialize objects, reset temporary state, and avoid relying on test order.

## 18. Deterministic and Flaky Tests

A **Deterministic Test** produces consistent results under the same conditions. A **Flaky Test** sometimes passes and sometimes fails without relevant code changes.

Common causes include timing assumptions, uncontrolled networks, shared state, unstable data, device behavior, asynchronous operations, time, and randomness. Prefer waiting for meaningful application state instead of arbitrary delays.

## 19. Mocks, Fakes, and Stubs

- **Stub:** Provides predefined responses.
- **Fake:** Provides a simplified working implementation, such as an in-memory repository.
- **Mock:** Allows verification of interactions.

They can isolate logic from networks, databases, time, external APIs, and authentication providers. Excessive mocking may test implementation details rather than behavior.

## 20. Test Coverage

**Test Coverage** describes how much code or behavior is exercised. Common measures include line, branch, and method coverage.

~~~java
if (value >= 0) {
    return "Non-negative";
}
return "Negative";
~~~

A test using only 5 misses the negative branch. Coverage can reveal untested code, but a test can execute a line without checking correctness. High coverage does not guarantee high test quality.

## 21. Mutation Testing

**Mutation Testing** evaluates tests by introducing small artificial code changes. If addition is changed to subtraction and tests still pass, assertions may be insufficient. This is an advanced technique; a new library is not necessary for every project.

## 22. Test-Driven Development

**Test-Driven Development (TDD)** writes tests before implementing behavior.

~~~text
RED: Write a test that fails for the expected reason.
GREEN: Implement the minimum required behavior.
REFACTOR: Improve structure while preserving behavior.
VERIFY: Run focused and broader relevant tests.
~~~

A meaningful red test should fail because the required behavior is missing, not because the test environment is broken.

## 23. Testing Strategy by Risk

| Change | Possible verification |
|---|---|
| Static text in an Android XML layout | Resource compilation, visual inspection, UI check |
| Add a required task field | Validation, positive/negative, persistence, UI feedback |
| Change authentication or scheduling | Unit, integration, permissions, boundaries, regression, manual review |

Testing effort should reflect consequences and complexity.

# Part II — Android Testing

## 24. Android Testing Overview

~~~text
app/src/main/java/        Application source
app/src/test/java/        Local JVM tests
app/src/androidTest/java/ Instrumented tests
~~~

Local tests commonly cover pure Java logic without Android runtime. Instrumented tests run on a device or emulator for framework, UI, or device-dependent behavior. Place tests according to their runtime needs.

## 25. Android Local Unit Tests

~~~java
package com.example.grouptrack;

public class TaskValidator {
    public boolean isValidTitle(String title) {
        return title != null && !title.trim().isEmpty();
    }
}
~~~

~~~java
package com.example.grouptrack;

import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class TaskValidatorTest {
    private final TaskValidator validator = new TaskValidator();

    @Test
    public void validTitle_returnsTrue() {
        assertTrue(validator.isValidTitle("Prepare report"));
    }

    @Test
    public void emptyTitle_returnsFalse() {
        assertFalse(validator.isValidTitle(""));
    }

    @Test
    public void whitespaceOnlyTitle_returnsFalse() {
        assertFalse(validator.isValidTitle("   "));
    }

    @Test
    public void nullTitle_returnsFalse() {
        assertFalse(validator.isValidTitle(null));
    }
}
~~~

This assumes the approved requirement is a nonblank title. Use an existing validation mechanism where available.

## 26. Android Gradle Testing Commands

~~~powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
.\gradlew.bat tasks
.\gradlew.bat test
.\gradlew.bat :app:testDebugUnitTest
.\gradlew.bat :app:assembleDebug
.\gradlew.bat connectedAndroidTest
~~~

Run a specific local test class:

~~~powershell
.\gradlew.bat :app:testDebugUnitTest --tests "com.example.grouptrack.TaskValidatorTest"
~~~

Replace the example package and class with the actual test. Confirm tasks in the project; connected tests generally require a device or emulator.

| Command | Purpose |
|---|---|
| test | Run local unit test tasks |
| :app:testDebugUnitTest | Run app debug local unit tests |
| :app:assembleDebug | Build debug APK |
| connectedAndroidTest | Run instrumented tests on connected devices |

A successful APK build is a compilation check, not behavioral testing.

## 27. Android Test Reports

Typical local test reports are under app/build/reports/tests/ and app/build/test-results/. Instrumented reports may be under app/build/reports/androidTests/ or app/build/outputs/androidTest-results/. Exact paths depend on plugin and task.

Review executed, passed, failed, and skipped counts, exceptions, and assertion failures. Inspect actual output instead of relying only on an agent summary.

## 28. Android Instrumented UI Testing

A login UI test might launch the screen, leave email empty, tap Login, verify validation feedback, and confirm no authenticated navigation occurs.

Depending on existing dependencies and architecture, tools may include AndroidX Test, Espresso, or UI Automator. Do not introduce a new library if the project has a suitable approach.

## 29. Android Example — GroupTrack Task Creation

Use documented fields and role permissions.

| Case | Condition | Expected result |
|---|---|---|
| TC-01 | Valid task data | Task created |
| TC-02 | Empty required title | Rejected |
| TC-03 | Whitespace title | Rejected if specified |
| TC-04 | Persistence fails | Appropriate error feedback |
| TC-05 | Duplicate submission | Matches approved rules |
| TC-06 | Reopen task list | Task appears if persistence succeeds |

Use unit tests for pure rules, integration tests for persistence, and UI tests for submission and feedback. With Firebase, avoid production data, use an approved test project or emulator, handle async calls, and keep real credentials out of tests.

# Part III — Laravel Testing

## 30. Laravel Testing Overview

~~~text
tests/
├── Feature/
├── Unit/
└── TestCase.php
~~~

Actual structure varies. Laravel unit tests commonly focus on isolated PHP logic; feature tests can boot Laravel and exercise HTTP, authentication, database, and other components.

## 31. PHPUnit vs. Pest

~~~php
use PHPUnit\Framework\TestCase;

class CalculatorTest extends TestCase
{
    public function test_addition(): void
    {
        $this->assertSame(8, 5 + 3);
    }
}
~~~

~~~php
test('addition returns the expected result', function () {
    expect(5 + 3)->toBe(8);
});
~~~

| Aspect | PHPUnit | Pest |
|---|---|---|
| Style | Class-based | Function-oriented |
| Assertions | PHPUnit assertions | Pest expectations and PHPUnit assertions |
| Laravel support | Yes | Yes |
| Choice | Follow project conventions | Follow project conventions |

Do not migrate frameworks just because another syntax appears shorter.

## 32. Laravel Testing Commands

~~~powershell
cd C:\xampp\htdocs\ecommerce\backend
php artisan test
php artisan test tests/Feature/TaskCreationTest.php
php artisan test --filter=task_creation
.\vendor\bin\phpunit
.\vendor\bin\pest
~~~

Run direct PHPUnit or Pest commands only if installed. Before database tests, confirm an isolated, disposable test database. A command named test is not proof it is safe against production data.

## 33. Laravel Feature Test — Required Validation

Assume the approved route is POST /api/tasks.

~~~php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class TaskCreationTest extends TestCase
{
    public function test_task_creation_requires_title(): void
    {
        $response = $this->postJson('/api/tasks', [
            'title' => '',
        ]);

        $response->assertUnprocessable();
        $response->assertJsonValidationErrors(['title']);
    }
}
~~~

This assumes the route exists, accepts JSON, reaches validation, and is not blocked by authentication. Adapt to actual middleware and contract.

## 34. Laravel Feature Test — Successful Persistence

~~~php
<?php

namespace Tests\Feature;

use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class TaskPersistenceTest extends TestCase
{
    use RefreshDatabase;

    public function test_valid_task_is_created(): void
    {
        $response = $this->postJson('/api/tasks', [
            'title' => 'Prepare presentation',
        ]);

        $response->assertSuccessful();
        $this->assertDatabaseHas('tasks', [
            'title' => 'Prepare presentation',
        ]);
    }
}
~~~

This assumes the table, route, and payload are valid and authentication is handled.

**RefreshDatabase may run migrations or reset database state. Use an explicitly verified, isolated test database, never production or valuable development data.**

## 35. Laravel Pest Feature Test

~~~php
<?php

use function Pest\Laravel\postJson;

test('task title is required', function () {
    $response = postJson('/api/tasks', ['title' => '']);
    $response->assertUnprocessable();
    $response->assertJsonValidationErrors(['title']);
});
~~~

The project must support the Laravel Pest context. Follow PHPUnit conventions if that is what it uses. Behavior and assertions matter more than syntax.

## 36. Laravel Database Testing

Database tests verify creation, updates, relationships, constraints, queries, and consistency.

~~~php
$this->assertDatabaseHas('tasks', [
    'title' => 'Prepare presentation',
]);

$this->assertDatabaseMissing('tasks', [
    'title' => 'Unexpected task',
]);
~~~

Use an isolated test environment such as a disposable database or compatible SQLite setup. SQLite and PostgreSQL can differ; database-specific behavior may need verification against PostgreSQL.

## 37. Laravel HTTP Response Testing

~~~php
$response->assertOk();
$response->assertCreated();
$response->assertNotFound();
$response->assertForbidden();
$response->assertUnprocessable();
~~~

Assert exact JSON messages only when part of the API contract. Tests should verify observable contracts, not arbitrary implementation details.

## 38. Laravel Example — Product Search

For approved product-name search, test exact and partial matches if supported, empty results, empty queries as specified, authorization if required, and multiple matches under pagination rules.

Account for case sensitivity, visibility filters, product status, pagination, authentication, and response shape. Avoid adding unrelated recommendations or analytics.

# Part IV — Codex Verification Workflows

## 39. Testing Before Implementation

Start a read-only session:

~~~powershell
codex --sandbox read-only
~~~

Ask Codex to inspect approved feature requirements and existing tests, then report acceptance criteria, current coverage, missing positive/negative/boundary cases, appropriate test levels, relevant files, commands, environment dependencies, and risks. Require actual repository evidence; prohibit edits and invented requirements.

## 40. Requirements Traceability Matrix

| Requirement | Test case | Test level | Status |
|---|---|---|---|
| Task title required | TC-01 | Unit / Feature | Not Run |
| Valid task persisted | TC-02 | Integration | Not Run |
| Error feedback displayed | TC-03 | UI | Not Run |
| Existing task list preserved | TC-04 | Regression | Not Run |

It highlights requirements without tests, unrelated tests, missing verification, and incomplete acceptance criteria. Every important requirement should have an appropriate verification method.

## 41. Codex — Writing Focused Tests

After approving a plan, ask Codex to implement only approved tests, follow the existing framework, cover approved positive and negative cases, use meaningful assertions and existing utilities, avoid dependencies, and leave application behavior unchanged. Before editing, identify test files; afterward, run focused tests only when safe and report commands, results, and environment limitations.

## 42. Codex — Verification-Only Workflow

Ask Codex to read instructions and criteria, avoid code modifications, select relevant tests, confirm environment safety, run authorized checks, inspect the diff, compare results with criteria, and report missing verification. It must not claim tests passed unless they ran, or modify production data.

A read-only sandbox may prevent commands that write build or test artifacts. Test execution and read-only investigation are not always the same task.

## 43. Focused Tests Before Broad Tests

~~~text
Implement -> Focused Test -> Related Feature Tests
-> Integration Tests -> Broader Regression Suite
-> Manual Verification if Needed
~~~

This gives faster feedback and easier diagnosis. Do not skip required broad checks just because a focused test passes.

## 44. Investigating Failed Tests

Possible causes include implementation defects, incorrect expectations, missing setup, configuration mismatch, unavailable dependencies, contaminated data, and flaky timing.

~~~text
Failure -> Read Output -> Identify Assertion or Exception
-> Compare with Requirement -> Inspect Code and Setup
-> Determine Cause -> Correct Appropriate Component -> Rerun
~~~

Do not weaken assertions simply to make tests pass. Update an expectation only after confirming intended behavior.

## 45. Avoiding False Confidence

Tests can mislead when they lack assertions, use unrealistic data, ignore failure paths, are skipped, swallow errors, weaken assertions, check only compilation, or use mocks unlike real integrations.

Weak claim: “Everything works because the build succeeded.”

Better: “The debug build succeeded. The login UI and Firebase integration were not exercised.”

Report exactly what evidence demonstrates and what remains untested.

## 46. Manual Testing

Manual verification may be needed for visual alignment, navigation, device-specific behavior, accessibility, and complex workflows.

~~~markdown
## Manual Test
### Feature
Login screen
### Device
[Device or emulator]
### Preconditions
[Required setup]
### Steps
1. Open the login screen.
2. Enter invalid credentials.
3. Tap Login.
### Expected
Appropriate error feedback appears.
### Actual
[Observed result]
### Status
Passed / Failed / Blocked
### Evidence
[Screenshot or notes]
~~~

Use defined steps and expected outcomes, not casual impressions.

## 47. Git Verification

~~~powershell
git status
git diff --name-only
git diff
git diff --check
git diff --cached
~~~

Ordinary git diff does not show untracked files; use git status --short to find them. Behavioral tests and code review complement each other.

## 48. Verification Report

~~~markdown
# Verification Report
## Task
[Approved task name]
## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
## Files Changed
[List relevant files]
## Tests Executed
| Test | Command | Result |
|---|---|---|
| Focused tests | ... | ... |
| Build check | ... | ... |
## Manual Verification
[Actions and observations]
## Git Review
[Summary of diff inspection]
## Limitations
[Untested behavior or unavailable environment]
## Final Status
Complete / In Progress / Blocked
## Next Action
[Recommended next step]
~~~

Distinguish executed checks from planned checks.

## 49. Definition of Done

A **Definition of Done (DoD)** describes conditions required before a task is complete:

- Approved acceptance criteria are satisfied.
- Required implementation is present.
- Relevant tests pass.
- Necessary manual verification is completed.
- Existing behavior is preserved.
- Changes follow project standards.
- Git diff is reviewed.
- Remaining limitations are documented.

If tests cannot run, report why, what was verified instead, what remains unverified, and whether more approval is needed.

**Completion is a verified state, not simply the end of code generation.**

## 50. Reusable AGENTS.md Testing Rules

Adapt this to existing instructions; do not duplicate rules.

~~~markdown
## Testing and Verification
- Follow the existing testing framework.
- Write focused tests for approved behavior.
- Cover relevant positive and negative cases.
- Do not weaken assertions to hide failures.
- Use isolated test environments.
- Do not access production data during tests.
- Run relevant verification after implementation.
- Review the Git diff before reporting completion.
- Report executed commands and actual results.
- Distinguish passed, failed, skipped, and blocked checks.
- Do not claim completion without required evidence.
- Stop when the approved task is verified.
~~~

# Part V — Practical Exercises

## 51. Exercise A — Design Test Cases

Design positive, negative, and boundary cases for a required title of at most 100 characters. Consider a valid title, empty, whitespace-only, exactly 100 characters, more than 100 characters, and null if applicable.

| Test ID | Input | Expected result |
|---|---|---|
| TC-01 | ... | ... |
| TC-02 | ... | ... |
| TC-03 | ... | ... |

## 52. Exercise B — Android JUnit Testing

Use an existing calculator class to test addition and subtraction with positive and negative integers. Run local tests and review the results.

~~~powershell
.\gradlew.bat :app:testDebugUnitTest
~~~

## 53. Exercise C — Android UI Test Planning

Plan cases for empty email, empty password, invalid credentials, valid credentials, loading state if required, and error feedback. Record steps, expected results, and prerequisites.

## 54. Exercise D — Laravel Feature Testing

Inspect the actual route and validation contract, write a feature test, submit invalid input, assert status and validation error, then run the focused test.

~~~powershell
php artisan test --filter=task_creation
~~~

## 55. Exercise E — Laravel Database Verification

Confirm approved fields, inspect model and migration, verify the isolated test database, prepare data, execute the request, assert response and database state, and review results.

**Never run database-resetting tests against production or valuable development data.**

## 56. Exercise F — Read-Only Codex Test Audit

~~~powershell
cd C:\Users\Acer\AndroidStudioProjects\GroupTrack
git status
codex --sandbox read-only
~~~

Ask Codex to inspect instructions, requirements, and tests; identify framework, directories, covered behavior, missing positive, negative, and boundary cases, integration risks, UI needs, commands, and environment limits. Prohibit edits and invented requirements. Review its audit.

## 57. Exercise G — Codex Verification Report

Ask Codex to inspect changed files and criteria, run authorized checks, review the diff, report actual pass/fail/blocked status, and identify unverified behavior. It must not claim success without evidence.

## 58. Knowledge Check

1. What is software testing?
2. How does testing differ from verification?
3. What is validation?
4. Why is testing important in agentic coding?
5. What is a unit test?
6. What is an integration test?
7. What is a feature test?
8. What is a UI test?
9. What is positive testing?
10. What is negative testing?
11. What is boundary value testing?
12. What is equivalence partitioning?
13. What makes an assertion meaningful?
14. Why is test isolation important?
15. What causes flaky tests?
16. What is a test double?
17. Why is high coverage not enough?
18. What is Red–Green–Refactor?
19. Where are Android local tests stored?
20. How do test and connectedAndroidTest differ?
21. How do PHPUnit and Pest differ?
22. What does Laravel RefreshDatabase support?
23. Why must test databases be isolated?
24. What is a requirements traceability matrix?
25. What should a verification report contain?
26. What is a Definition of Done?
27. Why should failed tests not be hidden or weakened?
28. Why should Git diff review accompany testing?

## 59. Key Terminologies

| Term | Definition |
|---|---|
| Software testing | Evaluation of software behavior |
| Verification | Checking conformance to requirements |
| Validation | Checking suitability for intended needs |
| Unit test | Test of isolated logic |
| Integration test | Test of interacting components |
| Feature test | Test of application capability |
| UI test | Test of user interface behavior |
| Positive test | Test using valid conditions |
| Negative test | Test using invalid or failure conditions |
| Boundary value testing | Testing values near limits |
| Equivalence partitioning | Grouping similar input conditions |
| Assertion | Check comparing actual and expected results |
| Test isolation | Independence from unrelated test state |
| Deterministic test | Test producing consistent results |
| Flaky test | Test with inconsistent results |
| Stub | Test double providing predefined responses |
| Fake | Simplified working test implementation |
| Mock | Test double used to verify interactions |
| Test coverage | Measurement of exercised code or behavior |
| Mutation testing | Evaluation of tests using artificial code changes |
| TDD | Test-Driven Development |
| Regression testing | Checking previously working behavior |
| Test environment | Configuration and resources used for testing |
| Traceability matrix | Mapping requirements to verification |
| Verification report | Record of actual checks and results |
| Definition of Done | Conditions required for task completion |

## 60. Key Takeaways

1. **Testing provides evidence about software behavior.**
2. **Verification checks requirements; validation considers intended user needs.**
3. **Unit, integration, feature, and UI tests address different risks.**
4. **Positive and negative tests cover valid and failure conditions.**
5. **Boundary testing helps find errors at input limits.**
6. **Meaningful assertions are essential to test quality.**
7. **Isolation and determinism improve reliability.**
8. **High coverage does not guarantee correct behavior.**
9. **Android local and instrumented tests run in different environments.**
10. **A successful build is not equivalent to passing behavioral tests.**
11. **Laravel supports testing through PHPUnit, Pest, and Artisan.**
12. **Database tests require isolated, verified environments.**
13. **Codex tests must follow approved requirements, not assumptions.**
14. **Failed tests should be investigated rather than hidden.**
15. **Verification reports must reflect commands actually executed.**
16. **Git review and tests provide complementary evidence.**
17. **A task is complete when its Definition of Done is satisfied.**

## 61. Official References

### OpenAI Codex

1. OpenAI — Codex Documentation
   https://developers.openai.com/codex/
2. OpenAI — Codex CLI
   https://developers.openai.com/codex/cli/
3. OpenAI — Codex CLI Reference
   https://developers.openai.com/codex/cli/reference/
4. OpenAI — Iterating Development Workflows with Codex
   https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex
5. OpenAI — AGENTS.md Guide
   https://developers.openai.com/codex/guides/agents-md/

### Android

6. Android Developers — Test Your App
   https://developer.android.com/studio/test
7. Android Developers — Test from the Command Line
   https://developer.android.com/studio/test/command-line
8. Android Developers — Fundamentals of Testing
   https://developer.android.com/training/testing/fundamentals
9. Android Developers — Local Unit Tests
   https://developer.android.com/training/testing/local-tests
10. Android Developers — Instrumented Tests
    https://developer.android.com/training/testing/instrumented-tests

### Laravel

11. Laravel 12.x — Testing: Getting Started
    https://laravel.com/docs/12.x/testing
12. Laravel 12.x — HTTP Tests
    https://laravel.com/docs/12.x/http-tests
13. Laravel 12.x — Database Testing
    https://laravel.com/docs/12.x/database-testing
14. Laravel 12.x — Mocking
    https://laravel.com/docs/12.x/mocking

### Testing Frameworks

15. PHPUnit Documentation
    https://docs.phpunit.de/
16. Pest Documentation
    https://pestphp.com/docs

### Git

17. Git — git diff
    https://git-scm.com/docs/git-diff

## 62. Lesson Completion Checklist

- [ ] Understand software testing.
- [ ] Differentiate testing, verification, and validation.
- [ ] Understand the testing pyramid.
- [ ] Differentiate unit, integration, feature, and UI tests.
- [ ] Design positive and negative test cases.
- [ ] Apply boundary value testing and equivalence partitioning.
- [ ] Write meaningful assertions.
- [ ] Understand test isolation and flaky tests.
- [ ] Understand test doubles, coverage, and TDD.
- [ ] Identify Android test directories.
- [ ] Write a basic Java JUnit test.
- [ ] Run Android Gradle tests.
- [ ] Understand Android instrumented tests.
- [ ] Differentiate PHPUnit and Pest.
- [ ] Write a Laravel feature test.
- [ ] Understand Laravel database testing.
- [ ] Use isolated test environments.
- [ ] Create a requirements traceability matrix.
- [ ] Perform a read-only Codex test audit.
- [ ] Understand verification-only workflows.
- [ ] Investigate failed tests and review Git changes.
- [ ] Create a verification report and apply a Definition of Done.
- [ ] Complete the practical exercises.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[07 - Git Integration and Code Review]]**

The next lesson focuses on using Git to manage and review AI-generated code changes.

Topics include:

- Git fundamentals for agentic coding
- Repository status and change inspection
- Branching strategies
- Understanding diffs
- Staging changes
- Writing meaningful commits
- Reviewing Codex-generated code
- Detecting unintended modifications
- Pull requests and review workflows
- Safe collaboration and rollback strategies

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.6*
*Learning Principle: Do not trust implementation claims without meaningful tests, observable results, and reviewable evidence.*
