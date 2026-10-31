---
name: agentic-coding
description: Execute bounded, reviewable coding work after Context Engineering has produced a verified, implementation-ready Work Scope. Use for planning, implementing, verifying, evaluating, retrying, recovering, and finishing repository changes without bypassing project guidance.
---

# Agentic Coding

Use this skill only after Context Engineering has established the execution artifacts required by the active repository Engineering Guide. The Engineering Guide is the durable authority for contracts, scope, write permissions, risk, validation, domain routing, artifact schemas, and handoff requirements. This skill defines the procedural sequence and must not replace or weaken that guide.

## Required execution sequence

For non-trivial behavior-changing implementation, enforce this order:

```text
User Request
-> Engineering Guide / repository instructions
-> Primary Domain routing
-> Primary Domain Knowledge
-> explicit INITIAL Work Scope
-> targeted investigation
-> explicit complete VERIFIED Work Scope
-> Contract Decision Gate
-> Write Gate
-> Agentic Coding Entry Gate
-> PLAN VALIDATED
-> explicit Work Progress
-> implementation
-> Work Progress update
-> verification
-> final Work Progress
-> evaluation
-> DONE / PARTIALLY COMPLETE / BLOCKED / Development Handoff
```

For read-only answers, audits, or diagnosis without implementation, use the applicable repository artifacts and validation rules proportionally. Do not invent implementation authorization from a diagnostic result.

## Context and Work Scope checkpoints

Read the active repository Engineering Guide before deep investigation. Route the task to the primary domain by responsibility, then load the primary domain knowledge required by that guide. Load secondary domains only when the verified call graph crosses an ownership boundary.

### INITIAL Work Scope — required before deep investigation

Create an explicit structured `INITIAL Work Scope` before targeted deep investigation. Use the active Engineering Guide’s Work Scope schema; do not replace it with internal reasoning, narration, a checklist, a status message, or an informal plan. Provisional values are allowed, but the artifact must explicitly record the required fields from the active guide, including as applicable:

- task and scope;
- `Observed` behavior;
- `Expected` behavior, marked provisional if not established;
- `Expected evidence`, or an explicit statement that evidence is not yet found;
- primary domain and candidate secondary domain(s);
- candidate affected surface, executable entry points, and tests;
- acceptance criteria;
- constraints, compatibility or contract risk, and unresolved correctness decisions;
- verification plan.

At INITIAL, do not make behavior-changing code or test edits. Investigation may refine the artifact, but must not silently replace it with prose.

### VERIFIED Work Scope — required before behavior-changing edits

After targeted executable-path inspection and before any behavior-changing code or test edit, explicitly update the same Work Scope to `VERIFIED`. The complete verification-critical artifact must be present; a one-line declaration such as `Work Scope: VERIFIED`, `Implementation gate READY`, or `root cause verified` is insufficient.

The VERIFIED artifact must explicitly separate:

- `Observed`: behavior established by executable inspection or reproduced evidence;
- `Expected`: intended behavior established by authoritative repository evidence, or unresolved if not established;
- `Expected evidence`: the code, tests, accepted domain contract, ADR, or explicit product decision supporting Expected;
- verified primary owner and affected executable path;
- verified files/components and bounded acceptance criteria;
- compatibility, scope, and unresolved correctness risks;
- focused verification plan.

A Work Scope may be VERIFIED for the observed path while implementation remains blocked when Expected behavior is unresolved.

## Contract Decision Gate

Record the gate explicitly before implementation:

```text
Contract Decision Gate: PASS | BLOCKED
```

Pass only when Expected behavior has sufficient authoritative evidence under the active Engineering Guide. Existing behavior may establish Observed without establishing Expected. Existing tests must not invent unresolved product policy. Pre-existing candidate code is evidence, not authorization.

If Expected behavior or a material correctness decision is unresolved, record `Contract Decision Gate: BLOCKED`, stop behavior-changing implementation and test creation, and report the exact missing decision or evidence.

## Write Gate

Record the gate explicitly before implementation:

```text
Write Gate: PASS | BLOCKED
```

Behavior-changing code or test edits are allowed only when all applicable repository conditions are satisfied:

- executable path verified;
- primary owner verified;
- Expected behavior established;
- complete VERIFIED Work Scope explicitly produced;
- Contract Decision Gate is `PASS`;
- no material unresolved correctness decision remains;
- implementation is bounded to the authorized request;
- implementation gate is `READY`.

If any condition fails, record `Write Gate: BLOCKED` and do not make behavior-changing edits. Documentation-only or read-only work may proceed only within the user’s authorization and the repository’s rules.

## Agentic Coding Entry Gate

Confirm all of the following before any behavior-changing write:

- complete VERIFIED Work Scope exists;
- Contract Decision Gate is `PASS`;
- Write Gate is `PASS`;
- the implementation gate is `READY` based on the preceding artifacts, not merely declared;
- primary domain, affected executable path, expected behavior, scope, and acceptance criteria are verified;
- relevant pre-existing changes are understood well enough for safe targeted editing;
- proportional validation is identified.

The Entry Gate must not treat a declaration of READY as evidence that the Work Scope or earlier gates were satisfied. If any condition fails, stop before writing and report the missing evidence or decision.

## PLAN VALIDATED checkpoint

Create a compact explicit `PLAN VALIDATED` checkpoint before implementation. It must identify at minimum:

- intended behavior;
- owning domain;
- technical surfaces;
- expected files/components to change;
- minimum necessary change;
- material risks;
- focused validation;
- browser/workflow verification requirement when applicable.

