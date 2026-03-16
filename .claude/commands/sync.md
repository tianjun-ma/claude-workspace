---
description: "只读快照：拉取最新状态，不写任何文件。随时可用。"
---

你正在执行 `/sync` 只读快照协议。

## 执行步骤

**Step 1** — 读取 `state/focus.md`
**Step 2** — 读取 `memory/MEMORY.md`

## 输出格式

```
[SYNC 快照 | YYYY-MM-DD]
当前阶段：[focus.md 中的阶段]
活跃任务：
  - 主线 0：[状态]
  - 主线 A：[状态]
  - 主线 B：[状态]
待执行项：[列出 focus.md 中未完成的待办]
MEMORY 最新记录：[最后一条纠正/模式]
---
⚠️ 本命令只读，不修改任何文件
```
