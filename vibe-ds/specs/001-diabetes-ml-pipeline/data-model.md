# Data Model: Diabetes ML Pipeline

**Created**: 2025-01-27  
**Purpose**: Entity definitions and data structures for the ML pipeline

## Entities

### Raw Diabetes Dataset

**Description**: Original patient encounter data from Diabetes 130-US Hospitals (UCI) dataset

**Attributes**:
- Patient identifiers (to be removed): `encounter_id`, `patient_nbr`
- Demographic features: age, race, gender
- Admission details: admission_type_id, discharge_disposition_id, admission_source_id
- Medical codes: ICD-9 diagnosis codes (primary and secondary), ICD-9 procedure codes
- Medication information: medication details (names/IDs)
- Medical history: diabetes medications, diabetes diagnoses
- Lab/test results: number of lab procedures, number of procedures, number of medications, number of outpatient/inpatient/emergency visits
- Target variable: `primary_diagnosis` (ICD-9 codes)

**Data Types**:
- Numeric: count variables, age (if numeric), length of stay
- Categorical: race, gender, admission types, discharge dispositions
- String/Code: ICD-9 diagnosis codes
- Ordinal: age (if categorical age groups), time_in_hospital
- Identifier: encounter_id, patient_nbr (to be removed)

**Validation Rules**:
- All patient identifiers must be removed before modeling
- Placeholder missing values ('?', 'Unknown') must be converted to NaN
- Columns with >40% missing values must be dropped
- ICD-9 codes must be validated against expected format

**Relationships**:
- One patient (patient_nbr) may have multiple encounters (encounter_id)
- Each encounter has one primary diagnosis (primary_diagnosis)

**State Transitions**:
- Raw → Cleaned (removal of identifiers, normalization)
- Cleaned → Preprocessed (missing value handling, outlier treatment)
- Preprocessed → Feature Engineered (encoding, scaling)
- Feature Engineered → Model Ready (final feature set)

---

### Preprocessed Dataset

**Description**: Cleaned and preprocessed dataset ready for modeling

**Attributes**:
- All attributes from Raw Dataset except:
  - Removed: patient identifiers (encounter_id, patient_nbr)
  - Removed: leakage-prone features
  - Removed: columns with >40% missing values
- Normalized categorical values (lowercase, stripped whitespace)
- Imputed missing values:
  - Numeric features: median imputation
  - Categorical features: mode imputation
- Outlier-treated numeric features (capped, removed, or retained per feature)

**Data Types**:
- Numeric: continuous and count variables (after imputation)
- Categorical: normalized categories (after imputation)
- No identifiers or leakage-prone features

**Validation Rules**:
- No missing values (after imputation)
- All categorical values normalized
- No patient identifiers present
- Outlier treatment decisions documented per feature

**Data Lineage**:
- Original → Cleaned → Preprocessed
- Preserves relationships between original and processed features
- Transformation decisions logged

---

### Engineered Target Variable

**Description**: Categorical target variable derived from ICD-9 diagnosis codes, grouped into clinical categories

**Attributes**:
- Original: `primary_diagnosis` (ICD-9 codes)
- Engineered: `diagnosis_category` (clinical category)

**Categories** (10 clinical categories):
1. Circulatory (ICD-9: 390-459, 785)
2. Respiratory (ICD-9: 460-519, 786)
3. Digestive (ICD-9: 520-579, 787)
4. Diabetes (ICD-9: 250.x)
5. Injury (ICD-9: 800-999)
6. Musculoskeletal (ICD-9: 710-739)
7. Genitourinary (ICD-9: 580-629, 788)
8. Neoplasms (ICD-9: 140-239)
9. Mental disorders (ICD-9: 290-319)
10. Other / V-codes / E-codes (ICD-9: V-codes, E-codes, other)

**Validation Rules**:
- All ICD-9 codes must map to exactly one category
- Categories with insufficient samples (< minimum threshold) must be removed
- Final category distribution must be displayed
- Mapping logic must be documented

**Data Types**:
- String/Categorical: diagnosis_category

**Relationships**:
- One-to-one mapping: primary_diagnosis → diagnosis_category
- Many-to-one: Multiple ICD-9 codes → One clinical category

---

### Feature Engineered Dataset

**Description**: Dataset with encoded and scaled features ready for modeling

**Attributes**:
- Numeric features: StandardScaler scaled
- Categorical features:
  - Low-cardinality (<20 categories): One-hot encoded
  - High-cardinality (≥20 categories): Frequency or target-independent encoded
