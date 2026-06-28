---
title: Initial Memory
index_id: initial-memory
memory_layer: initial
status: active
scope:
  - global
  - project
  - tool
  - publishing
  - visual
  - security
  - ops
  - content
source_system:
  - rubin-memory
  - brainstorm
  - claude-code
  - codex
  - hermes
  - openclaw
  - mavis-minimax
memory_type:
  - consensus
  - preference
  - workflow
  - failure
  - tool
  - security
  - project
sensitivity: medium
time_sensitivity: current
canonical: true
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/00_full-memory-extract.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/21_2026-06-17-consensus-audit.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md
exclude_from_startup: true
---

# 01_initial-memory

> 第一版初始化记忆：从完整抽取中筛出的当前有效、跨 Agent 有用、可重复使用内容。它不是全部历史，也不是最终核心规则；适合未来 Agent 启动后优先索引或按需召回。

## 元数据口径

每条记忆默认字段：

```yaml
status: active
memory_type: consensus | preference | workflow | failure | tool | security | project
scope: global | project | tool | visual | publishing | security | ops | content
source_system:
source_path:
```

---

# A. 全局共识

## A1. Rubin Studio 当前总锚

```yaml
memory_type: consensus
scope: global/content
source_system: brainstorm
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
```

Robin 当前做的是 **AI 内容工作室**，不是 AI 频道套利。最高优先级是变现：赛道服务变现，选题赛道优先于纯努力。护城河来自生产系统、判断、审美、人格、自有受众和 IP，而不是单纯用 AI 批量搬运。

## A2. Brainstorm 是跨 Agent 共识空间

```yaml
memory_type: consensus
scope: global/project
source_system: brainstorm
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md
```

`/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/` 是思路沟通、方案讨论、决策沉淀空间，不是工程生产区。这里只产 md；代码、脚本、媒体、最终交付物回到 AltmanCodex / AgentProject / 对应项目工作区。

## A3. 共识和记忆必须分开

```yaml
memory_type: consensus
scope: global/security
source_system: rubin-memory
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/README.md
```

Consensus 是当前有效规则，必须人工确认；Memory 是历史事实、经验、候选和案例，只能作为依据。任何 Agent 不得把聊天内容或自动总结直接升级为核心共识。

## A4. 候选记忆必须走 inbox

```yaml
memory_type: workflow
scope: global/ops
source_system: everos-research + rubin-memory
source_path: /Users/robin/Rubin-Studio-Research/everos-memory-architecture/04_RECOMMENDED_MVP.md
```

Agent 发现新经验时，应写入 `memory-inbox/<source-system>/`，等待去重、冲突检查、敏感信息检查和 Robin/治理流程确认。候选不能自动改 `02_core-consensus.md`。

## A5. EverOS-style 多 Agent 记忆路线

```yaml
memory_type: workflow
scope: global/ops
source_system: claude-code + rubin-memory
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md
```

EverOS 相关接续任务当前从 `06_rubin-memory` 路线继续：Markdown truth source + 可重建本地索引 + scalar filters + FTS5/BM25 + lightweight vector rerank + candidate inbox。当前不安装 EverOS、不照搬旧 Claude Code plugin、不导入原始 Agent memory；若未来接 EverOS，只索引 `06_rubin-memory` 摘要层，并保持 SQLite/LanceDB/`.index` 为可重建缓存。

---

# B. 安全与敏感信息

## B1. 敏感值不进入普通记忆

```yaml
memory_type: security
scope: global/security
source_system: claude-code + codex + mavis-minimax
source_path: /Users/robin/.claude/CLAUDE.md; /Users/robin/.codex/AGENTS.md; /Users/robin/.minimax/
```

API Key、Token、Cookie、SSH 密码、账号认证值、完整账号 ID 不进入 `06_rubin-memory`。只允许记录：文件存在、字段名称、配置类型、脱敏风险说明。

