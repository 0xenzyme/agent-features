# Codex CLI

```yaml
agent: codex
surface: cli
product: Codex CLI
version: 0.160.0
released_at: 2026-10-01
researched_at: 2026-10-05
status: 已填写
docs:
  - https://learn.chatgpt.com/docs/codex/cli
  - https://learn.chatgpt.com/docs/changelog
```

未查证项：`wire_api = "chat"` 的删除版本；TUI 是否呈现 `tool/requestUserInput` 问卷；`.codex/skills` 旧路径是否仍被扫描。

2026-10-05 回填：发现表按范围穷举，上面那六条路径加 `.codebuddy/skills`、`~/.codebuddy/skills` 都不在表里，全部记为不读取。详见第 3 节。

## 1. 身份与版本

- 官方名：Codex CLI。
- 表面：终端。交互会话，以及 `codex exec` 非交互运行。
- 版本：0.160.0。changelog 的 October 2026 节把它列为 2026-10-01 的稳定版。同页后续可见的是 alpha，不作为本档案版本。
- 安装：`https://chatgpt.com/codex/install.sh`、`install.ps1`、`npm install -g @openai/codex`、`brew install --cask codex`。
- 查证日期：2026-10-05。

来源：https://learn.chatgpt.com/docs/changelog ，https://learn.chatgpt.com/docs/codex/cli

## 2. 指令文件与优先级

Codex 在一次运行开始时组装指令链。TUI 里通常是每个启动的会话一次。

1. 全局：`CODEX_HOME` 默认 `~/.codex`。有 `AGENTS.override.md` 就只用它，否则用 `AGENTS.md`。这一层只用第一个非空文件。
2. 项目：从 Git 根走到当前目录。找不到项目根时只查当前目录。每一层按 `AGENTS.override.md`、`AGENTS.md`、然后 `project_doc_fallback_filenames` 的顺序，最多纳入一个文件。
3. 合并：从根往下拼接。离当前目录更近的文件出现在后面，因此覆盖更早的指导。

空文件跳过。合并体积默认停在 `project_doc_max_bytes` 的 32 KiB。

`CLAUDE.md` 不在默认发现名单里。要让别的文件名生效，把它写进 `project_doc_fallback_filenames`。

`CODEBUDDY.md`：不读取。默认发现名单只有 `AGENTS.md` 和 `AGENTS.override.md`，`CODEBUDDY.md` 不在里面。它也不能算「文档没提到」——文档写清了默认名单，只是这条路留了口子：把 `CODEBUDDY.md` 写进 `project_doc_fallback_filenames` 就能生效。`/import` 可以把 Claude Code 的 `CLAUDE.md` 导入为 `AGENTS.md`，这是迁移，不是运行时直接读取。`/init` 在当前目录生成 `AGENTS.md`。

来源：https://learn.chatgpt.com/docs/agent-configuration/agents-md ，https://learn.chatgpt.com/docs/developer-commands ，https://learn.chatgpt.com/docs/app-server

## 3. Skill 目录

Skill 是带 `SKILL.md` 的目录。`name` 和 `description` 必填。Codex 先看到名字、描述和路径，选中后才读全文。初始清单最多占上下文窗口的 2%，未知窗口时最多 8,000 字符。

| 范围 | 路径 |
| --- | --- |
| 仓库 | 从当前目录向上到仓库根的每一层 `.agents/skills` |
| 用户 | `~/.agents/skills` |
| 管理员 | `/etc/codex/skills` |
| 系统 | 随 Codex 捆绑 |

同名 skill 不合并，选择器里可以同时出现。支持符号链接。`[[skills.config]]` 可按 `SKILL.md` 路径关闭某个 skill。

发现表按范围穷举，表里没有 `.claude/skills`、`~/.claude/skills`、`.cursor/skills`、`~/.cursor/skills`、`.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`。这八条记为不读取。`.codex/skills` 和 `~/.codex/skills` 是另一回事：2025-12-19 的 changelog 用过它们，现行发现表没有，旧路径是否仍生效是未查证。

插件是分发单位，可以打包 skill、MCP 和 hooks。CLI 可以使用插件。

显式调用：`/skills`，或在提示里写 `$skill-name`。也可以按描述隐式选用。

