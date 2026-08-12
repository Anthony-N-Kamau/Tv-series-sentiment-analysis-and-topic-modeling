# TV Series Analysis: Sentiment Analysis & Topic Modeling

## Research Question

> How does audience sentiment differ between *Game of Thrones* and *Better Call Saul* during their penultimate seasons, and what does this reveal about how viewers respond as a series approaches its finale?

## Overview

This project compares audience sentiment and discussion topics on Reddit for *Game of Thrones* (penultimate season, 2017) and *Better Call Saul* (penultimate season, 2020). It combines lexicon-based and transformer-based sentiment analysis with topic modeling to examine both the emotional tone and the dominant themes in viewer discourse as each show neared its finale.

**Data:** ~50,000+ Reddit posts about popular TV series (`Reddit_TV_Discussions.csv` / `Reddit_Data.csv`), filtered down to *Game of Thrones* (2017) and *Better Call Saul* (2020) posts.

## Repository Contents

| Notebook | Description |
|---|---|
| `Preprocessing_Topic-modeling.ipynb` | Data loading, cleaning (URL removal, HTML entity decoding, whitespace normalization, deduplication), exploratory analysis (posts per title/year), word clouds, word-frequency analysis, and BERTopic topic modeling for both shows. |
| `VADER_analysis.ipynb` | Lexicon-based sentiment scoring with VADER (`vaderSentiment`), producing compound scores and pos/neu/neg labels, plus sentiment-over-time and per-show comparisons. |
| `BertBase_got_bcs.ipynb` | Sentiment classification using `nlptown/bert-base-multilingual-uncased-sentiment` (BERT Base) on the cleaned posts. |
| `RoBert_got_bcs.ipynb` | Sentiment classification using `cardiffnlp/twitter-roberta-base-sentiment-latest` (RoBERTa, fine-tuned on Twitter data) via batched inference with softmax class probabilities. |

## Methodology Summary

1. **Preprocessing:** Removal of URLs, HTML entities, and redundant whitespace; lowercasing, stop word removal, and punctuation removal for the word-frequency/topic-modeling pipeline. No train/test split was used since all models are pre-trained and not fine-tuned on this dataset.
2. **Sentiment Analysis:**
   - **VADER** — rule-based baseline suited to informal social media text.
   - **BERT Base (multilingual)** — transformer baseline; found to default heavily toward one class due to a mismatch with informal Reddit language.
   - **RoBERTa (CardiffNLP, Twitter-tuned)** — produced more balanced, context-aware sentiment classifications.
3. **Topic Modeling:** BERTopic was applied to the cleaned posts to identify latent themes, followed by manual qualitative review to consolidate topics into broader categories.

## Key Findings

- Both shows are dominated by **neutral** Reddit sentiment overall, consistent with Reddit's analytical/speculative discussion style.
- **VADER:** *Better Call Saul* skews slightly positive (mean 0.037), *Game of Thrones* skews slightly negative (mean −0.021).
- **BERT Base** struggled with informal, sarcastic Reddit language and defaulted heavily toward negative/neutral classes.
- **RoBERTa** gave more balanced results, reinforcing that *Better Call Saul* discussions are more neutral/reflective while *Game of Thrones* discussions are more emotionally charged.
- **Topic modeling:** *Game of Thrones* discussions cover plot events, character judgment, and joking commentary, and are scattered and emotional. *Better Call Saul* discussions focus on character growth, moral change, and scene interpretation, and are more reflective and coherent.
