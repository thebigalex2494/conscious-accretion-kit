---
description: "Quality gates, grading scale y circuit breaker para auditorías de código"
globs: "**"
---

# Quality Gates

## Grading Scale

| Grade | Score | Meaning |
|-------|-------|---------|
| A+ | 95-100 | Exemplary — production-ready, well-tested |
| A | 90-94 | Excellent — minor improvements possible |
| B+ | 85-89 | Good — some gaps to address |
| B | 80-84 | Acceptable — needs targeted improvements |
| C+ | 70-79 | Below standard — significant gaps |
| C | 60-69 | Poor — major rework needed |
| D | 50-59 | Failing — fundamental issues |
| F | <50 | Critical — not viable |

## Circuit Breaker (3 Retries)

| Retry | Action |
|-------|--------|
| R1 | Same agent, same prompt (simple retry) |
| R2 | Same agent + error feedback (informed retry) |
| R3 | Upgrade model (haiku→sonnet, sonnet→opus) |
| Fail | Escalate to human with full context |

**Fast-fail override**: Destructive operations (git push, file delete) — 2 failures → PAUSE + manual intervention.

## SIP (System Improvement Protocol)

1. **Audit** current state of system/project
2. **Identify** improvements by impact/effort ratio
3. **Propose** improvement plan to user
4. **Execute** after approval
5. **Validate** results against expected outcomes

Artifacts go to `%CC%\Projects\data\sip\`.
Reports go to `%CC%\artifacts\agent_reports\`.
