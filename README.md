# Fraud Detection in Financial Transactions

This project focuses on developing a machine learning model to detect fraudulent transactions for a financial company. The goal is to leverage insights from the model to create an actionable plan for fraud prevention and to inform infrastructure updates.

## Table of Contents
- [Business Context](#business-context)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Model Performance](#model-performance)
- [Key Predictive Factors](#key-predictive-factors)
- [Prevention Strategies](#prevention-strategies)
- [Evaluating Effectiveness](#evaluating-effectiveness)
- [How to Run](#how-to-run)

## Business Context
The objective of this project is to build a real-time fraud detection model. Financial fraud is a significant concern, and the ability to identify and prevent fraudulent transactions is crucial for maintaining customer trust and minimizing financial losses. This project analyzes a large transactional dataset to identify patterns indicative of fraud.

## Dataset
The analysis is based on a transactional dataset containing 6,362,620 records and 10 columns. The data is in CSV format and includes features such as transaction type, amount, old and new balances of both the sender and receiver, and a flag indicating if the transaction was fraudulent.

## Methodology
The project follows a structured data science workflow:
1.  **Data Cleaning & Preprocessing**: The initial data exploration revealed no missing values. A key finding was the presence of significant outliers in the 'amount' column. Instead of removing these outliers, which could represent important fraudulent activities, `RobustScaler` was used for feature scaling due to its resilience to outliers. A multicollinearity analysis identified a nearly perfect correlation between the old and new balance columns for both the originator and destination accounts. To address this, `newbalanceOrig` and `newbalanceDest` were removed.

2.  **Feature Selection & Engineering**:
    *   Identifier columns (`nameOrig`, `nameDest`) were dropped as they offer no predictive value.
    *   The categorical `type` column was one-hot encoded to be used in the machine learning model.
    *   Features with high multicollinearity were removed.
    *   The final feature set includes `step`, `amount`, `oldbalanceOrg`, `oldbalanceDest`, `isFlaggedFraud`, and the one-hot encoded transaction types.

3.  **Model Development**: A **Random Forest Classifier** was selected for this task. This model is well-suited for handling complex classification problems and is generally robust to outliers. To address the severe class imbalance in the dataset, the `class_weight='balanced'` parameter was utilized. This helps the model to learn the patterns of the rare fraud cases.

## Model Performance
The Random Forest model demonstrated excellent performance in identifying fraudulent transactions.

*   **Classification Report**: The model achieved high precision and a good recall for the 'Fraud' class, indicating it successfully identifies a significant percentage of actual fraudulent transactions while maintaining a low false positive rate.
*   **ROC AUC Score**: The ROC AUC score of **0.9825** signifies a strong ability to distinguish between fraudulent and non-fraudulent transactions.
*   **Confusion Matrix**: The confusion matrix provides a detailed breakdown of the model's predictions, showing the number of true positives, true negatives, false positives, and false negatives.

## Key Predictive Factors
The key factors that predict a fraudulent transaction, in order of importance, are:
1.  **oldbalanceOrg**: The sender's balance before the transaction.
2.  **amount**: The transaction amount.
3.  **type_TRANSFER**: Whether the transaction is a transfer.
4.  **type_CASH_OUT**: Whether the transaction is a cash-out.
5.  **step**: The time/hour of the transaction.

These factors align with typical fraud patterns, such as "drain-out" schemes where a fraudster attempts to empty an account through large transfers or cash-outs, often to new or low-balance "mule accounts".

## Prevention Strategies
Based on the model's insights, the following multi-layered security infrastructure is recommended:
*   **Implement a Dynamic, Real-Time Rule Engine**: Create rules based on the model's logic, such as flagging transactions where the amount is a high percentage of the original balance for 'CASH_OUT' or 'TRANSFER' types.
*   **Deploy Behavioral Anomaly Detection**: Profile normal customer behavior and flag significant deviations.
*   **Introduce Adaptive Multi-Factor Authentication (MFA)**: Trigger MFA for transactions with a medium-to-high risk score to balance security and user convenience.
*   **Enhance Recipient Account Scrutiny**: Implement real-time risk scoring for recipient accounts, flagging transfers to new or low-activity accounts.

## Evaluating Effectiveness
To determine the effectiveness of the implemented prevention strategies, a robust monitoring framework is essential.
*   **Key Performance Indicators (KPIs)**: Track metrics such as the fraud rate (both volume and value), false positive rate, and detection rate (recall).
*   **A/B Testing (Champion/Challenger Model)**: The most effective way to evaluate the new system is through A/B testing. A small, random subset of users would be moved to the new system (Challenger), while the rest remain on the old system (Champion). Comparing the KPIs between the two groups after a set period will provide a definitive measure of the new system's impact.

## How to Run
To run this project, you will need to have Python and the following libraries installed:
*   pandas
*   numpy
*   matplotlib
*   seaborn
*   scikit-learn

You can install these dependencies using pip:
pip install pandas numpy matplotlib seaborn scikit-learn
Then, you can run the Jupyter Notebook insaid_fraud_detection.ipynb to see the full analysis and model development process.
