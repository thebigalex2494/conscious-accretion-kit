# Architecture

Technical deep dive into the Conscious Accretion Kit's orchestration system.

## The 6-Phase Orchestration Protocol

Every task flows through six phases. Not every task needs all six -- simple tasks may compress phases 2-3 into one step. But the protocol ensures nothing is skipped accidentally.

```
  Request
     |
     v
 ┌────────────┐
 │  1. INTAKE  │  Classify intent, detect mode, filter context
 └─────┬──────┘
       v
 ┌────────────┐
 │  2. PLAN   │  Decompose task, assign models, build subtask graph
 └─────┬──────┘
       v
 ┌────────────┐
 │  3. EXECUTE │  Parallel dispatch: local models ($0) + cloud fallback
 └─────┬──────┘
       v
 ┌────────────┐
 │ 4. VALIDATE │  Deterministic checks → audit → QA → circuit breaker
 └─────┬──────┘
       v
 ┌────────────┐
 │ 5. AGGREGATE│  Synthesize results, executive summary
 └─────┬──────┘
       v
 ┌────────────┐
 │  6. HANDOFF │  Structured contracts, audit trail, context snapshot
 └────────────┘
```

### Phase 1: INTAKE

**Purpose**: Understand what's being asked before doing anything.

- Classify the intent (bug fix, feature, research, maintenance)
- Detect mode: **Autonomous** (user talking directly) or **Delegated** (another agent delegating)
- Filter context: which files, which project, which constraints apply

**Mode Router**:

| Signal | Mode | Behavior |
| ------ | ---- | -------- |
| Direct user interaction | Autonomous | Full 6-phase orchestration |
| Arrives via `claude -p` or API call | Delegated | Execute and report, no decomposition |
| User says "orchestrate with X" | Delegated | Defer to external orchestrator |

### Phase 2: PLAN

**Purpose**: Break the task into executable subtasks before writing any code.

- Read relevant code and documentation
- Identify files that need modification
- Create a subtask graph (which tasks depend on which)
- Assign models: complex reasoning → large model, boilerplate → small/local model
- Estimate scope: is this a 1-file fix or a multi-file refactor?

**Model Assignment Pattern**:

| Task Type | Recommended Model |
| --------- | ----------------- |
| Complex orchestration, architecture | Largest available (Opus, GPT-4) |
| Implementation, coding | Mid-tier (Sonnet, local coder) |
| Review, documentation | Smallest sufficient (Haiku, local chat) |
| ETL, data processing | Specialized local (Qwen Coder, DeepSeek) |

### Phase 3: EXECUTE

**Purpose**: Do the actual work.

- Independent subtasks run in parallel (as subagents or parallel tool calls)
- Dependent subtasks run sequentially with data contracts
- Local models ($0) are preferred when they can handle the task
- Cloud models are fallback for tasks requiring more capability

**Parallelization Rules**:
- If two subtasks don't share state → parallel
- If subtask B needs output from A → sequential
- Always aggregate results before presenting to user

### Phase 4: VALIDATE

**Purpose**: Verify the work before delivering it.

Four-step validation:
1. **Logical correctness** -- Does the output make sense?
2. **Request fulfillment** -- Does it answer what was asked?
3. **Edge cases** -- What could go wrong?
4. **Execution test** -- If code: does it compile/run?

**Circuit Breaker** (3 retries):

| Retry | Strategy |
| ----- | -------- |
| R1 | Same agent, same prompt (simple retry) |
| R2 | Same agent + error context (informed retry) |
| R3 | Upgrade to more capable model |
| Fail | Escalate to human with full context |

**Fast-fail override**: Destructive operations (git push, file delete) get only 2 retries before pausing for human confirmation.

### Phase 5: AGGREGATE

**Purpose**: Synthesize results into a coherent deliverable.

- Combine outputs from parallel subtasks
- Generate executive summary
- Translate to user's preferred language
- Format for the output medium (terminal, email, document)

### Phase 6: HANDOFF

**Purpose**: Ensure continuity for future sessions.

- Create audit trail of what changed and why
- Generate context snapshot for session resumption
- Commit changes if approved
- Update project registry with new state

## Dual-Mode Architecture

The system operates in two modes, automatically detected:

### Autonomous Mode

When you interact directly with your agent, it has full orchestration authority. It decides how to decompose tasks, which models to use, and when to validate.

### Delegated Mode

