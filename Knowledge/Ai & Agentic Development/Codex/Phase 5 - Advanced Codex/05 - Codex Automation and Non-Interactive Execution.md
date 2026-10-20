# 05 — Codex Automation and Non-Interactive Execution

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 5 — Advanced Codex
> **Lesson:** 5.5 — Codex Automation and Non-Interactive Execution
> **Difficulty:** Intermediate to Advanced
> **Environment:** Windows, PowerShell, Git, Codex CLI
> **Prerequisite:** [[04 - Multi-Agent Collaboration and Task Delegation]]
> **Next Lesson:** [[06 - Context and Token Efficiency]]
> **Status:** Theory Documented

---

# 1. Introduction

Most developers begin with interactive Codex: start a session, describe a task, review the response, and steer the work with follow-up instructions. This works well for exploration, debugging, planning, and ambiguous implementation tasks.

Repeated, well-defined tasks can also run without an ongoing conversation. Examples include reviewing a Git diff, analyzing test failures, checking documentation, investigating a build error, or producing a structured report.

Codex CLI provides `codex exec` for scripted and CI-style non-interactive runs. Automation still needs clear scope, appropriate permissions, output validation, and human review where consequences matter.

## Core Principle

**Automate repeatable tasks with clear boundaries, verify the result, and keep humans responsible for consequential decisions.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Differentiate interactive and non-interactive Codex execution.
2. Use `codex exec` for bounded tasks.
3. Write scoped automation prompts.
4. Capture final output and JSONL events.
5. Use JSON Schema for structured responses.
6. Handle process exit status in PowerShell.
7. Design repeatable scripts with logging and safe retries.
8. Understand CI/CD integration and its security boundaries.
9. Apply sandbox and least-privilege controls.
10. Use Codex-assisted automation for Android, Laravel, and Next.js work.
11. Validate automated output before relying on it.

---

# Part II — Interactive and Non-Interactive Execution

## 3. Interactive Execution

Start a terminal UI session with:

```powershell
codex
```

Typical flow:

```text
Developer → describes task → Codex investigates/acts → developer reviews and follows up
```

Interactive sessions are useful for unfamiliar code, ambiguous requirements, complex debugging, architectural discussion, or iterative implementation. The developer can clarify requirements as findings emerge.

---

## 4. Non-Interactive Execution

`codex exec` runs a task-oriented session that finishes without ongoing human interaction:

```powershell
codex exec "Inspect this repository and summarize its main modules. Do not modify files."
```

The command can still use the tools and permissions available to the session. Non-interactive does not mean text-only or automatically read-only. Choose sandbox and approval settings deliberately.

---

## 5. Comparison

| Aspect | Interactive | Non-Interactive |
|---|---|---|
| Main command | `codex` | `codex exec` |
| Human follow-up | Available during the session | Usually requires another invocation or a resumed session |
| Best fit | Exploration and changing requirements | Bounded, repeatable work |
| Script integration | Less direct | Designed for scripts and CI-style runs |
| Clarification | Natural | Prompt must anticipate questions and define stop conditions |

**Use interactive Codex when the task needs conversation. Use `codex exec` when the objective and expected result are clear.**

---

# Part III — `codex exec`

## 6. Inspect the Installed CLI

Options can change between releases. Check the actual installed version and help before putting a command into a recurring script:

```powershell
codex --version
codex exec --help
```

