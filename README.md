Telco Customer Churn Prediction

Customer churn prediction on the Telco Customer Churn dataset from Kaggle. The goal is to predict which customers are likely to cancel their subscription, based on their account and service information.

Dataset

7,043 customers, 21 features (demographics, account info, subscribed services) and a binary target Churn (Yes/No). The target is imbalanced: ~73% No / ~27% Yes.

Project Steps


Data Cleaning — fixed TotalCharges (stored as text with 11 blank values for new customers with tenure = 0), converted to numeric.
Exploratory Data Analysis (EDA) — analyzed churn distribution against contract type, tenure, monthly charges, and tech support. Key findings:

Month-to-month contracts churn far more (42.7%) than two-year contracts (2.8%)
Customers without Tech Support churn more (41.6% vs 15.2%)
Churned customers have lower average tenure (~18 months vs ~38 months)



Feature Encoding — binary mapping for Yes/No columns, One-Hot Encoding for multi-category columns.
Modeling — trained and compared three models:

Logistic Regression (with feature scaling, class_weight='balanced')
Random Forest (class_weight='balanced', 200 trees)
Gradient Boosting (default parameters as baseline)



Evaluation — compared models using ROC-AUC, Precision, Recall, F1, and validated with 5-fold cross-validation.
Model Interpretation — compared Random Forest feature importances with Logistic Regression coefficients to identify the most reliable churn drivers, and flagged a multicollinearity effect between tenure, MonthlyCharges, and TotalCharges.
Model Improvement — tuned Gradient Boosting with GridSearchCV, tested decision threshold adjustment to improve recall, and checked for overfitting by comparing train vs. test ROC-AUC.


Results

ModelROC-AUCRecall (Churn=1)Precision (Churn=1)Logistic Regression0.8410.7860.507Random Forest0.8290.4870.630Gradient Boosting0.8420.5110.656

Gradient Boosting achieved the highest ROC-AUC, but Logistic Regression has the best Recall for the churn class. In this business context, catching actual churners (Recall) matters more than avoiding false alarms, since missing a churning customer means losing them with no retention attempt — so the "best" model depends on which trade-off the business prioritizes, not just the single highest metric.

Key Churn Drivers

Consistent across both models and EDA:


Low tenure (new customers)
Month-to-month contracts
Fiber optic internet service
Lack of Tech Support / Online Security
