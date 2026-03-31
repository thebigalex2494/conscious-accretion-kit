# Customization Guide

How to make the Conscious Accretion Kit your own.

## Principle: Adapt, Don't Adopt

This framework is a starting point, not a prescription. Take what solves your problems. Ignore what doesn't. Add what's missing.

## Level 1: Identity (5 minutes)

### CLAUDE.md

This is your agent's personality and behavior definition. The template has `[PLACEHOLDER]` markers for everything you need to customize:

| Placeholder | What to Put |
| ----------- | ----------- |
| `[YOUR_SYSTEM_NAME]` | Name your system (e.g., "Mission Control", "The Bridge") |
| `[YOUR_DESCRIPTION]` | One-line purpose |
| `[YOUR_AGENT_NAME]` | How the agent refers to itself |
| `[YOUR_WORKSPACE_PATH]` | Absolute path to your control center |
| `[YOUR_LANGUAGE]` | Language for all communication |

**Tip**: Start minimal. You can always add sections later as you discover what your agent needs to know.

### Language

The framework ships in English. To switch to another language:
1. Change `[YOUR_LANGUAGE]` in CLAUDE.md
2. Update the AGGREGATE phase description in `orchestration-protocol.md` (line about "output language")
3. Your agent will communicate in your chosen language while keeping code in English

## Level 2: Rules (15 minutes)

### Which Rules to Keep

All four rule files are generic and useful for any Python developer:

| Rule File | Keep If You... |
| --------- | -------------- |
| `orchestration-protocol.md` | Want structured task execution (recommended for everyone) |
| `quality-gates.md` | Want code auditing with grades (recommended) |
| `development-standards.md` | Write Python (modify for other languages) |
| `file-classification.md` | Want automatic file organization |

### Adding Your Own Rules

Create a new `.md` file in `.claude/rules/` with this frontmatter:

```markdown
---
description: "What this rule does (shown in agent context)"
globs: "**/*.py"  # Which files this rule applies to
---

# Your Rule Name

Your rules here. Be specific and actionable.
```

**Examples of custom rules**:
- `testing-standards.md` -- Your team's testing conventions
- `api-design.md` -- REST/GraphQL patterns you follow
- `security-policy.md` -- Security requirements for your domain

### Modifying Development Standards

If you don't use Python, replace `development-standards.md` with your language's conventions:

**For TypeScript**:
```markdown
# Development Standards
- **ESLint** + **Prettier** for formatting
- **Strict TypeScript** -- no `any`, no `as` casts without justification
- **Zod** for runtime validation at boundaries
- **Vitest** for testing
```

**For Go**:
```markdown
# Development Standards
- **gofmt** + **golangci-lint**
- **Table-driven tests**
- **Context propagation** for cancellation
- **Errors as values** -- wrap with context
```

## Level 3: Device Profiles (10 minutes)

### Single Machine

If you only use one machine, simplify `.device-profiles.json`:

```json
{
  "version": "1.0.0",
  "detection": {
    "method": "hostname",
    "env_var": "COMPUTERNAME",
    "fallback_profile": "MY-PC"
  },
  "profiles": {
    "MY-PC": {
      "alias": "Main",
      "role": "executor",
      "capabilities": ["gpu", "ollama"]
    }
  }
}
```

### Multiple Machines

Add a profile for each machine. The agent reads these at session start to adapt:

- **executor**: Heavy compute, local models, CI/CD
- **supervisor**: Interactive work, code review, planning

### No Ollama

If you don't use local models, set `"ollama_available": false` and remove the `ollama_models` section. The agent will use cloud models only.

## Level 4: Slash Commands (20 minutes)

### Creating Custom Commands

Commands live in `.claude/commands/` as markdown files:

```markdown
---
description: "Run the full test suite and report results"
allowed-tools: ["Bash", "Read", "Glob"]
---

Run the project's test suite:
1. Detect the test framework (pytest, vitest, go test, etc.)
2. Execute tests with verbose output
3. Report: total, passed, failed, coverage if available
4. If failures: show the first 3 failure details
```

Save as `.claude/commands/test.md` and invoke with `/test`.

### Command Ideas

| Command | Purpose |
| ------- | ------- |
| `/status` | System overview: git status, running services, recent changes |
| `/review` | Code review with quality gate grading |
| `/deploy` | Pre-deployment checklist and execution |
| `/debug` | Structured debugging: reproduce, isolate, fix, verify |
| `/docs` | Generate or update documentation |

## Level 5: Sessions (30 minutes)

### When to Use Sessions

Sessions are valuable when:
- You work on the same project repeatedly
- You need to onboard new agents quickly
- You want reproducible starting points

### Session Design Patterns

**Audit Session** (read-only):
```yaml
tasks:
  - name: "Audit code quality"
    done_when: "Grade assigned per module"
rules:
  - "NO modifications -- audit only"
```

**Implementation Session** (focused scope):
```yaml
context_files:
  - path: "ARCHITECTURE.md"
  - path: "src/auth/middleware.ts"
tasks:
  - name: "Add rate limiting to auth middleware"
    done_when: "Rate limiter integrated, tests pass"
rules:
  - "Only modify files in src/auth/"
```

**Multi-Agent Session** (coordination):
```yaml
# A1: Implementation agent
# A2: Testing agent  
# A3: Documentation agent
# Each gets their own YAML file with specific focus
```

## Level 6: Hook System (15 minutes)

### Safety Hooks

Create `.claude/hooks.json`:

```json
{
  "hooks": [
    {
      "event": "PreToolUse",
      "tools": ["Write", "Edit"],
      "command": "your-script-that-validates-target-path"
    }
  ]
}
```

### Common Hook Patterns

**Block protected paths**:
```bash
# block_protected.sh
if echo "$TARGET" | grep -qE "(node_modules|\.env|__pycache__)"; then
  echo "BLOCKED: Cannot modify protected path"
  exit 1
fi
```

**Log all changes**:
```bash
# log_changes.sh
echo "$(date) | $TOOL | $TARGET" >> ~/.claude/audit.log
```

## Level 7: Multi-Model Routing (Advanced)

### Without the Engine

You don't need the Python engine for basic multi-model routing. Just document your routing preferences in CLAUDE.md or AGENTS.md:

```markdown
## Model Routing
- Architecture decisions → Claude Opus
- Implementation → Claude Sonnet
- Quick questions → Claude Haiku
- Data processing → Local Ollama (deepseek-coder)
```

Your agent will follow these preferences when planning tasks.

### With the Engine

The `engine/` directory provides a Python runtime with:
- Provider abstraction (Ollama, Claude, Gemini, etc.)
- Classifier that routes tasks by estimated complexity
- Supervisor/critic loop for autonomous operation

See `engine/README.md` for setup instructions.

## Growth Checklist

Track your system's evolution:

- [ ] CLAUDE.md configured with your identity
- [ ] At least one rule file active
- [ ] Device profile for your primary machine
- [ ] First custom slash command
- [ ] First session YAML file
- [ ] Hook for path protection
- [ ] Second device profile (when you get a second machine)
- [ ] Model routing preferences documented
- [ ] Project registry with 3+ projects
- [ ] Session templates for common workflows

Remember: **earn every feature**. Don't check all boxes at once. Add each one when you actually need it.
