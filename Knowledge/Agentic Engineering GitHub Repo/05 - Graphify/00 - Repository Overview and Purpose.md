# 00 — Graphify: Repository Overview and Purpose

> [!abstract] Lesson Information
> **Repository:** [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)
>
> **Category:** Agentic Engineering / Code Intelligence / Knowledge Graphs / Context Engineering
>
> **Difficulty:** Beginner to Intermediate
>
> **Learning Approach:** Theory First
>
> **Platform Focus:** Graphify CLI and Claude Code; Codex, Windows PowerShell, VS Code, Android Studio, Laravel, and Next.js as evaluation contexts
>
> **Lesson Objective:** Understand Graphify’s knowledge-graph approach, capabilities, limitations, and possible value for AI-assisted software development.
>
> **Prerequisites:** Basic programming, Git, repository navigation, and introductory familiarity with coding agents.

---

# Part I — Introduction

## 1. What Is Graphify?

**Graphify** is an open-source code and knowledge-intelligence tool from Graphify Labs. Its Python library analyzes a corpus and builds a queryable graph of concepts and relationships.

The current README presents Graphify as a **Claude Code skill** backed by a Python package. It can process source code, Markdown and text documents, PDFs, screenshots, diagrams, and images. Different input types use different extraction approaches.

Its central idea is:

> Represent useful connections among repository concepts so developers and coding agents can navigate relevant context more directly.

Graphify is a navigation and analysis aid. It does not prove that a code change is correct.

### Simple Example

~~~text
project/
├── controllers/UserController.php
├── services/UserService.php
├── models/User.php
└── routes/api.php
~~~

~~~mermaid
flowchart TD
    A["api.php"] -->|references| B["UserController"]
    B -->|uses| C["UserService"]
    C -->|uses| D["User"]
~~~

This diagram is illustrative. Actual edges depend on source structure, supported parsers, and resolution passes.

---

# Part II — The Codebase Understanding Problem

## 2. Why Relationships Are Hard to Discover

A feature may span routes, controllers, services, models, schemas, frontend components, configuration, and tests. An agent must locate the relevant pieces and understand how they connect.

For example, fixing login validation may require inspecting authentication routes, a handler, validation logic, a service, and related tests. Traditional search can work well, but repeated discovery can consume time and context.

~~~mermaid
flowchart TD
    A["Task"] --> B["Search names and symbols"]
    B --> C["Read source files"]
    C --> D["Find references"]
    D --> E["Trace relationships"]
    E --> F["Understand impact"]
~~~

A graph can narrow the investigation:

~~~mermaid
flowchart TD
    A["Task"] --> B["Query graph"]
    B --> C["Identify related concepts"]
    C --> D["Trace candidate relationships"]
    D --> E["Inspect current source and tests"]
    E --> F["Implement and verify"]
~~~

Always verify graph-derived context against current code and tests.

---

# Part III — Knowledge Graph Fundamentals

## 3. Nodes and Edges

A knowledge graph represents entities and relationships:

\[
G=(V,E)
\]

Here, \(V\) is the set of nodes and \(E\) is the set of edges. Nodes can represent functions, classes, files, documents, papers, or concepts. Edges describe calls, imports, uses, references, or inferred conceptual relationships.

~~~mermaid
flowchart LR
    A["UserController"] -->|calls| B["createUser"]
    B -->|uses| C["UserService"]
    C -->|references| D["User"]
~~~

Text search can answer “where is this class defined?” A graph may help answer “what depends on this class?” or “what connects these modules?”

---

# Part IV — Parsing and Extraction

## 4. AST-Based Code Analysis

Graphify’s README describes tree-sitter AST extraction for supported languages, followed by graph construction and relationship analysis.

An **Abstract Syntax Tree (AST)** represents source-code syntax. A parser can identify classes, methods, imports, and calls more accurately than searching for matching words in source, comments, and strings.

The current README lists languages including Python, TypeScript, JavaScript, Go, Rust, Java, C, C++, Ruby, C#, Kotlin, Scala, and PHP. Check the installed release for the exact list.

### Different Inputs, Different Extraction

| Input | Examples | Documented extraction |
|---|---|---|
| Code | .py, .ts, .js, .go, .rs, .java, .php and others | AST via tree-sitter and call-graph pass |
| Documents | .md, .txt, .rst | Concepts and relationships via Claude |
| Papers | .pdf | Citation mining and concept extraction |
| Images | .png, .jpg, .webp, .gif | Claude vision |

Static code parsing and model-assisted document/image interpretation are separate paths.

## 4.1 Relationship Confidence

The current architecture describes three edge labels:

