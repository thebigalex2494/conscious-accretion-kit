---
description: "Protocolo de orquestación de 6 fases y mode routing para Central Command"
globs: "CLAUDE.md,AGENTS.md,.central-agent/**"
---

# Orchestration Protocol

## 6-Phase Flow

| Phase | Name | Description | Owner |
|-------|------|-------------|-------|
| 1 | **INTAKE** | Intent classification, context filtering, mode routing | Orchestrator |
| 2 | **PLAN** | Task decomposition, subtask graph, model assignment | Orchestrator + Opus |
| 3 | **EXECUTE** | Parallel execution via multi-agent dispatch | Multi-agent |
| 4 | **VALIDATE** | Deterministic checks → audit → QA → circuit breaker | DeepSeek + Haiku |
| 5 | **AGGREGATE** | Synthesis → executive summary → Español MX output | Sonnet |
| 6 | **HANDOFF** | Structured contracts, audit trail, context snapshot | contracts.py |

## Mode Router (Phase 1 — INTAKE)

| Condition | Active Mode |
|-----------|-------------|
| User interacts directly | **Autonomous Mode** — full 6-phase orchestration |
| Task arrives via `claude -p` from Gemini CLI | **Delegated Mode** — focused executor |
| User says "orquesta con Gemini" | **Delegated Mode** |

## Auto-QA Protocol (Phase 4)

Before delivering any significant result:
1. Verify logical correctness of output
2. Confirm it fulfills the request
3. Review edge cases and potential errors
4. If code: verify it compiles/executes correctly

## Task Decomposition Pattern

```
1. [INTAKE]     Classify intent, determine scope
2. [PLAN]       Read code, identify files, create subtask graph
3. [EXECUTE]    Apply changes (Sonnet impl, Haiku review, Qwen ETL)
4. [VALIDATE]   Tests pass, logic validated, circuit breaker OK
5. [AGGREGATE]  Summary with justification (Español MX)
6. [HANDOFF]    Audit trail, context snapshot, commit if approved
```

## Parallelization Rules
- Independent subtasks → dispatch as parallel subagents
- Dependent subtasks → sequential with data contracts
- Always aggregate results before presenting to user
