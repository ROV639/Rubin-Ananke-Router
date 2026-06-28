---
title: AltmanCodex Artifact Is Not Result Trigger Card
index_id: trigger-altmancodex-artifact-not-result
memory_layer: trigger
status: active
scope:
  - project
  - ops
  - content
  - publishing
source_system:
  - codex
  - claude-code
  - rubin-memory
memory_type:
  - workflow
  - failure
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/AltmanCodex/Arthur_Observatory/.learnings/ERRORS.md
  - /Users/robin/AltmanCodex/ONE_SPARK/CURRENT.md
  - /Users/robin/AltmanCodex/Web6/.learnings/LEARNINGS.md
  - /Users/robin/AltmanCodex/Single/.learnings/LEARNINGS.md
exclude_from_startup: true
---

# altmancodex-artifact-not-result 触发卡

> 过程件不等于成果门禁。目标：防止把 smoke、路径、草案、开关、纯文本稿、preview 当成生产完成。

```text
触发：报告完成/成品/发布版/生产生效/巡航完成/已接入，但证据只是路径、草案、配置、preview、工具 smoke、纯文本稿或单屏截图。
先查：项目 CURRENT.md 的“当前有效产物/禁止误用”；相关 .learnings/ERRORS；发布/图床/成品类任务查 QC 或检查单。
必须：列出本轮实读证据、真实产物、验证方式、未验证项；生产域/发布链必须验证最终入口而非 preview。
禁止：过程件冒充成品；工具测试冒充业务进展；配置存在冒充真实运行；旧数据/全局最新文件冒充本次产物。
下一步：补齐真实验证 → 不足则标“过程件/待验证”，不得写完成；必要时回到上游阶段返工。
```
