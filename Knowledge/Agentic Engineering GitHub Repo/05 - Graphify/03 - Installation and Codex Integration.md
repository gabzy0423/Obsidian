# 03 — Graphify: Installation and Codex Integration

> [!abstract] Lesson Information
> **Repository:** [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)
>
> **Category:** Agentic Engineering / Developer Tooling / Codex CLI / Knowledge Graphs
>
> **Difficulty:** Intermediate
>
> **Learning Approach:** Theory First
>
> **Platform:** Windows PowerShell, Python, Git, Codex CLI
>
> **Prerequisites:** [[00 - Repository Overview and Purpose]], [[01 - Knowledge Graph Fundamentals]], [[02 - Architecture and Core Components]]
>
> **Primary Goal:** Understand the documented Graphify installation, generated artifacts, Windows setup, and the current limits of Codex integration.

---

# Part I — Learning Objectives

After completing this lesson, you should be able to:

1. Identify Graphify installation prerequisites.
2. Distinguish the Python package, CLI executable, graph data, and agent skill.
3. Understand Python environment isolation.
4. Install and verify the documented Graphify package.
5. Explain what Graphify's installer currently targets.
6. Understand graph generation and outputs.
7. Recognize documented query operations.
8. Distinguish an official Codex integration from a manual interoperability approach.
9. Understand Windows PowerShell considerations.
10. Apply security, upgrade, and rollback practices.

---

# Part II — What Is Being Installed?

## 1. Separate Components

| Component | Responsibility |
|---|---|
| Python distribution | Installs Graphify's Python implementation |
| CLI executable | Exposes Graphify commands |
| Graph artifacts | Store analyzed repository relationships |
| Claude Code skill | Guides Claude Code through Graphify workflows |
| MCP server mode | Optionally exposes graph functions over MCP stdio |
| Codex | A separate coding agent; current Graphify README does not document an official Codex skill installer |

The current Graphify README documents the Python package as **graphifyy**, while the CLI and skill command are named **graphify**. It documents `graphify install` for Claude Code and the `/graphify` invocation. Installing the package does not by itself mean that Codex has discovered or configured a Graphify skill.

~~~mermaid
flowchart TD
    A["Python package: graphifyy"] --> B["CLI: graphify"]
    B --> C["Graph generation and query operations"]
    C --> D["graphify-out artifacts"]
    B --> E["graphify install"]
    E --> F["Claude Code skill"]
    G["Codex"] -. "No official setup documented in current README" .-> D
~~~

---

# Part III — Prerequisites

## 2. Windows Environment

Graphify's current README lists Python 3.10+ and Claude Code for the documented skill workflow. Python is required to install and run the package. The Graphify README reviewed for this lesson does not list Codex as an installation prerequisite or supported skill target.

Check available tools in PowerShell:

~~~powershell
python --version
py --version
py -0p
git --version
codex --version
~~~

`python -m pip` helps target the package installer associated with the selected interpreter:

~~~powershell
python -m pip --version
~~~

## 2.1 Multiple Python Installations

Windows can have several Python interpreters, including a system interpreter, a virtual environment, and a `pipx` environment. A package installed for one interpreter may not be available to another. Check `Get-Command python`, `Get-Command graphify`, and `py -0p` when diagnosing path issues.

---

# Part IV — Package and CLI Names

## 3. Distribution Name vs Executable

The current README explains that the package is temporarily named `graphifyy` while the `graphify` package name is being reclaimed. The command and skill remain `graphify`.

~~~text
Python distribution: graphifyy
CLI executable:       graphify
Claude Code command:  /graphify
~~~

These names represent different parts of the installation and should not be mixed up.

---

# Part V — Installation Methods

## 4. Documented Python Installation

The Graphify README documents:

~~~powershell
python -m pip install graphifyy
graphify install
~~~

The second command installs/configures the documented Claude Code skill. Review the current README before running it, especially if you only want the Python CLI or do not use Claude Code.

## 4.1 Isolated CLI Installation with pipx

The README also recommends `pipx install graphifyy` in certain environments and says pipx can handle PATH exposure on Windows:

