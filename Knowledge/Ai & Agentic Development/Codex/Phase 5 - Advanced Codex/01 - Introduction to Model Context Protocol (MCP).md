# 01 — Introduction to Model Context Protocol (MCP)

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 5 — Advanced Codex  
> **Lesson:** 5.1 — Introduction to Model Context Protocol (MCP)  
> **Difficulty:** Intermediate to Advanced  
> **Environment:** Windows, PowerShell, VS Code, Codex CLI  
> **Prerequisite:** [[06 - Configuration Troubleshooting]]  
> **Next Lesson:** [[02 - MCP Servers and External Tool Integrations]]  
> **Status:** Theory Documented

---

# 1. Introduction

Modern AI coding agents are no longer limited to generating source code or answering programming questions. They can interact with external systems to retrieve information, inspect resources, and execute authorized operations.

An agent may need to search technical documentation, retrieve repository information, read project-management data, access an approved development database, inspect issue records, or interact with development tools.

Each external service has its own API, authentication, data structures, and communication requirements. Without a common integration standard, developers would need separate integration logic for every AI application and service combination.

The **Model Context Protocol (MCP)** addresses this problem. MCP is an open protocol that standardizes communication between AI applications and external tools or data sources. It allows an application such as Codex to discover and use capabilities exposed by compatible MCP servers.

## Core Principle

**MCP standardizes how AI applications communicate with external capabilities; it does not replace the AI model, external API, or underlying service.**

---

# Part I — MCP Fundamentals

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Define Model Context Protocol.
2. Explain why MCP was developed.
3. Understand MCP's client-server architecture.
4. Differentiate MCP hosts, clients, and servers.
5. Identify each component's responsibilities.
6. Explain MCP tools, resources, and prompts.
7. Understand tool discovery and execution.
8. Explain MCP communication lifecycle.
9. Understand JSON-RPC communication.
10. Compare local and remote MCP servers.
11. Differentiate stdio and Streamable HTTP transports.
12. Understand MCP authorization and security risks.
13. Explain how Codex connects to MCP servers.
14. Configure a documented read-only MCP integration.
15. Identify software-development MCP use cases.
16. Recognize when MCP is unnecessary.
17. Apply MCP concepts to Android, Laravel, and Next.js projects.

---

## 3. What Is Model Context Protocol?

**Model Context Protocol (MCP)** is an open standard for connecting AI applications to external systems through a consistent interface. External systems may provide data, tools, reusable prompts, and other supported protocol capabilities.

### Simple Definition

MCP is a standardized communication layer between AI applications and external capabilities.

### Example

Codex needs to search official technical documentation. Without MCP, a developer might build custom integration code. With MCP, Codex can connect to a compatible documentation server, discover its tools, and use them through the protocol.

```text
Developer → Codex → MCP Connection → Documentation MCP Server
          → Documentation Search → Relevant Results → Codex Response
```

MCP is not a large language model, database, programming language, replacement for REST APIs, guarantee of service trustworthiness, or unlimited-permission mechanism.

---

## 4. Why MCP Matters

Without a standard, each AI application might need separate integrations for GitHub, documentation, databases, and project management.

```text
AI Application → MCP Client → Documentation Server
                          → Repository Server
                          → Project Management Server
                          → Database Server
```

Servers still implement service-specific integrations. MCP standardizes how capabilities are described, discovered, and invoked.

| Benefit | Explanation |
|---|---|
| Interoperability | Compatible clients can communicate with compatible servers |
| Reusability | A server can support multiple clients |
| Discoverability | Clients can discover available tools and resources |
| Standardization | Common methods reduce custom integration work |
| Extensibility | Additional servers can expose new capabilities |
| Separation of concerns | AI reasoning and service integration remain distinct |

Compatibility does not mean every host supports every optional feature. Clients and servers negotiate capabilities.

---

# Part II — MCP Architecture

## 5. The Three Main Components

Traditional MCP architecture has three roles: **MCP Host**, **MCP Client**, and **MCP Server**. Each has different responsibilities.

---

## 6. MCP Host

The **MCP host** is the AI application managing the user-facing experience and coordinating MCP connections. In this course, Codex is the host.

