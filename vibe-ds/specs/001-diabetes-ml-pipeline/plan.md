# Implementation Plan: Diabetes ML Pipeline

**Branch**: `001-diabetes-ml-pipeline` | **Date**: 2025-01-27 | **Updated**: 2025-01-27 (In-line Documentation Requirements Added) | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/001-diabetes-ml-pipeline/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Build a fully reproducible, well-documented machine learning pipeline for multiclass classification of primary diagnosis from the Diabetes 130-US Hospitals dataset. The pipeline will be implemented as a Jupyter notebook with cells for each major step (15 phases: A through O), executing locally for data science analysis and model development.

**Update**: The pipeline now includes a secondary binary target variable (`readmitted_binary`) engineered from the `readmitted` column, which transforms readmission status into a binary classification target (1 if readmitted within 30 days, 0 otherwise). User Story 8 (Binary Readmission Classification) has been added to the specification, providing a complete binary classification pipeline parallel to the multiclass pipeline. This enables both multiclass diagnosis classification and binary readmission prediction tasks.

**Documentation Enhancement**: The specification now includes comprehensive in-line documentation requirements (User Story 7, FR-068 through FR-074, SC-014, SC-015) to ensure all code cells contain clear comments explaining major operations, parameter choices, non-obvious logic, data transformations, feature engineering steps, model configurations, and visualization purposes. This makes the codebase self-explanatory and easier to understand and maintain.

The technical approach involves using Python data science libraries (pandas, scikit-learn, XGBoost) to process raw patient data, engineer features, train multiple baseline models, perform hyperparameter tuning, evaluate performance, and create a saved pipeline artifact for inference. The notebook format enables iterative exploration and documentation of the analysis process.

## Technical Context

**Language/Version**: Python 3.10+ (Jupyter notebook format)  
**Primary Dependencies**: pandas, numpy, scikit-learn, xgboost, matplotlib, seaborn, shap, joblib, imbalanced-learn  
**Storage**: Local CSV files (diabetic_data.csv, IDS_mapping.csv), saved joblib pipeline artifacts  
**Testing**: Manual execution testing, notebook cell-by-cell validation (no automated unit tests - local analysis notebook)  
**Target Platform**: Local Jupyter notebook environment (JupyterLab/Jupyter Notebook)  
**Project Type**: Single notebook (data science analysis notebook)  
**Performance Goals**: Pipeline execution completes within reasonable time for dataset size (~100k records); inference on new data <5 seconds per prediction  
**Constraints**: Must run locally (no deployment); must be reproducible with fixed random seeds; must handle healthcare dataset size efficiently  
**Scale/Scope**: Single analyst use case; ~100k patient records; 15 pipeline phases (A-O) for multiclass + binary classification pipeline; 10 clinical diagnosis categories (multiclass target); 1 binary readmission target; 3 baseline multiclass models + 1 tuned multiclass model; 3 baseline binary models + 1 tuned binary model

## Current Implementation Status

**Primary Target (Multiclass Classification)**: ✅ Complete
- Phase A-O fully implemented for `diagnosis_category` target
- ICD-9 codes mapped to 10 clinical categories
- Full pipeline from data ingestion to model artifact creation

**Secondary Target (Binary Classification)**: ✅ Engineered | 📋 Specified
- Binary target `readmitted_binary` created after Phase C (Target Engineering)
- Transformation: 1 if `readmitted == '<30'`, 0 otherwise
- Distribution analysis and visualization included
- **User Story 8** added to specification with complete binary classification pipeline requirements
- **Status**: Binary classification pipeline specified and ready for implementation (Tasks T088-T106)

**Code Documentation**: 📋 Specified | ⏳ Pending Implementation
- **In-line documentation requirements** added to User Story 7 (FR-068 through FR-074, SC-014, SC-015)
- Requirements specify: comments for major operations, parameter choices, non-obvious logic, transformations, feature engineering, model configurations, visualizations, and function docstrings
- **Status**: Specification complete; in-line documentation needs to be added to all code cells in the notebook

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### I. Data Quality & Validation ✓
**Status**: Compliant
- Data validation will occur at notebook cell for data ingestion (Phase A)
- Missing value analysis, type checking, and schema validation included in Phase A and B
- Invalid data handling with logging documented in preprocessing cells
- Binary target engineering includes distribution validation

### II. Exploratory Data Analysis First ✓
**Status**: Compliant
- Comprehensive EDA phase (Phase D) performed after data cleaning
- Includes distribution analysis, correlation, missing patterns, outlier detection
- EDA findings documented with visualizations and summary statistics
- Binary target distribution analyzed and visualized

### III. Reproducibility & Versioning ✓
**Status**: Compliant
- Random seeds will be set explicitly for all stochastic operations
- Dependencies will be documented (requirements.txt or environment.yml)
- Code organized in modular cells with clear documentation
- Data versioning via file checksums/hashes

### IV. Feature Engineering & Transformation ✓
**Status**: Compliant
- Feature engineering steps traceable through notebook cells
- Transformations documented with input/output schemas
- Data lineage preserved through sequential notebook execution
- Feature selection decisions justified in documentation
- Both multiclass and binary target engineering documented

### V. Model Validation & Evaluation ✓
**Status**: Compliant
- Train/validation/test splits with stratification (Phase I for multiclass, User Story 8 for binary)
- Multiple evaluation metrics (accuracy, macro F1, weighted F1, confusion matrices for multiclass)
- Binary classification metrics (ROC-AUC, Precision-Recall AUC, F1) specified for User Story 8
- Baseline model comparison (Phase J for multiclass, User Story 8 for binary)
- Hyperparameter tuning with cross-validation (Phase K for multiclass, User Story 8 for binary)
- Final evaluation on held-out test set only (Phase L for multiclass, User Story 8 for binary)
- Both multiclass and binary classification pipelines follow same validation principles

