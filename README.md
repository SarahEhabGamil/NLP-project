# Milestone 1

## Dataset  
We analyzed transcriptions from the YouTube channel **Kareem Esmail**, a religious podcast. The NLP goal is to perform **sentiment analysis** to understand the emotional tone across episodes.  

## Data Preprocessing  

1. **Chunking**  
   - Divided transcriptions into **3-line segments** for better analysis.  
   - Used Pandas to store chunks in a new column and then transformed them into separate rows.  

2. **Clustering**  
   - Applied **TF-IDF vectorization** to convert text into numerical form.  
   - Used **K-Means clustering (7 clusters)** to group similar segments by both sentiment and theme.  

3. **Normalization**  
   - Standardized text by:  
     - Removing diacritics
     - Unifying (إ، أ، آ → ا)  
     - Unifying ؤ → و and ئ → ي  
     - Unifying ة → ه
     - Removing punctuation 

4. **Tokenization** 
   - Used **CAMeL Tools' `simple_word_tokenize`** to split text into individual words.  

5. **Stopword Removal**  
   - Filtered out **common Arabic stopwords** using NLTK (downloaded a set of arabic stop words).  

6. **Lemmatization**  
   - Extracted **root forms** of words using **CAMeL Tools** to ensure consistency.  
   - Focused on **nouns and verbs**, as they carry the most meaning.

## Data Analysis  

### 🔹 Word Cloud  
- **Visualized frequent words** to highlight key themes using *Noto Naskh Arabic* font.  

### 🔹 Word Frequency  
- Identified the **top 20 most used words** in transcriptions.  

### 🔹 Bigrams & Trigrams  
- **Bigrams**: Extracted common **two-word phrases** to identify sentiment patterns.  
- **Trigrams**: Captured **three-word sequences** to uncover structured sentiment expressions.    
