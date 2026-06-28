---
title: Retrieval Policy
index_id: retrieval-policy
memory_layer: retrieval_policy
status: active
scope:
  - global
  - ops
  - security
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
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/30_retrieval-policy.md
exclude_from_startup: false
---

# 30_retrieval-policy

> 检索策略：不要把全部记忆读进上下文。先用短共识和索引定位，再按任务读取 Top K 相关摘要，必要时打开单个原始文件。

## 目标

让 Claude Code、Codex、Hermes、OpenClaw、Mavis/MiniMax 或未来 EverOS/MCP bridge 都能用同一套读取逻辑：

```text
固定加载少量核心共识
→ 识别任务 scope
→ 过滤来源和状态
→ Top K 召回
→ 需要时再读原文
```

## 启动固定加载

默认只固定加载：

1. `02_core-consensus.md`
2. `10_source-index.md` 的相关系统段落
3. 如任务涉及记忆治理，再读 `20_conflict-index.md`
4. 如任务涉及索引实现或 EverOS 接入，再读 `40_index-schema.md`

不要默认加载：

- `00_full-memory-extract.md`
- `01_initial-memory.md` 全文
- 原始 `.claude/.codex/.hermes/.openclaw/.minimax` memory 全文
- archive / daily logs / session jsonl

## 任务识别字段

每个检索请求先判断：

```yaml
scope: global | project | tool | visual | publishing | security | ops | content
source_system: brainstorm | claude-code | codex | hermes | openclaw | mavis-minimax | any
memory_type: consensus | preference | decision | workflow | failure | tool | project | session
status: active | candidate | deprecated | archived | sensitive-excluded
project:
time_sensitivity: stable | current | historical
```

## 默认过滤规则

默认 include：

```yaml
status:
  - active
  - candidate  # 只在用户要求探索/对比时包括
memory_type:
  - consensus
  - preference
  - decision
  - workflow
  - failure
  - tool
```

默认 exclude：

```yaml
status:
  - deprecated
  - archived
  - sensitive-excluded
paths:
  - archive/**
  - _System/archive/**
  - .tmp/**
  - plugin cache
  - research_raw/**
  - sessions/*.jsonl
  - *.sqlite
```

## Top K

默认：

- 普通任务：Top K = 3。
- 调研/架构任务：Top K = 5。
- 审计任务：Top K = 10，但只返回摘要和路径。

超过预算时：

1. 只保留 active。
2. Top K 降到 3。
3. 返回一行摘要 + path。
4. 不打开原始大文件。

## 什么时候读全文

只有满足以下条件才打开原文：

- 需要确认敏感边界，但不输出值。
- 摘要冲突，需要判断哪个更新。
- 用户要求查看完整来源。
- 当前任务必须执行具体工作流。
- 检索结果不足以判断。

读全文前优先检查：

- 是否是 archive / deprecated。
- 是否可能包含 key/token/cookie。
- 是否是 session jsonl 或 sqlite。

## 查询模板

### 查询 MiniMax 应该怎么用

```yaml
scope: tool
source_system:
  - mavis-minimax
  - brainstorm
  - claude-code
memory_type:
  - tool
  - consensus
status: active
TopK: 3
```

优先读：

- `01_initial-memory.md` D1/D2。
- `20_conflict-index.md` MiniMax 路由重复。
- 原始 skill 仅在需要命令细节时读取。

### 查询 Brainstorm 能不能写代码

```yaml
scope: project
source_system: brainstorm
memory_type: consensus
status: active
TopK: 3
```

优先读：

- `02_core-consensus.md` 第 2 条。
- `01_initial-memory.md` A2 / E3。
- Brainstorm `Global_Agent_Rules.md` 原文按需。

### 查询各系统记忆源在哪里

```yaml
scope: ops
source_system: any
memory_type: source-index
status: active
TopK: 5
```

优先读：

- `10_source-index.md`。
- `00_full-memory-extract.md` 各系统总览。

### 查询新经验写哪里

```yaml
scope: ops
memory_type: workflow
status: active
TopK: 3
```

优先读：

- `02_core-consensus.md` 第 4 条。
- `01_initial-memory.md` A3/A4。
- `README.md` 写入规则。

### 查询哪些不能导入

```yaml
scope: security
memory_type:
  - security
  - failure
status: active
TopK: 5
```

优先读：

- `20_conflict-index.md`。
- `00_full-memory-extract.md` 不建议导入部分。
- `01_initial-memory.md` B 区。

### 查询社媒矩阵 / 平台打法