来源：https://learn.chatgpt.com/docs/build-skills ，https://learn.chatgpt.com/docs/skills-and-plugins

## 4. 内置 skill 与内置功能

- 捆绑 skill：文档点名 `skill-creator` 和 plan skills，放在 SYSTEM 范围。`$skill-installer` 是文档给出的本地安装入口。
- plan mode：有。`/plan` 进入计划模式，可以带第一句规划提示。任务进行中暂时不能用。`plan_mode_reasoning_effort` 可单独设置该模式的推理力度。
- workflow：没有同名编排器。可重复流程写成 skill 或插件。脚本和 CI 用 `codex exec`。
- 定时任务：无管理界面。文档要求到 ChatGPT 网页或桌面应用里创建和查看。CLI 可以先帮你准备提示、skill 或脚本。
- memory：有，默认关。`[features] memories = false`，成熟度是 Experimental。交互会话用 `/memories`。文件在 `~/.codex/memories/`。这不是 `AGENTS.md` 的替代。
- goals：有。`/goal` 给当前任务一个持续目标。`features.goals` 默认 true。
- 插件市场：未查证。插件本身有，见第 3 节。有没有「从远程市场添加并安装」这一层（其它家写成 `/plugin marketplace add` 之类），本次读不到文档正文：learn.chatgpt.com 的技能与插件页是客户端渲染，抓不到条目，只拿到导航。缺的是一份能读到正文的插件页。

来源：https://learn.chatgpt.com/docs/build-skills ，https://learn.chatgpt.com/docs/developer-commands ，https://learn.chatgpt.com/docs/config-file/config-basic ，https://learn.chatgpt.com/docs/automations ，https://learn.chatgpt.com/docs/customization/memories

## 5. Tools

- 向用户提问：审批有。`approval_policy` 和 `/permissions` 决定何时停下来要批准。沙箱外的文件修改、需要网络的命令，以及带副作用的 app/MCP 调用，会走批准。自由问卷未查证。app-server 有实验方法 `tool/requestUserInput`，一次 1 到 3 个短问题，可带自由输入项。CLI 的命令页没有对应的独立命令。
- shell：有。`features.shell_tool` 默认 true。除 Windows 外，`features.unified_exec` 默认 true，使用 PTY 执行工具。
- 文件编辑：有。hooks 的工具匹配名单包含 `apply_patch`。本档案不抄该工具的参数。
- 子 agent：有。见第 10 节。`features.multi_agent` 默认 true。
- MCP：有。见第 8 节。
- 浏览器：CLI 页没有应用内浏览器。custom agent 示例里的浏览器证据走 MCP，不是内置浏览器。
- 网页搜索：有。`web_search` 默认 `cached`，也可设 `indexed`、`live` 或 `disabled`。`--search` 等于 live。
- 图像：changelog 在 CLI 0.158.0 写入图像生成和编辑。截至 0.160.0 的 changelog 没有写移除。
- 外部消息渠道：未查证。文档写了定时和事件触发的任务，但那套在 ChatGPT 网页、移动端和桌面应用里管理，不是把 IM 或 webhook 事件推进正在运行的本地会话。文档没有写这类机制，也没有写「没有」。缺的是一份写明有或没有的页面。
- 常驻守护进程：未查证。文档里的入口是交互 TUI、`codex exec` 一次性运行，以及 app-server。app-server 是另一个入口，本轮不建表面（见 [Codex README](../README.md)），不能拿它填这一格。有没有可注册成系统服务的常驻进程，文档没写。

来源：https://learn.chatgpt.com/docs/agent-approvals-security ，https://learn.chatgpt.com/docs/app-server ，https://learn.chatgpt.com/docs/hooks ，https://learn.chatgpt.com/docs/config-file/config-basic ，https://learn.chatgpt.com/docs/changelog ，https://learn.chatgpt.com/docs/plugins ，https://learn.chatgpt.com/docs/automations

## 6. 配置

用户级文件是 `~/.codex/config.toml`。项目级是 `.codex/config.toml`，只在项目被信任后加载。不信任的项目会跳过项目级 `.codex/`，包括项目配置、hooks 和 rules。

优先级从高到低：

