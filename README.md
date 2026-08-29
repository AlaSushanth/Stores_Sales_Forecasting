🛒 Store Sales Forecasting

📌 Project Overview

Retail businesses need to make important decisions before actual sales occur. Poor demand estimation can lead to:

Overstocking and unnecessary inventory costs
Stockouts and lost sales opportunities
Inefficient allocation of products across stores
Poor staffing and operational planning
Ineffective promotional planning

This project develops a machine learning-based store sales forecasting system to predict near-term sales using historical sales patterns and other relevant features.

The main objective is not just to obtain a high prediction score, but to build a forecasting system that can help retail businesses estimate upcoming demand and make better data-driven decisions.

🎯 Business Objective

The objective of this project is to answer the question:

Can historical store-level sales data be used to forecast future sales and support better inventory, operational, and promotional decisions?

By forecasting future sales, a retail company can potentially use the predictions for:

Inventory planning
Demand estimation
Supply-chain planning
Staffing decisions
Promotional planning
📊 Dataset

The dataset contains store-level sales information with multiple store and product-family combinations over time.

The target variable is:

sales

The dataset also contains additional information related to stores, products, promotions, calendar events, and holidays.

Each sales time series is identified using:

store_nbr + family

Therefore, historical sales patterns were analysed separately for each store-product family combination.

🔄 Project Workflow
Raw Data
    ↓
Data Preprocessing
    ↓
Temporal Train-Test Split
    ↓
Feature Engineering
    ↓
Time Series Cross-Validation
    ↓
Hyperparameter Tuning
    ↓
Train Best Model on 8 Lakh Observations
    ↓
15-Day Recursive Forecasting
    ↓
Model Evaluation
    ↓
High-Demand Error Analysis
    ↓
Feature Importance Analysis
    ↓
Business Insights
🧹 Data Preprocessing

The preprocessing pipeline handles the different feature types in the dataset.

The pipeline includes:

Missing value handling
Numerical feature preprocessing
Categorical feature encoding
Feature scaling where required

A machine learning pipeline was used to ensure that preprocessing and model training were performed consistently.

⏳ Temporal Train-Test Split

Since this is a time-series forecasting problem, the data was not randomly split.

Instead, a chronological cutoff date was used.

Historical Data
       ↓
Training Dataset
       ↓
Cutoff Date
       ↓
Testing Dataset

The model was trained only on historical information and evaluated on future observations.

This approach prevents the model from using future sales information during training.

📈 Time Series Cross-Validation

TimeSeriesSplit was used for cross-validation.

Unlike normal cross-validation, it preserves the chronological order of the observations.

Conceptually:

Fold 1:

Train → Past Data
Test  → Future Data


Fold 2:

Train → More Past Data
Test  → Future Data


Fold 3:

Train → Even More Historical Data
Test  → Future Data

This provides a more realistic evaluation for a forecasting problem.

⚙️ Model Selection and Hyperparameter Tuning

Multiple model configurations were evaluated using GridSearchCV with time-series cross-validation.

The best-performing model was selected based on the cross-validation results.

The final model was then trained using approximately:

800,000 training observations
🧠 Feature Engineering

To capture the temporal behaviour of sales, lag features and Exponentially Weighted Moving Average (EWM) features were created.

Before creating these features, the data was ordered using:

store_nbr → family → date

This ensures that each store-product family has its own chronological sales history.

Lag Features

The following lag features were created:

lag_1

Sales from the previous day.

Yesterday's Sales
        ↓
Predict Today's Sales
lag_7

Sales from seven days earlier.

Sales 7 Days Ago
        ↓
Predict Future Sales
lag_14

Sales from fourteen days earlier.

These features allow the model to learn from previous sales behaviour.

📉 Exponentially Weighted Moving Average Features

The following EWM features were created:

ewm_7
ewm_14

EWM provides a smoothed representation of historical sales, while giving more importance to recent observations.

ewm_7

Represents the recent smoothed sales level.

It gives relatively more importance to recent sales and can capture short-term demand behaviour.

