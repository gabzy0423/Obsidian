# 01 — Introduction to OpenAI Codex

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 1 — Codex Fundamentals  
> **Lesson:** 1.1 — Introduction to OpenAI Codex  
> **Difficulty:** Beginner  
> **Status:** In Progress  
> **Prerequisites:** None; basic programming knowledge is helpful

> **Study note:** This lesson explains Codex and its workflows. Examples are for discussion; we are not starting a coding project.

## 1. Why Learn About Codex?

Software developers traditionally inspect project files, write code, run commands, find errors, and test changes themselves. AI tools can now assist with parts of this work.

OpenAI Codex is a coding agent: it can work with project context and, when the environment allows, use tools to inspect files, make changes, or run commands. This is different from asking a chatbot for a snippet because an agent may take several steps in a development environment.

This way of working is often called **agentic coding**. The person still sets the goal, gives constraints, reviews the work, and decides what to accept. Codex can assist, but it does not remove the need for human judgment.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Codex and explain its purpose.
2. Explain what an AI coding agent is.
3. Describe how an agent differs from a chatbot that only replies.
4. Name common Codex capabilities and limitations.
5. Recognize the main ways people can access Codex.
6. Explain the developer’s role when using an AI agent.
7. Give examples of tasks Codex may assist with.

## 3. What Is OpenAI Codex?

Codex is OpenAI’s coding agent for software development. It can help people understand, write, review, and debug code. Depending on the interface, available tools, and permissions, it can also inspect a project, change files, or run development commands.

### Common capabilities

- **Understand code:** Find relevant files and explain how code works.
- **Generate or modify code:** Draft new code or change existing files when asked and permitted.
- **Investigate problems:** Examine an error and suggest or make a fix.
- **Test and review:** Run available checks or inspect changes, then report what it found.

What Codex can actually do depends on the tools and permissions in its environment. It may not have access to every file, command, or service.

### Example: Understanding an Android project

Suppose a developer is unfamiliar with an Android app written in Java. They could ask Codex to inspect the project structure and explain where the login screen and validation are handled. They can ask for an explanation only, with no files changed.

A development task might go further and ask Codex to plan or implement a login screen. That is an example of a coding task, not an exercise for this lesson. Any proposed changes would need review and verification.

## 4. AI Assistant and AI Coding Agent

An **AI assistant** mainly responds to questions or instructions. An **AI agent** can also use available tools to take actions toward a goal.

| Aspect | Chat assistant | Coding agent |
|---|---|---|
| Main activity | Answers in a conversation | Answers and may perform tool-assisted steps |
| Code help | Can explain or draft code | Can also inspect project files when given access |
| File changes | Usually provides suggestions | May edit files if the environment permits it |
| Commands and tests | Can suggest commands | May run permitted commands and report results |
| Oversight | Check the answer | Review both the answer and any actions or changes |

These categories can overlap. Some chat assistants also have tools. The useful distinction is whether the system can take actions in an authorized environment, not what it is called.

## 5. Agentic Coding

**Agentic coding** means working with an AI agent on a software task that may involve several steps, such as exploring a codebase, planning a change, using tools, and checking results.

A typical development workflow looks like this:

1. The developer describes the goal and constraints.
2. Codex examines relevant project context, if it has access.
3. Codex proposes or follows an approach.
4. It may make authorized changes or run available tools.
5. It reports what it did and what the tools showed.
6. The developer reviews the outcome and decides what to do next.

**Core idea:** The agent can do delegated work. The person provides direction, sets boundaries, checks the result, and remains responsible for decisions.

## 6. Where Codex Is Available

