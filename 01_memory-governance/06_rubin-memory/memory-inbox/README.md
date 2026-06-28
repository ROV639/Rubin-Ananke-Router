---
title: Memory Inbox Write Guide
index_id: memory-inbox-readme
memory_layer: retrieval_policy
status: active
scope:
  - ops
  - global
source_system:
  - rubin-memory
memory_type:
  - workflow
sensitivity: low
time_sensitivity: stable
canonical: true
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/memory-inbox/
exclude_from_startup: true
---

# memory-inbox

> 候选记忆入口。任何 Agent 新发现经验、失败模式、工具边界或项目事实时，先写这里，不直接改 core consensus。

## 写入位置

按来源系统写入：

```text
memory-inbox/claude-code/
memory-inbox/codex/
memory-inbox/hermes/
memory-inbox/openclaw/
memory-inbox/mavis-minimax/
```

## 文件命名

```text
YYYY-MM-DD_<source-system>_<short-topic>.md
```

示例：

```text
2026-06-19_openclaw_buffer-feedback-observation.md
2026-06-19_codex_visual-script-failure.md
```

## 必需 frontmatter

```yaml
---
title: <候选记忆标题>
index_id: <YYYY-MM-DD-source-topic>
memory_layer: candidate
status: candidate
scope:
  - global | project | tool | visual | publishing | security | ops | content
source_system:
  - claude-code | codex | hermes | openclaw | mavis-minimax | brainstorm
memory_type:
  - consensus | preference | decision | workflow | failure | project | tool | security | source-index
sensitivity: low | medium | high | critical
time_sensitivity: stable | current | historical | volatile
confidence: high | medium | low
review_policy: human-review
created_at: YYYY-MM-DD
source_paths:
  - <原始来源路径或会话说明>
---
```

## 正文结构

```markdown
# <标题>

## 候选记忆

一句话写清楚可复用结论。

## 来源

- 路径或会话来源。
- 如果来自敏感文件，只写路径和字段类型，不写值。

## 为什么可能有用

说明它能减少哪类重复提醒、失败或误判。

## 风险与限制

说明是否过期、是否含平台动态、是否需要人工确认。

## 建议处理

- 保持 candidate。
- 合并进 `01_initial-memory.md`。
- 升级进 `02_core-consensus.md`。
- 标记 deprecated / sensitive-excluded。
```

## 审核规则

候选升级前必须检查：

1. 是否已有重复记忆。
2. 是否和 `20_conflict-index.md` 冲突。
3. 是否包含敏感值。
4. 是否只是一次性任务状态。
5. 是否需要 Robin 确认。

只有 Robin 明确确认或治理流程完成后，候选才能进入 `01_initial-memory.md` 或 `02_core-consensus.md`。