- Optional: PCA-transformed numeric features (if dimensionality reduction applied)

**Data Types**:
- Numeric: Continuous (scaled, potentially PCA-transformed)
- Binary/Categorical: One-hot encoded features
- Numeric: Frequency-encoded categorical features

**Validation Rules**:
- All features must be numeric (after encoding)
- No missing values
- Train/test split must preserve class distribution (stratified)
- Transformers fit only on training data (no data leakage)

**Data Lineage**:
- Preprocessed → Encoded → Scaled → (Optional PCA) → Feature Engineered
- Transformation pipeline documented
- Fit parameters saved for inference

---

### Model Artifacts

**Description**: Trained models and evaluation results

**Attributes**:
- **Baseline Models**:
  - Multinomial Logistic Regression
  - Random Forest
  - XGBoost
- **Tuned Model**: Best-performing baseline after hyperparameter tuning
- **Pipeline**: scikit-learn Pipeline containing preprocessing + model
- **Evaluation Metrics**: Accuracy, Macro F1, Weighted F1, Confusion Matrix, Per-class Precision/Recall

**Data Types**:
- Model objects: scikit-learn/XGBoost model objects
- Pipeline: scikit-learn Pipeline object
- Metrics: Numeric (accuracy, F1 scores), Arrays (confusion matrices)

**Validation Rules**:
- Models trained only on training data
- Hyperparameter tuning uses cross-validation (no test set leakage)
- Test set evaluation performed only once (after final model selection)
- All random seeds set for reproducibility

**Persistent Storage**:
- Saved as joblib files (.joblib extension)
- Pipeline artifact: `diabetes_pipeline_model.joblib`
- Includes version metadata and transformation steps

---

### EDA Artifacts

**Description**: Visualizations, statistics, and summary reports from exploratory data analysis

**Attributes**:
- Distribution plots for numeric features
- Value count visualizations for categorical features
- Target class distribution and imbalance metrics
- Correlation matrix for numeric features
- Outlier visualizations (boxplots)
- Summary statistics
- EDA findings summary document

**Data Types**:
- Images: Distribution plots, boxplots, correlation heatmaps
- Tables: Summary statistics, correlation matrices
- Text: EDA findings summary

**Validation Rules**:
- All visualizations must have clear labels and titles
- Summary statistics must be accurate
- Findings must be documented with modeling implications

**Relationships**:
- Generated from Preprocessed Dataset
- Informs feature engineering and model selection decisions

---

## Data Flow

```
Raw Dataset (CSV)
  ↓ [Phase A: Data Ingestion]
Loaded DataFrame
  ↓ [Phase B: Cleaning & Preprocessing]
Preprocessed Dataset
  ↓ [Phase C: Target Engineering]
Dataset with Engineered Target
  ↓ [Phase D: EDA]
EDA Artifacts + Insights
  ↓ [Phase E: Outlier Handling]
Outlier-Treated Dataset
  ↓ [Phase F: Feature Encoding & Scaling]
Feature Engineered Dataset
  ↓ [Phase G: Class Imbalance Handling]
Balanced Dataset (if applicable)
  ↓ [Phase H: Dimensionality Reduction (Optional)]
Reduced-Dimension Dataset (optional)
  ↓ [Phase I: Train/Test Split]
Training Set + Test Set
  ↓ [Phase J: Baseline Modeling]
Baseline Models + Evaluation
  ↓ [Phase K: Hyperparameter Tuning]
Tuned Model
  ↓ [Phase L: Final Evaluation]
Final Model Performance
  ↓ [Phase M: Interpretability]
Feature Importance + SHAP Values
  ↓ [Phase N: Pipeline Creation]
Saved Pipeline Artifact
  ↓ [Phase O: Documentation]
Final Documentation Report
```

## Data Validation Checkpoints

1. **After Phase A (Data Ingestion)**:
   - Dataset shape matches expected
   - All expected columns present
   - Data types correct

2. **After Phase B (Preprocessing)**:
   - No identifiers present
   - Missing values handled (imputed or removed)
   - All categorical values normalized

3. **After Phase C (Target Engineering)**:
   - All ICD-9 codes mapped to categories
   - Category distribution reasonable
   - No categories below minimum threshold

4. **After Phase F (Feature Engineering)**:
   - All features numeric
   - No missing values
   - Train/test split preserves class distribution

5. **After Phase N (Pipeline Creation)**:
   - Pipeline saves successfully
   - Pipeline can load and make predictions
   - Predictions match expected format
