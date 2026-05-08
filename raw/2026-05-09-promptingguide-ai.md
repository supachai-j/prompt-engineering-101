---
source_type: web
source_url: https://www.promptingguide.ai/introduction
ingested_at: 2026-05-09
title: Prompting Guide (DAIR.AI) — Introduction & taxonomy
---

# Prompting Guide (DAIR.AI)

Vendor-neutral, community-maintained guide to prompt engineering. Most-cited public resource.

## Definition

> "Prompt engineering is a relatively new discipline for developing and optimizing prompts to efficiently apply and build with large language models (LLMs)."

**Two audiences:**
- **Researchers** — enhance LLM safety + performance across tasks
- **Developers** — create techniques for interfacing with LLMs and tools

## LLM Settings (parameters that affect output)

| Setting | Effect |
|---|---|
| **Temperature** | 0 = deterministic, higher = more creative/random |
| **top_p** | Nucleus sampling — limit to top-p mass of probability |
| **Max length** | Cap output tokens |
| **Stop sequences** | Strings that halt generation |
| **Frequency penalty** | Discourage repeated tokens |
| **Presence penalty** | Discourage repeated topics |

> "Recommend altering temperature OR top_p, not both."

## Elements of a prompt

A prompt may contain:
- **Instruction** — what to do (e.g. "Translate the text to French")
- **Context** — background that informs the response
- **Input data** — the actual content to act on
- **Output indicator** — format/structure of response

Not all elements are required for every prompt — but knowing them helps you debug.

## Topics covered (this is the syllabus)

### Foundational
- LLM Settings
- Basics of Prompting
- Prompt Elements
- Tips for Designing Prompts
- Prompt Examples

### Advanced techniques
- Zero-shot prompting
- Few-shot prompting
- Chain-of-Thought (CoT)
- Self-consistency
- Generated knowledge prompting
- Prompt chaining
- Tree of Thoughts (ToT)
- Retrieval Augmented Generation (RAG)
- Automatic Reasoning + Tool-use (ART)
- Automatic Prompt Engineer (APE)
- Active-Prompt
- Directional Stimulus Prompting
- Program-Aided Language Models (PAL)
- ReAct
- Reflexion
- Multimodal CoT
- Graph Prompting

### Specialised areas
- AI Agents (workflows, tool use)
- Fine-tuning vs prompt engineering
- Context caching
- Synthetic data generation
- Adversarial prompting + safety risks

### Resources
- **Prompt Hub** — templates for classification, coding, creativity, math, reasoning, Q&A
- **Models** — Claude, GPT-4, Gemini, Llama, Mistral
- **Research findings** — papers on LLM behaviour

## Reference model

> "All examples use `gpt-3.5-turbo` as the reference model with default settings unless specified otherwise."

Note: many examples may be dated; principles still apply but exact outputs may differ for newer models.

## Why this source matters for our course

It's the **vendor-neutral baseline** — anything in our course should agree with this guide unless we explicitly note a vendor-specific divergence.
