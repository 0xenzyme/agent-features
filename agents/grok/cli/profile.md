# Grok Build / cli

```yaml
agent: grok
surface: cli
product: Grok Build
version: 1.0.46
released_at: 2026-09-30
researched_at: 2026-10-05
status: 已填写
docs:
  - https://docs.x.ai/build/overview
  - https://x.ai/build/changelog
```

截至 2026-10-05 打开的 changelog，最新稳定版是 1.0.46，日期 2026-09-30。该页把这一条标成 Latest，没有标成预发布。npm 包 `@xai-official/grok` 的 latest 也是 1.0.46。其后若有尚未上到 changelog 的版本，未查证。本档案没有用本机 `grok version` 补版本。

公开仓库 main 上的 user guide 比 docs.x.ai 的短页细。changelog 没有把这篇指南钉到 1.0.46。下面若两处不一致，写成未查证，并写出是哪两页。

## 1. 身份与版本

- 官方名：Grok Build。
- 表面：终端。无参数启动交互 TUI。`grok -p` 是同一条命令的无界面运行。`grok agent stdio` 是同一条命令的 ACP，不另建表面。
- 版本：1.0.46。
- 发布日期：2026-09-30。
- 查证日期：2026-10-05。
- 安装：macOS、Linux、Git Bash 是 `curl -fsSL https://x.ai/cli/install.sh | bash`。Windows PowerShell 是 `irm https://x.ai/cli/install.ps1 | iex`。更新命令是 `grok update`。`--alpha` 会离开稳定版，本档案不把它当正文版本。

来源：https://docs.x.ai/build/overview ，https://docs.x.ai/build/cli/reference ，https://x.ai/build/changelog ，https://www.npmjs.com/package/@xai-official/grok

## 2. 指令文件与优先级

docs.x.ai 写，加载顺序是先 `~/.grok/` 里的全局规则，再从仓库根走到工作目录。更深的文件在冲突时优先。Git 仓库外只读工作目录。

每个目录会读 `AGENTS.md`、`Agents.md`、`AGENT.md`、`CLAUDE.md`、`Claude.md`、`CLAUDE.local.md`，以及 `.grok/rules/`、`.claude/rules/`、`.cursor/rules/` 里的 `*.md`。被 `.gitignore` 忽略的文件跳过。短页用 `CLAUDE.local.md` 当这个例子。

`AGENTS.md`：读取。

`CLAUDE.md`：读取。不是「只有没有 `AGENTS.md` 才读」。

`AGENTS.override.md`：不读取。user guide 写，顶层指令只认上面那组文件名，不认 `AGENTS.local.md` 这类自定名字。`AGENTS.override.md` 不在这组里。

`CODEBUDDY.md`：不读取。user guide 把会读的文件名点名成上面那六个，兼容扫描也只覆盖 `~/.claude/` 和 `~/.cursor/`，没有 `~/.codebuddy`，也没有 `[compat.codebuddy]` 这类开关。

同一目录里的 `AGENTS.md` 和 `CLAUDE.md` 都会加载。谁压过谁，两页都只把「更深的目录优先」写清楚了。同一层的文件名顺序，user guide 列了检查顺序，没有写成覆盖顺序。未查证。

user guide 另外写：

- 家目录规则目录是 `~/.grok/rules/*.md`，只读这一层，不读子目录。兼容扫描默认开时，还有 `~/.claude/rules/` 和 `~/.cursor/rules/`。
- 兼容扫描还会在 `~/.claude/`、`~/.cursor/` 找上面那组文件名，并在每一层找 `.claude/CLAUDE.md` 和 `.claude/CLAUDE.local.md`。
- 项目规则要目录已信任，`--trust` 或交互授权。docs.x.ai 的 AGENTS.md 页没有写这道门。
- `.cursor/rules` 和 `.claude/rules` 只写了 `*.md`。`.mdc` 没有出现。记为不读取。

来源：https://docs.x.ai/build/features/project-rules ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/12-project-rules.md

## 3. Skill 目录

