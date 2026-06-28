---
title: AltmanCodex Production Failure Audit
index_id: verification-altmancodex-production-failure-audit-2026-06-19
memory_layer: verification
status: active
scope:
  - project
  - ops
  - visual
  - content
  - tool
source_system:
  - codex
  - claude-code
  - rubin-memory
memory_type:
  - verification
  - failure
  - workflow
sensitivity: medium
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/AltmanCodex/
exclude_from_startup: true
---

# 2026-06-19 AltmanCodex 生产失败模式审计

> 目标：回应 Robin 的纠正——不要只从规则表面推断，要逐项目查看生产过程、CURRENT、learnings、QC / review / 复盘，找出“纠正很多次、生成很多版本仍不对”的真实痛点，并判断应该转成触发卡、项目 preflight、脚本硬门禁、引导式提问，还是 rubin-memory 候选记忆。

## 总判断

AltmanCodex 当前的主要问题不是“没有规则”，而是：

1. 生产失败已经被写进各项目 `CURRENT.md` / `.learnings/` / QC 文件，但下一次任务不会自动把这些失败变成前置检查。
2. 很多规则是描述型，不是可执行门禁；模型容易在做了一部分工具动作后，把“过程通过”包装成“成果完成”。
3. 引导式提问机制在 Claude Code CLI 更容易触发，因为 Claude 全局规则明确写了“模糊 / ≥3 步 / 宽指令先问一句”；Codex 端更偏“任务清晰就直接执行，歧义才输出计划”，没有同等硬度的“问 Robin”要求。
4. 高频失败集中在五类：身份 / 母图 / 角色连续性、脚本与事实先行、成品与过程件混淆、工具验证替代业务成果、外部请求 / 账号 / 浏览器状态失控。

## 项目频率与痛点排序

### 1. Eve_Frames：EVE 真人图、母图、锁脸

**已读关键源**

- `/Users/robin/AltmanCodex/Eve_Frames/CURRENT.md`
- `/Users/robin/AltmanCodex/Eve_Frames/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Eve_Frames/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Eve_Frames/2026-06-19_EVE_伦敦草地网球周_16x9/03_MOTHER_REVIEW_母图判断_v1.md`
- `/Users/robin/AltmanCodex/Eve_Frames/2026-06-19_EVE_伦敦草地网球周_16x9/11_QC_shot01_v6_identity_angle_estate_mother_candidate.md`

**重复失败模式**

- 纯文本生图或没有真实喂 `identity_ref`，导致 EVE 变成泛 AI 美女。
- 母图定义被反复误解：不是“漂亮场景图”，而是后续扩展的身份 / 构图 / 气质锚点。
- Agent 容易把明显不该问 Robin 的失败图拿去问，或误判为 KEEP。
- 世界杯 KV 中出现复制姿势、脸像贴上去、国家画面信息正确但运动广告能量不足等问题。
- 锁脸不是“脸一模一样贴过去”；Robin 要的是身份稳定 + 画面自然 + 场景成立。

**应转门禁**

- `trigger card`：人物 / EVE / portrait / face consistency 时强制调用锁脸模板。
- `script hard gate`：没有真实参考图输入时，不得进入“母图候选”。
- `project preflight`：每个母图候选必须声明“身份锚点、构图锚点、可扩展性、失败风险”。
- `rubin-memory candidate`：Robin 不接受“泛 AI 美女脸”替代 EVE；母图必须先服务后续扩展，不是单张好看。

### 2. Rubin_Motion_Lab：视频审美、字幕、上下文爆炸

**已读关键源**

- `/Users/robin/AltmanCodex/Rubin_Motion_Lab/CURRENT.md`

**重复失败模式**

- 心流、AI99、Konbini 等任务多轮后才暴露：画面粗糙、信息不足、字幕错误、节奏不成立。
- 字幕曾按场景平均切分，而不是以真实 TTS / SRT 时间轴驱动。
- 反复高分辨率读图导致上下文爆炸，视觉判断反而变弱。
- Agent 自我 QC 容易把技术通过误认为审美通过。

**应转门禁**

- `script hard gate`：字幕必须由真实 TTS / SRT 时间轴驱动，禁止平均切分。
- `project preflight`：视频任务先明确“信息密度、字幕策略、镜头审美、交付版本”。
- `trigger card`：看到“视觉审查 / AI99 / Konbini / 字幕 / 高分辨率图”时，优先读压缩预览和 review md。
- `rubin-memory candidate`：弱 vision 不能做最终审美裁判；高清图不能反复灌上下文。

