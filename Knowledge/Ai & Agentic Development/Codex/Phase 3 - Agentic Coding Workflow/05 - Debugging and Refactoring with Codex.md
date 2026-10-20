# 05 — Debugging and Refactoring with Codex

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 3 — Agentic Coding Workflow  
> **Lesson:** 3.5 — Debugging and Refactoring with Codex  
> **Difficulty:** Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[04 - Task Decomposition and Feature Development]]  
> **Learning Approach:** Concepts, diagnostic workflows, Android Java and Laravel examples, practical exercises

---

## 1. Introduction

Software development continues after a feature is implemented. Applications can encounter unexpected behavior, runtime exceptions, incorrect calculations, integration failures, and performance problems. Some defects are easy to reproduce; others appear only under specific conditions.

Examples include an Android app that crashes on a button tap, a registration form that accepts invalid input, a Laravel API that returns an unexpected response, or a feature that works locally but fails in another environment.

Codex can help investigate by inspecting source code, interpreting errors, tracing execution paths, testing hypotheses, proposing focused corrections, and running available checks. A reliable debugging process starts with evidence, not an immediate rewrite.

**Core principle:** Reproduce the defect, investigate its cause, implement the smallest justified correction, and verify the result.

---

## 2. Learning Objectives

By the end of this lesson, learners should be able to:

1. Define debugging and refactoring and distinguish them.
2. Separate symptoms, evidence, hypotheses, and root causes.
3. Create reproducible bug reports and interpret logs and stack traces.
4. Apply hypothesis-driven investigation and trace execution or data flow.
5. Use Codex for read-only diagnosis and bounded fixes.
6. Choose minimal corrections, regression checks, and safe refactoring steps.
7. Apply the workflow to Android Java and Laravel projects.
8. Review agent changes and report uncertainty accurately.

---

## 3. Debugging and Common Defect Types

**Debugging** is the systematic process of identifying, understanding, and correcting software defects. A defect causes behavior to differ from the intended behavior.

| Defect type | Description | Example |
|---|---|---|
| Syntax or compile-time | Code cannot be parsed or compiled | Incorrect Java type |
| Runtime | Failure during execution | Null reference |
| Logic | Code runs but produces the wrong result | Incorrect formula |
| Validation | Invalid data is handled incorrectly | Empty title accepted |
| Integration | Components use incompatible contracts | Frontend expects a missing API field |
| State | Application state becomes inconsistent | UI shows stale data |
| Concurrency | Operations interfere | Duplicate processing |
| Performance | Resource use or latency is too high | Repeated database queries |
| Configuration | Environment or settings are wrong | Incorrect API URL |

Classify the failure before choosing a diagnostic technique. A compiler message, runtime trace, incorrect output, and slow response call for different evidence.

---

## 4. Symptoms, Evidence, and Root Causes

A **symptom** is the visible effect. A **root cause** is the underlying condition that produced it.

Example symptom: “The app crashes when Login is tapped.” Possible hypotheses include an uninitialized view, a null value, an invalid navigation destination, or an authentication callback error. These are not conclusions until evidence supports them.

Keep these categories separate:

- **Observation:** The API returned HTTP 422 with a validation error.
- **Hypothesis:** The submitted request may violate a validation rule.
- **Conclusion:** The inspected rule rejects the submitted value, confirmed by a focused reproduction.

Fixing a visible symptom without understanding its cause may leave the underlying defect in place. Do not present a plausible explanation as a confirmed root cause.

---

## 5. Reproduce the Bug and Record a Baseline

**Reproduction** means performing a sequence of actions that reliably triggers the defect. It establishes conditions for comparing a proposed fix.

```markdown
# Bug Report — Login Button Crash

## Environment
Android debug build; device or emulator details

## Preconditions
Application is installed and the login screen is accessible.

## Steps
1. Open the application.
2. Navigate to Login.
3. Enter test credentials.
4. Tap Login.

## Expected
The application handles the login request.

## Actual
The application closes unexpectedly.

## Frequency and Evidence
Reproduction frequency; relevant redacted Logcat trace.
```

