---
title: 06_rubin-memory
index_id: rubin-memory-readme
memory_layer: source_index
status: active
scope:
  - global
  - ops
source_system:
  - rubin-memory
memory_type:
  - source-index
  - workflow
sensitivity: low
time_sensitivity: stable
canonical: true
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/
exclude_from_startup: false
---

# 06_rubin-memory

> Rubin Studio 的多 Agent 共识记忆层：Markdown 是真相源，未来索引层只负责检索，不负责自动改共识。

## 定位

这里不是新的 Agent 配置目录，也不是替代 Claude Code / Codex / Hermes / OpenClaw / Mavis 的原始记忆库。

这里保存三类东西：

1. **完整抽取**：各系统已有记忆源和关键内容的脱敏清单。
2. **初始化精选**：适合作为跨 Agent 启动记忆的有效共识。
3. **索引策略**：未来让 Agent 或 EverOS 只读取当前任务需要的记忆。

## 核心原则

- Markdown 是人类可读、可编辑、可审计的真相源。
- 原始记忆不搬家，只在这里建立来源索引和精选摘要。
- Key / Token / Cookie / 账号认证值不进入本目录。
- `memory-inbox/` 是候选区，不能自动升级为 core consensus。
- `archive/` 默认不检索、不作为当前规则。
- 未来 EverOS 只作为索引层；SQLite / LanceDB / `.index` 不放进 Brainstorm 同步区。

## 关键文件

- `00_full-memory-extract.md`：完整记忆抽取，供 Robin 查漏。
- `01_initial-memory.md`：精选初始化记忆，供后续 Agent 优先加载/索引。
- `02_core-consensus.md`：最短核心共识，适合启动固定加载。
- `10_source-index.md`：各系统原始记忆在哪里、怎么读、哪些不能导入。
- `20_conflict-index.md`：重复、冲突、过期、高敏风险。
- `30_retrieval-policy.md`：按需召回策略。
- `40_index-schema.md`：frontmatter / 条目级字段标准，供未来 EverOS 或本地检索过滤。
- `triggers/`：高频/高风险场景短触发卡，避免长 skill 注意力稀释。
- `preflight/`：项目级最小执行前门禁样板，先覆盖 Eve_Frames、Rov_Eve、Paper Moon。
- `_verification/2026-06-19_retrieval-smoke-test.md`：第一轮模拟检索验证。
- `_verification/2026-06-19_local-index-validation.md`：本地 SQLite FTS5/filter/vector-rerank 索引验证。
- `_verification/2026-06-19_flow-review.md`：进入下一步前的全流程复查与 bug 修复记录。
- `_verification/2026-06-19_live-query-test.md`：`rubin_memory.sh` 稳定入口实操查询测试。
- `_verification/2026-06-19_task-replay-test.md`：用本轮历史任务回放测试自然语言、filter、template 三种检索方式。
- `_verification/2026-06-19_trigger-card-test.md`：ChatGPT 生图、AltmanCodex 工作流、API 搜索聚合引擎三张高频触发卡测试。

## 写入规则

- 修改 `02_core-consensus.md` 需要 Robin 明确确认。
- Agent 新发现经验写入 `memory-inbox/<system>/`。
- 候选记忆必须带来源、状态、置信度，并遵守 `memory-inbox/README.md`。
- 任何自动抽取内容必须先进入候选，不得直接写 core。
