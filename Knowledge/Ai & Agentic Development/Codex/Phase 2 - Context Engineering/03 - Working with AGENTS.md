# 03 — Working with AGENTS.md

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 2 — Context Engineering  
> **Lesson:** 2.3 — Working with AGENTS.md  
> **Difficulty:** Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[02 - Understanding Context Windows and Context Management]]

---

## 1. Introduction

Projects often have conventions and constraints that should guide more than one Codex task: architecture, coding standards, dependency policies, feature boundaries, or verification expectations. Repeating all of them in every prompt is inefficient.

Codex reads applicable `AGENTS.md` instruction files before working. These files provide reusable guidance at global, repository, or more specific directory levels.

**Core principle:** Keep `AGENTS.md` concise and durable. Use it to point Codex toward relevant project knowledge instead of copying the entire project documentation into it.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Explain what `AGENTS.md` does and what it does not do.
- Describe how Codex discovers global and project instructions.
- Explain directory scope, precedence, and `AGENTS.override.md`.
- Draft concise project instructions and reference supporting documentation.
- Recognize conflicts and outdated guidance.
- Verify which instruction files Codex loaded.

## 3. What Is `AGENTS.md`?

`AGENTS.md` is a Markdown file for instructions to coding agents. It is optional; Codex can work without one. It is useful when a project has guidance that should apply across multiple tasks.

```markdown
# Project Instructions

## Project
Android application using Java and XML layouts.

## Development Rules
- Follow the existing architecture and conventions.
- Reuse existing components and avoid unnecessary dependencies.
- Keep changes within the requested scope.

## Documentation
Read the relevant files under docs/ when changing architecture or features.

## Verification
Run relevant checks and report anything that could not be verified.
```

`AGENTS.md` is guidance. It is not source code, a full requirements specification, a testing substitute, or a security boundary. Permissions and sandbox settings enforce technical access limits.

## 4. Instruction Locations and Scope

Codex builds an instruction chain when it starts. The global instruction file comes from the Codex home directory, which defaults to `~/.codex` unless `CODEX_HOME` is set. Project instructions are then discovered from the project root (usually the Git root) down to the current working directory.

| Level | Typical location | Scope |
|---|---|---|
| Global | `~/.codex/AGENTS.md` | General preferences across projects |
| Repository | `<project>/AGENTS.md` | Shared project conventions |
| Nested | `<project>/<area>/AGENTS.md` | Guidance for that directory and its descendants |

For example:

```text
project/
├── AGENTS.md
├── frontend/
│   └── AGENTS.md
└── backend/
    └── AGENTS.md
```

When working under `frontend/`, Codex can apply the project-root guidance and the frontend-specific guidance. Backend rules are not part of that directory path.

## 5. Discovery and Precedence

At global scope, Codex checks `AGENTS.override.md` first; if it exists and is non-empty, it uses that instead of the global `AGENTS.md`. Otherwise, it uses the global `AGENTS.md`.

For each directory from the project root to the current working directory, Codex checks, in order:

1. `AGENTS.override.md`
2. `AGENTS.md`
3. Any configured fallback filenames

Codex uses at most one instruction file per directory. It concatenates the selected files from the root down; more specific instructions appear later and can specialize broader guidance. Empty files are skipped. The combined instructions have a configurable size limit (`project_doc_max_bytes`, 32 KiB by default).

If no project root is found, discovery checks only the current directory. Codex builds the instruction chain when it starts, so restart or launch a new session after changing instructions.

### `AGENTS.override.md`

An override replaces the regular `AGENTS.md` at the same directory for discovery; do not expect both to be combined. Use it only when a directory genuinely needs replacement or temporary override guidance. Prefer resolving conflicting rules and keeping the instruction structure simple.

## 6. Instruction Precedence in Practice

Root guidance and nested guidance should work together:

**Root `AGENTS.md`:**

```markdown
- Follow existing architecture.
- Avoid unnecessary dependencies.
- Run relevant checks after code changes.
```

**Backend `AGENTS.md`:**

