---
title: API Search Key Rotation Trigger Card
index_id: trigger-api-search-key-rotation
memory_layer: trigger
status: active
scope:
  - tool
  - security
  - ops
source_system:
  - codex
  - claude-code
  - rubin-memory
memory_type:
  - workflow
  - tool
  - security
sensitivity: medium
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/.codex/skills/api-search-media-route/SKILL.md
  - /Users/robin/.codex/skills/api-search-media-route/scripts/tavily.sh
  - /Users/robin/.codex/skills/api-search-media-route/scripts/lib.sh
exclude_from_startup: true
---

# api-search-key-rotation 触发卡

> Tavily / 搜索聚合引擎 Key 轮换短门禁。目标：防止 Agent 看到单 key 失败就停，或绕过 wrapper 直接调用 `tvly`。

```text
触发：Tavily/search/extract/crawl/research/map 返回 401/403/429/432、quota、额度耗尽、单 key 失败。
先查：api-search-media-route SKILL 的 Key 池硬门禁；必要时看 scripts/tavily.sh 的 9-key 顺序。
必须：用 scripts/tavily.sh 或绝对路径 wrapper；让 wrapper 自动切下一把 key；看 usage.log 只看 HTTP 状态和脱敏 key 尾号。
禁止：直接判定 Tavily 不可用；直接调用 tvly 单 key；把 HTTP_432 当全局失败；输出完整 Key。
下一步：重跑 wrapper → 若仍 all_fail 才降级其他搜索 → 报告 provider/endpoint/HTTP 状态/脱敏 key 尾号。
```

## 关键事实

- `tvly` CLI 只用当前单把 Key，不代表 9-Key 池整体失败。
- `scripts/tavily.sh` 内置 9 个 key 的固定轮询顺序。
- `HTTP_401/403/429/432` 都应该切下一把 Key。
- `~/.config/search-router/usage.log` 可用于确认是否已轮换，但不能输出完整 Key。
