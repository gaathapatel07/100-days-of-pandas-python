# 🐼 Day 10 — Merging, Joining & Concatenating DataFrames

<div align="center">

# 100 Days of Pandas

### Day 10 · Combining Multiple Datasets Like a Data Analyst

*"The most valuable insights often emerge when data from different sources is brought together."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-DataFrame%20Merging-blue)
![Day](https://img.shields.io/badge/Day-10-orange)

</div>

---

# 📚 Table of Contents

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

