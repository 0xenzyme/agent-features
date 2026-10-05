# Cursor CLI

```yaml
agent: cursor
surface: cli
product: Cursor CLI
version: 未查证
released_at: 2026-08-26
researched_at: 2026-10-05
status: 已填写
docs:
  - https://cursor.com/docs/cli/overview
  - https://cursor.com/docs/cli/changelog
```

截至 2026-10-05 打开的 CLI changelog，最新标题是 August 26, 2026 release。该页没有 semver。没有把这个标题标成预发布。其后若有尚未上到该页的版本，未查证。本档案没有用本机 `agent --version` 补版本。`released_at` 是这个标题的日期，不是构建号。

2026-10-05 回填：总表新增的 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills` 四行，发现表都没有这几条路径，全部记为不读取。详见第 3 节。

## 1. 身份与版本

- 官方名：Cursor CLI。
- 表面：终端。命令是 `agent`。交互会话，以及 `agent -p` 非交互运行。
- 版本：未查证。changelog 最新日期标题是 2026-08-26。
- 安装：macOS、Linux、WSL 是 `curl https://cursor.com/install -fsS | bash`。Windows PowerShell 是 `irm 'https://cursor.com/install?win32=true' | iex`。安装页让用户自己把 `~/.local/bin` 加进 PATH。文档写默认自动更新，也可以 `agent update`。验证命令是 `agent --version`，本档案没有运行它。
- 查证日期：2026-10-05。

来源：https://cursor.com/docs/cli/overview ，https://cursor.com/docs/cli/installation ，https://cursor.com/docs/cli/changelog

## 2. 指令文件与优先级

CLI 使用页写，CLI 使用和编辑器相同的规则系统。

- 项目规则：`.cursor/rules` 里的 `.mdc`。这个目录里的纯 `.md` 被忽略。
- 用户规则：Customize → Rules。迁移页写明用户规则不在文件系统上。
- 团队规则：dashboard，Team 和 Enterprise。强制规则不能在 Customize 里关掉。
- 顺序：Team Rules、Project Rules、User Rules。适用的规则合并，靠前的来源优先。

`AGENTS.md` 放在项目根或子目录。子目录和父目录拼接，更近的优先。`AGENTS.override.md` 在 Rules 页和 CLI 使用页都没有出现。记为不读取。

CLI 使用页另外写：项目根如果有 `AGENTS.md` 和 `CLAUDE.md`，会跟 `.cursor/rules` 一起套用。子目录的 `CLAUDE.md` 这一句没有写。未查证。

`AGENTS.md`、`CLAUDE.md` 和 `.cursor/rules` 冲突时谁优先，页面只写 alongside。未查证。`AGENTS.md` 不在 Team、Project、User 那句顺序里。

`CODEBUDDY.md`：不读取。规则系统由 `.cursor/rules`、项目根的 `AGENTS.md` 与 `CLAUDE.md`、以及 dashboard 上的团队和用户规则组成，没有「任意 .md 都算指令」的兜底。`CODEBUDDY.md` 不在任何一种里，文档也没有提到这个名字。

来源：https://cursor.com/docs/cli/using ，https://cursor.com/docs/rules ，https://cursor.com/docs/skills

## 3. Skill 目录

Skill 是带 `SKILL.md` 的目录。Frontmatter 要有 `name` 和 `description`，`name` 必须和父目录名一致。

| 范围 | 路径 |
| --- | --- |
| 项目 | `.agents/skills/`、`.cursor/skills/` |
| 用户 | `~/.agents/skills/`、`~/.cursor/skills/` |
| 兼容 | `.claude/skills/`、`.codex/skills/`、`~/.claude/skills/`、`~/.codex/skills/` |

仓库里任意位置的 `.cursor/skills/` 或 `.agents/skills/` 也会被发现，并自动只对那个目录下的文件生效。分类子目录只是分组，身份是含 `SKILL.md` 的那一层目录。

