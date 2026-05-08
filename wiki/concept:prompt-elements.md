---
id: concept:prompt-elements
type: concept
title: Elements of a prompt
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

# Elements of a prompt

## Summary

A well-structured prompt has up to **four elements**: instruction, context, input data, and output indicator. Not every prompt needs all four — but knowing them helps you debug what's missing when output disappoints.

## The four elements

```
┌───────────────────────────────────────────────┐
│ INSTRUCTION                                    │  ← what to do
│ "Translate the following text to French..."   │
├───────────────────────────────────────────────┤
│ CONTEXT                                        │  ← background that informs
│ "The text is from a children's book; keep    │
│  language simple and warm."                   │
├───────────────────────────────────────────────┤
│ INPUT DATA                                     │  ← actual content to act on
│ <text>The cat sat on the mat.</text>          │
├───────────────────────────────────────────────┤
│ OUTPUT INDICATOR                               │  ← format / structure
│ "Output as JSON: { 'fr': string }"            │
└───────────────────────────────────────────────┘
```

## Claims

- A prompt may contain four elements: **instruction, context, input data, output indicator**. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.95}`
- Not all elements required for every prompt. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.9}`
- Anthropic's "golden rule": "Show your prompt to a colleague with minimal context. If they'd be confused, Claude will be too." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- Be specific about the desired output format and constraints. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- Sequential numbered/bulleted steps when order matters. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- Providing context or motivation behind instructions can help models deliver more targeted responses. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`

## Anti-pattern: bare instruction

A prompt with only the instruction often underperforms because the model fills missing context with priors:

```text
❌ Bare:  "Summarise this."
✅ Full:  "Summarise the following customer review for our weekly report.
          Focus on actionable feedback. Output 3 bullets, ≤15 words each.
          <review>{{text}}</review>"
```

The full version specifies instruction (summarise), context (weekly report, actionable), input data (review), output indicator (3 bullets ≤15 words).

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.95}`
- depends-on → [[concept:llm-settings]] `{conf: 0.7}`
- composes → [[pattern:structured-output]] `{conf: 0.85}`

## Changelog

- 2026-05-09 — created
