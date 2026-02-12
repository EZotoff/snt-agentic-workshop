# Multi-Agent Development Pipeline

This directory contains 8 specialized AI agents that work together to implement an autonomous, hierarchical development pipeline.

## Architecture

The system uses a hierarchical "Manager-Worker" architecture where senior agents delegate tasks to specialized subagents.

```text
User
  │
  ▼
orchestrator (Claude Opus 4.6, 3x)
  ├──► advisor (Claude Opus 4.6, 3x)         — leaf node, no delegation
  ├──► writer (Gemini 3 Flash (Preview), 0.33x)        — leaf node, no delegation
  ├──► junior-dev (GPT-5.3-Codex, 1x)        — leaf node, direct access for simple tasks
  ├──► senior-dev (Claude Opus 4.6, 3x)      — CAN DELEGATE to: junior-dev, tester, writer
  │       └── Runs own implement → test → fix loops
  └──► ui-dev (Gemini 3 Pro (Preview), 1x)   — CAN DELEGATE to: ui-tester, writer
          └── Runs own implement → visual-verify → fix loops
```

## Agent Roster

The pipeline consists of the Orchestrator and 7 specialized worker agents.

| Agent | Model | Cost Tier | Role | Delegates? |
|-------|-------|-----------|------|------------|
| `@orchestrator` | Claude Opus 4.6 | 3x | Central coordinator, phase management | Yes (advisor, senior-dev, junior-dev, ui-dev, writer) |
| `@advisor` | Claude Opus 4.6 | 3x | Read-only thinker: analyzes requirements, designs architecture | No |
| `@senior-dev` | Claude Opus 4.6 | 3x | Complex implementation, architecture, backend verification | Yes (junior-dev, tester, writer) |
| `@ui-dev` | Gemini 3 Pro (Preview) | 1x | Frontend implementation and visual verification | Yes (ui-tester, writer) |
| `@junior-dev` | GPT-5.3-Codex | 1x | Routine implementation: pattern-following code | No |
| `@writer` | Gemini 3 Flash (Preview) | 0.33x | Documentation: proposals, specs, reports | No |
| `@tester` | GPT-5.3-Codex | 1x | Functional QA: runs code, verifies backend/logic | No |
| `@ui-tester` | GPT-5.3-Codex | 1x | Visual QA: pixel-level screenshot verification | No |

## Subagent Architecture

This system leverages **Multi-Level Delegation**:
1. **Orchestrator** acts as the Project Manager, assigning high-level goals to Lead Developers (`senior-dev`, `ui-dev`).
2. **Lead Developers** (`senior-dev`, `ui-dev`) break down tasks and assign them to specialized workers (`junior-dev`, `tester`, `ui-tester`).
3. **Leaf Nodes** (`tester`, `ui-tester`, `writer`) perform specific atomic tasks and return results up the chain.

**Key Constraint**: `tester` and `ui-tester` are **not** directly callable by the Orchestrator. They are specialized tools used by the `senior-dev` and `ui-dev` respectively to verify their own work before reporting back.

## Workflow

```text
User/Idea
    │
    ▼
@orchestrator ──► Coordinates development workflow
    │
    │ ╔══════════════════════════════════════════════════╗
    │ ║ Phase 1: Analysis & Planning                     ║
    │ ╚══════════════════════════════════════════════════╝
    ├──► @advisor ──► Analysis & Architecture
    │       │
    │       └──► @writer ──► Specs & Proposals
    │
    │ ╔══════════════════════════════════════════════════╗
    │ ║ Phase 2: Implementation & Verification           ║
    │ ╚══════════════════════════════════════════════════╝
    ├──► @junior-dev  ──► Simple/Routine Tasks (Direct)
    │
    ├──► @senior-dev  ──► Complex Logic / Backend
    │       │
    │       ├──► @junior-dev (Delegated Implementation)
    │       └──► @tester (Delegated Verification)
    │
    ├──► @ui-dev      ──► Frontend / UI
    │       │
    │       └──► @ui-tester (Delegated Visual QA)
    │
    │ ╔══════════════════════════════════════════════════╗
    │ ║ Phase 3: Documentation                           ║
    │ ╚══════════════════════════════════════════════════╝
    └──► @writer ──► Update Docs & Changelogs
```

## UI Development Workflow

UI/UX work follows a self-contained loop managed by `@ui-dev`:

```
@orchestrator
    │
    ├──► @ui-dev (Gemini 3 Pro Preview)
    │       │
    │       ├── 1. Implements frontend components
    │       ├── 2. Calls @ui-tester to take screenshots
    │       │       └── @ui-tester returns PASS/FAIL + Evidence
    │       ├── 3. Analyzes evidence and fixes issues
    │       └── 4. Returns final verified result to Orchestrator
```

The Orchestrator does not manage the visual QA loop; it simply receives the final, verified component.

## Parallel Execution

VS Code 1.109+ supports parallel subagent execution. The `@orchestrator` can call multiple agents simultaneously in the same response when tasks are independent.

**Example**:
- Tool Call 1: `@senior-dev` to implement the API (runs its own test loop).
- Tool Call 2: `@ui-dev` to implement the Dashboard (runs its own visual loop).

Both agents run in parallel, effectively doubling the development throughput.

## Escalation Protocol (5x Rule)

- **junior-dev stuck 5x (direct orchestrator assignment)**: Orchestrator calls `@advisor` to analyze the error, then assigns to `@senior-dev` to fix it.
- **senior-dev internal escalations**: `@senior-dev` handles its own failures. If a delegated `junior-dev` task fails, `senior-dev` fixes it or takes over.
- **ui-dev internal escalations**: `@ui-dev` handles its own `ui-tester` issues internally (e.g., iterating on CSS if visual verification fails).

## Usage Examples

**Start a new feature:**
```
@orchestrator Help me implement a user login system
```

**Request architectural review:**
```
@advisor Review the architecture of the authentication module
```

**Draft a specification:**
```
@writer Create an OpenSpec proposal for the notification system
```

**Note**: Agents like `@tester` and `@ui-tester` are typically invoked by `@senior-dev` or `@ui-dev` rather than directly by the user.

## Cost Optimization

The roster uses a tiered cost structure:
- **3x** (Opus): `orchestrator`, `advisor`, `senior-dev` — reserved for coordination, analysis, and complex work.
- **1x** (Codex/Pro): `junior-dev`, `tester`, `ui-dev`, `ui-tester` — the workhorses for routine implementation and verification.
- **0.33x** (Flash): `writer` — lightweight agent for documentation.
- **Savings**: By delegating routine coding to `junior-dev` and visual checks to `ui-tester` (via `ui-dev`), high-cost models are reserved for coordination and complex reasoning.

## Configuration Reference

To enable this multi-agent system, ensure the following settings are active in `.vscode/settings.json`:

```json
{
  "chat.customAgentInSubagent.enabled": true
}
```

**Leaf Agents**:
Specialized agents like `tester`, `ui-tester`, and `junior-dev` often have `user-invokable: false` in their configuration. This hides them from the agent picker to keep the UI clean, as they are designed primarily for delegation.
