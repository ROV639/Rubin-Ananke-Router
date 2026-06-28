---
title: AltmanCodex Guided Question Trigger Card
index_id: trigger-altmancodex-guided-question
memory_layer: trigger
status: active
scope:
  - project
  - ops
source_system:
  - codex
  - claude-code
  - rubin-memory
memory_type:
  - workflow
  - failure
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/AltmanCodex/Global_Agent_Rules.md
  - /Users/robin/.claude/CLAUDE.md
  - /Users/robin/.codex/AGENTS.md
exclude_from_startup: true
---

# altmancodex-guided-question 触发卡

> AltmanCodex 执行前引导式提问门禁。目标：恢复“先问清方向再执行”，避免宽任务被 Agent 自行猜完。

```text
触发：用户说继续/帮我做/开新选题/做一版/生产一个，且缺平台、受众、长度、风格、成功标准、交付等级或工具路线。
先查：当前子项目 AGENTS.md + CURRENT.md；若是视频/图文/生图/社媒，再查对应项目最近 learnings/QC。
必须：只问 1 个最关键决策问题；若三选一无法从上下文判定，必须问 Robin；问题后等待回答再执行。
禁止：把“先问自己”当已问 Robin；用模型猜测补齐交付标准；宽任务直接进入生成/发布/批量动作。
下一步：提 1 个选择题或默认推荐 + 备选 → Robin 确认 → 再进入项目 workflow/preflight。
```
