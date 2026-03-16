---
description: "对话初始化：读取 focus.md + MEMORY.md，输出当前状态报告。对话开始时使用。"
---

你正在执行 `/boot` 初始化协议。按以下顺序执行，不能跳过：

## 执行步骤

**Step 1** — 读取 `state/focus.md`（当前优先级 + 上次停点）

**Step 2** — 读取 `memory/MEMORY.md`（Bot 行为备忘录）

**Step 3** — 若 focus.md 中有未完成任务标注了 `📂 上下文初始化读取顺序`，执行以下逻辑：

1. **优先检查**：列表中是否包含 `outputs/session-snapshots/` 路径
   - **是** → 立即读取该 checkpoint 文件，解析 Section 2（停止点）和 Section 3（概念掌握状态）
   - **否** → 跳过此步
2. **继续**：读取列表中其余所有文件（checkpoint 之外的文件，无论是否检测到 checkpoint 都执行）

**Step 4** — 输出初始化报告（格式如下）：

```
[WORKSPACE 初始化完成]
用户：Tianjun | 当前阶段：[focus.md 中的阶段]
P1 焦点：[focus.md 中的 P1 项]
上次停点：[focus.md 中的下次工作起点]
已加载文件：[列出本次真正调用了 Read 的文件，不能写计划读的]
---
```

**Step 5** — 接续选项输出（根据 Step 3 分支）：

- **若检测到 checkpoint 文件**：
  ```
  🔁 检测到上次会话断点：[checkpoint 文件名中的 topic]
  停止点：[Section 2 的精确停止点，一句话]
  下一步：[Section 5 Pending Items 的 P1 项]

  直接接续？回复「继续」即开始，回复「不」说明本次任务。
  ```

- **若无 checkpoint**：
  询问用户本次任务意图，根据意图建议对应模式。

**重要约束：**
- "已加载文件"只列出本次对话中真正调用了 Read 的文件
- checkpoint 文件读取后，无需用户提供任何额外 prompt，直接进入接续状态
- 用户回复「继续」后，按 checkpoint Section 5 的 P1 项立即执行
