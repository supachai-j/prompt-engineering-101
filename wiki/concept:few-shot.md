---
id: concept:few-shot
type: concept
title: Few-shot prompting (multi-shot)
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
tags: [deck, foundational]
---

# Few-shot prompting (multi-shot)

## Summary

**Few-shot** = include 2-5 demonstrations of input → output before the real task. It's the single highest-leverage technique after clarity. Show the model the *shape* of what you want, and it imitates the shape.

## Claims

- Few-shot presents "high-quality input-output demonstrations" before the task. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- "Choice of prompt format, training examples, and the order of the examples can lead to dramatically different performance." `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- Anthropic recommends positive examples over negative: "Positive examples showing how Claude can communicate with the appropriate level of concision tend to be more effective than negative examples or instructions that tell the model what not to do." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- Example selection matters — research-backed strategies include semantic similarity (k-NN), contrastive learning, uncertainty-based active learning. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- Example **order** matters — different permutations can dramatically change results. Same order can fail across different model sizes. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.9}`

## Anatomy of a few-shot prompt

```text
Classify customer messages as: BILLING, BUG, FEATURE, or OTHER.

Examples:
Input: "Your dashboard charges me twice every month."
Output: BILLING

Input: "When I click 'Save', the page reloads and loses my data."
Output: BUG

Input: "Could you add a dark mode option to the settings page?"
Output: FEATURE

Now classify:
Input: "I love the new logo!"
Output:
```

## Selection strategies (research-backed)

| Strategy | When to use | Source |
|---|---|---|
| **Semantic similarity (k-NN)** | Examples retrieved per-task | Liu et al., 2021 |
| **Diverse sampling** | Avoid clustering on one type | Su et al., 2022 (graph-based) |
| **Contrastive quality** | Filter low-quality examples | Rubin et al., 2022 |
| **Uncertainty-based** | Active learning loop | Diao et al., 2023 |

`[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`

## How many examples?

| Count | Use case |
|---|---|
| **0** (zero-shot) | Standard tasks, instruction-tuned model |
| **1** (one-shot) | Format demonstration, no edge cases |
| **2-5** (few-shot) | Most production prompts |
| **5-10** | Complex output format, edge cases matter |
| **>10** | Diminishing returns; consider [[concept:retrieval-augmented-generation]] or fine-tuning |

## Anti-patterns

- ❌ **All examples the same class** → model biased toward that class on ambiguous inputs
- ❌ **Bad example formatting** — model copies bugs (typos, broken JSON) literally
- ❌ **Too many examples** — wastes context, hits diminishing returns
- ❌ **Random order** — re-test if you reorder; results can shift surprisingly

## Bias gotchas (Zhao et al., 2021)

Three documented biases that few-shot prompts exhibit:
- **Majority label bias** — model favours frequent label across examples
- **Recency bias** — repeats final examples' labels
- **Common token bias** — prefers frequent tokens

**Mitigation:** balance label distribution + randomise example order across runs.

`[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`

## Relationships

- alternative-to → [[concept:zero-shot]] `{conf: 0.95}`
- composes → [[concept:chain-of-thought]] `{conf: 0.85}` (few-shot CoT)
- depends-on → [[concept:prompt-elements]] `{conf: 0.85}`
- mitigates → [[risk:format-drift]] `{conf: 0.9}`

## Changelog

- 2026-05-09 — created
