# AmazonCollaborativeBasedBookRecommendation
Here’s the updated **README.md** content based on your request. It explains the project clearly, includes both execution paths, and details your approach, including the dataset sampling strategy and motivation.

---

# 📚 Collaborative Book Recommendation System

This project implements a **collaborative filtering–based book recommender system** using real-world Amazon reviews. It includes both **item-based** and **user-based** recommendations and is powered by natural language processing and text vectorization using TF-IDF + SVD.

---

## 💡 About Collaborative Filtering

Collaborative Filtering (CF) is a popular technique for recommender systems that relies on **user behavior**, not content. The key idea is:

* If users **A** and **B** rated similar items similarly in the past,
* Then the system can recommend new items to **A** that **B** liked (and vice versa).

This project uses both:

* **Item-Based Filtering**: Recommend items similar to what the user liked.
* **User-Based Filtering**: Recommend items liked by similar users.

---

## 📁 Dataset Overview

We use the [`mohamedbakhet/amazon-books-reviews`](https://www.kaggle.com/datasets/mohamedbakhet/amazon-books-reviews) dataset (over 1 GB, 3M+ reviews). Each record contains:

* `User_id`: reviewer ID
* `Title`: book title
* `review/score`: user’s rating
* `review/summary`: short comment/summary
* Other metadata (ignored in this project)

Due to memory constraints, we do not load the entire dataset for modeling.

---

## 🧪 Two Execution Modes

### ✅ 1. Using a Sampled Dataset (Faster)

A **10% sampled and cleaned version** of the dataset is included in this repository:
📄 `data/ratings2_processed.csv`

Use the script `collaborative_recommender.py` to:

* Load and clean the sample
* Perform sentiment analysis & text vectorization
* Train item-based and user-based models
* Generate top recommendations

This is **ideal for testing or low-resource machines**.

### ⚙️ 2. Sampling & Processing From Scratch

You can also run the full pipeline yourself on the **original Kaggle dataset**.
In this mode:

* The system **downloads** the dataset via `kagglehub`
* **Samples 10%** of the records using `pandas.sample()`
* Applies **title normalization**, **fuzzy matching**, and **deduplication**
* Performs sentiment and text enrichment

> This pipeline ensures data cleanliness while reducing memory usage significantly.

You can switch between the two modes by using the CLI version or modifying the `main()` block in the script.

---

## 🧹 Data Cleaning Highlights

* 🧾 **Title normalization**: removes punctuation, text in parentheses, replaces `&` with `and`, lowercases.
* ✨ **Fuzzy title deduplication**: merges slight title variations using `RapidFuzz` with token sort ratio.
* 📖 **The Lord of the Rings**–like titles are normalized manually.
* 💬 **Summary text** is cleaned, tokenized, lemmatized (with NLTK), and analyzed using VADER sentiment scoring.

---

## 🔍 Feature Extraction

* 📘 **TF-IDF Vectorization** of cleaned summaries
* 🔽 **Truncated SVD**: reduces dimensionality from 1000 to 100 components
* 😊 **Sentiment scores**: added using NLTK VADER

---

## 🔄 Recommendations

### 📌 Item-Based

* Find **top 5 books** most similar (cosine similarity) to the target book’s vector representation

### 👥 User-Based

* Find **nearest users** using cosine similarity on the user-item matrix
* Recommend books **liked by similar users** that the target user hasn’t seen

All ratings are optionally weighted by user similarity distance for improved ranking.

---

## 📦 Dependencies

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn nltk scikit-learn rapidfuzz kagglehub
```

Also download NLTK resources at runtime:

```python
nltk.download("punkt")
nltk.download("stopwords")
nltk.download("wordnet")
nltk.download("vader_lexicon")
```

---

## ▶️ Running the Code

### 1. Run with Sampled Dataset

No Kaggle account/API key needed:

```bash
python collaborative_recommender.py
```

### 2. Full Dataset Processing

This will:

* Download the dataset from Kaggle
* Sample and clean the data
* Save `ratings2_processed.csv` for future reuse

Just run the same script — it will detect if the dataset exists and reuses it if available.

---

## 📌 Notes

* The fuzzy title cleaning step may take **10–20 minutes**, depending on your hardware.
* You can interrupt the process **after sampling and fuzzy deduplication**, and resume later using the generated CSV.

---

## 📊 Sample Output

```bash
Top‑5 similar books to first item:
 1. Huckleberry Finn (dist=0.122)
 2. The Scarlet Letter (dist=0.202)
 3. The Great Gatsby (dist=0.233)
...

Top recommendations for user A12345XYZ:
 - The Lord Of The Rings: 4.9
 - To Kill A Mockingbird: 4.7
...
```

---

## 💻 Author

Reihan — Computer Engineer & AI Enthusiast
For feedback or suggestions, feel free to reach out!

---

Would you like me to prepare a downloadable `README.md` file or push this to your GitHub repo via instructions or zipped content?
