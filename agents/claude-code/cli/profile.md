# Claude Code CLI

```yaml
agent: claude-code
surface: cli
product: Claude Code
version: 2.1.289
released_at: 2026-10-03
researched_at: 2026-10-05
status: 已填写
docs:
  - https://code.claude.com/docs/en/setup
  - https://code.claude.com/docs/en/changelog
```

截至 2026-10-05 打开的 changelog，最新条目是 2.1.289，日期 2026-10-03。该页由 GitHub 上的 CHANGELOG.md 生成。其后若有尚未上到该页的版本，未查证。本档案没有用本机 `claude --version` 补版本。

2026-10-05 回填：总表新增的 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills` 四行，本档案的发现表都没有这几条路径，全部记为不读取。详见第 3 节。

## 1. 身份与版本

- 官方名：Claude Code。
- 表面：终端。交互会话，以及 `claude -p` 非交互运行。
- 版本：2.1.289。changelog 条目日期是 2026-10-03。没有把它标成预发布。
- 安装：setup 页的推荐原生命令是 `curl -fsSL https://claude.ai/install.sh | bash`。Windows PowerShell 是 `irm https://claude.ai/install.ps1 | iex`。Homebrew 是 `brew install --cask claude-code`，文档写这个 cask 跟踪稳定版。
- 查证日期：2026-10-05。

来源：https://code.claude.com/docs/en/changelog ，https://code.claude.com/docs/en/setup

## 2. 指令文件与优先级

`CLAUDE.md` 是持久指令文件。加载时从宽到窄拼接，不互相覆盖。离启动目录更近的文件出现在后面。

1. 托管策略：macOS `/Library/Application Support/ClaudeCode/CLAUDE.md`，Linux 和 WSL `/etc/claude-code/CLAUDE.md`，Windows `C:\Program Files\ClaudeCode\CLAUDE.md`。托管文件不能被 `claudeMdExcludes` 排除。
2. 用户：`~/.claude/CLAUDE.md`。
3. 项目：`./CLAUDE.md` 或 `./.claude/CLAUDE.md`。从当前目录向上每一层都会在启动时加载。
4. 本地：`./CLAUDE.local.md`。同一目录里它接在 `CLAUDE.md` 后面。

子目录里的 `CLAUDE.md` 和 `CLAUDE.local.md` 不在启动时全量加载。Claude 读到该目录的文件时才纳入。块级 HTML 注释会剥掉。`@path` 导入最深 4 层，代码跨度和代码围栏里的路径不导入。项目规则放 `.claude/rules/`。没有 `paths` 的规则与 `.claude/CLAUDE.md` 同一优先级，启动时加载。

`AGENTS.md` 从 2.1.277 起可以当项目指令。默认值 `claude-md-or-agents-md`：

- 当前目录及其上级都没有 `CLAUDE.md`、`.claude/CLAUDE.md`、`CLAUDE.local.md` 时，读取沿途的 `AGENTS.md` 和 `.claude/AGENTS.md`。
- 这三份里有任何一份时，只读 `CLAUDE.md` 文件，不读 `AGENTS.md`。
- `~/.claude/CLAUDE.md`、托管 `CLAUDE.md`、`.claude/rules/` 不参与这次判断，仍会加载。

另外三个值写在 `~/.claude/settings.json`、`--settings` 或托管设置的 `pluginConfigs["agents-md@builtin"].options.instructionFiles`。项目级和本地设置里的这个键被忽略。`claude-md-and-agents-md` 每个目录先读 `CLAUDE.md` 再读 `AGENTS.md`。`claude-md` 只读 `CLAUDE.md`。`managed-only` 启动时只留托管 `CLAUDE.md` 和 auto memory。

`CODEBUDDY.md`：不读取。记忆页把会读的指令文件枚举完整（上面那四层 `CLAUDE.md`，加上条件读取的 `AGENTS.md`），也显式点了不读的几项。`CODEBUDDY.md` 不在会读的清单里，文档也没有提到这个名字。要和 CodeBuddy Code 共用一份指令，得两个文件名各放一份。

`AGENTS.override.md`、`AGENTS.local.md`、`.agents/` 目录下的内容不读取。经这项设置载入的 `AGENTS.md` 不触发 `InstructionsLoaded`。`CLAUDE.md` 导入或符号链接到它时，hook 照常触发。`--add-dir` 在 `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` 时只额外加载那些目录的 `CLAUDE.md` 一类文件，不加载它们的 `AGENTS.md`。

