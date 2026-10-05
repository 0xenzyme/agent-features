# ChatGPT desktop app / Codex

```yaml
agent: codex
surface: app
product: ChatGPT desktop app
version: 26.924
released_at: 2026-09-25
researched_at: 2026-10-05
status: 已填写
docs:
  - https://learn.chatgpt.com/docs/app
  - https://learn.chatgpt.com/docs/changelog
```

26.924 是本次在 changelog 里定位到的最近桌面版本，条目是 2026-09-25 的 macOS 安全更新。October 2026 节列出的是 Codex CLI 0.160.0，没有更新的桌面版本号。26.924 之后是否还有未摘录到的构建，未查证。桌面应用内嵌的 Codex 引擎版本也未查证。故障排除页写明，桌面应用和 CLI 可以不是同一个 Codex 版本。

未查证项：内嵌引擎版本；桌面应用是否读取 `config.toml` 的每一个键；桌面应用和 CLI 是否共享同一份 `auth.json`；Codex 聊天里的浏览器和 Computer Use 是否与 Chat 聊天同一套；Codex 聊天是否使用和 CLI 相同的 `web_search` 工具。

2026-10-05 回填：发现表按范围穷举，上面那六条路径加 `.codebuddy/skills`、`~/.codebuddy/skills` 都不在表里，全部记为不读取。详见第 3 节。

## 1. 身份与版本

- 官方名：ChatGPT desktop app。Codex 是里面的一个产品模式，用产品选择器进入。
- 表面：macOS、Windows 桌面应用。Linux 有桌面包。
- 合并：2026-07-09，changelog 条目 26.707。此后更新桌面应用，而不是单独安装旧的 Codex app。
- 下载：https://chatgpt.com/download/ 。Linux 见桌面应用的 Linux 安装页。
- 查证日期：2026-10-05。

来源：https://learn.chatgpt.com/docs/app ，https://learn.chatgpt.com/docs/changelog ，https://learn.chatgpt.com/docs/glossary

## 2. 指令文件与优先级

`AGENTS.md` 的发现规则写在 Codex 文档里，示例命令是 CLI。文档没有为桌面应用另写一套发现算法。按这套规则，全局层在 `~/.codex`，项目层从根走到当前目录，默认文件名不包括 `CLAUDE.md`。桌面应用是否在某个版本改过发现顺序，未查证。

`CODEBUDDY.md`：不读取。沿用 Codex 文档那套发现规则，默认文件名是 `AGENTS.md` 和 `AGENTS.override.md`，`CODEBUDDY.md` 不在里面，桌面页也没有提到这个名字。要让它生效，得写进 `project_doc_fallback_filenames`，桌面应用是否暴露这个设置，未查证。

桌面应用可以从 Claude Code、Claude Cowork 和 Cursor 导入指令、设置、skill、插件、项目和近期工作。导入是迁移进 Codex 文件，不是在运行时改读对方的文件名。

`/init` 在应用 composer 的命令名单里。

来源：https://learn.chatgpt.com/docs/agent-configuration/agents-md ，https://learn.chatgpt.com/docs/developer-commands ，https://learn.chatgpt.com/docs/changelog

## 3. Skill 目录

和 CLI 相同。仓库从当前目录向上扫描 `.agents/skills`，用户级是 `~/.agents/skills`，管理员级是 `/etc/codex/skills`，另外有捆绑的 SYSTEM skill。

桌面应用侧边栏有 Skills，可以查看各项目里的 skill。Codex 聊天用 `$skill-name` 显式调用。`agents/openai.yaml` 给桌面应用提供显示名、图标和依赖。

发现表按范围穷举，表里没有 `.claude/skills`、`~/.claude/skills`、`.cursor/skills`、`~/.cursor/skills`、`.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`。这八条记为不读取。

插件可在桌面应用的 Codex 里安装。插件可以带 skill、MCP 和 hooks。

来源：https://learn.chatgpt.com/docs/build-skills ，https://learn.chatgpt.com/docs/skills-and-plugins

## 4. 内置 skill 与内置功能

