# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Trae Software Engineering Agent** (`trae-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Trae Software Engineering Agent (`trae-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Autonomous Software Engineering & Trajectory Analysis  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The Trae Software Engineering Agent is an autonomous software engineering platform engineered to resolve complex development tasks, debug defects, implement features, and conduct rigorous trajectory analysis. Built upon ByteDance's research-friendly architecture, the agent pairs multi-LLM reasoning with a rich tool ecosystem, surgical code editing, and real-time Lakeview summarization.

### 1. Decision Architecture

The software engineering, codebase exploration, and patch verification pipeline operates across a deterministic, five-stage architecture:

```
Developer Directive / Issue Trigger (GitHub Issue / Bug Report / Feature Request / Traceback)
    │
    ▼
[Stage 1: Intent & Issue Ingestion]
    │  - Decomposes problem statement and extracts technical constraints
    │  - Identifies target repository files, stack frames, and acceptance criteria
    │  - Initializes Lakeview trajectory logging store
    ▼
[Stage 2: Codebase Exploration & Symbol Mapping]
    │  - Executes targeted AST symbol resolution, ripgrep queries, and file-tree exploration
    │  - Bounds file reads to focused line windows (<150 lines) to prevent context saturation
    │  - Constructs dependency graphs between call sites and test suites
    ▼
[Stage 3: Sequential Hypothesis & Surgical Patch Synthesis]
    │  - Formulates step-by-step hypothesis using sequential thinking loops
    │  - Synthesizes atomic line-replacement diffs targeting only the defective logic blocks
    │  - Preserves surrounding indentation, coding conventions, and type annotations
    ▼
[Stage 4: Test Execution & Lint Verification]
    │  - Runs test runners (pytest, jest, cargo test) in bounded execution sandboxes
    │  - Parses test failures and compiler error tracebacks
    │  - Iterates through corrective patch loops if assertions fail (up to 3 cycles)
    ▼
[Stage 5: Lakeview Digest & Trajectory Archive]
    │  - Generates human-readable Lakeview step-by-step summaries
    │  - Scrubs private user paths, tokens, and credentials from trajectory logs
    │  - Packages verified git patch artifacts for human developer sign-off
    ▼
Validated Software Engineering Patch & Auditable Lakeview Trajectory Record
```

### 2. Decision Logic & Patch Verification Formulations

Trae evaluates code localization, patch safety, and test confidence using deterministic mathematical models:

1. **Symbol Localization Relevance ($R_{\text{loc}}$)**:
   $$R_{\text{loc}} = (w_s \cdot S_{\text{stack}}) + (w_t \cdot T_{\text{token}}) + (w_c \cdot C_{\text{callgraph}})$$
   where:
   - $S_{\text{stack}} \in [0, 1]$ represents direct traceback stack frame presence.
   - $T_{\text{token}} \in [0, 1]$ represents BM25 term frequency-inverse document frequency keyword matching.
   - $C_{\text{callgraph}} \in [0, 1]$ represents static call graph proximity to the failing assertion.
   - Weights: $w_s = 0.50, w_t = 0.25, w_c = 0.25$ ($\sum w_i = 1.0$).

2. **Patch Confidence Index ($I_{\text{patch}}$)**:
   $$I_{\text{patch}} = \frac{1}{3} \left( T_{\text{pass}} + L_{\text{lint}} + M_{\text{diff}} \right)$$
   where $T_{\text{pass}} \in \{0, 1\}$ indicates test suite success, $L_{\text{lint}} \in [0, 1]$ measures zero new linter errors, and $M_{\text{diff}} \in [0, 1]$ penalizes overly broad non-surgical edits. Delivery requires $I_{\text{patch}} \ge 0.90$.

### 3. Thresholding & Refusal Decision Criteria

Trae Software Engineering Agent enforces strict operational safety and integrity boundaries:
- **Refusal to Execute Destructive Host Commands**: Arbitrary host system shell commands (`rm -rf /`, formatting disks, killing unrelated system processes) are deterministically rejected with code `ERR_DESTRUCTIVE_COMMAND_PROHIBITED`.
- **Refusal to Bypass Security Filters**: Instructions to disable security linters, mock passing tests artificially, or bypass authentication routines are rejected (`ERR_SECURITY_BYPASS_REFUSED`).
- **Turn Ceiling Enforcement**: Autonomous coding iterations enforce a hard ceiling of `max_turns: 25` to eliminate runaway token exhaustion (`WARN_TURN_BUDGET_REACHED`).
- **Local Repository Isolation**: File modifications are restricted strictly to the workspace directory; modifications targeting system root or parent directories are blocked (`ERR_UNAUTHORIZED_DIRECTORY_TRAVERSAL`).

### 4. Fallback Decision Mechanism

