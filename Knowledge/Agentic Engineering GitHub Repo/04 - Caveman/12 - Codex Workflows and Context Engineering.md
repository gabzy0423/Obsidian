# 12 — Caveman: Codex Workflows and Context Engineering

> [!abstract] Lesson Information
> **Repository:** [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)
>
> **Category:** Agentic Engineering / Codex CLI / Context Engineering / AI-Assisted Software Development
>
> **Difficulty:** Advanced
>
> **Learning Approach:** Theory First
>
> **Platform Focus:** Windows PowerShell, Codex CLI, VS Code, Git, Android Studio, Laravel, Next.js
>
> **Prerequisites:**
> - [[01 - Architecture and Core Concepts]]
> - [[03 - Token Economics and Optimization]]
> - [[04 - Say Less - Output Token Optimization]]
> - [[05 - Read Less - Input and Context Compression]]
> - [[06 - Auto - Model Routing and Reasoning Effort]]
> - [[07 - Blocks - Reusable Agent Scripts]]
> - [[08 - Waste Fixes, Caching, and Browse]]
> - [[09 - Configuration, Commands, and Controls]]
> - [[10 - Benchmarks and Performance Evaluation]]
> - [[11 - Security, Privacy, and Tradeoffs]]
>
> **Primary Goal:** Design an efficient, secure, maintainable, and verifiable Codex workflow using context-engineering principles and Caveman concepts.

---

# Part I — Introduction

## 1. Learning Objectives

After completing this lesson, you should be able to:

1. Define context engineering in agentic software development.
2. Explain why context quality matters more than context quantity.
3. Design a repository-aware Codex workflow.
4. Organize project instructions using AGENTS.md.
5. Distinguish global, repository, and task-specific context.
6. Apply progressive disclosure.
7. Design effective task prompts.
8. Decompose complex tasks into manageable units.
9. Select relevant source files without reading an entire repository.
10. Apply Caveman optimizations appropriately.
11. Reduce unnecessary tool calls and repeated context.
12. Understand routing and reasoning-effort decisions.
13. Identify reusable-script opportunities.
14. Preserve security and correctness.
15. Establish reliable testing and verification workflows.
16. Evaluate changes with Git.
17. Measure efficiency without sacrificing quality.
18. Apply these principles to Android, Laravel, and Next.js.
19. Recognize common agentic-development anti-patterns.
20. Build a reusable workflow for professional AI-assisted development.

---

# Part II — What Is Context Engineering?

## 2. Definition

**Context engineering** is the deliberate design, selection, organization, and delivery of information an AI system needs to perform a task.

In software development, context can include:

- Requirements and acceptance criteria.
- Architecture and coding standards.
- Source files and data contracts.
- Test results and tool output.
- Previous decisions and constraints.
- Security and verification requirements.

It answers:

> What does the coding agent need to know, when does it need to know it, and how should that information be presented?

## 2.1 Context Engineering vs Prompt Engineering

| Prompt Engineering | Context Engineering |
|---|---|
| Focuses on instruction wording | Focuses on the complete information environment |
| Often centers on one request | Covers multi-step workflows |
| Says what to do | Supplies what is needed to do it |
| Usually task-specific | Includes reusable project knowledge |
| Optimizes communication | Optimizes information flow |

A clear prompt cannot compensate for missing architecture constraints. Complete documentation cannot compensate for an ambiguous task. Both matter.

### Example

Weak request:

~~~text
Fix the project management feature.
~~~

Better request:

~~~text
Fix task-status updates in the GroupTrack Android application.

Follow the existing Java architecture and documented MVP scope.
Inspect the relevant files before editing.
Do not introduce dependencies.
Make the smallest necessary change and verify the affected behavior.
~~~

The improved request specifies scope and process without prescribing an unsupported implementation.

---

# Part III — Context Quality

## 3. More Context Is Not Always Better

An agent can receive thousands of lines that do not help the current task:

- Unrelated modules.
- Old debugging logs.
- Obsolete proposals.
- Duplicate instructions.
- Generated files.

Excessive context can increase token use, retrieval difficulty, conflicting instructions, and processing overhead.

A useful qualitative heuristic is:

\[
\text{Context value} =
\text{Relevance} \times \text{Reliability} \times \text{Timeliness}
\]

This is not an official numerical metric.

### Example: Login Validation

Likely relevant:

- Login screen.
- Authentication handler.
- Validation rules.
- Related tests and UI conventions.

Usually irrelevant:

