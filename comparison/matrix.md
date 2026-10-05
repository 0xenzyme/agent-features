# 总表

单元格是短状态。细节在列头链到的档案里。

已覆盖表面：

- [codex/cli](../agents/codex/cli/profile.md)：Codex CLI 0.160.0
- [codex/app](../agents/codex/app/profile.md)：ChatGPT desktop app 中的 Codex，最近定位到的桌面版本 26.924
- [claude-code/cli](../agents/claude-code/cli/profile.md)：Claude Code CLI 2.1.289
- [claude-code/app](../agents/claude-code/app/profile.md)：Claude Desktop 的 Code 标签，应用构建号未查证
- [cursor/cli](../agents/cursor/cli/profile.md)：Cursor CLI，changelog 最新标题 2026-08-26，semver 未查证
- [cursor/app](../agents/cursor/app/profile.md)：Cursor 编辑器，构建号未查证
- [grok/cli](../agents/grok/cli/profile.md)：Grok Build 1.0.46
- [codebuddy/cli](../agents/codebuddy/cli/profile.md)：CodeBuddy Code CLI 2.161.2
- [codebuddy/webui](../agents/codebuddy/webui/profile.md)：CodeBuddy Code Web UI，与 CLI 同版 2.161.2

查证日期：2026-10-05。

| 能力 | codex/cli | codex/app | claude-code/cli | claude-code/app | cursor/cli | cursor/app | grok/cli | codebuddy/cli | codebuddy/webui |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `AGENTS.md` | 读取 | 读取 | 部分 | 部分 | 读取 | 读取 | 读取 | 部分 | 未查证 |
| `AGENTS.override.md` | 读取 | 读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 未查证 | 未查证 |
| `CLAUDE.md` 默认发现 | 不读取 | 不读取 | 读取 | 读取 | 读取 | 未查证 | 读取 | 不读取 | 未查证 |
| `CODEBUDDY.md` 默认发现 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 未查证 | 不读取 | 读取 | 未查证 |
| 项目 skill `.agents/skills` | 读取 | 读取 | 不读取 | 不读取 | 读取 | 读取 | 读取 | 未查证 | 未查证 |
| 用户 skill `~/.agents/skills` | 读取 | 读取 | 不读取 | 不读取 | 读取 | 读取 | 读取 | 未查证 | 未查证 |
| 项目 skill `.claude/skills` | 不读取 | 不读取 | 读取 | 读取 | 读取 | 读取 | 读取 | 不读取 | 未查证 |
| 用户 skill `~/.claude/skills` | 不读取 | 不读取 | 读取 | 读取 | 读取 | 读取 | 读取 | 不读取 | 未查证 |
| 项目 skill `.cursor/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 读取 | 读取 | 未查证 | 未查证 |
| 用户 skill `~/.cursor/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 读取 | 读取 | 未查证 | 未查证 |
| 项目 skill `.codex/skills` | 未查证 | 未查证 | 不读取 | 不读取 | 读取 | 读取 | 不读取 | 未查证 | 未查证 |
| 用户 skill `~/.codex/skills` | 未查证 | 未查证 | 不读取 | 不读取 | 读取 | 读取 | 不读取 | 未查证 | 未查证 |
| 项目 skill `.grok/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 未查证 | 未查证 |
| 用户 skill `~/.grok/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 未查证 | 未查证 |
| 项目 skill `.codebuddy/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 未查证 |
| 用户 skill `~/.codebuddy/skills` | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 不读取 | 读取 | 未查证 |
| plan mode | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 未查证 |
| 动态 workflow | 无 | 无 | 有 | 有 | 无 | 无 | 有 | 有 | 有 |
| 定时任务管理 | 无 | 有 | 部分 | 有 | 部分 | 部分 | 部分 | 部分 | 有 |
| memories | 有 | 有 | 有 | 有 | 未查证 | 未查证 | 有 | 有 | 未查证 |
| 插件市场 | 未查证 | 未查证 | 有 | 有 | 有 | 有 | 有 | 有 | 有 |
| 向用户提问（审批） | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 |
| 向用户提问（自由问卷） | 未查证 | 未查证 | 有 | 部分 | 有 | 部分 | 有 | 未查证 | 未查证 |
| shell | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 |
| 文件编辑 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 |
| 子 agent | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 |
| MCP | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 未查证 |
| 浏览器操作 | 无 | 部分 | 部分 | 有 | 未查证 | 有 | 未查证 | 未查证 | 未查证 |
| 网页搜索 | 有 | 未查证 | 有 | 有 | 有 | 有 | 有 | 有 | 未查证 |
| 外部消息渠道 | 未查证 | 未查证 | 有 | 未查证 | 未查证 | 未查证 | 未查证 | 有 | 有 |
| 常驻守护进程 | 未查证 | 未查证 | 有 | 未查证 | 未查证 | 未查证 | 未查证 | 有 | 有 |
| 自带 API key | 有 | 有 | 有 | 部分 | 部分 | 有 | 有 | 有 | 未查证 |
| 自定义 base URL | 有 | 未查证 | 有 | 有 | 未查证 | 未查证 | 有 | 有 | 未查证 |
| 任意模型 | 限制 | 限制 | 限制 | 限制 | 限制 | 限制 | 有 | 有 | 未查证 |
| 线上协议 | responses | responses | Anthropic API | Anthropic API | Cursor 后端 | Cursor 后端 | 自选 | Anthropic 兼容 | Anthropic 兼容 |
| 沙箱 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 未查证 |
| hooks | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 有 | 未查证 |
