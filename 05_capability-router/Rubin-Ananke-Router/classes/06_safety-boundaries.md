# 06 Safety Boundaries｜安全与边界

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

目标：防止 Agent 自动做不可逆或高风险动作。

第一版 cards：
- `secret-redaction`
- `delete-overwrite-confirm`
- `publish-confirm`
- `payment-remote-confirm`

覆盖：
- key / token / cookie / 私钥
- 删除 / 覆盖
- 发布 / 排期
- 付款
- 远程操作
- 敏感信息脱敏

边界：用户一次确认只覆盖当次明确动作，不自动扩展到相邻动作。

