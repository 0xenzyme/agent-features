# CodeBuddy Code / cli

```yaml
agent: codebuddy
surface: cli
product: CodeBuddy Code
version: 2.161.2
released_at: 2026-10-04
researched_at: 2026-10-05
status: 已填写
docs:
  - https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/README.md
  - https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/README.md
```

截至 2026-10-05 打开的 release notes 索引，最新条目是 v2.161.2，日期 2026-10-04。npm 上 `@tencent-ai/codebuddy-code` 的 `latest` 同日也是 2.161.2。该页由仓库里的 `docs/release-notes/` 生成，其后若有尚未上到该页的版本，未查证。本档案没有用本机 `codebuddy --version` 补版本。

同一份 release note 还给出同版本的 Agent SDK JS v0.3.271 与 Agent SDK Python v0.3.270。这两个号不是 CLI 的版本。

## 1. 身份与版本

- 官方名：CodeBuddy Code。文档自述为「腾讯云智能编程助手」，厂商腾讯。npm 包名 `@tencent-ai/codebuddy-code`。
- 表面：终端。交互会话，以及 `-p/--print` 非交互运行。文档把管道输入写成这个表面的特性：`git log --oneline | codebuddy "分析这些提交"`。
- 版本：2.161.2。条目日期 2026-10-04。没有把它标成预发布。
- 发布日期：2026-10-04。
- 查证日期：2026-10-05。
- 安装或下载入口：包管理器 `npm install -g @tencent-ai/codebuddy-code`（也写 pnpm、yarn、bun）；macOS/Linux 有 Homebrew tap `Tencent-CodeBuddy/tap`；另有原生二进制安装（文档标 Beta）`curl -fsSL https://www.codebuddy.cn/cli/install.sh | bash`，Windows 是 `irm https://www.codebuddy.cn/cli/install.ps1 | iex`。产品页两处：海外 https://www.codebuddy.ai/cli ，国内 https://copilot.tencent.com/cli 。命令名 `codebuddy`，短名 `cbc`。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/README.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/installation.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/README.md

## 2. 指令文件与优先级

记忆文件是 `CODEBUDDY.md`。分层五类，启动时全部自动加载：

| 类型 | 位置 |
| --- | --- |
| 用户记忆 | `~/.codebuddy/CODEBUDDY.md` |
| 用户规则 | `~/.codebuddy/rules/*.md` |
| 项目记忆 | `./CODEBUDDY.md` 或 `./.codebuddy/CODEBUDDY.md` |
| 项目规则 | `./.codebuddy/rules/*.md` |
| 项目记忆（本地） | `./CODEBUDDY.local.md` |

加载顺序：

1. 用户级：`~/.codebuddy/CODEBUDDY.md` 与 `~/.codebuddy/rules/`。
2. 项目级主文件：从当前工作目录向上递归到根（不含根 `/`），加载沿途所有 `CODEBUDDY.md` 和 `CODEBUDDY.local.md`。
3. 项目级规则：只加载当前工作目录的 `.codebuddy/rules/`，**不加载父目录的规则目录**。
4. 子目录记忆：操作子目录里的文件时，才动态加载该子目录的 `CODEBUDDY.md`。
5. 本地记忆：`./CODEBUDDY.local.md`。该文件自动进 `.gitignore`。

规则目录递归发现所有 `.md`，支持符号链接，循环链接会被检测。规则文件可用 frontmatter 控制：`enabled`、`alwaysApply`、`paths`。`paths` 是 glob（启用 `matchBase`），只在操作匹配文件时触发。`paths` 也可以写在 `CODEBUDDY.md`、`CODEBUDDY.local.md` 里。`CODEBUDDY.md` 里的 `@path` 导入最深 5 层，代码块和代码跨度里的 `@` 不解析。

`AGENTS.md`：支持，但是回退项。文档写「如果项目中存在 `CODEBUDDY.md` 文件，项目级记忆将使用 CODEBUDDY.md，否则使用 AGENTS.md」，并说系统自动检测。v2.33.0 起另外支持 `.mdc` 扩展名（`CODEBUDDY.mdc`、`AGENTS.mdc`）。

