---
title: EverOS agent memory architecture handoff
index_id: 2026-06-20-claude-code-everos-agent-memory-architecture-handoff
memory_layer: candidate
status: candidate
scope:
  - ops
  - global
source_system:
  - claude-code
memory_type:
  - workflow
  - source-index
sensitivity: low
time_sensitivity: current
confidence: high
review_policy: human-review
created_at: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/20_conflict-index.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/30_retrieval-policy.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/_verification/2026-06-19_local-index-validation.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/_verification/2026-06-19_live-query-test.md
---

# EverOS agent memory architecture handoff

## 候选记忆

`EverOS agent memory architecture` 这条接续任务当前应从 `06_rubin-memory` 继续，而不是去安装 EverOS 或导入原始 Agent memory；已验证的方向是 Markdown truth source + scalar filters + FTS5 + lightweight vector rerank + candidate inbox。

## 来源

- `20_conflict-index.md` 已裁定：学习 EverOS 架构精髓，不照搬旧 Claude Code plugin。
- `30_retrieval-policy.md` 已定义 EverOS 映射和禁止策略。
- `2026-06-19_local-index-validation.md` 已验证本地 disposable SQLite 索引可运行。
- `2026-06-19_live-query-test.md` 已验证 `rubin_memory.sh template everos-maturity` 能命中 EverOS 成熟度裁定。

## 为什么可能有用

它能让挂掉的进程或新 Agent 快速接上：先查 `rubin_memory.sh template everos-maturity`、读 `30_retrieval-policy.md` 的未来 EverOS 映射，再继续文档化或候选闭环，而不是重做调研或误接旧插件。

## 风险与限制

- 这是候选记忆，不是 core consensus。
- EverOS 上游状态可能变化，真正接入前必须重新调研。
- 当前本地 vector rerank 只是 MVP，不等于真正 embedding。
- 不包含任何 API key、token、cookie 或账号认证值。

## 建议处理

- 保持 candidate，观察 1–2 天真实检索是否继续稳定。
- 若 Robin 确认这就是长期路线，可合并摘要进 `01_initial-memory.md`。
- 不建议升级进 `02_core-consensus.md`，除非 Robin 明确要求把 EverOS-style 本地索引设为长期硬规则。
