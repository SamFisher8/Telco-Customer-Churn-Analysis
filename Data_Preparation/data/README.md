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
#### Xinyun Dai — US-2.3 (Encoding Rules: Target / Ordinal / Binary)

This subsection records *how* I decided the encoding rules , so the team can follow the same standard during implementation and modelling.

---

##### How I approached the task (step-by-step)

1) **Started from the dataset link in Confluence**  
I opened the dataset from our Confluence project space (Data Preparation section) and focused on the columns related to feature encoding for US-2.3.

2) **Checked each relevant column and confirmed the number of categories**  
For each candidate column, I inspected its unique values to determine whether it is:
- a **target label** (to be predicted),
- an **ordinal feature** (natural ordering), or
- a **binary predictor** (two categories only).

Example checks:
- `Churn` → target label with `Yes/No`
- `Contract` → ordinal with `Month-to-month / One year / Two year`
- `gender`, `Partner`, `Dependents`, `PhoneService` → binary (two categories)

3) **Selected the encoding method based on the category type**  
- **Target label** → define a clear 0/1 mapping, with positive class = 1  
- **Ordinal feature** → use 0/1/2 mapping to preserve natural order  
- **Binary feature** → use standard 0/1 label encoding (no need for one-hot)

---

##### Final encoding standards (agreed rules)

**1) Target variable — `Churn`**
- Original values: `Yes`, `No`
- Mapping: **Yes = 1, No = 0**
- Note: Positive class is kept as **1** to ensure consistent evaluation (precision/recall) across the team.

**2) Ordinal feature (critical) — `Contract`**
- Original values: `Month-to-month`, `One year`, `Two year`
- Mapping:
  - **Month-to-month = 0**
  - **One year = 1**
  - **Two year = 2**
- Why: Contract length reflects increasing commitment, so ordinal encoding preserves the natural order.

> Implementation reminder: `Contract` must be encoded using an explicit mapping dictionary (`map()`), not `LabelEncoder`, to avoid accidental alphabetical ordering.

**3) Binary predictors — `gender`, `Partner`, `Dependents`, `PhoneService`**
These columns were confirmed as binary (two categories only), so **0/1 label encoding** is sufficient and one-hot encoding is unnecessary.

Fixed mapping convention:
- `Partner`: **No = 0, Yes = 1**
- `Dependents`: **No = 0, Yes = 1**
- `PhoneService`: **No = 0, Yes = 1**
- `gender`: **Male = 0, Female = 1**

---

##### Handover notes (implementation & validation)

After encoding is applied in code, please verify:
- Encoded columns are numeric (int/float)
- No unexpected NaN introduced by mapping (commonly caused by spelling mismatch)
- Value ranges are correct:
  - `Contract` ∈ {0, 1, 2}
  - `Churn` ∈ {0, 1}
  - Binary predictors ∈ {0, 1}

---

##### References
- Jira subtasks: US-2.3.1 / US-2.3.2 / US-2.3.3  
- Confluence: Data Preparation – Variable Mapping (Ordinal/Target/Binary)

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

### Feature Scaling & Normalization (US-2.4)

To prevent numerical features with larger magnitudes from disproportionately influencing the neural network, feature scaling was applied as part of the data preparation pipeline.

The following numerical features were selected for normalization:
- MonthlyCharges
- TotalCharges
- tenure

TotalCharges was not explicitly available in the source dataset and was therefore derived using MonthlyCharges and tenure, which is consistent with standard telecom billing calculations.

StandardScaler from scikit-learn was used to transform these features to a common scale with zero mean and unit variance. This approach was chosen to preserve relative differences while ensuring stable and unbiased model convergence.

Post-scaling verification was performed using descriptive statistics, confirming:
- Mean values approximately equal to 0
- Standard deviation approximately equal to 1
- No distortion of feature distributions

This step ensures numerical consistency across inputs prior to model training.





#### Output Artifact

The final scaled dataset is stored as: Dataset_Scaled_v1.csv


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


## US-2.6: Train-Test Split Generation

### Objective
To prepare the dataset for supervised machine learning by dividing it into separate training and testing subsets. This ensures models are trained on one portion of the data and evaluated on unseen data.

### Rationale
Separating training and testing data prevents data leakage and allows realistic model evaluation. A fixed random state ensures reproducibility, and stratification preserves the original churn distribution.

### Methodology
Input dataset:
Dataset_Scaled_v1.csv

Target variable:
Churn

Split configuration:
- Training set: 80%
- Testing set: 20%
- Stratified by Churn
- random_state = 42

Tool used:
sklearn.model_selection.train_test_split

### Implementation
```python
from sklearn.model_selection import train_test_split

X = df.drop(columns=['Churn'])
y = df['Churn']

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)


### Outputs ###

The following files were generated and saved to:
Data_Preparation/data/processed/

X_train.csv

y_train.csv

X_test.csv

y_test.csv

### Validation ### 

Verified 80/20 split ratio

Confirmed no data leakage

Preserved class distribution via stratification

### Status ###

Completed