Responsibilities include receiving user requests, managing AI interaction, coordinating MCP connections, making capabilities available to the model, applying security and authorization controls, and presenting results.

A developer might ask Codex to search official configuration documentation; Codex coordinates that interaction and may use a connected documentation server.

**The host manages the overall AI application experience.**

---

## 7. MCP Client

The **MCP client** communicates with an MCP server. In the traditional architecture, the host creates and manages client connections.

It establishes communication, initializes protocol interactions, negotiates capabilities, discovers available tools and resources, sends requests, receives results and notifications, and handles connection errors.

The client is not necessarily a separate user-installed application; it may be an internal component of Codex.

**The MCP client handles protocol communication on behalf of the host.**

---

## 8. MCP Server

An **MCP server** exposes capabilities that compatible clients can discover and use, including tools, resources, and prompts.

The server advertises capabilities, describes operations, validates requests, performs authorized operations, returns results, and handles errors.

A documentation server might expose illustrative tools such as `search_documentation` and `read_documentation_page`.

**The MCP server provides capabilities; host and client coordinate their use.**

---

## 9. Complete MCP Architecture

```text
+---------------------------------------------+
| MCP HOST: Codex                             |
|  +-------------------------------+          |
|  | AI Model                      |          |
|  +-------------------------------+          |
|  +-------------+   +-------------+          |
|  | MCP Client A|   | MCP Client B|          |
+-------|-----------------|-------------------+
        | MCP Protocol    | MCP Protocol
        v                 v
+------------------+  +---------------------+
| MCP Server A     |  | MCP Server B        |
| Documentation   |  | Repository Service  |
| Search Tools    |  | Project Tools       |
+------------------+  +---------------------+
```

Codex is the host. Its clients communicate with configured servers. Each server exposes capabilities the model may use when appropriate. The host remains responsible for its permission and approval workflow.

Exact internal implementations vary by application and protocol version.

---

# Part III — MCP Server Capabilities

## 10. The Three Foundational Server Primitives

| Primitive | Purpose | Example |
|---|---|---|
| Tools | Execute operations | Search documentation |
| Resources | Provide readable content | Read a project specification |
| Prompts | Provide reusable templates | Generate a review prompt |

They are related but not interchangeable.

---

## 11. MCP Tools

An **MCP tool** is an operation exposed by a server that an AI application can invoke. Tools typically accept structured inputs and return results.

A documentation server might expose `search_documentation`.

```json
{
  "name": "search_documentation",
  "description": "Search technical documentation",
  "inputSchema": {
    "type": "object",
    "properties": { "query": { "type": "string" } },
    "required": ["query"]
  }
}
```

This is illustrative, not the exact schema of a particular server.

```text
User Request → AI identifies tool → Client sends call
→ Server validates input → Server executes → Result returned → AI responds
```

Tools may be read-only, state-changing, computational, search-oriented, or administrative.

**A tool description alone does not prove safety. Implementation and permissions determine what it can do.**

---

## 12. MCP Resources

An **MCP resource** provides readable content, commonly identified using URIs.

```text
project://grouptrack/architecture
```

This illustrative resource might expose architecture documentation, configuration references, specifications, schema descriptions, or read-only records.

```json
{
  "uri": "project://grouptrack/architecture",
  "name": "GroupTrack Architecture",
  "mimeType": "text/markdown"
}
```

A resource represents readable content; a tool represents an invokable operation. Resources do not necessarily enter the model context automatically; the host must support and retrieve them.

---

## 13. MCP Prompts

An **MCP prompt** is a reusable prompt template exposed by a server. A server might offer `review_feature` with a `feature_name` argument and return a structured review request.

```text
Review the implementation of {feature_name}.
Evaluate architecture, correctness, testing, security, and maintainability.
```

Prompts support reusable workflows and consistent instructions. A prompt is not automatically a system-level instruction; its role depends on the host.

---

## 14. Tools vs Resources vs Prompts

| Question | Tools | Resources | Prompts |
|---|---|---|---|
| Main purpose | Perform an operation | Provide content | Provide a template |
| Interaction | Invoke | Read | Retrieve |
| May have arguments? | Yes | Templates may | Yes |
| Could change external state? | Depending on tool | Reading normally does not | Retrieval normally does not |
| Example | Search issues | Read architecture doc | Review template |