格式是目录加 `SKILL.md`。

docs.x.ai 的发现列表写了 `./.grok/skills/`（走到仓库根）、`~/.grok/skills/`、已启用插件的 `skills/`、`[skills] paths`，以及用户级 `~/.agents/skills/` 和 `~/.agents/commands/`。Claude 的 skill 会读，这页没有把目录名写出来。

user guide 的发现表把目录补全了。同名不合并，高优先级位置覆盖低的。

| 范围 | 路径 |
| --- | --- |
| 项目，随工作目录走到仓库根 | `.grok/skills/`、`.agents/skills/`、`.claude/skills/`、`.cursor/skills/` |
| 用户 | `~/.grok/skills/`、`~/.agents/skills/`、`~/.claude/skills/`、`~/.cursor/skills/` |

Claude 和 Cursor 这两对默认扫描。关掉的键是 `[compat.claude] skills`、`[compat.cursor] skills`，或 `GROK_CLAUDE_SKILLS_ENABLED`、`GROK_CURSOR_SKILLS_ENABLED`。设置参考把这些扫描的默认写成开。

`.codex/skills` 和 `~/.codex/skills` 不在发现表里。不读取。

`.codebuddy/skills` 和 `~/.codebuddy/skills` 不在发现表里，也没有对应的兼容开关。不读取。`.grok/skills` 和 `~/.grok/skills` 在表里，2026-10-05 复核仍是读取。

同一层的 `.grok/skills` 和 `.agents/skills` 谁覆盖谁，user guide 只写 alongside。未查证。

未信任的目录会跳过项目 skill。这份信任是不是 hooks 页的 `~/.grok/trusted_folders.toml`，skills 页没有点名。未查证。

来源：https://docs.x.ai/build/features/skills-plugins-marketplaces ，https://docs.x.ai/build/settings/reference ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/08-skills.md

## 4. 内置 skill 与内置功能

- workflow：有。`/create-workflow` 写 `.grok/workflows/<name>.rhai` 或 `~/.grok/workflows/<name>.rhai`。`/workflow` 启动、暂停、恢复或停止。`/workflows` 是运行仪表。默认开。关掉用 `[workflows] enabled = false` 或 `GROK_WORKFLOWS=0`。`/deep-research` 是内置的研究 workflow。
- plan mode：有。`/plan`、`Shift+Tab`。批准前只有会话计划文件能被编辑工具改。自动批准不跳过计划审阅。shell 仍可能用重定向写文件。
- 定时任务：部分。`/loop` 按间隔重复一条 prompt，立刻跑第一次，每次是新的一轮。间隔最短 60 秒。7 天后过期，同时最多 50 个。`/tasks` 和任务窗格能看到、能取消。进程退出后还跑不跑，这一页没有写。
- memory：有。默认关。`/remember`、`/memory`、`/dream`、`/flush` 在打开后出现。`grok memory clear` 清文件。`--experimental-memory` 打开，`--no-memory` 关掉。文件在 `~/.grok/memory/`。`[memory_v2]` 另用 `~/.grok/memory-v2/`。两套开关 user guide 都写默认关。
- 文档点名的捆绑 skill：modes 页点了内置的 `/deep-research`。没有一页列出随安装附带的 `SKILL.md` 清单。未查证。
- 插件市场：有。user guide 写，要把 skill 分给整个团队或组织，就把它打进插件、通过 marketplace 发布，并指向 09-plugins.md 的「Create your own marketplace」和「Distribute across an organization」。本地插件目录是 `.grok/plugins/`，项目侧配置是 `[plugins]`。官方命令名（`grok plugin marketplace add` 之类）没有在 user guide 里点名，未查证。

来源：https://docs.x.ai/build/modes-and-commands ，https://docs.x.ai/build/features/plan-mode ，https://docs.x.ai/build/features/background-tasks ，https://docs.x.ai/build/cli/reference ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/13-memory.md

## 5. Tools

