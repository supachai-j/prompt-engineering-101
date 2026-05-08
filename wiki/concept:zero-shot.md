---
id: concept:zero-shot
type: concept
title: Zero-shot prompting
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-lilian-weng-prompt-engineering.md
  - raw/2026-05-09-promptingguide-ai.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, foundational]
---

# Zero-shot prompting

## Summary

**Zero-shot** is the simplest baseline: give the model a task with **no examples**, just the instruction. It works surprisingly often because modern LLMs are heavily instruction-tuned. Always start here — if zero-shot solves your problem, you're done.

## Claims

- The simplest approach: "feed the task text to the model and ask for results" without demonstrations. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- Zero-shot performance has improved dramatically with instruction-tuned models (InstructGPT, Claude 4.x, GPT-4+). `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- Use zero-shot as the baseline against which other techniques are measured. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.85}`

## Examples

### Classification (zero-shot)

```text
Classify the sentiment as positive, negative, or neutral.

Text: "I really enjoyed the movie. The plot kept me guessing."

Sentiment:
```

Modern instruction-tuned models reliably output `positive` here without examples.

### Extraction (zero-shot)

```text
Extract the company name and amount from the text.
Output as JSON: {"company": string, "amount_usd": number}

Text: "Apple announced a $5B share buyback today."
```

### Generation (zero-shot)

```text
Write a one-sentence apology email for a missed deadline.
```

## When zero-shot is enough

- Task is well-known to the model (sentiment, summarisation, translation, classification)
- Output format is straightforward
- You have a strong instruction-tuned model
- No domain-specific vocabulary

## When zero-shot is NOT enough

→ Move to [[concept:few-shot]]:
- Output format is unusual or precise
- Task involves domain-specific reasoning
- You see ambiguous interpretations

→ Move to [[concept:chain-of-thought]]:
- Task requires multi-step reasoning
- Math, logic, or complex analysis

## Relationships

- alternative-to → [[concept:few-shot]] `{conf: 0.95}`
- extends ← [[concept:chain-of-thought]] (zero-shot CoT) `{conf: 0.9}`
- composes → [[concept:prompt-engineering]] `{conf: 0.9}`

## Changelog

- 2026-05-09 — created
