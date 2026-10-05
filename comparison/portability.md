# 可移植写法

已覆盖表面：

- [codex/cli](../agents/codex/cli/profile.md)，Codex CLI 0.160.0
- [codex/app](../agents/codex/app/profile.md)，ChatGPT desktop app 中的 Codex
- [claude-code/cli](../agents/claude-code/cli/profile.md)，Claude Code CLI 2.1.289
- [claude-code/app](../agents/claude-code/app/profile.md)，Claude Desktop 的 Code 标签，应用构建号未查证
- [cursor/cli](../agents/cursor/cli/profile.md)，Cursor CLI，semver 未查证
- [cursor/app](../agents/cursor/app/profile.md)，Cursor 编辑器，构建号未查证
- [grok/cli](../agents/grok/cli/profile.md)，Grok Build 1.0.46
- [codebuddy/cli](../agents/codebuddy/cli/profile.md)，CodeBuddy Code CLI 2.161.2
- [codebuddy/webui](../agents/codebuddy/webui/profile.md)，CodeBuddy Code Web UI，与 CLI 同版 2.161.2

查证日期：2026-10-05。

每条规则写明它依赖哪些表面。只覆盖两列 Codex 的规则，不能写成其他表面的规则。Grok 这一轮只有 CLI。CodeBuddy 这一轮写了 CLI 和 Web UI，两者是同一个进程、同一个版本号。

CodeBuddy 带来了总表里原来没有的行：`CODEBUDDY.md` 默认发现、`.codebuddy/skills`、`~/.codebuddy/skills`、插件市场、外部消息渠道、常驻守护进程。这六行当日已经回填，同时把此前挂着的 `.grok/skills`、`~/.grok/skills` 两行，以及 Codex 在 `.claude/skills`、`~/.claude/skills`、`.cursor/skills`、`~/.cursor/skills` 上的四格一起回填完。总表现在没有「未回填」。

## 指令文件

### 只覆盖 Codex

共享层用 `AGENTS.md`。全局偏好放 `~/.codex/AGENTS.md`。仓库规则放仓库根和更近的子目录。临时全局覆盖用 `~/.codex/AGENTS.override.md`，同一层有 override 时，普通 `AGENTS.md` 不读。

`CLAUDE.md` 不是 Codex 的默认指令文件。要给 Codex 用，把它导入成 `AGENTS.md`，或把文件名写进用户级 `project_doc_fallback_filenames`。

依赖表面：`codex/cli`、`codex/app`。

### 四列一起看

仓库里只有 `AGENTS.md`、工作目录及其上级都没有 `CLAUDE.md`、`.claude/CLAUDE.md`、`CLAUDE.local.md` 时：Codex 两列读取 `AGENTS.md`。Claude Code 在默认 `claude-md-or-agents-md` 下也读取 `AGENTS.md`。

仓库里同时有项目级 `CLAUDE.md` 和 `AGENTS.md` 时：Codex 两列读取 `AGENTS.md`。Claude Code 默认只读取 `CLAUDE.md` 文件。

`AGENTS.override.md`：Codex 两列读取。Claude Code 两列不读取。

Claude Code 的用户级 `~/.claude/CLAUDE.md` 和托管 `CLAUDE.md` 不阻止 `AGENTS.md`。它们仍会加载。

Claude Code 这条默认从 CLI 2.1.277 起写在记忆页上。桌面页写 Code 标签和 CLI 共用 `CLAUDE.md`，并使用同一个引擎。桌面内嵌引擎是否已经不低于 2.1.277，未查证。

依赖表面：`codex/cli`、`codex/app`、`claude-code/cli`、`claude-code/app`。桌面版本门那一句只依赖 Claude Code 两列里的未查证项。Cursor 见下一节，不要把这一节套到 Cursor。

### Cursor

Cursor 两列都读 `AGENTS.md`，含子目录，更近的优先。都读 `.cursor/rules` 里的 `.mdc`。该目录里的纯 `.md` 不读。`AGENTS.override.md` 两列都不读取。

用户规则在 Customize → Rules，不在文件系统上。团队规则只在 Team 和 Enterprise，顺序是 Team Rules、Project Rules、User Rules。

`cursor/cli` 另外读取项目根的 `CLAUDE.md`，和 `AGENTS.md`、`.cursor/rules` 一起用。子目录 `CLAUDE.md` 未查证。`cursor/app` 的 Rules 页没有写 `CLAUDE.md`，这一格是未查证。

