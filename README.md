# Intelligent Predictive Maintenance

An end-to-end AI-powered predictive maintenance system for turbofan engines using machine learning and deep learning to estimate **Remaining Useful Life (RUL)** and support maintenance decision-making.

---

# Overview

Predictive maintenance is more than forecasting equipment failure. The real objective is to understand equipment degradation, estimate the remaining operational life of an asset, and translate model predictions into actionable maintenance decisions.

This project presents a complete predictive maintenance pipeline using the NASA C-MAPSS turbofan engine degradation dataset. The workflow integrates data preparation, feature engineering, machine learning, deep learning, explainable AI, and a PEAS-based maintenance decision-support agent to demonstrate how AI can support maintenance planning under maintenance capacity constraints.

---

# Project Workflow

```text
Engine Sensor Data
        │
        ▼
Data Preparation & Exploratory Data Analysis
        │
        ▼
Feature Engineering
        │
        ▼
Remaining Useful Life (RUL) Prediction
 ┌───────────────────────────────┐
 │ Machine Learning              │
 │ • Linear Regression           │
 │ • Random Forest               │
 │ • Gradient Boosting           │
 │ • XGBoost                     │
 └───────────────────────────────┘
                │
                ▼
 ┌───────────────────────────────┐
 │ Deep Learning                 │
 │ • LSTM                        │
 │ • GRU                         │
 └───────────────────────────────┘
                │
                ▼
Model Evaluation & Explainability
(Feature Importance & SHAP)
                │
                ▼
PEAS-based Maintenance
Decision-Support Agent
```

---

# Repository Guide

The project was developed incrementally across multiple notebooks before being consolidated into a single reproducible workflow.

| Notebook | Purpose |
|----------|---------|
| 01_data_preparation_eda_degradation_analysis.ipynb | Data preparation and exploratory data analysis |
| 02_eda_baseline_ml_rul_prediction.ipynb | Baseline machine learning models |
| 03_advanced_ml_modeling.ipynb | Advanced machine learning models |
| 04_deep_learning_rul_prediction.ipynb | Deep learning models (LSTM & GRU) |
| 05_maintenance_decision_support.ipynb | PEAS-based maintenance decision-support agent |
| **06_end_to_end_intelligent_predictive_maintenance.ipynb** | **Complete end-to-end implementation of the project** |

> **For project evaluation, only `06_end_to_end_intelligent_predictive_maintenance.ipynb` needs to be executed.**
>
> Notebooks **01–05** document the incremental development of individual project components and are included for transparency and reference.

---

# Project Objectives

- Analyze engine degradation using multivariate sensor measurements.
- Predict Remaining Useful Life (RUL) of turbofan engines.
- Compare traditional machine learning and deep learning approaches.
- Engineer temporal features for tabular models.
- Evaluate sequence learning using LSTM and GRU.
- Explain model predictions using Feature Importance and SHAP.
- Demonstrate AI-assisted maintenance prioritization through a PEAS-based decision-support agent.

---

# Dataset

This project uses the **NASA C-MAPSS Jet Engine Simulated Data**.

The implementation focuses on the **FD001** subset, which represents:

- Single operating condition
- Single fault mode

Each engine contains sequential observations consisting of:

- Engine identifier
- Operating cycle
- Three operating settings
- Twenty-one sensor measurements

---

# Models Implemented

## Machine Learning

- Linear Regression
- Random Forest
- Gradient Boosting Regressor
- XGBoost Regressor

## Deep Learning

- Long Short-Term Memory (LSTM)
- Gated Recurrent Unit (GRU)

---

# Evaluation Metrics

Models are evaluated using:

- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- NASA C-MAPSS asymmetric scoring function

Model behaviour is further interpreted using:

- Feature Importance
- SHAP
- Prediction error analysis
- Predicted vs. Actual RUL comparisons

---

# Repository Structure

```text
intelligent-predictive-maintenance/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── predictions/
│
├── models/
│
├── notebooks/
│   ├── 01_data_preparation_eda_degradation_analysis.ipynb
│   ├── 02_eda_baseline_ml_rul_prediction.ipynb
│   ├── 03_advanced_ml_modeling.ipynb
│   ├── 04_deep_learning_rul_prediction.ipynb
│   ├── 05_maintenance_decision_support.ipynb
│   └── 06_end_to_end_intelligent_predictive_maintenance.ipynb
│
├── outputs/
│   ├── figures/
│   └── reports/
│
├── LICENSE
├── README.md
├── pyproject.toml
├── uv.lock
├── .python-version
└── .gitignore
```

---

# Running the Project

## 1. Clone the repository

```bash
git clone <repository-url>
cd intelligent-predictive-maintenance
```

---

## 2. Install dependencies

This project uses **uv** for dependency management.

```bash
uv sync
```

Activate the virtual environment.

### macOS / Linux

```bash
source .venv/bin/activate
```

### Windows

```powershell
.venv\Scripts\activate
```

---

## 3. Download the NASA FD001 dataset

Place the following files inside:

```text
data/raw/CMAPSSData/
```

Required files:

```text
train_FD001.txt
test_FD001.txt
RUL_FD001.txt
```

The notebook automatically validates that these files exist before execution.

---

## 4. Launch Jupyter

```bash
jupyter lab
```

or

```bash
jupyter notebook
```

Open:

```text
notebooks/06_end_to_end_intelligent_predictive_maintenance.ipynb
```

Select:

```text
Kernel → Restart Kernel and Run All Cells
```

No project paths need to be modified manually.

---

# Generated Outputs

Running the final notebook automatically generates:

```text
data/
├── processed/
└── predictions/

models/

outputs/
├── figures/
└── reports/
```

These include:

- Processed datasets
- Engine metadata
- Model predictions
- Saved deep learning models
- Evaluation metrics
- Feature importance and SHAP visualizations
- Maintenance prioritization outputs
- Capacity analysis results

---

# Reproducibility

This project was designed for reproducible execution.

- Project-relative file paths (no machine-specific paths)
- Python version managed through `.python-version`
- Dependencies managed using `pyproject.toml`
- Locked package versions via `uv.lock`
- Automatic creation of required output directories
- Automatic validation of required dataset files
- Fixed random seeds
- Engine-level train/validation split to prevent data leakage
- Sequential deep learning models trained on complete engine trajectories
- Official NASA test set reserved for final evaluation

---

# PEAS-based Maintenance Decision-Support Agent

The final stage of the project demonstrates how predicted Remaining Useful Life can support maintenance planning.

The decision-support agent:

- Classifies engines into maintenance risk tiers
- Prioritizes engines using predicted Remaining Useful Life
- Generates a maintenance queue
- Evaluates maintenance capacity constraints
- Estimates remaining maintenance backlog

The objective is to illustrate how predictive models can support operational maintenance decisions rather than being used solely for prediction.

---

# Academic Context

Developed as part of

**AAI 501 – Introduction to Artificial Intelligence**

Master of Science in Applied Artificial Intelligence

---

# Team

- Sudhakaran Srinivasan
- Ivan Da Silva
- Russell Miller
- Mina Habib

---

# License

This project is licensed under the MIT License.