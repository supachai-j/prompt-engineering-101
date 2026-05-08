---
id: pattern:tool-use
type: pattern
title: Tool use / function calling
status: active
confidence: 0.85
sources:
  - raw/2026-05-09-lilian-weng-prompt-engineering.md
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

# Tool use / function calling

## Summary

**Tool use** lets a model decide *when to call an external function* and what arguments to pass. Instead of the model trying to do everything in text (and hallucinating), it punts deterministic operations (arithmetic, web search, DB query, sending email) to real code. The vendors call this "tool use" (Anthropic), "function calling" (OpenAI), or "function declarations" (Gemini) — the pattern is the same.

## Claims

- "TALM and Toolformer enable structured API calls through special tokens." `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- Tool ecosystems documented include: calculators, Q&A systems, search engines, translation systems, calendars. `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- "Claude Opus 4.7 has a tendency to use tools less often than Claude Opus 4.6 and to use reasoning more." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`
- "For scenarios where you want more tool use, you can also adjust your prompt to explicitly instruct the model about when and how to properly use its tools." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`

## The pattern

```
1. You define tools with name, description, parameter schema
2. You send the user's request + tool list to the model
3. Model responds either with:
   (a) a normal text answer (didn't need tools), OR
   (b) a tool call: {"name": "search_db", "arguments": {"query": "..."}}
4. Your code executes the tool with those arguments
5. You send the result back to the model
6. Model produces the final text answer (or another tool call)
```

The loop continues until the model produces a final answer.

## Tool definition (vendor-neutral shape)

```json
{
  "name": "lookup_customer",
  "description": "Look up customer details by ID. Use when user asks about a specific customer.",
  "parameters": {
    "type": "object",
    "properties": {
      "customer_id": {
        "type": "string",
        "description": "The unique customer identifier (format: CUS-12345)"
      }
    },
    "required": ["customer_id"]
  }
}
```

The **description** is what the model uses to decide when to call. Treat it like a [[concept:prompt-engineering]] task in itself.

## Description rules (the "trigger" of tool use)

Same rules as for skills (see Anthropic's `description optimization` guidance):
- **Third person, imperative.** "Looks up customer..." not "I look up..."
- **What + when.** "Use when user asks about a specific customer."
- **Concrete trigger phrases.** "Use when user mentions an order number, ticket ID, or customer ID."

A vague description ("does customer stuff") = unreliable triggering.

## When to use tools vs prompt-only

| Need | Tool use | Prompt-only |
|---|---|---|
| Fresh data (DB, API) | ✅ | ❌ |
| Deterministic computation | ✅ | ❌ (model arithmetic is unreliable) |
| External actions (send email, file ticket) | ✅ | ❌ |
| Pattern matching / classification | ❌ | ✅ |
| Reasoning / synthesis | ❌ | ✅ |
| Generating creative output | ❌ | ✅ |

## Anti-patterns

- ❌ **Vague tool descriptions** → model picks wrong tool or misses opportunities
- ❌ **Too many tools (>20)** → model overwhelmed; degrades selection quality
- ❌ **Tools with overlapping purpose** → model picks the easier one even when the other is better
- ❌ **Letting the model invent arguments** → strict parameter schemas + validation
- ❌ **Forgetting the schema** → free-form arg dict = parser breakage
- ❌ **No bound on the loop** → runaway tool calls. Cap iterations.

## Vendor differences

| Vendor | Mechanism | Notes |
|---|---|---|
| Anthropic Claude | `tools` parameter, `tool_use` content block | Maps cleanly to MCP |
| OpenAI GPT | `tools` parameter, `function_call` in response | Older `functions` API deprecated |
| Gemini | `tools` with `functionDeclarations` | Similar shape |
| MCP | Universal protocol — see [[concept:mcp]] (out of scope but referenced) | Cross-vendor |

## Composition with other patterns

- Tool use + [[pattern:prompt-chaining]] = orchestrated agents
- Tool use + [[concept:retrieval-augmented-generation]] = retrieval as a tool
- Tool use + [[pattern:structured-output]] = the tool *is* the structured output enforcer

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.85}`
- depends-on → [[pattern:structured-output]] `{conf: 0.9}` (tool args are JSON)
- enables → [[pattern:agent-loop]] `{conf: 0.85}`
- alternative-to → [[concept:retrieval-augmented-generation]] `{conf: 0.5}` (different mechanism for external knowledge)

## Changelog

- 2026-05-09 — created