Continuous engineering assistance is maintained through multi-tier fault recovery:
- **Model Cascade Failover**: When the primary foundation model experiences latency spikes or HTTP 429 rate limits, the orchestrator cascades automatically between `claude-3-5-sonnet`, `gpt-4o`, and `gemini-2.0-flash`.
- **Deterministic Static AST Fallback**: If LLM-guided exploration fails to localize a defect, the system falls back to static AST call-hierarchy trees and git-blame history.
- **Graceful Patch Reversion**: If a synthesized patch introduces regression failures that cannot be resolved within 3 turns, the agent automatically reverts the diff to a clean working state.

### 5. Human-in-the-Loop Governance

Human software engineers retain full oversight and final commit authority:
- **Explicit Operator Approval Gates**: Creating git commits, pushing branches, or modifying build configurations requires explicit human confirmation.
- **Immediate Execution Cancellation**: Operators can halt agent execution loops instantly via `Ctrl+C` interrupt signals.
- **Transparent Lakeview Summaries**: Lakeview provides concise, human-readable explanations of every decision, tool call, and file change before commits are finalized.

---

## The Data It Uses

Trae Software Engineering Agent operates under strict privacy, data minimization, and local workspace isolation standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill software engineering tasks:
- **Task Directives & Issue Descriptions**: Natural language problem descriptions, user bug reports, and stack traces.
- **Repository Source Code**: Local source files, test suites, and configuration manifests explicitly scoped to the repository.
- **Compiler & Linter Logs**: Output streams, error tracebacks, and assertion messages from local test runners.

### 2. Configuration & Reference Data

- **Lakeview Schema Configurations**: Event taxonomy schemas for tool call logging, observation recording, and digest generation.
- **Language Linter Schemas**: Pre-configured rulesets for flake8, black, ESLint, TypeScript, and pytest.
- **Multi-LLM Adapter Profiles**: Parameter templates, context window boundaries, and cost matrices across supported model providers.

### 3. Base Model & Inference Lineage

- **Deterministic Algorithmic Engines**: Ripgrep symbol indexing, AST parsing, subprocess sandbox execution, and git diff generators executed natively (100% deterministic with zero LLM variance).
- **Foundation LLMs**: High-capability frontier models (`claude-3-5-sonnet`, `gpt-4o`, `gemini-2.0-flash`) utilized for complex code reasoning, defect hypothesis, and surgical patch generation.
- **Zero Training on Proprietary Codebases**: User source code, local repositories, and debugging transcripts are never transmitted to external cloud training corpora.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Systematically protected against prompt injection, insecure output handling, and excessive authority.
- **Local-Only Trajectory Storage**: All Lakeview trajectory logs, step summaries, and diff artifacts reside exclusively on the user's filesystem.
- **Automated PII & Secret Scrubbing**: Environment variables, authentication keys, and user credentials are scrubbed from generation logs.
- **Zero Commercial Monetization**: Developer repositories, issue descriptions, and patch histories are never monetized, aggregated, or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of Trae Software Engineering Agent is essential for effective engineering deployment.

### 1. Large-Scale Multi-Repo Distributed Refactors
- **Limitation**: While highly proficient at surgical fixes within a single repository, coordinating simultaneous cross-repo architectural refactors exceeds single-session bounds.
- **Mitigation**: The agent focuses on intra-repo modularity and outputs standardized interface contracts for dependent repositories.

### 2. Graphical UI and Visual Rendering Defects
- **Limitation**: The agent operates on headless code, ASTs, and terminal outputs, lacking visual rendering pipelines to detect subtle CSS pixel misalignment.
- **Mitigation**: Trae relies on DOM snapshot assertions, visual regression test suites, and human developer review for visual interface polish.

### 3. Proprietary Binary Dependencies & Custom Hardware
- **Limitation**: Codebases dependent on closed-source binary drivers or proprietary hardware accelerators cannot be executed in standard headless test environments.
- **Mitigation**: The agent authors isolated mock interfaces and stubs to test algorithmic logic independently of physical hardware.

### 4. Flaky and Non-Deterministic Test Suites
- **Limitation**: Test suites exhibiting intermittent timing failures or network flakiness can provide confusing signals to automated patch validation loops.
- **Mitigation**: Trae runs regression tests multiple times and isolates known flaky test cases from patch verification criteria.

### 5. Ambiguous or Contradictory Requirements
- **Limitation**: Bug reports with contradictory descriptions can lead the agent to explore multiple plausible but mutually exclusive hypotheses.
- **Mitigation**: The agent explicitly lists conflicting interpretations and pauses execution to request developer clarification.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & patch verification formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested task directives, source code & compiler logs | Section 1 | Verified |
| - Configuration, Lakeview schemas & linter profiles | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Large-scale multi-repo distributed refactors | Section 1 | Verified |
| - Graphical UI and visual rendering defects | Section 2 | Verified |
| - Proprietary binary dependencies & custom hardware | Section 3 | Verified |
| - Flaky and non-deterministic test suites | Section 4 | Verified |
| - Ambiguous or contradictory requirements | Section 5 | Verified |
