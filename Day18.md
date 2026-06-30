# Day 18 — Advanced GroupBy Operations & Aggregations

<div align="center">

# 100 Days of Pandas

### Day 18 · Mastering GroupBy for Business Analytics

*"Raw data becomes valuable only after it is summarized into meaningful business insights."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Advanced%20GroupBy-blue)
![Day](https://img.shields.io/badge/Day-18-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why GroupBy Matters
3. Learning Objectives
4. Understanding GroupBy
5. Basic Aggregations
6. Multiple Aggregations
7. Named Aggregations
8. Summary

---

# 1. Introduction

In almost every business, raw transactional data contains thousands or even millions of records.

Managers rarely want to inspect individual transactions. Instead, they ask summary-based questions such as:

* Which region generated the highest revenue?
* Which department has the highest average salary?
* Which product category earns the greatest profit?
* Which customer segment places the most orders?

Answering these questions requires grouping similar records together and calculating summary statistics.

The **GroupBy** operation is one of the most powerful features in Pandas because it allows analysts to split data into groups, apply calculations to each group, and combine the results into meaningful reports.

---

# 2. Why GroupBy Matters

Imagine an e-commerce company has recorded **2 million customer orders**.

Each transaction contains:

| Order ID | Region | Category   | Sales |
| -------- | ------ | ---------- | ----: |
| 1001     | North  | Furniture  |  5200 |
| 1002     | South  | Technology |  8100 |
| 1003     | North  | Technology |  6200 |
| 1004     | West   | Furniture  |  7300 |

Instead of reading every transaction individually, management wants answers like:

| Region | Total Sales |
| ------ | ----------: |
| North  |       11400 |
| South  |        8100 |
| West   |        7300 |

GroupBy makes this possible using only a few lines of code.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Group data using one or multiple columns.
* Calculate summary statistics.
* Apply multiple aggregation functions.
* Create readable business reports.
* Use named aggregations.
* Build KPI summaries from transactional data.

---

# 4. Understanding GroupBy

The GroupBy operation follows three steps:

### Step 1 — Split

Divide the dataset into groups.

Example:

Group transactions by **Region**.

---

### Step 2 — Apply

Perform calculations for each group.

Examples:

* Sum
* Mean
* Count
* Maximum
* Minimum

---

### Step 3 — Combine

Merge the summarized results into one report.

This process is often called the **Split–Apply–Combine** strategy.

---

## Basic Syntax

```python id="grp01"
df.groupby("Region")
```

This creates a GroupBy object.

To calculate total sales:

```python id="grp02"
df.groupby(
    "Region"
)["Sales"].sum()
```

Output:

| Region | Sales |
| ------ | ----: |
| North  | 11400 |
| South  |  8100 |
| West   |  7300 |

---

# 5. Basic Aggregations

GroupBy supports many aggregation functions.

---

## Sum

```python id="grp03"
df.groupby(
    "Region"
)["Sales"].sum()
```

Calculates total sales for each region.

---

## Mean

```python id="grp04"
df.groupby(
    "Region"
)["Sales"].mean()
```

Calculates average sales.

---

## Count

```python id="grp05"
df.groupby(
    "Region"
)["Sales"].count()
```

Counts observations in each region.

---

## Maximum

```python id="grp06"
df.groupby(
    "Region"
)["Sales"].max()
```

Finds the highest sale.

---

## Minimum

```python id="grp07"
df.groupby(
    "Region"
)["Sales"].min()
```

Finds the smallest sale.

---

## Standard Deviation

```python id="grp08"
df.groupby(
    "Region"
)["Sales"].std()
```

Measures variability within each region.

---

# 6. Multiple Aggregations

Managers often require several statistics in one report.

Instead of creating multiple GroupBy operations, use `agg()`.

```python id="grp09"
df.groupby(
    "Region"
)["Sales"].agg([
    "sum",
    "mean",
    "max",
    "min",
    "count"
])
```

### Example Output

| Region |   Sum | Mean |  Max |  Min | Count |
| ------ | ----: | ---: | ---: | ---: | ----: |
| North  | 11400 | 5700 | 6200 | 5200 |     2 |
| South  |  8100 | 8100 | 8100 | 8100 |     1 |
| West   |  7300 | 7300 | 7300 | 7300 |     1 |

This creates a compact KPI report suitable for dashboards.

---

# 7. Named Aggregations

By default, aggregation names may not always be descriptive.

Named aggregations allow custom column names.

```python id="grp10"
df.groupby(
    "Region"
).agg(
    Total_Sales=("Sales", "sum"),
    Average_Sales=("Sales", "mean"),
    Highest_Sale=("Sales", "max"),
    Orders=("Sales", "count")
)
```

### Output

| Region | Total Sales | Average Sales | Highest Sale | Orders |
| ------ | ----------: | ------------: | -----------: | -----: |
| North  |       11400 |          5700 |         6200 |      2 |
| South  |        8100 |          8100 |         8100 |      1 |
| West   |        7300 |          7300 |         7300 |      1 |

Named aggregations produce cleaner reports that are easier to understand and share with stakeholders.

---

# Business Example

A retail company wants a monthly executive report.

Using GroupBy, analysts can generate:

* Total revenue by region.
* Average order value by customer segment.
* Maximum profit by category.
* Number of orders per city.
* Highest-selling product in each department.

Instead of manually calculating these values, GroupBy automates the entire reporting process.

---

# Best Practices

✔ Choose grouping columns that align with business questions.

✔ Use descriptive names with named aggregations.

✔ Combine multiple statistics into a single report whenever possible.

✔ Keep grouped reports focused on key performance indicators.

✔ Validate grouped results against the original data.

---

# Common Mistakes

### Forgetting to Select a Column

```python
df.groupby("Region").sum()
```

This aggregates every numeric column.

If you only need sales:

```python
df.groupby("Region")["Sales"].sum()
```

---

### Grouping by High-Cardinality Columns

Grouping by columns with thousands of unique values (such as Transaction ID) often produces reports that are too detailed to be useful.

Choose grouping columns that support meaningful analysis.

---

# Key Takeaways

After completing this section, you should understand:

* The Split–Apply–Combine strategy.
* How to group data by categories.
* How to calculate common aggregation metrics.
* How to perform multiple aggregations.
* How to create business-friendly reports using named aggregations.

> **"GroupBy transforms millions of individual records into concise summaries that drive business decisions, making it one of the most essential tools in every data analyst's workflow."**

# 8. Grouping by Multiple Columns

In real-world business scenarios, summarizing data using a single column is often not enough.

For example, management may ask:

* Sales by **Region** and **Category**
* Profit by **Department** and **City**
* Revenue by **Year** and **Quarter**

Grouping by multiple columns creates hierarchical summaries.

---

## Example Dataset

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
| North  | Technology |  6100 |
| South  | Furniture  |  7300 |
| South  | Technology |  8100 |
| West   | Furniture  |  6500 |

Group by Region and Category.

```python id="grp11"
df.groupby(
    ["Region", "Category"]
)["Sales"].sum()
```

### Output

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
| North  | Technology |  6100 |
| South  | Furniture  |  7300 |
| South  | Technology |  8100 |
| West   | Furniture  |  6500 |

Hierarchical grouping allows analysts to drill down into business performance.

---

# 9. Resetting the Index

Grouping creates a MultiIndex by default.

To convert the grouped output into a standard DataFrame:

```python id="grp12"
summary = (
    df.groupby(
        ["Region", "Category"]
    )["Sales"]
    .sum()
    .reset_index()
)
```

Output:

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5200 |
| North  | Technology |  6100 |
| South  | Furniture  |  7300 |
| South  | Technology |  8100 |
| West   | Furniture  |  6500 |

This format is easier to export to CSV or visualize.

---

# 10. Using `transform()`

Unlike `agg()`, which reduces each group to a single value, `transform()` returns a result for **every row** while preserving the original DataFrame shape.

This makes it useful for creating new calculated columns.

Example:

Calculate the average sales of each region for every transaction.

```python id="grp13"
df["Regional Average"] = (
    df.groupby("Region")["Sales"]
      .transform("mean")
)
```

### Output

| Region | Sales | Regional Average |
| ------ | ----: | ---------------: |
| North  |  5200 |             5650 |
| North  |  6100 |             5650 |
| South  |  7300 |             7700 |
| South  |  8100 |             7700 |

Each row now contains the average sales for its region.

---

# 11. Using `filter()`

Sometimes only groups meeting certain conditions should be retained.

Example:

Keep only regions whose total sales exceed ₹10,000.

```python id="grp14"
df.groupby("Region").filter(
    lambda group:
    group["Sales"].sum() > 10000
)
```

Output:

Only regions with total sales greater than ₹10,000 remain.

Typical use cases include:

* High-performing stores
* Premium customers
* High-revenue products
* Top-performing departments

---

# 12. Using `apply()`

The `apply()` function allows custom operations on each group.

Example:

Calculate the sales range.

```python id="grp15"
df.groupby("Region")["Sales"].apply(
    lambda x:
    x.max() - x.min()
)
```

Output:

| Region | Sales Range |
| ------ | ----------: |
| North  |         900 |
| South  |         800 |
| West   |           0 |

`apply()` is extremely flexible and can perform calculations beyond built-in aggregation functions.

---

# 13. Calculating Percentage Contribution

Businesses often need to know how much each transaction contributes to the regional total.

```python id="grp16"
df["Regional %"] = (
    df["Sales"]
    /
    df.groupby("Region")["Sales"]
      .transform("sum")
    * 100
)
```

### Output

| Region | Sales | Regional % |
| ------ | ----: | ---------: |
| North  |  5200 |      46.02 |
| North  |  6100 |      53.98 |
| South  |  7300 |      47.40 |
| South  |  8100 |      52.60 |

This calculation is widely used in KPI dashboards.

---

# 14. Ranking Within Groups

Rank employees, products, or stores inside each group.

Example:

```python id="grp17"
df["Regional Rank"] = (
    df.groupby("Region")["Sales"]
      .rank(
          method="dense",
          ascending=False
      )
)
```

Output:

| Region | Sales | Rank |
| ------ | ----: | ---: |
| North  |  6100 |    1 |
| North  |  5200 |    2 |
| South  |  8100 |    1 |
| South  |  7300 |    2 |

This is similar to SQL's `DENSE_RANK()`.

---

# 15. Group-Wise Standardization

Sometimes values should be normalized within each group.

Example:

```python id="grp18"
df["Standardized Sales"] = (
    df.groupby("Region")["Sales"]
      .transform(
          lambda x:
          (x - x.mean()) / x.std()
      )
)
```

This technique is commonly used before building machine learning models.

---

# Business Example

Suppose an international retailer wants to evaluate store performance.

Management requests:

* Regional sales summaries.
* Category-wise performance.
* Percentage contribution of each store.
* Rankings within regions.
* Average sales by category.

Using advanced GroupBy techniques, these reports can be created automatically without manual calculations.

---

# Best Practices

✔ Use multiple grouping columns for detailed analysis.

✔ Prefer `transform()` when creating new columns.

✔ Use `filter()` to remove low-value groups.

✔ Use `apply()` only when built-in functions cannot solve the problem.

✔ Reset indexes before exporting grouped results.

---

# Common Mistakes

### Confusing `agg()` and `transform()`

`agg()` reduces each group to one summary row.

`transform()` returns a value for every original row.

Understanding this distinction is essential.

---

### Overusing `apply()`

Although powerful, `apply()` is generally slower than built-in aggregation methods.

Whenever possible, prefer:

* `sum()`
* `mean()`
* `count()`
* `transform()`
* `agg()`

These methods are usually faster and easier to read.

---

# Quick Recap

You have now learned how to:

* Group by multiple columns.
* Reset grouped indexes.
* Create calculated columns using `transform()`.
* Filter entire groups.
* Apply custom functions.
* Calculate percentage contributions.
* Rank observations within groups.
* Standardize values inside groups.

> **"Advanced GroupBy techniques allow analysts to move beyond simple summaries and build dynamic, row-level metrics that power dashboards, business intelligence reports, and predictive analytics."**
