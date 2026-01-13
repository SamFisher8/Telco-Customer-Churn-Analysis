# Data Ingestion & Quality Validation

## Project Context

This repository contains the **data engineering deliverables** for a telecommunications customer churn analysis project. The objective of this phase is to ingest raw customer data, perform quality validation, and produce a clean, analysis-ready dataset for downstream analytics and machine learning.

This work was completed as part of an **internship program**, with responsibility held for data ingestion, validation, and preparation.

---

## Dataset Overview

* **Source file:** `Dataset_ATS_v2.csv`
* **Records:** 7,043 customer rows
* **Domain:** Telecommunications customer usage, billing, and service attributes
* **Purpose:** Support churn analysis and predictive modelling

---

## Directory Structure

```
Data_Preparation/
└── data/
    ├── raw/
    │   └── Dataset_ATS_v2.csv
    └── processed/
        └── Dataset_ATS_v2_cleaned.csv
```

### Folder Definitions

* **`raw/`**: Immutable source data exactly as received
* **`processed/`**: Cleaned and validated dataset ready for analysis

---

## Data Ingestion Process

The raw dataset was ingested using Python (Pandas) in a Jupyter Notebook environment (Google Colab). The ingestion process included schema inspection, record count verification, and basic sanity checks to ensure data integrity.

Verification steps included:

* Confirming successful load of all 7,043 records
* Inspecting column names and data types
* Validating file integrity after upload

---

## Data Quality Validation Steps

The following quality checks and transformations were applied:

### 1. Duplicate Detection

* Identified duplicate rows across all columns
* Removed duplicate records where detected

### 2. Missing Value Handling

* Checked all columns for null / NaN values
* Applied appropriate handling strategies:

  * Removed rows where missing values would compromise modelling
  * Preserved records where nulls were non-critical or expected

### 3. Data Type Validation

* Ensured numerical fields (e.g. `MonthlyCharges`, `TotalCharges`) were cast to numeric types
* Ensured categorical fields were stored as strings / objects
* Corrected mis-typed or inconsistent fields

### 4. Final Sanity Checks

* Verified row count post-cleaning
* Revalidated schema consistency
* Confirmed absence of breaking anomalies

---

## Output Artifact

* **Cleaned dataset:** `Dataset_ATS_v2_cleaned.csv`
* **Status:** Analysis-ready and safe for modelling
* **Intended consumers:** Data analysts, data scientists, and ML engineers

---

## Tools & Technologies

* Python 3
* Pandas
* NumPy
* Jupyter Notebook / Google Colab
* Git & GitHub for version control

---

## Handover Notes

* Raw data has been preserved without modification
* All cleaning logic was deterministic and reproducible
* No feature engineering or modelling was performed in this phase
* Downstream teams may proceed directly with EDA and model development

---

## Author

**Sameen Sadman**
Data Engineer (Internship Project)


### Categorical Feature Encoding (Stage 4)

#### Objective
Machine learning models such as **K-Means Clustering** and **Artificial Neural Networks (ANNs)** require numerical inputs. This stage converts all categorical (object-type) variables in the cleaned dataset into numeric representations while preserving semantic meaning and avoiding unintended ordinal relationships.

The output of this stage is a fully numeric dataset ready for modeling.

---

#### Encoding Strategy Overview

The dataset primarily contains **binary** and **ordinal** categorical variables. To maintain interpretability and model correctness, the following encoding strategies were applied:

| Feature Type | Encoding Method | Rationale |
|-------------|----------------|-----------|
| Target Variable | Manual Binary Mapping | Ensures correct positive class definition |
| Ordinal Features | Manual Mapping | Preserves logical ordering |
| Binary Features | Label Encoding | Compact and semantically safe |
| Nominal Features | Not applicable | Dataset does not contain high-cardinality nominal variables |

---

#### Manual Encoding (Critical Variables)

Manual mappings were applied to prevent incorrect alphabetical ordering from automatic encoders.

**Contract (Ordinal Feature)**  
Represents increasing contract commitment:

| Original Value | Encoded Value |
|---------------|--------------|
| Month-to-month | 0 |
| One year | 1 |
| Two year | 2 |

**Churn (Target Variable)**  
Defines churned customers as the positive class:

| Original Value | Encoded Value |
|---------------|--------------|
| No | 0 |
| Yes | 1 |

This encoding ensures compatibility with ANN loss functions and evaluation metrics.

---

#### Label Encoding (Binary Predictors)

Binary categorical variables were encoded using label encoding, resulting in values `{0, 1}`:

- `gender`
- `Partner`
- `Dependents`
- `PhoneService`
- `MultipleLines`
- `InternetService`

Label encoding is appropriate here because these variables have only two possible states and do not introduce misleading ordinal relationships.

---

#### Validation and Quality Assurance

Post-encoding checks were performed to ensure data integrity:

- All columns are numeric (`int` or `float`)
- No missing values (`NaN`) were introduced
- Ordinal hierarchy and target mapping were verified
- Encoded distributions were sanity-checked using value counts

---

#### Output Artifact

The final encoded dataset is stored as: Dataset_Encoded_v1.csv


## Class Imbalance Handling (US-2.5)

The dataset contains a significant class imbalance in the target variable Churn, where the majority of customers do not churn. If left unaddressed, machine learning models may become biased toward predicting the majority class.

### Observed Distribution (Before Balancing)

No Churn (0): 5,174 records  
Churn (1): 1,869 records  

### Applied Technique: SMOTE

Synthetic Minority Over-sampling Technique (SMOTE) was applied exclusively to the training dataset.

Key design decisions:
- SMOTE was applied after feature encoding
- SMOTE was applied after the train/test split
- The testing set was not modified to preserve real-world data distribution and prevent data leakage

### Training Set Distribution

Before SMOTE:
Churn = 0 -> 3,959  
Churn = 1 -> 1,433  

After SMOTE:
Churn = 0 -> 3,959  
Churn = 1 -> 3,959  

This resulted in a balanced 50/50 class distribution for model training.

### Rationale

Balancing the training data improves the model’s ability to correctly identify churned customers while maintaining a realistic evaluation environment using the original test data.

