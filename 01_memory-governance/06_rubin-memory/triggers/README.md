---
title: Trigger Cards Index
index_id: trigger-cards-index
memory_layer: trigger
status: active
scope:
  - global
  - ops
source_system:
  - rubin-memory
memory_type:
  - workflow
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/triggers/
exclude_from_startup: true
---

# triggers

> 高频/高风险场景短触发卡。用途：执行前先看 5 行门禁，避免长 skill 注意力稀释。

## 当前试点

| Trigger | 用途 | 命令 |
|---|---|---|
| `chatgpt-imagegen` | 高频生图前门禁 | `_workspace/scripts/rubin_memory.sh guard chatgpt-imagegen` |
| `chatgpt-portrait-template` | 人物/角色/锁脸模板门禁 | `_workspace/scripts/rubin_memory.sh guard chatgpt-portrait-template` |
| `altmancodex-workflow` | 进入 AltmanCodex / 子项目工作流 | `_workspace/scripts/rubin_memory.sh guard altmancodex-workflow` |
| `altmancodex-guided-question` | 宽任务执行前先问关键问题 | `_workspace/scripts/rubin_memory.sh guard altmancodex-guided-question` |
| `altmancodex-artifact-not-result` | 防止过程件冒充成品/生效 | `_workspace/scripts/rubin_memory.sh guard altmancodex-artifact-not-result` |
| `altmancodex-image-identity-mother` | 母图/角色/系列图身份门禁 | `_workspace/scripts/rubin_memory.sh guard altmancodex-image-identity-mother` |
| `api-search-media-route` | 搜索/事实核查/多媒体工具路由 | `_workspace/scripts/rubin_memory.sh guard api-search-media-route` |
| `api-search-key-rotation` | Tavily/API 搜索失败后的 key 轮换门禁 | `_workspace/scripts/rubin_memory.sh guard api-search-key-rotation` |

## 使用原则

1. 触发卡不是替代原 skill，只是执行前短门禁。
2. 触发卡只保留 5 行：触发、先查、必须、禁止、下一步。
3. 真执行仍回到对应 `SKILL.md`、项目 `AGENTS.md` 或 `CURRENT.md`。
4. 新增触发卡前先用真实任务验证，不把潜在高频当高频。
