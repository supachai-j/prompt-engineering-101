---
source_type: web
source_url: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
ingested_at: 2026-05-09
title: Anthropic — Prompting best practices (Claude 4.x)
---

# Anthropic — Prompting best practices

The deep reference for prompt engineering with Claude's Opus 4.6/4.7, Sonnet 4.6, Haiku 4.5.

## General principles

### 1. Be clear and direct

> "Think of Claude as a brilliant but new employee who lacks context on your norms and workflows. The more precisely you explain what you want, the better the result."

**Golden rule:** "Show your prompt to a colleague with minimal context on the task and ask them to follow it. If they'd be confused, Claude will be too."

Specifics:
- Be specific about output format + constraints
- Sequential numbered/bulleted steps when order matters

### 2. Add context to improve performance

> "Providing context or motivation behind your instructions, such as explaining to Claude why such behavior is important, can help Claude better understand your goals and deliver more targeted responses."

Don't just say what — say why.

## Output control techniques

### Use examples (multishot / few-shot)

Show 2-5 input-output pairs. Anthropic notes that "positive examples showing how Claude can communicate with the appropriate level of concision tend to be more effective than negative examples or instructions that tell the model what not to do."

### XML tags for structure

Use XML to delimit sections. Common tags: `<instructions>`, `<context>`, `<example>`, `<question>`, `<answer>`, `<format>`. Claude was specifically trained on XML structure.

### System prompts (role prompting)

Use the `system` parameter to set persona/role. Distinct from user message — affects how Claude approaches the entire conversation.

### Prefilling Claude's response

Start the assistant turn with a prefix (e.g. `{` for JSON, `<analysis>` to force structured output). Claude continues from where the prefill ends.

### Chain prompts

Break complex tasks into smaller prompts. Output of step 1 becomes input to step 2.

## Thinking / Chain-of-Thought

Two flavours:
1. **Inline CoT in regular prompts** — tell Claude to "think step by step" or "show your reasoning before answering"
2. **Extended thinking** (Claude 3.7+, Claude 4.x) — model has dedicated thinking budget; output includes `<thinking>` blocks

For Claude 4.x, the **effort parameter** replaces fine-grained thinking control:
- `max` — most thinking; can over-think
- `xhigh` — best for coding/agentic
- `high` — most intelligence-sensitive cases (recommended default)
- `medium` — cost-sensitive, trade some intelligence
- `low` — short scoped tasks, latency-sensitive

> "Claude Opus 4.7 respects effort levels strictly, especially at the low end."

## Claude 4.7-specific tuning

- **Verbosity calibrates to task complexity** — no longer fixed. To control: explicit instruction like "Provide concise, focused responses."
- **More literal instruction following** — won't generalise; if you want broad application, state scope explicitly: "Apply this formatting to every section, not just the first one"
- **Tone shift** — more direct/opinionated. If you want warm voice: "Use a warm, collaborative tone."
- **Fewer subagents by default** — steerable via prompt
- **House design defaults** — warm cream, serif type, terracotta accents. Override with concrete spec or "propose 4 directions before building"
- **Tool use threshold** — uses tools less than 4.6, reasons more. Raise effort for more tool use.
- **Code review** — at lower severity bar, may filter out findings. For coverage: "Report every issue, including low-severity. Filtering happens later."

## Examples of strong prompt patterns (verbatim from docs)

### Force concise output
```
Provide concise, focused responses. Skip non-essential context, and keep examples minimal.
```

### Force coverage in code review
```
Report every issue you find, including ones you are uncertain about or consider low-severity.
Do not filter for importance or confidence at this stage - a separate verification step will do that.
For each finding, include your confidence level and an estimated severity.
```

### Steer thinking trigger
```
Thinking adds latency and should only be used when it will meaningfully improve answer quality —
typically for problems that require multi-step reasoning. When in doubt, respond directly.
```

### Avoid generic AI aesthetic
```
<frontend_aesthetics>
NEVER use generic AI-generated aesthetics like overused font families (Inter, Roboto, Arial,
system fonts), cliched color schemes (particularly purple gradients on white or dark backgrounds),
predictable layouts and component patterns, and cookie-cutter design that lacks context-specific
character. Use unique fonts, cohesive colors and themes, and animations.
</frontend_aesthetics>
```

## Linked sub-pages

- /effort — effort parameter
- /computer-use-tool
- /tool-use — function/tool calling
- /thinking — extended thinking config
- /prefilling
- /chain-of-thought
- /system-prompts
