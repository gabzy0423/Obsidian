# 03 — Model Selection and Reasoning Settings

> **Course:** OpenAI Codex & Agentic Software Development  
> **Phase:** 4 — Codex Configuration  
> **Lesson:** 4.3 — Model Selection and Reasoning Settings  
> **Difficulty:** Intermediate  
> **Environment:** Windows, PowerShell, VS Code, Codex CLI  
> **Prerequisite:** [[02 - Understanding config.toml]]  
> **Next Lesson:** [[04 - Sandbox and Approval Policies]]  
> **Status:** In Progress

---

## 1. Introduction

In Lesson 4.2, we explored the structure of `config.toml`, including TOML syntax, configuration keys, nested tables, and configuration precedence.

Now, we will focus on two important Codex configuration settings:

```toml
model = "SUPPORTED_MODEL_ID"
model_reasoning_effort = "medium"
```

These settings answer two different questions:

1. **Model Selection:** Which AI model should Codex use?
2. **Reasoning Effort:** How much reasoning effort should the selected model apply?

Understanding these settings helps developers balance quality, speed, resource usage, and task complexity.

For example, improving the spacing of an Android XML layout is different from investigating an intermittent database transaction bug.

Both are software development tasks, but their reasoning requirements may differ significantly.

**Core Principle: Select models and reasoning settings according to task requirements, supported capabilities, and verified results—not simply by choosing the highest available setting.**

---

# Part I — Understanding Model Selection

## 2. Learning Objectives

By the end of this lesson, the learner should be able to:

1. Explain model selection in Codex.
2. Understand the `model` configuration key.
3. Distinguish model capability from reasoning effort.
4. Understand model availability and compatibility.
5. Explain reasoning effort levels.
6. Configure reasoning effort through `config.toml`.
7. Override models and reasoning settings through the CLI.
8. Understand temporary versus persistent configuration.
9. Compare quality, latency, and resource tradeoffs.
10. Choose settings according to task complexity.
11. Understand global and project-specific settings.
12. Use configuration profiles appropriately.
13. Recognize common model configuration errors.
14. Evaluate model performance using repeatable tasks.
15. Apply model selection principles to Android, Laravel, and Next.js development.

---

## 3. What Is Model Selection?

Model selection is the process of choosing which AI model performs the requested task.

Codex is an agentic coding environment that can interact with source code, execute permitted commands, and assist with software development.

The selected model provides the reasoning and language capabilities used within that environment.

### Conceptual Architecture

```text
Developer
    |
    v
Codex CLI
    |
    +-- Selected Model
    |       +-- Code understanding
    |       +-- Planning
    |       +-- Reasoning
    |       +-- Code generation
    |
    +-- Tools
    |       +-- File inspection
    |       +-- File editing
    |       +-- Terminal commands
    |
    +-- Project Context
            +-- AGENTS.md
            +-- Documentation
            +-- Existing source code
```

The model does not replace the development environment. It operates through the tools and permissions available to Codex.

**Key Principle: The model provides intelligence; Codex provides the agentic workflow and tool environment.**

---

## 4. Why Model Selection Matters

Different models can vary in their ability to understand large codebases, follow detailed instructions, reason about complex requirements, generate correct code, investigate subtle bugs, analyze architecture, use tools effectively, and complete multi-step tasks.

They can also differ in speed, resource usage, and availability.

### Example

**Task A:** Change a button label from "Login" to "Sign In".

**Task B:** Investigate why overlapping bus schedules occasionally pass validation when concurrent dispatch requests occur.

Task A is narrow. Task B may require understanding transactions, concurrency, locking, business rules, and tests. Evaluate whether Task B justifies a more capable model or higher reasoning effort.

---

## 5. Understanding Model Identifiers

Codex uses model identifiers to select supported models. A model identifier is a machine-readable name.

```toml
model = "SUPPORTED_MODEL_ID"
```

`SUPPORTED_MODEL_ID` is a placeholder. Replace it with an identifier supported by the current Codex environment.

A model's marketing name and technical identifier may differ. A model listed in the OpenAI API catalog is not automatically available in every Codex account or client version.

Before configuring a model:

