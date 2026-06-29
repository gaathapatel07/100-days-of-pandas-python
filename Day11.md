# Day 11 — Pivot Tables & Cross Tabulations in Pandas

<div align="center">

# 100 Days of Pandas

### Day 11 · Summarizing Data Like a Business Analyst

*"Raw data tells individual stories. Pivot tables reveal the bigger picture."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Pivot%20Tables-blue)
![Day](https://img.shields.io/badge/Day-11-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Pivot Tables Matter
3. Learning Objectives
4. What is a Pivot Table?
5. Understanding `pivot()`
6. Understanding `pivot_table()`
7. Aggregation in Pivot Tables
8. Summary

---

# 1. Introduction

Businesses collect millions of rows of transactional data every day.

Reading these records individually provides very little insight.

Managers usually ask summary-based questions such as:

* Which region generated the highest revenue?
* Which product category is the most profitable?
* What are the monthly sales trends?
* Which customer segment contributes the most orders?

Answering these questions efficiently requires summarizing large datasets.

One of the most powerful tools for this purpose is the **Pivot Table**.

A Pivot Table reorganizes raw data into meaningful summaries by grouping, aggregating, and restructuring information.

---

# 2. Why Pivot Tables Matter

Imagine an online retailer has recorded **500,000 orders**.

Instead of viewing every transaction, management wants a report like this:

| Region | Total Sales |
| ------ | ----------: |
| North  |  ₹12,45,000 |
| South  |  ₹15,82,000 |
| East   |  ₹10,18,000 |
| West   |  ₹17,31,000 |

Creating this summary manually would be difficult.

Using a Pivot Table, the report can be generated in a single line of code.

Pivot tables are widely used in:

* Sales Analysis
* HR Reporting
* Finance
* Inventory Management
* Marketing Analytics
* Business Intelligence

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Understand the purpose of Pivot Tables.
* Create Pivot Tables using Pandas.
* Apply aggregation functions.
* Summarize multiple dimensions simultaneously.
* Build Excel-like analytical reports.
* Perform cross-tabulation analysis.

---

# 4. What is a Pivot Table?

A Pivot Table transforms detailed records into summarized information.

Suppose we have the following sales dataset:

| Region | Category   | Sales |
| ------ | ---------- | ----: |
| North  | Furniture  |  5000 |
| South  | Furniture  |  7000 |
| North  | Technology |  6200 |
| West   | Furniture  |  8500 |
| South  | Technology |  9200 |

Instead of reading each row, management wants to know:

* Total sales by region
* Average sales by category
* Sales for each region-category combination

Pivot Tables answer these questions quickly and clearly.

---

# 5. Understanding `pivot()`

The `pivot()` function reshapes data **without performing aggregation**.

### Syntax

```python
df.pivot(
    index="Row",
    columns="Column",
    values="Value"
)
```

Example:

```python
df.pivot(
    index="Region",
    columns="Category",
    values="Sales"
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5000 |       6200 |
| South  |      7000 |       9200 |
| West   |      8500 |        NaN |

Notice:

* Each Region becomes a row.
* Each Category becomes a column.
* Sales become the cell values.

---

## Limitation of `pivot()`

`pivot()` requires that each combination of row and column values be **unique**.

Consider this dataset:

| Region | Category  | Sales |
| ------ | --------- | ----: |
| North  | Furniture |  5000 |
| North  | Furniture |  6200 |

Running:

```python
df.pivot(
    index="Region",
    columns="Category",
    values="Sales"
)
```

produces:

```text
ValueError:
Index contains duplicate entries.
Cannot reshape.
```

When duplicate combinations exist, use **`pivot_table()`** instead.

---

# 6. Understanding `pivot_table()`

Unlike `pivot()`, `pivot_table()` can handle duplicate records because it performs aggregation.

Example:

```python
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    aggfunc="sum"
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |     11200 |       6200 |
| South  |      7000 |       9200 |

Duplicate rows are automatically summarized.

---

# 7. Aggregation Functions

`pivot_table()` supports multiple aggregation functions.

### Sum

```python
aggfunc="sum"
```

Calculates total sales.

---

### Mean

```python
aggfunc="mean"
```

Calculates average sales.

---

### Count

```python
aggfunc="count"
```

Counts records.

---

### Maximum

```python
aggfunc="max"
```

Finds the highest value.

---

### Minimum

```python
aggfunc="min"
```

Finds the smallest value.

---

### Example

```python
pd.pivot_table(
    df,
    values="Profit",
    index="Region",
    aggfunc=["sum", "mean", "max"]
)
```

This produces multiple business metrics in a single report.

---

# Key Takeaways

After completing this section, you should understand:

* The purpose of Pivot Tables.
* The difference between `pivot()` and `pivot_table()`.
* Why duplicate records require `pivot_table()`.
* How aggregation functions summarize business data.
* How Pivot Tables simplify large datasets.

> **"Pivot Tables transform thousands of rows into a report that decision-makers can understand in seconds."**

# 8. Multi-Level Pivot Tables

Business reports often need to summarize data across more than one dimension.

For example:

* Sales by **Region** and **Customer Segment**
* Profit by **Category** and **Sub-Category**
* Revenue by **Year** and **Quarter**

Pandas allows multiple fields in both the **index** and **columns**.

---

## Example

Suppose the dataset contains:

| Region | Segment   | Sales |
| ------ | --------- | ----: |
| North  | Consumer  |  5200 |
| North  | Corporate |  6100 |
| South  | Consumer  |  7300 |
| South  | Corporate |  8100 |

Create a pivot table:

```python id="p81wd7"
pd.pivot_table(
    df,
    values="Sales",
    index=["Region","Segment"],
    aggfunc="sum"
)
```

### Output

| Region | Segment   | Sales |
| ------ | --------- | ----: |
| North  | Consumer  |  5200 |
| North  | Corporate |  6100 |
| South  | Consumer  |  7300 |
| South  | Corporate |  8100 |

This hierarchical structure provides more detailed business summaries.

---

# 9. Multiple Aggregation Functions

Often, managers need more than one statistic.

Instead of creating separate reports, Pandas allows multiple aggregations.

```python id="s8av6n"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc=[
        "sum",
        "mean",
        "max",
        "min"
    ]
)
```

### Output

| Region |   Sum | Mean |  Max |  Min |
| ------ | ----: | ---: | ---: | ---: |
| North  | 11300 | 5650 | 6100 | 5200 |
| South  | 15400 | 7700 | 8100 | 7300 |

This produces a compact summary suitable for business reporting.

---

# 10. Replacing Missing Values

Sometimes certain combinations do not exist.

Example:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5200 |       6100 |
| South  |      7300 |        NaN |

Instead of displaying `NaN`, replace missing values.

```python id="n6zh1g"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    columns="Category",
    fill_value=0
)
```

Output:

| Region | Furniture | Technology |
| ------ | --------: | ---------: |
| North  |      5200 |       6100 |
| South  |      7300 |          0 |

This creates cleaner reports and avoids confusion.

---

# 11. Adding Grand Totals

Business reports often include totals.

Enable totals using:

```python id="g7jk4r"
pd.pivot_table(
    df,
    values="Sales",
    index="Region",
    aggfunc="sum",
    margins=True
)
```

Output:

| Region  | Sales |
| ------- | ----: |
| North   | 11300 |
| South   | 15400 |
| West    |  9800 |
| **All** | 36500 |

The **All** row represents the overall total.

This feature is similar to Excel Pivot Tables.

---

# 12. Cross Tabulation

While Pivot Tables summarize numerical values, **Cross Tabs** summarize the frequency of categorical variables.

Pandas provides the `crosstab()` function.

Example:

```python id="f3g7kh"
pd.crosstab(
    df["Region"],
   
```
# 12. Cross Tabulation (Continued)

Cross tabulation helps analysts understand the relationship between two or more categorical variables.

Unlike `pivot_table()`, which summarizes numerical values, `crosstab()` counts occurrences.

### Example

Suppose we have customer data:

| Region | Customer Segment |
| ------ | ---------------- |
| North  | Consumer         |
|        |                  |