## B2. 高风险动作必须确认

```yaml
memory_type: consensus
scope: global/security
source_system: hermes
source_path: /Users/robin/.hermes/SOUL.md
```

删除、覆盖、发布、付款、远程操作、修改密钥、系统级命令都必须先确认。确认只覆盖当次明确动作，不自动扩展到相邻高风险动作。

## B3. 不编造、不把过期信息当事实

```yaml
memory_type: preference
scope: global/security
source_system: hermes
source_path: /Users/robin/.hermes/SOUL.md
```

不知道就说不知道；涉及实时信息、市场、政策、工具版本、平台规则时必须查证或标注可能过期。不要用旧 memory 直接判断当前状态。

---

# C. 多 Agent 原始记忆边界

## C1. Claude Code memory

```yaml
memory_type: source-index
scope: global/ops
source_system: claude-code
source_path: /Users/robin/.claude/projects/*/memory/
```

Claude Code memory 是按项目路径编码的分桶记忆，适合抽取稳定偏好、项目规则、工具路由和用户反馈。不要全量导入；其中 AgentProject memory 含多条敏感凭证类历史条目，必须排除。

## C2. Codex memory

```yaml
memory_type: source-index
scope: global/ops
source_system: codex
source_path: /Users/robin/.codex/memories/
```

Codex memory 更像自动归并的任务史、偏好索引、失败复盘和生产工作流。适合抽取可复用 workflow 和失败模式；历史任务状态、cwd、thread、rollout 原文不进入初始化记忆。

## C3. Hermes memory

```yaml
memory_type: source-index
scope: global/ops
source_system: hermes
source_path: /Users/robin/.hermes/memories/
```

Hermes memory 分为 `MEMORY.md` 系统/项目长期记忆和 `USER.md` 用户偏好。它适合作为 Nomos 判断、发布、矩阵规则参考；Nomos 身份细节不应变成所有 Agent 的全局规则。

## C4. OpenClaw memory

```yaml
memory_type: source-index
scope: global/ops
source_system: openclaw
source_path: /Users/robin/.openclaw/
```

OpenClaw 有 workspace 长期记忆、daily notes、session jsonl、sqlite 和自动晋升痕迹。适合抽取长期在线/发布/浏览器操作经验；自动晋升内容含噪，必须人工审核后才能进入共识。

## C5. Mavis / MiniMax memory

```yaml
memory_type: source-index
scope: global/tool
source_system: mavis-minimax
source_path: /Users/robin/.minimax/
```

Mavis 是轻量副手，MiniMax 是工具能力。MiniMax 相关 memory 和 skills 适合抽取工具边界，但配置文件中存在敏感字段，严禁导入值。

---

# D. 工具与生产路由

## D1. MiniMax 使用边界

```yaml
memory_type: tool
scope: tool/media
source_system: mavis-minimax + brainstorm
source_path: /Users/robin/.claude/skills/minimax-token-plan/SKILL.md; /Users/robin/.claude/skills/api-search-media-router/SKILL.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
```

MiniMax 主动用于 TTS、音乐/BGM、视频理解、音频理解、STT。MiniMax 生图只有 Robin 明确说“用 MiniMax 生图”或点名 `MiniMax image-01` 时才用。MiniMax 搜索和识图非必要不用。

## D2. 搜索与生图优先级

```yaml
memory_type: tool
scope: tool/content
source_system: claude-global + codex-global + mavis-minimax
source_path: /Users/robin/.claude/CLAUDE.md; /Users/robin/.codex/AGENTS.md; /Users/robin/.claude/skills/api-search-media-router/SKILL.md
```

资讯搜索优先走 `api-search-media-route` / `api-search-media-router` / Tavily / multi-search 路线。未指定工具的生图优先 ChatGPT/Codex 生图；MiniMax 生图不是默认路线。

## D3. 图片交付与视觉审查