2.1.277 之前、关掉内置 `agents-md` 插件、以及从 2.1.276 或更早升级后的第一次会话，只读 `CLAUDE.md`。2.1.281 之前，部分会话（文档举例 Amazon Bedrock 或关闭遥测）也只读 `CLAUDE.md`。

来源：https://code.claude.com/docs/en/memory

## 3. Skill 目录

Skill 是带 `SKILL.md` 的目录。自定义命令已经并进 skill。`.claude/commands/deploy.md` 和 `.claude/skills/deploy/SKILL.md` 都变成 `/deploy`。

| 范围 | 路径 |
| --- | --- |
| 企业 | 托管设置目录里的 `.claude/skills/<name>/SKILL.md` |
| 用户 | `~/.claude/skills/<name>/SKILL.md` |
| 项目 | `.claude/skills/<name>/SKILL.md`，从启动目录向上到仓库根 |
| 嵌套 | `<subdir>/.claude/skills/<name>/SKILL.md`，读到该目录的文件后才加载 |
| 插件 | `<plugin>/skills/<name>/SKILL.md`，命令是 `/plugin-name:skill-name` |

发现表没有 `.agents/skills`，也没有 `~/.agents/skills`。这两条记为不读取。2026-10-05 再打开同一页，正文也没有 `.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills`。这四条记为不读取。同一次打开，正文也没有 `.codebuddy/skills`、`~/.codebuddy/skills`、`.grok/skills`、`~/.grok/skills`。这四条同样记为不读取。

同名不合并。企业覆盖个人，个人覆盖项目。用户、项目或企业 skill 替换同名捆绑命令，不替换它的别名。插件 skill 带命名空间，两边都加载。嵌套 skill 也和根 skill 同时加载：`/deploy` 是根上的，`/apps/web:deploy` 是嵌套的。

保留目录名 `synced` 和 `anthropic-skills`。worktree 的向上搜索停在 worktree 根。2.1.277 起，worktree 根没有 `.claude/skills` 时，改加载主检出里的项目 skill。

来源：https://code.claude.com/docs/en/skills

## 4. 内置 skill 与内置功能

- 捆绑 skill：文档点名 `/doctor`、`/code-review`、`/batch`、`/debug`、`/loop`、`/claude-api`，以及 `/run`、`/verify`、`/run-skill-generator`。`/simplify` 出现在提交前检查的条件里。`/workflow-authoring` 只在动态 workflow 打开时可用。`disableBundledSkills` 关掉捆绑 skill。2.1.205 起 `/doctor` 仍可输入，除非另设 `DISABLE_DOCTOR_COMMAND` 或 `skillOverrides`。
- workflow：有。工具名 `Workflow`。它跑一段动态 workflow，在后台编排多个子 agent，再交回一份汇总结果。
- plan mode：有。工具 `EnterPlanMode` 进入，`ExitPlanMode` 把计划交出来批准。权限模式里也有 `plan`。
- 定时任务：部分。`CronCreate`、`CronDelete`、`CronList` 和 `/loop` 管当前会话里的提示。会话还开着才跑。未过期的任务在 `--resume` 或 `--continue` 时恢复。持久的桌面定时任务和云端 Routines 不是这个表面的管理界面。`RemoteTrigger` 背后是 claude.ai 的 Routines。
- 插件市场：有。会话内 `/plugin marketplace add <source>`，shell 里 `claude plugin marketplace add <source>`；source 可以是 GitHub owner/repo、git URL、本地目录或 marketplace.json。装插件是 `/plugin install` 或 `claude plugin install`。插件根带 `.claude-plugin/plugin.json`，可以打包 `skills/`、`agents/`、`hooks/hooks.json`、`.mcp.json`、`.lsp.json`、`commands/` 等。第一次交互终端会话会自动添加 Anthropic 官方市场，除非托管策略挡住。注意 claude.com/marketplace 那个网站不是用 `/plugin marketplace add` 添加的市场。
- memory：有，产品名是 auto memory。本地会话默认开。Claude Tag 之外的自托管环境默认关。`/memory` 把 `autoMemoryEnabled` 写进 `~/.claude/settings.json`。每个 Git 仓库一份目录，`~/.claude/projects/<project>/memory/`。启动时加载前 200 行或 25KB。它不代替 `CLAUDE.md`。

