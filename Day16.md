# Day 16 — Time Series Analysis with Pandas

<div align="center">

# 100 Days of Pandas

### Day 16 · Unlocking Insights from Time-Based Data

*"Every timestamp tells a story. Time series analysis helps us uncover trends, seasonality, and patterns hidden within chronological data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Time%20Series%20Analysis-blue)
![Day](https://img.shields.io/badge/Day-16-orange)

</div>

---

# Table of Contents

1. Introduction
2. Why Time Series Analysis Matters
3. Learning Objectives
4. Understanding Time Series Data
5. Creating a DateTime Index
6. Resampling Data
7. Frequency Conversion
8. Summary

---

# 1. Introduction

Many business datasets are organized around time.

Examples include:

* Daily sales transactions
* Hourly website traffic
* Monthly revenue reports
* Minute-by-minute stock prices
* Weather observations
* IoT sensor readings
* Hospital patient monitoring
* Energy consumption logs

Unlike ordinary datasets, time series data emphasizes the **order of observations**. The sequence of events is often as important as the values themselves.

Pandas provides specialized tools for analyzing chronological data efficiently.

---

# 2. Why Time Series Analysis Matters

Imagine you're working as a Data Analyst for an online retailer.

Management asks:

* Which month generated the highest revenue?
* Are weekend sales increasing?
* How does daily traffic compare with monthly trends?
* Which quarter experienced the strongest growth?
* Can we forecast future demand?

These questions require more than simple aggregation—they require time-aware analysis.

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

* Create DateTime indexes.
* Resample time series data.
* Convert between different frequencies.
* Prepare datasets for forecasting.
* Analyze long-term trends.
* Build time-based business reports.

---

# 4. Understanding Time Series Data

A **time series** is a sequence of observations recorded at regular or irregular intervals.

Example:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |
| 2025-01-04 |  6800 |

The **Date** column becomes the most important dimension because every observation depends on its position in time.

Time series datasets often include:

* Daily sales
* Hourly temperatures
* Weekly website visits
* Monthly profits
* Quarterly revenue

---

# 5. Creating a DateTime Index

Most time series operations require a DateTime index.

Suppose the dataset contains:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |

First, convert the Date column.

```python id="ts01"
df["Date"] = pd.to_datetime(
    df["Date"]
)
```

Now set it as the index.

```python id="ts02"
df = df.set_index("Date")
```

Output:

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |

Many Pandas time series functions depend on a DateTime index.

---

# 6. Resampling Data

Resampling changes the frequency of observations.

For example:

Daily data can become:

* Weekly
* Monthly
* Quarterly
* Yearly

Example:

Calculate monthly sales totals.

```python id="ts03"
df.resample("M").sum()
```

Output:

| Month    |  Sales |
| -------- | -----: |
| January  | 186000 |
| February | 172000 |

Resampling is fundamental for reporting and dashboard creation.

---

## Monthly Average

Instead of totals:

```python id="ts04"
df.resample("M").mean()
```

Common aggregation functions include:

* `sum()`
* `mean()`
* `max()`
* `min()`
* `count()`

---

# 7. Frequency Conversion

Pandas supports many time frequencies.

| Frequency | Meaning     |
| --------- | ----------- |
| `"D"`     | Daily       |
| `"W"`     | Weekly      |
| `"ME"`    | Month End   |
| `"QE"`    | Quarter End |
| `"YE"`    | Year End    |
| `"h"`     | Hourly      |
| `"min"`   | Minute      |
| `"s"`     | Second      |

Examples:

Weekly totals:

```python id="ts05"
df.resample("W").sum()
```

Quarterly averages:

```python id="ts06"
df.resample("QE").mean()
```

Yearly maximum:

```python id="ts07"
df.resample("YE").max()
```

Choosing the correct frequency depends on the business question being answered.

---

# Key Takeaways

After completing this section, you should understand:

* What makes time series data unique.
* Why a DateTime index is important.
* How to resample datasets.
* How to convert between time frequencies.
* Why time-aware analysis is essential in business intelligence.

> **"Time transforms isolated observations into meaningful trends, revealing patterns that static analysis can never uncover."**

# 8. Time Shifting with `shift()`

In many analytical tasks, comparing the current observation with previous or future observations is essential.

The `shift()` function moves data forward or backward without changing the original index.

This is commonly used to create **lag** and **lead** features.

---

## Example Dataset

| Date       | Sales |
| ---------- | ----: |
| 2025-01-01 |  5200 |
| 2025-01-02 |  6100 |
| 2025-01-03 |  5900 |
| 2025-01-04 |  6800 |

Create a one-day lag.

```python id="shift01"
df["Previous Day Sales"] = (
    df["Sales"]
      .shift(1)
)
```

### Output

| Date       | Sales | Previous Day Sales |
| ---------- | ----: | -----------------: |
| 2025-01-01 |  5200 |                NaN |
| 2025-01-02 |  6100 |               5200 |
| 2025-01-03 |  5900 |               6100 |
| 2025-01-04 |  6800 |               5900 |

Lag features are widely used in forecasting models.

---

## Lead Values

Move data backward.

```python id="shift02"
df["Next Day Sales"] = (
    df["Sales"]
      .shift(-1)
)
```

Output:

| Date       | Sales | Next Day Sales |
| ---------- | ----: | -------------: |
| 2025-01-01 |  5200 |           6100 |
| 2025-01-02 |  6100 |           5900 |
| 2025-01-03 |  5900 |           6800 |
| 2025-01-04 |  6800 |            NaN |

Lead values help compare current performance with future outcomes.

---

# 9. Difference Calculations

Instead of comparing absolute values, analysts often measure **change**.

Use `diff()`.

```python id="diff01"
df["Daily Change"] = (
    df["Sales"]
      .diff()
)
```

### Output

| Date       | Sales | Daily Change |
| ---------- | ----: | -----------: |
| 2025-01-01 |  5200 |          NaN |
| 2025-01-02 |  6100 |          900 |
| 2025-01-03 |  5900 |         -200 |
| 2025-01-04 |  6800 |          900 |

Positive values indicate growth, while negative values indicate decline.

---

## Multi-Period Difference

Compare with observations two days earlier.

```python id="diff02"
df["2-Day Change"] = (
    df["Sales"]
      .diff(2)
)
```

This is useful for weekly or monthly comparisons.

---

# 10. Percentage Change

Absolute differences are useful, but businesses often focus on **growth rates**.

Calculate percentage change.

```python id="pct01"
df["Growth Rate"] = (
    df["Sales"]
      .pct_change()
)
```

### Output

| Date       | Sales | Growth Rate |
| ---------- | ----: | ----------: |
| 2025-01-01 |  5200 |         NaN |
| 2025-01-02 |  6100 |      17.31% |
| 2025-01-03 |  5900 |      -3.28% |
| 2025-01-04 |  6800 |      15.25% |

Growth rates allow fair comparisons across different scales.

---

# 11. Lag Features for Machine Learning

Many forecasting models rely on previous observations.

Create multiple lag variables.

```python id="lag01"
df["Lag_1"] = (
    df["Sales"]
      .shift(1)
)

df["Lag_7"] = (
    df["Sales"]
      .shift(7)
)

df["Lag_30"] = (
    df["Sales"]
      .shift(30)
)
```

These features allow models to learn historical patterns.

Common lag periods include:

* Previous day
* Previous week
* Previous month
* Previous quarter

---

# 12. Rolling Time Windows

Rolling windows become even more powerful when combined with time series.

Calculate a 7-day moving average.

```python id="rollts01"
df["7-Day Average"] = (
    df["Sales"]
      .rolling(7)
      .mean()
)
```

Calculate a 30-day moving sum.

```python id="rollts02"
df["30-Day Sales"] = (
    df["Sales"]
      .rolling(30)
      .sum()
)
```

These metrics smooth short-term fluctuations and reveal longer-term trends.

---

# 13. Selecting Data by Date

Datetime indexes allow intuitive filtering.

Retrieve all records for a specific month.

```python id="date01"
df.loc["2025-03"]
```

Retrieve data for a specific day.

```python id="date02"
df.loc["2025-03-15"]
```

Retrieve a date range.

```python id="date03"
df.loc[
    "2025-01-01":"2025-03-31"
]
```

This syntax is concise and highly readable.

---

# Business Example

Imagine you're analyzing website traffic.

Your manager asks:

* Compare today's traffic with yesterday's.
* Calculate weekly moving averages.
* Identify daily growth rates.
* Create lag features for forecasting.
* Analyze quarterly trends.

These tasks rely heavily on shifting, differencing, rolling calculations, and time-based filtering.

---

# Best Practices

✔ Always sort data chronologically before time-series analysis.

✔ Set a DateTime index before resampling or slicing.

✔ Use `shift()` to create lag features.

✔ Use percentage change instead of absolute differences when comparing growth.

✔ Choose rolling window sizes based on the business cycle.

---

# Common Mistakes

### Forgetting to Sort Dates

```python id="sortts01"
df = df.sort_values("Date")
```

Time-series calculations depend on the correct chronological order.

---

### Applying `diff()` to Unsorted Data

If the dataset is unordered, calculated differences become meaningless.

Always verify the sequence before applying sequential calculations.

---

# Quick Recap

You have now learned how to:

* Shift observations forward and backward.
* Create lag and lead features.
* Measure absolute changes.
* Calculate growth rates.
* Build rolling time windows.
* Filter data using datetime indexes.

> **"Time-series analysis is not just about observing the past—it provides the foundation for understanding trends, detecting change, and preparing for the future."**

# 14. Real-World Business Case Study

## Scenario

You are working as a **Senior Data Analyst** at **RetailHub**, an international e-commerce company.

The executive team wants a **Sales Performance Dashboard** that tracks business performance over time.

The company has collected three years of daily sales data containing:

* Order Date
* Region
* Product Category
* Sales
* Profit
* Quantity

Management wants to understand long-term trends, seasonal behavior, growth rates, and historical comparisons before planning the next financial year.

---

# Business Questions

### Question 1

Convert the Order Date column into a DateTime index.

```python id="case_ts01"
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)

df = df.set_index("Order Date")
```

---

### Question 2

Calculate monthly revenue.

```python id="case_ts02"
monthly_sales = (
    df.resample("ME")["Sales"]
      .sum()
)
```

---

### Question 3

Create a 7-day moving average.

```python id="case_ts03"
df["7-Day Average"] = (
    df["Sales"]
      .rolling(7)
      .mean()
)
```

---

### Question 4

Calculate daily sales growth.

```python id="case_ts04"
df["Growth Rate"] = (
    df["Sales"]
      .pct_change()
)
```

---

### Question 5

Create a previous-day sales feature.

```python id="case_ts05"
df["Previous Day"] = (
    df["Sales"]
      .shift(1)
)
```

---

# Business Insights

After completing the analysis, you observe:

* Sales consistently increase during the fourth quarter each year.
* The 7-day moving average removes daily fluctuations and highlights long-term growth.
* Growth rates reveal periods of rapid expansion and temporary decline.
* Previous-day sales provide valuable context for short-term forecasting.
* Monthly resampling makes executive reports easier to interpret.

These insights support budgeting, inventory planning, staffing, and forecasting.

---

# 15. Practice Exercises

## Beginner

1. Convert a date column into a DateTime object.
2. Set the date column as the DataFrame index.
3. Calculate monthly sales totals.
4. Create weekly average sales.
5. Retrieve data for a specific month.

---

## Intermediate

6. Create lag features using `shift()`.
7. Calculate daily differences.
8. Calculate percentage growth rates.
9. Create a rolling average.
10. Compare monthly and quarterly sales.

---

## Advanced

11. Build a complete monthly KPI report.
12. Create multiple lag features for forecasting.
13. Analyze seasonal sales patterns.
14. Compare rolling averages with expanding averages.
15. Write five recommendations based on the observed time-series trends.

---

# 16. Interview Questions

## Beginner

1. What is a time series?
2. Why is a DateTime index important?
3. What does `resample()` do?
4. What is the purpose of `shift()`?
5. What is the difference between `diff()` and `pct_change()`?

---

## Intermediate

6. Why should dates be sorted before analysis?
7. Difference between upsampling and downsampling?
8. What is a rolling window?
9. Why are lag features important?
10. When should moving averages be used?

---

## Advanced

11. Explain a real-world forecasting workflow using Pandas.
12. Compare rolling averages and cumulative calculations.
13. How does time-series analysis support business decisions?
14. What challenges arise when analyzing irregular time-series data?
15. How would you prepare sales data for predictive modeling?

---

# 17. Cheat Sheet

| Operation           | Syntax                        |
| ------------------- | ----------------------------- |
| Convert to DateTime | `pd.to_datetime()`            |
| Set DateTime Index  | `df.set_index("Date")`        |
| Monthly Resample    | `df.resample("ME").sum()`     |
| Weekly Resample     | `df.resample("W").mean()`     |
| Shift Values        | `df.shift()`                  |
| Difference          | `df.diff()`                   |
| Percentage Change   | `df.pct_change()`             |
| Rolling Average     | `df.rolling(7).mean()`        |
| Rolling Sum         | `df.rolling(30).sum()`        |
| Select by Month     | `df.loc["2025-03"]`           |
| Select Date Range   | `df.loc["2025-01":"2025-06"]` |

---

# 18. Mini Project

## Sales Trend & Forecast Preparation

Using any retail, stock market, weather, or IoT dataset:

Complete the following tasks:

* Convert the date column into a DateTime object.
* Set it as the index.
* Resample the data by week and month.
* Calculate rolling averages.
* Create lag features.
* Compute percentage growth rates.
* Identify seasonal trends.
* Compare quarterly performance.
* Export the processed dataset.
* Write **five executive-level business insights**.

### Example Business Insights

* Sales peak consistently during the holiday season.
* Weekly averages reveal stable long-term growth despite daily fluctuations.
* Growth rates identify periods of exceptional business performance.
* Lag features indicate strong autocorrelation in sales patterns.
* Monthly summaries provide clearer insights for executive reporting than daily transaction data.

---

# 19. Summary

Congratulations! 🎉

Today you mastered **Time Series Analysis** in Pandas.

You learned how to:

* Create DateTime indexes.
* Resample data into different time frequencies.
* Shift observations.
* Calculate differences and growth rates.
* Build rolling averages.
* Generate lag features.
* Filter and analyze chronological data.

These techniques form the foundation of forecasting, financial analysis, demand planning, predictive modeling, and operational reporting.

---

# 20. What's Next?

In **Day 17**, you'll explore **Data Visualization with Pandas & Matplotlib**.

Topics include:

* Line Charts
* Bar Charts
* Histograms
* Box Plots
* Scatter Plots
* Pie Charts
* Subplots
* Figure Customization
* Business Dashboards

You'll learn how to transform cleaned and analyzed data into clear, compelling visualizations that communicate insights effectively.

---

<div align="center">

# Day 16 Complete!

You've now mastered one of the most powerful capabilities of Pandas: **Time Series Analysis**.

From resampling and rolling windows to lag features and growth analysis, you're equipped to analyze temporal data and prepare it for forecasting and strategic decision-making.


</div>
