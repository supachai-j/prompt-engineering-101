---
id: concept:prompt-engineering
type: concept
title: Prompt Engineering — definition + scope
status: active
confidence: 0.95
sources:
  - raw/2026-05-09-promptingguide-ai.md
  - raw/2026-05-09-lilian-weng-prompt-engineering.md
  - raw/2026-05-09-anthropic-prompt-engineering-overview.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, foundational]
---

# Prompt Engineering

## Summary

**Prompt engineering** is the discipline of communicating with LLMs to steer their behaviour without updating model weights. It's a *weights-frozen* practice: you change inputs, not parameters. The lever you pull is text (and adjacent control like settings + examples), and the goal is reliable, evaluable outputs.

## Claims

- "Methods for how to communicate with LLM to steer its behavior for desired outcomes without updating the model weights." `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- "Prompt engineering is a relatively new discipline for developing and optimizing prompts to efficiently apply and build with large language models." `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.9}`
- The discipline serves two audiences — researchers (LLM safety + performance) and developers (interfacing with LLMs and tools). `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.9}`
- Prompt engineering has 3 prerequisites before it's a useful tool: clear success criteria, ways to test against them, a first-draft prompt. `[src: raw/2026-05-09-anthropic-prompt-engineering-overview.md] {conf: 0.95}`
- Not every problem is a prompt-engineering problem — latency and cost are often better solved by model selection. `[src: raw/2026-05-09-anthropic-prompt-engineering-overview.md] {conf: 0.9}`

## When prompt engineering is the right lever

| Problem | Prompt engineering helps? |
|---|---|
| Output format wrong | ✅ yes |
| Output quality / accuracy | ✅ usually |
| Behaviour / persona drift | ✅ yes |
| Latency too high | ❌ pick a smaller/faster model |
| Cost too high | ❌ change model or architecture |
| Knowledge cutoff | ❌ use RAG (retrieval) |
| Domain expertise lacking | partly — combine with [[concept:retrieval-augmented-generation]] or fine-tuning |

## Relationships

- composes → [[concept:prompt-elements]] `{conf: 0.95}`
- enables → [[pattern:evaluation-driven-prompting]] `{conf: 0.9}`
- alternative-to → [[concept:fine-tuning]] `{conf: 0.7}` _(out of scope but referenced)_

## Open questions

- [ ] Where does the line between "good prompt" and "prompt engineering" actually fall? Is there a maturity spectrum worth documenting?

## Changelog

- 2026-05-09 — created
