
# Milestone 3: RAG-Based QA System with Prompting & Memory

##  Overview

This experiment explores how different prompting strategies (Zero-Shot vs. Chain-of-Thought) and the addition of memory influence the performance of a RAG (Retrieval-Augmented Generation) question-answering system. The setup is based on a combination of:

- **Retriever**: FAISS vector store with `MiniLM` sentence embeddings.
- **Generator**: `flan-t5-base` — a reasoning-capable model by Google.
- **Prompts**: 
  - **Zero-Shot**: Direct question answering.
  - **Chain-of-Thought (CoT)**: Prompts the model to reason step-by-step.
- **Memory**: Stores previous Q/A pairs and re-injects them into FAISS to simulate growing conversational memory.

---

## System Architecture

Each QA pair was tested in four setups:
1. Zero-Shot Prompting (No Memory)
2. Chain-of-Thought Prompting (No Memory)
3. Zero-Shot Prompting + Memory
4. Chain-of-Thought Prompting + Memory

---

##  Prompts Used

- **Zero-Shot**:
```
answer question: {question} context: {context}
```

- **Chain-of-Thought**:
```
Based on the context below, reason step-by-step to answer the question.

Context:
{context}

Question: {question}
Answer:
```

---

##  Evaluation Metrics

Three standard metrics were used:
- **F1 Score**: Measures token overlap (accuracy).
- **BLEU Score**: Measures n-gram precision.
- **ROUGE**: Measures recall overlap (rouge1, rouge2, rougeL).

---

## Results Summary

| Prompt Style | Memory | F1 Score | BLEU Score | ROUGE-1 | ROUGE-2 |
|--------------|--------|----------|------------|----------|----------|
| Zero-Shot    |  No   | 56.67%   | 0.489      | 0.567    | 0.300    |
| CoT          |  No   | 11.08%   | 0.022      | 0.109    | 0.071    |
| Zero-Shot    |  Yes  | 56.67%   | 0.489      | 0.567    | 0.300    |
| CoT          |  Yes  | 23.38%   | 0.038      | 0.231    | 0.158    |

---

##  Analysis

###  Zero-Shot
- Performed **consistently well** with or without memory.
- Gave concise, accurate answers aligned with references.
- Best F1 and BLEU scores.

### Chain-of-Thought (No Memory)
- Performance **dropped significantly**.
- Model often hallucinated, overexplained, or deviated.
- Low F1 and BLEU due to verbose, unmatched generations.

### Chain-of-Thought + Memory
- Memory helped **reduce hallucinations** and align with context.
- F1 improved from 11% → 23%.
- Still more verbose than ZS, but more grounded.

---

##  Key Takeaways

- Memory **positively impacts** CoT-style prompting by providing continuity.
- CoT alone can degrade performance unless guided by memory or clearer prompts.
- Zero-Shot remains the most efficient for direct QA tasks on short context.

---

## Limitations

1. **Small Sample Size**: Only 10 questions were used for quick evaluation.
2. **FLAN-T5 Base**: A larger model (`flan-t5-large`) would reason better.
3. **Prompt Sensitivity**: CoT output varies significantly with prompt wording.
4. **Evaluation Bias**: Long, reasoning-style answers scored poorly under exact-match metrics.
5. **Memory Simplicity**: Memory grows linearly with Q/A pairs; not clustered or deduplicated.

---

## Conclusion

This experiment demonstrates that while CoT prompting introduces complexity, its combination with memory provides value. Zero-shot prompting remains a reliable and efficient strategy, especially on short-form factoid QA.
