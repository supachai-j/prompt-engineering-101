---
id: pattern:prompt-chaining
type: pattern
title: Prompt chaining
status: active
confidence: 0.85
sources:
  - raw/2026-05-09-anthropic-prompting-best-practices.md
  - raw/2026-05-09-promptingguide-ai.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: procedural
half_life_days: 180
tags: [deck, advanced]
---

# Prompt chaining

## Summary

**Prompt chaining** = break a complex task into multiple smaller prompts, where the output of step N becomes input to step N+1. The opposite of "stuff everything into one mega-prompt". Each step is simpler, easier to debug, and easier to evaluate.

## Claims

- "Break complex tasks into smaller prompts. Output of step 1 becomes input to step 2." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- Chaining is one of the standard advanced techniques covered by major guides. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.85}`

## When to chain

| Signal | Action |
|---|---|
| Task has clear sequential phases (research → outline → draft → polish) | Chain |
| Output is high-stakes and benefits from intermediate validation | Chain with validation step |
| Single prompt fits in context and works reliably | Don't chain — overhead |
| Steps need different model temperatures/effort levels | Chain (different settings per step) |
| You need different sub-agents/tools per step | Chain (each step a sub-call) |

## Example: research synthesis chain

```
┌─────────────────────────────────────┐
│ Step 1: Identify key claims         │
│   Input: long article               │
│   Output: list of factual claims    │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│ Step 2: Verify each claim           │
│   Input: claim from step 1          │
│   Output: verified | needs-citation │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│ Step 3: Synthesise verified claims  │
│   Input: only verified claims       │
│   Output: narrative summary         │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│ Step 4: Polish for audience         │
│   Input: summary + audience profile │
│   Output: final article             │
└─────────────────────────────────────┘
```

Each step has a focused prompt and a clear success criterion.

## Pattern: plan → validate → execute

A 3-step chain that's particularly valuable for risky operations:

1. **Plan** — model produces a structured plan (often as JSON or XML).
2. **Validate** — a separate prompt (or programmatic check) verifies the plan against constraints.
3. **Execute** — model runs the plan only after validation passes.

If validation fails, loop back to step 1 with the error feedback.

## When **not** to chain

- Simple tasks (zero-shot or single few-shot prompt suffices)
- Tasks where intermediate outputs are unstable (compounding errors)
- Latency-critical paths (each step adds round-trip)
- Cost-sensitive paths (each step is a separate API call)

## Compared to alternatives

| Approach | When better |
|---|---|
| **Single mega-prompt** | Task fits in context, no clear phases |
| **Prompt chaining** | Multi-phase task, each phase has clear i/o |
| **Tool use / function calling** | Steps need to call external systems |
| **Subagents** | Steps benefit from independent context windows |

## Anti-patterns

- ❌ **Chain when chaining isn't needed** — adds latency + cost for no benefit
- ❌ **Loose i/o between steps** — step 1 says "the answer is X (probably)" → step 2 doesn't know whether to trust it. Force structured output between steps.
- ❌ **No validation step** — chain executes garbage all the way through
- ❌ **Same model + temperature across all steps** — different steps may need different settings

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.85}`
- composes → [[pattern:structured-output]] `{conf: 0.9}` (force structure between steps)
- alternative-to → [[pattern:tool-use]] `{conf: 0.7}` (different mechanism for breaking down tasks)
- enables → [[pattern:evaluation-driven-prompting]] `{conf: 0.85}` (eval each step independently)

## Changelog

- 2026-05-09 — created
