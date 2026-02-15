## Scaler Justification — Outlier and Distribution Analysis

Project: Telco Customer Churn Analysis
Stage: Data Preparation
Author: Sameen Sadman

# 1. Objective

The purpose of this analysis is to determine whether StandardScaler is an appropriate scaling method or if significant outliers require the use of RobustScaler.

To prevent data leakage, all analysis was conducted only on the training dataset after the train–test split.

# 2. Features Analysed

The following numerical features were evaluated:

- MonthlyCharges – Monthly billing amount

- TotalCharges – Cumulative customer charges

- tenure – Length of customer relationship in months

These features are critical for modelling customer behaviour and churn risk.

# 3. Visual Distribution Analysis

**3.1 Boxplot Analysis**

Boxplots were created to detect outliers beyond the standard 1.5 × IQR range.

**Observations:**

- Some outliers appear in TotalCharges, which is expected because long-term customers accumulate higher spending.

- The outliers are not extreme or isolated far from the distribution.

- No feature shows abnormal dispersion that would significantly distort the mean.

- These patterns represent natural customer behaviour rather than data errors.

# 3.2 Histogram Analysis

Histograms were used to examine distribution shape.

**Findings:**

- MonthlyCharges is approximately symmetric

- tenure shows mild right skew (more newer customers)

- TotalCharges shows a gradual right tail due to cumulative billing

- The distributions are continuous and smooth, with no abnormal spikes.

# 4. Statistical Evidence

Skewness and kurtosis were calculated using training data only.

- Feature	Skewness	Kurtosis	Interpretation
- MonthlyCharges	Near 0	Low	Approximately symmetric
- TotalCharges	Moderate positive	Moderate	Expected long-tail behaviour
- tenure	Mild positive	Low	Acceptable variation

# Interpretation:

- Skewness values fall within acceptable limits

- Kurtosis values do not indicate heavy-tailed extreme outliers

- No feature shows statistical evidence requiring RobustScaler

# 5. Scaling Decision

StandardScaler is retained

**Justification:**

- Outliers are business-driven, not data anomalies

- Mean and standard deviation remain reliable

- StandardScaler benefits:

**Neural networks (gradient-based optimisation)**

- Distance-based models

- Faster and more stable model convergence

- RobustScaler is unnecessary because extreme outliers are not present.

# 6. Impact on Modelling

Using StandardScaler ensures:

- Features have mean ≈ 0

- Features have standard deviation ≈ 1

- Neural networks train with better gradient stability

- Distance-based algorithms treat features fairly

- This improves learning stability and model performance in later stages.

# 7. Verification

Validation performed on the training set only confirmed:

- Means close to 0

- Standard deviations close to 1

- No data leakage (scaler fit only on training data)

# 8. Conclusion

The dataset contains manageable, behaviour-based outliers, not extreme statistical anomalies. Based on both visual and statistical evidence, StandardScaler remains the most appropriate scaling method for this project.

# Figures

(Available in the EDA folder under Data_Preparation)

- Boxplot of tenure (Training Data)

- Histograms for all three features
