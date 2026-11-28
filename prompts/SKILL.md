---
name: prompts
description: supai 平台提示词类约定（Agent 行为规范）。向用户提问、需要用户做选择或确认时必须先读：优先用 ask_user_question 卡片（多个问题合并成一张、能给选项就给选项、默认带“其他”），是/否用 confirm，而不是让用户打字回答。写报告（publish_report）前先读：用 format=markdown，结论先行、说人话、多配图（流程图 / 数据条 / 对比表），长内容折叠。
---

# prompts：平台提示词类说明

平台希望所有 Agent 遵守的行为约定，按主题一个文件。管理员改本仓库的文件并在 Web 技能页点“立即同步”后，不用重启服务即生效。

## 索引

| 主题 | 简短版（平台自动注入 say 模式每条消息） | 完整说明 |
|---|---|---|
| 向用户提问、让用户选择或确认 | [inject/ask-user.md](inject/ask-user.md) | [references/asking-the-user.md](references/asking-the-user.md) |
| 写给用户看的报告（publish_report）：结论先行、说人话、多图 | [inject/report.md](inject/report.md) | [references/writing-reports.md](references/writing-reports.md) |

## 约定（给维护者）

- `inject/*.md`：**短**（每个 ≤2KB、全部合计 ≤4KB，超出的会被平台跳过并记错误日志），平台读取后拼进 say 模式每条消息的提示。
- `references/*.md`：详细说明，Agent 按需读取，不自动注入。
- 新增主题：加一对文件并在上表登记。**文件必须在本文件里被引用，否则不会被同步进平台。**
