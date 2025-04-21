
In this milestone, we built a shallow neural network model to solve a question answering task using a benchmark dataset. The aim was to create a simple yet functional deep learning model from scratch (without using pre-trained models like BERT), and evaluate how well it could answer questions from a given context.
The core objective wasn’t to maximize performance but to demonstrate our understanding of the task, dataset, architecture design, training choices, and evaluation process.

Dataset Selection and Preprocessing : 

Dataset: Stanford Question Answering Dataset (SQuAD v1.1)
We chose the SQuAD dataset because it is a widely recognized benchmark for machine reading comprehension and question-answering tasks. It provides a paragraph (context), a question, and an answer that lies within the context.

Due to computational limits, we used a subset of the dataset:
- Size: Between 5,000 and 20,000 samples
- This keeps training manageable while still providing enough examples for the model to learn meaningful patterns.

Preprocessing Steps
- Tokenization: The text was tokenized using 'nltk.word_tokenize'.
- Padding: Ensured consistent input length using 'pad_sequences'.
- Vocabulary Construction: We built a vocabulary from the dataset and converted tokens to indices.
- Glove Embeddings: Instead of training embeddings from scratch, we used pre-trained GloVe embeddings to speed up learning and improve semantic understanding.

 Model Architecture: 

We implemented a shallow neural network using PyTorch, it consists of:

 Embedding Layer:
- Loaded pre-trained GloVe vectors.
- Converts tokenized inputs into vector representations.

Bidirectional LSTM:
- Captures dependencies in both forward and backward directions in the text.
- Provides better contextual understanding.

Fully Connected Layer:
- Maps LSTM outputs to the start and end positions of the answer span in the context.

Loss Function:
- Cross-entropy loss was used separately for both start and end indices.
- This allows the model to learn where the correct answer span begins and ends.

Optimizer:
- Adam optimizer was used with default learning rate, as it adapts the learning rate during training and performs well on NLP tasks.

Training Configuration:

- Epochs: 5
- Batch Size: 32
- Metrics Tracked: Training and validation loss, training and validation accuracy (start and end position separately)

We focused on tracking:
- How well the model reduces loss over time.
- Whether accuracy improves for identifying the correct start and end positions.

 Step by Step Implementation:

Step 1: Load and Explore Dataset
- Loaded a subset from SQuAD.
- Parsed the context, question, and answer text.
- Extracted start and end token positions for the answers.

Step 2: Text Preprocessing
- Cleaned and tokenized the input using NLTK.
- Built a word-to-index mapping.
- Padded sequences for consistent input shapes.

Step 3: Load GloVe Embeddings
- Used 100-dimensional GloVe vectors.
- Created an embedding matrix matching our vocabulary.

Step 4: Model Definition
- Defined a PyTorch 'nn.Module' class with an embedding layer, BiLSTM, and two linear layers for predicting the start and end positions.

Step 5: Training Loop
- Iterated through epochs and batches.
- Calculated loss and accuracy.
- Stored performance metrics for each epoch.

Step 6: Evaluation and Plotting
- Plotted accuracy and loss over epochs.
- Compared training vs validation performance.

 Results and Analysis

Epoch | Train Loss | Val Loss | Train Acc Start (%) | Train Acc End (%) | Val Acc Start (%) | Val Acc End (%)
1 | 6.10 | 7.58 | 13.3 | 14.48 | 7.68 | 8.72
2 | 5.93 | 8.07 | 13.8 | 15.36 | 8.20 | 9.21
3 | 5.87 | 7.96 | 13.84 | 15.60 | 8.55 | 10.30
4 | 5.81 | 7.83 | 14.48 | 15.50 | 8.44 | 10.30
5 | 5.75 | 7.89 | 15.12 | 16.06 | 8.83 | 9.70

Here’s what we observed:

Training/Validation Loss and Accuracy
From the graphs and epoch outputs:

Loss:
- Train Loss:
  - Decreased consistently from 6.09 in Epoch 1 to 5.74  in Epoch 5.
- Validation Loss:
  - Hovered around 7.5 to 8.0, peaking slightly at Epoch 3.
  - Slight improvement at Epoch 5 (~7.9)

- The training loss consistently decreased, showing the model was learning.
- Validation loss didn’t decrease much, indicating slight overfitting or limitations of a shallow model on this complex task.

Accuracy:
- Train Start Accuracy: Increased from 13.30% to 15.12%
- Train End Accuracy: Increased from 14.48% to 16.16%
- Validation Start Accuracy: Increased from 7.68% to 8.83%
- Validation End Accuracy: Increased from 8.72% to 9.70%

 Graph Analysis:
- Train accuracy gradually improved, with a ~2% gain for both start and end predictions.
- Validation accuracy also improved, but with a smaller margin (~1–1.5%).

- The model is learning but not generalizing well, which is expected for a shallow model and no fine-tuning.
- The gap between training and validation accuracy/loss suggests that the model may be memorizing training patterns more than learning to generalize.

 Justification of Design Choices

1. Why a Shallow Model?
- The milestone objective was to build a shallow model from scratch, so we avoided deep transformers or pre-trained QA models.

2. Why GloVe?
- GloVe embeddings offer semantic-rich word vectors, helping improve understanding of context and question with limited compute resources.

3. Why BiLSTM?
- A shallow BiLSTM balances complexity and performance. It captures bidirectional context (unlike basic RNNs) but is lighter than transformers.

4. Why Track Start and End Separately?
- Most QA datasets provide span-based answers. Accurately predicting both the start and end of the span is crucial.

Some of the Libraries we used in the project, their purpose and the reason why we chose them.

1. TensorFlow (specifically Keras) was used for building and training the model due to its simple and intuitive API, which makes it ideal for quick prototyping and experimentation. 

2. NumPy played a key role in handling data manipulation and performing efficient array operations, especially for preparing input data and managing sequences. 

3. Matplotlib was employed to generate plots for tracking loss and accuracy across epochs and to visualize the training progress, providing valuable insights into the model's learning behavior.

4. GloVe embeddings were incorporated to enrich the model with pre-trained word representations, enabling better semantic understanding of the input text without requiring a more complex or computationally heavy model.

Conclusion

We successfully implemented a shallow neural network model for the question answering task using the SQuAD dataset. Our model used pre-trained embeddings and a BiLSTM layer to predict start and end positions of answers within a given context.

- Our model learned to reduce training loss and slightly improved accuracy across 5 epochs.
- Validation accuracy showed marginal improvements, indicating the challenge of generalizing with a shallow model.
- GloVe embeddings helped boost learning without excessive training time.
- A deeper model or attention mechanism could help improve performance further.