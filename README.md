# Ethereum Fraud Detection Dataset

## 📖 Overview
This project analyzes Ethereum wallet behavior to classify addresses as either phishing (fraudulent) or normal.  
I started with exploratory data analysis (EDA) to understand behavioral differences between normal and fraudulent accounts, using visualizations to uncover patterns in transaction activity. Then, I applied machine learning to build a predictive fraud classification model based on these insights.

## 🎯 Objective

- Explore wallet behavior patterns between phishing and normal accounts.
- Visualize key differences and detect outliers.
- Train models to classify fraud, including logistic regression, KNN, and ensemble methods such as random forest and XGBoost.
- Evaluate model performance using precision, recall, and F1-score.

## 📂 Dataset

Dataset Source: [Kaggle - Ethereum Fraud Detection](https://www.kaggle.com/datasets/vagifa/ethereum-frauddetection-dataset)  
The dataset contains 9,841 Ethereum wallet addresses with 49 behavioral features.

#### 🎯 Target Variable: `FLAG`
- `1`: Phishing (fraudulent address)
- `0`: Normal address

## 🔍 Key Findings

- **Class Imbalance**  
  Out of 9,816 Ethereum addresses, **22.2%** were flagged as phishing, indicating a need for class-balancing techniques.

- **ERC-20 Activity Insight**  
  Many addresses with missing ERC-20 values had no corresponding token transfers. These were logically **imputed as zeros** to reflect no activity.

- **Transaction Timing**  
  - **Phishing wallets** often sent transactions **instantly** (median = 0 minutes), showing burst-like, immediate behavior.  
  - **Normal wallets** had a higher median delay (~23 minutes) and a broader range, suggesting more organic or sustained usage.

- **Value Behavior**  
  - Phishing wallets typically **sent small amounts** of ETH — 50% of them sent ≤ 1 ETH.  
  - They also **received significantly smaller amounts** — half received less than 2 ETH.  
  - Normal wallets showed **higher and more varied transaction values**, both sent and received.

- **Interaction Behavior**  
  - **Phishing wallets** interacted with a **larger number of unique addresses**, often one-time interactions with potential victims.  
  - **Normal wallets** showed **more frequent interactions** with a smaller, stable set of addresses.

- **Transaction Volume (Bar Chart Insight)**  
  - **Phishing wallets** had **fewer sent and received transactions**, suggesting short-lived or one-off activity.  
  - **Normal wallets** had **higher activity levels**, reflecting consistent and ongoing use of the network.
 
## 🧠 Modeling Summary

I trained and evaluated multiple models for fraud detection:
- **Logistic Regression** for interpretability.
- **KNN** to explore local neighborhood behavior.
- **Random Forest** and **XGBoost** for non-linear pattern detection.

**XGBoost** achieved the best performance:
- Accuracy: over 95%
- Precision (fraud): ~90%
- Recall (fraud): ~86%

### 🔝 Top 10 Significant Features Impacting Suspicious Flag (from Logistic Regression)

| Feature                                   | Coefficient    | Direction | Interpretation                                                                                                                     |
| ----------------------------------------- | -------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `received_higher_sent`                    | **-3.0399**    | ↓         | Addresses that receive more Ether than they send are **much less likely** to be flagged. Indicates legitimate accumulation.        |
| `ERC20_received_higher_sent`              | **2.1437**     | ↑         | Receiving more ERC20 tokens than sending is **strongly associated** with being flagged. May reflect malicious collection behavior. |
| `Time Diff between first and last (Mins)` | **-1.101e-05** | ↓         | Longer active duration **reduces** the chance of being flagged — fraudulent wallets often have short lifespans.                    |
| `ERC20 uniq rec contract addr`            | **0.0530**     | ↑         | More unique ERC20 receiving contract addresses **increase** the odds of being flagged. May reflect opportunistic or bot behavior.  |
| `Avg min between sent tnx`                | **1.178e-05**  | ↑         | Longer gaps between sent transactions are **linked to higher risk** — suggesting inactivity or bursts of malicious activity.       |
| `Sent tnx`                                | **-0.0090**    | ↓         | More outgoing transactions **decrease** the risk — reflects active, legitimate behavior.                                           |
| `max val sent`                            | **-0.0008**    | ↓         | Higher maximum sent values are **associated with lower** risk — possibly large, legitimate transfers.                              |
| `Total ERC20 tnxs`                        | **-0.0024**    | ↓         | More ERC20 transactions **reduce** the likelihood of being flagged. Likely reflects legitimate trading behavior.                   |
| `Received Tnx`                            | **-0.0004**    | ↓         | Higher number of received transactions **lowers** risk — shows network activity.                                                   |
| `ERC20 total Ether received`              | **2.717e-11**  | ↑         | A small but positive effect — more ERC20 token value received correlates with a **slightly higher** chance of being flagged.       |

The **Pseudo R-squared** of 0.4716 indicates that the model explains ~47% of the variation in the target variable. This is considered a strong fit for classification problems involving behavioral or fraud detection data.

## 🧾 Feature Description


|Variable                                  |Description                                                                 |
|:----------------------------------------|:---------------------------------------------------------------------------|
|Address                                  |The address of the Ethereum account                                         |
|FLAG                                     |Whether the transaction is fraud or not                                     |
|Avg min between sent tnx                 |Average time between sent transactions for account in minutes               |
|Avg_min_between_received_tnx             |Average time between received transactions for account in minutes           |
|Time_Diff_between_first_and_last(Mins)   |Time difference between the first and last transaction                      |
|Sent_tnx                                 |Total number of sent normal transactions                                    |
|Received_tnx                             |Total number of received normal transactions                                |
|Number_of_Created_Contracts              |Total number of created contract transactions                               |
|Unique_Received_From_Addresses           |Total unique addresses from which account received transactions             |
|Unique_Sent_To_Addresses20               |Total unique addresses to which account sent transactions                   |
|Min_Value_Received                       |Minimum value in Ether ever received                                        |
|Max_Value_Received                       |Maximum value in Ether ever received                                        |
|Avg_Value_Received5                      |Average value in Ether ever received                                        |
|Min_Val_Sent                             |Minimum value of Ether ever sent                                            |
|Max_Val_Sent                             |Maximum value of Ether ever sent                                            |
|Avg_Val_Sent                             |Average value of Ether ever sent                                            |
|Min_Value_Sent_To_Contract               |Minimum value of Ether sent to a contract                                   |
|Max_Value_Sent_To_Contract               |Maximum value of Ether sent to a contract                                   |
|Avg_Value_Sent_To_Contract               |Average value of Ether sent to contracts                                    |
|Total_Transactions(Including_Tnx_to_Create_Contract)|Total number of transactions including contract creation         |
|Total_Ether_Sent                         |Total Ether sent from the account address                                   |
|Total_Ether_Received                     |Total Ether received by the account address                                 |
|Total_Ether_Sent_Contracts               |Total Ether sent to contract addresses                                      |
|Total_Ether_Balance                      |Total Ether balance after transactions                                      |
|Total_ERC20_Tnxs                         |Total number of ERC20 token transfer transactions                           |
|ERC20_Total_Ether_Received               |Total ERC20 token received transactions in Ether                            |
|ERC20_Total_Ether_Sent                   |Total ERC20 token sent transactions in Ether                                |
|ERC20_Total_Ether_Sent_Contract          |Total ERC20 token sent to contracts in Ether                                |
|ERC20_Uniq_Sent_Addr                     |Number of ERC20 token transactions sent to unique addresses                 |
|ERC20_Uniq_Rec_Addr                      |Number of ERC20 token transactions received from unique addresses           |
|ERC20_Uniq_Rec_Contract_Addr             |Number of ERC20 token transactions received from unique contract addresses  |
|ERC20_Avg_Time_Between_Sent_Tnx          |Average time between ERC20 token sent transactions in minutes               |
|ERC20_Avg_Time_Between_Rec_Tnx           |Average time between ERC20 token received transactions in minutes           |
|ERC20_Avg_Time_Between_Contract_Tnx      |Average time between ERC20 contract token transactions                      |
|ERC20_Min_Val_Rec                        |Minimum Ether value received from ERC20 token transactions                  |
|ERC20_Max_Val_Rec                        |Maximum Ether value received from ERC20 token transactions                  |
|ERC20_Avg_Val_Rec                        |Average Ether value received from ERC20 token transactions                  |
|ERC20_Min_Val_Sent                       |Minimum Ether value sent from ERC20 token transactions                      |
|ERC20_Max_Val_Sent                       |Maximum Ether value sent from ERC20 token transactions                      |
|ERC20_Avg_Val_Sent                       |Average Ether value sent from ERC20 token transactions                      |
|ERC20_Uniq_Sent_Token_Name               |Number of unique ERC20 tokens transferred                                   |
|ERC20_Uniq_Rec_Token_Name                |Number of unique ERC20 tokens received                                      |
|ERC20_Most_Sent_Token_Type               |Most sent token type via ERC20 transactions                                 |
|ERC20_Most_Rec_Token_Type                |Most received token type via ERC20 transactions                             |
