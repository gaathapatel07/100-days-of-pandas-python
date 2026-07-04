# 🐼 Day 26 — Advanced GroupBy, Aggregation & Pivot Analysis

<div align="center">

# 100 Days of Pandas

### Day 26 · Summarizing Data Like a Data Analyst

*"Raw data becomes valuable only when it is summarized into meaningful business insights."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-GroupBy%20%26%20Aggregation-blue)
![Day](https://img.shields.io/badge/Day-26-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. Why GroupBy Matters
3. Learning Objectives
4. Understanding GroupBy
5. Basic Aggregations
6. Multiple Aggregations
7. Grouping by Multiple Columns
8. Summary

---

# 1. Introduction

One of the most common tasks in data analysis is summarizing large datasets into meaningful information.

Instead of examining millions of rows individually, analysts group similar records and calculate statistics.

Examples include:

* Total sales by city
* Average salary by department
* Monthly revenue by region
* Customer count by country
* Profit by product category

Pandas provides the powerful **GroupBy** functionality to perform these operations efficiently.

---

# 2. Why GroupBy Matters

Imagine an online retailer with one million transactions.

Management asks:

* Which city generates the highest revenue?
* Which department has the highest average salary?
* Which product category earns the greatest profit?
* Which payment method is most popular?

Without grouping, answering these questions would require extensive manual calculations.

GroupBy automates these summaries in just a few lines of code.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Group data by one or more columns.
* Calculate summary statistics.
* Apply multiple aggregation functions.
* Analyze grouped datasets.
* Prepare business reports efficiently.

---

# 4. Understanding GroupBy

The basic syntax is:

```python id="group01"
df.groupby("Department")
```

This creates a **GroupBy object**.

No calculations occur until an aggregation function is applied.

---

## Example Dataset

| Department | Salary |
| ---------- | -----: |
| IT         |  65000 |
| HR         |  52000 |
| IT         |  72000 |
| Finance    |  68000 |
| HR         |  55000 |

Calculate the average salary.

```python id="group02"
df.groupby(
    "Department"
)["Salary"].mean()
```

Output:

| Department | Average Salary |
| ---------- | -------------: |
| Finance    |          68000 |
| HR         |          53500 |
| IT         |          68500 |

---

# 5. Basic Aggregations

The most frequently used aggregation functions include:

| Function   | Description        |
| ---------- | ------------------ |
| `sum()`    | Total              |
| `mean()`   | Average            |
| `count()`  | Number of records  |
| `min()`    | Smallest value     |
| `max()`    | Largest value      |
| `median()` | Middle value       |
| `std()`    | Standard deviation |
| `var()`    | Variance           |

---

## Sum

```python id="agg01"
df.groupby(
    "City"
)["Sales"].sum()
```

---

## Average

```python id="agg02"
df.groupby(
    "City"
)["Sales"].mean()
```

---

## Count

```python id="agg03"
df.groupby(
    "City"
)["Sales"].count()
```

---

## Maximum

```python id="agg04"
df.groupby(
    "City"
)["Sales"].max()
```

---

## Minimum

```python id="agg05"
df.groupby(
    "City"
)["Sales"].min()
```

---

# 6. Multiple Aggregations

Business reports often require several statistics simultaneously.

Example:

```python id="agg06"
df.groupby(
    "Department"
)["Salary"].agg(
    [
        "count",
        "mean",
        "min",
        "max",
        "sum"
    ]
)
```

Output:

| Department | Count |  Mean |   Min |   Max |    Sum |
| ---------- | ----: | ----: | ----: | ----: | -----: |
| Finance    |     5 | 68000 | 60000 | 75000 | 340000 |
| HR         |     8 | 53000 | 45000 | 61000 | 424000 |
| IT         |    12 | 72000 | 58000 | 94000 | 864000 |

This provides a complete statistical summary.

---

# 7. Grouping by Multiple Columns

Sometimes one grouping variable is not enough.

Example:

Calculate sales by both **City** and **Product Category**.

```python id="multi01"
df.groupby(
    [
        "City",
        "Category"
    ]
)["Sales"].sum()
```

Example Output:

| City   | Category    | Sales |
| ------ | ----------- | ----: |
| Delhi  | Electronics | 85000 |
| Delhi  | Furniture   | 32000 |
| Mumbai | Electronics | 91000 |
| Mumbai | Furniture   | 40000 |

This creates a hierarchical (MultiIndex) result.

---

## Resetting the Index

Convert grouped output into a regular DataFrame.

```python id="multi02"
(
    df.groupby(
        [
            "City",
            "Category"
        ]
    )["Sales"]
    .sum()
    .reset_index()
)
```

This is useful before exporting data or creating visualizations.

---

# Business Example

A retail chain wants to compare sales across different cities and product categories.

Analysts group transactions by:

* City
* Product Category

Then calculate:

* Total Sales
* Average Sales
* Number of Orders

This allows management to identify top-performing regions and product lines.

---

# Best Practices

✔ Group only the columns needed for analysis.

✔ Use multiple aggregations to reduce repeated calculations.

✔ Reset the index before exporting grouped results.

✔ Use meaningful column names after aggregation.

✔ Validate grouped results against the original dataset.

---

# Common Mistakes

### Forgetting Aggregation

Incorrect:

```python id="mistake01"
df.groupby("City")
```

This only creates a GroupBy object.

Always follow it with an aggregation function such as:

```python id="mistake02"
.sum()
.mean()
.count()
```

---

### Grouping Too Many Columns

Grouping by many high-cardinality columns may create thousands of groups, making results difficult to interpret.

Choose grouping variables that answer a clear business question.

---

# Key Takeaways

After completing this section, you should understand:

* How `groupby()` works.
* How to calculate common summary statistics.
* How to apply multiple aggregation functions.
* How to group by multiple columns.
* How grouped summaries support business decision-making.

> **"GroupBy transforms millions of individual records into concise summaries that reveal trends, performance, and actionable business insights."**