```yaml
scope: content
source_system: brainstorm
memory_type:
  - consensus
  - decision
  - workflow
status: active
TopK: 5
```

优先读：

1. `02_core-consensus.md` 第 1 条。
2. `01_initial-memory.md` E1 / E1.1 / E2。
3. `21_2026-06-17-consensus-audit.md` 总体裁定。
4. 具体账号任务再读 `2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md` 对应段。
5. `2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md` 只读 workflow / 方法论段，不读 B站主次结论。

冲突处理：当前以 AI 内容工作室 v2 + 小红书 AI 视觉变现旗舰为上位共识；B站不是当前旗舰。平台机制、扶持政策、账号节奏和 Buffer channel/API 细节执行前需复核日期。

### Codex 生产型任务

```yaml
scope:
  - content
  - visual
  - publishing
source_system:
  - brainstorm
  - codex
  - claude-code
memory_type:
  - consensus
  - workflow
  - tool
status: active
TopK: 5
```

优先读：

1. `02_core-consensus.md` 第 1、2、9 条。
2. `01_initial-memory.md` D1/D2、E1/E1.1/E2。
3. 若涉及社媒矩阵，读 `21_2026-06-17-consensus-audit.md`。
4. 若涉及发布，读 Buffer 相关决定文件或 E2。

执行边界：Brainstorm 只写思路/共识 Markdown；代码、脚本、媒体、成片、素材回到 AltmanCodex / AgentProject / 对应项目工作区。生产内容中的观点、钩子、商业取舍不得由 Agent 现场编造，需 Robin / 判断链确认。

### Buffer 发布任务

```yaml
scope: publishing
source_system:
  - brainstorm
  - hermes
  - openclaw
memory_type:
  - consensus
  - workflow
  - security
status: active
TopK: 5
```

优先读：

1. `01_initial-memory.md` E2。
2. `20_conflict-index.md` 敏感信息风险。
3. `21_2026-06-17-consensus-audit.md` Buffer 段。
4. 必要时读 `2026-06-17_Buffer社媒发布接入说明_全Agent与双电脑.md`。

发布前必须确认账号、正文、媒体 URL、发布时间、是否立即发布。API key 不进文档、不进 Git、不进聊天；评论/互动不靠 Buffer。

### OpenClaw 长期观察 / 自动记忆任务

```yaml
scope:
  - ops
  - publishing
source_system: openclaw
memory_type:
  - workflow
  - failure
  - source-index
status: active
TopK: 5
```

优先读：

1. `02_core-consensus.md` 第 3、4、8 条。
2. `10_source-index.md` OpenClaw 段。
3. `20_conflict-index.md` 第 6 条。
4. `memory-inbox/README.md`。

OpenClaw 观察经验只能先写 `memory-inbox/openclaw/`，标记 `status: candidate` 和 `review_policy: human-review`。不导入 sqlite/jsonl 全文、Telegram/Feishu metadata、HEARTBEAT、模型切换和临时状态噪音。

## 未来 EverOS 映射

如果后续接 EverOS，建议把 Markdown frontmatter 映射为 scalar filters：

```yaml
app_id: rubin-studio
project_id: rubin-memory
source_system: claude-code | codex | hermes | openclaw | mavis-minimax | brainstorm
memory_type: consensus | preference | decision | workflow | failure | tool | project | source-index
status: active | candidate | deprecated | archived | sensitive-excluded
scope: global | project | tool | visual | publishing | security | ops | content
```

检索顺序：

1. scalar filter：先过滤 status/scope/source_system。
2. BM25：命中路径、工具名、项目名、关键词。
3. vector：补充语义相近记忆。
4. rerank：按当前任务相关性排序。
5. 返回 Top K 摘要 + source_path。

## 禁止策略

- 不把 `00_full-memory-extract.md` 作为默认启动文件。
- 不把 `memory-inbox` candidate 当 active。
- 不从 archive 召回当前规则。
- 不让 Agent 直接写 `02_core-consensus.md`。
- 不把原始 Agent memory 全量导入 EverOS。
- 不同步 EverOS `.index`、SQLite、LanceDB。

## 人工使用建议

Robin 检查遗漏时读：

1. `00_full-memory-extract.md`
2. `01_initial-memory.md`

Agent 日常启动优先读：

1. `02_core-consensus.md`
2. 与任务相关的 `10_source-index.md` 段落
3. 必要时读 `20_conflict-index.md`

未来索引层优先索引：

1. `01_initial-memory.md`
2. `02_core-consensus.md`
3. `10_source-index.md`
4. `20_conflict-index.md`
5. `memory-inbox/*` candidate 摘要