1. Check the installed Codex version.
2. Inspect the model choices available in Codex.
3. Confirm account access.
4. Check supported reasoning levels.
5. Test it on an actual task.

---

## 6. Model Availability

Availability can depend on Codex client version, authentication method, account access, organization restrictions, product support, and changes to the model catalog.

**Do not assume that a model mentioned in a tutorial is available in your installation.**

---

## 7. Model Selection vs Model Configuration

Model selection is the decision about which model to use. Model configuration is the mechanism used to apply that decision.

Decision: use a supported model suitable for complex code analysis.

Configuration:

```toml
model = "SUPPORTED_MODEL_ID"
```

To use Codex's effective default model, omit the `model` key.

**Explicit configuration is optional when the default already satisfies your requirements.**

---

# Part II — Understanding Reasoning Effort

## 8. What Is Reasoning Effort?

Reasoning effort is a setting that influences how much internal reasoning work a supported model performs while processing a request.

```toml
model_reasoning_effort = "medium"
```

It can affect problem analysis, planning depth, alternative evaluation, multi-step reasoning, latency, and resource consumption.

It does not directly specify the number of code files, tool calls, or implementation steps.

---

## 9. Model vs Reasoning Effort

| Model Selection | Reasoning Effort |
|---|---|
| Chooses the AI model | Controls requested reasoning intensity |
| Determines available capabilities | Influences reasoning applied |
| Uses the `model` key | Uses `model_reasoning_effort` |
| Depends on availability | Depends on supported effort values |

```text
Selected Model → Reasoning Effort → Task Analysis
             → Tool Use and Implementation → Verification
```

**A capable model at lower effort and a different model at higher effort are not necessarily equivalent.**

---

## 10. Common Reasoning Effort Levels

OpenAI reasoning models support model-dependent effort values.

| Level | General Characteristic |
|---|---|
| `low` | Prioritizes efficiency |
| `medium` | Balanced reasoning |
| `high` | More extensive reasoning |
| `xhigh` | Deeper reasoning where supported |
| `max` | Maximum supported reasoning for certain models |

Some models also support `none` or `minimal`. **Not every model supports every value.**

```toml
model_reasoning_effort = "high"
```

This requests high reasoning effort; it does not guarantee a better result than medium for every task.

---

## 11. Low Reasoning Effort

Low effort is generally appropriate for narrow tasks that need little investigation.

Examples: inspect a specific file, make a straightforward text change, rename a known variable, summarize a small code section, or correct obvious formatting.

```text
Read the existing login XML layout.
Change the button label from "Login" to "Sign In".
Do not modify any other UI element or authentication logic.
Report modified files.
```

Low effort may be faster and sufficient for clear tasks. It may be unsuitable when requirements are ambiguous or components interact.

---

## 12. Medium Reasoning Effort

Medium effort balances analysis and efficiency. It may suit a well-defined feature, validation update, focused tests, small service refactor, or localized bug.

```toml
model_reasoning_effort = "medium"
```

Example prompt: inspect the task creation workflow; identify current validation; implement the approved required-title rule; follow AGENTS.md; add focused regression tests; avoid unrelated modules; run relevant verification and report results.

**Medium is a useful starting point for many ordinary development tasks, subject to model support.**

---

## 13. High Reasoning Effort

High effort may suit complex debugging, architecture planning, inconsistent behavior, concurrency problems, multi-module interactions, or risky refactoring.

```toml
model_reasoning_effort = "high"
```

Example: investigate schedule validation, trace creation flow and transaction boundaries, identify evidence-supported causes, do not modify code, and provide a focused correction plan with risks and verification.

High effort should support analysis, not encourage unnecessary complexity.

---

## 14. Extra-High and Maximum Reasoning Effort

Some supported models provide higher levels such as `xhigh` or `max`. Use them only when supported by the selected model and Codex client.

Potential uses include difficult multi-step investigations, complex architecture tradeoffs, extensive code review, challenging debugging, or high-stakes correctness analysis.

Higher effort may increase response time, reasoning token usage, and applicable usage or API costs.

**Use very high reasoning settings only when representative tasks demonstrate a meaningful benefit.**

---

## 15. Does Higher Reasoning Always Mean Better Code?

