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

