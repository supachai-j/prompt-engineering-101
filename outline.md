# Outline — Prompt Engineering 101

_Phase 5 deliverable from `/scaffolding-course-portal` pipeline._
_Written: 2026-05-09 · Author: Supachai Jaturaprom_

15 modules across 2 tracks. Every module cites at least one wiki page.

```
┌──────────────────────────────┐    ┌────────────────────────────────────┐
│  TRACK A — Foundations       │    │  TRACK B — Production patterns      │
│  Modules 01-06 · ~25-30 min  │    │  Modules 07-15 · ~60-75 min         │
│  For: knowledge workers       │    │  For: engineers building LLM apps  │
└──────────────────────────────┘    └────────────────────────────────────┘
                              ↓ both share modules 01-06
                        15+ modules total · ~90 min full read
```

## Track A — Foundations (modules 01-06)

### Module 01 — Welcome &amp; the mental model
- **Wiki cited:** [[concept:prompt-engineering]]
- **Outcome:** Understand what prompt engineering is, when it's the right lever, and the 3 prerequisites (success criteria, evals, first draft)
- **Key claims:** weights-frozen practice; not a fix for latency/cost; needs measurable success criteria
- **Lab:** none (orientation module)

### Module 02 — What's in a prompt?
- **Wiki cited:** [[concept:prompt-elements]]
- **Outcome:** Recognise instruction / context / input / output indicator in any prompt; rewrite a bare instruction into a full prompt
- **Key claims:** the four elements; the "brilliant new employee" framing; sequential numbering when order matters
- **Lab:** Take a one-line ChatGPT-style prompt and expand it using all four elements

### Module 03 — The knobs that aren't your prompt
- **Wiki cited:** [[concept:llm-settings]]
- **Outcome:** Pick the right temperature/top_p/max_length/penalty for the task; understand vendor-specific knobs (Claude effort)
- **Key claims:** temperature controls *variety* not quality; OR top_p, never both; effort levels (Claude); silent truncation gotcha
- **Lab:** Run the same prompt at temperature 0 vs 0.9; observe variance

### Module 04 — Zero-shot vs few-shot
- **Wiki cited:** [[concept:zero-shot]], [[concept:few-shot]]
- **Outcome:** Decide when to add examples; pick + order them; avoid bias gotchas
- **Key claims:** start zero-shot; 2-5 examples typical; positive > negative; order matters; bias warnings (majority/recency/common-token)
- **Lab:** Write a zero-shot classifier; add 3 examples; measure accuracy delta

### Module 05 — Chain-of-Thought (think out loud)
- **Wiki cited:** [[concept:chain-of-thought]], [[vendor:claude-effort]]
- **Outcome:** Use "Let's think step by step" or extended thinking effectively; know when CoT helps vs hurts
- **Key claims:** zero-shot CoT magic phrases; few-shot CoT chains; helps math/multi-step, neutral on classification, no help on translation/recall; vendor-specific equivalents (Claude effort, OpenAI o1)
- **Lab:** Compare baseline vs zero-shot CoT on a multi-step word problem

### Module 06 — When to use prompt engineering (and when not)
- **Wiki cited:** [[concept:prompt-engineering]] + cross-references to [[concept:retrieval-augmented-generation]]
- **Outcome:** Decision tree — prompt engineering vs RAG vs fine-tuning vs model swap
- **Key claims:** PE for behaviour/quality/format; not for latency/cost (model swap); not for fresh knowledge (RAG); not for stable behaviour change (fine-tuning)
- **Lab:** Given 5 scenarios, classify each as "PE / RAG / fine-tune / model swap"

→ End of Track A. Knowledge workers can stop here. Engineers continue.

---

## Track B — Production patterns (modules 07-15)

### Module 07 — Role prompting &amp; system prompts
- **Wiki cited:** [[concept:role-prompting]]
- **Outcome:** Use system prompts effectively; choose persona, constraints, output style; avoid the "magic spell" anti-pattern
- **Key claims:** system vs user message split; concrete behaviour rules &gt;&gt; abstract expertise; vendor differences (Claude `system`, OpenAI role-based, Gemini `systemInstruction`)
- **Lab:** Write a 3-component system prompt (persona + constraints + output style) for a fictional product

### Module 08 — Structured output (XML, JSON, prefilling)
- **Wiki cited:** [[pattern:structured-output]]
- **Outcome:** Force machine-parseable output; combine schema + prefill for near-100% reliability
- **Key claims:** XML for Claude; JSON with explicit schema; prefilling eliminates preambles; the "double belt" pattern
- **Lab:** Convert a free-form extraction prompt into JSON-mode + prefill; measure parse-error rate

### Module 09 — Prompt chaining
- **Wiki cited:** [[pattern:prompt-chaining]]
- **Outcome:** Break complex tasks into steps with structured i/o between them; know when to chain vs single prompt
- **Key claims:** plan → validate → execute pattern; force structured output between steps; different settings per step
- **Lab:** Build a 3-step research-synthesis chain (claim extraction → verification → polish)