```yaml
memory_type: preference
scope: visual
source_system: claude-code + codex + brainstorm
source_path: /Users/robin/.claude/projects/-Users-robin-Rubin-Studio--Brainstorm---Agent--/memory/image-preview-direct-embed.md; /Users/robin/.codex/AGENTS.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md
```

图片交付不能只给路径，应优先让图片在对话中可预览。视觉审查要先用压缩预览，同图原则上只看一次；看完写 review/QC 结论，避免反复把高清图灌进上下文。

---

# E. 项目与内容共识

## E1. 社媒矩阵当前口径

```yaml
memory_type: consensus
scope: content/publishing
source_system: brainstorm
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md
```

2026-06-17 的上位社媒共识以 **AI 内容工作室 v2** 和 **小红书 AI 视觉变现旗舰** 为准。小红书是当前变现主阵地，Garfield2025 是 AI 视觉实操中长视频/深度图文旗舰；抖音是基本盘、涨粉和钩子验证阵地，ONCELUO2023 是 AI 判断基本盘。艺斌视觉线 + 泽川 AI 判断线两手抓，账号只有轻重，没有冷藏。

## E1.1 内容判断链不可外包

```yaml
memory_type: consensus
scope: content/security
source_system: brainstorm
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md
```

选题、观点、钩子、人设、审美终审、商业取舍和平台策略必须保留在人类判断链，由 Robin / Claude / DeepSeek 这类判断链确认。Agent 可以生产、检索、整理、执行和复用 workflow，但不要现场编观点，也不要把自动化流程当护城河。

## E2. Buffer 国际线定位

```yaml
memory_type: consensus
scope: publishing
source_system: brainstorm + hermes/openclaw
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_Buffer国际线发布规划与API账号映射.md
```

Buffer 是国际线轻量分发层，不是主阵地，不抢小红书、抖音、公众号主产能。发布前必须确认账号、正文、媒体 URL、发布时间、是否立即发布。发布执行以 Hermes / OpenClaw 为主，Codex 负责生产和知晓链路。

## E3. Brainstorm 文件落点

```yaml
memory_type: workflow
scope: project/ops
source_system: brainstorm + hermes
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md; /Users/robin/.hermes/memories/MEMORY.md
```

关键决定写 `02_决定与共识/`；完整方案写 `01_思路与方案/`；草稿写 `03_草稿与待定/`；参考写 `04_参考与引用/`。临时清单、快捷菜单、一次性说明默认不写 Brainstorm。

---

# F. 失败模式与治理

## F1. 自动记忆不能直接晋升

```yaml
memory_type: failure
scope: global/ops
source_system: openclaw + everos-research
source_path: /Users/robin/.openclaw/workspace-eve/MEMORY.md; /Users/robin/Rubin-Studio-Research/everos-memory-architecture/06_RISKS_AND_OPEN_QUESTIONS.md
```

OpenClaw 有自动晋升痕迹，但可能把 Telegram metadata、HEARTBEAT、模型切换等噪音提升进长期记忆。自动总结、自动晋升只能作为候选，必须人工审核。

## F2. 旧规则和归档默认不召回

```yaml
memory_type: failure
scope: global/ops
source_system: codex + claude-code + brainstorm
source_path: /Users/robin/.codex/memories/; /Users/robin/.claude/projects/*/memory/; /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/archive/
```

archive、deprecated、plugin cache、research_raw、过期文档可能含有旧规则。检索默认排除，除非任务是审计历史。

## F3. 上下文膨胀治理

```yaml
memory_type: workflow
scope: global/ops
source_system: everos-research + codex + brainstorm
source_path: /Users/robin/Rubin-Studio-Research/everos-memory-architecture/04_RECOMMENDED_MVP.md; /Users/robin/.codex/AGENTS.md
```

固定加载只放最短共识；历史经验通过 Top K 召回。视觉/图片类任务避免重复读高清图，文档类任务避免一次读完整记忆库。