| Label | Meaning |
|---|---|
| EXTRACTED | Explicit in source, such as an import or direct call |
| INFERRED | Deduced, for example through a call-graph pass or contextual co-occurrence |
| AMBIGUOUS | Uncertain and flagged for human review |

An inferred edge is useful evidence, not certainty. Verify important or ambiguous relationships against source.

---

# Part V — Graphify Outputs and Queries

## 5. Output Artifacts

The current README lists artifacts under graphify-out:

~~~text
graphify-out/
├── graph.html
├── obsidian/
├── wiki/
├── GRAPH_REPORT.md
├── graph.json
└── cache/
~~~

Exact outputs depend on options and installed version.

| Artifact | Purpose |
|---|---|
| graph.html | Interactive visualization with search and community filters |
| obsidian/ | Graph content arranged for an Obsidian vault |
| wiki/ | Optional agent-navigable Markdown articles |
| GRAPH_REPORT.md | High-degree concepts, surprising connections, suggested questions |
| graph.json | Persistent structured graph |
| cache/ | SHA256 cache so unchanged files need not be processed again |

Graphify documents update and watch workflows. Code changes can trigger AST rebuilds in watch mode; document or image changes may require a model-backed re-pass. Confirm behavior for the installed version. A stale graph can mislead exploration, so refresh it when inputs change.

## 5.1 Query, Path, and Explain

The current README documents operations through the Graphify skill:

~~~text
/graphify .
/graphify ./raw --mode deep
/graphify ./raw --update
/graphify query "what connects authentication to the user model?"
/graphify path "UserController" "User"
/graphify explain "UserService"
~~~

It also lists options for watch, wiki, SVG, GraphML, Neo4j export, and an MCP stdio server. Check the installed release for exact syntax.

- **Query** retrieves graph context relevant to a question.
- **Path** explores a connection between named concepts.
- **Explain** inspects one concept and its relationships.
- **Deep mode** is described as more aggressive inferred-edge extraction.

A query identifies candidates; source inspection confirms behavior.

---

# Part VI — Communities and Central Nodes

## 6. Communities

A graph community is a group of nodes more strongly connected to one another than to other groups. In software, clusters may roughly correspond to authentication, data access, UI, networking, or reporting.

~~~mermaid
flowchart TD
    subgraph A["Authentication"]
        A1["LoginController"] --> A2["AuthService"] --> A3["UserRepository"]
    end
    subgraph B["Tasks"]
        B1["TaskController"] --> B2["TaskService"] --> B3["TaskRepository"]
    end
    A3 --> B3
~~~

Detected communities are structural groupings, not guaranteed business domains or intended module boundaries.

## 6.1 High-Degree Nodes

Graphify highlights high-degree concepts, sometimes called “god nodes.” These may be core services, models, or shared utilities. Centrality can help prioritize impact analysis and regression tests. A highly connected node is not automatically a design flaw.

---

# Part VII — Graphs and Other Search Methods

## 7. Choose the Right Tool

| Technique | Strength | Limitation |
|---|---|---|
| Filename search | Locate a known file | Little relationship information |
| Text search | Find exact strings | May match comments or unrelated text |
| AST analysis | Identify syntax and declarations | Runtime links may be unresolved |
| Knowledge graph | Traverse structured or inferred links | May omit edges or become stale |
| Vector search | Retrieve semantically similar text | Similarity does not prove dependency |
| LLM reasoning | Interpret intent and behavior | Can make unsupported assumptions |

Graph search and vector search answer different questions and can be combined. Do not use graph analysis for every trivial lookup.

---

# Part VIII — Architecture and Data Boundaries

## 8. Current Technology

The repository architecture describes a Python library and skill orchestration with stages for file detection, extraction, graph construction, clustering, analysis, reporting, and export.

The documented stack includes Python, tree-sitter, NetworkX, Leiden community detection via graspologic, Claude for selected document/media extraction, and vis.js for visualization. Standard usage does not require Neo4j or a separate graph server; optional MCP mode starts a local stdio server.

## 8.1 Local Processing and Privacy

Graphify’s SECURITY.md describes it as a local development tool and says graph analysis makes no network calls except explicit URL ingestion. It also says the tool does not execute source code or store API keys.

The README separately describes Claude-assisted document and image processing. Distinguish Graphify’s own network behavior from the model/provider path used to interpret content. Follow the current release documentation and project data policy.

Before processing a repository, ask:

1. Which files will be included?
2. Will documents or images be interpreted through a model?
3. Are URLs explicitly ingested?
4. Where are graph outputs stored?
5. Could reports reveal sensitive architecture?
6. Are output and cache folders handled appropriately by Git?

---

