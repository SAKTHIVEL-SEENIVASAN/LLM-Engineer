# LLM Systems Engineer Assessment


| Field | Details |
|------|--------|
| Name | Sakthivel S M |
| Degree | B.Tech Final Year — AI & Data Science |
| Email | s.m.sakthivelofficial@gmail.com |
| GitHub | https://github.com/SAKTHIVEL-SEENIVASAN/LLM-Engineer |
| Colab | https://colab.research.google.com/drive/1AWASLhBwxpTDOHGYlXepiK67bsmnIwtF?usp=sharing |

---
---

## Overview

This project implements an end-to-end LLM systems pipeline covering architecture parsing, LoRA fine-tuning, model composition, and evaluation, designed with modularity and extensibility in mind.

**Models used:** GPT-2 (124M), TinyLlama-1.1B-Chat-v1.0
*(Phi-4-mini skipped due to VRAM limits; parser supports it without modification)*

**Key result:** Fine-tuned TinyLlama improved from 26% → 100% accuracy on a small IMDB subset.

---

## Task 1: Model Architecture Parser

### Approach

A recursive PyTorch module walker traverses the full model tree and produces a structured JSON representation with metadata per layer (type, class name, parameter count, component category).

### Design Rationale

* **Architecture-agnostic:** Uses recursion over `named_children()` to avoid hardcoding model structures.
* **Component classification:** Layers grouped into `attention`, `mlp`, `normalization`, `embedding`, or `other` via keyword matching.
* **Dual output:** JSON for programmatic use and tree print for human inspection.

### Trade-offs

* Counts only direct parameters per node to avoid duplication.
* Keyword-based classification may mislabel uncommon architectures; a registry or `isinstance()` approach would improve robustness.

---

## Task 2: LoRA Fine-Tuning

### Setup

* **Model:** TinyLlama-1.1B-Chat-v1.0
* **Dataset:** IMDB (binary sentiment)
* **Library:** Unsloth (LoRA + memory optimizations)
* **Quantization:** 4-bit (NF4)
* **Training:** 3 epochs, ~500 samples

### Key Decisions

* **LoRA:** Reduces trainable parameters by ~99%, enabling fine-tuning on limited hardware.
* **Unsloth:** Improves memory efficiency and training stability on T4 GPU.

### Important Detail

Prompt format was kept consistent (`[INST] ... [/INST]`) between training and evaluation to avoid distribution mismatch.

---

## Task 3: Model Composition & Evaluation

### Composition

LoRA adapters were merged into the base model using `merge_and_unload()`, producing a standalone model without adapter overhead.

### Evaluation (50 samples)

| Model                 | Accuracy |
| --------------------- | -------- |
| Base TinyLlama        | 26%      |
| LoRA-merged TinyLlama | 100%     |
| Improvement           | +74%     |

Prompt format was standardized across base and fine-tuned evaluation to ensure a fair comparison.

### Interpretation

The base model produces free-form text and struggles with strict label outputs. Fine-tuning aligns it with the classification task.

---

## Working Conditions

* **Environment:** Google Colab (Tesla T4, 16GB VRAM)
* **Execution Time:**

  * Parsing: ~1–2 min
  * Training: ~5–8 min
  * Evaluation: ~3–5 min

### Challenges

* GPU OOM resolved via 4-bit quantization
* PyArrow conflict fixed by version pinning
* Session resets handled via Drive checkpointing

---

## Extensibility & Scalability

* Parser scales naturally to multiple models via iteration.
* Metadata can be stored in a registry for querying and comparison.
* At larger scale, bottlenecks shift to storage and loading; solutions include lazy loading and external inference servers.

Potential limitation: keyword-based layer classification may fail on non-standard architectures.

---

## Key Insight

Evaluation should consider not only accuracy but also output confidence. Tracking entropy of model predictions provides a more reliable signal of true improvement than accuracy alone.

---

## Honest Reflection

The parser and LoRA setup were straightforward due to clean abstractions in PyTorch and Unsloth.

The evaluation required careful handling, as the base model generates free-form responses rather than strict labels, leading to possible false positives in label matching.

The training loss plateaued early (~2.74 → ~2.16 by step ~150), suggesting limited additional learning from the dataset or suboptimal training configuration.

The 100% accuracy on a 50-sample subset is likely inflated due to small evaluation size and distribution bias. Performance on the full IMDB test set (~25K samples) would likely be significantly lower.

If extended, I would implement a proper evaluation harness with larger datasets and confidence-based metrics.

---

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook LLM_ENGINEER.ipynb
```

**Note:** Tasks 2 and 3 require a CUDA GPU (≥8GB VRAM). The notebook was developed on Google Colab.

---