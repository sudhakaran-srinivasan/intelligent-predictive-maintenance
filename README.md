# Intelligent Predictive Maintenance

An end-to-end predictive maintenance system for turbofan engines using machine learning and deep learning to estimate **Remaining Useful Life (RUL)** and support capacity-constrained maintenance decision-making.

---

## Overview

Predictive maintenance is more than forecasting equipment failure. The practical objective is to understand equipment degradation, estimate the remaining operational life of an asset, and translate those predictions into actionable maintenance decisions.

This project uses the **NASA C-MAPSS turbofan engine degradation dataset (FD001)** to develop an end-to-end predictive maintenance workflow.

The project combines:

- exploratory degradation analysis,
- leakage-safe feature engineering,
- classical machine-learning models,
- LSTM and GRU sequence models,
- explainable AI using feature importance and SHAP,
- official NASA test-set evaluation,
- a PEAS-based maintenance decision-support framework, and
- a simplified Mixed-Integer Linear Programming (MILP) model for maintenance scheduling under limited capacity.

The final workflow therefore connects **predictive modeling** with **prescriptive maintenance decision support**.

---

## Project Workflow

```text
Engine Sensor Data
        │
        ▼
Data Preparation & Exploratory Data Analysis
        │
        ▼
RUL Target Construction
        │
        ▼
Leakage-Safe Feature Engineering
        │
        ▼
Machine Learning Models
• Linear Regression
• Random Forest
• Gradient Boosting
• XGBoost
        │
        ▼
Sequence Models
• LSTM
• GRU
        │
        ▼
Model Evaluation
• RMSE
• MAE
• Asymmetric C-MAPSS Score
        │
        ▼
Explainability
• Feature Importance
• SHAP
        │
        ▼
Official NASA Test Evaluation
        │
        ▼
PEAS-Based Maintenance
Decision-Support Agent
        │
        ▼
Capacity-Constrained MILP
Maintenance Scheduling
```

---

## Dataset

This project uses the **FD001 subset** of the NASA C-MAPSS Jet Engine Simulated Data.

FD001 contains:

- 100 training engines,
- 100 test engines,
- one operating condition,
- one fault mode,
- 3 operational settings, and
- 21 sensor measurements recorded over engine operating cycles.

The training trajectories run until failure. The official test trajectories stop before failure, and NASA provides the Remaining Useful Life for the final observed cycle of each test engine.

### Required Raw Files

The following files are required:

```text
train_FD001.txt
test_FD001.txt
RUL_FD001.txt
```

Place them in:

```text
data/raw/CMAPSSData/
```

The final notebook checks for these files before beginning the analysis and raises a clear error if they are missing.

---

## Repository Structure

```text
intelligent-predictive-maintenance/
│
├── data/
│   ├── raw/
│   │   └── CMAPSSData/
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
├── README.md
├── pyproject.toml
├── uv.lock
├── .python-version
└── LICENSE
```

The first five notebooks document the incremental development of the project.

For reproducing the complete final analysis, use:

```text
notebooks/06_end_to_end_intelligent_predictive_maintenance.ipynb
```

---

## Methodology

### 1. Data Preparation and RUL Construction

The raw C-MAPSS files are loaded and organized by engine and operating cycle.

Remaining Useful Life is calculated for each training observation as:

```text
RUL = final engine cycle - current cycle
```

A piecewise-linear RUL target capped at **125 cycles** is used to reduce emphasis on differences during the early healthy-life region.

---

### 2. Leakage-Safe Model Development

Because each engine contributes many temporally related observations, random row-level splitting could allow cycles from the same engine to appear in both training and validation data.

To avoid this leakage, model development uses **engine-level splitting**.

Feature screening, scaling, cross-validation, hyperparameter selection, and recurrent-model selection are performed without using the official NASA test labels.

---

### 3. Feature Engineering

Tabular models use the available nonconstant sensor measurements together with backward-looking temporal features for selected degradation-sensitive sensors.

These include:

- first differences,
- rolling means,
- rolling standard deviations, and
- deviations from recent rolling means.

