---
id: concept:agent-patterns
type: concept
title: Agent patterns — ReAct, Reflexion, planner-executor
status: active
confidence: 0.85
sources:
  - raw/2026-05-09-promptingguide-ai.md
  - raw/2026-05-09-anthropic-prompting-best-practices.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, advanced]
---

# Agent patterns

## Summary

When a single prompt isn't enough — and a chain of prompts isn't enough either — you need an **agent**: a loop where the model decides what to do next, takes an action (often via tool use), observes the result, and decides again. Three named patterns: **ReAct** (Reason + Act), **Reflexion** (self-critique loop), and **planner-executor** (one model plans, another executes).

## Claims

- Agent patterns covered include ReAct, Reflexion, planner-executor, ART (Automatic Reasoning + Tool-use). `[src: raw/2026-05-09-promptingguide-ai.md] {conf: 0.85}`
- Modern Claude models calibrate behaviour for agentic work — Opus 4.7 is "more autonomous than prior models" with strengths in "long-horizon agentic work." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}`
- Effort levels affect tool use intensity in agents — "high or xhigh effort settings show substantially more tool usage in agentic search and coding." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.9}`

## ReAct — Reason + Act

The model alternates between **Thought** (reasoning) and **Action** (tool call). After each Action, the **Observation** (tool result) feeds back into the next Thought.

```
User: What was the temperature in Bangkok at noon yesterday?

Thought: I need to look up historical weather data for Bangkok.
Action: search("Bangkok temperature 2026-05-08 noon")
Observation: 33°C, partly cloudy

Thought: I have the answer.
Final Answer: It was 33°C in Bangkok at noon on 2026-05-08, partly cloudy.
```

**When ReAct fits:**
- Multi-step tasks needing external data
- Tasks where reasoning *between* tool calls matters
- Open-ended research / debugging

## Reflexion — self-critique loop

The model produces an answer, **critiques its own answer**, then revises:

```
1. Initial answer: [draft]
2. Self-critique: "Is this answer correct? What might be wrong?"
3. Revised answer based on critique
4. (Optional) Another critique → revise loop
```

This works because models are often better at *finding* errors than at *avoiding* them in the first pass.

**When Reflexion fits:**
- Code review (model can spot bugs in its own draft)
- Math/logic where errors are detectable
- Writing that benefits from a "second draft" pass

## Planner-Executor

Two distinct roles:
- **Planner** — produces a structured plan (often as JSON / steps)
- **Executor** — executes each step (often a separate model invocation, possibly different model)

```
Planner prompt: "Given the goal, produce a plan as JSON with steps."
   ↓ output: [{step: 1, action: "search", args: {...}}, ...]

For each step:
   Executor prompt: "Execute step N. Plan: ... Return result."
   ↓ output: result of step

Final aggregator: "Given the plan + all results, produce final answer."
```

**Why split?** Planning needs broad context, execution needs precision — different optimal models / prompts.

## Common anti-patterns

- ❌ **Unbounded loops** — agent loops without iteration cap → cost/time runaway. Always cap (5? 20? task-specific).
- ❌ **No state between iterations** — agent forgets what it tried, repeats failures. Maintain a scratchpad.
- ❌ **Over-eager tool use** — agent calls tools when reasoning would suffice. Tune via [[concept:llm-settings]] effort or explicit "use tools only when X".
- ❌ **Lost-in-the-middle of long agent traces** — early instructions get diluted. Use [[pattern:defensive-prompting]] role-anchoring throughout.

## Vendor differences in agentic mode

| Vendor | Notable behaviour |
|---|---|
| Claude 4.7 | More autonomous, more reasoning between tool calls, fewer tool calls overall. _(specific to: claude)_ `[src: raw/2026-05-09-anthropic-prompting-best-practices.md]` |
| GPT-4 | More tool-call eager; more cycles to complete tasks |
| Gemini | Varies by version; longer context window helps long traces |

Re-tune cycle/effort settings when you swap models — defaults differ.

## Composition

Agent patterns compose with everything earlier:

```
agent loop
  └─ planner (using [[concept:chain-of-thought]] for plan reasoning)
       └─ executor (using [[pattern:tool-use]] for actions)
            └─ tool: retrieval ([[concept:retrieval-augmented-generation]])
            └─ tool: API call
       └─ self-critique ([[pattern:evaluation-driven-prompting]] within the loop)
```

## Relationships

- composes → [[pattern:tool-use]] `{conf: 0.95}`
- composes → [[pattern:prompt-chaining]] `{conf: 0.9}`
- depends-on → [[concept:chain-of-thought]] `{conf: 0.85}`
- composes → [[concept:retrieval-augmented-generation]] `{conf: 0.7}`

## Changelog

- 2026-05-09 — created
