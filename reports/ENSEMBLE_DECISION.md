# Ensemble decision

**KEEP SINGLE — Logistic Regression**

The six candidates were evaluated on the same 2,155 outer OOF requests over three forward folds. For the ensemble candidates, the inner fits determined blend weights or the stacking model before the outer validation period was scored.

| Candidate | Mean AP | Lift vs. Logistic | Mean Brier | Mean ECE |
| --- | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.39166 | 0.00000 | 0.06327 | 0.01882 |
| LightGBM | 0.34549 | −0.04617 | 0.06608 | 0.02311 |
| XGBoost | 0.34714 | −0.04452 | 0.06577 | 0.02309 |
| Equal blend | 0.36780 | −0.02386 | 0.06436 | 0.02097 |
| Weighted blend | 0.38765 | −0.00401 | 0.06347 | 0.01913 |
| Stack | 0.38279 | −0.00888 | 0.06606 | 0.03119 |

The gate considers Equal, Weighted and Stack in order and retains the simplest ensemble that gains more AP than the single model’s fold SD of **0.02981**, while limiting any Brier increase to **0.005** and any ECE increase to **0.010**. All three ensembles fell short on AP. The stack also exceeded the ECE tolerance by about **0.00237**.

Raw prediction correlations between the three single models ranged from **0.88 to 0.96**. Residual correlations ranged from **0.98 to 0.99**, which helps explain why combining their scores offered limited benefit in these folds. The gate is a selection rule using descriptive fold variability. Its evidence comes from three related time periods.

[Comparison CSV](../artifacts/ensemble_comparison.csv) · [Gate result](../artifacts/day5_ensemble_gate.json) · [Comparison figure](ensemble_comparison.png) · [Model card](MODEL_CARD.md)
