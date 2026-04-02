# Data Cleaning & Feature Engineering Decisions

## 1. Data Cleaning

### Duplicate Removal

Duplicates were identified based on `transaction_id`. Since each transaction should have a unique identifier, duplicate rows indicate redundancy or data errors. Therefore, duplicates were removed while keeping the first occurrence of each transaction.

### Amount Cleaning

Some values in the `montant` column used commas instead of dots as decimal separators, which caused the column to be interpreted as text (`object`). These commas were replaced with dots, and the column was converted to a numeric format (`float`) to allow proper calculations.

### Text Standardization

Several text columns were standardized to ensure consistency and avoid errors:

* `devise` values were converted to uppercase (e.g., eur → EUR)
* `segment_client` values were normalized to a consistent format (e.g., PREMIUM → Premium)
* Extra spaces in the `agence` column were removed

This ensures uniform formatting across the dataset.

### Missing Values Handling

Missing values were handled using different strategies depending on the column:

* The mode was used for categorical variables such as `segment_client`
* The median was used for numerical variables such as `score_credit_client`
* Missing values in `agence` were replaced with "Inconnue"

This approach preserves the dataset while minimizing bias.

---

## 2. Outlier Detection & Treatment

### Outliers in `montant`

Outliers were detected using the IQR method. This method was preferred over Z-score because it is more robust and focuses on the distribution of the majority of the data rather than being influenced by extreme values.

Both methods were tested:

* IQR detected 108 outliers
* Z-score detected only 20

Since Z-score depends on the mean and the dataset contains extreme values, IQR was considered more accurate and was selected.

### Credit Score Anomalies

For `score_credit_client`, instead of using IQR or Z-score, a rule-based approach was applied:

* Scores below 0 or above 850 were flagged as invalid

This approach was chosen because valid credit score ranges are already known, making statistical methods unnecessary in this case.

### Anomaly Flagging

A boolean column `is_anomaly` was created to flag abnormal records. No rows were deleted or modified.

This decision was made because anomalous values are not necessarily incorrect — they may represent rare but valid cases. Therefore, keeping them while marking them allows for further analysis without losing information.

---

## 3. Currency Verification

A new column `montant_eur_verifie` was calculated using:
montant / taux_change_eur

This was used to verify whether the existing `montant_eur` values were correctly computed.

During this process, some suspicious values were identified in the `montant` column (e.g., 99999, 200000, 888888). These values appear artificial or unrealistic. However, no correction or deletion was applied, as there is no confirmation that they are incorrect.

---

## 4. Final Dataset Preparation

### Column Cleaning and Organization

Unnecessary helper columns created during the analysis (such as intermediate calculation columns) were removed. The remaining columns were reorganized into a logical structure to improve readability and usability.

### Export

The final dataset was exported as `financecore_clean.csv`. Numeric values were rounded to improve readability, as some columns contained excessive decimal precision.

---
