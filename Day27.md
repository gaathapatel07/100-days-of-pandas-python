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
