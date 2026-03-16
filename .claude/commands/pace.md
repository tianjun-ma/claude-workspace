---
description: "颗粒度/节奏筛选：对当前主题输出三档解释，用户选一档后锁定节奏直到重新触发。触发词：/pace、调节节奏、选粒度。"
---

# /pace — Granularity Selector

**Trigger words:** `/pace`, "调节节奏", "选粒度", "换个颗粒度", "太深了/太浅了"

---

## Design Philosophy

**Core idea: 显化强制 AI 思考用户需求。**

Default behavior is for the AI to pick a granularity and run with it. `/pace` interrupts that and forces a three-way split: the AI must actually prepare all three levels before presenting options, not just label the same content differently. The user's selection becomes a session contract — the AI may not silently drift back to its default depth.

This works in any mode. The three levels adapt to context:

| Context | Level 1 (TL;DR) | Level 2 (Standard) | Level 3 (Deep Dive) |
|---------|-----------------|---------------------|----------------------|
| Teaching | One-liner + comparison table | Scene + definition + table + one example | Full walkthrough + worked example + traps + verification |
| Architecture/Design | Decision answer + 1-line rationale | Trade-off table + constraints | All trade-offs + failure modes + historical context |
| Code/Solve | Answer + pattern name | Step-by-step breakdown | Line-by-line + edge cases + complexity analysis |
| Concept explanation | One sentence what + why | Definition + analogy + one use case | Formal definition + counterexamples + connections to adjacent concepts |

---

## Execution Steps

### Step 1 — Identify the current topic

Extract the topic the user is currently working on from conversation context. If ambiguous, ask: "当前主题是 [X]，对吗？" and wait for confirmation before proceeding.

### Step 2 — Generate three previews

For the **same current topic**, produce three versions in a single response:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
/pace — 选一个颗粒度，我会按那个档持续输出

当前主题：[topic name]

[1] TL;DR（30秒版）
[一句话核心结论 + 最小必要信息，≤3行]

[2] Standard（2-3分钟版）
[场景切入 + 核心定义 + 关键点，约一段话预览]

[3] Deep Dive（不限时）
[走完整流程：定义 → 机制 → 例子 → 陷阱 → 验证，预览第一步]

回复 1 / 2 / 3，或 TL;DR / Standard / Deep Dive
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 3 — Lock the selection

After user picks a level:

1. Acknowledge with one line: `已锁定：[Level Name] 模式。接续中……`
2. Immediately continue the explanation at the chosen depth — do not ask further questions.
3. **Session lock**: all subsequent outputs on this and related topics maintain the selected granularity until `/pace` is triggered again.

### Step 4 — Maintain lock silently

- Do not announce the lock on every response.
- Do not drift. If you notice yourself about to exceed the locked depth, compress.
- If the topic shifts to something genuinely different (new concept, new problem), you may ask: "进入新主题了，继续用 [Level] 档吗？" — but only once per major topic shift. Default: continue with locked level.

---

## Level Definitions (context-adaptive)

### Level 1 — TL;DR
Goal: The minimum understanding needed to not be lost.
- One sentence: what it is
- One sentence: why it matters here
- At most one table or list if it aids orientation
- No examples, no history, no edge cases

### Level 2 — Standard
Goal: Functional understanding — can use the concept, can answer exam/interview questions.
- Short concrete scene that motivates the concept
- Definition (formal or semi-formal)
- Key points / structure (3-5 items)
- One representative example
- One common confusion or trap (brief)

### Level 3 — Deep Dive
Goal: Transferable understanding — can adapt under variation, explain to others, spot edge cases.
- Full teaching arc: motivation → definition → mechanism → example → edge cases → verification
- Worked example with intermediate steps shown
- Counterexamples or failure modes
- Connection to adjacent concepts
- Verification question for the user at the end

---

## Protocol Constraints

- **Previews must be real content**, not meta-descriptions. Level 1 preview IS the Level 1 answer. Level 2 and 3 previews are the opening of their respective answers.
- **Do not pick a default.** If the user invokes `/pace` but doesn't pick a level, re-display the menu. Don't proceed.
- **Lock overrides mode-specific depth defaults.** Teaching mode has its own depth rules; `/pace` overrides them for the locked duration.
- **Re-trigger resets.** User invoking `/pace` again starts a fresh selection for the current topic. Previous lock is released.
- **Compliance tag.** After delivering content under a locked level, append at the end of the response:

```
[/pace | 当前档：Level X — Name | 重新触发 /pace 可切换]
```

---

## Edge Cases

**User says "太深了" mid-explanation (without /pace):**
Treat as implicit `/pace` trigger. Pause, show the three-level menu, then let user re-anchor.

**User says "继续" without picking (after /pace menu shown):**
Default to Level 2 (Standard) and note the assumption: `未选择，默认 Standard 档。`

**No clear current topic (e.g., /pace invoked at session start):**
Reply: `还没有进行中的主题。告诉我你想了解什么，我会先给三档预览。`

**User is in quiz/drill mode:**
Level 1 = hint only (no answer). Level 2 = guided decomposition. Level 3 = full explanation + related variants.
