# Predicting Scheduled Flight Departure Delays Using Logistic Regression, Random Forest and XGBoost

**Authors:** `[[FILL]]` | **Affiliation:** `[[FILL]]` | **Email:** `[[FILL]]`

> DRAFT. Every `[[FILL]]` must be completed from `results/`. Trim to the page limit of your template (IEEE conference format, two columns).

**Abstract—** Flight delays impose significant costs on airlines and passengers. This paper presents a binary classification system that predicts whether a scheduled flight will depart more than `[[FILL: threshold]]` minutes late using only pre-departure information. Logistic Regression, Random Forest and XGBoost are compared on `[[FILL: dataset, n flights, period]]` using leakage-safe pipelines and a chronological split. The best model, `[[FILL]]`, reached ROC-AUC `[[FILL]]` and F1 `[[FILL]]` on held-out later flights. `[[FILL: key finding]]`.

**Keywords—** flight delay prediction, classification, random forest, XGBoost, logistic regression, data leakage, aviation analytics.

## I. Introduction
Delays reduce punctuality, increase fuel and crew costs, and propagate through airline networks. Predicting delay risk before departure helps schedulers and passengers act earlier. This work studies how well schedule-level features alone predict departure delay and how three common classifiers compare. Contributions: (1) a leakage-aware feature design that excludes all post-departure information; (2) a chronological evaluation that mimics forecasting future flights; (3) a comparison of a linear baseline with two tree ensembles; (4) a reusable prediction tool.

## II. Related Work
Rebollo and Balakrishnan characterised delay patterns in the US air network and used random forests with temporal and network features to predict delays [1]. Kalliguddi and Leboulluec compared machine-learning models for aircraft delay prediction [2]. Choi et al. incorporated weather data to predict weather-induced delays [3]. Tree ensembles [4], [5] are common for tabular problems of this type. `[[FILL: add 3–5 more verified studies you actually read and state the gap this paper addresses]]`

## III. Dataset Description
The data come from `[[FILL: Kaggle dataset, URL, access date]]`, containing `[[FILL]]` records for `[[FILL: period]]`. After removing duplicates, cancelled flights and rows without a departure delay, `[[FILL]]` flights remained (`[[FILL]]`% delayed). A flight is labelled delayed if its departure delay exceeds `[[FILL: 15]]` minutes. Features: airline, origin, destination, month, day of week, scheduled departure hour, distance and scheduled elapsed time. Arrival delay, actual times, taxi times and delay-cause fields were excluded because they are unavailable at prediction time.

## IV. Proposed Methodology
**A. Preprocessing.** Missing values are imputed (median/most frequent), numeric features standardised and categorical features one-hot encoded, all within a scikit-learn pipeline fitted on training data only. **B. Split.** The first 80% of flights by date form the training set and the last 20% the test set `[[FILL: confirm; or state stratified split if no date column]]`. **C. Imbalance.** Class weights (LR, RF) and `scale_pos_weight` (XGBoost). **D. Models.** Logistic Regression [baseline], Random Forest [4], XGBoost [5], implemented with scikit-learn [6] and XGBoost.

## V. Experimental Setup
Python `[[FILL version]]`, scikit-learn `[[FILL]]`, XGBoost `[[FILL]]`; hardware `[[FILL]]`. Hyperparameters: LR (max_iter=1000), RF (200 trees, max depth 18, min leaf 5), XGBoost (300 trees, depth 6, learning rate 0.1). Cross-validation: 3-fold time-series split on the training data `[[FILL: confirm]]`. Metrics: accuracy, precision, recall, F1 for the delayed class, and ROC-AUC. Selection criterion: ROC-AUC with F1 as tie-break.

## VI. Results and Discussion
TABLE I. Test-set performance `[[FILL: paste results/model_comparison.csv]]`

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` |
| Random Forest | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` |
| XGBoost | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` | `[[FILL]]` |

Fig. 1: ROC curves (`results/figures/roc_curves.png`). Fig. 2: confusion matrix of the best model. Fig. 3: feature importance.
Discussion `[[FILL: based strictly on results: best model; precision–recall trade-off; effect of the class balance on accuracy; most important features; delay patterns by hour/airline/month; CV vs. test gap]]`.

## VII. Conclusion
`[[FILL: 3–4 sentences summarising the findings]]`

## VIII. Future Scope
Integrating weather and airport congestion data, aircraft-rotation (previous-leg) delay, calibrated probabilities with cost-based thresholds, hyperparameter optimisation, and real-time deployment.

## References
[1] J. J. Rebollo and H. Balakrishnan, "Characterization and prediction of air traffic delays," *Transp. Res. Part C: Emerg. Technol.*, vol. 44, pp. 231–241, 2014.
[2] A. N. Kalliguddi and A. K. Leboulluec, "Predictive modeling of aircraft flight delay," *Univers. J. Manag.*, vol. 5, no. 10, pp. 485–491, 2017.
[3] S. Choi, Y. J. Kim, S. Briceno, and D. Mavris, "Prediction of weather-induced airline delays based on machine learning algorithms," in *Proc. IEEE/AIAA 35th Digit. Avionics Syst. Conf. (DASC)*, 2016.
[4] L. Breiman, "Random forests," *Mach. Learn.*, vol. 45, no. 1, pp. 5–32, 2001.
[5] T. Chen and C. Guestrin, "XGBoost: A scalable tree boosting system," in *Proc. 22nd ACM SIGKDD Int. Conf. Knowl. Discovery Data Mining*, 2016, pp. 785–794.
[6] F. Pedregosa *et al.*, "Scikit-learn: Machine learning in Python," *J. Mach. Learn. Res.*, vol. 12, pp. 2825–2830, 2011.
[7] `[[FILL: assigned dataset citation]]`

*Verify every reference in Google Scholar before submission.*
