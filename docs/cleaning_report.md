# Data Cleaning Report for Olist Capstone Project

This document explains the data cleaning steps performed to prepare the **Olist Brazilian E-Commerce dataset** for analysis and visualization.  
All steps ensure the dataset is **accurate, consistent, and analysis-ready**.

---

## 1. Missing Values

- **Problem:** Some tables have missing or null values (e.g., missing timestamps, missing review scores, missing geolocation).  
- **Action Taken:**
  - Orders with **missing delivery timestamps** were excluded because delivery time cannot be calculated without timestamps.  
  - **Review scores** were retained as nulls for orders without reviews.  
    - Rationale: Excluding orders without reviews could bias delivery-performance analysis, since not all orders are reviewed.  
  - Missing values in **optional customer or seller fields** were left as nulls but flagged for awareness.  

---

## 2. Duplicate Records

- **Problem:** Some tables might contain duplicate rows or repeated IDs.  
- **Action Taken:**
  - Verified that **primary keys** (`order_id`, `product_id`, `seller_id`, `customer_id`) are unique.  
  - Removed any exact row duplicates.  
  - Ensured no duplicates would inflate revenue, delivery metrics, or review aggregation.  

---

## 3. Outliers

- **Problem:** Extreme values in numeric columns (e.g., price, freight, delivery days) can skew analysis.  
- **Action Taken:**
  - Capped **price and freight values** at the 99th percentile to reduce the effect of outliers.  
  - Flagged **delivery days** that were extremely high (e.g., hundreds of days) and reviewed for correctness.  
- **Rationale:** Outlier handling ensures trends and patterns reflect typical customer experience, not rare anomalies.  

---

## 4. Data Type Conversions

- **Problem:** Some columns are in the wrong data type (e.g., timestamps stored as strings).  
- **Action Taken:**
  - Converted all **timestamp columns** (order purchase, estimated delivery, actual delivery) to `datetime` format.  
  - Ensured numeric fields (`price`, `freight_value`, `review_score`) are of correct numeric types for aggregation.  
- **Rationale:** Correct data types are essential for accurate calculations, grouping, and visualizations in Python and Power BI.  

---

## 5. Aggregation of Multi-Item Orders

- **Problem:** Some orders contain multiple items.  
- **Action Taken:**  
  - Aggregated revenue, freight, and item-level details at the **order_id level** when required.  
  - Prevents **double-counting revenue** or inflating delivery metrics.  

---

## 6. Missing / Incorrect Joins

- Ensured that all joins (orders ⟶ order_items, orders ⟶ reviews, etc.) are consistent with the **join strategy** documented in `joins_justification.md`.  
- Flagged any unmatched keys for investigation.  

---

## 7. Summary of Cleaning Actions

| Cleaning Step                  | Action Taken / Rationale                                         |
|--------------------------------|-----------------------------------------------------------------|
| Missing timestamps             | Excluded incomplete deliveries                                   |
| Missing review scores          | Retained nulls for unbiased delivery analysis                   |
| Duplicates                     | Validated uniqueness of primary keys, removed exact duplicates |
| Outliers (price, freight)      | Capped at 99th percentile                                        |
| Date/time fields               | Converted to datetime                                           |
| Multi-item orders              | Aggregated at order level to avoid double-counting              |
| Joins                          | Ensured consistency and flagged unmatched keys                  |

---

**Note:**  
This cleaning report will be updated as additional anomalies or data issues are discovered during EDA or dashboard building.  

---

**Outcome:**  
After applying these steps, we now have a **clean, analysis-ready master dataset** (`olist_master_cleaned.csv`) suitable for Python analysis and Power BI visualization.
