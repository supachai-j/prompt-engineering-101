---
id: concept:retrieval-augmented-generation
type: concept
title: Retrieval-Augmented Generation (RAG)
status: active
confidence: 0.9
sources:
  - raw/2026-05-09-lilian-weng-prompt-engineering.md
  - raw/2026-05-09-promptingguide-ai.md
created: 2026-05-09
updated: 2026-05-09
updated_log:
  - 2026-05-09: created
tiers: semantic
half_life_days: 180
tags: [deck, advanced]
---

# Retrieval-Augmented Generation (RAG)

## Summary

**RAG** = retrieve relevant external knowledge first, then inject it into the prompt. Solves the "model doesn't know X" problem without fine-tuning. The model becomes a *reasoner over your documents* rather than a *recaller* of training data.

## Claims

- RAG combines "external knowledge retrieval with generation". `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.95}`
- Even with retrieval, models perform worse on post-training-cutoff questions: "Despite LM having access to latest information via Google Search, its performance on post-2020 questions are still a lot worse than on pre-2020 questions." (Lazaridou et al., 2022) `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`
- Three answer-ranking methods are documented: RAG-style (TF-IDF weighted probabilities), noisy channel inference, Product-of-Experts (PoE — most effective in benchmarks). `[src: raw/2026-05-09-lilian-weng-prompt-engineering.md] {conf: 0.85}`

## When RAG is the right tool

| Problem | RAG fits? |
|---|---|
| Knowledge cutoff (e.g. "what shipped last week?") | ✅ yes |
| Domain-specific docs the model wasn't trained on | ✅ yes |
| Citations / traceability required | ✅ yes |
| Output must be consistent with internal docs | ✅ yes |
| Output is creative / generative | ❌ no — RAG constrains |
| Task is pure reasoning (math, code) | ❌ no — irrelevant |
| Latency budget is very tight | ⚠️ retrieval adds round-trip |

## The minimal RAG flow

```
1. INDEX (one-time)
   Documents → chunks → embeddings → vector store

2. QUERY (per-request)
   User query → embed → top-k similar chunks
   ↓
   Inject chunks into prompt:
     "Answer using only the context below.
      <context>{{chunks}}</context>
      Q: {{query}}
      A:"
   ↓
   Model output — should cite chunks
```

## Prompt patterns for RAG

### Force model to use retrieved context (not pretrained knowledge)

```
Use ONLY the information in <context> tags below to answer.
If the answer isn't in the context, say "I don't have that information."

<context>
{{retrieved_chunks}}
</context>

Question: {{user_question}}
```

### Force citation

```
Each claim must cite a source from the context using [chunk_id] notation.
If you can't cite a chunk for a claim, don't make the claim.
```

### Hybrid: pretrained + retrieved

```
You may use general knowledge, but for any domain-specific claim,
defer to the <context> below. If <context> contradicts general knowledge,
use the context.
```

## Common pitfalls

### Garbage in, garbage out
RAG quality bounded by retrieval quality. A model can't reason its way out of bad chunks.

→ Invest in retrieval quality first (chunking strategy, embedding model, re-ranking) before optimising the prompt.

### Stale chunks
If the index isn't refreshed, RAG outputs are stale. Build re-indexing into the pipeline.

### "Lost in the middle"
Modern long-context models still attend more to the start and end of context than the middle. Place critical chunks at the start (or end) of the retrieved block.

### Hallucinated citations
Without strong instruction + validation, models can generate plausible-looking citations to chunks that don't say what's claimed. Pattern: validate citations programmatically.

## RAG vs alternatives

| Approach | When |
|---|---|
| **RAG** | Need fresh / domain knowledge, traceability matters |
| **Fine-tuning** | Behaviour/style change, training data is large + stable |
| **Long context only** | Documents are small, fit easily in context window |
| **Web search tool** | Need real-time external knowledge |

## Relationships

- composes → [[concept:prompt-engineering]] `{conf: 0.9}`
- alternative-to → [[concept:few-shot]] `{conf: 0.6}` (different mechanism for adding context)
- composes → [[pattern:tool-use]] `{conf: 0.7}` (retrieval often via a tool)
- mitigates → [[risk:hallucination]] `{conf: 0.85}`

## Changelog

- 2026-05-09 — created
