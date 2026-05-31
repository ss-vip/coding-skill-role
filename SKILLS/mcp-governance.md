---
name: mcp-governance
description: MCP tool routing, memory policy, reasoning escalation control.
---
## MCP-GOVERNANCE

**PURPOSE**: Control memory, reasoning, external search, tool selection.

**TOOL & FUNCTION CALL PROTOCOL**:
- **Schema Strictness & Fallback**: NEVER hallucinate tools or parameters. Strictly adhere to the provided tool schema. If a specific tool is missing, DO NOT give up. Gracefully fallback to using OS-native shell commands (e.g., bash/powershell) or writing ad-hoc scripts (Python/Node.js) to accomplish the goal. *(All ad-hoc scripts MUST be written to `./temp/` and cleaned up after use)*.
- **Atomic & Sequential**: Do not chain dependent function calls in a single parallel payload (e.g., don't create a dir AND write a file inside it simultaneously). Wait for state verification.
- **Error Recovery**: If a tool returns an error, DO NOT blindly retry with the same parameters. You MUST read the error payload, analyze the failure, and adjust your parameters. Common recovery includes installing missing dependencies (e.g., `playwright install`, `npm install`, `pip install`). DO NOT assume dependencies auto-install — offer to run the required install command and retry.

**MEMORY RULE** (MCP context is expensive):
- **Store ONLY durable knowledge**: architecture decisions, stable facts. *(Example: "Using Next.js App Router", "API requires JWT auth")*
- **Never store (Use `./temp` instead)**: logs, runtime state. *(Example: "npm install output", "dev server running on port 3000")*
- **Knowledge Lifecycle**: Review MCP entities at session end. Remove obsolete observations, prune outdated architectural decisions. Keep knowledge graph lean.

**TOOL ROUTING & ESCALATION**:
- **Prefer order**: 1. direct reasoning → 2. memory (MCP) → 3. external search → 4. deep reasoning (last resort).
- **Escalation**: Use long-reasoning-mcp ONLY IF repeated failure or system-level ambiguity.

**COST CONTROL**: Avoid unnecessary MCP calls, reasoning escalation, or external search. Prefer direct execution.