三份同时存在时谁优先，Cursor 文档只写 alongside。未查证。

依赖表面：`cursor/cli`、`cursor/app`。`CLAUDE.md` 那两句分别只依赖对应的一列。

### Grok Build CLI

Grok Build CLI 读取 `AGENTS.md`，也读取 `CLAUDE.md`、`Claude.md`、`CLAUDE.local.md`、`Agents.md`、`AGENT.md`。同一目录里这几份都加载。更近的目录压过更远的。`AGENTS.override.md` 不读取。

家目录规则在 `~/.grok/rules/*.md`。兼容扫描默认开时，还会读 `~/.claude/rules/`、`~/.cursor/rules/` 里的 `*.md`，以及 `~/.claude/`、`~/.cursor/` 里的同名指令文件。`.cursor/rules` 里的 `.mdc` 不读取。该目录里的 `.md` 会读。Cursor 自己的两列相反：纯 `.md` 不读，`.mdc` 读。

被 `.gitignore` 忽略的指令文件会跳过。user guide 另写项目规则要目录已信任。docs.x.ai 的 AGENTS.md 页没有写这道门。同一目录里 `AGENTS.md` 和 `CLAUDE.md` 谁优先，未查证。

依赖表面：`grok/cli`。`.mdc` 那两句同时依赖 Cursor 两列。不要把这一节套到其他列。

### CodeBuddy Code

`CODEBUDDY.md` 是主指令文件。项目级 `./CODEBUDDY.md` 或 `./.codebuddy/CODEBUDDY.md`，用户级 `~/.codebuddy/CODEBUDDY.md`，本地 `./CODEBUDDY.local.md`（自动进 `.gitignore`）。启动时从当前目录向上递归加载沿途的 `CODEBUDDY.md` 和 `CODEBUDDY.local.md`。模块化规则放 `.codebuddy/rules/*.md`，但只加载当前工作目录那一层，父目录的规则目录不加载。

`AGENTS.md` 是回退项：存在 `CODEBUDDY.md` 时用它，不存在时才用 `AGENTS.md`。这是 CodeBuddy 与 Claude Code 相反的地方——Claude Code 是有 `CLAUDE.md` 时不用 `AGENTS.md`，CodeBuddy 是有 `CODEBUDDY.md` 时不用 `AGENTS.md`。

`CLAUDE.md` 不读取。要给 CodeBuddy 用，troubleshooting 给的是软链或复制成 `CODEBUDDY.md`。`AGENTS.override.md` 在文档里没有出现，未查证。

要给两家同时用，最省事的写法是各自一份：仓库里同时放 `CODEBUDDY.md` 和 `CLAUDE.md`，不要指望一方读另一方的文件名。只放 `AGENTS.md` 也能让 CodeBuddy 读到，但只要仓库里出现了 `CODEBUDDY.md`，那份 `AGENTS.md` 就会被跳过。

反过来：另外七列都不读 `CODEBUDDY.md`。Codex 两列、Claude Code 两列、Grok Build CLI 记为不读取（各自的文档都把会读的文件名或范围列清楚了），`cursor/cli` 记为不读取，`cursor/app` 是未查证——它的 Rules 页连 `CLAUDE.md` 都没写。Codex 留了口子：把 `CODEBUDDY.md` 写进 `project_doc_fallback_filenames` 就能生效。

依赖表面：`codebuddy/cli`。Web UI 与 CLI 同进程，但 webui 档案把这些格记为未查证，不要把这一节套到 `codebuddy/webui`。

## Skill

### 只覆盖 Codex

仓库 skill 放在从当前目录到仓库根的 `.agents/skills/<name>/SKILL.md`。个人 skill 放 `~/.agents/skills`。

不要只放在 `.codex/skills`。2025-12-19 的发布说明用过那条路径。2026-10-05 的发现表没有它。旧路径是否还生效，未查证。

要分发给别人时，把 skill 打成插件。插件还可以带 MCP 和 hooks。

依赖表面：`codex/cli`、`codex/app`。

### 已覆盖的六列

给 Claude Code 用的仓库 skill 放 `.claude/skills/<name>/SKILL.md`。个人 skill 放 `~/.claude/skills`。Claude Code 的 skills 页没有 `.agents/skills`、`~/.agents/skills`、`.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills`。这两列记为不读取。

Cursor 两列的发现表会读 `.agents/skills`、`.cursor/skills`、`.claude/skills`、`.codex/skills`，以及这四条的 `~/` 路径。同名是否合并，Cursor 的 skills 页没有写。