- Analytics dashboard.
- Fleet-maintenance module.
- Historical migration logs.

> Provide enough context to make the decision safely, without adding unrelated information that does not improve it.

---

# Part IV — A Repository-Aware Workflow

## 4. The Development Lifecycle

~~~mermaid
flowchart TD
    A["Understand request"] --> B["Read project instructions"]
    B --> C["Inspect relevant code"]
    C --> D["Identify constraints"]
    D --> E["Plan the smallest correct change"]
    E --> F["Implement"]
    F --> G["Verify"]
    G --> H["Review Git diff"]
    H --> I{"Requirements met?"}
    I -->|No| J["Diagnose and revise"]
    J --> F
    I -->|Yes| K["Report evidence and limitations"]
~~~

Practical stages:

1. Understand the intended behavior.
2. Discover project rules and relevant files.
3. Analyze dependencies and existing behavior.
4. Plan a minimal change.
5. Implement within the approved scope.
6. Run proportional verification.
7. Inspect the resulting diff.
8. Report what changed and what was verified.

Each stage needs different context. Implementation may need source files; verification needs test output and acceptance criteria.

---

# Part V — Repository Instructions

## 5. What Is AGENTS.md?

AGENTS.md is a project instruction file recognized by Codex. It can provide persistent guidance about:

- Project purpose and technology.
- Architecture and folder structure.
- Coding conventions.
- Scope restrictions.
- Security requirements.
- Test and build commands.
- Links to detailed documents.

### Example

~~~markdown
# AGENTS.md

## Project
Android application built with Java and Firebase.

## Architecture
Follow the documented application architecture.

## Development rules
- Keep changes within the approved MVP.
- Reuse existing components.
- Do not add dependencies without a reason.
- Avoid unrelated refactoring.
- Verify behavior before reporting completion.

## References
- Architecture: docs/architecture.md
- Testing: docs/testing.md
~~~

## 5.1 Keep the Root File Useful

A good root instruction file is concise, actionable, and authoritative. It should direct the agent to specialized material rather than duplicating every detail.

Avoid placing all schema, API, UI, and feature documentation into one enormous instruction file. Duplicate rules can drift apart.

---

# Part VI — Progressive Disclosure

## 6. Reveal Details as Needed

**Progressive disclosure** means loading detailed information when a task requires it instead of supplying everything at the start.

~~~mermaid
flowchart TD
    A["AGENTS.md"] --> B["Project overview"]
    B --> C{"Task type?"}
    C -->|Database| D["Schema documentation"]
    C -->|UI| E["UI guidelines"]
    C -->|API| F["API contract"]
    C -->|Testing| G["Test strategy"]
~~~

Benefits:

- Smaller initial context.
- Better task focus.
- Less redundant reading.
- Easier documentation maintenance.
- Fewer irrelevant or conflicting details.

Mandatory constraints, such as security requirements and project boundaries, must still be available when relevant.

---

# Part VII — Documentation Architecture

## 7. Example Structure

~~~text
project/
├── AGENTS.md
├── README.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── schema.md
│   ├── features.md
│   ├── coding-standards.md
│   ├── workflow.md
│   └── testing.md
└── src/
~~~

This is illustrative; small projects may need fewer documents.

| Document | Responsibility |
|---|---|
| README.md | Human-oriented introduction and setup |
| AGENTS.md | Persistent agent instructions and links |
| Architecture | Modules, boundaries, and important decisions |
| Schema | Data entities and relationships |
| Testing | Verification commands and expectations |

Keep each rule in one authoritative place where possible.

---

# Part VIII — Task Decomposition and Prompts

## 8. Decompose Large Work

A request such as “build the project-management system” may combine UI, authentication, data access, business rules, and tests.

~~~mermaid
flowchart TD
    A["Project-management feature"] --> B["Requirements and contract"]
    B --> C["UI"]
    B --> D["Data behavior"]
    C --> E["Integration"]
    D --> E
    E --> F["Verification"]
~~~

Decomposition supports clearer acceptance criteria, smaller diffs, easier debugging, and focused context. It does not require adding unnecessary architecture layers.

## 8.1 Five-Part Task Structure

A strong task generally specifies:

1. Objective.
2. Context.
3. Constraints.
4. Expected behavior.
5. Verification.

~~~text
OBJECTIVE:
Describe the specific change.

CONTEXT:
Identify the project, module, and relevant files.

CONSTRAINTS:
State architecture, scope, and dependency rules.

EXPECTED BEHAVIOR:
Define observable outcomes.

