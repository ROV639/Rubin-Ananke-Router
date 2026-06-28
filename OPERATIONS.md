# OPERATIONS｜首次整理与上架运维记录

日期：2026-06-29 CST

## 本次目标

把 Robin 已有的五类能力系统整理成一个 GitHub 仓库，供 Codex、Claude Code、OpenClaw、Hermes 或其他 Agent 查看：

- `06_rubin-memory`：总记忆与检索治理，truth source 和路由规则底座。
- `serenity-brainstorm`：分析/推理框架，主要用于投资和严肃判断。
- `_System/superpowers`：执行流程方法论，如 plan、verification、debugging。
- `luobin-ananke`：人格/判断风格系统，适合严肃思考、审判、诊断。
- `Rubin-Ananke-Router`：能力卡组和任务装备路由，支持单选和多选能力。

## 已整理内容

- 完整纳入 `06_rubin-memory` 资料。
- 纳入 `serenity-brainstorm` 的入口文档。
- 纳入 Superpowers 相关 `SKILL.md` 和 Brainstorm 里的 superpowers plans。
- 纳入 `luobin-ananke` 原始人格系统 spec。
- 纳入并修正 `Rubin-Ananke-Router`。
- 纳入生图和媒体工具的必要入口文档，避免把完整工具仓库无差别复制进来。

## 自查结论

### 已修正的问题

1. `Ananke_通用框架` 第一版漏掉 `luobin-ananke` 人格核心。
   - 已新增 `07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md`。
   - 已同步更新 `00_总纲`、`04_卡组`、`06_自检`、`09_部署`。
   - 已在 `91_SOURCE_AUDIT.md` 和 `01_REVIEW_这个文件夹是什么_引用来源与检查说明.md` 写明来源和修正记录。

2. `04_卡组_Ananke_通用能力卡摘要.md` 有一处 Markdown 表格分隔行错误。
   - 已修复表格渲染。

3. `01_REVIEW` 保留了“Ananke_通用框架 是最弱层”的旧判断。
   - 已改为：第一版确实弱，现版已补齐人格内核，但仍需真实任务测试后才能转正。

### 仍需验证的问题

- `Ananke_通用框架` 需要用真实任务测试：投资复盘、项目取舍、拖延诊断、内容方向审判。
- Router cards 是否真的能减少 Agent 错误调用，需要在 Codex / Claude Code / OpenClaw / Hermes 中各跑一轮测试。
- 媒体工具是否应继续留在本仓库，还是拆成独立 tool registry，后续再定。
- 当前仓库是参考资料包，不是正式安装包；任何 Agent 不应声称它已自动触发。

## 上架原则

- 默认创建私有 GitHub 仓库。
- 保留来源和 provenance，不把 Claude / Codex / 第三方 skill 的资料伪装成原创。
- 不上传 `.DS_Store`、`__pycache__`、`.pyc`、`node_modules`、`.env`、key、token、构建产物。
- 上传前执行敏感信息扫描；发现疑似 secret 必须脱敏处理或确认是假阳性。

## 上架前验证

- 文件规模：约 155 个文件，约 1.2MB。
- 缓存扫描：未发现 `.DS_Store`、`__pycache__`、`.pyc`、`node_modules`、`.env`。
- 大文件扫描：未发现超过 5MB 的文件。
- 敏感信息扫描：未发现真实 key/token/password/private key。命中项均为模板词或普通文档词，例如 `<task-slug>-<timestamp>`、`sub-skill`、`task-replay-test`。
- 旧判断残留扫描：已清理 `Ananke_通用框架` “目前过薄 / 最弱层”这类未更新结论；现文档改为“第一版漏人格核心，已补齐，但需真实任务测试后转正”。

## 给后续 Agent 的维护规则

1. 新增能力时，先判断属于 memory、analysis、execution、persona、router、media-tools 哪一层。
2. 新增或改写 Router card 时，必须写清：何时用、读什么、做什么、禁止什么、完成信号。
3. `Ananke_通用框架` 修改时必须同时检查 `07_人格` 和 `09_部署`，防止人格层与全局提示词漂移。
4. 历史草案只能放在 `06_source-proposals/` 或审计备注里，不得当成当前有效规则。
5. 每次较大更新后，在本文件或 `99_operations/` 追加运维记录。