加载位置这张表是穷举的，里面没有 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`。这四条记为不读取。

同名 skill 是否合并，skills 页没有写。未查证。

插件 skill 随插件出现在 Customize。固定磁盘路径这一页没有写。

2026-03 的 CLI changelog 写，CLI 还会发现 `.claude/skills`、`.agents/skills`、`.codex/skills`。2026-08-11 写，skill 和子 agent 扫描不再走进隐藏的点目录。现行 skills 页仍把 `.cursor`、`.claude`、`.agents`、`.codex` 列在加载位置。扫描跳过隐藏目录和这些显式目录如何同时成立，changelog 没有展开。未查证。

第三方导入开关在 Cursor Settings → Agents，默认开。hooks 页写它决定是否加载 Claude Code hooks。skills 页没有写兼容目录是否也看这个开关。未查证。

来源：https://cursor.com/docs/skills ，https://cursor.com/docs/cli/changelog ，https://cursor.com/docs/reference/third-party-hooks

## 4. 内置 skill 与内置功能

在 Agent 聊天里输入 `/` 可以调用。文档写 Agent 也可能自动选用其中一部分。

`/automate`、`/autopilot`、`/canvas`、`/create-hook`、`/create-rule`、`/create-skill`、`/create-subagent`、`/cursor-blame`、`/loop`、`/migrate-to-skills`、`/review`、`/review-bugbot`、`/review-security`、`/sdk`、`/shell`、`/split-to-prs`、`/statusline`、`/update-cli-config`、`/update-cursor-settings`。

- workflow：无。打开的页面没有同名编排器。`/automate` 创建的是 Cursor Automations，那是另一表面，未写。子 agent 文档里的 Orchestrator pattern 是示例，不是内置编排器。
- plan mode：有。Shift+Tab、`/plan`、`--plan`、`--mode=plan`。Agent 会先问澄清问题。2026-01 的 changelog 写计划会存到磁盘。具体目录未查证。编辑器页写的用户主目录不要抄到这里。
- 定时任务：部分。`/loop` 按间隔重复一条 prompt 或一个 skill。Agent 概览写，不给固定间隔时由 Agent 自己决定何时再跑，并可以停掉这个 loop。它有没有任务列表、退出进程后还在不在，skills 页没有写。Automations 的定时触发属于未写表面。
- memory：未查证。Rules 页用「模型不会在补全之间保留记忆」解释为什么要有规则，不是一项记忆功能。Automations 的 Memories 工具属于未写表面。
- 模式还有 Agent（默认）和 Ask（只读，`/ask`、`--mode=ask`）。`/goal` 在 changelog 里标成正在推出。
- 插件市场：有。插件把 rules、skills、agents、commands、MCP 服务器和 hooks 打成一个可分发的包，从 Customize 页安装和管理，官方插件在 Cursor Marketplace，社区插件和 MCP 在 cursor.directory。团队市场由管理员在 Default marketplace 里放 MCP 服务器，再给队友在 Agent Window、IDE 和 CLI 里安装和配置；加进市场不等于给每个人都装上。插件可以用 `${CURSOR_PLUGIN_ROOT}` 指插件根目录。文档说插件以 Git 仓库分发、经 Cursor 团队提交、每个都人工审核。

来源：https://cursor.com/docs/skills ，https://cursor.com/docs/cli/overview ，https://cursor.com/docs/cli/using ，https://cursor.com/docs/agent/overview ，https://cursor.com/docs/cli/changelog

## 5. Tools

- 向用户提问（审批）：有。跑终端命令前要 y 或 n。`approvalMode` 的取值是 `allowlist`、`auto-review`、`unrestricted`。默认是哪一个，配置页没有写。
- 向用户提问（自由问卷）：有。2026-04 的 changelog 写，澄清问题一次一道，并带自由文本 Other。工具的产品内名字这一条没有写。
- shell：有。权限记号是 `Shell(commandBase)`。另有 `/shell`。
- 文件编辑：有。权限记号是 `Write(pathOrGlob)` 和 `Read(pathOrGlob)`。使用页写非交互模式有完整写权限。权限页写 print mode 仍可用 allow、deny 和 `--force` 控制哪些操作不经提示就跑。两句如何同时生效，未查证。
- 子 agent：有。见第 10 节。
- MCP：有。见第 8 节。
- 浏览器操作：未查证。使用页写的是文件、搜索、shell 和 web access，没有编辑器那种应用内浏览器窗格。子 agent 页有一个通过 MCP 控制浏览器的内置 Browser，没有写它是否在 CLI 里启动。
- 网页搜索：有。2026-07-13 的 changelog 写 Auto-Accept Web Search，默认关，键是 `autoAcceptWebSearch`。拉页面是另一项权限 `WebFetch(domainOrPattern)`，没有 allow 时每次都要批准。
- 外部消息渠道：未查证。文档里有 Slack 集成和 Automations 的 Slack / GitHub 触发，但那是云端另起会话的自动化（未写表面），不是把 IM 或 webhook 事件推进正在运行的本地会话。后者文档没有写，也没有写「没有」。
- 常驻守护进程：未查证。CLI 概览和命令表没有 `--serve`，也没有 daemon 子命令。Cloud Agents 是把会话推到云端跑，不是本机常驻进程。缺的是一份写明有或没有的页面。

来源：https://cursor.com/docs/cli/using ，https://cursor.com/docs/cli/reference/permissions ，https://cursor.com/docs/cli/reference/configuration ，https://cursor.com/docs/cli/changelog ，https://cursor.com/docs/subagents ，https://cursor.com/docs/plugins

## 6. 配置

| 范围 | 路径 |
| --- | --- |
| 全局，macOS / Linux | `~/.cursor/cli-config.json` |
| 全局，Windows | `$env:USERPROFILE\.cursor\cli-config.json` |
| 项目 | `<project>/.cursor/cli.json` |

项目级只能写权限。其他 CLI 设置只能写全局。`CURSOR_CONFIG_DIR` 可以换目录。Linux 和 BSD 在设置了 `XDG_CONFIG_HOME` 时用 `$XDG_CONFIG_HOME/cursor/cli-config.json`。

Schema 的 `version` 当前是 `1`。模型用 `/model` 选，示例有 `auto`、`gpt-5`、`sonnet-4-thinking`。代理用 `HTTP_PROXY`、`HTTPS_PROXY`、`NODE_USE_ENV_PROXY=1`，以及可选的 `NODE_EXTRA_CA_CERTS`。

这份文件是 CLI 的。不要把它写成编辑器的设置文件。

来源：https://cursor.com/docs/cli/reference/configuration

## 7. BYOK 与 API 格式

- 自带 key：部分。`agent login` 是浏览器登录 Cursor 账号。`CURSOR_API_KEY` 或 `agent --api-key` 是 Dashboard → API Keys 的用户 key，用来在脚本里充当这次登录，不是 OpenAI 或 Anthropic 的提供商 key。`/bedrock` 在功能打开时配置 Bedrock。2026-02 的 changelog 写 `agent bedrock` 可以使用自己的 access key 或团队 IAM role。OpenAI、Anthropic、Google、Azure 的 key 能否用于这条 CLI，认证页没有写。未查证。编辑器 Settings 里保存的提供商 key 会不会被 `agent` 使用，未查证。
- 订阅和 API：浏览器登录走 Cursor 账号。Dashboard key 走同一账号的自动化入口。Bedrock 走自己的凭据。计费细节这一页没有按套餐展开。
- base URL：未查证。`agent status` 会显示 Current endpoint configuration。哪些端点可以改，认证页没有写。打开的页面没有 `base_url`。
- 任意模型：限制。`/model` 从目录里选，不是任意填一个模型 id。
- 线上协议：请求经过 Cursor 后端。下游格式未查证。
- 版本分界：未查证。

来源：https://cursor.com/docs/cli/reference/authentication ，https://cursor.com/docs/cli/reference/slash-commands ，https://cursor.com/docs/cli/changelog ，https://cursor.com/help/models-and-usage/api-keys

## 8. MCP

CLI 读取和编辑器相同的 `mcp.json`。项目是 `.cursor/mcp.json`，全局是 `~/.cursor/mcp.json`。配置示例是 stdio 的 `command`，以及远程 `url`。支持 OAuth。`command`、`args`、`env`、`url`、`headers` 里可以插值。

斜杠命令 `/mcp list` 和 `/mcp list-tools` 列出服务器和工具。2026-06-29 的 changelog 也写了 `agent mcp list`。

2026-07-13 的 changelog 写，同一个远程服务器出现在多层配置里时，CLI 只跑一个实例，在 `/mcp` 的 Configure Scope 里选哪一层生效，选择按项目保存。2026-01 的 changelog 写过项目配置覆盖用户配置。两句覆盖的范围是否相同，未查证。

来源：https://cursor.com/docs/cli/using ，https://cursor.com/docs/mcp ，https://cursor.com/docs/cli/changelog ，https://cursor.com/docs/cli/reference/slash-commands

## 9. 权限与沙箱

有。`/sandbox` 或 `--sandbox enabled|disabled` 开关沙箱和网络，设置会留到以后的会话。默认开还是关，概览页没有写。

`~/.cursor/sandbox.json` 和 `<project>/.cursor/sandbox.json` 会生效，包括 SSH。两份都在时，项目级优先。团队管理员策略和硬编码的安全规则盖在上面，本地文件不能放宽那些保护。

`permissions.json` 是 Auto-review 的说明，和 `sandbox.json` 不是同一份文件。CLI 从 2026-03 起读和 IDE 相同的终端与 MCP allowlist 文件。deny 压过 allow。

macOS 用 Seatbelt。Linux 用 Landlock 和 seccomp，内核不够时改为先问再跑。远程环境和 CLI 不自带 AppArmor profile，创建沙箱失败时要另装文档里的包。Run Modes 页没有写 Windows。Windows 上的沙箱未查证。

沙箱里的网络默认先挡住，再由网络模式和 `sandbox.json` 打开。

来源：https://cursor.com/docs/cli/overview ，https://cursor.com/docs/cli/reference/configuration ，https://cursor.com/docs/cli/reference/permissions ，https://cursor.com/docs/agent/security/run-modes ，https://cursor.com/docs/cli/changelog

## 10. 子 agent

有。可以自定义。

| 范围 | 路径 |
| --- | --- |
| 项目 | `.cursor/agents/`、`.claude/agents/`、`.codex/agents/` |
| 用户 | `~/.cursor/agents/`、`~/.claude/agents/`、`~/.codex/agents/` |

同名时项目压过用户。同一层里 `.cursor/` 压过 `.claude/` 或 `.codex/`。

文件是 markdown 加 YAML。`name` 可省略，默认用文件名。`readonly: true` 时不能改文件，也不能跑改变状态的 shell。内置三个：`explore`、`bash`、`browser`。Browser 通过 MCP 操作浏览器。

子 agent 可以再启动子 agent，但再下一层不能。文档把这个嵌套限制写成自 Cursor 2.5 起。2.5 不是本档案的当前版本号。

2026-03 的 CLI changelog 写，子 agent 继承凭据、规则和审批策略。是否继承沙箱，该条没有写。未查证。当前子 agent 页写它们继承父会话的工具，包括 MCP。

来源：https://cursor.com/docs/subagents ，https://cursor.com/docs/cli/changelog

## 11. Hooks

有。

2026-01 的 changelog 写，CLI 有会话开始和结束、带后续消息的 stop、压缩前、子 agent 生命周期这些 hook，并读取、合并 Claude Code 的 `settings.json` hooks。来源顺序是 enterprise、team、project、user。2026-04 写 `afterAgentThought` 和 `afterAgentResponse` 会在 CLI 触发，并接受 Claude Code 格式的响应。2026-08-11 写已安装插件里的 hooks 会跑。

hooks.md 的目录更长，其中包括 `preToolUse`、`postToolUse`、`beforeShellExecution`、`beforeReadFile`、`afterFileEdit`。该页把 `beforeTabFileRead`、`afterTabFileEdit` 写成 Tab 补全，把 `workspaceOpen` 写成打开工作区。这两类是编辑器生命周期。CLI 是否逐个触发目录里的每一项，changelog 没有逐项重印。未查证。

第三方 hooks 页写的开关在 Cursor Settings 里，默认开。那是编辑器设置页。这个开关是否同样控制 CLI，该页没有写。未查证。

来源：https://cursor.com/docs/cli/changelog ，https://cursor.com/docs/hooks ，https://cursor.com/docs/reference/third-party-hooks

## 12. 分叉

把一个已经按 Codex 或 Claude Code 配好的仓库交给这条 CLI：

- 只有 `AGENTS.md`：CLI 会读。Codex 也会读。Claude Code 在默认设置下，要工作目录及其上级都没有项目级 `CLAUDE.md` 才读。
- 项目根同时有 `CLAUDE.md` 和 `AGENTS.md`：CLI 两份都读，并和 `.cursor/rules` 一起用。冲突顺序未查证。Codex 默认读 `AGENTS.md`，不读 `CLAUDE.md`。Claude Code 默认只读 `CLAUDE.md` 文件。
- 只有 `.agents/skills`：CLI 的发现表会读。Claude Code 不读。Codex 会读。
- 只有 `.claude/skills`：CLI 的发现表会读。Claude Code 会读。Codex 不读。
- 只有 `.cursor/rules/*.mdc`：这是 Cursor 的项目规则。Codex 和 Claude Code 的档案没有把它当成指令文件。
- 只有 `.codebuddy/skills` 或 `CODEBUDDY.md`：这条 CLI 都不读。那是 CodeBuddy Code 的两条路径，本轮新增的两列才会发现。
- 没有同名的动态 workflow。Claude Code 的 `Workflow` 在这里没有对应工具。
- 线上协议不能和 Codex 的 Responses 或 Claude Code 的 Anthropic API 当成同一条线路。

来源：本档案第 2、3、4、7 节，以及已写的 Codex、Claude Code 档案。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| CLI 概览 | https://cursor.com/docs/cli/overview | 2026-10-05 |
| 安装 | https://cursor.com/docs/cli/installation | 2026-10-05 |
| CLI changelog | https://cursor.com/docs/cli/changelog | 2026-10-05 |
| 使用 | https://cursor.com/docs/cli/using | 2026-10-05 |
| 配置 | https://cursor.com/docs/cli/reference/configuration | 2026-10-05 |
| 权限 | https://cursor.com/docs/cli/reference/permissions | 2026-10-05 |
| 认证 | https://cursor.com/docs/cli/reference/authentication | 2026-10-05 |
| 斜杠命令 | https://cursor.com/docs/cli/reference/slash-commands | 2026-10-05 |
| Rules | https://cursor.com/docs/rules | 2026-10-05 |
| Skills | https://cursor.com/docs/skills | 2026-10-05 |
| Hooks | https://cursor.com/docs/hooks | 2026-10-05 |
| 第三方 hooks | https://cursor.com/docs/reference/third-party-hooks | 2026-10-05 |
| Plugins | https://cursor.com/docs/plugins | 2026-10-05 |
| 子 agent | https://cursor.com/docs/subagents | 2026-10-05 |
| MCP | https://cursor.com/docs/mcp | 2026-10-05 |
| Run Modes | https://cursor.com/docs/agent/security/run-modes | 2026-10-05 |
| Agent 概览 | https://cursor.com/docs/agent/overview | 2026-10-05 |
| BYOK 帮助 | https://cursor.com/help/models-and-usage/api-keys | 2026-10-05 |