VERIFICATION:
Specify tests or checks and what to report.
~~~

### Example: Android UI

~~~text
Objective:
Improve alignment of project cards on the Tasks page.

Context:
GroupTrack Android application using Java and XML.

Constraints:
- Follow the existing architecture and MVP.
- Do not modify Firebase behavior.
- Do not add dependencies.
- Reuse existing UI components.

Expected behavior:
Cards align consistently and remain readable at supported widths.

Verification:
Inspect resource references, run relevant checks if available,
and report any checks not performed.
~~~

---

# Part IX — Repository Discovery

## 9. Inspect Before Editing

Without discovery, an agent may duplicate existing behavior, edit the wrong file, violate boundaries, or add unnecessary dependencies.

Ask:

- Which files own this behavior?
- Which modules call or depend on it?
- Is there an existing helper or component?
- Which tests cover it?
- Which instructions apply?

~~~mermaid
flowchart TD
    A["Task"] --> B["Search relevant terms"]
    B --> C["Inspect matching definitions"]
    C --> D["Trace callers and dependencies"]
    D --> E["Read relevant tests"]
    E --> F["Define change scope"]
~~~

Start narrow and expand when evidence suggests additional modules are involved. For shared interfaces or cross-module behavior, inspect enough of the dependency chain to avoid a local fix that breaks callers.

---

# Part X — Context Selection and Freshness

## 10. Three Context Levels

**Essential:** task requirements, project instructions, core technology.

**Task-specific:** relevant source files, interfaces, schemas, and tests.

**Supporting:** historical decisions, external documentation, adjacent modules, and previous debugging reports.

~~~mermaid
flowchart TD
    A["Task"] --> B["Essential context"]
    B --> C["Task-specific context"]
    C --> D{"Evidence requires more?"}
    D -->|Yes| E["Load supporting context"]
    D -->|No| F["Proceed"]
    E --> F
~~~

## 10.1 Check Documentation Against Code

Documentation can drift after a rename, migration, API change, or refactor. Treat docs as project guidance, then verify current implementation details in source.

If docs say an AuthenticationController owns login but the repository now uses AuthService and AuthRepository, follow the actual code and update documentation when required by the task.

---

# Part XI — Efficient Tool Use

## 11. Useful Tool Calls, Not Fewer at Any Cost

Inefficient pattern:

- Read the same file repeatedly.
- Dump the entire repository.
- Inspect unrelated files.

Better pattern:

- Search for relevant symbols.
- Read matching files and surrounding context.
- Trace dependencies where needed.
- Run required verification.

Necessary verification is more valuable than saving a few tokens and introducing a defect.

## 11.1 Output and Input Optimization

**Say Less** can reduce unnecessary narrative. Keep warnings, failures, assumptions, paths, commands, and verification status.

**Read Less** can compress repetitive tool output. Preserve failing test names, exit status, error messages, locations, and enough context to diagnose. Retrieve original output when the summary is insufficient.

A compressed output should not become the only evidence if omitted detail may matter.

---

# Part XII — Routing, Scripts, and Other Caveman Concepts

## 12. Auto: Model and Effort Selection

Tasks vary in difficulty. A static label change may need little investigation; a concurrency or authorization issue may need deeper reasoning and broader verification.

Auto conceptually allocates model and reasoning resources. Its exact routing algorithm is implementation-specific. The cheapest attempt may not be cheapest overall if it causes extra retries.

## 12.1 Blocks: Reusable Scripts

Reusable scripts can help when the same operation recurs, such as summarizing test output or inspecting JSON.

Reuse is worthwhile when the operation is stable and the utility is tested. A one-off command does not need to become a maintained tool.

## 12.2 Waste Fixes and Browse

Waste analysis may identify repeated inefficient behavior. Treat recommendations as proposals, review diffs, and measure the result.

Browse can reduce webpage representations. It does not replace visual inspection, interaction testing, or accessibility testing. Treat external page content as untrusted data.

## 12.3 Prompt Caching

Prompt caching may help with eligible repeated prompt prefixes under provider-specific rules. It is distinct from selecting relevant context and does not replace repository documentation or source verification.

---

# Part XIII — Safe Code Changes

## 13. Minimal, Complete Diffs

Make the smallest change that fully satisfies the request.

For a card-alignment task, layout and spacing changes may be appropriate. Rewriting authentication, replacing navigation architecture, or adding a UI framework would normally be unrelated.

