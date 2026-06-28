---
title: Paper Moon Preflight
index_id: preflight-paper-moon
memory_layer: preflight
status: active
scope:
  - project
  - content
  - visual
  - tool
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
  - /Users/robin/AltmanCodex/Paper Moon/CURRENT.md
  - /Users/robin/AltmanCodex/Paper Moon/.learnings/LEARNINGS.md
  - /Users/robin/AltmanCodex/Paper Moon/_workspace/refs/429_BACKOFF_STANDARD.md
exclude_from_startup: true
---

# Paper Moon 最小 Preflight

> 用途：Paper Moon 视频、中文配音、精华剪辑、TTS、字幕、混音、成片交付前的短门禁。解决任务路由错、脚本阶段被跳过、字幕/混音多轮返工、429 处理临场发挥。

## 触发

- 用户提到 Paper Moon、视频、原片不剪、中文配音、TTS、字幕、混音、精华剪辑、成片、BGM、SFX。
- 任务路径在 `/Users/robin/AltmanCodex/Paper Moon`。

## 执行前 5 问

1. **任务路由**：这是不剪原片中文配音、精华剪辑、播客视频、还是发布资产整理？
2. **脚本权威**：字幕 / 旁白是逐条忠实翻译，还是可重构精华脚本？不确定先问 Robin。
3. **音色确认**：批量 TTS 前是否已有 10s / 30s 试音或音色选择记录？
4. **时间轴权威**：字幕是否来自真实 TTS / SRT？禁止按文本长度或场景平均切分。
5. **交付等级**：这次交付是样片、QC 包、最终成片，还是多平台发布包？

## 硬门禁

- “不剪原片 / 原片中文配音 / 消除原声”：禁止摘要压缩，必须逐条对齐原字幕 / 原时长。
- 精华剪辑：`source_outline / selection_map / fact_check / essence_script / script_self_check` 未 PASS 前，禁止 TTS / 剪辑 / 字幕烧录。
- 混音交付：不能只看总响度 / 静默；必须抽同一时间段三轨样本，真听 TTS 与 BGM / SFX 是否同时可闻。
- 字幕：固定位置 / 字号 / 字体先锁；禁止动态字号、第二字幕、顶部观点字，除非 Robin 明确指定。
- TTS：不能靠 speed 凑时长；批量生成前必须有试音确认或明确音色记录。
- 外部请求：默认单线程；首次 429 只允许一次小请求重试，第三次当日停止该域并记录原因。

## 路由表

```markdown
| 用户目标 | 正确路线 | 禁止误走 |
|---|---|---|
| 不剪原片中文配音 | full-length-subtitle-dub | 精华压缩/摘要脚本 |
| 精华剪辑/解说短片 | script-first recut | 先 TTS/先剪辑 |
| 播客/长文视频 | book-podcast-video | 直接套短视频字幕 |
| 成片发布包 | final delivery + multi-platform copy | 只给路径/过程件 |
```

## QC 表

```markdown
| 项目 | PASS/FAIL | 说明 |
|---|---|---|
| 任务路由正确 |  |  |
| 脚本/字幕权威明确 |  |  |
| TTS 试音已确认 |  |  |
| 字幕来自真实 TTS/SRT |  |  |
| 混音三轨样本已真听 |  |  |
| 字幕不遮挡关键画面 |  |  |
| 交付等级与产物一致 |  |  |
```

## 输出格式

- 开工前输出 `任务路由 + 缺口 + 下一步`。
- 交付前输出 `QC 表 + 产物清单 + 未验证项`。
- 有未验证项时不能写“最终完成”。
