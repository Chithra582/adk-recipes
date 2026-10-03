# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **adk-recipes** (`adk-recipes`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** adk-recipes (`adk-recipes`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Agent Development Kit Recipes & Scaffolding  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The agent operates via a strictly disciplined, 5-stage deterministic execution pipeline enforcing safety validation, recipe scaffolding, and verified state transitions.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic ADK Recipe Pipeline                          |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Intent Ingestion & Requirement Resolution Gate]                        |
|     --> Ingest developer requirements, target SDK language, & architectural tier  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Recipe Matching & Catalog Resolution]                                  |
|     --> Match intent against canonical core patterns or contrib solutions         |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Guardrail & Policy Safety Evaluation]                                  |
|     --> Filter inputs against safety policies; check injection & data constraints |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Execution, Tool Invocation & State Mutation]                           |
|     --> Execute recipe tools; manage session checkpoints; capture return metrics  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Output Verification & Manifest Validation]                             |
|     --> Validate response grounding, verify unit test pass rate, & finalize turn  |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations

Recipe selection affinity across candidate recipes $r \in R$ for a user goal $q$ is determined by a multi-attribute evaluation function:

$$S_{\text{recipe}}(r) = w_1 \cdot \text{PatternFit}(r, q) + w_2 \cdot \text{LanguageMatch}(r, q) + w_3 \cdot \text{ComplexityWeight}(r)$$

Where:
- $w_1 = 0.50$: Semantic relevance between user requirements and recipe intent.
- $w_2 = 0.30$: Strict matching of programming language runtime (Python, TypeScript, Go).
- $w_3 = 0.20$: Complexity fit favoring focused core recipes over overly complex multi-tier stacks for basic tasks.

Safety guardrail compliance $G_{\text{safety}}(x)$ for content $x$ enforces minimum safety thresholds:

$$G_{\text{safety}}(x) = 1.0 - \max_{c \in C} \text{ToxicityScore}(x, c)$$

Where content is approved only when $G_{\text{safety}}(x) \ge 0.90$.

### 3. Thresholding & Refusal Decision Criteria

adk-recipes enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on ERR_GUARDRAIL_POLICY_VIOLATION**: Safety Guardrail Breach (Toxicity or injection score > 0.10) halts execution with code `ERR_GUARDRAIL_POLICY_VIOLATION`.
- **Refusal on ERR_INVALID_RECIPE_MANIFEST**: Manifest Schema Mismatch (Schema validation failure) halts execution with code `ERR_INVALID_RECIPE_MANIFEST`.
- **Refusal on ERR_UNAUTHORIZED_TOOL_INVOCATION**: Unauthorized Tool Action (Invocation of unlisted tool) halts execution with code `ERR_UNAUTHORIZED_TOOL_INVOCATION`.
- **Refusal on ERR_EXECUTION_TIMEOUT**: Execution Latency (Execution time > 120 s) halts execution with code `ERR_EXECUTION_TIMEOUT`.
- **Refusal on ERR_SESSION_STATE_CORRUPTION**: Session State Desync (Corrupted memory checkpoint hash) halts execution with code `ERR_SESSION_STATE_CORRUPTION`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Tier 1 (Automated Tool Retry & Backoff)**: Transient network errors or brief API rate limits trigger up to 3 automatic retries with exponential backoff before failing.
- **Tier 2 (Recipe Downgrade & Alternative Suggestion)**: If a community recipe fails due to missing dependencies, the engine automatically recommends a simpler canonical core recipe.
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Tier 3 (Interactive Operator Governance)**: Destructive file modifications or highstakes API invocations halt execution and request affirmative confirmation from the developer.
- **Benchmark Trajectory Auditing**: Operators inspect evaluation traces, raw generation tokens, and container logs to verify scoring fidelity.

---

## The Data It Uses

adk-recipes operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Developer Instructions**: Natural language prompts describing desired agent functionality.
- **Recipe Manifests**: JSON metadata declaring supported SDKs, dependencies, and entrypoints.
- **Telemetry & Traces**: Unit test logs, validator outputs, and session state checkpoints.

### 2. Configuration & Reference Data

- **Agent Development Kit (ADK)**: Open-source agent framework specifications.
- **Model Context Protocol (MCP)**: JSON-RPC 2.0 interoperability standard for tools.
- **OAuth 2.0 / OpenID Connect**: Secure identity delegation standards.

### 3. Base Model & Inference Lineage

- **Target LLM Engines**: Google Gemini 1.5 Pro / Flash, Gemini 2.0, Anthropic Claude, OpenAI GPT-4o.
- **Runtime Environment**: Python 3.10+, uv/pip, Node.js/TypeScript, Go 1.22+.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of adk-recipes is essential for effective deployment.

### 1. Upstream ADK Version Drift Across Multi-Language SDKs
- **Limitation**: Asynchronous release schedules between ADK Python, TypeScript, and Go can cause minor feature disparity.
- **Mitigation**: Maintain explicit SDK compatibility version tags in recipe manifests and validate in CI.

### 2. Context Window Consumption During Lengthy Multi-Turn Sessions
- **Limitation**: Extensive conversational history can accumulate tokens and degrade reasoning quality.
- **Mitigation**: Implement automated sliding-window truncation and summary checkpointing.

### 3. Rate Limits on Upstream LLM Endpoints During High Concurrency
- **Limitation**: Running parallel evaluation benchmarks can trigger upstream API rate limits.
- **Mitigation**: Enforce concurrency throttling and automatic backoff with jitter.

### 4. Transient OAuth Expiration in Long-Running Background Workflows
- **Limitation**: User authorization tokens can expire during extended agent operations.
- **Mitigation**: Implement automatic refresh token rotation and prompt for re-authentication gracefully.

### 5. Varied Sandbox Isolation Levels Across Development Environments
- **Limitation**: Differences in host operating systems can impact local file and network sandboxing behavior.
- **Mitigation**: Provide standardized Docker container configurations for uniform sandbox execution.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Upstream ADK Version Drift Across Multi-Language SDKs | Section 1 | Verified |
| - Context Window Consumption During Lengthy Multi-Turn Sessions | Section 2 | Verified |
| - Rate Limits on Upstream LLM Endpoints During High Concurrency | Section 3 | Verified |
| - Transient OAuth Expiration in Long-Running Background Workflows | Section 4 | Verified |
| - Varied Sandbox Isolation Levels Across Development Environments | Section 5 | Verified |