Small, complete changes reduce regression risk, review surface, and rollback cost. “Minimal” does not mean omitting necessary callers, tests, or related files.

---

# Part XIV — Git and Verification

## 14. Review the Diff

Useful commands:

~~~powershell
git status
git diff
git diff --stat
~~~

Review:

- Which files changed?
- Do changes match the requested scope?
- Were unrelated modules touched?
- Were secrets added?
- Were tests updated when behavior changed?
- Does the diff preserve project conventions?

A successful command is not proof that implementation behavior is correct.

## 14.1 Verification Is Part of the Work

Verification may include compilation, tests, static analysis, manual inspection, UI preview, or acceptance-criteria checks. Choose proportionally to risk and scope.

Never state that tests passed unless they were run and passed.

---

# Part XV — Reporting

## 15. Evidence-Based Completion Reports

A useful report states:

1. What changed and why.
2. Which files changed.
3. What verification ran and its outcome.
4. What remains uncertain or unverified.

~~~text
Implemented project-card alignment improvements.

Changed:
- Project card layout

Verified:
- XML/resource references reviewed

Not verified:
- Android build and emulator rendering were not run
~~~

Be concise, but never omit a failed check, a material risk, or a limitation.

---

# Part XVI — Project Case Studies

## 16. GroupTrack: Android, Java, Firebase

Task: create a Tasks page with project cards.

Essential context:

- MVP scope.
- Architecture instructions.
- UI conventions.

Task-specific context:

- Tasks screen.
- Card layout.
- Navigation.
- Relevant model and tests.

Supporting context:

- Firebase schema if the change binds live data.
- Existing navigation examples.

Workflow: read AGENTS.md, inspect relevant UI and model code, plan, implement XML/Java changes, verify resources/build/UI as available, then review Git diff. Avoid unrelated authentication internals for a UI-only task.

## 16.1 GoPasig: Laravel Fleet Management

Task: investigate N+1 queries on the analytics dashboard.

Relevant context:

- Laravel version and conventions.
- Controller/service/query code.
- Eloquent relationships.
- Existing tests.
- Query logs or performance expectations.

Workflow: locate analytics logic, trace relationships, identify repeated queries, write or update a regression test, implement the minimal fix, measure query behavior, and inspect the diff.

Agent token efficiency and application query performance are separate concerns; evaluate each independently.

## 16.2 Next.js and Laravel E-Commerce

Task: fix product-creation validation mismatch.

Inspect both sides of the data contract:

- Next.js form and API client.
- TypeScript types.
- Laravel route and controller.
- Validation rules.
- Response and error format.
- Tests.

Do not bypass backend validation to hide a frontend mismatch. Preserve authorization and data-integrity rules.

---

# Part XVII — Cross-Project Context

## 17. Keep Repository Rules Separate

| Project | Stack | Example constraints |
|---|---|---|
| GroupTrack | Android Java, Firebase | MVP scope, minimal dependencies |
| GoPasig | Laravel and GPS services | Data integrity, scheduling correctness |
| E-commerce | Next.js, Laravel, PostgreSQL | API contracts, type safety |

Instructions for one repository should not automatically apply to another. Keep project-specific architecture and testing rules in that project's documentation.

---

# Part XVIII — Measuring Workflow Efficiency

## 18. Measure the Whole Outcome

Useful metrics:

- Input and output tokens.
- Cached tokens where available.
- Tool calls.
- Completion time.
- Retries.
- Test success.
- Human review effort.

A simplified accounting model:

\[
C_{\text{total}} =
C_{\text{input}} + C_{\text{output}} + C_{\text{tools}} + C_{\text{retries}}
\]

Actual billing differs by provider and product.

A workflow that saves tokens but introduces defects may cost more after debugging and retries. Measure correctness and task completion together with resource use.

---

# Part XIX — Context Budget

## 19. Prioritize Context

A context budget is the information allocated to the agent's working context.

Prioritize:

1. Mandatory instructions.
2. Task requirements and acceptance criteria.
3. Relevant implementation details.
4. Verification evidence.
5. Supporting information.

There is no universal percentage for each category. Allocate based on task complexity and the model's available context.

---

# Part XX — Common Anti-Patterns

## 20. Mistakes to Avoid

