# 06 — Caveman: Auto — Model Routing and Reasoning Effort

> [!abstract] Lesson Information
> **Repository:** [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)
>
> **Category:** Agentic Engineering / Model Routing / Reasoning Optimization / AI Cost Engineering
>
> **Difficulty:** Intermediate to Advanced
>
> **Learning Approach:** Theory First
>
> **Prerequisites:**
> - [[00 - Repository Overview and Purpose]]
> - [[01 - Architecture and Core Concepts]]
> - [[02 - Installation and Codex Integration]]
> - [[03 - Token Economics and Optimization]]
> - [[04 - Say Less - Output Token Optimization]]
> - [[05 - Read Less - Input and Context Compression]]
>
> **Primary Goal:** Understand how Caveman Auto selects models and reasoning effort, how routing affects cost and performance, and how to evaluate it without compromising correctness or overlooking data flow.

---

# 1. Learning Objectives

After completing this lesson, you should be able to:

1. Explain model routing.
2. Describe Caveman's Auto capability.
3. Distinguish model selection from reasoning-effort selection.
4. Understand why tasks need different reasoning resources.
5. Explain task-complexity estimation.
6. Understand the economic motivation for routing.
7. Calculate cost differences between routing strategies.
8. Interpret Caveman's published Auto benchmark.
9. Recognize benchmark limitations.
10. Understand Auto's account requirement.
11. Explain optional provider integration.
12. Identify authentication, privacy, and data-retention considerations.
13. Understand routing errors and recovery paths.
14. Evaluate cost, latency, and correctness together.
15. Design a controlled routing benchmark.
16. Apply routing concepts to Laravel, Next.js, and Android projects.
17. Identify when manual model selection may be preferable.
18. Evaluate Auto before broad adoption.

---

# Part I — Introduction to Auto

## 2. What Is Caveman Auto?

**Auto** is Caveman's model-routing capability. When selected in a supported coding agent's model picker, Caveman chooses a model and reasoning effort for each request.

Its basic idea is:

> Allocate enough reasoning resources for difficult tasks without automatically spending the same resources on every simple task.

### Example

**Request A**

~~~text
Rename userName to displayName in this component.
~~~

**Request B**

~~~text
Investigate a concurrency issue causing duplicate bus assignments.
~~~

Request B may require reading several modules, understanding transactions, and designing a regression test. Request A may be bounded and mechanical, depending on the surrounding code.

Auto aims to account for differences like these.

## 2.1 What Auto Does Not Guarantee

Auto does not guarantee:

- The selected model is correct for every request.
- Lower cost on every workload.
- Equal quality across all tasks.
- Correct implementation without tests.
- A replacement for project instructions or engineering judgment.

Routing selects resources; it does not define whether the result meets requirements.

---

# Part II — Understanding Model Routing

## 3. What Is Model Routing?

**Model routing** selects which language model should process a request. Depending on the system, routing may consider task characteristics, model availability, cost, expected latency, capabilities, or configuration.

~~~mermaid
flowchart TD
    A["Developer Request"] --> B["Model Router"]
    B --> C["Choose Model and Effort"]
    C --> D["Process Request"]
    D --> E["Verify Result"]
~~~

This is a conceptual model, not a verified description of Caveman's internal routing algorithm.

### Why Routing Exists

Models can differ in:

- Cost.
- Reasoning capability.
- Response speed.
- Context limits.
- Tool-use behavior.

Using the most expensive configuration for every request can be wasteful. Using the cheapest configuration for every request can increase failures and retries.

---

# Part III — Model Selection and Reasoning Effort

## 4. Two Related but Separate Decisions

A router may make two decisions:

**Model selection:** Which model processes the request?

**Reasoning effort:** How much reasoning effort is requested from a supported model?

| Dimension | Model Selection | Reasoning Effort |
|---|---|---|
| Decision | Choose a model | Choose an effort setting |
| Example | Model A or Model B | Low, medium, or high |
| Possible benefit | Match cost and capability | Avoid excess effort on simple requests |
| Main risk | Incompatible or insufficient model | Under-reasoning or unnecessary latency |
| Availability | Provider-dependent | Model/API-dependent |

Higher reasoning effort does not guarantee a better answer. It may increase cost, token usage, or latency. Provider settings are not standardized, so consult current model documentation.

---

# Part IV — Task Complexity

## 5. Why Complexity Matters

