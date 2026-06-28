# 2026-06-19_trigger-card-test

> 高频触发卡试点记录。目标：先处理 Robin 确认的真实高频：ChatGPT 生图、AltmanCodex 项目工作流、API 搜索聚合引擎；不把 baoyu 新部署 skill 群提前当高频。

## 新增触发卡

```yaml
trigger_cards:
  - 06_rubin-memory/triggers/chatgpt-imagegen.md
  - 06_rubin-memory/triggers/altmancodex-workflow.md
  - 06_rubin-memory/triggers/api-search-media-route.md
wrapper:
  - _workspace/scripts/rubin_memory.sh guard <name>
```

## Guard 输出测试

### chatgpt-imagegen

```text
触发：用户要生成/改图/封面/插画/母图/视觉资产，且未明确指定 MiniMax。
先查：rubin_memory.sh template minimax-usage；再读 chatgpt-imagegen SKILL 的 When to use / Production guardrail / Save-path policy。
必须：第一张就是真实任务图；prompt 写 aspect ratio，不写像素；输出必须有工作区 ASCII 预览图。
禁止：无关 test/smoke/sample/demo 图；同 prompt 多刷；自动写 CANON；把路径当最终交付不预览。
下一步：确认项目落点和预览路径 → 写/压缩引用图 → 调 chatgpt-imagegen → 展示预览 + 记录路径。
```

### altmancodex-workflow

```text
触发：任务路径在 /Users/robin/AltmanCodex，或 Robin 点名 AltmanCodex 子项目。
先查：最近 AGENTS.md → 同目录 CURRENT.md → 必要时 thread-handoff；涉及工具/多模态再查 api-search-media-route。
必须：不跨子项目读写；阶段点/暂停/完成更新 CURRENT.md；图片输出走工作区 ASCII 预览路径。
禁止：跳过 CURRENT.md；把 Brainstorm 当生产区；未确认写正式 Skill/Key/.env/shell/git push。
下一步：确认子项目与当前任务 → 读项目 AGENTS/CURRENT → 执行项目 workflow → 收尾回写 CURRENT。
```

### api-search-media-route

```text
触发：搜索/查资料/事实核查/找来源/国内平台搜索/视频音频理解/B-roll/图片搜索/生图工具选择。
先查：api-search-media-route SKILL 首屏路由表；高频记忆查 rubin_memory.sh template minimax-usage。
必须：资讯搜索优先；Tavily 走 Key 池 wrapper；高时效信息联网核查并列来源。
禁止：直接用 MiniMax 搜索替代普通搜索；把单 key tvly 失败当全局失败；输出完整 Key/Token/Cookie。
下一步：判定任务类型 → 选 Tavily/multi-search/MiniMax/chatgpt-imagegen/宿主视觉 → 执行后记录来源或产物。
```

## 索引测试

查询：

```bash
_workspace/scripts/rubin_memory.sh query "ChatGPT 生图 无关测试图 prompt 像素 CANON 预览" --scope visual --status active --top-k 5
```

Top 命中：

1. `triggers/chatgpt-imagegen.md` / `失败模式`
2. `triggers/chatgpt-imagegen.md` / `chatgpt-imagegen 触发卡`
3. `01_initial-memory.md` / `D3. 图片交付与视觉审查`

判断：通过。触发卡已进入索引，能比长 skill 更快召回门禁。

## 当前发现

1. ChatGPT 生图不是缺规则，是规则太长；触发卡能把最容易漏的 5 件事前置。
2. AltmanCodex 工作流的关键问题是层级多；触发卡强制先读最近 `AGENTS.md` 和同目录 `CURRENT.md`。
3. API 搜索聚合引擎结构相对健康；触发卡主要防止 Agent 绕过路由，直接用默认 web、MiniMax 或单 key `tvly`。
4. baoyu skill 群暂不纳入高频 guard；后续只做体检和首屏门禁建议。

## 下一步建议

1. 先让这 3 张卡跑 1–2 天。
2. 若有效，再考虑把 `rubin_memory.sh guard chatgpt-imagegen` 写进 ChatGPT 生图 skill 的最前面。
3. 若要接入 Codex 全局规则，只加一句短触发，不复制卡片全文，避免再次变长。
