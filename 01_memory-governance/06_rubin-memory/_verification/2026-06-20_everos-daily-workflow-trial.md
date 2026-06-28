---
title: EverOS Daily Workflow Trial
index_id: verification-2026-06-20-everos-daily-workflow-trial
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
  - failure
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/memory-inbox/claude-code/2026-06-20_claude-code_daily-recall-template-gap.md
exclude_from_startup: true
---

# 2026-06-20_everos-daily-workflow-trial

## 试跑目标

验证 EverOS-style 日常流程是否可执行：开工前 Top K 召回 → 执行一个真实微任务 → 任务后把经验写入 inbox → 验证候选可召回且不会污染 active。

## 开工前召回

执行：

```bash
bash _workspace/scripts/rubin_memory.sh template source-locations
bash _workspace/scripts/rubin_memory.sh template candidate-write
bash _workspace/scripts/rubin_memory.sh template everos-maturity
```

观察：

- `source-locations` 能召回 `10_source-index.md` / Claude Code、Mavis / MiniMax 和 source card。
- `everos-maturity` 能召回 `20_conflict-index.md` / EverOS 插件成熟度。
- `candidate-write` 能召回 `02_core-consensus.md` 的候选流程、记忆分层和敏感边界，但没有把 `memory-inbox/README.md` 的候选文件模板推到 Top K。

## 任务后写入

已写入候选：

`memory-inbox/claude-code/2026-06-20_claude-code_daily-recall-template-gap.md`

候选结论：`candidate-write` template 适合提醒“不要直接改 core”，但真实写 candidate 时还需要读 `memory-inbox/README.md`；后续可优化 template 让 inbox README 更靠前。

## 验证项

后续验证应确认：

1. 候选文件能通过 `--status candidate` 召回。
2. 候选文件不会通过 `--status active` 召回。
3. `rubin_memory.sh validate` 仍通过。
4. 不修改 `02_core-consensus.md`。

## 当前判断

日常流程可跑通，但暴露出一个真实可优化点：模板层不仅要召回原则，也要召回可复制的写入格式。