Codex 的发现表按范围穷举：仓库 `.agents/skills`、用户 `~/.agents/skills`、管理员 `/etc/codex/skills`、随安装捆绑。表外的路径都不读取。因此 Codex 不读 `.claude/skills`、`~/.claude/skills`、`.cursor/skills`、`~/.cursor/skills`、`.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`。`.codex/skills` 和 `~/.codex/skills` 仍是未查证，不是不读取：2025-12-19 的 changelog 用过这条路径，旧路径是否还在生效没有定论。skill 只放进 `.claude/skills` 或 `.cursor/skills`，Codex 不会发现。

同名 skill 在 Claude Code 里不合并：企业覆盖个人，个人覆盖项目。

依赖表面：六列。Codex 的八条不读取只依赖两份 Codex 档案的发现表。Claude Code 的不读取只依赖 Claude Code 两列。Cursor 的发现表只依赖 Cursor 两列。

### Grok Build CLI

发现表会读 `.grok/skills`、`~/.grok/skills`、`.agents/skills`、`~/.agents/skills`、`.claude/skills`、`~/.claude/skills`、`.cursor/skills`、`~/.cursor/skills`。`.codex/skills` 和 `~/.codex/skills` 不读取。

同名不合并。高优先级目录覆盖低的。同一层 `.grok` 和 `.agents` 谁覆盖谁，user guide 只写 alongside。未查证。

Claude 和 Cursor 的兼容扫描默认开。未信任目录会跳过项目 skill。

那六列在 2026-10-05 已回填：全部不读 `.grok/skills` 和 `~/.grok/skills`，依据都是各自的发现表。skill 只放在 `.grok/skills` 时，只有 `grok/cli` 会发现。

依赖表面：`grok/cli`。那六列不读取的结论分别依赖各自那份档案。

### CodeBuddy Code

仓库 skill 放 `.codebuddy/skills/<name>/SKILL.md`，个人 skill 放 `~/.codebuddy/skills/<name>/SKILL.md`，插件 skill 带命名空间。优先级项目 > 用户 > 插件。

`.claude/skills` 和 `~/.claude/skills` 不读取：troubleshooting 的「从 Claude Code 迁移」给的是软链或复制。`.agents/skills`、`~/.agents/skills`、`.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills`、`.grok/skills`、`~/.grok/skills` 在文档里没有出现，未查证。

要分发给别人时，把 skill 打成插件，走插件市场。插件还能带 `commands/`、`agents/`、`hooks/hooks.json`、`.mcp.json`、`.lsp.json`。

那七列在 2026-10-05 已回填：全部不读 `.codebuddy/skills` 和 `~/.codebuddy/skills`，依据都是各自的发现表。skill 只放在 `.codebuddy/skills` 时，只有 `codebuddy/cli` 会发现。

依赖表面：`codebuddy/cli`。那七列不读取的结论分别依赖各自那份档案。`codebuddy/webui` 这一格是未查证。

## 配置

### 只覆盖 Codex

MCP 可以写在 `~/.codex/config.toml`，两边都会读。项目级 `.codex/config.toml` 只有在项目被信任后才加载。

模型提供商、`openai_base_url` 和 `model_providers` 写在用户级配置。写进项目级文件会被忽略。

API key 和 ChatGPT 登录是两套计费和功能范围。需要套餐额度和工作区功能时用 ChatGPT 登录。CI 里的本机 Codex 用 API key。

依赖表面：MCP 共享依赖 `codex/cli`、`codex/app`。提供商键的忽略规则来自 Codex 配置参考，CLI 档案已按该参考填写。桌面应用是否读取每一个非 MCP 键，仍是 app 档案里的未查证项。

### Claude Code

终端和桌面 Code 标签读同一组 JSON：托管设置、`.claude/settings.local.json`、`.claude/settings.json`、`~/.claude/settings.json`。优先级从高到低是托管、命令行、项目本地、共享项目、用户。`permissions.allow` 这类列表会合并。

CLI 的自定义端点写 `ANTHROPIC_BASE_URL`，可以在 shell 或设置文件的 `env` 里。桌面 Code 标签不读这个变量，也不从 `settings.json` 取网关地址。桌面的网关填在第三方推理配置里。

CLI 可以用 `ANTHROPIC_API_KEY`。桌面 Code 标签不读这把 key，默认走 OAuth。

依赖表面：`claude-code/cli`、`claude-code/app`。

### Cursor