- 捆绑 skill：与 CLI 相同，文档点名 `skill-creator` 和 plan skills。
- plan mode：有。提示文档写明在应用 composer 里输入 `/plan`，让 Codex 先调查并提出做法，再编辑。
- workflow：没有同名编排器。可重复流程用 skill、插件，或桌面应用的定时任务。
- 定时任务：有。侧边栏 Scheduled 管理任务。桌面应用的任务可以跑在本地项目目录或隔离的 Git worktree 里，电脑需要开着，应用需要运行。Codex 聊天可以创建或更新定时任务。事件触发的任务只在 ChatGPT 网页和移动端，桌面应用没有。
- memory：有，默认关。设置里的 Personalization 打开 Enable memories，或在 `config.toml` 设 `[features] memories = true`。当前聊天用 `/memories`。本地记忆存在 `~/.codex/memories/`，和 ChatGPT 网页记忆不是同一份。
- goals：命令页把 `/goal` 列在应用 composer 的 slash 命令里。`features.goals` 的默认值写在 Codex 配置页，桌面应用是否暴露同一个开关，未查证。
- 插件市场：未查证。插件能在桌面应用的 Codex 里安装，见第 3 节。有没有远程市场这一层，本次读不到文档正文，理由同 [CLI 档案](../cli/profile.md) 第 4 节。

来源：https://learn.chatgpt.com/docs/prompting ，https://learn.chatgpt.com/docs/automations ，https://learn.chatgpt.com/docs/customization/memories ，https://learn.chatgpt.com/docs/developer-commands ，https://learn.chatgpt.com/docs/build-skills

## 5. Tools

- 向用户提问：批准有。子 agent 继承 composer 下方选定的权限模式。自由问卷未查证。app-server 的 `tool/requestUserInput` 是实验协议，用户文档没有写桌面 composer 会弹出 1 到 3 个问题的表单。
- shell：有。Codex 模式会显示 shell 和技术细节。具体工具名沿用 Codex 的 `shell` 和 `apply_patch`，见 CLI 档案。桌面构建若内嵌更老的引擎，工具集合可能不同。
- 文件编辑：有。Codex 模式提供 diff 和 review。
- 子 agent：有。见第 10 节。
- MCP：有。见第 8 节。
- 浏览器：部分。桌面应用有浏览器，文档也有 Computer Use 和 in-app browser 页面。这些页面没有按 Chat 和 Codex 两种模式拆开，所以 Codex 聊天里是否同一套，未查证。
- 网页搜索：未查证。`web_search` 写在 Codex 配置里。配置页明确共享配置层的是 CLI 和 IDE extension，没有点名桌面应用。
- 外部消息渠道：未查证。桌面应用有定时和事件触发的任务，事件触发的任务只在 ChatGPT 网页和移动端，桌面应用没有。把 IM 或 webhook 事件推进正在运行的本地会话，文档没有写这类机制，也没有写「没有」。
- 常驻守护进程：未查证。桌面应用本身是常驻 GUI，但定时任务那节写明电脑要开着、应用要运行。有没有脱离应用窗口的后台进程或可注册的系统服务，文档没写。

来源：https://learn.chatgpt.com/docs/app ，https://learn.chatgpt.com/docs/use-chatgpt ，https://learn.chatgpt.com/docs/agent-configuration/subagents ，https://learn.chatgpt.com/docs/app-server ，https://learn.chatgpt.com/docs/config-file/config-basic ，https://learn.chatgpt.com/docs/plugins ，https://learn.chatgpt.com/docs/automations

## 6. 配置

MCP 明确与 CLI、IDE extension 共用 `~/.codex/config.toml` 和受信任项目的 `.codex/config.toml`。

Config basics 写「CLI 和 IDE extension 共用同一套配置层」，这句话没有点名桌面应用。因此 base URL、沙箱和 feature flag 是否全部由桌面应用读取，未查证。

登录：桌面应用支持 ChatGPT 登录和 API key。文档明确 CLI 与 IDE extension 共用同一份缓存登录。桌面应用是否读写同一份 `~/.codex/auth.json`，未查证。

来源：https://learn.chatgpt.com/docs/extend/mcp ，https://learn.chatgpt.com/docs/config-file/config-basic ，https://learn.chatgpt.com/docs/auth

## 7. BYOK 与 API 格式

- 自带 key：有。未登录屏幕选择 Sign in another way，输入 OpenAI API key。
- 计费：和 CLI 一样。API key 走 Platform API 价格。依赖 ChatGPT 工作区的功能会受限。API key 登录可以使用 OpenAI 策展的一部分插件，需要不支持的 OAuth 的插件不可用。
- base URL：键写在用户级 `config.toml`。桌面应用是否提供对应设置，未查证。
- 任意模型：限制。模型页把当前 Codex 版本的协议限制同时用于桌面应用和 CLI。只接受 Responses。ChatGPT 登录还受套餐模型目录限制。
- 线上协议：`responses`。时间线见 [Codex README](../README.md)。

