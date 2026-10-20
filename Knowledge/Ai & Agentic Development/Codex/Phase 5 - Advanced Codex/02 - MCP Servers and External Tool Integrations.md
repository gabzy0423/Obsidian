# 02 — MCP Servers and External Tool Integrations

> **Course:** OpenAI Codex & Agentic Software Development
> **Phase:** 5 — Advanced Codex
> **Lesson:** 5.2 — MCP Servers and External Tool Integrations
> **Difficulty:** Intermediate to Advanced
> **Environment:** Windows 10/11, PowerShell, VS Code, Codex CLI
> **Prerequisite:** [[01 - Introduction to Model Context Protocol (MCP)]]
> **Next Lesson:** [[03 - Agent Skills and Reusable Workflows]]
> **Status:** Theory Documented

---

# 1. Introduction

In Lesson 5.1, we introduced the **Model Context Protocol (MCP)** and its host, client, and server roles. We also discussed tools, resources, prompts, and transports.

This lesson covers practical integration with Codex: registering MCP servers, configuring transports, managing authentication, reviewing exposed tools, and diagnosing connection problems. It focuses on using existing trusted servers rather than building one from scratch.

## Core Principle

**An MCP integration should provide a specific, necessary capability while exposing the smallest reasonable set of permissions.**

---

# Part I — Learning Objectives

## 2. Objectives

By the end of this lesson, the learner should be able to:

1. Differentiate local and remote MCP servers.
2. Understand stdio and Streamable HTTP transports.
3. Configure MCP servers through Codex CLI and `config.toml`.
4. List and inspect registered servers.
5. Understand environment variable forwarding and HTTP authentication.
6. Explain OAuth authentication and tool discovery.
7. Restrict exposed tools and configure their approval behavior.
8. Configure server and tool timeouts.
9. Diagnose common connection failures, including Windows-specific issues.
10. Apply MCP security principles to software projects.
11. Design a minimal integration strategy and verify it safely.

---

# Part II — Understanding MCP Servers

## 3. What Is an MCP Server?

An MCP server is a component that exposes capabilities to compatible AI applications using the Model Context Protocol. Capabilities may include tools, resources, or prompts.

```text
Developer → Codex host/client → MCP server → External capability or data
```

For example, a documentation server might provide search and page retrieval tools. Tool names and behavior are defined by the particular server; examples in this lesson are illustrative unless identified as official.

An MCP server extends Codex's available capabilities. It does not replace Codex or the project's own authorization rules.

---

## 4. Local vs Remote Servers

| Characteristic | Local server | Remote server |
|---|---|---|
| Where it runs | Local computer or execution environment | Network-accessible host |
| Common transport | stdio | Streamable HTTP |
| Configuration | Executable, arguments, optional working directory | Endpoint URL |
| Typical concerns | Trusting executable code and local access | Provider trust, authentication, and data transmission |
| Example | Local project utility | Hosted documentation service |

**Local vs remote** describes where the server runs. **stdio vs HTTP** describes how messages travel. These ideas often pair together but are not the same distinction.

---

## 5. stdio Transport

With stdio, Codex launches a local process and exchanges protocol messages over standard input and output.

```text
Codex MCP client ⇄ stdin/stdout ⇄ Local server process
```

Use cases include local documentation indexes and development utilities. The executable and its dependencies run on the machine, so inspect and trust the server before configuring it.

For a stdio server, stdout is reserved for protocol messages. The server should send ordinary logs to stderr so diagnostic text does not corrupt protocol communication.

---

## 6. Streamable HTTP

Streamable HTTP connects Codex to a server at a network address. Use the server's documented MCP endpoint, preferably HTTPS for remote services. A website URL is not necessarily its MCP endpoint.

Example OpenAI developer documentation endpoint:

```text
https://developers.openai.com/mcp
```