Conceptually:

Past Sales
    ↓
More importance to recent observations
    ↓
EWM_7
    ↓
Recent Smoothed Demand Level
ewm_14

Represents a smoother sales level over a relatively longer recent history.

EWM_7
   ↓
More responsive to recent changes

EWM_14
   ↓
More smooth and less sensitive to short-term fluctuations

The model can use both short-term and relatively longer-term sales behaviour for forecasting.

🔮 15-Day Recursive Forecasting

The final model was evaluated using a recursive multi-step forecasting approach.

The test period contained approximately:

26,730 observations

covering a 15-day forecasting horizon.

The forecasting process works as follows:

Predict Day 1
     ↓
Add Prediction to Historical Sales
     ↓
Predict Day 2
     ↓
Use Previous Prediction as Historical Information
     ↓
Predict Day 3
     ↓
...
     ↓
Predict Day 15

This simulates a realistic forecasting scenario because actual future sales are not assumed to be available while generating predictions.

📏 Model Evaluation

The final model was evaluated using:

Root Mean Squared Error (RMSE)

RMSE measures the average magnitude of prediction error, giving greater importance to large errors.

R² Score

R² measures how much variation in sales is explained by the model.

🏆 Final Results

The model was evaluated on approximately:

26,730 future observations
Overall Performance
Metric	Result
RMSE	279.65
R² Score	0.94

An R² score of approximately 0.94 indicates that the model captures a substantial proportion of the variation in sales for the test period.

🔥 High-Demand Sales Analysis

A separate analysis was performed to evaluate the model's performance during high-demand periods.

High-demand observations were defined as observations above the:

90th percentile of sales
Results
Metric	Result
High-Demand Threshold	1195
High-Demand Observations	2674
High-Demand RMSE	842.86

The high-demand RMSE was substantially higher than the overall RMSE.

This indicates that:

Extreme or high-demand sales periods remain more difficult for the model to predict accurately than normal sales periods.

This is an important limitation because high-demand observations represent a smaller portion of the dataset and can contain sudden demand changes.

🔍 Feature Importance Analysis

Feature importance analysis was performed using the trained model.

One of the most important features was:

EWM_7

with feature importance of approximately:

0.71

This indicates that the recent smoothed sales level contained substantial predictive information for forecasting future sales.

The finding suggests that:

Recent demand behaviour is highly informative for predicting near-term store sales.

The model therefore relies strongly on recent historical demand patterns while also considering other available features.

💼 Business Insights

The forecasting system can support retail businesses in estimating upcoming demand.

The predicted sales can potentially be used for:

📦 Inventory Planning

Estimate expected demand and plan inventory accordingly.

🚚 Supply-Chain Planning

Prepare product availability based on anticipated sales.

👥 Staffing

Allocate employees based on expected store demand.

📢 Promotional Planning

Identify periods of lower expected demand where promotional activities may be useful.

📊 Demand Monitoring

Compare recent sales patterns with predicted future demand.

⚠️ Key Limitation

The model performs well overall but has relatively higher prediction error during high-demand periods.

Possible reasons include:

Sudden demand spikes
Unusual events
High-demand observations being less frequent
Error accumulation during recursive forecasting

Future improvements could include:

Direct multi-step forecasting
Additional promotion and event-related features
Advanced gradient boosting models
Ensemble forecasting approaches
Special modelling strategies for extreme demand periods
🛠️ Technologies Used
Python
Pandas
NumPy
Scikit-learn
XGBoost
Matplotlib
🚀 Key Takeaway

This project demonstrates a complete machine learning workflow for store sales forecasting.

The model combines:

Historical Sales Patterns
        +
Lag Features
        +
Exponentially Weighted Moving Averages
        +
Other Business Features
        ↓
Future Sales Forecast
        ↓
Business Decision Support

The final model achieved an R² score of approximately 0.94 on a future 15-day test period, while feature importance analysis showed that recent smoothed sales behaviour was highly informative for near-term forecasting.
