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

# 8. Introduction to Window Operations

Traditional aggregation functions such as `sum()` or `mean()` calculate a single value for an entire dataset or group.

Window operations are different.

Instead of reducing the dataset to one result, they calculate statistics **while preserving every row**.

This allows analysts to observe how values evolve over time.

Window operations are extensively used in:

* Financial analysis
* Sales dashboards
* Customer behavior analysis
* Inventory forecasting
* Time-series analytics
* KPI monitoring

---

## Example Dataset

| Month    | Sales |
| -------- | ----: |
| January  |   100 |
| February |   120 |
| March    |   150 |
| April    |   180 |
| May      |   170 |

Instead of calculating one average for the entire dataset, analysts often want the average of the most recent observations.

This is where rolling windows become useful.

---

# 9. Rolling Window Calculations

A rolling window performs calculations over a fixed number of consecutive observations.

Suppose we calculate a **3-month moving average**.

```python id="roll01"
df["Rolling Mean"] = (
    df["Sales"]
      .rolling(window=3)
      .mean()
)
```

### Output

| Month    | Sales | Rolling Mean |
| -------- | ----: | -----------: |
| January  |   100 |          NaN |
| February |   120 |          NaN |
| March    |   150 |       123.33 |
| April    |   180 |       150.00 |
| May      |   170 |       166.67 |

The first two rows contain `NaN` because a full three-month window is not yet available.

---

## Rolling Sum

Instead of an average, calculate the cumulative sales for the previous three months.

```python id="roll02"
df["Rolling Sum"] = (
    df["Sales"]
      .rolling(window=3)
      .sum()
)
```

Output:

| Month    | Rolling Sum |
| -------- | ----------: |
| January  |         NaN |
| February |         NaN |
| March    |         370 |
| April    |         450 |
| May      |         500 |

Rolling sums are widely used for revenue tracking and inventory planning.

---

## Rolling Maximum

Find the highest value within each moving window.

```python id="roll03"
df["Rolling Max"] = (
    df["Sales"]
      .rolling(window=3)
      .max()
)
```

This is useful for identifying local peaks over time.

---

# 10. Expanding Window Calculations

Unlike rolling windows, expanding windows include **all previous observations**.

Every new calculation incorporates the complete history.

```python id="expand01"
df["Expanding Mean"] = (
    df["Sales"]
      .expanding()
      .mean()
)
```

Output:

| Month    | Sales | Expanding Mean |
| -------- | ----: | -------------: |
| January  |   100 |         100.00 |
| February |   120 |         110.00 |
| March    |   150 |         123.33 |
| April    |   180 |         137.50 |
| May      |   170 |         144.00 |

Expanding windows help analyze long-term trends.

---

## Expanding Sum

```python id="expand02"
df["Running Total"] = (
    df["Sales"]
      .expanding()
      .sum()
)
```

Output:

| Month    | Running Total |
| -------- | ------------: |
| January  |           100 |
| February |           220 |
| March    |           370 |
| April    |           550 |
| May      |           720 |

Running totals are common in financial and sales reporting.

---

# 11. Cumulative Functions

Pandas also provides cumulative calculations without explicitly using `expanding()`.

---

## Cumulative Sum

```python id="cum01"
df["Cumulative Sales"] = (
    df["Sales"]
      .cumsum()
)
```

Output:

| Month    | Cumulative Sales |
| -------- | ---------------: |
| January  |              100 |
| February |              220 |
| March    |              370 |
| April    |              550 |
| May      |              720 |

---

## Cumulative Maximum

```python id="cum02"
df["Highest Sales"] = (
    df["Sales"]
      .cummax()
)
```

Output:

| Month    | Highest Sales |
| -------- | ------------: |
| January  |           100 |
| February |           120 |
| March    |           150 |
| April    |           180 |
| May      |           180 |

---

## Cumulative Minimum

```python id="cum03"
df["Lowest Sales"] = (
    df["Sales"]
      .cummin()
)
```

---

## Cumulative Product

```python id="cum04"
df["Growth"] = (
    df["Sales"]
      .cumprod()
)
```

Although less common, cumulative products are useful in investment return calculations.

---

# 12. Ranking Within Groups

Businesses often compare performance **inside each category** rather than across the entire dataset.

