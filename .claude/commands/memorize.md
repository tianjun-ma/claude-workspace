# /memorize — 系统规则手动 Push

> 将规则/纠正/偏好注入到系统的**正确位置**，不触发完整 /save 流程。
> 触发词：`/memorize`、"记一下"、"注入到系统"、"注入协议"、"加入协议"

---

## 执行步骤（严格按序，不可跳过）

**Step 1 — 提取内容**
- 从用户当前消息或对话上下文中识别需要注入的规则/纠正/偏好
- 如果不明确，用一句话询问："你想注入的是[X]，对吗？"

**Step 2 — 路由判断**

根据内容类型，选择写入的目标文件：

| 内容类型 | 目标文件 | 示例 |
|----------|---------|------|
| 教学协议规则/补丁 | `.claude/commands/teach.md` | "教学时一次只讲一个概念" |
| 做题协议规则/补丁 | `.claude/commands/solve.md` | "做题前必须读 spec" |
| 工具学习协议补丁 | `.claude/commands/spiral.md` | "Spiral 2 必须有验证步骤" |
| 简历协议补丁 | `.claude/commands/resume.md` | "简历禁用 Spearheaded" |
| 全局行为约束 | `CLAUDE.md` | "所有模式下禁止猜测填空" |
| 路由/索引变更 | `WORKSPACE.md` | "新增 state/xxx.md 路由" |
| Bot 行为纠正/偏好/模式发现 | `memory/MEMORY.md` | "出错不用轻松语气" |
| 其他 skill 补丁 | 对应 `.claude/commands/<skill>.md` | skill 特定规则 |

**判断原则：** 规则属于哪个协议就写进哪个协议文件。只有"跨协议通用"或"不属于任何特定协议"的内容才写 MEMORY.md。

**Step 3 — 读取目标文件 → 找到合适位置 → 写入**

- 读取目标文件
- 找到语义上最匹配的 section（如 teach.md 的 Phase 2、MEMORY.md 的行为纠正记录）
- 追加规则，格式：
  - 协议文件：直接融入现有结构（加粗标注、列表项、或新增子 section）
  - MEMORY.md：`- **[YYYY-MM-DD]** [内容精炼，含触发场景+规则+执行要求]`

**Step 4 — 确认输出**

```
✅ 已注入系统
→ 目标文件: [文件路径]
→ 写入位置: [section 名或行号附近]
→ 内容: [写入的规则原文，一句话]
```

---

## 约束（不可违反）

- ❌ 不更新 `logs/daily/`、`logs/weekly/`、`state/focus.md`、`logs/changelog.md`
- ❌ 不做完整 /save 流程
- ✅ 每次只写一个目标文件的一处修改（原子操作）
- ✅ 写完立即报告，不等用户追问
- ✅ 如果目标文件需要新建 section，section 标题用 `## ` 开头

---

## 典型触发场景

```
用户："教学时否定辨析要问'如果缺失会怎样'，加入协议"
Bot：路由 → teach.md Phase 2 否定辨析行 → 追加规则 → 确认

用户："以后问工具问题先搜索，记一下"
Bot：路由 → MEMORY.md 行为纠正记录 → 追加 → 确认

用户："做题模式必须先读 spec 文档"
Bot：路由 → solve.md Step 0 → 追加前置规则 → 确认

用户："/memorize CLAUDE.md 加一条：缩写首次出现必须配全称+中文"
Bot：路由 → CLAUDE.md → 追加到硬性约束 → 确认
```
