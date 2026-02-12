# Agentic Workshop

Multi-agent AI development pipeline using GitHub Copilot agents + OpenSpec.

## What's Inside

- `.github/agents/` — 8 specialized AI agents (orchestrator, advisor, devs, testers, writer)
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
- Ask an agent: `@orchestrator Help me fill in openspec/project.md for a [describe project]`

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
1. Call `@advisor` to analyze requirements and propose a plan
2. Call `@writer` to write specifications (if needed)
3. Route implementation to `@junior-dev`, `@senior-dev`, or `@ui-dev`
4. Verify with `@tester` or `@ui-tester`

## Agent Roster (Quick Reference)

| Agent | Role |
|-------|------|
| `@orchestrator` | Central coordinator — routes work through the pipeline |
| `@advisor` | Analyzes requirements, designs architecture, debugs complex issues |
| `@writer` | Documentation: OpenSpec proposals, specs, changelogs, reports |
| `@senior-dev` | Complex implementation: architecture-heavy, cross-cutting code |
| `@junior-dev` | Routine implementation: pattern-following, scaffolding, bulk coding |
| `@tester` | User proxy: runs code, inspects outputs, reports honestly |
| `@ui-dev` | Frontend implementation: HTML/CSS/JS components |
| `@ui-tester` | Visual QA: screenshot-based verification |

→ See [`.github/agents/README.md`](.github/agents/README.md) for full details, workflow diagrams, and cost breakdown.

## How It Works

The pipeline follows: Analyze → Implement → Test → Document.
The `@orchestrator` coordinates everything — you just tell it what you want built.

## License

MIT
