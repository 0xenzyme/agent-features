# Codex

```yaml
agent: codex
vendor: OpenAI
status: 已填写
researched_at: 2026-10-05
```

## 产品线

OpenAI 的 Codex 有多条表面。第一轮写了 CLI 和桌面应用里的 Codex。

2026-07-09 的 changelog 写明：Codex 并入 ChatGPT desktop app（条目版本 26.707）。已有 Codex app 用户通过更新保留项目、设置和工作流。本仓库的 `app` 表面指这次合并之后的桌面应用，不是一个仍独立发布的 Codex app。

## 表面清单

| 表面 | 官方名称 | 档案 |
| --- | --- | --- |
| cli | Codex CLI | [已填写](cli/profile.md)，稳定版 0.160.0 |
| app | ChatGPT desktop app 中的 Codex | [已填写](app/profile.md)，最近定位到的桌面版本 26.924 |
| ide | Codex IDE extension | 未写 |

Cloud、GitHub、SDK、app-server 是文档里的其他入口。它们不是这一轮的表面，也不建目录。

CLI 和桌面应用可以携带不同的 Codex 版本。故障排除页写明：功能可能先到其中一个表面，实验功能也可能先到 CLI。

## 跨表面事实

### 指令文件

两份已写档案使用同一套发现规则：全局 `~/.codex/AGENTS.override.md` 或 `AGENTS.md`，然后从项目根走到当前目录，每层最多一个文件。默认文件名不包括 `CLAUDE.md`。

### Skill

当前发现路径是 `.agents/skills` 和 `~/.agents/skills`。插件是安装分发单位。2025-12-19 的 changelog 曾写 `~/.codex/skills` 和 `.codex/skills`。现行发现表没有这两条路径。旧路径在 0.160.0 是否仍被扫描，未查证。

### 协议时间线

现行文档要求 Responses API。

- `model_providers.<id>.wire_api` 的唯一合法值是 `responses`。省略时默认也是 `responses`。
- 模型页写明：当前 Codex 版本不支持 `wire_api = "chat"`，只提供 Chat Completions 的端点也不够。
- 网关页写明：能用的 Chat Completions 或 Anthropic Messages 端点不能证明 Responses 兼容。网关要接受 `POST /v1/responses`。
- 只改内置 OpenAI 提供商的地址时，用用户级 `openai_base_url`。自定义提供商用 `[model_providers.<id>]`，并用 `env_key` 或 `requires_openai_auth` 取凭证。
- 项目级 `.codex/config.toml` 里的 `openai_base_url`、`model_provider`、`model_providers` 会被忽略。这些键放在用户级配置。

早期配置曾经接受 `wire_api = "chat"`。本次没有在官方 changelog 里定位到删除该值的版本号。分界版本未查证。现行两列都按 Responses 记录。

## 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Changelog，CLI 0.160.0 | https://learn.chatgpt.com/docs/changelog | 2026-10-05 |
| Changelog，Codex 并入桌面应用 26.707 | https://learn.chatgpt.com/docs/changelog | 2026-10-05 |
| Troubleshooting | https://learn.chatgpt.com/docs/reference/troubleshooting | 2026-10-05 |
| AGENTS.md | https://learn.chatgpt.com/docs/agent-configuration/agents-md | 2026-10-05 |
| Build skills | https://learn.chatgpt.com/docs/build-skills | 2026-10-05 |
| Models | https://learn.chatgpt.com/docs/models | 2026-10-05 |
| Configuration reference | https://learn.chatgpt.com/docs/config-file/config-reference | 2026-10-05 |
| Gateway compatibility | https://learn.chatgpt.com/docs/enterprise/gateway-compatibility | 2026-10-05 |
| Advanced configuration | https://learn.chatgpt.com/docs/config-file/config-advanced | 2026-10-05 |