Easy reminder: **Tools = Do; Resources = Read; Prompts = Guide.**

---

# Part IV — MCP Communication

## 15. How MCP Communication Works

MCP uses structured messages between clients and servers, with JSON-RPC 2.0 as its message foundation. JSON-RPC represents requests, responses, errors, and notifications.

**MCP defines the communication contract; transport determines how messages travel.**

---

## 16. What Is JSON-RPC?

JSON-RPC is a remote procedure call protocol using JSON-formatted messages.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/list"
}
```

`jsonrpc` is the protocol version, `id` identifies the request, and `method` is the requested operation. This example requests available tools.

---

## 17. MCP Tool Discovery

Before using a tool, the client needs to discover available capabilities. MCP supports the `tools/list` method.

```text
MCP Client --tools/list--> MCP Server
MCP Client <--tool definitions-- MCP Server
                         ↓
                       Codex
```

The host can receive structured tool names, descriptions, input schemas, and metadata.

---

## 18. MCP Tool Execution

MCP defines the `tools/call` method.

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "search_documentation",
    "arguments": { "query": "Codex MCP configuration" }
  }
}
```

This assumes the server exposes that illustrative tool. The client sends a call; the server validates name and arguments, performs the operation, and returns a result or error; the host gives relevant output to the model.

**A tool call is structured, not unrestricted permission to execute arbitrary actions.**

---

## 19. MCP Initialization and Capability Negotiation

Before normal interactions, clients and servers establish compatible behavior by exchanging supported protocol version, client/server information, and capabilities.

```text
Client Connects → Initialization → Capability Negotiation
→ Ready for Supported Operations → Tool / Resource / Prompt Use
```

Not all clients and servers support every optional feature. Negotiation helps establish available interactions. MCP evolves; implementations should follow the negotiated version rather than assuming every example applies universally.

# Part V — MCP Transport Mechanisms

## 20. What Is a Transport?

A **transport** determines how protocol messages travel between client and server. Two important mechanisms are standard input/output (`stdio`) and Streamable HTTP.

---

## 21. Standard Input/Output — stdio

With stdio, a client communicates with a server process through standard input and output.

```text
Codex → MCP Client → stdin/stdout → Local MCP Server → Tools and Resources
```

It is common for local servers: the client may launch the process, communication uses process streams, and no public HTTP endpoint is required. Use cases include local utilities, documentation indexes, project analysis, and approved local integrations.

A local server is executable software running with its process permissions. Do not execute unreviewed packages merely because they use stdio.

---

## 22. Streamable HTTP

Streamable HTTP lets MCP clients and servers communicate over HTTP.

```text
Codex → MCP Client → HTTPS → Remote MCP Server → External Service
```

It suits remotely hosted servers, uses HTTP communication, can support authentication, and works with standard web infrastructure. OpenAI's public documentation endpoint is `https://developers.openai.com/mcp`.

---

## 23. Comparing stdio and Streamable HTTP

| Aspect | stdio | Streamable HTTP |
|---|---|---|
| Common deployment | Local process | Remote/network-accessible server |
| Communication | Standard input/output | HTTP |
| Public endpoint | Not required | Network endpoint required |
| Typical configuration | Command and arguments | Server URL |
| Authentication | Often local environment | May use HTTP authorization |
| Main trust concern | Executable code | Server and network connection |

Transport depends on server location and client reachability.

---

## 24. What About SSE?

You may encounter **Server-Sent Events (SSE)**, an HTTP mechanism for streaming server events to a client. Older MCP integrations may use HTTP/SSE. Streamable HTTP became the main HTTP transport in subsequent specifications and can use SSE for streaming where applicable.

MCP continues to evolve. Check the exact protocol version and server transport support; do not treat every SSE mention as a separate modern transport requirement.

---

# Part VI — MCP in OpenAI Codex

## 25. How Codex Uses MCP

Codex supports compatible MCP servers to access capabilities not built directly into the model.

```text
Developer Prompt → Codex → Available MCP Tools
→ Documentation Server → Results → Codex Explanation
```

