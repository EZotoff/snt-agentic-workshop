# Agentic Workshop

Multi-agent AI development pipeline using GitHub Copilot agents + OpenSpec.

## What's Inside

- `.github/agents/` — 12 specialized AI agents (orchestrator, planner, devs, testers, etc.)
- `openspec/` — Spec-driven development framework config
  - `AGENTS.md` — Instructions for AI assistants
  - `project.md` — Project context template (fill this first!)
- `.devcontainer/` — GitHub Codespaces config (auto-installs Python, Node, OpenSpec)

## Prerequisites

- GitHub Copilot with agent mode enabled
- VS Code (local or Codespaces)
- Node.js 20.19+ (auto-installed in Codespaces)

## Quick Start

### Step 1: Set up your project context
Fill in `openspec/project.md` with your project details.
- Edit manually, OR
- Ask an agent: `@planner Help me fill in openspec/project.md for a [describe project]`

### Step 2: Initialize OpenSpec
```bash
openspec init
```
This creates the `openspec/specs/` and `openspec/changes/` directories.

### Step 3: Build your first feature
Tell the orchestrator what to build:
```
@orchestrator I want to build [describe your feature]
```

The orchestrator will:
1. Call `@planner` to create a proposal
2. Call `@spec-writer` to write specifications
3. Route implementation to `@junior-dev` or `@ui-dev`
4. Verify with `@junior-tester` or `@ui-tester`

## Agent Roster (Quick Reference)

| Agent | Role |
|-------|------|
| `@orchestrator` | Central coordinator — routes work through the pipeline |
| `@planner` | Analyzes requirements, creates proposals |
| `@spec-writer` | Writes detailed specifications |
| `@junior-dev` | Implements backend code |
| `@ui-dev` | Implements UI/frontend |
| `@senior-dev` | Debugs complex issues (escalation) |
| `@junior-tester` | Verifies backend features |
| `@ui-tester` | Visual QA with screenshots |
| `@senior-tester` | Debugs test failures (escalation) |
| `@scaffolder` | Creates boilerplate files |
| `@deployer` | Handles deployment |
| `@reporter` | Archives changes, updates docs |

→ See [`.github/agents/README.md`](.github/agents/README.md) for full details, workflow diagrams, and cost breakdown.

## How It Works

The pipeline follows: Plan → Implement → Test → Deploy → Archive.
The `@orchestrator` coordinates everything — you just tell it what you want built.
Junior agents try first; if they fail 5 times, seniors step in.

## License

MIT
