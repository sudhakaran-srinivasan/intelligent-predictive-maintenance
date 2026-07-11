# Intelligent Predictive Maintenance

An AI-powered predictive maintenance system for turbofan engines using
machine learning and deep learning for Remaining Useful Life (RUL)
estimation and maintenance decision support.

## Overview

Predicting equipment failure is only one part of an effective predictive
maintenance system. The broader challenge is understanding equipment
degradation, estimating remaining operational life, and translating
model predictions into actionable maintenance decisions.

This project explores an end-to-end intelligent predictive maintenance
workflow using the NASA C-MAPSS turbofan engine degradation dataset.

The system is designed to analyze multivariate sensor measurements
across engine operational cycles, estimate Remaining Useful Life (RUL),
assess engine health, and support maintenance prioritization.

## System Workflow

``` text
Engine Sensor Data
        ↓
Data Preparation & Degradation Analysis
        ↓
RUL Prediction
   ┌───────────────┐
   │ Machine       │
   │ Learning      │
   └───────────────┘
          vs.
   ┌───────────────┐
   │ Deep Learning │
   │ LSTM / GRU    │
   └───────────────┘
        ↓
Model Evaluation & Comparison
        ↓
Engine Health Assessment
        ↓
Maintenance Prioritization
        ↓
Maintenance Decision Support
```

## Project Objectives

-   Analyze engine degradation patterns across operational cycles.
-   Develop Remaining Useful Life (RUL) prediction models.
-   Compare traditional machine learning and deep learning approaches.
-   Explore LSTM/GRU models for sequential sensor data.
-   Translate predicted RUL into engine health and maintenance
    priorities.
-   Explore maintenance planning and optimization as an extension.

## Dataset

This project uses the **NASA C-MAPSS Jet Engine Simulated Data**.

The initial analysis focuses on the **FD001** subset, which contains
simulated turbofan engine run-to-failure trajectories under a single
operating condition and fault mode.

Each engine is represented by sequential observations containing:

-   Engine unit identifier
-   Operational cycle
-   Three operational settings
-   Twenty-one sensor measurements

## Planned Modeling Approaches

### Machine Learning

-   Linear Regression
-   Random Forest
-   Gradient Boosting / XGBoost

### Deep Learning

-   Long Short-Term Memory (LSTM)
-   Gated Recurrent Unit (GRU)

## Evaluation

RUL prediction models will be evaluated using regression metrics such
as:

-   Root Mean Squared Error (RMSE)
-   Mean Absolute Error (MAE)

Model performance and the ability to support downstream maintenance
decisions will be compared.

## Repository Structure

``` text
intelligent-predictive-maintenance/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── 01_data_preparation_eda_degradation_analysis.ipynb
│   ├── 02_eda_baseline_ml_rul_prediction.ipynb
│   ├── 03_advanced_ml_modeling.ipynb
│   └── 04_deep_learning_rul_prediction.ipynb
│
├── src/
│
├── reports/
│
├── requirements.txt
└── README.md
```

## Project Status

🚧 **In Development**

The project is currently in the planning and initial development phase.
Models, evaluation results, and maintenance decision components will be
updated as development progresses.

## Academic Context

This project is being developed as part of **AAI 501 -- Introduction to
Artificial Intelligence** in the Master's in Applied Artificial
Intelligence program.

The project explores concepts related to supervised machine learning,
deep learning, sequential modeling, intelligent decision-making, and
optimization.

## Team

Team project developed collaboratively by four graduate students in
Applied Artificial Intelligence.

## License

This project is licensed under the MIT License.
