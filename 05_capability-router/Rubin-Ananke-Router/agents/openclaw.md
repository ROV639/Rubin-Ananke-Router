# Agent Adapter｜OpenClaw

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 定位

OpenClaw 是 Telegram/多通道自动化执行端，适合轻量任务、消息入口、自动化、发布链和远程调度。

## 执行规则

- 上下文要短，优先读 trigger/card 摘要，不读大文件。
- 对发布、排期、文件写入、删除操作，必须返回状态凭证。
- 长任务要避免直聊 session 爆上下文；必要时 reset/新开任务。
- 失败或超时要报告真实状态，不要文本补完成。

## 特别注意

OpenClaw 最容易出的问题是 session 长、上下文旧、任务慢但误以为卡死。装备 `publish-status-check`、`verification-before-finish`、`current-memory-writeback`。

