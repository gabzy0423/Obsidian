---
name: qa-testing
description: Safely verify GoPasig behavior with proportional Laravel, frontend, Playwright, and integration QA; investigate and classify failures without silently fixing application code.
---

# GoPasig QA Testing

Use this skill for evidence-based QA of the GoPasig Laravel application. It complements `AGENTS.md`, Context Engineering, domain knowledge, Work Scope/Work Progress, Development Handoff, and Agentic Coding. QA establishes verification evidence; Agentic Coding owns approved implementation work.

## Execution flow

```text
CONTEXT READY


→ QA ENTRY GATE
→ SAFETY / PREFLIGHT
→ TEST PLAN
→ EXECUTE VERIFICATION
→ INVESTIGATE FAILURES
→ CLASSIFY
→ EVALUATE EVIDENCE
→ PASS / FAIL / BLOCKED
→ RETEST when applicable
→ QA COMPLETION REPORT
```

## QA Entry Gate

Before testing, establish a structured QA scope containing:

- QA target and expected behavior;
- evidence establishing the expected contract;
- owning domain and affected surfaces;
- relevant executable entry points and tests;
- appropriate verification layers and acceptance criteria.

Use this source-of-truth order:

1. executable code and schema;
2. automated tests;
3. accepted ADRs and domain contracts;
4. narrative documentation.

If expected behavior is materially ambiguous, record `UNRESOLVED / BLOCKED`, do not invent a contract, and do not create regression coverage for the unresolved behavior.

Route through the repository domain router. Load the primary domain first, then only the secondary domain needed by the verified call graph. Inspect the target path and focused tests rather than entire application or test directories.

## Safety / preflight gate

Before browser or database QA, verify the isolated QA runtime:

- `APP_ENV=qa`;
- `QA_RUNTIME` is enabled;
- the approved disposable QA database is selected and its identity is verified;
- required fixtures are deterministic and isolated;
- required frontend assets exist;
- browser/runtime availability is established when browser QA is required.

For GoPasig, use the existing QA foundation and preflight command (`npm run qa:preflight`) and preserve its guards. The approved runtime database is `qa/runtime/database.sqlite` when preflight establishes it. Never use or mutate development or production data, and never bypass a safety guard. A safety failure is `BLOCKED`.

## Proportional test plan

Select only the layers needed by the target:

- syntax/static checks;
- PHPUnit/Laravel feature or unit tests;
- JavaScript checks and frontend build;
- Playwright browser QA;
- integration/workflow verification.

State why the selected checks are sufficient. Do not mechanically run every test or generate unnecessary artifacts for trivial passing checks.

Use PHPUnit/Laravel for HTTP/API behavior, validation, authorization, persistence, lifecycle transitions, transactions, domain rules, and boundary cases. Use frontend checks/builds for frontend changes. Use integration verification when correctness crosses UI, request/API, backend, persistence, response, and rendered UI boundaries.

## Playwright browser QA

Exercise the actual user path in a real browser for authentication, navigation, controls, rendering, visible state, network/API behavior, console/runtime errors, and regressions.

Prefer this locator order:

```text
semantic / visible locator
→ owning active screen or container
→ target control
```

Do not force-click hidden controls, call internal JavaScript navigation instead of using the UI, disable strictness, increase timeouts to conceal failures, or use `.first()` / `.nth()` merely to suppress ambiguity. Investigate repeated markup and establish screen ownership before changing a locator.

Report browser/workflow verification separately from source, backend, or build checks. A passing build or backend test does not prove browser-visible behavior.

Known GoPasig environment behavior: managed Codex execution may reject Playwright/Edge process spawning with `Error: spawn EPERM`. Classify browser execution as `ENVIRONMENT BLOCKER / BLOCKED`; do not classify it as an application failure, weaken configuration, or bypass safety. Continue useful non-browser verification and identify authorized local Windows Playwright + Microsoft Edge execution as the required browser path. Historical smoke results are not current evidence; rerun when required.

## Failure investigation and classification

A failed test requires investigation before classification. Capture proportional evidence: exact command/result, HTTP/API response, DOM assertion, console/runtime error, network behavior, screenshot, trace, and database/state evidence as relevant.

Use the most specific supported classification:

- `QA TEST DEFECT` — the test violates an established UI/test contract;
- `QA INFRASTRUCTURE DEFECT` — the QA harness or tooling is faulty;
- `QA FIXTURE DEFECT` — deterministic test data/setup is invalid;
- `APPLICATION DEFECT` — established behavior is violated;
- `EXPECTED BEHAVIOR / TEST MISMATCH` — implementation is consistent with the contract but the locator/assertion is not;
- `RISK / OBSERVATION` — evidence identifies a concern without a verified violation;
- `UNRESOLVED` — expected behavior or cause remains materially unclear;
- `ENVIRONMENT BLOCKER` — execution is prevented by environment/tooling/safety limits.

Promote a finding to `APPLICATION DEFECT / VERIFIED ISSUE` only when all four conditions hold:

1. behavior is reproducible;
2. actual behavior is evidenced;
3. expected behavior is established;
4. actual behavior violates that expectation.

Do not promote suspicion into a verified issue.

## No silent fixing and handoff

QA must not silently modify application business code, schema, domain contracts, production/development data, safety guards, or tests merely to make a failure disappear. Do not implement an application fix unless separately authorized.

When an application defect is verified, produce a bounded implementation handoff with the evidence, contract, affected path, acceptance criteria, constraints, and verification plan. Route approved implementation through Agentic Coding. QA may propose or add regression coverage only after the expected behavior is established and the scenario represents that verified contract.

## Retest

After an approved fix, reproduce the original scenario, execute the relevant regression coverage, verify the expected behavior, and perform nearby checks proportional to regression risk. Never declare pass solely because code changed.

Use these check states exactly:

- `PASSED` — expected behavior verified;
- `FAILED` — verified behavior does not satisfy the established expectation;
- `BLOCKED` — verification could not complete because of environment, safety, tooling, dependency, or unresolved contract;
- `NOT EXECUTED` — identified but intentionally not run.

Overall QA status is one of `PASS`, `FAIL`, `BLOCKED`, or `PASS WITH KNOWN RISKS`.

## QA completion report

Finish every QA run with this concise structure:

```text
QA TARGET
EXPECTED CONTRACT
ENVIRONMENT / PREFLIGHT
TESTS EXECUTED
RESULTS
EVIDENCE
FINDINGS
CLASSIFICATION
VERIFIED ISSUES
BLOCKED / NOT EXECUTED CHECKS
RESIDUAL RISKS
RETEST STATUS
FINAL QA STATUS
EXACT NEXT STEP
```

Distinguish identified checks from executed checks and report exact commands and limitations. Do not claim browser, persistence, network, screenshot, trace, or console evidence that was not actually collected.
