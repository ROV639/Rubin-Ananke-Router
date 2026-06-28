---
name: serenity-brainstorm
description: Use this skill when the user asks to analyze an account, judge whether a message or event is credible, evaluate an opportunity,拆解 @某账号, or when the user asks for content direction, topic mining, packaging, or copywriting based on evidence. Trigger phrases include "分析这个账号", "这件事怎么看", "这个消息可信吗", "有没有机会", "拆解 @", "写文案", "找选题", "内容方向", "话题挖掘", "怎么包装这个".
---

# Serenity Brainstorm

Serenity Brainstorm turns noisy account posts, market events, or content directions into evidence-backed judgment or publishable direction. It is a routing skill: decide the mode first, then load the relevant reference and execute each step file. Do not compress the chain into a summary.

## Mode Decision

Decide before doing any analysis:

- **Inference mode**: the user gives an existing account, event, message, thesis, rumor, or claim and wants judgment. Read `references/inference-mode.md`.
- **Execution mode**: the user gives a production direction and wants topics, packaging, copy, or a content plan. Read `references/execution-mode.md`.
- **Ambiguous**: ask one short question to choose the mode. Do not run both modes halfway.

## Chain

Both modes use the same method, but in opposite directions:

`信息源 -> 取证查证（交叉比对）-> 筛选标准 -> 论证结构 -> 语言包装 -> 发布节奏 -> 互动策略 -> 变现路径`

`2-verify` is the core step. It separates `已证实`, `推断`, and `未核实`; do not mark anything as verified from a single source.

## Route

After choosing the mode:

1. Read the corresponding mode reference.
2. Read and execute the step files named by that reference.
3. For every step, include the judgment basis. If a step cannot be completed, say what evidence is missing instead of filling the gap with confidence.

## Tool Interfaces

- X / live tweet evidence: use `x-grok-build xsearch` or Hermes `x_search`. If output is `degraded=true` or `STATUS: DEGRADED`, discard it or rerun with broader filters.
- Web / independent source evidence: use `api-search-media-route`, especially its Tavily wrapper at `api-search-media-route/scripts/tavily.sh`; do not call single-key `tvly` directly.
- Account timeline collection may need browser/X GraphQL when `xsearch` cannot return full originals. Keep raw text and metadata; analysis happens after collection.

## Output Discipline

- Inference mode must include conclusion, evidence, cross-verification, confidence, and disclosure.
- Execution mode must include topic/copy, contrast point, evidence, publishing advice, interaction plan, and monetization direction.
- Any claim with only one source is `未核实` or `推断`, never `已证实`.
- Conflicting sources must be recorded, not smoothed over.
