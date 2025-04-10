
# Technical Documentation: Data Engineering Pipeline for Fintech on Databricks

## 📌 Overview

This project implements a data engineering pipeline focused on financial transactions in a fintech context. It was developed on Databricks Community Edition with the goal of processing, transforming, enriching, and analyzing transaction data to simulate fraud detection scenarios.

---

## 🗂️ Pipeline Stages

### 📥 1. Data Ingestion

- **Source**: CSV file with synthetic financial transaction data.
- **Upload**: Performed through the Databricks web interface to DBFS.
- **Input format**: Comma-separated CSV.
- **Main columns**:
  - `transaction_id`, `customer_id`, `amount`, `timestamp`, `merchant`, `location`, `payment_method`, `status`, `fraud_flag`

---

### 🔧 2. Transformations

**Objective**: Standardize data, handle inconsistencies, and prepare it for analysis.

- **Data type conversion**:
  - `amount`: converted to `DoubleType`
  - `timestamp`: converted to `TimestampType`

- **Null handling**:
  - Rows with nulls in: `customer_id`, `timestamp`, `amount`, `fraud_flag` are removed.
  - Nulls in: `merchant`, `location`, `payment_method`, `status` are filled with `"unknown"`.

- **Derived columns**:
  - `day_of_week`: extracted from `timestamp`
  - `hour`: hour of the day extracted from `timestamp`

- **Value normalization and text cleaning**:
  - `lower()`, `trim()`, and `regexp_replace()` applied to standardize and clean text fields.
  - Columns cleaned: `merchant`, `location`, `payment_method`

---

### 📈 3. Enrichment

- **Geographic mapping (fictitious)**:
  - `location` column mapped to Brazilian states (e.g., São Paulo, Bahia, Minas Gerais).

- **Customer risk category (simulated)**:
  - Risk levels assigned based on behavior:
    - Customers with frequent and high-value transactions are labeled `high_risk`.
    - Others are labeled as `medium_risk` or `low_risk`.

---

### 📊 4. Aggregations (Gold Layer)

Key metrics generated using Spark SQL and the DataFrame API:

- **Spending volume per period**:
  - Total spent by customer per day, week, and month.

- **Volume by payment method**:
  - Grouped sums and counts by `payment_method`.

- **Fraud indicators**:
  - Fraud rate (`fraud_rate`) by:
    - Location (`location`)
    - Time of day (`hour`)
    - Transaction amount range (`amount`)

---

## 📌 Technical Notes

- Tools used:
  - Apache Spark (PySpark)
  - Databricks Notebooks (Community Edition)
  - DBFS (Databricks File System)

- The dataset was enriched and prepared for:
  - **Exploratory analysis**
  - **Dashboard development**
  - **Potential fraud detection modeling**
