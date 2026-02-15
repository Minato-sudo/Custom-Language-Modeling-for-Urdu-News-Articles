# 🗞️ Urdu News NLP: Language Modeling & Generation

An end-to-end Natural Language Processing (NLP) pipeline for Urdu news articles, featuring automated scraping, linguistic preprocessing, and n-gram based language modeling.

## 🚀 Project Overview

This project implements a complete pipeline to:
1.  **Scrape** 218+ articles from BBC Urdu using Selenium and Sitemap strategies.
2.  **Preprocess** Urdu text (diacritics removal, noise reduction, sentence segmentation, tokenization, and lemmatization).
3.  **Model** Urdu language using Unigram, Bigram, and Trigram models with Add-k smoothing and linear interpolation.
4.  **Evaluate** performance using Perplexity metrics across different pipelines.
5.  **Generate** synthetic Urdu news content and headlines.

## 📈 Performance Benchmarks

By standardizing evaluation with a consistent smoothing parameter ($k=0.0001$), we demonstrate the significant impact of the cleaning pipeline:

| Model | Raw Pipeline PPL | Cleaned Pipeline PPL | Improvement |
| :--- | :---: | :---: | :---: |
| **Bigram** | 470.52 | **354.75** | **~25%** |
| **Trigram** | 164.68 | **123.58** | **~25%** |

## 🛠️ Components

- `i23-2548_Assignment1_DS-A.ipynb`: Core implementation and analysis.
- `Project_Report.md`: Technical documentation and results discussion.
- `plots.png`: High-resolution performance and frequency visualizations.
- `cleaned.txt` / `raw.txt`: Processed and original corpora.

## 🛠️ Installation

```bash
pip install selenium beautifulsoup4 numpy matplotlib
```

## 📜 Acknowledgments
Developed as part of the NLP Assignment 1.
