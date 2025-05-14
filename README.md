# 🔍 RECAP: Retrieval-Augmented Question Answering System

**RECAP** is a Retrieval-Augmented Generation (RAG)-based question answering system built on the **RAG-12000** dataset. It focuses on enhancing the **retriever component** through fine-tuning strategies and evaluates how improved retrieval quality impacts the final answers.

---

## 🧠 Key Components

### 📖 Dataset
- **RAG-12000**: A structured dataset of triplets — `context`, `question`, and `answer`.

### 🔗 Model Architecture

- **Retriever**:  
  `Sentence Transformers (all-MiniLM-L6-v2)` with a **projection head** for better question-context alignment.
  
- **Fine-Tuning Techniques**:  
  - **LoRA** (Low-Rank Adaptation)  
  - **Partial Layer Freezing**

- **Generator**:  
  `T5 model` that takes `[retrieved context + question]` as input to generate the answer.

---

## ⚙️ Methodology

### 1. Retriever-Generator Pipeline
- Contexts are chunked (~300 characters each) to enhance retrieval granularity.
- Each chunk is embedded using `all-MiniLM-L6-v2`.
- FAISS is used for **similarity-based retrieval** over the encoded chunks.
- The top retrieved chunk + question is passed to the **T5 generator** for answer generation.

### 2. Retriever Fine-Tuning
- **LoRA**:  
  Adds small trainable low-rank matrices → efficient training with fewer parameters.
  
- **Partial Layer Freezing**:  
  Only bottom layers of the Sentence Transformer are unfrozen → balances performance and efficiency.

### 3. Training Configurations

| Strategy             | Epochs | Learning Rate | Batch Size |
|----------------------|--------|----------------|------------|
| LoRA                 | 3      | 5e-5           | 16         |
| Partial Layer Freezing | 10     | 2e-5           | 64         |

---

## 📊 Results: Retrieval Accuracy (Top-5)

| Model Variant            | Accuracy |
|--------------------------|----------|
| Partial Layer Freezing   | **0.93** |
| LoRA Fine-Tuning         | 0.90     |
| Baseline (MiniLM)        | 0.89     |
| Projection Head Only     | 0.002    |

---