编辑器的提供商 key 填在 Cursor Settings > Models。请求经过 Cursor 后端做 prompt 组装。帮助页没有写自定义 base URL，总表是未查证。

CLI 的 `CURSOR_API_KEY` 是 Cursor 账号的 Dashboard key，不是提供商 key。CLI 点名的自带凭据是 Bedrock。编辑器里保存的 OpenAI、Anthropic、Google、Azure key 会不会被 CLI 使用，未查证。

CLI 的全局配置是 `~/.cursor/cli-config.json`。项目级 `.cursor/cli.json` 只能写权限。编辑器不读这份 CLI 文件。

依赖表面：`cursor/cli`、`cursor/app`。Dashboard key 和 Bedrock 那句只依赖 `cursor/cli`。Settings > Models 只依赖 `cursor/app`。

### Grok Build CLI

用户配置是 `~/.grok/config.toml`。项目 `.grok/config.toml` 只贡献 MCP、插件和权限规则。模型、`base_url` 和 `permission_mode` 写进项目文件不会生效。

自定义模型可以写任意 model id 和 `base_url`。`api_backend` 在 `responses`、`chat_completions`、`messages` 里选。设置页示例把 `grok-4.7` 写成 `responses`。

`XAI_API_KEY` 和浏览器登录可以同时存在。按模型解析时，`model.api_key` 先于 `model.env_key`，然后是会话 token，然后是 `XAI_API_KEY`。

依赖表面：`grok/cli`。

### CodeBuddy Code

四层 JSON，优先级从高到低：命令行参数、`.codebuddy/settings.local.json`、`.codebuddy/settings.json`、`~/.codebuddy/settings.json`。企业策略文件写在 hooks 的作用域表里，没有给路径，未查证。

项目级不能覆盖两处：`permissions.defaultMode: "auto"` 只能由用户级或 CLI 授予；顶层 `autoMode` 不读共享项目 `.codebuddy/settings.json`。

MCP 三个作用域，`local > project > user`：USER `~/.codebuddy/.mcp.json`，PROJECT `<项目根>/.mcp.json`，LOCAL 写在 `~/.codebuddy.json` 的项目条目里。同作用域的多个文件不合并，只取第一个存在的。项目作用域的服务器首次连接要用户审批。

自定义端点写 `CODEBUDDY_BASE_URL`，key 是 `CODEBUDDY_API_KEY`、`apiKeyHelper` 或 `CODEBUDDY_AUTH_TOKEN`，优先级依次递减。任意兼容 Anthropic 协议的第三方模型都能接，`--model` 直接接受模型 ID、名称或别名。

依赖表面：`codebuddy/cli`。`codebuddy/webui` 的认证另有网关一层（`--auth password|none`、Bearer、Cookie），配置与 MCP 两格是未查证。

## 不能统一的部分

