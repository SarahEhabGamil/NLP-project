# Milestone 1

## Dataset  
Our dataset consists of transcriptions from the YouTube channel **Kareem Esmail**, which appears to be a religious podcast. Each text file in the dataset corresponds to a single episode, providing structured spoken content for analysis. The chosen NLP task for this dataset is **sentiment analysis**, allowing us to explore and classify the emotional tone conveyed throughout these episodes.

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
   We applied lemmatization using **CAMeL Tools**' built-in morphological analyzer for Modern Standard Arabic (MSA). Lemmatization reduces words to their root forms, ensuring consistency across different word variations. We prioritized extracting lemmas for nouns and verbs, as they carry the most meaning. If no noun or verb was found, we selected the best available lemma.

## Data Analysis  

### Word Cloud  
We visualized word distribution using a word cloud, which highlights the **most frequent words** in a more intuitive way. We used the Noto Naskh Arabic font to ensure proper Arabic text rendering. This visualization helped us quickly identify dominant terms and recurring patterns.

### Word Frequency  
To gain insights into the most commonly used words in the transcriptions, we conducted a word frequency analysis using the Counter module. By aggregating all words from the cleaned transcriptions, we identified the **top 20 most frequent words**, which provided an overview of recurring themes in the dataset. 

### Bigrams
Bigrams help us identify common word pairs that provide better context for understanding sentiment-related expressions. Since podcasts contain natural, conversational speech, analyzing single words alone might miss important meaning. Bigrams allow us to detect patterns which offer deeper insights into how opinions are expressed. By extracting and analyzing these frequent bigrams, we can better understand recurring themes and sentiment trends in podcast discussions.

### Trigrams  
Trigrams further enhance our analysis by capturing three-word sequences that reveal even more detailed sentiment expressions. Many opinions in Arabic podcasts are conveyed through structured phrases like "لا أحب هذا". By identifying frequent trigrams, we can uncover common phrases that shape audience sentiment and highlight how opinions are typically framed in podcast conversations. This helps us gain a clearer picture of how different topics and discussions are received by listeners. 
