# Intelligent Predictive Maintenance

An end-to-end AI-powered predictive maintenance system for turbofan engines using machine learning and deep learning to estimate Remaining Useful Life (RUL) and support maintenance decision-making.

---

## Overview

Predicting equipment failure is only one part of an effective predictive maintenance system. The broader challenge is understanding equipment degradation, estimating the remaining operational life of an asset, and translating model predictions into actionable maintenance decisions.

This project presents an end-to-end predictive maintenance workflow using the NASA C-MAPSS turbofan engine degradation dataset. The system analyzes multivariate sensor measurements collected over an engine's operational life, predicts Remaining Useful Life (RUL), compares multiple machine learning and deep learning models, and demonstrates how these predictions can support maintenance planning through a PEAS-based decision-support agent.

---

## Project Workflow

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
RUL Prediction Models
 ┌─────────────────────────────┐
 │ Machine Learning            │
 │ • Linear Regression         │
 │ • Random Forest             │
 │ • Gradient Boosting         │
 │ • XGBoost                   │
 └─────────────────────────────┘
                │
                ▼
 ┌─────────────────────────────┐
 │ Deep Learning               │
 │ • LSTM                      │
 │ • GRU                       │
 └─────────────────────────────┘
                │
                ▼
Model Evaluation & Comparison
                │
                ▼
Model Explainability (Feature Importance & SHAP)
                │
                ▼
Maintenance Decision Support
(PEAS-based Agent)
```

---

## Project Objectives

- Analyze engine degradation patterns using sensor measurements.
- Predict Remaining Useful Life (RUL) for turbofan engines.
- Compare traditional machine learning and deep learning approaches.
- Investigate temporal feature engineering for tabular models.
- Evaluate sequential learning using LSTM and GRU networks.
- Interpret model behavior using feature importance and SHAP.
- Demonstrate maintenance prioritization through a PEAS-based decision-support agent.

---

## Dataset

This project uses the **NASA C-MAPSS Jet Engine Simulated Data**.

The current implementation focuses on the **FD001** subset, which represents a single operating condition and a single fault mode.

Each engine contains sequential observations consisting of:

- Engine unit identifier
- Operational cycle
- Three operating settings
- Twenty-one sensor measurements

---

## Modeling Approaches

### Machine Learning

- Linear Regression
- Random Forest
- Gradient Boosting
- XGBoost

### Deep Learning

- Long Short-Term Memory (LSTM)
- Gated Recurrent Unit (GRU)

---

## Evaluation

Model performance is evaluated using:

- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- NASA C-MAPSS asymmetric scoring function

The best-performing models are further analyzed using feature importance and SHAP explanations before being incorporated into the maintenance decision-support workflow.

---

## Repository Structure

```text
intelligent-predictive-maintenance/
│
├── datasets/
│   └── raw/
│       └── CMAPSSData/
│
├── notebooks/
│   ├── 01_data_preparation_eda_degradation_analysis.ipynb
│   ├── 02_baseline_machine_learning.ipynb
│   ├── 03_advanced_machine_learning.ipynb
│   ├── 04_deep_learning.ipynb
│   └── 06_final_project.ipynb
│
├── outputs/
│   ├── processed/
│   ├── models/
│   └── reports/
│
├── README.md
├── LICENSE
├── requirements.txt
└── .gitignore
```

---

## Notebooks

The project was developed incrementally across multiple notebooks.

| Notebook | Purpose |
|-----------|---------|
| 01 | Data preparation and exploratory data analysis |
| 02 | Baseline machine learning experiments |
| 03 | Advanced machine learning experiments |
| 04 | Deep learning experiments |
| 06 | **Final integrated notebook containing the complete end-to-end predictive maintenance pipeline.** |

> **Note:** The recommended notebook for evaluation is **`06_final_project.ipynb`**, which consolidates the complete workflow, including data preparation, feature engineering, machine learning, deep learning, model evaluation, explainability, and the PEAS-based maintenance decision-support agent. The earlier notebooks document the incremental development of individual project components.

---

## Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd intelligent-predictive-maintenance
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Download the dataset

Place the following files inside:

```text
datasets/raw/CMAPSSData/
```

Required files:

- train_FD001.txt
- test_FD001.txt
- RUL_FD001.txt

### 4. Run the project

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open:

```text
notebooks/06_final_project.ipynb
```

Then select:

```text
Kernel → Restart Kernel and Run All Cells
```

---

## Generated Outputs

Running the final notebook automatically creates:

```text
outputs/
├── processed/
├── models/
└── reports/
```

These include processed datasets, trained models, evaluation summaries, explainability outputs, and maintenance decision reports.

---

## Reproducibility

This project was designed with reproducibility in mind.

- Fixed random seed used throughout the experiments.
- Engine-level train/validation split to prevent data leakage.
- Feature engineering uses only historical observations.
- Deep learning models are trained on sequential sensor trajectories.
- The official NASA test set is reserved for final model evaluation.

---

## Academic Context

This project was developed as part of **AAI 501 – Introduction to Artificial Intelligence** in the **Master of Science in Applied Artificial Intelligence** program.

The project integrates concepts from machine learning, deep learning, explainable AI, sequential modeling, and intelligent decision-support systems.

---

## Team

- Sudhakaran Srinivasan
- Ivan Da Silva
- Russell Miller
- Mina Habib

---

## License

This project is licensed under the MIT License.