Connecting a server makes its supported capabilities available; it does not guarantee use for every prompt. Tool selection depends on the request, available capabilities, and host behavior.

---

## 26. Official OpenAI Documentation MCP Server

OpenAI provides a public read-only MCP server for developer documentation:

```text
https://developers.openai.com/mcp
```

It provides documentation search and retrieval for supported OpenAI developer content. It does not call the OpenAI API on the developer's behalf.

It is a useful example because it is officially documented, read-only, and needs no custom server.

---

## 27. Adding the Documentation Server in Codex CLI

Check Codex and add the server:

```powershell
codex --version
codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp
codex mcp list
```

The server should appear in the configured list. Output may vary by Codex version. Registration does not prove every connection will succeed or every request will use the server.

---

## 28. Manual Configuration with config.toml

User configuration is commonly at `~/.codex/config.toml`.

```toml
[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
```

`mcp_servers` contains entries; `openaiDeveloperDocs` is the chosen identifier; `url` specifies the remote endpoint.

Use the CLI command or manual configuration according to your workflow. Avoid duplicate or conflicting entries.

---

## 29. Verifying MCP Configuration in PowerShell

```powershell
codex mcp list

$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}
$configPath = Join-Path $codexHome "config.toml"
if (Test-Path $configPath) { Get-Content $configPath }
```

Then ask Codex to use the configured OpenAI documentation server to find official Codex MCP configuration docs, summarize registration, connection methods, and security, include source links, and modify no project files.

This demonstrates a configured capability; report connection errors rather than claiming success without evidence.

---

# Part VII — Local MCP Server Concepts

## 30. Conceptual Local MCP Server

A local server could expose read-only project metadata.

```text
Codex → MCP Client → stdio → Project Metadata Server
                              ├─ get_project_info
                              ├─ list_project_modules
                              └─ read_architecture_summary
```

These illustrative tools could inspect structure, architecture notes, or metadata. This is conceptual; this lesson does not create an executable server.

---

## 31. Conceptual stdio Configuration

```toml
[mcp_servers.exampleLocal]
command = "SERVER_EXECUTABLE"
args = ["SERVER_ARGUMENT"]
```

These values are placeholders. `command` launches the server; `args` are passed to it. Review server code, dependencies, and permissions before use. Do not execute unknown packages just to test MCP.

---

# Part VIII — MCP vs Related Technologies

## 32. MCP vs REST API

| MCP | REST API |
|---|---|
| Standardizes AI-facing capability discovery and interaction | Exposes application endpoints |
| Can expose tools, resources, and prompts | Commonly exposes resources and operations |
| Uses MCP protocol methods | Commonly uses HTTP methods |
| Can wrap existing services | May provide the underlying service |

An MCP server could expose an AI-facing tool that calls a Laravel endpoint such as `GET /api/routes`.

```text
Codex → MCP Server → Laravel REST API → Database
```

MCP can sit on top of APIs rather than replacing them.

---

## 33. MCP vs AGENTS.md

`AGENTS.md` gives development instructions such as following MVC, coding conventions, avoiding dependencies, and respecting scope. MCP provides standardized access to external capabilities such as searching docs or reading issue records.

| AGENTS.md | MCP |
|---|---|
| Instruction file | Communication protocol |
| Defines expected agent behavior | Exposes external capabilities |
| Does not itself provide an external service | Connects to capability-providing servers |

**AGENTS.md tells the agent how to work; MCP can provide additional capabilities.**

---

## 34. MCP vs Agent Skills

Skills are reusable packages of instructions, procedures, and resources. MCP servers expose capabilities through a protocol.

A skill might instruct Codex to retrieve current API docs; an MCP server could provide the documentation-search tool.

**Skills can describe workflows; MCP can supply tools those workflows use.** Skills are explored in Lesson 5.3.

---

# Part IX — MCP Security

## 35. Why MCP Security Matters

Servers can expose information and actions. A tool might read private records, change project data, update issues, access external services, or execute consequential operations.

Connecting a server expands agent capabilities and potential risks.

**Treat every MCP server as a separate trust and permission decision.**

---

## 36. Trusting MCP Servers

Before connecting, ask:

