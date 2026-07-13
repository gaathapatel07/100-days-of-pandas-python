# 🐼 Day 40 — Advanced Time Series Analysis with Pandas

<div align="center">

# 100 Days of Pandas

### Day 40 · Analyzing Time-Based Data Like a Professional

*"Time-series analysis transforms chronological data into actionable insights by revealing trends, seasonality, and patterns over time."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Time%20Series-blue)
![Day](https://img.shields.io/badge/Day-40-orange)

</div>

---

# 📚 Table of Contents

1. Introduction
2. What is Time Series Data?
3. Why Time Series Matters
4. Learning Objectives
5. DateTime Conversion
6. DateTime Index
7. Time-Based Filtering
8. Summary

---

# 1. Introduction

Many real-world datasets contain a time component.

Examples include:

* Daily sales
* Stock prices
* Weather measurements
* Website traffic
* Sensor readings
* Hospital admissions
* Electricity consumption
* Customer orders

Analyzing how values change over time helps businesses identify trends and forecast future outcomes.

---

# 2. What is Time Series Data?

Time series data consists of observations recorded at regular or irregular time intervals.

Example:

| Date       | Sales |
| ---------- | ----: |
| 2026-01-01 |  5200 |
| 2026-01-02 |  6100 |
| 2026-01-03 |  5900 |
| 2026-01-04 |  6800 |

Unlike ordinary datasets, the order of observations is important.

---

# 3. Why Time Series Matters

Businesses frequently ask questions such as:

* Are sales increasing?
* Which month performs best?
* Is demand seasonal?
* How does revenue change year-over-year?
* Can future demand be forecast?

Time-series analysis provides answers to these questions.

---

# 4. Learning Objectives

By the end of this lesson, you will be able to:

* Work with DateTime objects.
* Create DateTime indexes.
* Filter data using dates.
* Extract date components.
* Prepare data for forecasting.

---

# 5. DateTime Conversion

Dates imported from CSV or Excel are often stored as strings.

Example:

```python id="datetime01"
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Check the data type.

```python id="datetime02"
df["Order Date"].dtype
```

Output

```text id="datetime03"
datetime64[ns]
```

Handle invalid dates safely.

```python id="datetime04"
df["Order Date"] = pd.to_datetime(
    df["Order Date"],
    errors="coerce"
)
```

Invalid dates become `NaT`.

---

# 6. Creating a DateTime Index

Many Pandas time-series operations require a DateTime index.

```python id="index01"
df = df.set_index(
    "Order Date"
)
```

Example

| Order Date | Sales |
| ---------- | ----: |
| 2026-01-01 |  5200 |
| 2026-01-02 |  6100 |

Check the index.

```python id="index02"
df.index
```

---

## Reset the Index

```python id="index03"
df = df.reset_index()
```

---

# 7. Time-Based Filtering

Once a DateTime index is created, filtering becomes simple.

---

## Filter by Year

```python id="filter01"
sales_2026 = df.loc["2026"]
```

---

## Filter by Month

```python id="filter02"
january = df.loc["2026-01"]
```

---

## Filter by Date Range

```python id="filter03"
sales = df.loc[
    "2026-01-01":"2026-03-31"
]
```

---

## Filter Using Conditions

```python id="filter04"
sales = df[
    df.index.year == 2026
]
```

---

# 8. Extract Date Components

Pandas makes it easy to extract useful features from dates.

---

## Year

```python id="extract01"
df["Year"] = (
    df.index.year
)
```

---

## Month

```python id="extract02"
df["Month"] = (
    df.index.month
)
```

---

## Day

```python id="extract03"
df["Day"] = (
    df.index.day
)
```

---

## Day Name

```python id="extract04"
df["Weekday"] = (
    df.index.day_name()
)
```

Example

| Date       | Weekday  |
| ---------- | -------- |
| 2026-01-01 | Thursday |
| 2026-01-02 | Friday   |

---

## Quarter

```python id="extract05"
df["Quarter"] = (
    df.index.quarter
)
```

Output

| Date       | Quarter |
| ---------- | ------: |
| 2026-02-15 |       1 |
| 2026-08-20 |       3 |

---

# Business Example

A retail company analyzes five years of order data.

The analytics team:

* Converts order dates to DateTime.
* Sets the date as the index.
* Filters yearly and monthly sales.
* Extracts weekdays and quarters.
* Prepares the dataset for forecasting.

This allows the business to study seasonal demand and long-term growth.

---

# Best Practices

✔ Convert date columns immediately after loading data.

✔ Use a DateTime index for time-series analysis.

✔ Extract date features for deeper insights.

✔ Handle invalid dates with `errors="coerce"`.

✔ Keep time zones consistent when combining datasets.

---

# Common Mistakes

### Treating Dates as Strings

Incorrect:

```python
df["Order Date"] > "2026-01-01"
```

Always convert to DateTime before comparison.

---

### Forgetting to Set a DateTime Index

Many time-series operations such as resampling require a DateTime index.

---

### Ignoring Invalid Dates

Always use:

```python
errors="coerce"
```

to safely identify malformed dates.

---

# Key Takeaways

After completing this section, you should understand:

* How to convert strings into DateTime.
* Why DateTime indexes are important.
* How to filter time-series data.
* How to extract useful date components.
* How to prepare datasets for forecasting and trend analysis.

> **"Time-series analysis begins with correctly structured dates. A well-prepared DateTime index enables powerful analysis, efficient filtering, and meaningful business insights."**

# 9. Resampling Time Series

Resampling changes the frequency of time-series data.

Examples:

* Daily → Monthly
* Daily → Weekly
* Hourly → Daily
* Monthly → Quarterly

---

## Monthly Sales

```python id="resample01"
monthly_sales = (
    df["Sales"]
      .resample("M")
      .sum()
)
```

Output

| Month |  Sales |
| ----- | -----: |
| Jan   | 152000 |
| Feb   | 168000 |
| Mar   | 175000 |

---

## Weekly Average

```python id="resample02"
weekly_avg = (
    df["Sales"]
      .resample("W")
      .mean()
)
```

---

## Quarterly Revenue

```python id="resample03"
quarterly = (
    df["Revenue"]
      .resample("Q")
      .sum()
)
```

---

## Common Resampling Frequencies

| Frequency | Code  |
| --------- | ----- |
| Daily     | `D`   |
| Weekly    | `W`   |
| Monthly   | `M`   |
| Quarterly | `Q`   |
| Yearly    | `Y`   |
| Hourly    | `H`   |
| Minutes   | `min` |

---

# 10. Rolling Windows

Rolling windows calculate statistics over a moving window.

---

## 7-Day Moving Average

```python id="rolling01"
df["7-Day Average"] = (
    df["Sales"]
      .rolling(window=7)
      .mean()
)
```

---

## 30-Day Rolling Sum

```python id="rolling02"
df["30-Day Sales"] = (
    df["Sales"]
      .rolling(30)
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

Rolling statistics smooth short-term fluctuations and reveal longer-term trends.

---

# 11. Expanding Windows

Unlike rolling windows, expanding windows include **all previous observations**.

---

## Expanding Mean

```python id="expand01"
df["Average Sales"] = (
    df["Sales"]
      .expanding()
      .mean()
)
```

---

## Expanding Maximum

```python id="expand02"
df["Highest So Far"] = (
    df["Sales"]
      .expanding()
      .max()
)
```

Applications:

* Cumulative performance
* Lifetime averages
* Running statistics

---

# 12. Time Shifting

Shift data forward or backward.

---

## Previous Day Sales

```python id="shift01"
df["Yesterday"] = (
    df["Sales"]
      .shift(1)
)
```

---

## Next Day Sales

```python id="shift02"
df["Tomorrow"] = (
    df["Sales"]
      .shift(-1)
)
```

Example

| Date  | Sales | Yesterday |
| ----- | ----: | --------: |
| Jan 1 |   500 |       NaN |
| Jan 2 |   620 |       500 |
| Jan 3 |   590 |       620 |

---

# 13. Lag Features

Lag features are widely used in forecasting models.

Create a one-day lag.

```python id="lag01"
df["Lag 1"] = (
    df["Sales"]
      .shift(1)
)
```

Three-day lag.

```python id="lag02"
df["Lag 3"] = (
    df["Sales"]
      .shift(3)
)
```

Seven-day lag.

```python id="lag03"
df["Lag 7"] = (
    df["Sales"]
      .shift(7)
)
```

Lag features help machine learning models learn historical patterns.

---

# 14. Moving Averages

Moving averages smooth noisy data.

---

## Simple Moving Average

```python id="ma01"
df["SMA"] = (
    df["Sales"]
      .rolling(7)
      .mean()
)
```

---

## Exponential Moving Average

Recent observations receive greater weight.

```python id="ma02"
df["EMA"] = (
    df["Sales"]
      .ewm(span=7)
      .mean()
)
```

Applications:

* Stock prices
* Sales forecasting
* Demand prediction
* Sensor monitoring

---

# 15. Seasonal Analysis

Extract seasonal behavior.

Monthly sales.

```python id="season01"
monthly = (
    df.groupby(
        df.index.month
    )["Sales"]
      .mean()
)
```

Weekday analysis.

```python id="season02"
weekday = (
    df.groupby(
        df.index.day_name()
    )["Sales"]
      .mean()
)
```

Quarterly performance.

```python id="season03"
quarter = (
    df.groupby(
        df.index.quarter
    )["Revenue"]
      .sum()
)
```

These analyses identify recurring patterns throughout the year.

---

# Business Trend Analysis

Compare monthly revenue.

```python id="trend01"
trend = (
    df.resample("M")
      ["Revenue"]
      .sum()
)
```

Calculate monthly growth.

```python id="trend02"
growth = (
    trend.pct_change()
    * 100
)
```

Output

| Month | Growth % |
| ----- | -------: |
| Feb   |      6.2 |
| Mar   |      3.9 |
| Apr   |     -1.5 |

This helps businesses monitor performance over time.

---

# Business Example

A supermarket chain analyzes daily sales.

Analysts:

* Resample daily sales into monthly totals.
* Calculate 7-day moving averages.
* Create lag features for forecasting.
* Analyze weekday purchasing behavior.
* Measure month-over-month growth.

These insights support inventory planning and staffing decisions.

---

# Best Practices

✔ Convert dates to a DateTime index before resampling.

✔ Use rolling averages to smooth noisy data.

✔ Create lag features for forecasting models.

✔ Compare seasonal performance across months and quarters.

✔ Interpret moving averages alongside raw values.

---

# Common Mistakes

### Resampling Without a DateTime Index

`resample()` requires a DateTime index.

```python
df = df.set_index("Order Date")
```

---

### Using Large Rolling Windows on Small Datasets

A 365-day rolling average is unlikely to be meaningful for a dataset containing only 30 days of observations.

Choose a window size appropriate for the data.

---

### Forgetting That `shift()` Introduces Missing Values

The first row after `shift(1)` becomes `NaN`.

Plan how these missing values will be handled before modeling.

---

# Quick Recap

You have now learned how to:

* Resample time-series data.
* Compute rolling statistics.
* Calculate expanding statistics.
* Shift observations through time.
* Create lag features.
* Apply moving averages.
* Analyze seasonality.
* Measure business trends over time.

> **"Time-series analysis transforms chronological records into meaningful trends, enabling organizations to forecast demand, monitor performance, and make proactive business decisions."**
# 16. Enterprise Time Series Analytics Workflow

Organizations analyze time-based data using a structured workflow.

```text id="workflow01"
Raw Time-Series Data
         │
         ▼
DateTime Conversion
         │
         ▼
DateTime Index
         │
         ▼
Data Cleaning
         │
         ▼
Resampling
         │
         ▼
Rolling Statistics
         │
         ▼
Feature Engineering
         │
         ▼
Trend & Seasonality Analysis
         │
         ▼
Forecasting / Dashboard
```

Following a consistent workflow improves forecasting accuracy and reporting reliability.

---

# 17. Forecasting Preparation Pipeline

Before building forecasting models, prepare the dataset.

```python id="pipeline01"
def prepare_time_series(df):

    df["Order Date"] = pd.to_datetime(
        df["Order Date"]
    )

    df = df.set_index(
        "Order Date"
    )

    df["Lag 1"] = (
        df["Sales"]
          .shift(1)
    )

    df["7-Day MA"] = (
        df["Sales"]
          .rolling(7)
          .mean()
    )

    df["Month"] = (
        df.index.month
    )

    return df
```

Execute:

```python id="pipeline02"
ts_df = prepare_time_series(df)
```

This prepares the dataset for forecasting and advanced analytics.

---

# 18. Time-Series Performance Optimization

Large time-series datasets require efficient processing.

### Sort by Date

```python id="perf01"
df = df.sort_index()
```

Many time-based operations assume chronological order.

---

### Select Required Columns

```python id="perf02"
sales = df[
    ["Sales"]
]
```

Processing fewer columns improves performance.

---

### Downsample Before Visualization

Instead of plotting millions of daily records:

```python id="perf03"
monthly = (
    df["Sales"]
      .resample("M")
      .sum()
)
```

Visualize the monthly totals rather than every individual observation.

---

### Optimize Date Parsing

When importing CSV files:

```python id="perf04"
df = pd.read_csv(
    "sales.csv",
    parse_dates=[
        "Order Date"
    ]
)
```

Parsing dates during import is generally more efficient than converting them later.

---

# 19. Enterprise Case Study

## Scenario

You are a **Senior Business Analyst** at **RetailHub**.

Management wants to understand:

* Sales trends
* Seasonal demand
* Quarterly growth
* Weekly purchasing behavior
* Forecast readiness

Available data:

* Order Date
* Sales
* Revenue
* Profit
* Quantity

---

## Business Questions

### Question 1

Calculate monthly revenue.

```python id="case01"
monthly = (
    df["Revenue"]
      .resample("M")
      .sum()
)
```

---

### Question 2

Compute a 30-day moving average.

```python id="case02"
df["30-Day MA"] = (
    df["Sales"]
      .rolling(30)
      .mean()
)
```

---

### Question 3

Create lag features.

```python id="case03"
df["Lag 7"] = (
    df["Sales"]
      .shift(7)
)
```

---

### Question 4

Analyze weekday sales.

```python id="case04"
weekday_sales = (
    df.groupby(
        df.index.day_name()
    )["Sales"]
      .mean()
)
```

---

### Question 5

Calculate monthly growth.

```python id="case05"
monthly_growth = (
    monthly
      .pct_change()
      * 100
)
```

---

# 20. Business Insights

After analyzing the data, the team discovers:

* Sales steadily increase during the fourth quarter.
* Weekend sales exceed weekday averages.
* A seven-day moving average highlights the long-term sales trend by smoothing daily fluctuations.
* Monthly growth rates reveal periods of rapid expansion and seasonal slowdowns.
* Lag features improve the quality of forecasting models by incorporating historical information.

---

# 21. Practice Exercises

## Beginner

1. Convert a column to DateTime.
2. Set a DateTime index.
3. Filter records for a specific month.
4. Extract the year and month.
5. Calculate monthly sales totals.

---

## Intermediate

6. Resample daily data to weekly totals.
7. Compute a rolling average.
8. Create lag features.
9. Calculate month-over-month growth.
10. Analyze quarterly revenue.

---

## Advanced

11. Build a forecasting preparation pipeline.
12. Create rolling and expanding statistics.
13. Analyze seasonal trends.
14. Optimize time-series processing for a large dataset.
15. Prepare a forecasting-ready dataset.

---

# 22. Interview Questions

## Beginner

1. What is time-series data?
2. Why use a DateTime index?
3. What does `resample()` do?
4. What is a rolling window?
5. Why create lag features?

---

## Intermediate

6. Explain the difference between rolling and expanding windows.
7. What is a moving average?
8. How do you measure month-over-month growth?
9. Why is chronological ordering important?
10. When should you resample data?

---

## Advanced

11. Design a forecasting pipeline for retail sales.
12. Explain seasonality with business examples.
13. Compare simple and exponential moving averages.
14. How would you optimize time-series analysis for millions of records?
15. How do lag features improve predictive models?

---

# 23. Cheat Sheet

| Task           | Syntax                   |
| -------------- | ------------------------ |
| Convert Date   | `pd.to_datetime()`       |
| Set Date Index | `set_index()`            |
| Filter by Date | `loc[]`                  |
| Extract Year   | `index.year`             |
| Extract Month  | `index.month`            |
| Resample       | `resample()`             |
| Rolling Mean   | `rolling().mean()`       |
| Expanding Mean | `expanding().mean()`     |
| Shift          | `shift()`                |
| Moving Average | `rolling(window).mean()` |
| Exponential MA | `ewm().mean()`           |
| Growth Rate    | `pct_change()`           |

---

# 24. Mini Project

## Retail Sales Forecast Preparation

Using any retail, banking, healthcare, logistics, finance, or IoT dataset:

Complete the following tasks:

* Convert date columns to DateTime.
* Set a DateTime index.
* Resample daily data into monthly summaries.
* Calculate rolling averages.
* Generate expanding statistics.
* Create lag features.
* Analyze seasonal patterns.
* Calculate month-over-month growth.
* Build a forecasting preparation pipeline.
* Write **five executive-level business insights**.
* Recommend **three business actions** based on observed trends.

### Example Business Insights

* Revenue peaks during festive months, indicating strong seasonal demand.
* Weekend sales consistently exceed weekday sales.
* Moving averages reveal sustained long-term growth despite short-term fluctuations.
* Monthly growth rates identify both expansion periods and temporary declines.
* Historical sales patterns provide valuable features for forecasting future demand.

---

# 25. Summary

Congratulations! 🎉

Today you mastered **Advanced Time Series Analysis with Pandas**.

You learned how to:

* Convert and index DateTime data.
* Filter time-based records.
* Resample datasets.
* Compute rolling and expanding statistics.
* Create lag features.
* Apply moving averages.
* Analyze seasonality.
* Prepare datasets for forecasting.

These skills are essential for forecasting, financial analysis, demand planning, operations, and business intelligence.

---

# 26. What's Next?

In **Day 41**, you'll learn **Advanced File Handling & Data Integration in Pandas**.

Topics include:

* Reading and writing CSV, Excel, JSON, and Parquet files
* Working with SQL databases
* Reading data from APIs
* Combining multiple files
* Batch file processing
* Compression formats
* Efficient data import/export
* Enterprise ETL workflows
* Data integration best practices

These techniques are essential for building real-world data pipelines that collect and integrate information from multiple sources.

---

<div align="center">

# 🎉 Day 40 Complete!

You've mastered **Advanced Time Series Analysis**, giving you the skills to analyze trends, seasonality, growth, and forecasting-ready data.

By combining time-based filtering, rolling statistics, lag features, and resampling, you're now equipped to work with real-world business time-series datasets.

⭐ **Next → Day 41: Advanced File Handling & Data Integration** 📁🐼

</div>
