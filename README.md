# Archelec — Topic Modeling of French Campaign Manifestos (1981–1993)

> NLP Project — ENSAE, Deep Learning for Natural Language Processing, 2026

## Summary

This project analyses the **campaign manifestos** (*professions de foi*) of candidates in the French legislative elections of **1981, 1988 and 1993** using topic modeling techniques (LDA, NMF, BERTopic). The goal is to extract dominant themes from political discourse, study their temporal evolution across three landmark elections of the Mitterrand era, and cross-reference these themes with candidate metadata (political affiliation, gender, profession).

The corpus comes from the [Archelec](https://archelec.sciencespo.fr/) project (Sciences Po / CEVIPOF), which gathers digitised electoral archives of the French Fifth Republic.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── data/                          # Data (not versioned)
│   ├── archelect_search.csv       # Metadata CSV (archelec.sciencespo.fr)
│   ├── txt_1981/                  # OCR transcriptions 1981
│   ├── txt_1988/                  # OCR transcriptions 1988
│   └── txt_1993/                  # OCR transcriptions 1993
│── 01_eda.ipynb                   # Loading, preprocessing, exploratory analysis
│── 02_main.ipynb                  # Topic modeling, metadata cross-analysis    
└── report.pdf
                    
```

## Methodology

### 1. Data and preprocessing

- **~12,500 documents** with rich metadata (party, gender, profession, department, etc.)
- OCR cleaning: boilerplate removal, hyphenation rejoining, whitespace normalisation
- Tokenisation and lemmatisation with spaCy (`fr_core_news_md`)
- Political affiliation mapping into 5 families + "Other/Unknown"

### 2. Topic Modeling

Three approaches compared, following the course curriculum:

| Method                       | Input                 | Approach                       |
| ---------------------------- | --------------------- | ------------------------------ |
| **LDA** (gensim)       | Bag-of-Words (counts) | Probabilistic generative model |
| **NMF** (scikit-learn) | TF-IDF                | Matrix factorisation           |
| **BERTopic**           | SBERT embeddings      | Embedding-based clustering     |

Evaluation using coherence metrics: c_v, u_mass, c_npmi.

### 3. Cross-analyses

- Topic distribution by political family (heatmaps)
- Party specialisation: ratio P(topic|family) / P(topic)
- Temporal evolution of topics (1981 → 1988 → 1993)
- Triple cross-tabulation: topics × family × year
- Topics × candidate gender
- Topics × candidate profession
- Supervised classification: predicting political family from text

## Key findings

- NMF produces the most coherent and interpretable topics, outperforming LDA and BERTopic on this corpus.
- Political discourse is highly segmented: each political family concentrates on a small set of distinct themes.
- Far-right, far-left and Green parties show strong thematic specialisation, while mainstream parties are more diffuse.
- Major shifts occur over time, with a rise in immigration and security issues and the emergence of environmental themes.
- Some topics change ownership across elections, reflecting evolving political dynamics.
- Topic distributions are consistent with candidate metadata, especially political affiliation and profession.

<!--
Drafting notes:
- NMF produces the most coherent topics (c_v = 0.74 vs 0.59 for LDA)
- BERTopic detects the "national template" phenomenon (identical tracts for FN, LO, Greens)
- Thematic evolution: rise of immigration theme between 81 and 93, disappearance of "union de la gauche"
- The classifier confusion matrix reveals which families "speak the same language"
-->

## Installation

```bash
pip install -r requirements.txt
python -m spacy download fr_core_news_md
```

## Data

Metadata can be downloaded from [archelec.sciencespo.fr/explorer](https://archelec.sciencespo.fr/explorer). OCR transcriptions are available via the GitLab repository [arkindex_archelec](https://gitlab.teklia.com/ckermorvant/arkindex_archelec) or directly from [archive.org](https://archive.org/) (URLs in the `ocr_url` column of the CSV).

## References

## Author

Antoine Gilson - ENSAE, 2026