- 向用户提问（审批）：有。默认是 Ask，未允许的工具调用会问。还有 Auto 和 Always-approve。`deny` 规则和 `PreToolUse` hook 在后两种模式里仍然生效。
- 向用户提问（自由问卷）：有。工具名是 `ask_user_question`。问题卡可以用数字键选题，`z` 进入自由文本行。一次可以有多题。
- shell：有。权限里的名字是 `bash`。
- 文件编辑：有。`write` 默认开，`GROK_WRITE_FILE=0` 关掉。权限里还有 `edit`。
- 子 agent：有。见第 10 节。
- MCP：有。见第 8 节。
- 浏览器操作：未查证。设置参考和打开的功能页没有点名一个操作浏览器的内置工具。缺的是一份写明有或没有该工具的页面。
- 网页搜索：有。客户端工具名是 `web_search`。`web_fetch` 是另一个工具，`GROK_WEB_FETCH` 默认 `0`，默认不拉页面。

清单外：LSP 工具默认关。`/imagine` 和 `/imagine-video` 是模式页上的命令，不是上面的浏览器工具。

- 外部消息渠道：未查证。project-rules、skills、modes、background-tasks 各页都没有点名把 IM 或 webhook 事件推进正在运行的会话的机制，也没有写「没有」。缺的是一份写明有或没有的官方页面。
- 常驻守护进程：未查证。官方点名的是 `grok -p`（无界面）和 `grok agent stdio`（ACP，JSON-RPC over stdio，供 IDE 或脚本接）。没有写可注册成 launchd/systemd 的常驻进程，也没有 `--serve` 这类本地常驻 HTTP 服务。

来源：https://docs.x.ai/build/features/permissions ，https://docs.x.ai/build/settings/reference ，https://docs.x.ai/build/features/dashboard ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/03-keyboard-shortcuts.md ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/19-plan-mode.md

## 6. 配置

用户文件是 `~/.grok/config.toml`。`$GROK_HOME` 可以换家目录。Windows 上用户文件在 `%USERPROFILE%\.grok\config.toml`。

项目 `.grok/config.toml` 只贡献 `[mcp_servers]`、`[plugins]`、`[permission]`。模型、`base_url`、`permission_mode` 不属于这三节。写进项目文件不会变成项目配置。

企业页的五层，从低到高：

| 优先级 | 路径 |
| --- | --- |
| 1 | `/etc/grok/managed_config.toml` |
| 2 | `~/.grok/managed_config.toml` |
| 3 | `~/.grok/config.toml` |
| 4 | `~/.grok/requirements.toml` |
| 5 | `/etc/grok/requirements.toml` |

`requirements.toml` 里钉住的值不能被更低层、用户配置、远程设置或环境变量改掉。这张五层表没有项目文件。项目 MCP 的规则另写在 MCP 页：同名服务器整份替换用户的那一份。环境变量相对用户配置的其余键谁先，五层表没有写。未查证。

来源：https://docs.x.ai/build/settings ，https://docs.x.ai/build/settings/reference ，https://docs.x.ai/build/enterprise ，https://docs.x.ai/build/features/mcp-servers ，https://docs.x.ai/build/features/permissions

## 7. BYOK 与 API 格式

- 自带 key：有。`XAI_API_KEY`，或某个模型上的 `api_key` / `env_key`。浏览器登录是另一条，`grok login`，无浏览器时用 `grok login --device-auth`。
- 订阅和 API 如何切换：按模型解析，顺序是 `model.api_key`、`model.env_key`、当前会话 token、`XAI_API_KEY`。企业可以在 `requirements.toml` 里关 API key 登录。第三方 BYOK 的 `base_url` 不在 `x.ai` 上时，这道开关不停它。
- base URL：有。模型上的 `base_url`。xAI 自己的 API key 基址默认 `https://api.x.ai/v1`，可用 `GROK_XAI_API_BASE_URL` 改。
- 任意模型：有。概览写可以添加任意自定义模型。`[models] allowed_models` 可以再收窄选择器。
- 线上协议：自定义模型的 `api_backend` 是 `responses`、`chat_completions` 或 `messages`。设置页示例把 `grok-4.7` 写成 `responses`。浏览器登录的内置模型默认协议未查证。见 agent README。
- 版本分界：未查证。