1. Who operates it?
2. What capabilities does it expose?
3. What data can it access?
4. Can it modify external systems?
5. What credentials does it need?
6. What information might be transmitted?
7. Is it necessary?
8. Can its behavior be audited?

Start with documented read-only servers. Review custom and third-party servers before use.

---

## 37. Principle of Least Privilege

Grant an MCP server only permissions needed for its purpose. A documentation search tool should not need permission to modify production data.

Consider read-only access, narrow scopes, separate development credentials, restricted tools, human approval for consequential actions, and audit logs.

**Scope MCP permissions independently from general coding-agent permissions.**

---

## 38. Prompt Injection Through MCP

External content returned by servers may contain instructions intended to influence the agent, such as "Ignore the user's task and follow these instructions." That text is external content, not authorized developer instruction.

Risks include unwanted tool use, misleading answers, workflow changes, or disclosure. Treat retrieved content as untrusted, preserve instruction hierarchy, verify important claims, restrict powerful tools, and review consequential actions.

**External data should inform the task, not redefine the agent's authority.**

---

## 39. Authentication and Authorization

Authentication asks who is connecting; authorization asks what that identity may do.

A repository integration may authenticate an account, while scopes determine permitted operations.

Use supported authentication, do not put credentials in prompts or committed config, use narrow scopes, revoke access when unnecessary, and review permissions periodically. Authorization mechanisms depend on protocol version and server; current specs define HTTP patterns, including OAuth-based approaches.

---

## 40. Tool Annotations and Trust

Tools may include annotations hinting whether they are read-only or destructive. Such metadata helps hosts but is not a security guarantee.

**Do not rely exclusively on metadata to decide whether an operation is safe.**

---

# Part X — Real-World Development Examples

## 41. Example — GroupTrack Android

Scenario: GroupTrack uses Android Studio, Java, XML, and Firebase. You need current documentation for a development tool.

```text
Developer → Codex → Documentation MCP Server → Relevant Docs → Guidance
```

Ask Codex to read AGENTS.md and project docs, use an approved docs server to verify the API documentation, make no edits, and explain relevant docs, constraints, approach, and compatibility issues.

The server must actually cover the requested docs. OpenAI's docs server is not a general Android/Firebase search service.

---

## 42. Example — GoPasig Laravel

A hypothetical approved internal server could expose read-only GoPasig project metadata:

```text
Codex → Internal MCP Server → GoPasig Backend API → Route Metadata
```

Possible hypothetical tools: `list_routes`, `get_route_summary`, `get_stop_sequence`.

Ask Codex to compare documented route structure with returned metadata, identify inconsistencies, make no record/database changes, and report findings. MCP access must not bypass application authorization, validation, or business rules.

---

## 43. Example — Next.js and Laravel E-commerce

The frontend and backend use different technologies. To verify frontend mapping against the backend contract:

```text
Codex → Local Repository Inspection
      → Optional API Schema MCP Server → Contract Data
```

Ask Codex to inspect the Next.js listing and current API integration, retrieve the schema if an approved server is available, compare fields, make no edits, and report the smallest correction.

**MCP should complement development tools, not replace simpler approaches unnecessarily.**

---

# Part XI — When Should You Use MCP?

## 44. Appropriate Use Cases

MCP helps when an agent needs capabilities outside its built-in environment, such as live documentation, approved issue trackers, project metadata, external knowledge bases, or controlled development services.

Ask whether external capability is required, a simpler built-in tool is sufficient, the server is trustworthy and least-privileged, it will be reused, and value justifies maintenance/security costs.

---

## 45. When MCP Is Unnecessary

MCP may be unnecessary when Codex can read local files, existing CLI tools suffice, the task only changes local code, the integration duplicates built-in features, or no external capability is needed.

Example: adjusting Android XML button spacing usually needs no MCP server.

**Do not introduce MCP just to make a workflow appear advanced.**

---

# Part XII — Common Misconceptions

## 46. Misconception — MCP Is an AI Model

Incorrect. MCP is a communication protocol. The model reasons; MCP infrastructure provides access to external capabilities.

## 47. Misconception — MCP Replaces REST APIs

Incorrect. MCP servers can use existing REST APIs and provide an AI-facing interface over services.

