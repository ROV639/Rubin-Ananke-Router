---
title: Index Schema
index_id: index-schema
memory_layer: schema
status: active
scope:
  - global
  - ops
source_system:
  - rubin-memory
memory_type:
  - workflow
  - source-index
sensitivity: low
time_sensitivity: stable
canonical: true
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/40_index-schema.md
exclude_from_startup: true
---

# 40_index-schema

> `06_rubin-memory` 的结构化索引字段标准。用途：让 Claude Code、Codex、Hermes、OpenClaw、Mavis/MiniMax 或未来 EverOS 能按字段过滤，而不是全文乱搜。

## 设计原则

- Markdown 是真相源；索引字段服务检索，不替代正文判断。
- 每个可检索条目必须能回答：它来自哪里、是否当前有效、适合什么任务、是否可直接执行。
- 不索引敏感值；只允许索引敏感字段存在与禁止导入裁定。
- `candidate` 永远不能被当作 `active` 执行规则。

## 文件级 frontmatter

新增或重写核心文件时，建议使用：

```yaml
---
title: <human-readable title>
index_id: <stable-kebab-id>
memory_layer: full_extract | initial | core | source_index | conflict_index | retrieval_policy | schema | verification | source_card | trigger | preflight | candidate
status: active | candidate | deprecated | archived | sensitive-excluded
scope:
  - global | project | tool | visual | publishing | security | ops | content
source_system:
  - brainstorm | claude-code | codex | hermes | openclaw | mavis-minimax | rubin-memory | everos-research
memory_type:
  - consensus | preference | decision | workflow | failure | project | tool | security | session | source-index | conflict-index | verification
sensitivity: low | medium | high | critical
time_sensitivity: stable | current | historical | volatile
canonical: true | false
last_reviewed: YYYY-MM-DD
source_paths:
  - <absolute or project-relative source path>
exclude_from_startup: true | false
---
```

## 条目级字段

当单个文件内包含多条记忆时，每条用正文里的 YAML block 标注：

```yaml
memory_id: <stable-kebab-id>
status: active | candidate | deprecated | archived | sensitive-excluded
memory_type: consensus | preference | decision | workflow | failure | project | tool | security | source-index | conflict-index
scope: global | project | tool | visual | publishing | security | ops | content
source_system: brainstorm | claude-code | codex | hermes | openclaw | mavis-minimax | rubin-memory | everos-research
source_path: <path>
confidence: high | medium | low
review_policy: fixed-load | topk | verify-first | human-review | never-load
```

## 字段语义

### `memory_layer`

- `core`：可固定加载的最短共识。
- `initial`：初始化精选，适合索引优先级高，但不默认全文加载。
- `full_extract`：查漏用完整抽取，不默认加载。
- `source_index`：定位原始系统。
- `conflict_index`：排除冲突、旧规则和污染源。
- `retrieval_policy`：决定读取顺序和 Top K。
- `verification`：验证记录和自优化日志。
- `source_card`：单个来源系统说明。
- `trigger`：高频/高风险场景短门禁，执行前只读 5 行。
- `preflight`：项目级执行前检查样板，用于把重复失败转成开工门禁。
- `candidate`：候选记忆，必须审核后才能升级。

### `status`

- `active`：当前有效，可作为检索结果。
- `candidate`：可参考，不可直接执行。
- `deprecated`：已废弃，只在审计历史时读。
- `archived`：归档，不作为当前规则。
- `sensitive-excluded`：只记录存在，禁止导入值。

### `time_sensitivity`

- `stable`：长期稳定规则，如敏感信息边界。
- `current`：当前阶段有效，如社媒矩阵主锚。
- `historical`：历史参考，不作为当前规则。
- `volatile`：平台机制、API 字段、账号状态、政策窗口，执行前必须复核。

### `review_policy`

- `fixed-load`：可启动固定加载，只适用于 `02_core-consensus.md` 这类短文件。
- `topk`：只按任务召回 Top K。
- `verify-first`：召回后必须复核日期、文件或外部事实。
- `human-review`：需 Robin 或治理流程确认。
- `never-load`：默认不读，只记录禁止导入。

## 推荐检索顺序

1. 固定读 `02_core-consensus.md`。
2. 根据任务字段过滤 `scope/status/source_system/memory_type/time_sensitivity`。
3. 从 `10_source-index.md` 定位来源。
4. 从 `20_conflict-index.md` 排除冲突和旧结论。
5. 读取 `01_initial-memory.md` 的相关段落。
6. 只有摘要不足时打开原始源。

## EverOS 映射

未来接 EverOS 时：

- Markdown frontmatter → scalar filters。
- 标题、正文、YAML block → BM25。
- 正文摘要 → vector embedding。
- `source_paths` 保留审计链。
- `.index`、SQLite、LanceDB 均视为可重建缓存，不进入 Brainstorm 同步区。

## 禁止

- 不把 `00_full-memory-extract.md` 加入启动固定加载。
- 不把 `memory-inbox/**` 自动设为 `active`。
- 不索引 API Key / Token / Cookie / 完整账号 ID / SSH 密码值。
- 不从 archive / deprecated / jsonl / sqlite 召回当前规则。
