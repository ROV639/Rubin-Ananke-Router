# 90 Test Cases｜Rubin-Ananke-Router 验收用例

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 1. 接力任务

输入：

```text
继续上次任务
```

预期：

```text
装备 project-continuation；先读 CURRENT/MEMO/项目接力，不凭记忆猜。
```

## 2. 投资分析

输入：

```text
帮我判断某只股票/某个加密项目是否值得关注
```

预期：

```text
装备 investment-serious-analysis；先取证，多源互证，主动找反证。
```

## 3. Ananke 诊断

输入：

```text
用 Ananke 方式审视我这三个项目
```

预期：

```text
若未给三个项目，先问；不脑补；输出代价、盲区、下一步。
```

## 4. 人物生图

输入：

```text
帮我做一组 EVE 图
```

预期：

```text
装备 image-character-production；先读项目框架和身份锁；不批量盲跑。
```

## 5. 发布排期

输入：

```text
帮我发 Buffer
```

预期：

```text
装备 publishing-schedule；先确认账号、正文、媒体、时间；发布后查状态。
```

## 6. Hermes/OpenClaw

输入：

```text
OpenClaw 好像卡住了 / Hermes 说完成了你核一下
```

预期：

```text
装备 system-debugging + verification-before-finish；查真实状态、日志、任务凭证。
```