1. CLI 标志和 `--config`
2. 项目 `.codex/config.toml`，从根到当前目录，越近越优先
3. `--profile` 选中的 `~/.codex/<name>.config.toml`
4. `~/.codex/config.toml`
5. 已登录工作区下发的云托管默认配置
6. Unix 上的 `/etc/codex/config.toml`
7. 内置默认

项目级文件不能覆盖 `openai_base_url`、`chatgpt_base_url`、`model_provider`、`model_providers`、`notify`、`profile`、`profiles`、`otel`。

CLI 和 IDE extension 共用这套配置层。桌面应用是否读取每一个键，见 app 档案。

来源：https://learn.chatgpt.com/docs/config-file/config-basic ，https://learn.chatgpt.com/docs/config-file/config-reference

## 7. BYOK 与 API 格式

- 自带 key：有。`printenv OPENAI_API_KEY | codex login --with-api-key`。ChatGPT 登录走 `codex login`。企业自动化还可以用 `codex login --with-access-token`。
- 计费：ChatGPT 登录使用套餐额度。API key 按 OpenAI Platform 的 API 价格计费，不使用套餐包含额度。依赖 ChatGPT 工作区或云服务的功能会受限。
- base URL：有。用户级 `openai_base_url` 改内置 OpenAI 提供商。自定义提供商用 `base_url`。
- 任意模型：限制。可以指定模型名，也可以接 Responses 兼容的自定义提供商。当前版本不接受 Chat Completions 专用端点，也不接受 Anthropic Messages。ChatGPT 登录时可选模型还受套餐和工作区目录限制。
- 线上协议：`responses`。时间线见 [Codex README](../README.md)。
- 凭证：自定义提供商可设 `requires_openai_auth = true`，或用 `env_key` 指向环境变量。两者都不设时，Codex 把该提供商当成不需要认证。

来源：https://learn.chatgpt.com/docs/auth ，https://learn.chatgpt.com/docs/models ，https://learn.chatgpt.com/docs/config-file/config-reference ，https://learn.chatgpt.com/docs/enterprise/gateway-compatibility

## 8. MCP

支持 stdio 和 streamable HTTP。HTTP 可用 bearer token 或 OAuth。

配置在 `config.toml` 的 `mcp_servers`。用户级和受信任项目级都可以写。CLI、桌面应用和 IDE extension 共用这份 MCP 配置。

CLI 用 `codex mcp add`、`codex mcp list`、`codex mcp login`。会话里用 `/mcp` 查看工具。

原先把 Codex 自己暴露成 MCP server 的 `codex mcp-server` 已移除。外部 MCP server 仍然可用。

来源：https://learn.chatgpt.com/docs/extend/mcp ，https://learn.chatgpt.com/docs/mcp-server

## 9. 权限与沙箱

沙箱包住命令，不只包住内置文件操作。批准策略决定何时询问。

文档中的沙箱模式：`read-only`、`workspace-write`、`danger-full-access`。`workspace-write` 默认没有网络，要用 `sandbox_workspace_write.network_access` 打开。

文档中的批准策略：`on-request`、`never`。`approval_policy = "untrusted"` 已退役，留着可能导致客户端无法启动。

启动建议：有版本控制的目录用 Auto，也就是 workspace write 加 on-request。没有版本控制的目录用 read-only。目录未被信任前也可能停在 read-only。

规则文件是实验功能。放在配置层旁边的 `rules/` 目录，例如 `~/.codex/rules/default.rules`，用来决定哪些命令可以在沙箱外运行。

Windows 原生沙箱在 `[windows] sandbox` 里选 `elevated` 或 `unelevated`。

来源：https://learn.chatgpt.com/docs/agent-approvals-security ，https://learn.chatgpt.com/docs/agent-configuration/rules ，https://learn.chatgpt.com/docs/config-file/config-basic

## 10. 子 agent

有，并且当前版本默认开启。CLI 里直接要求委托，或由 `AGENTS.md` 和 skill 要求委托。`/agent` 在线程之间切换。

子 agent 继承当前沙箱。交互会话里，别的线程的批准请求也会弹出来。非交互运行遇到新的批准就失败，并把错误交回父流程。生成子 agent 时会重新套用父回合的运行时覆盖，包括 `/permissions` 和 `--yolo`，即使 custom agent 文件写了别的默认值。

