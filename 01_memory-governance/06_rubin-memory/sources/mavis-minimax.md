# Mavis / MiniMax Memory Source Card

```yaml
source_system: mavis-minimax
root: /Users/robin/.minimax/
status: active-source
sensitivity: high around config
import_policy: tool boundary summaries only
```

## 结构

主要来源：

- `/Users/robin/.minimax/memory/user.md`
- `/Users/robin/.minimax/agents/mavis/agent.md`
- `/Users/robin/.minimax/agents/general/memory/MEMORY.md`
- `/Users/robin/.claude/skills/minimax-token-plan/SKILL.md`
- `/Users/robin/.codex/skills/minimax-token-plan/SKILL.md`
- `/Users/robin/.claude/skills/api-search-media-router/SKILL.md`

## 有用内容

- Mavis 是轻量副手。
- MiniMax 是工具能力，不是默认自主 Agent。
- MiniMax 适合 TTS、音乐/BGM、视频理解、音频理解、STT。
- MiniMax 生图必须明确点名。
- 搜索/识图非必要不用 MiniMax。

## 禁止导入

- `/Users/robin/.mmx/config.json` 的 `api_key` 值。
- `/Users/robin/.minimax/config.yaml` 的 API key 值。
- 任何账号认证值。

## 常见任务入口

- 判断是否用 MiniMax：读 `01_initial-memory.md` D1/D2 和 `30_retrieval-policy.md` MiniMax 查询模板。
- 语音/TTS/音乐/视频理解/STT：MiniMax 可主动考虑。
- 生图：只有 Robin 明确点名 MiniMax 或 `MiniMax image-01` 时使用。
- 搜索/识图：非必要不用 MiniMax，优先资讯搜索路由。

## 当前裁定

Mavis/MiniMax memory 只抽取工具边界和使用场景；配置和凭证信息一律排除。