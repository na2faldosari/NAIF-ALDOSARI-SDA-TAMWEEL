# Figure sources | مصادر الرسومات

| Project section | Figure shown in the project README | Run evidence |
| --- | --- | --- |
| [Day 1 · Baseline and boosting](../README.md#day-1-results) | [ROC and precision–recall curves](day1_roc_pr.png) | [Day 1 notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) · [Model comparison](../artifacts/day1_model_comparison.csv) |
| [Day 2 · Validation and tuning](../README.md#day-2-results) | [Validation protocol comparison](day2_validation_comparison.png) | [Day 2 validation summary](../artifacts/validation_summary.csv) |
| [Day 3 · Cost-sensitive decisions](../README.md#day-3-results) | [Threshold cost and review load](day3_cost_curve.png) | [Threshold sweep](../artifacts/threshold_sweep.csv) · [Threshold metrics](../artifacts/threshold_metrics.json) |
| [Day 4 · Explanation and calibration](../README.md#day-4-results) | [Global SHAP beeswarm](day4_shap_beeswarm.png) | [Day 4 notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_04.ipynb) · [Permutation importance](../artifacts/permutation_importance.csv) |
| [Day 5 · Final model and export](../README.md#day-5-results) | [Ensemble and single model comparison](day5_ensemble_comparison.png) | [Day 5 notebook](../notebooks/05_final_model.ipynb) · [Candidate scores](../artifacts/ensemble_comparison.csv) |

The Day 1 images, Day 3 cost curve and Day 4 and 5 figures match the submission copies in `artifacts/`.

## Supporting figures

| Day | Figure | Source |
| --- | --- | --- |
| 1 | [Boosting learning curves](day1_learning_curves.png) | [Day 1 notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) |
| 2 | [Training and validation fold sizes](day2_fold_sizes.png) | [Fold audit](../artifacts/fold_audit.csv) |
| 2 | [Optuna search history](day2_search.png) | [Optuna trials](../artifacts/optuna_results.csv) |
| 3 | [ROC and precision–recall curves](day3_roc_pr.png) | [Day 3 notebook](../notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_03.ipynb) |
| 3 | [Period capacity and regional rates](day3_capacity_regions.png) | [Decision card](DECISION_CARD.md) · [Region audit](../artifacts/day3_region_audit.csv) |
| 4 | [Local SHAP waterfall](day4_shap_waterfall.png) | [Interpretability report](INTERPRETABILITY_REPORT.md) |
| 4 | [Probability reliability curve](day4_reliability_curve.png) | [Calibration metrics](../artifacts/calibration_metrics.json) |
| 5 | [Model diversity](day5_diversity.png) | [Probability](../artifacts/day5_probability_correlation.csv) and [residual correlations](../artifacts/day5_residual_correlation.csv) |
| 5 | [OOF policy and regional diagnostic](day5_policy_regions.png) | [Threshold sweep](../artifacts/day5_threshold_sweep.csv) and [regional rates](../artifacts/day5_region_audit.csv) |
| 5 | [Calibration-fit diagnostic](day5_calibration_fit.png) | [Reliability bins](../artifacts/day5_calibration_fit_bins.csv) |
| 5 | [Challenge batch review capacity](day5_challenge_capacity.png) | [Final policy](../artifacts/final_policy.json) and [submission CSV](../submission/submission.csv) |