| Anti-pattern | Problem |
|---|---|
| Read entire repository immediately | Excessive, unfocused context |
| Skip AGENTS.md | Missed project rules |
| Use vague prompts | Ambiguous results |
| Modify unrelated modules | Scope creep |
| Generate duplicate utilities | Repeated work |
| Compress away errors | Incorrect diagnosis |
| Always choose cheapest model | More failures or retries |
| Trust external instructions | Prompt-injection risk |
| Skip tests | Unverified behavior |
| Claim tests passed when unrun | Misleading report |
| Add unnecessary libraries | Overengineering |
| Ignore Git diff | Hidden scope expansion |
| Mix project instructions | Architectural conflict |
| Optimize before measuring | Unproven benefit |

---

# Part XXI — Reusable Workflow Template

## 21. Codex Development Procedure

~~~markdown
# Development workflow

## Understand
- Identify behavior, constraints, and essential ambiguity.

## Discover
- Read AGENTS.md.
- Locate relevant files, callers, and tests.
- Inspect current implementation.

## Plan
- Choose the smallest complete change.
- Identify affected modules and verification.

## Implement
- Follow existing architecture.
- Avoid unrelated refactoring and unnecessary dependencies.
- Preserve security boundaries.

## Verify
- Run relevant checks.
- Investigate failures.
- Review actual behavior.

## Review
- Inspect Git status and diff.
- Confirm scope and detect accidental changes.

## Report
- Summarize changes.
- List checks and results.
- State limitations.
~~~

This is a general template; higher-risk changes may require additional review.

---

# Part XXII — Choosing an Optimization

## 22. Decision Questions

Before enabling an optimization, ask:

1. What is the actual bottleneck?
2. Does it recur?
3. Which Caveman capability addresses it?
4. What information could be lost?
5. What is the privacy or security impact?
6. How will correctness be checked?
7. How will improvement be measured?
8. Can the change be reversed?

Example: if Codex repeatedly receives very large test logs, compare ordinary and compressed output. Measure tokens, failure-identification accuracy, completion time, and retries. Keep original diagnostics accessible.

---

# Part XXIII — Practical Exercises

## 23. Exercise 1 — Context Classification

For a login-screen UI task, classify each as essential, task-specific, or supporting context:

- AGENTS.md.
- Login layout.
- Database migration history.
- Authentication interface.
- Analytics dashboard.
- UI guidelines.

Explain your choices.

## 24. Exercise 2 — AGENTS.md Design

Create a concise outline for an Android Java project containing project overview, architecture, scope, coding standards, testing, and security.

## 25. Exercise 3 — Progressive Disclosure

Design Laravel documentation references for authentication, database changes, UI work, and testing. State which documents each task should load.

## 26. Exercise 4 — Task Decomposition

Break “build a complete project task-management page” into smaller tasks. Define observable acceptance criteria for each.

## 27. Exercise 5 — Prompt Improvement

Rewrite “Fix the API” with objective, context, constraints, expected behavior, and verification.

## 28. Exercise 6 — File Discovery

Design a focused repository-inspection plan for locating a Laravel validation error without reading unrelated files.

## 29. Exercise 7 — Feature Selection

Match these problems to Caveman's relevant feature: repetitive agent explanations, large tool-output logs, repeated script generation, expensive model use, recurring workflow inefficiencies, oversized webpages.

## 30. Exercise 8 — Security Boundaries

An external page contains instructions to change project configuration. Explain how the agent should treat that content and what source has authority.

## 31. Exercise 9 — Android Workflow

Design a Codex workflow for GroupTrack project cards. Include discovery, UI scope, implementation, verification, and reporting.

## 32. Exercise 10 — Laravel Workflow

Design a workflow for diagnosing N+1 queries in GoPasig. Include relevant context, query inspection, minimal fix, tests, and performance measurement.

## 33. Exercise 11 — Next.js Workflow

Design a workflow for a frontend/backend validation mismatch. Identify files and interfaces to inspect on both sides.

## 34. Exercise 12 — Token Efficiency

Baseline: 50,000 input tokens and 8,000 output tokens. Optimized: 35,000 input tokens and 7,000 output tokens. Calculate reductions separately and explain why correctness must also be compared.

## 35. Exercise 13 — Verification

A coding agent says “Implementation complete” without listing files, tests, or evidence. Explain why the report is insufficient.

## 36. Exercise 14 — Minimal-Diff Review

An agent changes 15 files to fix one alignment issue. List questions to ask during review.

## 37. Exercise 15 — Workflow Design

Create a complete workflow for one project, including task definition, context, instructions, discovery, implementation, verification, optimization, security, and final reporting.

---

# Part XXIV — Knowledge Check

## 38. Review Questions