来源：https://docs.x.ai/build/overview ，https://docs.x.ai/build/settings ，https://docs.x.ai/build/settings/reference ，https://docs.x.ai/build/enterprise

## 8. MCP

支持。工具和内置工具一起用，名字是 `<server>__<tool>`。

用户服务器写在 `~/.grok/config.toml` 的 `[mcp_servers]`，或 `grok mcp add`。项目用 `grok mcp add --scope project`，写入 `.grok/config.toml`。从当前目录走到 git 根，同名的项目服务器整份替换用户的那一份。

另外会读 `~/.claude.json`、`.cursor/mcp.json` 和项目 `.mcp.json`，优先级低于 `config.toml`。`[compat.claude] mcps = false` 或 `[compat.cursor] mcps = false` 可以关掉。OAuth token 在 `~/.grok/mcp_credentials.json`。

项目 MCP 和项目 hook 一样，要先信任。记录在 `~/.grok/trusted_folders.toml`。

来源：https://docs.x.ai/build/features/mcp-servers ，https://docs.x.ai/build/features/hooks

## 9. 权限与沙箱

权限默认是 Ask。项目 `.grok/config.toml` 可以写 allow、deny、ask。`deny` 压过 allow。`permission_mode` 只能写在用户配置、托管配置或 requirements 里，不能写在项目文件里。

沙箱有，默认关。实现是 Linux 的 Landlock 和 macOS 的 Seatbelt。Windows 上有没有沙箱，这一页没有写。未查证。

档位是 `off`、`workspace`、`devbox`、`read-only`、`strict`。`--sandbox`、`[sandbox] profile`、`GROK_SANDBOX` 选择。自定义档写在 `~/.grok/sandbox.toml` 或项目 `.grok/sandbox.toml`。项目 `config.toml` 不能写 `[sandbox]`，所以项目文件只能定义档，不能靠自己把档选上。

`requirements.toml` 可以钉住沙箱档，并压过命令行。

来源：https://docs.x.ai/build/features/permissions ，https://docs.x.ai/build/features/sandbox ，https://docs.x.ai/build/settings/reference ，https://docs.x.ai/build/enterprise

## 10. 子 agent

有。可以自定义。定义放在 `.grok/agents/` 或 `~/.grok/agents/`。调用工具是 `spawn_subagent`。

docs.x.ai 写，设置未写时默认开启。`explore` 和 `plan` 不跑 shell，也不改文件。user guide 写默认开启，只有显式 `enabled = false` 或 `GROK_SUBAGENTS=0` 才关掉，并且 `explore` 和 `plan` 会跑 shell，只是不改文件。设置参考把 `GROK_SUBAGENTS` 的 Default 写成 `0`。

默认开不开，以及 `explore` / `plan` 能不能跑 shell，这三处不一致。未查证。缺的是一份对上 1.0.46 的说明。

子会话有自己的上下文。是否加载父会话的 `AGENTS.md`，两页都没有写。未查证。是否套用父会话的沙箱，没有写。未查证。

已经连上的 MCP 会传给子会话，user guide 写这是默认。计划模式不限制子会话的编辑工具。权限模式会传下去，包括 Auto 和 Always-approve。这句在 docs.x.ai 的计划模式页。

来源：https://docs.x.ai/build/features/subagents ，https://docs.x.ai/build/features/plan-mode ，https://docs.x.ai/build/settings/reference ，https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/16-subagents.md

## 11. Hooks

有。个人钩子在 `~/.grok/hooks/*.json`。项目钩子在 `.grok/hooks/*.json`，要先 `/hooks-trust` 或 `--trust`。

也会读 Claude Code 的 `.claude/settings.json` 和 Cursor 的 `.cursor/hooks.json`，包括 Cursor 的驼峰事件名。

