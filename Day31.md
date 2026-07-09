# Day 31 — Advanced Time Series Analysis in Pandas

<div align="center">

# 100 Days of Pandas

### Day 31 · Mastering Time Series Data

*"Every timestamp tells a story. Time series analysis helps us understand trends, seasonality, and patterns hidden within chronological data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Time%20Series-blue)
![Day](https://img.shields.io/badge/Day-31-orange)

</div>

---

# Table of Contents  

1. Introduction
2. What is Time Series Data?
3. Why Time Series Matters
4. Learning Objectives
5. Creating a DateTime Index
6. Time-Based Indexing
7. Selecting Data by Time
8. Summary

---

# 1. Introduction

A **Time Series** is a sequence of observations recorded over time.

Unlike ordinary datasets, **time is the primary dimension**.

Examples include:

* Daily stock prices
* Monthly revenue
* Hourly website traffic
* Weather measurements
* Electricity consumption
* Customer registrations
* Sensor readings
* Cryptocurrency prices

Pandas provides one of the most powerful time-series APIs available in Python.

---

# 2. What is Time Series Data?

Example:

| Date       | Sales |
| ---------- | ----: |
| 2026-01-01 |  5200 |
| 2026-01-02 |  6100 |
| 2026-01-03 |  5700 |
| 2026-01-04 |  6400 |

Notice that every observation has an associated timestamp.

Unlike categorical or numerical datasets, chronological order matters.

---

# 3. Why Time Series Matters

Businesses continuously ask questions such as:

* How have sales changed over time?
* Which months generate the highest revenue?
* Is customer growth accelerating?
* What seasonal patterns exist?
* Can future demand be predicted?

Time series analysis provides answers to these questions.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Create DateTime indexes.
* Work with chronological data.
* Select data by date.
* Filter using partial dates.
* Prepare datasets for forecasting.

---

# 5. Creating a DateTime Index

Suppose a dataset contains an **Order Date** column.

```python id="ts01"
df["Order Date"] = (
    pd.to_datetime(
        df["Order Date"]
    )
)
```

Convert the column into the DataFrame index.

```python id="ts02"
df = df.set_index(
    "Order Date"
)
```

Now check the index.

```python id="ts03"
df.index
```

Output

```text id="ts04"
DatetimeIndex(...)
```

A **DatetimeIndex** unlocks powerful time-series functionality.

---

# 6. Time-Based Indexing

Once a DateTime index exists, selecting records becomes simple.

Retrieve one specific date.

```python id="ts05"
df.loc[
    "2026-07-01"
]
```

Retrieve one month.

```python id="ts06"
df.loc[
    "2026-07"
]
```

Retrieve one year.

```python id="ts07"
df.loc[
    "2026"
]
```

Pandas automatically interprets these partial date strings.

---

# 7. Selecting Date Ranges

Retrieve observations between two dates.

```python id="ts08"
df.loc[
    "2026-01-01":"2026-03-31"
]
```

Example

| Date       | Sales |
| ---------- | ----: |
| 2026-01-01 |  5200 |
| 2026-01-15 |  6100 |
| 2026-02-10 |  5700 |

---

## Using Boolean Filtering

```python id="ts09"
df[
    df.index >=
    "2026-06-01"
]
```

Or

```python id="ts10"
df[
    (
        df.index >=
        "2026-01-01"
    )
    &
    (
        df.index <=
        "2026-06-30"
    )
]
```

---

# Business Example

An online retailer tracks daily revenue.

Management wants:

* Sales for July.
* Revenue during Q1.
* Weekend performance.
* Holiday sales.

Using a `DatetimeIndex`, these reports can be generated quickly with intuitive date-based filtering.

---

# Best Practices

✔ Convert date columns immediately after importing data.

✔ Use `DatetimeIndex` for time-series datasets.

✔ Keep timestamps in chronological order.

✔ Validate date formats before analysis.

✔ Store timestamps using `datetime64[ns]`.

---

# Common Mistakes

### Keeping Dates as Strings

Incorrect

```python id="mistake01"
object
```

Correct

```python id="mistake02"
datetime64[ns]
```

---

### Forgetting to Sort Dates

Before time-series analysis:

```python id="mistake03"
df = (
    df.sort_index()
)
```

Sorting ensures rolling calculations, slicing, and resampling behave correctly.

---

# Key Takeaways

After completing this section, you should understand:

* What time-series data is.
* Why chronological order matters.
* How to create a `DatetimeIndex`.
* How to select records by day, month, year, and date ranges.
* Why DateTime indexing is essential for forecasting and trend analysis.

> **"Time-series analysis begins with treating time as a first-class feature, enabling meaningful exploration of trends, cycles, and business performance over time."**

# 8. Resampling Time Series Data

Real-world data is often collected at one frequency but analyzed at another.

For example:

* Hourly → Daily
* Daily → Monthly
* Monthly → Quarterly
* Quarterly → Yearly

Pandas uses **`resample()`** for frequency conversion.

---

## Daily Sales → Monthly Sales

```python id="resample01"
monthly_sales = (
    df["Sales"]
      .resample("M")
      .sum()
)
```

Output

| Month | Total Sales |
| ----- | ----------: |
| Jan   |      155000 |
| Feb   |      148200 |
| Mar   |      172600 |

`"M"` represents **Month-End** frequency.

---

## Daily Sales → Weekly Sales

```python id="resample02"
weekly_sales = (
    df["Sales"]
      .resample("W")
      .sum()
)
```

---

## Daily Sales → Yearly Sales

```python id="resample03"
yearly_sales = (
    df["Sales"]
      .resample("Y")
      .sum()
)
```

---

# Common Resampling Frequencies

| Code  | Meaning     |
| ----- | ----------- |
| `D`   | Daily       |
| `W`   | Weekly      |
| `M`   | Month-End   |
| `MS`  | Month Start |
| `Q`   | Quarter-End |
| `Y`   | Year-End    |
| `H`   | Hourly      |
| `min` | Minute      |
| `s`   | Second      |

---

# 9. Frequency Conversion

Sometimes you need to change the frequency without aggregation.

Example:

```python id="freq01"
df = (
    df.asfreq("D")
)
```

Missing dates are inserted with `NaN`.

---

## Fill Missing Dates

```python id="freq02"
df = (
    df.asfreq("D")
      .ffill()
)
```

Useful for stock prices and sensor data where continuous dates are required.

---

# 10. Rolling Windows

Rolling windows calculate statistics over a moving window of observations.

Example:

Calculate a **7-day moving average**.

```python id="rolling01"
df["7-Day Average"] = (
    df["Sales"]
      .rolling(window=7)
      .mean()
)
```

Output

| Date  | Sales | 7-Day Average |
| ----- | ----: | ------------: |
| Jan 1 |  5200 |           NaN |
| Jan 2 |  6100 |           NaN |
| ...   |   ... |           ... |
| Jan 7 |  5900 |          5600 |

Rolling averages smooth short-term fluctuations.

---

## Rolling Sum

```python id="rolling02"
df["7-Day Total"] = (
    df["Sales"]
      .rolling(7)
      .sum()
)
```

---

## Rolling Maximum

```python id="rolling03"
df["Highest Week"] = (
    df["Sales"]
      .rolling(7)
      .max()
)
```

---

# 11. Expanding Windows

Unlike rolling windows, expanding windows include **all previous observations**.

```python id="expand01"
df["Running Average"] = (
    df["Sales"]
      .expanding()
      .mean()
)
```

Example

| Day | Sales | Running Average |
| --: | ----: | --------------: |
|   1 |  5000 |            5000 |
|   2 |  6000 |            5500 |
|   3 |  7000 |            6000 |

Useful for cumulative business metrics.

---

# 12. Shifting Time Series

Move observations forward or backward.

```python id="shift01"
df["Previous Day"] = (
    df["Sales"]
      .shift(1)
)
```

Output

| Day | Sales | Previous Day |
| --: | ----: | -----------: |
|   1 |  5000 |          NaN |
|   2 |  6200 |         5000 |
|   3 |  5800 |         6200 |

---

## Future Values

```python id="shift02"
df["Next Day"] = (
    df["Sales"]
      .shift(-1)
)
```

---

# 13. Lag Features

Lag features are widely used in forecasting and machine learning.

Example:

```python id="lag01"
df["Lag_1"] = (
    df["Sales"]
      .shift(1)
)

df["Lag_7"] = (
    df["Sales"]
      .shift(7)
)
```

These columns represent previous observations.

---

# 14. Lead Features

Lead features reference future values.

```python id="lead01"
df["Lead_1"] = (
    df["Sales"]
      .shift(-1)
)
```

Often used for comparison and target generation during model development.

---

# 15. Moving Averages

Moving averages smooth noisy data and highlight long-term trends.

### 30-Day Moving Average

```python id="ma01"
df["MA30"] = (
    df["Sales"]
      .rolling(30)
      .mean()
)
```

### 90-Day Moving Average

```python id="ma02"
df["MA90"] = (
    df["Sales"]
      .rolling(90)
      .mean()
)
```

Applications:

* Stock market analysis
* Demand forecasting
* Revenue trends
* Production planning

---

# Business Example

A retail company records daily sales.

Management wants:

* Weekly revenue.
* Monthly trends.
* Rolling 30-day average.
* Previous day's sales.
* Quarterly summaries.

Using:

* `resample()`
* `rolling()`
* `expanding()`
* `shift()`

analysts generate executive dashboards that reveal trends and seasonal patterns.

---

# Best Practices

✔ Sort data before time-series operations.

✔ Use meaningful rolling window sizes.

✔ Choose resampling frequencies that match business needs.

✔ Validate missing dates before resampling.

✔ Create lag features for forecasting models.

---

# Common Mistakes

### Resampling Without a `DatetimeIndex`

Incorrect:

```python id="mistake01"
df.resample("M")
```

This raises an error unless the DataFrame has a `DatetimeIndex`.

---

### Misinterpreting Initial `NaN` Values

Rolling calculations require enough observations to fill the window.

For a 7-day rolling mean, the first six rows will naturally contain `NaN`.

---

### Using Lag Features Without Handling Missing Values

Shifting introduces missing values at the beginning or end of the dataset.

Always decide whether to remove or impute these rows before modeling.

---

# Quick Recap

You have now learned how to:

* Resample time-series data.
* Convert frequencies using `asfreq()`.
* Compute rolling statistics.
* Calculate expanding metrics.
* Shift observations.
* Create lag and lead features.
* Build moving averages for trend analysis.

> **"Time-series analysis transforms chronological observations into meaningful trends, enabling organizations to monitor performance, identify patterns, and forecast future outcomes."**

# 16. Trend Analysis

One of the primary goals of time series analysis is to identify **long-term trends**.

A trend represents the general direction in which data moves over time.

Example:

| Month |  Sales |
| ----- | -----: |
| Jan   | 120000 |
| Feb   | 125000 |
| Mar   | 131000 |
| Apr   | 138000 |

This dataset shows an **upward trend**.

A simple way to visualize the trend is with a moving average.

```python id="trend01"
df["Trend"] = (
    df["Sales"]
      .rolling(30)
      .mean()
)
```

The moving average smooths daily fluctuations and highlights the long-term direction.

---

# 17. Seasonality Analysis

Seasonality refers to patterns that repeat at regular intervals.

Examples:

* Higher retail sales during festivals.
* Increased electricity usage during summer.
* Weekend restaurant sales.
* Monthly salary payments.

Extract useful date components.

```python id="season01"
df["Month"] = df.index.month

df["Day"] = df.index.day

df["Weekday"] = df.index.day_name()
```

Average sales by month.

```python id="season02"
monthly_avg = (
    df.groupby("Month")["Sales"]
      .mean()
)
```

Average sales by weekday.

```python id="season03"
weekday_avg = (
    df.groupby("Weekday")["Sales"]
      .mean()
)
```

---

# 18. Time-Based Grouping

Instead of creating new columns manually, Pandas provides **`pd.Grouper()`**.

Monthly revenue.

```python id="group01"
df.groupby(
    pd.Grouper(freq="M")
)["Sales"].sum()
```

Quarterly revenue.

```python id="group02"
df.groupby(
    pd.Grouper(freq="Q")
)["Sales"].sum()
```

Yearly revenue.

```python id="group03"
df.groupby(
    pd.Grouper(freq="Y")
)["Sales"].sum()
```

This is cleaner than extracting year and month separately.

---

# 19. Business Calendar Operations

Many organizations analyze business days instead of calendar days.

Count business days.

```python id="calendar01"
business_days = pd.bdate_range(
    start="2026-01-01",
    end="2026-01-31"
)
```

Create a business-day frequency.

```python id="calendar02"
df = df.asfreq("B")
```

`"B"` represents **Business Days**.

Useful for:

* Banking
* Stock markets
* Corporate reporting

---

# 20. Combining Time Series Operations

Professional analytics often combines several operations.

```python id="combine01"
report = (
    df
    .resample("M")
    .sum()
    .assign(
        MA3=lambda x:
        x["Sales"]
          .rolling(3)
          .mean()
    )
)
```

This pipeline:

* Resamples monthly.
* Calculates total sales.
* Adds a 3-month moving average.

---

# 21. Enterprise Case Study

## Scenario

You are a **Senior Business Analyst** at **RetailHub**.

The company stores:

* Daily sales
* Website traffic
* Marketing spend
* Customer registrations

Management requests:

* Monthly revenue.
* Quarterly growth.
* Rolling sales average.
* Best-performing weekdays.
* Seasonal demand patterns.

---

## Business Questions

### Question 1

Monthly revenue.

```python id="case01"
monthly_sales = (
    df["Sales"]
      .resample("M")
      .sum()
)
```

---

### Question 2

30-day moving average.

```python id="case02"
df["MA30"] = (
    df["Sales"]
      .rolling(30)
      .mean()
)
```

---

### Question 3

Previous day's sales.

```python id="case03"
df["Yesterday"] = (
    df["Sales"]
      .shift(1)
)
```

---

### Question 4

Average weekday sales.

```python id="case04"
df.groupby(
    df.index.day_name()
)["Sales"].mean()
```

---

### Question 5

Quarterly sales.

```python id="case05"
df.groupby(
    pd.Grouper(freq="Q")
)["Sales"].sum()
```

---

# 22. Business Insights

After analyzing the time-series data, analysts discover:

* Sales consistently increase during the fourth quarter.
* Weekend revenue is significantly higher than weekday revenue.
* The 30-day moving average reveals a sustained upward trend.
* Marketing campaigns lead to short-term spikes in customer registrations.
* Quarterly reports help management plan inventory and staffing more effectively.

---

# 23. Practice Exercises

## Beginner

1. Convert a column to `datetime`.
2. Set a `DatetimeIndex`.
3. Select data for a specific month.
4. Resample daily data into monthly totals.
5. Calculate a 7-day moving average.

---

## Intermediate

6. Compute quarterly sales.
7. Create lag features.
8. Build an expanding average.
9. Group by weekday.
10. Generate business-day frequencies.

---

## Advanced

11. Build a forecasting-ready dataset.
12. Create rolling statistics with multiple window sizes.
13. Analyze monthly seasonality.
14. Build a reusable time-series pipeline.
15. Generate executive KPI reports.

---

# 24. Interview Questions

## Beginner

1. What is time-series data?
2. What is a `DatetimeIndex`?
3. What does `resample()` do?
4. What is a rolling average?
5. What is a lag feature?

---

## Intermediate

6. Difference between `rolling()` and `expanding()`?
7. Difference between `resample()` and `groupby()`?
8. Why use `shift()`?
9. What is seasonality?
10. What is a business-day frequency?

---

## Advanced

11. Design a time-series analytics workflow.
12. Explain trend and seasonality.
13. How would you prepare time-series data for forecasting?
14. How do moving averages help businesses?
15. How do time-series techniques improve decision-making?

---

# 25. Cheat Sheet

| Task                 | Syntax                 |
| -------------------- | ---------------------- |
| Convert to DateTime  | `pd.to_datetime()`     |
| Set DateTime Index   | `set_index()`          |
| Resample             | `resample()`           |
| Frequency Conversion | `asfreq()`             |
| Rolling Average      | `rolling().mean()`     |
| Expanding Average    | `expanding().mean()`   |
| Shift                | `shift()`              |
| Lag Feature          | `shift(1)`             |
| Lead Feature         | `shift(-1)`            |
| Business Days        | `pd.bdate_range()`     |
| Time Grouper         | `pd.Grouper(freq="M")` |

---

# 26. Mini Project

## Retail Sales Time-Series Dashboard

Using any retail, finance, healthcare, logistics, or stock market dataset:

Complete the following tasks:

* Convert dates to `DatetimeIndex`.
* Resample daily data into weekly and monthly summaries.
* Calculate 7-day, 30-day, and 90-day moving averages.
* Create lag and lead features.
* Analyze monthly and weekday seasonality.
* Generate quarterly revenue reports.
* Build a reusable time-series pipeline.
* Write **five executive-level business insights**.
* Recommend **three strategies** for improving forecasting accuracy.

### Example Business Insights

* Fourth-quarter sales consistently exceeded other quarters.
* Weekend demand was significantly higher than weekday demand.
* Moving averages highlighted steady long-term revenue growth.
* Lag features revealed strong short-term sales dependencies.
* Monthly summaries simplified executive reporting.

---

# 27. Summary

Congratulations! 🎉

Today you mastered **Advanced Time Series Analysis** in Pandas.

You learned how to:

* Create and work with `DatetimeIndex`.
* Resample data at different frequencies.
* Calculate rolling and expanding statistics.
* Build lag and lead features.
* Analyze trends and seasonality.
* Group data using time frequencies.
* Prepare datasets for forecasting.

These techniques are widely used in finance, demand forecasting, operations, IoT analytics, and business intelligence.

---

# 28. What's Next?

In **Day 32**, you'll learn **Advanced Window Functions & Analytical Operations**.

Topics include:

* Rolling Window Functions
* Expanding Window Functions
* Exponentially Weighted Moving (EWM)
* Cumulative Operations
* Ranking
* Percentile Ranking
* Quantiles
* Advanced Statistical Analysis
* Business KPI Calculations
* Financial Analytics

These concepts are heavily used in financial modeling, risk analysis, KPI reporting, customer analytics, and production-grade data pipelines.

---

<div align="center">

# 🎉 Day 31 Complete!

You've mastered **Time Series Analysis**, a cornerstone of modern analytics.

From date indexing and resampling to rolling statistics and seasonality analysis, you now have the tools to explore and prepare chronological data for forecasting and business intelligence.

⭐ **Next → Day 32: Advanced Window Functions & Analytical Operations** 📈🐼

</div>
