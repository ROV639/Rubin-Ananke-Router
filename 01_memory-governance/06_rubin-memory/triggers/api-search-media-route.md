---
title: API Search Media Route Trigger Card
index_id: trigger-api-search-media-route
memory_layer: trigger
status: active
scope:
  - tool
  - ops
  - content
source_system:
  - claude-code
  - codex
  - rubin-memory
memory_type:
  - workflow
  - tool
sensitivity: medium
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/.codex/skills/api-search-media-route/SKILL.md
  - /Users/robin/.claude/skills/api-search-media-router/SKILL.md
exclude_from_startup: true
---

# api-search-media-route 触发卡

> 搜索、事实核查、多媒体工具选择的短门禁。目标是先路由，再执行。

```text
触发：搜索/查资料/事实核查/找来源/国内平台搜索/视频音频理解/B-roll/图片搜索/生图工具选择。
先查：api-search-media-route SKILL 首屏路由表；高频记忆查 rubin_memory.sh template minimax-usage。
必须：资讯搜索优先；Tavily 走 Key 池 wrapper；高时效信息联网核查并列来源。
禁止：直接用 MiniMax 搜索替代普通搜索；把单 key tvly 失败当全局失败；输出完整 Key/Token/Cookie。
下一步：判定任务类型 → 选 Tavily/multi-search/MiniMax/chatgpt-imagegen/宿主视觉 → 执行后记录来源或产物。
```

## 常见分流

- 最新事实 / 官方资料：先搜索，不凭记忆。
- 中文平台 / 国内信息：优先 multi-search-engine 或中文搜索入口。
- 读视频 / TTS / BGM / STT：MiniMax 是主动能力。
- 未指定工具的生图：ChatGPT/Codex 生图优先。
- MiniMax 生图：只有 Robin 明确点名 MiniMax 或 `image-01`。
