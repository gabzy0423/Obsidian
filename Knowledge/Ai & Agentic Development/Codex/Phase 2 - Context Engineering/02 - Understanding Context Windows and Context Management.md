# 02 — Understanding Context Windows and Context Management

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 2 — Context Engineering  
> **Lesson:** 2.2 — Understanding Context Windows and Context Management  
> **Difficulty:** Beginner–Intermediate  
> **Platform:** Windows / PowerShell / Codex CLI  
> **Status:** In Progress  
> **Prerequisite:** [[01 - Introduction to Context Engineering]]

---

## 1. Introduction

Coding agents such as Codex use project instructions, conversation messages, source code, documentation, and tool results to work on development tasks. A model can process only a limited amount of information in one context window.

**Context management** is the practice of keeping the information needed for the current task available, while reducing irrelevant or outdated details. It also means recording important decisions somewhere durable so work can continue later.

This lesson explains context windows and tokens, how context grows, compaction, session continuity, and ways to preserve useful project knowledge.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

- Define a context window and explain tokens at a high level.
- Identify information that may be included in an agent's context.
- Explain why long sessions benefit from focused context management.
- Distinguish Codex CLI `/compact` from API compaction.
- Differentiate conversation history from persistent project documentation.
- Choose when to continue a session, resume one, or start a new one.
- Prepare a concise handoff for work that continues later.

## 3. What Is a Context Window?

A **context window** is the maximum number of tokens that can be used in a model request. Depending on the model and interaction, this can include input, output, and reasoning tokens. The size varies by model; do not assume one fixed limit for every Codex session.

Context can include:

- User and project instructions
- Conversation history
- Relevant source code and documentation
- Tool calls and their results
- The model's response and, for some models, reasoning tokens

A larger context window does not make every detail equally relevant. Keeping the information focused still matters. Including too much can use capacity without helping the task.

## 4. Understanding Tokens

A **token** is a unit a language model uses to represent text and other input. Tokens do not correspond exactly to words: a word may be one token or several, depending on the tokenizer and content.

Code, JSON, logs, documentation, and natural-language prompts all use tokens when provided to a model. Large files and verbose command output can therefore take up substantial context.

Do not confuse these terms:

| Term | Meaning |
|---|---|
| Context window | Maximum token capacity for a model request |
| Token usage | Tokens processed or generated |
| Usage or rate limit | Product/account limits on use or request frequency |
| Compaction | Reducing conversation context while preserving useful state |

Compaction may reduce active context; it does not reset account usage or rate limits.

## 5. What Contributes to Context in a Coding Session?

Depending on the Codex workflow, relevant context may include:

1. **Instructions:** The task and any applicable `AGENTS.md` guidance.
2. **Project information:** Source files, configuration, and documentation that Codex inspects.
3. **Tool activity:** Search results, command output, test results, and error logs.
4. **Conversation:** Earlier prompts, decisions, and responses still available to the session.

This does not mean the entire repository is always loaded into context. The agent can retrieve information as it works, and the amount available depends on the product and task.

## 6. How Context Grows

A multi-step investigation may involve reading project instructions, finding relevant files, running a command, examining its output, making a change, and checking the result. Each relevant message or tool result can add information to the session.

Some details remain important, such as the task requirements and approved design decisions. Other details may become redundant, such as repeated file excerpts or old logs from failed approaches.

**The goal is not to keep every interaction. Keep the information needed to continue correctly.**

## 7. Context Management

Context management includes:

- **Selection:** Focus on information relevant to the task.
- **Organization:** Keep durable project guidance easy to find.
- **Retrieval:** Inspect more files when evidence shows they are needed.
- **Reduction:** Summarize or compact older conversation when appropriate.
- **Preservation:** Record important decisions outside the conversation.
- **Verification:** Recheck the objective and current project state before continuing.

For example, a UI alignment task may need a screen layout, related resources, and UI conventions. It likely does not need unrelated database documentation unless the task touches data behavior.

## 8. Codex CLI `/compact`

Codex CLI provides the interactive `/compact` command to summarize the visible chat and free context space. Current Codex behavior may ask for confirmation. Check the command picker in your installed version.

Use it when a long conversation is continuing on the same task and earlier discussion no longer needs to remain in full detail. It is not necessary after every turn, and it should not replace durable project documentation.

Before compacting a complex task, make sure the current objective, important constraints, completed work, remaining steps, and significant verification results are clear in the conversation or recorded in project notes.

### What compaction can and cannot do

- It can reduce the visible conversation context while retaining key points.
- It does not guarantee that every detail from the original discussion remains available in full.
- It does not restore prior file contents or undo project changes.
- The resulting summary should be checked before relying on it for consequential work.

## 9. Codex CLI Compaction and API Compaction Are Different Interfaces

OpenAI also documents compaction for the **Responses API**. API compaction can be server-side or performed through the standalone `/responses/compact` endpoint. Its returned compaction state is designed for programmatic continuation and is opaque rather than a normal human-written summary.

That API workflow is distinct from typing `/compact` in the Codex CLI. This lesson focuses on CLI use; the API reference is included for readers who later study agent development.

## 10. Conversation Context vs Persistent Project Knowledge

### Conversation context

Useful for the active task: recent instructions, temporary findings, tool results, and discussion. Its availability and detail can change as a session grows or is compacted.

### Persistent project knowledge

Stored in files such as `AGENTS.md`, architecture documents, feature requirements, coding standards, and testing guides. These files can be reviewed and reused across sessions.

If a rule should apply to future work—such as a project architecture constraint—document it in the appropriate project file instead of relying only on a past conversation.

## 11. Resuming or Starting a Session

