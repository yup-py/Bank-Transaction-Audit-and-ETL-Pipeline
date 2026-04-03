
# DECISIONS.md – Data Cleaning & Preparation

## Project: FinanceCore SA – Bank Transactions

---

## 1. Data Exploration

The dataset contains 2060 rows and several data quality issues:

* Missing values in important columns (`score_credit_client`, `segment_client`, `agence`)
* Duplicate transaction IDs
* Inconsistent date formats
* Incorrect numeric formats (commas instead of dots)
* Suspicious and extreme values in `montant`
* Inconsistent text formatting (case and spaces)

A full cleaning process was required before analysis.

---

## 2. Duplicate Handling

Duplicate rows were identified using `transaction_id`.

**Decision:**
Duplicates were removed while keeping the first occurrence.

**Reason:**
Each transaction should have a unique ID. Keeping the first occurrence ensures consistency without introducing ambiguity. Other options like keeping the last occurrence or aggregating were not used because duplicates should not represent multiple valid transactions.

---

## 3. Data Type and Format Corrections

### Dates

**Issue:** Mixed formats (`DD/MM/YYYY` and `YYYY-MM-DD`)

**Decision:** Converted all values to a unified datetime format

**Reason:** Required for time-based analysis and feature extraction

---

### Montant

**Issue:** Some values used commas instead of dots, causing incorrect data types

**Decision:** Replaced commas with dots and converted to float

**Reason:** Ensures correct numerical calculations. Using direct replacement avoids introducing missing values that could happen with automatic conversion methods.

---

### Solde Avant

**Issue:** Values contained text suffix ("EUR")

**Decision:** Removed the suffix and converted to float

**Reason:** Numeric fields must be clean for analysis.

---

## 4. Text Standardization

**Actions:**

* Converted `devise` to uppercase
* Standardized `segment_client` (e.g., PREMIUM → Premium)
* Removed extra spaces in `agence`

**Reason:**
Ensures consistency and prevents errors during grouping or analysis.

---

## 5. Missing Values Treatment

**Decisions:**

* Median for `score_credit_client`
* Mode for `segment_client`
* "Inconnue" for missing `agence`

**Reason:**
Median is robust to outliers, mode preserves the most frequent category, and replacing with "Inconnue" avoids data loss while keeping missing information explicit.

---

## 6. Outlier Detection and Treatment

### Montant

Two methods were tested:

* IQR → detected 108 outliers
* Z-score → detected 20 outliers

**Decision:** Used IQR

**Reason:**
IQR focuses on the central distribution and is not affected by extreme values. Z-score depends on the mean, which is influenced by large outliers, making it less reliable in this dataset.

---

### Credit Score

**Decision:** Used a rule-based approach

* Values < 0 or > 850 were flagged

**Reason:**
Credit scores follow a known valid range. Using statistical methods like IQR or Z-score is unnecessary when domain rules are available.

---

### Anomaly Handling

A column `is_anomaly` was created to flag abnormal values.

**Decision:** No rows were removed or corrected

**Reason:**
Outliers are not always errors. They may represent rare but valid cases. Keeping them with a flag preserves data integrity and allows further analysis.

---

## 7. Currency Verification

A new column `montant_eur_verifie` was created using:

montant / taux_change_eur

**Reason:**
To verify whether `montant_eur` was correctly calculated.

During this step, suspicious values such as 99999, 200000, and 888888 were identified. These values appear artificial, but no correction was applied due to lack of confirmation.

---

## 8. Feature Engineering

The following features were created:

* Date features: `annee`, `mois`, `trimestre`, `jour_semaine`
* Risk category: `categorie_risque`
* Client metrics: `nb_transactions`, `montant_moyen`, `nb_produits_distincts`
* Operational metric: `taux_rejet_agence`

**Reason:**
These features improve analysis and provide meaningful business insights.

---

## 9. Final Dataset Preparation

**Actions:**

* Removed helper columns used during processing
* Reordered columns logically
* Rounded numeric values

**Reason:**
Improves readability, usability, and overall dataset quality.

---

## 10. Final Output

The cleaned and enriched dataset was exported as:

financecore_clean.csv

It is now ready for analysis, dashboarding, and further use.

---
