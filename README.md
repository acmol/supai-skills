# supai-skills

supai 平台相关的 Agent 技能。每个技能一个目录，入口为 `SKILL.md`（frontmatter 含 `name` 与 `description`），详细内容放在同目录 `references/` 下按需读取。

| 技能 | 说明 |
|---|---|
| [supai-help](supai-help/SKILL.md) | supai 平台使用手册：命令、Task 生命周期、群聊规则、定时器、/data、Web 管理端、SSH 密钥、平台 API、常见问题与排障 |
| [tavily](tavily/SKILL.md) | 联网搜索 / 网页抽取 / 站点爬取（Tavily 远程 MCP）；需用户在 `/web/skills` 填自己的 Tavily API Key，搜索内容会发往 Tavily |
| [prompts](prompts/SKILL.md) | 平台提示词类约定（Agent 行为规范）；目前含“向用户提问优先用 ask_user_question / confirm 卡片” |
