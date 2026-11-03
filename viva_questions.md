# Viva Preparation - 30 Questions and Answers

## Flight delay classification, dataset and target
**1. What is the problem you solved?** Binary classification: predict whether a scheduled flight will depart late (1) or on time (0) from information known before departure.

**2. Why classification and not regression?** The project asks for a delayed/not-delayed decision. Regression would predict delay minutes; classification is simpler to evaluate and act on. (Delay minutes are used only for EDA and to build the label.)

**3. What is your target variable and how did you define "delayed"?** The target is a 0/1 label made from departure delay: 1 if delay > 15 minutes (the common FAA/BTS convention). `[[FILL: say if your dataset/brief defines it differently]]`

**4. Describe your dataset.** `[[FILL: name, source URL, rows, columns, period]]`; main columns: airline, origin, destination, date, scheduled departure time, distance, scheduled duration, departure delay.

**5. Which features did you use and why?** Airline, origin, destination, month, day of week, scheduled departure hour, distance, scheduled duration: all known before takeoff and plausibly related to delay.

## Cleaning and preprocessing
**6. What cleaning did you do?** Removed duplicates and cancelled flights, dropped rows with no departure delay, imputed missing feature values, created month/weekday/hour from date and time columns.

**7. How did you handle missing values?** Median for numeric and most-frequent for categorical columns, inside the pipeline so statistics come from training data only.

**8. Why one-hot encoding?** Categories such as airline have no natural order; one-hot avoids implying one. Rare categories are grouped and unseen ones ignored at prediction time.

**9. Why scale features?** Logistic Regression is sensitive to feature scale; trees are not, but a shared pipeline is simpler.

**10. How did you handle class imbalance?** Class weights (LR, RF) and `scale_pos_weight` (XGBoost) so the minority delayed class is not ignored; the test set is left untouched.

## EDA
**11. What did EDA show?** `[[FILL: your real findings: delay rate overall, by hour, airline, month]]`

**12. Why look at the correlation heatmap and what are its limits?** It shows linear relations between numeric variables and the target. It misses non-linear and categorical effects, which is why tree models and other plots are needed.

## Models
**13. Explain Logistic Regression.** A linear model passing a weighted sum of features through a sigmoid to output a probability of delay; coefficients show direction and size of effects. It is our baseline.

**14. Explain Random Forest.** An ensemble of decision trees, each trained on a bootstrap sample and random feature subsets; predictions are averaged, which reduces variance and overfitting.

**15. Explain XGBoost.** Gradient-boosted trees: trees are added one by one, each fitting the errors of the previous ensemble, with regularisation to limit overfitting.

**16. Why these three models?** Baseline linear model versus a bagging ensemble versus a boosting ensemble: increasing complexity lets us see whether non-linearity helps.

**17. Which model won and why?** `[[FILL: from model_comparison.csv; state metrics]]`. Do not say a model is best without the numbers.

## Split, over/underfitting
**18. How did you split the data and why chronologically?** Earliest 80% for training, latest 20% for testing, so the model is evaluated on future flights, as in real use. A random split would mix future and past and be too optimistic.

**19. What is cross-validation?** Repeatedly training on part of the training data and validating on the rest to estimate stability. I used a time-series version to respect time order.

**20. What is overfitting?** The model memorises training noise: high training score, much lower test score. Remedies: simpler model, regularisation, limit tree depth, more data.

**21. What is underfitting?** The model is too simple to capture patterns: poor on both training and test data. Remedies: richer features, a more flexible model.

**22. How can you tell from your results?** Compare CV/training and test metrics. `[[FILL: your gap]]`

## Metrics
**23. Why is accuracy not enough?** With imbalanced classes, predicting "on time" always can look accurate while catching no delays.

**24. Define precision, recall, F1.** Precision = TP/(TP+FP): of predicted delays, how many were real. Recall = TP/(TP+FN): of real delays, how many were caught. F1 = harmonic mean of both.

**25. What is ROC-AUC?** The probability that a random delayed flight gets a higher score than a random on-time one; 0.5 is chance, 1.0 perfect; it is independent of the decision threshold.

**26. What is a confusion matrix and what does yours show?** A table of TP, FP, TN, FN. `[[FILL: interpret your best model's matrix]]`

**27. Precision or recall: which matters more here?** Depends on cost: passengers want few missed delays (recall); airlines acting on alerts may want few false alarms (precision). The threshold can be tuned accordingly.

## Leakage and importance
**28. What is data leakage and how did you avoid it?** Using information unavailable at prediction time or letting test information influence training. I removed arrival delay, actual times, taxi times and delay-cause columns, and fitted all preprocessing inside pipelines on training data.

**29. What would happen if arrival delay was used?** Scores would be unrealistically high because arrival delay is almost determined by departure delay and is known only after the flight.

**30. How did you analyse feature importance and what did you find?** Tree models give impurity-based importances and LR gives coefficient magnitudes, aggregated per original feature. `[[FILL: your top features]]`. Importances show association, not causation.
