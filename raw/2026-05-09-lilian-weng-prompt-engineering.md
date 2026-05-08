---
source_type: web
source_url: https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/
ingested_at: 2026-05-09
title: Lilian Weng — Prompt Engineering (academic taxonomy)
---

# Lilian Weng — Prompt Engineering

Academic survey/taxonomy. Most cited primer on prompt engineering as a research area.

## Core definition

> "Methods for how to communicate with LLM to steer its behavior for desired outcomes without updating the model weights." — Weng (2023)

Key distinction: prompt engineering is **weights-frozen**. It's communication, not training.

## Primary techniques

### Zero-Shot Prompting
Simplest baseline: feed task text, ask for results. No demonstrations.

### Few-Shot Learning
Show high-quality input-output pairs. Critical caveat:

> "Choice of prompt format, training examples, and the order of the examples can lead to dramatically different performance."

**Selection strategies (research-backed):**
- Semantic similarity (k-NN clustering — Liu et al., 2021)
- Graph-based diverse sampling (Su et al., 2022)
- Contrastive learning quality measurement (Rubin et al., 2022)
- Uncertainty-based active learning (Diao et al., 2023)

### Instruction Prompting
Models like InstructGPT fine-tuned on (instruction, input, output) tuples via RLHF — improves instruction-following.

### Chain-of-Thought (CoT)
Generate "short sentences to describe reasoning logics step by step" before final answer.

**Two variants:**
- **Few-shot CoT** — manually-written reasoning chains
- **Zero-shot CoT** — prompts like "Let's think step by step"

> Benefits most pronounced for complex reasoning + large models (>50B parameters).

**Extensions:**
- Self-consistency sampling — majority voting across N samples
- Self-Ask — iterative follow-up questioning
- Tree of Thoughts — explore multiple reasoning paths
- Complexity-based — prefer elaborate chains

### Self-Consistency Sampling
"Sample multiple outputs with temperature > 0 and then selecting the best one" via majority voting or verification.

## Automatic Prompt Design

### APE (Automatic Prompt Engineer)
1. Generate instruction candidates from demonstrations
2. Filter by score (execution accuracy or log probability)
3. Iteratively improve via semantic variation prompts

### Auto-CoT (Shum et al., 2023)
- **Augment** — generate pseudo-chains
- **Prune** — remove incorrect chains
- **Select** — policy gradient to optimise example distribution

### Clustering approach (Zhang et al., 2023)
> "Models tend to make certain types of mistakes that cluster in embedding space; sampling diverse error clusters improves performance."

## Augmented Language Models

### Retrieval-Augmented Generation (RAG)
Combines external retrieval with generation. Lazaridou et al. (2022):

> "Despite LM having access to latest information via Google Search, its performance on post-2020 questions are still a lot worse than on pre-2020 questions."

**Answer ranking:**
- RAG style (TF-IDF weighted probabilities)
- Noisy channel inference
- Product-of-Experts (PoE) — most effective

### Programmatic Generation
**PAL / PoT** — model generates code, runtime (Python interpreter) executes. Decouples computation from reasoning.

### External API Integration
**TALM / Toolformer** — structured API calls via special tokens.

Toolformer's 5-tool ecosystem: calculators, Q&A systems, search engines, translators, calendars.

**Toolformer training:**
1. Annotate potential API calls via few-shot
2. Filter by self-supervised loss (does API result improve token prediction?)
3. Fine-tune on augmented dataset

## Bias findings (Zhao et al., 2021)

Three documented biases:
- **Majority label bias** — model favours frequent label
- **Recency bias** — repeats final labels
- **Common token bias** — prefers frequent tokens

## Example ordering (Lu et al., 2022)

> "Different permutations yield dramatically different results; same order fails across different model sizes."

Implication: example order is hyperparameter, not free choice.

## CoT complexity finding (Fu et al., 2023)

> "Prompts with demonstrations of higher reasoning complexity can achieve better performance. Newlines outperform other separators."

## Author's critical take

> "Some prompt engineering papers are not worthy 8 pages long, since those tricks can be explained in one or a few sentences and the rest is all about benchmarking."

A reminder that the field has signal-to-noise issues — distil what's actionable.
