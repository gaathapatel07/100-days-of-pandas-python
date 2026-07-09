# 🐼 Day 31 — Advanced Time Series Analysis in Pandas

<div align="center">

# 100 Days of Pandas

### Day 31 · Mastering Time Series Data

*"Every timestamp tells a story. Time series analysis helps us understand trends, seasonality, and patterns hidden within chronological data."*

![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-orange)
![Topic](https://img.shields.io/badge/Topic-Time%20Series-blue)
![Day](https://img.shields.io/badge/Day-31-orange)

</div>

---

# 📚 Table of Contents

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