The current [Codex CLI reference](https://developers.openai.com/codex/cli/reference/) documents `codex exec` as the non-interactive command and describes its current options.

---

## 7. Basic Read-Only Run

```powershell
codex exec --sandbox read-only "Inspect the repository structure and identify its main architectural components. Do not modify files."
```

`--sandbox read-only` limits workspace modification, while the prompt describes the requested analysis. Read-only is a good starting mode for reviews, but it does not make every possible external tool interaction harmless.

---

## 8. Common `codex exec` Options

| Option | Purpose |
|---|---|
| `--sandbox read-only` | Run with read-only sandbox permissions |
| `--sandbox workspace-write` | Permit writes within the configured writable workspace |
| `--output-last-message PATH` / `-o PATH` | Save the final assistant message to a file |
| `--output-schema PATH` | Provide a JSON Schema for the expected final response |
| `--json` | Write newline-delimited JSON events to stdout |
| `--cd PATH` / `-C PATH` | Set the workspace root for the run |
| `-c key=value` / `--config key=value` | Supply an invocation-specific configuration override |
| `--worktree` | Start a new or forked run in a managed Git worktree where supported |

See the installed `codex exec --help` for exact syntax and compatibility. Avoid `--dangerously-bypass-approvals-and-sandbox` (`--yolo`) except in a deliberately isolated runner; it bypasses safeguards.

---

# Part IV — Designing Automation Tasks

## 9. Automation Components

A useful Codex automation usually has:

1. A trigger, such as a developer command or CI event.
2. A known repository and working directory.
3. Explicit instructions and scope.
4. Deliberate sandbox and authentication settings.
5. Captured output and process status.
6. Validation and reporting.
7. Human review for consequential results.

```text
Trigger → script/CI → codex exec → captured result → validation → review/report
```

Start with a manually run script before adding schedules or repository-event triggers.

---

## 10. Write Prompts for Unattended Runs

Define the objective, scope, context, constraints, output format, verification, and stop condition. A non-interactive task has limited opportunity to ask for clarification.

```text
Task:
Review the current repository structure.

Scope:
Application source and project documentation.

Instructions:
Read applicable AGENTS.md files.
Do not modify files or install dependencies.

Output:
Return findings with file references, evidence, and verification limitations.

Stop:
Stop after producing the report.
```

Avoid vague instructions such as “improve the project.” State what Codex may inspect or change and what outcome counts as complete.

---

# Part V — Capture Results

## 11. Save the Final Message

`--output-last-message` saves the final assistant message to a file:

```powershell
codex exec `
    --sandbox read-only `
    --output-last-message "review-report.md" `
    "Review the repository architecture. Do not modify files. Return a concise Markdown report."
```

The report is generated content. Its existence does not prove its claims are correct; inspect evidence and verify important findings.

---

## 12. stdout, stderr, and Exit Status

Command-line programs commonly use:

| Output | Use |
|---|---|
| stdout | Normal output or machine-readable events |
| stderr | Diagnostics and errors |
| Exit status | Process completion result |

The final message can be captured with `--output-last-message`. `--json` writes JSONL events to stdout. Redirecting stdout to a file does not automatically make it a final-message-only report.

---

## 13. JSON Schema for Structured Output

Structured output is useful when another script needs predictable fields. A JSON Schema describes the expected response shape.

Save a schema as `review.schema.json`:

```json
{
  "type": "object",
  "properties": {
    "status": { "type": "string", "enum": ["reviewed", "blocked"] },
    "summary": { "type": "string" },
    "findings": { "type": "array", "items": { "type": "string" } },
    "requires_human_review": { "type": "boolean" }
  },
  "required": ["status", "summary", "findings", "requires_human_review"],
  "additionalProperties": false
}
```

Run Codex with the schema and a final-output file:

```powershell
codex exec `
    --sandbox read-only `
    --output-schema "review.schema.json" `
    --output-last-message "review.json" `
    "Review the repository. Do not modify files. Return a result matching the schema."
```

Example shape (illustrative only):

```json
{
  "status": "reviewed",
  "summary": "The repository was inspected.",
  "findings": ["Review the service/controller boundary."],
  "requires_human_review": true
}
```

A valid schema-shaped result is not proof that findings are true or complete.

---

## 14. Parse JSON in PowerShell

Only read the report after checking the process status and expected file:

```powershell
if ($LASTEXITCODE -ne 0) {
    throw "Codex failed with exit code $LASTEXITCODE"
}

if (-not (Test-Path "review.json")) {
    throw "Expected output file was not created."
}

$result = Get-Content "review.json" -Raw | ConvertFrom-Json
$result.status
$result.findings
```

PowerShell's `$LASTEXITCODE` contains the last native process exit status. Capture it immediately after `codex exec` because another native command can replace it.

---

## 15. JSONL Event Streams

JSONL (JSON Lines) contains one JSON object per line. With `--json`, Codex emits newline-delimited JSON events instead of formatted text:

```powershell
codex exec `
    --sandbox read-only `
    --json `
    "Inspect the repository structure. Do not modify files." `
    > "events.jsonl"
```

`--json` is useful for processing progress and event records. It is different from `--output-schema`, which describes the shape of the final response. Event names and schemas are Codex-version specific; use the official reference rather than assuming a homemade event schema.

The current CLI docs recommend pairing `--json` with `--output-last-message` in CI when both progress events and a final summary are useful.

---

# Part VI — Exit Codes and Error Handling

## 16. Process Success vs Task Findings

Exit code zero generally means the command completed successfully. It does not mean the application is correct or that a review found no issues.

| Process | Report | Meaning |
|---|---|---|
| Success | No findings reported | Analysis completed; validate its coverage |
| Success | Findings present | Analysis completed and reported potential issues |
| Failure | Missing or incomplete output | Execution or environment problem |
| Success | Insufficient evidence | Human investigation may still be needed |

Treat command status, findings, and application verification as separate signals.

---

## 17. PowerShell Exit-Code Handling

```powershell
codex exec `
    --sandbox read-only `
    --output-last-message "review.md" `
    "Review the repository. Do not modify files."

$codexExitCode = $LASTEXITCODE

if ($codexExitCode -ne 0) {
    throw "Codex execution failed with exit code $codexExitCode"
}

if (-not (Test-Path "review.md")) {
    throw "Expected report was not created."
}

Write-Host "Review completed: review.md"
```

The script checks the process result and the output artifact. A production script should also validate the report's content or schema as appropriate.

---

# Part VII — PowerShell Automation

## 18. Example Read-Only Review Script

Suggested structure:

```text
project/
├── AGENTS.md
├── scripts/
│   └── review-repository.ps1
└── src/
```

Example `scripts/review-repository.ps1`:

```powershell
$ErrorActionPreference = "Stop"

$projectRoot = Split-Path -Parent $PSScriptRoot
$outputDir = Join-Path $projectRoot "reports"
$outputFile = Join-Path $outputDir "repository-review.md"

New-Item -ItemType Directory -Path $outputDir -Force | Out-Null
Push-Location $projectRoot

try {
    codex exec `
        --sandbox read-only `
        --output-last-message $outputFile `
        "Read applicable AGENTS.md instructions. Review the repository structure. Do not modify source files. Produce a concise Markdown report."

    $codexExitCode = $LASTEXITCODE
    if ($codexExitCode -ne 0) {
        throw "Codex failed with exit code $codexExitCode"
    }
    if (-not (Test-Path $outputFile)) {
        throw "Expected report was not created."
    }

    Write-Host "Report generated: $outputFile"
}
finally {
    Pop-Location
}
```

The script itself creates a report directory and output file even though Codex is read-only. The sandbox restricts Codex operations, not every write performed by the surrounding PowerShell script. Choose the output path intentionally. Before using in CI, review authentication, permissions, cleanup, and artifact retention.

---

## 19. Running the Script

From the project root:

```powershell
.\scripts\review-repository.ps1
```

Prerequisites include an installed Codex CLI, valid authentication, an accessible project, and the needed permissions. If PowerShell blocks a script, inspect the local execution policy and script trust. Do not broadly weaken system policy to run an untrusted script.

---

# Part VIII — Logs, Repeatability, and Idempotency

## 20. Execution Records

Useful metadata includes the task identifier, start/end time, CLI version, exit status, output path, and validation result. Generate facts such as timestamps and process status in the orchestration script rather than asking the model to invent them.

Example record (not a built-in Codex format):

```json
{
  "task": "repository-review",
  "status": "completed",
  "exit_code": 0,
  "report": "reports/repository-review.md"
}
```

Keep logs useful but do not store credentials, tokens, or unnecessary private source content. Restrict access and choose an appropriate retention period.

---

## 21. Idempotency and Safe Retries

An operation is idempotent when repeating it does not create unintended additional effects. Read-only analysis is generally easier to retry than a file-modifying agent task.

For scripts that generate or update files:

- Use predictable output paths.
- Avoid appending duplicate content on each run.
- Define whether existing output is replaced or preserved.
- Check status and output before retrying.
- Keep the scope limited to one task.

The same prompt can produce different prose on different runs. Repeatability means the workflow can be safely repeated, not that every generated byte is identical.

---

# Part IX — CI/CD Integration

## 22. What Is CI/CD?

CI (Continuous Integration) automates activities such as builds, tests, and code-quality checks. CD commonly refers to Continuous Delivery or Continuous Deployment.

Codex can add analysis to a pipeline, but it should complement deterministic build, test, lint, and security tools rather than replace them.

```text
Repository event → CI runner → deterministic checks → optional Codex analysis
                 → validate output → publish report → human review
