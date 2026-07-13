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

