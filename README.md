# Predicting In-Hospital Mortality in Critically Ill Patients (SUPPORT2)

**Author:** Xinxin Zhou  
**Course:** DATA 1030 – Introduction to Machine Learning  
**Institution:** Data Science Institute - Brown University  

## Overview

This project aims to predict in-hospital mortality among critically ill patients using the SUPPORT2 clinical dataset. Mortality prediction plays a crucial role in critical care, helping clinicians assess patient risk, allocate resources, and guide discussions around treatment goals. However, real-world medical data are often incomplete, with high rates of missing values and heterogeneous feature types. This project demonstrates how interpretable machine learning can be applied to healthcare data while respecting data limitations, providing both predictive insights and interpretability that could support future clinical research.

## Dataset

- **Source:**  
  The dataset comes from the SUPPORT2 (Study to Understand Prognoses and Preferences for Outcomes and Risks of Treatments) clinical study, which contains detailed physiological, demographic, and health status data for critically ill hospitalized patients.

- **Features:**  
  - **19 continuous variables** capturing vital signs, lab results, and severity scores (e.g., `meanbp`, `aps`, `scoma`, `pafi`, `alb`, `bili`, `crea`).  
  - **7 categorical variables** including diagnosis group (`dzgroup`, `dzclass`), demographic and comorbidity indicators (`sex`, `race`, `diabetes`, `dementia`, `dnr`).  
  - **4 ordinal variables** such as income level and functional status scores (`income`, `ca`, `adls`, `adlp`).

- **Target Variable:**  
  `hospdead` — a binary variable indicating whether the patient died during hospitalization (1) or survived (0).

## Methodology

- **Preprocessing:**  
  - Removed post-outcome variables (e.g., total charges, length of stay, modelled survival) to avoid information leakage.  

- **Model Training**
  - **Models Implemented:**  
  - Logistic Regression (LR) 
  - Linear Support Vector Machine (SVM)
  - Random Forest (RF)
  - XGBoost (XGB)

- **Missing Data Handling:**  
  - For LR, SVM, and RF, implemented a pattern-reduced approach,  
    where separate submodels are trained for each unique missingness pattern  
    among continuous variables.  
  - XGBoost natively handles missing values through its internal tree-splitting mechanism.

- **Cross-Validation Strategy:**  
  - Conducted repeated random-state validation (5 runs) to assess model robustness.  
  - For LR, SVM, and RF, used manual grid search on the validation split.  
  - For XGBoost, applied a 5-fold KFold GridSearchCV with `roc_auc` scoring.

- **Hyperparameter Tuning:**  
  | Model | CV Type | Key Parameters |
  |:------|:---------|:---------------|
  | Logistic Regression | Manual grid-search | `C ∈ {0.01, 0.1, 1, 10, 100}` |
  | Linear SVM | Manual grid-search | `C ∈ {0.01, 0.1, 1, 10, 100}` |
  | Random Forest | Manual grid-search | `max_depth ∈ {1, 3, 10, 30, 100}`, `max_features ∈ {0.5, 0.75, 1.0}` |
  | XGBoost | 5-fold KFold + early stopping | `max_depth ∈ {1, 3, 10, 30, 100}`, `reg_alpha ∈ {0.1, 1, 10}`, `reg_lambda ∈ {0.1, 1, 10}`, `n_estimators = 10000 with early stopping` |

- **Evaluation Metrics:**  
  - Primary: AUC (Area Under ROC Curve)
  - Secondary: Accuracy, F1 Score
  - Baseline: Majority-class classifier predicting the most frequent outcome.
 
## Results

- **XGBoost** achieved the best overall performance with the highest AUC and F1 score, demonstrating both strong discrimination ability and stable results across folds.  
- **Logistic Regression** and **Linear SVM** produced nearly identical AUCs, confirming that linear models can perform competitively on well-preprocessed clinical data.  
- **Random Forest** also performed well, though slightly lower, possibly due to sensitivity to missingness patterns and reduced feature sets.

## References

The SUPPORT Principal Investigators. (1995). *A controlled trial to improve care for seriously ill hospitalized patients:  
The Study to Understand Prognoses and Preferences for Outcomes and Risks of Treatments (SUPPORT).*  
*Journal of the American Medical Association, 274*(20), 1591–1598.  
https://doi.org/10.1001/jama.1995.03530200027032

## Disclaimer

This repository is part of the DATA 1030 Final Project at Brown University.

All analyses are for academic learning purposes only and are not intended for real-world clinical use.