No. A higher setting does not prevent misunderstanding, overengineering, regressions, missed edge cases, unrelated edits, or incomplete tests.

These require good task scoping, project context, verification, and review.

**Reasoning effort is not a replacement for software engineering discipline.**

---

# Part III — Performance Tradeoffs

## 16. Quality, Latency, and Resource Usage

Selection involves quality, latency, and resource usage.

| Configuration | Potential Benefit | Potential Tradeoff |
|---|---|---|
| Lower effort | Faster routine work | Less extensive reasoning |
| Medium effort | Balanced performance | May need escalation for difficult tasks |
| Higher effort | More thorough analysis | Longer latency and greater usage |
| More capable model | Better on demanding tasks | Different access or cost constraints |

These are general tendencies, not guaranteed measurements.

---

## 17. Why More Reasoning Can Increase Latency

Reasoning models may perform internal reasoning before or between visible outputs and tool interactions. Actual response time also depends on model, task complexity, context size, tool execution, network conditions, and service load.

A small XML label change may finish quickly. A multi-file investigation with test runs may take longer; reasoning effort is not the only cause.

---

## 18. Reasoning Effort vs Context Window

**Context window** is the amount of information a model can process within its context limits. **Reasoning effort** is the requested amount of internal reasoning applied.

A large codebase may require context management even with high effort. Increasing effort does not make the entire repository fit into context.

**Context engineering and reasoning configuration solve different problems.**

---

## 19. Reasoning Effort vs Tool Execution

Reasoning effort does not replace tools. A model may analyze a Java compilation error, but needs actual build output to confirm compilation.

```text
Understand Requirements → Inspect Code → Analyze Problem
→ Implement Approved Change → Run Build or Tests → Review Results
```

**Verification should be based on observed tool results, not model confidence.**

# Part IV — Configuring Models in Codex

## 20. Global Model Configuration

User-level Codex configuration is commonly at `~/.codex/config.toml`.

Windows example: `C:\Users\Acer\.codex\config.toml`.

```toml
model = "SUPPORTED_MODEL_ID"
model_reasoning_effort = "medium"
```

Replace the placeholder with an available model identifier. Global configuration establishes reusable defaults, subject to precedence and enforced policies. One global preference may not suit every task.

---

## 21. Configuring Reasoning Without Selecting a Model

```toml
model_reasoning_effort = "medium"
```

Codex can use the effective selected or default model. This avoids unnecessarily pinning configuration to a model identifier. The selected model must support the effort level.

For beginners, keeping the default model and configuring only necessary settings is often simpler.

---

## 22. Combined Configuration Example

