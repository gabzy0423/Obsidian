# 02 — Codex Architecture and Agentic Coding Concepts

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 1 — Codex Fundamentals  
> **Lesson:** 1.2 — Codex Architecture and Agentic Coding Concepts  
> **Difficulty:** Beginner–Intermediate  
> **Status:** In Progress  
> **Prerequisite:** [[01 - Introduction to Codex]]

> **Study note:** This is a conceptual model of coding agents, not a full specification of Codex’s internal implementation. We can study the example without running commands or changing code.

## 1. Why Learn How an Agent Works?

Knowing Codex’s features and commands is only part of understanding it. It also helps to know how an agent uses instructions, project information, and tools to work through a task.

For example, if asked to investigate a login problem, an agent might search for authentication-related files, read the relevant code, and explain what it finds. If the task allows changes or commands, it may also edit files or run tools. What it can do depends on its environment and permissions.

The main ideas in this lesson are **model, context, tools, environment, execution loop, security controls, and human oversight**. OpenAI describes the Codex harness as the surrounding system that helps a model gather context, use tools, follow configured boundaries, handle approvals, and carry work across turns. ([OpenAI: Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform))

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Describe the conceptual parts of a coding agent.
2. Explain how a model, context, and tools work together.
3. Describe a simple agent execution loop.
4. Distinguish deciding on an action from carrying it out.
5. Explain the role of the development environment and working directory.
6. Describe how permissions, sandboxing, and approvals limit actions.
7. Explain why human review matters.
8. Compare agentic workflows with traditional automation.

## 3. A Simple Architecture

A useful way to picture an AI coding agent is as a set of parts working together:

| Part | What it does |
|---|---|
| **Developer** | Sets the goal, requirements, and limits |
| **AI model** | Interprets information and decides what response or action may help |
| **Context** | Gives the agent instructions, project information, and results |
| **Tools** | Let the agent search, read, edit, or run commands when available |
| **Environment** | Provides the files and development utilities the agent can access |
| **Agent loop** | Coordinates decisions, tool calls, results, and follow-up steps |
| **Security controls** | Limit actions or ask for approval |
| **Human oversight** | Checks the work and makes final decisions |

