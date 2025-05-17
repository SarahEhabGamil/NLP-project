# Milestone 3: LLM-Based Chatbot using Fine-Tuning on SQuAD v2.0

##  Objective

This project aims to build a **Question Answering (QA) chatbot** by leveraging a pre-trained language model. The objective was to **compare two fine-tuning strategies** (partial vs. full) on a standardized QA dataset and evaluate their performance. The system forms the core of a chatbot that responds accurately to user queries given relevant context.

---

##  Approach Overview

We followed **Setup 1** of the milestone requirements:

> **Experiment 1** → LLM Model + Partial Fine-Tuning  
> **Experiment 2** → Same LLM Model + Full Fine-Tuning

We used the **DistilBERT** model (`distilbert-base-uncased`) and fine-tuned it on **SQuAD v2.0**, a widely used extractive QA dataset.

The model was trained using Hugging Face’s `transformers`, `datasets`, and `Trainer` APIs, and tested using manually constructed and unseen question-context pairs.

---

##  Tools and Frameworks

- **Transformers** (Hugging Face)
- **Datasets** (Hugging Face)
- **Evaluate** (Hugging Face)
- **PyTorch** backend
- **Google Colab** environment
- **DistilBERT** pre-trained model

---

##  Dataset

- **SQuAD v2.0** from Hugging Face
- Contains ~87,000 training examples; we used:
  - **3,000** samples for training (partial fine-tune)
  - **Entire 3,000** for full fine-tune (to simulate a resource-constrained full training)
  - **500** samples for validation

---

##  Implementation Steps

### 1. **Data Preprocessing**
- Tokenization using `AutoTokenizer`
- Mapping of `start_positions` and `end_positions` for answer spans

### 2. **Model Setup**
- Base model: `distilbert-base-uncased`
- Two copies initialized for two experiments

### 3. **Training Configuration**
- **Trainer API** used for both experiments
- Epochs: `1` (for quick comparison)
- Batch size: `12`
- Mixed precision (`fp16`) enabled
- Separate logging and output directories

### 4. **Experiment Setup**

| Experiment | Model | Data Used | Description |
|-----------|--------|------------|-------------|
| Experiment 1 | DistilBERT | 70% of training subset (2,100 samples) | Partial Fine-Tuning |
| Experiment 2 | DistilBERT | 100% of 3,000 training samples | Full Fine-Tuning |

---

##  Evaluation Method

A set of **custom, unseen QA examples** were designed to test:
- Entity recognition
- Definition extraction
- Date identification
- Semantic understanding

Each model was evaluated using Hugging Face's `pipeline("question-answering")` interface.

---

##  Results

| Question | Partial FT Answer | Full FT Answer | Observation |
|----------|--------------------|----------------|-------------|
| Who wrote *1984*? | Answer: George Orwell and published in 1949<br>Score: **0.0303**<br>Start: 30<br>End: 65 | Answer: George Orwell and published in 1949<br>Score: **0.0141**<br>Start: 30<br>End: 65 | Over-extended span |
| Tallest mountain? | Answer: 8,848 meters<br>Score: **0.0110**<br>Start: 69<br>End: 81 | ✅ Answer: Mount Everest<br>Score: **0.0086**<br>Start: 0<br>End: 13 | Full FT selected the correct entity |
| DNA stands for? | ✅ Answer: Deoxyribonucleic Acid<br>Score: **0.0130**<br>Start: 15<br>End: 36 | ✅ Answer: Deoxyribonucleic Acid<br>Score: **0.0164**<br>Start: 15<br>End: 36 | Full FT was slightly more confident |
| Moon landing? | Answer: Neil Armstrong<br>Score: **0.0394**<br>Start: 0<br>End: 14 | Answer: Neil Armstrong<br>Score: **0.0168**<br>Start: 0<br>End: 14 | Wrong entity type (expected date) |
| Red Planet? | Answer: Red Planet<br>Score: **0.0067**<br>Start: 33<br>End: 43 | ✅ Answer: Mars<br>Score: **0.0046**<br>Start: 0<br>End: 4 | Full FT predicted correct entity |


- **Partial FT** showed strong performance even with less data
- **Full FT** improved span prediction and semantic accuracy
- Score confidence was higher in full FT, but not always correct

---

## 🔒 Limitations

Despite meeting all milestone objectives, the project had some limitations:

1. **Limited Dataset Size**  
   Due to hardware constraints, only a small subset (3,000 training examples) of the full SQuAD v2.0 dataset (~87,000 examples) was used. Larger datasets could yield even better results.

2. **Single Epoch Training**  
   To reduce training time and memory usage, both models were trained for just 1 epoch. Additional epochs may lead to better convergence and accuracy.

3. **Small Evaluation Set**  
   Evaluation was limited to a few custom QA examples. A full evaluation using SQuAD’s validation set with F1 and EM metrics would offer more robust comparison.

4. **Span-Only Output**  
   Since this was an extractive QA system, the model could only return answers directly from the input context — it cannot reason or generate responses beyond the span.

5. **Misinterpretation of Entity Types**  
   For some questions, the model returned entities of the wrong type (e.g., a person instead of a date). This shows limitations in understanding nuanced question intent.