Record the app version, platform, data, network conditions, and frequency when relevant. For intermittent bugs, record timing, application state, and events before the failure. Exclude credentials, tokens, and private data from logs and prompts.

Before editing, inspect the repository state:

```powershell
git status
git branch --show-current
git diff
```

Distinguish pre-existing user changes from work for the current fix. When practical, run relevant checks before editing to establish which failures already existed.

---

## 6. Logs and Stack Traces

Logs record application events, warnings, and errors. Useful entries include a timestamp, severity, component, operation, and error details. Avoid logging passwords, tokens, API keys, or unnecessary personal information.

| Level | Typical use |
|---|---|
| Debug | Detailed diagnostic information |
| Info | Normal application events |
| Warning | Unexpected but possibly recoverable condition |
| Error | Failed operation |
| Critical | Severe application problem |

A **stack trace** shows method calls associated with an exception. Example:

```text
java.lang.NullPointerException
    at com.example.app.LoginActivity.submit(LoginActivity.java:42)
    at com.example.app.LoginActivity.onCreate(LoginActivity.java:25)
```

Start with the exception type and first relevant application frame, inspect the indicated code and values, then trace how the state was produced. The reported line is a clue, not automatic proof of the underlying cause; bad data may have originated earlier.

Separate what the log actually says from what it might mean. Do not invent missing stack traces or claim an error was reproduced if it was not.

---

## 7. Hypothesis-Driven Debugging and Root Cause Analysis

**Hypothesis-driven debugging** proposes explanations that make testable predictions:

```text
Observe failure → form hypothesis → predict evidence
→ perform focused check → confirm or reject → update investigation
```

Example: “The login button does nothing.”

| Hypothesis | Predicted evidence | Focused check |
|---|---|---|
| Listener is not registered | Callback never runs | Inspect setup; use debugger breakpoint |
| Validation exits early | A validation branch returns | Inspect conditions and values |
| Authentication fails | Failure callback receives an error | Inspect callback and redacted error |
| Navigation fails after success | Exception follows success callback | Trace navigation destination |

**Root Cause Analysis (RCA)** asks what failed, what condition triggered it, where that condition originated, why existing safeguards did not prevent it, and what correction prevents recurrence. Trace the relevant execution or request path instead of changing whichever file is easiest to reach.

```text
Request → route → validation → application logic → persistence → response
```

Conclusions should name the evidence. If the evidence is insufficient, report unresolved hypotheses and the smallest next check needed.

---

## 8. The Minimal Fix Principle

A **minimal fix** is the smallest justified change that corrects a confirmed defect and preserves required behavior. “Minimal” does not mean omitting necessary validation, error handling, tests, or integration work.

Example: If a confirmed addition operation mistakenly uses subtraction, the correction may be a one-line operator change. Rewriting the calculator architecture is unnecessary.

Focused changes are easier to understand, review, and verify, and reduce the chance of breaking unrelated behavior. Keep a narrow bug fix separate from unrelated cleanup when practical.

---

## 9. Bug Fixing vs Refactoring

| | Bug fix | Refactoring |
|---|---|---|
| Goal | Correct incorrect behavior | Improve internal structure |
| Intended external behavior | Changes from incorrect to correct | Remains the same |
| Evidence | Reproduction and regression checks | Behavior-preservation checks |
| Scope | Confirmed defect | Approved structural improvement |

**Refactoring** changes internal code structure without intending to change observable behavior. Common techniques include extracting a method, clarifying names, removing genuine duplication, simplifying conditions, separating responsibilities within the existing architecture, and deleting code confirmed to be unused.

Do not create an abstraction for a trivial one-time operation without a clear benefit. Before refactoring, understand existing behavior, establish a verification baseline, preserve interfaces, make one small change at a time, and run relevant checks. A debugging task should not silently turn into a broad refactoring project.

---

## 10. Regression Testing and Verification

**Regression testing** checks that previously working behavior still works after a change. When practical, turn a reproducible defect into a repeatable automated test.

For a fix that rejects duplicate email registration, verify both that duplicates are rejected and that valid unique registrations still succeed. Include relevant invalid-input and response-contract checks.

