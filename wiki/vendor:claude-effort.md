---
id: vendor:claude-effort
type: vendor-feature
title: Claude effort parameter (vendor-specific)
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-anthropic-prompting-best-practices.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 90
tags: [vendor]
---

# Claude `effort` parameter _(specific to: claude 4.x)_

## Summary

Claude 4.x exposes an `effort` parameter (5 levels: `low | medium | high | xhigh | max`) that tunes how much the model "thinks" before responding. It's the vendor-specific equivalent of cranking [[concept:chain-of-thought]] / extended thinking. **Marked vendor-specific** — has no portable equivalent.

## Levels

| Level | When to use |
|---|---|
| `max` | Hardest reasoning tasks; can over-think; diminishing returns |
| `xhigh` | **Coding & agentic work** (Anthropic recommendation) |
| `high` | **Default for intelligence-sensitive tasks** (Anthropic recommendation) |
| `medium` | Cost-sensitive; trades intelligence for tokens |
| `low` | Latency-sensitive, scoped tasks; not for complex reasoning |

`[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`

## Claims

- "Claude Opus 4.7 respects effort levels strictly, especially at the low end." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- At `low` and `medium`, the model "scopes its work to what was asked rather than going above and beyond." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- "If you observe shallow reasoning on complex problems, raise effort to high or xhigh rather than prompting around it." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- "The triggering behavior for adaptive thinking is steerable" via prompt — but raising effort is the cleaner lever. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`
- For `max` or `xhigh`: "set a large max output token budget so the model has room to think and act across its subagents and tool calls. We recommend starting at 64k tokens." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`

## Decision guide

```
Coding agent / autonomous work          → xhigh
Code review with deep analysis           → xhigh or high
Complex multi-step reasoning             → high (or xhigh)
Standard production prompts              → high (default)
Latency-critical extraction/classify    → medium
Quick formatting / small transforms     → low
```

## Cross-vendor mapping

| Goal | Claude | OpenAI | Gemini |
|---|---|---|---|
| Force more reasoning | `effort: high` or `xhigh` | Append CoT instruction; or use o1/o3 reasoning models | Use thinking-enabled models; explicit CoT prompt |
| Reduce reasoning | `effort: low` | (n/a directly) — shorter prompts; no CoT | (n/a directly) |

There's **no direct cross-vendor equivalent** of effort levels. OpenAI and Google handle this via separate model families (o1, Gemini Thinking) rather than a parameter.

## Anti-patterns

- ❌ **Default to `low` to save tokens** — under-thinking on complex tasks; correctness matters more than speed
- ❌ **Use `max` always for "best quality"** — diminishing returns; sometimes over-thinks and degrades
- ❌ **Mix effort with explicit CoT prompting** — redundant; pick one mechanism
- ❌ **Forget output token budget** — at `xhigh`/`max`, default budgets often truncate

## Relationships

- composes → [[concept:llm-settings]] `{conf: 0.9}`
- vendor-equivalent-of → [[concept:chain-of-thought]] `{conf: 0.7}` _(approximate; not direct equivalent)_

## Changelog

- 2026-05-09 — created
