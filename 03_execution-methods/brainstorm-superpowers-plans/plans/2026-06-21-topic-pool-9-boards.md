# 题材池 9 板块重构与 topic-pool-refresh skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 把题材池从"两代结构并存、生成逻辑散在提示词"重构成"9 板块母题库（蓄水）+ 平台池（日更）+ 队列（周更）+ 反馈库"三层结构，并把每日刷新逻辑固化成单个跨 agent 一致的 Codex 端 skill。

**Architecture:** 板块（母题）= 内容真相源；账号 = 路由标签；项目 = 产线。母题库常青不日更，平台池引用母题 id 做账号化包装，队列周更。刷新逻辑收进单个 `topic-pool-refresh` skill（references 分层），一次编写多目录同步，任何 agent 调同名 skill 得到同一逻辑。真相源（板块定义、规则）留在 Brainstorm，skill 只引用不复制。

**Tech Stack:** Markdown（题材池文件）、Bash（skill 脚本/同步）、Codex 端 skill 体系（`~/.codex/skills/`、`~/.agents/skills/`、`~/.openclaw/.../skills/`）、`api-search-media-route` + `multi-search-engine` 搜索通道。

**执行边界:** 本计划由 Codex（生产主端）执行。Claude（架构端）已完成 P0（决定文档 + INDEX 重写 + 旧文件归档），本计划覆盖 P1-P3。所有路径基准目录 `/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/`。

---

## File Structure

**单一真相源（已存在，勿复制逻辑）:**
- `02_决定与共识/2026-06-21_决定_题材池9板块重构与topic-pool固化.md` — 9 板块 canon。
- `05_题材池/00_规则_题材池生成与筛选.md` — 规则/评分/字段。

**Create（母题库 9 板块骨架 + 信号/反馈层）:**
- `05_题材池/01_信号池_近7日.md`
- `05_题材池/11_母题库_B1_AI视觉变现实操.md` … `19_母题库_B9_前沿探索扩展.md`（9 个）
- `05_题材池/99_反馈库.md`

**Modify:**
- `05_题材池/00_规则_题材池生成与筛选.md` — 追加 §2 板块约束 + B9 维度 + 探索源通道分流。
- `05_题材池/20-23_平台池_*.md` — 条目改为引用母题 id（不再整段复制）。

**Create（skill）:**
- `~/.codex/skills/topic-pool-refresh/SKILL.md`
- `~/.codex/skills/topic-pool-refresh/references/{boards,rules,sources,fields}.md`
- `~/.codex/skills/topic-pool-refresh/scripts/sync_skill.sh`

**Archive（迁移后）:**
- `05_题材池/05_母题库_视觉资产与IP场景.md` → 内容并入 B4，原文件移 `_archive/`
- `05_题材池/06_母题库_普通人AI教程.md` → 并入 B3，移 `_archive/`
- `05_题材池/08_母题库_金融研究边界.md` → 并入 B8，移 `_archive/`

---

### Task 1: 追加 00_规则 的板块约束

**Files:**
- Modify: `05_题材池/00_规则_题材池生成与筛选.md`

- [ ] **Step 1: 在「## 5. 每条题材字段」的字段块顶部增加 `板块` 必填字段**

在 `- 上位共识对齐：...` 行之前插入：

```markdown
- 板块（必填，取自决定文档 B1-B9）：B1 AI视觉变现实操 / B2 AI判断与认知 / B3 普通人AI教程 / B4 EVE虚拟人像资产 / B5 ROV×EVE角色生活化 / B6 Motion动画解释 / B7 音乐歌词配音 / B8 公众号方法论与平台机制 / B9 前沿探索扩展
```

- [ ] **Step 2: 在「## 6. 评分标准」末尾追加 B9 维度说明**

在 `- <50：只留信号池。` 行后追加：

```markdown

### 6.1 B9 探索题材额外维度

B9（前沿探索）题材在 100 分主评分外，额外标注：

- 新颖度（1-5）：是否是当前矩阵从没做过的形态/工具/玩法。
- 可迁移性（1-5）：能否变成可复用形态并升级到 B1-B8。

B9 仍须过"大陆可用 + 用户问题"硬门禁；不能大陆落地的只留信号池，不转题材。
```

- [ ] **Step 3: 在「## 4. 搜索源标准」末尾追加探索源通道分流**