来源：https://code.claude.com/docs/en/skills ，https://code.claude.com/docs/en/tools-reference ，https://code.claude.com/docs/en/memory ，https://code.claude.com/docs/en/desktop-scheduled-tasks

## 5. Tools

- 向用户提问：审批有。权限规则、权限模式和 `PermissionRequest` hook 决定何时停下来。自由问卷有。`AskUserQuestion` 出选择题。用户可以选一项，也可以在 `Other` 行或备注栏输入自己的文字。默认一直开着，直到回答。
- shell：有。工具名 `Bash`。原生 Windows 没有 Git for Windows 时改用 `PowerShell`。
- 文件编辑：有。工具名 `Edit` 和 `Write`。笔记本用 `NotebookEdit`。
- 子 agent：有。工具名 `Agent`。见第 10 节。
- MCP：有。见第 8 节。
- 浏览器：部分。没有应用内浏览器。CLI 通过 Claude in Chrome 扩展操作 Chrome 或 Edge，动作发生在可见的浏览器窗口里。
- 网页搜索：有。工具名 `WebSearch`。抓取页面用 `WebFetch`。
- 外部消息渠道：有。channels 文档写，channel 是「an MCP server that pushes events into your running Claude Code session」，可以双向，Claude 从同一个 channel 回消息。研究预览里含 Telegram、Discord、iMessage，以插件形式安装、配自己的凭证，每插件维护发送者白名单，Telegram 和 Discord 走配对流程。文档写明事件只在会话开着时到达，要常开就得把 Claude 跑在后台进程或持久终端里。渠道需要 Bun，需要 claude.ai 或 Console key 登录，Bedrock、Vertex Agent Platform、Foundry 上没有。
- 常驻守护进程：有。后台会话由一个独立的 supervisor 进程托管，关掉 agent view、关掉 shell 或另开一个交互会话，派发出去的工作仍在跑；会话状态落盘，机器睡眠也保住。子命令是 `claude daemon status`（打印 supervisor 状态、版本、socket 目录、worker 数）和 `claude daemon stop --any`（可带 `--keep-workers`）。按需启动的 supervisor 是默认。能不能注册成 launchd 或 systemd 系统服务，文档没写，未查证。`--serve` 这个 flag 在官方文档里没有出现。

来源：https://code.claude.com/docs/en/tools-reference ，https://code.claude.com/docs/en/chrome ，https://code.claude.com/docs/en/setup ，https://code.claude.com/docs/en/channels ，https://code.claude.com/docs/en/cli-reference ，https://code.claude.com/docs/en/agent-view

## 6. 配置

设置是 JSON。终端、VS Code、JetBrains 和桌面应用读同一组文件。

优先级从高到低：

1. 托管设置：`managed-settings.json`、MDM，或 claude.ai 的 server-managed settings。除页面列出的少数安全相关例外，下面的层不能覆盖它。
2. 命令行：`claude --settings` 的 JSON 或文件，以及对应的单个 flag。只对这一次会话。写了的键覆盖下面三层，没写的键留在下层。
3. 项目本地 `.claude/settings.local.json`
4. 共享项目 `.claude/settings.json`
5. 用户 `~/.claude/settings.json`

`permissions.allow` 这类列表会合并。环境变量不在这五层里。shell 里的 `ANTHROPIC_MODEL` 盖过任何文件里的 `model`。`ANTHROPIC_DEFAULT_MODEL` 只在没有任何文件设置 `model` 时生效。文件里的 `env` 块是普通键，按上面五层走。

`pluginConfigs["agents-md@builtin"]` 在项目级和本地文件里被忽略。`CLAUDE_CONFIG_DIR` 把配置和凭证挪到另一个目录。`~/.claude.json` 另存登录会话、MCP 和项目信任状态，不是这五层设置文件。

来源：https://code.claude.com/docs/en/settings ，https://code.claude.com/docs/en/memory

## 7. BYOK 与 API 格式