| Check | Purpose |
|---|---|
| Reproduction check | Demonstrate the original failure |
| Focused test | Verify corrected behavior |
| Negative-path test | Check invalid input or failure handling |
| Regression test | Guard related working behavior |
| Build/type/lint check | Detect structural or compile issues |
| Diff review | Detect unrelated changes |
| Manual check | Verify behavior automation does not cover |

A successful build does not prove behavioral correctness. Report exactly which checks ran, their observed results, and what remains unverified. Use commands that the repository actually supports.

---

## 11. Read-Only Codex Debugging Workflow

Start with diagnosis before authorizing code changes. A read-only CLI example is:

```powershell
codex --sandbox read-only
```

Confirm current flags with `codex --help`. Running tests or applications can still create artifacts or contact services, so consider the environment before allowing commands.

Example investigation prompt:

```text
Perform a read-only debugging investigation.

Read applicable AGENTS.md instructions and relevant project docs.

Problem: [describe the issue]
Expected behavior: [expected result]
Actual behavior: [observed result]
Reproduction steps: [exact steps]
Evidence: [logs, trace, or test failure]

Inspect relevant files and trace execution or data flow. List plausible
hypotheses, evidence for and against each, missing information, the supported
root cause if established, the smallest justified fix, and regression checks.

Do not modify files. Do not invent evidence or refactor unrelated code.
Distinguish confirmed findings from hypotheses. Stop after reporting.
```

Review whether Codex references real files, distinguishes observation from inference, and identifies uncertainty instead of guessing.

---

## 12. Bounded Fix and Refactoring Workflows

After confirming the root cause and approving the fix, use a bounded prompt:

```text
Implement the approved fix for the confirmed root cause: [cause].

Scope: [allowed files or behavior]
Non-goals: no redesign, new dependencies, or unrelated refactoring.

Make the smallest justified correction, add or update a focused regression
check where appropriate, run relevant available verification, inspect the diff,
and report files changed, actual test results, and limitations.

Stop if the fix requires significant changes outside the approved scope.
Do not begin another task.
```

For refactoring, state the structural objective and explicitly require behavior preservation. Identify the target before editing, make small changes, run relevant tests, and compare behavior. Do not combine broad cleanup with a narrow fix unless necessary and approved.

Codex’s write capability depends on the active configuration and approvals. A workspace-write example is:

```powershell
codex --sandbox workspace-write
```

Confirm installed CLI syntax and effective permissions. The prompt, sandbox, and review each serve different purposes.

---

## 13. Android Java Debugging

### 13.1 Logcat

In Android Studio, run the app on a device or emulator, open **View → Tool Windows → Logcat**, select the target app/device, reproduce the failure, and inspect the exception and preceding events. Redact sensitive values before sharing logs.

Diagnostic logging can help confirm execution:

```java
private static final String TAG = "LoginActivity";

private void submitLogin() {
    Log.d(TAG, "Login submission started");
    // Existing login logic.
}
```

Never log credentials or tokens.

### 13.2 Debugger

Breakpoints, stepping, variable inspection, and call-stack inspection can show whether a listener ran, which branch executed, and what values existed. If a button appears inactive, set a breakpoint in its listener, tap it in debug mode, and follow the execution path.

### 13.3 Null Reference Example

If a button is declared but never initialized, registering its listener can throw a null reference exception:

```java
setContentView(R.layout.activity_login);
loginButton = findViewById(R.id.loginButton);
loginButton.setOnClickListener(v -> submitLogin());
```

This assumes the project uses `findViewById`. If it uses View Binding or another convention, follow that approach. Verify that the loaded XML contains the expected ID, build the app, exercise the interaction, and inspect related behavior. Do not redesign authentication to fix a localized initialization defect.

### 13.4 Logic Error Example

For a subtraction result bug, test a positive result (`10 - 3 = 7`), a negative result (`5 - 8 = -3`), zero on the left (`0 - 4 = -4`), and zero on the right (`7 - 0 = 7`). Logic defects may not throw exceptions, so assert the actual result.

---

## 14. Laravel Debugging

