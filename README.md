# Milestone 1

## Dataset  
Our dataset consists of transcriptions from the YouTube channel **Kareem Esmail**, which appears to be a religious channel. Each text file in the dataset corresponds to a single episode, providing structured spoken content for analysis. The chosen NLP task for this dataset is **sentiment analysis**, allowing us to explore and classify the emotional tone conveyed throughout these episodes.

## Data Preprocessing  

1. **Chunking**  
   We applied a chunking technique that divides the text into smaller, more manageable segments. This was necessary due to the length and structure of the episode transcriptions, which can be difficult to analyze as a whole. We implemented chunking by splitting the text based on 3 lines, maintaining coherency. We applied this function to our dataset using Pandas, storing the resulting chunks in a new column.Then we used the explode function to transform the chunked text into separate rows.

2. **Clustering**  
   We applied clustering to group similar segments based on their content and sentiment. Using **TF-IDF vectorization**, we transformed the text into numerical representations and then applied **K-Means clustering** with seven clusters. Each cluster was assigned a theme and an emotion. This allowed us to categorize the dataset into meaningful sentiment-driven themes.

3. **Normalization**
   We applied text normalization to standardize variations and improve processing. The normalize_arabic function removes diacritics, unifies Alef (إ، أ، آ → ا), normalizes ؤ → و and ئ → ي, converts ة → ه, and removes punctuation. This step ensures consistency, reducing variations that could affect NLP tasks.

4. **Tokenization** 
   To break down the text into manageable units, we applied tokenization using **CAMeL Tools' `simple_word_tokenize`**. This method splits the text into individual words, making it easier to analyze and process.

5. **Stopword Removal**
   We removed Arabic stopwords using NLTK to focus on important words. We downloaded Arabic stopwords, loaded them as a set, and used `remove_stopwords` to filter them out. This helps make the text cleaner and more meaningful for analysis.
   
6. **Lemmatization**
   We applied lemmatization using **CAMeL Tools' `morphological analyzer`** for Modern Standard Arabic (MSA), reducing words to their root forms for consistency. The unique vocabulary size dropped from 22,022 to 11,448, eliminating 10,574 redundant variations.
Frequent words like "انا" → "أَنَى", "الاعتقاد" → "ٱِعْتِقاد", and "عامله" → "عامِل" were standardized. However, some words like "ياخدوا" and "فرهده" remained unchanged, highlighting limitations with dialectal terms.
Overall, lemmatization improved text consistency, reducing redundancy while preserving meaning, enhancing dataset quality for sentiment analysis.

## Data Analysis  

### Word Cloud  
We visualized word distribution using a word cloud, which highlights the **most frequent words** in a more intuitive way. We used the Noto Naskh Arabic font to ensure proper Arabic text rendering. This visualization helped us quickly identify dominant terms and recurring patterns.

### Word Frequency  
To gain insights into the most commonly used words in the transcriptions, we conducted a word frequency analysis using the Counter module. By aggregating all words from the cleaned transcriptions, we identified the **top 20 most frequent words**, which provided an overview of recurring themes in the dataset. 

### Bigrams
Bigrams help us identify common word pairs that provide better context for understanding sentiment-related expressions. Since podcasts contain natural, conversational speech, analyzing single words alone might miss important meaning. Bigrams allow us to detect patterns which offer deeper insights into how opinions are expressed. By extracting and analyzing these frequent bigrams, we can better understand recurring themes and sentiment trends in podcast discussions.

### Trigrams  
Trigrams further enhance our analysis by capturing three-word sequences that reveal even more detailed sentiment expressions. Many opinions in Arabic podcasts are conveyed through structured phrases. By identifying frequent trigrams, we can uncover common phrases that shape audience sentiment and highlight how opinions are typically framed in podcast conversations. This helps us gain a clearer picture of how different topics and discussions are received by listeners. 

### Sentiment Analysis Using CAMeL-BERT
We integrated the **`CAMeL-Lab/bert-base-arabic-camelbert-da-sentiment`** model for sentiment analysis. The model was loaded using the transformers library, and we applied it to our dataset to classify sentiment. The implementation tokenizes Arabic text and predicts sentiment labels based on the highest logit value.
The last visualizations in the notebook illustrate the distribution of sentiment predictions across our two manually labelled categorical variables: themes and emotions. Each bar represents a category, with segments indicating the count of predictions for each sentiment class.
1. The first graph shows the distribution of predicted sentiment across different themes, such as "Encouraging," "Inspirational," and "Reflective," providing insights into how sentiment aligns with various thematic categories.
2. The second graph presents the distribution of predicted sentiment across labelled emotions, such as "Anxiety," "Motivation," and "Calm," offering an evaluation of how sentiment predictions correspond to specific emotions.

The sentiment predictions are categorized into three distinct values:
- 0 (Blue): Negative or low-intensity sentiment.
- 1 (Gray): Neutral or moderate sentiment.
- 2 (Red): Positive or high-intensity sentiment.
  
These visualizations indicate that our chunking and categorization align well with the sentiment classification model, as the predicted sentiment distributions across themes and emotions follow logical and expected patterns. The alignment suggests that our preprocessing effectively preserves the emotional and thematic context, ensuring meaningful sentiment predictions.

## Limitations

- **Incomplete Stopword Removal**  
  The stopword removal process relies on NLTK’s Arabic stopword list, which does not cover all stopwords in Arabic. As a result, some common stopwords may still remain in the text, potentially affecting the accuracy of the sentiment classification.

- **Loss of Context in Chunking**  
  The chunking method splits text into segments based on three lines. While this maintains coherence to some extent, some contextual relationships between sentences may be lost, impacting the sentiment analysis results.

- **Potential Misclassification in Clustering**  
  The K-Means clustering algorithm assumes that the data is well-separated into the predefined number of clusters. However, sentiment is often nuanced and does not always fit neatly into distinct clusters. This may lead to some misclassifications, especially if different emotional tones appear within the same cluster.

- **TF-IDF’s Lack of Context Awareness**  
  TF-IDF vectorization captures word importance based on frequency but does not account for the semantic meaning of words or their order in a sentence. Sentiment analysis often depends on context, negation, and nuanced phrasing, which TF-IDF cannot fully capture. This limitation may result in misclassifications, especially in cases where sentiment is expressed through idioms, sarcasm, or negation.

