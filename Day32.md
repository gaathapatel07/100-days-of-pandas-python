# 🐼 Day 32 — Advanced Window Functions & Analytical Operations

<div align="center">

# 100 Days of Pandas

### Day 32 · Advanced Analytical Calculations with Window Functions

*"Window functions allow analysts to uncover trends, rankings, and cumulative insights while preserving every original observation."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Window%20Functions-blue)
![Day](https://img.shields.io/badge/Day-32-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. What are Window Functions?
3. Why Window Functions Matter
4. Learning Objectives
5. Cumulative Operations
6. Ranking Operations
7. Quantiles & Percentiles
8. Summary

---

# 1. Introduction

Traditional aggregation reduces many rows into a smaller summary.

Example:

| Region | Total Sales |
| ------ | ----------: |
| North  |      520000 |
| South  |      610000 |

However, analysts often need calculations **while keeping every original row**.

Window functions make this possible.

Examples include:

* Running totals
* Running averages
* Customer rankings
* Percentile calculations
* Financial indicators
* KPI monitoring

Unlike `groupby()`, window functions preserve the original number of rows.

---

# 2. What are Window Functions?

A window function performs calculations across a set of related rows while returning one result for **each row**.

Example:

| Day | Sales | Running Total |
| --: | ----: | ------------: |
|   1 |  5000 |          5000 |
|   2 |  6200 |         11200 |
|   3 |  4800 |         16000 |
|   4 |  7100 |         23100 |

The dataset remains the same size, but each row gains additional analytical information.

---

# 3. Why Window Functions Matter

Businesses frequently ask questions such as:

* How much revenue has accumulated so far?
* Which employee ranks highest this month?
* Is today's sales above the historical average?
* Which customers belong to the top 10%?
* How has profit changed over time?

Window functions answer these questions efficiently.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Calculate cumulative statistics.
* Rank observations.
* Compute percentiles.
* Generate running KPIs.
* Perform advanced analytical calculations.

---

# 5. Cumulative Operations

Pandas provides several cumulative functions.

| Function    | Description     |
| ----------- | --------------- |
| `cumsum()`  | Running total   |
| `cumprod()` | Running product |
| `cummax()`  | Running maximum |
| `cummin()`  | Running minimum |

---

## Running Total

```python id="cum01"
df["Running Sales"] = (
    df["Sales"]
      .cumsum()
)
```

Output

| Day | Sales | Running Sales |
| --: | ----: | ------------: |
|   1 |  5000 |          5000 |
|   2 |  6200 |         11200 |
|   3 |  4800 |         16000 |

---

## Running Product

```python id="cum02"
df["Growth"] = (
    df["Growth Rate"]
      .cumprod()
)
```

Useful for:

* Investment returns
* Compound growth
* Portfolio performance

---

## Running Maximum

```python id="cum03"
df["Highest Sales"] = (
    df["Sales"]
      .cummax()
)
```

Output

| Day | Sales | Highest Sales |
| --: | ----: | ------------: |
|   1 |  5000 |          5000 |
|   2 |  6200 |          6200 |
|   3 |  4800 |          6200 |

---

## Running Minimum

```python id="cum04"
df["Lowest Sales"] = (
    df["Sales"]
      .cummin()
)
```

Tracks the smallest value observed so far.

---

# 6. Ranking Operations

Ranking identifies the relative position of observations.

Example:

```python id="rank01"
df["Rank"] = (
    df["Sales"]
      .rank(
          ascending=False
      )
)
```

Output

| Customer | Sales | Rank |
| -------- | ----: | ---: |
| Alice    |  9200 |    1 |
| Rahul    |  8500 |    2 |
| Priya    |  7800 |    3 |

---

## Ranking Within Groups

Rank employees inside each department.

```python id="rank02"
df["Department Rank"] = (
    df.groupby("Department")["Salary"]
      .rank(
          ascending=False
      )
)
```

Each department receives its own ranking.

---

## Dense Ranking

```python id="rank03"
df["Dense Rank"] = (
    df["Sales"]
      .rank(
          method="dense",
          ascending=False
      )
)
```

Unlike standard ranking, dense ranking does not skip numbers after ties.

---

# 7. Quantiles & Percentiles

Quantiles divide data into equal-sized groups.

Find the median.

```python id="quant01"
df["Sales"].quantile(0.50)
```

Find the 90th percentile.

```python id="quant02"
df["Sales"].quantile(0.90)
```

Common percentiles:

| Percentile | Quantile |
| ---------- | -------- |
| 25%        | 0.25     |
| 50%        | 0.50     |
| 75%        | 0.75     |
| 90%        | 0.90     |
| 95%        | 0.95     |

---

## Percentile Rank

Calculate the relative standing of each observation.

```python id="quant03"
df["Percentile"] = (
    df["Sales"]
      .rank(pct=True)
)
```

Output

| Sales | Percentile |
| ----: | ---------: |
|  5200 |       0.20 |
|  6100 |       0.40 |
|  7400 |       0.80 |
|  9100 |       1.00 |

Useful for customer segmentation and performance analysis.

---

# Business Example

A retail company wants to analyze daily performance.

Analysts calculate:

* Running revenue.
* Highest sales so far.
* Customer rankings.
* Top 10% customers.
* Quarterly performance percentiles.

These insights support executive dashboards and strategic decision-making.

---

# Best Practices

✔ Sort data before cumulative calculations.

✔ Use group-based ranking for fair comparisons.

✔ Choose ranking methods carefully (`average`, `min`, `max`, `dense`, `first`).

✔ Validate cumulative metrics against expected totals.

✔ Use percentiles for customer segmentation.

---

# Common Mistakes

### Calculating Running Totals on Unsorted Data

Always sort first.

```python id="mistake01"
df = (
    df.sort_values("Date")
)
```

---

### Confusing Rank and Dense Rank

Example:

Sales:

```text id="mistake02"
100

100

90
```

Standard Rank:

```text id="mistake03"
1

1

3
```

Dense Rank:

```text id="mistake04"
1

1

2
```

Choose the method that matches the business requirement.

---

# Key Takeaways

After completing this section, you should understand:

* How cumulative operations work.
* How ranking differs from aggregation.
* How to calculate percentiles.
* How window functions preserve original rows.
* Why these techniques are essential for business analytics.

> **"Window functions enrich every record with contextual insights, enabling analysts to measure progress, compare performance, and identify meaningful patterns without losing detail."**

