#  LLM Systems Engineer Assessment

**Name:** Sakthivel S M
**B.Tech (Final Year) — Artificial Intelligence & Data Science**
 [s.m.sakthivelofficial@gmail.com](mailto:s.m.sakthivelofficial@gmail.com)

---

##  Overview

This project demonstrates an end-to-end **LLM systems pipeline** covering:

* Model architecture parsing
* Efficient fine-tuning using LoRA
* Model composition via adapter merging
* Performance evaluation
* System design thinking and scalability

The goal is to move beyond simple model usage and build a **practical, extensible LLM system**.

---

## Task 1: Model Architecture Parsing

A **recursive parser** was implemented to analyze internal model structure.

### Key Features:

* Traverses PyTorch modules hierarchically
* Classifies components:

  * Attention layers
  * MLP blocks
  * Normalization layers
  * Embeddings
* Outputs:

  * Tree-style visualization
  * JSON representation

### Design Choice:

A recursive approach ensures flexibility across different architectures without hardcoding model-specific logic.

### Result:

Successfully extracted structured representations for:

* GPT-2
* TinyLlama

---

##  Task 2: LoRA Fine-Tuning

Fine-tuning was performed on **TinyLlama-1.1B-Chat** using **PEFT (LoRA)**.

### Setup:

* Dataset: IMDB (sentiment classification)
* Training: 1–3 epochs
* Optimizations:

  * 4-bit quantization
  * Gradient checkpointing

### Key Insight:

LoRA updates only a small subset of parameters, enabling efficient training on limited hardware.

### Result:

The model successfully adapted to the sentiment classification task with minimal resource usage.

---

##  Task 3: Model Composition & Evaluation

The LoRA adapter was merged into the base model and evaluated.

### Evaluation Setup:

* Prompt-based classification
* 50-sample test subset
* Deterministic generation

### Results:

| Model            | Accuracy |
| ---------------- | -------- |
| Base TinyLlama   | 26%      |
| Fine-tuned Model | 100%     |
| Improvement      | +74%     |

### Interpretation:

* Base model lacks task-specific understanding
* Fine-tuned model demonstrates strong adaptation
* Significant improvement confirms effectiveness of LoRA

 *Note: Evaluation was performed on a small subset; larger evaluation is required for full generalization validation.*

---

##  Working Conditions

* Environment: Google Colab
* GPU: Tesla T4
* RAM: ~12 GB

### Execution Time:

* Parsing: ~1–2 minutes
* Training: ~5–8 minutes
* Evaluation: ~3–5 minutes

### Challenges Faced:

* GPU memory constraints
* PyArrow compatibility issues
* Colab session resets (model persistence)

### Solutions:

* 4-bit quantization
* Dependency version control
* Saving models to Google Drive

---

##  Extensibility & Scalability

### Scaling to Multiple Models:

* Store parsed outputs as structured JSON
* Use modular pipelines for batch processing
* Maintain architecture abstraction layer

### Potential Bottlenecks:

* GPU memory (VRAM)
* Inference latency
* Storage for large models

### Handling Different Architectures:

* Use class-based abstraction instead of hardcoding
* Normalize components into common categories:

  * attention
  * mlp
  * normalization

---

##  Creativity & Future Vision

### 1. Self-Improving Model Pipeline

Automated loop:

```
Parse → Evaluate → Identify Weakness → Fine-tune → Merge
```

Enables continuous model improvement.

### 2. Selective Layer Fine-Tuning

Instead of full LoRA:

* Identify weak layers
* Apply targeted adaptation

### 3. Multi-Model Composition

Combine strengths of multiple models:

* Model A → reasoning
* Model B → classification

---

###  Unique Insight

Ensuring **prompt consistency** between training and evaluation significantly improves performance.
Misaligned prompts are a common but overlooked source of failure in LLM systems.

---

##  Honest Reflection

### Straightforward:

* Recursive model parsing
* LoRA integration using PEFT

### Challenging:

* Memory optimization during training
* Dependency conflicts (pyarrow)
* Ensuring correct evaluation logic

### External Help:

* HuggingFace documentation
* Debugging with LLM assistance

### Personal Contribution:

* Improved parser design
* Built evaluation pipeline
* Resolved real-world system issues

---

##  Conclusion

This project demonstrates:

* Strong understanding of LLM architecture
* Practical fine-tuning under constraints
* Effective model composition
* Systems-level thinking

 Achieved **+74% improvement**, showcasing the power of efficient adaptation techniques like LoRA.

---
