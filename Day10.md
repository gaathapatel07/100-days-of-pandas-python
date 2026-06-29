# Day 10 — Merging, Joining & Concatenating DataFrames

<div align="center">

# 100 Days of Pandas

### Day 10 · Combining Multiple Datasets Like a Data Analyst

*"The most valuable insights often emerge when data from different sources is brought together."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-DataFrame%20Merging-blue)
![Day](https://img.shields.io/badge/Day-10-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Combining Data Matters
3. Learning Objectives
4. Understanding DataFrame Merging
5. SQL JOIN vs Pandas Merge
6. Types of Joins
7. Inner Join
8. Left Join
9. Summary

---

# 1. Introduction

Until now, we've worked with a single dataset at a time. However, in real-world analytics, data is rarely stored in one table.

For example, an e-commerce company may maintain separate datasets for:

* Customers
* Orders
* Products
* Payments
* Shipments

To answer meaningful business questions, analysts must combine these datasets into a unified view.

Pandas provides several powerful functions to accomplish this:

* `merge()`
* `join()`
* `concat()`

Understanding when and how to use each function is essential for working with relational data.

---

# 2. Why Combining Data Matters

Imagine you're analyzing an online retail business.

You receive two separate datasets.

### Customers

| Customer ID | Customer Name |
| ----------: | ------------- |
|         101 | Alice         |
|         102 | Rahul         |
|         103 | Emma          |
|         104 | David         |

---

### Orders

| Order ID | Customer ID | Sales |
| -------- | ----------: | ----: |
| 5001     |         101 |  2500 |
| 5002     |         103 |  4200 |
| 5003     |         104 |  1800 |
| 5004     |         101 |  3200 |

Neither dataset alone answers:

* Which customer placed each order?
* What is the total spending of each customer?
* Which customers have never placed an order?

By combining them, analysts create a richer dataset capable of answering these questions.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the purpose of merging datasets.
* Perform different types of joins.
* Differentiate between `merge()`, `join()`, and `concat()`.
* Select the appropriate join for different scenarios.
* Combine multiple datasets using common keys.

---

# 4. Understanding `merge()`

The `merge()` function combines two DataFrames using one or more common columns.

Think of it as matching related information based on a shared identifier.

### Syntax

```python id="5nyj0w"
pd.merge(
    left_dataframe,
    right_dataframe,
    on="Common Column"
)
```

The **Common Column** is often called the **key**.

Examples include:

* Customer ID
* Employee ID
* Product ID
* Order ID
* Student ID

Without a common key, Pandas cannot determine how the datasets should be combined.

---

# 5. SQL JOIN vs Pandas Merge

If you've worked with SQL, the concept will feel familiar.

| SQL             | Pandas               |
| --------------- | -------------------- |
| INNER JOIN      | `merge(how="inner")` |
| LEFT JOIN       | `merge(how="left")`  |
| RIGHT JOIN      | `merge(how="right")` |
| FULL OUTER JOIN | `merge(how="outer")` |

Although the syntax differs, the underlying concept remains the same.

---

# 6. Types of Joins

Pandas supports four primary join types.

```text id="o6hm41"
               LEFT TABLE

      A     B     C     D

INNER JOIN → Common rows only

LEFT JOIN  → All left + matching right

RIGHT JOIN → All right + matching left

OUTER JOIN → Everything from both tables
```

Choosing the correct join type depends on the business question being answered.

---

# 7. Inner Join

An **Inner Join** returns only the rows that exist in **both** DataFrames.

### Customers

| Customer ID | Name  |
| ----------: | ----- |
|         101 | Alice |
|         102 | Rahul |
|         103 | Emma  |

---

### Orders

| Customer ID | Sales |
| ----------: | ----: |
|         101 |  2500 |
|         103 |  4200 |
|         104 |  1800 |

Merge them:

```python id="pt1q5x"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="inner"
)
```

Output:

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         103 | Emma  |  4200 |

Notice:

* Rahul is excluded because he has no matching order.
* Customer 104 is excluded because no customer record exists.

Inner joins are useful when you only need complete matches.

---

# 8. Left Join

A **Left Join** keeps **all rows from the left DataFrame** and adds matching information from the right DataFrame.

```python id="79sowk"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="left"
)
```

Output:

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         102 | Rahul |   NaN |
|         103 | Emma  |  4200 |

Rahul appears because every customer from the left table is preserved.

Since Rahul has not placed an order, the Sales column contains `NaN`.

Left joins are particularly useful when identifying missing relationships, such as customers without orders or employees without assigned projects.

---

# Key Takeaways

You now understand:

* Why datasets must often be combined.
* The purpose of `merge()`.
* The concept of common keys.
* The relationship between SQL joins and Pandas merges.
* The difference between Inner Join and Left Join.

> **"Data stored in separate tables becomes valuable when meaningful relationships are established between them."**

# 9. Right Join

A **Right Join** returns **all rows from the right DataFrame** and only the matching rows from the left DataFrame.

This join is useful when the right table contains the complete set of records that you want to preserve.

### Customers

| Customer ID | Name  |
| ----------: | ----- |
|         101 | Alice |
|         102 | Rahul |
|         103 | Emma  |

---

### Orders

| Customer ID | Sales |
| ----------: | ----: |
|         101 |  2500 |
|         103 |  4200 |
|         104 |  1800 |

Perform a Right Join:

```python id="4c7yup"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="right"
)
```

### Output

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         103 | Emma  |  4200 |
|         104 | NaN   |  1800 |

Notice that **Customer 104** appears even though no customer information exists.

This allows analysts to detect incomplete or inconsistent records between datasets.

---

# 10. Full Outer Join

A **Full Outer Join** combines **all rows from both DataFrames**.

Whenever no matching record exists, Pandas fills the missing values with `NaN`.

```python id="3x5l9a"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="outer"
)
```

### Output

| Customer ID | Name  | Sales |
| ----------: | ----- | ----: |
|         101 | Alice |  2500 |
|         102 | Rahul |   NaN |
|         103 | Emma  |  4200 |
|         104 | NaN   |  1800 |

Outer joins are useful for:

* Data reconciliation
* Auditing records
* Detecting missing information
* Comparing datasets from different systems

---

# 11. Merging on Multiple Columns

Sometimes a single column is not enough to uniquely identify a record.

Suppose we have:

### Sales Dataset

| Region | Product | Sales |
| ------ | ------- | ----: |
| North  | Laptop  |  5000 |
| North  | Mouse   |   800 |
| South  | Laptop  |  6200 |

---

### Inventory Dataset

| Region | Product | Stock |
| ------ | ------- | ----: |
| North  | Laptop  |    25 |
| North  | Mouse   |   120 |
| South  | Laptop  |    18 |

Merge using both **Region** and **Product**.

```python id="l8b0jh"
pd.merge(
    sales,
    inventory,
    on=["Region", "Product"]
)
```

### Output

| Region | Product | Sales | Stock |
| ------ | ------- | ----: | ----: |
| North  | Laptop  |  5000 |    25 |
| North  | Mouse   |   800 |   120 |
| South  | Laptop  |  6200 |    18 |

Using multiple keys helps prevent incorrect matches and is common in enterprise datasets.

---

# 12. Handling Duplicate Column Names

Sometimes both DataFrames contain columns with the same name.

Example:

Both tables contain a column named **Date**.

Without specifying suffixes, Pandas automatically creates:

```text id="az3m0p"
Date_x

Date_y
```

A clearer approach is to define custom suffixes.

```python id="2j1vse"
pd.merge(
    orders,
    shipments,
    on="Order ID",
    suffixes=(
        "_Order",
        "_Shipment"
    )
)
```

Example Output:

| Order ID | Date_Order | Date_Shipment |
| -------- | ---------- | ------------- |
| 5001     | 2025-01-15 | 2025-01-18    |

Meaningful suffixes improve readability and reduce confusion.

---

# 13. Understanding the Merge Indicator

When combining datasets, it's often useful to know **where each row originated**.

Use the `indicator=True` parameter.

```python id="3m7djq"
pd.merge(
    customers,
    orders,
    on="Customer ID",
    how="outer",
    indicator=True
)
```

### Output

| Customer ID | Name  | Sales | _merge     |
| ----------: | ----- | ----: | ---------- |
|         101 | Alice |  2500 | both       |
|         102 | Rahul |   NaN | left_only  |
|         104 | NaN   |  1800 | right_only |

The `_merge` column tells you:

| Value        | Meaning                            |
| ------------ | ---------------------------------- |
| `both`       | Record exists in both tables       |
| `left_only`  | Exists only in the left DataFrame  |
| `right_only` | Exists only in the right DataFrame |

This feature is extremely useful for debugging and validating data.

---

# 14. Business Example

Imagine an online retailer stores customer details in one database and order information in another.

Your tasks include:

* Identifying customers who have never placed an order.
* Finding orders that do not have a corresponding customer record.
* Comparing inventory with sales records.
* Reconciling data from multiple departments.

Each of these tasks relies on choosing the correct join type.

---

# Best Practices

✔ Always identify the correct join key before merging.

✔ Ensure key columns have matching data types.

✔ Use meaningful suffixes for duplicate column names.

✔ Validate merged datasets using `indicator=True`.

✔ Check for duplicate keys before merging to avoid unexpected results.

---

# Common Mistakes

### Merging Different Data Types

```python id="7f9k2r"
customers["Customer ID"]
```

might be `int64`, while

```python id="8t5m1n"
orders["Customer ID"]
```

might be `object`.

This can cause failed merges or unexpected results.

Always verify:

```python id="1p8d6z"
customers.dtypes

orders.dtypes
```

---

### Using the Wrong Join Type

Using an Inner Join when a Left Join is required may unintentionally remove important records.

Always choose the join type based on the business question rather than convenience.

---

# Quick Recap

You have now learned how to:

* Perform Right Joins.
* Perform Full Outer Joins.
* Merge using multiple columns.
* Handle duplicate column names with suffixes.
* Validate merges using the `_merge` indicator.

> **"Successful data integration depends not only on combining tables, but on choosing the correct join strategy to preserve meaningful information."**
