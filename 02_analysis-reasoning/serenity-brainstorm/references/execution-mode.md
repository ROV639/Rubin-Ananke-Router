# Execution Mode

Use this mode when the input is a production direction: copywriting, topic mining, content direction, packaging, or a publishing plan.

Goal: produce content or a topic list whose arguments have already passed verification. Do not write first and verify later.

## Step Order

Run:

1. `steps/1-source.md`
2. `steps/2-verify.md`
3. `steps/3-filter.md`
4. `steps/4-argument.md`
5. `steps/5-language.md`
6. `steps/6-cadence.md`
7. `steps/7-interaction.md`
8. `steps/8-monetize.md`

If the direction depends on live X discussion, use `steps/0-collect.md` first as upstream collection.

## Evidence Requirements

- Every publishable argument must have a verification path.
- Use X (`x-grok-build xsearch`) for live discussion and web (`api-search-media-route` / Tavily wrapper) for independent confirmation.
- If an argument is attractive but underverified, keep it as a question, hypothesis, or observation. Do not package it as a fact.
- For Robin's content system, route strong outputs back to the topic pool: verified signals may enter `05_题材池/01_信号池`; executable topics may enter the relevant board/platform pool after local rules are checked.

## Final Format

```text
选题/文案：[内容]
反差点：[为什么有反差，依据是什么]
证据：[具体例子/数据 + 可验证路径]
发布建议：[事件驱动/常规；窗口]
互动预案：[预判质疑 + 回应/留钩]
变现指向：[关注转化/产品引流/信任铺垫/其他]
```

If monetization direction is unclear, ask before producing final copy.
