# Day 3 decision card | بطاقة القرار

**Scope** · Tamweel Lite · 5,039 synthetic out-of-fold applications · Live CPU run · Weighted LightGBM specified for this exercise.

**Review rule** · Send the application to simulated review when score ≥ `0.6583471436014694`. Use the full cutoff to preserve the review list. [Threshold metrics](../artifacts/threshold_metrics.json)

| Measure | Selected operating point |
| --- | ---: |
| Reviews | 526 / 5,039 (10.44%) |
| True positives / false positives | 157 / 369 |
| False negatives / true negatives | 227 / 4,286 |
| Recall / precision | 40.89% / 29.85% |
| Pooled Average Precision | 0.3100 |
| Simulated loss (`10 × FN + FP`) | 2,639 units |
| Loss normalized per 10,000 applications | 5,237.15 units |
| Largest regional false-positive rate gap | 0.65 percentage points |

**Loss and capacity.** At 0.5, the weighted model flags 1,005 applications, with 2,403 loss units and a 23.20% review rate in the busiest period. The unrestricted loss minimum is 2,275 units at a cutoff of about 0.44864, also above capacity. The selected cutoff minimizes loss subject to `floor(0.12 × applications)` in each period. Periods 1, 2 and 3 use **137/195**, **183/200** and **206/207** slots, respectively. The cost increase of 236 units versus 0.5 reflects 65 more missed defaults and 414 fewer false flags. The selected cutoff also held in the tested scenarios with missed-default costs of 8 and 12 units. [Threshold sweep](../artifacts/threshold_sweep.csv) · [Cost and capacity chart](../artifacts/cost_curve.png)

**Regional audit.** The western group has the highest false-positive rate at 8.29% (96/1,158 non-defaults) and `other` the lowest at 7.64% (93/1,217). The 0.65-point gap describes this run. Investigating a difference would require uncertainty estimates and representative data. [Region audit](../artifacts/day3_region_audit.csv)

**Evidence limits.** A rule with no reviews gets 92.38% accuracy and zero recall. Each OOF score comes from a model trained outside that score's validation fold. The folds also separate customers and respect time and label maturity. The eligible rows cover 50.39% of the input. The cutoff and its reported loss use the same development scores. The study uses synthetic requests and educational loss units, with review as the simulated action. Calibration and an independent evaluation are still needed before relying on the rule outside this lab. [Executed notebook](../notebooks/delivery/NAIF_ALDOSARI_SDA_TAMWEEL_DAY_03.ipynb)

[Project results and daily answer](../README.md#day-3-results)
