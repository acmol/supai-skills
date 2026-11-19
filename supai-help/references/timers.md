# /timer 定时任务

按 crontab 到点触发，由属主在飞书里管理。Web 管理端暂不支持 Timer。Agent 也可通过平台操作 `create_timer` / `list_timers` / `get_timer` / `pause_timer` / `resume_timer` / `delete_timer` 管理（效果与飞书命令相同，见 [platform-api.md](platform-api.md)）。

## 语法

```
/timer add once|repeat [--in-task|--in-task-plus] [task=N] [max=N] [tz=Area/City] <分 时 日 月 周> | <提示词>
/timer list
/timer show <编号>
/timer pause <编号>
/timer resume <编号>
/timer del <编号>
```

- `once`：只触发一次；`repeat`：持续触发。
- 5 段 crontab：`分 时 日 月 周`；`|` 之后是触发时发给 Agent 的提示词。
- 时区默认 `Asia/Shanghai`，可用 `tz=` 指定。
- 管理（pause/resume/del）只有创建者或管理员可以操作。
- 限制（默认值，管理员可改）：repeat 两次触发的最小间隔 300 秒；每个会话最多 20 个未结束的定时任务；提示词最长 20000 字符。

## 三种模式（默认 new_task）

| 模式 | 写法 | 每次触发做什么 | Task 已停止时 |
|---|---|---|---|
| `new_task`（**默认**） | 不加模式参数 | 新建一个独立的子 Task（新话题、干净上下文）执行提示词 | 不涉及 |
| `in_task` | `--in-task`（或 `mode=in_task`） | 把提示词投递给**已有的** Task（`task=N` 指定，默认是当前 Task） | **不唤醒**，本次跳过；连续跳过 3 次自动暂停并通知创建者 |
| `in_task_plus` | `--in-task-plus`（或 `mode=in_task_plus`） | 同 in_task | **会唤醒**已停止的 Task（易浪费 token，仅在确需同一上下文持续工作时使用） |

补充：
- `max=N` 只用于 new_task：该定时任务同时存活（running/idle）的 Task 上限，默认 1，最大 5；超限的这次触发被跳过。
- `task=N` 只用于 in_task / in_task_plus，且必须是本会话里属于你、未被清空的 Task；`max=` 不能用于这两种模式。
- 上一轮投递还没处理完时，in_task 两种模式会跳过本次。
- 一次性定时任务遇到并发上限或开话题失败时会自动重试（默认每 5 分钟，最多 12 次）。
- 默认请用 new_task。不确定时推荐 new_task。

## 示例

```
/timer add repeat 0 9 * * 1-5 | 读 roadmap 和任务卡片，决定要做什么，创建子 Task 干活，更新卡片
/timer add repeat max=2 */30 * * * * | 汇报进展
/timer add repeat --in-task */30 * * * * | 继续汇报
```

（`/timer` 或 `/timer help` 会回复用法。）

## 常见问题

- “创建者与本会话主 Task 的属主不一致”：new_task 的新 Task 沿用主 Task 属主，非属主不能给别人的会话建。
- “运行中的任务已达上限”：并发满了，先 `/stop` 不用的 Task，或等待。
- 定时任务没触发：`/timer list` 看状态（active/paused）和下次触发时间；`/timer show <编号>` 看上次触发状态；in_task 连续跳过会被自动暂停，用 `/timer resume <编号>` 恢复。
- 在 Web 里发 `/timer` 只会提示去飞书使用。
