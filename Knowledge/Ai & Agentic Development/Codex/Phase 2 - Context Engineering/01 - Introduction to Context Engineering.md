# 01 — Introduction to Context Engineering

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 2 — Context Engineering
> **Lesson:** 2.1 — Introduction to Context Engineering
> **Difficulty:** Beginner–Intermediate
> **Status:** In Progress
> **Prerequisite:** Phase 1 — Codex Fundamentals

---

## 1. Introduction

AI coding agents such as OpenAI Codex can analyze existing applications, generate code, implement features, and execute development tools.

However, the quality of their work depends heavily on the information available to them.

A coding agent may understand programming languages and software engineering concepts, but it does not automatically know the specific architecture, requirements, conventions, or development priorities of every project.

For example, when instructed to implement authentication, an agent might consider several valid approaches. However, only one approach may be appropriate for the existing application.

The developer must provide enough information for the agent to understand the project's actual requirements and constraints.

This process is known as **Context Engineering**.

Context Engineering focuses on designing, organizing, and maintaining the information available to an AI system so it can perform tasks more accurately and consistently.

In agentic software development, it is an important practice for controlling implementation scope, preserving architectural consistency, and reducing unnecessary changes.

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define Context Engineering.
2. Explain why context matters in AI-assisted software development.
3. Differentiate Prompt Engineering from Context Engineering.
4. Identify the major sources of context in a software project.
5. Understand how Codex uses project instructions and documentation.
6. Explain the purpose of `AGENTS.md`.
7. Recognize the importance of architectural and coding constraints.
8. Identify common context-management problems.
9. Understand the relationship between context quality and agent reliability.

## 3. What Is Context Engineering?

**Context Engineering** is the practice of preparing and managing relevant information that an AI system needs to perform a task effectively.

In software development, this includes providing the coding agent with information about:

- Project objectives
- Technology stack
- System architecture
- Directory structure
- Coding standards
- Feature requirements
- Development workflows
- Testing procedures
- Implementation constraints

The goal is to make relevant information available when the agent needs it.

### 3.1 Example

Consider the instruction: “Implement a login feature.”

Without sufficient project context, Codex may need to make assumptions about:

- Programming language
- Authentication architecture
- Database structure
- User roles
- Validation requirements
- Existing UI components

Now consider an environment where the project already contains:

```text
project/
├── AGENTS.md
├── docs/
│   ├── architecture.md
│   ├── database-schema.md
│   ├── coding-standards.md
│   └── authentication.md
└── src/
```

These files can provide information about the application's structure and requirements. When Codex accesses relevant documents, it can make decisions informed by the existing project rather than relying entirely on assumptions.

**Key principle:** Context Engineering helps the agent understand how work should be performed within a specific project.

## 4. Prompt Engineering vs. Context Engineering

Prompt Engineering and Context Engineering are closely related, but they focus on different aspects of AI interaction.

### 4.1 Prompt Engineering

Prompt Engineering focuses on constructing effective instructions for a particular task.

Example: “Implement registration form validation. Do not add dependencies, preserve existing behavior, and run relevant tests.”

This describes the requested task and its constraints.

### 4.2 Context Engineering

Context Engineering focuses on preparing the information and environment that support the task.

Examples:

- Maintaining `AGENTS.md`
- Documenting system architecture
- Defining coding conventions
- Organizing feature specifications
- Providing relevant codebase access
- Maintaining testing instructions

### 4.3 Comparison

| Aspect | Prompt Engineering | Context Engineering |
|---|---|---|
| Primary focus | Task instructions | Information available to the agent |
| Scope | Often task-specific | Often project-wide |
| Main question | What should the agent do? | What should the agent know? |
| Example | Implement product search | Document product architecture |
| Typical artifact | Structured task prompt | `AGENTS.md` and project docs |
| Reusability | Depends on the task | Often reusable across tasks |

### 4.4 How They Work Together

Effective agentic development uses both.

**Context Engineering** establishes the project's technical and operational knowledge.

**Prompt Engineering** provides the specific task to perform using that knowledge.

Example:

- Context: “The application uses Laravel, PostgreSQL, and a service-based architecture.”
- Prompt: “Implement the approved product search endpoint using the existing service layer.”

Together, these help the agent work within the intended architecture.

## 5. Why Context Engineering Matters

### 5.1 Reduces Unnecessary Assumptions