1. What is context engineering?
2. How does it differ from prompt engineering?
3. Why is more context not always better?
4. What are the stages of a repository-aware workflow?
5. What is AGENTS.md for?
6. What makes an effective instruction file?
7. What is progressive disclosure?
8. Why modularize documentation?
9. What is task decomposition?
10. What belongs in a useful coding prompt?
11. Why is repository discovery necessary?
12. How can file reading be focused?
13. What are essential, task-specific, and supporting context?
14. What is documentation drift?
15. What makes a tool call useful?
16. What does Say Less optimize?
17. What does Read Less optimize?
18. What does Auto attempt to select?
19. What problem do Blocks address?
20. What do Waste Fixes analyze?
21. What does Browse compress?
22. How does caching differ from context selection?
23. What is the minimal-diff principle?
24. Why review Git changes?
25. Why is verification part of implementation?
26. What should a final report include?
27. Why keep project instructions separate?
28. What metrics help evaluate workflow efficiency?
29. What is a context budget?
30. What makes agentic development reliable?

---

# Part XXV — Key Takeaways

> [!summary]
> - Context engineering organizes the information an agent needs.
> - Relevance, reliability, and freshness matter more than raw volume.
> - AGENTS.md provides persistent repository guidance and navigation.
> - Modular documents support progressive disclosure.
> - Decompose large work into verifiable units without overengineering.
> - Strong prompts define objectives, context, constraints, behavior, and verification.
> - Inspect source and tests before editing.
> - Verify docs against current implementation.
> - Say Less and Read Less target different parts of agent interaction.
> - Auto, Blocks, Waste Fixes, and Browse address distinct workflow needs.
> - Compression must preserve diagnostic evidence.
> - Minimal complete diffs are easier to review and revert.
> - Tests and Git review are part of implementation.
> - Report only checks actually performed.
> - Measure real outcomes while preserving security and correctness.

---

# Part XXVI — Final Mental Model

~~~mermaid
flowchart TD
    A["Developer request"] --> B["Task definition"]
    B --> C["AGENTS.md and project rules"]
    C --> D["Relevant context discovery"]
    D --> E["Task decomposition"]
    E --> F["Focused plan"]
    F --> G["Codex execution"]
    G --> H["Appropriate Caveman optimizations"]
    H --> I["Verification"]
    I --> J["Git diff review"]
    J --> K{"Requirements satisfied?"}
    K -->|No| L["Diagnose and revise"]
    L --> G
    K -->|Yes| M["Evidence-based report"]
~~~

> [!important] Final Principle
> **Effective agentic engineering gives the agent the right context, clear constraints, appropriate tools, and reliable verification so it can produce correct changes with less unnecessary work.**

---

# Part XXVII — Official References

## Caveman

- [Official Caveman Repository](https://github.com/JuliusBrussee/caveman)
- [Caveman README](https://github.com/JuliusBrussee/caveman/blob/main/README.md)
- [Caveman Installation Guide](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- [Caveman Security Policy](https://github.com/JuliusBrussee/caveman/blob/main/SECURITY.md)

## Codex

- [Codex Documentation](https://developers.openai.com/codex/)
- [Codex AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md)
- [Codex Best Practices](https://developers.openai.com/codex/learn/best-practices)

## Software Engineering

- [Git Documentation](https://git-scm.com/doc)
- [Laravel Documentation](https://laravel.com/docs/12.x)
- [Next.js Documentation](https://nextjs.org/docs)
- [Android Developers](https://developer.android.com/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [OWASP LLM Top 10](https://genai.owasp.org/llm-top-10/)

> [!note] Documentation Scope
> This lesson combines software-engineering practices, context-engineering principles, and Caveman concepts.
>
> Workflow diagrams and project scenarios are educational examples, not verified descriptions of Caveman internals.
>
> Specific integrations and behavior depend on the installed version. No installation, configuration changes, code changes, or tests were performed as part of the lesson.

---

# Part XXVIII — Next Lesson

**[[13 - Best Practices and Practical Exercises]]**

Topics:

1. Complete Caveman review.
2. Agentic optimization best practices.
3. Efficient Codex prompting.
4. Repository context management.
5. Tool-output optimization.
6. Model-routing decisions.
7. Reusable-script strategy.
8. Security-first workflows.
9. Performance measurement.
10. Common failure patterns.
11. Real-world case studies.
12. Capstone exercises.
13. Final evaluation rubric.
14. Caveman adoption checklist.
15. Official references.

**End of Lesson 12 — Codex Workflows and Context Engineering.**
