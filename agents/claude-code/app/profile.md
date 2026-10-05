# Claude Desktop / Code

```yaml
agent: claude-code
surface: app
product: Claude Desktop
version: 未查证
released_at: 未查证
researched_at: 2026-10-05
status: 已填写
docs:
  - https://code.claude.com/docs/en/desktop
  - https://code.claude.com/docs/en/changelog
```

桌面页没有给出当前构建号。它写过功能所需的最低版本，例如窗格布局要 Claude Desktop v1.2581.0 或更新，1.1.5368 之前没有本机定时任务。这些都不是「最新稳定版」。内嵌的 Claude Code 引擎版本也未查证。CLI changelog 的 2.1.289 不能当成这个构建的版本。

2026-10-05 回填：总表新增的 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills` 四行，发现表都没有这几条路径，全部记为不读取。详见第 3 节。

未查证项：当前桌面构建号；内嵌 CLI 版本；`AskUserQuestion` 在 Code 标签里的对话框是否和终端一样有 `Other` 行。

## 1. 身份与版本

- 官方名：Claude Desktop。Code 是三个标签之一，另外两个是 Chat 和 Cowork。
- 表面：macOS、Windows。Linux 安装页标成 beta。
- 本档案只写 Code 标签里的本地会话。Cloud、SSH、WSL 是同一标签里的其他运行环境，行为差写在对应小节，不另建表面。
- 下载入口：https://code.claude.com/docs/en/desktop 。更新检查在 macOS 的 Claude → Check for Updates，Windows 的 Help → Check for Updates。
- 查证日期：2026-10-05。

来源：https://code.claude.com/docs/en/desktop

## 2. 指令文件与优先级

桌面页写明，Desktop 和 CLI 通过 `CLAUDE.md` 共享配置和项目记忆，并且跑同一个底层引擎。记忆页没有为桌面另写发现算法。按那一页，`CLAUDE.md` 从宽到窄拼接。默认的 `claude-md-or-agents-md` 只在没有项目级 `CLAUDE.md`、`.claude/CLAUDE.md`、`CLAUDE.local.md` 时改读 `AGENTS.md`。`AGENTS.override.md` 不读取。

`AGENTS.md` 的版本门是 CLI 2.1.277。当前桌面构建里的引擎是否已经不低于这个版本，未查证。

`CODEBUDDY.md`：不读取。桌面页写 Desktop 和 CLI 通过 `CLAUDE.md` 共享项目记忆；记忆页枚举的会读文件里没有 `CODEBUDDY.md`，这个名字在两页都没有出现。

`/config` 是终端界面。桌面应用不打开它。要改项目指令的取值，编辑 `~/.claude/settings.json`、`--settings` 或托管设置。桌面页没有写自己的 Project instructions 控件。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/memory ，https://code.claude.com/docs/en/settings

## 3. Skill 目录

和 CLI 相同。本地 Code 会话从 `~/.claude/skills/` 加载个人 skill，从仓库的 `.claude/skills/` 加载项目 skill。提示框输入 `/`，或点 `+` 里的 Slash commands，可以调用内置命令、自定义 skill、项目 skill 和已安装插件的 skill。

发现表没有 `.agents/skills` 和 `~/.agents/skills`。记为不读取。2026-10-05 再打开同一页，正文也没有 `.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills`。记为不读取。同一次打开，正文也没有 `.codebuddy/skills`、`~/.codebuddy/skills`、`.grok/skills`、`~/.grok/skills`。这四条同样记为不读取。

同名规则也和 CLI 相同：企业覆盖个人，个人覆盖项目，不合并。插件 skill 带命名空间。

SSH 会话读的是远程主机上的 `~/.claude/skills/`。云端会话不读本机的 `~/.claude/skills/`，改读 claude.ai 账号上启用的 skill，以及仓库里提交的项目 skill。Cowork 的 skill 来自 Customize，经 claude.ai 同步，不来自 CLI 的 `~/.claude`。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/skills

## 4. 内置 skill 与内置功能

- 捆绑 skill：Code 标签调用的是 Claude Code 的 skill。名单见 [CLI 档案](../cli/profile.md) 第 4 节。桌面页没有另列一份捆绑名单。
- workflow：有。工具参考把 `Workflow` 列为 Claude Code 的内置工具。桌面页写 Code 标签使用同一个引擎，没有另写一套工具表。
- plan mode：有。权限模式里有 Plan。Claude 先读文件、跑命令，再给出计划，不改源代码。这个选择只对当前会话，不记成该文件夹的默认模式。
- 定时任务：有。Code 标签的 Routines 可以建本机定时任务。应用开着、电脑醒着才跑。1.1.5368 之前没有这项。云端 Routines 在电脑关掉时也能跑，那是另一条产品入口。会话内的 `/loop` 仍要求会话开着，比较表把它和桌面定时任务分开。
- 插件市场：有。在终端、桌面应用的本地会话或 VS Code 扩展里以 user 作用域装的插件，在同一台机器的另外两处也可用，因为三处读同一组设置文件。云端会话（含 claude.ai/code 的浏览器会话）不加载本地设置里的插件。市场是 `/plugin marketplace add` 添加后再 `/plugin install`，见 [CLI 档案](../cli/profile.md) 第 4 节。
- memory：有。本地会话的 auto memory 默认开，目录和加载上限见 CLI 档案。桌面页确认项目记忆经 `CLAUDE.md` 与 CLI 共享。auto memory 的文件不是 `CLAUDE.md`。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/desktop-scheduled-tasks ，https://code.claude.com/docs/en/tools-reference ，https://code.claude.com/docs/en/memory

## 5. Tools

桌面页写 Code 标签使用和 CLI 相同的底层引擎。下面按桌面页点名的能力和工具参考里的工具名一起写。

- 向用户提问：审批有。模式选择器决定编辑和命令要不要先问。自由问卷有工具 `AskUserQuestion`。终端文档写了选择题、`Other` 行和备注栏。Code 标签的对话框是否同一套控件，未查证。
- shell：有。会话里有集成终端。Computer use 在任务是 shell 命令时改走 Bash。
- 文件编辑：有。Manual 模式会先给出 diff。Accept edits 自动接受文件编辑。
- 子 agent：有。窗格名单里有 subagent。工具名仍是 `Agent`。
- MCP：有。Connectors 是带图形安装流程的 MCP server。名单外的服务器按设置文件手动添加。见第 8 节。
- 浏览器：有。Browser 窗格可以预览本机应用，也可以打开外部网站。Computer use 默认关。需要已登录的个人浏览器时，改用 Claude in Chrome，它和 Browser 窗格不是同一个配置文件。
- 网页搜索：有。工具参考列出 `WebSearch` 和 `WebFetch`。桌面页没有单独把这两个名字再写一遍。
- 外部消息渠道：未查证。channels 页通篇按终端、后台进程、持久终端描述，没有点名桌面 Code 标签。插件框架虽然覆盖桌面，这个能力没有在桌面表面写实。缺的是一份写明桌面会话能否接渠道的文档。
- 常驻守护进程：未查证。桌面应用本身是常驻 GUI，但「脱离终端的 supervisor 进程 + `claude daemon` 子命令」这一套只在 CLI 参考里出现，桌面页没有写。缺的是一份写桌面是否有对应机制的文档。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/tools-reference ，https://code.claude.com/docs/en/channels ，https://code.claude.com/docs/en/plugins ，https://code.claude.com/docs/en/cli-reference

## 6. 配置

桌面应用和 CLI 读同一组设置文件。优先级从高到低仍是托管设置、命令行、`.claude/settings.local.json`、`.claude/settings.json`、`~/.claude/settings.json`。

在选择器里为某个文件夹选定的权限模式会记住，并盖过该文件夹的 `permissions.defaultMode`。Plan 除外，它只对当前会话。

`/config` 不在桌面里打开。桌面改设置靠编辑设置文件，或用应用自己的 Settings。

网关地址不走 `settings.json` 里的 `ANTHROPIC_BASE_URL`。见第 7 节。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/settings ，https://code.claude.com/docs/en/llm-gateway-connect

## 7. BYOK 与 API 格式

- 自带 key：部分。桌面和云端会话不读 `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN`、`apiKeyHelper`。默认用 OAuth。例外是第三方推理配置，会话改用那份配置里的凭证。
- 计费：普通 Code 会话用 Claude 登录。第三方推理打开后，环境选择器不再提供 SSH 和 Anthropic 托管的云环境。
- base URL：有，入口不同。管理员下发的第三方推理配置优先。没有下发时，打开 Developer → Configure Third-Party Inference，填网关 base URL。不读 `ANTHROPIC_BASE_URL`。
- 任意模型：限制。提示框旁的模型下拉选择。组织的 `availableModels` 会收窄可选模型。不是任意模型名。
- 线上协议：Anthropic API。第三方推理配置提供的是网关 base URL 和该配置的凭证。文档没有把 Responses 或 Chat Completions 写成桌面可接受的协议。
- 版本分界：见 [Claude Code README](../README.md)。桌面构建号未查证。

来源：https://code.claude.com/docs/en/authentication ，https://code.claude.com/docs/en/llm-gateway-connect ，https://code.claude.com/docs/en/desktop

## 8. MCP

支持。Code 标签有两条路。Connectors 用图形流程添加 GitHub、Slack、Linear 这类服务。名单外的服务器写入和 CLI 相同的配置文件。

本地 Code 会话里，同名 stdio 服务器如果同时出现在 `~/.claude.json` 顶层和 `.mcp.json`，Code 标签采用 `~/.claude.json` 的定义。这和 CLI「local 高于 project 高于 user」的顺序不是同一句。local 范围是否仍高于这两处，该句没有写，未查证。

定时任务会话可以使用配置文件里的 MCP，也可以使用按任务配置的 connectors。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/mcp ，https://code.claude.com/docs/en/desktop-scheduled-tasks

## 9. 权限与沙箱

Code 标签的权限模式是 Manual（`default`）、Accept edits（`acceptEdits`）、Plan（`plan`）、Auto（`auto`）、Bypass permissions（`bypassPermissions`）。`dontAsk` 只在 CLI。

选择器里的模式按文件夹记住，并盖过 `permissions.defaultMode`。Plan 只对当前会话。

Bash 沙箱的开关、默认关闭、以及 macOS、Linux、WSL2、原生 Windows 的差别，见 [CLI 档案](../cli/profile.md) 第 9 节。桌面读同一组设置。Computer use 不在这层沙箱里。桌面页写它跑在真实桌面上，和 sandboxed Bash tool 不是同一边界。Computer use 默认关。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/sandboxing

## 10. 子 agent

有。窗格可以打开 subagent。定义文件、优先级和 `CLAUDE.md` 的加载规则与 CLI 相同，见 CLI 档案第 10 节。桌面页没有另写一套子 agent 目录。

`--agents` 是启动 CLI 的 flag。桌面的 CLI flag 对照表没有它的等价项。桌面一次会话能否传入这份 JSON，未查证。

来源：https://code.claude.com/docs/en/desktop ，https://code.claude.com/docs/en/sub-agents

## 11. Hooks

有。hooks 页写明，终端、IDE 扩展、桌面应用和云端会话触发同一组事件。事件名单和配置位置见 [CLI 档案](../cli/profile.md) 第 11 节。

插件可以在桌面里安装。插件带 hooks。`allowManagedHooksOnly` 写在托管设置里，桌面同样读这组设置。

来源：https://code.claude.com/docs/en/hooks ，https://code.claude.com/docs/en/desktop

## 12. 分叉

相对 [Claude Code CLI](../cli/profile.md)：

- 同一套 `CLAUDE.md`、`.claude/skills` 和设置文件会被两边看到。
- 桌面构建号和内嵌引擎版本都未查证。按 CLI 2.1.289 写下的行为，不能直接当成当前桌面构建的行为。
- 桌面不读 `ANTHROPIC_API_KEY` 和 `ANTHROPIC_BASE_URL`。网关走第三方推理配置。
- 桌面有本机持久定时任务和应用内 Browser。CLI 的定时任务停在当前会话，浏览器走 Claude in Chrome。
- 同名 stdio MCP 在 `~/.claude.json` 顶层和 `.mcp.json` 冲突时，Code 标签采用前者。

相对 [ChatGPT desktop app 里的 Codex](../../codex/app/profile.md)：

- Codex 默认读 `AGENTS.md`。Claude Desktop 默认读 `CLAUDE.md`，只在没有项目级 `CLAUDE.md` 时改读 `AGENTS.md`。`AGENTS.override.md` 只有 Codex 读。
- Skill 目录一边是 `.agents/skills`，一边是 `.claude/skills`。
- Codex 档案写没有同名 workflow 编排器。Claude Code 的工具表有 `Workflow`。
- 线上协议一边是 Responses，一边是 Anthropic API。

来源：本节汇总前面各节。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Desktop application | https://code.claude.com/docs/en/desktop | 2026-10-05 |
| Desktop scheduled tasks | https://code.claude.com/docs/en/desktop-scheduled-tasks | 2026-10-05 |
| Changelog | https://code.claude.com/docs/en/changelog | 2026-10-05 |
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
| Plugins | https://code.claude.com/docs/en/plugins | 2026-10-05 |
| Channels | https://code.claude.com/docs/en/channels | 2026-10-05 |
| CLI reference | https://code.claude.com/docs/en/cli-reference | 2026-10-05 |
