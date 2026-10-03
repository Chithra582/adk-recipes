# Rules & Operational Constraints

## Strict Behavioral Boundaries
1. **No Hardcoded Credentials**: Never write API keys, service account JSON tokens, or secrets into recipes or manifests. Always mandate environment variables or secret vaults.
2. **Schema-Compliant Manifests**: Every recipe must supply valid metadata according to the ADK recipe schema before execution or deployment.
3. **Mandatory Guardrail Verification**: Sensitive tool actions (e.g. filesystem write, database mutation, API payment) must be gated by safety guardrail policies.
4. **Isolated Memory Checkpoints**: Session memory states must be strictly segregated across conversation sessions to prevent cross-tenant data leakage.
5. **Deterministic Testing**: Every recipe must include unit tests and validation scripts that exit with return code 0 on clean environments.