`CLAUDE.md`：不在记忆文件清单里，也不在加载顺序里。troubleshooting 的「从 Claude Code 迁移」一节把 `CLAUDE.md → CODEBUDDY.md` 列为需要迁移的项，给的是软链或复制（`cp ~/.claude/CLAUDE.md ~/.codebuddy/CODEBUDDY.md`）。这一格记为不读取。

`AGENTS.override.md`：文档没有提到这个文件名。未查证。缺的是一份列出全部指令文件名的文档。

额外目录：`--add-dir` 或 `/add-dir` 加进来的目录会加载其中的 skills；是否加载它的 `CODEBUDDY.md` 和规则，要看环境变量 `CODEBUDDY_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD`。写在设置里的 `additionalDirectories` 只授权，既不加载指令文件也不加载 skills。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/memory.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/troubleshooting.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/large-codebases.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/v2.33.0.md

## 3. Skill 目录

Skill 是带 `SKILL.md` 的目录。

| 范围 | 路径 |
| --- | --- |
| 项目 | `.codebuddy/skills/<name>/SKILL.md` |
| 用户 | `~/.codebuddy/skills/<name>/SKILL.md` |
| 插件 | `<plugin>/skills/<name>/SKILL.md`，命令带命名空间 `/plugin-name:skill-name` |

格式是 Markdown 加 YAML frontmatter。skills.md 的字段表把 `name` 标成可选（未指定时用目录名），plugins.md 又要求插件 skill 的 `SKILL.md` 必须写 `name` 和 `description`。两处口径不一致，按各自的来源记。其余字段包括 `description`、`allowed-tools`、`disable-model-invocation`、`user-invocable`、`context`、`agent`、`model`、`hooks`。`context: fork` 让 skill 跑在独立子 agent 上下文里。

发现规则：skills.md 只写了项目级和用户级两个目录，没有写向上搜索。large-codebases.md 写了子目录 skill：任何子目录都可以有自己的 `.codebuddy/skills/`，CodeBuddy 处理该目录的文件时才加载，例如 `packages/api/.codebuddy/skills/api-testing/`。启动时是否全量扫描仓库根的 `.codebuddy/skills/`，未查证。

优先级：项目级 > 用户级 > 插件级。同名 skill 是否合并，文档只说优先级，没有写合并。未查证。

`settings.json` 的 `skillOverrides` 可以在不改 `SKILL.md` 的前提下改单个 skill 的可见性，四态 `on`、`name-only`、`user-invocable-only`、`off`；生效顺序 `PROJECT_LOCAL > PROJECT > USER`。插件 skill 不受它影响。

文档点名的其他路径：`.claude/skills` 和 `~/.claude/skills` 出现在 troubleshooting 的迁移方案里（软链或复制到 `~/.codebuddy/skills`），这两条记为不读取。`.agents/skills`、`~/.agents/skills`、`.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills` 在本次读到的文档里没有出现，记为未查证。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/skills.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/codebuddy-dir.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/large-codebases.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/troubleshooting.md

## 4. 内置 skill 与内置功能