```toml
model_reasoning_effort = "medium"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

`model_reasoning_effort` requests medium effort; `approval_policy` controls interactive approval behavior; `sandbox_mode` restricts writes to the permitted workspace.

Model and reasoning settings do not override sandbox or approval restrictions.

---

## 23. Windows PowerShell — Inspect Configuration

Check the CLI version:

```powershell
codex --version
```

Resolve the Codex home and config path:

```powershell
$codexHome = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $HOME ".codex"
}
$configPath = Join-Path $codexHome "config.toml"
```

Inspect the file:

```powershell
if (Test-Path $configPath) {
    Get-Content $configPath
} else {
    Write-Output "No config.toml found."
}
```

Open in VS Code:

```powershell
if (Test-Path $configPath) {
    code $configPath
}
```

Inspect for sensitive integration details before sharing configuration contents.

---

## 24. Backing Up Configuration Before Changes

```powershell
if (Test-Path $configPath) {
    Copy-Item $configPath "$configPath.bak"
}
```

A configuration change may introduce invalid TOML, unsupported settings, incorrect model selection, or unexpected behavior.

**Treat configuration edits as controlled changes that should be reversible.**

---

# Part V — CLI Overrides

## 25. What Is a CLI Override?

A CLI override changes a setting for one Codex invocation without permanently editing user configuration. It is useful when a task needs a different model or effort.

---

## 26. Model Override with `--model`

```powershell
codex --model SUPPORTED_MODEL_ID
```

Short form:

```powershell
codex -m SUPPORTED_MODEL_ID
```

Replace the placeholder with a model actually available in the environment. Use CLI model overrides for temporary experimentation.

---

## 27. Reasoning Override with `-c`

```powershell
codex -c 'model_reasoning_effort="high"'
codex -c 'model_reasoning_effort="low"'
```

Single quotes help preserve the intended TOML string in PowerShell. The selected model must support the requested level.

---

## 28. Combining Model and Reasoning Overrides

```powershell
codex -m SUPPORTED_MODEL_ID -c 'model_reasoning_effort="high"'
```

This selects a model and requests high effort for the session without changing global defaults. Temporary overrides support controlled comparisons.

---

## 29. Interactive Model Selection

Depending on the installed version, Codex's interactive interface may provide a model selector such as `/model`.

1. Start Codex.
2. Inspect available interactive commands.
3. Open the selector if supported.
4. Review available models and reasoning settings.
5. Choose a supported combination.

Interactive controls can change between releases. Use the installed client's help and current official documentation.

---

# Part VI — Global and Project-Specific Settings

## 30. Global Configuration

A global configuration establishes reusable user-level preferences, commonly at `C:\Users\Acer\.codex\config.toml`.

```toml
model_reasoning_effort = "medium"
```

Advantages include consistent defaults, less repetitive setup, and centralized preferences. A single setting may not suit every project.

---

## 31. Project-Specific Configuration

Codex can load supported project-level configuration from a trusted repository.

```text
GroupTrack/
├── .codex/config.toml
├── AGENTS.md
├── docs/
└── app/
```

Example:

```toml
model_reasoning_effort = "medium"
```

Project loading depends on trust rules. Some settings cannot be overridden at project level; higher-priority sources and enforced policies still apply.

**Project configuration customizes supported operating settings; `AGENTS.md` defines development rules.**

---

## 32. Configuration Profiles

Profiles provide reusable named configuration variants.

```toml
[profiles.routine]
model_reasoning_effort = "medium"

