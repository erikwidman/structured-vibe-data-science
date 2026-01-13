<!--
Sync Impact Report:
- Version change: template → 1.0.0
- Principles added: 6 core principles (Data Quality, EDA First, Reproducibility, Feature Engineering, Model Validation, Documentation)
- Sections added: Data Science Workflow, Quality Standards
- Templates requiring updates: ⚠ pending verification
- Follow-up TODOs: None
-->

# Vibe Data Science Constitution

## Core Principles

### I. Data Quality & Validation
All data processing pipelines MUST validate input data before any transformation or analysis. Data quality checks include: schema validation, null/missing value assessment, type checking, range/domain validation, and duplicate detection. Invalid data MUST be logged with clear error messages and handled explicitly (fail fast, imputation, or exclusion with justification). Data validation is NON-NEGOTIABLE and must occur at pipeline entry points.

### II. Exploratory Data Analysis First
Before any modeling or advanced analysis, comprehensive EDA MUST be performed. This includes: distribution analysis, correlation analysis, missing data patterns, outlier detection, feature relationships, and target variable exploration. EDA findings MUST be documented with visualizations and summary statistics. Understanding data characteristics, quality issues, and relationships is REQUIRED before feature engineering or model selection.

### III. Reproducibility & Versioning
All analyses and pipelines MUST be reproducible. This requires: versioned data sources (with data hashing/checksums), deterministic random seeds for all stochastic operations, versioned dependencies (requirements.txt/environment.yml), and documented execution environments. Code MUST be organized in modular, reusable components. Random seeds MUST be set explicitly for any operation using randomness (sampling, train/test splits, model initialization).

### IV. Feature Engineering & Transformation
All feature engineering steps MUST be traceable and reversible. Transformations MUST be documented with: input/output schemas, transformation logic, handling of missing values, and scaling/normalization parameters. Feature engineering code MUST be tested independently. Transformation pipelines MUST preserve data lineage (original → intermediate → final features). Feature selection decisions MUST be justified and documented.

### V. Model Validation & Evaluation
All machine learning models MUST undergo rigorous validation using appropriate techniques: train/validation/test splits (or cross-validation), proper evaluation metrics for the problem type, baseline model comparison, and assessment of overfitting (training vs validation performance). Model performance MUST be evaluated on held-out test data only once. Hyperparameter tuning MUST use separate validation sets, never test sets. Model interpretability SHOULD be prioritized where possible (feature importance, SHAP values, partial dependence plots).

### VI. Documentation & Communication
All analyses, models, and results MUST be documented with: clear problem statements, methodology descriptions, assumptions and limitations, results interpretation, and actionable conclusions. Visualizations MUST be clear, labeled, and reproducible. Code MUST include docstrings explaining purpose, inputs, outputs, and key logic. Complex statistical or ML concepts MUST be explained in accessible language alongside technical details.

## Data Science Workflow

### Standard Analysis Pipeline
1. **Data Ingestion**: Load and validate input data
2. **Exploratory Data Analysis**: Understand data characteristics
3. **Data Cleaning**: Handle missing values, outliers, inconsistencies
4. **Feature Engineering**: Create and transform features
5. **Model Development**: Train and validate models
6. **Model Evaluation**: Assess performance on test data
7. **Documentation**: Document findings and methodology
8. **Deployment/Monitoring**: (If applicable) Deploy and monitor model performance

Each stage MUST produce artifacts (code, visualizations, summaries) that can be reviewed and reproduced.

## Quality Standards

### Code Quality
- Functions MUST be focused, testable, and well-documented
- Code MUST follow PEP 8 (Python) or equivalent style guides
- Complex logic MUST include explanatory comments
- Data processing functions MUST handle edge cases explicitly

### Testing Requirements
- Data validation functions MUST have unit tests
- Feature engineering transformations MUST be tested with known inputs/outputs
- Model training pipelines MUST be tested with synthetic or small datasets
- Integration tests MUST verify end-to-end pipeline execution

### Performance Considerations
- Data processing SHOULD be optimized for scale (vectorization, parallelization where appropriate)
- Large datasets SHOULD use memory-efficient processing (chunking, streaming)
- Model training SHOULD include time/complexity considerations
- Performance optimizations MUST not compromise correctness or reproducibility

## Governance

This constitution supersedes all other practices and guidelines. All data science work MUST align with these principles. Amendments to this constitution require:
- Documentation of the proposed change
- Justification for the amendment
- Review and approval process
- Update to version number following semantic versioning (MAJOR.MINOR.PATCH)
- Propagation of changes to dependent templates and documentation

All code reviews and analyses MUST verify compliance with these principles. Complexity that violates these principles MUST be explicitly justified with clear reasoning.

**Version**: 1.0.0 | **Ratified**: 2025-01-27 | **Last Amended**: 2025-01-27
