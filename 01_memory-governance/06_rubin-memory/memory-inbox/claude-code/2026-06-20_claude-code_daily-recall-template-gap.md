---
title: Daily recall template gap for candidate-write
index_id: 2026-06-20-claude-code-daily-recall-template-gap
memory_layer: candidate
status: candidate
scope:
  - ops
  - global
source_system:
  - claude-code
memory_type:
  - workflow
  - failure
sensitivity: low
time_sensitivity: current
confidence: high
review_policy: human-review
created_at: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/memory-inbox/README.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/02_core-consensus.md
---

# Daily recall template gap for candidate-write

## 候选记忆

日常流程试跑显示：`rubin_memory.sh template candidate-write` 带 `--scope ops`，而 `memory-inbox/README.md`（候选写入格式真相源）此前无 frontmatter、被 `infer_scope` 兜底成 `scope: global`，于是被 `--scope ops` 直接过滤掉，根本不进结果。这不是“排名不够靠前”，是 scope 过滤命中导致整条缺失。

## 复盘订正与修复（2026-06-20）

- 初版根因写成“没有召回到 Top K / 需要加权”，不准确；真实根因是 scope 过滤不匹配。
- 已修：给 `memory-inbox/README.md` 补 frontmatter（`scope: [ops, global]`、`memory_layer: retrieval_policy`、`status: active`），现在 `--scope ops` 能召回它。
- 另已修检索 path 去重，避免单文件多段占满 Top-K。

## 来源

- 2026-06-20 试跑 `source-locations`、`candidate-write`、`everos-maturity` 三个开工前 Top K 召回。
- `candidate-write` Top 结果主要是 `02_core-consensus.md` 第 3、4、5、6、10 条。
- 候选文件格式实际仍依赖 `memory-inbox/README.md`。

## 为什么可能有用

它暴露了日常工作流里的一个小缺口：执行规则能召回，但写入模板不够靠前。修好后，新 Agent 不容易写出 frontmatter 不完整的 candidate。

## 风险与限制

- 这只是一次试跑结果，不代表所有 query 都漏召回 inbox README。
- 不包含敏感值。
- 是否修改 wrapper template 需要另开实现任务；当前只记录候选经验。

## 建议处理

- 保持 candidate。
- 若后续再次复现，把 `candidate-write` template 改成直接输出或优先召回 `memory-inbox/README.md`。
- 不升级进 `02_core-consensus.md`。
