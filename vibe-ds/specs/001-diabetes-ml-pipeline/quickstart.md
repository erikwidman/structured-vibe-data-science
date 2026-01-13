# Quickstart: Diabetes ML Pipeline

**Created**: 2025-01-27  
**Purpose**: Quick reference for executing and validating the ML pipeline notebook

## Prerequisites

1. **Python Environment**: Python 3.10+
2. **Jupyter Notebook**: JupyterLab or Jupyter Notebook
3. **Dataset Files**: 
   - `diabetic_data.csv` (Diabetes 130-US Hospitals dataset)
   - `IDS_mapping.csv` (if needed for reference)
4. **Dependencies**: See `requirements.txt`

## Setup

1. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Launch Jupyter Notebook**:
   ```bash
   jupyter notebook diabetes_ml_pipeline.ipynb
   # OR
   jupyter lab diabetes_ml_pipeline.ipynb
   ```

3. **Verify Data Files**:
   - Ensure `diabetic_data.csv` is in the `data/` directory
   - Check file size and basic structure

## Execution Flow

### Phase A: Data Ingestion & Structure
**Expected Output**: Dataset shape, column names, data types, sample rows, summary statistics, missing value table

**Validation**:
- ✅ Dataset loads without errors
- ✅ Shape matches expected (~100k rows)
- ✅ All expected columns present
- ✅ Missing value percentage table generated

**Checkpoint**: Verify dataset structure matches expectations

---

### Phase B: Cleaning & Preprocessing
**Expected Output**: Cleaned dataset, removed identifiers, normalized values, imputed missing values, preprocessing log

**Validation**:
- ✅ Patient identifiers (encounter_id, patient_nbr) removed
- ✅ Leakage-prone features identified and removed
- ✅ Categorical values normalized (lowercase, stripped)
- ✅ Placeholder missing values ('?', 'Unknown') → NaN
- ✅ Columns with >40% missing values dropped
- ✅ Missing values imputed (numeric: median, categorical: mode)
- ✅ Preprocessing decisions logged

**Checkpoint**: Verify no identifiers present, all missing values handled

---

### Phase C: Target Engineering
**Expected Output**: Engineered target variable, category mapping, final class distribution

**Validation**:
- ✅ All ICD-9 codes mapped to clinical categories
- ✅ 10 clinical categories defined (or fewer if categories removed)
- ✅ Categories below minimum threshold removed
- ✅ Final class distribution displayed (counts and percentages)

**Checkpoint**: Verify target variable created with reasonable distribution

---

### Phase D: Exploratory Data Analysis
**Expected Output**: Distribution plots, value count visualizations, correlation matrix, target imbalance analysis, EDA summary

**Validation**:
- ✅ Distribution plots for all numeric features
- ✅ Value count visualizations for categorical features
- ✅ Target class imbalance metrics displayed
- ✅ Correlation matrix computed and visualized
- ✅ Skewness, multicollinearity, potential leakage identified
- ✅ EDA findings summary generated

**Checkpoint**: Review EDA insights and validate data characteristics

---

### Phase E: Outlier Detection & Handling
**Expected Output**: Outlier identification, boxplots, outlier handling strategy, before/after statistics

**Validation**:
- ✅ Numeric outliers identified using IQR method
- ✅ Outliers visualized with boxplots
- ✅ Outlier handling strategy decided per feature (cap/remove/retain)
- ✅ Strategy applied with justification
- ✅ Before/after statistics displayed

**Checkpoint**: Verify outlier handling decisions are reasonable

---

### Phase F: Feature Encoding & Scaling
**Expected Output**: Encoded features, scaled numeric features, no data leakage

**Validation**:
- ✅ Categorical features encoded (one-hot for low-cardinality, frequency/target-independent for high-cardinality)
- ✅ Numeric features scaled using StandardScaler
- ✅ Transformers fit only on training data (after train/test split)
- ✅ No data leakage (test set not seen during fitting)

**Checkpoint**: Verify all features numeric, no missing values, no leakage

---

### Phase G: Class Imbalance Handling
**Expected Output**: Imbalance metrics, balancing strategy applied, justification

**Validation**:
- ✅ Multiclass imbalance quantified
- ✅ Balancing strategy applied (class weights, SMOTE, or class removal)
- ✅ Strategy justified in documentation
- ✅ Balanced dataset ready for modeling (if applicable)

**Checkpoint**: Verify balancing strategy is appropriate for dataset

---

### Phase H: Dimensionality Reduction (Optional)
**Expected Output**: PCA analysis, variance explained, 2-D visualization, decision on inclusion

**Validation** (if applied):
- ✅ PCA applied to numeric features
- ✅ Components explaining ≥95% variance identified
- ✅ 2-D visualization (PCA or UMAP) colored by diagnosis class
- ✅ Decision documented (include or exclude in final pipeline)

