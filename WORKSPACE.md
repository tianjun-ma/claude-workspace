# WORKSPACE — AI Workspace Entry Point
> Single entry file. Read at conversation start, load modules via route table.
> This file is index + init flow + mode routing only, no substantive content.

---

## Initialization Protocol

**Bot must execute in order, no skipping:**
```
Step 1 → Read this file (WORKSPACE.md) — route table
Step 2 → ↑ Migrated to CLAUDE.md (auto-injected, skip)
Step 3 → Read memory/MEMORY.md — Bot behavior memo
Step 4 → Read state/focus.md — current priorities + last stop point
         └─ If pending items contain context init read order → read listed files
Step 5 → Determine mode based on user message, load corresponding files
Step 6 → Output init report
```

---

## Route Table

### System Skills (highest priority)
> Storage: `.claude/commands/*.md` (synced via cloud drive, zero setup on new machines)

| Skill | Purpose | Writes Files |
|-------|---------|-------------|
| `/boot` | Session init: read focus.md + MEMORY.md, output status report | No |
| `/save` | Git-style commit: update daily/weekly/focus/changelog/MEMORY | Yes |
| `/sync` | Read-only snapshot: pull latest state | No |
| `/log` | View history logs (today/week/date/changes/last) | No |
| `/retro` | Mid-conversation alignment check | No |
| `/checkpoint` | Session snapshot for cross-conversation resumption | Yes |
| `/teach` | Teaching mode: Socratic diagnosis + Frayer + Feynman | No |
| `/spiral` | Tool learning mode: spiral + project-driven | No |
| `/solve` | Problem solving: pre-mortem + step verification + CoVe | No |
| `/resume` | Resume mode: project experience → resume language rewrite | No |
| `/memorize` | System rule manual push: route to correct target file | Yes |
| `/diagnose` | Protocol breakdown diagnosis | No |
| `/pace` | Granularity selector: three-level preview + session lock | No |

### Config Layer (Identity + Protocols)
| File | Purpose | Load Timing |
|------|---------|-------------|
| `config/protocols/teaching.md` | Teaching protocol (diagnosis + Frayer + Feynman) | Teaching mode |
| `config/protocols/problem-solving.md` | Problem solving protocol | Problem solving mode |
| `config/protocols/resume.md` | Resume dual-track protocol | Resume mode |
| `config/protocols/dev-tool-learning.md` | Dev tool learning protocol (spiral + project-driven) | Tool learning mode |

### State Layer (Progress + Plans)
| File | Purpose |
|------|---------|
| `state/focus.md` | **Project Frontier**: priorities, continuation points, todos |

### Memory Layer
| File | Purpose | Load Timing |
|------|---------|-------------|
| `memory/MEMORY.md` | Bot behavior memo (corrections + preference tuning) | **Every init** |

### Output Layer
| Directory | Content |
|-----------|---------|
| `outputs/` | Generated documents, reports, session snapshots |

### Log Layer
| File | Purpose |
|------|---------|
| `logs/daily/` | Daily learning logs |
| `logs/weekly/` | Weekly reviews |
| `logs/changelog.md` | System change log |

---

## Mode System

| Mode | Trigger | Load Protocol |
|------|---------|--------------|
| Teaching | "I want to learn X" (with lecture notes/PDF) | `/teach` Skill |
| Tool Learning | "learn X tool" (no fixed notes) | `/spiral` Skill |
| Methodology | "discuss X" / "trade-offs of X" | No extra protocol |
| Problem Solving | "solve this problem" | `/solve` Skill |
| Resume | "resume mode" / "help with resume" | `/resume` Skill |
| Conversation | Default | None |

---

## Git-Style Conversation Rules

**pull = conversation start** → execute init protocol
**commit = conversation end** → trigger words: "save" / "end" / "update log"

```
Step 1 → logs/daily/YYYY-MM-DD.md (append)
Step 2 → logs/weekly/YYYY-WXX.md (append summary)
Step 3 → state/focus.md (update stop point)
Step 4 → Pointer integrity check
Step 5 → logs/changelog.md (only if config/ or WORKSPACE.md changed)
Step 6 → memory/MEMORY.md (only if corrections or new patterns)
```