```markdown
- Follow the existing Laravel service and validation patterns.
- Preserve the established API response format.
```

The backend instructions add detail within their directory. They do not grant extra filesystem permissions or override higher-priority system and security requirements.

Avoid contradictions such as “never add dependencies” at the root and “always add a dependency” in a nested folder. Decide the intended rule, remove outdated guidance, and rewrite the remaining instructions clearly.

## 7. Designing a Useful `AGENTS.md`

Useful sections often include:

- Project purpose and technology stack
- Architecture conventions that affect implementation
- Durable development rules and scope boundaries
- Links or paths to relevant documentation
- Verification expectations

Keep guidance specific, actionable, accurate, and durable. Avoid vague demands such as “write perfect code,” and avoid rules that force every task through an unnecessary process.

### What belongs in `AGENTS.md`?

Good candidates include architecture constraints, established coding conventions, dependency policy, testing expectations, and instructions about which documents to consult.

Detailed schemas, full API specifications, long feature requirements, diagrams, and historical logs are usually better stored in dedicated documents.

```text
project/
├── AGENTS.md
└── docs/
    ├── architecture.md
    ├── schema.md
    ├── features.md
    └── testing.md
```

```markdown
## Documentation
- Consult docs/architecture.md for service boundaries.
- Consult docs/schema.md before proposing data-model changes.
- Consult docs/features.md for feature requirements.
- Consult docs/testing.md for verification guidance.
```

Point to documentation when relevant; don’t require reading every document for every task. Instructions can also be truncated if the configured combined-size limit is reached.

## 8. Global Instructions on Windows

The default Codex home is `~/.codex` (normally under the user’s home directory on Windows). If `CODEX_HOME` is set, it changes that location.

PowerShell examples for inspecting the location and a global instruction file:

```powershell
$env:CODEX_HOME
Join-Path $HOME ".codex"
Get-Content "$HOME\.codex\AGENTS.md"
```

If `CODEX_HOME` is set, use that directory instead of assuming `$HOME\.codex`. A global file should contain general preferences that apply across repositories. Keep project-specific architecture and policies in that project’s own instruction files.

These commands are lesson references only; no global file needs to be created for this lesson.

## 9. Repository and Nested Instructions

Create or edit a repository `AGENTS.md` only when the project has durable guidance worth sharing. Inspect an existing file before editing it; don’t replace its content blindly.

Nested instruction files are useful when parts of the repository have genuinely different tools or conventions, such as a TypeScript frontend and a Laravel backend. Add them only where they provide specific guidance; each extra file adds another source to maintain.

## 10. Example — Android GroupTrack

Suppose GroupTrack is an Android application using Java, XML layouts, Firebase, and Gradle. The real project structure should be checked rather than assumed.

```markdown
# GroupTrack Instructions

## Project
Android application for student project management and contribution tracking.

## Technology
Java, XML layouts, Firebase, and Gradle. Confirm details against the repository.

## Architecture and Scope
- Follow the existing package structure and component responsibilities.
- Implement only approved requirements.
- Preserve existing behavior and avoid unnecessary dependencies or abstractions.

## Documentation
- Consult docs/architecture.md for component boundaries.
- Consult docs/features.md for feature requirements.
- Consult docs/schema.md before data-model changes.
- Consult docs/testing.md for relevant checks.

## Verification
- Run relevant available build checks.
- Report checks that could not be run; do not claim device testing unless performed.
```

The instructions establish project-level rules while leaving detailed feature requirements in their owning documents.

## 11. Example — Laravel Backend and Full-Stack Project

Backend-specific instructions can describe local conventions:

```markdown
# Backend Instructions

## Architecture
- Follow existing controller, service, and model patterns.
- Preserve API response conventions.
- Use the existing validation approach.

## Boundaries
- Avoid unnecessary dependencies.
- Do not modify unrelated endpoints.
- Explain proposed schema changes before making them.

## Verification
- Run relevant backend tests and report failures or unverified behavior.
```

In a full-stack repository, a small root file can hold shared rules while `frontend/AGENTS.md` and `backend/AGENTS.md` describe their own technology and boundaries. Add nested files only when the guidance differs in practice.

