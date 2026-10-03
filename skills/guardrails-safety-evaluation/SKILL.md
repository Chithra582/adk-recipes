---
name: guardrails-safety-evaluation
description: Use when enforcing safety policies, content filtering, prompt injection defense, and output verification on ADK agents.
---

# Guardrails & Safety Evaluation

## Overview
Enforces comprehensive safety boundaries across agent inputs, model inferences, and tool executions.

## When to Use
- When deploying agents that process untrusted user input or external web data.
- When guarding against prompt injection, jailbreaks, or data exfiltration attempts.
- When validating that generated responses meet corporate tone, privacy, and compliance guidelines.

## Core Capabilities
1. **Pre-Execution Input Filtering**: Scans user prompts for toxic content, PII, and adversarial injection patterns.
2. **Post-Generation Verification**: Verifies factual grounding and format adherence before emitting responses.
3. **Tool Call Interception**: Prevents unauthorized destructive tool actions using policy rules.