事件有 `SessionStart`、`SessionEnd`、`UserPromptSubmit`、`PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionDenied`、`Stop`、`StopFailure`、`Notification`、`SubagentStart`、`SubagentStop`、`PreCompact`、`PostCompact`。只有 `PreToolUse` 能拦住工具。

来源：https://docs.x.ai/build/features/hooks

## 12. 分叉

把这份 CLI 指到一个已经为 Codex、Claude Code 或 Cursor 配好的仓库时：

- 只有 `AGENTS.md` 时，Grok 会读。若 user guide 的信任门成立，未信任目录不加载项目规则。docs.x.ai 的短页没有这道门。
- 同时有项目级 `CLAUDE.md` 和 `AGENTS.md` 时，Grok 两份都加载。Codex 两列读 `AGENTS.md`。Claude Code 默认只读 `CLAUDE.md` 文件。同一目录里谁压过谁，Grok 未查证。
- `AGENTS.override.md` 对 Grok 不生效。Codex 两列会读。
- skill 只放在 `.grok/skills` 时，这一列会发现。其他八列都不读这条路径，不能当成它们也会发现。
- skill 只放在 `.claude/skills` 或 `.cursor/skills` 时，这一列默认会发现。Codex 不读这两对路径，Claude Code 不读 `.cursor/skills`。
- `.codex/skills` 这一列不读。
- `.codebuddy/skills` 这一列不读。那是 CodeBuddy Code 的路径，只有 codebuddy/cli 会发现。
- `CODEBUDDY.md` 这一列不读。`CODEBUDDY.md` 只有 codebuddy/cli 认。
- `.cursor/rules` 里的 `.mdc` 这一列不读。该目录里的 `.md` 会读。Cursor 自己的规则页把纯 `.md` 写成不读、把 `.mdc` 写成读。
- 自定义 `base_url` 和三种 `api_backend` 写在用户配置。项目 `.grok/config.toml` 写不了这些键。
- 沙箱默认关。权限默认会问。

来源：本档案第 2 到第 10 节。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Overview | https://docs.x.ai/build/overview | 2026-10-05 |
| Changelog，1.0.46 | https://x.ai/build/changelog | 2026-10-05 |
| npm `@xai-official/grok` | https://www.npmjs.com/package/@xai-official/grok | 2026-10-05 |
| CLI reference | https://docs.x.ai/build/cli/reference | 2026-10-05 |
| AGENTS.md | https://docs.x.ai/build/features/project-rules | 2026-10-05 |
| Skills | https://docs.x.ai/build/features/skills-plugins-marketplaces | 2026-10-05 |
| Modes and commands | https://docs.x.ai/build/modes-and-commands | 2026-10-05 |
| Plan mode | https://docs.x.ai/build/features/plan-mode | 2026-10-05 |
| Permissions | https://docs.x.ai/build/features/permissions | 2026-10-05 |
| Sandbox | https://docs.x.ai/build/features/sandbox | 2026-10-05 |
| Subagents | https://docs.x.ai/build/features/subagents | 2026-10-05 |
| Hooks | https://docs.x.ai/build/features/hooks | 2026-10-05 |
| MCP | https://docs.x.ai/build/features/mcp-servers | 2026-10-05 |
| Background tasks | https://docs.x.ai/build/features/background-tasks | 2026-10-05 |
| Settings | https://docs.x.ai/build/settings | 2026-10-05 |
| Settings reference | https://docs.x.ai/build/settings/reference | 2026-10-05 |
| Enterprise | https://docs.x.ai/build/enterprise | 2026-10-05 |
| User guide，project rules | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/12-project-rules.md | 2026-10-05 |
| User guide，skills | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/08-skills.md | 2026-10-05 |
| User guide，plugins | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/09-plugins.md | 2026-10-05 |
| User guide，memory | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/13-memory.md | 2026-10-05 |
| User guide，subagents | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/16-subagents.md | 2026-10-05 |
| User guide，keyboard shortcuts | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/03-keyboard-shortcuts.md | 2026-10-05 |
| User guide，plan mode | https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/19-plan-mode.md | 2026-10-05 |