### VI. Documentation & Communication ✓
**Status**: Compliant
- Markdown documentation report (Phase O)
- Code docstrings and comments in notebook cells
- **In-line documentation**: All code cells contain clear comments explaining major operations, parameter choices, and non-obvious logic (FR-068, FR-069, FR-070)
- **Transformation documentation**: Data transformations, feature engineering, and preprocessing decisions documented with explanatory comments (FR-071)
- **Model documentation**: Hyperparameters, evaluation metrics, and selection rationale documented with in-line comments (FR-072)
- **Visualization documentation**: Plot code includes comments explaining what is being plotted and why (FR-073)
- **Function documentation**: Reusable functions and complex code blocks have docstrings or header comments (FR-074)
- Visualizations with clear labels
- Clinical insights translation (Phase M)
- Binary target engineering documented with distribution analysis

**Constitution Compliance**: ✅ All principles satisfied

## Project Structure

### Documentation (this feature)

```text
specs/001-diabetes-ml-pipeline/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
# Jupyter notebook structure (local execution)
diabetes_ml_pipeline.ipynb    # Main pipeline notebook with cells for each phase
                              # Includes: multiclass target (diagnosis_category)
                              #          binary target (readmitted_binary)

# Supporting files
data/
├── diabetic_data.csv          # Raw dataset
└── IDS_mapping.csv            # ID mapping file (if needed)

artifacts/
├── diabetes_pipeline_model.joblib              # Saved pipeline artifact (multiclass model)
└── diabetes_binary_readmission_pipeline.joblib # Saved pipeline artifact (binary model)

outputs/
├── eda_visualizations/        # EDA plots and figures
├── model_results/             # Evaluation metrics, confusion matrices
└── documentation_report.md    # Final documentation report

requirements.txt               # Python dependencies
README.md                      # Project documentation
```

**Structure Decision**: Single Jupyter notebook format with organized cells for each of the 15 phases (A through O). Supporting data files in `data/` directory, saved artifacts in `artifacts/`, and generated outputs in `outputs/`. This structure supports local execution and iterative data science workflow while maintaining organization and reproducibility.

**Target Variables**:
- **Primary Target**: `diagnosis_category` (multiclass) - 10 clinical categories from ICD-9 codes
- **Secondary Target**: `readmitted_binary` (binary) - 1 if readmitted <30 days, 0 otherwise

## Implementation Phases

### Phase C - Target Engineering (Updated)
- ✅ Multiclass target: ICD-9 codes → 10 clinical categories
- ✅ Binary target: `readmitted` → `readmitted_binary` (1 if '<30', 0 otherwise)
- ✅ Distribution analysis and visualization for both targets

### Remaining Phases (Multiclass Pipeline)
- ✅ Phase D-O: Complete for multiclass classification

### Binary Classification Pipeline (User Story 8)
- 📋 **Specified**: User Story 8 with 19 tasks (T088-T106)
- Binary classification pipeline uses same preprocessing as multiclass
- Separate train/test split with stratification on `readmitted_binary`
- Binary baseline models: Logistic Regression, Random Forest, XGBoost (binary objective)
- Binary evaluation metrics: ROC-AUC, Precision-Recall AUC, F1 score
- Binary-specific visualizations: ROC curve, Precision-Recall curve, threshold analysis
- Separate pipeline artifact: `diabetes_binary_readmission_pipeline.joblib`
- **Status**: Ready for implementation

## Implementation Roadmap

**Note**: In-line documentation (User Story 7, FR-068 through FR-074) is a cross-cutting requirement that applies to all code cells across all phases. While documentation can be added incrementally, all code should eventually include appropriate comments explaining operations, parameter choices, and rationale.

### Phase 1: Multiclass Pipeline (Complete)
- ✅ Phases A-O: Data ingestion through model artifact creation
- ✅ All 87 tasks (T001-T087) completed
- ✅ Multiclass pipeline artifact saved
- ⏳ In-line documentation: Needs to be added to all code cells (cross-cutting requirement)

### Phase 2: Binary Classification Pipeline (Specified, Ready for Implementation)
- 📋 User Story 8 specified with 19 tasks (T088-T106)
- Can reuse preprocessing from multiclass pipeline
- Requires separate train/test split for binary target
- Binary model training, tuning, and evaluation
- Binary pipeline artifact creation
- ⏳ In-line documentation: Should be included during implementation (cross-cutting requirement)
- **Next**: Implement tasks T088-T106

### Phase 3: Documentation & Integration (Pending)
- **In-line Code Documentation** (New Requirement):
  - Add in-line comments to all code cells explaining major operations, steps, and transformations
  - Document parameter choices, thresholds, and configuration values with explanatory comments
  - Add comments for non-obvious logic, complex calculations, and domain-specific decisions
  - Document data transformations, feature engineering steps, and preprocessing decisions
  - Add comments for model hyperparameters, evaluation metrics, and selection rationale
  - Include comments for visualization code explaining what is being plotted and why
  - Add docstrings or header comments for reusable functions and complex code blocks
  - Follow consistent comment style: explain "why" not just "what", avoid redundant comments for obvious code
- Update Phase O documentation to include binary classification results
- Compare multiclass vs binary model performance
- Document both pipeline artifacts
- Tasks T107-T108 for final polish

## Future Considerations (Optional)

1. **Multi-Task Learning** (Advanced):
   - Joint modeling of both targets
   - Shared feature representations
   - Multi-objective optimization

2. **Model Comparison Analysis**:
   - Compare feature importance between multiclass and binary models
   - Analyze differences in predictive patterns
   - Clinical interpretation of both models

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

*No violations - all constitution principles satisfied*
