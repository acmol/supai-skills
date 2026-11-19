# Task 内可用的平台 API 工具

Agent 在 Task 内通过 MCP 工具使用平台能力。凭证是当前 Task 的 Token（代表该 Task 的属主），**不要把 Token 打印或发给用户**。身份始终是本 Task 的属主，不接受请求里自报的身份。

## 直接可用的 7 个工具（`supai-` 前缀）

| 工具 | 作用 |
|---|---|
| `create_task` | 创建子 Task（飞书里新话题）；**只有调度者（主 Task）或用户明确要求时才调用**，ack 必须是固定文本 |
| `submit_result` | 子 Task 把结果回报给父 Task（主 Task 不可用） |
| `wait_task` | 登记等待另一个 Task 退出 |
| `get_task_output` | 读取 Task 的消息记录 |
| `list_tasks` | 列出自己的最近 Task（任务树） |
| `supai_tool_search` | 按关键词搜索下面的平台操作，得到名称和参数 |
| `supai_tool_call` | 按名称调用平台操作 |

## 通过 supai_tool_search / supai_tool_call 调用的操作（`/supai/v1/`）

| 类别 | 操作 |
|---|---|
| Task | `list_tasks`、`get_task`、`get_task_messages`、`post_task_message`（以 Web 用户身份发消息，可带文件） |
| 文件 | `send_file`（把文件发到 Task 所在飞书会话，≤30 MB）、`download_file`、`download_attachment` |
| 环境变量 | `list_task_env`、`set_task_env`、`delete_task_env` |
| GitHub | `connect_github`（发起授权，把链接和验证码发到会话，同 `/connect github`） |
| Web | `get_web_login_link`（一次性登录链接，5 分钟、仅一次、给本人） |
| 定时器 | `create_timer`、`list_timers`、`get_timer`、`pause_timer`、`resume_timer`、`delete_timer`（同 `/timer`） |
| SSH 密钥 | `get_my_ssh_key`、`regenerate_my_ssh_key`（不返回私钥） |
| 管理员 | `restart_service`、`get_deploy_status`：仅当 Task 属主是管理员才允许（否则 403） |

规则：
- 只能访问自己属主名下的 Task；越权和不存在都表现为 404。
- 部署类操作（`restart_service`）**不要主动调用**，也不要替普通用户调用；用户询问时让其联系管理员。
- 受限的调度器主 Task 只有读文件工具和上述 7 个平台工具，没有 shell/写文件。
- 常见错误码：401 Token 无效/已撤销；409 非当前 generation；404 无权或不存在；413 文件过大；429 并发已达上限；502 飞书调用失败；503 未配置飞书。