**Checkpoint**: Verify PCA components are meaningful (if applied)

---

### Phase I: Train / Test Split
**Expected Output**: Training set, test set, class distribution verification

**Validation**:
- ✅ Data split 80/20 with stratification on target
- ✅ Class distribution consistent across splits
- ✅ Random seed set for reproducibility
- ✅ Test set isolated (not used until final evaluation)

**Checkpoint**: Verify split preserves class distribution

---

### Phase J: Baseline Modeling
**Expected Output**: Three trained baseline models, evaluation metrics, comparison table

**Validation**:
- ✅ Three baseline models trained:
  - Multinomial Logistic Regression
  - Random Forest
  - XGBoost
- ✅ All models evaluated with: Accuracy, Macro F1, Weighted F1, Confusion Matrix
- ✅ Results compared in single table
- ✅ Best-performing model identified

**Checkpoint**: Verify all models train successfully, metrics computed correctly

---

### Phase K: Hyperparameter Tuning
**Expected Output**: Tuned model, best hyperparameters, CV performance

**Validation**:
- ✅ Best baseline model selected for tuning
- ✅ RandomizedSearchCV with 5-fold stratified cross-validation
- ✅ Weighted F1 score optimized
- ✅ Best hyperparameters reported
- ✅ Cross-validation performance reported

**Checkpoint**: Verify tuning improves performance over baseline

---

### Phase L: Final Evaluation
**Expected Output**: Test set evaluation, classification report, confusion matrix, per-class metrics

**Validation**:
- ✅ Tuned model evaluated on held-out test set (only once)
- ✅ Classification report generated
- ✅ Normalized confusion matrix displayed
- ✅ Per-class precision and recall computed
- ✅ Optional: Top-k accuracy computed

**Checkpoint**: Verify final model performance meets success criteria

---

### Phase M: Interpretability
**Expected Output**: Feature importance, SHAP summary plots, per-class SHAP contributions, clinical insights

**Validation**:
- ✅ Feature importance computed and visualized
- ✅ SHAP summary plot generated
- ✅ Per-class SHAP contributions displayed
- ✅ Findings translated into clinical/operational insights

**Checkpoint**: Verify interpretability insights are meaningful

---

### Phase N: Pipeline & Artifacts
**Expected Output**: Saved pipeline artifact, example inference code

**Validation**:
- ✅ scikit-learn Pipeline created (preprocessing + model)
- ✅ Pipeline saved as joblib file
- ✅ Example inference code provided
- ✅ Pipeline can load and make predictions on new data
- ✅ Prediction time <5 seconds per prediction

**Checkpoint**: Verify pipeline artifact works for inference

---

### Phase O: Documentation
**Expected Output**: Markdown documentation report

**Validation**:
- ✅ Documentation includes all required sections:
  - Dataset overview
  - Target engineering rationale
  - EDA insights
  - Modeling approach
  - Evaluation results
  - Limitations and future work
- ✅ Code is modular and well-documented
- ✅ All docstrings present
- ✅ Code follows PEP 8 style guide

**Checkpoint**: Verify documentation is complete and accurate

## Common Issues & Solutions

### Issue: Dataset doesn't load
**Solution**: Verify file path, check file encoding, ensure pandas version compatible

### Issue: Memory errors
**Solution**: Check dataset size, consider dtype optimization, close other applications

### Issue: Model training fails
**Solution**: Verify data preprocessing complete, check for NaN values, verify feature types

### Issue: Pipeline save fails
**Solution**: Ensure all transformers are scikit-learn compatible, check disk space

### Issue: Inference fails on new data
**Solution**: Verify input data schema matches training data, check pipeline loaded correctly

## Testing Checklist

- [ ] All notebook cells execute without errors
- [ ] Dataset loads and validates correctly
- [ ] Preprocessing decisions logged
- [ ] Target engineering produces reasonable distribution
- [ ] EDA insights documented
- [ ] Models train successfully
- [ ] Evaluation metrics computed correctly
- [ ] Pipeline artifact saves and loads successfully
- [ ] Inference works on new data
- [ ] Documentation is complete
- [ ] Code follows PEP 8 style guide
- [ ] All random seeds set for reproducibility

## Success Criteria Validation

- **SC-001**: All 15 phases execute end-to-end without errors ✅
- **SC-002**: Final model achieves weighted F1 score above baseline ✅
- **SC-003**: Model performance balanced across categories ✅
- **SC-004**: Inference time <5 seconds per prediction ✅
- **SC-005**: All preprocessing decisions documented ✅
- **SC-006**: Documentation report complete ✅
- **SC-007**: Code quality checks pass ✅
- **SC-008**: Edge cases handled gracefully ✅
- **SC-009**: EDA findings identify key insights ✅
- **SC-010**: Interpretability features identify top features ✅
