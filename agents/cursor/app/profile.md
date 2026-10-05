# Cursor 编辑器

```yaml
agent: cursor
surface: app
product: Cursor
version: 未查证
released_at: 未查证
researched_at: 2026-10-05
status: 已填写
docs:
  - https://cursor.com/docs/agent/overview
  - https://cursor.com/changelog
```

截至 2026-10-05 打开的产品 changelog，最新日期标题是 2026-09-23，条目是 Rollouts and Security Review。该页没有编辑器构建号。这个日期不是 `released_at` 能填的构建发布日，所以 YAML 里仍是未查证。不要把 CLI changelog 的 2026-08-26 当成编辑器版本。本档案没有读取本机安装的版本。

2026-10-05 回填：总表新增的 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills` 四行，发现表都没有这几条路径，全部记为不读取。详见第 3 节。

Rollouts、Security Review、Projects 是别的表面。下面不把它们的能力写成这个编辑器 Agent 的能力。

## 1. 身份与版本

- 官方名：Cursor。这个表面是桌面编辑器里的 Agent。
- 表面：编辑器侧栏，文档写 Cmd+I。
- 版本：未查证。产品 changelog 最新标题日期是 2026-09-23。
- 安装：changelog 页的站点导航有 `/download`。本轮没有打开下载页，没有钉安装包版本。
- 查证日期：2026-10-05。

来源：https://cursor.com/changelog ，https://cursor.com/docs/agent/overview

## 2. 指令文件与优先级

四种规则：

- 项目规则：`.cursor/rules` 里的 `.mdc`。纯 `.md` 被忽略。类型是 Always Apply、Apply Intelligently、Apply to Specific Files、Apply Manually。建议短于 500 行。
- 用户规则：Customize → Rules，所有项目通用，只给 Agent 聊天。不作用于 Inline Edit（Cmd/Ctrl+K）。迁移页写明它们不在文件系统上。
- 团队规则：dashboard 的自由文本，可带 glob。Team 和 Enterprise。没有 glob 时每段对话都套用。
- `AGENTS.md`：项目根和子目录的纯 markdown。子目录和父目录拼接，更近的优先。

顺序是 Team Rules、Project Rules、User Rules。适用的规则合并，靠前的来源优先。

`AGENTS.override.md` 没有出现。记为不读取。

Rules 页没有写 `CLAUDE.md`。编辑器是否默认发现 `CLAUDE.md`，未查证。不要把 CLI 使用页那句抄到这里。

`AGENTS.md` 和 `.cursor/rules` 冲突时谁优先，Rules 页没有写。未查证。

`CODEBUDDY.md`：未查证。Rules 页列的四种规则里没有它，也没有「任意 .md 都算指令」的兜底。但这页连 `CLAUDE.md` 都没有写，`CLAUDE.md` 这一格本档案也是未查证，所以不把它直接记成不读取。缺的是一份把编辑器会读的指令文件名列全的页面。

来源：https://cursor.com/docs/rules ，https://cursor.com/docs/skills

## 3. Skill 目录

和 CLI 使用同一张发现表。

| 范围 | 路径 |
| --- | --- |
| 项目 | `.agents/skills/`、`.cursor/skills/` |
| 用户 | `~/.agents/skills/`、`~/.cursor/skills/` |
| 兼容 | `.claude/skills/`、`.codex/skills/`、`~/.claude/skills/`、`~/.codex/skills/` |

加载位置这张表是穷举的，里面没有 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`。这四条记为不读取。

格式是目录加 `SKILL.md`。嵌套的项目 skill 目录只对那个目录下的文件生效。同名是否合并，该页没有写。未查证。

`~/.cursor/skills/` 留在本机，除非打开 Sync Skills for Cloud Agents，或发布到团队。`~/.agents/skills/` 不会被复制到 Cloud Agents。那是未写表面。

插件 skill 随插件出现在 Customize → Skills。固定路径这一页没有写。

2026-08-11「跳过隐藏目录」写在 CLI changelog，不写进这个表面。

来源：https://cursor.com/docs/skills

## 4. 内置 skill 与内置功能

捆绑名单和 CLI 相同，见 [CLI 档案](../cli/profile.md) 第 4 节。skills 页写在 Agent 聊天里输入 `/`。