### Module 10 — Tool use / function calling
- **Wiki cited:** [[pattern:tool-use]]
- **Outcome:** Define tools, write descriptions that trigger reliably, handle the call-result loop
- **Key claims:** the description is itself a prompt-engineering task; ≤20 tools per agent; allowlist arguments; vendor differences (tools/functions/functionDeclarations)
- **Lab:** Define 3 tools for a customer-support bot; write descriptions; run trigger evals

### Module 11 — Retrieval-Augmented Generation
- **Wiki cited:** [[concept:retrieval-augmented-generation]]
- **Outcome:** Decide when RAG fits; build the minimal flow; force model to cite chunks
- **Key claims:** post-cutoff knowledge problem; "lost in the middle"; force citation; hallucinated citations gotcha; pair with retrieval quality investment
- **Lab:** Mock a tiny RAG flow with 5 chunks; force the model to cite chunk IDs; verify citations programmatically

### Module 12 — Evaluation-driven prompt development
- **Wiki cited:** [[pattern:evaluation-driven-prompting]]
- **Outcome:** Build evals before iterating; train/validation split; trigger-rate measurement; pick the best iteration
- **Key claims:** vibes-only iteration is the #1 anti-pattern; 10-50 examples first pass; 3 runs minimum; pick by validation pass rate, not latest iteration
- **Lab:** Build a 20-example labelled query set; run 3 iterations of a prompt; compare validation pass rates

### Module 13 — Prompt injection: the threat model
- **Wiki cited:** [[risk:prompt-injection]]
- **Outcome:** Recognise direct / indirect / progressive injection; understand the trust-boundary problem
- **Key claims:** untrusted text in prompt is *executed* by model; three classes; severity gradient; vendor differences in attack response
- **Lab:** Try 5 injection attacks on a sample bot; classify each; identify which succeeded

### Module 14 — Defensive prompting (the four layers)
- **Wiki cited:** [[pattern:defensive-prompting]]
- **Outcome:** Apply evaluation-first logic, role anchoring, output conditioning, instruction repetition; pair with programmatic guardrails
- **Key claims:** no single layer suffices; vendor profiles differ; production checklist (system prompt + anchoring + output conditioning + adversarial CI)
- **Lab:** Take Module 13's injection attacks; apply 4-layer defence; re-test; measure success-blocked rate

### Module 15 — Agent patterns
- **Wiki cited:** [[concept:agent-patterns]], [[pattern:tool-use]], [[pattern:prompt-chaining]]
- **Outcome:** Choose between ReAct, Reflexion, planner-executor; bound loops; maintain state
- **Key claims:** Thought→Action→Observation cycle; self-critique pattern; planner ≠ executor split; vendor agentic differences (Claude 4.7 autonomous, GPT-4 tool-eager); always cap iterations
- **Lab:** Implement a 3-iteration ReAct loop using tool-use definitions from Module 10

### Module 16 — Cheatsheet &amp; resources (bonus)
- **Wiki cited:** All
- **Outcome:** One-page reference; what to read next
- **Content:** Quality checklist (parse-rate, accuracy, injection-block, etc.); links to all 5 sources in raw/; what to do for follow-up courses (multimodal, fine-tuning)

---

## Module-to-wiki mapping (Phase 5 quality gate)

| Module | Wiki page(s) cited | Status |
|---|---|---|
| 01 | concept:prompt-engineering | ✓ |
| 02 | concept:prompt-elements | ✓ |
| 03 | concept:llm-settings | ✓ |
| 04 | concept:zero-shot, concept:few-shot | ✓ |
| 05 | concept:chain-of-thought, vendor:claude-effort | ✓ |
| 06 | concept:prompt-engineering, concept:retrieval-augmented-generation | ✓ |
| 07 | concept:role-prompting | ✓ |
| 08 | pattern:structured-output | ✓ |
| 09 | pattern:prompt-chaining | ✓ |
| 10 | pattern:tool-use | ✓ |
| 11 | concept:retrieval-augmented-generation | ✓ |
| 12 | pattern:evaluation-driven-prompting | ✓ |
| 13 | risk:prompt-injection | ✓ |
| 14 | pattern:defensive-prompting | ✓ |
| 15 | concept:agent-patterns, pattern:tool-use, pattern:prompt-chaining | ✓ |
| 16 | (cheatsheet — cross-references all) | ✓ |

**Phase 5 done when:** every module maps to ≥1 wiki page. ✅ Met.

## Time estimate to Phase 6 (course pages)

- Track A modules (01-06): ~3 hours
- Track B modules (07-15): ~5 hours
- Module 16 (cheatsheet): ~30 min
- Total Phase 6: ~8.5 hours bilingual = ~5 hours EN-only

## Done

- [x] 15+ modules drafted
- [x] Two tracks defined
- [x] Every module cites wiki
- [x] Lab activities specified (Modules 02-15)
- [x] outline.md committed

→ **Phase 5 complete.** Next: Phase 6 (course-en.html long-form).
