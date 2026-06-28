# 00_full-memory-extract

> 完整记忆抽取清单。目标是“尽量不漏来源和有用内容”，不是启动加载文件。敏感值已脱敏；原始记忆不复制搬家，只保留摘要、路径、类型、当前有效性和是否建议进入初始化记忆。

## 抽取范围

本轮只读抽取以下系统：

- Claude Code memory：`/Users/robin/.claude/projects/*/memory/`
- Codex memory：`/Users/robin/.codex/memories/`
- Hermes memory：`/Users/robin/.hermes/memories/`、`/Users/robin/.hermes/SOUL.md`
- OpenClaw memory：`/Users/robin/.openclaw/workspace*/MEMORY.md`、`/Users/robin/.openclaw/workspace*/memory/*.md`、sessions index
- Mavis / MiniMax memory：`/Users/robin/.minimax/memory/`、`/Users/robin/.minimax/agents/*/memory/`、MiniMax 相关 skills
- Brainstorm：`MEMO.md`、`CURRENT.md`、`Global_Agent_Rules.md`、`02_决定与共识/`

未做：

- 未 dump sqlite。
- 未导出完整 jsonl session。
- 未输出任何 API Key / Token / Cookie / 账号认证值。
- 未修改原始 memory。

## 记忆字段口径

```yaml
source_system:
source_path:
memory_type: consensus | preference | decision | workflow | failure | project | tool | security | session | archive | raw-log
status: active | candidate | deprecated | archived | sensitive-excluded
scope: global | project | tool | publishing | visual | security | ops | content | unknown
recommend_init: yes | maybe | no
reason:
```

---

# Claude Code Memory

## 总体结构

```yaml
source_system: claude-code
source_path: /Users/robin/.claude/projects/*/memory/
memory_type: mixed
status: active + archived + sensitive-excluded
scope: project/global/tool
recommend_init: maybe
reason: Claude Code memory 是项目分桶的长期记忆，适合抽取偏好和共识，但不能全量导入。
```

Claude Code 记忆按项目路径编码分桶。常见结构：

- `MEMORY.md`：索引。
- 单条记忆 `.md`：带 frontmatter 或普通 Markdown。
- 新式单条通常有 `name`、`description`、`metadata.type`。

## Brainstorm 项目 memory

```yaml
source_path: /Users/robin/.claude/projects/-Users-robin-Rubin-Studio--Brainstorm---Agent--/memory/
memory_type: preference
status: active
scope: global/communication
recommend_init: yes
```

有效条目：

1. `image-preview-direct-embed.md`：生成图片应直接在聊天中展示，不只给路径。
2. `claude-routing-via-cc-switch-surge.md`：Claude→cc-switch→Surge 路由视为有意 baseline。
3. `feedback-thread-continuity.md`：优先在当前线程恢复中断任务，不推新线程。

判断：这些是稳定偏好，适合进入初始化记忆，但图片预览规则只在图片交付任务时召回，不应每次启动全文加载。

## AltmanCodex 项目 memory

```yaml
source_path: /Users/robin/.claude/projects/-Users-robin-AltmanCodex/memory/
memory_type: preference/workflow
status: active
scope: visual/content
recommend_init: maybe
```

有效条目：

- `ugc-visual-style-preference.md`：偏好 Claude-like mythic illustration、clean layouts、不要默认页码。
- `image-delivery-preview-preference.md`：交付图片时打开图片文件夹或单张大图，默认避免 overview sheet。
- `compact-visual-review-prompts.md`：视觉 review 分批、避免 oversized prompts。

判断：适合进入视觉/图片任务检索，不适合 core consensus 固定加载。

## AgentProject memory

```yaml
source_path: /Users/robin/.claude/projects/-Users-robin-AgentProject/memory/
memory_type: mixed/security/tool/workflow/archive
status: mixed + sensitive-excluded
scope: project/tool/ops
recommend_init: no for full import; selective maybe
```

重要内容类别：

- NotebookLM 工作流。
- Poseidon / Single Song / Arthur 等历史项目经验。
- MiniMax quota / TTS voice / music 生产经验。
- OpenClaw memory agent 架构设想。
- Skill 扫描和 SkillHub 相关经验。
- UI / 设计 token / 中文 UI label 偏好。
- 多条凭证类、API keys、账号映射类 memory。