- workflow：无。没有同名编排器。`/automate` 指向 Automations，未写。
- plan mode：有。Shift+Tab，或模式选择器。Agent 先问澄清问题，再给出可编辑的计划。计划默认存在用户主目录。Save to workspace 把它移进仓库。
- 定时任务：部分。`/loop` 按间隔重复一条 prompt 或一个 skill。概览页写可以停掉 loop，不给间隔时由 Agent 决定何时再跑。持久任务列表和退出应用后是否还在，该页没有写。Automations 的定时触发未写。
- memory：未查证。Rules 页那句「模型不会在补全之间保留记忆」是在解释规则，不是记忆功能。Automations 的 Memories 未写。
- 插件市场：有。插件把 rules、skills、agents、commands、MCP 服务器和 hooks 打成可分发的包。从 Customize 页安装和管理，官方插件在 Cursor Marketplace 浏览安装，社区插件和 MCP 在 cursor.directory。插件以 Git 仓库分发、经 Cursor 团队提交、每个都人工审核。编辑器侧还能用插件自带的 canvas。

来源：https://cursor.com/docs/skills ，https://cursor.com/docs/agent/plan-mode ，https://cursor.com/docs/agent/overview ，https://cursor.com/docs/rules

## 5. Tools

Agent 概览把工具写成这些能力，没有展开参数。

- 向用户提问（审批）：有。Run Modes 在 Settings > Agents > Approvals & Execution。模式是 Auto-review、Allowlist、Run Everything。分类器拦住之后仍可能弹出批准。默认选中哪一个，该页没有写。它把 Auto-review 写成多数人更合适的用法，不是默认值。
- 向用户提问（自由问卷）：部分。工具节有 Ask questions，任务中途提问，等待回答时 Agent 还可以继续读文件、改文件或跑命令。选择题、自由文本、Other 这三项，这一页没有写。Plan Mode 也会问澄清问题，控件是否同一套，未查证。
- shell：有。Run shell commands。默认用第一个可用的终端 profile。
- 文件编辑：有。Read files、Edit files。Checkpoints 在较大改动前给文件做本地快照，恢复时只回滚文件，不删对话。
- 子 agent：有。见第 10 节。
- MCP：有。见第 8 节。
- 浏览器操作：有。应用内浏览器窗格，可以导航、点击、输入、滚动、截图、看控制台和网络。工具默认要批准。
- 网页搜索：有。工具节的 Web 是生成查询并做网页搜索。另有代码库搜索，不是这一行。

图像生成写在同一工具节。图片默认存到项目的 `assets/`。

- 外部消息渠道：未查证。编辑器的集成里有 Slack，Automations 可以用 Slack 和 GitHub 事件触发，但那是云端另起会话的自动化（未写表面），不是把 IM 或 webhook 事件推进正在运行的本地会话。后者文档没有写，也没有写「没有」。
- 常驻守护进程：未查证。编辑器本身是常驻 GUI，但文档没有写脱离应用窗口的后台进程、可注册的系统服务或 `--serve` 这类本地常驻服务。缺的是一份写明有或没有的页面。

来源：https://cursor.com/docs/agent/overview ，https://cursor.com/docs/agent/security/run-modes ，https://cursor.com/docs/agent/tools/browser ，https://cursor.com/docs/agent/tools/search ，https://cursor.com/docs/plugins

## 6. 配置

编辑器不使用 `~/.cursor/cli-config.json`。那是 CLI 文件。

这一表面写下来的文件：

- 用户规则不在文件系统上。
- `~/.cursor/permissions.json` 和 `<project>/.cursor/permissions.json`。两份都在时合并，个人说明和项目说明都生效。团队在 dashboard 定义了 Auto-review 配置时，团队优先，本地两份被忽略。
- `~/.cursor/sandbox.json` 和 `<project>/.cursor/sandbox.json`。两份都在时项目级优先。团队策略和硬编码规则盖在上面。
- MCP、hooks 见第 8、11 节。
- 提供商 key 在 Cursor Settings > Models。
- Run Modes 在 Settings > Agents > Approvals & Execution。

项目级不能覆盖的键，编辑器没有一份和 CLI `cli.json` 对等的清单。未查证。

来源：https://cursor.com/docs/agent/security/run-modes ，https://cursor.com/docs/cli/reference/configuration ，https://cursor.com/help/models-and-usage/api-keys

## 7. BYOK 与 API 格式

