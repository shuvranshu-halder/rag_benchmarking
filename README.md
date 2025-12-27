
## Overview

This project presents a **systematic benchmarking of Retrieval-Augmented Generation (RAG) architectures** with a strong focus on **hallucination reduction, retrieval quality, and factual grounding**.

We progressively enhance a Classical RAG pipeline by integrating:
- Knowledge Graphs (KG)
- Optimized Chunking
- Hybrid Retrieval (Semantic + BM25)
- Context Tuning

All configurations are evaluated using **Hallucination Score, RAGAS metrics, and Mean Reciprocal Rank (MRR)**.

---

## Motivation

Large Language Models (LLMs) often generate:
- Confident but incorrect answers (hallucinations)
- Answers unsupported by retrieved documents
- Outdated or fabricated facts

**RAG mitigates these issues** by grounding generation in retrieved external knowledge without retraining the model.

---

## Dataset & Tools

- **Dataset:** SQuAD v2  
- **Embedding Model:** all-MiniLM-L6-v2  
- **Vector Store:** Chroma / FAISS  
- **Retriever:** Semantic Retriever, BM25  
- **LLM:** meta-Llama-3.1-8B  
- **Knowledge Graph:** SpaCy-based triple extraction  

---

## RAG Architectures Implemented

### 1. Classical RAG
- Semantic retrieval from vector database
- Retrieved chunks passed directly to LLM

### 2. Classical RAG + Knowledge Graph (KG)
- Structured `(subject, relation, object)` triples
- Entity-linked KG facts added to prompt
- Improves factual grounding

### 3. Classical RAG + KG + Chunking
- Optimized chunk sizes (up to 800 tokens)
- Noise reduction and fine-grained retrieval
- Strong hallucination control

### 4. Hybrid Retrieval (KG + BM25)
- Combines semantic similarity with lexical matching
- Improves recall for keyword-heavy queries

### 5. Context-Tuned RAG
- Secondary LLM rewrites and enriches user queries
- Improves retrieval alignment

---

## Hallucination Analysis

### Classical RAG + KG + Chunking (800 tokens)
| Contexts | Hallucination Score |
|--------|--------------------|
| 3 | 0.11 |
| 10 | 0.05 |
| 20+ | **0.02** |

✅ **Lowest hallucination achieved: 0.02**  
➡️ ~98% reduction compared to baseline Classical RAG

---

## RAGAS Evaluation

### KG + Chunking (Best Setup)

| Chunk Size | Contexts | Context Recall | Faithfulness | Factual Correctness |
|----------|----------|---------------|--------------|-------------------|
| 800 | 6 | **0.55** | **0.86** | **0.76** |

- High faithfulness indicates minimal hallucination
- Balanced recall and correctness

---

## Mean Reciprocal Rank (MRR)

### KG + Chunking

| Chunk Size | Contexts | MRR |
|----------|----------|-----|
| 400 | 5 | 0.87 |
| 800 | 7 | 0.92 |
| **800** | **10** | **0.94** |

🏆 **Highest MRR achieved: 0.94**  
➡️ Correct answer appears at rank-1 in most queries

---

## Key Milestones Achieved

- ✔ Built a complete **RAG benchmarking pipeline**
- ✔ Reduced hallucination from **0.92 → 0.02**
- ✔ Achieved **MRR = 0.94** using KG + Chunking
- ✔ Demonstrated effectiveness of **structured knowledge grounding**
- ✔ Identified optimal retrieval sweet spot (~20 contexts)