Laravel logging configuration determines where entries are written; a common location is `storage/logs/`. Example PowerShell commands, when the project uses `laravel.log`:

```powershell
Get-ChildItem storage\logs
Get-Content storage\logs\laravel.log -Tail 100
Get-Content storage\logs\laravel.log -Wait -Tail 30
```

Protect log contents and never enable detailed debugging in production just to simplify an investigation.

### 14.1 Validation Bug Example

Suppose an approved requirement says task titles are required, but validation permits null values:

```php
$request->validate([
    'title' => 'nullable|string',
]);
```

A possible correction is:

```php
$request->validate([
    'title' => 'required|string',
]);
```

This is illustrative and assumes the requirement is a non-empty string. Check the existing Form Request or validation conventions, response contract, database rules, and authorization before changing code.

An illustrative feature test might assert that an empty title returns an unprocessable response with a title validation error. Real tests may require project-specific routes, authentication, and fixtures. Verify invalid and valid cases and preserve the API’s response format.

### 14.2 N+1 Query Investigation

Repeated relationship access may trigger unnecessary queries:

```php
$tasks = Task::all();
foreach ($tasks as $task) {
    echo $task->user->name;
}
```

If evidence confirms the N+1 issue, eager loading may help:

```php
$tasks = Task::with('user')->get();
```

Measure or inspect query behavior and verify filtering, authorization, and returned data. Do not change code based only on a suspicious-looking pattern.

---

## 15. Debugging Integration Failures

When a frontend displays an empty product list, trace the complete path before assigning blame:

```text
Next.js request → URL/network response → Laravel route/controller
→ database result → response shape → frontend mapping
```

Check the actual request URL, status and response body, route, controller output, database results, and frontend field names. Potential causes include a wrong URL, authentication failure, empty data, a response-shape mismatch, or incorrect frontend mapping. Confirm each with evidence.

---

## 16. Review the Agent’s Changes and Handle Blockers

Inspect changes with:

```powershell
git diff --name-only
git diff
git diff --check
git status --short
```

`git diff` does not display untracked file contents; `git status` reveals them so they can be inspected separately. Check whether the fix addresses the confirmed cause, preserves interfaces, avoids unrelated changes, adds meaningful checks, and leaves no temporary diagnostics.

If blocked, report confirmed evidence, untested hypotheses, checks attempted, the blocker, and the minimum input needed. Do not make speculative changes or claim resolution because the defect failed to reproduce once.

```markdown
## Debugging Status
Confirmed: ...
Unconfirmed: ...
Checks performed: ...
Blocker: ...
Next information or decision needed: ...
```

---

## 17. Practical Exercises

### Exercise A — Reproducible Bug Report

For an Android calculator that returns the wrong subtraction result, record the expected and actual behavior, input values, exact reproduction steps, environment, frequency, and relevant evidence.

### Exercise B — Interpret a Stack Trace

```text
java.lang.NullPointerException
    at com.example.app.MainActivity.calculate(MainActivity.java:58)
    at com.example.app.MainActivity.onCreate(MainActivity.java:31)
```

Identify the exception, relevant application method, source location to inspect, what the trace proves, and what additional evidence is required.

### Exercise C — Test Hypotheses

A Laravel API returns HTTP 500 when creating a task. Write three possible causes. For each, state the predicted evidence, focused check, and what would confirm or disprove the hypothesis.

### Exercise D — Read-Only Codex Investigation

In a practice project, check `git status`, start Codex with a supported read-only policy, and ask it to inspect the login flow, relevant errors/tests, possible failure points, and evidence needed to diagnose them. Tell it not to assume a defect or edit files. Verify its file references and uncertainty against the repository.

### Exercise E — Minimal Fix

Reproduce the calculator defect, locate the incorrect operation, apply the minimum correction, verify positive and negative results, and review the diff.

### Exercise F — Safe Refactoring

Identify genuine duplicated calculation logic, confirm behavior checks, extract a helper only if it improves clarity, run the same checks, and verify the refactor did not alter behavior or create unnecessary abstraction.

---

## 18. Knowledge Check

