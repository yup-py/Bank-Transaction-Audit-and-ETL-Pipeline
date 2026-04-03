# 📊 FinanceCore SA – Bank Transactions Analysis

## 📌 Project Overview

This project focuses on cleaning, preparing, and enriching a bank transactions dataset to make it ready for analysis and dashboarding.

The dataset contained multiple data quality issues such as missing values, duplicates, inconsistent formats, and outliers, which were handled through a structured data cleaning pipeline.

---

## 🎯 Objectives

- Clean and preprocess raw transaction data
- Handle missing values and inconsistencies
- Detect and analyze outliers
- Standardize data formats
- Create new features for analysis
- Produce a final clean dataset

---

## 📂 Dataset

- **File:** `bank-transactions.csv`
- **Rows:** 2060
- **Description:** Contains transaction-level data including clients, amounts, dates, and agencies

## 📥 Installation

```bash
pip install -r requirements.txt
```

---

## ⚙️ Data Cleaning Steps

### 1. Data Exploration

- Identified missing values
- Detected duplicates
- Checked data types and inconsistencies

---

### 2. Duplicate Handling

- Removed duplicates based on `transaction_id`
- Kept the first occurrence

---

### 3. Data Type Fixing

- Converted dates using `pd.to_datetime()`
- Cleaned numeric columns (`montant`, `solde_avant`)
- Fixed incorrect formats (commas, text values)

---

### 4. Text Standardization

- Standardized categories (`segment_client`, `devise`)
- Removed extra spaces
- Ensured consistent formatting

---

### 5. Missing Values Treatment

- Numerical columns → filled with median
- Categorical columns → filled with mode or `"Inconnue"`

---

### 6. Outlier Detection

- Tested:
  - IQR method (selected)
  - Z-score method (for comparison using SciPy)
- Created `is_anomaly` column
- No rows removed to preserve data integrity

---

### 7. Data Validation

- Verified currency conversion (`montant_eur_verifie`)
- Identified suspicious values

---

## 🧠 Feature Engineering

Created new features to improve analysis:

- Date features: `annee`, `mois`, `trimestre`, `jour_semaine`
- Risk category: `categorie_risque`
- Client metrics:
  - Number of transactions
  - Average transaction amount
  - Number of distinct products
- Agency metric:
  - Rejection rate

---

## 📤 Final Output

- Clean dataset: `financecore_clean.csv`
- Documentation: `DECISIONS.md`

---

## 🛠️ Tools & Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- SciPy
- Jupyter Notebook

---

## 📊 Methods & Techniques

- Data cleaning and preprocessing
- Missing value imputation (median, mode)
- Outlier detection (IQR, Z-score)
- Feature engineering
- Data validation

---

## 📈 Result

The final dataset is:

- Clean and consistent
- Enriched with meaningful features
- Ready for analysis, visualization, and machine learning