Record `PLAN VALIDATED` only after the Work Scope, Contract Decision Gate, Write Gate, and Entry Gate pass. Keep the plan proportional. Do not require separate user approval for ordinary bounded implementation unless the existing elevated-risk rules require it.

## Work Progress

Once the Work Scope is VERIFIED and implementation is authorized, create an explicit structured `Work Progress` artifact using the active Engineering Guide’s Work Progress structure. Do not invent a competing schema. It must record the guide-required current phase, current focus, verified discoveries, changes made, validation, blockers, unresolved execution issues, exact next step, and optional follow-up debt.

Record meaningful Work Progress updates at these boundaries:

1. `PLAN VALIDATED` / implementation start;
2. implementation complete;
3. material failed validation when it changes the next action;
4. required validation complete;
5. immediately before final evaluation;
6. before Development Handoff when continuation is required.

Do not rewrite it after every terminal command. Informal statements such as “I found the root cause,” “Implementation complete,” “Tests passed,” or “Work Progress: complete” do not replace the structured artifact.

## Implement

Make the Minimum Necessary Change. Prefer existing owners, services, validators, policies, patterns, and the smallest necessary file set. Keep edits focused and do not perform opportunistic cleanup. Record useful but unnecessary discoveries as Follow-up debt.

Before editing each relevant file, classify provenance as `PRE-EXISTING WORK`, `CURRENT AGENT CHANGE`, `UNCHANGED`, or `UNCERTAIN PROVENANCE`. When Git is unavailable, assume existing edits are user work, use narrow patches, and stop if safe editing or rollback cannot be established. Never modify an unapproved candidate merely because it is present.

## Test and Verify

Inspect relevant existing tests first. Tests validate approved behavior and must not create a business contract. Add or update focused regression coverage for approved bug fixes and features when practical. Preserve coverage during refactors. Require stronger authorization and coverage for security, schema/data, destructive, and cross-domain lifecycle work.

Identify checks proportionally before execution. Use exactly the active repository validation vocabulary:

- `IDENTIFIED` — selected as relevant but not necessarily run;
- `EXECUTED` — actually run;
- `PASSED` — executed successfully;
- `FAILED` — executed with a relevant failure;
- `BLOCKED` — could not run because of environment, tooling, dependency, permission, or external-service limitation;
- `NOT EXECUTED` — intentionally not run and not validation evidence.

Never claim `PASSED` unless the check was actually executed successfully. Record the exact command and limitation for `BLOCKED`; distinguish it from intentional `NOT EXECUTED`.

Use proportional checks, normally:

1. syntax/static check;
2. smallest relevant focused test;
3. closely related tests;
4. subsystem tests when justified;
5. full suite only when breadth or risk warrants it.

For frontend changes, follow the repository requirement for the production asset build and relevant tests. For routes, authorization, schema, or data changes, include the repository-specific checks required by its Engineering Guide.

### Browser/workflow verification

For browser-visible or interaction-dependent acceptance criteria—such as clicking, navigation, modal behavior, asynchronous refresh, UI synchronization, direction switching, or visible persistence after reload—record browser/workflow verification separately using the active validation vocabulary:

```text
Browser/workflow verification: IDENTIFIED | EXECUTED | PASSED | FAILED | BLOCKED | NOT EXECUTED
```

A successful frontend build, backend test, source-string assertion, or Blade consistency test does not prove a real browser interaction. If browser testing is unavailable, record `BLOCKED`; if it was intentionally not performed, record `NOT EXECUTED`. Do not claim direct browser verification when none occurred. Do not block every low-risk UI fix solely because browser automation is unavailable unless the acceptance criteria require direct browser proof.

## Final Work Progress and evaluation

Immediately before final evaluation, record one final structured Work Progress update containing the actual changes, validation states/results, blockers, unresolved execution issues, exact next step, and Follow-up debt. Do not accept “Work Progress: complete” as a substitute.

Then perform an explicit compact final evaluation against:

- Expected behavior and Expected evidence;
- every acceptance criterion;
- scope and primary ownership;
- changed-file provenance;
- regressions and cross-domain effects;
- contract ambiguity;
- validation evidence, including browser/workflow status when applicable;
- blockers and follow-up debt.

Assign exactly one truthful terminal state:

- `DONE` — expected behavior and acceptance criteria are satisfied, required validation is complete, scope is bounded, provenance is known, and no blocking issue remains;
- `PARTIALLY COMPLETE` — valid in-scope work is complete but non-blocking continuation remains;
- `BLOCKED` — correctness, authority, environment, validation, or safe-recovery conditions prevent completion.

If continuation is required, record the current Work Progress and terminal evaluation first, then use the existing Development Handoff mechanism. Do not create a competing continuation mechanism. Pickup remains responsible for resumption.

## Retry and failure handling

Classify failures as implementation, test, environment/tooling, contract, scope, or unexpected regression. Implementation and test failures may receive at most two evidence-driven corrective retries for normal work and one by default for high-risk work. A retry needs a specific hypothesis, an in-scope correction, unchanged expected behavior, and evidence that the correction addresses the failure.

Contract failures and scope/authority failures receive zero speculative retries. Environment/tooling failures require environment investigation, not application changes that hide the problem. Unexpected regressions require preserving evidence and safely reverting only current-agent changes when provenance is certain. If understanding becomes invalid, return to Context Engineering and Work Scope verification.

## Recovery and safety

Never blindly revert pre-existing or uncertain work. Preserve evidence before rollback. If safe recovery cannot be established, stop. Do not repair forward through destructive or high-risk state without authorization.

Preserve elevated-risk approval requirements, validation proportionality, authorization boundaries, transaction and rollback safeguards, and the rule that tests validate approved contracts rather than create unresolved business behavior.