- workflow：有，产品名 Dynamic Workflow，处于研究预览阶段，要求 v2.105.0 或更高，默认已启用。工具名 `Workflow`。它让 CodeBuddy 现场写一段 JavaScript 编排脚本，后台调度数十到数百个子 agent；脚本 API 有 `agent()`、`parallel()`、`pipeline()`、`phase()`、`log()`，单次 run 上限 1000 个 agent，并发上限 16。入口是 `Workflow` 工具、提示里的关键词 `ultracode`、`/effort ultracode`，以及内置 `/deep-research`。写好的脚本存到 `.codebuddy/workflows/` 或 `~/.codebuddy/workflows/`，项目级优先，以 `/<name>` 调用。
- plan mode：有。工具 `EnterPlanMode` / `ExitPlanMode`。权限模式里也有 `plan`。
- 定时任务：部分。`CronCreate` / `CronList` / `CronDelete`。默认会话级，退出即清除，不写盘。传 `durable: true` 会写盘持久化，需要一次授权，写入项目目录，重启后恢复，同项目有 daemon 时优先由 daemon 承载；持久化任务各有一条专属任务会话。循环任务创建 3 天后自动过期，过期前最后触发一次；每会话最多 50 个；最小间隔 1 分钟。文档写明在 Web UI 里创建或编辑任务时可以启用持久化。
- memory：有，产品名 Auto Memory。位置 `~/.codebuddy/memories/{project-id}/` 和 `~/.codebuddy/memories/global/`，每个项目一份 `MEMORY.md` 索引，前 200 行进上下文。Typed Memory 默认开，四类 `user`、`feedback`、`project`、`reference`。开关有 `/memory` 面板、`/config` 面板、`settings.json` 的 `memory.autoMemoryEnabled`、环境变量 `CODEBUDDY_DISABLE_AUTO_MEMORY`。它不是 `CODEBUDDY.md` 的替代品。
- 插件市场：有。`/plugin marketplace add <owner/repo | git URL | 本地路径 | HTTP URL>` 再加 `/plugin install <插件名>@<市场名>`。安装作用域 user（默认）、project（写进 `.codebuddy/settings.json`）、local、managed。官方市场 `codebuddy-plugins-official` 启动即内置。插件可以带 `commands/`、`agents/`、`skills/`、`hooks/hooks.json`、`.mcp.json`、`.lsp.json`。
- 文档点名的内置命令（slash-commands.md 的表）：`/init`、`/memory`、`/doctor`、`/compact`、`/config`、`/agents`、`/model`、`/permissions`、`/plan`、`/mcp`、`/plugin`、`/skills`、`/sandbox`、`/code-review`、`/security-review`、`/simplify`、`/verify`、`/debug`、`/insights`、`/goal`、`/loop` 等。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/workflows.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/scheduled-tasks.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/memory.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugin-marketplaces.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/slash-commands.md

## 5. Tools

- 向用户提问：有。`AskUserQuestion` 出多选题。另有 `AskUserForStructuredInput`，把受限 JSON Schema 渲染成表单收集结构化答复，需环境变量 `CODEBUDDY_ENABLE_ASK_USER_FOR_STRUCTURED_INPUT=1` 且客户端声明 `elicitation.form` capability，否则模型看不到该工具。
- shell：有。工具名 `Bash`。Windows 另有 `PowerShell`（别名 `pwsh`、`ps`），无 Git Bash 时 Bash 自动禁用、PowerShell 成为唯一 shell 工具；macOS/Linux 上没有 PowerShell。
- 文件编辑：有。工具名 `Edit`、`MultiEdit`、`Write`、`NotebookEdit`。
- 子 agent：有。工具名 `Agent`。见第 10 节。
- MCP：有。工具名 `ListMcpResources`、`ReadMcpResource`、`WaitForMcpServers`，延迟加载走 `ToolSearch` + `DeferExecuteTool`。见第 8 节。
- 浏览器：工具表没有点名任何内置浏览器工具。未查证。缺的是一份浏览器自动化文档。Web UI 是浏览器里的界面，不是这个表面的工具。
- 网页搜索：有。工具名 `WebSearch`，抓取页面用 `WebFetch`。

清单外的新工具，写在下面：

- `Workflow`：动态编排。总表的「动态 workflow」行已经覆盖。
- `TeamCreate` / `TeamDelete` / `SendMessage`：Agent 团队，多代理协作。
- `CronCreate` / `CronList` / `CronDelete`：定时任务。
- `EnterPlanMode` / `ExitPlanMode`、`EnterWorktree` / `LeaveWorktree`。
- `ImageGen`、`VideoGen`：生成图片和视频。
- `LSP`：代码智能，需要插件及其语言服务器二进制。
- `REPL`：隔离沙箱里执行 JavaScript 编排；PTC 和极简模式下是模型面唯一直连工具。
- `Artifact` / `ArtifactControl`：发布和取消发布分享链接。
- `PushNotification`、`ReportFindings`、`StructuredOutput`、`SlashCommand`、`Skill`。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/tools-reference.md

## 6. 配置

设置是 JSON。四层，优先级从高到低：

1. 命令行参数
2. 本地项目 `.codebuddy/settings.local.json`（CLI 自动配 git 忽略）
3. 共享项目 `.codebuddy/settings.json`
4. 用户 `~/.codebuddy/settings.json`

