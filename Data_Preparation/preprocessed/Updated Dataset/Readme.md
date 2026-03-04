# Repository Update — Corrected Scaled Datasets

## Overview

Following mentor feedback regarding potential data leakage in the original scaling implementation, the preprocessing pipeline has been corrected.

Feature scaling is now performed **after the train–test split** and the scaler is fitted **exclusively on the training data**.

To preserve traceability and avoid overwriting previous outputs, a new folder has been created within the processed data directory.

---

## New Folder Structure

The following directory has been added:

Data_Preparation/
└── data/
└── processed/
└── Updated Dataset/


This folder contains the corrected, leakage-free scaled datasets that must be used for all downstream modelling tasks.

---

## Updated Files

The following files are now the authoritative datasets for modelling:

- `Dataset_Scaled_v1.csv`  
  *(Full scaled dataset – reference only)*

- `X_train_scaled_v2.csv`  
  *(Training feature matrix scaled using training-only fit)*

- `X_test_scaled_v2.csv`  
  *(Testing feature matrix transformed using training scaler parameters)*

These files replace the earlier scaled datasets that were created before correcting the pipeline.

---

## Key Methodological Correction

### Previous Approach (Revised)

1. StandardScaler applied to the entire dataset  
2. Train–test split performed afterward  

**Issue:**  
This introduced potential data leakage because scaling parameters were influenced by test data.

---

### Current Correct Approach

1. Perform train–test split (80/20, stratified)  
2. Fit `StandardScaler` on training data only  
3. Apply learned parameters to:
   - Training set (fit_transform)
   - Test set (transform only)  
4. Save corrected scaled outputs  

This ensures:

- No information from the test set influences scaling  
- Fair and unbiased model evaluation  
- Compliance with machine learning best practices  

---

## Downstream Usage Requirement

All future modelling stages (e.g., neural network training, clustering, evaluation) must use:

Data_Preparation/data/processed/Updated Dataset/


Specifically:

- `X_train_scaled_v2.csv`
- `X_test_scaled_v2.csv`

Older scaled files should not be used for further experimentation.

---

## Project Integrity Improvement

This update improves:

- Evaluation integrity  
- Reproducibility  
- Transparency in methodology  
- Alignment with academic feedback  

---

## Summary

The data preparation pipeline has been strengthened to eliminate data leakage and ensure proper scaling protocol.  
All downstream tasks must reference the corrected datasets in the **Updated Dataset** folder.
