# CodeBuddy Code / webui

```yaml
agent: codebuddy
surface: webui
product: CodeBuddy Code Web UI
version: 2.161.2
released_at: 2026-10-04
researched_at: 2026-10-05
status: 已填写
docs:
  - https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md
  - https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/daemon.md
```

没有独立版本号。Web UI 由同一个 CLI 进程以 `codebuddy --serve` 启动，或由 daemon 常驻提供，文档写「不依赖终端窗口」。因此版本号跟随 CLI：截至 2026-10-05 的 release notes 索引，最新条目 v2.161.2，日期 2026-10-04。本档案没有用本机 `codebuddy --version` 补版本。

v2.161.2 这一条的改动几乎全部落在 Web UI 上（Agent View 列表与侧栏加载、`/btw` 侧问面板、Agent View 工作区恢复、后台 Worker 回收）。这可以佐证两者同版发布，但不构成 Web UI 有独立版本号的证据。

## 1. 身份与版本

- 官方名：Web UI。文档把它写成 CodeBuddy Code 内置的浏览器界面。
- 表面：浏览器。两种拉起方式：`codebuddy --serve`（跟随终端，终端退出即结束）和 daemon 常驻服务（`daemon start`，可注册成 launchd/systemd/计划任务，登录自启、崩溃恢复）。
- 版本：2.161.2（跟随 CLI，无独立版本号）。
- 发布日期：2026-10-04。
- 查证日期：2026-10-05。
- 入口：`codebuddy --serve --port 7890` 后打开 `http://127.0.0.1:7890`；或在会话里输入 `/gateway` 由远程控制拉起，终端给出二维码、Local 地址和 Tunnel 地址。另有 `--base-path` 把界面挂到固定 path 下。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/daemon.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/remote-control.md

## 2. 指令文件与优先级

Web UI 与 cli 是同一个进程。文档概述写「Web UI 提供与终端界面相同的核心能力，并针对浏览器进行了可视化布局优化」，但没有逐项复述记忆文件的发现规则、加载顺序和 skill 目录。

本档案不把 cli 档案的结论照抄过来。这一格记为未查证。缺的是一份专门写 Web UI 会话如何加载指令文件的文档。

可以确定的是：Web UI 的会话管理里写明「主 Web UI 使用持久化的 session ID 加载历史」，后台会话可以指定启动目录（`bgIsolation` 支持 `none` / `worktree`），工作目录可以在界面里添加或移除。这些只说明会话与工作目录的组织方式，不等于指令文件的加载规则。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md

## 3. Skill 目录

未查证。同上：文档没有为 Web UI 单独写 skill 发现规则。

可以确定的是界面一侧有插件管理面板（「插件：管理插件安装和插件市场」），以及设置面板可以配置主 Agent。这两项不等于 skill 目录规则。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md

## 4. 内置 skill 与内置功能

- workflow：有。`/workflows` 视图是 Web UI 的一个面板，按键 `p` 暂停、`x` 中止、`r` 重启、`s` 保存。Dynamic Workflow 的功能本体见 [cli 档案](../cli/profile.md) 第 4 节。
- plan mode：未查证。`--permission-mode plan` 在 `--serve` 命令行可用，但界面上有没有独立的 plan 交互，文档没有写。
- 定时任务：有。Web UI 的任务面板可以「浏览任务模版并创建定时任务」。scheduled-tasks.md 写明在 Web UI 创建或编辑任务时可以启用持久化，任务写入项目目录、重启后恢复，同项目有就绪 daemon 时由 daemon 承载。这是与 cli 相反的一点：cli 的 `CronCreate` 默认会话级、退出即清除。
- memory：未查证。文档没有为 Web UI 单独写记忆。
- 插件市场：有。界面有插件与插件市场管理面板。
- 界面自带的能力（文档概述列出的面板）：对话、编辑器、终端、Workers、日志、远程控制、监控、任务、插件、设置、文档、API 文档。编辑器最多 4 个分组、可跨组拖拽、终端可停靠；终端基于 xterm.js，最多 4 个面板，刷新后保持。移动端响应式，支持 PWA 加到主屏幕、扫码访问。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/scheduled-tasks.md

## 5. Tools

工具表是 CLI 的文档，不是按表面拆分的。所以这一节按「文档为 Web UI 单独写了什么」来记。