- 自带 key：有。Cursor Settings > Models，提供商是 OpenAI、Anthropic、Google、Azure OpenAI、AWS Bedrock。OpenAI 只限标准的非推理聊天模型，选择器显示哪些可用。Anthropic 是 Anthropic API 上的 Claude 模型。Google 是 Google AI API 的 Gemini。Azure 是你实例里已部署的模型。Bedrock 用 IDE 里的 AWS access key，或 dashboard 上的 IAM role。自定义 key 只用于聊天模型。Tab 补全仍用 Cursor 自带模型。
- 订阅和 API：个人方案 Pro、Pro+、Ultra，提供商向你收费，这些请求不扣套餐内用量。Team 和 Enterprise，提供商收模型费，Cursor 仍收 Cursor Token Rate，每百万 token 0.25 美元，含输入、输出和缓存，记在 Other Models。Enterprise 可以在 Team Settings → Models 禁止个人 API key。禁止名单的原文是 OpenAI、Anthropic、Azure、AWS Bedrock。Google 是否被同一开关盖住，这一句没有写。
- 钥匙存储：不存在 Cursor 的服务器上。每次请求会送到 Cursor 后端，因为 prompt 在那里组装。传输加密，请求结束后不持久保存。Zero Data Retention 不覆盖 BYOK。
- base URL：未查证。帮助页没有自定义端点。
- 任意模型：限制。只能用上面列出的提供商，不能任意加一个模型 id。
- 线上协议：请求经过 Cursor 后端。下游格式未查证。OpenAI 那句「非推理聊天模型」没有写成 Chat Completions。
- 版本分界：未查证。

来源：https://cursor.com/help/models-and-usage/api-keys ，https://cursor.com/docs/enterprise/model-and-integration-management

## 8. MCP

有。项目 `.cursor/mcp.json`，全局 `~/.cursor/mcp.json`。示例是 stdio 的 `command` 和远程 `url`。支持 OAuth。插值字段是 `command`、`args`、`env`、`url`、`headers`。

同名服务器谁覆盖谁，mcp.md 没有写。未查证。不要把 CLI changelog 里的 Configure Scope 抄到编辑器。

团队可以在 Dashboard > Plugins & MCPs 分发服务器。原文写这些服务器先给 Cloud Agents。要给 Agent Window、IDE 和 CLI 用，需要 Add to Team Marketplace。Enterprise 有 MCP allowlist。

聊天里的 MCP 工具走 Run Mode 的批准。

来源：https://cursor.com/docs/mcp

## 9. 权限与沙箱

有。桌面应用在 Settings > Agents > Approvals & Execution 选 Run Mode。

Auto-review 不是安全边界。分类器跑在 Cursor 后端，文档写当前用 Gemini 3.5 Flash Lite，回退是 Claude 4.5 Haiku。它可以对本机做只读的 ReadFile、Grep、Glob、ListDir。

沙箱包住受支持的终端命令。工作区里可读写。`.git/config`、`.git/hooks`、`.vscode`、`.cursorignore` 和敏感的 Cursor 配置在保护路径里。网络默认挡住。`/tmp` 默认可写，除非 `sandbox.json` 关掉。需要完整系统权限的命令会绕过沙箱，并先请求批准。

macOS 用 Seatbelt，要求 Cursor v2.0 或更新，不用额外安装。Linux 用 Landlock 和 seccomp，内核 6.2 或更新，且要有非特权 user namespace。不够则改为先问再跑。桌面包装了 AppArmor profile。Run Modes 页没有写 Windows。Windows 沙箱未查证。

v2.0 是沙箱要求里的下限，不是这个表面的当前版本。

来源：https://cursor.com/docs/agent/security/run-modes

## 10. 子 agent

有。可以自定义。路径和优先级与 CLI 档案第 10 节相同，来源是同一页：项目 `.cursor/agents/`、`.claude/agents/`、`.codex/agents/`，用户目录是这三套的 `~/` 路径。同名时项目优先，`.cursor/` 优先于另两套。

内置 `explore`、`bash`、`browser`。可以再嵌套一层子 agent，再下一层不行。文档写自 Cursor 2.5 起。它们继承父会话的工具，包括 MCP。

当前这一页没有写自定义子 agent 是否加载 `AGENTS.md` 或 `.cursor/rules`，也没有写是否共享沙箱。未查证。CLI changelog 里「继承规则和审批策略」那句不要抄到这里。

来源：https://cursor.com/docs/subagents

## 11. Hooks

有。`hooks.json` 可以在项目 `<project>/.cursor/hooks.json` 和用户 `~/.cursor/hooks.json`。插件也可以从 Customize 安装。类型是 command 和 prompt。

来源顺序，从高到低：Enterprise、Team、Project、User。Enterprise 的路径是 macOS `/Library/Application Support/Cursor/hooks.json`，Linux 和 WSL `/etc/cursor/hooks.json`，Windows `C:\ProgramData\Cursor\hooks.json`。

