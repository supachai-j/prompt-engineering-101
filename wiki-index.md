# Index — Prompt Engineering 101

A wiki for Prompt Engineering — fundamentals, applied patterns, evaluation, and safety. Vendor-neutral but exemplified with Claude, GPT, and Gemini where useful. Pages cite raw sources under `raw/`.

## Concepts (foundations)

- [Prompt Engineering — definition](wiki/concept:prompt-engineering.md) — what it is, when it's the right lever
- [Elements of a prompt](wiki/concept:prompt-elements.md) — instruction · context · input · output indicator
- [LLM settings](wiki/concept:llm-settings.md) — temperature, top_p, max length, penalties
- [Zero-shot prompting](wiki/concept:zero-shot.md) — the baseline
- [Few-shot prompting](wiki/concept:few-shot.md) — show examples; selection + ordering matter
- [Chain-of-Thought](wiki/concept:chain-of-thought.md) — think out loud (zero-shot CoT, few-shot CoT, self-consistency)
- [Role prompting & system prompts](wiki/concept:role-prompting.md) — set persona at the API level

## Patterns (production techniques)

- [Structured output](wiki/pattern:structured-output.md) — XML / JSON / prefilling
- [Prompt chaining](wiki/pattern:prompt-chaining.md) — break complex tasks into steps
- [Tool use / function calling](wiki/pattern:tool-use.md) — let models call code
- [Retrieval-Augmented Generation (RAG)](wiki/concept:retrieval-augmented-generation.md) — inject external knowledge
- [Agent patterns](wiki/concept:agent-patterns.md) — ReAct, Reflexion, planner-executor
- [Defensive prompting](wiki/pattern:defensive-prompting.md) — layered defences against injection
- [Evaluation-driven prompt development](wiki/pattern:evaluation-driven-prompting.md) — eval-first iteration loop

## Risks

- [Prompt injection](wiki/risk:prompt-injection.md) — the LLM SQL injection (direct, indirect, progressive)

## Vendor-specific features

- [Claude `effort` parameter](wiki/vendor:claude-effort.md) — _(specific to Claude 4.x)_ — 5 levels controlling thinking budget

## Decisions

_(populated as the course evolves)_

## Training portal

- [Portal home](index.html) — bilingual landing
- [Self-paced course (EN)](course-en.html) — 15+ modules across 2 tracks
- [Self-paced course (TH)](course-th.html) — Thai mirror

## Slides

- [English deck](slides/training-en.html)
- [Thai deck](slides/training-th.html)
