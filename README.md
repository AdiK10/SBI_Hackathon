# 📊 UPI PDF Statement Analyzer

This project allows users to upload their PhonePe/UPI transaction statement in PDF format and automatically generates:

- ✅ A cleaned CSV file (`upi_first60_with_location.csv`)
- 🧾 A PDF Summary report (`upi_summary.pdf`)
- 🌐 A PDF of Recipient Location URLs (`upi_location_urls.pdf`)

---

## 🔧 How It Works

1. Upload your UPI transaction PDF via the web interface.
2. The backend (built with Flask) processes the PDF:
   - Extracts transaction info
   - Classifies top recipients by type (person/place)
   - Searches for location URLs (JustDial/Zomato/etc.)
3. Generates downloadable files:
   - Parsed CSV
   - Transaction summary PDF
   - Location URLs PDF
     
<img width="308" height="175" alt="image" src="https://github.com/user-attachments/assets/84607998-e01a-4078-8ee3-8d4473fd61bd" />


---

## 🚀 How to Use

### 1. Clone the Repo

```bash
git clone https://github.com/AdiK10/SBI_Hackathon.git
cd SBI_Hackathon
# SBI_Hackathon
2. Install Dependencies
Use pip to install required packages:

Machine Learning Model: Defaulter Prediction
This project aims to develop a reliable binary classification model to predict whether a financial account is likely to default. The approach includes data cleaning, feature engineering, model training, hyperparameter tuning, and threshold optimization.

🔍 Problem Statement
Given structured financial and behavioral data, predict whether a user will default (1) or not (0).
The dataset is highly imbalanced, with only ~9.8% defaulters in the training data.

📊 Dataset Overview
Input: Anonymized account-level data including credit flags, behavioral summaries, transactional stats, and CRIF score bins.

Target: Binary class (1 = defaulter, 0 = non-defaulter)

Challenge: Severe class imbalance and partially anonymized feature names.

🧹 Data Preprocessing
Steps applied before modeling:

Null Handling

Columns with high missing value fractions (> 90%) were dropped.

Remaining nulls were imputed using appropriate strategies (mean/median/mode).

Feature Engineering

Aggregated monthly transactional features into behavior indicators.

Extracted time-series statistics (min, max, average, volatility).

Encoded categorical features and created binary flags where applicable.

Standardization

All numeric features were scaled using StandardScaler to aid model convergence.

Feature Selection

Highly correlated features were removed.

Top features were selected using feature importance from a base Random Forest classifier (via skbeast).

⚙️ Model Training & Tuning
We used XGBoostClassifier, which handles class imbalance and provides gradient boosting efficiency.

🔁 Hyperparameter Tuning
Hyperparameters were optimized using RandomizedSearchCV with 5-fold stratified cross-validation. Evaluation metric: F1-score.

✅ Best Hyperparameters:
python
Copy
Edit
{
  'max_depth': 7,
  'learning_rate': 0.05,
  'n_estimators': 500,
  'subsample': 0.8,
  'reg_lambda': 5,
  'reg_alpha': 0.5,
  'gamma': 1,
  'colsample_bytree': 1.0,
  'scale_pos_weight': 9.2  # ratio of negative to positive samples
}
🎯 Threshold Optimization
After training, probability predictions were converted to class labels using a tuned decision threshold.

Default threshold 0.5 led to over-prediction of defaulters (~33,000 accounts).

Tuning the threshold to 0.7 achieved better precision-recall balance and aligned predicted defaulter proportion with real-world data (~9%).

Threshold	Precision	Recall	F1-Score	Accuracy	Predicted Defaulters
0.5	0.43	0.82	0.57	87%	~33,000 (~17%)
0.7	0.56	0.68	0.61	91%	~18,000 (~9%)

🟢 Final threshold chosen: 0.7

📈 Final Model Evaluation (on Validation Set)
F1 Score: 0.611

ROC AUC: 0.925

Confusion Matrix (Threshold = 0.7):

lua
Copy
Edit
[[54642  3819]
 [ 2287  4801]]
💾 Model Saving & Inference
python
Copy
Edit
# Save model
import joblib
joblib.dump(xgb_best, 'xgb_model.pkl')

# Predict with threshold
probas = xgb_best.predict_proba(X_test)[:, 1]
preds = (probas >= 0.7).astype(int)

# Append to test DataFrame
df_test["prediction"] = preds
🧠 Insights & Observations
Hyperparameter tuning significantly improved generalization (validated via cross-validation).

Threshold tuning reduced false positives, aligning with business needs (fewer flagged accounts with higher confidence).

The proportion of predicted defaulters (~9%) now closely matches the real-world defaulter rate.

bash
Copy
Edit
pip install -r requirements.txt

python app.py
