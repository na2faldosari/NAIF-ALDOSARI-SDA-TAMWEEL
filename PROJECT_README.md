# Tamweel Lite project record

Tamweel Lite ranks synthetic applications by 90-day default risk using information available at application time. The exported score and policy refer applications to simulated human review. The [main README](README.md) records each lab, the five daily answers and links to their outputs.

The five labs cover baseline comparison, time- and customer-separated validation, cost and review capacity, interpretation and calibration, and final model selection. The [evidence exports](evidence) retain the original tables, run metadata and response checkpoints from the executed lab notebooks. The [final notebook](notebooks/delivery/NAIF_ALDOSARI_SDA_TAMWEEL_FINAL.ipynb) brings those stages together.

## Final model and policy

Logistic Regression led three forward validation folds at 0.3917 mean Average Precision. The best ensemble scored 0.3877, so the selection gate retained the single model. The frozen batch policy flagged 300 of 2,500 unlabeled challenge applications for simulated review. The [Model Card](reports/MODEL_CARD.md) describes the data, evaluation, policy and limits. The [Ensemble Decision](reports/ENSEMBLE_DECISION.md) records how the final model was selected.

## Reproduce the exported scores

```sh
python -m pip install -r requirements-colab.txt -c constraints.txt
python scripts/replay_final.py
```

Run both commands from the repository root. Replay compares the probabilities and review flags produced by the saved model and batch policy with [submission.csv](submission/submission.csv). The [official Notebook 99](notebooks/99_final_submission_check.ipynb) runs the technical checks across the lab notebooks and final files.
