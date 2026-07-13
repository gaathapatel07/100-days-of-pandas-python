
# 🐼 Day 38 — Advanced Data Aggregation & Business Analytics

<div align="center">

# 100 Days of Pandas

### Day 38 · Transforming Raw Data into Business Intelligence

*"Aggregation transforms millions of records into meaningful business insights that drive strategic decisions."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Data%20Aggregation-blue)
![Day](https://img.shields.io/badge/Day-38-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. What is Data Aggregation?
3. Why Aggregation Matters
4. Learning Objectives
5. Advanced GroupBy
6. Multi-Level Grouping
7. Multiple Aggregations
8. Summary

---

# 1. Introduction

Organizations collect massive amounts of raw data every day.

Examples include:

* Customer transactions
* Website visits
* Banking records
* Hospital admissions
* Manufacturing logs
* Sales invoices

Raw records are useful, but decision-makers usually require summarized information.

Data aggregation transforms detailed records into concise business metrics.

---

# 2. What is Data Aggregation?

Aggregation combines multiple observations into meaningful summaries.

Example:

| Region | Sales |
| ------ | ----: |
| North  |  5000 |
| North  |  6200 |
| South  |  4800 |
| South  |  7100 |

Aggregate by region.

```python id="agg01"
df.groupby("Region")["Sales"].sum()
```

Output

| Region | Total Sales |
| ------ | ----------: |
| North  |       11200 |
| South  |       11900 |

---

# 3. Why Aggregation Matters

Businesses frequently ask questions such as:

* Which region generated the highest revenue?
* Which products are most profitable?
* What is the average order value?
* Which month achieved the highest sales?
* Which department performs best?

Aggregation provides direct answers to these questions.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Perform advanced grouping.
* Aggregate multiple metrics.
* Group by multiple columns.
* Create business summaries.
* Generate KPI reports.

---

# 5. Advanced GroupBy

Group by a single column.

```python id="group01"
df.groupby(
    "Region"
)["Sales"].sum()
```

Average sales.

```python id="group02"
df.groupby(
    "Region"
)["Sales"].mean()
```

Maximum sales.

```python id="group03"
df.groupby(
    "Region"
)["Sales"].max()
```

Minimum sales.

```python id="group04"
df.groupby(
    "Region"
)["Sales"].min()
```

Count observations.

```python id="group05"
df.groupby(
    "Region"
)["Sales"].count()
```

---

# 6. Multi-Level Grouping

Group by multiple columns.

```python id="multi01"
df.groupby(
    [
        "Region",
        "Category"
    ]
)["Sales"].sum()
```

Example Output

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  | 52000 |
| North  | Technology | 61000 |
| South  | Furniture  | 47000 |
| South  | Technology | 59000 |

This creates a hierarchical summary.

---

## Reset Index

Convert the grouped result into a regular DataFrame.

```python id="multi02"
summary = (
    df.groupby(
        [
            "Region",
            "Category"
        ]
    )["Sales"]
     .sum()
     .reset_index()
)
```

---

# 7. Multiple Aggregations

Businesses usually require multiple statistics simultaneously.

```python id="multiagg01"
df.groupby(
    "Region"
)["Sales"].agg(
    [
        "sum",
        "mean",
        "max",
        "min",
        "count"
    ]
)
```

Output

| Region |    Sum | Mean |   Max |  Min | Count |
| ------ | -----: | ---: | ----: | ---: | ----: |
| North  | 520000 | 6200 |  9800 | 1200 |    84 |
| South  | 610000 | 6400 | 10200 | 1400 |    95 |

---

## Named Aggregations

Assign meaningful names to aggregated columns.

```python id="multiagg02"
summary = (
    df.groupby("Region")
      .agg(
          Total_Sales=("Sales", "sum"),
          Average_Sales=("Sales", "mean"),
          Highest_Sale=("Sales", "max"),
          Orders=("Sales", "count")
      )
)
```

Output

| Region | Total_Sales | Average_Sales | Highest_Sale | Orders |
| ------ | ----------: | ------------: | -----------: | -----: |
| North  |      520000 |          6200 |         9800 |     84 |
| South  |      610000 |          6400 |        10200 |     95 |

Named aggregations make reports easier to read.

---

# Business Example

A supermarket chain wants to evaluate regional performance.

Analysts calculate:

* Total revenue by region.
* Average order value.
* Highest transaction.
* Number of orders.
* Performance by region and product category.

These summaries are shared with regional managers to monitor business performance.

---

# Best Practices

✔ Choose aggregation functions based on the business question.

✔ Use named aggregations for readable reports.

✔ Reset indexes when exporting grouped results.

✔ Validate aggregated values before reporting.

✔ Keep grouped outputs well organized.

---

# Common Mistakes

### Forgetting to Reset the Index

Grouped results often have grouped columns as the index.

Use:

```python id="mistake01"
.reset_index()
```

when you need a regular DataFrame.

---

### Applying the Wrong Aggregation

For example, calculating the average of customer IDs has little business meaning.

Always select aggregation functions that match the metric.

---

### Ignoring Missing Values

Some aggregation functions skip missing values by default.

Review the dataset beforehand to ensure summaries accurately reflect the underlying data.

---

# Key Takeaways

After completing this section, you should understand:

* How `groupby()` creates summaries.
* How to group by multiple columns.
* How to calculate multiple statistics at once.
* How named aggregations improve readability.
* Why aggregation is central to business analytics.

> **"Aggregation transforms detailed operational data into concise business intelligence, enabling organizations to monitor performance, identify trends, and make informed decisions."**
