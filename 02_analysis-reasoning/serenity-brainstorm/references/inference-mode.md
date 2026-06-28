# Inference Mode

Use this mode when the input is an account, message, event, rumor, thesis, or claim that already exists.

Goal: produce an evidence-backed judgment, not a style summary.

## Step Order

Run:

1. `steps/0-collect.md` when the target is an account, X thread, or live claim that needs raw source capture.
2. `steps/1-source.md`
3. `steps/2-verify.md`
4. `steps/3-filter.md`
5. `steps/4-argument.md`

Use `steps/5-language.md`, `steps/6-cadence.md`, `steps/7-interaction.md`, and `steps/8-monetize.md` only when the user asks to explain an account's full content machine or asks how to emulate it.

## Evidence Requirements

- X evidence and web evidence should be independent legs when the claim is high-stakes or current.
- For X: `x-grok-build xsearch` is acceptable only when it returns non-degraded results with usable X links.
- For web: use independent official, company, media, research, documentation, or database sources. Prefer first-party or primary sources when available.
- Record the date of current sources. Old data cannot prove a current claim without time context.

## Final Format

```text
结论：[一句话判断]

依据：
- [证据1，来源/时间]
- [证据2，来源/时间]
- [证据3，来源/时间]

交叉验证：
- claim：[验证状态：已证实/推断/未核实]
  来源三角：[X/live source] + [web/primary source] + [optional third source]
  冲突记录：[有/无；如有写明]
  反向证据：[查了什么；结果是什么]

confidence：已证实 / 推断 / 无法核实
disclosure：[利益相关、立场、工具限制；没有就写“无直接利益相关”]
```

Do not omit `交叉验证`, `confidence`, or `disclosure`.
