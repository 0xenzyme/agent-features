# 总表

单元格是短状态。细节在列头链到的档案里。

已覆盖表面：

- [codex/cli](../agents/codex/cli/profile.md)：Codex CLI 0.160.0
- [codex/app](../agents/codex/app/profile.md)：ChatGPT desktop app 中的 Codex，最近定位到的桌面版本 26.924

查证日期：2026-10-05。

| 能力 | codex/cli | codex/app |
| --- | --- | --- |
| `AGENTS.md` | 读取 | 读取 |
| `AGENTS.override.md` | 读取 | 读取 |
| `CLAUDE.md` 默认发现 | 不读取 | 不读取 |
| 项目 skill `.agents/skills` | 读取 | 读取 |
| 用户 skill `~/.agents/skills` | 读取 | 读取 |
| plan mode | 有 | 有 |
| 定时任务管理 | 无 | 有 |
| memories | 有 | 有 |
| 向用户提问（审批） | 有 | 有 |
| 向用户提问（自由问卷） | 未查证 | 未查证 |
| shell | 有 | 有 |
| 文件编辑 | 有 | 有 |
| 子 agent | 有 | 有 |
| MCP | 有 | 有 |
| 浏览器操作 | 无 | 部分 |
| 网页搜索 | 有 | 未查证 |
| 自带 API key | 有 | 有 |
| 自定义 base URL | 有 | 未查证 |
| 任意模型 | 限制 | 限制 |
| 线上协议 | responses | responses |
| 沙箱 | 有 | 有 |
| hooks | 有 | 有 |
