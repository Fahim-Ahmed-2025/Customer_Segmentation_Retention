# Customer Segmentation & Retention Analysis (Decision Assistant)

A strong end-to-end project that turns transactional retail data into **customer segments** and **churn risk** insights.  
This repository includes segmentation (KMeans + representation learning), retention analysis (cohorts), and churn prediction (LogReg, RandomForest, HistGradientBoosting) with clear evaluation using **correlation heatmaps, ROC curves, loss, and learning curves**.

---

## Project Goals
- Build **meaningful customer segments** from transaction history
- Perform **retention analysis** using cohort retention heatmaps
- Predict **churn/retention risk** and generate a **priority list** for action
- Provide a simple **customer decision assistant** (risk/value/priority)

---

## Dataset
**Online Retail.xlsx** (transaction-level invoice line items)

Core columns used:
- `InvoiceNo`, `InvoiceDate`, `CustomerID`, `Quantity`, `UnitPrice`, `StockCode`, `Country`

> Dataset source: UCI Machine Learning Repository – Online Retail  

---

## Repository Structure (Recommended)
```
.
├── notebooks/
│   └── Customer_Segmentation_Retention_Simplified.ipynb
├── reports/
│   ├── Customer Segmentation & Churn Analysis.pptx
│   └── Customer_Segmentation_Project_Input_Output_Report.docx
├── outputs/
│   ├── customer_segments_and_risk.csv
│   └── priority_high_value_high_risk.csv
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Workflow Summary

### 1) Data Cleaning
- Drop missing `CustomerID`
- Remove cancellations (`InvoiceNo` contains `C`)
- Keep `Quantity > 0` and `UnitPrice > 0`
- Create `TotalPrice = Quantity × UnitPrice`

### 2) Feature Engineering (Customer-level)
Main engineered features:
- `recency_days`, `frequency`, `monetary`
- `avg_order_value`, `basket_size`, `unique_products`
- `tenure_days`, purchase gap stats (`gap_mean`, `gap_std`, `gap_min`, `gap_max`)
- `return_rate`, `cancel_rate`

### 3) Correlation Analysis
- Spearman correlation **matrix + heatmap**
- Helps understand redundancy and relationships among features

### 4) Segmentation (Clustering)
**Models used**
- KMeans baseline
- Representation learning (Autoencoder-style) + KMeans

**Evaluation metrics**
- Silhouette score
- Davies-Bouldin index
- Calinski-Harabasz index

**Learning curve**
- KMeans elbow curve (inertia vs k)
- Representation model train vs validation loss (+ test loss)

### 5) Retention (Cohort Analysis)
- Cohort by first purchase month
- Retention table + heatmap

### 6) Churn Prediction (Supervised)
**Models compared**
- Logistic Regression (baseline)
- Random Forest
- HistGradientBoosting

**Evaluation**
- ROC-AUC, PR-AUC
- LogLoss (loss function)
- Lift@10%
- Learning curve for each model (PR-AUC vs training size)
- ROC curve for all models

### 7) Hyperparameter Tuning
- RandomForest tuned with RandomizedSearchCV
- Compare tuned vs baseline (PR-AUC / LogLoss)

### 8) Decision Outputs
Outputs for each customer:
- `segment_id`, `segment_name`
- `risk_prob`, `risk_score`
- `value_score`
- `priority_score`

---

## Outputs
The notebook exports:
- `customer_segments_and_risk.csv`  
  Contains segments + churn risk + value + priority for all customers
- `priority_high_value_high_risk.csv`  
  Top priority customers (default top 200)

---

## How to Run

### 1) Install dependencies
```bash
pip install -r requirements.txt
```

### 2) Add dataset
Place the dataset in the project root (or update the path inside the notebook):
```
Online Retail.xlsx
```

### 3) Run notebook
Open and run:
- `notebooks/Customer_Segmentation_Retention_Simplified.ipynb`

---

## Requirements
See `requirements.txt`. Main libraries:
- pandas, numpy, matplotlib
- scikit-learn
- openpyxl
- torch (only if using the true autoencoder implementation)

---

## Notes (Important)
- **Clustering has no "accuracy"** because there is no ground truth label.
- For churn prediction, **PR-AUC** is emphasized because churn is often imbalanced.
- Churn label is **heuristic** (example: `recency_days > 90`), and can be improved with time-based validation.

---

## Ethical Considerations
- CustomerIDs are anonymized
- No personal identifiers are used
- Recommend sharing only aggregated insights and anonymized scores

---

## References (Short)
- MacQueen, J. (1967) KMeans clustering
- Lloyd, S. (1982) KMeans / quantization
- Hinton & Salakhutdinov (2006) representation learning / autoencoders
- Hughes, A.M. Strategic Database Marketing (RFM)
- UCI ML Repository – Online Retail dataset

---

## Author
**Fahim Ahmed**  
M.Sc. Data Science Student, University of Hertfordshire