Suppose the dataset contains:

| Region | Salesperson | Sales |
| ------ | ----------- | ----: |
| North  | Alice       | 42000 |
| North  | Rahul       | 51000 |
| South  | Emma        | 48000 |
| South  | David       | 52000 |

Rank salespeople within each region.

```python id="grouprank01"
df["Regional Rank"] = (
    df.groupby("Region")["Sales"]
      .rank(
          ascending=False,
          method="dense"
      )
)
```

### Output

| Region | Salesperson | Sales | Regional Rank |
| ------ | ----------- | ----: | ------------: |
| North  | Alice       | 42000 |             2 |
| North  | Rahul       | 51000 |             1 |
| South  | Emma        | 48000 |             2 |
| South  | David       | 52000 |             1 |

This approach is widely used in performance dashboards and sales competitions.

---

# Business Example

Imagine you're working for an online retailer.

Management asks:

* Show the running total of monthly revenue.
* Calculate the three-month moving average of sales.
* Rank stores within each region.
* Identify the highest revenue achieved so far.
* Compare current sales with historical trends.

Window functions answer all of these questions while preserving every transaction.

---

# Best Practices

✔ Sort time-series data before applying rolling calculations.

✔ Choose a window size that matches the business problem.

✔ Use cumulative functions for long-term trend analysis.

✔ Rank records within meaningful groups rather than globally when appropriate.

✔ Clearly document the meaning of calculated columns.

---

# Common Mistakes

### Forgetting to Sort Before Rolling

Always sort chronologically before applying rolling windows.

```python id="sortroll01"
df = df.sort_values("Order Date")
```

Otherwise, moving averages may be calculated in the wrong order.

---

### Using Rolling Windows on Categorical Data

Rolling calculations are intended for numerical or datetime-based sequences.

Applying them to text columns does not produce meaningful results.

---

# Quick Recap

You have now learned how to:

* Perform rolling calculations.
* Calculate moving averages.
* Create running totals.
* Apply cumulative functions.
* Rank observations within groups.
* Analyze sequential business data using window operations.

> **"Window functions preserve every observation while revealing trends, momentum, and performance over time—making them indispensable for modern business analytics."**

 # 13. Real-World Business Case Study

## Scenario

You are working as a **Business Data Analyst** at **RetailHub**, an international retail company.

The executive leadership team wants a monthly performance dashboard that not only shows sales figures but also tracks trends, rankings, and cumulative business performance.

The dataset contains:

* Order Date
* Region
* Store
* Salesperson
* Product Category
* Sales
* Profit

Your task is to build analytical metrics that help management monitor business performance over time.

---

# Business Questions

### Question 1

Sort the dataset by **Order Date** before analysis.

```python id="case_sort01"
df = df.sort_values("Order Date")
```

---

### Question 2

Calculate a **3-month moving average** of sales.

```python id="case_roll01"
df["3 Month Avg"] = (
    df["Sales"]
      .rolling(3)
      .mean()
)
```

---

### Question 3

Calculate the cumulative revenue.

```python id="case_cum01"
df["Running Revenue"] = (
    df["Sales"]
      .cumsum()
)
```

---

### Question 4

Rank salespeople within each region.

```python id="case_rank01"
df["Regional Rank"] = (
    df.groupby("Region")["Sales"]
      .rank(
          method="dense",
          ascending=False
      )
)
```

---

### Question 5

Calculate percentile rankings for sales.

```python id="case_pct01"
df["Sales Percentile"] = (
    df["Sales"]
      .rank(pct=True)
)
```

---

# Business Insights

After completing the analysis, you discover:

* The **West** region consistently achieves the highest cumulative revenue.
* Sales growth becomes more stable when viewed through a rolling average instead of daily fluctuations.
* A small number of salespeople consistently rank in the top percentile across multiple months.
* Running totals clearly highlight seasonal revenue growth.
* Percentile rankings identify high-performing stores that deserve additional investment.

These insights support executive decision-making in sales planning, staffing, forecasting, and incentive programs.

---

# 14. Practice Exercises

## Beginner

1. Sort a dataset by sales.
2. Sort by multiple columns.
3. Rank products by revenue.
4. Calculate dense rankings.
5. Compute percentile rankings.

---