## 12. Using `/init`

In Codex versions that provide it, `/init` can generate an `AGENTS.md` scaffold based on the project. Treat the result as a draft: verify its technology details, architecture, documentation paths, and rules against the repository before relying on it.

You can also ask Codex to review an existing instruction file without editing it:

> Review the applicable AGENTS.md files for unclear rules, contradictions, duplication, and details that belong in supporting documentation. Suggest improvements without changing files.

## 13. Verifying Instruction Discovery

Start Codex from the intended project directory and ask it to identify the instruction sources it loaded. Compare its answer with the actual directory path and files.

Example read-only request:

> List the instruction files applicable to this working directory and summarize their main rules. Do not modify files. Mention any uncertainty.

If instructions appear missing or stale, check the project root, working directory, `CODEX_HOME`, and any `AGENTS.override.md` files. Start a new Codex session after changing instruction files so the chain is rebuilt.

An agent’s summary is useful evidence, but actual behavior still needs review. Instruction files guide the agent; they do not technically enforce permissions.

## 14. Common Mistakes

| Mistake | Better approach |
|---|---|
| Making one instruction file a project encyclopedia | Keep it focused and link to dedicated docs |
| Putting project-specific rules in global guidance | Store them at the repository or relevant nested level |
| Creating nested files without distinct rules | Add them only when directory needs differ |
| Duplicating requirements across several documents | Keep one source of truth and reference it |
| Leaving contradictory or outdated rules | Resolve and update instructions as the project changes |
| Assuming every document is loaded automatically | Point to relevant docs and ask Codex to inspect them for the task |
| Treating instructions as security controls | Configure permissions and sandboxing separately |

## 15. Practice Exercises

These exercises are for a future study session; no project files need to be created now.

### Exercise A — Inspect instructions

Review an existing project’s `AGENTS.md`. Identify project-wide rules, documentation references, unclear instructions, and anything that belongs elsewhere.

### Exercise B — Draft a minimal file

For a disposable practice repository, draft a short `AGENTS.md` with the project purpose, a few durable development rules, and verification expectations. Check it against the actual project before using it.

### Exercise C — Explore nested scope

Sketch a root `AGENTS.md` plus frontend and backend instruction files. Identify which instructions are shared and which apply only to each directory. Check for contradictions and unnecessary duplication.

## 16. Knowledge Check

1. What purpose does `AGENTS.md` serve?
2. What is the default global instruction location, and what changes it?
3. How does Codex choose between `AGENTS.override.md` and `AGENTS.md` in one directory?
4. What does root-to-working-directory discovery mean?
5. How can nested instructions specialize root guidance?
6. What is the default combined instruction-size limit?
7. What information belongs in separate project documentation?
8. Why is `AGENTS.md` not a security boundary?
9. How can you check which instruction files Codex loaded?
10. Why should Codex be restarted after changing instruction files?

## 17. Key Takeaways

1. `AGENTS.md` gives Codex durable, reusable guidance; it is optional.
2. Codex combines global guidance with applicable project instructions from the repository root to the working directory.
3. At each directory, an override file takes precedence over the regular instruction file.
4. More specific guidance can refine broader rules, so remove contradictions.
5. Keep instructions concise and put detailed requirements in their own documents.
6. Verify discovery and behavior; use sandbox and permission settings for technical access control.

## Official References

- [OpenAI — Custom instructions with AGENTS.md](https://developers.openai.com/codex/guides/agents-md/)
- [OpenAI — Codex CLI](https://developers.openai.com/codex/cli/)
- [OpenAI — Codex configuration reference](https://developers.openai.com/codex/config-reference/)
- [OpenAI Cookbook — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
- [OpenAI Cookbook — Codex Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide)

## Next Lesson

**[[04 - Project Documentation and Knowledge Organization]]**

---

*Phase 2 — Context Engineering | Lesson 2.3*  
*Learning Principle: Keep instructions concise, scoped, and aligned with the actual project.*