来源：https://learn.chatgpt.com/docs/auth ，https://learn.chatgpt.com/docs/models ，https://learn.chatgpt.com/docs/plugins

## 8. MCP

有。设置里打开 MCP servers，添加 stdio 或 streamable HTTP，保存后重启。

这份配置和 CLI 共用。项目级配置仍然要求项目受信任。

来源：https://learn.chatgpt.com/docs/extend/mcp

## 9. 权限与沙箱

沙箱文档把桌面应用、IDE 和 CLI 放在同一套模型里。命令跑在约束环境中。越界时走批准。

桌面应用、CLI 和 IDE 的默认 `workspace-write` 都不开网络，除非配置打开。

子 agent 使用父回合在 composer 下方选定的权限模式。单个 custom agent 仍可把 `sandbox_mode` 写成 `read-only`。

定时任务无人值守，使用默认沙箱。组织允许时，定时任务使用 `approval_policy = "never"`。组织禁止该值时，退回所选权限模式的批准行为。

来源：https://learn.chatgpt.com/docs/agent-approvals-security ，https://learn.chatgpt.com/docs/agent-configuration/subagents ，https://learn.chatgpt.com/docs/automations

## 10. 子 agent

有，当前本地 Codex 版本默认开启。在 Codex 聊天里直接要求委托，或由 `AGENTS.md` 和 skill 要求。应用会显示每个子 agent 线程。

子 agent 继承当前沙箱策略和 composer 下方的权限模式。自定义 agent 文件与 CLI 相同：`~/.codex/agents/` 和 `.codex/agents/`。

来源：https://learn.chatgpt.com/docs/agent-configuration/subagents

## 11. Hooks

Codex hooks 对本地 Codex 工作流生效，配置位置与 CLI 相同。桌面应用没有单独的 `/hooks` 命令页。CLI 的 `/hooks` 是 TUI 命令。

插件带的 lifecycle hooks 覆盖 Codex runtime，文档把 ChatGPT Work 和 Codex 写在一起。

来源：https://learn.chatgpt.com/docs/hooks ，https://learn.chatgpt.com/docs/skills-and-plugins

## 12. 分叉

相对 [Codex CLI](../cli/profile.md)：

- `AGENTS.md`、`.agents/skills`、custom agents 和 MCP 配置是共享层。
- 定时任务、本地 worktree 上的后台跑法，属于桌面应用。CLI 不能管理 Scheduled。
- 应用内浏览器和 Computer Use 写在桌面应用文档里，不写在 CLI 文档里。
- 不要假设桌面应用的内嵌引擎等于 CLI 0.160.0。
- 仓库里只有 `CLAUDE.md` 时，两边都不会把它当作默认指令文件。

来源：本节汇总前面各节。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| ChatGPT desktop app | https://learn.chatgpt.com/docs/app | 2026-10-05 |
| Changelog | https://learn.chatgpt.com/docs/changelog | 2026-10-05 |
| Glossary | https://learn.chatgpt.com/docs/glossary | 2026-10-05 |
| Troubleshooting | https://learn.chatgpt.com/docs/reference/troubleshooting | 2026-10-05 |
| Use ChatGPT | https://learn.chatgpt.com/docs/use-chatgpt | 2026-10-05 |
| AGENTS.md | https://learn.chatgpt.com/docs/agent-configuration/agents-md | 2026-10-05 |
| Developer commands | https://learn.chatgpt.com/docs/developer-commands | 2026-10-05 |
| Prompting | https://learn.chatgpt.com/docs/prompting | 2026-10-05 |
| Build skills | https://learn.chatgpt.com/docs/build-skills | 2026-10-05 |
| Scheduled tasks | https://learn.chatgpt.com/docs/automations | 2026-10-05 |
| Memories | https://learn.chatgpt.com/docs/customization/memories | 2026-10-05 |
| Authentication | https://learn.chatgpt.com/docs/auth | 2026-10-05 |
| Models | https://learn.chatgpt.com/docs/models | 2026-10-05 |
| MCP | https://learn.chatgpt.com/docs/extend/mcp | 2026-10-05 |
| Subagents | https://learn.chatgpt.com/docs/agent-configuration/subagents | 2026-10-05 |
| Hooks | https://learn.chatgpt.com/docs/hooks | 2026-10-05 |
| Approvals and security | https://learn.chatgpt.com/docs/agent-approvals-security | 2026-10-05 |
| Plugins | https://learn.chatgpt.com/docs/plugins | 2026-10-05 |