### 3. Arthur_Observatory：公众号成品、事实核查、版式

**已读关键源**

- `/Users/robin/AltmanCodex/Arthur_Observatory/CURRENT.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/AGENTS.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/MEMO.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/.learnings/INDEX.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/_workspace/runs/2026-06-15_creator-platform-five/09_rework_diagnosis.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/_workspace/runs/2026-06-15_creator-platform-five/10_manual_layout_redesign.md`
- `/Users/robin/AltmanCodex/Arthur_Observatory/_workspace/runs/2026-06-15_creator-platform-five/11_manual_layout_v2_index.md`

**重复失败模式**

- 把过程件 / 纯文本稿误报成“公众号导入版 / 成品包”。
- Robin 说“事实依据 / 没按要求 / 太简单”后，Agent 第一反应是倒修 docx，而不是回到事实、结构、视觉规划阶段。
- `02_fact_sheet.md` 只有来源名或概述，缺 URL / 日期 / 来源性质 / 事实边界。
- 批量文章容易模板换皮：换配色、换组件顺序，但骨架一样。
- 图片生成后没有贴聊天预览、没有回填最终 docx、封面和正文插图混用。

**应转门禁**

- `project preflight`：每次声明当前处于 gzh-00 到 gzh-09 哪个阶段。
- `script hard gate`：docx 成品检查 `word/media/`、占位符、禁用文本、文件名、媒体数量；媒体为 0 禁止标“成品 / 发布版”。
- `trigger card`：Robin 说“事实依据 / 太简单 / 不算成品 / 千篇一律”时，禁止直接补 docx，先输出返工诊断表。
- `script hard gate`：事实表逐条必须有 URL、日期、来源性质、风险表达。
- `rubin-memory candidate`：Robin 不接受过程件冒充成品，不接受模板换皮，不接受路径式交付替代可预览结果。

### 4. Paper Moon：视频配音、字幕、混音、脚本

**已读关键源**

- `/Users/robin/AltmanCodex/Paper Moon/CURRENT.md`
- `/Users/robin/AltmanCodex/Paper Moon/AGENTS.md`
- `/Users/robin/AltmanCodex/Paper Moon/MEMO.md`
- `/Users/robin/AltmanCodex/Paper Moon/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Paper Moon/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/refs/429_BACKOFF_STANDARD.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/skills/book-podcast-video/steps/02-voice-tts.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/skills/book-podcast-video/steps/05-assemble-qc.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/runs/2026-06-10_youtube_Mo1A45ShcMo_dub/06_qc_delivery/QC_REPORT_v1.4.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/runs/2026-06-10_youtube_Mo1A45ShcMo_dub/06_qc_delivery/QC_REPORT_v2.0.md`
- `/Users/robin/AltmanCodex/Paper Moon/_workspace/runs/2026-06-07_meat_2min_voicefirst_v3/06_qc_v3_6/QC_REPORT.md`

**重复失败模式**

- 把“不剪原片中文配音”误走成“精华剪辑 / 摘要压缩”。
- 混音 QC 只看技术指标，未真正确认中文 TTS 与 BGM / SFX 同时可闻。
- 字幕位置、字号、第二字幕、遮挡原片图形多轮返工。
- 脚本阶段被低估，过早进入 TTS / 剪辑 / 字幕。
- 外部请求 429 处理临场发挥，容易把并发、请求体量、TPM、风控都误判成 RPM。
- MiniMax / 工具理解 PASS 被误当成审美或事实 PASS。
- TTS 不能靠 speed 凑时长，字幕时间轴必须以真实 TTS / SRT 为准。

**应转门禁**

- `trigger card`：触发“不剪原片 / 中文配音 / 消除原声”时，强制路由到 full-length-subtitle-dub，禁止摘要压缩逻辑。
- `script hard gate`：最终交付前导出同一时间段 voice / background / final_mix 三轨样本，并记录真听结论。
- `script hard gate`：`source_outline / selection_map / fact_check / essence_script / script_self_check` 全 PASS 前，禁止 TTS、镜头映射、字幕烧录。
- `project preflight`：外部请求默认单线程；首次 429 只允许一次小请求重试，第三次当日停止该域。
- `guided-question`：批量 TTS 前必须有 10s / 30s 试音确认或音色选择记录。

### 5. Rov_Eve：动漫角色连续性、漫画页、UI 信息层

**已读关键源**

