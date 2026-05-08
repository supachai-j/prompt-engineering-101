---
id: pattern:evaluation-driven-prompting
type: pattern
title: Evaluation-driven prompt development
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-anthropic-prompt-engineering-overview.md
  - raw/2026-05-09-anthropic-prompting-best-practices.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: procedural
half_life_days: 180
tags: [deck, foundational]
---

# Evaluation-driven prompt development

## Summary

Don't iterate prompts by vibes. **Build evaluations first**, then optimise the prompt against them. Same loop ML engineers use for models — train/validation split, run, measure, iterate. The single biggest difference between hobby prompts and production prompts.

## Claims

- Anthropic explicitly lists "Some ways to empirically test against [success criteria]" as a prerequisite for prompt engineering. `[src: raw/2026-05-09-anthropic-prompt-engineering-overview.md] {conf: 0.95}`
- "We recommend iterating on prompts against a subset of your evals or test cases to validate recall or F1 score gains." `[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.95}`

## The 5-step loop

```
1. Define success criteria (in plain language, then measurable)
2. Build an eval set (10-50 labelled examples)
3. Run prompt → score outputs
4. Identify failure mode → revise prompt
5. Re-run, measure delta, decide ship/iterate
```

## Step 1 — Success criteria

**Vague (won't work):** "The output should be good."

**Concrete (works):**
- For extraction: "100% of fields valid JSON; ≥95% match human-labelled values."
- For classification: "≥90% accuracy on labelled test set; ≤2% false positives on negatives."
- For generation: "≥80% of outputs rated 'acceptable' by human reviewer; <1% policy violations."
- For tool selection: "≥90% of test queries trigger correct tool."

If you can't measure it, you can't optimise it.

## Step 2 — Build an eval set

Aim for 10-50 labelled examples in the first pass. Each is `(input, expected_output)` or `(input, list_of_acceptable_outputs)`.

For trigger evals (does the prompt activate the right behaviour?):
```json
[
  { "input": "Refund my order #4421", "should_route_to": "refund_handler", "should_trigger": true },
  { "input": "What's the weather?", "should_route_to": "refund_handler", "should_trigger": false }
]
```

**Vary along axes:**
- Phrasing (formal / casual / typos)
- Explicitness (names domain vs hides it)
- Detail (terse vs context-heavy)
- Complexity (single-step vs multi-step)

**Strong negatives are near-misses** — same keywords, different intent. Weak negatives ("write a poem") test nothing.

## Step 3 — Run + score

For binary outcomes (correct/incorrect): run, count.

For graded outcomes: rubric (1-5 scale on dimensions like correctness, format, tone). Either grade manually or use an LLM-as-judge with a separate prompt.

**Run multiple times** — models are non-deterministic. Three runs minimum:

```
trigger_rate = (number of runs that triggered) / (total runs)

A test passes if trigger_rate matches expectation:
  should_trigger=true:  trigger_rate > 0.5
  should_trigger=false: trigger_rate < 0.5
```

## Step 4 — Identify failure mode

Don't rewrite the whole prompt — diagnose the specific failure:

| Symptom | Likely fix |
|---|---|
| Wrong format | Stronger format instruction + prefilling + JSON mode |
| Misses obvious cases | Add more representative few-shot examples |
| Triggers on unrelated inputs | Description too broad — narrow it, mark out-of-scope |
| Drops to safety refusal incorrectly | Persona / role too restrictive |
| Inconsistent across runs | Lower temperature; or accept variance and ensemble |
| Reasoning shortcuts | Force CoT or raise effort |

## Step 5 — Train/validation split (avoid overfitting)

Split your eval set:
- **Train (~60%)** — drives revisions
- **Validation (~40%)** — held back; measures generalisation

Iterate against train, **measure final pick on validation**. Pick the iteration with **best validation pass rate** — not necessarily the latest one. Earlier iterations sometimes generalise better.

`[src: raw/2026-05-09-anthropic-prompting-best-practices.md] {conf: 0.85}` _(general optimisation hygiene; documented in best-practices for description optimisation)_

## When iteration is enough

Stop when:
- ≥3 iterations show no measurable improvement on validation
- Pass rate is "good enough" for your bar (varies — 95% for extraction, 80% for generation)
- Cost / latency budget is hit

## Anti-patterns

- ❌ **Vibes-only iteration** — "this version feels better" → ship it. No data, no rollback.
- ❌ **Eval set too small** (<5 examples) — noise dominates signal
- ❌ **Eval set is your training set** — overfit guaranteed
- ❌ **Single-run scoring** — ignores variance
- ❌ **Pasting failed inputs as examples** — overfit; doesn't generalise
- ❌ **No baseline** — "improvement" relative to what?

## Reusable harness sketch

```bash
#!/bin/bash
# eval.sh — minimal harness for prompt iteration
QUERIES_FILE="$1"
PROMPT_FILE="$2"
RUNS=3

count=$(jq length "$QUERIES_FILE")
for i in $(seq 0 $((count - 1))); do
  query=$(jq -r ".[$i].query" "$QUERIES_FILE")
  expected=$(jq -r ".[$i].expected" "$QUERIES_FILE")
  passes=0
  for run in $(seq 1 $RUNS); do
    output=$(call_llm "$PROMPT_FILE" "$query")
    matches "$output" "$expected" && passes=$((passes + 1))
  done
  echo "{\"query\": \"$query\", \"passes\": $passes, \"runs\": $RUNS}"
done | jq -s '.'
```

(Vendor-neutral — replace `call_llm` with your call.)

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.95}`
- enables → [[pattern:prompt-chaining]] `{conf: 0.85}` (eval each step independently)
- enables → [[pattern:defensive-prompting]] `{conf: 0.85}` (red-team adversarial evals)

## Changelog

- 2026-05-09 — created
