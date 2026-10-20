# 05 — Prompting Fundamentals

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 1 — Codex Fundamentals  
> **Lesson:** 1.5 — Prompting Fundamentals  
> **Difficulty:** Beginner–Intermediate  
> **Platform:** Windows / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[04 - CLI Navigation and Essential Commands]]

---

## 1. Introduction

Effective prompting means explaining the outcome you want, the context Codex needs, and any boundaries it should follow. This matters because Codex can use tools to inspect or change a project, depending on its permissions.

A prompt does not need to be long. It needs to be clear enough for the task. Even a well-written prompt cannot guarantee correct results, so review the work and verify it.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Explain prompting and prompt engineering.
- Identify the task, context, constraints, success criteria, and verification steps in a prompt.
- Scope work to avoid unrelated changes.
- Distinguish analysis, planning, implementation, debugging, and review prompts.
- Refine an unclear prompt and identify when Codex should ask before proceeding.

## 3. What Is Prompting?

A **prompt** is an instruction or information given to an AI system. **Prompt engineering** is the practice of designing and refining instructions to get useful, relevant results. It improves clarity; it does not guarantee correctness.

For example:

**General question:** “Explain how authentication works in Laravel.”

**Project analysis request:** “Inspect this Laravel project’s authentication flow. Identify the relevant routes, middleware, controllers, and models. Explain how login works. Do not modify files.”

The second prompt tells Codex what to inspect, what to report, and what not to change.

## 4. A Practical Prompt Structure

Use this learning framework when a task needs more detail:

**Task + Context + Constraints + Acceptance Criteria + Verification**

This is a practical template for the course, not special Codex syntax. Simple requests may need only a sentence or two.

### Task — What outcome do you want?

Start with a concrete action, such as analyze a module, fix a bug, or add a search field.

### Context — What does Codex need to know?

Mention the relevant application, technology, feature, or documentation. Codex can inspect files when permitted, so direct it toward the context relevant to the task rather than pasting the whole project into the prompt.

### Constraints — What boundaries should it follow?

Examples: do not change backend logic, add dependencies, or modify unrelated files; follow existing project conventions.

### Acceptance criteria — What should be true when complete?

State observable conditions, such as an empty required field showing a validation message or search filtering results case-insensitively.

### Verification — How should the result be checked?

Ask for relevant tests or checks, a summary of changed files, and clear reporting of anything that could not be verified.

### Example

> Add client-side validation to the existing registration form. Inspect the authentication module and follow its conventions. Do not change backend logic, add dependencies, or modify unrelated components. Required fields must show errors, invalid email formats must be rejected, and password confirmation must match. Run the relevant available checks and summarize the changed files and results.

## 5. Scope and Non-Goals

**Scope** defines what the task includes. **Non-goals** describe related work that should stay out of the task.

For an Android login layout adjustment:

- **In scope:** alignment, spacing, and layout positioning.
- **Out of scope:** authentication logic, Firebase configuration, navigation architecture, and new UI libraries.

Clear scope makes changes easier to review and reduces the chance of unrelated work. Ask for the simplest maintainable solution that fits the existing architecture; avoid requesting redesigns or abstractions that the task does not need.

## 6. Prompt Types for Development Work

### Analysis

Understand existing behavior without edits.

> Inspect the authentication module and explain the login flow, including the relevant files and their roles. Do not modify files.

### Planning

Propose an approach before implementation.

> Inspect the product module and propose a plan for product search. Identify likely affected files, risks, and verification steps. Do not implement yet.

### Implementation

Carry out a sufficiently defined task.

> Implement the approved product-search approach. Follow existing conventions, keep changes within the agreed scope, and avoid unnecessary dependencies.

### Debugging

Investigate an error using evidence such as logs, reproduction steps, and expected versus actual behavior.

> Investigate this registration validation error using the relevant code and tests. Explain the likely cause and proposed fix before changing files.

### Verification or review

Check changes against requirements and report findings.

> Review the current diff against the acceptance criteria. Run relevant checks and report bugs, missing coverage, or unverified behavior. Do not modify files.

## 7. Match the Prompt to the Task

A useful workflow is **Explore → Plan → Implement → Verify → Review**, but not every task needs separate prompts for every stage.

