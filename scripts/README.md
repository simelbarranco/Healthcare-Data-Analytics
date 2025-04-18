# 🏥 Healthcare Analytics – Data Preparation & Feature Engineering

This document outlines the data preparation and feature engineering process for the Healthcare Analytics project. The pipeline is designed to ensure data quality, enable insightful analysis, and support downstream modeling and dashboard reporting.

---

## 📘 1. Data Preprocessing  
### **Python (Pandas & NumPy)** 🐍

This notebook contains the initial data preparation steps. The dataset is cleaned and standardized to make it suitable for analysis, visualization, and further transformation.

### 🔹 Data Loading & Overview

We begin by importing the dataset and inspecting its basic structure. Key tasks:

- Loaded and previewed the dataset
- Verified number of rows and columns
- Checked column data types
- Assessed missing values and data completeness

### 🔹 Data Cleaning

Performed the following operations:

- Standardized column names for consistency
- Removed duplicate entries
- Handled missing values through imputation or row removal
- Formatted date fields and converted types

### ✅ Preprocessing Conclusion

This preprocessing pipeline produces a clean and reliable dataset that is ready for exploratory data analysis, feature engineering, and modeling.

---

## 🧠 2. Feature Engineering

In this notebook, new features were derived to enhance analytical value and model performance. These include:

- A unique synthetic `patient_id`
- `length_of_stay` in days
- An estimated `treatment_cost`
- A `readmitted` flag for 30-day readmissions

---

### 🔹 Loading the Cleaned Dataset

The cleaned dataset from the preprocessing phase is used as input for feature engineering.

---

### 🔹 Feature 1: `patient_id` (Unique Identifier)

To track patients across records, we implemented a custom ID generation strategy:

- **Readable Format**: Combines initials, room number, and condition abbreviation  
  Example: Bobby Jackson, Room 328, Carcinoma → `BJ328CA`
- **Fallback Mechanism**: To avoid duplicates in large datasets, a SHA256 hash of `name + room_number + condition` is used when collisions are detected.

This approach balances **human readability** and **technical uniqueness**.

---

### 🔹 Feature 2: `length_of_stay` (Days)

To derive the Length of Stay for each patient, we performed the following steps:

- Converted `date_of_admission` and `discharge_date` to datetime format
- Calculated the stay as the difference (in days) between discharge and admission
- Replaced zero or negative durations with a minimum of 1 day to maintain logical consistency

This feature offers a foundational metric for healthcare utilization and cost-related insights.

---

### 🔹 Feature 3: `projected_treatment_cost`

Since the dataset lacks a direct `treatment_cost` field, we created a proxy using `billing_amount`:

- Outliers handled via IQR-based capping
- Normalized values for modeling compatibility
- Added cost classification buckets (low, medium, high) based on percentiles

---

### 🔹 Feature 4: `readmitted_risk_flag` Flag

To capture patient readmission trends, we engineered a binary `readmitted` variable:

- Calculated based on date difference between discharge and next admission
- Requires chronological sorting and grouping by patient
- Useful for predictive modeling and performance metrics

---

### ✅ Feature Engineering Conclusion

These engineered features add structure and meaning to the dataset, enabling richer insights and stronger model performance. Each transformation ensures interpretability, consistency, and alignment with healthcare analysis needs.

---