- 自带 key：有。设置 `ANTHROPIC_API_KEY` 后，首次启动跳过登录，改为确认这把 key。也可以用 Console 的 API key、`ANTHROPIC_AUTH_TOKEN` 或 `apiKeyHelper`。
- 计费：claude.ai 的 Pro、Max、Teams、Enterprise 用订阅登录。Claude Console 用 API 凭证。Amazon Bedrock、Google Cloud Agent Platform、Microsoft Foundry 走云厂商账单，不需要浏览器登录。组织网关用企业 SSO。
- base URL：有。`ANTHROPIC_BASE_URL` 把请求送到自定义端点。可以放在 shell 里，也可以放进设置文件的 `env`。同一变量两边都有时，设置文件的值生效。
- 任意模型：限制。模型来自 Anthropic 的模型目录，或当前云厂商和网关暴露的模型。不是任意填写一个模型名就能用。
- 线上协议：Anthropic API。key 走 `X-Api-Key`，bearer token 走 `Authorization: Bearer`。现行文档没有把 Responses 或 Chat Completions 写成可接受协议。时间线见 [Claude Code README](../README.md)。
- 凭证：macOS 优先 Keychain，写失败时落到 `~/.claude/.credentials.json`，模式 `0600`。Linux 用同一文件。Windows 用 `%USERPROFILE%\.claude\.credentials.json`。

托管设置里的 `forceLoginMethod` 或 `forceLoginOrgUUID` 生效时，启动会挡住 `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN` 和 `apiKeyHelper`。

来源：https://code.claude.com/docs/en/authentication ，https://code.claude.com/docs/en/llm-gateway-connect ，https://code.claude.com/docs/en/settings

## 8. MCP

支持远程 HTTP 和本地 stdio。`claude mcp add` 添加。

三个范围，同名服务器整份采用高优先级的那一份，字段不合并：

1. local，默认。只在当前项目、只对你。写在 `~/.claude.json` 里该项目的条目。
2. project。`.mcp.json` 放在项目根，可提交。交互会话第一次使用前要批准。
3. user。`~/.claude.json` 顶层，所有项目，只对你。

再往后是插件提供的服务器，然后是 claude.ai connectors。托管的 `managedMcpServers` 高于以上全部，2.1.259 起。

来源：https://code.claude.com/docs/en/mcp

## 9. 权限与沙箱

权限模式和操作系统沙箱是两层。

文档中的权限模式包括 `default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`。`dontAsk` 只在 CLI。`permissions.defaultMode` 给出新会话的默认模式。

Bash 沙箱默认关。`/sandbox` 或 `sandbox.enabled: true` 打开。它包住 Bash、PowerShell、Monitor 以及它们拉起的进程。文件工具、MCP 和 hooks 在沙箱外。

打开之后，写入限于工作目录、每用户临时目录和额外目录。读取默认覆盖机器上的大部分路径，包括凭证文件，除非另加拒绝。网络走本机代理，允许域名从空名单开始。`autoAllowBashIfSandboxed` 默认 true。

macOS 用 Seatbelt。Linux 和 WSL2 用 bubblewrap 和 socat。原生 Windows 不上操作系统沙箱，要沙箱就在 WSL2 里跑。

来源：https://code.claude.com/docs/en/sandboxing ，https://code.claude.com/docs/en/desktop

## 10. 子 agent

有，可以自定义。文件是带 YAML frontmatter 的 Markdown。`name` 和 `description` 必填。身份看 `name`，不看路径。

同名时采用更高优先级的位置：

1. 托管设置目录里的 `.claude/agents/`
2. 本次会话的 `--agents` JSON
3. 项目 `.claude/agents/`，从当前目录向上到仓库根，更近的同名定义获胜
4. 用户 `~/.claude/agents/`
5. 插件 `agents/`，最低

同一棵目录里两个文件同名时，按文件系统读取顺序只留一个，没有另写的优先级。`/doctor` 会报告。`/agents` 在当前版本只提示去编辑这些目录。交互向导在 2.1.197 及更早版本里。

内置 Explore 和 Plan 跳过 `CLAUDE.md` 和 git status 快照。其他内置和自定义子 agent 默认都加载。`omitClaudeMd` 跳过用户、项目和本地 `CLAUDE.md`。插件子 agent 忽略 `hooks`、`mcpServers`、`permissionMode`。

父会话只收到子 agent 的最终结果。设置、托管策略和插件里的 hook 也会在子 agent 里跑。工具事件带 `agent_id` 和 `agent_type`。

来源：https://code.claude.com/docs/en/sub-agents ，https://code.claude.com/docs/en/tools-reference ，https://code.claude.com/docs/en/hooks

## 11. Hooks

有。终端、IDE 扩展、桌面应用和云端会话触发同一组事件。

