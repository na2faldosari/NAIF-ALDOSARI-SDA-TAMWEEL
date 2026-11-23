# Tamweel Lite | تمويل لايت

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1gFq8oMK6yUSCPfUD1HMygbfTQD3gmM3J)

**Can information available at application time help rank financing requests by 90-day default risk?**

Tamweel Lite is a project for **Advanced Machine Learning Methods (SDA-DSC-211)** at SDAIA Academy. It uses synthetic financing applications to study how a risk model can support an approve, review, or decline decision. **Trainee code** `211`.

**Current stage** The Colab readiness checks passed. Day 1 compared three models and selected XGBoost as a provisional candidate. The next lab tests whether that result holds when validation respects time and customer boundaries.

[Open the executed notebook](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) · [See the Day 1 results](#day-1-results) · [Read the daily answers](#daily-answers--الإجابات-اليومية)

## Project roadmap

| Stage | Question | Progress |
| --- | --- | --- |
| 00 · Readiness | Does Colab have the required packages, and do the course data and tools work? | `READY` on CPU |
| 01 · Baseline and boosting | Which of the three models is the strongest candidate on the comparison set? | `TECHNICAL_READY` |
| 02 · Validation and tuning | Does the candidate hold up after separating time periods and customers and checking for leakage? | Next |
| 03 · Cost-sensitive decisions | How do error costs and review capacity affect the threshold? | Planned |
| 04 · Explanation and calibration | What drives the predictions, and how well do the probabilities reflect observed risk? | Planned |
| 05 · Final model and export | Which model and decision policy should be exported with the documented evidence? | Planned |

## Readiness and reproduction

The readiness run passed on Colab CPU with **10,000 training applications**. The data checks and six compatibility checks passed. The setup cell at the top of the project notebook prepares the package versions required by the course.

1. Open the [executed notebook in GitHub](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) to review its saved tables and figures.
2. Open the [project notebook in Google Colab](https://colab.research.google.com/drive/1gFq8oMK6yUSCPfUD1HMygbfTQD3gmM3J), use a CPU runtime and run from the top with `FAST_MODE=True`. If the setup cell restarts the session, run from the top again.
3. Check for `TECHNICAL_READY` after the export cell. The course code uses seed `211` and fetches the course files. Compare your results with the [published comparison table](artifacts/day1_model_comparison.csv). Training times can vary between sessions.

## Day 1 results

Day 1 used 22 approved predictors to rank applications by the risk of `default_within_90d`. The outer split reserved 8,000 applications for development and 2,000 for comparison. Inside development, 6,000 applications were used to fit the boosting models while 2,000 were used for early stopping. The selected tree counts were fixed before refitting on all 8,000 development applications.

| Model | Average Precision | ROC-AUC | Training time (s) | Selected trees |
| --- | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.3258 | **0.8213** | 0.0891 | — |
| XGBoost | **0.3338** | 0.8124 | 0.4918 | 61 |
| LightGBM | 0.3248 | 0.8138 | 0.3399 | 41 |

Of the 2,000 comparison applications, 158 defaulted within the target window (**7.9%**). Average Precision focuses on how well the models retrieve this rare class across score cutoffs. ROC-AUC measures ranking across both classes. Logistic Regression led on ROC-AUC, while XGBoost led on Average Precision by 0.0080 over the baseline. Boosting times include tree selection and refitting.

<details>
<summary>See the comparison curves</summary>

![ROC and precision–recall curves from the Day 1 comparison set](artifacts/day1_roc_pr.png)

</details>

### Evidence in this repository

| File | What to look for |
| --- | --- |
| [Executed project notebook](notebooks/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_01.ipynb) | Setup, data split, training output and interpretation |
| [Model comparison](artifacts/day1_model_comparison.csv) | The three models evaluated on the same 2,000 applications |
| [Learning curves](artifacts/day1_learning_curves.png) | How validation log loss informed the selected tree counts |
| [ROC and precision–recall curves](artifacts/day1_roc_pr.png) | How the models trade off detection and false alerts across cutoffs |

## Daily answers | الإجابات اليومية

### Day 1 | اليوم الأول

I picked XGBoost for now. Only 7.9% of the requests in the comparison set defaulted, so I paid more attention to how well each model ranked those cases. XGBoost had the highest Average Precision at 0.3338, just ahead of Logistic Regression at 0.3258. The gap was small, so I’ll see if it holds up on Day 2.

**What Day 2 will test.** The Day 1 split separated application rows, and 1,226 customers still appeared in both the development and comparison sets. Requests from the same customer can be related. A time- and customer-aware split will test whether the AP lead holds for later applications and customers outside the training group.

## Scope and references

The data are synthetic. Day 1 gives an initial ranking comparison. The course project addresses threshold selection, calibration, explanations and final export in later labs.

[Course portal](https://almiyead-rgb.github.io/advanced-machine-learning-methods-sda-dsc-211/) · [Day 1 guide](https://github.com/almiyead-rgb/sda-dsc-211-student-template/blob/main/DAY1_GUIDE.md) · [Day 2 guide](https://github.com/almiyead-rgb/sda-dsc-211-student-template/blob/main/DAY2_GUIDE.md) · [SDAIA Academy on GitHub](https://github.com/SDAIAAcademy)
