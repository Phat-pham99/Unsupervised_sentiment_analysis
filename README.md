# Unsupervised Sentiment Analysis for Vietnamese

Unsupervised sentiment analysis of Vietnamese customer reviews using **Word2Vec**, **Sentence2Vec**, **PCA**, and **K-means clustering**. The pipeline learns sentiment polarity directly from unlabeled text — no annotated training data required.

![clustering-result](https://user-images.githubusercontent.com/57822898/161768865-f77def6c-8154-4b53-a1a7-fded5576c5fa.png)

> Visualization of the K-means result (3 clusters) projected onto 2 dimensions via PCA. Red: Negative, Green: Neutral, Blue: Positive.

## Table of Contents
- [Overview](#overview)
- [Pipeline](#pipeline)
- [Approach](#approach)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Usage](#usage)
- [Configuration](#configuration)
- [Results](#results)
- [Project Structure](#project-structure)
- [Future Work](#future-work)
- [License](#license)

## Overview

This project performs **sentiment analysis for Vietnamese** without relying on labeled data or an external sentiment lexicon. Instead, it:

1. Learns Vietnamese word embeddings from raw product reviews using **Word2Vec**.
2. Represents each review (sentence) as a fixed-length vector by averaging its word vectors — a **Sentence2Vec** approach.
3. Reduces the dimensionality of the sentence vectors with **PCA**.
4. Groups the sentences into sentiment categories using **K-means clustering** with cosine distance.

The result is a fully unsupervised classification of reviews into **Negative**, **Neutral**, and **Positive** sentiment.

## Pipeline

```text
Raw Vietnamese reviews
        │
        ▼
 Word2Vec (word embeddings)
        │
        ▼
 Sentence2Vec (mean of word vectors)
        │
        ▼
 PCA (dimensionality reduction)
        │
        ▼
 K-means clustering (cosine distance)
        │
        ▼
 Sentiment labels: Negative / Neutral / Positive
```

## Approach

### Word2Vec
A Word2Vec model (skip-gram style, applied with the Gensim library) is trained on tokenized Vietnamese reviews. It captures semantic relationships between words, e.g. the nearest neighbors of *tốt* ("good") include *rất_đẹp*, *rất_ưng*, *rất_thích*, *rất_tốt* — all strongly positive words, confirming that sentiment is encoded in the learned embeddings.

| Attribute   | Value  |
|-------------|--------|
| Vector size | 400    |
| Window      | 10     |
| Epochs      | 35     |
| Min count   | 5      |
| Seed        | 567    |

### Sentence2Vec
Each review is tokenized with the `underthesea` tokenizer, its word vectors are fetched from the Word2Vec model, and the sentence vector is computed as the **mean of the (summed) word vectors**, normalized by the vector size. A cosine-similarity method is provided to compare sentences.

### PCA
The high-dimensional (400-D) sentence vectors are projected down to 80 dimensions with PCA to improve clustering efficiency and reduce noise.

### K-means Clustering
K-means clustering is applied with **cosine distance**, normalization enabled, repeated across 20 attempts, with empty clusters disallowed. `NUM_CLUSTERS = 3` maps directly onto the three sentiment classes.

## Getting Started

### Prerequisites
- Python 3.7+
- [Jupyter Notebook](https://jupyter.org/)
- An Excel file of Vietnamese product reviews with a `comment` column (and a `comment_1` normalized column), loaded as `data`.

### Installation

```bash
pip install -r requirement.txt
```

### Usage

Open and run the notebook cell-by-cell:

```bash
jupyter notebook Unsupervised_sentiment_analysis.ipynb
```

The notebook walks through the full pipeline: importing the data, training Word2Vec, building sentence vectors, reducing dimensions with PCA, clustering with K-means, and finally plotting the results with the sentiment labels.

> **Note:** The `w2v.model` artifact is produced by training and then re-loaded via the `Sentence2Vec` class.

## Configuration

Key hyperparameters are defined at the top of the notebook:

| Constant        | Value | Description                                        |
|-----------------|-------|----------------------------------------------------|
| `W2V_SIZE`      | 400   | Word vector dimensionality                         |
| `W2V_WINDOW`    | 10    | Maximum distance between current and predicted word |
| `W2V_EPOCH`     | 35    | Number of training epochs                          |
| `W2V_MIN_COUNT` | 5     | Ignores tokens with frequency lower than this       |
| `SEED`          | 567   | Random seed for reproducibility                    |
| `NUM_CLUSTERS`  | 3     | Number of sentiment clusters                       |
| `iteration`     | 20    | K-means repeated restarts                          |

## Results

After clustering, each review is assigned a label (0, 1, or 2):
- **Label 0** — Negative (e.g., *"vải thô, không mềm, bị bạc"*)
- **Label 1** — Neutral (e.g., *"Size không chính xác, áo quá nóng"*)
- **Label 2** — Positive (e.g., *"Vải dày mịn áo y hình rất đẹp"*)

The final scatter plot (projected to 2D) visually separates the three sentiment groups, demonstrating that unsupervised clustering recovers sentiment structure from raw Vietnamese text.

## Project Structure

```text
.
├── Unsupervised_sentiment_analysis.ipynb   # Main notebook with the full pipeline
├── requirement.txt                         # Python dependencies
└── README.md
```

## Future Work

- Tune the number of clusters (e.g., via the silhouette score / elbow method).
- Experiment with other embedding approaches (e.g., FastText, BERT embeddings).
- Validate cluster quality against a small labeled subset.

## License

This project is licensed under the [MIT License](LICENSE).