hooks.md 的作用域表另列一层「企业策略：集成发布的策略文件」，没有给路径。企业策略文件的路径未查证。

权限规则不走这四层的顺序。permissions.md 给的顺序是 `flagSettings/cliArg/session > userSettings > policySettings > projectSettings > localSettings > command/sandbox 来源`，其中 `policySettings` 标注为进程态、不落盘；`deny` 在所有作用域合并，任一命中即拒。

项目级不能覆盖的键，文档点名两处：

- `permissions.defaultMode: "auto"`：只有用户级 `~/.codebuddy/settings.json` 和 CLI 注入的 settings 能授予。`.codebuddy/settings.json` 和 `.codebuddy/settings.local.json` 写了都会被忽略并回退 `default`。
- 顶层 `autoMode`：只从用户级、项目本地和 CLI `--settings` 读取，**不读共享项目 `.codebuddy/settings.json`**。文档给的理由是本地安全边界不该由仓库提交的配置悄悄改变。

`disableBypassPermissionsMode` 和 `disableAutoMode` 在四层任一层写成 `"disable"` 即生效。`CODEBUDDY_CONFIG_DIR` 把配置和数据目录挪走。目录信任另有 `trustAll`：免除启动时的「是否信任此目录」提示，但不跳过工具审批。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/settings.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permissions.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/installation.md

## 7. BYOK 与 API 格式

- 自带 key：有。文档点名 `CODEBUDDY_API_KEY`、`apiKeyHelper`、`CODEBUDDY_AUTH_TOKEN`。同时配置时优先级 `CODEBUDDY_AUTH_TOKEN > apiKeyHelper > CODEBUDDY_API_KEY`。`-p` 非交互模式下始终使用 `CODEBUDDY_API_KEY`。
- 订阅和 API 如何切换：iam.md 的认证方式表按场景给的是个人用 API Key、企业/团队用 `apiKeyHelper`（OAuth 应用）、CI/CD 已有 token 用 `CODEBUDDY_AUTH_TOKEN`、第三方模型服务用 `CODEBUDDY_API_KEY` + base URL。有没有订阅登录或套餐额度这一层，本次读到的文档没有写。未查证。
- 版本选择：`CODEBUDDY_INTERNET_ENVIRONMENT` 区分海外版（默认，不设）、中国版 `internal`、iOA 版 `ioa`、专享版 `cloudhosted`。Key 的获取地址分别是 codebuddy.ai、copilot.tencent.com、tencent.sso.copilot.tencent.com。
- base URL：有。`CODEBUDDY_BASE_URL`，settings 里另有 `endpoint`。
- 任意模型：`有`。文档写明「对接任意兼容 Anthropic 协议的第三方模型服务（如 DeepSeek），只需配置 Base URL、API Key 和模型变量，无需额外修改 `models.json`」。`models.json` 可以自定义模型条目（用户级 `~/.codebuddy/models.json`，项目级优先），`--model` 直接接受模型 ID、名称或别名字符串。前提是端点兼容 Anthropic 协议。
- 线上协议：Anthropic 兼容协议。文档只在第三方模型那段用「兼容 Anthropic 协议」表述，没有给出像 Responses 那样的正式协议名，也没有写协议版本。协议名与版本未查证。
- 版本分界：跨表面的时间线写在 [CodeBuddy README](../README.md)。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/iam.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/env-vars.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/models.md

## 8. MCP

支持 STDIO、SSE、HTTP 三种传输。`codebuddy mcp add --transport sse|http`。

三个作用域，优先级 `local > project > user`：

1. USER：`~/.codebuddy/.mcp.json` > `~/.codebuddy/mcp.json`（已废弃）> `~/.codebuddy.json`。
2. PROJECT：`<项目根>/.mcp.json` > `mcp.json`（已废弃）。
3. LOCAL：`~/.codebuddy.json` 里 `#/projects/<workspace_path>` 下的条目。

系统不会合并同一作用域的多个配置文件，只使用第一个存在的文件。跨作用域同名服务器也不合并，按上面的顺序整份采用。

