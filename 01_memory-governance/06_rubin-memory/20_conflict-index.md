---
title: Conflict Index
index_id: conflict-index
memory_layer: conflict_index
status: active
scope:
  - global
  - security
  - ops
  - content
  - publishing
source_system:
  - rubin-memory
  - brainstorm
  - claude-code
  - codex
  - hermes
  - openclaw
  - mavis-minimax
memory_type:
  - conflict-index
  - security
  - failure
sensitivity: medium
time_sensitivity: current
canonical: true
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/00_full-memory-extract.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/21_2026-06-17-consensus-audit.md
exclude_from_startup: false
---

# 20_conflict-index

> 各系统记忆之间的重复、冲突、过期和高敏风险索引。检索时先用本文件排除旧规则和污染源。

## 冲突状态

```yaml
conflict_type: duplicate | boundary | stale | sensitive | noisy | source-priority
severity: low | medium | high | critical
resolution: use | ignore | candidate-only | verify-first | human-review
```

---

# 1. 敏感信息风险

```yaml
conflict_type: sensitive
severity: critical
resolution: ignore values; record existence only
sources:
  - /Users/robin/.claude/projects/-Users-robin-AgentProject/memory/
  - /Users/robin/.mmx/config.json
  - /Users/robin/.minimax/config.yaml
  - /Users/robin/.claude.json
  - /Users/robin/.claude/settings.json
  - /Users/robin/.codex/config.toml
```

问题：多个系统的 memory/config 中存在 credentials、API key、token、account id、SSH 等敏感字段或历史条目。

裁定：

- 不复制完整值。
- 不进入 `00_full-memory-extract.md` 细节。
- 不进入 `01_initial-memory.md`。
- 只记录“存在敏感配置/字段名/路径/禁止导入”。

---

# 2. Brainstorm 规则重复

```yaml
conflict_type: duplicate
severity: medium
resolution: Brainstorm Global_Agent_Rules + MEMO as primary; others as references
sources:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CLAUDE.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/AGENTS.md
  - /Users/robin/.hermes/memories/MEMORY.md
  - /Users/robin/.codex/AGENTS.md
```

重复内容：Brainstorm 是思路/方案/决策沉淀区，只产 md，不做工程生产。

裁定：以 Brainstorm 本地 `Global_Agent_Rules.md` 和 `MEMO.md` 为主；Hermes/Codex 中的 Brainstorm 描述作为引用，若冲突则以 Brainstorm 本地规则为准。

---

# 3. Claude / Codex / Hermes / OpenClaw 分工重复

```yaml
conflict_type: duplicate
severity: low
resolution: keep only as contextual consensus, not memory structure
sources:
  - /Users/robin/.claude/CLAUDE.md
  - /Users/robin/.codex/AGENTS.md
  - /Users/robin/.hermes/SOUL.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
```

问题：多处都描述 Codex 主生产、Claude 架构、Hermes 发布/自动化、OpenClaw 长期在线等分工。

Robin 已纠正：`06_rubin-memory` 不应按这些分工建目录核心，因为这是执行分工，不是记忆功能。

裁定：分工只作为背景共识；目录按记忆功能和来源索引组织。

---

# 4. MiniMax 路由重复

```yaml
conflict_type: duplicate
severity: medium
resolution: current routing consensus in global rules + skills wins
sources:
  - /Users/robin/.claude/CLAUDE.md
  - /Users/robin/.codex/AGENTS.md
  - /Users/robin/.claude/skills/minimax-token-plan/SKILL.md
  - /Users/robin/.claude/skills/api-search-media-router/SKILL.md
  - /Users/robin/.minimax/memory/user.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
```

重复内容：MiniMax 是工具能力；生产调用走 Codex/mmx；Claude 端路由多为健康检查/边界；未点名 MiniMax 生图时优先 ChatGPT/Codex 生图；MiniMax 搜索/识图非必要不用。

裁定：以最新全局规则和 skill 文档为准。Mavis memory 可作参考，但不覆盖 Brainstorm/Codex/Claude 的当前路由共识。

---

# 5. Hermes “不要主动处理 OpenClaw” vs 跨系统记忆索引

```yaml
conflict_type: boundary
severity: medium
resolution: reading index is allowed for memory governance; operational handling still requires explicit user request
sources:
  - /Users/robin/.hermes/memories/USER.md
  - /Users/robin/.hermes/memories/MEMORY.md
  - /Users/robin/.openclaw/workspace-eve/AGENTS.md
```

问题：Hermes USER 里有“除非用户明确表示，否则不要检查或处理 OpenClaw”；但跨系统记忆架构需要索引 OpenClaw 记忆来源。

裁定：

- 为本次“记忆治理”只读索引 OpenClaw 是允许的，因为用户明确要求。
- 未来普通 Hermes/Nomos 任务不应主动处理 OpenClaw 运维，除非用户点名。
- `06_rubin-memory` 记录 OpenClaw 来源，不等于授权任意 Agent 修改 OpenClaw。