高敏条目类型：

- Cloudflare credentials。
- ElevenLabs credentials。
- Tavily keys。
- NAS SSH method。
- NotebookLM ID。
- API keys 存档段。

判断：AgentProject memory 是历史资产丰富区，但不应全量导入。只可抽取脱敏规则和可复用工作流。任何 credentials / ID / key 类条目都只能记录“存在敏感配置”，不能进入初始化记忆。

## Hermes 项目 Claude memory

```yaml
source_path: /Users/robin/.claude/projects/-Users-robin--hermes/memory/
memory_type: tool/workflow/reference
status: active
scope: tool/ops
recommend_init: maybe
```

条目：

- `minmax_m3_multimodal_capabilities.md`：mmx CLI 与 Matrix MCP 字段差异、升级调用方式、region/key 提醒。
- `skillhub_centralization.md`：新 skill 必须进入 SkillHub，不再本地散落。
- `search_router_skill.md`：api-search-media-router 是个人本地例外，不入 SkillHub；key 在 `~/.config/search-router/.env`。
- `agent_project_self_trigger.md`：进入 AgentProject 必读结构规则和简报索引。

判断：可抽取为工具路由/Skill 规则，但 key 路径只保留路径和边界，不记录值。

## Claude root memory

```yaml
source_path: /Users/robin/.claude/projects/-Users-robin/memory/
memory_type: user/project/reference
status: mixed
scope: global/project
recommend_init: maybe
```

内容：

- 用户身份与沟通风格。
- 回答逻辑：结论→证据→分析→不确定性。
- 达芬奇实验室 OpenClaw 多 Agent 系统架构。
- ROVEN 四大项目、目录原则等较旧结构。

判断：部分可用，但要和 Brainstorm 当前共识对齐，旧 ROVEN/达芬奇信息默认作为历史参考。

---

# Codex Memory

## 总体结构

```yaml
source_system: codex
source_path: /Users/robin/.codex/memories/
memory_type: auto-summary + preference + project/workflow/failure
status: active + historical
scope: global/project/tool/content
recommend_init: yes selective
```

文件结构：

- `MEMORY.md`：综合用户画像、偏好、项目索引。
- `memory_summary.md`：按 task group 聚合的结构化摘要。
- `raw_memories.md`：保留 thread/cwd/task/outcome/keywords/preference/failure 等原始归并记录。
- `rollout_summaries/`：按时间和任务形成的 rollout 摘要。
- `extensions/ad_hoc/instructions.md`：扩展/临时指令。

## 最新内容类型

Codex memory 在 2026-06-19 仍有更新，集中在：

- Rubin Motion Lab。
- Single / Web6 / Paper Moon。
- SkillHub。
- MiniMax。
- Hermes / OpenClaw。
- 图片生成与 continuity。
- 生产流程与失败复盘。

## 有用内容

```yaml
memory_type: workflow/failure/preference
status: active or historical
recommend_init: yes selective
```

适合抽取：

- Codex 是生产主端的稳定共识。
- 生产任务要真实验证，不要未验证就报完成。
- 图片交付需要可预览。
- 视觉审查要压缩预览、一图一次、写 review/QC 结论。
- SkillHub / skill 生产链的中心化和隔离原则。
- 项目执行状态应进入项目 `CURRENT.md`，跨项目思路进入 Brainstorm。

不适合初始化：

- 具体 rollout 的历史任务状态。
- 过期 cwd / thread / task id。
- 一次性失败日志原文。
- 自动汇总中未确认的推断。

---

# Hermes Memory

## 总体结构

```yaml
source_system: hermes
source_path: /Users/robin/.hermes/memories/
memory_type: user-preference + system-memory + judgment-rules
status: active
scope: global/ops/content/publishing
recommend_init: yes selective
```

核心文件：

- `/Users/robin/.hermes/memories/MEMORY.md`
- `/Users/robin/.hermes/memories/USER.md`
- `/Users/robin/.hermes/SOUL.md`

Hermes 的 memory 以 `§` 分隔条目。工具层有 `memory_tool.py`，支持 add/replace/remove/read，并有文件锁和注入扫描。Hermes provider 支持 session 生命周期的 prefetch、sync_turn、on_session_end、on_pre_compress。

