---
description: "查看历史日志。用法：/log today | /log week | /log YYYY-MM-DD | /log changes | /log last"
---

你正在执行 `/log` 日志查看协议。根据用户提供的参数执行对应操作。

## 参数映射

| 参数 | 操作 |
|------|------|
| `today` | 读取 `logs/daily/YYYY-MM-DD.md`（今天日期） |
| `week` | 读取当前周的 `logs/weekly/YYYY-WXX.md` |
| `YYYY-MM-DD` | 读取 `logs/daily/YYYY-MM-DD.md`（指定日期） |
| `changes` | 读取 `logs/changelog.md` |
| `last` | 读取 `state/focus.md` 中的"近期会话记录"部分 |
| 无参数 | 询问用户要查看哪种日志 |

## 输出要求

- 直接输出文件内容，不加额外解释
- 若文件不存在，提示"该日期暂无日志记录"
- 只读，不修改任何文件
