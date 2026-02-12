# Multi-Agent Development Pipeline

This directory contains specialized AI agents that work together to implement an autonomous development pipeline.

## Agent Roster

### Tier 1: Coordination Layer
| Agent | Model | Cost | Role |
|-------|-------|------|------|
| `@orchestrator` | `claude-opus-4.6` | 1x | Workflow coordination, phase management |

### Tier 2: Senior Specialists
| Agent | Model | Cost | Role |
|-------|-------|------|------|
| `@planner` | `claude-opus-4.6` | 1x | Requirements analysis, architecture design |
| `@senior-dev` | `claude-opus-4.6` | 1x | Complex debugging, junior escalations |
| `@senior-tester` | `claude-opus-4.6` | 1x | Test strategy, exploratory testing |

### Tier 3: Junior Workers
| Agent | Model | Cost | Role |
|-------|-------|------|------|
| `@junior-dev` | `GPT-5.3-Codex` | 0.33x | Backend/pipeline implementation |
| `@ui-dev` | `Gemini 3 Pro (Preview)` | 0.5x | UI/UX implementation (React, Tailwind) |
| `@junior-tester` | `GPT-5.3-Codex` | 0.33x | Backend verification (user proxy) |
| `@ui-tester` | `Gemini 3 Flash (Preview)` | 0.33x | Visual QA (pixel verification) |
| `@deployer` | `claude-haiku-4.5` | 0.33x | Deployment operations |

### Tier 4: Trivial Task Handlers
| Agent | Model | Cost | Role |
|-------|-------|------|------|
| `@scaffolder` | `Raptor mini (Preview)` | 0x | Boilerplate file creation |
| `@spec-writer` | `Gemini 3 Pro (Preview)` | 0x | Spec document writing |
| `@reporter` | `Gemini 3 Flash (Preview)` | 0x | Documentation updates |

## Workflow

```
User/Idea
    │
    ▼
@orchestrator ──► Coordinates development workflow
    │
    │ ╔══════════════════════════════════════════════╗
    │ ║ STAGE 1: Planning & Design                   ║
    │ ╚══════════════════════════════════════════════╝
    ├──► @planner ──► @spec-writer ──► Change Proposal
    │         │
    │         └──► Automated Validation
    │         └──► User approval required
    │
    │ ╔══════════════════════════════════════════════╗
    │ ║ STAGE 2: Implementation & Testing            ║
    │ ╚══════════════════════════════════════════════╝
    ├──► @scaffolder ──► @junior-dev ──► Implementation
    │         │              │
    │         │              └──► Updates task list
    │         └──► (5x fail) ──► @senior-dev
    │
    ├──► @junior-tester + @ui-tester ──► Verification
    │         │
    │         └──► (5x fail) ──► @senior-tester
    │
    ├──► @deployer ──► Staging ──► Production
    │
    │ ╔══════════════════════════════════════════════╗
    │ ║ STAGE 3: Delivery & Documentation            ║
    │ ╚══════════════════════════════════════════════╝
    └──► @reporter ──► Archive & Report
              │
              └──► Documentation updates
```

## UI Development Workflow

UI/UX work uses a specialized two-agent pattern:

```
@orchestrator
    │
    ├──► @ui-dev (Gemini 3 Pro Preview)
    │       │
    │       ├── Implements React/Tailwind components
    │       ├── Verifies DOM structure via browser_snapshot
    │       └── ⚠️ CANNOT see screenshots (Gemini limitation)
    │
    └──► @ui-tester (Gemini 3 Flash Preview)
            │
            ├── Takes actual screenshots
            ├── Runs 15-point Visual QA Checklist
            ├── Verifies: colors, dark mode, spacing, responsive
            └── Returns: PASS / PARTIAL / FAIL with evidence
```

**Why two agents?**
- `@ui-dev` uses Gemini which excels at code generation but cannot process images
- `@ui-tester` uses Gemini Flash which can see and analyze screenshots
- Together they ensure both code quality AND visual correctness

**8-Point Visual QA Checklist**:
- Dark Mode Desktop: 8 checks (text, colors, spacing, images, icons, interactions, forms, layout)
- Desktop only (1280px) — no responsive testing
- Dark mode only — no light mode testing

## Escalation Protocol (5x Rule)

When a junior agent fails the same task 5 times:
1. Junior reports escalation with error summary
2. Orchestrator routes to corresponding senior
3. Senior debugs and either fixes or provides guidance
4. Work continues from resolution point

## Usage

### Direct orchestration
```
@orchestrator Help me implement a user login system with JWT
```

### Create a new proposal
```
@planner I need to add two-factor authentication to the login flow
```

### Quick UI test
```
@ui-tester Navigate to http://localhost:3000/dashboard and verify the sidebar
```

### Archive a completed feature
```
@reporter Summarize the changes made for the login system and update documentation
```

## Cost Optimization

Estimated cost per feature (typical complexity):
- **With junior/senior layering**: ~10x equivalent requests
- **All senior-level**: ~20x equivalent requests
- **Savings**: 40-50%

