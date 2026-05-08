---
id: concept:role-prompting
type: concept
title: Role prompting & system prompts
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-anthropic-prompting-best-practices.md
  - raw/2026-05-09-promptingguide-ai.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck]
---

# Role prompting & system prompts

## Summary

**Role prompting** = telling the model what *kind of expert* to be. **System prompts** are the API-level mechanism: a separate field that sets persona/role/constraints distinct from the user's message. Together they're how you turn a general-purpose model into a specialised assistant.

## Claims

- "Use the `system` parameter to set persona/role. Distinct from user message — affects how Claude approaches the entire conversation." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`
- System prompts persist across turns; user messages are the per-turn input. `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`
- Role prompting works with any model — even without a dedicated system field, prefacing the user message with "You are a senior product manager..." has measurable effect. `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.8}`

## Patterns that work

### Persona

```
You are a senior software engineer reviewing pull requests for a Python codebase.
Your team values clarity over cleverness, prefers simple solutions, and
uses pytest for testing. You give concise, actionable feedback.
```

### Constraints

```
You are a customer service agent for ACME Corp. You can:
- Answer product questions
- Handle return requests up to $500
- Escalate complex issues to a human

You cannot:
- Discuss competitors
- Make claims about products you can't verify
- Process refunds without an order number
```

### Output style

```
You are a technical writer. Your responses:
- Use active voice
- Avoid jargon unless necessary
- Default to bullet points over paragraphs
- Cite sources inline as [1], [2], etc.
```

## When system prompt vs user message?

| Goal | Where it goes |
|---|---|
| Persona / expertise | System |
| Constraints (what model can/cannot do) | System |
| Output format defaults | System |
| Per-task instructions | User message |
| The actual question/data | User message |
| Examples (few-shot) | Either — system if reused, user if task-specific |

## Anthropic's "Brilliant new employee" framing

> "Think of Claude as a brilliant but new employee who lacks context on your norms and workflows. The more precisely you explain what you want, the better the result."

`[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`

This translates directly into system-prompt design: tell the new hire who they are, what they care about, how your team operates.

## Anti-patterns

- ❌ **Role-as-magic-spell** — "You are an expert PhD researcher" with no concrete behaviour change. Stating expertise alone has small effect; concrete behaviour rules have large effect.
- ❌ **Conflicting personas** — "You are a friendly assistant. Also be ruthlessly objective." Pick one tone.
- ❌ **System prompt for per-task data** — putting variable input in system causes cache invalidation and is harder to debug.
- ❌ **Too long** — system prompts that bloat past ~1000 tokens often don't help proportionally.

## Vendor differences

| Vendor | System mechanism |
|---|---|
| Claude | `system` parameter in API |
| OpenAI | `messages: [{role: "system", ...}]` |
| Gemini | `systemInstruction` field |

Behaviour is similar across vendors but not identical — test before assuming portability.

## Relationships

- composes → [[concept:prompt-elements]] `{conf: 0.85}`
- enables → [[pattern:defensive-prompting]] `{conf: 0.85}`
- alternative-to → [[concept:few-shot]] `{conf: 0.5}` (different lever for shaping output)

## Changelog

- 2026-05-09 — created