[profiles.deep_review]
model_reasoning_effort = "high"
```

Launch a profile:

```powershell
codex --profile routine
codex --profile deep_review
```

The model must support the profile's reasoning effort.

---

## 33. Profiles vs CLI Overrides

| Profiles | CLI Overrides |
|---|---|
| Reusable named settings | Temporary invocation-specific settings |
| Stored in configuration | Provided at launch |
| Useful for repeated workflows | Useful for one-time adjustments |
| Require initial setup | Require no saved profile |

```powershell
codex --profile routine
codex --profile deep_review
codex -c 'model_reasoning_effort="high"'
```

Use profiles for repeated workflows and overrides for occasional exceptions.

---

# Part VII — Task-Based Model Selection

## 34. Choosing Settings by Task Complexity

Classify task complexity before choosing an effort:

```text
Simple and narrow → Consider lower effort
Moderate complexity → Consider medium effort
Complex or ambiguous → Consider high effort, if supported
Then verify actual results
```

These are starting points, not strict rules.

---

## 35. Task Selection Matrix

| Task | Starting Effort | Reason |
|---|---|---|
| Read a small source file | Low | Limited reasoning required |
| Update a button label | Low | Narrow and explicit change |
| Add basic form validation | Medium | Requires behavior and test review |
| Implement a scoped feature | Medium | Several implementation steps |
| Refactor a service | Medium or high | Depends on dependencies and risk |
| Debug an intermittent issue | High | Requires hypothesis evaluation |
| Analyze concurrency problems | High | Multiple interacting conditions |
| Review a complex architecture | High | Requires broader reasoning |
| Investigate a difficult regression | High or higher if supported | Extensive analysis may help |

**Task risk and ambiguity matter as much as task size.**

---

## 36. Example — Android UI Development

Scenario: improve spacing and alignment of an existing GroupTrack login screen.

Complexity is low to moderate. A starting setting might be:

```toml
model_reasoning_effort = "medium"
```

Prompt Codex to inspect the screen, change spacing/alignment only, preserve authentication, navigation, architecture, and MVP scope, add no dependencies, run relevant checks, report files, and not commit or push.

For only a text-label update, lower effort may suffice.

---

## 37. Example — Android Crash Investigation

An activity crashes during initialization; complexity may be moderate to high.

```toml
model_reasoning_effort = "high"
```

Ask Codex to inspect lifecycle, view initialization, XML references, stack trace, and tests; identify the likely cause using evidence; make no edits yet; and provide a minimal correction plan and verification steps.

---

## 38. Example — Laravel Validation

A Laravel API accepts invalid request data. A starting point might be:

```toml
model_reasoning_effort = "medium"
```

Ask Codex to inspect route, controller, Form Request, and tests; correct the approved validation issue; preserve the API contract; add a focused regression test in an isolated test environment; avoid unrelated modules; and report actual results.

A clear validation task often benefits more from good context and tests than unnecessarily high effort.

---

## 39. Example — GoPasig Scheduling Investigation

The fleet system occasionally permits conflicting schedule assignments. Concurrency and scheduling correctness may require reasoning across several components and rules.

```toml
model_reasoning_effort = "high"
```

Ask Codex to inspect scheduling documentation, validation, bus/driver availability, transactions, locks, services, and tests. Request evidence-supported causes, correction options, regression strategy, risks, and assumptions. Do not modify code during analysis.

---

## 40. Example — Next.js Frontend

A product list renders incorrect API field values. A starting setting may be medium. Ask Codex to compare frontend mapping with the Laravel API contract, identify the wrong field, implement the smallest approved fix, avoid backend changes unless evidence requires them, run relevant checks, and report files/results.

The goal is enough reasoning to make the correct change, not maximum effort.

# Part VIII — Measuring Model Performance

## 41. Why Benchmark Models?

Evaluate models using actual tasks rather than assumptions. A model may perform well on one problem type and less well on another.

Consider: Did it satisfy requirements? Did tests pass? Were unrelated files modified? Was code maintainable? Were instructions followed? How long did the task take? What resources were used?

**Compare models based on verified outcomes.**

---

## 42. Designing a Fair Comparison

Use the same repository baseline, requirements, instructions, acceptance criteria, test commands, and review standards. Change one primary variable at a time, such as effort with a fixed model, or model with fixed effort.

AI outputs vary; one successful attempt does not establish consistent performance.

---

## 43. Example Evaluation Table

| Metric | Run A | Run B |
|---|---|---|
| Model | Record ID | Record ID |
| Reasoning effort | Medium | High |
| Task | Same task | Same task |
| Tests passed | Record result | Record result |
| Scope violations | Record count | Record count |
| Unexpected files | Record count | Record count |
| Completion time | Record duration | Record duration |
| Review findings | Record findings | Record findings |
| Overall decision | Evaluate | Evaluate |

Do not invent measurements. For comparisons, use several representative tasks.

---

## 44. Quality Evaluation Criteria

- **Correctness:** Does the solution behave as required?
- **Scope compliance:** Did Codex stay within the task?
- **Maintainability:** Is the implementation understandable?
- **Architecture consistency:** Does it follow existing conventions?
- **Verification:** Are test claims supported by command output?
- **Efficiency:** Was work completed with reasonable time and resource usage?

A faster result is not better if it introduces defects or unnecessary changes.

---

# Part IX — Common Configuration Mistakes

## 45. Mistake — Unsupported Model Identifier

```toml
model = "UNKNOWN_MODEL_ID"
```

The model may not exist or be available. Inspect Codex's available models and verify the identifier.

---

## 46. Mistake — Unsupported Reasoning Effort

```toml
model_reasoning_effort = "super-high"
```

This is not a documented standard value. Use a level supported by the selected model.

---

## 47. Mistake — Assuming Every Model Supports Every Effort

An effort level supported by one model may be unavailable for another. Check model-specific capabilities.

**Model and reasoning settings must be compatible.**

---

## 48. Mistake — Always Using Maximum Reasoning

Maximum effort may add latency and resource use without helping simple tasks. Start appropriately and increase only when useful.

---

## 49. Mistake — Changing Too Many Settings

Changing model, effort, sandbox, and approval policy together complicates troubleshooting. Change one setting at a time and verify its effect.

---

## 50. Mistake — Confusing Reasoning with Autonomy

Reasoning effort does not grant filesystem permissions.

```toml
model_reasoning_effort = "high"
sandbox_mode = "read-only"
```

The model may reason extensively, while the sandbox still prevents file changes.

**Reasoning capability and execution permissions are independent.**

---

## 51. Mistake — Using Reasoning to Compensate for Missing Context

High effort cannot reliably repair a vague request.

Weak prompt: `Fix the project.`

Better: Ask Codex to inspect the task creation workflow, investigate why empty titles are accepted, follow AGENTS.md, make no edits, and report evidence-supported cause and a focused plan.

**Better context and clearer requirements often provide more value than simply increasing effort.**

---

# Part X — Safe Configuration Workflow

## 52. Recommended Workflow

```text
Identify task → Assess complexity and risk → Check available models
→ Choose supported effort → Start Codex → Provide context
→ Execute approved workflow → Test and review diff
→ Evaluate results → Keep or adjust settings
```

Model selection is an engineering decision informed by evidence.

---

## 53. Recommended Starting Configuration

A learner using Codex across Android, Laravel, and Next.js might start with:

```toml
model_reasoning_effort = "medium"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