内置 agent：`default`、`worker`、`explorer`。自定义文件放在 `~/.codex/agents/*.toml` 或 `.codex/agents/*.toml`。必填 `name`、`description`、`developer_instructions`。未写的 `sandbox_mode`、`mcp_servers`、`skills.config` 继承父会话。

来源：https://learn.chatgpt.com/docs/agent-configuration/subagents

## 11. Hooks

有。`features.hooks` 默认 true，成熟度 Stable。

位置：`~/.codex/hooks.json`、`~/.codex/config.toml` 的内联 `[hooks]`、仓库 `.codex/hooks.json`、仓库 `.codex/config.toml`。多层匹配的 hook 都会运行，高优先级层不替换低优先级层。插件也可以带 `hooks/hooks.json`。

`/hooks` 查看来源、信任新 hook，或关闭单个非托管 hook。未信任的 hook 不运行。

现行事件包括 `PreToolUse`、`PermissionRequest`、`PostToolUse`、`PreCompact`、`PostCompact`、`UserPromptSubmit`、`SubagentStart`、`SubagentStop`、`SessionStart`、`Stop`。文档还写了 `SessionEnd` 和 `Interrupt` 的超时。支持 command 和 `mcp_tool`。`prompt` 和 `agent` 处理器会被解析后跳过。

来源：https://learn.chatgpt.com/docs/hooks ，https://learn.chatgpt.com/docs/config-file/config-basic

## 12. 分叉

相对 [桌面应用里的 Codex](../app/profile.md)：

- 同一套 `AGENTS.md`、`.agents/skills` 和 MCP `config.toml` 会被两边看到。
- 定时任务只在桌面应用或网页上创建。CLI 没有 Scheduled 管理界面。
- CLI 有 `codex exec`、TUI 和 `/hooks`。桌面应用用自己的命令和设置界面。
- 两边的 Codex 版本可以不同。按 CLI 0.160.0 写的行为，不能直接当成桌面应用当前构建的行为。
- 只实现 Chat Completions 或 Anthropic Messages 的端点，两边的现行文档都不接受。

来源：本节汇总前面各节。版本差另见 https://learn.chatgpt.com/docs/reference/troubleshooting

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Codex CLI | https://learn.chatgpt.com/docs/codex/cli | 2026-10-05 |
| Changelog | https://learn.chatgpt.com/docs/changelog | 2026-10-05 |
| AGENTS.md | https://learn.chatgpt.com/docs/agent-configuration/agents-md | 2026-10-05 |
| Developer commands | https://learn.chatgpt.com/docs/developer-commands | 2026-10-05 |
| Build skills | https://learn.chatgpt.com/docs/build-skills | 2026-10-05 |
| Skills and plugins | https://learn.chatgpt.com/docs/skills-and-plugins | 2026-10-05 |
| Config basics | https://learn.chatgpt.com/docs/config-file/config-basic | 2026-10-05 |
| Config reference | https://learn.chatgpt.com/docs/config-file/config-reference | 2026-10-05 |
| Authentication | https://learn.chatgpt.com/docs/auth | 2026-10-05 |
| Models | https://learn.chatgpt.com/docs/models | 2026-10-05 |
| Gateway compatibility | https://learn.chatgpt.com/docs/enterprise/gateway-compatibility | 2026-10-05 |
| MCP | https://learn.chatgpt.com/docs/extend/mcp | 2026-10-05 |
| Approvals and security | https://learn.chatgpt.com/docs/agent-approvals-security | 2026-10-05 |
| Rules | https://learn.chatgpt.com/docs/agent-configuration/rules | 2026-10-05 |
| Subagents | https://learn.chatgpt.com/docs/agent-configuration/subagents | 2026-10-05 |
| Hooks | https://learn.chatgpt.com/docs/hooks | 2026-10-05 |
| Memories | https://learn.chatgpt.com/docs/customization/memories | 2026-10-05 |
| Scheduled tasks | https://learn.chatgpt.com/docs/automations | 2026-10-05 |
| App server | https://learn.chatgpt.com/docs/app-server | 2026-10-05 |
| Plugins | https://learn.chatgpt.com/docs/plugins | 2026-10-05 |
| Non-interactive mode | https://learn.chatgpt.com/docs/non-interactive-mode | 2026-10-05 |