## Hermes MEMORY.md 关键条目

```yaml
source_path: /Users/robin/.hermes/memories/MEMORY.md
status: active
recommend_init: yes selective
```

关键内容：

- Nomos/OpenClaw 边界：Nomos 只记路径/人物，不主动跨系统深处理。
- MiniMax 生图经验：手指错误高，hands out of frame / 虚化 / 隐藏 / 裁切常用。
- ChatGPT 生图：Claude skill 路径，订阅/Codex OAuth，generate-only，顺序生成。
- 模型路由：tk-x.ai OpenAI 兼容协议和 Claude/Anthropic Messages 协议区分。
- 工作区认知：AgentProject、AltmanCodex、Rubin-Studio 的边界。
- 项目接力规则：进入项目先读 AGENTS/CLAUDE，再读 CURRENT/MEMO。
- Brainstorm 规则：只产 md，不跑构建、不动代码/媒体、不删除；决定进 02，草稿进 03，完整方案进 01。
- OpenClaw 当前状态：无 cron 不等于宕机；EVE Buffer skill 和 key 路径只做路径记录。

## Hermes USER.md 关键条目

```yaml
source_path: /Users/robin/.hermes/memories/USER.md
status: active
recommend_init: maybe
```

关键内容：

- 不用“私人AI秘书”，用“AI秘书”或“Nomos”。
- 除非用户明确表示，否则不要检查或处理 OpenClaw；优先只处理 Hermes/Nomos 自己任务。
- 临时清单、快捷指令菜单、一次性说明默认不写 Brainstorm 03；重要方案/决策/可复用资产才落盘。
- Rovin/Nomos 社媒矩阵的发文口径和账号目标。

判断：Hermes USER 是高价值偏好源，但有些属于 Nomos 特定输出风格，不应变成所有 Agent 的全局规则。

## Hermes SOUL.md

```yaml
source_path: /Users/robin/.hermes/SOUL.md
memory_type: identity/judgment-rules
status: active for Hermes/Nomos
recommend_init: maybe
```

关键内容：

- Nomos 是判断中枢与矩阵秘书官。
- 目标服务内容增长、视觉资产、工程效率和最终变现。
- 高风险操作必须确认。
- 不知道就说不知道。
- 信息可能过期时必须标注。
- 工程任务考虑安全、回滚、日志和测试。
- 不鼓励无意义扩张 Agent 矩阵。

判断：其中“高风险确认、不编造、时效查证、服务变现”适合抽入初始化；Nomos 身份细节不适合作为所有 Agent 共识。

---

# OpenClaw Memory

## 总体结构

```yaml
source_system: openclaw
source_path: /Users/robin/.openclaw/
memory_type: workspace-memory + daily-notes + sessions + sqlite
status: active + noisy + historical
scope: agent/session/publishing/browser/social
recommend_init: maybe selective
```

结构：

- `~/.openclaw/memory/*.sqlite`：全局/Agent sqlite 记忆，未 dump。
- `~/.openclaw/workspace-*/MEMORY.md`：各 workspace 长期精选。
- `~/.openclaw/workspace-*/memory/YYYY-MM-DD.md`：短期日记/原始事件。
- `~/.openclaw/agents/*/sessions/sessions.json`：session 索引。
- `*.jsonl` / `.trajectory.jsonl`：会话轨迹，未导出全文。

## 重要 workspace

```yaml
source_path: /Users/robin/.openclaw/workspace-eve/
memory_type: workspace-memory/session/social/publishing
status: active + noisy
recommend_init: maybe
```

Eve 相关：

- `workspace-eve/MEMORY.md`：Eve 主线长期记忆。
- `workspace-eve/AGENTS.md`：先读 IDENTITY / USER / SOUL / TOOLS，近期 memory 按需读。
- `workspace-eve/TOOLS.md`：浏览器/发布工具规则。
- `memory/2026-06-17-buffer-api-handoff.md`：Buffer API 交接和只读测试边界。
- `agents/eve/sessions/sessions.json`：Eve session 索引，含 telegram/feishu/sessionId/skillsSnapshot 等。

## OpenClaw 自动晋升风险

发现：

