# Scope — Prompt Engineering 101

_Phase 1 deliverable from `/scaffolding-course-portal` pipeline._
_Written: 2026-05-09 · Author: Supachai Jaturaprom_

## Outcome (one sentence)

> **After this course, the reader will be able to design, evaluate, and ship reliable prompts for any modern LLM — moving from ad-hoc trial-and-error to repeatable, vendor-neutral patterns.**

## Audience — two tracks

The course is split into two tracks that share fundamentals (modules 01-06)
and diverge after that:

### Track A — Foundations (modules 01-06, ~30 min)
- **Audience:** Knowledge workers, students, anyone using Claude/ChatGPT in chat
- **Focus:** Mental model + practical patterns for everyday prompting
- **No coding required**

### Track B — Production patterns (modules 07-15+, ~60 min)
- **Audience:** Engineers building LLM apps, AI engineers, technical leads
- **Focus:** Evaluation, safety, structured outputs, function calling, agents
- **Code examples** in vendor-neutral pseudo-code (Claude/GPT/Gemini sketches)

## Format

- **Self-paced reading** primary (course-en.html, course-th.html)
- **Reveal.js slides** secondary (training-en.html, training-th.html) — for live workshops
- **Wiki** as source-of-truth (cited knowledge pages under `wiki/`)
- **15+ modules**, ~90 min full read, ~25 min Track A only

## Languages

**Bilingual EN + TH** mirrored 1:1.
- EN written first
- TH mirror follows the rules in `/translating-to-thai-technical` skill
- Code examples stay in English (Rule 1 of the skill)

## Model focus — vendor-neutral

Patterns and concepts that apply to **any** modern LLM. Examples may show
Claude, GPT, or Gemini specifically, but every claim should generalise.

When a feature is genuinely vendor-specific (e.g. Claude's prompt caching,
OpenAI's function calling JSON schema), it gets a clearly-labelled callout
box: "_Specific to vendor X_".

## Time budget

| Phase | Estimated | Skip-ability |
|---|---|---|
| 1 — Scope | done (this doc) | — |
| 2 — Research | ~3-4 h | required |
| 3 — Wiki init | ~30 min | required |
| 4 — Ingest | ~3-4 h (scope is wider) | required |
| 5 — Outline | ~1.5 h (15 modules) | required |
| 6 — Course pages EN | ~6-8 h (more modules) | trim if needed |
| 7 — Slide deck EN | ~3-4 h | optional if no live session |
| 8 — Bilingual TH mirror | ~5-6 h | last priority |
| 9 — Landing tuning | ~1-2 h | required |
| 10 — Deploy | ~30 min | required |
| 11 — Validate | ~2 h | required |
| 12 — Promote | ~1 h | optional |

**Total:** ~25-35 hours bilingual full · ~10-12 hours EN-only fast path.

## Success criteria

- [ ] A junior dev can read Track A and feel confident chatting with Claude/GPT productively
- [ ] An engineer can read Track B and ship a production prompt with eval + safety guards
- [ ] All code samples are runnable (vendor-neutral but actually working when adapted)
- [ ] Every claim is cited to a primary source in `raw/`
- [ ] Both EN and TH read naturally — TH not a literal-translation feel
- [ ] Live URL returns 200 on all entry points

## Out of scope

- **Fine-tuning / training models** — different topic; mention as "out of scope, see X"
- **Specific tool tutorials** (LangChain, LlamaIndex) — vendor-neutral means no framework coupling
- **Image / multimodal prompting** — explicitly mentioned, but deep coverage in a follow-up course
- **Voice / speech** — out

## Done when

- [x] Outcome is one sentence
- [x] Audience defined (two tracks)
- [x] Format chosen (self-paced + slides + wiki)
- [x] Time budget mapped per phase
- [x] Out-of-scope explicit

→ **Phase 1 complete.** Next: Phase 2 (research & capture).