# Part IX — Codex Compatibility

## 9. What the Current Repository Documents

The current Graphify README describes a **Claude Code skill**, lists Claude Code and Python 3.10+ as requirements, and documents the /graphify invocation. The README reviewed on October 23, 2026 does not mention Codex integration.

This lesson therefore does **not** claim that Graphify provides an official Codex skill or supported $graphify command. Verify upstream docs for future releases before claiming Codex compatibility.

Possible ways to evaluate with Codex:

- Ask Codex to inspect generated graph artifacts directly.
- Point Codex at the generated wiki index if one was produced.
- Investigate documented MCP stdio mode only if the host supports it and Graphify documents compatible setup.

These are integration ideas to validate, not guaranteed out-of-the-box support. A Python CLI, graph artifact, MCP server, and agent skill are separate interfaces.

---

# Part X — Context Engineering and Benchmarks

## 10. Selecting Relevant Context

A graph may help find relevant concepts and files before loading source:

~~~text
Query graph
Inspect candidate nodes and links
Read current source and tests
Verify relationships
Implement
~~~

A hypothetical token-reduction formula is:

\[
\text{Reduction} = \frac{T_{baseline}-T_{graph-assisted}}{T_{baseline}}\times100
\]

Include graph-build time, update cost, query time, and stale-graph errors in a fair evaluation.

## 10.1 Repository-Reported Result

The current README reports **71.5x fewer tokens per query** on a selected 52-file mixed corpus of code, papers, and images. It also reports about 1x reduction for a six-file example, where structural clarity rather than compression was the benefit.