A short prompt may describe a difficult task:

~~~text
Fix the deadlock.
~~~

This could require understanding concurrent transactions, shared state, and database locking. Prompt length alone is a poor complexity measure.

Possible complexity signals include:

- Number of affected components.
- Dependency count.
- Ambiguity.
- Required reasoning depth.
- Architectural impact.
- Testing burden.
- Consequences of failure.

These are useful evaluation dimensions, not a verified list of Caveman's internal routing criteria.

| Task | Possible Complexity |
|---|---|
| Change a button label | Low |
| Adjust a responsive layout | Low to moderate |
| Fix validation behavior | Moderate |
| Refactor authentication | Moderate to high |
| Analyze a race condition | High |
| Redesign a distributed architecture | High |

Treat these classifications as hypotheses. Inspect the task and repository before deciding what reasoning it needs.

---

# Part V — Conceptual Routing Pipeline

## 6. Auto Workflow

~~~mermaid
flowchart TD
    A["Incoming Request"] --> B["Auto Routing Decision"]
    B --> C["Select Available Model"]
    C --> D["Select Effort Where Supported"]
    D --> E["Run Request"]
    E --> F["Inspect and Verify Outcome"]
~~~

The exact selection logic is implementation-specific. Verification remains the developer's responsibility.

A routing system can fail by:

- Underestimating complexity.
- Choosing a model without needed capabilities.
- Selecting too little effort.
- Adding routing latency.
- Routing through an unexpected provider.

A task that appears simple may span multiple layers or have high consequences.

---

# Part VI — The Economic Motivation

## 7. Hypothetical Cost Comparison

Assume two configurations:

| Configuration | Hypothetical Cost per Request |
|---|---:|
| Lower-cost | $0.02 |
| Higher-cost | $0.10 |

For 100 requests:

**Higher-cost configuration for every request**

\[
100 \times 0.10 = \$10.00
\]

**Route 70 requests to lower-cost and 30 to higher-cost**

\[
70(0.02) + 30(0.10) = \$4.40
\]

Under these assumptions, the modeled savings are $5.60, or 56%.

These prices are invented for teaching. The calculation assumes equivalent correctness and does not model retries, subscriptions, caching, or human review.

---

# Part VII — Caveman's Published Auto Benchmark

## 8. Repository-Reported Results

The current Caveman README reports this interim comparison:

| Configuration | Tasks Solved | Cost per Try | Median Time |
|---|---:|---:|---:|
| Opus 5.5, xhigh effort, every request | 44.3% | $7.83 | 35.2 min |
| Auto, Claude models only | 43.7% | $3.59 | 15.9 min |
| Auto, plus OpenAI and open models | 42.9% | $3.47 | 16.7 min |

The README says the evaluation used 85 coding tasks from SWE-Interact and SWE-Together, one attempt each. It reports Auto at about 54% lower cost than the Opus 5.5 xhigh comparison.