```

---

## 23. CI/CD Boundaries

Before putting Codex in a pipeline:

- Verify supported authentication and current integration guidance.
- Use scoped credentials and minimum repository permissions.
- Treat pull-request content and generated files as untrusted input.
- Restrict write access; start with read-only analysis.
- Validate structured output before using it downstream.
- Keep human approval for consequential changes.
- Avoid exposing secrets in prompts, logs, or artifacts.

Codex can report concerns, but the pipeline should retain ordinary tests and deterministic checks.

---

# Part X — Security in Unattended Execution

## 24. Risks

Unattended runs have fewer opportunities for immediate intervention. Risks include unintended file changes, excessive network access, sensitive-data exposure, unsafe commands, prompt injection in repository content, and misleading reports.

The less human supervision a task receives, the more important technical safeguards become.

---

## 25. Sandbox and Approval

For analysis, start with:

```powershell
codex exec --sandbox read-only "Review the repository without modifying files."
```

For approved local work, `workspace-write` may be appropriate when the writable workspace is sufficient. Avoid bypassing approvals and sandboxing as a convenience. The current CLI reference recommends keeping unrestricted bypass options for isolated runners only.

Unattended automation may not be able to respond to an interactive approval request. Configure the workflow so it can complete within its intended permissions; do not grant broad access simply to make it run unattended.

---

## 26. Authentication and Secrets

- Use supported credential mechanisms and limited-scope credentials.
- Do not hardcode tokens in scripts or committed configuration.
- Avoid printing secrets in logs.
- Restrict access to CI secret stores and generated artifacts.
- Keep untrusted pull-request code away from privileged credentials.
- Rotate or revoke credentials if exposure is suspected.

Do not embed secrets in `AGENTS.md`, `SKILL.md`, PowerShell scripts, Git-tracked configuration, or generated reports.

---

# Part XI — Stack Examples

## 27. GroupTrack Android — UI Review

Use a read-only review to inspect Android XML layouts without allowing Codex to modify source files:

```powershell
Set-Location "C:\Users\Acer\AndroidStudioProjects\GroupTrack"