OpenAI currently documents Codex experiences in the command line, IDE extension, cloud, and ChatGPT desktop, web, and mobile apps. Features and availability can differ by interface and may change over time. See the [Codex documentation](https://learn.chatgpt.com/docs) for current details.

### Codex CLI

Codex CLI is used from a terminal. The basic launch command is:

```powershell
codex
```

This is an example for reference; we do not need to install or run it during this lesson. In a real session, the user would sign in and give Codex a task in the chosen project context.

### IDE extension

An IDE extension brings Codex into a supported code editor. This can make it easier to ask questions while looking at a project.

### Cloud and ChatGPT apps

Codex can also be used through cloud workflows and Codex experiences in ChatGPT apps. The exact setup and behavior depend on the product and account access.

For this learning track, CLI is the primary tool we will study conceptually. We can learn what its commands and permissions mean before deciding to run anything.

## 7. A Simple Model of How Codex Works

Think of Codex as a loop involving the user, project context, an AI model, and tools:

1. **Instruction:** The user describes what they want and any limits.
2. **Context:** Codex uses the instructions and project information available to it.
3. **Action:** It chooses a next step, such as searching files or explaining code.
4. **Tool use:** It may use permitted tools; some actions may require approval.
5. **Review of results:** It interprets tool output and may continue the task.
6. **Report:** It summarizes findings or changes for the user to review.

This is a simplified model. The details vary by Codex interface, configuration, and task.

## 8. Example Use Cases

These are examples to help us understand Codex. We can discuss them without making code changes.

### Understand an unfamiliar project

> “Explain the main folders and describe how a request moves through the controllers, services, and models. Do not change any files.”

### Plan a feature

> “Review the product search area and outline an implementation plan that follows the existing architecture. Do not edit files.”

### Investigate an error

> “Explain what this error may mean, what information would help find its cause, and how a developer could investigate it. Do not make changes.”

### Review a proposed change

> “Explain what a Git diff shows and list the kinds of issues a reviewer should look for. Do not modify files.”

## 9. Capabilities, Limits, and Human Oversight

### What Codex may help with

- Exploring an unfamiliar codebase
- Planning scoped changes
- Implementing features
- Investigating bugs
- Creating or running tests
- Refactoring code
- Reviewing changes

These are possible uses, not guarantees. They depend on access, tools, configuration, and the quality of the instructions.

### What to keep in mind

- **It can be wrong:** Code or explanations may contain mistakes.
- **It may lack context:** Missing instructions or documentation can lead to incorrect assumptions.
- **Its access is limited:** Permissions, sandboxing, and available tools control what it can do.
- **Unclear requests can cause surprises:** Vague scope may lead to unintended work.
- **Passing checks is not proof:** A command or test can pass while other problems remain.

### The developer’s responsibilities

A developer using Codex should set clear requirements and boundaries, provide relevant context, review proposed changes, check results, and make the final decisions. For this learning track, we will focus on understanding and reviewing these responsibilities rather than writing code.

## 10. Key Terms

| Term | Plain-language meaning |
|---|---|
| **Codex** | OpenAI’s coding agent for software development |
| **Codex CLI** | A way to use Codex from a terminal |
| **AI agent** | An AI system that can use tools to work toward a goal |
| **Agentic coding** | Software work where a person directs and reviews an AI agent’s multi-step work |
| **Prompt** | The instruction or question given to the AI |
| **Context** | The project information and instructions available to the agent |
| **Tool** | A capability the agent can use, such as searching files or running a command |
| **Sandbox** | A restricted environment that limits what actions are allowed |
| **Approval** | A user’s authorization for an action that requires permission |
| **Git diff** | A view of the changes between versions of files |

## 11. Key Takeaways

1. Codex is a coding agent that can work with project context and tools.
2. Its ability to act depends on the interface, available tools, and permissions.
3. Agentic coding involves a person directing and reviewing multi-step work.
4. Codex can make mistakes or miss context, so its output needs review.
5. The person remains responsible for requirements, quality, security, and final decisions.
6. We can study Codex’s concepts and examples without starting a coding project.

## 12. Knowledge Check

Try answering these in your own words:

1. What is Codex designed to help with?
2. What makes a coding agent different from a chatbot that only provides suggestions?
3. What does “agentic coding” mean?
4. Name three tasks Codex may help with.
5. Why can’t we assume that a passing test proves everything is correct?
6. What do permissions and sandboxing control?
7. What should a person review before relying on Codex’s work?

### Reflection

Use a **hypothetical** software project. Describe one question you could ask Codex to help understand it. What context would Codex need, and how would you judge whether its explanation is useful?

## 13. Official References

- [Codex documentation](https://developers.openai.com/codex/)
- [Codex CLI documentation](https://developers.openai.com/codex/cli)
- [Codex IDE extension documentation](https://developers.openai.com/codex/ide)
- [Codex Cloud documentation](https://developers.openai.com/codex/cloud)
- [Codex learning resources](https://developers.openai.com/learn/codex)
- [OpenAI Codex GitHub repository](https://github.com/openai/codex)

## Next Lesson

**[[02 - Codex Architecture and Agentic Coding Concepts]]**

---

*Phase 1 — Codex Fundamentals | Lesson 1.1*  
*Learning principle: Understand the tool before relying on it.*
