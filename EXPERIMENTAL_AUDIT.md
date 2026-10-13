# PolySent-X Experimental Audit

## Purpose

This revision addresses the experimental inconsistencies identified in the
earlier manuscript presentation. The central XLM-R/XNLI topic and emotion
method is retained, while the evaluation protocol and reporting are corrected.

## Data

- Cleaned headlines: 10,310
- Topic-filtered/emotion-scored headlines: 3,183
- Companies: 7
- Languages: English, German, Spanish, French, Hindi
- Daily observations: 1,724
- Daily date range: 2010-07-28 to 2026-02-26

## Topic scoring

The implemented topic score is the maximum zero-shot score over:

1. stock market news
2. company earnings
3. financial markets
4. business news
5. technology news
6. sports news
7. entertainment news

Retention criterion: TopicScore > 0.60.

The previous keyword-ratio equation has been removed because it did not
describe the implemented method.

## Emotion scoring

XLM-R/XNLI zero-shot scoring is applied to:

- joy
- fear
- anger
- sadness
- surprise
- neutral

Derived polarity is:

`(Joy + Surprise) - (Fear + Anger + Sadness)`

## Temporal protocol

The revised evaluation uses expanding walk-forward validation with
horizon-aware purging. Model selection is performed only on the walk-forward
validation folds. The final chronological holdout (2025-07-02 to 2026-02-26,
733 observations) remains untouched until selection is complete.

### Validation folds

1. Train through 2023-12-21; validate 2023-12-22 to 2024-07-24.
2. Train through 2024-07-24; validate 2024-07-25 to 2025-01-27.
3. Train through 2025-01-27; validate 2025-01-28 to 2025-07-01.

Each validation fold contains 100 unique dates.

## Selected configurations

| Horizon | Selected configuration | Walk-forward mean accuracy |
|---|---|---:|
| 1-day | Headline-count + Logistic Regression | 54.19% ± 5.38% |
| 3-day | Topic + Emotion + Random Forest | 53.87% ± 0.39% |
| 5-day | Headline-count + Logistic Regression | 55.49% ± 4.20% |

## Final untouched holdout

| Horizon | Accuracy | F1 | Balanced accuracy |
|---|---:|---:|---:|
| 1-day | 51.16% | 47.66% | 51.23% |
| 3-day | 49.93% | 43.80% | 50.40% |
| 5-day | 54.02% | 61.75% | 53.18% |

Bootstrap 95% confidence intervals for accuracy:

- 1-day: 47.48%–54.71%
- 3-day: 46.38%–53.48%
- 5-day: 50.48%–57.71%

## Relevance-filter ablation

Using the same ticker-date universe:

- 1-day: all headlines 52.11%, topic-filtered 52.25%
- 3-day: all headlines 53.34%, topic-filtered 51.84%
- 5-day: all headlines 54.84%, topic-filtered 52.39%

Therefore, the relevance filter should not be presented as consistently
improving prediction.

## Restricted multilingual comparison

Because English filtered headlines begin only in May 2025, a full-period
English-vs-multilingual comparison is not valid. On the restricted common
period:

- 1-day: multilingual 59.65%, English-only 58.77%, p=1.000
- 3-day: multilingual 59.65%, English-only 54.39%, p=0.392
- 5-day: multilingual 61.40%, English-only 53.51%, p=0.188

These differences are descriptive and do not establish a statistically
significant multilingual advantage.

## Corrected confusion matrices

The earlier near-perfect confusion matrix must not be used. The audited final
holdout counts are:

- 1-day: TN=212, FP=151, FN=207, TP=163
- 3-day: TN=223, FP=130, FN=237, TP=143
- 5-day: TN=124, FP=225, FN=112, TP=272

## Interpretation

The revised results do not support a claim of consistent predictive
superiority. PolySent-X is better framed as a multilingual topic- and
emotion-aware framework for investigating the incremental predictive value of
news signals across multiple horizons.

## Limitations

- The zero-shot topic score is not a calibrated financial-relevance
  probability.
- The maximum topic label was not consistently preserved for retrospective
  topic-specific auditing.
- English data are temporally concentrated.
- The study uses headlines rather than full article content.
- The final predictive gains are modest and horizon dependent.