- `/Users/robin/AltmanCodex/Rov_Eve/CURRENT.md`
- `/Users/robin/AltmanCodex/Rov_Eve/AGENTS.md`
- `/Users/robin/AltmanCodex/Rov_Eve/MEMO.md`
- `/Users/robin/AltmanCodex/Rov_Eve/.learnings/INDEX.md`
- `/Users/robin/AltmanCodex/Rov_Eve/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Rov_Eve/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Rov_Eve/_workspace/refs/ROVEVE_VISUAL_LESSONS_AND_PATTERNS.md`
- `/Users/robin/AltmanCodex/Rov_Eve/_workspace/refs/ROVEVE_FIXED_IMAGE_RULES_v1.md`
- `/Users/robin/AltmanCodex/Rov_Eve/_workspace/refs/ROVEVE_SERIES_AND_SELF_DIRECTOR.md`
- `/Users/robin/AltmanCodex/Rov_Eve/_workspace/workflows/ROVEVE_IMAGE_PRODUCTION_WORKFLOW_v1.md`
- `/Users/robin/AltmanCodex/Rov_Eve/2026-06-12_看脸vs看球_ROVxEVE_制作包/05_制作日志与QC.md`
- `/Users/robin/AltmanCodex/Rov_Eve/2026-06-13_看脸vs看球_ROVxEVE_重做包/05_制作日志与QC.md`
- `/Users/robin/AltmanCodex/Rov_Eve/2026-05-24_个人信息系统搭建_ROVxEVE_制作包/08_制作日志/01_制作日志.md`
- `/Users/robin/AltmanCodex/Rov_Eve/2026-05-24_个人信息系统搭建_ROVxEVE_制作包/09_Codex执行矩阵/01_Codex执行矩阵与母图计划.md`
- `/Users/robin/AltmanCodex/Rov_Eve/2026-05-24_个人信息系统搭建_ROVxEVE_制作包/05_执行规则与验收/04_常见返工原因.md`

**重复失败模式**

- 参考图 / 角色锁定失守，纯文母图导致角色不像、画风漂。
- 系列图模板感：只换标题、文案、服装，不换镜头、动作、UI 载体、教学点。
- 实体手机 / 平板 / 纸张 / 球员卡朝向混乱，画内角色使用和观众阅读没有分层。
- UI 信息层过重，像 PPT、软件截图、大 dashboard；或 HUD 拆太碎且信息太薄。
- 母图夜晚客厅、坐姿、室内关系锁死整组，导致时间 / 地点 / 动作没有变化。
- 体育 / 现实事件画面里事实逻辑没有先复盘，比分、队服、动作、立场互相打架。
- prompt 里写字号、百分比、像素尺寸，或后端尺寸不合规。

**应转门禁**

- `script hard gate`：正式母图前检查工具支持参考图，并真喂角色锁 / 风格参考；禁止纯文母图。
- `project preflight`：逐图变量矩阵必须填服装、场景、时间、机位、景别、动作、表情、视线、UI 载体、AI 帮助点；相邻图至少 4 项变化。
- `trigger card`：漫画页 / 分镜 / storyboard / 六格 / AI Slop 触发时，先产出漫画页分镜结构 + 逐场服装矩阵。
- `script hard gate`：prompt 扫描禁止 `px`、字号、百分比、`1080x1920` 等布局 / 后端尺寸噪音。
- `guided-question`：真实赛事 / 现实事件入画时，先问是否需要真实事实源、是否允许虚构。
- `rubin-memory candidate`：ROV×EVE 服装是 IP 资产；生活画面先成立，再叠透明信息层。

### 6. Web6 + Single：生产入口、事实核账、真实生效

**已读关键源**

- `/Users/robin/AltmanCodex/Web6/CURRENT.md`
- `/Users/robin/AltmanCodex/Web6/AGENTS.md`
- `/Users/robin/AltmanCodex/Web6/MEMO.md`
- `/Users/robin/AltmanCodex/Web6/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Web6/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Web6/_workspace/logs/2026-06-05_dual_site_self_review_v1.md`
- `/Users/robin/AltmanCodex/Web6/_workspace/logs/2026-06-17_IMAGEPIPE_rubin_buffer_publish_v1.md`
- `/Users/robin/AltmanCodex/Web6/_workspace/logs/2026-06-17_SKILL_social_media_image_publish_group_v1.md`
- `/Users/robin/AltmanCodex/Web6/_workspace/skills_ready/media-pipeline-maintenance/SKILL.md`
- `/Users/robin/AltmanCodex/Web6/_workspace/skills_ready/media-publish/SKILL.md`
- `/Users/robin/AltmanCodex/Web6/rrubin.cc.cd/AGENTS.md`
- `/Users/robin/AltmanCodex/Web6/rrubin.cc.cd/MEMO.md`
- `/Users/robin/AltmanCodex/Single/CURRENT.md`
- `/Users/robin/AltmanCodex/Single/AGENTS.md`
- `/Users/robin/AltmanCodex/Single/MEMO.md`
- `/Users/robin/AltmanCodex/Single/METHODOLOGY.md`
- `/Users/robin/AltmanCodex/Single/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/Single/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/Single/2026-06-09_报告准确度复盘/2026-06-09_报告_准确度命中率复盘_v1.md`
- `/Users/robin/AltmanCodex/Single/2026-06-19_信号更新/2026-06-19_信号_端午分化_v1.md`