在 `## 5. 每条题材字段` 之前插入新小节：

```markdown
### 4.4 探索源通道（B9 专用）

B9 走英文前沿源，与 B1-B8 的大陆源分开：

- 入口：`api-search-media-route` / Tavily / 宿主 web 搜索。
- 站点：Reddit（r/artificial、r/StableDiffusion、r/aivideo、r/SideProject、r/InternetIsBeautiful）、X/Twitter AI 创作者、Hacker News、Product Hunt、前沿论坛。
- 海外信号必须经"产品化转译 + 大陆适配 + 国产工具平替"才能下放为题材，否则只进 `01_信号池_近7日.md`。
```

- [ ] **Step 4: 验证规则文件无占位符且板块约束已写入**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵'
/opt/homebrew/bin/rg -n '板块（必填|6.1 B9|4.4 探索源' 05_题材池/00_规则_题材池生成与筛选.md
```

Expected: 三行命中。

---

### Task 2: 建信号池与反馈库骨架

**Files:**
- Create: `05_题材池/01_信号池_近7日.md`
- Create: `05_题材池/99_反馈库.md`

- [ ] **Step 1: 写信号池骨架**

写入 `05_题材池/01_信号池_近7日.md`：

```markdown
# 信号池 · 近 7 日（L1）

> 角色：L0 原始报告/前沿源 → L1 信号。只存"发生了什么、证据强弱、有效期、可转译方向"，不是题材。
> 维护：Codex（每日刷新追加）。超 7 日条目移入 `_archive/信号池_YYYY-周.md`。
> 大陆源走 multi-search-engine；B9 前沿源走 api-search-media-route/Tavily。

## 字段

```text
- 信号标题：
- 来源文件/链接：
- 原始来源时间：
- 事实强度（1-5）：
- 有效期：
- 可转译方向（候选板块 B1-B9）：
- 数据缺口：
```

## 今日信号（YYYY-MM-DD）

（待 Codex 刷新填充；不能转译为大陆题材的信号停留在此，不进母题库/平台池。）
```

- [ ] **Step 2: 写反馈库骨架**

写入 `05_题材池/99_反馈库.md`：

```markdown
# 反馈库（L5）

> 角色：发布后数据回流。判断一条题材/板块是否放大、降权或淘汰。
> 维护：Codex（发布后补录）。

## 字段

```text
- 题材标题/板块：
- 账号/发布时间：
- 核心指标实测（收藏/完播/主页访问/转粉/私信等）：
- 对比预期：达标 / 低于预期 / 超预期
- 处理：放大同母题 / 降权 / 淘汰 / 改形态
- 回写母题库动作：
```

## 记录（倒序）

（待 Codex 发布后补录。）
```

- [ ] **Step 3: 验证两文件存在**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵'
ls 05_题材池/01_信号池_近7日.md 05_题材池/99_反馈库.md && echo ok
```

Expected: 两路径列出 + `ok`。

---

### Task 3: 建 9 个母题库板块骨架（含旧内容迁移）

**Files:**
- Create: `05_题材池/11_母题库_B1_AI视觉变现实操.md` … `19_母题库_B9_前沿探索扩展.md`
- Source（迁移）: `05_题材池/05_母题库_视觉资产与IP场景.md`（→ B4）、`06_母题库_普通人AI教程.md`（→ B3）、`08_母题库_金融研究边界.md`（→ B8 结构观察段）

- [ ] **Step 1: 为每个板块写统一骨架**

对 B1-B9，各建一个文件，套用以下模板（以 B1 为例，其余替换 id/名称/主供账号/产线/来源，取自决定文档 §2 表）：

```markdown
# 母题库 · B1 AI视觉变现实操

> 角色：蓄水池 · 真相源 · 常青（不日更，按需扩充）。
> 主供账号：Garfield2025 ｜ 产线：视觉工作流（即梦/剪映/醒图）｜ 来源通道：大陆站内+国产工具
> 板块定义见 `02_决定与共识/2026-06-21_决定_题材池9板块重构与topic-pool固化.md` §2。

## 常青母题（每条带评分，去重，唯一存放）

### <母题标题>

- 板块：B1 AI视觉变现实操
- 适配账号：Garfield2025
- 用户问题：
- 大陆适配（含合规/AIGC标识）：
- 国产工具路径：
- 护城河来源：
- 变现路径：
- 交付物：
- 评分：
- 失效条件：
- 母题 id：B1-001

（B9 文件额外在每条加 `新颖度(1-5)` 与 `可迁移性(1-5)`，并在抬头注明来源通道为 api-search-media-route/Tavily 前沿源。）
```

