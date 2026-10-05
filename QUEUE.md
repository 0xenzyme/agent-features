# 队列

状态：`未开始`、`查证中`、`已填写`、`未回填`。

| 顺序 | slug | 官方产品 | 表面 | 状态 | 备注 |
| --- | --- | --- | --- | --- | --- |
| 1 | codex | Codex | `cli`、`app` | 已填写 | 第一轮样板。IDE extension 已确认，未写。2026-10-05 回填了 `.claude/skills`、`.cursor/skills`、`.grok/skills`、`.codebuddy/skills` 八格（发现表按范围穷举，全部不读取）、`CODEBUDDY.md`（不读取）、插件市场（未查证，插件页是客户端渲染读不到正文）、外部消息渠道与常驻守护进程（均未查证） |
| 2 | claude-code | Claude Code | `cli`、`app` | 已填写 | 桌面表面是 Claude Desktop 的 Code 标签。VS Code、JetBrains、网页、移动端已确认，未写。2026-10-05 回填：`.grok/skills` 与 `.codebuddy/skills` 四格不读取、`CODEBUDDY.md` 不读取；CLI 另有插件市场（有）、channels（有，研究预览）、`claude daemon` 常驻 supervisor（有）；桌面这三格分别是 有、未查证、未查证 |
| 3 | cursor | Cursor | `cli`、`app` | 已填写 | 编辑器是桌面 IDE。Cloud Agents、cursor.com/agents、JetBrains、Xcode、移动端、SDK、Automations、Bugbot、Projects 已确认，未写。两边都没有 semver。2026-10-05 回填：`.grok/skills` 与 `.codebuddy/skills` 四格不读取、插件市场两列都有（Cursor Marketplace 与 Customize）；`CODEBUDDY.md` 在 cli 是不读取、在 app 是未查证；外部消息渠道与常驻守护进程均未查证 |
| 4 | grok | Grok Build | `cli` | 已填写 | 厂商是 SpaceXAI。桌面 GUI 没有写成这个产品的表面。Grok 网页和手机里的构建、Grok Bot 已确认，未写。2026-10-05 回填：`.codebuddy/skills` 两条不读取、`CODEBUDDY.md` 不读取、插件市场有（user guide 写可经 marketplace 发布）、外部消息渠道与常驻守护进程未查证 |
| 5 | codebuddy | CodeBuddy Code | `cli`、`webui` | 已填写 | 厂商腾讯。两列同一进程、同一版本号 2.161.2。IDE 集成（ACP 协议与 `--ide` 插件后端）和 Agent SDK 已确认，未写。另有共用引擎的 WorkBuddy，未写 |
| 6 | kimi-code | 未确认 | 未确认 | 未开始 | 开始前核对官方产品名和表面 |
| 7 | zcode | 未确认 | 未确认 | 未开始 | 开始前核对官方产品名和表面 |

OpenCode 不在队列里。开放度用各表面 BYOK 节的「能否任意加模型」表达。