**重复失败模式**

- 把配置 / 草案 / 开关存在误判为生产已生效。
- `enabled=True` 不等于 cron 点火；说明文档不等于真实入口已更新。
- 金融 / 市场报告存在行情口径错位、资金结构缺口、接口失败后继续追打等问题。
- Web6 多站点部署方式不同，曾出现生产分支 `master` 与默认 `main` 错配，preview 可用但生产域失败。
- 前端 UI 或文档完成但真实 API 未接，CLI 未暴露参数却做成控件。
- 图床 / Buffer 链缺请求级验证，可能把 HTML 错链或全局最新文件当成功结果。

**应转门禁**

- `script hard gate`：自动化 / cron / Agent 注入必须同时验证配置层、执行层日志或 `lastRunMs`、产出层来源。
- `script hard gate`：Pages / GitHub / Buffer 图床链路必须验证 production branch、部署成功、最终 URL `200 image/*`。
- `project preflight`：金融报告关键数据至少双源核对；失败或缺口显式标 `[待验证]` 或降权。
- `project preflight`：CLI / MCP / API 以真实 `--help`、schema 或样本为准，UI 不暴露未支持参数。
- `rubin-memory candidate`：配置存在不等于真实运行；预览可用不等于生产生效。

### 7. ONE_SPARK + MacBook_Rovin：社媒巡航、浏览器状态、Skill 引入

**已读关键源**

- `/Users/robin/AltmanCodex/ONE_SPARK/CURRENT.md`
- `/Users/robin/AltmanCodex/ONE_SPARK/AGENTS.md`
- `/Users/robin/AltmanCodex/ONE_SPARK/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/ONE_SPARK/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/ONE_SPARK/00_PROMPT_ONE_SPARK_第一批Skill制造_Codex启动任务_v1.md`
- `/Users/robin/AltmanCodex/ONE_SPARK/00_PROMPT_ONE_SPARK_Reference更新_Codex启动任务_v1.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/CURRENT.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/AGENTS.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/MEMO.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/.learnings/LEARNINGS.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/.learnings/ERRORS.md`
- `/Users/robin/AltmanCodex/MacBook_Rovin/03_MD_中文区Skills仓库中估与Rubin落地建议_v1.md`

**重复失败模式**

- 把浏览器 / 标签 / 按钮测试当进展；Robin 要的是账号数据、评论互动、候选动作、策略判断。
- 旧数据、历史截图、单屏读取、异常空页被包装成完整调研。
- 截图没显示、来源不明、多个来源冲突时，模型容易补全或误判。
- 只看播放 / 粉丝，漏评论、点赞、收藏、分享、主页访问、活跃粉丝。
- 浏览器标签过多、弹窗残留、Chrome 控制超时、辅助树不同步时仍继续操作。
- 发布、评论、点赞、收藏、关注、删除、改名、批量动作、登录态、Cookie、Key 都需要权限分层。
- 第三方 skill 容易被高估，不能按 README / star / 作者名直接引入生产。

**应转门禁**

- `project preflight`：任何巡航 / 调研报告必须列本轮实读证据、历史可核验证据、待核对字段、未读取原因。
- `script hard gate`：关键数据字段强制标 `public_page`、`creator_backend`、`user_input`、`tool_output`、`unknown`；`unknown` 不进入事实结论。
- `script hard gate`：社媒调研至少公开主页 + 创作者后台 / 数据中心双入口。
- `trigger card`：控制超时、标签过多、弹窗残留、辅助树与前台不一致时停止操作，先截图 / 记录 / 关闭异常标签或重开。
- `project preflight`：所有写入、发布、删除、互动、登录态、Cookie / Key、批量动作先输出动作清单和权限层。
- `rubin-memory candidate`：工具 smoke 不是业务进展；第三方 skill 入生产前必须 provenance / license / secret surface / 沙盒实测。

