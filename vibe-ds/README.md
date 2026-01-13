# Diabetes ML Pipeline

A fully reproducible, well-documented machine learning pipeline for multiclass classification of primary diagnosis from the Diabetes 130-US Hospitals dataset (UCI).

## Overview

This project implements a complete ML pipeline from raw data to saved model artifact, following best practices for healthcare datasets. The pipeline is organized as a Jupyter notebook with cells for each major step (15 phases: A through O).

## Dataset

- **Dataset**: Diabetes 130-US Hospitals (UCI)
- **Target Variable**: `primary_diagnosis` → Multiclass classification
- **Task**: Classification of diagnosis codes into clinically meaningful categories

## Project Structure

```
vibe-ds/
├── diabetes_ml_pipeline.ipynb    # Main pipeline notebook
├── data/                          # Raw dataset files
│   ├── diabetic_data.csv
│   └── IDS_mapping.csv
├── artifacts/                     # Saved model artifacts
│   └── diabetes_pipeline_model.joblib
├── outputs/                       # Generated outputs
│   ├── eda_visualizations/        # EDA plots and figures
│   ├── model_results/             # Evaluation metrics, confusion matrices
│   └── documentation_report.md    # Final documentation report
├── requirements.txt               # Python dependencies
└── README.md                      # This file
```

## Setup

### Prerequisites

- Python 3.10 or higher
- Jupyter Notebook or JupyterLab

### Installation

1. **Create a virtual environment** (recommended):

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

3. **Place dataset files**:

   - Copy `diabetic_data.csv` to `data/diabetic_data.csv`
   - Copy `IDS_mapping.csv` to `data/IDS_mapping.csv` (if needed)

4. **Launch Jupyter**:

   ```bash
   jupyter notebook diabetes_ml_pipeline.ipynb
   # OR
   jupyter lab diabetes_ml_pipeline.ipynb
   ```

## Pipeline Phases

The notebook is organized into 15 phases:

- **Phase A**: Data Ingestion & Structure
- **Phase B**: Cleaning & Preprocessing
- **Phase C**: Target Engineering
- **Phase D**: Exploratory Data Analysis (EDA)
- **Phase E**: Outlier Detection & Handling
- **Phase F**: Feature Encoding & Scaling
- **Phase G**: Class Imbalance Handling
- **Phase H**: Dimensionality Reduction (Optional)
- **Phase I**: Train / Test Split
- **Phase J**: Baseline Modeling
- **Phase K**: Hyperparameter Tuning
- **Phase L**: Final Evaluation
- **Phase M**: Interpretability
- **Phase N**: Pipeline & Artifacts
- **Phase O**: Documentation

## Usage

1. Open the notebook: `diabetes_ml_pipeline.ipynb`
2. Execute cells sequentially from top to bottom
3. Review outputs and visualizations at each phase
4. The final pipeline artifact will be saved to `artifacts/diabetes_pipeline_model.joblib`

## Reproducibility

- All random seeds are set for reproducibility
- Dependencies are pinned in `requirements.txt`
- Environment details are documented in the notebook

## License

[Add license information if applicable]

## References

- Dataset: [Diabetes 130-US Hospitals for Years 1999-2008](https://archive.ics.uci.edu/ml/datasets/diabetes+130-us+hospitals+for+years+1999-2008)
- UCI Machine Learning Repository