codex exec `
    --sandbox read-only `
    --output-last-message "android-ui-review.md" `
    "Read applicable AGENTS.md and project documentation. Review XML layouts for constraints, alignment, accessibility, and resource consistency. Do not modify files. Report actionable findings and limitations."
```

Static inspection cannot prove rendering across device sizes. Use Android Studio preview, emulator, or physical-device checks where appropriate.

---

## 28. GoPasig Laravel — Controller Review

```powershell
codex exec `
    --sandbox read-only `
    --output-last-message "laravel-review.md" `
    "Read applicable project instructions. Review the schedule-related controllers and services for validation, authorization, query efficiency, and separation of concerns. Do not modify files or database records. Cite evidence and limitations."
```

Codex review does not replace Laravel tests. Run project tests separately in a correctly configured test environment:

```powershell
php artisan test
```

Then inspect the proposed changes and `git diff` before accepting any fix.

---

## 29. Next.js — Frontend Review

```powershell
Set-Location "C:\xampp\htdocs\ecommerce\frontend"

codex exec `
    --sandbox read-only `
    --output-last-message "frontend-review.md" `
    "Review the Next.js frontend for component structure, accessibility, responsive layout, and duplication. Do not modify files or change backend API contracts. Report evidence and limitations."
```

Use scripts that actually exist in `package.json`:

