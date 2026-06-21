# ai-driven-fd
This project introduces an end-to-end AI Decision Agent designed to detect online payment fraud with high accuracy. By utilizing an Optuna-optimized LightGBM model, the system achieves a 2.4x performance increase over the baseline.
Autonomous Financial Fraud Detection System

An end-to-end, cost-sensitive machine learning system that detects fraudulent online
credit-card transactions and wraps the model in a rule-based decision agent that
autonomously routes each transaction to APPROVE, REVIEW, or FLAG.

Graduation project — Biruni University, Computer Engineering (3-person team).


🎯 Key Results

MetricScoreAUC-PR0.6307  (≈ 2.4× over baseline)AUC-ROC0.9179Primary metricsF2-score & AUC-PR — chosen over accuracy because of the 3.5% fraud rateDatasetIEEE-CIS — 590K+ transactions, 434 raw features

🧩 Why this project

Fraud detection is hard because fraud is rare (here only ~3.5% of transactions) and
the cost of errors is asymmetric — missing a real fraud is far more expensive than a
false alarm. Optimizing for plain accuracy is misleading in this setting. This system is
built around those realities: cost-sensitive learning, the right evaluation metrics, and
an explainable decision layer that a risk team could actually act on.

⚙️ How it works

1. Feature Engineering


Processed the IEEE-CIS dataset (590K+ transactions, 434 raw columns).
Synthesized a unique User ID (UID) to tie transactions to behavioral identities.
Engineered 135 leakage-safe features using expanding-window velocity features and
target encoding, with explicit care to prevent time-based data leakage.


2. Cost-Sensitive Modeling


Addressed extreme class imbalance (3.5% fraud) and asymmetric error cost.
Trained LightGBM (with XGBoost as a comparison), tuned via Optuna (TPE) Bayesian
hyperparameter search.
Optimized for F2-score and AUC-PR rather than accuracy.


3. Explainability (XAI)


Integrated SHAP (TreeExplainer) for both global feature importance and
per-transaction waterfall explanations.
Makes every decision auditable at the feature level — supporting transparency and
regulatory requirements such as the "right to explanation."


4. Rule-Based Decision Agent


Instead of returning a raw probability, a 7-step decision agent (FraudDetectionAgent)
synthesizes the model score, the SHAP explanation, velocity checks, and
banking risk rules.
Produces an autonomous decision per transaction: APPROVE / REVIEW / FLAG.


5. Concept-Drift Protection


Added dynamic data-masking strategies so the model stays robust as user behavior shifts
over time in a production-like setting.


🛠️ Tech Stack

Python · LightGBM · XGBoost · Optuna (Bayesian optimization / TPE) ·
SHAP (explainable AI) · scikit-learn · pandas · Feature Engineering

📊 Dataset

IEEE-CIS Fraud Detection (Kaggle)
— 590K+ transactions, 434 raw features.

📁 Project Structure

fraud-detection/
├── notebooks/        # EDA, feature engineering, modeling
├── src/              # FraudDetectionAgent and pipeline code
├── data/             # gitignored — download from Kaggle
├── requirements.txt
└── README.md



📈 Results 


SHAP global feature-importance (summary) plot
A per-transaction SHAP waterfall example
Precision–Recall curve
Confusion matrix


![SHAP summary](images/shap-summary.png)

👥 Team

3-person graduation project — Biruni University, Computer Engineering.




Note: the "decision agent" is a deterministic, rule-based orchestration layer
(model score + SHAP + velocity + risk rules) — not an LLM-based agent.
