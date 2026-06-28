---
title: Source Index
index_id: source-index
memory_layer: source_index
status: active
scope:
  - global
  - ops
source_system:
  - rubin-memory
memory_type:
  - source-index
sensitivity: medium
time_sensitivity: current
canonical: true
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/.claude/projects/*/memory/
  - /Users/robin/.codex/memories/
  - /Users/robin/.hermes/memories/
  - /Users/robin/.openclaw/
  - /Users/robin/.minimax/
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/
exclude_from_startup: false
---

# 10_source-index

> 各系统原始记忆源索引。用途是告诉 Agent “该去哪里查”，不是把原文搬进来。

## 读取优先级

1. 当前任务如涉及跨 Agent 共识：先读 `02_core-consensus.md` 和 `01_initial-memory.md`。
2. 如需查来源：读本文件定位原始系统。
3. 如需排除冲突：读 `20_conflict-index.md`。
4. 如需打开原文：只打开本任务相关的 1–3 个文件。

## Claude Code

```yaml
source_system: claude-code
root: /Users/robin/.claude/projects/*/memory/
format: per-project memory directory + MEMORY.md index + individual md facts
sensitivity: mixed; some project memories contain credentials references
import_policy: selective-summary-only
```

### 可读内容

- 稳定用户偏好。
- 项目规则。
- 工具路由。
- 历史任务沉淀。
- 用户反馈。

### 禁止导入

- credentials / key / token / account id 原文。
- 旧工作日志全文。
- 未确认旧项目状态。

### 重要路径

- `/Users/robin/.claude/projects/-Users-robin-Rubin-Studio--Brainstorm---Agent--/memory/`
- `/Users/robin/.claude/projects/-Users-robin-AltmanCodex/memory/`
- `/Users/robin/.claude/projects/-Users-robin-AgentProject/memory/`
- `/Users/robin/.claude/projects/-Users-robin--hermes/memory/`
- `/Users/robin/.claude/projects/-Users-robin/memory/`

## Codex

```yaml
source_system: codex
root: /Users/robin/.codex/memories/
format: central memory repo + summary + raw memories + rollout summaries
sensitivity: mixed; mostly task history and preferences, may include operational details
import_policy: workflow-and-failure-summary-only
```

### 可读内容

- `MEMORY.md`：综合偏好、用户画像、项目索引。
- `memory_summary.md`：按任务聚合的摘要。
- `raw_memories.md`：任务原始归并记录。
- `rollout_summaries/`：单次任务/rollout 总结。

### 禁止导入

- 过期 task state。
- cwd/thread/task id 原文。
- 一次性错误日志全文。
- 未确认自动推断。

### 重要路径

- `/Users/robin/.codex/memories/MEMORY.md`
- `/Users/robin/.codex/memories/memory_summary.md`
- `/Users/robin/.codex/memories/raw_memories.md`
- `/Users/robin/.codex/memories/rollout_summaries/`

## Hermes

```yaml
source_system: hermes
root: /Users/robin/.hermes/memories/
format: MEMORY.md + USER.md using section separators
sensitivity: medium; contains user preferences and system boundaries
import_policy: preference-and-boundary-summary-only
```

### 可读内容

- `MEMORY.md`：Nomos/OpenClaw 边界、Brainstorm 规则、模型路由、项目快照。
- `USER.md`：用户偏好、社媒口径、不要主动处理 OpenClaw 等。
- `SOUL.md`：Nomos 身份、判断框架、高风险动作规则。

### 禁止导入

- Nomos 专属身份细节作为所有 Agent 规则。
- 临时项目快照作为长期共识。
- 路径中潜在 key 值或账号认证信息。

### 重要路径

- `/Users/robin/.hermes/memories/MEMORY.md`
- `/Users/robin/.hermes/memories/USER.md`
- `/Users/robin/.hermes/SOUL.md`
- `/Users/robin/.hermes/hermes-agent/tools/memory_tool.py`（仅理解机制）
- `/Users/robin/.hermes/hermes-agent/agent/memory_provider.py`（仅理解机制）

## OpenClaw

```yaml
source_system: openclaw
root: /Users/robin/.openclaw/
format: workspace MEMORY.md + daily memory md + sessions json/jsonl + sqlite
sensitivity: mixed; sessions and sqlite may contain detailed operational traces
import_policy: curated-summary-only; no sqlite dump by default
```

### 可读内容

- workspace `MEMORY.md`：长期精选。
- workspace `AGENTS.md`：workspace 规则。
- `memory/YYYY-MM-DD.md`：短期日记/事件。
- `sessions/sessions.json`：session 索引。

### 禁止导入

- sqlite 全量内容。
- jsonl session 全文。
- Telegram/Feishu metadata 原文。
- HEARTBEAT / 模型切换噪音。
- 自动晋升中未审核内容。

### 重要路径

- `/Users/robin/.openclaw/workspace-eve/MEMORY.md`
- `/Users/robin/.openclaw/workspace-eve/AGENTS.md`
- `/Users/robin/.openclaw/workspace-eve/memory/`
- `/Users/robin/.openclaw/agents/eve/sessions/sessions.json`
- `/Users/robin/.openclaw/workspace*/MEMORY.md`
- `/Users/robin/.openclaw/workspace*/memory/*.md`

## Mavis / MiniMax

```yaml
source_system: mavis-minimax
root: /Users/robin/.minimax/
format: user memory + agent memory + builtin skills + config
sensitivity: high around config files
import_policy: tool-boundary-summary-only
```

### 可读内容

- Mavis 用户画像。
- Mavis agent 规则。
- MiniMax TTS/music/video/STT 经验。
- MiniMax Token Plan skill。
- api-search-media-router MiniMax 边界。

### 禁止导入

- `/Users/robin/.mmx/config.json` 中的 `api_key` 值。
- `/Users/robin/.minimax/config.yaml` 中的 API key 值。
- 任何账号认证值。

### 重要路径

- `/Users/robin/.minimax/memory/user.md`
- `/Users/robin/.minimax/agents/mavis/agent.md`
- `/Users/robin/.minimax/agents/general/memory/MEMORY.md`
- `/Users/robin/.claude/skills/minimax-token-plan/SKILL.md`
- `/Users/robin/.codex/skills/minimax-token-plan/SKILL.md`
- `/Users/robin/.claude/skills/api-search-media-router/SKILL.md`

## Brainstorm

```yaml
source_system: brainstorm
root: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/
format: MEMO/CURRENT/rules + numbered Markdown directories
sensitivity: low to medium; contains strategy and project decisions
import_policy: primary-consensus-source
```

### 可读内容

- `MEMO.md`：长期状态板。
- `CURRENT.md`：当前接力，时效性强。
- `Global_Agent_Rules.md`：Brainstorm 空间规则。
- `02_决定与共识/`：已拍板决定。
- `01_思路与方案/`：完整方案，可参考。

### 禁止导入

- `03_草稿与待定/` 中未确认草稿作为 active consensus。
- `_System/archive/` 作为当前规则。
- 高时效题材不经重查直接使用。

### 重要路径

- `/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md`
- `/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md`
- `/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md`
- `/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/`

## EverOS 未来索引定位

```yaml
source_system: everos-future
root: not-installed-in-this-phase
format: Markdown truth source + SQLite state + LanceDB BM25/vector/filter index
import_policy: index 06_rubin-memory summaries first; do not index raw private memory by default
```

未来 EverOS 只应索引 `06_rubin-memory` 的摘要层和候选层；不要直接全量索引 `.claude`、`.codex`、`.hermes`、`.openclaw`、`.minimax`。