- `workspace-eve/MEMORY.md` 出现 `Promoted From Short-Term Memory` / `openclaw-memory-promotion` 注释。
- 部分晋升内容可能含 Telegram metadata、模型切换、HEARTBEAT_OK 等噪音。
- `workspace-hunter/memory/dreaming/deep/2026-06-19.md` 有 ranked candidates / promoted candidates 痕迹，本次样本候选为 0。

判断：OpenClaw 的“自动记忆晋升”是有用机制，但质量需人工治理。适合借鉴候选→晋升流程，不适合全量导入。

---

# Mavis / MiniMax Memory

## 总体结构

```yaml
source_system: mavis-minimax
source_path: /Users/robin/.minimax/
memory_type: user-profile + tool-memory + skill-rules
status: active + sensitive-excluded
scope: tool/media/assistant
recommend_init: yes selective
```

关键路径：

- `/Users/robin/.minimax/memory/user.md`
- `/Users/robin/.minimax/agents/mavis/agent.md`
- `/Users/robin/.minimax/agents/general/memory/MEMORY.md`
- `/Users/robin/.claude/skills/minimax-token-plan/SKILL.md`
- `/Users/robin/.claude/skills/api-search-media-router/SKILL.md`
- `/Users/robin/.codex/skills/minimax-token-plan/SKILL.md`

## 有效内容

- Mavis 是轻量副手，能力外转交 Codex / Claude Code。
- MiniMax 是工具能力，不是默认自主 Agent。
- MiniMax 正式适用：TTS、BGM/音乐、视频理解、音频理解、STT。
- MiniMax 生图仅在 Robin 明确说“用 MiniMax 生图”或点名 `MiniMax image-01` 时使用。
- 未指定工具的生图优先 ChatGPT/Codex。
- 资讯搜索优先 api-search-media-router / Tavily / multi-search-engine 路线；MiniMax 搜索/识图非必要不用。
- MiniMax 生产调用走 Codex 端 `minimax-token-plan` / `mmx` CLI；Claude 端路由只保留健康检查/边界。

## 敏感风险

```yaml
status: sensitive-excluded
recommend_init: no
```

发现：

- `/Users/robin/.mmx/config.json` 存在完整 `api_key` 字段。
- `/Users/robin/.minimax/config.yaml` 存在明文或占位 API key 字段。

处理：不导入、不复制、不输出值。只在冲突/风险索引中记录“存在敏感配置，不得导入”。

---

# Brainstorm Memory

## 总体结构

```yaml
source_system: brainstorm
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/
memory_type: consensus + current + ideas + decisions + references
status: active + archive + draft
scope: global/project/content/agent-collaboration
recommend_init: yes
```

核心文件：

- `MEMO.md`：长期状态板。
- `CURRENT.md`：当前进度接力。
- `Global_Agent_Rules.md`：Brainstorm 全局规则。
- `CLAUDE.md` / `AGENTS.md`：端侧入口规则。
- `02_决定与共识/`：已拍板决定。
- `01_思路与方案/`：完整方案。
- `03_草稿与待定/`：草稿候选。
- `04_参考与引用/`：参考框架。

## MEMO.md 当前有效共识

```yaml
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
status: active
recommend_init: yes
```

关键内容：

- Robin 当前做的是 AI 内容工作室，不是 AI 频道套利。
- 锚点 = 变现；赛道服务变现。
- 小红书是主打变现阵地，Garfield2025 是 AI 视觉变现旗舰。
- 抖音是基本盘/涨粉阵地，ONCELUO2023 走 AI 信息差/判断口播。
- 艺斌视觉线 + 泽川 AI 判断线两手抓。
- 护城河 = 生产系统 × 判断 / 审美 / 人格 / 自有受众 / IP。
- Buffer 是国际线轻量分发层，不抢小红书/抖音/公众号主产能。
- 发布执行以 Hermes / OpenClaw 为主，Codex 负责生产、知晓链路，不默认抢发布。
- Claude 是架构端；Codex 是生产主端；Hermes 是较信任发布/自动化执行端；ChatGPT/MiniMax 是被调用工具。

## CURRENT.md 当前有效接力

```yaml
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md
status: active but time-sensitive
recommend_init: maybe
```

关键内容：

