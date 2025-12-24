Dual-Layer Spectral CT–Based Models for Differentiating Breast Lesions

This repository contains the complete analysis code used in the study
“Dual-Layer Spectral CT-Based Models for the Differential Diagnosis of Breast Lesions”,
submitted to the Journal of Breast Imaging.

The code implements a fully transparent, leakage-safe, and reproducible machine-learning pipeline for feature selection, model development, validation, and performance evaluation, strictly adhering to the methodological descriptions reported in the manuscript.

Overview of the Study

This retrospective study aimed to develop and validate diagnostic models for differentiating benign from malignant breast lesions incidentally detected on chest CT examinations using:

Clinico-radiological features

Dual-layer spectral CT (DSCT) quantitative parameters

A hybrid model integrating both feature domains

All modeling procedures were explicitly designed to minimize overfitting, prevent information leakage, and ensure reproducibility, in accordance with current best practices for medical prediction modeling.

Analysis Environment

Python: 3.9

All analyses—including feature selection, model training, validation, and nomogram coefficient estimation—were performed in Python.

Key Python Packages

numpy, pandas, scipy

scikit-learn

statsmodels

matplotlib, seaborn

Dataset and Privacy Statement

The dataset contains retrospective clinical and imaging-derived variables from patients who underwent clinically indicated chest dual-layer spectral CT examinations.

No raw imaging data are included

No patient identifiers are present

All analyses were performed on de-identified tabular data in accordance with institutional review board approval

Due to privacy regulations, the original dataset is not publicly shared.
However, the entire analysis pipeline is provided to ensure methodological transparency and reproducibility.

Data Partitioning Strategy

To prevent overfitting and information leakage, the cohort was split using stratified sampling:

Training set: ~70%

Validation set: ~14%

Independent testing set: ~16%

Key principles:

The testing set was strictly held out and used only for final performance reporting

Feature selection, collinearity assessment, and model fitting were conducted exclusively within the training set

The validation set was used for intermediate model comparison and tuning, but not for final inference

Code Structure

The analysis pipeline is organized into four sequential modules, all executed within a single, fully reproducible Python script.

MODULE 1 – Data Preparation and Univariate Analysis

Purpose

Data loading and column harmonization

Train / validation / test splitting

Univariate statistical analysis (training set only)

Generation of descriptive tables and univariable performance metrics

Statistical testing

Automatic selection of appropriate tests based on data properties:

Shapiro–Wilk normality test

Levene test for variance homogeneity

Independent-sample t-test / Welch test / Mann–Whitney U test

Chi-square test / Fisher’s exact test

Generated outputs

Table 2: Clinical and morphological characteristics

Table 3: DSCT parameters (arterial + venous phases), reported as:

Mean ± SD for normally distributed variables

Median (min, max) for non-normal variables

Corresponding F-values or Z-values and P-values

Supplementary Tables S2–S4: Univariable ROC analysis

Parameter-level ROC curves for clinical, arterial-phase, and venous-phase features

Critical outputs for downstream modeling

sig_clinical

sig_dsct

These variables represent statistically significant features identified only within the training cohort and are passed directly to MODULE 2.

MODULE 2 – Collinearity Assessment and Feature Selection

Purpose

To obtain robust, non-redundant predictors for model construction.

Workflow

Univariate filtering (from MODULE 1)

Pearson correlation filtering (|r| > 0.70)

Variance inflation factor (VIF) filtering (VIF > 5)

Applied to clinical and DSCT models only

LASSO regression with:

Five-fold cross-validation

One–standard-error criterion (λ₁se)

Independent feature selection performed for

Clinico-radiological model

DSCT-based model

Hybrid model

(Pearson correlation filtering only, no VIF, per study design)

Generated outputs

Pearson correlation heatmaps

VIF bar plots

LASSO cross-validation curves

LASSO coefficient paths

Supplementary Tables S5–S9

Final selected feature lists:

clin_final_vars

dsct_final_vars

hybrid_final_vars

MODULE 3 – Model Development and Validation

Models

Clinico-radiological model

DSCT-based model

Hybrid model

All models were constructed using multivariable logistic regression based on LASSO-selected features.

Performance evaluation

Models were evaluated across:

Training set (apparent performance)

Validation set

Independent testing set

Reported metrics

AUC with 95% confidence intervals (bootstrap resampling)

Sensitivity and specificity

Accuracy

Matthews correlation coefficient (MCC)

Optimal cutoff values determined by the Youden index

Robustness assessment

Nested cross-validation

Outer loop: 5 folds

Inner loop: 5 folds

Feature selection and model fitting repeated within each iteration

Generated publication-grade figures

ROC curves

Calibration curves

Decision curve analysis (DCA)

All figures are exported as high-resolution TIFF files (600 dpi) suitable for direct journal submission.

MODULE 4 – Final Hybrid Nomogram Construction

A final hybrid nomogram was constructed entirely in Python for interpretability and transparency.

Key points:

The nomogram was based strictly on predictors selected by the final hybrid LASSO model

Explicit dummy coding with fixed reference categories was applied

A final multivariable logistic regression model was refitted on the full dataset

The resulting logit-scale prediction formula and coefficients were exported as supplementary material

No additional feature selection or performance evaluation was performed at this stage.

Reproducibility Notes

All random processes use a fixed random seed (RANDOM_STATE = 2025)

All feature selection steps are properly nested within resampling frameworks

Strict separation of training, validation, and testing datasets is maintained throughout

Intended Use

This repository is provided for:

Methodological transparency

Peer review and editorial evaluation

Academic reproducibility and educational reference

The code is not intended for direct clinical deployment without independent external validation and regulatory approval.

Citation

If you find this code useful, please cite the associated manuscript when available.

Contact

For questions regarding the analysis pipeline or methodology, please contact the corresponding author.