Without project context, an agent may select an implementation that is technically valid but inconsistent with the application. Relevant documentation reduces the need for unsupported assumptions.

### 5.2 Improves Architectural Consistency

An agent that understands the existing architecture is better positioned to follow established patterns. For example, a Laravel application may use controllers for HTTP handling and services for business logic. Without that information, an agent might place complex logic directly inside controllers.

### 5.3 Helps Prevent Overengineering

Project documentation can explicitly prohibit unnecessary dependencies, abstractions, or out-of-scope features.

### 5.4 Supports Consistent Development

Shared instructions allow different tasks and sessions to follow the same engineering standards.

### 5.5 Improves Maintainability

Consistent architectural decisions and coding conventions make generated changes easier to understand and maintain.

### 5.6 Supports Verification

Documentation can define how the agent should test changes and report incomplete validation.

**Important:** Better context improves the conditions for reliable work but does not guarantee correct code. Developer review remains necessary.

## 6. The Main Sources of Context

A coding agent can receive information from multiple sources.

### 6.1 Developer Instructions

Instructions provided directly in the current conversation.

Example: “Analyze the authentication module without modifying files.”

### 6.2 Project Source Code

Existing application files provide information about actual implementation details.

Examples include controllers, models, services, UI components, configuration files, and tests.

### 6.3 AGENTS.md

`AGENTS.md` provides project-specific instructions to Codex. It may define:

- Project overview
- Architecture conventions
- Development rules
- Testing commands
- Implementation constraints

### 6.4 Project Documentation

Documentation explains requirements and architectural decisions that may not be obvious from the source code alone.

### 6.5 Tool Results

Terminal commands, file searches, and test results provide additional information during task execution.

### 6.6 Conversation History

Earlier instructions and decisions may remain available during a session. However, important project requirements should not depend exclusively on conversation history.

## 7. Understanding the Context Engineering Workflow

A practical Context Engineering workflow includes five stages.

### Stage 1 — Identify Required Knowledge

Determine which information the agent needs. Examples:

- What is the application's purpose?
- Which technology stack is used?
- What architecture must be followed?
- Which features are in scope?
- Which actions are prohibited?

### Stage 2 — Organize the Information

Separate information into appropriate documents.

```text
docs/
├── overview.md
├── architecture.md
├── database-schema.md
├── features.md
├── coding-standards.md
└── testing.md
```

### Stage 3 — Define Agent Instructions

Create or maintain `AGENTS.md` with concise project guidance. It can direct the agent toward relevant documentation.

### Stage 4 — Provide Task-Specific Context

For each task, specify the relevant feature, expected outcome, and constraints.

### Stage 5 — Validate Agent Behavior

Inspect whether the agent followed project conventions, respected implementation boundaries, modified only relevant files, completed required verification, and reported uncertainty accurately.

## 8. Understanding AGENTS.md

`AGENTS.md` is a Markdown file used to provide instructions to coding agents such as Codex. It helps establish persistent project guidance.

### 8.1 Example

```markdown
# Project Instructions

## Overview

This project is an Android application developed
using Java and XML layouts.

## Architecture

Follow the existing application structure.

## Development Rules

- Reuse existing components.
- Avoid unnecessary dependencies.
- Do not modify unrelated features.
- Keep implementations within the approved scope.

## Verification

- Run relevant build checks.
- Review modified files.
- Report tests that could not be completed.
```

### 8.2 Why AGENTS.md Is Useful

Instead of repeating the same project rules in every prompt, developers can maintain reusable instructions in the repository. The file should remain concise and relevant. Detailed architectural documentation can be stored separately and referenced when needed.

### 8.3 Important Distinction

`AGENTS.md` is not a replacement for the actual source code or feature specifications. It should help the agent discover and follow the appropriate project information.

**Related lesson:** [[03 - Working with AGENTS.md]]

## 9. Project Documentation as Context

Project documentation helps explain information that may not be immediately obvious from the codebase.

### 9.1 Common Documentation Files

| Document | Purpose |
|---|---|
| `overview.md` | Explains the project's purpose and scope |
| `architecture.md` | Describes system components and relationships |
| `database-schema.md` | Documents entities and data relationships |
| `features.md` | Defines feature requirements |
| `coding-standards.md` | Establishes coding conventions |
| `workflow.md` | Defines development procedures |
| `testing.md` | Explains validation requirements |

### 9.2 Example Structure