- [ ] **Step 2: 迁移旧母题库内容到对应新板块**

- `05_母题库_视觉资产与IP场景.md` 的题材并入 `14_母题库_B4_EVE虚拟人像资产.md`（角色/场景类）与 `11_母题库_B1...`（变现实操类），按内容归属拆分。
- `06_母题库_普通人AI教程.md` 的题材并入 `13_母题库_B3_普通人AI教程.md`。
- `08_母题库_金融研究边界.md` 的题材并入 `18_母题库_B8_公众号方法论与平台机制.md` 的"金融结构观察"段，并显式标注"不做操作建议"。
- 每条迁移后补 `母题 id`（如 B3-001）。

- [ ] **Step 3: 归档旧母题库文件**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/05_题材池'
for f in 05_母题库_视觉资产与IP场景.md 06_母题库_普通人AI教程.md 08_母题库_金融研究边界.md; do
  mv "$f" _archive/ && echo "archived: $f"
done
```

Expected: 三行 `archived:`。

- [ ] **Step 4: 验证 9 板块齐全且旧文件已归档**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/05_题材池'
ls 1[1-9]_母题库_B*.md | wc -l
ls _archive/0[568]_母题库_*.md 2>/dev/null | wc -l
```

Expected: 第一行 `9`；第二行 `3`。

---

### Task 4: 平台池条目改为引用母题 id

**Files:**
- Modify: `05_题材池/20_平台池_小红书_Garfield2025.md`、`21_平台池_抖音_ONCELUO2023.md`、`22_平台池_公众号_亚瑟的观测局.md`、`23_平台池_小红书_Rubin639.md`

- [ ] **Step 1: 在每个平台池文件抬头加引用约定**

在每个平台池文件的 `## 今日 N 个话题` 之前插入：

```markdown
> 引用约定：本文件是账号日更路由层。每条今日话题须引用一个母题 id（如 `母题：B1-003`），只写账号化包装（标题候选/承诺链/首屏钩子/核心指标），不整段复制母题正文。母题不存在时先在对应 `1X_母题库_*.md` 建条目再引用。
```

- [ ] **Step 2: 为每条现有今日话题补 `母题：BX-NNN` 行**

逐条在 `- 内容层级：` 行下方插入 `- 母题：BX-NNN`（按内容归到 §2 对应板块；母题库尚无则在 Task 3 文件补建后回填 id）。

- [ ] **Step 3: 验证平台池均含母题引用**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/05_题材池'
for f in 2[0-3]_平台池_*.md; do
  c=$(/opt/homebrew/bin/rg -c '母题：B[1-9]-' "$f"); echo "$f: $c"
done
```

Expected: 四个文件计数均 ≥1。

---

### Task 5: 建 topic-pool-refresh skill

**Files:**
- Create: `~/.codex/skills/topic-pool-refresh/SKILL.md`
- Create: `~/.codex/skills/topic-pool-refresh/references/boards.md`
- Create: `~/.codex/skills/topic-pool-refresh/references/rules.md`
- Create: `~/.codex/skills/topic-pool-refresh/references/sources.md`
- Create: `~/.codex/skills/topic-pool-refresh/references/fields.md`

- [ ] **Step 1: 用模板初始化 skill**

Run:

```bash
bash ~/.claude/skills/_templates/basic-skill/scripts/init_skill.sh topic-pool-refresh --with-references --with-scripts 2>/dev/null || mkdir -p ~/.codex/skills/topic-pool-refresh/{references,scripts}
echo ready
```

Expected: `ready`（或模板脚本成功输出）。

- [ ] **Step 2: 写 SKILL.md 主流程（< 500 行，渐进式）**

写入 `~/.codex/skills/topic-pool-refresh/SKILL.md`：

```markdown
---
name: topic-pool-refresh
description: Rubin Studio 题材池每日刷新。读上位共识→信号抽取→产品化转译→9板块评分→写母题库→路由平台池→生产队列。任何 agent 执行得到同一逻辑。
---

