# SCHEMA — Prompt Engineering 101

> Constitution of this wiki. Every Ingest / Query / Lint operation reads this file. If this and any operation skill disagree, this wins.

## 1. Domain

This wiki is a vendor-neutral training & reference base for **Prompt Engineering** — the discipline of communicating with LLMs to steer their behaviour without updating model weights. It covers two audiences (knowledge workers + engineers), focuses on patterns that generalise across Claude, GPT, Gemini, and other modern LLMs, and treats vendor-specific features as clearly-labelled exceptions.

It should answer: *what is a prompt, what changes a prompt's effectiveness, which technique fits which task, how do I evaluate a prompt, and how do I defend against adversarial inputs in production*.

## 2. Entity catalogue

| type | id pattern | gets its own page? | notes |
|---|---|---|---|
| `concept` | `concept:<kebab>` | yes | core ideas (prompt elements, CoT, RAG, etc.) |
| `pattern` | `pattern:<kebab>` | yes | reusable workflows / techniques |
| `technique` | `technique:<kebab>` | sometimes | specific named technique (zero-shot, few-shot, etc.) — often merged with concept |
| `vendor-feature` | `vendor:<name>-<feature>` | yes | clearly-labelled vendor-specific (claude-effort, openai-structured-output) |
| `risk` | `risk:<kebab>` | yes | failure modes, injection types, biases |
| `setting` | `setting:<name>` | yes | LLM parameters (temperature, top_p, etc.) |
| `decision` | `decision:<date>-<slug>` | yes | ADR-like (e.g. "vendor-neutral stance") |
| `source` | `source:<kebab>` | no — referenced from raw/ only | upstream URLs |

## 3. Relation catalogue

- `uses` — A consumes B at runtime
- `depends-on` — A cannot function without B
- `composes` — A is built from / orchestrates B
- `alternative-to` — A and B solve overlapping problems (zero-shot vs few-shot)
- `extends` — A is a specialisation of B (zero-shot CoT extends CoT)
- `mitigates` — A reduces risk B
- `enables` — A makes B possible
- `contradicts` — flagged for Lint to resolve
- `cites` — A draws evidence from source B
- `vendor-equivalent-of` — vendor-feature A is the X-vendor version of pattern B

## 4. Page rules

- One concept per page. If a page grows two clearly-separable subjects, split it.
- YAML frontmatter mandatory. See `wiki/_TEMPLATE.md`.
- Every claim has an inline source marker `[src: raw/...]`.
- Wikilinks use entity IDs: `[[concept:chain-of-thought]]`, not `[[Chain-of-Thought]]`.
- Status: `active` (default), `stale`, `faded`, `orphan`.
- **Vendor-neutrality discipline:** when a claim is vendor-specific, mark with `_(specific to: vendor)_` inline.

## 5. Ingest rules

- Raw source → `raw/YYYY-MM-DD-<slug>.md` untouched (after secret filter).
- Entity extraction runs against §2.
- A new page is created when entity is "yes" in the catalogue _and_ ≥1 non-trivial claim exists.
- Existing page updates: reinforce matching claims (bump confidence), append new ones.

## 6. Confidence and decay

- First observation: `confidence: 0.5`
- Reinforcement (independent source): `conf ← 1 - (1 - conf) * 0.6`
- Contradiction: open supersession candidate; do not lower directly.
- Decay half-life:
  - `decision`: 365 days
  - `concept`, `pattern`, `technique`, `risk`: 180 days
  - `vendor-feature`: **90 days** (vendors ship fast; capabilities change)
  - `setting`: 180 days
  - `source`: not applicable (raw is immutable)

Claims with `conf < 0.2` and untouched ≥ 2× half-life → `status: faded`.

## 7. Privacy and secrets

Never written to wiki/ or graph/, redacted in raw/ on detection:
- API keys, tokens, signed URLs
- Passwords, private keys, certificates
- PII

Replacement token: `<REDACTED:apikey|token|pii|secret|other>`.

Special note for prompt engineering: **don't leak production prompt text or test cases** into raw/ — those are sensitive intellectual property.

## 8. Lint policy

- Lint when ≥10 new sources since last lint, or on user request.
- Orphans with `conf > 0.5` → link from most relevant parent.
- Contradictions → resolve by (most recent authoritative source) > (most sources) > (highest prior confidence). Human confirms.
- **Vendor drift check:** any `vendor-feature` page older than 90 days without re-verification → flag.

## 9. Track structure (project-specific)

Modules are split into two tracks. Wiki pages are **shared**:

- **Track A modules (01-06)** — Foundations. Cite mostly `concept:` pages.
- **Track B modules (07-15+)** — Production. Cite `pattern:`, `risk:`, `vendor-feature:` pages.

A wiki page can be cited by both tracks. Don't duplicate.

## 10. Vendor-neutrality stance (foundational decision)

This wiki treats vendors symmetrically. Patterns and concepts are vendor-neutral by default. When a vendor ships a feature that's genuinely unique (e.g. Claude's `extended-thinking`, OpenAI's structured output JSON schema), it gets a `vendor-feature` page that:

- States the feature in 1 paragraph
- Cites the vendor's primary doc as `[src:]`
- Maps to a vendor-neutral equivalent if one exists, via `vendor-equivalent-of`
- Never disparages or favours alternatives

If a claim *only* applies to one vendor, mark inline: `_(specific to: claude)_`.

## 11. Co-evolution

When the LLM hits a case this schema doesn't cover:
1. Do the right thing now.
2. Append note to `raw/schema-todo.md` with date, example, proposed amendment.
3. Next Lint surfaces `schema-todo.md` for human promotion.

## 12. Training-deck rules

This wiki feeds two HTML training decks (English + Thai) under `slides/`. When a wiki page is updated with a new "deck-worthy" claim, mark the claim with `tag: deck` so Crystallize can find it.