```text
project/
├── AGENTS.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── database-schema.md
│   ├── features.md
│   ├── coding-standards.md
│   ├── workflow.md
│   └── testing.md
├── src/
└── README.md
```

This is an example, not a mandatory structure. Documentation should match the project's complexity. A small application may need only a README and AGENTS.md. A larger application may benefit from more detailed documentation.

## 10. Context Quality vs. Context Quantity

A common misconception is that providing more context always produces better results. Excessive information may make it harder to identify what is relevant.

### 10.1 Low-Quality Context

Examples include outdated documentation, conflicting instructions, duplicate requirements, unrelated technical information, and unclear architectural decisions.

### 10.2 High-Quality Context

Useful context is relevant, accurate, consistent, organized, up to date, and easy to locate.

### 10.3 Example

A task involves improving an Android login layout. Relevant context may include the existing XML layout, UI conventions, relevant resources, and supported screen sizes. Unrelated documentation about database analytics may not be necessary.

**Key principle:** Select context according to the task rather than supplying it indiscriminately.

## 11. Context Engineering and Context Windows

AI models have limited context windows. A context window determines how much information can be processed within an active model interaction. Large codebases may contain far more information than can fit into a single context window.

Effective context engineering therefore emphasizes selective information retrieval.

### 11.1 Practical Strategies

- Keep project instructions concise.
- Organize documentation by topic.
- Reference relevant files.
- Avoid unnecessary duplication.
- Maintain clear feature boundaries.
- Use focused development tasks.

### 11.2 Example

Instead of placing an entire system specification into every prompt, say: “Read the authentication requirements in `docs/features/authentication.md` and inspect the relevant source files before proposing changes.”

This directs the agent toward relevant information.

**Related lesson:** [[02 - Understanding Context Windows and Context Management]]

## 12. Real-World Example — Android Project

### Scenario

A developer is building GroupTrack, an Android student project management application. The project uses Android Studio, Java, XML layouts, Firebase, and Git. The development team wants to keep implementation consistent with a defined MVP.

### 12.1 Without Context Engineering

A developer asks: “Implement task management.”

The agent may make assumptions about feature scope, data model, UI architecture, Firebase integration, and additional dependencies.

### 12.2 With Context Engineering

The project contains:

```text
GroupTrack/
├── AGENTS.md
├── docs/
│   ├── overview.md
│   ├── architecture.md
│   ├── schema.md
│   ├── features.md
│   ├── coding-standards.md
│   ├── workflow.md
│   └── testing.md
└── app/
```

The developer provides this prompt:

“Inspect AGENTS.md and the relevant feature and architecture documentation. Analyze the existing task management implementation and propose the smallest solution consistent with the documented MVP. Do not implement anything yet.”

### 12.3 Expected Benefits

The agent has a clearer basis for understanding feature boundaries, following established architecture, avoiding unnecessary dependencies, identifying relevant files, and proposing a maintainable implementation.

This does not eliminate the need for developer review.

## 13. Context Engineering vs. Overengineering

Context Engineering should simplify development rather than create unnecessary documentation overhead.

### 13.1 Overengineered Approach

A small practice application contains dozens of instruction files, overlapping architecture documents, and repeated coding rules. This creates maintenance problems and increases the chance of contradictions.

### 13.2 Practical Approach

Start with the minimum useful documentation.

```text
project/
├── AGENTS.md
├── README.md
└── src/
```

Add dedicated documents when the project requires them.

### 13.3 Recommended Principle

**Document what the agent needs to know, not everything that could possibly be documented.**

## 14. Common Context Engineering Mistakes

### Mistake 1 — No Project Instructions

The agent repeatedly makes assumptions about architecture and conventions.

**Improvement:** Maintain concise project guidance.

### Mistake 2 — Outdated Documentation

The agent may follow information that no longer reflects the actual application.

**Improvement:** Update documentation when important decisions change.

### Mistake 3 — Conflicting Requirements

Different files provide contradictory instructions.

**Improvement:** Establish a clear source of truth for each topic.

### Mistake 4 — Excessive Documentation

Too many overlapping files increase complexity.

**Improvement:** Keep documentation proportional to project needs.

### Mistake 5 — Relying Only on Conversation History

Important decisions may become unavailable or difficult to retrieve.

**Improvement:** Store durable requirements in project documentation.

### Mistake 6 — Treating Documentation as Automatically Correct

Documentation may be incomplete or inconsistent with source code.

**Improvement:** Verify important claims against the actual implementation.