# topic-pool-refresh

> 真相源在 Brainstorm，不复制：
> - 板块 canon：`/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-21_决定_题材池9板块重构与topic-pool固化.md`
> - 规则：`05_题材池/00_规则_题材池生成与筛选.md`

## 何时用

每日刷新题材池，或任何 agent 需要把信号/前沿源转成合格题材时。

## 固定流程

1. 读 `MEMO.md` / `CURRENT.md` / 三份 6-17 共识 / 决定文档 / `00_规则`。
2. 信号抽取：大陆源走 multi-search-engine；B9 前沿源走 api-search-media-route/Tavily。结果写 `01_信号池_近7日.md`（只存信号，不是题材）。
3. 产品化转译：每条信号回答"能否转成大陆用户问题/匹配账号/国产工具完成/形成标题封面脚本"；不能则留信号池。详见 `references/fields.md`。
4. 评分（`references/rules.md` 100 分函数 + 硬淘汰）；定板块（`references/boards.md` B1-B9）。
5. 写母题库 `1X_母题库_BX_*.md`（常青、去重、带母题 id）。
6. 路由平台池 `2X_平台池_*.md`（引用母题 id + 账号化包装，不复制正文）。
7. 80+ 进 `90_生产队列_本周.md`。
8. 发布后数据回 `99_反馈库.md`。

## 边界

- 母题库不日更（蓄水）；平台池日更；队列周更。
- 不直接搬 Matrix/海外新闻当题材；金融不进视觉/教程板块。
- Brainstorm 只产 Markdown；不在此写代码/素材/成片。

## 渐进式加载

- `references/boards.md`：9 板块定义 + 账号路由 + 项目产线 + 来源通道。
- `references/rules.md`：评分函数、硬淘汰、入池阈值。
- `references/sources.md`：大陆源与 B9 前沿源查询模板。
- `references/fields.md`：题材卡字段模板 + 正确/错误转译示例。
```

- [ ] **Step 3: 写 references/boards.md（从决定文档 §2 同步 9 板块表）**

写入 `~/.codex/skills/topic-pool-refresh/references/boards.md`：把决定文档 §2 的 9 板块表（id/板块/装什么/主供账号/对应项目/来源通道）+ §2.1 喂料源 + §3 B9 专项规则原样落入；抬头标 `> 同步自决定文档 §2，改动回决定文档改，不在此改`。

- [ ] **Step 4: 写 references/rules.md / sources.md / fields.md**

- `rules.md`：从 `00_规则` §6 评分表 + §7 硬淘汰 + §6.1 B9 维度同步。
- `sources.md`：从 `00_规则` §4 搜索源 + §4.4 探索源通道同步，列大陆源与前沿源查询模板。
- `fields.md`：从 `00_规则` §5 字段块 + 决定文档 B9 额外维度，附 1 个正确/错误转译对照例（沿用方案文档 §9 的 OpenAI spend controls 例）。

- [ ] **Step 5: 验证 skill 结构与行数**

Run:

```bash
ls ~/.codex/skills/topic-pool-refresh/SKILL.md ~/.codex/skills/topic-pool-refresh/references/{boards,rules,sources,fields}.md
wc -l ~/.codex/skills/topic-pool-refresh/SKILL.md
```

Expected: 5 文件存在；SKILL.md 行数 < 500。

---

### Task 6: 多目录同步 skill（跨 agent 一致）

**Files:**
- Create: `~/.codex/skills/topic-pool-refresh/scripts/sync_skill.sh`

- [ ] **Step 1: 写同步脚本**

写入 `~/.codex/skills/topic-pool-refresh/scripts/sync_skill.sh`：

```bash
#!/usr/bin/env bash
set -euo pipefail
SRC="$HOME/.codex/skills/topic-pool-refresh"
TARGETS=(
  "$HOME/.agents/skills/topic-pool-refresh"
  "$HOME/.openclaw/workspace/skills/topic-pool-refresh"
)
for dst in "${TARGETS[@]}"; do
  mkdir -p "$dst"
  rsync -a --delete "$SRC/" "$dst/"
  echo "synced -> $dst"
