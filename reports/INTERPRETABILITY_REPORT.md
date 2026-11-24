# Day 4 interpretability report | تقرير التفسير والمعايرة

**Run** · Tamweel Lite · Live CPU training and SHAP · Weighted LightGBM, 80 trees · `TECHNICAL_READY` · `CAPACITY_REVIEW_REQUIRED`.

## Model and evaluation roles

The model uses application-time inputs to score synthetic 90-day default. The split keeps customers separate and waits for each earlier role's labels to mature before the next role starts. It assigns 2,516 rows to fit, 584 to calibration (40 defaults), 589 to policy, and 1,733 to evaluation (139 defaults). Sigmoid calibration was fitted on the frozen model's scores from the calibration period. The evaluation data previously appeared in earlier labs and serves as a later-period check for this exercise. [Executed notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_04.ipynb)

## Global dependence and one local explanation

`bureau_score` ranked first in global SHAP (mean absolute contribution 0.9042 raw log-odds across 300 × 22 values) and held-out permutation importance (mean AP decrease 0.1270). Shuffling `dti` lowered AP by 0.0690. Permutation measures a performance change when the model loses a feature's intact signal. SHAP allocates the distance between the model's base value and its raw margin. The associations can reflect correlations and provide no causal estimate of changing credit score. [SHAP beeswarm](day4_shap_beeswarm.png) · [Permutation table](../artifacts/permutation_importance.csv)

`TR-009585` had the highest raw score within the SHAP sample. Its score was 0.90308 before calibration and 0.47952 after. The three largest positive contributions are below. None of these three inputs was imputed on this case.

| Feature | Value used | Contribution to raw margin |
| --- | ---: | ---: |
| Synthetic application-time credit score (`bureau_score`) | 497 | +2.2693 log-odds |
| Debt-to-income ratio including the proposed installment (`dti`) | 1.2806 | +1.1981 log-odds |
| Requested financing amount (`loan_amount_sar`) | 93,437.29 SAR | +0.2007 log-odds |

These contributions sum with the base value and the other features on the raw log-odds scale. They explain how the model scored this one request. The ±1-point perturbation of `bureau_score` preserved the scores and the same three reasons in this local check. [SHAP waterfall](day4_shap_waterfall.png)

## Calibration and stability

On the same 1,733 evaluation applications, Brier fell from **0.11303** to **0.06711**, ECE from **0.14687** to **0.02249**, and log loss from **0.35799** to **0.24675**. ROC-AUC stayed at **0.77080** and AP at **0.25868**. The sigmoid was strictly increasing, so it changed the probability scale while preserving the ranking. The reliability plot shows ten bins and their counts, including sparse regions of the scale. [Reliability curve](day4_reliability_curve.png) · [Calibration metrics](../artifacts/calibration_metrics.json)

The 200 paired customer-cluster resamples produced a 95% interval of **0.19744 to 0.33785** for AP and **−0.05418 to −0.03741** for the change in Brier. These intervals condition on the fixed model, fixed calibrator and observed evaluation period. They do not measure uncertainty from model training, calibration fitting or future drift. [Bootstrap output in the notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_04.ipynb)

## Review capacity

The policy split selected `0.5881953696965011` on the raw scale with 68 flags against 70 available slots. This maps to `0.17331013263107387` after calibration and preserves the same risk flags. The diagnostic ±0.02 probability band spans approximately 0.15331–0.19331. A candidate queue combines risk flags and near-cutoff cases, counting each request once.

| Evaluation quarter | Capacity | Risk flags | Candidate reviews |
| --- | ---: | ---: | ---: |
| 2024 Q3 | 100 | 97 | 109 |
| 2024 Q4 | 107 | 109 | 122 |

Both candidate queues exceed capacity. Risk flags alone also exceed the ceiling in Q4. `CAPACITY_REVIEW_REQUIRED` records that finding. The evaluation keeps the cutoff and ceiling fixed. A revised policy would require new development evidence and a later evaluation. [Calibration and policy export](../artifacts/calibration_metrics.json)