When your agent is invoked by another system (e.g., a Gemini CLI orchestrator calling `claude -p "task"`), it acts as a focused executor:
- Receives a specific task
- Executes precisely
- Reports result concisely
- Does NOT decompose or orchestrate

This enables multi-agent workflows where a high-level orchestrator delegates specific tasks to specialized agents.

## Quality Gates

### Grading Scale

| Grade | Score | Meaning |
| ----- | ----- | ------- |
| A+ | 95-100 | Exemplary -- production-ready, well-tested |
| A | 90-94 | Excellent -- minor improvements possible |
| B+ | 85-89 | Good -- some gaps to address |
| B | 80-84 | Acceptable -- needs targeted improvements |
| C+ | 70-79 | Below standard -- significant gaps |
| C | 60-69 | Poor -- major rework needed |
| D | 50-59 | Failing -- fundamental issues |
| F | <50 | Critical -- not viable |

Use this scale when auditing code, reviewing architectures, or assessing project health.

### System Improvement Protocol (SIP)

A structured approach to improving your system:

1. **Audit** -- Assess current state with grading scale
2. **Identify** -- Prioritize improvements by impact/effort ratio
3. **Propose** -- Present plan to human for approval
4. **Execute** -- Implement after approval
5. **Validate** -- Verify results against expected outcomes

## Device-Aware Execution

### Why Multi-Device Matters

If you work from multiple machines (desktop + laptop, office + home, etc.), your orchestration system needs to know where it's running. A GPU-equipped desktop can run local models; a lightweight laptop cannot.

### Device Detection Flow

```
Session Start
     |
     v
Read $COMPUTERNAME (or $HOSTNAME on Linux)
     |
     v
Lookup in .device-profiles.json
     |
     ├── Found → Load profile (capabilities, models, role)
     │
     └── Not found → Use fallback profile
```

### Profile Schema

```json
{
  "[HOSTNAME]": {
    "alias": "Human-friendly name",
    "role": "executor | supervisor",
    "capabilities": ["gpu", "ollama", "browser-mcp"],
    "ollama_models": {
      "primary": "model-name",
      "fallback": "smaller-model"
    }
  }
}
```

The agent reads this at session start and adapts:
- Which models to use for local inference
- Whether to attempt GPU-accelerated tasks
- What role to assume in multi-agent coordination

## Session System

### YAML-Based Context Loading

Sessions are defined as YAML files that pre-load context for focused agent work:

```yaml
session:
  id: "A1-2026-04-01"
  name: "Code Audit Sprint"
  status: pending
  priority: HIGH

project:
  name: "My Project"
  working_dir: "/path/to/project"

context_files:
  - path: "README.md"
    purpose: "Architecture overview"
  - path: "src/core/config.py"
    purpose: "Configuration system"

tasks:
  - id: T1
    name: "Audit configuration module"
    done_when: "Quality grade assigned"

rules:
  - "Read-only -- no modifications"
  - "Use quality gates grading scale"
```

### Why Sessions Matter

Sessions solve the "cold start" problem. Instead of explaining your project from scratch every conversation, you define a session file once. The agent loads it, reads the context files, and starts working immediately.

**Naming convention**: `YYYY-MM-DD_A{N}_{slug}.yaml`
- Date: when the session was created
- A{N}: agent number (for multi-agent sessions)
- Slug: short project identifier

## Hook System

### Safety Interceptors

Hooks execute shell commands before or after tool use:

```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "tools": ["Write", "Edit"],
      "command": "script that blocks writes to protected paths"
    },
    {
      "event": "PostToolUse",
      "tools": ["Bash"],
      "command": "script that blocks destructive commands"
    }
  ]
}
```

**Common hook patterns**:
- Block writes to `node_modules/`, `__pycache__/`, `.env`
- Block destructive commands (`rm -rf /`, `format`, `drop table`)
- Log all file modifications for audit trail
- Validate output format before delivery

## Multi-Model Routing

### Local-First Strategy

The framework encourages a **$0 local-first** approach:

1. Try local model (Ollama) first -- free, private, fast for simple tasks
2. If local fails or task is too complex -- escalate to cloud model
3. Use the smallest sufficient model for each subtask

### Router Architecture

```
Incoming Task
     |
     v
Classify complexity (simple / moderate / complex)
     |
     ├── Simple → Local model (llama3, phi, etc.)
     ├── Moderate → Mid-tier (Sonnet, local coder)
     └── Complex → Top-tier (Opus, GPT-4)
```

This is implemented in the optional `engine/` directory with provider abstractions and a classifier that routes tasks based on estimated complexity.