1. What is debugging?
2. How does a symptom differ from a root cause?
3. Why is reproduction important?
4. What information belongs in a bug report?
5. Why inspect the Git baseline before editing?
6. What does a stack trace provide, and what does it not prove?
7. What makes a debugging hypothesis testable?
8. What is a minimal fix?
9. How does refactoring differ from bug fixing?
10. What is regression testing?
11. Why is a successful build not sufficient proof of behavior?
12. How can Logcat and a debugger help investigate Android issues?
13. Where might Laravel logs be found, and what should be redacted?
14. How should Codex report an unresolved bug?

---

## 19. Key Terminologies

| Term | Definition |
|---|---|
| Debugging | Investigating and correcting software defects |
| Symptom | Observable effect of a defect |
| Root Cause | Underlying condition responsible for failure |
| Reproduction | Repeating conditions that trigger a defect |
| Baseline | Initial state used for comparison |
| Log | Recorded application event or diagnostic message |
| Stack Trace | Call sequence associated with an exception |
| Hypothesis | Testable explanation of an observation |
| Minimal Fix | Smallest justified correction |
| Refactoring | Internal restructuring without intended behavior change |
| Regression | Previously working behavior becoming incorrect |
| Regression Test | Repeatable check guarding against a defect returning |
| Breakpoint | Debugger pause point |
| Call Stack | Active sequence of method calls |
| Verification Evidence | Observed result supporting a completion claim |

---

## 20. Key Takeaways

1. Debugging begins with observation, reproduction, and evidence.
2. Symptoms, hypotheses, and confirmed root causes are different.
3. Logs and stack traces guide investigation but require interpretation.
4. Hypothesis-driven checks reduce guesswork.
5. Make the smallest justified fix, including necessary tests and safeguards.
6. Refactoring should preserve behavior and remain within approved scope.
7. Regression checks help prevent corrected defects from returning.
8. A build passing does not prove every behavior is correct.
9. Review the diff and report exactly what was verified.
10. When evidence is insufficient, state uncertainty and the next needed input.

---

## 21. References

### OpenAI Codex

1. [OpenAI — Codex Documentation](https://developers.openai.com/codex/)
2. [OpenAI — Codex CLI](https://developers.openai.com/codex/cli/)
3. [OpenAI — Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
4. [OpenAI — Codex Security](https://developers.openai.com/codex/security/)
5. [OpenAI — AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md/)
6. [OpenAI — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)

### Android

7. [Android Developers — Debug Your App](https://developer.android.com/studio/debug)
8. [Android Developers — View Logs with Logcat](https://developer.android.com/studio/debug/logcat)
9. [Android Developers — Analyze a Stack Trace](https://developer.android.com/studio/debug/stacktraces)
10. [Android Developers — Testing](https://developer.android.com/training/testing)

### Laravel and Git

11. [Laravel — Error Handling](https://laravel.com/docs/12.x/errors)
12. [Laravel — Logging](https://laravel.com/docs/12.x/logging)
13. [Laravel — Testing](https://laravel.com/docs/12.x/testing)
14. [Laravel — Validation](https://laravel.com/docs/12.x/validation)
15. [Git — git diff](https://git-scm.com/docs/git-diff)
16. [Git Documentation](https://git-scm.com/doc)

---

## 22. Lesson Completion Checklist

- [ ] Distinguish symptoms, evidence, hypotheses, and root causes.
- [ ] Create a reproducible bug report and establish a repository baseline.
- [ ] Interpret logs and stack traces carefully.
- [ ] Apply hypothesis-driven diagnosis and minimal fixes.
- [ ] Distinguish bug fixing from refactoring.
- [ ] Plan regression checks and review agent changes.
- [ ] Practice Android and Laravel debugging examples.
- [ ] Complete the exercises and knowledge check.

---

## Next Lesson

**[[06 - Testing and Validation]]**

The next lesson covers software testing fundamentals, test types and quality, positive and negative cases, Codex-assisted validation, Android and Laravel testing, verification reports, and the Definition of Done.

---

*Phase 3 — Agentic Coding Workflow | Lesson 3.5*  
*Learning Principle: Reproduce, investigate, confirm, fix minimally, verify thoroughly, and report accurately.*