与内置工具并存：工具名 `mcp__<服务器名>__<工具名>`，走 `deny > ask > allow` 权限规则。支持 `defer_loading` 加 `ToolSearch` 延迟加载。输出超过 `MAX_MCP_OUTPUT_TOKENS`（默认 20000）时落盘到 `~/.codebuddy/projects/<hash>/<session>/tool-results/`。MCP Prompts 会注册成 `/服务器名:prompt名称`。插件可以自带根目录 `.mcp.json`，市场条目也可以带 `mcpServers`。

审批：项目作用域的服务器首次连接时需要用户批准。`-p` 非交互下无法弹审批，要预先用 `--settings` 配 `enableAllProjectMcpServers: true` 或 `enabledMcpjsonServers`。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md

## 9. 权限与沙箱

权限模式和操作系统沙箱是两层。

文档里的用户可切换模式：`default`、`acceptEdits`、`auto`、`dontAsk`、`plan`、`bypassPermissions`、`delegate`。程序化或集成模式另有 `fullAccess`、`work`、`ignore`（`ignore` 只在子代理场景）。`Shift+Tab` 的循环是 `default → bypassPermissions → acceptEdits → auto → plan → delegate`，`dontAsk` 不在循环里，只能从 CLI、settings 或 SDK/IDE 信号进入。

默认模式：permission-modes.md 把 `default` 标为「默认；适合敏感工作 / 上手期」。settings.md 的 `defaultMode` 一行示例值写的是 `"acceptEdits"`。两处不一致，本档案按 permission-modes.md 记 `default`，并把不一致记在这里。

放行规则写在四层 settings 的 `permissions.allow` / `ask` / `deny`，另有进程态 `--allowedTools` / `--disallowedTools`、`--settings`、`policySettings`。`/permissions` 面板可以写回任一作用域。Bash 规则是前缀匹配，另有 glob 形式；复合命令要求所有子命令都命中 allow 才会放行。未信任目录下项目级 `allow` 归入「不可信层」，不能越过命令安全检查。

沙箱：默认关。`/sandbox` 命令或 `sandbox.enabled: true` 打开。它包住沙箱化 Bash 及其全部子进程；写保护同时作用于 Write / Edit / MultiEdit。是否覆盖 Read / Grep 等内置工具，文档没写，未查证。默认写入范围是当前工作目录及子目录，默认读取范围是整台机器除被拒目录；网络经沙箱外的代理，只能访问已批准域，新域触发审批。Linux 用 bubblewrap，macOS 用 Seatbelt；Windows 不支持，文档写「目前支持 Linux 和 macOS；计划支持 Windows」。沙箱默认把 `settings.json`、`settings.local.json` 加进写保护，防止提示注入改写配置。

目录信任：`trustAll` 免除「是否信任此目录」的启动提示，但工具审批仍由 `permissions.defaultMode` 决定。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permission-modes.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permissions.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/bash-sandboxing.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/settings.md

## 10. 子 agent

有，可以自定义。文件是带 YAML frontmatter 的 Markdown，`name` 和 `description` 必填，另有 `tools`、`model`、`permissionMode`、`skills`、`mcpServers`、`disallowedTools`、`effort`、`maxTurns`、`background`、`initialPrompt`、`memory` 等字段。

| 位置 | 优先级 |
| --- | --- |
| 项目 `.codebuddy/agents/` | 最高 |
| 用户 `~/.codebuddy/agents/` | 较低 |
| 插件 `agents/` | 见 plugins-reference |
| 本次会话 `--agents` JSON | 见 sub-agents.md |

不写 `tools` 时继承主线程全部工具，含 MCP。内置三个：`general-purpose`、`Explore`（只读，内置声明 `lite`）、`Plan`（仅在计划模式下使用，用于先研究再出计划）。嵌套深度封顶 5 层，子代理默认不持有 `Agent` 工具，要继续嵌套必须在定义里显式写 `tools: Agent`。每会话 spawn 预算默认 200，可用 `CODEBUDDY_CODE_MAX_SUBAGENTS_PER_SESSION` 调整，`/clear` 后重置。

权限：settings 的 `subagentPermissionMode` 可以统一覆盖子代理的权限模式，子代理自己在 frontmatter 里写的 `permissionMode` 更高，但主会话处于 `auto` / `dontAsk` 时子代理仍受父会话上限约束。

