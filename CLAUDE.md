# CLAUDE.md — Workspace Identity & Rules
> Project-level auto-injection layer. Contains workspace rules + mode routing.
> Global behavior constraints (5 hard rules + search rules) go in ~/.claude/CLAUDE.md (not included here).

---

## Who Am I

**[Your Name]**, [Your Role/Background]
Current phase: [e.g., Job search + AI tool practice]

## Current P1 Focus

1. **[Primary Goal]** — e.g., Backend job search
2. **[Secondary Goal]** — e.g., AI tool practice (Claude Code learning)

## Priority Declaration

All TODOs go through `state/focus.md`, not `TASKS.md`.
Bot should not auto-create productivity skill files (TASKS.md / dashboard.html).

---

## Core Philosophy

1. **Internalization over storage** — The real question is whether you've processed it, not whether you've recorded it
2. **Output proves understanding** — If you can explain it in three sentences (what/why/how), you've internalized it
3. **AI handles retrieval, human handles judgment** — Tools evolve, judgment doesn't expire

---

## Bot Behavior Rules

**Auto-initialization (triggered on first message of every conversation):**
- User explicitly inputs `/boot` → follow boot skill, skip this rule
- All other first messages → auto-execute:
  1. Read `state/focus.md`
  2. Read `memory/MEMORY.md`
  3. Check if focus.md has `context init read order` for current P1 task
  4. Output concise init report, then wait

**File read rules:**
- User mentions a course/project/workflow → read `state/focus.md` first
- User says "what's next" → read `state/focus.md`

**File modification rules:**
- Before creating/rewriting/modifying any user file → state reason and plan, execute only after user approves

---

## Trigger Words

| Trigger | AI Behavior |
|---------|-------------|
| "explain line by line" | One line at a time, skip nothing |
| "explain from code execution perspective" | Simulate runtime, show memory state each step |
| "I don't understand keyword X" | Precisely target X, don't repeat the whole section |
| "can you go deeper from source code" | Most detailed code dissection mode |
| "why do we need X" | Scenario comparison + guided reasoning |

---

## Mode System

| Mode | Trigger | Protocol |
|------|---------|----------|
| Teaching | "I want to learn X" / upload PDF | `/teach` Skill |
| Tool Learning | Tool/framework learning without fixed lecture notes | `/spiral` Skill |
| Methodology | Design/architecture/trade-offs | Frayer + trade-off (no extra loading) |
| Problem Solving | Coding/step-based tasks | `/solve` Skill |
| Resume | Resume-related | `/resume` Skill |
| Conversation | Default | None |

**Auto-detection (in conversation mode):**
- "why/how + technical concept" → suggest `/teach`
- Contains code/error → suggest `/solve`
- "design/architecture/trade-off/compare" → enter methodology directly
- "learn X tool/framework" + no fixed notes → suggest `/spiral`

---

## Git-Style Conversation Rules

**commit trigger words:** "save progress" / "end session" / "update log" / "that's it for today"

**commit sequence:**
1. `logs/daily/YYYY-MM-DD.md` (append, don't overwrite)
2. `logs/weekly/YYYY-WXX.md` (append summary line)
3. `state/focus.md` (update stop point + todos)
4. Pointer integrity check → sync WORKSPACE.md route table if new files created
5. `logs/changelog.md` (only if config/ or WORKSPACE.md modified)
6. `memory/MEMORY.md` (only if corrections or new patterns discovered)

**Degradation mechanism:** Over 10 rounds without commit → Bot proactively reminds "want to save progress?"

---

## Compliance Tag (Teaching / Problem Solving / Resume modes)

Append at end of each output:
```
[Compliance Check | Protocol: X | Phase Y Step Z]
Rule text: "..."
Executed / Omitted / Waiting
```
Not required for methodology and conversation modes.