done
```

- [ ] **Step 2: 赋权并试运行（仅存在的目标目录）**

Run:

```bash
chmod +x ~/.codex/skills/topic-pool-refresh/scripts/sync_skill.sh
bash ~/.codex/skills/topic-pool-refresh/scripts/sync_skill.sh || echo "目标目录缺失，按实际 agent skill 目录调整 TARGETS 后重跑"
```

Expected: 每个存在的目标打印 `synced -> ...`；缺失目录提示调整。

- [ ] **Step 3: 验证至少一个目标同步成功**

Run:

```bash
ls ~/.agents/skills/topic-pool-refresh/SKILL.md 2>/dev/null && echo agents-ok || echo "agents 目录待确认"
```

Expected: `agents-ok` 或明确提示待确认（不阻塞，记录在交付报告）。

---

### Task 7: 停用/改造 Hermes/Nomos 09:00 旧任务

**Files:**
- 说明性任务（Hermes 端配置，Codex 协调，不在 Brainstorm 改文件）。

- [ ] **Step 1: 定位旧定时任务**

Run:

```bash
ls ~/.hermes 2>/dev/null && /opt/homebrew/bin/rg -l -i '题材池|topic|09:00|题材' ~/.hermes 2>/dev/null | head
```

Expected: 列出疑似定时任务/提示词文件（若无则记录"未在本机找到，需 Hermes 端确认"）。

- [ ] **Step 2: 改造原则（写入交付报告，不擅自改 Hermes 配置）**

记录处理方案：旧 09:00 任务停止"直接写题材池"，改为只调 `topic-pool-refresh` 的信号抽取段产 `01_信号池_近7日.md`；题材化与板块写入由 Codex 主导。是否改 Hermes 配置须 Robin 确认（高风险跨端动作）。

- [ ] **Step 3: 在 CURRENT 记录待办**

在 `CURRENT.md` 触发记录追加一行，注明 Hermes 旧任务待停用/改造，指向本决定文档。

---

### Task 8: 收尾更新 INDEX 与 CURRENT

**Files:**
- Modify: `05_题材池/INDEX.md`、`CURRENT.md`

- [ ] **Step 1: 更新 INDEX 母题库段为 9 板块实际文件**

把 INDEX「母题库」段的"扩充中"占位替换为 `11_…B1` 至 `19_…B9` 实际文件列表，并加 `01_信号池` / `99_反馈库` 两行。

- [ ] **Step 2: CURRENT 追加完成记录**

在 `CURRENT.md` 触发记录顶部加一行，说明 9 板块骨架 + skill 已建、Hermes 旧任务待处理，指向决定文档。

- [ ] **Step 3: 终验**

Run:

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/05_题材池'
echo "母题库板块:"; ls 1[1-9]_母题库_B*.md | wc -l
echo "信号/反馈:"; ls 01_信号池_近7日.md 99_反馈库.md
echo "skill:"; ls ~/.codex/skills/topic-pool-refresh/SKILL.md
echo "旧母题库归档:"; ls _archive/0[568]_母题库_*.md | wc -l
```

Expected: 板块 `9`；信号/反馈两文件在；skill 在；归档 `3`。

---

## Self-Review

**Spec coverage（对照决定文档）:**
- §2 9 板块 → Task 3（建骨架）+ Task 1（规则约束）。
- §2.1 喂料源（Single/ONE_SPARK）→ Task 3 Step 2（金融并入 B8）+ boards.md。
- §3 B9 专项（前沿源/转译门禁/额外维度）→ Task 1 Step 2/3 + Task 5 boards/sources。
- §5 skill 固化 + 多目录同步 → Task 5 + Task 6。
- §6 单一 owner + Hermes 改造 → Task 7。
- §7 目标目录（信号池/反馈库/母题库/归档）→ Task 2 + Task 3 + Task 8。

**Placeholder scan:** 无 `TBD/TODO/待补/implement later`；母题正文留空是设计（蓄水池待 Codex 填真实题材），已在骨架注明。

**一致性:** 母题 id 命名 `BX-NNN` 在 Task 3/4 一致；文件编号 `1X_母题库_BX_*` 在 Task 3/8 一致；skill 名 `topic-pool-refresh` 全程一致。

**已知非阻塞项:** Task 6/7 依赖其它 agent 的 skill 目录与 Hermes 配置，按实际环境调整，结果写交付报告，不擅改高风险跨端配置。
