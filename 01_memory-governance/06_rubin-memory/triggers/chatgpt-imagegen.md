---
title: ChatGPT Imagegen Trigger Card
index_id: trigger-chatgpt-imagegen
memory_layer: trigger
status: active
scope:
  - visual
  - content
source_system:
  - claude-code
  - codex
  - rubin-memory
memory_type:
  - workflow
  - tool
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/.claude/skills/chatgpt-imagegen/SKILL.md
  - /Users/robin/AltmanCodex/AGENTS.md
exclude_from_startup: true
---

# chatgpt-imagegen 触发卡

> 高频生图任务的执行前短门禁。长规则仍以 `chatgpt-imagegen/SKILL.md` 为准，本卡只负责防漏。

```text
触发：用户要生成/改图/封面/插画/母图/视觉资产，且未明确指定 MiniMax。
先查：rubin_memory.sh template minimax-usage；再读 chatgpt-imagegen SKILL 的 When to use / Production guardrail / Save-path policy。
必须：第一张就是真实任务图；prompt 写 aspect ratio，不写像素；输出必须有工作区 ASCII 预览图。
禁止：无关 test/smoke/sample/demo 图；同 prompt 多刷；自动写 CANON；把路径当最终交付不预览。
下一步：确认项目落点和预览路径 → 写/压缩引用图 → 调 chatgpt-imagegen → 展示预览 + 记录路径。
```

## 失败模式

- 只给路径不展示图。
- 为测试工具先生成无关图。
- 把 `2048px / 4K / 8K` 写进 prompt，而不是 CLI `--size`。
- Robin 说“通过”后自动写 CANON。
- 重复生成多张同 prompt，浪费 quota。
