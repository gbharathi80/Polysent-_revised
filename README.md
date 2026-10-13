# PolySent-X: Revised Experimental Repository

This repository contains the revised implementation, audit results, and manuscript support files for:

**PolySent-X: A Multilingual Emotion and Topic Aware Framework for Multi-Horizon Stock Direction Prediction**

## What was corrected

The revision addresses the experimental concerns identified during editorial screening:

- Corrected the topic-score definition to match the implemented XLM-R/XNLI zero-shot method.
- Added chronological ordering and leakage-controlled expanding walk-forward validation.
- Added horizon-aware purging.
- Separated model selection from a final untouched chronological holdout.
- Added direct relevance-filter ablation.
- Added a restricted multilingual ablation with statistical testing.
- Added uncertainty estimates through bootstrap confidence intervals.
- Removed the incompatible near-perfect confusion matrix.
- Revised the interpretation to avoid claiming consistent predictive superiority.

## Core data audit

- Cleaned headlines: 10,310
- Topic-filtered/emotion-scored headlines: 3,183
- Daily observations: 1,724
- Companies: AAPL, AMZN, GOOGL, META, MSFT, NVDA, TSLA
- Languages: English, German, Spanish, French, Hindi
- Date range: 2010-07-28 to 2026-02-26
- Topic threshold: TopicScore > 0.60
- Zero-shot model: `joeddav/xlm-roberta-large-xnli`

## Final evaluation

| Horizon | Selected model/features | Accuracy | F1 | Balanced accuracy |
|---|---|---:|---:|---:|
| 1-day | Headline-count + Logistic Regression | 51.16% | 47.66% | 51.23% |
| 3-day | Topic + Emotion + Random Forest | 49.93% | 43.80% | 50.40% |
| 5-day | Headline-count + Logistic Regression | 54.02% | 61.75% | 53.18% |

The final holdout covers **2025-07-02 to 2026-02-26 (733 observations)**.

## Walk-forward model selection

| Horizon | Selected configuration | Mean validation accuracy |
|---|---|---:|
| 1-day | Headline-count + Logistic Regression | 54.19% ± 5.38% |
| 3-day | Topic + Emotion + Random Forest | 53.87% ± 0.39% |
| 5-day | Headline-count + Logistic Regression | 55.49% ± 4.20% |

## Ablations

Topic relevance filtering did not consistently improve prediction:

- 1-day: 52.11% all vs 52.25% filtered
- 3-day: 53.34% all vs 51.84% filtered
- 5-day: 54.84% all vs 52.39% filtered

The English subset begins only in May 2025, so multilingual comparison is restricted to a common period. The observed multilingual differences are not statistically significant.

## Repository structure

```text
PolySent-X-Revised/
├── README.md
├── requirements.txt
├── EXPERIMENTAL_AUDIT.md
├── src/
├── results/
│   ├── tables/
│   └── plots/
└── manuscript/
```

## Reproducibility note

The source scripts document the corrected methodology. Running the full XLM-R/XNLI scoring stage requires the listed Python dependencies and access to the pretrained model weights. Processed result tables from the audit are included separately so that the reported results can be inspected without rerunning inference.

## Interpretation

The revised evidence supports the framework as a structured empirical methodology for investigating multilingual financial-news signals. It does **not** support a claim that topic and emotion features consistently improve stock-direction prediction.
