# Claude Code

```yaml
agent: claude-code
vendor: Anthropic
status: 已填写
researched_at: 2026-10-05
```

## 产品线

Anthropic 的 Claude Code 有多条表面。这一轮写终端 CLI，以及 Claude Desktop 里的 Code 标签。

桌面应用有 Chat、Cowork、Code 三个标签。本仓库的 `app` 表面只指 Code 标签。Cowork 不是这个表面。

## 表面清单

| 表面 | 官方名称 | 档案 |
| --- | --- | --- |
| cli | Claude Code CLI | [已填写](cli/profile.md)，changelog 最新条目 2.1.289 |
| app | Claude Desktop 的 Code 标签 | [已填写](app/profile.md)，应用构建号未查证 |
| vscode | VS Code extension | 未写 |
| jetbrains | JetBrains | 未写 |
| web | Claude Code on the web | 未写 |

Chrome 扩展是 CLI 和 VS Code 的浏览器集成，不单独建目录。手机上的 Dispatch 会在 Code 标签里开会话，不单独建目录。Agent SDK、云端会话、Slack 是文档里的其他入口，这一轮不建目录。

桌面页写明：Desktop 跑的是和 CLI 相同的底层引擎，并用图形界面呈现。两边可以同时开，会话列表各自保存，通过 `CLAUDE.md` 共享项目记忆。桌面应用内嵌的 CLI 版本号，桌面页和 CLI changelog 都没有写。未查证。

## 跨表面事实

### 指令文件

两份已写档案使用记忆页上的同一套规则。`CLAUDE.md` 是主文件，从宽到窄拼接，不互相覆盖。`AGENTS.md` 从 CLI 2.1.277 起可以当项目指令，默认只在工作目录及其上级都没有 `CLAUDE.md`、`.claude/CLAUDE.md`、`CLAUDE.local.md` 时读取。`AGENTS.override.md` 不读取。`.agents/` 目录下的指令不读取。`CODEBUDDY.md` 不在会读的清单里，两列都记为不读取。

桌面内嵌引擎是否已经不低于 2.1.277，未查证。因此「2.1.277 起」这条版本门对当前桌面构建是否成立，未查证。记忆页没有为桌面另写一套发现算法。

### Skill

当前发现路径是 `.claude/skills` 和 `~/.claude/skills`，再加上企业托管目录、插件和 claude.ai 账号同步的 skill。发现表没有 `.agents/skills`、`~/.agents/skills`、`.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills`。2026-10-05 回填：发现表同样没有 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`，这四条两列都记为不读取。

同名 skill 不合并。企业覆盖个人，个人覆盖项目。插件 skill 带命名空间，可以和同名的项目 skill 同时存在。

### 协议时间线

现行文档把模型请求发到 Anthropic API。

- CLI 用 `ANTHROPIC_BASE_URL` 改端点。`ANTHROPIC_API_KEY` 放进 `X-Api-Key`。`ANTHROPIC_AUTH_TOKEN` 放进 `Authorization: Bearer`。
- 桌面 Code 标签不读 `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN`、`apiKeyHelper`。网关地址来自第三方推理配置，不来自 `ANTHROPIC_BASE_URL` 或 `settings.json`。
- 本次没有在 changelog 里定位到改用 Responses 或 Chat Completions 的版本。分界版本未查证。
- 两列的现行文档都没有把 Responses 或 Chat Completions 写成可接受的线上协议。

## 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Changelog，CLI 2.1.289 | https://code.claude.com/docs/en/changelog | 2026-10-05 |
| Desktop application | https://code.claude.com/docs/en/desktop | 2026-10-05 |
| Memory | https://code.claude.com/docs/en/memory | 2026-10-05 |
| Skills | https://code.claude.com/docs/en/skills | 2026-10-05 |
| Authentication | https://code.claude.com/docs/en/authentication | 2026-10-05 |
| LLM gateway | https://code.claude.com/docs/en/llm-gateway-connect | 2026-10-05 |