## 引导式提问机制为什么没触发

**找到的机制**

- `/Users/robin/.claude/CLAUDE.md`：Claude 端明确“任务 ≥3 步或要求模糊 → 先输出方案再执行”“宽指令反问‘你漏掉了什么？’”。
- `/Users/robin/.codex/AGENTS.md`：Codex 端更偏“任务清晰时直接执行，仅在多步骤、跨项目、有破坏风险、有歧义时输出计划”。未看到同等强度的“必须问 Robin”规则。
- `/Users/robin/AltmanCodex/Global_Agent_Rules.md`：有“先问一句”，但主要覆盖删除、覆盖、跨项目、写正式 Skill、外部 Skill 越级等破坏性或高风险动作。
- 多个项目 `CURRENT.md` 有“新选题先问是否看题材池”，但只覆盖新选题，不覆盖“主题已给但规格 / 风格 / 成功标准不清”。
- `Paper Moon/MEMO.md` 有“先问自己是继续旧任务、开新任务还是整理资产”，但不是必须问 Robin。

**失效原因**

1. Claude Code CLI 触发，是因为全局 Claude 规则更强调“模糊 / 宽指令 / 多步骤先问”。
2. Codex / AltmanCodex 的规则更强调“读 CURRENT、按项目工作流执行”，但没有把“需求澄清”做成硬门禁。
3. 项目规则中的 preflight 多半是工具链检查，不是“问清目标、成功标准、交付等级”。
4. 模型一旦认为任务“看起来明确”，就会直接执行；后续即使发现不清，也常用自我猜测补全。
5. 许多 skill / workflow 写了“先判断”，但没有要求在不确定时必须停下问 Robin。

**修复建议**

- 在 AltmanCodex 全局规则增加非破坏性澄清触发：新选题、创作方向不明、交付标准不明、跨工具选择、用户只说“继续 / 帮我做”。
- 在 Codex 端全局规则补齐 Claude 端的“宽指令反问”机制，并写明哪些场景必须等待确认。
- 把各项目 `CURRENT.md` 的“题材池规则”升级成统一模板：已给主题但缺平台、受众、长度、风格、成功标准时，只问 1 个关键问题。
- 把“先问自己”改成可执行门禁：若三选一无法从上下文判定，必须问 Robin，不得直接开工。

## 优先落地清单

### P0：立刻加短触发卡

1. `chatgpt-portrait-template`：人物 / 角色 / EVE / ROV / 锁脸 / 参考图时必须读人物模板。
2. `api-search-key-rotation`：Tavily / search 返回 401 / 403 / 429 / 432 时必须走 key 池 wrapper，不得判全局失败。
3. `altmancodex-guided-question`：用户给宽任务、继续任务、创作任务但缺平台 / 成功标准 / 交付等级时，先问 1 个关键问题。
4. `altmancodex-artifact-not-result`：工具 smoke / 过程件 / 路径 / 草案 / 开关存在，不等于成品或生产生效。
5. `altmancodex-image-identity-mother`：母图 / 角色 / 系列图必须先过身份锚点、参考图输入、变量矩阵。

### P1：项目 preflight

- Eve_Frames：母图候选表。
- Rubin_Motion_Lab：字幕时间轴 + 压缩预览 + 一图一次 review。
- Arthur_Observatory：gzh 阶段声明 + fact sheet 完整性。
- Paper Moon：任务路由声明：不剪原片 / 精华剪辑 / TTS / 字幕 / 混音。
- Rov_Eve：逐图变量矩阵 + prompt hygiene。
- Web6 / Single：生产生效验证 + 双源核账。
- ONE_SPARK：本轮实读证据 + 权限分层。

### P2：脚本硬门禁

- docx 成品媒体与占位符检查。
- prompt 尺寸 / 字号 / 百分比噪音检查。
- 图床 URL `200 image/*` 检查。
- 自动化真实运行状态检查。
- 事实表 URL / 日期 / 来源性质检查。
- TTS / SRT 时间轴检查。

## 结论

Robin 说“不要太表面”是对的。真实痛点不是再写一份长规则，而是把已经反复发生的失败变成短触发、项目 preflight、脚本硬门禁和必须停下问的问题。

下一步不应该先继续扩写长文档，而应该先把 P0 的 5 张短触发卡接入 `rubin_memory.sh guard`，并选 2 个项目做最小 preflight 样板：`Eve_Frames` 和 `Paper Moon`。