## Intermediate

6. Create a rolling average using a 5-row window.
7. Calculate a rolling sum.
8. Generate cumulative sales.
9. Calculate cumulative maximum profit.
10. Rank employees within departments.

---

## Advanced

11. Compare rolling averages with cumulative averages.
12. Create regional leaderboards using grouped rankings.
13. Identify the top 10% of customers based on revenue.
14. Build a monthly KPI summary using window calculations.
15. Write five business recommendations based on trend analysis.

---

# 15. Interview Questions

## Beginner

1. What is the difference between sorting and ranking?
2. What does `sort_values()` do?
3. What is the purpose of `rank()`?
4. What is Dense Rank?
5. What is a rolling window?

---

## Intermediate

6. Difference between rolling and expanding windows?
7. Why should time-series data be sorted before rolling calculations?
8. What is a cumulative sum?
9. How do grouped rankings work?
10. What is percentile ranking?

---

## Advanced

11. Explain the business value of rolling averages.
12. Compare cumulative calculations with standard aggregation.
13. Describe a real-world use case for window functions.
14. How do Pandas window functions compare with SQL window functions?
15. How would you build a KPI dashboard using rolling metrics and rankings?

---

# 16. Cheat Sheet

| Operation          | Syntax                      |
| ------------------ | --------------------------- |
| Sort values        | `df.sort_values()`          |
| Sort index         | `df.sort_index()`           |
| Rank               | `df.rank()`                 |
| Dense Rank         | `df.rank(method="dense")`   |
| Percentile Rank    | `df.rank(pct=True)`         |
| Rolling Mean       | `.rolling(window=3).mean()` |
| Rolling Sum        | `.rolling(window=3).sum()`  |
| Expanding Mean     | `.expanding().mean()`       |
| Running Total      | `.cumsum()`                 |
| Cumulative Maximum | `.cummax()`                 |
| Cumulative Minimum | `.cummin()`                 |
| Group Ranking      | `groupby().rank()`          |

---

# 17. Mini Project

## Sales Performance Dashboard

Using any retail or sales dataset:

Perform the following tasks:

* Sort transactions by date.
* Calculate a 7-day rolling average of sales.
* Compute cumulative revenue.
* Rank products by total sales.
* Rank salespeople within each region.
* Identify the top 10% of customers using percentile rankings.
* Calculate cumulative maximum sales.
* Compare monthly revenue trends using rolling and expanding averages.
* Export the enriched dataset.
* Write **five executive-level business insights**.

### Example Business Insights

* Rolling averages reveal a steady upward sales trend despite short-term fluctuations.
* The West region contributes the highest cumulative revenue.
* A small group of salespeople consistently outperform their peers.
* Running totals highlight strong seasonal growth in the final quarter.
* Percentile rankings identify premium customers who account for a disproportionate share of total revenue.

---

# 18. Summary

Congratulations! 🎉

Today you explored **Advanced Sorting, Ranking & Window Operations**, which are among the most valuable analytical techniques in Pandas.

You learned how to:

* Sort datasets using one or multiple columns.
* Rank observations with different ranking methods.
* Calculate percentile rankings.
* Apply rolling windows for moving averages and sums.
* Build cumulative metrics.
* Rank records within groups.

These techniques are widely used in financial reporting, sales analytics, customer segmentation, forecasting, and executive dashboards.

---

# 19. What's Next?

In **Day 16**, you'll dive into **Time Series Analysis**, one of the most powerful applications of Pandas.

Topics include:

* Datetime Indexes
* Resampling (`resample()`)
* Upsampling & Downsampling
* Frequency Conversion
* Time Shifting (`shift()`)
* Lag Features
* Difference Calculations (`diff()`)
* Percentage Change (`pct_change()`)

These concepts form the foundation of forecasting, demand planning, stock market analysis, IoT monitoring, and predictive analytics.

---

<div align="center">

# 🎉 Day 15 Complete!

You've now mastered sorting, ranking, and window operations—core techniques used by data analysts, business intelligence professionals, and financial analysts every day.

These concepts bridge the gap between Pandas and SQL window functions, making your analytical skills more versatile across tools and platforms.

⭐ **Next → Day 16: Time Series Analysis with Pandas** 📅📈🐼

</div>
