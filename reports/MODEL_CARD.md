# Model card | Tamweel Lite

| Item | Value |
| --- | --- |
| Course | SDAIA Academy · SDA-DSC-211 |
| Task | Rank synthetic applications by the probability of default within 90 days |
| Prediction time | At application using the 22 features approved by the course data contract |
| Selected model | Logistic Regression selected by the ensemble comparison gate |
| Output | Calibrated default probability and a batch-level simulated review flag |

## Purpose

The score ranks requests for simulated human review using synthetic data. The project evaluates ranking quality, probability calibration and the cost of missed defaults and unnecessary reviews.

## Data

The dataset contains 10,000 labeled synthetic applications and 2,500 unlabeled challenge applications. The final evaluation assigned 6,576 labeled rows to model fitting and selection, 836 to sigmoid fitting, and excluded 2,588 from those two roles.

## Model selection and evaluation

Nested forward validation produced 2,155 outer OOF predictions across three periods. Earlier training windows respected 90-day label maturity and held out customers in the evaluation window. Logistic Regression led with mean AP **0.39166**, fold SD **0.02981**, mean Brier **0.06327** and mean ECE **0.01882**. These metrics summarize the model-selection folds.

## Probability calibration

The sigmoid was fitted on the separately reserved 836 rows, with raw and mapped Brier of **0.07647** and **0.07806** on those same rows. Raw and mapped ECE were **0.02112** and **0.03487**. The challenge set supplies no labels for evaluation.

## Review policy

The OOF sweep minimized `10 × FN + FP` subject to flagging at most 12% in each forward period. At the chosen raw cutoff of **0.1689216143**, there were 84 true flags, 161 false flags and 95 missed defaults. The highest period review rate was **11.75%**. The spread in regional OOF false-positive rates was **3.98 percentage points** across four supported regions. This was a descriptive diagnostic.

The frozen sigmoid maps the cutoff to **0.1222584314**. The challenge policy ranks the 330 threshold-eligible applications by score and fits whole equal-score blocks within the 300-slot cap. It flagged 300 of 2,500 applications. Replay matched all decision flags and probabilities within an absolute tolerance of `1e-12`.

## Explanation scope

The SHAP and permutation figures explain the exploratory weighted LightGBM. Inspect the final Logistic Regression coefficients on their processed input scales. A later labeled cohort would support a separate feature-reliance check for the final model.

## Intended use and limitations

This prototype uses synthetic data to study ranking, cost and review capacity. Its binary flag represents simulated routing to a review queue. Use of these scores for real financing decisions would require representative labeled data, independent validation and review of subgroup error rates. With labeled outcomes, reassess drift, Brier, subgroup false-positive rates and queue saturation before reconsidering the model or cutoff.

## Reproducibility

The [final notebook](../notebooks/delivery/NAIF_ALDOSARI_SDA_TAMWEEL_FINAL.ipynb) records model export and replay. The [replay script](../scripts/replay_final.py) compares the model's scores and review flags with the exported challenge file.

## Project and course materials

The [project README](../README.md) links the notebooks, evidence and source materials.

[Final metrics](../artifacts/final_metrics.json) · [Frozen policy](../artifacts/final_policy.json) · [Ensemble decision](ENSEMBLE_DECISION.md) · [Challenge submission](../submission/submission.csv)
