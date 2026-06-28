# Bundle｜publishing-schedule

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 适用任务

Buffer、Hermes、OpenClaw、社媒发布、排期、草稿。

## 装备卡组

```text
publish-confirm
buffer-publish
publish-status-check
secret-redaction
current-memory-writeback
```

## 执行顺序

1. 确认账号、平台、正文、媒体、时间、立即/排期。
2. 调用对应发布工具。
3. 复查状态凭证。
4. 回写排期/失败/待确认。

## 必须验证

发布/排期必须有 post id、draft id、scheduled_at、message id、截图或状态返回。

## 回写要求

记录账号、时间、状态；不记录 token/cookie。

