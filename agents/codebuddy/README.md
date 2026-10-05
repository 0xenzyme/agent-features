# CodeBuddy Code

```yaml
agent: codebuddy
vendor: 腾讯（Tencent / 腾讯云）
status: 已填写
researched_at: 2026-10-05
```

## 产品线

CodeBuddy Code 是腾讯的智能编程工具，文档自述为「腾讯云智能编程助手」，npm 包名 `@tencent-ai/codebuddy-code`，命令 `codebuddy`（短名 `cbc`）。产品页两处：海外 https://www.codebuddy.ai/cli ，国内 https://copilot.tencent.com/cli 。

这一轮写了两个表面：终端 CLI，和同一个进程提供的 Web UI。

Web UI 不是独立产品。它由 `codebuddy --serve` 或 daemon 常驻提供，文档写「不依赖终端窗口」；两个表面共用同一个版本号。

文档里还有其他入口，这一轮不建目录：

- IDE 集成，两种形态：ACP 协议（Zed 等支持 ACP 的编辑器把 CodeBuddy Code 当 Agent Server 用），以及 `--ide` / `/ide` 插件后端（VS Code、Cursor、Windsurf、JetBrains 系列，插件通过本地 MCP 传工作区上下文，并暴露 `openFile` / `openDiff` / `getDiagnostics` 等能力）。
- Agent SDK（JS 与 Python）。它在同一份 release note 里有自己的版本号：v2.161.2 对应 Agent SDK JS v0.3.271、Agent SDK Python v0.3.270。SDK 与 CLI 不在同一套 semver 上。
- HTTP API（`/api/v1/*`，含 Swagger UI）。它是 Web UI 背后的接口，不是独立表面。

另有 WorkBuddy 这一款产品共用 CodeBuddy 引擎，文档在两个地方提到：安装页把「与其他使用 CodeBuddy 引擎的应用（如 WorkBuddy）共存时避免配置冲突」写成 `CODEBUDDY_CONFIG_DIR` 的用途之一；Web UI 页写 WorkBuddy 不展示、不解析主 Agent chip，`allowUnopted` 缺省关时 WorkBuddy 保持原生 `cli`。WorkBuddy 不在这一轮的表面清单里，未写。

## 表面清单

| 表面 | 官方名称 | 档案 |
| --- | --- | --- |
| cli | CodeBuddy Code CLI | [已填写](cli/profile.md)，2.161.2 |
| webui | Web UI | [已填写](webui/profile.md)，与 CLI 同版 2.161.2 |
| ide | IDE 集成（ACP 协议 / `--ide` 插件后端） | 未写 |
| sdk | Agent SDK（JS / Python） | 未写 |

`http-api` 不建目录：它是 Web UI 的接口层，事实并入 webui 档案。

## 跨表面事实

### 指令文件

只有 cli 档案写了完整规则：`CODEBUDDY.md` 为主，`AGENTS.md` 为回退项（存在 `CODEBUDDY.md` 时不用 `AGENTS.md`），`CLAUDE.md` 不读取。webui 文档的概述只写「与终端界面相同的核心能力」，没有逐项复述。因此 webui 档案里这些格是未查证，不是照抄 cli。

### Skill

项目级 `.codebuddy/skills/<name>/SKILL.md`，用户级 `~/.codebuddy/skills/<name>/SKILL.md`，插件级 `<plugin>/skills/<name>/SKILL.md`（带命名空间）。优先级项目 > 用户 > 插件。这套路径只在 cli 文档里写明，webui 侧未查证。

`.claude/skills` 与 `~/.claude/skills` 出现在 troubleshooting 的迁移方案里（软链或复制），记为不读取。`.agents/skills`、`~/.agents/skills`、`.cursor/skills`、`~/.cursor/skills`、`.codex/skills`、`~/.codex/skills` 在本次读到的文档里没有出现，未查证。

插件与插件市场是分发单位：`.codebuddy-plugin/plugin.json` 加 `commands/`、`agents/`、`skills/`、`hooks/hooks.json`、`.mcp.json`、`.lsp.json`。

### 协议时间线

现行文档要求「兼容 Anthropic 协议」的端点。第三方模型那一段写明：对接任意兼容 Anthropic 协议的第三方模型服务（如 DeepSeek），只需配置 Base URL、API Key 和模型变量，无需改 `models.json`。

文档没有给出像 Responses 那样的正式协议名，也没有写协议版本。协议名与版本未查证。

早期版本是否接受过别的协议，本次没有在官方 release notes 里定位到任何分界版本。分界版本未查证。

### 版本

单一 semver，CLI 与 Web UI 同号。v2.161.2 的 release note 同时给出 Agent SDK JS v0.3.271 与 Agent SDK Python v0.3.270，这两个号属于 SDK，不是 CLI 版本。

hooks.md 开头写「本文档针对 CodeBuddy Code v1.16.0 及以上版本中提供的 Hooks 实现」，与 2.x 的版本体系对不上。照原文记，不推断，也不拿它当分界版本。

## 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| 文档首页 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/README.md | 2026-10-05 |
| Release notes 索引 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/README.md | 2026-10-05 |
| v2.161.2 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/release-notes/v2.161.2.md | 2026-10-05 |
| 安装指南 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/installation.md | 2026-10-05 |
| IDE 集成 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/ide-integrations.md | 2026-10-05 |
| SDK | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/sdk.md | 2026-10-05 |
| Web UI | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/web-ui.md | 2026-10-05 |
| 记忆 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/memory.md | 2026-10-05 |
| Skills | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/skills.md | 2026-10-05 |
| 插件 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/plugins.md | 2026-10-05 |
| 环境变量 | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/env-vars.md | 2026-10-05 |
| Hooks | https://cnb.cool/codebuddy/codebuddy-code/-/git/raw/main/docs/hooks.md | 2026-10-05 |
| npm 包 | https://registry.npmjs.org/@tencent-ai/codebuddy-code/latest | 2026-10-05 |