是否共享主会话的指令文件：sub-agents.md 没有写。agent-teams.md 只写团队成员「加载与普通会话相同的项目上下文（CODEBUDDY.md、MCP 服务器、技能等）」，这是团队场景，不是 `Agent` 工具场景。未查证。是否共享主会话的沙箱：文档只写了权限模式与 Bash 沙箱本身，未查证。

`memory` 字段给子代理单独一份记忆目录：`user` 落到 `~/.codebuddy/agent-memory/<name>/`，`project` 落到 `.codebuddy/agent-memory/<name>/`，还有 `local`。spawn 时注入 `MEMORY.md`，截断规则 200 行 / 25KB。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/sub-agents.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/agent-teams.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/settings.md

## 11. Hooks

有，文档标 Beta。

事件：hooks.md 概览写「完整支持 Hook 事件家族（27+ 种）」，并列出工具生命周期（`PreToolUse` / `PostToolUse` / `PostToolUseFailure`）、会话与子代理（`SessionStart` / `SessionEnd` / `Stop` / `StopFailure` / `SubagentStart` / `SubagentStop`）、用户交互（`UserPromptSubmit` / `Notification` / `PermissionRequest` / `PermissionDenied` / `Elicitation` / `ElicitationResult`）、上下文（`PreCompact` / `PostCompact` / `InstructionsLoaded` / `ConfigChange`）、任务与团队（`TaskCreated` / `TaskCompleted` / `TeammateIdle`）、文件与环境（`FileChanged` / `CwdChanged` / `WorktreeCreate` / `WorktreeRemove`）、启动维护（`Setup`）。完整清单文档指向 plugins-reference.md 的事件表，本次没有逐条核对那一页。hooks.md 正文的事件表只列了 10 个：`PreToolUse`、`PostToolUse`、`Notification`、`UserPromptSubmit`、`Stop`、`SubagentStop`、`PreCompact`、`PostCompact`、`SessionStart`、`SessionEnd`。

位置：用户、项目、项目本地三层 settings，企业策略文件（路径未查证），插件 `hooks/hooks.json`，以及自定义 Agent `.md` 与 Skill `SKILL.md` 的 frontmatter。

处理器类型：settings 里写 `command` 和 `prompt`（`prompt` 只支持 `Stop`、`UserPromptSubmit`、`PreToolUse`）；frontmatter 里支持 `command`、`prompt`、`agent`、`http` 四种。

合并与闸门：不同来源的 hooks 合并而非覆盖，同一事件下并行执行。插件 `hooks/hooks.json` 不受 `allowUntrustedFrontmatterHooks` 闸门约束；Agent / Skill frontmatter 里的 hooks 受该闸门约束，默认 `false`，即 `.codebuddy/agents|skills/*.md` 与插件市场来源的 frontmatter hooks 默认不注册，只有 product 内置的放行。frontmatter hooks 随子 agent 生命周期注册与注销，写的 `Stop` 会被重写成 `SubagentStop`。执行细节：60 秒超时、并行、去重、stdin 传 JSON；启动时快照，外部改动要在 `/hooks` 面板确认才生效。

版本要求那一行原文写「本文档针对 CodeBuddy Code v1.16.0 及以上版本中提供的 Hooks 实现」，与 2.x 的版本体系对不上。照原文记，不推断。

来源：https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/skills.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md

## 12. 分叉

相对 [CodeBuddy Code Web UI](../webui/profile.md)：

- 两者是同一个 CLI 进程的两种界面，同一个版本号。Web UI 由 `--serve` 或 daemon 拉起。
- 定时任务：这个表面默认会话级；Web UI 里创建或编辑任务时可以启用持久化，写进项目目录、重启后恢复，有 daemon 时交给 daemon 承载。
- 这个表面读 `CODEBUDDY_API_KEY`、`CODEBUDDY_BASE_URL`、`.mcp.json` 与四层 settings；Web UI 另外有网关认证（`--auth password|none`、`gateway.auth`）和 `--agent` 四种模式。
- MCP Apps 的 widget 只在这个表面之外生效：mcp-apps.md 写它仅在 Web UI / IDE 生效，TUI 与 `-p` 会自动降级成文本。
- Web UI 有内嵌编辑器、内嵌终端、Workers、日志、频道、监控、插件市场管理等面板，这个表面没有。

