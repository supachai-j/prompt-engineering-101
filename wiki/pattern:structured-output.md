---
id: pattern:structured-output
type: pattern
title: Structured output (XML, JSON, prefilling)
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-anthropic-prompting-best-practices.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: procedural
half_life_days: 180
tags: [deck, advanced]
---

# Structured output

## Summary

When you need machine-parseable output, ad-hoc prose won't cut it. Three reliable techniques: **XML tags** (Claude's native), **JSON with schema**, and **prefilling** (force the response to start with a specific token). Pick based on downstream needs.

## Claims

- "Use XML to delimit sections. Common tags: `<instructions>`, `<context>`, `<example>`, `<question>`, `<answer>`, `<format>`. Claude was specifically trained on XML structure." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`
- Prefilling: "Start the assistant turn with a prefix (e.g. `{` for JSON, `<analysis>` to force structured output). Claude continues from where the prefill ends." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`

## Technique 1 — XML tags (Claude-friendly)

```text
Extract the customer info from the email.

<email>
Hi, my name is Lisa Chen, account #4421. I want to update my shipping
address to 123 Main St, Bangkok 10110. Thanks!
</email>

Output your answer as:
<extraction>
  <name>...</name>
  <account>...</account>
  <address>
    <street>...</street>
    <city>...</city>
    <postal>...</postal>
  </address>
</extraction>
```

XML works particularly well for Claude because it was trained on XML structure. For other vendors, XML still works but may need stronger format anchoring.

## Technique 2 — JSON with explicit schema

```text
Extract customer info as JSON.

Schema:
{
  "name": string,
  "account": string,
  "address": {
    "street": string,
    "city": string,
    "postal": string
  }
}

Email: ...

Output only valid JSON, no commentary.
```

**Reliability tips:**
- Always say "output **only** valid JSON, no commentary"
- Specify schema, don't just say "as JSON" — vague produces creative variations
- Use modern JSON-mode features when available _(specific to: openai response_format, claude tool-use)_

## Technique 3 — Prefilling (force the start)

In the API, you can pre-fill the assistant's response. The model continues:

```text
User: Extract the customer info as JSON.
Email: ...
Assistant: {
  "name": "
```

The model continues from `"`, very likely producing valid JSON because it's already started. This **eliminates** "Sure, here's the JSON..." preambles.

Common prefills:

| Prefill | Forces |
|---|---|
| `{` | JSON object |
| `[` | JSON array |
| `<analysis>` | Structured XML |
| `1. ` | Numbered list |
| `- ` | Bullet list |
| `\`\`\`python\n` | Code block |

## Combining: schema + prefill (the "double belt")

Best reliability comes from layering:

```text
[Prompt with explicit JSON schema]
[Strong "output only JSON" instruction]
[Prefill with "{"]
```

This gets you near-100% valid JSON across complex extractions.

## Anti-patterns

- ❌ **Asking for JSON without specifying schema** — creative interpretations galore
- ❌ **Mixing "explain this" with "output JSON"** — model adds prose around the JSON, breaking parse
- ❌ **No prefill on JSON-mode-disabled APIs** — "Sure! Here's the JSON: { … }" breaks `JSON.parse`
- ❌ **Using XML tags inconsistently** — `<note>` in one place, `<NOTE>` elsewhere → model imitates the inconsistency

## Validation loop (pair with this pattern)

Always parse the output and validate against schema. If parse fails:
1. Send the failed response back to the model
2. Say "Your previous output was not valid JSON. Specifically: [error]. Output again, valid this time."
3. Or use vendor JSON-mode to enforce parser-side

→ See [[pattern:evaluation-driven-prompting]] for systematic eval.

## Relationships

- composes → [[concept:prompt-elements]] `{conf: 0.85}`
- depends-on → [[concept:role-prompting]] `{conf: 0.5}`
- enables → [[pattern:tool-use]] `{conf: 0.85}`
- mitigates → [[risk:format-drift]] `{conf: 0.95}`

## Changelog

- 2026-05-09 — created
