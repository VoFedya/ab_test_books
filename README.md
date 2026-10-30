# A/B Testing and Hypothesis Testing: Yandex Books and BitMotion Kit

Statistical analysis project (Yandex Practicum data analyst track) made of two parts: a hypothesis test on reading activity in the Yandex Books app, and an evaluation of an A/B test for the online store BitMotion Kit.

## Part 1. Reading activity: Moscow vs. Saint Petersburg

**Question:** do users from Saint Petersburg spend more time reading and listening to books in the app than users from Moscow? (Earlier LTV analysis showed higher LTV for Saint Petersburg, so the team expected more engaged users there.)

- Data: total hours of reading and listening per user, preprocessed in SQL (`yandex_knigi_data.csv`).
- Data checks: duplicates by user ID, users appearing in both cities (removed), group sizes, per-city statistics and histograms.
- Method: one-sided two-sample t-test, α = 0.05.
  - H₀: μ(SPb) ≤ μ(Moscow)
  - H₁: μ(SPb) > μ(Moscow)
- Result: p-value ≈ 0.33, so H₀ is not rejected and there is no evidence that Saint Petersburg users are more active.

## Part 2. A/B test of a simplified store interface (BitMotion Kit)

**Question:** does a new, simpler interface increase the share of users who make a purchase within 7 days of registration?

- Data: `ab_test_participants` (group assignment) and `ab_test_events` (user events), loaded from the Yandex datasets URLs.
- Test validity checks: group sizes, overlap with a competing test (`recommender_system_test`, overlapping users removed), overlap between groups.
- Horizon: purchases within the first 7 days after registration.
- Sample size: baseline conversion 30%, MDE 3 pp, power 80%, confidence 95%, which gives about 3,761 users per group (the test has about 4,950 and 5,010).
- Results: conversion 27.8% (A) vs. 29.5% (B), an uplift of about +1.73 pp (+6.2% relative), which is below the 3 pp target. A two-sided z-test for proportions gives p ≈ 0.057, so the difference is not significant at α = 0.05.
- Conclusion in the notebook: do not roll out yet; continue optimizing the interface.

## Data

- Part 1 expects `/datasets/yandex_knigi_data.csv` (available on the Practicum platform). The file is not included in this repository. City values in the data are in Russian (`Москва`, `Санкт-Петербург`).
- Part 2 loads data directly from:
  - `https://code.s3.yandex.net/datasets/ab_test_participants.csv`
  - `https://code.s3.yandex.net/datasets/ab_test_events.zip`

## Repository structure

```
ab-testing-analysis/
├── ab_testing_analysis.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Running the notebook

```bash
pip install -r requirements.txt
jupyter notebook ab_testing_analysis.ipynb
```

If you run Part 1 locally, change the `/datasets/...` path to wherever you saved the CSV file.

## Stack

Python, pandas, NumPy, matplotlib, seaborn, SciPy, statsmodels.
