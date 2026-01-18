# Joins Justification for Olist Capstone Project

When working with the Olist Brazilian E-Commerce dataset, data is split across multiple relational tables (orders, order_items, order_reviews, products, sellers, customers, etc.).  
To answer the analytical questions and build a master dataset, we need to **combine information from multiple tables**, a process known as **joining tables**.  

---

## What is a Join?

A **join** is a way to merge two tables based on a common key (column).  

- Example: `order_id` is present in both `orders` and `order_items`. We can join on `order_id` to combine order info with the items purchased.  
- Different types of joins determine which rows are kept in the result.

---

## Types of Joins Used

1. **Inner Join**  
   - Keeps only rows that exist in **both tables**.  
   - Example: `orders ⟶ order_items`  
     - Only orders that have items are kept.  
     - Important for revenue calculations — orders without items cannot generate revenue.  

2. **Left Join**  
   - Keeps **all rows from the first table**, and brings in matching rows from the second table if they exist.  
   - Fills missing matches with `NULL` if there’s no corresponding row in the second table.  
   - Example: `orders ⟶ order_reviews`  
     - Some orders have no review yet. Using a left join keeps **all orders** so that delivery analysis is complete.  

---

## Join Strategy for This Project

- **orders ⟶ order_items: inner join**  
  - Keeps only orders that actually contain items.  
  - Needed for **revenue calculation**, since orders without items have no revenue.  

- **orders ⟶ order_reviews: left join**  
  - Keeps all orders even if no review exists.  
  - Ensures analysis of delivery performance is unbiased.  

- **orders ⟶ customers: left join**  
  - Ensures all orders are kept even if some customer data is missing.  
  - Necessary to link orders to region or state for geographic analysis.  

- **products ⟶ order_items: left join**  
  - Attaches product-level information (category, price) to items.  

- **sellers ⟶ order_items: left join**  
  - Attaches seller info for each item to analyze seller-level performance.  

---

## Notes on Multi-Item Orders

- Some orders contain multiple items.  
- To avoid **counting revenue or items multiple times**, metrics are **aggregated at the order level** when necessary.  
  - Example: sum all item prices + freight for an order to get **total order revenue**.  

---

## Why This Documentation Matters

1. Ensures anyone reading the repo understands **why we joined tables in this specific way**.  
2. Justifies decisions that affect the final dataset and downstream analysis.  
3. Prevents misinterpretation of metrics by future team members or graders.  
4. Makes the workflow reproducible and academically rigorous.  

---

With this strategy, we create a **clean, analysis-ready master dataset** that can be used in Python notebooks for EDA and then exported to Power BI for visualization.