Codex CLI can resume saved sessions, for example:

```powershell
codex resume
codex resume --last
```

Resuming continues a saved interaction; it does not prove that every old detail is still active or that project files have not changed. Git is the tool for tracking source changes and versions.

After resuming a long task, a read-only orientation prompt can help reestablish the current state:

> Before continuing, inspect the current repository state and relevant project instructions. Summarize the objective, completed work, pending work, and important constraints. Do not modify files. Point out anything uncertain.

Continue a session when its context is still relevant to the same task. Start a fresh session when switching to unrelated work or when the existing discussion would add confusion.

## 12. Practical Context Management Strategies

1. **Keep project instructions focused.** Put durable rules in `AGENTS.md`; avoid unrelated detail.
2. **Organize documentation by topic.** For example, keep architecture, schema, feature requirements, and testing guidance in appropriate documents.
3. **Retrieve relevant files.** Start with the task area and expand the investigation only when needed.
4. **Prefer focused output.** Ask for relevant excerpts or summaries instead of repeatedly collecting full files and logs.
5. **Record decisions that affect future work.** Preserve approved architecture, scope, data contracts, and unresolved questions.
6. **Separate unrelated tasks.** Use distinct sessions when the prior context is no longer useful.
7. **Check state after compaction or resumption.** Confirm the current objective and repository status before significant work.
8. **Use Git for code history.** Session history and Git history serve different purposes.

## 13. Context Handoff Notes

A handoff note records what someone needs to continue work in a later session. It is most useful for multi-session tasks, not every small request.

```markdown
# Development Handoff

## Objective
What the task is trying to accomplish.

## Important Decisions and Constraints
Approved approach, boundaries, and open questions.

## Completed Work
Only items that have been verified.

## Relevant Files
Files that matter to the next step.

## Remaining Work
The next actions needed.

## Verification
Checks performed and their results; note anything not checked.
```

Keep the handoff concise and distinguish verified facts from assumptions.

## 14. Example — GroupTrack Android Project

Suppose GroupTrack is an Android application using Java, XML layouts, and Firebase. The task is to adjust the login screen's spacing without changing authentication behavior.

**Analysis prompt:**

> Inspect the login screen layout and relevant UI guidance. Identify spacing or alignment issues. Do not modify files or inspect unrelated modules unless needed to explain a finding.

**Implementation prompt, after the approach is agreed:**

> Apply the approved layout adjustments. Preserve existing IDs, event bindings, authentication logic, and navigation. Do not add dependencies or change unrelated screens. Summarize the files changed and the checks performed.

If work continues later, a short handoff can preserve the objective, constraints, changed files, checks, and pending visual review. The conversation does not need to retain every intermediate observation.

## 15. Common Context Management Mistakes

| Mistake | Better approach |
|---|---|
| Assuming the model remembers every detail indefinitely | Preserve important decisions in project documentation |
| Including or inspecting the entire repository without need | Retrieve context relevant to the task |
| Repeating the same guidance in multiple places | Maintain a clear source of truth |
| Relying on compaction as an exact transcript | Save critical requirements and decisions durably |
| Combining unrelated work in one long session | Start a new session when task context changes |
| Treating resume as restoration of source files | Use Git to inspect and restore tracked project versions |
| Continuing after compaction without checking the summary | Reconfirm requirements, constraints, and current project state |

## 16. Practice Exercises

These are for a future learning session. No Codex commands need to be run now.

### Exercise A — Identify useful context

Imagine you are investigating a login screen layout issue. List the project files and instructions likely to be relevant, then list one unrelated area you would avoid inspecting unless evidence points to it.

### Exercise B — Draft a handoff

Write a short handoff for a feature that spans two sessions. Include its objective, constraints, verified progress, relevant files, remaining work, and verification status.

### Exercise C — Review a compaction summary

Imagine a long session was compacted. Check whether its summary preserves the current objective, important constraints, decisions, completed work, and pending checks. Identify what must be clarified before continuing.

## 17. Knowledge Check

1. What is a context window?
2. What is a token, and why is it not the same as a word?
3. What kinds of information can contribute to context during agent work?
4. Why can a long session become harder to manage?
5. What does Codex CLI `/compact` do?
6. How is CLI `/compact` different from Responses API compaction?
7. Why should durable project decisions live in documentation?
8. What does session resumption not guarantee?
9. When is a handoff note useful?
10. Why should requirements and project state be rechecked after resuming or compacting?

## 18. Key Takeaways

1. A context window has a model-specific token limit; it is not unlimited memory.
2. Instructions, relevant files, conversation, and tool results may all contribute to context.
3. Focused context is more useful than accumulating every available detail.
4. Codex CLI `/compact` summarizes a chat to free context; API compaction is a separate programmatic workflow.
5. Persistent documentation preserves durable project knowledge across sessions.
6. Session history helps continue conversations, while Git tracks source changes.
7. Handoffs should preserve verified decisions and next steps without unnecessary transcript detail.

## Official References

- [Codex CLI guide](https://developers.openai.com/codex/cli/)
- [Codex CLI commands and slash commands](https://learn.chatgpt.com/docs/developer-commands)
- [OpenAI API — Conversation state and context windows](https://developers.openai.com/api/docs/guides/conversation-state)
- [OpenAI API — Compaction](https://developers.openai.com/api/docs/guides/compaction)
- [OpenAI Cookbook — Iterating Development Workflows with Codex](https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex)

## Next Lesson

**[[03 - Working with AGENTS.md]]**

---

*Phase 2 — Context Engineering | Lesson 2.2*  
*Learning Principle: Preserve essential knowledge, retrieve relevant information, and minimize unnecessary context.*
