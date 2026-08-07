# FD001 Experiment Contract

This document defines the shared data and evaluation choices for notebooks 01–03.
It keeps preprocessing, baseline modeling, and advanced modeling comparable and
reproducible.

## Project scope

The current experiment uses only the NASA C-MAPSS **FD001** subset:

- 100 training trajectories;
- 100 test trajectories;
- one operating condition (sea level);
- one fault mode (high-pressure-compressor degradation).

FD001 is an appropriate first subset because it is the smallest and simplest
C-MAPSS scenario. It isolates degradation modeling from changes in operating
condition and fault type, which makes it suitable for a course project and an
initial comparison of regression algorithms.

This choice limits external validity. Results must not be generalized to six
operating conditions, fan degradation, or mixed fault modes without additional
testing on FD002, FD003, or FD004.

| Subset | Train engines | Test engines | Conditions | Fault modes |
|---|---:|---:|---:|---|
| FD001 | 100 | 100 | 1 | HPC degradation |
| FD002 | 260 | 259 | 6 | HPC degradation |
| FD003 | 100 | 100 | 1 | HPC and fan degradation |
| FD004 | 248 | 249 | 6 | HPC and fan degradation |

## Repository data contract

Notebooks must use files committed under the repository and must not depend on
Google Colab `/content` files or downloads from a mutable GitHub branch.

```text
datasets/
├── raw/CMAPSSData/
│   ├── train_FD001.txt
│   ├── test_FD001.txt
│   └── RUL_FD001.txt
└── processed/
    └── fd001_train_with_rul.csv
```

Notebook 01 is the owner of the processed training CSV. EDA-only fields such as
`RUL_bin` must not be saved in the canonical processed dataset.

## Column contract

Each raw record contains 26 space-separated values:

- `engine_id`;
- `cycle`;
- `setting1` through `setting3`;
- `sensor1` through `sensor21`.

The original description sometimes calls the final raw column “sensor
measurement 26,” referring to its file position. There are 21 sensor variables,
occupying raw columns 6–26.

Notebook 01 adds:

- `max_cycle`: final observed training cycle for the engine;
- `RUL`: `max_cycle - cycle`.

## Modeling contract

- Target used for the main comparison: `RUL_capped = min(RUL, 125)`.
- Constant sensors removed: `sensor1`, `sensor5`, `sensor10`, `sensor16`,
  `sensor18`, and `sensor19`.
- Predictors: three settings plus the remaining 15 sensors (18 total).
- Training/validation split: engine IDs, 80/20, `random_state=42`.
- The same engine may not appear in both training and validation.
- Official test evaluation: final observed cycle of each test engine, joined to
  `RUL_FD001.txt`.
- Primary metric: RMSE. Supporting metrics: MAE and R².
- Every table must label whether RUL is capped or uncapped.
- The official test set must not be used for hyperparameter selection.

## Notebook responsibilities

- `01_data_preparation_eda_degradation_analysis.ipynb`: validate raw FD001,
  construct RUL, perform EDA, and save the canonical processed CSV.
- `02_eda_baseline_ml_rul_prediction.ipynb`: construct the shared split, train
  baseline regressors, evaluate them, and save baseline artifacts.
- `03_advanced_ml_modeling.ipynb`: tune XGBoost with group-aware
  cross-validation and compare it fairly with notebook 02.

## Output contract

Small result files belong under `reports/`; reusable fitted artifacts belong
under `models/`. Review generated files before committing them.

Notebook 02 produces:

- `reports/baseline_model_comparison.csv`;
- `reports/baseline_test_predictions.csv`;
- `reports/baseline_engine_split.csv`;
- baseline scaler and model files under `models/`.

Notebook 03 produces:

- `reports/advanced_ml_model_comparison.csv`;
- `reports/advanced_ml_test_predictions.csv`;
- `reports/advanced_ml_cv_results.csv`;
- `reports/advanced_ml_engine_split.csv`;
- `models/tuned_xgboost_fd001.json`.

## Reproduction order

1. Restart the Python runtime.
2. Run notebook 01 from top to bottom.
3. Run notebook 02 from top to bottom.
4. Run notebook 03 from top to bottom.
5. Confirm every notebook completes without an error.
6. Review generated changes with `git status` before committing.
