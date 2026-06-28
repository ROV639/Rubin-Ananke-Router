# 07 Verification Handoff｜验收与回写

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

目标：任务不是说完，而是交付、验证、接力。

第一版 cards：
- `file-delivered-check`
- `preview-qc-check`
- `publish-status-check`
- `current-memory-writeback`

覆盖：
- 是否真的生成文件
- 是否真的发布/排期
- 是否截图/预览/QC
- 是否更新 `CURRENT.md`
- 是否写入 memory inbox

边界：没有真实验证时，只能报告“未验证”或“部分完成”。