All temporal features use only the current and previous engine observations.

---

### 4. Machine Learning Models

The project evaluates:

- Mean prediction baseline
- Linear Regression
- Random Forest
- Gradient Boosting
- XGBoost

XGBoost hyperparameters are selected using grouped cross-validation so that observations from the same engine remain within the same fold.

---

### 5. Deep Learning Models

LSTM and GRU recurrent neural networks are used to learn degradation directly from ordered sensor histories.

Each sequence contains the previous **30 operating cycles** for an engine.

A bounded architecture comparison is performed for both model families, with early stopping used during development.

---

### 6. Model Evaluation

Models are evaluated using:

- **RMSE** — overall prediction error with greater penalty on large errors,
- **MAE** — average absolute prediction error, and
- **C-MAPSS Score** — an asymmetric prognostics metric that penalizes late RUL predictions more heavily than early predictions.

The official NASA test set is reserved for final evaluation.

The final comparison evaluates XGBoost, LSTM, and GRU on the **same 100 official FD001 test-engine endpoints**.

---

### 7. Explainability

Tree-based models are examined using:

- feature importance, and
- SHAP values.

These methods help identify which sensor and temporal features contribute most strongly to model predictions.

---

### 8. PEAS-Based Maintenance Decision Support

The selected recurrent model's RUL predictions are translated into a maintenance decision-support framework using **PEAS**:

- **Performance measure:** reduce risky maintenance delay while respecting limited capacity.
- **Environment:** the engine fleet and available maintenance slots.
- **Actuators:** schedule an engine, defer maintenance beyond the current planning horizon, or continue monitoring.
- **Sensors:** predicted RUL, derived risk tier, and LSTM-GRU model disagreement.

Model disagreement is used only as a practical **human-review signal** and is not treated as a calibrated confidence interval.

---

### 9. Capacity-Constrained MILP Scheduling

A simplified **Mixed-Integer Linear Programming (MILP)** model converts the predictive output into a capacity-constrained maintenance schedule.

The optimization determines whether an eligible engine should be:

- scheduled in planning period 1,
- scheduled in planning period 2,
- scheduled in planning period 3, or
- deferred beyond the current planning horizon.

The objective minimizes maintenance delay relative to predicted RUL while enforcing:

- one maintenance decision per eligible engine,
- limited maintenance capacity per period, and
- scheduling of Critical engines within the planning horizon when sufficient total capacity exists.

The maintenance periods, risk thresholds, and available maintenance slots are **simulated assumptions for demonstrating the decision-support framework**. They are not operational rules provided by NASA.

---

## Installation and Reproduction

### Option 1 — Local Machine / Jupyter

Python **3.11** is recommended.

Clone the repository:

```bash
git clone <repository-url>
cd intelligent-predictive-maintenance
```

Install the project dependencies using `uv`:

```bash
uv sync
```

The project dependencies are defined in `pyproject.toml`, and `uv.lock` provides the locked environment used for reproducibility.

Ensure the FD001 raw files are located at:

```text
data/raw/CMAPSSData/
```

Then launch Jupyter:

```bash
uv run jupyter lab
```

Open:

```text
notebooks/06_end_to_end_intelligent_predictive_maintenance.ipynb
```

and select:

```text
Restart Kernel and Run All Cells
```

The notebook automatically locates the repository root, so no machine-specific file paths need to be edited.

Generated directories and outputs are created automatically where needed.

---

## Google Colab

The notebook uses repository-relative paths. Therefore, when using Google Colab, the **full repository must be available in the Colab runtime** rather than opening only the notebook file.

In a Colab cell, clone the repository:

```python
!git clone <repository-url>
%cd intelligent-predictive-maintenance
```

Install the required dependencies:

```python
!pip install -e .
```

Verify that the required FD001 files exist under:

```text
data/raw/CMAPSSData/
```

Then run the final notebook.

> **Note:** Opening the `.ipynb` file directly from GitHub in Colab without cloning the repository will not provide the notebook with the required `data/`, `notebooks/`, and project files. Clone the full repository first.

