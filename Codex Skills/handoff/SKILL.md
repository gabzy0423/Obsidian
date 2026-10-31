---
name: handoff
description: Create a complete, timestamped Markdown continuity note for the current session when the user says "handoff". Do not use for ordinary summaries.
---

# Development Handoff

When the user says `handoff`, create a durable continuation note for the entire current session.

## Destination and filename

- Save the note in `C:\Users\Acer\Documents\Obsidian\Knowledge Vault\Codex Handoffs`.
- Create the directory if it is missing.
- For new notes, use the local system time and the filename format `YYYY-MM-DD_HHMM_<task-slug>_handoff.md`.
- The task slug must be lowercase kebab-case, normally 2–6 meaningful words, and describe the primary work topic. Do not use generic names such as `task`, `work`, `summary`, `session`, `current`, or `update`, and do not use internal Level terminology.
- If the same task produces another note in the same minute, add seconds: `YYYY-MM-DD_HHMMSS_<task-slug>_handoff.md`.
- Never overwrite an existing note. Resolve any remaining filename collision by adding seconds or another clearly unique timestamp component; do not use arbitrary overwrite behavior.

## Required content

Write only facts supported by the conversation, workspace inspection, or tool results. State uncertainty explicitly rather than filling gaps. Capture every detail that can affect accurate continuation, including:

- the plain-language task and its current interpretation;
- project/workspace state, relevant instructions, requirements, constraints, permissions, and assumptions;
- decisions made and why;
- work completed and current progress;
- files created, modified, inspected, or deliberately left unchanged, with paths and relevant details;
- commands, checks, findings, errors, warnings, and external state that matter;
- open questions, unresolved issues, blockers, and risks;
- the exact recommended next action(s), in order.

Use this developer-readable structure without mechanically duplicating Work Scope and Work Progress:

```markdown
# Development Handoff

## Task
<plain-language task>

## Current Status
<where the work currently stands>

## Expected Behavior
<desired result or unresolved expected behavior>

## Scope

Primary:
<primary business domain>

Related:
- <related domain>

## What We Found
- <important verified finding>

## Relevant Code
- <verified path/class>

## Relevant Tests
- <verified test>

## Changes Made
- <file + purpose>
or
None

## Validation
- <what was actually run/checked>
- <result>

## Blockers / Decisions Needed
<material unresolved issue>
or
None

## Next Step
<one exact continuation action>

## Follow-up
- <out-of-scope debt>
```

Translate Work Scope and Work Progress into these sections. Preserve continuation-critical information, but omit permanent Engineering Guide rules, full Domain Knowledge, routing algorithms, maturity history, and conversation history. Distinguish identified tests from tests actually executed. End with one concrete next step. Do not claim an operation succeeded without evidence. Do not include secrets; identify their location or redaction only when that is necessary for continuity.

Existing notes using `Task Context`, `Session Context`, `Level 3`, or `Level 4` do not need migration.

## Completion

Verify that the new file exists and is non-empty, then report its full path. Creating the handoff does not itself perform or authorize any unfinished work.
