# EverOS Agent Memory Architecture Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把已验证的 `06_rubin-memory` 本地索引 MVP 沉淀成一份 EverOS-style 多 Agent 记忆架构方案，并补齐候选记忆闭环验证。

**Architecture:** Markdown 仍是真相源，SQLite/FTS/vector 仍是可重建缓存，EverOS 只作为未来索引层参考而不是当前依赖。本计划只在 Brainstorm 空间写文档和验证记录，不安装 EverOS、不导入原始 `.claude/.codex/.hermes/.openclaw/.minimax` 全量记忆、不修改 `02_core-consensus.md`。

**Tech Stack:** Markdown frontmatter, Bash wrapper `_workspace/scripts/rubin_memory.sh`, Python SQLite FTS5 indexer `_workspace/scripts/rubin_memory_index.py`, `06_rubin-memory` governance docs.

---

## File Structure

**Create:**
- `01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md` — 人读的总架构方案，说明目标、边界、数据流、组件、分阶段路线。
- `06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md` — 本轮“进程挂掉后接续”的候选记忆，进入 inbox，不直接改 core。
- `06_rubin-memory/_verification/2026-06-20_everos-agent-memory-architecture-continuation.md` — 接续验证记录，证明当前 wrapper/template/index 还能支撑 EverOS-style 查询。

**Modify:**
- `CURRENT.md` — 增加一行 2026-06-20 接力记录，让下个 Agent 知道已接上。

**Do not modify:**
- `06_rubin-memory/02_core-consensus.md` — 核心共识必须 Robin 明确确认后才能改。
- `06_rubin-memory/20_conflict-index.md` — 当前 EverOS 裁定已存在，除非发现新事实才改。
- `_workspace/scripts/rubin_memory_index.py` — 本计划先文档化和验证，不改检索实现。

---

### Task 1: Write Architecture Proposal

**Files:**
- Create: `01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md`

- [ ] **Step 1: Create the proposal document**

Write this exact document:

```markdown
# 2026-06-20_方案_EverOS-style多Agent记忆架构_v1

## 一句话结论

当前不要安装或照搬 EverOS Claude Code plugin；先把 `06_rubin-memory` 作为 Markdown truth source，用本地可重建索引模拟 EverOS 的 filter + BM25 + vector 检索层。

## 背景

Robin 要的是跨 Claude Code、Codex、Hermes、OpenClaw、Mavis/MiniMax 的共享记忆架构，不是把所有 Agent 原始 memory 合并成一个大库。`06_rubin-memory` 已经完成第一轮抽取、冲突裁定、schema、retrieval policy、本地索引和多视角验证。

现有 EverOS 值得学习的是：

- Markdown truth source。
- 派生索引可重建。
- scalar filter 优先于语义乱搜。
- user memory / agent memory 分轨。

当前不直接采用的是：

- 旧 Claude Code plugin 的 cloud `/api/v1/memories/*` routes。
- 尚未落地的 Knowledge Wiki / Reflection 设想。
- SQLite / LanceDB / `.index` 作为同步真相源。

## 总体架构

```text
Agent 原始记忆源
  Claude Code / Codex / Hermes / OpenClaw / Mavis
        │
        │ selective summary only
        ▼
06_rubin-memory Markdown truth source
  02_core-consensus.md
  01_initial-memory.md
  10_source-index.md
  20_conflict-index.md
  30_retrieval-policy.md
  40_index-schema.md
  memory-inbox/*
        │
        │ build disposable index
        ▼
本地索引层
  SQLite rows = chunks
  FTS5 = BM25 keyword recall
  scalar columns = status/scope/source_system/memory_type
  hashed vector = lightweight rerank
        │
        │ query/template/guard/preflight
        ▼
Agent 开工前 Top K 召回
  摘要 + 状态 + 路径
  必要时打开单个原文
```

## 记忆分层

### 1. Core consensus

路径：`06_rubin-memory/02_core-consensus.md`

用途：启动固定加载的最短共识。

规则：

- 只能放当前有效、跨 Agent、低敏、稳定规则。
- Agent 不直接改。
- Robin 明确确认后才更新。

### 2. Initial memory

路径：`06_rubin-memory/01_initial-memory.md`

用途：比 core 更详细的精选初始化记忆。

规则：

- 可进入索引高优先级。
- 不默认全文加载。
- 适合从候选升级后的稳定经验。

### 3. Source index

路径：`06_rubin-memory/10_source-index.md` 和 `06_rubin-memory/sources/*.md`

用途：告诉 Agent 原始记忆在哪里、哪些能读、哪些不能导入。

规则：

- 只记录位置和导入策略。
- 不搬运敏感值。
- 原始 sqlite/jsonl/session 不默认打开。

### 4. Conflict index

路径：`06_rubin-memory/20_conflict-index.md`

用途：排除旧结论、重复规则、敏感风险和自动记忆污染。

规则：

- 查询高风险或冲突任务时优先读。
- EverOS 当前裁定放在这里：学架构，不照搬旧 plugin。

### 5. Retrieval policy

路径：`06_rubin-memory/30_retrieval-policy.md`

用途：把任务类型映射成 scope/source/status/top-k/template。

规则：

- Agent 先判断任务类型，再查。
- 高频任务走 `rubin_memory.sh template <name>`。
- 普通任务 Top K = 3，架构任务 Top K = 5，审计任务 Top K = 10。

### 6. Candidate inbox

路径：`06_rubin-memory/memory-inbox/<source-system>/`

用途：承接新经验、失败模式、工具边界。

规则：

- `status: candidate`。
- `review_policy: human-review`。
- 不自动升级到 core。
- 审核前不得作为 active 执行规则。

## 检索流程

### Agent 默认流程

```text
1. 读 02_core-consensus.md
2. 判断任务 scope/source_system/memory_type/status
3. 优先选 template；没有 template 才用 query + filters
4. 召回 Top K 摘要
5. 检查 status、time_sensitivity、sensitivity
6. 必要时打开单个原文
7. 新经验只写 memory-inbox
```

### 当前可用命令

```bash
bash _workspace/scripts/rubin_memory.sh build
bash _workspace/scripts/rubin_memory.sh validate
bash _workspace/scripts/rubin_memory.sh query "Buffer 发布前检查" --scope publishing --status active --top-k 5
bash _workspace/scripts/rubin_memory.sh template everos-maturity
bash _workspace/scripts/rubin_memory.sh template source-locations
bash _workspace/scripts/rubin_memory.sh template candidate-write
bash _workspace/scripts/rubin_memory.sh guard chatgpt-imagegen
bash _workspace/scripts/rubin_memory.sh preflight paper-moon
```

## EverOS 映射

| EverOS-style 概念 | 当前对应物 | 裁定 |
|---|---|---|
| Markdown truth source | `06_rubin-memory/*.md` | 已采用 |
| Scalar filters | frontmatter + section YAML | 已采用 |
| BM25 | SQLite FTS5 | 已采用 MVP |
| Vector search | hashed token vector rerank | 仅 MVP，未来可替换 |
| User memory / agent memory 分轨 | `source_system` + `memory_type` | 已采用 |
| Knowledge Wiki | `01_initial-memory.md` + `02_core-consensus.md` | 手工维护，不自动生成 |
| Reflection | `memory-inbox/*` + verification docs | 候选制，不自动晋升 |
| Cloud API routes | 无 | 当前不用 |
| LanceDB / `.index` 同步 | 无 | 不进 Brainstorm 同步区 |

## 安全边界

禁止：

- 全量导入 `.claude/.codex/.hermes/.openclaw/.minimax`。
- 索引 API Key、Token、Cookie、SSH 密码、完整账号 ID。
- 把 session jsonl、sqlite、trajectory 全文作为当前规则。
- 从 archive/deprecated 召回当前规则。
- 让 OpenClaw 自动晋升内容直接进入 core。
- 把 `_System/tests/rubin-memory-index/rubin_memory.sqlite` 当真相源同步。

允许：

- 记录敏感字段存在和风险，不记录值。
- 记录原始来源路径和导入策略。
- 摘要导入稳定偏好、工作流、失败模式。
- 用本地索引缓存提升召回。

## 分阶段路线

### Phase 0：已完成

- 建立 `06_rubin-memory` truth source。
- 建立 source/conflict/retrieval/schema 四层治理文档。
- 建立 `rubin_memory.sh` wrapper。
- 建立本地 SQLite FTS5 + filter + lightweight vector index。
- 建立 trigger/preflight 查询入口。

### Phase 1：当前建议

- 用真实任务继续跑 1–2 天。
- 每次漏召回只补 template 或 metadata，不急着上外部系统。
- 新经验写 `memory-inbox/<source-system>/`。
- 每天或每轮任务后跑 `rubin_memory.sh validate`。

### Phase 2：候选闭环

- 挑 3 条真实候选记忆。
- 写入 inbox。
- 用索引查到。
- 人工判断是否合并到 `01_initial-memory.md`。
- 仍不自动改 `02_core-consensus.md`。

### Phase 3：EverOS 接入评估

只有满足以下条件才重新评估 EverOS：

- 本地 wrapper 明确不够用。
- EverOS plugin API 已稳定。
- Knowledge Wiki / Reflection 有可验证实现。
- 能只索引 `06_rubin-memory` 摘要层。
- 能保证 `.index`/SQLite/LanceDB 不成为同步真相源。

## 当前推荐

继续本地路线，不安装 EverOS。把 EverOS 当架构灵感，而不是依赖。下一步重点是候选记忆实操闭环和高频 template 精修。
```

- [ ] **Step 2: Verify the file exists**

Run:

```bash
test -f '01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md' && echo ok
```

Expected:

```text
ok
```

- [ ] **Step 3: Confirm no forbidden placeholders**

Run:

```bash
/opt/homebrew/bin/rg -n 'TBD|TODO|待补|implement later|fill in details' '01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md' || true
```

Expected: no matches.

- [ ] **Step 4: Commit**

Skip commit because this working directory is not a git repository. Record this in the final report instead of running `git commit`.

---

### Task 2: Capture Continuation Candidate

**Files:**
- Create: `06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md`

- [ ] **Step 1: Create candidate memory directory if needed**

Run:

```bash
mkdir -p '06_rubin-memory/memory-inbox/claude-code'
```

Expected: command exits 0.

- [ ] **Step 2: Write candidate memory**

Write this exact file:

```markdown
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
```

- [ ] **Step 3: Verify candidate frontmatter is indexed as candidate**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh build
bash _workspace/scripts/rubin_memory.sh query 'EverOS agent memory architecture handoff' --status candidate --top-k 5
```

Expected: output contains:

```text
2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md
```

- [ ] **Step 4: Verify active queries do not treat it as active**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh query 'EverOS agent memory architecture handoff' --status active --top-k 5
```

Expected: output should not contain:

```text
2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md
```

- [ ] **Step 5: Commit**

Skip commit because this working directory is not a git repository. Record this in the final report instead of running `git commit`.

---

### Task 3: Write Continuation Verification

**Files:**
- Create: `06_rubin-memory/_verification/2026-06-20_everos-agent-memory-architecture-continuation.md`

- [ ] **Step 1: Write verification record**

Write this exact file:

```markdown
---
title: EverOS Agent Memory Architecture Continuation Verification
index_id: verification-2026-06-20-everos-agent-memory-architecture-continuation
memory_layer: verification
status: active
scope:
  - ops
  - global
source_system:
  - rubin-memory
  - claude-code
memory_type:
  - verification
  - workflow
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/20_conflict-index.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/30_retrieval-policy.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
exclude_from_startup: true
---

# 2026-06-20_everos-agent-memory-architecture-continuation

## 接续目标

Robin 提到 `EverOS agent memory architecture` 相关进程挂掉，需要找回并继续。当前可接续的有效工作不是上游 EverOS 安装，而是 `06_rubin-memory` 的本地 EverOS-style 多 Agent 记忆架构。

## 已确认上下文

- `CURRENT.md` 记录的最近主线是 Brainstorm 长期状态板同步和 `06_rubin-memory` 检索层验证。
- `20_conflict-index.md` 已有 EverOS 插件成熟度裁定。
- `30_retrieval-policy.md` 已有未来 EverOS 映射。
- `40_index-schema.md` 已有 frontmatter 和条目级字段标准。
- `_workspace/scripts/rubin_memory.sh` 已有 `template everos-maturity`。

## 本轮验证命令

```bash
bash _workspace/scripts/rubin_memory.sh template everos-maturity
bash _workspace/scripts/rubin_memory.sh validate
```

## 期望结果

- `template everos-maturity` Top 结果命中 `20_conflict-index.md` / `EverOS 插件成熟度`。
- `validate` 能返回 active 检索结果，不报错。

## 当前判断

接续方向明确：继续本地 Markdown truth source + disposable index + candidate inbox 闭环。不要安装 EverOS，不要导入原始 Agent memory，不要改 `02_core-consensus.md`。

## 下一步

- 写一份人读架构方案到 `01_思路与方案/`。
- 写一条候选记忆到 `memory-inbox/claude-code/`，记录本次接续经验。
- 更新 `CURRENT.md` 接力纸条。
```

- [ ] **Step 2: Run validation command**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh template everos-maturity
```

Expected: Top result includes `20_conflict-index.md` and heading `10. EverOS 插件成熟度`.

- [ ] **Step 3: Run full wrapper validation**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh validate >/tmp/rubin-memory-validate-2026-06-20.log && tail -n 20 /tmp/rubin-memory-validate-2026-06-20.log
```

Expected: command exits 0 and prints JSON-like query results.

- [ ] **Step 4: Commit**

Skip commit because this working directory is not a git repository. Record this in the final report instead of running `git commit`.

---

### Task 4: Update Current Handoff

**Files:**
- Modify: `CURRENT.md:6-12`

- [ ] **Step 1: Insert latest trigger record**

Insert this bullet at the top of `## 触发记录`:

```markdown
- 2026-06-20 ｜ Claude ｜ **EverOS agent memory architecture 接续完成**。已确认挂掉进程可从 `06_rubin-memory` 接上：当前路线是不安装 EverOS、不导入原始 Agent memory，而是继续 Markdown truth source + 本地 disposable index + scalar filter/BM25/lightweight vector + candidate inbox。方案见 `01_思路与方案/2026-06-20_方案_EverOS-style多Agent记忆架构_v1.md`，候选记忆见 `06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md`。
```

Update line 6 to:

```markdown
更新时间：2026-06-20 CST
```

Update line 7 to:

```markdown
当前在聊：EverOS-style 多 Agent 记忆架构已接续到 `06_rubin-memory` 路线。下一步若继续，不是安装 EverOS，而是跑候选记忆闭环：写入 inbox → 索引召回 → 人工判断是否进 `01_initial-memory.md`。
```

- [ ] **Step 2: Verify CURRENT mentions new plan**

Run:

```bash
/opt/homebrew/bin/rg -n 'EverOS agent memory architecture 接续完成|2026-06-20_方案_EverOS-style多Agent记忆架构_v1' CURRENT.md
```

Expected: two matching lines.

- [ ] **Step 3: Commit**

Skip commit because this working directory is not a git repository. Record this in the final report instead of running `git commit`.

---

### Task 5: Final Verification

**Files:**
- Read-only verification across created/modified files.

- [ ] **Step 1: Rebuild index**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh build
```

Expected: output contains `indexed_chunks=` and exits 0.

- [ ] **Step 2: Confirm candidate status behavior**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh query 'EverOS agent memory architecture handoff' --status candidate --top-k 3
bash _workspace/scripts/rubin_memory.sh query 'EverOS agent memory architecture handoff' --status active --top-k 3
```

Expected:

- Candidate query includes `2026-06-20_claude-code_everos-agent-memory-architecture-handoff.md`.
- Active query does not include that candidate file.

- [ ] **Step 3: Confirm EverOS template still works**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh template everos-maturity
```

Expected: output includes `20_conflict-index.md` and `EverOS 插件成熟度`.

- [ ] **Step 4: Confirm verification layer remains excluded by default**

Run:

```bash
bash _workspace/scripts/rubin_memory.sh query 'Continuation Verification EverOS Agent Memory Architecture' --status active --top-k 5
```

Expected: output should not include `_verification/2026-06-20_everos-agent-memory-architecture-continuation.md`, because verification layer is excluded by default.

- [ ] **Step 5: Final report**

Report in Chinese using the 4-line format:

```text
结论：EverOS agent memory architecture 已接上，当前路线是本地 `06_rubin-memory`，不是安装 EverOS。
改动：新增架构方案、候选记忆、验证记录，并更新 CURRENT 接力。
文件：列出 4 个路径。
下一步：如果继续，跑候选记忆闭环；若要实施代码改造，再单独开计划。
```

---

## Self-Review

**Spec coverage:**
- “找一找看能不能接下去” covered by Task 3 verification and Task 4 handoff.
- “然后继续” covered by Task 1 architecture proposal and Task 2 candidate memory.
- Existing EverOS maturity裁定 covered by references to `20_conflict-index.md` and `30_retrieval-policy.md`.
- Brainstorm boundary covered: only Markdown docs and verification, no production code outside `_workspace/scripts` and no media artifacts.

**Placeholder scan:**
- The plan intentionally contains no `TBD`, `TODO`, `待补`, `implement later`, or `fill in details` placeholders.

**Type/path consistency:**
- Candidate file path is consistent across Task 2, Task 4, and Task 5.
- Proposal file path is consistent across Task 1 and Task 4.
- Commands use the existing wrapper path `_workspace/scripts/rubin_memory.sh`.
