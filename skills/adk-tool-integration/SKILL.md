---
name: adk-tool-integration
description: Use when authoring custom tools, OAuth authentication flows, or external API bridges for ADK agent applications.
---

# ADK Tool Integration

## Overview
Guides the creation, typing, and registration of custom tools, authentication providers, and Model Context Protocol (MCP) clients into ADK agents.

## When to Use
- When binding external APIs, databases, or cloud services into the agent execution loop.
- When implementing OAuth2 user authorization flows for personalized agent actions.
- When adapting existing Python or TypeScript functions into typed tool definitions.

## Core Capabilities
1. **Schema Generation**: Automatically extracts JSON schemas from function signatures and type annotations.
2. **Authentication Injection**: Attaches secure bearer tokens and API keys at execution time without prompt leakage.
3. **Structured Response Formatting**: Standardizes tool return values into clean, digestible context turns for LLMs.
