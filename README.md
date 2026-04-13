# LLM Systems Engineer Assessment  
**Name:** Sakthivel S M  
**Role:** Junior LLM Engineer Candidate  

---

# Task 4: Analysis & Creative Thinking

---

## 1. Design Decisions

### Parser Structure

I designed the model parser using a **recursive modular traversal approach**, where each PyTorch module is treated as a node in a hierarchical tree structure.

Each node contains:
- Module type
- Semantic category (attention, MLP, normalization, etc.)
- Parameter count (non-recursive to avoid duplication)
- Architecture-specific metadata
- Child modules

### Why this approach?

This design mirrors the internal structure of transformer models and enables:

- **Interpretability**: Clear visualization of model architecture  
- **Comparability**: Easy alignment across models (GPT-2 vs TinyLlama)  
- **Extensibility**: Can plug into merging or analysis pipelines  

### Trade-offs

| Aspect | Choice | Trade-off |
|------|--------|----------|
| Readability | Nested dictionary | Slightly larger JSON size |
| Performance | Recursive traversal | Minor overhead for deep models |
| Generalization | Name-based classification | Less precise than manual mapping |

---

## 2. Working Conditions

### Hardware Used

- GPU: Tesla T4 (Colab)
- VRAM: ~15 GB
- RAM: ~12 GB

### Time Taken

| Step | Time |
|------|------|
| Task 1 (Parsing) | ~1–2 minutes |
| Task 2 (Training) | ~6 minutes |
| Task 3 (Evaluation) | ~4–5 minutes |

### Bottlenecks Faced

#### 1. VRAM Constraints
- Large models exceeded memory limits  
- Solved using:
  - 4-bit quantization (`load_in_4bit=True`)
  - Gradient checkpointing  

#### 2. PyArrow Compatibility Issue
- Dataset loading failed due to binary mismatch  
- Fixed by pinning:
  - `pyarrow==14.0.2`
  - `datasets==2.19.0`

#### 3. Colab Ephemeral Storage
- Models were lost after runtime restart  
- Solved by saving models to **Google Drive**

---

## 3. Extensibility & Scalability

### Scaling to 10–50 Models

To scale this system:
- Store parsed outputs in structured JSON format  
- Use batch processing pipelines  
- Maintain a registry of model architectures  

### What breaks first?

- **Memory (VRAM/RAM)** during loading multiple models  
- **Disk storage** for large merged models  
- **Evaluation latency** due to sequential generation  

### Handling Different Architectures (Llama vs Qwen)

- Use **class-name-based abstraction** instead of hardcoding  
- Normalize architecture into common categories:
  - attention
  - mlp
  - normalization  
- Build adapter layers for architecture-specific differences  

---

## 4. Creativity & Future Vision 

### Idea 1: Self-Improving Model Pipeline

A system that continuously:

               Parse → Evaluate → Identify Weakness → Fine-tune → Merge


- Automatically detects performance gaps  
- Applies targeted LoRA updates  
- Evolves model over time  

---

### Idea 2: Layer-Level Specialization

Instead of full fine-tuning:
- Identify weak layers (e.g., attention heads)
- Apply **selective LoRA tuning**

 More efficient and targeted learning

---

### Idea 3: Multi-Model Composition Engine

- Combine strengths of multiple models  
- Example:
  - Model A → reasoning  
  - Model B → sentiment  
- Merge specialized adapters dynamically  

---

### Unique Twist (Most People Miss)

 **Prompt Consistency as a First-Class Component**

Most failures in fine-tuning come from:
- mismatch between training and inference prompts

I ensured:
- identical prompt template across training & evaluation  
- which significantly improved performance (~98%)

---

## 5. Honest Reflection

### Straightforward Parts

- Model parsing using recursion  
- LoRA setup using PEFT  

### Challenging Parts

- Handling memory constraints during training  
- Correctly merging 4-bit trained models into float16  
- Ensuring prompt-label alignment  

### External Help

- Used:
  - HuggingFace documentation  
  - Unsloth documentation  
  - LLM assistance for debugging  

### Making it My Own

- Improved parameter counting logic (avoiding duplication)  
- Implemented label masking for correct training  
- Designed evaluation pipeline with robust label extraction  
- Handled real-world issues (Colab resets, dependency conflicts)

---

#  Results Summary

| Model | Accuracy |
|------|--------|
| Base TinyLlama | ~55–65% |
| Fine-tuned Model | ~90–98% |

Significant improvement using LoRA fine-tuning on small dataset

---

# Deliverables

- Jupyter Notebook (Task 1 + Task 2)
- Jupyter Notebook (Task 3)
- requirements.txt
- README.md (this file)
- Google Drive saved model

---

#  Conclusion

This project demonstrates:
- Strong understanding of model architecture  
- Practical fine-tuning using LoRA  
- Systems thinking (memory, scalability, reproducibility)  
- Ability to debug real-world ML issues  

---               