位置包括用户、项目、本地设置，托管策略，插件 `hooks/hooks.json`，skill frontmatter，子 agent frontmatter。处理器可以是 command、HTTP、MCP tool、prompt、agent。插件还可以另注册进程内的 JS hook，那部分在 mods 文档。

`allowManagedHooksOnly` 可以挡住用户、项目、本地和插件 hook。托管设置强制启用的插件除外。

现行事件表包括 `SessionStart`、`Setup`、`UserPromptSubmit`、`UserPromptExpansion`、`PreToolUse`、`PermissionRequest`、`PermissionDenied`、`PostToolUse`、`PostToolUseFailure`、`PostToolBatch`、`Notification`、`MessageDisplay`、`SubagentStart`、`SubagentStop`、`TaskCreated`、`TaskCompleted`、`Stop`、`StopFailure`、`TeammateIdle`、`InstructionsLoaded`、`ConfigChange`、`CwdChanged`、`DirectoryAdded`、`FileChanged`、`WorktreeCreate`、`WorktreeRemove`、`PreCompact`、`PostCompact`、`PreModelSwitch`、`PostModelSwitch`、`Elicitation`、`ElicitationResult`、`SessionEnd`。

来源：https://code.claude.com/docs/en/hooks ，https://code.claude.com/docs/en/memory

## 12. 分叉

相对 [Claude Desktop 的 Code 标签](../app/profile.md)：

- 指令文件、skill 目录和设置文件是同一套。桌面内嵌的 CLI 版本未查证，不能把 2.1.289 的行为直接写成当前桌面构建的行为。
- CLI 读 `ANTHROPIC_API_KEY` 和 `ANTHROPIC_BASE_URL`。桌面 Code 标签不读这组环境变量，网关走第三方推理配置。
- `dontAsk` 只在 CLI。桌面有应用内 Browser，还有本机持久定时任务。
- 同名 stdio MCP 同时写在 `~/.claude.json` 顶层和 `.mcp.json` 时，Code 标签采用 `~/.claude.json` 的定义。CLI 的顺序仍是 local、project、user。

相对 [Codex CLI](../../codex/cli/profile.md)：

- 只有 `AGENTS.md`、没有项目级 `CLAUDE.md` 时，两边默认都读 `AGENTS.md`。两份都在时，Claude Code 默认只读 `CLAUDE.md`，Codex 默认读 `AGENTS.md`。
- Skill 目录分别是 `.claude/skills` 和 `.agents/skills`。
- Claude Code 有 `Workflow` 工具。Codex CLI 档案写的是没有同名编排器。
- 线上协议一边是 Anthropic API，一边是 Responses。

来源：本节汇总前面各节。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Changelog | https://code.claude.com/docs/en/changelog | 2026-10-05 |
| Setup | https://code.claude.com/docs/en/setup | 2026-10-05 |
| Memory | https://code.claude.com/docs/en/memory | 2026-10-05 |
| Skills | https://code.claude.com/docs/en/skills | 2026-10-05 |
| Tools reference | https://code.claude.com/docs/en/tools-reference | 2026-10-05 |
| Settings | https://code.claude.com/docs/en/settings | 2026-10-05 |
| Authentication | https://code.claude.com/docs/en/authentication | 2026-10-05 |
| LLM gateway | https://code.claude.com/docs/en/llm-gateway-connect | 2026-10-05 |
| MCP | https://code.claude.com/docs/en/mcp | 2026-10-05 |
| Sandboxing | https://code.claude.com/docs/en/sandboxing | 2026-10-05 |
| Subagents | https://code.claude.com/docs/en/sub-agents | 2026-10-05 |
| Hooks | https://code.claude.com/docs/en/hooks | 2026-10-05 |
| Chrome | https://code.claude.com/docs/en/chrome | 2026-10-05 |
| Desktop scheduled tasks | https://code.claude.com/docs/en/desktop-scheduled-tasks | 2026-10-05 |
| Desktop | https://code.claude.com/docs/en/desktop | 2026-10-05 |
| Plugins | https://code.claude.com/docs/en/plugins | 2026-10-05 |
| Channels | https://code.claude.com/docs/en/channels | 2026-10-05 |
| CLI reference | https://code.claude.com/docs/en/cli-reference | 2026-10-05 |
| Agent view | https://code.claude.com/docs/en/agent-view | 2026-10-05 |