---

## Reproducibility

The project is designed so that no machine-specific paths are required.

The final notebook:

- dynamically locates the repository root,
- validates the presence of required FD001 input files,
- automatically creates generated-data and output directories,
- uses fixed random seeds for the primary experiments,
- uses engine-level splitting to prevent data leakage,
- preserves the official NASA test set for final evaluation, and
- saves model artifacts, predictions, figures, and reports to structured repository directories.

For the cleanest reproduction test:

```text
Clone repository
      ↓
Install dependencies
      ↓
Verify FD001 raw files
      ↓
Open Notebook 06
      ↓
Restart Kernel
      ↓
Run All Cells
```

---

## Generated Outputs

Running the final notebook generates artifacts including:

### Models

```text
models/
```

Contains the final trained LSTM and GRU model checkpoints and preprocessing metadata.

### Predictions

```text
data/predictions/
```

Contains official NASA test predictions and model comparison outputs.

### Figures

```text
outputs/figures/
```

Contains degradation plots, model diagnostics, feature-importance plots, SHAP outputs, learning curves, and actual-versus-predicted RUL figures.

### Reports

```text
outputs/reports/
```

Contains evaluation summaries and maintenance decision-support outputs, including the optimized maintenance schedule.

---

## Key Limitations

This project is a course-scale decision-support prototype rather than a production aviation maintenance system.

Important limitations include:

- FD001 contains only one operating condition and one fault mode.
- C-MAPSS contains simulated rather than live airline operating data.
- The 125-cycle RUL cap is a modeling assumption supported by prior C-MAPSS research.
- Recurrent-model results use a fixed primary random seed rather than repeated multi-seed experiments.
- RUL predictions are point estimates rather than calibrated predictive distributions.
- LSTM-GRU disagreement is not a formal uncertainty interval.
- Maintenance thresholds and capacity assumptions are simulated.
- The MILP does not model actual staffing, hangar availability, maintenance duration, spare parts, mission schedules, or preventive/corrective maintenance costs.

These limitations are intentionally stated so that the maintenance optimization is interpreted as a demonstration of how RUL predictions can support constrained decision-making rather than as a deployable airline scheduling system.

---

## Research Context

The project methodology was informed by prior work on RUL prediction and predictive-maintenance decision support, including research showing the importance of temporal degradation modeling and connecting prognostic predictions to downstream maintenance decisions.

Key references include:

- Cao, X., Li, P., & Ming, S. (2021). Remaining useful life prediction-based maintenance decision model for stochastic deterioration equipment under data-driven. *Sustainability, 13*(15), 8548.
- Kamariotis, A., Tatsis, K., Chatzi, E., Goebel, K., & Straub, D. (2024). A metric for assessing and optimizing data-driven prognostic algorithms for predictive maintenance. *Reliability Engineering & System Safety, 242*, 109723.
- Mitici, M., de Pater, I., Barros, A., & Zeng, Z. (2023). Dynamic predictive maintenance for multiple components using data-driven probabilistic RUL prognostics: The case of turbofan engines. *Reliability Engineering & System Safety, 234*, 109199.
- Wang, L., Li, B., & Zhao, X. (2024). Multi-objective predictive maintenance scheduling models integrating remaining useful life prediction and maintenance decisions. *Computers & Industrial Engineering, 197*, 110581.

The maintenance optimization in this repository is intentionally simpler than the multi-component and multi-objective formulations used in published research. Its purpose is to demonstrate the progression from **RUL prediction → maintenance priority → constrained scheduling** within the scope of the course project.

---

## Final Project Objective

The project demonstrates how an ML/AI predictive-maintenance workflow can move beyond simply predicting equipment failure:

```text
Sensor Data
    ↓
Predict Remaining Useful Life
    ↓
Understand Prediction Drivers
    ↓
Assess Maintenance Urgency
    ↓
Allocate Limited Maintenance Capacity
    ↓
Support Maintenance Decisions
```

The result is an end-to-end demonstration of how predictive modeling and optimization can work together to support data-driven maintenance planning.