相对 [Claude Code CLI](../../claude-code/cli/profile.md)：

- 指令文件名一边是 `CODEBUDDY.md`，一边是 `CLAUDE.md`。两边都支持 `AGENTS.md`，但优先级相反：CodeBuddy 有 `CODEBUDDY.md` 时就不读 `AGENTS.md`，Claude Code 在默认 `claude-md-or-agents-md` 下是有 `CLAUDE.md` 时就不读 `AGENTS.md`。`CLAUDE.md` 在 CodeBuddy 是不读取，需要迁移。
- Skill 目录一边是 `.codebuddy/skills` / `~/.codebuddy/skills`，一边是 `.claude/skills` / `~/.claude/skills`。`.claude/skills` 在 CodeBuddy 也是不读取。
- 两边都有动态 workflow：Claude Code 的 `Workflow` 跑动态 workflow，CodeBuddy 的 Dynamic Workflow 是现场写 JS 编排脚本、后台调度子 agent，并发上限 16、单次 run 上限 1000 个 agent。
- 权限模式：CodeBuddy 另有 `delegate`（主代理只做协调），Claude Code 的 `dontAsk` 是 CLI 独有；CodeBuddy 的 `dontAsk` 不在 `Shift+Tab` 循环里。
- 线上协议：Claude Code 是 Anthropic API，CodeBuddy 文档只写「兼容 Anthropic 协议」，没有给正式协议名。

相对 [Codex CLI](../../codex/cli/profile.md)：

- Skill 目录一边是 `.codebuddy/skills`，一边是 `.agents/skills`。`.agents/skills` 在 CodeBuddy 未查证。
- 指令文件：Codex 默认读 `AGENTS.md`；CodeBuddy 有 `CODEBUDDY.md` 时就不用 `AGENTS.md`。
- 线上协议一边是 Responses，一边是 Anthropic 兼容协议。

来源：本节汇总前面各节，以及 https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md ，https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp-apps.md

## 13. 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| 文档首页 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/README.md | 2026-10-05 |
| Release notes 索引 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/README.md | 2026-10-05 |
| v2.161.2 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/v2.161.2.md | 2026-10-05 |
| v2.33.0 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/v2.33.0.md | 2026-10-05 |
| 安装指南 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/installation.md | 2026-10-05 |
| 记忆 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/memory.md | 2026-10-05 |
| Skills | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/skills.md | 2026-10-05 |
| 斜杠命令 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/slash-commands.md | 2026-10-05 |
| CodeBuddy 目录 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/codebuddy-dir.md | 2026-10-05 |
| 插件 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md | 2026-10-05 |
| 插件市场 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugin-marketplaces.md | 2026-10-05 |
| MCP | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp.md | 2026-10-05 |
| MCP Apps | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/mcp-apps.md | 2026-10-05 |
| 设置 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/settings.md | 2026-10-05 |
| 权限 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permissions.md | 2026-10-05 |
| 权限模式 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/permission-modes.md | 2026-10-05 |
| Bash 沙箱 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/bash-sandboxing.md | 2026-10-05 |
| 身份和访问管理 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/iam.md | 2026-10-05 |
| 环境变量 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/env-vars.md | 2026-10-05 |
| 模型 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/models.md | 2026-10-05 |
| 工具参考 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/tools-reference.md | 2026-10-05 |
| 子代理 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/sub-agents.md | 2026-10-05 |
| Agent 团队 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/agent-teams.md | 2026-10-05 |
| 定时任务 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/scheduled-tasks.md | 2026-10-05 |
| Dynamic Workflows | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/workflows.md | 2026-10-05 |
| Hooks | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md | 2026-10-05 |
| 大型代码库 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/large-codebases.md | 2026-10-05 |
| 故障排除 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/troubleshooting.md | 2026-10-05 |
| Web UI | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md | 2026-10-05 |
| 远程控制 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/remote-control.md | 2026-10-05 |
| Channels | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/channels.md | 2026-10-05 |
| Daemon | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/daemon.md | 2026-10-05 |
| IDE 集成 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/ide-integrations.md | 2026-10-05 |