~~~powershell
pipx install graphifyy
~~~

Use one package-management approach intentionally. Avoid installing the same CLI into several environments and then guessing which executable PowerShell selected.

## 4.2 Python Virtual Environment

A virtual environment isolates Python dependencies for a project:

~~~powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install graphifyy
~~~

If PowerShell blocks activation, do not weaken a system-wide execution policy just for convenience. Use an approved environment-specific approach or invoke the environment interpreter directly:

~~~powershell
.\.venv\Scripts\python.exe -m pip install graphifyy
~~~

A virtual environment isolates Python packages; it is **not** an operating-system security sandbox.

---

# Part VI — Verify the Installation

## 5. Package and Executable Checks

~~~powershell
python -m pip show graphifyy
Get-Command graphify
graphify --help
~~~

If Graphify is installed through pipx, use the corresponding pipx inspection command as documented by pipx.

Check multiple layers:

1. The expected Python environment reports the package.
2. PowerShell resolves the intended `graphify` executable.
3. The CLI help output matches the installed version.
4. A non-sensitive sample corpus can be processed before indexing an important project.

An installed Python package does not prove the Claude Code skill is installed, and neither proves Codex integration.

---

# Part VII — Generate and Inspect Graph Artifacts

## 6. Documented Workflow

The Graphify README describes invoking its skill in Claude Code:

~~~text
/graphify .
/graphify ./raw --update
/graphify ./raw --mode deep
~~~

The current README also lists operations such as query, path, explain, watch, wiki, SVG/GraphML export, Neo4j export, and MCP mode. Check the installed version's instructions for exact command syntax. Do not assume examples written as skill invocations are standalone CLI subcommands.

## 6.1 Outputs

Depending on options, the documented output directory can contain:

~~~text
graphify-out/
├── graph.html
├── obsidian/
├── wiki/
├── GRAPH_REPORT.md
├── graph.json
└── cache/
~~~

Inspect the generated files before sharing or committing them. They can expose internal names, file paths, and architecture. Add the output directory to `.gitignore` only if that matches the team's policy and the graph is not intended to be shared.

---

# Part VIII — Codex Integration: Current Status

## 7. What Is Officially Documented?

The Graphify README reviewed on October 23, 2026 describes Graphify as a **Claude Code skill**, lists Claude Code as a requirement for that workflow, and shows `/graphify`. The README does not document a Codex skill path, `$graphify` command, or Codex installer target.

Therefore, do not present the following as verified Graphify instructions:

- Copying Graphify files into a Codex skill directory.
- Invoking `$graphify` in Codex.
- Assuming `graphify install` registers a Codex skill.
- Assuming the Codex CLI can automatically call Graphify's skill.