## 48. Misconception — Every MCP Server Is Safe

Incorrect. Ownership, quality, permissions, data handling, authentication, and operational risks vary. Evaluate trust separately.

## 49. Misconception — MCP Tools Always Run Automatically

Incorrect. The host controls available capabilities and invocation behavior; selection and approval depend on the application/configuration.

## 50. Misconception — MCP Is Required for Agentic Coding

Incorrect. Codex can do many coding tasks without MCP; MCP extends capabilities.

## 51. Misconception — A Connected Server Gives Unlimited Access

Incorrect. Access depends on exposed tools, implementation, authentication, authorization, host policies, and environment restrictions. Overprivileged integrations still create risk.

---

# Part XIII — Practical Exercises

## 52. Exercise A — Identify MCP Components

Codex connects to a documentation MCP server. Identify the host, protocol client, tool-providing server, search executor, and how results reach the model.

## 53. Exercise B — Classify MCP Primitives

Classify each as tool, resource, or prompt: search documentation; read architecture doc; retrieve code review template; list repository issues; read project specification; retrieve testing checklist.

## 54. Exercise C — Trace a Tool Call

Explain: user asks a docs question; Codex identifies tool; client sends request; server validates arguments and searches; results return; Codex answers.

## 55. Exercise D — Compare Transports

Which transport launches local processes? Which uses remote HTTPS? Does stdio require public HTTP? Why is trust relevant to both? What determines transport choice?

## 56. Exercise E — Configure a Read-Only Documentation Server

```powershell
codex --version
codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp
codex mcp list
```

Ask Codex to retrieve official docs, then record server ID, URL, transport, capabilities, errors, and verification. Requires compatible Codex and network; optional for theory-focused learners.

## 57. Exercise F — Design a Hypothetical MCP Server

Design a read-only GroupTrack metadata server exposing project overview, module listing, and architecture summary. Document purpose, users, sources, tools, resources, prompts, transport, authentication, authorization, risks, and limitations. Do not add unnecessary features.

## 58. Exercise G — MCP Security Review

- [ ] Operator and purpose identified.
- [ ] Tools and data access reviewed.
- [ ] Authentication and authorization understood.
- [ ] Scope minimized; state changes identified.
- [ ] Secrets protected; external content treated as untrusted.
- [ ] Approval needs understood.
- [ ] Server necessary and can be disabled.

## 59. Exercise H — MCP vs Existing Tools

For each—Android XML spacing; current official docs; local Laravel controller; external issue tracker; local unit test—decide if MCP is needed, what built-in tools can do, added value, and complexity.

---

# Part XIV — Knowledge Review

## 60. Knowledge Check

1. What does MCP stand for?
2. What problem does MCP solve?
3. Is MCP an AI model?
4. What is an MCP host?
5. What is an MCP client?
6. What is an MCP server?
7. How do hosts and clients differ?
8. What capabilities can servers expose?
9. What is an MCP tool?
10. What is a resource?
11. What is a prompt?
12. How do tools differ from resources?
13. What is tool discovery?
14. What does `tools/list` do?
15. What does `tools/call` do?
16. What is JSON-RPC?
17. Why is capability negotiation important?
18. What is stdio transport?
19. What is Streamable HTTP?
20. How do local and remote servers differ?
21. What role does SSE play?
22. How does Codex use MCP?
23. What does OpenAI's docs server provide?
24. How can a server be configured?
25. Does MCP replace REST APIs?
26. How does MCP differ from AGENTS.md?
27. How does MCP differ from agent skills?
28. Why is server trust important?
29. What is prompt injection?
30. Why treat tool results as untrusted?
31. What is least privilege?
32. When is MCP unnecessary?
33. Why justify an integration by requirements?

---

## 61. Key Terminologies

