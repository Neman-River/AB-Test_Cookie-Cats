# Cookie Cats — Gate Placement A/B Test

A/B test analysis examining the effect of moving the first gate in the mobile game **Cookie Cats** from level 30 to level 40, with a focus on player retention.

## Background

Cookie Cats is a mobile puzzle game where players occasionally encounter gates — forced pauses requiring a wait or an in-app purchase to continue. These gates serve two purposes: driving in-app purchases and giving players an enforced break that is hypothesised to increase long-term enjoyment and reduce churn.

The experiment tests whether moving the gate from **level 30** (control) to **level 40** (treatment) improves or hurts player retention.

## Dataset

90,189 players who installed Cookie Cats during the A/B test period.

| Column | Description |
|---|---|
| `userid` | Unique player identifier |
| `version` | `gate_30` (control) or `gate_40` (treatment) |
| `sum_gamerounds` | Rounds played in the first 14 days |
| `retention_1` | Returned 1 day after install (bool) |
| `retention_7` | Returned 7 days after install (bool) |

Source: [DataCamp / Kaggle — Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)

## Analysis

All analysis lives in `ab_tests.ipynb`:

1. **EDA** — group size validation, Sample Ratio Mismatch check, engagement distribution, segment comparisons
2. **Z-test & Chi-squared** — two-proportion tests for retention_1 and retention_7
3. **Mann-Whitney U** — non-parametric engagement comparison (sum_gamerounds)
4. **Bootstrap 95% CIs** — sampling distribution of the retention difference
5. **Effect size (Cohen's h)** — practical significance assessment
6. **Bayesian A/B test** — Beta-Binomial posterior, P(gate_30 > gate_40), expected loss

## Key Results

| Metric | gate_30 | gate_40 | Difference | Significant? |
|---|---|---|---|---|
| 1-day retention | 44.82% | 44.23% | +0.59 pp | No (p = 0.074) |
| 7-day retention | 19.02% | 18.20% | +0.82 pp | **Yes (p = 0.0016)** |
| Engagement | — | — | — | No (p = 0.050) |

## Recommendation

**Keep the gate at level 30.**

The gate_30 group shows significantly higher 7-day retention across every method tested (frequentist, bootstrap, Bayesian). Moving the gate to level 40 causes a measurable retention loss with no compensating benefit to engagement. At scale, keeping gate_30 retains approximately **+8,200 more players per 1M installs** on day 7.

## Setup

Requires Python 3.12+ and [uv](https://github.com/astral-sh/uv).

```bash
# Install dependencies
uv sync

# Launch the notebook
uv run jupyter notebook ab_tests.ipynb
```

## Dependencies

| Package | Purpose |
|---|---|
| `pandas`, `numpy` | Data manipulation |
| `scipy` | Z-test, chi-squared, Mann-Whitney |
| `statsmodels` | Bootstrap, effect size, power analysis |
| `matplotlib`, `seaborn` | Visualisations |
