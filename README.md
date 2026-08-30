# 🎵 Vietnamese Song Lyrics Analysis & Topic Modeling (1900-2025)

## 📖 Project Overview
This project applies **Quantitative Historical Analysis** and **Text Mining** to a comprehensive dataset of Vietnamese song lyrics spanning from 1900 to 2025. 
The system automates the process of cleaning, tokenizing, and clustering lyrics into semantic topics to analyze the evolution of Vietnamese musical trends over time.

### ✨ Key Features
- **Vietnamese NLP Pipeline**: Utilizes VnCoreNLP for Word Segmentation, POS Tagging, and Named Entity Recognition (NER).
- **Topic Modeling & Clustering**: Implements and evaluates five different Machine Learning and Deep Learning algorithms for semantic clustering of lyrics. The **CTM (Contextualized Topic Models)** is selected as the primary model.
- **Phrase Detection**: Automatically detects semantic phrases and collocations (e.g., "tình_yêu" - love) using Gensim.
- **Data Visualization**: Employs advanced visualization techniques including Stacked Area charts and Small Multiples to display historical trends and composer profiles.

## 🧠 Algorithms Used
This project focuses on **Topic Modeling**, a specialized form of text clustering. While Topic Modeling shares the clustering goal of grouping similar documents, it operates as a "soft clustering" approach, allowing for the discovery of latent topics and probabilistic topic distributions for each song.

The following five algorithms were implemented and compared:
1. **LDA (Latent Dirichlet Allocation)**: A traditional generative probabilistic model based on Dirichlet distributions.
2. **NMF (Non-negative Matrix Factorization)**: A matrix factorization technique operating on TF-IDF features.
3. **LSA (Latent Semantic Analysis)**: A dimensionality reduction method utilizing TruncatedSVD.
4. **BERTopic (Embedding + Clustering)**: A modern approach that natively utilizes **Clustering**. BERTopic embeds lyrics using Sentence-BERT, reduces dimensionality via UMAP, and performs clustering using **HDBSCAN**.
5. **CTM (Contextualized Topic Models) - Selected Model**: An advanced neural network-based algorithm (Variational Autoencoder). CTM combines the interpretability of traditional Bag-of-Words with the semantic power of Contextual Embeddings (from pre-trained BERT) to achieve the most accurate topic predictions for Vietnamese texts.

## 🛠 System Requirements
The project is optimized for execution on **Google Colab (Linux environment)**.

- **Python**: 3.10+
- **Java (JDK)**: OpenJDK 11 (Required for VnCoreNLP).
- **Hardware**: GPU (Tesla T4 or better) is highly recommended for BERT/CTM embeddings computation.

**Core Python Dependencies:**
- `py-vncorenlp` (VnCoreNLP Python Wrapper)
- `contextualized-topic-models` (CTM)
- `bertopic`, `sentence-transformers`
- `gensim` (LDA, NMF, Coherence)
- `scikit-learn`, `numpy`, `pandas`
- `matplotlib`, `seaborn`, `plotly`

## 📁 Project Structure
```text
├── report.docx, report.pdf, ...       # Project reports, posters, and presentations
├── source_code/
│   ├── source.ipynb                   # Main source code (Jupyter Notebook)
│   ├── data/                          # Data directory (CSV, models, cache)
│   │   ├── final_dataset_v3.csv                 # (Input) Raw dataset
│   │   ├── vietnamese-stopwords.txt             # (Input) Stop words list
│   │   ├── final_dataset_v3_preprocessed.csv    # (Output) Cleaned dataset
│   │   ├── final_dataset_with_topics.csv        # (Output) Dataset with assigned topics
│   │   └── embeddings_cache.npy                 # (Output/Cache) Cached embeddings
```

## 🚀 Usage Instructions
The project is structured as a sequential Jupyter Notebook. Please execute the cells in the following order:

### Step 1: Setup & Data Ingestion
- Run the utility script to download the dataset and stopwords from Google Drive.
- The script automatically verifies/installs OpenJDK 11 and downloads the VnCoreNLP model JAR files if missing.
- Initializes the VnCoreNLP Java server.

### Step 2: Preprocessing Pipeline
- **Cleaning**: Removes special characters, HTML tags, and numerical digits.
- **Tokenization**: Segments Vietnamese words using VnCoreNLP.
- **Filtering**: Retains only Nouns, Verbs, and Adjectives; eliminates predefined Stopwords.
- **Phrase Detection**: Merges highly co-occurrent bigrams using Gensim Phrases.
- **Output**: `final_dataset_v3_preprocessed.csv`

### Step 3: Topic Modeling (Training)
- The notebook contains implementations for LSA, LDA, NMF, BERTopic, and CTM.
- Execute the **CTM (Contextualized Topic Models)** block for the final analysis.
- **Grid Search**: Evaluates topic counts (K=6 to 31) to determine the optimal Coherence Score.
- **Inference**: Assigns the dominant topic and probability to each song.
- **Output**: `final_dataset_with_topics.csv`

### Step 4: Visualization & Analytics (Final Report)
- Execute the final visualization blocks to generate analytical charts:
  1. Confidence Score Distribution.
  2. Genre Distinctiveness (Boxplot).
  3. Top Composers across 20 topics.
  4. Macro Evolution (Stacked Area Chart for 6 primary groups).
  5. Small Multiples (Detailed timelines for all 20 topics).

### Step 5: Cleanup
- Execute `close_vncorenlp()` to safely terminate the Java process and free up memory.

> **💡 TIP (Time-Saving Options):**
> You can bypass intermediate processing stages if you have precomputed data or cache files available:
> 
> *   **[CASE A]** Preprocessed data available (`final_dataset_v3_preprocessed.csv`): Run Step 1 -> Upload file to Colab -> Skip Step 2 -> Run Step 3.
> *   **[CASE B]** Cache available (`embeddings_cache.npy`) AND preprocessed CSV *(Recommended)*: Run Step 1 -> Upload both files -> Run Step 3 (The model will load the cache, bypassing the expensive embedding generation step).
> *   **[CASE C]** Final results available (`final_dataset_with_topics.csv`): Run Step 1 -> Upload file -> Skip Steps 2 & 3 -> Run Step 4 directly.

## 🏷 Topic Labels
The model optimally categorizes the lyrics into 20 distinct themes:
- **00**: Letting Go & Healing
- **01**: Lyrical Bolero & Melancholic Fate
- **02**: War, History & The Nation
- **03**: Tet Atmosphere & Spring Reunion
- **04**: Betrayal & Heartbreak
- **05**: Nature's Beauty & Homeland
- **06**: Romantic Emotions & Season of Love
- **07**: Southern Folk & Mekong Delta
- **08**: Family Affection & Parents
- **09**: Loneliness & Tears
- **10**: Waiting & Missing Lover
- **11**: Rap/Hip-hop & Lifestyle
- **12**: Sea & Islands
- **13**: Rain, Cold Nights & Memories
- **14**: Religious Faith & Prayer
- **15**: Happy & Sweet Love
- **16**: Social Realities & Materialism
- **17**: Memories & Love Letters
- **18**: Autumn, Hanoi & Nostalgia
- **19**: School Age & Summer

## ⚠️ Important Notes
- **Reproducibility**: The pipeline utilizes `set_seed(42)` to ensure consistent results across different runs.
- **Memory Requirements**: VnCoreNLP is memory-intensive and requires a substantial Java Heap allocation (~4GB).
