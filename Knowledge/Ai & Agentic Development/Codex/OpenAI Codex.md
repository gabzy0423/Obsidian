# OpenAI Codex — Learning Overview

> **Learning track:** AI-assisted software development and agentic coding  
> **Primary tool:** OpenAI Codex CLI  
> **Current approach:** Theory → Demonstration → Review  
> **Status:** In progress

## Introduction

OpenAI Codex is an AI coding agent for software development. It can help explain, write, review, and debug code. Depending on the interface and permissions, it can also inspect project files and use tools in a development environment.

Learning Codex is more than memorizing commands. It means giving clear instructions, providing useful project context, understanding permissions, and checking the agent’s work. Our focus is to understand these ideas first; we are not starting a coding project.

## Learning Objectives

By the end of this learning track, you should be able to:

1. Explain what Codex does and understand its capabilities and limits.
2. Navigate Codex CLI and understand its interactive commands.
3. Write clear, actionable instructions for Codex.
4. Explain how project context and `AGENTS.md` guide Codex.
5. Describe a structured workflow for agent-assisted development.
6. Understand configuration, permissions, and security considerations.
7. Explain advanced concepts such as MCP and reusable agent skills.
8. Review Codex’s work critically instead of relying on it without understanding.

## Prerequisites

Helpful background, but not required to begin:

- Basic programming concepts
- Familiarity with a command line
- Understanding of files, folders, and project structure
- Basic Git and GitHub concepts
- Experience with a code editor or IDE

We can learn the concepts even if some of these are new.

## How We’ll Learn

1. **Understand the concept** — Learn what a feature does and why it matters.
2. **See a demonstration** — Follow a simple example of Codex in use.
3. **Try guided, non-coding practice** — Explore commands or ask Codex to explain sample code without building a project.
4. **Review and reflect** — Examine what Codex did, what it got right, and what needs human judgment.
5. **Record what we learn** — Keep useful concepts, commands, and lessons in this vault.

## Learning Roadmap

### 1. Codex Fundamentals — Beginner

Understand Codex as a coding agent and how a person works with it.

- Codex CLI installation and authentication
- Interactive CLI commands
- Effective prompts
- Asking Codex to explain or analyze code
- Approvals, permissions, and sandboxing

### 2. Context Engineering

Learn how to give Codex the information and rules it needs to understand a project.

- `AGENTS.md` instructions
- Project documentation in `docs/`
- Coding standards and architecture rules
- Task scope and feature constraints
- Avoiding overengineering

### 3. Agentic Coding Workflow

Understand the stages of a development task. We’ll study the process before deciding whether to try coding tasks.

**Explore → Plan → Implement → Test → Review**

- Breaking tasks into smaller steps
- Working with existing codebases
- Debugging and refactoring
- Understanding Git diffs and commits
- Rollbacks and recovery

### 4. Codex Configuration and Customization

Learn how settings affect Codex and how instructions can be reused.

- `config.toml`
- Model and reasoning settings
- Approval and sandbox configuration
- Project-specific configuration
- Reusable instructions and skills

### 5. Advanced Codex

Explore features for connecting Codex to tools and handling more complex work.

- Model Context Protocol (MCP)
- Custom agent skills
- Multi-agent workflows, where supported
- Automations and scripting
- Managing large codebases
- Token efficiency and context management

## What You’ll Understand

The goal is to understand how Codex can assist with tasks such as exploring unfamiliar code, planning scoped changes, debugging, testing, reviewing diffs, and following project conventions. These are examples to learn about; we do not need to perform them as coding exercises.

The larger goal is to know how to direct, supervise, and evaluate an AI coding agent while keeping human judgment and responsibility in the loop.

## Learning Navigation

- [[01 - Learning Roadmap]]
[[OpenAI Codex]]]]
- [[02 - Codex CLI Basics]]
- [[03 - Prompting Fundamentals]]
- [[04 - Tools and Permissions]]
- [[05 - Hands-on Practice]]

## Official References

- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [OpenAI Codex GitHub Repository](https://github.com/openai/codex)
- [OpenAI Docs: Code generation](https://developers.openai.com/api/docs/guides/code-generation)

---

*Learning principle: Understand the task, guide the agent, verify the results, and take responsibility for the outcome.*
