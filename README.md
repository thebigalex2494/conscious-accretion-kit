# Conscious Accretion Kit

> Build your own AI-powered control system. Grow it organically around your actual workflow, instead of adapting to someone else's tool.

## The Problem

AI tools are generic. Your workflow is not.

Current solutions force you to adapt to their structure: their project templates, their agent patterns, their orchestration flows. But the most effective systems are the ones that grew from your real work -- not from a template.

## The Philosophy

This framework is built on three principles:

**Gall's Law** -- Complex systems that work always evolved from simple systems that worked. Don't design the perfect system upfront. Start with what solves today's problem.

**Conscious Accretion** -- Like planets forming from dust, your control system grows by accumulating solutions around a functional core. Each addition solves a real problem, not a hypothetical one.

**Metasystem Transition** -- When a control mechanism becomes sophisticated enough, it becomes a new level of organization. Your personal tool becomes your operating system.

## What You Get

A framework for building a personal AI orchestration system with:

- **6-Phase Orchestration Protocol** -- INTAKE > PLAN > EXECUTE > VALIDATE > AGGREGATE > HANDOFF
- **Dual-Mode Architecture** -- Autonomous agent + delegated executor
- **Circuit Breaker** -- 3-retry escalation with model upgrade fallback
- **Multi-Model Routing** -- Local-first ($0), cloud-fallback strategy
- **Device-Aware Profiles** -- Multi-machine setups with capability detection
- **Session System** -- YAML-based reproducible agent contexts
- **Quality Gates** -- Grading scale (A+ to F) with SIP improvement protocol
- **File Classification** -- Automatic taxonomy for project organization
- **Hook System** -- Pre/post-tool safety interceptors

## Quick Start

See [docs/quickstart.md](docs/quickstart.md) for a 30-minute setup guide.

### Minimal Setup (5 minutes)

1. Copy `framework/CLAUDE.md.template` to your project root as `CLAUDE.md`
2. Copy `framework/.claude/rules/` to your project
3. Fill in the `[YOUR_...]` placeholders
4. Start a Claude Code session -- the orchestration protocol is now active

### Full Setup (30 minutes)

1. Copy the entire `framework/` directory to your workspace
2. Run `framework/setup/Setup-ControlCenter.ps1` (Windows) or `setup.sh` (Linux/macOS)
3. Configure `.device-profiles.json` with your machines
4. Create your first session YAML in `sessions/`

## Architecture

```
[User Request]
     |
     v
PHASE 1: INTAKE -- Classify intent, detect mode (autonomous vs delegated)
     |
     v
PHASE 2: PLAN -- Decompose task, assign models, create subtask graph
     |
     v
PHASE 3: EXECUTE -- Parallel dispatch (local models $0, cloud fallback)
     |
     v
PHASE 4: VALIDATE -- Deterministic checks > audit > QA > circuit breaker
     |
     v
PHASE 5: AGGREGATE -- Synthesize results, executive summary
     |
     v
PHASE 6: HANDOFF -- Structured contracts, audit trail, context snapshot
```

## Directory Structure

```
framework/
  CLAUDE.md.template          -- Your agent's identity and behavior
  AGENTS.md.template          -- Shared knowledge across all agents
  .device-profiles.json.example
  .claude/
    rules/                    -- Auto-loaded development standards
    commands/                 -- Custom slash commands
    hooks.json.example        -- Safety interceptors
  .central-agent/
    config/                   -- Classification taxonomy, AI indicators
    skills/                   -- Reusable operation blocks
    docs/templates/           -- Prompt engineering frameworks
  sessions/
    example_session.yaml      -- Reproducible agent context definition

engine/                       -- Python runtime (optional)
  src/conscious_accretion/
    providers/                -- Multi-model provider abstraction
    local_router/             -- Intelligent model routing
    autonomy/                 -- Supervisor + critic system

examples/
  minimal/                    -- Bare minimum: 1 CLAUDE.md + 1 rule file
  full/                       -- Complete setup with sample projects
```

## Born From

This framework was extracted from a production system used daily to:
- Orchestrate AI agents across 6+ development projects
- Manage automated ETL pipelines processing 60K+ records
- Coordinate work between Claude, Gemini, local models (Ollama), and browser automation
- Operate across multiple devices with Google Drive sync

The core insight: **the orchestration layer became more valuable than any individual project it managed.** That's the metasystem transition in action.

## Documentation

- [Philosophy](docs/philosophy.md) -- Why conscious accretion works
- [Quick Start](docs/quickstart.md) -- Setup in 30 minutes
- [Architecture](docs/architecture.md) -- Technical deep dive
- [Customization](docs/customization.md) -- Make it yours

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT -- See [LICENSE](LICENSE)
