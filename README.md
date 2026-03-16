# claude-workspace

A structured personal workspace template for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), featuring 13 custom slash commands, learning protocols, and a git-style conversation management system.

## What This Is

A **complete workspace architecture** for Claude Code that turns ad-hoc AI conversations into a managed workflow with:

- **Session lifecycle management** — Boot → Work → Save, with automatic state persistence
- **Learning protocols** — Socratic diagnosis, Frayer Model teaching, Feynman verification loops
- **Cross-conversation continuity** — Checkpoint snapshots that let new sessions resume exactly where you left off
- **Behavioral memory** — Bot corrections and preference tuning that persist across conversations

## Architecture

```
claude-workspace/
├── CLAUDE.md                    # Auto-injected workspace rules (identity + modes + triggers)
├── WORKSPACE.md                 # Route table + initialization protocol
├── .claude/
│   └── commands/                # 13 slash commands (the core of the system)
│       ├── boot.md              # /boot — Session initialization
│       ├── save.md              # /save — Git-style commit (update logs/state/memory)
│       ├── sync.md              # /sync — Read-only state snapshot
│       ├── log.md               # /log — View history logs
│       ├── retro.md             # /retro — Mid-conversation alignment check
│       ├── checkpoint.md        # /checkpoint — Cross-conversation session snapshot
│       ├── teach.md             # /teach — Socratic + Frayer + Feynman teaching
│       ├── spiral.md            # /spiral — Spiral tool/framework learning
│       ├── solve.md             # /solve — Pre-mortem + CoVe problem solving
│       ├── resume.md            # /resume — Resume writing with dual-track protocol
│       ├── memorize.md          # /memorize — Manual push rules to correct system file
│       ├── diagnose.md          # /diagnose — Protocol breakdown diagnosis
│       └── pace.md              # /pace — Granularity selector (3 levels)
├── config/
│   └── protocols/               # Detailed protocol files (loaded on-demand by skills)
│       ├── teaching.md
│       ├── problem-solving.md
│       ├── resume.md
│       └── dev-tool-learning.md
├── state/
│   └── focus.md                 # Current priorities + continuation points
├── memory/
│   └── MEMORY.md                # Bot behavior memo (corrections + patterns)
├── logs/                        # (gitignored) Daily/weekly logs
└── outputs/                     # (gitignored) Generated documents
```

## Skill Categories

### Lifecycle (6 skills)
| Skill | What It Does |
|-------|-------------|
| `/boot` | Reads focus + memory, detects checkpoints, outputs status report |
| `/save` | Git-style commit: scans deferred items, updates logs/state/memory |
| `/sync` | Read-only snapshot of current state |
| `/log` | Query logs by date, week, or type |
| `/retro` | Mid-conversation goal vs. progress alignment check |
| `/checkpoint` | Generates a briefing memo (not a summary) for cross-conversation handoff |

### Learning Modes (5 skills)
| Skill | What It Does |
|-------|-------------|
| `/teach` | Socratic diagnosis → Frayer Model 4-dimension teaching → Feynman verification |
| `/spiral` | Three-layer spiral for tool/framework learning (scan → project → deep dive) |
| `/solve` | Pre-mortem analysis → step-by-step with verification checkpoints → CoVe chain |
| `/resume` | Dual-track resume protocol (experience mining → resume language generation) |
| `/pace` | Shows 3 granularity levels for current topic, locks selection for the session |

### Utilities (2 skills)
| Skill | What It Does |
|-------|-------------|
| `/memorize` | Routes a rule/correction to the correct system file (not just MEMORY.md) |
| `/diagnose` | Diagnoses protocol deviation: 4 failure modes → root cause → fix |

## Quick Start

1. **Clone** this repo into your Claude Code project directory:
   ```bash
   git clone https://github.com/tianjun-ma/claude-workspace.git my-workspace
   cd my-workspace
   ```

2. **Customize** `CLAUDE.md` — Replace placeholders with your identity, goals, and preferences

3. **Customize** `state/focus.md` — Set your current priorities and tasks

4. **Start Claude Code** in the workspace directory — CLAUDE.md is auto-injected

5. **Type `/boot`** to initialize — Bot reads your state and memory, outputs a status report

## Design Principles

- **Internalization over storage** — The system helps you process information, not just record it
- **Output proves understanding** — Teaching protocols require you to explain back (Feynman loops)
- **AI retrieves, human judges** — Bot does the searching and organizing, you make the decisions
- **Only optimize, never degrade** — Any system change must be verified to not break existing functionality

## How the Conversation Lifecycle Works

```
/boot (pull)                    /save (commit)
    │                               │
    ▼                               ▼
Read state/focus.md            Scan deferred items
Read memory/MEMORY.md    →     Update logs/daily/
Detect checkpoints             Update state/focus.md
Output status report           Update memory/MEMORY.md
    │                               │
    ▼                               ▼
 [Work session]              [Next conversation
  /teach /solve                picks up where
  /spiral /resume              you left off]
  /retro /pace]
```

## License

MIT
