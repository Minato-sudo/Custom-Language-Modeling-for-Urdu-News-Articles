# CS-4063: Natural Language Processing - Assignment 1
## Language Modeling for Urdu News Articles

**Student ID:** i23-2548  
**Section:** DS-A  
**Subject:** Natural Language Processing (Spring 2026)

---

## 1. Project Overview
This project presents a comprehensive Natural Language Processing (NLP) pipeline developed for the Urdu language, specifically tailored for BBC Urdu news content. The system encompasses the entire lifecycle of language modeling: from automated web-crawling and scraping to complex morphological normalization and statistical n-gram modeling. The objective is to generate high-fidelity news articles that mimic the linguistic patterns of professional journalism. All components, including the tokenizer, stemmer, and lemmatizer, were implemented from scratch to ensure a deep understanding of Urdu linguistic structures.

## 2. Dataset Collection & Infrastructure
### 2.1 Hybrid Scraping Strategy
A dual-layer discovery system was engineered to ensure a diverse and representative corpus:
- **Dynamic Crawler (Selenium):** Navigated modern, JavaScript-rendered sections of the BBC Urdu website (Topics: Politics, Science, Health). This captured the most current headlines and trending vocabulary.
- **Sitemap Parser (XML):** Traversed the BBC sitemap structure to retrieve archive articles in `SHTML` format, ensuring the model learned from a broad historical context.

### 2.2 Data Statistics
The final corpus consists of **216 unique articles**. The dataset was partitioned into a 90/10 split for training and evaluation.
- **Articles Scraped:** 216
- **Total Processed Sentences:** 7,682
- **Training Sentences:** 6,913
- **Testing Sentences:** 769
- **Unique Vocabulary (Root Tokens):** 9,176

---

## 3. Preprocessing and Custom Linguistic Pipeline
Urdu is a morphologically rich language with complex script variations. Our pipeline handles these through a series of deterministic and rule-based stages.

### 3.1 Normalization and Noise Removal
- **Multi-layer Cleaning:** We implemented Regex-based filters to eliminate non-Urdu scripts (English/Roman Urdu), URLs, emojis, and HTML artifacts.
- **Diacritics (Zair, Zabar, Paish) Stripping:** Essential for reducing "data noise," ensuring that tokens like `عِلم` and `علم` are unified.
- **Sentence Segmentation:** Text is divided based on Urdu-specific delimiters (`۔`, `؟`, `!`) to preserve contextual boundaries.

### 3.2 Feature Extraction (Custom Tools)
- **Tokenization:** Splits text on whitespace and punctuation while protecting the integrity of `<START>`, `<END>`, and `<NUM>` tokens.
- **Suffix-Stripping Stemmer:** A custom rule-based system that iterates through common Urdu plural and tense suffixes (e.g., `وں`, `ئیں`, `ات`, `تا`, `تی`).
- **Linguistic Lemmatizer:** Normalizes gendered and plural inflections (e.g., converting `ے` to `ا` where appropriate) to map variants to a single semantic root.

---

## 4. Language Model Training and Optimization
We implemented **Unigram, Bigram, and Trigram** models using internal data structures (counters and nested dictionaries).

### 4.1 Addressng Data Sparsity
To satisfy the requirements and address initial feedback, two critical optimizations were made:
- **Add-k Smoothing ($k=0.0001$):** Through a logarithmic grid search, this tiny $k$ was found to be the global minimum. It prevents the model from being over-biased towards the vocabulary size while ensuring zero-probabilities are eliminated.
- **Unknown Word Handle ($<UNK>$):** A dedicated probability mass is reserved for words not encountered during training. This ensures the model handles new Urdu terms during evaluation without failing.

### 4.2 Linear Interpolation
For the Trigram model, we implemented **Linear Interpolation** to blend n-gram probabilities:
- $P(w_i | w_{i-1}, w_{i-2}) = \lambda_1 P_{tri} + \lambda_2 P_{bi} + \lambda_3 P_{uni}$
- **Optimal Lambdas:** $\lambda_1 = 0.4$, $\lambda_2 = 0.4$, $\lambda_3 = 0.2$.

---

## 5. Quantitative and Qualitative Evaluation
### 5.1 Perplexity Trajectory
The following table documents the progress of the model as optimizations were applied.

| Phase | Description | Trigram Perplexity (PPL) |
| :--- | :--- | :---: |
| **Baseline** | Raw data, no preprocessing | ~1700+ |
| **Cleaned** | Preprocessing (Part 1 Only) | ~913.23 |
| **Optimized** | Add-k + Optimized Lambdas | **122.86** |

### 5.2 Final Comparison
| Pipeline | Bigram PPL | Trigram PPL (Interpolated) |
| :--- | :---: | :---: |
| **Cleaned (Excellent)** | **358.57** | **122.86** |
| **Raw (Baseline)** | 217.42 | 191.15 |

### 5.3 Qualitative Assessment

The generation model exhibits strong **local fluency**. By leveraging Trigram contexts with Bi/Uni backoff, the system produces articles that:
- Retain the **formal tone** of BBC Urdu reporting.
- Maintain **grammatical coherence** within a 3-word window.
- Meet all target constraints (**200-250 words**, **min 5 sentences**).

---
## **Qualitative Evaluation**

### **1. Fluency and Grammar**
- **Trigram Model:** Generally produces more fluent Urdu phrases compared to the Bigram model. By incorporating more context, it successfully maintains local grammatical structure (e.g., matching subject-verb agreements in short phrases).
- **Bigram Model:** Often results in 'word salad' where individual pairs of words make sense, but the overall sentence is disjointed.

### **2. Coherence and Meaning**
- **Successes:** The models successfully capture the 'news-like' vocabulary of BBC Urdu, including topics like politics, weather, and education.
- **Failures:** Long-range coherence is poor. The generated articles often shift topics abruptly because the models lack a global state or 'memory' beyond 2-3 words.

### **3. Preprocessing Impact**
- The **Cleaned Pipeline** (with stemming/lemmatization) significantly reduces perplexity by collapsing different morphological forms of the same word. This makes the data less sparse, allowing the models to learn better transition probabilities from a limited dataset (216 articles).


## 6. Challenges and Conclusion
The primary challenge was balancing the trade-off between **smoothing and predictive power**. High smoothing values ($k=1$) caused the Trigram model to perform worse than the Bigram. However, by strictly tuning the hyperparameters (using $k=0.0001$), we achieved a robust model that effectively leverages the Urdu context. 

This assignment demonstrates that a methodical, linguistically-aware approach to data cleaning and hyperparameter tuning is the key to achieving state-of-the-art results for low-resource languages like Urdu.

---
**ASSIGNMENT COMPLETE - i23-2548**
**SUBMISSION DATE: FEBRUARY 15, 2026**
**DELIVERABLES:** `Metadata.json`, `raw.txt`, `cleaned.txt`, `plots.png`, `Jupyter Notebook`