```powershell
npm run lint
npm run build
```

Build and lint commands may create files or caches; use permissions appropriate to the task and environment.

---

# Part XII — Troubleshooting

## 30. Common Problems

| Problem | Possible cause | First check |
|---|---|---|
| `codex` not recognized | CLI missing or not on PATH | `Get-Command codex` |
| Authentication failure | Missing/expired login or CI credentials | Verify the supported login setup |
| Unsupported option | Version mismatch | `codex exec --help` |
| Output file missing | Process failed or wrong path | Exit status and output path |
| Invalid JSON | Wrong schema/output assumptions | Validate the actual final message |
| Permission denied | Sandbox or filesystem boundary | Review requested access and sandbox |
| Unexpected changes | Write permission was available | `git status` and `git diff` |
| Incomplete report | Prompt too broad or context missing | Narrow scope and cite evidence |
| CI failure | Environment or credential difference | Inspect redacted runner diagnostics |
| Duplicate output | Unsafe retry or append behavior | Review script's overwrite/retry logic |

---

## 31. Troubleshooting Procedure

```text
Check CLI version/help
        ↓
Check authentication and working directory
        ↓
Check sandbox and configuration
        ↓
Capture exit status and diagnostics
        ↓
Confirm expected artifacts exist
        ↓
Validate output format and claims
        ↓
Retry only after identifying the failure
```

Reproduce a failure with the smallest safe task. Do not repeatedly rerun an unchanged automation without learning anything from the previous attempt.

---

# Part XIII — Practical Exercises

## 32. Exercise A — Choose an Execution Mode

Choose interactive or non-interactive execution for:

1. Exploring an unfamiliar repository.
2. Generating a recurring architecture report.
3. Discussing implementation alternatives.
4. Producing a structured read-only review.
5. Debugging through several rounds of new evidence.

Explain why each mode fits the task.

---

## 33. Exercise B — Basic `codex exec`

```powershell
codex exec --sandbox read-only "Summarize the repository structure. Do not modify files."
```

Record the inspected areas, output, exit status, and any reported limitations. Do not treat a successful process as proof that every statement is correct.

---

## 34. Exercise C — Save a Report

```powershell
codex exec `
    --sandbox read-only `
    --output-last-message "review.md" `
    "Review the repository structure. Do not modify files."
