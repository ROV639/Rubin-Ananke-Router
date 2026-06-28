---
title: Eve Frames Preflight
index_id: preflight-eve-frames
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
  - /Users/robin/AltmanCodex/Eve_Frames/CURRENT.md
  - /Users/robin/AltmanCodex/Eve_Frames/.learnings/LEARNINGS.md
exclude_from_startup: true
---

# Eve_Frames 最小 Preflight

> 用途：EVE 真人图、母图、锁脸、系列扩展前的短门禁。解决纯文生图、母图定义错误、误判 KEEP、泛 AI 美女脸、后续扩展不可用。

## 触发

- 用户提到 EVE、人像、portrait、锁脸、identity、reference、母图、系列图、KV、封面、16x9 / 9x16 扩展。
- 任务路径在 `/Users/robin/AltmanCodex/Eve_Frames`。

## 执行前 5 问

1. **任务类型**：这是母图、单张成片、系列扩展、KV，还是返工 QC？
2. **身份锚点**：这轮是否有真实参考图输入？没有则不得进入母图候选。
3. **母图用途**：这张图是否会作为后续扩展参考？如果会，优先身份/构图/可扩展性，不只看单张漂亮。
4. **允许变化**：服装、场景、姿势、光线、裁切哪些可变？脸部身份哪些不可变？
5. **验收方式**：Robin 是要先看候选母图、QC 判断，还是直接要最终图？不确定先问 1 个问题。

## 硬门禁

- 没有真实 `identity_ref` / 参考图输入：不得标“锁脸”“母图候选”。
- 只靠文字描述生成的 EVE：必须标为草稿 / 风格探索，不能进成品链。
- 母图候选必须写清：身份锚点、构图锚点、可扩展性、失败风险。
- 明显脸小、脸糊、贴脸感、泛 AI 美女脸、复制旧姿势：Agent 自行 REJECT，不拿去问 Robin。
- 世界杯 / 体育 KV：不得复制上一国家姿势；必须检查国家元素、运动广告能量、自然身体比例。

## QC 表

```markdown
| 项目 | PASS/FAIL | 说明 |
|---|---|---|
| 真实参考图已输入 |  |  |
| 身份像 EVE 而非泛 AI 美女 |  |  |
| 脸部自然，不是贴脸 |  |  |
| 构图适合后续扩展 |  |  |
| 场景/服装/姿势与任务相符 |  |  |
| 不复制旧图姿势或构图 |  |  |
| 可作为母图/成品的判断 |  |  |
```

## 输出格式

- 先给 `QC 表 + KEEP/REJECT + 下一步建议`。
- 只有全 PASS 才进入扩展或成品交付。
- FAIL 时直接说明返工方向，不包装成“可选候选”。
