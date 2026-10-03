---
name: session-memory-state
description: Use when configuring multi-turn conversation memory, session state machines, and checkpoint persistence for ADK agents.
---

# Session Memory & State

## Overview
Architects persistent, crash-resilient session memory and state management for multi-turn conversational agents.

## When to Use
- When agents need to maintain context across multiple turns or sessions.
- When configuring storage adapters (in-memory, Redis, SQLite, Postgres).
- When implementing checkpointing to recover agent execution after process termination.

## Core Capabilities
1. **Multi-Turn Context Pruning**: Manages window sizes, summarization buffers, and sliding memory.
2. **State Machine Guards**: Validates state transitions between conversational phases.
3. **Pluggable Persistence**: Connects ADK memory abstractions to production databases.
