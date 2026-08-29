# Store Sales Forecasting 

## Project Overview

Forecasting retail sales is one of the most important business problems in the retail industry. Accurate sales predictions help companies optimize inventory, reduce stock shortages, improve supply chain planning, and make better promotional decisions.

In this project, I developed an end-to-end machine learning pipeline to forecast daily sales for multiple stores and product families using historical sales data. Rather than relying only on historical sales, I integrated multiple business datasets including store information, customer transactions, holiday events, and oil prices to improve prediction accuracy.

The project focuses not only on model building but also on solving real-world data engineering challenges such as merging multiple datasets, handling duplicate holiday records, feature engineering, missing value treatment, and model interpretation using SHAP.

---

# Business Problem

Retail companies generate enormous amounts of transactional data every day. However, sales are influenced by many external factors such as holidays, promotions, store location, customer traffic, and even oil prices.

The objective of this project is to build a machine learning model capable of accurately predicting daily sales for each store-product combination while identifying the most influential factors affecting sales.

---

# Dataset Description

The project uses the **Store Sales – Time Series Forecasting** dataset available on Kaggle.

Dataset Link

https://www.kaggle.com/competitions/store-sales-time-series-forecasting

The project combines information from six different datasets.

| Dataset | Purpose |
|----------|---------|
| train.csv | Historical sales data used for training |
| test.csv | Future data used for prediction |
| stores.csv | Store metadata such as city, state, cluster and store type |
| holidays_events.csv | National, regional and local holiday information |
| oil.csv | Daily oil prices |
| transactions.csv | Daily customer transaction count for each store |

**Note**

The original `train.csv` file is larger than GitHub's upload limit (100 MB). Therefore, it has not been included in this repository. The complete dataset can be downloaded from the Kaggle competition page.

---

# Project Workflow

The overall workflow followed during the project is shown below.

```

Raw Datasets
│
▼
Data Cleaning
│
▼
Merge Multiple Datasets
│
▼
Handle Missing Values
│
▼
Feature Engineering
│
▼
Preprocessing Pipeline
│
▼
XGBoost Regression
│
▼
Model Evaluation
│
▼
Model Explainability
│
▼
Business Insights

```

---

# Data Integration

One of the most challenging parts of this project was integrating information spread across multiple datasets.

The original sales dataset did not contain important business information such as customer transactions, store metadata, holiday events, or oil prices. Therefore, these datasets were merged using appropriate keys such as **date**, **store number**, **city**, and **state**.

During the merging process, the holiday dataset introduced duplicate rows because multiple holiday events could occur on the same date. Instead of allowing duplicate rows to increase the size of the training dataset, holiday records were aggregated before merging. This ensured that every sales record remained unique while preserving all holiday information.

---

# Data Preprocessing

A significant amount of preprocessing was performed before model training.

### Missing Value Handling

Missing values in numerical and categorical features were handled differently based on their business meaning.

- Missing oil prices were imputed.
- Missing holiday information was replaced with "No Holiday".
- Missing transfer information was handled separately.

This approach preserved the semantic meaning of the data instead of blindly applying statistical imputation.

---

### Feature Engineering

Several new features were created to improve model performance.

Holiday information was separated into

- National Holiday
- National Holiday Type
- National Transfer Status
- Regional Holiday
- Regional Holiday Type
- Regional Transfer Status
- Local Holiday
- Local Holiday Type
- Local Transfer Status

Additional business features included

- Customer Transactions
- Oil Price
- Promotion Count
- Store Metadata

---

### Categorical Encoding

Categorical variables such as

- Product Family
- Store Type
- City
- State
- Holiday Type

were encoded using **One-Hot Encoding**.

---

### Numerical Pipeline

Numerical features were processed using a preprocessing pipeline consisting of

- Missing Value Imputation
- Outlier Capping
- Power Transformation
- Standard Scaling

The entire preprocessing workflow was implemented using Scikit-learn's **Pipeline** and **ColumnTransformer**, ensuring consistent preprocessing during both training and testing.

---

# Model Development

After preprocessing, multiple regression models were considered. The final model selected for this project was **XGBoost Regressor** because of its ability to model complex nonlinear relationships while handling large structured datasets efficiently.

Hyperparameter tuning was performed using **RandomizedSearchCV** to obtain the optimal model configuration.

Cross-validation was used during training to ensure that the model generalized well and did not overfit the training data.

---

# Model Performance

The final XGBoost model achieved the following performance.

| Metric | Score |
|---------|-------|
| Cross Validation R² | **0.79** |
| Test R² | **0.82** |
| RMSE | **472** |

The close agreement between the cross-validation and test scores indicates that the model generalizes well to unseen data.

---

# Feature Importance

Feature importance analysis showed that customer transactions, product family, promotions, and oil prices were among the most influential variables affecting sales predictions.

---

# Model Explainability using SHAP

To improve model interpretability, SHAP (SHapley Additive exPlanations) was used.

The SHAP Summary Plot provides a global explanation of how each feature contributes to the model predictions.

Important observations include

- Customer transactions have the strongest positive impact on sales.
- Product family significantly influences demand.
- Oil prices moderately affect sales.
- Store-specific characteristics contribute to prediction accuracy.
- Holiday-related features have relatively smaller influence compared to customer traffic.

![SHAP Summary]

---

# Actual vs Predicted Analysis

The Actual vs Predicted plot demonstrates how closely the predicted sales follow the actual sales values.

Most observations lie close to the ideal prediction line, indicating strong predictive performance. A small number of observations with extremely high sales are underestimated, suggesting that rare peak-demand events remain challenging for the model.

![Actual vs Predicted]

---

# Residual Analysis

Residual analysis was performed to evaluate model errors.

The residual plot shows that the majority of residuals are centered around zero, indicating unbiased predictions for most observations. However, a few large positive residuals correspond to rare high-sales events where the model underestimates demand.

![Residual Plot]

---

# Business Insights

The analysis produced several useful business insights.

- Customer transactions are the strongest indicator of future sales.
- Product category plays a major role in sales forecasting.
- Promotional campaigns positively influence sales.
- Oil prices have a measurable impact on purchasing behaviour.
- Holiday effects vary depending on holiday type and location.
- The model performs well under normal business conditions but struggles with extremely high-demand events.

---

# Technologies Used

Programming Language

- Python

Libraries

- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- SHAP

Machine Learning Concepts

- Feature Engineering
- Pipeline
- ColumnTransformer
- One-Hot Encoding
- Cross Validation
- Hyperparameter Tuning
- Model Explainability

---

# Repository Structure

```

Store-Sales-Forecasting
│
├── data/
├── images/
├── notebooks/
├── requirements.txt
└── README.md

```

---

# Future Improvements

Several improvements can further enhance the forecasting performance.

- Incorporate lag-based time-series features.
- Introduce rolling-window statistics.
- Compare XGBoost with CatBoost and LightGBM.
- Improve prediction of rare peak-sales events.
- Deploy the model as a forecasting API or dashboard.

---

# Conclusion

This project demonstrates an end-to-end machine learning workflow, beginning with data integration and preprocessing, followed by feature engineering, model development, evaluation, and explainability. Beyond building a predictive model, the project emphasizes solving practical data challenges and extracting business insights from retail sales data, making it representative of a real-world data science workflow.