- For a small, clear task, one scoped prompt may be enough.
- For a large or uncertain task, begin with read-only analysis and planning.
- Review the plan before implementation when requirements, architecture, or data may be affected.
- After changes, inspect the diff and verify relevant behavior.

This is a flexible workflow, not a requirement to create extra steps for simple tasks.

## 8. Improve a Vague Prompt

**Vague:** “Make my product page better.”

**Clearer:** “Add case-insensitive client-side search by product name to the existing product page. Follow the current component and styling conventions. Do not change the backend, add dependencies, or redesign the page. Clearing the search should show all products again. Run available lint and type checks and report any behavior that needs manual browser testing.”

The improved version defines an outcome, relevant context, boundaries, success criteria, and verification.

## 9. Handling Ambiguity

When important requirements are missing, tell Codex to investigate and report questions before implementation. Do not ask it to invent decisions that affect architecture, data, or external services.

Example:

> Inspect this application and list the decisions needed before implementing payments, including provider, supported methods, and transaction flow. Do not modify files. Identify assumptions and ask for clarification before proposing implementation details.

Useful stop conditions include:

- Pause if requirements conflict or an API contract is missing.
- Ask before changing a database schema or external integration.
- Report checks that could not be run instead of claiming success.

## 10. Reusable Prompt Template

```text
Task: [Describe the desired outcome.]

Context: [Name the project area or relevant files and conventions.]

Constraints: [State boundaries and changes to avoid.]

Acceptance criteria:
- [Observable condition]
- [Observable condition]

Verification: [Checks to run and what to report.]

If requirements are missing or conflict, stop and ask before making assumptions.
```

Use only the sections that help clarify the task. Avoid repeating repository information Codex can discover or adding instructions that do not affect the result.

## 11. Prompting Best Practices and Common Mistakes

| Practice | Why it helps |
|---|---|
| State the desired outcome | Gives the task a clear target |
| Point to relevant context | Helps Codex investigate the right area |
| Set scope and non-goals | Reduces unrelated edits |
| Make criteria observable | Makes completion easier to judge |
| Ask for evidence and limitations | Keeps reports grounded in actual checks |
| Keep instructions proportional | Avoids clutter and conflicting requirements |
| Protect secrets | Prevents unnecessary exposure of credentials |

Common mistakes include vague tasks (“fix my app”), missing boundaries, success criteria that cannot be checked, asking for changes before understanding uncertain requirements, and assuming a confident summary proves the work is correct.

## 12. Practice Exercise — Rewrite the Prompt

This exercise is for a later practice session. No project changes are required now.

Rewrite each vague request using the prompt structure:

1. **Android:** “Improve my splash screen.” Limit the request to layout and preserve app behavior.
2. **Laravel:** “Fix my API.” Focus on a specific endpoint with validation errors; tell Codex what it should inspect first.
3. **Next.js:** “Make my product page responsive.” Define supported layouts, existing design constraints, out-of-scope work, and verification.

Then compare your rewritten prompts. Can someone identify the desired result, boundaries, and success conditions without guessing?

## 13. Knowledge Check

1. What is prompting, and what does prompt engineering try to improve?
2. Name the five parts of the practical prompt structure.
3. How do constraints differ from acceptance criteria?
4. When is it useful to separate planning from implementation?
5. What is a non-goal?
6. What should Codex do when a critical requirement is missing?
7. Why does a good prompt still require verification?
8. Why are longer prompts not always better?

## 14. Key Takeaways

1. Clear prompts state the desired outcome and provide only relevant context.
2. Constraints and non-goals help keep the work within scope.
3. Acceptance criteria make success observable.
4. Analysis, planning, implementation, and review prompts serve different purposes.
5. Ask Codex to pause when important decisions or requirements are missing.
6. Inspect changes and verification evidence; prompting does not replace developer judgment.

## Official References

- [OpenAI Codex Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide)
- [Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)
- [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
- [Codex Documentation](https://developers.openai.com/codex/)

## Next Lesson

**[[06 - Tools, Permissions, Approvals, and Sandboxing]]**

---

*Phase 1 — Codex Fundamentals | Lesson 1.5*  
*Learning Principle: Define the outcome, give relevant context, set boundaries, and verify the result.*