Check the live [Graphify README](https://github.com/Graphify-Labs/graphify/blob/main/README.md) and release notes for support added after this review.

## 7.1 Manual Interoperability Ideas

Even without an official Codex skill installer, developers can evaluate generated graph artifacts as ordinary project files:

1. Generate the graph using a supported documented workflow.
2. Ask Codex to inspect `GRAPH_REPORT.md`, `graph.json`, or the generated wiki index.
3. Verify graph claims in current source code and tests.

The README also documents an MCP stdio mode. Codex and MCP may be configurable independently, but Graphify's README does not provide a Codex-specific setup recipe. Treat manual MCP configuration as an experiment: confirm the host's current configuration format, use a sample graph, and verify tool behavior before relying on it.

These approaches are interoperability options, not proof of official out-of-the-box Graphify/Codex support.

---

# Part IX — Skill vs MCP vs CLI

## 8. Comparison

| Interface | Main role | What it does not imply |
|---|---|---|
| CLI | Run Graphify operations | Does not automatically register an agent skill |
| Claude Code skill | Provides workflow instructions to Claude Code | Does not establish Codex support |
| MCP server | Exposes tools over MCP stdio | Does not configure Codex automatically |
| Generated graph | Persisted repository analysis | Does not guarantee freshness or correctness |

Use the smallest integration that meets the need. MCP is optional; do not add it merely because it exists.

---

# Part X — Windows PowerShell Troubleshooting

## 9. Common Issues

| Problem | Possible cause | First check |
|---|---|---|
| `python` not found | Runtime/PATH issue | `py -0p` and `Get-Command python` |
| Package missing after install | Different interpreter/environment | `python -m pip show graphifyy` |
| `graphify` not found | Scripts folder absent from PATH | `Get-Command graphify` and package environment |
| Skill not available | Claude Code skill setup incomplete | Inspect documented skill location and installer result |
| Empty or incomplete graph | Unsupported files, wrong scope, or extraction issue | Review report and supported extensions |
| Query returns no useful result | Missing/stale graph or node mismatch | Inspect graph outputs and refresh inputs |
| MCP connection fails | Client/server configuration mismatch | Test server and client separately |

The README gives a Windows PATH hint for the Python Scripts directory and recommends pipx as an option. Confirm the actual Python version and Scripts path rather than copying a guessed directory.

Troubleshoot in order: Python, package environment, executable, graph generation, query, then optional agent integration. Fix the earliest failing layer first.

---

# Part XI — Security, Updates, and Rollback

## 10. Installation Safety

Before installing a developer tool:

- Confirm the official repository and package name.
- Review install scripts and dependencies.
- Prefer an isolated Python environment.
- Avoid unnecessary administrator privileges.
- Protect API keys and credentials.
- Review what content is processed by a model.
- Treat generated graph reports as potentially sensitive.
- Keep a rollback path.

Graphify's security policy describes graph analysis as local and says explicit URL ingestion is the network path. The README describes Claude-assisted document and image analysis. Check current data-flow details for the exact version and workflow.

## 10.1 Version Management

Record Python, Graphify, and agent versions for reproducibility. Before upgrading, read release notes, review changed commands, and test graph generation and queries on a sample repository.

## 10.2 Removal

Removal depends on how Graphify was installed:

~~~powershell
python -m pip uninstall graphifyy
pipx uninstall graphifyy
~~~

Use only the matching uninstall command. Removing the package may not remove separately installed skill files, MCP configuration, or generated `graphify-out` artifacts. Inspect each location before deletion.

---

# Part XII — Reproducibility and Evaluation

## 11. Record the Setup

Document the operating system, Python version, Graphify package version, agent version, skill source/revision, MCP status, and graph output policy. Keep machine-level configuration separate from project-level documentation.

## 11.1 Measure Value

Compare ordinary repository search with graph-assisted exploration on equivalent tasks. Measure:

- Time to locate relevant files.
- Number of files read.
- Query usefulness and dependency accuracy.
- Graph construction and update time.
- Token use where available.
- Missed relationships and maintenance overhead.

Graphify adds value only when improved discovery outweighs indexing and maintenance costs without harming task correctness.

---

# Part XIII — Practical Exercises

## 12. Exercise 1 — Installation Components

Draw the relationship between Python, the `graphifyy` package, `graphify` CLI, graph artifacts, Claude Code skill, Codex, and optional MCP.

## 13. Exercise 2 — Package vs Executable

Explain why `graphifyy` and `graphify` are different names.

## 14. Exercise 3 — Environment Isolation

Describe the benefit and security limitation of a Python virtual environment.

## 15. Exercise 4 — Verification

List the checks that establish package installation, CLI availability, graph generation, and agent discovery as separate concerns.

## 16. Exercise 5 — Codex Support

The current README documents Claude Code. What upstream evidence would you need before claiming official Codex support?

## 17. Exercise 6 — MCP Decision

Explain when MCP may help and why it should not be configured automatically.

## 18. Exercise 7 — Windows Troubleshooting

The package is installed but PowerShell cannot find `graphify`. Give a safe diagnostic sequence.

## 19. Exercise 8 — Security

Create an installation checklist for a repository containing proprietary source code.

## 20. Exercise 9 — Project Evaluation

Design separate Graphify-assisted exploration tasks for GroupTrack, GoPasig, and the Next.js/Laravel application. Identify correctness checks for each.

---

# Part XIV — Review Questions

1. What does the Python distribution name identify?
2. What is the Graphify CLI executable called?
3. What Python version does the current README require?
4. What does `graphify install` currently target?
5. Why does package installation not prove skill discovery?
6. What is a virtual environment, and what is it not?
7. How do you verify the package and CLI separately?
8. What graph artifacts may be generated?
9. Why should generated artifacts be reviewed before sharing?
10. Does the reviewed README document a Codex skill installer?
11. What is the difference between a CLI, a skill, MCP, and a graph artifact?
12. What does the current README say about graph watch/update behavior?
13. How can Windows PATH issues be diagnosed?
14. Which network/data-flow details should be checked before processing sensitive sources?
15. What may remain after uninstalling the Python package?
16. What metrics help determine whether Graphify is worth maintaining?

---

# Part XV — Key Takeaways

> [!summary]
> - The current Python distribution is `graphifyy`; the CLI and skill command are named `graphify`.
> - The documented skill installer targets Claude Code.
> - The README reviewed for this lesson does not document official Codex skill integration or a `$graphify` invocation.
> - Graph generation, package installation, skill discovery, and MCP configuration are separate steps.
> - The README describes Python 3.10+ and documents pip or pipx installation approaches.
> - Generated graph artifacts may reveal internal repository structure.
> - Codex can be evaluated against generated files or manually configured MCP, but that is not verified out-of-the-box support.
> - Check exact commands and data-flow guidance against the installed version.
> - Use isolated environments, verify each layer, and preserve a rollback path.

---

# Part XVI — Final Mental Model

~~~mermaid
flowchart TD
    A["Windows + Python"] --> B["Install graphifyy"]
    B --> C["Verify graphify CLI"]
    C --> D["Generate and inspect graph artifacts"]
    C --> E["Install documented Claude Code skill"]
    F["Codex"] -. "No official setup in reviewed README" .-> D
    F -. "MCP setup requires separate verification" .-> G["Optional MCP stdio"]
    D --> H["Inspect source and tests"]
    H --> I["Implement and verify changes"]
~~~

> [!important] Final Principle
> **Installing Graphify's package is not the same as installing an agent skill or configuring MCP. The current official workflow targets Claude Code; Codex interoperability needs separate, version-specific verification.**

---

# Part XVII — Official References

## Graphify

- [Official Graphify Repository](https://github.com/Graphify-Labs/graphify)
- [Graphify README](https://github.com/Graphify-Labs/graphify/blob/main/README.md)
- [Graphify Architecture](https://github.com/Graphify-Labs/graphify/blob/main/ARCHITECTURE.md)
- [Graphify Security Policy](https://github.com/Graphify-Labs/graphify/blob/main/SECURITY.md)
- [Graphify Releases](https://github.com/Graphify-Labs/graphify/releases)

## Python and Codex

- [Python virtual environments](https://docs.python.org/3/library/venv.html)
- [pip documentation](https://pip.pypa.io/en/stable/)
- [pipx documentation](https://pipx.pypa.io/stable/)
- [Codex documentation](https://developers.openai.com/codex/)
- [Codex MCP documentation](https://developers.openai.com/codex/mcp/)

## MCP

- [Model Context Protocol](https://modelcontextprotocol.io/)

> [!note] Documentation Scope
> Graphify claims reflect the README, architecture, and security policy reviewed on October 23, 2026. CLI arguments, installation paths, and Codex capabilities may change; check the current release before running commands.
>
> No installation, indexing, configuration change, or code execution was performed.

---

# Part XVIII — Next Lesson

**[[04 - AST Parsing and Code Intelligence]]**

Topics:

1. Abstract Syntax Trees.
2. Tree-sitter architecture.
3. Parsing vs semantic analysis.
4. Syntax nodes and traversal.
5. Entity and relationship extraction.
6. Import and symbol resolution.
7. Extracted, inferred, and ambiguous edges.
8. Java, PHP, and TypeScript parsing.
9. Dynamic-language limits.
10. Incremental parsing, performance, exercises, and references.

**End of Lesson 03 — Installation and Codex Integration.**
