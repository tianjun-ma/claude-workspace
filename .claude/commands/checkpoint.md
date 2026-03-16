# /checkpoint — Session Snapshot for Cross-Conversation Resumption

**Trigger words:** `/checkpoint`, "存档一下", "切对话", "我要换个对话窗口", "session快照"

---

## Design Philosophy

This is NOT a conversation summary. It is a **briefing memo** — the kind a new Bot reads to operate at the same calibration level immediately, without re-deriving anything from scratch.

What's hardest to reconstruct across conversations:
- Per-concept understanding state (what the user has actually demonstrated vs. what was covered)
- Session-specific calibration (pace, effective examples, stuck points)
- In-session protocol patches (temporary rules that haven't been /memorize'd yet)
- Active hypotheses driving the Bot's current teaching/solving strategy

Token cost is irrelevant. Seamless handoff is the goal.

---

## Execution Steps

When `/checkpoint` is triggered:

### Step 1 — Synthesize (not extract) the snapshot

Internally reconstruct the following from conversation context. Do NOT copy-paste raw exchanges. Synthesize, compress, and infer.

### Step 2 — Determine the filename

Format: `YYYY-MM-DD-[topic].md`
- Date: today's date from MEMORY.md or system context
- Topic: 2-4 word slug describing the session's core subject (e.g., `aws-storage`, `lc-graphs`, `claude-code-spiral2`)

### Step 3 — Write the file

Write to: `outputs/session-snapshots/YYYY-MM-DD-[topic].md`

Use the exact template below. All 7 sections are required. Do not omit any section; write "N/A" only if a section genuinely has no content.

### Step 4 — Confirm

Print exactly:
```
Checkpoint saved → outputs/session-snapshots/YYYY-MM-DD-[topic].md
下次对话开始时，初始化会自动检测此文件并给出接续选项，无需额外操作。
```

---

## Snapshot Template

```markdown
# Session Checkpoint: [Topic]
Date: YYYY-MM-DD | Created by: /checkpoint

## 1. Identity & Active Mode
- User: Tianjun, CS Master UIUC, OPT prep
- Current mode: [Teaching / Quiz / Solve / Spiral / Conversation / etc.]
- Active skill/protocol: [e.g., teaching.md Phase 2 Step B, problem-solving.md Step 3]
- Active protocol patches: [list any in-session adjustments not yet written to MEMORY.md — e.g., "slowing pace on X topic", "skipping quiz phase per user request"]

## 2. Exact Stopping Point
[One sentence. What was being done, and exactly where it stopped. Be surgical — e.g., "Explained EBS vs EFS analogy; about to ask Feynman verification question for EFS."]

## 3. Per-Concept Understanding State
| Concept | Status | Evidence | Notes |
|---------|--------|----------|-------|
| [name] | ✅ Confirmed / ⏳ In Progress / ❌ Not Started / ⚠️ Shaky | [what user said or did that revealed this] | [calibration notes for next Bot] |

Status definitions:
- ✅ Confirmed: user demonstrated understanding (Feynman, applied correctly, or passed verification)
- ⏳ In Progress: covered but not yet verified
- ❌ Not Started: queued but not reached
- ⚠️ Shaky: covered, but user showed confusion or partial understanding

## 4. Session Calibration Parameters
- Pace: [fast / slow / variable — describe what worked, e.g., "user absorbs quickly but needs visual anchors"]
- Effective examples/analogies: [specific examples that landed well]
- Stuck points: [where user needed extra time, re-explanation, or asked clarifying questions]
- Active hypotheses: [e.g., "Situated Cognition approach landing better than definition-first", "user prefers code examples over diagrams for this topic"]
- Preferred output format this session: [e.g., "inline code comments", "bullet lists", "tables"]

## 5. Pending Items
- Immediate next action: [exact next step — specific enough that new Bot can start without asking]
- This session's queue (ordered):
  1. [item]
  2. [item]
- Deferred to later (explicitly postponed): [what and why]
  ⚠️ Each deferred item here MUST also be /memorize'd to MEMORY.md before checkpoint is complete — do not rely on Section 5 alone, as checkpoint files are not always read on boot if the topic changes.

## 6. In-Session Discoveries (not yet in MEMORY.md)
[Patterns, preferences, or rules observed this session that should inform the next Bot but haven't been /memorize'd. If nothing new: write "None this session."]

## 7. Boot Sequence for New Conversation
> ⚠️ 用户无需额外操作。对话初始化（每次对话必然执行）会自动检测 focus.md 中的 session-snapshots/ 路径，读取本文件，并给出接续选项。用户回复「继续」即可。

If read by boot, announce: "Checkpoint loaded. Resuming: [one-line description]. Next action: [exact first action]."
```

---

## Quality Checklist (Bot self-check before writing)

Before writing the file, verify:
- [ ] Section 3 entries are based on **observed evidence**, not assumption (e.g., "user said X" not "user probably understood")
- [ ] Section 5 "Immediate next action" is specific enough that a new Bot can execute without asking for clarification
- [ ] Section 7 boot sequence is correct and does NOT instruct user to copy-paste anything
- [ ] No raw conversation excerpts pasted — all content is synthesized
- [ ] File path uses today's date and a meaningful topic slug

---

## Notes

- This skill does NOT update `state/focus.md`, `logs/`, or `memory/MEMORY.md` — those are handled by `/save` (git-式 commit)
- If the user says "存档然后收工" or "checkpoint then save" — run `/checkpoint` first, then `/save`
- Checkpoint resumption is handled automatically by the boot initialization — no manual `/checkpoint resume:` command needed