These are upstream results for selected corpora, not a guarantee for every project. See the [README and worked examples](https://github.com/Graphify-Labs/graphify#worked-examples).

---

# Part XI — Graphify and Caveman

## 11. Different Problems

| Area | Graphify | Caveman |
|---|---|---|
| Primary objective | Represent and navigate relationships | Reduce selected agent workflow waste |
| Main mechanism | Graph extraction and querying | Output/input optimization, routing, scripts, and other features |
| Dependency tracing | Central use case | Indirect |
| Context discovery | Central use case | Adjacent optimization |
| Agent integration | Current README documents Claude Code skill | Multiple integrations, version-dependent |

They may complement one another, but compatibility and performance should be tested rather than assumed.

---

# Part XII — Project Scenarios

## 12. GroupTrack: Android, Java, Firebase

Question: How does project selection on the Tasks page lead to the task list?

Graph queries may identify candidate activities, models, navigation classes, or data-access code. Verify the path in current Java/XML and test on an emulator when needed. A graph cannot replace UI preview, runtime testing, or Firebase security-rule review.

## 12.1 GoPasig: Laravel Fleet Management

Question: Which services and models participate in bus dispatch?

A graph may help trace candidate controllers, services, schedules, buses, drivers, and trips. Laravel conventions, dependency injection, and runtime behavior may not all appear as direct static edges. Verify transactions and authorization in code and tests.

## 12.2 Next.js and Laravel E-Commerce

Question: How does a frontend product form connect to backend creation?

A frontend HTTP call and Laravel route may not be linked automatically by AST parsing. Cross-language API contracts require manual tracing, documentation, or metadata. Verify both sides of the contract.

---

# Part XIII — Benefits, Limits, and Adoption

## 13. Potential Benefits

- Faster repository orientation.
- More targeted source inspection.
- Dependency and impact analysis.
- Architecture visualization.
- Reusable graph reports and wiki pages.
- Optional links among code, documents, papers, and images.

## 13.1 Limitations

Graphify cannot guarantee complete extraction, correct runtime behavior, a fresh graph after changes, or correct interpretation of inferred edges. Static analysis can miss reflection, dynamic dispatch, generated code, dependency injection, and framework runtime behavior. Indexing and maintenance also have costs.

Consider Graphify when relationship discovery is a recurring bottleneck. It may add little value for a tiny repository or a trivial text change.

## 13.2 Evaluation

Compare ordinary source search with graph-assisted exploration using equivalent tasks and repository snapshots. Measure time to locate files, files read, query usefulness, dependency accuracy, graph build/update time, token use where available, missed relationships, and maintenance overhead.

Success means relevant context is found without missing important dependencies, while task correctness remains the same or improves.

---

# Part XIV — Practical Exercises

## 14. Exercise 1 — Explain Graphify

Explain Graphify without using the phrase “knowledge graph.”

## 15. Exercise 2 — Nodes and Edges

For a TaskController calling TaskService and reading a Task model, identify possible nodes and relationships.

## 16. Exercise 3 — Search vs Graph

Give one question ordinary text search can answer and one relationship question that benefits from graph traversal.

## 17. Exercise 4 — Confidence Labels

Create examples of EXTRACTED, INFERRED, and AMBIGUOUS edges. State how each should be verified.

## 18. Exercise 5 — Graph Freshness

A developer refactors services after graph generation. Describe the risk of stale data and a safe refresh/verification approach.

## 19. Exercise 6 — Codex Compatibility

The README documents Claude Code but not Codex. What evidence is required before claiming official Codex support?

## 20. Exercise 7 — Adoption Decision

Choose a project and decide whether relationship discovery is costly enough to justify graph construction and maintenance.

---

# Part XV — Knowledge Check

## 21. Review Questions

1. What problem does Graphify address?
2. What are nodes and edges?
3. What is an AST?
4. What does tree-sitter contribute?
5. Which confidence labels does the current architecture describe?
6. Why is an inferred edge not certainty?
7. What output artifacts does the current README list?
8. What are query, path, and explain for?
9. What is a graph community?
10. How does graph retrieval differ from vector search?
11. Which inputs use Claude according to the README?
12. What does the security policy say about network calls?
13. Does the reviewed README document Codex integration?
14. Why does graph freshness matter?
15. What runtime relationships can static analysis miss?
16. When is Graphify useful?
17. What should an adoption benchmark measure?

---

# Part XVI — Key Takeaways

> [!summary]
> - Graphify extracts entities and relationships into a queryable graph.
> - tree-sitter supports AST-based extraction for listed code languages.
> - The current README also describes document, PDF, and image processing.
> - Edges can be EXTRACTED, INFERRED, or AMBIGUOUS.
> - HTML, JSON, reports, optional Obsidian/wiki exports, and cache support exploration.
> - Graphs help discover context but do not replace source review or tests.
> - Graph freshness and extraction coverage matter.
> - Code parsing and model-assisted document/media analysis are separate paths.
> - The reviewed README documents Claude Code, not official Codex support.
> - Graphify’s token-reduction figures are corpus-specific, not universal.
> - Graphify and Caveman address different, potentially complementary needs.
> - Adopt Graphify when relationship discovery is a measurable problem.

---

# Part XVII — Final Mental Model

~~~mermaid
flowchart TD
    A["Repository and supporting corpus"] --> B["Graphify extraction"]
    B --> C["AST and relationship analysis"]
    B --> D["Document and media analysis"]
    C --> E["Knowledge graph"]
    D --> E
    E --> F["Query, path, explain"]
    E --> G["Report, HTML, wiki, JSON"]
    F --> H["Candidate context"]
    G --> H
    H --> I["Inspect current source and tests"]
    I --> J["Implement and verify"]
~~~

> [!important] Final Principle
> **Graphify helps developers discover how code and concepts connect. Its value is better context discovery; source verification, privacy review, and engineering judgment remain necessary.**

---

# Part XVIII — Official References

## Graphify

- [Official Graphify Repository](https://github.com/Graphify-Labs/graphify)
- [Graphify README](https://github.com/Graphify-Labs/graphify/blob/main/README.md)
- [Graphify Architecture](https://github.com/Graphify-Labs/graphify/blob/main/ARCHITECTURE.md)
- [Graphify Benchmarks](https://github.com/Graphify-Labs/graphify/blob/main/BENCHMARKS.md)
- [Graphify Security Policy](https://github.com/Graphify-Labs/graphify/blob/main/SECURITY.md)

## Related Technologies

- [tree-sitter](https://tree-sitter.github.io/tree-sitter/)
- [NetworkX](https://networkx.org/documentation/stable/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Codex Documentation](https://developers.openai.com/codex/)
- [Agent Skills Specification](https://agentskills.io/)

> [!note] Documentation Scope
> This lesson reflects the Graphify README and repository files reviewed on October 23, 2026. Graphify-specific claims are version-sensitive; check upstream before installation.
>
> Diagrams and project scenarios are educational examples, not results from analysis of the user's projects. The 71.5x token figure is an upstream result for a selected mixed corpus and was not independently reproduced here.
>
> No installation, indexing, configuration change, or code execution was performed.

---

# Part XIX — Next Lesson

**[[01 - Knowledge Graph Fundamentals]]**

Topics:

1. Graph theory foundations.
2. Nodes and edges.
3. Directed, undirected, weighted, and labeled graphs.
4. Graph traversal and paths.
5. Degree and centrality.
6. Connected components and communities.
7. Graph schemas and code dependencies.
8. Knowledge graphs vs ASTs.
9. Knowledge graphs vs vector databases.
10. Practical examples, exercises, and references.

**End of Lesson 00 — Repository Overview and Purpose.**
