---
name: prompts
description: supai 平台提示词类约定（Agent 行为规范）。向用户提问、需要用户做选择或确认时必须先读：优先用 ask_user_question 卡片（多个问题合并成一张、能给选项就给选项、默认带“其他”），是/否用 confirm，而不是让用户打字回答。
---

# prompts：平台提示词类说明

放平台希望所有 Agent 遵守的行为约定。以后新增的同类说明放在 `references/` 下，并在下表登记。

| 场景 | 读 |
|---|---|
| 向用户提问、让用户选择或确认 | [references/asking-the-user.md](references/asking-the-user.md) |
