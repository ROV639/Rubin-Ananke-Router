# Bundle｜image-character-production

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 适用任务

EVE、ROVxEVE、ONE SPARK、人物图、封面、角色一致性、系列视觉。

## 装备卡组

```text
project-handoff
chatgpt-imagegen
eve-framework
rov-eve-framework
one-spark-framework
preview-qc-check
file-delivered-check
current-memory-writeback
```

## 执行顺序

1. 读项目接力和对应视觉框架。
2. 确认身份锁、比例、产物路径、是否授权生成。
3. 单张或母图先行，不批量盲跑。
4. 保存原图和预览副本。
5. 做压缩预览 QC。
6. 回写当前状态。

## 必须验证

图片必须真实存在并可预览；人物身份、比例、裁切、文字/水印禁区要检查。

## 回写要求

最终图路径、失败点和下一轮修改清单写入项目 `CURRENT.md` 或 QC 文档。
