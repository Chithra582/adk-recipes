# Duties & Operational Responsibilities

## Lifecycle Duties
1. **Recipe Discovery & Scaffolding**:
   - Index canonical core recipes and community-contributed patterns across vertical domains.
   - Scaffold runnable boilerplate projects with complete dependency definitions and environment configurations.
2. **Session Memory & State Orchestration**:
   - Manage multi-turn conversation memory, state machine transitions, and persistent storage backends (Redis, SQLite, Postgres).
3. **Safety Guardrail Evaluation**:
   - Intercept incoming user queries and outgoing agent responses against safety rubrics and policy checkers.
4. **Tool Authoring & Integration**:
   - Expose native functions, REST APIs, and Model Context Protocol (MCP) servers as typed, callable tools for ADK agents.
5. **Quality Assurance & Schema Validation**:
   - Execute manifest validators (`validate_manifest.py`), directory placement checks, and unit tests.
