---
title: ChatGPT Portrait Template Trigger Card
index_id: trigger-chatgpt-portrait-template
memory_layer: trigger
status: active
scope:
  - visual
  - content
source_system:
  - claude-code
  - codex
  - rubin-memory
memory_type:
  - workflow
  - tool
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/.claude/skills/chatgpt-imagegen/SKILL.md
  - /Users/robin/.claude/skills/chatgpt-imagegen/references/prompt-parameters.md
  - /Users/robin/.claude/skills/chatgpt-imagegen/references/visual-parameters.md
exclude_from_startup: true
---

# chatgpt-portrait-template 触发卡

> 人物 / 角色 / 锁脸 / 参考图生成的模板调用门禁。目标：解决 Agent 调了生图 skill 但不用人物模板、锁脸模板的问题。

```text
触发：人物、人像、角色、头像、EVE/ROV、锁脸、参考图、identity、character、portrait、face consistency。
先查：chatgpt-imagegen references/prompt-parameters.md：A 人物参数、C4 EVE 锁脸、C5 通用人物锁、C6 动漫角色锁；旧 visual-parameters 只作索引。
必须：prompt 包含 identity anchor / fixed identity / allowed changes；有参考图时用 --image/--reference；先母图再扩展。
禁止：只写泛风格不写身份固定；发明看不见的脸部细节；不用锁脸模板直接换场景；把 visual-parameters 当第二套模板。
下一步：判断 EVE / 通用人物 / 动漫 → 复制对应锁脸模板骨架 → 填可见特征和可变项 → 再调用 chatgpt-imagegen。
```

## 模板定位

- EVE 真人：`prompt-parameters.md` C4。
- 通用人物 / 半真人 / 虚构人物：`prompt-parameters.md` C5。
- 动漫角色：`prompt-parameters.md` C6。
- 非人类风格 / 场景锁定：`prompt-parameters.md` C7。
- 旧 `visual-parameters.md` 只提示去新文件，不作为第二套执行模板。
