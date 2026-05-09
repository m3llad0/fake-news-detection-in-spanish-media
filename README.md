# Fake News Detection in Spanish Media

Master's thesis project: a hybrid BETO + Graph Attention Network classifier for
distinguishing real, fake, and satirical news in Spanish.

This repository holds the labeled dataset and the training/evaluation notebook.
The scraping pipeline that produced the dataset lives in
[`fake-news-data-extraction/`](./fake-news-data-extraction).

<a href="https://colab.research.google.com/github/m3llad0/fake-news-detection-in-spanish-media/blob/main/notebooks/Fake_news_detection_model_Final_Version.ipynb" target="_parent">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

## Dataset

`data/scraped_news.csv` — 1,290 articles scraped from Spanish-language outlets.

| Column      | Description                                    |
|-------------|------------------------------------------------|
| `TITULO`    | Article headline                               |
| `CORPUS`    | Full article body                              |
| `AUTOR`     | Author or outlet                               |
| `FECHA`     | Publication date                               |
| `URL`       | Source URL                                     |
| `METADATA`  | Additional scraped metadata                    |
| `VERACIDAD` | Label: `true`, `false`, or `satira`            |

Class distribution:

| Label    | Count |
|----------|------:|
| `true`   |   433 |
| `satira` |   433 |
| `false`  |   424 |

The notebook also pulls in [FakeNewsCorpusSpanish](https://github.com/jpposadas/FakeNewsCorpusSpanish)
for data augmentation.

## Model

`notebooks/Fake_news_detection_model_Final_Version.ipynb` implements `HybridGAT`,
which combines:

- **BETO** (`dccuchile/bert-base-spanish-wwm-cased`) — `[CLS]` embeddings from
  the Spanish BERT model as 768-dim text representations.
- **Graph Attention Network** (`torch_geometric.nn.GATConv`) — two layers
  (768 → 64×4 heads → 64) propagate context across an article-similarity graph.
- **Auxiliary signal** — clickbait scores from `taniwasl/clickbait_es`.
- **Output** — 3-class softmax (`true` / `false` / `satira`), trained with
  focal loss and stratified k-fold CV.

The trained classifier is wrapped in a `FakeNewsClassifier` class with a
`predict()` interface.

## Repository structure

```
fake-news-detection-in-spanish-media/
├── data/
│   └── scraped_news.csv
├── notebooks/
│   └── Fake_news_detection_model_Final_Version.ipynb
├── fake-news-data-extraction/      # Flask scraping pipeline (separate component)
├── LICENSE
└── README.md
```

## Running the notebook

The easiest path is the Colab badge above — it provides the GPU needed by
BETO and `torch_geometric`. To run locally:

```bash
pip install torch torch-geometric transformers scikit-learn pandas numpy textblob matplotlib
jupyter notebook notebooks/Fake_news_detection_model_Final_Version.ipynb
```

A CUDA-capable GPU is recommended.

## License

MIT — see [`LICENSE`](./LICENSE).

## Author

Diego Mellado — *Spanish fake news detection in media* (master's thesis, 2026).
