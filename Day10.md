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

# 15. Understanding `join()`

While `merge()` is the most flexible method for combining DataFrames, Pandas also provides the `join()` function.

The primary difference is that `join()` combines DataFrames **based on their indexes**, whereas `merge()` typically combines them using one or more columns.

### Example

Suppose two DataFrames use **Customer ID** as their index.

**Customer Details**

| Customer ID | Customer Name |
| ----------: | ------------- |
|         101 | Alice         |
|         102 | Rahul         |
|         103 | Emma          |

**Customer Orders**

| Customer ID | Sales |
| ----------: | ----: |
|         101 |  2500 |
|         103 |  4200 |

```python id="1a9kzq"
customers.join(
    orders,
    how="left"
)
```

Output:

| Customer ID | Customer Name | Sales |
| ----------: | ------------- | ----: |
|         101 | Alice         |  2500 |
|         102 | Rahul         |   NaN |
|         103 | Emma          |  4200 |

Use `join()` when indexes already represent the relationship between datasets.

---

# 16. Understanding `concat()`

Unlike `merge()` and `join()`, the `concat()` function **does not match records using keys**.

Instead, it simply combines DataFrames either **vertically (row-wise)** or **horizontally (column-wise)**.

---

## Vertical Concatenation

Suppose two regional sales reports need to be combined.

### North Region

| Order ID | Sales |
| -------- | ----: |
| 101      |  2500 |
| 102      |  1800 |

### South Region

| Order ID | Sales |
| -------- | ----: |
| 103      |  3100 |
| 104      |  2700 |

```python id="7m2cqp"
pd.concat(
    [north, south]
)
```

Output:

| Order ID | Sales |
| -------- | ----: |
| 101      |  2500 |
| 102      |  1800 |
| 103      |  3100 |
| 104      |  2700 |

This is useful when combining datasets that have the same structure.

---

## Horizontal Concatenation

```python id="8y4gwr"
pd.concat(
    [customer_details, customer_scores],
    axis=1
)
```

Here, the DataFrames are placed side by side.

---

# 17. `merge()` vs `join()` vs `concat()`

Understanding when to use each function is an essential interview topic.

| Function   | Purpose                      | Common Use Case                 |
| ---------- | ---------------------------- | ------------------------------- |
| `merge()`  | Combine using common columns | SQL-style joins                 |
| `join()`   | Combine using indexes        | Indexed DataFrames              |
| `concat()` | Stack DataFrames             | Monthly reports, multiple files |

---

# 18. Real-World Business Case Study

## Scenario

You are a Data Analyst at **RetailHub**, where customer information, orders, payments, and shipment details are stored in separate databases.

Your task is to prepare a unified dataset for the Business Intelligence team.

### Available Tables

**Customers**

* Customer ID
* Customer Name
* Region

**Orders**

* Order ID
* Customer ID
* Product
* Sales

**Payments**

* Order ID
* Payment Method
* Payment Status

**Shipments**

* Order ID
* Delivery Date
* Shipping Cost

---

### Tasks

1. Merge customer and order information.
2. Merge payment details using Order ID.
3. Merge shipment information.
4. Identify customers without orders.
5. Detect orders missing payment records.
6. Create one master dataset for reporting.

This mirrors how analysts build datasets before creating dashboards or reports.

---

# 19. Practice Exercises

## Beginner

1. Perform an Inner Join on two DataFrames.
2. Perform a Left Join.
3. Perform a Right Join.
4. Perform a Full Outer Join.
5. Merge using Customer ID.

---

## Intermediate

6. Merge using two columns.
7. Add custom suffixes.
8. Use `indicator=True`.
9. Join two indexed DataFrames.
10. Concatenate two monthly sales reports.

---

## Advanced

11. Merge customer, order, and payment tables into one dataset.
12. Detect records that exist only in one table.
13. Compare `merge()` and `join()` using the same data.
14. Create a complete master DataFrame from multiple sources.
15. Write five business insights after merging the datasets.

---

# 20. Interview Questions

## Beginner

1. What is the purpose of `merge()`?
2. Difference between Inner Join and Left Join?
3. What is a common key?
4. What does `concat()` do?
5. When should you use `join()`?

---

## Intermediate

6. Difference between `merge()` and `join()`?
7. Difference between `merge()` and `concat()`?
8. Why are suffixes useful?
9. What does the `_merge` column indicate?
10. Why must key columns have matching data types?

---

## Advanced

11. Explain the four join types with examples.
12. Describe a real-world scenario where multiple DataFrames must be merged.
13. How would you validate a merged dataset?
14. What problems arise from duplicate keys?
15. How does `merge()` in Pandas compare with SQL joins?

---

# 21. Cheat Sheet

| Operation                 | Syntax                            |
| ------------------------- | --------------------------------- |
| Inner Join                | `pd.merge(df1, df2, how="inner")` |
| Left Join                 | `pd.merge(df1, df2, how="left")`  |
| Right Join                | `pd.merge(df1, df2, how="right")` |
| Outer Join                | `pd.merge(df1, df2, how="outer")` |
| Merge on multiple columns | `on=["Region","Product"]`         |
| Add suffixes              | `suffixes=("_x","_y")`            |
| Merge indicator           | `indicator=True`                  |
| Join by index             | `df1.join(df2)`                   |
| Vertical concat           | `pd.concat([df1, df2])`           |
| Horizontal concat         | `pd.concat([df1, df2], axis=1)`   |

---

# 22. Mini Project

## Building a Master Sales Dataset

Using four separate CSV files:

* Customers
* Orders
* Payments
* Shipments

Perform the following:

* Import all datasets.
* Merge customers with orders.
* Merge payments using Order ID.
* Merge shipment details.
* Detect missing relationships using `indicator=True`.
* Generate a final master dataset.
* Export the dataset as a CSV file.
* Write **five business insights** from the combined data.

Example insights:

* Customers in the West region generate the highest average order value.
* Most delayed shipments occur in the South region.
* A small percentage of orders are missing payment records.
* Credit Card is the most commonly used payment method.
* Premium customers contribute a disproportionately large share of revenue.

---

# 23. Summary

Congratulations! 🎉

Today you mastered one of the most valuable skills in Pandas—**combining multiple datasets**.

You learned how to:

* Merge DataFrames using common keys.
* Perform Inner, Left, Right, and Outer Joins.
* Merge using multiple columns.
* Handle duplicate column names with suffixes.
* Validate merges using the `_merge` indicator.
* Join indexed DataFrames.
* Concatenate DataFrames vertically and horizontally.

These operations are fundamental in data engineering, business intelligence, analytics, and machine learning workflows.

---

# 24. What's Next?

In **Day 11**, you'll explore **Pivot Tables & Cross Tabulations**, where you'll learn how to summarize data in a format similar to Microsoft Excel PivotTables.

Topics include:

* `pivot()`
* `pivot_table()`
* `crosstab()`
* Multiple aggregations
* Margins and totals
* Multi-level pivot tables

These tools allow analysts to transform large datasets into concise, decision-ready summaries.

---

<div align="center">

# 🎉 Day 10 Complete!

You've reached an important milestone in your Pandas journey.

You can now integrate multiple datasets, validate relationships, and prepare unified data for analysis—skills that are expected in real-world analytics roles.

⭐ Keep learning, keep building, and keep sharing your progress.

**Next → Day 11: Pivot Tables & Cross Tabulations** 📊🐼

</div>