- 2026-06-19 已同步长期共识。
- 近期重点是社媒矩阵、AI 内容工作室、Buffer 国际线、Agent 分工。
- 生产前优先读 `05_题材池/INDEX.md`；高时效题材必须当天重查。
- RML / ROV_EVE 出图或视频 QC 执行压缩预览、一图一次、review md 结论。

判断：CURRENT 是时效接力，不应直接进入长期 core；可以抽取“当前仍有效”的视觉审查和题材池规则。

## Global_Agent_Rules.md

```yaml
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md
status: active for Brainstorm
recommend_init: yes selective
```

关键内容：

- Brainstorm 是思路沟通 + 方案沉淀，不是工程执行区。
- 只产 md，代码/脚本/媒体/最终交付物去对应项目。
- Claude / Codex 通过 md 共享，不直接通信。
- 同主题已有 md 就续写，不另起。
- 目录映射：01 思路方案、02 决定共识、03 草稿待定、04 参考引用、_System archive。
- 新会话靠 md 保持连续，不靠对话历史。

判断：适合进入初始化记忆。

## 02_决定与共识

```yaml
source_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/
status: active unless superseded
recommend_init: yes selective
```

高价值文件：

- `2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md`
- `2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md`
- `2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md`
- `2026-06-17_共识_Buffer国际线发布规划与API账号映射.md`
- `2026-06-17_Buffer社媒发布接入说明_全Agent与双电脑.md`
- `2026-06-11_审美CANON总纲_跨项目通用铁律_v1.md`
- `2026-06-11_全局工作流优化_AltmanCodex与双端配置升级计划.md`
- `2026-05-28_工程_Brainstorm空间初始化决定.md`

判断：这是当前最可靠的人类确认共识源，优先级高于各 Agent 自动 memory。

---

# 不建议导入初始化记忆的内容

## 高敏信息

```yaml
status: sensitive-excluded
recommend_init: no
```

类别：

- API Key。
- Token。
- Cookie。
- Account ID / Zone ID / Notebook ID 等认证或可关联账号信息。
- SSH 密码或连接方法中包含的认证字段。
- settings/config 原文。

路径类型：

- Claude AgentProject memory 中 credentials 类条目。
- MiniMax/mmx 配置。
- `.claude.json`、`settings.json`、Codex config。
- 搜索路由 `.env`。

## 自动记忆噪音

```yaml
status: candidate/noisy
recommend_init: no
```

类别：

- Telegram metadata。
- HEARTBEAT_OK。
- 模型切换临时片段。
- session id / trajectory id。
- 单次任务日志。
- rollouts 中已过期 cwd / thread / task。

## 过期/归档规则

```yaml
status: archived/deprecated
recommend_init: no
```

类别：

- plugin cache。
- `_workspace/research_raw` 外部仓库。
- `归档/过期文档`。
- EverOS pre-1.0 cloud plugin 旧 API 示例。
- 旧项目状态。

---

# 候选进入初始化记忆的内容池

建议进入 `01_initial-memory.md` 的内容：

1. Brainstorm 是跨 Agent 思路/共识空间，只产 md，不做工程生产。
2. Claude/Codex/Hermes/OpenClaw/Mavis 的原始记忆各有边界；`06_rubin-memory` 不复制原文，只做索引和精选。
3. Robin 当前总锚：AI 内容工作室，不是 AI 频道套利；锚点是变现。
4. Agent 共享共识必须来自人工确认文件或经过审核的候选，不来自自动总结直接晋升。
5. 敏感值永不进入普通记忆；只记录字段存在和安全边界。
6. MiniMax 是工具能力，主要用于 TTS/音乐/视频理解/STT；未点名时不默认用于搜索/识图/生图。
7. ChatGPT/Codex 生图优先；图片交付要能直接预览。
8. 视觉审查要压缩预览、一图一次、写 review/QC 结论。
9. 项目执行状态看项目 CURRENT/MEMO；跨项目思路和已拍板共识看 Brainstorm。
10. OpenClaw 自动晋升有用但含噪，不能无审核并入 core。
11. Hermes/Nomos 记忆适合作为判断/发布/矩阵规则参考，但不要把 Nomos 身份细节变成所有 Agent 规则。
12. Codex 自动 memory 适合抽取生产工作流和失败复盘，但历史任务状态默认不进初始化。