**Source:** [Caveman README — Auto benchmark](https://github.com/JuliusBrussee/caveman#auto-54-cheaper-same-score)

## 8.1 Benchmark Caveats

The repository itself says:

- The run was interim/half-complete.
- The observed score gap was too small to distinguish confidently at this sample size (the README reports uncertainty of roughly ±7 points).
- Most savings came from Auto lowering effort on easier requests.
- Savings against a moderate-effort baseline had not been measured in that report.

The results suggest possible savings compared with always using a high-cost, high-effort configuration. They do not establish equivalent quality for every task or workload.

### Statistical Caution

A small observed score difference is not proof of equivalence. A sound equivalence claim needs a suitable study design, enough observations, comparable tasks, and an explicit acceptable difference.

---

# Part VIII — Cost per Attempt and Cost per Success

## 9. Why Cost per Attempt Is Incomplete

A cheap attempt that fails often may cost more overall.

A useful metric is:

\[
\text{Cost per successful task}
=
\frac{\text{Total measured cost}}{\text{Number of successful tasks}}
\]

For a simplified fixed-cost model:

\[
\text{Expected cost per success}
\approx
\frac{\text{Cost per attempt}}{\text{Success probability}}
\]

Example:

- A: $0.08 per attempt, 80% success → modeled $0.10 per success.
- B: $0.04 per attempt, 50% success → modeled $0.08 per success.

This simplified calculation ignores task difficulty, retries, quality differences, human time, and consequences of failure. For consequential engineering work, those factors matter.

---

# Part IX — Latency and Quality

## 10. Measure End-to-End Work

A fast initial answer may require more debugging. Measure full task duration, including tool use and verification.

\[
T_{\text{total}} =
T_{\text{routing}} + T_{\text{model}} + T_{\text{tools}} + T_{\text{verification}}
\]

This sequential formula is only conceptual; some operations may overlap.

Evaluate routing across:

- Cost.
- Completion time.
- Success rate.
- Retry count.
- Test results.
- Human intervention.
- Quality of successful changes.

Do not optimize a single metric in isolation.

---

# Part X — Account, Providers, and Data Flow

## 11. Account and Provider Setup

The current Caveman README documents:

~~~powershell
caveman login
~~~

It describes this as enabling Auto through a free account. It also documents optional OpenAI provider setup:

~~~powershell
caveman providers add openai
~~~

These commands are documentation examples only; they were not run for this lesson. Provider options and setup may change, so check current official instructions before use.

Adding providers can expand model choices, but also adds credentials, configuration, and data-processing considerations.

## 11.1 Current README Data Disclosure

The current README says that, after the user signs in and selects Auto, each request sends Caveman Cloud:

- The latest ask, including context the agent attaches, such as CLAUDE.md, memory, or explicitly mentioned files.
- The previous ask.
- The end of the agent's last reply.
- Tool names, effort, selected agent headers, and previous request token counts.

It also says Caveman may retain Free-plan data to improve routing.

For requests routed to a Caveman Cloud model, the README says the full request—including system prompt, tool results, and code—passes through Caveman Cloud and is billed to the project's stored key. Requests sent directly to a provider or an added provider follow their corresponding path. The README documents caveman off routing and caveman providers cloud off as controls for these respective paths.

**Read the live disclosure before enabling Auto:** [Caveman README — data and routing details](https://github.com/JuliusBrussee/caveman#caveman-cloud)

Treat this disclosure as version-specific. Verify current behavior, retention, provider routing, and controls in the repository's [Security Policy](https://github.com/JuliusBrussee/caveman/blob/main/SECURITY.md) and documentation.

## 11.2 Privacy Review Checklist

Before enabling Auto in a project, determine:

1. What prompt and attached context is transmitted?
2. Which services and model providers receive requests?
3. Is request data retained, and for what purpose?
4. Which credentials are used and where are they stored?
5. Can routing or cloud-provider paths be disabled?
6. What happens when the routing service is unavailable?
7. Does the project's data policy allow these transfers?

Never commit API keys or authentication tokens. Do not assume that routing is entirely local; inspect the current data-flow documentation.

---

# Part XI — Codex and Manual Selection

## 12. Codex Integration

The current README lists Codex as a supported environment for Auto and describes it as an option in the model picker after setup. Exact behavior depends on Caveman and Codex versions, authentication, configured providers, and available model settings.

~~~mermaid
flowchart TD
    A["Developer"] --> B["Codex"]
    B --> C["Caveman Auto"]
    C --> D["Choose Model and Effort"]
    D --> E["Execute Task"]
    E --> F["Test and Review"]
~~~

Auto is convenient for mixed task complexity. Manual selection may be preferable for:

- Reproducible evaluations.
- Complex architecture work.
- Tasks requiring a specific model.
- Sensitive work with strict provider boundaries.
- Investigations where previous Auto routing failed.

Record model and routing choices in controlled experiments.

---

# Part XII — Controlled Evaluation

## 13. Benchmark Auto Fairly

Compare a fixed configuration with Auto using:

1. The same task set.
2. Equivalent repository snapshots.
3. The same acceptance criteria.
4. The same tools and permissions.
5. The same verification steps.
6. Multiple trials where practical.

Do not let one run inherit another run's code changes.

| Metric | Why Measure It |
|---|---|
| Cost per attempt | Direct usage cost |
| Cost per successful task | Useful economic outcome |
| Completion time | End-to-end speed |
| Success rate | Task correctness |
| Retry count | Hidden overhead |
| Test results | Implementation evidence |
| Model/effort distribution | Explains routing behavior |
| Human intervention | Captures review and repair effort |

### Benchmark Process

~~~mermaid
flowchart TD
    A["Define Task Set"] --> B["Run Fixed Baseline"]
    B --> C["Record Cost, Time, and Quality"]
    C --> D["Run Auto from Equivalent State"]
    D --> E["Record Same Metrics"]
    E --> F["Analyze Failures and Variability"]
    F --> G["Adopt, Adjust, or Revert"]
~~~

One trial is weak evidence. Report task mix, versions, sample size, and limitations.

---

# Part XIII — Project Scenarios

## 14. Laravel: GoPasig

| Task | Likely Reasoning Need |
|---|---|
| Change a button label | Low |
| Fix a validation rule | Moderate |
| Investigate N+1 queries | Moderate |
| Prove simultaneous dispatch requests cannot double-assign a bus | High |

Concurrency analysis may require inspecting transactions, locks, validation, and race-focused tests. Whatever model is selected, the same acceptance criteria and tests apply.

## 15. Next.js: E-Commerce

| Task | Possible Complexity |
|---|---|
| Adjust product-card spacing | Low |
| Fix form validation feedback | Moderate |
| Refactor authentication flow | Moderate to high |
| Diagnose checkout success despite a backend rejection | High |

A seemingly narrow issue may involve frontend state, API responses, asynchronous behavior, and backend validation.

## 16. Android: GroupTrack

| Task | Possible Complexity |
|---|---|
| Change a button label | Low |
| Fix RecyclerView updates after insertion | Moderate |
| Diagnose inconsistent Firebase membership state | High |

Higher reasoning effort should help investigate the actual issue; it does not justify adding unnecessary architecture or expanding the MVP.

---

# Part XIV — Common Mistakes

## 17. Auto Anti-Patterns

| Mistake | Why It Fails |
|---|---|
| Always use the cheapest model | May increase failures and retries |
| Always request maximum effort | Can waste resources |
| Estimate complexity from prompt length | Brief tasks can hide deep issues |
| Ignore retries | Understates actual cost |
| Treat one benchmark as universal | Workloads differ |
| Treat similar scores as proven equivalence | Statistical uncertainty remains |
| Ignore provider and retention paths | Misses privacy implications |
| Route without verification | Incorrect output can pass unnoticed |
| Change models during a benchmark without recording it | Hurts reproducibility |
| Treat Auto as engineering judgment | Routing cannot establish correctness |

---

# Part XV — Practical Exercises

## 18. Exercise 1 — Task Classification

Classify each as relatively low, moderate, or high complexity and explain why:

1. Rename a CSS class.
2. Fix a Laravel validation error.
3. Analyze a database deadlock.
4. Update an Android button label.
5. Investigate inconsistent authentication state.
6. Design a concurrency-safe dispatch workflow.

## 19. Exercise 2 — Cost Calculation

Assume hypothetical costs:

- Lower-cost configuration: $0.03 per request.
- Higher-cost configuration: $0.12 per request.
- 80 total requests.
- Auto routes 50 to the lower-cost option and 30 to the higher-cost option.

Calculate the all-high cost, routed cost, and percentage savings. State why this is not proof of equal quality.

## 20. Exercise 3 — Cost per Success

Configuration A costs $0.08 per attempt and succeeds 80% of the time. Configuration B costs $0.04 per attempt and succeeds 50% of the time.

Calculate simplified expected cost per success. Identify omitted factors such as task mix, retries, and human correction.

## 21. Exercise 4 — Benchmark Interpretation

The README reports 44.3% solved for a fixed high-effort baseline and 43.7% for Claude-only Auto.

Explain why this small difference does not prove exact equivalence. Include sample size and uncertainty.

## 22. Exercise 5 — Routing Failure

Auto selects a low-effort setting for a concurrency bug, and the fix fails under simultaneous requests.

Describe:
- Why routing may have underestimated the task.
- How to investigate and recover.
- Which regression tests are needed.

## 23. Exercise 6 — Privacy Assessment

Use the current README's data disclosure to create a checklist for a project with sensitive source code. Identify the request context, provider path, retention policy, credentials, and controls you would verify before enabling Auto.

## 24. Exercise 7 — Benchmark Design

Design an evaluation using one Laravel UI task, one query-optimization task, and one transaction-concurrency task. Specify equivalent starting states, quality criteria, cost/time metrics, and limitations.

---

# Part XVI — Knowledge Check

## 25. Review Questions

1. What is Caveman Auto?
2. What is model routing?
3. What is reasoning effort?
4. How do model choice and effort differ?
5. Why do coding tasks need different resources?
6. Why is prompt length a poor complexity measure?
7. How can routing reduce cost?
8. How can routing increase total cost?
9. What is cost per successful task?
10. What did the README's Auto benchmark report?
11. Why is the result interim?
12. Why is a score difference not proof of equivalence?
13. What account setup does the README document?
14. What optional provider command does it document?
15. What data does the current README say Auto sends for routing?
16. When does the README say a full request can pass through Caveman Cloud?
17. What controls does the README document for routing/cloud-provider paths?
18. When might manual model selection be preferable?
19. What metrics belong in a routing benchmark?
20. What makes routing successful?

---

# Part XVII — Key Takeaways

> [!summary]
> - Auto chooses a model and reasoning effort for requests in supported agents.
> - Model selection and reasoning-effort selection are distinct.
> - Short prompts can describe complex work.
> - The current README reports 85 tasks, one try each, and a 54% lower-cost comparison against Opus 5.5 at xhigh; the result is interim and uncertainty is substantial.
> - Savings depend on the baseline; the README says savings against medium effort were not measured there.
> - Cost per successful task is more useful than cost per attempt alone.
> - Auto requires sign-in according to the current README.
> - Optional providers add configuration and data-flow considerations.
> - The current README discloses prompt/context and metadata sent for routing, possible Free-plan retention, and full-request routing through Caveman Cloud models.
> - Check the live security and privacy disclosures before using Auto with sensitive project data.
> - Manual selection remains useful for reproducibility, sensitive work, and difficult investigations.
> - Correctness, scope, testing, and acceptance criteria do not change when routing changes.

---

# Part XVIII — Final Mental Model

~~~mermaid
flowchart TD
    A["Developer Task"] --> B["Codex Agent"]
    B --> C["Caveman Auto"]
    C --> D["Model Selection"]
    C --> E["Effort Selection"]
    D --> F["Model Execution"]
    E --> F
    F --> G["Implementation"]
    G --> H["Verification"]
    H --> I["Evaluate Quality, Cost, and Time"]
    C -. "Version-specific data flow" .-> J["Caveman Cloud / Provider"]
~~~

> [!important] Final Principle
> **Auto's goal is to allocate reasoning resources efficiently. The developer still verifies quality and should understand where request data flows.**

---

# Part XIX — Official References

## Caveman

- [Official Repository](https://github.com/JuliusBrussee/caveman)
- [README — Auto benchmark and setup](https://github.com/JuliusBrussee/caveman#auto-54-cheaper-same-score)
- [README — Data and routing disclosure](https://github.com/JuliusBrussee/caveman#caveman-cloud)
- [Installation Guide](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- [Security Policy](https://github.com/JuliusBrussee/caveman/blob/main/SECURITY.md)

## OpenAI and Codex

- [OpenAI Codex Documentation](https://developers.openai.com/codex/)
- [OpenAI Reasoning Guide](https://developers.openai.com/api/docs/guides/reasoning)
- [OpenAI API Pricing](https://openai.com/api/pricing/)
- [OpenAI Models](https://platform.openai.com/docs/models)

## Evaluation

- [SWE-bench](https://www.swebench.com/)
- [Git Documentation](https://git-scm.com/doc)

> [!note] Documentation Scope
> This lesson reflects the Caveman README as reviewed on October 21, 2026. The benchmark table and data-flow statements are repository-reported, not independently reproduced or audited here.
>
> Cost examples and architecture diagrams are educational illustrations. Auto's exact routing algorithm and provider behavior can change; consult the installed version's documentation and security disclosures.
>
> No account setup, installation, routing, or benchmark execution was performed.

---

# Part XX — Next Lesson

**[[07 - Blocks - Reusable Agent Scripts]]**

Topics:

1. Introduction to Blocks.
2. Code mode and reusable execution.
3. Why agents repeatedly generate scripts.
4. Blocks architecture.
5. Repository-local script reuse.
6. Built-in Blocks.
7. json-peek and jsonl-stats.
8. test-summary and first-error.
9. wait-for and grep-defs.
10. http-json and replace-in-file.
11. Blocks discovery and indexing.
12. Integration with AGENTS.md.
13. Script security and testing.
14. Token savings and benchmarks.
15. Codex workflows.
16. Practical exercises and official references.

**End of Lesson 06 — Auto: Model Routing and Reasoning Effort.**
