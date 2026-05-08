---
id: concept:chain-of-thought
type: concept
title: Chain-of-Thought (CoT) prompting
status: active
confidence: 0.95
sources:
  - raw/2026-05-09-lilian-weng-prompt-engineering.md
  - raw/2026-05-09-anthropic-prompting-best-practices.md
  - raw/2026-05-09-promptingguide-ai.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, foundational, advanced]
---

# Chain-of-Thought (CoT)

## Summary

**Chain-of-Thought** asks the model to "think out loud" — generate intermediate reasoning steps before the final answer. For complex reasoning tasks (math, multi-step logic, analysis), CoT consistently improves accuracy. Two flavours: **zero-shot CoT** (a magic phrase) and **few-shot CoT** (worked examples).

## Claims

- CoT generates "short sentences to describe reasoning logics step by step" before the final answer. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- Two variants: **few-shot CoT** (manually-written reasoning chains) and **zero-shot CoT** (prompts like "Let's think step by step"). `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- "Benefits are most pronounced for complex reasoning tasks using large models (>50B parameters)." `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.9}`
- "Prompts with demonstrations of higher reasoning complexity can achieve better performance" (Fu et al., 2023). `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- Newlines outperform other separators in CoT prompts. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.8}`
- Modern Claude models have CoT-equivalent built into the **effort parameter** — manual CoT prompting still helps but the gap is smaller. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`

## Zero-shot CoT — the magic phrases

Append one of these to any prompt:

```text
Let's think step by step.
```
```text
Show your reasoning before answering.
```
```text
First, list the relevant facts. Then reason from them. Finally, state the answer.
```

For Claude models, you can also force structured thinking via XML:

```text
First, write your reasoning inside <thinking> tags.
Then write the final answer inside <answer> tags.
```

## Few-shot CoT — show, then ask

```text
Q: Roger has 5 tennis balls. He buys 2 more cans. Each can has 3 balls.
   How many balls does he have now?
A: Roger started with 5 balls. 2 cans × 3 balls = 6 new balls.
   5 + 6 = 11. Answer: 11.

Q: There are 23 apples in the cafeteria. They use 20 for lunch and buy 6 more.
   How many do they have?
A: Started with 23. Used 20: 23 − 20 = 3. Bought 6 more: 3 + 6 = 9. Answer: 9.

Q: A store has 12 widgets. It sells 7 and receives a shipment of 15.
A:
```

## Extensions of CoT

| Extension | Description | Source |
|---|---|---|
| **Self-consistency** | Sample multiple CoTs at temperature > 0; majority vote | Lilian Weng |
| **Self-Ask** | Iteratively ask follow-up questions before answering | Lilian Weng |
| **Tree of Thoughts** | Explore multiple reasoning paths in tree structure | Lilian Weng |
| **Complexity-based** | Prefer elaborate chains over short ones | Fu et al., 2023 |

## When CoT helps vs hurts

| Task type | CoT helps? |
|---|---|
| Math word problems | ✅ big improvement |
| Multi-step logic | ✅ big improvement |
| Code debugging | ✅ helps |
| Long-context analysis | ✅ helps |
| Simple classification | ⚠️ neutral or slight regression (overhead with no benefit) |
| Translation | ❌ no benefit |
| Pure recall ("what is the capital of France?") | ❌ no benefit |
| Creative writing | ⚠️ depends — may help structure, may dilute voice |

## Vendor-specific note: extended thinking

Modern Claude models have built-in thinking modes (the `effort` parameter or older `extended-thinking` API). When using these, **don't also prompt-CoT** — you get redundant chains. _(specific to: claude)_

`[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`

## Relationships

- extends → [[concept:zero-shot]] `{conf: 0.9}` (zero-shot CoT)
- extends → [[concept:few-shot]] `{conf: 0.9}` (few-shot CoT)
- composes → [[vendor:claude-effort]] `{conf: 0.85}`
- mitigates → [[risk:reasoning-shortcut]] `{conf: 0.85}`

## Changelog

- 2026-05-09 — created
