# 可移植写法

已覆盖表面：

- [codex/cli](../agents/codex/cli/profile.md)，Codex CLI 0.160.0
- [codex/app](../agents/codex/app/profile.md)，ChatGPT desktop app 中的 Codex

查证日期：2026-10-05。

下面每条规则只依赖这两列。还没有 Claude Code、Cursor、Grok 的档案，不能把这些规则写成所有 coding agent 的规则。

## 指令文件

共享层用 `AGENTS.md`。全局偏好放 `~/.codex/AGENTS.md`。仓库规则放仓库根和更近的子目录。临时全局覆盖用 `~/.codex/AGENTS.override.md`，同一层有 override 时，普通 `AGENTS.md` 不读。

`CLAUDE.md` 不是默认指令文件。要给 Codex 用，把它导入成 `AGENTS.md`，或把文件名写进用户级 `project_doc_fallback_filenames`。导入和 fallback 是两件不同的事。

依赖表面：`codex/cli`、`codex/app`。

## Skill

仓库 skill 放在从当前目录到仓库根的 `.agents/skills/<name>/SKILL.md`。个人 skill 放 `~/.agents/skills`。

不要只放在 `.codex/skills`。2025-12-19 的发布说明用过那条路径。2026-10-05 的发现表没有它。旧路径是否还生效，未查证。

要分发给别人时，把 skill 打成插件。插件还可以带 MCP 和 hooks。

依赖表面：`codex/cli`、`codex/app`。

## 配置

MCP 可以写在 `~/.codex/config.toml`，两边都会读。项目级 `.codex/config.toml` 只有在项目被信任后才加载。

模型提供商、`openai_base_url` 和 `model_providers` 写在用户级配置。写进项目级文件会被忽略。

API key 和 ChatGPT 登录是两套计费和功能范围。需要套餐额度和工作区功能时用 ChatGPT 登录。CI 里的本机 Codex 用 API key。

依赖表面：MCP 共享依赖两列。提供商键的忽略规则来自 Codex 配置参考，CLI 档案已按该参考填写。桌面应用是否读取每一个非 MCP 键，仍是 app 档案里的未查证项。

## 不能统一的部分

- 线上协议只有 Responses。Chat Completions 和 Anthropic Messages 都不被当前 Codex 文档接受。早期 `wire_api = "chat"` 的删除版本未查证。
- 模型名不能任意填进一个 Chat Completions 兼容服务就用。自定义端点必须实现 `POST /v1/responses`，并保留工具调用和多回合续写。
- 定时任务只在桌面应用和 ChatGPT 网页管理。CLI 没有这个界面。事件触发任务两边都没有，它在网页和移动端。
- 审批是两边都有的提问方式。一次提 1 到 3 个自由问题的界面，两边的用户文档都没有写实，不能当成已有工具。
- 桌面应用的构建版本和 CLI 0.160.0 不是同一个版本号。把 CLI 上刚出现的行为写成桌面应用行为之前，先核对桌面 changelog。
