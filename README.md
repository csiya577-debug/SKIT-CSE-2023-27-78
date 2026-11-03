# Flight Delay Classification (Project No. 4)

Predicts whether a scheduled flight will depart late, using only information known **before** departure.
Models: Logistic Regression (baseline), Random Forest, XGBoost.

> **Dataset:** put the dataset assigned in Project No. 4 in `data/` as a CSV.
> Dataset name / Kaggle URL: `[[FILL: exact assigned dataset URL]]`

## 0. Expected dataset layout (Kaggle "Airlines Delay", `Airlines.csv`)
`id, Airline, Flight, AirportFrom, AirportTo, DayOfWeek, Time, Length, Delay` - the code is configured for this layout:
`Time` = scheduled departure (minutes after midnight, converted to hour), `Length` = scheduled flight length,
`Delay` = ready-made 0/1 target (so the 15-minute threshold is not applied). `Flight` (an identifier) and `id` are not used as features.
This dataset has **no date or month column**, so the split is stratified-random (not chronological) and
monthly/seasonal analysis is not possible - say so in the report. Confirm your file's columns match before running.

## 1. Setup
```bash
pip install -r requirements.txt
```
Place the CSV in `data/` (or set `FLIGHT_DATA=/path/to/file.csv`).

## 2. Check the configuration (important)
Open `src/data_preprocessing.py` and look at the CONFIG block:
- `DELAY_THRESHOLD = 15` - delayed means departure delay > 15 min. Change if your project defines it differently.
- `TARGET_COL_OVERRIDE` - set to the column name if your dataset already has a 0/1 delay label.
- `ALIASES` - add your dataset's column names if the script prints "No departure-delay column found".
- `MAX_ROWS` - sampling cap for speed (None = all rows).

The script prints the columns it detected. Confirm they are right before trusting any result.

## 3. Run
```bash
python src/data_preprocessing.py   # sanity check: shapes, delay rate
python src/eda.py                  # figures + results/eda_summary.txt
python src/train_models.py         # models, metrics, results/model_comparison.csv
python src/predict.py --airline AA --origin JFK --dest LAX --date 2024-07-15 --dep-time 1830 --distance 2475 --elapsed 330
```
Run the commands from the project root. Everything is saved under `results/` and `models/`.

**Jupyter / VS Code:** `jupyter notebook notebooks/flight_delay_analysis.ipynb`, run all cells.
**Google Colab:** upload/unzip the folder to Colab, run `%cd /content/flight-delay-classification` and `!pip install -r requirements.txt`, then run the notebook (or the commands above prefixed with `!`).

## 4. Design decisions
- **No leakage:** arrival delay, actual times, taxi times, delay-cause columns and cancellation info are never used as features.
- **Pipelines:** imputation, scaling and one-hot encoding are inside a scikit-learn `Pipeline`, fitted on training data only.
- **Chronological split** (first 80% of dates train, last 20% test) when a date column exists - mimics predicting future flights. Otherwise a stratified split.
- **Cross-validation:** `TimeSeriesSplit` (3 folds) for dated data, `StratifiedKFold` otherwise, on a subsample of the training set.
- **Imbalance:** `class_weight` for LR/RF, `scale_pos_weight` for XGBoost.
- **Model selection:** highest test ROC-AUC (F1 as tie-break); precision/recall trade-off is reported, not hidden.
- The saved `models/*.joblib` are full pipelines, so `predict.py` applies exactly the training preprocessing.

## 5. After running
Fill every `[[FILL: ...]]` in `project_report.md` and `research_paper.md` using `results/` (see the table of where each number comes from inside those files).
