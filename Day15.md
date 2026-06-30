# Day 15 — Advanced Sorting, Ranking & Window Operations

<div align="center">

# 100 Days of Pandas

### Day 15 · Analyzing Trends, Rankings, and Sequential Data

*"Not all analysis is about totals. Sometimes the most valuable insights come from understanding order, rank, and change over time."*

![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-yellow)
![Topic](https://img.shields.io/badge/Topic-Sorting%20Ranking%20%26%20Window%20Functions-blue)
![Day](https://img.shields.io/badge/Day-15-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Ranking Matters
3. Learning Objectives
4. Advanced Sorting
5. Understanding Ranking
6. Ranking Methods
7. Percentile Ranking
8. Summary

---

# 1. Introduction

Businesses constantly compare performance.

Managers want to identify:

* The highest-selling products.
* The top-performing employees.
* The most profitable stores.
* The fastest-growing regions.
* The best customers based on revenue.

Simply sorting a dataset is often insufficient. Analysts also need rankings, percentiles, and sequential calculations that preserve the relationship between observations.

Pandas provides a powerful collection of functions that support these analytical tasks.

---

# 2. Why Ranking Matters

Suppose an online retailer records the following sales:

| Product |  Sales |
| ------- | -----: |
| Laptop  | 85,000 |
| Phone   | 62,000 |
| Tablet  | 62,000 |
| Mouse   | 15,000 |

Sorting displays the products in descending order.

However, management also wants to know:

* Which product ranks first?
* How should tied values be handled?
* Which products fall within the top 10%?

Ranking answers these questions.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Sort DataFrames efficiently.
* Rank records using multiple strategies.
* Handle tied values correctly.
* Calculate percentile rankings.
* Prepare datasets for leaderboard-style reports.
* Understand the foundation of window operations.

---

# 4. Advanced Sorting

Sorting organizes data into meaningful order.

### Sort by a Single Column

```python id="sort01"
df.sort_values(
    by="Sales"
)
```

Descending order:

```python id="sort02"
df.sort_values(
    by="Sales",
    ascending=False
)
```

---

### Sort by Multiple Columns

Suppose two products have identical sales.

Use profit as a secondary sorting criterion.

```python id="sort03"
df.sort_values(
    by=[
        "Sales",
        "Profit"
    ],
    ascending=[
        False,
        False
    ]
)
```

Output:

| Product | Sales | Profit |
| ------- | ----: | -----: |
| Laptop  | 85000 |  18000 |
| Phone   | 62000 |  12000 |
| Tablet  | 62000 |   9500 |
| Mouse   | 15000 |   3200 |

Sorting by multiple columns produces more consistent reports.

---

# 5. Understanding Ranking

Ranking assigns an ordered position to each observation.

Unlike sorting, ranking retains every row while adding a new column.

Example:

```python id="rank01"
df["Rank"] = (
    df["Sales"]
    .rank(
        ascending=False
    )
)
```

Output:

| Product | Sales | Rank |
| ------- | ----: | ---: |
| Laptop  | 85000 |    1 |
| Phone   | 62000 |  2.5 |
| Tablet  | 62000 |  2.5 |
| Mouse   | 15000 |    4 |

Notice that tied values receive the average rank by default.

---

# 6. Ranking Methods

Pandas supports several ranking strategies.

---

## Average Ranking (Default)

```python id="rank02"
df["Sales"].rank()
```

Equal values receive the average of their rank positions.

---

## Minimum Ranking

```python id="rank03"
df["Sales"].rank(
    method="min"
)
```

Output:

| Sales | Rank |
| ----: | ---: |
| 85000 |    4 |
| 62000 |    2 |
| 62000 |    2 |
| 15000 |    1 |

---

## Maximum Ranking

```python id="rank04"
df["Sales"].rank(
    method="max"
)
```

Tied observations receive the highest possible rank.

---

## Dense Ranking

```python id="rank05"
df["Sales"].rank(
    method="dense"
)
```

Output:

| Sales | Dense Rank |
| ----: | ---------: |
| 85000 |          3 |
| 62000 |          2 |
| 62000 |          2 |
| 15000 |          1 |

Unlike standard ranking, Dense Rank does **not** skip numbers after ties.

This is similar to SQL's `DENSE_RANK()` function.

---

## First Ranking

```python id="rank06"
df["Sales"].rank(
    method="first"
)
```

Tied values receive ranks according to their order in the dataset.

---

# 7. Percentile Ranking

Sometimes businesses classify observations by percentage rather than absolute rank.

Example:

```python id="rank07"
df["Percentile"] = (
    df["Sales"]
    .rank(
        pct=True
    )
)
```

Output:

| Product | Percentile |
| ------- | ---------: |
| Laptop  |       1.00 |
| Phone   |       0.75 |
| Tablet  |       0.75 |
| Mouse   |       0.25 |

Percentile rankings help answer questions such as:

* Which customers belong to the top 10%?
* Which employees are above the 90th percentile?
* Which stores fall below the bottom quartile?

---

# Key Takeaways

After completing this section, you should understand:

* The difference between sorting and ranking.
* How to sort by one or multiple columns.
* Various ranking methods available in Pandas.
* How Dense Rank differs from standard ranking.
* Why percentile rankings are valuable in business analytics.

> **"Sorting organizes data. Ranking adds meaning by showing the relative position of every observation."**