## 15. Practical Exercise — Context Identification

### Objective

Identify the information a coding agent needs before implementing a feature.

### Scenario

You are developing an Android application using Java and Firebase. The task is to implement a new task creation screen.

### Part A — Identify Required Context

Determine which information the agent needs. Consider:

1. Existing application architecture
2. Navigation structure
3. UI conventions
4. Firebase data model
5. Task creation requirements
6. Validation rules
7. Testing procedures

### Part B — Identify Documentation Sources

For each requirement, identify where the information should be stored.

| Information | Possible source |
|---|---|
| Project purpose | `overview.md` |
| Architecture | `architecture.md` |
| Feature requirements | `features.md` |
| Firebase data model | `schema.md` |
| Coding conventions | `coding-standards.md` |
| Agent instructions | `AGENTS.md` |

### Part C — Write a Context-Aware Prompt

Example:

“Read AGENTS.md and the relevant architecture, feature, and schema documentation. Inspect the existing Android navigation and UI components. Propose a task creation screen consistent with the current MVP. Identify affected files and validation requirements. Do not modify files yet.”

### Part D — Reflection

1. What information does the agent need?
2. Which information belongs in AGENTS.md?
3. Which information belongs in dedicated documentation?
4. What assumptions should the agent avoid?
5. How can you verify that the agent followed the project context?

## 16. Key Terminologies

| Term | Definition |
|---|---|
| Context Engineering | Preparing and managing information for an AI system |
| Prompt Engineering | Designing effective task instructions |
| Project context | Information describing a specific software project |
| `AGENTS.md` | Markdown file providing instructions to coding agents |
| Context window | Limit on information processed in an active model interaction |
| Architecture documentation | Description of system components and design decisions |
| Coding standards | Rules governing implementation conventions |
| Source of truth | Authoritative location for a particular requirement |
| Context retrieval | Obtaining relevant information when needed |
| Context quality | Relevance, accuracy, and consistency of available information |

## 17. Key Takeaways

1. **Context Engineering prepares the information an AI coding agent needs to perform tasks effectively.**
2. **Prompt Engineering focuses on instructions, while Context Engineering focuses on supporting information and the environment.**
3. **Project documentation helps agents understand architecture, requirements, and constraints.**
4. **AGENTS.md provides reusable guidance for Codex within a project.**
5. **Relevant context reduces unsupported assumptions and improves consistency.**
6. **More context is not always better; relevance and accuracy matter more than volume.**
7. **Context Engineering should remain proportional to project complexity.**
8. **Documentation must be maintained and verified against the actual codebase.**
9. **Human oversight remains essential even when project context is well organized.**

## 18. Knowledge Check

1. What is Context Engineering?
2. How does it differ from Prompt Engineering?
3. Why does an AI coding agent need project context?
4. What are the main sources of context?
5. What is the purpose of AGENTS.md?
6. Why should project documentation be organized by topic?
7. What is the relationship between context windows and context management?
8. Why is excessive documentation sometimes harmful?
9. What is a source of truth?
10. How does Context Engineering help prevent overengineering?

## 19. Official References

1. **OpenAI — Codex Documentation**
   https://developers.openai.com/codex/
2. **OpenAI — AGENTS.md Guide**
   https://developers.openai.com/codex/guides/agents-md/
3. **OpenAI — Codex Prompting**
   https://developers.openai.com/codex/prompting/
4. **OpenAI — Codex GitHub Repository**
   https://github.com/openai/codex
5. **OpenAI — Prompt Engineering**
   https://developers.openai.com/api/docs/guides/prompt-engineering

## 20. Lesson Completion Checklist

- [ ] Understand Context Engineering.
- [ ] Differentiate Context Engineering from Prompt Engineering.
- [ ] Identify the major sources of project context.
- [ ] Understand the purpose of AGENTS.md.
- [ ] Recognize the importance of project documentation.
- [ ] Understand context quality versus quantity.
- [ ] Identify common context-management problems.
- [ ] Explain how context helps prevent overengineering.
- [ ] Complete the practical exercise.
- [ ] Answer the knowledge-check questions.

---

## Next Lesson

**[[02 - Understanding Context Windows and Context Management]]**

The next lesson explores how context windows work, why context becomes limited, and how developers can manage information efficiently during long-running Codex sessions.

---

*Phase 2 — Context Engineering | Lesson 2.1*
*Learning Principle: Provide the right information, at the right time, for the right task.*