This is a teaching model. The exact components and how they are arranged vary by Codex interface and task. OpenAI’s Agents API architecture also distinguishes the **harness** that manages the model and tool loop from the **environment** where files and commands are available. ([OpenAI Docs: Agents API architecture](https://developers.openai.com/api/docs/guides/agents-api/architecture))

### Example: Read-only investigation

Instruction:

> “Analyze the authentication module and explain why login validation might be failing. Do not modify any files.”

A possible sequence is:

1. Codex interprets this as an investigation, not an implementation request.
2. It uses the available project context to look for relevant files.
3. It reads authentication code or documentation if permitted.
4. It analyzes what it finds and may inspect another relevant file.
5. It explains its findings to the developer.

The read-only constraint sets a clear boundary: the task does not authorize file changes.

## 4. The AI Model: Decide and Explain

The **AI model** interprets the request, considers the information available to it, and generates responses or decisions about possible next steps. It may help determine which files seem relevant, what a tool result means, or whether more information is needed.

### Reasoning and execution are different

- **Deciding:** The agent concludes that it should search for a registration validator.
- **Executing:** The agent uses an available search tool to locate one.
- **Observing:** The tool returns file names or matching text.
- **Continuing:** The model interprets those results and decides what to do next.

The model does not automatically know every file in a project. It needs relevant information in the conversation or information retrieved through available tools.

## 5. Context: Information for the Task

**Context** is the information available to the model while it works. Useful context helps it understand the project, the requested outcome, and the limits of the task.

Common sources include:

- **The request:** Goal, requirements, and constraints
- **Project files:** Relevant source code and configuration
- **Documentation:** Architecture notes, feature specifications, and development instructions
- **`AGENTS.md`:** Project or directory instructions for Codex
- **Tool results:** Search output, command results, or test output
- **Conversation history:** Earlier details that remain relevant to the task

### Clearer context makes the task easier to understand

**Vague:** “Create a login feature.”

**More specific:** “Explain how this Android Java project currently handles login. Follow the project’s existing terminology and do not change any files.”

The second request gives the agent a specific goal and a boundary. Good context reduces guesswork, though it cannot guarantee a correct answer.

### Context window

A model can only process a limited amount of information at one time. In a large project, it may need to retrieve relevant files as needed instead of considering every file at once. Context management becomes more important for large repositories and long tasks.

**Related topic:** [[Phase 2 - Context Engineering]]

## 6. Tools: Ways to Act

Tools give an agent ways to interact with its environment. Depending on what is available and permitted, Codex may be able to:

| Capability | Example |
|---|---|
| Read files | Inspect an existing Java class |
| Search files | Find references to a function |
| Edit files | Change a controller or configuration |
| Run commands | Start a build or other development command |
| Run tests | Execute the project’s test suite |
| Inspect Git | Review a diff or repository status |

A tool call is a request to use one of these capabilities. It may fail because a file is missing, a command is invalid, dependencies are unavailable, or access is restricted. The agent should report results and errors rather than assume an attempted action succeeded.

## 7. The Agent Execution Loop

An **execution loop** is a simple way to describe how an agent can work through a multi-step task:

1. **Receive a goal** — Read the user’s request and constraints.
2. **Review context** — Consider instructions and relevant project information.
3. **Choose a step** — Decide whether to answer or use a tool.
4. **Use a permitted tool** — Search, read, edit, or run a command if authorized.
5. **Observe the result** — Receive the tool’s output or error.
6. **Reassess** — Decide whether the result answers the question or another step is needed.
7. **Report** — Summarize findings, actions, and limits for the user.

A debugging task might involve reading an error, finding related files, running an allowed check, and then summarizing the likely cause. The exact sequence depends on the request and the information found. The user can steer the agent as it works.

This loop explains why agentic work can involve multiple actions rather than one generated snippet. It also means the final report should be checked against the actual tool results.

## 8. Development Environment and Working Directory

The **environment** is where an agent can access files or run commands. For Codex CLI, this commonly means the project directory and the tools available through the terminal. An environment may include:

- Project folders and source files
- Configuration and documentation
- Installed dependencies and language runtimes
- Build and test tools
- A Git repository

The **working directory** is the current folder used by a command. It affects which files relative paths refer to and which project Codex starts from. Before a real task, confirm that Codex is in the intended project directory.

Example project layout:

```text
GroupTrack/
├── app/
│   └── src/main/
│       ├── java/
│       ├── res/
│       └── AndroidManifest.xml
├── docs/
├── AGENTS.md
├── build.gradle
└── settings.gradle
```

This is only an illustration. We do not need to open or change a project for this lesson.

## 9. Permissions, Approvals, and Sandboxing

An agent’s tools are subject to configured permissions. Some actions may be allowed, some may require approval, and others may be blocked. A **sandbox** is a restricted environment that limits which files or system resources the agent can access.

These controls help keep actions within the intended boundaries. Exact behavior depends on the Codex interface and configuration. OpenAI’s Codex documentation explains the available security and permission controls. ([OpenAI Docs: Codex security](https://developers.openai.com/codex/security/))

Good habits include:

- Use only the access a task needs.
- Read an approval request before accepting it.
- State when a task must be read-only.
- Keep important work under version control.
- Review changes before relying on them.
- Avoid putting secrets or credentials into prompts or project files.

**Related topic:** [[04 - Tools and Permissions]]

## 10. Human Oversight

The developer remains responsible for deciding whether the result is correct and appropriate. Codex can assist with a task, but the person sets its purpose and evaluates its output.

A human-reviewed workflow might look like this:

1. The developer describes a task and its constraints.
2. Codex explains a proposed plan.
3. The developer reviews the plan.
4. Codex carries out an authorized task.
5. Codex reports results and checks performed.
6. The developer reviews the result and decides what to accept.

Human review is especially useful when a change affects security, important data, or unfamiliar parts of a project.

## 11. Agentic Coding and Traditional Automation

**Traditional automation** follows predefined steps. For example, a build command runs the build script configured by the project.

**Agentic coding** starts with a goal. An agent can inspect available information, choose among its tools, react to results, and decide whether another step is needed.

| Aspect | Traditional automation | Agentic coding |
|---|---|---|
| Instructions | Predefined steps or rules | A goal with requirements and constraints |
| Decisions | Mostly fixed in advance | Model-guided based on context and results |
| Behavior | Predictable for the same inputs | Can adapt, but needs careful review |
| Best suited to | Repeatable, well-defined operations | Investigation or tasks needing interpretation |
| Oversight | Depends on the process | Important when results or changes matter |

They can also be used together: an agent may call a standard build or test script as one step in a larger task.

## 12. Worked Example: Exploring an Android Project

**Scenario:** A developer wants to understand an Android app called GroupTrack before making a future change.

**Read-only request:**

> “Explain how the authentication area of this project is organized. Identify relevant files and describe their responsibilities. Do not create, modify, or delete files.”

A conceptual workflow:

1. Codex identifies this as an explanation task.
2. It checks the project instructions and documentation available to it.
3. It searches for authentication-related files.
4. It reads relevant code and notes relationships between components.
5. It summarizes the structure and points to the files it used.
6. The developer checks the explanation against those files.

This example shows how an agent can use context and tools for a read-only task. The person still verifies the explanation.

## 13. Common Misconceptions

- **“Codex already knows my whole project.”** It uses context available to it and may need to retrieve relevant project information.
- **“Codex always gets the code right.”** It can make mistakes or misunderstand requirements.
- **“Every attempted tool call succeeds.”** Commands and tools can fail; results need to be read.
- **“More access is always better.”** Extra permissions can add risk without helping the task.
- **“Passing tests proves everything works.”** Tests cover specific cases and may miss other problems.
- **“Developers no longer need to understand code.”** Technical understanding helps people review results and make sound decisions.

## 14. Key Terms

| Term | Plain-language definition |
|---|---|
| **AI model** | The system that interprets information and generates responses or action decisions |
| **Agent** | A system that uses a model and tools to work toward a delegated goal |
| **Harness** | The surrounding system that manages context, tool use, progress, and the agent’s work |
| **Context** | Information available to the agent for a task |
| **Context window** | The amount of information a model can process at one time |
| **Tool call** | A request from the agent to use an available capability |
| **Tool result** | The output returned by a tool |
| **Execution loop** | Repeated decisions, actions, observations, and reassessment |
| **Working directory** | The current folder used to resolve paths and run commands |
| **Sandbox** | A restricted environment for limiting access or actions |
| **Approval** | User authorization required for a particular action |
| **Human-in-the-loop** | A workflow with human review or decisions during the process |

## 15. Key Takeaways

1. A coding agent combines a model with context, tools, and an environment.
2. Context helps the model understand the task and project.
3. Tools let the agent act, subject to permissions and availability.
4. The execution loop lets an agent respond to results and continue multi-step work.
5. Sandboxing and approvals help control what the agent can do.
6. Human review remains important because agents can be wrong or miss context.
7. Agentic coding can complement traditional automation.

## 16. Knowledge Check

Answer in your own words:

1. What are the main parts of a coding agent’s conceptual architecture?
2. How are a model and an agent different?
3. Why does an agent need context?
4. What is a tool call, and what happens after one?
5. Describe the execution loop.
6. Why does the working directory matter in CLI use?
7. What do permissions and sandboxing control?
8. How is agentic coding different from a fixed automation script?
9. Why should a person review an agent’s result even if a test passed?
10. How can project documentation help an agent?

## 17. Optional Read-Only Practice — Later

When we are ready to use Codex CLI, we can observe a read-only analysis in a safe practice project. This is optional; there is no need to install or run anything now.

Example request:

> “Describe this project’s main folders, languages, configuration files, and likely entry points. Explain which files you inspected and why. Do not create, modify, or delete files.”

Afterward, consider:

- Which files did Codex inspect, and why?
- What did the tool results tell it?
- Did it need more than one step?
- Did it respect the read-only instruction?
- How could you check whether its explanation is accurate?

## 18. Official References

- [OpenAI: Codex as a platform](https://developers.openai.com/blog/codex-as-a-platform)
- [OpenAI Docs: Agents API architecture](https://developers.openai.com/api/docs/guides/agents-api/architecture)
- [OpenAI Docs: Codex CLI](https://developers.openai.com/codex/cli/)
- [OpenAI Docs: Codex security](https://developers.openai.com/codex/security/)
- [OpenAI Docs: `AGENTS.md`](https://developers.openai.com/codex/guides/agents-md/)
- [OpenAI Codex GitHub repository](https://github.com/openai/codex)

## Next Lesson

**[[03 - Codex CLI Installation and Environment Setup]]**

---

*Phase 1 — Codex Fundamentals | Lesson 1.2*  
*Learning principle: Understand how the agent works before delegating complex tasks.*
