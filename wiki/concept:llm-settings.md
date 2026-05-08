---
id: concept:llm-settings
type: concept
title: LLM settings — temperature, top_p, and friends
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-promptingguide-ai.md
  - raw/2026-05-09-anthropic-prompting-best-practices.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, foundational]
---

# LLM settings (the parameters that aren't your prompt)

## Summary

Most prompt failures aren't fixed by rewording — they're fixed by setting the right knobs. Six parameters do most of the work: **temperature, top_p, max length, stop sequences, frequency penalty, presence penalty**. Plus vendor-specific knobs like Claude's effort.

## The 6 universal settings

| Setting | What it controls | Typical use |
|---|---|---|
| **temperature** | Randomness. 0 = deterministic, higher = creative/varied | 0 for extraction/code; 0.7-1.0 for creative writing |
| **top_p** (nucleus) | Probability mass to sample from | Alternative to temperature; 0.9 typical |
| **max length** | Cap output tokens | Cost + latency control; ensure room for response |
| **stop sequences** | Strings that halt generation | Force end-of-output (e.g. `</answer>`) |
| **frequency penalty** | Discourage repeated tokens | Reduce literal repetition |
| **presence penalty** | Discourage repeated topics | Force topical variety |

## Claims

- "Recommend altering temperature OR top_p, not both." `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.95}`
- 0 temperature gives near-deterministic output (same prompt → same output, mostly). `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.85}`
- Higher temperature increases output diversity at cost of consistency. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.9}`
- For Claude 4.x, the **effort parameter** is a major lever distinct from temperature — it controls thinking budget. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- Effort levels: `low | medium | high | xhigh | max`. Default for intelligence-sensitive tasks: minimum `high`. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- "Claude Opus 4.7 respects effort levels strictly, especially at the low end." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`

## Decision guide

```
Need exact, deterministic output? → temperature: 0
Need creative variety? → temperature: 0.7-1.0
Output is being truncated? → raise max length
Output keeps repeating phrases? → frequency_penalty: 0.5
Output covers same topics? → presence_penalty: 0.5
Output stops too early? → adjust stop sequences
On Claude 4.x and reasoning is shallow? → raise effort to high/xhigh
```

## Vendor-specific extras

| Vendor | Extra setting | Notes |
|---|---|---|
| Claude 4.x | `effort` | low/medium/high/xhigh/max — see [[vendor:claude-effort]] |
| OpenAI | `response_format` | Force JSON mode |
| Gemini | `safety_settings` | Per-category content filters |

## Anti-patterns

- ❌ **Cranking temperature for "better quality"** — temperature controls *variety*, not quality. Higher temperature on extraction tasks usually hurts.
- ❌ **Using both temperature and top_p** — pick one. Combined behaviour is harder to reason about.
- ❌ **Ignoring max length** — silent truncation is a common bug. Always log/check.

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.9}`
- depends-on → [[setting:temperature]] `{conf: 0.95}`
- depends-on → [[setting:top-p]] `{conf: 0.9}`
- composes → [[vendor:claude-effort]] `{conf: 0.7}`

## Changelog

- 2026-05-09 — created
