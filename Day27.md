# Day 27 — Advanced Merge, Join & Concatenation

<div align="center">

# 100 Days of Pandas

### Day 27 · Combining Multiple Datasets Efficiently

*"The most valuable business insights often emerge when multiple datasets are combined into a single analytical view."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Merge%20%26%20Join-blue)
![Day](https://img.shields.io/badge/Day-27-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Merge Matters
3. Learning Objectives
4. Understanding `merge()`
5. Types of SQL-style Joins
6. Inner Join
7. Left Join
8. Summary

---

# 1. Introduction

In real-world analytics, information is usually spread across multiple datasets.

For example, an e-commerce company may store:

* Customer information
* Product catalog
* Orders
* Payments
* Shipping details
* Employee records

Each dataset contains only part of the information.

To perform meaningful analysis, analysts combine these datasets using joins.

Pandas provides powerful tools such as:

* `merge()`
* `join()`
* `concat()`

These functions behave similarly to SQL joins.

---

# 2. Why Merge Matters

Imagine you have two datasets.

### Customers

| Customer ID | Customer |
| ----------- | -------- |
| 101         | Alice    |
| 102         | Rahul    |
| 103         | Priya    |

### Orders

| Customer ID | Sales |
| ----------- | ----: |
| 101         |  5200 |
| 102         |  6300 |
| 101         |  4100 |

Neither dataset alone provides a complete picture.

After merging:

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Alice    |  4100 |
| Rahul    |  6300 |

Now meaningful business analysis becomes possible.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Merge DataFrames.
* Understand SQL-style joins.
* Choose the correct join type.
* Handle duplicate keys.
* Build integrated analytical datasets.

---

# 4. Understanding `merge()`

The basic syntax is:

```python id="merge01"
pd.merge(
    left_df,
    right_df,
    on="Customer ID"
)
```

Parameters:

| Parameter | Purpose         |
| --------- | --------------- |
| `left`    | Left DataFrame  |
| `right`   | Right DataFrame |
| `on`      | Common column   |
| `how`     | Join type       |

---

# Example Data

Customers

| Customer ID | Name  |
| ----------- | ----- |
| 101         | Alice |
| 102         | Rahul |
| 103         | Priya |

Orders

| Customer ID | Sales |
| ----------- | ----: |
| 101         |  5200 |
| 102         |  6400 |
| 101         |  4800 |

Merge:

```python id="merge02"
merged = pd.merge(
    customers,
    orders,
    on="Customer ID"
)
```

Output

| Customer ID | Name  | Sales |
| ----------- | ----- | ----: |
| 101         | Alice |  5200 |
| 101         | Alice |  4800 |
| 102         | Rahul |  6400 |

---

# 5. Types of Joins

Pandas supports the same joins found in SQL.

| Join  | Description                |
| ----- | -------------------------- |
| Inner | Matching rows only         |
| Left  | All rows from left table   |
| Right | All rows from right table  |
| Outer | Every row from both tables |
| Cross | Cartesian Product          |

Choosing the correct join depends on the business question.

---

# 6. Inner Join

An Inner Join keeps only matching records.

Example:

Customers

| Customer ID | Name  |
| ----------- | ----- |
| 101         | Alice |
| 102         | Rahul |
| 103         | Priya |

Orders

| Customer ID | Sales |
| ----------- | ----: |
| 101         |  5200 |
| 102         |  6400 |
| 104         |  7100 |

```python id="inner01"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="inner"
)
```

Output

| Customer ID | Name  | Sales |
| ----------- | ----- | ----: |
| 101         | Alice |  5200 |
| 102         | Rahul |  6400 |

Customer **103** has no order.

Customer **104** does not exist.

Both are excluded.

---

# 7. Left Join

A Left Join keeps **every record from the left DataFrame**.

```python id="left01"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="left"
)
```

Output

| Customer ID | Name  | Sales |
| ----------- | ----- | ----: |
| 101         | Alice |  5200 |
| 102         | Rahul |  6400 |
| 103         | Priya |   NaN |

Every customer is retained.

Missing order information becomes `NaN`.

Left joins are among the most frequently used joins in business analytics because they preserve the primary dataset while enriching it with additional information.

---

# Business Example

An HR department maintains two datasets:

Employees

* Employee ID
* Name
* Department

Payroll

* Employee ID
* Salary

Using an Inner Join, analysts can retrieve only employees with payroll records.

Using a Left Join, analysts can identify employees who have not yet been assigned a salary.

---

# Best Practices

✔ Verify that key columns contain the same data type before merging.

✔ Use meaningful primary keys whenever possible.

✔ Inspect the merged result for unexpected duplicates.

✔ Validate row counts before and after joins.

✔ Remove unnecessary columns before merging large datasets.

---

# Common Mistakes

### Merging Columns with Different Data Types

Incorrect:

```python id="mistake01"
customers["Customer ID"].dtype

# int64

orders["Customer ID"].dtype

# object
```

Always align data types before merging.

```python id="mistake02"
orders["Customer ID"] = (
    orders["Customer ID"]
      .astype(int)
)
```

---

### Assuming Every Merge Is One-to-One

One customer may have:

* Multiple orders
* Multiple payments
* Multiple shipments

Understand the relationship before merging.

---

# Key Takeaways

After completing this section, you should understand:

* Why datasets need to be merged.
* How `merge()` works.
* The purpose of Inner and Left joins.
* How SQL joins map to Pandas.
* Why validating keys is important.

> **"Effective data analysis depends on connecting related datasets into a single, trustworthy source of information."**

# 8. Right Join

A **Right Join** keeps **every record from the right DataFrame**.

### Example

Customers

| Customer ID | Name  |
| ----------- | ----- |
| 101         | Alice |
| 102         | Rahul |
| 103         | Priya |

Orders

| Customer ID | Sales |
| ----------- | ----: |
| 101         |  5200 |
| 102         |  6400 |
| 104         |  7100 |

```python id="right01"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="right"
)
```

Output

| Customer ID | Name  | Sales |
| ----------- | ----- | ----: |
| 101         | Alice |  5200 |
| 102         | Rahul |  6400 |
| 104         | NaN   |  7100 |

Customer **104** appears because it exists in the Orders table.

---

# 9. Outer Join

An **Outer Join** keeps **every row from both DataFrames**.

```python id="outer01"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="outer"
)
```

Output

| Customer ID | Name  | Sales |
| ----------- | ----- | ----: |
| 101         | Alice |  5200 |
| 102         | Rahul |  6400 |
| 103         | Priya |   NaN |
| 104         | NaN   |  7100 |

Outer joins are useful for identifying missing relationships.

---

# 10. Cross Join

A **Cross Join** creates the Cartesian Product.

Every row from the first DataFrame is matched with every row from the second DataFrame.

Example

Products

| Product |
| ------- |
| Laptop  |
| Phone   |

Colors

| Color  |
| ------ |
| Black  |
| Silver |

```python id="cross01"
pd.merge(
    products,
    colors,
    how="cross"
)
```

Output

| Product | Color  |
| ------- | ------ |
| Laptop  | Black  |
| Laptop  | Silver |
| Phone   | Black  |
| Phone   | Silver |

Use cases:

* Product combinations
* Pricing simulations
* Inventory planning

---

# 11. Joining on Different Column Names

Sometimes the key columns have different names.

Customers

| Customer_ID | Name  |
| ----------- | ----- |
| 101         | Alice |

Orders

| Client_ID | Sales |
| --------- | ----: |
| 101       |  5200 |

Use:

```python id="merge03"
pd.merge(
    customers,
    orders,
    left_on="Customer_ID",
    right_on="Client_ID"
)
```

This joins columns with different names.

---

# 12. Joining Using Indexes

Merge using DataFrame indexes instead of columns.

```python id="index01"
pd.merge(
    df1,
    df2,
    left_index=True,
    right_index=True
)
```

Useful when indexes represent unique identifiers.

---

# 13. Merge Indicators

Sometimes analysts need to know where each row originated.

```python id="indicator01"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="outer",
    indicator=True
)
```

Output

| Customer ID | Name  | Sales | _merge     |
| ----------- | ----- | ----: | ---------- |
| 101         | Alice |  5200 | both       |
| 103         | Priya |   NaN | left_only  |
| 104         | NaN   |  7100 | right_only |

Indicator values:

| Value      | Meaning              |
| ---------- | -------------------- |
| both       | Found in both tables |
| left_only  | Only in left table   |
| right_only | Only in right table  |

This is extremely useful for data validation.

---

# 14. Handling Duplicate Column Names

Suppose both tables contain a column named **City**.

Pandas automatically creates suffixes.

```python id="suffix01"
pd.merge(
    customers,
    orders,
    on="Customer ID"
)
```

Output

```text id="suffix02"
City_x

City_y
```

Provide meaningful suffixes.

```python id="suffix03"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    suffixes=(
        "_Customer",
        "_Order"
    )
)
```

Output

```text id="suffix04"
City_Customer

City_Order
```

---

# 15. Merge Validation

Large datasets may contain unexpected duplicate keys.

Use `validate=` to verify assumptions.

### One-to-One

```python id="validate01"
pd.merge(
    customers,
    profile,
    on="Customer ID",
    validate="one_to_one"
)
```

---

### One-to-Many

```python id="validate02"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    validate="one_to_many"
)
```

Other options:

| Validation     | Meaning                       |
| -------------- | ----------------------------- |
| `one_to_one`   | Unique keys in both tables    |
| `one_to_many`  | Left unique, right duplicates |
| `many_to_one`  | Right unique, left duplicates |
| `many_to_many` | Duplicates allowed in both    |

Validation helps detect data quality issues early.

---

# Business Example

A telecom company maintains:

**Customers**

* Customer ID
* Name
* City

**Subscriptions**

* Customer ID
* Plan

**Payments**

* Customer ID
* Amount

Analysts:

* Merge customer and subscription tables.
* Identify customers without subscriptions.
* Find payments without customer records.
* Validate that customer IDs remain unique.

This ensures accurate billing and reporting.

---

# Best Practices

✔ Choose the correct join type for the business problem.

✔ Validate key uniqueness before merging.

✔ Use merge indicators when reconciling datasets.

✔ Rename duplicate columns using descriptive suffixes.

✔ Check row counts before and after merging.

---

# Common Mistakes

### Accidentally Creating a Many-to-Many Merge

If both tables contain duplicate keys, the merged result can grow unexpectedly.

Example:

Customers

| Customer ID |
| ----------- |
| 101         |
| 101         |

Orders

| Customer ID |
| ----------- |
| 101         |
| 101         |

The merge creates **4 rows**, not 2.

Always inspect duplicate keys before merging.

---

### Ignoring Merge Indicators

When performing reconciliation tasks, adding `indicator=True` can quickly reveal missing or unmatched records.

---

# Quick Recap

You have now learned how to:

* Perform Right Joins.
* Perform Outer Joins.
* Create Cross Joins.
* Merge on different column names.
* Merge using indexes.
* Use merge indicators.
* Handle duplicate column names.
* Validate merge relationships.

> **"Successful data integration depends not only on combining datasets, but also on validating relationships and preserving data integrity."**

# 16. Understanding `concat()`

While `merge()` combines DataFrames using **common keys**, `concat()` simply **stacks DataFrames together**.

There are two primary ways to concatenate:

* Vertically (`axis=0`)
* Horizontally (`axis=1`)

---

# Vertical Concatenation (Default)

Suppose monthly sales are stored separately.

### January

| Order ID | Sales |
| -------- | ----: |
| 101      |  5000 |
| 102      |  6200 |

### February

| Order ID | Sales |
| -------- | ----: |
| 103      |  7100 |
| 104      |  8300 |

Combine them.

```python id="concat01"
combined = pd.concat(
    [
        january,
        february
    ],
    ignore_index=True
)
```

Output

| Order ID | Sales |
| -------- | ----: |
| 101      |  5000 |
| 102      |  6200 |
| 103      |  7100 |
| 104      |  8300 |

---

# Horizontal Concatenation

Combine columns instead of rows.

```python id="concat02"
pd.concat(
    [
        customer_df,
        sales_df
    ],
    axis=1
)
```

Output

| Customer | Sales |
| -------- | ----: |
| Alice    |  5200 |
| Rahul    |  6100 |

Horizontal concatenation joins DataFrames based on their indexes.

---

# 17. Understanding `join()`

`join()` is a convenient method for combining DataFrames using their indexes.

Suppose both DataFrames have **Customer ID** as the index.

```python id="join01"
customers.join(
    orders,
    how="left"
)
```

This is equivalent to a merge using indexes.

---

## Join on a Column

```python id="join02"
customers.join(
    orders.set_index("Customer ID"),
    on="Customer ID"
)
```

`join()` is concise when indexes are already configured appropriately.

---

# 18. `merge()` vs `join()` vs `concat()`

Choosing the correct function is important.

| Function   | Purpose                   | Common Use Case        |
| ---------- | ------------------------- | ---------------------- |
| `merge()`  | Combine using common keys | SQL-style joins        |
| `join()`   | Combine using indexes     | Indexed DataFrames     |
| `concat()` | Stack DataFrames          | Monthly files, reports |

---

## Visual Comparison

### `merge()`

```text id="mergevisual"
Customers
      +
Orders
      │
      ▼
Merged Dataset
```

---

### `concat()`

```text id="concatvisual"
January
February
March
April
   │
   ▼
Combined Dataset
```

---

### `join()`

```text id="joinvisual"
Index
 │
 ▼
Aligned DataFrames
```

---

# 19. Enterprise ETL Workflow

A multinational retailer receives:

* Customer database
* Sales database
* Product database
* Inventory reports
* Shipping information

Typical workflow:

```text id="etl02"
CSV Files
Excel Files
SQL Tables
API Responses
      │
      ▼
Import
      │
      ▼
Merge Customer Data
      │
      ▼
Join Inventory
      │
      ▼
Concatenate Monthly Reports
      │
      ▼
Clean Data
      │
      ▼
Generate Dashboard Dataset
```

This represents a typical ETL process in enterprise analytics.

---

# 20. Merge Performance Optimization

Large datasets require efficient merging.

### Ensure Matching Data Types

```python id="perf_merge01"
customers["Customer ID"] = (
    customers["Customer ID"]
      .astype(int)
)

orders["Customer ID"] = (
    orders["Customer ID"]
      .astype(int)
)
```

---

### Merge Only Required Columns

Instead of:

```python id="perf_merge02"
pd.merge(
    customers,
    orders
)
```

Prefer:

```python id="perf_merge03"
pd.merge(
    customers[
        ["Customer ID", "Name"]
    ],
    orders[
        ["Customer ID", "Sales"]
    ],
    on="Customer ID"
)
```

Reducing unnecessary columns improves performance and memory usage.

---

### Validate Before Exporting

```python id="perf_merge04"
merged.info()
```

```python id="perf_merge05"
merged.isnull().sum()
```

Always inspect the merged dataset before using it for reporting or machine learning.

---

# 21. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**.

Data arrives from different departments:

### Customers

* Customer ID
* Name
* City

### Orders

* Order ID
* Customer ID
* Sales

### Products

* Product ID
* Category

### Monthly Sales

* January
* February
* March

Your task is to build a single analytics-ready dataset.

---

## Business Questions

### Question 1

Merge customer and order information.

```python id="case_merge01"
customer_orders = pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="left"
)
```

---

### Question 2

Join product information.

```python id="case_merge02"
complete = pd.merge(
    customer_orders,
    products,
    on="Product ID",
    how="left"
)
```

---

### Question 3

Combine monthly sales reports.

```python id="case_merge03"
sales = pd.concat(
    [
        january,
        february,
        march
    ],
    ignore_index=True
)
```

---

### Question 4

Identify unmatched customers.

```python id="case_merge04"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="outer",
    indicator=True
)
```

---

### Question 5

Validate merge relationships.

```python id="case_merge05"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    validate="one_to_many"
)
```

---

# 22. Business Insights

After integrating multiple datasets, analysts discover:

* Several customers have never placed an order.
* Certain orders reference invalid customer IDs.
* Monthly reports can be consolidated automatically using `concat()`.
* Merge validation identifies unexpected duplicate keys.
* Combining customer, product, and sales data enables comprehensive business dashboards.

---

# 23. Practice Exercises

## Beginner

1. Perform an Inner Join.
2. Perform a Left Join.
3. Perform a Right Join.
4. Perform an Outer Join.
5. Concatenate two DataFrames vertically.

---

## Intermediate

6. Merge on different column names.
7. Merge using indexes.
8. Create a Cross Join.
9. Use merge indicators.
10. Join multiple monthly datasets.

---

## Advanced

11. Build an enterprise ETL pipeline.
12. Validate merge relationships.
13. Optimize merge performance.
14. Detect unmatched records.
15. Design a multi-table reporting workflow.

---

# 24. Interview Questions

## Beginner

1. What is `merge()`?
2. Difference between `merge()` and `concat()`?
3. Difference between Inner and Left Join?
4. What is `join()`?
5. What is a Cross Join?

---

## Intermediate

6. Difference between Left and Right Join?
7. Why use `indicator=True`?
8. What does `validate=` do?
9. How do you merge on different column names?
10. When should `concat()` be used?

---

## Advanced

11. Explain a complete ETL workflow using Pandas.
12. Compare `merge()`, `join()`, and `concat()`.
13. How would you optimize joins for a dataset containing 100 million rows?
14. How do you identify duplicate key problems?
15. How do joins support business intelligence reporting?

---

# 25. Cheat Sheet

| Task             | Syntax             |
| ---------------- | ------------------ |
| Merge            | `pd.merge()`       |
| Inner Join       | `how="inner"`      |
| Left Join        | `how="left"`       |
| Right Join       | `how="right"`      |
| Outer Join       | `how="outer"`      |
| Cross Join       | `how="cross"`      |
| Merge Indicator  | `indicator=True`   |
| Merge Validation | `validate=`        |
| Join             | `DataFrame.join()` |
| Concatenate      | `pd.concat()`      |

---

# 26. Mini Project

## Customer 360 Analytics Pipeline

Using any retail, banking, healthcare, HR, or e-commerce dataset:

Complete the following tasks:

* Merge customer and transaction data.
* Join product information.
* Concatenate monthly sales reports.
* Validate merge relationships.
* Identify unmatched records using merge indicators.
* Remove duplicate keys.
* Export the integrated dataset.
* Write **five executive-level business insights**.
* Recommend **three improvements** for improving data integration and quality.

### Example Business Insights

* A portion of customers had no associated transactions, highlighting opportunities for targeted marketing.
* Merge validation uncovered duplicate customer IDs that required data quality improvements.
* Consolidating monthly reports reduced manual reporting effort.
* Product-level joins enabled profitability analysis by category.
* Unified datasets improved dashboard accuracy and decision-making.

---

# 27. Summary

Congratulations! 🎉

Today you mastered **Merge, Join & Concatenation** in Pandas.

You learned how to:

* Perform SQL-style joins using `merge()`.
* Use Inner, Left, Right, Outer, and Cross Joins.
* Merge on different column names.
* Join DataFrames using indexes.
* Concatenate DataFrames vertically and horizontally.
* Validate merge relationships.
* Build enterprise-ready data integration pipelines.

These techniques are essential for SQL, Power BI, Tableau, ETL pipelines, and modern data engineering workflows.

---

# 28. What's Next?

In **Day 28**, you'll learn **MultiIndex, Hierarchical Indexing & Advanced Index Operations**.

Topics include:

* Creating MultiIndex
* Hierarchical Rows & Columns
* `set_index()`
* `reset_index()`
* `swaplevel()`
* `sort_index()`
* `stack()`
* `unstack()`
* Advanced Index Slicing
* Performance Benefits of Indexing

Mastering MultiIndex will allow you to work efficiently with complex, multi-dimensional datasets and perform advanced analytical operations.

---

<div align="center">

# Day 27 Complete!

You've mastered one of the most essential skills in Pandas—combining data from multiple sources using **merge**, **join**, and **concat**.

These techniques form the foundation of real-world ETL pipelines, SQL-style data integration, and business intelligence reporting.

</div>
