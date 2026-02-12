# Multi-Agent Development Pipeline

This directory contains 8 specialized AI agents that work together to implement an autonomous development pipeline.

## Agent Roster

The pipeline consists of the Orchestrator and 7 specialized worker agents.

| Agent | Model | Cost Tier | Role |
|-------|-------|-----------|------|
| `@orchestrator` | Claude Opus 4.6 | 1x | Central coordinator, phase management |
| `@advisor` | Claude Opus 4.6 | 1x | Read-only thinker: analyzes requirements, designs architecture |
| `@senior-dev` | Claude Opus 4.6 | 1x | Complex implementation, junior escalations |
| `@ui-dev` | Gemini 3 Pro (Preview) | 0.5x | Frontend: HTML/CSS/JS implementation |
| `@junior-dev` | GPT-5.3-Codex | 0.33x | Routine implementation: pattern-following code |
| `@writer` | Gemini 3 Flash (Preview) | 0.33x | Documentation: proposals, specs, reports |
| `@tester` | GPT-5.3-Codex | 0.33x | User proxy: runs code, verifies backend/logic |
| `@ui-tester` | Gemini 3 Flash (Preview) | 0.33x | Visual QA: pixel-level screenshot verification |

## Workflow

```
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
    │ ║ Phase 2: Implementation (Parallel Execution)     ║
    │ ╚══════════════════════════════════════════════════╝
    ├──► @junior-dev  ──► Backend/Routine Logic
    │       ║
    │       ╠══ (+) Parallel Execution Supported
    │       ║
    ├──► @ui-dev      ──► Frontend Components
    │       ║
    │       ╚══ (or) @senior-dev ──► Complex Architecture
    │
    │ ╔══════════════════════════════════════════════════╗
    │ ║ Phase 3: Verification                            ║
    │ ╚══════════════════════════════════════════════════╝
    ├──► @tester      ──► Functional Verification
    │       ║
    │       ╚══ (+) @ui-tester ──► Visual QA
    │
    │ ╔══════════════════════════════════════════════════╗
    │ ║ Phase 4: Documentation                           ║
    │ ╚══════════════════════════════════════════════════╝
    └──► @writer ──► Update Docs & Changelogs
```

## UI Development Workflow

UI/UX work uses a specialized two-agent pattern to ensure visual quality:

```
@orchestrator
    │
    ├──► @ui-dev (Gemini 3 Pro Preview)
    │       │
    │       ├── Implements frontend components
    │       ├── Verifies DOM structure via browser_snapshot
    │       └── ⚠️ CANNOT see screenshots (Gemini limitation)
    │
    └──► @ui-tester (Gemini 3 Flash Preview)
            │
            ├── Takes actual screenshots
            ├── Verifies visual correctness (colors, layout, spacing)
            └── Returns: PASS / FAIL with evidence
```

**Why this split?**
- `@ui-dev` uses **Gemini 3 Pro (Preview)** which excels at generating HTML/CSS code.
- `@ui-tester` uses **Gemini 3 Flash (Preview)** which can process images for visual QA.
- Together they ensure code quality AND visual correctness.

## Parallel Execution

VS Code 1.109+ supports parallel subagent execution. The `@orchestrator` can call multiple agents simultaneously in the same response when tasks are independent (e.g., implementing backend and frontend features concurrently).

## Escalation Protocol (5x Rule)

If a worker agent fails to complete a task after 5 attempts, the Orchestrator escalates:

- **junior-dev stuck 5x** → `@advisor` analyzes the error, then `@senior-dev` fixes it.
- **tester stuck 5x** → `@advisor` debugs the test approach or data.
- **ui-dev stuck 5x** → `@senior-dev` (if code issue) or `@advisor` (if design issue).

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

**Verify UI appearance:**
```
@ui-tester Navigate to http://localhost:3000 and verify the dashboard layout
```

## Cost Optimization

The roster relies on a heavy junior/senior split to optimize costs:
- Routine tasks go to **0.33x** agents (`junior-dev`, `tester`).
- Complex tasks go to **1x** agents (`senior-dev`, `advisor`).
- **Savings**: This structure saves approximately **40-50%** compared to an all-senior roster.

## Configuration Reference

To enable this multi-agent system, ensure the following setting is active in VS Code:
- `chat.customAgentInSubagent.enabled`: `true`
