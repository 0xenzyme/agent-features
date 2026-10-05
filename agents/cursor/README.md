# Cursor

```yaml
agent: cursor
vendor: Cursor
status: 已填写
researched_at: 2026-10-05
```

## 产品线

Cursor 的编码代理有多条表面。这一轮写终端 CLI，以及桌面编辑器里的 Agent。

CLI 的命令是 `agent`。编辑器里的 Agent 从侧栏进入，文档写的快捷键是 Cmd+I。

## 表面清单

| 表面 | 官方名称 | 档案 |
| --- | --- | --- |
| cli | Cursor CLI | [已填写](cli/profile.md)，changelog 最新标题 2026-08-26，semver 未查证 |
| app | Cursor 编辑器 | [已填写](app/profile.md)，changelog 最新标题 2026-09-23，构建号未查证 |
| cloud | Cloud Agents | 未写 |
| web | cursor.com/agents | 未写 |
| jetbrains | JetBrains | 未写 |
| xcode | Xcode | 未写 |
| mobile | 移动端 | 未写 |
| sdk | SDK | 未写 |
| automations | Automations | 未写 |
| bugbot | Bugbot / Security Review / Rollouts | 未写 |
| projects | Projects | 未写 |

文档索引里还有 Origin。这一轮不建目录。

Cloud Agents 的限制不要抄到 CLI 或编辑器上。Automations 的定时触发和 Memories 工具属于未写表面。

## 跨表面事实

### 指令文件

Rules 页对编辑器成立，CLI 页写 CLI 使用同一套规则。项目规则是 `.cursor/rules` 里的 `.mdc`。该目录里的纯 `.md` 被忽略。用户规则在 Customize → Rules，迁移页写明它们不在文件系统上。团队规则来自 dashboard，只在 Team 和 Enterprise。冲突时的顺序是 Team Rules、Project Rules、User Rules。适用的规则合并，靠前的来源优先。

`AGENTS.md` 可以放在仓库根和子目录。子目录文件和父目录拼接，更近的优先。`AGENTS.override.md` 两份档案都没有在文档里看到，记为不读取。

CLI 使用页另写：项目根的 `AGENTS.md` 和 `CLAUDE.md` 会跟 `.cursor/rules` 一起套用。编辑器的 Rules 页没有写 `CLAUDE.md`。编辑器是否读取 `CLAUDE.md`，未查证。

`AGENTS.md`、`CLAUDE.md`、`.cursor/rules` 同时存在时谁压过谁，两边的文档都只写 alongside，没有顺序。未查证。

### Skill

现行发现表对 Agent 列出四对目录：`.agents/skills` 与 `~/.agents/skills`，`.cursor/skills` 与 `~/.cursor/skills`，以及兼容用的 `.claude/skills`、`.codex/skills`、`~/.claude/skills`、`~/.codex/skills`。格式是目录加 `SKILL.md`。同名是否合并，该页没有写。未查证。2026-10-05 回填：发现表同样没有 `.grok/skills`、`~/.grok/skills`、`.codebuddy/skills`、`~/.codebuddy/skills`，这四条两列都记为不读取。

`~/.agents/skills/` 和未同步的本机 skill 不会复制到 Cloud Agents、Agents Window 的远程 SSH，或自托管 worker。这三处是未写表面。

### 协议时间线

BYOK 帮助页写，请求都经过 Cursor 的服务器做最终的 prompt 组装。下游是 Responses、Anthropic Messages 还是别的格式，打开的模型页和帮助页都没有写。未查证。

- 编辑器可以在 Settings > Models 填 OpenAI、Anthropic、Google、Azure OpenAI、AWS Bedrock 的 key。任意模型 id 不开放。
- CLI 的 `CURSOR_API_KEY` 是 Cursor Dashboard 的用户 key，用来登录账号，不是提供商 key。CLI 文档点名的自带凭据是 Bedrock 的 `/bedrock`。OpenAI、Anthropic、Google、Azure 的 key 能否用于 CLI，未查证。
- 自定义 base URL，两边打开的页面都没有写。未查证。
- 没有在 changelog 里定位到改用某一种下游协议的版本。分界版本未查证。

## 来源

- https://cursor.com/docs/rules 。查证日期：2026-10-05。
- https://cursor.com/docs/skills 。查证日期：2026-10-05。
- https://cursor.com/docs/cli/using 。查证日期：2026-10-05。
- https://cursor.com/help/models-and-usage/api-keys 。查证日期：2026-10-05。
- https://cursor.com/docs/llms.txt 。查证日期：2026-10-05。
