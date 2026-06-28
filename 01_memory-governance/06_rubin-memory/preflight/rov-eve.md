---
title: Rov Eve Preflight
index_id: preflight-rov-eve
memory_layer: preflight
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
  - project
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/AltmanCodex/Rov_Eve/CURRENT.md
  - /Users/robin/AltmanCodex/Rov_Eve/.learnings/LEARNINGS.md
  - /Users/robin/AltmanCodex/Rov_Eve/_workspace/refs/ROVEVE_FIXED_IMAGE_RULES_v1.md
exclude_from_startup: true
---

# Rov_Eve 最小 Preflight

> 用途：ROV×EVE 动漫角色、系列图、漫画页、分镜、AI 教程图前的短门禁。解决角色漂移、模板感、服装不换、UI 信息层混乱、事实逻辑打架。

## 触发

- 用户提到 ROV、EVE、动漫角色、漫画页、分镜、六格、AI Slop、角色连续性、LOCK、系列图。
- 任务路径在 `/Users/robin/AltmanCodex/Rov_Eve`。

## 执行前 5 问

1. **路线**：这是单张、系列图、漫画页，还是教程分镜？漫画页必须先做分镜结构。
2. **参考图**：是否真喂 `LOCK_` / 角色 / 风格参考？不支持参考图的工具不得做正式母图。
3. **逐图变量**：每张是否至少改变服装、场景、时间、机位、动作、UI 载体中的 4 项？
4. **AI 教学点**：每张图普通人能学到的 AI 帮助点是什么？不能只有好看人物。
5. **事实边界**：涉及真实赛事 / 产品 / 数据时，是否允许虚构？不确定先问 Robin。

## 硬门禁

- 禁止纯文母图；正式角色图必须真实喂参考图。
- 禁止六等分横条直接出图；漫画页必须先有页面节奏、格子层级、逐场服装矩阵。
- prompt 禁止写 `px`、字号、百分比、后端尺寸；只写 aspect ratio。
- 实体手机 / 平板 / 纸张朝向必须服务画内角色；观众信息用独立透明 HUD。
- UI 不能像 PPT / 软件截图 / 大 dashboard；生活画面先成立，再叠信息层。
- 相邻图如果只是换标题 / 文案 / 颜色，直接 FAIL。

## 逐图变量矩阵

```markdown
| 图号 | 服装 | 场景/时间 | 机位/景别 | 动作/表情 | UI 载体 | AI 教学点 | 与上一张差异数 |
|---|---|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  | - |
| 2 |  |  |  |  |  |  |  |
| 3 |  |  |  |  |  |  |  |
```

## QC 表

```markdown
| 项目 | PASS/FAIL | 说明 |
|---|---|---|
| 角色参考图已输入 |  |  |
| ROV/EVE 角色一致 |  |  |
| 逐图差异足够 |  |  |
| 生活画面自然成立 |  |  |
| UI 信息层不过重 |  |  |
| 真实事实/虚构边界清楚 |  |  |
| prompt 无尺寸/字号噪音 |  |  |
```

## 输出格式

- 先给 `逐图变量矩阵 + QC 表 + KEEP/REJECT`。
- 系列图未过矩阵，不进入批量生成。
- 角色 / 风格基因漂移时停止局部修，回到母图重做。