This leaves model selection to the effective Codex default, requests balanced effort, and retains approval and workspace controls. It is an example, not universal configuration.

For read-only investigation:

```powershell
codex --sandbox read-only
```

For difficult analysis:

```powershell
codex -c 'model_reasoning_effort="high"' --sandbox read-only
```

---

# Part XI — Practical Exercises

## 54. Exercise A — Inspect Available Models

Check the Codex version and help, inspect model-selection controls, record available identifiers, and identify supported effort levels where displayed.

```powershell
codex --version
codex --help
```

Deliverable: a short model availability inventory.

---

## 55. Exercise B — Reasoning Level Classification

Choose a starting effort for: rename an XML label; add required-field validation; investigate an Android lifecycle crash; review a transaction race; summarize a small Java class; implement a scoped API endpoint.

For each, explain the choice and verification strategy.

---

## 56. Exercise C — Global Configuration

Practice setting a default:

```toml
model_reasoning_effort = "medium"
```

Locate and back up user config, review settings, update if appropriate, validate TOML, and confirm Codex accepts it.

---

## 57. Exercise D — CLI Override

```powershell
codex -c 'model_reasoning_effort="high"'
```

Does this permanently edit `config.toml`? Why use a temporary override? What if the model does not support high effort?

---

## 58. Exercise E — Configuration Profiles

```toml
[profiles.routine]
model_reasoning_effort = "medium"

[profiles.deep_review]
model_reasoning_effort = "high"
```

Explain profiles, match tasks to each, try `--profile`, and verify that Codex accepts the settings.

---

## 59. Exercise F — Android Model Evaluation

Compare medium and high effort on a read-only analysis of GroupTrack login UI. Keep repository, prompt, instructions, and criteria consistent; make no edits. Evaluate accuracy, scope relevance, unnecessary recommendations, duration, and clarity.

---

## 60. Exercise G — Laravel Debugging Analysis

On a Laravel invalid-request issue, provide route and validation context, request read-only root-cause analysis, record findings, check them against source, and determine whether higher effort was useful.

---

## 61. Exercise H — Build a Model Selection Matrix

| Task Category | Starting Effort | Verification |
|---|---|---|
| Small UI changes | Low or medium | Layout/build checks |
| API validation | Medium | Feature tests |
| Complex debugging | High | Reproduction and regression tests |
| Architecture analysis | High | Evidence-based review |
| Code review | Medium or high | Diff and test inspection |

Customize this matrix for the projects you work on.

---

# Part XII — Knowledge Review

## 62. Knowledge Check

1. What is model selection?
2. Why does model choice matter?
3. What does the `model` key control?
4. What is a model identifier?
5. Why can availability differ?
6. What is reasoning effort?
7. What does `model_reasoning_effort` control?
8. How does model selection differ from reasoning effort?
9. What is low effort suitable for?
10. What is medium effort suitable for?
11. When might high effort help?
12. Does higher effort guarantee better code?
13. What is latency?
14. How can effort affect resource usage?
15. How does effort differ from context size?
16. How does effort differ from sandbox permissions?
17. What is a global model setting?
18. What is a CLI model override?
19. What does `-m` do?
20. What does `-c` do?
21. What is a configuration profile?
22. Why check model compatibility?
23. How can models be evaluated fairly?
24. Why include test results in evaluation?
25. Why is a clear prompt important regardless of effort?