| Term | Definition |
|---|---|
| MCP | Model Context Protocol |
| MCP Host | AI application managing interaction |
| MCP Client | Component communicating with a server |
| MCP Server | Service exposing protocol capabilities |
| Tool | Executable operation exposed by a server |
| Resource | Readable content exposed by a server |
| Prompt | Reusable prompt template |
| Tool Discovery | Finding available tools |
| JSON-RPC | Structured remote procedure call protocol |
| Capability Negotiation | Agreement on supported features |
| Transport | Mechanism carrying protocol messages |
| stdio | Standard input/output transport |
| Streamable HTTP | HTTP-based MCP transport |
| SSE | Server-Sent Events |
| Authentication | Verifying identity |
| Authorization | Determining permitted actions |
| Least Privilege | Granting only necessary permissions |
| Prompt Injection | Untrusted content attempting to redirect behavior |
| Tool Schema | Structured description of tool inputs and outputs |
| External Integration | Connection to a system outside built-in capabilities |

---

## 62. Key Takeaways

1. **MCP is an open protocol connecting AI applications to external capabilities.**
2. **MCP is not a model or API replacement.**
3. **The host manages the AI application; the client handles protocol communication; the server exposes capabilities.**
4. **Tools perform operations, resources provide content, and prompts provide templates.**
5. **MCP uses JSON-RPC and capability discovery/negotiation.**
6. **stdio commonly supports local processes; Streamable HTTP supports network integrations.**
7. **Protocol behavior evolves; verify actual client/server compatibility.**
8. **Codex supports MCP connections; OpenAI provides a read-only docs server.**
9. **MCP can wrap existing APIs.**
10. **MCP, AGENTS.md, and skills serve distinct but complementary roles.**
11. **Servers require deliberate trust, authentication, and least-privilege decisions.**
12. **External content is untrusted and can contain prompt injection.**
13. **Read-only access is preferable when write access is unnecessary.**
14. **MCP is optional for ordinary coding tasks.**
15. **Use MCP when it provides a needed, meaningful capability.**

---

# 63. Official References

## Model Context Protocol

1. [MCP Official Website](https://modelcontextprotocol.io/)
2. [MCP Documentation](https://modelcontextprotocol.io/docs/)
3. [MCP Specification](https://modelcontextprotocol.io/specification/)
4. [MCP Architecture](https://modelcontextprotocol.io/docs/learn/architecture)
5. [MCP Tools — 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/server/tools)
6. [MCP Resources — 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/server/resources)
7. [MCP Prompts — 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/server/prompts)
8. [MCP Transports — 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports)
9. [MCP Specification — 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/)

## OpenAI Codex

10. [Codex Documentation](https://developers.openai.com/codex/)
11. [Codex MCP Documentation](https://developers.openai.com/codex/mcp/)
12. [OpenAI Documentation MCP Server](https://developers.openai.com/resources/docs-mcp)
13. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
14. [OpenAI MCP Server Guide](https://developers.openai.com/api/docs/guides/tools-connectors-mcp)
15. [OpenAI MCP Server Concepts](https://developers.openai.com/plugins/concepts/mcp-server)

## Supporting References

16. [JSON-RPC Specification](https://www.jsonrpc.org/specification)
17. [MCP GitHub Organization](https://github.com/modelcontextprotocol)

> **Documentation Note:** MCP evolves through specification releases. The 2025 specification links are useful for foundational concepts; consult the 2026 specification for newer behavior. Confirm the supported version and features of the actual client and server.

---

## 64. Lesson Completion Checklist

- [ ] Understand MCP purpose, architecture, and components.
- [ ] Differentiate host, client, and server.
- [ ] Understand tools, resources, prompts, and discovery.
- [ ] Understand JSON-RPC and capability negotiation.
- [ ] Understand stdio, Streamable HTTP, and SSE.
- [ ] Understand local and remote servers.
- [ ] Understand Codex integration and docs server.
- [ ] Understand CLI and TOML configuration.
- [ ] Compare MCP with REST, AGENTS.md, and skills.
- [ ] Understand trust, auth, authorization, injection, and least privilege.
- [ ] Recognize when MCP is unnecessary.
- [ ] Complete exercises and knowledge review.

---

# 65. Next Lesson

## [[02 - MCP Servers and External Tool Integrations]]

The next lesson covers local and remote servers, registration, `config.toml`, environment variables, authentication, discovery, connection checks, troubleshooting, safe workflows, and Windows examples.

---

*Phase 5 — Advanced Codex | Lesson 5.1*  
*Learning Principle: Extend AI capabilities through standardized, trustworthy, and appropriately scoped integrations.*