MCP protocol revisions can change lifecycle and transport details. The MCP specification published for `2026-07-28` includes substantial changes to request handling and discovery. Do not assume every Codex client/server pair implements the same revision; follow the versions supported by the installed client and chosen server. See the [MCP specification](https://modelcontextprotocol.io/specification/).

---

## 7. Choosing a Transport

| Requirement | Common option |
|---|---|
| Launch a reviewed local tool | stdio |
| Search hosted documentation | Streamable HTTP |
| Use a managed remote service | Streamable HTTP |
| Run a local development utility | stdio |
| Connect to an internal HTTP service | Streamable HTTP with appropriate access controls |

Choose the simplest supported transport that meets the real requirement. Avoid adding a server just because it is available.

---

# Part III — Codex MCP CLI

## 8. Check Installed CLI Support

Codex options can change. Check the installed version and command help before following a CLI example:

```powershell
codex --version
codex mcp --help
```

The [Codex MCP guide](https://developers.openai.com/codex/mcp/) currently documents `add`, `list`, and OAuth `login` workflows, and points users to `codex mcp --help` for the full command set supported by their installed version.

---

## 9. Add a Remote MCP Server

Register the official OpenAI developer documentation server:

```powershell
codex mcp add openaiDeveloperDocs --url https://developers.openai.com/mcp
```

This adds its configuration. It does not by itself prove the service is reachable or that a tool call works.

---

## 10. Add a stdio Server

The documented CLI pattern for a stdio server is:

```powershell
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

This example follows the [Codex MCP documentation](https://developers.openai.com/codex/mcp/). It may require Node.js, npm/npx, network access to obtain the package, and appropriate review of the package and its permissions. Do not install or run third-party server code without understanding its origin and access.

---

## 11. List and Inspect Servers

List configured integrations:

```powershell
codex mcp list
```

Inside the Codex terminal UI, use:

```text
/mcp
```

`codex mcp list` shows configuration entries, while `/mcp` helps inspect active servers in the session. **Configured does not necessarily mean connected and operational.**

Use `codex mcp --help` for additional inspection or management commands supported by the installed version. Do not rely on undocumented `get`, `remove`, or `logout` subcommands without checking that help output.

---

## 12. Remove or Disable a Server

To disable a configured server while keeping its settings:

```toml
[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
enabled = false
```

To remove a registration, use a removal command if the installed CLI documents one, or carefully remove the corresponding `[mcp_servers.<name>]` table from the configuration file. Review the file and back it up before editing. Removing local configuration does not revoke credentials at the external provider; revoke authorization there when appropriate.

---

# Part IV — Configuring MCP in `config.toml`

## 13. Configuration Location

The default user configuration is:

```text
~/.codex/config.toml
```

On Windows this is commonly `C:\Users\Acer\.codex\config.toml`. If `CODEX_HOME` is set, Codex uses that home directory instead. MCP servers may also be configured in a trusted project's `.codex/config.toml`, subject to project configuration restrictions.

PowerShell path discovery:

```powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}

$configPath = Join-Path $codexHome "config.toml"
$configPath
```

---

## 14. Remote Server Entry

```toml
[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
```

`mcp_servers` is the configuration namespace, `openaiDeveloperDocs` is a local identifier, and `url` is the server address. Use unique, understandable identifiers.

---

## 15. Local stdio Server Entry

```toml
[mcp_servers.projectMetadata]
command = "python"
args = ["C:/MCP/project_metadata_server.py"]
cwd = "C:/MCP"

[mcp_servers.projectMetadata.env]
PROJECT_MODE = "development"
```

This example assumes a compatible server exists at the path and the Python environment is configured. `command`, `args`, `cwd`, and `env` configure the process and environment. The server name and file path are illustrative.

---

## 16. Multiple Servers and Enable/Disable

Each server has its own table and unique identifier:

```toml
[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"

[mcp_servers.internalDocs]
url = "https://mcp.example.com/docs"
enabled = false
```

The second endpoint is hypothetical. `enabled = false` disables a server without deleting its entry. Avoid configuring multiple servers with overlapping tools unless there is a clear need.

---

# Part V — Environment and Authentication

## 17. Environment Variables

Environment variables supply process-specific configuration. They can carry credentials, but they are not automatically a secure secret store.

Check a non-sensitive value in PowerShell without printing secrets:

```powershell
Test-Path Env:PROJECT_MODE
```

Set a value for the current PowerShell process and child processes:

```powershell
$env:PROJECT_MODE = "development"
```

This assignment is temporary to the current shell process unless configured elsewhere.

---

## 18. `env` and `env_vars`

Codex supports `env` to set explicit variables for stdio servers:

```toml
[mcp_servers.projectMetadata.env]
PROJECT_MODE = "development"
```

`env_vars` allows selected existing variables to be forwarded:

```toml
[mcp_servers.projectMetadata]
command = "python"
args = ["C:/MCP/project_metadata_server.py"]
env_vars = ["PROJECT_MODE"]
```

Forward only variables the server needs. Avoid exposing unrelated credentials through a child process environment.

---

## 19. Bearer Tokens and HTTP Headers

For a remote server that supports bearer authentication, Codex can read a token from an environment variable:

```toml
[mcp_servers.privateDocs]
url = "https://mcp.example.com/mcp"
bearer_token_env_var = "PRIVATE_DOCS_TOKEN"
```

The URL and variable name are hypothetical. Codex also supports static `http_headers` and environment-sourced `env_http_headers` for supported scenarios. Do not hardcode secrets into a shared project file or paste them into prompts. Follow the provider's recommended credential handling.

---

## 20. OAuth Authentication

OAuth lets a user authorize a supported integration without giving Codex their account password. For a configured server that supports MCP OAuth, the documented login flow is:

```powershell
codex mcp login SERVER_NAME
```

Replace `SERVER_NAME` with the configured identifier. Not every server supports OAuth. Follow the provider's sign-in and scope instructions. Logging out locally, if supported by the client, may not revoke the grant at the provider; review connected-app authorization separately when needed.

### Authentication vs Authorization

- **Authentication** verifies an identity.
- **Authorization** determines what that identity may access or change.

The MCP server must enforce authorization itself; a prompt or local tool policy is not a substitute for server-side access checks.

---

# Part VI — Tool Discovery and Policy

## 21. Tool Discovery

An MCP client discovers capabilities exposed by a server, including tools and their input descriptions. The protocol methods and lifecycle vary by protocol revision; do not assume a particular RPC flow unless the client and server support it.

Use `/mcp` in the Codex TUI to inspect active integrations and their available tools. Verify a server's actual capabilities rather than assuming its website description matches the connected tool set.

---

## 22. Restrict Exposed Tools

Codex supports an `enabled_tools` allowlist and a `disabled_tools` denylist. Disabled tools are applied after the allowlist.

```toml
[mcp_servers.projectService]
url = "https://mcp.example.com/mcp"
enabled_tools = ["search_projects", "get_project"]
disabled_tools = ["delete_project"]
```

Tool names and endpoint are hypothetical. Prefer an allowlist when only a small number of tools are needed. Server-side permissions must still protect data and state-changing operations.

---

## 23. MCP Tool Approval

Codex supports `default_tools_approval_mode` for a server and `approval_mode` overrides for individual tools. Documented default modes include `auto`, `prompt`, `writes`, and `approve`; `writes` prompts for tools that are not marked read-only.

```toml
[mcp_servers.projectService]
url = "https://mcp.example.com/mcp"
default_tools_approval_mode = "prompt"

[mcp_servers.projectService.tools.search_projects]
approval_mode = "auto"
```

MCP tool approval is distinct from shell sandbox and approval policies. It does not replace server-side authorization or careful review of consequential actions.

---

## 24. Timeouts and Required Servers

The current Codex MCP reference documents:

| Setting | Purpose | Documented default |
|---|---|---|
| `startup_timeout_sec` | Maximum wait for server startup | 10 seconds |
| `tool_timeout_sec` | Maximum wait for one tool call | 60 seconds |
| `required` | If enabled and initialization fails, fail startup/resume rather than silently continue | Not applicable |

Example:

```toml
[mcp_servers.projectService]
url = "https://mcp.example.com/mcp"
startup_timeout_sec = 20
tool_timeout_sec = 90
required = false
```

Increasing a timeout does not fix invalid URLs, missing executables, bad credentials, or server errors. Keep optional services optional; mark a server required only when the workflow cannot function correctly without it.

---

# Part VII — Troubleshooting

## 25. Basic Troubleshooting Sequence

```text
Confirm registration
        ↓
Check active Codex home and config
        ↓
Verify transport and executable/endpoint
        ↓
Check authentication and authorization
        ↓
Inspect tool filters and enabled state
        ↓
Test a low-risk capability
```

Start with the layer that failed. A registration entry alone does not establish a successful connection or tool invocation.

---

## 26. Server Not Listed

Check the installed client and active configuration location:

```powershell
codex mcp --help
codex mcp list
$env:CODEX_HOME
```

Possible causes include a different `CODEX_HOME`, an untrusted project's local configuration, invalid TOML, a disabled entry, or an incorrect server identifier.

---

## 27. Local Server Fails to Start

Check that required executables and files exist:

```powershell
Get-Command python -ErrorAction SilentlyContinue
Get-Command node -ErrorAction SilentlyContinue
Get-Command npx -ErrorAction SilentlyContinue
Test-Path "C:\MCP\project_metadata_server.py"
```

Common causes include missing runtimes, wrong PATH, incorrect arguments, absent dependencies, bad working directory, or server startup errors. On Windows, package launchers may be `.cmd` files; follow the server's Windows-specific instructions instead of guessing command syntax.

---

## 28. Remote Server Unreachable

Check the exact MCP endpoint, network policy, DNS, firewall/proxy, provider availability, transport support, and authentication. A TCP connectivity check such as `Test-NetConnection host -Port 443` only verifies basic connectivity; it does not prove MCP protocol compatibility or endpoint health.

---

## 29. Authentication Failure or Missing Tools

For authentication failures, confirm the server's supported method, credential source, account, and required scopes. Never solve a private service authentication error by disabling its authorization.

If tools do not appear, check server startup, tool allow/deny lists, authentication-dependent discovery, enabled state, client support, and whether the current session needs to reload its configuration. Use `/mcp` to inspect the active tool set.

---

## 30. Tool Timeout or Wrong Environment

Inspect the server's health and the operation's expected duration before increasing `tool_timeout_sec`. For environment issues, check variable names and process inheritance without printing secrets. A variable set in another terminal may not be available to the Codex process.

| Symptom | First check |
|---|---|
| Server missing | `codex mcp list`, `CODEX_HOME`, project trust |
| Local process fails | `Get-Command`, file path, `cwd`, dependencies |
| Remote connection fails | Endpoint, network, TLS, authentication |
| Tools missing | `/mcp`, enabled state, allow/deny lists |
| Tool times out | Server health and operation duration |
| Environment missing | Variable name and process inheritance |

---

# Part VIII — Security and Project Examples

## 31. Treat MCP Servers as Trust Boundaries

Before enabling a server, determine who maintains it, what data and operations it can reach, whether it requires credentials, and whether it can make external changes. An MCP server is not trustworthy merely because it implements MCP.

Apply least privilege:

- Prefer read-only capabilities when they meet the task.
- Limit tools with an allowlist where appropriate.
- Use narrow provider scopes.
- Forward only required environment variables.
- Keep credentials out of Git and prompts.
- Review state-changing calls.
- Remove unused integrations and revoke external grants when appropriate.

---

## 32. Prompt Injection and External Results

MCP results may contain untrusted external text, including instructions that attempt to redirect the agent. Treat returned content as data, not as authorization to change the task, bypass project rules, or perform unrelated operations. Keep the task scope explicit and review consequential tool calls.

---

## 33. GroupTrack Android Example

Project context: Android Studio, Java, XML layouts, Firebase, and a documented MVP.

A documentation MCP may help verify API usage, but the OpenAI developer documentation server covers OpenAI developer materials; it is not a general Android or Firebase documentation source.

```text
Read project instructions and inspect the sign-in implementation.
If an approved documentation MCP covers the API in question, use it to verify usage.
Do not modify files yet. Report the smallest implementation that stays within the MVP.
```

MCP complements source inspection and project documentation; it does not authorize scope expansion.

---

## 34. GoPasig Laravel Example

An approved read-only service might expose route or stop metadata. Such example tools are hypothetical. The Laravel application and MCP server must still enforce user permissions and business rules.

```text
Inspect the route-management code and approved business rules.
If the configured read-only metadata MCP is available, retrieve the relevant summary.
Do not modify route data. Report discrepancies with evidence.
```

---

## 35. Next.js and Laravel Example

For a frontend/backend integration, an approved documentation server may provide an API contract. Compare it with actual Laravel routes and responses and inspect the Next.js mapping. MCP complements codebase exploration; it does not replace verifying the server implementation.

---

# Part IX — Practical Exercises

## 36. Exercise A — Compare Server Types

Explain where local and remote servers run, which transports are common, their distinct risks, and when each deployment model is useful.

## 37. Exercise B — Inspect CLI Support

Run `codex --version`, `codex mcp --help`, and `codex mcp list`. Record which management commands the installed version supports. Do not assume commands from another release exist locally.

## 38. Exercise C — Register Official Documentation MCP (Optional)

Add the OpenAI developer documentation integration using the command in Section 9. List the configured server, inspect `/mcp` in Codex, then make a low-risk documentation query. Record registration and functional verification separately.

## 39. Exercise D — Review a TOML Entry

```toml
[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
enabled = true
```

Identify the table, identifier, URL, and effect of `enabled = false`. Explain why registering the same server under multiple names may be confusing.

## 40. Exercise E — Restrict Tool Access

Given a hypothetical server with `search_projects`, `get_project`, `update_project`, and `delete_project`, configure only the first two through `enabled_tools`. Explain why server-side authorization is still required.

## 41. Exercise F — Diagnose a Connection Failure

For a missing server, failed local process, unreachable endpoint, OAuth error, or missing tool, record the symptom, likely causes, first diagnostic, safe next action, and verification method.

## 42. Exercise G — Design an Integration Plan

```markdown
# MCP Integration Plan

## Project and problem
## Why MCP is needed
## Proposed trusted server
## Local or remote / transport
## Required data and tools
## Authentication and authorization
## Security risks and mitigations
## Configuration scope
## Verification and troubleshooting
## Removal and credential revocation
```

Include only capabilities needed for the approved workflow.

---

# Part X — Review

## 43. Knowledge Check

1. What is an MCP server?
2. How do local/remote deployment and stdio/HTTP transport differ?
3. What does the `command` key configure?
4. What does the `url` key configure?
5. How can you add and list an MCP server in Codex?
6. Why does registration not prove a server works?
7. What do `env` and `env_vars` do?
8. What does `bearer_token_env_var` reference?
9. How do authentication and authorization differ?
10. How can `enabled_tools` and `disabled_tools` restrict tools?
11. How is MCP tool approval different from shell sandbox policy?
12. What do startup and tool timeouts control?
13. What can cause a local server not to start on Windows?
14. Why should external tool results be treated as untrusted data?
15. Why must an MCP server enforce authorization itself?
16. When is adding an MCP integration unnecessary?

---

## 44. Key Terminologies

| Term | Definition |
|---|---|
| MCP Server | Component exposing capabilities over MCP |
| Local MCP Server | Server process running in a local or execution environment |
| Remote MCP Server | Server reached over a network |
| stdio | Process communication using standard input and output |
| Streamable HTTP | MCP transport over HTTP |
| Server Registration | Adding an MCP server to Codex configuration |
| `CODEX_HOME` | Environment variable selecting the Codex home directory |
| `env` | Explicit environment variables for a stdio server |
| `env_vars` | Selected environment variables forwarded to a stdio server |
| Bearer Token | Credential sent in an HTTP Authorization header |
| OAuth | Authorization flow for delegated access |
| Tool Discovery | Learning which capabilities a server exposes |
| Tool Allowlist | Explicit set of available tools |
| Tool Approval Mode | Policy for approving server tool calls |
| Startup Timeout | Maximum wait for a server to initialize |
| Tool Timeout | Maximum wait for an individual server tool call |
| Prompt Injection | Untrusted content attempting to redirect model behavior |
| Least Privilege | Limiting access to what the task requires |

---

## 45. Key Takeaways

1. MCP servers extend Codex with external tools and data.
2. Local/remote describes deployment; stdio/Streamable HTTP describes transport.
3. Codex can configure servers through CLI commands or `config.toml`.
4. A configured server is not necessarily connected or functional.
5. Environment variables and credentials should be forwarded deliberately.
6. Authentication and authorization are separate responsibilities.
7. Tool allowlists and approval settings can reduce unnecessary exposure.
8. MCP tool approval does not replace server-side authorization.
9. Timeouts should reflect real operation requirements.
10. Treat each server and its returned content as a trust boundary.
11. Inspect the installed CLI help and current docs because behavior evolves.
12. Add only integrations that solve a real project need.

---

## 46. Official References

### OpenAI Codex

1. [Codex MCP Documentation](https://developers.openai.com/codex/mcp/)
2. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
3. [Codex Configuration Basics](https://developers.openai.com/codex/config-basic/)
4. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
5. [OpenAI Developer Documentation MCP](https://developers.openai.com/learn/docs-mcp)

### Model Context Protocol

6. [Official MCP Website](https://modelcontextprotocol.io/)
7. [MCP Documentation](https://modelcontextprotocol.io/docs/)
8. [MCP Specification](https://modelcontextprotocol.io/specification/)
9. [MCP 2025-11-25 Transport Specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports)
10. [MCP 2026-07-28 Specification](https://modelcontextprotocol.io/specification/2026-07-28/)

### Supporting References

11. [PowerShell Documentation](https://learn.microsoft.com/powershell/)
12. [TOML Specification](https://toml.io/en/)
13. [JSON-RPC Specification](https://www.jsonrpc.org/specification)

> **Documentation Note:** Codex commands, configuration keys, authentication options, and supported MCP protocol revisions can change. Confirm against the installed Codex version and current official references.

---

## 47. Lesson Completion Checklist

- [ ] Understand local and remote MCP servers.
- [ ] Understand stdio and Streamable HTTP transports.
- [ ] Inspect current Codex MCP CLI support.
- [ ] Register, list, and verify an MCP server.
- [ ] Configure MCP servers in `config.toml`.
- [ ] Understand `CODEX_HOME` and project configuration scope.
- [ ] Understand environment variable forwarding.
- [ ] Understand bearer-token and OAuth authentication.
- [ ] Distinguish authentication from authorization.
- [ ] Inspect tool discovery and restrict tool access.
- [ ] Understand MCP tool approval settings.
- [ ] Configure and troubleshoot timeouts.
- [ ] Diagnose local and remote connection failures.
- [ ] Apply least-privilege and credential-protection practices.
- [ ] Understand prompt injection risks from external content.
- [ ] Apply MCP concepts to Android, Laravel, and Next.js projects.
- [ ] Complete practical exercises and knowledge check.

---

# 48. Next Lesson

## [[03 - Agent Skills and Reusable Workflows]]

The next lesson covers reusable agent skills, `SKILL.md`, skill discovery and invocation, how skills relate to `AGENTS.md` and MCP, and how to avoid unnecessary workflow abstractions.

---

*Phase 5 — Advanced Codex | Lesson 5.2*
*Learning Principle: Integrate only the external capabilities your workflow needs, verify every connection, and preserve clear security boundaries.*