---

## 63. Key Terminologies

| Term | Definition |
|---|---|
| Model Selection | Choosing the AI model used by Codex |
| Model Identifier | Technical name identifying a model |
| Reasoning Effort | Requested level of internal model reasoning |
| Latency | Time required to produce a result |
| Resource Usage | Compute, tokens, or applicable usage consumed |
| Model Capability | Tasks and features a model can support |
| Model Availability | Whether a model can be used in an environment |
| Global Configuration | User-level Codex settings |
| Project Configuration | Supported repository-specific settings |
| CLI Override | Temporary invocation-specific setting |
| Configuration Profile | Reusable named settings |
| Benchmark | Structured performance comparison |
| Regression Test | Test intended to catch reintroduced defects |
| Context Window | Model's context capacity |
| Sandbox | Execution restriction environment |
| Scope Compliance | Adherence to approved requirements |

---

## 64. Key Takeaways

1. **Model selection determines which AI model Codex uses.**
2. **Reasoning effort controls the requested depth of internal reasoning.**
3. **Model selection and reasoning effort are independent settings.**
4. **The `model` key specifies a supported model identifier.**
5. **The `model_reasoning_effort` key requests a supported level.**
6. **Not all models support all reasoning values.**
7. **Low effort can suit narrow tasks; medium is a practical routine starting point.**
8. **High effort can help with complex debugging and architecture analysis.**
9. **Higher effort does not guarantee correct code.**
10. **Higher effort may increase latency and resource usage.**
11. **Good context engineering remains essential.**
12. **CLI overrides allow temporary changes without editing global configuration.**
13. **Profiles support reusable configuration variants.**
14. **Global and project settings have different scopes.**
15. **Evaluate models using consistent tasks and objective criteria.**
16. **Build results, tests, and Git reviews remain necessary.**
17. **Use the lightest supported combination that reliably meets quality needs.**

---

## 65. Official References

### OpenAI Codex

1. [Codex Documentation](https://developers.openai.com/codex/)
2. [Codex Models](https://developers.openai.com/codex/models/)
3. [Codex Basic Configuration](https://developers.openai.com/codex/config-basic/)
4. [Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced/)
5. [Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
6. [Codex CLI Reference](https://developers.openai.com/codex/cli/reference/)
7. [Codex Security](https://developers.openai.com/codex/security/)

### OpenAI API — Supporting Concepts

8. [Model Catalog](https://developers.openai.com/api/docs/models)
9. [Reasoning Models Guide](https://developers.openai.com/api/docs/guides/reasoning)
10. [Model Selection Guide](https://developers.openai.com/api/docs/guides/model-selection)

The API references explain general concepts. API parameters, billing, and availability should not be assumed to apply identically to Codex CLI.

---

## 66. Lesson Completion Checklist

- [ ] Understand model selection, identifiers, and availability.
- [ ] Understand `model` and `model_reasoning_effort`.
- [ ] Differentiate model and reasoning settings.
- [ ] Understand low, medium, high, and higher supported effort.
- [ ] Understand latency and resource tradeoffs.
- [ ] Differentiate reasoning effort, context window, and permissions.
- [ ] Configure global reasoning defaults and project settings.
- [ ] Use CLI model and reasoning overrides.
- [ ] Understand configuration profiles.
- [ ] Choose settings based on complexity.
- [ ] Evaluate performance using real results.
- [ ] Recognize unsupported settings.
- [ ] Complete exercises and knowledge check.

---

## 67. Next Lesson

**[[04 - Sandbox and Approval Policies]]**

The next lesson covers sandbox fundamentals, read-only and workspace-write execution, full-access risks, approval policy, network and filesystem access, permission escalation, and secure agentic workflows.

---

*Phase 4 — Codex Configuration | Lesson 4.3*  
*Learning Principle: Match model capability and reasoning effort to task complexity, then verify results using real software engineering evidence.*
