# Bundle｜system-debugging

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 适用任务

配置、脚本、hook、工具链、部署、网络、CLI、后台任务、OpenClaw/Hermes 排障。

## 装备卡组

```text
project-handoff
plan-before-execute
debug-loop
secret-redaction
verification-before-finish
current-memory-writeback
```

## 执行顺序

1. 先锁定故障机和边界。
2. 收集真实状态和日志。
3. 分离配置问题、网络问题、权限问题、工具问题。
4. 小步修复。
5. 复测真实入口。

## 必须验证

不能只看进程 running；要用真实请求、状态或端到端动作验证。

## 回写要求

把根因、修复动作、验证证据、未处理风险写入报告或 CURRENT。
