---
source_type: web
source_url: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview
ingested_at: 2026-05-09
title: Anthropic — Prompt engineering overview
---

# Anthropic — Prompt engineering overview

## Pre-conditions for prompt engineering

Anthropic's docs explicitly require **3 prerequisites** before you start prompt engineering:

1. A clear definition of the success criteria for your use case
2. Some ways to empirically test against those criteria
3. A first draft prompt you want to improve

Without these three, prompt engineering is "premature optimisation".

## When prompt engineering is the right lever

> "This guide focuses on success criteria that are controllable through prompt engineering. Not every success criteria or failing eval is best solved by prompt engineering. For example, latency and cost can be sometimes more easily improved by selecting a different model."

**Implication:** Use prompt engineering for behaviour/quality/format problems. Use model selection (or fine-tuning) for latency/cost issues.

## Techniques (covered in linked best-practices page)

The umbrella techniques Anthropic recommends:
- Clarity (be clear and direct)
- Examples (multishot / few-shot prompting)
- XML structuring
- Role prompting (system prompts)
- Thinking / Chain-of-thought
- Prompt chaining

## Tools Anthropic ships

- **Claude Console prompt generator** — for first draft prompts
- **Templates and variables** — for parameterising
- **Prompt improver** — automated suggestions
- **Interactive tutorial** — github.com/anthropics/prompt-eng-interactive-tutorial
- **Spreadsheet tutorial** — Google Sheets-based interactive

## Linked sub-pages

- /build-with-claude/prompt-engineering/claude-prompting-best-practices (the deep version)
- /build-with-claude/prompt-engineering/prompting-tools
- /test-and-evaluate/develop-tests (success criteria + evals)