- 向用户提问：有。对话视图写明「问答面板：回答 Agent 的多选问题」，权限请求也可以在浏览器里直接批准或拒绝。`AskUserQuestion` 在 cli 档案第 5 节。
- shell：有。终端视图是基于 xterm.js 的内嵌终端，每个面板一个独立 PTY 会话，刷新后保持。这与工具层的 `Bash` 不是同一件事，两者都在这个表面可用。
- 文件编辑：有。右侧工作台有独立的编辑标签，支持跳转到指定行或行号范围，本地 HTML 可在源码与安全预览之间切换，编辑器、预览和文件树可以把当前文件下载到本地。
- 子 agent：有。Agent View 通过 `/api/v1/jobs` 管理后台 agent，可派发新会话并指定主 Agent、模型、思考强度、启动权限、启动目录、shell 模式，支持图片与文件附件；列表经 `/api/v1/jobs/events` 接收 `snapshot`、`added`、`changed`、`removed`、`keepalive`。
- MCP：有，界面能管插件与 MCP 相关的安装。MCP Apps 的 widget 只在这个表面和 IDE 生效，cli 的 TUI 与 `-p` 会降级成文本。
- 浏览器：未查证。文档没写这个表面有浏览器自动化工具。Web UI 本身运行在浏览器里，这不是同一件事。
- 网页搜索：未查证。工具表属于 CLI 文档，没有按表面拆分。

清单外的界面能力，写在下面：

- 远程控制与频道：Gateway 经 Cloudflare Tunnel 或局域网暴露界面；频道支持微信（内置，走微信 ClawBot）、Telegram、Discord、fakechat（后三者需装插件）。微信凭证存 `~/.codebuddy/channels/wechat/credentials.json`。频道是 Beta。
- 监控：系统资源指标和各 Worker 进程级内存/运行时间指标。
- 日志：独立日志查看器，支持多种日志类型和关键词搜索。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp-apps.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/channels.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/remote-control.md

## 6. 配置

四层 settings 是 CLI 的文档内容，未查证 Web UI 是否逐条读同一组文件。

Web UI 单独的配置写在两处：

- 命令行：`--serve`、`--open`、`--auth password|none`、`--base-path`、`--host`、`--port`、`--agent`、`--permission-mode`。
- 用户级 settings.json：`"gateway.auth": "none"` 对应 `--auth none`。

界面设置面板可以改：主题、语言、模型、权限模式、主 Agent。主 Agent 的总闸是 `codebuddy.mainAgent.enabled`（缺省开），「允许未声明宿主」是 `allowUnopted`（缺省关，WorkBuddy 保持原生 `cli`）。chip 或 TUI `/agent-mode` 的选择只写 `lastUsed`，不改 `default`；进程 `--agent` 压过 `lastUsed`。已有历史的会话锁定当前 Agent。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md

## 7. BYOK 与 API 格式

- 自带 key：未查证。文档没有为 Web UI 单独写凭证。CLI 侧是 `CODEBUDDY_API_KEY` / `apiKeyHelper` / `CODEBUDDY_AUTH_TOKEN`，见 [cli 档案](../cli/profile.md) 第 7 节。
- 订阅和 API 如何切换：未查证。
- base URL：未查证。
- 任意模型：未查证。界面设置面板有模型选项，「从可用选项中选择 AI 模型」。
- 线上协议：Anthropic 兼容协议，跟随 CLI。文档只在第三方模型那段用「兼容 Anthropic 协议」表述，没有给正式协议名和版本。
- 网关认证：这个是 Web UI 独有的。`--serve` 默认 `--auth password`，启动打印密码和带密码的链接；`--auth none` 显式关闭并打印警告，文档写明同机任意进程可经此服务执行命令、读写文件。认证方式四种：URL 参数 `?password=`（仅首页有效，对 `/api/v1/*` 无效）、登录页、Bearer Token、Cookie `gateway_session`（30 天）。远程控制侧 Gateway 启动时自动生成随机 token，另有登录限流：每分钟最多 2 次失败、每小时额外 12 次。
- 版本分界：跨表面的时间线写在 [CodeBuddy README](../README.md)。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/remote-control.md

## 8. MCP

未查证。文档没有为 Web UI 单独写 MCP 配置的路径与作用域。

可以确定的是：mcp-apps.md 写 MCP Apps 的 widget 仅在 Web UI / IDE 生效，TUI 与 `-p` 自动降级成文本；另外 Web UI 的对话视图可以在浏览器里批准或拒绝工具权限。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp-apps.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md

## 9. 权限与沙箱

权限模式：有，是这个表面文档写得最细的一节。

`--serve` 的 `--permission-mode` 取值：`default`、`acceptEdits`、`bypassPermissions`、`plan`、`dontAsk`、`auto`，运行时还认 `fullAccess`（help 里没列，语义接近 `bypassPermissions`）。文档写明 `minimal` + `plan` 会返回 400，不要这样配。会话内另有 `/permissions` 与浏览器内的批准/拒绝。`--subagent-permission-mode` 给子代理/队友单独设模式，不继承主会话。

