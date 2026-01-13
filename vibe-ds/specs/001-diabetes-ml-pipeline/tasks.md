# Tasks: Diabetes ML Pipeline

**Input**: Design documents from `/specs/001-diabetes-ml-pipeline/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story. User Stories 1-7 map to 15 pipeline phases (A through O) for multiclass classification. User Story 8 implements binary classification pipeline using the same preprocessing. User Story 7 includes both high-level documentation (Phase O) and in-line code documentation (cross-cutting requirement for all phases).

## Task Summary

**Total Tasks**: 122
- **Completed**: 107 tasks (T001-T087, T034a, T088-T106)
- **Pending**: 15 tasks (T107-T108 for documentation, T109-T120a for in-line documentation)

**User Stories**:
- **US1-US7**: Complete (multiclass classification pipeline)
- **US8**: ✅ Complete (binary classification pipeline implemented)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Jupyter Notebook**: `diabetes_ml_pipeline.ipynb` at repository root
- **Data files**: `data/diabetic_data.csv`, `data/IDS_mapping.csv`
- **Artifacts**: 
  - `artifacts/diabetes_pipeline_model.joblib` (multiclass model)
  - `artifacts/diabetes_binary_readmission_pipeline.joblib` (binary model)
- **Outputs**: `outputs/` directory for visualizations and reports
- **Dependencies**: `requirements.txt` at repository root

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [x] T001 Create project directory structure (data/, artifacts/, outputs/)
- [x] T002 [P] Create requirements.txt with all dependencies (pandas, numpy, scikit-learn, xgboost, matplotlib, seaborn, shap, joblib, imbalanced-learn)
- [x] T003 [P] Create README.md with project overview and setup instructions
- [x] T004 [P] Create diabetes_ml_pipeline.ipynb notebook file
- [x] T005 [P] Create outputs/eda_visualizations/ directory
- [x] T006 [P] Create outputs/model_results/ directory

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T007 Create initial notebook structure with imports cell (Phase A header cell)
- [x] T008 [P] Add reproducibility setup cell (random seeds: numpy, random, sklearn, xgboost)
- [x] T009 [P] Add data directory verification cell (check data/ directory exists and files present)
- [x] T010 Create helper functions cell for common utilities (file paths, data validation helpers)

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Data Ingestion and Structure Analysis (Priority: P1) 🎯 MVP

**Goal**: Load and understand the raw diabetes dataset structure, including data types, missing values, and column characteristics

**Independent Test**: Load dataset, display shape/columns/types/statistics, generate missing value table - delivers immediate visibility into data quality and structure

**Pipeline Phases Covered**: Phase A (Data Ingestion & Structure)

### Implementation for User Story 1

- [x] T011 [US1] Create Phase A header markdown cell with title "PHASE A — DATA INGESTION & STRUCTURE"
- [x] T012 [US1] Create data loading cell in diabetes_ml_pipeline.ipynb (load diabetic_data.csv into pandas DataFrame)
- [x] T013 [US1] Create dataset shape and metadata cell (display shape, column names, data types, head)
- [x] T014 [US1] Create summary statistics cell (df.describe() and info() output)
- [x] T015 [US1] Create column categorization cell (identify numeric, categorical, ordinal, identifier, leakage-prone columns)
- [x] T016 [US1] Create missing value analysis cell (generate missing value percentage table for all columns)

**Checkpoint**: At this point, User Story 1 should be fully functional - dataset loaded and structure analyzed

---

## Phase 4: User Story 2 - Data Cleaning and Preprocessing (Priority: P1)

**Goal**: Clean and preprocess raw data by removing identifiers, normalizing values, handling missing data, and preparing features for modeling

**Independent Test**: Apply cleaning transformations, verify identifiers removed, values normalized, missing values handled, preprocessing decisions logged - delivers clean, analysis-ready dataset

**Pipeline Phases Covered**: Phase B (Cleaning & Preprocessing), Phase E (Outlier Detection & Handling)

### Implementation for User Story 2

- [x] T017 [US2] Create Phase B header markdown cell with title "PHASE B — CLEANING & PREPROCESSING"
- [x] T018 [US2] Create identifier removal cell (remove encounter_id, patient_nbr and other leakage-prone features)
- [x] T019 [US2] Create categorical normalization cell (lowercase, strip whitespace for all categorical columns)
- [x] T020 [US2] Create placeholder conversion cell (convert '?', 'Unknown' to NaN)
- [x] T021 [US2] Create high missing value filter cell (drop columns with >40% missing values with justification logging)
- [x] T022 [US2] Create imputation cell (numeric: median, categorical: mode) in diabetes_ml_pipeline.ipynb
- [x] T023 [US2] Create preprocessing decisions log cell (document all decisions with justifications)
- [x] T024 [US2] Create Phase E header markdown cell with title "PHASE E — OUTLIER DETECTION & HANDLING"
- [x] T025 [US2] Create outlier detection cell (IQR method for numeric features)
- [x] T026 [US2] Create outlier visualization cell (boxplots for numeric features with outliers)
- [x] T027 [US2] Create outlier handling strategy cell (decide cap/remove/retain per feature with justification)
- [x] T028 [US2] Create outlier handling application cell (apply strategy and show before/after statistics)

**Checkpoint**: At this point, User Story 2 should be fully functional - data cleaned and preprocessed, outliers handled

---

## Phase 5: User Story 3 - Target Variable Engineering (Priority: P1)

**Goal**: Convert raw ICD-9 diagnosis codes into clinically meaningful categories suitable for multiclass classification modeling

**Independent Test**: Map ICD-9 codes to clinical categories, remove rare categories, display final class distribution - delivers well-structured target variable ready for modeling

**Pipeline Phases Covered**: Phase C (Target Engineering)

### Implementation for User Story 3

- [x] T029 [US3] Create Phase C header markdown cell with title "PHASE C — TARGET ENGINEERING"
- [x] T030 [US3] Create ICD-9 mapping dictionary cell (define mapping from ICD-9 codes to 10 clinical categories)
- [x] T031 [US3] Create target engineering function cell (convert primary_diagnosis codes to categories: Circulatory, Respiratory, Digestive, Diabetes, Injury, Musculoskeletal, Genitourinary, Neoplasms, Mental disorders, Other/V/E-codes)
- [x] T032 [US3] Create target category application cell (apply mapping to create diagnosis_category column) in diabetes_ml_pipeline.ipynb
- [x] T033 [US3] Create rare category filter cell (remove categories below minimum sample threshold with justification)
- [x] T034 [US3] Create class distribution display cell (show final class distribution with counts and percentages)
- [x] T034a [US3] Create binary target engineering cell (transform 'readmitted' column into binary target: 1 if readmitted == '<30', 0 otherwise, with distribution analysis and visualization) in diabetes_ml_pipeline.ipynb

**Checkpoint**: At this point, User Story 3 should be fully functional - target variable engineered with reasonable distribution (multiclass and binary targets available)

---

## Phase 6: User Story 4 - Exploratory Data Analysis (Priority: P2)

**Goal**: Explore data characteristics, relationships, and patterns to inform feature engineering and model selection decisions

**Independent Test**: Generate visualizations, compute statistics, produce summary of findings - delivers data-driven insights for modeling strategy

**Pipeline Phases Covered**: Phase D (Exploratory Data Analysis)

### Implementation for User Story 4

- [x] T035 [US4] Create Phase D header markdown cell with title "PHASE D — EXPLORATORY DATA ANALYSIS (EDA)"
- [x] T036 [US4] Create numeric distribution plots cell (plot distributions for all numeric features using matplotlib/seaborn)
- [x] T037 [US4] Create categorical value counts cell (plot value counts for categorical features)
- [x] T038 [US4] Create target imbalance analysis cell (analyze and display target class imbalance metrics)
- [x] T039 [US4] Create correlation matrix cell (compute and visualize correlation matrix for numeric variables) in diabetes_ml_pipeline.ipynb
- [x] T040 [US4] Create data quality summary cell (identify skewness, multicollinearity, potential feature leakage)
- [x] T041 [US4] Create EDA findings summary cell (document insights and modeling implications in markdown)

**Checkpoint**: At this point, User Story 4 should be fully functional - EDA complete with documented insights

---

## Phase 7: User Story 5 - Model Development and Evaluation (Priority: P1)

**Goal**: Train, tune, and evaluate multiple machine learning models to identify the best-performing approach for the classification task

**Independent Test**: Train baseline models, tune best performer, evaluate on test data with comprehensive metrics - delivers validated, production-ready model

**Pipeline Phases Covered**: Phase F (Feature Encoding & Scaling), Phase G (Class Imbalance Handling), Phase H (Dimensionality Reduction - Optional), Phase I (Train/Test Split), Phase J (Baseline Modeling), Phase K (Hyperparameter Tuning), Phase L (Final Evaluation), Phase M (Interpretability)

### Implementation for User Story 5

- [x] T042 [US5] Create Phase F header markdown cell with title "PHASE F — FEATURE ENCODING & SCALING"
- [x] T043 [US5] Create categorical encoding cell (one-hot for low-cardinality <20, frequency/target-independent for high-cardinality) in diabetes_ml_pipeline.ipynb
- [x] T044 [US5] Create numeric scaling cell (StandardScaler for numeric features - note: fit only on training data after split)
- [x] T045 [US5] Create Phase G header markdown cell with title "PHASE G — CLASS IMBALANCE HANDLING"
- [x] T046 [US5] Create imbalance quantification cell (quantify multiclass imbalance metrics)
- [x] T047 [US5] Create balancing strategy cell (apply class weights, SMOTE, or class removal with justification)
- [x] T048 [US5] Create Phase H header markdown cell with title "PHASE H — DIMENSIONALITY REDUCTION (OPTIONAL)"
- [x] T049 [US5] Create PCA analysis cell (apply PCA, identify components explaining ≥95% variance, generate 2-D visualization)
- [x] T050 [US5] Create dimensionality reduction decision cell (document whether to include in final pipeline)
- [x] T051 [US5] Create Phase I header markdown cell with title "PHASE I — TRAIN / TEST SPLIT"
- [x] T052 [US5] Create train/test split cell (80/20 split with stratification on target, set random seed)
- [x] T053 [US5] Create split validation cell (verify class distribution consistency across splits)
- [x] T054 [US5] Create Phase J header markdown cell with title "PHASE J — BASELINE MODELING"
- [x] T055 [US5] Create baseline model training cell (train Multinomial Logistic Regression, Random Forest, XGBoost)
- [x] T056 [US5] Create baseline evaluation cell (evaluate all models with accuracy, macro F1, weighted F1, confusion matrices) in diabetes_ml_pipeline.ipynb
- [x] T057 [US5] Create baseline comparison cell (compare results in single table, identify best model)
- [x] T058 [US5] Create Phase K header markdown cell with title "PHASE K — HYPERPARAMETER TUNING"
- [x] T059 [US5] Create hyperparameter tuning cell (RandomizedSearchCV with 5-fold stratified CV, optimize weighted F1)
- [x] T060 [US5] Create tuning results cell (report best hyperparameters and CV performance)
- [x] T061 [US5] Create Phase L header markdown cell with title "PHASE L — FINAL EVALUATION"
- [x] T062 [US5] Create final evaluation cell (evaluate tuned model on test set, generate classification report)
- [x] T063 [US5] Create confusion matrix cell (generate normalized confusion matrix, per-class precision/recall)
- [x] T064 [US5] Create optional top-k accuracy cell (compute top-k accuracy if applicable)
- [x] T065 [US5] Create Phase M header markdown cell with title "PHASE M — INTERPRETABILITY"
- [x] T066 [US5] Create feature importance cell (compute and visualize feature importance)
- [x] T067 [US5] Create SHAP analysis cell (generate SHAP summary plot and per-class SHAP contributions) in diabetes_ml_pipeline.ipynb
- [x] T068 [US5] Create clinical insights cell (translate findings into clinical/operational insights in markdown)

**Checkpoint**: At this point, User Story 5 should be fully functional - model trained, tuned, evaluated, and interpreted

---

## Phase 8: User Story 6 - Pipeline Artifact Creation (Priority: P1)

**Goal**: Create a reusable, deployable pipeline that encapsulates all preprocessing and modeling steps for production inference

**Independent Test**: Create pipeline object, save it, demonstrate inference on new data - delivers reusable, production-ready artifact

**Pipeline Phases Covered**: Phase N (Pipeline & Artifacts)

### Implementation for User Story 6

- [x] T069 [US6] Create Phase N header markdown cell with title "PHASE N — PIPELINE & ARTIFACTS"
- [x] T070 [US6] Create pipeline construction cell (build scikit-learn Pipeline with preprocessing, encoding, scaling, optional dimensionality reduction, final model) in diabetes_ml_pipeline.ipynb
- [x] T071 [US6] Create pipeline save cell (save pipeline using joblib to artifacts/diabetes_pipeline_model.joblib)
- [x] T072 [US6] Create inference example cell (load pipeline, demonstrate prediction on new patient data example)

**Checkpoint**: At this point, User Story 6 should be fully functional - pipeline artifact saved and inference working

---

## Phase 9: User Story 7 - Documentation and Reporting (Priority: P2)

**Goal**: Generate comprehensive documentation of the entire pipeline process, findings, and results for stakeholders and future reference

**Independent Test**: Generate markdown report covering dataset overview, methodology, results, limitations - delivers comprehensive project documentation

**Pipeline Phases Covered**: Phase O (Documentation)

### Implementation for User Story 7

- [x] T073 [US7] Create Phase O header markdown cell with title "PHASE O — DOCUMENTATION"
- [x] T074 [US7] Create documentation report cell (generate markdown report with dataset overview section)
- [x] T075 [US7] Create target engineering rationale cell (document target engineering rationale in report)
- [x] T076 [US7] Create EDA insights cell (document EDA insights in report)
- [x] T077 [US7] Create modeling approach cell (document modeling approach in report) in outputs/documentation_report.md
- [x] T078 [US7] Create evaluation results cell (document evaluation results in report)
- [x] T079 [US7] Create limitations cell (document limitations and future work in report)
- [x] T080 [US7] Create code quality review cell (ensure all code cells have docstrings, follow PEP 8, modular structure)

**Checkpoint**: At this point, User Story 7 should be fully functional - comprehensive documentation complete

### In-line Documentation Implementation for User Story 7

**Goal**: Add comprehensive in-line comments and docstrings to all code cells to make the codebase self-explanatory and easy to understand

**Independent Test**: Review code cells - all major operations, parameter choices, transformations, and non-obvious logic should have explanatory comments. Functions should have docstrings. Code should be readable without external documentation.

**Note**: These tasks are cross-cutting and apply to code cells across all phases (A through O, plus binary classification). They can be implemented incrementally as code is reviewed or added all at once during a documentation pass.

- [ ] T109 [US7] Add in-line comments to Phase A cells (data ingestion) explaining major operations, data loading steps, and column categorization logic in diabetes_ml_pipeline.ipynb
- [ ] T110 [US7] Add in-line comments to Phase B cells (cleaning/preprocessing) explaining identifier removal rationale, normalization steps, missing value handling strategies, and imputation choices in diabetes_ml_pipeline.ipynb
- [ ] T111 [US7] Add in-line comments to Phase C cells (target engineering) explaining ICD-9 mapping logic, category grouping rationale, rare category filtering thresholds, and binary target transformation in diabetes_ml_pipeline.ipynb
- [ ] T112 [US7] Add in-line comments to Phase D cells (EDA) explaining visualization purposes, statistical analysis rationale, and correlation interpretation in diabetes_ml_pipeline.ipynb
- [ ] T113 [US7] Add in-line comments to Phase E cells (outlier handling) explaining IQR calculation, outlier detection thresholds, and handling strategy rationale in diabetes_ml_pipeline.ipynb
- [ ] T114 [US7] Add in-line comments to Phase F-H cells (feature encoding, scaling, imbalance handling, dimensionality reduction) explaining encoding strategy choices, scaling rationale, class imbalance approach, and PCA decisions in diabetes_ml_pipeline.ipynb
- [ ] T115 [US7] Add in-line comments to Phase I-L cells (train/test split, baseline modeling, hyperparameter tuning, evaluation) explaining split strategy, model selection rationale, hyperparameter search space, evaluation metric choices, and threshold decisions in diabetes_ml_pipeline.ipynb
- [ ] T116 [US7] Add in-line comments to Phase M cells (interpretability) explaining feature importance calculation methods, SHAP analysis approach, and clinical insight derivation in diabetes_ml_pipeline.ipynb
- [ ] T117 [US7] Add in-line comments to Phase N cells (pipeline construction) explaining pipeline component selection, artifact saving process, and inference workflow in diabetes_ml_pipeline.ipynb
- [ ] T118 [US7] Add in-line comments to binary classification cells (T088-T106) explaining binary target validation, binary model training, binary evaluation metrics, ROC/PR curve interpretation, threshold analysis, and binary pipeline construction in diabetes_ml_pipeline.ipynb
- [ ] T119 [US7] Add docstrings or header comments to all reusable functions and complex code blocks explaining function purpose, parameters, return values, and usage examples in diabetes_ml_pipeline.ipynb
- [ ] T120 [US7] Document all parameter choices, thresholds, and configuration values with explanatory comments explaining why specific values were chosen (e.g., 80/20 split, 40% missing threshold, 5-fold CV, etc.) in diabetes_ml_pipeline.ipynb
- [ ] T120a [US7] Review and ensure all comments follow consistent style: explain "why" not just "what", avoid redundant comments for obvious code, maintain clear and concise explanations in diabetes_ml_pipeline.ipynb

**Checkpoint**: At this point, all code cells should have appropriate in-line documentation making the codebase self-explanatory

---

## Phase 10: User Story 8 - Binary Readmission Classification (Priority: P2)

**Goal**: Train, tune, and evaluate binary classification models to predict patient readmission within 30 days using the `readmitted_binary` target variable

**Independent Test**: Train binary baseline models, tune best performer, evaluate on test data with binary classification metrics (ROC-AUC, Precision-Recall, F1) - delivers validated binary classification model for readmission prediction

**Pipeline Phases Covered**: Binary classification modeling (parallel to multiclass pipeline, uses same preprocessing)

**Prerequisites**: 
- User Story 3 complete (binary target `readmitted_binary` engineered)
- User Story 2 complete (data cleaned and preprocessed)
- Can reuse same feature encoding/scaling from multiclass pipeline

### Implementation for User Story 8

- [x] T088 [US8] Create binary classification header markdown cell with title "BINARY CLASSIFICATION: READMISSION PREDICTION"
- [x] T089 [US8] Create binary target validation cell (verify `readmitted_binary` exists, display distribution, check class balance) in diabetes_ml_pipeline.ipynb
- [x] T090 [US8] Create binary train/test split cell (80/20 split with stratification on `readmitted_binary`, set random seed, verify class distribution consistency)
- [x] T091 [US8] Create binary baseline model training cell (train Logistic Regression, Random Forest, XGBoost with `objective='binary:logistic'`) in diabetes_ml_pipeline.ipynb
- [x] T092 [US8] Create binary baseline evaluation cell (evaluate all models with accuracy, F1 score, ROC-AUC, precision-recall AUC, confusion matrices)
- [x] T093 [US8] Create binary baseline comparison cell (compare results in single table, identify best model)
- [x] T094 [US8] Create binary hyperparameter tuning cell (RandomizedSearchCV with 5-fold stratified CV, optimize ROC-AUC or F1 score)
- [x] T095 [US8] Create binary tuning results cell (report best hyperparameters and CV performance)
- [x] T096 [US8] Create binary final evaluation cell (evaluate tuned model on test set, generate classification report)
- [x] T097 [US8] Create binary confusion matrix cell (generate confusion matrix with normalized and raw counts)
- [x] T098 [US8] Create ROC curve visualization cell (plot ROC curve with AUC score)
- [x] T099 [US8] Create precision-recall curve visualization cell (plot precision-recall curve with AUC score)
- [x] T100 [US8] Create threshold analysis cell (analyze precision-recall tradeoff, identify optimal threshold)
- [x] T101 [US8] Create binary feature importance cell (compute and visualize feature importance for binary model)
- [x] T102 [US8] Create binary SHAP analysis cell (generate SHAP summary plot and binary classification SHAP contributions) in diabetes_ml_pipeline.ipynb
- [x] T103 [US8] Create binary clinical insights cell (translate findings into clinical/operational insights for readmission prediction in markdown)
- [x] T104 [US8] Create binary pipeline construction cell (build scikit-learn Pipeline with preprocessing, encoding, scaling, final binary model)
- [x] T105 [US8] Create binary pipeline save cell (save binary pipeline using joblib to artifacts/diabetes_binary_readmission_pipeline.joblib)
- [x] T106 [US8] Create binary inference example cell (load binary pipeline, demonstrate readmission prediction on new patient data example)

**Checkpoint**: At this point, User Story 8 should be fully functional - binary classification model trained, tuned, evaluated, and saved as separate pipeline artifact

---

## Phase 11: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [x] T081 [P] Review and ensure all notebook cells have proper markdown documentation
- [x] T082 [P] Verify all random seeds are set for reproducibility
- [x] T083 [P] Ensure all file paths are consistent throughout notebook
- [x] T084 [P] Run quickstart.md validation checklist
- [x] T085 [P] Review code quality (PEP 8 compliance, docstrings, comments)
- [x] T086 [P] Test end-to-end notebook execution (run all cells sequentially)
- [x] T087 [P] Verify all outputs directory structure (EDA visualizations, model results, documentation)
- [ ] T107 [P] Update documentation to include binary classification results and comparison with multiclass model
- [ ] T108 [P] Verify both pipeline artifacts (multiclass and binary) can be loaded and used independently
- [ ] T109-T120a [US7] Add comprehensive in-line documentation to all code cells across all phases (see User Story 7 - In-line Documentation Implementation section above)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed sequentially (notebook cells must be executed in order)
  - Sequential execution required due to notebook nature (cells build on previous results)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P1)**: Depends on User Story 1 completion - Needs data loaded
- **User Story 3 (P1)**: Depends on User Story 2 completion - Needs cleaned data
- **User Story 4 (P2)**: Depends on User Story 2 completion - Needs cleaned data (can potentially run after US2)
- **User Story 5 (P1)**: Depends on User Stories 1-3 completion - Needs preprocessed data and target variable
- **User Story 6 (P1)**: Depends on User Story 5 completion - Needs trained model
- **User Story 7 (P2)**: Depends on all previous stories - Needs complete pipeline results
  - High-level documentation (Phase O): Depends on all pipeline phases
  - In-line documentation (T109-T120a): Can be added incrementally as code is written or in a dedicated documentation pass. Cross-cutting requirement that applies to all phases.
- **User Story 8 (P2)**: Depends on User Stories 2-3 completion - Needs cleaned data and binary target variable (`readmitted_binary`). Can run in parallel with or after multiclass pipeline (US5-US6) since it uses same preprocessing but different target

### Within Each User Story

- Markdown header cells before code cells
- Sequential cell execution (notebook nature requires this)
- Code cells build on previous cells
- Documentation cells after implementation cells

### Parallel Opportunities

**Note**: Due to notebook sequential execution model, parallel opportunities are limited:
- Setup tasks marked [P] can be prepared in parallel (different files)
- Foundational tasks marked [P] can be prepared in parallel (different files)
- Documentation review tasks in Polish phase can be done in parallel
- However, notebook cells must execute sequentially due to data dependencies

---

## Implementation Strategy

### MVP First (User Stories 1-3, 5-6)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Complete Phase 3: User Story 1 (Data Ingestion)
4. Complete Phase 4: User Story 2 (Data Cleaning)
5. Complete Phase 5: User Story 3 (Target Engineering)
6. Complete Phase 7: User Story 5 (Model Development)
7. Complete Phase 8: User Story 6 (Pipeline Artifact)
8. **STOP and VALIDATE**: Test end-to-end pipeline independently
9. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently (load and analyze data)
3. Add User Story 2 → Test independently (clean data)
4. Add User Story 3 → Test independently (engineer targets - multiclass and binary)
5. Add User Story 4 → Test independently (EDA insights)
6. Add User Story 5 → Test independently (train and evaluate multiclass model)
7. Add User Story 6 → Test independently (save multiclass pipeline, test inference)
8. Add User Story 7 → Complete documentation
9. Add User Story 8 → Test independently (train and evaluate binary model, save binary pipeline)

### Notebook Execution Strategy

With sequential notebook execution:
1. Execute cells in order (cannot skip due to dependencies)
2. Test each phase by executing cells up to that phase
3. Validate outputs at each checkpoint
4. Save notebook frequently during development
5. Use checkpoints (save intermediate states if needed)

---

## Notes

- [P] tasks = different files, can be prepared in parallel
- [Story] label maps task to specific user story for traceability
- Notebook cells must execute sequentially (notebook execution model)
- Each user story should be independently testable by executing up to that point
- Verify notebook executes end-to-end without errors
- Commit after each phase or logical group
- Stop at any checkpoint to validate story independently
- Avoid: skipping cells, executing cells out of order, not testing intermediate outputs
