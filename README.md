# Can Unsupervised Clustering Recover Sentiment from Vietnamese Text?

An exploratory study of whether unsupervised text representations and clustering can recover sentiment-related structure in Vietnamese customer reviews. The notebook groups reviews by vector similarity; it does not establish that those groups represent positive, neutral, or negative sentiment.

## Contents

- [Research Question](#research-question)
- [Current Notebook](#current-notebook)
- [Data](#data)
- [Interpretation and Limitations](#interpretation-and-limitations)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Future Work](#future-work)
- [License](#license)

## Research Question

> Can unsupervised text representations and clustering recover sentiment-related structure without using sentiment labels during fitting?

Clustering optimizes similarity in a representation space, not sentiment. A cluster can instead reflect topic, vocabulary, writing style, text length, or other properties. Cluster separation is therefore not evidence by itself that sentiment has been recovered.

## Current Notebook

The notebook is an initial prototype. Its current workflow trains Word2Vec embeddings on review text, constructs review vectors, applies PCA, and clusters the vectors with K-means using cosine distance. It sets the number of clusters to three, but this is a modelling choice, not evidence that the data contains three sentiment classes.

The notebook also projects vectors to two dimensions for visualization. The plotted coordinates are only a projection and cannot validate cluster meaning. Cluster IDs are arbitrary and must not be called negative, neutral, or positive without independent evaluation.

The current implementation and its outputs have not yet been validated. In particular, preprocessing and review-vector construction should be checked before interpreting any clusters.

## Data

The notebook expects an external Excel file with `comment` and `comment_1` columns. The data is not included in this repository, and its source, licence, sample count, and label availability are not documented here. Those details must be established before the experiment can be reproduced or evaluated.

If sentiment labels are available, they should be kept out of representation learning and clustering and used only for clearly identified post-hoc evaluation. If labels are unavailable, a manually annotated validation subset would need a documented sampling and annotation procedure.

## Interpretation and Limitations

- The notebook does not currently use sentiment labels or measure cluster agreement with sentiment.
- Internal clustering metrics and plots can describe geometric structure, but cannot establish sentiment validity.
- Topic, vocabulary, writing style, and other signals may dominate polarity.
- Results are not reproducible from this repository alone because the input data is absent and its provenance is unspecified.

This repository is an exploratory prototype, not a validated sentiment classifier. Any sentiment interpretation should be treated as a hypothesis until supported by independent evidence.

## Getting Started

### Prerequisites

- A Python environment with the packages listed in `requirement.txt` (the notebook may require additional packages for data loading).
- [Jupyter Notebook](https://jupyter.org/).
- An Excel file of Vietnamese product reviews with `comment` and `comment_1` columns. The data file is not included.

### Installation

```bash
pip install -r requirement.txt
```

### Usage

Make the expected Excel data file available at the path used by the notebook, then open and run the notebook cell-by-cell:

```bash
jupyter notebook Unsupervised_sentiment_analysis.ipynb
```

The notebook imports the reviews, trains Word2Vec, builds review vectors, reduces dimensions with PCA, clusters with K-means, and plots the resulting cluster assignments. The plot does not provide sentiment labels or validate sentiment.

The Word2Vec and K-means parameters, including `W2V_SIZE`, `W2V_WINDOW`, `W2V_EPOCH`, `W2V_MIN_COUNT`, `SEED`, `NUM_CLUSTERS`, and `iteration`, are defined directly in the notebook. These are prototype settings, not validated or optimized values.

## Project Structure

```text
.
├── Unsupervised_sentiment_analysis.ipynb   # Exploratory notebook
├── requirement.txt                         # Python dependencies
├── README.md
└── LICENSE
```

## Future Work

- Document the dataset source, licence, preprocessing, and label availability.
- Audit and correct the existing vectorization workflow before interpreting results.
- Evaluate cluster stability across repeated runs and modelling choices.
- Compare clusters with held-out sentiment labels, if available, or a manually annotated subset.
- Inspect representative and contradictory reviews to assess topic-sentiment entanglement.
- Compare a small number of motivated text representations and clustering methods.

## License

This project is licensed under the [MIT License](LICENSE).