---

# 6. OpenClaw 自动晋升含噪

```yaml
conflict_type: noisy
severity: high
resolution: candidate-only; never direct core import
sources:
  - /Users/robin/.openclaw/workspace-eve/MEMORY.md
  - /Users/robin/.openclaw/workspace*/memory/*.md
  - /Users/robin/.openclaw/agents/*/sessions/
```

问题：OpenClaw 存在短期记忆晋升到长期记忆的机制，可能把 Telegram metadata、HEARTBEAT、模型切换、临时状态写入长期 memory。

裁定：OpenClaw 自动晋升是机制参考，不是内容权威。进入 `01_initial-memory.md` 前必须人工筛选。

---

# 7. Codex 自动 memory 的历史状态风险

```yaml
conflict_type: stale
severity: medium
resolution: workflow/failure may import; task state must verify current files
sources:
  - /Users/robin/.codex/memories/raw_memories.md
  - /Users/robin/.codex/memories/rollout_summaries/
```

问题：Codex memory 记录大量 task/cwd/thread/outcome，很多是当时状态，不代表当前状态。

裁定：

- 可抽取可复用工作流、失败模式、偏好。
- 不把历史 task outcome 当当前项目状态。
- 涉及路径/项目状态时必须读当前项目 `CURRENT.md` 或实际文件。

---

# 8. Claude AgentProject memory 的高敏历史混杂

```yaml
conflict_type: sensitive
severity: critical
resolution: no full import; only selective redacted lessons
sources:
  - /Users/robin/.claude/projects/-Users-robin-AgentProject/memory/MEMORY.md
```

问题：AgentProject memory 既有高价值工作流，也有凭证和账号类条目。

裁定：

- 不全量导入。
- 凭证类只记录“存在敏感历史，不导入”。
- 可抽取 NotebookLM、Skill 扫描、视觉/内容工作流等脱敏经验。

---

# 9. CURRENT.md 时效性

```yaml
conflict_type: stale
severity: medium
resolution: current-session only; stable items must move to MEMO/decisions
sources:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md
```

问题：CURRENT 是接力纸条，高时效，不适合直接作为长期初始化记忆。

裁定：

- 只抽取仍有效的工作方式和当前主线。
- 稳定共识以 `MEMO.md` 和 `02_决定与共识/` 为准。
- 高时效题材必须当天重查。

---

# 10. EverOS 插件成熟度

```yaml
conflict_type: stale/source-priority
severity: medium
resolution: use EverOS architecture ideas; do not use old cloud plugin as canonical local integration
sources:
  - https://github.com/EverMind-AI/EverOS
  - /Users/robin/Rubin-Studio-Research/everos-memory-architecture/00_EVEROS_RESEARCH.md
```

问题：EverOS 的 Claude Code plugin slice 仍使用旧 cloud `/api/v1/memories/*` routes；Knowledge Wiki / Reflection 仍是 coming next。

裁定：

- 保留 EverOS 精髓：Markdown truth source、派生索引可重建、filter 检索、user/agent memory 分轨。
- 不直接照搬旧 Claude Code plugin。
- 本阶段不安装 EverOS，只设计未来索引策略。

# 11. 2026-06-17 B站旗舰旧结论被覆盖

```yaml
conflict_type: stale/source-priority
severity: high
resolution: use AI 内容工作室 v2 + 小红书 AI 视觉旗舰 as current primary consensus
sources:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/21_2026-06-17-consensus-audit.md
```

问题：`2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md` 中的“B站 = 旗舰主场（确认）”与后续 v2 共识冲突。

裁定：当前以“小红书 AI 视觉变现旗舰 + 抖音判断基本盘”为上位共识。B站保留为轻维护观察 / 遇好思路好素材再加码 / 不作废；`指南方针` 只作为 workflow、方法论和历史参考，不作为平台主次结论来源。

---

# 12. 平台方法论时效性

```yaml
conflict_type: stale
severity: medium
resolution: verify platform mechanics before execution
sources:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/21_2026-06-17-consensus-audit.md
```

问题：小红书、抖音、公众号、快手、B站、Threads/X/IG、Buffer channel/API 细节都含平台动态，不能永久当事实。

裁定：平台方法论可作为 active-reference，但执行具体发布、账号策略、平台机制判断前需按日期复核。

## 检索时的冲突处理顺序

1. `status=active` 优先。
2. `Brainstorm MEMO/02_决定` 高于 Agent 自动 memory。
3. 人工确认高于自动总结。
4. 当前项目 `CURRENT/MEMO` 高于历史 rollout。
5. 敏感/archived/deprecated/noisy 默认排除。
6. candidate 只能作为参考，不能作为执行规则。