四种主 Agent 模式是 `--serve` 独有的分叉：`cli`（标准，原生全工具）、`ptc`（Code Mode，只有 `REPL`，沙箱里全量）、`minimal`（极简，只有 `REPL` 加沙箱 Bash/Edit/Skill，无 MCP）、`create`（创造，与标准同一套工具）。也可以是自定义 id（`.codebuddy/agents/<name>.md` 的 `name`）。`multitask` 不是 `--serve` 的启动值，入口守卫会拒。

沙箱：未查证。文档没有为 Web UI 单独写沙箱。daemon 一侧的默认权限模式是 `delegate`：主 agent 只协调，实现交给子 agent。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/daemon.md

## 10. 子 agent

有。Agent View 用 `/api/v1/jobs` 管理后台 agent，派发时可指定主 Agent（内置四种加自定义）、模型、思考强度、启动权限、启动目录、名称和 shell 模式；`GET /api/v1/jobs/dispatch-context` 返回 `defaultAgentName`、`agents`、`permissionMode`。生命周期支持 reply、stop、respawn、delete；删除可能因前台持有或 worktree 守卫返回 `{ deleted: false, reason }`。

自定义子 agent 的文件位置（`.codebuddy/agents/` 与 `~/.codebuddy/agents/`）是 CLI 文档的内容，未查证 Web UI 是否读同一组路径。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/http-api.md

## 11. Hooks

未查证。hooks.md 按作用域写的是用户、项目、项目本地 settings 加企业策略、插件、Agent/Skill frontmatter，没有按表面区分，也没有写 Web UI 是否触发同一组事件。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md

## 12. 分叉

相对 [CodeBuddy Code CLI](../cli/profile.md)：

- 同一个进程、同一个版本号。指令文件、skill 目录、settings、MCP、hooks 这些只在 CLI 文档里写明，Web UI 侧文档只写「与终端界面相同的核心能力」，本档案没有照抄，记为未查证。
- 定时任务方向相反：cli 默认会话级、退出即清除；Web UI 创建或编辑任务时可启用持久化，写进项目目录、重启后恢复，有 daemon 时由 daemon 承载。
- Web UI 有网关认证（`--auth password|none`、Bearer、Cookie、URL 参数）和登录限流，cli 没有这一层。
- `--agent` 四种模式（`cli` / `ptc` / `minimal` / `create`）只在 `--serve` 这条上有；`minimal` 与 `plan` 同用会 400。
- MCP Apps 的 widget 只在 Web UI / IDE 生效，cli 的 TUI 与 `-p` 降级成文本。
- 频道（微信 / Telegram / Discord / fakechat）是 Web UI 与远程控制的入口，cli 侧没有对应的界面。
- daemon 常驻后，`--serve` 跟随终端退出这一点被改变：daemon 不依赖终端窗口，可注册为系统服务。

相对 [Claude Desktop 的 Code 标签](../../claude-code/app/profile.md)：

- 两边都是「非终端的一层界面」。Claude Code 那侧文档写桌面 Code 标签与 CLI 共用 `CLAUDE.md` 和同一个引擎；CodeBuddy 这侧文档只写「相同的核心能力」，没有逐项确认。
- CodeBuddy 的 Web UI 由本机 CLI 或 daemon 提供，地址在本机或 Tunnel 上；Claude Desktop 的 Code 标签是桌面应用里的一个标签。
- 定时任务的持久化方向相反：CodeBuddy 在 Web UI 侧可持久化，cli 侧默认不持久；Claude Code 是桌面有本机持久定时任务、CLI 只有会话内的 `CronCreate` 与 `/loop`。

来源：本节汇总前面各节。

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Web UI | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md | 2026-10-05 |
| Daemon | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/daemon.md | 2026-10-05 |
| 远程控制 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/remote-control.md | 2026-10-05 |
| Channels | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/channels.md | 2026-10-05 |
| HTTP API | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/http-api.md | 2026-10-05 |
| MCP Apps | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp-apps.md | 2026-10-05 |
| 定时任务 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/scheduled-tasks.md | 2026-10-05 |
| 权限模式 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permission-modes.md | 2026-10-05 |
| Hooks | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md | 2026-10-05 |
| Release notes 索引 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/README.md | 2026-10-05 |
| v2.161.2 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/v2.161.2.md | 2026-10-05 |
| ACP 协议 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/acp.md | 2026-10-05 |
