# Day 32 — Advanced Window Functions & Analytical Operations

<div align="center">

# 100 Days of Pandas

### Day 32 · Advanced Analytical Calculations with Window Functions

*"Window functions allow analysts to uncover trends, rankings, and cumulative insights while preserving every original observation."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Window%20Functions-blue)
![Day](https://img.shields.io/badge/Day-32-orange)

</div>

---

# Table of Contents

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

# 8. Advanced Rolling Statistics

Rolling windows are not limited to averages.

Pandas supports many statistical calculations over moving windows.

Example:

Calculate a **7-day rolling standard deviation**.

```python id="roll01"
df["Rolling Std"] = (
    df["Sales"]
      .rolling(7)
      .std()
)
```

Output

| Date  | Sales | Rolling Std |
| ----- | ----: | ----------: |
| Day 1 |  5200 |         NaN |
| Day 7 |  6100 |      450.21 |
| Day 8 |  5900 |      430.12 |

Rolling standard deviation measures short-term variability.

---

## Rolling Minimum

```python id="roll02"
df["Rolling Min"] = (
    df["Sales"]
      .rolling(7)
      .min()
)
```

---

## Rolling Maximum

```python id="roll03"
df["Rolling Max"] = (
    df["Sales"]
      .rolling(7)
      .max()
)
```

---

## Rolling Median

```python id="roll04"
df["Rolling Median"] = (
    df["Sales"]
      .rolling(7)
      .median()
)
```

---

# 9. Expanding Statistics

Unlike rolling windows, expanding windows include **all observations from the beginning**.

Running average.

```python id="expand01"
df["Running Mean"] = (
    df["Sales"]
      .expanding()
      .mean()
)
```

Running maximum.

```python id="expand02"
df["Running Max"] = (
    df["Sales"]
      .expanding()
      .max()
)
```

Running standard deviation.

```python id="expand03"
df["Running Std"] = (
    df["Sales"]
      .expanding()
      .std()
)
```

These metrics are useful for long-term performance tracking.

---

# 10. Exponentially Weighted Moving (EWM)

Traditional moving averages assign equal weight to every observation.

Exponentially Weighted Moving (EWM) gives **greater importance to recent observations**.

Example:

```python id="ewm01"
df["EWM"] = (
    df["Sales"]
      .ewm(span=7)
      .mean()
)
```

Recent sales influence the average more than older sales.

---

## Why Use EWM?

Compared with a simple moving average:

* Responds faster to recent changes.
* Smooths noisy data.
* Widely used in financial markets.
* Common in forecasting models.

---

# 11. Window Functions with `groupby()`

Window calculations can also be performed **within groups**.

Example:

Running sales total by region.

```python id="group01"
df["Regional Running Sales"] = (
    df.groupby("Region")["Sales"]
      .cumsum()
)
```

Output

| Region | Sales | Running Sales |
| ------ | ----: | ------------: |
| North  |  5200 |          5200 |
| North  |  6100 |         11300 |
| South  |  4800 |          4800 |

Each region maintains its own cumulative total.

---

## Rolling Average by Group

```python id="group02"
df["Regional Average"] = (
    df.groupby("Region")["Sales"]
      .transform(
          lambda x:
          x.rolling(3).mean()
      )
)
```

Each region receives an independent rolling calculation.

---

# 12. KPI Tracking

Businesses often monitor Key Performance Indicators (KPIs) over time.

Example:

Running revenue.

```python id="kpi01"
df["Revenue KPI"] = (
    df["Revenue"]
      .cumsum()
)
```

Customer growth.

```python id="kpi02"
df["Customer Growth"] = (
    df["Customers"]
      .pct_change()
)
```

Running profit margin.

```python id="kpi03"
df["Profit Margin"] = (
    df["Profit"]
    /
    df["Revenue"]
) * 100
```

These KPIs support executive dashboards.

---

# 13. Financial Analytics

Window functions are widely used in finance.

Calculate daily return.

```python id="finance01"
df["Daily Return"] = (
    df["Close"]
      .pct_change()
)
```

Calculate cumulative return.

```python id="finance02"
df["Cumulative Return"] = (
    (
        1 +
        df["Daily Return"]
    )
    .cumprod()
    - 1
)
```

Calculate a 20-day moving average.

```python id="finance03"
df["MA20"] = (
    df["Close"]
      .rolling(20)
      .mean()
)
```

These metrics help analysts evaluate investment performance.

---

# 14. Comparing Rolling vs Expanding vs EWM

| Feature     | Rolling        | Expanding       | EWM                             |
| ----------- | -------------- | --------------- | ------------------------------- |
| Window Size | Fixed          | Growing         | Infinite (weighted)             |
| Older Data  | Removed        | Retained        | Retained with decreasing weight |
| Recent Data | Equal Weight   | Equal Weight    | Higher Weight                   |
| Common Use  | Moving Average | Running Metrics | Financial Forecasting           |

---

# Business Example

A retail company wants to monitor:

* Weekly sales trends.
* Running annual revenue.
* Regional cumulative performance.
* Customer growth.
* Profit margins.

Using rolling, expanding, and exponentially weighted calculations, analysts build dashboards that reveal both short-term fluctuations and long-term trends.

---

# Best Practices

✔ Sort data before window calculations.

✔ Choose window sizes based on business requirements.

✔ Use EWM when recent observations should have more influence.

✔ Apply window functions within groups for segmented analysis.

✔ Validate cumulative calculations after transformations.

---

# Common Mistakes

### Using Window Functions on Unsorted Data

Always sort chronologically before applying rolling or expanding calculations.

```python id="mistake01"
df = df.sort_values("Date")
```

---

### Choosing an Inappropriate Window Size

A 365-day rolling average may hide important weekly patterns.

Select window sizes that align with the business problem.

---

### Ignoring Missing Values

Rolling and expanding calculations may produce `NaN` values at the beginning.

Handle these appropriately before visualization or modeling.

---

# Quick Recap

You have now learned how to:

* Calculate advanced rolling statistics.
* Use expanding windows.
* Apply Exponentially Weighted Moving averages.
* Combine window functions with `groupby()`.
* Build KPI tracking metrics.
* Perform financial analytics using window calculations.

> **"Advanced window functions allow analysts to monitor performance over time, identify trends, and generate dynamic business insights while preserving every original observation."**

# 15. Customer Segmentation Using Percentiles

One of the most common business applications of window functions is customer segmentation.

Suppose we want to classify customers based on total spending.

```python id="segment01"
df["Percentile"] = (
    df["Sales"]
      .rank(pct=True)
)
```

Example

| Customer | Sales | Percentile |
| -------- | ----: | ---------: |
| Alice    |  9200 |       1.00 |
| Rahul    |  8100 |       0.80 |
| Priya    |  6400 |       0.60 |
| Riya     |  4200 |       0.40 |

Now classify customers.

```python id="segment02"
df["Customer Tier"] = (
    pd.cut(
        df["Percentile"],
        bins=[0,0.50,0.80,1],
        labels=[
            "Bronze",
            "Silver",
            "Gold"
        ]
    )
)
```

Output

| Customer | Tier   |
| -------- | ------ |
| Alice    | Gold   |
| Rahul    | Gold   |
| Priya    | Silver |
| Riya     | Bronze |

This technique is commonly used in CRM systems and loyalty programs.

---

# 16. Building KPI Dashboards

Window functions are essential for business dashboards.

Example:

```python id="kpi01"
dashboard = (
    df
    .assign(
        Running_Revenue=lambda x:
        x["Revenue"].cumsum(),

        MA30=lambda x:
        x["Revenue"].rolling(30).mean(),

        Growth=lambda x:
        x["Revenue"].pct_change()*100
    )
)
```

Generated KPIs include:

* Running Revenue
* Moving Average
* Growth Rate

These metrics help executives monitor business performance over time.

---

# 17. Enterprise Analytical Workflow

Large organizations often process data through multiple analytical stages.

```text id="workflow01"
Raw Data
     │
     ▼
Data Cleaning
     │
     ▼
Feature Engineering
     │
     ▼
Window Functions
     │
     ▼
KPIs
     │
     ▼
Executive Dashboard
     │
     ▼
Business Decisions
```

Each stage adds analytical value without losing the original observations.

---

# 18. Performance Optimization

Window calculations can become expensive on large datasets.

### Sort Before Window Operations

```python id="perf01"
df = (
    df.sort_values("Date")
)
```

---

### Use Appropriate Data Types

```python id="perf02"
df["Region"] = (
    df["Region"]
      .astype("category")
)
```

---

### Avoid Recomputing Metrics

Instead of repeatedly calculating:

```python id="perf03"
df["Sales"].rolling(30).mean()
```

Store the result once.

```python id="perf04"
df["MA30"] = (
    df["Sales"]
      .rolling(30)
      .mean()
)
```

Reuse the calculated column throughout your analysis.

---

# 19. Enterprise Case Study

## Scenario

You are a **Senior Data Analyst** at **RetailHub**.

The company tracks:

* Daily Sales
* Revenue
* Profit
* Customers
* Marketing Spend

Management requests an executive dashboard.

---

## Business Questions

### Question 1

Calculate cumulative revenue.

```python id="case01"
df["Running Revenue"] = (
    df["Revenue"]
      .cumsum()
)
```

---

### Question 2

Calculate a 30-day moving average.

```python id="case02"
df["MA30"] = (
    df["Revenue"]
      .rolling(30)
      .mean()
)
```

---

### Question 3

Rank stores by profit.

```python id="case03"
df["Profit Rank"] = (
    df["Profit"]
      .rank(
          ascending=False
      )
)
```

---

### Question 4

Create customer spending percentiles.

```python id="case04"
df["Customer Percentile"] = (
    df["Sales"]
      .rank(
          pct=True
      )
)
```

---

### Question 5

Compute cumulative customer growth.

```python id="case05"
df["Customer Growth"] = (
    df["Customers"]
      .pct_change()
      .cumsum()
)
```

---

# 20. Business Insights

After applying advanced window functions, analysts discover:

* Revenue is growing steadily despite short-term fluctuations.
* The 30-day moving average smooths daily volatility and reveals long-term trends.
* A small percentage of customers generate a significant share of revenue.
* Regional rankings highlight the highest-performing business units.
* KPI dashboards enable faster and more informed executive decisions.

---

# 21. Practice Exercises

## Beginner

1. Calculate a running total.
2. Compute a cumulative maximum.
3. Rank employees by salary.
4. Find the median using `quantile()`.
5. Calculate a 7-day moving average.

---

## Intermediate

6. Create percentile ranks.
7. Build rolling standard deviations.
8. Calculate exponentially weighted averages.
9. Rank observations within groups.
10. Compute cumulative sales by region.

---

## Advanced

11. Build a KPI dashboard.
12. Create customer segmentation using percentiles.
13. Design a financial analytics workflow.
14. Optimize window calculations for large datasets.
15. Develop a reusable analytics pipeline.

---

# 22. Interview Questions

## Beginner

1. What is a window function?
2. Difference between `cumsum()` and `sum()`?
3. What does `rank()` do?
4. What is a rolling window?
5. What is an expanding window?

---

## Intermediate

6. Difference between rolling and expanding calculations?
7. What is EWM?
8. Why use percentile ranking?
9. Difference between `rank()` and `dense rank()`?
10. How do window functions differ from `groupby()`?

---

## Advanced

11. Design a KPI dashboard using window functions.
12. Explain customer segmentation using percentiles.
13. Compare rolling averages and EWM.
14. Optimize analytical workflows for millions of records.
15. Explain how window functions support executive decision-making.

---

# 23. Cheat Sheet

| Task              | Syntax                 |
| ----------------- | ---------------------- |
| Running Total     | `cumsum()`             |
| Running Product   | `cumprod()`            |
| Running Maximum   | `cummax()`             |
| Running Minimum   | `cummin()`             |
| Rank              | `rank()`               |
| Dense Rank        | `rank(method="dense")` |
| Percentile Rank   | `rank(pct=True)`       |
| Rolling Mean      | `rolling().mean()`     |
| Rolling Std       | `rolling().std()`      |
| Expanding Mean    | `expanding().mean()`   |
| EWM               | `ewm().mean()`         |
| Percentage Change | `pct_change()`         |

---

# 24. Mini Project

## Executive KPI Analytics Dashboard

Using any retail, banking, finance, healthcare, or e-commerce dataset:

Complete the following tasks:

* Calculate cumulative revenue.
* Build rolling 7-day, 30-day, and 90-day averages.
* Create expanding statistics.
* Apply EWM to smooth trends.
* Rank customers by spending.
* Segment customers using percentile ranks.
* Generate KPI metrics.
* Build a reusable analytical pipeline.
* Write **five executive-level business insights**.
* Recommend **three strategic improvements** based on KPI trends.

### Example Business Insights

* The top 20% of customers contributed the majority of revenue.
* Rolling averages revealed steady long-term growth despite daily fluctuations.
* EWM responded more quickly to recent market changes than simple moving averages.
* Regional cumulative sales highlighted consistently strong-performing markets.
* KPI dashboards enabled proactive monitoring of business performance.

---

# 25. Summary

Congratulations! 🎉

Today you mastered **Advanced Window Functions & Analytical Operations** in Pandas.

You learned how to:

* Calculate cumulative metrics.
* Apply rolling and expanding windows.
* Use exponentially weighted averages.
* Rank observations and calculate percentiles.
* Build KPI dashboards.
* Perform financial and customer analytics.
* Create enterprise-ready analytical workflows.

These techniques are fundamental for business intelligence, financial modeling, customer analytics, forecasting, and executive reporting.

---

# 26. What's Next?

In **Day 33**, you'll learn **Advanced Feature Engineering & Data Transformation**.

Topics include:

* Creating New Features
* Binning with `cut()` and `qcut()`
* One-Hot Encoding
* Label Encoding
* Dummy Variables
* Scaling & Normalization
* Standardization
* Mathematical Transformations
* Business Feature Engineering
* Machine Learning Data Preparation

These concepts bridge the gap between raw datasets and machine learning-ready data.

---

<div align="center">

# Day 32 Complete!

You've mastered **Window Functions & Analytical Operations**, one of the most valuable analytical skill sets in Pandas.

From cumulative metrics and rolling windows to KPI dashboards and customer segmentation, you can now build sophisticated analytical workflows while preserving every original record.


</div>
