# Grok Build

```yaml
agent: grok
vendor: SpaceXAI
status: 已填写
researched_at: 2026-10-05
```

## 产品线

SpaceXAI 的编码代理产品名是 Grok Build。命令是 `grok`。文档站标题也用 SpaceXAI。npm 组织是 `@xai-official`。API 主机是 `api.x.ai`。

这一轮只写终端。交互 TUI、`grok -p` 和无界面的 ACP（`grok agent stdio`）是同一个二进制，不另建目录。

## 表面清单

| 表面 | 官方名称 | 档案 |
| --- | --- | --- |
| cli | Grok Build | [已填写](cli/profile.md)，稳定版 1.0.46 |
| web | Grok 网页和手机里的构建 | 未写 |
| bot | Grok Bot | 未写 |

Grok Build 的文档没有把一块桌面 GUI 写成这个产品的表面。`grok dashboard` 是终端里的会话总览，不是另一个应用。

[Grok](https://docs.x.ai/grok/overview) 是 grok.com 和 iOS、Android 上的助手。2026-08-19 的新闻写，网页和手机上可以在对话里生成应用，并导出到 GitHub 后改用终端里的 Grok Build。那是未写表面。不要把终端档案抄到那边。

[Grok Bot](https://docs.x.ai/grok-bot/overview) 是另一产品。它有 macOS、Windows、Linux 桌面应用，以及 iOS 和 Android。Bot 在云端电脑上工作。不要把它的浏览器、记忆或定时任务抄到 Grok Build CLI。

## 跨表面事实

这一轮只有 CLI 一份档案。指令文件、skill 和协议都写在那份档案里。

### 协议时间线

CLI 的自定义模型用 `api_backend` 选择 `responses`、`chat_completions` 或 `messages`。设置页的示例把 `grok-4.7` 写成 `responses`。浏览器登录时内置模型默认走哪一种，设置参考没有写成单一值。未查证。

本次没有在 changelog 里定位到「改为只走某一种协议」的版本。分界版本未查证。

## 来源

| 文档 | URL | 查证日期 |
| --- | --- | --- |
| Grok Build | https://docs.x.ai/build/overview | 2026-10-05 |
| Changelog，1.0.46 | https://x.ai/build/changelog | 2026-10-05 |
| Settings | https://docs.x.ai/build/settings | 2026-10-05 |
| Grok | https://docs.x.ai/grok/overview | 2026-10-05 |
| Grok Build on web and mobile | https://x.ai/news/grok-build-for-everyone | 2026-10-05 |
| Grok Bot | https://docs.x.ai/grok-bot/overview | 2026-10-05 |
