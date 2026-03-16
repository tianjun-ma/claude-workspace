---
description: "对话结束时的 git-commit：更新 daily/weekly/focus/changelog/MEMORY。触发词：收工/结束/更新log/今天先到这里/帮我记录今天的日志。"
---

你正在执行 `/save` git-commit 协议。按以下顺序执行，所有文件修改前必须说明方案并等待用户 approve。

## 执行步骤

**Step 0 — Pre-flight 待办清扫（必须最先执行，不可跳过）**

扫描本次对话中所有被 deferred/pending 的事项（关键词："之后做"/"quiz后"/"以后"/"暂缓"/"记一下但先不做"），输出如下表格：

```
| # | 事项 | 选项 |
|---|------|------|
| 1 | [事项描述] | A) 存入系统（/memorize） B) 当场执行 |
| 2 | [事项描述] | A) 存入系统（/memorize） B) 当场执行 |
```

用户对每项回复 A 或 B：
- **A → 立即 /memorize 到 MEMORY.md**，不留在上下文
- **B → 当场执行，完成后继续 Step 1**

⚠️ 不允许"列出后问一句'哪个先做'"——必须逐项强制二选一，不允许模糊收尾。
所有项处理完毕后，才进入 Step 1。

**Step 1** — 询问用户今天做了什么（若对话中已有充分记录则直接总结）

**Step 2** — 更新 `logs/daily/YYYY-MM-DD.md`（追加，不覆盖；文件不存在则新建）
- 格式：`## [时间段] — [主题]` + 内容

**Step 3** — 更新 `logs/weekly/YYYY-WXX.md`（追加一行摘要）
- 格式：`| YYYY-MM-DD | [主题] | [一句话概括] | [状态] |`

**Step 4** — 更新 `state/focus.md`（更新停点 + 待办）
- 更新"上次停点"为本次对话结束位置
- 若有未完成的 🎓/🛠️/🔧 任务，必须写 `📂 上下文初始化读取顺序`

**Step 5** — 指针完整性检查
- 本次对话新建或归档了 state/ 或 config/ 文件？→ 必须同步更新 WORKSPACE.md 路由表

**Step 6** — 更新 `logs/changelog.md`（仅当本次修改了 config/ 或 WORKSPACE.md 时）
- 格式：对话ID列用 `YYYY-MM-DD-#N`

**Step 7** — 更新 `memory/MEMORY.md`（仅当本次对话有被纠正或发现新模式时追加）

## Commit 完成后

提醒用户将本对话标题改为 `YYYY-MM-DD-#N — 简短描述`（多主题用 ` + ` 连接）

## #N 分配规则

同一对话窗口内多次 commit 沿用同一个 #N；续接对话也沿用，标题累积追加主题。
