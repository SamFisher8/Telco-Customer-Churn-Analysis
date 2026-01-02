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

