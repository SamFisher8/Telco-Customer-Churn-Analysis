# Predictive Modeling – ANN Architecture Design (US-4.1)

## Overview
This folder contains the architecture design and documentation for the Artificial Neural Network (ANN) used in the Telco Customer Churn Analysis project.

The purpose of this task (US-4.1) is to define and justify the neural network structure prior to model training and hyperparameter tuning.

---

## Model Architecture Design

### Input Layer
- The input layer is configured to match the number of features after data preprocessing and feature scaling.
- All input features are numerical and standardized to ensure stable training.

### Hidden Layers
- The ANN consists of one or more hidden layers.
- Each hidden layer uses the **ReLU (Rectified Linear Unit)** activation function.
- ReLU is chosen for its efficiency, ability to handle non-linearity, and reduced risk of vanishing gradients.

### Output Layer
- The output layer contains a single neuron.
- A **Sigmoid** activation function is used to output a probability value between 0 and 1.
- This is appropriate for binary classification (customer churn vs non-churn).

---

## Design Rationale
- ANN models are well-suited for capturing complex, non-linear relationships in customer behavior data.
- The chosen architecture balances model expressiveness and interpretability.
- This design serves as the foundation for subsequent training and hyperparameter tuning (US-4.2).

---

## Files in This Folder
- `README.md` – ANN architecture explanation and design rationale
- *(Additional architecture documents such as PDF or Word files may be added here)*

---

## Related User Stories
- **US-4.1**: ANN Architecture Design & Configuration  
- **US-4.2**: Model Training & Hyperparameter Tuning
- ## Additional Documentation

- 📄 **ANN_Architecture_Design_US-4.1.docx** – Detailed architecture explanation and rationale (Word format)

