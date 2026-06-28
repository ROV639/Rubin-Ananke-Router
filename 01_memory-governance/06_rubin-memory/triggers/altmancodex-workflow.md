---
title: AltmanCodex Workflow Trigger Card
index_id: trigger-altmancodex-workflow
memory_layer: trigger
status: active
scope:
  - project
  - ops
source_system:
  - codex
  - rubin-memory
memory_type:
  - workflow
sensitivity: medium
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/AltmanCodex/AGENTS.md
  - /Users/robin/AltmanCodex/_System/skills/ready/thread-handoff/SKILL.md
exclude_from_startup: true
---

# AltmanCodex 工作流触发卡

> 进入 AltmanCodex 或其子项目时的短门禁。目标是防止只读一层规则就开干。

```text
触发：任务路径在 /Users/robin/AltmanCodex，或 Robin 点名 AltmanCodex 子项目。
先查：最近 AGENTS.md → 同目录 CURRENT.md → 必要时 thread-handoff；涉及工具/多模态再查 api-search-media-route。
必须：不跨子项目读写；阶段点/暂停/完成更新 CURRENT.md；图片输出走工作区 ASCII 预览路径。
禁止：跳过 CURRENT.md；把 Brainstorm 当生产区；未确认写正式 Skill/Key/.env/shell/git push。
下一步：确认子项目与当前任务 → 读项目 AGENTS/CURRENT → 执行项目 workflow → 收尾回写 CURRENT。
```

## 高频子项目入口

- `Arthur_Observatory`：公众号长文、排版、封面、信任沉淀。
- `Rubin_Motion_Lab`：无真人出镜动画解释视频 / Remotion。
- `Web6`：站点维护、Cloudflare、媒体上传链路。
- `Eve_Frames` / `Rov_Eve`：视觉生产、人像与生活化教程。
- `Single`：A股 AI 赛道金融研究，事实核查优先。