```

Inspect the generated Markdown and compare file references with the repository.

---

## 35. Exercise D — Structured Output

Create a JSON Schema with a required `status`, `summary`, `findings`, and `requires_human_review`. Run Codex using `--output-schema` and `--output-last-message`, then parse the output with `ConvertFrom-Json`. Record both process status and whether the response is useful and evidence-based.

---

## 36. Exercise E — Exit-Code Handling

Run a read-only Codex task, immediately save `$LASTEXITCODE`, and branch on success or failure. Verify that the expected output file exists before attempting to parse it.

Questions:

1. Why capture `$LASTEXITCODE` immediately?
2. Does exit code zero prove no code issues exist?
3. What should a script do when the process fails?

---

## 37. Exercise F — Design Stack-Specific Reviews

Write scoped prompts for:

- GroupTrack Android XML UI review without edits.
- Laravel controller validation/authorization review without database changes.
- Next.js accessibility and responsive-layout review without API changes.

Each prompt should request evidence and state limitations.

---

## 38. Exercise G — CI/CD Security Review

- [ ] Trigger and source branch are understood.
- [ ] Credentials are scoped and protected.
- [ ] Repository permissions are minimal.
- [ ] Untrusted inputs are handled cautiously.
- [ ] Sandbox is appropriate.
- [ ] Output is validated before use.
- [ ] Logs and artifacts exclude secrets.
- [ ] Human approval exists where needed.
- [ ] Automated modifications are controlled.
- [ ] Results can be audited.

---

# Part XIV — Knowledge Review

## 39. Knowledge Check

1. What is non-interactive execution?
2. How does it differ from an interactive session?
3. What does `codex exec` do?
4. Why should prompts define scope and stop conditions?
5. What does `--output-last-message` do?
6. What is JSON Schema used for?
7. What does `--json` produce?
8. How does JSONL differ from a single JSON response?
9. What does `$LASTEXITCODE` contain?
10. Does a successful exit code prove findings are correct?
11. Why should scripts verify output files?
12. What is idempotency?
13. How does logging support observability?
14. Why should Codex complement deterministic CI checks?
15. What security risks matter in unattended execution?
16. Why is read-only mode useful for review automation?
17. Why should CI credentials be limited?
18. When should a task stay interactive?

---

## 40. Key Terminologies

| Term | Definition |
|---|---|
| Interactive Execution | Ongoing task execution with follow-up interaction |
| Non-Interactive Execution | Task execution without continuous conversation |
| `codex exec` | Codex CLI command for scripted or CI-style runs |
| stdout | Standard output stream for normal results/events |
| stderr | Standard error stream for diagnostics |
| Exit Status | Numeric process completion result |
| JSON Schema | Description of the expected JSON response structure |
| JSONL | One JSON object per line |
| Structured Output | Machine-readable response with a defined shape |
| CI/CD | Automated integration and delivery/deployment workflows |
| Idempotency | Safe repetition without unintended cumulative effects |
| Observability | Ability to understand execution behavior from records |
| Artifact | File produced by an automated workflow |
| Least Privilege | Granting only the access required for the task |
| Human-in-the-Loop | Human review or approval during an automated workflow |

---

## 41. Key Takeaways

1. `codex exec` is suited to scripted and CI-style non-interactive tasks.
2. Use it when the objective and constraints are clear.
3. Non-interactive does not mean read-only; choose permissions deliberately.
4. Save final messages with `--output-last-message` when useful.
5. `--json` emits JSONL events; `--output-schema` describes a structured final response.
6. Check exit status and expected artifacts before parsing results.
7. Exit success does not prove code quality or factual correctness.
8. Make retries safe and generate execution metadata in the script.
9. Keep logs useful without exposing secrets.
10. Codex can supplement CI but should not replace deterministic tests and builds.
11. Unattended workflows need strong permission boundaries and human review for consequential changes.
12. Start with small read-only automations and expand only after verifying them.

---

## 42. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
3. [Codex Security](https://developers.openai.com/codex/security/)
4. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
5. [Codex GitHub Action](https://developers.openai.com/codex/github-action/)

### OpenAI Cookbook

6. [Build Iterative Repair Loops with Codex](https://developers.openai.com/cookbook/examples/codex/build_iterative_repair_loops_with_codex)
7. [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)

### Supporting Documentation

8. [PowerShell Documentation](https://learn.microsoft.com/powershell/)
9. [GitHub Actions Documentation](https://docs.github.com/actions)
10. [JSON Schema](https://json-schema.org/)
11. [Git Documentation](https://git-scm.com/docs)

> **Documentation Note:** Codex CLI flags, authentication, output behavior, and CI integrations can change. Check the installed CLI help and current official documentation before deploying unattended workflows.

---

## 43. Lesson Completion Checklist

- [ ] Understand interactive and non-interactive execution.
- [ ] Use `codex exec` and inspect its installed help.
- [ ] Write scoped prompts and stop conditions.
- [ ] Choose a suitable sandbox.
- [ ] Capture final output and JSONL events appropriately.
- [ ] Understand JSON Schema and structured responses.
- [ ] Handle exit codes and missing output files in PowerShell.
- [ ] Design repeatable, safe-to-retry automation.
- [ ] Understand CI/CD boundaries and security risks.
- [ ] Protect credentials and logs.
- [ ] Apply automation concepts to Android, Laravel, and Next.js.
- [ ] Verify generated findings before relying on them.
- [ ] Complete the practical exercises and knowledge check.

---

# 44. Next Lesson

## [[06 - Context and Token Efficiency]]

The next lesson covers context selection, token use, progressive disclosure, efficient prompts, long-running sessions, and performance tradeoffs.

---

*Phase 5 — Advanced Codex | Lesson 5.5*
*Learning Principle: Automate clearly defined tasks, restrict permissions, validate outputs, and preserve human control over consequential changes.*
