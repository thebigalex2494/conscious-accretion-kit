# Quick Start Guide

Get your personal control system running in 30 minutes.

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) (CLI or IDE extension)
- Git
- Python 3.10+ (optional, for the engine)
- Ollama (optional, for local models)

## Step 1: Create Your Workspace (5 min)

Choose a directory that will be your control center:

```bash
mkdir ~/control-center
cd ~/control-center
git init
```

## Step 2: Copy the Framework (5 min)

Copy these files from the kit's `framework/` directory:

```
control-center/
  CLAUDE.md              <-- from CLAUDE.md.template
  .device-profiles.json  <-- from .device-profiles.json.example
  .claude/
    rules/
      orchestration-protocol.md
      quality-gates.md
      development-standards.md
      file-classification.md
  sessions/
    (empty for now)
```

## Step 3: Configure Your Identity (10 min)

### CLAUDE.md

Open `CLAUDE.md` and replace all `[YOUR_...]` placeholders:

- `[YOUR_SYSTEM_NAME]` -- What you want to call your system (e.g., "Mission Control")
- `[YOUR_DESCRIPTION]` -- What it does for you
- `[YOUR_AGENT_NAME]` -- Your agent's name
- `[YOUR_WORKSPACE_PATH]` -- Absolute path to this directory
- `[YOUR_LANGUAGE]` -- Your preferred language

### .device-profiles.json

Replace the hostname placeholders with your actual machine:

```bash
# Find your hostname
hostname    # Linux/macOS
$env:COMPUTERNAME  # Windows PowerShell
```

Fill in:
- `[YOUR_PRIMARY_HOSTNAME]` -- Your main machine's hostname
- `[YOUR_USERNAME]` -- Your OS username
- `[YOUR_WORKSPACE_PATH]` -- Path to this control center

## Step 4: Test It (5 min)

Open a Claude Code session in your control center:

```bash
cd ~/control-center
claude
```

Try these:
1. Ask Claude to read the CLAUDE.md and confirm it loaded
2. Ask it to describe the orchestration protocol
3. Ask it to audit a file using the quality gates

If Claude references the 6-phase flow, the rules loaded correctly.

## Step 5: Create Your First Session (5 min)

Copy `framework/sessions/example_session.yaml` to `sessions/` and customize it:

```yaml
session:
  id: "A1-2026-04-01"
  name: "My First Session"
  status: pending
  priority: HIGH

project:
  name: "My Project"
  working_dir: "/path/to/my/project"

context_files:
  - path: "README.md"
    purpose: "Project overview"

tasks:
  - id: T1
    name: "Understand the codebase"
    done_when: "Architecture summary written"

rules:
  - "Read all context files before starting"
```

## What's Next?

- **Add more rules** as you discover recurring patterns
- **Add device profiles** when you work from multiple machines
- **Add slash commands** in `.claude/commands/` for frequent operations
- **Install the engine** if you want multi-model routing (see `engine/README.md`)

Read [Philosophy](philosophy.md) to understand the growth principles, or [Architecture](architecture.md) for the technical deep dive.
