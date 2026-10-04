# Coding agent 能力差距

这个仓库记录 coding agent 的能力差，以及这些差会让同一个仓库走出什么不同结果。

比较单位是产品表面，不是厂商。`agents/codex/cli` 和 `agents/codex/app` 是两列。

事实只来自官方文档。每条结论带版本和查证日期。文档没写的项写成「未查证」。

## 怎么读

- [总表](comparison/matrix.md) 看一行能力在各表面上的短状态。
- [可移植写法](comparison/portability.md) 只覆盖已经写完的表面。
- `agents/<agent>/README.md` 写这家的表面清单和跨表面事实。
- `agents/<agent>/<surface>/profile.md` 写该表面的档案。

## 怎么加一个 agent

按 [WORKFLOW.md](WORKFLOW.md) 做。队列在 [QUEUE.md](QUEUE.md)。

模板：

- [templates/agent.md](templates/agent.md)
- [templates/surface.md](templates/surface.md)
