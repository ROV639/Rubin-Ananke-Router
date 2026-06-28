# Rubin-Ananke-Router

Robin 的多 Agent 能力路由参考仓库。

本仓库不是已安装的全局 skill，也不是某个 Agent 的自动执行系统。它用于把现有能力整理成可检查、可迁移、可由其他 Agent 阅读的资料包：记忆治理、严肃分析、执行方法论、Luobin-Ananke 人格判断系统、能力卡组路由，以及少量媒体工具入口。

默认按私有仓库维护，因为资料中包含本地路径、内部工作流和未转正的 agent 规则。

## 先读顺序

给其他 Agent 的建议入口：

1. `README.md`：本文件，确认仓库定位。
2. `INDEX.md`：全目录索引。
3. `OPERATIONS.md`：整理过程、自查、已知问题。
4. `05_capability-router/Rubin-Ananke-Router/00_DECK_INDEX.md`：能力卡组总索引。
5. `05_capability-router/Rubin-Ananke-Router/Ananke_通用框架/README.md`：AI 工具层轻量 Ananke 包。

## 五个核心系统

| 目录 | 来源角色 | 用途 |
|---|---|---|
| `01_memory-governance/` | `06_rubin-memory` | 总记忆与检索治理，truth source 和路由规则底座 |
| `02_analysis-reasoning/` | `serenity-brainstorm` | 投资、市场、账号、严肃判断的分析/推理框架 |
| `03_execution-methods/` | `_System/superpowers` + Superpowers skills | 计划、调试、验证、完成前检查等执行流程方法论 |
| `04_persona-judgment/` | `luobin-ananke` | 人格/判断风格系统，适合审判、诊断、严肃取舍 |
| `05_capability-router/` | `Rubin-Ananke-Router` | 能力卡组、bundle、Agent adapter、AI 工具轻量层 |

辅助目录：

| 目录 | 用途 |
|---|---|
| `06_source-proposals/` | 早期能力卡组方案来源，保留 provenance |
| `07_media-tools/` | 生图和媒体工具入口文档，只放必要路由资料 |
| `99_operations/` | 后续运维记录归档区 |

## Ananke 特别说明

`Ananke_通用框架` 现在必须同时包含两层：

- Ananke：代价、盲区、反证、执行、边界的判断镜头。
- Luobin-Ananke：烧瓶小人人格、七宗罪双面雷达、冷水后给行动驱动。

如果只读判断流程、不读 `07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md`，就会丢掉人格核心。

## 使用边界

- 不要把本仓库直接当成正式全局配置安装。
- 不要把 candidate 或历史草案自动升级为当前规则。
- 其他 Agent 使用时，应按 `INDEX.md` 和 Router cards 选择必要文件，不要全仓库灌上下文。
- 涉及时效事实、投资、医疗、法律、价格、政策，必须重新核查当前资料。