- 线上协议。Codex 两列的现行值是 Responses。Claude Code 两列的现行值是 Anthropic API。Cursor 两列的请求经过 Cursor 后端，下游格式未查证。Grok Build CLI 的自定义模型在 `responses`、`chat_completions`、`messages` 里选。浏览器登录的内置模型默认协议未查证。CodeBuddy 两列是「兼容 Anthropic 协议」的端点，文档没有给正式协议名，也没有写协议版本。依赖表面：九列。Grok 那句只依赖 `grok/cli`，CodeBuddy 那句只依赖 CodeBuddy 两列。
- 动态 workflow。Claude Code 两列有 `Workflow` 工具。Grok Build CLI 有 `/workflow`，文件是 `.rhai`。CodeBuddy 两列有 Dynamic Workflow（研究预览，v2.105.0+ 默认开），是现场写 JS 编排脚本后台调度子 agent，并发上限 16、单次 run 上限 1000 个 agent。Codex 两列和 Cursor 两列的档案写没有同名编排器。依赖表面：九列。CodeBuddy 那句只依赖 CodeBuddy 两列，其中脚本调度细节只有 `codebuddy/cli` 档案写了。
- 定时任务。Codex 只在桌面应用和 ChatGPT 网页管理，CLI 没有这个界面。Claude Code CLI 有会话内的 `CronCreate` 和 `/loop`。Claude Desktop 的 Code 标签另有本机持久定时任务。Cursor 两列有 `/loop`，持久任务列表未查证，总表记为「部分」。Grok Build CLI 有 `/loop`，7 天后过期，同时最多 50 个。进程退出后还跑不跑，未查证，总表记为「部分」。CodeBuddy 两列方向相反：CLI 的 `CronCreate` 默认会话级、退出即清除；Web UI 创建或编辑任务时可以启用持久化，写进项目目录、重启后恢复，同项目有就绪 daemon 时由 daemon 承载。总表因此一列「部分」一列「有」。Cursor Automations 的定时触发是未写表面。依赖表面：九列。Grok 那句只依赖 `grok/cli`，CodeBuddy 那句只依赖 CodeBuddy 两列。
- 自由问卷。Claude Code CLI 的 `AskUserQuestion` 有选择题，也有 `Other` 和备注栏。Claude Desktop 的对话框是否同一套控件，未查证，总表记为「部分」。Cursor CLI 的 changelog 写澄清问题一次一道，并带自由文本 Other。Cursor 编辑器有 Ask questions，选项形态未查证，总表记为「部分」。Grok Build CLI 的 `ask_user_question` 有选项，也有自由文本行。Codex 两列的用户文档没有把对应界面写实。CodeBuddy CLI 的 `AskUserQuestion` 文档只写「多选问题」，另有 `AskUserForStructuredInput` 走 JSON Schema 表单（需开关和客户端 capability），有没有自由文本行未查证，两列总表都是未查证。依赖表面：九列。Grok 那句只依赖 `grok/cli`，CodeBuddy 那句只依赖 CodeBuddy 两列。
- Skill 目录不能收成一处。见上面的 Skill 节。
- 插件市场。Claude Code 两列、Cursor 两列、Grok Build CLI、CodeBuddy 两列都有：装插件的那套命令和插件包结构各不相同，但都能把 skills、agents、hooks、MCP 打成一个包分发出去。Codex 两列是未查证：插件是它的分发单位，但远程市场这层这次读不到文档正文。依赖表面：九列。
- 外部消息渠道。只有 `claude-code/cli` 和 CodeBuddy 两列确认有：把 IM 或 webhook 事件推进正在运行的会话，带发送者白名单或配对。Claude Code 那边是研究预览，需要 Bun 和插件安装；CodeBuddy 那边微信内置，Telegram 和 Discord 要装插件。`claude-code/app` 未查证，channels 页只按终端和持久终端写。Codex 两列、Cursor 两列、Grok Build CLI 都是未查证：它们有云端自动化或事件触发任务，但那是另起会话，不是推进本地会话。依赖表面：九列。
- 常驻守护进程。只有 `claude-code/cli` 和 CodeBuddy 两列确认有。Claude Code 是后台会话的 supervisor 进程加 `claude daemon` 子命令；CodeBuddy 是 `daemon start` 常驻服务，可以注册成系统服务。其余七列未查证：文档没有写可注册成系统服务的常驻进程，也没有 `--serve` 这类本地常驻服务。依赖表面：九列。
- Hooks。Cursor 编辑器在第三方导入开着时加载 Claude Code 的 `settings.json` hooks，默认开。这些 hook 排在 Cursor 自己的 enterprise、team、project、user 后面。`loop_limit` 在 Cursor 原生 hook 默认是 5，文档写 Claude Code hooks 的这项默认不限制。CLI 是否看同一个开关，未查证。CLI changelog 写它会合并 Claude Code 的 `settings.json` hooks。依赖表面：`cursor/cli`、`cursor/app`、`claude-code/cli`、`claude-code/app`。
- Grok Build CLI 会读 Claude Code 的 `.claude/settings.json` hooks，也会读 Cursor 的 `.cursor/hooks.json`。项目钩子要先信任。依赖表面：`grok/cli`。Claude 和 Cursor 的钩子文件格式分别依赖它们自己的档案，这一句只写 Grok 会加载。
- 桌面构建和 CLI 版本不是同一个号。Codex 桌面最近定位到 26.924，CLI 是 0.160.0。Claude Code CLI 是 2.1.289，桌面构建号未查证。Cursor CLI 的最新 changelog 标题是 2026-08-26，没有 semver。Cursor 编辑器构建号未查证，产品 changelog 最新标题是 2026-09-23。Grok Build 这一轮只有 CLI 1.0.46，没有写成这个产品的桌面 GUI。CodeBuddy 是唯一的例外：Web UI 由同一个 CLI 进程提供，没有独立版本号，两列都记 2.161.2。把某一侧刚出现的行为写成另一侧的行为之前，先核对那一侧的 changelog。依赖表面：九列。Grok 那句只依赖 `grok/cli`，CodeBuddy 那句只依赖 CodeBuddy 两列。