匹配的 hook 都会跑。hooks.md 写：`deny` 压过 `ask`，`ask` 压过 `allow`，与来源无关。`user_message` 和 `agent_message` 拼接。其他字段按上述顺序合并，低优先级后写入，所以覆盖高优先级。第三方 hooks 页另写「冲突时高优先级优先」。两句如何同时成立，未查证。

Agent 事件：`sessionStart`、`sessionEnd`、`preToolUse`、`postToolUse`、`postToolUseFailure`、`subagentStart`、`subagentStop`、`beforeShellExecution`、`afterShellExecution`、`beforeMCPExecution`、`afterMCPExecution`、`beforeReadFile`、`afterFileEdit`、`beforeSubmitPrompt`、`preCompact`、`stop`、`afterAgentResponse`、`afterAgentThought`。

Tab 事件：`beforeTabFileRead`、`afterTabFileEdit`。工作区事件：`workspaceOpen`。

`preToolUse` 的输出可以是 allow 或 deny。schema 接受 `ask`，文档写今天的 `preToolUse` 并不执行 `ask`。

Claude Code 的 `.claude/settings.local.json`、`.claude/settings.json`、`~/.claude/settings.json` 可以加载。开关是 Include Third-Party Plugins, Skills, and Other Configs，在 Cursor Settings → Agents → Third-Party Imports，默认开。映射包括 `PreToolUse` 到 `preToolUse`、`PostToolUse` 到 `postToolUse`、`UserPromptSubmit` 到 `beforeSubmitPrompt`、`Stop`、`SubagentStop`、`SessionStart`、`SessionEnd`、`PreCompact`。`Notification` 和 `PermissionRequest` 不支持。Cursor 原生 `loop_limit` 默认 5。文档写 Claude Code hooks 的这项默认是不限制。

来源：https://cursor.com/docs/hooks ，https://cursor.com/docs/reference/third-party-hooks

## 12. 分叉

把一个已经按 Codex 或 Claude Code 配好的仓库交给这个编辑器：

- `AGENTS.md` 会读，含子目录。`AGENTS.override.md` 不读。
- `CLAUDE.md` 是否读取，未查证。不能假设它和 CLI 一样读项目根。
- `.agents/skills`、`.claude/skills`、`.codex/skills` 以及对应的用户目录都在发现表里。Claude Code 不读 `.agents`、`.cursor`、`.codex` 这几条。Codex 也不读 `.claude/skills` 和 `.cursor/skills`。
- `.codebuddy/skills` 和 `CODEBUDDY.md`：这一列都不读。那是 CodeBuddy Code 的两条路径。
- `.cursor/rules` 的 `.mdc` 是这个表面的项目规则。另外两家的档案没有读这个扩展名。
- 没有同名动态 workflow。
- 自带 key 经过 Cursor 后端，不能把 `ANTHROPIC_BASE_URL` 或 Codex 的 `openai_base_url` 写成这个表面的端点设置。
- Claude Code 的 `settings.json` hooks 在第三方导入开着时会跑，并且排在 Cursor 自己的 enterprise、team、project、user hooks 后面。

来源：本档案第 2、3、4、7、11 节，以及已写的 Codex、Claude Code 档案。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| 产品 changelog | https://cursor.com/changelog | 2026-10-05 |
| Agent 概览 | https://cursor.com/docs/agent/overview | 2026-10-05 |
| Rules | https://cursor.com/docs/rules | 2026-10-05 |
| Skills | https://cursor.com/docs/skills | 2026-10-05 |
| Plan Mode | https://cursor.com/docs/agent/plan-mode | 2026-10-05 |
| Browser | https://cursor.com/docs/agent/tools/browser | 2026-10-05 |
| 代码搜索 | https://cursor.com/docs/agent/tools/search | 2026-10-05 |
| Run Modes | https://cursor.com/docs/agent/security/run-modes | 2026-10-05 |
| 子 agent | https://cursor.com/docs/subagents | 2026-10-05 |
| MCP | https://cursor.com/docs/mcp | 2026-10-05 |
| Hooks | https://cursor.com/docs/hooks | 2026-10-05 |
| 第三方 hooks | https://cursor.com/docs/reference/third-party-hooks | 2026-10-05 |
| BYOK 帮助 | https://cursor.com/help/models-and-usage/api-keys | 2026-10-05 |
| 企业模型控制 | https://cursor.com/docs/enterprise/model-and-integration-management | 2026-10-05 |
| 文档索引 | https://cursor.com/docs/llms.txt | 2026-10-05 |
| Plugins | https://cursor.com/docs/plugins | 2026-10-05 |
