# Tamweel Lite | تمويل لايت

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1gFq8oMK6yUSCPfUD1HMygbfTQD3gmM3J)

**Can information available at application time help rank financing requests by 90-day default risk?**

Tamweel Lite is a project for **Advanced Machine Learning Methods (SDA-DSC-211)** at SDAIA Academy. It uses synthetic financing applications to study how a risk model can support an approve, review, or decline decision. **Trainee code** `211`.

**Current stage** The Colab readiness checks passed. Day 1 compared three models and selected XGBoost as a provisional candidate. Day 2 compared fixed and tuned LightGBM under time- and customer-aware validation. Day 3 will apply that validation setup to decision costs.

[Open the executed notebook](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_02.ipynb) · [Day 1](#day-1-results) · [Day 2](#day-2-results) · [Daily answers](#daily-answers--الإجابات-اليومية) · [Figure sources](reports/FIGURES.md)

## Project roadmap

| Stage | Question | Progress |
| --- | --- | --- |
| 00 · Readiness | Does Colab have the required packages, and do the course data and tools work? | `READY` on CPU |
| 01 · Baseline and boosting | Which of the three models is the strongest candidate on the comparison set? | `TECHNICAL_READY` |
| 02 · Validation and tuning | How do leakage, time and customer separation affect the comparison? | `TECHNICAL_READY` |
| 03 · Cost-sensitive decisions | How do error costs and review capacity affect the threshold? | Planned |
| 04 · Explanation and calibration | What drives the predictions, and how well do the probabilities reflect observed risk? | Planned |
| 05 · Final model and export | Which model and decision policy should be exported with the documented evidence? | Planned |

## Readiness and reproduction

The readiness run passed on Colab CPU with **10,000 training applications**. The data checks and six compatibility checks passed. The setup cell at the top of the project notebook prepares the package versions required by the course.

1. Open the [executed notebook in GitHub](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_02.ipynb) to review its saved tables and figures.
2. Open the [project notebook in Google Colab](https://colab.research.google.com/drive/1gFq8oMK6yUSCPfUD1HMygbfTQD3gmM3J), use a CPU runtime and run from the top with `FAST_MODE=True`. If the setup cell restarts the session, run from the top again.
3. Check for `TECHNICAL_READY` after the export cell. The course code uses seed `211` and fetches the course files. Compare the [Day 1 model table](artifacts/day1_model_comparison.csv) and [Day 2 validation summary](artifacts/validation_summary.csv). Training times can vary between sessions.

## Day 1 results

Day 1 used 22 approved predictors to rank applications by the risk of `default_within_90d`. The outer split reserved 8,000 applications for development and 2,000 for comparison. Inside development, 6,000 applications were used to fit the boosting models while 2,000 were used for early stopping. The selected tree counts were fixed before refitting on all 8,000 development applications.

| Model | Average Precision | ROC-AUC | Training time (s) | Selected trees |
| --- | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.3258 | **0.8213** | 0.0891 | — |
| XGBoost | **0.3338** | 0.8124 | 0.4918 | 61 |
| LightGBM | 0.3248 | 0.8138 | 0.3399 | 41 |

Of the 2,000 comparison applications, 158 defaulted within the target window (**7.9%**). Average Precision focuses on how well the models retrieve this rare class across score cutoffs. ROC-AUC measures ranking across both classes. Logistic Regression led on ROC-AUC, while XGBoost led on Average Precision by 0.0080 over the baseline. Boosting times include tree selection and refitting.

![ROC and precision–recall curves comparing the three Day 1 models](artifacts/day1_roc_pr.png)

### Evidence in this repository

| File | What to look for |
| --- | --- |
| [Day 1 notebook](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) | The executed readiness and baseline comparison |
| [Model comparison](artifacts/day1_model_comparison.csv) | The three models evaluated on the same 2,000 applications |

## Day 2 results

[Executed Day 2 notebook](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_02.ipynb)

Day 2 used LightGBM to compare four validation protocols. The feature audit identified two post-application fields, `days_past_due_60` and `collection_calls`. Removing them left 22 approved predictors. The duplicate check found zero duplicates among 10,000 applications. Each forward fold used training labels whose 90-day outcome windows closed before validation began.

| Validation start | Eligible training applications | Validation applications | Shared customers |
| --- | ---: | ---: | ---: |
| July 2023 | 3,223 | 1,632 | 0 |
| January 2024 | 4,460 | 1,674 | 0 |
| July 2024 | 5,731 | 1,733 | 0 |

Optuna completed all 8 trials on 1,935 reserved historical applications. The search excluded all outer-validation customers. Its best internal mean AP was 0.3333, with a learning rate of 0.0853 (rounded), 31 leaves, and 40 minimum child samples. Those settings were fixed before outer evaluation. Each forward run covered all 5,039 eligible validation applications, or 50.39% of the dataset.

| Validation protocol | Mean AP ± SD | Mean ROC-AUC ± SD |
| --- | ---: | ---: |
| Leaky random control | 0.9988 ± 0.0014 | 0.9999 ± 0.0002 |
| Clean random control | 0.3110 ± 0.0293 | 0.8010 ± 0.0200 |
| Time- and customer-aware, fixed LightGBM | **0.3153 ± 0.0426** | **0.7976 ± 0.0206** |
| Time- and customer-aware, tuned LightGBM | 0.3133 ± 0.0259 | 0.7855 ± 0.0232 |

![Day 2 validation comparison showing the effect of leakage and the scores of both LightGBM configurations](reports/day2_validation/day2_validation_comparison.png)

The random controls show how much the evaluation changed when we removed post-application fields and fit imputation inside training. Mean AP fell from 0.9988 to 0.3110, a gap of 0.6879 calculated before rounding. The clean random and forward protocols cover different applications. Within the same three forward folds, fixed LightGBM scored slightly higher on both means, while tuned LightGBM had less AP variation. The bars show the sample standard deviation across folds.

| Evidence | What to check |
| --- | --- |
| [Leakage audit](artifacts/leakage_audit.csv) | Which fields were available when the application arrived |
| [Fold audit](artifacts/fold_audit.csv) | Three validation windows, zero shared customers and the training label cutoff |
| [Optuna trials](artifacts/optuna_results.csv) | Eight live trials and their internal fold scores |
| [Validation summary](artifacts/validation_summary.csv) | Mean and sample standard deviation for AP and ROC-AUC |

## Daily answers | الإجابات اليومية

### Day 1 | اليوم الأول

I picked XGBoost for now. Only 7.9% of the requests in the comparison set defaulted, so I paid more attention to how well each model ranked those cases. XGBoost had the highest Average Precision at 0.3338, just ahead of Logistic Regression at 0.3258. The gap was small, so I’ll see if it holds up on Day 2.

**Why I changed the validation.** The Day 1 split separated application rows, but 1,226 customers still appeared in both development and comparison. Day 2 addressed that overlap and time ordering using LightGBM as the course lab’s common model. The comparison across Day 1’s three models still stands as a baseline for that original split.

### Day 2 | اليوم الثاني

I found that `collection_calls` counted contacts made after the application. I removed it and `days_past_due_60` because neither was known when the applicant applied. The leaky random control scored 0.9988 mean AP, compared with 0.3110 for the clean random control. The clean run also learned its imputation medians inside training. For the later validation windows, I excluded those customers from training and used only earlier applications whose 90-day outcome window had already closed. All three folds had zero shared customers.

## Scope and references

The data are synthetic. Day 1 gives an initial ranking comparison, and Day 2 evaluates later applications from customers kept out of each training fold. The course project addresses thresholds, calibration, explanations and final export in later labs.

[Course portal](https://almiyead-rgb.github.io/advanced-machine-learning-methods-sda-dsc-211/) · [Day 1 guide](https://github.com/almiyead-rgb/sda-dsc-211-student-template/blob/main/DAY1_GUIDE.md) · [Day 2 guide](https://github.com/almiyead-rgb/sda-dsc-211-student-template/blob/main/DAY2_GUIDE.md) · [SDAIA Academy on GitHub](https://github.com/SDAIAAcademy)
