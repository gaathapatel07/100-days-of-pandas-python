# 🐼 Day 48 — Time Series Analysis with Pandas

## 📖 Introduction

Time Series Analysis is the process of analyzing data collected over time to identify trends, seasonal patterns, cycles, and future behavior.

Unlike ordinary datasets, time series data has a **time component**, meaning the order of observations matters.

Examples include:

- Daily stock prices
- Monthly sales
- Website traffic
- Weather reports
- Hospital admissions
- Electricity consumption

Pandas provides powerful tools to manipulate and analyze time-based data efficiently.

---

# 📚 Topics Covered

- Introduction to Time Series
- Datetime Data Type
- Converting Strings to Dates
- Extracting Date Components
- Creating Date Ranges
- Time Indexing
- Filtering by Date
- Resampling
- Rolling Windows
- Expanding Windows

---

# 1. What is Time Series Data?

Time series data consists of observations recorded at regular intervals.

Example:

| Date | Sales |
|------|-------|
|2025-01-01|1200|
|2025-01-02|1350|
|2025-01-03|1420|
|2025-01-04|1500|

Unlike categorical data, time series maintains chronological order.

---

# 2. Why Time Series Analysis Matters

Businesses use time series analysis to:

- Forecast future sales
- Detect trends
- Identify seasonality
- Monitor KPIs
- Predict demand
- Detect anomalies

Industries using time series:

- Finance
- Retail
- Healthcare
- Manufacturing
- Banking
- Telecommunications

---

# 3. Learning Objectives

By the end of this lesson, you will be able to:

- Work with datetime objects.
- Convert strings into dates.
- Extract useful date information.
- Create date ranges.
- Index data by time.
- Perform resampling.
- Calculate rolling statistics.

---

# 4. Datetime Data Type

Convert a string column into datetime.

```python
df["Order Date"] = pd.to_datetime(
    df["Order Date"]
)
```

Check data type.

```python
df["Order Date"].dtype
```

Output:

```
datetime64[ns]
```

Datetime enables powerful time-based operations.

---

# 5. Extracting Date Components

Extract year.

```python
df["Year"] = df["Order Date"].dt.year
```

Extract month.

```python
df["Month"] = df["Order Date"].dt.month
```

Extract month name.

```python
df["Month Name"] = df["Order Date"].dt.month_name()
```

Extract day.

```python
df["Day"] = df["Order Date"].dt.day
```

Extract weekday.

```python
df["Weekday"] = df["Order Date"].dt.day_name()
```

Extract quarter.

```python
df["Quarter"] = df["Order Date"].dt.quarter
```

---

# 6. Creating Date Ranges

Generate a sequence of dates.

```python
dates = pd.date_range(

    start="2025-01-01",

    periods=10,

    freq="D"

)

print(dates)
```

Monthly range.

```python
pd.date_range(

    "2025-01-01",

    periods=12,

    freq="M"

)
```

Hourly range.

```python
pd.date_range(

    "2025-01-01",

    periods=24,

    freq="H"

)
```

---

# 7. Setting a Date Index

Use dates as the DataFrame index.

```python
df = df.set_index("Order Date")
```

View the index.

```python
df.index
```

Benefits:

- Faster filtering
- Time slicing
- Resampling
- Rolling calculations

---

# 8. Filtering by Date

Retrieve data for a specific year.

```python
df["2025"]
```

Filter a specific month.

```python
df["2025-03"]
```

Filter a date range.

```python
df.loc["2025-01-01":"2025-03-31"]
```

---

# 9. Resampling

Resampling changes the frequency of time-series data.

Monthly sales.

```python
monthly_sales = df["Sales"].resample("M").sum()
```

Weekly sales.

```python
weekly_sales = df["Sales"].resample("W").sum()
```

Quarterly sales.

```python
quarterly_sales = df["Sales"].resample("Q").sum()
```

Annual sales.

```python
yearly_sales = df["Sales"].resample("Y").sum()
```

---

# 10. Rolling Windows

Rolling calculations analyze moving periods.

7-day moving average.

```python
df["Sales"].rolling(7).mean()
```

30-day moving average.

```python
df["Sales"].rolling(30).mean()
```

Rolling sum.

```python
df["Sales"].rolling(7).sum()
```

Rolling maximum.

```python
df["Sales"].rolling(7).max()
```

Rolling windows smooth short-term fluctuations.

---

# 11. Expanding Windows

Expanding windows calculate cumulative statistics.

Cumulative average.

```python
df["Sales"].expanding().mean()
```

Cumulative sum.

```python
df["Sales"].expanding().sum()
```

Cumulative maximum.

```python
df["Sales"].expanding().max()
```

Useful for tracking long-term business performance.

---

# Business Example

An e-commerce company analyzes two years of sales data.

Using time-series analysis, analysts discover:

- Sales increase during festive seasons.
- Mondays have the lowest average revenue.
- Quarterly growth is steadily increasing.
- A 30-day moving average removes daily fluctuations.
- Rolling statistics reveal long-term growth trends.

These insights help management improve demand forecasting and inventory planning.

---

# Best Practices

✔ Always convert date columns to `datetime`.

✔ Use a datetime index for time-series operations.

✔ Choose the correct resampling frequency.

✔ Use rolling averages to identify trends.

✔ Consider seasonality when analyzing data.

---

# Common Mistakes

### Treating Dates as Strings

String dates cannot use Pandas' powerful datetime functions.

---

### Ignoring Missing Dates

Missing dates can distort trend analysis and forecasts.

---

### Choosing Incorrect Resampling Frequency

Daily, weekly, monthly, or yearly aggregation should match the business problem.

---

# Key Takeaways

After completing this section, you should understand:

- Datetime conversion
- Date component extraction
- Date ranges
- Time indexing
- Date filtering
- Resampling
- Rolling windows
- Expanding windows

> **"Time-series analysis reveals how data evolves over time, enabling organizations to identify trends, forecast demand, and make proactive business decisions."**

---

## Next (Day 48 – Part 2)

The next section covers:

- Shift & Lag Operations
- Difference Calculations
- Percentage Change
- Time-Based Grouping
- Rolling Correlation
- Time-Series Visualization
- Business Trend Analysis
- Forecasting Preparation
