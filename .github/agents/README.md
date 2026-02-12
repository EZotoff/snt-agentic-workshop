# Multi-Agent Development Pipeline

This directory contains 8 specialized AI agents that work together to implement an autonomous development pipeline.

## Agent Roster

The pipeline consists of the Orchestrator and 7 specialized worker agents.

| Agent | Model | Cost Tier | Role |
|-------|-------|-----------|------|
| `@orchestrator` | Claude Opus 4.6 | 3x | Central coordinator, phase management |
| `@advisor` | Claude Opus 4.6 | 3x | Read-only thinker: analyzes requirements, designs architecture |
| `@senior-dev` | Claude Opus 4.6 | 3x | Complex implementation, junior escalations |
| `@ui-dev` | Gemini 3 Pro (Preview) | 1x | Frontend: HTML/CSS/JS implementation |
| `@junior-dev` | GPT-5.3-Codex | 1x | Routine implementation: pattern-following code |
| `@writer` | Gemini 3 Flash (Preview) | 0.33x | Documentation: proposals, specs, reports |
| `@tester` | GPT-5.3-Codex | 1x | User proxy: runs code, verifies backend/logic |
| `@ui-tester` | GPT-5.3-Codex | 1x | Visual QA: pixel-level screenshot verification |

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
    └──► @ui-tester (GPT-5.3-Codex)
            │
            ├── Takes actual screenshots
            ├── Verifies visual correctness (colors, layout, spacing)
            └── Returns: PASS / FAIL with evidence
```

**Why this split?**
- `@ui-dev` uses **Gemini 3 Pro (Preview)** which excels at generating HTML/CSS code.
- `@ui-tester` uses **GPT-5.3-Codex** which can process images for visual QA.
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

The roster uses a tiered cost structure:
- **3x** (Opus): `orchestrator`, `advisor`, `senior-dev` — reserved for coordination, analysis, and complex work.
- **1x** (Codex/Pro): `junior-dev`, `tester`, `ui-dev` — the workhorses for routine implementation and verification.
- **0.33x** (Flash): `writer` — lightweight agent for documentation.
- `ui-tester` also uses Codex (1x) since Gemini models are unreliable at reading screenshots.
- **Savings**: Routing routine work to 1x/0.33x agents saves significantly compared to an all-Opus roster.

## Configuration Reference

To enable this multi-agent system, ensure the following setting is active in VS Code:
- `chat.customAgentInSubagent.enabled`: `true`
