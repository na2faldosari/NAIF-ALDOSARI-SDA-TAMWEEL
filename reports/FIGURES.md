# Figure sources | مصادر الرسومات

| Project section | Figure | Run evidence |
| --- | --- | --- |
| [Baseline model comparison](../README.md#baseline-model-comparison) | [ROC and precision–recall curves](baseline_roc_pr.png) | [Executed notebook](../notebooks/01_baseline_boosting.ipynb) · [Model comparison](../artifacts/day1_model_comparison.csv) |
| [Time-aware validation and tuning](../README.md#time-aware-validation-and-tuning) | [Validation protocol comparison](validation_protocol_comparison.png) | [Validation summary](../artifacts/validation_summary.csv) |
| [Review policy and capacity](../README.md#review-policy-and-capacity) | [Threshold cost and review load](review_cost_curve.png) | [Threshold sweep](../artifacts/threshold_sweep.csv) · [Threshold metrics](../artifacts/threshold_metrics.json) |
| [Model interpretation and calibration](../README.md#model-interpretation-and-calibration) | [Global SHAP beeswarm](global_shap.png) | [Executed notebook](../notebooks/04_explain_calibrate.ipynb) · [Permutation importance](../artifacts/permutation_importance.csv) |
| [Final model and inference](../README.md#final-model-and-inference) | [Ensemble and single model comparison](ensemble_comparison.png) | [Executed notebook](../notebooks/05_final_model.ipynb) · [Candidate scores](../artifacts/ensemble_comparison.csv) |

The figures link back to the evidence from the executed runs.

## Supporting figures

| Project stage | Figure | Source |
| --- | --- | --- |
| Baseline model comparison | [Boosting learning curves](boosting_learning_curves.png) | [Executed notebook](../notebooks/01_baseline_boosting.ipynb) |
| Time-aware validation and tuning | [Training and validation fold sizes](validation_fold_sizes.png) | [Fold audit](../artifacts/fold_audit.csv) |
| Time-aware validation and tuning | [Optuna search history](optuna_search.png) | [Optuna trials](../artifacts/optuna_results.csv) |
| Review policy and capacity | [ROC and precision–recall curves](imbalance_roc_pr.png) | [Executed notebook](../notebooks/03_cost_sensitive_decision.ipynb) |
| Review policy and capacity | [Period capacity and regional rates](review_capacity_and_regions.png) | [Decision card](DECISION_CARD.md) · [Region audit](../artifacts/day3_region_audit.csv) |
| Model interpretation and calibration | [Local SHAP waterfall](local_shap.png) | [Interpretability report](INTERPRETABILITY_REPORT.md) |
| Model interpretation and calibration | [Probability reliability curve](probability_reliability.png) | [Calibration metrics](../artifacts/calibration_metrics.json) |
| Final model and inference | [Model diversity](candidate_diversity.png) | [Probability](../artifacts/day5_probability_correlation.csv) and [residual correlations](../artifacts/day5_residual_correlation.csv) |
| Final model and inference | [OOF policy and regional diagnostic](final_policy_regions.png) | [Threshold sweep](../artifacts/day5_threshold_sweep.csv) and [regional rates](../artifacts/day5_region_audit.csv) |
| Final model and inference | [Calibration-fit diagnostic](final_calibration_fit.png) | [Reliability bins](../artifacts/day5_calibration_fit_bins.csv) |
| Final model and inference | [Challenge batch review capacity](batch_review_capacity.png) | [Final policy](../artifacts/final_policy.json) and [submission CSV](../submission/submission.csv) |
