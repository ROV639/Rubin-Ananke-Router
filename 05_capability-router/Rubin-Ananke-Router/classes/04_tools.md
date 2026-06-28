# 04 Tools｜工具调用

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

目标：让 Agent 选对工具，而不是乱造脚本或错用平台。

第一版 cards：
- `search-router`
- `chatgpt-imagegen`
- `minimax-media`
- `browser-control`
- `buffer-publish`

默认映射：
- 搜索：`api-search-media-route` / Tavily / multi-search
- 生图：宿主 `image_gen` / `chatgpt-imagegen` / `gpt-image-2`
- MiniMax：TTS / 音乐 / STT / 视频理解
- 浏览器：browser / chrome / computer use
- 发布：Hermes / OpenClaw / Buffer

边界：工具 API、模型参数、价格、额度都可能变化；执行前按 tool/card 要求复核。

