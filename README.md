# Bank transaction anomaly detection
Unsupervised anomaly detection on bank transactions using K-Means and KNN, with EDA, feature engineering, and PCA/t-SNE visualization.

## Overview

This project analyzes a dataset of 2,512 bank transactions to identify suspicious behavior without the use of labeled fraud data. Because no explicit fraud labels exist, the problem is treated as an **unsupervised anomaly detection** task — the goal is to identify transactions that behave differently from the majority.

## Dataset

- **Source:** [Bank Transaction Dataset for Fraud Detection — Kaggle](https://www.kaggle.com/)
- **Size:** 2,512 transactions × 16 columns
- **Features:** Transaction amount, account balance, login attempts, customer age, occupation, channel, transaction duration, and timestamps

## Project Structure

├── Bank_Transaction.ipynb   
├── README.md               

## Methodology

### 1. Exploratory Data Analysis
- Dataset structure, missing values, and duplicates
- Descriptive statistics and IQR-based outlier detection
- Distribution visualizations and feature relationships
- Key findings: 4.86% of transactions had multiple login attempts and 4.5% had unusually high amounts — identified as the two primary anomaly signals

### 2. Feature Engineering
The following features were engineered to better capture suspicious behavior:

| Feature | Description |
|---|---|
| TimeSincePreviousTransaction | Days between current and previous transaction |
| AmountToBalanceRatio | Transaction amount divided by account balance |
| AmountZScore | How unusual the amount is relative to that account's history |
| HighLoginFlag | Binary flag for transactions with more than 1 login attempt |
| HighAmountFlag | Binary flag for transactions above the IQR upper bound of $914 |

### 3. Dimensionality Reduction
- **PCA** — retained 30.9% variance; high-amount transactions visually separated from normal ones
- **t-SNE** — produced cleaner separation, with high-amount and high-login transactions forming distinct isolated clusters

### 4. Anomaly Detection

Two unsupervised algorithms were applied and optimized:

**K-Means Clustering**
- Transactions furthest from their cluster center flagged as anomalies
- Tested n_clusters: 4, 6, 8, 10, 12 → selected n_clusters=8
- Identified **126 anomalies**

**K-Nearest Neighbors (KNN)**
- Transactions with the largest average distance to their 20 nearest neighbors flagged as anomalies
- Tested n_neighbors: 5, 10, 20, 30, 50 → selected n_neighbors=20
- Identified **126 anomalies**

**87 transactions were flagged by both models**, representing the strongest anomaly candidates.

## Key Findings

The top anomalies shared a clear pattern — students making transactions of $600–$1,500 against account balances under $500, in some cases spending over 700% of their available balance in a single transaction. Additional red flags included transactions with 4–5 login attempts combined with high amounts. The ratio of transaction amount to account balance proved to be the strongest signal for distinguishing suspicious transactions from normal behavior.

## Tools & Libraries

- Python, Pandas, NumPy
- Matplotlib, Seaborn
- Scikit-learn (KMeans, NearestNeighbors, PCA, t-SNE, StandardScaler)

## How to Run

1. Clone the repository
2. Upload `bank_transactions_data_2.csv` to your Google Drive
3. Open `Bank_Transaction.ipynb` in Google Colab
4. Update the file path in Cell 1 to match your Drive location
5. Run all cells in order
