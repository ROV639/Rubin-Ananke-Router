---
title: AltmanCodex Image Identity Mother Trigger Card
index_id: trigger-altmancodex-image-identity-mother
memory_layer: trigger
status: active
scope:
  - project
  - visual
  - content
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
  - /Users/robin/AltmanCodex/Eve_Frames/.learnings/LEARNINGS.md
  - /Users/robin/AltmanCodex/Rov_Eve/.learnings/LEARNINGS.md
  - /Users/robin/AltmanCodex/Rov_Eve/_workspace/refs/ROVEVE_FIXED_IMAGE_RULES_v1.md
exclude_from_startup: true
---

# altmancodex-image-identity-mother 触发卡

> AltmanCodex 图像身份/母图/系列图门禁。目标：防止纯文生图、母图定义错误、角色漂移、系列图模板感。

```text
触发：EVE/ROV/人物/角色/母图/系列图/漫画页/分镜/锁脸/参考图/identity_ref/character consistency。
先查：项目 AGENTS.md + CURRENT.md；Eve_Frames/Rov_Eve 的 lock/ref/mother/image workflow；chatgpt-portrait-template 触发卡。
必须：真实喂参考图；母图说明身份锚点+构图锚点+可扩展性；系列图先填变量矩阵（服装/场景/机位/动作/UI/教学点）。
禁止：纯文母图；把单张好看当母图；复制上一张姿势/服装/镜头；锁脸变贴脸；prompt 写 px/字号/百分比。
下一步：先出母图/变量矩阵/QC 判断 → 不过门禁则重做母图，不进入批量扩展。
```
