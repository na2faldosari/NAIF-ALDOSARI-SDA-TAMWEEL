# Tamweel Lite project record

Tamweel Lite ranks synthetic applications by 90-day default risk using information available at application time. The exported score and policy refer applications to simulated human review. The [main README](README.md) records each lab, the five daily answers and links to their outputs.

The five labs cover baseline comparison, time- and customer-separated validation, cost and review capacity, interpretation and calibration, and final model selection. The [evidence exports](evidence) retain the original tables, run metadata and response checkpoints from the executed lab notebooks. The [final notebook](notebooks/delivery/NAIF_ALDOSARI_SDA_TAMWEEL_FINAL.ipynb) brings those stages together.

## Final model and policy

Logistic Regression led three forward validation folds at 0.3917 mean Average Precision. The best ensemble scored 0.3877, so the selection gate retained the single model. The frozen batch policy flagged 300 of 2,500 unlabeled challenge applications for simulated review. The [Model Card](reports/MODEL_CARD.md) describes the data, evaluation, policy and limits. The [Ensemble Decision](reports/ENSEMBLE_DECISION.md) records how the final model was selected.

## Export verification

The saved model and batch policy reproduced all review flags in [submission.csv](submission/submission.csv) and matched the submitted probabilities within `1e-12`. The replay results are saved in the [final notebook](notebooks/delivery/NAIF_ALDOSARI_SDA_TAMWEEL_FINAL.ipynb).

The [official Notebook 99](notebooks/99_final_submission_check.ipynb) checks the saved notebooks, daily answers, and required outputs.
