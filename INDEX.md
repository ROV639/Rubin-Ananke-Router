# INDEX｜Rubin-Ananke-Router 仓库索引

## 00 根入口

| 文件 | 用途 |
|---|---|
| `README.md` | 仓库定位、先读顺序、核心系统说明 |
| `INDEX.md` | 本索引 |
| `OPERATIONS.md` | 本次整理、自查、上架记录 |
| `.gitignore` | 防止缓存、密钥、构建产物进入仓库 |

## 01 记忆治理

路径：`01_memory-governance/06_rubin-memory/`

用途：总记忆与检索治理，负责 truth source、candidate inbox、冲突裁定、检索规则。

建议先读：

- `README.md`
- `02_core-consensus.md`
- `30_retrieval-policy.md`
- `40_index-schema.md`

## 02 分析推理

路径：`02_analysis-reasoning/serenity-brainstorm/`

用途：严肃分析、投资方向、账号/项目判断、多源核查和反证。

建议先读：

- `SKILL.md`
- `README.md`

## 03 执行方法

路径：

- `03_execution-methods/superpowers-skills/`
- `03_execution-methods/brainstorm-superpowers-plans/`

用途：计划、调试、验证、写 skill、完成前检查等执行流程。

建议先读：

- `superpowers-skills/writing-plans/SKILL.md`
- `superpowers-skills/systematic-debugging/SKILL.md`
- `superpowers-skills/verification-before-completion/SKILL.md`

## 04 人格判断

路径：`04_persona-judgment/`

用途：`luobin-ananke` 原始人格系统来源。它定义 Ananke 不是普通严肃语气，而是烧瓶小人观察者和七宗罪双面雷达。

建议先读：

- `2026-06-23_luobin-ananke_persona-system-spec.md`

对应整合位置：

- `05_capability-router/Rubin-Ananke-Router/Ananke_通用框架/07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md`
- `05_capability-router/Rubin-Ananke-Router/Ananke_通用框架/09_部署_Ananke_全局提示词.md`

## 05 能力路由

路径：`05_capability-router/Rubin-Ananke-Router/`

用途：总装备系统。Agent 开始任务前，用它选择要装备的能力卡、bundle 和平台适配规则。

建议先读：

- `README.md`
- `01_REVIEW_这个文件夹是什么_引用来源与检查说明.md`
- `00_DECK_INDEX.md`
- `91_SOURCE_AUDIT.md`
- `90_TEST_CASES.md`

关键子目录：

- `classes/`：能力大类。
- `cards/`：单张能力卡。
- `bundles/`：常见任务装备包。
- `agents/`：Codex、Claude Code、OpenClaw、Hermes 适配。
- `Ananke_通用框架/`：Claude / ChatGPT / Grok 可上传的轻量框架。

## 06 来源方案

路径：`06_source-proposals/`

用途：早期 `rubin-capability-router` 能力卡组方案。保留为 provenance，不直接作为当前执行入口。

## 07 媒体工具

路径：`07_media-tools/`

用途：只放生图和媒体工具的必要入口文档，帮助 Router 指向能力，不复制完整工具生态。

包含：

- `chatgpt-imagegen/`
- `codex-system-imagegen/`
- `gpt-image-2/`
- `baoyu-image-gen/`
- `baoyu-cover-image/`
- `baoyu-xhs-images/`
- `minimax-image/`

## 99 运维归档

路径：`99_operations/`

用途：后续维护记录归档。当前根目录 `OPERATIONS.md` 是首版上架记录。
